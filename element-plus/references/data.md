# Element-Plus - Data

**Pages:** 11

---

## Skeleton

**URL:** llms-txt#skeleton

**Contents:**
- Basic usage
- Configurable Rows
- Animation
- Customized Template
- Loading state
- Rendering a list of data
- Avoiding rendering bouncing.
- Initial rendering loading ^(2.8.8)
- Toggle show/hide without rending bouncing ^(2.8.8)
- ## Skeleton API

When loading data, and you need a rich experience for visual and interactions for your end users, you can choose `skeleton`.

You can configure the row numbers yourself, for more precise rendering effect, the actual rendered row number will always be 1 row more than the given number, that is because we are rendering a title row with 33% width of the others.

We have provided a switch flag indicating whether showing the loading animation, called `animated` when this is true, all children of `el-skeleton` will show animation

## Customized Template

Element Plus only provides the most common template, sometimes that could be a problem, so you have a slot named `template` to do that work.

Also we have provided different types skeleton unit that you can choose, for more detailed info, please scroll down to the bottom of this page to see the API description. Also, when building your own customized skeleton structure, you should be structuring them as closer to the real DOM as possible, which avoiding the DOM bouncing caused by the height difference.

When `Loading` ends, we always need to show the real UI with data to our end users. with the attribute `loading` we can control whether showing the DOM. You can also use slot `default` to structure the real DOM element.

## Rendering a list of data

Most of the time, skeleton is used as indicators of rendering a list of data which haven't been fetched from server yet, then we need to create a list of skeleton out of no where to make it look like it is loading, with `count` attribute, you can control how many these templates you need to render to the browser.

## Avoiding rendering bouncing.

Sometimes API responds very quickly, when that happens, the skeleton just gets rendered to the DOM then it needs to switch back to real DOM, that causes the sudden flashy. To avoid such thing, you can use the `throttle` attribute.

## Initial rendering loading ^(2.8.8)

When the initial value of loading is true, you can set `throttle: {initVal: true, leading: xxx}` to control the immediate display of the initial skeleton screen without throttling.

## Toggle show/hide without rending bouncing ^(2.8.8)

Sometimes you want to render the business components more smoothly when loading toggle show or hide. You can use set the `throttle: {leading: xxx, trailing:xxx}` to control the rendering bouncing.

### Skeleton Attributes

| Name     | Description                                                                                                                                                                                                                                 | Type                                                                              | Default |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | ------- |
| animated | whether showing the animation                                                                                                                                                                                                               | ^[boolean]                                                                        | false   |
| count    | how many fake items to render to the DOM                                                                                                                                                                                                    | ^[number]                                                                         | 1       |
| loading  | whether showing the real DOM                                                                                                                                                                                                                | ^[boolean]                                                                        | false   |
| rows     | numbers of the row, only useful when no template slot were given                                                                                                                                                                            | ^[number]                                                                         | 3       |
| throttle | rendering delay in milliseconds. Numbers represent delayed display, and can also be set to delay hide, for example `{ leading: 500, trailing: 500 }`. When needing to control the initial value of loading, you can set `{ initVal: true }` | ^[number] / ^[object]`{ leading?: number, trailing?: number, initVal?: boolean }` | 0       |

| Name     | Description                            | Type                       |
| -------- | -------------------------------------- | -------------------------- |
| default  | real rendering DOM                     | ^[object]`$attrs`          |
| template | content as rendering skeleton template | ^[object]`{ key: number }` |

### SkeletonItem Attributes

| Name    | Description                         | Type                                                                                             | Default |
| ------- | ----------------------------------- | ------------------------------------------------------------------------------------------------ | ------- |
| variant | the current rendering skeleton type | ^[enum]`'p' \| 'text' \| 'h1' \| 'h3' \| 'caption' \| 'button' \| 'image' \| 'circle' \| 'rect'` | text    |

### avoiding-rendering-bouncing.vue

### configurable-rows.vue

### customized-template.vue

### initial-rendering-loading.vue

### leading-trailing-without-bouncing.vue

### loading-state.vue

### rendering-with-data.vue

---
Title: Slider
URL: https://element-plus.org/en-US/component/slider
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-skeleton :rows="5" animated />
</template>
```

Example 2 (vue):
```vue
<template>
  <el-space direction="vertical" alignment="flex-start">
    <div>
      <label style="margin-right: 16px">Switch Loading</label>
      <el-switch v-model="loading" />
    </div>
    <el-skeleton
      style="width: 240px"
      :loading="loading"
      animated
      :throttle="500"
    >
      <template #template>
        <el-skeleton-item variant="image" style="width: 240px; height: 265px" />
        <div style="padding: 14px">
          <el-skeleton-item variant="h3" style="width: 50%" />
          <div
            style="
              display: flex;
              align-items: center;
              justify-items: space-between;
              margin-top: 16px;
              height: 16px;
            "
          >
            <el-skeleton-item variant="text" style="margin-right: 16px" />
            <el-skeleton-item variant="text" style="width: 30%" />
          </div>
        </div>
      </template>
      <template #default>
        <el-card :body-style="{ padding: '0px', marginBottom: '1px' }">
          <img
            src="https://shadow.elemecdn.com/app/element/hamburger.9cf7b091-55e9-11e9-a976-7f4d0b07eef6.png"
            class="image"
          />
          <div style="padding: 14px">
            <span>Delicious hamburger</span>
            <div class="bottom card-header">
              <div class="time">{{ currentDate }}</div>
              <el-button text class="button">operation button</el-button>
            </div>
          </div>
        </el-card>
      </template>
    </el-skeleton>
  </el-space>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const loading = ref(false)
const currentDate = new Date().toDateString()
</script>
```

Example 3 (vue):
```vue
<template>
  <el-skeleton />
  <br />
  <el-skeleton style="--el-skeleton-circle-size: 100px">
    <template #template>
      <el-skeleton-item variant="circle" />
    </template>
  </el-skeleton>
</template>
```

Example 4 (vue):
```vue
<template>
  <el-skeleton :rows="5" />
</template>
```

---

## Empty

**URL:** llms-txt#empty

**Contents:**
- Basic usage
- Custom image
- Image size
- Bottom content
- Custom styles
  - Default Variables
- API
  - Attributes
  - Slots
- Vue Examples

Placeholder hints for empty states.

Use `image` prop to set image URL.

Use `image-size` prop to control image size.

Use the default slot to insert content at the bottom.

Now you can set custom style for empty component.
Use `css/scss` language to change the global or local color. We set some global color variables: `--el-empty-fill-color-0`, `--el-empty-fill-color-1`, `--el-empty-fill-color-2`, ......, `--el-empty-fill-color-9`. You can use like: `:root { --el-empty-fill-color-0: red; --el-empty-fill-color-1: blue; }`.
But usually, if you want to change style, you need to change all color, because these colors are a combination.

### Default Variables

| Variable                | Color                 |
| ----------------------- | --------------------- |
| --el-empty-fill-color-0 | var(--el-color-white) |
| --el-empty-fill-color-1 | #fcfcfd               |
| --el-empty-fill-color-2 | #f8f9fb               |
| --el-empty-fill-color-3 | #f7f8fc               |
| --el-empty-fill-color-4 | #eeeff3               |
| --el-empty-fill-color-5 | #edeef2               |
| --el-empty-fill-color-6 | #e9ebef               |
| --el-empty-fill-color-7 | #e5e7e9               |
| --el-empty-fill-color-8 | #e0e3e9               |
| --el-empty-fill-color-9 | #d5d7de               |

| Name        | Description                 | Type      | Default |
| ----------- | --------------------------- | --------- | ------- |
| image       | image URL of empty          | ^[string] | ''      |
| image-size  | image size (width) of empty | ^[number] | —       |
| description | description of empty        | ^[string] | ''      |

| Name        | Description               |
| ----------- | ------------------------- |
| default     | content as bottom content |
| image       | content as image          |
| description | content as description    |

### bottom-content.vue

---
Title: Form
URL: https://element-plus.org/en-US/component/form
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-empty description="description" />
</template>
```

Example 2 (vue):
```vue
<template>
  <el-empty>
    <el-button type="primary">Button</el-button>
  </el-empty>
</template>
```

Example 3 (vue):
```vue
<template>
  <el-empty
    image="https://shadow.elemecdn.com/app/element/hamburger.9cf7b091-55e9-11e9-a976-7f4d0b07eef6.png"
  />
</template>
```

Example 4 (vue):
```vue
<template>
  <el-empty :image-size="200" />
</template>
```

---

## Tour

**URL:** llms-txt#tour

**Contents:**
- Basic usage
- Non modal
- Placement
- Custom mask style
- Custom indicator
- Target
- Tour API
  - Tour Attributes
  - Tour slots
  - Tour events

A popup component for guiding users through a product. Use when you want to guide users through a product.

Use `:mask="false"` to make Tour non-modal. At the meantime it is recommended to use with `type="primary"` to emphasize the guide itself.

Change the placement of the guide relative to the target, there are 12 placements available. When `target` is empty the guide will show in the center.

Various parameter passing types of target. The string and Function types are supported since ^(2.5.2).

| Property                  | Description                                                                      | Type                                                                                                                                                                        | Default                            |
| ------------------------- | -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| append-to                 | which element the TourContent appends to                                         | ^[CSSSelector] / ^[HTMLElement]                                                                                                                                             | `body`                             |
| show-arrow                | whether to show the arrow                                                        | `boolean`                                                                                                                                                                   | true                               |
| placement                 | position of the guide card relative to the target element                        | ^[enum]`'top' \| 'top-start' \| 'top-end' \| 'bottom' \| 'bottom-start' \| 'bottom-end' \| 'left' \| 'left-start' \| 'left-end' \| 'right' \| 'right-start' \| 'right-end'` | `bottom`                           |
| content-style             | custom style for content                                                         | `CSSProperties`                                                                                                                                                             | —                                  |
| mask                      | whether to enable masking, change mask style and fill color by pass custom props | `boolean` \| ^[Object]`{ style?: CSSProperties; color?: string; }`                                                                                                          | `true`                             |
| gap                       | transparent gap between mask and target                                          | `TourGap`                                                                                                                                                                   | ^[Object]`{ offset: 6, radius: 2}` |
| type                      | type, affects the background color and text color                                | `default` \| `primary`                                                                                                                                                      | `default`                          |
| model-value / v-model     | open tour                                                                        | `boolean`                                                                                                                                                                   | `false`                            |
| current / v-model:current | what is the current step                                                         | `number`                                                                                                                                                                    | `0`                                |
| scroll-into-view-options  | support pass custom scrollIntoView options                                       | `boolean` \| `ScrollIntoViewOptions`                                                                                                                                        | ^[Object]`{ block: 'center' }`     |
| z-index                   | Tour's zIndex                                                                    | `number`                                                                                                                                                                    | `2001`                             |
| show-close                | whether to show a close button                                                   | `boolean`                                                                                                                                                                   | `true`                             |
| close-icon                | custom close icon, default is Close                                              | `string` \| `Component`                                                                                                                                                     | —                                  |
| close-on-press-escape     | whether the Dialog can be closed by pressing ESC                                 | `boolean`                                                                                                                                                                   | `true`                             |
| target-area-clickable     | whether the target element can be clickable, when using mask                     | `boolean`                                                                                                                                                                   | `true`                             |

| Name       | Description             | Type                                          |
| ---------- | ----------------------- | --------------------------------------------- |
| default    | tourStep component list | —                                             |
| indicators | custom indicator        | ^[object]`{ current: number, total: number }` |

| Name   | Description                    | Type                                   |
| ------ | ------------------------------ | -------------------------------------- |
| close  | callback function on shutdown  | ^[Function]`(current: number) => void` |
| finish | callback function on finished  | ^[Function]`() => void`                |
| change | callback when the step changes | ^[Function]`(current: number) => void` |

### TourStep Attributes

| Property                 | Description                                                                                                                                                                                            | Type                                                                                                                                                                        | Default   |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| target                   | get the element the guide card points to. Empty makes it show in center of screen. the string and Function types are supported since ^(2.5.2). the string type is selectors of document.querySelector. | `HTMLElement` \| `string` \| ^[Function]`() => HTMLElement`                                                                                                                 | —         |
| show-arrow               | whether to show the arrow                                                                                                                                                                              | `boolean`                                                                                                                                                                   | —         |
| title                    | title                                                                                                                                                                                                  | `string`                                                                                                                                                                    | —         |
| description              | description                                                                                                                                                                                            | `string`                                                                                                                                                                    | —         |
| placement                | position of the guide card relative to the target element                                                                                                                                              | ^[enum]`'top' \| 'top-start' \| 'top-end' \| 'bottom' \| 'bottom-start' \| 'bottom-end' \| 'left' \| 'left-start' \| 'left-end' \| 'right' \| 'right-start' \| 'right-end'` | `bottom`  |
| content-style            | custom style for content                                                                                                                                                                               | `CSSProperties`                                                                                                                                                             | —         |
| mask                     | whether to enable masking, change mask style and fill color by pass custom props                                                                                                                       | `boolean` \| ^[Object]`{ style?: CSSProperties; color?: string; }`                                                                                                          | —         |
| type                     | type, affects the background color and text color                                                                                                                                                      | `default` \| `primary`                                                                                                                                                      | `default` |
| next-button-props        | properties of the Next button                                                                                                                                                                          | ^[Object]`{ children: VueNode \| string; onClick: Function }`                                                                                                               | —         |
| prev-button-props        | properties of the previous button                                                                                                                                                                      | ^[Object]`{ children: VueNode \| string; onClick: Function }`                                                                                                               | —         |
| scroll-into-view-options | support pass custom scrollIntoView options, the default follows the `scrollIntoViewOptions` property of Tour                                                                                           | `boolean` \| `ScrollIntoViewOptions`                                                                                                                                        | —         |
| show-close               | whether to show a close button                                                                                                                                                                         | `boolean`                                                                                                                                                                   | —         |
| close-icon               | custom close icon, default is Close                                                                                                                                                                    | `string` \| `Component`                                                                                                                                                     | —         |

| Name    | Description           |
| ------- | --------------------- |
| default | custom description    |
| header  | custom header content |

| Name  | Description                   | Arguments               |
| ----- | ----------------------------- | ----------------------- |
| close | callback function on shutdown | ^[Function]`() => void` |

---
Title: Transfer
URL: https://element-plus.org/en-US/component/transfer
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-button type="primary" @click="open = true">Begin Tour</el-button>

  <el-divider />

  <el-space>
    <el-button ref="ref1">Upload</el-button>
    <el-button ref="ref2" type="primary">Save</el-button>
    <el-button ref="ref3" :icon="MoreFilled" />
  </el-space>

  <el-tour v-model="open">
    <el-tour-step :target="ref1?.$el" title="Upload File">
      <img
        style="width: 240px"
        src="https://element-plus.org/images/element-plus-logo.svg"
        alt="tour.png"
      />
      <div>Put you files here.</div>
    </el-tour-step>
    <el-tour-step
      :target="ref2?.$el"
      title="Save"
      description="Save your changes"
    />
    <el-tour-step
      :target="ref3?.$el"
      title="Other Actions"
      description="Click to see other"
    />
  </el-tour>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { MoreFilled } from '@element-plus/icons-vue'

import type { ButtonInstance } from 'element-plus'

const ref1 = ref<ButtonInstance>()
const ref2 = ref<ButtonInstance>()
const ref3 = ref<ButtonInstance>()

const open = ref(false)
</script>
```

Example 2 (vue):
```vue
<template>
  <el-button type="primary" @click="open = true">Begin Tour</el-button>

  <el-divider />

  <el-space>
    <el-button ref="ref1">Upload</el-button>
    <el-button ref="ref2" type="primary">Save</el-button>
    <el-button ref="ref3" :icon="MoreFilled" />
  </el-space>

  <el-tour v-model="open">
    <el-tour-step
      :target="ref1?.$el"
      title="Upload File"
      description="Put you files here."
    />
    <el-tour-step
      :target="ref2?.$el"
      title="Save"
      description="Save your changes"
    />
    <el-tour-step
      :target="ref3?.$el"
      title="Other Actions"
      description="Click to see other"
    />
    <template #indicators="{ current, total }">
      <span>{{ current + 1 }} / {{ total }}</span>
    </template>
  </el-tour>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { MoreFilled } from '@element-plus/icons-vue'

import type { ButtonInstance } from 'element-plus'

const ref1 = ref<ButtonInstance>()
const ref2 = ref<ButtonInstance>()
const ref3 = ref<ButtonInstance>()

const open = ref(false)
</script>
```

Example 3 (vue):
```vue
<template>
  <el-button type="primary" @click="open = true">Begin Tour</el-button>

  <el-divider />

  <el-space>
    <el-button ref="ref1">Upload</el-button>
    <el-button ref="ref2" type="primary">Save</el-button>
    <el-button ref="ref3" :icon="MoreFilled" />
  </el-space>

  <el-tour
    v-model="open"
    :mask="{
      style: {
        boxShadow: 'inset 0 0 15px #333',
      },
      color: 'rgba(80, 255, 255, .4)',
    }"
  >
    <el-tour-step :target="ref1?.$el" title="Upload File">
      <img
        src="https://element-plus.org/images/element-plus-logo.svg"
        alt="tour.png"
      />
      <div>Put you files here.</div>
    </el-tour-step>
    <el-tour-step
      :target="ref2?.$el"
      title="Save"
      description="Save your changes"
      :mask="{
        style: {
          boxShadow: 'inset 0 0 15px #fff',
        },
        color: 'rgba(40, 0, 255, .4)',
      }"
    />
    <el-tour-step
      :target="ref3?.$el"
      title="Other Actions"
      description="Click to see other"
      :mask="false"
    />
  </el-tour>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { MoreFilled } from '@element-plus/icons-vue'

import type { ButtonInstance } from 'element-plus'

const ref1 = ref<ButtonInstance>()
const ref2 = ref<ButtonInstance>()
const ref3 = ref<ButtonInstance>()

const open = ref(false)
</script>
```

Example 4 (vue):
```vue
<template>
  <el-button type="primary" @click="open = true">Begin Tour</el-button>

  <el-divider />

  <el-space>
    <el-button ref="ref1">Upload</el-button>
    <el-button ref="ref2" type="primary">Save</el-button>
    <el-button ref="ref3" :icon="MoreFilled" />
  </el-space>

  <el-tour v-model="open" type="primary" :mask="false">
    <el-tour-step
      :target="ref1?.$el"
      title="Upload File"
      description="Put you files here."
    />
    <el-tour-step
      :target="ref2?.$el"
      title="Save"
      description="Save your changes"
    />
    <el-tour-step
      :target="ref3?.$el"
      title="Other Actions"
      description="Click to see other"
    />
  </el-tour>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { MoreFilled } from '@element-plus/icons-vue'

import type { ButtonInstance } from 'element-plus'

const ref1 = ref<ButtonInstance>()
const ref2 = ref<ButtonInstance>()
const ref3 = ref<ButtonInstance>()

const open = ref(false)
</script>
```

---

## Infinite Scroll

**URL:** llms-txt#infinite-scroll

**Contents:**
- Basic usage
- Disable Loading
- Directives
- Vue Examples
  - basic.vue
  - disable-loading.vue

Load more data while reach bottom of the page

Add `v-infinite-scroll` to the list to automatically execute loading method when scrolling to the bottom.

| Name                      | Description                                                                                                      | Type        | Default |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------- | ----------- | ------- |
| v-infinite-scroll         | Load more data while reach bottom of the page                                                                    | ^[Function] | —       |
| infinite-scroll-disabled  | is disabled                                                                                                      | ^[boolean]  | false   |
| infinite-scroll-delay     | throttle delay (ms)                                                                                              | ^[number]   | 200     |
| infinite-scroll-distance  | trigger distance (px)                                                                                            | ^[number]   | 0       |
| infinite-scroll-immediate | Whether to execute the loading method immediately, in case the content cannot be filled up in the initial state. | ^[boolean]  | true    |

### disable-loading.vue

---
Title: Input Number
URL: https://element-plus.org/en-US/component/input-number
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <ul v-infinite-scroll="load" class="infinite-list" style="overflow: auto">
    <li v-for="i in count" :key="i" class="infinite-list-item">{{ i }}</li>
  </ul>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const count = ref(0)
const load = () => {
  count.value += 2
}
</script>

<style>
.infinite-list {
  height: 300px;
  padding: 0;
  margin: 0;
  list-style: none;
}
.infinite-list .infinite-list-item {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 50px;
  background: var(--el-color-primary-light-9);
  margin: 10px;
  color: var(--el-color-primary);
}
.infinite-list .infinite-list-item + .list-item {
  margin-top: 10px;
}
</style>
```

Example 2 (vue):
```vue
<template>
  <div class="infinite-list-wrapper" style="overflow: auto">
    <ul
      v-infinite-scroll="load"
      class="list"
      :infinite-scroll-disabled="disabled"
    >
      <li v-for="i in count" :key="i" class="list-item">{{ i }}</li>
    </ul>
    <p v-if="loading">Loading...</p>
    <p v-if="noMore">No more</p>
  </div>
</template>

<script lang="ts" setup>
import { computed, ref } from 'vue'

const count = ref(10)
const loading = ref(false)
const noMore = computed(() => count.value >= 20)
const disabled = computed(() => loading.value || noMore.value)
const load = () => {
  loading.value = true
  setTimeout(() => {
    count.value += 2
    loading.value = false
  }, 2000)
}
</script>

<style>
.infinite-list-wrapper {
  height: 300px;
  text-align: center;
}
.infinite-list-wrapper .list {
  padding: 0;
  margin: 0;
  list-style: none;
}

.infinite-list-wrapper .list-item {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 50px;
  background: var(--el-color-danger-light-9);
  color: var(--el-color-danger);
}
.infinite-list-wrapper .list-item + .list-item {
  margin-top: 10px;
}
</style>
```

---

## Image

**URL:** llms-txt#image

**Contents:**
- Basic Usage
- Placeholder
- Load Failed
- Lazy Load
- Image Preview
- Manually Open Preview ^(2.9.4)
- Custom Toolbar ^(2.9.4)
- Custom progress ^(2.9.4)
- Image API
  - Image Attributes

Besides the native features of img, support lazy load, custom placeholder and load failure, etc.

## Manually Open Preview ^(2.9.4)

## Custom Toolbar ^(2.9.4)

## Custom progress ^(2.9.4)

| Name                   | Description                                                                                                                                       | Type                                                                    | Default |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- | ------- |
| src                    | image source, same as native.                                                                                                                     | ^[string]                                                               | ''      |
| fit                    | indicate how the image should be resized to fit its container, same as [object-fit](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit). | ^[enum]`'' \| 'fill' \| 'contain' \| 'cover' \| 'none' \| 'scale-down'` | ''      |
| hide-on-click-modal    | when enabling preview, use this flag to control whether clicking on backdrop can exit preview mode.                                               | ^[boolean]                                                              | false   |
| loading ^(2.2.3)       | Indicates how the browser should load the image, same as [native](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#attr-loading).    | ^[enum]`'eager' \| 'lazy'`                                              | —       |
| lazy                   | whether to use lazy load.                                                                                                                         | ^[boolean]                                                              | false   |
| scroll-container       | the container to add scroll listener when using lazy load. By default, the container to add scroll listener when using lazy load.                 | ^[string] / ^[object]`HTMLElement`                                      | —       |
| alt                    | native attribute `alt`.                                                                                                                           | ^[string]                                                               | —       |
| referrerpolicy         | native attribute [referrerPolicy](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/referrerPolicy).                              | ^[string]                                                               | —       |
| crossorigin            | native attribute [crossorigin](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/crossorigin).                                         | ^[enum]`'' \| 'anonymous' \| 'use-credentials'`                         | —       |
| preview-src-list       | allow big image preview.                                                                                                                          | ^[object]`string[]`                                                     | []      |
| z-index                | set image preview z-index.                                                                                                                        | ^[number]                                                               | —       |
| initial-index          | initial preview image index, less than the length of `url-list`.                                                                                  | ^[number]                                                               | 0       |
| close-on-press-escape  | whether the image-viewer can be closed by pressing ESC.                                                                                           | ^[boolean]                                                              | true    |
| preview-teleported     | whether to append image-viewer to body. A nested parent element attribute transform should have this attribute set to `true`.                     | ^[boolean]                                                              | false   |
| infinite               | whether the viewer preview is infinite.                                                                                                           | ^[boolean]                                                              | true    |
| zoom-rate              | the zoom rate of the image viewer zoom event.                                                                                                     | ^[number]                                                               | 1.2     |
| scale ^(2.11.3)        | the preview image scale.                                                                                                                          | ^[number]                                                               | 1       |
| min-scale ^(2.4.0)     | the min scale of the image viewer zoom event.                                                                                                     | ^[number]                                                               | 0.2     |
| max-scale ^(2.4.0)     | the max scale of the image viewer zoom event.                                                                                                     | ^[number]                                                               | 7       |
| show-progress ^(2.9.4) | whether to display the preview image progress content.                                                                                            | ^[boolean]                                                              | false   |

| Name   | Description                                                                                       | Type                                 |
| ------ | ------------------------------------------------------------------------------------------------- | ------------------------------------ |
| load   | same as native load.                                                                              | ^[Function]`(e: Event) => void`      |
| error  | same as native error.                                                                             | ^[Function]`(e: Event) => void`      |
| switch | trigger when switching images.                                                                    | ^[Function]`(index: number) => void` |
| close  | trigger when clicking on close button or when `hide-on-click-modal` enabled clicking on backdrop. | ^[Function]`() => void`              |
| show   | trigger when the viewer displays                                                                  | ^[Function]`() => void`              |

| Name                                      | Description                                                           | Type |
| ----------------------------------------- | --------------------------------------------------------------------- | ---- |
| placeholder                               | custom placeholder content when image hasn't loaded yet.              | -    |
| error                                     | custom image load failed content.                                     | -    |
| [image viewer slots](#image-viewer-slots) | when you allow big image preview, image viewer slots all can be used. | -    |

| Name                 | Description                     | Type                    |
| -------------------- | ------------------------------- | ----------------------- |
| showPreview ^(2.9.4) | manually open preview big image | ^[Function]`() => void` |

### Image Viewer Attributes

| Name                   | Description                                                                                                                   | Type                  | Default |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------- | --------------------- | ------- |
| url-list               | preview link list.                                                                                                            | ^[object]`string[]`   | []      |
| z-index                | preview backdrop z-index.                                                                                                     | ^[number] / ^[string] | —       |
| initial-index          | the initial preview image index, less than or equal to the length of `url-list`.                                              | ^[number]             | 0       |
| infinite               | whether preview is infinite.                                                                                                  | ^[boolean]            | true    |
| hide-on-click-modal    | whether user can emit close event when clicking backdrop.                                                                     | ^[boolean]            | false   |
| teleported             | whether to append image itself to body. A nested parent element attribute transform should have this attribute set to `true`. | ^[boolean]            | false   |
| zoom-rate ^(2.2.27)    | the zoom rate of the image viewer zoom event.                                                                                 | ^[number]             | 1.2     |
| scale ^(2.11.3)        | the preview image scale.                                                                                                      | ^[number]             | 1       |
| min-scale ^(2.4.0)     | the min scale of the image viewer zoom event.                                                                                 | ^[number]             | 0.2     |
| max-scale ^(2.4.0)     | the max scale of the image viewer zoom event.                                                                                 | ^[number]             | 7       |
| close-on-press-escape  | whether the image-viewer can be closed by pressing ESC.                                                                       | ^[boolean]            | true    |
| show-progress ^(2.9.4) | whether to display the preview image progress content                                                                         | ^[boolean]            | false   |

### Image Viewer Events

| Name             | Description                                                                                       | Type                                 |
| ---------------- | ------------------------------------------------------------------------------------------------- | ------------------------------------ |
| close            | trigger when clicking on close button or when `hide-on-click-modal` enabled clicking on backdrop. | ^[Function]`() => void`              |
| error ^(2.11.3)  | same as native error.                                                                             | ^[Function]`(e: Event) => void`      |
| switch           | trigger when switching images.                                                                    | ^[Function]`(index: number) => void` |
| rotate ^(2.3.13) | trigger when rotating images.                                                                     | ^[Function]`(deg: number) => void`   |

### Image Viewer Slots

| Name                   | Description                                                            | Type                                                                                                                                                                                                              |
| ---------------------- | ---------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| viewer                 | custom content                                                         | -                                                                                                                                                                                                                 |
| progress ^(2.9.4)      | custom progress content (Priority is higher than `show-progress` prop) | ^[object]`{ activeIndex: number, total: number }`                                                                                                                                                                 |
| toolbar ^(2.9.4)       | custom toolbar content                                                 | ^[object]`{actions: (action: ImageViewerAction, options?: ImageViewerActionOptions ) => void, prev: ()=> void, next: () => void,reset: () => void, activeIndex: number }, setActiveItem: (index: number) => void` |
| viewer-error ^(2.11.3) | custom image load failed content.                                      | ^[object]`{ activeIndex: number, src: string }`                                                                                                                                                                   |

### Image Viewer Exposes

| Name          | Description           | Type                                 |
| ------------- | --------------------- | ------------------------------------ |
| setActiveItem | manually switch image | ^[Function]`(index: number) => void` |

<details>
  <summary>Show declarations</summary>

### custom-progress.vue

### custom-toolbar.vue

### image-preview.vue

### manually-preview.vue

---
Title: Infinite Scroll
URL: https://element-plus.org/en-US/component/infinite-scroll
---

**Examples:**

Example 1 (ts):
```ts
type ImageViewerAction = 'zoomIn' | 'zoomOut' | 'clockwise' | 'anticlockwise'
type ImageViewerActionOptions = {
  enableTransition?: boolean
  zoomRate?: number
  rotateDeg?: number
}
```

Example 2 (vue):
```vue
<template>
  <div class="demo-image">
    <div v-for="fit in fits" :key="fit" class="block">
      <span class="demonstration">{{ fit }}</span>
      <el-image style="width: 100px; height: 100px" :src="url" :fit="fit" />
    </div>
  </div>
</template>

<script lang="ts" setup>
import type { ImageProps } from 'element-plus'

const fits = [
  'fill',
  'contain',
  'cover',
  'none',
  'scale-down',
] as ImageProps['fit'][]
const url =
  'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg'
</script>

<style scoped>
.demo-image .block {
  padding: 30px 0;
  text-align: center;
  border-right: solid 1px var(--el-border-color);
  display: inline-block;
  width: 20%;
  min-width: 100px;
  box-sizing: border-box;
  vertical-align: top;
}
.demo-image .block:last-child {
  border-right: none;
}
.demo-image .demonstration {
  display: block;
  color: var(--el-text-color-secondary);
  font-size: 14px;
  margin-bottom: 20px;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <div class="demo-image__custom-toolbar">
    <el-image
      style="width: 100px; height: 100px"
      :src="url"
      :preview-src-list="srcList"
      fit="cover"
    >
      <template #progress="{ activeIndex, total }">
        <span>{{ activeIndex + 1 + '-' + total }}</span>
      </template>
    </el-image>
  </div>
</template>

<script lang="ts" setup>
const url =
  'https://fuss10.elemecdn.com/a/3f/3302e58f9a181d2509f3dc0fa68b0jpeg.jpeg'
const srcList = [
  'https://fuss10.elemecdn.com/a/3f/3302e58f9a181d2509f3dc0fa68b0jpeg.jpeg',
  'https://fuss10.elemecdn.com/1/34/19aa98b1fcb2781c4fba33d850549jpeg.jpeg',
  'https://fuss10.elemecdn.com/0/6f/e35ff375812e6b0020b6b4e8f9583jpeg.jpeg',
  'https://fuss10.elemecdn.com/9/bb/e27858e973f5d7d3904835f46abbdjpeg.jpeg',
  'https://fuss10.elemecdn.com/d/e6/c4d93a3805b3ce3f323f7974e6f78jpeg.jpeg',
  'https://fuss10.elemecdn.com/3/28/bbf893f792f03a54408b3b7a7ebf0jpeg.jpeg',
  'https://fuss10.elemecdn.com/2/11/6535bcfb26e4c79b48ddde44f4b6fjpeg.jpeg',
]
</script>
```

Example 4 (vue):
```vue
<template>
  <div class="demo-image__custom-toolbar">
    <el-image
      style="width: 100px; height: 100px"
      :src="url"
      :preview-src-list="srcList"
      fit="cover"
      show-progress
    >
      <template
        #toolbar="{ actions, prev, next, reset, activeIndex, setActiveItem }"
      >
        <el-icon @click="prev"><Back /></el-icon>
        <el-icon @click="next"><Right /></el-icon>
        <el-icon @click="setActiveItem(srcList.length - 1)">
          <DArrowRight />
        </el-icon>
        <el-icon @click="actions('zoomOut')"><ZoomOut /></el-icon>
        <el-icon
          @click="actions('zoomIn', { enableTransition: false, zoomRate: 2 })"
        >
          <ZoomIn />
        </el-icon>
        <el-icon
          @click="
            actions('clockwise', { rotateDeg: 180, enableTransition: false })
          "
        >
          <RefreshRight />
        </el-icon>
        <el-icon @click="actions('anticlockwise')"><RefreshLeft /></el-icon>
        <el-icon @click="reset"><Refresh /></el-icon>
        <el-icon @click="download(activeIndex)"><Download /></el-icon>
      </template>
    </el-image>
  </div>
</template>

<script lang="ts" setup>
import { ElIcon } from 'element-plus'
import {
  Back,
  DArrowRight,
  Download,
  Refresh,
  RefreshLeft,
  RefreshRight,
  Right,
  ZoomIn,
  ZoomOut,
} from '@element-plus/icons-vue'

const url =
  'https://fuss10.elemecdn.com/a/3f/3302e58f9a181d2509f3dc0fa68b0jpeg.jpeg'
const srcList = [
  'https://fuss10.elemecdn.com/a/3f/3302e58f9a181d2509f3dc0fa68b0jpeg.jpeg',
  'https://fuss10.elemecdn.com/1/34/19aa98b1fcb2781c4fba33d850549jpeg.jpeg',
  'https://fuss10.elemecdn.com/0/6f/e35ff375812e6b0020b6b4e8f9583jpeg.jpeg',
  'https://fuss10.elemecdn.com/9/bb/e27858e973f5d7d3904835f46abbdjpeg.jpeg',
  'https://fuss10.elemecdn.com/d/e6/c4d93a3805b3ce3f323f7974e6f78jpeg.jpeg',
  'https://fuss10.elemecdn.com/3/28/bbf893f792f03a54408b3b7a7ebf0jpeg.jpeg',
  'https://fuss10.elemecdn.com/2/11/6535bcfb26e4c79b48ddde44f4b6fjpeg.jpeg',
]

const download = (index: number) => {
  const url = srcList[index]
  const suffix = url.slice(url.lastIndexOf('.'))
  const filename = Date.now() + suffix

  fetch(url)
    .then((response) => response.blob())
    .then((blob) => {
      const blobUrl = URL.createObjectURL(new Blob([blob]))
      const link = document.createElement('a')
      link.href = blobUrl
      link.download = filename
      document.body.appendChild(link)
      link.click()
      URL.revokeObjectURL(blobUrl)
      link.remove()
    })
}
</script>
```

---

## Result

**URL:** llms-txt#result

**Contents:**
- Basic usage
- Customized content
- API
  - Attributes
  - Slots
- Vue Examples
  - basic-usage.vue
  - customized-content.vue

Used to give feedback on the result of user's operation or access exception.

## Customized content

| Name      | Description         | Type                                                                       | Default |
| --------- | ------------------- | -------------------------------------------------------------------------- | ------- |
| title     | title of result     | ^[string]                                                                  | ''      |
| sub-title | sub title of result | ^[string]                                                                  | ''      |
| icon      | icon type of result | ^[enum]`'primary' (2.9.11) \| 'success' \| 'warning' \| 'info' \| 'error'` | info    |

| Name      | Description                  |
| --------- | ---------------------------- |
| icon      | content as result icon       |
| title     | content as result title      |
| sub-title | content as result sub title  |
| extra     | content as result extra area |

### customized-content.vue

---
Title: Scrollbar
URL: https://element-plus.org/en-US/component/scrollbar
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-row>
    <el-col :sm="12" :lg="6" :xl="4">
      <el-result
        icon="primary"
        title="Primary Tip"
        sub-title="Please follow the instructions"
      >
        <template #extra>
          <el-button type="primary">Back</el-button>
        </template>
      </el-result>
    </el-col>
    <el-col :sm="12" :lg="6" :xl="4">
      <el-result
        icon="success"
        title="Success Tip"
        sub-title="Please follow the instructions"
      >
        <template #extra>
          <el-button type="primary">Back</el-button>
        </template>
      </el-result>
    </el-col>
    <el-col :sm="12" :lg="6" :xl="4">
      <el-result
        icon="warning"
        title="Warning Tip"
        sub-title="Please follow the instructions"
      >
        <template #extra>
          <el-button type="primary">Back</el-button>
        </template>
      </el-result>
    </el-col>
    <el-col :sm="12" :lg="6" :xl="4">
      <el-result
        icon="error"
        title="Error Tip"
        sub-title="Please follow the instructions"
      >
        <template #extra>
          <el-button type="primary">Back</el-button>
        </template>
      </el-result>
    </el-col>
    <el-col :sm="12" :lg="6" :xl="4">
      <el-result icon="info" title="Info Tip">
        <template #sub-title>
          <p>Using slot as subtitle</p>
        </template>
        <template #extra>
          <el-button type="primary">Back</el-button>
        </template>
      </el-result>
    </el-col>
  </el-row>
</template>

<script setup lang="ts"></script>
```

Example 2 (vue):
```vue
<template>
  <el-result title="404" sub-title="Sorry, request error">
    <template #icon>
      <el-image
        src="https://shadow.elemecdn.com/app/element/hamburger.9cf7b091-55e9-11e9-a976-7f4d0b07eef6.png"
      />
    </template>
    <template #extra>
      <el-button type="primary">Back</el-button>
    </template>
  </el-result>
</template>
```

---

## Calendar

**URL:** llms-txt#calendar

**Contents:**
- Basic
- Custom Content
- Range
- Customize header
- Localization
- API
  - Attributes
  - Slots
  - Exposes
- Type Declarations

The default locale of is English, if you need to use other languages, please check [Internationalization](/en-US/guide/i18n)

Note, date time locale (month name, first day of the week ...) are also configured in localization.

| Name                  | Description                                                                                                                                                    | Type                   | Default |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------- | ------- |
| model-value / v-model | binding value                                                                                                                                                  | ^[Date]                | —       |
| range                 | time range, including start time and end time. Start time must be start day of week, end time must be end day of week, the time span cannot exceed two months. | ^[array]`[Date, Date]` | —       |

| Name      | Description                                                                                                                                                                                                                                               | Type                                                                                                                         |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| date-cell | `type` indicates which month the date belongs, optional values are prev-month, current-month, next-month; `isSelected` indicates whether the date is selected; `day` is the formatted date in the format `YYYY-MM-DD`; `date` is date the cell represents | ^[object]`{ data: { type: 'prev-month' \| 'current-month' \| 'next-month', isSelected: boolean, day: string, date: Date } }` |
| header    | content of the Calendar header                                                                                                                                                                                                                            | ^[object]`{ date: string }`                                                                                                  |

| Name                        | Description                                                            | Type                                                                                          |
| --------------------------- | ---------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| selectedDay                 | currently selected date                                                | ^[object]`ComputedRef<Dayjs \| undefined>`                                                    |
| pickDay                     | select a specific date                                                 | ^[Function]`(day: dayjs.Dayjs) => void`                                                       |
| selectDate                  | select date                                                            | ^[Function]`(type: CalendarDateType) => void`                                                 |
| calculateValidatedDateRange | Calculate the validate date range according to the start and end dates | ^[Function]`(startDayjs: dayjs.Dayjs, endDayjs: dayjs.Dayjs) => [dayjs.Dayjs, dayjs.Dayjs][]` |

<details>
  <summary>Show declarations</summary>

---
Title: Card
URL: https://element-plus.org/en-US/component/card
---

**Examples:**

Example 1 (ts):
```ts
type CalendarDateType =
  | 'prev-month'
  | 'next-month'
  | 'prev-year'
  | 'next-year'
  | 'today'
```

Example 2 (vue):
```vue
<template>
  <el-calendar v-model="value" />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref(new Date())
</script>
```

Example 3 (vue):
```vue
<template>
  <el-calendar>
    <template #date-cell="{ data }">
      <p :class="data.isSelected ? 'is-selected' : ''">
        {{ data.day.split('-').slice(1).join('-') }}
        {{ data.isSelected ? '✔️' : '' }}
      </p>
    </template>
  </el-calendar>
</template>

<style>
.is-selected {
  color: #1989fa;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <el-calendar ref="calendar">
    <template #header="{ date }">
      <span>Custom header content</span>
      <span>{{ date }}</span>
      <el-button-group>
        <el-button size="small" @click="selectDate('prev-year')">
          Previous Year
        </el-button>
        <el-button size="small" @click="selectDate('prev-month')">
          Previous Month
        </el-button>
        <el-button size="small" @click="selectDate('today')">Today</el-button>
        <el-button size="small" @click="selectDate('next-month')">
          Next Month
        </el-button>
        <el-button size="small" @click="selectDate('next-year')">
          Next Year
        </el-button>
      </el-button-group>
    </template>
  </el-calendar>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

import type { CalendarDateType, CalendarInstance } from 'element-plus'

const calendar = ref<CalendarInstance>()
const selectDate = (val: CalendarDateType) => {
  if (!calendar.value) return
  calendar.value.selectDate(val)
}
</script>
```

---

## Descriptions

**URL:** llms-txt#descriptions

**Contents:**
- Basic usage
- Sizes
- Vertical List
- Rowspan ^(2.8.1)
- Customized Style
- Descriptions API
  - Descriptions Attributes
  - Descriptions Slots
- DescriptionsItem API
  - DescriptionsItem Attributes

Display multiple fields in list form.

### Descriptions Attributes

| Name                 | Description                                | Type                                           | Default    |
| -------------------- | ------------------------------------------ | ---------------------------------------------- | ---------- |
| border               | with or without border                     | ^[boolean]                                     | false      |
| column               | numbers of `Descriptions Item` in one line | ^[number]                                      | 3          |
| direction            | direction of list                          | ^[enum]`'vertical' \| 'horizontal'`            | horizontal |
| size                 | size of list                               | ^[enum]`'' \| 'large' \| 'default' \| 'small'` | —          |
| title                | title text, display on the top left        | ^[string]                                      | ''         |
| extra                | extra text, display on the top right       | ^[string]                                      | ''         |
| label-width ^(2.8.8) | label width of every column                | ^[string] / ^[number]                          | —          |

### Descriptions Slots

| Name    | Description                                 | Subtags           |
| ------- | ------------------------------------------- | ----------------- |
| default | customize default content                   | Descriptions Item |
| title   | custom title, display on the top left       | —                 |
| extra   | custom extra area, display on the top right | —                 |

## DescriptionsItem API

### DescriptionsItem Attributes

| Name                 | Description                                                                                                                                                                                  | Type                                   | Default |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- | ------- |
| label                | label text                                                                                                                                                                                   | ^[string]                              | ''      |
| span                 | colspan of column                                                                                                                                                                            | ^[number]                              | 1       |
| rowspan ^(2.8.1)     | the number of rows a cell should span                                                                                                                                                        | ^[number]                              | 1       |
| width                | column width, the width of the same column in different rows is set by the max value (If no `border`, width contains label and content)                                                      | ^[string] / ^[number]                  | ''      |
| min-width            | column minimum width, columns with `width` has a fixed width, while columns with `min-width` has a width that is distributed in proportion (If no`border`, width contains label and content) | ^[string] / ^[number]                  | ''      |
| label-width ^(2.8.8) | column label width, if not set, it will be the same as the width of the column. Higher priority than the `label-width` of `Descriptions`                                                     | ^[string] / ^[number]                  | —       |
| align                | column content alignment (If no `border`, effective for both label and content)                                                                                                              | ^[enum]`'left' \| 'center' \| 'right'` | left    |
| label-align          | column label alignment, if omitted, the value of the above `align` attribute will be applied (If no `border`, please use `align` attribute)                                                  | ^[enum]`'left' \| 'center' \| 'right'` | —       |
| class-name           | column content custom class name                                                                                                                                                             | ^[string]                              | ''      |
| label-class-name     | column label custom class name                                                                                                                                                               | ^[string]                              | ''      |

### DescriptionsItem Slots

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |
| label   | custom label              |

### customized-style.vue

### vertical-list.vue

---
Title: Dialog
URL: https://element-plus.org/en-US/component/dialog
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-descriptions title="User Info">
    <el-descriptions-item label="Username">kooriookami</el-descriptions-item>
    <el-descriptions-item label="Telephone">18100000000</el-descriptions-item>
    <el-descriptions-item label="Place">Suzhou</el-descriptions-item>
    <el-descriptions-item label="Remarks">
      <el-tag size="small">School</el-tag>
    </el-descriptions-item>
    <el-descriptions-item label="Address">
      No.1188, Wuzhong Avenue, Wuzhong District, Suzhou, Jiangsu Province
    </el-descriptions-item>
  </el-descriptions>
</template>
```

Example 2 (vue):
```vue
<template>
  <el-descriptions title="Customized style list" :column="3" border>
    <el-descriptions-item
      label="Username"
      label-align="right"
      align="center"
      label-class-name="my-label"
      class-name="my-content"
      width="150px"
    >
      kooriookami
    </el-descriptions-item>
    <el-descriptions-item label="Telephone" label-align="right" align="center">
      18100000000
    </el-descriptions-item>
    <el-descriptions-item label="Place" label-align="right" align="center">
      Suzhou
    </el-descriptions-item>
    <el-descriptions-item label="Remarks" label-align="right" align="center">
      <el-tag size="small">School</el-tag>
    </el-descriptions-item>
    <el-descriptions-item label="Address" label-align="right" align="center">
      No.1188, Wuzhong Avenue, Wuzhong District, Suzhou, Jiangsu Province
    </el-descriptions-item>
  </el-descriptions>
</template>

<style scoped>
:deep(.my-label) {
  background: var(--el-color-success-light-9) !important;
}
:deep(.my-content) {
  background: var(--el-color-danger-light-9);
}
</style>
```

Example 3 (vue):
```vue
<template>
  <el-descriptions title="Width horizontal list" border>
    <el-descriptions-item
      :rowspan="2"
      :width="140"
      label="Photo"
      align="center"
    >
      <el-image
        style="width: 100px; height: 100px"
        src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"
      />
    </el-descriptions-item>
    <el-descriptions-item label="Username">kooriookami</el-descriptions-item>
    <el-descriptions-item label="Telephone">18100000000</el-descriptions-item>
    <el-descriptions-item label="Place">Suzhou</el-descriptions-item>
    <el-descriptions-item label="Remarks">
      <el-tag size="small">School</el-tag>
    </el-descriptions-item>
    <el-descriptions-item label="Address">
      No.1188, Wuzhong Avenue, Wuzhong District, Suzhou, Jiangsu Province
    </el-descriptions-item>
  </el-descriptions>

  <el-descriptions
    title="Width vertical list"
    direction="vertical"
    border
    style="margin-top: 20px"
  >
    <el-descriptions-item
      :rowspan="2"
      :width="140"
      label="Photo"
      align="center"
    >
      <el-image
        style="width: 100px; height: 100px"
        src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"
      />
    </el-descriptions-item>
    <el-descriptions-item label="Username">kooriookami</el-descriptions-item>
    <el-descriptions-item label="Telephone">18100000000</el-descriptions-item>
    <el-descriptions-item label="Place">Suzhou</el-descriptions-item>
    <el-descriptions-item label="Remarks">
      <el-tag size="small">School</el-tag>
    </el-descriptions-item>
    <el-descriptions-item label="Address">
      No.1188, Wuzhong Avenue, Wuzhong District, Suzhou, Jiangsu Province
    </el-descriptions-item>
  </el-descriptions>
</template>
```

Example 4 (vue):
```vue
<template>
  <el-radio-group v-model="size">
    <el-radio value="large">Large</el-radio>
    <el-radio value="default">Default</el-radio>
    <el-radio value="small">Small</el-radio>
  </el-radio-group>

  <el-descriptions
    class="margin-top"
    title="With border"
    :column="3"
    :size="size"
    border
  >
    <template #extra>
      <el-button type="primary">Operation</el-button>
    </template>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <user />
          </el-icon>
          Username
        </div>
      </template>
      kooriookami
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <iphone />
          </el-icon>
          Telephone
        </div>
      </template>
      18100000000
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <location />
          </el-icon>
          Place
        </div>
      </template>
      Suzhou
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <tickets />
          </el-icon>
          Remarks
        </div>
      </template>
      <el-tag size="small">School</el-tag>
    </el-descriptions-item>
    <el-descriptions-item>
      <template #label>
        <div class="cell-item">
          <el-icon :style="iconStyle">
            <office-building />
          </el-icon>
          Address
        </div>
      </template>
      No.1188, Wuzhong Avenue, Wuzhong District, Suzhou, Jiangsu Province
    </el-descriptions-item>
  </el-descriptions>

  <el-descriptions
    class="margin-top"
    title="Without border"
    :column="3"
    :size="size"
    :style="blockMargin"
  >
    <template #extra>
      <el-button type="primary">Operation</el-button>
    </template>
    <el-descriptions-item label="Username">kooriookami</el-descriptions-item>
    <el-descriptions-item label="Telephone">18100000000</el-descriptions-item>
    <el-descriptions-item label="Place">Suzhou</el-descriptions-item>
    <el-descriptions-item label="Remarks">
      <el-tag size="small">School</el-tag>
    </el-descriptions-item>
    <el-descriptions-item label="Address">
      No.1188, Wuzhong Avenue, Wuzhong District, Suzhou, Jiangsu Province
    </el-descriptions-item>
  </el-descriptions>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import {
  Iphone,
  Location,
  OfficeBuilding,
  Tickets,
  User,
} from '@element-plus/icons-vue'

import type { ComponentSize } from 'element-plus'

const size = ref<ComponentSize>('default')

const iconStyle = computed(() => {
  const marginMap = {
    large: '8px',
    default: '6px',
    small: '4px',
  }
  return {
    marginRight: marginMap[size.value] || marginMap.default,
  }
})
const blockMargin = computed(() => {
  const marginMap = {
    large: '32px',
    default: '28px',
    small: '24px',
  }
  return {
    marginTop: marginMap[size.value] || marginMap.default,
  }
})
</script>

<style scoped>
.el-descriptions {
  margin-top: 20px;
}
.cell-item {
  display: flex;
  align-items: center;
}
.margin-top {
  margin-top: 20px;
}
</style>
```

---

## Statistic

**URL:** llms-txt#statistic

**Contents:**
- Basic usage
- Countdown
- Card usage
- Statistic API
  - Statistic Attributes
  - Statistic Slots
  - Statistic Exposes
- Countdown API
  - Countdown Attributes
  - Countdown Events

### Statistic Attributes

| Attribute         | Description                    | Type                                                                | Default |
| ----------------- | ------------------------------ | ------------------------------------------------------------------- | ------- |
| value             | Numerical content              | ^[number]                                                           | 0       |
| decimal-separator | Setting the decimal point      | ^[string]                                                           | .       |
| formatter         | Custom numerical presentation  | ^[Function]`(value: number) => string \| number`                    | —       |
| group-separator   | Sets the thousandth identifier | ^[string]                                                           | ,       |
| precision         | numerical precision            | ^[number]                                                           | 0       |
| prefix            | Sets the prefix of a number    | ^[string]                                                           | —       |
| suffix            | Sets the suffix of a number    | ^[string]                                                           | —       |
| title             | Numeric titles                 | ^[string]                                                           | —       |
| value-style       | Styles numeric values          | ^[string] / ^[object]`CSSProperties \| CSSProperties[] \| string[]` | —       |

| Name   | Description                 |
| ------ | --------------------------- |
| prefix | Numeric prefix              |
| suffix | Suffixes for numeric values |
| title  | Numeric titles              |

### Statistic Exposes

| Name         | Description           | Type                             |
| ------------ | --------------------- | -------------------------------- |
| displayValue | current display value | ^[object]`Ref<string \| number>` |

### Countdown Attributes

| Attribute   | Description                      | Type                                                                | Default  |
| ----------- | -------------------------------- | ------------------------------------------------------------------- | -------- |
| value       | target time                      | ^[number] / ^[Dayjs]                                                | —        |
| format      | Formatting the countdown display | ^[string]                                                           | HH:mm:ss |
| prefix      | Sets the prefix of a countdown   | ^[string]                                                           | —        |
| suffix      | Sets the suffix of a countdown   | ^[string]                                                           | —        |
| title       | countdown titles                 | ^[string]                                                           | —        |
| value-style | Styles countdown values          | ^[string] / ^[object]`CSSProperties \| CSSProperties[] \| string[]` | —        |

| Name   | Description                  | Type                                 |
| ------ | ---------------------------- | ------------------------------------ |
| change | Time difference change event | ^[Function]`(value: number) => void` |
| finish | countdown end event          | ^[Function]`() => void`              |

| Name   | Description            |
| ------ | ---------------------- |
| prefix | countdown value prefix |
| suffix | countdown value suffix |
| title  | countdown title        |

### Countdown Exposes

| Name         | Description           | Type                   |
| ------------ | --------------------- | ---------------------- |
| displayValue | current display value | ^[object]`Ref<string>` |

---
Title: Steps
URL: https://element-plus.org/en-US/component/steps
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-row :gutter="16">
    <el-col :xs="24" :sm="12" :md="6" class="text-center mb-4">
      <el-statistic title="Daily active users" :value="268500" />
    </el-col>
    <el-col :xs="24" :sm="12" :md="6" class="text-center mb-4">
      <el-statistic :value="138">
        <template #title>
          <div style="display: inline-flex; align-items: center">
            Ratio of men to women
            <el-icon style="margin-left: 4px" :size="12">
              <Male />
            </el-icon>
          </div>
        </template>
        <template #suffix>/100</template>
      </el-statistic>
    </el-col>
    <el-col :xs="24" :sm="12" :md="6" class="text-center mb-4">
      <el-statistic title="Total Transactions" :value="outputValue" />
    </el-col>
    <el-col :xs="24" :sm="12" :md="6" class="text-center mb-4">
      <el-statistic title="Feedback number" :value="562">
        <template #suffix>
          <el-icon style="vertical-align: -0.125em">
            <ChatLineRound />
          </el-icon>
        </template>
      </el-statistic>
    </el-col>
  </el-row>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { useTransition } from '@vueuse/core'
import { ChatLineRound, Male } from '@element-plus/icons-vue'

const source = ref(0)
const outputValue = useTransition(source, {
  duration: 1500,
})
source.value = 172000
</script>
```

Example 2 (vue):
```vue
<template>
  <el-row :gutter="16">
    <el-col :xs="24" :sm="12" :md="8" class="mb-4">
      <div class="statistic-card">
        <el-statistic :value="98500">
          <template #title>
            <div style="display: inline-flex; align-items: center">
              Daily active users
              <el-tooltip
                effect="dark"
                content="Number of users who logged into the product in one day"
                placement="top"
              >
                <el-icon style="margin-left: 4px" :size="12">
                  <Warning />
                </el-icon>
              </el-tooltip>
            </div>
          </template>
        </el-statistic>
        <div class="statistic-footer">
          <div class="footer-item">
            <span>than yesterday</span>
            <span class="green">
              24%
              <el-icon>
                <CaretTop />
              </el-icon>
            </span>
          </div>
        </div>
      </div>
    </el-col>
    <el-col :xs="24" :sm="12" :md="8" class="mb-4">
      <div class="statistic-card">
        <el-statistic :value="693700">
          <template #title>
            <div style="display: inline-flex; align-items: center">
              Monthly Active Users
              <el-tooltip
                effect="dark"
                content="Number of users who logged into the product in one month"
                placement="top"
              >
                <el-icon style="margin-left: 4px" :size="12">
                  <Warning />
                </el-icon>
              </el-tooltip>
            </div>
          </template>
        </el-statistic>
        <div class="statistic-footer">
          <div class="footer-item">
            <span>month on month</span>
            <span class="red">
              12%
              <el-icon>
                <CaretBottom />
              </el-icon>
            </span>
          </div>
        </div>
      </div>
    </el-col>
    <el-col :xs="24" :sm="12" :md="8" class="mb-4">
      <div class="statistic-card">
        <el-statistic :value="72000" title="New transactions today">
          <template #title>
            <div style="display: inline-flex; align-items: center">
              New transactions today
            </div>
          </template>
        </el-statistic>
        <div class="statistic-footer">
          <div class="footer-item">
            <span>than yesterday</span>
            <span class="green">
              16%
              <el-icon>
                <CaretTop />
              </el-icon>
            </span>
          </div>
          <div class="footer-item">
            <el-icon :size="14">
              <ArrowRight />
            </el-icon>
          </div>
        </div>
      </div>
    </el-col>
  </el-row>
</template>

<script lang="ts" setup>
import {
  ArrowRight,
  CaretBottom,
  CaretTop,
  Warning,
} from '@element-plus/icons-vue'
</script>

<style scoped>
:global(h2#card-usage ~ .example .example-showcase) {
  background-color: var(--el-fill-color) !important;
}

.el-statistic {
  --el-statistic-content-font-size: 28px;
}

.statistic-card {
  height: 100%;
  padding: 20px;
  border-radius: 4px;
  background-color: var(--el-bg-color-overlay);
}

.statistic-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  font-size: 12px;
  color: var(--el-text-color-regular);
  margin-top: 16px;
}

.statistic-footer .footer-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.statistic-footer .footer-item span:last-child {
  display: inline-flex;
  align-items: center;
  margin-left: 4px;
}

.green {
  color: var(--el-color-success);
}
.red {
  color: var(--el-color-error);
}
</style>
```

Example 3 (vue):
```vue
<template>
  <el-row :gutter="16">
    <el-col :xs="24" :sm="12" :md="8" class="text-center mb-4">
      <el-countdown title="Start to grab" :value="value" />
    </el-col>
    <el-col :xs="24" :sm="12" :md="8" class="text-center mb-4">
      <el-countdown
        title="Remaining VIP time"
        format="HH:mm:ss"
        :value="value1"
      />
      <el-button class="countdown-footer" type="primary" @click="reset">
        Reset
      </el-button>
    </el-col>
    <el-col :xs="24" :sm="12" :md="8" class="text-center mb-4">
      <el-countdown format="DD [days] HH:mm:ss" :value="value2">
        <template #title>
          <div style="display: inline-flex; align-items: center">
            <el-icon style="margin-right: 4px" :size="12">
              <Calendar />
            </el-icon>
            Still to go until next month
          </div>
        </template>
      </el-countdown>
      <div class="countdown-footer">{{ value2.format('YYYY-MM-DD') }}</div>
    </el-col>
  </el-row>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import dayjs from 'dayjs'
import { Calendar } from '@element-plus/icons-vue'

const value = ref(Date.now() + 1000 * 60 * 60 * 7)
const value1 = ref(Date.now() + 1000 * 60 * 60 * 24 * 2)
const value2 = ref(dayjs().add(1, 'month').startOf('month'))

function reset() {
  value1.value = Date.now() + 1000 * 60 * 60 * 24 * 2
}
</script>

<style scoped>
.countdown-footer {
  margin-top: 8px;
}
</style>
```

---

## Collapse

**URL:** llms-txt#collapse

**Contents:**
- Basic usage
- Accordion
- Custom title
- Custom icon ^(2.8.3)
- Custom icon position ^(2.9.10)
- Prevent collapsing ^(2.9.11)
- Collapse API
  - Collapse Attributes
  - Collapse Events
  - Collapse Slots

Use Collapse to store contents.

You can expand multiple panels

In accordion mode, only one panel can be expanded at once

Besides using the `title` attribute, you can customize panel title with named slots, which makes adding custom content, e.g. icons, possible.

## Custom icon ^(2.8.3)

Besides using the `icon` attribute, you can customize icon of panel item with named slots, which makes adding custom content.

## Custom icon position ^(2.9.10)

using the `expand-icon-position` attribute, you can customize icon position.

## Prevent collapsing ^(2.9.11)

set the `before-collapse` property, If `false` is returned or a `Promise` is returned and then is rejected, will stop collapsing.

### Collapse Attributes

| Name                           | Description                                                                                                                                          | Type                                           | Default |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | ------- |
| model-value / v-model          | currently active panel, the type is `string` in accordion mode, otherwise it is `array`                                                              | ^[string] / ^[array]                           | []      |
| accordion                      | whether to activate accordion mode                                                                                                                   | ^[boolean]                                     | false   |
| expand-icon-position ^(2.9.10) | set expand icon position                                                                                                                             | ^[enum]`'left' \| 'right' `                    | right   |
| before-collapse ^(2.9.11)      | before-collapse hook before the collapse state changes. If `false` is returned or a `Promise` is returned and then is rejected, will stop collapsing | ^[Function]`() => Promise<boolean> \| boolean` | —       |

| Name   | Description                                                                                                   | Type                                                |
| ------ | ------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| change | triggers when active panels change, the parameter type is `string` in accordion mode, otherwise it is `array` | ^[Function]`(activeNames: array \| string) => void` |

| Name    | Description               | Subtags       |
| ------- | ------------------------- | ------------- |
| default | customize default content | Collapse Item |

| Name           | Description                  | Type                                                     |
| -------------- | ---------------------------- | -------------------------------------------------------- |
| activeNames    | currently active panel names | ^[object]`ComputedRef<(string \| number)[]>`             |
| setActiveNames | set active panel names       | ^[Function]`(activeNames: (string \| number)[]) => void` |

### Collapse Item Attributes

| Name          | Description                        | Type                     | Default    |
| ------------- | ---------------------------------- | ------------------------ | ---------- |
| name          | unique identification of the panel | ^[string] / ^[number]    | —          |
| title         | title of the panel                 | ^[string]                | ''         |
| icon ^(2.8.3) | icon of the collapse item          | ^[string] / ^[Component] | ArrowRight |
| disabled      | disable the collapse item          | ^[boolean]               | false      |

### Collapse Item Slot

| Name          | Description                    | Type                             |
| ------------- | ------------------------------ | -------------------------------- |
| default       | content of Collapse Item       | —                                |
| title         | content of Collapse Item title | ^[object]`{ isActive: boolean }` |
| icon ^(2.8.3) | content of Collapse Item icon  | ^[object]`{ isActive: boolean }` |

### Collapse Item Exposes

| Name     | Description                                 | Type                                         |
| -------- | ------------------------------------------- | -------------------------------------------- |
| isActive | whether the current collapse item is active | ^[object]`ComputedRef<boolean \| undefined>` |

### custom-icon-position.vue

### customization.vue

### prevent-collapsing.vue

---
Title: ColorPickerPanel
URL: https://element-plus.org/en-US/component/color-picker-panel
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <div class="demo-collapse">
    <el-collapse v-model="activeName" accordion>
      <el-collapse-item title="Consistency" name="1">
        <div>
          Consistent with real life: in line with the process and logic of real
          life, and comply with languages and habits that the users are used to;
        </div>
        <div>
          Consistent within interface: all elements should be consistent, such
          as: design style, icons and texts, position of elements, etc.
        </div>
      </el-collapse-item>
      <el-collapse-item title="Feedback" name="2">
        <div>
          Operation feedback: enable the users to clearly perceive their
          operations by style updates and interactive effects;
        </div>
        <div>
          Visual feedback: reflect current state by updating or rearranging
          elements of the page.
        </div>
      </el-collapse-item>
      <el-collapse-item title="Efficiency" name="3">
        <div>
          Simplify the process: keep operating process simple and intuitive;
        </div>
        <div>
          Definite and clear: enunciate your intentions clearly so that the
          users can quickly understand and make decisions;
        </div>
        <div>
          Easy to identify: the interface should be straightforward, which helps
          the users to identify and frees them from memorizing and recalling.
        </div>
      </el-collapse-item>
      <el-collapse-item title="Controllability" name="4">
        <div>
          Decision making: giving advices about operations is acceptable, but do
          not make decisions for the users;
        </div>
        <div>
          Controlled consequences: users should be granted the freedom to
          operate, including canceling, aborting or terminating current
          operation.
        </div>
      </el-collapse-item>
    </el-collapse>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const activeName = ref('1')
</script>
```

Example 2 (vue):
```vue
<template>
  <div class="demo-collapse">
    <el-collapse v-model="activeNames" @change="handleChange">
      <el-collapse-item title="Consistency" name="1">
        <div>
          Consistent with real life: in line with the process and logic of real
          life, and comply with languages and habits that the users are used to;
        </div>
        <div>
          Consistent within interface: all elements should be consistent, such
          as: design style, icons and texts, position of elements, etc.
        </div>
      </el-collapse-item>
      <el-collapse-item title="Feedback" name="2">
        <div>
          Operation feedback: enable the users to clearly perceive their
          operations by style updates and interactive effects;
        </div>
        <div>
          Visual feedback: reflect current state by updating or rearranging
          elements of the page.
        </div>
      </el-collapse-item>
      <el-collapse-item title="Efficiency" name="3">
        <div>
          Simplify the process: keep operating process simple and intuitive;
        </div>
        <div>
          Definite and clear: enunciate your intentions clearly so that the
          users can quickly understand and make decisions;
        </div>
        <div>
          Easy to identify: the interface should be straightforward, which helps
          the users to identify and frees them from memorizing and recalling.
        </div>
      </el-collapse-item>
      <el-collapse-item title="Controllability" name="4">
        <div>
          Decision making: giving advice about operations is acceptable, but do
          not make decisions for the users;
        </div>
        <div>
          Controlled consequences: users should be granted the freedom to
          operate, including canceling, aborting or terminating current
          operation.
        </div>
      </el-collapse-item>
    </el-collapse>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

import type { CollapseModelValue } from 'element-plus'

const activeNames = ref(['1'])
const handleChange = (val: CollapseModelValue) => {
  console.log(val)
}
</script>
```

Example 3 (vue):
```vue
<template>
  <div class="demo-collapse-position">
    <div class="flex items-center mb-4">
      <span class="mr-4">expand icon position: </span>
      <el-switch
        v-model="position"
        inactive-value="left"
        active-value="right"
        inactive-text="left"
        active-text="right"
        style="--el-switch-off-color: #88b8fe"
      />
    </div>

    <el-collapse :expand-icon-position="position">
      <el-collapse-item title="Consistency" name="1">
        <div>
          Consistent with real life: in line with the process and logic of real
          life, and comply with languages and habits that the users are used to;
        </div>
        <div>
          Consistent within interface: all elements should be consistent, such
          as: design style, icons and texts, position of elements, etc.
        </div>
      </el-collapse-item>
      <el-collapse-item title="Feedback" name="2">
        <div>
          Operation feedback: enable the users to clearly perceive their
          operations by style updates and interactive effects;
        </div>
        <div>
          Visual feedback: reflect current state by updating or rearranging
          elements of the page.
        </div>
      </el-collapse-item>
      <el-collapse-item title="Efficiency" name="3">
        <div>
          Simplify the process: keep operating process simple and intuitive;
        </div>
        <div>
          Definite and clear: enunciate your intentions clearly so that the
          users can quickly understand and make decisions;
        </div>
        <div>
          Easy to identify: the interface should be straightforward, which helps
          the users to identify and frees them from memorizing and recalling.
        </div>
      </el-collapse-item>
    </el-collapse>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

import type { CollapseIconPositionType } from 'element-plus'

const position = ref<CollapseIconPositionType>('left')
</script>
```

Example 4 (vue):
```vue
<template>
  <div class="demo-collapse">
    <el-collapse v-model="activeNames" @change="handleChange">
      <el-collapse-item title="Consistency" name="1" :icon="CaretRight">
        <div>
          Consistent with real life: in line with the process and logic of real
          life, and comply with languages and habits that the users are used to;
        </div>
        <div>
          Consistent within interface: all elements should be consistent, such
          as: design style, icons and texts, position of elements, etc.
        </div>
      </el-collapse-item>
      <el-collapse-item title="Feedback" name="2">
        <template #icon="{ isActive }">
          <span class="icon-ele">
            {{ isActive ? 'Expanded' : 'Collapsed' }}
          </span>
        </template>
        <div>
          Operation feedback: enable the users to clearly perceive their
          operations by style updates and interactive effects;
        </div>
        <div>
          Visual feedback: reflect current state by updating or rearranging
          elements of the page.
        </div>
      </el-collapse-item>
      <el-collapse-item title="Efficiency" name="3">
        <div>
          Simplify the process: keep operating process simple and intuitive;
        </div>
        <div>
          Definite and clear: enunciate your intentions clearly so that the
          users can quickly understand and make decisions;
        </div>
        <div>
          Easy to identify: the interface should be straightforward, which helps
          the users to identify and frees them from memorizing and recalling.
        </div>
      </el-collapse-item>
      <el-collapse-item title="Controllability" name="4">
        <div>
          Decision making: giving advices about operations is acceptable, but do
          not make decisions for the users;
        </div>
        <div>
          Controlled consequences: users should be granted the freedom to
          operate, including canceling, aborting or terminating current
          operation.
        </div>
      </el-collapse-item>
    </el-collapse>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { CaretRight } from '@element-plus/icons-vue'

import type { CollapseModelValue } from 'element-plus'

const activeNames = ref(['1'])
const handleChange = (val: CollapseModelValue) => {
  console.log(val)
}
</script>

<style scoped>
.icon-ele {
  margin: 0 8px 0 auto;
  color: #409eff;
}
</style>
```

---

## Table

**URL:** llms-txt#table

**Contents:**
- Basic table
- Striped Table
- Table with border
- Table with status
- Table with show overflow tooltip
- Table with fixed header
- Table with fixed column
- Table with fixed columns and header
- Fluid-height Table with fixed header (and columns)
- Grouping table head

Display multiple data with similar format. You can sort, filter, compare your data in a table.

Basic table is just for data display.

Striped table makes it easier to distinguish different rows.

You can highlight your table content to distinguish between "success, information, warning, danger" and other states.

## Table with show overflow tooltip

When the content is too long, it will break into multiple lines, you can use `show-overflow-tooltip` to keep it in one line.

## Table with fixed header

When there are too many rows, you can use a fixed header.

## Table with fixed column

When there are too many columns, you can fix some of them.

## Table with fixed columns and header

When you have huge chunks of data to put in a table, you can fix the header and columns at the same time.

## Fluid-height Table with fixed header (and columns)

When the the data is dynamically changed, you might want the table to have a maximum height rather than a fixed height and to show the scroll bar if needed.

## Grouping table head

When the data structure is complex, you can use group header to show the data hierarchy.

## Table with fixed group header

fixed group head is supported

Single row selection is supported.

You can also select multiple rows.

After ^(2.8.3), `toggleRowSelection` supports the third parameter `ignoreSelectable` to determine whether to ignore the selectable attribute.

Sort the data to find or compare data quickly.

Filter the table to find desired data.

## Custom column template

Customize table column so it can be integrated with other components.

## Table with custom header

Customize table header so it can be even more customized.

When the row content is too long and you do not want to display the horizontal scroll bar, you can use the expandable row feature.

After ^(2.9.7), `preserve-expanded-content` is added to control whether to preserve expanded row content in DOM when collapsed.

## Tree data and lazy mode

## Selectable tree ^(2.8.0)

For table of numbers, you can add an extra row at the table footer displaying each column's sum.

## Rowspan and colspan

Configuring rowspan and colspan allows you to merge cells

You can customize row index in `type=index` columns.

The [table-layout](https://developer.mozilla.org/en-US/docs/Web/CSS/table-layout) property sets the algorithm used to lay out table cells, rows, and columns.

## Tooltip formatter ^(2.9.4)

You can use `tooltip-formatter` to customize the tooltip content.

| Name                               | Description                                                                                                                                                                                                                                                                | Type                                                                                                                                                                 | Default                                                                                                                 |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| data                               | table data                                                                                                                                                                                                                                                                 | ^[array]`any[]`                                                                                                                                                      | []                                                                                                                      |
| height                             | table's height. By default it has an `auto` height. If its value is a number, the height is measured in pixels; if its value is a string, the value will be assigned to element's style.height, the height is affected by external styles                                  | ^[string] / ^[number]                                                                                                                                                | —                                                                                                                       |
| max-height                         | table's max-height. The legal value is a number or the height in px                                                                                                                                                                                                        | ^[string] / ^[number]                                                                                                                                                | —                                                                                                                       |
| stripe                             | whether Table is striped                                                                                                                                                                                                                                                   | ^[boolean]                                                                                                                                                           | false                                                                                                                   |
| border                             | whether Table has vertical border                                                                                                                                                                                                                                          | ^[boolean]                                                                                                                                                           | false                                                                                                                   |
| size                               | size of Table                                                                                                                                                                                                                                                              | ^[enum]`'' \| 'large' \| 'default' \| 'small'`                                                                                                                       | —                                                                                                                       |
| fit                                | whether width of column automatically fits its container                                                                                                                                                                                                                   | ^[boolean]                                                                                                                                                           | true                                                                                                                    |
| show-header                        | whether Table header is visible                                                                                                                                                                                                                                            | ^[boolean]                                                                                                                                                           | true                                                                                                                    |
| highlight-current-row              | whether current row is highlighted                                                                                                                                                                                                                                         | ^[boolean]                                                                                                                                                           | false                                                                                                                   |
| current-row-key                    | key of current row, a set only prop                                                                                                                                                                                                                                        | ^[string] / ^[number]                                                                                                                                                | —                                                                                                                       |
| row-class-name                     | function that returns custom class names for a row, or a string assigning class names for every row                                                                                                                                                                        | ^[Function]`(data: { row: any, rowIndex: number }) => string` / ^[string]                                                                                            | —                                                                                                                       |
| row-style                          | function that returns custom style for a row, or an object assigning custom style for every row                                                                                                                                                                            | ^[Function]`(data: { row: any, rowIndex: number }) => CSSProperties` / ^[object]`CSSProperties`                                                                      | —                                                                                                                       |
| cell-class-name                    | function that returns custom class names for a cell, or a string assigning class names for every cell                                                                                                                                                                      | ^[Function]`(data: { row: any, column: TableColumnCtx<T>, rowIndex: number, columnIndex: number }) => string` / ^[string]                                            | —                                                                                                                       |
| cell-style                         | function that returns custom style for a cell, or an object assigning custom style for every cell                                                                                                                                                                          | ^[Function]`(data: { row: any, column: TableColumnCtx<T>, rowIndex: number, columnIndex: number }) => CSSProperties` / ^[object]`CSSProperties`                      | —                                                                                                                       |
| header-row-class-name              | function that returns custom class names for a row in table header, or a string assigning class names for every row in table header                                                                                                                                        | ^[Function]`(data: { row: any, rowIndex: number }) => string` / ^[string]                                                                                            | —                                                                                                                       |
| header-row-style                   | function that returns custom style for a row in table header, or an object assigning custom style for every row in table header                                                                                                                                            | ^[Function]`(data: { row: any, rowIndex: number }) => CSSProperties` / ^[object]`CSSProperties`                                                                      | —                                                                                                                       |
| header-cell-class-name             | function that returns custom class names for a cell in table header, or a string assigning class names for every cell in table header                                                                                                                                      | ^[Function]`(data: { row: any, column: TableColumnCtx<T>, rowIndex: number, columnIndex: number }) => string` / ^[string]                                            | —                                                                                                                       |
| header-cell-style                  | function that returns custom style for a cell in table header, or an object assigning custom style for every cell in table header                                                                                                                                          | ^[Function]`(data: { row: any, column: TableColumnCtx<T>, rowIndex: number, columnIndex: number }) => CSSProperties` / ^[object]`CSSProperties`                      | —                                                                                                                       |
| row-key                            | key of row data, used for optimizing rendering. Required if `reserve-selection` is on or display tree data. When its type is String, multi-level access is supported, e.g. `user.info.id`, but `user.info[0].id` is not supported, in which case `Function` should be used | ^[function]`(row: any) => string` / ^[string]                                                                                                                        | —                                                                                                                       |
| empty-text                         | displayed text when data is empty. You can customize this area with `#empty`                                                                                                                                                                                               | ^[string]                                                                                                                                                            | No Data                                                                                                                 |
| default-expand-all                 | whether expand all rows by default, works when the table has a column type="expand" or contains tree structure data                                                                                                                                                        | ^[boolean]                                                                                                                                                           | false                                                                                                                   |
| expand-row-keys                    | set expanded rows by this prop, prop's value is the keys of expand rows, you should set row-key before using this prop.                                                                                                                                                    | ^[object]`Array<string>`                                                                                                                                             | —                                                                                                                       |
| default-sort                       | set the default sort column and order. property `prop` is used to set default sort column, property `order` is used to set default sort order                                                                                                                              | ^[object]`Sort`                                                                                                                                                      | if `prop` is set, and `order` is not set, then `order` is default to ascending                                          |
| tooltip-effect                     | the `effect` of the overflow tooltip                                                                                                                                                                                                                                       | ^[enum]`'dark' \| 'light'`                                                                                                                                           | dark                                                                                                                    |
| tooltip-options ^(2.2.28)          | the options for the overflow tooltip, [see the following tooltip component](tooltip.html#attributes)                                                                                                                                                                       | ^[object]`Pick<ElTooltipProps, 'effect' \| 'enterable' \| 'hideAfter' \| 'offset' \| 'placement' \| 'popperClass' \| 'popperOptions' \| 'showAfter' \| 'showArrow'>` | ^[object]`{ enterable: true, placement: 'top', showArrow: true, hideAfter: 200, popperOptions: { strategy: 'fixed' } }` |
| append-filter-panel-to ^(2.8.4)    | which element the filter panels appends to                                                                                                                                                                                                                                 | ^[string]                                                                                                                                                            | —                                                                                                                       |
| show-summary                       | whether to display a summary row                                                                                                                                                                                                                                           | ^[boolean]                                                                                                                                                           | false                                                                                                                   |
| sum-text                           | displayed text for the first column of summary row                                                                                                                                                                                                                         | ^[string]                                                                                                                                                            | Sum                                                                                                                     |
| summary-method                     | custom summary method                                                                                                                                                                                                                                                      | ^[Function]`(data: { columns: any[], data: any[] }) => (VNode \| string)[]`                                                                                          | —                                                                                                                       |
| span-method                        | method that returns rowspan and colspan                                                                                                                                                                                                                                    | ^[Function]`(data: { row: any, column: TableColumnCtx<T>, rowIndex: number, columnIndex: number }) => number[] \| { rowspan: number, colspan: number } \| void`      | —                                                                                                                       |
| select-on-indeterminate            | controls the behavior of master checkbox in multi-select tables when only some rows are selected (but not all). If true, all rows will be selected, else deselected                                                                                                        | ^[boolean]                                                                                                                                                           | true                                                                                                                    |
| indent                             | horizontal indentation of tree data                                                                                                                                                                                                                                        | ^[number]                                                                                                                                                            | 16                                                                                                                      |
| lazy                               | whether to lazy loading data                                                                                                                                                                                                                                               | ^[boolean]                                                                                                                                                           | false                                                                                                                   |
| load                               | method for loading child row data, only works when `lazy` is true                                                                                                                                                                                                          | ^[Function]`(row: any, treeNode: TreeNode, resolve: (data: any[]) => void) => void`                                                                                  | —                                                                                                                       |
| tree-props                         | configuration for rendering nested data                                                                                                                                                                                                                                    | ^[object]`{ hasChildren?: string, children?: string, checkStrictly?: boolean }`                                                                                      | ^[object]`{ hasChildren: 'hasChildren', children: 'children', checkStrictly: false }`                                   |
| table-layout                       | sets the algorithm used to lay out table cells, rows, and columns                                                                                                                                                                                                          | ^[enum]`'fixed' \| 'auto'`                                                                                                                                           | fixed                                                                                                                   |
| scrollbar-always-on                | always show scrollbar                                                                                                                                                                                                                                                      | ^[boolean]                                                                                                                                                           | false                                                                                                                   |
| show-overflow-tooltip              | whether to hide extra content and show them in a tooltip when hovering on the cell.It will affect all the table columns, refer to table [tooltip-options](#table-attributes)                                                                                               | ^[boolean] / [`object`](#table-attributes) ^(2.3.7)                                                                                                                  | —                                                                                                                       |
| flexible ^(2.2.1)                  | ensure main axis minimum-size doesn't follow the content                                                                                                                                                                                                                   | ^[boolean]                                                                                                                                                           | false                                                                                                                   |
| scrollbar-tabindex ^(2.8.3)        | body scrollbar's wrap container tabindex                                                                                                                                                                                                                                   | ^[string] / ^[number]                                                                                                                                                | —                                                                                                                       |
| allow-drag-last-column ^(2.9.2)    | whether to allow drag the last column                                                                                                                                                                                                                                      | ^[boolean]                                                                                                                                                           | true                                                                                                                    |
| tooltip-formatter ^(2.9.4)         | customize tooltip content when using `show-overflow-tooltip`                                                                                                                                                                                                               | ^[Function]`(data: { row: any, column: TableColumnCtx<T>, cellValue: any }) => VNode \| string`                                                                      | —                                                                                                                       |
| preserve-expanded-content ^(2.9.7) | whether to preserve expanded row content in DOM when collapsed                                                                                                                                                                                                             | ^[boolean]                                                                                                                                                           | false                                                                                                                   |
| native-scrollbar ^(2.10.5)         | whether to use native scrollbars                                                                                                                                                                                                                                           | ^[boolean]                                                                                                                                                           | false                                                                                                                   |

| Name               | Description                                                                                                                                  | Type                                                                                                    |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| select             | triggers when user clicks the checkbox in a row                                                                                              | ^[Function]`<T = any>(selection: T[], row: T) => void`                                                  |
| select-all         | triggers when user clicks the checkbox in table header                                                                                       | ^[Function]`(selection: any[]) => void`                                                                 |
| selection-change   | triggers when selection changes                                                                                                              | ^[Function]`(newSelection: any[]) => void`                                                              |
| cell-mouse-enter   | triggers when hovering into a cell                                                                                                           | ^[Function]`(row: any, column: TableColumnCtx<T>, cell: HTMLTableCellElement, event: Event) => void`    |
| cell-mouse-leave   | triggers when hovering out of a cell                                                                                                         | ^[Function]`(row: any, column: TableColumnCtx<T>, cell: HTMLTableCellElement, event: Event) => void`    |
| cell-click         | triggers when clicking a cell                                                                                                                | ^[Function]`(row: any, column: TableColumnCtx<T>, cell: HTMLTableCellElement, event: Event) => void`    |
| cell-dblclick      | triggers when double clicking a cell                                                                                                         | ^[Function]`(row: any, column: TableColumnCtx<T>, cell: HTMLTableCellElement, event: Event) => void`    |
| cell-contextmenu   | triggers when user right clicks on a cell                                                                                                    | ^[Function]`(row: any, column: TableColumnCtx<T>, cell: HTMLTableCellElement, event: Event) => void`    |
| row-click          | triggers when clicking a row                                                                                                                 | ^[Function]`(row: any, column: TableColumnCtx<T>, event: Event) => void`                                |
| row-contextmenu    | triggers when user right clicks on a row                                                                                                     | ^[Function]`(row: any, column: TableColumnCtx<T>, event: Event) => void`                                |
| row-dblclick       | triggers when double clicking a row                                                                                                          | ^[Function]`(row: any, column: TableColumnCtx<T>, event: Event) => void`                                |
| header-click       | triggers when clicking a column header                                                                                                       | ^[Function]`(column: TableColumnCtx<T>, event: Event) => void`                                          |
| header-contextmenu | triggers when user right clicks on a column header                                                                                           | ^[Function]`(column: TableColumnCtx<T>, event: Event) => void`                                          |
| sort-change        | triggers when Table's sorting changes                                                                                                        | ^[Function]`(data: {column: TableColumnCtx<T>, prop: string, order: any }) => void`                     |
| filter-change      | triggers when the table's filter changes                                                                                                     | ^[Function]`(newFilters: any) => void`                                                                  |
| current-change     | triggers when current row changes                                                                                                            | ^[Function]`(currentRow: any, oldCurrentRow: any) => void`                                              |
| header-dragend     | triggers after changing a column's width by dragging the column header's border                                                              | ^[Function]`(newWidth: number, oldWidth: number, column: TableColumnCtx<T>, event: MouseEvent) => void` |
| expand-change      | triggers when user expands or collapses a row (for expandable table, second param is expandedRows; for tree Table, second param is expanded) | ^[Function]`(row: any, expandedRows: any[]) => void & (row: any, expanded: boolean) => void`            |
| scroll ^(2.9.0)    | Invoked after scrolled                                                                                                                       | ^[Function]`({ scrollLeft: number, scrollTop: number }) => void`                                        |

| Name    | Description                                                                                                                                                                                   | Subtags      |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| default | customize default content                                                                                                                                                                     | Table-column |
| append  | Contents to be inserted after the last row. You may need this slot if you want to implement infinite scroll for the table. This slot will be displayed above the summary row if there is one. | —            |
| empty   | you can customize content when data is empty.                                                                                                                                                 | —            |

| Method                     | Description                                                                                                                                                       | Type                                                                         |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| clearSelection             | used in multiple selection Table, clear user selection                                                                                                            | ^[Function]`() => void`                                                      |
| getSelectionRows           | returns the currently selected rows                                                                                                                               | ^[Function]`() => any[]`                                                     |
| toggleRowSelection         | used in multiple selection Table, toggle if a certain row is selected. With the second parameter, you can directly set if this row is selected                    | ^[Function]`(row: any, selected?: boolean, ignoreSelectable = true) => void` |
| toggleAllSelection         | used in multiple selection Table, toggle select all and deselect all                                                                                              | ^[Function]`() => void`                                                      |
| toggleRowExpansion         | used in expandable Table or tree Table, toggle if a certain row is expanded. With the second parameter, you can directly set if this row is expanded or collapsed | ^[Function]`(row: any, expanded?: boolean) => void`                          |
| setCurrentRow              | used in single selection Table, set a certain row selected. If called without any parameter, it will clear selection                                              | ^[Function]`(row: any) => void`                                              |
| clearSort                  | clear sorting, restore data to the original order                                                                                                                 | ^[Function]`() => void`                                                      |
| clearFilter                | clear filters of the columns whose `columnKey` are passed in. If no params, clear all filters                                                                     | ^[Function]`(columnKeys?: string[]) => void`                                 |
| doLayout                   | refresh the layout of Table. When the visibility of Table changes, you may need to call this method to get a correct layout                                       | ^[Function]`() => void`                                                      |
| sort                       | sort Table manually. Property `prop` is used to set sort column, property `order` is used to set sort order                                                       | ^[Function]`(prop: string, order: string) => void`                           |
| scrollTo                   | scrolls to a particular set of coordinates                                                                                                                        | ^[Function]`(options: number \| ScrollToOptions, yCoord?: number) => void`   |
| setScrollTop               | set vertical scroll position                                                                                                                                      | ^[Function]`(top?: number) => void`                                          |
| setScrollLeft              | set horizontal scroll position                                                                                                                                    | ^[Function]`(left?: number) => void`                                         |
| columns ^(2.7.6)           | Get table columns context.                                                                                                                                        | ^[array]`TableColumnCtx<T>[]`                                                |
| updateKeyChildren ^(2.8.4) | used in lazy Table, must set `rowKey`, update key children                                                                                                        | ^[Function]`(key: string, data: T[]) => void`                                |

### Table-column Attributes

| Name                       | Description                                                                                                                                                                                                        | Type                                                                                                                                                                        | Default                           |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- |
| type                       | type of the column. If set to `selection`, the column will display checkbox. If set to `index`, the column will display index of the row (staring from 1). If set to `expand`, the column will display expand icon | ^[enum]`'default' \| 'selection' \| 'index' \| 'expand'`                                                                                                                    | default                           |
| index                      | customize indices for each row, works on columns with `type=index`                                                                                                                                                 | ^[number] / ^[Function]`(index: number) => number`                                                                                                                          | —                                 |
| label                      | column label                                                                                                                                                                                                       | ^[string]                                                                                                                                                                   | —                                 |
| column-key                 | column's key. If you need to use the filter-change event, you need this attribute to identify which column is being filtered                                                                                       | ^[string]                                                                                                                                                                   | —                                 |
| prop                       | field name. You can also use its alias: `property`                                                                                                                                                                 | ^[string]                                                                                                                                                                   | —                                 |
| width                      | column width                                                                                                                                                                                                       | ^[string] / ^[number]                                                                                                                                                       | ''                                |
| min-width                  | column minimum width. Columns with `width` has a fixed width, while columns with `min-width` has a width that is distributed in proportion                                                                         | ^[string] / ^[number]                                                                                                                                                       | ''                                |
| fixed                      | whether column is fixed at left / right. Will be fixed at left if `true`                                                                                                                                           | ^[enum]`'left' \| 'right'` / ^[boolean]                                                                                                                                     | false                             |
| render-header              | render function for table header of this column                                                                                                                                                                    | ^[Function]`(data: { column: TableColumnCtx<T>, $index: number }) => void`                                                                                                  | —                                 |
| sortable                   | whether column can be sorted. Remote sorting can be done by setting this attribute to 'custom' and listening to the `sort-change` event of Table                                                                   | ^[boolean] / ^[string]                                                                                                                                                      | false                             |
| sort-method                | sorting method, works when `sortable` is `true`. Should return a number, just like Array.sort                                                                                                                      | ^[Function]`<T = any>(a: T, b: T) => number`                                                                                                                                | —                                 |
| sort-by                    | specify which property to sort by, works when `sortable` is `true` and `sort-method` is `undefined`. If set to an Array, the column will sequentially sort by the next property if the previous one is equal       | ^[Function]`(row: any, index: number) => string` / ^[string] / ^[object]`string[]`                                                                                          | —                                 |
| sort-orders                | the order of the sorting strategies used when sorting the data, works when `sortable` is `true`. Accepts an array, as the user clicks on the header, the column is sorted in order of the elements in the array    | ^[object]`('ascending' \| 'descending' \| null)[]`                                                                                                                          | ['ascending', 'descending', null] |
| resizable                  | whether column width can be resized, works when `border` of `el-table` is `true`                                                                                                                                   | ^[boolean]                                                                                                                                                                  | true                              |
| formatter                  | function that formats cell content                                                                                                                                                                                 | ^[function]`(row: any, column: TableColumnCtx<T>, cellValue: any, index: number) => VNode \| string`                                                                        | —                                 |
| show-overflow-tooltip      | whether to hide extra content and show them in a tooltip when hovering on the cell                                                                                                                                 | ^[boolean] / [`object`](#table-attributes) ^(2.2.28)                                                                                                                        | undefined                         |
| align                      | alignment                                                                                                                                                                                                          | ^[enum]`'left' \| 'center' \| 'right'`                                                                                                                                      | left                              |
| header-align               | alignment of the table header. If omitted, the value of the above `align` attribute will be applied                                                                                                                | ^[enum]`'left' \| 'center' \| 'right'`                                                                                                                                      | left                              |
| class-name                 | class name of cells in the column                                                                                                                                                                                  | ^[string]                                                                                                                                                                   | —                                 |
| label-class-name           | class name of the label of this column                                                                                                                                                                             | ^[string]                                                                                                                                                                   | —                                 |
| selectable                 | function that determines if a certain row can be selected, works when `type` is 'selection'                                                                                                                        | ^[Function]`(row: any, index: number) => boolean`                                                                                                                           | —                                 |
| reserve-selection          | whether to reserve selection after data refreshing, works when `type` is 'selection'. Note that `row-key` is required for this to work                                                                             | ^[boolean]                                                                                                                                                                  | false                             |
| filters                    | an array of data filtering options. For each element in this array, `text` and `value` are required                                                                                                                | ^[array]`Array<{text: string, value: string}>`                                                                                                                              | —                                 |
| filter-placement           | placement for the filter dropdown                                                                                                                                                                                  | ^[enum]`'top' \| 'top-start' \| 'top-end' \| 'bottom' \| 'bottom-start' \| 'bottom-end' \| 'left' \| 'left-start' \| 'left-end' \| 'right' \| 'right-start' \| 'right-end'` | —                                 |
| filter-class-name ^(2.5.0) | className for the filter dropdown                                                                                                                                                                                  | ^[string]                                                                                                                                                                   | —                                 |
| filter-multiple            | whether data filtering supports multiple options                                                                                                                                                                   | ^[boolean]                                                                                                                                                                  | true                              |
| filter-method              | data filtering method. If `filter-multiple` is on, this method will be called multiple times for each row, and a row will display if one of the calls returns `true`                                               | ^[function]`(value: any, row: any, column: TableColumnCtx<T>) => void`                                                                                                      | —                                 |
| filtered-value             | filter value for selected data, might be useful when table header is rendered with `render-header`                                                                                                                 | ^[object]`string[]`                                                                                                                                                         | —                                 |
| tooltip-formatter ^(2.9.4) | customize tooltip content when using `show-overflow-tooltip`                                                                                                                                                       | ^[Function]`(data: { row: any, column: TableColumnCtx<T>, cellValue: any }) => VNode \| string`                                                                             | —                                 |

### Table-column Slots

| Name                 | Description                       | Type                                                               |
| -------------------- | --------------------------------- | ------------------------------------------------------------------ |
| default              | Custom content for table columns  | ^[object]`{ row: any, column: TableColumnCtx<T>, $index: number }` |
| header               | Custom content for table header   | ^[object]`{ column: TableColumnCtx<T>, $index: number }`           |
| filter-icon ^(2.7.8) | Custom content for filter icon    | ^[object]`{ filterOpened: boolean }`                               |
| expand ^(2.10.0)     | Custom content for expand columns | ^[object]`{ expanded: boolean }`                                   |

<details>
  <summary>Show declarations</summary>

#### How to use image preview in the table?

#### Why column is not rendered when use DOM templates?

Typical issue: [#5046](https://github.com/element-plus/element-plus/issues/5046) [#5862](https://github.com/element-plus/element-plus/issues/5862) [#6919](https://github.com/element-plus/element-plus/issues/6919)

This is because the HTML spec only allows a few specific elements to omit closing tags, the most common being `<input>` and `<img>`. For all other elements, if you omit the closing tag, the native HTML parser will think you never terminated the opening tag

For more details please refer to [vue docs](https://vuejs.org/guide/essentials/component-basics.html#self-closing-tags)

### check-strictly.vue

### custom-column.vue

### custom-header.vue

### expandable-row.vue

### fixed-column-and-group-header.vue

### fixed-column-and-header.vue

### fixed-header-with-fluid-header.vue

### grouping-header.vue

### rowspan-and-colspan.vue

### show-overflow-tooltip.vue

### single-select.vue

### tooltip-formatter.vue

### tree-and-lazy.vue

---
Title: Tabs
URL: https://element-plus.org/en-US/component/tabs
---

**Examples:**

Example 1 (ts):
```ts
interface Sort {
  prop: string
  order: 'ascending' | 'descending'
  init?: any
  silent?: any
}

interface TreeNode {
  expanded?: boolean
  loading?: boolean
  noLazyChildren?: boolean
  indent?: number
  level?: number
  display?: boolean
}

type DefaultRow = Record<PropertyKey, any>

type TableColumnCtx<T extends DefaultRow = DefaultRow> = {
  id: string
  realWidth: number | null
  type: string
  label: string
  className: string
  labelClassName: string
  property: string
  prop: string
  width?: string | number
  minWidth: string | number
  renderHeader: (data: CI<T>) => VNode
  sortable: boolean | string
  sortMethod: (a: T, b: T) => number
  sortBy: string | ((row: T, index: number, array?: T[]) => string) | string[]
  resizable: boolean
  columnKey: string
  rawColumnKey: string
  align: string
  headerAlign: string
  showOverflowTooltip?: boolean | TableOverflowTooltipOptions
  tooltipFormatter?: TableOverflowTooltipFormatter<T>
  fixed: boolean | string
  formatter: (
    row: T,
    column: TableColumnCtx<T>,
    cellValue: any,
    index: number
  ) => VNode | string
  selectable: (row: T, index: number) => boolean
  reserveSelection: boolean
  filterMethod: FilterMethods<T>
  filteredValue: string[]
  filters: Filters
  filterPlacement: string
  filterMultiple: boolean
  filterClassName: string
  index: number | ((index: number) => number)
  sortOrders: (TableSortOrder | null)[]
  renderCell: (data: any) => VNode | VNode[]
  colSpan: number
  rowSpan: number
  children?: TableColumnCtx<T>[]
  level: number
  filterable: boolean | FilterMethods<T> | Filters
  order: TableSortOrder | null
  isColumnGroup: boolean
  isSubColumn: boolean
  columns: TableColumnCtx<T>[]
  getColumnIndex: () => number
  no: number
  filterOpened?: boolean
  renderFilterIcon?: (scope: any) => VNode
  renderExpand?: (scope: any) => VNode
}
```

Example 2 (unknown):
```unknown
#### Why column is not rendered when use DOM templates?

Typical issue: [#5046](https://github.com/element-plus/element-plus/issues/5046) [#5862](https://github.com/element-plus/element-plus/issues/5862) [#6919](https://github.com/element-plus/element-plus/issues/6919)

This is because the HTML spec only allows a few specific elements to omit closing tags, the most common being `<input>` and `<img>`. For all other elements, if you omit the closing tag, the native HTML parser will think you never terminated the opening tag

For more details please refer to [vue docs](https://vuejs.org/guide/essentials/component-basics.html#self-closing-tags)

## Vue Examples

### basic.vue
```

Example 3 (unknown):
```unknown
### check-strictly.vue
```

Example 4 (unknown):
```unknown
### custom-column.vue
```

---
