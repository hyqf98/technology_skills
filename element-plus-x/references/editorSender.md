---
title: 编辑器发送器组件
---

## 简介

**`EditorSender`** 来了！🙊 专为**多模态模型**和**自定义提示词场景**设计，该输入框组件解决了**标签插入、内容提及、自定义提示词输入**等核心开发需求，更好地展示多模态功能的能力。

::: warning
`EditorSender` 组件与 `Sender` 组件存在一定的开发差异。请根据实际需求选择使用。

<img src="./image.png" width="100%">
:::

## 代码演示

### 基础用法

<demo src="./demos/basic.vue"></demo>

### 占位符文本

<demo src="./demos/placeholder.vue"></demo>

### 自动聚焦

<demo src="./demos/autoFocus.vue"></demo>

### 状态属性

<demo src="./demos/state.vue"></demo>

### 变体 - 垂直样式

<demo src="./demos/variant.vue"></demo>

### 自定义操作列表

<demo src="./demos/action-list.vue"></demo>

### 自定义前缀

<demo src="./demos/prefix.vue"></demo>

### 自定义头部

<demo src="./demos/header.vue"></demo>

### 自定义底部

<demo src="./demos/footer.vue"></demo>

### 自定义输入框样式

<demo src="./demos/custom-style.vue"></demo>

### 最大输入长度

<demo src="./demos/max-length.vue"></demo>

### 提交方式

<demo src="./demos/submit-type.vue"></demo>

### 粘贴文件

<demo src="./demos/pasteFile.vue"></demo>

## 高级用法

### 插入文本内容

<demo src="./demos/insert-text.vue"></demo>

### 插入 HTML 内容

<demo src="./demos/insert-html.vue"></demo>

### 插入选择标签

<demo src="./demos/insert-select-tag.vue"></demo>

### 插入输入标签

<demo src="./demos/insert-input-tag.vue"></demo>

### 插入用户标签

<demo src="./demos/insert-user-tag.vue"></demo>

### 插入自定义标签

<demo src="./demos/insert-custom-tag.vue"></demo>

### 混合标签覆盖写法

<demo src="./demos/mix-tag.vue"></demo>

### 前缀提示标签

<demo src="./demos/prefix-tag.vue"></demo>

## 属性

| 属性名                 | 类型                | 是否必填 | 默认值         | 说明                                                                                                      |
| ---------------------- | ------------------- | -------- | -------------- | --------------------------------------------------------------------------------------------------------- |
| `placeholder`          | String              | false    | '请输入内容'    | 输入框的占位符文本                                                                                         |
| `device`               | 'pc' \| 'h5' \| 'h5' | false    | 'auto'         | 编辑器的设备类型                                                                                           |
| `autoFocus`            | Boolean             | false    | false          | 组件挂载后是否自动聚焦输入框                                                                              |
| `variant`              | 'default' \| 'updown' | false  | 'default'      | 输入框变体类型："default" 为水平布局，"updown" 为垂直布局                                                  |
| `mentionConfig`        | MentionConfig       | false    | undefined      | @提及功能的配置项                                                                                          |
| `triggerConfig`        | TriggerConfig[]     | false    | undefined      | 扩展触发自定义弹窗的配置项                                                                                 |
| `selectConfig`         | SelectConfig[]      | false    | undefined      | 标签下拉选择的配置项                                                                                       |
| `maxLength`            | Number              | false    | -1             | 限制输入框的最大字符数，会产生较大的性能开销，除非必要，不建议设置                                           |
| `submitType`           | 'enter' \| 'shiftEnter' | false  | 'enter'        | 控制换行和提交模式                                                                                         |
| `customStyle`          | Record<string, any> | false    | {}             | 用于修改输入框的样式                                                                                       |
| `loading`              | Boolean             | false    | false          | 发送按钮的加载状态，设置为 true 时显示加载动画                                                             |
| `disabled`             | Boolean             | false    | false          | 是否禁用输入框，禁用后不允许输入和交互                                                                     |
| `clearable`            | Boolean             | false    | false          | 是否显示清空按钮                                                                                           |
| `headerAnimationTimer` | Number              | false    | 300            | 头部展开/收起动画的持续时间，单位毫秒（ms）                                                                 |
| `tipConfig`            | Boolean \| TipConfig | false    | true           | 启用前缀标签或自定义模板                                                                                   |
| `getPlugin`            | () => typeof XSender | false  | undefined      | 自定义基础库依赖的更新                                                                                     |

## 事件

| 事件名       | 说明                                             | 回调参数                                                         |
| ------------ | ------------------------------------------------- | ---------------------------------------------------------------- |
| `submit`     | 提交内容时触发                                     | null                                                             |
| `change`     | 输入内容变化时触发                                 | null                                                             |
| `cancel`     | 取消加载状态时触发                                 | null                                                             |
| `pasteFile`  | 粘贴文件时触发的事件                               | `interface PasteFileEvent{firstFile: File; fileList: FileList}`  |

## Ref 方法

| 方法名           | 类型                              | 说明                                                      |
| ---------------- | --------------------------------- | --------------------------------------------------------- |
| `getSender`      | () => XSender                     | 获取当前发送器的 XSender 实例对象                          |
| `focus`          | (type: 'first' \| 'last' \| 'mark') => void | 将光标聚焦到文本的指定位置                          |
| `blur`           | () => void                        | 移除输入框的焦点                                           |
| `selectAll`      | () => void                        | 选中输入框的所有内容                                       |
| `clear`          | () => void                        | 清空输入框的内容                                           |
| `setSelect`      | (key: string, id: string) => void | 插入一个选择标签                                           |
| `setInput`       | (key: string, placeholder: string, defaultValue?: string) => void | 插入一个输入标签                          |
| `setMention`     | (id: string) => void              | 插入一个提及标签                                           |
| `setTrigger`     | (key: string, id: string) => void | 插入一个自定义触发标签                                     |
| `setChatNode`    | (model: ChatNode[][]) => void     | 在当前光标位置插入多个混合类型的标签                       |
| `setHtml`        | (html: string) => void            | 在当前光标位置插入 HTML 片段（建议使用 inline 或 inline-block 元素） |
| `setText`        | (txt: string) => void             | 在当前光标位置插入文本                                      |
| `showSelect`     | (key: string, elm: HTMLElement) => void | 调用方法在外部显示标签选择弹窗                          |
| `showTip`        | (props: Record<string, string>) => void | 打开前缀提示标签                                      |
| `closeTip`       | () => void                        | 关闭前缀提示标签                                           |
| `senderState`    | SenderState                       | 暴露组件状态对象，用于判断内容是否为空、是否启用前缀提示  |

## 插槽

| 插槽名          | 参数 | 说明                       |
| --------------- | ---- | -------------------------- |
| `#header`       | -    | 用于自定义输入框头部内容   |
| `#prefix`       | -    | 用于自定义输入框前缀内容   |
| `#action-list`  | -    | 用于自定义输入框操作列表内容 |
| `#footer`       | -    | 用于自定义输入框底部内容   |

## 更多自定义功能
1. **自定义交互弹窗**：请参考 [CustomDialog](https://jianfv.top/XSender/guide/customDialog)
2. **自定义交互组件**：请参考 [CustomComponent](https://jianfv.top/XSender/guide/customComponent)

## 功能特性
1. **全类型标签引擎**：无缝支持 @用户、选择标签、自定义标签等多种标签类型。标签插入/更新/管理轻松自如，满足复杂内容标注需求。
2. **跨设备自适应交互**：PC 内置弹窗系统，H5 支持自定义弹窗。自动适配不同设备操作习惯，兼顾原生体验与定制自由度。
