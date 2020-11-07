# Hello, World!

This section will show you how to compile a simple Swift code into WebAssembly and run the produced binary on WASI supported WebAssembly runtime.

## 1. Create a Swift file

```sh
$ echo 'print("Hello, world!")' > hello.swift
```


## 2. Compile Swift code into WebAssembly with WASI

```
$ swiftc -target wasm32-unknown-wasi hello.swift -o hello.wasm
```


## 3. Run the produced binary on WebAssembly runtime

You can the run the produced binary with [wasmer](https://wasmer.io/) (or other WebAssembly runtime):

```sh
$ wasmer hello.wasm
```

The produced binary depends on WASI which is an interface of system call for WebAssembly.
So you need to use WASI supported runtime and when you run the binary on browser, you need WASI polyfill library like [@wasmer/wasi](https://github.com/wasmerio/wasmer-js/tree/master/packages/wasi).