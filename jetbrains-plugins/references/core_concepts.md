# IntelliJ Platform 核心概念

**页数:** 20

---

## 概述 | 插件架构

IntelliJ Platform 插件基于以下核心概念构建：

1. **平台（Platform）** - IDE 的核心基础设施
2. **扩展点（Extension Points）** - 定义可扩展的接口
3. **扩展（Extensions）** - 实现扩展点的具体实现
4. **动作（Actions）** - 用户可触发的操作
5. **服务（Services）** - 后台运行的组件
6. **组件（Components）** - UI 和功能模块

---

## 1. 扩展点（Extension Points）

### 什么是扩展点？

扩展点是插件向 IDE 或其他插件提供的接口，允许其他插件扩展其功能。

### 声明扩展点

**在 plugin.xml 中声明：**

```xml
<idea-plugin>
    <id>com.example.myplugin</id>

    <!-- 声明扩展点 -->
    <extensionPoints>
        <!-- 接口类型的扩展点 -->
        <extensionPoint
            name="myExtension"
            interface="com.example.MyExtensionInterface"/>

        <!-- Bean 类型的扩展点 -->
        <extensionPoint
            name="myBeanExtension"
            beanClass="com.example.MyExtensionBean"
            dynamic="true"
            area="IDEA_PROJECT"/>
    </extensionPoints>
</idea-plugin>
```

### 扩展点属性

| 属性 | 说明 | 必需 |
|------|------|------|
| `name` | 扩展点名称 | ✅ |
| `interface` | 扩展接口 | ✅* |
| `beanClass` | 扩展 Bean 类 | ✅* |
| `dynamic` | 是否支持动态加载 | ❌ |
| `area` | 作用域（APPLICATION/PROJECT/MODULE） | ❌ |

*`interface` 和 `beanClass` 至少需要一个

### 扩展点类型

#### 接口扩展点（Interface Extension Point）

**定义接口：**

```java
package com.example;

public interface MyExtensionInterface {
    String process(String input);
    void onEvent(EventArgs args);
}
```

**声明扩展点：**

```xml
<extensionPoint
    name="dataProcessor"
    interface="com.example.MyExtensionInterface"/>
```

**其他插件实现扩展：**

```xml
<!-- 另一个插件的 plugin.xml -->
<idea-plugin>
    <depends>com.example.myplugin</depends>

    <extensions defaultExtensionNs="com.example.myplugin">
        <dataProcessor
            implementation="com.other.MyDataProcessorImpl"/>
    </extensions>
</idea-plugin>
```

**实现类：**

```java
package com.other;

import com.example.MyExtensionInterface;

public class MyDataProcessorImpl implements MyExtensionInterface {
    @Override
    public String process(String input) {
        return input.toUpperCase();
    }

    @Override
    public void onEvent(EventArgs args) {
        // 处理事件
    }
}
```

#### Bean 扩展点（Bean Extension Point）

**定义 Bean 类：**

```java
package com.example;

import com.intellij.openapi.extensions.ExtensionPointBean;

public class MyExtensionBean extends ExtensionPointBean {
    private String key;

    private String implementationClass;

    // Getter 和 Setter
    public String getKey() {
        return key;
    }

    public void setKey(String key) {
        this.key = key;
    }

    public String getImplementationClass() {
        return implementationClass;
    }

    public void setImplementationClass(String implementationClass) {
        this.implementationClass = implementationClass;
    }
}
```

**声明扩展点：**

```xml
<extensionPoint
    name="customProcessor"
    beanClass="com.example.MyExtensionBean"
    dynamic="true"/>
```

**其他插件使用扩展：**

```xml
<extensions defaultExtensionNs="com.example.myplugin">
    <customProcessor
        key="processor1"
        implementationClass="com.other.Processor1"/>
    <customProcessor
        key="processor2"
        implementationClass="com.other.Processor2"/>
</extensions>
```

---

## 2. 扩展（Extensions）

### 什么是扩展？

扩展是对扩展点的具体实现，插件通过扩展来扩展 IDE 或其他插件的功能。

### 查看可用扩展点

IntelliJ Platform 提供了数百个内置扩展点：

**常用扩展点：**

| 扩展点名 | 功能 | 命名空间 |
|---------|------|---------|
| `applicationService` | 应用级服务 | `com.intellij` |
| `projectService` | 项目级服务 | `com.intellij` |
| `toolWindow` | 工具窗口 | `com.intellij` |
| `editorFactoryListener` | 编辑器监听器 | `com.intellij` |
| `fileType` | 文件类型 | `com.intellij` |
| `colorSettingsPage` | 颜色设置页面 | `com.intellij` |

### 注册扩展

**在 plugin.xml 中注册扩展：**

```xml
<idea-plugin>
    <extensions defaultExtensionNs="com.intellij">
        <!-- 应用服务 -->
        <applicationService
            serviceImplementation="com.example.MyApplicationServiceImpl"/>

        <!-- 项目服务 -->
        <projectService
            serviceImplementation="com.example.MyProjectServiceImpl"/>

        <!-- 工具窗口 -->
        <toolWindow
            id="MyToolWindow"
            anchor="right"
            factoryClass="com.example.MyToolWindowFactory"/>
    </extensions>
</idea-plugin>
```

### 扩展作用域

| 作用域 | 说明 | 生命周期 |
|--------|------|---------|
| `IDEA_APPLICATION` | 应用级别 | IDE 启动到关闭 |
| `IDEA_PROJECT` | 项目级别 | 项目打开到关闭 |
| `IDEA_MODULE` | 模块级别 | 模块加载到卸载 |

**配置作用域：**

```xml
<extensionPoint
    name="projectExtension"
    interface="com.example.ProjectExtension"
    area="IDEA_PROJECT"/>
```

---

## 3. 动作（Actions）

### 动作系统

动作是用户可以触发的操作，如菜单项、工具栏按钮等。

### 动作继承层次

```
AnAction (抽象基类)
    ├── AnAction.Transactional (事务性动作)
    ├── ToggleAction (切换动作)
    └── DumbAwareAction (无索引操作时可用)
```

### 创建动作

#### 基础动作

```java
package com.example.actions;

import com.intellij.openapi.actionSystem.*;
import com.intellij.openapi.project.Project;
import org.jetbrains.annotations.NotNull;

public class MyAction extends AnAction {

    @Override
    public void actionPerformed(@NotNull AnActionEvent event) {
        // 获取数据上下文
        DataContext dataContext = event.getDataContext();

        // 获取项目
        Project project = event.getProject();

        // 获取当前文件
        VirtualFile file = event.getData(CommonDataKeys.VIRTUAL_FILE);

        // 执行业务逻辑
        // ...
    }

    @Override
    public void update(@NotNull AnActionEvent event) {
        // 更新动作状态（启用/禁用）
        Project project = event.getProject();
        event.getPresentation().setEnabled(project != null);
    }
}
```

#### 切换动作

```java
package com.example.actions;

import com.intellij.openapi.actionSystem.*;
import com.intellij.openapi.project.Project;
import org.jetbrains.annotations.NotNull;

public class MyToggleAction extends ToggleAction {
    @Override
    public boolean isSelected(@NotNull AnActionEvent event) {
        // 返回当前状态
        MyPluginState state = MyPluginState.getInstance();
        return state.isFeatureEnabled();
    }

    @Override
    public void setSelected(@NotNull AnActionEvent event, boolean state) {
        // 切换状态
        MyPluginState pluginState = MyPluginState.getInstance();
        pluginState.setFeatureEnabled(state);
    }
}
```

#### 动作组（Action Group）

```java
package com.example.actions;

import com.intellij.openapi.actionSystem.*;
import org.jetbrains.annotations.NotNull;

public class MyActionGroup extends ActionGroup {
    public MyActionGroup() {
        // 设置组属性
        setPopup(true);  // 显示为弹出菜单
        setText("My Actions");
    }

    @NotNull
    @Override
    public AnAction[] getChildren(@Nullable AnActionEvent event) {
        // 返回子动作
        return new AnAction[]{
            new MyAction1(),
            new MyAction2(),
            Separator.create(),
            new MyAction3()
        };
    }
}
```

### 注册动作

**在 plugin.xml 中注册：**

```xml
<actions>
    <!-- 简单动作 -->
    <action
        id="MyPlugin.MyAction"
        class="com.example.actions.MyAction"
        text="My Action"
        description="Perform my action"
        icon="META-INF/icons/myAction.png">
        <add-to-group group-id="ToolsMenu" anchor="first"/>
        <keyboard-shortcut keymap="$default" first-keystroke="ctrl shift M"/>
    </action>

    <!-- 动作组 -->
    <group
        id="MyPlugin.MyActionGroup"
        class="com.example.actions.MyActionGroup"
        text="My Actions"
        popup="true">
        <add-to-group group-id="ToolsMenu" anchor="after" relative-to-action="MyPlugin.MyAction"/>
    </group>

    <!-- 在编辑器弹出菜单中添加 -->
    <action
        id="MyPlugin.EditorAction"
        class="com.example.actions.EditorAction"
        text="Editor Action">
        <add-to-group group-id="EditorPopupMenu" anchor="first"/>
    </action>
</actions>
```

### 常用动作组位置

| 组 ID | 位置 | 说明 |
|-------|------|------|
| `ToolsMenu` | Tools 菜单 | 工具菜单 |
| `FileMenu` | File 菜单 | 文件菜单 |
| `EditMenu` | Edit 菜单 | 编辑菜单 |
| `EditorPopupMenu` | 编辑器右键菜单 | 上下文菜单 |
| `ProjectViewPopupMenu` | 项目视图右键菜单 | 项目上下文菜单 |
| `MainToolBar` | 主工具栏 | 顶部工具栏 |
| `NavBarToolBar` | 导航栏工具栏 | 导航栏 |

### 动作数据键（Data Keys）

从 `AnActionEvent` 获取常用数据：

```java
@Override
public void actionPerformed(@NotNull AnActionEvent event) {
    // 项目
    Project project = event.getProject();

    // 虚拟文件
    VirtualFile file = event.getData(CommonDataKeys.VIRTUAL_FILE);

    // PSI 文件
    PsiFile psiFile = event.getData(CommonDataKeys.PSI_FILE);

    // 编辑器
    Editor editor = event.getData(CommonDataKeys.EDITOR);

    // 模块
    Module module = event.getData(CommonDataKeys.MODULE);

    // 自定义数据
    MyCustomData data = event.getData(MyDataKeys.MY_CUSTOM_DATA);
}
```

---

## 4. 服务（Services）

### 服务概念

服务是后台运行的组件，提供特定功能。服务分为三种级别：

1. **应用服务（Application Service）** - 全局单例
2. **项目服务（Project Service）** - 项目级别单例
3. **模块服务（Module Service）** - 模块级别单例

### 应用服务

**实现服务接口：**

```java
package com.example.services;

import com.intellij.openapi.components.Service;

@Service
public final class MyApplicationService {
    private static final Logger LOG = Logger.getInstance(MyApplicationService.class);

    private final MyState state;

    public MyApplicationService() {
        this.state = new MyState();
        LOG.info("Application service initialized");
    }

    public void doSomething() {
        // 服务逻辑
        LOG.info("Doing something");
    }

    public MyState getState() {
        return state;
    }
}
```

**注册服务（plugin.xml）：**

```xml
<extensions defaultExtensionNs="com.intellij">
    <applicationService
        serviceImplementation="com.example.services.MyApplicationService"/>
</extensions>
```

**使用服务：**

```java
// 获取服务实例
MyApplicationService service = MyApplicationService.getInstance();
service.doSomething();
```

### 项目服务

**实现服务接口：**

```java
package com.example.services;

import com.intellij.openapi.components.Service;
import com.intellij.openapi.project.Project;

@Service(Service.Level.PROJECT)
public final class MyProjectService {
    private final Project project;

    public MyProjectService(Project project) {
        this.project = project;
    }

    public void doSomething() {
        // 使用 project
    }
}
```

**注册服务（plugin.xml）：**

```xml
<extensions defaultExtensionNs="com.intellij">
    <projectService
        serviceImplementation="com.example.services.MyProjectService"/>
</extensions>
```

**使用服务：**

```java
// 在有 Project 上下文的地方
MyProjectService service = project.getService(MyProjectService.class);
service.doSomething();
```

### 持久化状态服务

**定义状态类：**

```java
package com.example.state;

import com.intellij.openapi.components.PersistentStateComponent;
import com.intellij.openapi.components.State;
import com.intellij.openapi.components.Storage;
import com.intellij.util.xmlb.XmlSerializerUtil;

@State(
    name = "MyPluginSettings",
    storages = @Storage("MyPluginSettings.xml")
)
public class MyPluginSettings implements PersistentStateComponent<MyPluginSettings> {
    public String userName = "Default User";
    public boolean enableFeature = true;
    public int maxCount = 100;

    @Override
    public MyPluginSettings getState() {
        return this;
    }

    @Override
    public void loadState(@NotNull MyPluginSettings state) {
        XmlSerializerUtil.copyBean(state, this);
    }

    public static MyPluginSettings getInstance() {
        return Application.getApplication().getService(MyPluginSettings.class);
    }
}
```

**注册服务：**

```xml
<extensions defaultExtensionNs="com.intellij">
    <applicationService
        serviceImplementation="com.example.state.MyPluginSettings"/>
</extensions>
```

**使用状态：**

```java
// 获取设置
MyPluginSettings settings = MyPluginSettings.getInstance();
String userName = settings.userName;

// 更新设置
settings.userName = "New User";
settings.enableFeature = false;
```

---

## 5. 工具窗口（Tool Windows）

### 创建工具窗口

**实现工厂类：**

```java
package com.example.toolwindow;

import com.intellij.openapi.project.Project;
import com.intellij.openapi.wm.ToolWindow;
import com.intellij.openapi.wm.ToolWindowFactory;
import com.intellij.ui.content.Content;
import com.intellij.ui.content.ContentFactory;
import org.jetbrains.annotations.NotNull;

public class MyToolWindowFactory implements ToolWindowFactory {

    @Override
    public void createToolWindowContent(@NotNull Project project, @NotNull ToolWindow toolWindow) {
        // 创建工具窗口内容
        MyToolWindowContent content = new MyToolWindowContent(project);

        // 添加到工具窗口
        ContentFactory contentFactory = ContentFactory.getInstance();
        Content contentPanel = contentFactory.createContent(
            content.getContentPanel(),
            "",
            false
        );
        toolWindow.getContentManager().addContent(contentPanel);
    }
}
```

**创建内容面板：**

```java
package com.example.toolwindow;

import com.intellij.openapi.project.Project;
import com.intellij.openapi.ui.SimpleToolWindowPanel;
import com.intellij.ui.components.JBList;
import com.intellij.ui.components.JBScrollPane;

import javax.swing.*;

public class MyToolWindowContent {
    private final Project project;
    private final SimpleToolWindowPanel panel;

    public MyToolWindowContent(Project project) {
        this.project = project;
        this.panel = new SimpleToolWindowPanel(true, true);

        // 创建内容组件
        JComponent content = createContent();

        // 设置内容
        panel.setContent(content);
    }

    private JComponent createContent() {
        // 创建列表
        JBList<String> list = new JBList<>(
            new String[]{"Item 1", "Item 2", "Item 3"}
        );

        // 创建滚动面板
        return new JBScrollPane(list);
    }

    public JComponent getContentPanel() {
        return panel;
    }
}
```

**注册工具窗口：**

```xml
<extensions defaultExtensionNs="com.intellij">
    <toolWindow
        id="MyToolWindow"
        anchor="right"
        factoryClass="com.example.toolwindow.MyToolWindowFactory"
        icon="META-INF/icons/toolWindow.png"/>
</extensions>
```

**工具窗口属性：**

| 属性 | 说明 | 可选值 |
|------|------|--------|
| `id` | 工具窗口唯一标识 | 任意字符串 |
| `anchor` | 位置 | `left`, `right`, `top`, `bottom` |
| `factoryClass` | 工厂类完整路径 | 类路径 |
| `icon` | 图标路径 | 资源路径 |
| `secondary` | 是否为次要窗口 | `true`, `false` |

---

## 6. 监听器（Listeners）

### 监听器类型

IntelliJ Platform 提供了多种事件监听器：

#### 项目监听器

```java
package com.example.listeners;

import com.intellij.openapi.project.Project;
import com.intellij.openapi.project.ProjectManagerListener;
import org.jetbrains.annotations.NotNull;

public class MyProjectManagerListener implements ProjectManagerListener {

    @Override
    public void projectOpened(@NotNull Project project) {
        System.out.println("项目已打开: " + project.getName());
    }

    @Override
    public void projectClosing(@NotNull Project project) {
        System.out.println("项目正在关闭: " + project.getName());
    }
}
```

**注册监听器：**

```xml
<extensions defaultExtensionNs="com.intellij">
    <projectManagerListener
        implementation="com.example.listeners.MyProjectManagerListener"/>
</extensions>
```

#### 编辑器监听器

```java
package com.example.listeners;

import com.intellij.openapi.editor.event.EditorFactoryEvent;
import com.intellij.openapi.editor.event.EditorFactoryListener;
import org.jetbrains.annotations.NotNull;

public class MyEditorFactoryListener implements EditorFactoryListener {

    @Override
    public void editorCreated(@NotNull EditorFactoryEvent event) {
        System.out.println("编辑器已创建");
    }

    @Override
    public void editorReleased(@NotNull EditorFactoryEvent event) {
        System.out.println("编辑器已释放");
    }
}
```

**注册监听器：**

```xml
<extensions defaultExtensionNs="com.intellij">
    <editorFactoryListener
        implementation="com.example.listeners.MyEditorFactoryListener"/>
</extensions>
```

#### 文件监听器

```java
package com.example.listeners;

import com.intellij.openapi.vfs.VirtualFile;
import com.intellij.openapi.vfs.VirtualFileListener;
import com.intellij.openapi.vfs.VirtualFileEvent;
import org.jetbrains.annotations.NotNull;

public class MyVirtualFileListener implements VirtualFileListener {

    @Override
    public void contentsChanged(@NotNull VirtualFileEvent event) {
        VirtualFile file = event.getFile();
        System.out.println("文件内容已更改: " + file.getName());
    }

    @Override
    public void fileCreated(@NotNull VirtualFileEvent event) {
        VirtualFile file = event.getFile();
        System.out.println("文件已创建: " + file.getName());
    }

    @Override
    public void fileDeleted(@NotNull VirtualFileEvent event) {
        VirtualFile file = event.getFile();
        System.out.println("文件已删除: " + file.getName());
    }
}
```

**注册监听器：**

```xml
<extensions defaultExtensionNs="com.intellij">
    <virtualFileListener
        implementation="com.example.listeners.MyVirtualFileListener"/>
</extensions>
```

---

## 7. 数据上下文（Data Context）

### DataKeys

数据上下文允许访问 IDE 中的各种对象。

**定义自定义 DataKey：**

```java
package com.example;

import com.intellij.openapi.actionSystem.DataKey;

public class MyDataKeys {
    public static final DataKey<MyCustomData> MY_CUSTOM_DATA =
        DataKey.create("myCustomData");

    public static final DataKey<String> MY_STRING_KEY =
        DataKey.create("myStringKey");
}
```

**在动作中使用：**

```java
@Override
public void actionPerformed(@NotNull AnActionEvent event) {
    MyCustomData data = event.getData(MyDataKeys.MY_CUSTOM_DATA);
    String value = event.getData(MyDataKeys.MY_STRING_KEY);

    if (data != null) {
        // 使用数据
    }
}
```

**提供数据：**

```java
package com.example;

import com.intellij.openapi.actionSystem.DataProvider;
import org.jetbrains.annotations.Nullable;
import org.jetbrains.annotations.NotNull;

public class MyPanel extends JPanel implements DataProvider {
    private MyCustomData data;

    @Nullable
    @Override
    public Object getData(@NotNull String dataId) {
        if (MyDataKeys.MY_CUSTOM_DATA.is(dataId)) {
            return data;
        }
        return null;
    }
}
```

---

## 8. PSI（Program Structure Interface）

### PSI 概念

PSI 是 IntelliJ Platform 的核心，用于表示和操作代码结构。

### PSI 层次结构

```
PsiFile (文件)
    ├── PsiDirectory (目录)
    └── PsiElement (元素)
        ├── PsiWhiteSpace (空白)
        ├── PsiComment (注释)
        ├── PsiErrorElement (错误元素)
        └── 具体语言元素
            ├── PsiJavaFile (Java 文件)
            ├── PsiClass (类)
            ├── PsiMethod (方法)
            └── PsiField (字段)
```

### 使用 PSI

**访问 PSI 文件：**

```java
package com.example.psi;

import com.intellij.openapi.vfs.VirtualFile;
import com.intellij.psi.PsiFile;
import com.intellij.psi.PsiManager;
import com.intellij.openapi.project.Project;

public class PsiExample {
    public static void processFile(Project project, VirtualFile virtualFile) {
        PsiManager psiManager = PsiManager.getInstance(project);
        PsiFile psiFile = psiManager.findFile(virtualFile);

        if (psiFile != null) {
            // 处理 PSI 文件
            processPsiFile(psiFile);
        }
    }

    private static void processPsiFile(PsiFile psiFile) {
        // 遍历 PSI 元素
        psiFile.accept(new PsiElementVisitor() {
            @Override
            public void visitElement(@NotNull PsiElement element) {
                // 处理每个元素
                System.out.println("元素类型: " + element.getClass().getSimpleName());
                super.visitElement(element);
            }
        });
    }
}
```

**Java PSI 示例：**

```java
package com.example.psi;

import com.intellij.psi.*;
import com.intellij.psi.util.PsiTreeUtil;

public class JavaPsiExample {

    public static void processJavaClass(PsiClass psiClass) {
        // 获取类名
        String className = psiClass.getName();

        // 获取方法
        PsiMethod[] methods = psiClass.getMethods();
        for (PsiMethod method : methods) {
            processMethod(method);
        }

        // 获取字段
        PsiField[] fields = psiClass.getFields();
        for (PsiField field : fields) {
            processField(field);
        }
    }

    private static void processMethod(PsiMethod method) {
        // 获取方法名
        String methodName = method.getName();

        // 获取返回类型
        PsiType returnType = method.getReturnType();

        // 获取参数
        PsiParameter[] parameters = method.getParameterList().getParameters();

        // 获取方法体
        PsiCodeBlock body = method.getBody();
    }

    private static void processField(PsiField field) {
        // 获取字段名
        String fieldName = field.getName();

        // 获取字段类型
        PsiType fieldType = field.getType();

        // 检查是否为静态
        boolean isStatic = field.hasModifierProperty(PsiModifier.STATIC);
    }
}
```

---

## 9. 通知（Notifications）

### 显示通知

```java
package com.example.notifications;

import com.intellij.notification.*;
import com.intellij.openapi.project.Project;

public class MyNotification {
    private static final NotificationGroup NOTIFICATION_GROUP =
        new NotificationGroup(
            "My Plugin Notification Group",
            NotificationDisplayType.BALLOON,
            true
        );

    public static void showInfo(Project project, String title, String content) {
        Notification notification = NOTIFICATION_GROUP.createNotification(
            title,
            content,
            NotificationType.INFORMATION,
            null
        );
        notification.notify(project);
    }

    public static void showWarning(Project project, String title, String content) {
        Notification notification = NOTIFICATION_GROUP.createNotification(
            title,
            content,
            NotificationType.WARNING,
            null
        );
        notification.notify(project);
    }

    public static void showError(Project project, String title, String content) {
        Notification notification = NOTIFICATION_GROUP.createNotification(
            title,
            content,
            NotificationType.ERROR,
            null
        );
        notification.notify(project);
    }
}
```

**使用通知：**

```java
// 显示信息通知
MyNotification.showInfo(project, "成功", "操作已完成");

// 显示警告通知
MyNotification.showWarning(project, "警告", "请检查配置");

// 显示错误通知
MyNotification.showError(project, "错误", "操作失败");
```

---

## 10. 后台任务（Background Tasks）

### 后台任务

```java
package com.example.tasks;

import com.intellij.openapi.progress.ProgressIndicator;
import com.intellij.openapi.progress.ProgressManager;
import com.intellij.openapi.progress.Task;
import com.intellij.openapi.project.Project;
import org.jetbrains.annotations.NotNull;

public class MyBackgroundTask {

    public static void runBackgroundTask(Project project) {
        ProgressManager.getInstance().run(new Task.Backgroundable(project, "My Task", true) {
            private String result;

            @Override
            public void run(@NotNull ProgressIndicator indicator) {
                // 更新进度文本
                indicator.setText("正在处理...");
                indicator.setIndeterminate(false);

                // 执行任务
                for (int i = 0; i < 100; i++) {
                    // 检查是否取消
                    if (indicator.isCanceled()) {
                        return;
                    }

                    // 更新进度
                    indicator.setFraction((double) i / 100);

                    // 执行工作
                    doWork();

                    // 检查是否取消
                    if (indicator.isCanceled()) {
                        return;
                    }
                }

                result = "任务完成";
            }

            @Override
            public void onSuccess() {
                // 任务成功后执行（在 EDT 中）
                MyNotification.showInfo(project, "成功", result);
            }

            @Override
            public void onThrowable(@NotNull Throwable error) {
                // 任务出错时执行
                MyNotification.showError(project, "错误", error.getMessage());
            }

            private void doWork() {
                try {
                    Thread.sleep(50);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        });
    }
}
```

---

## 最佳实践

### 1. 服务优先

优先使用服务而不是单例：

```java
// ❌ 不好：使用单例
MyService.getInstance().doSomething();

// ✅ 好：使用服务
project.getService(MyService.class).doSomething();
```

### 2. 异步操作

耗时操作应在后台线程执行：

```java
// ❌ 不好：在 EDT 中执行耗时操作
@Override
public void actionPerformed(@NotNull AnActionEvent event) {
    heavyOperation();  // 阻塞 UI
}

// ✅ 好：在后台线程执行
@Override
public void actionPerformed(@NotNull AnActionEvent event) {
    ProgressManager.getInstance().run(new Task.Backgroundable(...) {
        @Override
        public void run(@NotNull ProgressIndicator indicator) {
            heavyOperation();
        }
    });
}
```

### 3. 数据验证

始终验证数据：

```java
@Override
public void actionPerformed(@NotNull AnActionEvent event) {
    // 验证项目
    Project project = event.getProject();
    if (project == null) {
        return;
    }

    // 验证文件
    VirtualFile file = event.getData(CommonDataKeys.VIRTUAL_FILE);
    if (file == null || !file.isValid()) {
        return;
    }

    // 执行操作
    performAction(project, file);
}
```

---

## 总结

IntelliJ Platform 的核心概念为插件开发提供了强大而灵活的架构：

- ✅ **扩展点** - 定义可扩展的接口
- ✅ **扩展** - 实现具体功能
- ✅ **动作** - 用户交互
- ✅ **服务** - 后台逻辑
- ✅ **工具窗口** - 自定义 UI
- ✅ **监听器** - 响应事件
- ✅ **PSI** - 代码操作
- ✅ **通知** - 用户反馈
- ✅ **后台任务** - 异步处理

掌握这些概念后，你就可以开发功能丰富的 IntelliJ 插件了！

---

© 2025 IntelliJ Platform Plugin Development Guide
