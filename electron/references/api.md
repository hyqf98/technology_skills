# Electron - Api

**Pages:** 153

---

## TraceCategoriesAndOptions Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/trace-categories-and-options

**Contents:**
- TraceCategoriesAndOptions Object

---

## Certificate Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/certificate

**Contents:**
- Certificate Object

---

## IpcMainInvokeEvent Object extends Event

**URL:** https://www.electronjs.org/docs/latest/api/structures/ipc-main-invoke-event

**Contents:**
- IpcMainInvokeEvent Object extends Event

---

## UploadData Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/upload-data

**Contents:**
- UploadData Object

---

## CrashReport Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/crash-report

**Contents:**
- CrashReport Object

---

## ThumbarButton Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/thumbar-button

**Contents:**
- ThumbarButton Object

The flags is an array that can include following strings:

---

## Class: Dock

**URL:** https://www.electronjs.org/docs/latest/api/dock

**Contents:**
- Class: Dock
- Class: Dock​
  - Instance Methods​
    - dock.bounce([type]) macOS​
    - dock.cancelBounce(id) macOS​
    - dock.downloadFinished(filePath) macOS​
    - dock.setBadge(text) macOS​
    - dock.getBadge() macOS​
    - dock.hide() macOS​
    - dock.show() macOS​

Control your app in the macOS dock

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

See also: A detailed guide about how to implement Dock menus.

Returns Integer - an ID representing the request.

When critical is passed, the dock icon will bounce until either the application becomes active or the request is canceled.

When informational is passed, the dock icon will bounce for one second. However, the request remains active until either the application becomes active or the request is canceled.

This method can only be used while the app is not focused; when the app is focused it will return -1.

Cancel the bounce of id.

Bounces the Downloads stack if the filePath is inside the Downloads folder.

Sets the string to be displayed in the dock’s badging area.

You need to ensure that your application has the permission to display notifications for this method to work.

Returns string - The badge string of the dock.

Returns Promise<void> - Resolves when the dock icon is shown.

Returns boolean - Whether the dock icon is visible.

Sets the application's dock menu.

Returns Menu | null - The application's dock menu.

Sets the image associated with this dock icon.

---

## inAppPurchase

**URL:** https://www.electronjs.org/docs/latest/api/in-app-purchase

**Contents:**
- inAppPurchase
- Events​
  - Event: 'transactions-updated'​
- Methods​
  - inAppPurchase.purchaseProduct(productID[, opts])​
  - inAppPurchase.getProducts(productIDs)​
  - inAppPurchase.canMakePayments()​
  - inAppPurchase.restoreCompletedTransactions()​
  - inAppPurchase.getReceiptURL()​
  - inAppPurchase.finishAllTransactions()​

In-app purchases on Mac App Store.

The inAppPurchase module emits the following events:

Emitted when one or more transactions have been updated.

The inAppPurchase module has the following methods:

Returns Promise<boolean> - Returns true if the product is valid and added to the payment queue.

You should listen for the transactions-updated event as soon as possible and certainly before you call purchaseProduct.

Returns Promise<Product[]> - Resolves with an array of Product objects.

Retrieves the product descriptions.

Returns boolean - whether a user can make a payment.

Restores finished transactions. This method can be called either to install purchases on additional devices, or to restore purchases for an application that the user deleted and reinstalled.

The payment queue delivers a new transaction for each previously completed transaction that can be restored. Each transaction includes a copy of the original transaction.

Returns string - the path to the receipt.

Completes all pending transactions.

Completes the pending transactions corresponding to the date.

---

## MouseInputEvent Object extends InputEvent

**URL:** https://www.electronjs.org/docs/latest/api/structures/mouse-input-event

**Contents:**
- MouseInputEvent Object extends InputEvent

---

## PrinterInfo Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/printer-info

**Contents:**
- PrinterInfo Object
- Example​

The number represented by status means different things on different platforms: on Windows its potential values can be found here, and on Linux and macOS they can be found here.

Below is an example of some of the additional options that may be set which may be different on each platform.

**Examples:**

Example 1 (json):
```json
{  name: 'Austin_4th_Floor_Printer___C02XK13BJHD4',  displayName: 'Austin 4th Floor Printer @ C02XK13BJHD4',  description: 'TOSHIBA ColorMFP',  options: {    copies: '1',    'device-uri': 'dnssd://Austin%204th%20Floor%20Printer%20%40%20C02XK13BJHD4._ipps._tcp.local./?uuid=71687f1e-1147-3274-6674-22de61b110bd',    finishings: '3',    'job-cancel-after': '10800',    'job-hold-until': 'no-hold',    'job-priority': '50',    'job-sheets': 'none,none',    'marker-change-time': '0',    'number-up': '1',    'printer-commands': 'ReportLevels,PrintSelfTestPage,com.toshiba.ColourProfiles.update,com.toshiba.EFiling.update,com.toshiba.EFiling.checkPassword',    'printer-info': 'Austin 4th Floor Printer @ C02XK13BJHD4',    'printer-is-accepting-jobs': 'true',    'printer-is-shared': 'false',    'printer-is-temporary': 'false',    'printer-location': '',    'printer-make-and-model': 'TOSHIBA ColorMFP',    'printer-state': '3',    'printer-state-change-time': '1573472937',    'printer-state-reasons': 'offline-report,com.toshiba.snmp.failed',    'printer-type': '10531038',    'printer-uri-supported': 'ipp://localhost/printers/Austin_4th_Floor_Printer___C02XK13BJHD4',    system_driverinfo: 'T'  }}
```

---

## PaymentDiscount Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/payment-discount

**Contents:**
- PaymentDiscount Object

---

## TouchBar

**URL:** https://www.electronjs.org/docs/latest/api/touch-bar

**Contents:**
- TouchBar
- Class: TouchBar​
  - new TouchBar(options)​
  - Static Properties​
    - TouchBarButton​
    - TouchBarColorPicker​
    - TouchBarGroup​
    - TouchBarLabel​
    - TouchBarPopover​
    - TouchBarScrubber​

Electron's built-in classes cannot be subclassed in user code. For more information, see the FAQ.

Create TouchBar layouts for native macOS applications

Creates a new touch bar with the specified items. Use BrowserWindow.setTouchBar to add the TouchBar to a window.

The TouchBar API is currently experimental and may change or be removed in future Electron releases.

If you don't have a MacBook with Touch Bar, you can use Touch Bar Simulator to test Touch Bar usage in your app.

A typeof TouchBarButton reference to the TouchBarButton class.

A typeof TouchBarColorPicker reference to the TouchBarColorPicker class.

A typeof TouchBarGroup reference to the TouchBarGroup class.

A typeof TouchBarLabel reference to the TouchBarLabel class.

A typeof TouchBarPopover reference to the TouchBarPopover class.

A typeof TouchBarScrubber reference to the TouchBarScrubber class.

A typeof TouchBarSegmentedControl reference to the TouchBarSegmentedControl class.

A typeof TouchBarSlider reference to the TouchBarSlider class.

A typeof TouchBarSpacer reference to the TouchBarSpacer class.

A typeof TouchBarOtherItemsProxy reference to the TouchBarOtherItemsProxy class.

The following properties are available on instances of TouchBar:

A TouchBarItem that will replace the "esc" button on the touch bar when set. Setting to null restores the default "esc" button. Changing this value immediately updates the escape item in the touch bar.

Below is an example of a simple slot machine touch bar game with a button and some labels.

To run the example above, you'll need to (assuming you've got a terminal open in the directory you want to run the example):

You should then see a new Electron window and the app running in your touch bar (or touch bar emulator).

**Examples:**

Example 1 (javascript):
```javascript
const { app, BrowserWindow, TouchBar } = require('electron')const { TouchBarLabel, TouchBarButton, TouchBarSpacer } = TouchBarlet spinning = false// Reel labelsconst reel1 = new TouchBarLabel({ label: '' })const reel2 = new TouchBarLabel({ label: '' })const reel3 = new TouchBarLabel({ label: '' })// Spin result labelconst result = new TouchBarLabel({ label: '' })// Spin buttonconst spin = new TouchBarButton({  label: '🎰 Spin',  backgroundColor: '#7851A9',  click: () => {    // Ignore clicks if already spinning    if (spinning) {      return    }    spinning = true    result.label = ''    let timeout = 10    const spinLength = 4 * 1000 // 4 seconds    const startTime = Date.now()    const spinReels = () => {      updateReels()      if ((Date.now() - startTime) >= spinLength) {        finishSpin()      } else {        // Slow down a bit on each spin        timeout *= 1.1        setTimeout(spinReels, timeout)      }    }    spinReels()  }})const getRandomValue = () => {  const values = ['🍒', '💎', '7️⃣', '🍊', '🔔', '⭐', '🍇', '🍀']  return values[Math.floor(Math.random() * values.length)]}const updateReels = () => {  reel1.label = getRandomValue()  reel2.label = getRandomValue()  reel3.label = getRandomValue()}const finishSpin = () => {  const uniqueValues = new Set([reel1.label, reel2.label, reel3.label]).size  if (uniqueValues === 1) {    // All 3 values are the same    result.label = '💰 Jackpot!'    result.textColor = '#FDFF00'  } else if (uniqueValues === 2) {    // 2 values are the same    result.label = '😍 Winner!'    result.textColor = '#FDFF00'  } else {    // No values are the same    result.label = '🙁 Spin Again'    result.textColor = null  }  spinning = false}const touchBar = new TouchBar({  items: [    spin,    new TouchBarSpacer({ size: 'large' }),    reel1,    new TouchBarSpacer({ size: 'small' }),    reel2,    new TouchBarSpacer({ size: 'small' }),    reel3,    new TouchBarSpacer({ size: 'large' }),    result  ]})let windowapp.whenReady().then(() => {  window = new BrowserWindow({    frame: false,    titleBarStyle: 'hiddenInset',    width: 200,    height: 200,    backgroundColor: '#000'  })  window.loadURL('about:blank')  window.setTouchBar(touchBar)})
```

---

## Class: TouchBarSlider

**URL:** https://www.electronjs.org/docs/latest/api/touch-bar-slider

**Contents:**
- Class: TouchBarSlider
- Class: TouchBarSlider​
  - new TouchBarSlider(options)​
  - Instance Properties​
    - touchBarSlider.label​
    - touchBarSlider.value​
    - touchBarSlider.minValue​
    - touchBarSlider.maxValue​

Create a slider in the touch bar for native macOS applications

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

The following properties are available on instances of TouchBarSlider:

A string representing the slider's current text. Changing this value immediately updates the slider in the touch bar.

A number representing the slider's current value. Changing this value immediately updates the slider in the touch bar.

A number representing the slider's current minimum value. Changing this value immediately updates the slider in the touch bar.

A number representing the slider's current maximum value. Changing this value immediately updates the slider in the touch bar.

---

## PreloadScriptRegistration Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/preload-script-registration

**Contents:**
- PreloadScriptRegistration Object

---

## safeStorage

**URL:** https://www.electronjs.org/docs/latest/api/safe-storage

**Contents:**
- safeStorage
- Methods​
  - safeStorage.isEncryptionAvailable()​
  - safeStorage.encryptString(plainText)​
  - safeStorage.decryptString(encrypted)​
  - safeStorage.setUsePlainTextEncryption(usePlainText)​
  - safeStorage.getSelectedStorageBackend() Linux​

Allows access to simple encryption and decryption of strings for storage on the local machine.

This module adds extra protection to data being stored on disk by using OS-provided cryptography systems. Current security semantics for each platform are outlined below.

Note that on Mac, access to the system Keychain is required and these calls can block the current thread to collect user input. The same is true for Linux, if a password management tool is available.

The safeStorage module has the following methods:

Returns boolean - Whether encryption is available.

On Linux, returns true if the app has emitted the ready event and the secret key is available. On MacOS, returns true if Keychain is available. On Windows, returns true once the app has emitted the ready event.

Returns Buffer - An array of bytes representing the encrypted string.

This function will throw an error if encryption fails.

Returns string - the decrypted string. Decrypts the encrypted buffer obtained with safeStorage.encryptString back into a string.

This function will throw an error if decryption fails.

This function on Linux will force the module to use an in memory password for creating symmetric key that is used for encrypt/decrypt functions when a valid OS password manager cannot be determined for the current active desktop environment. This function is a no-op on Windows and MacOS.

Returns string - User friendly name of the password manager selected on Linux.

This function will return one of the following values:

---

## nativeTheme

**URL:** https://www.electronjs.org/docs/latest/api/native-theme

**Contents:**
- nativeTheme
- Events​
  - Event: 'updated'​
- Properties​
  - nativeTheme.shouldUseDarkColors Readonly​
  - nativeTheme.themeSource​
  - nativeTheme.shouldUseHighContrastColors macOS Windows Readonly​
  - nativeTheme.shouldUseDarkColorsForSystemIntegratedUI macOS Windows Readonly​
  - nativeTheme.shouldUseInvertedColorScheme macOS Windows Readonly​
  - nativeTheme.inForcedColorsMode Windows Readonly​

Read and respond to changes in Chromium's native color theme.

The nativeTheme module emits the following events:

Emitted when something in the underlying NativeTheme has changed. This normally means that either the value of shouldUseDarkColors, shouldUseHighContrastColors or shouldUseInvertedColorScheme has changed. You will have to check them to determine which one has changed.

The nativeTheme module has the following properties:

A boolean for if the OS / Chromium currently has a dark mode enabled or is being instructed to show a dark-style UI. If you want to modify this value you should use themeSource below.

A string property that can be system, light or dark. It is used to override and supersede the value that Chromium has chosen to use internally.

Setting this property to system will remove the override and everything will be reset to the OS default. By default themeSource is system.

Settings this property to dark will have the following effects:

Settings this property to light will have the following effects:

The usage of this property should align with a classic "dark mode" state machine in your application where the user has three options.

Your application should then always use shouldUseDarkColors to determine what CSS to apply.

A boolean for if the OS / Chromium currently has high-contrast mode enabled or is being instructed to show a high-contrast UI.

A boolean property indicating whether or not the system theme has been set to dark or light.

On Windows this property distinguishes between system and app light/dark theme, returning true if the system theme is set to dark theme and false otherwise. On macOS the return value will be the same as nativeTheme.shouldUseDarkColors.

A boolean for if the OS / Chromium currently has an inverted color scheme or is being instructed to use an inverted color scheme.

A boolean indicating whether Chromium is in forced colors mode, controlled by system accessibility settings. Currently, Windows high contrast is the only system setting that triggers forced colors mode.

A boolean that indicates the whether the user has chosen via system accessibility settings to reduce transparency at the OS level.

---

## globalShortcut

**URL:** https://www.electronjs.org/docs/latest/api/global-shortcut

**Contents:**
- globalShortcut
- Methods​
  - globalShortcut.register(accelerator, callback)​
  - globalShortcut.registerAll(accelerators, callback)​
  - globalShortcut.isRegistered(accelerator)​
  - globalShortcut.unregister(accelerator)​
  - globalShortcut.unregisterAll()​

Detect keyboard events when the application does not have keyboard focus.

The globalShortcut module can register/unregister a global keyboard shortcut with the operating system so that you can customize the operations for various shortcuts.

The shortcut is global; it will work even if the app does not have the keyboard focus. This module cannot be used before the ready event of the app module is emitted. Please also note that it is also possible to use Chromium's GlobalShortcutsPortal implementation, which allows apps to bind global shortcuts when running within a Wayland session.

See also: A detailed guide on Keyboard Shortcuts.

The globalShortcut module has the following methods:

Returns boolean - Whether or not the shortcut was registered successfully.

Registers a global shortcut of accelerator. The callback is called when the registered shortcut is pressed by the user.

When the accelerator is already taken by other applications, this call will silently fail. This behavior is intended by operating systems, since they don't want applications to fight for global shortcuts.

The following accelerators will not be registered successfully on macOS 10.14 Mojave unless the app has been authorized as a trusted accessibility client:

Registers a global shortcut of all accelerator items in accelerators. The callback is called when any of the registered shortcuts are pressed by the user.

When a given accelerator is already taken by other applications, this call will silently fail. This behavior is intended by operating systems, since they don't want applications to fight for global shortcuts.

The following accelerators will not be registered successfully on macOS 10.14 Mojave unless the app has been authorized as a trusted accessibility client:

Returns boolean - Whether this application has registered accelerator.

When the accelerator is already taken by other applications, this call will still return false. This behavior is intended by operating systems, since they don't want applications to fight for global shortcuts.

Unregisters the global shortcut of accelerator.

Unregisters all of the global shortcuts.

**Examples:**

Example 1 (javascript):
```javascript
const { app, globalShortcut } = require('electron')// Enable usage of Portal's globalShortcuts. This is essential for cases when// the app runs in a Wayland session.app.commandLine.appendSwitch('enable-features', 'GlobalShortcutsPortal')app.whenReady().then(() => {  // Register a 'CommandOrControl+X' shortcut listener.  const ret = globalShortcut.register('CommandOrControl+X', () => {    console.log('CommandOrControl+X is pressed')  })  if (!ret) {    console.log('registration failed')  }  // Check whether a shortcut is registered.  console.log(globalShortcut.isRegistered('CommandOrControl+X'))})app.on('will-quit', () => {  // Unregister a shortcut.  globalShortcut.unregister('CommandOrControl+X')  // Unregister all shortcuts.  globalShortcut.unregisterAll()})
```

---

## Opening windows from the renderer

**URL:** https://www.electronjs.org/docs/latest/api/window-open

**Contents:**
- Opening windows from the renderer
  - window.open(url[, frameName][, features])​
  - Native Window example​

There are several ways to control how windows are created from trusted or untrusted content within a renderer. Windows can be created from the renderer in two ways:

For same-origin content, the new window is created within the same process, enabling the parent to access the child window directly. This can be very useful for app sub-windows that act as preference panels, or similar, as the parent can render to the sub-window directly, as if it were a div in the parent. This is the same behavior as in the browser.

Electron pairs this native Chrome Window with a BrowserWindow under the hood. You can take advantage of all the customization available when creating a BrowserWindow in the main process by using webContents.setWindowOpenHandler() for renderer-created windows.

BrowserWindow constructor options are set by, in increasing precedence order: parsed options from the features string from window.open(), security-related webPreferences inherited from the parent, and options given by webContents.setWindowOpenHandler. Note that webContents.setWindowOpenHandler has final say and full privilege because it is invoked in the main process.

Returns Window | null

features is a comma-separated key-value list, following the standard format of the browser. Electron will parse BrowserWindowConstructorOptions out of this list where possible, for convenience. For full control and better ergonomics, consider using webContents.setWindowOpenHandler to customize the BrowserWindow creation.

A subset of WebPreferences can be set directly, unnested, from the features string: zoomFactor, nodeIntegration, javascript, contextIsolation, and webviewTag.

To customize or cancel the creation of the window, you can optionally set an override handler with webContents.setWindowOpenHandler() from the main process. Returning { action: 'deny' } cancels the window. Returning { action: 'allow', overrideBrowserWindowOptions: { ... } } will allow opening the window and setting the BrowserWindowConstructorOptions to be used when creating the window. Note that this is more powerful than passing options through the feature string, as the renderer has more limited privileges in deciding security preferences than the main process.

In addition to passing in action and overrideBrowserWindowOptions, outlivesOpener can be passed like: { action: 'allow', outlivesOpener: true, overrideBrowserWindowOptions: { ... } }. If set to true, the newly created window will not close when the opener window closes. The default value is false.

**Examples:**

Example 1 (unknown):
```unknown
window.open('https://github.com', '_blank', 'top=500,left=200,frame=false,nodeIntegration=no')
```

Example 2 (css):
```css
// main.jsconst mainWindow = new BrowserWindow()// In this example, only windows with the `about:blank` url will be created.// All other urls will be blocked.mainWindow.webContents.setWindowOpenHandler(({ url }) => {  if (url === 'about:blank') {    return {      action: 'allow',      overrideBrowserWindowOptions: {        frame: false,        fullscreenable: false,        backgroundColor: 'black',        webPreferences: {          preload: 'my-child-window-preload-script.js'        }      }    }  }  return { action: 'deny' }})
```

Example 3 (csharp):
```csharp
// renderer process (mainWindow)const childWindow = window.open('', 'modal')childWindow.document.write('<h1>Hello</h1>')
```

---

## Class: ClientRequest

**URL:** https://www.electronjs.org/docs/latest/api/client-request

**Contents:**
- Class: ClientRequest
- Class: ClientRequest​
  - new ClientRequest(options)​
  - Instance Events​
    - Event: 'response'​
    - Event: 'login'​
    - Event: 'finish'​
    - Event: 'abort'​
    - Event: 'error'​
    - Event: 'close'​

Make HTTP/HTTPS requests.

Process: Main, Utility This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

ClientRequest implements the Writable Stream interface and is therefore an EventEmitter.

options properties such as protocol, host, hostname, port and path strictly follow the Node.js model as described in the URL module.

For instance, we could have created the same request to 'github.com' as follows:

Emitted when an authenticating proxy is asking for user credentials.

The callback function is expected to be called back with user credentials:

Providing empty credentials will cancel the request and report an authentication error on the response object:

Emitted just after the last chunk of the request's data has been written into the request object.

Emitted when the request is aborted. The abort event will not be fired if the request is already closed.

Emitted when the net module fails to issue a network request. Typically when the request object emits an error event, a close event will subsequently follow and no response object will be provided.

Emitted as the last event in the HTTP request-response transaction. The close event indicates that no more events will be emitted on either the request or response objects.

Emitted when the server returns a redirect response (e.g. 301 Moved Permanently). Calling request.followRedirect will continue with the redirection. If this event is handled, request.followRedirect must be called synchronously, otherwise the request will be cancelled.

A boolean specifying whether the request will use HTTP chunked transfer encoding or not. Defaults to false. The property is readable and writable, however it can be set only before the first write operation as the HTTP headers are not yet put on the wire. Trying to set the chunkedEncoding property after the first write will throw an error.

Using chunked encoding is strongly recommended if you need to send a large request body as data will be streamed in small chunks instead of being internally buffered inside Electron process memory.

Adds an extra HTTP header. The header name will be issued as-is without lowercasing. It can be called only before first write. Calling this method after the first write will throw an error. If the passed value is not a string, its toString() method will be called to obtain the final value.

Certain headers are restricted from being set by apps. These headers are listed below. More information on restricted headers can be found in Chromium's header utils.

Additionally, setting the Connection header to the value upgrade is also disallowed.

Returns string - The value of a previously set extra header name.

Removes a previously set extra header name. This method can be called only before first write. Trying to call it after the first write will throw an error.

callback is essentially a dummy function introduced in the purpose of keeping similarity with the Node.js API. It is called asynchronously in the next tick after chunk content have been delivered to the Chromium networking layer. Contrary to the Node.js implementation, it is not guaranteed that chunk content have been flushed on the wire before callback is called.

Adds a chunk of data to the request body. The first write operation may cause the request headers to be issued on the wire. After the first write operation, it is not allowed to add or remove a custom header.

Sends the last chunk of the request data. Subsequent write or end operations will not be allowed. The finish event is emitted just after the end operation.

Cancels an ongoing HTTP transaction. If the request has already emitted the close event, the abort operation will have no effect. Otherwise an ongoing event will emit abort and close events. Additionally, if there is an ongoing response object,it will emit the aborted event.

Continues any pending redirection. Can only be called during a 'redirect' event.

You can use this method in conjunction with POST requests to get the progress of a file upload or other data transfer.

**Examples:**

Example 1 (css):
```css
const request = net.request({  method: 'GET',  protocol: 'https:',  hostname: 'github.com',  port: 443,  path: '/'})
```

Example 2 (javascript):
```javascript
request.on('login', (authInfo, callback) => {  callback('username', 'password')})
```

Example 3 (javascript):
```javascript
request.on('response', (response) => {  console.log(`STATUS: ${response.statusCode}`)  response.on('error', (error) => {    console.log(`ERROR: ${JSON.stringify(error)}`)  })})request.on('login', (authInfo, callback) => {  callback()})
```

---

## BaseWindow

**URL:** https://www.electronjs.org/docs/latest/api/base-window

**Contents:**
- BaseWindow
- Parent and child windows​
- Modal windows​
- Platform notices​
- Resource management​
- Class: BaseWindow​
  - new BaseWindow([options])​
  - Instance Events​
    - Event: 'close'​
    - Event: 'closed'​

Create and control windows.

BaseWindow provides a flexible way to compose multiple web views in a single window. For windows with only a single, full-size web view, the BrowserWindow class may be a simpler option.

This module cannot be used until the ready event of the app module is emitted.

By using parent option, you can create child windows:

The child window will always show on top of the parent window.

A modal window is a child window that disables parent window. To create a modal window, you have to set both the parent and modal options:

When you add a WebContentsView to a BaseWindow and the BaseWindow is closed, the webContents of the WebContentsView are not destroyed automatically.

It is your responsibility to close the webContents when you no longer need them, e.g. when the BaseWindow is closed:

Unlike with a BrowserWindow, if you don't explicitly close the webContents, you'll encounter memory leaks.

Create and control windows.

BaseWindow is an EventEmitter.

It creates a new BaseWindow with native properties as set by the options.

Electron's built-in classes cannot be subclassed in user code. For more information, see the FAQ.

When setting minimum or maximum window size with minWidth/maxWidth/ minHeight/maxHeight, it only constrains the users. It won't prevent you from passing a size that does not follow size constraints to setBounds/setSize or to the constructor of BrowserWindow.

The possible values and behaviors of the type option are platform dependent. Possible values are:

Objects created with new BaseWindow emit the following events:

Some events are only available on specific operating systems and are labeled as such.

Emitted when the window is going to be closed. It's emitted before the beforeunload and unload event of the DOM. Calling event.preventDefault() will cancel the close.

Usually you would want to use the beforeunload handler to decide whether the window should be closed, which will also be called when the window is reloaded. In Electron, returning any value other than undefined would cancel the close. For example:

There is a subtle difference between the behaviors of window.onbeforeunload = handler and window.addEventListener('beforeunload', handler). It is recommended to always set the event.returnValue explicitly, instead of only returning a value, as the former works more consistently within Electron.

Emitted when the window is closed. After you have received this event you should remove the reference to the window and avoid using it any more.

Emitted when a session is about to end due to a shutdown, machine restart, or user log-off. Calling event.preventDefault() can delay the system shutdown, though it’s generally best to respect the user’s choice to end the session. However, you may choose to use it if ending the session puts the user at risk of losing data.

Emitted when a session is about to end due to a shutdown, machine restart, or user log-off. Once this event fires, there is no way to prevent the session from ending.

Emitted when the window loses focus.

Emitted when the window gains focus.

Emitted when the window is shown.

Emitted when the window is hidden.

Emitted when window is maximized.

Emitted when the window exits from a maximized state.

Emitted when the window is minimized.

Emitted when the window is restored from a minimized state.

Emitted before the window is resized. Calling event.preventDefault() will prevent the window from being resized.

Note that this is only emitted when the window is being resized manually. Resizing the window with setBounds/setSize will not emit this event.

The possible values and behaviors of the edge option are platform dependent. Possible values are:

Emitted after the window has been resized.

Emitted once when the window has finished being resized.

This is usually emitted when the window has been resized manually. On macOS, resizing the window with setBounds/setSize and setting the animate parameter to true will also emit this event once resizing has finished.

Emitted before the window is moved. On Windows, calling event.preventDefault() will prevent the window from being moved.

Note that this is only emitted when the window is being moved manually. Moving the window with setPosition/setBounds/center will not emit this event.

Emitted when the window is being moved to a new position.

Emitted once when the window is moved to a new position.

On macOS, this event is an alias of move.

Emitted when the window enters a full-screen state.

Emitted when the window leaves a full-screen state.

Emitted when the window is set or unset to show always on top of other windows.

Emitted when an App Command is invoked. These are typically related to keyboard media keys or browser commands, as well as the "Back" button built into some mice on Windows.

Commands are lowercased, underscores are replaced with hyphens, and the APPCOMMAND_ prefix is stripped off. e.g. APPCOMMAND_BROWSER_BACKWARD is emitted as browser-backward.

The following app commands are explicitly supported on Linux:

Emitted on 3-finger swipe. Possible directions are up, right, down, left.

The method underlying this event is built to handle older macOS-style trackpad swiping, where the content on the screen doesn't move with the swipe. Most macOS trackpads are not configured to allow this kind of swiping anymore, so in order for it to emit properly the 'Swipe between pages' preference in System Preferences > Trackpad > More Gestures must be set to 'Swipe with two or three fingers'.

Emitted on trackpad rotation gesture. Continually emitted until rotation gesture is ended. The rotation value on each emission is the angle in degrees rotated since the last emission. The last emitted event upon a rotation gesture will always be of value 0. Counter-clockwise rotation values are positive, while clockwise ones are negative.

Emitted when the window opens a sheet.

Emitted when the window has closed a sheet.

Emitted when the native new tab button is clicked.

Emitted when the system context menu is triggered on the window, this is normally only triggered when the user right clicks on the non-client area of your window. This is the window titlebar or any area you have declared as -webkit-app-region: drag in a frameless window.

Calling event.preventDefault() will prevent the menu from being displayed.

To convert point to DIP, use screen.screenToDipPoint(point).

The BaseWindow class has the following static methods:

Returns BaseWindow[] - An array of all opened browser windows.

Returns BaseWindow | null - The window that is focused in this application, otherwise returns null.

Returns BaseWindow | null - The window with the given id.

Objects created with new BaseWindow have the following properties:

A Integer property representing the unique ID of the window. Each ID is unique among all BaseWindow instances of the entire Electron application.

A View property for the content view of the window.

A string (optional) property that is equal to the tabbingIdentifier passed to the BrowserWindow constructor or undefined if none was set.

A boolean property that determines whether the window menu bar should hide itself automatically. Once set, the menu bar will only show when users press the single Alt key.

If the menu bar is already visible, setting this property to true won't hide it immediately.

A boolean property that determines whether the window is in simple (pre-Lion) fullscreen mode.

A boolean property that determines whether the window is in fullscreen mode.

A boolean property that determines whether the window is focusable.

A boolean property that determines whether the window is visible on all workspaces.

Always returns false on Windows.

A boolean property that determines whether the window has a shadow.

A boolean property that determines whether the menu bar should be visible.

If the menu bar is auto-hide, users can still bring up the menu bar by pressing the single Alt key.

A boolean property that determines whether the window is in kiosk mode.

A boolean property that specifies whether the window’s document has been edited.

The icon in title bar will become gray when set to true.

A string property that determines the pathname of the file the window represents, and the icon of the file will show in window's title bar.

A string property that determines the title of the native window.

The title of the web page can be different from the title of the native window.

A boolean property that determines whether the window can be manually minimized by user.

On Linux the setter is a no-op, although the getter returns true.

A boolean property that determines whether the window can be manually maximized by user.

On Linux the setter is a no-op, although the getter returns true.

A boolean property that determines whether the maximize/zoom window button toggles fullscreen mode or maximizes the window.

A boolean property that determines whether the window can be manually resized by user.

A boolean property that determines whether the window can be manually closed by user.

On Linux the setter is a no-op, although the getter returns true.

A boolean property that determines Whether the window can be moved by user.

On Linux the setter is a no-op, although the getter returns true.

A boolean property that determines whether the window is excluded from the application’s Windows menu. false by default.

A string property that defines an alternative title provided only to accessibility tools such as screen readers. This string is not directly visible to users.

A boolean property that indicates whether the window is arranged via Snap.

Objects created with new BaseWindow have the following instance methods:

Some methods are only available on specific operating systems and are labeled as such.

Sets the content view of the window.

Returns View - The content view of the window.

Force closing the window, the unload and beforeunload event won't be emitted for the web page, and close event will also not be emitted for this window, but it guarantees the closed event will be emitted.

Try to close the window. This has the same effect as a user manually clicking the close button of the window. The web page may cancel the close though. See the close event.

Focuses on the window.

Removes focus from the window.

Returns boolean - Whether the window is focused.

Returns boolean - Whether the window is destroyed.

Shows and gives focus to the window.

Shows the window but doesn't focus on it.

Returns boolean - Whether the window is visible to the user in the foreground of the app.

Returns boolean - Whether current window is a modal window.

Maximizes the window. This will also show (but not focus) the window if it isn't being displayed already.

Unmaximizes the window.

Returns boolean - Whether the window is maximized.

Minimizes the window. On some platforms the minimized window will be shown in the Dock.

Restores the window from minimized state to its previous state.

Returns boolean - Whether the window is minimized.

Sets whether the window should be in fullscreen mode.

On macOS, fullscreen transitions take place asynchronously. If further actions depend on the fullscreen state, use the 'enter-full-screen' or > 'leave-full-screen' events.

Returns boolean - Whether the window is in fullscreen mode.

Enters or leaves simple fullscreen mode.

Simple fullscreen mode emulates the native fullscreen behavior found in versions of macOS prior to Lion (10.7).

Returns boolean - Whether the window is in simple (pre-Lion) fullscreen mode.

Returns boolean - Whether the window is in normal state (not maximized, not minimized, not in fullscreen mode).

This will make a window maintain an aspect ratio. The extra size allows a developer to have space, specified in pixels, not included within the aspect ratio calculations. This API already takes into account the difference between a window's size and its content size.

Consider a normal window with an HD video player and associated controls. Perhaps there are 15 pixels of controls on the left edge, 25 pixels of controls on the right edge and 50 pixels of controls below the player. In order to maintain a 16:9 aspect ratio (standard aspect ratio for HD @1920x1080) within the player itself we would call this function with arguments of 16/9 and { width: 40, height: 50 }. The second argument doesn't care where the extra width and height are within the content view--only that they exist. Sum any extra width and height areas you have within the overall content view.

The aspect ratio is not respected when window is resized programmatically with APIs like win.setSize.

To reset an aspect ratio, pass 0 as the aspectRatio value: win.setAspectRatio(0).

Examples of valid backgroundColor values:

Sets the background color of the window. See Setting backgroundColor.

Uses Quick Look to preview a file at a given path.

Closes the currently open Quick Look panel.

Resizes and moves the window to the supplied bounds. Any properties that are not supplied will default to their current values.

On macOS, the y-coordinate value cannot be smaller than the Tray height. The tray height has changed over time and depends on the operating system, but is between 20-40px. Passing a value lower than the tray height will result in a window that is flush to the tray.

Returns Rectangle - The bounds of the window as Object.

On macOS, the y-coordinate value returned will be at minimum the Tray height. For example, calling win.setBounds({ x: 25, y: 20, width: 800, height: 600 }) with a tray height of 38 means that win.getBounds() will return { x: 25, y: 38, width: 800, height: 600 }.

Returns string - Gets the background color of the window in Hex (#RRGGBB) format.

See Setting backgroundColor.

The alpha value is not returned alongside the red, green, and blue values.

Resizes and moves the window's client area (e.g. the web page) to the supplied bounds.

Returns Rectangle - The bounds of the window's client area as Object.

Returns Rectangle - Contains the window bounds of the normal state

Whatever the current state of the window : maximized, minimized or in fullscreen, this function always returns the position and size of the window in normal state. In normal state, getBounds and getNormalBounds returns the same Rectangle.

Disable or enable the window.

Returns boolean - whether the window is enabled.

Resizes the window to width and height. If width or height are below any set minimum size constraints the window will snap to its minimum size.

Returns Integer[] - Contains the window's width and height.

Resizes the window's client area (e.g. the web page) to width and height.

Returns Integer[] - Contains the window's client area's width and height.

Sets the minimum size of window to width and height.

Returns Integer[] - Contains the window's minimum width and height.

Sets the maximum size of window to width and height.

Returns Integer[] - Contains the window's maximum width and height.

Sets whether the window can be manually resized by the user.

Returns boolean - Whether the window can be manually resized by the user.

Sets whether the window can be moved by user. On Linux does nothing.

Returns boolean - Whether the window can be moved by user.

On Linux always returns true.

Sets whether the window can be manually minimized by user. On Linux does nothing.

Returns boolean - Whether the window can be manually minimized by the user.

On Linux always returns true.

Sets whether the window can be manually maximized by user. On Linux does nothing.

Returns boolean - Whether the window can be manually maximized by user.

On Linux always returns true.

Sets whether the maximize/zoom window button toggles fullscreen mode or maximizes the window.

Returns boolean - Whether the maximize/zoom window button toggles fullscreen mode or maximizes the window.

Sets whether the window can be manually closed by user. On Linux does nothing.

Returns boolean - Whether the window can be manually closed by user.

On Linux always returns true.

Sets whether the window will be hidden when the user toggles into mission control.

Returns boolean - Whether the window will be hidden when the user toggles into mission control.

Sets whether the window should show always on top of other windows. After setting this, the window is still a normal window, not a toolbox window which can not be focused on.

Returns boolean - Whether the window is always on top of other windows.

Moves window above the source window in the sense of z-order. If the mediaSourceId is not of type window or if the window does not exist then this method throws an error.

Moves window to top(z-order) regardless of focus

Moves window to the center of the screen.

Moves window to x and y.

Returns Integer[] - Contains the window's current position.

Changes the title of native window to title.

Returns string - The title of the native window.

The title of the web page can be different from the title of the native window.

Changes the attachment point for sheets on macOS. By default, sheets are attached just below the window frame, but you may want to display them beneath a HTML-rendered toolbar. For example:

window.flashFrame(bool) will flash dock icon continuously on macOS

Starts or stops flashing the window to attract user's attention.

Makes the window not show in the taskbar.

Enters or leaves kiosk mode.

Returns boolean - Whether the window is in kiosk mode.

Returns boolean - Whether the window is in Windows 10 tablet mode.

Since Windows 10 users can use their PC as tablet, under this mode apps can choose to optimize their UI for tablets, such as enlarging the titlebar and hiding titlebar buttons.

This API returns whether the window is in tablet mode, and the resize event can be be used to listen to changes to tablet mode.

Returns string - Window id in the format of DesktopCapturerSource's id. For example "window:1324:0".

More precisely the format is window:id:other_id where id is HWND on Windows, CGWindowID (uint64_t) on macOS and Window (unsigned long) on Linux. other_id is used to identify web contents (tabs) so within the same top level window.

Returns Buffer - The platform-specific handle of the window.

The native type of the handle is HWND on Windows, NSView* on macOS, and Window (unsigned long) on Linux.

Hooks a windows message. The callback is called when the message is received in the WndProc.

Returns boolean - true or false depending on whether the message is hooked.

Unhook the window message.

Unhooks all of the window messages.

Sets the pathname of the file the window represents, and the icon of the file will show in window's title bar.

Returns string - The pathname of the file the window represents.

Specifies whether the window’s document has been edited, and the icon in title bar will become gray when set to true.

Returns boolean - Whether the window's document has been edited.

Sets the menu as the window's menu bar.

Remove the window's menu bar.

Sets progress value in progress bar. Valid range is [0, 1.0].

Remove progress bar when progress < 0; Change to indeterminate mode when progress > 1.

On Linux platform, only supports Unity desktop environment, you need to specify the *.desktop file name to desktopName field in package.json. By default, it will assume {app.name}.desktop.

On Windows, a mode can be passed. Accepted values are none, normal, indeterminate, error, and paused. If you call setProgressBar without a mode set (but with a value within the valid range), normal will be assumed.

Sets a 16 x 16 pixel overlay onto the current taskbar icon, usually used to convey some sort of application status or to passively notify the user.

Invalidates the window shadow so that it is recomputed based on the current window shape.

BaseWindows that are transparent can sometimes leave behind visual artifacts on macOS. This method can be used to clear these artifacts when, for example, performing an animation.

Sets whether the window should have a shadow.

Returns boolean - Whether the window has a shadow.

Sets the opacity of the window. On Linux, does nothing. Out of bound number values are clamped to the [0, 1] range.

Returns number - between 0.0 (fully transparent) and 1.0 (fully opaque). On Linux, always returns 1.

Setting a window shape determines the area within the window where the system permits drawing and user interaction. Outside of the given region, no pixels will be drawn and no mouse events will be registered. Mouse events outside of the region will not be received by that window, but will fall through to whatever is behind the window.

Returns boolean - Whether the buttons were added successfully

Add a thumbnail toolbar with a specified set of buttons to the thumbnail image of a window in a taskbar button layout. Returns a boolean object indicates whether the thumbnail has been added successfully.

The number of buttons in thumbnail toolbar should be no greater than 7 due to the limited room. Once you setup the thumbnail toolbar, the toolbar cannot be removed due to the platform's limitation. But you can call the API with an empty array to clean the buttons.

The buttons is an array of Button objects:

The flags is an array that can include following strings:

Sets the region of the window to show as the thumbnail image displayed when hovering over the window in the taskbar. You can reset the thumbnail to be the entire window by specifying an empty region: { x: 0, y: 0, width: 0, height: 0 }.

Sets the toolTip that is displayed when hovering over the window thumbnail in the taskbar.

Sets the properties for the window's taskbar button.

relaunchCommand and relaunchDisplayName must always be set together. If one of those properties is not set, then neither will be used.

Sets the system accent color and highlighting of active window border.

The accentColor parameter accepts the following values:

Returns string | boolean - the system accent color and highlighting of active window border in Hex RGB format.

If a color has been set for the window that differs from the system accent color, the window accent color will be returned. Otherwise, a boolean will be returned, with true indicating that the window uses the global system accent color, and false indicating that accent color highlighting is disabled for this window.

Sets whether the window traffic light buttons should be visible.

Sets whether the window menu bar should hide itself automatically. Once set the menu bar will only show when users press the single Alt key.

If the menu bar is already visible, calling setAutoHideMenuBar(true) won't hide it immediately.

Returns boolean - Whether menu bar automatically hides itself.

Sets whether the menu bar should be visible. If the menu bar is auto-hide, users can still bring up the menu bar by pressing the single Alt key.

Returns boolean - Whether the menu bar is visible.

Returns boolean - whether the window is arranged via Snap.

The window is snapped via buttons shown when the mouse is hovered over window maximize button, or by dragging it to the edges of the screen.

Sets whether the window should be visible on all workspaces.

This API does nothing on Windows.

Returns boolean - Whether the window is visible on all workspaces.

This API always returns false on Windows.

Makes the window ignore all mouse events.

All mouse events happened in this window will be passed to the window below this window, but if this window has focus, it will still receive keyboard events.

Prevents the window contents from being captured by other apps.

On macOS it sets the NSWindow's sharingType to NSWindowSharingNone. On Windows it calls SetWindowDisplayAffinity with WDA_EXCLUDEFROMCAPTURE. For Windows 10 version 2004 and up the window will be removed from capture entirely, older Windows versions behave as if WDA_MONITOR is applied capturing a black window.

Returns boolean - whether or not content protection is currently enabled.

Changes whether the window can be focused.

On macOS it does not remove the focus from the window.

Returns boolean - Whether the window can be focused.

Sets parent as current window's parent window, passing null will turn current window into a top-level window.

Returns BaseWindow | null - The parent window or null if there is no parent.

Returns BaseWindow[] - All child windows.

Controls whether to hide cursor when typing.

Selects the previous tab when native tabs are enabled and there are other tabs in the window.

Selects the next tab when native tabs are enabled and there are other tabs in the window.

Shows or hides the tab overview when native tabs are enabled.

Merges all windows into one window with multiple tabs when native tabs are enabled and there is more than one open window.

Moves the current tab into a new window if native tabs are enabled and there is more than one tab in the current window.

Toggles the visibility of the tab bar if native tabs are enabled and there is only one tab in the current window.

Adds a window as a tab on this window, after the tab for the window instance.

Adds a vibrancy effect to the window. Passing null or an empty string will remove the vibrancy effect on the window.

This method sets the browser window's system-drawn background material, including behind the non-client area.

See the Windows documentation for more details.

This method is only supported on Windows 11 22H2 and up.

Set a custom position for the traffic light buttons in frameless window. Passing null will reset the position to default.

Returns Point | null - The custom position for the traffic light buttons in frameless window, null will be returned when there is no custom position.

Sets the touchBar layout for the current window. Specifying null or undefined clears the touch bar. This method only has an effect if the machine has a touch bar.

The TouchBar API is currently experimental and may change or be removed in future Electron releases.

On a Window with Window Controls Overlay already enabled, this method updates the style of the title bar overlay.

On Linux, the symbolColor is automatically calculated to have minimum accessible contrast to the color if not explicitly set.

**Examples:**

Example 1 (css):
```css
// In the main process.const { BaseWindow, WebContentsView } = require('electron')const win = new BaseWindow({ width: 800, height: 600 })const leftView = new WebContentsView()leftView.webContents.loadURL('https://electronjs.org')win.contentView.addChildView(leftView)const rightView = new WebContentsView()rightView.webContents.loadURL('https://github.com/electron/electron')win.contentView.addChildView(rightView)leftView.setBounds({ x: 0, y: 0, width: 400, height: 600 })rightView.setBounds({ x: 400, y: 0, width: 400, height: 600 })
```

Example 2 (javascript):
```javascript
const { BaseWindow } = require('electron')const parent = new BaseWindow()const child = new BaseWindow({ parent })
```

Example 3 (javascript):
```javascript
const { BaseWindow } = require('electron')const parent = new BaseWindow()const child = new BaseWindow({ parent, modal: true })
```

Example 4 (javascript):
```javascript
const { BaseWindow, WebContentsView } = require('electron')const win = new BaseWindow({ width: 800, height: 600 })const view = new WebContentsView()win.contentView.addChildView(view)win.on('closed', () => {  view.webContents.close()})
```

---

## powerSaveBlocker

**URL:** https://www.electronjs.org/docs/latest/api/power-save-blocker

**Contents:**
- powerSaveBlocker
- Methods​
  - powerSaveBlocker.start(type)​
  - powerSaveBlocker.stop(id)​
  - powerSaveBlocker.isStarted(id)​

Block the system from entering low-power (sleep) mode.

The powerSaveBlocker module has the following methods:

Returns Integer - The blocker ID that is assigned to this power blocker.

Starts preventing the system from entering lower-power mode. Returns an integer identifying the power save blocker.

prevent-display-sleep has higher precedence over prevent-app-suspension. Only the highest precedence type takes effect. In other words, prevent-display-sleep always takes precedence over prevent-app-suspension.

For example, an API calling A requests for prevent-app-suspension, and another calling B requests for prevent-display-sleep. prevent-display-sleep will be used until B stops its request. After that, prevent-app-suspension is used.

Stops the specified power save blocker.

Returns boolean - Whether the specified powerSaveBlocker has been stopped.

Returns boolean - Whether the corresponding powerSaveBlocker has started.

**Examples:**

Example 1 (javascript):
```javascript
const { powerSaveBlocker } = require('electron')const id = powerSaveBlocker.start('prevent-display-sleep')console.log(powerSaveBlocker.isStarted(id))powerSaveBlocker.stop(id)
```

---

## BrowserWindow

**URL:** https://www.electronjs.org/docs/latest/api/browser-window

**Contents:**
- BrowserWindow
- Window customization​
- Showing the window gracefully​
  - Using the ready-to-show event​
  - Setting the backgroundColor property​
- Parent and child windows​
- Modal windows​
- Page visibility​
- Platform notices​
- Class: BrowserWindow extends BaseWindow​

Create and control browser windows.

This module cannot be used until the ready event of the app module is emitted.

The BrowserWindow class exposes various ways to modify the look and behavior of your app's windows. For more details, see the Window Customization tutorial.

When loading a page in the window directly, users may see the page load incrementally, which is not a good experience for a native app. To make the window display without a visual flash, there are two solutions for different situations.

While loading the page, the ready-to-show event will be emitted when the renderer process has rendered the page for the first time if the window has not been shown yet. Showing the window after this event will have no visual flash:

This event is usually emitted after the did-finish-load event, but for pages with many remote resources, it may be emitted before the did-finish-load event.

Please note that using this event implies that the renderer will be considered "visible" and paint even though show is false. This event will never fire if you use paintWhenInitiallyHidden: false

For a complex app, the ready-to-show event could be emitted too late, making the app feel slow. In this case, it is recommended to show the window immediately, and use a backgroundColor close to your app's background:

Note that even for apps that use ready-to-show event, it is still recommended to set backgroundColor to make the app feel more native.

Some examples of valid backgroundColor values include:

For more information about these color types see valid options in win.setBackgroundColor.

By using parent option, you can create child windows:

The child window will always show on top of the top window.

A modal window is a child window that disables parent window. To create a modal window, you have to set both the parent and modal options:

The Page Visibility API works as follows:

It is recommended that you pause expensive operations when the visibility state is hidden in order to minimize power consumption.

Create and control browser windows.

BrowserWindow is an EventEmitter.

It creates a new BrowserWindow with native properties as set by the options.

Electron's built-in classes cannot be subclassed in user code. For more information, see the FAQ.

Objects created with new BrowserWindow emit the following events:

Some events are only available on specific operating systems and are labeled as such.

Emitted when the document changed its title, calling event.preventDefault() will prevent the native window's title from changing. explicitSet is false when title is synthesized from file URL.

Emitted when the window is going to be closed. It's emitted before the beforeunload and unload event of the DOM. Calling event.preventDefault() will cancel the close.

Usually you would want to use the beforeunload handler to decide whether the window should be closed, which will also be called when the window is reloaded. In Electron, returning any value other than undefined would cancel the close. For example:

There is a subtle difference between the behaviors of window.onbeforeunload = handler and window.addEventListener('beforeunload', handler). It is recommended to always set the event.returnValue explicitly, instead of only returning a value, as the former works more consistently within Electron.

Emitted when the window is closed. After you have received this event you should remove the reference to the window and avoid using it any more.

Emitted when a session is about to end due to a shutdown, machine restart, or user log-off. Calling event.preventDefault() can delay the system shutdown, though it’s generally best to respect the user’s choice to end the session. However, you may choose to use it if ending the session puts the user at risk of losing data.

Emitted when a session is about to end due to a shutdown, machine restart, or user log-off. Once this event fires, there is no way to prevent the session from ending.

Emitted when the web page becomes unresponsive.

Emitted when the unresponsive web page becomes responsive again.

Emitted when the window loses focus.

Emitted when the window gains focus.

Emitted when the window is shown.

Emitted when the window is hidden.

Emitted when the web page has been rendered (while not being shown) and window can be displayed without a visual flash.

Please note that using this event implies that the renderer will be considered "visible" and paint even though show is false. This event will never fire if you use paintWhenInitiallyHidden: false

Emitted when window is maximized.

Emitted when the window exits from a maximized state.

Emitted when the window is minimized.

Emitted when the window is restored from a minimized state.

Emitted before the window is resized. Calling event.preventDefault() will prevent the window from being resized.

Note that this is only emitted when the window is being resized manually. Resizing the window with setBounds/setSize will not emit this event.

The possible values and behaviors of the edge option are platform dependent. Possible values are:

Emitted after the window has been resized.

Emitted once when the window has finished being resized.

This is usually emitted when the window has been resized manually. On macOS, resizing the window with setBounds/setSize and setting the animate parameter to true will also emit this event once resizing has finished.

Emitted before the window is moved. On Windows, calling event.preventDefault() will prevent the window from being moved.

Note that this is only emitted when the window is being moved manually. Moving the window with setPosition/setBounds/center will not emit this event.

Emitted when the window is being moved to a new position.

Emitted once when the window is moved to a new position.

On macOS, this event is an alias of move.

Emitted when the window enters a full-screen state.

Emitted when the window leaves a full-screen state.

Emitted when the window enters a full-screen state triggered by HTML API.

Emitted when the window leaves a full-screen state triggered by HTML API.

Emitted when the window is set or unset to show always on top of other windows.

Emitted when an App Command is invoked. These are typically related to keyboard media keys or browser commands, as well as the "Back" button built into some mice on Windows.

Commands are lowercased, underscores are replaced with hyphens, and the APPCOMMAND_ prefix is stripped off. e.g. APPCOMMAND_BROWSER_BACKWARD is emitted as browser-backward.

The following app commands are explicitly supported on Linux:

Emitted on 3-finger swipe. Possible directions are up, right, down, left.

The method underlying this event is built to handle older macOS-style trackpad swiping, where the content on the screen doesn't move with the swipe. Most macOS trackpads are not configured to allow this kind of swiping anymore, so in order for it to emit properly the 'Swipe between pages' preference in System Preferences > Trackpad > More Gestures must be set to 'Swipe with two or three fingers'.

Emitted on trackpad rotation gesture. Continually emitted until rotation gesture is ended. The rotation value on each emission is the angle in degrees rotated since the last emission. The last emitted event upon a rotation gesture will always be of value 0. Counter-clockwise rotation values are positive, while clockwise ones are negative.

Emitted when the window opens a sheet.

Emitted when the window has closed a sheet.

Emitted when the native new tab button is clicked.

Emitted when the system context menu is triggered on the window, this is normally only triggered when the user right clicks on the non-client area of your window. This is the window titlebar or any area you have declared as -webkit-app-region: drag in a frameless window.

Calling event.preventDefault() will prevent the menu from being displayed.

To convert point to DIP, use screen.screenToDipPoint(point).

The BrowserWindow class has the following static methods:

Returns BrowserWindow[] - An array of all opened browser windows.

Returns BrowserWindow | null - The window that is focused in this application, otherwise returns null.

Returns BrowserWindow | null - The window that owns the given webContents or null if the contents are not owned by a window.

The BrowserView class is deprecated, and replaced by the new WebContentsView class.

Returns BrowserWindow | null - The window that owns the given browserView. If the given view is not attached to any window, returns null.

Returns BrowserWindow | null - The window with the given id.

Objects created with new BrowserWindow have the following properties:

A WebContents object this window owns. All web page related events and operations will be done via it.

See the webContents documentation for its methods and events.

A Integer property representing the unique ID of the window. Each ID is unique among all BrowserWindow instances of the entire Electron application.

A string (optional) property that is equal to the tabbingIdentifier passed to the BrowserWindow constructor or undefined if none was set.

A boolean property that determines whether the window menu bar should hide itself automatically. Once set, the menu bar will only show when users press the single Alt key.

If the menu bar is already visible, setting this property to true won't hide it immediately.

A boolean property that determines whether the window is in simple (pre-Lion) fullscreen mode.

A boolean property that determines whether the window is in fullscreen mode.

A boolean property that determines whether the window is focusable.

A boolean property that determines whether the window is visible on all workspaces.

Always returns false on Windows.

A boolean property that determines whether the window has a shadow.

A boolean property that determines whether the menu bar should be visible.

If the menu bar is auto-hide, users can still bring up the menu bar by pressing the single Alt key.

A boolean property that determines whether the window is in kiosk mode.

A boolean property that specifies whether the window’s document has been edited.

The icon in title bar will become gray when set to true.

A string property that determines the pathname of the file the window represents, and the icon of the file will show in window's title bar.

A string property that determines the title of the native window.

The title of the web page can be different from the title of the native window.

A boolean property that determines whether the window can be manually minimized by user.

On Linux the setter is a no-op, although the getter returns true.

A boolean property that determines whether the window can be manually maximized by user.

On Linux the setter is a no-op, although the getter returns true.

A boolean property that determines whether the maximize/zoom window button toggles fullscreen mode or maximizes the window.

A boolean property that determines whether the window can be manually resized by user.

A boolean property that determines whether the window can be manually closed by user.

On Linux the setter is a no-op, although the getter returns true.

A boolean property that determines Whether the window can be moved by user.

On Linux the setter is a no-op, although the getter returns true.

A boolean property that determines whether the window is excluded from the application’s Windows menu. false by default.

A string property that defines an alternative title provided only to accessibility tools such as screen readers. This string is not directly visible to users.

A boolean property that indicates whether the window is arranged via Snap.

Objects created with new BrowserWindow have the following instance methods:

Some methods are only available on specific operating systems and are labeled as such.

Force closing the window, the unload and beforeunload event won't be emitted for the web page, and close event will also not be emitted for this window, but it guarantees the closed event will be emitted.

Try to close the window. This has the same effect as a user manually clicking the close button of the window. The web page may cancel the close though. See the close event.

Focuses on the window.

On Wayland (Linux), the desktop environment may show a notification or flash the app icon if the window or app is not already focused.

Removes focus from the window.

Not supported on Wayland (Linux).

Returns boolean - Whether the window is focused.

Returns boolean - Whether the window is destroyed.

Shows and gives focus to the window.

Shows the window but doesn't focus on it.

Not supported on Wayland (Linux).

Returns boolean - Whether the window is visible to the user in the foreground of the app.

Returns boolean - Whether current window is a modal window.

Maximizes the window. This will also show (but not focus) the window if it isn't being displayed already.

Unmaximizes the window.

Returns boolean - Whether the window is maximized.

Minimizes the window. On some platforms the minimized window will be shown in the Dock.

Restores the window from minimized state to its previous state.

Returns boolean - Whether the window is minimized.

Sets whether the window should be in fullscreen mode.

On macOS, fullscreen transitions take place asynchronously. If further actions depend on the fullscreen state, use the 'enter-full-screen' or 'leave-full-screen' events.

Returns boolean - Whether the window is in fullscreen mode.

On macOS, fullscreen transitions take place asynchronously. When querying for a BrowserWindow's fullscreen status, you should ensure that either the 'enter-full-screen' or 'leave-full-screen' events have been emitted.

Enters or leaves simple fullscreen mode.

Simple fullscreen mode emulates the native fullscreen behavior found in versions of macOS prior to Lion (10.7).

Returns boolean - Whether the window is in simple (pre-Lion) fullscreen mode.

Returns boolean - Whether the window is in normal state (not maximized, not minimized, not in fullscreen mode).

This will make a window maintain an aspect ratio. The extra size allows a developer to have space, specified in pixels, not included within the aspect ratio calculations. This API already takes into account the difference between a window's size and its content size.

Consider a normal window with an HD video player and associated controls. Perhaps there are 15 pixels of controls on the left edge, 25 pixels of controls on the right edge and 50 pixels of controls below the player. In order to maintain a 16:9 aspect ratio (standard aspect ratio for HD @1920x1080) within the player itself we would call this function with arguments of 16/9 and { width: 40, height: 50 }. The second argument doesn't care where the extra width and height are within the content view--only that they exist. Sum any extra width and height areas you have within the overall content view.

The aspect ratio is not respected when window is resized programmatically with APIs like win.setSize.

To reset an aspect ratio, pass 0 as the aspectRatio value: win.setAspectRatio(0).

Examples of valid backgroundColor values:

Sets the background color of the window. See Setting backgroundColor.

Uses Quick Look to preview a file at a given path.

Closes the currently open Quick Look panel.

Resizes and moves the window to the supplied bounds. Any properties that are not supplied will default to their current values.

On Wayland (Linux), has the same limitations as setSize and setPosition.

On macOS, the y-coordinate value cannot be smaller than the Tray height. The tray height has changed over time and depends on the operating system, but is between 20-40px. Passing a value lower than the tray height will result in a window that is flush to the tray.

Returns Rectangle - The bounds of the window as Object.

On macOS, the y-coordinate value returned will be at minimum the Tray height. For example, calling win.setBounds({ x: 25, y: 20, width: 800, height: 600 }) with a tray height of 38 means that win.getBounds() will return { x: 25, y: 38, width: 800, height: 600 }.

Returns string - Gets the background color of the window in Hex (#RRGGBB) format.

See Setting backgroundColor.

The alpha value is not returned alongside the red, green, and blue values.

Resizes and moves the window's client area (e.g. the web page) to the supplied bounds.

On Wayland (Linux), has the same limitations as setContentSize and setPosition.

Returns Rectangle - The bounds of the window's client area as Object.

Returns Rectangle - Contains the window bounds of the normal state

Whatever the current state of the window (maximized, minimized or in fullscreen), this function always returns the position and size of the window in normal state. In normal state, getBounds and getNormalBounds return the same Rectangle.

Disable or enable the window.

Returns boolean - whether the window is enabled.

Resizes the window to width and height. If width or height are below any set minimum size constraints the window will snap to its minimum size.

On Wayland (Linux), may not work as some window managers restrict programmatic window resizing.

Returns Integer[] - Contains the window's width and height.

Resizes the window's client area (e.g. the web page) to width and height.

On Wayland (Linux), may not work as some window managers restrict programmatic window resizing.

Returns Integer[] - Contains the window's client area's width and height.

Sets the minimum size of window to width and height.

Returns Integer[] - Contains the window's minimum width and height.

Sets the maximum size of window to width and height.

Returns Integer[] - Contains the window's maximum width and height.

Sets whether the window can be manually resized by the user.

Returns boolean - Whether the window can be manually resized by the user.

Sets whether the window can be moved by user. On Linux does nothing.

Returns boolean - Whether the window can be moved by user.

On Linux always returns true.

Sets whether the window can be manually minimized by user. On Linux does nothing.

Returns boolean - Whether the window can be manually minimized by the user.

On Linux always returns true.

Sets whether the window can be manually maximized by user. On Linux does nothing.

Returns boolean - Whether the window can be manually maximized by user.

On Linux always returns true.

Sets whether the maximize/zoom window button toggles fullscreen mode or maximizes the window.

Returns boolean - Whether the maximize/zoom window button toggles fullscreen mode or maximizes the window.

Sets whether the window can be manually closed by user. On Linux does nothing.

Returns boolean - Whether the window can be manually closed by user.

On Linux always returns true.

Sets whether the window will be hidden when the user toggles into mission control.

Returns boolean - Whether the window will be hidden when the user toggles into mission control.

Sets whether the window should show always on top of other windows. After setting this, the window is still a normal window, not a toolbox window which can not be focused on.

Returns boolean - Whether the window is always on top of other windows.

Moves window above the source window in the sense of z-order. If the mediaSourceId is not of type window or if the window does not exist then this method throws an error.

Moves window to top(z-order) regardless of focus.

Not supported on Wayland (Linux).

Moves window to the center of the screen.

Not supported on Wayland (Linux).

Moves window to x and y.

Not supported on Wayland (Linux).

Returns Integer[] - Contains the window's current position.

Changes the title of native window to title.

Returns string - The title of the native window.

The title of the web page can be different from the title of the native window.

Changes the attachment point for sheets on macOS. By default, sheets are attached just below the window frame, but you may want to display them beneath a HTML-rendered toolbar. For example:

window.flashFrame(bool) will flash dock icon continuously on macOS

Starts or stops flashing the window to attract user's attention.

Makes the window not show in the taskbar.

Enters or leaves kiosk mode.

Returns boolean - Whether the window is in kiosk mode.

Returns boolean - Whether the window is in Windows 10 tablet mode.

Since Windows 10 users can use their PC as tablet, under this mode apps can choose to optimize their UI for tablets, such as enlarging the titlebar and hiding titlebar buttons.

This API returns whether the window is in tablet mode, and the resize event can be be used to listen to changes to tablet mode.

Returns string - Window id in the format of DesktopCapturerSource's id. For example "window:1324:0".

More precisely the format is window:id:other_id where id is HWND on Windows, CGWindowID (uint64_t) on macOS and Window (unsigned long) on Linux. other_id is used to identify web contents (tabs) so within the same top level window.

Returns Buffer - The platform-specific handle of the window.

The native type of the handle is HWND on Windows, NSView* on macOS, and Window (unsigned long) on Linux.

Hooks a windows message. The callback is called when the message is received in the WndProc.

Returns boolean - true or false depending on whether the message is hooked.

Unhook the window message.

Unhooks all of the window messages.

Sets the pathname of the file the window represents, and the icon of the file will show in window's title bar.

Returns string - The pathname of the file the window represents.

Specifies whether the window’s document has been edited, and the icon in title bar will become gray when set to true.

Returns boolean - Whether the window's document has been edited.

Returns Promise<NativeImage> - Resolves with a NativeImage

Captures a snapshot of the page within rect. Omitting rect will capture the whole visible page. If the page is not visible, rect may be empty. The page is considered visible when its browser window is hidden and the capturer count is non-zero. If you would like the page to stay hidden, you should ensure that stayHidden is set to true.

Returns Promise<void> - the promise will resolve when the page has finished loading (see did-finish-load), and rejects if the page fails to load (see did-fail-load). A noop rejection handler is already attached, which avoids unhandled rejection errors. If the existing page has a beforeUnload handler, did-fail-load will be called unless will-prevent-unload is handled.

Same as webContents.loadURL(url[, options]).

The url can be a remote address (e.g. http://) or a path to a local HTML file using the file:// protocol.

To ensure that file URLs are properly formatted, it is recommended to use Node's url.format method:

You can load a URL using a POST request with URL-encoded data by doing the following:

Returns Promise<void> - the promise will resolve when the page has finished loading (see did-finish-load), and rejects if the page fails to load (see did-fail-load).

Same as webContents.loadFile, filePath should be a path to an HTML file relative to the root of your application. See the webContents docs for more information.

Same as webContents.reload.

Sets the menu as the window's menu bar.

Remove the window's menu bar.

Sets progress value in progress bar. Valid range is [0, 1.0].

Remove progress bar when progress < 0; Change to indeterminate mode when progress > 1.

On Linux platform, only supports Unity desktop environment, you need to specify the *.desktop file name to desktopName field in package.json. By default, it will assume {app.name}.desktop.

On Windows, a mode can be passed. Accepted values are none, normal, indeterminate, error, and paused. If you call setProgressBar without a mode set (but with a value within the valid range), normal will be assumed.

Sets a 16 x 16 pixel overlay onto the current taskbar icon, usually used to convey some sort of application status or to passively notify the user.

Invalidates the window shadow so that it is recomputed based on the current window shape.

BrowserWindows that are transparent can sometimes leave behind visual artifacts on macOS. This method can be used to clear these artifacts when, for example, performing an animation.

Sets whether the window should have a shadow.

Returns boolean - Whether the window has a shadow.

Sets the opacity of the window. On Linux, does nothing. Out of bound number values are clamped to the [0, 1] range.

Returns number - between 0.0 (fully transparent) and 1.0 (fully opaque). On Linux, always returns 1.

Setting a window shape determines the area within the window where the system permits drawing and user interaction. Outside of the given region, no pixels will be drawn and no mouse events will be registered. Mouse events outside of the region will not be received by that window, but will fall through to whatever is behind the window.

Returns boolean - Whether the buttons were added successfully

Add a thumbnail toolbar with a specified set of buttons to the thumbnail image of a window in a taskbar button layout. Returns a boolean object indicates whether the thumbnail has been added successfully.

The number of buttons in thumbnail toolbar should be no greater than 7 due to the limited room. Once you setup the thumbnail toolbar, the toolbar cannot be removed due to the platform's limitation. But you can call the API with an empty array to clean the buttons.

The buttons is an array of Button objects:

The flags is an array that can include following strings:

Sets the region of the window to show as the thumbnail image displayed when hovering over the window in the taskbar. You can reset the thumbnail to be the entire window by specifying an empty region: { x: 0, y: 0, width: 0, height: 0 }.

Sets the toolTip that is displayed when hovering over the window thumbnail in the taskbar.

Sets the properties for the window's taskbar button.

relaunchCommand and relaunchDisplayName must always be set together. If one of those properties is not set, then neither will be used.

Sets the system accent color and highlighting of active window border.

The accentColor parameter accepts the following values:

Returns string | boolean - the system accent color and highlighting of active window border in Hex RGB format.

If a color has been set for the window that differs from the system accent color, the window accent color will be returned. Otherwise, a boolean will be returned, with true indicating that the window uses the global system accent color, and false indicating that accent color highlighting is disabled for this window.

Same as webContents.showDefinitionForSelection().

Sets whether the window traffic light buttons should be visible.

Sets whether the window menu bar should hide itself automatically. Once set the menu bar will only show when users press the single Alt key.

If the menu bar is already visible, calling setAutoHideMenuBar(true) won't hide it immediately.

Returns boolean - Whether menu bar automatically hides itself.

Sets whether the menu bar should be visible. If the menu bar is auto-hide, users can still bring up the menu bar by pressing the single Alt key.

Returns boolean - Whether the menu bar is visible.

Returns boolean - whether the window is arranged via Snap.

The window is snapped via buttons shown when the mouse is hovered over window maximize button, or by dragging it to the edges of the screen.

Sets whether the window should be visible on all workspaces.

This API does nothing on Windows.

Returns boolean - Whether the window is visible on all workspaces.

This API always returns false on Windows.

Makes the window ignore all mouse events.

All mouse events happened in this window will be passed to the window below this window, but if this window has focus, it will still receive keyboard events.

Prevents the window contents from being captured by other apps.

On Windows, it calls SetWindowDisplayAffinity with WDA_EXCLUDEFROMCAPTURE. For Windows 10 version 2004 and up the window will be removed from capture entirely, older Windows versions behave as if WDA_MONITOR is applied capturing a black window.

On macOS, it sets the NSWindow's sharingType to NSWindowSharingNone. Unfortunately, due to an intentional change in macOS, newer Mac applications that use ScreenCaptureKit will capture your window despite win.setContentProtection(true). See here.

Returns boolean - whether or not content protection is currently enabled.

Changes whether the window can be focused.

On macOS it does not remove the focus from the window.

Returns boolean - Whether the window can be focused.

Sets parent as current window's parent window, passing null will turn current window into a top-level window.

Returns BrowserWindow | null - The parent window or null if there is no parent.

Returns BrowserWindow[] - All child windows.

Controls whether to hide cursor when typing.

Selects the previous tab when native tabs are enabled and there are other tabs in the window.

Selects the next tab when native tabs are enabled and there are other tabs in the window.

Shows or hides the tab overview when native tabs are enabled.

Merges all windows into one window with multiple tabs when native tabs are enabled and there is more than one open window.

Moves the current tab into a new window if native tabs are enabled and there is more than one tab in the current window.

Toggles the visibility of the tab bar if native tabs are enabled and there is only one tab in the current window.

Adds a window as a tab on this window, after the tab for the window instance.

Adds a vibrancy effect to the browser window. Passing null or an empty string will remove the vibrancy effect on the window. The animationDuration parameter only animates fading in or fading out the vibrancy effect. Animating between different types of vibrancy is not supported.

This method sets the browser window's system-drawn background material, including behind the non-client area.

See the Windows documentation for more details.

This method is only supported on Windows 11 22H2 and up.

Set a custom position for the traffic light buttons in frameless window. Passing null will reset the position to default.

Returns Point | null - The custom position for the traffic light buttons in frameless window, null will be returned when there is no custom position.

Sets the touchBar layout for the current window. Specifying null or undefined clears the touch bar. This method only has an effect if the machine has a touch bar.

The TouchBar API is currently experimental and may change or be removed in future Electron releases.

The BrowserView class is deprecated, and replaced by the new WebContentsView class.

Returns BrowserView | null - The BrowserView attached to win. Returns null if one is not attached. Throws an error if multiple BrowserViews are attached.

The BrowserView class is deprecated, and replaced by the new WebContentsView class.

Replacement API for setBrowserView supporting work with multi browser views.

The BrowserView class is deprecated, and replaced by the new WebContentsView class.

The BrowserView class is deprecated, and replaced by the new WebContentsView class.

Raises browserView above other BrowserViews attached to win. Throws an error if browserView is not attached to win.

The BrowserView class is deprecated, and replaced by the new WebContentsView class.

Returns BrowserView[] - a sorted by z-index array of all BrowserViews that have been attached with addBrowserView or setBrowserView. The top-most BrowserView is the last element of the array.

The BrowserView class is deprecated, and replaced by the new WebContentsView class.

On a window with Window Controls Overlay already enabled, this method updates the style of the title bar overlay.

On Linux, the symbolColor is automatically calculated to have minimum accessible contrast to the color if not explicitly set.

**Examples:**

Example 1 (css):
```css
// In the main process.const { BrowserWindow } = require('electron')const win = new BrowserWindow({ width: 800, height: 600 })// Load a remote URLwin.loadURL('https://github.com')// Or load a local HTML filewin.loadFile('index.html')
```

Example 2 (javascript):
```javascript
const { BrowserWindow } = require('electron')const win = new BrowserWindow({ show: false })win.once('ready-to-show', () => {  win.show()})
```

Example 3 (javascript):
```javascript
const { BrowserWindow } = require('electron')const win = new BrowserWindow({ backgroundColor: '#2e2c29' })win.loadURL('https://github.com')
```

Example 4 (javascript):
```javascript
const win = new BrowserWindow()win.setBackgroundColor('hsl(230, 100%, 50%)')win.setBackgroundColor('rgb(255, 145, 145)')win.setBackgroundColor('#ff00a3')win.setBackgroundColor('blueviolet')
```

---

## screen

**URL:** https://www.electronjs.org/docs/latest/api/screen

**Contents:**
- screen
- Events​
  - Event: 'display-added'​
  - Event: 'display-removed'​
  - Event: 'display-metrics-changed'​
- Methods​
  - screen.getCursorScreenPoint()​
  - screen.getPrimaryDisplay()​
  - screen.getAllDisplays()​
  - screen.getDisplayNearestPoint(point)​

Retrieve information about screen size, displays, cursor position, etc.

This module cannot be used until the ready event of the app module is emitted.

screen is an EventEmitter.

In the renderer / DevTools, window.screen is a reserved DOM property, so writing let { screen } = require('electron') will not work.

An example of creating a window that fills the whole screen:

Another example of creating a window in the external display:

Screen coordinates used by this module are Point structures. There are two kinds of coordinates available to the process: Physical screen points are raw hardware pixels on a display. Device-independent pixel (DIP) points are virtualized screen points scaled based on the DPI (dots per inch) of the display.

The screen module emits the following events:

Emitted when newDisplay has been added.

Emitted when oldDisplay has been removed.

Emitted when one or more metrics change in a display. The changedMetrics is an array of strings that describe the changes. Possible changes are bounds, workArea, scaleFactor and rotation.

The screen module has the following methods:

The current absolute position of the mouse pointer.

The return value is a DIP point, not a screen physical point.

Returns Display - The primary display.

Returns Display[] - An array of displays that are currently available.

Returns Display - The display nearest the specified point.

Returns Display - The display that most closely intersects the provided bounds.

Converts a screen physical point to a screen DIP point. The DPI scale is performed relative to the display containing the physical point.

Not currently supported on Wayland - if used there it will return the point passed in with no changes.

Converts a screen DIP point to a screen physical point. The DPI scale is performed relative to the display containing the DIP point.

Not currently supported on Wayland.

Converts a screen physical rect to a screen DIP rect. The DPI scale is performed relative to the display nearest to window. If window is null, scaling will be performed to the display nearest to rect.

Converts a screen DIP rect to a screen physical rect. The DPI scale is performed relative to the display nearest to window. If window is null, scaling will be performed to the display nearest to rect.

**Examples:**

Example 1 (javascript):
```javascript
// Retrieve information about screen size, displays, cursor position, etc.//// For more info, see:// https://www.electronjs.org/docs/latest/api/screenconst { app, BrowserWindow, screen } = require('electron/main')let mainWindow = nullapp.whenReady().then(() => {  // Create a window that fills the screen's available work area.  const primaryDisplay = screen.getPrimaryDisplay()  const { width, height } = primaryDisplay.workAreaSize  mainWindow = new BrowserWindow({ width, height })  mainWindow.loadURL('https://electronjs.org')})
```

Example 2 (javascript):
```javascript
const { app, BrowserWindow, screen } = require('electron')let winapp.whenReady().then(() => {  const displays = screen.getAllDisplays()  const externalDisplay = displays.find((display) => {    return display.bounds.x !== 0 || display.bounds.y !== 0  })  if (externalDisplay) {    win = new BrowserWindow({      x: externalDisplay.bounds.x + 50,      y: externalDisplay.bounds.y + 50    })    win.loadURL('https://github.com')  }})
```

---

## MimeTypedBuffer Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/mime-typed-buffer

**Contents:**
- MimeTypedBuffer Object

---

## OpenExternalPermissionRequest Object extends PermissionRequest

**URL:** https://www.electronjs.org/docs/latest/api/structures/open-external-permission-request

**Contents:**
- OpenExternalPermissionRequest Object extends PermissionRequest

---

## Cookie Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/cookie

**Contents:**
- Cookie Object

---

## Class: TouchBarLabel

**URL:** https://www.electronjs.org/docs/latest/api/touch-bar-label

**Contents:**
- Class: TouchBarLabel
- Class: TouchBarLabel​
  - new TouchBarLabel(options)​
  - Instance Properties​
    - touchBarLabel.label​
    - touchBarLabel.accessibilityLabel​
    - touchBarLabel.textColor​

Create a label in the touch bar for native macOS applications

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

When defining accessibilityLabel, ensure you have considered macOS best practices.

The following properties are available on instances of TouchBarLabel:

A string representing the label's current text. Changing this value immediately updates the label in the touch bar.

A string representing the description of the label to be read by a screen reader.

A string hex code representing the label's current text color. Changing this value immediately updates the label in the touch bar.

---

## MediaAccessPermissionRequest Object extends PermissionRequest

**URL:** https://www.electronjs.org/docs/latest/api/structures/media-access-permission-request

**Contents:**
- MediaAccessPermissionRequest Object extends PermissionRequest

---

## clipboard

**URL:** https://www.electronjs.org/docs/latest/api/clipboard

**Contents:**
- clipboard
- Methods​
  - clipboard.readText([type])​
  - clipboard.writeText(text[, type])​
  - clipboard.readHTML([type])​
  - clipboard.writeHTML(markup[, type])​
  - clipboard.readImage([type])​
  - clipboard.writeImage(image[, type])​
  - clipboard.readRTF([type])​
  - clipboard.writeRTF(text[, type])​

Perform copy and paste operations on the system clipboard.

Process: Main, Renderer (non-sandboxed only)

If you want to call this API from a renderer process with context isolation enabled, place the API call in your preload script and expose it using the contextBridge API.

On Linux, there is also a selection clipboard. To manipulate it you need to pass selection to each method:

The clipboard module has the following methods:

Experimental APIs are marked as such and could be removed in future.

Returns string - The content in the clipboard as plain text.

Writes the text into the clipboard as plain text.

Returns string - The content in the clipboard as markup.

Writes markup to the clipboard.

Returns NativeImage - The image content in the clipboard.

Writes image to the clipboard.

Returns string - The content in the clipboard as RTF.

Writes the text into the clipboard in RTF.

Returns an Object containing title and url keys representing the bookmark in the clipboard. The title and url values will be empty strings when the bookmark is unavailable. The title value will always be empty on Windows.

Writes the title (macOS only) and url into the clipboard as a bookmark.

Most apps on Windows don't support pasting bookmarks into them so you can use clipboard.write to write both a bookmark and fallback text to the clipboard.

Returns string - The text on the find pasteboard, which is the pasteboard that holds information about the current state of the active application’s find panel.

This method uses synchronous IPC when called from the renderer process. The cached value is reread from the find pasteboard whenever the application is activated.

Writes the text into the find pasteboard (the pasteboard that holds information about the current state of the active application’s find panel) as plain text. This method uses synchronous IPC when called from the renderer process.

Clears the clipboard content.

Returns string[] - An array of supported formats for the clipboard type.

Returns boolean - Whether the clipboard supports the specified format.

Returns string - Reads format type from the clipboard.

format should contain valid ASCII characters and have / separator. a/c, a/bc are valid formats while /abc, abc/, a/, /a, a are not valid.

Returns Buffer - Reads format type from the clipboard.

Writes the buffer into the clipboard as format.

Writes data to the clipboard.

**Examples:**

Example 1 (javascript):
```javascript
const { clipboard } = require('electron')clipboard.writeText('Example string', 'selection')console.log(clipboard.readText('selection'))
```

Example 2 (javascript):
```javascript
const { clipboard } = require('electron')clipboard.writeText('hello i am a bit of text!')const text = clipboard.readText()console.log(text)// hello i am a bit of text!'
```

Example 3 (javascript):
```javascript
const { clipboard } = require('electron')const text = 'hello i am a bit of text!'clipboard.writeText(text)
```

Example 4 (javascript):
```javascript
const { clipboard } = require('electron')clipboard.writeHTML('<b>Hi</b>')const html = clipboard.readHTML()console.log(html)// <meta charset='utf-8'><b>Hi</b>
```

---

## ProtocolResponse Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/protocol-response

**Contents:**
- ProtocolResponse Object

---

## Point Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/point

**Contents:**
- Point Object

Both x and y must be whole integers, when providing a point object as input to an Electron API we will automatically round your x and y values to the nearest whole integer.

---

## TraceConfig Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/trace-config

**Contents:**
- TraceConfig Object

An example TraceConfig that roughly matches what Chrome DevTools records:

**Examples:**

Example 1 (json):
```json
{  recording_mode: 'record-until-full',  included_categories: [    'devtools.timeline',    'disabled-by-default-devtools.timeline',    'disabled-by-default-devtools.timeline.frame',    'disabled-by-default-devtools.timeline.stack',    'v8.execute',    'blink.console',    'blink.user_timing',    'latencyInfo',    'disabled-by-default-v8.cpu_profiler',    'disabled-by-default-v8.cpu_profiler.hires'  ],  excluded_categories: ['*']}
```

---

## MemoryInfo Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/memory-info

**Contents:**
- MemoryInfo Object

Note that all statistics are reported in Kilobytes.

---

## IpcMainServiceWorkerEvent Object extends Event

**URL:** https://www.electronjs.org/docs/latest/api/structures/ipc-main-service-worker-event

**Contents:**
- IpcMainServiceWorkerEvent Object extends Event

---

## Class: TouchBarScrubber

**URL:** https://www.electronjs.org/docs/latest/api/touch-bar-scrubber

**Contents:**
- Class: TouchBarScrubber
- Class: TouchBarScrubber​
  - new TouchBarScrubber(options)​
  - Instance Properties​
    - touchBarScrubber.items​
    - touchBarScrubber.selectedStyle​
    - touchBarScrubber.overlayStyle​
    - touchBarScrubber.showArrowButtons​
    - touchBarScrubber.mode​
    - touchBarScrubber.continuous​

Create a scrubber (a scrollable selector)

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

The following properties are available on instances of TouchBarScrubber:

A ScrubberItem[] array representing the items in this scrubber. Updating this value immediately updates the control in the touch bar. Updating deep properties inside this array does not update the touch bar.

A string representing the style that selected items in the scrubber should have. Updating this value immediately updates the control in the touch bar. Possible values:

A string representing the style that selected items in the scrubber should have. This style is overlaid on top of the scrubber item instead of being placed behind it. Updating this value immediately updates the control in the touch bar. Possible values:

A boolean representing whether to show the left / right selection arrows in this scrubber. Updating this value immediately updates the control in the touch bar.

A string representing the mode of this scrubber. Updating this value immediately updates the control in the touch bar. Possible values:

A boolean representing whether this scrubber is continuous or not. Updating this value immediately updates the control in the touch bar.

---

## Class: Cookies

**URL:** https://www.electronjs.org/docs/latest/api/cookies

**Contents:**
- Class: Cookies
- Class: Cookies​
  - Instance Events​
    - Event: 'changed'​
  - Instance Methods​
    - cookies.get(filter)​
    - cookies.set(details)​
    - cookies.remove(url, name)​
    - cookies.flushStore()​

Query and modify a session's cookies.

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

Instances of the Cookies class are accessed by using cookies property of a Session.

The following events are available on instances of Cookies:

Emitted when a cookie is changed because it was added, edited, removed, or expired.

The following methods are available on instances of Cookies:

Returns Promise<Cookie[]> - A promise which resolves an array of cookie objects.

Sends a request to get all cookies matching filter, and resolves a promise with the response.

Returns Promise<void> - A promise which resolves when the cookie has been set

Sets a cookie with details.

Returns Promise<void> - A promise which resolves when the cookie has been removed

Removes the cookies matching url and name

Returns Promise<void> - A promise which resolves when the cookie store has been flushed

Writes any unwritten cookies data to disk

Cookies written by any method will not be written to disk immediately, but will be written every 30 seconds or 512 operations

Calling this method can cause the cookie to be written to disk immediately.

**Examples:**

Example 1 (javascript):
```javascript
const { session } = require('electron')// Query all cookies.session.defaultSession.cookies.get({})  .then((cookies) => {    console.log(cookies)  }).catch((error) => {    console.log(error)  })// Query all cookies associated with a specific url.session.defaultSession.cookies.get({ url: 'https://www.github.com' })  .then((cookies) => {    console.log(cookies)  }).catch((error) => {    console.log(error)  })// Set a cookie with the given cookie data;// may overwrite equivalent cookies if they exist.const cookie = { url: 'https://www.github.com', name: 'dummy_name', value: 'dummy' }session.defaultSession.cookies.set(cookie)  .then(() => {    // success  }, (error) => {    console.error(error)  })
```

---

## KeyboardEvent Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/keyboard-event

**Contents:**
- KeyboardEvent Object

---

## ProductSubscriptionPeriod Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/product-subscription-period

**Contents:**
- ProductSubscriptionPeriod Object

---

## BaseWindowConstructorOptions Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/base-window-options

**Contents:**
- BaseWindowConstructorOptions Object

When setting minimum or maximum window size with minWidth/maxWidth/ minHeight/maxHeight, it only constrains the users. It won't prevent you from passing a size that does not follow size constraints to setBounds/setSize or to the constructor of BrowserWindow.

The possible values and behaviors of the type option are platform dependent. Possible values are:

---

## WindowOpenHandlerResponse Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/window-open-handler-response

**Contents:**
- WindowOpenHandlerResponse Object

---

## SharedTextureHandle Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/shared-texture-handle

**Contents:**
- SharedTextureHandle Object

---

## Chrome Extension Support

**URL:** https://www.electronjs.org/docs/latest/api/extensions

**Contents:**
- Chrome Extension Support
- Loading extensions​
- Supported Extensions APIs​
  - Supported Manifest Keys​
  - chrome.devtools.inspectedWindow​
  - chrome.devtools.network​
  - chrome.devtools.panels​
  - chrome.extension​
  - chrome.management​
  - chrome.runtime​

Electron supports a subset of the Chrome Extensions API, primarily to support DevTools extensions and Chromium-internal extensions, but it also happens to support some other extension capabilities.

Electron does not support arbitrary Chrome extensions from the store, and it is a non-goal of the Electron project to be perfectly compatible with Chrome's implementation of Extensions.

Electron only supports loading unpacked extensions (i.e., .crx files do not work). Extensions are installed per-session. To load an extension, call ses.extensions.loadExtension:

Loaded extensions will not be automatically remembered across exits; if you do not call loadExtension when the app runs, the extension will not be loaded.

Note that loading extensions is only supported in persistent sessions. Attempting to load an extension into an in-memory session will throw an error.

See the session documentation for more information about loading, unloading, and querying active extensions.

We support the following extensions APIs, with some caveats. Other APIs may additionally be supported, but support for any APIs not listed here is provisional and may be removed.

See Manifest file format for more information about the purpose of each possible key.

All features of this API are supported.

See official documentation for more information.

All features of this API are supported.

See official documentation for more information.

All features of this API are supported.

See official documentation for more information.

The following properties of chrome.extension are supported:

The following methods of chrome.extension are supported:

See official documentation for more information.

The following methods of chrome.management are supported:

The following events of chrome.management are supported:

See official documentation for more information.

The following properties of chrome.runtime are supported:

The following methods of chrome.runtime are supported:

The following events of chrome.runtime are supported:

See official documentation for more information.

All features of this API are supported.

See official documentation for more information.

The following methods of chrome.storage are supported:

chrome.storage.sync and chrome.storage.managed are not supported.

See official documentation for more information.

The following methods of chrome.tabs are supported:

In Chrome, passing -1 as a tab ID signifies the "currently active tab". Since Electron has no such concept, passing -1 as a tab ID is not supported and will raise an error.

See official documentation for more information.

All features of this API are supported.

Electron's webRequest module takes precedence over chrome.webRequest if there are conflicting handlers.

See official documentation for more information.

**Examples:**

Example 1 (javascript):
```javascript
const { session } = require('electron')session.defaultSession.loadExtension('path/to/unpacked/extension').then(({ id }) => {  // ...})
```

---

## Class: TouchBarButton

**URL:** https://www.electronjs.org/docs/latest/api/touch-bar-button

**Contents:**
- Class: TouchBarButton
- Class: TouchBarButton​
  - new TouchBarButton(options)​
  - Instance Properties​
    - touchBarButton.accessibilityLabel​
    - touchBarButton.label​
    - touchBarButton.backgroundColor​
    - touchBarButton.icon​
    - touchBarButton.iconPosition​
    - touchBarButton.enabled​

Create a button in the touch bar for native macOS applications

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

When defining accessibilityLabel, ensure you have considered macOS best practices.

The following properties are available on instances of TouchBarButton:

A string representing the description of the button to be read by a screen reader. Will only be read by screen readers if no label is set.

A string representing the button's current text. Changing this value immediately updates the button in the touch bar.

A string hex code representing the button's current background color. Changing this value immediately updates the button in the touch bar.

A NativeImage representing the button's current icon. Changing this value immediately updates the button in the touch bar.

A string - Can be left, right or overlay. Defaults to overlay.

A boolean representing whether the button is in an enabled state.

---

## USBDevice Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/usb-device

**Contents:**
- USBDevice Object

---

## ResolvedHost Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/resolved-host

**Contents:**
- ResolvedHost Object

---

## PermissionRequest Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/permission-request

**Contents:**
- PermissionRequest Object

---

## WebPreferences Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/web-preferences

**Contents:**
- WebPreferences Object

---

## BrowserView

**URL:** https://www.electronjs.org/docs/latest/api/browser-view

**Contents:**
- BrowserView
- Class: BrowserView​
  - Example​
  - new BrowserView([options]) Experimental Deprecated​
  - Instance Properties​
    - view.webContents Experimental Deprecated​
  - Instance Methods​
    - view.setAutoResize(options) Experimental Deprecated​
    - view.setBounds(bounds) Experimental Deprecated​
    - view.getBounds() Experimental Deprecated​

The BrowserView class is deprecated, and replaced by the new WebContentsView class.

A BrowserView can be used to embed additional web content into a BrowserWindow. It is like a child window, except that it is positioned relative to its owning window. It is meant to be an alternative to the webview tag.

Create and control views.

The BrowserView class is deprecated, and replaced by the new WebContentsView class.

This module cannot be used until the ready event of the app module is emitted.

Electron's built-in classes cannot be subclassed in user code. For more information, see the FAQ.

Objects created with new BrowserView have the following properties:

A WebContents object owned by this view.

Objects created with new BrowserView have the following instance methods:

Standardized auto-resizing behavior across all platforms

Resizes and moves the view to the supplied bounds relative to the window.

The bounds of this BrowserView instance as Object.

Examples of valid color values:

Hex format with alpha takes AARRGGBB or ARGB, not RRGGBBAA or RGB.

**Examples:**

Example 1 (unknown):
```unknown
API DEPRECATED
```

Example 2 (unknown):
```unknown
API DEPRECATED
```

Example 3 (javascript):
```javascript
// In the main process.const { app, BrowserView, BrowserWindow } = require('electron')app.whenReady().then(() => {  const win = new BrowserWindow({ width: 800, height: 600 })  const view = new BrowserView()  win.setBrowserView(view)  view.setBounds({ x: 0, y: 0, width: 300, height: 300 })  view.webContents.loadURL('https://electronjs.org')})
```

Example 4 (unknown):
```unknown
API DEPRECATED
```

---

## Class: TouchBarPopover

**URL:** https://www.electronjs.org/docs/latest/api/touch-bar-popover

**Contents:**
- Class: TouchBarPopover
- Class: TouchBarPopover​
  - new TouchBarPopover(options)​
  - Instance Properties​
    - touchBarPopover.label​
    - touchBarPopover.icon​

Create a popover in the touch bar for native macOS applications

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

The following properties are available on instances of TouchBarPopover:

A string representing the popover's current button text. Changing this value immediately updates the popover in the touch bar.

A NativeImage representing the popover's current button icon. Changing this value immediately updates the popover in the touch bar.

---

## DesktopCapturerSource Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/desktop-capturer-source

**Contents:**
- DesktopCapturerSource Object

---

## powerMonitor

**URL:** https://www.electronjs.org/docs/latest/api/power-monitor

**Contents:**
- powerMonitor
- Events​
  - Event: 'suspend'​
  - Event: 'resume'​
  - Event: 'on-ac' macOS Windows​
  - Event: 'on-battery' macOS Windows​
  - Event: 'thermal-state-change' macOS​
  - Event: 'speed-limit-change' macOS Windows​
  - Event: 'shutdown' Linux macOS​
  - Event: 'lock-screen' macOS Windows​

Monitor power state changes.

The powerMonitor module emits the following events:

Emitted when the system is suspending.

Emitted when system is resuming.

Emitted when the system changes to AC power.

Emitted when system changes to battery power.

Emitted when the thermal state of the system changes. Notification of a change in the thermal status of the system, such as entering a critical temperature range. Depending on the severity, the system might take steps to reduce said temperature, for example, throttling the CPU or switching on the fans if available.

Apps may react to the new state by reducing expensive computing tasks (e.g. video encoding), or notifying the user. The same state might be received repeatedly.

See https://developer.apple.com/library/archive/documentation/Performance/Conceptual/power_efficiency_guidelines_osx/RespondToThermalStateChanges.html

Notification of a change in the operating system's advertised speed limit for CPUs, in percent. Values below 100 indicate that the system is impairing processing power due to thermal management.

Emitted when the system is about to reboot or shut down. If the event handler invokes e.preventDefault(), Electron will attempt to delay system shutdown in order for the app to exit cleanly. If e.preventDefault() is called, the app should exit as soon as possible by calling something like app.quit().

Emitted when the system is about to lock the screen.

Emitted as soon as the systems screen is unlocked.

Emitted when a login session is activated. See documentation for more information.

Emitted when a login session is deactivated. See documentation for more information.

The powerMonitor module has the following methods:

Returns string - The system's current idle state. Can be active, idle, locked or unknown.

Calculate the system idle state. idleThreshold is the amount of time (in seconds) before considered idle. locked is available on supported systems only.

Returns Integer - Idle time in seconds

Calculate system idle time in seconds.

Returns string - The system's current thermal state. Can be unknown, nominal, fair, serious, or critical.

Returns boolean - Whether the system is on battery power.

To monitor for changes in this property, use the on-battery and on-ac events.

A boolean property. True if the system is on battery power.

See powerMonitor.isOnBatteryPower().

---

## Extension Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/extension

**Contents:**
- Extension Object

---

## HIDDevice Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/hid-device

**Contents:**
- HIDDevice Object

---

## IpcRendererEvent Object extends Event

**URL:** https://www.electronjs.org/docs/latest/api/structures/ipc-renderer-event

**Contents:**
- IpcRendererEvent Object extends Event

---

## WebRequestFilter Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/web-request-filter

**Contents:**
- WebRequestFilter Object

---

## Display Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/display

**Contents:**
- Display Object

The Display object represents a physical display connected to the system. A fake Display may exist on a headless system, or a Display may correspond to a remote, virtual display.

---

## Class: CommandLine

**URL:** https://www.electronjs.org/docs/latest/api/command-line

**Contents:**
- Class: CommandLine
- Class: CommandLine​
  - Instance Methods​
    - commandLine.appendSwitch(switch[, value])​
    - commandLine.appendArgument(value)​
    - commandLine.hasSwitch(switch)​
    - commandLine.getSwitchValue(switch)​
    - commandLine.removeSwitch(switch)​

Manipulate the command line arguments for your app that Chromium reads

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

The following example shows how to check if the --disable-gpu flag is set.

For more information on what kinds of flags and switches you can use, check out the Command Line Switches document.

Append a switch (with optional value) to Chromium's command line.

This will not affect process.argv. The intended usage of this function is to control Chromium's behavior.

Append an argument to Chromium's command line. The argument will be quoted correctly. Switches will precede arguments regardless of appending order.

If you're appending an argument like --switch=value, consider using appendSwitch('switch', 'value') instead.

This will not affect process.argv. The intended usage of this function is to control Chromium's behavior.

Returns boolean - Whether the command-line switch is present.

Returns string - The command-line switch value.

This function is meant to obtain Chromium command line switches. It is not meant to be used for application-specific command line arguments. For the latter, please use process.argv.

When the switch is not present or has no value, it returns empty string.

Removes the specified switch from Chromium's command line.

This will not affect process.argv. The intended usage of this function is to control Chromium's behavior.

**Examples:**

Example 1 (javascript):
```javascript
const { app } = require('electron')app.commandLine.hasSwitch('disable-gpu')
```

Example 2 (javascript):
```javascript
const { app } = require('electron')app.commandLine.appendSwitch('remote-debugging-port', '8315')
```

Example 3 (javascript):
```javascript
const { app } = require('electron')app.commandLine.appendArgument('--enable-experimental-web-platform-features')
```

Example 4 (javascript):
```javascript
const { app } = require('electron')app.commandLine.appendSwitch('remote-debugging-port', '8315')const hasPort = app.commandLine.hasSwitch('remote-debugging-port')console.log(hasPort) // true
```

---

## Size Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/size

**Contents:**
- Size Object

---

## Class: TouchBarOtherItemsProxy

**URL:** https://www.electronjs.org/docs/latest/api/touch-bar-other-items-proxy

**Contents:**
- Class: TouchBarOtherItemsProxy
- Class: TouchBarOtherItemsProxy​
  - new TouchBarOtherItemsProxy()​

Instantiates a special "other items proxy", which nests TouchBar elements inherited from Chromium at the space indicated by the proxy. By default, this proxy is added to each TouchBar at the end of the input. For more information, see the AppKit docs on NSTouchBarItemIdentifierOtherItemsProxy

Only one instance of this class can be added per TouchBar.

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

---

## netLog

**URL:** https://www.electronjs.org/docs/latest/api/net-log

**Contents:**
- netLog
- Methods​
  - netLog.startLogging(path[, options])​
  - netLog.stopLogging()​
- Properties​
  - netLog.currentlyLogging Readonly​

Logging network events for a session.

See --log-net-log to log network events throughout the app's lifecycle.

All methods unless specified can only be used after the ready event of the app module gets emitted.

Returns Promise<void> - resolves when the net log has begun recording.

Starts recording network events to path.

Returns Promise<void> - resolves when the net log has been flushed to disk.

Stops recording network events. If not called, net logging will automatically end when app quits.

A boolean property that indicates whether network logs are currently being recorded.

**Examples:**

Example 1 (javascript):
```javascript
const { app, netLog } = require('electron')app.whenReady().then(async () => {  await netLog.startLogging('/path/to/net-log')  // After some network events  const path = await netLog.stopLogging()  console.log('Net-logs written to', path)})
```

---

## contentTracing

**URL:** https://www.electronjs.org/docs/latest/api/content-tracing

**Contents:**
- contentTracing
- Methods​
  - contentTracing.getCategories()​
  - contentTracing.startRecording(options)​
  - contentTracing.stopRecording([resultFilePath])​
  - contentTracing.getTraceBufferUsage()​

Collect tracing data from Chromium to find performance bottlenecks and slow operations.

This module does not include a web interface. To view recorded traces, use trace viewer, available at chrome://tracing in Chrome.

You should not use this module until the ready event of the app module is emitted.

The contentTracing module has the following methods:

Returns Promise<string[]> - resolves with an array of category groups once all child processes have acknowledged the getCategories request

Get a set of category groups. The category groups can change as new code paths are reached. See also the list of built-in tracing categories.

NOTE: Electron adds a non-default tracing category called "electron". This category can be used to capture Electron-specific tracing events.

Returns Promise<void> - resolved once all child processes have acknowledged the startRecording request.

Start recording on all processes.

Recording begins immediately locally and asynchronously on child processes as soon as they receive the EnableRecording request.

If a recording is already running, the promise will be immediately resolved, as only one trace operation can be in progress at a time.

Returns Promise<string> - resolves with a path to a file that contains the traced data once all child processes have acknowledged the stopRecording request

Stop recording on all processes.

Child processes typically cache trace data and only rarely flush and send trace data back to the main process. This helps to minimize the runtime overhead of tracing since sending trace data over IPC can be an expensive operation. So, to end tracing, Chromium asynchronously asks all child processes to flush any pending trace data.

Trace data will be written into resultFilePath. If resultFilePath is empty or not provided, trace data will be written to a temporary file, and the path will be returned in the promise.

Returns Promise<Object> - Resolves with an object containing the value and percentage of trace buffer maximum usage

Get the maximum usage across processes of trace buffer as a percentage of the full state.

**Examples:**

Example 1 (javascript):
```javascript
const { app, contentTracing } = require('electron')app.whenReady().then(() => {  (async () => {    await contentTracing.startRecording({      included_categories: ['*']    })    console.log('Tracing started')    await new Promise(resolve => setTimeout(resolve, 5000))    const path = await contentTracing.stopRecording()    console.log('Tracing data recorded to ' + path)  })()})
```

---

## FileFilter Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/file-filter

**Contents:**
- FileFilter Object

---

## ImageView

**URL:** https://www.electronjs.org/docs/latest/api/image-view

**Contents:**
- ImageView
- Class: ImageView extends View​
  - new ImageView() Experimental​
  - Instance Methods​
    - image.setImage(image) Experimental​

A View that displays an image.

This module cannot be used until the ready event of the app module is emitted.

Useful for showing splash screens that will be swapped for WebContentsViews when the content finishes loading.

Note that ImageView is experimental and may be changed or removed in the future.

A View that displays an image.

ImageView inherits from View.

ImageView is an EventEmitter.

Electron's built-in classes cannot be subclassed in user code. For more information, see the FAQ.

Creates an ImageView.

The following methods are available on instances of the ImageView class, in addition to those inherited from View:

Sets the image for this ImageView. Note that only image formats supported by NativeImage can be used with an ImageView.

**Examples:**

Example 1 (javascript):
```javascript
const { BaseWindow, ImageView, nativeImage, WebContentsView } = require('electron')const path = require('node:path')const win = new BaseWindow({ width: 800, height: 600 })// Create a "splash screen" image to display while the WebContentsView loadsconst splashView = new ImageView()const splashImage = nativeImage.createFromPath(path.join(__dirname, 'loading.png'))splashView.setImage(splashImage)win.setContentView(splashView)const webContentsView = new WebContentsView()webContentsView.webContents.once('did-finish-load', () => {  // Now that the WebContentsView has loaded, swap out the "splash screen" ImageView  win.setContentView(webContentsView)})webContentsView.webContents.loadURL('https://electronjs.org')
```

---

## webFrameMain

**URL:** https://www.electronjs.org/docs/latest/api/web-frame-main

**Contents:**
- webFrameMain
- Methods​
  - webFrameMain.fromId(processId, routingId)​
  - webFrameMain.fromFrameToken(processId, frameToken)​
- Class: WebFrameMain​
  - Instance Events​
    - Event: 'dom-ready'​
  - Instance Methods​
    - frame.executeJavaScript(code[, userGesture])​
    - frame.reload()​

Control web pages and iframes.

The webFrameMain module can be used to lookup frames across existing WebContents instances. Navigation events are the common use case.

You can also access frames of existing pages by using the mainFrame property of WebContents.

These methods can be accessed from the webFrameMain module:

Returns WebFrameMain | undefined - A frame with the given process and routing IDs, or undefined if there is no WebFrameMain associated with the given IDs.

Returns WebFrameMain | null - A frame with the given process and frame token, or null if there is no WebFrameMain associated with the given IDs.

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

Emitted when the document is loaded.

Returns Promise<unknown> - A promise that resolves with the result of the executed code or is rejected if execution throws or results in a rejected promise.

Evaluates code in page.

In the browser window some HTML APIs like requestFullScreen can only be invoked by a gesture from the user. Setting userGesture to true will remove this limitation.

Returns boolean - Whether the reload was initiated successfully. Only results in false when the frame has no history.

Returns boolean - Whether the frame is destroyed.

Send an asynchronous message to the renderer process via channel, along with arguments. Arguments will be serialized with the Structured Clone Algorithm, just like postMessage, so prototype chains will not be included. Sending Functions, Promises, Symbols, WeakMaps, or WeakSets will throw an exception.

The renderer process can handle the message by listening to channel with the ipcRenderer module.

Send a message to the renderer process, optionally transferring ownership of zero or more MessagePortMain objects.

The transferred MessagePortMain objects will be available in the renderer process by accessing the ports property of the emitted event. When they arrive in the renderer, they will be native DOM MessagePort objects.

Returns Promise<string> | Promise<void> - A promise that resolves with the currently running JavaScript call stack. If no JavaScript runs in the frame, the promise will never resolve. In cases where the call stack is otherwise unable to be collected, it will return undefined.

This can be useful to determine why the frame is unresponsive in cases where there's long-running JavaScript. For more information, see the proposed Crash Reporting API.

An IpcMain instance scoped to the frame.

IPC messages sent with ipcRenderer.send, ipcRenderer.sendSync or ipcRenderer.postMessage will be delivered in the following order:

Handlers registered with invoke will be checked in the following order. The first one that is defined will be called, the rest will be ignored.

In most cases, only the main frame of a WebContents can send or receive IPC messages. However, if the nodeIntegrationInSubFrames option is enabled, it is possible for child frames to send and receive IPC messages also. The WebContents.ipc interface may be more convenient when nodeIntegrationInSubFrames is not enabled.

A string representing the current URL of the frame.

A string representing the current origin of the frame, serialized according to RFC 6454. This may be different from the URL. For instance, if the frame is a child window opened to about:blank, then frame.origin will return the parent frame's origin, while frame.url will return the empty string. Pages without a scheme/host/port triple origin will have the serialized origin of "null" (that is, the string containing the letters n, u, l, l).

A WebFrameMain | null representing top frame in the frame hierarchy to which frame belongs.

A WebFrameMain | null representing parent frame of frame, the property would be null if frame is the top frame in the frame hierarchy.

A WebFrameMain[] collection containing the direct descendents of frame.

A WebFrameMain[] collection containing every frame in the subtree of frame, including itself. This can be useful when traversing through all frames.

An Integer representing the id of the frame's internal FrameTreeNode instance. This id is browser-global and uniquely identifies a frame that hosts content. The identifier is fixed at the creation of the frame and stays constant for the lifetime of the frame. When the frame is removed, the id is not used again.

A string representing the frame name.

A string which uniquely identifies the frame within its associated renderer process. This is equivalent to webFrame.frameToken.

An Integer representing the operating system pid of the process which owns this frame.

An Integer representing the Chromium internal pid of the process which owns this frame. This is not the same as the OS process ID; to read that use frame.osProcessId.

An Integer representing the unique frame id in the current renderer process. Distinct WebFrameMain instances that refer to the same underlying frame will have the same routingId.

A string representing the visibility state of the frame.

See also how the Page Visibility API is affected by other Electron APIs.

A Boolean representing whether the frame is detached from the frame tree. If a frame is accessed while the corresponding page is running any unload listeners, it may become detached as the newly navigated page replaced it in the frame tree.

**Examples:**

Example 1 (javascript):
```javascript
const { BrowserWindow, webFrameMain } = require('electron')const win = new BrowserWindow({ width: 800, height: 1500 })win.loadURL('https://twitter.com')win.webContents.on(  'did-frame-navigate',  (event, url, httpResponseCode, httpStatusText, isMainFrame, frameProcessId, frameRoutingId) => {    const frame = webFrameMain.fromId(frameProcessId, frameRoutingId)    if (frame) {      const code = 'document.body.innerHTML = document.body.innerHTML.replaceAll("heck", "h*ck")'      frame.executeJavaScript(code)    }  })
```

Example 2 (javascript):
```javascript
const { BrowserWindow } = require('electron')async function main () {  const win = new BrowserWindow({ width: 800, height: 600 })  await win.loadURL('https://reddit.com')  const youtubeEmbeds = win.webContents.mainFrame.frames.filter((frame) => {    try {      const url = new URL(frame.url)      return url.host === 'www.youtube.com'    } catch {      return false    }  })  console.log(youtubeEmbeds)}main()
```

Example 3 (csharp):
```csharp
// Main processconst win = new BrowserWindow()const { port1, port2 } = new MessageChannelMain()win.webContents.mainFrame.postMessage('port', { message: 'hello' }, [port1])// Renderer processipcRenderer.on('port', (e, msg) => {  const [port] = e.ports  // ...})
```

Example 4 (javascript):
```javascript
const { app } = require('electron')app.commandLine.appendSwitch('enable-features', 'DocumentPolicyIncludeJSCallStacksInCrashReports')app.on('web-contents-created', (_, webContents) => {  webContents.on('unresponsive', async () => {    // Interrupt execution and collect call stack from unresponsive renderer    const callStack = await webContents.mainFrame.collectJavaScriptCallStack()    console.log('Renderer unresponsive\n', callStack)  })})
```

---

## Class: NavigationHistory

**URL:** https://www.electronjs.org/docs/latest/api/navigation-history

**Contents:**
- Class: NavigationHistory
- Class: NavigationHistory​
  - Instance Methods​
    - navigationHistory.canGoBack()​
    - navigationHistory.canGoForward()​
    - navigationHistory.canGoToOffset(offset)​
    - navigationHistory.clear()​
    - navigationHistory.getActiveIndex()​
    - navigationHistory.getEntryAtIndex(index)​
    - navigationHistory.goBack()​

Manage a list of navigation entries, representing the user's browsing history within the application.

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

Each NavigationEntry corresponds to a specific visited page. The indexing system follows a sequential order, where the entry for the earliest visited page is at index 0 and the entry for the most recent visited page is at index N.

Some APIs in this class also accept an offset, which is an integer representing the relative position of an index from the current entry according to the above indexing system (i.e. an offset value of 1 would represent going forward in history by one page).

Maintaining this ordered list of navigation entries enables seamless navigation both backward and forward through the user's browsing history.

Returns boolean - Whether the browser can go back to previous web page.

Returns boolean - Whether the browser can go forward to next web page.

Returns boolean - Whether the web page can go to the specified relative offset from the current entry.

Clears the navigation history.

Returns Integer - The index of the current page, from which we would go back/forward or reload.

Returns NavigationEntry - Navigation entry at the given index.

If index is out of bounds (greater than history length or less than 0), null will be returned.

Makes the browser go back a web page.

Makes the browser go forward a web page.

Navigates browser to the specified absolute web page index.

Navigates to the specified relative offset from the current entry.

Returns Integer - History length.

Removes the navigation entry at the given index. Can't remove entry at the "current active index".

Returns boolean - Whether the navigation entry was removed from the webContents history.

Returns NavigationEntry[] - WebContents complete history.

Restores navigation history and loads the given entry in the in stack. Will make a best effort to restore not just the navigation stack but also the state of the individual pages - for instance including HTML form values or the scroll position. It's recommended to call this API before any navigation entries are created, so ideally before you call loadURL() or loadFile() on the webContents object.

This API allows you to create common flows that aim to restore, recreate, or clone other webContents.

Returns Promise<void> - the promise will resolve when the page has finished loading the selected navigation entry (see did-finish-load), and rejects if the page fails to load (see did-fail-load). A noop rejection handler is already attached, which avoids unhandled rejection errors.

---

## NotificationAction Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/notification-action

**Contents:**
- NotificationAction Object
- Platform / Action Support​
  - Button support on macOS​

In order for extra notification buttons to work on macOS your app must meet the following criteria.

If either of these requirements are not met the button won't appear.

---

## PreloadScript Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/preload-script

**Contents:**
- PreloadScript Object

---

## SerialPort Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/serial-port

**Contents:**
- SerialPort Object

---

## UserDefaultTypes Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/user-default-types

**Contents:**
- UserDefaultTypes Object

This type is a helper alias, no object will ever exist of this type.

---

## IpcMainEvent Object extends Event

**URL:** https://www.electronjs.org/docs/latest/api/structures/ipc-main-event

**Contents:**
- IpcMainEvent Object extends Event

---

## ExtensionInfo Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/extension-info

**Contents:**
- ExtensionInfo Object

---

## nativeImage

**URL:** https://www.electronjs.org/docs/latest/api/native-image

**Contents:**
- nativeImage
- Supported Formats​
- High Resolution Image​
- Template Image macOS​
- Methods​
  - nativeImage.createEmpty()​
  - nativeImage.createThumbnailFromPath(path, size) macOS Windows​
  - nativeImage.createFromPath(path)​
  - nativeImage.createFromBitmap(buffer, options)​
  - nativeImage.createFromBuffer(buffer[, options])​

Create tray, dock, and application icons using PNG or JPG files.

Process: Main, Renderer

If you want to call this API from a renderer process with context isolation enabled, place the API call in your preload script and expose it using the contextBridge API.

The nativeImage module provides a unified interface for manipulating system images. These can be handy if you want to provide multiple scaled versions of the same icon or take advantage of macOS template images.

Electron APIs that take image files accept either file paths or NativeImage instances. An empty and transparent image will be used when null is passed.

For example, when creating a Tray or setting a BrowserWindow's icon, you can either pass an image file path as a string:

or generate a NativeImage instance from the same file:

Currently, PNG and JPEG image formats are supported across all platforms. PNG is recommended because of its support for transparency and lossless compression.

On Windows, you can also load ICO icons from file paths. For best visual quality, we recommend including at least the following sizes:

Check the Icon Scaling section in the Windows App Icon Construction reference.

EXIF metadata is currently not supported and will not be taken into account during image encoding and decoding.

On platforms that support high pixel density displays (such as Apple Retina), you can append @2x after image's base filename to mark it as a 2x scale high resolution image.

For example, if icon.png is a normal image that has standard resolution, then icon@2x.png will be treated as a high resolution image that has double Dots per Inch (DPI) density.

If you want to support displays with different DPI densities at the same time, you can put images with different sizes in the same folder and use the filename without DPI suffixes within Electron. For example:

The following suffixes for DPI are also supported:

On macOS, template images consist of black and an alpha channel. Template images are not intended to be used as standalone images and are usually mixed with other content to create the desired final appearance.

The most common case is to use template images for a menu bar (Tray) icon, so it can adapt to both light and dark menu bars.

To mark an image as a template image, its base filename should end with the word Template (e.g. xxxTemplate.png). You can also specify template images at different DPI densities (e.g. xxxTemplate@2x.png).

The nativeImage module has the following methods, all of which return an instance of the NativeImage class:

Creates an empty NativeImage instance.

Returns Promise<NativeImage> - fulfilled with the file's thumbnail preview image, which is a NativeImage.

Windows implementation will ignore size.height and scale the height according to size.width.

Creates a new NativeImage instance from an image file (e.g., PNG or JPEG) located at path. This method returns an empty image if the path does not exist, cannot be read, or is not a valid image.

Creates a new NativeImage instance from buffer that contains the raw bitmap pixel data returned by toBitmap(). The specific format is platform-dependent.

Creates a new NativeImage instance from buffer. Tries to decode as PNG or JPEG first.

Creates a new NativeImage instance from dataUrl, a base 64 encoded Data URL string.

Creates a new NativeImage instance from the NSImage that maps to the given image name. See Apple's NSImageName documentation and SF Symbols for a list of possible values.

The hslShift is applied to the image with the following rules:

This means that [-1, 0, 1] will make the image completely white and [-1, 1, 0] will make the image completely black.

In some cases, the NSImageName doesn't match its string representation; one example of this is NSFolderImageName, whose string representation would actually be NSFolder. Therefore, you'll need to determine the correct string representation for your image before passing it in. This can be done with the following:

where SYSTEM_IMAGE_NAME should be replaced with any value from this list.

For SF Symbols, usage looks as follows:

where 'square.and.pencil' is the symbol name from the SF Symbols app.

Natively wrap images such as tray, dock, and application icons.

Process: Main, Renderer This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

The following methods are available on instances of the NativeImage class:

Returns Buffer - A Buffer that contains the image's PNG encoded data.

Returns Buffer - A Buffer that contains the image's JPEG encoded data.

Returns Buffer - A Buffer that contains a copy of the image's raw bitmap pixel data.

nativeImage.toDataURL will preserve PNG colorspace

Returns string - The Data URL of the image.

Legacy alias for image.toBitmap().

Returns Buffer - A Buffer that stores C pointer to underlying native handle of the image. On macOS, a pointer to NSImage instance is returned.

Notice that the returned pointer is a weak pointer to the underlying native image instead of a copy, so you must ensure that the associated nativeImage instance is kept around.

Returns boolean - Whether the image is empty.

If scaleFactor is passed, this will return the size corresponding to the image representation most closely matching the passed value.

Marks the image as a macOS template image.

Returns boolean - Whether the image is a macOS template image.

Returns NativeImage - The cropped image.

Returns NativeImage - The resized image.

If only the height or the width are specified then the current aspect ratio will be preserved in the resized image.

Returns Number - The image's aspect ratio (width divided by height).

If scaleFactor is passed, this will return the aspect ratio corresponding to the image representation most closely matching the passed value.

Returns Number[] - An array of all scale factors corresponding to representations for a given NativeImage.

Add an image representation for a specific scale factor. This can be used to programmatically add different scale factor representations to an image. This can be called on empty images.

A boolean property that determines whether the image is considered a template image.

Please note that this property only has an effect on macOS.

**Examples:**

Example 1 (javascript):
```javascript
const { BrowserWindow, Tray } = require('electron')const tray = new Tray('/Users/somebody/images/icon.png')const win = new BrowserWindow({ icon: '/Users/somebody/images/window.png' })
```

Example 2 (javascript):
```javascript
const { BrowserWindow, nativeImage, Tray } = require('electron')const trayIcon = nativeImage.createFromPath('/Users/somebody/images/icon.png')const appIcon = nativeImage.createFromPath('/Users/somebody/images/window.png')const tray = new Tray(trayIcon)const win = new BrowserWindow({ icon: appIcon })
```

Example 3 (python):
```python
images/├── icon.png├── icon@2x.png└── icon@3x.png
```

Example 4 (javascript):
```javascript
const { Tray } = require('electron')const appTray = new Tray('/Users/somebody/images/icon.png')
```

---

## Supported Command Line Switches

**URL:** https://www.electronjs.org/docs/latest/api/command-line-switches

**Contents:**
- Supported Command Line Switches
- Electron CLI Flags​
  - --auth-server-whitelist=url​
  - --auth-negotiate-delegate-whitelist=url​
  - --disable-ntlm-v2​
  - --disable-http-cache​
  - --disable-http2​
  - --disable-renderer-backgrounding​
  - --disk-cache-size=size​
  - --enable-logging[=file]​

Command line switches supported by Electron.

You can use app.commandLine.appendSwitch to append them in your app's main script before the ready event of the app module is emitted:

A comma-separated list of servers for which integrated authentication is enabled.

then any url ending with example.com, foobar.com, baz will be considered for integrated authentication. Without * prefix the URL has to match exactly.

A comma-separated list of servers for which delegation of user credentials is required. Without * prefix the URL has to match exactly.

Disables NTLM v2 for POSIX platforms, no effect elsewhere.

Disables the disk cache for HTTP requests.

Disable HTTP/2 and SPDY/3.1 protocols.

Prevents Chromium from lowering the priority of invisible pages' renderer processes.

This flag is global to all renderer processes, if you only want to disable throttling in one window, you can take the hack of playing silent audio.

Forces the maximum disk space to be used by the disk cache, in bytes.

Prints Chromium's logging to stderr (or a log file).

The ELECTRON_ENABLE_LOGGING environment variable has the same effect as passing --enable-logging.

Passing --enable-logging will result in logs being printed on stderr. Passing --enable-logging=file will result in logs being saved to the file specified by --log-file=..., or to electron_debug.log in the user-data directory if --log-file is not specified.

On Windows, logs from child processes cannot be sent to stderr. Logging to a file is the most reliable way to collect logs on Windows.

See also --log-file, --log-level, --v, and --vmodule.

Field trials to be forcefully enabled or disabled.

For example: WebRTC-Audio-Red-For-Opus/Enabled/

A comma-separated list of rules that control how hostnames are mapped.

These mappings apply to the endpoint host in a net request (the TCP connect and host resolver in a direct connection, and the CONNECT in an HTTP proxy connection, and the endpoint host in a SOCKS proxy connection).

Deprecated: Use the --host-resolver-rules switch instead.

A comma-separated list of rules that control how hostnames are mapped.

These rules only apply to the host resolver.

Ignores certificate related errors.

Ignore the connections limit for domains list separated by ,.

Specifies the flags passed to the V8 engine. In order to enable the flags in the main process, this switch must be passed on startup.

Run node --v8-options or electron --js-flags="--help" in your terminal for the list of available flags. These can be used to enable early-stage JavaScript features, or log and manipulate garbage collection, among other things.

For example, to trace V8 optimization and deoptimization:

If --enable-logging is specified, logs will be written to the given path. The parent directory must exist.

Setting the ELECTRON_LOG_FILE environment variable is equivalent to passing this flag. If both are present, the command-line switch takes precedence.

Enables net log events to be saved and writes them to path.

Sets the verbosity of logging when used together with --enable-logging. N should be one of Chrome's LogSeverities.

Note that two complimentary logging mechanisms in Chromium -- LOG() and VLOG() -- are controlled by different switches. --log-level controls LOG() messages, while --v and --vmodule control VLOG() messages. So you may want to use a combination of these three switches depending on the granularity you want and what logging calls are made by the code you're trying to watch.

See Chromium Logging source for more information on how LOG() and VLOG() interact. Loosely speaking, VLOG() can be thought of as sub-levels / per-module levels inside LOG(INFO) to control the firehose of LOG(INFO) data.

See also --enable-logging, --log-level, --v, and --vmodule.

Don't use a proxy server and always make direct connections. Overrides any other proxy server flags that are passed.

Disables the Chromium sandbox. Forces renderer process and Chromium helper processes to run un-sandboxed. Should only be used for testing.

Instructs Electron to bypass the proxy server for the given semi-colon-separated list of hosts. This flag has an effect only if used in tandem with --proxy-server.

Will use the proxy server for all hosts except for local addresses (localhost, 127.0.0.1 etc.), google.com subdomains, hosts that contain the suffix foo.com and anything at 1.2.3.4:5678.

Uses the PAC script at the specified url.

Use a specified proxy server, which overrides the system setting. This switch only affects requests with HTTP protocol, including HTTPS and WebSocket requests. It is also noteworthy that not all proxy servers support HTTPS and WebSocket requests. The proxy URL does not support username and password authentication per Chromium issue.

Enables remote debugging over HTTP on the specified port.

Gives the default maximal active V-logging level; 0 is the default. Normally positive values are used for V-logging levels.

This switch only works when --enable-logging is also passed.

See also --enable-logging, --log-level, and --vmodule.

Gives the per-module maximal V-logging levels to override the value given by --v. E.g. my_module=2,foo*=3 would change the logging level for all code in source files my_module.* and foo*.*.

Any pattern containing a forward or backward slash will be tested against the whole pathname and not only the module. E.g. */foo/bar/*=2 would change the logging level for all code in the source files under a foo/bar directory.

This switch only works when --enable-logging is also passed.

See also --enable-logging, --log-level, and --v.

Force using discrete GPU when there are multiple GPUs available.

Force using integrated GPU when there are multiple GPUs available.

Sets the minimum required version of XDG portal implementation to version in order to use the portal backend for file dialogs on linux. File dialogs will fallback to using gtk or kde depending on the desktop environment when the required version is unavailable. Current default is set to 3.

Electron supports some of the CLI flags supported by Node.js.

Passing unsupported command line switches to Electron when it is not running in ELECTRON_RUN_AS_NODE will have no effect.

Activate inspector on host:port and break at start of user script. Default host:port is 127.0.0.1:9229.

Aliased to --debug-brk=[host:]port.

Activate inspector on host:port and break at start of the first internal JavaScript script executed when the inspector is available. Default host:port is 127.0.0.1:9229.

Set the host:port to be used when the inspector is activated. Useful when activating the inspector by sending the SIGUSR1 signal. Default host is 127.0.0.1.

Aliased to --debug-port=[host:]port.

Activate inspector on host:port. Default is 127.0.0.1:9229.

V8 inspector integration allows tools such as Chrome DevTools and IDEs to debug and profile Electron instances. The tools attach to Electron instances via a TCP port and communicate using the Chrome DevTools Protocol.

See the Debugging the Main Process guide for more details.

Aliased to --debug[=[host:]port.

Specify ways of the inspector web socket url exposure.

By default inspector websocket url is available in stderr and under /json/list endpoint on http://host:port/json/list.

Enable support for devtools network inspector events, for visibility into requests made by the nodejs http and https modules.

Silence deprecation warnings.

Throw errors for deprecations.

Print stack traces for deprecations.

Print stack traces for process warnings (including deprecations).

Set the default value of the verbatim parameter in the Node.js dns.lookup() and dnsPromises.lookup() functions. The value could be:

The default is verbatim and dns.setDefaultResultOrder() have higher priority than --dns-result-order.

Set the directory to which all Node.js diagnostic output files are written. Defaults to current working directory.

Affects the default output directory of v8.setHeapSnapshotNearHeapLimit.

Disable exposition of Navigator API on the global scope from Node.js.

There isn't a documented list of all Chromium switches, but there are a few ways to find them.

The easiest way is through Chromium's flags page, which you can access at about://flags. These flags don't directly match switch names, but they show up in the process's command-line arguments.

To see these arguments, enable a flag in about://flags, then go to about://version in Chromium. You'll find a list of command-line arguments, including --flag-switches-begin --your --list --flag-switches-end, which contains the list of your flag enabled switches.

Most flags are included as part of --enable-features=, but some are standalone switches, like --enable-experimental-web-platform-features.

A complete list of flags exists in Chromium's flag metadata page, but this list includes platform, environment and GPU specific, expired and potentially non-functional flags, so many of them might not always work in every situation.

Keep in mind that standalone switches can sometimes be split into individual features, so there's no fully complete list of switches.

Finally, you'll need to ensure that the version of Chromium in Electron matches the version of the browser you're using to cross-reference the switches.

**Examples:**

Example 1 (javascript):
```javascript
const { app } = require('electron')app.commandLine.appendSwitch('remote-debugging-port', '8315')app.commandLine.appendSwitch('host-rules', 'MAP * 127.0.0.1')app.whenReady().then(() => {  // Your code here})
```

Example 2 (unknown):
```unknown
--auth-server-whitelist='*example.com, *foobar.com, *baz'
```

Example 3 (unknown):
```unknown
$ electron --js-flags="--harmony_proxies --harmony_collections" your-app
```

Example 4 (unknown):
```unknown
$ electron --js-flags="--trace-opt --trace-deopt" your-app
```

---

## Class: IncomingMessage

**URL:** https://www.electronjs.org/docs/latest/api/incoming-message

**Contents:**
- Class: IncomingMessage
- Class: IncomingMessage​
  - Instance Events​
    - Event: 'data'​
    - Event: 'end'​
    - Event: 'aborted'​
    - Event: 'error'​
  - Instance Properties​
    - response.statusCode​
    - response.statusMessage​

Handle responses to HTTP/HTTPS requests.

Process: Main, Utility This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

IncomingMessage implements the Readable Stream interface and is therefore an EventEmitter.

The data event is the usual method of transferring response data into applicative code.

Indicates that response body has ended. Must be placed before 'data' event.

Emitted when a request has been canceled during an ongoing HTTP transaction.

Emitted when an error was encountered while streaming response data events. For instance, if the server closes the underlying while the response is still streaming, an error event will be emitted on the response object and a close event will subsequently follow on the request object.

An IncomingMessage instance has the following readable properties:

An Integer indicating the HTTP response status code.

A string representing the HTTP status message.

A Record<string, string | string[]> representing the HTTP response headers. The headers object is formatted as follows:

A string indicating the HTTP protocol version number. Typical values are '1.0' or '1.1'. Additionally httpVersionMajor and httpVersionMinor are two Integer-valued readable properties that return respectively the HTTP major and minor version numbers.

An Integer indicating the HTTP protocol major version number.

An Integer indicating the HTTP protocol minor version number.

A string[] containing the raw HTTP response headers exactly as they were received. The keys and values are in the same list. It is not a list of tuples. So, the even-numbered offsets are key values, and the odd-numbered offsets are the associated values. Header names are not lowercased, and duplicates are not merged.

**Examples:**

Example 1 (javascript):
```javascript
// Prints something like://// [ 'user-agent',//   'this is invalid because there can be only one',//   'User-Agent',//   'curl/7.22.0',//   'Host',//   '127.0.0.1:8000',//   'ACCEPT',//   '*/*' ]console.log(response.rawHeaders)
```

---

## Rectangle Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/rectangle

**Contents:**
- Rectangle Object

---

## pushNotifications

**URL:** https://www.electronjs.org/docs/latest/api/push-notifications

**Contents:**
- pushNotifications
- Events​
    - Event: 'received-apns-notification' macOS​
- Methods​
  - pushNotifications.registerForAPNSNotifications() macOS​
  - pushNotifications.unregisterForAPNSNotifications() macOS​

Register for and receive notifications from remote push notification services

For example, when registering for push notifications via Apple push notification services (APNS):

The pushNotification module emits the following events:

Emitted when the app receives a remote notification while running. See: https://developer.apple.com/documentation/appkit/nsapplicationdelegate/1428430-application?language=objc

The pushNotification module has the following methods:

Returns Promise<string>

Registers the app with Apple Push Notification service (APNS) to receive Badge, Sound, and Alert notifications. If registration is successful, the promise will be resolved with the APNS device token. Otherwise, the promise will be rejected with an error message. See: https://developer.apple.com/documentation/appkit/nsapplication/1428476-registerforremotenotificationtyp?language=objc

Unregisters the app from notifications received from APNS.

Apps unregistered through this method can always reregister.

See: https://developer.apple.com/documentation/appkit/nsapplication/1428747-unregisterforremotenotifications?language=objc

**Examples:**

Example 1 (javascript):
```javascript
const { pushNotifications, Notification } = require('electron')pushNotifications.registerForAPNSNotifications().then((token) => {  // forward token to your remote notification server})pushNotifications.on('received-apns-notification', (event, userInfo) => {  // generate a new Notification object with the relevant userInfo fields})
```

---

## Class: Debugger

**URL:** https://www.electronjs.org/docs/latest/api/debugger

**Contents:**
- Class: Debugger
- Class: Debugger​
  - Instance Events​
    - Event: 'detach'​
    - Event: 'message'​
  - Instance Methods​
    - debugger.attach([protocolVersion])​
    - debugger.isAttached()​
    - debugger.detach()​
    - debugger.sendCommand(method[, commandParams, sessionId])​

An alternate transport for Chrome's remote debugging protocol.

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

Chrome Developer Tools has a special binding available at JavaScript runtime that allows interacting with pages and instrumenting them.

Emitted when the debugging session is terminated. This happens either when webContents is closed or devtools is invoked for the attached webContents.

Emitted whenever the debugging target issues an instrumentation event.

Attaches the debugger to the webContents.

Returns boolean - Whether a debugger is attached to the webContents.

Detaches the debugger from the webContents.

Returns Promise<any> - A promise that resolves with the response defined by the 'returns' attribute of the command description in the remote debugging protocol or is rejected indicating the failure of the command.

Send given command to the debugging target.

**Examples:**

Example 1 (javascript):
```javascript
const { BrowserWindow } = require('electron')const win = new BrowserWindow()try {  win.webContents.debugger.attach('1.1')} catch (err) {  console.log('Debugger attach failed : ', err)}win.webContents.debugger.on('detach', (event, reason) => {  console.log('Debugger detached due to : ', reason)})win.webContents.debugger.on('message', (event, method, params) => {  if (method === 'Network.requestWillBeSent') {    if (params.request.url === 'https://www.github.com') {      win.webContents.debugger.detach()    }  }})win.webContents.debugger.sendCommand('Network.enable')
```

---

## Class: ServiceWorkers

**URL:** https://www.electronjs.org/docs/latest/api/service-workers

**Contents:**
- Class: ServiceWorkers
- Class: ServiceWorkers​
  - Instance Events​
    - Event: 'console-message'​
    - Event: 'registration-completed'​
    - Event: 'running-status-changed' Experimental​
  - Instance Methods​
    - serviceWorkers.getAllRunning()​
    - serviceWorkers.getInfoFromVersionID(versionId)​
    - serviceWorkers.getFromVersionID(versionId) Deprecated​

Query and receive events from a sessions active service workers.

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

Instances of the ServiceWorkers class are accessed by using serviceWorkers property of a Session.

The following events are available on instances of ServiceWorkers:

Emitted when a service worker logs something to the console.

Emitted when a service worker has been registered. Can occur after a call to navigator.serviceWorker.register('/sw.js') successfully resolves or when a Chrome extension is loaded.

Emitted when a service worker's running status has changed.

The following methods are available on instances of ServiceWorkers:

Returns Record<number, ServiceWorkerInfo> - A ServiceWorkerInfo object where the keys are the service worker version ID and the values are the information about that service worker.

Returns ServiceWorkerInfo - Information about this service worker

If the service worker does not exist or is not running this method will throw an exception.

Returns ServiceWorkerInfo - Information about this service worker

If the service worker does not exist or is not running this method will throw an exception.

Deprecated: Use the new serviceWorkers.getInfoFromVersionID API.

Returns ServiceWorkerMain | undefined - Instance of the service worker associated with the given version ID. If there's no associated version, or its running status has changed to 'stopped', this will return undefined.

Returns Promise<ServiceWorkerMain> - Resolves with the service worker when it's started.

Starts the service worker or does nothing if already running.

**Examples:**

Example 1 (javascript):
```javascript
const { session } = require('electron')// Get all service workers.console.log(session.defaultSession.serviceWorkers.getAllRunning())// Handle logs and get service worker infosession.defaultSession.serviceWorkers.on('console-message', (event, messageDetails) => {  console.log(    'Got service worker message',    messageDetails,    'from',    session.defaultSession.serviceWorkers.getFromVersionID(messageDetails.versionId)  )})
```

Example 2 (javascript):
```javascript
const { app, session } = require('electron')const { serviceWorkers } = session.defaultSession// Collect service workers scopesconst workerScopes = Object.values(serviceWorkers.getAllRunning()).map((info) => info.scope)app.on('browser-window-created', async (event, window) => {  for (const scope of workerScopes) {    try {      // Ensure worker is started      const serviceWorker = await serviceWorkers.startWorkerForScope(scope)      serviceWorker.send('window-created', { windowId: window.id })    } catch (error) {      console.error(`Failed to start service worker for ${scope}`)      console.error(error)    }  }})
```

---

## Referrer Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/referrer

**Contents:**
- Referrer Object

---

## MouseWheelInputEvent Object extends MouseInputEvent

**URL:** https://www.electronjs.org/docs/latest/api/structures/mouse-wheel-input-event

**Contents:**
- MouseWheelInputEvent Object extends MouseInputEvent

---

## IpcMainServiceWorkerInvokeEvent Object extends Event

**URL:** https://www.electronjs.org/docs/latest/api/structures/ipc-main-service-worker-invoke-event

**Contents:**
- IpcMainServiceWorkerInvokeEvent Object extends Event

---

## WindowSessionEndEvent Object extends Event

**URL:** https://www.electronjs.org/docs/latest/api/structures/window-session-end-event

**Contents:**
- WindowSessionEndEvent Object extends Event

Unfortunately, Windows does not offer a way to differentiate between a shutdown and a reboot, meaning the 'shutdown' reason is triggered in both scenarios. For more details on the WM_ENDSESSION message and its associated reasons, refer to the MSDN documentation.

---

## CustomScheme Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/custom-scheme

**Contents:**
- CustomScheme Object

---

## Class: DownloadItem

**URL:** https://www.electronjs.org/docs/latest/api/download-item

**Contents:**
- Class: DownloadItem
- Class: DownloadItem​
  - Instance Events​
    - Event: 'updated'​
    - Event: 'done'​
  - Instance Methods​
    - downloadItem.setSavePath(path)​
    - downloadItem.getSavePath()​
    - downloadItem.setSaveDialogOptions(options)​
    - downloadItem.getSaveDialogOptions()​

Control file downloads from remote sources.

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

DownloadItem is an EventEmitter that represents a download item in Electron. It is used in will-download event of Session class, and allows users to control the download item.

Emitted when the download has been updated and is not done.

The state can be one of following:

Emitted when the download is in a terminal state. This includes a completed download, a cancelled download (via downloadItem.cancel()), and interrupted download that can't be resumed.

The state can be one of following:

The downloadItem object has the following methods:

The API is only available in session's will-download callback function. If path doesn't exist, Electron will try to make the directory recursively. If user doesn't set the save path via the API, Electron will use the original routine to determine the save path; this usually prompts a save dialog.

Returns string - The save path of the download item. This will be either the path set via downloadItem.setSavePath(path) or the path selected from the shown save dialog.

This API allows the user to set custom options for the save dialog that opens for the download item by default. The API is only available in session's will-download callback function.

Returns SaveDialogOptions - Returns the object previously set by downloadItem.setSaveDialogOptions(options).

Returns boolean - Whether the download is paused.

Resumes the download that has been paused.

To enable resumable downloads the server you are downloading from must support range requests and provide both Last-Modified and ETag header values. Otherwise resume() will dismiss previously received bytes and restart the download from the beginning.

Returns boolean - Whether the download can resume.

Cancels the download operation.

Returns string - The origin URL where the item is downloaded from.

Returns string - The files mime type.

Returns boolean - Whether the download has user gesture.

Returns string - The file name of the download item.

The file name is not always the same as the actual one saved in local disk. If user changes the file name in a prompted download saving dialog, the actual name of saved file will be different.

Returns Integer - The current download speed in bytes per second.

Returns Integer - The total size in bytes of the download item.

If the size is unknown, it returns 0.

Returns Integer - The received bytes of the download item.

Returns Integer - The download completion in percent.

Returns string - The Content-Disposition field from the response header.

Returns string - The current state. Can be progressing, completed, cancelled or interrupted.

The following methods are useful specifically to resume a cancelled item when session is restarted.

Returns string[] - The complete URL chain of the item including any redirects.

Returns string - Last-Modified header value.

Returns string - ETag header value.

Returns Double - Number of seconds since the UNIX epoch when the download was started.

Returns Double - Number of seconds since the UNIX epoch when the download ended.

A string property that determines the save file path of the download item.

The property is only available in session's will-download callback function. If user doesn't set the save path via the property, Electron will use the original routine to determine the save path; this usually prompts a save dialog.

**Examples:**

Example 1 (javascript):
```javascript
// In the main process.const { BrowserWindow } = require('electron')const win = new BrowserWindow()win.webContents.session.on('will-download', (event, item, webContents) => {  // Set the save path, making Electron not to prompt a save dialog.  item.setSavePath('/tmp/save.pdf')  item.on('updated', (event, state) => {    if (state === 'interrupted') {      console.log('Download is interrupted but can be resumed')    } else if (state === 'progressing') {      if (item.isPaused()) {        console.log('Download is paused')      } else {        console.log(`Received bytes: ${item.getReceivedBytes()}`)      }    }  })  item.once('done', (event, state) => {    if (state === 'completed') {      console.log('Download successfully')    } else {      console.log(`Download failed: ${state}`)    }  })})
```

---

## ShareMenu

**URL:** https://www.electronjs.org/docs/latest/api/share-menu

**Contents:**
- ShareMenu
- Class: ShareMenu​
  - new ShareMenu(sharingItem)​
  - Instance Methods​
    - shareMenu.popup([options])​
    - shareMenu.closePopup([browserWindow])​

The ShareMenu class creates Share Menu on macOS, which can be used to share information from the current context to apps, social media accounts, and other services.

For including the share menu as a submenu of other menus, please use the shareMenu role of MenuItem.

Create share menu on macOS.

Electron's built-in classes cannot be subclassed in user code. For more information, see the FAQ.

Creates a new share menu.

The shareMenu object has the following instance methods:

Pops up this menu as a context menu in the BrowserWindow.

Closes the context menu in the browserWindow.

---

## dialog

**URL:** https://www.electronjs.org/docs/latest/api/dialog

**Contents:**
- dialog
- Methods​
  - dialog.showOpenDialogSync([window, ]options)​
  - dialog.showOpenDialog([window, ]options)​
  - dialog.showSaveDialogSync([window, ]options)​
  - dialog.showSaveDialog([window, ]options)​
  - dialog.showMessageBoxSync([window, ]options)​
  - dialog.showMessageBox([window, ]options)​
  - dialog.showErrorBox(title, content)​
  - dialog.showCertificateTrustDialog([window, ]options) macOS Windows​

Display native system dialogs for opening and saving files, alerting, etc.

An example of showing a dialog to select multiple files:

The dialog module has the following methods:

Returns string[] | undefined, the file paths chosen by the user; if the dialog is cancelled it returns undefined.

The window argument allows the dialog to attach itself to a parent window, making it modal.

The filters specifies an array of file types that can be displayed or selected when you want to limit the user to a specific type. For example:

The extensions array should contain extensions without wildcards or dots (e.g. 'png' is good but '.png' and '*.png' are bad). To show all files, use the '*' wildcard (no other wildcard is supported).

On Windows and Linux an open dialog can not be both a file selector and a directory selector, so if you set properties to ['openFile', 'openDirectory'] on these platforms, a directory selector will be shown.

On Linux defaultPath is not supported when using portal file chooser dialogs unless the portal backend is version 4 or higher. You can use --xdg-portal-required-version command-line switch to force gtk or kde dialogs.

Returns Promise<Object> - Resolve with an object containing the following:

The window argument allows the dialog to attach itself to a parent window, making it modal.

The filters specifies an array of file types that can be displayed or selected when you want to limit the user to a specific type. For example:

The extensions array should contain extensions without wildcards or dots (e.g. 'png' is good but '.png' and '*.png' are bad). To show all files, use the '*' wildcard (no other wildcard is supported).

On Windows and Linux an open dialog can not be both a file selector and a directory selector, so if you set properties to ['openFile', 'openDirectory'] on these platforms, a directory selector will be shown.

On Linux defaultPath is not supported when using portal file chooser dialogs unless the portal backend is version 4 or higher. You can use --xdg-portal-required-version command-line switch to force gtk or kde dialogs.

Returns string, the path of the file chosen by the user; if the dialog is cancelled it returns an empty string.

The window argument allows the dialog to attach itself to a parent window, making it modal.

The filters specifies an array of file types that can be displayed, see dialog.showOpenDialog for an example.

Returns Promise<Object> - Resolve with an object containing the following:

The window argument allows the dialog to attach itself to a parent window, making it modal.

The filters specifies an array of file types that can be displayed, see dialog.showOpenDialog for an example.

On macOS, using the asynchronous version is recommended to avoid issues when expanding and collapsing the dialog.

Returns Integer - the index of the clicked button.

Shows a message box, it will block the process until the message box is closed. It returns the index of the clicked button.

The window argument allows the dialog to attach itself to a parent window, making it modal. If window is not shown dialog will not be attached to it. In such case it will be displayed as an independent window.

Returns Promise<Object> - resolves with a promise containing the following properties:

The window argument allows the dialog to attach itself to a parent window, making it modal.

Displays a modal dialog that shows an error message.

This API can be called safely before the ready event the app module emits, it is usually used to report errors in early stage of startup. If called before the app readyevent on Linux, the message will be emitted to stderr, and no GUI dialog will appear.

Returns Promise<void> - resolves when the certificate trust dialog is shown.

On macOS, this displays a modal dialog that shows a message and certificate information, and gives the user the option of trusting/importing the certificate. If you provide a window argument the dialog will be attached to the parent window, making it modal.

On Windows the options are more limited, due to the Win32 APIs used:

showOpenDialog and showSaveDialog resolve to an object with a bookmarks field. This field is an array of Base64 encoded strings that contain the security scoped bookmark data for the saved file. The securityScopedBookmarks option must be enabled for this to be present.

On macOS, dialogs are presented as sheets attached to a window if you provide a BaseWindow reference in the window parameter, or modals if no window is provided.

You can call BaseWindow.getCurrentWindow().setSheetOffset(offset) to change the offset from the window frame where sheets are attached.

**Examples:**

Example 1 (javascript):
```javascript
const { dialog } = require('electron')console.log(dialog.showOpenDialog({ properties: ['openFile', 'multiSelections'] }))
```

Example 2 (json):
```json
{  filters: [    { name: 'Images', extensions: ['jpg', 'png', 'gif'] },    { name: 'Movies', extensions: ['mkv', 'avi', 'mp4'] },    { name: 'Custom File Type', extensions: ['as'] },    { name: 'All Files', extensions: ['*'] }  ]}
```

Example 3 (css):
```css
dialog.showOpenDialogSync(mainWindow, {  properties: ['openFile', 'openDirectory']})
```

Example 4 (json):
```json
{  filters: [    { name: 'Images', extensions: ['jpg', 'png', 'gif'] },    { name: 'Movies', extensions: ['mkv', 'avi', 'mp4'] },    { name: 'Custom File Type', extensions: ['as'] },    { name: 'All Files', extensions: ['*'] }  ]}
```

---

## SharedWorkerInfo Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/shared-worker-info

**Contents:**
- SharedWorkerInfo Object

---

## Class: WebRequest

**URL:** https://www.electronjs.org/docs/latest/api/web-request

**Contents:**
- Class: WebRequest
- Class: WebRequest​
  - Instance Methods​
    - webRequest.onBeforeRequest([filter, ]listener)​
    - webRequest.onBeforeSendHeaders([filter, ]listener)​
    - webRequest.onSendHeaders([filter, ]listener)​
    - webRequest.onHeadersReceived([filter, ]listener)​
    - webRequest.onResponseStarted([filter, ]listener)​
    - webRequest.onBeforeRedirect([filter, ]listener)​
    - webRequest.onCompleted([filter, ]listener)​

Intercept and modify the contents of a request at various stages of its lifetime.

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

Instances of the WebRequest class are accessed by using the webRequest property of a Session.

The methods of WebRequest accept an optional filter and a listener. The listener will be called with listener(details) when the API's event has happened. The details object describes the request.

⚠️ Only the last attached listener will be used. Passing null as listener will unsubscribe from the event.

The filter object has a urls property which is an Array of URL patterns that will be used to filter out the requests that do not match the URL patterns. If the filter is omitted then all requests will be matched.

For certain events the listener is passed with a callback, which should be called with a response object when listener has done its work.

An example of adding User-Agent header for requests:

The following methods are available on instances of WebRequest:

The listener will be called with listener(details, callback) when a request is about to occur.

The uploadData is an array of UploadData objects.

The callback has to be called with an response object.

Some examples of valid urls:

The listener will be called with listener(details, callback) before sending an HTTP request, once the request headers are available. This may occur after a TCP connection is made to the server, but before any http data is sent.

The callback has to be called with a response object.

The listener will be called with listener(details) just before a request is going to be sent to the server, modifications of previous onBeforeSendHeaders response are visible by the time this listener is fired.

The listener will be called with listener(details, callback) when HTTP response headers of a request have been received.

The callback has to be called with a response object.

The listener will be called with listener(details) when first byte of the response body is received. For HTTP requests, this means that the status line and response headers are available.

The listener will be called with listener(details) when a server initiated redirect is about to occur.

The listener will be called with listener(details) when a request is completed.

The listener will be called with listener(details) when an error occurs.

**Examples:**

Example 1 (javascript):
```javascript
const { session } = require('electron')// Modify the user agent for all requests to the following urls.const filter = {  urls: ['https://*.github.com/*', '*://electron.github.io/*']}session.defaultSession.webRequest.onBeforeSendHeaders(filter, (details, callback) => {  details.requestHeaders['User-Agent'] = 'MyAgent'  callback({ requestHeaders: details.requestHeaders })})
```

Example 2 (typescript):
```typescript
'<all_urls>''http://foo:1234/''http://foo.com/''http://foo:1234/bar''*://*/*''*://example.com/*''*://example.com/foo/*''http://*.foo:1234/''file://foo:1234/bar''http://foo:*/''*://www.foo.com/'
```

---

## desktopCapturer

**URL:** https://www.electronjs.org/docs/latest/api/desktop-capturer

**Contents:**
- desktopCapturer
- Methods​
  - desktopCapturer.getSources(options)​
- Caveats​

Access information about media sources that can be used to capture audio and video from the desktop using the navigator.mediaDevices.getUserMedia API.

The following example shows how to capture video from a desktop window whose title is Electron:

See navigator.mediaDevices.getDisplayMedia for more information.

navigator.mediaDevices.getDisplayMedia does not permit the use of deviceId for selection of a source - see specification.

The desktopCapturer module has the following methods:

Returns Promise<DesktopCapturerSource[]> - Resolves with an array of DesktopCapturerSource objects, each DesktopCapturerSource represents a screen or an individual window that can be captured.

Capturing the screen contents requires user consent on macOS 10.15 Catalina or higher, which can detected by systemPreferences.getMediaAccessStatus.

desktopCapturer.getSources(options) only returns a single source on Linux when using Pipewire.

PipeWire supports a single capture for both screens and windows. If you request the window and screen type, the selected source will be returned as a window capture.

navigator.mediaDevices.getUserMedia does not work on macOS for audio capture due to a fundamental limitation whereby apps that want to access the system's audio require a signed kernel extension. Chromium, and by extension Electron, does not provide this.

It is possible to circumvent this limitation by capturing system audio with another macOS app like Soundflower and passing it through a virtual audio input device. This virtual device can then be queried with navigator.mediaDevices.getUserMedia.

**Examples:**

Example 1 (javascript):
```javascript
// main.jsconst { app, BrowserWindow, desktopCapturer, session } = require('electron')app.whenReady().then(() => {  const mainWindow = new BrowserWindow()  session.defaultSession.setDisplayMediaRequestHandler((request, callback) => {    desktopCapturer.getSources({ types: ['screen'] }).then((sources) => {      // Grant access to the first screen found.      callback({ video: sources[0], audio: 'loopback' })    })    // If true, use the system picker if available.    // Note: this is currently experimental. If the system picker    // is available, it will be used and the media request handler    // will not be invoked.  }, { useSystemPicker: true })  mainWindow.loadFile('index.html')})
```

Example 2 (javascript):
```javascript
// renderer.jsconst startButton = document.getElementById('startButton')const stopButton = document.getElementById('stopButton')const video = document.querySelector('video')startButton.addEventListener('click', () => {  navigator.mediaDevices.getDisplayMedia({    audio: true,    video: {      width: 320,      height: 240,      frameRate: 30    }  }).then(stream => {    video.srcObject = stream    video.onloadedmetadata = (e) => video.play()  }).catch(e => console.log(e))})stopButton.addEventListener('click', () => {  video.pause()})
```

Example 3 (html):
```html
<!-- index.html --><html><meta http-equiv="content-security-policy" content="script-src 'self' 'unsafe-inline'" />  <body>    <button id="startButton" class="button">Start</button>    <button id="stopButton" class="button">Stop</button>    <video width="320" height="240" autoplay></video>    <script src="renderer.js"></script>  </body></html>
```

---

## Menu

**URL:** https://www.electronjs.org/docs/latest/api/menu

**Contents:**
- Menu
- Class: Menu​
  - new Menu()​
  - Static Methods​
    - Menu.setApplicationMenu(menu)​
    - Menu.getApplicationMenu()​
    - Menu.sendActionToFirstResponder(action) macOS​
    - Menu.buildFromTemplate(template)​
  - Instance Methods​
    - menu.popup([options])​

Create native application menus and context menus.

See also: A detailed guide about how to implement menus in your application.

Electron's built-in classes cannot be subclassed in user code. For more information, see the FAQ.

The Menu class has the following static methods:

Sets menu as the application menu on macOS. On Windows and Linux, the menu will be set as each window's top menu.

Also on Windows and Linux, you can use a & in the top-level item name to indicate which letter should get a generated accelerator. For example, using &File for the file menu would result in a generated Alt-F accelerator that opens the associated menu. The indicated character in the button label then gets an underline, and the & character is not displayed on the button label.

In order to escape the & character in an item name, add a proceeding &. For example, &&File would result in &File displayed on the button label.

Passing null will suppress the default menu. On Windows and Linux, this has the additional effect of removing the menu bar from the window.

The default menu will be created automatically if the app does not set one. It contains standard items such as File, Edit, View, Window and Help.

Returns Menu | null - The application menu, if set, or null, if not set.

The returned Menu instance doesn't support dynamic addition or removal of menu items. Instance properties can still be dynamically modified.

Sends the action to the first responder of application. This is used for emulating default macOS menu behaviors. Usually you would use the role property of a MenuItem.

See the macOS Cocoa Event Handling Guide for more information on macOS' native actions.

Generally, the template is an array of options for constructing a MenuItem. The usage can be referenced above.

You can also attach other fields to the element of the template and they will become properties of the constructed menu items.

The menu object has the following instance methods:

Pops up this menu as a context menu in the BaseWindow.

For more details, see the Context Menu guide.

Closes the context menu in the window.

Appends the menuItem to the menu.

Returns MenuItem | null the item with the specified id

Inserts the menuItem to the pos position of the menu.

Objects created with new Menu or returned by Menu.buildFromTemplate emit the following events:

Some events are only available on specific operating systems and are labeled as such.

Emitted when menu.popup() is called.

Emitted when a popup is closed either manually or with menu.closePopup().

menu objects also have the following properties:

A MenuItem[] array containing the menu's items.

Each Menu consists of multiple MenuItem instances and each MenuItem can nest a Menu into its submenu property.

---

## Class: ServiceWorkerMain

**URL:** https://www.electronjs.org/docs/latest/api/service-worker-main

**Contents:**
- Class: ServiceWorkerMain
- Class: ServiceWorkerMain​
  - Instance Methods​
    - serviceWorker.isDestroyed() Experimental​
    - serviceWorker.send(channel, ...args) Experimental​
    - serviceWorker.startTask() Experimental​
  - Instance Properties​
    - serviceWorker.ipc Readonly Experimental​
    - serviceWorker.scope Readonly Experimental​
    - serviceWorker.scriptURL Readonly Experimental​

An instance of a Service Worker representing a version of a script for a given scope.

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

Returns boolean - Whether the service worker has been destroyed.

Send an asynchronous message to the service worker process via channel, along with arguments. Arguments will be serialized with the Structured Clone Algorithm, just like postMessage, so prototype chains will not be included. Sending Functions, Promises, Symbols, WeakMaps, or WeakSets will throw an exception.

The service worker process can handle the message by listening to channel with the ipcRenderer module.

Initiate a task to keep the service worker alive until ended.

An IpcMainServiceWorker instance scoped to the service worker.

A string representing the scope URL of the service worker.

A string representing the script URL of the service worker.

A number representing the ID of the specific version of the service worker script in its scope.

---

## ColorSpace Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/color-space

**Contents:**
- ColorSpace Object
- Common ColorSpace definitions​
  - Standard Color Spaces​
  - HDR Color Spaces​
  - Video Color Spaces​

primaries string - The color primaries of the color space. Can be one of the following values:

transfer string - The transfer function of the color space. Can be one of the following values:

matrix string - The color matrix of the color space. Can be one of the following values:

range string - The color range of the color space. Can be one of the following values:

Extended sRGB (extends sRGB to all real values):

scRGB Linear (linear transfer function for all real values):

scRGB Linear 80 Nits (with an SDR white level of 80 nits):

HDR10 (BT.2020 primaries with PQ transfer function):

HLG (BT.2020 primaries with HLG transfer function):

JPEG (typical color space for JPEG images):

**Examples:**

Example 1 (css):
```css
const cs = {  primaries: 'bt709',  transfer: 'srgb',  matrix: 'rgb',  range: 'full'}
```

Example 2 (css):
```css
const cs = {  primaries: 'p3',  transfer: 'srgb',  matrix: 'rgb',  range: 'full'}
```

Example 3 (css):
```css
const cs = {  primaries: 'xyz-d50',  transfer: 'linear',  matrix: 'rgb',  range: 'full'}
```

Example 4 (css):
```css
const cs = {  primaries: 'bt709',  transfer: 'srgb-hdr',  matrix: 'rgb',  range: 'full'}
```

---

## FilePathWithHeaders Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/file-path-with-headers

**Contents:**
- FilePathWithHeaders Object

---

## Class: Extensions

**URL:** https://www.electronjs.org/docs/latest/api/extensions-api

**Contents:**
- Class: Extensions
- Class: Extensions​
  - Instance Events​
    - Event: 'extension-loaded'​
    - Event: 'extension-unloaded'​
    - Event: 'extension-ready'​
  - Instance Methods​
    - extensions.loadExtension(path[, options])​
    - extensions.removeExtension(extensionId)​
    - extensions.getExtension(extensionId)​

Load and interact with extensions.

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

Instances of the Extensions class are accessed by using extensions property of a Session.

The following events are available on instances of Extensions:

Emitted after an extension is loaded. This occurs whenever an extension is added to the "enabled" set of extensions. This includes:

Emitted after an extension is unloaded. This occurs when Session.removeExtension is called.

Emitted after an extension is loaded and all necessary browser state is initialized to support the start of the extension's background page.

The following methods are available on instances of Extensions:

Returns Promise<Extension> - resolves when the extension is loaded.

This method will raise an exception if the extension could not be loaded. If there are warnings when installing the extension (e.g. if the extension requests an API that Electron does not support) then they will be logged to the console.

Note that Electron does not support the full range of Chrome extensions APIs. See Supported Extensions APIs for more details on what is supported.

Note that in previous versions of Electron, extensions that were loaded would be remembered for future runs of the application. This is no longer the case: loadExtension must be called on every boot of your app if you want the extension to be loaded.

This API does not support loading packed (.crx) extensions.

This API cannot be called before the ready event of the app module is emitted.

Loading extensions into in-memory (non-persistent) sessions is not supported and will throw an error.

Unloads an extension.

This API cannot be called before the ready event of the app module is emitted.

Returns Extension | null - The loaded extension with the given ID.

This API cannot be called before the ready event of the app module is emitted.

Returns Extension[] - A list of all loaded extensions.

This API cannot be called before the ready event of the app module is emitted.

**Examples:**

Example 1 (javascript):
```javascript
const { app, session } = require('electron')const path = require('node:path')app.whenReady().then(async () => {  await session.defaultSession.extensions.loadExtension(    path.join(__dirname, 'react-devtools'),    // allowFileAccess is required to load the devtools extension on file:// URLs.    { allowFileAccess: true }  )  // Note that in order to use the React DevTools extension, you'll need to  // download and unzip a copy of the extension.})
```

---

## Notification

**URL:** https://www.electronjs.org/docs/latest/api/notification

**Contents:**
- Notification
- Class: Notification​
  - Static Methods​
    - Notification.isSupported()​
  - new Notification([options])​
  - Instance Events​
    - Event: 'show'​
    - Event: 'click'​
    - Event: 'close'​
    - Event: 'reply' macOS​

Create OS desktop notifications

If you want to show notifications from a renderer process you should use the web Notifications API

Create OS desktop notifications

Notification is an EventEmitter.

It creates a new Notification with native properties as set by the options.

Electron's built-in classes cannot be subclassed in user code. For more information, see the FAQ.

The Notification class has the following static methods:

Returns boolean - Whether or not desktop notifications are supported on the current system

Objects created with new Notification emit the following events:

Some events are only available on specific operating systems and are labeled as such.

Emitted when the notification is shown to the user. Note that this event can be fired multiple times as a notification can be shown multiple times through the show() method.

Emitted when the notification is clicked by the user.

Emitted when the notification is closed by manual intervention from the user.

This event is not guaranteed to be emitted in all cases where the notification is closed.

On Windows, the close event can be emitted in one of three ways: programmatic dismissal with notification.close(), by the user closing the notification, or via system timeout. If a notification is in the Action Center after the initial close event is emitted, a call to notification.close() will remove the notification from the action center but the close event will not be emitted again.

Emitted when the user clicks the "Reply" button on a notification with hasReply: true.

Emitted when an error is encountered while creating and showing the native notification.

Objects created with the new Notification() constructor have the following instance methods:

Immediately shows the notification to the user. Unlike the web notification API, instantiating a new Notification() does not immediately show it to the user. Instead, you need to call this method before the OS will display it.

If the notification has been shown before, this method will dismiss the previously shown notification and create a new one with identical properties.

Dismisses the notification.

On Windows, calling notification.close() while the notification is visible on screen will dismiss the notification and remove it from the Action Center. If notification.close() is called after the notification is no longer visible on screen, calling notification.close() will try remove it from the Action Center.

A string property representing the title of the notification.

A string property representing the subtitle of the notification.

A string property representing the body of the notification.

A string property representing the reply placeholder of the notification.

A string property representing the sound of the notification.

A string property representing the close button text of the notification.

A boolean property representing whether the notification is silent.

A boolean property representing whether the notification has a reply action.

A string property representing the urgency level of the notification. Can be 'normal', 'critical', or 'low'.

Default is 'low' - see NotifyUrgency for more information.

A string property representing the type of timeout duration for the notification. Can be 'default' or 'never'.

If timeoutType is set to 'never', the notification never expires. It stays open until closed by the calling API or the user.

A NotificationAction[] property representing the actions of the notification.

A string property representing the custom Toast XML of the notification.

On macOS, you can specify the name of the sound you'd like to play when the notification is shown. Any of the default sounds (under System Preferences > Sound) can be used, in addition to custom sound files. Be sure that the sound file is copied under the app bundle (e.g., YourApp.app/Contents/Resources), or one of the following locations:

See the NSSound docs for more information.

---

## NotificationResponse Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/notification-response

**Contents:**
- NotificationResponse Object

---

## CPUUsage Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/cpu-usage

**Contents:**
- CPUUsage Object

---

## shell

**URL:** https://www.electronjs.org/docs/latest/api/shell

**Contents:**
- shell
- Methods​
  - shell.showItemInFolder(fullPath)​
  - shell.openPath(path)​
  - shell.openExternal(url[, options])​
  - shell.trashItem(path)​
  - shell.beep()​
  - shell.writeShortcutLink(shortcutPath[, operation], options) Windows​
  - shell.readShortcutLink(shortcutPath) Windows​

Manage files and URLs using their default applications.

Process: Main, Renderer (non-sandboxed only)

The shell module provides functions related to desktop integration.

An example of opening a URL in the user's default browser:

While the shell module can be used in the renderer process, it will not function in a sandboxed renderer.

The shell module has the following methods:

Show the given file in a file manager. If possible, select the file.

Returns Promise<string> - Resolves with a string containing the error message corresponding to the failure if a failure occurred, otherwise "".

Open the given file in the desktop's default manner.

Returns Promise<void>

Open the given external protocol URL in the desktop's default manner. (For example, mailto: URLs in the user's default mail agent).

Returns Promise<void> - Resolves when the operation has been completed. Rejects if there was an error while deleting the requested item.

This moves a path to the OS-specific trash location (Trash on macOS, Recycle Bin on Windows, and a desktop-environment-specific location on Linux).

Returns boolean - Whether the shortcut was created successfully.

Creates or updates a shortcut link at shortcutPath.

Returns ShortcutDetails

Resolves the shortcut link at shortcutPath.

An exception will be thrown when any error happens.

**Examples:**

Example 1 (javascript):
```javascript
const { shell } = require('electron')shell.openExternal('https://github.com')
```

---

## ProtocolResponseUploadData Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/protocol-response-upload-data

**Contents:**
- ProtocolResponseUploadData Object

---

## ResolvedEndpoint Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/resolved-endpoint

**Contents:**
- ResolvedEndpoint Object

---

## Class: TouchBarColorPicker

**URL:** https://www.electronjs.org/docs/latest/api/touch-bar-color-picker

**Contents:**
- Class: TouchBarColorPicker
- Class: TouchBarColorPicker​
  - new TouchBarColorPicker(options)​
  - Instance Properties​
    - touchBarColorPicker.availableColors​
    - touchBarColorPicker.selectedColor​

Create a color picker in the touch bar for native macOS applications

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

The following properties are available on instances of TouchBarColorPicker:

A string[] array representing the color picker's available colors to select. Changing this value immediately updates the color picker in the touch bar.

A string hex code representing the color picker's currently selected color. Changing this value immediately updates the color picker in the touch bar.

---

## Class: IpcMainServiceWorker

**URL:** https://www.electronjs.org/docs/latest/api/ipc-main-service-worker

**Contents:**
- Class: IpcMainServiceWorker
- Class: IpcMainServiceWorker​
  - Instance Methods​
    - ipcMainServiceWorker.on(channel, listener)​
    - ipcMainServiceWorker.once(channel, listener)​
    - ipcMainServiceWorker.removeListener(channel, listener)​
    - ipcMainServiceWorker.removeAllListeners([channel])​
    - ipcMainServiceWorker.handle(channel, listener)​
    - ipcMainServiceWorker.handleOnce(channel, listener)​
    - ipcMainServiceWorker.removeHandler(channel)​

Communicate asynchronously from the main process to service workers.

This API is a subtle variation of IpcMain—targeted for communicating with service workers. For communicating with web frames, consult the IpcMain documentation.

Electron's built-in classes cannot be subclassed in user code. For more information, see the FAQ.

Listens to channel, when a new message arrives listener would be called with listener(event, args...).

Adds a one time listener function for the event. This listener is invoked only the next time a message is sent to channel, after which it is removed.

Removes the specified listener from the listener array for the specified channel.

Removes listeners of the specified channel.

Handles a single invokeable IPC message, then removes the listener. See ipcMainServiceWorker.handle(channel, listener).

Removes any handler for channel, if present.

---

## ScrubberItem Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/scrubber-item

**Contents:**
- ScrubberItem Object

---

## <webview> Tag

**URL:** https://www.electronjs.org/docs/latest/api/webview-tag

**Contents:**
- <webview> Tag
- Warning​
- Enabling​
- Overview​
- Example​
- Internal implementation​
- CSS Styling Notes​
- Tag Attributes​
  - src​
  - nodeintegration​

Electron's webview tag is based on Chromium's webview, which is undergoing dramatic architectural changes. This impacts the stability of webviews, including rendering, navigation, and event routing. We currently recommend to not use the webview tag and to consider alternatives, like iframe, a WebContentsView, or an architecture that avoids embedded content altogether.

By default the webview tag is disabled in Electron >= 5. You need to enable the tag by setting the webviewTag webPreferences option when constructing your BrowserWindow. For more information see the BrowserWindow constructor docs.

Display external web content in an isolated frame and process.

Process: Renderer This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

Use the webview tag to embed 'guest' content (such as web pages) in your Electron app. The guest content is contained within the webview container. An embedded page within your app controls how the guest content is laid out and rendered.

Unlike an iframe, the webview runs in a separate process than your app. It doesn't have the same permissions as your web page and all interactions between your app and embedded content will be asynchronous. This keeps your app safe from the embedded content.

Most methods called on the webview from the host page require a synchronous call to the main process.

To embed a web page in your app, add the webview tag to your app's embedder page (this is the app page that will display the guest content). In its simplest form, the webview tag includes the src of the web page and css styles that control the appearance of the webview container:

If you want to control the guest content in any way, you can write JavaScript that listens for webview events and responds to those events using the webview methods. Here's sample code with two event listeners: one that listens for the web page to start loading, the other for the web page to stop loading, and displays a "loading..." message during the load time:

Under the hood webview is implemented with Out-of-Process iframes (OOPIFs). The webview tag is essentially a custom element using shadow DOM to wrap an iframe element inside it.

So the behavior of webview is very similar to a cross-domain iframe, as examples:

Please note that the webview tag's style uses display:flex; internally to ensure the child iframe element fills the full height and width of its webview container when used with traditional and flexbox layouts. Please do not overwrite the default display:flex; CSS property, unless specifying display:inline-flex; for inline layout.

The webview tag has the following attributes:

A string representing the visible URL. Writing to this attribute initiates top-level navigation.

Assigning src its own value will reload the current page.

The src attribute can also accept data URLs, such as data:text/plain,Hello, world!.

A boolean. When this attribute is present the guest page in webview will have node integration and can use node APIs like require and process to access low level system resources. Node integration is disabled by default in the guest page.

A boolean for the experimental option for enabling NodeJS support in sub-frames such as iframes inside the webview. All your preloads will load for every iframe, you can use process.isMainFrame to determine if you are in the main frame or not. This option is disabled by default in the guest page.

A boolean. When this attribute is present the guest page in webview will be able to use browser plugins. Plugins are disabled by default.

A string that specifies a script that will be loaded before other scripts run in the guest page. The protocol of script's URL must be file: (even when using asar: archives) because it will be loaded by Node's require under the hood, which treats asar: archives as virtual directories.

When the guest page doesn't have node integration this script will still have access to all Node APIs, but global objects injected by Node will be deleted after this script has finished executing.

A string that sets the referrer URL for the guest page.

A string that sets the user agent for the guest page before the page is navigated to. Once the page is loaded, use the setUserAgent method to change the user agent.

A boolean. When this attribute is present the guest page will have web security disabled. Web security is enabled by default.

This value can only be modified before the first navigation.

A string that sets the session used by the page. If partition starts with persist:, the page will use a persistent session available to all pages in the app with the same partition. if there is no persist: prefix, the page will use an in-memory session. By assigning the same partition, multiple pages can share the same session. If the partition is unset then default session of the app will be used.

This value can only be modified before the first navigation, since the session of an active renderer process cannot change. Subsequent attempts to modify the value will fail with a DOM exception.

A boolean. When this attribute is present the guest page will be allowed to open new windows. Popups are disabled by default.

A string which is a comma separated list of strings which specifies the web preferences to be set on the webview. The full list of supported preference strings can be found in BrowserWindow.

The string follows the same format as the features string in window.open. A name by itself is given a true boolean value. A preference can be set to another value by including an =, followed by the value. Special values yes and 1 are interpreted as true, while no and 0 are interpreted as false.

A string which is a list of strings which specifies the blink features to be enabled separated by ,. The full list of supported feature strings can be found in the RuntimeEnabledFeatures.json5 file.

A string which is a list of strings which specifies the blink features to be disabled separated by ,. The full list of supported feature strings can be found in the RuntimeEnabledFeatures.json5 file.

The webview tag has the following methods:

The webview element must be loaded before using the methods.

Returns Promise<void> - The promise will resolve when the page has finished loading (see did-finish-load), and rejects if the page fails to load (see did-fail-load).

Loads the url in the webview, the url must contain the protocol prefix, e.g. the http:// or file://.

Initiates a download of the resource at url without navigating.

Returns string - The URL of guest page.

Returns string - The title of guest page.

Returns boolean - Whether guest page is still loading resources.

Returns boolean - Whether the main frame (and not just iframes or frames within it) is still loading.

Returns boolean - Whether the guest page is waiting for a first-response for the main resource of the page.

Stops any pending navigation.

Reloads the guest page.

Reloads the guest page and ignores cache.

Returns boolean - Whether the guest page can go back.

Returns boolean - Whether the guest page can go forward.

Returns boolean - Whether the guest page can go to offset.

Clears the navigation history.

Makes the guest page go back.

Makes the guest page go forward.

Navigates to the specified absolute index.

Navigates to the specified offset from the "current entry".

Returns boolean - Whether the renderer process has crashed.

Overrides the user agent for the guest page.

Returns string - The user agent for guest page.

Returns Promise<string> - A promise that resolves with a key for the inserted CSS that can later be used to remove the CSS via <webview>.removeInsertedCSS(key).

Injects CSS into the current web page and returns a unique key for the inserted stylesheet.

Returns Promise<void> - Resolves if the removal was successful.

Removes the inserted CSS from the current web page. The stylesheet is identified by its key, which is returned from <webview>.insertCSS(css).

Returns Promise<any> - A promise that resolves with the result of the executed code or is rejected if the result of the code is a rejected promise.

Evaluates code in page. If userGesture is set, it will create the user gesture context in the page. HTML APIs like requestFullScreen, which require user action, can take advantage of this option for automation.

Opens a DevTools window for guest page.

Closes the DevTools window of guest page.

Returns boolean - Whether guest page has a DevTools window attached.

Returns boolean - Whether DevTools window of guest page is focused.

Starts inspecting element at position (x, y) of guest page.

Opens the DevTools for the shared worker context present in the guest page.

Opens the DevTools for the service worker context present in the guest page.

Set guest page muted.

Returns boolean - Whether guest page has been muted.

Returns boolean - Whether audio is currently playing.

Executes editing command undo in page.

Executes editing command redo in page.

Executes editing command cut in page.

Executes editing command copy in page.

Centers the current text selection in page.

Executes editing command paste in page.

Executes editing command pasteAndMatchStyle in page.

Executes editing command delete in page.

Executes editing command selectAll in page.

Executes editing command unselect in page.

Scrolls to the top of the current <webview>.

Scrolls to the bottom of the current <webview>.

Adjusts the current text selection starting and ending points in the focused frame by the given amounts. A negative amount moves the selection towards the beginning of the document, and a positive amount moves the selection towards the end of the document.

See webContents.adjustSelection for examples.

Executes editing command replace in page.

Executes editing command replaceMisspelling in page.

Returns Promise<void>

Inserts text to the focused element.

Returns Integer - The request id used for the request.

Starts a request to find all matches for the text in the web page. The result of the request can be obtained by subscribing to found-in-page event.

Stops any findInPage request for the webview with the provided action.

Returns Promise<void>

Prints webview's web page. Same as webContents.print([options]).

Returns Promise<Uint8Array> - Resolves with the generated PDF data.

Prints webview's web page as PDF, Same as webContents.printToPDF(options).

Returns Promise<NativeImage> - Resolves with a NativeImage

Captures a snapshot of the page within rect. Omitting rect will capture the whole visible page.

Returns Promise<void>

Send an asynchronous message to renderer process via channel, you can also send arbitrary arguments. The renderer process can handle the message by listening to the channel event with the ipcRenderer module.

See webContents.send for examples.

Returns Promise<void>

Send an asynchronous message to renderer process via channel, you can also send arbitrary arguments. The renderer process can handle the message by listening to the channel event with the ipcRenderer module.

See webContents.sendToFrame for examples.

Returns Promise<void>

Sends an input event to the page.

See webContents.sendInputEvent for detailed description of event object.

Changes the zoom factor to the specified factor. Zoom factor is zoom percent divided by 100, so 300% = 3.0.

Changes the zoom level to the specified level. The original size is 0 and each increment above or below represents zooming 20% larger or smaller to default limits of 300% and 50% of original size, respectively. The formula for this is scale := 1.2 ^ level.

The zoom policy at the Chromium level is same-origin, meaning that the zoom level for a specific domain propagates across all instances of windows with the same domain. Differentiating the window URLs will make zoom work per-window.

Returns number - the current zoom factor.

Returns number - the current zoom level.

Returns Promise<void>

Sets the maximum and minimum pinch-to-zoom level.

Shows pop-up dictionary that searches the selected word on the page.

Returns number - The WebContents ID of this webview.

The following DOM events are available to the webview tag:

Fired when a load has committed. This includes navigation within the current document as well as subframe document-level loads, but does not include asynchronous resource loads.

Fired when the navigation is done, i.e. the spinner of the tab will stop spinning, and the onload event is dispatched.

This event is like did-finish-load, but fired when the load failed or was cancelled, e.g. window.stop() is invoked.

Fired when a frame has done navigation.

Corresponds to the points in time when the spinner of the tab starts spinning.

Corresponds to the points in time when the spinner of the tab stops spinning.

Fired when attached to the embedder web contents.

Fired when document in the given frame is loaded.

Fired when page title is set during navigation. explicitSet is false when title is synthesized from file url.

Fired when page receives favicon urls.

Fired when page enters fullscreen triggered by HTML API.

Fired when page leaves fullscreen triggered by HTML API.

Fired when the guest window logs a console message.

The following example code forwards all log messages to the embedder's console without regard for log level or other properties.

Fired when a result is available for webview.findInPage request.

Emitted when a user or the page wants to start navigation. It can happen when the window.location object is changed or a user clicks a link in the page.

This event will not emit when the navigation is started programmatically with APIs like <webview>.loadURL and <webview>.back.

It is also not emitted during in-page navigation, such as clicking anchor links or updating the window.location.hash. Use did-navigate-in-page event for this purpose.

Calling event.preventDefault() does NOT have any effect.

Emitted when a user or the page wants to start navigation anywhere in the <webview> or any frames embedded within. It can happen when the window.location object is changed or a user clicks a link in the page.

This event will not emit when the navigation is started programmatically with APIs like <webview>.loadURL and <webview>.back.

It is also not emitted during in-page navigation, such as clicking anchor links or updating the window.location.hash. Use did-navigate-in-page event for this purpose.

Calling event.preventDefault() does NOT have any effect.

Emitted when any frame (including main) starts navigating. isInPlace will be true for in-page navigations.

Emitted after a server side redirect occurs during navigation. For example a 302 redirect.

Emitted when a navigation is done.

This event is not emitted for in-page navigations, such as clicking anchor links or updating the window.location.hash. Use did-navigate-in-page event for this purpose.

Emitted when any frame navigation is done.

This event is not emitted for in-page navigations, such as clicking anchor links or updating the window.location.hash. Use did-navigate-in-page event for this purpose.

Emitted when an in-page navigation happened.

When in-page navigation happens, the page URL changes but does not cause navigation outside of the page. Examples of this occurring are when anchor links are clicked or when the DOM hashchange event is triggered.

Fired when the guest page attempts to close itself.

The following example code navigates the webview to about:blank when the guest attempts to close itself.

Fired when the guest page has sent an asynchronous message to embedder page.

With sendToHost method and ipc-message event you can communicate between guest page and embedder page:

Fired when the renderer process unexpectedly disappears. This is normally because it was crashed or killed.

Fired when the WebContents is destroyed.

Emitted when media starts playing.

Emitted when media is paused or done playing.

Emitted when a page's theme color changes. This is usually due to encountering a meta tag:

Emitted when mouse moves over a link or the keyboard moves the focus to a link.

Emitted when a link is clicked in DevTools or 'Open in new tab' is selected for a link in its context menu.

Emitted when 'Search' is selected for text in its context menu.

Emitted when DevTools is opened.

Emitted when DevTools is closed.

Emitted when DevTools is focused / opened.

Emitted when there is a new context menu that needs to be handled.

**Examples:**

Example 1 (html):
```html
<webview id="foo" src="https://www.github.com/" style="display:inline-flex; width:640px; height:480px"></webview>
```

Example 2 (html):
```html
<script>  onload = () => {    const webview = document.querySelector('webview')    const indicator = document.querySelector('.indicator')    const loadstart = () => {      indicator.innerText = 'loading...'    }    const loadstop = () => {      indicator.innerText = ''    }    webview.addEventListener('did-start-loading', loadstart)    webview.addEventListener('did-stop-loading', loadstop)  }</script>
```

Example 3 (html):
```html
<webview src="https://www.github.com/"></webview>
```

Example 4 (html):
```html
<webview src="https://www.google.com/" nodeintegration></webview>
```

---

## PostBody Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/post-body

**Contents:**
- PostBody Object

---

## net

**URL:** https://www.electronjs.org/docs/latest/api/net

**Contents:**
- net
- Methods​
  - net.request(options)​
  - net.fetch(input[, init])​
  - net.isOnline()​
  - net.resolveHost(host, [options])​
- Properties​
  - net.online Readonly​

Issue HTTP/HTTPS requests using Chromium's native networking library

Process: Main, Utility

The net module is a client-side API for issuing HTTP(S) requests. It is similar to the HTTP and HTTPS modules of Node.js but uses Chromium's native networking library instead of the Node.js implementation, offering better support for web proxies. It also supports checking network status.

The following is a non-exhaustive list of why you may consider using the net module instead of the native Node.js modules:

The API components (including classes, methods, properties and event names) are similar to those used in Node.js.

The net API can be used only after the application emits the ready event. Trying to use the module before the ready event will throw an error.

The net module has the following methods:

Returns ClientRequest

Creates a ClientRequest instance using the provided options which are directly forwarded to the ClientRequest constructor. The net.request method would be used to issue both secure and insecure HTTP requests according to the specified protocol scheme in the options object.

Returns Promise<GlobalResponse> - see Response.

Sends a request, similarly to how fetch() works in the renderer, using Chrome's network stack. This differs from Node's fetch(), which uses Node.js's HTTP stack.

This method will issue requests from the default session. To send a fetch request from another session, use ses.fetch().

See the MDN documentation for fetch() for more details.

By default, requests made with net.fetch can be made to custom protocols as well as file:, and will trigger webRequest handlers if present. When the non-standard bypassCustomProtocolHandlers option is set in RequestInit, custom protocol handlers will not be called for this request. This allows forwarding an intercepted request to the built-in handler. webRequest handlers will still be triggered when bypassing custom protocols.

In the utility process, custom protocols are not supported.

Returns boolean - Whether there is currently internet connection.

A return value of false is a pretty strong indicator that the user won't be able to connect to remote sites. However, a return value of true is inconclusive; even if some link is up, it is uncertain whether a particular connection attempt to a particular remote site will be successful.

Returns Promise<ResolvedHost> - Resolves with the resolved IP addresses for the host.

This method will resolve hosts from the default session. To resolve a host from another session, use ses.resolveHost().

A boolean property. Whether there is currently internet connection.

A return value of false is a pretty strong indicator that the user won't be able to connect to remote sites. However, a return value of true is inconclusive; even if some link is up, it is uncertain whether a particular connection attempt to a particular remote site will be successful.

**Examples:**

Example 1 (javascript):
```javascript
const { app } = require('electron')app.whenReady().then(() => {  const { net } = require('electron')  const request = net.request('https://github.com')  request.on('response', (response) => {    console.log(`STATUS: ${response.statusCode}`)    console.log(`HEADERS: ${JSON.stringify(response.headers)}`)    response.on('data', (chunk) => {      console.log(`BODY: ${chunk}`)    })    response.on('end', () => {      console.log('No more data in response.')    })  })  request.end()})
```

Example 2 (javascript):
```javascript
async function example () {  const response = await net.fetch('https://my.app')  if (response.ok) {    const body = await response.json()    // ... use the result.  }}
```

Example 3 (typescript):
```typescript
protocol.handle('https', (req) => {  if (req.url === 'https://my-app.com') {    return new Response('<body>my app</body>')  } else {    return net.fetch(req, { bypassCustomProtocolHandlers: true })  }})
```

---

## autoUpdater

**URL:** https://www.electronjs.org/docs/latest/api/auto-updater

**Contents:**
- autoUpdater
- Platform Notices​
  - macOS​
  - Windows​
- Events​
  - Event: 'error'​
  - Event: 'checking-for-update'​
  - Event: 'update-available'​
  - Event: 'update-not-available'​
  - Event: 'update-downloaded'​

Enable apps to automatically update themselves.

See also: A detailed guide about how to implement updates in your application.

autoUpdater is an EventEmitter.

Currently, only macOS and Windows are supported. There is no built-in support for auto-updater on Linux, so it is recommended to use the distribution's package manager to update your app.

In addition, there are some subtle differences on each platform:

On macOS, the autoUpdater module is built upon Squirrel.Mac, meaning you don't need any special setup to make it work. For server-side requirements, you can read Server Support. Note that App Transport Security (ATS) applies to all requests made as part of the update process. Apps that need to disable ATS can add the NSAllowsArbitraryLoads key to their app's plist.

Your application must be signed for automatic updates on macOS. This is a requirement of Squirrel.Mac.

On Windows, you have to install your app into a user's machine before you can use the autoUpdater, so it is recommended that you use electron-winstaller or Electron Forge's Squirrel.Windows maker to generate a Windows installer.

Apps built with Squirrel.Windows will trigger custom launch events that must be handled by your Electron application to ensure proper setup and teardown.

Squirrel.Windows apps will launch with the --squirrel-firstrun argument immediately after installation. During this time, Squirrel.Windows will obtain a file lock on your app, and autoUpdater requests will fail until the lock is released. In practice, this means that you won't be able to check for updates on first launch for the first few seconds. You can work around this by not checking for updates when process.argv contains the --squirrel-firstrun flag or by setting a 10-second timeout on your update checks (see electron/electron#7155 for more information).

The installer generated with Squirrel.Windows will create a shortcut icon with an Application User Model ID in the format of com.squirrel.PACKAGE_ID.YOUR_EXE_WITHOUT_DOT_EXE, examples are com.squirrel.slack.Slack and com.squirrel.code.Code. You have to use the same ID for your app with app.setAppUserModelId API, otherwise Windows will not be able to pin your app properly in task bar.

The autoUpdater object emits the following events:

Emitted when there is an error while updating.

Emitted when checking for an available update has started.

Emitted when there is an available update. The update is downloaded automatically.

Emitted when there is no available update.

Emitted when an update has been downloaded.

On Windows only releaseName is available.

It is not strictly necessary to handle this event. A successfully downloaded update will still be applied the next time the application starts.

This event is emitted after a user calls quitAndInstall().

When this API is called, the before-quit event is not emitted before all windows are closed. As a result you should listen to this event if you wish to perform actions before the windows are closed while a process is quitting, as well as listening to before-quit.

The autoUpdater object has the following methods:

Sets the url and initialize the auto updater.

Returns string - The current update feed URL.

Asks the server whether there is an update. You must call setFeedURL before using this API.

If an update is available it will be downloaded automatically. Calling autoUpdater.checkForUpdates() twice will download the update two times.

Restarts the app and installs the update after it has been downloaded. It should only be called after update-downloaded has been emitted.

Under the hood calling autoUpdater.quitAndInstall() will close all application windows first, and automatically call app.quit() after all windows have been closed.

It is not strictly necessary to call this function to apply an update, as a successfully downloaded update will always be applied the next time the application starts.

---

## systemPreferences

**URL:** https://www.electronjs.org/docs/latest/api/system-preferences

**Contents:**
- systemPreferences
- Events​
  - Event: 'accent-color-changed' Windows Linux​
  - Event: 'color-changed' Windows​
- Methods​
  - systemPreferences.isSwipeTrackingFromScrollEventsEnabled() macOS​
  - systemPreferences.postNotification(event, userInfo[, deliverImmediately]) macOS​
  - systemPreferences.postLocalNotification(event, userInfo) macOS​
  - systemPreferences.postWorkspaceNotification(event, userInfo) macOS​
  - systemPreferences.subscribeNotification(event, callback) macOS​

Get system preferences.

Process: Main, Utility

The systemPreferences object emits the following events:

Returns boolean - Whether the Swipe between pages setting is on.

Posts event as native notifications of macOS. The userInfo is an Object that contains the user information dictionary sent along with the notification.

Posts event as native notifications of macOS. The userInfo is an Object that contains the user information dictionary sent along with the notification.

Posts event as native notifications of macOS. The userInfo is an Object that contains the user information dictionary sent along with the notification.

Returns number - The ID of this subscription

Subscribes to native notifications of macOS, callback will be called with callback(event, userInfo) when the corresponding event happens. The userInfo is an Object that contains the user information dictionary sent along with the notification. The object is the sender of the notification, and only supports NSString values for now.

The id of the subscriber is returned, which can be used to unsubscribe the event.

Under the hood this API subscribes to NSDistributedNotificationCenter, example values of event are:

If event is null, the NSDistributedNotificationCenter doesn’t use it as criteria for delivery to the observer. See docs for more information.

Returns number - The ID of this subscription

Same as subscribeNotification, but uses NSNotificationCenter for local defaults. This is necessary for events such as NSUserDefaultsDidChangeNotification.

If event is null, the NSNotificationCenter doesn’t use it as criteria for delivery to the observer. See docs for more information.

Returns number - The ID of this subscription

Same as subscribeNotification, but uses NSWorkspace.sharedWorkspace.notificationCenter. This is necessary for events such as NSWorkspaceDidActivateApplicationNotification.

If event is null, the NSWorkspaceNotificationCenter doesn’t use it as criteria for delivery to the observer. See docs for more information.

Removes the subscriber with id.

Same as unsubscribeNotification, but removes the subscriber from NSNotificationCenter.

Same as unsubscribeNotification, but removes the subscriber from NSWorkspace.sharedWorkspace.notificationCenter.

Add the specified defaults to your application's NSUserDefaults.

Returns UserDefaultTypes[Type] - The value of key in NSUserDefaults.

Some popular key and types are:

Set the value of key in NSUserDefaults.

Note that type should match actual type of value. An exception is thrown if they don't.

Some popular key and types are:

Removes the key in NSUserDefaults. This can be used to restore the default or global value of a key previously set with setUserDefault.

Returns string - The users current system wide accent color preference in RGBA hexadecimal form.

This API is only available on macOS 10.14 Mojave or newer.

Returns string - The system color setting in RGBA hexadecimal form (#RRGGBBAA). See the Windows docs and the macOS docs for more details.

The following colors are only available on macOS 10.14: find-highlight, selected-content-background, separator, unemphasized-selected-content-background, unemphasized-selected-text-background, and unemphasized-selected-text.

Returns string - The standard system color formatted as #RRGGBBAA.

Returns one of several standard system colors that automatically adapt to vibrancy and changes in accessibility settings like 'Increase contrast' and 'Reduce transparency'. See Apple Documentation for more details.

Returns string - Can be dark, light or unknown.

Gets the macOS appearance setting that is currently applied to your application, maps to NSApplication.effectiveAppearance

Returns boolean - whether or not this device has the ability to use Touch ID.

Returns Promise<void> - resolves if the user has successfully authenticated with Touch ID.

This API itself will not protect your user data; rather, it is a mechanism to allow you to do so. Native apps will need to set Access Control Constants like kSecAccessControlUserPresence on their keychain entry so that reading it would auto-prompt for Touch ID biometric consent. This could be done with node-keytar, such that one would store an encryption key with node-keytar and only fetch it if promptTouchID() resolves.

Returns boolean - true if the current process is a trusted accessibility client and false if it is not.

Returns string - Can be not-determined, granted, denied, restricted or unknown.

This user consent was not required on macOS 10.13 High Sierra so this method will always return granted. macOS 10.14 Mojave or higher requires consent for microphone and camera access. macOS 10.15 Catalina or higher requires consent for screen access.

Windows 10 has a global setting controlling microphone and camera access for all win32 applications. It will always return granted for screen and for all media types on older versions of Windows.

Returns Promise<boolean> - A promise that resolves with true if consent was granted and false if it was denied. If an invalid mediaType is passed, the promise will be rejected. If an access request was denied and later is changed through the System Preferences pane, a restart of the app will be required for the new permissions to take effect. If access has already been requested and denied, it must be changed through the preference pane; an alert will not pop up and the promise will resolve with the existing access status.

Important: In order to properly leverage this API, you must set the NSMicrophoneUsageDescription and NSCameraUsageDescription strings in your app's Info.plist file. The values for these keys will be used to populate the permission dialogs so that the user will be properly informed as to the purpose of the permission request. See Electron Application Distribution for more information about how to set these in the context of Electron.

This user consent was not required until macOS 10.14 Mojave, so this method will always return true if your system is running 10.13 High Sierra.

Returns an object with system animation settings.

A boolean property which determines whether the app avoids using semitransparent backgrounds. This maps to NSWorkspace.accessibilityDisplayShouldReduceTransparency

Deprecated: Use the new nativeTheme.prefersReducedTransparency API.

A string property that can be dark, light or unknown.

Returns the macOS appearance setting that is currently applied to your application, maps to NSApplication.effectiveAppearance

**Examples:**

Example 1 (javascript):
```javascript
const { systemPreferences } = require('electron')console.log(systemPreferences.getEffectiveAppearance())
```

Example 2 (javascript):
```javascript
const color = systemPreferences.getAccentColor() // `"aabbccdd"`const red = color.substr(0, 2) // "aa"const green = color.substr(2, 2) // "bb"const blue = color.substr(4, 2) // "cc"const alpha = color.substr(6, 2) // "dd"
```

Example 3 (javascript):
```javascript
const { systemPreferences } = require('electron')systemPreferences.promptTouchID('To get consent for a Security-Gated Thing').then(success => {  console.log('You have successfully authenticated with Touch ID!')}).catch(err => {  console.log(err)})
```

---

## webContents

**URL:** https://www.electronjs.org/docs/latest/api/web-contents

**Contents:**
- webContents
- Navigation Events​
  - Document Navigations​
  - In-page Navigation​
  - Frame Navigation​
- Methods​
  - webContents.getAllWebContents()​
  - webContents.getFocusedWebContents()​
  - webContents.fromId(id)​
  - webContents.fromFrame(frame)​

Render and control web pages.

webContents is an EventEmitter. It is responsible for rendering and controlling a web page and is a property of the BrowserWindow object. An example of accessing the webContents object:

Several events can be used to monitor navigations as they occur within a webContents.

When a webContents navigates to another page (as opposed to an in-page navigation), the following events will be fired.

Subsequent events will not fire if event.preventDefault() is called on any of the cancellable events.

In-page navigations don't cause the page to reload, but instead navigate to a location within the current page. These events are not cancellable. For an in-page navigations, the following events will fire in this order:

The will-navigate and did-navigate events only fire when the mainFrame navigates. If you want to also observe navigations in <iframe>s, use will-frame-navigate and did-frame-navigate events.

These methods can be accessed from the webContents module:

Returns WebContents[] - An array of all WebContents instances. This will contain web contents for all windows, webviews, opened devtools, and devtools extension background pages.

Returns WebContents | null - The web contents that is focused in this application, otherwise returns null.

Returns WebContents | undefined - A WebContents instance with the given ID, or undefined if there is no WebContents associated with the given ID.

Returns WebContents | undefined - A WebContents instance with the given WebFrameMain, or undefined if there is no WebContents associated with the given WebFrameMain.

Returns WebContents | undefined - A WebContents instance with the given TargetID, or undefined if there is no WebContents associated with the given TargetID.

When communicating with the Chrome DevTools Protocol, it can be useful to lookup a WebContents instance based on its assigned TargetID.

Render and control the contents of a BrowserWindow instance.

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

Emitted when the navigation is done, i.e. the spinner of the tab has stopped spinning, and the onload event was dispatched.

This event is like did-finish-load but emitted when the load failed. The full list of error codes and their meaning is available here.

This event is like did-fail-load but emitted when the load was cancelled (e.g. window.stop() was invoked).

Emitted when a frame has done navigation.

Corresponds to the points in time when the spinner of the tab started spinning.

Corresponds to the points in time when the spinner of the tab stopped spinning.

Emitted when the document in the top-level frame is loaded.

Fired when page title is set during navigation. explicitSet is false when title is synthesized from file url.

Emitted when page receives favicon urls.

Emitted when the page calls window.moveTo, window.resizeTo or related APIs.

By default, this will move the window. To prevent that behavior, call event.preventDefault().

Emitted after successful creation of a window via window.open in the renderer. Not emitted if the creation of the window is canceled from webContents.setWindowOpenHandler.

See window.open() for more details and how to use this in conjunction with webContents.setWindowOpenHandler.

Emitted when a user or the page wants to start navigation on the main frame. It can happen when the window.location object is changed or a user clicks a link in the page.

This event will not emit when the navigation is started programmatically with APIs like webContents.loadURL and webContents.back.

It is also not emitted for in-page navigations, such as clicking anchor links or updating the window.location.hash. Use did-navigate-in-page event for this purpose.

Calling event.preventDefault() will prevent the navigation.

Emitted when a user or the page wants to start navigation in any frame. It can happen when the window.location object is changed or a user clicks a link in the page.

Unlike will-navigate, will-frame-navigate is fired when the main frame or any of its subframes attempts to navigate. When the navigation event comes from the main frame, isMainFrame will be true.

This event will not emit when the navigation is started programmatically with APIs like webContents.loadURL and webContents.back.

It is also not emitted for in-page navigations, such as clicking anchor links or updating the window.location.hash. Use did-navigate-in-page event for this purpose.

Calling event.preventDefault() will prevent the navigation.

Emitted when any frame (including main) starts navigating.

Emitted when a server side redirect occurs during navigation. For example a 302 redirect.

This event will be emitted after did-start-navigation and always before the did-redirect-navigation event for the same navigation.

Calling event.preventDefault() will prevent the navigation (not just the redirect).

Emitted after a server side redirect occurs during navigation. For example a 302 redirect.

This event cannot be prevented, if you want to prevent redirects you should checkout out the will-redirect event above.

Emitted when a main frame navigation is done.

This event is not emitted for in-page navigations, such as clicking anchor links or updating the window.location.hash. Use did-navigate-in-page event for this purpose.

Emitted when any frame navigation is done.

This event is not emitted for in-page navigations, such as clicking anchor links or updating the window.location.hash. Use did-navigate-in-page event for this purpose.

Emitted when an in-page navigation happened in any frame.

When in-page navigation happens, the page URL changes but does not cause navigation outside of the page. Examples of this occurring are when anchor links are clicked or when the DOM hashchange event is triggered.

Emitted when a beforeunload event handler is attempting to cancel a page unload.

Calling event.preventDefault() will ignore the beforeunload event handler and allow the page to be unloaded.

This will be emitted for BrowserViews but will not be respected - this is because we have chosen not to tie the BrowserView lifecycle to its owning BrowserWindow should one exist per the specification.

Emitted when the renderer process unexpectedly disappears. This is normally because it was crashed or killed.

Emitted when the web page becomes unresponsive.

Emitted when the unresponsive web page becomes responsive again.

Emitted when webContents is destroyed.

Emitted when an input event is sent to the WebContents. See InputEvent for details.

Emitted before dispatching the keydown and keyup events in the page. Calling event.preventDefault will prevent the page keydown/keyup events and the menu shortcuts.

To only prevent the menu shortcuts, use setIgnoreMenuShortcuts:

Emitted before dispatching mouse events in the page.

Calling event.preventDefault will prevent the page mouse events.

Emitted when the window enters a full-screen state triggered by HTML API.

Emitted when the window leaves a full-screen state triggered by HTML API.

Emitted when the user is requesting to change the zoom level using the mouse wheel.

Emitted when the WebContents loses focus.

Emitted when the WebContents gains focus.

Note that on macOS, having focus means the WebContents is the first responder of window, so switching focus between windows would not trigger the focus and blur events of WebContents, as the first responder of each window is not changed.

The focus and blur events of WebContents should only be used to detect focus change between different WebContents and BrowserView in the same window.

Emitted when a link is clicked in DevTools or 'Open in new tab' is selected for a link in its context menu.

Emitted when 'Search' is selected for text in its context menu.

Emitted when DevTools is opened.

Emitted when DevTools is closed.

Emitted when DevTools is focused / opened.

Emitted when failed to verify the certificate for url.

The usage is the same with the certificate-error event of app.

Emitted when a client certificate is requested.

The usage is the same with the select-client-certificate event of app.

Emitted when webContents wants to do basic auth.

The usage is the same with the login event of app.

Emitted when a result is available for webContents.findInPage request.

Emitted when media starts playing.

Emitted when media is paused or done playing.

Emitted when media becomes audible or inaudible.

Emitted when a page's theme color changes. This is usually due to encountering a meta tag:

Emitted when mouse moves over a link or the keyboard moves the focus to a link.

Emitted when the cursor's type changes. The type parameter can be pointer, crosshair, hand, text, wait, help, e-resize, n-resize, ne-resize, nw-resize, s-resize, se-resize, sw-resize, w-resize, ns-resize, ew-resize, nesw-resize, nwse-resize, col-resize, row-resize, m-panning, m-panning-vertical, m-panning-horizontal, e-panning, n-panning, ne-panning, nw-panning, s-panning, se-panning, sw-panning, w-panning, move, vertical-text, cell, context-menu, alias, progress, nodrop, copy, none, not-allowed, zoom-in, zoom-out, grab, grabbing, custom, null, drag-drop-none, drag-drop-move, drag-drop-copy, drag-drop-link, ns-no-resize, ew-no-resize, nesw-no-resize, nwse-no-resize, or default.

If the type parameter is custom, the image parameter will hold the custom cursor image in a NativeImage, and scale, size and hotspot will hold additional information about the custom cursor.

Emitted when there is a new context menu that needs to be handled.

Emitted when a bluetooth device needs to be selected when a call to navigator.bluetooth.requestDevice is made. callback should be called with the deviceId of the device to be selected. Passing an empty string to callback will cancel the request.

If no event listener is added for this event, all bluetooth requests will be cancelled.

If event.preventDefault is not called when handling this event, the first available device will be automatically selected.

Due to the nature of bluetooth, scanning for devices when navigator.bluetooth.requestDevice is called may take time and will cause select-bluetooth-device to fire multiple times until callback is called with either a device id or an empty string to cancel the request.

Emitted when a new frame is generated. Only the dirty area is passed in the buffer.

When using shared texture (set webPreferences.offscreen.useSharedTexture to true) feature, you can pass the texture handle to external rendering pipeline without the overhead of copying data between CPU and GPU memory, with Chromium's hardware acceleration support. This feature is helpful for high-performance rendering scenarios.

Only a limited number of textures can exist at the same time, so it's important that you call texture.release() as soon as you're done with the texture. By managing the texture lifecycle by yourself, you can safely pass the texture.textureInfo to other processes through IPC.

More details can be found in the offscreen rendering tutorial. To learn about how to handle the texture in native code, refer to offscreen rendering's code documentation..

Emitted when the devtools window instructs the webContents to reload

Emitted when a <webview>'s web contents is being attached to this web contents. Calling event.preventDefault() will destroy the guest page.

This event can be used to configure webPreferences for the webContents of a <webview> before it's loaded, and provides the ability to set settings that can't be set via <webview> attributes.

Emitted when a <webview> has been attached to this web contents.

Emitted when the associated window logs a console message.

Emitted when the preload script preloadPath throws an unhandled exception error.

Emitted when the renderer process sends an asynchronous message via ipcRenderer.send().

See also webContents.ipc, which provides an IpcMain-like interface for responding to IPC messages specifically from this WebContents.

Emitted when the renderer process sends a synchronous message via ipcRenderer.sendSync().

See also webContents.ipc, which provides an IpcMain-like interface for responding to IPC messages specifically from this WebContents.

Emitted when the WebContents preferred size has changed.

This event will only be emitted when enablePreferredSizeMode is set to true in webPreferences.

Emitted when the mainFrame, an <iframe>, or a nested <iframe> is loaded within the page.

Returns Promise<void> - the promise will resolve when the page has finished loading (see did-finish-load), and rejects if the page fails to load (see did-fail-load). A noop rejection handler is already attached, which avoids unhandled rejection errors. If the existing page has a beforeUnload handler, did-fail-load will be called unless will-prevent-unload is handled.

Loads the url in the window. The url must contain the protocol prefix, e.g. the http:// or file://. If the load should bypass http cache then use the pragma header to achieve it.

Returns Promise<void> - the promise will resolve when the page has finished loading (see did-finish-load), and rejects if the page fails to load (see did-fail-load).

Loads the given file in the window, filePath should be a path to an HTML file relative to the root of your application. For instance an app structure like this:

Would require code like this

Initiates a download of the resource at url without navigating. The will-download event of session will be triggered.

Returns string - The URL of the current web page.

Returns string - The title of the current web page.

Returns boolean - Whether the web page is destroyed.

Closes the page, as if the web content had called window.close().

If the page is successfully closed (i.e. the unload is not prevented by the page, or waitForBeforeUnload is false or unspecified), the WebContents will be destroyed and no longer usable. The destroyed event will be emitted.

Focuses the web page.

Returns boolean - Whether the web page is focused.

Returns boolean - Whether web page is still loading resources.

Returns boolean - Whether the main frame (and not just iframes or frames within it) is still loading.

Returns boolean - Whether the web page is waiting for a first-response from the main resource of the page.

Stops any pending navigation.

Reloads the current web page.

Reloads current page and ignores cache.

Returns boolean - Whether the browser can go back to previous web page.

Deprecated: Should use the new contents.navigationHistory.canGoBack API.

Returns boolean - Whether the browser can go forward to next web page.

Deprecated: Should use the new contents.navigationHistory.canGoForward API.

Returns boolean - Whether the web page can go to offset.

Deprecated: Should use the new contents.navigationHistory.canGoToOffset API.

Clears the navigation history.

Deprecated: Should use the new contents.navigationHistory.clear API.

Makes the browser go back a web page.

Deprecated: Should use the new contents.navigationHistory.goBack API.

Makes the browser go forward a web page.

Deprecated: Should use the new contents.navigationHistory.goForward API.

Navigates browser to the specified absolute web page index.

Deprecated: Should use the new contents.navigationHistory.goToIndex API.

Navigates to the specified offset from the "current entry".

Deprecated: Should use the new contents.navigationHistory.goToOffset API.

Returns boolean - Whether the renderer process has crashed.

Forcefully terminates the renderer process that is currently hosting this webContents. This will cause the render-process-gone event to be emitted with the reason=killed || reason=crashed. Please note that some webContents share renderer processes and therefore calling this method may also crash the host process for other webContents as well.

Calling reload() immediately after calling this method will force the reload to occur in a new process. This should be used when this process is unstable or unusable, for instance in order to recover from the unresponsive event.

Overrides the user agent for this web page.

Returns string - The user agent for this web page.

Returns Promise<string> - A promise that resolves with a key for the inserted CSS that can later be used to remove the CSS via contents.removeInsertedCSS(key).

Injects CSS into the current web page and returns a unique key for the inserted stylesheet.

Returns Promise<void> - Resolves if the removal was successful.

Removes the inserted CSS from the current web page. The stylesheet is identified by its key, which is returned from contents.insertCSS(css).

Returns Promise<any> - A promise that resolves with the result of the executed code or is rejected if the result of the code is a rejected promise.

Evaluates code in page.

In the browser window some HTML APIs like requestFullScreen can only be invoked by a gesture from the user. Setting userGesture to true will remove this limitation.

Code execution will be suspended until web page stop loading.

Returns Promise<any> - A promise that resolves with the result of the executed code or is rejected if the result of the code is a rejected promise.

Works like executeJavaScript but evaluates scripts in an isolated context.

Ignore application menu shortcuts while this web contents is focused.

handler Function<WindowOpenHandlerResponse>

Returns WindowOpenHandlerResponse - When set to { action: 'deny' } cancels the creation of the new window. { action: 'allow' } will allow the new window to be created. Returning an unrecognized value such as a null, undefined, or an object without a recognized 'action' value will result in a console error and have the same effect as returning {action: 'deny'}.

Called before creating a window a new window is requested by the renderer, e.g. by window.open(), a link with target="_blank", shift+clicking on a link, or submitting a form with <form target="_blank">. See window.open() for more details and how to use this in conjunction with did-create-window.

An example showing how to customize the process of new BrowserWindow creation to be BrowserView attached to main window instead:

Mute the audio on the current web page.

Returns boolean - Whether this page has been muted.

Returns boolean - Whether audio is currently playing.

Changes the zoom factor to the specified factor. Zoom factor is zoom percent divided by 100, so 300% = 3.0.

The factor must be greater than 0.0.

Returns number - the current zoom factor.

Changes the zoom level to the specified level. The original size is 0 and each increment above or below represents zooming 20% larger or smaller to default limits of 300% and 50% of original size, respectively. The formula for this is scale := 1.2 ^ level.

The zoom policy at the Chromium level is same-origin, meaning that the zoom level for a specific domain propagates across all instances of windows with the same domain. Differentiating the window URLs will make zoom work per-window.

Returns number - the current zoom level.

Returns Promise<void>

Sets the maximum and minimum pinch-to-zoom level.

Visual zoom is disabled by default in Electron. To re-enable it, call:const win = new BrowserWindow()win.webContents.setVisualZoomLevelLimits(1, 3)

Executes the editing command undo in web page.

Executes the editing command redo in web page.

Executes the editing command cut in web page.

Executes the editing command copy in web page.

Centers the current text selection in web page.

Copy the image at the given position to the clipboard.

Executes the editing command paste in web page.

Executes the editing command pasteAndMatchStyle in web page.

Executes the editing command delete in web page.

Executes the editing command selectAll in web page.

Executes the editing command unselect in web page.

Scrolls to the top of the current webContents.

Scrolls to the bottom of the current webContents.

Adjusts the current text selection starting and ending points in the focused frame by the given amounts. A negative amount moves the selection towards the beginning of the document, and a positive amount moves the selection towards the end of the document.

For a call of win.webContents.adjustSelection({ start: 1, end: 5 })

Executes the editing command replace in web page.

Executes the editing command replaceMisspelling in web page.

Returns Promise<void>

Inserts text to the focused element.

Returns Integer - The request id used for the request.

Starts a request to find all matches for the text in the web page. The result of the request can be obtained by subscribing to found-in-page event.

Stops any findInPage request for the webContents with the provided action.

Returns Promise<NativeImage> - Resolves with a NativeImage

Captures a snapshot of the page within rect. Omitting rect will capture the whole visible page. The page is considered visible when its browser window is hidden and the capturer count is non-zero. If you would like the page to stay hidden, you should ensure that stayHidden is set to true.

Returns boolean - Whether this page is being captured. It returns true when the capturer count is greater than 0.

Get the system printer list.

Returns Promise<PrinterInfo[]> - Resolves with a PrinterInfo[]

When a custom pageSize is passed, Chromium attempts to validate platform specific minimum values for width_microns and height_microns. Width and height must both be minimum 353 microns but may be higher on some operating systems.

Prints window's web page. When silent is set to true, Electron will pick the system's default printer if deviceName is empty and the default settings for printing.

Some possible failureReasons for print failure include:

Use page-break-before: always; CSS style to force to print to a new page.

Returns Promise<Buffer> - Resolves with the generated PDF data.

Prints the window's web page as PDF.

The landscape will be ignored if @page CSS at-rule is used in the web page.

An example of webContents.printToPDF:

See Page.printToPdf for more information.

Adds the specified path to DevTools workspace. Must be used after DevTools creation:

Removes the specified path from DevTools workspace.

Uses the devToolsWebContents as the target WebContents to show devtools.

The devToolsWebContents must not have done any navigation, and it should not be used for other purposes after the call.

By default Electron manages the devtools by creating an internal WebContents with native view, which developers have very limited control of. With the setDevToolsWebContents method, developers can use any WebContents to show the devtools in it, including BrowserWindow, BrowserView and <webview> tag.

Note that closing the devtools does not destroy the devToolsWebContents, it is caller's responsibility to destroy devToolsWebContents.

An example of showing devtools in a <webview> tag:

An example of showing devtools in a BrowserWindow:

When contents is a <webview> tag, the mode would be detach by default, explicitly passing an empty mode can force using last used dock state.

On Windows, if Windows Control Overlay is enabled, Devtools will be opened with mode: 'detach'.

Returns boolean - Whether the devtools is opened.

Returns boolean - Whether the devtools view is focused .

Returns string - the current title of the DevTools window. This will only be visible if DevTools is opened in undocked or detach mode.

Changes the title of the DevTools window to title. This will only be visible if DevTools is opened in undocked or detach mode.

Toggles the developer tools.

Starts inspecting element at position (x, y).

Opens the developer tools for the shared worker context.

Inspects the shared worker based on its ID.

Returns SharedWorkerInfo[] - Information about all Shared Workers.

Opens the developer tools for the service worker context.

Send an asynchronous message to the renderer process via channel, along with arguments. Arguments will be serialized with the Structured Clone Algorithm, just like postMessage, so prototype chains will not be included. Sending Functions, Promises, Symbols, WeakMaps, or WeakSets will throw an exception.

Sending non-standard JavaScript types such as DOM objects or special Electron objects will throw an exception.

For additional reading, refer to Electron's IPC guide.

Send an asynchronous message to a specific frame in a renderer process via channel, along with arguments. Arguments will be serialized with the Structured Clone Algorithm, just like postMessage, so prototype chains will not be included. Sending Functions, Promises, Symbols, WeakMaps, or WeakSets will throw an exception.

NOTE: Sending non-standard JavaScript types such as DOM objects or special Electron objects will throw an exception.

The renderer process can handle the message by listening to channel with the ipcRenderer module.

If you want to get the frameId of a given renderer context you should use the webFrame.routingId value. E.g.

You can also read frameId from all incoming IPC messages in the main process.

Send a message to the renderer process, optionally transferring ownership of zero or more MessagePortMain objects.

The transferred MessagePortMain objects will be available in the renderer process by accessing the ports property of the emitted event. When they arrive in the renderer, they will be native DOM MessagePort objects.

Enable device emulation with the given parameters.

Disable device emulation enabled by webContents.enableDeviceEmulation.

Sends an input event to the page.

The BrowserWindow containing the contents needs to be focused for sendInputEvent() to work.

Begin subscribing for presentation events and captured frames, the callback will be called with callback(image, dirtyRect) when there is a presentation event.

The image is an instance of NativeImage that stores the captured frame.

The dirtyRect is an object with x, y, width, height properties that describes which part of the page was repainted. If onlyDirty is set to true, image will only contain the repainted area. onlyDirty defaults to false.

End subscribing for frame presentation events.

Sets the item as dragging item for current drag-drop operation, file is the absolute path of the file to be dragged, and icon is the image showing under the cursor when dragging.

Returns Promise<void> - resolves if the page is saved.

Shows pop-up dictionary that searches the selected word on the page.

Returns boolean - Indicates whether offscreen rendering is enabled.

If offscreen rendering is enabled and not painting, start painting.

If offscreen rendering is enabled and painting, stop painting.

Returns boolean - If offscreen rendering is enabled returns whether it is currently painting.

If offscreen rendering is enabled sets the frame rate to the specified number. Only values between 1 and 240 are accepted.

Returns Integer - If offscreen rendering is enabled returns the current frame rate.

Schedules a full repaint of the window this web contents is in.

If offscreen rendering is enabled invalidates the frame and generates a new one through the 'paint' event.

Returns string - Returns the WebRTC IP Handling Policy.

Setting the WebRTC IP handling policy allows you to control which IPs are exposed via WebRTC. See BrowserLeaks for more details.

By default this value is { min: 0, max: 0 } , which would apply no restriction on the udp port range.

Setting the WebRTC UDP Port Range allows you to restrict the udp port range used by WebRTC. By default the port range is unrestricted.

To reset to an unrestricted port range this value should be set to { min: 0, max: 0 }.

Returns string - The identifier of a WebContents stream. This identifier can be used with navigator.mediaDevices.getUserMedia using a chromeMediaSource of tab. The identifier is restricted to the web contents that it is registered to and is only valid for 10 seconds.

Returns Integer - The operating system pid of the associated renderer process.

Returns Integer - The Chromium internal pid of the associated renderer. Can be compared to the frameProcessId passed by frame specific navigation events (e.g. did-frame-navigate)

Returns Promise<void> - Indicates whether the snapshot has been created successfully.

Takes a V8 heap snapshot and saves it to filePath.

Returns boolean - whether or not this WebContents will throttle animations and timers when the page becomes backgrounded. This also affects the Page Visibility API.

WebContents.backgroundThrottling set to false affects all WebContents in the host BrowserWindow

Controls whether or not this WebContents will throttle animations and timers when the page becomes backgrounded. This also affects the Page Visibility API.

Returns string - the type of the webContent. Can be backgroundPage, window, browserView, remote, webview or offscreen.

Sets the image animation policy for this webContents. The policy only affects new images, existing images that are currently being animated are unaffected. This is a known limitation in Chromium, you can force image animation to be recalculated with img.src = img.src which will result in no network traffic but will update the animation policy.

This corresponds to the animationPolicy accessibility feature in Chromium.

An IpcMain scoped to just IPC messages sent from this WebContents.

IPC messages sent with ipcRenderer.send, ipcRenderer.sendSync or ipcRenderer.postMessage will be delivered in the following order:

Handlers registered with invoke will be checked in the following order. The first one that is defined will be called, the rest will be ignored.

A handler or event listener registered on the WebContents will receive IPC messages sent from any frame, including child frames. In most cases, only the main frame can send IPC messages. However, if the nodeIntegrationInSubFrames option is enabled, it is possible for child frames to send IPC messages also. In that case, handlers should check the senderFrame property of the IPC event to ensure that the message is coming from the expected frame. Alternatively, register handlers on the appropriate frame directly using the WebFrameMain.ipc interface.

A boolean property that determines whether this page is muted.

A string property that determines the user agent for this web page.

A number property that determines the zoom level for this web contents.

The original size is 0 and each increment above or below represents zooming 20% larger or smaller to default limits of 300% and 50% of original size, respectively. The formula for this is scale := 1.2 ^ level.

A number property that determines the zoom factor for this web contents.

The zoom factor is the zoom percent divided by 100, so 300% = 3.0.

An Integer property that sets the frame rate of the web contents to the specified number. Only values between 1 and 240 are accepted.

Only applicable if offscreen rendering is enabled.

A Integer representing the unique ID of this WebContents. Each ID is unique among all WebContents instances of the entire Electron application.

A Session used by this webContents.

A NavigationHistory used by this webContents.

A WebContents instance that might own this WebContents.

A WebContents | null property that represents the of DevTools WebContents associated with a given WebContents.

Users should never store this object because it may become null when the DevTools has been closed.

A Debugger instance for this webContents.

WebContents.backgroundThrottling set to false affects all WebContents in the host BrowserWindow

A boolean property that determines whether or not this WebContents will throttle animations and timers when the page becomes backgrounded. This also affects the Page Visibility API.

A WebFrameMain property that represents the top frame of the page's frame hierarchy.

A WebFrameMain | null property that represents the frame that opened this WebContents, either with open(), or by navigating a link with a target attribute.

A WebFrameMain | null property that represents the currently focused frame in this WebContents. Can be the top frame, an inner <iframe>, or null if nothing is focused.

**Examples:**

Example 1 (javascript):
```javascript
const { BrowserWindow } = require('electron')const win = new BrowserWindow({ width: 800, height: 1500 })win.loadURL('https://github.com')const contents = win.webContentsconsole.log(contents)
```

Example 2 (javascript):
```javascript
const { webContents } = require('electron')console.log(webContents)
```

Example 3 (javascript):
```javascript
async function lookupTargetId (browserWindow) {  const wc = browserWindow.webContents  await wc.debugger.attach('1.3')  const { targetInfo } = await wc.debugger.sendCommand('Target.getTargetInfo')  const { targetId } = targetInfo  const targetWebContents = await wc.fromDevToolsTargetId(targetId)}
```

Example 4 (javascript):
```javascript
const { BrowserWindow, dialog } = require('electron')const win = new BrowserWindow({ width: 800, height: 600 })win.webContents.on('will-prevent-unload', (event) => {  const choice = dialog.showMessageBoxSync(win, {    type: 'question',    buttons: ['Leave', 'Stay'],    title: 'Do you want to leave this site?',    message: 'Changes you made may not be saved.',    defaultId: 0,    cancelId: 1  })  const leave = (choice === 0)  if (leave) {    event.preventDefault()  }})
```

---

## View

**URL:** https://www.electronjs.org/docs/latest/api/view

**Contents:**
- View
- Class: View​
  - new View()​
  - Instance Events​
    - Event: 'bounds-changed'​
  - Instance Methods​
    - view.addChildView(view[, index])​
    - view.removeChildView(view)​
    - view.setBounds(bounds)​
    - view.getBounds()​

Create and layout native views.

This module cannot be used until the ready event of the app module is emitted.

View is an EventEmitter.

Electron's built-in classes cannot be subclassed in user code. For more information, see the FAQ.

Objects created with new View emit the following events:

Emitted when the view's bounds have changed in response to being laid out. The new bounds can be retrieved with view.getBounds().

Objects created with new View have the following instance methods:

If the same View is added to a parent which already contains it, it will be reordered such that it becomes the topmost view.

If the view passed as a parameter is not a child of this view, this method is a no-op.

Returns Rectangle - The bounds of this View, relative to its parent.

Examples of valid color values:

Hex format with alpha takes AARRGGBB or ARGB, not RRGGBBAA or RGB.

The area cutout of the view's border still captures clicks.

Returns boolean - Whether the view should be drawn. Note that this is different from whether the view is visible on screen—it may still be obscured or out of view.

Objects created with new View have the following properties:

A View[] property representing the child views of this view.

**Examples:**

Example 1 (javascript):
```javascript
const { BaseWindow, View } = require('electron')const win = new BaseWindow()const view = new View()view.setBackgroundColor('red')view.setBounds({ x: 0, y: 0, width: 100, height: 100 })win.contentView.addChildView(view)
```

---

## Tray

**URL:** https://www.electronjs.org/docs/latest/api/tray

**Contents:**
- Tray
- Class: Tray​
  - new Tray(image, [guid])​
  - Instance Events​
    - Event: 'click'​
    - Event: 'right-click' macOS Windows​
    - Event: 'double-click' macOS Windows​
    - Event: 'middle-click' Windows​
    - Event: 'balloon-show' Windows​
    - Event: 'balloon-click' Windows​

Add icons and context menus to the system's notification area.

Tray is an EventEmitter.

See also: A detailed guide about how to implement Tray menus.

Electron's built-in classes cannot be subclassed in user code. For more information, see the FAQ.

Platform Considerations

On Windows, if the executable is signed and the signature contains an organization in the subject line then the GUID is permanently associated with that signature. OS level settings like the position of the tray icon in the system tray will persist even if the path to the executable changes. If the executable is not code-signed then the GUID is permanently associated with the path to the executable. Changing the path to the executable will break the creation of the tray icon and a new GUID must be used. However, it is highly recommended to use the GUID parameter only in conjunction with code-signed executable. If an App defines multiple tray icons then each icon must use a separate GUID.

On macOS, the guid is a string used to uniquely identify the tray icon and allow it to retain its position between relaunches. Using the same string for a new tray item will create it in the same position as the previous tray item to use the string.

Creates a new tray icon associated with the image.

The Tray module emits the following events:

Emitted when the tray icon is clicked.

Note that on Linux this event is emitted when the tray icon receives an activation, which might not necessarily be left mouse click.

Emitted when the tray icon is right clicked.

Emitted when the tray icon is double clicked.

Emitted when the tray icon is middle clicked.

Emitted when the tray balloon shows.

Emitted when the tray balloon is clicked.

Emitted when the tray balloon is closed because of timeout or user manually closes it.

Emitted when any dragged items are dropped on the tray icon.

Emitted when dragged files are dropped in the tray icon.

Emitted when dragged text is dropped in the tray icon.

Emitted when a drag operation enters the tray icon.

Emitted when a drag operation exits the tray icon.

Emitted when a drag operation ends on the tray or ends at another location.

Emitted when the mouse is released from clicking the tray icon.

This will not be emitted if you have set a context menu for your Tray using tray.setContextMenu, as a result of macOS-level constraints.

Emitted when the mouse clicks the tray icon.

Emitted when the mouse enters the tray icon.

Emitted when the mouse exits the tray icon.

Emitted when the mouse moves in the tray icon.

The Tray class has the following methods:

Destroys the tray icon immediately.

Sets the image associated with this tray icon.

Sets the image associated with this tray icon when pressed on macOS.

Sets the hover text for this tray icon. Setting the text to an empty string will remove the tooltip.

Sets the title displayed next to the tray icon in the status bar (Support ANSI colors).

Returns string - the title displayed next to the tray icon in the status bar

Sets the option to ignore double click events. Ignoring these events allows you to detect every individual click of the tray icon.

This value is set to false by default.

Returns boolean - Whether double click events will be ignored.

Displays a tray balloon.

Removes a tray balloon.

Returns focus to the taskbar notification area. Notification area icons should use this message when they have completed their UI operation. For example, if the icon displays a shortcut menu, but the user presses ESC to cancel it, use tray.focus() to return focus to the notification area.

Pops up the context menu of the tray icon. When menu is passed, the menu will be shown instead of the tray icon's context menu.

The position is only available on Windows, and it is (0, 0) by default.

Closes an open context menu, as set by tray.setContextMenu().

Sets the context menu for this icon.

The bounds of this tray icon as Object.

Returns string | null - The GUID used to uniquely identify the tray icon and allow it to retain its position between relaunches, or null if none is set.

Returns boolean - Whether the tray icon is destroyed.

**Examples:**

Example 1 (javascript):
```javascript
const { app, Menu, Tray } = require('electron')let tray = nullapp.whenReady().then(() => {  tray = new Tray('/path/to/my/icon')  const contextMenu = Menu.buildFromTemplate([    { label: 'Item1', type: 'radio' },    { label: 'Item2', type: 'radio' },    { label: 'Item3', type: 'radio', checked: true },    { label: 'Item4', type: 'radio' }  ])  tray.setToolTip('This is my application.')  tray.setContextMenu(contextMenu)})
```

Example 2 (javascript):
```javascript
const { app, Menu, Tray } = require('electron')let appIcon = nullapp.whenReady().then(() => {  appIcon = new Tray('/path/to/my/icon')  const contextMenu = Menu.buildFromTemplate([    { label: 'Item1', type: 'radio' },    { label: 'Item2', type: 'radio' }  ])  // Make a change to the context menu  contextMenu.items[1].checked = false  // Call this again for Linux because we modified the context menu  appIcon.setContextMenu(contextMenu)})
```

Example 3 (javascript):
```javascript
const { app, Menu, Tray } = require('electron')let appIcon = nullapp.whenReady().then(() => {  appIcon = new Tray('/path/to/my/icon')  const contextMenu = Menu.buildFromTemplate([    { label: 'Item1', type: 'radio' },    { label: 'Item2', type: 'radio' }  ])  // Make a change to the context menu  contextMenu.items[1].checked = false  // Call this again for Linux because we modified the context menu  appIcon.setContextMenu(contextMenu)})
```

---

## UploadFile Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/upload-file

**Contents:**
- UploadFile Object

---

## Class: TouchBarGroup

**URL:** https://www.electronjs.org/docs/latest/api/touch-bar-group

**Contents:**
- Class: TouchBarGroup
- Class: TouchBarGroup​
  - new TouchBarGroup(options)​

Create a group in the touch bar for native macOS applications

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

---

## WebSource Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/web-source

**Contents:**
- WebSource Object

---

## SharingItem Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/sharing-item

**Contents:**
- SharingItem Object

---

## crashReporter

**URL:** https://www.electronjs.org/docs/latest/api/crash-reporter

**Contents:**
- crashReporter
- Methods​
  - crashReporter.start(options)​
  - crashReporter.getLastCrashReport()​
  - crashReporter.getUploadedReports()​
  - crashReporter.getUploadToServer()​
  - crashReporter.setUploadToServer(uploadToServer)​
  - crashReporter.addExtraParameter(key, value)​
  - crashReporter.removeExtraParameter(key)​
  - crashReporter.getParameters()​

Submit crash reports to a remote server.

Process: Main, Renderer

If you want to call this API from a renderer process with context isolation enabled, place the API call in your preload script and expose it using the contextBridge API.

The following is an example of setting up Electron to automatically submit crash reports to a remote server:

For setting up a server to accept and process crash reports, you can use following projects:

Electron uses Crashpad, not Breakpad, to collect and upload crashes, but for the time being, the upload protocol is the same.

Or use a 3rd party hosted solution:

Crash reports are stored temporarily before being uploaded in a directory underneath the app's user data directory, called 'Crashpad'. You can override this directory by calling app.setPath('crashDumps', '/path/to/crashes') before starting the crash reporter.

Electron uses crashpad to monitor and report crashes.

The crashReporter module has the following methods:

This method must be called before using any other crashReporter APIs. Once initialized this way, the crashpad handler collects crashes from all subsequently created processes. The crash reporter cannot be disabled once started.

This method should be called as early as possible in app startup, preferably before app.on('ready'). If the crash reporter is not initialized at the time a renderer process is created, then that renderer process will not be monitored by the crash reporter.

You can test out the crash reporter by generating a crash using process.crash().

If you need to send additional/updated extra parameters after your first call start you can call addExtraParameter.

Parameters passed in extra, globalExtra or set with addExtraParameter have limits on the length of the keys and values. Key names must be at most 39 bytes long, and values must be no longer than 127 bytes. Keys with names longer than the maximum will be silently ignored. Key values longer than the maximum length will be truncated.

This method is only available in the main process.

Returns CrashReport | null - The date and ID of the last crash report. Only crash reports that have been uploaded will be returned; even if a crash report is present on disk it will not be returned until it is uploaded. In the case that there are no uploaded reports, null is returned.

This method is only available in the main process.

Returns CrashReport[]:

Returns all uploaded crash reports. Each report contains the date and uploaded ID.

This method is only available in the main process.

Returns boolean - Whether reports should be submitted to the server. Set through the start method or setUploadToServer.

This method is only available in the main process.

This would normally be controlled by user preferences. This has no effect if called before start is called.

This method is only available in the main process.

Set an extra parameter to be sent with the crash report. The values specified here will be sent in addition to any values set via the extra option when start was called.

Parameters added in this fashion (or via the extra parameter to crashReporter.start) are specific to the calling process. Adding extra parameters in the main process will not cause those parameters to be sent along with crashes from renderer or other child processes. Similarly, adding extra parameters in a renderer process will not result in those parameters being sent with crashes that occur in other renderer processes or in the main process.

Parameters have limits on the length of the keys and values. Key names must be no longer than 39 bytes, and values must be no longer than 20320 bytes. Keys with names longer than the maximum will be silently ignored. Key values longer than the maximum length will be truncated.

Remove an extra parameter from the current set of parameters. Future crashes will not include this parameter.

Returns Record<string, string> - The current 'extra' parameters of the crash reporter.

Since require('electron') is not available in Node child processes, the following APIs are available on the process object in Node child processes.

See crashReporter.start().

Note that if the crash reporter is started in the main process, it will automatically monitor child processes, so it should not be started in the child process. Only use this method if the main process does not initialize the crash reporter.

See crashReporter.getParameters().

See crashReporter.addExtraParameter(key, value).

See crashReporter.removeExtraParameter(key).

The crash reporter will send the following data to the submitURL as a multipart/form-data POST:

**Examples:**

Example 1 (css):
```css
const { crashReporter } = require('electron')crashReporter.start({ submitURL: 'https://your-domain.com/url-to-submit' })
```

---

## OffscreenSharedTexture Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/offscreen-shared-texture

**Contents:**
- OffscreenSharedTexture Object

---

## JumpListItem Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/jump-list-item

**Contents:**
- JumpListItem Object

---

## Transaction Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/transaction

**Contents:**
- Transaction Object

---

## UploadRawData Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/upload-raw-data

**Contents:**
- UploadRawData Object

---

## Class: TouchBarSpacer

**URL:** https://www.electronjs.org/docs/latest/api/touch-bar-spacer

**Contents:**
- Class: TouchBarSpacer
- Class: TouchBarSpacer​
  - new TouchBarSpacer(options)​
  - Instance Properties​
    - touchBarSpacer.size​

Create a spacer between two items in the touch bar for native macOS applications

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

The following properties are available on instances of TouchBarSpacer:

A string representing the size of the spacer. Can be small, large or flexible.

---

## Product Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/product

**Contents:**
- Product Object

---

## webUtils

**URL:** https://www.electronjs.org/docs/latest/api/web-utils

**Contents:**
- webUtils
- Methods​
  - webUtils.getPathForFile(file)​

A utility layer to interact with Web API objects (Files, Blobs, etc.)

If you want to call this API from a renderer process with context isolation enabled, place the API call in your preload script and expose it using the contextBridge API.

The webUtils module has the following methods:

Returns string - The file system path that this File object points to. In the case where the object passed in is not a File object an exception is thrown. In the case where the File object passed in was constructed in JS and is not backed by a file on disk an empty string is returned.

This method superseded the previous augmentation to the File object with the path property. An example is included below.

**Examples:**

Example 1 (csharp):
```csharp
// Before (renderer)const oldPath = document.querySelector('input[type=file]').files[0].path
```

Example 2 (javascript):
```javascript
// After// Renderer:const file = document.querySelector('input[type=file]').files[0]electronApi.doSomethingWithFile(file)// Preload script:const { contextBridge, webUtils } = require('electron')contextBridge.exposeInMainWorld('electronApi', {  doSomethingWithFile (file) {    const path = webUtils.getPathForFile(file)    // Do something with the path, e.g., send it over IPC to the main process.    // It's best not to expose the full file path to the web content if possible.  }})
```

---

## Class: TouchBarSegmentedControl

**URL:** https://www.electronjs.org/docs/latest/api/touch-bar-segmented-control

**Contents:**
- Class: TouchBarSegmentedControl
- Class: TouchBarSegmentedControl​
  - new TouchBarSegmentedControl(options)​
  - Instance Properties​
    - touchBarSegmentedControl.segmentStyle​
    - touchBarSegmentedControl.segments​
    - touchBarSegmentedControl.selectedIndex​
    - touchBarSegmentedControl.mode​

Create a segmented control (a button group) where one button has a selected state

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

The following properties are available on instances of TouchBarSegmentedControl:

A string representing the controls current segment style. Updating this value immediately updates the control in the touch bar.

A SegmentedControlSegment[] array representing the segments in this control. Updating this value immediately updates the control in the touch bar. Updating deep properties inside this array does not update the touch bar.

An Integer representing the currently selected segment. Changing this value immediately updates the control in the touch bar. User interaction with the touch bar will update this value automatically.

A string representing the current selection mode of the control. Can be single, multiple or buttons.

---

## SharedDictionaryUsageInfo Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/shared-dictionary-usage-info

**Contents:**
- SharedDictionaryUsageInfo Object

---

## GPUFeatureStatus Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/gpu-feature-status

**Contents:**
- GPUFeatureStatus Object

---

## Environment Variables

**URL:** https://www.electronjs.org/docs/latest/api/environment-variables

**Contents:**
- Environment Variables
- Production Variables​
  - NODE_OPTIONS​
  - NODE_EXTRA_CA_CERTS​
  - GOOGLE_API_KEY​
  - ELECTRON_NO_ASAR​
  - ELECTRON_RUN_AS_NODE​
  - ELECTRON_NO_ATTACH_CONSOLE Windows​
  - ELECTRON_FORCE_WINDOW_MENU_BAR Linux​
  - ELECTRON_TRASH Linux​

Control application configuration and behavior without changing code.

Certain Electron behaviors are controlled by environment variables because they are initialized earlier than the command line flags and the app's code.

Windows console example:

The following environment variables are intended primarily for use at runtime in packaged Electron applications.

Electron includes support for a subset of Node's NODE_OPTIONS. The majority are supported with the exception of those which conflict with Chromium's use of BoringSSL.

Unsupported options are:

NODE_OPTIONS are explicitly disallowed in packaged apps, except for the following:

If the nodeOptions fuse is disabled, NODE_OPTIONS will be ignored.

See Node.js cli documentation for details.

If the nodeOptions fuse is disabled, NODE_EXTRA_CA_CERTS will be ignored.

Geolocation support in Electron requires the use of Google Cloud Platform's geolocation webservice. To enable this feature, acquire a Google API key and place the following code in your main process file, before opening any browser windows that will make geolocation requests:

By default, a newly generated Google API key may not be allowed to make geolocation requests. To enable the geolocation webservice for your project, enable it through the API library.

N.B. You will need to add a Billing Account to the project associated to the API key for the geolocation webservice to work.

Disables ASAR support. This variable is only supported in forked child processes and spawned child processes that set ELECTRON_RUN_AS_NODE.

Starts the process as a normal Node.js process.

In this mode, you will be able to pass cli options to Node.js as you would when running the normal Node.js executable, with the exception of the following flags:

These flags are disabled owing to the fact that Electron uses BoringSSL instead of OpenSSL when building Node.js' crypto module, and so will not work as designed.

If the runAsNode fuse is disabled, ELECTRON_RUN_AS_NODE will be ignored.

Don't attach to the current console session.

Don't use the global menu bar on Linux.

Set the trash implementation on Linux. Default is gio.

The following environment variables are intended primarily for development and debugging purposes.

Prints Chromium's internal logging to the console.

Setting this variable is the same as passing --enable-logging on the command line. For more info, see --enable-logging in command-line switches.

Sets the file destination for Chromium's internal logging.

Setting this variable is the same as passing --log-file on the command line. For more info, see --log-file in command-line switches.

Adds extra logs to Notification lifecycles on macOS to aid in debugging. Extra logging will be displayed when new Notifications are created or activated. They will also be displayed when common actions are taken: a notification is shown, dismissed, its button is clicked, or it is replied to.

When Electron reads from an ASAR file, log the read offset and file path to the system tmpdir. The resulting file can be provided to the ASAR module to optimize file ordering.

Prints the stack trace to the console when Electron crashes.

This environment variable will not work if the crashReporter is started.

Shows the Windows's crash dialog when Electron crashes.

This environment variable will not work if the crashReporter is started.

When running from the electron package, this variable tells the electron command to use the specified build of Electron instead of the one downloaded by npm install. Usage:

**Examples:**

Example 1 (unknown):
```unknown
$ export ELECTRON_ENABLE_LOGGING=true$ electron
```

Example 2 (powershell):
```powershell
> set ELECTRON_ENABLE_LOGGING=true> electron
```

Example 3 (unknown):
```unknown
export NODE_OPTIONS="--no-warnings --max-old-space-size=2048"
```

Example 4 (unknown):
```unknown
--use-bundled-ca--force-fips--enable-fips--openssl-config--use-openssl-ca
```

---

## ProtocolRequest Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/protocol-request

**Contents:**
- ProtocolRequest Object

---

## protocol

**URL:** https://www.electronjs.org/docs/latest/api/protocol

**Contents:**
- protocol
- Using protocol with a custom partition or session​
- Methods​
  - protocol.registerSchemesAsPrivileged(customSchemes)​
  - protocol.handle(scheme, handler)​
  - protocol.unhandle(scheme)​
  - protocol.isProtocolHandled(scheme)​
  - protocol.registerFileProtocol(scheme, handler) Deprecated​
  - protocol.registerBufferProtocol(scheme, handler) Deprecated​
  - protocol.registerStringProtocol(scheme, handler) Deprecated​

Register a custom protocol and intercept existing protocol requests.

An example of implementing a protocol that has the same effect as the file:// protocol:

All methods unless specified can only be used after the ready event of the app module gets emitted.

A protocol is registered to a specific Electron session object. If you don't specify a session, then your protocol will be applied to the default session that Electron uses. However, if you define a partition or session on your browserWindow's webPreferences, then that window will use a different session and your custom protocol will not work if you just use electron.protocol.XXX.

To have your custom protocol work in combination with a custom session, you need to register it to that session explicitly.

The protocol module has the following methods:

This method can only be used before the ready event of the app module gets emitted and can be called only once.

Registers the scheme as standard, secure, bypasses content security policy for resources, allows registering ServiceWorker, supports fetch API, streaming video/audio, and V8 code cache. Specify a privilege with the value of true to enable the capability.

An example of registering a privileged scheme, that bypasses Content Security Policy:

A standard scheme adheres to what RFC 3986 calls generic URI syntax. For example http and https are standard schemes, while file is not.

Registering a scheme as standard allows relative and absolute resources to be resolved correctly when served. Otherwise the scheme will behave like the file protocol, but without the ability to resolve relative URLs.

For example when you load following page with custom protocol without registering it as standard scheme, the image will not be loaded because non-standard schemes can not recognize relative URLs:

Registering a scheme as standard will allow access to files through the FileSystem API. Otherwise the renderer will throw a security error for the scheme.

By default web storage apis (localStorage, sessionStorage, webSQL, indexedDB, cookies) are disabled for non standard schemes. So in general if you want to register a custom protocol to replace the http protocol, you have to register it as a standard scheme.

Protocols that use streams (http and stream protocols) should set stream: true. The <video> and <audio> HTML elements expect protocols to buffer their responses by default. The stream flag configures those elements to correctly expect streaming responses.

Register a protocol handler for scheme. Requests made to URLs with this scheme will delegate to this handler to determine what response should be sent.

Either a Response or a Promise<Response> can be returned.

See the MDN docs for Request and Response for more details.

Removes a protocol handler registered with protocol.handle.

Returns boolean - Whether scheme is already handled.

protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle

Returns boolean - Whether the protocol was successfully registered

Registers a protocol of scheme that will send a file as the response. The handler will be called with request and callback where request is an incoming request for the scheme.

To handle the request, the callback should be called with either the file's path or an object that has a path property, e.g. callback(filePath) or callback({ path: filePath }). The filePath must be an absolute path.

By default the scheme is treated like http:, which is parsed differently from protocols that follow the "generic URI syntax" like file:.

protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle

Returns boolean - Whether the protocol was successfully registered

Registers a protocol of scheme that will send a Buffer as a response.

The usage is the same with registerFileProtocol, except that the callback should be called with either a Buffer object or an object that has the data property.

protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle

Returns boolean - Whether the protocol was successfully registered

Registers a protocol of scheme that will send a string as a response.

The usage is the same with registerFileProtocol, except that the callback should be called with either a string or an object that has the data property.

protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle

Returns boolean - Whether the protocol was successfully registered

Registers a protocol of scheme that will send an HTTP request as a response.

The usage is the same with registerFileProtocol, except that the callback should be called with an object that has the url property.

protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle

Returns boolean - Whether the protocol was successfully registered

Registers a protocol of scheme that will send a stream as a response.

The usage is the same with registerFileProtocol, except that the callback should be called with either a ReadableStream object or an object that has the data property.

It is possible to pass any object that implements the readable stream API (emits data/end/error events). For example, here's how a file could be returned:

protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle

Returns boolean - Whether the protocol was successfully unregistered

Unregisters the custom protocol of scheme.

protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle

Returns boolean - Whether scheme is already registered.

protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle

Returns boolean - Whether the protocol was successfully intercepted

Intercepts scheme protocol and uses handler as the protocol's new handler which sends a file as a response.

protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle

Returns boolean - Whether the protocol was successfully intercepted

Intercepts scheme protocol and uses handler as the protocol's new handler which sends a string as a response.

protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle

Returns boolean - Whether the protocol was successfully intercepted

Intercepts scheme protocol and uses handler as the protocol's new handler which sends a Buffer as a response.

protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle

Returns boolean - Whether the protocol was successfully intercepted

Intercepts scheme protocol and uses handler as the protocol's new handler which sends a new HTTP request as a response.

protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle

Returns boolean - Whether the protocol was successfully intercepted

Same as protocol.registerStreamProtocol, except that it replaces an existing protocol handler.

protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle

Returns boolean - Whether the protocol was successfully unintercepted

Remove the interceptor installed for scheme and restore its original handler.

protocol.register*Protocol and protocol.intercept*Protocol methods have been replaced with protocol.handle

Returns boolean - Whether scheme is already intercepted.

**Examples:**

Example 1 (javascript):
```javascript
const { app, protocol, net } = require('electron')const path = require('node:path')const url = require('node:url')app.whenReady().then(() => {  protocol.handle('atom', (request) => {    const filePath = request.url.slice('atom://'.length)    return net.fetch(url.pathToFileURL(path.join(__dirname, filePath)).toString())  })})
```

Example 2 (javascript):
```javascript
const { app, BrowserWindow, net, protocol, session } = require('electron')const path = require('node:path')const url = require('node:url')app.whenReady().then(() => {  const partition = 'persist:example'  const ses = session.fromPartition(partition)  ses.protocol.handle('atom', (request) => {    const filePath = request.url.slice('atom://'.length)    return net.fetch(url.pathToFileURL(path.resolve(__dirname, filePath)).toString())  })  const mainWindow = new BrowserWindow({ webPreferences: { partition } })})
```

Example 3 (css):
```css
const { protocol } = require('electron')protocol.registerSchemesAsPrivileged([  { scheme: 'foo', privileges: { bypassCSP: true } }])
```

Example 4 (html):
```html
<body>  <img src='test.png'></body>
```

---

## MemoryUsageDetails Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/memory-usage-details

**Contents:**
- MemoryUsageDetails Object

---

## ProxyConfig Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/proxy-config

**Contents:**
- ProxyConfig Object

When mode is unspecified, pacScript and proxyRules are provided together, the proxyRules option is ignored and pacScript configuration is applied.

The proxyRules has to follow the rules below:

The proxyBypassRules is a comma separated list of rules described below:

[ URL_SCHEME "://" ] HOSTNAME_PATTERN [ ":" <port> ]

Match all hostnames that match the pattern HOSTNAME_PATTERN.

Examples: "foobar.com", "*foobar.com", "*.foobar.com", "*foobar.com:99", "https://x.\*.y.com:99"

"." HOSTNAME_SUFFIX_PATTERN [ ":" PORT ]

Match a particular domain suffix.

Examples: ".google.com", ".com", "http://.google.com"

[ SCHEME "://" ] IP_LITERAL [ ":" PORT ]

Match URLs which are IP address literals.

Examples: "127.0.1", "[0:0::1]", "[::1]", "http://[::1]:99"

IP_LITERAL "/" PREFIX_LENGTH_IN_BITS

Match any URL that is to an IP literal that falls between the given range. IP range is specified using CIDR notation.

Examples: "192.168.1.1/16", "fefe:13::abc/33".

Match local addresses. The meaning of <local> is whether the host matches one of: "127.0.0.1", "::1", "localhost".

**Examples:**

Example 1 (typescript):
```typescript
proxyRules = schemeProxies[";"<schemeProxies>]schemeProxies = [<urlScheme>"="]<proxyURIList>urlScheme = "http" | "https" | "ftp" | "socks"proxyURIList = <proxyURL>[","<proxyURIList>]proxyURL = [<proxyScheme>"://"]<proxyHost>[":"<proxyPort>]
```

---

## ServiceWorkerInfo Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/service-worker-info

**Contents:**
- ServiceWorkerInfo Object

---

## webFrame

**URL:** https://www.electronjs.org/docs/latest/api/web-frame

**Contents:**
- webFrame
- Methods​
  - webFrame.setZoomFactor(factor)​
  - webFrame.getZoomFactor()​
  - webFrame.setZoomLevel(level)​
  - webFrame.getZoomLevel()​
  - webFrame.setVisualZoomLevelLimits(minimumLevel, maximumLevel)​
  - webFrame.setSpellCheckProvider(language, provider)​
  - webFrame.insertCSS(css[, options])​
  - webFrame.removeInsertedCSS(key)​

Customize the rendering of the current web page.

If you want to call this API from a renderer process with context isolation enabled, place the API call in your preload script and expose it using the contextBridge API.

webFrame export of the Electron module is an instance of the WebFrame class representing the current frame. Sub-frames can be retrieved by certain properties and methods (e.g. webFrame.firstChild).

An example of zooming current page to 200%.

The WebFrame class has the following instance methods:

Changes the zoom factor to the specified factor. Zoom factor is zoom percent divided by 100, so 300% = 3.0.

The factor must be greater than 0.0.

Returns number - The current zoom factor.

Changes the zoom level to the specified level. The original size is 0 and each increment above or below represents zooming 20% larger or smaller to default limits of 300% and 50% of original size, respectively.

The zoom policy at the Chromium level is same-origin, meaning that the zoom level for a specific domain propagates across all instances of windows with the same domain. Differentiating the window URLs will make zoom work per-window.

Returns number - The current zoom level.

Sets the maximum and minimum pinch-to-zoom level.

Visual zoom is disabled by default in Electron. To re-enable it, call:webFrame.setVisualZoomLevelLimits(1, 3)

Visual zoom only applies to pinch-to-zoom behavior. Cmd+/-/0 zoom shortcuts are controlled by the 'zoomIn', 'zoomOut', and 'resetZoom' MenuItem roles in the application Menu. To disable shortcuts, manually define the Menu and omit zoom roles from the definition.

Sets a provider for spell checking in input fields and text areas.

If you want to use this method you must disable the builtin spellchecker when you construct the window.

The provider must be an object that has a spellCheck method that accepts an array of individual words for spellchecking. The spellCheck function runs asynchronously and calls the callback function with an array of misspelt words when complete.

An example of using node-spellchecker as provider:

Returns string - A key for the inserted CSS that can later be used to remove the CSS via webFrame.removeInsertedCSS(key).

Injects CSS into the current web page and returns a unique key for the inserted stylesheet.

Removes the inserted CSS from the current web page. The stylesheet is identified by its key, which is returned from webFrame.insertCSS(css).

Inserts text to the focused element.

Returns Promise<any> - A promise that resolves with the result of the executed code or is rejected if execution throws or results in a rejected promise.

Evaluates code in page.

In the browser window some HTML APIs like requestFullScreen can only be invoked by a gesture from the user. Setting userGesture to true will remove this limitation.

Returns Promise<any> - A promise that resolves with the result of the executed code or is rejected if execution could not start.

Works like executeJavaScript but evaluates scripts in an isolated context.

Note that when the execution of script fails, the returned promise will not reject and the result would be undefined. This is because Chromium does not dispatch errors of isolated worlds to foreign worlds.

Set the security origin, content security policy and name of the isolated world.

If the csp is specified, then the securityOrigin also has to be specified.

Returns an object describing usage information of Blink's internal memory caches.

Attempts to free memory that is no longer being used (like images from a previous navigation).

Note that blindly calling this method probably makes Electron slower since it will have to refill these emptied caches, you should only call it if an event in your app has occurred that makes you think your page is actually using less memory (i.e. you have navigated from a super heavy page to a mostly empty one, and intend to stay there).

Returns WebFrame | null - The frame element in webFrame's document selected by selector, null would be returned if selector does not select a frame or if the frame is not in the current renderer process.

Returns WebFrame | null - A child of webFrame with the supplied name, null would be returned if there's no such frame or if the frame is not in the current renderer process.

Returns WebFrame | null - that has the supplied routingId, null if not found.

Deprecated: Use the new webFrame.findFrameByToken API.

Returns WebFrame | null - that has the supplied frameToken, null if not found.

Returns boolean - True if the word is misspelled according to the built in spellchecker, false otherwise. If no dictionary is loaded, always return false.

Returns string[] - A list of suggested words for a given word. If the word is spelled correctly, the result will be empty.

A WebFrame | null representing top frame in frame hierarchy to which webFrame belongs, the property would be null if top frame is not in the current renderer process.

A WebFrame | null representing the frame which opened webFrame, the property would be null if there's no opener or opener is not in the current renderer process.

A WebFrame | null representing parent frame of webFrame, the property would be null if webFrame is top or parent is not in the current renderer process.

A WebFrame | null representing the first child frame of webFrame, the property would be null if webFrame has no children or if first child is not in the current renderer process.

A WebFrame | null representing next sibling frame, the property would be null if webFrame is the last frame in its parent or if the next sibling is not in the current renderer process.

An Integer representing the unique frame id in the current renderer process. Distinct WebFrame instances that refer to the same underlying frame will have the same routingId.

Deprecated: Use the new webFrame.frameToken API.

A string representing the unique frame token in the current renderer process. Distinct WebFrame instances that refer to the same underlying frame will have the same frameToken.

**Examples:**

Example 1 (javascript):
```javascript
const { webFrame } = require('electron')webFrame.setZoomFactor(2)
```

Example 2 (unknown):
```unknown
webFrame.setVisualZoomLevelLimits(1, 3)
```

Example 3 (css):
```css
const mainWindow = new BrowserWindow({  webPreferences: {    spellcheck: false  }})
```

Example 4 (javascript):
```javascript
const { webFrame } = require('electron')const spellChecker = require('spellchecker')webFrame.setSpellCheckProvider('en-US', {  spellCheck (words, callback) {    setTimeout(() => {      const misspelled = words.filter(x => spellchecker.isMisspelled(x))      callback(misspelled)    }, 0)  }})
```

---

## session

**URL:** https://www.electronjs.org/docs/latest/api/session

**Contents:**
- session
- Methods​
  - session.fromPartition(partition[, options])​
  - session.fromPath(path[, options])​
- Properties​
  - session.defaultSession​
- Class: Session​
  - Instance Events​
    - Event: 'will-download'​
    - Event: 'extension-loaded'​

Manage browser sessions, cookies, cache, proxy settings, etc.

The session module can be used to create new Session objects.

You can also access the session of existing pages by using the session property of WebContents, or from the session module.

The session module has the following methods:

Returns Session - A session instance from partition string. When there is an existing Session with the same partition, it will be returned; otherwise a new Session instance will be created with options.

If partition starts with persist:, the page will use a persistent session available to all pages in the app with the same partition. if there is no persist: prefix, the page will use an in-memory session. If the partition is empty then default session of the app will be returned.

To create a Session with options, you have to ensure the Session with the partition has never been used before. There is no way to change the options of an existing Session object.

Returns Session - A session instance from the absolute path as specified by the path string. When there is an existing Session with the same absolute path, it will be returned; otherwise a new Session instance will be created with options. The call will throw an error if the path is not an absolute path. Additionally, an error will be thrown if an empty string is provided.

To create a Session with options, you have to ensure the Session with the path has never been used before. There is no way to change the options of an existing Session object.

The session module has the following properties:

A Session object, the default session object of the app.

Get and set properties of a session.

Process: Main This class is not exported from the 'electron' module. It is only available as a return value of other methods in the Electron API.

You can create a Session object in the session module:

The following events are available on instances of Session:

Emitted when Electron is about to download item in webContents.

Calling event.preventDefault() will cancel the download and item will not be available from next tick of the process.

Emitted after an extension is loaded. This occurs whenever an extension is added to the "enabled" set of extensions. This includes:

Emitted after an extension is unloaded. This occurs when Session.removeExtension is called.

Emitted after an extension is loaded and all necessary browser state is initialized to support the start of the extension's background page.

Emitted when a render process requests preconnection to a URL, generally due to a resource hint.

Emitted when a hunspell dictionary file has been successfully initialized. This occurs after the file has been downloaded.

Emitted when a hunspell dictionary file starts downloading

Emitted when a hunspell dictionary file has been successfully downloaded

Emitted when a hunspell dictionary file download fails. For details on the failure you should collect a netlog and inspect the download request.

Emitted when a HID device needs to be selected when a call to navigator.hid.requestDevice is made. callback should be called with deviceId to be selected; passing no arguments to callback will cancel the request. Additionally, permissioning on navigator.hid can be further managed by using ses.setPermissionCheckHandler(handler) and ses.setDevicePermissionHandler(handler).

Emitted after navigator.hid.requestDevice has been called and select-hid-device has fired if a new device becomes available before the callback from select-hid-device is called. This event is intended for use when using a UI to ask users to pick a device so that the UI can be updated with the newly added device.

Emitted after navigator.hid.requestDevice has been called and select-hid-device has fired if a device has been removed before the callback from select-hid-device is called. This event is intended for use when using a UI to ask users to pick a device so that the UI can be updated to remove the specified device.

Emitted after HIDDevice.forget() has been called. This event can be used to help maintain persistent storage of permissions when setDevicePermissionHandler is used.

Emitted when a serial port needs to be selected when a call to navigator.serial.requestPort is made. callback should be called with portId to be selected, passing an empty string to callback will cancel the request. Additionally, permissioning on navigator.serial can be managed by using ses.setPermissionCheckHandler(handler) with the serial permission.

Emitted after navigator.serial.requestPort has been called and select-serial-port has fired if a new serial port becomes available before the callback from select-serial-port is called. This event is intended for use when using a UI to ask users to pick a port so that the UI can be updated with the newly added port.

Emitted after navigator.serial.requestPort has been called and select-serial-port has fired if a serial port has been removed before the callback from select-serial-port is called. This event is intended for use when using a UI to ask users to pick a port so that the UI can be updated to remove the specified port.

Emitted after SerialPort.forget() has been called. This event can be used to help maintain persistent storage of permissions when setDevicePermissionHandler is used.

Emitted when a USB device needs to be selected when a call to navigator.usb.requestDevice is made. callback should be called with deviceId to be selected; passing no arguments to callback will cancel the request. Additionally, permissioning on navigator.usb can be further managed by using ses.setPermissionCheckHandler(handler) and ses.setDevicePermissionHandler(handler).

Emitted after navigator.usb.requestDevice has been called and select-usb-device has fired if a new device becomes available before the callback from select-usb-device is called. This event is intended for use when using a UI to ask users to pick a device so that the UI can be updated with the newly added device.

Emitted after navigator.usb.requestDevice has been called and select-usb-device has fired if a device has been removed before the callback from select-usb-device is called. This event is intended for use when using a UI to ask users to pick a device so that the UI can be updated to remove the specified device.

Emitted after USBDevice.forget() has been called. This event can be used to help maintain persistent storage of permissions when setDevicePermissionHandler is used.

The following methods are available on instances of Session:

Returns Promise<Integer> - the session's current cache size, in bytes.

Returns Promise<void> - resolves when the cache clear operation is complete.

Clears the session’s HTTP cache.

Returns Promise<void> - resolves when the storage data has been cleared.

Writes any unwritten DOMStorage data to disk.

Returns Promise<void> - Resolves when the proxy setting process is complete.

Sets the proxy settings.

You may need ses.closeAllConnections to close currently in flight connections to prevent pooled sockets using previous proxy from being reused by future requests.

Returns Promise<ResolvedHost> - Resolves with the resolved IP addresses for the host.

Returns Promise<string> - Resolves with the proxy information for url.

Returns Promise<void> - Resolves when the all internal states of proxy service is reset and the latest proxy configuration is reapplied if it's already available. The pac script will be fetched from pacScript again if the proxy mode is pac_script.

Sets download saving directory. By default, the download directory will be the Downloads under the respective app folder.

Emulates network with the given configuration for the session.

Preconnects the given number of sockets to an origin.

Returns Promise<void> - Resolves when all connections are closed.

It will terminate / fail all requests currently in flight.

Returns Promise<GlobalResponse> - see Response.

Sends a request, similarly to how fetch() works in the renderer, using Chrome's network stack. This differs from Node's fetch(), which uses Node.js's HTTP stack.

See also net.fetch(), a convenience method which issues requests from the default session.

See the MDN documentation for fetch() for more details.

By default, requests made with net.fetch can be made to custom protocols as well as file:, and will trigger webRequest handlers if present. When the non-standard bypassCustomProtocolHandlers option is set in RequestInit, custom protocol handlers will not be called for this request. This allows forwarding an intercepted request to the built-in handler. webRequest handlers will still be triggered when bypassing custom protocols.

Disables any network emulation already active for the session. Resets to the original network configuration.

Sets the certificate verify proc for session, the proc will be called with proc(request, callback) whenever a server certificate verification is requested. Calling callback(0) accepts the certificate, calling callback(-2) rejects it.

Calling setCertificateVerifyProc(null) will revert back to default certificate verify proc.

NOTE: The result of this procedure is cached by the network service.

Sets the handler which can be used to respond to permission requests for the session. Calling callback(true) will allow the permission and callback(false) will reject it. To clear the handler, call setPermissionRequestHandler(null). Please note that you must also implement setPermissionCheckHandler to get complete permission handling. Most web APIs do a permission check and then make a permission request if the check is denied.

Sets the handler which can be used to respond to permission checks for the session. Returning true will allow the permission and false will reject it. Please note that you must also implement setPermissionRequestHandler to get complete permission handling. Most web APIs do a permission check and then make a permission request if the check is denied. To clear the handler, call setPermissionCheckHandler(null).

isMainFrame will always be false for a fileSystem request as a result of Chromium limitations.

This handler will be called when web content requests access to display media via the navigator.mediaDevices.getDisplayMedia API. Use the desktopCapturer API to choose which stream(s) to grant access to.

useSystemPicker allows an application to use the system picker instead of providing a specific video source from getSources. This option is experimental, and currently available for MacOS 15+ only. If the system picker is available and useSystemPicker is set to true, the handler will not be invoked.

Passing a WebFrameMain object as a video or audio stream will capture the video or audio stream from that frame.

Passing null instead of a function resets the handler to its default state.

Sets the handler which can be used to respond to device permission checks for the session. Returning true will allow the device to be permitted and false will reject it. To clear the handler, call setDevicePermissionHandler(null). This handler can be used to provide default permissioning to devices without first calling for permission to devices (eg via navigator.hid.requestDevice). If this handler is not defined, the default device permissions as granted through device selection (eg via navigator.hid.requestDevice) will be used. Additionally, the default behavior of Electron is to store granted device permission in memory. If longer term storage is needed, a developer can store granted device permissions (eg when handling the select-hid-device event) and then read from that storage with setDevicePermissionHandler.

Sets the handler which can be used to override which USB classes are protected. The return value for the handler is a string array of USB classes which should be considered protected (eg not available in the renderer). Valid values for the array are:

Returning an empty string array from the handler will allow all USB classes; returning the passed in array will maintain the default list of protected USB classes (this is also the default behavior if a handler is not defined). To clear the handler, call setUSBProtectedClassesHandler(null).

Sets a handler to respond to Bluetooth pairing requests. This handler allows developers to handle devices that require additional validation before pairing. When a handler is not defined, any pairing on Linux or Windows that requires additional validation will be automatically cancelled. macOS does not require a handler because macOS handles the pairing automatically. To clear the handler, call setBluetoothPairingHandler(null).

Returns Promise<void> - Resolves when the operation is complete.

Clears the host resolver cache.

Dynamically sets whether to always send credentials for HTTP NTLM or Negotiate authentication.

Overrides the userAgent and acceptLanguages for this session.

The acceptLanguages must a comma separated ordered list of language codes, for example "en-US,fr,de,ko,zh-CN,ja".

This doesn't affect existing WebContents, and each WebContents can use webContents.setUserAgent to override the session-wide user agent.

Returns boolean - Whether or not this session is a persistent one. The default webContents session of a BrowserWindow is persistent. When creating a session from a partition, session prefixed with persist: will be persistent, while others will be temporary.

Returns string - The user agent for this session.

Sets the SSL configuration for the session. All subsequent network requests will use the new configuration. Existing network connections (such as WebSocket connections) will not be terminated, but old sockets in the pool will not be reused for new connections.

Returns Promise<Buffer> - resolves with blob data.

Initiates a download of the resource at url. The API will generate a DownloadItem that can be accessed with the will-download event.

This does not perform any security checks that relate to a page's origin, unlike webContents.downloadURL.

Allows resuming cancelled or interrupted downloads from previous Session. The API will generate a DownloadItem that can be accessed with the will-download event. The DownloadItem will not have any WebContents associated with it and the initial state will be interrupted. The download will start only when the resume API is called on the DownloadItem.

Returns Promise<void> - resolves when the session’s HTTP authentication cache has been cleared.

Adds scripts that will be executed on ALL web contents that are associated with this session just before normal preload scripts run.

Deprecated: Use the new ses.registerPreloadScript API.

Returns string[] an array of paths to preload scripts that have been registered.

Deprecated: Use the new ses.getPreloadScripts API. This will only return preload script paths for frame context types.

Registers preload script that will be executed in its associated context type in this session. For frame contexts, this will run prior to any preload defined in the web preferences of a WebContents.

Returns string - The ID of the registered preload script.

Returns PreloadScript[]: An array of paths to preload scripts that have been registered.

Sets the directory to store the generated JS code cache for this session. The directory is not required to be created by the user before this call, the runtime will create if it does not exist otherwise will use the existing directory. If directory cannot be created, then code cache will not be used and all operations related to code cache will fail silently inside the runtime. By default, the directory will be Code Cache under the respective user data folder.

Note that by default code cache is only enabled for http(s) URLs, to enable code cache for custom protocols, codeCache: true and standard: true must be specified when registering the protocol.

Returns Promise<void> - resolves when the code cache clear operation is complete.

Returns Promise<SharedDictionaryUsageInfo[]> - an array of shared dictionary information entries in Chromium's networking service's storage.

Shared dictionaries are used to power advanced compression of data sent over the wire, specifically with Brotli and ZStandard. You don't need to call any of the shared dictionary APIs in Electron to make use of this advanced web feature, but if you do, they allow deeper control and inspection of the shared dictionaries used during decompression.

To get detailed information about a specific shared dictionary entry, call getSharedDictionaryInfo(options).

Returns Promise<SharedDictionaryInfo[]> - an array of shared dictionary information entries in Chromium's networking service's storage.

To get information about all present shared dictionaries, call getSharedDictionaryUsageInfo().

Returns Promise<void> - resolves when the dictionary cache has been cleared, both in memory and on disk.

Returns Promise<void> - resolves when the dictionary cache has been cleared for the specified isolation key, both in memory and on disk.

Sets whether to enable the builtin spell checker.

Returns boolean - Whether the builtin spell checker is enabled.

The built in spellchecker does not automatically detect what language a user is typing in. In order for the spell checker to correctly check their words you must call this API with an array of language codes. You can get the list of supported language codes with the ses.availableSpellCheckerLanguages property.

On macOS, the OS spellchecker is used and will detect your language automatically. This API is a no-op on macOS.

Returns string[] - An array of language codes the spellchecker is enabled for. If this list is empty the spellchecker will fallback to using en-US. By default on launch if this setting is an empty list Electron will try to populate this setting with the current OS locale. This setting is persisted across restarts.

On macOS, the OS spellchecker is used and has its own list of languages. On macOS, this API will return whichever languages have been configured by the OS.

By default Electron will download hunspell dictionaries from the Chromium CDN. If you want to override this behavior you can use this API to point the dictionary downloader at your own hosted version of the hunspell dictionaries. We publish a hunspell_dictionaries.zip file with each release which contains the files you need to host here.

The file server must be case insensitive. If you cannot do this, you must upload each file twice: once with the case it has in the ZIP file and once with the filename as all lowercase.

If the files present in hunspell_dictionaries.zip are available at https://example.com/dictionaries/language-code.bdic then you should call this api with ses.setSpellCheckerDictionaryDownloadURL('https://example.com/dictionaries/'). Please note the trailing slash. The URL to the dictionaries is formed as ${url}${filename}.

On macOS, the OS spellchecker is used and therefore we do not download any dictionary files. This API is a no-op on macOS.

Returns Promise<string[]> - An array of all words in app's custom dictionary. Resolves when the full dictionary is loaded from disk.

Returns boolean - Whether the word was successfully written to the custom dictionary. This API will not work on non-persistent (in-memory) sessions.

On macOS and Windows, this word will be written to the OS custom dictionary as well.

Returns boolean - Whether the word was successfully removed from the custom dictionary. This API will not work on non-persistent (in-memory) sessions.

On macOS and Windows, this word will be removed from the OS custom dictionary as well.

Returns Promise<Extension> - resolves when the extension is loaded.

This method will raise an exception if the extension could not be loaded. If there are warnings when installing the extension (e.g. if the extension requests an API that Electron does not support) then they will be logged to the console.

Note that Electron does not support the full range of Chrome extensions APIs. See Supported Extensions APIs for more details on what is supported.

Note that in previous versions of Electron, extensions that were loaded would be remembered for future runs of the application. This is no longer the case: loadExtension must be called on every boot of your app if you want the extension to be loaded.

This API does not support loading packed (.crx) extensions.

This API cannot be called before the ready event of the app module is emitted.

Loading extensions into in-memory (non-persistent) sessions is not supported and will throw an error.

Deprecated: Use the new ses.extensions.loadExtension API.

Unloads an extension.

This API cannot be called before the ready event of the app module is emitted.

Deprecated: Use the new ses.extensions.removeExtension API.

Returns Extension | null - The loaded extension with the given ID.

This API cannot be called before the ready event of the app module is emitted.

Deprecated: Use the new ses.extensions.getExtension API.

Returns Extension[] - A list of all loaded extensions.

This API cannot be called before the ready event of the app module is emitted.

Deprecated: Use the new ses.extensions.getAllExtensions API.

Returns string | null - The absolute file system path where data for this session is persisted on disk. For in memory sessions this returns null.

Returns Promise<void> - resolves when all data has been cleared.

Clears various different types of data.

This method clears more types of data and is more thorough than the clearStorageData method.

Cookies are stored at a broader scope than origins. When removing cookies and filtering by origins (or excludeOrigins), the cookies will be removed at the registrable domain level. For example, clearing cookies for the origin https://really.specific.origin.example.com/ will end up clearing all cookies for example.com. Clearing cookies for the origin https://my.website.example.co.uk/ will end up clearing all cookies for example.co.uk.

Clearing cache data will also clear the shared dictionary cache. This means that any dictionaries used for compression may be reloaded after clearing the cache. If you wish to clear the shared dictionary cache but leave other cached data intact, you may want to use the clearSharedDictionaryCache method.

For more information, refer to Chromium's BrowsingDataRemover interface.

The following properties are available on instances of Session:

A string[] array which consists of all the known available spell checker languages. Providing a language code to the setSpellCheckerLanguages API that isn't in this array will result in an error.

A boolean indicating whether builtin spell checker is enabled.

A string | null indicating the absolute file system path where data for this session is persisted on disk. For in memory sessions this returns null.

A Cookies object for this session.

A Extensions object for this session.

A ServiceWorkers object for this session.

A WebRequest object for this session.

A Protocol object for this session.

A NetLog object for this session.

**Examples:**

Example 1 (javascript):
```javascript
const { BrowserWindow } = require('electron')const win = new BrowserWindow({ width: 800, height: 600 })win.loadURL('https://github.com')const ses = win.webContents.sessionconsole.log(ses.getUserAgent())
```

Example 2 (javascript):
```javascript
const { session } = require('electron')const ses = session.fromPartition('persist:name')console.log(ses.getUserAgent())
```

Example 3 (javascript):
```javascript
const { session } = require('electron')session.defaultSession.on('will-download', (event, item, webContents) => {  event.preventDefault()  require('got')(item.getURL()).then((response) => {    require('node:fs').writeFileSync('/somewhere', response.body)  })})
```

Example 4 (javascript):
```javascript
const { app, dialog, BrowserWindow, session } = require('electron')async function createWindow () {  const mainWindow = new BrowserWindow()  await mainWindow.loadURL('https://buzzfeed.com')  session.defaultSession.on('file-system-access-restricted', async (e, details, callback) => {    const { origin, path } = details    const { response } = await dialog.showMessageBox({      message: `Are you sure you want ${origin} to open restricted path ${path}?`,      title: 'File System Access Restricted',      buttons: ['Choose a different folder', 'Allow', 'Cancel'],      cancelId: 2    })    if (response === 0) {      callback('tryAgain')    } else if (response === 1) {      callback('allow')    } else {      callback('deny')    }  })  mainWindow.webContents.executeJavaScript(`    window.showDirectoryPicker({      id: 'electron-demo',      mode: 'readwrite',      startIn: 'downloads',    }).catch(e => {      console.log(e)    })`, true  )}app.whenReady().then(() => {  createWindow()  app.on('activate', () => {    if (BrowserWindow.getAllWindows().length === 0) createWindow()  })})app.on('window-all-closed', function () {  if (process.platform !== 'darwin') app.quit()})
```

---

## InputEvent Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/input-event

**Contents:**
- InputEvent Object

---

## BrowserWindowConstructorOptions Object extends BaseWindowConstructorOptions

**URL:** https://www.electronjs.org/docs/latest/api/structures/browser-window-options

**Contents:**
- BrowserWindowConstructorOptions Object extends BaseWindowConstructorOptions

---

## SharedDictionaryInfo Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/shared-dictionary-info

**Contents:**
- SharedDictionaryInfo Object

---

## ShortcutDetails Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/shortcut-details

**Contents:**
- ShortcutDetails Object

---

## FilesystemPermissionRequest Object extends PermissionRequest

**URL:** https://www.electronjs.org/docs/latest/api/structures/filesystem-permission-request

**Contents:**
- FilesystemPermissionRequest Object extends PermissionRequest

---

## parentPort

**URL:** https://www.electronjs.org/docs/latest/api/parent-port

**Contents:**
- parentPort
- Events​
  - Event: 'message'​
- Methods​
  - parentPort.postMessage(message)​

Interface for communication with parent process.

parentPort is an EventEmitter. This object is not exported from the 'electron' module. It is only available as a property of the process object in the Electron API.

The parentPort object emits the following events:

Emitted when the process receives a message. Messages received on this port will be queued up until a handler is registered for this event.

Sends a message from the process to its parent.

**Examples:**

Example 1 (javascript):
```javascript
// Main processconst child = utilityProcess.fork(path.join(__dirname, 'test.js'))child.postMessage({ message: 'hello' })child.on('message', (data) => {  console.log(data) // hello world!})// Child processprocess.parentPort.on('message', (e) => {  process.parentPort.postMessage(`${e.data} world!`)})
```

---

## Task Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/task

**Contents:**
- Task Object

---

## ipcRenderer

**URL:** https://www.electronjs.org/docs/latest/api/ipc-renderer

**Contents:**
- ipcRenderer
- Methods​
  - ipcRenderer.on(channel, listener)​
  - ipcRenderer.off(channel, listener)​
  - ipcRenderer.once(channel, listener)​
  - ipcRenderer.addListener(channel, listener)​
  - ipcRenderer.removeListener(channel, listener)​
  - ipcRenderer.removeAllListeners([channel])​
  - ipcRenderer.send(channel, ...args)​
  - ipcRenderer.invoke(channel, ...args)​

ipcRenderer can no longer be sent over the contextBridge

Communicate asynchronously from a renderer process to the main process.

If you want to call this API from a renderer process with context isolation enabled, place the API call in your preload script and expose it using the contextBridge API.

The ipcRenderer module is an EventEmitter. It provides a few methods so you can send synchronous and asynchronous messages from the render process (web page) to the main process. You can also receive replies from the main process.

See IPC tutorial for code examples.

The ipcRenderer module has the following method to listen for events and send messages:

Listens to channel, when a new message arrives listener would be called with listener(event, args...).

Do not expose the event argument to the renderer for security reasons! Wrap any callback that you receive from the renderer in another function like this: ipcRenderer.on('my-channel', (event, ...args) => callback(...args)). Not wrapping the callback in such a function would expose dangerous Electron APIs to the renderer process. See the security guide for more info.

Removes the specified listener from the listener array for the specified channel.

Adds a one time listener function for the event. This listener is invoked only the next time a message is sent to channel, after which it is removed.

Alias for ipcRenderer.on.

Alias for ipcRenderer.off.

Removes all listeners from the specified channel. Removes all listeners from all channels if no channel is specified.

Send an asynchronous message to the main process via channel, along with arguments. Arguments will be serialized with the Structured Clone Algorithm, just like window.postMessage, so prototype chains will not be included. Sending Functions, Promises, Symbols, WeakMaps, or WeakSets will throw an exception.

NOTE: Sending non-standard JavaScript types such as DOM objects or special Electron objects will throw an exception.

Since the main process does not have support for DOM objects such as ImageBitmap, File, DOMMatrix and so on, such objects cannot be sent over Electron's IPC to the main process, as the main process would have no way to decode them. Attempting to send such objects over IPC will result in an error.

The main process handles it by listening for channel with the ipcMain module.

If you need to transfer a MessagePort to the main process, use ipcRenderer.postMessage.

If you want to receive a single response from the main process, like the result of a method call, consider using ipcRenderer.invoke.

Returns Promise<any> - Resolves with the response from the main process.

Send a message to the main process via channel and expect a result asynchronously. Arguments will be serialized with the Structured Clone Algorithm, just like window.postMessage, so prototype chains will not be included. Sending Functions, Promises, Symbols, WeakMaps, or WeakSets will throw an exception.

The main process should listen for channel with ipcMain.handle().

If you need to transfer a MessagePort to the main process, use ipcRenderer.postMessage.

If you do not need a response to the message, consider using ipcRenderer.send.

Sending non-standard JavaScript types such as DOM objects or special Electron objects will throw an exception.Since the main process does not have support for DOM objects such as ImageBitmap, File, DOMMatrix and so on, such objects cannot be sent over Electron's IPC to the main process, as the main process would have no way to decode them. Attempting to send such objects over IPC will result in an error.

Since the main process does not have support for DOM objects such as ImageBitmap, File, DOMMatrix and so on, such objects cannot be sent over Electron's IPC to the main process, as the main process would have no way to decode them. Attempting to send such objects over IPC will result in an error.

If the handler in the main process throws an error, the promise returned by invoke will reject. However, the Error object in the renderer process will not be the same as the one thrown in the main process.

Returns any - The value sent back by the ipcMain handler.

Send a message to the main process via channel and expect a result synchronously. Arguments will be serialized with the Structured Clone Algorithm, just like window.postMessage, so prototype chains will not be included. Sending Functions, Promises, Symbols, WeakMaps, or WeakSets will throw an exception.

NOTE: Sending non-standard JavaScript types such as DOM objects or special Electron objects will throw an exception.

Since the main process does not have support for DOM objects such as ImageBitmap, File, DOMMatrix and so on, such objects cannot be sent over Electron's IPC to the main process, as the main process would have no way to decode them. Attempting to send such objects over IPC will result in an error.

The main process handles it by listening for channel with ipcMain module, and replies by setting event.returnValue.

Sending a synchronous message will block the whole renderer process until the reply is received, so use this method only as a last resort. It's much better to use the asynchronous version, invoke().

Send a message to the main process, optionally transferring ownership of zero or more MessagePort objects.

The transferred MessagePort objects will be available in the main process as MessagePortMain objects by accessing the ports property of the emitted event.

For more information on using MessagePort and MessageChannel, see the MDN documentation.

Like ipcRenderer.send but the event will be sent to the <webview> element in the host page instead of the main process.

**Examples:**

Example 1 (javascript):
```javascript
// Renderer processipcRenderer.invoke('some-name', someArgument).then((result) => {  // ...})// Main processipcMain.handle('some-name', async (event, someArgument) => {  const result = await doSomeWork(someArgument)  return result})
```

Example 2 (csharp):
```csharp
// Renderer processconst { port1, port2 } = new MessageChannel()ipcRenderer.postMessage('port', { message: 'hello' }, [port1])// Main processipcMain.on('port', (e, msg) => {  const [port] = e.ports  // ...})
```

---

## contextBridge

**URL:** https://www.electronjs.org/docs/latest/api/context-bridge

**Contents:**
- contextBridge
- Glossary​
  - Main World​
  - Isolated World​
- Methods​
  - contextBridge.exposeInMainWorld(apiKey, api)​
  - contextBridge.exposeInIsolatedWorld(worldId, apiKey, api)​
  - contextBridge.executeInMainWorld(executionScript) Experimental​
- Usage​
  - API​

ipcRenderer can no longer be sent over the contextBridge

Create a safe, bi-directional, synchronous bridge across isolated contexts

An example of exposing an API to a renderer from an isolated preload script is given below:

The "Main World" is the JavaScript context that your main renderer code runs in. By default, the page you load in your renderer executes code in this world.

When contextIsolation is enabled in your webPreferences (this is the default behavior since Electron 12.0.0), your preload scripts run in an "Isolated World". You can read more about context isolation and what it affects in the security docs.

The contextBridge module has the following methods:

Returns any - A copy of the resulting value from executing the function in the main world. Refer to the table on how values are copied between worlds.

The api provided to exposeInMainWorld must be a Function, string, number, Array, boolean, or an object whose keys are strings and values are a Function, string, number, Array, boolean, or another nested object that meets the same conditions.

Function values are proxied to the other context and all other values are copied and frozen. Any data / primitives sent in the API become immutable and updates on either side of the bridge do not result in an update on the other side.

An example of a complex API is shown below:

An example of exposeInIsolatedWorld is shown below:

Function values that you bind through the contextBridge are proxied through Electron to ensure that contexts remain isolated. This results in some key limitations that we've outlined below.

Because parameters, errors and return values are copied when they are sent over the bridge, there are only certain types that can be used. At a high level, if the type you want to use can be serialized and deserialized into the same object it will work. A table of type support has been included below for completeness:

If the type you care about is not in the above table, it is probably not supported.

Attempting to send the entire ipcRenderer module as an object over the contextBridge will result in an empty object on the receiving side of the bridge. Sending over ipcRenderer in full can let any code send any message, which is a security footgun. To interact through ipcRenderer, provide a safe wrapper like below:

The contextBridge can be used by the preload script to give your renderer access to Node APIs. The table of supported types described above also applies to Node APIs that you expose through contextBridge. Please note that many Node APIs grant access to local system resources. Be very cautious about which globals and APIs you expose to untrusted remote content.

**Examples:**

Example 1 (javascript):
```javascript
// Preload (Isolated World)const { contextBridge, ipcRenderer } = require('electron')contextBridge.exposeInMainWorld(  'electron',  {    doThing: () => ipcRenderer.send('do-a-thing')  })
```

Example 2 (csharp):
```csharp
// Renderer (Main World)window.electron.doThing()
```

Example 3 (javascript):
```javascript
const { contextBridge, ipcRenderer } = require('electron')contextBridge.exposeInMainWorld(  'electron',  {    doThing: () => ipcRenderer.send('do-a-thing'),    myPromises: [Promise.resolve(), Promise.reject(new Error('whoops'))],    anAsyncFunction: async () => 123,    data: {      myFlags: ['a', 'b', 'c'],      bootTime: 1234    },    nestedAPI: {      evenDeeper: {        youCanDoThisAsMuchAsYouWant: {          fn: () => ({            returnData: 123          })        }      }    }  })
```

Example 4 (javascript):
```javascript
const { contextBridge, ipcRenderer } = require('electron')contextBridge.exposeInIsolatedWorld(  1004,  'electron',  {    doThing: () => ipcRenderer.send('do-a-thing')  })
```

---

## KeyboardInputEvent Object extends InputEvent

**URL:** https://www.electronjs.org/docs/latest/api/structures/keyboard-input-event

**Contents:**
- KeyboardInputEvent Object extends InputEvent

---

## BluetoothDevice Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/bluetooth-device

**Contents:**
- BluetoothDevice Object

---

## NavigationEntry Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/navigation-entry

**Contents:**
- NavigationEntry Object

---

## MenuItem

**URL:** https://www.electronjs.org/docs/latest/api/menu-item

**Contents:**
- MenuItem
- Class: MenuItem​
  - new MenuItem(options)​
  - Instance Properties​
    - menuItem.id​
    - menuItem.label​
    - menuItem.click​
    - menuItem.submenu​
    - menuItem.type​
    - menuItem.role​

Add items to native application menus and context menus.

See Menu for examples.

Electron's built-in classes cannot be subclassed in user code. For more information, see the FAQ.

acceleratorWorksWhenHidden is specified as being macOS-only because accelerators always work when items are hidden on Windows and Linux. The option is exposed to users to give them the option to turn it off, as this is possible in native macOS development.

The following properties are available on instances of MenuItem:

A string indicating the item's unique id. This property can be dynamically changed.

A string indicating the item's visible label.

A Function that is fired when the MenuItem receives a click event. It can be called with menuItem.click(event, focusedWindow, focusedWebContents).

A Menu (optional) containing the menu item's submenu, if present.

A string indicating the type of the item. Can be normal, separator, submenu, checkbox, radio, header or palette.

header and palette are only available on macOS 14 and up.

A string (optional) indicating the item's role, if set. Can be undo, redo, cut, copy, paste, pasteAndMatchStyle, delete, selectAll, reload, forceReload, toggleDevTools, resetZoom, zoomIn, zoomOut, toggleSpellChecker, togglefullscreen, window, minimize, close, help, about, services, hide, hideOthers, unhide, quit, startSpeaking, stopSpeaking, zoom, front, appMenu, fileMenu, editMenu, viewMenu, shareMenu, recentDocuments, toggleTabBar, selectNextTab, selectPreviousTab, showAllTabs, mergeAllWindows, clearRecentDocuments, moveTabToNewWindow or windowMenu

An Accelerator (optional) indicating the item's accelerator, if set.

An Accelerator | null indicating the item's user-assigned accelerator for the menu item.

This property is only initialized after the MenuItem has been added to a Menu. Either via Menu.buildFromTemplate or via Menu.append()/insert(). Accessing before initialization will just return null.

A NativeImage | string (optional) indicating the item's icon, if set.

A string indicating the item's sublabel.

A string indicating the item's hover text.

A boolean indicating whether the item is enabled. This property can be dynamically changed.

A boolean indicating whether the item is visible. This property can be dynamically changed.

A boolean indicating whether the item is checked. This property can be dynamically changed.

A checkbox menu item will toggle the checked property on and off when selected.

A radio menu item will turn on its checked property when clicked, and will turn off that property for all adjacent items in the same menu.

You can add a click function for additional behavior.

A boolean indicating if the accelerator should be registered with the system or just displayed.

This property can be dynamically changed.

A SharingItem indicating the item to share when the role is shareMenu.

This property can be dynamically changed.

A number indicating an item's sequential unique id.

A Menu that the item is a part of.

---

## ProductDiscount Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/product-discount

**Contents:**
- ProductDiscount Object

---

## JumpListCategory Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/jump-list-category

**Contents:**
- JumpListCategory Object

If a JumpListCategory object has neither the type nor the name property set then its type is assumed to be tasks. If the name property is set but the type property is omitted then the type is assumed to be custom.

The maximum length of a Jump List item's description property is 260 characters. Beyond this limit, the item will not be added to the Jump List, nor will it be displayed.

---

## app

**URL:** https://www.electronjs.org/docs/latest/api/app

**Contents:**
- app
- Events​
  - Event: 'will-finish-launching'​
  - Event: 'ready'​
  - Event: 'window-all-closed'​
  - Event: 'before-quit'​
  - Event: 'will-quit'​
  - Event: 'quit'​
  - Event: 'open-file' macOS​
  - Event: 'open-url' macOS​

Control your application's event lifecycle.

The following example shows how to quit the application when the last window is closed:

The app object emits the following events:

Emitted when the application has finished basic startup. On Windows and Linux, the will-finish-launching event is the same as the ready event; on macOS, this event represents the applicationWillFinishLaunching notification of NSApplication.

In most cases, you should do everything in the ready event handler.

Emitted once, when Electron has finished initializing. On macOS, launchInfo holds the userInfo of the NSUserNotification or information from UNNotificationResponse that was used to open the application, if it was launched from Notification Center. You can also call app.isReady() to check if this event has already fired and app.whenReady() to get a Promise that is fulfilled when Electron is initialized.

The ready event is only fired after the main process has finished running the first tick of the event loop. If an Electron API needs to be called before the ready event, ensure that it is called synchronously in the top-level context of the main process.

Emitted when all windows have been closed.

If you do not subscribe to this event and all windows are closed, the default behavior is to quit the app; however, if you subscribe, you control whether the app quits or not. If the user pressed Cmd + Q, or the developer called app.quit(), Electron will first try to close all the windows and then emit the will-quit event, and in this case the window-all-closed event would not be emitted.

Emitted before the application starts closing its windows. Calling event.preventDefault() will prevent the default behavior, which is terminating the application.

If application quit was initiated by autoUpdater.quitAndInstall(), then before-quit is emitted after emitting close event on all windows and closing them.

On Windows, this event will not be emitted if the app is closed due to a shutdown/restart of the system or a user logout.

Emitted when all windows have been closed and the application will quit. Calling event.preventDefault() will prevent the default behavior, which is terminating the application.

See the description of the window-all-closed event for the differences between the will-quit and window-all-closed events.

On Windows, this event will not be emitted if the app is closed due to a shutdown/restart of the system or a user logout.

Emitted when the application is quitting.

On Windows, this event will not be emitted if the app is closed due to a shutdown/restart of the system or a user logout.

Emitted when the user wants to open a file with the application. The open-file event is usually emitted when the application is already open and the OS wants to reuse the application to open the file. open-file is also emitted when a file is dropped onto the dock and the application is not yet running. Make sure to listen for the open-file event very early in your application startup to handle this case (even before the ready event is emitted).

You should call event.preventDefault() if you want to handle this event.

On Windows, you have to parse process.argv (in the main process) to get the filepath.

Emitted when the user wants to open a URL with the application. Your application's Info.plist file must define the URL scheme within the CFBundleURLTypes key, and set NSPrincipalClass to AtomApplication.

As with the open-file event, be sure to register a listener for the open-url event early in your application startup to detect if the application is being opened to handle a URL. If you register the listener in response to a ready event, you'll miss URLs that trigger the launch of your application.

Emitted when the application is activated. Various actions can trigger this event, such as launching the application for the first time, attempting to re-launch the application when it's already running, or clicking on the application's dock or taskbar icon.

Emitted when the application becomes active. This differs from the activate event in that did-become-active is emitted every time the app becomes active, not only when Dock icon is clicked or application is re-launched. It is also emitted when a user switches to the app via the macOS App Switcher.

Emitted when the app is no longer active and doesn’t have focus. This can be triggered, for example, by clicking on another application or by using the macOS App Switcher to switch to another application.

Emitted during Handoff when an activity from a different device wants to be resumed. You should call event.preventDefault() if you want to handle this event.

A user activity can be continued only in an app that has the same developer Team ID as the activity's source app and that supports the activity's type. Supported activity types are specified in the app's Info.plist under the NSUserActivityTypes key.

Emitted during Handoff before an activity from a different device wants to be resumed. You should call event.preventDefault() if you want to handle this event.

Emitted during Handoff when an activity from a different device fails to be resumed.

Emitted during Handoff after an activity from this device was successfully resumed on another one.

Emitted when Handoff is about to be resumed on another device. If you need to update the state to be transferred, you should call event.preventDefault() immediately, construct a new userInfo dictionary and call app.updateCurrentActivity() in a timely manner. Otherwise, the operation will fail and continue-activity-error will be called.

Emitted when the user clicks the native macOS new tab button. The new tab button is only visible if the current BrowserWindow has a tabbingIdentifier

Emitted when a browserWindow gets blurred.

Emitted when a browserWindow gets focused.

Emitted when a new browserWindow is created.

Emitted when a new webContents is created.

Emitted when failed to verify the certificate for url, to trust the certificate you should prevent the default behavior with event.preventDefault() and call callback(true).

Emitted when a client certificate is requested.

The url corresponds to the navigation entry requesting the client certificate and callback can be called with an entry filtered from the list. Using event.preventDefault() prevents the application from using the first certificate from the store.

Emitted when webContents or Utility process wants to do basic auth.

The default behavior is to cancel all authentications. To override this you should prevent the default behavior with event.preventDefault() and call callback(username, password) with the credentials.

If callback is called without a username or password, the authentication request will be cancelled and the authentication error will be returned to the page.

Emitted whenever there is a GPU info update.

Emitted when the renderer process unexpectedly disappears. This is normally because it was crashed or killed.

Emitted when the child process unexpectedly disappears. This is normally because it was crashed or killed. It does not include renderer processes.

Emitted when Chrome's accessibility support changes. This event fires when assistive technologies, such as screen readers, are enabled or disabled. See https://www.chromium.org/developers/design-documents/accessibility for more details.

Emitted when Electron has created a new session.

This event will be emitted inside the primary instance of your application when a second instance has been executed and calls app.requestSingleInstanceLock().

argv is an Array of the second instance's command line arguments, and workingDirectory is its current working directory. Usually applications respond to this by making their primary window focused and non-minimized.

argv will not be exactly the same list of arguments as those passed to the second instance. The order might change and additional arguments might be appended. If you need to maintain the exact same arguments, it's advised to use additionalData instead.

If the second instance is started by a different user than the first, the argv array will not include the arguments.

This event is guaranteed to be emitted after the ready event of app gets emitted.

Extra command line arguments might be added by Chromium, such as --original-process-start-time.

The app object has the following methods:

Some methods are only available on specific operating systems and are labeled as such.

Try to close all windows. The before-quit event will be emitted first. If all windows are successfully closed, the will-quit event will be emitted and by default the application will terminate.

This method guarantees that all beforeunload and unload event handlers are correctly executed. It is possible that a window cancels the quitting by returning false in the beforeunload event handler.

Exits immediately with exitCode. exitCode defaults to 0.

All windows will be closed immediately without asking the user, and the before-quit and will-quit events will not be emitted.

Relaunches the app when the current instance exits.

By default, the new instance will use the same working directory and command line arguments as the current instance. When args is specified, the args will be passed as the command line arguments instead. When execPath is specified, the execPath will be executed for the relaunch instead of the current app.

Note that this method does not quit the app when executed. You have to call app.quit or app.exit after calling app.relaunch to make the app restart.

When app.relaunch is called multiple times, multiple instances will be started after the current instance exits.

An example of restarting the current instance immediately and adding a new command line argument to the new instance:

Returns boolean - true if Electron has finished initializing, false otherwise. See also app.whenReady().

Returns Promise<void> - fulfilled when Electron is initialized. May be used as a convenient alternative to checking app.isReady() and subscribing to the ready event if the app is not ready yet.

On macOS, makes the application the active app. On Windows, focuses on the application's first window. On Linux, either focuses on the first visible window (X11) or requests focus but may instead show a notification or flash the app icon (Wayland).

You should seek to use the steal option as sparingly as possible.

Hides all application windows without minimizing them.

Returns boolean - true if the application—including all of its windows—is hidden (e.g. with Command-H), false otherwise.

Shows application windows after they were hidden. Does not automatically focus them.

Sets or creates a directory your app's logs which can then be manipulated with app.getPath() or app.setPath(pathName, newPath).

Calling app.setAppLogsPath() without a path parameter will result in this directory being set to ~/Library/Logs/YourAppName on macOS, and inside the userData directory on Linux and Windows.

Returns string - The current application directory.

Returns string - A path to a special directory or file associated with name. On failure, an Error is thrown.

If app.getPath('logs') is called without called app.setAppLogsPath() being called first, a default log directory will be created equivalent to calling app.setAppLogsPath() without a path parameter.

Returns Promise<NativeImage> - fulfilled with the app's icon, which is a NativeImage.

Fetches a path's associated icon.

On Windows, there a 2 kinds of icons:

On Linux and macOS, icons depend on the application associated with file mime type.

Overrides the path to a special directory or file associated with name. If the path specifies a directory that does not exist, an Error is thrown. In that case, the directory should be created with fs.mkdirSync or similar.

You can only override paths of a name defined in app.getPath.

By default, web pages' cookies and caches will be stored under the sessionData directory. If you want to change this location, you have to override the sessionData path before the ready event of the app module is emitted.

Returns string - The version of the loaded application. If no version is found in the application's package.json file, the version of the current bundle or executable is returned.

Returns string - The current application's name, which is the name in the application's package.json file.

Usually the name field of package.json is a short lowercase name, according to the npm modules spec. You should usually also specify a productName field, which is your application's full capitalized name, and which will be preferred over name by Electron.

Overrides the current application's name.

This function overrides the name used internally by Electron; it does not affect the name that the OS uses.

Returns string - The current application locale, fetched using Chromium's l10n_util library. Possible return values are documented here.

To set the locale, you'll want to use a command line switch at app startup, which may be found here.

When distributing your packaged app, you have to also ship the locales folder.

This API must be called after the ready event is emitted.

To see example return values of this API compared to other locale and language APIs, see app.getPreferredSystemLanguages().

Returns string - User operating system's locale two-letter ISO 3166 country code. The value is taken from native OS APIs.

When unable to detect locale country code, it returns empty string.

Returns string - The current system locale. On Windows and Linux, it is fetched using Chromium's i18n library. On macOS, [NSLocale currentLocale] is used instead. To get the user's current system language, which is not always the same as the locale, it is better to use app.getPreferredSystemLanguages().

Different operating systems also use the regional data differently:

Therefore, this API can be used for purposes such as choosing a format for rendering dates and times in a calendar app, especially when the developer wants the format to be consistent with the OS.

This API must be called after the ready event is emitted.

To see example return values of this API compared to other locale and language APIs, see app.getPreferredSystemLanguages().

Returns string[] - The user's preferred system languages from most preferred to least preferred, including the country codes if applicable. A user can modify and add to this list on Windows or macOS through the Language and Region settings.

The API uses GlobalizationPreferences (with a fallback to GetSystemPreferredUILanguages) on Windows, \[NSLocale preferredLanguages\] on macOS, and g_get_language_names on Linux.

This API can be used for purposes such as deciding what language to present the application in.

Here are some examples of return values of the various language and locale APIs with different configurations:

On Windows, given application locale is German, the regional format is Finnish (Finland), and the preferred system languages from most to least preferred are French (Canada), English (US), Simplified Chinese (China), Finnish, and Spanish (Latin America):

On macOS, given the application locale is German, the region is Finland, and the preferred system languages from most to least preferred are French (Canada), English (US), Simplified Chinese, and Spanish (Latin America):

Both the available languages and regions and the possible return values differ between the two operating systems.

As can be seen with the example above, on Windows, it is possible that a preferred system language has no country code, and that one of the preferred system languages corresponds with the language used for the regional format. On macOS, the region serves more as a default country code: the user doesn't need to have Finnish as a preferred language to use Finland as the region,and the country code FI is used as the country code for preferred system languages that do not have associated countries in the language name.

Adds path to the recent documents list.

This list is managed by the OS. On Windows, you can visit the list from the task bar, and on macOS, you can visit it from dock menu.

Clears the recent documents list.

Returns string[] - An array containing documents in the most recent documents list.

Returns boolean - Whether the call succeeded.

Sets the current executable as the default handler for a protocol (aka URI scheme). It allows you to integrate your app deeper into the operating system. Once registered, all links with your-protocol:// will be opened with the current executable. The whole link, including protocol, will be passed to your application as a parameter.

On macOS, you can only register protocols that have been added to your app's info.plist, which cannot be modified at runtime. However, you can change the file during build time via Electron Forge, Electron Packager, or by editing info.plist with a text editor. Please refer to Apple's documentation for details.

In a Windows Store environment (when packaged as an appx) this API will return true for all calls but the registry key it sets won't be accessible by other applications. In order to register your Windows Store application as a default protocol handler you must declare the protocol in your manifest.

The API uses the Windows Registry and LSSetDefaultHandlerForURLScheme internally.

Returns boolean - Whether the call succeeded.

This method checks if the current executable as the default handler for a protocol (aka URI scheme). If so, it will remove the app as the default handler.

Returns boolean - Whether the current executable is the default handler for a protocol (aka URI scheme).

On macOS, you can use this method to check if the app has been registered as the default protocol handler for a protocol. You can also verify this by checking ~/Library/Preferences/com.apple.LaunchServices.plist on the macOS machine. Please refer to Apple's documentation for details.

The API uses the Windows Registry and LSCopyDefaultHandlerForURLScheme internally.

Returns string - Name of the application handling the protocol, or an empty string if there is no handler. For instance, if Electron is the default handler of the URL, this could be Electron on Windows and Mac. However, don't rely on the precise format which is not guaranteed to remain unchanged. Expect a different format on Linux, possibly with a .desktop suffix.

This method returns the application name of the default handler for the protocol (aka URI scheme) of a URL.

Returns Promise<Object> - Resolve with an object containing the following:

This method returns a promise that contains the application name, icon and path of the default handler for the protocol (aka URI scheme) of a URL.

Adds tasks to the Tasks category of the Jump List on Windows.

tasks is an array of Task objects.

Returns boolean - Whether the call succeeded.

If you'd like to customize the Jump List even more use app.setJumpList(categories) instead.

Sets or removes a custom Jump List for the application, and returns one of the following strings:

If categories is null the previously set custom Jump List (if any) will be replaced by the standard Jump List for the app (managed by Windows).

If a JumpListCategory object has neither the type nor the name property set then its type is assumed to be tasks. If the name property is set but the type property is omitted then the type is assumed to be custom.

Users can remove items from custom categories, and Windows will not allow a removed item to be added back into a custom category until after the next successful call to app.setJumpList(categories). Any attempt to re-add a removed item to a custom category earlier than that will result in the entire custom category being omitted from the Jump List. The list of removed items can be obtained using app.getJumpListSettings().

The maximum length of a Jump List item's description property is 260 characters. Beyond this limit, the item will not be added to the Jump List, nor will it be displayed.

Here's a very simple example of creating a custom Jump List:

The return value of this method indicates whether or not this instance of your application successfully obtained the lock. If it failed to obtain the lock, you can assume that another instance of your application is already running with the lock and exit immediately.

I.e. This method returns true if your process is the primary instance of your application and your app should continue loading. It returns false if your process should immediately quit as it has sent its parameters to another instance that has already acquired the lock.

On macOS, the system enforces single instance automatically when users try to open a second instance of your app in Finder, and the open-file and open-url events will be emitted for that. However when users start your app in command line, the system's single instance mechanism will be bypassed, and you have to use this method to ensure single instance.

An example of activating the window of primary instance when a second instance starts:

This method returns whether or not this instance of your app is currently holding the single instance lock. You can request the lock with app.requestSingleInstanceLock() and release with app.releaseSingleInstanceLock()

Releases all locks that were created by requestSingleInstanceLock. This will allow multiple instances of the application to once again run side by side.

Creates an NSUserActivity and sets it as the current activity. The activity is eligible for Handoff to another device afterward.

Returns string - The type of the currently running activity.

Invalidates the current Handoff user activity.

Marks the current Handoff user activity as inactive without invalidating it.

Updates the current activity if its type matches type, merging the entries from userInfo into its current userInfo dictionary.

Changes the Application User Model ID to id.

Sets the activation policy for a given app.

Activation policy types:

Imports the certificate in pkcs12 format into the platform certificate store. callback is called with the result of import operation, a value of 0 indicates success while any other value indicates failure according to Chromium net_error_list.

Configures host resolution (DNS and DNS-over-HTTPS). By default, the following resolvers will be used, in order:

This can be configured to either restrict usage of non-encrypted DNS (secureDnsMode: "secure"), or disable DNS-over-HTTPS (secureDnsMode: "off"). It is also possible to enable or disable the built-in resolver.

To disable insecure DNS, you can specify a secureDnsMode of "secure". If you do so, you should make sure to provide a list of DNS-over-HTTPS servers to use, in case the user's DNS configuration does not include a provider that supports DoH.

This API must be called after the ready event is emitted.

Disables hardware acceleration for current app.

This method can only be called before app is ready.

Returns boolean - whether hardware acceleration is currently enabled.

This information is only usable after the gpu-info-update event is emitted.

By default, Chromium disables 3D APIs (e.g. WebGL) until restart on a per domain basis if the GPU processes crashes too frequently. This function disables that behavior.

This method can only be called before app is ready.

Returns ProcessMetric[]: Array of ProcessMetric objects that correspond to memory and CPU usage statistics of all the processes associated with the app.

Returns GPUFeatureStatus - The Graphics Feature Status from chrome://gpu/.

This information is only usable after the gpu-info-update event is emitted.

Returns Promise<unknown>

For infoType equal to complete: Promise is fulfilled with Object containing all the GPU Information as in chromium's GPUInfo object. This includes the version and driver information that's shown on chrome://gpu page.

For infoType equal to basic: Promise is fulfilled with Object containing fewer attributes than when requested with complete. Here's an example of basic response:

Using basic should be preferred if only basic information like vendorId or deviceId is needed.

Returns boolean - Whether the call succeeded.

Sets the counter badge for current app. Setting the count to 0 will hide the badge.

On macOS, it shows on the dock icon. On Linux, it only works for Unity launcher.

Unity launcher requires a .desktop file to work. For more information, please read the Unity integration documentation.

On macOS, you need to ensure that your application has the permission to display notifications for this method to work.

Returns Integer - The current value displayed in the counter badge.

Returns boolean - Whether the current desktop environment is Unity launcher.

If you provided path and args options to app.setLoginItemSettings, then you need to pass the same arguments here for openAtLogin to be set correctly.

Set the app's login item settings.

To work with Electron's autoUpdater on Windows, which uses Squirrel, you'll want to set the launch path to your executable's name but a directory up, which is a stub application automatically generated by Squirrel which will automatically launch the latest version.

For more information about setting different services as login items on macOS 13 and up, see SMAppService.

Returns boolean - true if Chrome's accessibility support is enabled, false otherwise. This API will return true if the use of assistive technologies, such as screen readers, has been detected. See https://www.chromium.org/developers/design-documents/accessibility for more details.

Manually enables Chrome's accessibility support, allowing to expose accessibility switch to users in application settings. See Chromium's accessibility docs for more details. Disabled by default.

This API must be called after the ready event is emitted.

Rendering accessibility tree can significantly affect the performance of your app. It should not be enabled by default. Calling this method will enable the following accessibility support features: nativeAPIs, webContents, inlineTextBoxes, and extendedProperties.

Returns string[] - Array of strings naming currently enabled accessibility support components. Possible values:

To disable all supported features, pass an empty array [].

Show the app's about panel options. These options can be overridden with app.setAboutPanelOptions(options). This function runs asynchronously.

Set the about panel options. This will override the values defined in the app's .plist file on macOS. See the Apple docs for more details. On Linux, values must be set in order to be shown; there are no defaults.

If you do not set credits but still wish to surface them in your app, AppKit will look for a file named "Credits.html", "Credits.rtf", and "Credits.rtfd", in that order, in the bundle returned by the NSBundle class method main. The first file found is used, and if none is found, the info area is left blank. See Apple documentation for more information.

Returns boolean - whether or not the current OS version allows for native emoji pickers.

Show the platform's native emoji picker.

Returns Function - This function must be called once you have finished accessing the security scoped file. If you do not remember to stop accessing the bookmark, kernel resources will be leaked and your app will lose its ability to reach outside the sandbox completely, until your app is restarted.

Start accessing a security scoped resource. With this method Electron applications that are packaged for the Mac App Store may reach outside their sandbox to access files chosen by the user. See Apple's documentation for a description of how this system works.

Enables full sandbox mode on the app. This means that all renderers will be launched sandboxed, regardless of the value of the sandbox flag in WebPreferences.

This method can only be called before app is ready.

Returns boolean - Whether the application is currently running from the systems Application folder. Use in combination with app.moveToApplicationsFolder()

Returns boolean - Whether the move was successful. Please note that if the move is successful, your application will quit and relaunch.

No confirmation dialog will be presented by default. If you wish to allow the user to confirm the operation, you may do so using the dialog API.

NOTE: This method throws errors if anything other than the user causes the move to fail. For instance if the user cancels the authorization dialog, this method returns false. If we fail to perform the copy, then this method will throw an error. The message in the error should be informative and tell you exactly what went wrong.

By default, if an app of the same name as the one being moved exists in the Applications directory and is not running, the existing app will be trashed and the active app moved into its place. If it is running, the preexisting running app will assume focus and the previously active app will quit itself. This behavior can be changed by providing the optional conflict handler, where the boolean returned by the handler determines whether or not the move conflict is resolved with default behavior. i.e. returning false will ensure no further action is taken, returning true will result in the default behavior and the method continuing.

Would mean that if an app already exists in the user directory, if the user chooses to 'Continue Move' then the function would continue with its default behavior and the existing app will be trashed and the active app moved into its place.

Returns boolean - whether Secure Keyboard Entry is enabled.

By default this API will return false.

Set the Secure Keyboard Entry is enabled in your application.

By using this API, important information such as password and other sensitive information can be prevented from being intercepted by other processes.

See Apple's documentation for more details.

Enable Secure Keyboard Entry only when it is needed and disable it when it is no longer needed.

Returns Promise<void> - Resolves when the proxy setting process is complete.

Sets the proxy settings for networks requests made without an associated Session. Currently this will affect requests made with Net in the utility process and internal requests made by the runtime (ex: geolocation queries).

This method can only be called after app is ready.

Returns Promise<string> - Resolves with the proxy information for url that will be used when attempting to make requests using Net in the utility process.

handler Function<Promise<string>>

Returns Promise<string> - Resolves with the password

The handler is called when a password is needed to unlock a client certificate for hostname.

A boolean property that's true if Chrome's accessibility support is enabled, false otherwise. This property will be true if the use of assistive technologies, such as screen readers, has been detected. Setting this property to true manually enables Chrome's accessibility support, allowing developers to expose accessibility switch to users in application settings.

See Chromium's accessibility docs for more details. Disabled by default.

This API must be called after the ready event is emitted.

Rendering accessibility tree can significantly affect the performance of your app. It should not be enabled by default.

A Menu | null property that returns Menu if one has been set and null otherwise. Users can pass a Menu to set this property.

An Integer property that returns the badge count for current app. Setting the count to 0 will hide the badge.

On macOS, setting this with any nonzero integer shows on the dock icon. On Linux, this property only works for Unity launcher.

Unity launcher requires a .desktop file to work. For more information, please read the Unity integration documentation.

On macOS, you need to ensure that your application has the permission to display notifications for this property to take effect.

A CommandLine object that allows you to read and manipulate the command line arguments that Chromium uses.

A Dock | undefined property (Dock on macOS, undefined on all other platforms) that allows you to perform actions on your app icon in the user's dock.

A boolean property that returns true if the app is packaged, false otherwise. For many apps, this property can be used to distinguish development and production environments.

A string property that indicates the current application's name, which is the name in the application's package.json file.

Usually the name field of package.json is a short lowercase name, according to the npm modules spec. You should usually also specify a productName field, which is your application's full capitalized name, and which will be preferred over name by Electron.

A string which is the user agent string Electron will use as a global fallback.

This is the user agent that will be used when no user agent is set at the webContents or session level. It is useful for ensuring that your entire app has the same user agent. Set to a custom value as early as possible in your app's initialization to ensure that your overridden value is used.

A boolean which when true indicates that the app is currently running under an ARM64 translator (like the macOS Rosetta Translator Environment or Windows WOW).

You can use this property to prompt users to download the arm64 version of your application when they are mistakenly running the x64 version under Rosetta or WOW.

**Examples:**

Example 1 (javascript):
```javascript
const { app } = require('electron')app.on('window-all-closed', () => {  app.quit()})
```

Example 2 (javascript):
```javascript
const { app } = require('electron')app.on('certificate-error', (event, webContents, url, error, certificate, callback) => {  if (url === 'https://github.com') {    // Verification logic.    event.preventDefault()    callback(true)  } else {    callback(false)  }})
```

Example 3 (javascript):
```javascript
const { app } = require('electron')app.on('select-client-certificate', (event, webContents, url, list, callback) => {  event.preventDefault()  callback(list[0])})
```

Example 4 (javascript):
```javascript
const { app } = require('electron')app.on('login', (event, webContents, details, authInfo, callback) => {  event.preventDefault()  callback('username', 'secret')})
```

---

## SegmentedControlSegment Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/segmented-control-segment

**Contents:**
- SegmentedControlSegment Object

---

## WebContentsView

**URL:** https://www.electronjs.org/docs/latest/api/web-contents-view

**Contents:**
- WebContentsView
- Class: WebContentsView extends View​
  - new WebContentsView([options])​
  - Instance Properties​
    - view.webContents Readonly​

A View that displays a WebContents.

This module cannot be used until the ready event of the app module is emitted.

A View that displays a WebContents.

WebContentsView inherits from View.

WebContentsView is an EventEmitter.

Electron's built-in classes cannot be subclassed in user code. For more information, see the FAQ.

Creates a WebContentsView.

Objects created with new WebContentsView have the following properties, in addition to those inherited from View:

A WebContents property containing a reference to the displayed WebContents. Use this to interact with the WebContents, for instance to load a URL.

**Examples:**

Example 1 (javascript):
```javascript
const { BaseWindow, WebContentsView } = require('electron')const win = new BaseWindow({ width: 800, height: 400 })const view1 = new WebContentsView()win.contentView.addChildView(view1)view1.webContents.loadURL('https://electronjs.org')view1.setBounds({ x: 0, y: 0, width: 400, height: 400 })const view2 = new WebContentsView()win.contentView.addChildView(view2)view2.webContents.loadURL('https://github.com/electron/electron')view2.setBounds({ x: 400, y: 0, width: 400, height: 400 })
```

Example 2 (javascript):
```javascript
const { WebContentsView } = require('electron')const view = new WebContentsView()view.webContents.loadURL('https://electronjs.org/')
```

---

## CertificatePrincipal Object

**URL:** https://www.electronjs.org/docs/latest/api/structures/certificate-principal

**Contents:**
- CertificatePrincipal Object

---
