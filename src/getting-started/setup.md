# Installation - Latest Release (SwiftWasm 5.10)

To install Swift for WebAssembly toolchain, download one of the packages below and follow the instructions for your operating system.

Tag: [swift-wasm-5.10.0-RELEASE](https://github.com/swiftwasm/swift/releases/tag/swift-wasm-5.10.0-RELEASE)

| Download | Docker Tag |
|:------------------:|:----------:|
| [macOS arm64](https://github.com/swiftwasm/swift/releases/download/swift-wasm-5.10.0-RELEASE/swift-wasm-5.10.0-RELEASE-macos_arm64.pkg) | Unavailable |
| [macOS x86](https://github.com/swiftwasm/swift/releases/download/swift-wasm-5.10.0-RELEASE/swift-wasm-5.10.0-RELEASE-macos_x86_64.pkg) | Unavailable |
| [Ubuntu 18.04 x86_64](https://github.com/swiftwasm/swift/releases/download/swift-wasm-5.10.0-RELEASE/swift-wasm-5.10.0-RELEASE-ubuntu18.04_x86_64.tar.gz) | [5.10-bionic, bionic](https://github.com/orgs/swiftwasm/packages/container/package/swift) |
| [Ubuntu 20.04 x86_64](https://github.com/swiftwasm/swift/releases/download/swift-wasm-5.10.0-RELEASE/swift-wasm-5.10.0-RELEASE-ubuntu20.04_x86_64.tar.gz) | [5.10-focal, focal](https://github.com/orgs/swiftwasm/packages/container/package/swift) |
| [Ubuntu 20.04 aarch64](https://github.com/swiftwasm/swift/releases/download/swift-wasm-5.10.0-RELEASE/swift-wasm-5.10.0-RELEASE-ubuntu20.04_aarch64.tar.gz) | [5.10-focal, focal](https://github.com/orgs/swiftwasm/packages/container/package/swift) |
| [Ubuntu 22.04 x86_64](https://github.com/swiftwasm/swift/releases/download/swift-wasm-5.10.0-RELEASE/swift-wasm-5.10.0-RELEASE-ubuntu22.04_x86_64.tar.gz) | [5.10, 5.10-jammy, jammy, latest](https://github.com/orgs/swiftwasm/packages/container/package/swift) |


You can find older releases from the [GitHub Releases page](https://github.com/swiftwasm/swift/releases?q=prerelease%3Afalse)

## Toolchain Installation

### macOS

1. [Download the latest package release](#latest-release) according to your CPU architecture (arm64 for [Apple Silicon Macs](https://support.apple.com/en-us/HT211814), x86 for Intel Macs).
2. Run the package installer, which will install an Xcode toolchain into `/Library/Developer/Toolchains/`.
3. To use the Swift toolchain with command-line tools, use `env TOOLCHAINS=swiftwasm swift` or add the Swift toolchain to your path as follows:

```bash
export PATH=/Library/Developer/Toolchains/<toolchain name>.xctoolchain/usr/bin:"${PATH}"
```

4. Run `swift --version`. If you installed the toolchain successfully, you can get the following message.

```bash
$ swift --version
# Or TOOLCHAINS=swiftwasm swift --version
SwiftWasm Swift version 5.10-dev
Target: x86_64-apple-darwin21.6.0
```

If you want to uninstall the toolchain, you can remove the toolchain directory from `/Library/Developer/Toolchains/` and make sure to remove the toolchain from your `PATH`.

## Linux

1. [Download the latest package release](#latest-release) according to your Ubuntu version and CPU architecture.
2. Follow the official Swift installation guide for Linux from [swift.org](https://www.swift.org/install/linux/#installation-via-tarball) while skipping GPG key verification, which is not provided for SwiftWasm releases.

## Experimental: Swift SDK

SwiftWasm provides [Swift SDK](https://github.com/apple/swift-evolution/blob/main/proposals/0387-cross-compilation-destinations.md)s for WebAssembly. You can use the Swift SDK to cross-compile Swift packages for WebAssembly without installing the whole toolchain.

To use the Swift SDK, you need to install the official Swift toolchain 5.10 or later. Then, you can install the Swift SDK using the following command while replacing `<your platform>`:

```bash
$ swift experimental-sdk install https://github.com/swiftwasm/swift/releases/download/swift-wasm-5.10.0-RELEASE/swift-wasm-5.10.0-RELEASE-<your platform>.artifactbundle.zip
```

You can find the latest Swift SDK release from [the GitHub Releases page](https://github.com/swiftwasm/swift/releases/tag/swift-wasm-5.10.0-RELEASE).

After installing the Swift SDK, you can see the installed SDKs using the following command:

```bash
$ swift experimental-sdk list
<SDK name>
...
```

You can use the installed SDKs to cross-compile Swift packages for WebAssembly using the following command:

```bash
$ swift build --experimental-swift-sdk <SDK name>
```

## Docker

SwiftWasm offical Docker images are hosted on [GitHub Container Registry](https://github.com/orgs/swiftwasm/packages/container/package/swift).

SwiftWasm Dockerfiles are located on [swiftwasm-docker](https://github.com/swiftwasm/swiftwasm-docker) repository.

### Supported Platforms

- Ubuntu 18.04 (x86_64)
- Ubuntu 20.04 (x86_64, aarch64)
- Ubuntu 22.04 (x86_64)

### Using Docker Images

1. Pull the Docker image from [GitHub Container Registry](https://github.com/orgs/swiftwasm/packages/container/package/swift):

```bash
docker pull ghcr.io/swiftwasm/swift:latest
```

2. Create a container using tag `latest` and attach it to the container:

```bash
docker run --rm -it ghcr.io/swiftwasm/swift:latest /bin/bash
```
