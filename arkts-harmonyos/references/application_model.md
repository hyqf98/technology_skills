# 应用模型 (Application Model)

应用模型是 HarmonyOS 应用开发的核心概念，定义了应用的结构、生命周期和组件间的关系。本文档详细介绍 HarmonyOS NEXT 的应用模型相关技术。

## 目录

- [UIAbility](#uiability)
- [ExtensionAbility](#extensionability)
- [窗口管理](#窗口管理)
- [应用生命周期](#应用生命周期)
- [示例代码](#示例代码)

## UIAbility

UIAbility 是一种包含 UI 界面的应用组件，主要用于和用户进行交互。它是 HarmonyOS 系统调度的基本单元，为应用提供绘制界面的窗口。

### UIAbility 生命周期

UIAbility 的生命周期包括以下四个状态：

- **Create（创建）**：UIAbility 实例创建时触发，系统会调用 `onCreate()` 方法
- **Foreground（前台）**：UIAbility 进入前台状态，可以与用户交互
- **Background（后台）**：UIAbility 进入后台状态，不可见但仍运行
- **Destroy（销毁）**：UIAbility 实例销毁时触发，系统会调用 `onDestroy()` 方法

### 生命周期回调方法

```typescript
import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  // Ability 创建时回调
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');

    // 获取启动参数
    hilog.info(0x0000, 'testTag', 'launchReason: %{public}d', launchParam.launchReason);
    hilog.info(0x0000, 'testTag', 'lastExitReason: %{public}d', launchParam.lastExitReason);
  }

  // Ability 销毁时回调
  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
    // 释放资源
  }

  // 窗 stage 创建时回调
  onWindowStageCreate(windowStage: window.WindowStage): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    // 设置 UI 页面
    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s',
          JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  // 窗口销毁时回调
  onWindowStageDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
    // 释放 UI 相关资源
  }

  // Ability 转换到前台时回调
  onForeground(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
    // 恢复音频、动画等资源
  }

  // Ability 转换到后台时回调
  onBackground(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackGround');
    // 释放音频、动画等资源
  }
}
```

### 启动模式

UIAbility 支持多种启动模式，通过 `module.json5` 配置：

```json
{
  "abilities": [
    {
      "name": "EntryAbility",
      "launchType": "singleton"  // singleton, standard, specified
    }
  ]
}
```

- **singleton**：单实例模式，默认值
- **standard**：多实例模式
- **specified**：指定实例模式

## ExtensionAbility

ExtensionAbility 是一种没有 UI 界面的能力组件，用于提供特定的扩展能力。

### 常见 ExtensionAbility 类型

1. **FormAbility**：卡片扩展能力
2. **WorkScheduler**：后台任务调度
3. **InputMethod**：输入法扩展
4. **Accessibility**：无障碍服务
5. **DataShare**：数据共享扩展

### BackupExtensionAbility 示例

BackupExtensionAbility 用于应用数据备份和恢复：

```typescript
import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  // 备份时回调
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
    // 实现数据备份逻辑
    await Promise.resolve();
  }

  // 恢复时回调
  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s',
      JSON.stringify(bundleVersion));
    // 实现数据恢复逻辑
    await Promise.resolve();
  }
}
```

## 窗口管理

HarmonyOS NEXT 提供了强大的窗口管理能力，支持多窗口、悬浮窗等场景。

### WindowStage 介绍

WindowStage 是窗口管理的核心类，用于管理窗口的生命周期：

```typescript
import { window } from '@kit.ArkUI';

// 获取主窗口
let windowStage = await window.getLastWindow(context);

// 设置窗口属性
await windowStage.setWindowLayoutFullScreen(true);  // 全屏布局
await windowStage.setWindowTouchable(true);         // 可触摸

// 获取窗口对象
let mainWindow = windowStage.getMainWindow();
await mainWindow.setFullScreen(true);               // 设置全屏
```

### 窗口属性设置

```typescript
// 设置窗口背景色
mainWindow.setWindowBackgroundColor('#000000');

// 设置窗口亮度
mainWindow.setBrightness(1.0);

// 设置窗口透明度
mainWindow.setOpacity(0.9);

// 设置窗口方向
await mainwindow.setPreferredOrientation(window.Orientation.LANDSCAPE);
```

## 应用生命周期

### Application 状态管理

```typescript
export default class MyApplication extends AbilityPackage {
  onCreate() {
    console.log('Application onCreate');
    // 应用初始化
  }

  onDestroy() {
    console.log('Application onDestroy');
    // 应用清理
  }
}
```

## Want 传递

Want 是对象间信息传递的载体，用于启动 Ability 和传递数据。

```typescript
// 创建 Want 对象
let want: Want = {
  bundleName: 'com.example.app',
  abilityName: 'EntryAbility',
  parameters: {
    message: 'Hello from launcher'
  }
};

// 启动 Ability
context.startAbility(want, (err) => {
  if (err.code) {
    console.error('Failed to start ability');
    return;
  }
  console.info('Start ability succeeded');
});
```

## 示例代码

### 完整的 UIAbility 示例

```typescript
import { AbilityConstant, ConfigurationConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

const DOMAIN = 0x0000;

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    // 设置颜色模式
    try {
      this.context.getApplicationContext().setColorMode(
        ConfigurationConstant.ColorMode.COLOR_MODE_NOT_SET
      );
    } catch (err) {
      hilog.error(DOMAIN, 'testTag', 'Failed to set colorMode. Cause: %{public}s',
        JSON.stringify(err));
    }

    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onCreate');
    hilog.info(DOMAIN, 'testTag', 'Want params: %{public}s',
      JSON.stringify(want.parameters));
  }

  onDestroy(): void {
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    // 加载页面
    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(DOMAIN, 'testTag', 'Failed to load the content. Cause: %{public}s',
          JSON.stringify(err));
        return;
      }
      hilog.info(DOMAIN, 'testTag', 'Succeeded in loading the content.');
    });

    // 配置窗口属性
    this.configureWindow(windowStage);
  }

  onWindowStageDestroy(): void {
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onBackground');
  }

  // 配置窗口属性
  private configureWindow(windowStage: window.WindowStage): void {
    windowStage.getMainWindow((err, mainWindow) => {
      if (err.code) {
        hilog.error(DOMAIN, 'testTag', 'Failed to obtain the main window. Cause: %{public}s',
          JSON.stringify(err));
        return;
      }

      // 设置窗口全屏
      mainWindow.setFullScreen(true).then(() => {
        hilog.info(DOMAIN, 'testTag', 'Succeeded in setting the window to full screen.');
      }).catch((err: Error) => {
        hilog.error(DOMAIN, 'testTag', 'Failed to set the window to full screen. Cause: %{public}s',
          JSON.stringify(err));
      });
    });
  }

  // 处理新意图
  onNewWant(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(DOMAIN, 'testTag', 'onNewWant, want: %{public}s',
      JSON.stringify(want.parameters));
  }

  // 内存警告
  onMemoryLevel(level: AbilityConstant.MemoryLevel): void {
    hilog.info(DOMAIN, 'testTag', 'onMemoryLevel, level: %{public}d', level);
    // 释放不必要的资源
  }
}
```

### BackupAbility 示例

```typescript
import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

const DOMAIN = 0x0000;

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(DOMAIN, 'testTag', 'onBackup started');

    try {
      // 获取应用数据
      const appData = this.getApplicationData();

      // 备份数据到指定位置
      await this.backupData(appData);

      hilog.info(DOMAIN, 'testTag', 'onBackup succeeded');
    } catch (error) {
      hilog.error(DOMAIN, 'testTag', 'onBackup failed: %{public}s',
        JSON.stringify(error));
    }
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(DOMAIN, 'testTag', 'onRestore started, version: %{public}s',
      JSON.stringify(bundleVersion));

    try {
      // 从备份恢复数据
      const backupData = await this.restoreData();

      // 应用恢复的数据
      await this.applyData(backupData);

      hilog.info(DOMAIN, 'testTag', 'onRestore succeeded');
    } catch (error) {
      hilog.error(DOMAIN, 'testTag', 'onRestore failed: %{public}s',
        JSON.stringify(error));
    }
  }

  private getApplicationData(): Object {
    // 实现获取应用数据的逻辑
    return {};
  }

  private async backupData(data: Object): Promise<void> {
    // 实现备份数据的逻辑
    await Promise.resolve();
  }

  private async restoreData(): Promise<Object> {
    // 实现恢复数据的逻辑
    return {};
  }

  private async applyData(data: Object): Promise<void> {
    // 实现应用数据的逻辑
    await Promise.resolve();
  }
}
```

## HarmonyOS NEXT 新特性

### 1. 增强的生命周期管理

HarmonyOS NEXT 提供了更细粒度的生命周期控制：

- **onConfigurationUpdate**：配置更新时回调
- **onContinue**：跨设备迁移时回调
- **onSaveState**：保存状态时回调

```typescript
onConfigurationUpdate(newConfig: Configuration): void {
  hilog.info(0x0000, 'testTag', 'Configuration updated: %{public}s',
    JSON.stringify(newConfig));
  // 处理配置变更，如语言、方向等
}

onContinue(wantParam: { [key: string]: Object }): AbilityConstant.OnContinueResult {
  // 实现跨设备迁移逻辑
  return AbilityConstant.OnContinueResult.AGREE;
}

onSaveState(wantParam: { [key: string]: Object }): void {
  // 保存应用状态
  wantParam['myData'] = this.savedData;
}
```

### 2. 改进的多窗口支持

```typescript
// 创建子窗口
async createSubWindow() {
  let windowStage = await window.getLastWindow(this.context);
  await windowStage.createSubWindow('mySubWindow', (err, mainWindow) => {
    if (err.code) {
      console.error('Failed to create sub window');
      return;
    }
    // 配置子窗口
    mainWindow.moveWindowTo(100, 100);
    mainWindow.resize(800, 600);
    mainWindow.showWindow();
  });
}
```

### 3. 场景化能力

HarmonyOS NEXT 引入了场景化能力，可以根据使用场景优化应用行为：

```typescript
// 设置应用场景
this.context.getApplicationContext().setSceneMode(
  AbilityConstant.SceneMode.DEFAULT
);

// 可选的场景模式：
// - DEFAULT: 默认模式
// - HOME_CONTROL: 家庭控制模式
// - HEALTH: 健康模式
// - EDUCATION: 教育模式
```

## 最佳实践

### 1. 资源管理

- 在 `onCreate()` 中初始化资源
- 在 `onDestroy()` 中释放资源
- 在 `onBackground()` 中暂停耗时操作
- 在 `onForeground()` 中恢复耗时操作

### 2. 状态保存

```typescript
onSaveState(wantParam: { [key: string]: Object }): void {
  // 保存重要状态
  wantParam['userInput'] = this.userInput;
  wantParam['scrollPosition'] = this.scrollPosition;
}

onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
  // 恢复状态
  if (launchParam.lastExitReason === AbilityConstant.LastExitReason.ABILITY_DIED) {
    this.restoreState(want.parameters);
  }
}
```

### 3. 错误处理

```typescript
onWindowStageCreate(windowStage: window.WindowStage): void {
  windowStage.loadContent('pages/Index', (err) => {
    if (err.code) {
      // 记录错误日志
      hilog.error(0x0000, 'testTag', 'Load content failed: %{public}s',
        JSON.stringify(err));

      // 显示错误提示
      this.showErrorDialog();

      // 跳转到错误页面
      windowStage.loadContent('pages/Error');
      return;
    }
    hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
  });
}
```

## 常见问题

### Q1: 如何判断 Ability 的启动原因？

```typescript
onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
  switch (launchParam.launchReason) {
    case AbilityConstant.LaunchReason.START_ABILITY:
      hilog.info(0x0000, 'testTag', 'Launched by startAbility');
      break;
    case AbilityConstant.LaunchReason.CONTINUATION:
      hilog.info(0x0000, 'testTag', 'Launched by continuation');
      break;
    case AbilityConstant.LaunchReason.LAUNCHER:
      hilog.info(0x0000, 'testTag', 'Launched by launcher');
      break;
  }
}
```

### Q2: 如何处理配置变更？

```typescript
onConfigurationUpdate(newConfig: Configuration): void {
  if (newConfig.language !== this.currentLanguage) {
    // 语言变更
    this.reloadResources();
  }

  if (newConfig.colorMode !== this.currentColorMode) {
    // 颜色模式变更
    this.updateTheme();
  }
}
```

### Q3: 如何优化启动性能？

1. 延迟初始化非关键资源
2. 使用懒加载策略
3. 避免在 `onCreate()` 中执行耗时操作
4. 使用异步初始化

```typescript
async onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
  // 立即执行的关键初始化
  this.initEssential();

  // 异步执行非关键初始化
  setTimeout(() => {
    this.initOptional();
  }, 100);
}
```

## 总结

应用模型是 HarmonyOS 应用开发的基础，掌握 UIAbility 和 ExtensionAbility 的生命周期、窗口管理和 Want 传递等核心概念，对于开发高质量的 HarmonyOS 应用至关重要。

## 参考资源

- [HarmonyOS 应用模型官方文档](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/application-models-V5)
- [UIAbility 开发指南](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/uiability-development-V5)
- [ExtensionAbility 开发指南](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/extensionability-development-V5)
