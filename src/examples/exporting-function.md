# Exporting function for host environment

## Swift 6.0 or later

If you use Swift 6.0 or later, you can use `@_expose(wasm, "add")` and omit the `--export` linker flag.

```swift
// File name: lib.swift
@_expose(wasm, "add")
@_cdecl("add") // This is still required to call the function with C ABI
func add(_ lhs: Int, _ rhs: Int) -> Int {
    return lhs + rhs
}
```

Then you can compile the Swift code with the following command without `--export` linker flag.

```bash
$ swiftc \
    -target wasm32-unknown-wasi \
    -parse-as-library \
    lib.swift -o lib.wasm \
    -Xclang-linker -mexec-model=reactor
```

## Swift 5.10 or earlier

You can expose a Swift function for host environment using special attribute and linker option.

```swift
// File name: lib.swift
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

```bash
$ swiftc \
    -target wasm32-unknown-wasi \
    -parse-as-library \
    lib.swift -o lib.wasm \
    -Xlinker --export=add \
    -Xclang-linker -mexec-model=reactor
```

Then, you can use the exported function from host environment.

```javascript
// File name: main.mjs
import { WASI, File, OpenFile, ConsoleStdout } from "@bjorn3/browser_wasi_shim";
import fs from "fs/promises";

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

const wasmBinary = await fs.readFile("lib.wasm");

// Instantiate the WebAssembly file
const { instance } = await WebAssembly.instantiate(wasmBinary, {
  wasi_snapshot_preview1: wasi.wasiImport,
});
// Initialize the instance by following WASI reactor ABI
wasi.initialize(instance);
// Get the exported function
const addFn = instance.exports.add;
console.log("2 + 3 = " + addFn(2, 3))
```

If you use SwiftPM package, you can omit linker flag using clang's `__atribute__`. Please see [swiftwasm/JavaScriptKit#91](https://github.com/swiftwasm/JavaScriptKit/pull/91/files) for more detail info

