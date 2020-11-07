# Exporting function for host environment

You can expose a Swift function for host environment using special attribute and linker option.

```swift
@_cdecl("add")
func add(_ lhs: Int, _ rhs: Int) -> Int {
    return lhs + rhs
}
```

You need to compile the Swift code with linker option `--export`.

```bash
$ swiftc \
    -target wasm32-unknown-wasi \
    lib.swift -o lib.wasm \
    -Xlinker --export=add
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
  let { instance } = await WebAssembly.instantiate(wasmBinary, {
    wasi_snapshot_preview1: wasi.wasiImport,
  });
  // Get the exported function
  const addFn = instance.exports.add;
  console.log("2 + 3 = " + addFn(2, 3))

};

main()
```

If you use SwiftPM package, you can omit linker flag using clang's `__atribute__`. Please see [swiftwasm/JavaScriptKit#91](https://github.com/swiftwasm/JavaScriptKit/pull/91/files) for more detail info