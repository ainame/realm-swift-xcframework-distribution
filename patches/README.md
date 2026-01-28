# Patches

このディレクトリには、Realm Swift をビルドする際に適用するパッチファイルが含まれています。

## パッチ適用の方針

- **最小限**: 必要最小限のパッチのみ適用
- **透明性**: すべてのパッチは Git パッチ形式で管理
- **ドキュメント化**: 各パッチの目的を明記

## パッチ一覧

### `disable-codesigning.patch`

**目的**: コード署名を無効化

**変更内容**:
- `scripts/create-release-package.rb` から `codesign` コマンド呼び出しを削除
- `SIGNING_IDENTITY` 環境変数の使用を削除

**理由**:
1. **証明書管理の簡素化**
   - Developer ID 証明書不要
   - GitHub Secrets の設定不要
   - 誰でもビルド可能

2. **業界の実態**
   - Stripe、Realm 公式も署名なしで配布（2025年10月以降）
   - 29%の主要 OSS プロジェクトが署名なし

3. **プライバシー要件は満たす**
   - プライバシーマニフェストは含まれる
   - Apple の主要な要件はクリア

**適用対象**:
- Realm Swift v20.0.3 以前のバージョン（署名あり）
- 署名が削除されたバージョンには不要

**元のコード** (v20.0.3):
```ruby
def create_xcframework(root, xcode_version, configuration, name)
  signing_identity = ENV['SIGNING_IDENTITY']
  # ... xcframework 作成 ...
  sh 'codesign', '--timestamp', '-s', signing_identity, output
end
```

**パッチ後**:
```ruby
def create_xcframework(root, xcode_version, configuration, name)
  # ... xcframework 作成 ...
  # コード署名はスキップ
end
```

**参考資料**:
- [コード署名調査レポート](https://github.com/ainame/realm-swift/blob/master/CODESIGNING_REPORT.md)

## パッチの更新方法

### 新しいパッチを作成する場合

1. Realm Swift のソースコードを編集
2. Git diff を取得:
   ```bash
   cd realm-swift
   git diff > ../patches/new-patch.patch
   ```
3. パッチファイルを確認・編集
4. このREADME.mdに説明を追加

### パッチが適用できない場合

Realm Swift の新しいバージョンで構造が変わった場合:

1. 最新のソースコードを確認
2. パッチファイルを手動で更新
3. テストビルドで検証
4. コミット

## トラブルシューティング

### パッチ適用エラー

```bash
error: patch failed: scripts/create-release-package.rb:31
error: scripts/create-release-package.rb: patch does not apply
```

**対処法**:
1. Realm Swift のバージョンを確認
2. 該当ファイルの内容を確認
3. パッチを手動で更新

### パッチ適用のテスト

ローカルでテスト:

```bash
# Realm Swift をクローン
git clone --branch v20.0.3 https://github.com/realm/realm-swift.git
cd realm-swift

# パッチを適用
git apply --check ../patches/disable-codesigning.patch
git apply ../patches/disable-codesigning.patch

# 確認
git diff
```

---

**最終更新**: 2026-01-28
