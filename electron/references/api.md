# Electron - API 文档

**页数:** 153

---

## 概述

Electron API 文档包含 153 页的详细 API 参考，涵盖了所有可用的模块、类、方法和事件。本文档提供了 Electron 框架的完整 API 参考。

---

## 主要 API 类别

### 1. 应用程序生命周期 (App Lifecycle)

**app 模块**
- 控制应用程序的生命周期
- 管理应用程序事件
- 处理命令行参数
- 访问应用程序信息

**关键方法:**
- `app.on('ready')`: 应用程序准备就绪
- `app.quit()`: 退出应用程序
- `app.getPath(name)`: 获取系统路径
- `app.setAppUserModelId(id)`: 设置用户模型 ID (Windows)
- `app.focus()`: 聚焦应用程序

**技术说明:**
- 主进程模块
- 必须在 ready 事件后才能创建窗口
- 处理应用程序级别的权限和请求

---

### 2. 浏览器窗口 (BrowserWindow)

**BrowserWindow 类**
- 创建和控制浏览器窗口
- 管理窗口属性和行为
- 处理窗口事件

**关键方法:**
- `new BrowserWindow(options)`: 创建新窗口
- `win.loadURL(url)`: 加载 URL
- `win.loadFile(path)`: 加载本地文件
- `win.webContents`: 访问 WebContents
- `win.close()`: 关闭窗口

**窗口选项:**
- `width`, `height`: 窗口尺寸
- `x`, `y`: 窗口位置
- `frame`: 是否显示边框
- `transparent`: 透明窗口
- `resizable`: 可调整大小
- `webPreferences`: Web 首选项

**技术说明:**
- 主进程类
- 每个窗口在独立的渲染进程中运行
- 支持原生窗口控制

---

### 3. 进程间通信 (IPC)

**ipcMain 模块**
- 从主进程向渲染进程发送消息
- 监听渲染进程发送的消息

**ipcRenderer 模块**
- 从渲染进程向主进程发送消息
- 监听主进程发送的消息

**关键方法:**
```javascript
// 主进程
ipcMain.on('channel', (event, ...args) => {
  // 处理消息
  event.reply('reply', 'response')
})

// 渲染进程
ipcRenderer.send('channel', 'data')
ipcRenderer.on('reply', (event, ...args) => {
  // 处理回复
})
```

**技术说明:**
- 使用结构化克隆算法序列化消息
- 支持同步和异步通信
- 推荐使用异步通信避免阻塞

---

### 4. 对话框 (Dialog)

**dialog 模块**
- 显示原生系统对话框
- 文件选择对话框
- 消息对话框

**关键方法:**
- `dialog.showOpenDialog()`: 打开文件对话框
- `dialog.showSaveDialog()`: 保存文件对话框
- `dialog.showMessageBox()`: 消息框

**技术说明:**
- 主进程模块
- 在渲染进程中使用需通过 IPC
- 返回 Promise 或使用回调

---

### 5. 菜单 (Menu)

**Menu 类**
- 创建原生应用菜单
- 创建上下文菜单
- 创建托盘菜单

**MenuItem 类**
- 定义菜单项
- 配置菜单项属性

**关键方法:**
- `Menu.buildFromTemplate(template)`: 从模板构建菜单
- `menu.popup()`: 显示上下文菜单
- `win.setMenu(menu)`: 设置窗口菜单

**技术说明:**
- 主进程类
- 支持原生菜单角色（复制、粘贴等）
- 自动适配不同操作系统

---

### 6. 系统托盘 (Tray)

**Tray 类**
- 创建系统托盘图标
- 托盘图标菜单
- 托盘图标提示

**关键方法:**
- `new Tray(image)`: 创建托盘图标
- `tray.setTooltip(text)`: 设置提示文本
- `tray.setContextMenu(menu)`: 设置上下文菜单
- `tray.displayBalloon()`: 显示气球通知 (Windows)

**技术说明:**
- 主进程类
- 不同操作系统有不同的托盘行为
- 需要保持 Tray 对象引用避免被垃圾回收

---

### 7. 通知 (Notification)

**Notification 类**
- 显示系统通知
- 跨平台通知 API

**关键方法:**
```javascript
const notification = new Notification({
  title: '标题',
  body: '通知内容'
})
notification.show()
```

**技术说明:**
- 支持主进程和渲染进程
- 不同操作系统有不同的通知样式
- 需要用户权限才能显示

---

### 8. WebContents

**WebContents 类**
- 渲染和控制 Web 页面
- 管理页面生命周期
- 处理页面事件

**关键方法:**
- `contents.loadURL(url)`: 加载 URL
- `contents.executeJavaScript(code)`: 执行 JavaScript
- `contents.on('dom-ready')`: DOM 准备就绪事件
- `contents.openDevTools()`: 打开开发者工具

**技术说明:**
- BrowserWindow 和 webview 都有 webContents
- 提供对渲染页面的完全控制
- 支持导航、缩放、打印等功能

---

### 9. Session 和 Cookies

**session 模块**
- 管理浏览器会话
- 处理 cookies
- 管理网络请求

**关键方法:**
- `session.defaultSession`: 默认会话
- `ses.cookies`: 访问 cookies
- `ses.setUserAgent()`: 设置用户代理
- `ses.clearStorageData()`: 清除存储数据

**技术说明:**
- 主进程模块
- 每个窗口可以有独立的会话
- 支持分区会话

---

### 10. 协议 (Protocol)

**protocol 模块**
- 注册自定义协议
- 拦截协议请求

**关键方法:**
- `protocol.registerSchemesAsPrivileged()`: 注册特权方案
- `protocol.handle()`: 处理协议请求

**技术说明:**
- 必须在 app ready 之前注册
- 支持自定义协议如 `app://`, `custom://`
- 可以拦截现有协议

---

### 11. 屏幕 (Screen)

**screen 模块**
- 获取屏幕信息
- 管理显示器

**关键方法:**
- `screen.getCursorScreenPoint()`: 获取光标位置
- `screen.getPrimaryDisplay()`: 获取主显示器
- `screen.getAllDisplays()`: 获取所有显示器

**技术说明:**
- 主进程和渲染进程都可用
- 支持多显示器环境
- 返回 Display 对象包含屏幕尺寸

---

### 12. 剪贴板 (Clipboard)

**clipboard 模块**
- 系统剪贴板操作
- 复制、粘贴、清除

**关键方法:**
- `clipboard.readText()`: 读取文本
- `clipboard.writeText(text)`: 写入文本
- `clipboard.clear()`: 清除剪贴板

**技术说明:**
- 主进程和渲染进程都可用
- 支持多种格式（文本、HTML、图像）
- 需要用户授权

---

### 13. 原生图像 (NativeImage)

**NativeImage 类**
- 处理图像数据
- 跨平台图像格式

**关键方法:**
- `nativeImage.createFromPath(path)`: 从路径创建
- `image.toPNG()`: 转换为 PNG
- `image.toDataURL()`: 转换为 Data URL
- `image.resize()`: 调整大小

**技术说明:**
- 支持多种图像格式
- 自动适配 DPI 缩放
- 用于托盘图标、窗口图标等

---

### 14. 全局快捷键 (globalShortcut)

**globalShortcut 模块**
- 注册全局快捷键
- 监听全局快捷键

**关键方法:**
- `globalShortcut.register(accelerator, callback)`: 注册快捷键
- `globalShortcut.isRegistered(accelerator)`: 检查是否注册
- `globalShortcut.unregisterAll()`: 取消所有快捷键

**技术说明:**
- 主进程模块
- 快捷键全局有效，即使应用失去焦点
- 需要在 app ready 后注册

---

### 15. Shell 操作

**shell 模块**
- 桌面集成
- 文件操作
- 默认应用程序

**关键方法:**
- `shell.openExternal(url)`: 在外部浏览器打开 URL
- `shell.openPath(path)`: 使用默认应用打开文件
- `shell.showItemInFolder(path)`: 在文件夹中显示文件
- `shell.beep()`: 播放系统蜂鸣声

**技术说明:**
- 主进程和渲染进程都可用
- 跨平台兼容
- 使用系统默认应用程序

---

### 16. 电源监控 (powerMonitor)

**powerMonitor 模块**
- 监控电源状态
- 电池电量变化
- 系统挂起/恢复

**关键事件:**
- `suspend`: 系统即将挂起
- `resume`: 系统已恢复
- `on-ac`: 接入电源
- `on-battery`: 使用电池

**技术说明:**
- 主进程模块
- 跨平台电源状态监控
- 用于优化应用性能

---

### 17. 网络服务 (net)

**net 模块**
- HTTP/HTTPS 请求
- 网络拦截
- 自定义协议

**关键方法:**
- `net.request(options)`: 创建 HTTP 请求
- `net.fetch(url)`: 类似浏览器的 fetch API

**技术说明:**
- 主进程模块
- 类似 Node.js 的 http 模块
- 支持 Chromium 的网络栈

---

### 18. Crash Reporter

**crashReporter 模块**
- 崩溃报告
- 错误跟踪

**关键方法:**
- `crashReporter.start(options)`: 启动崩溃报告
- `crashReporter.getLastCrashReport()`: 获取最后一次崩溃报告

**技术说明:**
- 主进程和渲染进程都可用
- 上传崩溃报告到服务器
- 帮助调试应用崩溃

---

### 19. 自动更新 (autoUpdater)

**autoUpdater 模块**
- 应用程序自动更新
- 检查更新
- 下载和安装更新

**关键方法:**
- `autoUpdater.checkForUpdates()`: 检查更新
- `autoUpdater.downloadUpdate()`: 下载更新
- `autoUpdater.quitAndInstall()`: 退出并安装

**事件:**
- `update-available`: 有可用更新
- `update-downloaded`: 更新已下载
- `error`: 更新错误

**技术说明:**
- 主进程模块
- 需要配置更新服务器
- macOS 需要代码签名

---

### 20. WebFrame

**webFrame 模块**
- 自定义页面渲染
- 缩放级别
- 代码执行

**关键方法:**
- `webFrame.setZoomLevel(level)`: 设置缩放级别
- `webFrame.insertCSS(css)`: 插入 CSS
- `webFrame.executeJavaScript(code)`: 执行 JavaScript

**技术说明:**
- 渲染进程模块
- 影响当前窗口的渲染
- 用于自定义页面行为

---

## 高级 API

### Touch Bar (macOS)
**touchBar 模块**
- 创建 Touch Bar 布局
- Touch Bar 控件
- 原生 macOS 功能

### Dock (macOS)
**dock 模块**
- 应用 Dock 图标
- Dock 菜单
- 跳转列表

### Windows Taskbar
**任务栏功能**
- 任务栏进度
- 缩略图工具栏
- Flash 任务栏图标

### 安全存储 (safeStorage)
**safeStorage 模块**
- 加密字符串存储
- 系统密钥链集成
- 跨平台加密

### 原生主题 (nativeTheme)
**nativeTheme 模块**
- 主题检测
- 深色/浅色模式
- 系统主题变化

---

## 结构化对象

### Cookies
- Cookie 信息
- Cookie 管理

### Processor Features
- CPU 信息
- 特性检测

### Display
- 显示器信息
- 多显示器支持

### Point, Rectangle, Size
- 几何对象
- 坐标系统

### Keyboard Event
- 键盘事件
- 快捷键处理

### Input Event
- 输入事件
- 鼠标/触摸事件

---

## 使用最佳实践

### 1. 进程模型
- **主进程**: 使用 app、BrowserWindow、Menu 等
- **渲染进程**: 使用 ipcRenderer、webFrame 等
- **Preload 脚本**: 使用 contextBridge 暴露安全 API

### 2. 安全性
- 始终启用 `contextIsolation`
- 禁用 `nodeIntegration`（除非必要）
- 启用 `sandbox`
- 使用 `contextBridge` 暴露 API

### 3. 性能优化
- 懒加载 BrowserWindow
- 使用 Web Workers 处理繁重任务
- 优化 IPC 通信
- 管理内存使用

### 4. 错误处理
- 监听 `uncaughtException` 事件
- 使用崩溃报告器
- 实现错误日志记录
- 提供用户友好的错误消息

---

## API 版本历史

每个 API 文档都包含 API History 块，记录:
- API 引入的版本
- 弃用的版本
- 破坏性更改
- 相关 Pull Request

**查看完整 API 文档:**
- 在线文档: https://www.electronjs.org/docs/latest/api/
- API 类型定义: electron.d.ts
- 示例代码: Electron Fiddle

---

## 注意事项

### 兼容性
- API 在不同操作系统上可能有不同行为
- 某些 API 仅在特定平台上可用
- 查看 API 文档的平台标记

### 弃用警告
- 定期检查弃用的 API
- 及时更新到新 API
- 查看破坏性更改文档

### 性能考虑
- 避免在主线程执行繁重任务
- 使用异步 API
- 优化渲染进程性能
- 监控内存和 CPU 使用

---

## 相关资源

- **快速入门**: ./getting_started.md
- **教程**: ./tutorial.md
- **开发指南**: ./development.md
- **进程管理**: ./processes.md
- **常见问题**: ./other.md

---

**最后更新:** 基于 Electron 最新稳定版
**文档版本:** 对应 Electron 官方文档
