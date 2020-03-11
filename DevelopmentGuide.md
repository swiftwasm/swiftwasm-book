# Development Guide

## How to build

First, checkout the project source code.

```sh
$ mkdir swiftwasm-source
$ cd swiftwasm-source
$ git clone https://github.com/swiftwasm/swift.git
$ ./swift/utils/update-checkout --scheme wasm --clone
```

Before building Swift, please install required dependencies.

```sh
# On macOS
$ ./utils/webassembly/macos/install-dependencies.sh
# On Linux
$ ./utils/webassembly/linux/install-dependencies.sh
```

We support both Linux and macOS to build Swift.
These script wraps `./utils/build-script` to simplify build options.
You can pass additional build-script options through these scripts.

```sh
# On macOS
$ ./utils/webassembly/build-mac.sh -R
# On Linux
$ ./utils/webassembly/build-linux.sh -R
```

If you want to get more information about build system, please feel free to ask @kateinoigakukun on Twitter.

## Development Tips

### Cache
Compilation time of LLVM project is very long, so we recommend to cache the artifact using `sccache`

[Use sccache to cache build artifacts](https://github.com/apple/swift/blob/master/docs/DevelopmentTips.md#use-sccache-to-cache-build-artifacts)


### Debugging

When you want to debug a WebAssembly binary, [wasminspect](https://github.com/kateinoigakukun/wasminspect) is very helpful to investigate.

