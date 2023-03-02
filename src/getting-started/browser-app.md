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

- macOS 10.15 and Xcode 11.4 or later.
- [Swift 5.2 or later](https://swift.org/download/) and Ubuntu 18.04 for Linux users.

### Installation

On macOS `carton` can be installed with [Homebrew](https://brew.sh/). Make sure you have Homebrew
installed and then run:

```sh
brew install swiftwasm/tap/carton
```

You'll have to build `carton` from sources on Linux. Clone the repository and run
`swift build -c release`, the `carton` binary will be located in the `.build/release`
directory after that.
Assuming you already have Homebrew installed, you can create a new Tokamak
app by following these steps:

1. Install `carton`:

```
brew install swiftwasm/tap/carton
```

If you had `carton` installed before this, make sure you have version 0.6.1 or greater:

```
carton --version
```

2. Create a directory for your project and make it current:

```
mkdir TokamakApp && cd TokamakApp
```

3. Initialize the project from a template with `carton`:

```
carton init --template tokamak
```

4. Build the project and start the development server, `carton dev` can be kept running
   during development:

```
carton dev
```

5. Open [http://127.0.0.1:8080/](http://127.0.0.1:8080/) in your browser to see the app
   running. You can edit the app source code in your favorite editor and save it, `carton`
   will immediately rebuild the app and reload all browser tabs that have the app open.

You can also clone [the Tokamak repository](https://github.com/TokamakUI/Tokamak) and run `carton
dev --product TokamakDemo` in its root directory. This will build the demo app that shows almost all of the currently
implemented APIs.
