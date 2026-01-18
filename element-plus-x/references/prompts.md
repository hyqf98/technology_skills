---
title: 提示词组件
---

## 简介

`Prompts` 用于显示一组与当前上下文相关的预定义问题或建议。

## 代码示例

### 基础用法

<demo src="./demos/base.vue"></demo>

### 禁用状态

<demo src="./demos/disabled.vue"></demo>

### 垂直布局

<demo src="./demos/vertical.vue"></demo>

### 换行

<demo src="./demos/wrap.vue"></demo>

### 响应式宽度

<demo src="./demos/responsive.vue"></demo>

### 自定义样式

<demo src="./demos/customized.vue"></demo>

### 嵌套组合

<demo src="./demos/nested.vue"></demo>

## 属性

| 属性名     | 类型                  | 是否必填 | 默认值  | 说明                                                                               |
| ---------- | --------------------- | -------- | ------- | ---------------------------------------------------------------------------------- |
| `title`    | `string`              | 否       | `''`    | 提示词组的主标题文本内容                                                           |
| `items`    | `PromptsItemsProps[]` | 否       | `[]`    | 提示词项数组，每个元素包含 label、icon、description 等信息（详见下方结构详情）      |
| `wrap`     | `boolean`             | 否       | `false` | 是否允许提示词项自动换行（仅在水平布局时生效）                                      |
| `vertical` | `boolean`             | 否       | `false` | 是否垂直排列提示词项（垂直模式下布局方向为列）                                     |
| `style`    | `CSSProperties`       | 否       | `{}`    | 组件容器的自定义样式（直接应用到最外层 `div.el-prompts`）                          |

**`PromptsItemsProps` 结构详情**（单个提示词项的属性）：

```typescript
interface PromptsItemsProps {
  key: string | number; // 唯一标识符（用于状态关联）
  label?: string; // 提示词项的标签文本
  icon?: ComponentVNode; // 提示词项的图标（Vue 组件形式）
  description?: string; // 提示词项的描述文本
  disabled?: boolean; // 是否禁用（禁用时不响应交互）
  itemStyle?: CSSProperties; // 提示词项的自定义基础样式
  itemHoverStyle?: CSSProperties; // 提示词项悬停状态的自定义样式
  itemActiveStyle?: CSSProperties; // 提示词项激活状态的自定义样式
  children?: PromptsItemsProps[]; // 子提示词项数组（用于嵌套显示）
}
```

## 事件

| 事件名        | 参数                         | 类型     | 说明                       |
| ------------- | ---------------------------- | -------- | -------------------------- |
| `@itemClick`  | `(item: PromptsItemsProps)`  | Function | 提示词项点击时触发的事件   |

## 插槽

| 插槽名          | 参数                         | 类型    | 说明                                                                                                               |
| --------------- | ---------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------ |
| `#title`        | -                            | `Slot`  | 自定义提示词组标题内容（如果同时设置了 `title` 属性，插槽内容将覆盖属性文本）                                      |
| `#icon`         | `{ item: PromptsItemsProps }` | `Slot`  | 自定义提示词项的图标内容（接收当前提示词项 `item` 参数，可覆盖 `item.icon`）                                       |
| `#label`        | `{ item: PromptsItemsProps }` | `Slot`  | 自定义提示词项的标签内容（接收当前提示词项 `item` 参数，可覆盖 `item.label`）                                      |
| `#description`  | `{ item: PromptsItemsProps }` | `Slot`  | 自定义提示词项的描述内容（接收当前提示词项 `item` 参数，可覆盖 `item.description`）                                |

## 特性

1. **多维度内容展示**：通过 `items` 属性支持配置标签、图标、描述等基本信息，同时提供 `label`/`icon`/`description` 插槽实现高度定制化内容。
2. **灵活的布局控制**：通过 `vertical` 属性切换垂直/水平排列模式，通过 `wrap` 属性控制水平排列时的自动换行能力，适应不同场景的布局需求。
3. **交互状态反馈**：内置悬停（背景色变浅）和激活（背景色变深）状态样式，支持通过 `itemHoverStyle`/`itemActiveStyle` 自定义状态样式，增强交互体验。
4. **禁用状态支持**：通过 `item.disabled` 属性可单独禁用提示词项，禁用状态下不响应点击事件且背景色变灰，清晰标识不可操作状态。
5. **嵌套层级展示**：支持通过 `item.children` 配置子提示词项，组件自动递归渲染嵌套结构，满足多级分类或相关提示词展示需求。
6. **细粒度样式定制**：通过 `style` 属性控制整体组件样式，通过 `itemStyle` 控制单个提示词项基础样式，支持分别配置状态样式（`itemHoverStyle`/`itemActiveStyle`）。
