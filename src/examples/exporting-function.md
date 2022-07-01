# Exporting function for host environment

You can expose a Swift function for host environment using special attribute and linker option.

```swift
@_cdecl("add")
func add(_ lhs: Int, _ rhs: Int) -> Int {
    return lhs + rhs
}
```

You need to compile the Swift code with linker option `--export`.

To call the exported function as a library multiple times, you need to:

1. Compile it as a [*WASI reactor* execution model](https://github.com/WebAssembly/WASI/blob/bac366c8aeb69cacfea6c4c04a503191bf1cede1/legacy/application-abi.md).
   The default execution model is *command*, so you need to pass `-mexec-model=reactor` to linker.
2. Call `_initialize` function before interacting with the instance.

If your code has any top-level code, you need to export `main` function as well, and call it after `_initialize` function.

```bash
$ swiftc \
    -target wasm32-unknown-wasi \
    lib.swift -o lib.wasm \
    -Xlinker --export=add \
    -Xswiftc -Xclang-linker -Xswiftc -mexec-model=reactor \
    -Xlinker --export=main # Optional
```

Then, you can use the exported function from host environment.

```javascript
const WASI = require("@wasmer/wasi").WASI;
const WasmFs = require("@wasmer/wasmfs").WasmFs;

const promisify = require("util").promisify;
const fs = require("fs");
const readFile = promisify(fs.readFile);

const main = async () => {
  // Instantiate a new WASI Instance
  const wasmFs = new WasmFs();
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
  const { instance } = await WebAssembly.instantiate(wasmBinary, {
    wasi_snapshot_preview1: wasi.wasiImport,
  });
  // Initialize the instance by following WASI reactor ABI
  instance.exports._initialize();
  // (Optional) Run the top-level code
  instance.exports.main();
  // Get the exported function
  const addFn = instance.exports.add;
  console.log("2 + 3 = " + addFn(2, 3))

};

main()
```

If you use SwiftPM package, you can omit linker flag using clang's `__atribute__`. Please see [swiftwasm/JavaScriptKit#91](https://github.com/swiftwasm/JavaScriptKit/pull/91/files) for more detail info
