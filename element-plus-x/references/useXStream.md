---
title: useXStream Hook
---

## 简介

此 Hook 函数让用户更方便地控制**流式请求**。提供`发起请求`、`取消请求`，返回 `loading` 请求状态，返回 SSE 协议`实时数据流`，并返回请求`错误`信息。

::: info
目前仅测试了 SSE 和 SIP 协议请求，其他协议尚未测试。如果您有好的想法或发现，欢迎加入社区群 👨‍👩‍👧‍👧 **[社区群](https://element-plus-x.com/en/introduce.html#%F0%9F%91%A5-%E7%A4%BE%E5%8C%BA%E6%94%AF%E6%8C%81)**，联系我们，分享解决方案，提交 issues 和 PRs。请阅读 👉 **[开发指南](https://element-plus-x.com/en/guide/develop.html)** 了解提交规范。
:::

## 代码示例

<demo src="./demos/useSSE.vue"></demo>

<demo src="./demos/useSIP.vue"></demo>

::: warning
此 Hook 的解析规则与 ant-design-x 保持一致，全部在内部处理。**可放心切换使用**

sseEventPart
**`'event: message\ndata: {"id":"${i}","content":"${contentChunks[i]}"}\n\n'`**

```ts
// 默认流分隔符（使用两个换行符来分割流数据）
const DEFAULT_STREAM_SEPARATOR = '\n\n';
// 默认部分分隔符（使用单个换行符来打断当前数据）
const DEFAULT_PART_SEPARATOR = '\n';
// 默认键值对分隔符（使用冒号）
const DEFAULT_KV_SEPARATOR = ':';
```

:::

## 返回的 Hooks

| 属性         | 说明                         | 类型                                                  |
| ------------ | ---------------------------- | ----------------------------------------------------- |
| startStream  | 以流模式开始请求             | `({readableStream, transformStream}) => void`          |
| cancel       | 取消流式请求                 | `() => void`                                          |
| loading      | 是否正在加载流数据            | `boolean`                                             |
| data         | 实时流数据                   | `string`                                              |
| error        | 流式请求错误信息             | `string`                                              |
