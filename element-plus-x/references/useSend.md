---
title: useSend Hook
---

## XRequest 已弃用，推荐使用 hook-fetch (https://jsonlee12138.github.io/hook-fetch/)

## 背景介绍

基于 `ant-design-x` 的 `XRequest` 和 `XStream`，我们进行了深入学习和讨论。

在复刻了 `XStream` 之后，为了更通用的**控制请求数据**和**中止请求**场景，我们重构了 `ant-design-x` 的 `XRequest`，并将其拆分为**`前端中止场景`**和**`请求中止场景`**

这两种场景对应：

- hooks `useSend` -- 前端中止场景
- 工具类 `XRequest` -- 请求中止场景

**🍒 两者可以分别使用，结合使用时可以实现 `useXStream`。以下是它们的使用示例**

## 代码演示

您只需传入一个`start 方法`即可获得相应的 **loading** 状态和 **finish** 方法。

单一控制，代码不超过 10 行

<demo src="./demos/useSend-base.vue"></demo>

有了状态控制，我们可以轻松地为某些按钮自定义加载状态

<demo src="./demos/useSend-use.vue"></demo>

了解了 `useSend` 的基本用法后，既然有控制`前端 loading 状态`的能力，那肯定也有`请求状态`的控制。接下来，让我们介绍工具类 `XRequest` 的简单用法。

<demo src="./demos/XRequest-base.vue"></demo>

::: warning
这里为了方便大家阅读文档和查看请求，我们写了一个简单的 node 服务。在这个示例中，💩 请不要疯狂点击。它会疯狂请求接口，请适度。💩 我们没有做任何安全处理 🙉 因为我们不会

这也能帮助大家更好地理解工具类 `XRequest` 的用法，它只处理`请求`。
:::

<demo src="./demos/XRequest-use.vue"></demo>

下面，让我们介绍 `useSend` 和 `useSendStream` 的结合用法

**使用 `useSend` 控制前端状态，使用 `useSendStream` 控制后端状态**

<demo src="./demos/useSend-XRequest.vue"></demo>

## 配置参数和返回 Hooks

#### - `useSend`

- **参数**

| 参数名        | 说明        | 类型          |
| ------------- | ----------- | ------------- |
| sendHandler   | 发送方法    | `() => void`  |
| abortHandler  | 中止方法    | `() => void`  |

- **返回值**

| 属性名   | 说明             | 类型          |
| -------- | ---------------- | ------------- |
| send     | 开始加载状态     | `() => void`  |
| abort    | 中止加载状态     | `() => void`  |
| loading  | 加载状态         | `boolean`     |
| finish   | 结束加载状态     | `() => void`  |

#### - `XRequest`

- **参数**

| 配置参数名    | 说明                                                   | 类型                                                         |
| ------------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| baseURL       | 基础请求 URL                                           | `string`                                                     |
| type          | 请求类型，默认 SSE                                      | `BaseSSEProps<T = string>.type?: SSEType \| undefined`       |
| transformer   | 转换器回调，可在此处解析和处理数据                      | `(e: string) => string \| undefined`                         |
| onMessage     | 请求过程中的回调                                        | `(msg: string \| undefined) => void`                         |
| onError       | 错误回调                                               | `(es: EventSource, e: Event) => void`                        |
| onOpen        | SSE 打开状态                                           | `SSEWithSSEProps.onOpen?: (() => void) \| undefined`         |
| onAbort       | 请求中止时的回调                                       | `(messages: (string \| undefined)[]) => void`                |
| onFinish      | 请求结束时的回调                                       | `(data: (string \| undefined)[]) => void`                    |

- **返回值**

| 属性名 | 说明                         | 类型                                                                                                                                     |
| ------ | ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| send   | 开始请求接口                 | `XRequest<string \| undefined>.send(url: string, options?: EventSourceInit \| BaseFetchOptions): Promise<XRequest<string \| undefined>>` |
| abort  | 中止请求                     | `XRequest<string \| undefined>.abort(): void`                                                                                            |

## 总结

`useSend` 让用户更方便地在前端显示和控制**loading** 状态。它是 `loading` 状态的封装方案。
它接收一个`send 回调`和一个`abort 回调`，提供 `send`、`abort` 加载状态、`结束加载状态`，并返回 `loading` 状态。

`XRequest` 是对请求的封装，提供了更方便的请求方式。它接收一个`请求配置`，并返回一个`请求响应`对象。
