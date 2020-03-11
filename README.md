# Introduction

Welcome to the SwiftWasm Documentation!

SwiftWasm is an open source project to support WebAssembly target for Swift.

The goal of this project is fully support of WebAssembly target for Swift and to be merged into upstream repository https://github.com/apple/swift.

## Forum posts

- [Design of this project](https://forums.swift.org/t/wasm-support/16087/14)
- [Swift Package Manager Support](https://forums.swift.org/t/webassembly-swiftpm/34343)

## [How to develop](./DevelopmentGuide.md)


## Repositories


### [swiftwasm/swift](https://github.com/swiftwasm/swift)

The main repository of this project. Forked from [apple/swift](https://github.com/swiftwasm/swift). We are tracking upstream changes using [pull](https://github.com/wei/pull)

#### Branching scheme

- `swiftwasm` is the main develop branch
- `master` is mirror of `master` branch of `apple/master`. This branch is necessary to avoid [some issues](https://github.com/swiftwasm/swift/pull/36)

### [swiftwas/llvm-project](https://github.com/swiftwasm/llvm-project)

Forked from [apple/llvm-project](https://github.com/apple/llvm-project).

`swiftwasm` branch is based on `swift/master` branch of `apple/llvm-project`.

Please see [AppleBranchingScheme.md](https://github.com/apple/llvm-project/blob/apple/master/apple-docs/AppleBranchingScheme.md)


### [swiftwasm/icu4c-wasi](https://github.com/swiftwasm/icu4c-wasi)

Build script and patches for building ICU project for WebAssembly

We are sending patches to upstream repository.

- https://github.com/unicode-org/icu/pull/990

### [swiftwasm/wasi-sdk](https://github.com/swiftwasm/wasi-sdk) and [swiftwasm/wasi-libc](https://github.com/swiftwasm/wasi-libc)

Forked from [WebAssembly/wasi-sdk](https://github.com/WebAssembly/wasi-sdk) and [WebAssembly/wasi-libc](https://github.com/WebAssembly/wasi-libc).

We fork them to build `wasi-sysroot` with pthread header. There aren't so many diff from upstream.

