---
name: element-plus-x
description: Element Plus X - 基于 Vue3 的扩展 AI 组件库。使用此技能构建带有 AI 聊天界面、打字机效果、Markdown 渲染和对话管理的 Vue3 应用。
---

# Element Plus X 技能文档

## 何时使用此技能

在以下场景中使用此技能：
- 构建带有 **AI 聊天界面**的 **Vue 3 应用**
- 实现基于 Element Plus 的**对话式 UI**
- 添加**打字机效果**、**聊天气泡**或**流式响应**
- 创建支持**语音输入**的组件
- 在聊天界面中渲染 **Markdown 内容**
- 构建 **AI 助手**、**聊天机器人**或**消息应用**
- 实现**思维链**或**AI 思考指示器**
- 管理**对话历史**和**文件附件**

## 技术概述

**Element Plus X** 是基于 Vue 3 + Element Plus 构建的企业级 AI 组件库。它提供了开箱即用的预构建组件，用于创建现代 AI 聊天界面。

### 核心特性

- ✨ **企业级 AI 组件**：聊天气泡、打字机效果、对话管理
- 🚀 **零配置**：基于 Element Plus 设计系统，开箱即用
- 📦 **Tree Shaking**：按需加载，优化打包体积
- 🎤 **语音支持**：内置浏览器语音识别 API
- 📝 **Markdown 渲染**：通过 XMarkdown 组件支持完整 Markdown
- 💬 **流式传输**：SSE（服务器发送事件）流式 Hooks

## 快速参考

### 核心组件

| 组件 | 描述 | 关键属性 | 使用场景 |
|------|------|---------|---------|
| **Bubble** | 聊天气泡 | `content`、`placement`、`avatar`、`typing`、`isMarkdown` | 带打字效果的单条消息 |
| **BubbleList** | 消息列表 | `list`、`autoScroll` | 显示对话历史 |
| **Sender** | 输入框 | `modelValue`、`voiceEnabled` | 基础文本输入 + 语音 |
| **EditorSender** | 富编辑器 | `modelValue`、`enableCommands` | 支持格式化的高级输入 |
| **Typewriter** | 文本动画 | `content`、`interval`、`step` | 模拟打字效果 |
| **XMarkdown** | Markdown 渲染器 | `content`、`enableMermaid` | 显示格式化的 Markdown |
| **Conversations** | 聊天历史 | `conversations`、`activeId` | 管理多个对话 |
| **Thinking** | 加载指示器 | `loading`、`text` | 显示 AI 处理状态 |
| **ThoughtChain** | 推理步骤 | `steps` | 显示 AI 思考过程 |
| **Attachments** | 文件上传 | `modelValue`、`accept` | 处理文件输入 |

### 常见配置模式

#### 模式 1：完整聊天界面

```vue
<script setup>
import { ref } from 'vue';
import { BubbleList, Sender, Thinking } from 'vue-element-plus-x';

const messages = ref([]);
const isThinking = ref(false);

const handleSend = async (content) => {
  // 添加用户消息
  messages.value.push({
    id: Date.now(),
    content,
    role: 'user',
    avatar: '/user.png'
  });

  isThinking.value = true;
  const response = await callAI(content);

  // 添加 AI 响应（带打字效果和 Markdown）
  messages.value.push({
    id: Date.now(),
    content: response,
    role: 'assistant',
    avatar: '/bot.png',
    typing: { step: 2, interval: 30 },
    isMarkdown: true
  });

  isThinking.value = false;
};
</script>

<template>
  <div class="chat-container">
    <BubbleList :list="messages" :auto-scroll="true" />
    <Thinking v-if="isThinking" />
    <Sender @send="handleSend" />
  </div>
</template>
```

#### 模式 2：流式聊天响应

```vue
<script setup>
import { ref } from 'vue';
import { useXStream } from 'vue-element-plus-x';

const currentResponse = ref('');
const messages = ref([]);

const { start, isLoading } = useXStream({
  url: '/api/chat/stream',
  method: 'POST',
  onMessage: (chunk) => {
    currentResponse.value += chunk;
  },
  onComplete: () => {
    messages.value.push({
      id: Date.now(),
      content: currentResponse.value,
      role: 'assistant'
    });
    currentResponse.value = '';
  }
});

const handleSend = async (content) => {
  messages.value.push({ id: Date.now(), content, role: 'user' });
  await start({ message: content });
};
</script>

<template>
  <div class="chat-container">
    <BubbleList :list="messages" />
    <div v-if="isLoading">{{ currentResponse }}</div>
    <Sender @send="handleSend" />
  </div>
</template>
```

#### 模式 3：语音输入

```vue
<script setup>
import { ref } from 'vue';
import { useRecord } from 'vue-element-plus-x';

const message = ref('');

const { startRecording, stopRecording, transcript } = useRecord({
  lang: 'zh-CN',
  onResult: (text) => {
    message.value = text;
  }
});
</script>

<template>
  <Sender
    v-model="message"
    :voice-enabled="true"
    @voice-start="startRecording"
    @voice-end="stopRecording"
  />
</template>
```

#### 模式 4：Markdown 渲染

```vue
<script setup>
import { XMarkdown } from 'vue-element-plus-x';

const markdownContent = `
# 标题

**粗体** 和 *斜体*

\`\`\`javascript
console.log('Hello World');
\`\`\`

- 列表项 1
- 列表项 2
`;
</script>

<template>
  <XMarkdown :content="markdownContent" :enable-mermaid="true" />
</template>
```

#### 模式 5：思维链显示

```vue
<script setup>
import { ref } from 'vue';
import { ThoughtChain } from 'vue-element-plus-x';

const thoughtSteps = ref([
  { title: '分析问题', content: '正在理解用户意图...', status: 'completed' },
  { title: '检索知识', content: '从知识库中查找相关信息...', status: 'processing' },
  { title: '生成回答', content: '等待完成...', status: 'pending' }
]);
</script>

<template>
  <ThoughtChain :steps="thoughtSteps" />
</template>
```

#### 模式 6：对话管理

```vue
<script setup>
import { ref } from 'vue';
import { Conversations } from 'vue-element-plus-x';

const conversations = ref([
  { id: '1', title: '新对话 1', time: '10:30' },
  { id: '2', title: '新对话 2', time: '11:00' }
]);
const activeId = ref('1');

const handleCreate = () => {
  const newId = Date.now().toString();
  conversations.value.unshift({
    id: newId,
    title: `新对话 ${conversations.value.length + 1}`,
    time: new Date().toLocaleTimeString()
  });
  activeId.value = newId;
};
</script>

<template>
  <Conversations
    :conversations="conversations"
    :active-id="activeId"
    @create="handleCreate"
  />
</template>
```

### Hooks 与 Composables

#### useXStream - 流式 API

```typescript
import { useXStream } from 'vue-element-plus-x';

const { data, isLoading, error, start, stop } = useXStream({
  url: '/api/chat/stream',
  method: 'POST',
  onMessage: (chunk) => console.log(chunk),
  onComplete: () => console.log('完成'),
  onError: (err) => console.error(err)
});
```

#### useRecord - 语音识别

```typescript
import { useRecord } from 'vue-element-plus-x';

const { isRecording, transcript, startRecording, stopRecording } = useRecord({
  lang: 'zh-CN',
  continuous: true,
  onResult: (text) => console.log(text),
  onError: (err) => console.error(err)
});
```

#### useSend - HTTP 请求

```typescript
import { useSend } from 'vue-element-plus-x';

const { send, loading, response } = useSend({
  baseURL: '/api',
  headers: { 'Authorization': 'Bearer token' }
});

// 发送请求
await send('/chat', { method: 'POST', data: { message: 'Hello' } });
```

## 最佳实践

### 1. 消息管理

```typescript
// 为消息使用唯一 ID 和响应式 refs
const messages = ref([]);

const addMessage = (content, role) => {
  messages.value.push({
    id: `${Date.now()}-${Math.random()}`,
    content,
    role,
    timestamp: Date.now()
  });
};
```

### 2. 错误处理

```vue
<script setup>
import { ref } from 'vue';

const error = ref(null);

const handleSend = async (content) => {
  try {
    error.value = null;
    const response = await callAI(content);
    // 处理响应
  } catch (err) {
    error.value = '发送失败，请重试';
    console.error(err);
  }
};
</script>

<template>
  <div v-if="error" class="error-message">{{ error }}</div>
  <Sender @send="handleSend" />
</template>
```

### 3. 性能优化

```typescript
// 限制显示的消息数量
const MAX_DISPLAY_MESSAGES = 100;

const addMessage = (message) => {
  messages.value.push(message);
  if (messages.value.length > MAX_DISPLAY_MESSAGES) {
    messages.value.shift(); // 移除最旧的消息
  }
};
```

### 4. Markdown 配置

```vue
<template>
  <!-- 仅对 AI 响应启用 Markdown -->
  <BubbleList :list="messages">
    <template #default="{ item }">
      <XMarkdown
        v-if="item.isMarkdown"
        :content="item.content"
        :enable-mermaid="false"
        :enable-code-highlight="true"
      />
      <div v-else>{{ item.content }}</div>
    </template>
  </BubbleList>
</template>
```

## TypeScript 支持

完整的 TypeScript 类型定义：

```typescript
import type {
  BubbleProps,
  SenderProps,
  Message,
  Conversation,
  ThoughtStep
} from 'vue-element-plus-x';

// 定义消息类型
interface ChatMessage extends Message {
  id: string;
  content: string;
  role: 'user' | 'assistant' | 'system';
  avatar?: string;
  typing?: { step: number; interval: number };
  isMarkdown?: boolean;
}

const messages = ref<ChatMessage[]>([]);
```

## 参考文档

此技能在 `references/` 目录中包含 22 个全面的文档文件：

**核心组件：**
- `bubble.md` - 聊天气泡组件
- `bubbleList.md` - 消息列表组件
- `sender.md` - 基础输入框
- `editorSender.md` - 富文本编辑器输入
- `typewriter.md` - 打字动画
- `xmarkdown.md` - Markdown 渲染器

**对话管理：**
- `conversations.md` - 多对话管理
- `welcome.md` - 欢迎界面
- `prompts.md` - 提示建议

**AI 功能：**
- `thinking.md` - AI 思考指示器
- `thoughtChain.md` - 推理链显示

**文件处理：**
- `attachments.md` - 文件上传
- `filesCard.md` - 文件显示

**Hooks：**
- `useRecord.md` - 语音识别
- `useXStream.md` - 流式 API
- `useSend.md` - HTTP 请求

**指南：**
- `install.md` - 安装指南
- `develop.md` - 开发指南
- `readme.md` - 项目概述

## 使用指南

### 对于初学者
1. 安装 `vue-element-plus-x`
2. 使用 `BubbleList` 和 `Sender` 创建基础聊天界面
3. 学习 `useXStream` 实现流式响应
4. 添加 `Thinking` 组件显示加载状态

### 对于进阶开发
- 使用 `useRecord` 实现语音输入
- 通过 `XMarkdown` 支持 Markdown 渲染
- 使用 `ThoughtChain` 显示 AI 推理过程
- 通过 `Conversations` 管理多对话

### 对于性能优化
- 启用按需加载
- 限制消息列表长度
- 对大文本内容使用虚拟滚动
- 合理配置打字效果参数

## 资源

### references/
从官方来源提取的组织化文档，包含：
- 详细的组件说明
- 带语言标注的代码示例
- 原始文档链接
- 快速导航目录

### scripts/
添加用于常见自动化任务的辅助脚本。

### assets/
添加模板、样板代码或示例项目。

## 注意事项

- 此技能从官方文档自动生成
- 确保正确配置 Element Plus
- 打字效果会影响性能，仅在需要时启用
- 语音识别需要浏览器支持 Web Speech API

## 更新说明

刷新此技能的文档：
1. 使用相同配置重新运行爬虫
2. 技能将使用最新信息重建

## 相关资源

### 官方资源
- [Element Plus X 官方文档](https://element-plus-x.com/)
- [GitHub 仓库](https://github.com/HeJiaYue520/Element-Plus-X)
- [NPM 包](https://www.npmjs.com/package/vue-element-plus-x)
- [在线演示](https://chat.element-plus-x.com/)
