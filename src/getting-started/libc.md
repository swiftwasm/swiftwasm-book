# `WASILibc` module

When porting existing projects from other platforms to SwiftWasm you might find code that relies on
importing a platform-specific [C library](https://en.wikipedia.org/wiki/C_standard_library) module
directly. It looks like `import Glibc` on Linux, or `import Darwin` on Apple platforms. Fortunately,
most of the standard C library APIs are available when using SwiftWasm, you just need to use
`import WASILibc` to get access to it. Most probably you want to preserve compatibility with other
platforms, thus your imports would look like this:

```swift
#if canImport(Darwin)
import Darwin
#elseif canImport(Glibc)
import Glibc
#elseif canImport(WASILibc)
import WASILibc
#endif
```

## Limitations

WebAssembly and [WASI](https://wasi.dev/) provide a constrained environment, which currently
does not directly support multi-threading, or filesystem access in the browser. Thus, you should
not expect these APIs to work when importing `WASILibc`. This has an impact on what [can support
in the Swift Foundation](/getting-started/foundation.md) at the moment, please be aware of these
limitations when porting your code.