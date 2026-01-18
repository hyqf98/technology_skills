# Element Plus - 导航组件

**页面数：** 7

---

## Affix 固钉

**URL：** llms-txt#affix

**内容概览：**
- 基础用法
- 目标容器
- 固定位置
- API
  - Attributes（属性）
  - Events（事件）
  - Slots（插槽）
  - Exposes（暴露方法）
- Vue 示例
  - basic.vue

### 组件说明
将元素固定在特定的可见区域，常用于固定导航栏、操作按钮等需要保持在视口内的元素。

### 基础用法
Affix 默认固定在页面顶部。

### 目标容器
可以设置 `target` 属性使固钉始终保持在容器内。超出范围时会自动隐藏。

### 固定位置
固钉组件提供两种固定位置：`top`（顶部）和 `bottom`（底部）。

### Attributes（属性）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| offset | 偏移距离 | ^[number] | 0 |
| position | 固钉位置 | ^[enum]`'top' \| 'bottom'` | top |
| target | 目标容器（CSS 选择器） | ^[string] | — |
| z-index | 固钉的 z-index 层级 | ^[number] | 100 |
| teleported ^(2.13.0) | 是否将固钉元素传送到 body，为 `true` 时传送到 `append-to` 指定的位置 | ^[boolean] | false |
| append-to ^(2.13.0) | 固钉元素附加到哪个元素 | ^[CSSSelector] / ^[HTMLElement] | body |

### Events（事件）

| 事件名 | 说明 | 类型 |
| ------ | ---- | ---- |
| change | 固定状态改变时触发 | ^[Function]`(fixed: boolean) => void` |
| scroll | 滚动时触发 | ^[Function]`(value: { scrollTop: number, fixed: boolean }) => void` |

### Slots（插槽）

| 插槽名 | 说明 |
| ------ | ---- |
| default | 自定义默认内容 |

### Exposes（暴露方法）

| 方法名 | 说明 | 类型 |
| ------ | ---- | ---- |
| update | 手动更新固钉状态 | ^[Function]`() => void` |
| updateRoot | 更新根元素信息 | ^[Function]`() => void` |

**使用示例：**

示例 1 (vue) - 基础用法：
```vue
<template>
  <el-affix :offset="120">
    <el-button type="primary">距离顶部 120px</el-button>
  </el-affix>
</template>
```

示例 2 (vue) - 底部固定：
```vue
<template>
  <el-affix position="bottom" :offset="20">
    <el-button type="primary">距离底部 20px</el-button>
  </el-affix>
</template>
```

示例 3 (vue) - 目标容器：
```vue
<template>
  <div class="affix-container">
    <el-affix target=".affix-container" :offset="80">
      <el-button type="primary">目标容器</el-button>
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

## Backtop 回到顶部

**URL：** llms-txt#backtop

**内容概览：**
- 基础用法
- 自定义显示
- API
  - Attributes（属性）
  - Events（事件）
  - Slots（插槽）
- Vue 示例
  - basic.vue
  - custom.vue

### 组件说明
返回页面顶部的按钮，当页面滚动到一定距离后显示。

### 基础用法
向下滚动页面以查看右下角的按钮。

### 自定义显示
显示区域为 40px × 40px。

### Attributes（属性）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| target | 触发滚动的目标对象 | ^[string] | — |
| visibility-height | 滚动高度达到此值时才显示按钮 | ^[number] | 200 |
| right | 距离右侧的距离 | ^[number] | 40 |
| bottom | 距离底部的距离 | ^[number] | 40 |

### Events（事件）

| 事件名 | 说明 | 参数 |
| ------ | ---- | ---- |
| click | 点击时触发 | ^[Function]`(evt: MouseEvent) => void` |

### Slots（插槽）

| 插槽名 | 说明 |
| ------ | ---- |
| default | 自定义默认内容 |

**使用示例：**

示例 1 (vue) - 基础用法：
```vue
<template>
  向下滚动查看右下角的按钮。
  <el-backtop :right="100" :bottom="100" />
</template>
```

示例 2 (vue) - 自定义内容：
```vue
<template>
  向下滚动查看右下角的按钮。
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

## Anchor 锚点

**URL：** llms-txt#anchor

**内容概览：**
- 基础用法
- 水平模式
- 滚动容器
- 锚点链接变更
- 下划线类型
- 固钉模式
- Anchor API
  - Anchor Attributes（锚点属性）
  - Anchor Events（锚点事件）
  - Anchor Exposes（锚点暴露方法）

### 组件说明
通过锚点可以快速找到当前页面上信息内容的位置。

### 基础用法
基本使用方式，点击链接滚动到对应位置。

### 水平模式
水平对齐的锚点。

### 滚动容器
自定义滚动区域，使用 `offset` 属性可以设置锚点滚动偏移量，监听 `link-click` 事件并阻止浏览器默认行为则不会改变历史记录。

### 锚点链接变更
监听锚点链接的变更。

### 下划线类型
设置 `type="underline"` 改为下划线类型。

### 固钉模式
使用固钉组件将锚点固定在页面内。

### Anchor Attributes（锚点属性）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| container | 滚动容器 | `string` \| `HTMLElement` \| `Window ` | — |
| offset | 设置锚点滚动的偏移量 | `number` | 0 |
| bound | 元素开始触发锚点的偏移量 | `number` | 15 |
| duration | 设置容器滚动持续时间，单位毫秒 | `number` | 300 |
| marker | 是否显示标记 | ^[boolean] | true |
| type | 设置 Anchor 类型 | ^[enum]`'default' \| 'underline'` | `default` |
| direction | 设置 Anchor 方向 | ^[enum]`'vertical' \| 'horizontal'` | `vertical` |
| select-scroll-top ^(2.9.2) | 选中链接时是否滚动到顶部 | ^[boolean] | false |

### Anchor Events（锚点事件）

| 事件名 | 说明 | 类型 |
| ------ | ---- | ---- |
| change | 步骤改变时的回调 | ^[Function]`(href: string) => void` |
| click | 用户点击链接时触发 | ^[Function]`(e: MouseEvent, href?: string) => void` |

### Anchor Exposes（锚点暴露方法）

| 方法名 | 说明 | 类型 |
| ------ | ---- | ---- |
| scrollTo | 手动滚动到指定位置 | ^[Function]`(href: string) => void` |

### Anchor Slots（锚点插槽）

| 插槽名 | 说明 |
| ------ | ---- |
| default | AnchorLink 组件列表 |

### AnchorLink Attributes（锚点链接属性）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| title | 锚点链接的文本内容 | `string` | — |
| href | 锚点链接的地址 | `string` | — |

### AnchorLink Slots（锚点链接插槽）

| 插槽名 | 说明 |
| ------ | ---- |
| default | 锚点链接的内容 |
| sub-link | 子链接的插槽 |

**使用示例：**

示例 1 (vue) - 基础用法：
```vue
<template>
  <el-affix :offset="60">
    <el-anchor :offset="70" style="width: 300px">
      <el-anchor-link :href="`#${locale['basic-usage']}`">
        {{ locale['基础用法'] }}
      </el-anchor-link>
      <el-anchor-link :href="`#${locale['horizontal-mode']}`">
        {{ locale['水平模式'] }}
      </el-anchor-link>
      <el-anchor-link :href="`#${locale['scroll-container']}`">
        {{ locale['滚动容器'] }}
      </el-anchor-link>
      <el-anchor-link :href="`#${locale['anchor-api']}`">
        {{ locale['Anchor API'] }}
        <template #sub-link>
          <el-anchor-link :href="`#${locale['anchor-attributes']}`">
            {{ locale['Anchor 属性'] }}
          </el-anchor-link>
          <el-anchor-link :href="`#${locale['anchor-events']}`">
            {{ locale['Anchor 事件'] }}
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

---

## Page Header 页面头部

**URL：** llms-txt#page-header

**内容概览：**
- 完整示例
- 基础用法
- 自定义图标
- 无图标
- 面包屑
- 额外操作区域
- 主要内容
- 结构说明
- API
  - Attributes（属性）

### 组件说明
如果页面路径较简单，建议使用 PageHeader 而不是 Breadcrumb（面包屑）。

### 基础用法
标准页面头部，用于简单场景。

### 自定义图标
默认图标可能不满足需求，可以通过设置 `icon` 属性来自定义图标。

### 无图标
有时页面充满元素，你可能不希望图标显示在页面上，可以将 `icon` 属性设置为 `""` 来移除它。

### 面包屑
页面头部允许通过 `breadcrumb` 插槽添加面包屑，为用户提供路由信息。

### 额外操作区域
头部可以设计得很复杂，可以添加额外的区域，以实现丰富的交互。

### 主要内容
有时我们希望头部显示一些相关内容，可以利用 `default` 插槽来实现。

### 结构说明
组件由以下部分组成。

### Attributes（属性）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| icon | 页面头部的图标组件 | ^[string] / ^[Component] | Back |
| title | 页面头部的主标题，默认为内置无障碍功能的 Back | ^[string] | '' |
| content | 页面头部的内容 | ^[string] | '' |

### Events（事件）

| 事件名 | 说明 | 类型 |
| ------ | ---- | ---- |
| back | 右侧被点击时触发 | ^[Function]`() => void` |

### Slots（插槽）

| 插槽名 | 说明 |
| ------ | ---- |
| icon | 图标内容 |
| title | 标题内容 |
| content | 内容 |
| extra | 额外内容 |
| breadcrumb | 面包屑内容 |
| default | 主要内容 |

**使用示例：**

示例 1 (vue) - 完整结构：
```vue
<template>
  <el-page-header>
    <!-- 第一行 -->
    <template #breadcrumb />
    <!-- 第二行 -->
    <template #icon />
    <template #title />
    <template #content />
    <template #extra />
    <!-- 第二行之后的内容 -->
    <template #default />
  </el-page-header>
</template>
```

示例 2 (vue) - 自定义内容：
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
        <span class="text-large font-600 mr-3"> 标题 </span>
        <span class="text-sm mr-2" style="color: var(--el-text-color-regular)">
          副标题
        </span>
        <el-tag>默认</el-tag>
      </div>
    </template>
    <template #extra>
      <div class="flex items-center">
        <el-button>打印</el-button>
        <el-button type="primary" class="ml-2">编辑</el-button>
      </div>
    </template>
  </el-page-header>
</template>
```

示例 3 (vue) - 基础用法：
```vue
<template>
  <el-page-header @back="goBack">
    <template #content>
      <span class="text-large font-600 mr-3"> 标题 </span>
    </template>
  </el-page-header>
</template>

<script lang="ts" setup>
const goBack = () => {
  console.log('返回')
}
</script>
```

示例 4 (vue) - 面包屑：
```vue
<template>
  <el-page-header>
    <template #breadcrumb>
      <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: './page-header.html' }">
          首页
        </el-breadcrumb-item>
        <el-breadcrumb-item
          ><a href="./page-header.html">路由 1</a></el-breadcrumb-item
        >
        <el-breadcrumb-item>路由 2</el-breadcrumb-item>
      </el-breadcrumb>
    </template>
    <template #content>
      <span class="text-large font-600 mr-3"> 标题 </span>
    </template>
  </el-page-header>
</template>
```

---

## Breadcrumb 面包屑

**URL：** llms-txt#breadcrumb

**内容概览：**
- 基础用法
- 图标分隔符
- Breadcrumb API
  - Breadcrumb Attributes（面包屑属性）
  - Breadcrumb Slots（面包屑插槽）
- BreadcrumbItem API
  - BreadcrumbItem Attributes（面包屑项属性）
  - BreadcrumbItem Slots（面包屑项插槽）
- Vue 示例
  - basic.vue

### 组件说明
显示当前页面的位置，方便浏览器返回导航。

### 基础用法
最简单的用法。

### 图标分隔符
使用图标作为分隔符。

### Breadcrumb Attributes（面包屑属性）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| separator | 分隔符字符 | ^[string] | / |
| separator-icon | 图标分隔符的图标组件 | ^[string] / ^[Component] | — |

### Breadcrumb Slots（面包屑插槽）

| 插槽名 | 说明 | 子组件 |
| ------ | ---- | ---- |
| default | 自定义默认内容 | Breadcrumb Item |

### BreadcrumbItem Attributes（面包屑项属性）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| to | 链接的目标路由，同 `vue-router` 的 `to` | ^[string] / ^[object]`RouteLocationRaw` | '' |
| replace | 如果为 `true`，导航不会留下历史记录 | ^[boolean] | false |

### BreadcrumbItem Slots（面包屑项插槽）

| 插槽名 | 说明 |
| ------ | ---- |
| default | 自定义默认内容 |

**使用示例：**

示例 1 (vue) - 基础用法：
```vue
<template>
  <el-breadcrumb separator="/">
    <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
    <el-breadcrumb-item>
      <a href="/">活动管理</a>
    </el-breadcrumb-item>
    <el-breadcrumb-item>活动列表</el-breadcrumb-item>
    <el-breadcrumb-item>活动详情</el-breadcrumb-item>
  </el-breadcrumb>
</template>
```

示例 2 (vue) - 图标分隔符：
```vue
<template>
  <el-breadcrumb :separator-icon="ArrowRight">
    <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
    <el-breadcrumb-item>活动管理</el-breadcrumb-item>
    <el-breadcrumb-item>活动列表</el-breadcrumb-item>
    <el-breadcrumb-item>活动详情</el-breadcrumb-item>
  </el-breadcrumb>
</template>

<script lang="ts" setup>
import { ArrowRight } from '@element-plus/icons-vue'
</script>
```

---

## Steps 步骤条

**URL：** llms-txt#steps

**内容概览：**
- 基础用法
- 带状态的步骤条
- 居中对齐
- 带描述的步骤条
- 带图标的步骤条
- 垂直步骤条
- 简单步骤条
- Steps API
  - Steps Attributes（步骤条属性）
  - Steps Slots（步骤条插槽）

### 组件说明
引导用户按照流程完成任务的步骤条。步骤可以根据实际应用场景设置，步骤数不能少于 2。

### 基础用法
简单的步骤条。

### 带状态的步骤条
显示每个步骤的状态。

### 居中对齐
标题和描述居中显示。

### 带描述的步骤条
每个步骤都有描述。

### 带图标的步骤条
步骤条可以使用多种自定义图标。

### 垂直步骤条
垂直方向的步骤条。

### 简单步骤条
简单步骤条，其中 `align-center`、`description`、`direction` 和 `space` 将被忽略。

### Steps Attributes（步骤条属性）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| space | 每个步骤的间距，如果省略则自适应。支持百分比。 | ^[number] / ^[string] | '' |
| direction | 显示方向 | ^[enum]`'vertical' \| 'horizontal'` | horizontal |
| active | 当前激活步骤 | ^[number] | 0 |
| process-status | 当前步骤的状态 | ^[enum]`'wait' \| 'process' \| 'finish' \| 'error' \| 'success'` | process |
| finish-status | 结束步骤的状态 | ^[enum]`'wait' \| 'process' \| 'finish' \| 'error' \| 'success'` | finish |
| align-center | 标题和描述居中 | ^[boolean] | — |
| simple | 是否应用简单主题 | ^[boolean] | — |

### Steps Slots（步骤条插槽）

| 插槽名 | 说明 | 子组件 |
| ------ | ---- | ---- |
| default | 自定义默认内容 | Step |

### Step Attributes（步骤属性）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| title | 步骤标题 | ^[string] | '' |
| description | 步骤描述 | ^[string] | '' |
| icon | 步骤自定义图标。也可以通过命名插槽传递 | ^[string] / ^[Component] | — |
| status | 当前状态。如果未配置，将由 Steps 自动设置 | ^[enum]`'' \| 'wait' \| 'process' \| 'finish' \| 'error' \| 'success'` | '' |

### Step Slots（步骤插槽）

| 插槽名 | 说明 |
| ------ | ---- |
| icon | 自定义图标 |
| title | 步骤标题 |
| description | 步骤描述 |

**使用示例：**

示例 1 (vue) - 基础用法：
```vue
<template>
  <el-steps style="max-width: 600px" :active="active" finish-status="success">
    <el-step title="步骤 1" />
    <el-step title="步骤 2" />
    <el-step title="步骤 3" />
  </el-steps>

  <el-button style="margin-top: 12px" @click="next">下一步</el-button>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const active = ref(0)

const next = () => {
  if (active.value++ > 2) active.value = 0
}
</script>
```

示例 2 (vue) - 带描述：
```vue
<template>
  <el-steps style="max-width: 600px" :active="2" align-center>
    <el-step title="步骤 1" description="一些描述" />
    <el-step title="步骤 2" description="一些描述" />
    <el-step title="步骤 3" description="一些描述" />
  </el-steps>
</template>
```

示例 3 (vue) - 简单步骤条：
```vue
<template>
  <el-steps
    class="mb-4"
    style="max-width: 600px"
    :space="200"
    :active="1"
    simple
  >
    <el-step title="步骤 1" :icon="Edit" />
    <el-step title="步骤 2" :icon="UploadFilled" />
    <el-step title="步骤 3" :icon="Picture" />
  </el-steps>

  <el-steps style="max-width: 600px" :active="1" finish-status="success" simple>
    <el-step title="步骤 1" />
    <el-step title="步骤 2" />
    <el-step title="步骤 3" />
  </el-steps>
</template>

<script lang="ts" setup>
import { Edit, Picture, UploadFilled } from '@element-plus/icons-vue'
</script>
```

示例 4 (vue) - 垂直步骤条：
```vue
<template>
  <div style="height: 300px; max-width: 600px">
    <el-steps direction="vertical" :active="1">
      <el-step title="步骤 1" />
      <el-step title="步骤 2" />
      <el-step title="步骤 3" />
    </el-steps>
  </div>
</template>
```

---

## Menu 导航菜单

**URL：** llms-txt#menu

**内容概览：**
- 顶部导航
- 左右导航
- 侧边栏
- 折叠
- Popper 偏移 ^(2.4.4)
- Menu API
  - Menu Attributes（菜单属性）
  - Menu Events（菜单事件）
  - Menu Slots（菜单插槽）
  - Menu Exposes（菜单暴露方法）

### 组件说明
为网站提供导航的菜单组件。

### 顶部导航
顶部菜单可用于多种场景。

### 侧边栏
带有子菜单的垂直菜单。

### 折叠
垂直菜单可以折叠。

### Popper 偏移 ^(2.4.4)
带有 popperOffset 的子菜单将覆盖菜单的 `popper-offset`。

### Menu Attributes（菜单属性）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| mode | 菜单显示模式 | ^[enum]`'horizontal' \| 'vertical'` | vertical |
| collapse | 是否折叠菜单（仅在垂直模式下可用） | ^[boolean] | false |
| ellipsis | 是否省略菜单（仅在水平模式下可用） | ^[boolean] | true |
| ellipsis-icon ^(2.4.4) | 自定义省略图标（仅在水平模式和 ellipsis 为 true 时可用） | ^[string] / ^[Component] | — |
| popper-offset ^(2.4.4) | 弹出框的偏移量（对所有子菜单有效） | ^[number] | 6 |
| default-active | 页面加载时激活菜单的索引 | ^[string] | '' |
| default-openeds | 当前激活子菜单的索引数组 | ^[object]`string[]` | [] |
| unique-opened | 是否只激活一个子菜单 | ^[boolean] | false |
| menu-trigger | 子菜单的触发方式，仅在 `mode` 为 'horizontal' 时有效 | ^[enum]`'hover' \| 'click'` | hover |
| router | 是否启用 `vue-router` 模式。如果为 true，index 将作为 'path' 来激活路由动作。与 `default-active` 配合使用以在加载时设置激活项。 | ^[boolean] | false |
| collapse-transition | 是否启用折叠过渡 | ^[boolean] | true |
| popper-effect ^(2.2.26) | Tooltip 主题，内置主题：`dark` / `light`，菜单折叠时 | ^[enum]`'dark' \| 'light'` / ^[string] | dark |
| close-on-click-outside ^(2.4.4) | 可选，点击外部时是否折叠菜单 | ^[boolean] | false |
| popper-class ^(2.5.0) | 所有弹出菜单和标题 Tooltip 的自定义类名 | ^[string] | — |
| popper-style ^(2.11.5) | 所有弹出菜单和标题 Tooltip 的自定义样式 | ^[string] / ^[object] | — |
| show-timeout ^(2.5.0) | 显示前所有菜单的控制超时时间 | ^[number] | 300 |
| hide-timeout ^(2.5.0) | 隐藏前所有菜单的控制超时时间 | ^[number] | 300 |
| background-color ^(已弃用) | 菜单的背景颜色（十六进制格式）（建议在样式类中使用 `--el-menu-bg-color` 代替） | ^[string] | #ffffff |
| text-color ^(已弃用) | 菜单的文本颜色（十六进制格式）（建议在样式类中使用 `--el-menu-text-color` 代替） | ^[string] | #303133 |
| active-text-color ^(已弃用) | 当前激活菜单项的文本颜色（十六进制格式）（建议在样式类中使用 `--el-menu-active-color` 代替） | ^[string] | #409eff |
| persistent ^(2.9.5) | 当菜单不活跃且 `persistent` 为 `false` 时，下拉菜单将被销毁 | ^[boolean] | true |

### Menu Events（菜单事件）

| 事件名 | 说明 | 类型 |
| ------ | ---- | ---- |
| select | 菜单激活时的回调函数 | ^[Function]`MenuSelectEvent` |
| open | 子菜单展开时的回调函数 | ^[Function]`MenuOpenEvent` |
| close | 子菜单折叠时的回调函数 | ^[Function]`MenuCloseEvent` |

### Menu Slots（菜单插槽）

| 插槽名 | 说明 | 子组件 |
| ------ | ---- | ---- |
| default | 自定义默认内容 | SubMenu / Menu-Item / Menu-Item-Group |

### Menu Exposes（菜单暴露方法）

| 方法名 | 说明 | 类型 |
| ------ | ---- | ---- |
| open | 打开特定的子菜单，参数为要打开的子菜单索引 | ^[Function]`(index: string) => void` |
| close | 关闭特定的子菜单，参数为要关闭的子菜单索引 | ^[Function]`(index: string) => void` |
| handleResize | 手动触发菜单宽度重新计算 | ^[Function]`() => void` |
| updateActiveIndex ^(2.9.8) | 设置激活菜单的索引 | ^[Function]`(index: string) => void` |

### SubMenu Attributes（子菜单属性）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| index ^(必填) | 唯一标识 | ^[string] | — |
| popper-class | 弹出菜单的自定义类名 | ^[string] | — |
| popper-style ^(2.11.5) | 弹出菜单的自定义样式 | ^[string] / ^[object] | — |
| show-timeout | 显示子菜单前的超时时间（默认继承菜单的 `show-timeout`） | ^[number] | — |
| hide-timeout | 隐藏子菜单前的超时时间（默认继承菜单的 `hide-timeout`） | ^[number] | — |
| disabled | 是否禁用子菜单 | ^[boolean] | false |
| teleported | 弹出菜单是否传送到 body，默认一级 SubMenu 为 true，其他为 false | ^[boolean] | undefined |
| popper-offset | 弹出框的偏移量（覆盖菜单的 `popper`） | ^[number] | — |
| expand-close-icon | 菜单展开且子菜单关闭时的图标，`expand-close-icon` 和 `expand-open-icon` 需要一起传递才能生效 | ^[string] / ^[Component] | — |
| expand-open-icon | 菜单展开且子菜单打开时的图标，`expand-open-icon` 和 `expand-close-icon` 需要一起传递才能生效 | ^[string] / ^[Component] | — |
| collapse-close-icon | 菜单折叠且子菜单关闭时的图标，`collapse-close-icon` 和 `collapse-open-icon` 需要一起传递才能生效 | ^[string] / ^[Component] | — |
| collapse-open-icon | 菜单折叠且子菜单打开时的图标，`collapse-open-icon` 和 `collapse-close-icon` 需要一起传递才能生效 | ^[string] / ^[Component] | — |

### MenuItem Attributes（菜单项属性）

| 属性名 | 说明 | 类型 | 默认值 |
| ------ | ---- | ---- | ---- |
| index ^(必填) | 唯一标识 | ^[string] | — |
| route | Vue Router 路由位置参数 | ^[string] / ^[object] | — |
| disabled | 是否禁用 | ^[boolean] | false |

### MenuItem Events（菜单项事件）

| 事件名 | 说明 | 类型 |
| ------ | ---- | ---- |
| click | 菜单项被点击时的回调函数，参数为菜单项实例 | ^[Function]`(item: MenuItemRegistered) => void` |

**使用示例：**

示例 1 (vue) - 顶部导航：
```vue
<template>
  <el-menu
    :default-active="activeIndex"
    class="el-menu-demo"
    mode="horizontal"
    @select="handleSelect"
  >
    <el-menu-item index="1">处理中心</el-menu-item>
    <el-sub-menu index="2">
      <template #title>工作区</template>
      <el-menu-item index="2-1">选项一</el-menu-item>
      <el-menu-item index="2-2">选项二</el-menu-item>
      <el-menu-item index="2-3">选项三</el-menu-item>
      <el-sub-menu index="2-4">
        <template #title>选项四</template>
        <el-menu-item index="2-4-1">选项一</el-menu-item>
        <el-menu-item index="2-4-2">选项二</el-menu-item>
        <el-menu-item index="2-4-3">选项三</el-menu-item>
      </el-sub-menu>
    </el-sub-menu>
    <el-menu-item index="3" disabled>消息</el-menu-item>
    <el-menu-item index="4">订单</el-menu-item>
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
    <el-menu-item index="1">处理中心</el-menu-item>
    <el-sub-menu index="2">
      <template #title>工作区</template>
      <el-menu-item index="2-1">选项一</el-menu-item>
      <el-menu-item index="2-2">选项二</el-menu-item>
      <el-menu-item index="2-3">选项三</el-menu-item>
      <el-sub-menu index="2-4">
        <template #title>选项四</template>
        <el-menu-item index="2-4-1">选项一</el-menu-item>
        <el-menu-item index="2-4-2">选项二</el-menu-item>
        <el-menu-item index="2-4-3">选项三</el-menu-item>
      </el-sub-menu>
    </el-sub-menu>
    <el-menu-item index="3" disabled>消息</el-menu-item>
    <el-menu-item index="4">订单</el-menu-item>
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

示例 2 (vue) - 侧边栏：
```vue
<template>
  <el-radio-group v-model="isCollapse" style="margin-bottom: 20px">
    <el-radio-button :value="false">展开</el-radio-button>
    <el-radio-button :value="true">折叠</el-radio-button>
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
        <span>导航一</span>
      </template>
      <el-menu-item-group>
        <template #title><span>分组一</span></template>
        <el-menu-item index="1-1">选项一</el-menu-item>
        <el-menu-item index="1-2">选项二</el-menu-item>
      </el-menu-item-group>
      <el-menu-item-group title="分组二">
        <el-menu-item index="1-3">选项三</el-menu-item>
      </el-menu-item-group>
      <el-sub-menu index="1-4">
        <template #title><span>选项四</span></template>
        <el-menu-item index="1-4-1">选项一</el-menu-item>
      </el-sub-menu>
    </el-sub-menu>
    <el-menu-item index="2">
      <el-icon><icon-menu /></el-icon>
      <template #title>导航二</template>
    </el-menu-item>
    <el-menu-item index="3" disabled>
      <el-icon><document /></el-icon>
      <template #title>导航三</template>
    </el-menu-item>
    <el-menu-item index="4">
      <el-icon><setting /></el-icon>
      <template #title>导航四</template>
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

---
