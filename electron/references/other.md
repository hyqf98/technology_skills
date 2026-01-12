# Electron - Other

**Pages:** 4

---

## Why Electron

**URL:** https://www.electronjs.org/docs/latest/why-electron

**Contents:**
- Why Electron
- Why choose web technologies​
  - Versatility​
  - Reliability​
  - Interoperability​
  - Ubiquity​
- Why choose Electron​
  - Enterprise-grade​
  - Mature​
  - Stability, security, performance​

Electron is a framework enabling developers to build cross-platform desktop applications for macOS, Windows, and Linux by combining web technologies (HTML, JavaScript, CSS) with Node.js and native code. It is open-source, MIT-licensed, and free for both commercial and personal use. In this document, we’ll explain why companies and developers choose Electron.

We can split up the benefits of Electron in two questions: First, why should you use web technologies to build your application? Then, why should you choose Electron as the framework to do so?

If you’re already using web technologies for your application, you can skip straight to the Why Electron? section below.

Web technologies include HTML, CSS, JavaScript, and WebAssembly. They’re the storefront of the modern Internet. Those technologies have emerged as the best choice for building user interfaces — both for consumer applications as well as mission-critical business applications. This is true both for applications that need to run in a browser as well as desktop applications that are not accessible from a browser. Our bold claim here is that this isn’t just true for cross-platform applications that need to run on multiple operating systems but true overall.

As an example, NASA’s actual Mission Control is written with web technologies. The Bloomberg Terminal, the computer system found at every financial institution, is written with web technologies and runs inside Chromium. It costs $25,000 per user, per year. The McDonald’s ordering kiosk, powering the world’s biggest food retailer, is entirely built with Chromium. The SpaceX’s Dragon 2 space capsule uses Chromium to display its interface. You get the point: web technologies are a great tech stack to build user interfaces.

Here are the reasons we, the Electron maintainers, are betting on the web.

Modern versions of HTML and CSS enable your developers and designers to fully express themselves. The web’s showcase includes Google Earth, Netflix, Spotify, Gmail, Facebook, Airbnb, or GitHub. Whatever interface your application needs, you will be able to express it with HTML, CSS, and JavaScript.

If you want to focus on building a great product without figuring out how you can realize your designer’s vision in a specific UI framework, the web is a safe bet.

Web technologies are the most-used foundation for user interfaces on the planet. They have been hardened accordingly. Modern computers have been optimized from the CPU to the operating system to be good at running web technologies. The manufacturers of your user’s devices—be that an Android phone or the latest MacBook—will ensure that they can visit websites, play videos on YouTube, or display emails. In turn, they’ll also ensure that your app has a stable foundation, even if you have just one user.

If you want to focus on building a great product without debugging a weird quirk that nobody has found before, the web is a safe bet.

Whatever provider or customer data you need to interact with, they will have probably thought of an integration path with the web. Depending on your technology choice, embedding a YouTube video either takes 30 seconds or requires you to hire a team devoted to streaming and hardware-accelerated video decoding. In the case of YouTube, using anything other than the provided players is actually against their terms and conditions, so you’ll likely embed a browser frame before you implement your own video streaming decoder.

There will be virtually no platform where your app cannot run if you build it with web technologies. Virtually all devices with a display—be that an ATM, a car infotainment system, a smart TV, a fridge, or a Nintendo Switch—come with means to display web technologies. The web is safe bet if you want to be cross-platform.

It’s easy to find developers with experience building with web technologies. If you’re a developer, it’ll be easy to find answers to your questions on Google, Stack Overflow, GitHub, or a coding AI of your choice. Whatever problem you need to solve, it’s likely that somebody has solved it well before—and that you can find the answer to the puzzle online.

If you want to focus on building a great product with ample access to resources and materials, the web is a safe bet.

Electron combines Chromium, Node.js, and the ability to write custom native code into one framework for building powerful desktop applications. There are three main reasons to use Electron:

Electron is reliable, secure, stable, and mature. It is the premier choice for companies building their flagship product. We have a list of some of those companies on our homepage, but just among chat apps, Slack, Discord, and Signal are built with Electron. Among AI applications, both OpenAI’s ChatGPT and Anthropic’s Claude use Electron. Visual Studio Code, Loom, Canva, Notion, Docker, and countless other leading developers of software bet on Electron.

We did make it a priority to make Electron easy to work with and a delight for developers. That’s likely the main reason why Electron became as popular as it is today — but what keeps Electron alive and thriving is the maintainer’s focus on making Electron as stable, secure, performant, and capable of mission-critical use cases for end users as possible. We’re building an Electron that is ready to be used in scenarios where unfixable bugs, unpatched security holes, and outages of any kind are worst-case scenarios.

Our current estimation is that most desktop computers on the planet run at least one Electron app. Electron has grown by prioritizing talent in its maintainer group, fostering excellent and sustainable engineering practices in managing the ongoing maintenance, and proactively inviting companies betting on Electron to directly contribute to the project. We’re an impact project with the OpenJS foundation, which is itself a part of the Linux foundation. We share resources and expertise with other foundation projects like Node.js, ESLint, Webpack - or the Linux Kernel or Kubernetes.

What does all of that mean for you, a developer, in practice?

Electron delivers the best experience on all target platforms (macOS, Windows, Linux) by bundling the latest version of Chromium, V8, and Node.js directly with the application binary. When it comes to running and rendering web content with upmost stability, security, and performance, we currently believe that stack to be “best in class”.

You might wonder why we bundle Chromium’s web stack with our apps when most modern operating systems already ship a browser and some form of web view. Bundling doesn’t just increase the amount of work for Electron maintainers dramatically, it also increases the total disk size of Electron apps (most apps are >100MB). Many Electron maintainers once developed applications that did make use of embedded web views — and have since accepted the increased disk size and maintainer work as a worthy trade-off.

When using an operating system's built-in web view, you're limited by the browser version included in the oldest operating system version you need to support. We have found the following problems with this approach:

Electron aims to enable the apps it supports to deliver the best possible user experience, followed by the best possible developer experience. Chromium is currently the best cross-platform rendering stack available. Node.js uses Chromium’s JavaScript engine V8, allowing us to combine the powers of both.

To summarize, we aim to build an Electron that is mature, enterprise-grade, and ready for mission-critical applications. We prioritize reliability, stability, security, and performance. That said, you might also choose Electron for its developer experience:

As outlined above, the web is an amazing platform for building interfaces. That doesn’t mean that we, the maintainers, would build everything with HTML and CSS. Here are some notable exceptions:

Resource-Constrained Environments and IoT: In scenarios with very limited memory or processing power (say, one megabyte of memory and 100MHz of processing power on a low-powered ARM Cortex-M), you will likely need to use a low-level language to directly talk to the display to output basic text and images. Even on slightly higher-powered single-chip devices you might want to consider an embedded UI framework. A classic example is a smart watch.

Small Disk Footprint: Zipped Electron apps are usually around 80 to 100 Megabytes. If a smaller disk footprint is a hard requirement, you’ll have to use something else.

Operating System UI Frameworks and Libraries: By allowing you to write native code, Electron can do anything a native application can do, including the use of the operating system’s UI components, like WinUI, SwiftUI, or AppKit. In practice, most Electron apps make rare use of that ability. If you want the majority of your app to be built with operating system-provided interface components, you’ll likely be better off building fully native apps for each operating system you’d like to target. It’s not that it’s impossible with Electron, it’ll just likely be an overall easier development process.

Games and Real-Time Graphics: If you're building a high-performance game or application requiring complex real-time 3D graphics, native frameworks like Unity, Unreal Engine, or DirectX/OpenGL will provide better performance and more direct access to graphics hardware. Web fans might point out caveats, like the fact that even Unreal Engine ships with Chromium — or that WebGPU and WebGL are developing rapidly and many game engines, including the ones listed here, can now output their games in a format that runs in a browser. That said, if you asked us to build the next AAA game, we’d likely use something else than just web technologies.

Embedding Lightweight Websites: Electron apps typically are mostly web apps with native code sprinkled in where useful. Processing-heavy Electron applications tend to write the UI in HTML/CSS and build the backend in Rust, C++, or another native language. If you’re planning to build a primarily native application that also wants to display a little website in a specific view, you might be better off using the OS-provided web view or something like ultralight.

---

## Glossary

**URL:** https://www.electronjs.org/docs/latest/glossary

**Contents:**
- Glossary
  - ASAR​
  - ASAR integrity​
  - code signing​
  - context isolation​
  - CRT​
  - DMG​
  - IME​
  - IDL​
  - IPC​

This page defines some terminology that is commonly used in Electron development.

ASAR stands for Atom Shell Archive Format. An asar archive is a simple tar-like format that concatenates files into a single file. Electron can read arbitrary files from it without unpacking the whole file.

The ASAR format was created primarily to improve performance on Windows when reading large quantities of small files (e.g. when loading your app's JavaScript dependency tree from node_modules).

ASAR integrity is an security feature that validates the contents of your app's ASAR archives at runtime. When enabled, your Electron app will verify the header hash of its ASAR archive on runtime. If no hash is present or if there is a mismatch in the hashes, the app will forcefully terminate.

See the ASAR Integrity guide for more details.

Code signing is a process where an app developer digitally signs their code to ensure that it hasn't been tampered with after packaging. Both Windows and macOS implement their own version of code signing. As a desktop app developer, it's important that you sign your code if you plan on distributing it to the general public.

For more information, read the Code Signing tutorial.

Context isolation is a security measure in Electron that ensures that your preload script cannot leak privileged Electron or Node.js APIs to the web contents in your renderer process. With context isolation enabled, the only way to expose APIs from your preload script is through the contextBridge API.

For more information, read the Context Isolation tutorial.

See also: preload script, renderer process

The C Runtime Library (CRT) is the part of the C++ Standard Library that incorporates the ISO C99 standard library. The Visual C++ libraries that implement the CRT support native code development, and both mixed native and managed code, and pure managed code for .NET development.

An Apple Disk Image is a packaging format used by macOS. DMG files are commonly used for distributing application "installers".

Input Method Editor. A program that allows users to enter characters and symbols not found on their keyboard. For example, this allows users of Latin keyboards to input Chinese, Japanese, Korean and Indic characters.

Interface description language. Write function signatures and data types in a format that can be used to generate interfaces in Java, C++, JavaScript, etc.

IPC stands for inter-process communication. Electron uses IPC to send serialized JSON messages between the main and renderer processes.

see also: main process, renderer process

The main process, commonly a file named main.js, is the entry point to every Electron app. It controls the life of the app, from open to close. It also manages native elements such as the Menu, Menu Bar, Dock, Tray, etc. The main process is responsible for creating each new renderer process in the app. The full Node API is built in.

Every app's main process file is specified in the main property in package.json. This is how electron . knows what file to execute at startup.

In Chromium, this process is referred to as the "browser process". It is renamed in Electron to avoid confusion with renderer processes.

See also: process, renderer process

Acronym for Apple's Mac App Store. For details on submitting your app to the MAS, see the Mac App Store Submission Guide.

An IPC system for communicating intra- or inter-process, and that's important because Chrome is keen on being able to split its work into separate processes or not, depending on memory pressures etc.

See https://chromium.googlesource.com/chromium/src/+/main/mojo/README.md

On Windows, MSI packages are used by the Windows Installer (also known as Microsoft Installer) service to install and configure applications.

More information can be found in Microsoft's documentation.

Native modules (also called addons in Node.js) are modules written in C or C++ that can be loaded into Node.js or Electron using the require() function, and used as if they were an ordinary Node.js module. They are used primarily to provide an interface between JavaScript running in Node.js and C/C++ libraries.

Native Node modules are supported by Electron, but since Electron is very likely to use a different V8 version from the Node binary installed in your system, you have to manually specify the location of Electron’s headers when building native modules.

For more information, read the Native Node Modules tutorial.

Notarization is a macOS-specific process where a developer can send a code-signed app to Apple servers to get verified for malicious components through an automated service.

See also: code signing

OSR (offscreen rendering) can be used for loading heavy page in background and then displaying it after (it will be much faster). It allows you to render page without showing it on screen.

For more information, read the Offscreen Rendering tutorial.

Preload scripts contain code that executes in a renderer process before its web contents begin loading. These scripts run within the renderer context, but are granted more privileges by having access to Node.js APIs.

See also: renderer process, context isolation

A process is an instance of a computer program that is being executed. Electron apps that make use of the main and one or many renderer process are actually running several programs simultaneously.

In Node.js and Electron, each running process has a process object. This object is a global that provides information about, and control over, the current process. As a global, it is always available to applications without using require().

See also: main process, renderer process

The renderer process is a browser window in your app. Unlike the main process, there can be multiple of these and each is run in a separate process. They can also be hidden.

See also: process, main process

The sandbox is a security feature inherited from Chromium that restricts your renderer processes to a limited set of permissions.

For more information, read the Process Sandboxing tutorial.

Squirrel is an open-source framework that enables Electron apps to update automatically as new versions are released. See the autoUpdater API for info about getting started with Squirrel.

This term originated in the Unix community, where "userland" or "userspace" referred to programs that run outside of the operating system kernel. More recently, the term has been popularized in the Node and npm community to distinguish between the features available in "Node core" versus packages published to the npm registry by the much larger "user" community.

Like Node, Electron is focused on having a small set of APIs that provide all the necessary primitives for developing multi-platform desktop applications. This design philosophy allows Electron to remain a flexible tool without being overly prescriptive about how it should be used. Userland enables users to create and share tools that provide additional functionality on top of what is available in "core".

The utility process is a child of the main process that allows running any untrusted services that cannot be run in the main process. Chromium uses this process to perform network I/O, audio/video processing, device inputs etc. In Electron, you can create this process using UtilityProcess API.

See also: process, main process

V8 is Google's open source JavaScript engine. It is written in C++ and is used in Google Chrome. V8 can run standalone, or can be embedded into any C++ application.

Electron builds V8 as part of Chromium and then points Node to that V8 when building it.

V8's version numbers always correspond to those of Google Chrome. Chrome 59 includes V8 5.9, Chrome 58 includes V8 5.8, etc.

webview tags are used to embed 'guest' content (such as external web pages) in your Electron app. They are similar to iframes, but differ in that each webview runs in a separate process. It doesn't have the same permissions as your web page and all interactions between your app and embedded content will be asynchronous. This keeps your app safe from the embedded content.

---

## Breaking Changes

**URL:** https://www.electronjs.org/docs/latest/breaking-changes

**Contents:**
- Breaking Changes
  - Types of Breaking Changes​
- Planned Breaking API Changes (39.0)​
  - Deprecated: --host-rules command line switch​
  - Behavior Changed: window.open popups are always resizable​
  - Behavior Changed: shared texture OSR paint event data structure​
- Planned Breaking API Changes (38.0)​
  - Removed: ELECTRON_OZONE_PLATFORM_HINT environment variable​
  - Removed: ORIGINAL_XDG_CURRENT_DESKTOP environment variable​
  - Removed: macOS 11 support​

Breaking changes will be documented here, and deprecation warnings added to JS code where possible, at least one major version before the change is made.

This document uses the following convention to categorize breaking changes:

Chromium is deprecating the --host-rules switch.

You should use --host-resolver-rules instead.

Per current WHATWG spec, the window.open API will now always create a resizable popup window.

To restore previous behavior:

When using shared texture offscreen rendering feature, the paint event now emits a more structured object. It moves the sharedTextureHandle, planes, modifier into a unified handle property. See the OffscreenSharedTexture API structure for more details.

The default value of the --ozone-platform flag changed to auto.

Electron now defaults to running as a native Wayland app when launched in a Wayland session (when XDG_SESSION_TYPE=wayland). Users can force XWayland by passing --ozone-platform=x11.

Previously, Electron changed the value of XDG_CURRENT_DESKTOP internally to Unity, and stored the original name of the desktop session in a separate variable. XDG_CURRENT_DESKTOP is no longer overriden and now reflects the actual desktop environment.

macOS 11 (Big Sur) is no longer supported by Chromium.

Older versions of Electron will continue to run on Big Sur, but macOS 12 (Monterey) or later will be required to run Electron v38.0.0 and higher.

The plugin-crashed event has been removed from webContents.

The routingId property will be removed from webFrame objects.

You should use webFrame.frameToken instead.

The webFrame.findFrameByRoutingId(routingId) function will be removed.

You should use webFrame.findFrameByToken(frameToken) instead.

Utility Processes will now warn with an error message when an unhandled rejection occurs instead of crashing the process.

To restore the previous behavior, you can use:

Calling process.exit() in a utility process will now kill the utility process synchronously. This brings the behavior of process.exit() in line with Node.js behavior.

Please refer to the Node.js docs and PR #45690 to understand the potential implications of that, e.g., when calling console.log() before process.exit().

WebUSB and Web Serial now support the WebUSB Blocklist and Web Serial Blocklist used by Chromium and outlined in their respective specifications.

To disable these, users can pass disable-usb-blocklist and disable-serial-blocklist as command line flags.

This deprecated feature has been removed.

Previously, setting the ProtocolResponse.session property to null would create a random independent session. This is no longer supported.

Using single-purpose sessions here is discouraged due to overhead costs; however, old code that needs to preserve this behavior can emulate it by creating a random session with session.fromPartition(some_random_string) and then using it in ProtocolResponse.session.

BrowserWindow.IsVisibleOnAllWorkspaces() will now return false on Linux if the window is not currently visible.

app.commandLine will convert upper-cases switches and arguments to lowercase.

app.commandLine was only meant to handle chromium switches (which aren't case-sensitive) and switches passed via app.commandLine will not be passed down to any of the child processes.

If you were using app.commandLine to control the behavior of the main process, you should do this via process.argv.

NativeImage.toBitmap() returns a newly-allocated copy of the bitmap. NativeImage.getBitmap() was originally an alternative function that returned the original instead of a copy. This changed when sandboxing was introduced, so both return a copy and are functionally equivalent.

Client code should call NativeImage.toBitmap() instead:

These properties have been removed from the PrinterInfo Object because they have been removed from upstream Chromium.

When calling Session.clearStorageData(options), the options.quota type syncable is no longer supported because it has been removed from upstream Chromium.

Previously, setting the ProtocolResponse.session property to null Would create a random independent session. This is no longer supported.

Using single-purpose sessions here is discouraged due to overhead costs; however, old code that needs to preserve this behavior can emulate it by creating a random session with session.fromPartition(some_random_string) and then using it in ProtocolResponse.session.

When calling Session.clearStorageData(options), the options.quota property is deprecated. Since the syncable type was removed, there is only type left -- 'temporary' -- so specifying it is unnecessary.

session.loadExtension, session.removeExtension, session.getExtension, session.getAllExtensions, 'extension-loaded' event, 'extension-unloaded' event, and 'extension-ready' events have all moved to the new session.extensions class.

The systemPreferences.isAeroGlassEnabled() function has been removed without replacement. It has been always returning true since Electron 23, which only supports Windows 10+, where DWM composition can no longer be disabled.

https://learn.microsoft.com/en-us/windows/win32/dwm/composition-ovw#disabling-dwm-composition-windows7-and-earlier

After an upstream change, GTK 4 is now the default when running GNOME.

In rare cases, this may cause some applications or configurations to error with the following message:

Affected users can work around this by specifying the gtk-version command-line flag:

The same can be done with the app.commandLine.appendSwitch function.

On Linux, the required portal version for file dialogs has been reverted to 3 from 4. Using the defaultPath option of the Dialog API is not supported when using portal file chooser dialogs unless the portal backend is version 4 or higher. The --xdg-portal-required-version command-line switch can be used to force a required version for your application. See #44426 for more details.

The session.serviceWorkers.fromVersionID(versionId) API has been deprecated in favor of session.serviceWorkers.getInfoFromVersionID(versionId). This was changed to make it more clear which object is returned with the introduction of the session.serviceWorkers.getWorkerFromVersionID(versionId) API.

registerPreloadScript, unregisterPreloadScript, and getPreloadScripts are introduced as a replacement for the deprecated methods. These new APIs allow third-party libraries to register preload scripts without replacing existing scripts. Also, the new type option allows for additional preload targets beyond frame.

The console-message event on WebContents has been updated to provide details on the Event argument.

Additionally, level is now a string with possible values of info, warning, error, and debug.

Previously, an empty urls array was interpreted as including all URLs. To explicitly include all URLs, developers should now use the <all_urls> pattern, which is a designated URL pattern that matches every possible URL. This change clarifies the intent and ensures more predictable behavior.

The systemPreferences.isAeroGlassEnabled() function has been deprecated without replacement. It has been always returning true since Electron 23, which only supports Windows 10+, where DWM composition can no longer be disabled.

https://learn.microsoft.com/en-us/windows/win32/dwm/composition-ovw#disabling-dwm-composition-windows7-and-earlier

This brings the behavior to parity with Linux. Prior behavior: Menu bar is still visible during fullscreen on Windows. New behavior: Menu bar is hidden during fullscreen on Windows.

Correction: This was previously listed as a breaking change in Electron 33, but was first released in Electron 34.

The synchronous clipboard read API document.execCommand("paste") has been deprecated in favor of async clipboard API. This is to align with the browser defaults.

The enableDeprecatedPaste option on WebPreferences that triggers the permission checks for this API and the associated permission type deprecated-sync-clipboard-read are also deprecated.

APIs which provide access to a WebFrameMain instance may return an instance with frame.detached set to true, or possibly return null.

When a frame performs a cross-origin navigation, it enters into a detached state in which it's no longer attached to the page. In this state, it may be running unload handlers prior to being deleted. In the event of an IPC sent during this state, frame.detached will be set to true with the frame being destroyed shortly thereafter.

When receiving an event, it's important to access WebFrameMain properties immediately upon being received. Otherwise, it's not guaranteed to point to the same webpage as when received. To avoid misaligned expectations, Electron will return null in the case of late access where the webpage has changed.

Due to changes made in Chromium to support Non-Special Scheme URLs, custom protocol URLs that use Windows file paths will no longer work correctly with the deprecated protocol.registerFileProtocol and the baseURLForDataURL property on BrowserWindow.loadURL, WebContents.loadURL, and <webview>.loadURL. protocol.handle will also not work with these types of URLs but this is not a change since it has always worked that way.

The webContents property in the login event from app will be null when the event is triggered for requests from the utility process created with respondToAuthRequestsFromMainProcess option.

The textured option of type in BrowserWindowConstructorOptions has been deprecated with no replacement. This option relied on the NSWindowStyleMaskTexturedBackground style mask on macOS, which has been deprecated with no alternative.

macOS 10.15 (Catalina) is no longer supported by Chromium.

Older versions of Electron will continue to run on Catalina, but macOS 11 (Big Sur) or later will be required to run Electron v33.0.0 and higher.

Due to changes made upstream, both V8 and Node.js now require C++20 as a minimum version. Developers using native node modules should build their modules with --std=c++20 rather than --std=c++17. Images using gcc9 or lower may need to update to gcc10 in order to compile. See #43555 for more details.

The systemPreferences.accessibilityDisplayShouldReduceTransparency property is now deprecated in favor of the new nativeTheme.prefersReducedTransparency, which provides identical information and works cross-platform.

The nonstandard path property of the Web File object was added in an early version of Electron as a convenience method for working with native files when doing everything in the renderer was more common. However, it represents a deviation from the standard and poses a minor security risk as well, so beginning in Electron 32.0 it has been removed in favor of the webUtils.getPathForFile method.

The navigation-related APIs are now deprecated.

These APIs have been moved to the navigationHistory property of WebContents to provide a more structured and intuitive interface for managing navigation history.

If you have a directory called databases in the directory returned by app.getPath('userData'), it will be deleted when Electron 32 is first run. The databases directory was used by WebSQL, which was removed in Electron 31. Chromium now performs a cleanup that deletes this directory. See issue #45396.

Chromium has removed support for WebSQL upstream, transitioning it to Android only. See Chromium's intent to remove discussion for more information.

PNG decoder implementation has been changed to preserve colorspace data, the encoded data returned from this function now matches it.

See crbug.com/332584706 for more information.

This brings the behavior to parity with Windows and Linux. Prior behavior: The first flashFrame(true) bounces the dock icon only once (using the NSInformationalRequest level) and flashFrame(false) does nothing. New behavior: Flash continuously until flashFrame(false) is called. This uses the NSCriticalRequest level instead. To explicitly use NSInformationalRequest to cause a single dock icon bounce, it is still possible to use dock.bounce('informational').

Cross-origin iframes must now specify features available to a given iframe via the allow attribute in order to access them.

See documentation for more information.

This switch was never formally documented but it's removal is being noted here regardless. Chromium itself now has better support for color spaces so this flag should not be needed.

In Electron 30, BrowserView is now a wrapper around the new WebContentsView API.

Previously, the setAutoResize function of the BrowserView API was backed by autoresizing on macOS, and by a custom algorithm on Windows and Linux. For simple use cases such as making a BrowserView fill the entire window, the behavior of these two approaches was identical. However, in more advanced cases, BrowserViews would be autoresized differently on macOS than they would be on other platforms, as the custom resizing algorithm for Windows and Linux did not perfectly match the behavior of macOS's autoresizing API. The autoresizing behavior is now standardized across all platforms.

If your app uses BrowserView.setAutoResize to do anything more complex than making a BrowserView fill the entire window, it's likely you already had custom logic in place to handle this difference in behavior on macOS. If so, that logic will no longer be needed in Electron 30 as autoresizing behavior is consistent.

The BrowserView class has been deprecated and replaced by the new WebContentsView class.

BrowserView related methods in BrowserWindow have also been deprecated:

The inputFormType property of the params object in the context-menu event from WebContents has been removed. Use the new formControlType property instead.

Chromium has removed access to this information.

Attempting to send the entire ipcRenderer module as an object over the contextBridge will now result in an empty object on the receiving side of the bridge. This change was made to remove / mitigate a security footgun. You should not directly expose ipcRenderer or its methods over the bridge. Instead, provide a safe wrapper like below:

The renderer-process-crashed event on app has been removed. Use the new render-process-gone event instead.

The crashed events on WebContents and <webview> have been removed. Use the new render-process-gone event instead.

The gpu-process-crashed event on app has been removed. Use the new child-process-gone event instead.

WebContents.backgroundThrottling set to false will disable frames throttling in the BrowserWindow for all WebContents displayed by it.

BrowserWindow.setTrafficLightPosition(position) has been removed, the BrowserWindow.setWindowButtonPosition(position) API should be used instead which accepts null instead of { x: 0, y: 0 } to reset the position to system default.

BrowserWindow.getTrafficLightPosition() has been removed, the BrowserWindow.getWindowButtonPosition() API should be used instead which returns null instead of { x: 0, y: 0 } when there is no custom position.

The ipcRenderer.sendTo() API has been removed. It should be replaced by setting up a MessageChannel between the renderers.

The senderId and senderIsMainFrame properties of IpcRendererEvent have been removed as well.

The app.runningUnderRosettaTranslation property has been removed. Use app.runningUnderARM64Translation instead.

The renderer-process-crashed event on app has been deprecated. Use the new render-process-gone event instead.

The inputFormType property of the params object in the context-menu event from WebContents has been deprecated. Use the new formControlType property instead.

The crashed events on WebContents and <webview> have been deprecated. Use the new render-process-gone event instead.

The gpu-process-crashed event on app has been deprecated. Use the new child-process-gone event instead.

macOS 10.13 (High Sierra) and macOS 10.14 (Mojave) are no longer supported by Chromium.

Older versions of Electron will continue to run on these operating systems, but macOS 10.15 (Catalina) or later will be required to run Electron v27.0.0 and higher.

The ipcRenderer.sendTo() API has been deprecated. It should be replaced by setting up a MessageChannel between the renderers.

The senderId and senderIsMainFrame properties of IpcRendererEvent have been deprecated as well.

The following systemPreferences events have been removed:

Use the new updated event on the nativeTheme module instead.

The following vibrancy options have been removed:

These were previously deprecated and have been removed by Apple in 10.15.

The webContents.getPrinters method has been removed. Use webContents.getPrintersAsync instead.

The systemPreferences.getAppLevelAppearance and systemPreferences.setAppLevelAppearance methods have been removed, as well as the systemPreferences.appLevelAppearance property. Use the nativeTheme module instead.

The alternate-selected-control-text value for systemPreferences.getColor has been removed. Use selected-content-background instead.

The webContents.getPrinters method has been deprecated. Use webContents.getPrintersAsync instead.

The systemPreferences.getAppLevelAppearance and systemPreferences.setAppLevelAppearance methods have been deprecated, as well as the systemPreferences.appLevelAppearance property. Use the nativeTheme module instead.

The alternate-selected-control-text value for systemPreferences.getColor has been deprecated. Use selected-content-background instead.

The protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle.

The new method can either register a new protocol or intercept an existing protocol, and responses can be of any type.

BrowserWindow.setTrafficLightPosition(position) has been deprecated, the BrowserWindow.setWindowButtonPosition(position) API should be used instead which accepts null instead of { x: 0, y: 0 } to reset the position to system default.

BrowserWindow.getTrafficLightPosition() has been deprecated, the BrowserWindow.getWindowButtonPosition() API should be used instead which returns null instead of { x: 0, y: 0 } when there is no custom position.

The maxSize parameter has been changed to size to reflect that the size passed in will be the size the thumbnail created. Previously, Windows would not scale the image up if it were smaller than maxSize, and macOS would always set the size to maxSize. Behavior is now the same across platforms.

Previous Behavior (on Windows):

The implementation of draggable regions (using the CSS property -webkit-app-region: drag) has changed on macOS to bring it in line with Windows and Linux. Previously, when a region with -webkit-app-region: no-drag overlapped a region with -webkit-app-region: drag, the no-drag region would always take precedence on macOS, regardless of CSS layering. That is, if a drag region was above a no-drag region, it would be ignored. Beginning in Electron 23, a drag region on top of a no-drag region will correctly cause the region to be draggable.

Additionally, the customButtonsOnHover BrowserWindow property previously created a draggable region which ignored the -webkit-app-region CSS property. This has now been fixed (see #37210 for discussion).

As a result, if your app uses a frameless window with draggable regions on macOS, the regions which are draggable in your app may change in Electron 23.

Windows 7, Windows 8, and Windows 8.1 are no longer supported. Electron follows the planned Chromium deprecation policy, which will deprecate Windows 7 support beginning in Chromium 109.

Older versions of Electron will continue to run on these operating systems, but Windows 10 or later will be required to run Electron v23.0.0 and higher.

The deprecated scroll-touch-begin, scroll-touch-end and scroll-touch-edge events on BrowserWindow have been removed. Instead, use the newly available input-event event on WebContents.

The webContents.incrementCapturerCount(stayHidden, stayAwake) function has been removed. It is now automatically handled by webContents.capturePage when a page capture completes.

The webContents.decrementCapturerCount(stayHidden, stayAwake) function has been removed. It is now automatically handled by webContents.capturePage when a page capture completes.

webContents.incrementCapturerCount(stayHidden, stayAwake) has been deprecated. It is now automatically handled by webContents.capturePage when a page capture completes.

webContents.decrementCapturerCount(stayHidden, stayAwake) has been deprecated. It is now automatically handled by webContents.capturePage when a page capture completes.

The new-window event of WebContents has been removed. It is replaced by webContents.setWindowOpenHandler().

The new-window event of <webview> has been removed. There is no direct replacement.

The scroll-touch-begin, scroll-touch-end and scroll-touch-edge events on BrowserWindow are deprecated. Instead, use the newly available input-event event on WebContents.

The V8 memory cage has been enabled, which has implications for native modules which wrap non-V8 memory with ArrayBuffer or Buffer. See the blog post about the V8 memory cage for more details.

webContents.printToPDF() has been modified to conform to Page.printToPDF in the Chrome DevTools Protocol. This has been changes in order to address changes upstream that made our previous implementation untenable and rife with bugs.

macOS 10.11 (El Capitan) and macOS 10.12 (Sierra) are no longer supported by Chromium.

Older versions of Electron will continue to run on these operating systems, but macOS 10.13 (High Sierra) or later will be required to run Electron v20.0.0 and higher.

Previously, renderers that specified a preload script defaulted to being unsandboxed. This meant that by default, preload scripts had access to Node.js. In Electron 20, this default has changed. Beginning in Electron 20, renderers will be sandboxed by default, unless nodeIntegration: true or sandbox: false is specified.

If your preload scripts do not depend on Node, no action is needed. If your preload scripts do depend on Node, either refactor them to remove Node usage from the renderer, or explicitly specify sandbox: false for the relevant renderers.

On X11, skipTaskbar sends a _NET_WM_STATE_SKIP_TASKBAR message to the X11 window manager. There is not a direct equivalent for Wayland, and the known workarounds have unacceptable tradeoffs (e.g. Window.is_skip_taskbar in GNOME requires unsafe mode), so Electron is unable to support this feature on Linux.

The handler invoked when session.setDevicePermissionHandler(handler) is used has a change to its arguments. This handler no longer is passed a frame WebFrameMain, but instead is passed the origin, which is the origin that is checking for device permission.

This is a result of Chromium 102.0.4999.0 dropping support for IA32 Linux. This concludes the removal of support for IA32 Linux.

Prior to Electron 15, window.open was by default shimmed to use BrowserWindowProxy. This meant that window.open('about:blank') did not work to open synchronously scriptable child windows, among other incompatibilities. Since Electron 15, nativeWindowOpen has been enabled by default.

See the documentation for window.open in Electron for more details.

The desktopCapturer.getSources API is now only available in the main process. This has been changed in order to improve the default security of Electron apps.

If you need this functionality, it can be replaced as follows:

However, you should consider further restricting the information returned to the renderer; for instance, displaying a source selector to the user and only returning the selected source.

Prior to Electron 15, window.open was by default shimmed to use BrowserWindowProxy. This meant that window.open('about:blank') did not work to open synchronously scriptable child windows, among other incompatibilities. Since Electron 15, nativeWindowOpen has been enabled by default.

See the documentation for window.open in Electron for more details.

The underlying implementation of the crashReporter API on Linux has changed from Breakpad to Crashpad, bringing it in line with Windows and Mac. As a result of this, child processes are now automatically monitored, and calling process.crashReporter.start in Node child processes is no longer needed (and is not advisable, as it will start a second instance of the Crashpad reporter).

There are also some subtle changes to how annotations will be reported on Linux, including that long values will no longer be split between annotations appended with __1, __2 and so on, and instead will be truncated at the (new, longer) annotation value limit.

Usage of the desktopCapturer.getSources API in the renderer has been deprecated and will be removed. This change improves the default security of Electron apps.

See here for details on how to replace this API in your app.

Prior to Electron 15, window.open was by default shimmed to use BrowserWindowProxy. This meant that window.open('about:blank') did not work to open synchronously scriptable child windows, among other incompatibilities. nativeWindowOpen is no longer experimental, and is now the default.

See the documentation for window.open in Electron for more details.

The app.runningUnderRosettaTranslation property has been deprecated. Use app.runningUnderARM64Translation instead.

The remote module was deprecated in Electron 12, and will be removed in Electron 14. It is replaced by the @electron/remote module.

The app.allowRendererProcessReuse property will be removed as part of our plan to more closely align with Chromium's process model for security, performance and maintainability.

For more detailed information see #18397.

The affinity option when constructing a new BrowserWindow will be removed as part of our plan to more closely align with Chromium's process model for security, performance and maintainability.

For more detailed information see #18397.

The optional parameter frameName will no longer set the title of the window. This now follows the specification described by the native documentation under the corresponding parameter windowName.

If you were using this parameter to set the title of a window, you can instead use win.setTitle(title).

In Electron 14, worldSafeExecuteJavaScript will be removed. There is no alternative, please ensure your code works with this property enabled. It has been enabled by default since Electron 12.

You will be affected by this change if you use either webFrame.executeJavaScript or webFrame.executeJavaScriptInIsolatedWorld. You will need to ensure that values returned by either of those methods are supported by the Context Bridge API as these methods use the same value passing semantics.

Prior to Electron 14, windows opened with window.open would inherit BrowserWindow constructor options such as transparent and resizable from their parent window. Beginning with Electron 14, this behavior is removed, and windows will not inherit any BrowserWindow constructor options from their parents.

Instead, explicitly set options for the new window with setWindowOpenHandler:

The deprecated additionalFeatures property in the new-window and did-create-window events of WebContents has been removed. Since new-window uses positional arguments, the argument is still present, but will always be the empty array []. (Though note, the new-window event itself is deprecated, and is replaced by setWindowOpenHandler.) Bare keys in window features will now present as keys with the value true in the options object.

The handler methods first parameter was previously always a webContents, it can now sometimes be null. You should use the requestingOrigin, embeddingOrigin and securityOrigin properties to respond to the permission check correctly. As the webContents can be null it can no longer be relied on.

The deprecated synchronous shell.moveItemToTrash() API has been removed. Use the asynchronous shell.trashItem() instead.

The deprecated extension APIs have been removed:

Use the session APIs instead:

The following systemPreferences methods have been deprecated:

Use the following nativeTheme properties instead:

The new-window event of WebContents has been deprecated. It is replaced by webContents.setWindowOpenHandler().

Chromium has removed support for Flash, and so we must follow suit. See Chromium's Flash Roadmap for more details.

In Electron 12, worldSafeExecuteJavaScript will be enabled by default. To restore the previous behavior, worldSafeExecuteJavaScript: false must be specified in WebPreferences. Please note that setting this option to false is insecure.

This option will be removed in Electron 14 so please migrate your code to support the default value.

In Electron 12, contextIsolation will be enabled by default. To restore the previous behavior, contextIsolation: false must be specified in WebPreferences.

We recommend having contextIsolation enabled for the security of your application.

Another implication is that require() cannot be used in the renderer process unless nodeIntegration is true and contextIsolation is false.

For more details see: https://github.com/electron/electron/issues/23506

The crashReporter.getCrashesDirectory method has been removed. Usage should be replaced by app.getPath('crashDumps').

The following crashReporter methods are no longer available in the renderer process:

They should be called only from the main process.

See #23265 for more details.

The default value of the compress option to crashReporter.start has changed from false to true. This means that crash dumps will be uploaded to the crash ingestion server with the Content-Encoding: gzip header, and the body will be compressed.

If your crash ingestion server does not support compressed payloads, you can turn off compression by specifying { compress: false } in the crash reporter options.

The remote module is deprecated in Electron 12, and will be removed in Electron 14. It is replaced by the @electron/remote module.

The synchronous shell.moveItemToTrash() has been replaced by the new, asynchronous shell.trashItem().

The experimental APIs BrowserView.{destroy, fromId, fromWebContents, getAllViews} have now been removed. Additionally, the id property of BrowserView has also been removed.

For more detailed information, see #23578.

The companyName argument to crashReporter.start(), which was previously required, is now optional, and further, is deprecated. To get the same behavior in a non-deprecated way, you can pass a companyName value in globalExtra.

The crashReporter.getCrashesDirectory method has been deprecated. Usage should be replaced by app.getPath('crashDumps').

Calling the following crashReporter methods from the renderer process is deprecated:

The only non-deprecated methods remaining in the crashReporter module in the renderer are addExtraParameter, removeExtraParameter and getParameters.

All above methods remain non-deprecated when called from the main process.

See #23265 for more details.

Setting { compress: false } in crashReporter.start is deprecated. Nearly all crash ingestion servers support gzip compression. This option will be removed in a future version of Electron.

In Electron 9, using the remote module without explicitly enabling it via the enableRemoteModule WebPreferences option began emitting a warning. In Electron 10, the remote module is now disabled by default. To use the remote module, enableRemoteModule: true must be specified in WebPreferences:

We recommend moving away from the remote module.

The APIs are now synchronous and the optional callback is no longer needed.

The APIs are now synchronous and the optional callback is no longer needed.

The registered or intercepted protocol does not have effect on current page until navigation happens.

This API is deprecated and users should use protocol.isProtocolRegistered and protocol.isProtocolIntercepted instead.

As of Electron 9 we do not allow loading of non-context-aware native modules in the renderer process. This is to improve security, performance and maintainability of Electron as a project.

If this impacts you, you can temporarily set app.allowRendererProcessReuse to false to revert to the old behavior. This flag will only be an option until Electron 11 so you should plan to update your native modules to be context aware.

For more detailed information see #18397.

The following extension APIs have been deprecated:

Use the session APIs instead:

This API, which was deprecated in Electron 8.0, is now removed.

Chromium has removed support for changing the layout zoom level limits, and it is beyond Electron's capacity to maintain it. The function was deprecated in Electron 8.x, and has been removed in Electron 9.x. The layout zoom level limits are now fixed at a minimum of 0.25 and a maximum of 5.0, as defined here.

In Electron 8.0, IPC was changed to use the Structured Clone Algorithm, bringing significant performance improvements. To help ease the transition, the old IPC serialization algorithm was kept and used for some objects that aren't serializable with Structured Clone. In particular, DOM objects (e.g. Element, Location and DOMMatrix), Node.js objects backed by C++ classes (e.g. process.env, some members of Stream), and Electron objects backed by C++ classes (e.g. WebContents, BrowserWindow and WebFrame) are not serializable with Structured Clone. Whenever the old algorithm was invoked, a deprecation warning was printed.

In Electron 9.0, the old serialization algorithm has been removed, and sending such non-serializable objects will now throw an "object could not be cloned" error.

The shell.openItem API has been replaced with an asynchronous shell.openPath API. You can see the original API proposal and reasoning here.

The algorithm used to serialize objects sent over IPC (through ipcRenderer.send, ipcRenderer.sendSync, WebContents.send and related methods) has been switched from a custom algorithm to V8's built-in Structured Clone Algorithm, the same algorithm used to serialize messages for postMessage. This brings about a 2x performance improvement for large messages, but also brings some breaking changes in behavior.

Sending any objects that aren't native JS types, such as DOM objects (e.g. Element, Location, DOMMatrix), Node.js objects (e.g. process.env, Stream), or Electron objects (e.g. WebContents, BrowserWindow, WebFrame) is deprecated. In Electron 8, these objects will be serialized as before with a DeprecationWarning message, but starting in Electron 9, sending these kinds of objects will throw a 'could not be cloned' error.

This API is implemented using the remote module, which has both performance and security implications. Therefore its usage should be explicit.

However, it is recommended to avoid using the remote module altogether.

Chromium has removed support for changing the layout zoom level limits, and it is beyond Electron's capacity to maintain it. The function will emit a warning in Electron 8.x, and cease to exist in Electron 9.x. The layout zoom level limits are now fixed at a minimum of 0.25 and a maximum of 5.0, as defined here.

The following systemPreferences events have been deprecated:

Use the new updated event on the nativeTheme module instead.

The following systemPreferences methods have been deprecated:

Use the following nativeTheme properties instead:

This is the URL specified as disturl in a .npmrc file or as the --dist-url command line flag when building native Node modules. Both will be supported for the foreseeable future but it is recommended that you switch.

Deprecated: https://atom.io/download/electron

Replace with: https://electronjs.org/headers

The session.clearAuthCache API no longer accepts options for what to clear, and instead unconditionally clears the whole cache.

This property was removed in Chromium 77, and as such is no longer available.

The webkitdirectory property on HTML file inputs allows them to select folders. Previous versions of Electron had an incorrect implementation where the event.target.files of the input returned a FileList that returned one File corresponding to the selected folder.

As of Electron 7, that FileList is now list of all files contained within the folder, similarly to Chrome, Firefox, and Edge (link to MDN docs).

As an illustration, take a folder with this structure:

In Electron <=6, this would return a FileList with a File object for:

In Electron 7, this now returns a FileList with a File object for:

Note that webkitdirectory no longer exposes the path to the selected folder. If you require the path to the selected folder rather than the folder contents, see the dialog.showOpenDialog API (link).

Electron 5 and Electron 6 introduced Promise-based versions of existing asynchronous APIs and deprecated their older, callback-based counterparts. In Electron 7, all deprecated callback-based APIs are now removed.

These functions now only return Promises:

These functions now have two forms, synchronous and Promise-based asynchronous:

Mixed-sandbox mode is now enabled by default.

Under macOS Catalina our former Tray implementation breaks. Apple's native substitute doesn't support changing the highlighting behavior.

The following webPreferences option default values are deprecated in favor of the new defaults listed below.

E.g. Re-enabling the webviewTag

Child windows opened with the nativeWindowOpen option will always have Node.js integration disabled, unless nodeIntegrationInSubFrames is true.

Renderer process APIs webFrame.registerURLSchemeAsPrivileged and webFrame.registerURLSchemeAsBypassingCSP as well as browser process API protocol.registerStandardSchemes have been removed. A new API, protocol.registerSchemesAsPrivileged has been added and should be used for registering custom schemes with the required privileges. Custom schemes are required to be registered before app ready.

The spellCheck callback is now asynchronous, and autoCorrectWord parameter has been removed.

webContents.getZoomLevel and webContents.getZoomFactor no longer take callback parameters, instead directly returning their number values.

The following list includes the breaking API changes made in Electron 4.0.

When building native modules for windows, the win_delay_load_hook variable in the module's binding.gyp must be true (which is the default). If this hook is not present, then the native module will fail to load on Windows, with an error message like Cannot find module. See the native module guide for more.

Electron 18 will no longer run on 32-bit Linux systems. See discontinuing support for 32-bit Linux for more information.

The following list includes the breaking API changes in Electron 3.0.

This is the URL specified as disturl in a .npmrc file or as the --dist-url command line flag when building native Node modules.

Deprecated: https://atom.io/download/atom-shell

Replace with: https://atom.io/download/electron

The following list includes the breaking API changes made in Electron 2.0.

Each Electron release includes two identical ARM builds with slightly different filenames, like electron-v1.7.3-linux-arm.zip and electron-v1.7.3-linux-armv7l.zip. The asset with the v7l prefix was added to clarify to users which ARM version it supports, and to disambiguate it from future armv6l and arm64 assets that may be produced.

The file without the prefix is still being published to avoid breaking any setups that may be consuming it. Starting at 2.0, the unprefixed file will no longer be published.

For details, see 6986 and 7189.

**Examples:**

Example 1 (css):
```css
webContents.setWindowOpenHandler((details) => {  return {    action: 'allow',    overrideBrowserWindowOptions: {      resizable: details.features.includes('resizable=yes')    }  }})
```

Example 2 (javascript):
```javascript
process.on('unhandledRejection', () => {  process.exit(1)})
```

Example 3 (unknown):
```unknown
// Deprecatedbitmap = image.getBitmap()// Use this insteadbitmap = image.toBitmap()
```

Example 4 (julia):
```julia
Gtk-ERROR **: 11:30:38.382: GTK 2/3 symbols detected. Using GTK 2/3 and GTK 4 in the same process is not supported
```

---

## Electron FAQ

**URL:** https://www.electronjs.org/docs/latest/faq

**Contents:**
- Electron FAQ
- Why am I having trouble installing Electron?​
- How are Electron binaries downloaded?​
- When will Electron upgrade to latest Chromium?​
- When will Electron upgrade to latest Node.js?​
- How to share data between web pages?​
- My app's tray disappeared after a few minutes.​
- I can not use jQuery/RequireJS/Meteor/AngularJS in Electron.​
- require('electron').xxx is undefined.​
- The font looks blurry, what is this and what can I do?​

When running npm install electron, some users occasionally encounter installation errors.

In almost all cases, these errors are the result of network problems and not actual issues with the electron npm package. Errors like ELIFECYCLE, EAI_AGAIN, ECONNRESET, and ETIMEDOUT are all indications of such network problems. The best resolution is to try switching networks, or wait a bit and try installing again.

You can also attempt to download Electron directly from GitHub Releases if installing via npm is failing.

If you need to install Electron through a custom mirror or proxy, see the Advanced Installation documentation for more details.

When you run npm install electron, the Electron binary for the corresponding version is downloaded into your project's node_modules folder via npm's postinstall lifecycle script.

This logic is handled by the @electron/get utility package under the hood.

Every new major version of Electron releases with a Chromium major version upgrade. By releasing every 8 weeks, Electron is able to pull in every other major Chromium release on the very same day that it releases upstream. Security fixes will be backported to stable release channels ahead of time.

See the Electron Releases documentation for more details or releases.electronjs.org to see our Release Status dashboard.

When a new version of Node.js gets released, we usually wait for about a month before upgrading the one in Electron. So we can avoid getting affected by bugs introduced in new Node.js versions, which happens very often.

New features of Node.js are usually brought by V8 upgrades, since Electron is using the V8 shipped by Chrome browser, the shiny new JavaScript feature of a new Node.js version is usually already in Electron.

To share data between web pages (the renderer processes) the simplest way is to use HTML5 APIs which are already available in browsers. Good candidates are Storage API, localStorage, sessionStorage, and IndexedDB.

Alternatively, you can use the IPC primitives that are provided by Electron. To share data between the main and renderer processes, you can use the ipcMain and ipcRenderer modules. To communicate directly between web pages, you can send a MessagePort from one to the other, possibly via the main process using ipcRenderer.postMessage(). Subsequent communication over message ports is direct and does not detour through the main process.

This happens when the variable which is used to store the tray gets garbage collected.

If you encounter this problem, the following articles may prove helpful:

If you want a quick fix, you can make the variables global by changing your code from this:

Due to the Node.js integration of Electron, there are some extra symbols inserted into the DOM like module, exports, require. This causes problems for some libraries since they want to insert the symbols with the same names.

To solve this, you can turn off node integration in Electron:

But if you want to keep the abilities of using Node.js and Electron APIs, you have to rename the symbols in the page before including other libraries:

When using Electron's built-in module you might encounter an error like this:

It is very likely you are using the module in the wrong process. For example electron.app can only be used in the main process, while electron.webFrame is only available in renderer processes.

If sub-pixel anti-aliasing is deactivated, then fonts on LCD screens can look blurry. Example:

Sub-pixel anti-aliasing needs a non-transparent background of the layer containing the font glyphs. (See this issue for more info).

To achieve this goal, set the background in the constructor for BrowserWindow:

The effect is visible only on (some?) LCD screens. Even if you don't see a difference, some of your users may. It is best to always set the background this way, unless you have reasons not to do so.

Notice that just setting the background in the CSS does not have the desired effect.

Electron classes cannot be subclassed with the extends keyword (also known as class inheritance). This feature was never implemented in Electron due to the added complexity it would add to C++/JavaScript interop in Electron's internals.

For more information, see electron/electron#23.

**Examples:**

Example 1 (javascript):
```javascript
const { app, Tray } = require('electron')app.whenReady().then(() => {  const tray = new Tray('/path/to/icon.png')  tray.setTitle('hello world')})
```

Example 2 (javascript):
```javascript
const { app, Tray } = require('electron')let tray = nullapp.whenReady().then(() => {  tray = new Tray('/path/to/icon.png')  tray.setTitle('hello world')})
```

Example 3 (css):
```css
// In the main process.const { BrowserWindow } = require('electron')const win = new BrowserWindow({  webPreferences: {    nodeIntegration: false  }})win.show()
```

Example 4 (html):
```html
<head><script>window.nodeRequire = require;delete window.require;delete window.exports;delete window.module;</script><script type="text/javascript" src="jquery.js"></script></head>
```

---
