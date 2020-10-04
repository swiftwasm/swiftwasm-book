# How to build toolchain

## 1. Checkout the project source code.

```sh
$ mkdir swiftwasm-source
$ cd swiftwasm-source
$ git clone https://github.com/swiftwasm/swift.git
$ ./swift/utils/update-checkout --scheme wasm --clone
```

## 2. Install required dependencies

Before building Swift, please install required dependencies.

```sh
# On macOS
$ brew install cmake ninja llvm sccache wasmer
$ ./utils/webassembly/macos/install-dependencies.sh
# On Linux
$ ./utils/webassembly/linux/install-dependencies.sh
```

## 3. Build using custom preset options

We support both Linux and macOS to build Swift. You need to select the preset name, [`sccache`](./cache.md) path and LLVM tools directory.


```sh
# On macOS
$ ./utils/build-script \
        --preset=webassembly-macos-target \
        --preset-file ./utils/webassembly/build-presets.ini  \
        SOURCE_PATH=$(dirname $(pwd)) \
        LLVM_BIN_DIR=/usr/local/opt/llvm/bin \
        C_CXX_LAUNCHER=$(which sccache)
# On Linux
$ ./utils/build-script \
        --preset=webassembly-linux-target \
        --preset-file ./utils/webassembly/build-presets.ini  \
        SOURCE_PATH=$(dirname $(pwd)) \
        LLVM_BIN_DIR=/usr/local/opt/llvm/bin \
        C_CXX_LAUNCHER=$(which sccache)
```

Or if you want to build whole toolchain, please use `./utils/webassembly/build-toolchain.sh`. This script builds compiler, Swift Standard Library for host environment (e.g. macOS or Linux) and target environment (`wasm32-unknown-wasi`), Foundation and other packages. So it takes longer time than the above script.

```bash
$ ./utils/webassembly/build-toolchain.sh
```

If you want to get more information about build system, please feel free to ask @kateinoigakukun on Twitter or GitHub.
