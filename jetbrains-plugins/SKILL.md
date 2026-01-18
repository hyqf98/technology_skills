---
name: jetbrains-plugins
description: JetBrains IDE 插件开发 - 为 IntelliJ IDEA、PyCharm、WebStorm 等 JetBrains IDE 开发、管理和发布插件。包括插件开发、API 参考、测试和 Marketplace 发布。
---

# JetBrains 插件开发技能文档

基于官方文档生成的 JetBrains 插件开发综合指南。

## 何时使用此技能

在以下场景中使用此技能：
- 开发 JetBrains IDE 插件
- 管理已安装的插件
- 发布插件到 JetBrains Marketplace
- 学习插件开发最佳实践
- 调试插件代码

## 技术概述

**JetBrains 插件开发** 是为 IntelliJ IDEA、PyCharm、WebStorm、CLion 等 JetBrains 系列 IDE 开发扩展插件的过程。通过插件可以扩展 IDE 功能，添加新的语言支持、工具集成、主题等。

### 核心特性

- **统一平台**：所有 JetBrains IDE 共享相同的插件平台
- **丰富 API**：提供强大的扩展点和 API
- **Java/Kotlin 支持**：支持使用 Java 或 Kotlin 开发插件
- **DevKit 工具**：专门用于插件开发的 IntelliJ IDEA 插件
- **测试支持**：内置测试框架和工具
- **Marketplace 发布**：一键发布到官方插件仓库

### 支持的 IDE

| IDE | 全称 | 主要用途 |
|-----|------|---------|
| **IntelliJ IDEA** | Java 集成开发环境 | Java、Kotlin 开发 |
| **PyCharm** | Python IDE | Python 开发 |
| **WebStorm** | JavaScript 开发工具 | Web 前端开发 |
| **CLion** | C/C++ IDE | C/C++ 开发 |
| **GoLand** | Go 语言 IDE | Go 开发 |
| **Rider** | .NET IDE | C#/F# 开发 |
| **DataGrip** | 数据库管理工具 | 数据库操作 |
| **AppCode** | Swift/Kotlin IDE | iOS/macOS 开发 |

## 快速参考

### 环境准备

#### 方式 1：使用 IntelliJ IDEA Community Edition（推荐）

```bash
# 1. 下载并安装 IntelliJ IDEA Community Edition
# https://www.jetbrains.com/idea/download/

# 2. 启动 IntelliJ IDEA

# 3. 安装 Plugin DevKit 插件
# Settings/Preferences → Plugins → 搜索 "Plugin DevKit" → 安装

# 4. 创建新的插件项目
# File → New → Project → IntelliJ Platform Plugin
```

#### 方式 2：使用 IntelliJ IDEA Ultimate Edition

```bash
# Ultimate Edition 包含更多功能，但需要许可证
# 对于插件开发，Community Edition 已经足够
```

### 常见配置模式

#### 模式 1：创建插件项目

```xml
<!-- build.gradle.kts -->
plugins {
    id("java")
    id("org.jetbrains.kotlin.jvm") version "1.9.0"
    id("intellij") version "1.17.0"
}

group = "com.example"
version = "1.0.0"

repositories {
    mavenCentral()
}

intellij {
    version.set("2023.3")          // 目标 IDE 版本
    type.set("IC")                  // IC = IntelliJ IDEA Community
    plugins.set(listOf(/* 依赖的插件 */))
    downloadSources.set(true)
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
}
```

#### 模式 2：配置 plugin.xml

```xml
<!-- src/main/resources/META-INF/plugin.xml -->
<idea-plugin>
    <!-- 插件唯一标识符 -->
    <id>com.example.myplugin</id>

    <!-- 插件名称 -->
    <name>My Plugin</name>

    <!-- 插件版本 -->
    <version>1.0.0</version>

    <!-- 供应商信息 -->
    <vendor email="support@example.com" url="https://example.com">
        Example Company
    </vendor>

    <!-- 插件描述 -->
    <description><![CDATA[
        This plugin provides...
        <ul>
            <li>Feature 1</li>
            <li>Feature 2</li>
        </ul>
    ]]></description>

    <!-- 插件变更日志 -->
    <change-notes><![CDATA[
        <h2>1.0.0</h2>
        <ul>
            <li>Initial release</li>
        </ul>
    ]]></change-notes>

    <!-- 兼容的 IDE 版本 -->
    <idea-version since-build="233" until-build="241.*"/>

    <!-- 依赖的插件 -->
    <depends>com.intellij.java</depends>

    <!-- 扩展点 -->
    <extensions defaultExtensionNs="com.intellij">
        <!-- 添加应用程序服务 -->
        <applicationService
            serviceImplementation="com.example.MyApplicationService"/>

        <!-- 添加项目服务 -->
        <projectService
            serviceImplementation="com.example.MyProjectService"/>

        <!-- 添加工具窗口 -->
        <toolWindow
            id="My Tool Window"
            anchor="left"
            factoryClass="com.example.MyToolWindowFactory"/>
    </extensions>

    <!-- 操作注册 -->
    <actions>
        <!-- 添加菜单项 -->
        <action id="com.example.MyAction"
                 class="com.example.MyAction"
                 text="My Action"
                 description="My action description">
            <add-to-group group-id="ToolsMenu" anchor="first"/>
            <keyboard-shortcut keymap="$default" first-keystroke="ctrl shift M"/>
        </action>
    </actions>
</idea-plugin>
```

#### 模式 3：创建简单的 Action

```java
// src/main/java/com/example/MyAction.java
package com.example;

import com.intellij.openapi.actionSystem.AnAction;
import com.intellij.openapi.actionSystem.AnActionEvent;
import com.intellij.openapi.notification.Notification;
import com.intellij.openapi.notification.NotificationType;
import com.intellij.openapi.notification.Notifications;
import org.jetbrains.annotations.NotNull;

public class MyAction extends AnAction {

    @Override
    public void actionPerformed(@NotNull AnActionEvent event) {
        // 显示通知
        Notification notification = new Notification(
            "My Plugin",
            "Action Executed",
            "My action was executed successfully!",
            NotificationType.INFORMATION
        );
        Notifications.Bus.notify(notification, event.getProject());
    }

    @Override
    public void update(@NotNull AnActionEvent event) {
        // 根据条件启用/禁用操作
        event.getPresentation().setEnabledAndVisible(
            event.getProject() != null
        );
    }
}
```

#### 模式 4：创建工具窗口

```java
// src/main/java/com/example/MyToolWindowFactory.java
package com.example;

import com.intellij.openapi.project.Project;
import com.intellij.openapi.wm.ToolWindow;
import com.intellij.openapi.wm.ToolWindowFactory;
import com.intellij.ui.content.Content;
import com.intellij.ui.content.ContentFactory;
import org.jetbrains.annotations.NotNull;

import javax.swing.*;

public class MyToolWindowFactory implements ToolWindowFactory {

    @Override
    public void createToolWindowContent(@NotNull Project project,
                                        @NotNull ToolWindow toolWindow) {
        // 创建工具窗口内容
        MyToolWindowContent content = new MyToolWindowContent();
        ContentFactory contentFactory = ContentFactory.getInstance();

        Content myContent = contentFactory.createContent(
            content.getContent(),
            "",
            false
        );
        toolWindow.getContentManager().addContent(myContent);
    }
}

// src/main/java/com/example/MyToolWindowContent.java
class MyToolWindowContent {
    private JPanel myContent;

    public MyToolWindowContent() {
        myContent = new JPanel();
        myContent.add(new JLabel("Hello from Tool Window!"));
    }

    public JComponent getContent() {
        return myContent;
    }
}
```

#### 模式 5：创建项目服务

```java
// src/main/java/com/example/MyProjectService.java
package com.example;

import com.intellij.openapi.components.Service;
import com.intellij.openapi.project.Project;
import org.jetbrains.annotations.NotNull;

@Service(Service.Level.PROJECT)
public final class MyProjectService {

    private final Project project;

    public MyProjectService(@NotNull Project project) {
        this.project = project;
    }

    public static MyProjectService getInstance(@NotNull Project project) {
        return project.getService(MyProjectService.class);
    }

    public void doSomething() {
        // 服务实现
    }
}
```

#### 模式 6：持久化状态

```java
// src/main/java/com/example/MyPluginSettings.java
package com.example;

import com.intellij.openapi.components.PersistentStateComponent;
import com.intellij.openapi.components.Service;
import com.intellij.openapi.components.State;
import com.intellij.openapi.components.Storage;
import org.jetbrains.annotations.NotNull;
import org.jetbrains.annotations.Nullable;

@State(
    name = "MyPluginSettings",
    storages = @Storage("MyPluginSettings.xml")
)
@Service(Service.Level.PROJECT)
public final class MyPluginSettings implements PersistentStateComponent<MyPluginSettings.State> {

    public static class State {
        public String apiKey = "";
        public boolean enableFeature = true;
        public int maxItems = 100;
    }

    private State state = new State();

    public static MyPluginSettings getInstance(@NotNull com.intellij.openapi.project.Project project) {
        return project.getService(MyPluginSettings.class);
    }

    @Override
    public @NotNull State getState() {
        return state;
    }

    @Override
    public void loadState(@NotNull State state) {
        this.state = state;
    }

    // Getter 和 Setter
    public String getApiKey() {
        return state.apiKey;
    }

    public void setApiKey(String apiKey) {
        state.apiKey = apiKey;
    }

    public boolean isEnableFeature() {
        return state.enableFeature;
    }

    public void setEnableFeature(boolean enableFeature) {
        state.enableFeature = enableFeature;
    }

    public int getMaxItems() {
        return state.maxItems;
    }

    public void setMaxItems(int maxItems) {
        state.maxItems = maxItems;
    }
}
```

#### 模式 7：运行和调试插件

```gradle
// build.gradle.kts

// 运行 IDE 并加载插件
task<RunIdeTask>("runIde") {
    systemProperty("idea.debug.mode", "true")
}

// 调试插件
task<RunIdeTask>("debugIde") {
    systemProperty("idea.debug.mode", "true")
    jvmArgs("-Xdebug", "-Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5005")
}

// 验证插件
task<VerifyPluginTask>("verifyPlugin") {
    pluginIds.add("com.example.myplugin")
}
```

#### 模式 8：构建和发布插件

```bash
# 1. 验证插件
./gradlew verifyPlugin

# 2. 构建插件
./gradlew buildPlugin

# 3. 发布到 JetBrains Marketplace
./gradlew publishPlugin

# 或手动上传
# 构建的插件位于 build/distributions/
# 访问 https://plugins.jetbrains.com/ 上传
```

### 核心概念

#### 扩展点（Extension Points）

扩展点是插件与 IDE 交互的主要方式：

| 扩展点类型 | 说明 | 示例 |
|-----------|------|------|
| **Application Service** | 应用级别的服务 | 配置管理、全局状态 |
| **Project Service** | 项目级别的服务 | 项目配置、项目状态 |
| **Tool Window** | 工具窗口 | Git、Database 工具窗口 |
| **Editor Action** | 编辑器操作 | 代码格式化、重构 |
| **File Type** | 文件类型识别 | 自定义文件类型支持 |
| **Completion Contributor** | 代码补全 | 自定义补全逻辑 |
| **Line Marker Provider** | 行标记 | 覆盖指示器、导航图标 |

#### 生命周期

```java
// 插件组件生命周期
public class MyComponent implements ProjectComponent {

    @Override
    public void projectOpened() {
        // 项目打开时调用
    }

    @Override
    public void projectClosed() {
        // 项目关闭时调用
    }

    @Override
    public void initComponent() {
        // 组件初始化时调用
    }

    @Override
    public void disposeComponent() {
        // 组件销毁时调用
    }
}
```

## 开发最佳实践

### 1. 遵循命名规范

```
com.example.myplugin
├── actions/          // 操作类
├── services/         // 服务类
├── toolwindows/      // 工具窗口
├── components/       // UI 组件
├── utils/            // 工具类
└── model/            // 数据模型
```

### 2. 使用依赖注入

```java
// 使用构造函数注入依赖
public class MyService {
    private final Project project;
    private final MyProjectSettings settings;

    public MyService(Project project) {
        this.project = project;
        this.settings = MyProjectSettings.getInstance(project);
    }
}
```

### 3. 正确使用读操作

```java
// 使用 Read Action 包装读操作
String content = ReadAction.compute(() ->
    myFile.getText()
);
```

### 4. 正确使用写操作

```java
// 使用 Write Action 包装写操作
WriteAction.run(() -> {
    myFile.setText("New content");
});
```

### 5. 异步处理

```java
// 使用后台任务
Task.Backgroundable.submit(project, "Processing...", task -> {
    // 后台处理逻辑
    ProgressManager.checkCanceled(); // 检查取消
});
```

## 测试插件

### 单元测试

```java
// src/test/java/com/example/MyServiceTest.java
package com.example;

import com.intellij.testFramework.LightPlatformTestCase;
import org.junit.Test;

public class MyServiceTest extends LightPlatformTestCase {

    @Test
    public void testService() {
        MyProjectService service = MyProjectService.getInstance(getProject());
        assertNotNull(service);
        // 更多测试...
    }
}
```

### 运行测试

```bash
# 运行所有测试
./gradlew test

# 运行特定测试
./gradlew test --tests MyServiceTest

# 生成测试报告
./gradlew test report
```

## 发布插件

### 准备工作

1. **创建 JetBrains 账户**
   - 访问 https://account.jetbrains.com/
   - 注册或登录账户

2. **生成令牌**
   - 访问 https://plugins.jetbrains.com/author/me/tokens
   - 创建新的发布令牌

### 发布流程

```bash
# 1. 配置 Gradle 属性
# ~/.gradle/gradle.properties
PUBLISH_TOKEN=your_token_here

# 2. 验证插件
./gradlew verifyPlugin

# 3. 构建插件
./gradlew buildPlugin

# 4. 发布插件
./gradlew publishPlugin
```

### 手动发布

1. 构建插件：`./gradlew buildPlugin`
2. 访问 https://plugins.jetbrains.com/
3. 登录并点击 "Upload plugin"
4. 上传 `build/distributions/` 中的 ZIP 文件

## 参考文档

此技能在 `references/` 目录中包含全面的文档：

| 文档 | 描述 |
|------|------|
| **plugin_management.md** | 插件管理文档 |

## 使用指南

### 对于初学者
1. 安装 IntelliJ IDEA Community Edition
2. 创建新的插件项目
3. 运行和调试示例代码
4. 学习 plugin.xml 配置
5. 开发简单的 Action

### 对于进阶开发
- 掌握扩展点机制
- 学习服务和状态持久化
- 创建自定义 UI 组件
- 实现代码补全和检查

### 对于发布
- 验证插件兼容性
- 编写完整的文档
- 准备截图和演示
- 提交到 Marketplace

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
- 插件开发需要 Java 基础知识
- 不同 IDE 版本可能有 API 差异
- 发布前务必充分测试

## 更新说明

刷新此技能的文档：
1. 使用相同配置重新运行爬虫
2. 技能将使用最新信息重建

## 相关资源

### 官方文档
- [IntelliJ Platform Plugin SDK](https://plugins.jetbrains.com/docs/intellij/welcome.html)
- [Developing a Plugin](https://plugins.jetbrains.com/docs/intellij/developing-plugins.html)
- [Publishing a Plugin](https://plugins.jetbrains.com/docs/intellij/publishing-plugin.html)
- [JetBrains Marketplace](https://plugins.jetbrains.com/)

### 最新更新（2025）
- [IntelliJ Platform 2025.3: What Plugin Developers Should Know](https://blog.jetbrains.com/platform/2025/11/intellij-platform-2025-3-what-plugin-developers-should-know/)
- [JetBrains Plugin Developer Conf 2025](https://www.youtube.com/watch?v=0D3sYiT2Ca0)
- [JetBrains Plugin Developer Conf 2025 Recordings](https://blog.jetbrains.com/platform/2025/11/jetbrains-plugin-developer-conf-2025-recordings-are-now-live/)

### 学习资源
- [Plugin Development Tutorial](https://plugins.jetbrains.com/docs/intellij/getting_started.html)
- [Plugin Examples](https://github.com/JetBrains/intellij-sdk-code-samples)
- [JetBrains Platform Slack](https://plugins.jetbrains.com/slack)
