# Element-Plus - Navigation

**Pages:** 7

---

## Affix

**URL:** llms-txt#affix

**Contents:**
- Basic Usage
- Target Container
- Fixed Position
- API
  - Attributes
  - Events
  - Slots
  - Exposes
- Vue Examples
  - basic.vue

Fix the element to a specific visible area.

Affix is fixed at the top of the page by default.

You can set `target` attribute to keep the affix in the container at all times. It will be hidden if out of range.

The affix component provides two fixed positions: `top` and `bottom`.

| Name                 | Description                                                                                    | Type                            | Default |
| -------------------- | ---------------------------------------------------------------------------------------------- | ------------------------------- | ------- |
| offset               | offset distance                                                                                | ^[number]                       | 0       |
| position             | position of affix                                                                              | ^[enum]`'top' \| 'bottom'`      | top     |
| target               | target container (CSS selector)                                                                | ^[string]                       | —       |
| z-index              | `z-index` of affix                                                                             | ^[number]                       | 100     |
| teleported ^(2.13.0) | whether affix element is teleported, if `true` it will be teleported to where `append-to` sets | ^[boolean]                      | false   |
| append-to ^(2.13.0)  | which element the affix element appends to                                                     | ^[CSSSelector] / ^[HTMLElement] | body    |

| Name   | Description                       | Type                                                                |
| ------ | --------------------------------- | ------------------------------------------------------------------- |
| change | triggers when fixed state changed | ^[Function]`(fixed: boolean) => void`                               |
| scroll | triggers when scrolling           | ^[Function]`(value: { scrollTop: number, fixed: boolean }) => void` |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

| Name       | Description                 | Type                    |
| ---------- | --------------------------- | ----------------------- |
| update     | update affix state manually | ^[Function]`() => void` |
| updateRoot | update rootRect info        | ^[Function]`() => void` |

---
Title: Alert
URL: https://element-plus.org/en-US/component/alert
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-affix :offset="120">
    <el-button type="primary">Offset top 120px</el-button>
  </el-affix>
</template>
```

Example 2 (vue):
```vue
<template>
  <el-affix position="bottom" :offset="20">
    <el-button type="primary">Offset bottom 20px</el-button>
  </el-affix>
</template>
```

Example 3 (vue):
```vue
<template>
  <div class="affix-container">
    <el-affix target=".affix-container" :offset="80">
      <el-button type="primary">Target container</el-button>
    </el-affix>
  </div>
</template>

<style scoped>
.affix-container {
  text-align: center;
  height: 400px;
  border-radius: 4px;
  background: var(--el-color-primary-light-9);
}
</style>
```

---

## Backtop

**URL:** llms-txt#backtop

**Contents:**
- Basic Usage
- Customizations
- API
  - Attributes
  - Events
  - Slots
- Vue Examples
  - basic.vue
  - custom.vue

A button to back to top.

Scroll down to see the bottom-right button.

Display area is 40px \* 40px.

| Name              | Description                                                          | Type      | Default |
| ----------------- | -------------------------------------------------------------------- | --------- | ------- |
| target            | the target to trigger scroll.                                        | ^[string] | —       |
| visibility-height | the button will not show until the scroll height reaches this value. | ^[number] | 200     |
| right             | right distance.                                                      | ^[number] | 40      |
| bottom            | bottom distance.                                                     | ^[number] | 40      |

| Name  | Description          | Parameters                             |
| ----- | -------------------- | -------------------------------------- |
| click | triggers when click. | ^[Function]`(evt: MouseEvent) => void` |

| Name    | Description                |
| ------- | -------------------------- |
| default | customize default content. |

---
Title: Badge
URL: https://element-plus.org/en-US/component/badge
---

**Examples:**

Example 1 (vue):
```vue
<template>
  Scroll down to see the bottom-right button.
  <el-backtop :right="100" :bottom="100" />
</template>
```

Example 2 (vue):
```vue
<template>
  Scroll down to see the bottom-right button.
  <el-backtop :bottom="100">
    <div
      style="
        height: 100%;
        width: 100%;
        background-color: var(--el-bg-color-overlay);
        box-shadow: var(--el-box-shadow-lighter);
        text-align: center;
        line-height: 40px;
        color: #1989fa;
      "
    >
      UP
    </div>
  </el-backtop>
</template>
```

---

## Anchor

**URL:** llms-txt#anchor

**Contents:**
- Basic Usage
- Horizontal Mode
- Scroll Container
- Anchor link change
- Underline type
- Affix Mode
- Anchor API
  - Anchor Attributes
  - Anchor Events
  - Anchor Exposes

Through the anchor point, you can quickly find the position of the information content on the current page.

Horizontally aligned anchors

Custom scroll area, use `offset` props can set anchor scroll offset, listen the `link-click` event and prevents browser default behavior then it will not change history.

## Anchor link change

Listening for anchor link change

set `type="underline"` change to underline type

Use the affix component to fix the anchor point within the page.

### Anchor Attributes

| Property                   | Description                                                | Type                                   | Default    |
| -------------------------- | ---------------------------------------------------------- | -------------------------------------- | ---------- |
| container                  | scroll container.                                          | `string` \| `HTMLElement` \| `Window ` | —          |
| offset                     | set the offset of the anchor scroll.                       | `number`                               | 0          |
| bound                      | the offset of the element starting to trigger the anchor.  | `number`                               | 15         |
| duration                   | set the scroll duration of the container, in milliseconds. | `number`                               | 300        |
| marker                     | whether to show the marker.                                | ^[boolean]                             | true       |
| type                       | set Anchor type.                                           | ^[enum]`'default' \| 'underline'`      | `default`  |
| direction                  | Set Anchor direction.                                      | ^[enum]`'vertical' \| 'horizontal'`    | `vertical` |
| select-scroll-top ^(2.9.2) | scroll whether link is selected at the top                 | ^[boolean]                             | false      |

| Name   | Description                                | Type                                                |
| ------ | ------------------------------------------ | --------------------------------------------------- |
| change | callback when the step changes             | ^[Function]`(href: string) => void`                 |
| click  | Triggered when the user clicks on the link | ^[Function]`(e: MouseEvent, href?: string) => void` |

| Name     | Description                               | Type                                |
| -------- | ----------------------------------------- | ----------------------------------- |
| scrollTo | Manually scroll to the specific position. | ^[Function]`(href: string) => void` |

| Name    | Description               |
| ------- | ------------------------- |
| default | AnchorLink component list |

### AnchorLink Attributes

| Property | Description                          | Type     | Default |
| -------- | ------------------------------------ | -------- | ------- |
| title    | the text content of the anchor link. | `string` | —       |
| href     | The address of the anchor link.      | `string` | —       |

| Name     | Description                     |
| -------- | ------------------------------- |
| default  | the content of the anchor link. |
| sub-link | slots for child links.          |

---
Title: Autocomplete
URL: https://element-plus.org/en-US/component/autocomplete
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-affix :offset="60">
    <el-anchor :offset="70" style="width: 300px">
      <el-anchor-link :href="`#${locale['basic-usage']}`">
        {{ locale['Basic Usage'] }}
      </el-anchor-link>
      <el-anchor-link :href="`#${locale['horizontal-mode']}`">
        {{ locale['Horizontal Mode'] }}
      </el-anchor-link>
      <el-anchor-link :href="`#${locale['scroll-container']}`">
        {{ locale['Scroll Container'] }}
      </el-anchor-link>
      <el-anchor-link :href="`#${locale['anchor-api']}`">
        {{ locale['Anchor API'] }}
        <template #sub-link>
          <el-anchor-link :href="`#${locale['anchor-attributes']}`">
            {{ locale['Anchor Attributes'] }}
          </el-anchor-link>
          <el-anchor-link :href="`#${locale['anchor-events']}`">
            {{ locale['Anchor Events'] }}
          </el-anchor-link>
        </template>
      </el-anchor-link>
    </el-anchor>
  </el-affix>
</template>

<script lang="ts" setup>
import { computed } from 'vue'
import anchorLocale from '../../.vitepress/i18n/component/anchor.json'
import { useLang } from '~/composables/lang'

const lang = useLang()
const locale = computed(() => anchorLocale[lang.value])
</script>
```

Example 2 (vue):
```vue
<template>
  <el-anchor :offset="70">
    <el-anchor-link :href="`#${locale['basic-usage']}`">
      {{ locale['Basic Usage'] }}
    </el-anchor-link>
    <el-anchor-link :href="`#${locale['horizontal-mode']}`">
      {{ locale['Horizontal Mode'] }}
    </el-anchor-link>
    <el-anchor-link :href="`#${locale['scroll-container']}`">
      {{ locale['Scroll Container'] }}
    </el-anchor-link>
    <el-anchor-link :href="`#${locale['anchor-api']}`">
      {{ locale['Anchor API'] }}
      <template #sub-link>
        <el-anchor-link :href="`#${locale['anchor-attributes']}`">
          {{ locale['Anchor Attributes'] }}
        </el-anchor-link>
        <el-anchor-link :href="`#${locale['anchor-events']}`">
          {{ locale['Anchor Events'] }}
        </el-anchor-link>
      </template>
    </el-anchor-link>
  </el-anchor>
</template>

<script lang="ts" setup>
import { computed } from 'vue'
import anchorLocale from '../../.vitepress/i18n/component/anchor.json'
import { useLang } from '~/composables/lang'

const lang = useLang()
const locale = computed(() => anchorLocale[lang.value])
</script>
```

Example 3 (vue):
```vue
<template>
  <el-anchor :offset="70" @change="handleChange">
    <el-anchor-link :href="`#${locale['basic-usage']}`">
      {{ locale['Basic Usage'] }}
    </el-anchor-link>
    <el-anchor-link :href="`#${locale['horizontal-mode']}`">
      {{ locale['Horizontal Mode'] }}
    </el-anchor-link>
    <el-anchor-link :href="`#${locale['scroll-container']}`">
      {{ locale['Scroll Container'] }}
    </el-anchor-link>
    <el-anchor-link :href="`#${locale['anchor-api']}`">
      {{ locale['Anchor API'] }}
      <template #sub-link>
        <el-anchor-link :href="`#${locale['anchor-attributes']}`">
          {{ locale['Anchor Attributes'] }}
        </el-anchor-link>
        <el-anchor-link :href="`#${locale['anchor-events']}`">
          {{ locale['Anchor Events'] }}
        </el-anchor-link>
      </template>
    </el-anchor-link>
  </el-anchor>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import anchorLocale from '../../.vitepress/i18n/component/anchor.json'
import { useLang } from '~/composables/lang'

const lang = useLang()
const locale = computed(() => anchorLocale[lang.value])

const handleChange = (href: string) => {
  console.log(`anchor change: ${href}`)
}
</script>
```

Example 4 (vue):
```vue
<template>
  <el-anchor :offset="70" direction="horizontal">
    <el-anchor-link :href="`#${locale['basic-usage']}`">
      {{ locale['Basic Usage'] }}
    </el-anchor-link>
    <el-anchor-link :href="`#${locale['horizontal-mode']}`">
      {{ locale['Horizontal Mode'] }}
    </el-anchor-link>
    <el-anchor-link :href="`#${locale['scroll-container']}`">
      {{ locale['Scroll Container'] }}
    </el-anchor-link>
  </el-anchor>
</template>

<script lang="ts" setup>
import { computed } from 'vue'
import anchorLocale from '../../.vitepress/i18n/component/anchor.json'
import { useLang } from '~/composables/lang'

const lang = useLang()
const locale = computed(() => anchorLocale[lang.value])
</script>
```

---

## Page Header

**URL:** llms-txt#page-header

**Contents:**
- Complete example
- Basic usage
- Custom icon
- No icon
- Breadcrumbs
- Additional operation section
- Main content
- Anatomy
- API
  - Attributes

If path of the page is simple, it is recommended to use PageHeader instead of the Breadcrumb.

Standard page header, for simply scenarios.

The default icon might not meet your satisfaction, you can customize the icon by setting `icon` attribute
like the example.

Sometimes the page is just full of elements, and you might not want the icon to show up on the page,
you can set the `icon` attribute to `""` to get rid of it.

Page header allows you to add breadcrumbs for giving route information to the users by `breadcrumb` slot.

## Additional operation section

The header can be as complicated as needed, you may add additional sections to the header, to allow rich
interactions.

Sometimes we want the head to show with some co-responding content, we can utilize the `default` slot for doing so.

The component is consisted of these parts

| Name    | Description                                                   | Type                     | Default |
| ------- | ------------------------------------------------------------- | ------------------------ | ------- |
| icon    | icon component of page header                                 | ^[string] / ^[Component] | Back    |
| title   | main title of page header, default is Back that built-in a11y | ^[string]                | ''      |
| content | content of page header                                        | ^[string]                | ''      |

| Name | Description                         | Type                    |
| ---- | ----------------------------------- | ----------------------- |
| back | triggers when right side is clicked | ^[Function]`() => void` |

| Name       | Description           |
| ---------- | --------------------- |
| icon       | content as icon       |
| title      | content as title      |
| content    | content               |
| extra      | extra                 |
| breadcrumb | content as breadcrumb |
| default    | main content          |

### additional-sections.vue

---
Title: Pagination
URL: https://element-plus.org/en-US/component/pagination
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-page-header>
    <!-- Line 1 -->
    <template #breadcrumb />
    <!-- Line 2 -->
    <template #icon />
    <template #title />
    <template #content />
    <template #extra />
    <!-- Lines after 2 -->
    <template #default />
  </el-page-header>
</template>
```

Example 2 (vue):
```vue
<template>
  <el-page-header icon="">
    <template #content>
      <div class="flex items-center">
        <el-avatar
          :size="32"
          class="mr-3"
          src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"
        />
        <span class="text-large font-600 mr-3"> Title </span>
        <span class="text-sm mr-2" style="color: var(--el-text-color-regular)">
          Sub title
        </span>
        <el-tag>Default</el-tag>
      </div>
    </template>
    <template #extra>
      <div class="flex items-center">
        <el-button>Print</el-button>
        <el-button type="primary" class="ml-2">Edit</el-button>
      </div>
    </template>
  </el-page-header>
</template>
```

Example 3 (vue):
```vue
<template>
  <el-page-header @back="goBack">
    <template #content>
      <span class="text-large font-600 mr-3"> Title </span>
    </template>
  </el-page-header>
</template>

<script lang="ts" setup>
const goBack = () => {
  console.log('go back')
}
</script>
```

Example 4 (vue):
```vue
<template>
  <el-page-header>
    <template #breadcrumb>
      <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: './page-header.html' }">
          homepage
        </el-breadcrumb-item>
        <el-breadcrumb-item
          ><a href="./page-header.html">route 1</a></el-breadcrumb-item
        >
        <el-breadcrumb-item>route 2</el-breadcrumb-item>
      </el-breadcrumb>
    </template>
    <template #content>
      <span class="text-large font-600 mr-3"> Title </span>
    </template>
  </el-page-header>
</template>
```

---

## Breadcrumb

**URL:** llms-txt#breadcrumb

**Contents:**
- Basic usage
- Icon separator
- Breadcrumb API
  - Breadcrumb Attributes
  - Breadcrumb Slots
- BreadcrumbItem API
  - BreadcrumbItem Attributes
  - BreadcrumbItem Slots
- Vue Examples
  - basic.vue

Displays the location of the current page, making it easier to browser back.

### Breadcrumb Attributes

| Name           | Description                      | Type                     | Default |
| -------------- | -------------------------------- | ------------------------ | ------- |
| separator      | separator character              | ^[string]                | /       |
| separator-icon | icon component of icon separator | ^[string] / ^[Component] | —       |

| Name    | Description               | Subtags         |
| ------- | ------------------------- | --------------- |
| default | customize default content | Breadcrumb Item |

## BreadcrumbItem API

### BreadcrumbItem Attributes

| Name    | Description                                               | Type                                    | Default |
| ------- | --------------------------------------------------------- | --------------------------------------- | ------- |
| to      | target route of the link, same as `to` of `vue-router`    | ^[string] / ^[object]`RouteLocationRaw` | ''      |
| replace | if `true`, the navigation will not leave a history record | ^[boolean]                              | false   |

### BreadcrumbItem Slots

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

---
Title: Button
URL: https://element-plus.org/en-US/component/button
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-breadcrumb separator="/">
    <el-breadcrumb-item :to="{ path: '/' }">homepage</el-breadcrumb-item>
    <el-breadcrumb-item>
      <a href="/">promotion management</a>
    </el-breadcrumb-item>
    <el-breadcrumb-item>promotion list</el-breadcrumb-item>
    <el-breadcrumb-item>promotion detail</el-breadcrumb-item>
  </el-breadcrumb>
</template>
```

Example 2 (vue):
```vue
<template>
  <el-breadcrumb :separator-icon="ArrowRight">
    <el-breadcrumb-item :to="{ path: '/' }">homepage</el-breadcrumb-item>
    <el-breadcrumb-item>promotion management</el-breadcrumb-item>
    <el-breadcrumb-item>promotion list</el-breadcrumb-item>
    <el-breadcrumb-item>promotion detail</el-breadcrumb-item>
  </el-breadcrumb>
</template>

<script lang="ts" setup>
import { ArrowRight } from '@element-plus/icons-vue'
</script>
```

---

## Steps

**URL:** llms-txt#steps

**Contents:**
- Basic usage
- Step bar that contains status
- Center
- Step bar with description
- Step bar with icon
- Vertical step bar
- Simple step bar
- Steps API
  - Steps Attributes
  - Steps Slots

Guide the user to complete tasks in accordance with the process. Its steps can be set according to the actual application scenario and the number of the steps can't be less than 2.

## Step bar that contains status

Shows the status of the step for each step.

Title and description can be centered.

## Step bar with description

There is description for each step.

## Step bar with icon

A variety of custom icons can be used in the step bar.

Simple step bars, where `align-center`, `description`, `direction` and `space` will be ignored.

| Name           | Description                                                                   | Type                                                             | Default    |
| -------------- | ----------------------------------------------------------------------------- | ---------------------------------------------------------------- | ---------- |
| space          | the spacing of each step, will be responsive if omitted. Supports percentage. | ^[number] / ^[string]                                            | ''         |
| direction      | display direction                                                             | ^[enum]`'vertical' \| 'horizontal'`                              | horizontal |
| active         | current activation step                                                       | ^[number]                                                        | 0          |
| process-status | status of current step                                                        | ^[enum]`'wait' \| 'process' \| 'finish' \| 'error' \| 'success'` | process    |
| finish-status  | status of end step                                                            | ^[enum]`'wait' \| 'process' \| 'finish' \| 'error' \| 'success'` | finish     |
| align-center   | center title and description                                                  | ^[boolean]                                                       | —          |
| simple         | whether to apply simple theme                                                 | ^[boolean]                                                       | —          |

| Name    | Description               | Subtags |
| ------- | ------------------------- | ------- |
| default | customize default content | Step    |

| Name        | Description                                                              | Type                                                                   | Default |
| ----------- | ------------------------------------------------------------------------ | ---------------------------------------------------------------------- | ------- |
| title       | step title                                                               | ^[string]                                                              | ''      |
| description | step description                                                         | ^[string]                                                              | ''      |
| icon        | step custom icon. Icons can be passed via named slot as well             | ^[string] / ^[Component]                                               | —       |
| status      | current status. It will be automatically set by Steps if not configured. | ^[enum]`'' \| 'wait' \| 'process' \| 'finish' \| 'error' \| 'success'` | ''      |

| Name        | Description      |
| ----------- | ---------------- |
| icon        | custom icon      |
| title       | step title       |
| description | step description |

### with-description.vue

---
Title: Switch
URL: https://element-plus.org/en-US/component/switch
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-steps style="max-width: 600px" :active="active" finish-status="success">
    <el-step title="Step 1" />
    <el-step title="Step 2" />
    <el-step title="Step 3" />
  </el-steps>

  <el-button style="margin-top: 12px" @click="next">Next step</el-button>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const active = ref(0)

const next = () => {
  if (active.value++ > 2) active.value = 0
}
</script>
```

Example 2 (vue):
```vue
<template>
  <el-steps style="max-width: 600px" :active="2" align-center>
    <el-step title="Step 1" description="Some description" />
    <el-step title="Step 2" description="Some description" />
    <el-step title="Step 3" description="Some description" />
  </el-steps>
</template>
```

Example 3 (vue):
```vue
<template>
  <el-steps
    class="mb-4"
    style="max-width: 600px"
    :space="200"
    :active="1"
    simple
  >
    <el-step title="Step 1" :icon="Edit" />
    <el-step title="Step 2" :icon="UploadFilled" />
    <el-step title="Step 3" :icon="Picture" />
  </el-steps>

  <el-steps style="max-width: 600px" :active="1" finish-status="success" simple>
    <el-step title="Step 1" />
    <el-step title="Step 2" />
    <el-step title="Step 3" />
  </el-steps>
</template>

<script lang="ts" setup>
import { Edit, Picture, UploadFilled } from '@element-plus/icons-vue'
</script>
```

Example 4 (vue):
```vue
<template>
  <div style="height: 300px; max-width: 600px">
    <el-steps direction="vertical" :active="1">
      <el-step title="Step 1" />
      <el-step title="Step 2" />
      <el-step title="Step 3" />
    </el-steps>
  </div>
</template>
```

---

## Menu

**URL:** llms-txt#menu

**Contents:**
- Top bar
- Left And Right
- Side bar
- Collapse
- Popper Offset ^(2.4.4)
- Menu API
  - Menu Attributes
  - Menu Events
  - Menu Slots
  - Menu Exposes

Menu that provides navigation for your website.

Top bar Menu can be used in a variety of scenarios.

Vertical Menu with sub-menus.

Vertical Menu could be collapsed.

## Popper Offset ^(2.4.4)

Submenu with popperOffset will override Menu's `popper-offset`.

| Name                            | Description                                                                                                                                                           | Type                                   | Default  |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- | -------- |
| mode                            | menu display mode                                                                                                                                                     | ^[enum]`'horizontal' \| 'vertical'`    | vertical |
| collapse                        | whether the menu is collapsed (available only in vertical mode)                                                                                                       | ^[boolean]                             | false    |
| ellipsis                        | whether the menu is ellipsis (available only in horizontal mode)                                                                                                      | ^[boolean]                             | true     |
| ellipsis-icon ^(2.4.4)          | custom ellipsis icon (available only in horizontal mode and ellipsis is true)                                                                                         | ^[string] / ^[Component]               | —        |
| popper-offset ^(2.4.4)          | offset of the popper (effective for all submenus)                                                                                                                     | ^[number]                              | 6        |
| default-active                  | index of active menu on page load                                                                                                                                     | ^[string]                              | ''       |
| default-openeds                 | array that contains indexes of currently active sub-menus                                                                                                             | ^[object]`string[]`                    | []       |
| unique-opened                   | whether only one sub-menu can be active                                                                                                                               | ^[boolean]                             | false    |
| menu-trigger                    | how sub-menus are triggered, only works when `mode` is 'horizontal'                                                                                                   | ^[enum]`'hover' \| 'click'`            | hover    |
| router                          | whether `vue-router` mode is activated. If true, index will be used as 'path' to activate the route action. Use with `default-active` to set the active item on load. | ^[boolean]                             | false    |
| collapse-transition             | whether to enable the collapse transition                                                                                                                             | ^[boolean]                             | true     |
| popper-effect ^(2.2.26)         | Tooltip theme, built-in theme: `dark` / `light` when menu is collapsed                                                                                                | ^[enum]`'dark' \| 'light'` / ^[string] | dark     |
| close-on-click-outside ^(2.4.4) | optional, whether menu is collapsed when clicking outside                                                                                                             | ^[boolean]                             | false    |
| popper-class ^(2.5.0)           | custom class name for all popup menus and titles' tooltips                                                                                                            | ^[string]                              | —        |
| popper-style ^(2.11.5)          | custom style for all popup menus and titles' tooltips                                                                                                                 | ^[string] / ^[object]                  | —        |
| show-timeout ^(2.5.0)           | control timeout for all menus before showing                                                                                                                          | ^[number]                              | 300      |
| hide-timeout ^(2.5.0)           | control timeout for all menus before hiding                                                                                                                           | ^[number]                              | 300      |
| background-color ^(deprecated)  | background color of Menu (hex format) (use `--el-menu-bg-color` in a style class instead)                                                                             | ^[string]                              | #ffffff  |
| text-color ^(deprecated)        | text color of Menu (hex format) ( use `--el-menu-text-color` in a style class instead)                                                                                | ^[string]                              | #303133  |
| active-text-color ^(deprecated) | text color of currently active menu item (hex format) ( use `--el-menu-active-color` in a style class instead)                                                        | ^[string]                              | #409eff  |
| persistent ^(2.9.5)             | when menu inactive and `persistent` is `false` , dropdown menu will be destroyed                                                                                      | ^[boolean]                             | true     |

| Name   | Description                               | Type                         |
| ------ | ----------------------------------------- | ---------------------------- |
| select | callback function when menu is activated  | ^[Function]`MenuSelectEvent` |
| open   | callback function when sub-menu expands   | ^[Function]`MenuOpenEvent`   |
| close  | callback function when sub-menu collapses | ^[Function]`MenuCloseEvent`  |

| Name    | Description               | Subtags                               |
| ------- | ------------------------- | ------------------------------------- |
| default | customize default content | SubMenu / Menu-Item / Menu-Item-Group |

| Name                       | Description                                                            | Type                                 |
| -------------------------- | ---------------------------------------------------------------------- | ------------------------------------ |
| open                       | open a specific sub-menu, the param is index of the sub-menu to open   | ^[Function]`(index: string) => void` |
| close                      | close a specific sub-menu, the param is index of the sub-menu to close | ^[Function]`(index: string) => void` |
| handleResize               | manually trigger menu width recalculation                              | ^[Function]`() => void`              |
| updateActiveIndex ^(2.9.8) | set index of active menu                                               | ^[Function]`(index: string) => void` |

### SubMenu Attributes

| Name                   | Description                                                                                                                                   | Type                     | Default   |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------ | --------- |
| index ^(required)      | unique identification                                                                                                                         | ^[string]                | —         |
| popper-class           | custom class name for the popup menu                                                                                                          | ^[string]                | —         |
| popper-style ^(2.11.5) | custom style for the popup menu                                                                                                               | ^[string] / ^[object]    | —         |
| show-timeout           | timeout before showing a sub-menu(inherit `show-timeout` of the menu by default.)                                                             | ^[number]                | —         |
| hide-timeout           | timeout before hiding a sub-menu(inherit `hide-timeout` of the menu by default.)                                                              | ^[number]                | —         |
| disabled               | whether the sub-menu is disabled                                                                                                              | ^[boolean]               | false     |
| teleported             | whether popup menu is teleported to the body, the default is true for the level one SubMenu, false for other SubMenus                         | ^[boolean]               | undefined |
| popper-offset          | offset of the popper (overrides the `popper` of menu)                                                                                         | ^[number]                | —         |
| expand-close-icon      | Icon when menu are expanded and submenu are closed, `expand-close-icon` and `expand-open-icon` need to be passed together to take effect      | ^[string] / ^[Component] | —         |
| expand-open-icon       | Icon when menu are expanded and submenu are opened, `expand-open-icon` and `expand-close-icon` need to be passed together to take effect      | ^[string] / ^[Component] | —         |
| collapse-close-icon    | Icon when menu are collapsed and submenu are closed, `collapse-close-icon` and `collapse-open-icon` need to be passed together to take effect | ^[string] / ^[Component] | —         |
| collapse-open-icon     | Icon when menu are collapsed and submenu are opened, `collapse-open-icon` and `collapse-close-icon` need to be passed together to take effect | ^[string] / ^[Component] | —         |

| Name    | Description               | Subtags                               |
| ------- | ------------------------- | ------------------------------------- |
| default | customize default content | SubMenu / Menu-Item / Menu-Item-Group |
| title   | customize title content   | —                                     |

### Menu-Item Attributes

| Name              | Description                          | Type                  | Default |
| ----------------- | ------------------------------------ | --------------------- | ------- |
| index ^(required) | unique identification                | ^[string]             | —       |
| route             | Vue Router Route Location Parameters | ^[string] / ^[object] | —       |
| disabled          | whether disabled                     | ^[boolean]            | false   |

| Name  | Description                                                                  | Type                                            |
| ----- | ---------------------------------------------------------------------------- | ----------------------------------------------- |
| click | callback function when menu-item is clicked, the param is menu-item instance | ^[Function]`(item: MenuItemRegistered) => void` |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |
| title   | customize title content   |

## Menu-Item-Group API

### Menu-Item-Group Attributes

| Name  | Description | Type      | Default |
| ----- | ----------- | --------- | ------- |
| title | group title | ^[string] | —       |

### Menu-Item-Group Slots

| Name    | Description               | Subtags   |
| ------- | ------------------------- | --------- |
| default | customize default content | Menu-Item |
| title   | customize group title     | —         |

<details>
  <summary>Show declarations</summary>

### left-and-right.vue

### popper-offset.vue

---
Title: Message Box
URL: https://element-plus.org/en-US/component/message-box
---

**Examples:**

Example 1 (ts):
```ts
/**
 * @param index index of activated menu
 * @param indexPath index path of activated menu
 * @param item the selected menu item
 * @param routerResult result returned by `vue-router` if `router` is enabled
 */
type MenuSelectEvent = (
  index: string,
  indexPath: string[],
  item: MenuItemClicked,
  routerResult?: Promise<void | NavigationFailure>
) => void

/**
 * @param index index of expanded sub-menu
 * @param indexPath index path of expanded sub-menu
 */
type MenuOpenEvent = (index: string, indexPath: string[]) => void

/**
 * @param index index of collapsed sub-menu
 * @param indexPath index path of collapsed sub-menu
 */
type MenuCloseEvent = (index: string, indexPath: string[]) => void

interface MenuItemRegistered {
  index: string
  indexPath: string[]
  active: boolean
}

interface MenuItemClicked {
  index: string
  indexPath: string[]
  route?: RouteLocationRaw
}
```

Example 2 (vue):
```vue
<template>
  <el-menu
    :default-active="activeIndex"
    class="el-menu-demo"
    mode="horizontal"
    @select="handleSelect"
  >
    <el-menu-item index="1">Processing Center</el-menu-item>
    <el-sub-menu index="2">
      <template #title>Workspace</template>
      <el-menu-item index="2-1">item one</el-menu-item>
      <el-menu-item index="2-2">item two</el-menu-item>
      <el-menu-item index="2-3">item three</el-menu-item>
      <el-sub-menu index="2-4">
        <template #title>item four</template>
        <el-menu-item index="2-4-1">item one</el-menu-item>
        <el-menu-item index="2-4-2">item two</el-menu-item>
        <el-menu-item index="2-4-3">item three</el-menu-item>
      </el-sub-menu>
    </el-sub-menu>
    <el-menu-item index="3" disabled>Info</el-menu-item>
    <el-menu-item index="4">Orders</el-menu-item>
  </el-menu>
  <div class="h-6" />
  <el-menu
    :default-active="activeIndex2"
    class="el-menu-demo"
    mode="horizontal"
    background-color="#545c64"
    text-color="#fff"
    active-text-color="#ffd04b"
    @select="handleSelect"
  >
    <el-menu-item index="1">Processing Center</el-menu-item>
    <el-sub-menu index="2">
      <template #title>Workspace</template>
      <el-menu-item index="2-1">item one</el-menu-item>
      <el-menu-item index="2-2">item two</el-menu-item>
      <el-menu-item index="2-3">item three</el-menu-item>
      <el-sub-menu index="2-4">
        <template #title>item four</template>
        <el-menu-item index="2-4-1">item one</el-menu-item>
        <el-menu-item index="2-4-2">item two</el-menu-item>
        <el-menu-item index="2-4-3">item three</el-menu-item>
      </el-sub-menu>
    </el-sub-menu>
    <el-menu-item index="3" disabled>Info</el-menu-item>
    <el-menu-item index="4">Orders</el-menu-item>
  </el-menu>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const activeIndex = ref('1')
const activeIndex2 = ref('1')
const handleSelect = (key: string, keyPath: string[]) => {
  console.log(key, keyPath)
}
</script>
```

Example 3 (vue):
```vue
<template>
  <el-radio-group v-model="isCollapse" style="margin-bottom: 20px">
    <el-radio-button :value="false">expand</el-radio-button>
    <el-radio-button :value="true">collapse</el-radio-button>
  </el-radio-group>
  <el-menu
    default-active="2"
    class="el-menu-vertical-demo"
    :collapse="isCollapse"
    @open="handleOpen"
    @close="handleClose"
  >
    <el-sub-menu index="1">
      <template #title>
        <el-icon><location /></el-icon>
        <span>Navigator One</span>
      </template>
      <el-menu-item-group>
        <template #title><span>Group One</span></template>
        <el-menu-item index="1-1">item one</el-menu-item>
        <el-menu-item index="1-2">item two</el-menu-item>
      </el-menu-item-group>
      <el-menu-item-group title="Group Two">
        <el-menu-item index="1-3">item three</el-menu-item>
      </el-menu-item-group>
      <el-sub-menu index="1-4">
        <template #title><span>item four</span></template>
        <el-menu-item index="1-4-1">item one</el-menu-item>
      </el-sub-menu>
    </el-sub-menu>
    <el-menu-item index="2">
      <el-icon><icon-menu /></el-icon>
      <template #title>Navigator Two</template>
    </el-menu-item>
    <el-menu-item index="3" disabled>
      <el-icon><document /></el-icon>
      <template #title>Navigator Three</template>
    </el-menu-item>
    <el-menu-item index="4">
      <el-icon><setting /></el-icon>
      <template #title>Navigator Four</template>
    </el-menu-item>
  </el-menu>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import {
  Document,
  Menu as IconMenu,
  Location,
  Setting,
} from '@element-plus/icons-vue'

const isCollapse = ref(true)
const handleOpen = (key: string, keyPath: string[]) => {
  console.log(key, keyPath)
}
const handleClose = (key: string, keyPath: string[]) => {
  console.log(key, keyPath)
}
</script>

<style>
.el-menu-vertical-demo:not(.el-menu--collapse) {
  width: 200px;
  min-height: 400px;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <el-menu
    :default-active="activeIndex"
    class="el-menu-demo"
    mode="horizontal"
    :ellipsis="false"
    @select="handleSelect"
  >
    <el-menu-item index="0">
      <img
        style="width: 100px"
        src="/images/element-plus-logo.svg"
        alt="Element logo"
      />
    </el-menu-item>
    <el-menu-item index="1">Processing Center</el-menu-item>
    <el-sub-menu index="2">
      <template #title>Workspace</template>
      <el-menu-item index="2-1">item one</el-menu-item>
      <el-menu-item index="2-2">item two</el-menu-item>
      <el-menu-item index="2-3">item three</el-menu-item>
      <el-sub-menu index="2-4">
        <template #title>item four</template>
        <el-menu-item index="2-4-1">item one</el-menu-item>
        <el-menu-item index="2-4-2">item two</el-menu-item>
        <el-menu-item index="2-4-3">item three</el-menu-item>
      </el-sub-menu>
    </el-sub-menu>
  </el-menu>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const activeIndex = ref('1')
const handleSelect = (key: string, keyPath: string[]) => {
  console.log(key, keyPath)
}
</script>

<style scoped>
.el-menu--horizontal > .el-menu-item:nth-child(1) {
  margin-right: auto;
}
</style>
```

---
