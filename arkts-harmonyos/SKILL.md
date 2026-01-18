---
name: arkts-harmonyos
description: ArkTS 和 HarmonyOS 应用开发 - 使用 ArkTS 语言和 ArkUI 框架构建 HarmonyOS 应用程序。涵盖应用模型、UI 开发、数据存储、网络请求等核心功能。
---

# HarmonyOS 应用开发技能文档

## 何时使用此技能

在以下场景中使用此技能：
- 开发 HarmonyOS 应用程序
- 使用 ArkTS 语言进行开发
- 使用 ArkUI 框架构建 UI
- 使用 DevEco Studio
- 实现 HarmonyOS 功能（Ability、数据存储、网络等）

## 技术概述

**HarmonyOS** 是华为推出的分布式操作系统，**ArkTS** 是 HarmonyOS 优选的主力应用开发语言。它是 TypeScript 的扩展，提供了声明式 UI 能力和更丰富的状态管理机制。

### 核心特性

- **ArkTS 语言**：基于 TypeScript 扩展，支持声明式 UI
- **ArkUI 框架**：提供丰富的组件和布局能力
- **分布式能力**：跨设备协同、硬件互助
- **一次开发，多端部署**：手机、平板、手表、车机等
- **高性能**：原生渲染，流畅体验

### 文档结构

| 分类 | 页数 | 描述 |
|------|------|------|
| **Getting Started** | 8 页 | 入门指南和快速开始 |
| **ArkTS** | 170 页 | ArkTS 语言详解 |
| **ArkUI** | 83 页 | ArkUI 框架组件 |
| **Application Model** | 6 页 | 应用模型（Ability） |
| **Network** | 1 页 | 网络请求 |
| **Data** | 3 页 | 数据存储 |
| **Samples** | 10 页 | 示例代码 |
| **Other** | 10 页 | 其他内容 |

## 快速参考

### 项目结构

```
MyHarmonyOSApp/
├── entry/
│   └── src/
│       └── main/
│           ├── ets/                 # ArkTS 源代码
│           │   ├── entryability/    # 应用入口
│           │   │   └── EntryAbility.ets
│           │   ├── pages/           # 页面
│           │   │   ├── Index.ets
│           │   │   └── Second.ets
│           │   └── model/           # 数据模型
│           ├── resources/           # 资源文件
│           │   ├── base/
│           │   └── rawfile/
│           └── module.json5         # 模块配置
├── oh-package.json5                 # 依赖配置
└── app.json5                       # 应用配置
```

### 常见配置模式

#### 模式 1：创建基础页面

```typescript
// pages/Index.ets
@Entry
@Component
struct Index {
  @State message: string = 'Hello HarmonyOS'

  build() {
    Column() {
      Text(this.message)
        .fontSize(50)
        .fontWeight(FontWeight.Bold)

      Button('点击我')
        .onClick(() => {
          this.message = '你点击了按钮！'
        })
    }
    .width('100%')
    .height('100%')
    .justifyContent(FlexAlign.Center)
  }
}
```

#### 模式 2：页面导航

```typescript
// pages/Index.ets
import router from '@ohos.router'

@Entry
@Component
struct Index {
  build() {
    Column() {
      Button('跳转到第二页')
        .onClick(() => {
          router.pushUrl({
            url: 'pages/Second'
          })
        })
    }
  }
}

// pages/Second.ets
import router from '@ohos.router'

@Entry
@Component
struct Second {
  build() {
    Column() {
      Text('这是第二页')

      Button('返回')
        .onClick(() => {
          router.back()
        })
    }
  }
}
```

#### 模式 3：列表渲染

```typescript
@Entry
@Component
struct ListDemo {
  @State fruits: Array<string> = ['苹果', '香蕉', '橙子', '葡萄']

  build() {
    Column() {
      List({ space: 10 }) {
        ForEach(this.fruits, (item: string) => {
          ListItem() {
            Text(item)
              .fontSize(20)
              .padding(10)
              .backgroundColor('#f0f0f0')
              .borderRadius(8)
          }
        }, (item: string) => item)
      }
      .width('100%')
      .height('100%')
      .padding(10)
    }
  }
}
```

#### 模式 4：条件渲染

```typescript
@Entry
@Component
struct ConditionalDemo {
  @State isShow: boolean = true
  @State count: number = 0

  build() {
    Column() {
      if (this.isShow) {
        Text('这是条件显示的内容')
          .fontSize(20)
      }

      Button(this.isShow ? '隐藏' : '显示')
        .onClick(() => {
          this.isShow = !this.isShow
        })

      // 使用条件表达式
      Text(this.count > 10 ? '计数大于10' : '计数小于等于10')
        .fontSize(16)
        .margin({ top: 20 })

      Button('增加计数')
        .onClick(() => {
          this.count++
        })
    }
    .justifyContent(FlexAlign.Center)
    .width('100%')
    .height('100%')
  }
}
```

#### 模式 5：状态管理

```typescript
// 使用 @State 装饰器
@Component
struct StateDemo {
  @State count: number = 0

  increment() {
    this.count++
  }

  build() {
    Column() {
      Text(`计数: ${this.count}`)
      Button('增加')
        .onClick(() => this.increment())
    }
  }
}

// 使用 @Prop 装饰器（父子组件通信）
@Component
struct ChildComponent {
  @Prop message: string

  build() {
    Text(this.message)
  }
}

@Entry
@Component
struct ParentComponent {
  @State parentMessage: string = '来自父组件的消息'

  build() {
    Column() {
      ChildComponent({ message: this.parentMessage })
      Button('修改消息')
        .onClick(() => {
          this.parentMessage = '消息已更新'
        })
    }
  }
}

// 使用 @Provide 和 @Consume（跨层级组件通信）
@Component
struct GrandChild {
  @Consume message: string

  build() {
    Text(this.message)
  }
}

@Component
struct Child {
  build() {
    GrandChild()
  }
}

@Entry
@Component
struct GrandParent {
  @Provide message: string = '跨层级传递的消息'

  build() {
    Column() {
      Child()
      Button('修改消息')
        .onClick(() => {
          this.message = '消息已更新'
        })
    }
  }
}
```

#### 模式 6：网络请求

```typescript
import http from '@ohos.net.http'

@Entry
@Component
struct NetworkDemo {
  @State responseText: string = '等待请求...'

  async makeRequest() {
    try {
      const httpRequest = http.createHttp()

      const response = await httpRequest.request(
        'https://api.example.com/data',
        {
          method: http.RequestMethod.GET,
          header: {
            'Content-Type': 'application/json'
          },
          readTimeout: 10000,
          connectTimeout: 10000
        }
      )

      if (response.responseCode === 200) {
        this.responseText = response.result as string
      }
    } catch (error) {
      this.responseText = `请求失败: ${error}`
    }
  }

  build() {
    Column() {
      Text(this.responseText)
        .fontSize(16)
        .padding(10)

      Button('发送请求')
        .onClick(() => {
          this.makeRequest()
        })
    }
    .justifyContent(FlexAlign.Center)
    .width('100%')
    .height('100%')
  }
}
```

#### 模式 7：数据持久化

```typescript
// 使用 Preferences 存储数据
import dataPreferences from '@ohos.data.preferences'

@Entry
@Component
struct PreferencesDemo {
  @State savedText: string = ''

  async saveData() {
    try {
      const preferences = await dataPreferences.getPreferences(
        getContext(),
        'myPreferences'
      )

      await preferences.put('key', '保存的数据')
      await preferences.flush()

      this.savedText = '数据已保存'
    } catch (error) {
      this.savedText = `保存失败: ${error}`
    }
  }

  async loadData() {
    try {
      const preferences = await dataPreferences.getPreferences(
        getContext(),
        'myPreferences'
      )

      const value = await preferences.get('key', '默认值')
      this.savedText = value as string
    } catch (error) {
      this.savedText = `加载失败: ${error}`
    }
  }

  build() {
    Column() {
      Text(this.savedText)
        .fontSize(16)
        .padding(10)

      Button('保存数据')
        .onClick(() => {
          this.saveData()
        })

      Button('加载数据')
        .onClick(() => {
          this.loadData()
        })
    }
    .justifyContent(FlexAlign.Center)
    .width('100%')
    .height('100%')
  }
}
```

#### 模式 8：Ability 启动

```typescript
// EntryAbility.ets
import UIAbility from '@ohos.app.ability.UIAbility'
import window from '@ohos.window'
import Want from '@ohos.app.ability.Want'

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: Record<string, number>) {
    console.log('Ability onCreate')
  }

  onDestroy() {
    console.log('Ability onDestroy')
  }

  onWindowStageCreate(windowStage: window.WindowStage) {
    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        console.error('加载页面失败')
        return
      }
      console.log('页面加载成功')
    })
  }

  onWindowStageDestroy() {
    console.log('WindowStage destroy')
  }

  onForeground() {
    console.log('Ability onForeground')
  }

  onBackground() {
    console.log('Ability onBackground')
  }
}
```

### ArkUI 常用组件

| 组件 | 描述 | 主要属性 |
|------|------|---------|
| **Text** | 文本显示 | `fontSize`、`fontColor`、`fontWeight` |
| **Image** | 图片显示 | `src`、`objectFit` |
| **Button** | 按钮 | `type`、`stateEffect` |
| **TextInput** | 文本输入 | `placeholder`、`type` |
| **Column** | 纵向布局 | `space`、`alignItems` |
| **Row** | 横向布局 | `space`、`justifyContent` |
| **List** | 列表 | `space`、`initialIndex` |
| **Grid** | 网格 | `columnsTemplate`、`rowsTemplate` |
| **Scroll** | 滚动容器 | `scrollable`、`scrollBar` |
| **Tabs** | 标签页 | `barPosition`、`animationDuration` |

### 状态装饰器

| 装饰器 | 描述 | 使用场景 |
|--------|------|---------|
| `@State` | 组件内部状态 | 组件内部可变数据 |
| `@Prop` | 父子组件传递 | 单向数据流 |
| `@Provide` / `@Consume` | 跨层级传递 | 跨组件状态共享 |
| `@ObjectLink` | 对象链接 | 嵌套对象状态同步 |
| `@Link` | 双向绑定 | 父子组件双向同步 |

## 参考文档

此技能在 `references/` 目录中包含全面的文档：

| 文档 | 描述 |
|------|------|
| **getting_started.md** | 入门指南（8 页） |
| **arkts.md** | ArkTS 语言（170 页） |
| **arkui.md** | ArkUI 框架（83 页） |
| **application_model.md** | 应用模型（6 页） |
| **network.md** | 网络请求（1 页） |
| **data.md** | 数据存储（3 页） |
| **samples.md** | 示例代码（10 页） |
| **tools.md** | 工具文档 |
| **other.md** | 其他内容（10 页） |

## 使用指南

### 对于初学者
1. 从 `getting_started.md` 开始了解基础
2. 学习 ArkTS 基本语法
3. 掌握 ArkUI 常用组件
4. 实践页面跳转和状态管理
5. 学习数据持久化

### 对于进阶开发
- 深入学习 ArkTS 高级特性
- 掌握自定义组件开发
- 学习 Ability 生命周期管理
- 实现分布式能力

### 对于 UI 开发
- 学习 ArkUI 布局系统
- 掌握动画和转场效果
- 实现响应式布局
- 优化性能

## 资源

### references/
从官方来源提取的组织化文档，包含：
- 详细的功能说明
- 带语言标注的代码示例
- 原始文档链接
- 快速导航目录

### scripts/
添加用于常见自动化任务的辅助脚本。

### assets/
添加模板、样板代码或示例项目。

## 注意事项

- 此技能从官方文档自动生成
- ArkTS 基于 TypeScript，建议先掌握 TS 基础
- DevEco Studio 是官方推荐的开发工具
- 不同 HarmonyOS 版本可能有 API 差异

## 更新说明

刷新此技能的文档：
1. 使用相同配置重新运行爬虫
2. 技能将使用最新信息重建

## 相关资源

### 官方文档
- [HarmonyOS 官方网站](https://developer.huawei.com/consumer/cn/harmonyos/)
- [ArkTS 语言参考](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/arkts-get-started-V5)
- [ArkUI 开发指南](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/arkui-overview-V5)

### 开发工具
- [DevEco Studio 下载](https://developer.huawei.com/consumer/cn/deveco-studio/)
- [示例代码仓库](https://gitee.com/harmonyos/codelabs)

### 学习资源
- HarmonyOS 开发者文档
- 官方示例代码
- HarmonyOS 开发者论坛
