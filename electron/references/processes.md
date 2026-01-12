# Electron - Processes

**Pages:** 7

---

## MessageChannelMain

**URL:** https://www.electronjs.org/docs/latest/api/message-channel-main

**Contents:**
- MessageChannelMain
- Class: MessageChannelMain​
  - Instance Properties​
    - channel.port1​
    - channel.port2​

MessageChannelMain is the main-process-side equivalent of the DOM MessageChannel object. Its singular function is to create a pair of connected MessagePortMain objects.

See the Channel Messaging API documentation for more information on using channel messaging.

Channel interface for channel messaging in the main process.

Electron's built-in classes cannot be subclassed in user code. For more information, see the FAQ.

A MessagePortMain property.

A MessagePortMain property.

**Examples:**

Example 1 (javascript):
```javascript
// Main processconst { BrowserWindow, MessageChannelMain } = require('electron')const w = new BrowserWindow()const { port1, port2 } = new MessageChannelMain()w.webContents.postMessage('port', null, [port2])port1.postMessage({ some: 'message' })// Renderer processconst { ipcRenderer } = require('electron')ipcRenderer.on('port', (e) => {  // e.ports is a list of ports sent along with this message  e.ports[0].onmessage = (messageEvent) => {    console.log(messageEvent.data)  }})
```

---

## MessagePortMain

**URL:** https://www.electronjs.org/docs/latest/api/message-port-main

**Contents:**
- MessagePortMain
- Class: MessagePortMain​
  - Instance Methods​
    - port.postMessage(message, [transfer])​
    - port.start()​
    - port.close()​
  - Instance Events​
    - Event: 'message'​
    - Event: 'close'​

MessagePortMain is the main-process-side equivalent of the DOM MessagePort object. It behaves similarly to the DOM version, with the exception that it uses the Node.js EventEmitter event system, instead of the DOM EventTarget system. This means you should use port.on('message', ...) to listen for events, instead of port.onmessage = ... or port.addEventListener('message', ...)

See the Channel Messaging API documentation for more information on using channel messaging.

MessagePortMain is an EventEmitter.

Port interface for channel messaging in the main process.

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

Sends a message from the port, and optionally, transfers ownership of objects to other browsing contexts.

Starts the sending of messages queued on the port. Messages will be queued until this method is called.

Disconnects the port, so it is no longer active.

Emitted when a MessagePortMain object receives a message.

Emitted when the remote end of a MessagePortMain object becomes disconnected.

---

## ProcessMemoryInfo Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/process-memory-info

**Contents:**
- ProcessMemoryInfo Object

---

## utilityProcess

**URL:** https://www.electronjs.org/docs/latest/api/utility-process

**Contents:**
- utilityProcess
- Methods​
  - utilityProcess.fork(modulePath[, args][, options])​
- Class: UtilityProcess​
  - Instance Methods​
    - child.postMessage(message, [transfer])​
    - child.kill()​
  - Instance Properties​
    - child.pid​
    - child.stdout​

utilityProcess creates a child process with Node.js and Message ports enabled. It provides the equivalent of child_process.fork API from Node.js but instead uses Services API from Chromium to launch the child process.

Returns UtilityProcess

utilityProcess.fork can only be called after the ready event has been emitted on App.

Instances of the UtilityProcess represent the Chromium spawned child process with Node.js integration.

UtilityProcess is an EventEmitter.

Send a message to the child process, optionally transferring ownership of zero or more MessagePortMain objects.

Terminates the process gracefully. On POSIX, it uses SIGTERM but will ensure the process is reaped on exit. This function returns true if the kill is successful, and false otherwise.

A Integer | undefined representing the process identifier (PID) of the child process. Until the child process has spawned successfully, the value is undefined. When the child process exits, then the value is undefined after the exit event is emitted.

You can use the pid to determine if the process is currently running.

A NodeJS.ReadableStream | null that represents the child process's stdout. If the child was spawned with options.stdio[1] set to anything other than 'pipe', then this will be null. When the child process exits, then the value is null after the exit event is emitted.

A NodeJS.ReadableStream | null that represents the child process's stderr. If the child was spawned with options.stdio[2] set to anything other than 'pipe', then this will be null. When the child process exits, then the value is null after the exit event is emitted.

Emitted once the child process has spawned successfully.

Emitted when the child process needs to terminate due to non continuable error from V8.

No matter if you listen to the error event, the exit event will be emitted after the child process terminates.

Emitted after the child process ends.

Emitted when the child process sends a message using process.parentPort.postMessage().

**Examples:**

Example 1 (javascript):
```javascript
// Main processconst { port1, port2 } = new MessageChannelMain()const child = utilityProcess.fork(path.join(__dirname, 'test.js'))child.postMessage({ message: 'hello' }, [port1])// Child processprocess.parentPort.once('message', (e) => {  const [port] = e.ports  // ...})
```

Example 2 (javascript):
```javascript
const child = utilityProcess.fork(path.join(__dirname, 'test.js'))console.log(child.pid) // undefinedchild.on('spawn', () => {  console.log(child.pid) // Integer})child.on('exit', () => {  console.log(child.pid) // undefined})
```

Example 3 (javascript):
```javascript
// Main processconst { port1, port2 } = new MessageChannelMain()const child = utilityProcess.fork(path.join(__dirname, 'test.js'))child.stdout.on('data', (data) => {  console.log(`Received chunk ${data}`)})
```

---

## RenderProcessGoneDetails Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/render-process-gone-details

**Contents:**
- RenderProcessGoneDetails Object

---

## ProcessMetric Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/process-metric

**Contents:**
- ProcessMetric Object

---

## process

**URL:** https://www.electronjs.org/docs/latest/api/process

**Contents:**
- process
- Sandbox​
- Events​
  - Event: 'loaded'​
- Properties​
  - process.defaultApp Readonly​
  - process.isMainFrame Readonly​
  - process.mas Readonly​
  - process.noAsar​
  - process.noDeprecation​

Extensions to process object.

Process: Main, Renderer

Electron's process object is extended from the Node.js process object. It adds the following events, properties, and methods:

In sandboxed renderers the process object contains only a subset of the APIs:

Emitted when Electron has loaded its internal initialization script and is beginning to load the web page or the main script.

A boolean. When the app is started by being passed as parameter to the default Electron executable, this property is true in the main process, otherwise it is undefined. For example when running the app with electron ., it is true, even if the app is packaged (isPackaged) is true. This can be useful to determine how many arguments will need to be sliced off from process.argv.

A boolean, true when the current renderer context is the "main" renderer frame. If you want the ID of the current frame you should use webFrame.routingId.

A boolean. For Mac App Store build, this property is true, for other builds it is undefined.

A boolean that controls ASAR support inside your application. Setting this to true will disable the support for asar archives in Node's built-in modules.

A boolean that controls whether or not deprecation warnings are printed to stderr. Setting this to true will silence deprecation warnings. This property is used instead of the --no-deprecation command line flag.

A string representing the path to the resources directory.

A boolean. When the renderer process is sandboxed, this property is true, otherwise it is undefined.

A boolean that indicates whether the current renderer context has contextIsolation enabled. It is undefined in the main process.

A boolean that controls whether or not deprecation warnings will be thrown as exceptions. Setting this to true will throw errors for deprecations. This property is used instead of the --throw-deprecation command line flag.

A boolean that controls whether or not deprecations printed to stderr include their stack trace. Setting this to true will print stack traces for deprecations. This property is instead of the --trace-deprecation command line flag.

A boolean that controls whether or not process warnings printed to stderr include their stack trace. Setting this to true will print stack traces for process warnings (including deprecations). This property is instead of the --trace-warnings command line flag.

A string representing the current process's type, can be:

A string representing Chrome's version string.

A string representing Electron's version string.

A boolean. If the app is running as a Windows Store app (appx), this property is true, for otherwise it is undefined.

A string (optional) representing a globally unique ID of the current JavaScript context. Each frame has its own JavaScript context. When contextIsolation is enabled, the isolated world also has a separate JavaScript context. This property is only available in the renderer process.

A Electron.ParentPort property if this is a UtilityProcess (or null otherwise) allowing communication with the parent process.

The process object has the following methods:

Causes the main thread of the current process crash.

Returns number | null - The number of milliseconds since epoch, or null if the information is unavailable

Indicates the creation time of the application. The time is represented as number of milliseconds since epoch. It returns null if it is unable to get the process creation time.

Returns an object with V8 heap statistics. Note that all statistics are reported in Kilobytes.

Returns an object with Blink memory information. It can be useful for debugging rendering / DOM related memory issues. Note that all values are reported in Kilobytes.

Returns Promise<ProcessMemoryInfo> - Resolves with a ProcessMemoryInfo

Returns an object giving memory usage statistics about the current process. Note that all statistics are reported in Kilobytes. This api should be called after app ready.

Chromium does not provide residentSet value for macOS. This is because macOS performs in-memory compression of pages that haven't been recently used. As a result the resident set size value is not what one would expect. private memory is more representative of the actual pre-compression memory usage of the process on macOS.

Returns an object giving memory usage statistics about the entire system. Note that all statistics are reported in Kilobytes.

Returns string - The version of the host operating system.

It returns the actual operating system version instead of kernel version on macOS unlike os.release().

Returns boolean - Indicates whether the snapshot has been created successfully.

Takes a V8 heap snapshot and saves it to filePath.

Causes the main thread of the current process hang.

Sets the file descriptor soft limit to maxDescriptors or the OS hard limit, whichever is lower for the current process.

**Examples:**

Example 1 (javascript):
```javascript
const version = process.getSystemVersion()console.log(version)// On macOS -> '10.13.6'// On Windows -> '10.0.17763'// On Linux -> '4.15.0-45-generic'
```

---
