# Introduction

Welcome to the SwiftWasm Documentation!

[SwiftWasm is an open source project to support the WebAssembly target for Swift.](https://github.com/swiftwasm)

The goal of this project is to fully support the WebAssembly target for [Swift](https://swift.org) and to be merged into [the upstream repository](https://github.com/apple/swift).

WebAssembly is described on its [home page](https://webassembly.org/) as:

> WebAssembly (abbreviated as Wasm) is a binary instruction format for a stack-based virtual machine. Wasm is designed as a portable compilation target for programming languages, enabling deployment on the web for client and server applications.


We use LLVM as a compiler backend to produce WebAssembly binaries. Our resulting binaries also depend on [WASI](https://wasi.dev), which is a modular system interface for WebAssembly. WASI is mainly required to compile Swift Standard Library.

## Project Status

[All compiler and standard library changes have been upstreamed to the official Swift repository, and the upstream CI is now testing WebAssembly targets.](https://forums.swift.org/t/stdlib-and-runtime-tests-for-wasm-wasi-now-available-on-swift-ci/70385)

The remaining works are:

- Integrating the build system with the official Swift CI.
