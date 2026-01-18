# JetBrains 插件开发文档索引

## 📚 文档结构

欢迎使用 IntelliJ Platform 插件开发技术文档！本文档基于最新的 IntelliJ Platform SDK 编写，提供了完整的插件开发说明、代码示例和最佳实践。

## 📂 文档分类

### 🚀 快速入门

**文件:** `getting_started.md` | **页数:** 15

- **简介**: IntelliJ Platform 概述、插件概念、核心功能
- **环境准备**: JDK 配置、IDE 安装、开发环境设置
- **创建项目**: 使用模板、项目向导、项目结构详解
- **构建配置**: Gradle 配置、plugin.xml 详解、依赖管理
- **快速开始**: 创建第一个 Action、运行插件、调试技巧
- **构建分发**: 打包插件、本地测试、发布到 Marketplace
- **最佳实践**: 代码组织、命名规范、性能优化、安全建议

**适合人群**: IntelliJ 插件开发初学者、需要快速上手的开发者

---

### 🔧 核心概念

**文件:** `core_concepts.md` | **页数:** 20

- **扩展点（Extension Points）**: 定义和实现扩展点、接口和 Bean 扩展点
- **扩展（Extensions）**: 查看可用扩展点、注册扩展、扩展作用域
- **动作（Actions）**: 动作系统、基础动作、切换动作、动作组、注册动作
- **服务（Services）**: 应用服务、项目服务、持久化状态服务
- **工具窗口（Tool Windows）**: 创建工具窗口、工具窗口属性、自定义内容
- **监听器（Listeners）**: 项目监听器、编辑器监听器、文件监听器
- **数据上下文（Data Context）**: DataKeys 定义和使用、自定义数据提供
- **PSI**: PSI 概念、PSI 层次结构、Java PSI 使用示例
- **通知（Notifications）**: 显示通知、通知类型、通知组
- **后台任务（Background Tasks）**: 后台任务执行、进度指示器、任务取消

**适合人群**: 需要深入理解 IntelliJ Platform 架构的开发者

---

### 📦 插件管理

**文件:** `plugin_management.md` | **页数:** 1

- **插件安装**: 从 Marketplace 安装、从磁盘安装、命令行安装
- **插件管理**: 启用/禁用插件、更新插件、卸载插件
- **插件兼容性**: IDE 版本兼容性、检查兼容性方法
- **企业仓库**: 配置自定义仓库、企业插件管理
- **常见问题**: 安装失败、插件冲突、性能问题

**适合人群**: 插件用户、需要管理插件的开发者

---

### 🎨 UI 开发（待补充）

**文件:** `ui_development.md` | **页数:** 待定

**计划内容**:
- **Swing 基础**: IntelliJ UI 组件、布局管理器
- **工具窗口**: 自定义工具窗口、工具窗口工厂
- **编辑器扩展**: 编辑器注入、内联提示、代码折叠
- **菜单和工具栏**: 自定义菜单、工具栏按钮、快捷键
- **对话框和向导**: 创建对话框、向导页面、表单验证
- **弹出菜单**: 上下文菜单、动态菜单、菜单分隔符
- **状态栏**: 自定义状态栏小部件、状态更新

**适合人群**: 需要自定义 UI 的插件开发者

---

### 🔍 高级主题（待补充）

**文件:** `advanced_topics.md` | **页数:** 待定

**计划内容**:
- **PSI 深入**: PSI 元素遍历、PSI 修改、PSI 事件
- **代码检查**: 自定义检查、本地检查、快速修复
- **意图动作（Intentions）**: 意图动作注册、意图处理器
- **实时模板**: Live Template 配置、模板变量、模板上下文
- **代码生成**: 生成器实现、代码生成选项
- **索引系统**: 自定义索引、索引查询、文件索引
- **VFS（虚拟文件系统）**: VFS 监听、文件操作、刷新机制
- **文档服务**: 文本范围、高亮、导航

**适合人群**: 需要高级功能的开发者

---

### 🧪 测试和调试（待补充）

**文件:** `testing_debugging.md` | **页数:** 待定

**计划内容**:
- **单元测试**: JUnit 测试、测试规则、Mock 对象
- **UI 测试**: Swing 测试、机器人测试
- **集成测试**: 插件测试框架、功能测试
- **调试技巧**: 断点调试、日志调试、性能分析
- **测试覆盖率**: JaCoCo 配置、覆盖率报告
- **持续集成**: GitHub Actions、TeamCity 配置

**适合人群**: 需要保证插件质量的开发者

---

### 📦 打包和发布（待补充）

**文件:** `packaging_publishing.md` | **页数:** 待定

**计划内容**:
- **插件打包**: Gradle 构建任务、签名插件、验证插件
- **发布流程**: JetBrains Marketplace 上传、版本管理、更新日志
- **持续集成/持续部署**: 自动化构建、自动发布
- **插件验证**: 验证工具、兼容性检查
- **版本管理**: 语义化版本、版本兼容性
- **许可证管理**: 商业插件、许可证验证

**适合人群**: 需要发布插件的开发者

---

### 🔌 扩展开发（待补充）

**文件:** `extensions.md` | **页数:** 待定

**计划内容**:
- **语言支持**: 语法高亮、代码补全、代码格式化
- **框架集成**: Spring、Vue、React 集成
- **工具集成**: Git、Docker、Kubernetes 集成
- **主题开发**: 自定义主题、颜色方案、图标主题

**适合人群**: 需要开发特定功能扩展的开发者

---

## 🚀 快速导航

### 按学习路径

#### 1️⃣ 入门阶段（1-2周）
1. 阅读 [getting_started.md](./getting_started.md) - 了解基本概念
2. 跟随快速开始示例 - 创建第一个插件
3. 学习 Action 基础 - 创建简单的菜单操作
4. 掌握调试技巧 - 运行和调试插件

#### 2️⃣ 进阶阶段（3-5周）
1. 学习核心概念 - 扩展点、服务、监听器
2. 掌握工具窗口 - 创建自定义 UI 面板
3. 了解 PSI - 操作代码结构
4. 学习数据持久化 - 保存插件设置

#### 3️⃣ 高级阶段（1-2月）
1. 深入 PSI - 高级代码操作
2. 学习代码检查 - 自定义检查和修复
3. 掌握索引系统 - 提高查询性能
4. 了解多平台支持 - 兼容多个 JetBrains IDE

#### 4️⃣ 专家阶段（持续学习）
1. 性能优化 - 插件性能调优
2. UI/UX 设计 - 用户体验优化
3. 测试策略 - 全面测试覆盖
4. 发布维护 - 版本管理和用户支持

### 按功能分类

#### 基础功能
- **快速入门**: [getting_started.md](./getting_started.md) - 开始开发
- **核心概念**: [core_concepts.md](./core_concepts.md) - 理解架构
- **插件管理**: [plugin_management.md](./plugin_management.md) - 管理插件

#### UI 开发
- **工具窗口**: [core_concepts.md](./core_concepts.md#5-工具窗口tool-windows) - 自定义面板
- **动作系统**: [core_concepts.md](./core_concepts.md#3-动作actions) - 用户交互
- **通知系统**: [core_concepts.md](./core_concepts.md#9-通知notifications) - 用户反馈

#### 代码操作
- **PSI 基础**: [core_concepts.md](./core_concepts.md#8-psiprogram-structure-interface) - 代码结构
- **服务系统**: [core_concepts.md](./core_concepts.md#4-服务services) - 后台逻辑
- **监听器**: [core_concepts.md](./core_concepts.md#6-监听器listeners) - 响应事件

---

## 📖 版本说明

本文档基于 **IntelliJ Platform 2023.3** 版本编写。

### 支持的 IDE

- IntelliJ IDEA Ultimate 2023.3+
- IntelliJ IDEA Community 2023.3+
- 其他 JetBrains IDE（PyCharm、WebStorm 等）

### 构建工具

- Gradle 8.0+
- IntelliJ Platform Gradle Plugin 1.17.0+
- JDK 17+

---

## 💡 使用建议

### 1. 开发环境

- **推荐 IDE**: IntelliJ IDEA Ultimate
- **JDK**: JetBrains Runtime 17
- **构建工具**: Gradle (Kotlin DSL)
- **版本控制**: Git

### 2. 学习路径建议

- **初学者**: 从 [getting_started.md](./getting_started.md) 开始，逐步学习
- **有经验开发者**: 重点阅读 [core_concepts.md](./core_concepts.md)
- **高级开发者**: 深入研究高级主题和扩展开发

### 3. 实战建议

1. **从小插件开始**: 先开发简单的功能插件
2. **阅读源码**: 学习官方和开源插件的实现
3. **参与社区**: 在 GitHub 上提交 Issue 和 PR
4. **发布插件**: 将插件发布到 Marketplace

---

## 🔗 相关资源

### 官方资源
- **IntelliJ Platform Plugin SDK**: https://plugins.jetbrains.com/docs/intellij/welcome.html
- **JetBrains Marketplace**: https://plugins.jetbrains.com/
- **IntelliJ Platform GitHub**: https://github.com/JetBrains/intellij-community

### 社区资源
- **IntelliJ Platform DevOps**: https://plugins.jetbrains.com/docs/intellij/tools-intellij-platform-gradle-plugin.html
- **Plugin Template**: https://github.com/JetBrains/intellij-platform-plugin-template
- **Awesome IntelliJ Plugins**: https://github.com/tisonkun/awesome-intellij-plugins

### 开发工具
- **IntelliJ Platform Gradle Plugin**: 构建和打包插件
- **Plugin Verifier**: 验证插件兼容性
- **IDE DevKit**: 开发插件支持

---

## ❓ 常见问题

### Q: 需要购买 IntelliJ IDEA Ultimate 吗？

A: 基础插件开发可以使用 Community Edition，但 Ultimate Edition 提供更多调试和验证功能。

### Q: 插件支持哪些 JetBrains IDE？

A: 通过配置 `depends` 和 `optional` 依赖，可以支持多个 IDE。

### Q: 如何学习 PSI？

A: PSI 是一个复杂的系统，建议从简单的 PSI 操作开始，逐步深入学习。

### Q: 插件可以商业化吗？

A: 可以。你可以出售插件或提供付费服务。

---

## 📝 更新日志

本文档最后更新时间：2025-01-18

### 最近更新
- ✅ 添加了快速入门指南
- ✅ 补充了核心概念文档
- ✅ 完善了项目结构和配置说明
- ✅ 添加了最佳实践和常见问题

---

## 🤝 贡献指南

欢迎参与文档改进：

1. Fork 本项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

---

## 📄 许可证

本文档采用 MIT 许可证

---

## 📮 联系方式

如有问题或建议，请通过以下方式联系：

- **GitHub Issues**: https://github.com/your-repo/issues
- **JetBrains Plugin Forum**: https://plugins.jetbrains.com/docs/intellij/forum.html

---

**祝您开发愉快！🎉**

© 2025 JetBrains Plugin Development Guide

## 文档结构

```
jetbrains-plugins/
├── SKILL.md              # 主技能文档
├── references/           # 参考文档目录
│   ├── index.md         # 本文件
│   └── plugin_management.md  # 插件管理文档
├── scripts/             # 辅助脚本
└── assets/              # 资源文件
```

## 快速链接

- [IntelliJ Platform Plugin SDK](https://plugins.jetbrains.com/docs/intellij/welcome.html)
- [JetBrains Marketplace](https://plugins.jetbrains.com/)
- [插件开发教程](https://plugins.jetbrains.com/docs/intellij/getting_started.html)
- [API 参考文档](https://javadoc.io/doc/org.jetbrains/intellij-community/intellij-platform/latest/index.html)
