# Electron - 快速入门

**页数:** 1

---

## 简介 | Electron

**URL:** https://www.electronjs.org/docs/latest/

Electron 是一个使用 JavaScript、HTML 和 CSS 构建跨平台桌面应用程序的框架。它将 Chromium 和 Node.js 结合到一个运行时中，允许您使用纯 Web 技术构建桌面应用程序。

---

## 主要特点

### 跨平台支持
- 支持 Windows、macOS 和 Linux
- 一套代码，多平台运行
- 自动适配不同操作系统的原生特性

### 技术栈
- **Chromium**: 负责渲染 Web 内容
- **Node.js**: 提供后端 API 和文件系统访问
- **V8**: Google 的 JavaScript 引擎

### 开发优势
- 使用熟悉的 Web 技术（HTML/CSS/JavaScript）
- 丰富的 npm 生态系统
- 活跃的社区支持
- 成熟的工具链和调试工具

---

## 快速开始

### 安装
```bash
# 使用 npm 安装
npm install electron

# 使用 yarn 安装
yarn add electron
```

### 创建应用
```javascript
// main.js - 主进程文件
const { app, BrowserWindow } = require('electron')

function createWindow () {
  // 创建浏览器窗口
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true
    }
  })

  // 加载 index.html
  win.loadFile('index.html')
}

// 当 Electron 完成初始化时创建窗口
app.whenReady().then(createWindow)
```

### 运行应用
```bash
electron .
```

---

## 核心概念

### 主进程 (Main Process)
- 每个 Electron 应用都有一个主进程
- 管理 application 生命周期和所有 BrowserWindow
- 可以使用 Node.js 的所有 API

### 渲染进程 (Renderer Process)
- 每个 BrowserWindow 都有一个独立的渲染进程
- 负责渲染 Web 页面
- 默认情况下无法访问 Node.js API

### 预加载脚本 (Preload Scripts)
- 在渲染进程加载之前执行
- 可以访问 Node.js API
- 通过 contextBridge 安全地暴露 API 给渲染进程

---

## 下一步
- 查看 [教程](./tutorial.md) 了解更多
- 阅读 [API 文档](./api.md) 深入学习
- 参考 [开发指南](./development.md) 构建应用
