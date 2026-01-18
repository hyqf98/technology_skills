# 开发工具 (Development Tools)

本文档介绍 HarmonyOS 开发过程中常用的工具和实用资源，帮助开发者提高开发效率。

## 目录

- [DevEco Studio](#deveco-studio)
- [命令行工具](#命令行工具)
- [调试工具](#调试工具)
- [性能分析工具](#性能分析工具)
- [第三方工具](#第三方工具)
- [开发资源](#开发资源)

## DevEco Studio

DevEco Studio 是华为官方提供的 HarmonyOS 应用开发 IDE，基于 IntelliJ IDEA 开发。

### 主要特性

- **智能代码编辑**：支持 ArkTS 语法高亮、代码补全、智能提示
- **可视化开发**：支持 UI 预览、实时预览、组件拖拽
- **多设备模拟器**：支持手机、平板、穿戴设备等多种设备模拟器
- **性能分析**：内置性能分析工具，帮助优化应用性能
- **版本管理**：集成 Git，方便代码版本控制

### 下载安装

**最新版本**：DevEco Studio 5.0.1 (HarmonyOS NEXT)

**下载地址**：https://developer.huawei.com/consumer/cn/deveco-studio/

**系统要求**：
- 操作系统：Windows 10/11 64 位、macOS 10.15+、Linux 64 位
- 内存：8GB 及以上（推荐 16GB）
- 硬盘：15GB 及以上可用空间
- JDK：DevEco Studio 内置 JDK 17

### 常用功能

#### 1. 创建项目

```
File → New → Project
选择模板：
- Empty Ability：空应用
- Native C++：带 C++ 代码的应用
- WebView：基于 WebView 的应用
```

#### 2. 运行应用

```
方式一：使用模拟器
1. Tools → Device Manager
2. 创建并启动模拟器
3. 点击运行按钮

方式二：使用真机
1. 开启开发者模式
2. 连接电脑
3. 选择设备并运行
```

#### 3. UI 预览

```
方式一：静态预览
- 打开 .ets 文件
- 点击编辑器右侧的预览按钮

方式二：动态预览
- 在预览窗口中进行交互
- 实时查看 UI 变化

方式三：多设备预览
- 同时在多个设备尺寸下预览
- 确保界面适配性
```

#### 4. 代码模板

```typescript
// 快速生成组件
输入：List
按 Tab：自动生成 List 组件模板

// 快速生成生命周期
输入：onCreate
按 Tab：自动生成 onCreate 方法模板
```

### 常用快捷键

| 功能 | Windows/Linux | macOS |
|------|---------------|-------|
| 代码补全 | Ctrl + Space | Ctrl + Space |
| 格式化代码 | Ctrl + Alt + L | Cmd + Option + L |
| 查找 | Ctrl + F | Cmd + F |
| 全局查找 | Ctrl + Shift + F | Cmd + Shift + F |
| 运行 | Shift + F10 | Ctrl + R |
| 调试 | Shift + F9 | Ctrl + D |
| 重命名 | Shift + F6 | Shift + F6 |
| 快捷修复 | Alt + Enter | Option + Enter |

## 命令行工具

### hdc (HarmonyOS Device Connector)

hdc 是 HarmonyOS 的命令行工具，用于设备连接和应用调试。

#### 常用命令

```bash
# 查看连接的设备
hdc list targets

# 安装应用
hdc install app.hap

# 卸载应用
hdc uninstall com.example.app

# 启动应用
hdc shell aa start -a EntryAbility -b com.example.app

# 查看日志
hdc shell hilog

# 文件传输
hdc file send local_path /data/local/tmp/
hdc file recv /data/local/tmp/file local_path

# 查看设备信息
hdc shell bm dump -n com.example.app

# 截屏
hdc shell snapshot_display -f /data/local/tmp/screenshot.png

# 清除日志
hdc shell hilog -r
```

### 证书管理

```bash
# 生成调试证书
keytool -genkeypair -alias "debug" -keyalg EC -validity 365 -keystore debug.keystore

# 生成发布证书
keytool -genkeypair -alias "release" -keyalg EC -validity 1095 -keystore release.keystore

# 查看证书信息
keytool -list -v -keystore debug.keystore
```

## 调试工具

### 1. 日志工具 (hilog)

```bash
# 查看实时日志
hdc shell hilog

# 按标签过滤
hdc shell hilog -T MyTag

# 按级别过滤
hdc shell hilog -L I/W/E/F

# 保存日志到文件
hdc shell hilog > log.txt

# 清除日志
hdc shell hilog -r
```

### 2. 网络抓包

使用 DevEco Studio 内置的网络抓包工具：

```
1. Run → Edit Configurations
2. 勾选 Enable Network Capture
3. 运行应用
4. Network Capture 窗口查看请求
```

### 3. 性能分析

```typescript
// 使用性能分析 API
import { hiTraceMeter } from '@kit.PerformanceAnalysisKit';

// 开始性能追踪
hiTraceMeter.startTrace('myOperation', 1);

// 执行操作
// ...

// 结束性能追踪
hiTraceMeter.finishTrace('myOperation', 1);
```

## 性能分析工具

### 1. Profiler

DevEco Studio 内置的性能分析工具：

```
View → Tool Windows → Profiler
```

**功能包括**：
- CPU 使用率
- 内存使用情况
- 网络请求
- 能耗分析
- 渲染性能

### 2. 内存分析

```typescript
// 检查内存泄漏
import { heapCapture } from '@kit.PerformanceAnalysisKit';

// 捕获内存快照
let snapshot = heapCapture.capture();
console.info('内存使用:', snapshot);
```

### 3. 帧率分析

```typescript
// 监控帧率
import { hiAppEvent } from '@kit.HarmonyOsAppEvent';

hiAppEvent.write('frame_event', {
  frameRate: 60,
  dropFrames: 0
});
```

## 第三方工具

### 1. Postman

用于 API 测试和调试：

```json
{
  "method": "POST",
  "header": {
    "Content-Type": "application/json"
  },
  "body": {
    "name": "张三",
    "age": 25
  }
}
```

### 2. Charles

用于网络抓包和调试：

```
1. 安装 Charles
2. 配置代理
3. 安装 HTTPS 证书
4. 开始抓包
```

### 3. VS Code

轻量级代码编辑器，支持 ArkTS 插件：

```
1. 安装 VS Code
2. 安装 HarmonyOS 插件
3. 打开项目
4. 开始编码
```

## 开发资源

### 官方资源

- **开发者官网**：https://developer.huawei.com/consumer/cn/
- **官方文档**：https://developer.huawei.com/consumer/cn/doc/
- **API 参考**：https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/
- **示例代码**：https://developer.huawei.com/consumer/cn/codelabs/
- **视频教程**：https://developer.huawei.com/consumer/cn/training/
- **论坛社区**：https://developer.huawei.com/consumer/cn/forum/

### 学习资源

#### 在线教程

1. **HarmonyOS 第一课**
   - 零基础入门教程
   - 涵盖应用开发全流程

2. **官方示例**
   - 包含多种场景的示例代码
   - 可直接运行和学习

3. **视频教程**
   - 华为开发者联盟官方频道
   - 系统的学习路径

#### 书籍推荐

1. 《鸿蒙HarmonyOS应用开发从入门到精通》
2. 《鸿蒙HarmonyOS手机应用开发实战》
3. 《鸿蒙之光HarmonyOS NEXT原生应用开发入门》
4. 《HarmonyOS 6应用开发入门》

#### 社区资源

1. **Gitee**：https://gitee.com/harmonyos
2. **GitHub**：https://github.com/harmonyos
3. **Stack Overflow**：https://stackoverflow.com/questions/tagged/harmonyos

### 工具推荐

#### 设计工具

1. **Figma**：UI/UX 设计
2. **Sketch**：界面设计
3. **Adobe XD**：原型设计

#### 开发工具

1. **DevEco Studio**：官方 IDE
2. **VS Code**：轻量级编辑器
3. **Android Studio**：跨平台开发

#### 测试工具

1. **Appium**：自动化测试
2. **Selenium**：Web 测试
3. **JMeter**：性能测试

## 最佳实践

### 1. 项目结构

```
MyApplication/
├── entry/
│   ├── src/
│   │   └── main/
│   │       ├── ets/           # ArkTS 代码
│   │       │   ├── entryability/
│   │       │   ├── pages/
│   │       │   ├── components/
│   │       │   └── utils/
│   │       ├── resources/     # 资源文件
│   │       │   ├── base/
│   │       │   ├── element/
│   │       │   └── rawfile/
│   │       └── module.json5   # 模块配置
│   └── build-profile.json5    # 构建配置
├── oh-package.json5            # 依赖配置
└── build-profile.json5         # 项目配置
```

### 2. 代码规范

```typescript
// 命名规范
class UserService {              // 类名：大驼峰
  private userName: string = '';  // 变量名：小驼峰
  private MAX_COUNT: number = 100; // 常量：全大写

  async getUserData(): Promise<Object> {  // 方法名：小驼峰
    // 实现
  }
}

// 组件命名
@Component
struct UserList {                  // 组件名：大驼峰
  @State userList: string[] = [];   // 状态变量：小驼峰

  build() {
    // 实现
  }
}

// 文件命名
UserService.ets                    // 类名与文件名一致
user_list_data.ets                  // 多个单词用下划线分隔
```

### 3. 注释规范

```typescript
/**
 * 用户服务类
 * 提供用户相关的业务逻辑处理
 */
class UserService {
  /**
   * 获取用户信息
   * @param userId 用户ID
   * @returns 用户信息对象
   */
  async getUserInfo(userId: string): Promise<UserInfo> {
    // 实现
  }
}
```

### 4. 版本控制

```bash
# .gitignore 配置
/node_modules
.idea
build
.hvigor
local.properties

# Git 工作流
git checkout -b feature/new-feature
git add .
git commit -m "feat: 添加新功能"
git push origin feature/new-feature
```

## 常见问题

### Q1: DevEco Studio 无法启动模拟器？

**答**：
1. 检查系统虚拟化是否开启
2. 检查 BIOS 中的 VT-x/AMD-V 是否启用
3. 重新安装模拟器
4. 检查防火墙设置

### Q2: hdc 命令无法识别？

**答**：
```bash
# 添加 hdc 到环境变量
# Windows：
setx PATH "%PATH%;C:\Users\YourName\Huawei\DevEco Studio\tools"

# macOS/Linux：
export PATH=$PATH:/path/to/deveco-studio/tools
```

### Q3: 如何查看应用崩溃日志？

**答**：
```bash
# 查看所有日志
hdc shell hilog

# 过滤错误日志
hdc shell hilog -L E

# 保存日志到文件
hdc shell hilog > crash_log.txt
```

### Q4: 如何提高编译速度？

**答**：
1. 增加内存分配
2. 使用增量编译
3. 关闭不必要的插件
4. 使用 SSD 硬盘
5. 优化项目依赖

## 总结

掌握 HarmonyOS 开发工具的使用，可以显著提高开发效率：

1. **DevEco Studio**：官方 IDE，功能全面
2. **命令行工具**：快速高效
3. **调试工具**：问题定位
4. **性能分析**：性能优化
5. **第三方工具**：扩展能力

选择合适的工具，遵循最佳实践，让 HarmonyOS 开发更加高效。

## 参考资源

- [DevEco Studio 用户指南](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/ide-installation-V5)
- [hdc 工具使用指南](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/ide-command-line-tools-V5)
- [性能分析指南](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/ide-performance-profiling-V5)
