# JetBrains 插件开发快速入门

**页数:** 15

---

## 简介 | IntelliJ Platform Plugin SDK

**URL:** https://plugins.jetbrains.com/docs/intellij/welcome.html

### 什么是 IntelliJ Platform？

IntelliJ Platform 是一个开源平台，为 JetBrains IDE（IntelliJ IDEA、PyCharm、WebStorm 等）提供核心功能。通过插件开发，您可以：

- **扩展 IDE 功能**：添加新的工具、菜单、编辑器功能
- **支持新语言**：为编程语言提供语法高亮、代码补全、重构等功能
- **集成外部系统**：连接 CI/CD、 issue 跟踪器、API 工具
- **定制工作流**：自动化重复任务，提高开发效率

### 核心概念

#### 1. 插件（Plugin）

插件是一个独立的模块，可以扩展 IDE 的功能。每个插件包含：

- **plugin.xml**：插件描述文件
- **代码**：实现功能的 Java/Kotlin 类
- **资源**：图标、UI 文件、配置文件

#### 2. IntelliJ Platform SDK

提供开发插件所需的 API 和工具：

- **OpenAPI**：核心 API 接口
- **UI 组件**：Swing 组件库
- **PSI（Program Structure Interface）**：代码结构分析
- **索引系统**：快速代码搜索

---

## 环境准备 | 开发环境配置

### 系统要求

- **JDK**: 17 或更高版本（推荐使用 JetBrains Runtime 17）
- **IDE**: IntelliJ IDEA Ultimate（推荐）或 Community
- **构建工具**: Gradle 8.0+
- **操作系统**: Windows、macOS、Linux

### 安装 IntelliJ IDEA

**Ultimate Edition vs Community Edition:**

| 功能 | Community | Ultimate |
|------|-----------|----------|
| 基础插件开发 | ✅ | ✅ |
| 多平台支持 | 有限 | 完整 |
| 调试插件 | ✅ | ✅ |
| 插件验证工具 | ❌ | ✅ |

**下载地址：** https://www.jetbrains.com/idea/download/

### 配置 JDK

**推荐使用 JetBrains Runtime 17:**

```bash
# 下载 JetBrains Runtime 17
# https://confluence.jetbrains.com/display/JBR/JetBrains+Runtime

# 配置 IDEA 使用的 JDK
# File → Project Structure → SDKs
# 添加 JetBrains Runtime 17 路径
```

---

## 创建插件项目 | 第一个插件

### 方法 1：使用 IntelliJ Platform Plugin Template（推荐）

这是创建新插件项目的最简单方法：

**步骤：**

1. **使用模板生成项目**

   访问 [intellij-platform-plugin-template](https://github.com/JetBrains/intellij-platform-plugin-template)

2. **点击 "Use this template"**

3. **填写项目信息**

   ```yaml
   # 示例配置
   project_name: My First Plugin
   plugin_id: com.example.myfirstplugin
   plugin_name: My First Plugin
   plugin_description: My first IntelliJ plugin
   ```

4. **克隆并打开项目**

   ```bash
   git clone https://github.com/your-username/my-first-plugin.git
   cd my-first-plugin
   ```

5. **在 IntelliJ IDEA 中打开项目**

### 方法 2：使用 New Project 向导

**步骤：**

1. **创建新项目**
   - 打开 IntelliJ IDEA
   - 选择 `File` → `New` → `Project`

2. **选择项目类型**
   - 左侧选择 `IntelliJ Platform Plugin`
   - 选择 `New Project` 向导

3. **配置项目**
   ```yaml
   Name: MyFirstPlugin
   Location: /path/to/MyFirstPlugin
   Language: Java / Kotlin
   Build system: Gradle
   JDK: 17
   ```

4. **点击 Create**

---

## 项目结构 | 插件项目组织

### 典型项目结构

```
MyFirstPlugin/
├── build.gradle.kts           # Gradle 构建配置
├── gradle.properties          # Gradle 属性配置
├── settings.gradle.kts        # Gradle 设置
├── src/
│   └── main/
│       ├── java/              # Java 源代码
│       │   └── com/
│       │       └── example/
│       │           └── myfirstplugin/
│       │               ├── actions/       # 动作类
│       │               ├── services/      # 服务类
│       │               └── MyFirstPlugin.java  # 插件主类
│       ├── resources/
│       │   └── META-INF/
│       │       └── plugin.xml      # 插件描述文件（关键）
│       └── ui/                # UI 表单文件
├── CHANGELOG.md               # 更新日志
├── LICENSE                    # 许可证
└── README.md                  # 项目说明
```

### plugin.xml 详解

**插件的核心配置文件：**

```xml
<idea-plugin>
    <!-- 插件唯一标识 -->
    <id>com.example.myfirstplugin</id>

    <!-- 插件名称 -->
    <name>My First Plugin</name>

    <!-- 插件版本 -->
    <version>1.0.0</version>

    <!-- 供应商信息 -->
    <vendor email="support@example.com" url="https://example.com">
        Example Inc.
    </vendor>

    <!-- 插件描述 -->
    <description><![CDATA[
        <p>My first IntelliJ IDEA plugin.</p>
        <p>Features:</p>
        <ul>
            <li>Feature 1</li>
            <li>Feature 2</li>
        </ul>
    ]]></description>

    <!-- 变更日志（可选） -->
    <change-notes><![CDATA[
        <h3>1.0.0</h3>
        <ul>
            <li>Initial release</li>
        </ul>
    ]]></change-notes>

    <!-- IDE 版本兼容性 -->
    <idea-version since-build="233" until-build="241.*"/>

    <!-- 依赖项 -->
    <depends>com.intellij.modules.platform</depends>
    <depends>com.intellij.modules.java</depends>

    <!-- 扩展点 -->
    <extensions defaultExtensionNs="com.intellij">
        <!-- 添加扩展 -->
    </extensions>

    <!-- 动作注册 -->
    <actions>
        <!-- 添加动作 -->
    </actions>
</idea-plugin>
```

---

## 构建配置 | Gradle 配置

### build.gradle.kts（Kotlin DSL）

**完整配置示例：**

```kotlin
plugins {
    id("java")
    id("org.jetbrains.kotlin.jvm") version "1.9.0"
    id("org.jetbrains.intellij") version "1.17.0"
}

group = "com.example"
version = "1.0.0"

repositories {
    mavenCentral()
}

dependencies {
    // Kotlin 标准库
    implementation("org.jetbrains.kotlin:kotlin-stdlib")

    // 测试依赖
    testImplementation("junit:junit:4.13.2")
    testImplementation("org.jetbrains.kotlin:kotlin-test")
}

// IntelliJ Platform Gradle Plugin 配置
intellij {
    pluginName.set("My First Plugin")
    version.set("2023.3")
    type.set("IC") // IC = Community, IU = Ultimate
    plugins.set(listOf(/* 插件依赖 */))
}

tasks {
    // 设置 JVM 兼容性
    withType<JavaCompile> {
        sourceCompatibility = "17"
        targetCompatibility = "17"
    }
    withType<org.jetbrains.kotlin.gradle.tasks.KotlinCompile> {
        kotlinOptions.jvmTarget = "17"
    }

    patchPluginXml {
        sinceBuild.set("233")
        untilBuild.set("241.*")
    }

    signPlugin {
        certificateChain.set(System.getenv("CERTIFICATE_CHAIN"))
        privateKey.set(System.getenv("PRIVATE_KEY"))
        password.set(System.getenv("PRIVATE_KEY_PASSWORD"))
    }

    publishPlugin {
        token.set(System.getenv("PUBLISH_TOKEN"))
    }
}
```

### build.gradle（Groovy DSL）

```groovy
plugins {
    id 'java'
    id 'org.jetbrains.kotlin.jvm' version '1.9.0'
    id 'org.jetbrains.intellij' version '1.17.0'
}

group = 'com.example'
version = '1.0.0'

repositories {
    mavenCentral()
}

dependencies {
    implementation "org.jetbrains.kotlin:kotlin-stdlib"
    testImplementation "junit:junit:4.13.2"
}

intellij {
    pluginName = 'My First Plugin'
    version = '2023.3'
    type = 'IC'
    plugins = []
}

patchPluginXml {
    sinceBuild = '233'
    untilBuild = '241.*'
}
```

---

## 快速开始 | 创建第一个动作（Action）

### 什么是 Action？

Action 是用户可以触发的操作，如：
- 菜单项
- 工具栏按钮
- 键盘快捷键
- 上下文菜单项

### 创建简单 Action

**步骤：**

#### 1. 创建 Action 类

**位置：** `src/main/java/com/example/myfirstplugin/actions/HelloWorldAction.java`

```java
package com.example.myfirstplugin.actions;

import com.intellij.openapi.actionSystem.AnAction;
import com.intellij.openapi.actionSystem.AnActionEvent;
import com.intellij.openapi.project.Project;
import com.intellij.openapi.ui.Messages;
import org.jetbrains.annotations.NotNull;

/**
 * Hello World Action
 * 点击后显示一个对话框
 */
public class HelloWorldAction extends AnAction {

    @Override
    public void actionPerformed(@NotNull AnActionEvent event) {
        // 获取当前项目
        Project project = event.getProject();

        // 显示消息对话框
        Messages.showMessageDialog(
            project,
            "Hello, World! 这是我的第一个插件！",
            "问候",
            Messages.getInformationIcon()
        );
    }
}
```

#### 2. 注册 Action

在 `plugin.xml` 中注册：

```xml
<actions>
    <action
        id="MyFirstPlugin.HelloWorld"
        class="com.example.myfirstplugin.actions.HelloWorldAction"
        text="Hello World"
        description="显示 Hello World 消息">
        <add-to-group group-id="ToolsMenu" anchor="first"/>
        <keyboard-shortcut keymap="$default" first-keystroke="ctrl shift H"/>
    </action>
</actions>
```

**属性说明：**

| 属性 | 说明 |
|------|------|
| `id` | Action 的唯一标识 |
| `class` | Action 类的完整路径 |
| `text` | 显示在菜单中的文本 |
| `description` | 鼠标悬停时显示的提示 |
| `add-to-group` | 添加到哪个菜单 |
| `keyboard-shortcut` | 键盘快捷键 |

#### 3. 运行插件

**方法：使用 Gradle 任务**

```bash
# 运行测试 IDE
./gradlew runIde
```

**方法：使用 IDEA 运行配置**

1. 点击 `Run` → `Edit Configurations`
2. 点击 `+` → 选择 `Plugin`
3. 配置：
   - `Name`: Run with IDE`
4. 在 Tools 菜单中点击 "Hello World"
5. 或按 `Ctrl + Shift + H`

#### 5. 验证功能

你应该看到一个显示 "Hello, World! 这是我的第一个插件！" 的对话框。

---

## 调试插件 | Debug

### 设置断点

在 Action 类中设置断点：

```java
@Override
public void actionPerformed(@NotNull AnActionEvent event) {
    Project project = event.getProject();  // 在这里设置断点

    Messages.showMessageDialog(
        project,
        "Hello, World!",
        "问候",
        Messages.getInformationIcon()
    );
}
```

### 启动调试模式

**方法 1：使用 IDEA**

1. 点击 `Run` → `Debug`
2. 或点击工具栏的 🐛 图标

**方法 2：使用 Gradle**

```bash
./gradlew runIde --debug-jvm
```

### 调试技巧

1. **查看日志**
   - `Help` → `Show Log in Explorer`
   - 或查看 `build/idea-sandbox/system/log/`

2. **使用断点**
   - 查看变量值
   - 单步执行
   - 表达式求值

3. **常见调试问题**
   - 插件未加载：检查 `plugin.xml` 配置
   - Action 不显示：检查注册位置
   - 类找不到：检查依赖配置

---

## 构建和分发 | 打包插件

### 构建插件

**使用 Gradle 构建插件 JAR：**

```bash
# 构建插件
./gradlew buildPlugin

# 验证插件
./gradlew verifyPlugin

# 查看构建产物
ls build/distributions/
# 输出: MyFirstPlugin-1.0.0.zip
```

### 本地安装测试

**步骤：**

1. **运行测试 IDE**

   ```bash
   ./gradlew runIde
   ```

2. **在测试 IDE 中安装插件**
   - `Settings` → `Plugins`
   - 点击 ⚙️ → `Install Plugin from Disk`
   - 选择 `build/distributions/MyFirstPlugin-1.0.0.zip`
   - 重启测试 IDE

---

## 发布插件 | 分发到 JetBrains Marketplace

### 准备发布

**1. 创建 JetBrains 账户**

访问 [JetBrains Account](https://account.jetbrains.com/)

**2. 获取发布令牌**

```bash
# 在 JetBrains Account 中生成令牌
# Settings → Token → New Token
# 选择：Plugin Publishing Token
```

**3. 配置签名（可选但推荐）**

```bash
# 设置环境变量
export CERTIFICATE_CHAIN="path/to/certificate-chain"
export PRIVATE_KEY="path/to/private-key"
export PRIVATE_KEY_PASSWORD="your-password"
```

### 发布到 Marketplace

**方法 1：使用 Gradle 任务**

```bash
# 发布插件
./gradlew publishPlugin

# 首次发布时需要：
# 1. 登录 JetBrains Marketplace
# 2. 创建插件仓库
# 3. 获取仓库 URL
```

**方法 2：手动上传**

1. 访问 [JetBrains Marketplace](https://plugins.jetbrains.com/author/upload)
2. 上传 `build/distributions/MyFirstPlugin-1.0.0.zip`
3. 填写插件信息
4. 等待审核（通常 1-3 天）

### 版本管理

**语义化版本：**

```
MAJOR.MINOR.PATCH
  |     |     |
  |     |     +--- 修复版本（Bug 修复）
  |     +--------- 次版本（新功能，向后兼容）
  +--------------- 主版本（破坏性变更）
```

**示例：**

```kotlin
// build.gradle.kts
version = "1.2.3"  // MAJOR.MINOR.PATCH

// 更新插件版本
version = "1.2.4"  // 修复版本
version = "1.3.0"  // 新功能
version = "2.0.0"  // 破坏性变更
```

---

## 最佳实践 | 开发建议

### 代码组织

```
src/main/java/com/example/myplugin/
├── actions/           # 用户动作
├── components/        # UI 组件
├── services/          # 后台服务
├── utils/             # 工具类
├── model/             # 数据模型
└── MyPlugin.java      # 插件主类
```

### 命名规范

- **包名**: `com.example.myplugin.feature`
- **类名**: `PascalCase`（如 `HelloWorldAction`）
- **方法名**: `camelCase`（如 `actionPerformed`）
- **常量名**: `UPPER_SNAKE_CASE`（如 `MAX_COUNT`）

### 性能优化

1. **延迟加载**
   ```java
   // 使用 LazyValue
   private final LazyValue<MyService> service = LazyValue.atomic(() -> {
       return new MyService();
   });
   ```

2. **缓存结果**
   ```java
   // 缓存计算结果
   private static final Map<String, Result> cache = new ConcurrentHashMap<>();
   ```

3. **异步操作**
   ```java
   // 使用后台任务
   Task.Backgroundable.submit(project, "标题", task -> {
       // 耗时操作
   });
   ```

### 安全最佳实践

1. **验证输入**
   ```java
   public void actionPerformed(@NotNull AnActionEvent event) {
       // 验证项目是否存在
       Project project = event.getProject();
       if (project == null) {
           return;
       }

       // 验证文件是否可写
       VirtualFile file = event.getData(CommonDataKeys.VIRTUAL_FILE);
       if (file == null || !file.isWritable()) {
           return;
       }
   }
   ```

2. **使用 PSI**
   ```java
   // 使用 PSI 进行代码操作
   PsiFile psiFile = PsiDocumentManager.getInstance(project)
       .getPsiFile(document);
   ```

---

## 常见问题 | FAQ

### Q1: 插件无法加载？

**A:** 检查以下几点：

1. `plugin.xml` 配置是否正确
2. IDE 版本是否在兼容范围内
3. 依赖插件是否已安装
4. 查看日志文件

### Q2: Action 不显示？

**A:** 检查：

1. Action 是否正确注册
2. `add-to-group` 的 `group-id` 是否正确
3. 是否需要特定的上下文

### Q3: 如何支持多平台？

**A:** 在 `plugin.xml` 中配置：

```xml
<depends optional="true" config-file="platform-clion.xml">
com.intellij.modules.clion.platform</depends>
<depends optional="true" config-file="platform-pycharm.xml">
com.intellij.modules.python</depends>
```

---

## 下一步 | 学习资源

### 官方资源

- **[IntelliJ Platform Plugin SDK](https://plugins.jetbrains.com/docs/intellij/welcome.html)** - 官方文档
- **[JetBrains Platform DevOps](https://plugins.jetbrains.com/docs/intellij/tools-intellij-platform-gradle-plugin.html)** - 构建工具
- **[IntelliJ Platform SDK Code Samples](https://github.com/JetBrains/intellij-platform-plugin-template)** - 示例代码

### 推荐教程

1. **创建工具窗口** - 添加自定义面板
2. **编辑器扩展** - 自定义代码编辑器
3. **PSI 简介** - 代码结构分析
4. **持久化状态** - 保存插件数据
5. **语言支持** - 为新语言添加支持

### 社区资源

- **[IntelliJ Platform Slack](https://jetbrains-platform.slack.com/)** - 开发者社区
- **[Stack Overflow](https://stackoverflow.com/questions/tagged/intellij-idea-plugin)** - 问答社区
- **[GitHub Discussions](https://github.com/JetBrains/intellij-platform-plugin-template/discussions)** - 讨论区

---

## 总结

恭喜！你已经完成了 IntelliJ 插件开发的入门学习。现在你可以：

- ✅ 创建插件项目
- ✅ 实现基本的 Action
- ✅ 调试和测试插件
- ✅ 构建和分发插件

**继续学习：**

- 探索更多扩展点
- 学习高级特性
- 阅读官方文档
- 查看开源插件示例

祝你在插件开发的旅程中一切顺利！

---

© 2025 JetBrains Plugin Development Guide
