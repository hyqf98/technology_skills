---
title: 提及发送器组件
---

## 简介

`MentionSender` 是用于聊天场景的输入框组件。

::: warning
其功能与 `Sender` 组件基本相同。唯一的区别是**与指令弹窗相关的属性和方法**。点击此处快速了解差异 👉 [**指令差异**](https://element-plus-x.com/components/mentionSender/#packages-vue-element-plus-x-src-mentionSender-demos-options)

目前，我们不计划将 `MentionSender` 和 `Sender` 的**指令功能**合并为一个，仅通过组件来区分。

## 代码示例

### 基础用法

<demo src="./demos/basic.vue"></demo>

### 占位符

<demo src="./demos/placeholder.vue"></demo>

### 双向绑定（未绑定，value 不会改变）

<demo src="./demos/v-model.vue"></demo>

### 提交按钮禁用状态

<demo src="./demos/submit-btn-disabled.vue"></demo>

### 自定义最大和最小行数

<demo src="./demos/autosize.vue"></demo>

### 输入组件的各种状态

<demo src="./demos/state.vue"></demo>

### 提交方式

<demo src="./demos/submit-type.vue"></demo>

### 粘贴文件

<demo src="./demos/pasteFile.vue"></demo>

### 语音识别

::: warning
内置浏览器语音识别 API。您可以使用 [`useRecord`](https://element-plus-x.com/components/useRecord/) **hook** 更方便地集成和控制内置语音识别。

<demo src="./demos/allow-speech.vue"></demo>

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

<demo src="./demos/input-style.vue"></demo>

### 输入焦点控制

<demo src="./demos/focus.vue"></demo>

## 提及指令用法（与 Sender 组件的区别）

::: warning
以下是**与 Sender 组件不同的**关于指令的**属性和方法**。请注意**使用差异**。

**💌 如果您需要在内容中间触发提及指令列表，可以使用此组件。**

此温馨提示最后更新时间：`2025-04-16`
:::

### 自定义触发指令数组

<demo src="./demos/options.vue"></demo>

### 自定义触发指令字符串

<demo src="./demos/trigger-strings.vue"></demo>

### 自定义触发指令分隔符

<demo src="./demos/trigger-split.vue"></demo>

### 触发指令加载

<demo src="./demos/trigger-loading.vue"></demo>

### 自定义触发指令过滤器

<demo src="./demos/filter-option.vue"></demo>

### 整体删除

<demo src="./demos/whole.vue"></demo>

### 触发指令弹窗位置

<demo src="./demos/trigger-popover-placement.vue"></demo>

### 触发指令弹窗偏移

<demo src="./demos/trigger-popover-offset.vue"></demo>

### 弹窗插槽

<demo src="./demos/solts.vue"></demo>

### 搜索事件

<demo src="./demos/search.vue"></demo>

### 选择事件

<demo src="./demos/select.vue"></demo>

## 属性

| 属性名                    | 类型                 | 是否必填 | 默认值                        | 说明                                                                                                                                                                                                                                                  |
| ------------------------- | -------------------- | -------- | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `v-model`                 | String               | 否       | ''                             | 输入框的绑定值，使用 `v-model` 进行双向绑定                                                                                                                                                                                                          |
| `placeholder`             | String               | 否       | ''                             | 输入框的占位符文本                                                                                                                                                                                                                                    |
| `auto-size`               | Object               | 否       | \{ minRows:1, maxRows:6 \}     | 设置输入框的最小和最大可见行数                                                                                                                                                                                                                        |
| `read-only`               | Boolean              | 否       | false                          | 输入框是否只读                                                                                                                                                                                                                                        |
| `disabled`                | Boolean              | 否       | false                          | 输入框是否禁用                                                                                                                                                                                                                                        |
| `submitBtnDisabled`       | Boolean \| undefined | 否       | undefined                      | 禁用内置发送按钮（注意使用场景）                                                                                                                                                                                                                      |
| `loading`                 | Boolean              | 否       | false                          | 是否显示加载状态，当为 `true` 时，输入框将显示加载动画                                                                                                                                                                                                |
| `clearable`               | Boolean              | 否       | false                          | 输入框是否可清空，显示默认清空按钮                                                                                                                                                                                                                    |
| `allowSpeech`             | Boolean              | 否       | false                          | 是否允许语音输入，默认显示内置语音识别按钮，使用浏览器内置语音识别 API                                                                                                                                                                                 |
| `submitType`              | String               | 否       | 'enter'                        | 提交方式。支持 `'shiftEnter'`（`Shift + Enter` 提交）、`'cmdOrCtrlEnter'`（`Command + Enter` 或 `Ctrl + Enter` 提交）、`'altEnter'`（`Alt + Enter` 提交）                                                                                                  |
| `headerAnimationTimer`    | Number               | 否       | 300                            | 自定义头部显示持续时间，单位毫秒                                                                                                                                                                                                                      |
| `inputWidth`              | String               | 否       | '100%'                         | 输入框宽度                                                                                                                                                                                                                                            |
| `variant`                 | String               | 否       | 'default'                      | 输入框变体类型，支持 `'default'`、`'updown'`                                                                                                                                                                                                         |
| `showUpdown`              | Boolean              | 否       | true                           | 变体为 `updown` 时是否显示内置样式                                                                                                                                                                                                                   |
| `inputStyle`              | Object               | 否       | \{}                            | 输入框样式                                                                                                                                                                                                                                           |
| `triggerStrings`          | string[]             | 否       | []                             | 触发指令的字符串数组                                                                                                                                                                                                                                  |
| `triggerPopoverVisible`   | Boolean              | 否       | false                          | 触发指令的弹窗是否可见，使用 `v-model:triggerPopoverVisible` 控制                                                                                                                                                                                     |
| `triggerPopoverWidth`     | String               | 否       | 'fit-content'                  | 触发指令弹窗的宽度，支持百分比和其他 CSS 单位                                                                                                                                                                                                         |
| `triggerPopoverLeft`      | String               | 否       | '0px'                          | 触发指令弹窗的左边距，支持百分比和其他 CSS 单位                                                                                                                                                                                                       |
| `triggerPopoverOffset`    | Number               | 否       | 8                              | 触发指令弹窗的偏移量，必须是数字，单位为 px                                                                                                                                                                                                          |
| `triggerPopoverPlacement` | String               | 否       | 'top-start'                    | 触发指令弹窗的位置，可选值：`'top'` \| `'top-start'` \| `'top-end'` \| `'bottom'` \| `'bottom-start'` \| `'bottom-end'` \| `'left'` \| `'left-start'` \| `'left-end'` \| `'right'` \| `'right-start'` \| `'right-end'` |

## 事件

| 事件名           | 说明                                                   | 回调参数                                                         |
| ---------------- | ------------------------------------------------------ | ---------------------------------------------------------------- |
| `submit`         | 内置提交按钮点击时触发                                   | 无                                                               |
| `cancel`         | 内置加载按钮点击时触发                                   | 无                                                               |
| `recordingChange` | 内置语音识别状态变化时触发                              | 无                                                               |
| `select`         | 触发字段按下时触发                                      | `option: MentionOption`                                          |
| `search`         | 用户选择选项时触发                                      | `searchValue: string, prefix: string`                             |
| `pasteFile`      | 粘贴文件时触发                                          | `interface PasteFileEvent{firstFile: File; fileList: FileList}`  |

## Ref 实例方法

| 方法名            | 类型     | 说明                                                                                                                                                                                        |
| ----------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `openHeader`      | Function | 打开输入框的自定义头部                                                                                                                                                                       |
| `closeHeader`     | Function | 关闭输入框的自定义头部                                                                                                                                                                       |
| `clear`           | Function | 清空输入框的内容                                                                                                                                                                             |
| `blur`            | Function | 移除输入框的焦点                                                                                                                                                                             |
| `focus`           | Function | 聚焦输入框，默认 `focus('all')` 选中所有文本，`focus('start')` 聚焦到开头，`focus('end')` 聚焦到结尾                                                                                             |
| `submit`          | Function | 提交输入内容                                                                                                                                                                                 |
| `cancel`          | Function | 取消加载状态                                                                                                                                                                                 |
| `startRecognition` | Function | 开始语音识别                                                                                                                                                                                 |
| `stopRecognition`  | Function | 停止语音识别                                                                                                                                                                                 |
| `popoverVisible`   | Boolean  | 触发指令弹窗的可见性                                                                                                                                                                          |
| `inputInstance`    | Object   | 输入框实例                                                                                                                                                                                   |

## 插槽

| 插槽名            | 参数                          | 类型 | 说明                       |
| ----------------- | ----------------------------- | ---- | -------------------------- |
| `#header`         | -                             | Slot | 用于自定义头部内容         |
| `#prefix`         | -                             | Slot | 用于自定义前缀内容         |
| `#action-list`    | -                             | Slot | 用于自定义操作列表         |
| `#footer`         | -                             | Slot | 用于自定义底部内容         |
| `#trigger-label`  | `#trigger-label={ item, index }` | Slot | 用于自定义弹窗标签         |
| `#trigger-loading` | -                             | Slot | 用于自定义弹窗加载状态     |
| `#trigger-header` | -                             | Slot | 用于自定义弹窗头部         |
| `#trigger-footer` | -                             | Slot | 用于自定义弹窗底部         |

## 特性

1. **焦点控制**：支持将焦点设置到开头、结尾或选中所有文本，也可以移除焦点
2. **自定义内容**：提供头部、前缀和操作列表的插槽，允许用户自定义这些部分
3. **提交功能**：支持使用 `Shift + Enter` 提交输入，并允许提交后执行自定义操作
4. **加载状态**：可以显示加载状态以模拟提交过程
5. **语音输入**：支持语音输入，更加便捷地输入内容
6. **清空功能**：可以清空输入框，方便重新输入
