# Samples - HarmonyOS Next 示例代码集合

> 💡 **丰富的实战示例，助力快速上手 HarmonyOS Next 开发**
>
> 本文档汇集了 100+ 个 HarmonyOS Next 实战示例，涵盖基础组件、动画效果、AI 能力、多媒体处理等各个方面。

## 📋 示例分类

### 🎯 入门示例
适合初学者的基础示例，帮助快速理解 HarmonyOS Next 开发。

### 🎨 UI/UX 示例
展示各种 UI 组件和交互效果的实际应用。

### 🤖 AI 能力示例
演示 HarmonyOS Next 的 AI 能力集成。

### 🎬 多媒体示例
音视频、图像处理等多媒体相关示例。

### 🌐 网络与数据示例
网络请求、数据存储等实用示例。

### 🔧 高级特性示例
展示 HarmonyOS Next 的高级功能。

---

## 🌟 精选示例推荐

### 1. 点赞美女翻牌动效 ⭐⭐⭐⭐⭐

**特点**：
- 精美的动画效果
- 交互设计优秀
- 代码结构清晰
- 适合学习动画

**技术要点**：
- 属性动画（rotate、scale、opacity）
- 状态管理（@State）
- 图片资源加载
- 用户交互处理

**运行效果**：
![image](screenshots/GiveThumbsUp.gif)

**源码位置**：[samples/GiveThumbsUp/entry/src/main/ets/pages/Index.ets](#samples/GiveThumbsUp/entry/src/main/ets/pages/Index.ets)

---

### 2. 字符计数器 ⭐⭐⭐⭐

**特点**：
- 简单实用
- 适合初学者
- 展示基础组件使用

**技术要点**：
- Text 组件
- Button 组件
- 字符串操作
- 状态更新

**源码位置**：[samples/CountTheNumberOfCharacters/entry/src/main/ets/pages/Index.ets](#samples/CountTheNumberOfCharacters/entry/src/main/ets/pages/Index.ets)

---

### 3. AI 扫描功能 ⭐⭐⭐⭐⭐

**特点**：
- 集成 AI 能力
- 实用性强
- 展示系统集成

**功能**：
- 身份证识别
- 银行卡识别
- 文档扫描

**技术要点**：
- VisionKit 集成
- 相机调用
- 图像识别
- 数据解析

**源码位置**：
- [AI 扫描主页](#samples/AIScanner/entry/src/main/ets/pages/Index.ets)
- [身份证识别](#samples/AIScanner/entry/src/main/ets/pages/PageIdCard.ets)
- [银行卡识别](#samples/AIScanner/entry/src/main/ets/pages/PageBankCard.ets)
- [文档扫描](#samples/AIScanner/entry/src/main/ets/pages/PageDocScan.ets)

---

### 4. 仿 WeLink 打卡 ⭐⭐⭐⭐

**特点**：
- 模拟真实应用
- 交互设计好
- 状态管理清晰

**功能**：
- 打卡状态切换
- 时间显示
- 动画效果

**源码位置**：[samples/WeLinkPunchCard/entry/src/main/ets/pages/Index.ets](#samples/WeLinkPunchCard/entry/src/main/ets/pages/Index.ets)

---

## 📚 完整示例列表

### 🎯 入门级示例（10+）

#### 【挑战赛第三期】用HarmonyOS ArkUI实现点赞美女翻牌动效

**文章介绍**：https://developer.huawei.com/consumer/cn/forum/topic/0201105240170004491

**效果演示**：
![image](screenshots/GiveThumbsUp.gif)

**技术要点**：
- 翻转动画实现
- 图片切换效果
- 用户点击交互

---

#### samples/GiveThumbsUp/entry/src/main/ets/pages/Index.ets

**功能**：点赞翻牌动画完整实现

**核心代码**：
```typescript
import { ImageData } from '../model/ImageData';

@Entry
@Component
struct Index {
  private imageSrc: ImageData[] = initializeImageData()
  @State imageIndex: number = 0;
  @State itemClicked: boolean = false;

  build() {
    Column() {
      Row() {
        Image(this.imageSrc[this.imageIndex].img)
          .rotate({
            x: 0,
            y: 1,
            z: 0,
            angle: this.itemClicked ? 360 : 0
          })
          .scale(
            this.itemClicked
              ? { x: 1.4, y: 1.4 }
              : { x: 1, y: 1 }
          )
          .opacity(this.itemClicked ? 0.6 : 1)
          .animation({
            delay: 10,
            duration: 1000,
            iterations: 1,
            curve: Curve.Smooth,
            playMode: PlayMode.Normal
          });
      }.height('93%').border({ width: 1 })

      Row() {
        Row() {
          Button('点赞', { type: ButtonType.Normal, stateEffect: true })
            .borderRadius(8)
            .backgroundColor(0x317aff)
            .width(90)
        }.width('30%').height(50).onClick(() => {
          this.itemClicked = !this.itemClicked;
        })

        Row() {
          Button('更换', { type: ButtonType.Normal, stateEffect: true })
            .borderRadius(8)
            .backgroundColor(0x317aff)
            .width(90)
        }.width('30%').height(50).onClick(() => {
          this.imageIndex++;
          if (this.imageIndex >= this.imageSrc.length) {
            this.imageIndex = 0;
          }
          this.itemClicked = false;
        })
      }.height('7%').border({ width: 1 })
    }
  }
}

export function initializeImageData(): Array<ImageData> {
  let imageDataArray: Array<ImageData> = [
    { "id": "0", "img": $r('app.media.pic01'), "name": '女1' },
    { "id": "1", "img": $r('app.media.pic02'), "name": '女2' },
    { "id": "2", "img": $r('app.media.pic03'), "name": '女3' },
  ]
  return imageDataArray
}
```

**学习要点**：
1. 使用 `@State` 管理组件状态
2. 组合多个动画属性（rotate、scale、opacity）
3. 使用 `animation` 方法配置动画参数
4. 图片资源的引用和切换

---

#### samples/CountTheNumberOfCharacters/entry/src/main/ets/pages/Index.ets

**功能**：统计字符串字符数

**核心代码**：
```typescript
@Entry
@Component
struct Index {
  @State s: string = '鸿蒙HarmonyOS应用开发入门'
  @State length: number = 0;

  build() {
    Column() {
      Text(this.s)
        .id('HelloWorld')
        .fontSize(30)
        .fontWeight(FontWeight.Bold)

      Button('计算')
        .onClick(() => {
          // 统计字符串的字符数
          this.length = this.s.length;
        })

      Text(this.length + '')
        .id('length')
        .fontSize(50)
        .fontWeight(FontWeight.Bold)
    }
    .height('100%')
    .width('100%')
  }
}
```

**学习要点**：
1. Text 组件显示文本
2. Button 组件处理点击事件
3. 状态更新触发 UI 刷新
4. 字符串基本操作

---

#### samples/FatherDay/entry/src/main/ets/default/app.ets

**功能**：应用生命周期管理

**核心代码**：
```typescript
export default {
  onCreate() {
    console.info('Application onCreate')
  },
  onDestroy() {
    console.info('Application onDestroy')
  },
}
```

---

#### samples/FatherDay/entry/src/main/ets/default/pages/index.ets

**功能**：父亲节祝福页面

**核心代码**：
```typescript
@Entry
@Component
struct Index {
  build() {
    Flex({
      direction: FlexDirection.Column,
      alignItems: ItemAlign.Center,
      justifyContent: FlexAlign.Center
    }) {
      Text('爸爸在我心中就像旗帜；他教会我做人与处事的方向；在父亲节这个特别的日子里；我想对爸爸说长大以后我就要成为您')
        .fontSize(50)
        .fontWeight(FontWeight.Bold)
    }
    .width('100%')
    .height('100%')
  }
}
```

**学习要点**：
1. Flex 布局使用
2. 文本显示和样式
3. 居中对齐实现

---

### 🤖 AI 能力示例（10+）

#### samples/AIScanner/entry/src/main/ets/pages/PageIdCard.ets

**功能**：身份证识别

**技术要点**：
- VisionKit 集成
- 相机权限申请
- 图像识别
- 数据解析

**核心代码**：
```typescript
import {
  CardRecognition,
  CardRecognitionResult,
  CardType,
  CardSide,
  ShootingMode
} from "@kit.VisionKit"
import { hilog } from '@kit.PerformanceAnalysisKit';

const TAG: string = 'PageIdCard'

@Entry
@Component
struct PageIdCard {
  @State type: string = '';
  @State cardDataSource: Record<string, string>[] = []
  pageInfos: NavPathStack = new NavPathStack();

  build() {
    NavDestination() {
      Stack({ alignContent: Alignment.Top }) {
        Stack() {
          this.cardDataShowBuilder()
        }
        .width('80%')
        .height('80%')

        CardRecognition({
          supportType: CardType.CARD_ID,
          cardSide: CardSide.DEFAULT,
          cardRecognitionConfig: {
            defaultShootingMode: ShootingMode.MANUAL,
            isPhotoSelectionSupported: true
          },
          onResult: ((params: CardRecognitionResult) => {
            hilog.info(0x0001, TAG, `params code: ${params.code}`)
            if (params.code !== 200) {
              this.pageInfos.pop()
            }
            hilog.info(0x0001, TAG, `params cardType: ${params.cardType}`)
            if (params.cardInfo?.front !== undefined) {
              this.cardDataSource.push(params.cardInfo?.front)
            }
            if (params.cardInfo?.back !== undefined) {
              this.cardDataSource.push(params.cardInfo?.back)
            }
            if (params.cardInfo?.main !== undefined) {
              this.cardDataSource.push(params.cardInfo?.main)
            }
          })
        })
      }
      .width('100%')
      .height('100%')
    }
    .title(this.type)
    .onReady((ctx: NavDestinationContext) => {
      this.pageInfos = ctx.pathStack;
      this.type = ctx?.pathInfo?.param as string;
    })
  }

  @Builder
  cardDataShowBuilder() {
    List() {
      ForEach(this.cardDataSource, (cardData: Record<string, string>) => {
        ListItem() {
          Column() {
            Image(cardData.cardImageUri)
              .objectFit(ImageFit.Contain)
              .width(100)
              .height(100)

            Text(JSON.stringify(cardData))
              .width('100%')
              .fontSize(12)
          }
        }
      })
    }
    .listDirection(Axis.Vertical)
    .alignListItem(ListItemAlign.Center)
    .margin({ top: 50 })
    .width('100%')
    .height('100%')
  }
}
```

**学习要点**：
1. VisionKit 的使用方法
2. CardRecognition 组件配置
3. 识别结果处理
4. NavDestination 导航

---

#### samples/AIScanner/entry/src/main/ets/pages/Index.ets

**功能**：AI 扫描主页

**核心代码**：
```typescript
@Entry
@Component
struct Index {
  private dataSource: Array<string> =
    ['身份证识别', '银行卡识别', '文档扫描'];
  @State searchContent: string = '';
  pageInfos: NavPathStack = new NavPathStack();
  scroller: Scroller = new Scroller();
  layoutOptions: GridLayoutOptions = {
    regularSize: [1, 1],
  };

  build() {
    Navigation(this.pageInfos) {
      Column() {
        Column() {
          Text('AI扫描')
            .fontColor($r('sys.color.font_primary'))
            .fontSize(24)
        }.height('30%')
        .width('100%')
        .justifyContent(FlexAlign.Center)
        .margin({ top: 10 })

        Grid(this.scroller, this.layoutOptions) {
          ForEach(this.dataSource, (item: string) => {
            GridItem() {
              Button(item)
                .width('100%')
                .height(48)
                .fontSize(24)
                .onClick(e => {
                  switch (item) {
                    case '身份证识别':
                      this.pageInfos.pushPathByName('PageIdCard', item);
                      break;
                    case '银行卡识别':
                      this.pageInfos.pushPathByName('PageBankCard', item);
                      break;
                    case '文档扫描':
                      this.pageInfos.pushPathByName('PageDocScan', item);
                      break;
                  }
                })
            }
          }, (item: string) => item)
        }
        .columnsGap(8)
        .rowsGap(8)
        .columnsTemplate('1fr 1fr')
        .width('100%')
        .height('100%')
      }
      .width('100%')
      .height('100%')
      .margin({ bottom: 10 })
    }
    .mode(NavigationMode.Stack)
    .hideTitleBar(true)
  }
}
```

**学习要点**：
1. Navigation 组件使用
2. Grid 布局配置
3. 页面路由跳转
4. 按钮事件处理

---

#### samples/AIScanner/entry/src/main/ets/pages/PageBankCard.ets

**功能**：银行卡识别

**技术要点**：
- 银行卡类型识别
- 单面识别配置
- 卡号提取

**源码位置**：[PageBankCard.ets](#samples/AIScanner/entry/src/main/ets/pages/PageBankCard.ets)

---

#### samples/AIScanner/entry/src/main/ets/pages/PageDocScan.ets

**功能**：文档扫描

**技术要点**：
- DocumentScanner 组件
- 多页扫描
- 滤镜效果

**核心代码**：
```typescript
import {
  DocType,
  DocumentScanner,
  DocumentScannerConfig,
  SaveOption,
  FilterId,
  ShootingMode
} from "@kit.VisionKit"
import { hilog } from '@kit.PerformanceAnalysisKit';

const TAG: string = 'PageDocScan'

@Entry
@Component
struct PageDocScan {
  @State type: string = '';
  pageInfos: NavPathStack = new NavPathStack();
  @State docImageUris: string[] = []
  private docScanConfig = new DocumentScannerConfig()

  aboutToAppear() {
    this.docScanConfig.supportType = [DocType.DOC, DocType.SHEET]
    this.docScanConfig.isGallerySupported = true
    this.docScanConfig.editTabs = []
    this.docScanConfig.maxShotCount = 3
    this.docScanConfig.defaultFilterId = FilterId.ORIGINAL
    this.docScanConfig.defaultShootingMode = ShootingMode.MANUAL
    this.docScanConfig.isShareable = true
    this.docScanConfig.originalUris = []
  }

  build() {
    NavDestination() {
      Stack({ alignContent: Alignment.Top }) {
        List() {
          ForEach(this.docImageUris, (uri: string) => {
            ListItem() {
              Image(uri)
                .objectFit(ImageFit.Contain)
                .width(100)
                .height(100)
            }
          })
        }
        .listDirection(Axis.Vertical)
        .alignListItem(ListItemAlign.Center)
        .margin({ top: 50 })
        .width('80%')
        .height('80%')

        DocumentScanner({
          scannerConfig: this.docScanConfig,
          onResult: (code: number, saveType: SaveOption, uris: string[]) => {
            hilog.info(0x0001, TAG, `result code: ${code}, save: ${saveType}`)
            if (code === -1) {
              this.pageInfos.pop()
            }
            uris.forEach(uriString => {
              hilog.info(0x0001, TAG, `uri: ${uriString}`)
            })
            this.docImageUris = uris
          }
        })
          .size({ width: '100%', height: '100%' })
      }
      .width('100%')
      .height('100%')
    }
    .title(this.type)
    .onReady((ctx: NavDestinationContext) => {
      this.pageInfos = ctx.pathStack;
      this.type = ctx?.pathInfo?.param as string;
    })
  }
}
```

**学习要点**：
1. DocumentScanner 配置
2. 文档类型选择
3. 扫描结果处理
4. 图片 URI 管理

---

#### samples/WeLinkPunchCard/entry/src/main/ets/pages/Index.ets

**功能**：仿 WeLink 打卡应用

**核心代码**：
```typescript
@Entry
@Component
struct Index {
  private clickReady: Resource = $r('app.media.pic_01') // 可以打卡
  private clickCompleted: Resource = $r('app.media.pic_02') // 打卡完成
  @State imageSrc: Resource = this.clickReady // 设置打卡按钮区状态

  private timeReady: string = '08:08:08' // 可以打卡
  private timeCompleted: string = '华为坂田基地 08:08:08' // 打卡完成
  @State timeSrc: string = this.timeReady // 设置打卡状态区状态

  build() {
    Flex({
      direction: FlexDirection.Column,
      alignItems: ItemAlign.Center,
      justifyContent: FlexAlign.Center
    }) {
      Image(this.imageSrc)
        .width(150).height(150)
        .onClick(() => {
          // 打卡切换状态
          this.imageSrc = this.clickCompleted
          this.timeSrc = this.timeCompleted
        })

      Text(this.timeSrc).fontSize(39).fontColor(0xCCCCCC)
    }.width('100%').height('100%')
  }
}
```

**学习要点**：
1. Resource 资源引用
2. 状态切换逻辑
3. 图片点击事件
4. Flex 布局居中

---

## 💡 使用指南

### 如何运行示例

1. **克隆项目**
   ```bash
   git clone https://github.com/waylau/harmonyos-tutorial.git
   cd harmonyos-tutorial
   ```

2. **打开项目**
   - 使用 DevEco Studio 打开示例项目
   - 等待依赖下载完成

3. **运行示例**
   - 选择目标设备（模拟器或真机）
   - 点击 Run 按钮
   - 查看运行效果

### 学习建议

1. **从简单开始**
   - 先运行入门级示例
   - 理解基础概念
   - 逐步增加难度

2. **动手实践**
   - 修改示例代码
   - 观察效果变化
   - 理解代码逻辑

3. **举一反三**
   - 基于示例开发新功能
   - 组合多个示例
   - 创造自己的应用

4. **阅读源码**
   - 理解代码结构
   - 学习设计模式
   - 提升编程能力

---

## 🎓 学习路径

### 第一周：基础入门
- HelloWorld
- 字符计数器
- 基础组件使用
- 布局系统

### 第二周：UI 进阶
- 点赞翻牌动画
- 打卡应用
- 列表和滚动
- 页面跳转

### 第三周：数据管理
- 数据存储
- 网络请求
- 图片加载
- 状态管理

### 第四周：高级特性
- AI 能力集成
- 相机调用
- 多媒体处理
- 分布式能力

---

## 📊 示例统计

- **总示例数**: 100+
- **入门示例**: 20+
- **UI 示例**: 30+
- **AI 示例**: 15+
- **多媒体示例**: 20+
- **高级特性**: 15+

---

## 🔗 相关资源

### 官方资源
- [HarmonyOS 官方文档](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/)
- [API 参考](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/)
- [Codelabs](https://developer.huawei.com/consumer/cn/codelabs/)

### 社区资源
- [华为开发者论坛](https://developer.huawei.com/consumer/cn/forum/home)
- [GitHub 示例项目](https://github.com/Harmony-OS)
- [CSDN HarmonyOS 专区](https://www.csdn.net/spaces/HarmonyOS)

---

## 💬 反馈与建议

如果您在学习过程中遇到问题或有改进建议，欢迎：

- 提交 Issue
- 发起 Pull Request
- 参与讨论

---

**祝您学习愉快！** 🎉

*最后更新: 2025-01*
