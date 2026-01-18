<div align="center">
  <a href="https://element-plus-x.com">
    <img src="https://cdn.element-plus-x.com/element-plus-x.png" alt="Element-Plus-X" width="180" class="logo" />
  </a>
</div>

<div align="center">

[![License](https://img.shields.io/badge/license-MIT-blue)](https://github.com/HeJiaYue520/Element-Plus-X/blob/main/LICENSE)&emsp;[![GitHub stars](https://img.shields.io/github/stars/HeJiaYue520/Element-Plus-X)](https://github.com/HeJiaYue520/Element-Plus-X)&emsp;[![npm version](https://img.shields.io/npm/v/vue-element-plus-x)](https://www.npmjs.com/package/vue-element-plus-x)&emsp;[![npm](https://img.shields.io/npm/dm/vue-element-plus-x.svg)](https://www.npmjs.com/package/vue-element-plus-x)&emsp;[![english doc](https://img.shields.io/badge/%E6%96%87%E6%A1%A3-%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87-brightgreen?style=flat-square&logo=read-the-docs)](https://github.com/HeJiaYue520/Element-Plus-X/blob/main/README.md)

</div>

<div align="center">
<h2>💖项目模板已发布💖</h2>
<img src="https://cdn.element-plus-x.com/chat/1.webp" />&emsp;
<img src="https://cdn.element-plus-x.com/demo.webp" calss="element-plus-x-bubble" />&emsp;
<img src="https://cdn.element-plus-x.com/demo1.webp" calss="element-plus-x-bubble" />&emsp;
<img src="https://cdn.element-plus-x.com/demo3.webp" calss="element-plus-x-bubble" />&emsp;
<img src="https://cdn.element-plus-x.com/demo4.webp" calss="element-plus-x-bubble" />&emsp;
</div>

<div align="center">

**English** | [简体中文](./README.md)

</div>&emsp;

# 🚀 Element-Plus-X

**一个开箱即用的企业级 AI 组件库（基于 Vue 3 + Element-Plus）**

## 📢 快速链接

| 资源类型                  | <div style="width: 300px;">链接</div>                                                                                      |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **文档**                  | [📖 开发文档](https://element-plus-x.com)                                                                                   |
| **在线演示**              | [👁️ 在线预览](https://v.element-plus-x.com)                                                                                |
| **代码仓库**              | [🐙 GitHub](https://github.com/HeJiaYue520/Element-Plus-X) <br> [🚠 Gitee](https://gitee.com/he-jiayue/element-plus-x)     |
| **NPM 包**                | [📦 npm](https://www.npmjs.com/package/vue-element-plus-x)                                                                 |
| **问题反馈**              | [🐛 提交 Bug](https://github.com/HeJiaYue520/Element-Plus-X/issues)                                                        |
| **社区**                  | [🐒 讨论组](https://element-plus-x.com/en/introduce.html#%F0%9F%91%A5-%E7%A4%BE%E5%8C%BA%E6%94%AF%E6%8C%81)               |
| **模板项目预览**          | [👀 在线预览](https://chat.element-plus-x.com/)                                                                            |
| **模板项目源代码**        | [🐙 GitHub](https://github.com/HeJiaYue520/ruoyi-element-ai) <br> [🚠 Gitee](https://gitee.com/he-jiayue/ruoyi-element-ai) |

## 🛠️ 核心特性

- ✨ **企业级 AI 组件**：预置聊天机器人、语音交互等场景化组件
- 🚀 **零配置集成**：基于 Element-Plus 设计系统，开箱即用
- 📦 **按需加载**：提供 Tree Shaking 优化

## 🔎 安装

```bash
# NPM
npm install vue-element-plus-x

# PNPM（推荐）
pnpm install vue-element-plus-x

# Yarn
yarn install vue-element-plus-x

```

## 📚 使用示例

1. **按需导入**

```vue
<script setup>
import { BubbleList, Sender } from 'vue-element-plus-x';

const list = [
  {
    content: '你好，Element Plus X',
    role: 'user'
  }
];
</script>

<template>
  <div
    style="display: flex; flex-direction: column; height: 230px; justify-content: space-between;"
  >
    <BubbleList :list="list" />
    <Sender />
  </div>
</template>
```

2. **全量导入**

```ts
// main.ts
import { createApp } from 'vue';
import ElementPlusX from 'vue-element-plus-x';
import App from './App.vue';

const app = createApp(App);
// 使用 app.use() 进行全局导入
app.use(ElementPlusX);
app.mount('#app');
```

3. **CDN 导入**

```html
<!-- 此方法仍在测试中 -->
<!-- CDN 导入 -->
<script src="https://unpkg.com/vue-element-plus-x@1.3.0/dist/umd/index.js"></script>
```

## 🌟 已实现的组件和 Hooks

| 组件名称        | 说明                                                              | 文档链接                                                              |
| --------------- | ----------------------------------------------------------------- | --------------------------------------------------------------------- |
| `Typewriter`    | 打字机动画组件                                                     | [📄 文档](https://element-plus-x.com/en/components/typewriter/)        |
| `Bubble`        | 扩展气泡消息组件                                                   | [📄 文档](https://element-plus-x.com/en/components/bubble/)            |
| `BubbleList`    | 扩展气泡消息列表组件                                               | [📄 文档](https://element-plus-x.com/en/components/bubbleList/)       |
| `Conversations` | 扩展会话管理组件                                                   | [📄 文档](https://element-plus-x.com/en/components/conversations/)    |
| `Welcome`       | 欢迎组件                                                          | [📄 文档](https://element-plus-x.com/en/components/welcome/)          |
| `Prompts`       | 提示词组件                                                        | [📄 文档](https://element-plus-x.com/en/components/prompts/)          |
| `FilesCard`     | 文件卡片组件                                                      | [📄 文档](https://element-plus-x.com/en/components/filesCard/)        |
| `Attachments`   | 文件附件上传组件                                                  | [📄 文档](https://element-plus-x.com/en/components/attachments/)      |
| `Sender`        | 智能输入框（含语音交互和内置指令操作）                             | [📄 文档](https://element-plus-x.com/en/components/sender/)           |
| `MentionSender` | 指令输入框（含提及列表）                                          | [📄 文档](https://element-plus-x.com/en/components/mentionSender/)    |
| `Thinking`      | 扩展的"思考中..."组件                                             | [📄 文档](https://element-plus-x.com/en/components/thinking/)         |
| `ThoughtChain`  | 思想链组件                                                        | [📄 文档](https://element-plus-x.com/en/components/thoughtChain/)     |
| `useRecord`     | 浏览器内置语音识别 API Hooks                                      | [📄 文档](https://element-plus-x.com/en/components/useRecord/)        |
| `useXStream`    | 流模式接口 Hooks                                                  | [📄 文档](https://element-plus-x.com/en/components/useXStream/)       |
| `useSend & XRequest` | 流模式 Hooks 的拆分版（扩展）                                | [📄 文档](https://element-plus-x.com/en/components/useSend/)          |

## 🎯 开发计划（每周更新）

🎀 我们会收集大家在 issues 和交流群中遇到的问题和需求场景，制定短期和长期的开发计划。更多详情请移步 👉 **[开发计划](https://element-plus-x.com/en/roadmap.html)**

## 🤝 贡献

1. **Fork 仓库** → 2. **创建特性分支** → 3. **提交 Pull Request**

详情可移步 👉 **[📄 文档](https://element-plus-x.com/en/guide/develop.html)**

我们欢迎：

- 🐛 Bug 修复
- 💡 新功能建议
- 📝 文档改进
- 🎨 样式优化

## 👥 贡献者

<a href="https://openomy.app/github/element-plus-x/Element-Plus-X" target="_blank" style="display: block; width: 100%;" align="center">
  <img src="https://openomy.app/svg?repo=element-plus-x/Element-Plus-X&chart=bubble&latestMonth=3" target="_blank" alt="贡献排行榜" style="display: block; width: 100%;" />
 </a>

<a href="https://github.com/element-plus-x/Element-Plus-X/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=element-plus-x/Element-Plus-X" />
</a>

## 👥 社区支持

<div align="center">
<img src="https://cdn.element-plus-x.com/vx-2025-07-28.png" alt="微信交流群" width="180" style="margin: 20px;" />
<p>加入微信交流群，获取最新资讯和技术支持</p>

<p>如果群链接过期，请扫描作者二维码：</p>
<img src="https://cdn.element-plus-x.com/element-plus-x-author-vx.png" alt="作者微信" width="180" style="margin: 20px;" />
</div>
