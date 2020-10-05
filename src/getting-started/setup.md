# Installation

You can install latest toolchain of SwiftWasm from [Release Page](https://github.com/swiftwasm/swift/releases)

The toolchains can be installed via [`swiftenv`](https://github.com/kylef/swiftenv) like [official nightly snapshot](https://swift.org/download/#snapshots).

Here's an example `swiftenv` invocation for installing the latest stable SwiftWasm toolchain on macOS Catalina:
```sh

$ swiftenv install \
  https://github.com/swiftwasm/swift/releases/download/swift-wasm-5.3-SNAPSHOT-2020-10-02-a/swift-wasm-5.3-SNAPSHOT-2020-10-02-a-osx.tar.gz
$ swift --version
Swift version 5.3-dev (LLVM ba56ef042e, Swift 5855a96018)
Target: x86_64-apple-darwin19.3.0
```
