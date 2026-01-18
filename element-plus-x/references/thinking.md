---
title: 思考组件
---

## 简介

`Thinking` 是一个用于显示思考状态的组件，支持**状态管理**、**内容展开/收起**和**自定义样式**。通过不同状态（开始/思考中/结束/错误/取消）的视觉反馈，帮助用户直观理解 AI 的思考过程。组件内置过渡动画并提供灵活的扩展插槽，适用于智能对话、数据分析等场景。

::: info

该组件可以与 `BubbleList` 等组件配合使用，实现更丰富的交互体验。

:::

## 代码示例

### 基础用法

<demo src="./demos/base.vue"></demo>

### 内容展开/收起

<demo src="./demos/content.vue"></demo>

### 状态管理

<demo src="./demos/v-model.vue"></demo>

### 状态样式

<demo src="./demos/status.vue"></demo>

### 自动收起

<demo src="./demos/autoCollapse.vue"></demo>

### 禁用状态

<demo src="./demos/disabled.vue"></demo>

### 宽度自定义

<demo src="./demos/width.vue"></demo>

### 内容颜色样式自定义

<demo src="./demos/color.vue"></demo>

### 插槽自定义

<demo src="./demos/solt.vue"></demo>

## 属性

| 属性名          | 类型           | 是否必填 | 默认值                  | 说明                                                                                                              |
| --------------- | -------------- | -------- | ----------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `content`       | String         | 否       | `''`                    | 要显示的主要内容文本，无打字机效果，由接口响应决定                                                                  |
| `modelValue`    | Boolean        | 否       | `true`                  | 通过 v-model 绑定的展开状态，默认展开                                                                              |
| `status`        | ThinkingStatus | 否       | `'start'`               | 组件状态：`start`（开始）/ `thinking`（思考中）/ `end`（完成）/ `error`（错误）/ `cancel`（取消）                  |
| `autoCollapse`  | Boolean        | 否       | `false`                 | 当组件状态变为 `end` 时是否自动收起内容区域                                                                        |
| `disabled`      | Boolean        | 否       | `false`                 | 是否禁用组件交互                                                                                                   |
| `buttonWidth`   | String         | 否       | `'160px'`               | 触发按钮宽度                                                                                                       |
| `duration`      | String         | 否       | `'0.2s'`                | 过渡动画持续时间                                                                                                   |
| `maxWidth`      | String         | 否       | `'500px'`               | 内容区域最大宽度                                                                                                   |
| `backgroundColor` | String         | 否       | `'#fcfcfc'`             | 内容区域背景颜色                                                                                                   |
| `color`         | String         | 否       | `'var(--el-color-info)'`| 内容文本颜色                                                                                                       |

## 事件

| 事件名    | 参数                              | 类型     | 说明                                   |
| --------- | --------------------------------- | -------- | -------------------------------------- |
| `@change` | \{value:boolean,status:ThinkingStatus\} | Function | 展开状态或状态变化时触发                |

## 插槽

| 插槽名          | 参数    | 类型 | 说明                       |
| --------------- | ------- | ---- | -------------------------- |
| `#status-icon` | \{ status \} | Slot | 自定义状态图标              |
| `#label`       | \{ status \} | Slot | 自定义按钮文字              |
| `#arrow`       | -              | Slot | 自定义箭头图标              |
| `#content`     | \{ content \} | Slot | 自定义内容区域（非错误状态） |
| `#error`       | -              | Slot | 自定义错误消息内容显示      |

## 特性

1. **多状态管理**
   - 支持五种状态：`start`/`thinking`/`end`/`error`/`cancel`，自动切换对应的图标和文字
   - 错误状态强制显示固定错误提示

2. **交互反馈**
   - 展开/收起内容区域时平滑滑动动画
   - 按钮点击反馈支持自定义过渡效果

3. **样式定制**
   - 通过 CSS 变量控制尺寸、颜色等视觉属性
   - 提供完整的插槽扩展能力，支持自定义图标和内容

4. **智能行为**
   - 状态变化时自动调整展开状态
   - 禁用状态下保持视觉反馈但阻止交互
