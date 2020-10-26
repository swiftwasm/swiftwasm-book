# Use Swift Foundation in your code

[The Foundation core library](https://swift.org/core-libraries/#foundation) is available in
SwiftWasm, but in a limited capacity. The main reason is that [the Dispatch core
library](https://swift.org/core-libraries/#libdispatch) is unavailable due to lack of 
standardized multi-threading support in WebAssembly. Many Foundation APIs rely on the presence
of Dispatch under the hood, specifically file system access and threading helpers. A few other types
are unavailable in browsers or aren't standardized in WASI hosts, such as support for sockets and
low-level networking, or support for time zone files. and they had to be disabled. These types are
therefore absent in SwiftWasm Foundation:

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
