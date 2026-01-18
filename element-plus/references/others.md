# Element Plus - 其他组件

**页面数：** 2

---

## Watermark 水印

**URL：** llms-txt#watermark

**内容概览：**
- 基础用法
- 多行水印
- 图片水印
- 自定义配置
- API
  - Attributes（属性）
  - Font（字体）
  - Slots（插槽）
- Vue 示例
  - basic.vue

### 组件说明
在页面添加特定的文本或图案作为水印，用于保护内容版权或标识文档状态。

### 基础用法
最基本的水印使用方式，默认显示 "Element Plus" 文本。

### 多行水印
通过 `content` 属性设置字符串数组来指定多行文本水印内容。

### 图片水印
通过 `image` 属性指定图片地址。为确保图片清晰且不拉伸，应设置合适的宽度和高度，建议上传至少 2 倍于显示尺寸的图片。

### 自定义配置
通过配置自定义参数来预览水印效果。

### Attributes（属性）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| width | 水印的宽度，`content` 为字符串时默认使用其自身宽度 | ^[number] | 120 |
| height | 水印的高度，`content` 为字符串时默认使用其自身高度 | ^[number] | 64 |
| rotate | 绘制水印时的旋转角度，单位 `°` | ^[number] | -22 |
| z-index | 添加的水印元素的 z-index 层级 | ^[number] | 9 |
| image | 图片源，建议导出 2x 或 3x 图片，优先级较高 | ^[string] | — |
| content | 水印文本内容 | ^[string]/^[object]`string[]` | Element Plus |
| font | 文本样式 | [Font](#font) | [Font](#font) |
| gap | 水印之间的间距 | ^[object]`[number, number]` | \[100, 100\] |
| offset | 水印相对于容器左上角的偏移量，默认为 `gap/2` | ^[object]`[number, number]` | \[gap\[0\]/2, gap\[1\]/2\] |

### Font（字体配置）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| color | 字体颜色 | ^[string] | rgba(0,0,0,.15) |
| fontSize | 字体大小 | ^[number] / ^[string] | 16 |
| fontWeight | 字体粗细 | ^[enum]`'normal' \| 'light' \| 'weight' \| number` | normal |
| fontFamily | 字体族 | ^[string] | sans-serif |
| fontGap ^(2.11.5) | 字体间距 | ^[number] | 3 |
| fontStyle | 字体样式 | ^[enum]`'none' \| 'normal' \| 'italic' \| 'oblique'` | normal |
| textAlign | 文本对齐 | ^[enum]`'left' \| 'right' \| 'center' \| 'start' \| 'end' ` | center |
| textBaseline | 文本基线 | ^[enum]`'top' \| 'hanging' \| 'middle' \| 'alphabetic' \| 'ideographic' \| 'bottom'` | hanging |

### Slots（插槽）

| 插槽名 | 说明 |
| ------ | ---- |
| default | 需要添加水印的容器内容 |

**使用示例：**

示例 1 (vue) - 基础用法：
```vue
<script setup lang="ts">
import { reactive, watch } from 'vue'
import { isDark } from '~/composables/dark'

const font = reactive({
  color: 'rgba(0, 0, 0, .15)',
})

// 根据暗黑模式自动调整水印颜色
watch(
  isDark,
  () => {
    font.color = isDark.value
      ? 'rgba(255, 255, 255, .15)'
      : 'rgba(0, 0, 0, .15)'
  },
  {
    immediate: true,
  }
)
</script>

<template>
  <el-watermark :font="font">
    <div style="height: 500px" />
  </el-watermark>
</template>
```

示例 2 (vue) - 自定义配置：
```vue
<script setup lang="ts">
import { reactive } from 'vue'

const config = reactive({
  content: 'Element Plus',
  font: {
    fontSize: 16,
    color: 'rgba(0, 0, 0, 0.15)',
  },
  zIndex: -1,
  rotate: -22,
  gap: [100, 100] as [number, number],
  offset: [] as unknown as [number, number],
})
</script>

<template>
  <div class="wrapper">
    <el-watermark
      class="watermark"
      :content="config.content"
      :font="config.font"
      :z-index="config.zIndex"
      :rotate="config.rotate"
      :gap="config.gap"
      :offset="config.offset"
    >
      <div class="watermark-container">
        <h1>Element Plus</h1>
        <h2>基于 Vue 3 的组件库</h2>
        <img src="/images/hamburger.png" alt="示例图片" />
      </div>
    </el-watermark>
    <el-form
      class="form"
      :model="config"
      label-position="top"
      label-width="50px"
    >
      <el-form-item label="水印内容">
        <el-input v-model="config.content" />
      </el-form-item>
      <el-form-item label="字体颜色">
        <el-color-picker v-model="config.font.color" show-alpha />
      </el-form-item>
      <el-form-item label="字体大小">
        <el-slider v-model="config.font.fontSize" />
      </el-form-item>
      <el-form-item label="层级 z-index">
        <el-slider v-model="config.zIndex" />
      </el-form-item>
      <el-form-item label="旋转角度">
        <el-slider v-model="config.rotate" :min="-180" :max="180" />
      </el-form-item>
      <el-form-item label="间距 gap">
        <el-space>
          <el-input-number v-model="config.gap[0]" controls-position="right" />
          <el-input-number v-model="config.gap[1]" controls-position="right" />
        </el-space>
      </el-form-item>
      <el-form-item label="偏移量 offset">
        <el-space>
          <el-input-number
            v-model="config.offset[0]"
            placeholder="左侧偏移"
            controls-position="right"
          />
          <el-input-number
            v-model="config.offset[1]"
            placeholder="顶部偏移"
            controls-position="right"
          />
        </el-space>
      </el-form-item>
    </el-form>
  </div>
</template>

<style scoped>
.wrapper {
  display: flex;
}
.watermark {
  display: flex;
  flex: auto;
}
.watermark-container {
  flex: auto;
}
.form {
  width: 330px;
  margin-left: 20px;
  border-left: 1px solid #eee;
  padding-left: 20px;
}

img {
  z-index: 10;
  width: 100%;
  max-width: 300px;
  position: relative;
}
</style>
```

示例 3 (vue) - 图片水印：
```vue
<template>
  <el-watermark
    :width="130"
    :height="30"
    image="https://element-plus.org/images/element-plus-logo.svg"
  >
    <div style="height: 500px" />
  </el-watermark>
</template>
```

示例 4 (vue) - 多行文本水印：
```vue
<script setup lang="ts">
import { reactive, watch } from 'vue'
import { isDark } from '~/composables/dark'

const font = reactive({
  color: 'rgba(0, 0, 0, .15)',
})

watch(
  isDark,
  () => {
    font.color = isDark.value
      ? 'rgba(255, 255, 255, .15)'
      : 'rgba(0, 0, 0, .15)'
  },
  {
    immediate: true,
  }
)
</script>

<template>
  <!-- 使用数组设置多行水印 -->
  <el-watermark :font="font" :content="['Element+', 'Element Plus']">
    <div style="height: 500px" />
  </el-watermark>
</template>
```

---

## Divider 分割线

**URL：** llms-txt#divider

**内容概览：**
- 基础用法
- 自定义内容
- 虚线
- 垂直分割线
- API
  - Attributes（属性）
  - Slots（插槽）
- Vue 示例
  - basic-usage.vue
  - custom-content.vue

### 组件说明
用于分隔内容的分割线，可以在不同段落之间创建视觉分隔。

### 基础用法
分隔不同段落的文本。

### 自定义内容
可以在分割线上自定义显示的内容。

### 虚线样式
可以设置分割线的样式为虚线或其他 CSS 边框样式。

### 垂直分割线
设置垂直方向的分割线。

### Attributes（属性）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| direction | 设置分割线的方向 | ^[enum]`'horizontal' \| 'vertical'` | horizontal |
| border-style | 设置分割线的样式 | ^[enum]`'none' \| 'solid' \| 'hidden' \| 'dashed' \| ...` [css/border-style](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-style) | solid |
| content-position | 自定义内容在分割线上的位置 | ^[enum]`'left' \| 'right' \| 'center' ` | center |

### Slots（插槽）

| 插槽名 | 说明 |
| ------ | ---- |
| default | 分割线上的自定义内容 |

**使用示例：**

示例 1 (vue) - 基础用法：
```vue
<template>
  <div>
    <span>
      我今早坐在窗前，世界如过客般在我面前停留片刻，
      向我点头致意后离去。
    </span>
    <el-divider />
    <span>
      这些微小的思绪是树叶的沙沙声；它们在我的心中
      充满着欢乐的低语。
    </span>
  </div>
</template>
```

示例 2 (vue) - 自定义内容：
```vue
<template>
  <div>
    <span>你看不见你自己，你所看见的只是你的影子。</span>
    <el-divider content-position="left">泰戈尔</el-divider>
    <span>
      我的愿望是傻瓜，他们大声叫嚷着穿过你的歌，
      我的主啊，让我只是倾听吧。
    </span>
    <el-divider>
      <el-icon><star-filled /></el-icon>
    </el-divider>
    <span>我不能选择那最好的。是那最好的选择我。</span>
    <el-divider content-position="right">泰戈尔</el-divider>
  </div>
</template>

<script lang="ts" setup>
import { StarFilled } from '@element-plus/icons-vue'
</script>
```

示例 3 (vue) - 虚线样式：
```vue
<template>
  <div>
    <span>大海啊，你说的是什么语言？</span>
    <el-divider border-style="dashed" />
    <span>永恒疑问的语言。</span>
  </div>
  <el-divider border-style="dotted" />
  <span>天空啊，你回答的是什么语言？</span>
  <el-divider border-style="double" />
  <span>永恒沉默的语言。</span>
</template>
```

示例 4 (vue) - 垂直分割线：
```vue
<template>
  <div>
    <span>雨</span>
    <el-divider direction="vertical" />
    <span>家</span>
    <el-divider direction="vertical" border-style="dashed" />
    <span>草</span>
  </div>
</template>
```

---
