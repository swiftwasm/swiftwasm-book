# Running `async` functions in WebAssembly

On macOS, iOS, and Linux, `libdispatch`-based executor is used by default, but `libdispatch` is not supported in single-threaded WebAssembly environment.
However, there are still two global task executors available in SwiftWasm.

If you need multi-threading support on Web, please see [Multithreading guide](./multithreading.md) for more details.

## Cooperative Task Executor

`Cooperative Task Executor` is the default task executor in SwiftWasm. It is a simple single-threaded cooperative task executor implemented in [Swift Concurrency library](https://github.com/apple/swift/blob/0c67ce64874d83b2d4f8d73b899ee58f2a75527f/stdlib/public/Concurrency/CooperativeGlobalExecutor.inc).
If you are not familiar with "Cooperative" in concurrent programming term, see [its definition for more details](https://en.wikipedia.org/wiki/Cooperative_multitasking).

This executor has an *event loop* that dispatches tasks until no more tasks are enqueued, and exits immediately after all tasks are dispatched.
Note that this executor won't yield control to the host environment during execution, so any host's async operation cannot call back to the Wasm execution.

This executor is suitable for WASI command line tools, or host-independent standalone applications.

```swift
// USAGE
// $ swiftc -target wasm32-unknown-wasi -parse-as-library main.swift -o main.wasm
// $ wasmtime main.wasm
@main
struct Main {
    static func main() async throws {
        print("Sleeping for 1 second... 😴")
        try await Task.sleep(nanoseconds: 1_000_000_000)
        print("Wake up! 😁")
    }
}
```

## JavaScript Event Loop-based Task Executor

`JavaScript Event Loop-based Task Executor` is a task executor that cooperates with the JavaScript's event loop. It is provided by [`JavaScriptKit`](https://github.com/swiftwasm/JavaScriptKit), and you need to activate it explicitly by calling a predefined `JavaScriptEventLoop.installGlobalExecutor()` function (see below for more details).

This executor also has its own *event loop* that dispatches tasks until no more tasks are enqueued synchronously.
It yields control to the JavaScript side after all pending tasks are dispatched, so JavaScript program can call back to the executed Wasm module.
After a task is resumed by callbacks from JavaScript, the executor starts its event loop again in the next microtask tick.

To enable this executor, you need to use `JavaScriptEventLoop` module, which is provided as a part of `JavaScriptKit` package.

0. Ensure that you added `JavaScriptKit` dependency to your `Package.swift`
1. Add `JavaScriptEventLoop` dependency to your targets that use this executor

```swift
.product(name: "JavaScriptEventLoop", package: "JavaScriptKit"),
```
2. Import `JavaScriptEventLoop` and call `JavaScriptEventLoop.installGlobalExecutor()` before spawning any tasks to activate the executor instead of the default cooperative executor.

Note that this executor is only available on JavaScript host environment.

See also [`JavaScriptKit` package `README`](https://github.com/swiftwasm/JavaScriptKit/#asyncawait) for more details.

```swift
import JavaScriptEventLoop
import JavaScriptKit

JavaScriptEventLoop.installGlobalExecutor()

let document = JSObject.global.document
var asyncButtonElement = document.createElement("button")
_ = document.body.appendChild(asyncButtonElement)

asyncButtonElement.innerText = "Fetch Zen"
func printZen() async throws {
    let fetch = JSObject.global.fetch.function!
    let response = try await JSPromise(fetch("https://api.github.com/zen").object!)!.value
    let text = try await JSPromise(response.text().object!)!.value
    print(text)
}
asyncButtonElement.onclick = .object(JSClosure { _ in
    Task {
        do {
            try await printZen()
        } catch {
            print(error)
        }
    }

    return .undefined
})
```
