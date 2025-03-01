# Installation - Latest Release (Swift 6.0.3)

SwiftWasm provides [Swift SDK](https://github.com/apple/swift-evolution/blob/main/proposals/0387-cross-compilation-destinations.md)s for WebAssembly.

Before installing the Swift SDK, you need to ensure the following:

- You need to [install an Open Source toolchain from swift.org](https://www.swift.org/install/). (Not the Xcode toolchain)
- You cannot use toolchains bundled with Xcode to use the Swift SDK.
- If you are using macOS, please follow the [official guide](https://www.swift.org/install/macos/package_installer/) to install the toolchain.

Please ensure you have installed the Swift 6.0.3 Open Source toolchain.

```sh
swift --version
```

| Toolchain | Output |
|-----------|--------|
| ❌ Xcode | `Apple Swift version 6.0.3 (swiftlang-6.0.3.1.10 clang-1600.0.30.1)` |
| ✅ Open Source (macOS) | `Apple Swift version 6.0.3 (swift-6.0.3-RELEASE)` |
| ✅ Open Source (Others) | `Swift version 6.0.3 (swift-6.0.3-RELEASE)` |

Once you have installed the Open Source toolchain, you can install the Swift SDK for WebAssembly:

```bash
swift sdk install "https://github.com/swiftwasm/swift/releases/download/swift-wasm-6.0.3-RELEASE/swift-wasm-6.0.3-RELEASE-wasm32-unknown-wasi.artifactbundle.zip" --checksum "31d3585b06dd92de390bacc18527801480163188cd7473f492956b5e213a8618"
```

After installing the Swift SDK, you can see `6.0.3-RELEASE-wasm32-unknown-wasi` in the Swift SDK list:

```bash
swift sdk list
```

You can also find other SDKs from [the GitHub Releases page](https://github.com/swiftwasm/swift/releases).

## Hello, World

First, create a new directory for your project and navigate into it:

```bash
$ mkdir hello && cd hello
```

Create a new Swift package:

```bash
$ swift package init --type executable
```

You can use the installed SDKs to cross-compile Swift packages for WebAssembly:

```bash
$ swift build --swift-sdk wasm32-unknown-wasi
...
$ file .build/wasm32-unknown-wasi/debug/hello.wasm
.build/wasm32-unknown-wasi/debug/hello.wasm: WebAssembly (wasm) binary module version 0x1 (MVP)
```

You can run the built WebAssembly module using [`wasmtime`](https://wasmtime.dev/):

```bash
$ wasmtime .build/wasm32-unknown-wasi/debug/hello.wasm
Hello, world!
```

## FAQ

### How to check if I am using Open Source toolchain or Xcode toolchain?

```bash
$ swift --version | head -n1
```

| Toolchain | Output |
|-----------|--------|
| Xcode | `Apple Swift version 6.0.3 (swiftlang-6.0.3.1.10 clang-1600.0.30.1)` |
| Open Source (macOS) | `Apple Swift version 6.0.3 (swift-6.0.3-RELEASE)` |
| Open Source (Others) | `Swift version 6.0.3 (swift-6.0.3-RELEASE)` |


### `<unknown>:0: error: module compiled with Swift 6.0.3 cannot be imported by the Swift x.y.z`

This error occurs when the Swift toolchain version you are using is different from the version of the Swift SDK you have installed.

To resolve this issue, you can either:

1. Switch the Swift toolchain to the same version as the Swift SDK you have installed. Check the [official guide](https://www.swift.org/install/macos/package_installer/) for how to switch the toolchain.
2. Install the Swift SDK for the same version as the Swift toolchain you are using.
  Use the following shell snippet to query compatible Swift SDK for your current toolchain version:

```console
(
  V="$(swiftc --version | head -n1)"; \
  TAG="$(curl -sL "https://raw.githubusercontent.com/swiftwasm/swift-sdk-index/refs/heads/main/v1/tag-by-version.json" | jq -r --arg v "$V" '.[$v] | .[-1]')"; \
  curl -sL "https://raw.githubusercontent.com/swiftwasm/swift-sdk-index/refs/heads/main/v1/builds/$TAG.json" | \
  jq -r '.["swift-sdks"]["wasm32-unknown-wasi"] | "swift sdk install \"\(.url)\" --checksum \"\(.checksum)\""'
)
```


### What is the difference between the Swift Toolchain and the Swift SDK?

The Swift toolchain is a complete package that includes the Swift compiler, standard library, and other tools.

The Swift SDK includes a subset of the Swift toolchain that includes only the necessary components for cross-compilation and some supplementary resources.

### What is included in the Swift SDK for WebAssembly?

The Swift SDK for WebAssembly includes only the pre-built Swift standard libraries for WebAssembly. It does not include the Swift compiler or other tools that are part of the Swift toolchain.
