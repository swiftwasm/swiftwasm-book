# Importing a function from host environments

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
// swift-tools-version:5.3
// The swift-tools-version declares the minimum version of Swift required to build this package.
import PackageDescription

let package = Package(
    name: "SwiftWasmApp",
    targets: [
      .target(name: "HostFunction", dependencies: []),
      .target(name: "SwiftWasmApp", dependencies: ["HostFunction"]),
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

Note that we use `env` as default import module name. We will add a way to specify module name to
import in the near future.

```javascript
const WASI = require("@wasmer/wasi").WASI;
const WasmFs = require("@wasmer/wasmfs").WasmFs;

const promisify = require("util").promisify;
const fs = require("fs");
const readFile = promisify(fs.readFile);

const main = async () => {
  const wasmFs = new WasmFs();
  // Output stdout and stderr to console
  const originalWriteSync = wasmFs.fs.writeSync;
  wasmFs.fs.writeSync = (fd, buffer, offset, length, position) => {
    const text = new TextDecoder("utf-8").decode(buffer);
    switch (fd) {
      case 1:
        console.log(text);
        break;
      case 2:
        console.error(text);
        break;
    }
    return originalWriteSync(fd, buffer, offset, length, position);
  };

  // Instantiate a new WASI Instance
  let wasi = new WASI({
    args: [],
    env: {},
    bindings: {
      ...WASI.defaultBindings,
      fs: wasmFs.fs,
    },
  });

  const wasmBinary = await readFile("lib.wasm");

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
