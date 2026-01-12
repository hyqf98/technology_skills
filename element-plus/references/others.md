# Element-Plus - Others

**Pages:** 2

---

## Watermark

**URL:** llms-txt#watermark

**Contents:**
- Basic usage
- Multi-line watermark
- Image watermark
- Custom configuration
- API
  - Attributes
  - Font
  - Slots
- Vue Examples
  - basic.vue

Add specific text or patterns to the page.

The most basic usage.

## Multi-line watermark

Use `content` to set an array of strings to specify multi-line text watermark content.

Specify the image address via `image`. To ensure that the image is high definition and not stretched, set the width and height, and upload at least twice the width and height of the logo image address.

## Custom configuration

Preview the watermark effect by configuring custom parameters.

| Name    | Description                                                                                     | Type                          | Default                    |
| ------- | ----------------------------------------------------------------------------------------------- | ----------------------------- | -------------------------- |
| width   | The width of the watermark, the default value of `content` is its own width                     | ^[number]                     | 120                        |
| height  | The height of the watermark, the default value of `content` is its own height                   | ^[number]                     | 64                         |
| rotate  | When the watermark is drawn, the rotation Angle, unit `°`                                       | ^[number]                     | -22                        |
| z-index | The z-index of the appended watermark element                                                   | ^[number]                     | 9                          |
| image   | Image source, it is recommended to export 2x or 3x image, high priority                         | ^[string]                     | —                          |
| content | Watermark text content                                                                          | ^[string]/^[object]`string[]` | Element Plus               |
| font    | Text style                                                                                      | [Font](#font)                 | [Font](#font)              |
| gap     | The spacing between watermarks                                                                  | ^[object]`[number, number]`   | \[100, 100\]               |
| offset  | The offset of the watermark from the upper left corner of the container. The default is `gap/2` | ^[object]`[number, number]`   | \[gap\[0\]/2, gap\[1\]/2\] |

| Name              | Description   | Type                                                                                 | Default         |
| ----------------- | ------------- | ------------------------------------------------------------------------------------ | --------------- |
| color             | font color    | ^[string]                                                                            | rgba(0,0,0,.15) |
| fontSize          | font size     | ^[number] / ^[string]                                                                | 16              |
| fontWeight        | font weight   | ^[enum]`'normal' \| 'light' \| 'weight' \| number`                                   | normal          |
| fontFamily        | font family   | ^[string]                                                                            | sans-serif      |
| fontGap ^(2.11.5) | font gap      | ^[number]                                                                            | 3               |
| fontStyle         | font style    | ^[enum]`'none' \| 'normal' \| 'italic' \| 'oblique'`                                 | normal          |
| textAlign         | text align    | ^[enum]`'left' \| 'right' \| 'center' \| 'start' \| 'end' `                          | center          |
| textBaseline      | text baseline | ^[enum]`'top' \| 'hanging' \| 'middle' \| 'alphabetic' \| 'ideographic' \| 'bottom'` | hanging         |

| Name    | Description                    |
| ------- | ------------------------------ |
| default | container for adding watermark |

**Examples:**

Example 1 (vue):
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
  <el-watermark :font="font">
    <div style="height: 500px" />
  </el-watermark>
</template>
```

Example 2 (vue):
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
        <h2>A Vue 3 based component library for designers and developers</h2>
        <img src="/images/hamburger.png" alt="示例图片" />
      </div>
    </el-watermark>
    <el-form
      class="form"
      :model="config"
      label-position="top"
      label-width="50px"
    >
      <el-form-item label="Content">
        <el-input v-model="config.content" />
      </el-form-item>
      <el-form-item label="Color">
        <el-color-picker v-model="config.font.color" show-alpha />
      </el-form-item>
      <el-form-item label="FontSize">
        <el-slider v-model="config.font.fontSize" />
      </el-form-item>
      <el-form-item label="zIndex">
        <el-slider v-model="config.zIndex" />
      </el-form-item>
      <el-form-item label="Rotate">
        <el-slider v-model="config.rotate" :min="-180" :max="180" />
      </el-form-item>
      <el-form-item label="Gap">
        <el-space>
          <el-input-number v-model="config.gap[0]" controls-position="right" />
          <el-input-number v-model="config.gap[1]" controls-position="right" />
        </el-space>
      </el-form-item>
      <el-form-item label="Offset">
        <el-space>
          <el-input-number
            v-model="config.offset[0]"
            placeholder="offsetLeft"
            controls-position="right"
          />
          <el-input-number
            v-model="config.offset[1]"
            placeholder="offsetTop"
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

Example 3 (vue):
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

Example 4 (vue):
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
  <el-watermark :font="font" :content="['Element+', 'Element Plus']">
    <div style="height: 500px" />
  </el-watermark>
</template>
```

---

## Divider

**URL:** llms-txt#divider

**Contents:**
- Basic usage
- Custom content
- dashed line
- Vertical divider
- API
  - Attributes
  - Slots
- Vue Examples
  - basic-usage.vue
  - custom-content.vue

The dividing line that separates the content.

Divide the text of different paragraphs.

You can customize the content on the divider line.

You can set the style of divider.

| Name             | Description                                                | Type                                                                                                                                        | Default    |
| ---------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| direction        | Set divider's direction                                    | ^[enum]`'horizontal' \| 'vertical'`                                                                                                         | horizontal |
| border-style     | Set the style of divider                                   | ^[enum]`'none' \| 'solid' \| 'hidden' \| 'dashed' \| ...` [css/border-style](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-style) | solid      |
| content-position | the position of the customized content on the divider line | ^[enum]`'left' \| 'right' \| 'center' `                                                                                                     | center     |

| Name    | Description                            |
| ------- | -------------------------------------- |
| default | customized content on the divider line |

### custom-content.vue

### vertical-divider.vue

---
Title: Drawer
URL: https://element-plus.org/en-US/component/drawer
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <div>
    <span>
      I sit at my window this morning where the world like a passer-by stops for
      a moment, nods to me and goes.
    </span>
    <el-divider />
    <span>
      There little thoughts are the rustle of leaves; they have their whisper of
      joy in my mind.
    </span>
  </div>
</template>
```

Example 2 (vue):
```vue
<template>
  <div>
    <span>What you are you do not see, what you see is your shadow. </span>
    <el-divider content-position="left">Rabindranath Tagore</el-divider>
    <span>
      My wishes are fools, they shout across thy song, my Master. Let me but
      listen.
    </span>
    <el-divider>
      <el-icon><star-filled /></el-icon>
    </el-divider>
    <span>I cannot choose the best. The best chooses me.</span>
    <el-divider content-position="right">Rabindranath Tagore</el-divider>
  </div>
</template>

<script lang="ts" setup>
import { StarFilled } from '@element-plus/icons-vue'
</script>
```

Example 3 (vue):
```vue
<template>
  <div>
    <span>What language is thine, O sea?</span>
    <el-divider border-style="dashed" />
    <span>The language of eternal question.</span>
  </div>
  <el-divider border-style="dotted" />
  <span>What language is thy answer, O sky?</span>
  <el-divider border-style="double" />
  <span>The language of eternal silence.</span>
</template>
```

Example 4 (vue):
```vue
<template>
  <div>
    <span>Rain</span>
    <el-divider direction="vertical" />
    <span>Home</span>
    <el-divider direction="vertical" border-style="dashed" />
    <span>Grass</span>
  </div>
</template>
```

---
