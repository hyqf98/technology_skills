---
name: springai
description: Spring AI 框架 - 用于在 Spring Boot 中构建 AI 驱动的应用程序。使用此技能进行 Spring AI 开发，集成 AI 模型（OpenAI、Anthropic、Ollama 等），实现 RAG 模式、向量数据库、嵌入模型、聊天模型和 AI 代理。
---

# Spring AI 技能文档

基于官方文档生成的 Spring AI 开发综合指南。

## 何时使用此技能

在以下场景中使用此技能：
- 开发 Spring AI 应用程序
- 查询 Spring AI 功能或 API
- 实现 Spring AI 解决方案
- 调试 Spring AI 代码
- 学习 Spring AI 最佳实践

## 技术概述

**Spring AI** 是 Spring 生态系统中的 AI 工程应用框架，它为 AI 应用开发提供了 Spring 友好的 API 和抽象层。该项目于 2025 年 5 月发布 1.0 正式版，旨在简化 AI 应用的开发流程。

### 核心特性

- **多模型支持**：支持 OpenAI、Anthropic、Ollama、Azure OpenAI 等主流 AI 模型
- **RAG 架构**：内置检索增强生成（RAG）支持，模块化架构支持自定义 RAG 流程
- **向量数据库集成**：支持 Apache Cassandra、PostgreSQL/PGVector、MongoDB Atlas、Redis、Weaviate、Chroma、Milvus 等向量数据库
- **标准化接口**：统一的检索、生成和提示管理接口
- **Spring 原生集成**：与 Spring Boot 无缝集成，支持自动配置

### 版本说明

- **稳定版本**：1.0.0 及以上版本已在 Maven Central 提供发布
- **开发版本**：1.1.0-SNAPSHOT 等快照版本需配置额外的仓库

## 快速参考

### 常见配置模式

#### 模式 1：手动配置向量存储

不使用 Spring Boot 自动配置，而是通过构建器模式手动配置 Weaviate 向量存储：

```java
@Bean
public WeaviateClient weaviateClient() {
    return new WeaviateClient(new Config("http", "localhost:8080"));
}

@Bean
public VectorStore vectorStore(WeaviateClient weaviateClient, EmbeddingModel embeddingModel) {
    return WeaviateVectorStore.builder(weaviateClient, embeddingModel)
        .options(options)                           // 可选：自定义选项
        .consistencyLevel(ConsistentLevel.QUORUM)   // 可选：默认为 ConsistentLevel.ONE
        .filterMetadataFields(List.of(              // 可选：可用于过滤的字段
            MetadataField.text("country"),
            MetadataField.number("year")))
        .build();
}
```

#### 模式 2：运行时选项覆盖

在启动时使用 `spring.ai.openai.audio.speech` 配置，但可以在运行时覆盖这些选项：

```java
OpenAiAudioSpeechOptions speechOptions = OpenAiAudioSpeechOptions.builder()
    .model("gpt-4o-mini-tts")
    .voice(OpenAiAudioApi.SpeechRequest.Voice.ALLOY)
    .responseFormat(OpenAiAudioApi.SpeechRequest.AudioResponseFormat.MP3)
    .speed(1.0)
    .build();

TextToSpeechPrompt speechPrompt = new TextToSpeechPrompt(
    "Hello, this is a text-to-speech example.",
    speechOptions
);

TextToSpeechResponse response = openAiAudioSpeechModel.call(speechPrompt);
```

#### 模式 3：内置递归顾问（Advisors）

Spring AI 提供两个内置递归顾问：

**ToolCallAdvisor** - 工具调用循环
- 通过设置 `setInternalToolExecutionEnabled(false)` 禁用模型的内部工具执行
- 支持工具的"返回直接"功能 - 当工具执行设置 `returnDirect=true` 时，直接返回结果到客户端
- 使用 `callAdvisorChain.copy(this)` 创建递归调用的子链

```java
var toolCallAdvisor = ToolCallAdvisor.builder()
    .toolCallingManager(toolCallingManager)
    .advisorOrder(BaseAdvisor.HIGHEST_PRECEDENCE + 300)
    .build();

var chatClient = ChatClient.builder(chatModel)
    .defaultAdvisors(toolCallAdvisor)
    .build();
```

**StructuredOutputValidationAdvisor** - 结构化输出验证
- 自动从预期输出类型生成 JSON Schema
- 验证 LLM 响应是否符合 Schema
- 验证失败时重试，最多可配置次数

```java
var validationAdvisor = StructuredOutputValidationAdvisor.builder()
    .outputType(MyResponseType.class)
    .maxRepeatAttempts(3)
    .advisorOrder(BaseAdvisor.HIGHEST_PRECEDENCE + 1000)
    .build();

var chatClient = ChatClient.builder(chatModel)
    .defaultAdvisors(validationAdvisor)
    .build();
```

#### 模式 4：Neo4j 向量存储配置

```java
// 1. 添加依赖
// Maven:
// <dependency>
//     <groupId>org.springframework.ai</groupId>
//     <artifactId>spring-ai-neo4j-store</artifactId>
// </dependency>

// 2. 创建 Neo4j Driver bean
@Bean
public Driver driver() {
    return GraphDatabase.driver(
        "neo4j://<host>:<bolt-port>",
        AuthTokens.basic("<username>", "<password>")
    );
}

// 3. 创建 Neo4jVectorStore bean
@Bean
public VectorStore vectorStore(Driver driver, EmbeddingModel embeddingModel) {
    return Neo4jVectorStore.builder(driver, embeddingModel)
        .databaseName("neo4j")              // 可选：默认为 "neo4j"
        .distanceType(Neo4jDistanceType.COSINE)  // 可选：默认为 COSINE
        .embeddingDimension(1536)           // 可选：默认为 1536
        .label("Document")                  // 可选：默认为 "Document"
        .embeddingProperty("embedding")     // 可选：默认为 "embedding"
        .indexName("custom-index")          // 可选：默认为 "spring-ai-document-index"
        .initializeSchema(true)             // 可选：默认为 false
        .batchingStrategy(new TokenCountBatchingStrategy())  // 可选
        .build();
}
```

#### 模式 5：图像生成运行时选项

```java
ImageResponse response = azureOpenaiImageModel.call(
    new ImagePrompt(
        "A light cream colored mini golden doodle",
        OpenAiImageOptions.builder()
            .quality("hd")
            .N(4)
            .height(1024)
            .width(1024)
            .build()
    )
);
```

### 代码示例模式

#### 示例 1：添加 MCP 服务器依赖

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webmvc</artifactId>
</dependency>
```

#### 示例 2：添加 OpenAI 模型依赖

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

#### 示例 3：注入聊天记忆

```java
@Autowired
ChatMemory chatMemory;
```

#### 示例 4：配置消息窗口记忆

```java
MessageWindowChatMemory memory = MessageWindowChatMemory.builder()
    .maxMessages(10)
    .build();
```

#### 示例 5：添加 PostgresML 嵌入模型依赖

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-postgresml-embedding</artifactId>
</dependency>
```

## 依赖管理

### 使用 BOM（推荐）

Spring AI 的物料清单（BOM）声明了所有依赖的推荐版本：

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-bom</artifactId>
            <version>${spring-ai.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### Gradle 配置

```gradle
dependencies {
    implementation platform('org.springframework.ai:spring-ai-bom:${spring-ai.version}')
    // 添加具体依赖
    implementation 'org.springframework.ai:spring-ai-openai-spring-boot-starter'
}
```

### 快照版本配置

如需使用快照版本，需添加以下仓库：

**Maven:**
```xml
<repositories>
    <repository>
        <id>spring-snapshots</id>
        <name>Spring Snapshots</name>
        <url>https://repo.spring.io/snapshot</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
    <repository>
        <id>central-portal-snapshots</id>
        <url>https://central.sonatype.com/repository/maven-snapshots/</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
</repositories>
```

**Gradle:**
```gradle
repositories {
    mavenCentral()
    maven { url 'https://repo.spring.io/snapshot' }
    maven { url 'https://central.sonatype.com/repository/maven-snapshots/' }
}
```

## 支持的功能模块

### AI 模型集成
- **聊天模型**：OpenAI、Anthropic、Azure OpenAI、Ollama 等
- **嵌入模型**：OpenAI、Azure OpenAI、PostgresML、Transformers（ONNX）
- **图像模型**：OpenAI DALL-E、Azure OpenAI
- **音频模型**：文本转语音（TTS）、语音转文本

### 向量数据库
- Apache Cassandra、Azure Vector Search
- Chroma、Milvus、MongoDB Atlas
- PostgreSQL (PGVector)、Redis、Weaviate
- Neo4j、Oracle AI Vector Search

### RAG（检索增强生成）
- 模块化 RAG 架构
- 自定义文档加载器
- 文档分割器
- 嵌入存储与检索
- 内置 Advisor 流程

### AI 功能
- **AI 代理（Agents）**：工具调用、函数执行
- **内容审核**：内容安全检查
- **可观测性**：追踪、监控、日志
- **提示工程**：提示模板与管理

## 参考文档

此技能在 `references/` 目录中包含全面的文档：

| 文档 | 描述 |
|------|------|
| **agents.md** | AI 代理文档 |
| **api.md** | API 参考 |
| **audio.md** | 音频模型文档 |
| **chat_models.md** | 聊天模型文档 |
| **embeddings.md** | 嵌入模型文档 |
| **getting_started.md** | 入门指南（17 页） |
| **image_models.md** | 图像模型文档 |
| **mcp.md** | MCP 协议文档 |
| **moderation.md** | 内容审核文档 |
| **observability.md** | 可观测性文档 |
| **rag.md** | RAG 模式文档 |
| **vector_databases.md** | 向量数据库文档 |

## 使用指南

### 对于初学者
1. 从 `getting_started.md` 开始了解基础概念
2. 通过 Spring Initializr 创建新项目
3. 添加 BOM 和所需依赖
4. 配置 AI 模型 API 密钥
5. 运行第一个示例

### 对于特定功能
- 查找对应类别的参考文档（api、rag 等）
- 查看代码示例和配置选项
- 参考官方文档链接获取详细信息

### 对于代码调试
- 使用快速参考中的常见模式
- 检查依赖版本兼容性
- 验证 API 密钥配置
- 查看日志输出

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
- 参考文档保留了源文档的结构和示例
- 代码示例包含语言检测以支持语法高亮
- 快速参考模式提取自文档中的常见用法示例

## 更新说明

刷新此技能的文档：
1. 使用相同配置重新运行爬虫
2. 技能将使用最新信息重建

## 最新特性 (2025)

### Function Calling (函数调用)

**实现天气查询函数调用:**
```java
@SpringBootApplication
public class FunctionCallingExample {

    public static void main(String[] args) {
        SpringApplication.run(FunctionCallingExample.class, args);
    }

    @Bean
    CommandLineRunner runner(ChatClient.Builder chatClientBuilder) {
        return args -> {
            var chatClient = chatClientBuilder.build();

            var response = chatClient.prompt()
                .user("What is the weather in Amsterdam and Paris?")
                .functions("weatherFunction")  // 引用函数名称
                .call()
                .content();

            System.out.println(response);
        };
    }

    @Bean
    @Description("Get the weather in location")
    public Function<WeatherRequest, WeatherResponse> weatherFunction() {
        return new MockWeatherService();
    }

    public static class MockWeatherService implements Function<WeatherRequest, WeatherResponse> {

        public record WeatherRequest(String location, String unit) {}
        public record WeatherResponse(double temp, String unit) {}

        @Override
        public WeatherResponse apply(WeatherRequest request) {
            double temperature = request.location().contains("Amsterdam") ? 20 : 25;
            return new WeatherResponse(temperature, request.unit());
        }
    }
}
```

### 对话式 RAG (Conversational RAG)

**结合记忆和 RAG:**
```java
import org.springframework.ai.rag.advisor.RetrievalAugmentationAdvisor;
import org.springframework.ai.rag.preretrieval.query.transformation.CompressionQueryTransformer;
import org.springframework.ai.rag.retrieval.search.VectorStoreDocumentRetriever;
import org.springframework.ai.chat.memory.ChatMemory;
import org.springframework.ai.chat.memory.MessageWindowChatMemory;
import org.springframework.ai.chat.memory.MessageChatMemoryAdvisor;

@Service
public class ConversationalRAGService {
    private final ChatClient chatClient;

    public ConversationalRAGService(ChatModel chatModel, VectorStore vectorStore) {
        // 结合记忆和 RAG
        ChatMemory memory = MessageWindowChatMemory.builder().build();

        MessageChatMemoryAdvisor memoryAdvisor = MessageChatMemoryAdvisor
            .builder(memory)
            .build();

        RetrievalAugmentationAdvisor ragAdvisor = RetrievalAugmentationAdvisor.builder()
            // 使用对话历史压缩查询
            .queryTransformers(
                CompressionQueryTransformer.builder()
                    .chatClientBuilder(ChatClient.builder(chatModel))
                    .build()
            )
            .documentRetriever(
                VectorStoreDocumentRetriever.builder()
                    .vectorStore(vectorStore)
                    .build()
            )
            .build();

        this.chatClient = ChatClient.builder(chatModel)
            .defaultAdvisors(memoryAdvisor, ragAdvisor)
            .build();
    }

    public String chat(String conversationId, String message) {
        // 结合记忆上下文和 RAG 知识
        return chatClient.prompt()
            .user(message)
            .advisors(a -> a.param(ChatMemory.CONVERSATION_ID, conversationId))
            .call()
            .content();
    }
}
```

### 综合配置 (YAML)

**完整的 Spring AI 配置示例:**
```yaml
spring:
  ai:
    # OpenAI 配置
    openai:
      api-key: ${OPENAI_API_KEY}
      chat:
        options:
          model: gpt-4o
          temperature: 0.7
          max-tokens: 2048
      embedding:
        options:
          model: text-embedding-3-small

    # Anthropic (Claude) 配置
    anthropic:
      api-key: ${ANTHROPIC_API_KEY}
      chat:
        options:
          model: claude-3-5-sonnet-20241022
          temperature: 0.7
          max-tokens: 4096

    # Azure OpenAI 配置
    azure:
      openai:
        api-key: ${AZURE_OPENAI_API_KEY}
        endpoint: ${AZURE_OPENAI_ENDPOINT}
        chat:
          options:
            deployment-name: gpt-4o
            temperature: 0.7

    # Ollama 本地模型配置
    ollama:
      base-url: http://localhost:11434
      chat:
        options:
          model: llama3.2
          temperature: 0.8

    # Mistral AI 配置
    mistralai:
      api-key: ${MISTRAL_API_KEY}
      chat:
        options:
          model: mistral-large-latest
          temperature: 0.7

    # Google Gemini 配置
    google:
      genai:
        api-key: ${GOOGLE_API_KEY}
        chat:
          options:
            model: gemini-1.5-pro
            temperature: 0.7

    # 向量存储配置 (PostgreSQL PGVector)
    vectorstore:
      pgvector:
        dimensions: 1536
        distance-type: COSINE_DISTANCE
        schema-name: public
        table-name: vector_store
        index-type: HNSW

      # MongoDB Atlas 向量存储
      mongodb:
        database-name: vector_db
        collection-name: vector_store
        path-name: embedding
        index-name: vector_index

      # Elasticsearch 向量存储
      elasticsearch:
        url: http://localhost:9200
        index-name: spring-ai-vectors
        dimensions: 1536

    # ChatClient 配置
    chat:
      client:
        enabled: true
        observations:
          log-prompt: false  # 安全性: 生产环境不记录提示

# PostgreSQL 数据源配置 (用于 PGVector)
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/vectordb
    username: postgres
    password: ${DB_PASSWORD}
  jpa:
    hibernate:
      ddl-auto: update

# 可观测性配置 (Micrometer)
management:
  endpoints:
    web:
      exposure:
        include: health,metrics,prometheus
  metrics:
    export:
      prometheus:
        enabled: true
  tracing:
    sampling:
      probability: 1.0
```

### Docker Model Runner 集成

**使用 Docker 运行的本地模型:**
```properties
# application.properties
spring.ai.openai.api-key=test
spring.ai.openai.base-url=http://localhost:12434/engines
spring.ai.openai.chat.options.model=ai/gemma3:4B-F16
```

```java
@SpringBootApplication
public class DockerModelRunnerApplication {

    public static void main(String[] args) {
        SpringApplication.run(DockerModelRunnerApplication.class, args);
    }

    @Bean
    CommandLineRunner runner(ChatClient.Builder chatClientBuilder) {
        return args -> {
            var chatClient = chatClientBuilder.build();

            var response = chatClient.prompt()
                .user("What is the weather in Amsterdam and Paris?")
                .functions("weatherFunction")
                .call()
                .content();

            System.out.println(response);
        };
    }

    @Bean
    @Description("Get the weather in location")
    public Function<WeatherRequest, WeatherResponse> weatherFunction() {
        return new MockWeatherService();
    }

    public static class MockWeatherService implements Function<WeatherRequest, WeatherResponse> {

        public record WeatherRequest(String location, String unit) {}
        public record WeatherResponse(double temp, String unit) {}

        @Override
        public WeatherResponse apply(WeatherRequest request) {
            double temperature = request.location().contains("Amsterdam") ? 20 : 25;
            return new WeatherResponse(temperature, request.unit());
        }
    }
}
```

### 支持的 AI 提供商 (2025)

| 提供商 | 模型类型 | 支持状态 |
|--------|----------|----------|
| **OpenAI** | Chat、Embedding、Image、Audio | ✅ 完全支持 |
| **Anthropic** | Chat (Claude) | ✅ 完全支持 |
| **Azure OpenAI** | Chat、Embedding、Image | ✅ 完全支持 |
| **Ollama** | 本地模型 | ✅ 完全支持 |
| **Mistral AI** | Chat | ✅ 完全支持 |
| **Google Gemini** | Chat、Embedding | ✅ 完全支持 |
| **DeepSeek** | Chat | ✅ 完全支持 |
| **PostgresML** | Embedding | ✅ 完全支持 |

### 支持的向量数据库 (2025)

| 向量数据库 | 支持状态 | 特性 |
|-----------|----------|------|
| **PostgreSQL PGVector** | ✅ | 开源、ACID 兼容 |
| **MongoDB Atlas** | ✅ | 完全托管、可扩展 |
| **Elasticsearch** | ✅ | 全文搜索 + 向量搜索 |
| **Azure Cosmos DB** | ✅ | 全球分布、低延迟 |
| **Chroma** | ✅ | 轻量级、易于使用 |
| **Redis** | ✅ | 高性能、内存优先 |
| **Weaviate** | ✅ | GraphQL API |
| **Milvus** | ✅ | 高性能分布式 |
| **Neo4j** | ✅ | 图数据库 + 向量 |
| **Apache Cassandra** | ✅ | 分布式 NoSQL |

## 相关资源

- [Spring AI 官方网站](https://spring.io/projects/spring-ai)
- [Spring AI 参考文档](https://docs.spring.io/spring-ai/reference/index.html)
- [GitHub 仓库](https://github.com/spring-projects/spring-ai)
- [Spring AI 1.0 发布说明](https://www.infoq.com/news/2025/05/spring-ai-1-0-streamlines-apps/)
