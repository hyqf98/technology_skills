---
name: electron
description: Electron 框架 - 使用 JavaScript、HTML 和 CSS 构建跨平台桌面应用程序。涵盖主进程和渲染进程、IPC 通信、原生 API、安全最佳实践和应用分发。
---

# Electron 技能文档

基于官方文档生成的 Electron 开发综合指南。

## 何时使用此技能

在以下场景中使用此技能：
- 开发 Electron 跨平台桌面应用
- 查询 Electron 功能或 API
- 实现 Electron 解决方案
- 调试 Electron 代码
- 学习 Electron 最佳实践

## 技术概述

**Electron** 是一个使用 JavaScript、HTML 和 CSS 构建跨平台桌面应用程序的框架。它将 Chromium 和 Node.js 结合到同一个运行时环境中，使开发者能够使用 Web 技术创建原生级别的桌面应用。

### 核心特性

- **跨平台支持**：支持 Windows、macOS 和 Linux
- **Web 技术**：使用 HTML、CSS、JavaScript 开发
- **Node.js 集成**：访问所有 Node.js API 和原生模块
- **自动更新**：内置自动更新机制
- **原生菜单**：系统级菜单和通知
- **强大的打包工具**：electron-builder、electron-forge 等

### 架构模式

Electron 采用多进程架构：

```
┌─────────────────────────────────────────────────────────────┐
│                      Electron 应用架构                         │
├─────────────────────────────────────────────────────────────┤
│  主进程（Main Process）          │  渲染进程（Renderer Process）  │
│  ├─ 应用生命周期                 │  ├─ Web 页面渲染              │
│  ├─ 窗口管理                     │  ├─ 用户界面                 │
│  ├─ 原生菜单/对话框              │  └─ 沙箱环境                 │
│  ├─ ipcMain 模块                 │                              │
│  └─ Node.js 完整访问             │  └─ ipcRenderer（通过 preload）│
└─────────────────────────────────────────────────────────────┘
```

## 快速参考

### 常见配置模式

#### 模式 1：创建主窗口

```javascript
const { app, BrowserWindow } = require('electron');

function createWindow() {
    const win = new BrowserWindow({
        width: 800,
        height: 600,
        webPreferences: {
            nodeIntegration: false,
            contextIsolation: true,
            preload: path.join(__dirname, 'preload.js')
        }
    });

    win.loadFile('index.html');
}

app.whenReady().then(createWindow);
```

#### 模式 2：IPC 通信（单向）

**主进程 (main.js):**
```javascript
const { ipcMain } = require('electron');

ipcMain.on('message-from-renderer', (event, data) => {
    console.log('收到消息:', data);
    // 发送回渲染进程
    event.reply('message-from-main', '回复: ' + data);
});
```

**预加载脚本 (preload.js):**
```javascript
const { contextBridge, ipcRenderer } = require('electron');

contextBridge.exposeInMainWorld('electronAPI', {
    sendMessage: (message) => ipcRenderer.send('message-from-renderer', message),
    onMessage: (callback) => ipcRenderer.on('message-from-main', callback)
});
```

**渲染进程:**
```javascript
// 发送消息
window.electronAPI.sendMessage('Hello Main!');

// 接收消息
window.electronAPI.onMessage((event, message) => {
    console.log('收到消息:', message);
});
```

#### 模式 3：IPC 通信（双向 - invoke/handle）

**主进程:**
```javascript
ipcMain.handle('get-app-version', async () => {
    return app.getVersion();
});

ipcMain.handle('save-file', async (event, content) => {
    // 执行文件保存操作
    return { success: true };
});
```

**预加载脚本:**
```javascript
contextBridge.exposeInMainWorld('electronAPI', {
    getAppVersion: () => ipcRenderer.invoke('get-app-version'),
    saveFile: (content) => ipcRenderer.invoke('save-file', content)
});
```

**渲染进程:**
```javascript
async function getVersion() {
    const version = await window.electronAPI.getAppVersion();
    console.log('应用版本:', version);
}
```

#### 模式 4：使用 BaseWindow（多视图窗口）

```javascript
const { BaseWindow, WebContentsView } = require('electron');

const win = new BaseWindow({ width: 800, height: 600 });

// 创建左侧视图
const leftView = new WebContentsView();
leftView.webContents.loadURL('https://electronjs.org');
win.contentView.addChildView(leftView);

// 创建右侧视图
const rightView = new WebContentsView();
rightView.webContents.loadURL('https://github.com/electron/electron');
win.contentView.addChildView(rightView);

// 设置视图布局
leftView.setBounds({ x: 0, y: 0, width: 400, height: 600 });
rightView.setBounds({ x: 400, y: 0, width: 400, height: 600 });
```

#### 模式 5：安全配置（推荐）

```javascript
const win = new BrowserWindow({
    webPreferences: {
        // 安全配置
        nodeIntegration: false,        // 禁用 Node.js 集成
        contextIsolation: true,        // 启用上下文隔离
        sandbox: true,                 // 启用沙箱模式
        preload: path.join(__dirname, 'preload.js'),
        // 内容安全策略
        webSecurity: true
    }
});
```

#### 模式 6：处理窗口打开事件

```javascript
const win = new BrowserWindow();

// 限制窗口打开行为
win.webContents.setWindowOpenHandler(({ url }) => {
    // 只允许特定 URL
    if (url === 'about:blank') {
        return {
            action: 'allow',
            overrideBrowserWindowOptions: {
                frame: false,
                fullscreenable: false,
                backgroundColor: 'black'
            }
        };
    }
    // 阻止其他所有窗口打开
    return { action: 'deny' };
});
```

### 代码示例模式

#### 示例 1：安装测试依赖

```bash
# 使用 WebdriverIO 进行测试
npm init wdio@latest ./

# 或使用 Yarn
yarn create wdio@latest ./
```

#### 示例 2：配置 WebdriverIO

```javascript
// wdio.conf.js
export const config = {
    services: ['electron'],
    capabilities: [{
        browserName: 'electron',
        'wdio:electronServiceOptions': {
            appArgs: ['foo', 'bar=baz']
        }
    }]
};
```

#### 示例 3：使用 Playwright 测试

```javascript
import { test, _electron as electron } from '@playwright/test';

test('launch app', async () => {
    const electronApp = await electron.launch({ args: ['.'] });

    // 获取应用信息
    const isPackaged = await electronApp.evaluate(async ({ app }) => {
        return app.isPackaged;
    });

    // 获取窗口截图
    const window = await electronApp.firstWindow();
    await window.screenshot({ path: 'intro.png' });

    await electronApp.close();
});
```

#### 示例 4：设置 CSP 头部

```javascript
// main.js
const session = mainWindow.webContents.session;

session.webRequest.onHeadersReceived((details, callback) => {
    callback({
        responseHeaders: {
            ...details.responseHeaders,
            'Content-Security-Policy': [
                "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline';"
            ]
        }
    });
});
```

#### 示例 5：处理未捕获的异常

```javascript
// 主进程
process.on('uncaughtException', (error) => {
    console.error('未捕获的异常:', error);
});

process.on('unhandledRejection', (reason, promise) => {
    console.error('未处理的 Promise 拒绝:', reason);
});
```

## 安全最佳实践（2025）

### 必须启用的安全配置

```javascript
const win = new BrowserWindow({
    webPreferences: {
        // ✅ 必须启用
        nodeIntegration: false,       // 禁止渲染进程直接访问 Node.js
        contextIsolation: true,       // 隔离预加载脚本上下文
        sandbox: true,                // 启用操作系统级别的沙箱
        preload: path.join(__dirname, 'preload.js'),
        webSecurity: true,            // 启用 Web 安全策略

        // ❌ 禁止使用的配置
        // nodeIntegration: true       // 永远不要在生产环境启用
        // contextIsolation: false     // 永远不要禁用
    }
});
```

### 预加载脚本模式

```javascript
// preload.js - 安全的 API 暴露
const { contextBridge, ipcRenderer } = require('electron');

// 只暴露必要的 API
contextBridge.exposeInMainWorld('electronAPI', {
    // 白名单频道
    sendMessage: (channel, data) => {
        const validChannels = ['update-data', 'save-settings'];
        if (validChannels.includes(channel)) {
            ipcRenderer.send(channel, data);
        }
    },
    onMessage: (channel, callback) => {
        const validChannels = ['update-complete'];
        if (validChannels.includes(channel)) {
            ipcRenderer.on(channel, (event, ...args) => callback(...args));
        }
    }
});
```

### 内容安全策略（CSP）

```html
<!-- 在 HTML 文件中设置 CSP -->
<meta http-equiv="Content-Security-Policy"
      content="default-src 'self';
               script-src 'self';
               style-src 'self' 'unsafe-inline';
               img-src 'self' data: https:;
               font-src 'self';">
```

### 常见安全问题

| 安全问题 | 风险 | 解决方案 |
|---------|------|---------|
| 禁用 contextIsolation | 预加载脚本可被网页访问 | 始终启用 `contextIsolation: true` |
| 启用 nodeIntegration | 渲染进程可访问 Node.js API | 始终设置 `nodeIntegration: false` |
| 直接暴露 ipcRenderer | 恶意网页可滥用 IPC | 通过 `contextBridge` 白名单暴露 |
| 加载远程内容 | XSS 和代码注入风险 | 验证来源，使用 CSP |
| 使用 eval() | 代码注入风险 | 避免使用，使用 JSON.parse |

## IPC 通信模式

### 单向消息（发送后不等待响应）

```javascript
// 主进程
ipcMain.on('log-message', (event, message) => {
    console.log(message);
});

// 渲染进程
window.electronAPI.log('Hello from renderer');
```

### 双向消息（等待响应）

```javascript
// 主进程
ipcMain.handle('fetch-data', async (event, id) => {
    return await database.fetch(id);
});

// 渲染进程
const data = await window.electronAPI.fetchData(123);
```

### 流式数据传输

```javascript
// 主进程
ipcMain.on('start-stream', (event) => {
    const stream = fs.createReadStream('large-file.txt');
    stream.on('data', (chunk) => {
        event.sender.send('stream-chunk', chunk);
    });
    stream.on('end', () => {
        event.sender.send('stream-end');
    });
});
```

## 测试自动化

### 使用 WebdriverIO

```bash
npm install --save-dev @wdio/electron-service
```

```javascript
// 测试文件
describe('Electron App', () => {
    it('should detect keyboard input', async () => {
        await browser.keys(['y', 'o']);
        await expect($('keypress-count')).toHaveText('YO');
    });

    it('should trigger message modal', async () => {
        await browser.electron.execute(
            (electron, param1, param2) => {
                const appWindow = electron.BrowserWindow.getFocusedWindow();
                electron.dialog.showMessageBox(appWindow, {
                    message: `Sum: ${param1 + param2}`
                });
            },
            1, 2
        );
    });
});
```

### 使用 Playwright

```bash
npm install --save-dev @playwright/test
```

```javascript
import { test, expect, _electron as electron } from '@playwright/test';

test('app test', async () => {
    const electronApp = await electron.launch({ args: ['.'] });
    const window = await electronApp.firstWindow();

    // 测试标题
    await expect(window).toHaveTitle(/My App/);

    // 测试元素
    const button = window.locator('button#submit');
    await button.click();

    await electronApp.close();
});
```

## 打包与分发

### 使用 electron-builder

```json
// package.json
{
    "build": {
        "appId": "com.example.myapp",
        "productName": "MyApp",
        "directories": {
            "output": "dist"
        },
        "files": [
            "build/**/*",
            "node_modules/**/*"
        ],
        "mac": {
            "category": "public.app-category.productivity"
        },
        "win": {
            "target": ["nsis"]
        },
        "linux": {
            "target": ["AppImage", "deb"]
        }
    }
}
```

```bash
# 构建应用
npm run build

# 或使用 electron-builder
npx electron-builder
```

### 使用 electron-forge

```bash
# 初始化项目
npm init electron-forge@latest my-app

# 构建应用
npm run make

# 运行应用
npm start
```

## 参考文档

此技能在 `references/` 目录中包含全面的文档：

| 文档 | 描述 |
|------|------|
| **api.md** | API 完整参考 |
| **development.md** | 开发指南 |
| **getting_started.md** | 入门教程 |
| **other.md** | 其他主题 |
| **processes.md** | 进程模型详解 |
| **tutorial.md** | 完整教程 |

## 使用指南

### 对于初学者
1. 从 `getting_started.md` 开始了解基础
2. 创建第一个 Hello World 应用
3. 理解主进程和渲染进程的区别
4. 学习 IPC 通信基础
5. 实践窗口管理和原生功能

### 对于安全开发
- 始终启用 `contextIsolation` 和 `sandbox`
- 使用预加载脚本作为安全桥梁
- 实施严格的 IPC 频道验证
- 定期更新 Electron 版本
- 参考 `development.md` 中的安全章节

### 对于性能优化
- 使用懒加载减少启动时间
- 优化 IPC 通信频率
- 使用 BrowserWindow 的 `show: false` 预加载窗口
- 参考 `development.md` 中的性能优化章节

## 资源

### references/
从官方来源提取的组织化文档，包含：
- 详细的 API 说明
- 带语言标注的代码示例
- 原始文档链接
- 快速导航目录

### scripts/
添加用于常见自动化任务的辅助脚本。

### assets/
添加模板、样板代码或示例项目。

## 注意事项

- 此技能从官方文档自动生成
- 安全配置对生产环境至关重要
- IPC 通信需要严格的频道验证
- 定期更新以获取安全补丁

## 更新说明

刷新此技能的文档：
1. 使用相同配置重新运行爬虫
2. 技能将使用最新信息重建

## 最新特性 (2025)

### setWindowOpenHandler API

**主进程窗口创建控制:**
```javascript
const mainWindow = new BrowserWindow();

// 控制新窗口的创建
mainWindow.webContents.setWindowOpenHandler(({ url }) => {
    if (url === 'about:blank') {
        return {
            action: 'allow',
            overrideBrowserWindowOptions: {
                frame: false,
                fullscreenable: false,
                backgroundColor: 'black',
                webPreferences: {
                    preload: 'my-child-window-preload-script.js'
                }
            }
        };
    }
    return { action: 'deny' };
});
```

**技术说明:**
- 安全地控制窗口打开行为
- 可以覆盖 BrowserWindow 构造选项
- 支持自定义安全配置
- 推荐用于防止意外窗口打开

### 安全的 contextBridge 模式 (2025 推荐)

**正确的 IPC API 暴露方式:**
```javascript
// preload.js - 推荐模式
const { contextBridge, ipcRenderer } = require('electron');

// ✅ 好的做法 - 只暴露特定方法
contextBridge.exposeInMainWorld('myAPI', {
    loadPreferences: () => ipcRenderer.invoke('load-prefs')
});

// ❌ 坏的做法 - 暴露过多权限
contextBridge.exposeInMainWorld('electronAPI', {
    on: ipcRenderer.on  // 危险！暴露了事件对象
});

// ❌ 同样不好的做法
contextBridge.exposeInMainWorld('electronAPI', {
    onUpdateCounter: (callback) => ipcRenderer.on('update-counter', callback)
});

// ✅ 正确的做法 - 只传递数据
contextBridge.exposeInMainWorld('electronAPI', {
    onUpdateCounter: (callback) => ipcRenderer.on('update-counter', (_event, value) => callback(value))
});
```

### MessageChannelMain 高级通信

**主进程端通道通信:**
```javascript
const { BrowserWindow, MessageChannelMain } = require('electron');

const w = new BrowserWindow();
const { port1, port2 } = new MessageChannelMain();

// 将 port2 发送到渲染进程
w.webContents.postMessage('port', null, [port2]);

// 在主进程中使用 port1 发送消息
port1.postMessage({ some: 'message' });
```

**渲染进程接收:**
```javascript
const { ipcRenderer } = require('electron');

ipcRenderer.on('port', (e) => {
    // e.ports 是随此消息发送的端口列表
    e.ports[0].onmessage = (messageEvent) => {
        console.log(messageEvent.data);
    };
});
```

### utilityProcess 进程管理

**创建子进程:**
```javascript
const { utilityProcess, MessageChannelMain } = require('electron');

// 创建具有 Node.js 的子进程
const child = utilityProcess.fork(path.join(__dirname, 'worker.js'));

// 发送消息
child.postMessage({ message: 'hello' });

// 监听事件
child.on('spawn', () => console.log('Process spawned'));
child.on('error', (error) => console.error('Process error:', error));
child.on('exit', (code) => console.log('Process exited with code:', code));
```

**子进程代码 (worker.js):**
```javascript
process.parentPort.on('message', (e) => {
    console.log('Received:', e.data);
    process.parentPort.postMessage({ reply: 'ack' });
});
```

## 版本注意事项 (2025)

### Electron 39.x/38.x 更新

**破坏性更改:**
- `--host-rules` 命令行开关已弃用，改用 `--host-resolver-rules`
- `window.open` 弹窗始终可调整大小
- 移除了 macOS 11 支持
- 移除了部分环境变量

**迁移建议:**
- 检查破坏性更改文档
- 更新命令行参数
- 测试在最新版本上的兼容性
- 更新依赖的安全补丁

## 相关资源

### 官方文档
- [Electron 官方网站](https://www.electronjs.org/)
- [Electron 文档](https://www.electronjs.org/docs/latest/)
- [Electron GitHub](https://github.com/electron/electron)

### 安全资源
- [Electron 安全指南](https://www.electronjs.org/docs/latest/tutorial/security)
- [上下文隔离](https://www.electronjs.org/docs/latest/tutorial/context-isolation)
- [IPC 通信](https://www.electronjs.org/docs/latest/tutorial/ipc)
- [窗口打开处理](https://www.electronjs.org/docs/latest/api/window-open)

### 测试资源
- [WebdriverIO Electron Service](https://webdriver.io/docs/electron-service)
- [Playwright Electron Guide](https://playwright.dev/docs/api/class-electron)

### 中文资源
- [2025 Electron窗口通信完全指南](https://blog.csdn.net/gitblog_00728/article/details/151853189)
- [Electron 进程间通信](https://electron.js.cn/docs/latest/tutorial/ipc)
