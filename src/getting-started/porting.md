# Porting code from other platforms to WebAssembly

In the form that's currently standardized and supported by browsers and other hosts, WebAssembly
is a 32-bit architecture. You have to take this into account when porting code from other
platforms, since `Int` type is a signed 32-bit integer, and `UInt` is an unsigned 32-bit integer
when building with SwiftWasm. You'll need to audit codepaths that cast 64-bit integers to `Int`
or `UInt`, and a good amount of cross-platform test coverage can help with that.

Additionally, there a differences in APIs exposed by the standard C library and Swift core
libraries which we discuss in the next few subsections.

## `WASILibc` module

When porting existing projects from other platforms to SwiftWasm you might stumble upon code that
relies on importing a platform-specific [C
library](https://en.wikipedia.org/wiki/C_standard_library) module directly. It looks like `import
Glibc` on Linux, or `import Darwin` on Apple platforms. Fortunately, most of the standard C library
APIs are available when using SwiftWasm, you just need to use `import WASILibc` to get access to it.
Most probably you want to preserve compatibility with other platforms, thus your imports would look
like this:

```swift
#if canImport(Darwin)
import Darwin
#elseif canImport(Glibc)
import Glibc
#elseif canImport(WASILibc)
import WASILibc
#endif
```

### Limitations

WebAssembly and [WASI](https://wasi.dev/) provide a constrained environment, which currently does
not directly support multi-threading, or filesystem access in the browser. Thus, you should not
expect these APIs to work when importing `WASILibc`. Please be aware of these limitations when
porting your code, which also has an impact on what [can be supported in the Swift
Foundation](#swift-foundation-and-dispatch) at the moment.

## Swift Foundation and Dispatch

[The Foundation core library](https://swift.org/core-libraries/#foundation) is available in
SwiftWasm, but in a limited capacity. The main reason is that [the Dispatch core
library](https://swift.org/core-libraries/#libdispatch) is unavailable due to [the lack of 
standardized multi-threading support](https://github.com/swiftwasm/swift/issues/1887) in WebAssembly
and SwiftWasm itself. Many Foundation APIs rely on the presence of Dispatch under the hood,
specifically file system access and threading helpers. A few other types are unavailable in browsers
or aren't standardized in WASI hosts, such as support for sockets and low-level networking, or
support for time zone files, and they had to be disabled. These types are therefore absent in
SwiftWasm Foundation:

* `FoundationNetworking` types, such as `URLSession` and related APIs
* `FileManager`
* `Host`
* `Notification`
* `NotificationQueue`
* `NSKeyedArchiver`
* `NSKeyedArchiverHelpers`
* `NSKeyedCoderOldStyleArray`
* `NSKeyedUnarchiver`
* `NSNotification`
* `NSSpecialValue`
* `Port`
* `PortMessage`
* `Process`
* `ProcessInfo`
* `PropertyListEncoder`
* `RunLoop`
* `Stream`
* `Thread`
* `Timer`
* `UserDefaults`

Related functions and properties on other types are also absent or disabled. We would like to make
them available in the future as soon as possible, and [we invite you to 
contribute](../contribution-guide/index.md) and help us in achieving this goal!
diff --git a/src/getting-started/libc.md b/src/getting-started/libc.md
