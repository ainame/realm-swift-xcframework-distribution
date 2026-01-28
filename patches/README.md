# Patches

This directory contains patch files applied to Realm Swift during the build process.

## Current Patches

### `disable-codesigning.patch`

**Purpose**: Remove code signing requirement

**What it does**:
- Removes `codesign` command from `scripts/create-release-package.rb`
- Removes `SIGNING_IDENTITY` environment variable usage

**Why**:
- No certificate management needed
- Anyone can build without Apple Developer account
- Major projects (Stripe, Realm official) ship unsigned binaries

**Note**: This patch modifies a Ruby script, but our build workflow doesn't run Ruby. The script is only used by Realm's release process which we don't use. We patch it to prevent errors if the script is accidentally invoked.

## How Patches Are Applied

The GitHub Actions workflow automatically applies patches:

```bash
git apply --verbose patches/*.patch
```

No Ruby installation required - `git apply` is a Git built-in command.

## Creating New Patches

If you need to modify Realm Swift source:

1. Clone and edit:
   ```bash
   git clone --branch v20.0.3 https://github.com/realm/realm-swift.git
   cd realm-swift
   # Make your changes
   ```

2. Generate patch:
   ```bash
   git diff > ../patches/my-new-patch.patch
   ```

3. Document it in this README

## Testing Patches

Test patch locally before committing:

```bash
cd realm-swift
git apply --check ../patches/disable-codesigning.patch
```

If it fails, the patch may need updating for newer Realm versions.

---

**Last updated**: 2026-01-28
