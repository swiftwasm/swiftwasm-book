# How to build toolchain

This document describes how to build the toolchain for WebAssembly.
This is just a quick guide, so if you want to know more about the toolchain, it might be good entry point to read [continuous integration scripts](https://github.com/swiftwasm/swift/blob/swiftwasm/.github/workflows/build-toolchain.yml).
Or you can ask questions in GitHub issues or SwiftWasm Discord server (see [the official website](https://swiftwasm.org) for the link).

## 1. Checkout the project source code.

```sh
$ mkdir swiftwasm-source
$ cd swiftwasm-source
$ git clone https://github.com/swiftwasm/swift.git
$ ./swift/utils/update-checkout --clone --scheme wasm
```

## 2. Install required dependencies

1. [Please follow the upstream instruction](https://github.com/apple/swift/blob/main/docs/HowToGuides/GettingStarted.md#installing-dependencies)
2. Download WebAssembly specific build toolchain

```sh
$ ./swift/utils/webassembly/install-build-sdk.sh
```

3. (If you want to run test suite) Install [`Wasmer`](https://wasmer.io/)

## 3. Build the toolchain

`./swift/utils/webassembly/build-toolchain.sh` will build:

1. Swift compiler that can compile Swift code to WebAssembly support
2. Swift standard library and core libraries for WebAssembly


## Build on Docker

You can also build the toolchain on Docker image used in CI.

```sh
$ docker volume create oss-swift-package
$ docker run --name swiftwasm-ci-buildbot \
    -dit \
    -w /home/build-user/ \
    -v $PWD/swift:/home/build-user/swift \
    -v oss-swift-package:/home/build-user \
    ghcr.io/swiftwasm/swift-ci:main-ubuntu-20.04
$ docker exec swiftwasm-ci-buildbot ./swift/utils/webassembly/ci.sh
$ docker cp swiftwasm-ci-buildbot:/home/build-user/swift-wasm-DEVELOPMENT-SNAPSHOT-*-ubuntu-20.04.tar.gz .
```

