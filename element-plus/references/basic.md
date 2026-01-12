# Element-Plus - Basic

**Pages:** 37

---

## Alert

**URL:** llms-txt#alert

**Contents:**
- Basic Usage
- Theme
- Customizable Close Button
- With Icon
- Centered Text
- With Description
- With Icon and Description
- Alert API
  - Attributes
  - Events

Displays important alert messages.

Alert components are non-overlay elements in the page that does not disappear automatically.

Alert provide two different themes, `light` and `dark`.

## Customizable Close Button

Customize the close button as texts or other symbols.

Displaying an icon improves readability.

Use the `center` attribute to center the text.

Description includes a message with more detailed information.

## With Icon and Description

| Name                               | Description                              | Type                                                                        | Default |
| ---------------------------------- | ---------------------------------------- | --------------------------------------------------------------------------- | ------- |
| title                              | alert title.                             | ^[string]                                                                   | —       |
| type                               | alert type.                              | ^[enum]`'primary' (2.9.11) \| 'success' \| 'warning' \| 'info' \| 'error' ` | info    |
| description                        | descriptive text.                        | ^[string]                                                                   | —       |
| closable                           | whether alert can be dismissed.          | ^[boolean]                                                                  | true    |
| center                             | whether content is placed in the center. | ^[boolean]                                                                  | false   |
| close-text                         | customized close button text.            | ^[string]                                                                   | —       |
| show-icon                          | whether a type icon is displayed.        | ^[boolean]                                                                  | false   |
| effect                             | theme style.                             | ^[enum]`'light' \| 'dark'`                                                  | light   |
| show-after ^(2.10.0) ^(deprecated) | delay of appearance, in millisecond      | ^[number]                                                                   | 0       |
| hide-after ^(2.10.0) ^(deprecated) | delay of disappear, in millisecond       | ^[number]                                                                   | 200     |
| auto-close ^(2.10.0) ^(deprecated) | timeout in milliseconds to hide alert    | ^[number]                                                                   | 0       |

| Name                        | Description                   | Type                                     |
| --------------------------- | ----------------------------- | ---------------------------------------- |
| close                       | trigger when alert is closed. | ^[Function]`(event: MouseEvent) => void` |
| open ^(2.10.0)^(deprecated) | trigger when alert is opened. | ^[Function]`() => void`                  |

| Name          | Description                       |
| ------------- | --------------------------------- |
| default       | content of the alert description. |
| title         | content of the alert title.       |
| icon ^(2.9.7) | content of the alert icon.        |

### icon-description.vue

---
Title: Anchor
URL: https://element-plus.org/en-US/component/anchor
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <div style="max-width: 600px">
    <el-alert title="Primary alert" type="primary" />
    <el-alert title="Success alert" type="success" />
    <el-alert title="Info alert" type="info" />
    <el-alert title="Warning alert" type="warning" />
    <el-alert title="Error alert" type="error" />
  </div>
</template>

<style scoped>
.el-alert {
  margin: 20px 0 0;
}
.el-alert:first-child {
  margin: 0;
}
</style>
```

Example 2 (vue):
```vue
<template>
  <div style="max-width: 600px">
    <el-alert title="Primary alert" type="primary" center show-icon />
    <el-alert title="Success alert" type="success" center show-icon />
    <el-alert title="Info alert" type="info" center show-icon />
    <el-alert title="Warning alert" type="warning" center show-icon />
    <el-alert title="Error alert" type="error" center show-icon />
  </div>
</template>

<style scoped>
.el-alert {
  margin: 20px 0 0;
}
.el-alert:first-child {
  margin: 0;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <div style="max-width: 600px">
    <el-alert title="Unclosable alert" type="success" :closable="false" />
    <el-alert title="Customized close text" type="info" close-text="Gotcha" />
    <el-alert title="Alert with callback" type="warning" @close="hello" />
  </div>
</template>

<script lang="ts" setup>
const hello = () => {
  // eslint-disable-next-line no-alert
  alert('Hello World!')
}
</script>

<style scoped>
.el-alert {
  margin: 20px 0 0;
}
.el-alert:first-child {
  margin: 0;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <div style="max-width: 600px">
    <el-alert
      title="With description"
      type="success"
      description="This is a description."
    />
  </div>
</template>
```

---

## Timeline

**URL:** llms-txt#timeline

**Contents:**
- Basic usage
- Custom node
- Custom timestamp
- Vertically centered
- Reverse ^(2.11.9)
- Timeline API
  - Timeline Attributes
  - Timeline Slots
- Timeline-Item API
  - Timeline-Item Attributes

Visually display timeline.

Timeline can be split into multiple activities. Timestamps are important features that distinguish them from other components. Note the difference with Steps.

Size, color, and icons can be customized in node.

Timestamp can be placed on top of content when content is too high.

## Vertically centered

Timeline-Item is centered vertically.

Use the reverse property to control the order of the nodes.

### Timeline Attributes

| Name              | Description           | Type       | Default |
| ----------------- | --------------------- | ---------- | ------- |
| reverse ^(2.11.9) | whether reverse order | ^[boolean] | false   |

| Name    | Description                            | Subtags       |
| ------- | -------------------------------------- | ------------- |
| default | customize default content for timeline | Timeline-Item |

### Timeline-Item Attributes

| Name           | Description                 | Type                                                               | Default |
| -------------- | --------------------------- | ------------------------------------------------------------------ | ------- |
| timestamp      | timestamp content           | ^[string]                                                          | ''      |
| hide-timestamp | whether to show timestamp   | ^[boolean]                                                         | false   |
| center         | whether vertically centered | ^[boolean]                                                         | false   |
| placement      | position of timestamp       | ^[enum]`'top' \| 'bottom'`                                         | bottom  |
| type           | node type                   | ^[enum]`'primary' \| 'success' \| 'warning' \| 'danger' \| 'info'` | ''      |
| color          | background color of node    | ^[enum]`'hsl' \| 'hsv' \| 'hex' \| 'rgb'`                          | ''      |
| size           | node size                   | ^[enum]`'normal' \| 'large'`                                       | normal  |
| icon           | icon component              | ^[string] / ^[Component]                                           | —       |
| hollow         | icon is hollow              | ^[boolean]                                                         | false   |

### Timeline-Item Slots

| Name    | Description                                 |
| ------- | ------------------------------------------- |
| default | customize default content for timeline item |
| dot     | customize defined node for timeline item    |

### custom-timestamp.vue

---
Title: Tooltip
URL: https://element-plus.org/en-US/component/tooltip
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-timeline style="max-width: 600px">
    <el-timeline-item
      v-for="(activity, index) in activities"
      :key="index"
      :timestamp="activity.timestamp"
    >
      {{ activity.content }}
    </el-timeline-item>
  </el-timeline>
</template>

<script lang="ts" setup>
const activities = [
  {
    content: 'Event start',
    timestamp: '2018-04-15',
  },
  {
    content: 'Approved',
    timestamp: '2018-04-13',
  },
  {
    content: 'Success',
    timestamp: '2018-04-11',
  },
]
</script>
```

Example 2 (vue):
```vue
<template>
  <el-timeline style="max-width: 600px">
    <el-timeline-item center timestamp="2018/4/12" placement="top">
      <el-card>
        <h4>Update Github template</h4>
        <p>Tom committed 2018/4/12 20:46</p>
      </el-card>
    </el-timeline-item>
    <el-timeline-item timestamp="2018/4/3" placement="top">
      <el-card>
        <h4>Update Github template</h4>
        <p>Tom committed 2018/4/3 20:46</p>
      </el-card>
    </el-timeline-item>
    <el-timeline-item center timestamp="2018/4/2" placement="top">
      Event start
    </el-timeline-item>
    <el-timeline-item timestamp="2018/4/2" placement="top">
      Event end
    </el-timeline-item>
  </el-timeline>
</template>
```

Example 3 (vue):
```vue
<template>
  <el-timeline style="max-width: 600px">
    <el-timeline-item
      v-for="(activity, index) in activities"
      :key="index"
      :icon="activity.icon"
      :type="activity.type"
      :color="activity.color"
      :size="activity.size"
      :hollow="activity.hollow"
      :timestamp="activity.timestamp"
    >
      {{ activity.content }}
    </el-timeline-item>
  </el-timeline>
</template>

<script lang="ts" setup>
import { MoreFilled } from '@element-plus/icons-vue'

import type { TimelineItemProps } from 'element-plus'

interface ActivityType extends Partial<TimelineItemProps> {
  content: string
}

const activities: ActivityType[] = [
  {
    content: 'Custom icon',
    timestamp: '2018-04-12 20:46',
    size: 'large',
    type: 'primary',
    icon: MoreFilled,
  },
  {
    content: 'Custom color',
    timestamp: '2018-04-03 20:46',
    color: '#0bbd87',
  },
  {
    content: 'Custom size',
    timestamp: '2018-04-03 20:46',
    size: 'large',
  },
  {
    content: 'Custom hollow',
    timestamp: '2018-04-03 20:46',
    type: 'primary',
    hollow: true,
  },
  {
    content: 'Default node',
    timestamp: '2018-04-03 20:46',
  },
]
</script>
```

Example 4 (vue):
```vue
<template>
  <el-timeline style="max-width: 600px">
    <el-timeline-item timestamp="2018/4/12" placement="top">
      <el-card>
        <h4>Update Github template</h4>
        <p>Tom committed 2018/4/12 20:46</p>
      </el-card>
    </el-timeline-item>
    <el-timeline-item timestamp="2018/4/3" placement="top">
      <el-card>
        <h4>Update Github template</h4>
        <p>Tom committed 2018/4/3 20:46</p>
      </el-card>
    </el-timeline-item>
    <el-timeline-item timestamp="2018/4/2" placement="top">
      <el-card>
        <h4>Update Github template</h4>
        <p>Tom committed 2018/4/2 20:46</p>
      </el-card>
    </el-timeline-item>
  </el-timeline>
</template>
```

---

## Carousel

**URL:** llms-txt#carousel

**Contents:**
- Basic usage
- Motion blur ^(2.6.0)
- Indicators
- Arrows
- Auto height
- Card mode
- Vertical
- Carousel API
  - Carousel Attributes
  - Carousel Events

Loop a series of images or texts in a limited space

## Motion blur ^(2.6.0)

Add motion blur to infuse dynamism and smoothness into the carousel.

Indicators can be displayed outside the carousel

You can define when arrows are displayed

When the `height` of `carousel` is set to `auto`, the `carousel` height will be automatically set according to the height of the `carousel item`

When a page is wide enough but has limited height, you can activate card mode for carousels

By default, `direction` is `horizontal`. Let carousel be displayed in the vertical direction by setting `direction` to `vertical`.

### Carousel Attributes

| Name                 | Description                                           | Type                                    | Default    |
| -------------------- | ----------------------------------------------------- | --------------------------------------- | ---------- |
| height               | height of the carousel                                | ^[string]                               | ''         |
| initial-index        | index of the initially active slide (starting from 0) | ^[number]                               | 0          |
| trigger              | how indicators are triggered                          | ^[enum]`'hover' \| 'click'`             | hover      |
| autoplay             | whether automatically loop the slides                 | ^[boolean]                              | true       |
| interval             | interval of the auto loop, in milliseconds            | ^[number]                               | 3000       |
| indicator-position   | position of the indicators                            | ^[enum]`'' \| 'none' \| 'outside'`      | ''         |
| arrow                | when arrows are shown                                 | ^[enum]`'always' \| 'hover' \| 'never'` | hover      |
| type                 | type of the Carousel                                  | ^[enum]`'' \| 'card'`                   | ''         |
| card-scale ^(2.7.8)  | when type is card, scaled size of secondary cards     | ^[number]                               | 0.83       |
| loop                 | display the items in loop                             | ^[boolean]                              | true       |
| direction            | display direction                                     | ^[enum]`'horizontal' \| 'vertical'`     | horizontal |
| pause-on-hover       | pause autoplay when hover                             | ^[boolean]                              | true       |
| motion-blur ^(2.6.0) | infuse dynamism and smoothness into the carousel      | ^[boolean]                              | false      |

| Name   | Description                                                                                                                                              | Type                                                    |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| change | triggers when the active slide switches, it has two parameters, the one is the index of the new active slide, and other is index of the old active slide | ^[Function]`(current: number, prev: number) => boolean` |

| Name    | Description               | Subtags       |
| ------- | ------------------------- | ------------- |
| default | customize default content | Carousel-Item |

| Method               | Description                                                                                                                     | Type                                           |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| activeIndex ^(2.7.8) | active slide index                                                                                                              | ^[number]                                      |
| setActiveItem        | manually switch slide, index of the slide to be switched to, starting from 0; or the `name` of corresponding `el-carousel-item` | ^[Function]`(index: string \| number) => void` |
| prev                 | switch to the previous slide                                                                                                    | ^[Function]`() => void`                        |
| next                 | switch to the next slide                                                                                                        | ^[Function]`() => void`                        |

### Carousel-Item Attributes

| Name  | Description                                      | Type                  | Default |
| ----- | ------------------------------------------------ | --------------------- | ------- |
| name  | name of the item, can be used in `setActiveItem` | ^[string]             | ''      |
| label | text content for the corresponding indicator     | ^[string] / ^[number] | ''      |

### Carousel-Item Slots

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

---
Title: Cascader
URL: https://element-plus.org/en-US/component/cascader
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-carousel :interval="5000" arrow="always">
    <el-carousel-item v-for="item in 4" :key="item">
      <h3 text="2xl" justify="center">{{ item }}</h3>
    </el-carousel-item>
  </el-carousel>
</template>

<style scoped>
.el-carousel__item h3 {
  color: #475669;
  opacity: 0.75;
  line-height: 300px;
  margin: 0;
  text-align: center;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n + 1) {
  background-color: #d3dce6;
}
</style>
```

Example 2 (vue):
```vue
<template>
  <div class="block text-center" style="height: 300px">
    <span class="demonstration">each carousel-item has a different height</span>
    <el-carousel height="auto" autoplay>
      <el-carousel-item style="height: 100px">
        <h3 class="small justify-center" text="2xl">height 100px</h3>
      </el-carousel-item>
      <el-carousel-item style="height: 200px">
        <h3 class="small justify-center" text="2xl">height 200px</h3>
      </el-carousel-item>
      <el-carousel-item style="height: 300px">
        <h3 class="small justify-center" text="2xl">height 300px</h3>
      </el-carousel-item>
    </el-carousel>
  </div>
</template>

<style scoped>
.carousel-item {
  color: #475669;
  opacity: 0.75;
  margin: 0;
  text-align: center;
}

.el-carousel__item h3 {
  color: #475669;
  opacity: 0.75;
  display: flex;
  align-items: center;
  margin: 0;
  text-align: center;
  height: 100%;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n + 1) {
  background-color: #d3dce6;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <div class="block text-center">
    <span class="demonstration">
      Switch when indicator is hovered (default)
    </span>
    <el-carousel height="150px">
      <el-carousel-item v-for="item in 4" :key="item">
        <h3 class="small justify-center" text="2xl">{{ item }}</h3>
      </el-carousel-item>
    </el-carousel>
  </div>
  <div class="block text-center" m="t-4">
    <span class="demonstration">Switch when indicator is clicked</span>
    <el-carousel trigger="click" height="150px">
      <el-carousel-item v-for="item in 4" :key="item">
        <h3 class="small justify-center" text="2xl">{{ item }}</h3>
      </el-carousel-item>
    </el-carousel>
  </div>
</template>

<style scoped>
.demonstration {
  color: var(--el-text-color-secondary);
}

.el-carousel__item h3 {
  color: #475669;
  opacity: 0.75;
  line-height: 150px;
  margin: 0;
  text-align: center;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n + 1) {
  background-color: #d3dce6;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <el-carousel :interval="4000" type="card" height="200px">
    <el-carousel-item v-for="item in 6" :key="item">
      <h3 text="2xl" justify="center">{{ item }}</h3>
    </el-carousel-item>
  </el-carousel>
</template>

<style scoped>
.el-carousel__item h3 {
  color: #475669;
  opacity: 0.75;
  line-height: 200px;
  margin: 0;
  text-align: center;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n + 1) {
  background-color: #d3dce6;
}
</style>
```

---

## Mention

**URL:** llms-txt#mention

**Contents:**
- Basic Usage
- Props ^(2.11.3)
- Textarea
- Customize label
- Load remote options
- Customize trigger token
- Delete as a whole
- Work with form
- API
  - Attributes

Used to mention someone or something in an input.

The most basic usage.

You can customize the alias of the `options` through the `props` attribute.

The input type can be set to `textarea`.

Customize label by `label` slot.

## Load remote options

Load options asynchronously.

## Customize trigger token

Customize trigger token by `prefix` props. Default to `@`, `Array<string>` also supported.

Set the `whole` attribute to `true`, and when you press the backspace, the mention will be deleted as a whole.
Set the `check-is-whole` attribute to customize the checking logic.

to work with `el-form`.

| Name                                 | Description                                                                            | Type                                                                         | Default                                                  |
| ------------------------------------ | -------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | -------------------------------------------------------- |
| options                              | mention options list                                                                   | ^[array]`MentionOption[]`                                                    | `[]`                                                     |
| props ^(2.11.3)                      | configuration options                                                                  | ^[object]`MentionOptionProps`                                                | `{value: 'value', label: 'label', disabled: 'disabled'}` |
| prefix                               | prefix character to trigger mentions. The string length must be exactly 1              | ^[string] \| ^[array]`string[]`                                              | `'@'`                                                    |
| split                                | character to split mentions. The string length must be exactly 1                       | ^[string]                                                                    | `' '`                                                    |
| filter-option                        | customize filter option logic                                                          | ^[false] \| ^[Function]`(pattern: string, option: MentionOption) => boolean` | —                                                        |
| placement                            | set popup placement                                                                    | ^[string]`'bottom' \| 'top'`                                                 | `'bottom'`                                               |
| show-arrow                           | whether the dropdown panel has an arrow                                                | ^[boolean]                                                                   | `false`                                                  |
| offset                               | offset of the dropdown panel                                                           | ^[number]                                                                    | `0`                                                      |
| whole                                | when backspace is pressed to delete, whether the mention content is deleted as a whole | ^[boolean]                                                                   | `false`                                                  |
| check-is-whole                       | when backspace is pressed to delete, check if the mention is a whole                   | ^[Function]`(pattern: string, prefix: string) => boolean`                    | —                                                        |
| loading                              | whether the dropdown panel of mentions is in a loading state                           | ^[boolean]                                                                   | `false`                                                  |
| model-value / v-model                | input value                                                                            | ^[string]                                                                    | —                                                        |
| popper-class                         | custom class name for dropdown panel                                                   | ^[string] / ^[object]                                                        | ''                                                       |
| popper-style ^(2.11.5)               | custom style for dropdown panel                                                        | ^[string] / ^[object]                                                        | —                                                        |
| popper-options                       | [popper.js](https://popper.js.org/docs/v2/) parameters                                 | ^[object] refer to [popper.js doc](https://popper.js.org/docs/v2/)           | —                                                        |
| [input props](./input.md#attributes) | —                                                                                      | —                                                                            | —                                                        |

| Name                              | Description                                                                                 | Type                                                         |
| --------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| search                            | trigger when prefix hit                                                                     | ^[Function]`(pattern: string, prefix: string) => void`       |
| select                            | trigger when user select the option                                                         | ^[Function]`(option: MentionOption, prefix: string) => void` |
| whole-remove ^(2.10.4)            | trigger when a whole mention is removed and `whole` is `true` or `check-is-whole` is `true` | ^[Function]`(pattern: string, prefix: string) => void`       |
| [input events](./input.md#events) | —                                                                                           | —                                                            |

| Name                            | Description                           | Type                                              |
| ------------------------------- | ------------------------------------- | ------------------------------------------------- |
| label                           | content as option label               | ^[object]`{ item: MentionOption, index: number }` |
| loading                         | content as option loading             | —                                                 |
| header                          | content at the top of the dropdown    | —                                                 |
| footer                          | content at the bottom of the dropdown | —                                                 |
| [input slots](./input.md#slots) | —                                     | —                                                 |

| Name                     | Description                   | Type                                    |
| ------------------------ | ----------------------------- | --------------------------------------- |
| input                    | el-input component instance   | ^[object]`Ref<InputInstance \| null>`   |
| tooltip                  | el-tooltip component instance | ^[object]`Ref<TooltipInstance \| null>` |
| dropdownVisible ^(2.8.5) | tooltip display status        | ^[object]`ComputedRef<boolean>`         |

<details>
  <summary>Show declarations</summary>

---
Title: Menu
URL: https://element-plus.org/en-US/component/menu
---

**Examples:**

Example 1 (ts):
```ts
type MentionOption = {
  value?: string
  label?: string
  disabled?: boolean
  [key: string]: any
}

type MentionOptionProps = {
  value?: string
  label?: string
  disabled?: string
  [key: string]: string | undefined
}
```

Example 2 (vue):
```vue
<template>
  <el-mention
    v-model="value"
    :options="options"
    style="width: 320px"
    placeholder="Please input"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue'

const value = ref('@')

const options = ref([
  {
    label: 'Fuphoenixes',
    value: 'Fuphoenixes',
  },
  {
    label: 'kooriookami',
    value: 'kooriookami',
  },
  {
    label: 'Jeremy',
    value: 'Jeremy',
  },
  {
    label: 'btea',
    value: 'btea',
  },
])
</script>
```

Example 3 (vue):
```vue
<template>
  <el-form
    ref="ruleFormRef"
    style="max-width: 600px"
    :model="ruleForm"
    :rules="rules"
  >
    <el-form-item label="name" prop="name">
      <el-mention v-model="ruleForm.name" :options="options" />
    </el-form-item>
    <el-form-item label="desc" prop="desc">
      <el-mention v-model="ruleForm.desc" type="textarea" :options="options" />
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="submitForm(ruleFormRef)">
        Submit
      </el-button>
      <el-button @click="resetForm(ruleFormRef)">Reset</el-button>
    </el-form-item>
  </el-form>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue'

import type { FormInstance, FormRules } from 'element-plus'

interface RuleForm {
  name: string
  desc: string
}
const ruleFormRef = ref<FormInstance>()
const ruleForm = reactive<RuleForm>({
  name: '',
  desc: '',
})

const options = ref([
  {
    label: 'Fuphoenixes',
    value: 'Fuphoenixes',
  },
  {
    label: 'kooriookami',
    value: 'kooriookami',
  },
  {
    label: 'Jeremy',
    value: 'Jeremy',
  },
  {
    label: 'btea',
    value: 'btea',
  },
])

const rules = reactive<FormRules<RuleForm>>({
  name: [{ required: true, message: 'Please input name', trigger: 'blur' }],
  desc: [{ required: true, message: 'Please input desc', trigger: 'blur' }],
})

const submitForm = async (formEl: FormInstance | undefined) => {
  if (!formEl) return
  await formEl.validate((valid, fields) => {
    if (valid) {
      console.log('submit!')
    } else {
      console.log('error submit!', fields)
    }
  })
}

const resetForm = (formEl: FormInstance | undefined) => {
  if (!formEl) return
  formEl.resetFields()
}
</script>
```

Example 4 (vue):
```vue
<template>
  <el-mention
    v-model="value"
    :options="options"
    style="width: 320px"
    placeholder="Please input"
  >
    <template #label="{ item }">
      <div style="display: flex; align-items: center">
        <el-avatar :size="24" :src="item.avatar" />
        <span style="margin-left: 6px">{{ item.value }}</span>
      </div>
    </template>
  </el-mention>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const value = ref('')

const options = ref([
  {
    value: 'Fuphoenixes',
    avatar: 'https://avatars.githubusercontent.com/u/27912232',
  },
  {
    value: 'kooriookami',
    avatar: 'https://avatars.githubusercontent.com/u/38392315',
  },
  {
    value: 'Jeremy',
    avatar: 'https://avatars.githubusercontent.com/u/15975785',
  },
  {
    value: 'btea',
    avatar: 'https://avatars.githubusercontent.com/u/24516654',
  },
])
</script>
```

---

## Dropdown

**URL:** llms-txt#dropdown

**Contents:**
- Basic usage
- Placement
- Triggering element
- How to trigger
- Menu hiding behavior
- Command event
- Dropdown methods
- Sizes
- Virtual triggering ^(2.11.3)
- Dropdown API

Toggleable menu for displaying lists of links and actions.

Hover on the dropdown menu to unfold it for more actions.

Support 6 placements.

## Triggering element

Use the button to trigger the dropdown list.

Click the triggering element or hover on it.

## Menu hiding behavior

Use `hide-on-click` to define if menu closes on clicking.

Clicking each dropdown item fires an event whose parameter is assigned by each item.

You can open or close the dropdown menu by manually use `handleOpen` or `handleClose`

Besides default size, Dropdown component provides three additional sizes for you to choose among different scenarios.

## Virtual triggering ^(2.11.3)

Sometimes we want to render the dropdown on some other trigger element, we can separate the trigger and the content.

### Dropdown Attributes

| Name                         | Description                                                                                                           | Type                                                                                                         | Default                                                                    |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------- |
| type                         | menu button type, refer to `Button` Component, only works when `split-button` is true                                 | ^[enum]`'' \| 'default' \| 'primary' \| 'success' \| 'warning' \| 'info' \| 'danger' \| 'text' (deprecated)` | ''                                                                         |
| size                         | menu size, also works on the split button                                                                             | ^[enum]`'' \| 'large' \| 'default' \| 'small'`                                                               | ''                                                                         |
| button-props                 | props for the button component, refer to [Button Attributes](./button.html#button-attributes)                         | ^[object]                                                                                                    | —                                                                          |
| max-height                   | the max height of menu                                                                                                | ^[string] / ^[number]                                                                                        | ''                                                                         |
| split-button                 | whether a button group is displayed                                                                                   | ^[boolean]                                                                                                   | false                                                                      |
| disabled                     | whether to disable                                                                                                    | ^[boolean]                                                                                                   | false                                                                      |
| placement                    | placement of pop menu                                                                                                 | ^[enum]`'top' \| 'top-start' \| 'top-end' \| 'bottom' \| 'bottom-start' \| 'bottom-end'`                     | bottom                                                                     |
| effect                       | Tooltip theme, built-in theme: `dark` / `light`                                                                       | ^[enum]`'dark' \| 'light'` / ^[string]                                                                       | light                                                                      |
| trigger                      | how to trigger                                                                                                        | ^[enum]`'click' \| 'hover' \| 'contextmenu'` / ^[array]`Array<'click' \| 'hover' \| 'contextmenu'>`          | hover                                                                      |
| trigger-keys ^(2.9.1)        | specify which keys on the keyboard can trigger when pressed                                                           | ^[array]`string[]`                                                                                           | `['Enter', 'Space', 'ArrowDown', 'NumpadEnter']`                           |
| virtual-triggering ^(2.11.3) | indicates whether virtual triggering is enabled                                                                       | ^[boolean]                                                                                                   | —                                                                          |
| virtual-ref ^(2.11.3)        | indicates the reference element to which the dropdown is attached                                                     | ^[HTMLElement]                                                                                               | —                                                                          |
| hide-on-click                | whether to hide menu after clicking menu-item                                                                         | ^[boolean]                                                                                                   | true                                                                       |
| show-arrow ^(2.11.3)         | whether the tooltip content has an arrow                                                                              | ^[boolean]                                                                                                   | true                                                                       |
| show-timeout                 | delay time before show a dropdown (only works when trigger is `hover`)                                                | ^[number]                                                                                                    | 150                                                                        |
| hide-timeout                 | delay time before hide a dropdown (only works when trigger is `hover`)                                                | ^[number]                                                                                                    | 150                                                                        |
| role                         | the ARIA role attribute for the dropdown menu. Depending on the use case, you may want to change this to 'navigation' | ^[enum]`'dialog' \| 'grid' \| 'group' \| 'listbox' \| 'menu' \| 'navigation' \| 'tooltip' \| 'tree'`         | menu                                                                       |
| tabindex                     | [tabindex](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex) of Dropdown                  | ^[number] / ^[string]                                                                                        | 0                                                                          |
| popper-class                 | custom class name for Dropdown's dropdown                                                                             | ^[string] / ^[object]                                                                                        | ''                                                                         |
| popper-style ^(2.11.5)       | custom style for Dropdown's dropdown                                                                                  | ^[string] / ^[object]                                                                                        | —                                                                          |
| popper-options               | [popper.js](https://popper.js.org/docs/v2/) parameters                                                                | ^[object]                                                                                                    | `{modifiers: [{name: 'computeStyles',options: {gpuAcceleration: false}}]}` |
| teleported ^(2.2.20)         | whether the dropdown popup is teleported to the body                                                                  | ^[boolean]                                                                                                   | true                                                                       |
| append-to ^(2.13.0)          | which element the dropdown CONTENT appends to                                                                         | ^[CSSSelector] / ^[HTMLElement]                                                                              | —                                                                          |
| persistent ^(2.9.5)          | when dropdown inactive and `persistent` is `false` , dropdown menu will be destroyed                                  | ^[boolean]                                                                                                   | true                                                                       |

| Name     | Description                                                                                                                                   | Subtags       |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| default  | content of Dropdown. Notice: Must be a valid html dom element (ex. `<span>, <button> etc.`) or `el-component`, to attach the trigger listener | —             |
| dropdown | content of the Dropdown Menu, usually a `<el-dropdown-menu>` element                                                                          | Dropdown-Menu |

| Name           | Description                                                                                               | Type                                  |
| -------------- | --------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| click          | if `split-button` is `true`, triggers when left button is clicked                                         | ^[Function]`(e: MouseEvent) => void`  |
| command        | triggers when a dropdown item is clicked, the parameters is the command dispatched from the dropdown item | ^[Function]`(...args: any[]) => void` |
| visible-change | triggers when the dropdown appears/disappears, the param is true when it appears, and false otherwise     | ^[Function]`(val: boolean) => void`   |

| Method      | Description             | Type                    |
| ----------- | ----------------------- | ----------------------- |
| handleOpen  | open the dropdown menu  | ^[Function]`() => void` |
| handleClose | close the dropdown menu | ^[Function]`() => void` |

### Dropdown-Menu Slots

| Name    | Description              | Subtags       |
| ------- | ------------------------ | ------------- |
| default | content of Dropdown Menu | Dropdown-Item |

### Dropdown-Item Attributes

| Name     | Description                                                 | Type                              | Default |
| -------- | ----------------------------------------------------------- | --------------------------------- | ------- |
| command  | a command to be dispatched to Dropdown's `command` callback | ^[string] / ^[number] / ^[object] | —       |
| disabled | whether the item is disabled                                | ^[boolean]                        | false   |
| divided  | whether a divider is displayed                              | ^[boolean]                        | false   |
| icon     | custom icon                                                 | ^[string] / ^[Component]          | —       |

### Dropdown-Item Slots

| Name    | Description                |
| ------- | -------------------------- |
| default | customize of Dropdown Item |

### command-event.vue

### dropdown-methods.vue

### how-to-trigger.vue

### menu-hiding-behavior.vue

### triggering-element.vue

### virtual-trigger.vue

---
Title: Empty
URL: https://element-plus.org/en-US/component/empty
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-dropdown>
    <span class="el-dropdown-link">
      Dropdown List
      <el-icon class="el-icon--right">
        <arrow-down />
      </el-icon>
    </span>
    <template #dropdown>
      <el-dropdown-menu>
        <el-dropdown-item>Action 1</el-dropdown-item>
        <el-dropdown-item>Action 2</el-dropdown-item>
        <el-dropdown-item>Action 3</el-dropdown-item>
        <el-dropdown-item disabled>Action 4</el-dropdown-item>
        <el-dropdown-item divided>Action 5</el-dropdown-item>
      </el-dropdown-menu>
    </template>
  </el-dropdown>
</template>

<script lang="ts" setup>
import { ArrowDown } from '@element-plus/icons-vue'
</script>

<style scoped>
.example-showcase .el-dropdown-link {
  cursor: pointer;
  color: var(--el-color-primary);
  display: flex;
  align-items: center;
}
</style>
```

Example 2 (vue):
```vue
<template>
  <el-dropdown @command="handleCommand">
    <span class="el-dropdown-link">
      Dropdown List<el-icon class="el-icon--right"><arrow-down /></el-icon>
    </span>
    <template #dropdown>
      <el-dropdown-menu>
        <el-dropdown-item command="a">Action 1</el-dropdown-item>
        <el-dropdown-item command="b">Action 2</el-dropdown-item>
        <el-dropdown-item command="c">Action 3</el-dropdown-item>
        <el-dropdown-item command="d" disabled>Action 4</el-dropdown-item>
        <el-dropdown-item command="e" divided>Action 5</el-dropdown-item>
      </el-dropdown-menu>
    </template>
  </el-dropdown>
</template>

<script lang="ts" setup>
import { ElMessage } from 'element-plus'
import { ArrowDown } from '@element-plus/icons-vue'

const handleCommand = (command: string | number | object) => {
  ElMessage(`click on item ${command}`)
}
</script>

<style scoped>
.example-showcase .el-dropdown-link {
  cursor: pointer;
  color: var(--el-color-primary);
  display: flex;
  align-items: center;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <div style="font-size: 14px">
    <p>open(close) the Dropdown list2 will close(open) the Dropdown List1.</p>
  </div>
  <div style="margin: 15px">
    <el-button @click="showClick">show</el-button>
  </div>
  <el-dropdown ref="dropdown1" trigger="contextmenu" style="margin-right: 30px">
    <span class="el-dropdown-link"> Dropdown List1 </span>
    <template #dropdown>
      <el-dropdown-menu>
        <el-dropdown-item>Action 1</el-dropdown-item>
        <el-dropdown-item>Action 2</el-dropdown-item>
        <el-dropdown-item>Action 3</el-dropdown-item>
        <el-dropdown-item disabled>Action 4</el-dropdown-item>
        <el-dropdown-item divided>Action 5</el-dropdown-item>
      </el-dropdown-menu>
    </template>
  </el-dropdown>

  <el-dropdown trigger="contextmenu" @visible-change="handleVisible2">
    <span class="el-dropdown-link"> Dropdown List2 </span>
    <template #dropdown>
      <el-dropdown-menu>
        <el-dropdown-item>Action 1</el-dropdown-item>
        <el-dropdown-item>Action 2</el-dropdown-item>
        <el-dropdown-item>Action 3</el-dropdown-item>
        <el-dropdown-item disabled>Action 4</el-dropdown-item>
        <el-dropdown-item divided>Action 5</el-dropdown-item>
      </el-dropdown-menu>
    </template>
  </el-dropdown>
</template>

<script setup lang="ts">
import { ref } from 'vue'

import type { DropdownInstance } from 'element-plus'

const dropdown1 = ref<DropdownInstance>()
function handleVisible2(visible: any) {
  if (!dropdown1.value) return
  if (visible) {
    dropdown1.value.handleClose()
  } else {
    dropdown1.value.handleOpen()
  }
}
function showClick() {
  if (!dropdown1.value) return
  dropdown1.value.handleOpen()
}
</script>

<style scoped>
.example-showcase .el-dropdown-link {
  cursor: pointer;
  color: var(--el-color-primary);
  display: flex;
  align-items: center;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <el-row class="block-col-2">
    <el-col :span="8">
      <span class="demonstration">hover to trigger</span>
      <el-dropdown>
        <span class="el-dropdown-link">
          Dropdown List<el-icon class="el-icon--right"><arrow-down /></el-icon>
        </span>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item :icon="Plus">Action 1</el-dropdown-item>
            <el-dropdown-item :icon="CirclePlusFilled">
              Action 2
            </el-dropdown-item>
            <el-dropdown-item :icon="CirclePlus">Action 3</el-dropdown-item>
            <el-dropdown-item :icon="Check">Action 4</el-dropdown-item>
            <el-dropdown-item :icon="CircleCheck">Action 5</el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </el-col>
    <el-col :span="8">
      <span class="demonstration">click to trigger</span>
      <el-dropdown trigger="click">
        <span class="el-dropdown-link">
          Dropdown List<el-icon class="el-icon--right"><arrow-down /></el-icon>
        </span>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item :icon="Plus">Action 1</el-dropdown-item>
            <el-dropdown-item :icon="CirclePlusFilled">
              Action 2
            </el-dropdown-item>
            <el-dropdown-item :icon="CirclePlus">Action 3</el-dropdown-item>
            <el-dropdown-item :icon="Check">Action 4</el-dropdown-item>
            <el-dropdown-item :icon="CircleCheck">Action 5</el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </el-col>
    <el-col :span="8">
      <span class="demonstration">right click to trigger</span>
      <el-dropdown trigger="contextmenu">
        <span class="el-dropdown-link">
          Dropdown List<el-icon class="el-icon--right"><arrow-down /></el-icon>
        </span>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item :icon="Plus">Action 1</el-dropdown-item>
            <el-dropdown-item :icon="CirclePlusFilled">
              Action 2
            </el-dropdown-item>
            <el-dropdown-item :icon="CirclePlus">Action 3</el-dropdown-item>
            <el-dropdown-item :icon="Check">Action 4</el-dropdown-item>
            <el-dropdown-item :icon="CircleCheck">Action 5</el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </el-col>
  </el-row>
</template>

<script lang="ts" setup>
import {
  ArrowDown,
  Check,
  CircleCheck,
  CirclePlus,
  CirclePlusFilled,
  Plus,
} from '@element-plus/icons-vue'
</script>

<style scoped>
.block-col-2 .demonstration {
  display: block;
  color: var(--el-text-color-secondary);
  font-size: 14px;
  margin-bottom: 20px;
}

.block-col-2 .el-dropdown-link {
  display: flex;
  align-items: center;
}
</style>
```

---

## as they are displayed as plain text outside of GitLab

**URL:** llms-txt#as-they-are-displayed-as-plain-text-outside-of-gitlab

---

## Badge

**URL:** llms-txt#badge

**Contents:**
- Basic Usage
- Max Value
- Customizations
- Red Dot
- Offset ^(2.7.0)
- API
  - Attributes
  - Slots
- Vue Examples
  - basic.vue

A number or status mark on buttons and icons.

Displays the amount of new messages.

You can customize the max value.

Displays text content other than numbers. Or you can use the `content` slot to customize content.

Use a red dot to mark content that needs to be noticed.

| Name                 | Description                                                                   | Type                                                               | Default |
| -------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------ | ------- |
| value                | display value.                                                                | ^[string] / ^[number]                                              | ''      |
| max                  | maximum value, shows `{max}+` when exceeded. Only works if value is a number. | ^[number]                                                          | 99      |
| is-dot               | if a little dot is displayed.                                                 | ^[boolean]                                                         | false   |
| hidden               | hidden badge.                                                                 | ^[boolean]                                                         | false   |
| type                 | badge type.                                                                   | ^[enum]`'primary' \| 'success' \| 'warning' \| 'danger' \| 'info'` | danger  |
| show-zero ^(2.6.0)   | Whether to show badge when value is zero.                                     | ^[boolean]                                                         | true    |
| color ^(2.6.3)       | background color of the dot                                                   | ^[string]                                                          |         |
| offset ^(2.7.0)      | offset of badge                                                               | ^[array]`[number, number]`                                         | —       |
| badge-style ^(2.7.1) | custom style of badge                                                         | ^[object]`CSSProperties`                                           | —       |
| badge-class ^(2.7.1) | custom class of badge                                                         | ^[string]                                                          | —       |

| Name             | Description               | Type                         |
| ---------------- | ------------------------- | ---------------------------- |
| default          | customize default content | -                            |
| content ^(2.9.1) | customize badge content   | ^[object]`{ value: string }` |

---
Title: Border
URL: https://element-plus.org/en-US/component/border
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-badge :value="12" class="item">
    <el-button>comments</el-button>
  </el-badge>
  <el-badge :value="3" class="item">
    <el-button>replies</el-button>
  </el-badge>
  <el-badge :value="1" class="item" type="primary">
    <el-button>comments</el-button>
  </el-badge>
  <el-badge :value="2" class="item" type="warning">
    <el-button>replies</el-button>
  </el-badge>
  <el-badge :value="1" class="item" color="green">
    <el-button>custom background</el-button>
  </el-badge>
  <el-dropdown trigger="click">
    <span class="el-dropdown-link">
      Click Me
      <el-icon class="el-icon--right"><caret-bottom /></el-icon>
    </span>
    <template #dropdown>
      <el-dropdown-menu>
        <el-dropdown-item class="clearfix">
          comments
          <el-badge class="mark" :value="12" />
        </el-dropdown-item>
        <el-dropdown-item class="clearfix">
          replies
          <el-badge class="mark" :value="3" />
        </el-dropdown-item>
      </el-dropdown-menu>
    </template>
  </el-dropdown>
</template>

<script lang="ts" setup>
import { CaretBottom } from '@element-plus/icons-vue'
</script>

<style scoped>
.item {
  margin-top: 10px;
  margin-right: 30px;
}

.el-dropdown {
  margin-top: 1.1rem;
}
</style>
```

Example 2 (vue):
```vue
<template>
  <el-badge value="new" class="item">
    <el-button>comments</el-button>
  </el-badge>
  <el-badge value="hot" class="item">
    <el-button>replies</el-button>
  </el-badge>
  <el-badge value="99" class="item">
    <el-button>share</el-button>
    <template #content="{ value }">
      <div class="custom-content">
        <el-icon>
          <Message />
        </el-icon>
        <span>{{ value }}</span>
      </div>
    </template>
  </el-badge>
</template>

<script setup lang="ts">
import { Message } from '@element-plus/icons-vue'
</script>

<style scoped>
.item {
  margin-top: 10px;
  margin-right: 40px;
}

.custom-content {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 4px;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <el-badge is-dot class="item">query</el-badge>
  <el-badge is-dot class="item">
    <el-button class="share-button" :icon="Share" type="primary" />
  </el-badge>
</template>

<script lang="ts" setup>
import { Share } from '@element-plus/icons-vue'
</script>

<style scoped>
.item {
  margin-top: 10px;
  margin-right: 40px;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <el-badge :value="200" :max="99" class="item">
    <el-button>comments</el-button>
  </el-badge>
  <el-badge :value="100" :max="10" class="item">
    <el-button>replies</el-button>
  </el-badge>
</template>

<style scoped>
.item {
  margin-top: 10px;
  margin-right: 40px;
}
</style>
```

---

## <ElBadge value="beta">Tree V2 virtualized tree</ElBadge>

**URL:** llms-txt#<elbadge-value="beta">tree-v2-virtualized-tree</elbadge>

**Contents:**
- Basic usage
- Selectable
- Disabled checkbox
- Default expanded and default checked
- Custom node content
- Custom node class ^(2.9.0)
- Custom node icon ^(2.10.3)
- Tree node filtering ^(2.9.1)
- TreeV2 API
  - TreeV2 Attributes

Tree view with blazing fast scrolling performance for any amount of data

Basic tree structure.

Used for node selection.

The checkbox of a node can be set as disabled.

## Default expanded and default checked

Tree nodes can be initially expanded or checked

## Custom node content

The content of tree nodes can be customized, so you can add icons or buttons as you will

## Custom node class ^(2.9.0)

The class of tree nodes can be customized

## Custom node icon ^(2.10.3)

You can customize icons for different node states. Tree nodes expose the `expanded` property and `isLeaf` property, allowing you to dynamically render different icons based on the node's state: leaf nodes, expanded nodes, or collapsed nodes.

## Tree node filtering ^(2.9.1)

The `filter-method` method can only accept the third parameter after version `2.9.1`.
Tree nodes can be filtered

### TreeV2 Attributes

| Name                          | Description                                                                                                                                  | Type                                                                        | Default |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- | ------- |
| data                          | tree data                                                                                                                                    | ^[object]`Array<{[key: string]: any}>`                                      | —       |
| empty-text                    | text displayed when data is void                                                                                                             | ^[string]                                                                   | —       |
| [props](#props)               | configuration options, see the following table                                                                                               | ^[object]                                                                   | —       |
| highlight-current             | whether current node is highlighted                                                                                                          | ^[boolean]                                                                  | false   |
| expand-on-click-node          | whether to expand or collapse node when clicking on the node, if false, then expand or collapse node only when clicking on the arrow icon.   | ^[boolean]                                                                  | true    |
| check-on-click-node           | whether to check or uncheck node when clicking on the node, if false, the node can only be checked or unchecked by clicking on the checkbox. | ^[boolean]                                                                  | false   |
| check-on-click-leaf ^(2.9.6)  | whether to check or uncheck node when clicking on leaf node (last children).                                                                 | ^[boolean]                                                                  | true    |
| default-expanded-keys         | array of keys of initially expanded nodes                                                                                                    | ^[object]`Array<string \| number>`                                          | —       |
| show-checkbox                 | whether node is selectable                                                                                                                   | ^[boolean]                                                                  | false   |
| check-strictly                | whether checked state of a node not affects its father and child nodes when `show-checkbox` is `true`                                        | ^[boolean]                                                                  | false   |
| default-checked-keys          | array of keys of initially checked nodes                                                                                                     | ^[object]`Array<string \| number>`                                          | —       |
| current-node-key              | key of initially selected node                                                                                                               | ^[string] / ^[number]                                                       | —       |
| filter-method                 | this function will be executed on each node when use filter method. if return `false`, tree node will be hidden.                             | ^[Function]`(query: string, data: TreeNodeData, node: TreeNode) => boolean` | —       |
| indent                        | horizontal indentation of nodes in adjacent levels in pixels                                                                                 | ^[number]                                                                   | 16      |
| icon                          | custom tree node icon component                                                                                                              | ^[string] / ^[Component]                                                    | —       |
| item-size ^(2.2.33)           | custom tree node height                                                                                                                      | ^[number]                                                                   | 26      |
| scrollbar-always-on ^(2.10.4) | always show scrollbar                                                                                                                        | ^[boolean]                                                                  | false   |
| height                        | height of the tree                                                                                                                           | ^[number]                                                                   | 200     |

| Attribute      | Description                                                                          | Type                                                                                                | Default  |
| -------------- | ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------- | -------- |
| value          | unique identity key name for nodes, its value should be unique across the whole tree | ^[string]                                                                                           | id       |
| label          | specify which key of node object is used as the node's label                         | ^[string]                                                                                           | label    |
| children       | specify which node object is used as the node's subtree                              | ^[string]                                                                                           | children |
| disabled       | specify which key of node object represents if node's checkbox is disabled           | ^[string]                                                                                           | disabled |
| class ^(2.9.0) | custom node class name                                                               | ^[string] / ^[Function]`(data: TreeNodeData, node: TreeNode) => string \| {[key: string]: boolean}` | —        |

`Tree` has the following method, which returns the currently selected array of nodes.

| Method                | Description                                                                                                                   | Parameters                                                           |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| filter                | filter all tree nodes, filtered nodes will be hidden                                                                          | `(query: string)`                                                    |
| getCheckedNodes       | If the node can be selected (`show-checkbox` is `true`), it returns the currently selected array of nodes                     | `(leafOnly: boolean)`                                                |
| getCheckedKeys        | If the node can be selected (`show-checkbox` is `true`), it returns the currently selected array of node's keys               | `(leafOnly: boolean)`                                                |
| setCheckedKeys        | set certain nodes to be checked                                                                                               | `(keys: TreeKey[])`                                                  |
| setChecked            | set node to be checked or not                                                                                                 | `(key: TreeKey, checked: boolean)`                                   |
| setExpandedKeys       | set certain nodes to be expanded                                                                                              | `(keys: TreeKey[])`                                                  |
| getHalfCheckedNodes   | If the node can be selected (`show-checkbox` is `true`), it returns the currently half selected array of nodes                | —                                                                    |
| getHalfCheckedKeys    | If the node can be selected (`show-checkbox` is `true`), it returns the currently half selected array of node's keys          | —                                                                    |
| getCurrentKey         | return the highlight node's key (undefined if no node is highlighted)                                                         | —                                                                    |
| getCurrentNode        | return the highlight node's data (undefined if no node is highlighted)                                                        | —                                                                    |
| setCurrentKey         | set highlighted node by key                                                                                                   | `(key: TreeKey)`                                                     |
| getNode               | get node by key or data                                                                                                       | `(data: TreeKey \| TreeNodeData)`                                    |
| expandNode            | expand specified node                                                                                                         | `(node: TreeNode)`                                                   |
| collapseNode          | collapse specified node                                                                                                       | `(node: TreeNode)`                                                   |
| setData               | When the data is very large, using reactive data will cause the poor performance, so we provide a way to avoid this situation | `(data: TreeData)`                                                   |
| scrollTo ^(2.8.0)     | scroll to a given position                                                                                                    | `(offset: number)`                                                   |
| scrollToNode ^(2.8.0) | scroll to a given tree key with specified scroll strategy                                                                     | `(key: TreeKey, strategy?: auto \| smart \| center \| start \| end)` |

| Name               | Description                                          | Parameters                                                                                                                              |
| ------------------ | ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| node-click         | triggers when a node is clicked                      | `(data: TreeNodeData, node: TreeNode, e: MouseEvent)`                                                                                   |
| node-drop ^(2.8.3) | triggers when drag something and drop on a node      | `(data: TreeNodeData, node: TreeNode, e: DragEvent)`                                                                                    |
| node-contextmenu   | triggers when a node is clicked by right button      | `(e: Event, data: TreeNodeData, node: TreeNode)`                                                                                        |
| check-change       | triggers when the selected state of the node changes | `(data: TreeNodeData, checked: boolean)`                                                                                                |
| check              | triggers after clicking the checkbox of a node       | `(data: TreeNodeData, info: { checkedKeys: TreeKey[],checkedNodes: TreeData, halfCheckedKeys: TreeKey[], halfCheckedNodes: TreeData,})` |
| current-change     | triggers when current node changes                   | `(data: TreeNodeData, node: TreeNode)`                                                                                                  |
| node-expand        | triggers when current node open                      | `(data: TreeNodeData, node: TreeNode)`                                                                                                  |
| node-collapse      | triggers when current node close                     | `(data: TreeNodeData, node: TreeNode)`                                                                                                  |

| Name           | Description                       | Type                                              |
| -------------- | --------------------------------- | ------------------------------------------------- |
| default        | custom content for tree nodes     | ^[object]`{ node: TreeNode, data: TreeNodeData }` |
| empty ^(2.9.0) | custom content when data is empty | —                                                 |

<details>
  <summary>Show declarations</summary>

### custom-node-class.vue

### default-state.vue

---
Title: Tree
URL: https://element-plus.org/en-US/component/tree
---

**Examples:**

Example 1 (ts):
```ts
type TreeNodeData = Record<string, any>
type TreeKey = string | number
type TreeData = TreeNodeData[]

interface TreeNode {
  key: TreeKey
  level: number
  parent?: TreeNode
  children?: TreeNode[]
  data: TreeNodeData
  disabled?: boolean
  label?: string
  isLeaf?: boolean
  expanded?: boolean
  isEffectivelyChecked?: boolean
}
```

Example 2 (vue):
```vue
<template>
  <el-tree-v2
    style="max-width: 600px"
    :data="data"
    :props="props"
    :height="200"
  />
</template>

<script lang="ts" setup>
interface Tree {
  id: string
  label: string
  children?: Tree[]
}

const getKey = (prefix: string, id: number) => {
  return `${prefix}-${id}`
}

const createData = (
  maxDeep: number,
  maxChildren: number,
  minNodesNumber: number,
  deep = 1,
  key = 'node'
): Tree[] => {
  let id = 0
  return Array.from({ length: minNodesNumber })
    .fill(deep)
    .map(() => {
      const childrenNumber =
        deep === maxDeep ? 0 : Math.round(Math.random() * maxChildren)
      const nodeKey = getKey(key, ++id)
      return {
        id: nodeKey,
        label: nodeKey,
        children: childrenNumber
          ? createData(maxDeep, maxChildren, childrenNumber, deep + 1, nodeKey)
          : undefined,
      }
    })
}

const props = {
  value: 'id',
  label: 'label',
  children: 'children',
}
const data = createData(4, 30, 40)
</script>
```

Example 3 (vue):
```vue
<template>
  <el-tree-v2
    style="max-width: 600px"
    :data="data"
    :props="props"
    :height="200"
  >
    <template #default="{ node }">
      <el-icon class="node-icon" :class="{ 'is-leaf': node.isLeaf }">
        <Document v-if="node.isLeaf" />
        <Folder v-else-if="!node.expanded" />
        <FolderOpened v-else />
      </el-icon>
      <span>{{ node.label }}</span>
    </template>
  </el-tree-v2>
</template>

<script lang="ts" setup>
import { Document, Folder, FolderOpened } from '@element-plus/icons-vue'

interface Tree {
  id: string
  label: string
  children?: Tree[]
}

const getKey = (prefix: string, id: number) => {
  return `${prefix}-${id}`
}

const createData = (
  maxDeep: number,
  maxChildren: number,
  minNodesNumber: number,
  deep = 1,
  key = 'node'
): Tree[] => {
  let id = 0
  return Array.from({ length: minNodesNumber })
    .fill(deep)
    .map(() => {
      const childrenNumber =
        deep === maxDeep ? 0 : Math.round(Math.random() * maxChildren)
      const nodeKey = getKey(key, ++id)
      return {
        id: nodeKey,
        label: nodeKey,
        children: childrenNumber
          ? createData(maxDeep, maxChildren, childrenNumber, deep + 1, nodeKey)
          : undefined,
      }
    })
}

const props = {
  value: 'id',
  label: 'label',
  children: 'children',
}
const data = createData(4, 30, 40)
</script>

<style scoped>
.node-icon {
  margin-right: 5px;
  color: var(--el-color-warning);
}
</style>
```

Example 4 (vue):
```vue
<template>
  <el-tree-v2
    style="max-width: 600px"
    :data="data"
    show-checkbox
    :expand-on-click-node="false"
    :props="{ class: customNodeClass }"
  />
</template>

<script lang="ts" setup>
import type { TreeNode, TreeNodeData } from 'element-plus'

interface Tree {
  id?: string
  value?: string
  label?: string
  isPenultimate?: boolean
  children?: Tree[]
}

const customNodeClass = ({ isPenultimate }: TreeNodeData, node: TreeNode) =>
  isPenultimate ? 'is-penultimate' : ''

const data: Tree[] = [
  {
    id: '1',
    label: 'Level one 1',
    children: [
      {
        id: '4',
        label: 'Level two 1-1',
        isPenultimate: true,
        children: [
          {
            id: '9',
            label: 'Level three 1-1-1',
          },
          {
            id: '10',
            label: 'Level three 1-1-2',
          },
        ],
      },
    ],
  },
  {
    id: '2',
    label: 'Level one 2',
    isPenultimate: true,
    children: [
      {
        id: '5',
        label: 'Level two 2-1',
      },
      {
        id: '6',
        label: 'Level two 2-2',
      },
    ],
  },
  {
    id: '3',
    label: 'Level one 3',
    isPenultimate: true,
    children: [
      {
        id: '7',
        label: 'Level two 3-1',
      },
      {
        id: '8',
        label: 'Level two 3-2',
      },
    ],
  },
]
</script>

<style>
.is-penultimate > .el-tree-node__content {
  color: var(--el-color-primary);
}
</style>
```

---

## --------------------

**URL:** llms-txt#--------------------

**Contents:**
  - Template for commit messages
  - Useful links

md
feat(components): [button] I did something with button

Blank between subject and body is expected.(period is expected)
Describes your change in one line or multi-line.
Capitalize your first letter when starting a new line
Please do not exceeds 72 characters per line, because that would be harder to comprehend.

- You can also add bullet list symbol for better layout

[type](scope): [messages]
```

You can checkout the allowed values for **type** and **scope** in [commitlint.config.js](https://github.com/element-plus/element-plus/blob/c2ee36a7fc72b17742d43ecdff4e2912c416141d/commitlint.config.js#L57),

[Keeping git commit history clean](https://about.gitlab.com/blog/2018/06/07/keeping-git-commit-history-clean/)

---
Title: Dark Mode
URL: https://element-plus.org/en-US/guide/dark-mode
---

**Examples:**

Example 1 (unknown):
```unknown
### Template for commit messages

Below is a template commit message for your reference.
```

Example 2 (unknown):
```unknown
For the subject header, the format is:
```

---

## Scrollbar

**URL:** llms-txt#scrollbar

**Contents:**
- Basic usage
- Horizontal scroll
- Max height
- Manual scroll
- Infinite scroll ^(2.10.0)
- API
  - Attributes
  - Events
  - Slots
  - Exposes

Used to replace the browser's native scrollbar.

## Infinite scroll ^(2.10.0)

| Name                              | Description                                                                                                                     | Type                                                                | Default |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- | ------- |
| height                            | height of scrollbar                                                                                                             | ^[string] / ^[number]                                               | —       |
| max-height                        | max height of scrollbar                                                                                                         | ^[string] / ^[number]                                               | —       |
| native                            | whether to use the native scrollbar style                                                                                       | ^[boolean]                                                          | false   |
| wrap-style                        | style of wrap container                                                                                                         | ^[string] / ^[object]`CSSProperties \| CSSProperties[] \| string[]` | —       |
| wrap-class                        | class of wrap container                                                                                                         | ^[string]                                                           | —       |
| view-style                        | style of view                                                                                                                   | ^[string] / ^[object]`CSSProperties \| CSSProperties[] \| string[]` | —       |
| view-class                        | class of view                                                                                                                   | ^[string]                                                           | —       |
| noresize                          | do not respond to container size changes, if the container size does not change, it is better to set it to optimize performance | ^[boolean]                                                          | false   |
| tag                               | element tag of the view                                                                                                         | ^[string]                                                           | div     |
| always                            | always show scrollbar                                                                                                           | ^[boolean]                                                          | false   |
| min-size                          | minimum size of scrollbar                                                                                                       | ^[number]                                                           | 20      |
| id ^(2.4.0)                       | id of view                                                                                                                      | ^[string]                                                           | —       |
| role ^(2.4.0) ^(a11y)             | role of view                                                                                                                    | ^[string]                                                           | —       |
| aria-label ^(2.4.0) ^(a11y)       | aria-label of view                                                                                                              | ^[string]                                                           | —       |
| aria-orientation ^(2.4.0) ^(a11y) | aria-orientation of view                                                                                                        | ^[enum]`'horizontal' \| 'vertical'`                                 | —       |
| tabindex ^(2.8.3)                 | tabindex of wrap container                                                                                                      | ^[number] / ^[string]                                               | —       |
| distance ^(2.10.5)                | trigger end-reached event distance(px)                                                                                          | ^[number]                                                           | 0       |

| Name                  | Description                                           | Type                                                                     |
| --------------------- | ----------------------------------------------------- | ------------------------------------------------------------------------ |
| scroll                | triggers when scrolling, return distance of scrolling | ^[Function]`({ scrollLeft: number, scrollTop: number }) => void`         |
| end-reached ^(2.10.0) | triggers when the end of a scroll is triggered        | ^[Function]`(direction: 'top' \| 'bottom' \| 'left' \| 'right') => void` |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

| Name          | Description                                | Type                                                                       |
| ------------- | ------------------------------------------ | -------------------------------------------------------------------------- |
| handleScroll  | handle scroll event                        | ^[Function]`() => void`                                                    |
| scrollTo      | scrolls to a particular set of coordinates | ^[Function]`(options: ScrollToOptions \| number, yCoord?: number) => void` |
| setScrollTop  | Set distance to scroll top                 | ^[Function]`(scrollTop: number) => void`                                   |
| setScrollLeft | Set distance to scroll left                | ^[Function]`(scrollLeft: number) => void`                                  |
| update        | update scrollbar state manually            | ^[Function]`() => void`                                                    |
| wrapRef       | scrollbar wrap ref                         | ^[object]`Ref<HTMLDivElement>`                                             |

### horizontal-scroll.vue

### infinite-scroll.vue

### manual-scroll.vue

---
Title: Segmented
URL: https://element-plus.org/en-US/component/segmented
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-scrollbar height="400px">
    <p v-for="item in 20" :key="item" class="scrollbar-demo-item">{{ item }}</p>
  </el-scrollbar>
</template>

<style scoped>
.scrollbar-demo-item {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 50px;
  margin: 10px;
  text-align: center;
  border-radius: 4px;
  background: var(--el-color-primary-light-9);
  color: var(--el-color-primary);
}
</style>
```

Example 2 (vue):
```vue
<template>
  <el-scrollbar>
    <div class="scrollbar-flex-content">
      <p v-for="item in 50" :key="item" class="scrollbar-demo-item">
        {{ item }}
      </p>
    </div>
  </el-scrollbar>
</template>

<style scoped>
.scrollbar-flex-content {
  display: flex;
  width: fit-content;
}
.scrollbar-demo-item {
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100px;
  height: 50px;
  margin: 10px;
  text-align: center;
  border-radius: 4px;
  background: var(--el-color-danger-light-9);
  color: var(--el-color-danger);
}
</style>
```

Example 3 (vue):
```vue
<template>
  <el-scrollbar height="400px" @end-reached="loadMore">
    <p v-for="item in num" :key="item" class="scrollbar-demo-item">
      {{ item }}
    </p>
  </el-scrollbar>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

import type { ScrollbarDirection } from 'element-plus'

const num = ref(30)

const loadMore = (direction: ScrollbarDirection) => {
  if (direction === 'bottom') {
    num.value += 5
  }
}
</script>

<style scoped>
.scrollbar-demo-item {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 50px;
  margin: 10px;
  text-align: center;
  border-radius: 4px;
  background: var(--el-color-primary-light-9);
  color: var(--el-color-primary);
}
.el-slider {
  margin-top: 20px;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <el-scrollbar ref="scrollbarRef" height="400px" always @scroll="scroll">
    <div ref="innerRef">
      <p v-for="item in 20" :key="item" class="scrollbar-demo-item">
        {{ item }}
      </p>
    </div>
  </el-scrollbar>

  <el-slider
    v-model="value"
    :max="max"
    :format-tooltip="formatTooltip"
    @input="inputSlider"
  />
</template>

<script lang="ts" setup>
import { onMounted, ref } from 'vue'

import type { ScrollbarInstance } from 'element-plus'

type Arrayable<T> = T | T[]

const max = ref(0)
const value = ref(0)
const innerRef = ref<HTMLDivElement>()
const scrollbarRef = ref<ScrollbarInstance>()

onMounted(() => {
  max.value = innerRef.value!.clientHeight - 380
})

const inputSlider = (value: Arrayable<number>) => {
  scrollbarRef.value!.setScrollTop(value as number)
}
const scroll = ({ scrollTop }) => {
  value.value = scrollTop
}
const formatTooltip = (value: number) => `${value} px`
</script>

<style scoped>
.scrollbar-demo-item {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 50px;
  margin: 10px;
  text-align: center;
  border-radius: 4px;
  background: var(--el-color-primary-light-9);
  color: var(--el-color-primary);
}
.el-slider {
  margin-top: 20px;
}
</style>
```

---

## Loading

**URL:** llms-txt#loading

**Contents:**
- Loading inside a container
- Customization
- Full screen loading
- Service
- App context inheritance ^(2.9.10)
- API
  - Options
  - Directives
- Vue Examples
  - basic.vue

Show animation while loading data.

## Loading inside a container

Displays animation in a container (such as a table) while loading data.

You can customize loading text, loading spinner and background color.

## Full screen loading

Show a full screen animation while loading data.

You can also invoke Loading with a service. Import Loading service:

The parameter `options` is the configuration of Loading, and its details can be found in the following table. `LoadingService` returns a Loading instance, and you can close it by invoking its `close` method:

Note that in this case the full screen Loading is singleton. If a new full screen Loading is invoked before an existing one is closed, the existing full screen Loading instance will be returned instead of actually creating another Loading instance:

Calling the `close` method on any one of them can close this full screen Loading.

If Element Plus is imported entirely, a globally method `$loading` will be registered to `app.config.globalProperties`. You can invoke it like this: `this.$loading(options)`, and it also returns a Loading instance.

## App context inheritance ^(2.9.10)

Now loading accepts a `context` as second parameter of the loading constructor which allows you to inject current app's context to loading which allows you to inherit all the properties of the app.

You can use it like this:

| Name                 | Description                                                                                                                                                              | Type                                     | Default       |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------- | ------------- |
| target               | the DOM node Loading needs to cover. Accepts a DOM object or a string. If it's a string, it will be passed to `document.querySelector` to get the corresponding DOM node | ^[string] / ^[HTMLElement]               | document.body |
| body                 | same as the `body` modifier of `v-loading`                                                                                                                               | ^[boolean]                               | false         |
| fullscreen           | same as the `fullscreen` modifier of `v-loading`                                                                                                                         | ^[boolean]                               | true          |
| lock                 | same as the `lock` modifier of `v-loading`                                                                                                                               | ^[boolean]                               | false         |
| text                 | loading text that displays under the spinner                                                                                                                             | ^[string] / ^[VNode] / ^[array]`VNode[]` | —             |
| spinner              | class name of the custom spinner                                                                                                                                         | ^[string]                                | —             |
| background           | background color of the mask                                                                                                                                             | ^[string]                                | —             |
| customClass          | custom class name for loading                                                                                                                                            | ^[string]                                | —             |
| svg                  | custom SVG element to override the default loading spinner                                                                                                               | ^[string]                                | —             |
| svgViewBox           | sets the viewBox attribute for loading svg element                                                                                                                       | ^[string]                                | —             |
| beforeClose ^(2.7.8) | Function executed before loading attempts to close. If this function returns false, the closing process will be aborted. Otherwise, the loading will close.              | ^[Function]`() => boolean`               | —             |
| closed ^(2.7.8)      | Function triggered after loading has completely closed                                                                                                                   | ^[Function]`() => void`                  | —             |

| Name                         | Description                                                  | Type                           |
| ---------------------------- | ------------------------------------------------------------ | ------------------------------ |
| v-loading                    | show animation while loading data                            | ^[boolean] / ^[LoadingOptions] |
| element-loading-text         | loading text that displays under the spinner                 | ^[string]                      |
| element-loading-spinner      | icon of the custom spinner                                   | ^[string]                      |
| element-loading-svg          | icon of the custom spinner (same as element-loading-spinner) | ^[string]                      |
| element-loading-svg-view-box | sets the viewBox attribute for loading svg element           | ^[string]                      |
| element-loading-background   | background color of the mask                                 | ^[string]                      |
| element-loading-custom-class | custom class name for loading                                | ^[string]                      |

### customization.vue

---
Title: Mention
URL: https://element-plus.org/en-US/component/mention
---

**Examples:**

Example 1 (ts):
```ts
import { ElLoading } from 'element-plus'
```

Example 2 (ts):
```ts
ElLoading.service(options)
```

Example 3 (ts):
```ts
const loadingInstance = ElLoading.service(options)
nextTick(() => {
  // Loading should be closed asynchronously
  loadingInstance.close()
})
```

Example 4 (ts):
```ts
const loadingInstance1 = ElLoading.service({ fullscreen: true })
const loadingInstance2 = ElLoading.service({ fullscreen: true })
console.log(loadingInstance1 === loadingInstance2) // true
```

---

## Avatar

**URL:** llms-txt#avatar

**Contents:**
- Basic Usage
- Types
- Fallback
- Fit Container
- API
  - Attributes
  - Events
  - Slots
- Vue Examples
  - basic.vue

Avatars can be used to represent people or objects. It supports images, Icons, or characters.

Use `shape` and `size` prop to set avatar's shape and size.

It supports images, Icons, or characters.

fallback when image load error.

Set how the image fit its container for an image avatar, same as [object-fit](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit).

| Name    | Description                                               | Type                                                              | Default |
| ------- | --------------------------------------------------------- | ----------------------------------------------------------------- | ------- |
| icon    | representation type to icon, more info on icon component. | ^[string] / ^[Component]                                          | —       |
| size    | avatar size.                                              | ^[number] / ^[enum]`'large' \| 'default' \| 'small'`              | default |
| shape   | avatar shape.                                             | ^[enum]`'circle' \| 'square'`                                     | circle  |
| src     | the source of the image for an image avatar.              | `string`                                                          | —       |
| src-set | native attribute `srcset` of image avatar.                | `string`                                                          | —       |
| alt     | native attribute `alt` of image avatar.                   | `string`                                                          | —       |
| fit     | set how the image fit its container for an image avatar.  | ^[enum]`'fill' \| 'contain' \| 'cover' \| 'none' \| 'scale-down'` | cover   |

| Name  | Description                    | Type                            |
| ----- | ------------------------------ | ------------------------------- |
| error | trigger when image load error. | ^[Function]`(e: Event) => void` |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize avatar content. |

---
Title: Backtop
URL: https://element-plus.org/en-US/component/backtop
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-row class="demo-avatar demo-basic">
    <el-col :lg="12" :md="12">
      <div class="sub-title">circle</div>
      <div class="demo-basic--circle">
        <div class="block">
          <el-avatar :size="50" :src="circleUrl" />
        </div>
        <div v-for="size in sizeList" :key="size" class="block">
          <el-avatar :size="size" :src="circleUrl" />
        </div>
      </div>
    </el-col>
    <el-col :lg="12" :md="12">
      <div class="sub-title">square</div>
      <div class="demo-basic--circle">
        <div class="block">
          <el-avatar shape="square" :size="50" :src="squareUrl" />
        </div>
        <div v-for="size in sizeList" :key="size" class="block">
          <el-avatar shape="square" :size="size" :src="squareUrl" />
        </div>
      </div>
    </el-col>
  </el-row>
</template>

<script lang="ts" setup>
import { reactive, toRefs } from 'vue'

const state = reactive({
  circleUrl:
    'https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png',
  squareUrl:
    'https://cube.elemecdn.com/9/c2/f0ee8a3c7c9638a54940382568c9dpng.png',
  sizeList: ['small', '', 'large'] as const,
})

const { circleUrl, squareUrl, sizeList } = toRefs(state)
</script>

<style scoped>
.demo-basic {
  text-align: center;
}
.demo-basic .sub-title {
  margin-bottom: 10px;
  font-size: 14px;
  color: var(--el-text-color-secondary);
}
.demo-basic .demo-basic--circle,
.demo-basic .demo-basic--square {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.demo-basic .block:not(:last-child) {
  border-right: 1px solid var(--el-border-color);
}
.demo-basic .block {
  flex: 1;
}
.demo-basic .el-col:not(:last-child) {
  border-right: 1px solid var(--el-border-color);
}
@media screen and (max-width: 992px) {
  .demo-basic .el-col:not(:last-child) {
    border-right: none;
  }
}
</style>
```

Example 2 (vue):
```vue
<template>
  <div class="demo-type">
    <el-avatar :size="60" src="https://empty" @error="errorHandler">
      <img
        src="https://cube.elemecdn.com/e/fd/0fc7d20532fdaf769a25683617711png.png"
      />
    </el-avatar>
  </div>
</template>

<script lang="ts" setup>
const errorHandler = () => true
</script>
```

Example 3 (vue):
```vue
<template>
  <div class="demo-fit">
    <div v-for="fit in fits" :key="fit" class="block">
      <span class="title">{{ fit }}</span>
      <el-avatar shape="square" :size="100" :fit="fit" :src="url" />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { reactive, toRefs } from 'vue'

import type { CSSProperties } from 'vue'

const state = reactive({
  fits: [
    'fill',
    'contain',
    'cover',
    'none',
    'scale-down',
  ] as CSSProperties['object-fit'][],
  url: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg',
})

const { fits, url } = toRefs(state)
</script>

<style scoped>
.demo-fit {
  display: flex;
  text-align: center;
  justify-content: space-between;
}
.demo-fit .block {
  flex: 1;
  display: flex;
  flex-direction: column;
  flex-grow: 0;
}

.demo-fit .title {
  margin-bottom: 10px;
  font-size: 14px;
  color: var(--el-text-color-secondary);
}
</style>
```

Example 4 (vue):
```vue
<template>
  <div class="demo-type">
    <div>
      <el-avatar :icon="UserFilled" />
    </div>
    <div>
      <el-avatar
        src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"
      />
    </div>
    <div>
      <el-avatar> user </el-avatar>
    </div>
  </div>
</template>

<script setup lang="ts">
import { UserFilled } from '@element-plus/icons-vue'
</script>

<style scoped>
.demo-type {
  display: flex;
}
.demo-type > div {
  flex: 1;
  text-align: center;
}

.demo-type > div:not(:last-child) {
  border-right: 1px solid var(--el-border-color);
}
</style>
```

---

## Color

**URL:** llms-txt#color

**Contents:**
- Main Color
- Secondary Color
- Neutral Color

Element Plus uses a specific set of palettes to specify colors to provide a consistent look and feel for the products you build.

<style lang="scss">
.demo-color-box {
  position: relative;
  border-radius: 4px;
  padding: 20px;
  margin: 8px 0;
  height: 112px;
  box-sizing: border-box;
  color: var(--el-color-white);
  font-size: 14px;

.bg-color-sub {
    width: 100%;
    height: 40px;
    left: 0;
    bottom: 0;
    position: absolute;

.bg-blue-sub-item {
      height: 100%;
      display: inline-block;

&:first-child {
        border-radius: 0 0 0 var(--el-border-radius-base);
      }
    }

.bg-secondary-sub-item {
      height: 100%;
      display: inline-block;
      &:first-child {
        border-radius: 0 0 0 var(--el-border-radius-base);
      }
    }
  }

.value {
    margin-top: 2px;
  }
}

.demo-color-box-lite {
  color: var(--el-text-color-primary);
}
</style>

The main color of Element Plus is bright and friendly blue.

<!-- Do not touch -->
<ClientOnly>
  <MainColor />
</ClientOnly>

Besides the main color, you need to use different scene colors in different scenarios (for example, dangerous color indicates dangerous operation)

<!-- Do not touch -->
<ClientOnly>
  <SecondaryColors />
</ClientOnly>

Neutral colors are for text, background and border colors. You can use different neutral colors to represent the hierarchical structure.

<!-- Do not touch -->
<ClientOnly>
  <NeutralColor />
</ClientOnly>

---
Title: Config Provider
URL: https://element-plus.org/en-US/component/config-provider
---

---

## Layout

**URL:** llms-txt#layout

**Contents:**
- Basic layout
- Column spacing
- Hybrid layout
- Column offset
- Alignment
- Responsive Layout
- Utility classes for hiding elements
- Row API
  - Row Attributes
  - Row Slots

Quickly and easily create layouts with the basic 24-column.

Create basic grid layout using columns.

Column spacing is supported.

Form a more complex hybrid layout by combining the basic 1/24 columns.

You can specify column offsets.

Default use the flex layout to make flexible alignment of columns.

Taking example by Bootstrap's responsive design, five breakpoints are preset:
xs, sm, md, lg and xl.

## Utility classes for hiding elements

Additionally, Element Plus provides a series of classes for hiding elements under
certain conditions. These classes can be added to any DOM elements or custom components.
You need to import the following CSS file to use these classes:

- `hidden-xs-only` - hide when on extra small viewports only
- `hidden-sm-only` - hide when on small viewports only
- `hidden-sm-and-down` - hide when on small viewports and down
- `hidden-sm-and-up` - hide when on small viewports and up
- `hidden-md-only` - hide when on medium viewports only
- `hidden-md-and-down` - hide when on medium viewports and down
- `hidden-md-and-up` - hide when on medium viewports and up
- `hidden-lg-only` - hide when on large viewports only
- `hidden-lg-and-down` - hide when on large viewports and down
- `hidden-lg-and-up` - hide when on large viewports and up
- `hidden-xl-only` - hide when on extra large viewports only

| Name    | Description                         | Type                                                                                         | Default |
| ------- | ----------------------------------- | -------------------------------------------------------------------------------------------- | ------- |
| gutter  | grid spacing                        | ^[number]                                                                                    | 0       |
| justify | horizontal alignment of flex layout | ^[enum]`'start' \| 'end' \| 'center' \| 'space-around' \| 'space-between' \| 'space-evenly'` | start   |
| align   | vertical alignment of flex layout   | ^[enum]`'top' \| 'middle' \| 'bottom'`                                                       | —       |
| tag     | custom element tag                  | ^[string]                                                                                    | div     |

| Name    | Description               | Subtags |
| ------- | ------------------------- | ------- |
| default | customize default content | Col     |

| Name   | Description                                         | Type                                                                                  | Default |
| ------ | --------------------------------------------------- | ------------------------------------------------------------------------------------- | ------- |
| span   | number of column the grid spans                     | ^[number]                                                                             | 24      |
| offset | number of spacing on the left side of the grid      | ^[number]                                                                             | 0       |
| push   | number of columns that grid moves to the right      | ^[number]                                                                             | 0       |
| pull   | number of columns that grid moves to the left       | ^[number]                                                                             | 0       |
| xs     | `<768px` Responsive columns or column props object  | ^[number] / ^[object]`{span?: number, offset?: number, pull?: number, push?: number}` | —       |
| sm     | `≥768px` Responsive columns or column props object  | ^[number] / ^[object]`{span?: number, offset?: number, pull?: number, push?: number}` | —       |
| md     | `≥992px` Responsive columns or column props object  | ^[number] / ^[object]`{span?: number, offset?: number, pull?: number, push?: number}` | —       |
| lg     | `≥1200px` Responsive columns or column props object | ^[number] / ^[object]`{span?: number, offset?: number, pull?: number, push?: number}` | —       |
| xl     | `≥1920px` Responsive columns or column props object | ^[number] / ^[object]`{span?: number, offset?: number, pull?: number, push?: number}` | —       |
| tag    | custom element tag                                  | ^[string]                                                                             | div     |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

<style lang="scss">
@use '../../examples/layout/index.scss';
</style>

### column-offset.vue

### column-spacing.vue

### hybrid-layout.vue

### responsive-layout.vue

---
Title: Link
URL: https://element-plus.org/en-US/component/link
---

**Examples:**

Example 1 (js):
```js
import 'element-plus/theme-chalk/display.css'
```

Example 2 (vue):
```vue
<template>
  <el-row class="row-bg">
    <el-col :span="6"><div class="grid-content ep-bg-purple" /></el-col>
    <el-col :span="6"><div class="grid-content ep-bg-purple-light" /></el-col>
    <el-col :span="6"><div class="grid-content ep-bg-purple" /></el-col>
  </el-row>
  <el-row class="row-bg" justify="center">
    <el-col :span="6"><div class="grid-content ep-bg-purple" /></el-col>
    <el-col :span="6"><div class="grid-content ep-bg-purple-light" /></el-col>
    <el-col :span="6"><div class="grid-content ep-bg-purple" /></el-col>
  </el-row>
  <el-row class="row-bg" justify="end">
    <el-col :span="6"><div class="grid-content ep-bg-purple" /></el-col>
    <el-col :span="6"><div class="grid-content ep-bg-purple-light" /></el-col>
    <el-col :span="6"><div class="grid-content ep-bg-purple" /></el-col>
  </el-row>
  <el-row class="row-bg" justify="space-between">
    <el-col :span="6"><div class="grid-content ep-bg-purple" /></el-col>
    <el-col :span="6"><div class="grid-content ep-bg-purple-light" /></el-col>
    <el-col :span="6"><div class="grid-content ep-bg-purple" /></el-col>
  </el-row>
  <el-row class="row-bg" justify="space-around">
    <el-col :span="6"><div class="grid-content ep-bg-purple" /></el-col>
    <el-col :span="6"><div class="grid-content ep-bg-purple-light" /></el-col>
    <el-col :span="6"><div class="grid-content ep-bg-purple" /></el-col>
  </el-row>
  <el-row class="row-bg" justify="space-evenly">
    <el-col :span="6"><div class="grid-content ep-bg-purple" /></el-col>
    <el-col :span="6"><div class="grid-content ep-bg-purple-light" /></el-col>
    <el-col :span="6"><div class="grid-content ep-bg-purple" /></el-col>
  </el-row>
</template>

<style>
.el-row {
  margin-bottom: 20px;
}
.el-row:last-child {
  margin-bottom: 0;
}
.el-col {
  border-radius: 4px;
}

.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <el-row>
    <el-col :span="24">
      <div class="grid-content ep-bg-purple-dark" />
    </el-col>
  </el-row>
  <el-row>
    <el-col :span="12">
      <div class="grid-content ep-bg-purple" />
    </el-col>
    <el-col :span="12">
      <div class="grid-content ep-bg-purple-light" />
    </el-col>
  </el-row>
  <el-row>
    <el-col :span="8">
      <div class="grid-content ep-bg-purple" />
    </el-col>
    <el-col :span="8">
      <div class="grid-content ep-bg-purple-light" />
    </el-col>
    <el-col :span="8">
      <div class="grid-content ep-bg-purple" />
    </el-col>
  </el-row>
  <el-row>
    <el-col :span="6">
      <div class="grid-content ep-bg-purple" />
    </el-col>
    <el-col :span="6">
      <div class="grid-content ep-bg-purple-light" />
    </el-col>
    <el-col :span="6">
      <div class="grid-content ep-bg-purple" />
    </el-col>
    <el-col :span="6">
      <div class="grid-content ep-bg-purple-light" />
    </el-col>
  </el-row>
  <el-row>
    <el-col :span="4">
      <div class="grid-content ep-bg-purple" />
    </el-col>
    <el-col :span="4">
      <div class="grid-content ep-bg-purple-light" />
    </el-col>
    <el-col :span="4">
      <div class="grid-content ep-bg-purple" />
    </el-col>
    <el-col :span="4">
      <div class="grid-content ep-bg-purple-light" />
    </el-col>
    <el-col :span="4">
      <div class="grid-content ep-bg-purple" />
    </el-col>
    <el-col :span="4">
      <div class="grid-content ep-bg-purple-light" />
    </el-col>
  </el-row>
</template>

<style>
.el-row {
  margin-bottom: 20px;
}

.el-row:last-child {
  margin-bottom: 0;
}

.el-col {
  border-radius: 4px;
}

.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <el-row :gutter="20">
    <el-col :span="6"><div class="grid-content ep-bg-purple" /></el-col>
    <el-col :span="6" :offset="6">
      <div class="grid-content ep-bg-purple" />
    </el-col>
  </el-row>
  <el-row :gutter="20">
    <el-col :span="6" :offset="6">
      <div class="grid-content ep-bg-purple" />
    </el-col>
    <el-col :span="6" :offset="6">
      <div class="grid-content ep-bg-purple" />
    </el-col>
  </el-row>
  <el-row :gutter="20">
    <el-col :span="12" :offset="6">
      <div class="grid-content ep-bg-purple" />
    </el-col>
  </el-row>
</template>

<style>
.el-row {
  margin-bottom: 20px;
}
.el-row:last-child {
  margin-bottom: 0;
}
.el-col {
  border-radius: 4px;
}

.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
</style>
```

---

## Typography

**URL:** llms-txt#typography

**Contents:**
- Font
- Font Convention
- Font Line Height
- Font-family
- Vue Examples
  - convention.vue
  - font.vue
  - line-height.vue

We create a font convention to ensure the best presentation across different platforms.

---
Title: Upload
URL: https://element-plus.org/en-US/component/upload
---

**Examples:**

Example 1 (css):
```css
font-family:
  Inter, 'Helvetica Neue', Helvetica, 'PingFang SC', 'Hiragino Sans GB',
  'Microsoft YaHei', '微软雅黑', Arial, sans-serif;
```

Example 2 (vue):
```vue
<template>
  <table class="demo-typo-size">
    <tbody>
      <tr>
        <td>Level</td>
        <td>Font Size</td>
        <td class="color-dark-light">Demo</td>
      </tr>
      <tr
        v-for="(fontSize, i) in fontSizes"
        :key="i"
        :style="`font-size: var(--el-font-size-${fontSize.type})`"
      >
        <td>{{ fontSize.level }}</td>
        <td>
          {{
            useCssVar(`--el-font-size-${fontSize.type}`).value +
            ' ' +
            formatType(fontSize.type)
          }}
        </td>
        <td>Build with Element</td>
      </tr>
    </tbody>
  </table>
</template>

<script lang="ts" setup>
import { useCssVar } from '@vueuse/core'

const fontSizes = [
  {
    level: 'Supplementary text',
    type: 'extra-small',
  },
  {
    level: 'Body (small)',
    type: 'small',
  },
  {
    level: 'Body',
    type: 'base',
  },
  {
    level: 'Small Title',
    type: 'medium',
  },
  {
    level: 'Title',
    type: 'large',
  },
  {
    level: 'Main Title',
    type: 'extra-large',
  },
]

function formatType(type: string) {
  return type
    .split('-')
    .map((item) => item.charAt(0).toUpperCase() + item.slice(1))
    .join(' ')
}
</script>
```

Example 3 (vue):
```vue
<script lang="ts" setup>
import { isDark } from '~/composables/dark'
</script>

<template>
  <div v-if="!isDark" class="demo-term-box">
    <img src="/images/typography/term-pingfang.png" alt="" />
    <img src="/images/typography/term-hiragino.png" alt="" />
    <img src="/images/typography/term-microsoft.png" alt="" />
    <img src="/images/typography/term-helvetica.png" alt="" />
    <img src="/images/typography/term-arial.png" alt="" />
  </div>
  <div v-else class="demo-term-box">
    <img src="/images/typography/term-pingfang-dark.png" alt="" />
    <img src="/images/typography/term-hiragino-dark.png" alt="" />
    <img src="/images/typography/term-microsoft-dark.png" alt="" />
    <img src="/images/typography/term-helvetica-dark.png" alt="" />
    <img src="/images/typography/term-arial-dark.png" alt="" />
  </div>
</template>

<style scoped>
img {
  width: 220px;
  height: 174px;
  margin: 0 24px 24px 0;
}
img:nth-of-type(3) {
  margin-right: 0;
}
</style>
```

Example 4 (vue):
```vue
<script lang="ts" setup>
import { isDark } from '~/composables/dark'
</script>

<template>
  <div>
    <img
      v-if="isDark"
      class="lineH-left"
      src="/images/typography/line-height-dark.png"
    />
    <img v-else class="lineH-left" src="/images/typography/line-height.png" />
    <ul class="lineH-right">
      <li>line-height:1 <span>No line height</span></li>
      <li>line-height:1.3 <span>Compact</span></li>
      <li>line-height:1.5 <span>Regular</span></li>
      <li>line-height:1.7 <span>Loose</span></li>
    </ul>
  </div>
</template>
```

---

## Progress

**URL:** llms-txt#progress

**Contents:**
- Linear progress bar
- Internal percentage
- Custom color
- Circular progress bar
- Dashboard progress bar
- Customized content
- Indeterminate progress
- Striped progress
- API
  - Attributes

Progress is used to show the progress of current operation, and inform the user the current status.

## Linear progress bar

## Internal percentage

In this case the percentage takes no additional space.

You can use `color` attr to set the progress bar color. it accepts color string, function, or array.

## Circular progress bar

## Dashboard progress bar

You also can specify `type` attribute to `dashboard` to use dashboard progress bar.

## Customized content

## Indeterminate progress

| Name                   | Description                                                                           | Type                                                                                                        | Default |
| ---------------------- | ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- | ------- |
| percentage ^(required) | percentage                                                                            | ^[number]`(0-100)`                                                                                          | 0       |
| type                   | the type of progress bar                                                              | ^[enum]`'line' \| 'circle' \| 'dashboard'`                                                                  | line    |
| stroke-width           | the width of progress bar                                                             | ^[number]                                                                                                   | 6       |
| text-inside            | whether to place the percentage inside progress bar, only works when `type` is 'line' | ^[boolean]                                                                                                  | false   |
| status                 | the current status of progress bar                                                    | ^[enum]`'success' \| 'exception' \| 'warning'`                                                              | —       |
| indeterminate          | set indeterminate progress                                                            | ^[boolean]                                                                                                  | false   |
| duration               | control the animation duration of indeterminate progress or striped flow progress     | ^[number]                                                                                                   | 3       |
| color                  | background color of progress bar. Overrides `status` prop                             | ^[string] / ^[function]`(percentage: number) => string` / ^[Array]`{ color: string; percentage: number }[]` | ''      |
| width                  | the canvas width of circle progress bar                                               | ^[number]                                                                                                   | 126     |
| show-text              | whether to show percentage                                                            | ^[boolean]                                                                                                  | true    |
| stroke-linecap         | circle/dashboard type shape at the end path                                           | ^[enum]`'butt' \| 'round' \| 'square'`                                                                      | round   |
| format                 | custom text format                                                                    | ^[Function]`(percentage: number) => string`                                                                 | —       |
| striped ^(2.3.4)       | stripe over the progress bar's color                                                  | ^[boolean]                                                                                                  | false   |
| striped-flow ^(2.3.4)  | get the stripes to flow                                                               | ^[boolean]                                                                                                  | false   |

| Name    | Description        | Type                              |
| ------- | ------------------ | --------------------------------- |
| default | Customized content | ^[object]`{ percentage: number }` |

### circular-progress-bar.vue

### customized-content.vue

### dashboard-progress-bar.vue

### indeterminate-progress.vue

### internal-percentage.vue

### linear-progress-bar.vue

### striped-progress.vue

---
Title: Radio
URL: https://element-plus.org/en-US/component/radio
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <div class="demo-progress">
    <el-progress type="circle" :percentage="0" />
    <el-progress type="circle" :percentage="25" />
    <el-progress type="circle" :percentage="100" status="success" />
    <el-progress type="circle" :percentage="70" status="warning" />
    <el-progress type="circle" :percentage="50" status="exception" />
  </div>
</template>

<style scoped>
.demo-progress .el-progress--circle {
  margin-right: 15px;
}
</style>
```

Example 2 (vue):
```vue
<template>
  <div class="demo-progress">
    <el-progress :percentage="percentage" :color="customColor" />

    <el-progress :percentage="percentage" :color="customColorMethod" />

    <el-progress :percentage="percentage" :color="customColors" />
    <el-progress :percentage="percentage" :color="customColors" />
    <div>
      <el-button-group>
        <el-button :icon="Minus" @click="decrease" />
        <el-button :icon="Plus" @click="increase" />
      </el-button-group>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { Minus, Plus } from '@element-plus/icons-vue'

const percentage = ref(20)
const customColor = ref('#409eff')

const customColors = [
  { color: '#f56c6c', percentage: 20 },
  { color: '#e6a23c', percentage: 40 },
  { color: '#5cb87a', percentage: 60 },
  { color: '#1989fa', percentage: 80 },
  { color: '#6f7ad3', percentage: 100 },
]

const customColorMethod = (percentage: number) => {
  if (percentage < 30) {
    return '#909399'
  }
  if (percentage < 70) {
    return '#e6a23c'
  }
  return '#67c23a'
}
const increase = () => {
  percentage.value += 10
  if (percentage.value > 100) {
    percentage.value = 100
  }
}
const decrease = () => {
  percentage.value -= 10
  if (percentage.value < 0) {
    percentage.value = 0
  }
}
</script>

<style scoped>
.demo-progress .el-progress--line {
  margin-bottom: 15px;
  max-width: 600px;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <div class="demo-progress">
    <el-progress :percentage="50">
      <el-button text>Content</el-button>
    </el-progress>
    <el-progress
      :text-inside="true"
      :stroke-width="20"
      :percentage="50"
      status="exception"
    >
      <span>Content</span>
    </el-progress>
    <el-progress type="circle" :percentage="100" status="success">
      <el-button type="success" :icon="Check" circle />
    </el-progress>
    <el-progress type="dashboard" :percentage="80">
      <template #default="{ percentage }">
        <span class="percentage-value">{{ percentage }}%</span>
        <span class="percentage-label">Progressing</span>
      </template>
    </el-progress>
  </div>
</template>

<script lang="ts" setup>
import { Check } from '@element-plus/icons-vue'
</script>

<style scoped>
.percentage-value {
  display: block;
  margin-top: 10px;
  font-size: 28px;
}
.percentage-label {
  display: block;
  margin-top: 10px;
  font-size: 12px;
}
.demo-progress .el-progress--line {
  margin-bottom: 15px;
  max-width: 600px;
}
.demo-progress .el-progress--circle {
  margin-right: 15px;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <div class="demo-progress">
    <el-progress type="dashboard" :percentage="percentage" :color="colors" />
    <el-progress type="dashboard" :percentage="percentage2" :color="colors" />
    <div>
      <el-button-group>
        <el-button :icon="Minus" @click="decrease" />
        <el-button :icon="Plus" @click="increase" />
      </el-button-group>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { onMounted, ref } from 'vue'
import { Minus, Plus } from '@element-plus/icons-vue'

const percentage = ref(10)
const percentage2 = ref(0)

const colors = [
  { color: '#f56c6c', percentage: 20 },
  { color: '#e6a23c', percentage: 40 },
  { color: '#5cb87a', percentage: 60 },
  { color: '#1989fa', percentage: 80 },
  { color: '#6f7ad3', percentage: 100 },
]

const increase = () => {
  percentage.value += 10
  if (percentage.value > 100) {
    percentage.value = 100
  }
}
const decrease = () => {
  percentage.value -= 10
  if (percentage.value < 0) {
    percentage.value = 0
  }
}
onMounted(() => {
  setInterval(() => {
    percentage2.value = (percentage2.value % 100) + 10
  }, 500)
})
</script>

<style scoped>
.demo-progress .el-progress--line {
  margin-bottom: 15px;
  max-width: 600px;
}
.demo-progress .el-progress--circle {
  margin-right: 15px;
}
</style>
```

---

## Link

**URL:** llms-txt#link

**Contents:**
- Basic
- Disabled
- Underline
- Icon
- API
  - Attributes
  - Slots
- Vue Examples
  - basic.vue
  - disabled.vue

Disabled state of link

Controlling when underlines should appear

| Name      | Description                         | Type                                                                            | Default |
| --------- | ----------------------------------- | ------------------------------------------------------------------------------- | ------- |
| type      | type                                | ^[enum]`'primary' \| 'success' \| 'warning' \| 'danger' \| 'info' \| 'default'` | default |
| underline | when underlines should appear       | ^[enum]`'always' \| 'hover' \| 'never' \| boolean`                              | hover   |
| disabled  | whether the component is disabled   | ^[boolean]                                                                      | false   |
| href      | same as native hyperlink's `href`   | ^[string]                                                                       | —       |
| target    | same as native hyperlink's `target` | ^[enum]`'_blank' \| '_parent' \| '_self' \| '_top'`                             | \_self  |
| icon      | icon component                      | ^[string] / ^[Component]                                                        | —       |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |
| icon    | customize icon component  |

---
Title: Loading
URL: https://element-plus.org/en-US/component/loading
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <!-- works before 2.9.9, use 'hover' after, removed in 3.0.0 -->
  <el-link underline>link</el-link>
  <!-- works before 2.9.9, use 'never' after, removed in 3.0.0 -->
  <el-link :underline="false">link</el-link>
</template>
```

Example 2 (vue):
```vue
<template>
  <div>
    <el-link href="https://element-plus.org" target="_blank">default</el-link>
    <el-link type="primary">primary</el-link>
    <el-link type="success">success</el-link>
    <el-link type="warning">warning</el-link>
    <el-link type="danger">danger</el-link>
    <el-link type="info">info</el-link>
  </div>
</template>

<style scoped>
.el-link {
  margin-right: 8px;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <div>
    <el-link disabled>default</el-link>
    <el-link type="primary" disabled>primary</el-link>
    <el-link type="success" disabled>success</el-link>
    <el-link type="warning" disabled>warning</el-link>
    <el-link type="danger" disabled>danger</el-link>
    <el-link type="info" disabled>info</el-link>
  </div>
</template>

<style scoped>
.el-link {
  margin-right: 8px;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <div>
    <el-link>default</el-link>
    <el-link underline="always">always</el-link>
    <el-link underline="hover">hover</el-link>
    <el-link underline="never">never</el-link>
  </div>
</template>

<style scoped>
.el-link {
  margin-right: 8px;
}
</style>
```

---

## Pagination

**URL:** llms-txt#pagination

**Contents:**
- Basic usage
- Number of pagers
- Buttons with background color
- Small Pagination
- Hide pagination when there is only one page
- More elements
- API
  - Attributes
  - Events
  - Slots

If you have too much data to display in one page, use pagination.

## Buttons with background color

Use small pagination in the case of limited space.

## Hide pagination when there is only one page

When there is only one page, hide the pagination by setting the `hide-on-single-page` attribute.

Add more modules based on your scenario.

| Name                                | Description                                                                                                                     | Type                                                                              | Default                              |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | ------------------------------------ |
| size ^(2.7.6)                       | pagination size                                                                                                                 | ^[enum]`'large' \| 'default' \| 'small'`                                          | 'default'                            |
| background                          | whether the buttons have a background color                                                                                     | ^[boolean]                                                                        | false                                |
| page-size / v-model:page-size       | item count of each page                                                                                                         | ^[number]                                                                         | —                                    |
| default-page-size                   | default initial value of page size, not setting is the same as setting 10                                                       | ^[number]                                                                         | —                                    |
| total                               | total item count                                                                                                                | ^[number]                                                                         | —                                    |
| page-count                          | total page count. Set either `total` or `page-count` and pages will be displayed; if you need `page-sizes`, `total` is required | ^[number]                                                                         | —                                    |
| pager-count                         | number of pagers. Pagination collapses when the total page count exceeds this value                                             | ^[number]`5 \| 7 \| 9 \| 11 \| 13 \| 15 \| 17 \| 19 \| 21`                        | 7                                    |
| current-page / v-model:current-page | current page number                                                                                                             | ^[number]                                                                         | —                                    |
| default-current-page                | default initial value of current-page, not setting is the same as setting 1                                                     | ^[number]                                                                         | —                                    |
| layout                              | layout of Pagination, elements separated with a comma                                                                           | ^[string]`string (consists of sizes, prev, pager, next, jumper, ->, total, slot)` | prev, pager, next, jumper, ->, total |
| page-sizes                          | options of item count per page                                                                                                  | ^[object]`number[]`                                                               | [10, 20, 30, 40, 50, 100]            |
| append-size-to ^(2.8.4)             | which element the size dropdown appends to                                                                                      | ^[string]                                                                         | —                                    |
| popper-class                        | custom class name for the page size Select's dropdown                                                                           | ^[string]                                                                         | ''                                   |
| popper-style ^(2.11.5)              | custom style for the page size Select's dropdown                                                                                | ^[string] / ^[object]                                                             | —                                    |
| prev-text                           | text for the prev button                                                                                                        | ^[string]                                                                         | ''                                   |
| prev-icon                           | icon for the prev button, has a lower priority than `prev-text`                                                                 | ^[string] / ^[Component]                                                          | ArrowLeft                            |
| next-text                           | text for the next button                                                                                                        | ^[string]                                                                         | ''                                   |
| next-icon                           | icon for the next button, has a lower priority than `next-text`                                                                 | ^[string] / ^[Component]                                                          | ArrowRight                           |
| disabled                            | whether Pagination is disabled                                                                                                  | ^[boolean]                                                                        | false                                |
| teleported ^(2.3.13)                | whether Pagination select dropdown is teleported to the body                                                                    | ^[boolean]                                                                        | true                                 |
| hide-on-single-page                 | whether to hide when there's only one page                                                                                      | ^[boolean]                                                                        | false                                |
| small ^(deprecated)                 | whether to use small pagination                                                                                                 | ^[boolean]                                                                        | false                                |

| Name            | Description                                                       | Type                                                         |
| --------------- | ----------------------------------------------------------------- | ------------------------------------------------------------ |
| size-change     | triggers when `page-size` changes                                 | ^[Function]`(value: number) => void`                         |
| current-change  | triggers when `current-page` changes                              | ^[Function]`(value: number) => void`                         |
| change ^(2.4.4) | triggers when `current-page` or `page-size` changes               | ^[Function]`(currentPage: number, pageSize: number) => void` |
| prev-click      | triggers when the prev button is clicked and current page changes | ^[Function]`(value: number) => void`                         |
| next-click      | triggers when the next button is clicked and current page changes | ^[Function]`(value: number) => void`                         |

| Name    | Description                                                         |
| ------- | ------------------------------------------------------------------- |
| default | custom content. To use this, you need to declare `slot` in `layout` |

### auto-hide-pagination.vue

### background-color.vue

### more-elements.vue

### number-of-pagers.vue

### small-pagination.vue

---
Title: Popconfirm
URL: https://element-plus.org/en-US/component/popconfirm
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <div>
    <el-switch v-model="value" />
    <hr class="my-4" />
    <el-pagination
      :hide-on-single-page="value"
      :total="5"
      layout="prev, pager, next"
    />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref(false)
</script>
```

Example 2 (vue):
```vue
<template>
  <el-pagination background layout="prev, pager, next" :total="1000" />
</template>
```

Example 3 (vue):
```vue
<template>
  <div class="example-pagination-block">
    <div class="example-demonstration">When you have few pages</div>
    <el-pagination layout="prev, pager, next" :total="50" />
  </div>
  <div class="example-pagination-block">
    <div class="example-demonstration">When you have more than 7 pages</div>
    <el-pagination layout="prev, pager, next" :total="1000" />
  </div>
</template>

<style scoped>
.example-pagination-block + .example-pagination-block {
  margin-top: 10px;
}
.example-pagination-block .example-demonstration {
  margin-bottom: 16px;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <div class="flex items-center mb-4">
    <el-radio-group v-model="size" class="mr-4">
      <el-radio-button value="default">default</el-radio-button>
      <el-radio-button value="large">large</el-radio-button>

      <el-radio-button value="small">small</el-radio-button>
    </el-radio-group>
    <div>
      background:
      <el-switch v-model="background" class="ml-2" />
    </div>
    <div class="ml-4">
      disabled: <el-switch v-model="disabled" class="ml-2" />
    </div>
  </div>

  <hr class="my-4" />

  <div class="demo-pagination-block">
    <div class="demonstration">Total item count</div>
    <el-pagination
      v-model:current-page="currentPage1"
      :page-size="100"
      :size="size"
      :disabled="disabled"
      :background="background"
      layout="total, prev, pager, next"
      :total="1000"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
    />
  </div>
  <div class="demo-pagination-block">
    <div class="demonstration">Change page size</div>
    <el-pagination
      v-model:current-page="currentPage2"
      v-model:page-size="pageSize2"
      :page-sizes="[100, 200, 300, 400]"
      :size="size"
      :disabled="disabled"
      :background="background"
      layout="sizes, prev, pager, next"
      :total="1000"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
    />
  </div>
  <div class="demo-pagination-block">
    <div class="demonstration">Jump to</div>
    <el-pagination
      v-model:current-page="currentPage3"
      v-model:page-size="pageSize3"
      :size="size"
      :disabled="disabled"
      :background="background"
      layout="prev, pager, next, jumper"
      :total="1000"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
    />
  </div>
  <div class="demo-pagination-block">
    <div class="demonstration">All combined</div>
    <el-pagination
      v-model:current-page="currentPage4"
      v-model:page-size="pageSize4"
      :page-sizes="[100, 200, 300, 400]"
      :size="size"
      :disabled="disabled"
      :background="background"
      layout="total, sizes, prev, pager, next, jumper"
      :total="400"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
    />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

import type { ComponentSize } from 'element-plus'

const currentPage1 = ref(5)
const currentPage2 = ref(5)
const currentPage3 = ref(5)
const currentPage4 = ref(4)
const pageSize2 = ref(100)
const pageSize3 = ref(100)
const pageSize4 = ref(100)
const size = ref<ComponentSize>('default')
const background = ref(false)
const disabled = ref(false)

const handleSizeChange = (val: number) => {
  console.log(`${val} items per page`)
}
const handleCurrentChange = (val: number) => {
  console.log(`current page: ${val}`)
}
</script>

<style scoped>
.demo-pagination-block + .demo-pagination-block {
  margin-top: 10px;
}
.demo-pagination-block .demonstration {
  margin-bottom: 16px;
}
</style>
```

---

## Container

**URL:** llms-txt#container

**Contents:**
- Common layouts
- Example
- Container API
  - Container Attributes
  - Container Slots
- Header API
  - Header Attributes
  - Header Slots
- Aside API
  - Aside Attributes

Container components for scaffolding basic structure of the page:

`<el-container>`: wrapper container. When nested with a `<el-header>` or `<el-footer>`, all its child elements will be vertically arranged. Otherwise horizontally.

`<el-header>`: container for headers.

`<el-aside>`: container for side sections (usually a side nav).

`<el-main>`: container for main sections.

`<el-footer>`: container for footers.

<style lang="scss">
@use '../../examples/container/common-layout.scss';
</style>

### Container Attributes

| Name      | Description                         | Type                                | Default                                                                    |
| --------- | ----------------------------------- | ----------------------------------- | -------------------------------------------------------------------------- |
| direction | layout direction for child elements | ^[enum]`'horizontal' \| 'vertical'` | vertical when nested with `el-header` or `el-footer`; horizontal otherwise |

| Name    | Description               | Subtags                                    |
| ------- | ------------------------- | ------------------------------------------ |
| default | customize default content | Container / Header / Aside / Main / Footer |

### Header Attributes

| Name   | Description          | Type      | Default |
| ------ | -------------------- | --------- | ------- |
| height | height of the header | ^[string] | 60px    |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

| Name  | Description               | Type      | Default |
| ----- | ------------------------- | --------- | ------- |
| width | width of the side section | ^[string] | 300px   |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

### Footer Attributes

| Name   | Description          | Type      | Default |
| ------ | -------------------- | --------- | ------- |
| height | height of the footer | ^[string] | 60px    |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

---
Title: DatePickerPanel
URL: https://element-plus.org/en-US/component/date-picker-panel
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-container class="layout-container-demo" style="height: 500px">
    <el-aside width="200px">
      <el-scrollbar>
        <el-menu :default-openeds="['1', '3']">
          <el-sub-menu index="1">
            <template #title>
              <el-icon><message /></el-icon>Navigator One
            </template>
            <el-menu-item-group>
              <template #title>Group 1</template>
              <el-menu-item index="1-1">Option 1</el-menu-item>
              <el-menu-item index="1-2">Option 2</el-menu-item>
            </el-menu-item-group>
            <el-menu-item-group title="Group 2">
              <el-menu-item index="1-3">Option 3</el-menu-item>
            </el-menu-item-group>
            <el-sub-menu index="1-4">
              <template #title>Option4</template>
              <el-menu-item index="1-4-1">Option 4-1</el-menu-item>
            </el-sub-menu>
          </el-sub-menu>
          <el-sub-menu index="2">
            <template #title>
              <el-icon><icon-menu /></el-icon>Navigator Two
            </template>
            <el-menu-item-group>
              <template #title>Group 1</template>
              <el-menu-item index="2-1">Option 1</el-menu-item>
              <el-menu-item index="2-2">Option 2</el-menu-item>
            </el-menu-item-group>
            <el-menu-item-group title="Group 2">
              <el-menu-item index="2-3">Option 3</el-menu-item>
            </el-menu-item-group>
            <el-sub-menu index="2-4">
              <template #title>Option 4</template>
              <el-menu-item index="2-4-1">Option 4-1</el-menu-item>
            </el-sub-menu>
          </el-sub-menu>
          <el-sub-menu index="3">
            <template #title>
              <el-icon><setting /></el-icon>Navigator Three
            </template>
            <el-menu-item-group>
              <template #title>Group 1</template>
              <el-menu-item index="3-1">Option 1</el-menu-item>
              <el-menu-item index="3-2">Option 2</el-menu-item>
            </el-menu-item-group>
            <el-menu-item-group title="Group 2">
              <el-menu-item index="3-3">Option 3</el-menu-item>
            </el-menu-item-group>
            <el-sub-menu index="3-4">
              <template #title>Option 4</template>
              <el-menu-item index="3-4-1">Option 4-1</el-menu-item>
            </el-sub-menu>
          </el-sub-menu>
        </el-menu>
      </el-scrollbar>
    </el-aside>

    <el-container>
      <el-header style="text-align: right; font-size: 12px">
        <div class="toolbar">
          <el-dropdown>
            <el-icon style="margin-right: 8px; margin-top: 1px">
              <setting />
            </el-icon>
            <template #dropdown>
              <el-dropdown-menu>
                <el-dropdown-item>View</el-dropdown-item>
                <el-dropdown-item>Add</el-dropdown-item>
                <el-dropdown-item>Delete</el-dropdown-item>
              </el-dropdown-menu>
            </template>
          </el-dropdown>
          <span>Tom</span>
        </div>
      </el-header>

      <el-main>
        <el-scrollbar>
          <el-table :data="tableData">
            <el-table-column prop="date" label="Date" width="140" />
            <el-table-column prop="name" label="Name" width="120" />
            <el-table-column prop="address" label="Address" />
          </el-table>
        </el-scrollbar>
      </el-main>
    </el-container>
  </el-container>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { Menu as IconMenu, Message, Setting } from '@element-plus/icons-vue'

const item = {
  date: '2016-05-02',
  name: 'Tom',
  address: 'No. 189, Grove St, Los Angeles',
}
const tableData = ref(Array.from({ length: 20 }).fill(item))
</script>

<style scoped>
.layout-container-demo .el-header {
  position: relative;
  background-color: var(--el-color-primary-light-7);
  color: var(--el-text-color-primary);
}
.layout-container-demo .el-aside {
  color: var(--el-text-color-primary);
  background: var(--el-color-primary-light-8);
}
.layout-container-demo .el-menu {
  border-right: none;
}
.layout-container-demo .el-main {
  padding: 0;
}
.layout-container-demo .toolbar {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  right: 20px;
}
</style>
```

Example 2 (vue):
```vue
<template>
  <div class="common-layout">
    <el-container>
      <el-aside width="200px">Aside</el-aside>
      <el-container>
        <el-header>Header</el-header>
        <el-main>Main</el-main>
      </el-container>
    </el-container>
  </div>
</template>
```

Example 3 (vue):
```vue
<template>
  <div class="common-layout">
    <el-container>
      <el-aside width="200px">Aside</el-aside>
      <el-container>
        <el-header>Header</el-header>
        <el-main>Main</el-main>
        <el-footer>Footer</el-footer>
      </el-container>
    </el-container>
  </div>
</template>
```

Example 4 (vue):
```vue
<template>
  <div class="common-layout">
    <el-container>
      <el-aside width="200px">Aside</el-aside>
      <el-main>Main</el-main>
      <el-aside width="200px">Aside</el-aside>
    </el-container>
  </div>
</template>
```

---

## Switch

**URL:** llms-txt#switch

**Contents:**
- Basic usage
- Sizes
- Text description
- Display custom icons
- Extended value types
- Disabled
- Loading
- Prevent switching
- Custom action icon ^(2.3.9)
- Custom action slot ^(2.4.4)

Switch is used for switching between two opposing states.

You can add `active-text` and `inactive-text` attribute to show texts. use `inline-prompt` attribute to control text is displayed inside dot.

## Display custom icons

## Extended value types

## Custom action icon ^(2.3.9)

## Custom action slot ^(2.4.4)

| Name                          | Description                                                                                                                                     | Type                                           | Default |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | ------- |
| model-value / v-model         | binding value, it should be equivalent to either `active-value` or `inactive-value`, by default it's `boolean` type                             | ^[boolean] / ^[string] / ^[number]             | false   |
| disabled                      | whether Switch is disabled                                                                                                                      | ^[boolean]                                     | false   |
| loading                       | whether Switch is in loading state                                                                                                              | ^[boolean]                                     | false   |
| size                          | size of Switch                                                                                                                                  | ^[enum]`'' \| 'large' \| 'default' \| 'small'` | ''      |
| width                         | width of Switch                                                                                                                                 | ^[number] / ^[string]                          | ''      |
| inline-prompt                 | whether icon or text is displayed inside dot, only the first character will be rendered for text                                                | ^[boolean]                                     | false   |
| active-icon                   | component of the icon displayed when in `on` state, overrides `active-text`                                                                     | ^[string] / ^[Component]                       | —       |
| inactive-icon                 | component of the icon displayed when in `off` state, overrides `inactive-text`                                                                  | ^[string] / ^[Component]                       | —       |
| active-action-icon ^(2.3.9)   | component of the icon displayed in action when in `on` state                                                                                    | ^[string] / ^[Component]                       | —       |
| inactive-action-icon ^(2.3.9) | component of the icon displayed in action when in `off` state                                                                                   | ^[string] / ^[Component]                       | —       |
| active-text                   | text displayed when in `on` state                                                                                                               | ^[string]                                      | ''      |
| inactive-text                 | text displayed when in `off` state                                                                                                              | ^[string]                                      | ''      |
| active-value                  | switch value when in `on` state                                                                                                                 | ^[boolean] / ^[string] / ^[number]             | true    |
| inactive-value                | switch value when in `off` state                                                                                                                | ^[boolean] / ^[string] / ^[number]             | false   |
| name                          | input name of Switch                                                                                                                            | ^[string]                                      | ''      |
| validate-event                | whether to trigger form validation                                                                                                              | ^[boolean]                                     | true    |
| before-change                 | before-change hook before the switch state changes. If `false` is returned or a `Promise` is returned and then is rejected, will stop switching | ^[Function]`() => Promise<boolean> \| boolean` | —       |
| id                            | id for input                                                                                                                                    | ^[string]                                      | —       |
| tabindex                      | tabindex for input                                                                                                                              | ^[string] / ^[number]                          | —       |
| aria-label ^(a11y) ^(2.7.2)   | same as `aria-label` in native input                                                                                                            | ^[string]                                      | —       |
| active-color ^(deprecated)    | background color when in `on` state ( use CSS var `--el-switch-on-color` instead )                                                              | ^[string]                                      | ''      |
| inactive-color ^(deprecated)  | background color when in `off` state ( use CSS var `--el-switch-off-color` instead )                                                            | ^[string]                                      | ''      |
| border-color ^(deprecated)    | border color of the switch ( use CSS var `--el-switch-border-color` instead )                                                                   | ^[string]                                      | ''      |
| label ^(a11y) ^(deprecated)   | same as `aria-label` in native input                                                                                                            | ^[string]                                      | —       |

| Name   | Description                 | Type                                                    |
| ------ | --------------------------- | ------------------------------------------------------- |
| change | triggers when value changes | ^[Function]`(val: boolean \| string \| number) => void` |

| Name                     | Description                |
| ------------------------ | -------------------------- |
| active-action ^(2.4.4)   | customize active action    |
| inactive-action ^(2.4.4) | customize inactive action  |
| active ^(2.13.0)         | customize active content   |
| inactive ^(2.13.0)       | customize inactive content |

| Method | Description                          | Type                    |
| ------ | ------------------------------------ | ----------------------- |
| focus  | manual focus to the switch component | ^[Function]`() => void` |

### custom-action-icon.vue

### custom-action-slot.vue

### extended-value-types.vue

### prevent-switching.vue

### text-description.vue

---
Title: Virtualized Table
URL: https://element-plus.org/en-US/component/table-v2
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-switch v-model="value1" />
  <el-switch
    v-model="value2"
    class="ml-2"
    style="--el-switch-on-color: #13ce66; --el-switch-off-color: #ff4949"
  />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value1 = ref(true)
const value2 = ref(true)
</script>
```

Example 2 (vue):
```vue
<template>
  <el-switch
    v-model="value1"
    :active-action-icon="View"
    :inactive-action-icon="Hide"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { Hide, View } from '@element-plus/icons-vue'

const value1 = ref(true)
</script>
```

Example 3 (vue):
```vue
<template>
  <el-switch v-model="value1">
    <template #active-action>
      <span class="custom-active-action">T</span>
    </template>
    <template #inactive-action>
      <span class="custom-inactive-action">F</span>
    </template>
  </el-switch>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const value1 = ref(true)
</script>
```

Example 4 (vue):
```vue
<template>
  <el-switch v-model="value1" :active-icon="Check" :inactive-icon="Close" />
  <br />
  <el-switch
    v-model="value2"
    class="mt-2"
    style="margin-left: 24px"
    inline-prompt
    :active-icon="Check"
    :inactive-icon="Close"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { Check, Close } from '@element-plus/icons-vue'

const value1 = ref(true)
const value2 = ref(true)
</script>
```

---

## Text

**URL:** llms-txt#text

**Contents:**
- Basic
- Sizes
- Ellipsis
- Override
- Mixed
- API
  - Attributes
  - Slots
- Vue Examples
  - basic.vue

| Name                | Description        | Type                                                               | Default |
| ------------------- | ------------------ | ------------------------------------------------------------------ | ------- |
| type                | text type          | ^[enum]`'primary' \| 'success' \| 'warning' \| 'danger' \| 'info'` | —       |
| size                | text size          | ^[enum]`'large' \| 'default' \| 'small'`                           | default |
| truncated           | render ellipsis    | ^[boolean]                                                         | false   |
| line-clamp ^(2.4.0) | maximum lines      | ^[string] / ^[number]                                              | —       |
| tag                 | custom element tag | ^[string]                                                          | span    |

| Name    | Description     |
| ------- | --------------- |
| default | default content |

---
Title: TimePicker
URL: https://element-plus.org/en-US/component/time-picker
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-text class="mx-1">Default</el-text>
  <el-text class="mx-1" type="primary">Primary</el-text>
  <el-text class="mx-1" type="success">Success</el-text>
  <el-text class="mx-1" type="info">Info</el-text>
  <el-text class="mx-1" type="warning">Warning</el-text>
  <el-text class="mx-1" type="danger">Danger</el-text>
</template>
```

Example 2 (vue):
```vue
<template>
  <el-space direction="vertical">
    <el-text>
      <el-icon>
        <ElementPlus />
      </el-icon>
      Element-Plus
    </el-text>
    <el-row>
      <el-text>Rate</el-text>
      <el-rate class="ml-1" />
    </el-row>
    <el-text>
      This is text mixed icon
      <el-icon>
        <Bell />
      </el-icon>
      and component
      <el-button>Button</el-button>
    </el-text>
  </el-space>
</template>

<script lang="ts" setup>
import { Bell, ElementPlus } from '@element-plus/icons-vue'
</script>
```

Example 3 (vue):
```vue
<template>
  <el-space direction="vertical">
    <el-text>span</el-text>
    <el-text tag="p">This is a paragraph.</el-text>
    <el-text tag="b">Bold</el-text>
    <el-text tag="i">Italic</el-text>
    <el-text>
      This is
      <el-text tag="sub" size="small">subscript</el-text>
    </el-text>
    <el-text>
      This is
      <el-text tag="sup" size="small">superscript</el-text>
    </el-text>
    <el-text tag="ins">Inserted</el-text>
    <el-text tag="del">Deleted</el-text>
    <el-text tag="mark">Marked</el-text>
  </el-space>
</template>
```

Example 4 (vue):
```vue
<template>
  <el-text class="mx-1" size="large">Large</el-text>
  <el-text class="mx-1">Default</el-text>
  <el-text class="mx-1" size="small">Small</el-text>
</template>
```

---

## Icon

**URL:** llms-txt#icon

**Contents:**
- Icon Usage
- Installation
  - Using packaging manager
  - Register All Icons
  - Import in Browser
  - Auto Import
- Simple Usage
- Combined with el-icon
- Using SVG icon directly
- Icon Collection{#icon-collection}

Element Plus provides a set of common icons.

- If you want to **use directly** like the example, you need to [globally register](https://v3.vuejs.org/guide/component-registration.html#global-registration) the components before using it.

- If you want to see all available SVG icons please check [@element-plus/icons-vue@1.x](https://unpkg.com/browse/@element-plus/icons-vue@1/dist/es/)[@element-plus/icons-vue@latest](https://unpkg.com/browse/@element-plus/icons-vue@latest/dist/types/components/) and the source [element-plus-icons](https://github.com/element-plus/element-plus-icons) out or [Icon Collection](#icon-collection)

### Using packaging manager

Choose a package manager you like.

### Register All Icons

You need import all icons from `@element-plus/icons-vue` and register them globally.

You can also refer to [this template](https://codepen.io/sxzz/pen/xxpvdrg).

### Import in Browser

Import Element Plus Icons through browser HTML tags directly, and use global variable `ElementPlusIconsVue`.

According to different CDN providers, there are different introduction methods.
Here we use [unpkg](https://unpkg.com) and [jsDelivr](https://jsdelivr.com) as example.
You can also use other CDN providers.

Use [unplugin-icons](https://github.com/antfu/unplugin-icons) and [unplugin-auto-import](https://github.com/antfu/unplugin-auto-import)
to automatically import any icon collections from iconify.
You can refer to [this template](https://github.com/sxzz/element-plus-best-practices/blob/db2dfc983ccda5570033a0ac608a1bd9d9a7f658/vite.config.ts#L21-L58).

<vp-script setup>
import { Edit, Share, Delete, Search, Loading } from '@element-plus/icons-vue'
</vp-script>

<ElRow>
  <div>
    <ElIcon :size="30">
      <Edit />
    </ElIcon>
    <Edit />
  </div>
</ElRow>

## Combined with el-icon

`el-icon` provides extra attributes for raw SVG icon, for more detail, please read to the end.

<ElRow>
  <p>
    with extra class <b>is-loading</b>, your icon is able to rotate 360 deg in 2
    seconds, you can also override this
  </p>
  <div style="display: flex; align-items: center; justify-content: space-between; width: 100%;">
    <ElIcon :size="20">
      <Edit />
    </ElIcon>
    <ElIcon color="#409efc" class="no-inherit">
      <Share />
    </ElIcon>
    <ElIcon>
      <Delete />
    </ElIcon>
    <ElIcon class="is-loading">
      <Loading />
    </ElIcon>
    <ElButton type="primary">
      <ElIcon style="vertical-align: middle; color: #fff;">
        <Search />
      </ElIcon>
      <span style="vertical-align: middle;"> Search </span>
    </ElButton>
  </div>
</ElRow>

## Using SVG icon directly

<ElRow>
  <div style="font-size: 20px;">
    <!-- Since svg icons do not carry any attributes by default -->
    <!-- You need to provide attributes directly -->
    <Edit style="width: 1em; height: 1em; margin-right: 8px;" />
    <Share style="width: 1em; height: 1em; margin-right: 8px;" />
    <Delete style="width: 1em; height: 1em; margin-right: 8px;" />
    <Search style="width: 1em; height: 1em; margin-right: 8px;" />
  </div>
</ElRow>

## Icon Collection{#icon-collection}

| Name  | Description                | Type                  | Default                |
| ----- | -------------------------- | --------------------- | ---------------------- |
| color | SVG tag's fill attribute   | ^[string]             | inherit from color     |
| size  | SVG icon size, size x size | ^[number] / ^[string] | inherit from font size |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

---
Title: Image
URL: https://element-plus.org/en-US/component/image
---

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown

```

Example 3 (unknown):
```unknown
:::

### Register All Icons

You need import all icons from `@element-plus/icons-vue` and register them globally.
```

Example 4 (unknown):
```unknown
You can also refer to [this template](https://codepen.io/sxzz/pen/xxpvdrg).

### Import in Browser

Import Element Plus Icons through browser HTML tags directly, and use global variable `ElementPlusIconsVue`.

According to different CDN providers, there are different introduction methods.
Here we use [unpkg](https://unpkg.com) and [jsDelivr](https://jsdelivr.com) as example.
You can also use other CDN providers.

#### unpkg
```

---

## ColorPicker

**URL:** llms-txt#colorpicker

**Contents:**
- Basic usage
- Alpha
- Predefined colors
- Sizes
- API
  - Attributes
  - Events
  - Exposes
- Vue Examples
  - alpha.vue

ColorPicker is a color selector supporting multiple color formats.

| Name                        | Description                                                                                                    | Type                                                                                                             | Default |
| --------------------------- | -------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------- |
| model-value / v-model       | binding value                                                                                                  | ^[string]                                                                                                        | —       |
| disabled                    | whether to disable the ColorPicker                                                                             | ^[boolean]                                                                                                       | false   |
| size                        | size of ColorPicker                                                                                            | ^[enum]`'large' \| 'default' \| 'small'`                                                                         | —       |
| show-alpha                  | whether to display the alpha slider                                                                            | ^[boolean]                                                                                                       | false   |
| color-format                | color format of v-model                                                                                        | ^[enum]`'hsl' \| 'hsv' \| 'hex' \| 'rgb' \| 'hex' (when show-alpha is false) \| 'rgb' (when show-alpha is true)` | —       |
| popper-class                | custom class name for ColorPicker's dropdown                                                                   | ^[string] / ^[object]                                                                                            | ''      |
| popper-style ^(2.11.4)      | custom style for ColorPicker's dropdown                                                                        | ^[string] / ^[object]                                                                                            | —       |
| predefine                   | predefined color options                                                                                       | ^[object]`string[]`                                                                                              | —       |
| validate-event              | whether to trigger form validation                                                                             | ^[boolean]                                                                                                       | true    |
| tabindex                    | ColorPicker tabindex                                                                                           | ^[string] / ^[number]                                                                                            | 0       |
| aria-label ^(a11y) ^(2.7.2) | ColorPicker aria-label                                                                                         | ^[string]                                                                                                        | —       |
| empty-values ^(2.10.3)      | empty values of component, [see config-provider](/en-US/component/config-provider#empty-values-configurations) | ^[array]                                                                                                         | —       |
| value-on-clear ^(2.10.3)    | clear return value, [see config-provider](/en-US/component/config-provider#empty-values-configurations)        | ^[string] / ^[number] / ^[boolean] / ^[Function]                                                                 | —       |
| id                          | ColorPicker id                                                                                                 | ^[string]                                                                                                        | —       |
| teleported ^(2.7.2)         | whether color-picker popper is teleported to the body                                                          | ^[boolean]                                                                                                       | true    |
| label ^(a11y) ^(deprecated) | ColorPicker aria-label                                                                                         | ^[string]                                                                                                        | —       |
| persistent ^(2.10.5)        | when color-picker inactive and persistent is false, the color panel will be destroyed                          | ^[boolean]                                                                                                       | true    |
| append-to ^(2.10.5)         | which element the color-picker panel appends to                                                                | ^[CSSSelector] / ^[HTMLElement]                                                                                  | -       |

| Name           | Description                                    | Type                                     |
| -------------- | ---------------------------------------------- | ---------------------------------------- |
| change         | triggers when input value changes              | ^[Function]`(value: string) => void`     |
| active-change  | triggers when the current active color changes | ^[Function]`(value: string) => void`     |
| focus ^(2.4.0) | triggers when Component focuses                | ^[Function]`(event: FocusEvent) => void` |
| blur ^(2.4.0)  | triggers when Component blurs                  | ^[Function]`(event: FocusEvent) => void` |

| Name            | Description               | Type                    |
| --------------- | ------------------------- | ----------------------- |
| color           | current color object      | ^[object]`Color`        |
| show ^(2.3.3)   | manually show ColorPicker | ^[Function]`() => void` |
| hide ^(2.3.3)   | manually hide ColorPicker | ^[Function]`() => void` |
| focus ^(2.3.13) | focus the picker element  | ^[Function]`() => void` |
| blur ^(2.3.13)  | blur the picker element   | ^[Function]`() => void` |

### predefined-color.vue

---
Title: Color
URL: https://element-plus.org/en-US/component/color
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-color-picker v-model="color" show-alpha />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const color = ref('rgba(19, 206, 102, 0.8)')
</script>
```

Example 2 (vue):
```vue
<template>
  <div class="demo-color-block">
    <span class="demonstration">With default value</span>
    <el-color-picker v-model="color1" />
  </div>
  <div class="demo-color-block">
    <span class="demonstration">With no default value</span>
    <el-color-picker v-model="color2" />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const color1 = ref('#409EFF')
const color2 = ref()
</script>

<style>
.demo-color-block {
  display: flex;
  align-items: center;
  margin-bottom: 16px;
}
.demo-color-block .demonstration {
  margin-right: 16px;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <el-color-picker v-model="color" show-alpha :predefine="predefineColors" />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const color = ref('rgba(255, 69, 0, 0.68)')
const predefineColors = ref([
  '#ff4500',
  '#ff8c00',
  '#ffd700',
  '#90ee90',
  '#00ced1',
  '#1e90ff',
  '#c71585',
  'rgba(255, 69, 0, 0.68)',
  'rgb(255, 120, 0)',
  'hsv(51, 100, 98)',
  'hsva(120, 40, 94, 0.5)',
  'hsl(181, 100%, 37%)',
  'hsla(209, 100%, 56%, 0.73)',
  '#c7158577',
])
</script>
```

Example 4 (vue):
```vue
<template>
  <div class="demo-color-sizes">
    <el-color-picker v-model="color" size="large" />
    <el-color-picker v-model="color" />
    <el-color-picker v-model="color" size="small" />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const color = ref('#409EFF')
</script>

<style>
.demo-color-sizes .el-color-picker:not(:last-child) {
  margin-right: 16px;
}
</style>
```

---

## Tabs

**URL:** llms-txt#tabs

**Contents:**
- Basic usage
- Card Style
- Border card
- Tab position
- Custom Tab
- Add & close tab
- Customized add button icon ^(2.4.0)
- Customized trigger button of new tab
- Default value ^(2.11.9)
- Tabs API

Divide data collections which are related yet belong to different types.

Basic and concise tabs.

Tabs styled as cards.

You can use `tab-position` attribute to set the tab's position.

You can use named slot to customize the tab label content.

Only card type Tabs support addable & closeable.

## Customized add button icon ^(2.4.0)

## Customized trigger button of new tab

## Default value ^(2.11.9)

| Name                    | Description                                                                                                                             | Type                                                                                             | Default    |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | ---------- |
| model-value / v-model   | binding value, name of the selected tab, the default value is the name of first tab                                                     | ^[string] / ^[number]                                                                            | —          |
| default-value ^(2.11.9) | The value of the tab that should be active when initially rendered. (avoid initial transition)                                          | ^[string] / ^[number]                                                                            | —          |
| type                    | type of Tab                                                                                                                             | ^[enum]`'' \| 'card' \| 'border-card'`                                                           | ''         |
| closable                | whether Tab is closable                                                                                                                 | ^[boolean]                                                                                       | false      |
| addable                 | whether Tab is addable                                                                                                                  | ^[boolean]                                                                                       | false      |
| editable                | whether Tab is addable and closable                                                                                                     | ^[boolean]                                                                                       | false      |
| tab-position            | position of tabs                                                                                                                        | ^[enum]`'top' \| 'right' \| 'bottom' \| 'left'`                                                  | top        |
| stretch                 | whether width of tab automatically fits its container                                                                                   | ^[boolean]                                                                                       | false      |
| before-leave            | hook function before switching tab. If `false` is returned or a `Promise` is returned and then is rejected, switching will be prevented | ^[Function]`(activeName: TabPaneName, oldActiveName: TabPaneName) => Awaitable<void \| boolean>` | () => true |
| tabindex ^(2.11.7)      | tabs tabindex                                                                                                                           | ^[string] / ^[number]                                                                            | 0          |

| Name       | Description                                           | Parameters                                                                           |
| ---------- | ----------------------------------------------------- | ------------------------------------------------------------------------------------ |
| tab-click  | triggers when a tab is clicked                        | ^[Function]`(pane: TabsPaneContext, ev: Event) => void`                              |
| tab-change | triggers when `activeName` is changed                 | ^[Function]`(name: TabPaneName) => void`                                             |
| tab-remove | triggers when tab-remove button is clicked            | ^[Function]`(name: TabPaneName) => void`                                             |
| tab-add    | triggers when tab-add button is clicked               | ^[Function]`() => void`                                                              |
| edit       | triggers when tab-add button or tab-remove is clicked | ^[Function]`(paneName: TabPaneName \| undefined, action: 'remove' \| 'add') => void` |

| Name                           | Description               | Subtags  |
| ------------------------------ | ------------------------- | -------- |
| default                        | customize default content | Tab-pane |
| add-icon ^(2.5.4)              | customize add button icon | —        |
| addIcon ^(2.4.0) ^(deprecated) | customize add button icon | —        |

| Name                | Description                | Type                                        |
| ------------------- | -------------------------- | ------------------------------------------- |
| currentName         | current active pane name   | ^[object]`Ref<TabPaneName>`                 |
| tabNavRef ^(2.9.10) | tab-nav component instance | ^[object]`Ref<TabNavInstance \| undefined>` |

| Name                 | Description                 | Type                                        |
| -------------------- | --------------------------- | ------------------------------------------- |
| scrollToActiveTab    | scroll to the active tab    | ^[Function]`() => Promise<void>`            |
| removeFocus          | remove focus status         | ^[Function]`() => boolean`                  |
| tabListRef ^(2.9.10) | el_tabs\_\_nav html element | ^[object]`Ref<HTMLDivElement \| undefined>` |
| tabBarRef ^(2.9.10)  | el_tabs\_\_nav bar instance | ^[object]`Ref<TabBarInstance \| undefined>` |

### Tab-pane Attributes

| Name     | Description                                                                                                                                                                         | Type                  | Default |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- | ------- |
| label    | title of the tab                                                                                                                                                                    | ^[string]             | ''      |
| disabled | whether Tab is disabled                                                                                                                                                             | ^[boolean]            | false   |
| name     | identifier corresponding to the name of Tabs, representing the alias of the tab-pane, the default is ordinal number of the tab-pane in the sequence, e.g. the first tab-pane is '0' | ^[string] / ^[number] | —       |
| closable | whether Tab is closable                                                                                                                                                             | ^[boolean]            | false   |
| lazy     | whether Tab is lazily rendered                                                                                                                                                      | ^[boolean]            | false   |

| Name    | Description        |
| ------- | ------------------ |
| default | Tab-pane's content |
| label   | Tab-pane's label   |

<details>
  <summary>Show declarations</summary>

#### How to use sortable/draggable tabs ?

We exposed the necessary information to implement it yourself.
You can use a native way to do it, [demo](https://tinyurl.com/2jkyw82j).
Or using [SortableJs](https://github.com/SortableJS/Sortable), [demo](https://tinyurl.com/2r8js24y).

### customized-add-button-icon.vue

### customized-trigger.vue

### default-value.vue

---
Title: Tag
URL: https://element-plus.org/en-US/component/tag
---

**Examples:**

Example 1 (ts):
```ts
type TabBarInstance = InstanceType<typeof TabBar> & {
  /** @description tab root html element */
  ref: barRef
  /** @description method to manually update tab bar style */
  update
}
```

Example 2 (vue):
```vue
<template>
  <el-tabs v-model="activeName" class="demo-tabs" @tab-click="handleClick">
    <el-tab-pane label="User" name="first">User</el-tab-pane>
    <el-tab-pane label="Config" name="second">Config</el-tab-pane>
    <el-tab-pane label="Role" name="third">Role</el-tab-pane>
    <el-tab-pane label="Task" name="fourth">Task</el-tab-pane>
  </el-tabs>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

import type { TabsPaneContext } from 'element-plus'

const activeName = ref('first')

const handleClick = (tab: TabsPaneContext, event: Event) => {
  console.log(tab, event)
}
</script>

<style>
.demo-tabs > .el-tabs__content {
  padding: 32px;
  color: #6b778c;
  font-size: 32px;
  font-weight: 600;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <el-tabs type="border-card">
    <el-tab-pane label="User">User</el-tab-pane>
    <el-tab-pane label="Config">Config</el-tab-pane>
    <el-tab-pane label="Role">Role</el-tab-pane>
    <el-tab-pane label="Task">Task</el-tab-pane>
  </el-tabs>
</template>
```

Example 4 (vue):
```vue
<template>
  <el-tabs
    v-model="activeName"
    type="card"
    class="demo-tabs"
    @tab-click="handleClick"
  >
    <el-tab-pane label="User" name="first">User</el-tab-pane>
    <el-tab-pane label="Config" name="second">Config</el-tab-pane>
    <el-tab-pane label="Role" name="third">Role</el-tab-pane>
    <el-tab-pane label="Task" name="fourth">Task</el-tab-pane>
  </el-tabs>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

import type { TabsPaneContext } from 'element-plus'

const activeName = ref('first')

const handleClick = (tab: TabsPaneContext, event: Event) => {
  console.log(tab, event)
}
</script>

<style>
.demo-tabs > .el-tabs__content {
  padding: 32px;
  color: #6b778c;
  font-size: 32px;
  font-weight: 600;
}
</style>
```

---

## for esm we also need link element-plus for dist

**URL:** llms-txt#for-esm-we-also-need-link-element-plus-for-dist

pnpm link --global element-plus

---

## Transfer

**URL:** llms-txt#transfer

**Contents:**
- Basic usage
- Filterable
- Customizable
- Custom empty content ^(2.9.0)
- Prop aliases
- Transfer API
  - Transfer Attributes
  - Transfer Events
  - Transfer Slots
  - Transfer Exposes

You can search and filter data items.

You can customize list titles, button texts, render function for data items, checking status texts in list footer and list footer contents.

## Custom empty content ^(2.9.0)

You can customize the content when the list is empty or when no filtering results are found.

By default, Transfer looks for `key`, `label` and `disabled` in a data item. If your data items have different key names, you can use the `props` attribute to define aliases.

### Transfer Attributes

| Name                        | Description                                                                                                                                                                                                                                                                        | Type                                                               | Default  |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ | -------- |
| model-value / v-model       | binding value                                                                                                                                                                                                                                                                      | ^[object]`Array<string \| number>`                                 | []       |
| data                        | data source                                                                                                                                                                                                                                                                        | ^[object]`Record<string, any>[]`                                   | []       |
| filterable                  | whether Transfer is filterable                                                                                                                                                                                                                                                     | ^[boolean]                                                         | false    |
| filter-placeholder          | placeholder for the filter input                                                                                                                                                                                                                                                   | ^[string]                                                          | —        |
| filter-method               | custom filter method                                                                                                                                                                                                                                                               | ^[Function]`(query: string, item: Record<string, any>) => boolean` | —        |
| target-order                | order strategy for elements in the target list. If set to `original`, the elements will keep the same order as the data source. If set to `push`, the newly added elements will be pushed to the bottom. If set to `unshift`, the newly added elements will be inserted on the top | ^[enum]`'original' \| 'push' \| 'unshift'`                         | original |
| titles                      | custom list titles                                                                                                                                                                                                                                                                 | ^[object]`[string, string]`                                        | []       |
| button-texts                | custom button texts                                                                                                                                                                                                                                                                | ^[object]`[string, string]`                                        | []       |
| render-content              | custom render function for data items                                                                                                                                                                                                                                              | ^[object]`renderContent`                                           | —        |
| format                      | texts for checking status in list header                                                                                                                                                                                                                                           | ^[object]`TransferFormat`                                          | {}       |
| [props](#type-declarations) | prop aliases for data source                                                                                                                                                                                                                                                       | ^[object]`TransferPropsAlias`                                      | —        |
| left-default-checked        | key array of initially checked data items of the left list                                                                                                                                                                                                                         | ^[object]`Array<string \| number>`                                 | []       |
| right-default-checked       | key array of initially checked data items of the right list                                                                                                                                                                                                                        | ^[object]`Array<string \| number>`                                 | []       |
| validate-event              | whether to trigger form validation                                                                                                                                                                                                                                                 | ^[boolean]                                                         | true     |

| Name               | Description                                                                         | Type                                                                                                |
| ------------------ | ----------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| change             | triggers when data items change in the right list                                   | ^[Function]`(value: TransferKey[], direction: TransferDirection, movedKeys: TransferKey[]) => void` |
| left-check-change  | triggers when end user changes the checked state of any data item in the left list  | ^[Function]`(value: TransferKey[], movedKeys?: TransferKey[]) => void`                              |
| right-check-change | triggers when end user changes the checked state of any data item in the right list | ^[Function]`(value: TransferKey[], movedKeys?: TransferKey[]) => void`                              |

| Name                 | Description                                                          | Type                                    |
| -------------------- | -------------------------------------------------------------------- | --------------------------------------- |
| default              | Custom content for data items.                                       | ^[object]`{ option: TransferDataItem }` |
| left-footer          | content of left list footer                                          | —                                       |
| right-footer         | content of right list footer                                         | —                                       |
| left-empty ^(2.9.0)  | content when left panel is empty or when no data matches the filter  | —                                       |
| right-empty ^(2.9.0) | content when right panel is empty or when no data matches the filter | —                                       |

| Name       | Description                                 | Type                                            |
| ---------- | ------------------------------------------- | ----------------------------------------------- |
| clearQuery | clear the filter keyword of a certain panel | ^[Function]`(which: TransferDirection) => void` |
| leftPanel  | left panel ref                              | ^[object]`Ref<TransferPanelInstance>`           |
| rightPanel | right panel ref                             | ^[object]`Ref<TransferPanelInstance>`           |

## Transfer Panel API

### Transfer Panel Exposes

| Name  | Description    | Type      |
| ----- | -------------- | --------- |
| query | filter keyword | ^[string] |

<details>
  <summary>Show declarations</summary>

### empty-content.vue

---
Title: TreeSelect
URL: https://element-plus.org/en-US/component/tree-select
---

**Examples:**

Example 1 (ts):
```ts
import type { h as H, VNode } from 'vue'

type TransferKey = string | number

type TransferDirection = 'left' | 'right'

type TransferDataItem = Record<string, any>

type renderContent = (h: typeof H, option: TransferDataItem) => VNode | VNode[]

interface TransferFormat {
  noChecked?: string
  hasChecked?: string
}

interface TransferPropsAlias {
  label?: string
  key?: string
  disabled?: string
}
```

Example 2 (vue):
```vue
<template>
  <el-transfer v-model="value" :data="data" />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

interface Option {
  key: number
  label: string
  disabled: boolean
}

const generateData = () => {
  const data: Option[] = []
  for (let i = 1; i <= 15; i++) {
    data.push({
      key: i,
      label: `Option ${i}`,
      disabled: i % 4 === 0,
    })
  }
  return data
}

const data = ref<Option[]>(generateData())
const value = ref([])
</script>
```

Example 3 (vue):
```vue
<template>
  <p style="text-align: center; margin: 0 0 20px">
    Customize data items using render-content
  </p>
  <div style="text-align: center">
    <el-transfer
      v-model="leftValue"
      style="text-align: left; display: inline-block"
      filterable
      :left-default-checked="[2, 3]"
      :right-default-checked="[1]"
      :render-content="renderFunc"
      :titles="['Source', 'Target']"
      :button-texts="['To left', 'To right']"
      :format="{
        noChecked: '${total}',
        hasChecked: '${checked}/${total}',
      }"
      :data="data"
      @change="handleChange"
    >
      <template #left-footer>
        <el-button class="transfer-footer" size="small">Operation</el-button>
      </template>
      <template #right-footer>
        <el-button class="transfer-footer" size="small">Operation</el-button>
      </template>
    </el-transfer>
    <p style="text-align: center; margin: 50px 0 20px">
      Customize data items using scoped slot
    </p>
    <div style="text-align: center">
      <el-transfer
        v-model="rightValue"
        style="text-align: left; display: inline-block"
        filterable
        :left-default-checked="[2, 3]"
        :right-default-checked="[1]"
        :titles="['Source', 'Target']"
        :button-texts="['To left', 'To right']"
        :format="{
          noChecked: '${total}',
          hasChecked: '${checked}/${total}',
        }"
        :data="data"
        @change="handleChange"
      >
        <template #default="{ option }">
          <span>{{ option.key }} - {{ option.label }}</span>
        </template>
        <template #left-footer>
          <el-button class="transfer-footer" size="small">Operation</el-button>
        </template>
        <template #right-footer>
          <el-button class="transfer-footer" size="small">Operation</el-button>
        </template>
      </el-transfer>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

import type {
  TransferDirection,
  TransferKey,
  renderContent,
} from 'element-plus'

interface Option {
  key: number
  label: string
  disabled: boolean
}

const generateData = (): Option[] => {
  const data: Option[] = []
  for (let i = 1; i <= 15; i++) {
    data.push({
      key: i,
      label: `Option ${i}`,
      disabled: i % 4 === 0,
    })
  }
  return data
}

const data = ref(generateData())
const rightValue = ref([1])
const leftValue = ref([1])

const renderFunc: renderContent = (h, option) => h('span', null, option.label)

const handleChange = (
  value: TransferKey[],
  direction: TransferDirection,
  movedKeys: TransferKey[]
) => {
  console.log(value, direction, movedKeys)
}
</script>

<style>
.transfer-footer {
  margin-left: 15px;
  padding: 6px 5px;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <el-transfer v-model="value" :data="data">
    <template #left-empty>
      <el-empty :image-size="60" description="No data" />
    </template>
    <template #right-empty>
      <el-empty :image-size="60" description="No data" />
    </template>
  </el-transfer>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

interface DataItem {
  key: number
  label: string
  disabled: boolean
}
const generateData = (): DataItem[] => {
  const data: DataItem[] = []
  for (let i = 1; i <= 15; i++) {
    data.push({
      key: i,
      label: `Option ${i}`,
      disabled: i % 4 === 0,
    })
  }
  return data
}

const data = ref(generateData())
const value = ref([])
</script>
```

---

## Provide links or keys to any relevant tickets, articles or other resources

**URL:** llms-txt#provide-links-or-keys-to-any-relevant-tickets,-articles-or-other-resources

---

## ColorPickerPanel ^(beta)

**URL:** llms-txt#colorpickerpanel-^(beta)

**Contents:**
- Basic usage
- Alpha
- Predefined colors
- Border
- Disabled
- API
  - Attributes
  - Exposes
- Vue Examples
  - alpha.vue

`ColorPickerPanel` is the core component of `ColorPicker`.

By default the color-picker-panel is bordered but in some case you don't want it.

The `disabled` attribute determines if the color picker is fully disabled.

| Name                     | Description                                | Type                                                                                                             | Default |
| ------------------------ | ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------- | ------- |
| model-value / v-model    | binding value                              | ^[string]                                                                                                        | —       |
| border                   | whether the color picker panel is bordered | ^[boolean]                                                                                                       | true    |
| disabled                 | whether to disable the color picker        | ^[boolean]                                                                                                       | false   |
| show-alpha               | whether to display the alpha slider        | ^[boolean]                                                                                                       | false   |
| color-format             | color format of v-model                    | ^[enum]`'hsl' \| 'hsv' \| 'hex' \| 'rgb' \| 'hex' (when show-alpha is false) \| 'rgb' (when show-alpha is true)` | —       |
| predefine                | predefined color options                   | ^[object]`string[]`                                                                                              | —       |
| validate-event ^(2.11.7) | whether to trigger form validation         | ^[boolean]                                                                                                       | true    |

| Name             | Description           | Type                     |
| ---------------- | --------------------- | ------------------------ |
| color            | current color object  | ^[object]`Color`         |
| inputRef         | custom input ref      | ^[object]`InputInstance` |
| update ^(2.11.4) | update sub components | ^[Function]`() => void`  |

### predefined-color.vue

---
Title: ColorPicker
URL: https://element-plus.org/en-US/component/color-picker
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-color-picker-panel v-model="color" show-alpha />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const color = ref('rgba(19, 206, 102, 0.8)')
</script>
```

Example 2 (vue):
```vue
<template>
  <el-color-picker-panel v-model="color" />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const color = ref('#409EFF')
</script>
```

Example 3 (vue):
```vue
<template>
  <div ref="containerRef">
    <div class="text-center">No border:</div>
    <el-divider />
    <div class="flex flex-wrap justify-center gap-4">
      <div class="p-5">
        <el-color-picker-panel v-model="value" :border="false" />
      </div>
      <el-divider
        class="h-auto"
        :direction="isNarrow ? 'horizontal' : 'vertical'"
      />
      <el-card>
        <el-color-picker-panel v-model="value" :border="false" />
      </el-card>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { computed, ref } from 'vue'
import { useElementSize } from '@vueuse/core'

const value = ref('#ff6900')
const containerRef = ref<HTMLElement>()

const { width } = useElementSize(containerRef)

const isNarrow = computed(() => width.value < 815)
</script>
```

Example 4 (vue):
```vue
<template>
  <el-color-picker-panel
    v-model="color"
    disabled
    show-alpha
    :predefine="predefineColors"
  />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const color = ref('#ff6900')
const predefineColors = [
  '#ff4500',
  '#ff8c00',
  '#ffd700',
  '#90ee90',
  '#00ced1',
  '#1e90ff',
  '#c71585',
]
</script>
```

---

## Design Disciplines

**URL:** llms-txt#design-disciplines

**Contents:**
- Consistency
- Feedback
- Efficiency
- Controllability

- **Consistent with real life:** in line with the process and logic of real life,
  and comply with languages and habits that the users are used to.

- **Consistent within interface:** all elements should be consistent, such as:
  design style, icons and texts, position of elements, etc.

- **Operation feedback:** enable the users to clearly perceive their operations
  by style updates and interactive effects.

- **Visual feedback:** reflect current state by updating or
  rearranging elements of the page.

- **Simplify the process:** keep operating process simple and intuitive.

- **Definite and clear:** enunciate your intentions clearly so
  that the users can quickly understand and make decisions.

- **Easy to identify:** the interface should be straightforward,
  which helps the users to identify and frees them from memorizing and recalling.

- **Decision making:** giving advice about operations is acceptable, but do not
  make decisions for the users.

- **Controlled consequences:** users should be granted the freedom to operate,
  including canceling, aborting or terminating current operation.

---
Title: Development FAQ
URL: https://element-plus.org/en-US/guide/dev-faq
---

---

## Rate

**URL:** llms-txt#rate

**Contents:**
- Basic usage
- Sizes
- With allow-half
- With text
- Clearable
- More icons
- Read-only
- Custom styles
  - Default Variables
- API

Using text to indicate rating score

You can use different icons to distinguish different rate components.

Read-only Rate is for displaying rating score. Half star is supported.

Now you can set custom style for rate component.
Use `css/scss` language to change the global or local color. We set some global color variables: `--el-rate-void-color`, `--el-rate-fill-color`, `--el-rate-disabled-void-color`, `--el-rate-text-color`. You can use like: `:root { --el-rate-void-color: red; --el-rate-fill-color: blue; }`.

### Default Variables

| Variable                      | Default Color                 |
| ----------------------------- | ----------------------------- |
| --el-rate-void-color          | var(--el-border-color-darker) |
| --el-rate-fill-color          | #f7ba2a                       |
| --el-rate-disabled-void-color | var(--el-fill-color)          |
| --el-rate-text-color          | var(--el-text-color-primary)  |

| Name                        | Description                                                                                                                                                                                                                    | Type                                                                      | Default                                                            |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| model-value / v-model       | binding value                                                                                                                                                                                                                  | ^[number]                                                                 | 0                                                                  |
| max                         | max rating score                                                                                                                                                                                                               | ^[number]                                                                 | 5                                                                  |
| size                        | size of Rate                                                                                                                                                                                                                   | ^[enum]`'large' \| 'default' \| 'small'`                                  | —                                                                  |
| disabled                    | whether Rate is read-only                                                                                                                                                                                                      | ^[boolean]                                                                | false                                                              |
| allow-half                  | whether picking half start is allowed                                                                                                                                                                                          | ^[boolean]                                                                | false                                                              |
| low-threshold               | threshold value between low and medium level. The value itself will be included in low level                                                                                                                                   | ^[number]                                                                 | 2                                                                  |
| high-threshold              | threshold value between medium and high level. The value itself will be included in high level                                                                                                                                 | ^[number]                                                                 | 4                                                                  |
| colors                      | colors for icons. If array, it should have 3 elements, each of which corresponds with a score level, else if object, the key should be threshold value between two levels, and the value should be corresponding color         | ^[object]`string[] \| Record<number, string>`                             | ['#f7ba2a', '#f7ba2a', '#f7ba2a']                                  |
| void-color                  | color of unselected icons                                                                                                                                                                                                      | ^[string]                                                                 | #c6d1de                                                            |
| disabled-void-color         | color of unselected read-only icons                                                                                                                                                                                            | ^[string]                                                                 | #eff2f7                                                            |
| icons                       | icon components. If array, it should have 3 elements, each of which corresponds with a score level, else if object, the key should be threshold value between two levels, and the value should be corresponding icon component | ^[object]`string[] \| Component[] \| Record<number, string \| Component>` | [StarFilled, StarFilled, StarFilled]                               |
| void-icon                   | component of unselected icons                                                                                                                                                                                                  | ^[string] / ^[Component]                                                  | Star                                                               |
| disabled-void-icon          | component of unselected read-only icons                                                                                                                                                                                        | ^[string] / ^[Component]                                                  | StarFilled                                                         |
| show-text                   | whether to display texts                                                                                                                                                                                                       | ^[boolean]                                                                | false                                                              |
| show-score                  | whether to display current score. show-score and show-text cannot be true at the same time                                                                                                                                     | ^[boolean]                                                                | false                                                              |
| text-color                  | color of texts                                                                                                                                                                                                                 | ^[string]                                                                 | ''                                                                 |
| texts                       | text array                                                                                                                                                                                                                     | ^[array]`string[]`                                                        | ['Extremely bad', 'Disappointed', 'Fair', 'Satisfied', 'Surprise'] |
| score-template              | score template                                                                                                                                                                                                                 | ^[string]                                                                 | {value}                                                            |
| clearable ^(2.2.18)         | whether value can be reset to `0`                                                                                                                                                                                              | ^[boolean]                                                                | false                                                              |
| id                          | native `id` attribute                                                                                                                                                                                                          | ^[string]                                                                 | —                                                                  |
| aria-label ^(a11y) ^(2.7.2) | same as `aria-label` in Rate                                                                                                                                                                                                   | ^[string]                                                                 | —                                                                  |
| label ^(a11y) ^(deprecated) | same as `aria-label` in Rate                                                                                                                                                                                                   | ^[string]                                                                 | —                                                                  |

| Name   | Description                         | Type                                 |
| ------ | ----------------------------------- | ------------------------------------ |
| change | Triggers when rate value is changed | ^[Function]`(value: number) => void` |

| Name              | Description         | Type                                 |
| ----------------- | ------------------- | ------------------------------------ |
| setCurrentValue   | set current value   | ^[Function]`(value: number) => void` |
| resetCurrentValue | reset current value | ^[Function]`() => void`              |

---
Title: Result
URL: https://element-plus.org/en-US/component/result
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-rate v-model="value" allow-half />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref()
</script>
```

Example 2 (vue):
```vue
<template>
  <div class="demo-rate-block">
    <span class="demonstration">Default</span>
    <el-rate v-model="value1" />
  </div>
  <div class="demo-rate-block">
    <span class="demonstration">Color for different levels</span>
    <el-rate v-model="value2" :colors="colors" />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value1 = ref(0)
const value2 = ref(0)
const colors = ref(['#99A9BF', '#F7BA2A', '#FF9900']) // same as { 2: '#99A9BF', 4: { value: '#F7BA2A', excluded: true }, 5: '#FF9900' }
</script>

<style scoped>
.demo-rate-block {
  padding: 30px 0;
  text-align: center;
  border-right: solid 1px var(--el-border-color);
  display: inline-block;
  width: 49%;
  box-sizing: border-box;
}
.demo-rate-block:last-child {
  border-right: none;
}
.demo-rate-block .demonstration {
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
  <el-rate v-model="value" clearable />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref(3)
</script>
```

Example 4 (vue):
```vue
<template>
  <el-rate
    v-model="value"
    :icons="icons"
    :void-icon="ChatRound"
    :colors="['#409eff', '#67c23a', '#FF9900']"
  />
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { ChatDotRound, ChatLineRound, ChatRound } from '@element-plus/icons-vue'

const value = ref()
const icons = [ChatRound, ChatLineRound, ChatDotRound] // same as { 2: ChatRound, 4: { value: ChatLineRound, excluded: true }, 5: ChatDotRound }
</script>
```

---

## Border

**URL:** llms-txt#border

**Contents:**
- Border style
- Radius
- Shadow
- Vue Examples
  - border.vue
  - radius.vue
  - shadow.vue

We standardize the borders that can be used in buttons, cards, pop-ups and other components.

There are few border styles to choose.

There are few radius styles to choose.

There are few shadow styles to choose.

---
Title: Breadcrumb
URL: https://element-plus.org/en-US/component/breadcrumb
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <table class="demo-border">
    <tbody>
      <tr>
        <td class="text">Name</td>
        <td class="text">Thickness</td>
        <td class="line">Demo</td>
      </tr>
      <tr>
        <td class="text">Solid</td>
        <td class="text">1px</td>
        <td class="line">
          <div />
        </td>
      </tr>
      <tr>
        <td class="text">Dashed</td>
        <td class="text">2px</td>
        <td class="line">
          <div class="dashed" />
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped>
.demo-border .text {
  width: 15%;
}
.demo-border .line {
  width: 70%;
}
.demo-border .line div {
  width: 100%;
  height: 0;
  border-top: 1px solid var(--el-border-color);
}
.demo-border .line .dashed {
  border-top: 2px dashed var(--el-border-color);
}
</style>
```

Example 2 (vue):
```vue
<template>
  <el-row :gutter="12" class="demo-radius">
    <el-col
      v-for="(radius, i) in radiusGroup"
      :key="i"
      :span="6"
      :xs="{ span: 12 }"
    >
      <div class="title">{{ radius.name }}</div>
      <div class="value">
        <code>
          border-radius:
          {{
            radius.type
              ? useCssVar(`--el-border-radius-${radius.type}`)
              : '"0px"'
          }}
        </code>
      </div>
      <div
        class="radius"
        :style="{
          borderRadius: radius.type
            ? `var(--el-border-radius-${radius.type})`
            : '',
        }"
      />
    </el-col>
  </el-row>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { useCssVar } from '@vueuse/core'

const radiusGroup = ref([
  {
    name: 'No Radius',
    type: '',
  },
  {
    name: 'Small Radius',
    type: 'small',
  },
  {
    name: 'Large Radius',
    type: 'base',
  },
  {
    name: 'Round Radius',
    type: 'round',
  },
])
</script>

<style scoped>
.demo-radius .title {
  color: var(--el-text-color-regular);
  font-size: 18px;
  margin: 10px 0;
}
.demo-radius .value {
  color: var(--el-text-color-primary);
  font-size: 16px;
  margin: 10px 0;
}
.demo-radius .radius {
  height: 40px;
  width: 70%;
  border: 1px solid var(--el-border-color);
  border-radius: 0;
  margin-top: 20px;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <div class="flex justify-between items-center flex-wrap">
    <div
      v-for="(shadow, i) in shadowGroup"
      :key="i"
      class="flex flex-col justify-center items-center"
      m="auto"
      w="46"
    >
      <div
        class="inline-flex"
        h="30"
        w="30"
        m="2"
        :style="{
          boxShadow: `var(${getCssVarName(shadow.type)})`,
        }"
      />
      <span p="y-4" class="demo-shadow-text" text="sm">
        {{ shadow.name }}
      </span>
      <code text="xs">
        {{ getCssVarName(shadow.type) }}
      </code>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const shadowGroup = ref([
  {
    name: 'Basic Shadow',
    type: '',
  },
  {
    name: 'Light Shadow',
    type: 'light',
  },
  {
    name: 'Lighter Shadow',
    type: 'lighter',
  },
  {
    name: 'Dark Shadow',
    type: 'dark',
  },
])

const getCssVarName = (type: string) => {
  return `--el-box-shadow${type ? '-' : ''}${type}`
}
</script>
```

---

## Tree

**URL:** llms-txt#tree

**Contents:**
- Basic usage
- Selectable
- Custom leaf node in lazy mode
- Lazy loading multiple times ^(2.6.3)
- Disabled checkbox
- Default expanded and default checked
- Checking tree nodes
- Custom node content
- Custom node class
- Tree node filtering

Display a set of data with hierarchies.

Basic tree structure.

Used for node selection.

## Custom leaf node in lazy mode

## Lazy loading multiple times ^(2.6.3)

The checkbox of a node can be set as disabled.

## Default expanded and default checked

Tree nodes can be initially expanded or checked

## Checking tree nodes

## Custom node content

The content of tree nodes can be customized, so you can add icons or buttons as you will

The class of tree nodes can be customized

## Tree node filtering

Tree nodes can be filtered

Only one node among the same level can be expanded at one time.

You can drag and drop Tree nodes by adding a `draggable` attribute.

| Name                         | Description                                                                                                                                                                                                                                                                                                                                                                 | Type                                                   | Default |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ | ------- |
| data                         | tree data                                                                                                                                                                                                                                                                                                                                                                   | ^[object]`Array<{[key: string]: any}>`                 | —       |
| empty-text                   | text displayed when data is void                                                                                                                                                                                                                                                                                                                                            | ^[string]                                              | —       |
| node-key                     | unique identity key name for nodes, its value should be unique across the whole tree                                                                                                                                                                                                                                                                                        | ^[string]                                              | —       |
| [props](#props)              | configuration options, see the following table                                                                                                                                                                                                                                                                                                                              | ^[object]                                              | —       |
| render-after-expand          | whether to render child nodes only after a parent node is expanded for the first time                                                                                                                                                                                                                                                                                       | ^[boolean]                                             | true    |
| load                         | method for loading subtree data, only works when `lazy` is true                                                                                                                                                                                                                                                                                                             | ^[Function]`(node, resolve, reject) => void`           | —       |
| render-content               | render function for tree node                                                                                                                                                                                                                                                                                                                                               | ^[Function]`(h, { node, data, store }) => void`        | —       |
| highlight-current            | whether current node is highlighted                                                                                                                                                                                                                                                                                                                                         | ^[boolean]                                             | false   |
| default-expand-all           | whether to expand all nodes by default                                                                                                                                                                                                                                                                                                                                      | ^[boolean]                                             | false   |
| expand-on-click-node         | whether to expand or collapse node when clicking on the node, if false, then expand or collapse node only when clicking on the arrow icon.                                                                                                                                                                                                                                  | ^[boolean]                                             | true    |
| check-on-click-node          | whether to check or uncheck node when clicking on the node, if false, the node can only be checked or unchecked by clicking on the checkbox.                                                                                                                                                                                                                                | ^[boolean]                                             | false   |
| check-on-click-leaf ^(2.9.6) | whether to check or uncheck node when clicking on leaf node (last children).                                                                                                                                                                                                                                                                                                | ^[boolean]                                             | true    |
| auto-expand-parent           | whether to expand father node when a child node is expanded                                                                                                                                                                                                                                                                                                                 | ^[boolean]                                             | true    |
| default-expanded-keys        | array of keys of initially expanded nodes                                                                                                                                                                                                                                                                                                                                   | ^[object]`Array<string \| number>`                     | —       |
| show-checkbox                | whether node is selectable                                                                                                                                                                                                                                                                                                                                                  | ^[boolean]                                             | false   |
| check-strictly               | whether checked state of a node not affects its father and child nodes when `show-checkbox` is `true`                                                                                                                                                                                                                                                                       | ^[boolean]                                             | false   |
| default-checked-keys         | array of keys of initially checked nodes                                                                                                                                                                                                                                                                                                                                    | ^[object]`Array<string \| number>`                     | —       |
| current-node-key             | key of initially selected node                                                                                                                                                                                                                                                                                                                                              | ^[string] / ^[number]                                  | —       |
| filter-node-method           | this function will be executed on each node when use filter method. if return `false`, tree node will be hidden.                                                                                                                                                                                                                                                            | ^[Function]`(value, data, node) => boolean`            | —       |
| accordion                    | whether only one node among the same level can be expanded at one time                                                                                                                                                                                                                                                                                                      | ^[boolean]                                             | false   |
| indent                       | horizontal indentation of nodes in adjacent levels in pixels                                                                                                                                                                                                                                                                                                                | ^[number]                                              | 18      |
| icon                         | custom tree node icon component                                                                                                                                                                                                                                                                                                                                             | ^[string] / ^[Component]                               | —       |
| lazy                         | whether to lazy load leaf node, used with `load` attribute                                                                                                                                                                                                                                                                                                                  | ^[boolean]                                             | false   |
| draggable                    | whether enable tree nodes drag and drop                                                                                                                                                                                                                                                                                                                                     | ^[boolean]                                             | false   |
| allow-drag                   | this function will be executed before dragging a node. If `false` is returned, the node can not be dragged                                                                                                                                                                                                                                                                  | ^[Function]`(node) => boolean`                         | —       |
| allow-drop                   | this function will be executed before the dragging node is dropped. If `false` is returned, the dragging node can not be dropped at the target node. `type` has three possible values: 'prev' (inserting the dragging node before the target node), 'inner' (inserting the dragging node to the target node) and 'next' (inserting the dragging node after the target node) | ^[Function]`(draggingNode, dropNode, type) => boolean` | —       |

| Attribute | Description                                                                   | Type                                             | Default |
| --------- | ----------------------------------------------------------------------------- | ------------------------------------------------ | ------- |
| label     | specify which key of node object is used as the node's label                  | ^[string] / ^[Function]`(data, node) => string`  | —       |
| children  | specify which node object is used as the node's subtree                       | ^[string]                                        | —       |
| disabled  | specify which key of node object represents if node's checkbox is disabled    | ^[string] / ^[Function]`(data, node) => boolean` | —       |
| isLeaf    | specify whether the node is a leaf node, only works when lazy load is enabled | ^[string] / ^[Function]`(data, node) => boolean` | —       |
| class     | custom node class name                                                        | ^[string] / ^[Function]`(data, node) => string`  | —       |

`Tree` has the following method, which returns the currently selected array of nodes.

| Method              | Description                                                                                                          | Parameters                                                                                                                                                                                                                                                                                  |
| ------------------- | -------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter              | filter all tree nodes, filtered nodes will be hidden                                                                 | Accept a parameter which will be used as first parameter for filter-node-method                                                                                                                                                                                                             |
| updateKeyChildren   | set new data to node, only works when `node-key` is assigned                                                         | (key, data) Accept two parameters: 1. key of node 2. new data                                                                                                                                                                                                                               |
| getCheckedNodes     | If the node can be selected (`show-checkbox` is `true`), it returns the currently selected array of nodes            | (leafOnly, includeHalfChecked) Accept two boolean type parameters: 1. default value is `false`. If the parameter is `true`, it only returns the currently selected array of sub-nodes. 2. default value is `false`. If the parameter is `true`, the return value contains halfchecked nodes |
| setCheckedNodes     | set certain nodes to be checked, only works when `node-key` is assigned                                              | (nodes, leafOnly) Accept two parameters: 1. an array of node objects to be checked 2. a boolean parameter. If set to `true`, only the checked status of leaf nodes will be set. The default value is `false`.                                                                               |
| getCheckedKeys      | If the node can be selected (`show-checkbox` is `true`), it returns the currently selected array of node's keys      | (leafOnly) Accept a boolean type parameter whose default value is `false`. If the parameter is `true`, it only returns the currently selected array of sub-nodes.                                                                                                                           |
| setCheckedKeys      | set certain nodes to be checked, only works when `node-key` is assigned                                              | (keys, leafOnly) Accept two parameters: 1. an array of node's keys to be checked 2. a boolean parameter. If set to `true`, only the checked status of leaf nodes will be set. The default value is `false`.                                                                                 |
| setChecked          | set node to be checked or not, only works when `node-key` is assigned                                                | (key/data, checked, deep) Accept three parameters: 1. node's key or data to be checked 2. a boolean typed parameter indicating checked or not. 3. a boolean typed parameter indicating deep or not (note that `check-strictly` must be `false`).                                            |
| getHalfCheckedNodes | If the node can be selected (`show-checkbox` is `true`), it returns the currently half selected array of nodes       | —                                                                                                                                                                                                                                                                                           |
| getHalfCheckedKeys  | If the node can be selected (`show-checkbox` is `true`), it returns the currently half selected array of node's keys | —                                                                                                                                                                                                                                                                                           |
| getCurrentKey       | return the highlight node's key (null if no node is highlighted)                                                     | —                                                                                                                                                                                                                                                                                           |
| getCurrentNode      | return the highlight node's data (null if no node is highlighted)                                                    | —                                                                                                                                                                                                                                                                                           |
| setCurrentKey       | set highlighted node by key, only works when `node-key` is assigned                                                  | (key, shouldAutoExpandParent=true) 1. the node's key to be highlighted. If `null`, cancel the currently highlighted node 2. whether to automatically expand parent node                                                                                                                     |
| setCurrentNode      | set highlighted node, only works when `node-key` is assigned                                                         | (node, shouldAutoExpandParent=true) 1. the node to be highlighted 2. whether to automatically expand parent node                                                                                                                                                                            |
| getNode             | get node by data or key                                                                                              | (data) the node's data or key                                                                                                                                                                                                                                                               |
| remove              | remove a node, only works when node-key is assigned                                                                  | (data) the node's data or node to be deleted                                                                                                                                                                                                                                                |
| append              | append a child node to a given node in the tree                                                                      | (data, parentNode) 1. child node's data to be appended 2. parent node's data, key or node                                                                                                                                                                                                   |
| insertBefore        | insert a node before a given node in the tree                                                                        | (data, refNode) 1. node's data to be inserted 2. reference node's data, key or node                                                                                                                                                                                                         |
| insertAfter         | insert a node after a given node in the tree                                                                         | (data, refNode) 1. node's data to be inserted 2. reference node's data, key or node                                                                                                                                                                                                         |

| Name             | Description                                               | Parameters                                                                                                                                                                                       |
| ---------------- | --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| node-click       | triggers when a node is clicked                           | four parameters: node object corresponding to the node clicked, `node` property of TreeNode, TreeNode itself, event object                                                                       |
| node-contextmenu | triggers when a node is clicked by right button           | four parameters: event, node object corresponding to the node clicked, `node` property of TreeNode, TreeNode itself                                                                              |
| check-change     | triggers when the selected state of the node changes      | three parameters: node object corresponding to the node whose selected state is changed, whether the node is selected, whether node's subtree has selected nodes                                 |
| check            | triggers after clicking the checkbox of a node            | two parameters: node object corresponding to the node that is checked / unchecked, tree checked status object which has four props: checkedNodes, checkedKeys, halfCheckedNodes, halfCheckedKeys |
| current-change   | triggers when current node changes                        | two parameters: node object corresponding to the current node, `node` property of TreeNode                                                                                                       |
| node-expand      | triggers when current node open                           | three parameters: node object corresponding to the node opened, `node` property of TreeNode, TreeNode itself                                                                                     |
| node-collapse    | triggers when current node close                          | three parameters: node object corresponding to the node closed, `node` property of TreeNode, TreeNode itself                                                                                     |
| node-drag-start  | triggers when dragging starts                             | two parameters: node object corresponding to the dragging node, event.                                                                                                                           |
| node-drag-enter  | triggers when the dragging node enters another node       | three parameters: node object corresponding to the dragging node, node object corresponding to the entering node, event.                                                                         |
| node-drag-leave  | triggers when the dragging node leaves a node             | three parameters: node object corresponding to the dragging node, node object corresponding to the leaving node, event.                                                                          |
| node-drag-over   | triggers when dragging over a node (like mouseover event) | three parameters: node object corresponding to the dragging node, node object corresponding to the dragging over node, event.                                                                    |
| node-drag-end    | triggers when dragging ends                               | four parameters: node object corresponding to the dragging node, node object corresponding to the dragging end node (may be `undefined`), node drop type (before / after / inner), event.        |
| node-drop        | triggers after the dragging node is dropped               | four parameters: node object corresponding to the dragging node, node object corresponding to the dropped node, node drop type (before / after / inner), event.                                  |

| Name           | Description                       | Type                                                                                |
| -------------- | --------------------------------- | ----------------------------------------------------------------------------------- |
| default        | custom content for tree nodes     | ^[object]`{ node: UnwrapRef<RootTreeType['root']>, data: Tree \| TreeOptionProps }` |
| empty ^(2.3.4) | custom content when data is empty | —                                                                                   |

<details>
  <summary>Show declarations</summary>

### checking-tree.vue

### custom-node-class.vue

### customized-node.vue

### default-state.vue

### multiple-times-load.vue

---
Title: Typography
URL: https://element-plus.org/en-US/component/typography
---

**Examples:**

Example 1 (ts):
```ts
interface RootTreeType {
  root: Ref<Node>
  // ...
}

// UnwrapRef<RootTreeType['root']> => Node
type Node = {
  canFocus: boolean
  checked: boolean
  childNodes: Node[]
  data: TreeNodeData
  expanded: boolean
  id: number
  indeterminate: boolean
  isCurrent: boolean
  isEffectivelyChecked: boolean
  isLeaf?: boolean
  isLeafByUser?: boolean
  level: number
  loaded: boolean
  loading: boolean
  parent: Node | null
  store: TreeStore
  text: string | null
  visible: boolean
}

// TreeNodeData => Tree / TreeOptionProps
// Tree type is your prop type.
// TreeOptionProps is default prop type
interface TreeOptionProps {
  children?: string
  label?: string | ((data: TreeNodeData, node: Node) => string)
  disabled?: string | ((data: TreeNodeData, node: Node) => boolean)
  isLeaf?: string | ((data: TreeNodeData, node: Node) => boolean)
  class?: (
    data: TreeNodeData,
    node: Node
  ) => string | { [key: string]: boolean }
}
```

Example 2 (vue):
```vue
<template>
  <el-tree
    style="max-width: 600px"
    :data="data"
    :props="defaultProps"
    accordion
    @node-click="handleNodeClick"
  />
</template>

<script lang="ts" setup>
interface Tree {
  label: string
  children?: Tree[]
}

const handleNodeClick = (data: Tree) => {
  console.log(data)
}

const data: Tree[] = [
  {
    label: 'Level one 1',
    children: [
      {
        label: 'Level two 1-1',
        children: [
          {
            label: 'Level three 1-1-1',
          },
        ],
      },
    ],
  },
  {
    label: 'Level one 2',
    children: [
      {
        label: 'Level two 2-1',
        children: [
          {
            label: 'Level three 2-1-1',
          },
        ],
      },
      {
        label: 'Level two 2-2',
        children: [
          {
            label: 'Level three 2-2-1',
          },
        ],
      },
    ],
  },
  {
    label: 'Level one 3',
    children: [
      {
        label: 'Level two 3-1',
        children: [
          {
            label: 'Level three 3-1-1',
          },
        ],
      },
      {
        label: 'Level two 3-2',
        children: [
          {
            label: 'Level three 3-2-1',
          },
        ],
      },
    ],
  },
]
const defaultProps = {
  children: 'children',
  label: 'label',
}
</script>
```

Example 3 (vue):
```vue
<template>
  <el-tree
    style="max-width: 600px"
    :data="data"
    :props="defaultProps"
    @node-click="handleNodeClick"
  />
</template>

<script lang="ts" setup>
interface Tree {
  label: string
  children?: Tree[]
}

const handleNodeClick = (data: Tree) => {
  console.log(data)
}

const data: Tree[] = [
  {
    label: 'Level one 1',
    children: [
      {
        label: 'Level two 1-1',
        children: [
          {
            label: 'Level three 1-1-1',
          },
        ],
      },
    ],
  },
  {
    label: 'Level one 2',
    children: [
      {
        label: 'Level two 2-1',
        children: [
          {
            label: 'Level three 2-1-1',
          },
        ],
      },
      {
        label: 'Level two 2-2',
        children: [
          {
            label: 'Level three 2-2-1',
          },
        ],
      },
    ],
  },
  {
    label: 'Level one 3',
    children: [
      {
        label: 'Level two 3-1',
        children: [
          {
            label: 'Level three 3-1-1',
          },
        ],
      },
      {
        label: 'Level two 3-2',
        children: [
          {
            label: 'Level three 3-2-1',
          },
        ],
      },
    ],
  },
]

const defaultProps = {
  children: 'children',
  label: 'label',
}
</script>
```

Example 4 (vue):
```vue
<template>
  <el-tree
    ref="treeRef"
    style="max-width: 600px"
    :data="data"
    show-checkbox
    default-expand-all
    node-key="id"
    highlight-current
    :props="defaultProps"
  />

  <div class="flex flex-wrap gap-1 mt-2">
    <el-button class="!ml-0" @click="getCheckedNodes">get by node</el-button>
    <el-button class="!ml-0" @click="getCheckedKeys">get by key</el-button>
    <el-button class="!ml-0" @click="setCheckedNodes">set by node</el-button>
    <el-button class="!ml-0" @click="setCheckedKeys">set by key</el-button>
    <el-button class="!ml-0" @click="resetChecked">reset</el-button>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

import type { RenderContentContext, TreeInstance } from 'element-plus'

interface Tree {
  id: number
  label: string
  children?: Tree[]
}
type Node = RenderContentContext['node']

const treeRef = ref<TreeInstance>()

const getCheckedNodes = () => {
  console.log(treeRef.value!.getCheckedNodes(false, false))
}
const getCheckedKeys = () => {
  console.log(treeRef.value!.getCheckedKeys(false))
}
const setCheckedNodes = () => {
  treeRef.value!.setCheckedNodes(
    [
      {
        id: 5,
        label: 'Level two 2-1',
      },
      {
        id: 9,
        label: 'Level three 1-1-1',
      },
    ] as Node[],
    false
  )
}
const setCheckedKeys = () => {
  treeRef.value!.setCheckedKeys([3], false)
}
const resetChecked = () => {
  treeRef.value!.setCheckedKeys([], false)
}

const defaultProps = {
  children: 'children',
  label: 'label',
}

const data: Tree[] = [
  {
    id: 1,
    label: 'Level one 1',
    children: [
      {
        id: 4,
        label: 'Level two 1-1',
        children: [
          {
            id: 9,
            label: 'Level three 1-1-1',
          },
          {
            id: 10,
            label: 'Level three 1-1-2',
          },
        ],
      },
    ],
  },
  {
    id: 2,
    label: 'Level one 2',
    children: [
      {
        id: 5,
        label: 'Level two 2-1',
      },
      {
        id: 6,
        label: 'Level two 2-2',
      },
    ],
  },
  {
    id: 3,
    label: 'Level one 3',
    children: [
      {
        id: 7,
        label: 'Level two 3-1',
      },
      {
        id: 8,
        label: 'Level two 3-2',
      },
    ],
  },
]
</script>
```

---

## Input

**URL:** llms-txt#input

**Contents:**
- Basic usage
- Disabled
- Clearable
- Custom Clear Icon ^(2.11.0)
- Formatter
- Password box
- Input with icon
- Textarea
- Autosize Textarea
- Mixed input

Input data using mouse or keyboard.

## Custom Clear Icon ^(2.11.0)

Display value within it's situation with `formatter`, and we usually use `parser` at the same time.

Add an icon to indicate input type.

Resizable for entering multiple lines of text information. Add attribute `type="textarea"` to change `input` into native `textarea`.

Setting the `autosize` prop for a textarea type of Input makes the height to automatically adjust based on the content. An options object can be provided to `autosize` to specify the minimum and maximum number of lines the textarea can automatically adjust.

Prepend or append an element, generally a label or a button.

| Name                          | Description                                                                                                                            | Type                                                                                                | Default     |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | ----------- |
| type                          | type of input, see more in [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#Form_%3Cinput%3E_types)               | ^[string]`'text' \| 'textarea' \| 'number' \| 'password' \| 'email' \| 'search' \| 'tel' \|  'url'` | text        |
| model-value / v-model         | binding value                                                                                                                          | ^[string] / ^[number]                                                                               | —           |
| model-modifiers ^(2.11.5)     | v-model modifiers, reference [Vue modifiers](https://vuejs.org/guide/essentials/forms.html#modifiers)                                  | ^[object]`{ lazy?: boolean, number?: boolean, trim?: boolean }`                                     | —           |
| maxlength                     | same as `maxlength` in native input                                                                                                    | ^[string] / ^[number]                                                                               | —           |
| minlength                     | same as `minlength` in native input                                                                                                    | ^[string] / ^[number]                                                                               | —           |
| show-word-limit               | whether show word count, only works when `type` is 'text' or 'textarea'                                                                | ^[boolean]                                                                                          | false       |
| word-limit-position ^(2.11.5) | word count position, valid when `show-word-limit` is true                                                                              | ^[enum]`'inside' \| 'outside' `                                                                     | "inside"    |
| placeholder                   | placeholder of Input                                                                                                                   | ^[string]                                                                                           | —           |
| clearable                     | whether to show clear button, only works when `type` is not 'textarea'                                                                 | ^[boolean]                                                                                          | false       |
| clear-icon ^(2.11.0)          | custom clear icon component                                                                                                            | ^[string] / ^[object]`Component`                                                                    | CircleClose |
| formatter                     | specifies the format of the value presented input.(only works when `type` is 'text')                                                   | ^[Function]`(value: string \| number) => string`                                                    | —           |
| parser                        | specifies the value extracted from formatter input.(only works when `type` is 'text')                                                  | ^[Function]`(value: string) => string`                                                              | —           |
| show-password                 | whether to show toggleable password input                                                                                              | ^[boolean]                                                                                          | false       |
| disabled                      | whether Input is disabled                                                                                                              | ^[boolean]                                                                                          | false       |
| size                          | size of Input, works when `type` is not 'textarea'                                                                                     | ^[enum]`'large' \| 'default' \| 'small'`                                                            | —           |
| prefix-icon                   | prefix icon component                                                                                                                  | ^[string] / ^[Component]                                                                            | —           |
| suffix-icon                   | suffix icon component                                                                                                                  | ^[string] / ^[Component]                                                                            | —           |
| rows                          | number of rows of textarea, only works when `type` is 'textarea'                                                                       | ^[number]                                                                                           | 2           |
| autosize                      | whether textarea has an adaptive height, only works when `type` is 'textarea'. Can accept an object, e.g. `{ minRows: 2, maxRows: 6 }` | ^[boolean] / ^[object]`{ minRows?: number, maxRows?: number }`                                      | false       |
| autocomplete                  | same as `autocomplete` in native input                                                                                                 | ^[string]                                                                                           | off         |
| name                          | same as `name` in native input                                                                                                         | ^[string]                                                                                           | —           |
| readonly                      | same as `readonly` in native input                                                                                                     | ^[boolean]                                                                                          | false       |
| max                           | same as `max` in native input                                                                                                          | —                                                                                                   | —           |
| min                           | same as `min` in native input                                                                                                          | —                                                                                                   | —           |
| step                          | same as `step` in native input                                                                                                         | —                                                                                                   | —           |
| resize                        | control the resizability                                                                                                               | ^[enum]`'none' \| 'both' \| 'horizontal' \| 'vertical'`                                             | —           |
| autofocus                     | same as `autofocus` in native input                                                                                                    | ^[boolean]                                                                                          | false       |
| form                          | same as `form` in native input                                                                                                         | `string`                                                                                            | —           |
| aria-label ^(a11y) ^(2.7.2)   | same as `aria-label` in native input                                                                                                   | ^[string]                                                                                           | —           |
| tabindex                      | input tabindex                                                                                                                         | ^[string] / ^[number]                                                                               | —           |
| validate-event                | whether to trigger form validation                                                                                                     | ^[boolean]                                                                                          | true        |
| input-style                   | the style of the input element or textarea element                                                                                     | ^[string] / ^[object]`CSSProperties \| CSSProperties[] \| string[]`                                 | {}          |
| label ^(a11y) ^(deprecated)   | same as `aria-label` in native input                                                                                                   | ^[string]                                                                                           | —           |
| inputmode ^(2.10.3)           | same as `inputmode` in native input                                                                                                    | ^[string]                                                                                           | —           |

| Name              | Description                                                                                           | Type                                                        |
| ----------------- | ----------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| blur              | triggers when Input blurs                                                                             | ^[Function]`(event: FocusEvent) => void`                    |
| focus             | triggers when Input focuses                                                                           | ^[Function]`(event: FocusEvent) => void`                    |
| change            | triggers when the input box loses focus or the user presses Enter, only if the modelValue has changed | ^[Function]`(value: string \| number, evt?: Event) => void` |
| input             | triggers when the Input value change                                                                  | ^[Function]`(value: string \| number) => void`              |
| clear             | triggers when the Input is cleared by clicking the clear button                                       | ^[Function]`() => void`                                     |
| keydown           | triggers when a key is pressed down                                                                   | ^[Function]`(event: KeyboardEvent \| Event) => void`        |
| mouseleave        | triggers when the mouse leaves the Input element                                                      | ^[Function]`(event: MouseEvent) => void`                    |
| mouseenter        | triggers when the mouse enters the Input element                                                      | ^[Function]`(event: MouseEvent) => void`                    |
| compositionstart  | triggers when the composition starts                                                                  | ^[Function]`(event: CompositionEvent) => void`              |
| compositionupdate | triggers when the composition is updated                                                              | ^[Function]`(event: CompositionEvent) => void`              |
| compositionend    | triggers when the composition ends                                                                    | ^[Function]`(event: CompositionEvent) => void`              |

| Name    | Description                                                               |
| ------- | ------------------------------------------------------------------------- |
| prefix  | content as Input prefix, only works when `type` is not 'textarea'         |
| suffix  | content as Input suffix, only works when `type` is not 'textarea'         |
| prepend | content to prepend before Input, only works when `type` is not 'textarea' |
| append  | content to append after Input, only works when `type` is not 'textarea'   |

| Name                 | Description                      | Type                                                    |
| -------------------- | -------------------------------- | ------------------------------------------------------- |
| blur                 | blur the input element           | ^[Function]`() => void`                                 |
| clear                | clear input value                | ^[Function]`() => void`                                 |
| focus                | focus the input element          | ^[Function]`() => void`                                 |
| input                | HTML input element               | ^[object]`Ref<HTMLInputElement>`                        |
| ref                  | HTML element, input or textarea  | ^[object]`Ref<HTMLInputElement \| HTMLTextAreaElement>` |
| resizeTextarea       | resize textarea                  | ^[Function]`() => void`                                 |
| select               | select the text in input element | ^[Function]`() => void`                                 |
| textarea             | HTML textarea element            | ^[object]`Ref<HTMLTextAreaElement>`                     |
| textareaStyle        | style of textarea                | ^[object]`Ref<StyleValue>`                              |
| isComposing ^(2.8.0) | is input composing               | ^[object]`Ref<boolean>`                                 |

#### Why is the width of the ElInput component expanded by clearable?

Typical issue: [#7287](https://github.com/element-plus/element-plus/issues/7287)

PS: Since the ElInput component does not have a default width, when the clearable icon is displayed, the width of the component will be expanded, which can be solved by setting width.

### auto-sizing-textarea.vue

### length-limiting.vue

---
Title: Layout
URL: https://element-plus.org/en-US/component/layout
---

**Examples:**

Example 1 (vue):
```vue
<el-input v-model="input" clearable style="width: 200px" />
```

Example 2 (vue):
```vue
<template>
  <el-input
    v-model="textarea1"
    style="width: 240px"
    autosize
    type="textarea"
    placeholder="Please input"
  />
  <div style="margin: 20px 0" />
  <el-input
    v-model="textarea2"
    style="width: 240px"
    :autosize="{ minRows: 2, maxRows: 4 }"
    type="textarea"
    placeholder="Please input"
  />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const textarea1 = ref('')
const textarea2 = ref('')
</script>
```

Example 3 (vue):
```vue
<template>
  <el-input v-model="input" style="width: 240px" placeholder="Please input" />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const input = ref('')
</script>
```

Example 4 (vue):
```vue
<template>
  <el-input
    v-model="input"
    clearable
    :clear-icon="CloseBold"
    placeholder="Custom clear icon"
  />
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { CloseBold } from '@element-plus/icons-vue'

const input = ref('Custom clear icon')
</script>
```

---

## go to your project, link to `element-plus`

**URL:** llms-txt#go-to-your-project,-link-to-`element-plus`

**Contents:**
- Theme

cd your-project
pnpm link --global element-plus
```

> More info see [pnpm link](https://pnpm.io/cli/link).

We should not write Chinese comments in scss files.

It will generate warning `@charset "UTF-8";` in the header of css file when built with vite.

> More info see [#3219](https://github.com/element-plus/element-plus/issues/3219).

---
Title: Local Development
URL: https://element-plus.org/en-US/guide/dev-guide
---

---

## Space

**URL:** llms-txt#space

**Contents:**
- Basic usage
- Vertical layout
- Control the size of the space
- Customized Size
- Auto wrapping
- Spacer
- Literal type spacer
- Spacer can also be VNode
- Alignment
- Fill the container

Even though we have [Divider](/en-US/component/divider), but sometimes we need more than one [Divider](/en-US/component/divider) to split the elements apart, so we stack each elements upon [Divider](/en-US/component/divider), but doing so not only makes our code ugly but also makes it difficult to maintain. **Space** is this kind of component provides us both productivity and elegance.

The basic use case is using this component to provide unified space between each components

Using `direction` attribute to control the layout, we use `flex-direction` to implement this.

## Control the size of the space

Control the space size via `size` API.

You can set the size with built-in sizes `small`, `default`, `large`, these size corresponds to `8px`, `12px`, `16px`. The default size is `small`, A.K.A. `8px`

You can also using customized size to override it. Refer to the next part.

Sometimes built-in sizes could not meet the business needs, we can use custom size (number type) to control the space between items.

When in **horizontal** mode, using `wrap` (**bool type**) to control auto wrapping behavior.

Sometimes we want something more than blank space, so we have (spacer) to help us.

## Literal type spacer

## Spacer can also be VNode

Setting this attribute can adjust the alignment of child nodes, the desirable value can be found at [align-items](https://developer.mozilla.org/en-US/docs/Web/CSS/align-items).

## Fill the container

Through the `fill` **(Boolean type)** parameter, you can control whether the child node automatically fills the container.

In the following example, when set to `fill`, the width of the child node will automatically adapt to the width of the container.

You can also use the `fillRatio` parameter to customize the filling ratio. The default value is `100`, which represents filling based on the width of the parent container at `100%`.

It should be noted that the expression of horizontal layout and vertical layout is slightly different, the specific effect can be viewed in the following example.

| Name       | Description                     | Type                                                                                                                          | Default    |
| ---------- | ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | ---------- |
| alignment  | Controls the alignment of items | ^[enum]`'center' \| 'normal' \| 'stretch' \| ...` [align-items](https://developer.mozilla.org/en-US/docs/Web/CSS/align-items) | center     |
| class      | className                       | ^[string] / ^[object] / ^[array]                                                                                              | —          |
| direction  | Placement direction             | ^[enum]`'vertical' \| 'horizontal'`                                                                                           | horizontal |
| prefix-cls | Prefix for space-items          | ^[string]                                                                                                                     | —          |
| style      | Extra style rules               | ^[string] / ^[object]`CSSProperties \| CSSProperties[] \| string[]`                                                           | —          |
| spacer     | Spacer                          | ^[string] / ^[number] / ^[VNode]                                                                                              | —          |
| size       | Spacing size                    | ^[enum]`'default' \| 'small' \| 'large'` / ^[number] / ^[array]`[number, number]`                                             | small      |
| wrap       | Auto wrapping                   | ^[boolean]                                                                                                                    | false      |
| fill       | Whether to fill the container   | ^[boolean]                                                                                                                    | false      |
| fill-ratio | Ratio of fill                   | ^[number]                                                                                                                     | 100        |

| name    | description        |
| ------- | ------------------ |
| default | Items to be spaced |

### auto-wrapping.vue

### customized-size.vue

### literal-type-spacer.vue

### vertical-layout.vue

### vnode-type-spacer.vue

---
Title: Splitter
URL: https://element-plus.org/en-US/component/splitter
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <div class="alignment-container">
    <el-space>
      string
      <el-button> button </el-button>
      <el-card>
        <template #header> header </template>
        body
      </el-card>
    </el-space>
  </div>
  <div class="alignment-container">
    <el-space alignment="flex-start">
      string
      <el-button> button </el-button>
      <el-card>
        <template #header> header </template>
        body
      </el-card>
    </el-space>
  </div>
  <div class="alignment-container">
    <el-space alignment="flex-end">
      string
      <el-button> button </el-button>
      <el-card>
        <template #header> header </template>
        body
      </el-card>
    </el-space>
  </div>
</template>

<style>
.alignment-container {
  width: 240px;
  margin-bottom: 20px;
  padding: 8px;
  border: 1px solid var(--el-border-color);
}
</style>
```

Example 2 (vue):
```vue
<template>
  <el-space wrap>
    <div v-for="i in 20" :key="i">
      <el-button text> Text button </el-button>
    </div>
  </el-space>
</template>
```

Example 3 (vue):
```vue
<template>
  <el-space wrap>
    <el-card v-for="i in 3" :key="i" class="box-card" style="width: 250px">
      <template #header>
        <div class="card-header">
          <span>Card name</span>
          <el-button class="button" text>Operation button</el-button>
        </div>
      </template>
      <div v-for="o in 4" :key="o" class="text item">
        {{ 'List item ' + o }}
      </div>
    </el-card>
  </el-space>
</template>
```

Example 4 (vue):
```vue
<template>
  <el-space direction="vertical" alignment="start" :size="30">
    <el-radio-group v-model="size">
      <el-radio value="large">Large</el-radio>
      <el-radio value="default">Default</el-radio>
      <el-radio value="small">Small</el-radio>
    </el-radio-group>

    <el-space wrap :size="size">
      <el-card v-for="i in 3" :key="i" class="box-card" style="width: 250px">
        <template #header>
          <div class="card-header">
            <span>Card name</span>
            <el-button class="button" text>Operation button</el-button>
          </div>
        </template>
        <div v-for="o in 4" :key="o" class="text item">
          {{ 'List item ' + o }}
        </div>
      </el-card>
    </el-space>
  </el-space>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

import type { ComponentSize } from 'element-plus'

const size = ref<ComponentSize>('default')
</script>
```

---

## Config Provider

**URL:** llms-txt#config-provider

**Contents:**
- i18n Configurations
- Button Configurations
- Link Configurations ^(2.9.11)
- Card Configurations ^(2.10.5)
- Dialog Configurations ^(2.10.7)
- Message Configurations
- Empty Values Configurations ^(2.7.0)
- Experimental features
- API
  - Config Provider Attributes

Config Provider is used for providing global configurations, which enables your entire application to access these configurations everywhere.

## i18n Configurations

Configure i18n related properties via Config Provider, to get language switching feature.

## Button Configurations

## Link Configurations ^(2.9.11)

## Card Configurations ^(2.10.5)

## Dialog Configurations ^(2.10.7)

## Message Configurations

## Empty Values Configurations ^(2.7.0)

<details>
  <summary>Supported components list</summary>

- Cascader
- ColorPicker ^(2.10.3)
- DatePicker
- Select
- SelectV2
- TimePicker
- TimeSelect
- TreeSelect

Set `empty-values` to support empty values of components. The fallback value is `['', null, undefined]`. If you think the empty string is meaningful, write `[undefined, null]`.

Set `value-on-clear` to set the return value when cleared. The fallback value is `undefined`. In the date component is `null`. If you want to set `undefined`, use `() => undefined`.

## Experimental features

In this section, you can learn how to use Config Provider to provide experimental features. For now, we haven't added any experimental features, but in the feature roadmap, we will add some experimental features. You can use this config to manage the features you want or not.

### Config Provider Attributes

| Name                    | Description                                                                                                                                                            | Type                                                                                                                                                                                                                                                           | Default                                                                                |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| locale                  | Locale Object                                                                                                                                                          | ^[object]`{name: string, el: TranslatePair}`[](https://github.com/element-plus/element-plus/blob/a98ff9b40c0c3d2b9959f99919bd8363e3e3c25a/packages/locale/index.ts#L5) [languages](https://github.com/element-plus/element-plus/tree/dev/packages/locale/lang) | [en](https://github.com/element-plus/element-plus/blob/dev/packages/locale/lang/en.ts) |
| size                    | global component size                                                                                                                                                  | ^[enum]`'large' \| 'default' \| 'small'`                                                                                                                                                                                                                       | default                                                                                |
| zIndex                  | global Initial zIndex                                                                                                                                                  | ^[number]                                                                                                                                                                                                                                                      | —                                                                                      |
| namespace               | global component className prefix (cooperated with [$namespace](https://github.com/element-plus/element-plus/blob/dev/packages/theme-chalk/src/mixins/config.scss#L1)) | ^[string]                                                                                                                                                                                                                                                      | el                                                                                     |
| button                  | button related configuration, [see the following table](#button-attribute)                                                                                             | ^[object]`{autoInsertSpace?: boolean, type?: string, plain?: boolean, round?: boolean}`                                                                                                                                                                        | see the following table                                                                |
| link                    | link related configuration, [see the following table](#link-attribute)                                                                                                 | ^[object]`{type?: string, underline?: boolean \| string}`                                                                                                                                                                                                      | see the following table                                                                |
| dialog ^(2.10.7)        | dialog related configuration, [see the following table](#dialog-attribute)                                                                                             | ^[object]`{alignCenter?: boolean, draggable?: boolean, overflow?: boolean, transition?: DialogTransition}`                                                                                                                                                     | see the following table                                                                |
| message                 | message related configuration, [see the following table](#message-attribute)                                                                                           | ^[object]`{max?: number}`                                                                                                                                                                                                                                      | see the following table                                                                |
| experimental-features   | features at experimental stage to be added, all features are default to be set to false                                                                                | ^[object]                                                                                                                                                                                                                                                      | —                                                                                      |
| empty-values ^(2.7.0)   | global empty values of components                                                                                                                                      | ^[array]                                                                                                                                                                                                                                                       | —                                                                                      |
| value-on-clear ^(2.7.0) | global clear return value                                                                                                                                              | ^[string] / ^[number] / ^[boolean] / ^[Function]                                                                                                                                                                                                               | —                                                                                      |

| Attribute       | Description                                                                                                                                          | Type                                                                                      | Default |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ------- |
| type ^(2.9.11)  | button type, when setting `color`, the latter prevails                                                                                               | ^[enum]`'primary' \| 'success' \| 'warning' \| 'danger' \| 'info' \| 'text' (deprecated)` | —       |
| autoInsertSpace | automatically insert a space between two chinese characters(this will only take effect when the text length is 2 and all characters are in Chinese.) | ^[boolean]                                                                                | false   |
| plain ^(2.9.11) | determine whether it's a plain button                                                                                                                | ^[boolean]                                                                                | false   |
| text ^(2.11.0)  | determine whether it's a text button                                                                                                                 | ^[boolean]                                                                                | false   |
| round ^(2.9.11) | determine whether it's a round button                                                                                                                | ^[boolean]                                                                                | false   |

| Attribute           | Description                   | Type                                                                            | Default |
| ------------------- | ----------------------------- | ------------------------------------------------------------------------------- | ------- |
| type ^(2.9.11)      | type                          | ^[enum]`'primary' \| 'success' \| 'warning' \| 'danger' \| 'info' \| 'default'` | default |
| underline ^(2.9.11) | when underlines should appear | ^[enum]`'always' \| 'hover' \| 'never' \| boolean`                              | hover   |

| Attribute        | Description               | Type                              | Default |
| ---------------- | ------------------------- | --------------------------------- | ------- |
| shadow ^(2.10.5) | when to show card shadows | ^[enum]`always \| never \| hover` | —       |

| Attribute              | Description                                                                                                                    | Type                                   | Default |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------- | ------- |
| align-center ^(2.10.7) | whether to align the dialog both horizontally and vertically                                                                   | ^[boolean]                             | false   |
| draggable ^(2.10.7)    | enable dragging feature for Dialog                                                                                             | ^[boolean]                             | false   |
| overflow ^(2.10.7)     | draggable Dialog can overflow the viewport long                                                                                | ^[boolean]                             | false   |
| transition ^(2.10.7)   | custom transition configuration for dialog animation. Can be a string (transition name) or an object with Vue transition props | ^[string] / ^[object]`TransitionProps` | —       |

### Message Attribute

| Attribute           | Description                                                                    | Type                                                                                       | Default |
| ------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ | ------- |
| max                 | the maximum number of messages that can be displayed at the same time          | ^[number]                                                                                  | —       |
| grouping ^(2.8.2)   | merge messages with the same content, type of VNode message is not supported   | ^[boolean]                                                                                 | —       |
| duration ^(2.8.2)   | display duration, millisecond. If set to 0, it will not turn off automatically | ^[number]                                                                                  | —       |
| showClose ^(2.8.2)  | whether to show a close button                                                 | ^[boolean]                                                                                 | —       |
| offset ^(2.8.2)     | set the distance to the top of viewport                                        | ^[number]                                                                                  | —       |
| plain ^(2.9.11)     | whether message is plain                                                       | ^[boolean]                                                                                 | —       |
| placement ^(2.11.0) | message placement position                                                     | ^[enum]`'top' \| 'top-left' \| 'top-right' \| 'bottom' \| 'bottom-left' \| 'bottom-right'` | —       |

### Config Provider Slots

| Name    | Description               | Type                                                    |
| ------- | ------------------------- | ------------------------------------------------------- |
| default | customize default content | config: provided global config (inherited from the top) |

---
Title: Container
URL: https://element-plus.org/en-US/component/container
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <div>
    <div>
      <el-checkbox v-model="config.autoInsertSpace">
        autoInsertSpace
      </el-checkbox>
      <el-checkbox v-model="config.plain"> plain </el-checkbox>
      <el-checkbox v-model="config.round"> round </el-checkbox>
      <el-checkbox v-model="config.text"> text </el-checkbox>
      <el-select v-model="config.type" class="ml-5" style="max-width: 150px">
        <el-option
          v-for="type in buttonTypes.filter(Boolean)"
          :key="type"
          :value="type"
        />
      </el-select>
    </div>
    <el-divider />
    <el-config-provider :button="config">
      <el-button>中文</el-button>
    </el-config-provider>
  </div>
</template>

<script lang="ts" setup>
import { reactive } from 'vue'
import { buttonTypes } from 'element-plus'

const config = reactive({
  autoInsertSpace: true,
  type: 'default',
  plain: true,
  round: true,
  text: false,
})
</script>
```

Example 2 (vue):
```vue
<script lang="ts" setup>
import { reactive } from 'vue'

const config = reactive({
  shadow: 'always',
})
</script>

<template>
  Shadow:
  <div class="flex flex-col justify-center">
    <el-radio-group v-model="config.shadow">
      <el-radio value="always">always</el-radio>
      <el-radio value="hover">hover</el-radio>
      <el-radio value="never">never</el-radio>
    </el-radio-group>
    <el-divider />
    <el-config-provider :card="config">
      <el-card>Card desu!</el-card>
    </el-config-provider>
  </div>
</template>
```

Example 3 (vue):
```vue
<script lang="ts" setup>
import { computed, nextTick, ref, shallowReactive } from 'vue'

import type { ButtonInstance, DialogTransition } from 'element-plus'

type GlobalConfig = {
  alignCenter: boolean
  draggable: boolean
  overflow: boolean
  transition?: DialogTransition
}

const config = shallowReactive<GlobalConfig>({
  alignCenter: false,
  draggable: false,
  overflow: false,
})
const visible = ref(false)
const enableTransition = ref(false)
const isObjectTransition = ref(false)

const buttonRef = ref<ButtonInstance>()

const ANIMATION_DURATION = 300

const globalConfig = computed<GlobalConfig>(() => {
  let transition: DialogTransition | undefined
  if (enableTransition.value) {
    if (isObjectTransition.value) {
      transition = {
        css: false,
        onBeforeEnter(el) {
          nextTick(() => {
            if (buttonRef.value) {
              const buttonRect = buttonRef.value.ref!.getBoundingClientRect()
              const dialogEl = el.querySelector('.el-dialog') as HTMLElement

              if (dialogEl) {
                const dialogRect = dialogEl.getBoundingClientRect()

                const offsetX =
                  buttonRect.left +
                  buttonRect.width / 2 -
                  (dialogRect.left + dialogRect.width / 2)
                const offsetY =
                  buttonRect.top +
                  buttonRect.height / 2 -
                  (dialogRect.top + dialogRect.height / 2)

                dialogEl.style.transform = `translate(${offsetX}px, ${offsetY}px) scale(0.3)`
                dialogEl.style.opacity = '0'
              }
            }
          })
        },
        onEnter(el, done) {
          nextTick(() => {
            const dialogEl = el.querySelector('.el-dialog') as HTMLElement
            if (dialogEl) {
              // force reflow
              dialogEl.offsetHeight

              dialogEl.style.transition = `all ${ANIMATION_DURATION}ms cubic-bezier(0.4, 0, 1, 1)`
              dialogEl.style.transform = 'translate(0, 0) scale(1)'
              dialogEl.style.opacity = '1'

              // wait for animation to complete, then cleanup inline styles to avoid affecting drag
              setTimeout(() => {
                dialogEl.style.transition = ''
                dialogEl.style.transform = ''
                dialogEl.style.opacity = ''
                done()
              }, ANIMATION_DURATION)
            } else {
              done()
            }
          })
        },
        onLeave(el, done) {
          const dialogEl = el.querySelector('.el-dialog') as HTMLElement
          if (dialogEl) {
            if (buttonRef.value) {
              const buttonRect = buttonRef.value.ref!.getBoundingClientRect()
              const dialogRect = dialogEl.getBoundingClientRect()

              const currentTransform = dialogEl.style.transform
              let dragOffsetX = 0
              let dragOffsetY = 0

              // avoid draggable effect
              if (currentTransform) {
                const translateMatch = currentTransform.match(
                  /translate\(([^,]+),\s*([^)]+)\)/
                )
                if (translateMatch) {
                  dragOffsetX = Number.parseFloat(translateMatch[1])
                  dragOffsetY = Number.parseFloat(translateMatch[2])
                }
              }

              const offsetX =
                buttonRect.left +
                buttonRect.width / 2 -
                (dialogRect.left + dialogRect.width / 2) +
                dragOffsetX
              const offsetY =
                buttonRect.top +
                buttonRect.height / 2 -
                (dialogRect.top + dialogRect.height / 2) +
                dragOffsetY

              dialogEl.style.transition = `all ${ANIMATION_DURATION}ms cubic-bezier(0.4, 0, 1, 1)`
              dialogEl.style.transform = `translate(${offsetX}px, ${offsetY}px) scale(0.3)`
              dialogEl.style.opacity = '0'

              // wait for animation to complete, then cleanup inline styles
              setTimeout(() => {
                dialogEl.style.transition = ''
                dialogEl.style.transform = ''
                dialogEl.style.opacity = ''
                done()
              }, ANIMATION_DURATION)
            } else {
              done()
            }
          } else {
            done()
          }
        },
      }
    } else {
      transition = 'dialog-bounce'
    }
  }
  return {
    alignCenter: config.alignCenter,
    draggable: config.draggable,
    overflow: config.overflow,
    transition,
  }
})
</script>

<template>
  <div class="flex flex-col gap-4 justify-center">
    <div class="flex items-center gap-2">
      <el-switch v-model="config.alignCenter" active-text="alignCenter" />
    </div>
    <div class="flex items-center gap-4">
      <el-switch v-model="config.draggable" active-text="draggable" />
      <el-switch
        v-model="config.overflow"
        :disabled="!config.draggable"
        active-text="overflow"
      />
    </div>
    <div class="flex items-center gap-2">
      <el-switch v-model="enableTransition" active-text="enable transition" />
      <el-switch
        v-model="isObjectTransition"
        :disabled="!enableTransition"
        active-text="transition: object"
        inactive-text="transition: string"
      />
    </div>
    <div class="flex items-center gap-2">
      <el-button
        ref="buttonRef"
        type="primary"
        size="small"
        @click="visible = true"
      >
        Open Dialog
      </el-button>
    </div>
    <el-config-provider :dialog="globalConfig">
      <el-dialog v-model="visible" title="Dialog Title" destroy-on-close>
        Dialog Content
      </el-dialog>
    </el-config-provider>
    <div v-if="enableTransition" class="text-xs opacity-70">
      <div v-if="isObjectTransition">
        Using JavaScript controlled animation:
        <code>{{ JSON.stringify(globalConfig.transition) }}</code>
      </div>
      <div v-else>
        Using string transition name:
        <code>{{ globalConfig.transition }}</code>
      </div>
    </div>
  </div>
</template>

<style>
/* Bounce Animation */
.dialog-bounce-enter-active,
.dialog-bounce-leave-active,
.dialog-bounce-enter-active .el-dialog,
.dialog-bounce-leave-active .el-dialog {
  transition: all 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.dialog-bounce-enter-from,
.dialog-bounce-leave-to {
  opacity: 0;
}

.dialog-bounce-enter-from .el-dialog,
.dialog-bounce-leave-to .el-dialog {
  transform: scale(0.3) translateY(-50px);
  opacity: 0;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <el-config-provider :value-on-clear="null" :empty-values="[undefined, null]">
    <div class="flex flex-wrap gap-4 items-center">
      <el-select
        v-model="value1"
        clearable
        placeholder="Select"
        style="width: 240px"
        @change="handleChange"
      >
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        />
      </el-select>
      <el-select-v2
        v-model="value2"
        clearable
        placeholder="Select"
        style="width: 240px"
        :options="options"
        :value-on-clear="() => undefined"
        @change="handleChange"
      />
    </div>
  </el-config-provider>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { ElMessage } from 'element-plus'

const value1 = ref('')
const value2 = ref('')
const options = [
  {
    value: '',
    label: 'All',
  },
  {
    value: 'Option1',
    label: 'Option1',
  },
  {
    value: 'Option2',
    label: 'Option2',
  },
  {
    value: 'Option3',
    label: 'Option3',
  },
  {
    value: 'Option4',
    label: 'Option4',
  },
  {
    value: 'Option5',
    label: 'Option5',
  },
]

const handleChange = (value) => {
  if ([undefined, null].includes(value)) {
    ElMessage.info(`The clear value is: ${value}`)
  }
}
</script>
```

---

## Button

**URL:** llms-txt#button

**Contents:**
- Basic usage
- Disabled Button
- Link Button
- Text Button
- Icon Button
- Button Group
- Loading Button
- Sizes
- Tag ^(2.3.4)
- Custom Color ^(beta)

Commonly used button.

The `disabled` attribute determines if the button is disabled.

Buttons without border and background.

Use icons to add more meaning to Button. You can use icon alone to save some space, or use it with text.

Displayed as a button group, can be used to group a series of similar operations.

In ^(2.11.9) you can use the `direction` attribute.

Click the button to load data, then the button displays a loading state.

Set `loading` attribute to `true` to display loading state.

Besides default size, Button component provides three additional sizes for you to choose among different scenarios.

You can custom element tag, For example button, div, a, router-link, nuxt-link.

## Custom Color ^(beta)

You can custom button color.

We will calculate hover color & active color automatically.

### Button Attributes

| Name              | Description                                                                                                                                          | Type                                                                                                         | Default |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ | ------- |
| size              | button size                                                                                                                                          | ^[enum]`'large' \| 'default' \| 'small'`                                                                     | —       |
| type              | button type, when setting `color`, the latter prevails                                                                                               | ^[enum]`'default' \| 'primary' \| 'success' \| 'warning' \| 'danger' \| 'info' \| '' \| 'text' (deprecated)` | —       |
| plain             | determine whether it's a plain button                                                                                                                | ^[boolean]                                                                                                   | false   |
| text ^(2.2.0)     | determine whether it's a text button                                                                                                                 | ^[boolean]                                                                                                   | false   |
| bg ^(2.2.0)       | determine whether the text button background color is always on                                                                                      | ^[boolean]                                                                                                   | false   |
| link ^(2.2.1)     | determine whether it's a link button                                                                                                                 | ^[boolean]                                                                                                   | false   |
| round             | determine whether it's a round button                                                                                                                | ^[boolean]                                                                                                   | false   |
| circle            | determine whether it's a circle button                                                                                                               | ^[boolean]                                                                                                   | false   |
| loading           | determine whether it's loading                                                                                                                       | ^[boolean]                                                                                                   | false   |
| loading-icon      | customize loading icon component                                                                                                                     | ^[string] / ^[Component]                                                                                     | Loading |
| disabled          | disable the button                                                                                                                                   | ^[boolean]                                                                                                   | false   |
| icon              | icon component                                                                                                                                       | ^[string] / ^[Component]                                                                                     | —       |
| autofocus         | same as native button's `autofocus`                                                                                                                  | ^[boolean]                                                                                                   | false   |
| native-type       | same as native button's `type`                                                                                                                       | ^[enum]`'button' \| 'submit' \| 'reset'`                                                                     | button  |
| auto-insert-space | automatically insert a space between two chinese characters(this will only take effect when the text length is 2 and all characters are in Chinese.) | ^[boolean]                                                                                                   | false   |
| color             | custom button color, automatically calculate `hover` and `active` color                                                                              | ^[string]                                                                                                    | —       |
| dark              | dark mode, which automatically converts `color` to dark mode colors                                                                                  | ^[boolean]                                                                                                   | false   |
| tag ^(2.3.4)      | custom element tag                                                                                                                                   | ^[string] / ^[Component]                                                                                     | button  |

| Name    | Description                 |
| ------- | --------------------------- |
| default | customize default content   |
| loading | customize loading component |
| icon    | customize icon component    |

| Name           | Description          | Type                                                                                                           |
| -------------- | -------------------- | -------------------------------------------------------------------------------------------------------------- |
| ref            | button html element  | ^[object]`Ref<HTMLButtonElement>`                                                                              |
| size           | button size          | ^[object]`ComputedRef<'' \| 'small' \| 'default' \| 'large'>`                                                  |
| type           | button type          | ^[object]`ComputedRef<'' \| 'default' \| 'primary' \| 'success' \| 'warning' \| 'info' \| 'danger' \| 'text'>` |
| disabled       | button disabled      | ^[object]`ComputedRef<boolean>`                                                                                |
| shouldAddSpace | whether adding space | ^[object]`ComputedRef<boolean>`                                                                                |

### ButtonGroup Attributes

| Name                | Description                                      | Type                                                               | Default    |
| ------------------- | ------------------------------------------------ | ------------------------------------------------------------------ | ---------- |
| size                | control the size of buttons in this button-group | ^[enum]`'large' \| 'default' \| 'small'`                           | —          |
| type                | control the type of buttons in this button-group | ^[enum]`'primary' \| 'success' \| 'warning' \| 'danger' \| 'info'` | —          |
| direction ^(2.11.9) | display direction                                | ^[enum]`'horizontal' \| 'vertical'`                                | horizontal |

### ButtonGroup Slots

| Name    | Description                    | Subtags |
| ------- | ------------------------------ | ------- |
| default | customize button group content | Button  |

---
Title: Calendar
URL: https://element-plus.org/en-US/component/calendar
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <div class="button-example">
    <div class="button-row">
      <el-button>Default</el-button>
      <el-button type="primary">Primary</el-button>
      <el-button type="success">Success</el-button>
      <el-button type="info">Info</el-button>
      <el-button type="warning">Warning</el-button>
      <el-button type="danger">Danger</el-button>
    </div>

    <div class="button-row">
      <el-button plain>Plain</el-button>
      <el-button type="primary" plain>Primary</el-button>
      <el-button type="success" plain>Success</el-button>
      <el-button type="info" plain>Info</el-button>
      <el-button type="warning" plain>Warning</el-button>
      <el-button type="danger" plain>Danger</el-button>
    </div>

    <div class="button-row">
      <el-button round>Round</el-button>
      <el-button type="primary" round>Primary</el-button>
      <el-button type="success" round>Success</el-button>
      <el-button type="info" round>Info</el-button>
      <el-button type="warning" round>Warning</el-button>
      <el-button type="danger" round>Danger</el-button>
    </div>

    <div class="button-row">
      <el-button :icon="Search" circle />
      <el-button type="primary" :icon="Edit" circle />
      <el-button type="success" :icon="Check" circle />
      <el-button type="info" :icon="Message" circle />
      <el-button type="warning" :icon="Star" circle />
      <el-button type="danger" :icon="Delete" circle />
    </div>
  </div>
</template>

<script lang="ts" setup>
import {
  Check,
  Delete,
  Edit,
  Message,
  Search,
  Star,
} from '@element-plus/icons-vue'
</script>

<style scoped>
.button-example {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.button-row {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  align-items: center;
}

.button-row > * {
  margin: 0;
}
</style>
```

Example 2 (vue):
```vue
<script lang="ts" setup>
import { isDark } from '~/composables/dark'
</script>

<template>
  <div>
    <el-button color="#626aef" :dark="isDark">Default</el-button>
    <el-button color="#626aef" :dark="isDark" plain>Plain</el-button>

    <el-button color="#626aef" :dark="isDark" disabled>Disabled</el-button>
    <el-button color="#626aef" :dark="isDark" disabled plain>
      Disabled Plain
    </el-button>
  </div>
</template>
```

Example 3 (vue):
```vue
<template>
  <div class="button-example">
    <div class="button-row">
      <el-button disabled>Default</el-button>
      <el-button type="primary" disabled>Primary</el-button>
      <el-button type="success" disabled>Success</el-button>
      <el-button type="info" disabled>Info</el-button>
      <el-button type="warning" disabled>Warning</el-button>
      <el-button type="danger" disabled>Danger</el-button>
    </div>

    <div class="button-row">
      <el-button plain disabled>Plain</el-button>
      <el-button type="primary" plain disabled>Primary</el-button>
      <el-button type="success" plain disabled>Success</el-button>
      <el-button type="info" plain disabled>Info</el-button>
      <el-button type="warning" plain disabled>Warning</el-button>
      <el-button type="danger" plain disabled>Danger</el-button>
    </div>
  </div>
</template>

<style scoped>
.button-example {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.button-row {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  align-items: center;
}

.button-row > * {
  margin: 0;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <el-button-group class="mb-4">
    <el-button type="primary" :icon="ArrowLeft">Previous Page</el-button>
    <el-button type="primary">
      Next Page<el-icon class="el-icon--right"><ArrowRight /></el-icon>
    </el-button>
  </el-button-group>
  <br />
  <el-radio-group v-model="direction" class="mb-2">
    <el-radio value="horizontal">Horizontal</el-radio>
    <el-radio value="vertical">Vertical</el-radio>
  </el-radio-group>
  <br />

  <el-button-group :direction="direction">
    <el-button type="primary" :icon="House" />
    <el-button type="primary" :icon="Operation" />
    <el-button type="primary" :icon="Notification" />
  </el-button-group>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import {
  ArrowLeft,
  ArrowRight,
  House,
  Notification,
  Operation,
} from '@element-plus/icons-vue'

const direction = ref<'horizontal' | 'vertical'>('horizontal')
</script>
```

---
