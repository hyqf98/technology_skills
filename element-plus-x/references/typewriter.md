---
title: 打字机组件
---

::: warning
**`XMarkdown 组件`**已发布，可与`打字机组件`配合使用。请升级到`beta 1.2.2`版本体验。
:::

::: info
`v1.2.0 版本`为**样式覆盖**、**图表渲染**和**自定义代码高亮样式、自定义插件**提供了简单解决方案。

1. 我们将官方的 `prismjs` CSS 样式文件添加到了组件库中，可以直接在项目中导入，解决 **md 代码块高亮**问题。

2. 我们将 `Mermaid.js` 添加到了组件库中。用于解决简单的 **图表**渲染问题（`mermaid 格式`）。

3. 我们暴露了 `markdown-it` 内置的**代码高亮方法**和**插件**。使开发者更容易集成第三方生态系统**样式**和**插件**。

🐵 此温馨提示更新时间：`2025-07-06`
:::

## 简介

`Typewriter` 是一个高度可定制的`打字机组件`，灵感来源于 `ant-design-x` 官方 `bubble 组件`案例，提取了打字机方法。支持 Markdown 渲染和动态打字机效果。

## 代码示例

### 基础用法

<demo src="./demos/content.vue"></demo>

### Markdown 渲染

<demo src="./demos/isMarkdown.vue"></demo>

### MD 代码块高亮（v1.2.0 新增）

提供内置样式

<demo src="./demos/newMarkDown.vue"></demo>

### MD 插件模式（v1.2.0 新增）

如果您认为内置样式不好看，或者内置插件无法满足您的需求，可以通过插件模式自定义**样式**和**插件**。

<demo src="./demos/mermaid.vue"></demo>

### 启用打字机效果

<demo src="./demos/typing.vue"></demo>

### 打字机雾化效果

<demo src="./demos/isFog.vue"></demo>

### 动态内容更新

<demo src="./demos/updates.vue"></demo>

### 控制打字机

<demo src="./demos/customized.vue"></demo>

## 属性

| 属性名            | 类型                                                          | 是否必填 | 默认值    | 说明                                                            |
| ----------------- | ------------------------------------------------------------- | -------- | --------- | --------------------------------------------------------------- |
| `content`         | String                                                        | 否       | `''`      | 要显示的文本内容，支持纯文本或 Markdown 格式                    |
| `isMarkdown`      | Boolean                                                       | 否       | `false`   | 是否启用 Markdown 渲染模式                                      |
| `typing`          | Boolean \| `{ step?: number, interval?: number, suffix?: string }` | 否 | `false` | 是否启用打字机效果                                              |
| `typing.step`     | Number                                                        | 否       | `2`       | 每次打字的字符数                                                |
| `typing.interval` | Number                                                        | 否       | `50`      | 每次打字的间隔时间，单位（`ms`）                                |
| `typing.suffix`   | String                                                        | 否       | `'\|'`    | 打字机后缀光标字符（仅在非 Markdown 模式下生效）                 |
| `isFog`           | Boolean \| `{ bgColor?: string, width?: string }`             | 否       | `false`   | 是否启用雾化效果，可设置背景颜色和宽度                          |

## 事件

| 事件名     | 参数            | 类型     | 说明                       |
| ---------- | --------------- | -------- | -------------------------- |
| `@start`   | `ref` 实例      | Function | 打字机效果开始时触发       |
| `@finish`  | `ref` 实例      | Function | 打字机效果完成时触发       |
| `@writing` | `ref` 实例      | Function | 打字机打字过程中持续触发   |

## Ref 实例方法

| 属性名          | 类型     | 说明                               |
| --------------- | -------- | ----------------------------------- |
| `interrupt`     | Function | 中断打字机                          |
| `continue`      | Function | 继续未完成的打字机                  |
| `restart`       | Function | 重新开始打字机                      |
| `destroy`       | Function | 主动销毁打字机组件                  |
| `renderedContent` | String   | 获取打字机组件的渲染内容            |
| `isTyping`      | Boolean  | 是否正在打字机                      |
| `progress`      | Number   | 打字机进度，范围 0-100              |

## 特性

1. **Markdown 支持**：支持渲染 Markdown 格式文本并应用 GitHub 风格样式
2. **动态打字机效果**：可模拟打字机效果，逐字显示文本内容
3. **代码高亮**：内置 Prism.js，支持代码块语法高亮
4. **XSS 安全**：使用 DOMPurify 过滤 HTML 内容，防止 XSS 攻击
5. **灵活配置**：支持自定义打字速度、光标字符、后缀等参数
6. **可定制开发**：支持基于组件打字机状态进行定制化开发
