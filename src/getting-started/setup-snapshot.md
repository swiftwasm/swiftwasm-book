# Installation - Development Snapshot

## Swift SDK for WebAssembly - Cross compile to WebAssembly

The upstream Open Source Swift toolchain does support WebAssembly as a target platform since **Swift 6.0**.
For those versions, you just need to install [Swift SDK for cross-compilation](https://github.com/apple/swift-evolution/blob/main/proposals/0387-cross-compilation-destinations.md).


## Instructions

1. **Find a Development Snapshot**

   Visit the [SwiftWasm GitHub Releases page](https://github.com/swiftwasm/swift/releases).

2. **Match the Swift Toolchain Version**

   Refer to the release note's table to find the corresponding upstream Swift toolchain version.
   > For example, [`swift-wasm-DEVELOPMENT-SNAPSHOT-2024-06-12-a`](https://github.com/swiftwasm/swift/releases/tag/swift-wasm-DEVELOPMENT-SNAPSHOT-2024-06-12-a) corresponds to `swift-DEVELOPMENT-SNAPSHOT-2024-06-11-a`

3. **Download the Swift Toolchain**

   Download the appropriate Swift toolchain version from [swift.org/install](https://www.swift.org/install).

4. **Get the Swift SDK artifactbundle URL**

   Find the URL ending with `.artifactbundle.zip` from the release page you found in step 1.
   Currently we provide Swift SDKs for the following WebAssembly targets:
   - `wasm32-unknown-wasi` (Recommended): for [WASI Preview 1](https://github.com/WebAssembly/WASI/blob/main/legacy/preview1/docs.md)
   - `wasm32-unknown-wasip1-threads`: for WASI Preview 1 with [wasi-threads](https://github.com/WebAssembly/wasi-threads) extension
   > Choose `wasm32-unknown-wasi` if you are not sure which target to use.

5. **Install the Swift SDK for WebAssembly**

   Use the following command to install the Swift SDK:
   ```console
   $ swift sdk install <URL-or-filename-of-swift-sdk-artifactbundle>
   ```
   > For example, if you selected `swift-wasm-DEVELOPMENT-SNAPSHOT-2024-06-12-a`, you need to run the following command:
   > ```console
   > $ swift sdk install https://github.com/swiftwasm/swift/releases/download/swift-wasm-DEVELOPMENT-SNAPSHOT-2024-06-12-a/swift-wasm-DEVELOPMENT-SNAPSHOT-2024-06-12-a-wasm32-unknown-wasi.artifactbundle.zip
   > ```
6. You can see the installed SDKs using the following command:
   ```console
   $ swift sdk list
   <Swift SDK id>
   ...
   ```
   > Once you don't need the SDK, you can remove it using the following command:
   > ```console
   >  $ swift sdk remove <Swift SDK id>
   > ```

## How to use the Swift SDK

First, create a new Swift package using the following command:

```console
$ mkdir Example
$ cd Example
$ swift package init --type executable
```

Then, you can build the package using the Swift SDK:

```console
$ swift build --swift-sdk wasm32-unknown-wasi
# or
$ swift build --swift-sdk <Swift SDK id>
```

## FAQ

### What is the difference between the Swift Toolchain and the Swift SDK?

The Swift toolchain is a complete package that includes the Swift compiler, standard library, and other tools.

The Swift SDK includes a subset of the Swift toolchain that includes only the necessary components for cross-compilation and some supplementary resources.

### What is included in the Swift SDK for WebAssembly?

The Swift SDK for WebAssembly includes only the pre-built Swift standard libraries for WebAssembly. It does not include the Swift compiler or other tools that are part of the Swift toolchain.
