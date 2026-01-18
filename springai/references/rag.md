# Spring AI RAG 实现指南

**页数:** 6

---

## 1. RAG 基础概念

RAG（检索增强生成）通过检索相关文档来增强AI生成能力。

**核心流程：**
1. **文档加载** - 从各种来源加载文档
2. **文档分割** - 将长文档分割成小块
3. **向量化** - 将文本转换为向量
4. **存储** - 将向量存储到向量数据库
5. **检索** - 根据查询检索相关文档
6. **生成** - 基于检索结果生成回答

---

## 2. 依赖配置

### 2.1 核心依赖

```xml
<dependencies>
    <!-- Spring AI OpenAI -->
    <dependency>
        <groupId>org.springframework.ai</groupId>
        <artifactId>spring-ai-openai-spring-boot-starter</artifactId>
    </dependency>

    <!-- 向量数据库（以Simple Vector Store为例）-->
    <dependency>
        <groupId>org.springframework.ai</groupId>
        <artifactId>spring-ai-simple-vector-store</artifactId>
    </dependency>

    <!-- 文档加载器 -->
    <dependency>
        <groupId>org.springframework.ai</groupId>
        <artifactId>spring-ai-pdf-document-reader</artifactId>
    </dependency>
</dependencies>
```

### 2.2 向量数据库选择

| 数据库 | 适用场景 | 依赖 |
|--------|---------|------|
| **Simple Vector Store** | 开发测试、小数据量 | spring-ai-simple-vector-store |
| **PgVector** | 已有PostgreSQL | spring-ai-pgvector-store |
| **Chroma** | 开源、易部署 | spring-ai-chroma-store |
| **Milvus** | 大规模生产 | spring-ai-milvus-store |
| **Redis** | 已有Redis | spring-ai-redis-store |

---

## 3. 文档处理（ETL）

### 3.1 加载文档

**文本文件**

```java
@Service
public class DocumentLoader {

    private final DocumentReader documentReader;

    public List<Document> loadTextFiles(String directory) {
        return new TextReader().read(
            new FileSystemResource(directory)
        );
    }
}
```

**PDF文件**

```java
public List<Document> loadPdfFiles(String pdfPath) {
    PagePdfDocumentReader reader = new PagePdfDocumentReader(
        new FileSystemResource(pdfPath)
    );
    return reader.get();
}
```

**多个文档**

```java
public List<Document> loadMultipleDocuments(List<String> paths) {
    List<Document> allDocuments = new ArrayList<>();

    for (String path : paths) {
        if (path.endsWith(".pdf")) {
            allDocuments.addAll(loadPdfFiles(path));
        } else if (path.endsWith(".txt")) {
            allDocuments.addAll(loadTextFiles(path));
        }
    }

    return allDocuments;
}
```

### 3.2 分割文档

**按字符分割**

```java
@Service
public class DocumentSplitter {

    public List<Document> splitByCharacters(List<Document> documents) {
        TokenTextSplitter splitter = new TokenTextSplitter(
            500,  // chunk size
            50,   // overlap
            5,    // min chunk size to keep
            10000 // max chunk size
        );

        return splitter.apply(documents);
    }
}
```

**按固定大小分割**

```java
public List<Document> splitByFixedSized(List<Document> documents) {
    TokenTextSplitter splitter = new TokenTextSplitter(
        1000,  // 1000 tokens per chunk
        100    // 100 tokens overlap
    );

    return splitter.apply(documents);
}
```

### 3.3 文档转换

**添加元数据**

```java
public List<Document> addMetadata(List<Document> documents) {
    documents.forEach(doc -> {
        Map<String, Object> metadata = doc.getMetadata();
        metadata.put("source", "knowledge-base");
        metadata.put("timestamp", Instant.now());
        metadata.put("author", "AI Assistant");
    });

    return documents;
}
```

**清理文本**

```java
public List<Document> cleanText(List<Document> documents) {
    documents.forEach(doc -> {
        String content = doc.getText()
            .replaceAll("\\s+", " ")  // 移除多余空白
            .trim();

        doc.setContent(content);
    });

    return documents;
}
```

---

## 4. 向量存储

### 4.1 Simple Vector Store

**配置**

```yaml
spring:
  ai:
    vectorstore:
      simple:
        initialize-schema: true
```

**使用**

```java
@Service
public class VectorStoreService {

    private final VectorStore vectorStore;

    public void storeDocuments(List<Document> documents) {
        // 分割文档
        List<Document> chunks = splitByCharacters(documents);

        // 存储到向量数据库
        vectorStore.add(chunks);
    }
}
```

### 4.2 PgVector Store

**依赖**

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-pgvector-store-spring-boot-starter</artifactId>
</dependency>
```

**配置**

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/vector_db
    username: user
    password: password
  ai:
    vectorstore:
      pgvector:
        dimension: 1536
        distance-type: cosine
        initialize-schema: true
```

### 4.3 Redis Vector Store

**依赖**

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-redis-store-spring-boot-starter</artifactId>
</dependency>
```

**配置**

```yaml
spring:
  data:
    redis:
      host: localhost
      port: 6379
  ai:
    vectorstore:
      redis:
        index-name: document-index
        prefix: "doc:"
        initialize-schema: true
```

---

## 5. 检索和生成

### 5.1 基础 RAG

```java
@Service
public class RagService {

    private final ChatClient chatClient;
    private final VectorStore vectorStore;

    public String query(String question) {
        // 1. 检索相关文档
        List<Document> relevantDocs = vectorStore.similaritySearch(
            SearchRequest.query(question)
                .withTopK(5)
                .withSimilarityThreshold(0.75)
        );

        // 2. 构建上下文
        String context = relevantDocs.stream()
            .map(Document::getContent)
            .collect(Collectors.joining("\n\n"));

        // 3. 生成回答
        String prompt = """
            基于以下上下文信息回答问题。如果上下文中没有相关信息，请明确说明。

            上下文：
            {context}

            问题：{question}

            答案：
            """.formatted(context, question);

        return chatClient.prompt()
            .user(prompt)
            .call()
            .content();
    }
}
```

### 5.2 使用 RetrievalAugmentationAdvisor

**配置 Advisor**

```java
@Configuration
public class RagConfig {

    @Bean
    public RetrievalAugmentationAdvisor ragAdvisor(VectorStore vectorStore) {
        VectorStoreDocumentRetriever retriever = VectorStoreDocumentRetriever.builder()
            .vectorStore(vectorStore)
            .topK(5)
            .similarityThreshold(0.75)
            .build();

        return RetrievalAugmentationAdvisor.builder()
            .documentRetriever(retriever)
            .queryTransformer(RewriteQueryTransformer.builder()
                .chatClientBuilder(ChatClient.builder(chatModel))
                .build())
            .build();
    }

    @Bean
    public ChatClient ragChatClient(
            ChatModel chatModel,
            RetrievalAugmentationAdvisor ragAdvisor) {
        return ChatClient.builder(chatModel)
            .defaultAdvisors(ragAdvisor)
            .build();
    }
}
```

**使用**

```java
@Service
public class EnhancedRagService {

    private final ChatClient ragChatClient;

    public String ask(String question) {
        // RAG 自动处理检索和生成
        return ragChatClient.prompt()
            .user(question)
            .call()
            .content();
    }
}
```

### 5.3 高级检索

**混合检索**

```java
public List<Document> hybridSearch(String query) {
    // 1. 向量检索
    List<Document> vectorResults = vectorStore.similaritySearch(
        SearchRequest.query(query).withTopK(5)
    );

    // 2. 关键词检索
    List<Document> keywordResults = keywordSearch(query, 5);

    // 3. 合并去重
    return Stream.concat(vectorResults.stream(), keywordResults.stream())
        .distinct()
        .limit(10)
        .toList();
}
```

**重排序**

```java
public List<Document> rerank(String query, List<Document> documents) {
    // 使用模型重新评分
    return documents.stream()
        .sorted((d1, d2) -> {
            double score1 = calculateRelevance(query, d1);
            double score2 = calculateRelevance(query, d2);
            return Double.compare(score2, score1);
        })
        .limit(5)
        .toList();
}
```

---

## 6. 高级特性

### 6.1 查询转换

**重写查询**

```java
@Service
public class QueryTransformationService {

    private final ChatClient chatClient;

    public String rewriteQuery(String originalQuery) {
        return chatClient.prompt()
            .user("""
                将以下查询重写为更适合检索的形式。

                原始查询：{query}

                重写后的查询：
                """.formatted(originalQuery))
            .call()
            .content();
    }
}
```

**查询扩展**

```java
public List<String> expandQuery(String query) {
    return chatClient.prompt()
        .user("""
            为以下查询生成3个不同的变体，以提高检索效果。

        原始查询：{query}

        变体（用换行分隔）：
        """.formatted(query))
        .call()
        .content()
        .lines()
        .toList();
}
```

### 6.2 多轮对话

```java
@Service
public class ConversationalRagService {

    private final ChatClient chatClient;
    private final VectorStore vectorStore;
    private final List<Message> conversationHistory = new ArrayList<>();

    public String chat(String userMessage) {
        // 1. 添加用户消息到历史
        conversationHistory.add(new UserMessage(userMessage));

        // 2. 检索相关文档
        String context = retrieveRelevantContext(userMessage);

        // 3. 构建提示
        String prompt = buildPromptWithContext(context, conversationHistory);

        // 4. 生成回答
        String response = chatClient.prompt()
            .messages(conversationHistory)
            .user(prompt)
            .call()
            .content();

        // 5. 添加助手响应到历史
        conversationHistory.add(new AssistantMessage(response));

        return response;
    }
}
```

### 6.3 引用来源

```java
public String answerWithCitations(String question) {
    // 1. 检索文档
    List<Document> docs = vectorStore.similaritySearch(question, 3);

    // 2. 生成回答
    String answer = chatClient.prompt()
        .user("""
            基于以下文档回答问题，并在答案中引用来源：

            文档：
            {docs}

            问题：{question}

            请使用 [来源X] 的格式引用。
            """.formatted(
                docs.stream()
                    .map(d -> "[" + docs.indexOf(d) + "] " + d.getContent())
                    .collect(Collectors.joining("\n")),
                question
            ))
        .call()
        .content();

    // 3. 添加来源列表
    String sources = docs.stream()
        .map(d -> "- " + d.getMetadata().get("source"))
        .collect(Collectors.joining("\n"));

    return answer + "\n\n来源：\n" + sources;
}
```

---

## 7. 性能优化

### 7.1 批量处理

```java
public void batchStoreDocuments(List<Document> documents) {
    // 分批处理，每批100个文档
    int batchSize = 100;
    for (int i = 0; i < documents.size(); i += batchSize) {
        int end = Math.min(i + batchSize, documents.size());
        List<Document> batch = documents.subList(i, end);
        vectorStore.add(batch);
    }
}
```

### 7.2 缓存检索结果

```java
@Cacheable(value = "rag-cache", key = "#question")
public List<Document> retrieveWithCache(String question) {
    return vectorStore.similaritySearch(question);
}
```

### 7.3 异步处理

```java
@Service
public class AsyncRagService {

    @Async
    public CompletableFuture<String> asyncQuery(String question) {
        List<Document> docs = vectorStore.similaritySearch(question);
        String answer = chatClient.prompt()
            .user(buildPrompt(docs, question))
            .call()
            .content();
        return CompletableFuture.completedFuture(answer);
    }
}
```

---

## 8. 完整示例

### 8.1 知识库问答系统

```java
@Service
public class KnowledgeBaseQA {

    private final ChatClient chatClient;
    private final VectorStore vectorStore;
    private final DocumentLoader documentLoader;
    private final DocumentSplitter documentSplitter;

    @PostConstruct
    public void initializeKnowledgeBase() {
        // 1. 加载文档
        List<Document> documents = documentLoader.loadPdfFiles("knowledge-base.pdf");

        // 2. 分割文档
        List<Document> chunks = documentSplitter.splitByCharacters(documents);

        // 3. 存储到向量数据库
        vectorStore.add(chunks);
    }

    public String ask(String question) {
        // 1. 检索相关文档
        List<Document> relevantDocs = vectorStore.similaritySearch(question, 5);

        // 2. 构建上下文
        String context = relevantDocs.stream()
            .map(Document::getContent)
            .collect(Collectors.joining("\n\n---\n\n"));

        // 3. 生成回答
        return chatClient.prompt()
            .system("你是知识库助手，基于提供的上下文准确回答问题")
            .user("""
                上下文信息：
                {context}

                问题：{question}

                请基于上下文信息提供准确、详细的答案。如果上下文中没有相关信息，请明确说明。
                """.formatted(context, question))
            .call()
            .content();
    }
}
```

### 8.2 多文档 RAG

```java
@Service
public class MultiDocumentRag {

    private final Map<String, VectorStore> documentStores;

    public String queryAcrossDocuments(String query) {
        // 1. 从所有文档库检索
        List<Document> allResults = documentStores.values().stream()
            .flatMap(store -> store.similaritySearch(query).stream())
            .collect(Collectors.toList());

        // 2. 重排序
        List<Document> topDocs = rerank(query, allResults).subList(0, 5);

        // 3. 生成答案
        return generateAnswer(query, topDocs);
    }
}
```

---

## 9. 监控和评估

### 9.1 检索质量评估

```java
@Service
public class RagEvaluator {

    public double evaluateRetrieval(String question, List<Document> retrieved) {
        // 计算检索准确率
        List<String> relevantDocIds = getRelevantDocIds(question);
        List<String> retrievedIds = retrieved.stream()
            .map(d -> d.getId())
            .toList();

        long correct = retrievedIds.stream()
            .filter(relevantDocIds::contains)
            .count();

        return (double) correct / relevantDocIds.size();
    }
}
```

### 9.2 生成质量评估

```java
public String evaluateAnswer(String question, String answer, List<Document> context) {
    String evaluationPrompt = """
        评估以下答案的质量：

        问题：%s
        上下文：%s
        答案：%s

        评估标准：
        1. 准确性：答案是否基于上下文且准确
        2. 完整性：答案是否完整回答了问题
        3. 清晰性：答案是否清晰易懂

        请给出评分（1-10）和具体建议。
        """.formatted(question, context, answer);

    return chatClient.prompt()
        .user(evaluationPrompt)
        .call()
        .content();
}
```

---

## 10. 最佳实践

### 10.1 文档预处理

- 清理HTML标签和特殊字符
- 移除无关内容（页眉、页脚、导航）
- 标准化格式
- 添加有意义的元数据

### 10.2 分块策略

- 根据内容类型选择合适的分块大小
- 保持语义完整性
- 使用重叠避免信息丢失
- 考虑LLM的上下文窗口限制

### 10.3 检索优化

- 调整相似度阈值
- 使用混合检索提高召回率
- 对结果进行重排序
- 缓存常见查询

### 10.4 生成优化

- 提供清晰的上下文格式
- 明确指示引用要求
- 使用结构化输出
- 实现多轮对话支持

---

© 2025 Spring AI 技术文档
