---
title: XMarkdown 组件
---

## 简介

`XMarkdown` 组件内置了**行内代码**、**代码块**、**数学公式功能（行内/块级）**、**mermaid 图表**等基础样式。

:::info

⚠️ 在此开发文档中，部分样式演示可能效果不佳，但这不应影响集成使用。如果存在集成或使用问题，可以加入 👉[社区群](https://element-plus-x.com/en/introduce.html#%F0%9F%91%A5-%E7%A4%BE%E5%8C%BA%E6%94%AF%E6%8C%81)获取最新技术支持。

:::

:::warning

文档演示中的所有 `h 函数`演示都意味着您可以使用 `h` 函数进行组件渲染，或者使用`自定义 Vue 组件`进行渲染。

**例如：**

```vue
<!-- 这是文档中的 h 函数演示 -->
(props:any) => {return h('div', props, {default: () => '测试'})}
```

**🙊 实际上可以写成自定义组件：**

```vue
<!-- 从外部导入您的 Vue 组件 -->
import SelfButton from './SelfButton.vue'

<!-- 自定义组件渲染 -->
<!-- SelfButton 是您上方的自定义 Vue 组件 -->
<!-- selfProps 是自定义属性，接收 props -->
(props:any) => {return h(SelfButton, { selfProps: props })}
```

**💟 在您的自定义组件中可以这样获取 props：**

```vue
<!-- SelfButton.vue -->

<script setup lang="ts">
const props = defineProps({
  selfProps: { type: Object, default: () => ({}) }
});

console.log(props.selfProps); // 在控制台查看您获取的 props
</script>

<template>
  <!-- 您可以获取 props 属性进行一些自定义渲染 -->
  <button>{{ selfProps.text }}</button>
</template>
```

:::

## 代码演示

### 基础用法

<demo src="./demos/base.vue"></demo>

### 设置默认高亮/暗黑模式

<demo src="./demos/default-theme-mode.vue"></demo>

### 内置 Shiki 主题

代码块高亮有多个内置主题可供选择。

<demo src="./demos/shiki-style.vue"></demo>

### 单独定义代码块高亮样式

<demo src="./demos/color-replacements.vue"></demo>

### 统一样式覆盖

<demo src="./demos/base-style.vue"></demo>

下面以使用 `github` 样式文件统一覆盖样式为例

### Github 样式

<demo src="./demos/github-style.vue"></demo>

如果您想单独控制代码块高亮样式，可以这样做：

### allowHtml

<demo src="./demos/allow-html.vue"></demo>

### enableLatex

<demo src="./demos/enable-latex.vue"></demo>

### enableBreaks

<demo src="./demos/enable-breaks.vue"></demo>

### 预览 HTML 代码块

<demo src="./demos/view-html.vue"></demo>

### 自定义代码块渲染

<demo src="./demos/code-x-render.vue"></demo>

示例代码中引入的 echarts 组件。源码放在这里供大家理解。如何自定义组件，这里的 echarts 组件实现仅供参考。实际使用时请根据自己的后端数据和需求进行封装。

::: details 💝查看 `echarts` 组件代码示例

```vue
<!-- echarts.vue -->

<script setup lang="ts">
import * as echarts from 'echarts';

// 保持原始 props.code 逻辑，同时添加可选配置
const props = defineProps<{
  code: string; // 原始 JSON 字符串配置
  width?: string; // 可选：图表宽度
  height?: string; // 可选：图表高度
  theme?: string; // 可选：图表主题
}>();

const refEle = ref<HTMLElement>();
let myChart: echarts.ECharts | null = null; // 图表实例引用

function parseEChartsOption(str: string): any {
  try {
    let cleanedStr = str.replace(/^option\s*=\s*/, '').replace(/;\s*$/, '');
    cleanedStr = cleanedStr.replace(/'/g, '"');
    cleanedStr = cleanedStr.replace(/(\w+)\s*:/g, '"$1":');
    return JSON.parse(cleanedStr);
  }
  catch (error) {
    console.error('解析 ECharts 配置失败:', error);
    return null;
  }
}

// 核心渲染逻辑（保持原始解析过程）
function renderChart() {
  if (!refEle.value)
    return;

  try {
    // 解析 JSON 配置（保持原始逻辑）
    const cleanedStr = parseEChartsOption(props.code);

    // 初始化/更新图表
    if (!myChart) {
      myChart = echarts.init(refEle.value, props.theme);
    }
    myChart.setOption(cleanedStr);
  }
  catch (error) {
    console.error('图表配置解析失败:', error);
  }
}

// 窗口大小调整处理
function handleResize() {
  myChart?.resize();
}

// 销毁逻辑
function destroyChart() {
  if (myChart) {
    myChart.dispose(); // 释放 ECharts 实例
    myChart = null;
  }
  window.removeEventListener('resize', handleResize);
}

// 初始化渲染
onMounted(() => {
  renderChart();
  window.addEventListener('resize', handleResize); // 添加大小调整监听
});

// 监听代码变化自动更新（关键优化）
watch(
  () => props.code,
  () => {
    renderChart(); // 配置变化时重新渲染
  }
);

// 卸载时清理资源
onUnmounted(() => {
  destroyChart();
});
</script>

<template>
  <div class="echarts-wrap">
    <span class="echarts-titlt">这是我的自定义 echarts 组件</span>
    <div
      ref="refEle"
      :style="{
        height: height || '400px', // 可选高度，默认 400px
        width: width || '100%' // 可选宽度，默认 100%
      }"
    />
  </div>
</template>

<style scoped lang="less">
.echarts-wrap {
  position: relative;

  .echarts-titlt {
    position: absolute;
    width: fit-content;
    margin-left: 20px;
    color: blue;
    font-size: 20px;
    font-weight: bold;
  }
}
</style>
```

:::

### 自定义代码块顶部渲染

如果您只想修改我们内置的代码块顶部内容，可以使用 `codeXSlot`。我们暴露了内置的 `折叠`、`主题切换`和`复制`方法。您可以保留默认功能，只更改样式。

<demo src="./demos/code-x-slot.vue"></demo>

### 自定义属性

<demo src="./demos/custom-attrs.vue"></demo>

### Mermaid 操作栏配置

<demo src="./demos/mermaid-config.vue"></demo>

### 插槽标签拦截

<demo src="./demos/slot.vue"></demo>

### 内置代码块语言

<demo src="./demos/lang.vue"></demo>

## 属性

| 属性名        | 类型   | 是否必填 | 默认值 | 说明                |
| ------------- | ------ | -------- | ------ | ------------------- |
| `markdown`    | string | 是       | ''     | markdown 内容       |
| `allowHtml`   | bool   | 否       | `false`| 是否渲染 html       |
| `enableLatex` | bool   | 否       | `true` | 是否渲染 latex      |
| `enableBreaks`| bool   | 否       | `true` | 是否渲染换行        |
| `codeXRender` | Object | 否       | `()=>{}`| 自定义代码块渲染 |
| `codeXSlot`   | Object | 否       | `()=>{}`| 自定义代码块顶部插槽渲染 |
| `customAttrs` | Object | 否       | `()=>{}`| 自定义属性         |
| `mermaidConfig`| Object | 否       | `()=>{}`| mermaid 配置       |

## 特性

- 支持增量渲染，性能优异
- 支持自定义插槽，可以是 h 函数组件或 template 组件。更容易上手
- 内置丰富的数学公式、mermaid 图表等基础样式，减少开发者负担
- 支持多种拦截和自定义渲染，达到上限
