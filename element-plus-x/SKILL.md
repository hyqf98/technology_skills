---
name: element-plus-x
description: Element Plus X - Extended UI component library for Vue3 with AI experience components. Use this skill when building Vue3 applications with Element Plus and AI-powered chat interfaces, typewriter effects, markdown rendering, and conversation management.
---

# Element Plus X - Vue3 AI Component Library

## When to Use This Skill

Use this skill when:
- Building **Vue 3 applications** with **AI chat interfaces**
- Implementing **conversational UI** with Element Plus
- Adding **typewriter effects**, **chat bubbles**, or **streaming responses**
- Creating **voice-enabled** input components
- Rendering **Markdown content** in chat interfaces
- Building **AI assistants**, **chatbots**, or **messaging applications**
- Implementing **thought chains** or **AI thinking indicators**
- Managing **conversation history** and **file attachments**

## Overview

**Element Plus X** is an enterprise-level AI component library built on Vue 3 + Element Plus. It provides pre-built, production-ready components for creating modern AI chat interfaces with zero configuration.

### Key Features

- ✨ **Enterprise-level AI Components**: Chat bubbles, typewriter effects, conversation management
- 🚀 **Zero-configuration**: Based on Element Plus design system, works out of the box
- 📦 **Tree Shaking**: On-demand loading for optimal bundle size
- 🎤 **Voice Support**: Built-in browser voice recognition API
- 📝 **Markdown Rendering**: Full markdown support with XMarkdown component
- 💬 **Streaming**: SSE (Server-Sent Events) streaming hooks

## Installation

```bash
# NPM
npm install vue-element-plus-x

# PNPM (Recommended)
pnpm install vue-element-plus-x

# Yarn
yarn add vue-element-plus-x
```

## Quick Start

### On-demand Import (Recommended)

```vue
<script setup>
import { BubbleList, Sender } from 'vue-element-plus-x';

const messages = [
  { content: 'Hello! How can I help you?', role: 'assistant' },
  { content: 'I need help with Vue3', role: 'user' }
];
</script>

<template>
  <div style="display: flex; flex-direction: column; height: 500px;">
    <BubbleList :list="messages" />
    <Sender @send="handleSend" />
  </div>
</template>
```

### Global Import

```ts
// main.ts
import { createApp } from 'vue';
import ElementPlusX from 'vue-element-plus-x';
import App from './App.vue';

const app = createApp(App);
app.use(ElementPlusX);
app.mount('#app');
```

## Quick Reference - Core Components

| Component | Description | Key Props | Use Case |
|-----------|-------------|-----------|----------|
| **Bubble** | Chat message bubble | `content`, `placement`, `avatar`, `typing`, `isMarkdown` | Display single messages with typing effects |
| **BubbleList** | Message list | `list`, `autoScroll` | Display conversation history |
| **Sender** | Input box | `modelValue`, `voiceEnabled` | Basic text input with voice |
| **EditorSender** | Rich editor | `modelValue`, `enableCommands` | Advanced input with formatting |
| **Typewriter** | Text animation | `content`, `interval`, `step` | Simulate typing effect |
| **XMarkdown** | Markdown renderer | `content`, `enableMermaid` | Display formatted markdown |
| **Conversations** | Chat history | `conversations`, `activeId` | Manage multiple conversations |
| **Thinking** | Loading indicator | `loading`, `text` | Show AI processing |
| **ThoughtChain** | Reasoning steps | `steps` | Display AI thinking process |
| **Attachments** | File upload | `modelValue`, `accept` | Handle file inputs |

## Quick Reference - Common Patterns

### Complete Chat Interface

```vue
<script setup>
import { ref } from 'vue';
import { BubbleList, Sender, Thinking } from 'vue-element-plus-x';

const messages = ref([]);
const isThinking = ref(false);

const handleSend = async (content) => {
  messages.value.push({
    id: Date.now(),
    content,
    role: 'user',
    avatar: '/user.png'
  });

  isThinking.value = true;
  const response = await callAI(content);

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

### Streaming Chat Response

```vue
<script setup>
import { useXStream } from 'vue-element-plus-x';

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
  }
});
</script>
```

### Voice-Enabled Input

```vue
<script setup>
import { useRecord } from 'vue-element-plus-x';

const { startRecording, stopRecording, transcript } = useRecord({
  lang: 'en-US',
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

## Hooks & Composables

### useXStream - Streaming API
```typescript
const { data, isLoading, error, start, stop } = useXStream({
  url: '/api/chat/stream',
  method: 'POST',
  onMessage: (chunk) => console.log(chunk),
  onComplete: () => console.log('Done')
});
```

### useRecord - Voice Recognition
```typescript
const { isRecording, transcript, startRecording, stopRecording } = useRecord({
  lang: 'en-US',
  continuous: true,
  onResult: (text) => console.log(text)
});
```

### useSend - API Calls
```typescript
const { send, loading, response } = useSend({
  baseURL: '/api',
  headers: { 'Authorization': 'Bearer token' }
});
```

## Reference Files

This skill includes 22 comprehensive documentation files in `references/`:

**Core Components:**
- `bubble.md` - Chat bubble component
- `bubbleList.md` - Message list component
- `sender.md` - Basic input box
- `editorSender.md` - Rich text editor input
- `typewriter.md` - Typing animation
- `xmarkdown.md` - Markdown renderer

**Conversation Management:**
- `conversations.md` - Multi-chat management
- `welcome.md` - Welcome screen
- `prompts.md` - Prompt suggestions

**AI Features:**
- `thinking.md` - AI thinking indicator
- `thoughtChain.md` - Reasoning chain display

**File Handling:**
- `attachments.md` - File upload
- `filesCard.md` - File display

**Hooks:**
- `useRecord.md` - Voice recognition
- `useXStream.md` - Streaming API
- `useSend.md` - HTTP requests

**Guides:**
- `install.md` - Installation guide
- `develop.md` - Development guide
- `readme.md` - Project overview

Use Claude Code's `view` command to read specific reference files for detailed documentation.

## Best Practices

1. **Message Management**: Use unique IDs for messages and reactive refs
2. **Error Handling**: Always wrap API calls in try-catch blocks
3. **Typing Effects**: Enable only for AI responses to save performance
4. **Markdown**: Enable markdown rendering for AI responses
5. **Performance**: Limit displayed messages for large conversations

## TypeScript Support

Full TypeScript definitions included:
```typescript
import type { BubbleProps, SenderProps, Message } from 'vue-element-plus-x';
```

## Resources

- **Documentation**: https://element-plus-x.com/en/
- **GitHub**: https://github.com/HeJiaYue520/Element-Plus-X
- **NPM**: https://www.npmjs.com/package/vue-element-plus-x
- **Demo**: https://chat.element-plus-x.com/
- Table of contents for quick navigation

### scripts/
Add helper scripts here for common automation tasks.

### assets/
Add templates, boilerplate, or example projects here.

## Notes

- This skill was automatically generated from official documentation
- Reference files preserve the structure and examples from source docs
- Code examples include language detection for better syntax highlighting
- Quick reference patterns are extracted from common usage examples in the docs

## Updating

To refresh this skill with updated documentation:
1. Re-run the scraper with the same configuration
2. The skill will be rebuilt with the latest information
