# Electron - 进程

**页数:** 7

---

## MessageChannelMain

**URL:** https://www.electronjs.org/docs/latest/api/message-channel-main

**内容:**
- MessageChannelMain
- 类: MessageChannelMain
  - 实例属性
    - channel.port1
    - channel.port2

**概述:**
MessageChannelMain 是 DOM MessageChannel 对象在主进程端的等价物。它的唯一功能是创建一对连接的 MessagePortMain 对象。

**技术说明:**
- 用于主进程中的通道消息传递
- 参见通道消息传递 API 文档了解更多信息
- Electron 的内置类不能在用户代码中子类化

**属性:**
- `port1`: MessagePortMain 属性
- `port2`: MessagePortMain 属性

**使用示例:**
```javascript
// 主进程
const { BrowserWindow, MessageChannelMain } = require('electron')
const w = new BrowserWindow()
const { port1, port2 } = new MessageChannelMain()
w.webContents.postMessage('port', null, [port2])
port1.postMessage({ some: 'message' })

// 渲染进程
const { ipcRenderer } = require('electron')
ipcRenderer.on('port', (e) => {
  // e.ports 是随此消息发送的端口列表
  e.ports[0].onmessage = (messageEvent) => {
    console.log(messageEvent.data)
  }
})
```

---

## MessagePortMain

**URL:** https://www.electronjs.org/docs/latest/api/message-port-main

**内容:**
- MessagePortMain
- 类: MessagePortMain
  - 实例方法
    - port.postMessage(message, [transfer])
    - port.start()
    - port.close()
  - 实例事件
    - 事件: 'message'
    - 事件: 'close'

**概述:**
MessagePortMain 是 DOM MessagePort 对象在主进程端的等价物。它的行为与 DOM 版本类似，但使用 Node.js 的 EventEmitter 事件系统，而不是 DOM 的 EventTarget 系统。

**技术说明:**
- 使用 `port.on('message', ...)` 监听事件，而不是 `port.onmessage = ...`
- MessagePortMain 是 EventEmitter
- 主进程类：不从 'electron' 模块导出
- 只能作为 Electron API 中其他方法的返回值使用

**方法:**
- `postMessage(message, [transfer])`: 从端口发送消息，可选择转移对象所有权
- `start()`: 开始发送端口上排队的消息
- `close()`: 断开端口连接，使其不再活动

**事件:**
- `'message'`: 当 MessagePortMain 对象收到消息时发出
- `'close'`: 当 MessagePortMain 对象的远程端断开连接时发出

---

## ProcessMemoryInfo 对象

**URL:** https://www.electronjs.org/docs/latest/api/structures/process-memory-info

**内容:**
- ProcessMemoryInfo 对象

**概述:**
进程内存信息对象，包含进程的内存使用统计数据。

**属性:**
- `workingSetSize`: 进程的工作集大小（以字节为单位）
- `peakWorkingSetSize`: 进程的峰值工作集大小
- `privateBytes`: 进程的私有字节数
- `sharedBytes`: 进程的共享字节数

---

## utilityProcess

**URL:** https://www.electronjs.org/docs/latest/api/utility-process

**内容:**
- utilityProcess
- 方法
  - utilityProcess.fork(modulePath[, args][, options])
- 类: UtilityProcess
  - 实例方法
    - child.postMessage(message, [transfer])
    - child.kill()
  - 实例属性
    - child.pid
    - child.stdout

**概述:**
utilityProcess 创建一个启用了 Node.js 和消息端口的子进程。它提供 Node.js 的 `child_process.fork` API 的等价物，但使用 Chromium 的 Services API 来启动子进程。

**技术说明:**
- 只能在 app 的 ready 事件发出后调用
- UtilityProcess 实例代表 Chromium 生成的具有 Node.js 集成的子进程
- UtilityProcess 是 EventEmitter

**方法:**
- `fork(modulePath[, args][, options])`: 返回 UtilityProcess
  - 创建具有 Node.js 和 Message ports 的子进程

**实例方法:**
- `postMessage(message, [transfer])`: 向子进程发送消息
- `kill()`: 优雅地终止进程

**实例属性:**
- `pid`: 子进程的进程标识符（Integer | undefined）
- `stdout`: 子进程的标准输出（NodeJS.ReadableStream | null）

**事件:**
- `'spawn'`: 子进程成功生成后发出
- `'error'`: 子进程需要因 V8 的不可继续错误而终止时发出
- `'exit'`: 子进程结束后发出
- `'message'`: 子进程使用 `process.parentPort.postMessage()` 发送消息时发出

**使用示例:**
```javascript
// 主进程
const { port1, port2 } = new MessageChannelMain()
const child = utilityProcess.fork(path.join(__dirname, 'test.js'))
child.postMessage({ message: 'hello' }, [port1])

// 子进程
process.parentPort.once('message', (e) => {
  const [port] = e.ports
  // ...
})
```

---

## RenderProcessGoneDetails 对象

**URL:** https://www.electronjs.org/docs/latest/api/structures/render-process-gone-details

**内容:**
- RenderProcessGoneDetails 对象

**概述:**
渲染进程消失详细信息对象，包含渲染进程终止的原因和详细信息。

**属性:**
- `reason`: 进程终止的原因（String）
  - `exited`: 正常退出
  - `killed`: 被杀死
  - `crashed`: 崩溃
  - `oom`: 内存不足
  - `launch-failed`: 启动失败
  - `integrity-check-failed`: 完整性检查失败
- `exitCode`: 进程的退出代码（Integer）
- `reasonString`: 人类可读的原因字符串（String）

---

## ProcessMetric 对象

**URL:** https://www.electronjs.org/docs/latest/api/structures/process-metric

**内容:**
- ProcessMetric 对象

**概述:**
进程指标对象，包含进程的 CPU 和内存使用信息。

**属性:**
- `pid`: 进程 ID（Integer）
- `type`: 进程类型（String）
  - 'Browser'
  - 'Tab'
  - 'Utility'
  - 'Zygote'
  - 'Sandbox helper'
  - 'GPU'
  - 'Pepper Plugin'
  - 'Pepper Plugin Broker'
- `name`: 进程名称（String）
- `cpu`: CPU 使用百分比（Number）
- `memory`: 内存使用信息（ProcessMemoryInfo）

---

## process

**URL:** https://www.electronjs.org/docs/latest/api/process

**内容:**
- process
- 沙箱
- 事件
  - 事件: 'loaded'
- 属性
  - process.defaultApp 只读
  - process.isMainFrame 只读
  - process.mas 只读
  - process.noAsar
  - process.noDeprecation

**概述:**
process 对象的扩展。Electron 的 process 对象从 Node.js process 对象扩展而来，添加了以下事件、属性和方法。

**进程类型:**
- 主进程 (Main)
- 渲染进程 (Renderer)

**技术说明:**
- 在沙箱渲染器中，process 对象只包含 API 的子集
- 所有统计数据以 KB 为单位报告

**事件:**
- `'loaded'`: Electron 已加载其内部初始化脚本并开始加载网页或主脚本时发出

**属性:**
- `defaultApp` (Boolean): 当应用作为参数传递给默认 Electron 可执行文件时为 true
- `isMainFrame` (Boolean): 当前渲染器上下文是否为"主"渲染器框架
- `mas` (Boolean): 对于 Mac App Store 构建为 true
- `noAsar` (Boolean): 控制 ASAR 支持
- `noDeprecation` (Boolean): 控制是否打印弃用警告
- `resourcesPath` (String): 资源目录的路径
- `sandboxed` (Boolean): 渲染进程是否被沙箱化
- `contextIsolated` (Boolean): 当前渲染器上下文是否启用了 contextIsolation
- `throwDeprecation` (Boolean): 控制是否将弃用警告抛出为异常
- `traceDeprecation` (Boolean): 控制弃用警告是否包含堆栈跟踪
- `traceProcessWarnings` (Boolean): 控制进程警告是否包含堆栈跟踪
- `type` (String): 当前进程的类型
  - 'browser'
  - 'renderer'
  - 'worker'
- `versions.chrome` (String): Chrome 的版本字符串
- `versions.electron` (String): Electron 的版本字符串
- `windowsStore` (Boolean): 应用是否作为 Windows Store 应用运行
- `contextId` (String): 当前 JavaScript 上下文的全局唯一 ID
- `parentPort` (Electron.ParentPort | null): 与父进程通信

**方法:**
- `crash()`: 导致当前进程的主线程崩溃
- `getCreationTime()`: 返回应用的创建时间（毫秒）
- `getHeapStatistics()`: 返回 V8 堆统计信息的对象
- `getBlinkMemoryInfo()`: 返回 Blink 内存信息的对象
- `getProcessMemoryInfo()`: 返回当前进程内存使用统计信息的 Promise
- `getSystemMemoryInfo()`: 返回整个系统内存使用统计信息的对象
- `getSystemVersion()`: 返回主机操作系统的版本
- `takeHeapSnapshot(filePath)`: 拍摄 V8 堆快照并保存到 filePath
- `hang()`: 导致当前进程的主线程挂起
- `setFdLimit(maxDescriptors)`: 将文件描述符软限制设置为 maxDescriptors

**使用示例:**
```javascript
const version = process.getSystemVersion()
console.log(version)
// macOS -> '10.13.6'
// Windows -> '10.0.17763'
// Linux -> '4.15.0-45-generic'
```

**技术说明:**
- `getProcessMemoryInfo()` 应在 app ready 后调用
- macOS 不提供 residentSet 值，因为内存压缩
- `getSystemVersion()` 返回实际操作系统版本，而不是内核版本
- `contextId` 仅在渲染进程中可用
- `takeHeapSnapshot()` 返回是否成功创建快照的布尔值
