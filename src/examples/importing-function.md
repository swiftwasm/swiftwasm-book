# Importing a function from host environments

## Swift 5.10 or earlier

You can import a function from your host environment using the integration of Swift Package Manager
with C targets. Firstly, you should declare a signature for your function in a C header with an
appropriate `__import_name__` attribute:

```c
__attribute__((__import_name__("add")))
extern int add(int lhs, int rhs);
```

Here `__import_name__` specifies the name under which this function will be exposed to Swift code.
Move this C header to a separate target, we'll call it `HostFunction` in this example. Your
`Package.swift` manifest for your WebAssembly app would look like this:

```swift
// swift-tools-version:5.9
import PackageDescription

let package = Package(
    name: "Example",
    targets: [
      .target(name: "HostFunction", dependencies: []),
      .executableTarget(name: "Example", dependencies: ["HostFunction"]),
    ]
)
```

Place this header into the `include` subdirectory of your `HostFunction` target directory. You can
then import this host function module into Swift code just as any other module:

```swift
import HostFunction

print(add(2, 2))
```

Then, you can inject a host function into the produced WebAssembly binary.

Note that we use `env` as default import module name. You can specify the module name as
`__import_module__` in your C header. The full list of attributes in the header could look
like `__attribute__((__import_module__("env"),__import_name__("add")))`.

```javascript
// File name: main.mjs
import { WASI, File, OpenFile, ConsoleStdout } from "@bjorn3/browser_wasi_shim";
import fs from "fs/promises";

const main = async () => {

  // Instantiate a new WASI Instance
  // See https://github.com/bjorn3/browser_wasi_shim/ for more detail about constructor options
  let wasi = new WASI([], [],
    [
      new OpenFile(new File([])), // stdin
      ConsoleStdout.lineBuffered(msg => console.log(`[WASI stdout] ${msg}`)),
      ConsoleStdout.lineBuffered(msg => console.warn(`[WASI stderr] ${msg}`)),
    ],
    { debug: false }
  );

  const wasmBinary = await fs.readFile(".build/wasm32-unknown-wasi/debug/Example.wasm");

  // Instantiate the WebAssembly file
  let { instance } = await WebAssembly.instantiate(wasmBinary, {
    wasi_snapshot_preview1: wasi.wasiImport,
    env: {
      add: (lhs, rhs) => (lhs + rhs),
    }
  });

  wasi.start(instance);
};

main()
```

If you use Go bindings for Wasmer as your host environment, you should check [an example 
repository](https://github.com/hassan-shahbazi/swiftwasm-go) from one of our contributors that shows
an integration with an imported host function.

A more streamlined way to import host functions will be implemented in the future version of the
SwiftWasm toolchain.


## Swift 6.0 or later

If you are using Swift 6.0 or later, you can use experimental `@_extern(wasm)` attribute

Swift 6.0 introduces a new attribute `@_extern(wasm)` to import a function from the host environment.
To use this experimental feature, you need to enable it in your SwiftPM manifest file:

```swift
.executableTarget(
    name: "Example",
    swiftSettings: [
        .enableExperimentalFeature("Extern")
    ]),
```

Then, you can import a function from the host environment as follows without using C headers:

```swift
@_extern(wasm, module: "env", name: "add")
func add(_ a: Int, _ b: Int) -> Int

print(add(2, 2))
```
