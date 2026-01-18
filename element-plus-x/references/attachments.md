---
title: 附件组件
---

## 简介

`Attachments` 是一个功能丰富的附件管理组件，支持文件列表展示、上传、拖拽交互、滚动浏览等功能。适用于需要处理多个文件上传和展示的场景（如表单附件、文件管理界面）。组件内置文件上传按钮、拖拽提示区域，并提供灵活的自定义插槽和样式配置。

## 代码示例

### 基础用法

<demo src="./demos/base.vue"></demo>

### 滚动模式

<demo src="./demos/scroll-mode.vue"></demo>

### 自定义文件列表

<demo src="./demos/custom-list.vue"></demo>

### 拖拽上传

<demo src="./demos/drag-upload.vue"></demo>

### 自定义滚动按钮

<demo src="./demos/custom-scroll-buttons.vue"></demo>

## 属性

| 属性名         | 类型                                         | 是否必填 | 默认值       | 说明                                                                                     |
| -------------- | -------------------------------------------- | -------- | ------------ | ---------------------------------------------------------------------------------------- |
| `items`        | `FilesCardProps[]`                           | 否       | `[]`         | 文件列表数据（包含文件名称、类型、状态等基本信息）                                        |
| `overflow`     | `'scrollX' \| 'scrollY' \| 'wrap'`           | 否       | `'scrollX'`  | 滚动布局模式（水平滚动/垂直滚动/自动换行）                                                |
| `listStyle`    | `CSSProperties`                              | 否       | `{}`         | 列表容器自定义样式                                                                       |
| `uploadIconSize` | `string`                                     | 否       | `'64px'`     | 上传按钮图标尺寸                                                                         |
| `dragTarget`   | `string \| Ref<HTMLElement> \| null`         | 否       | `null`       | 拖拽目标元素（支持选择器字符串或 DOM 引用，默认为组件自身）                               |
| `hideUpload`   | `boolean`                                    | 否       | `false`      | 是否隐藏默认上传按钮                                                                     |
| `limit`        | `number`                                     | 否       | `undefined`  | 文件数量限制（超过后隐藏上传按钮）                                                       |
| `beforeUpload` | `(file: File) => boolean`                    | 否       | `undefined`  | 上传前验证函数（返回 `false` 可阻止上传）                                                |
| `httpRequest`  | `(options: { file: File }) => Promise<void>` | 否       | `undefined`  | 自定义上传请求函数（必须返回 Promise）                                                   |

## 插槽

| 插槽名           | 插槽参数                                            | 说明                                                                 |
| ---------------- | --------------------------------------------------- | -------------------------------------------------------------------- |
| `#file-list`     | `{ items: FilesCardProps[] }`                        | 自定义文件列表内容（覆盖默认的 `FilesCard` 展示）                     |
| `#prev-button`   | `{ show: boolean, onScrollLeft: () => void }`        | 自定义左侧滚动按钮（`scrollX` 模式），`show` 控制显示状态             |
| `#next-button`   | `{ show: boolean, onScrollRight: () => void }`       | 自定义右侧滚动按钮（`scrollX` 模式），`show` 控制显示状态             |
| `#empty-upload`  | `-`                                                 | 文件列表为空时的自定义上传区域（默认显示加号上传按钮）                |
| `#no-empty-upload` | `-`                                                 | 文件列表不为空时的自定义上传占位符（默认显示加号上传按钮）           |
| `#drop-area`     | `-`                                                 | 拖拽上传时的自定义遮罩层内容（默认显示上传图标和文字）                |

## 事件

| 事件名        | 回调参数                                            | 说明                                                      |
| ------------- | --------------------------------------------------- | --------------------------------------------------------- |
| `uploadChange`  | `(file: File, fileList: FileListProps)`              | 文件选择变化时触发（包含选中的文件和当前文件列表）         |
| `uploadSuccess` | `(response: any, file: File, fileList: FileListProps)` | 文件上传成功时触发（返回响应、当前文件和文件列表）         |
| `uploadError`   | `(error: any, file: File, fileList: FileListProps)`  | 文件上传失败时触发（返回错误、当前文件和文件列表）         |
| `uploadDrop`    | `(files: File[], props: FileListProps)`              | 文件拖入时触发（包含拖入的文件数组和组件属性）             |
| `deleteCard`    | `(item: FilesCardProps, index: number)`              | 文件卡片删除按钮点击时触发（返回被删除的文件信息和索引）   |

## 支持 el-upload 属性

组件内部使用了 **elementplus** 的 `el-upload` 组件，因此支持其大部分上传属性，如：`httpRequest`、`beforeUpload` 等。详情请参考：[element-plus/upload](https://element-plus.org/zh-CN/component/upload.html)

## 特性

1. **多种布局模式**：支持 `scrollX`（水平滚动）、`scrollY`（垂直滚动）和 `wrap`（自动换行）布局，适应不同屏幕空间和文件数量。
2. **拖拽上传交互**：内置拖拽目标区域（可通过 `dragTarget` 自定义），拖拽时显示半透明遮罩提示，支持文件夹过滤和文件类型验证。
3. **高度可定制**：通过 `#file-list` 插槽完全自定义文件列表展示（如替换为自定义卡片组件），支持自定义滚动按钮和上传按钮样式。
4. **文件状态管理**：配合 `FilesCard` 组件，支持上传中（进度条）、已完成、失败等视觉状态，自动同步文件列表更新。
