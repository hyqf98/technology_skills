# Electron - 开发指南

**页数:** 22

---

## 在 C++ 代码上使用 clang-tidy

**URL:** https://www.electronjs.org/docs/latest/development/clang-tidy

**内容:**
- 在 C++ 代码上使用 clang-tidy

clang-tidy 是一个自动检查 C/C++/Objective-C 代码的风格违规、编程错误和最佳实践的工具。

Electron 的 clang-tidy 集成作为一个 linter 脚本提供，可以通过 `npm run lint:clang-tidy` 运行。虽然 clang-tidy 检查磁盘上的文件，但您需要先构建 Electron，以便它知道使用了哪些编译器标志。该脚本有一个必需选项 `--output-dir`，用于告诉脚本从哪个构建目录提取编译信息。典型用法是：`npm run lint:clang-tidy --out-dir ../out/Testing`

**技术说明:**
- clang-tidy 使用 Chromium 的 `.clang-tidy` 配置
- 可以通过 `--checks=` 选项自定义检查项
- 支持通配符，可以通过 `-` 前缀禁用某些检查
- 由于内存消耗大，默认作业数为 1

---

## 拉取请求 (Pull Requests)

**URL:** https://www.electronjs.org/docs/latest/development/pull-requests

**内容:**
- 拉取请求
- 设置本地环境
  - 步骤 1: Fork
  - 步骤 2: 构建
  - 步骤 3: 分支
- 进行更改
  - 步骤 4: 编码
  - 步骤 5: 提交
    - 提交签名
    - 提交消息指南

**工作流程:**
1. 在 GitHub 上 Fork 项目并本地克隆您的 fork
2. 根据操作系统构建项目（参见构建指南）
3. 从 main 分支创建本地分支
4. 进行代码更改并运行 `npm run lint` 确保代码风格正确
5. 提交更改（需要签名提交）
6. 使用 `git rebase` 同步主仓库
7. 推送到您的 fork 并创建 PR

**提交消息规范:**
- 使用语义化提交消息（feat:, fix:, chore: 等）
- 如果有破坏性更改，在消息中包含 `BREAKING CHANGE:`

---

## 编码风格

**URL:** https://www.electronjs.org/docs/latest/development/coding-style

**内容:**
- 编码风格
- 通用代码
- C++ 和 Python
- 文档
- JavaScript
- 命名规范

**规范说明:**
- C++ 和 Python 遵循 Chromium 编码风格
- Python 版本：3.9
- JavaScript 遵循 Node.js 大小写约定
- 优先使用 getters/setters 而非 jQuery 风格的单函数
- 运行 `npm run lint` 检查代码风格
- 运行 `npm run lint:docs` 检查文档格式

---

## 调试

**URL:** https://www.electronjs.org/docs/latest/development/debugging

**内容:**
- Electron 调试
- 通用调试
- 打印堆栈跟踪
- 断点调试
- 平台特定调试
- 使用符号服务器调试

**调试技术:**
- 使用 Chromium 的日志宏（INFO, WARN, ERROR）
- 使用 `base::debug::StackTrace()` 打印堆栈
- 构建调试版本（`symbol_level = 2`）
- 使用 LLDB（macOS）或 Visual Studio（Windows）
- 配置符号服务器以获得更好的调试体验

---

## 构建说明

**URL:** https://www.electronjs.org/docs/latest/development/build-instructions-gn

**内容:**
- 构建说明
- 平台先决条件
- 构建工具
- GN 文件
- GN 先决条件
  - 设置 git 缓存
- 获取代码
  - 关于拉取/推送的说明
- 构建
  - 打包

**构建步骤:**
1. 安装 depot_tools
2. 设置环境变量（Windows: `DEPOT_TOOLS_WIN_TOOLCHAIN=0`）
3. 克隆仓库：`gclient config --name "src/electron" --unmanaged https://github.com/electron/electron`
4. 同步依赖：`gclient sync`
5. 生成构建配置：`gn gen out/Testing --args="import(\"//electron/build/args/testing.gn\")"`
6. 编译：`ninja -C out/Testing electron`

---

## 测试

**URL:** https://www.electronjs.org/docs/latest/development/testing

**内容:**
- 测试
- Lint 检查
- 单元测试
- Node.js 烟雾测试
  - 在 Windows 10 设备上测试
    - 运行单元测试的额外步骤
    - 缺失字体
    - 像素测量

**测试命令:**
```bash
# 运行 lint
npm run lint

# 运行所有测试
npm run test

# 运行特定测试
npm run test -- -g=PATTERN

# 运行 Node.js 测试
node script/node-spec-runner.js
```

**Windows 特定要求:**
- 安装 Visual Studio 2019
- 编译 Node 头文件
- 复制 electron.lib 为 node.lib

---

## Electron 中的补丁

**URL:** https://www.electronjs.org/docs/latest/development/patches

**内容:**
- Electron 中的补丁
- 补丁理由
- 补丁系统
  - 使用方法
    - 添加新补丁
    - 编辑现有补丁
    - 删除补丁
    - 解决冲突

**补丁管理:**
- 所有补丁位于 `patches/` 目录
- 使用 `git-import-patches` 和 `git-export-patches` 工具
- 每个补丁必须在提交消息中说明理由
- 支持 3-way 合并自动解决冲突

---

## API 历史迁移指南

**URL:** https://www.electronjs.org/docs/latest/development/api-history-migration-guide

**内容:**
- Electron API 历史迁移指南
- API 历史信息
  - 破坏性更改
  - 新增功能
- 示例

**添加 API 历史的步骤:**
1. 在 `breaking-changes.md` 中查找相关更改
2. 使用 `git blame` 找到相关的 Pull Request
3. 在 API 文档中添加 API History 块
4. 使用 `git log -L :<funcname>:<file>` 查找函数历史

---

## 源代码目录结构

**URL:** https://www.electronjs.org/docs/latest/development/source-code-directory-structure

**内容:**
- 源代码目录结构
- 源代码结构
- 其他目录结构

**主要目录:**
- `build/`: GN 构建配置
- `lib/`: JavaScript/TypeScript 源代码
- `shell/`: C++ 源代码
- `patches/`: 应用于上游依赖的补丁
- `spec/`: 测试套件

---

## 文档样式指南

**URL:** https://www.electronjs.org/docs/latest/development/style-guide

**内容:**
- Electron 文档样式指南
- 标题
- Markdown 规则
- 词语选择
- API 参考
  - 标题和描述
  - 模块方法和事件
  - 类
  - 方法及其参数
    - 标题级别

**文档规范:**
- 使用 `markdownlint` 包确保样式一致
- API 文档使用实际对象名称作为标题
- 添加单行描述作为 markdown 引用
- 方法和事件按特定格式组织
- 使用 API History 块记录 API 变更

---

## 问题追踪

**URL:** https://www.electronjs.org/docs/latest/development/issues

**内容:**
- Electron 中的问题
- 如何为问题做贡献
- 寻求一般帮助
- 提交错误报告
- 分类错误报告
- 解决错误报告

**贡献方式:**
- 报告 bug
- 提供代码修复（通过 PR）
- 审查和测试 PR
- 帮助分类问题

**Bug 报告要求:**
- 清晰的错误描述
- 可重现的测试用例
- 最小化、完整、可验证的示例

---

## 创建新的 Electron 浏览器模块

**URL:** https://www.electronjs.org/docs/latest/development/creating-api

**内容:**
- 创建新的 Electron 浏览器模块
- 将文件添加到 Electron 项目配置
- 创建 API 文档
- 设置 ObjectTemplateBuilder 和 Wrappable
- 将 Electron API 与 Node 链接
- 向 TypeScript 公开 API
  - 将 API 导出为模块
  - 向 TypeScript 公开模块

**创建步骤:**
1. 将文件名添加到 `filenames.gni`
2. 创建 `.md` 文档文件
3. 在 C++ 中实现类（继承 `gin::Wrappable`）
4. 在 `node_bindings.cc` 中注册模块
5. 创建 TypeScript 定义文件
6. 添加到 `module-list.ts`

---

## 平台特定构建指南

**URL:**
- macOS: https://www.electronjs.org/docs/latest/development/build-instructions-macos
- Windows: https://www.electronjs.org/docs/latest/development/build-instructions-windows
- Linux: https://www.electronjs.org/docs/latest/development/build-instructions-linux

**macOS 特定:**
- 需要 Xcode 和命令行工具
- 配置 `~/.lldbinit` 用于源代码映射
- Arm64 特定要求

**Windows 特定:**
- 需要 Visual Studio 2019 或更新版本
- 排除源树免受 Windows 安全监控
- 32 位构建支持

**Linux 特定:**
- 安装依赖脚本
- 交叉编译支持
- 使用系统 clang 或其他编译器

---

## 高级开发主题

**内容:**
- Chromium 开发
- V8 开发
- 使用 Xcode 调试
- 使用 WinDbg 调试
- Reclient（远程执行）
- 符号服务器设置

**资源:**
- Chromium 贡献指南
- V8 文档和资源
- LLDB 和 WinDbg 调试技术
- 远程构建执行和缓存
