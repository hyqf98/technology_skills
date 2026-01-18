---
name: element-plus
description: Element Plus - Vue 3 组件库，为设计师和开发者提供企业级 UI 组件。涵盖 81+ 组件，包括按钮、表单、表格、导航、反馈和数据展示组件。包括安装配置、主题定制、国际化和组件 API 参考。
---

# Element Plus 技能文档

基于官方文档生成的 Element Plus 开发综合指南。

## 何时使用此技能

在以下场景中使用此技能：
- 开发 Vue 3 + Element Plus 应用
- 查询 Element Plus 组件功能或 API
- 实现 Element Plus 解决方案
- 调试 Element Plus 代码
- 学习 Element Plus 最佳实践

## 技术概述

**Element Plus** 是一套基于 Vue 3 的企业级 UI 组件库，为设计师和开发者提供了丰富的组件和设计规范。它是 Element UI 在 Vue 3 时代的续作，完全使用 TypeScript 重写，支持按需引入和主题定制。

### 核心特性

- **81+ 组件**：涵盖企业应用所需的各种 UI 组件
- **TypeScript 支持**：完整的 TypeScript 类型定义
- **Vue 3 原生**：充分利用 Vue 3 Composition API
- **按需引入**：支持 Tree Shaking，优化打包体积
- **主题定制**：使用 CSS 变量实现动态主题切换
- **国际化**：内置多语言支持
- **无障碍**：遵循 WAI-ARIA 标准

### 组件分类

| 分类 | 组件数量 | 主要组件 |
|------|---------|---------|
| **基础组件** | 12+ | Button、Icon、Link、Text、Scrollbar |
| **布局组件** | 4+ | Container、Layout、Row、Col |
| **表单组件** | 20+ | Form、Input、Select、DatePicker、Upload |
| **数据组件** | 15+ | Table、Pagination、Tree、Tag、Badge |
| **导航组件** | 8+ | Menu、Tabs、Breadcrumb、Dropdown |
| **反馈组件** | 10+ | Dialog、Message、Notification、Loading |
| **其他组件** | 12+ | Card、Carousel、Avatar、Calendar |

## 快速参考

### 安装与配置

#### 方式 1：完整引入（适合原型开发）

```bash
npm install element-plus
```

```typescript
// main.ts
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'

const app = createApp(App)
app.use(ElementPlus)
app.mount('#app')
```

#### 方式 2：按需引入（推荐用于生产环境）

```bash
npm install element-plus
npm install -D unplugin-vue-components unplugin-auto-import
```

```typescript
// vite.config.ts
import { defineConfig } from 'vite'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

export default defineConfig({
  // ...
  plugins: [
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
})
```

#### 方式 3：手动按需引入

```typescript
import { ElButton, ElSelect } from 'element-plus'
import 'element-plus/es/components/button/style/css'
import 'element-plus/es/components/select/style/css'

app.use(ElButton)
app.use(ElSelect)
```

### 常见配置模式

#### 模式 1：表单验证

```vue
<template>
  <el-form ref="formRef" :model="form" :rules="rules" label-width="120px">
    <el-form-item label="活动名称" prop="name">
      <el-input v-model="form.name" />
    </el-form-item>
    <el-form-item label="活动区域" prop="region">
      <el-select v-model="form.region" placeholder="请选择活动区域">
        <el-option label="区域一" value="shanghai" />
        <el-option label="区域二" value="beijing" />
      </el-select>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="submitForm">立即创建</el-button>
      <el-button @click="resetForm">重置</el-button>
    </el-form-item>
  </el-form>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue'
import type { FormInstance, FormRules } from 'element-plus'

const formRef = ref<FormInstance>()
const form = reactive({
  name: '',
  region: ''
})

const rules = reactive<FormRules>({
  name: [
    { required: true, message: '请输入活动名称', trigger: 'blur' },
    { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
  ],
  region: [
    { required: true, message: '请选择活动区域', trigger: 'change' }
  ]
})

const submitForm = async () => {
  if (!formRef.value) return
  await formRef.value.validate((valid, fields) => {
    if (valid) {
      console.log('提交成功', form)
    } else {
      console.log('验证失败', fields)
    }
  })
}

const resetForm = () => {
  formRef.value?.resetFields()
}
</script>
```

#### 模式 2：表格分页

```vue
<template>
  <el-table :data="tableData" style="width: 100%">
    <el-table-column prop="date" label="日期" width="180" />
    <el-table-column prop="name" label="姓名" width="180" />
    <el-table-column prop="address" label="地址" />
    <el-table-column label="操作" width="180">
      <template #default="scope">
        <el-button size="small" @click="handleEdit(scope.row)">编辑</el-button>
        <el-button size="small" type="danger" @click="handleDelete(scope.row)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>

  <el-pagination
    v-model:current-page="currentPage"
    v-model:page-size="pageSize"
    :page-sizes="[10, 20, 50, 100]"
    layout="total, sizes, prev, pager, next, jumper"
    :total="total"
    @size-change="handleSizeChange"
    @current-change="handleCurrentChange"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue'

const currentPage = ref(1)
const pageSize = ref(10)
const total = ref(0)

const tableData = ref([])

const fetchData = async () => {
  // 从 API 获取数据
  const response = await api.getUsers({
    page: currentPage.value,
    pageSize: pageSize.value
  })
  tableData.value = response.data
  total.value = response.total
}

const handleEdit = (row: any) => {
  console.log('编辑', row)
}

const handleDelete = (row: any) => {
  console.log('删除', row)
}

const handleSizeChange = (val: number) => {
  pageSize.value = val
  fetchData()
}

const handleCurrentChange = (val: number) => {
  currentPage.value = val
  fetchData()
}

fetchData()
</script>
```

#### 模式 3：动态主题切换

```typescript
// theme.ts
import { ref } from 'vue'

export const useTheme = () => {
  const isDark = ref(false)

  const toggleTheme = () => {
    isDark.value = !isDark.value
    document.documentElement.classList.toggle('dark', isDark.value)

    // 更新 Element Plus CSS 变量
    if (isDark.value) {
      document.documentElement.style.setProperty('--el-bg-color', '#1a1a1a')
      document.documentElement.style.setProperty('--el-text-color-primary', '#ffffff')
    } else {
      document.documentElement.style.setProperty('--el-bg-color', '#ffffff')
      document.documentElement.style.setProperty('--el-text-color-primary', '#303133')
    }
  }

  // 保存到 localStorage
  const saveTheme = () => {
    localStorage.setItem('theme', isDark.value ? 'dark' : 'light')
  }

  // 从 localStorage 恢复
  const loadTheme = () => {
    const theme = localStorage.getItem('theme')
    if (theme === 'dark') {
      isDark.value = true
      document.documentElement.classList.add('dark')
    }
  }

  return {
    isDark,
    toggleTheme,
    saveTheme,
    loadTheme
  }
}
```

```vue
<!-- ThemeToggle.vue -->
<template>
  <el-switch
    v-model="isDark"
    inline-prompt
    active-text="深色"
    inactive-text="浅色"
    @change="handleThemeChange"
  />
</template>

<script setup lang="ts">
import { useTheme } from './theme'

const { isDark, toggleTheme, saveTheme, loadTheme } = useTheme()

const handleThemeChange = () => {
  toggleTheme()
  saveTheme()
}

loadTheme()
</script>
```

#### 模式 4：国际化配置

```typescript
// i18n.ts
import { createI18n } from 'vue-i18n'
import zhCn from 'element-plus/es/locale/lang/zh-cn'
import en from 'element-plus/es/locale/lang/en'

const messages = {
  'zh-CN': {
    // 自定义翻译
  },
  'en': {
    // custom translations
  }
}

const i18n = createI18n({
  locale: 'zh-CN',
  fallbackLocale: 'en',
  messages
})

export default i18n
```

```vue
<template>
  <el-config-provider :locale="locale">
    <App />
  </el-config-provider>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import zhCn from 'element-plus/es/locale/lang/zh-cn'
import en from 'element-plus/es/locale/lang/en'

const currentLang = computed(() => {
  return 'zh-CN' ? zhCn : en
})
</script>
```

### 代码示例模式

#### 示例 1：骨架屏加载

```vue
<template>
  <el-skeleton :rows="5" animated />
</template>
```

#### 示例 2：空状态

```vue
<template>
  <el-empty description="暂无数据" />
</template>

<!-- 带操作按钮 -->
<el-empty>
  <el-button type="primary">按钮</el-button>
</el-empty>
```

#### 示例 3：固钉

```vue
<template>
  <el-affix :offset="120">
    <el-button type="primary">距离顶部 120px</el-button>
  </el-affix>
</template>
```

## 主题定制

### CSS 变量覆盖

```css
/* styles/theme.css */
:root {
  /* 主色 */
  --el-color-primary: #409eff;
  --el-color-success: #67c23a;
  --el-color-warning: #e6a23c;
  --el-color-danger: #f56c6c;
  --el-color-error: #f56c6c;
  --el-color-info: #909399;

  /* 文字颜色 */
  --el-text-color-primary: #303133;
  --el-text-color-regular: #606266;
  --el-text-color-secondary: #909399;

  /* 边框颜色 */
  --el-border-color: #dcdfe6;
  --el-border-color-light: #e4e7ed;

  /* 背景色 */
  --el-bg-color: #ffffff;
  --el-bg-color-page: #f2f3f5;

  /* 圆角 */
  --el-border-radius-base: 4px;
  --el-border-radius-small: 2px;
  --el-border-radius-round: 20px;
  --el-border-radius-circle: 100%;
}
```

### SCSS 变量定制

```bash
npm install -D sass
```

```typescript
// vite.config.ts
export default defineConfig({
  css: {
    preprocessorOptions: {
      scss: {
        additionalData: `use @ "@/styles/element-variables.scss" as *;`
      }
    }
  }
})
```

```scss
// styles/element-variables.scss
@forward 'element-plus/theme-chalk/src/common/var.scss' with (
  $colors: (
    'primary': (
      'base': #409eff
    )
  ),
  $button: (
    'border-radius': 8px
  )
);
```

## 最佳实践

### 1. 按需引入优化

```typescript
// 使用自动导入插件减少打包体积
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

plugins: [
  AutoImport({
    resolvers: [ElementPlusResolver()],
  }),
  Components({
    resolvers: [ElementPlusResolver()],
  }),
]
```

### 2. TypeScript 类型支持

```typescript
import type { FormInstance, FormRules } from 'element-plus'

interface LoginForm {
  username: string
  password: string
}

const loginForm = reactive<LoginForm>({
  username: '',
  password: ''
})

const rules = reactive<FormRules<LoginForm>>({
  username: [
    { required: true, message: '请输入用户名', trigger: 'blur' }
  ],
  password: [
    { required: true, message: '请输入密码', trigger: 'blur' },
    { min: 6, message: '密码长度不能少于6位', trigger: 'blur' }
  ]
})
```

### 3. 响应式布局

```vue
<template>
  <el-row :gutter="20">
    <el-col :xs="24" :sm="12" :md="8" :lg="6" :xl="4">
      <div class="grid-content">内容</div>
    </el-col>
  </el-row>
</template>

<style scoped>
.grid-content {
  background: #f0f2f5;
  border-radius: 4px;
  min-height: 36px;
}
</style>
```

### 4. 组件二次封装

```vue
<!-- components/CustomTable.vue -->
<template>
  <el-table
    :data="data"
    v-loading="loading"
    :height="height"
    stripe
    border
  >
    <slot></slot>
  </el-table>
</template>

<script setup lang="ts">
interface Props {
  data: any[]
  loading?: boolean
  height?: string | number
}

withDefaults(defineProps<Props>(), {
  loading: false,
  height: 'auto'
})
</script>
```

## 常见问题解决

| 问题 | 解决方案 |
|------|---------|
| 样式丢失 | 确保 `import 'element-plus/dist/index.css'` 已引入 |
| 图标不显示 | 单独引入 `@element-plus/icons-vue` 并注册 |
| 主题不生效 | 检查 CSS 变量作用域，避免被其他样式覆盖 |
| 按需引入报错 | 配置 `unplugin-vue-components` 和 `unplugin-auto-import` |
| TypeScript 报错 | 安装 `@element-plus/icons-vue` 的类型定义 |

## 参考文档

此技能在 `references/` 目录中包含全面的文档：

| 文档 | 描述 |
|------|------|
| **basic.md** | 基础组件文档 |
| **data.md** | 数据组件文档 |
| **feedback.md** | 反馈组件文档 |
| **form.md** | 表单组件文档 |
| **getting_started.md** | 入门指南 |
| **navigation.md** | 导航组件文档 |
| **other.md** | 其他组件文档 |
| **others.md** | 其他内容文档 |

## 使用指南

### 对于初学者
1. 从 `getting_started.md` 开始了解基础
2. 创建 Vue 3 + Vite 项目
3. 按需引入 Element Plus
4. 学习常用组件（Button、Form、Table）
5. 实践表单验证和数据展示

### 对于主题定制
- 使用 CSS 变量实现动态主题
- 通过 SCSS 变量深度定制
- 参考官方设计规范
- 保持组件一致性

### 对于性能优化
- 启用按需引入
- 使用虚拟滚动处理大数据
- 懒加载非首屏组件
- 优化打包配置

## 资源

### references/
从官方来源提取的组织化文档，包含：
- 详细的组件说明
- 带语言标注的代码示例
- 原始文档链接
- 快速导航目录

### scripts/
添加用于常见自动化任务的辅助脚本。

### assets/
添加模板、样板代码或示例项目。

## 注意事项

- 此技能从官方文档自动生成
- Vue 3 是必需的前置条件
- 推荐使用 TypeScript 开发
- 定期更新以获取最新组件和修复

## 更新说明

刷新此技能的文档：
1. 使用相同配置重新运行爬虫
2. 技能将使用最新信息重建

## 相关资源

### 官方文档
- [Element Plus 官方网站](https://element-plus.org/)
- [Element Plus 文档](https://element-plus.org/zh-CN/)
- [Element Plus GitHub](https://github.com/element-plus/element-plus)

### 主题定制
- [主题定制指南](https://element-plus.org/zh-CN/guide/theming.html)
- [设计规范](https://element-plus.org/zh-CN/guide/design.html)

### 中文资源
- [Vue 3 + Element Plus 集成指南](https://blog.csdn.net/weixin_52648900/article/details/145932023)
- [Vue3中集成Element Plus组件库的完整指南](https://comate.baidu.com/zh/page/qpu1la2iu69)
- [Vue3.0整合Element Plus的实践指南](https://www.trae.cn/article/706670594)

---

## 最新特性 (2025)

### SCSS 主题定制 (Vite 配置)

**配置 Vite 处理自定义 SCSS 主题:**
```typescript
import path from 'path'
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import ElementPlus from 'unplugin-element-plus/vite'

export default defineConfig({
  resolve: {
    alias: {
      '~/': `${path.resolve(__dirname, 'src')}/`
    },
  },
  css: {
    preprocessorOptions: {
      scss: {
        // 自动引入自定义 SCSS 变量文件
        additionalData: `@use "~/styles/element/index.scss" as *;`
      }
    }
  },
  plugins: [
    vue(),
    // 使用 unplugin-element-plus 自动引入样式
    ElementPlus({
      useSource: true
    })
  ]
})
```

### CSS 变量动态主题切换

**使用 CSS 变量实现主题切换:**
```css
/* 全局样式 */
:root {
  --el-color-primary: green;
}

/* 组件级定制 */
.el-popper.is-customized {
  padding: 6px 12px;
  background: linear-gradient(90deg, rgb(159, 229, 151), rgb(204, 229, 129));
}

.el-popper.is-customized .el-popper__arrow::before {
  background: linear-gradient(45deg, #b2e68d, #bce689);
  right: 0;
}
```

### Tooltip 主题定制

**三种主题效果:**
```vue
<template>
  <!-- Dark 主题 (默认) -->
  <el-tooltip content="Top center" placement="top">
    <el-button>Dark</el-button>
  </el-tooltip>

  <!-- Light 主题 -->
  <el-tooltip content="Bottom center" placement="bottom" effect="light">
    <el-button>Light</el-button>
  </el-tooltip>

  <!-- 自定义主题 -->
  <el-tooltip content="Bottom center" effect="customized">
    <el-button>Customized theme</el-button>
  </el-tooltip>
</template>

<style>
.el-popper.is-customized {
  padding: 6px 12px;
  background: linear-gradient(90deg, rgb(159, 229, 151), rgb(204, 229, 129));
}

.el-popper.is-customized .el-popper__arrow::before {
  background: linear-gradient(45deg, #b2e68d, #bce689);
  right: 0;
}
</style>
```

### 完整表单示例 (2025 更新)

**典型的 Element Plus 表单布局:**
```vue
<template>
  <el-form :model="form" label-width="auto" style="max-width: 600px">
    <el-form-item label="Activity name">
      <el-input v-model="form.name" />
    </el-form-item>

    <el-form-item label="Activity zone">
      <el-select v-model="form.region" placeholder="please select your zone">
        <el-option label="Zone one" value="shanghai" />
        <el-option label="Zone two" value="beijing" />
      </el-select>
    </el-form-item>

    <el-form-item label="Activity time">
      <el-col :span="11">
        <el-date-picker
          v-model="form.date1"
          type="date"
          placeholder="Pick a date"
          style="width: 100%"
        />
      </el-col>
      <el-col :span="2" class="text-center">
        <span class="text-gray-500">-</span>
      </el-col>
      <el-col :span="11">
        <el-time-picker
          v-model="form.date2"
          placeholder="Pick a time"
          style="width: 100%"
        />
      </el-col>
    </el-form-item>

    <el-form-item label="Instant delivery">
      <el-switch v-model="form.delivery" />
    </el-form-item>

    <el-form-item label="Activity type">
      <el-checkbox-group v-model="form.type">
        <el-checkbox value="Online activities" name="type">
          Online activities
        </el-checkbox>
        <el-checkbox value="Promotion activities" name="type">
          Promotion activities
        </el-checkbox>
        <el-checkbox value="Offline activities" name="type">
          Offline activities
        </el-checkbox>
        <el-checkbox value="Simple brand exposure" name="type">
          Simple brand exposure
        </el-checkbox>
      </el-checkbox-group>
    </el-form-item>

    <el-form-item label="Resources">
      <el-radio-group v-model="form.resource">
        <el-radio value="Sponsor">Sponsor</el-radio>
        <el-radio value="Venue">Venue</el-radio>
      </el-radio-group>
    </el-form-item>

    <el-form-item label="Activity form">
      <el-input v-model="form.desc" type="textarea" />
    </el-form-item>

    <el-form-item>
      <el-button type="primary" @click="onSubmit">Create</el-button>
      <el-button>Cancel</el-button>
    </el-form-item>
  </el-form>
</template>

<script lang="ts" setup>
import { reactive } from 'vue'

// 使用 reactive 定义表单数据
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

const onSubmit = () => {
  console.log('submit!')
}
</script>
```

### 2025 新增组件

| 组件名称 | 说明 | 状态 |
|---------|------|------|
| **DateTimePicker** | 日期时间选择器 | ✅ 稳定 |
| **Form** | 表单组件 | ✅ 稳定 |
| **Tooltip** | 提示组件 | ✅ 稳定 |
| **CheckboxGroup** | 复选框组 | ✅ 稳定 |
| **RadioGroup** | 单选框组 | ✅ 稳定 |
| **Switch** | 开关 | ✅ 稳定 |
| **Input** | 输入框 | ✅ 稳定 |
| **Select** | 选择器 | ✅ 稳定 |
| **DatePicker** | 日期选择器 | ✅ 稳定 |
| **TimePicker** | 时间选择器 | ✅ 稳定 |

### 按需引入优化 (2025 推荐)

**使用 unplugin-vue-components 自动引入:**
```typescript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

export default defineConfig({
  plugins: [
    vue(),
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [
        ElementPlusResolver({
          importStyle: 'sass', // 或 'css'
        })
      ],
    }),
  ]
})
```

### TypeScript 类型支持 (2025)

**完整类型定义:**
```typescript
import type {
  FormInstance,
  FormRules,
  FormItemProp
} from 'element-plus'

interface LoginForm {
  username: string
  password: string
  remember?: boolean
}

const loginForm = reactive<LoginForm>({
  username: '',
  password: '',
  remember: false
})

const rules = reactive<FormRules<LoginForm>>({
  username: [
    { required: true, message: '请输入用户名', trigger: 'blur' },
    { min: 3, max: 20, message: '长度在 3 到 20 个字符', trigger: 'blur' }
  ],
  password: [
    { required: true, message: '请输入密码', trigger: 'blur' },
    { min: 6, message: '密码长度不能少于6位', trigger: 'blur' }
  ]
})
```

### 国际化配置 (i18n)

**配置多语言支持:**
```typescript
import { createI18n } from 'vue-i18n'
import zhCn from 'element-plus/es/locale/lang/zh-cn'
import en from 'element-plus/es/locale/lang/en'

const i18n = createI18n({
  locale: 'zh-CN',
  fallbackLocale: 'en',
  messages: {
    'zh-CN': {
      // 自定义翻译
    },
    'en': {
      // custom translations
    }
  }
})

export default i18n
```

```vue
<template>
  <el-config-provider :locale="currentLang">
    <App />
  </el-config-provider>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import zhCn from 'element-plus/es/locale/lang/zh-cn'
import en from 'element-plus/es/locale/lang/en'

const currentLang = computed(() => {
  // 根据当前语言切换
  return 'zh-CN' ? zhCn : en
})
</script>
```
