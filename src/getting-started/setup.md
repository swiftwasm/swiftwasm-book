# Installation

To install Swift for WebAssembly toolchain, download one of the packages below and follow the instructions for your operating system.

## Releases

### SwiftWasm 5.8

Tag: [swift-wasm-5.8.0-RELEASE](https://github.com/swiftwasm/swift/releases/tag/swift-wasm-5.8.0-RELEASE)

| Download | Docker Tag |
|:------------------:|:----------:|
| [macOS arm64](https://github.com/swiftwasm/swift/releases/download/swift-wasm-5.8.0-RELEASE/swift-wasm-5.8.0-RELEASE-macos_arm64.pkg) | Unavailable |
| [macOS x86](https://github.com/swiftwasm/swift/releases/download/swift-wasm-5.8.0-RELEASE/swift-wasm-5.8.0-RELEASE-macos_x86_64.pkg) | Unavailable |
| [Ubuntu 18.04 x86_64](https://github.com/swiftwasm/swift/releases/download/swift-wasm-5.8.0-RELEASE/swift-wasm-5.8.0-RELEASE-ubuntu18.04_x86_64.tar.gz) | [5.8-bionic, bionic](https://github.com/orgs/swiftwasm/packages/container/package/swift) |
| [Ubuntu 20.04 x86_64](https://github.com/swiftwasm/swift/releases/download/swift-wasm-5.8.0-RELEASE/swift-wasm-5.8.0-RELEASE-ubuntu20.04_x86_64.tar.gz) | [5.8-focal, focal](https://github.com/orgs/swiftwasm/packages/container/package/swift) |
| [Ubuntu 20.04 aarch64](https://github.com/swiftwasm/swift/releases/download/swift-wasm-5.8.0-RELEASE/swift-wasm-5.8.0-RELEASE-ubuntu20.04_aarch64.tar.gz) | [5.8-focal, focal](https://github.com/orgs/swiftwasm/packages/container/package/swift) |
| [Ubuntu 22.04 x86_64](https://github.com/swiftwasm/swift/releases/download/swift-wasm-5.8.0-RELEASE/swift-wasm-5.8.0-RELEASE-ubuntu22.04_x86_64.tar.gz) | [5.8, 5.8-jammy, jammy, latest](https://github.com/orgs/swiftwasm/packages/container/package/swift) |


You can download the latest development snapshot from [the Releases page](https://github.com/swiftwasm/swift/releases)


# Using Downloads

## macOS

An Xcode toolchain (`.xctoolchain`) includes a copy of the compiler, linker, and other related tools needed to provide a cohesive development experience for working in a specific version of Swift.


### Requirements

- macOS 10.15 or later


### Installation

1. [Download the latest package release](https://book.swiftwasm.org/getting-started/setup.html#swiftwasm-57) according to your CPU architecture (arm64 for [Apple Silicon Macs](https://support.apple.com/en-us/HT211814), x86 for Intel Macs).
2. Run the package installer, which will install an Xcode toolchain into `/Library/Developer/Toolchains/`.
3. To use the Swift toolchain with command-line tools, use `xcrun --toolchain swiftwasm` or add the Swift toolchain to your path as follows:

```bash
export PATH=/Library/Developer/Toolchains/swift-latest.xctoolchain/usr/bin:"${PATH}"
```

4. Run `swift --version`. If you installed the toolchain successfully, you can get the following message.

```bash
$ swift --version
SwiftWasm Swift version 5.8.0 (swiftlang-5.8.0)
Target: x86_64-apple-darwin21.6.0
```

> Warning: `xcrun` finds executable binary based on `--toolchain` option or `TOOLCHAINS` environment variable, but it also sets `SDKROOT` as host target SDK (e.g. `MacOSX.sdk`). So you need to specify `-sdk` option as `/Library/Developer/Toolchains/swift-wasm-5.8.0-RELEASE.xctoolchain/usr/share/wasi-sysroot` when launching `swiftc` from xcrun. `swift build` or other SwiftPM commands automatically find SDK path based on target triple, so they don't require to specify it.


## Linux

Packages for Linux are tar archives including a copy of the Swift compiler, linker, and related tools. You can install them anywhere as long as the extracted tools are in your PATH.

### Requirements

- Ubuntu 18.04 or 20.04 (64-bit)

### Installation

1. Install required dependencies:


```bash
# Ubuntu 18.04
apt-get install \
          binutils \
          git \
          libc6-dev \
          libcurl4 \
          libedit2 \
          libgcc-5-dev \
          libpython2.7 \
          libsqlite3-0 \
          libstdc++-5-dev \
          libxml2 \
          pkg-config \
          tzdata \
          zlib1g-dev
# Ubuntu 20.04
apt-get install \
          binutils \
          git \
          gnupg2 \
          libc6-dev \
          libcurl4 \
          libedit2 \
          libgcc-9-dev \
          libpython2.7 \
          libsqlite3-0 \
          libstdc++-9-dev \
          libxml2 \
          libz3-dev \
          pkg-config \
          tzdata \
          zlib1g-dev
```

2. Download the latest binary release above.

The `swift-wasm-<VERSION>-<PLATFORM>.tar.gz` file is the toolchain itself.

3. Extract the archive with the following command:

```bash
tar xzf swift-wasm-<VERSION>-<PLATFORM>.tar.gz
```
This creates a usr/ directory in the location of the archive.


4. Add the Swift toolchain to your path as follows:

```
export PATH=$(pwd)/usr/bin:"${PATH}"
```

You can now execute the swiftc command to build Swift projects.


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
