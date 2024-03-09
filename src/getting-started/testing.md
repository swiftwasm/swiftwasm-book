# Testing your app

You can write a test suite for your SwiftWasm app or library, or run an existing test suite
written for `XCTest` if you port existing code to SwiftWasm. Your project has to have a
`Package.swift` package manifest for this to work. We assume that you use SwiftPM to build your
project and that you have a working package manifest. Please follow [our SwiftPM guide](./swift-package.md) for new projects.

## A simple test case

Let's assume you have a `SwiftWasmLibrary` target in your project that you'd like to test. Your
`Package.swift` should also have a test suite target with a dependency on the library target. It
would probably look like this:

```swift
// swift-tools-version: 5.9

import PackageDescription

let package = Package(
    name: "Example",
    products: [
        .library(name: "Example", targets: ["Example"]),
    ],
    targets: [
        .target(name: "Example"),
        .testTarget(name: "ExampleTests", dependencies: ["Example"]),
    ]
)
```

Now you should make sure there's `Tests/ExampleTests` subdirectory in your project.
If you don't have any files in it yet, create `ExampleTests.swift` in it:

```swift
import Example
import XCTest

final class ExampleTests: XCTestCase {
  func testTrivial() {
    XCTAssertEqual(text, "Hello, world")
  }
}
```

This code assumes that your `Example` defines some `text` with `"Hello, world"` value
for this test to pass. Your test functions should all start with `test`, please see [XCTest 
documentation](https://developer.apple.com/documentation/xctest/defining_test_cases_and_test_methods)
for more details.

## XCTest limitations in the SwiftWasm toolchain

As was mentioned in [our section about Swift Foundation](/foundation.md), multi-threading and
file system APIs are currently not available in SwiftWasm. This means that `XCTestExpectation`
and test hooks related to `Bundle` (such as `testBundleWillStart(_:)` and `testBundleDidFinish(_:)`)
are not available in test suites compiled with SwiftWasm. If you have an existing test suite you're
porting to WebAssembly, you should use `#if os(WASI)` directives to exclude places where you use
these APIs from compilation.

## Building and running the test suite with `SwiftPM`

You can build your test suite by running this command in your terminal:

```sh
$ swift build --build-tests --triple wasm32-unknown-wasi
```

If you're used to running `swift test` to run test suites for other Swift platforms, we have to
warn you that this won't work. `swift test` doesn't know what WebAssembly environment you'd like to 
use to run your tests. Because of this building tests and running them are two separate steps when
using `SwiftPM`. After your tests are built, you can use a WASI-compatible host such as
[wasmtime](https://wasmtime.dev/) to run the test bundle:

```sh
$ wasmtime .build/wasm32-unknown-wasi/debug/ExamplePackageTests.wasm
```

As you can see, the produced test binary starts with the name of your package followed by
`PackageTests.wasm`. It is located in the `.build/debug` subdirectory, or in the `.build/release`
subdirectory when you build in release mode.

## Building and running the test suite with `carton`

If you use [`carton`](https://carton.dev) to develop and build your app, as described in [our guide
for browser apps](./browser-app.md), just run `carton test` in the
root directory of your package. This will automatically build the test suite and run it with a WASI runtime for you.
