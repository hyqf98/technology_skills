# Spring AI 技术文档索引

## 📚 文档结构

欢迎使用 Spring AI 技术文档！本文档提供完整的 Spring AI 使用指南，包括依赖配置、API 使用、最佳实践等内容。

## 📂 文档分类

### 🚀 快速入门

**文件:** `getting_started.md` | **页数:** 10

- **项目配置**: Maven/Gradle 依赖、仓库配置
- **API 配置**: OpenAI、Azure OpenAI 配置
- **基础 API**: ChatClient 使用、结构化输出、函数调用
- **Embedding**: 文本向量化
- **向量存储**: SimpleVectorStore 配置和使用
- **RAG 实现**: 基础 RAG、RetrievalAugmentationAdvisor
- **Prompt 模板**: 模板使用和参数化
- **错误处理**: 重试配置、异常处理
- **完整示例**: Controller、配置类
- **最佳实践**: API Key 管理、成本优化、性能优化

**适合人群**: Spring AI 初学者

---

### 💬 聊天模型 API

**文件:** `chat_models.md` | **页数:** 8

- **模型提供商**: OpenAI、Azure OpenAI、Anthropic Claude、Ollama
- **ChatClient API**: 基础对话、多轮对话、流式响应、结构化输出
- **函数调用**: 定义函数、启用函数调用、多个函数
- **多模态支持**: 图像理解、音频处理（STT/TTS）
- **高级配置**: Chat Options、动态选项、默认选项
- **记忆管理**: Chat Memory、持久化记忆、窗口记忆
- **提示工程**: Few-Shot、Chain of Thought、ReAct 模式
- **错误处理和重试**: 自动重试、异常处理
- **性能优化**: 批量处理、缓存、连接池
- **完整示例**: 智能客服

**适合人群**: 需要使用聊天模型的开发者

---

### 🔍 RAG 实现

**文件:** `rag.md` | **页数:** 6

- **RAG 基础**: 核心流程、ETL 处理
- **依赖配置**: 核心依赖、向量数据库选择
- **文档处理**: 加载文档、分割文档、文档转换
- **向量存储**: Simple/PgVector/Redis 配置
- **检索和生成**: 基础 RAG、RetrievalAugmentationAdvisor、高级检索
- **高级特性**: 查询转换、多轮对话、引用来源
- **性能优化**: 批量处理、缓存、异步处理
- **完整示例**: 知识库问答、多文档 RAG
- **监控评估**: 检索质量、生成质量评估
- **最佳实践**: 文档预处理、分块策略、检索优化

**适合人群**: 需要 RAG 能力的开发者

---

### 🗄️ 向量数据库

**文件:** `vector_databases.md` | **页数:** 6

- **向量数据库概述**: 支持的数据库列表
- **Simple Vector Store**: 依赖配置、使用方法
- **PgVector Store**: 依赖配置、PostgreSQL 集成
- **Chroma Vector Store**: 开源向量数据库
- **Redis Vector Store**: Redis 集成
- **Milvus Vector Store**: 大规模生产环境
- **向量存储操作**: 添加文档、相似度搜索、删除更新
- **高级功能**: 自定义 Embedding、批量操作、混合搜索
- **性能优化**: 索引优化、连接池配置、批量操作
- **完整示例**: 文档管理系统、智能问答系统
- **最佳实践**: 数据库选择、向量化策略、检索优化

**适合人群**: 需要向量存储的开发者

---

### 🤖 Agent 开发

**文件:** `agents.md` | **页数:** 5

- **Agent 概述**: 核心能力、基础概念
- **基础 Agent**: 简单 Agent、带工具的 Agent
- **工作流模式**:
  - 链式工作流：顺序执行
  - 并行工作流：并行处理
  - 路由工作流：智能分配
  - 编排器-工作器：任务分解
- **高级 Agent**: 带记忆的 Agent、自主 Agent、多 Agent 协作
- **Agent 配置**: ChatClient 配置、工具注册
- **最佳实践**: 任务分解、工具设计、状态管理、错误处理
- **完整示例**:
  - 智能客服 Agent
  - 研究助手 Agent
- **监控调试**: 执行追踪、性能监控

**适合人群**: 需要构建 AI Agent 的开发者

---

## 🚀 快速导航

### 按学习路径

#### 1️⃣ 入门阶段（1周）
1. 阅读 `getting_started.md` - 了解基础配置
2. 运行第一个 ChatClient 示例
3. 学习简单的函数调用
4. 实现基础 RAG

#### 2️⃣ 进阶阶段（2-3周）
1. 学习 `chat_models.md` - 掌握聊天模型
2. 实现 `rag.md` - 完整的 RAG 系统
3. 学习向量数据库 - `vector_databases.md`
4. 实现智能问答系统

#### 3️⃣ 高级阶段（1-2月）
1. 学习 `agents.md` - 构建 AI Agent
2. 实现多 Agent 协作
3. 优化性能和成本
4. 部署生产环境

### 按功能分类

#### 核心功能
- **快速入门**: `getting_started.md` - 开始使用
- **聊天模型**: `chat_models.md` - 对话功能
- **RAG**: `rag.md` - 检索增强生成
- **向量数据库**: `vector_databases.md` - 向量存储

#### 高级功能
- **Agent**: `agents.md` - AI 代理
- **函数调用**: `chat_models.md` - 第3节
- **记忆管理**: `chat_models.md` - 第6节
- **工作流**: `agents.md` - 第3节

---

## 💡 使用建议

### 1. 依赖管理

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-bom</artifactId>
            <version>1.0.0-M4</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### 2. 模型选择

| 使用场景 | 推荐模型 | 成本 |
|---------|---------|------|
| 简单对话 | gpt-3.5-turbo | 低 |
| 复杂推理 | gpt-4o | 高 |
| 代码生成 | gpt-4o | 高 |
| 本地部署 | llama3.2 (Ollama) | 免费 |

### 3. 向量数据库选择

| 数据规模 | 推荐数据库 |
|---------|-----------|
| < 10K | Simple Vector Store |
| 10K-1M | PgVector, Chroma |
| > 1M | Milvus, Qdrant |

---

## 🔗 相关资源

- **官方文档**: https://docs.spring.io/spring-ai/reference/
- **GitHub**: https://github.com/spring-projects/spring-ai
- **示例代码**: https://github.com/spring-projects/spring-ai-examples
- **API 文档**: https://docs.spring.io/spring-ai/api/

---

## 📝 更新日志

**最后更新**: 2025-01-18

### 最近更新
- ✅ 重写了 `getting_started.md` - 聚焦 API 使用
- ✅ 重写了 `chat_models.md` - 精简内容，增加示例
- ✅ 重写了 `rag.md` - 完整的 RAG 实现指南
- ✅ 重写了 `vector_databases.md` - 多种向量数据库集成
- ✅ 重写了 `agents.md` - Agent 开发完整指南

---

**祝开发愉快！🎉**

© 2025 Spring AI 技术文档
