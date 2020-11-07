# JavaScript interoperation

[JavaScriptKit](https://github.com/swiftwasm/JavaScriptKit) is a Swift framework to interact with JavaScript through WebAssembly.

You can use any JavaScript API from Swift with this library. Here's a quick example of JavaScriptKit
usage in a browser app:

```swift
import JavaScriptKit

let document = JSObject.global.document

var divElement = document.createElement("div")
divElement.innerText = "Hello, world"
_ = document.body.appendChild(divElement)
```

You can also use JavaScriptKit in SwiftWasm apps integrated with Node.js, as there no assumptions
that any browser API is present in the library.

JavaScriptKit consists of a runtime library package [hosted on
npm](https://www.npmjs.com/package/javascript-kit-swift), and a SwiftPM package for the API on the
Swift side. To integrate the JavaScript runtime automatically into your app, we recommend following
the corresponding [guide for browser apps in our book](./browser-app.md).


You can get more detailed JavaScriptKit documentation [here](https://swiftwasm.github.io/JavaScriptKit/)
