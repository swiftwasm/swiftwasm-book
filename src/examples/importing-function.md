# Importing function from host environment

You can import a function from host environment using special attribute and linker option.

```swift
@_silgen_name("add")
func add(_: Int, _: Int) -> Int

print("2 + 3 = \(add(2, 3))")
```

You need to compile the Swift code with linker option `--allow-undefined`.


```bash
$ TOOLCHAIN_PATH=$(dirname $(swiftenv which swiftc))/../
$ swiftc \
    -target wasm32-unknown-wasi \
    -sdk $TOOLCHAIN_PATH/share/wasi-sysroot \
    lib.swift -o lib.wasm \
    -Xlinker --allow-undefined
```

Then, you can inject a host function into the produced WebAssembly binary.

Note that we use `env` as default import module name. We will add a way to specify module name to import in the near future.

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

If you use SwiftPM package, you can omit linker flag using clang's `__atribute__`. Please see [swiftwasm/JavaScriptKit#91](https://github.com/swiftwasm/JavaScriptKit/pull/91/files) for more detail info