# Electron - 教程

**页数:** 73

---

## 概述

Electron 教程包含 73 页的详细教程，涵盖了从基础到高级的所有主题。本文档提供了构建 Electron 应用程序的完整教程和最佳实践。

---

## 核心教程

### 1. 快速入门

**创建您的第一个 Electron 应用**

```javascript
// main.js
const { app, BrowserWindow } = require('electron')

function createWindow () {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true
    }
  })

  win.loadFile('index.html')
}

app.whenReady().then(createWindow)
```

**项目结构:**
```
my-electron-app/
├── package.json
├── main.js
└── index.html
```

**技术要点:**
- 主进程入口文件
- 创建浏览器窗口
- 加载 HTML 内容
- 应用生命周期管理

---

### 2. 进程模型

**主进程 vs 渲染进程**

**主进程:**
- 每个 Electron 应用只有一个主进程
- 运行 package.json 中指定的入口脚本
- 管理应用的生命周期和所有 BrowserWindow
- 可以使用所有 Node.js API

**渲染进程:**
- 每个 BrowserWindow 都有一个独立的渲染进程
- 负责渲染 Web 页面
- 默认情况下无法访问 Node.js API（出于安全考虑）

**进程间通信 (IPC):**
```javascript
// 主进程
const { ipcMain } = require('electron')
ipcMain.on('asynchronous-message', (event, arg) => {
  console.log(arg) // prints "ping"
  event.reply('asynchronous-reply', 'pong')
})

// 渲染进程
const { ipcRenderer } = require('electron')
ipcRenderer.send('asynchronous-message', 'ping')
ipcRenderer.on('asynchronous-reply', (event, arg) => {
  console.log(arg) // prints "pong"
})
```

---

### 3. 应用程序打包

**使用 Electron Forge 打包**

```bash
# 安装 Electron Forge
npm install --save-dev @electron-forge/cli

# 初始化 Forge 配置
npx electron-forge init

# 打包应用
npm run make
```

**打包步骤:**
1. 安装打包工具
2. 配置应用元数据
3. 执行打包命令
4. 获取安装程序

**技术说明:**
- Electron Forge 是官方推荐的打包工具
- 支持 Windows、macOS、Linux
- 可以生成安装程序和可执行文件

---

### 4. 应用程序分发

**代码签名**

**Windows 代码签名:**
```bash
signtool sign /f certificate.pfx /p password /t timestamp_url dist\Setup.exe
```

**macOS 代码签名:**
```bash
codesign --deep --force --verify --verbose --sign "Developer ID Application: Your Name" YourApp.app
```

**公证 (macOS):**
```bash
xcrun notarytool submit YourApp.dmg --apple-id "your@email.com" --password "app-specific-password" --team-id "team-id"
```

**分发渠道:**
- 直接下载（网站分发）
- 应用商店（Mac App Store、Microsoft Store）
- 自动更新系统

---

### 5. 安全性

**安全检查清单**

**必须执行:**
- ✅ 启用 `contextIsolation`
- ✅ 禁用 `nodeIntegration`
- ✅ 启用 `sandbox`
- ✅ 使用 `contextBridge` 暴露 API

**示例配置:**
```javascript
const mainWindow = new BrowserWindow({
  webPreferences: {
    preload: path.join(app.getAppPath(), 'preload.js'),
    contextIsolation: true,
    nodeIntegration: false,
    sandbox: true
  }
})
```

**Preload 脚本:**
```javascript
// preload.js
const { contextBridge, ipcRenderer } = require('electron')

contextBridge.exposeInMainWorld('myAPI', {
  send: (channel, data) => ipcRenderer.send(channel, data),
  on: (channel, func) => ipcRenderer.on(channel, (event, ...args) => func(...args))
})
```

**渲染进程使用:**
```javascript
// renderer.js
window.myAPI.send('message', 'data')
window.myAPI.on('reply', (data) => {
  console.log(data)
})
```

**技术说明:**
- 上下文隔离确保预加载脚本和渲染进程隔离
- 沙箱限制渲染进程的权限
- contextBridge 提供安全的 API 暴露机制

---

### 6. 自动更新

**使用 electron-updater**

**安装:**
```bash
npm install electron-updater
```

**配置:**
```javascript
const { app, autoUpdater } = require('electron')

// 配置更新服务器
autoUpdater.setFeedURL({
  provider: 'generic',
  url: 'https://your-server.com/updates'
})

// 检查更新
autoUpdater.checkForUpdates()

// 监听更新事件
autoUpdater.on('update-available', () => {
  console.log('Update available')
})

autoUpdater.on('update-downloaded', () => {
  console.log('Update downloaded')
  autoUpdater.quitAndInstall()
})
```

**更新服务器:**
- 需要托管更新元数据
- 发布版本到服务器
- 配置更新频率

**技术说明:**
- macOS 需要代码签名
- Windows 需要配置安装程序
- 支持增量更新

---

### 7. 通知

**系统通知**

**主进程通知:**
```javascript
const { Notification } = require('electron')

const notification = new Notification({
  title: '标题',
  body: '通知内容',
  icon: '/path/to/icon.png'
})

notification.show()
```

**渲染进程通知:**
```javascript
const notification = new Notification('标题', {
  body: '通知内容'
})

notification.onclick = () => {
  console.log('Notification clicked')
}
```

**平台差异:**
- **Windows**: 需要 AppUserModelID
- **macOS**: 遵循 Apple 通知指南
- **Linux**: 使用 libnotify

**技术说明:**
- 不同平台有不同的通知样式
- 某些平台需要用户权限
- 通知内容长度有限制

---

### 8. 最近文档

**管理最近使用的文档**

**添加最近文档:**
```javascript
const { app } = require('electron')

// 添加文件到最近文档列表
app.addRecentDocument('/path/to/file.txt')

// 清除最近文档
app.clearRecentDocuments()

// 获取最近文档列表
const recentDocs = app.getRecentDocuments()
```

**Windows 特定:**
- 需要注册为文件类型处理器
- 显示在跳转列表中
- 点击时启动应用并传递文件路径

**macOS 特定:**
- 显示在 Dock 菜单中
- 自动管理最近文档

---

### 9. 进程沙箱

**沙箱化渲染进程**

**启用沙箱:**
```javascript
const mainWindow = new BrowserWindow({
  webPreferences: {
    sandbox: true
  }
})
```

**沙箱限制:**
- 无法访问 Node.js API
- 无法加载原生模块
- 限制文件系统访问
- 限制网络访问

**预加载脚本:**
```javascript
// 沙箱环境中可用的 Node.js API subset
const { contextBridge, ipcRenderer } = require('electron')

contextBridge.exposeInMainWorld('app', {
  getVersion: () => process.versions.electron
})
```

**技术说明:**
- 沙箱提高安全性
- 防止恶意代码执行
- 限制系统资源访问

---

### 10. 离屏渲染

**离屏渲染技术**

**GPU 加速渲染:**
```javascript
const win = new BrowserWindow({
  webPreferences: {
    offscreen: true,
    webSecurity: false
  }
})

win.webContents.on('paint', (event, dirty, image) => {
  // 使用图像数据
})

win.webContents.setFrameRate(60)
```

**软件渲染:**
```javascript
const { app } = require('electron')
app.disableHardwareAcceleration()

const win = new BrowserWindow({
  webPreferences: {
    offscreen: true
  }
})
```

**使用场景:**
- 后台渲染
- 3D 场景纹理
- 视频处理
- 自动化测试

---

### 11. 多线程

**Web Workers**

**启用 Worker 中的 Node.js:**
```javascript
const win = new BrowserWindow({
  webPreferences: {
    nodeIntegrationInWorker: true
  }
})
```

**创建 Worker:**
```javascript
// main.js
const worker = new Worker('worker.js')

// worker.js
const fs = require('node:fs')
fs.readFile('/path/to/file', (err, data) => {
  postMessage(data)
})
```

**技术说明:**
- Worker 可以使用 Node.js API
- 不能使用 Electron API
- 适合 CPU 密集型任务
- 注意线程安全问题

---

### 12. 调试

**调试技术**

**Chrome DevTools:**
```javascript
// 打开 DevTools
mainWindow.webContents.openDevTools()

// 自动打开 DevTools
const mainWindow = new BrowserWindow({
  webPreferences: {
    devTools: true
  }
})
```

**VS Code 调试:**
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug Main Process",
      "type": "node",
      "request": "launch",
      "cwd": "${workspaceFolder}",
      "runtimeExecutable": "${workspaceFolder}/node_modules/.bin/electron",
      "windows": {
        "runtimeExecutable": "${workspaceFolder}/node_modules/.bin/electron.cmd"
      },
      "args": ["."]
    }
  ]
}
```

**性能分析:**
- 使用 Chrome Tracing
- CPU 性能分析
- 内存堆快照
- 网络性能监控

---

### 13. 测试

**自动化测试**

**使用 Spectron:**
```javascript
const Application = require('spectron').Application
const electronPath = require('electron')
const path = require('path')

const app = new Application({
  path: electronPath,
  args: [path.join(__dirname, '..')]
})

describe('Application launch', function () {
  this.timeout(30000)

  beforeEach(async () => {
    await app.start()
  })

  afterEach(async () => {
    if (app && app.isRunning()) {
      await app.stop()
    }
  })

  it('shows an initial window', async () => {
    const windowCount = await app.client.getWindowCount()
    assert.equal(windowCount, 1)
  })
})
```

**使用 Playwright:**
```javascript
const { _electron: electron } = require('playwright')

test('example test', async () => {
  const electronApp = await electron.launch({
    args: ['./main.js']
  })

  const window = await electronApp.firstWindow()
  await window.screenshot({ path: 'intro.png' })

  await electronApp.close()
})
```

**技术说明:**
- 测试主进程和渲染进程
- 模拟用户交互
- 测试跨平台兼容性

---

### 14. 性能优化

**性能优化策略**

**测量性能:**
```javascript
const { app } = require('electron')

app.on('ready', () => {
  console.log('App ready time:', process.uptime())
})
```

**优化技巧:**
1. **懒加载窗口**
   - 延迟创建非必要窗口
   - 使用 `show: false` 创建隐藏窗口

2. **优化 IPC**
   - 批量处理消息
   - 使用异步通信
   - 避免传输大量数据

3. **内存管理**
   - 及时释放不再需要的对象
   - 使用 `--js-flags` 优化内存
   - 监控内存使用

4. **渲染优化**
   - 使用虚拟滚动
   - 优化 CSS 选择器
   - 减少 DOM 操作

**技术说明:**
- 使用 Chrome DevTools 分析性能
- 监控 CPU 和内存使用
- 优化启动时间
- 减少内存占用

---

### 15. 分发概览

**应用分发流程**

**1. 打包应用**
```bash
npm run package
```

**2. 代码签名**
- Windows: 使用证书签名
- macOS: 使用开发者 ID 签名

**3. 公开发布**
- 上传到网站
- 提交到应用商店
- 配置自动更新

**技术说明:**
- 打包将应用和运行时捆绑
- 代码签名确保应用完整性
- 自动更新提供无缝更新体验

---

## 高级主题

### 16. 原生模块

**使用原生 Node 模块**

**重新构建原生模块:**
```bash
npm rebuild --runtime=electron --target=$(electron -v) --disturl=https://electronjs.org/headers
```

**注意事项:**
- 必须针对 Electron 构建
- 使用正确的 Node.js 版本
- 考虑安全性影响

---

### 17. 自定义协议

**注册自定义协议**

```javascript
const { protocol } = require('electron')

// 注册特权协议
protocol.registerSchemesAsPrivileged([
  {
    scheme: 'app',
    privileges: {
      secure: true,
      standard: true,
      supportFetchAPI: true
    }
  }
])

// 处理协议请求
protocol.handle('app', (request) => {
  const url = request.url.substr(7) // 去掉 'app://'
  return net.fetch(`file://${path.join(__dirname, url)}`)
})
```

**技术说明:**
- 必须在 app ready 之前注册
- 支持自定义 URL 方案
- 可以拦截现有协议

---

### 18. 扩展支持

**加载 Chrome 扩展**

```javascript
const { session, app } = require('electron')

app.on('ready', async () => {
  const ext = await session.defaultSession.loadExtension(
    '/path/to/unpacked/extension'
  )
  console.log(`Loaded extension: ${ext.id}`)
})
```

**技术说明:**
- 支持部分 Chrome 扩展
- 某些 API 可能不可用
- 需要解压的扩展

---

### 19. 多窗口管理

**管理多个窗口**

```javascript
const windows = new Set()

function createWindow() {
  const win = new BrowserWindow({
    width: 800,
    height: 600
  })

  windows.add(win)

  win.on('closed', () => {
    windows.delete(win)
  })

  return win
}

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit()
  }
})
```

**技术说明:**
- 管理窗口生命周期
- 处理窗口关闭事件
- 实现 macOS 特定的窗口行为

---

### 20. 系统集成

**桌面集成功能**

**文件关联:**
```json
{
  "name": "my-app",
  "fileAssociations": [
    {
      "ext": ["txt", "md"],
      "name": "Text Document",
      "role": "Editor"
    }
  ]
}
```

**URL 协议处理:**
```javascript
app.setAsDefaultProtocolClient('my-app')

app.on('open-url', (event, url) => {
  console.log('Opened URL:', url)
})
```

**技术说明:**
- 注册文件类型
- 处理自定义 URL 方案
- 实现深度链接

---

## 最佳实践

### 1. 项目结构

**推荐的目录结构:**
```
my-app/
├── src/
│   ├── main/          # 主进程代码
│   ├── renderer/      # 渲染进程代码
│   ├── preload/       # 预加载脚本
│   └── shared/        # 共享代码
├── assets/            # 静态资源
├── tests/             # 测试文件
└── package.json
```

### 2. 配置管理

**环境变量:**
```javascript
const isDev = require('electron-is-dev')

const config = {
  apiUrl: isDev ? 'http://localhost:3000' : 'https://api.example.com'
}
```

### 3. 错误处理

**全局错误处理:**
```javascript
process.on('uncaughtException', (error) => {
  console.error('Uncaught Exception:', error)
})

process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled Rejection at:', promise, 'reason:', reason)
})
```

### 4. 日志记录

**使用 electron-log:**
```bash
npm install electron-log
```

```javascript
const log = require('electron-log')

log.info('Application started')
log.error('An error occurred', error)
```

---

## 相关资源

- **API 文档**: ./api.md
- **开发指南**: ./development.md
- **进程管理**: ./processes.md
- **常见问题**: ./other.md
- **快速入门**: ./getting_started.md

---

**最后更新:** 基于 Electron 最新稳定版
**文档版本:** 对应 Electron 官方教程文档
