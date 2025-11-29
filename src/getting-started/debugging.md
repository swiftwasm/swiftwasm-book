# Debugging

Debugging is one of the most important parts of application development. This section describes debugging tools compatible with SwiftWasm.

These tools are DWARF-based, so you need to build your application with DWARF sections enabled.
If you are using `swift build`, it is enabled by default.

## Debugging in JavaScript environments

If you're debugging a SwiftWasm app that runs in JavaScript environments (browsers or Node.js), please refer to the [JavaScriptKit Debugging documentation](https://swiftpackageindex.com/swiftwasm/javascriptkit/documentation/javascriptkit/debugging) for detailed information on how to set up and use debugging tools in JavaScript environments.

## Standalone debugging tools

### [wasminspect](https://github.com/kateinoigakukun/wasminspect)

[wasminspect](https://github.com/kateinoigakukun/wasminspect)
can help in the investigation if the debugged binary does not rely on integration with JavaScript.
We recommend splitting your packages into separate executable targets, most of which shouldn't 
assume the availability of JavaScript to make debugging easier.

![demo](https://raw.githubusercontent.com/kateinoigakukun/wasminspect/master/assets/demo.gif)
