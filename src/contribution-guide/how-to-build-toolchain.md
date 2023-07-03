# How to build toolchain

This document describes how to build the toolchain for WebAssembly.
This is just a quick guide, so if you want to know more about the toolchain, it might be good entry point to read [continuous integration scripts](https://github.com/swiftwasm/swiftwasm-build/blob/main/.github/workflows/build-toolchain.yml).
Or you can ask questions in GitHub issues or SwiftWasm Discord server (see [the official website](https://swiftwasm.org) for the link).

## 1. Checkout the project source code.

```sh
$ mkdir swiftwasm-source
$ cd swiftwasm-source
$ git clone https://github.com/swiftwasm/swiftwasm-build.git
$ cd swiftwasm-build
$ ./tools/build/install-build-sdk.sh main
$ ./tools/git-swift-workspace --scheme main
```

## 2. Install required dependencies

1. [Please follow the upstream instruction](https://github.com/apple/swift/blob/main/docs/HowToGuides/GettingStarted.md#installing-dependencies)
2. (If you want to run test suite) Install [`Wasmtime`](https://wasmtime.dev/)

## 3. Build the toolchain

`./tools/build/build-toolchain.sh`

This script will build the following components:

1. Swift compiler that can compile Swift code to WebAssembly support
2. Swift standard library and core libraries for WebAssembly


## Build on Docker

You can also build the toolchain on Docker image used in CI.
Note that you have already checked out the source code in [the previous step](#1-checkout-the-project-source-code).

```sh
$ docker volume create oss-swift-package
$ docker run --name swiftwasm-ci-buildbot \
    -dit \
    -w /home/build-user/ \
    -v ./swiftwasm-source:/source \
    -v oss-swift-package:/home/build-user \
    ghcr.io/swiftwasm/swift-ci:main-ubuntu-20.04
$ docker exec swiftwasm-ci-buildbot /bin/bash -lc 'env; cp -r /source/* /home/build-user/; ./swiftwasm-build/tools/build/ci.sh main'
$ docker cp swiftwasm-ci-buildbot:/home/build-user/swift-wasm-DEVELOPMENT-SNAPSHOT-*-ubuntu-20.04.tar.gz .
```

