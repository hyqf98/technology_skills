---
title: 文件卡片组件
---

## 简介

`FilesCard` 是一个灵活的文件展示组件，支持多种文件类型（图片、文档、压缩包等）的可视化呈现，包括文件图标、名称、描述、状态等信息。提供丰富的自定义选项和交互功能，适用于文件管理、上传预览等场景。

## 代码示例

### 基础用法

<demo src="./demos/base.vue"></demo>

### 状态设置

<demo src="./demos/status.vue"></demo>

### 显示删除图标

<demo src="./demos/delete-icon.vue"></demo>

### 图片文件显示

<demo src="./demos/image-preview.vue"></demo>

### 自定义样式和交互

<demo src="./demos/custom-style.vue"></demo>

### 自定义内置文件颜色

<demo src="./demos/custom-color.vue"></demo>

## 属性

| 属性名         | 类型                               | 是否必填 | 默认值             | 说明                                                                              |
| -------------- | ---------------------------------- | -------- | ------------------ | --------------------------------------------------------------------------------- |
| `uid`          | `string \| number`                 | 是       | -                  | 文件唯一标识符                                                                    |
| `name`         | `string`                           | 否       | `undefined`        | 文件名称（支持自动解析后缀以匹配图标）                                             |
| `fileSize`     | `number`                           | 否       | `undefined`        | 文件大小（单位：字节，自动转换为可读格式）                                         |
| `fileType`     | `string`                           | 否       | `undefined`        | 文件类型（优先级高于 `name` 后缀解析，如 `'image'`、`'document'`）                 |
| `description`  | `string`                           | 否       | `undefined`        | 描述文本（支持动态生成文件类型和大小信息）                                        |
| `url`          | `string`                           | 否       | `undefined`        | 文件访问 URL（图片文件可用于预览）                                                |
| `thumbUrl`     | `string`                           | 否       | `undefined`        | 图片缩略图 URL                                                                    |
| `imgFile`      | `File \| Blob`                     | 否       | `undefined`        | 图片文件流（自动解析为预览 URL，仅用于上传前的临时显示）                          |
| `iconSize`     | `string`                           | 否       | `'42px'`           | 图标/图片尺寸                                                                     |
| `iconColor`    | `string`                           | 否       | `undefined`        | 非图片文件的图标颜色（支持自定义颜色值）                                          |
| `showDelIcon`  | `boolean`                          | 否       | `false`            | 是否显示悬停时的删除图标                                                          |
| `maxWidth`     | `string`                           | 否       | `'236px'`          | 卡片最大宽度                                                                      |
| `style`        | `CSSProperties`                    | 否       | `undefined`        | 卡片自定义样式                                                                    |
| `hoverStyle`   | `CSSProperties`                    | 否       | `undefined`        | 卡片悬停时的自定义样式                                                            |
| `imgVariant`   | `'rectangle' \| 'square'`          | 否       | `'rectangle'`      | 图片卡片形态（矩形/方形）                                                         |
| `imgPreview`   | `boolean`                          | 否       | `true`             | 是否启用图片预览功能                                                              |
| `imgPreviewMask` | `boolean`                         | 否       | `true`             | 是否显示图片预览遮罩层                                                            |
| `status`       | `'uploading' \| 'done' \| 'error'` | 否       | `undefined`        | 文件状态（控制进度条、错误提示等视觉反馈）                                         |
| `percent`      | `number`                           | 否       | `0`                | 上传进度百分比（与 `status="uploading"` 配合使用）                                |
| `errorTip`     | `string`                           | 否       | `'上传失败'`       | 自定义错误状态提示文本                                                            |

## 插槽

| 插槽名                | 插槽参数                                    | 说明                                                     |
| --------------------- | ------------------------------------------- | -------------------------------------------------------- |
| `#icon`               | `{ item: FilesCardProps }`                  | 自定义图标区域（优先级高于自动解析的内置图标）            |
| `#content`            | `{ item: FilesCardProps }`                  | 自定义内容区域（覆盖默认的名称和描述显示）                |
| `#name-prefix`        | `{ item: FilesCardProps, prefix, suffix }`  | 自定义文件名前缀（用于截断显示场景）                      |
| `#name-suffix`        | `{ item: FilesCardProps, prefix, suffix }`  | 自定义文件名后缀（用于截断显示场景）                      |
| `#description`        | `{ item: FilesCardProps, prefix, suffix }`  | 自定义描述文本（覆盖默认生成的描述）                      |
| `#image-preview-actions` | `{ item: FilesCardProps, prefix, suffix }` | 图片预览遮罩内容（悬停时显示，需要 `imgPreviewMask`）     |
| `#del-icon`           | `{ item: FilesCardProps }`                  | 自定义删除图标（默认使用 Element Plus 的 `CircleCloseFilled` 图标） |

## 事件

| 事件名        | 回调参数      | 说明                                       |
| ------------- | ------------- | ------------------------------------------ |
| `delete`      | `{ ...props }` | 删除按钮点击时触发，传递当前卡片的完整属性  |
| `image-preview` | `{ ...props }` | 图片预览功能调用时触发（点击图片或遮罩层） |

## 特性

1. **自动文件类型识别**：根据文件名后缀自动匹配内置图标（支持 `.pdf`、`.png`、`.zip` 等常见格式），未匹配时显示通用文件图标。
2. **多状态可视化**：支持 `uploading`（上传中带进度条）、`done`（完成）、`error`（失败带自定义提示）三种状态，状态样式自动切换。
3. **增强的图片文件支持**：支持图片预览功能（基于 Element Plus 图片预览组件），提供方形/矩形变体，支持通过 `imgFile` 直接解析本地图片文件流。
4. **高度可定制**：自定义图标颜色、尺寸、卡片样式和悬停效果，通过插槽灵活扩展内容（如文件名截断显示、状态遮罩层）。
5. **响应式设计**：通过 `maxWidth` 控制卡片最大宽度，适应不同布局场景，文件描述自动截断防止溢出。
