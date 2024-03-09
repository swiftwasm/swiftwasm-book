# Compile a SwiftPM package to WebAssembly

You can also use SwiftPM for managing packages in the same way as other platforms.


## 1. Create a package from template

```sh
$ swift package init --type executable --name Example 
Creating executable package: Example
Creating Package.swift
Creating .gitignore
Creating Sources/
Creating Sources/main.swift
```

## 2. Build the Project into a WebAssembly binary

You need to pass `--triple` option, which indicates that you are building for the target.

```sh
$ swift build --triple wasm32-unknown-wasi
```

<!--
If [you installed Swift SDK instead of the whole toolchain](./setup.md#experimental-swift-sdk), you need to use the following command:

```sh
$ swift build --experimental-swift-sdk <SDK name>
```
-->

## 3. Run the produced binary

Just as in the [previous section](./hello-world.md), you can run the produced binary with WebAssembly runtimes like `wasmtime`.

```sh
$ wasmtime ./.build/wasm32-unknown-wasi/debug/Example.wasm
Hello, world!
```
