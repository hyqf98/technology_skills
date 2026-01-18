# Electron - 其他主题

**页数:** 4

---

## 为什么选择 Electron

**URL:** https://www.electronjs.org/docs/latest/why-electron

**内容:**
- 为什么选择 Electron
- 为什么选择 Web 技术
  - 多功能性
  - 可靠性
  - 互操作性
  - 普遍性
- 为什么选择 Electron 框架
  - 企业级
  - 成熟度
  - 稳定性、安全性、性能

**核心优势:**

### Web 技术的优势
- **多功能性**: HTML、CSS、JavaScript 可以实现任何用户界面
- **可靠性**: Web 技术经过数十年的优化和硬化
- **互操作性**: 几乎所有服务和数据都提供 Web 集成
- **普遍性**: 几乎所有设备都能运行 Web 技术

**实际案例:**
- NASA 的任务控制中心
- 彭博终端（每用户每年 $25,000）
- 麦当劳点餐亭
- SpaceX Dragon 2 飞船界面

### Electron 的企业级特性
- **成熟稳定**: 被 Slack、Discord、Signal、VS Code、Notion 等主流应用使用
- **安全可靠**: 优先考虑稳定性、安全性和性能
- **持续维护**: 作为 OpenJS 基金会项目，与 Node.js、ESLint、Webpack 等共享资源
- **最佳体验**: 捆绑最新版本的 Chromium、V8 和 Node.js

**技术架构:**
- 捆绑 Chromium 网络栈确保一致的渲染
- 避免依赖操作系统内置 WebView 的版本限制
- 提供 Node.js 集成，实现完整的系统访问

**适用场景:**
- 跨平台桌面应用
- 需要 Web 技术的桌面软件
- 企业级应用
- 开发工具和生产力应用

**不适用场景:**
- 资源受限环境（IoT 设备）
- 需要极小磁盘占用的应用
- 主要使用原生 UI 组件的应用
- 高性能游戏和实时 3D 图形
- 仅嵌入轻量级网站的应用

---

## 术语表

**URL:** https://www.electronjs.org/docs/latest/glossary

**内容:**
- 术语表
  - ASAR
  - ASAR 完整性
  - 代码签名
  - 上下文隔离
  - CRT
  - DMG
  - IME
  - IDL
  - IPC

### 重要术语解释

**ASAR (Atom Shell Archive Format)**
- 类似 tar 的归档格式
- 将多个文件连接成单个文件
- Electron 可以无需解压直接读取
- 主要用于优化 Windows 上的大量小文件读取性能

**ASAR 完整性**
- 安全功能，验证 ASAR 归档内容
- 运行时验证头部哈希
- 哈希不匹配时强制终止应用

**代码签名 (Code Signing)**
- 数字签名代码以确保未被篡改
- Windows 和 macOS 各自实现
- 分发应用的必要步骤

**上下文隔离 (Context Isolation)**
- 安全措施，确保预加载脚本不会泄露特权 API
- 需要通过 contextBridge API 暴露 API
- 默认启用以提高安全性

**CRT (C Runtime Library)**
- C++ 标准库的一部分
- Visual C++ 库实现 CRT
- 支持原生代码和混合代码开发

**DMG (Apple Disk Image)**
- macOS 的打包格式
- 常用于分发应用"安装程序"

**IME (Input Method Editor)**
- 输入法编辑器
- 允许用户输入键盘上没有的字符
- 支持中文、日文、韩文等

**IPC (Inter-Process Communication)**
- 进程间通信
- Electron 使用 IPC 在主进程和渲染进程间发送序列化 JSON 消息

**主进程 (Main Process)**
- 应用的入口点（通常是 main.js）
- 控制应用生命周期
- 管理原生元素（菜单、托盘等）
- 创建和管理渲染进程
- 完整的 Node API 内置

**渲染进程 (Renderer Process)**
- 应用中的浏览器窗口
- 可以有多个，每个在独立进程中运行
- 可以隐藏

**沙箱 (Sandbox)**
- 继承自 Chromium 的安全功能
- 限制渲染进程的权限
- 只能访问有限的系统资源

**预加载脚本 (Preload Scripts)**
- 在渲染进程加载前执行的代码
- 运行在渲染进程上下文中
- 被授予访问 Node.js API 的特权

**V8**
- Google 的开源 JavaScript 引擎
- 用 C++ 编写
- Electron 作为 Chromium 的一部分构建 V8
- 版本号与 Google Chrome 对应

**Webview 标签**
- 用于在 Electron 应用中嵌入"访客"内容
- 类似 iframe，但每个 webview 在独立进程中运行
- 异步交互，保持应用安全

---

## 破坏性更改

**URL:** https://www.electronjs.org/docs/latest/breaking-changes

**内容:**
- 破坏性更改
  - 破坏性更改类型
- 计划的破坏性 API 更改 (39.0)
- 计划的破坏性 API 更改 (38.0)

**破坏性更改政策:**
- 至少提前一个主要版本发出弃用警告
- 在可能的情况下在 JS 代码中添加弃用警告
- 文档化所有破坏性更改

**近期重要更改:**

### Electron 39.0 计划更改
- 弃用 `--host-rules` 命令行开关，改用 `--host-resolver-rules`
- `window.open` 弹窗始终可调整大小
- 共享纹理 OSR 绘制事件数据结构变更

### Electron 38.0 计划更改
- 移除 `ELECTRON_OZONE_PLATFORM_HINT` 环境变量
- 移除 `ORIGINAL_XDG_CURRENT_DESKTOP` 环境变量
- 不再支持 macOS 11

### 历史重要更改
- **macOS 支持**: 不再支持 macOS 10.13 (High Sierra) 和 10.14 (Mojave)
- **Windows 支持**: 不再支持 Windows 7、8 和 8.1
- **IPC 更改**: 使用结构化克隆算法替代自定义序列化
- **上下文隔离**: 默认启用
- **沙箱**: 渲染进程默认启用沙箱
- **Remote 模块**: 已弃用，推荐使用 `@electron/remote`

**迁移指南:**
- 查看具体版本的破坏性更改文档
- 使用语义化 PR 标题追踪更改
- 参考 API History 块了解详细变更信息

---

## Electron 常见问题

**URL:** https://www.electronjs.org/docs/latest/faq

**内容:**
- Electron FAQ
- 为什么我在安装 Electron 时遇到问题？
- Electron 二进制文件如何下载？
- Electron 何时升级到最新的 Chromium？
- Electron 何时升级到最新的 Node.js？
- 如何在网页之间共享数据？
- 我的应用托盘几分钟后消失了。
- 我不能在 Electron 中使用 jQuery/RequireJS/Meteor/AngularJS。
- require('electron').xxx 未定义。
- 字体看起来模糊，这是什么，我该怎么办？

**常见问题解答:**

### 安装问题
**问题**: `npm install electron` 失败，出现 `ELIFECYCLE`、`EAI_AGAIN`、`ECONNRESET` 等错误

**解决方案**:
- 通常是网络问题，不是 electron 包本身的问题
- 尝试切换网络或稍后重试
- 可以从 GitHub Releases 直接下载
- 参考[高级安装文档](https://www.electronjs.org/docs/latest/tutorial/installation)

### 版本升级
**Chromium 升级**:
- 每 8 周发布一次新版本
- 每隔一个 Chromium 主要版本在发布当天纳入
- 安全修复会提前移植到稳定版本

**Node.js 升级**:
- 新版本发布后等待约一个月
- 避免新版本中的 bug
- 新功能通常来自 V8 升级

### 数据共享
**网页间共享数据**:
- 使用 HTML5 API（Storage API、localStorage、sessionStorage、IndexedDB）
- 使用 IPC 原语（ipcMain 和 ipcRenderer）
- 使用 MessagePort 直接通信

### 托盘消失
**问题**: 托盘图标几分钟后消失

**原因**: 存储托盘的变量被垃圾回收

**解决方案**:
```javascript
// 错误示例 - 变量会被垃圾回收
const { app, Tray } = require('electron')
app.whenReady().then(() => {
  const tray = new Tray('/path/to/icon.png')
  tray.setTitle('hello world')
})

// 正确示例 - 保持引用
const { app, Tray } = require('electron')
let tray = null
app.whenReady().then(() => {
  tray = new Tray('/path/to/icon.png')
  tray.setTitle('hello world')
})
```

### 库冲突
**问题**: jQuery、RequireJS 等库无法使用

**原因**: Electron 的 Node.js 集成插入了额外的符号（module、exports、require）

**解决方案**:
```javascript
// 方案 1: 禁用 Node 集成
const { BrowserWindow } = require('electron')
const win = new BrowserWindow({
  webPreferences: {
    nodeIntegration: false
  }
})

// 方案 2: 重命名符号
<head>
  <script>
    window.nodeRequire = require;
    delete window.require;
    delete window.exports;
    delete window.module;
  </script>
  <script type="text/javascript" src="jquery.js"></script>
</head>
```

### API 未定义
**问题**: `require('electron').xxx` 返回 undefined

**原因**: 在错误的进程中使用模块

**解决方案**:
- `electron.app` 只能在主进程使用
- `electron.webFrame` 只在渲染进程可用
- 检查 API 文档确认进程类型

### 字体模糊
**问题**: 字体在 LCD 屏幕上看起来模糊

**原因**: 子像素抗锯齿被禁用

**解决方案**:
```javascript
const { BrowserWindow } = require('electron')
const win = new BrowserWindow({
  backgroundColor: '#fff'
  // 需要非透明背景
})
```

**注意**:
- 仅在 CSS 中设置背景无效
- 必须在构造函数中设置
- 对某些用户可见，即使您看不到差异

### 类继承
**问题**: 无法使用 `extends` 关键字继承 Electron 类

**原因**: Electron 从未实现此功能

**说明**: 由于 C++/JavaScript 互操作的复杂性，不支持子类化

**参考**: [electron/electron#23](https://github.com/electron/electron/issues/23)
