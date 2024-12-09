# Using threads in WebAssembly

## Background

While the WebAssembly spec defines [atomic operations](https://github.com/WebAssembly/threads),
it does not define a way to create threads. This means that WebAssembly modules can't create
threads themselves, and the host environment must provide a way to create threads and run
WebAssembly modules on them.

## [`wasi-threads`](https://github.com/WebAssembly/wasi-threads) proposal

The WebAssembly System Interface (WASI) had a proposal for adding thread creation APIs to WASI.
The proposal was implemented in several WASI host runtimes, including Wasmtime and wasm-micro-runtime,
but it was [withdrawn in August 2023](https://github.com/WebAssembly/wasi-threads/issues/48#issuecomment-1696407630) in favor of [shared-everything-threads](https://github.com/WebAssembly/shared-everything-threads) proposal. However, the shared-everything-threads proposal is still in the early stages of development and is not yet available in any WASI host runtime. So, for now, we are employing the `wasi-threads` ABI in SwiftWasm to provide thread support immediately.

The `wasi-threads` feature is only available in the `wasm32-unknown-wasip1-threads` target triple, which is explicitly distinct from the `wasm32-unknown-wasi` target triple.

The `wasm32-unknown-wasip1-threads` target triple is only available in the nightly Swift SDK for WebAssembly.

You can run WebAssembly programs built with the `wasm32-unknown-wasip1-threads` target by using the `wasmtime` runtime with the `--wasi threads` flag.
Check [a recent nightly Swift SDK release](https://github.com/swiftwasm/swift/releases) and how to install it [here](./setup-snapshot.md).

```console
# Assume you are using swift-DEVELOPMENT-SNAPSHOT-2024-12-04-a toolchain
$ swift sdk install https://github.com/swiftwasm/swift/releases/download/swift-wasm-DEVELOPMENT-SNAPSHOT-2024-12-05-a/swift-wasm-DEVELOPMENT-SNAPSHOT-2024-12-05-a-wasm32-unknown-wasip1-threads.artifactbundle.zip --checksum 1796ae86f3c90b45d06ee29bb124577aa4135585bbd922430b6d1786f855697d
$ swift build --swift-sdk wasm32-unknown-wasip1-threads
# Enable the `wasi-threads` feature in wasmtime
$ wasmtime --wasi threads .build/debug/Example.wasm
```

Note that even with the `wasi-threads` feature, the default concurrency execution model is still single-threaded as we have not yet ported libdispatch. The `wasi-threads` feature is only used to provide a low-level `pthread` API for creating threads.

## `WebWorkerTaskExecutor` - shared-everything concurrency

We provide `WebWorkerTaskExecutor`, a [`TaskExecutor`](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0417-task-executor-preference.md) implementation that runs `Task`s in a Web Worker. This allows you to run Swift code concurrently in a Web Worker sharing the same memory space.

This feature is available in the `JavaScriptKit` package and you need to use `wasm32-unkonwn-wasip1-threads` target and [`SharedArrayBuffer`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer) to use it.

See more details in the following links:

- [Add `WebWorkerTaskExecutor` · Pull Request #256 · swiftwasm/JavaScriptKit](https://github.com/swiftwasm/JavaScriptKit/pull/256)
- [JavaScriptKit/Examples/Multithreading at main · swiftwasm/JavaScriptKit](https://github.com/swiftwasm/JavaScriptKit/tree/main/Examples/Multithreading)
- [WebWorkerTaskExecutor | Documentation](https://swiftpackageindex.com/swiftwasm/javascriptkit/main/documentation/javascripteventloop/webworkertaskexecutor)

## `WebWorkerKit` - shared-nothing concurrency

If you can't use `SharedArrayBuffer` or want to run Swift code in a separate memory space, you can use `WebWorkerKit`. `WebWorkerKit` is a library that provides a way to run Swift Distributed Actors in their own worker "thread" in a Web Worker. It's message-passing based and allows you to run Swift code concurrently in a Web Worker without sharing memory space.

Check the repository for more details: [swiftwasm/WebWorkerKit: A way of running Swift Distributed Actors in their own worker "thread"](https://github.com/swiftwasm/WebWorkerKit)
