# Creating a Browser App

Currently, [the Tokamak UI framework](https://tokamak.dev) is one of the easiest ways to build a
browser app with SwiftWasm. It tries to be compatible with [the SwiftUI
API](https://developer.apple.com/xcode/swiftui/) as much as possible, which potentially allows
you to share most if not all code between SwiftWasm and other platforms.

## Requirements

Tokamak relies on [the `carton` development tool](https://carton.dev) for development and testing.
While you can build Tokamak apps without `carton`, that would require a lot of manual steps that are
already automated with `carton`.

### System Requirements

- [Swift 5.9.2 or later](https://swift.org/download/)

### Installation

1. Create a directory for your project and make it current:

```
mkdir MyApp && cd MyApp
```

2. Initialize the project:

```
swift package init --type executable
```

3. Add Tokamak and carton as dependencies to your `Package.swift`:

```swift
// swift-tools-version:5.8
import PackageDescription
let package = Package(
    name: "MyApp",
    platforms: [.macOS(.v11), .iOS(.v13)],
    dependencies: [
        .package(url: "https://github.com/TokamakUI/Tokamak", from: "0.11.0"),
        .package(url: "https://github.com/swiftwasm/carton", from: "1.0.0"),
    ],
    targets: [
        .executableTarget(
            name: "MyApp",
            dependencies: [
                .product(name: "TokamakShim", package: "Tokamak")
            ]),
    ]
)
```

4. Add your first view to `Sources/main.swift`:

```swift
import TokamakDOM

@main
struct TokamakApp: App {
    var body: some Scene {
        WindowGroup("Tokamak App") {
            ContentView()
        }
    }
}

struct ContentView: View {
    var body: some View {
        Text("Hello, world!")
    }
}
```

5. Build the project and start the development server, `swift run carton dev` can be kept running
   during development:

```
swift run carton dev
```

6. Open [http://127.0.0.1:8080/](http://127.0.0.1:8080/) in your browser to see the app
   running. You can edit the app source code in your favorite editor and save it, `carton`
   will immediately rebuild the app and reload all browser tabs that have the app open.

You can also clone [the Tokamak repository](https://github.com/TokamakUI/Tokamak) and run `carton
dev --product TokamakDemo` in its root directory. This will build the demo app that shows almost all of the currently
implemented APIs.
