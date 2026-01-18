# 安装指南

#### 一、环境要求

| 工具      | 版本要求            | 说明           |
| --------- | ------------------- | -------------- |
| Node.js   | ≥ 18.x（推荐 ≥20.x）| 主流 LTS 版本  |
| Vue       | ≥ 3.3.X             | Vue 3 官方版本 |
| pnpm      | ≥ 10.X              | pnpm 安装      |

#### 二、安装方式

::: code-group

```sh [npm]
npm install vue-element-plus-x --save
```

```sh [pnpm]
pnpm add vue-element-plus-x --save
```

```sh [yarn]
yarn add vue-element-plus-x --save
```

:::

**CDN 导入方式**

```html
<!-- 此方法需要测试 -->
<!-- CDN 导入 -->
<script src="https://unpkg.com/vue-element-plus-x@1.3.0/dist/umd/index.js"></script>

```

#### 三、验证安装

1. 检查 `package.json` 文件是否包含：

   ```json
   {
     "dependencies": {
       "vue-element-plus-x": "^1.3.0"
     }
   }
   ```

2. 运行项目验证组件是否可用：

   ```bash
   npm run dev
   ```

#### 四、按需加载说明

内置 **Tree Shaking** 优化，无需额外配置

1. **按需导入**

```vue
<script setup>
import { BubbleList, Sender } from 'vue-element-plus-x';

const list = [
  {
    content: '你好，Element Plus X',
    role: 'user'
  }
];
</script>

<template>
  <div
    style="display: flex; flex-direction: column; height: 230px; justify-content: space-between;"
  >
    <BubbleList :list="list" />
    <Sender />
  </div>
</template>
```

2. **全量导入**

```ts
// main.ts
import { createApp } from 'vue';
import ElementPlusX from 'vue-element-plus-x';
import App from './App.vue';

const app = createApp(App);
// 使用 app.use() 进行全局导入
app.use(ElementPlusX);
app.mount('#app');
```
