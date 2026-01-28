# Realm Swift XCFramework Distribution

Community-built XCFrameworks for Realm Swift with newer Xcode versions.

## Why This Repository?

The official [realm/realm-swift](https://github.com/realm/realm-swift) repository stopped distributing pre-built binaries for newer Xcode versions. This repository provides XCFrameworks built from official Realm Swift sources.

### Why not fork?

- No need to maintain Realm source code
- Only maintain build scripts
- Minimal patches applied

## Usage

### 1. Trigger a Build

1. Go to [Actions](../../actions) tab
2. Click **"Build and Release XCFramework"**
3. Click **"Run workflow"**
4. Enter:
   - **tag**: Realm Swift tag (e.g., `v20.0.3`)
   - **xcode-version**: Xcode version (e.g., `16.2`)
5. Wait ~20-30 minutes

### 2. Download

Built XCFrameworks are available on the [Releases](../../releases) page:
- `Realm.xcframework.zip`
- `RealmSwift.xcframework.zip`

## Installation

### Manual

1. Download zip files from [Releases](../../releases)
2. Unzip and drag `.xcframework` files to your Xcode project
3. Add to "Frameworks, Libraries, and Embedded Content"

### Swift Package Manager

Add to `Package.swift`:

```swift
.binaryTarget(
    name: "Realm",
    url: "https://github.com/ainame/realm-swift-xcframework-distribution/releases/download/v20.0.3/Realm.xcframework.zip",
    checksum: "..." // Use: swift package compute-checksum Realm.xcframework.zip
),
.binaryTarget(
    name: "RealmSwift",
    url: "https://github.com/ainame/realm-swift-xcframework-distribution/releases/download/v20.0.3/RealmSwift.xcframework.zip",
    checksum: "..." // Use: swift package compute-checksum RealmSwift.xcframework.zip
),
```

## Build Configuration

- **Platform**: iOS only (device + simulator)
- **Xcode**: Default 16.2 (configurable)
- **Patches**: Code signing removed (no certificate required)

## Maintenance

### New Realm Version

Run workflow with new tag (e.g., `v20.0.4`)

### New Xcode Version

Update `.github/workflows/build-release.yml`:
```yaml
xcode-version:
  default: '16.3'  # Update this
```

## Technical Details

1. Clones `realm/realm-swift` at specified tag
2. Applies patches from `patches/` directory
3. Builds using official `build.sh ios-swift`
4. Creates and uploads XCFrameworks to GitHub Release

See [`patches/README.md`](./patches/README.md) for patch details.

## License

- This repository: MIT License
- Realm Swift: [Apache License 2.0](https://github.com/realm/realm-swift/blob/master/LICENSE)
