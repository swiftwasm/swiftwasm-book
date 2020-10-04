# Build Swift PM package into WebAssembly

You can also use SwiftPM for managing packages in the same way as other platforms.


## 1. Create a package from template

```sh
$ swift package init --type executable
Creating executable package: hello
Creating Package.swift
Creating README.md
Creating .gitignore
Creating Sources/
Creating Sources/hello/main.swift
Creating Tests/
Creating Tests/LinuxMain.swift
Creating Tests/helloTests/
Creating Tests/helloTests/helloTests.swift
Creating Tests/helloTests/XCTestManifests.swift
```

## 2. Build the Project into WebAssembly binary

You need to pass `--triple` option, which indicates that you are building for the target.

```sh
$ swift build --triple wasm32-unknown-wasi
```

## 3. Run the produced binary

As well as [previous section](./hello-world.md), you can run the produced binary with WebAssembly runtime.

```sh
$ wasmer ./.build/debug/hello-swiftwasm
Hello, world!
```