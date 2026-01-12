# Element-Plus - Feedback

**Pages:** 7

---

## Drawer

**URL:** llms-txt#drawer

**Contents:**
- Basic Usage
- No Title
- Customized Content
- Customized Header
- Resizable Drawer ^(2.11.0)
- Nested Drawer
- Modal
- API
  - Attributes
  - Events

Sometimes, `Dialog` does not always satisfy our requirements, let's say you have a massive form, or you need space to display something like `terms & conditions`, `Drawer` has almost identical API with `Dialog`, but it introduces different user experience.

Callout a temporary drawer, from multiple direction

When you no longer need a title, you can remove it from the drawer.

## Customized Content

Like `Dialog`, `Drawer` can be used to display a multitude of diverse interactions.

The `header` slot can be used to customize the area where the title is displayed. In order to maintain accessibility, use the `title` attribute in addition to using this slot, or use the `titleId` slot property to specify which element should be read out as the drawer title.

## Resizable Drawer ^(2.11.0)

Try to drag the edge part.

You can also have multiple layer of `Drawer` just like `Dialog`.

Setting `modal` to `false` will hide modal (overlay) of drawer.

Starting from version ^(2.11.7), `modal-penetrable` attribute is added, which can be penetrable.

| Name                       | Description                                                                                                                                                                                                                                                                                                  | Type                                                                                                                                                                                           | Default |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| model-value / v-model      | Should Drawer be displayed                                                                                                                                                                                                                                                                                   | ^[boolean]                                                                                                                                                                                     | false   |
| append-to-body             | Controls should Drawer be inserted to DocumentBody Element, nested Drawer must assign this param to **true**                                                                                                                                                                                                 | ^[boolean]                                                                                                                                                                                     | false   |
| append-to ^(2.8.0)         | which element the Drawer appends to. Will override `append-to-body`                                                                                                                                                                                                                                          | ^[CSSSelector] / ^[HTMLElement]                                                                                                                                                                | body    |
| lock-scroll                | whether scroll of body is disabled while Drawer is displayed                                                                                                                                                                                                                                                 | ^[boolean]                                                                                                                                                                                     | true    |
| before-close               | If set, closing procedure will be halted                                                                                                                                                                                                                                                                     | ^[Function]`(done: (cancel?: boolean) => void) => void(done is function type that accepts a boolean as parameter, calling done with true or without parameter will abort the close procedure)` | —       |
| close-on-click-modal       | whether the Drawer can be closed by clicking the mask                                                                                                                                                                                                                                                        | ^[boolean]                                                                                                                                                                                     | true    |
| close-on-press-escape      | Indicates whether Drawer can be closed by pressing ESC                                                                                                                                                                                                                                                       | ^[boolean]                                                                                                                                                                                     | true    |
| open-delay                 | Time(milliseconds) before open                                                                                                                                                                                                                                                                               | ^[number]                                                                                                                                                                                      | 0       |
| close-delay                | Time(milliseconds) before close                                                                                                                                                                                                                                                                              | ^[number]                                                                                                                                                                                      | 0       |
| destroy-on-close           | Indicates whether children should be destroyed after Drawer closed                                                                                                                                                                                                                                           | ^[boolean]                                                                                                                                                                                     | false   |
| modal                      | Should show shadowing layer                                                                                                                                                                                                                                                                                  | ^[boolean]                                                                                                                                                                                     | true    |
| modal-penetrable ^(2.11.7) | whether the mask is penetrable. The modal attribute must be `false`.                                                                                                                                                                                                                                         | ^[boolean]                                                                                                                                                                                     | false   |
| direction                  | Drawer's opening direction                                                                                                                                                                                                                                                                                   | ^[enum]`'rtl' \| 'ltr' \| 'ttb' \| 'btt'`                                                                                                                                                      | rtl     |
| resizable ^(2.11.0)        | enable resizable feature for Drawer                                                                                                                                                                                                                                                                          | ^[boolean]                                                                                                                                                                                     | false   |
| show-close                 | Should show close button at the top right of Drawer                                                                                                                                                                                                                                                          | ^[boolean]                                                                                                                                                                                     | true    |
| size                       | Drawer's size, if Drawer is horizontal mode, it effects the width property, otherwise it effects the height property, when size is `number` type, it describes the size by unit of pixels; when size is `string` type, it should be used with `x%` notation, other wise it will be interpreted to pixel unit | ^[number] / ^[string]                                                                                                                                                                          | 30%     |
| title                      | Drawer's title, can also be set by named slot, detailed descriptions can be found in the slot form                                                                                                                                                                                                           | ^[string]                                                                                                                                                                                      | —       |
| with-header                | Flag that controls the header section's existence, default to true, when withHeader set to false, both `title attribute` and `title slot` won't work                                                                                                                                                         | ^[boolean]                                                                                                                                                                                     | true    |
| modal-class                | Extra class names for shadowing layer                                                                                                                                                                                                                                                                        | ^[string]                                                                                                                                                                                      | —       |
| header-class ^(2.9.3)      | custom class names for header wrapper                                                                                                                                                                                                                                                                        | ^[string]                                                                                                                                                                                      | —       |
| body-class ^(2.9.3)        | custom class names for body wrapper                                                                                                                                                                                                                                                                          | ^[string]                                                                                                                                                                                      | —       |
| footer-class ^(2.9.3)      | custom class names for footer wrapper                                                                                                                                                                                                                                                                        | ^[string]                                                                                                                                                                                      | —       |
| z-index                    | set z-index                                                                                                                                                                                                                                                                                                  | ^[number]                                                                                                                                                                                      | —       |
| header-aria-level ^(a11y)  | header's `aria-level` attribute                                                                                                                                                                                                                                                                              | ^[string]                                                                                                                                                                                      | 2       |
| custom-class ^(deprecated) | Extra class names for Drawer                                                                                                                                                                                                                                                                                 | ^[string]                                                                                                                                                                                      | —       |

| Name                   | Description                                                  | Type                                                 |
| ---------------------- | ------------------------------------------------------------ | ---------------------------------------------------- |
| open                   | Triggered before Drawer opening animation begins             | ^[Function]`() => void`                              |
| opened                 | Triggered after Drawer opening animation ended               | ^[Function]`() => void`                              |
| close                  | Triggered before Drawer closing animation begins             | ^[Function]`() => void`                              |
| closed                 | Triggered after Drawer closing animation ended               | ^[Function]`() => void`                              |
| open-auto-focus        | triggers after Drawer opens and content focused              | ^[Function]`() => void`                              |
| close-auto-focus       | triggers after Drawer closed and content focused             | ^[Function]`() => void`                              |
| resize-start ^(2.11.8) | Triggered when resizing starts (when `resizable` is enabled) | ^[Function]`(evt: MouseEvent, size: number) => void` |
| resize ^(2.11.8)       | Triggered while resizing (when `resizable` is enabled)       | ^[Function]`(evt: MouseEvent, size: number) => void` |
| resize-end ^(2.11.8)   | Triggered when resizing ends (when `resizable` is enabled)   | ^[Function]`(evt: MouseEvent, size: number) => void` |

| Name                | Description                                                                                    |
| ------------------- | ---------------------------------------------------------------------------------------------- |
| default             | Drawer's Content                                                                               |
| header              | Drawer header section; Replacing this removes the title, but does not remove the close button. |
| footer              | Drawer footer Section                                                                          |
| title ^(deprecated) | Works the same as the header slot. Use that instead.                                           |

| Name        | Description                                                     |
| ----------- | --------------------------------------------------------------- |
| handleClose | In order to close Drawer, this method will call `before-close`. |

### customization-content.vue

### customization-header.vue

### nested-drawer.vue

---
Title: Dropdown
URL: https://element-plus.org/en-US/component/dropdown
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-radio-group v-model="direction">
    <el-radio value="ltr">left to right</el-radio>
    <el-radio value="rtl">right to left</el-radio>
    <el-radio value="ttb">top to bottom</el-radio>
    <el-radio value="btt">bottom to top</el-radio>
  </el-radio-group>

  <el-button type="primary" style="margin-left: 16px" @click="drawer = true">
    open
  </el-button>
  <el-button type="primary" style="margin-left: 16px" @click="drawer2 = true">
    with footer
  </el-button>

  <el-drawer
    v-model="drawer"
    title="I am the title"
    :direction="direction"
    :before-close="handleClose"
  >
    <span>Hi, there!</span>
  </el-drawer>
  <el-drawer v-model="drawer2" :direction="direction">
    <template #header>
      <h4>set title by slot</h4>
    </template>
    <template #default>
      <div>
        <el-radio v-model="radio1" value="Option 1" size="large">
          Option 1
        </el-radio>
        <el-radio v-model="radio1" value="Option 2" size="large">
          Option 2
        </el-radio>
      </div>
    </template>
    <template #footer>
      <div style="flex: auto">
        <el-button @click="cancelClick">cancel</el-button>
        <el-button type="primary" @click="confirmClick">confirm</el-button>
      </div>
    </template>
  </el-drawer>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { ElMessageBox } from 'element-plus'

import type { DrawerProps } from 'element-plus'

const drawer = ref(false)
const drawer2 = ref(false)
const direction = ref<DrawerProps['direction']>('rtl')
const radio1 = ref('Option 1')
const handleClose = (done: () => void) => {
  ElMessageBox.confirm('Are you sure you want to close this?')
    .then(() => {
      done()
    })
    .catch(() => {
      // catch error
    })
}
function cancelClick() {
  drawer2.value = false
}
function confirmClick() {
  ElMessageBox.confirm(`Are you confirm to chose ${radio1.value} ?`)
    .then(() => {
      drawer2.value = false
    })
    .catch(() => {
      // catch error
    })
}
</script>
```

Example 2 (vue):
```vue
<template>
  <el-button text @click="table = true">
    Open Drawer with nested table
  </el-button>
  <el-button text @click="dialog = true">
    Open Drawer with nested form
  </el-button>
  <el-drawer
    v-model="table"
    title="I have a nested table inside!"
    direction="rtl"
    size="50%"
  >
    <el-table :data="gridData">
      <el-table-column property="date" label="Date" width="150" />
      <el-table-column property="name" label="Name" width="200" />
      <el-table-column property="address" label="Address" />
    </el-table>
  </el-drawer>

  <el-drawer
    v-model="dialog"
    title="I have a nested form inside!"
    :before-close="handleClose"
    direction="ltr"
    class="demo-drawer"
  >
    <div class="demo-drawer__content">
      <el-form :model="form">
        <el-form-item label="Name" :label-width="formLabelWidth">
          <el-input v-model="form.name" autocomplete="off" />
        </el-form-item>
        <el-form-item label="Area" :label-width="formLabelWidth">
          <el-select
            v-model="form.region"
            placeholder="Please select activity area"
          >
            <el-option label="Area1" value="shanghai" />
            <el-option label="Area2" value="beijing" />
          </el-select>
        </el-form-item>
      </el-form>
      <div class="demo-drawer__footer">
        <el-button @click="cancelForm">Cancel</el-button>
        <el-button type="primary" :loading="loading" @click="onClick">
          {{ loading ? 'Submitting ...' : 'Submit' }}
        </el-button>
      </div>
    </div>
  </el-drawer>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue'
import { ElMessageBox } from 'element-plus'

const formLabelWidth = '80px'
let timer

const table = ref(false)
const dialog = ref(false)
const loading = ref(false)

const form = reactive({
  name: '',
  region: '',
  date1: '',
  date2: '',
  delivery: false,
  type: [],
  resource: '',
  desc: '',
})

const gridData = [
  {
    date: '2016-05-02',
    name: 'Peter Parker',
    address: 'Queens, New York City',
  },
  {
    date: '2016-05-04',
    name: 'Peter Parker',
    address: 'Queens, New York City',
  },
  {
    date: '2016-05-01',
    name: 'Peter Parker',
    address: 'Queens, New York City',
  },
  {
    date: '2016-05-03',
    name: 'Peter Parker',
    address: 'Queens, New York City',
  },
]

const onClick = () => {
  loading.value = true
  setTimeout(() => {
    loading.value = false
    dialog.value = false
  }, 400)
}

const handleClose = (done) => {
  if (loading.value) {
    return
  }
  ElMessageBox.confirm('Do you want to submit?')
    .then(() => {
      loading.value = true
      timer = setTimeout(() => {
        done()
        // 动画关闭需要一定的时间
        setTimeout(() => {
          loading.value = false
        }, 400)
      }, 2000)
    })
    .catch(() => {
      // catch error
    })
}

const cancelForm = () => {
  loading.value = false
  dialog.value = false
  clearTimeout(timer)
}
</script>
```

Example 3 (vue):
```vue
<template>
  <el-button @click="visible = true">
    Open Drawer with customized header
  </el-button>
  <el-drawer v-model="visible" :show-close="false">
    <template #header="{ close, titleId, titleClass }">
      <h4 :id="titleId" :class="titleClass">This is a custom header!</h4>
      <el-button type="danger" @click="close">
        <el-icon class="el-icon--left"><CircleCloseFilled /></el-icon>
        Close
      </el-button>
    </template>
    This is drawer content.
  </el-drawer>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { CircleCloseFilled } from '@element-plus/icons-vue'

const visible = ref(false)
</script>
```

Example 4 (vue):
```vue
<template>
  <el-button plain @click="drawerVisible = true">
    Open the modal Drawer
  </el-button>

  <el-drawer v-model="drawerVisible" :modal="false" modal-penetrable>
    <span>It's a modal Drawer</span>
    <template #footer>
      <div class="drawer-footer">
        <el-button @click="drawerVisible = false">Cancel</el-button>
        <el-button type="primary" @click="drawerVisible = false">
          Confirm
        </el-button>
      </div>
    </template>
  </el-drawer>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const drawerVisible = ref(false)
</script>
```

---

## Popconfirm

**URL:** llms-txt#popconfirm

**Contents:**
- Placement
- Basic usage
- Customize
- Trigger event
- API
  - Attributes
  - Events
  - Slots
  - Exposes
- Vue Examples

A simple confirmation dialog of an element click action.

popconfirm has 9 placements.

Popconfirm is similar to Popover. So for some duplicated attributes, please refer to the documentation of Popover.

You can customize Popconfirm like:

Click the button to trigger the event

| Name                               | Description                                                                                         | Type                                                                         | Default        |
| ---------------------------------- | --------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | -------------- |
| title                              | Title                                                                                               | ^[string]                                                                    | —              |
| effect ^(2.11.2)                   | Tooltip theme, built-in theme: `dark` / `light`                                                     | ^[enum]`'dark' \| 'light'` / ^[string]                                       | light          |
| confirm-button-text                | Confirm button text                                                                                 | ^[string]                                                                    | —              |
| cancel-button-text                 | Cancel button text                                                                                  | ^[string]                                                                    | —              |
| confirm-button-type                | Confirm button type                                                                                 | ^[enum]`'primary' \| 'success' \| 'warning' \| 'danger' \| 'info' \| 'text'` | primary        |
| cancel-button-type                 | Cancel button type                                                                                  | ^[enum]`'primary' \| 'success' \| 'warning' \| 'danger' \| 'info' \| 'text'` | text           |
| icon                               | Icon Component                                                                                      | ^[string] / ^[Component]                                                     | QuestionFilled |
| icon-color                         | Icon color                                                                                          | ^[string]                                                                    | #f90           |
| hide-icon                          | is hide Icon                                                                                        | ^[boolean]                                                                   | false          |
| hide-after                         | delay of disappear, in millisecond                                                                  | ^[number]                                                                    | 200            |
| teleported                         | whether popconfirm is teleported to the body                                                        | ^[boolean]                                                                   | true           |
| persistent                         | when popconfirm inactive and `persistent` is `false` , popconfirm will be destroyed                 | ^[boolean]                                                                   | false          |
| width                              | popconfirm width, min width 150px                                                                   | ^[string] / ^[number]                                                        | 150            |
| [tooltip](./tooltip.md#attributes) | Inherits all attributes from Tooltip, except: `popper-class`, `popper-style`, `fallback-placements` | —                                                                            | —              |

| Name    | Description                        | Type                                 |
| ------- | ---------------------------------- | ------------------------------------ |
| confirm | triggers when click confirm button | ^[Function]`(e: MouseEvent) => void` |
| cancel  | triggers when click cancel button  | ^[Function]`(e: MouseEvent) => void` |

| Name             | Description                           | Type                                                                             |
| ---------------- | ------------------------------------- | -------------------------------------------------------------------------------- |
| reference        | HTML element that triggers Popconfirm | —                                                                                |
| actions ^(2.8.1) | content of the Popconfirm footer      | ^[object]`{ confirm: (e: MouseEvent) => void, cancel: (e: MouseEvent) => void }` |

| Name                | Description                  | Type                                        |
| ------------------- | ---------------------------- | ------------------------------------------- |
| popperRef ^(2.10.7) | el-popper component instance | ^[object]`Ref<PopperInstance \| undefined>` |
| hide ^(2.10.7)      | hide popconfirm              | ^[Function]`() => void`                     |

### trigger-event.vue

---
Title: Popover
URL: https://element-plus.org/en-US/component/popover
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-popconfirm title="Are you sure to delete this?">
    <template #reference>
      <el-button>Delete</el-button>
    </template>
  </el-popconfirm>
</template>
```

Example 2 (vue):
```vue
<template>
  <el-popconfirm
    width="220"
    :icon="InfoFilled"
    icon-color="#626AEF"
    title="Are you sure to delete this?"
    @cancel="onCancel"
  >
    <template #reference>
      <el-button>Delete</el-button>
    </template>
    <template #actions="{ confirm, cancel }">
      <el-button size="small" @click="cancel">No!</el-button>
      <el-button
        type="danger"
        size="small"
        :disabled="!clicked"
        @click="confirm"
      >
        Yes?
      </el-button>
    </template>
  </el-popconfirm>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { InfoFilled } from '@element-plus/icons-vue'

const clicked = ref(false)
function onCancel() {
  clicked.value = true
}
</script>
```

Example 3 (vue):
```vue
<template>
  <div class="popconfirm-base-box">
    <div class="row center">
      <el-popconfirm
        class="box-item"
        title="Top Left prompts info"
        placement="top-start"
      >
        <template #reference>
          <el-button>top-start</el-button>
        </template>
      </el-popconfirm>
      <el-popconfirm
        class="box-item"
        title="Top Center prompts info"
        placement="top"
      >
        <template #reference>
          <el-button>top</el-button>
        </template>
      </el-popconfirm>
      <el-popconfirm
        class="box-item"
        title="Top Right prompts info"
        placement="top-end"
      >
        <template #reference>
          <el-button>top-end</el-button>
        </template>
      </el-popconfirm>
    </div>
    <div class="row">
      <el-popconfirm
        class="box-item"
        title="Left Top prompts info"
        placement="left-start"
      >
        <template #reference>
          <el-button>left-start</el-button>
        </template>
      </el-popconfirm>
      <el-popconfirm
        class="box-item"
        title="Right Top prompts info"
        placement="right-start"
      >
        <template #reference>
          <el-button>right-start</el-button>
        </template>
      </el-popconfirm>
    </div>
    <div class="row">
      <el-popconfirm
        class="box-item"
        title="Left Center prompts info"
        placement="left"
      >
        <template #reference>
          <el-button class="mt-3 mb-3">left</el-button>
        </template>
      </el-popconfirm>
      <el-popconfirm
        class="box-item"
        title="Right Center prompts info"
        placement="right"
      >
        <template #reference>
          <el-button>right</el-button>
        </template>
      </el-popconfirm>
    </div>
    <div class="row">
      <el-popconfirm
        class="box-item"
        title="Left Bottom prompts info"
        placement="left-end"
      >
        <template #reference>
          <el-button>left-end</el-button>
        </template>
      </el-popconfirm>
      <el-popconfirm
        class="box-item"
        title="Right Bottom prompts info"
        placement="right-end"
      >
        <template #reference>
          <el-button>right-end</el-button>
        </template>
      </el-popconfirm>
    </div>
    <div class="row center">
      <el-popconfirm
        class="box-item"
        title="Bottom Left prompts info"
        placement="bottom-start"
      >
        <template #reference> <el-button>bottom-start</el-button></template>
      </el-popconfirm>
      <el-popconfirm
        class="box-item"
        title="Bottom Center prompts info"
        placement="bottom"
      >
        <template #reference> <el-button>bottom</el-button></template>
      </el-popconfirm>
      <el-popconfirm
        class="box-item"
        title="Bottom Right prompts info"
        placement="bottom-end"
      >
        <template #reference>
          <el-button>bottom-end</el-button>
        </template>
      </el-popconfirm>
    </div>
  </div>
</template>

<style>
.popconfirm-base-box {
  width: 600px;
}

.popconfirm-base-box .row {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.popconfirm-base-box .center {
  justify-content: center;
}

.popconfirm-base-box .box-item {
  width: 110px;
  margin-top: 10px;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <el-popconfirm
    confirm-button-text="Yes"
    cancel-button-text="No"
    :icon="InfoFilled"
    icon-color="#626AEF"
    title="Are you sure to delete this?"
    @confirm="confirmEvent"
    @cancel="cancelEvent"
  >
    <template #reference>
      <el-button>Delete</el-button>
    </template>
  </el-popconfirm>
</template>

<script setup lang="ts">
import { InfoFilled } from '@element-plus/icons-vue'

const confirmEvent = () => {
  console.log('confirm!')
}
const cancelEvent = () => {
  console.log('cancel!')
}
</script>
```

---

## Dialog

**URL:** llms-txt#dialog

**Contents:**
- Basic usage
- Customized Content
- Customized Header
- Nested Dialog
- Centered content
- Align Center dialog
- Destroy on Close
- Draggable Dialog
- Fullscreen
- Modal

Informs users while preserving the current page state.

Dialog pops up a dialog box, and it's quite customizable.

## Customized Content

The content of Dialog can be anything, even a table or a form. This example shows how to use Element Plus Table and Form with Dialog.

The `header` slot can be used to customize the area where the title is displayed. In order to maintain accessibility, use the `title` attribute in addition to using this slot, or use the `titleId` slot property to specify which element should be read out as the dialog title.

If a Dialog is nested in another Dialog, `append-to-body` is required.

Dialog's content can be centered.

## Align Center dialog

Open dialog from the center of the screen.

When this is feature is enabled, the content under default slot will be destroyed with a `v-if` directive. Enable this when you have perf concerns.

Try to drag the `header` part.

Set the `fullscreen` attribute to open fullscreen dialog.

Setting `modal` to `false` will hide modal (overlay) of dialog.

Starting from version ^(2.10.5), `modal-penetrable` attribute is added, which can be penetrable.

## Custom Animation ^(2.10.5)

Customize dialog animation through the `transition` attribute, which accepts either:

- ​Transition name​​ (string)

- ​​Vue transition configuration​​ (object)

Open developer console (ctrl + shift + J), to see order of events.

| Name                       | Description                                                                                                                    | Type                                   | Default     |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------- | ----------- |
| model-value / v-model      | visibility of Dialog                                                                                                           | ^[boolean]                             | false       |
| title                      | title of Dialog. Can also be passed with a named slot (see the following table)                                                | ^[string]                              | ''          |
| width                      | width of Dialog, default is 50%                                                                                                | ^[string] / ^[number]                  | ''          |
| fullscreen                 | whether the Dialog takes up full screen                                                                                        | ^[boolean]                             | false       |
| top                        | value for `margin-top` of Dialog CSS, default is 15vh                                                                          | ^[string]                              | ''          |
| modal                      | whether a mask is displayed                                                                                                    | ^[boolean]                             | true        |
| modal-penetrable ^(2.10.5) | whether the mask is penetrable. The modal attribute must be `false`.                                                           | ^[boolean]                             | false       |
| modal-class                | custom class names for mask                                                                                                    | ^[string]                              | —           |
| header-class ^(2.9.3)      | custom class names for header wrapper                                                                                          | ^[string]                              | —           |
| body-class ^(2.9.3)        | custom class names for body wrapper                                                                                            | ^[string]                              | —           |
| footer-class ^(2.9.3)      | custom class names for footer wrapper                                                                                          | ^[string]                              | —           |
| append-to-body             | whether to append Dialog itself to body. A nested Dialog should have this attribute set to `true`                              | ^[boolean]                             | false       |
| append-to ^(2.4.3)         | which element the Dialog appends to. Will override `append-to-body`                                                            | ^[CSSSelector] / ^[HTMLElement]        | body        |
| lock-scroll                | whether scroll of body is disabled while Dialog is displayed                                                                   | ^[boolean]                             | true        |
| open-delay                 | the Time(milliseconds) before open                                                                                             | ^[number]                              | 0           |
| close-delay                | the Time(milliseconds) before close                                                                                            | ^[number]                              | 0           |
| close-on-click-modal       | whether the Dialog can be closed by clicking the mask                                                                          | ^[boolean]                             | true        |
| close-on-press-escape      | whether the Dialog can be closed by pressing ESC                                                                               | ^[boolean]                             | true        |
| show-close                 | whether to show a close button                                                                                                 | ^[boolean]                             | true        |
| before-close               | callback before Dialog closes, and it will prevent Dialog from closing, use done to close the dialog                           | ^[Function]`(done: DoneFn) => void`    | —           |
| draggable                  | enable dragging feature for Dialog                                                                                             | ^[boolean]                             | false       |
| overflow ^(2.5.4)          | draggable Dialog can overflow the viewport                                                                                     | ^[boolean]                             | false       |
| center                     | whether to align the header and footer in center                                                                               | ^[boolean]                             | false       |
| align-center ^(2.2.16)     | whether to align the dialog both horizontally and vertically                                                                   | ^[boolean]                             | false       |
| destroy-on-close           | destroy elements in Dialog when closed                                                                                         | ^[boolean]                             | false       |
| close-icon                 | custom close icon, default is Close                                                                                            | ^[string] / ^[Component]               | —           |
| z-index                    | same as z-index in native CSS, z-order of dialog                                                                               | ^[number]                              | —           |
| header-aria-level ^(a11y)  | header's `aria-level` attribute                                                                                                | ^[string]                              | 2           |
| transition ^(2.10.5)       | custom transition configuration for dialog animation. Can be a string (transition name) or an object with Vue transition props | ^[string] / ^[object]`TransitionProps` | dialog-fade |
| custom-class ^(deprecated) | custom class names for Dialog                                                                                                  | ^[string]                              | ''          |

| Name                | Description                                                                                           |
| ------------------- | ----------------------------------------------------------------------------------------------------- |
| default             | default content of Dialog                                                                             |
| header              | content of the Dialog header; Replacing this removes the title, but does not remove the close button. |
| footer              | content of the Dialog footer                                                                          |
| title ^(deprecated) | works the same as the header slot. Use that instead.                                                  |

| Name             | Description                                      | Type                    |
| ---------------- | ------------------------------------------------ | ----------------------- |
| open             | triggers when the Dialog opens                   | ^[Function]`() => void` |
| opened           | triggers when the Dialog opening animation ends  | ^[Function]`() => void` |
| close            | triggers when the Dialog closes                  | ^[Function]`() => void` |
| closed           | triggers when the Dialog closing animation ends  | ^[Function]`() => void` |
| open-auto-focus  | triggers after Dialog opens and content focused  | ^[Function]`() => void` |
| close-auto-focus | triggers after Dialog closed and content focused | ^[Function]`() => void` |

| Name                   | Description    | Type                    |
| ---------------------- | -------------- | ----------------------- |
| resetPosition ^(2.8.1) | reset position | ^[Function]`() => void` |
| handleClose ^(2.9.8)   | close dialog   | ^[Function]`() => void` |

#### Using dialog in SFC, the scope style does not take effect

Typical issue: [#10515](https://github.com/element-plus/element-plus/issues/10515)

PS: Since the dialog is rendered using `Teleport`, the style of the root node is recommended to be written globally.

#### When the dialog is displayed and hidden, there is a situation where the page elements are displaced back and forth

Typical issue: [#10481](https://github.com/element-plus/element-plus/issues/10481)

PS: It is recommended to place the scroll area inside a vue mounted node, e.g. `<div id="app" />`, and use the `overflow: hidden` style for the body.

### centered-content.vue

### custom-animation.vue

### customization-content.vue

### customization-header.vue

### destroy-on-close.vue

### draggable-dialog.vue

### nested-dialog.vue

---
Title: Divider
URL: https://element-plus.org/en-US/component/divider
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-button plain @click="centerDialogVisible = true">
    Click to open the Dialog
  </el-button>

  <el-dialog
    v-model="centerDialogVisible"
    title="Warning"
    width="500"
    align-center
  >
    <span>Open the dialog from the center from the screen</span>
    <template #footer>
      <div class="dialog-footer">
        <el-button @click="centerDialogVisible = false">Cancel</el-button>
        <el-button type="primary" @click="centerDialogVisible = false">
          Confirm
        </el-button>
      </div>
    </template>
  </el-dialog>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const centerDialogVisible = ref(false)
</script>
```

Example 2 (vue):
```vue
<template>
  <el-button plain @click="dialogVisible = true">
    Click to open the Dialog
  </el-button>

  <el-dialog
    v-model="dialogVisible"
    title="Tips"
    width="500"
    :before-close="handleClose"
  >
    <span>This is a message</span>
    <template #footer>
      <div class="dialog-footer">
        <el-button @click="dialogVisible = false">Cancel</el-button>
        <el-button type="primary" @click="dialogVisible = false">
          Confirm
        </el-button>
      </div>
    </template>
  </el-dialog>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { ElMessageBox } from 'element-plus'

const dialogVisible = ref(false)

const handleClose = (done: () => void) => {
  ElMessageBox.confirm('Are you sure to close this dialog?')
    .then(() => {
      done()
    })
    .catch(() => {
      // catch error
    })
}
</script>
```

Example 3 (vue):
```vue
<template>
  <el-button plain @click="centerDialogVisible = true">
    Click to open the Dialog
  </el-button>

  <el-dialog v-model="centerDialogVisible" title="Warning" width="500" center>
    <span>
      It should be noted that the content will not be aligned in center by
      default
    </span>
    <template #footer>
      <div class="dialog-footer">
        <el-button @click="centerDialogVisible = false">Cancel</el-button>
        <el-button type="primary" @click="centerDialogVisible = false">
          Confirm
        </el-button>
      </div>
    </template>
  </el-dialog>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const centerDialogVisible = ref(false)
</script>
```

Example 4 (vue):
```vue
<template>
  <div>
    <el-button plain @click="openDialog('fade')"> Default </el-button>
    <el-button plain @click="openDialog('scale')"> Scale </el-button>
    <el-button plain @click="openDialog('slide')"> Slide </el-button>
    <el-button plain @click="openDialog('bounce')"> Bounce </el-button>
    <el-button plain @click="openDialogWithObject"> Object Config </el-button>
  </div>

  <el-dialog
    v-model="dialogVisible"
    class="custom-transition-dialog"
    :title="`${currentAnimation} Animation Dialog`"
    width="30%"
    :transition="transitionConfig"
  >
    <div>
      <p>
        Current animation: <strong>{{ currentAnimation }}</strong>
      </p>
      <p>
        This dialog demonstrates the {{ currentAnimation }} animation effect.
      </p>
      <p v-if="isObjectConfig">
        <strong>Using object configuration:</strong><br />
        <code>{{ JSON.stringify(transitionConfig, null, 2) }}</code>
      </p>
    </div>
    <template #footer>
      <el-button @click="dialogVisible = false">Cancel</el-button>
      <el-button type="primary" @click="dialogVisible = false">
        Confirm
      </el-button>
    </template>
  </el-dialog>
</template>

<script lang="ts" setup>
import { computed, ref } from 'vue'

import type { DialogTransition } from 'element-plus'

const dialogVisible = ref(false)
const currentAnimation = ref('fade')
const isObjectConfig = ref(false)

const transitionConfig = computed<DialogTransition>(() => {
  if (isObjectConfig.value) {
    return {
      name: 'dialog-custom-object',
      appear: true,
      mode: 'out-in',
      duration: 500,
    }
  }
  return `dialog-${currentAnimation.value}`
})

const openDialog = (type: string) => {
  currentAnimation.value = type
  isObjectConfig.value = false
  dialogVisible.value = true
}

const openDialogWithObject = () => {
  currentAnimation.value = 'object-config'
  isObjectConfig.value = true
  dialogVisible.value = true
}
</script>

<style scoped>
code {
  background: var(--el-bg-color-page);
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 12px;
  display: block;
  margin-top: 8px;
}
</style>

<style>
/* Scale Animation */
.dialog-scale-enter-active,
.dialog-scale-leave-active,
.dialog-scale-enter-active .el-dialog,
.dialog-scale-leave-active .el-dialog {
  transition: all 0.2s cubic-bezier(0.645, 0.045, 0.355, 1);
}

.dialog-scale-enter-from,
.dialog-scale-leave-to {
  opacity: 0;
}

.dialog-scale-enter-from .el-dialog,
.dialog-scale-leave-to .el-dialog {
  transform: scale(0.5);
  opacity: 0;
}

/* Slide Animation */
.dialog-slide-enter-active,
.dialog-slide-leave-active,
.dialog-slide-enter-active .el-dialog,
.dialog-slide-leave-active .el-dialog {
  transition: all 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.dialog-slide-enter-from,
.dialog-slide-leave-to {
  opacity: 0;
}

.dialog-slide-enter-from .el-dialog,
.dialog-slide-leave-to .el-dialog {
  transform: translateY(-100px);
  opacity: 0;
}

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

/* Object Configuration Animation */
.dialog-custom-object-enter-active,
.dialog-custom-object-leave-active,
.dialog-custom-object-enter-active .el-dialog,
.dialog-custom-object-leave-active .el-dialog {
  transition: all 0.5s cubic-bezier(0.25, 0.8, 0.25, 1);
}

.dialog-custom-object-enter-from,
.dialog-custom-object-leave-to {
  opacity: 0;
}

.dialog-custom-object-enter-from .el-dialog,
.dialog-custom-object-leave-to .el-dialog {
  transform: rotate(180deg) scale(0.5);
  opacity: 0;
}
</style>
```

---

## Tooltip

**URL:** llms-txt#tooltip

**Contents:**
- Basic usage
- Theme
- More Content
- Advanced usage
- HTML as content
- Virtual triggering
- Singleton
- Controlled
- Animations
- Use the `append-to`

Display prompt information for mouse hover.

Tooltip has 9 placements.

Tooltip has two built-in themes: `dark` and `light`.

Display multiple lines of text and set their format.

In addition to basic usages, there are some attributes that allow you to customize your own:

`transition` attribute allows you to customize the animation in which the tooltip shows or hides, and the default value is el-fade-in-linear.

`disabled` attribute allows you to disable `tooltip`. You just need set it to `true`.

In fact, Tooltip is an extension based on [ElPopper](https://github.com/element-plus/element-plus/tree/dev/packages/components/popper), you can use any attribute that are allowed in ElPopper.

The content attribute can be set to HTML string.

## Virtual triggering

Sometimes we want to render the tooltip on some other trigger element,
we can separate the trigger and the content.

Tooltip can also be singleton, which means you can have multiple trigger with only one tooltip instance, this function is implemented based on `Virtual triggering`

Tooltip can be controlled by the parent component, by using `:visible` you can implement two way binding.

Tooltip can be customized animated, you can set the desired animation use `transition`.

## Use the `append-to`

You must wait for the DOM to be mounted before using `targetElement`.

| Name                      | Description                                                                                                                                                                           | Type                                                                                                                                                                        | Default           |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| append-to                 | which element the tooltip CONTENT appends to                                                                                                                                          | ^[CSSSelector] / ^[HTMLElement]                                                                                                                                             | —                 |
| effect                    | Tooltip theme, built-in theme: `dark` / `light`                                                                                                                                       | ^[enum]`'dark' \| 'light'`                                                                                                                                                  | dark              |
| content                   | display content, can be overridden by `slot#content`                                                                                                                                  | ^[string]                                                                                                                                                                   | ''                |
| raw-content               | whether `content` is treated as HTML string                                                                                                                                           | ^[boolean]                                                                                                                                                                  | false             |
| placement                 | position of Tooltip                                                                                                                                                                   | ^[enum]`'top' \| 'top-start' \| 'top-end' \| 'bottom' \| 'bottom-start' \| 'bottom-end' \| 'left' \| 'left-start' \| 'left-end' \| 'right' \| 'right-start' \| 'right-end'` | bottom            |
| fallback-placements       | list of possible positions for Tooltip [popper.js](https://popper.js.org/docs/v2/modifiers/flip/#fallbackplacements)                                                                  | ^[array]`Placement[]`                                                                                                                                                       | —                 |
| visible / v-model:visible | visibility of Tooltip                                                                                                                                                                 | ^[boolean]                                                                                                                                                                  | —                 |
| disabled                  | whether Tooltip is disabled                                                                                                                                                           | ^[boolean]                                                                                                                                                                  | —                 |
| offset                    | offset of the Tooltip                                                                                                                                                                 | ^[number]                                                                                                                                                                   | 12                |
| transition                | animation name                                                                                                                                                                        | ^[string]                                                                                                                                                                   | —                 |
| popper-options            | [popper.js](https://popper.js.org/docs/v2/) parameters                                                                                                                                | ^[object]refer to [popper.js](https://popper.js.org/docs/v2/) doc                                                                                                           | {}                |
| arrow-offset ^(2.9.10)    | Controls the offset (padding) of the tooltip’s arrow relative to the popper.                                                                                                          | ^[number]                                                                                                                                                                   | 5                 |
| show-after                | delay of appearance, in millisecond, not valid in controlled mode                                                                                                                     | ^[number]                                                                                                                                                                   | 0                 |
| show-arrow                | whether the tooltip content has an arrow                                                                                                                                              | ^[boolean]                                                                                                                                                                  | true              |
| hide-after                | delay of disappear, in millisecond, not valid in controlled mode                                                                                                                      | ^[number]                                                                                                                                                                   | 200               |
| auto-close                | timeout in milliseconds to hide tooltip, not valid in controlled mode                                                                                                                 | ^[number]                                                                                                                                                                   | 0                 |
| popper-class              | custom class name for Tooltip's popper                                                                                                                                                | ^[string]                                                                                                                                                                   | —                 |
| popper-style              | custom style for Tooltip's popper                                                                                                                                                     | ^[string] / ^[object]                                                                                                                                                       | —                 |
| enterable                 | whether the mouse can enter the tooltip                                                                                                                                               | ^[boolean]                                                                                                                                                                  | true              |
| teleported                | whether tooltip content is teleported, if `true` it will be teleported to where `append-to` sets                                                                                      | ^[boolean]                                                                                                                                                                  | true              |
| trigger                   | How should the tooltip be triggered (to show), not valid in controlled mode                                                                                                           | ^[enum]`'hover' \| 'click' \| 'focus' \| 'contextmenu'` / ^[array]`Array<'click' \| 'focus' \| 'hover' \| 'contextmenu'>`                                                   | hover             |
| virtual-triggering        | Indicates whether virtual triggering is enabled                                                                                                                                       | ^[boolean]                                                                                                                                                                  | —                 |
| virtual-ref               | Indicates the reference element to which the tooltip is attached                                                                                                                      | ^[HTMLElement]                                                                                                                                                              | —                 |
| trigger-keys              | When you click the mouse to focus on the trigger element, you can define a set of keyboard codes to control the display of tooltip through the keyboard, not valid in controlled mode | ^[Array]                                                                                                                                                                    | ['Enter','Space'] |
| persistent                | when tooltip inactive and `persistent` is `false` , tooltip will be destroyed                                                                                                         | ^[boolean]                                                                                                                                                                  | —                 |
| aria-label ^(a11y)        | same as `aria-label`                                                                                                                                                                  | ^[string]                                                                                                                                                                   | —                 |
| focus-on-target ^(2.11.2) | when triggering tooltips through hover, whether to focus the trigger element, which improves accessibility                                                                            | ^[boolean]                                                                                                                                                                  | false             |

| Name        | Description                                                           | Type                                 |
| ----------- | --------------------------------------------------------------------- | ------------------------------------ |
| before-show | Triggers before tooltip is shown. Passes trigger reason as argument.  | ^[Function]`(event?: Event) => void` |
| show        | Triggers when tooltip is shown. Passes trigger reason as argument.    | ^[Function]`(event?: Event) => void` |
| before-hide | Triggers before tooltip is hidden. Passes trigger reason as argument. | ^[Function]`(event?: Event) => void` |
| hide        | Triggers when tooltip is hidden. Passes trigger reason as argument.   | ^[Function]`(event?: Event) => void` |

| Name    | Description                                                                    |
| ------- | ------------------------------------------------------------------------------ |
| default | Tooltip triggering & reference element, only a single root element is accepted |
| content | customize content                                                              |

| Name                 | Description                                                       | Type                                                |
| -------------------- | ----------------------------------------------------------------- | --------------------------------------------------- |
| popperRef            | el-popper component instance                                      | ^[object]`Ref<PopperInstance \| undefined>`         |
| contentRef           | el-tooltip-content component instance                             | ^[object]`Ref<TooltipContentInstance \| undefined>` |
| isFocusInsideContent | validate current focus event is trigger inside el-tooltip-content | ^[Function]`() => boolean \| undefined`             |
| updatePopper         | update el-popper component instance                               | ^[Function]`() => void`                             |
| onOpen               | expose onOpen function to mange el-tooltip open state             | ^[Function]`(event?: Event \| undefined) => void`   |
| onClose              | expose onClose function to mange el-tooltip open state            | ^[Function]`(event?: Event \| undefined) => void`   |
| hide                 | expose hide function                                              | ^[Function]`(event?: Event \| undefined) => void`   |

#### How to allow spaces in the input box when tooltip is nested?

Typical issue: [#20907](https://github.com/element-plus/element-plus/issues/20907)

### advanced-usage.vue

### virtual-trigger.vue

---
Title: Tour
URL: https://element-plus.org/en-US/component/tour
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-tooltip content="tooltip content" placement="top" :trigger-keys="[]">
    <el-input v-model="value" placeholder="" />
  </el-tooltip>
</template>
```

Example 2 (vue):
```vue
<template>
  <el-tooltip
    :disabled="disabled"
    content="click to close tooltip function"
    placement="bottom"
    effect="light"
  >
    <el-button @click="disabled = !disabled">
      click to {{ disabled ? 'active' : 'close' }} tooltip function
    </el-button>
  </el-tooltip>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const disabled = ref(false)
</script>
```

Example 3 (vue):
```vue
<template>
  <el-tooltip content="I am an el-tooltip" transition="slide-fade">
    <el-button>trigger me</el-button>
  </el-tooltip>
</template>

<script lang="ts" setup></script>

<style>
.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(120px);
  opacity: 0;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <el-tooltip
    :append-to="targetElement"
    trigger="click"
    content="Append to .target"
    placement="top"
  >
    <el-button class="target">Click to open tooltip</el-button>
  </el-tooltip>
</template>

<script lang="ts" setup>
import { onMounted, ref } from 'vue'

const targetElement = ref('')

onMounted(() => {
  targetElement.value = '.target'
})
</script>

<style scoped>
.target {
  position: relative;
  margin-top: 20px;
}
</style>
```

---

## Notification

**URL:** llms-txt#notification

**Contents:**
- Basic usage
- With types
- Custom position
- With offset
- Use HTML string
- Message using functions ^(2.9.0)
- Hide close button
- Global method
- Local import
- App context inheritance <el-tag>> 2.0.4</el-tag>

Displays a global notification message at a corner of the page.

We provide four types: success, warning, info and error.

Notification can emerge from any corner you like.

Customize Notification's offset from the edge of the screen.

`message` supports HTML string.

## Message using functions ^(2.9.0)

`message` can be VNode.

After ^(2.9.0), `message` supports a function whose return value is a VNode.

It is possible to hide the close button

Element Plus has added a global method `$notify` for `app.config.globalProperties`. So in a vue instance you can call `Notification` like what we did in this page.

In this case you should call `ElNotification(options)`. We have also registered methods for different types, e.g. `ElNotification.success(options)`. You can call `ElNotification.closeAll()` to manually close all the instances. In ^(2.10.5) you can manually update the offsets of all instances in a specific direction by calling `ElNotification.updateOffsets(position)`.

## App context inheritance <el-tag>> 2.0.4</el-tag>

Now notification accepts a `context` as second parameter of the message constructor which allows you to inject current app's context to notification which allows you to inherit all the properties of the app.

You can use it like this:

| Name                     | Description                                                                                                        | Type                                                                             | Default   |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------- | --------- |
| title                    | title                                                                                                              | ^[string]                                                                        | ''        |
| message                  | description text                                                                                                   | ^[string] / ^[VNode] / ^[Function]`() => VNode`                                  | ''        |
| dangerouslyUseHTMLString | whether `message` is treated as HTML string                                                                        | ^[boolean]                                                                       | false     |
| type                     | notification type                                                                                                  | ^[enum]`'primary' (2.9.11) \| 'success' \| 'warning' \| 'info' \| 'error' \| ''` | ''        |
| icon                     | custom icon component. It will be overridden by `type`                                                             | ^[string] / ^[Component]                                                         | —         |
| customClass              | custom class name for Notification                                                                                 | ^[string]                                                                        | ''        |
| duration                 | duration before close. It will not automatically close if set 0                                                    | ^[number]                                                                        | 4500      |
| position                 | custom position                                                                                                    | ^[enum]`'top-right' \| 'top-left' \| 'bottom-right' \| 'bottom-left'`            | top-right |
| showClose                | whether to show a close button                                                                                     | ^[boolean]                                                                       | true      |
| onClose                  | callback function when closed                                                                                      | ^[Function]`() => void`                                                          | —         |
| onClick                  | callback function when notification clicked                                                                        | ^[Function]`() => void`                                                          | —         |
| offset                   | offset from the top edge of the screen. Every Notification instance of the same moment should have the same offset | ^[number]                                                                        | 0         |
| appendTo                 | set the root element for the notification, default to `document.body`                                              | ^[CSSSelector] / ^[HTMLElement]                                                  | —         |
| zIndex                   | initial zIndex                                                                                                     | ^[number]                                                                        | 0         |
| closeIcon ^(2.9.8)       | custom close icon                                                                                                  | ^[string] / ^[Component]                                                         | Close     |

`Notification` and `this.$notify` returns the current Notification instance. To manually close the instance, you can call `close` on it.

| Name  | Description            | Type                    |
| ----- | ---------------------- | ----------------------- |
| close | close the Notification | ^[Function]`() => void` |

### different-types.vue

---
Title: Overview
URL: https://element-plus.org/en-US/component/overview
---

**Examples:**

Example 1 (javascript):
```javascript
import { ElNotification } from 'element-plus'
import { CloseBold } from '@element-plus/icons-vue'

ElNotification({
  title: 'Title',
  message: 'This is a message',
  closeIcon: CloseBold,
})
```

Example 2 (ts):
```ts
import { getCurrentInstance } from 'vue'
import { ElNotification } from 'element-plus'

// in your setup method
const { appContext } = getCurrentInstance()!
ElNotification({}, appContext)
```

Example 3 (vue):
```vue
<template>
  <div class="flex flex-wrap gap-1">
    <el-button class="!ml-0" plain @click="open1">
      Closes automatically
    </el-button>
    <el-button class="!ml-0" plain @click="open2">
      Won't close automatically
    </el-button>
  </div>
</template>

<script lang="ts" setup>
import { h } from 'vue'
import { ElNotification } from 'element-plus'

const open1 = () => {
  ElNotification({
    title: 'Title',
    message: h('i', { style: 'color: teal' }, 'This is a reminder'),
  })
}

const open2 = () => {
  ElNotification({
    title: 'Prompt',
    message: 'This is a message that does not automatically close',
    duration: 0,
  })
}
</script>
```

Example 4 (vue):
```vue
<template>
  <div class="flex flex-wrap gap-1">
    <el-button class="!ml-0" plain @click="open5">Primary</el-button>
    <el-button class="!ml-0" plain @click="open1">Success</el-button>
    <el-button class="!ml-0" plain @click="open2">Warning</el-button>
    <el-button class="!ml-0" plain @click="open3">Info</el-button>
    <el-button class="!ml-0" plain @click="open4">Error</el-button>
  </div>
</template>

<script lang="ts" setup>
import { ElNotification } from 'element-plus'

const open1 = () => {
  ElNotification({
    title: 'Success',
    message: 'This is a success message',
    type: 'success',
  })
}

const open2 = () => {
  ElNotification({
    title: 'Warning',
    message: 'This is a warning message',
    type: 'warning',
  })
}

const open3 = () => {
  ElNotification({
    title: 'Info',
    message: 'This is an info message',
    type: 'info',
  })
}

const open4 = () => {
  ElNotification({
    title: 'Error',
    message: 'This is an error message',
    type: 'error',
  })
}

const open5 = () => {
  ElNotification({
    title: 'Primary',
    message: 'This is a primary message',
    type: 'primary',
  })
}
</script>
```

---

## Popover

**URL:** llms-txt#popover

**Contents:**
- Placement
- Basic usage
- Virtual triggering
- Rich content
- Nested operation
- Directive
- API
  - Attributes
  - Slots
  - Events

Popover has 9 placements.

Popover is built with `ElTooltip`. So for some duplicated attributes, please refer to the documentation of Tooltip.

## Virtual triggering

Like Tooltip, Popover can be triggered by virtual elements, if your use case includes separate the triggering element and the content element, you should definitely use the mechanism, normally we use `#reference` to place our triggering element, with `triggering-element` API you can set your triggering element anywhere you like, but notice that the triggering element should be an element that accepts `mouse` and `keyboard` event.

Other components/elements can be nested in popover. Following is an example of nested table.

Of course, you can nest other operations. It's more light-weight than using a dialog.

You can still using popover in directive way but this is **not recommended** anymore since this makes your application
complicated, you may refer to the virtual triggering for more information.

| Name                               | Description                                                                                                                                                                           | Type                                                                                                                                                                        | Default                                                                    |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| trigger                            | how the popover is triggered, not valid in controlled mode                                                                                                                            | ^[enum]`'click' \| 'focus' \| 'hover' \| 'contextmenu'` / ^[array]`Array<'click' \| 'focus' \| 'hover' \| 'contextmenu'>`                                                   | hover                                                                      |
| trigger-keys ^(2.9.8)              | When you click the mouse to focus on the trigger element, you can define a set of keyboard codes to control the display of popover through the keyboard, not valid in controlled mode | ^[Array]                                                                                                                                                                    | ['Enter','Space']                                                          |
| title                              | popover title                                                                                                                                                                         | ^[string]                                                                                                                                                                   | —                                                                          |
| effect                             | Tooltip theme, built-in theme: `dark` / `light`                                                                                                                                       | ^[enum]`'dark' \| 'light'` / ^[string]                                                                                                                                      | light                                                                      |
| content                            | popover content, can be replaced with a default `slot`                                                                                                                                | ^[string]                                                                                                                                                                   | ''                                                                         |
| width                              | popover width                                                                                                                                                                         | ^[string] / ^[number]                                                                                                                                                       | 150                                                                        |
| placement                          | popover placement                                                                                                                                                                     | ^[enum]`'top' \| 'top-start' \| 'top-end' \| 'bottom' \| 'bottom-start' \| 'bottom-end' \| 'left' \| 'left-start' \| 'left-end' \| 'right' \| 'right-start' \| 'right-end'` | bottom                                                                     |
| disabled                           | whether Popover is disabled                                                                                                                                                           | ^[boolean]                                                                                                                                                                  | false                                                                      |
| visible / v-model:visible          | whether popover is visible                                                                                                                                                            | ^[boolean] / ^[null]                                                                                                                                                        | null                                                                       |
| offset                             | popover offset, `Popover` is built with `Tooltip`, offset of `Popover` is `undefined`, but offset of `Tooltip` is 12                                                                  | ^[number]                                                                                                                                                                   | undefined                                                                  |
| transition                         | popover transition animation, the default is el-fade-in-linear                                                                                                                        | ^[string]                                                                                                                                                                   | —                                                                          |
| show-arrow                         | whether a tooltip arrow is displayed or not. For more info, please refer to [ElPopper](https://github.com/element-plus/element-plus/tree/dev/packages/components/popper)              | ^[boolean]                                                                                                                                                                  | true                                                                       |
| popper-options                     | parameters for [popper.js](https://popper.js.org/docs/v2/)                                                                                                                            | ^[object]                                                                                                                                                                   | `{modifiers: [{name: 'computeStyles',options: {gpuAcceleration: false}}]}` |
| popper-class                       | custom class name for popover                                                                                                                                                         | ^[string]                                                                                                                                                                   | —                                                                          |
| popper-style                       | custom style for popover                                                                                                                                                              | ^[string] / ^[object]                                                                                                                                                       | —                                                                          |
| show-after                         | delay of appearance, in millisecond, not valid in controlled mode                                                                                                                     | ^[number]                                                                                                                                                                   | 0                                                                          |
| hide-after                         | delay of disappear, in millisecond, not valid in controlled mode                                                                                                                      | ^[number]                                                                                                                                                                   | 200                                                                        |
| auto-close                         | timeout in milliseconds to hide tooltip, not valid in controlled mode                                                                                                                 | ^[number]                                                                                                                                                                   | 0                                                                          |
| tabindex                           | [tabindex](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex) of Popover                                                                                   | ^[number] / ^[string]                                                                                                                                                       | 0                                                                          |
| teleported                         | whether popover dropdown is teleported to the body                                                                                                                                    | ^[boolean]                                                                                                                                                                  | true                                                                       |
| append-to ^(2.9.10)                | which element the popover CONTENT appends to                                                                                                                                          | ^[CSSSelector] / ^[HTMLElement]                                                                                                                                             | body                                                                       |
| persistent                         | when popover inactive and `persistent` is `false` , popover will be destroyed                                                                                                         | ^[boolean]                                                                                                                                                                  | true                                                                       |
| virtual-triggering                 | Indicates whether virtual triggering is enabled                                                                                                                                       | ^[boolean]                                                                                                                                                                  | —                                                                          |
| virtual-ref                        | Indicates the reference element to which the popover is attached                                                                                                                      | ^[HTMLElement]                                                                                                                                                              | —                                                                          |
| [tooltip](./tooltip.md#attributes) | Inherits all attributes from Tooltip                                                                                                                                                  | —                                                                                                                                                                           | —                                                                          |

| Name      | Description                                                                |
| --------- | -------------------------------------------------------------------------- |
| default   | text content of popover                                                    |
| reference | HTML element that triggers popover, only a single root element is accepted |

| Name         | Description                                  | Type                    |
| ------------ | -------------------------------------------- | ----------------------- |
| show         | triggers when popover shows                  | ^[Function]`() => void` |
| before-enter | triggers when the entering transition before | ^[Function]`() => void` |
| after-enter  | triggers when the entering transition ends   | ^[Function]`() => void` |
| hide         | triggers when popover hides                  | ^[Function]`() => void` |
| before-leave | triggers when the leaving transition before  | ^[Function]`() => void` |
| after-leave  | triggers when the leaving transition ends    | ^[Function]`() => void` |

| Name | Description  | Type                    |
| ---- | ------------ | ----------------------- |
| hide | hide popover | ^[Function]`() => void` |

### directive-usage.vue

### nested-information.vue

### nested-operation.vue

### virtual-triggering.vue

---
Title: Progress
URL: https://element-plus.org/en-US/component/progress
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-popover
    placement="top-start"
    title="Title"
    :width="200"
    trigger="hover"
    content="this is content, this is content, this is content"
  >
    <template #reference>
      <el-button class="m-2">Hover to activate</el-button>
    </template>
  </el-popover>

  <el-popover
    placement="bottom"
    title="Title"
    :width="200"
    trigger="click"
    content="this is content, this is content, this is content"
  >
    <template #reference>
      <el-button class="m-2">Click to activate</el-button>
    </template>
  </el-popover>

  <el-popover
    ref="popover"
    placement="right"
    title="Title"
    :width="200"
    trigger="focus"
    content="this is content, this is content, this is content"
  >
    <template #reference>
      <el-button class="m-2">Focus to activate</el-button>
    </template>
  </el-popover>

  <el-popover
    ref="popover"
    title="Title"
    :width="200"
    trigger="contextmenu"
    content="this is content, this is content, this is content"
  >
    <template #reference>
      <el-button class="m-2">contextmenu to activate</el-button>
    </template>
  </el-popover>

  <el-popover
    :visible="visible"
    placement="bottom"
    title="Title"
    :width="200"
    content="this is content, this is content, this is content"
  >
    <template #reference>
      <el-button class="m-2" @click="visible = !visible">
        Manual to activate
      </el-button>
    </template>
  </el-popover>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const visible = ref(false)
</script>

<style scoped>
.el-button + .el-button {
  margin-left: 8px;
}
</style>
```

Example 2 (vue):
```vue
<template>
  <el-button v-popover="popoverRef" v-click-outside="onClickOutside">
    Click me
  </el-button>

  <el-popover
    ref="popoverRef"
    trigger="click"
    title="With title"
    virtual-triggering
    persistent
  >
    <span> Some content </span>
  </el-popover>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { ClickOutside as vClickOutside } from 'element-plus'

import type { PopoverInstance } from 'element-plus'

const popoverRef = ref<PopoverInstance>()
const onClickOutside = () => {
  popoverRef.value?.hide()
}
</script>
```

Example 3 (vue):
```vue
<template>
  <div style="display: flex; align-items: center">
    <el-popover placement="right" :width="400" trigger="click">
      <template #reference>
        <el-button style="margin-right: 16px">Click to activate</el-button>
      </template>
      <el-table :data="gridData">
        <el-table-column width="150" property="date" label="date" />
        <el-table-column width="100" property="name" label="name" />
        <el-table-column width="300" property="address" label="address" />
      </el-table>
    </el-popover>

    <el-popover
      :width="300"
      popper-style="box-shadow: rgb(14 18 22 / 35%) 0px 10px 38px -10px, rgb(14 18 22 / 20%) 0px 10px 20px -15px; padding: 20px;"
    >
      <template #reference>
        <el-avatar src="https://avatars.githubusercontent.com/u/72015883?v=4" />
      </template>
      <template #default>
        <div
          class="demo-rich-conent"
          style="display: flex; gap: 16px; flex-direction: column"
        >
          <el-avatar
            :size="60"
            src="https://avatars.githubusercontent.com/u/72015883?v=4"
            style="margin-bottom: 8px"
          />
          <div>
            <p
              class="demo-rich-content__name"
              style="margin: 0; font-weight: 500"
            >
              Element Plus
            </p>
            <p
              class="demo-rich-content__mention"
              style="margin: 0; font-size: 14px; color: var(--el-color-info)"
            >
              @element-plus
            </p>
          </div>

          <p class="demo-rich-content__desc" style="margin: 0">
            Element Plus, a Vue 3 based component library for developers,
            designers and product managers
          </p>
        </div>
      </template>
    </el-popover>
  </div>
</template>

<script lang="ts" setup>
const gridData = [
  {
    date: '2016-05-02',
    name: 'Jack',
    address: 'New York City',
  },
  {
    date: '2016-05-04',
    name: 'Jack',
    address: 'New York City',
  },
  {
    date: '2016-05-01',
    name: 'Jack',
    address: 'New York City',
  },
  {
    date: '2016-05-03',
    name: 'Jack',
    address: 'New York City',
  },
]
</script>
```

Example 4 (vue):
```vue
<template>
  <el-popover :visible="visible" placement="top" :width="180">
    <p>Are you sure to delete this?</p>
    <div style="text-align: right; margin: 0">
      <el-button size="small" text @click="visible = false">cancel</el-button>
      <el-button size="small" type="primary" @click="visible = false">
        confirm
      </el-button>
    </div>
    <template #reference>
      <el-button @click="visible = true">Delete</el-button>
    </template>
  </el-popover>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const visible = ref(false)
</script>
```

---

## Message

**URL:** llms-txt#message

**Contents:**
- Basic usage
- Types
- Plain ^(2.6.3)
- Closable
- Use HTML string
- Grouping
- Placement ^(2.11.0)
- Global method
- Local import
- App context inheritance ^(2.0.3)

Used to show feedback after an activity. The difference with Notification is that the latter is often used to show a system level passive notification.

Displays at the top by default, and disappears after 3 seconds. You can control the position using the `placement` property.

Used to show the feedback of Success, Warning, Message and Error activities.

Set `plain` to have a plain background.

A close button can be added.

`message` supports HTML string.

merge messages with the same content.

## Placement ^(2.11.0)

Control the position where messages appear. Messages can be displayed at the top (default) or other placements of the viewport.

Element Plus has added a global method `$message` for `app.config.globalProperties`. So in a vue instance you can call `Message` like what we did in this page.

In this case you should call `ElMessage(options)`. We have also registered methods for different types, e.g. `ElMessage.success(options)`. You can call `ElMessage.closeAll()` to manually close all the instances.

## App context inheritance ^(2.0.3)

Now message accepts a `context` as second parameter of the message constructor which allows you to inject current app's context to message which allows you to inherit all the properties of the app.

You can use it like this:

| Name                     | Description                                                                                            | Type                                                                                       | Default |
| ------------------------ | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ | ------- |
| message                  | message text                                                                                           | ^[string] / ^[VNode] / ^[Function]`() => VNode`                                            | ''      |
| type                     | message type                                                                                           | ^[enum]`'primary' (2.9.11) \| 'success' \| 'warning' \| 'info' \| 'error'`                 | info    |
| plain ^(2.6.3)           | whether message is plain                                                                               | ^[boolean]                                                                                 | false   |
| icon                     | custom icon component, overrides `type`                                                                | ^[string] / ^[Component]                                                                   | —       |
| dangerouslyUseHTMLString | whether `message` is treated as HTML string                                                            | ^[boolean]                                                                                 | false   |
| customClass              | custom class name for Message                                                                          | ^[string]                                                                                  | ''      |
| duration                 | display duration, millisecond. If set to 0, it will not turn off automatically                         | ^[number]                                                                                  | 3000    |
| showClose                | whether to show a close button                                                                         | ^[boolean]                                                                                 | false   |
| onClose                  | callback function when closed with the message instance as the parameter                               | ^[Function]`() => void`                                                                    | —       |
| offset                   | set the distance to the viewport edge (top when placement is 'top', bottom when placement is 'bottom') | ^[number]                                                                                  | 16      |
| placement ^(2.11.0)      | message placement position                                                                             | ^[enum]`'top' \| 'top-left' \| 'top-right' \| 'bottom' \| 'bottom-left' \| 'bottom-right'` | top     |
| appendTo                 | set the root element for the message, default to `document.body`                                       | ^[CSSSelector] / ^[HTMLElement]                                                            | —       |
| grouping                 | merge messages with the same content, type of VNode message is not supported                           | ^[boolean]                                                                                 | false   |
| repeatNum                | The number of repetitions, similar to badge, is used as the initial number when used with `grouping`   | ^[number]                                                                                  | 1       |

`Message` and `this.$message` returns the current Message instance. To manually close the instance, you can call `close` on it.

| Name  | Description       | Type                    |
| ----- | ----------------- | ----------------------- |
| close | close the Message | ^[Function]`() => void` |

### different-types.vue

---
Title: Notification
URL: https://element-plus.org/en-US/component/notification
---

**Examples:**

Example 1 (ts):
```ts
import { ElMessage } from 'element-plus'
```

Example 2 (ts):
```ts
import { getCurrentInstance } from 'vue'
import { ElMessage } from 'element-plus'

// in your setup method
const { appContext } = getCurrentInstance()!
ElMessage({}, appContext)
```

Example 3 (vue):
```vue
<template>
  <div class="flex flex-wrap gap-1">
    <el-button class="!ml-0" :plain="true" @click="open">
      Show message
    </el-button>
    <el-button class="!ml-0" :plain="true" @click="openVn">VNode</el-button>
  </div>
</template>

<script lang="ts" setup>
import { h } from 'vue'
import { ElMessage } from 'element-plus'

const open = () => {
  ElMessage('This is a message.')
}

const openVn = () => {
  ElMessage({
    message: h('p', { style: 'line-height: 1; font-size: 14px' }, [
      h('span', null, 'Message can be '),
      h('i', { style: 'color: teal' }, 'VNode'),
    ]),
  })
}
</script>
```

Example 4 (vue):
```vue
<template>
  <div class="flex flex-wrap gap-1">
    <el-button class="!ml-0" :plain="true" @click="open6">Primary</el-button>
    <el-button class="!ml-0" :plain="true" @click="open2">Success</el-button>
    <el-button class="!ml-0" :plain="true" @click="open3">Warning</el-button>
    <el-button class="!ml-0" :plain="true" @click="open1">Info</el-button>
    <el-button class="!ml-0" :plain="true" @click="open4">Error</el-button>
    <el-button class="!ml-0" :plain="true" @click="open5">
      Won't close automatically
    </el-button>
  </div>
</template>

<script lang="ts" setup>
import { ElMessage } from 'element-plus'

const open1 = () => {
  ElMessage({
    showClose: true,
    message: 'This is a info message.',
  })
}
const open2 = () => {
  ElMessage({
    showClose: true,
    message: 'Congrats, this is a success message.',
    type: 'success',
  })
}
const open3 = () => {
  ElMessage({
    showClose: true,
    message: 'Warning, this is a warning message.',
    type: 'warning',
  })
}
const open4 = () => {
  ElMessage({
    showClose: true,
    message: 'Oops, this is a error message.',
    type: 'error',
  })
}
const open5 = () => {
  ElMessage({
    showClose: true,
    message: 'Oops, this is a message that does not automatically close.',
    duration: 0,
  })
}
const open6 = () => {
  ElMessage({
    showClose: true,
    message: 'This is a primary message.',
    type: 'primary',
  })
}
</script>
```

---
