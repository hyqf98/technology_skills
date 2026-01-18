---
title: 气泡组件
---

::: warning
`版本 1.1.6` 新增打字机雾化效果继承。请更新版本体验。

🐵 最后更新时间：`2025-04-13`
:::

## 简介

`Bubble` 是一个聊天气泡组件，常用于聊天对话场景。可以展示对话内容，支持自定义头像、头部、内容、底部，并具有打字机效果和加载状态展示。组件内置 `Typewriter` 组件，可实现文本打字机动画效果。

## 代码示例

### 基础用法

<demo src="./demos/content.vue"></demo>

### 头像和位置

<demo src="./demos/avatar-and-placement.vue"></demo>

### 头部和底部

<demo src="./demos/header-and-footer.vue"></demo>

### 加载状态

<demo src="./demos/loading.vue"></demo>

### 打字机配置

<demo src="./demos/typing.vue"></demo>

### 启用 Markdown 渲染

<demo src="./demos/is-markdown.vue"></demo>

### 继承打字机图表和 MD 样式

<demo src="./demos/cssAndMermaid.vue"></demo>

### 雾化效果

<demo src="./demos/is-fog.vue"></demo>

### 自定义内容

<demo src="./demos/content-customize.vue"></demo>

### 变体和形状

<demo src="./demos/variant-and-shape.vue"></demo>

### 控制打字机

<demo src="./demos/customized.vue"></demo>

## 属性

| 属性名        | 类型              | 默认值   | 说明                                                                                                                              |
| ------------- | ----------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------- |
| `content`     | String            | ''       | 气泡内显示的文本内容                                                                                                               |
| `placement`   | String            | 'start'  | 气泡位置，可选值为 `'start'` 或 `'end'`，分别代表左侧和右侧                                                                       |
| `avatar`      | String            | ''       | 气泡头像的图片 URL                                                                                                                |
| `loading`     | Boolean           | false    | 是否显示加载状态，当为 `true` 时，气泡内会显示加载动画                                                                             |
| `shape`       | String            | null     | 气泡形状，可选值为 `'round'`（圆角）或 `'corner'`（直角）                                                                        |
| `variant`     | String            | 'filled' | 气泡风格变体，可选值为 `'filled'`（实心）、`'borderless'`（无边框）、`'outlined'`（轮廓）、`'shadow'`（阴影）                    |
| `noStyle`     | Boolean           | false    | 是否移除样式，当为 `true` 时，移除气泡的内置 `padding` 和 `背景颜色`                                                              |
| `isMarkdown`  | Boolean           | false    | 是否将 `content` 作为 Markdown 格式处理                                                                                           |
| `typing`      | Boolean \| Object | false    | 是否启用打字机效果，如果是对象，可以设置 `step`（每次渲染的字符数）和 `suffix`（打字机光标后缀内容），`interval` 表示打字间隔时间，单位 `ms` |
| `maxWidth`    | String            | '500px'  | 气泡内容的最大宽度                                                                                                                |
| `avatar-size` | String            | ''       | 设置头像占位符尺寸                                                                                                                |
| `avatar-gap`  | String            | '12px'   | 设置头像与气泡之间的 `gap` 值                                                                                                     |
| `avatar-shape` | String           | ''       | 头像形状，可选值为 `'circle'`（圆形）或 `'square'`（方形）                                                                       |
| `avatar-icon` | String            | ''       | 头像图标，优先级高于 `avatar`，支持传入图标名称如 `'user'`                                                                        |
| `avatar-src-set` | String          | ''       | 设置头像图片的 srcset 属性                                                                                                        |
| `avatar-alt`  | String            | ''       | 设置头像图片的 alt 属性                                                                                                           |
| `avatar-fit`  | String            | 'cover'  | 设置头像图片的 `object-fit` 属性，可选值：`'cover'`、`'contain'`、`'fill'`、`'none'`、`'scale-down'`                             |

## 事件

| 事件名       | 参数            | 类型     | 说明                     |
| ------------ | --------------- | -------- | ------------------------ |
| `@start`     | `ref` 实例      | Function | 打字机效果开始时触发     |
| `@finish`    | `ref` 实例      | Function | 打字机效果完成时触发     |
| `@writing`   | `ref` 实例      | Function | 打字机打字过程中实时触发 |
| `@avatarError` | `ref` 实例     | Function | 头像加载失败时触发       |

## Ref 实例方法

| 方法名          | 类型     | 说明                       |
| --------------- | -------- | --------------------------- |
| `interrupt`     | Function | 中断打字机                 |
| `continue`      | Function | 继续未完成的打字机         |
| `restart`       | Function | 重新开始打字机             |
| `destroy`       | Function | 主动销毁 Bubble 组件       |
| `renderedContent` | String   | 获取打字机组件的渲染内容   |
| `isTyping`      | Boolean  | 是否正在打字机             |
| `progress`      | Number   | 打字机进度，范围 0-100     |

## 插槽

| 插槽名      | 参数 | 类型 | 说明                 |
| ----------- | ---- | ---- | -------------------- |
| `#avatar`   | -    | Slot | 自定义头像显示内容   |
| `#header`   | -    | Slot | 自定义气泡顶部显示内容 |
| `#content`  | -    | Slot | 自定义气泡显示内容   |
| `#loading`  | -    | Slot | 自定义气泡加载状态显示内容 |
| `#footer`   | -    | Slot | 自定义气泡底部显示内容 |

## 特性

1. **布局方向** - 支持左对齐（`start`）和右对齐（`end`）
2. **内容类型** - 支持纯文本、Markdown、自定义插槽内容
3. **加载状态** - 内置加载动画，支持自定义加载内容
4. **视觉效果** - 提供多种形状和变体（圆角/直角、实心/轮廓/阴影等）
5. **打字机动画** - 支持文本逐字输出的打字机效果
6. **灵活插槽** - 提供头像、头部、内容、底部、加载状态等插槽
