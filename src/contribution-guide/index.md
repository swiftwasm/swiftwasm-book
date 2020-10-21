# Contribution Guide

## Forum posts

- [Design of this project](https://forums.swift.org/t/wasm-support/16087/14)
- [Swift Package Manager Support](https://forums.swift.org/t/webassembly-swiftpm/34343)

## Repositories

### [swiftwasm/swift](https://github.com/swiftwasm/swift)

The main repository of this project. Forked from [apple/swift](https://github.com/swiftwasm/swift). We are tracking upstream changes using [pull](https://github.com/wei/pull)

#### Branching scheme

- `swiftwasm` is the main development branch.
- `main` is a mirror of the `main` branch of the upstream `apple/swift` repository. This branch is necessary to avoid [some issues](https://github.com/swiftwasm/swift/pull/36).
- `swiftwasm-release/5.3` is the branch where 5.3 release of SwiftWasm is prepared.
- `release/5.3` is a mirror of the upstream `release/5.3` branch.

### [swiftwasm/llvm-project](https://github.com/swiftwasm/llvm-project)

This repository is a fork of [apple/llvm-project](https://github.com/apple/llvm-project).

`swiftwasm` branch is based on `swift/main` branch of `apple/llvm-project`.

Please see the [AppleBranchingScheme.md](https://github.com/apple/llvm-project/blob/apple/main/apple-docs/AppleBranchingScheme.md)
document in the upstream repository for more details.


### [swiftwasm/icu4c-wasi](https://github.com/swiftwasm/icu4c-wasi)

Build script and patches for building ICU project for WebAssembly. [The required changes to build 
it](https://github.com/unicode-org/icu/pull/990) were merged to the upstream repository.

### [swiftwasm/wasi-sdk](https://github.com/swiftwasm/wasi-sdk) and [swiftwasm/wasi-libc](https://github.com/swiftwasm/wasi-libc)

Forked from [WebAssembly/wasi-sdk](https://github.com/WebAssembly/wasi-sdk) and [WebAssembly/wasi-libc](https://github.com/WebAssembly/wasi-libc).

We fork them to build `wasi-sysroot` with pthread header. There aren't so many diff from upstream.
