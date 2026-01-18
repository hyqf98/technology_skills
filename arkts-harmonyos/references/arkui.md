# ArkUI 框架参考文档

> **HarmonyOS Next ArkUI 完整参考手册**
> **更新时间**: 2026-01-18
> **适用版本**: HarmonyOS Next API 12+
> **文档页数**: 83+ 页

## 📋 目录导航

### 快速导航
- [🚀 ArkUI 新特性](#arkui-新特性) - 最新组件和功能
- [🎨 核心组件](#核心组件) - 基础组件详解
- [🔧 高级特性](#高级特性) - 动画、手势、路由
- [💡 最佳实践](#最佳实践) - 性能优化和设计模式
- [🔨 自定义组件](#自定义组件) - 组件开发指南
- [📱 响应式布局](#响应式布局) - 多设备适配
- [🐛 常见问题](#常见问题) - 问题排查和解决方案
- [📚 完整示例](#完整示例) - 项目源码

### 主要内容分类
1. **基础组件** - Text、Image、Button、Input等
2. **容器组件** - Column、Row、Stack、List等
3. **媒体组件** - Video、Image、Web等
4. **画布组件** - Canvas、SVG等
5. **交互组件** - Dialog、ActionSheet、Toast等
6. **高级功能** - 动画、手势、路由、状态管理

---

## 🚀 ArkUI 新特性

### API 12+ 核心增强

#### 1. 新增组件

##### WaterFlow (瀑布流组件)
```typescript
@Entry
@Component
struct WaterFlowExample {
  @State dataSource: WaterFlowDataSource = new WaterFlowDataSource();

  build() {
    WaterFlow() {
      LazyForEach(this.dataSource, (item: WaterFlowItem) => {
        FlowItem() {
          Column() {
            Image(item.imageUrl)
              .width('100%')
              .objectFit(ImageFit.Cover)
              .borderRadius(8)

            Text(item.title)
              .fontSize(14)
              .margin({ top: 8 })
          }
          .padding(8)
          .backgroundColor(Color.White)
          .borderRadius(8)
        }
        .width('100%')
        .height(item.height)
      }, (item: WaterFlowItem) => item.id)
    }
    .columnsTemplate('1fr 1fr')  // 两列布局
    .columnsGap(10)
    .rowsGap(10)
    .padding(10)
    .backgroundColor('#f5f5f5')
  }
}
```

##### Search (搜索组件)
```typescript
@Entry
@Component
struct SearchExample {
  @State searchText: string = '';
  @State searchResults: string[] = [];

  build() {
    Column() {
      // 搜索框
      Search({ value: this.searchText, placeholder: '搜索内容' })
        .searchButton('搜索')
        .width('100%')
        .backgroundColor(Color.White)
        .onChange((value: string) => {
          this.searchText = value;
        })
        .onSubmit((value: string) => {
          this.performSearch(value);
        })

      // 搜索结果列表
      if (this.searchResults.length > 0) {
        List() {
          ForEach(this.searchResults, (result: string) => {
            ListItem() {
              Text(result)
                .width('100%')
                .padding(15)
                .backgroundColor(Color.White)
                .borderRadius(8)
                .margin({ bottom: 8 })
            }
          })
        }
        .margin({ top: 10 })
      } else if (this.searchText.length > 0) {
        Text('暂无搜索结果')
          .fontSize(16)
          .fontColor(Color.Gray)
          .margin({ top: 50 })
      }
    }
    .width('100%')
    .height('100%')
    .padding(15)
    .backgroundColor('#f5f5f5')
  }

  performSearch(query: string): void {
    // 模拟搜索
    this.searchResults = [`结果 1: ${query}`, `结果 2: ${query}`, `结果 3: ${query}`];
  }
}
```

#### 2. 动画增强

##### Spring Motion (弹性动画)
```typescript
@Entry
@Component
struct SpringAnimationExample {
  @State scale: number = 1;
  @State rotate: number = 0;

  build() {
    Column({ space: 20 }) {
      // 弹性缩放动画
      Column() {
        Text('弹性动画')
          .fontSize(20)
          .fontWeight(FontWeight.Bold)
      }
      .width(150)
      .height(150)
      .backgroundColor('#FF6B6B')
      .borderRadius(20)
      .justifyContent(FlexAlign.Center)
      .scale({ x: this.scale, y: this.scale })
      .rotate({ angle: this.rotate })
      .onClick(() => {
        // 使用弹性曲线
        animateTo({
          duration: 1000,
          curve: Curve.FastOutSlowIn,  // 弹性曲线
          onFinish: () => {
            this.scale = 1;
            this.rotate = 0;
          }
        }, () => {
          this.scale = 1.5;
          this.rotate = 360;
        })
      })

      Button('触发动画')
        .onClick(() => {
          animateTo({
            duration: 800,
            curve: Curve.SpringMotion  // 弹性运动曲线
          }, () => {
            this.scale = this.scale === 1 ? 1.2 : 1;
          })
        })
    }
    .width('100%')
    .height('100%')
    .justifyContent(FlexAlign.Center)
    .backgroundColor('#f0f0f0')
  }
}
```

##### 属性动画优化
```typescript
@Entry
@Component
struct PropertyAnimationExample {
  @State offsetX: number = 0;
  @State offsetY: number = 0;
  @State opacity: number = 1;

  build() {
    Column({ space: 20 }) {
      // 使用属性动画
      Column() {
        Text('滑动我')
          .fontSize(20)
          .fontWeight(FontWeight.Bold)
      }
      .width(150)
      .height(150)
      .backgroundColor('#4ECDC4')
      .borderRadius(20)
      .justifyContent(FlexAlign.Center)
      .translate({ x: this.offsetX, y: this.offsetY })
      .opacity(this.opacity)
      .gesture(
        // 拖拽手势
        PanGesture({ direction: PanDirection.All })
          .onActionStart(() => {
            // 动画开始
          })
          .onActionUpdate((event: GestureEvent) => {
            this.offsetX = event.offsetX;
            this.offsetY = event.offsetY;
          })
          .onActionEnd(() => {
            // 弹性回归动画
            animateTo({
              duration: 500,
              curve: Curve.SpringMotion
            }, () => {
              this.offsetX = 0;
              this.offsetY = 0;
            })
          })
      )

      // 透明度动画
      Button('淡入淡出')
        .onClick(() => {
          animateTo({
            duration: 1000,
            curve: Curve.EaseInOut
          }, () => {
            this.opacity = this.opacity === 1 ? 0.2 : 1;
          })
        })
    }
    .width('100%')
    .height('100%')
    .justifyContent(FlexAlign.Center)
    .backgroundColor('#f0f0f0')
  }
}
```

#### 3. 手势增强

##### 组合手势
```typescript
@Entry
@Component
struct CompositeGestureExample {
  @State scale: number = 1;
  @State angle: number = 0;
  @State offsetX: number = 0;
  @State offsetY: number = 0;

  build() {
    Column() {
      Column() {
        Text('多手势操作')
          .fontSize(20)
          .fontWeight(FontWeight.Bold)
      }
      .width(200)
      .height(200)
      .backgroundColor('#95E1D3')
      .borderRadius(20)
      .justifyContent(FlexAlign.Center)
      .scale({ x: this.scale, y: this.scale })
      .rotate({ angle: this.angle })
      .translate({ x: this.offsetX, y: this.offsetY })
      .gesture(
        // 组合手势：捏合 + 旋转 + 拖拽
        GestureGroup(GestureMode.Parallel,
          // 捏合手势（缩放）
          PinchGesture({ fingers: 2 })
            .onActionUpdate((event: GestureEvent) => {
              this.scale = event.scale;
            }),

          // 旋转手势
          RotationGesture({ fingers: 2 })
            .onActionUpdate((event: GestureEvent) => {
              this.angle = event.angle;
            }),

          // 拖拽手势
          PanGesture()
            .onActionUpdate((event: GestureEvent) => {
              this.offsetX = event.offsetX;
              this.offsetY = event.offsetY;
            })
        )
      )
    }
    .width('100%')
    .height('100%')
    .justifyContent(FlexAlign.Center)
    .backgroundColor('#f0f0f0')
  }
}
```

#### 4. 状态管理增强

##### @ObservedV2 和 @Trace
```typescript
// 使用新的状态管理装饰器
@ObservedV2
class UserProfile {
  @Trace name: string = '';  // 自动深度观察
  @Trace age: number = 0;
  @Trace avatar: string = '';

  @Computed get displayName(): string {  // 计算属性
    return `${this.name} (${this.age})`;
  }
}

@Entry
@Component
struct ProfilePage {
  @Local profile: UserProfile = new UserProfile();

  build() {
    Column({ space: 20 }) {
      Text(this.profile.displayName)
        .fontSize(24)
        .fontWeight(FontWeight.Bold)

      Image(this.profile.avatar)
        .width(100)
        .height(100)
        .borderRadius(50)

      Button('更新资料')
        .onClick(() => {
          this.profile.name = '新名字';
          this.profile.age = 25;
          // 自动触发UI更新
        })
    }
    .width('100%')
    .height('100%')
    .justifyContent(FlexAlign.Center)
  }
}
```

---

## 🎨 核心组件

### 基础组件详解

#### 1. Text (文本组件)
```typescript
@Entry
@Component
struct TextExample {
  build() {
    Column({ space: 15 }) {
      // 基础文本
      Text('基础文本')
        .fontSize(16)
        .fontColor(Color.Black)

      // 富文本
      Text('富文本示例')
        .fontSize(20)
        .fontWeight(FontWeight.Bold)
        .fontColor(Color.Blue)
        .decoration({
          type: TextDecorationType.Underline,
          color: Color.Red
        })

      // 文本截断
      Text('这是一段很长的文本，当超过最大宽度时会自动截断并显示省略号')
        .width(200)
        .maxLines(2)
        .textOverflow({ overflow: TextOverflow.Ellipsis })
        .fontSize(14)

      // 文本对齐
      Text('居中对齐文本')
        .width('100%')
        .textAlign(TextAlign.Center)
        .fontSize(16)
        .backgroundColor('#f0f0f0')
        .padding(10)

      // 文本阴影
      Text('带阴影的文本')
        .fontSize(24)
        .fontWeight(FontWeight.Bold)
        .fontColor(Color.White)
        .textShadow({
          radius: 5,
          color: Color.Gray,
          offsetX: 2,
          offsetY: 2
        })

      // 文本行高和字间距
      Text('多行文本\n支持行高设置\n和字间距调整')
        .fontSize(16)
        .lineHeight(30)
        .letterSpacing(2)
        .fontColor(Color.Black)

      // Span 组合文本
      Text() {
        Span('红色部分').fontColor(Color.Red).fontSize(18)
        Span(' + ').fontSize(16)
        Span('蓝色部分').fontColor(Color.Blue).fontSize(18)
        Span(' = ').fontSize(16)
        Span('紫色效果').fontColor(Color.Purple).fontSize(18).fontWeight(FontWeight.Bold)
      }
      .fontSize(16)
    }
    .width('100%')
    .height('100%')
    .padding(20)
    .backgroundColor('#fafafa')
  }
}
```

#### 2. Image (图片组件)
```typescript
@Entry
@Component
struct ImageExample {
  @State imageWidth: number = 200;
  @State imageHeight: number = 200;

  build() {
    Scroll() {
      Column({ space: 20 }) {
        // 基础图片加载
        Text('网络图片')
          .fontSize(16)
          .fontWeight(FontWeight.Bold)

        Image('https://example.com/image.jpg')
          .width(200)
          .height(200)
          .objectFit(ImageFit.Cover)
          .borderRadius(10)
          .onError(() => {
            console.error('图片加载失败');
          })
          .onComplete(() => {
            console.info('图片加载完成');
          })

        // 本地图片
        Text('本地资源')
          .fontSize(16)
          .fontWeight(FontWeight.Bold)

        Image($r('app.media.icon'))
          .width(100)
          .height(100)

        // 图片填充模式
        Text('不同填充模式')
          .fontSize(16)
          .fontWeight(FontWeight.Bold)

        Row({ space: 10 }) {
          Image($r('app.media.example'))
            .width(100)
            .height(100)
            .objectFit(ImageFit.Cover)
            .backgroundColor('#f0f0f0')

          Image($r('app.media.example'))
            .width(100)
            .height(100)
            .objectFit(ImageFit.Contain)
            .backgroundColor('#f0f0f0')

          Image($r('app.media.example'))
            .width(100)
            .height(100)
            .objectFit(ImageFit.Fill)
            .backgroundColor('#f0f0f0')
        }

        // 图片圆角和边框
        Text('圆角和边框')
          .fontSize(16)
          .fontWeight(FontWeight.Bold)

        Image($r('app.media.example'))
          .width(150)
          .height(150)
          .borderRadius(20)
          .border({ width: 3, color: Color.Blue })

        // 图片滤镜
        Text('图片滤镜效果')
          .fontSize(16)
          .fontWeight(FontWeight.Bold)

        Row({ space: 10 }) {
          Image($r('app.media.example'))
            .width(100)
            .height(100)
            .grayscale(1)  // 灰度

          Image($r('app.media.example'))
            .width(100)
            .height(100)
            .blur(10)  // 模糊

          Image($r('app.media.example'))
            .width(100)
            .height(100)
            .brightness(1.5)  // 亮度
        }

        // 动态调整图片大小
        Text('动态调整大小')
          .fontSize(16)
          .fontWeight(FontWeight.Bold)

        Image($r('app.media.example'))
          .width(this.imageWidth)
          .height(this.imageHeight)
          .objectFit(ImageFit.Cover)

        Slider({
          value: this.imageWidth,
          min: 50,
          max: 300,
          step: 10
        })
          .width('100%')
          .onChange((value) => {
            this.imageWidth = value;
            this.imageHeight = value;
          })
      }
      .width('100%')
      .padding(20)
    }
    .width('100%')
    .height('100%')
    .backgroundColor('#fafafa')
  }
}
```

#### 3. Button (按钮组件)
```typescript
@Entry
@Component
struct ButtonExample {
  @State buttonText: string = '点击我';
  @State clickCount: number = 0;

  build() {
    Scroll() {
      Column({ space: 15 }) {
        // 基础按钮
        Text('基础按钮样式')
          .fontSize(18)
          .fontWeight(FontWeight.Bold)

        Row({ space: 10 }) {
          Button('普通按钮')
            .onClick(() => {
              console.info('点击了普通按钮');
            })

          Button('胶囊按钮', { type: ButtonType.Capsule })
            .onClick(() => {
              console.info('点击了胶囊按钮');
            })

          Button('圆形按钮', { type: ButtonType.Circle })
            .width(60)
            .height(60)
        }

        // 不同状态按钮
        Text('按钮状态')
          .fontSize(18)
          .fontWeight(FontWeight.Bold)

        Row({ space: 10 }) {
          Button('正常状态')
            .enabled(true)

          Button('禁用状态')
            .enabled(false)
            .backgroundColor(Color.Gray)
        }

        // 自定义样式按钮
        Text('自定义样式')
          .fontSize(18)
          .fontWeight(FontWeight.Bold)

        Button('渐变按钮')
          .width(200)
          .height(50)
          .fontSize(18)
          .fontWeight(FontWeight.Bold)
          .linearGradient({
            angle: 90,
            colors: [['#FF6B6B', 0.0], ['#4ECDC4', 1.0]]
          })
          .onClick(() => {
            console.info('点击了渐变按钮');
          })

        // 图标按钮
        Text('图标按钮')
          .fontSize(18)
          .fontWeight(FontWeight.Bold)

        Row({ space: 20 }) {
          Button() {
            Row() {
              Image($r('app.media.icon_like'))
                .width(24)
                .height(24)
              Text('点赞')
                .fontSize(16)
                .margin({ left: 5 })
            }
          }
          .type(ButtonType.Capsule)
          .backgroundColor(Color.Red)
          .onClick(() => {
            console.info('点赞成功');
          })

          Button() {
            Image($r('app.media.icon_share'))
              .width(24)
              .height(24)
          }
          .type(ButtonType.Circle)
          .width(50)
          .height(50)
          .backgroundColor(Color.Blue)
          .onClick(() => {
            console.info('分享成功');
          })
        }

        // 加载状态按钮
        Text('加载状态')
          .fontSize(18)
          .fontWeight(FontWeight.Bold)

        Button() {
          if (this.clickCount > 0) {
            Row() {
              LoadingProgress()
                .width(20)
                .height(20)
                .color(Color.White)
              Text('加载中...')
                .fontSize(16)
                .margin({ left: 10 })
            }
          } else {
            Text('点击加载')
              .fontSize(16)
          }
        }
        .type(ButtonType.Capsule)
        .width(150)
        .height(45)
        .enabled(this.clickCount === 0)
        .onClick(() => {
          this.clickCount++;
          this.buttonText = '加载中...';
          // 模拟异步操作
          setTimeout(() => {
            this.clickCount = 0;
            this.buttonText = '点击我';
          }, 2000);
        })

        // 计数按钮
        Text('计数器')
          .fontSize(18)
          .fontWeight(FontWeight.Bold)

        Text(`点击次数: ${this.clickCount}`)
          .fontSize(16)
          .margin({ bottom: 10 })

        Button('点击计数')
          .type(ButtonType.Capsule)
          .width(150)
          .height(50)
          .onClick(() => {
            this.clickCount++;
          })
      }
      .width('100%')
      .padding(20)
    }
    .width('100%')
    .height('100%')
    .backgroundColor('#fafafa')
  }
}
```

### 容器组件详解

#### 1. Column 和 Row (布局容器)
```typescript
@Entry
@Component
struct LayoutExample {
  build() {
    Scroll() {
      Column({ space: 20 }) {
        // Column 垂直布局
        Text('Column 垂直布局')
          .fontSize(18)
          .fontWeight(FontWeight.Bold)

        Column({ space: 10 }) {
          Text('第一行')
            .width('100%')
            .height(50)
            .backgroundColor('#FF6B6B')
            .textAlign(TextAlign.Center)

          Text('第二行')
            .width('100%')
            .height(50)
            .backgroundColor('#4ECDC4')
            .textAlign(TextAlign.Center)

          Text('第三行')
            .width('100%')
            .height(50)
            .backgroundColor('#95E1D3')
            .textAlign(TextAlign.Center)
        }
        .width('100%')
        .padding(10)

        // Row 水平布局
        Text('Row 水平布局')
          .fontSize(18)
          .fontWeight(FontWeight.Bold)

        Row({ space: 10 }) {
          Text('左')
            .width(80)
            .height(50)
            .backgroundColor('#FF6B6B')
            .textAlign(TextAlign.Center)

          Text('中')
            .width(80)
            .height(50)
            .backgroundColor('#4ECDC4')
            .textAlign(TextAlign.Center)

          Text('右')
            .width(80)
            .height(50)
            .backgroundColor('#95E1D3')
            .textAlign(TextAlign.Center)
        }
        .width('100%')
        .padding(10)

        // 对齐方式示例
        Text('对齐方式')
          .fontSize(18)
          .fontWeight(FontWeight.Bold)

        Column({ space: 10 }) {
          // 顶部对齐
          Row() {
            Text('上').height(40).backgroundColor('#FF6B6B').textAlign(TextAlign.Center)
            Text('中').height(60).backgroundColor('#4ECDC4').textAlign(TextAlign.Center)
            Text('下').height(80).backgroundColor('#95E1D3').textAlign(TextAlign.Center)
          }
          .width('100%')
          .height(100)
          .backgroundColor('#f0f0f0')
          .alignItems(VerticalAlign.Top)  // 顶部对齐
          .justifyContent(FlexAlign.SpaceEvenly)  // 均匀分布
          .padding(10)

          // 居中对齐
          Row() {
            Text('上').height(40).backgroundColor('#FF6B6B').textAlign(TextAlign.Center)
            Text('中').height(60).backgroundColor('#4ECDC4').textAlign(TextAlign.Center)
            Text('下').height(80).backgroundColor('#95E1D3').textAlign(TextAlign.Center)
          }
          .width('100%')
          .height(100)
          .backgroundColor('#f0f0f0')
          .alignItems(VerticalAlign.Center)  // 居中对齐
          .justifyContent(FlexAlign.Center)  // 居中分布
          .padding(10)

          // 底部对齐
          Row() {
            Text('上').height(40).backgroundColor('#FF6B6B').textAlign(TextAlign.Center)
            Text('中').height(60).backgroundColor('#4ECDC4').textAlign(TextAlign.Center)
            Text('下').height(80).backgroundColor('#95E1D3').textAlign(TextAlign.Center)
          }
          .width('100%')
          .height(100)
          .backgroundColor('#f0f0f0')
          .alignItems(VerticalAlign.Bottom)  // 底部对齐
          .padding(10)
        }
        .width('100%')

        // 权重布局
        Text('权重布局')
          .fontSize(18)
          .fontWeight(FontWeight.Bold)

        Row() {
          Text('1/3')
            .layoutWeight(1)
            .height(50)
            .backgroundColor('#FF6B6B')
            .textAlign(TextAlign.Center)

          Text('1/3')
            .layoutWeight(1)
            .height(50)
            .backgroundColor('#4ECDC4')
            .textAlign(TextAlign.Center)

          Text('1/3')
            .layoutWeight(1)
            .height(50)
            .backgroundColor('#95E1D3')
            .textAlign(TextAlign.Center)
        }
        .width('100%')
        .padding(10)

        // 嵌套布局
        Text('嵌套布局')
          .fontSize(18)
          .fontWeight(FontWeight.Bold)

        Column() {
          Row() {
            Column() {
              Text('左上').width('100%').height(50).backgroundColor('#FF6B6B').textAlign(TextAlign.Center)
              Text('左下').width('100%').height(50).backgroundColor('#4ECDC4').textAlign(TextAlign.Center)
            }
            .layoutWeight(1)
            .height('100%')
            .space(5)

            Column() {
              Text('右上').width('100%').height(50).backgroundColor('#95E1D3').textAlign(TextAlign.Center)
              Text('右下').width('100%').height(50).backgroundColor('#DDA0DD').textAlign(TextAlign.Center)
            }
            .layoutWeight(1)
            .height('100%')
            .space(5)
          }
          .width('100%')
          .height(110)
          .space(5)
        }
        .width('100%')
        .padding(10)
      }
      .width('100%')
      .padding(20)
    }
    .width('100%')
    .height('100%')
    .backgroundColor('#fafafa')
  }
}
```

#### 2. Stack (堆叠容器)
```typescript
@Entry
@Component
struct StackExample {
  @State stackAlign: Alignment = Alignment.Center;

  build() {
    Column({ space: 20 }) {
      // 基础堆叠
      Text('基础堆叠布局')
        .fontSize(18)
        .fontWeight(FontWeight.Bold)

      Stack() {
        Text('底层')
          .width('100%')
          .height('100%')
          .backgroundColor('#FF6B6B')
          .textAlign(TextAlign.Center)
          .fontSize(20)

        Text('中层')
          .width(150)
          .height(150)
          .backgroundColor('#4ECDC4')
          .textAlign(TextAlign.Center)
          .fontSize(20)

        Text('顶层')
          .width(100)
          .height(100)
          .backgroundColor('#95E1D3')
          .textAlign(TextAlign.Center)
          .fontSize(20)
      }
      .width(200)
      .height(200)
      .backgroundColor('#f0f0f0')

      // 不同对齐方式
      Text('对齐方式选择')
        .fontSize(18)
        .fontWeight(FontWeight.Bold)

      Row({ space: 10 }) {
        Button('左上')
          .onClick(() => {
            this.stackAlign = Alignment.TopStart;
          })

        Button('居中')
          .onClick(() => {
            this.stackAlign = Alignment.Center;
          })

        Button('右下')
          .onClick(() => {
            this.stackAlign = Alignment.BottomEnd;
          })
      }

      Stack({ alignContent: this.stackAlign }) {
        // 背景图
        Image($r('app.media.background'))
          .width('100%')
          .height('100%')
          .objectFit(ImageFit.Cover)

        // 遮罩层
        Column() {
          Text('标题')
            .fontSize(24)
            .fontWeight(FontWeight.Bold)
            .fontColor(Color.White)

          Text('副标题')
            .fontSize(16)
            .fontColor(Color.White)
            .margin({ top: 10 })
        }
        .padding(20)
      }
      .width(300)
      .height(200)
      .backgroundColor('#f0f0f0')

      // 图片+文字堆叠
      Text('图片与文字堆叠')
        .fontSize(18)
        .fontWeight(FontWeight.Bold)

      Stack({ alignContent: Alignment.Bottom }) {
        Image($r('app.media.card'))
          .width(300)
          .height(180)
          .objectFit(ImageFit.Cover)
          .borderRadius(10)

        Column() {
          Text('卡片标题')
            .fontSize(18)
            .fontWeight(FontWeight.Bold)
            .fontColor(Color.White)

          Text('卡片描述文本')
            .fontSize(14)
            .fontColor(Color.White)
            .margin({ top: 5 })
        }
        .width('100%')
        .padding(15)
        .linearGradient({
          direction: GradientDirection.Bottom,
          colors: [['rgba(0,0,0,0)', 0.0], ['rgba(0,0,0,0.7)', 1.0]]
        })
      }
      .width(300)
      .height(180)
      .borderRadius(10)
    }
    .width('100%')
    .height('100%')
    .padding(20)
    .backgroundColor('#fafafa')
  }
}
```

#### 3. List (列表组件)
```typescript
interface ListItem {
  id: string;
  title: string;
  subtitle: string;
  icon: Resource;
}

@Entry
@Component
struct ListExample {
  @State items: ListItem[] = [
    { id: '1', title: '项目 1', subtitle: '描述文本 1', icon: $r('app.media.icon1') },
    { id: '2', title: '项目 2', subtitle: '描述文本 2', icon: $r('app.media.icon2') },
    { id: '3', title: '项目 3', subtitle: '描述文本 3', icon: $r('app.media.icon3') },
    { id: '4', title: '项目 4', subtitle: '描述文本 4', icon: $r('app.media.icon4') },
    { id: '5', title: '项目 5', subtitle: '描述文本 5', icon: $r('app.media.icon5') },
  ];

  build() {
    Column() {
      // 基础列表
      Text('基础列表')
        .fontSize(18)
        .fontWeight(FontWeight.Bold)
        .width('100%')
        .padding(15)

      List({ space: 10 }) {
        ForEach(this.items, (item: ListItem) => {
          ListItem() {
            Row({ space: 15 }) {
              Image(item.icon)
                .width(50)
                .height(50)
                .borderRadius(25)

              Column() {
                Text(item.title)
                  .fontSize(16)
                  .fontWeight(FontWeight.Medium)

                Text(item.subtitle)
                  .fontSize(14)
                  .fontColor(Color.Gray)
                  .margin({ top: 5 })
              }
              .alignItems(HorizontalAlign.Start)
            }
            .width('100%')
            .padding(15)
            .backgroundColor(Color.White)
            .borderRadius(8)
          }
        }, (item: ListItem) => item.id)
      }
      .width('100%')
      .padding({ left: 15, right: 15 })
      .backgroundColor('#f5f5f5')

      // 分组列表
      Text('分组列表')
        .fontSize(18)
        .fontWeight(FontWeight.Bold)
        .width('100%')
        .padding(15)
        .margin({ top: 20 })

      List() {
        ListItemGroup({ header: this.ListItemGroupHeader('第一组'), space: 10 }) {
          ForEach(this.items.slice(0, 2), (item: ListItem) => {
            ListItem() {
              Text(item.title)
                .width('100%')
                .height(50)
                .backgroundColor(Color.White)
                .textAlign(TextAlign.Center)
                .borderRadius(8)
            }
          }, (item: ListItem) => item.id)
        }

        ListItemGroup({ header: this.ListItemGroupHeader('第二组'), space: 10 }) {
          ForEach(this.items.slice(2, 4), (item: ListItem) => {
            ListItem() {
              Text(item.title)
                .width('100%')
                .height(50)
                .backgroundColor(Color.White)
                .textAlign(TextAlign.Center)
                .borderRadius(8)
            }
          }, (item: ListItem) => item.id)
        }
      }
      .width('100%')
      .padding({ left: 15, right: 15 })
      .backgroundColor('#f5f5f5')
    }
    .width('100%')
    .height('100%')
    .backgroundColor('#fafafa')
  }

  @Builder
  ListItemGroupHeader(title: string) {
    Column() {
      Text(title)
        .fontSize(14)
        .fontColor(Color.Gray)
        .width('100%')
        .padding({ top: 10, bottom: 10 })
    }
  }
}
```

---

## 💡 最佳实践

### 1. 性能优化建议

#### 使用 LazyForEach 优化长列表
```typescript
/**
 * LazyForEach 数据源接口
 */
interface IDataSource {
  totalCount(): number;
  getData(index: number): ListItemData;
  registerDataChangeListener(listener: DataChangeListener): void;
  unregisterDataChangeListener(listener: DataChangeListener): void;
}

/**
 * LazyForEach 数据源实现
 */
class ListDataSource implements IDataSource {
  private data: ListItemData[] = [];
  private listeners: DataChangeListener[] = [];

  constructor(data: ListItemData[]) {
    this.data = data;
  }

  totalCount(): number {
    return this.data.length;
  }

  getData(index: number): ListItemData {
    return this.data[index];
  }

  registerDataChangeListener(listener: DataChangeListener): void {
    this.listeners.push(listener);
  }

  unregisterDataChangeListener(listener: DataChangeListener): void {
    const index = this.listeners.indexOf(listener);
    if (index >= 0) {
      this.listeners.splice(index, 1);
    }
  }

  // 通知数据变化
  notifyDataReload(): void {
    this.listeners.forEach(listener => {
      listener.onDataReloaded();
    });
  }
}

@Entry
@Component
struct OptimizedListExample {
  private dataSource: ListDataSource = new ListDataSource([]);

  build() {
    List() {
      LazyForEach(this.dataSource, (item: ListItemData) => {
        ListItem() {
          OptimizedListItem({ item: item })
        }
      }, (item: ListItemData) => item.id)
    }
    .cachedCount(10)  // 缓存屏幕外的项
    .width('100%')
    .height('100%')
  }
}

@Component
struct OptimizedListItem {
  @Prop item: ListItemData;

  build() {
    Row() {
      Image(this.item.icon)
        .width(50)
        .height(50)
        .borderRadius(25)

      Text(this.item.title)
        .fontSize(16)
        .margin({ left: 10 })
    }
    .width('100%')
    .padding(10)
  }
}
```

#### 避免不必要的重建
```typescript
// ❌ 不好的做法：每次渲染都创建新对象
@Component
struct BadExample {
  build() {
    Column() {
      ForEach([1, 2, 3], (item: number) => {
        Text(`项目 ${item}`)
          .onClick(() => {
            console.info(`点击了 ${item}`);
          })
      })
    }
  }
}

// ✅ 好的做法：提取为独立组件
@Component
struct GoodExample {
  build() {
    Column() {
      ForEach([1, 2, 3], (item: number) => {
        ItemComponent({ value: item })
      })
    }
  }
}

@Component
struct ItemComponent {
  @Prop value: number;

  build() {
    Text(`项目 ${this.value}`)
      .onClick(() => {
        console.info(`点击了 ${this.value}`);
      })
  }
}
```

### 2. 响应式设计

#### 使用 Grid 和 GridItem
```typescript
@Entry
@Component
struct GridLayoutExample {
  @State items: string[] = Array.from({ length: 20 }, (_, i) => `项目 ${i + 1}`);

  build() {
    Grid() {
      ForEach(this.items, (item: string) => {
        GridItem() {
          Text(item)
            .fontSize(16)
            .backgroundColor('#4ECDC4')
            .borderRadius(8)
            .width('100%')
            .height('100%')
            .textAlign(TextAlign.Center)
        }
      })
    }
    .columnsTemplate('1fr 1fr 1fr')  // 三列布局
    .rowsTemplate('1fr 1fr 1fr')     // 三行布局
    .columnsGap(10)
    .rowsGap(10)
    .width('100%')
    .height('100%')
    .padding(10)
    .backgroundColor('#f5f5f5')
  }
}
```

#### 断点适配
```typescript
@Entry
@Component
struct BreakpointExample {
  @State currentBreakpoint: string = 'sm';

  aboutToAppear(): void {
    // 监听断点变化
    BreakpointSystem.getCurrentBreakPoint().then((breakpoint: string) => {
      this.currentBreakpoint = breakpoint;
    });

    BreakpointSystem.on('breakpoint', (breakpoint: string) => {
      this.currentBreakpoint = breakpoint;
    });
  }

  build() {
    if (this.currentBreakpoint === 'sm') {
      // 手机布局
      this.SmallLayout()
    } else if (this.currentBreakpoint === 'md') {
      // 平板布局
      this.MediumLayout()
    } else {
      // 桌面布局
      this.LargeLayout()
    }
  }

  @Builder
  SmallLayout() {
    Column() {
      Text('手机布局')
        .fontSize(24)
        .margin({ bottom: 20 })

      Column() {
        Text('内容 1')
        Text('内容 2')
        Text('内容 3')
      }
      .width('100%')
      .space(10)
    }
    .width('100%')
    .height('100%')
    .padding(20)
  }

  @Builder
  MediumLayout() {
    Row() {
      Column() {
        Text('平板布局')
          .fontSize(24)
          .margin({ bottom: 20 })

        Column() {
          Text('内容 1')
          Text('内容 2')
          Text('内容 3')
        }
        .space(10)
      }
      .layoutWeight(1)
    }
    .width('100%')
    .height('100%')
    .padding(20)
  }

  @Builder
  LargeLayout() {
    Row() {
      Column() {
        Text('桌面布局')
          .fontSize(24)
          .margin({ bottom: 20 })

        Column() {
          Text('内容 1')
          Text('内容 2')
          Text('内容 3')
        }
        .space(10)
      }
      .layoutWeight(1)

      Column() {
        Text('侧边栏')
          .fontSize(18)
          .margin({ bottom: 10 })

        Column() {
          Text('项目 1')
          Text('项目 2')
          Text('项目 3')
        }
        .space(10)
      }
      .width(200)
      .backgroundColor('#f0f0f0')
      .padding(10)
    }
    .width('100%')
    .height('100%')
  }
}
```

---

## 🔨 自定义组件

### 组件封装示例

#### 1. 自定义卡片组件
```typescript
/**
 * 自定义卡片组件
 * 支持标题、内容、操作按钮等配置
 */
interface CardConfig {
  title?: string;
  subtitle?: string;
  content?: string;
  icon?: Resource;
  showAction?: boolean;
  actionText?: string;
}

@Preview
@Component
export struct CustomCard {
  @Prop config: CardConfig = {};
  private onAction?: () => void;

  build() {
    Column() {
      // 卡片头部
      if (this.config.title || this.config.icon) {
        Row({ space: 10 }) {
          if (this.config.icon) {
            Image(this.config.icon)
              .width(40)
              .height(40)
              .borderRadius(20)
          }

          Column() {
            if (this.config.title) {
              Text(this.config.title)
                .fontSize(18)
                .fontWeight(FontWeight.Bold)
            }

            if (this.config.subtitle) {
              Text(this.config.subtitle)
                .fontSize(14)
                .fontColor(Color.Gray)
                .margin({ top: 5 })
            }
          }
          .alignItems(HorizontalAlign.Start)
          .layoutWeight(1)
        }
        .width('100%')
        .margin({ bottom: 15 })
      }

      // 卡片内容
      if (this.config.content) {
        Text(this.config.content)
          .fontSize(14)
          .fontColor('#333')
          .width('100%')
          .margin({ bottom: 15 })
      }

      // 操作按钮
      if (this.config.showAction && this.onAction) {
        Button(this.config.actionText || '确定')
          .width('100%')
          .type(ButtonType.Capsule)
          .onClick(() => {
            this.onAction?.();
          })
      }
    }
    .width('100%')
    .padding(20)
    .backgroundColor(Color.White)
    .borderRadius(12)
    .shadow({
      radius: 8,
      color: 'rgba(0, 0, 0, 0.1)',
      offsetX: 0,
      offsetY: 2
    })
  }
}

// 使用示例
@Entry
@Component
struct CustomCardExample {
  build() {
    Column({ space: 20 }) {
      CustomCard({
        title: '卡片标题',
        subtitle: '副标题',
        content: '这是卡片的内容文本',
        icon: $r('app.media.icon'),
        showAction: true,
        actionText: '了解更多'
      })
        .onAction(() => {
          console.info('点击了操作按钮');
        })
    }
    .width('100%')
    .height('100%')
    .padding(20)
    .backgroundColor('#f5f5f5')
  }
}
```

#### 2. 自定义加载组件
```typescript
/**
 * 自定义加载组件
 * 支持多种加载样式
 */
enum LoadingType {
  Default = 'default',
  Circle = 'circle',
  Wave = 'wave'
}

@Preview
@Component
export struct CustomLoading {
  @Prop loadingText: string = '加载中...';
  @Prop loadingType: LoadingType = LoadingType.Default;

  build() {
    Column({ space: 15 }) {
      if (this.loadingType === LoadingType.Default) {
        // 默认加载动画
        LoadingProgress()
          .width(50)
          .height(50)
          .color('#4ECDC4')
      } else if (this.loadingType === LoadingType.Circle) {
        // 圆形加载动画
        this.CircleLoading()
      } else if (this.loadingType === LoadingType.Wave) {
        // 波浪加载动画
        this.WaveLoading()
      }

      if (this.loadingText) {
        Text(this.loadingText)
          .fontSize(14)
          .fontColor(Color.Gray)
      }
    }
    .width('100%')
    .padding(20)
  }

  @Builder
  CircleLoading() {
    Stack() {
      Circle()
        .width(50)
        .height(50)
        .fill(Color.Transparent)
        .stroke('#4ECDC4')
        .strokeWidth(3)

      Circle()
        .width(50)
        .height(50)
        .fill(Color.Transparent)
        .stroke('#4ECDC4')
        .strokeWidth(3)
        .strokeDasharray([100, 200])
        .rotate({ angle: 90 })
    }
    .width(50)
    .height(50)
  }

  @Builder
  WaveLoading() {
    Row({ space: 5 }) {
      ForEach([1, 2, 3], (item: number) => {
        Column()
          .width(5)
          .height(20)
          .backgroundColor('#4ECDC4')
          .borderRadius(2.5)
      })
    }
  }
}
```

---

## 🐛 常见问题

### Q1: 如何解决列表性能问题？
```typescript
// 使用 LazyForEach 而不是 ForEach
// 添加合理的 cachedCount
// 使用唯一且稳定的 key
List() {
  LazyForEach(dataSource, (item) => {
    ListItem() {
      // 列表项内容
    }
  }, (item) => item.id)  // 使用唯一 ID 作为 key
}
.cachedCount(10)  // 根据实际需求调整
```

### Q2: 如何优化图片加载性能？
```typescript
// 1. 使用适当的图片格式和尺寸
Image('image.jpg')
  .width(200)
  .height(200)
  .objectFit(ImageFit.Cover)
  .onError(() => {
    // 加载失败时的处理
  })
  .onComplete(() => {
    // 加载完成后的处理
  })

// 2. 使用缓存
// 系统会自动缓存已加载的图片

// 3. 懒加载
List() {
  ForEach(items, (item) => {
    ListItem() {
      Image(item.imageUrl)
        .width(100)
        .height(100)
        .objectFit(ImageFit.Cover)
        .onAppear(() => {
          // 图片进入可视区域时才开始加载
        })
    }
  })
}
```

### Q3: 如何实现组件间通信？
```typescript
// 方式1: 使用 @Provide 和 @Consume
@Entry
@Component
struct Parent {
  @Provide sharedData: string = '共享数据';

  build() {
    Column() {
      Child()
    }
  }
}

@Component
struct Child {
  @Consume sharedData: string;

  build() {
    Text(this.sharedData)
  }
}

// 方式2: 使用回调函数
@Component
struct ChildWithCallback {
  @Prop onAction: (data: string) => void;

  build() {
    Button('点击')
      .onClick(() => {
        this.onAction('子组件数据');
      })
  }
}

// 方式3: 使用事件总线
// 参考前文 EventBus 实现
```

---

## 📚 完整示例

## HarmonyOS体验官【挑战赛第二期】用HarmonyOS ArkUI调用三方库PhotoView实现图片的联播、缩放

Source: harmonyos-tutorial/samples/ArkUIThirdPartyLibrary/README.md

# HarmonyOS体验官【挑战赛第二期】用HarmonyOS ArkUI调用三方库PhotoView实现图片的联播、缩放


## 文章介绍

<https://developer.huawei.com/consumer/cn/forum/topic/0202103760075502191>

## 效果演示

![](https://alliance-communityfile-drcn.dbankcdn.com/FileServer/getFile/cmtybbs/042/413/002/0000000000042413002.20221113220812.37632792635971837998206179952980:50531117153402:2800:CE2EEF07EEB8FA5B04AB310CD148CAE4A16FD4C0ED3F52A810C5ACE6D14A993E.gif)


---

## 用HarmonyOS ArkUI来开发一个健康饮食应用

Source: harmonyos-tutorial/samples/ArkUIHealthyDiet/README.md

# 用HarmonyOS ArkUI来开发一个健康饮食应用

## 介绍

https://developer.huawei.com/consumer/cn/forum/topic/0202103820939502206?fid=0101591351254000314

## 展示

![](https://alliance-communityfile-drcn.dbankcdn.com/FileServer/getFile/cmtybbs/042/413/002/0000000000042413002.20221115175417.06278100692190794783052837606006:50531114124406:2800:DD0F0B665DD616497E38E8AC76891262751C05A186C7666AE79AB49E2C62850F.gif)



kevinlau2011@qq.com+ArkUIHealthyDiet

https://gitee.com/openharmony-sig/contest/pulls/585

---

## 基于HarmonyOS ArkTS中秋国庆祝福程序

Source: harmonyos-tutorial/samples/ArkUIMidAutumnFestival/README.md

# 基于HarmonyOS ArkTS中秋国庆祝福程序



中秋、国庆双节将至，作为程序员，以代码之名，表达对于阖家团圆的祝福。本节将演示如何在基于HarmonyOS ArkUI的SwiperController、Image、Swiper等组件来实现节日祝福轮播程序。


## 效果演示

手机效果图如下：

![](screenshots/arkuimidautumnfestival.gif)





## 图文介绍

见：https://developer.huawei.com/consumer/cn/forum/topic/0201131193862897018?fid=0101591351254000314




---

## 七夕壁纸轮播

Source: harmonyos-tutorial/samples/ArkUIExpressingLove/README.md

# 七夕壁纸轮播



作为程序员，以代码之名，表达爱。本节将演示如何在基于HarmonyOS ArkUI的SwiperController、Image、Swiper等组件来实现七夕壁纸轮播。


## 效果演示

手机效果图如下：

![](screenshots/love.gif)



完整视频演示见：https://www.bilibili.com/video/BV1qh4y1T7dU/

## 图文介绍

见：https://developer.huawei.com/consumer/cn/forum/topic/0209128602919619570?fid=0101591351254000314




---

## 用HarmonyOS ArkUI抽个盲盒头像

Source: harmonyos-tutorial/samples/ArkUIExperience/README.md

# 用HarmonyOS ArkUI抽个盲盒头像

<https://developer.huawei.com/consumer/cn/forum/topic/0202103570335932166?fid=0101591351254000314>

---

## samples/ArkUIMediaComponents/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkUIMediaComponents/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkUIMediaComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkUIMediaComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkUIMediaComponents/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIMediaComponents/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {

  private currentVideoSrc: string = 'video_01.mp4'
  private currentPreviewUri: string = 'app.media.video_cover_01'
  @State videoSrc: Resource = $rawfile(this.currentVideoSrc)
  @State previewUri: Resource = $r(this.currentPreviewUri)
  @State curRate: PlaybackSpeed = PlaybackSpeed.Speed_Forward_1_00_X
  @State isAutoPlay: boolean = false
  @State showControls: boolean = true
  controller: VideoController = new VideoController()

  build() {
    Column() {
      Video({
        src: this.videoSrc,
        previewUri: this.previewUri,
        currentProgressRate: this.curRate,
        controller: this.controller
      })
        .width('100%')
        .height(600)
        .autoPlay(this.isAutoPlay)
        .controls(this.showControls)
        .onStart(() => {
          console.info('onStart')
        })
        .onPause(() => {
          console.info('onPause')
        })
        .onFinish(() => {
          console.info('onFinish')
        })
        .onError(() => {
          console.info('onError')
        })
        .onStop(() => {
          console.info('onStop')
        })
        .onPrepared((e?: DurationObject) => {
          if (e != undefined) {
            console.info('onPrepared is ' + e.duration)
          }
        })
        .onSeeking((e?: TimeObject) => {
          if (e != undefined) {
            console.info('onSeeking is ' + e.time)
          }
        })
        .onSeeked((e?: TimeObject) => {
          if (e != undefined) {
            console.info('onSeeked is ' + e.time)
          }
        })
        .onUpdate((e?: TimeObject) => {
          if (e != undefined) {
            console.info('onUpdate is ' + e.time)
          }
        })

      Row() {
        Button('src').onClick(() => {
          if (this.currentVideoSrc === 'video_01.mp4') {
            this.currentVideoSrc = 'video_00.mp4'
            this.currentPreviewUri = 'app.media.video_cover_00'
          } else {
            this.currentVideoSrc = 'video_01.mp4'
            this.currentPreviewUri = 'app.media.video_cover_01'
          }

          this.videoSrc = $rawfile(this.currentVideoSrc) // 切换视频源
          this.previewUri = $r(this.currentPreviewUri) // 切换视频预览海报
        }).margin(5)

        Button('controls').onClick(() => {
          this.showControls = !this.showControls // 切换是否显示视频控制栏
        }).margin(5)
      }

      Row() {
        Button('start').onClick(() => {
          this.controller.start() // 开始播放
        }).margin(2)
        Button('pause').onClick(() => {
          this.controller.pause() // 暂停播放
        }).margin(2)
        Button('stop').onClick(() => {
          this.controller.stop() // 结束播放
        }).margin(2)
        Button('reset').onClick(() => {
          this.controller.reset() // 重置AVPlayer
        }).margin(2)
        Button('setTime').onClick(() => {
          this.controller.setCurrentTime(10, SeekMode.Accurate) // 精准跳转到视频的10s位置
        }).margin(2)
      }

      Row() {
        Button('rate 0.75').onClick(() => {
          this.curRate = PlaybackSpeed.Speed_Forward_0_75_X // 0.75倍速播放
        }).margin(5)
        Button('rate 1').onClick(() => {
          this.curRate = PlaybackSpeed.Speed_Forward_1_00_X // 原倍速播放
        }).margin(5)
        Button('rate 2').onClick(() => {
          this.curRate = PlaybackSpeed.Speed_Forward_2_00_X // 2倍速播放
        }).margin(5)
      }
    }
  }
}

interface DurationObject {
  duration: number;
}

interface TimeObject {
  time: number;
}

---

## samples/ArkUISwiper/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkUISwiper/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkUISwiper/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkUISwiper/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkUISwiper/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUISwiper/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  private imageSrc: ImageData[] = initializeImageData()

  build() {
    Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
      Swiper() {
        ForEach(this.imageSrc, (item: ImageData) => {
          Image(item.img)
        }, (item: ImageData) => item.name)
      }.autoPlay(true) //自动轮播
    }
  }
}

export function initializeImageData(): Array<ImageData> {
  let imageDataArray: Array<ImageData> = [
    new ImageData( '0',  $r('app.media.pic01'),  '1024程序员节'),
    new ImageData( '1',  $r('app.media.pic02'),  '程序员日常'),
    new ImageData( '2',  $r('app.media.pic03'),  'HDC2021')
  ]
  return imageDataArray
}

export class ImageData {
  id: string
  img: Resource
  name: string

  constructor(id: string, img: Resource, name: string) {
    this.id = id // 图片唯一表示
    this.img = img // 图片资源
    this.name = name // 图片名称
  }
}

---

## samples/ArkUIWantOpenURI/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIWantOpenURI/entry/src/main/ets/pages/Index.ets

// 导入context
import context from '@ohos.application.context';
import wantConstant from '@ohos.ability.wantConstant';

@Entry
@Component
struct Index {
  @State message: string = 'Hello World'

  build() {
    Row() {
      Column() {
        // 添加按钮，启动Ability
        Button('启动')
          .fontSize(40)
          .onClick(this.implicitStartAbility) // 隐示启动Ability
      }
      .width('100%')
    }
    .height('100%')
  }

  // 隐示启动Ability
  async implicitStartAbility() {
    try {
      let want = {
        "action": "ohos.want.action.viewData", // 等同于ACTION_VIEW_DATA
        "entities": [ "entity.system.browsable" ], // 等同于ENTITY_BROWSABLE
        "uri": "https://www.test.com:8080/query/student",
        "type": "text/plain"
      }
      let context = getContext(this) as context.AbilityContext;
      await context.startAbility(want)
      console.info(`implicit start ability succeed`)
    } catch (error) {
      console.info(`implicit start ability failed with ${error.code}`)
    }
  }
}

---

## samples/EtsUISwiperAutoPlay/entry/src/main/ets/default/app.ets

Source: harmonyos-tutorial/samples/EtsUISwiperAutoPlay/entry/src/main/ets/default/app.ets

export default {
  onCreate() {
    console.info('Application onCreate')
  },
  onDestroy() {
    console.info('Application onDestroy')
  },
}


---

## samples/EtsUISwiperAutoPlay/entry/src/main/ets/default/model/imageData.ets

Source: harmonyos-tutorial/samples/EtsUISwiperAutoPlay/entry/src/main/ets/default/model/imageData.ets

export class ImageData {
  id: string
  img: Resource
  name: string

  constructor(id: string, img: Resource, name: string) {
    this.id = id // 图片唯一表示
    this.img = img // 图片资源
    this.name = name // 图片名称
  }
}


---

## samples/EtsUISwiperAutoPlay/entry/src/main/ets/default/pages/index.ets

Source: harmonyos-tutorial/samples/EtsUISwiperAutoPlay/entry/src/main/ets/default/pages/index.ets

import { ImageData } from '../model/imageData.ets';

@Entry
@Component
struct Index {
  private imageSrc: ImageData[] = initializeImageData()

  build() {
    Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
      Swiper() {
        ForEach(this.imageSrc, item => {
          Image(item.img)
        }, item => item.name)
      }.autoPlay(true) // 自动播放
    }
  }
}


export function initializeImageData(): Array<ImageData> {
  let imageDataArray: Array<ImageData> = [
    { "id": "0", "img": $r('app.media.pic01'), "name": '麻辣香锅' },
    { "id": "1", "img": $r('app.media.pic02'), "name": '香辣毛血旺' },
    { "id": "2", "img": $r('app.media.pic03'), "name": '关东煮' },
    { "id": "3", "img": $r('app.media.pic04'), "name": '菠萝咕噜肉' },
    { "id": "4", "img": $r('app.media.pic05'), "name": '可乐鸡翅' },
    { "id": "5", "img": $r('app.media.pic06'), "name": '宫保鸡丁' }
  ]
  return imageDataArray
}


---

## samples/EtsUISwiper/entry/src/main/ets/default/app.ets

Source: harmonyos-tutorial/samples/EtsUISwiper/entry/src/main/ets/default/app.ets

export default {
  onCreate() {
    console.info('Application onCreate')
  },
  onDestroy() {
    console.info('Application onDestroy')
  },
}


---

## samples/EtsUISwiper/entry/src/main/ets/default/pages/index.ets

Source: harmonyos-tutorial/samples/EtsUISwiper/entry/src/main/ets/default/pages/index.ets

@Entry
@Component
struct Index {
  private imageSrc: ImageData[] = initializeImageData()

  build() {
    Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
      Swiper() {
        ForEach(this.imageSrc, item => {
          Image(item.img)
        }, item => item.name)
      }
    }
  }
}

export function initializeImageData(): Array<ImageData> {
  let imageDataArray: Array<ImageData> = [
    { "id": "0", "img": $r('app.media.pic01'), "name": '1024程序员节' },
    { "id": "1", "img": $r('app.media.pic02'), "name": '程序员日常' },
    { "id": "2", "img": $r('app.media.pic03'), "name": 'HDC2021' },
  ]
  return imageDataArray
}

export class ImageData {
  id: string
  img: Resource
  name: string

  constructor(id: string, img: Resource, name: string) {
    this.id = id // 图片唯一表示
    this.img = img // 图片资源
    this.name = name // 图片名称
  }
}

---

## samples/ArkUIThirdPartyLibrary/entry/src/main/ets/view/OuterComponent.ets

Source: harmonyos-tutorial/samples/ArkUIThirdPartyLibrary/entry/src/main/ets/view/OuterComponent.ets

/*
 * Copyright (c) 2022 Huawei Device Co., Ltd.
 * Licensed under the Apache License,Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

import lottie from '@ohos/lottieETS';
import { Logger } from '../common/utils/log/logger';
import { CommonConstants } from '../common/constants/CommonConst';


@Component
export struct Outer {
  private renderingSettings: RenderingContextSettings = new RenderingContextSettings(true);
  private renderingContext: CanvasRenderingContext2D = new CanvasRenderingContext2D(this.renderingSettings);
  private animateName: string = CommonConstants.ANIMATE_NAME;
  private animateItem: any = null;
  @State canvasTitle: Resource = undefined;

  aboutToDisappear(): void {
    Logger.info(CommonConstants.OUTER_TAG, `aboutToDisappear`);
    lottie.destroy();
  }

  onPageShow(): void {
    Logger.info(CommonConstants.OUTER_TAG, `onPageShow`);
    lottie.play();
  }

  onPageHide(): void {
    Logger.info(CommonConstants.OUTER_TAG, `onPageShow`);
    lottie.pause();
  }

  build() {
    Flex({ direction: FlexDirection.Column, justifyContent: FlexAlign.SpaceBetween }) {

      // Canvas area
      Column() {
        Canvas(this.renderingContext)
          .width(CommonConstants.CONTAINER_WIDTH)
          .aspectRatio(CommonConstants.ASPECT_RATIO_176)
          .backgroundImage($r('app.media.canvasBg'))
          .backgroundImageSize(ImageSize.Cover)
          .onDisAppear(() => {
            lottie.destroy(this.animateName);
          })
        Text(this.canvasTitle)
          .width(CommonConstants.CONTAINER_WIDTH)
          .fontSize($r('app.float.fontSize_14'))
          .textAlign(TextAlign.Center)
          .fontWeight(FontWeight.Bold)
          .fontColor($r('app.color.outer_canvas_title'))
          .margin({ top: $r('app.float.default_12') })
          .opacity(CommonConstants.OPACITY_4)
      }
      .margin({
        top: $r('app.float.default_10'),
        left: $r('app.float.default_10'),
        right: $r('app.float.default_10')
      })

      // Buttons area
      Column({ space: CommonConstants.SPACE_12 }) {
        Button() {
          Text($r('app.string.outer_button_load'))
            .fontSize($r('app.float.fontSize_16'))
            .fontColor($r('app.color.outer_button_font'))
            .fontWeight(FontWeight.Bold)
        }
        .width(CommonConstants.CONTAINER_WIDTH)
        .height($r('app.float.default_40'))
        .backgroundColor($r('app.color.outer_button_bg'))
        .onClick(() => {
          this.canvasTitle = $r('app.string.outer_button_load');
          this.animateItem = lottie.loadAnimation({
            container: this.renderingContext,
            renderer: 'canvas',
            loop: 10,
            autoplay: true,
            name: this.animateName,
            path: 'common/lottie/data.json'
          });
        })

        Button() {
          Text($r('app.string.outer_button_end'))
            .fontSize($r('app.float.fontSize_16'))
            .fontColor($r('app.color.outer_button_font'))
            .fontWeight(FontWeight.Bold)
        }
        .width(CommonConstants.CONTAINER_WIDTH)
        .height($r('app.float.default_40'))
        .backgroundColor($r('app.color.outer_button_bg'))
        .onClick(() => {
          this.canvasTitle = $r('app.string.outer_button_end');
          this.animateItem.goToAndPlay(CommonConstants.ZERO_FRAME, true);
        })

        Flex({ justifyContent: FlexAlign.SpaceBetween }) {
          Button() {
            Text($r('app.string.outer_button_start'))
              .fontSize($r('app.float.fontSize_16'))
              .fontColor($r('app.color.outer_button_font'))
              .fontWeight(FontWeight.Bold)
          }
          .width(CommonConstants.CONTAINER_HALF_WIDTH)
          .height($r('app.float.default_40'))
          .backgroundColor($r('app.color.outer_button_bg'))
          .onClick(() => {
            this.canvasTitle = $r('app.string.outer_button_start');
            lottie.play();
          })

          Button() {
            Text($r('app.string.outer_button_pause'))
              .fontSize($r('app.float.fontSize_16'))
              .fontColor($r('app.color.outer_button_font'))
              .fontWeight(FontWeight.Bold)
          }
          .width(CommonConstants.CONTAINER_HALF_WIDTH)
          .height($r('app.float.default_40'))
          .backgroundColor($r('app.color.outer_button_bg'))
          .onClick(() => {
            this.canvasTitle = $r('app.string.outer_button_pause');
            lottie.pause();
          })
        }
      }
      .padding({
        left: $r('app.float.default_23'),
        right: $r('app.float.default_23'),
        bottom: $r('app.float.default_41')
      })
    }
    .height(CommonConstants.CONTAINER_HEIGHT)
  }
}

---

## samples/ArkUIThirdPartyLibrary/entry/src/main/ets/view/InnerComponent.ets

Source: harmonyos-tutorial/samples/ArkUIThirdPartyLibrary/entry/src/main/ets/view/InnerComponent.ets

/*
 * Copyright (c) 2022 Huawei Device Co., Ltd.
 * Licensed under the Apache License,Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

import { Buttons } from '@ohos/library';
import InnerViewModel from '../viewmodel/InnerViewModel'
import { ButtonList } from '../common/bean/ButtonList';
import { CommonConstants } from '../common/constants/CommonConst';

@Component
export struct Inner {
  @State buttonList: ButtonList[] = InnerViewModel.getButtonListData();
  scroller: Scroller = new Scroller();

  build() {
    Scroll(this.scroller) {
      Column({ space: CommonConstants.SPACE_12 }) {
        ForEach(this.buttonList, (item) => {
          Column() {
            Flex({
              direction: FlexDirection.Column,
              justifyContent: FlexAlign.SpaceBetween,
              alignItems: ItemAlign.Start
            }) {
              Column() {
                Text(item.title)
                  .height($r('app.float.default_21'))
                  .fontSize($r('app.float.fontSize_16'))
                  .fontColor($r('app.color.common_color'))
                  .fontWeight(FontWeight.Bold)
                Text(item.subtitle)
                  .height($r('app.float.default_16'))
                  .fontSize($r('app.float.fontSize_12'))
                  .fontColor($r('app.color.common_color'))
                  .fontWeight(CommonConstants.FONT_WEIGHT_400)
                  .margin({ top: $r('app.float.default_4') })
                  .opacity(CommonConstants.OPACITY_6)
              }
              .alignItems(HorizontalAlign.Start)

              Column() {
                Buttons({
                  buttonText: item.buttonText,
                  buttonShape: item.buttonShape,
                  buttonType: item.buttonType,
                  stateEffect: item.stateEffect,
                  fontColor: item.fontColor
                })
                  .alignSelf(ItemAlign.Center)
                  .margin({ bottom: $r('app.float.default_21') })
              }
              .width($r('app.float.default_260'))
              .height($r('app.float.default_90'))
              .backgroundImage($r('app.media.mobile'))
              .backgroundImageSize(ImageSize.Contain)
              .justifyContent(FlexAlign.End)
              .alignSelf(ItemAlign.Center)
              .align(Alignment.End)
            }
            .padding({
              bottom: $r('app.float.default_24')
            })
            .width(CommonConstants.CONTAINER_WIDTH)
            .height(CommonConstants.CONTAINER_HEIGHT)
          }
          .width(CommonConstants.CONTAINER_WIDTH)
          .aspectRatio(CommonConstants.ASPECT_RATIO_176)
          .padding({
            top: $r('app.float.default_12'),
            left: $r('app.float.default_8')
          })
          .backgroundColor($r('app.color.white'))
          .borderRadius($r('app.float.default_24'))
        })
      }
      .width(CommonConstants.CONTAINER_WIDTH)
      .padding({
        left: $r('app.float.default_12'),
        right: $r('app.float.default_12'),
        top: $r('app.float.default_12')
      })
    }
    .scrollable(ScrollDirection.Vertical)
    .scrollBar(BarState.Off)
    .margin({bottom: $r('app.float.default_24')})
  }
}

---

## samples/ArkUIThirdPartyLibrary/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIThirdPartyLibrary/entry/src/main/ets/pages/Index.ets

import {PhotoView} from '@ohos/photoview';

@Entry
@Component
struct Index {
  @State data: PhotoView.Model = new PhotoView.Model();
  @State data1: PhotoView.Model = new PhotoView.Model();
  @State data2: PhotoView.Model = new PhotoView.Model();
  private swiperController: SwiperController = new SwiperController()

  aboutToAppear() {
    this.data
      .setImageResource($r('app.media.harmony'))
    this.data1
      .setImageResource($r('app.media.harmony1'))
    this.data2
      .setImageResource($r('app.media.harmony2'))

  }

  build() {
    Column() {
      Swiper(this.swiperController) {
        PhotoView({model: this.data})
        PhotoView({model: this.data1})
        PhotoView({model: this.data2})
      }
      .index(0)
      .autoPlay(true) // 自动播放
      .indicator(true) // 默认开启指示点
      .loop(true) // 默认开启循环播放
      .duration(50)
      .vertical(true) // 默认横向切换
      .itemSpace(0)
      .onChange((index: number) => {
        this.data.resetMatrix()
        this.data1.resetMatrix()
        this.data2.resetMatrix()
        console.info("ViewPager"+index.toString())
      })
    }.height('100%').width('100%').backgroundColor(0x3d3d3d)
  }
}

---

## samples/ArkUIHealthyDiet/entry/src/main/ets/mock/MockData.ets

Source: harmonyos-tutorial/samples/ArkUIHealthyDiet/entry/src/main/ets/mock/MockData.ets

import { FoodInfo, CategoryId} from '../model/DataModels'

// 构造数据的mock数据
export let mockFoods: Array<FoodInfo> = [
  {
    id: 0,
    letter: 'Kiwi',
    name: $r('app.string.food_name_kiwi'),
    image: $r('app.media.kiwi'),
    categoryId: CategoryId.Fruit,
    calories: 61,
    protein: 0.8,
    fat: 0.6,
    carbohydrates: 14.5,
    vitaminC: 62
  },
  {
    id: 1,
    letter: 'Walnut',
    name: $r('app.string.food_name_walnut'),
    image: $r('app.media.walnut'),
    categoryId: CategoryId.Nut,
    calories: 646,
    protein: 14.9,
    fat: 58.8,
    carbohydrates: 19.1,
    vitaminC: 1.0
  },
  {
    id: 2,
    letter: 'Cucumber',
    name: $r('app.string.food_name_cucumber'),
    image: $r('app.media.cucumber'),
    categoryId: CategoryId.Vegetable,
    calories: 16,
    protein: 0.8,
    fat: 0.2,
    carbohydrates: 2.9,
    vitaminC: 9.0
  },
  {
    id: 3,
    letter: 'Blueberry',
    name: $r('app.string.food_name_blueberry'),
    image: $r('app.media.blueberry'),
    categoryId: CategoryId.Fruit,
    calories: 57,
    protein: 0.7,
    fat: 0.3,
    carbohydrates: 14.5,
    vitaminC: 9.7
  },
  {
    id: 4,
    letter: 'Crab',
    name: $r('app.string.food_name_crab'),
    image: $r('app.media.crab'),
    categoryId: CategoryId.Seafood,
    calories: 97,
    protein: 19,
    fat: 1.5,
    carbohydrates: 0,
    vitaminC: 7.6
  },
  {
    id: 5,
    letter: 'IceCream',
    name: $r('app.string.food_name_ice_cream'),
    image: $r('app.media.icecream'),
    categoryId: CategoryId.Dessert,
    calories: 150,
    protein: 3.5,
    fat: 11,
    carbohydrates: 24,
    vitaminC: 0.6
  },
  {
    id: 6,
    letter: 'Onion',
    name: $r('app.string.food_name_onion'),
    image: $r('app.media.onion'),
    categoryId: CategoryId.Vegetable,
    calories: 40,
    protein: 1.1,
    fat: 0.2,
    carbohydrates: 9,
    vitaminC: 8.0
  },
  {
    id: 7,
    letter: 'Mushroom',
    name: $r('app.string.food_name_mushroom'),
    image: $r('app.media.mushroom'),
    categoryId: CategoryId.Vegetable,
    calories: 20,
    protein: 3.1,
    fat: 0.3,
    carbohydrates: 3.3,
    vitaminC: 206
  },
  {
    id: 8,
    letter: 'Tomato',
    name: $r('app.string.food_name_tomato'),
    image: $r('app.media.tomato'),
    categoryId: CategoryId.Vegetable,
    calories: 15,
    protein: 0.9,
    fat: 0.2,
    carbohydrates: 3.3,
    vitaminC: 14.0
  },
  {
    id: 9,
    letter: 'Pitaya',
    name: $r('app.string.food_name_pitaya'),
    image: $r('app.media.pitaya'),
    categoryId: CategoryId.Fruit,
    calories: 55,
    protein: 1.1,
    fat: 0.2,
    carbohydrates: 13.3,
    vitaminC: 3.0
  },
  {
    id: 10,
    letter: 'Avocado',
    name: $r('app.string.food_name_avocado'),
    image: $r('app.media.avocado'),
    categoryId: CategoryId.Fruit,
    calories: 171,
    protein: 2.0,
    fat: 15.3,
    carbohydrates: 7.4,
    vitaminC: 8.0
  },
  {
    id: 11,
    letter: 'Strawberry',
    name: $r('app.string.food_name_strawberry'),
    image: $r('app.media.strawberry'),
    categoryId: CategoryId.Fruit,
    calories: 32,
    protein: 1.0,
    fat: 0.2,
    carbohydrates: 7.1,
    vitaminC: 47.0
  }
]


---

## samples/ArkUIHealthyDiet/entry/src/main/ets/model/DataUtil.ets

Source: harmonyos-tutorial/samples/ArkUIHealthyDiet/entry/src/main/ets/model/DataUtil.ets

import { FoodInfo } from './DataModels'
import { mockFoods } from '../mock/MockData'

export function getFoods(): Array<FoodInfo> {
  return mockFoods
}


---

## samples/ArkUIHealthyDiet/entry/src/main/ets/model/DataModels.ets

Source: harmonyos-tutorial/samples/ArkUIHealthyDiet/entry/src/main/ets/model/DataModels.ets

export enum CategoryId {
  Fruit = 0,
  Vegetable,
  Nut,
  Seafood,
  Dessert
}


export type FoodInfo = {
  id: number
  letter: string
  name: string | Resource
  image: Resource
  categoryId: CategoryId
  calories: number
  protein: number
  fat: number
  carbohydrates: number
  vitaminC: number
}


---

## samples/ArkUIHealthyDiet/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIHealthyDiet/entry/src/main/ets/pages/Index.ets

import { FoodInfo } from '../model/DataModels'
import { getFoods } from '../model/DataUtil'

@Component
struct FoodListItem {
  private foodItem: FoodInfo

  build() {
    // 增加路由导航
    Navigator({ target: 'pages/FoodDetail' }) {
      Flex({ justifyContent: FlexAlign.Start, alignItems: ItemAlign.Center }) {
        Image(this.foodItem.image)
          .objectFit(ImageFit.Contain)
          .height(40)
          .width(40)
          .backgroundColor('#FFf1f3f5')
          .margin({ right: 16 })
        Text(this.foodItem.name)
          .fontSize(14)
          .flexGrow(1)
        Text(this.foodItem.calories + ' kcal')
          .fontSize(14)
      }
      .height(64)
    }
    // 页面间数据传递
    .params({ foodInfo: this.foodItem })
    .margin({ right: 24, left:32 })
  }
}

@Entry
@Component
struct FoodList {
  private foodItems: FoodInfo[] = getFoods()

  build() {
    Column() {
      Flex({ justifyContent: FlexAlign.Start, alignItems: ItemAlign.Center }) {
        Text('Food List')
          .fontSize(20)
          .margin({ left: 20 })
      }
      .height('7%')
      .backgroundColor('#FFf1f3f5')

      List() {
        ForEach(this.foodItems, item => {
          ListItem() {
            FoodListItem({ foodItem: item })
          }
        }, item => item.id.toString())
      }
      .height('93%')
    }
  }
}


---

## samples/ArkUIHealthyDiet/entry/src/main/ets/pages/FoodDetail.ets

Source: harmonyos-tutorial/samples/ArkUIHealthyDiet/entry/src/main/ets/pages/FoodDetail.ets

import router from '@ohos.router'
import { FoodInfo } from '../model/DataModels'

@Component
struct PageTitle {
  build() {
    Flex({ alignItems: ItemAlign.Start }) {
      Image($r('app.media.back'))
        .width(21.8)
        .height(19.6)
      Text('Food Detail')
        .fontSize(21.8)
        .margin({left: 17.4})
    }
    .height(61)
    .backgroundColor('#FFedf2f5')
    .padding({ top: 13, bottom: 15, left: 28.3 })
    .onClick(() => {
      router.back()
    })
  }
}

@Component
struct FoodImageDisplay {
  private foodItem: FoodInfo
  build() {
    Stack({ alignContent: Alignment.BottomStart }) {
      Image(this.foodItem.image)
        .objectFit(ImageFit.Contain)
      Text(this.foodItem.name)
        .fontSize(26)
        .fontWeight(500)
        .margin({ left: 26, bottom: 17.4 })
    }
    .height(357)
    .backgroundColor('#FFedf2f5')
  }
}

@Component
struct ContentTable {
  private foodItem: FoodInfo

  @Builder IngredientItem(title:string, name: string, value: string) {
    Flex() {
      Text(title)
        .fontSize(17.4)
        .fontWeight(FontWeight.Bold)
        .layoutWeight(1)
      Flex() {
        Text(name)
          .fontSize(17.4)
          .flexGrow(1)
        Text(value)
          .fontSize(17.4)
      }
      .layoutWeight(2)
    }
  }

  build() {
    Flex({ direction: FlexDirection.Column, justifyContent: FlexAlign.SpaceBetween, alignItems: ItemAlign.Start }) {
      this.IngredientItem('Calories', 'Calories', this.foodItem.calories + 'kcal')
      this.IngredientItem('Nutrition', 'Protein', this.foodItem.protein + 'g')
      this.IngredientItem('', 'Fat', this.foodItem.fat + 'g')
      this.IngredientItem('', 'Carbohydrates', this.foodItem.carbohydrates + 'g')
      this.IngredientItem('', 'VitaminC', this.foodItem.vitaminC + 'mg')
    }
    .height(280)
    .padding({ top: 30, right: 30, left: 30 })
  }
}

@Entry
@Component
struct FoodDetail {
  private foodItem: FoodInfo = router.getParams()['foodInfo']

  build() {
    Column() {
      Stack( { alignContent: Alignment.TopStart }) {
        FoodImageDisplay({ foodItem: this.foodItem })
        PageTitle()
      }
      ContentTable({ foodItem: this.foodItem })
    }
    .alignItems(HorizontalAlign.Center)
  }
}


---

## samples/ArkUICanvasComponents/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkUICanvasComponents/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkUICanvasComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkUICanvasComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkUICanvasComponents/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUICanvasComponents/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  private renderingContextSettings: RenderingContextSettings = new RenderingContextSettings(true)

  //使用RenderingContext在Canvas组件上进行绘制，绘制对象可以是矩形、文本、图片等。
  private canvasRenderingContext2D: CanvasRenderingContext2D = new CanvasRenderingContext2D(this.renderingContextSettings)

  build() {
    Row() {
      Column() {
        Canvas(this.canvasRenderingContext2D)
          .width('100%')
          .height('100%')
            // onReady是Canvas组件初始化完成时的事件回调，该事件之后Canvas组件宽高确定且可获取
          .onReady(() => {
            // 绘制矩形。
            this.canvasRenderingContext2D.fillRect(0, 30, 100, 100)

            // 绘制贝赛尔曲线。
            this.canvasRenderingContext2D.beginPath()
            this.canvasRenderingContext2D.moveTo(170, 10)
            this.canvasRenderingContext2D.bezierCurveTo(20, 100, 200, 100, 200, 20)
            this.canvasRenderingContext2D.stroke()

            // 绘制渐变对象。
            let grad = this.canvasRenderingContext2D.createLinearGradient(150, 0, 300, 100)
            grad.addColorStop(0.0, 'red')
            grad.addColorStop(0.5, 'white')
            grad.addColorStop(1.0, 'green')
            this.canvasRenderingContext2D.fillStyle = grad
            this.canvasRenderingContext2D.fillRect(200, 0, 100, 100)
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}

---

## samples/ArkUIMidAutumnFestival/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIMidAutumnFestival/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  private swiperController: SwiperController = new SwiperController()


  build() {
    Column() {
      Swiper(this.swiperController) {
        Image($r('app.media.001'))

        Image($r('app.media.002'))

        Image($r('app.media.003'))
      }
      .index(0)
      .autoPlay(true) // 自动播放
      .indicator(true) // 默认开启指示点
      .loop(true) // 默认开启循环播放
      .duration(50)
      .vertical(true) // 默认横向切换
      .itemSpace(0)
    }.height('100%').width('100%').backgroundColor(0x3d3d3d)
  }
}


---

## samples/ArkUIPagesRouter/entry/src/main/ets/pages/Second.ets

Source: harmonyos-tutorial/samples/ArkUIPagesRouter/entry/src/main/ets/pages/Second.ets

// 导入router模块
import router from '@ohos.router';

@Entry
@Component
struct Second {
  @State message: string = 'Second页面'
  @State src: string = router.getParams()?.['src'];

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)

        // 显示传参的内容
        Text(this.src)
          .fontSize(30)

        // 添加按钮，触发返回
        Button('返回')
          .fontSize(40)
          .onClick(() => {
            router.back();
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}

---

## samples/ArkUIPagesRouter/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIPagesRouter/entry/src/main/ets/pages/Index.ets

// 导入router模块
import router from '@ohos.router';

@Entry
@Component
struct Index {
  @State message: string = 'Index页面'

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)

        // 添加按钮，触发跳转
        Button('跳转')
          .fontSize(40)
          .onClick(() => {
            router.push({
              url: 'pages/Second',
              params: {
                src: 'Index页面传来的数据',
              }
            });
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}

---

## samples/ArkUIExpressingLove/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIExpressingLove/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  private swiperController: SwiperController = new SwiperController()


  build() {
    Column() {
      Swiper(this.swiperController) {
        Image($r('app.media.001'))

        Image($r('app.media.002'))

        Image($r('app.media.003'))
      }
      .index(0)
      .autoPlay(true) // 自动播放
      .indicator(true) // 默认开启指示点
      .loop(true) // 默认开启循环播放
      .duration(50)
      .vertical(true) // 默认横向切换
      .itemSpace(0)
    }.height('100%').width('100%').backgroundColor(0x3d3d3d)
  }
}

---

## samples/ArkUIWeChat/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkUIWeChat/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkUIWeChat/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkUIWeChat/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkUIWeChat/entry/src/main/ets/model/CommonStyle.ets

Source: harmonyos-tutorial/samples/ArkUIWeChat/entry/src/main/ets/model/CommonStyle.ets

import {WE_CHAT_COLOR} from './WeChatData'
import router from '@system.router';

@Component
export struct WeChatTitle {
  private text: string = "";

  build() {
    Flex({ alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
      Text(this.text).fontSize('18fp').padding('20px')
    }.height('120px').backgroundColor(WE_CHAT_COLOR)
  }
}

@Component
export struct ChatItemStyle {
  weChatImage: string = "";
  weChatName: string = "";
  chatInfo: string = "";
  time: string = "";

  build() {
    Column() {
      Flex({ alignItems: ItemAlign.Center, justifyContent: FlexAlign.Start }) {
        Image($rawfile(this.weChatImage)).width('120px').height('120px').margin({ left: '50px', right: "50px" })

        Column() {
          Text(this.weChatName).fontSize('16fp')
          Text(this.chatInfo).fontSize('12fp').width('620px').fontColor("#c2bec2").maxLines(1)
        }.alignItems(HorizontalAlign.Start).flexGrow(1)

        Text(this.time).fontSize('12fp')
          .margin({ right: "50px" }).fontColor("#c2bec2")

      }
      .height('180px')
      .width('100%')

      Row() {
        Text().width('190px').height('3px')
        Divider()
          .vertical(false)
          .color(WE_CHAT_COLOR)
          .strokeWidth('3px')
      }
      .height('3px')
      .width('100%')
    }

  }
}

@Component
export struct ContactItemStyle {
  private imageSrc: string = "";
  private text: string = "";

  build() {
    Column() {
      Flex({ alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
        Image($rawfile(this.imageSrc)).width('100px').height('100px').margin({ left: '50px' })
        Text(this.text).fontSize('15vp').margin({ left: '40px' }).flexGrow(1)
      }
      .height('150px')
      .width('100%')

      Row() {
        Text().width('190px').height('3px')
        Divider()
          .vertical(false)
          .color(WE_CHAT_COLOR)
          .strokeWidth('3px')
      }
      .height('3px')
      .width('100%')
    }
  }
}

@Component
export struct WeChatItemStyle {
  private imageSrc: string = "";
  private text: string = "";
  private arrow: string = "arrow.png"

  build() {
    Column() {
      Flex({ alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
        Image($rawfile(this.imageSrc)).width('75px').height('75px').margin({ left: '50px' })
        Text(this.text).fontSize('15vp').margin({ left: '40px' }).flexGrow(1)
        Image($rawfile(this.arrow))
          .margin({ right: '40px' })
          .width('75px')
          .height('75px')
      }
      .height('150px')
      .width('100%')
    }.onClick(() => {
      if (this.text === "视频号") {
        router.push({ uri: 'pages/VideoPage' })
      }
    })
  }
}

@Component
export struct MyDivider {
  private style: string = ""

  build() {
    Row() {
      Divider()
        .vertical(false)
        .color(WE_CHAT_COLOR)
        .strokeWidth(this.style == "1" ? '3px' : '23px')
    }
    .height(this.style == "1" ? '3px' : '23px')
    .width('100%')
  }
}

---

## samples/ArkUIWeChat/entry/src/main/ets/model/WeChatData.ets

Source: harmonyos-tutorial/samples/ArkUIWeChat/entry/src/main/ets/model/WeChatData.ets

import { Person } from './Person'

export const CONTACTS: Person[] = [
  {
    id: 1,
    weChatImage: "person (1).jpg",
    weChatName: "苏轼",
    chatInfo: "大江东去，浪淘尽，千古风流人物。",
    time: "18:30"
  },
  {
    id: 2,
    weChatImage: "person (2).jpg",
    weChatName: "子由",
    chatInfo: "丙辰中秋，欢饮达旦，大醉，作此篇，兼怀子由。",
    time: "17:29"
  },
  {
    id: 3,
    weChatImage: "person (3).jpg",
    weChatName: "擎苍",
    chatInfo: "老夫聊发少年狂，左牵黄，右擎苍，锦帽貂裘，千骑卷平冈。",
    time: "17:28"
  },
  {
    id: 4,
    weChatImage: "person (4).jpg",
    weChatName: "春水",
    chatInfo: "春未老，风细柳斜斜。试上超然台上望，半壕春水一城花。",
    time: "16:27"
  },
  {
    id: 5,
    weChatImage: "person (5).jpg",
    weChatName: "夜曲",
    chatInfo: "为你弹奏萧邦的夜曲，纪念我死去的爱情",
    time: "15:26"
  },
  {
    id: 6,
    weChatImage: "person (6).jpg",
    weChatName: "不思量",
    chatInfo: "十年生死两茫茫，不思量，自难忘。千里孤坟，无处话凄凉",
    time: "14:25"
  },
  {
    id: 7,
    weChatImage: "person (7).jpg",
    weChatName: "千里之外",
    chatInfo: "我送你离开千里之外你无声黑白",
    time: "13:24"
  },
  {
    id: 8,
    weChatImage: "person (8).jpg",
    weChatName: "思量",
    chatInfo: "似花还似非花，也无人惜从教坠。抛家傍路，思量却是，无情有思。",
    time: "11:23"
  },
  {
    id: 9,
    weChatImage: "person (9).jpg",
    weChatName: "园游会",
    chatInfo: "我顶着大太阳，只想为你撑伞",
    time: "10:22"
  },
  {
    id: 10,
    weChatImage: "person (10).jpg",
    weChatName: "沙湖",
    chatInfo: "三月七日，沙湖道中遇雨。",
    time: "10:21"
  },
  {
    id: 11,
    weChatImage: "person (11).jpg",
    weChatName: "燕子",
    chatInfo: "花褪残红青杏小。燕子飞时，绿水人家绕。枝上柳绵吹又少。",
    time: "10:20"
  },
  {
    id: 12,
    weChatImage: "person (12).jpg",
    weChatName: "衣巾落枣",
    chatInfo: "簌簌衣巾落枣花，村南村北响缲车。牛衣古柳卖黄瓜。",
    time: "10:19"
  },
  {
    id: 13,
    weChatImage: "person (13).jpg",
    weChatName: "七里香",
    chatInfo: "雨下整夜我的爱溢出就像雨水",

    time: "10:18"
  },
  {
    id: 14,
    weChatImage: "person (14).jpg",
    weChatName: "淡烟",
    chatInfo: "细雨斜风作晓寒，淡烟疏柳媚晴滩。",
    time: "10:17"
  },
  {
    id: 15,
    weChatImage: "person (15).jpg",
    weChatName: "大梦",
    chatInfo: "世事一场大梦，人生几度秋凉。",
    time: "10:16"
  }
]

export const WE_CHAT_COLOR: string = "#ededed"

---

## samples/ArkUIWeChat/entry/src/main/ets/model/Person.ets

Source: harmonyos-tutorial/samples/ArkUIWeChat/entry/src/main/ets/model/Person.ets

export class Person {
  id: number = 0;
  weChatImage: string = "";
  weChatName: string = "";
  chatInfo: string = "";
  time: string = "";
}

---

## samples/ArkUIWeChat/entry/src/main/ets/pages/ContactPage.ets

Source: harmonyos-tutorial/samples/ArkUIWeChat/entry/src/main/ets/pages/ContactPage.ets

import {ContactItemStyle, WeChatTitle} from '../model/CommonStyle'
import {Person} from '../model/Person'
import { CONTACTS, WE_CHAT_COLOR} from '../model/WeChatData'

@Component
export struct ContactPage {
  build() {
    Column() {
      // 标题
      WeChatTitle({ text: "通讯录" })

      // 列表
      Scroll() {
        Column() {
          // 固定列表
          ContactItemStyle({ imageSrc: "new_friend.png", text: "新的朋友" })
          ContactItemStyle({ imageSrc: "group.png", text: "群聊" })
          ContactItemStyle({ imageSrc: "biaoqian.png", text: "标签" })
          ContactItemStyle({ imageSrc: "gonzh.png", text: "公众号" })

          // 企业联系人
          Text("      我的企业及企业联系人").fontSize('12fp').backgroundColor(WE_CHAT_COLOR).height('80px').width('100%')
          ContactItemStyle({ imageSrc: "qiye.png", text: "企业微信联系人" })

          // 微信好友
          Text("      我的微信好友").fontSize('12fp').backgroundColor(WE_CHAT_COLOR).height('80px').width('100%')
          List() {
            ForEach(CONTACTS, (item: Person) => {
              ListItem() {
                ContactItemStyle({ imageSrc: item.weChatImage, text: item.weChatName })
              }
            }, (item: Person) => item.id.toString())
          }
        }
      }

    }.alignItems(HorizontalAlign.Start)
    .width('100%')
    .height('100%')
  }
}

---

## samples/ArkUIWeChat/entry/src/main/ets/pages/DiscoveryPage.ets

Source: harmonyos-tutorial/samples/ArkUIWeChat/entry/src/main/ets/pages/DiscoveryPage.ets

import {WeChatItemStyle, MyDivider, WeChatTitle} from '../model/CommonStyle'

@Component
export struct DiscoveryPage {
  build() {
    Column() {
      // 标题
      WeChatTitle({ text: "发现" })

      // 列表
      WeChatItemStyle({ imageSrc: "moments.png", text: "朋友圈" })
      MyDivider()

      WeChatItemStyle({ imageSrc: "shipinghao.png", text: "视频号" })
      MyDivider({ style: '1' })
      WeChatItemStyle({ imageSrc: "zb.png", text: "直播" })
      MyDivider()

      WeChatItemStyle({ imageSrc: "sys.png", text: "扫一扫" })
      MyDivider({ style: '1' })
      WeChatItemStyle({ imageSrc: "yyy.png", text: "摇一摇" })
      MyDivider()

      WeChatItemStyle({ imageSrc: "kyk.png", text: "看一看" })
      MyDivider({ style: '1' })
      WeChatItemStyle({ imageSrc: "souyisou.png", text: "搜一搜" })
      MyDivider()

      WeChatItemStyle({ imageSrc: "fujin.png", text: "附近" })
      MyDivider()

      WeChatItemStyle({ imageSrc: "gw.png", text: "购物" })
      MyDivider({ style: '1' })
      WeChatItemStyle({ imageSrc: "game.png", text: "游戏" })
      MyDivider()

      WeChatItemStyle({ imageSrc: "xcx.png", text: "小程序" })
      MyDivider()
    }.alignItems(HorizontalAlign.Start)
    .width('100%')
    .height('100%')
  }
}


---

## samples/ArkUIWeChat/entry/src/main/ets/pages/ChatPage.ets

Source: harmonyos-tutorial/samples/ArkUIWeChat/entry/src/main/ets/pages/ChatPage.ets

import {ChatItemStyle, WeChatTitle} from '../model/CommonStyle'
import { CONTACTS} from '../model/WeChatData'
import {Person} from '../model/Person'

@Component
export struct ChatPage {
  build() {
    Column() {
      // 标题
      WeChatTitle({ text: "微信" })

      // 列表
      List() {
        ForEach(CONTACTS, (item: Person) => {
          ListItem() {
            ChatItemStyle({
              weChatImage: item.weChatImage,
              weChatName: item.weChatName,
              chatInfo: item.chatInfo,
              time: item.time
            })
          }
        }, (item: Person) => item.id.toString())
      }
      .height('100%')
      .width('100%')
    }
  }
}

---

## samples/ArkUIWeChat/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIWeChat/entry/src/main/ets/pages/Index.ets

import { ChatPage } from './ChatPage'
import { ContactPage } from './ContactPage'
import { DiscoveryPage } from './DiscoveryPage'
import { MyPage } from './MyPage'

@Entry
@Component
struct Index {
  @Provide currentPage: number = 0
  @State currentIndex: number = 0;

  build() {
    Column() {
      Tabs({
        index: this.currentIndex,
        barPosition: BarPosition.End
      }) {
        TabContent() {
          ChatPage()
        }
        .tabBar(this.TabBuilder('微信', 0, $r('app.media.wechat2'), $r('app.media.wechat1')))

        TabContent() {
          ContactPage()
        }
        .tabBar(this.TabBuilder('联系人', 1, $r('app.media.contacts2'), $r('app.media.contacts1')))

        TabContent() {
          DiscoveryPage()
        }
        .tabBar(this.TabBuilder('发现', 2, $r('app.media.find2'), $r('app.media.find1')))

        TabContent() {
          MyPage()
        }
        .tabBar(
          this.TabBuilder('我', 3, $r('app.media.me2'), $r('app.media.me1'))
        )
      }
      .barMode(BarMode.Fixed)
      .onChange((index: number) => {
        this.currentIndex = index;
      })
    }
  }

  @Builder TabBuilder(title: string, targetIndex: number, selectedImg: Resource, normalImg: Resource) {
    Column() {
      Image(this.currentIndex === targetIndex ? selectedImg : normalImg)
        .size({ width: 25, height: 25 })
      Text(title)
        .fontColor(this.currentIndex === targetIndex ? '#1698CE' : '#6B6B6B')
    }
    .width('100%')
    .height(50)
    .justifyContent(FlexAlign.Center)
  }
}

@Component
struct HomeTopPage {
  @Consume currentPage: number

  build() {
    Swiper() {
      ChatPage()
      ContactPage()
      DiscoveryPage()
      MyPage()
    }
    .onChange((index: number) => {
      this.currentPage = index
    })
    .index(this.currentPage)
    .loop(false)
    .indicator(false)
    .width('100%')
    .height('100%')
  }
}


---

## samples/ArkUIWeChat/entry/src/main/ets/pages/MyPage.ets

Source: harmonyos-tutorial/samples/ArkUIWeChat/entry/src/main/ets/pages/MyPage.ets

import {WeChatItemStyle, MyDivider} from '../model/CommonStyle'

@Component
export struct MyPage {
  private imageTitle: string = "title.png"

  build() {
    Column() {
      // 用户信息部分
      Image($rawfile(this.imageTitle)).height(144).width('100%')

      // 列表
      WeChatItemStyle({ imageSrc: "pay.png", text: "服务" })
      MyDivider()

      WeChatItemStyle({ imageSrc: "favorites.png", text: "收藏" })
      MyDivider({ style: '1' })
      WeChatItemStyle({ imageSrc: "moments2.png", text: "朋友圈" })
      MyDivider({ style: '1' })
      WeChatItemStyle({ imageSrc: "video.png", text: "视频号" })
      MyDivider({ style: '1' })
      WeChatItemStyle({ imageSrc: "card.png", text: "卡包" })
      MyDivider({ style: '1' })
      WeChatItemStyle({ imageSrc: "emoticon.png", text: "表情" })
      MyDivider()

      WeChatItemStyle({ imageSrc: "setting.png", text: "设置" })
      MyDivider()
    }.alignItems(HorizontalAlign.Start)
    .width('100%')
    .height('100%')
  }
}


---

## samples/ArkUIContainerComponents/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkUIContainerComponents/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkUIContainerComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkUIContainerComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkUIContainerComponents/entry/src/main/ets/pages/NavigatorExample.ets

Source: harmonyos-tutorial/samples/ArkUIContainerComponents/entry/src/main/ets/pages/NavigatorExample.ets

@Entry
@Component
struct NavigatorExample {
  @State active: boolean = false
  @State name: NameObject = { name: 'news' }

  build() {
    Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Start, justifyContent: FlexAlign.SpaceBetween }) {
      Navigator({ target: 'pages/container/navigator/Detail', type: NavigationType.Push }) {
        Text('Go to ' + this.name.name + ' page')
          .width('100%').textAlign(TextAlign.Center)
      }.params(new TextObject(this.name)) // 传参数到Detail页面

      Navigator() {
        Text('Back to previous page').width('100%').textAlign(TextAlign.Center)
      }.active(this.active)
      .onClick(() => {
        this.active = true
      })
    }.height(150).width(350).padding(35)
  }
}

interface NameObject {
  name: string;
}

class TextObject {
  text: NameObject;

  constructor(text: NameObject) {
    this.text = text;
  }
}

---

## samples/ArkUIContainerComponents/entry/src/main/ets/pages/BackExample.ets

Source: harmonyos-tutorial/samples/ArkUIContainerComponents/entry/src/main/ets/pages/BackExample.ets

@Entry
@Component
struct BackExample {
  build() {
    Column() {
      Navigator({ target: 'pages/container/navigator/Navigator', type: NavigationType.Back }) {
        Text('Return to Navigator Page').width('100%').textAlign(TextAlign.Center)
      }
    }.width('100%').height(200).padding({ left: 35, right: 35, top: 35 })
  }
}

---

## samples/ArkUIContainerComponents/entry/src/main/ets/pages/DetailExample.ets

Source: harmonyos-tutorial/samples/ArkUIContainerComponents/entry/src/main/ets/pages/DetailExample.ets

import { router } from '@kit.ArkUI'

@Entry
@Component
struct DetailExample {
  // 接收Navigator.ets的传参
  params: Record<string, NameObject> = router.getParams() as Record<string, NameObject>
  @State name: NameObject = this.params.text

  build() {
    Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Start, justifyContent: FlexAlign.SpaceBetween }) {
      Navigator({ target: 'pages/container/navigator/Back', type: NavigationType.Push }) {
        Text('Go to back page').width('100%').height(20)
      }

      Text('This is ' + this.name.name + ' page')
        .width('100%').textAlign(TextAlign.Center)
    }
    .width('100%').height(200).padding({ left: 35, right: 35, top: 35 })
  }
}

interface NameObject {
  name: string;
}

---

## samples/ArkUIContainerComponents/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIContainerComponents/entry/src/main/ets/pages/Index.ets

class Timetable {
  title: string = '';
  projects: String[] = [];
}

@Entry
@Component
struct Index {
  private numberArray: String[] = ['0', '1', '2', '3', '4'];
  private bgColors: Color[] = [Color.Red, Color.Orange, Color.Yellow, Color.Green, Color.Pink, Color.Grey, Color.Blue, Color.Brown];
  private timetableListItemGroup: Timetable[] = [
    {
      title:'星期一',
      projects:['语文', '数学', '英语']
    },
    {
      title:'星期二',
      projects:['物理', '化学', '生物']
    },
    {
      title:'星期三',
      projects:['历史', '地理', '政治']
    },
    {
      title:'星期四',
      projects:['美术', '音乐', '体育']
    }
  ]

  private alphabetIndexerArrayA: string[] = ['安']
  private alphabetIndexerArrayB: string[] = ['卜', '白', '包', '毕', '丙']
  private alphabetIndexerArrayC: string[] = ['曹', '成', '陈', '催']
  private alphabetIndexerArrayL: string[] = ['刘', '李', '楼', '梁', '雷', '吕', '柳', '卢']
  private alphabetIndexerArrayValue: string[] = ['#', 'A', 'B', 'C', 'D', 'E', 'F', 'G',
    'H', 'I', 'J', 'K', 'L', 'M', 'N',
    'O', 'P', 'Q', 'R', 'S', 'T', 'U',
    'V', 'W', 'X', 'Y', 'Z']
  @State counterValue: number = 0;

  build() {
    Column() {
      /*** 以下为 Column和Row 示例 ***/
      /*Column() {
        // 设置子组件水平方向的间距为5
        Row({ space: 5 }) {
          Row().width('30%').height(50).backgroundColor(0xAFEEEE)
          Row().width('30%').height(50).backgroundColor(0x00FFFF)
        }.width('90%').height(107).border({ width: 1 })

        // 设置子元素垂直方向对齐方式
        Row() {
          Row().width('30%').height(50).backgroundColor(0xAFEEEE)
          Row().width('30%').height(50).backgroundColor(0x00FFFF)
        }.width('90%').alignItems(VerticalAlign.Bottom).height('15%').border({ width: 1 })

        Row() {
          Row().width('30%').height(50).backgroundColor(0xAFEEEE)
          Row().width('30%').height(50).backgroundColor(0x00FFFF)
        }.width('90%').alignItems(VerticalAlign.Center).height('15%').border({ width: 1 })

        // 设置子元素水平方向对齐方式
        Row() {
          Row().width('30%').height(50).backgroundColor(0xAFEEEE)
          Row().width('30%').height(50).backgroundColor(0x00FFFF)
        }.width('90%').border({ width: 1 }).justifyContent(FlexAlign.End)

        Row() {
          Row().width('30%').height(50).backgroundColor(0xAFEEEE)
          Row().width('30%').height(50).backgroundColor(0x00FFFF)
        }.width('90%').border({ width: 1 }).justifyContent(FlexAlign.Center)
      }*/

      /*** 以下为 ColumnSplit和RowSplit 示例 ***//*
      // 纵向的分割线
      RowSplit() {
        Text('1').width('10%').height(400).backgroundColor(0xF5DEB3).textAlign(TextAlign.Center)
        Text('2').width('10%').height(400).backgroundColor(0xD2B48C).textAlign(TextAlign.Center)
        Text('3').width('10%').height(400).backgroundColor(0xF5DEB3).textAlign(TextAlign.Center)
        Text('4').width('10%').height(400).backgroundColor(0xD2B48C).textAlign(TextAlign.Center)
        Text('5').width('10%').height(400).backgroundColor(0xF5DEB3).textAlign(TextAlign.Center)
      }
      .resizeable(true) // 可拖动
      .width('90%').height(400)

      // 横向的分割线
      ColumnSplit() {
        Text('1').width('100%').height(50).backgroundColor(0xF5DEB3).textAlign(TextAlign.Center)
        Text('2').width('100%').height(50).backgroundColor(0xD2B48C).textAlign(TextAlign.Center)
        Text('3').width('100%').height(50).backgroundColor(0xF5DEB3).textAlign(TextAlign.Center)
        Text('4').width('100%').height(50).backgroundColor(0xD2B48C).textAlign(TextAlign.Center)
        Text('5').width('100%').height(50).backgroundColor(0xF5DEB3).textAlign(TextAlign.Center)
      }
      .resizeable(true) // 可拖动
      .width('90%').height('60%')
      */

      /*** 以下为 Flex 示例 ***//*
      // 主轴方向为FlexDirection.Row
      Flex({ direction: FlexDirection.Row }) {
        Text('1').width('20%').height(50).backgroundColor(0xF5DEB3)
        Text('2').width('20%').height(50).backgroundColor(0xD2B48C)
        Text('3').width('20%').height(50).backgroundColor(0xF5DEB3)
        Text('4').width('20%').height(50).backgroundColor(0xD2B48C)
      }
      .height('40%')
      .width('90%')
      .padding(10)
      .backgroundColor(0xAFEEEE)

      // 主轴方向为FlexDirection.Column
      Flex({ direction: FlexDirection.Column }) {
        Text('1').width('20%').height(50).backgroundColor(0xF5DEB3)
        Text('2').width('20%').height(50).backgroundColor(0xD2B48C)
        Text('3').width('20%').height(50).backgroundColor(0xF5DEB3)
        Text('4').width('20%').height(50).backgroundColor(0xD2B48C)
      }
      .height('40%')
      .width('90%')
      .padding(10)
      .backgroundColor(0xAFEEEE)
      */

      /*** 以下为 Grid和GridItem 示例 ***/
      // 主轴方向为FlexDirection.Row
      /*Grid() {
        ForEach(this.numberArray, (day: string) => {
          ForEach(this.numberArray, (day: string) => {
            GridItem() {
              Text(day)
                .fontSize(16)
                .backgroundColor(0xF9CF93)
                .width('100%')
                .height('100%')
                .textAlign(TextAlign.Center)
            }
          }, (day:number) => day + '')
        }, (day:number) => day + '')
      }
      .columnsTemplate('1fr 1fr 1fr 1fr 1fr') // 设置当前网格布局列的数量
      .rowsTemplate('1fr 1fr 1fr 1fr 1fr') // 设置当前网格布局行的数量
      .columnsGap(10) // 设置列与列的间距
      .rowsGap(10) // 设置行与行的间距
      .width('90%')
      .backgroundColor(0xFAEEE0)
      .height(300)*/

      /*** 以下为 ridRow和GridCol 示例 ***/
      /*GridRow({
        columns: 5, // 设置布局列数
        gutter: { x: 5, y: 20 }, // 栅格布局间距，x代表水平方向，y代表垂直方向
        breakpoints: { value: ["400vp", "600vp", "800vp"], // 断点发生变化时触发回调
          reference: BreakpointsReference.WindowSize },
        direction: GridRowDirection.Row // 	栅格布局排列方向
      }) {
        ForEach(this.bgColors, (color: Color) => {
          GridCol({ span: { xs: 1, sm: 2, md: 3, lg: 4 } }) {
            Row().width("100%").height("80vp")
          }.borderColor(color).borderWidth(2)
        })
      }.width("100%").height("100%")*/

      /*** 以下为 List、ListItem和ListItemGroup 示例 ***/
      /*List({ space: 2 }) {
        ForEach(this.timetableListItemGroup, (item: Timetable) => {
          ListItemGroup() {
            ForEach(item.projects, (project: string) => {
              ListItem() {
                Text(project)
                  .width("100%").height(30).fontSize(20)
                  .textAlign(TextAlign.Center)
              }
            }, (item: Timetable) => JSON.stringify(item))
          }
          .borderRadius(20)
          .divider({ strokeWidth: 2, color: 0xDCDCDC }) // 每行之间的分界线
        })
      }
      .width('100%')*/

      /*** 以下为 AlphabetIndexer 示例 ***/
      /*Row() {
        List({ space: 10, initialIndex: 0 }) {
          ForEach(this.alphabetIndexerArrayA, (item: string) => {
            ListItem() {
              Text(item)
                .width('80%')
                .height('5%')
                .fontSize(20)
                .textAlign(TextAlign.Center)
            }
          }, (item: string) => item)

          ForEach(this.alphabetIndexerArrayB, (item: string) => {
            ListItem() {
              Text(item)
                .width('80%')
                .height('5%')
                .fontSize(20)
                .textAlign(TextAlign.Center)
            }
          }, (item: string) => item)

          ForEach(this.alphabetIndexerArrayC, (item: string) => {
            ListItem() {
              Text(item)
                .width('80%')
                .height('5%')
                .fontSize(20)
                .textAlign(TextAlign.Center)
            }
          }, (item: string) => item)

          ForEach(this.alphabetIndexerArrayL, (item: string) => {
            ListItem() {
              Text(item)
                .width('80%')
                .height('5%')
                .fontSize(20)
                .textAlign(TextAlign.Center)
            }
          }, (item: string) => item)
        }
        .width('50%')
        .height('100%')

        AlphabetIndexer({ arrayValue: this.alphabetIndexerArrayValue, selected: 0 })
          .selectedColor(0xFFFFFF) // 选中项文本颜色
          .popupColor(0xFFFAF0) // 弹出框文本颜色
          .selectedBackgroundColor(0xCCCCCC) // 选中项背景颜色
          .popupBackground(0xD2B48C) // 弹出框背景颜色
          .usingPopup(true) // 是否显示弹出框
          .selectedFont({ size: 16, weight: FontWeight.Bolder }) // 选中项字体样式
          .popupFont({ size: 30, weight: FontWeight.Bolder }) // 弹出框内容的字体样式
          .itemSize(28) // 每一项的尺寸大小
          .alignStyle(IndexerAlign.Left) // 弹出框在索引条右侧弹出
          .onRequestPopupData((index: number) => {
            if (this.alphabetIndexerArrayValue[index] == 'A') {
              return this.alphabetIndexerArrayA // 当选中A时，弹出框里面的提示文本列表显示A对应的列表arrayA，选中B、C、L时也同样
            } else if (this.alphabetIndexerArrayValue[index] == 'B') {
              return this.alphabetIndexerArrayB
            } else if (this.alphabetIndexerArrayValue[index] == 'C') {
              return this.alphabetIndexerArrayC
            } else if (this.alphabetIndexerArrayValue[index] == 'L') {
              return this.alphabetIndexerArrayL
            } else {
              return [] // 选中其余子母项时，提示文本列表为空
            }
          })
        }*/


      /*** 以下为 Badge 示例 ***/
      // 如果不设置position，默认是在右上显示红点
      /*Badge({
        value: '',
        style: { badgeSize: 16, badgeColor: '#FA2A2D' }
      }) {
        Image($r('app.media.portrait'))
          .width(40)
          .height(40)
      }
      .width(40)
      .height(40)

      // 在右侧显示“New”
      Badge({
        value: 'New',
        position: BadgePosition.Right,
        style: { badgeSize: 16, badgeColor: '#FA2A2D' }
      }) {
        Text('我的消息').width(170).height(40).fontSize(40).fontColor('#182431')
      }.width(170).height(40)

      // 在右侧显示“数字
      Badge({
        value: '1',
        position: BadgePosition.Right,
        style: { badgeSize: 16, badgeColor: '#FA2A2D' }
      }) {
        Text('我的消息').width(170).height(40).fontSize(40).fontColor('#182431')
      }.width(170).height(40)*/

      /*** 以下为 Counter 示例 ***/
      /*Counter() {
        Text(this.counterValue.toString())
      }.margin(100)
      // 监听数值增加事件
      .onInc(() => {
        this.counterValue++
      })
      // 监听数值减少事件。
      .onDec(() => {
        this.counterValue--
      })*/


      /*** 以下为 Refresh 示例 ***/
      /*Refresh({ refreshing: true, //当前组件是否正在刷新
        offset: 120, // 新组件静止时距离父组件顶部的距离
        friction: 100 }) { //下拉摩擦系数，取值范围为0到100。默认值是62
        Text('下拉刷新 ')
          .fontSize(30)
          .margin(10)
      }*/

      /*** 以下为 RelativeContainer 示例 ***/
      /*RelativeContainer() {
        Row()
          .width(100)
          .height(100)
          .backgroundColor('#FF3333')
          .alignRules({
            top: { anchor: '__container__', align: VerticalAlign.Top },  //以父容器为锚点，竖直方向顶头对齐
            middle: { anchor: '__container__', align: HorizontalAlign.Center }  //以父容器为锚点，水平方向居中对齐
          })
          .id('row1')  //设置锚点为row1

        Row() {
          Image($r('app.media.startIcon'))
        }
        .height(100).width(100)
        .alignRules({
          top: { anchor: 'row1', align: VerticalAlign.Bottom },  //以row1组件为锚点，竖直方向底端对齐
          left: { anchor: 'row1', align: HorizontalAlign.Start }  //以row1组件为锚点，水平方向开头对齐
        })
        .id('row2')  //设置锚点为row2

        Row()
          .width(100)
          .height(100)
          .backgroundColor('#FFCC00')
          .alignRules({
            top: { anchor: 'row2', align: VerticalAlign.Top }
          })
          .id('row3')  //设置锚点为row3

        Row()
          .width(100)
          .height(100)
          .backgroundColor('#FF9966')
          .alignRules({
            top: { anchor: 'row2', align: VerticalAlign.Top },
            left: { anchor: 'row2', align: HorizontalAlign.End },
          })
          .id('row4')  //设置锚点为row4

        Row()
          .width(100)
          .height(100)
          .backgroundColor('#FF66FF')
          .alignRules({
            top: { anchor: 'row2', align: VerticalAlign.Bottom },
            middle: { anchor: 'row2', align: HorizontalAlign.Center }
          })
          .id('row5')  //设置锚点为row5
      }
      .width(300).height(300)
      .border({ width: 2, color: '#6699FF' })*/

      /*** 以下为 Scroll 示例 ***/
      // 与Scroller绑定
      /*Scroll(new Scroller()) {
        Column() {
          ForEach(this.numberArray, (item: string) => {
            Text(item.toString())
              .width('90%')
              .height(250)
              .backgroundColor(0xFFFFFF)
              .borderRadius(15)
              .fontSize(26)
              .textAlign(TextAlign.Center)
              .margin({ top: 10 })
          }, (item: string) => item)
        }.width('100%')
      }
      .scrollable(ScrollDirection.Vertical)  // 滚动方向纵向
      .scrollBar(BarState.On)  // 滚动条常驻显示
      .scrollBarColor(Color.Gray)  // 滚动条颜色
      .scrollBarWidth(40) // 滚动条宽度
      .edgeEffect(EdgeEffect.None)
      .onWillScroll((xOffset: number, yOffset: number) => {
        console.info(xOffset + ' ' + yOffset)
      })
      .onScrollEdge((side: Edge) => {
        console.info('To the edge')
      })
      .onScrollStop(() => {
        console.info('Scroll Stop')
      }).backgroundColor(0xDCDCDC)*/

      /*** 以下为 SideBarContainer 示例 ***/
      /*SideBarContainer(SideBarContainerType.Embed) {
        Column() {
          Text('菜单1').fontSize(25)
          Text('菜单2').fontSize(25)
        }.width('100%')
        .justifyContent(FlexAlign.SpaceEvenly)
        .backgroundColor('#19000000')

        Column() {
          Text('内容1').fontSize(25)
          Text('内容2').fontSize(25)
        }
      }*/

      /*** 以下为 Text 示例 ***/
      /*Stack({ alignContent: Alignment.Bottom }) {
        // 第一层组件
        Text('第一层')
          .width('90%')
          .height('100%')
          .backgroundColor(Color.Grey)
          .align(Alignment.Top)
          .fontSize(40)

        // 第二层组件
        Text('第二层')
          .width('70%')
          .height('60%')
          .backgroundColor(Color.Orange)
          .align(Alignment.Top)
          .fontSize(40)
      }.width('100%').height(400).margin({ top: 5 })*/


      /*** 以下为 Swiper 示例 ***/
      /*Swiper() {
        Image($r('app.media.book01'))
          .width(280).height(380)
        Image($r('app.media.book02'))
          .width(280).height(380)
        Image($r('app.media.book03'))
          .width(280).height(380)
        Image($r('app.media.book04'))
          .width(280).height(380)
      }
      .cachedCount(2) // 设置预加载子组件个数。
      .index(1) //设置当前在容器中显示的子组件的索引值。
      .autoPlay(true) //子组件是否自动播放，自动播放状态下，导航点不可操作。
      .interval(4000)//使用自动播放时播放的时间间隔，单位为毫秒。
      .indicator(true) //是否启用导航点指示器。
      .loop(true)//是否开启循环。
      .duration(1000)//子组件切换的动画时长，单位为毫秒。
      .itemSpace(0) //设置子组件与子组件之间间隙。
      .curve(Curve.Linear) // 设置Swiper的动画曲线*/

      /*** 以下为 Tabs和TabContent 示例 ***/
      /*Tabs({ barPosition: BarPosition.Start, //设置Tabs的页签位置。
        controller: new TabsController() //设置Tabs控制器。
      }) {
        TabContent() {
          Column().width('100%').height('100%').backgroundColor(Color.Orange)
        }.tabBar('首页')

        TabContent() {
          Column().width('100%').height('100%').backgroundColor(Color.Blue)
        }.tabBar('商城')

        TabContent() {
          Column().width('100%').height('100%').backgroundColor(Color.Red)
        }.tabBar('直播')
      }
      .vertical(false) //设置为false是为横向Tabs，设置为true时为纵向Tabs。
      .barMode(BarMode.Fixed) // TabBar布局模式
      .barWidth(360) // TabBar的宽度值
      .barHeight(56) // TabBar的高度值
      .animationDuration(400) //TabContent滑动动画时长
      .width(360)
      .height(296)
      .margin({ top: 52 })*/

    }
    .height('100%')
  }

}

---

## samples/ArkUILogin/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkUILogin/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkUILogin/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkUILogin/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkUILogin/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUILogin/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {

  build() {
    Row() {
      // 子组件都按照垂直方向排列
      Column() {
        // 头像
        Image($r('app.media.waylau_181_181'))
          .height(108)
          .width(108)

        // 账号输入框
        TextInput({ placeholder: '请输入账号' })
          .width(320)
          .height(50)
          .borderRadius(8)
          .backgroundColor('#f9f9f9')
          .margin({ top: '10' })
          .fontSize(27)
          .type(InputType.Normal) // 输入框类型：平常

        // 密码输入框
        TextInput({ placeholder: '请输入密码' })
          .width(320)
          .height(50)
          .borderRadius(8)
          .backgroundColor('#f9f9f9')
          .margin({ top: '10' })
          .fontSize(27)
          .type(InputType.Password) // 输入框类型：密码

        // 登录按钮
        Button('登录', { type: ButtonType.Normal })
          .width('320')
          .height('50')
          .borderRadius(8)
          .backgroundColor('#ffd0da')
          .margin({ top: '10' })
          .fontSize(24)

        // 注册按钮
        Text('注册')
          .fontColor(Color.Black)
          .margin({ top: '10' })
          .fontSize(24)
      }
      .width('100%')
    }
    .height('100%')
  }
}

---

## samples/ArkUICalculator/entry/src/main/ets/Calculator.ets

Source: harmonyos-tutorial/samples/ArkUICalculator/entry/src/main/ets/Calculator.ets

/**
 * 计算器计算逻辑
 */
export class Calculator {

  /**
   * 计算
   * @param input  例：input = '1.1-10%+2×3÷4'
   */
  public static calculate(input: string): string {
    // 先将百分数转为小数,
    input = input.replace(RegExp(`(((\\d*\\.\\d*)|(\\d+))%)`, 'g'), s => String(Number(s.replace(/%/, '')) / 100)) // input = '1.1-0.1+2×3÷4'
    // 要将input分割为数与运算符，分割节点的索引储存在splitIndex
    let splitIndex = [0]
    for (let i = 1;i < input.length; i++) {
      if (input[i].match(RegExp('(\\+|-|×|÷)')) != null) {
        splitIndex.push(i)
        splitIndex.push(i + 1)
        i++
      }
    }
    splitIndex.push(input.length) // splitIndex = [0, 3, 4, 7, 8, 9, 10, 11, 12, 13]
    // 分割input为数与运算符,储存在split
    let split: string[]= []
    for (let j = 0;j < splitIndex.length - 1; j++) {
      split.push(input.substring(splitIndex[j], splitIndex[j+1]))
    }
    // split = ['1.1', '-', '0.1', '+', '2', '×', '3', '÷', '4']
    return Calculator.recursiveCompute(split)[0] // 递归计算直至完成
  }

  /**
   * 递归计算直至完成，一次计算一对数，从左往右，乘除法优先于加减法
   * @param split
   * 例：split = ['1.1', '-', '0.1', '+', '2', '×', '3', '÷', '4']
   * 第1次：split = ['1.1', '-', '0.1', '+', '6', '÷', '4']
   * 第2次：split = ['1.1', '-', '0.1', '+', '1.5']
   * 第3次：split = ['1', '+', '1.5']
   * 第4次：split = ['2.5']
   */
  private static recursiveCompute(split: string[]): string[] {
    let symbolIndex:number = -1 // 符号索引
    // 先寻找乘除符号
    for (let i = 0;i < split.length; i++) {
      if (split[i].match(RegExp('^(×|÷)$')) != null) {
        symbolIndex = i
        break
      }
    }
    // 若没找到乘除符号，则寻找加减符号
    if (symbolIndex == -1) {
      for (let j = 0;j < split.length; j++) {
        if (split[j].match(RegExp('^(\\+|-)$')) != null) {
          symbolIndex = j
          break
        }
      }
    }
    if (symbolIndex == -1) { // 若没找到运算符号，表明计算结束，返回结果
      return split
    } else { // 若找到运算符号，运算后继续寻找运算
      let num1 = +parseInt(split[symbolIndex-1])
      let symbo1: string = split[symbolIndex]
      let num2 = +parseInt(split[symbolIndex+1])
      let result = 0
      switch (symbo1) {
        case '+':
          result = num1 + num2
          break
        case '-':
          result = num1 - num2
          break
        case '×':
          result = num1 * num2
          break
        case '÷':
          result = num1 / num2
          break
      }
      split = split.slice(0, symbolIndex - 1).concat(`${result}`).concat(split.slice(symbolIndex + 2))
      return Calculator.recursiveCompute(split)
    }
  }
}

---

## samples/ArkUICalculator/entry/src/main/ets/CalculatorButtonInfo.ets

Source: harmonyos-tutorial/samples/ArkUICalculator/entry/src/main/ets/CalculatorButtonInfo.ets

// 按钮样式信息
export class CalculatorButtonInfo {
  text: string // 按钮上的文字
  textColor: number // 文字的颜色
  bgColor: number // 按钮背景颜色

  constructor(text: string, textColor: number = Color.Black, bgColor: number = Color.White) {
    this.text = text
    this.textColor = textColor
    this.bgColor = bgColor
  }
}

---

## samples/ArkUICalculator/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkUICalculator/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkUICalculator/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkUICalculator/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkUICalculator/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUICalculator/entry/src/main/ets/pages/Index.ets

// 导入Calculator
import { Calculator } from '../Calculator';
// 导入CalculatorButtonInfo
import { CalculatorButtonInfo } from '../CalculatorButtonInfo';

@Entry
@Component
struct Index {
  private BTN_INFO_ARRAY: CalculatorButtonInfo[] = [ // 所有按钮样式信息
    new CalculatorButtonInfo('C', Color.Blue),
    new CalculatorButtonInfo('÷', Color.Blue),
    new CalculatorButtonInfo('×', Color.Blue),
    new CalculatorButtonInfo('←', Color.Blue),
    new CalculatorButtonInfo('7'),
    new CalculatorButtonInfo('8'),
    new CalculatorButtonInfo('9'),
    new CalculatorButtonInfo('-', Color.Blue),
    new CalculatorButtonInfo('4'),
    new CalculatorButtonInfo('5'),
    new CalculatorButtonInfo('6'),
    new CalculatorButtonInfo('+', Color.Blue),
    new CalculatorButtonInfo('1'),
    new CalculatorButtonInfo('2'),
    new CalculatorButtonInfo('3'),
    new CalculatorButtonInfo('=', Color.White, Color.Blue),
    new CalculatorButtonInfo('%'),
    new CalculatorButtonInfo('0'),
    new CalculatorButtonInfo('.')
  ]

  @State input: string = '' // 输入内容

  // 点击计算器按钮
  onClickBtn = (text: string) => {
    switch (text) {
      case 'C': // 清空所有输入
        this.input = ''
        break
      case '←': // 删除输入的最后一个字符
        if (this.input.length > 0) {
          this.input = this.input.substring(0, this.input.length - 1)
        }
        break
      case '=': // 计算结果
        this.input = Calculator.calculate(this.input)
        break
      default: // 输入内容
        this.input += text
        break
    }
  }

  // 构造计算器按钮
  @Builder CalculatorButton(btnInfo: CalculatorButtonInfo) { // 计算器按钮组件
    GridItem() {
      Text(btnInfo.text) // 文本
        .fontSize(50)
        .fontWeight(FontWeight.Bold)
        .width('100%')
        .height('100%')
        .textAlign(TextAlign.Center)
        .borderRadius(100) // 圆角
        .fontColor(btnInfo.textColor) // 字体颜色
        .backgroundColor(btnInfo.bgColor) // 背景颜色
    }
    .onClick(() => this.onClickBtn(btnInfo.text))
    .rowStart(btnInfo.text == '=' ? 4 : null)
    .rowEnd(btnInfo.text == '=' ? 5 : null) // 等于按钮占两格，其他按钮默认
  }

  build() {
    Stack({ alignContent: Alignment.Bottom }) {
      Column() {
        Text(this.input.length == 0 ? '0' : this.input) // 输入内容，若没有内容显示0
          .width('100%')
          .padding(10)
          .textAlign(TextAlign.End)
          .fontSize(46)
        Grid() {
          // 遍历生成按钮
          ForEach(this.BTN_INFO_ARRAY, (btnInfo:CalculatorButtonInfo) => this.CalculatorButton(btnInfo))
        }
        .columnsTemplate('1fr 1fr 1fr 1fr') // 按钮比重分配
        .rowsTemplate('1fr 1fr 1fr 1fr 1fr')
        .columnsGap(2) // 按钮间隙
        .rowsGap(2)
        .width('100%')
        .aspectRatio(1) // 长宽比
      }
    }.width('100%').height('100%').backgroundColor(Color.Gray)
  }
}


---

## samples/ArkUIWantOpenManageApplications/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIWantOpenManageApplications/entry/src/main/ets/pages/Index.ets

// 导入context
import context from '@ohos.application.context';
// 导入wantConstant
import wantConstant from '@ohos.ability.wantConstant';

@Entry
@Component
struct Index {
  build() {
    Row() {
      Column() {
        // 添加按钮，启动Ability
        Button('启动')
          .fontSize(40)
          .onClick(this.implicitStartAbility) // 隐示启动Ability
      }
      .width('100%')
    }
    .height('100%')
  }

  // 隐示启动Ability
  async implicitStartAbility() {
    try {
      let want = {
        // 调用应用管理
        "action": wantConstant.Action.ACTION_MANAGE_APPLICATIONS_SETTINGS
      }
      let context = getContext(this) as context.AbilityContext;
      await context.startAbility(want)
      console.info(`implicit start ability succeed`)
    } catch (error) {
      console.info(`implicit start ability failed with ${error.code}`)
    }
  }
}


---

## samples/ArkUIShopping/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkUIShopping/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkUIShopping/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkUIShopping/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkUIShopping/entry/src/main/ets/model/Menu.ets

Source: harmonyos-tutorial/samples/ArkUIShopping/entry/src/main/ets/model/Menu.ets

export class Menu {
  id: number = 0;
  title: string = '';
  num: number = 0;
}

export class ImageItem {
  id: number = 0;
  title: string = '';
  imageSrc: string = '';
}

---

## samples/ArkUIShopping/entry/src/main/ets/model/GoodsData.ets

Source: harmonyos-tutorial/samples/ArkUIShopping/entry/src/main/ets/model/GoodsData.ets

export class GoodsData {
  id: number = 0;
  title: string = '';
  content: string = '';
  price: number = 0;
  imgSrc: string = '';
}

---

## samples/ArkUIShopping/entry/src/main/ets/model/GoodsDataModels.ets

Source: harmonyos-tutorial/samples/ArkUIShopping/entry/src/main/ets/model/GoodsDataModels.ets

import { GoodsData } from './GoodsData'

import { Menu, ImageItem } from './Menu'
import { ArsData } from './ArsData'

export function initializeOnStartup(): Array<GoodsData> {
  return GoodsComposition;
}

export function getIconPath(): Array<string> {
  let IconPath: Array<string> = ['nav/icon-buy.png', 'nav/icon-shopping-cart.png', 'nav/icon-my.png']

  return IconPath;
}

export function getIconPathSelect(): Array<string> {
  let IconPathSelect: Array<string> =
    ['nav/icon-home.png', 'nav/icon-shopping-cart-select.png', 'nav/icon-my-select.png']

  return IconPathSelect;
}

export function getDetailImages(): Array<string> {
  let detailImages: Array<string> =
    ['computer/computer1.png', 'computer/computer2.png', 'computer/computer3.png', 'computer/computer4.png',
      'computer/computer5.png', 'computer/computer6.png']

  return detailImages;
}


export function getMenu(): Array<Menu> {
  return MyMenu;
}

export function getTrans(): Array<ImageItem> {
  return MyTrans;
}

export function getMore(): Array<ImageItem> {
  return MyMore;
}

export function getArs(): Array<ArsData> {
  return ArsList;
}

const GoodsComposition: GoodsData[] = [
  {
    "id": 1,
    "title": 'HUAWEI nova 8 Pro ',
    "content": 'Goes on sale: 10:08',
    "price": 3999,
    "imgSrc": 'picture/HW (1).png'
  },
  {
    "id": 2,
    "title": 'HUAWEI Mate 30E Pro 5G',
    "content": '3 interest-free payments ',
    "price": 5299,
    "imgSrc": 'picture/HW (2).png'
  },
  {
    "id": 3,
    "title": 'HUAWEI MatePad Pro',
    "content": 'Flagship ',
    "price": 3799,
    "imgSrc": 'picture/HW (3).png'
  },
  {
    "id": 4,
    "title": 'HUAWEI Nova 8 Pro',
    "content": 'New arrival ',
    "price": 3999,
    "imgSrc": 'picture/HW (4).png'
  },
  {
    "id": 5,
    "title": 'HUAWEI WATCH FIT',
    "content": 'Versatile',
    "price": 769,
    "imgSrc": 'picture/HW (5).png'
  },
  {
    "id": 6,
    "title": 'HUAWEI nova 8 Pro ',
    "content": 'Goes on sale: 10:08',
    "price": 3999,
    "imgSrc": 'picture/HW (6).png'
  },
  {
    "id": 7,
    "title": 'HUAWEI Mate 30E Pro 5G',
    "content": '3 interest-free payments ',
    "price": 5299,
    "imgSrc": 'picture/HW (7).png'
  },
  {
    "id": 8,
    "title": 'HUAWEI MatePad Pro',
    "content": 'Flagship ',
    "price": 3799,
    "imgSrc": 'picture/HW (8).png'
  },
  {
    "id": 9,
    "title": 'HUAWEI Nova 8 Pro',
    "content": 'New arrival ',
    "price": 3999,
    "imgSrc": 'picture/HW (9).png'
  },
  {
    "id": 10,
    "title": 'HUAWEI WATCH FIT',
    "content": 'Versatile',
    "price": 769,
    "imgSrc": 'picture/HW (10).png'
  }
]

const MyMenu: Menu[] = [
  {
    'id': 1,
    'title': 'Favorites',
    'num': 10
  },
  {
    'id': 2,
    'title': 'Searched',
    'num': 1000
  },
  {
    'id': 3,
    'title': 'Following',
    'num': 100
  },
  {
    'id': 4,
    'title': 'Followers',
    'num': 345
  }
]


const MyTrans: ImageItem[] = [
  {
    'id': 1,
    'title': 'Post: 520',
    'imageSrc': 'nav/icon-menu-release.png'
  },
  {
    'id': 2,
    'title': 'Sold: 520',
    'imageSrc': 'nav/icon-menu-sell.png'
  },
  {
    'id': 3,
    'title': 'Bought: 10',
    'imageSrc': 'nav/icon-menu-buy.png'
  }
]

const MyMore: ImageItem[] = [
  {
    'id': 1,
    'title': 'Guide',
    'imageSrc': 'nav/icon-menu-buy.png'
  },
  {
    'id': 2,
    'title': 'Create',
    'imageSrc': 'nav/icon-menu-buy.png'
  },
  {
    'id': 3,
    'title': 'Poster',
    'imageSrc': 'nav/icon-menu-buy.png'
  },
  {
    'id': 4,
    'title': 'Games',
    'imageSrc': 'nav/icon-menu-buy.png'
  },
  {
    'id': 5,
    'title': 'Jobber',
    'imageSrc': 'nav/icon-menu-buy.png'
  },
  {
    'id': 6,
    'title': 'Myself',
    'imageSrc': 'nav/icon-menu-buy.png'
  },
  {
    'id': 7,
    'title': 'About',
    'imageSrc': 'nav/icon-menu-buy.png'
  },
  {
    'id': 8,
    'title': 'Rental',
    'imageSrc': 'nav/icon-menu-buy.png'
  },
  {
    'id': 9,
    'title': 'Author',
    'imageSrc': 'nav/icon-menu-buy.png'
  },

]

const ArsList: ArsData[] = [
  {
    'id': 0,
    'title': 'Display Size',
    'content': '13.9 inches',
  },
  {
    'id': 1,
    'title': 'Memory',
    'content': '16 GB',
  },
  {
    'id': 2,
    'title': 'Marketing Name',
    'content': 'HUAWEI MateBook X Pro',
  },
  {
    'id': 3,
    'title': 'Color Gamut',
    'content': '100% sRGB color gamut (Typical)',
  },
  {
    'id': 4,
    'title': 'Battery',
    'content': '56 Wh (rated capacity)',
  },
  {
    'id': 5,
    'title': 'Storage',
    'content': '512 GB',
  },
  {
    'id': 6,
    'title': 'Resolution',
    'content': '3000x2000',
  },
  {
    'id': 7,
    'title': 'Processor',
    'content': '11th Gen Intel® Core™ i7-1165G7 Processor',
  },
  {
    'id': 8,
    'title': 'CPU Cores',
    'content': '4',
  },
  {
    'id': 9,
    'title': 'Launch Time',
    'content': 'January 2021',
  }
]

---

## samples/ArkUIShopping/entry/src/main/ets/pages/ShoppingDetail.ets

Source: harmonyos-tutorial/samples/ArkUIShopping/entry/src/main/ets/pages/ShoppingDetail.ets

import router from '@system.router';
import { ArsData } from '../model/ArsData'
import { getArs, getDetailImages } from '../model/GoodsDataModels'
import prompt from '@system.prompt';

@Entry
@Component
struct ShoppingDetail {
  private arsItems: ArsData[] = getArs()
  private detailImages: string[] = getDetailImages()

  build() {
    Column() {
      DetailTop()
      Scroll() {
        Column() {
          SwiperTop()
          DetailText()
          DetailArsList({ arsItems: this.arsItems })
          Image($rawfile('computer/computer1.png'))
            .height(220)
            .width('100%')
            .margin({ top: 30 })
          Image($rawfile('computer/computer2.png'))
            .height(220)
            .width('100%')
            .margin({ top: 30 })
          Image($rawfile('computer/computer3.png'))
            .height(220)
            .width('100%')
            .margin({ top: 30 })
          Image($rawfile('computer/computer4.png'))
            .height(220)
            .width('100%')
            .margin({ top: 30 })
          Image($rawfile('computer/computer5.png'))
            .height(220)
            .width('100%')
            .margin({ top: 30 })
          Image($rawfile('computer/computer6.png'))
            .height(220)
            .width('100%')
            .margin({ top: 30 })
        }
        .width('100%')
        .flexGrow(1)
      }
      .scrollable(ScrollDirection.Vertical)

      DetailBottom()
    }
    .height(630)

  }
}

@Component
struct DetailTop {
  build() {
    Column() {
      Row() {
        Image($rawfile('detail/icon-return.png'))
          .height(20)
          .width(20)
          .margin({ left: 20, right: 250 })
          .onClick(() => {
            router.push({
              uri: "pages/Index"
            })
          })

      }
      .width('100%')
      .height(25)
      .backgroundColor('#FF87CEEB')
    }
    .width('100%')
    .height(30)
  }
}

@Component
struct SwiperTop {
  build() {
    Column() {
      Swiper() {
        Image($rawfile('computer/computer1.png'))
          .height(220)
          .width('100%')
        Image($rawfile('computer/computer2.png'))
          .height(220)
          .width('100%')
        Image($rawfile('computer/computer3.png'))
          .height(220)
          .width('100%')
        Image($rawfile('computer/computer4.png'))
          .height(220)
          .width('100%')
        Image($rawfile('computer/computer5.png'))
          .height(220)
          .width('100%')
        Image($rawfile('computer/computer6.png'))
          .height(220)
          .width('100%')

      }
      .index(0)
      .autoPlay(true)
      .interval(3000)
      .indicator(true)
      .loop(true)
      .height(250)
      .width('100%')
    }
    .height(250)
    .width('100%')
  }
}

@Component
struct DetailText {
  build() {
    Column() {
      Row() {
        Image($rawfile('computer/icon-promotion.png'))
          .height(30)
          .width(30)
          .margin({ left: 10 })
        Text('Special Offer: ￥9999')
          .fontColor(Color.White)
          .fontSize(20)
          .margin({ left: 10 })

      }
      .width('100%')
      .height(35)
      .backgroundColor(Color.Red)

      Column() {
        Text('New Arrival: HUAWEI MateBook X Pro 2021')
          .fontSize(15)
          .margin({ left: 10 })
          .alignSelf(ItemAlign.Start)
        Text('13.9-Inch, 11th Gen Intel® Core™ i7, 16 GB of Memory, 512 GB of Storage, Ultra-slim Business Laptop, 3K FullView Display, Multi-screen Collaboration, Emerald Green')
          .fontSize(10)
          .margin({ left: 10 })
        Row() {
          Image($rawfile('nav/icon-buy.png'))
            .height(15)
            .width(15)
            .margin({ left: 10 })
          //TODO 暂不支持跑马灯组件，用Text代替
          Text('Limited offer')
            .fontSize(10)
            .fontColor(Color.Red)
            .margin({ left: 100 })

        }
        .backgroundColor(Color.Pink)
        .width('100%')
        .height(25)
        .margin({ top: 10 })

        Text(' Shipment:         2-day shipping')
          .fontSize(13)
          .fontColor(Color.Red)
          .margin({ left: 10, top: 5 })
          .alignSelf(ItemAlign.Start)
        Text('    Ship To:         Hubei,Wuhan,China')
          .fontSize(13)
          .fontColor(Color.Red)
          .margin({ left: 10, top: 5 })
          .alignSelf(ItemAlign.Start)
          .onClick(() => {
            prompt.showDialog({ title: 'select address', })

          })
        Text('Guarantee:         Genuine guaranteed')
          .fontSize(13)
          .margin({ left: 10, top: 5 })
          .alignSelf(ItemAlign.Start)
      }
      .height(150)
      .width('100%')
    }
    .height(160)
    .width('100%')
  }
}


@Component
struct DetailArsList {
  private arsItems: ArsData[] = [];

  build() {
    Scroll() {
      Column() {
        List() {
          ForEach(this.arsItems, (item: ArsData) => {
            ListItem() {
              ArsListItem({ arsItem: item })
            }
          }, (item: ArsData) => item.id.toString())
        }
        .height('100%')
        .width('100%')
        .margin({ top: 5 })
        .listDirection(Axis.Vertical)
      }
      .height(200)
    }
  }
}

@Component
struct ArsListItem {
  private arsItem: ArsData = new ArsData();

  build() {
    Row() {
      Text(this.arsItem.title + " :")
        .fontSize(11)
        .margin({ left: 20 })
        .flexGrow(1)
      Text(this.arsItem.content)
        .fontSize(11)
        .margin({ right: 20 })

    }
    .height(14)
    .width('100%')
    .backgroundColor(Color.White)
  }
}


@Component
struct DetailBottom {
  @Provide
  private value: number = 1
  dialogController: CustomDialogController = new CustomDialogController({
    builder: DialogExample(),
    cancel: this.existApp,
    autoCancel: true
  });

  onAccept() {

  }

  existApp() {

  }

  build() {
    Column() {
      Text('Buy')
        .width(40)
        .height(25)
        .fontSize(20)
        .fontColor(Color.White)
        .onClick(() => {
          this.value = 1
          this.dialogController.open()
        })
    }
    .alignItems(HorizontalAlign.Center)
    .backgroundColor(Color.Red)
    .width('100%')
    .height(40)
  }
}


@CustomDialog
struct DialogExample {
  @Consume
  private value: number
  controller: CustomDialogController;

  build() {
    Column() {
      Progress({ value: this.value++ >= 100 ? 100 : this.value, total: 100, style: ProgressStyle.Eclipse })
        .height(50)
        .width(100)
        .margin({ top: 5 })

    }
    .height(60)
    .width(100)

  }
}


---

## samples/ArkUIShopping/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIShopping/entry/src/main/ets/pages/Index.ets

import { GoodsData } from '../model/GoodsData'
import { initializeOnStartup, getIconPath, getIconPathSelect } from '../model/GoodsDataModels'
import { ShoppingCart } from './ShoppingCart'
import { MyInfo } from './MyPage'

/**
 * 应用主页
 */
@Entry
@Component
struct Index {
  @Provide currentPage: number = 1
  private goodsItems: GoodsData[] = initializeOnStartup()

  build() {
    Column() {
      Scroll() {
        Column() {
          if (this.currentPage == 1) {
            // 商品列表页
            GoodsHome({ goodsItems: this.goodsItems })
          } else if (this.currentPage == 2) {
            // 购物车列表
            ShoppingCart()
          } else {
            // 我的
            MyInfo()
          }
        }
        .height(700)
      }
      .flexGrow(1)

      HomeBottom()
    }
    .backgroundColor("white")
  }
}

@Component
struct GoodsHome {
  private goodsItems: GoodsData[] = [];

  build() {
    Column() {
      Tabs() {
        TabContent() {
          GoodsList({ goodsItems: this.goodsItems });
        }
        .tabBar("Top Sellers")
        .backgroundColor(Color.White)

        TabContent() {
          GoodsList({ goodsItems: this.goodsItems });
        }
        .tabBar("Recommended")
        .backgroundColor(Color.White)

        TabContent() {
          GoodsList({ goodsItems: this.goodsItems });
        }
        .tabBar("Lifestyle")
        .backgroundColor(Color.White)

        TabContent() {
          GoodsList({ goodsItems: this.goodsItems });
        }
        .tabBar("Deals")
        .backgroundColor(Color.White)
      }
      .barWidth(500)
      .barHeight(40)
      .scrollable(true)
      .barMode(BarMode.Scrollable)
      .backgroundColor('#F1F3F5')
      .height(700)

    }
    .alignItems(HorizontalAlign.Start)
    .width('100%')
  }
}

@Component
struct GoodsList {
  private goodsItems: GoodsData[] = [];

  build() {
    Column() {
      List() {
        ForEach(this.goodsItems, (item: GoodsData) => {
          ListItem() {
            GoodsListItem({ goodsItem: item })
          }
        }, (item: GoodsData) => item.id.toString())
      }
      .height('100%')
      .width('100%')
      .align(Alignment.Top)
      .margin({ top: 5 })
    }
  }
}


@Component
struct GoodsListItem {
  private goodsItem: GoodsData = new GoodsData();

  build() {
    Navigator({ target: 'pages/ShoppingDetail' }) {
      Row() {
        Column() {
          Text(this.goodsItem.title)
            .fontSize(14)
          Text(this.goodsItem.content)
            .fontSize(10)
          Text('￥' + this.goodsItem.price)
            .fontSize(14)
            .fontColor(Color.Red)
        }
        .height(100)
        .width('50%')
        .margin({ left: 20 })
        .alignItems(HorizontalAlign.Start)

        Image($rawfile(this.goodsItem.imgSrc))
          .objectFit(ImageFit.ScaleDown)
          .height(100)
          .width('40%')
          .renderMode(ImageRenderMode.Original)
          .margin({ right: 10, left: 10 })

      }
      .backgroundColor(Color.White)

    }
    .params({ goodsData: this.goodsItem })
    .margin({ right: 5 })
  }
}

@Component
struct HomeBottom {
  @Consume currentPage: number
  private iconPathTmp: string[] = getIconPath()
  private iconPathSelectsTmp: string[] = getIconPathSelect()
  @State iconPath: string[] = getIconPath()

  build() {
    Row() {
      List() {
        ForEach(this.iconPath, (item: string) => {
          ListItem() {
            Image($rawfile(item))
              .objectFit(ImageFit.Cover)
              .height(30)
              .width(30)
              .renderMode(ImageRenderMode.Original)
              .onClick(() => {
                if (item == this.iconPath[0]) {
                  this.iconPath[0] = this.iconPathTmp[0]
                  this.iconPath[1] = this.iconPathTmp[1]
                  this.iconPath[2] = this.iconPathTmp[2]
                  this.currentPage = 1
                }
                if (item == this.iconPath[1]) {
                  this.iconPath[0] = this.iconPathSelectsTmp[0]
                  this.iconPath[1] = this.iconPathSelectsTmp[1]
                  this.iconPath[2] = this.iconPathTmp[2]
                  this.currentPage = 2
                }
                if (item == this.iconPath[2]) {
                  this.iconPath[0] = this.iconPathSelectsTmp[0]
                  this.iconPath[1] = this.iconPathTmp[1]
                  this.iconPath[2] = this.iconPathSelectsTmp[2]
                  this.currentPage = 3
                }
              })
          }
          .width(120)
          .height(40)
        }, (item: string) => item)
      }
      .margin({ left: 10 })
      .align(Alignment.BottomStart)
      .listDirection(Axis.Horizontal)
    }
    .alignItems(VerticalAlign.Bottom)
    .height(30)
    .margin({ top: 10, bottom: 10 })
  }
}


---

## samples/ArkUIShopping/entry/src/main/ets/pages/MyPage.ets

Source: harmonyos-tutorial/samples/ArkUIShopping/entry/src/main/ets/pages/MyPage.ets

import { getMenu, getTrans, getMore } from '../model/GoodsDataModels'
import { Menu, ImageItem } from '../model/Menu'

@Entry
@Component
export struct MyInfo {
  build() {
    Column() {
      Row() {
        Image($rawfile('nav/waylau_181_181.jpg'))
          .margin({ left: 20 })
          .objectFit(ImageFit.Cover)
          .height(50)
          .width(50)
          .renderMode(ImageRenderMode.Original)
          .margin({ left: 40, right: 40 })

        Column() {
          Text('Way Lau')
            .fontSize(15)
          Text('Member Name : Way Lau                     >')
        }
        .height(60)
        .margin({ left: 40, top: 10 })
        .alignItems(HorizontalAlign.Start)
      }

      TopList()
      MyTransList()
      MoreGrid()

    }
    .alignItems(HorizontalAlign.Start)
    .width('100%')
    .flexGrow(1)
  }
}


@Component
struct TopList {
  private menus: Menu[] = getMenu()

  build() {
    Row() {
      List() {
        ForEach(this.menus, (item: Menu) => {
          ListItem() {
            MenuItemView({ menu: item })
          }
        }, (item: Menu) => item.id.toString())
      }
      .height('100%')
      .width('100%')
      .margin({ top: 5 })
      .edgeEffect(EdgeEffect.None)
      .listDirection(Axis.Horizontal)
    }
    .width('100%')
    .height(50)
  }
}

@Component
struct MyTransList {
  private imageItems: ImageItem[] = getTrans()

  build() {
    Column() {
      Text('My Transaction')
        .fontSize(20)
        .margin({ left: 10 })
        .width('100%')
        .height(30)
      Row() {
        List() {
          ForEach(this.imageItems, (item: ImageItem) => {
            ListItem() {
              DataItem({ imageItem: item })
            }
          }, (item: ImageItem) => item.id.toString())
        }
        .height(70)
        .width('100%')
        .align(Alignment.Top)
        .margin({ top: 5 })
        .listDirection(Axis.Horizontal)
      }
    }
    .height(120)
  }
}

@Component
struct MoreGrid {
  private gridRowTemplate: string = ''
  private imageItems: ImageItem[] = getMore()
  private heightValue: number = 0;

  aboutToAppear() {
    let rows = Math.round(this.imageItems.length / 3);
    this.gridRowTemplate = '1fr '.repeat(rows);
    this.heightValue = rows * 75;
  }

  build() {
    Column() {
      Text('More')
        .fontSize(20)
        .margin({ left: 10 })
        .width('100%')
        .height(30)
      Scroll() {
        Grid() {
          ForEach(this.imageItems, (item: ImageItem) => {
            GridItem() {
              DataItem({ imageItem: item })
            }
          }, (item: ImageItem) => item.id.toString())
        }
        .rowsTemplate(this.gridRowTemplate)
        .columnsTemplate('1fr 1fr 1fr')
        .columnsGap(8)
        .rowsGap(8)
        .height(this.heightValue)
      }
      .padding({ left: 16, right: 16 })
    }
    .height(400)
  }
}


@Component
struct DataItem {
  private imageItem: ImageItem = new ImageItem();

  build() {
    Column() {
      Image($rawfile(this.imageItem.imageSrc))
        .objectFit(ImageFit.Contain)
        .height(50)
        .width(50)
        .renderMode(ImageRenderMode.Original)
      Text(this.imageItem.title)
        .fontSize(15)


    }
    .height(70)
    .width(80)
    .margin({ left: 15, right: 15 })
    .backgroundColor(Color.White)
  }
}

@Component
struct MenuItemView {
  private menu: Menu = new Menu();

  build() {
    Column() {
      Text(this.menu.title)
        .fontSize(15)
      Text(this.menu.num + '')
        .fontSize(13)

    }
    .height(50)
    .width(80)
    .margin({ left: 8, right: 8 })
    .alignItems(HorizontalAlign.Start)
    .backgroundColor(Color.White)
  }
}



---

## samples/ArkUIShopping/entry/src/main/ets/pages/ShoppingCart.ets

Source: harmonyos-tutorial/samples/ArkUIShopping/entry/src/main/ets/pages/ShoppingCart.ets

import { GoodsData } from '../model/GoodsData'
import { initializeOnStartup } from '../model/GoodsDataModels'
import { promptAction } from '@kit.ArkUI'


@Entry
@Component
export struct ShoppingCart {
  @Provide totalPrice: number = 0
  private goodsItems: GoodsData[] = initializeOnStartup()

  build() {
    Column() {
      Column() {
        Text('ShoppingCart')
          .fontColor(Color.Black)
          .fontSize(25)
          .margin({ left: 60, right: 60 })
          .align(Alignment.Center)
      }
      .backgroundColor('#FF00BFFF')
      .width('100%')
      .height(30)

      ShopCartList({ goodsItems: this.goodsItems });
      ShopCartBottom()
    }
    .alignItems(HorizontalAlign.Start)
  }
}

@Component
struct ShopCartList {
  private goodsItems: GoodsData[] = [];

  build() {
    Column() {
      List() {
        ForEach(this.goodsItems, (item: GoodsData) => {
          ListItem() {
            ShopCartListItem({ goodsItem: item })
          }
        }, (item: GoodsData) => item.id.toString())
      }
      .height('100%')
      .width('100%')
      .align(Alignment.Top)
      .margin({ top: 5 })
    }
    .height(570)
  }
}

@Component
struct ShopCartListItem {
  @Consume totalPrice: number
  private goodsItem: GoodsData = new GoodsData();

  build() {
    Row() {
      Toggle({ type: ToggleType.Checkbox })
        .width(10)
        .height(10)
        .onChange((isOn: boolean) => {
          if (isOn) {
            this.totalPrice += parseInt(this.goodsItem.price + '', 0)
          } else {
            this.totalPrice -= parseInt(this.goodsItem.price + '', 0)
          }
        })
      Image($rawfile(this.goodsItem.imgSrc))
        .objectFit(ImageFit.ScaleDown)
        .height(100)
        .width(100)
        .renderMode(ImageRenderMode.Original)
      Column() {
        Text(this.goodsItem.title)
          .fontSize(14)
        Text('￥' + this.goodsItem.price)
          .fontSize(14)
          .fontColor(Color.Red)
      }
    }
    .height(100)
    .width(180)
    .margin({ left: 20 })
    .alignItems(VerticalAlign.Center)
    .backgroundColor(Color.White)
  }
}

@Component
struct ShopCartBottom {
  @Consume totalPrice: number

  build() {
    Row() {
      Text('Total:  ￥' + this.totalPrice)
        .fontColor(Color.Red)
        .fontSize(18)
        .margin({ left: 20 })
        .width(150)
      Text('Check Out')
        .fontColor(Color.Black)
        .fontSize(18)
        .margin({ right: 20, left: 100 })
        .onClick(() => {
          promptAction.showToast({
            message: 'Checking Out',
            duration: 10,
            bottom: 100
          })
        })
    }
    .height(30)
    .width('100%')
    .backgroundColor('#FF7FFFD4')
    .alignItems(VerticalAlign.Bottom)
  }
}


---

## samples/ArkUIExperience/entry/src/main/ets/common/bean/NftResponse.ets

Source: harmonyos-tutorial/samples/ArkUIExperience/entry/src/main/ets/common/bean/NftResponse.ets

/*
 * Copyright (c) 2022 Huawei Device Co., Ltd.
 * Licensed under the Apache License,Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

/**
 * 获取NFT请求状态响应类
 */
export default class NftResponse {
  public success: boolean;
  public message: string;
  public errCode: string;
  public result: NftResult;

  constructor(success: boolean, message: string, errCode: string, result: NftResult) {
    this.success = success;
    this.message = message;
    this.errCode = errCode;
    this.result = result;
  }
};

/**
 * 获取NFT结果响应类
 *
 */
class NftResult {
  public id: number;
  public imageId: string;
  public ip: string;
  public name: string;
  public tokenId: string;
  public assertId: string;
  public createTime: string;
  public mintTime: string;
  public hash: string;
  public quality: string;

  constructor() {}
};

---

## samples/ArkUIExperience/entry/src/main/ets/common/constants/Constants.ets

Source: harmonyos-tutorial/samples/ArkUIExperience/entry/src/main/ets/common/constants/Constants.ets

/*
 * Copyright (c) 2022 Huawei Device Co., Ltd.
 * Licensed under the Apache License,Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

/**
 * 保存应用中所有常量
 */

// 服务器前缀
const SEVER_PREFIX_URL = 'http://139.9.102.131:8081/dac-api';
// 华为数字藏品网址
export const DacUrl = 'https://www.huaweicloud.com/product/bcs/dac.html';

// 服务端相关常量
export const Sever = {
  PREFIX_URL: SEVER_PREFIX_URL,
  WEB_PAGE_URL: SEVER_PREFIX_URL + '/web/image',
  GET_NFT_URL: SEVER_PREFIX_URL + '/dac/get',
};

// 字体大小
export enum FontSize {
  MINI = 14,
  SMALL = 16,
  MIDDLE = 18,
  LARGE = 20
}
;

/**
 * ErrorDialog组件InfoDialog组件和样式
 */
export const DialogStyle = {
  TEXT_EMPTY_TITLE_WIDTH: 30,
  TEXT_EMPTY_LINE_HEIGHT: 24,
  IMAGE_CANCEL_WIDTH: 30,
  IMAGE_CANCEL_HEIGHT: 30,
  IMAGE_CANCEL_MARGIN_RIGHT: 10,
  FLEX_GRADIENT_ANGLE: 90,
  FLEX_GRADIENT_START: 0.0,
  FLEX_GRADIENT_END: 1.0,
  FLEX_GRADIENT_WIDTH: '100%',
  FLEX_GRADIENT_HEIGHT: 60,
  TEXT_CONTENT_MARGIN: 20,
  FLEX_BUTTON_BOX_MARGIN_BOTTOM: 20,
  IMAGE_GONE_WIDTH: 168,
  IMAGE_GONE_HEIGHT: 84,
  IMAGE_GONE_MARGIN_TOP: 30,
};

/**
 * Input组件样式
 */
export const InputStyle = {
  TEXT_INPUT_OPACITY: .2,
  TEXT_INPUT_PLACEHOLDER_SIZE: 20,
  TEXT_INPUT_PLACEHOLDER_WEIGHT: 300,
  TEXT_INPUT_PLACEHOLDER_HEIGHT: '100%',
  TEXT_INPUT_PLACEHOLDER_RADIUS: 2,
  TEXT_INPUT_PLACEHOLDER_PADDING_RIGHT: 70,
  TEXT_INPUT_PLACEHOLDER_MARGIN_RIGHT: 5,
  TEXT_INPUT_PLACEHOLDER_MARGIN_LEFT: 5,
  BUTTON_CONFIRM_OFFSET_X: -70,
  BUTTON_CONFIRM_WIDTH: 70,
  BUTTON_CONFIRM_HEIGHT: '100%',
  BUTTON_CONFIRM_RADIUS: 6,
  ROW_WIDTH: 292,
  ROW_HEIGHT: 40,
  ROW_RADIUS: 6
};

// Index页面样式
export const IndexStyle = {
  IMAGE_TITLE_WIDTH: 330,
  IMAGE_TITLE_HEIGHT: 112,
  IMAGE_TITLE_MARGIN_TOP: 50,
  IMAGE_ANIMATION_WIDTH: 240,
  IMAGE_ANIMATION_HEIGHT: 240,
  IMAGE_EXPLAIN_MARGIN_TOP: 20,
  COLUMN_EMPTY_LAYOUT_WEIGHT: 1,
  BLANK_EMPTY_MARGIN_BOTTOM: 20,
  BUTTON_LOTTERY_OPACITY: .5,
  BUTTON_LOTTERY_WIDTH: 120,
  BUTTON_LOTTERY_HEIGHT: 40,
  BUTTON_LOTTERY_RADIUS: 40,
  BUTTON_LOTTERY_ANGLE: 90,
  BUTTON_LOTTERY_GRADIENT_START: 0.0,
  BUTTON_LOTTERY_GRADIENT_END: 1.0,
  BUTTON_LOTTERY_MARGIN_BOTTOM: 50,
  BUTTON_BACKGROUND_IMAGE_WIDTH: '100%',
  BUTTON_BACKGROUND_IMAGE_HEIGHT: '100%',
  BUTTON_MAIN_WIDTH: '100%',
  BUTTON_MAIN_HEIGHT: '100%',
};

// Nft页面样式
export const NftStyle = {
  ANIMATOR_WIDTH: 240,
  ANIMATOR_HEIGHT: 240,
  ANIMATOR_MARGIN_TOP: 162,
  ANIMATOR_LAYOUT_WIDTH: '100%',
  ANIMATOR_LAYOUT_HEIGHT: '100%',
  IMAGE_BACKGROUND_WIDTH: '100%',
  IMAGE_BACKGROUND_HEIGHT: '100%',
  TEXT_GONE_MARGIN_TOP: 28,
  IMAGE_GONE_ASPECT_RATIO: 2,
  IMAGE_GONE_WIDTH: '100%',
  TEXT_NO_PRIZE_MARGIN_TOP: 10,
  COLUMN_GONE_WIDTH: '50%',
  COLUMN_GONE_ASPECT_RATIO: 1,
  COLUMN_GONE_BORDER_RADIUS: 12,
  COLUMN_GONE_BORDER_WIDTH: 2,
  COLUMN_GONE_MARGIN_TOP: 20,
  TEXT_CONGRATULATIONS_MARGIN_TOP: 28,
  TEXT_AVATAR_MARGIN_TOP: 8,
  IMAGE_NFT_WIDTH: '50%',
  IMAGE_NFT_ASPECT_RATIO: 1,
  IMAGE_NFT_RADIUS: 12,
  IMAGE_NFT_MARGIN_TOP: 20,
  TEXT_INDEX_MARGIN_RIGHT: 8,
  TEXT_EMPTY_WIDTH: 40,
  TEXT_QUALITY_MARGIN_RIGHT: 2,
  IMAGE_ABOUT_WIDTH: 16,
  IMAGE_ABOUT_HEIGHT: 16,
  IMAGE_ABOUT_MARGIN_RIGHT: 8,
  TEXT_QUALITY_WIDTH: 33,
  FLEX_NFT_INFO_MARGIN_TOP: 10,
  FLEX_NFT_INFO_MARGIN_BOTTOM: 10,
  QRCODE_WIDTH: '55%',
  TEXT_QRCODE_WIDTH: '80%',
  TEXT_QRCODE_MARGIN_TOP: 12,
  FLEX_QRCODE_LAYOUT_WEIGHT: 1,
  COLUMN_RESULT_WIDTH: '90%',
  COLUMN_RESULT_HEIGHT: '90%',
  COLUMN_RESULT_RADIUS: 12,
  NFT_WIDTH: '100%',
  NFT_HEIGHT: '100%',
};

---

## samples/ArkUIExperience/entry/src/main/ets/common/utils/LogUtil.ets

Source: harmonyos-tutorial/samples/ArkUIExperience/entry/src/main/ets/common/utils/LogUtil.ets

/*
 * Copyright (c) 2022 Huawei Device Co., Ltd.
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

/**
 * 日志工具
 */
import hilog from '@ohos.hilog';

class CommonLog {
  private domain: number;
  private prefix: string;
  private format: string = '[%{public}s] %{public}s';

  constructor(prefix: string) {
    this.prefix = prefix;
    this.domain = 0xFF00;
  }

  debug(...args: any[]) {
    hilog.debug(this.domain, this.prefix, this.format, args);
  }

  info(...args: any[]) {
    hilog.info(this.domain, this.prefix, this.format, args);
  }

  warn(...args: any[]) {
    hilog.warn(this.domain, this.prefix, this.format, args);
  }

  error(...args: any[]) {
    hilog.error(this.domain, this.prefix, this.format, args);
  }
}

export default new CommonLog('ArkUIExperience');

---

## samples/ArkUIExperience/entry/src/main/ets/view/InfoDialog.ets

Source: harmonyos-tutorial/samples/ArkUIExperience/entry/src/main/ets/view/InfoDialog.ets

/*
 * Copyright (c) 2022 Huawei Device Co., Ltd.
 * Licensed under the Apache License,Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

/**
 * 普通提示弹框组件
 */
import { FontSize, DialogStyle } from '../common/constants/Constants';

@CustomDialog
struct InfoDialog {
  controller: CustomDialogController;
  textStr: ResourceStr;
  title: ResourceStr;

  build() {
    Column() {
      Flex({
        direction: FlexDirection.Row,
        justifyContent: FlexAlign.SpaceBetween,
        alignItems: ItemAlign.Center
      }) {
        Text('')
          .width(DialogStyle.TEXT_EMPTY_TITLE_WIDTH)
        Text(this.title)
          .fontSize(FontSize.MIDDLE)
          .fontColor(Color.Black)
          .fontWeight(FontWeight.Bolder)
          .lineHeight(DialogStyle.TEXT_EMPTY_LINE_HEIGHT)
        Image($r('app.media.ic_public_cancel'))
          .width(DialogStyle.IMAGE_CANCEL_WIDTH)
          .height(DialogStyle.IMAGE_CANCEL_HEIGHT)
          .margin({ right: DialogStyle.IMAGE_CANCEL_MARGIN_RIGHT })
          .onClick(() => {
            this.controller.close();
          })
      }
      .backgroundImage($r('app.media.dialog_BG'))
      .backgroundImageSize(ImageSize.Cover)
      .linearGradient({
        angle: DialogStyle.FLEX_GRADIENT_ANGLE,
        colors: [[0xFDBF35, DialogStyle.FLEX_GRADIENT_START], [0xFF8C00, DialogStyle.FLEX_GRADIENT_END]]
      })
      .width(DialogStyle.FLEX_GRADIENT_WIDTH)
      .height(DialogStyle.FLEX_GRADIENT_HEIGHT)

      Text(this.textStr)
        .fontSize(FontSize.MIDDLE)
        .margin(DialogStyle.TEXT_CONTENT_MARGIN)
      Flex({ justifyContent: FlexAlign.SpaceAround }) {
        Button($r('app.string.button_text_confirm'))
          .fontSize(FontSize.MIDDLE)
          .backgroundColor(0x0D000000)
          .fontColor($r('app.color.dialog_button_confirm'))
          .onClick(() => {
            this.controller.close();
          })
      }.margin({ bottom: DialogStyle.FLEX_BUTTON_BOX_MARGIN_BOTTOM })
    }
  }
}

export default InfoDialog;

---

## samples/ArkUIExperience/entry/src/main/ets/view/Input.ets

Source: harmonyos-tutorial/samples/ArkUIExperience/entry/src/main/ets/view/Input.ets

/*
 * Copyright (c) 2022 Huawei Device Co., Ltd.
 * Licensed under the Apache License,Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

/**
 * 自定义输入框组件
 */
import { FontSize, InputStyle } from '../common/constants/Constants';

@Component
struct Input {
  @Link inputText: String;
  confirm?: () => void;

  build() {
    Row() {
      TextInput({ placeholder: '请输入名字' })
        .placeholderColor(Color.Black)
        .opacity(InputStyle.TEXT_INPUT_OPACITY)
        .placeholderFont({
          size: InputStyle.TEXT_INPUT_PLACEHOLDER_SIZE,
          weight: InputStyle.TEXT_INPUT_PLACEHOLDER_WEIGHT,
          style: FontStyle.Italic
        })
        .caretColor(Color.Blue)
        .backgroundColor(Color.White)
        .height(InputStyle.TEXT_INPUT_PLACEHOLDER_HEIGHT)
        .fontSize(FontSize.LARGE)
        .borderRadius(InputStyle.TEXT_INPUT_PLACEHOLDER_RADIUS)
        .fontWeight(FontWeight.Bold)
        .fontStyle(FontStyle.Normal)
        .fontColor(Color.Black)
        .padding({ right: InputStyle.TEXT_INPUT_PLACEHOLDER_PADDING_RIGHT })
        .margin({
          right: InputStyle.TEXT_INPUT_PLACEHOLDER_MARGIN_RIGHT,
          left: InputStyle.TEXT_INPUT_PLACEHOLDER_MARGIN_LEFT
        })
        .onChange((value: string) => {
          this.inputText = value;
        })
      Button('确定', { type: ButtonType.Normal, stateEffect: true })
        .offset({ x: InputStyle.BUTTON_CONFIRM_OFFSET_X, y: 0 })
        .width(InputStyle.BUTTON_CONFIRM_WIDTH)
        .height(InputStyle.BUTTON_CONFIRM_HEIGHT)
        .fontColor(Color.White)
        .backgroundColor($r('app.color.input_button_confirm'))
        .borderRadius(InputStyle.BUTTON_CONFIRM_RADIUS)
        .onClick(() => {
          if (this.confirm) {
            this.confirm();
          }
        })
    }
    .width(InputStyle.ROW_WIDTH)
    .height(InputStyle.ROW_HEIGHT)
    .backgroundColor(Color.White)
    .borderRadius(InputStyle.ROW_RADIUS)
  }
}

export default Input;

---

## samples/ArkUIExperience/entry/src/main/ets/view/ErrorDialog.ets

Source: harmonyos-tutorial/samples/ArkUIExperience/entry/src/main/ets/view/ErrorDialog.ets

/*
 * Copyright (c) 2022 Huawei Device Co., Ltd.
 * Licensed under the Apache License,Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

/**
 * 错误提示弹框组件
 */
import { FontSize, DialogStyle } from '../common/constants/Constants';

@CustomDialog
struct ErrorDialog {
  controller: CustomDialogController;
  confirm?: () => void;
  textStr: ResourceStr;
  title: ResourceStr;

  build() {
    Column() {
      Flex({
        direction: FlexDirection.Row,
        justifyContent: FlexAlign.SpaceBetween,
        alignItems: ItemAlign.Center
      }) {
        Text('')
          .width(DialogStyle.TEXT_EMPTY_TITLE_WIDTH)
        Text(this.title)
          .fontSize(FontSize.MIDDLE)
          .fontColor(Color.Black)
          .fontWeight(FontWeight.Bolder)
          .lineHeight(DialogStyle.TEXT_EMPTY_LINE_HEIGHT)
        Image($r('app.media.ic_public_cancel'))
          .width(DialogStyle.IMAGE_CANCEL_WIDTH)
          .height(DialogStyle.IMAGE_CANCEL_HEIGHT)
          .margin({ right: DialogStyle.IMAGE_CANCEL_MARGIN_RIGHT })
          .onClick(() => {
            this.controller.close();
          })
      }
      .backgroundImage($r('app.media.dialog_BG'))
      .backgroundImageSize(ImageSize.Cover)
      .linearGradient({
        angle: DialogStyle.FLEX_GRADIENT_ANGLE,
        colors: [[0xFDBF35, DialogStyle.FLEX_GRADIENT_START], [0xFF8C00, DialogStyle.FLEX_GRADIENT_END]]
      })
      .width(DialogStyle.FLEX_GRADIENT_WIDTH)
      .height(DialogStyle.FLEX_GRADIENT_HEIGHT)

      Image($r('app.media.ic_nft_gone'))
        .width(DialogStyle.IMAGE_GONE_WIDTH)
        .height(DialogStyle.IMAGE_GONE_HEIGHT)
        .margin({ top: DialogStyle.IMAGE_GONE_MARGIN_TOP })
      Text(this.textStr)
        .fontSize(FontSize.MIDDLE)
        .margin(DialogStyle.TEXT_CONTENT_MARGIN)
      Flex({ justifyContent: FlexAlign.SpaceAround }) {
        Button($r('app.string.button_text_confirm'))
          .fontSize(FontSize.MIDDLE)
          .backgroundColor(0xffffff)
          .fontColor($r('app.color.nft_orange'))
          .onClick(() => {
            this.controller.close();
            if (this.confirm) {
              this.confirm();
            }
          })
      }.margin({ bottom: DialogStyle.FLEX_BUTTON_BOX_MARGIN_BOTTOM })
    }
  }
}

export default ErrorDialog;

---

## samples/ArkUIExperience/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIExperience/entry/src/main/ets/pages/Index.ets

/*
 * Copyright (c) 2022 Huawei Device Co., Ltd.
 * Licensed under the Apache License,Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
/**
 * 应用首页
 */
import router from '@ohos.router';
import LogUtil from '../common/utils/LogUtil';
import InfoDialog from '../view/InfoDialog';
import Input from '../view/Input';
import ErrorDialog from '../view/ErrorDialog';
import { FontSize, IndexStyle } from '../common/constants/Constants';

const TAG = 'index.ets';

@Entry
@Component
struct Index {
  @State name: string = '';
  @State isShowInput: boolean = true;
  dialogController: CustomDialogController = new CustomDialogController({
    builder: InfoDialog({
      textStr: $r('app.string.dialog_content_avatar'),
      title: $r('app.string.dialog_title_avatar')
    }),
    autoCancel: true
  });

  build() {
    Column() {
      Image($r('app.media.home_title'))
        .width(IndexStyle.IMAGE_TITLE_WIDTH)
        .height(IndexStyle.IMAGE_TITLE_HEIGHT)
        .margin({ top: IndexStyle.IMAGE_TITLE_MARGIN_TOP })
      Image('/common/animation/animation1.png')
        .width(IndexStyle.IMAGE_ANIMATION_WIDTH)
        .height(IndexStyle.IMAGE_ANIMATION_HEIGHT)
      // 输入框
      if (this.isShowInput) {
        Input({ inputText: $name, confirm: () => this.confirm() })
      }
      Text($r('app.string.text_nft_explain'))
        .fontSize(FontSize.LARGE)
        .fontColor($r('app.color.index_text_explain'))
        .margin({ top: IndexStyle.IMAGE_EXPLAIN_MARGIN_TOP })
        .onClick(() => {
          this.dialogController.open();
        })
      Blank().margin({ bottom: IndexStyle.BLANK_EMPTY_MARGIN_BOTTOM })
      Button($r('app.string.button_start_Lottery'), { stateEffect: true })
        .backgroundColor($r('app.color.index_button_background'))
        .opacity(IndexStyle.BUTTON_LOTTERY_OPACITY)
        .width(IndexStyle.BUTTON_LOTTERY_WIDTH)
        .height(IndexStyle.BUTTON_LOTTERY_HEIGHT)
        .borderRadius(IndexStyle.BUTTON_LOTTERY_RADIUS)
        .visibility(Visibility.Visible)
        .linearGradient({
          angle: IndexStyle.BUTTON_LOTTERY_ANGLE,
          colors: [[0xFDBF35, IndexStyle.BUTTON_LOTTERY_GRADIENT_START], [0xFF8C00, IndexStyle.BUTTON_LOTTERY_GRADIENT_END]]
        })
        .margin({ bottom: IndexStyle.BUTTON_LOTTERY_MARGIN_BOTTOM })
        .onClick(() => {
          this.startLottery();
        })
    }
    .backgroundImage($r('app.media.home_BG'))
    .backgroundImageSize({
      width: IndexStyle.BUTTON_BACKGROUND_IMAGE_WIDTH,
      height: IndexStyle.BUTTON_BACKGROUND_IMAGE_WIDTH
    })
    .width(IndexStyle.BUTTON_MAIN_WIDTH)
    .height(IndexStyle.BUTTON_MAIN_HEIGHT)

  }

  // 确定事件
  confirm() {
    LogUtil.info(TAG, 'click confirm:startLottery');
    this.startLottery();
  }

  // 跳转抽奖页面
  startLottery() {
    LogUtil.info(TAG, 'startLottery');
    // 获取输入框组件文本值name，传递给抽奖页面
    if (!this.name) {
      this.showFailDialog($r('app.string.dialog_title_code_fail'), $r('app.string.dialog_content_name'));
    } else {
      router.replace({
        url: 'pages/Nft',
        params:{
          name:this.name
        }
      });
    }
  }

  // 显示错误提示弹框
  showFailDialog(title: ResourceStr, text: ResourceStr, callback?: () => void) {
    LogUtil.info(TAG, 'showFailDialog');
    let failDialog: CustomDialogController;
    failDialog = new CustomDialogController({
      builder: ErrorDialog({ confirm: callback,
        textStr: text,
        title: title,
        controller: failDialog
      }),
      cancel: callback,
      autoCancel: true
    });
    failDialog.open();
  }
}

---

## samples/ArkUIExperience/entry/src/main/ets/pages/Nft.ets

Source: harmonyos-tutorial/samples/ArkUIExperience/entry/src/main/ets/pages/Nft.ets

/*
 * Copyright (c) 2022 Huawei Device Co., Ltd.
 * Licensed under the Apache License,Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

/**
 * 应用抽取数字藏品页面
 */
import router from '@ohos.router';
import { axios } from '@ohos/axios';
import LogUtil from '../common/utils/LogUtil';
import InfoDialog from '../view/InfoDialog';
import ErrorDialog from '../view/ErrorDialog';
import NftResponse from '../common/bean/NftResponse';
import { Sever, DacUrl, FontSize, NftStyle } from '../common/constants/Constants';

const TAG = 'nft.ets';

@Entry
@Component
struct Nft {
  private preDecode = 5; // 预解码
  private iterations = 1;
  @State nftImgUrl: string = ''; // nft地址
  @State quality: string = '无'; // 稀有度
  @State qrValue: string = DacUrl; // 二维码内容
  @State nftIndex: string = ''; // nfc序号
  @State name: string = 'name'; // index页面输入的名字
  @State state: AnimationStatus = AnimationStatus.Initial; // 初始状态
  // 图片帧信息集合
  @State animatorImageArr: Array<{
    src: string;
    width?: number | string;
    height?: number | string;
    top?: number | string;
    left?: number | string;
    duration?: number;
  }> = [];
  @State isAnimatorFinish: boolean = false; // 是否播放完动画
  @State isShowNFTGone: boolean = false; // 是否显示奖品抢光的提示

  // 稀有度弹框
  nfcInfoDialog: CustomDialogController = new CustomDialogController({
    builder: InfoDialog({
      textStr: $r('app.string.dialog_content_quality'),
      title: '',
    }),
    autoCancel: true
  });

  build() {
    Stack() {
      // 先显示开箱动画
      if (!this.isAnimatorFinish) {
        Column() {
          ImageAnimator()
            .images(this.animatorImageArr)
            .state(this.state)
            .reverse(false)
            .fixedSize(false)
            .preDecode(this.preDecode)
            .fillMode(FillMode.None)
            .iterations(this.iterations)
            .width(NftStyle.ANIMATOR_WIDTH)
            .height(NftStyle.ANIMATOR_HEIGHT)
            .margin({ top: NftStyle.ANIMATOR_MARGIN_TOP })
            .onStart(() => { // 当帧动画开始播放后触发
              LogUtil.info(TAG, 'ImageAnimator Start');
            })
            .onPause(() => {
              LogUtil.info(TAG, 'ImageAnimator Pause');
            })
            .onRepeat(() => {
              LogUtil.info(TAG, 'ImageAnimator Repeat');
            })
            .onCancel(() => {
              LogUtil.info(TAG, 'ImageAnimator Cancel');
            })
            .onFinish(() => { // 当帧动画播放完成后触发
              this.animatorFinish();
            })
        }
        .backgroundColor($r('app.color.nft_animator_background'))
        .height(NftStyle.ANIMATOR_LAYOUT_WIDTH)
        .width(NftStyle.ANIMATOR_LAYOUT_HEIGHT)
      } else {
        // 开箱结果
        // 背景图
        Image($r('app.media.nft_BG'))
          .width(NftStyle.IMAGE_BACKGROUND_WIDTH)
          .height(NftStyle.IMAGE_BACKGROUND_HEIGHT)
          .objectFit(ImageFit.Fill)
        Column() {
          // 数字藏品已抢光
          if (this.isShowNFTGone) {
            Text($r('app.string.text_title_sorry'))
              .fontWeight(FontWeight.Bolder)
              .fontSize(FontSize.MIDDLE)
              .margin({ top: NftStyle.TEXT_GONE_MARGIN_TOP })
            Column() {
              Image($r('app.media.ic_nft_gone'))
                .aspectRatio(NftStyle.IMAGE_GONE_ASPECT_RATIO)
                .width(NftStyle.IMAGE_GONE_WIDTH)
              Text($r('app.string.text_no_prize'))
                .fontColor($r('app.color.nft_text_no_prize'))
                .fontSize(FontSize.MINI)
                .margin({ top: NftStyle.TEXT_NO_PRIZE_MARGIN_TOP })
            }
            .width(NftStyle.COLUMN_GONE_WIDTH)
            .aspectRatio(NftStyle.COLUMN_GONE_ASPECT_RATIO)
            .border({
              radius: NftStyle.COLUMN_GONE_BORDER_RADIUS,
              width: NftStyle.COLUMN_GONE_BORDER_WIDTH,
              color: $r('app.color.nft_border')
            }).margin({ top: NftStyle.COLUMN_GONE_MARGIN_TOP })
          } else {
            // 中奖提示
            Text($r('app.string.text_title_congratulations'))
              .fontWeight(FontWeight.Bolder)
              .fontSize(FontSize.MIDDLE)
              .margin({ top: NftStyle.TEXT_CONGRATULATIONS_MARGIN_TOP })
            Text($r('app.string.text_title_avatar', this.name))
              .fontSize(FontSize.SMALL)
              .margin({ top: NftStyle.TEXT_AVATAR_MARGIN_TOP })
            Image(this.nftImgUrl)
              .width(NftStyle.IMAGE_NFT_WIDTH)
              .aspectRatio(NftStyle.IMAGE_NFT_ASPECT_RATIO)
              .borderRadius(NftStyle.IMAGE_NFT_RADIUS)
              .margin({ top: NftStyle.IMAGE_NFT_MARGIN_TOP })
          }
          Flex({
            direction: FlexDirection.Row,
            justifyContent: FlexAlign.Center,
            alignItems: ItemAlign.Center
          }) {
            // 序号
            Text($r('app.string.text_serial_number'))
              .fontColor($r('app.color.nft_subtitle'))
              .fontSize(FontSize.SMALL)
              .margin({ right: NftStyle.TEXT_INDEX_MARGIN_RIGHT })
            Text(this.nftIndex)
              .fontColor($r('app.color.nft_orange'))
              .fontSize(FontSize.SMALL)
            Text()
              .width(NftStyle.TEXT_EMPTY_WIDTH) // 中间间隔
            // 品质
            Text($r('app.string.text_quality'))
              .fontColor($r('app.color.nft_subtitle'))
              .fontSize(FontSize.SMALL)
              .margin({ right: NftStyle.TEXT_QUALITY_MARGIN_RIGHT })
            Image($r('app.media.ic_about'))
              .width(NftStyle.IMAGE_ABOUT_WIDTH)
              .height(NftStyle.IMAGE_ABOUT_HEIGHT)
              .margin({ right: NftStyle.IMAGE_ABOUT_MARGIN_RIGHT })
            Text(this.quality)
              .fontColor($r('app.color.nft_subtitle'))
              .fontSize(FontSize.MINI)
              .width(NftStyle.TEXT_QUALITY_WIDTH)
              .textAlign(TextAlign.Center)
              .backgroundColor($r('app.color.nft_text_quality'))
              .borderRadius(2)
          }
          .margin({
            top: NftStyle.FLEX_NFT_INFO_MARGIN_TOP,
            bottom: NftStyle.FLEX_NFT_INFO_MARGIN_BOTTOM
          })
          .onClick(() => {
            LogUtil.info(TAG, 'open nfcInfoDialog');
            this.nfcInfoDialog.open();
          })
          // 二维码
          Flex({ direction: FlexDirection.Column, justifyContent: FlexAlign.Center }) {
            QRCode(this.qrValue).color(0x000000).width(NftStyle.QRCODE_WIDTH).aspectRatio(1)
            Text($r('app.string.text_content_qrcode'))
              .width(NftStyle.TEXT_QRCODE_WIDTH)
              .fontColor($r('app.color.nft_text_qrcode'))
              .fontSize(FontSize.MINI)
              .textAlign(TextAlign.Center)
              .margin({ top: NftStyle.TEXT_QRCODE_MARGIN_TOP })
          }.layoutWeight(NftStyle.FLEX_QRCODE_LAYOUT_WEIGHT)
        }
        .width(NftStyle.COLUMN_RESULT_WIDTH)
        .height(NftStyle.COLUMN_RESULT_WIDTH)
        .backgroundColor($r('app.color.nft_main_background'))
        .borderRadius(NftStyle.COLUMN_RESULT_RADIUS)
      }
    }
    .width(NftStyle.NFT_WIDTH)
    .height(NftStyle.NFT_HEIGHT)
  }

  aboutToAppear() {
    // 获取index页面参数
    let routerParams = router.getParams();
    LogUtil.info(TAG, 'routerParams = ' + JSON.stringify(routerParams));
    // 若路由未传递name参数则弹出提示框
    if (!routerParams || !routerParams['name']) {
      this.showFailDialog($r('app.string.dialog_title_code_fail'), $r('app.string.dialog_content_router_fail'), () => {
        router.replace({ url: 'pages/index' });
      })
      return;
    }
    this.name = router.getParams()['name'];

    // 插入播放动画代码
    this.playAnimator();

    // 检测是否有播放动画
    setTimeout(() => {
      if (this.state == AnimationStatus.Initial) {
        // 未播放动画弹框
        this.showFailDialog($r('app.string.dialog_title_code_fail'), $r('app.string.dialog_content_animation_fail'));
      }
    }, 3000);
  }

  // 显示错误提示弹框
  showFailDialog(title: ResourceStr, text: ResourceStr, callback?: () => void) {
    let failDialog: CustomDialogController;
    failDialog = new CustomDialogController({
      builder: ErrorDialog({ confirm: callback,
        textStr: text,
        title: title,
        controller: failDialog
      }),
      cancel: callback,
      autoCancel: true
    });
    failDialog.open();
  }

  // 初始化动画并播放
  playAnimator() {
    // 初始化动画数据
    let animatorImageArr = [];
    for (let i = 0; i < 25; i++) {
      let src = `/common/animation/animation${i + 1}.png`
      let item = {
        src: src,
        duration: 250,
        width: 240,
        height: 240,
        top: 0,
        left: 0
      };
      animatorImageArr.push(item);
    }
    this.animatorImageArr = animatorImageArr;
    // 开始播放帧动画
    this.state = AnimationStatus.Running;
  }

  // 动画播放完成
  animatorFinish() {
    LogUtil.info(TAG, 'ImageAnimator Finish');
    this.state = AnimationStatus.Stopped; // 暂停动画
    this.isAnimatorFinish = true;
    this.getNFT(this.name); // 开始获取NFT
  }

  // 获取NFT数据
  async getNFT(name: string) {
    LogUtil.info(TAG, 'start getNFT');
    // 调用Axios进行网络请求，获取数字藏品数据
    axios({
      method: 'get',
      url: Sever.GET_NFT_URL,
      params: {
        name: name
      }
    }).then(response => {
      LogUtil.info(TAG, 'result:' + JSON.stringify(response));
      let res = response.data as NftResponse;
      // 若服务器返回结果为失败，则为数字藏品已抢完，并显示相应提示
      if (!res.success) {
        LogUtil.error(TAG, 'getNFT fail:' + JSON.stringify(res.message));
        this.isShowNFTGone = true;
        return;
      }
      ;
      // 获取数字藏品成功，相应组件设置对应数据
      this.isShowNFTGone = false;
      const imgPrefixUrl = Sever.PREFIX_URL;
      const webPageUrl = Sever.WEB_PAGE_URL; // web页面地址
      this.nftImgUrl = imgPrefixUrl + '/' + res.result.imageId; // 头像预览地址
      let nftIndex = res.result.imageId; // 设置序号
      this.nftIndex = nftIndex.substring(0, nftIndex.indexOf('.')); // 删除文件名后缀并赋值
      this.quality = res.result.quality; // 设置二维码内容
      this.qrValue = webPageUrl + `?assertId=${res.result.assertId}`; // 设置二维码内容
    }).catch(error => {
      LogUtil.error(TAG, 'getNFT error:' + error);
      // 显示NFT抢完提醒
      this.isShowNFTGone = true;
    })
  }
}

---

## samples/ArkUIDrawingComponents/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkUIDrawingComponents/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkUIDrawingComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkUIDrawingComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkUIDrawingComponents/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIDrawingComponents/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  @State message: string = 'Hello World'

  build() {
    Row() {
      Column() {
        /*** 以下为 Circle和Ellipse 示例***/
        /*// 绘制一个直径为150的圆
        Circle({ width: 150, height: 150 })

        // 绘制一个直径为150、线条为红色虚线的圆环（宽高设置不一致时以短边为直径）
        Circle()
          .width(150)
          .height(200)
          .fillOpacity(0) // 设置填充区域透明度
          .strokeWidth(3) //设置边框宽度
          .stroke(Color.Red) //设置边框颜色
          .strokeDashArray([1, 2]) //设置边框间隙

        // 绘制一个 150 * 80 的椭圆
        Ellipse({ width: 150, height: 50 })

        // 绘制一个 150 * 100 、线条为蓝色的椭圆环
        Ellipse()
          .width(150)
          .height(50)
          .fillOpacity(0) // 设置填充区域透明度
          .strokeWidth(3) //设置边框宽度
          .stroke(Color.Red) //设置边框颜色
          .strokeDashArray([1, 2]) //设置边框间隙*/

        /*** 以下为 Line 示例***/
        // 线条绘制的起止点坐标均是相对于Line组件本身绘制区域的坐标
        /*Line()
          .startPoint([0, 0])
          .endPoint([50, 100])
          .backgroundColor('#F5F5F5')
        Line()
          .width(200)
          .height(200)
          .startPoint([50, 50])
          .endPoint([150, 150])
          .strokeWidth(5)
          .stroke(Color.Orange)
          .strokeOpacity(0.5)
          .backgroundColor('#F5F5F5')

        // 当坐标点设置的值超出Line组件的宽高范围时，线条会画出组件绘制区域
        Line({ width: 50, height: 50 })
          .startPoint([0, 0])
          .endPoint([100, 100])
          .strokeWidth(3)
          .strokeDashArray([10, 3])
          .backgroundColor('#F5F5F5')

        // strokeDashOffset用于定义关联虚线strokeDashArray数组渲染时的偏移
        Line({ width: 50, height: 50 })
          .startPoint([0, 0])
          .endPoint([100, 100])
          .strokeWidth(3)
          .strokeDashArray([10, 3])
          .strokeDashOffset(5)
          .backgroundColor('#F5F5F5')*/

        /*** 以下为 Polyline 示例***/
        // 在 100 * 100 的矩形框中绘制一段折线，起点(0, 0)，经过(20,60)，到达终点(100, 100)
        /*Polyline({ width: 100, height: 100 })
          .points([[0, 0], [20, 60], [100, 100]])
          .fillOpacity(0)
          .stroke(Color.Blue)
          .strokeWidth(3)

        // 在 100 * 100 的矩形框中绘制一段折线，起点(20, 0)，经过(0,100)，到达终点(100, 90)
        Polyline()
          .width(100)
          .height(100)
          .fillOpacity(0)
          .stroke(Color.Red)
          .strokeWidth(8)
          .points([[20, 0], [0, 100], [100, 90]])
            // 设置折线拐角处为圆弧
          .strokeLineJoin(LineJoinStyle.Round)
            // 设置折线两端为半圆
          .strokeLineCap(LineCapStyle.Round)*/

        /*** 以下为 Polygon 示例***/
        // 在 100 * 100 的矩形框中绘制一个三角形，起点(0, 0)，经过(50, 100)，终点(100, 0)
        /*Polygon({ width: 100, height: 100 })
          .points([[0, 0], [50, 100], [100, 0]])
          .fill(Color.Green)
          .stroke(Color.Transparent)

        // 在 100 * 100 的矩形框中绘制一个四边形，起点(0, 0)，经过(0, 100)和(100, 100)，终点(100, 0)
        Polygon()
          .width(100)
          .height(100)
          .points([[0, 0], [0, 100], [100, 100], [100, 0]])
          .fillOpacity(0)
          .strokeWidth(5)
          .stroke(Color.Blue)

        // 在 100 * 100 的矩形框中绘制一个五边形，起点(50, 0)，依次经过(0, 50)、(20, 100)和(80, 100)，终点(100, 50)
        Polygon()
          .width(100)
          .height(100)
          .points([[50, 0], [0, 50], [20, 100], [80, 100], [100, 50]])
          .fill(Color.Red)
          .fillOpacity(0.6)
          .stroke(Color.Transparent)*/

        /*** 以下为 Path 示例***/
        // 绘制一条长900px，宽3vp的直线
        /*Path()
          .height(10)
          .commands('M0 0 L600 0')
          .stroke(Color.Black)
          .strokeWidth(3)

        // 绘制直线图形
        Path()
          .commands('M100 0 L200 240 L0 240 Z')
          .fillOpacity(0)
          .stroke(Color.Black)
          .strokeWidth(3)
        Path()
          .commands('M0 0 H200 V200 H0 Z')
          .fillOpacity(0)
          .stroke(Color.Black)
          .strokeWidth(3)
        Path()
          .commands('M100 0 L0 100 L50 200 L150 200 L200 100 Z')
          .fillOpacity(0)
          .stroke(Color.Black)
          .strokeWidth(3)


        // 绘制弧线图形
        Path()
          .commands("M0 300 S100 0 240 300 Z")
          .fillOpacity(0)
          .stroke(Color.Black)
          .strokeWidth(3)
        Path()
          .commands('M0 150 C0 100 140 0 200 150 L100 300 Z')
          .fillOpacity(0)
          .stroke(Color.Black)
          .strokeWidth(3)
        Path()
          .commands('M0 100 A30 20 20 0 0 200 100 Z')
          .fillOpacity(0)
          .stroke(Color.Black)
          .strokeWidth(3)*/


        // 绘制90% * 50矩形
        /*Rect({ width: '90%', height: 50 })
          .fill(Color.Pink)
          .stroke(Color.Transparent)

        // 绘制90% * 50的矩形框
        Rect()
          .width('90%')
          .height(50)
          .fillOpacity(0)
          .stroke(Color.Red)
          .strokeWidth(3)

        // 绘制90% * 80的矩形, 圆角宽高分别为40、20
        Rect({ width: '90%', height: 80 })
          .radiusHeight(20)
          .radiusWidth(40)
          .fill(Color.Pink)
          .stroke(Color.Transparent)

        // 绘制90% * 80的矩形, 圆角宽高为20
        Rect({ width: '90%', height: 80 })
          .radius(20)
          .fill(Color.Pink)
          .stroke(Color.Transparent)

        // 绘制90% * 50矩形, 左上圆角宽高40,右上圆角宽高20,右下圆角宽高40,左下圆角宽高20
        Rect({ width: '90%', height: 80 })
          .radius([[40, 40], [20, 20], [40, 40], [20, 20]])
          .fill(Color.Pink)
          .stroke(Color.Transparent)*/

        // 在Shape的(-2, 118)点绘制一个 300 * 10 直线路径,颜色0x317AF7,边框颜色黑色,宽度4,间隙20,向左偏移10,线条两端样式为半圆,拐角样式圆角,抗锯齿(默认开启)
        /*Shape() {
          Rect().width(300).height(50)
          Ellipse().width(300).height(50).offset({ x: 0, y: 60 })
          Path().width(300).height(10).commands('M0 0 L900 0').offset({ x: 0, y: 120 })
        }
        .viewPort({ x: -2, y: -2, width: 304, height: 130 })
        .fill(0x317AF7)
        .stroke(Color.Black)
        .strokeWidth(4)
        .strokeDashArray([20])
        .strokeDashOffset(10)
        .strokeLineCap(LineCapStyle.Round)
        .strokeLineJoin(LineJoinStyle.Round)
        .antiAlias(true)

        // 分别在Shape的(0, 0)、(-5, -5)点绘制一个 300 * 50 带边框的矩形,可以看出之所以将视口的起始位置坐标设为负值是因为绘制的起点默认为线宽的中点位置，因此要让边框完全显示则需要让视口偏移半个线宽
        Shape() {
          Rect().width(300).height(50)
        }
        .viewPort({ x: 0, y: 0, width: 320, height: 70 })
        .fill(0x317AF7)
        .stroke(Color.Black)
        .strokeWidth(10)

        // 在Shape的(0, -5)点绘制一条直线路径,颜色0xEE8443,线条宽度10,线条间隙20
        Shape() {
          Path().width(300).height(10).commands('M0 0 L900 0')
        }
        .viewPort({ x: 0, y: -5, width: 300, height: 20 })
        .stroke(0xEE8443)
        .strokeWidth(10)
        .strokeDashArray([20])

        // 在Shape的(0, -5)点绘制一条直线路径,颜色0xEE8443,线条宽度10,线条间隙20,向左偏移10
        Shape() {
          Path().width(300).height(10).commands('M0 0 L900 0')
        }
        .viewPort({ x: 0, y: -5, width: 300, height: 20 })
        .stroke(0xEE8443)
        .strokeWidth(10)
        .strokeDashArray([20])
        .strokeDashOffset(10)

        // 在Shape的(0, -5)点绘制一条直线路径,颜色0xEE8443,线条宽度10,透明度0.5
        Shape() {
          Path().width(300).height(10).commands('M0 0 L900 0')
        }
        .viewPort({ x: 0, y: -5, width: 300, height: 20 })
        .stroke(0xEE8443)
        .strokeWidth(10)
        .strokeOpacity(0.5)

        // 在Shape的(0, -5)点绘制一条直线路径,颜色0xEE8443,线条宽度10,线条间隙20,线条两端样式为半圆
        Shape() {
          Path().width(300).height(10).commands('M0 0 L900 0')
        }
        .viewPort({ x: 0, y: -5, width: 300, height: 20 })
        .stroke(0xEE8443)
        .strokeWidth(10)
        .strokeDashArray([20])
        .strokeLineCap(LineCapStyle.Round)

        // 在Shape的(-80, -5)点绘制一个封闭路径,颜色0x317AF7,线条宽度10,边框颜色0xEE8443,拐角样式锐角（默认值）
        Shape() {
          Path().width(200).height(60).commands('M0 0 L400 0 L400 150 Z')
        }
        .viewPort({ x: -80, y: -5, width: 310, height: 90 })
        .fill(0x317AF7)
        .stroke(0xEE8443)
        .strokeWidth(10)
        .strokeLineJoin(LineJoinStyle.Miter)
        .strokeMiterLimit(5)*/
      }
      .width('100%')
    }
    .height('100%')
  }
}

---

## samples/ArkUIHelloWorld/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkUIHelloWorld/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkUIHelloWorld/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkUIHelloWorld/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkUIHelloWorld/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIHelloWorld/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  @State message: string = 'Hello World';

  build() {
    RelativeContainer() {
      Text(this.message)
        .id('HelloWorld')
        .fontSize(50)
        .fontWeight(FontWeight.Bold)
        .alignRules({
          center: { anchor: '__container__', align: VerticalAlign.Center },
          middle: { anchor: '__container__', align: HorizontalAlign.Center }
        })
    }
    .height('100%')
    .width('100%')
  }
}

---

## 属性动画

Source: HarmonyOSNextStudyNote/9.动画.md

9.动画
===

### 属性动画

```TypeScript
Image($r('app.media.image1'))   
   .animation({   
      duration: 1000,    
      // 动画的播放速度，值越大动画播放越快，值越小播放越慢，为0时无动画效果。
      tempo: 1.0,    
      delay: 0,    
      curve: Curve.Linear,   
      // 设置动画播放模式，默认播放完成后重头开始播放。 
      playMode: PlayMode.Normal,  
      // 播放次数，默认一次，设置为-1时表示无限次播放。  
      iterations: 1  
   })
```

1、animation属性作用域。animation自身也是组件的一个属性，其作用域为animation之前。即产生属性动画的属性须在animation之前声明，其后声明的将不会产生属性动画。

2、产生属性动画的属性变化时需触发UI状态更新。在本示例中，产生动画的属性width，其值是通过变量iconWidth从30变为100，故该变量iconWidth的改变需触发UI状态更新。

3、产生属性动画的属性本身需满足一定的要求，并非任何属性都可以产生属性动画。目前支持的属性包括width、height、position、opacity、backgroundColor、scale、rotate、translate等


### 页面动画






----------


- [上一篇:8.通知](https://github.com/CharonChui/HarmonyOSNextStudyNote/blob/main/8.%E9%80%9A%E7%9F%A5.md)
- [下一篇:10.网络请求](https://github.com/CharonChui/HarmonyOSNextStudyNote/blob/main/10.%E7%BD%91%E7%BB%9C%E8%AF%B7%E6%B1%82.md)



    
---

- 邮箱 ：charon.chui@gmail.com  
- Good Luck! 


---

## UIAbility

Source: HarmonyOSNextStudyNote/7.应用组件UIAbility.md

7.应用组件UIAbility
===


## UIAbility

UIAbility是一种包含用户界面的应用组件，主要用于和用户进行交互。 

UIAbility也是系统调度的单元，为应用提供窗口在其中绘制界面。 


UIAbility是一种包含用户界面的应用组件，主要用于和用户进行交互。UIAbility也是系统调度的单元，为应用提供窗口在其中绘制界面。

每一个UIAbility实例，都对应于一个最近任务列表中的任务。

一个应用可以有一个UIAbility，也可以有多个UIAbility，如下图所示。例如浏览器应用可以通过一个UIAbility结合多页面的形式让用户进行的搜索和浏览内容；而聊天应用增加一个“外卖功能”的场景，则可以将聊天应用中“外卖功能”的内容独立为一个UIAbility，当用户打开聊天应用的“外卖功能”，查看外卖订单详情，此时有新的聊天消息，即可以通过最近任务列表切换回到聊天窗口继续进行聊天对话。

一个UIAbility可以对应于多个页面，建议将一个独立的功能模块放到一个UIAbility中，以多页面的形式呈现。例如新闻应用在浏览内容的时候，可以进行多页面的跳转使用。

![image](https://github.com/CharonChui/Pictures/blob/master/UIAbility_single_more.png?raw=true)

## UIAbility内页面的跳转和数据传递

### Page

在一个应用包含一个UIAbility的场景下，可以通过新建多个页面来实现和丰富应用的内容

src/main/ets/pages目录中的文件

### 路由

页面间的导航可以通过页面路由router模块来实现。页面路由模块根据页面url找到目标页面，从而实现跳转。通过页面路由模块，可以使用不同的url访问不同的页面，包括跳转到UIAbility内的指定页面、用UIAbility内的某个页面替换当前页面、返回上一页面或指定的页面等。
url路径需要在entry/src/resources/base/profile/main_pages.json中配置


页面跳转的几种方式，根据需要选择一种方式跳转即可。

方式一：API9及以上，router.pushUrl()方法新增了mode参数，可以将mode参数配置为router.RouterMode.Single单实例模式和router.RouterMode.Standard多实例模式。
在单实例模式下：如果目标页面的url在页面栈中已经存在同url页面，离栈顶最近同url页面会被移动到栈顶，移动后的页面为新建页，原来的页面仍然存在栈中，页面栈的元素数量不变；如果目标页面的url在页面栈中不存在同url页面，按多实例模式跳转，页面栈的元素数量会加1。

说明
当页面栈的元素数量较大或者超过32时，可以通过调用router.clear()方法清除页面栈中的所有历史页面，仅保留当前页面作为栈顶页面。


```TypeScript
router.pushUrl({
  url: 'pages/Second',
  params: {
    src: 'Index页面传来的数据',
  }
}, router.RouterMode.Single)
```


方式二：API9及以上，router.replaceUrl()方法新增了mode参数，可以将mode参数配置为router.RouterMode.Single单实例模式和router.RouterMode.Standard多实例模式。
在单实例模式下：如果目标页面的url在页面栈中已经存在同url页面，离栈顶最近同url页面会被移动到栈顶，替换当前页面，并销毁被替换的当前页面，移动后的页面为新建页，页面栈的元素数量会减1；如果目标页面的url在页面栈中不存在同url页面，按多实例模式跳转，页面栈的元素数量不变。

```TypeScript
router.replaceUrl({
  url: 'pages/Second',
  params: {
    src: 'Index页面传来的数据',
  }
}, router.RouterMode.Single)
```

第二个页面获取
```TypeScript
import router from '@ohos.router';

@Entry
@Component
struct Second {
  src: string = (router.getParams() as Record<string, string>)['src'];
  // 页面刷新展示
  ...
}
```
第二个页面返回: 
```TypeScript
router.back()
```

页面返回和参数接收
经常还会遇到一个场景，在Second页面中，完成了一些功能操作之后，希望能返回到Index页面，那我们要如何实现呢？

在Second页面中，可以通过调用router.back()方法实现返回到上一个页面，或者在调用router.back()方法时增加可选的options参数（增加url参数）返回到指定页面。

说明
调用router.back()返回的目标页面需要在页面栈中存在才能正常跳转。
例如调用router.pushUrl()方法跳转到Second页面，在Second页面可以通过调用router.back()方法返回到上一个页面。
例如调用router.clear()方法清空了页面栈中所有历史页面，仅保留当前页面，此时则无法通过调用router.back()方法返回到上一个页面。
返回上一个页面。

```TypeScript
router.back();
```

返回到指定页面。
```TypeScript
router.back({
  url: 'pages/Index',
  params: {
    src: 'Second页面传来的数据',
  }
})
```

从Second页面返回到Index页面。在Index页面通过调用router.getParams()方法，获取Second页面传递过来的自定义参数。

##### 说明
调用router.back()方法，不会新建页面，返回的是原来的页面，在原来页面中@State声明的变量不会重复声明，以及也不会触发页面的aboutToAppear()生命周期回调，因此无法直接在变量声明以及页面的aboutToAppear()生命周期回调中接收和解析router.back()传递过来的自定义参数。

可以放在业务需要的位置进行参数解析。示例代码在Index页面中的onPageShow()生命周期回调中进行参数的解析。
```TypeScript
import router from '@ohos.router';
@Entry
@Component
struct Index {
  @State src: string = '';
  onPageShow() {
    this.src = (router.getParams() as Record<string, string>)['src'];
  }
  // 页面刷新展示
  ...
}
```


即在调用router.back()方法之前，可以先调用router.enableBackPageAlert()方法开启页面返回询问对话框功能。

#### 说明
router.enableBackPageAlert()方法开启页面返回询问对话框功能，只针对当前页面生效。例如在调用router.pushUrl()或者router.replaceUrl()方法，跳转后的页面均为新建页面，因此在页面返回之前均需要先调用router.enableBackPageAlert()方法之后，页面返回询问对话框功能才会生效。
如需关闭页面返回询问对话框功能，可以通过调用router.disableAlertBeforeBackPage()方法关闭该功能即可。
```TypeScript
router.enableBackPageAlert({
  message: 'Message Info'
});

router.back();
```

### UIAbility的生命周期

为了实现多设备形态上的裁剪和多窗口的可扩展性，系统对组件管理和窗口管理进行了解耦。

UIAbility的生命周期包括Create、Foreground、Background、Destroy四个状态，WindowStageCreate和WindowStageDestroy为窗口管理器（WindowStage）在UIAbility中管理UI界面功能的两个生命周期回调，从而实现UIAbility与窗口之间的弱耦合。

所以要注意WindowStageCreate和WindowStageDestroy并不是UIAbility的生命周期状态。 


![image](https://github.com/CharonChui/Pictures/blob/master/uiability_shengmingzhouqi.png?raw=true)



- Create状态，在UIAbility实例创建时触发，系统会调用onCreate回调。可以在onCreate回调中进行相关初始化操作。例如用户打开电池管理应用，在应用加载过程中，在UI页面可见之前，可以在onCreate回调中读取当前系统的电量情况，用于后续的UI页面展示。

- UIAbility实例创建完成之后，在进入Foreground之前，系统会创建一个WindowStage。每一个UIAbility实例都对应持有一个WindowStage实例。
WindowStage为本地窗口管理器，用于管理窗口相关的内容，例如与界面相关的获焦/失焦、可见/不可见。

可以在onWindowStageCreate回调中，设置UI页面加载、设置WindowStage的事件订阅。

在onWindowStageCreate(windowStage)中通过loadContent接口设置应用要加载的页面。  

```TypeScript
import UIAbility from '@ohos.app.ability.UIAbility';
import window from '@ohos.window';

export default class EntryAbility extends UIAbility {
    ...

    onWindowStageCreate(windowStage: window.WindowStage) {
        // 设置UI页面加载
        // 设置WindowStage的事件订阅（获焦/失焦、可见/不可见）
        ...

        windowStage.loadContent('pages/Index', (err, data) => {
            ...
        });
    }
    ...
}
```

例如用户打开游戏应用，正在打游戏的时候，有一个消息通知，打开消息，消息会以弹窗的形式弹出在游戏应用的上方，此时，游戏应用就从获焦切换到了失焦状态，消息应用切换到了获焦状态。对于消息应用，在onWindowStageCreate回调中，会触发获焦的事件回调，可以进行设置消息应用的背景颜色、高亮等操作。



- Foreground和Background状态，分别在UIAbility切换至前台或者切换至后台时触发。
分别对应于onForeground回调和onBackground回调。

onForeground回调，在UIAbility的UI页面可见之前，即UIAbility切换至前台时触发。可以在onForeground回调中申请系统需要的资源，或者重新申请在onBackground中释放的资源。

onBackground回调，在UIAbility的UI页面完全不可见之后，即UIAbility切换至后台时候触发。可以在onBackground回调中释放UI页面不可见时无用的资源，或者在此回调中执行较为耗时的操作，例如状态保存等。

例如用户打开地图应用查看当前地理位置的时候，假设地图应用已获得用户的定位权限授权。在UI页面显示之前，可以在onForeground回调中打开定位功能，从而获取到当前的位置信息。

当地图应用切换到后台状态，可以在onBackground回调中停止定位功能，以节省系统的资源消耗。

- onWindowStageDestroy: 对应于onWindowStageCreate回调。在UIAbility实例销毁之前，则会先进入onWindowStageDestroy回调，我们可以在该回调中释放UI页面资源。
例如在onWindowStageCreate中设置的获焦/失焦等WindowStage订阅事件。

- Destroy状态，在UIAbility销毁时触发。可以在onDestroy回调中进行系统资源的释放、数据的保存等操作。
例如用户使用应用的程序退出功能，会调用UIAbilityContext的terminalSelf()方法，从而完成UIAbility销毁。或者用户使用最近任务列表关闭该UIAbility实例时，也会完成UIAbility的销毁。


#### WindowStageCreate和WindowStageDestroy状态


![image](https://github.com/CharonChui/Pictures/blob/master/arkui_uiability_windowstage.png?raw=true)

UIAbility实例创建完成之后，在进入Foreground之前，系统会创建一个WindowStage。WindowStage创建完成后会进入onWindowStageCreate()回调，可以在该回调中设置UI加载、设置WindowStage的事件订阅。

在onWindowStageCreate()回调中通过loadContent()方法设置应用要加载的页面，并根据需要调用on('windowStageEvent')方法订阅WindowStage的事件（获焦/失焦、可见/不可见）。

- 每一个UIAbility都包含一个Context属性。 


### UIAbility的启动模式

对于浏览器或者新闻等应用，用户在打开该应用，并浏览访问相关内容后，回到桌面，再次打开该应用，显示的仍然是用户当前访问的界面。

对于应用的分屏操作，用户希望使用两个不同应用（例如备忘录应用和图库应用）之间进行分屏，也希望能使用同一个应用（例如备忘录应用自身）进行分屏。

对于文档应用，用户从文档应用中打开一个文档内容，回到文档应用，继续打开同一个文档，希望打开的还是同一个文档内容。

基于以上场景的考虑，UIAbility当前支持singleton（单实例模式）、multiton（多实例模式）和specified（指定实例模式）3种启动模式。

对启动模式的详细说明如下：

#### singleton（单实例模式）
当用户打开浏览器或者新闻等应用，并浏览访问相关内容后，回到桌面，再次打开该应用，显示的仍然是用户当前访问的界面。

这种情况下可以将UIAbility配置为singleton（单实例模式）。每次调用startAbility()方法时，如果应用进程中该类型的UIAbility实例已经存在，则复用系统中的UIAbility实例，系统中只存在唯一一个该UIAbility实例。

即在最近任务列表中只存在一个该类型的UIAbility实例。

singleton启动模式，也是默认情况下的启动模式。

singleton启动模式，每次调用startAbility()启动UIAbility时，如果应用进程中该类型的UIAbility实例已经存在，则复用系统中的UIAbility实例，并回调UIAbility的onNewWant()回调，不会进入其onCreate()和onWindowStageCreate()回调。系统中只存在唯一一个该UIAbility实例。

singleton启动模式的开发使用，在module.json5文件中的“launchType”字段配置为“singleton”即可。

{
   "module": {
     ...
     "abilities": [
       {
         "launchType": "singleton",
         ...
       }
     ]
  }
}




#### multiton（多实例模式）

用户在使用分屏功能时，希望使用两个不同应用（例如备忘录应用和图库应用）之间进行分屏，也希望能使用同一个应用（例如备忘录应用自身）进行分屏。

这种情况下可以将UIAbility配置为multiton（多实例模式）。每次调用startAbility()方法时，都会在应用进程中创建一个该类型的UIAbility实例。

即在最近任务列表中可以看到有多个该类型的UIAbility实例。

multiton启动模式，每次调用startAbility()方法时，都会在应用进程中创建一个该类型的UIAbility实例。

multiton启动模式的开发使用，在module.json5文件中的“launchType”字段配置为“multiton”即可。


#### specified（指定实例模式）

用户打开文档应用，从文档应用中打开一个文档内容，回到文档应用，继续打开同一个文档，希望打开的还是同一个文档内容；以及在文档应用中新建一个新的文档，每次新建文档，希望打开的都是一个新的空白文档内容。

这种情况下可以将UIAbility配置为specified（指定实例模式）。在UIAbility实例新创建之前，允许开发者为该实例创建一个字符串Key，新创建的UIAbility实例绑定Key之后，后续每次调用startAbility方法时，都会询问应用使用哪个Key对应的UIAbility实例来响应startAbility请求。如果匹配有该UIAbility实例的Key，则直接拉起与之绑定的UIAbility实例，否则创建一个新的UIAbility实例。运行时由UIAbility内部业务决定是否创建多实例。


specified启动模式，根据业务需要是否创建一个新的UIAbility实例。在UIAbility实例创建之前，会先进入AbilityStage的onAcceptWant回调，在onAcceptWant回调中为每一个UIAbility实例创建一个Key，后续每次调用startAbility()方法创建该类型的UIAbility实例都会询问使用哪个Key对应的UIAbility实例来响应startAbility()请求。

specified启动模式的开发使用的步骤如下所示。

在module.json5文件中的“launchType”字段配置为“specified”。
{
   "module": {
     ...
     "abilities": [
       {
         "launchType": "specified",
         ...
       }
     ]
  }
}
在调用startAbility()方法的want参数中，增加一个自定义参数来区别UIAbility实例，例如增加一个“instanceKey”自定义参数。
// 在启动指定实例模式的UIAbility时，给每一个UIAbility实例配置一个独立的Key标识
function getInstance() {
    ...
}
let context:common.UIAbilityContext = ...; // context为调用方UIAbility的UIAbilityContext
let want: Want = {
    deviceId: '', // deviceId为空表示本设备
    bundleName: 'com.example.myapplication',
    abilityName: 'SpecifiedAbility',
    moduleName: 'specified', // moduleName非必选
    parameters: { // 自定义信息
        instanceKey: getInstance(),
    },
}
context.startAbility(want).then(() => {
    ...
}).catch((err: BusinessError) => {
    ...
})
在被拉起方UIAbility对应的AbilityStage的onAcceptWant生命周期回调中，解析传入的want参数，获取“instanceKey”自定义参数。根据业务需要返回一个该UIAbility实例的字符串Key标识。如果之前启动过此Key标识的UIAbility，则会将之前的UIAbility拉回前台并获焦，而不创建新的实例，否则创建新的实例并启动。
onAcceptWant(want: want): string {
    // 在被启动方的AbilityStage中，针对启动模式为specified的UIAbility返回一个UIAbility实例对应的一个Key值
    // 当前示例指的是device Module的EntryAbility
   if (want.abilityName === 'MainAbility') {
        return `DeviceModule_MainAbilityInstance_${want.parameters.instanceKey}`;
    }
    return '';
}
例如在文档应用中，可以对不同的文档实例内容绑定不同的Key值。当每次新建文档的时候，可以传入不同的新Key值（如可以将文件的路径作为一个Key标识），此时AbilityStage中启动UIAbility时都会创建一个新的UIAbility实例；当新建的文档保存之后，回到桌面，或者新打开一个已保存的文档，回到桌面，此时再次打开该已保存的文档，此时AbilityStage中再次启动该UIAbility时，打开的仍然是之前原来已保存的文档界面。


### UIAbilityContext

UIAbilityContext是UIAbility的上下文环境，继承自Context。 

UIAbility类拥有自身的上下文信息，该信息为UIAbilityContext类的实例，UIAbilityContext类拥有abilityInfo、currentHapModuleInfo等属性。    

通过UIAbilityContext可以获取UIAbility的相关配置信息，如包代码路径、Bundle名称、Ability名称和应用程序需要的环境状态等属性信息，以及可以获取操作UIAbility实例的方法（如startAbility()、connectServiceExtensionAbility()、terminateSelf()等）。

在UIAbility中可以通过this.context获取UIAbility实例的上下文信息。


### EventHub

EventHub为UIAbility提供了事件机制，使它们能够进行订阅、取消订阅和处罚事件等数据通信能力。 

在基类Context中，提供了EventHub对象，可用于UIAbility组件实例内通信。 

```TypeScript
class EntryAbility extends UIAbility {
    onCreate(want: Want, launchParam: AbilityConstant.LaunchParam) {
        let eventhub = this.context.eventhub;

        eventhub.on('event1', (data: string) => {
        ...
        });
    }
}
```
在UI中通过eventHub.emit()发送事件:   
```TypeScript
import common from '@ohos.app.ability.common';

@Entry
@Component
struct Index {
  private context = getContext(this) as common.UIAbilityContext;

  eventHubFunc() {
    // 不带参数触发自定义“event1”事件
    this.context.eventHub.emit('event1');
    // 带1个参数触发自定义“event1”事件
    this.context.eventHub.emit('event1', 1);
    // 带2个参数触发自定义“event1”事件
    this.context.eventHub.emit('event1', 2, 'test');
    }
}
```

可以根据需要调用eventHub.off()方法取消该事件的订阅。



----------


- [上一篇:6.常用组件](https://github.com/CharonChui/HarmonyOSNextStudyNote/blob/main/6.%E5%B8%B8%E7%94%A8%E7%BB%84%E4%BB%B6.md)
- [下一篇:8.通知](https://github.com/CharonChui/HarmonyOSNextStudyNote/blob/main/8.%E9%80%9A%E7%9F%A5.md)



    
---

- 邮箱 ：charon.chui@gmail.com  
- Good Luck! 


---

## 6.常用组件

Source: HarmonyOSNextStudyNote/6.常用组件.md

# 6.常用组件


在我们常用的应用中，经常会有视图内容切换的场景，来展示更加丰富的内容。比如经典的首页和我的页面，点击底部的页签的选项，可以实现“首页”和“我的”

Tabs组件仅可包含子组件TabContent，每一个页签对应一个内容视图即TabContent组件。下面的示例代码构建了一个简单的页签页面：

```TypeScript
@Entry
@Component
struct TabsExample {
  private controller: TabsController = new TabsController()

  build() {
    Column() {
      Tabs({ barPosition: BarPosition.Start, controller: this.controller }) {
        TabContent() {
          Column().width('100%').height('100%').backgroundColor(Color.Green)
        }
        .tabBar('green')

        TabContent() {
          Column().width('100%').height('100%').backgroundColor(Color.Blue)
        }
        .tabBar('blue')

        TabContent() {
          Column().width('100%').height('100%').backgroundColor(Color.Yellow)
        }
        .tabBar('yellow')

        TabContent() {
          Column().width('100%').height('100%').backgroundColor(Color.Pink)
        }
        .tabBar('pink')
      }
      .barWidth('100%') // 设置TabBar宽度
      .barHeight(60) // 设置TabBar高度
      .width('100%') // 设置Tabs组件宽度
      .height('100%') // 设置Tabs组件高度
      .backgroundColor(0xF5F5F5) // 设置Tabs组件背景颜色
    }
    .width('100%')
    .height('100%')
  }
}
```

- TabContent组件不支持设置通用宽度属性，其宽度默认撑满Tabs父组件。
- TabContent组件不支持设置通用高度属性，其高度由Tabs父组件高度与TabBar组件高度决定。



因为Tabs的布局模式默认是Fixed的，所以Tabs的页签是不可滑动的。当页签比较多的时候，可能会导致页签显示不全，将布局模式设置为Scrollable的话，可以实现页签的滚动。

Tabs的布局模式有Fixed（默认）和Scrollable两种：

- BarMode.Fixed：所有TabBar平均分配barWidth宽度（纵向时平均分配barHeight高度）,页签不可滚动。

- BarMode.Scrollable：每一个TabBar均使用实际布局宽度，超过总长度（横向Tabs的barWidth，纵向Tabs的barHeight）后可滑动。

Tabs组件页签默认显示在顶部，某些场景下您可能希望Tabs页签出现在底部或者侧边，您可以使用Tabs组件接口中的参数barPosition设置页签位置。此外页签显示位置还与vertical属性相关联，vertical属性用于设置页签的排列方向，当vertical的属性值为false（默认值）时页签横向排列，为true时页签纵向排列。

barPosition的值可以设置为BarPosition.Start（默认值）和BarPosition.End


### 自定义TabBar
![image](https://github.com/CharonChui/Pictures/blob/master/arkts_custom_tabbar.png?raw=true)

上面的这种底部页签效果，需要用到自定义TabBar。   


TabContent的tabBar属性除了支持string类型，还支持使用@Builder装饰器修饰的函数。您可以使用@Builder装饰器，构造一个生成自定义TabBar样式的函数，实现上面的底部页签效果，示例代码如下：

```TypeScript
@Entry
@Component
struct TabsExample {
  @State currentIndex: number = 0;
  private tabsController: TabsController = new TabsController();

  @Builder TabBuilder(title: string, targetIndex: number, selectedImg: Resource, normalImg: Resource) {
    Column() {
      Image(this.currentIndex === targetIndex ? selectedImg : normalImg)
        .size({ width: 25, height: 25 })
      Text(title)
        .fontColor(this.currentIndex === targetIndex ? '#1698CE' : '#6B6B6B')
    }
    .width('100%')
    .height(50)
    .justifyContent(FlexAlign.Center)
    .onClick(() => {
      this.currentIndex = targetIndex;
      this.tabsController.changeIndex(this.currentIndex);
    })
  }

  build() {
    Tabs({ barPosition: BarPosition.End, controller: this.tabsController }) {
      TabContent() {
        Column().width('100%').height('100%').backgroundColor('#00CB87')
      }
      .tabBar(this.TabBuilder('首页', 0, $r('app.media.home_selected'), $r('app.media.home_normal')))

      TabContent() {
        Column().width('100%').height('100%').backgroundColor('#007DFF')
      }
      .tabBar(this.TabBuilder('我的', 1, $r('app.media.mine_selected'), $r('app.media.mine_normal')))
    }
    .barWidth('100%')
    .barHeight(50)
    .onChange((index: number) => {
      this.currentIndex = index;
    })
  }
}
```



示例代码中将barPosition的值设置为BarPosition.End，使页签显示在底部。使用@Builder修饰TabBuilder函数，生成由Image和Text组成的页签。同时也给Tabs组件设置了TabsController控制器，当点击某个页签时，调用changeIndex方法进行页签内容切换。

最后还需要给Tabs添加onChange事件，Tab页签切换后触发该事件，这样当我们左右滑动内容视图的时候，页签样式也会跟着改变。


#### 播放器

```TypeScript
Video(value: {src?: string | Resource, currentProgressRate?: number | string |PlaybackSpeed, previewUri?: string |PixelMap | Resource, controller?: VideoController})
```

- src表示视频播放源的路径，可以支持本地视频路径和网络路径。使用网络地址时，如https，需要注意的是需要在module.json5文件中申请网络权限。
- currentProgressRate表示视频播放倍速，其参数类型为number，取值支持0.75，1.0，1.25，1.75，2.0，默认值为1.0倍速；
- previewUri表示视频未播放时的预览图片路径；
- controller表示视频控制器。


**视频支持的规格是：mp4、mkv、webm、TS。**

objectFit 中视频显示模式包括Contain、Cover、Auto、Fill、ScaleDown、None 6种模式，默认情况下使用ImageFit.Cover（保持宽高比进行缩小或者放大，使得图片两边都大于或等于显示边界），其他效果（如自适应显示、保持原有尺寸显示、不保持宽高比进行缩放等）可以根据具体使用场景/设备来进行选择。



### Video组件回调事件介绍

Video组件能够支持常规的点击、触摸等通用事件，同时也支持onStart、onPause、onFinish、onError等事件，具体事件的功能描述见下表：


- onStart(event:() => void) : 播放时触发该事件。

- onPause(event:() => void) : 暂停时触发该事件。

- onFinish(event:() => void) : 播放结束时触发该事件。

- onError(event:() => void) : 播放失败时触发该事件。

- onPrepared(callback:(event?: { duration: number }) => void)    
视频准备完成时触发该事件，通过duration可以获取视频时长，单位为s。

- onSeeking(callback:(event?: { time: number }) => void)      
操作进度条过程时上报时间信息，单位为s。

- onSeeked(callback:(event?: { time: number }) => void)     
操作进度条完成后，上报播放时间信息，单位为s。

- onUpdate(callback:(event?: { time: number }) => void)     
播放进度变化时触发该事件，单位为s，更新时间间隔为250ms。

- onFullscreenChange(callback:(event?: { fullscreen: boolean }) => void)      
在全屏播放与非全屏播放状态之间切换时触发该事件


## Dialog


弹窗是一种模态窗口，通常用来展示用户当前需要的或用户必须关注的信息或操作。在弹出框消失之前，用户无法操作其他界面内容。ArkUI为我们提供了丰富的弹窗功能，弹窗按照功能可以分为以下两类：

- 确认类：例如警告弹窗AlertDialog。
- 选择类：包括文本选择弹窗TextPickerDialog 、日期滑动选择弹窗DatePickerDialog、时间滑动选择弹窗TimePickerDialog等。

```TypeScript
Button('点击显示弹窗')
  .onClick(() => {
    AlertDialog.show(
      {
        title: '删除联系人', // 标题
        message: '是否需要删除所选联系人?', // 内容
        autoCancel: false, // 点击遮障层时，是否关闭弹窗。
        alignment: DialogAlignment.Bottom, // 弹窗在竖直方向的对齐方式
        offset: { dx: 0, dy: -20 }, // 弹窗相对alignment位置的偏移量
        primaryButton: {
          value: '取消',
          action: () => {
            console.info('Callback when the first button is clicked');
          }
        },
        secondaryButton: {
          value: '删除',
          fontColor: '#D94838',
          action: () => {
            console.info('Callback when the second button is clicked');
          }
        },
        cancel: () => { // 点击遮障层关闭dialog时的回调
          console.info('Closed callbacks');
        }
      }
    )
  })
```
此外，您还可以使用AlertDialog，构建只包含一个操作按钮的确认弹窗，使用confirm响应操作按钮回调。

```TypeScript
AlertDialog.show(
  {
    title: '提示',
    message: '提示信息',
    autoCancel: true,
    alignment: DialogAlignment.Bottom,
    offset: { dx: 0, dy: -20 },
    confirm: {
      value: '确认',
      action: () => {
        console.info('Callback when confirm button is clicked');
      }
    },
    cancel: () => {
      console.info('Closed callbacks')
    }
  }
)
```



### 自定义弹窗

自定义弹窗的使用更加灵活，适用于更多的业务场景，在自定义弹窗中您可以自定义弹窗内容，构建更加丰富的弹窗界面。自定义弹窗的界面可以通过装饰器@CustomDialog定义的组件来实现，然后结合CustomDialogController来控制自定义弹窗的显示和隐藏。


## Web组件

ArkUI为我们提供了Web组件来加载网页，借助它我们就相当于在自己的应用程序里嵌入一个浏览器，从而非常轻松地展示各种各样的网页。


Web组件的使用非常简单，只需要在Page目录下的ArkTS文件中创建一个Web组件，传入两个参数就可以了。其中

```TypeScript
@Entry
@Component
struct WebComponent {
  controller: WebController = new WebController();
  build() {
    Column() {
      Web({ src: 'https://developer.harmonyos.com/', controller: this.controller })
    }
  }
}
```


访问在线网页时您需要在module.json5文件中申明网络访问权限：ohos.permission.INTERNET。

#### 加载本地网页

前面实现了Web组件加载在线网页，Web组件同样也可以加载本地网页。首先在main/resources/rawfile目录下创建一个HTML文件，然后通过$rawfile引用本地网页资源，示例代码如下：

```TypeScript
@Entry
@Component
struct SecondPage {
  controller: WebController = new WebController();

  build() {
    Column() {
      Web({ src: $rawfile('index.html'), controller: this.controller })
    }
  }
}
```


有的网页可能不能很好适配手机屏幕，需要对其缩放才能有更好的效果，开发者可以根据需要给Web组件设置zoomAccess属性，zoomAccess用于设置是否支持手势进行缩放，默认允许执行缩放。Web组件默认支持手势进行缩放。      
您还可以使用zoom(factor: number)方法用于设置网站的缩放比例。其中factor表示缩放倍数。   


#### 文本缩放

如果需要对文本进行缩放，可以使用textZoomAtio(textZoomAtio: number)方法。其中textZoomAtio用于设置页面的文本缩放百分比，默认值为100，表示100%，使用textZoomAtio，文本会放大，但是图片不会随着文本一起放大。

#### Web组件事件 

Web组件还提供了处理Javascript的对话框、网页加载进度及各种通知与请求事件的方法。例如onProgressChange可以监听网页的加载进度，onPageEnd在网页加载完成时触发该回调，且只在主frame触发，onConfirm则在网页触发confirm告警弹窗时触发回调。下面以onConfirm事件为例讲解Web组件事件的使用，更多Web组件事件可以查看事件。


如果您希望响应Web组件中网页的警告弹窗事件，您可以在onAlert或者onConfirm的回调方法中处理这些事件。以confirm弹窗为例，在网页触发onConfirm()告警弹窗时，显示一个AlertDialog弹窗。

```TypeScript
@Entry
@Component
struct WebComponent {
  controller:WebController = new WebController();
  build() {
    Column() {
      Web({ src:$rawfile('index.html'), controller:this.controller })
        .onConfirm((event) => {
          AlertDialog.show({
            title: 'title',
            message: event.message,
            confirm: {
              value: 'onAlert',
              action: () => {
                event.result.handleConfirm();
              }
            },
            cancel: () => {
              event.result.handleCancel();
            }
          })
          return true;
        })
    }
  }
}
```

#### Web和JavaScript交互

在开发专为适配Web组件的网页时，您可以实现Web组件和JavaScript代码之间的交互。Web组件可以调用JavaScript方法，JavaScript也可以调用Web组件里面的方法。

如果您希望加载的网页在Web组件中运行JavaScript，则必须为您的Web组件启用JavaScript功能，默认情况下是允许JavaScript执行的。

```TypeScript
Web({ src:'https://www.example.com', controller:this.controller })
    .javaScriptAccess(true)
```


Web组件调用JS方法
您可以在Web组件onPageEnd事件中添加runJavaScript方法。事件是网页加载完成时的回调，runJavaScript方法可以执行HTML中的JavaScript脚本。

```
// xxx.ets
@Entry
@Component
struct WebComponent {
  controller: WebController = new WebController();
  @State webResult: string = ''
  build() {
    Column() {
      Text(this.webResult).fontSize(20)
      Web({ src: $rawfile('index.html'), controller: this.controller })
      .javaScriptAccess(true)
      .onPageEnd(e => {
        this.controller.runJavaScript({
          script: 'test()',
          callback: (result: string)=> {
            this.webResult = result;
          }});
      })
    }
  }
}
<!-- index.html -->
<!DOCTYPE html>
<html>
  <meta charset="utf-8">
  <body>
  </body>
  <script type="text/javascript">
  function test() {
      return "This value is from index.html"
  }
  </script>
</html>
```

当页面加载完成时，触发onPageEnd事件，调用HTML文件中的test方法并将结果返回给Web组件。

##### JS调用Web组件方法


您可以使用registerJavaScriptProxy将Web组件中的JavaScript对象注入daowindow对象中，这样网页中的JS就可以直接调用该对象了。需要注意的是，要想registerJavaScriptProxy方法生效，须调用refresh方法。下面的示例将ets文件中的对象testObj注入到了window对象中。

```
// xxx.ets
@Entry
@Component
struct WebComponent{
  @State dataFromHtml: string = ''
  controller: WebController = new WebController()
  testObj = {
    test: (data) => {
      this.dataFromHtml = data；
      return 'ArkUI Web Component';
    },
    toString: () => {
      console.log('Web Component toString');
    }
  }

  build() {
    Column() {
      Text(this.dataFromHtml).fontSize(20)
      Row() {
        Button('Register JavaScript To Window').onClick(() => {
          this.controller.registerJavaScriptProxy({
            object: this.testObj,
            name: 'objName',
            methodList: ['test', 'toString'],
          });
          this.controller.refresh();
        })
      }

      Web({ src: $rawfile('index.html'), controller: this.controller })
        .javaScriptAccess(true)
    }
  }
}
```

其中object表示参与注册的对象，name表示注册对象的名称为objName，与window中调用的对象名一致；methodList表示参与注册的应用侧JavaScript对象的方法，包含test、toString两个方法。在HTML中使用的时候直接使用objName调用methodList里面对应的方法即可，示例如下:
```
// index.html
<!DOCTYPE html>
<html>
<meta charset="utf-8">
<body>
<button onclick="htmlTest()">调用Web组件里面的方法</button>
</body>
<script type="text/javascript">
    function htmlTest() {
        str = objName.test("param from Html");
    }
</script>
</html>
```

您还可以使用deleteJavaScriptRegister删除通过registerJavaScriptProxy注册到window上的指定name的应用侧JavaScript对象。



#### 处理页面导航

当我们在使用浏览器浏览网页时，可以执行返回、前进、刷新等操作，Web组件同样支持这些操作。您可以使用backward()返回到上一个页面，使用forward()前进一个页面，您也可以使用refresh()刷新页面，使用clearHistory()来清除历史记录。


----------


- [上一篇:5.ArkTS声明式UI入门](https://github.com/CharonChui/HarmonyOSNextStudyNote/blob/main/5.ArkTS%E5%A3%B0%E6%98%8E%E5%BC%8FUI%E5%85%A5%E9%97%A8.md)
- [下一篇:7.应用组件UIAbility](https://github.com/CharonChui/HarmonyOSNextStudyNote/blob/main/7.%E5%BA%94%E7%94%A8%E7%BB%84%E4%BB%B6UIAbility.md)


    
---

- 邮箱 ：charon.chui@gmail.com  
- Good Luck! 


---

