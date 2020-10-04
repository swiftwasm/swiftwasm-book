# JavaScript Interoperation

[JavaScriptKit](https://github.com/swiftwasm/JavaScriptKit) is a Swift framework to interact with JavaScript through WebAssembly.

You can use any JavaScript API from Swift using this library.

```swift
import JavaScriptKit

let document = JSObject.global.document.object!

let divElement = document.createElement!("div").object!
divElement.innerText = "Hello, world"
let body = document.body.object!
_ = body.appendChild!(divElement)
```

JavaScriptKit consists of NPM runtime library package and SwiftPM package for Swift side API.