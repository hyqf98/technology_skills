# JetBrains 插件管理

**页数:** 1

---

## 插件安装与管理

### 插件来源

JetBrains IDE 支持从以下来源安装插件：

| 来源 | 描述 | 推荐度 |
|------|------|--------|
| **JetBrains Marketplace** | 官方插件市场 | ⭐⭐⭐⭐⭐ |
| **自定义插件仓库** | 企业内部或第三方仓库 | ⭐⭐⭐ |
| **本地文件** | 从磁盘安装插件 | ⭐⭐ |

---

## 在 JetBrains Marketplace 浏览插件

**URL:** https://www.jetbrains.com/help/idea/2025.3/managing-plugins.html

### 访问方式

1. **在线浏览**
   - 访问 [JetBrains Marketplace](https://plugins.jetbrains.com/)
   - 搜索所需插件
   - 查看插件详情、评分和评论

2. **IDE 内浏览**
   - 打开 **Settings/Preferences**
   - 选择 **Plugins**
   - 点击 **Marketplace** 标签
   - 浏览和搜索插件

---

## 安装插件

### 方法 1：从 Marketplace 安装（推荐）

1. 打开 IDE 设置
   - **Windows/Linux**: `File` → `Settings`
   - **macOS**: `IntelliJ IDEA` → `Preferences`

2. 导航到 **Plugins**

3. 点击 **Marketplace** 标签

4. 搜索插件名称

5. 点击 **Install** 按钮

6. 重启 IDE 以激活插件

### 方法 2：从磁盘安装

1. 下载插件 ZIP 文件

2. 打开 **Settings/Preferences**

3. 选择 **Plugins**

4. 点击 ⚙️ 图标

5. 选择 **Install Plugin from Disk...**

6. 选择下载的 ZIP 文件

7. 重启 IDE

### 方法 3：命令行安装

```bash
# 使用 bin/installer.sh 脚本
/path/to/idea.sh install-plugin /path/to/plugin.zip

# 或者在 Linux 上使用
idea install-plugin /path/to/plugin.zip
```

---

## 管理已安装的插件

### 启用/禁用插件

1. 打开 **Settings/Preferences**

2. 选择 **Plugins**

3. 切换到 **Installed** 标签

4. 勾选或取消勾选插件复选框

5. 点击 **OK**

### 更新插件

**自动更新：**

1. 打开 **Settings/Preferences**

2. 选择 **Plugins**

3. 切换到 **Installed** 标签

4. 点击 **Update** 按钮（如果有可用更新）

**手动更新：**

1. 下载新版本插件

2. 按照安装步骤进行安装

3. 替换旧版本

### 卸载插件

1. 打开 **Settings/Preferences**

2. 选择 **Plugins**

3. 切换到 **Installed** 标签

4. 右键点击插件

5. 选择 **Uninstall**

---

## 插件兼容性

### IDE 版本兼容性

插件通常会声明兼容的 IDE 版本范围：

```xml
<!-- plugin.xml -->
<idea-version since-build="233" until-build="241.*"/>
```

| 版本字段 | 说明 |
|---------|------|
| `since-build` | 最低支持的 IDE 版本 |
| `until-build` | 最高兼容的 IDE 版本（`*` 表示包含所有小版本） |

### 检查兼容性

在安装插件前，检查：

1. **IDE 版本**：`Help` → `About` 查看版本号
2. **插件要求**：查看 Marketplace 页面
3. **构建号**：确保在 `since-build` 和 `until-build` 范围内

---

## 企业插件仓库

### 配置自定义仓库

1. 打开 **Settings/Preferences**

2. 选择 **Plugins**

3. 点击 ⚙️ 图标

4. 选择 **Manage Plugin Repositories...**

5. 点击 **+** 添加仓库

6. 输入仓库 URL

### 企业仓库示例

```
https://plugins.example.com/
https://internal.company.com/plugins/
```

---

## 常见问题

### 插件安装失败

**问题：** 安装插件时出现错误

**解决方案：**
1. 检查网络连接
2. 尝试使用 VPN
3. 下载插件后手动安装
4. 检查 IDE 版本兼容性

### 插件冲突

**问题：** 安装插件后 IDE 出现问题

**解决方案：**
1. 禁用最近安装的插件
2. 使用安全模式启动：`idea.sh --no-plugins`
3. 查看日志文件：`Help` → `Show Log in Explorer`

### 插件性能问题

**问题：** 插件导致 IDE 变慢

**解决方案：**
1. 禁用不必要的插件
2. 查看插件资源使用情况
3. 更新插件到最新版本

---

## 相关资源

- [JetBrains Marketplace](https://plugins.jetbrains.com/)
- [插件开发文档](https://plugins.jetbrains.com/docs/intellij/welcome.html)
- [IDE 版本历史](https://www.jetbrains.com/idea/whatsnew/)
- [插件验证指南](https://plugins.jetbrains.com/docs/intellij/plugin_verification.html)
