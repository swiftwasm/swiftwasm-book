# Introduction

Welcome to the SwiftWasm Documentation!

[SwiftWasm is an open source project to support the WebAssembly target for Swift.](https://github.com/swiftwasm)

The goal of this project is to fully support the WebAssembly target for [Swift](https://swift.org) and to be merged into [the upstream repository](https://github.com/apple/swift).

WebAssembly is described on its [home page](https://webassembly.org/) as:

> WebAssembly (abbreviated Wasm) is a binary instruction format for a stack-based virtual machine. Wasm is designed as a portable compilation target for programming languages, enabling deployment on the web for client and server applications.


We use LLVM as compiler backend to produce WebAssembly and depends on [WASI](https://wasi.dev), which is a modular system interface for WebAssembly. WASI is mainly used to compile Swift Standard Library.
