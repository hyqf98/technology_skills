---
title: 欢迎组件
---

## 简介

`Welcome` 组件可以清晰地传达给用户可实现意图的范围和预期功能。使用合适的欢迎推荐组件可以有效降低用户学习成本，帮助用户快速理解并顺利上手。

## 代码示例

### 基础用法

<demo src="./demos/base.vue"></demo>

### 样式变体

<demo src="./demos/variant.vue"></demo>

### 背景颜色

<demo src="./demos/bg.vue"></demo>

### 自定义图片

<demo src="./demos/image.vue"></demo>

### 自定义副标题

<demo src="./demos/extra.vue"></demo>

## 属性

| 属性名         | 类型   | 是否必填 | 默认值  | 说明                                              |
| -------------- | ------ | -------- | ------- | ------------------------------------------------- |
| `variant`      | string | 否       | filled  | 组件样式变体（filled/borderless）                 |
| `direction`    | string | 否       | ltr     | 文本方向（ltr/rtl）                               |
| `icon`         | string | 否       | -       | 图标 URL 地址                                      |
| `title`        | string | 否       | -       | 主标题文本内容                                     |
| `extra`        | string | 否       | -       | 副标题文本内容                                     |
| `description`  | string | 否       | -       | 描述文本内容                                       |
| `className`    | string | 否       | -       | 外层容器的自定义类名                               |
| `rootClassName`| string | 否       | -       | 根节点的自定义类名                                 |
| `classNames`   | object | 否       | -       | 各部分的自定义类名（{ icon, title, extra, description }） |
| `style`        | object | 否       | -       | 外层容器的自定义样式                               |
| `styles`       | object | 否       | -       | 各部分的自定义样式（{ icon, title, extra, description }） |
| `prefixCls`    | string | 否       | welcome | 组件类名前缀                                       |

## 插槽

| 插槽名    | 参数 | 类型 | 说明                   |
| --------- | ---- | ---- | ---------------------- |
| `#image`  | -    | Slot | 自定义欢迎图片内容     |
| `#extra`  | -    | Slot | 自定义副标题内容       |

## 特性

1. 目前通过 `variant` 属性支持 `filled`（实心）和 `borderless`（无边框）两种视觉样式
2. 支持 `direction` 属性控制文本方向
3. 可通过 `classNames` 和 `styles` 控制样式，实现细粒度定制
4. 支持 `image` 和 `extra` 插槽扩展自定义内容
