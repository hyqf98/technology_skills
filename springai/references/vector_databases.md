# Spring AI 向量数据库集成

**页数:** 6

---

## 1. 向量数据库概述

Spring AI 支持多种向量数据库，用于存储和检索文档向量。

### 1.1 支持的向量数据库

| 数据库 | 适用场景 | 依赖 |
|--------|---------|------|
| **Simple Vector Store** | 开发测试、内存存储 | spring-ai-simple-vector-store |
| **PgVector** | Postgres + 向量搜索 | spring-ai-pgvector-store |
| **Chroma** | 开源向量数据库 | spring-ai-chroma-store |
| **Milvus** | 大规模生产环境 | spring-ai-milvus-store |
| **Redis** | Redis + 向量搜索 | spring-ai-redis-store |
| **MongoDB Atlas** | MongoDB + 向量搜索 | spring-ai-mongodb-atlas-store |
| **Neo4j** | 图数据库 + 向量搜索 | spring-ai-neo4j-store |
| **Qdrant** | 高性能向量数据库 | spring-ai-qdrant-store |
| **Weaviate** | 语义搜索 | spring-ai-weaviate-store |

---

## 2. Simple Vector Store

### 2.1 依赖配置

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-simple-vector-store</artifactId>
</dependency>
```

### 2.2 配置

**application.yml**

```yaml
spring:
  ai:
    vectorstore:
      simple:
        initialize-schema: true
```

### 2.3 使用

```java
@Service
public class SimpleVectorStoreService {

    private final VectorStore vectorStore;

    public void addDocuments(List<Document> documents) {
        vectorStore.add(documents);
    }

    public List<Document> search(String query) {
        return vectorStore.similaritySearch(
            SearchRequest.query(query)
                .withTopK(5)
                .withSimilarityThreshold(0.7)
        );
    }

    public void delete(String id) {
        vectorStore.delete(List.of(id));
    }
}
```

---

## 3. PgVector Store

### 3.1 依赖配置

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-pgvector-store-spring-boot-starter</artifactId>
</dependency>
```

### 3.2 配置

**application.yml**

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/vectordb
    username: postgres
    password: password
  ai:
    vectorstore:
      pgvector:
        dimension: 1536
        distance-type: cosine
        initialize-schema: true
```

### 3.3 初始化表

```sql
CREATE EXTENSION IF NOT EXISTS vector;

CREATE TABLE vector_store (
    id SERIAL PRIMARY KEY,
    content TEXT,
    metadata JSONB,
    embedding vector(1536),
    similarity_score FLOAT
);

CREATE INDEX ON vector_store USING ivfflat (embedding vector_cosine_ops);
```

### 3.4 使用

```java
@Service
public class PgVectorService {

    private final VectorStore vectorStore;

    public void storeDocument(String content, Map<String, Object> metadata) {
        Document document = new Document(content, metadata);
        vectorStore.add(List.of(document));
    }

    public List<Document> search(String query, int topK) {
        return vectorStore.similaritySearch(
            SearchRequest.query(query)
                .withTopK(topK)
                .withSimilarityThreshold(0.75)
        );
    }
}
```

---

## 4. Chroma Vector Store

### 4.1 依赖配置

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-chroma-store-spring-boot-starter</artifactId>
</dependency>
```

### 4.2 配置

**application.yml**

```yaml
spring:
  ai:
    vectorstore:
      chroma:
        client:
          host: localhost
          port: 8000
        collection-name: documents
        initialize-schema: true
```

### 4.3 使用

```java
@Service
public class ChromaVectorStoreService {

    private final VectorStore chromaVectorStore;

    public void addDocument(Document document) {
        chromaVectorStore.add(List.of(document));
    }

    public List<Document> similaritySearch(String query, double threshold) {
        return chromaVectorStore.similaritySearch(
            SearchRequest.query(query)
                .withSimilarityThreshold(threshold)
                .withTopK(10)
        );
    }

    public void deleteCollection() {
        chromaVectorStore.delete(List.of()); // 清空所有文档
    }
}
```

---

## 5. Redis Vector Store

### 5.1 依赖配置

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-redis-store-spring-boot-starter</artifactId>
</dependency>
```

### 5.2 配置

**application.yml**

```yaml
spring:
  data:
    redis:
      host: localhost
      port: 6379
      password: ${REDIS_PASSWORD}
  ai:
    vectorstore:
      redis:
        index-name: document-index
        prefix: "doc:"
        initialize-schema: true
```

### 5.3 使用

```java
@Service
public class RedisVectorStoreService {

    private final VectorStore redisVectorStore;

    public void storeDocument(String id, String content) {
        Document document = new Document(id, content);
        redisVectorStore.add(List.of(document));
    }

    public List<Document> search(String query, int topK) {
        return redisVectorStore.similaritySearch(
            SearchRequest.query(query)
                .withTopK(topK)
        );
    }

    public void deleteDocument(String id) {
        redisVectorStore.delete(List.of(id));
    }
}
```

---

## 6. Milvus Vector Store

### 6.1 依赖配置

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-milvus-store-spring-boot-starter</artifactId>
</dependency>
```

### 6.2 配置

**application.yml**

```yaml
spring:
  ai:
    vectorstore:
      milvus:
        client:
          host: localhost
          port: 19530
        collection-name: documents
        dimension: 1536
        index-type: IVF_FLAT
        metric-type: COSINE
        initialize-schema: true
```

### 6.3 使用

```java
@Service
public class MilvusVectorStoreService {

    private final VectorStore milvusVectorStore;

    public void batchInsert(List<Document> documents) {
        milvusVectorStore.add(documents);
    }

    public List<Document> hybridSearch(String query, String filter) {
        return milvusVectorStore.similaritySearch(
            SearchRequest.query(query)
                .withTopK(10)
                .withFilterExpression(filter)
        );
    }
}
```

---

## 7. 向量存储操作

### 7.1 添加文档

```java
@Service
public class VectorStoreOperations {

    private final VectorStore vectorStore;

    // 单个文档
    public void addDocument(String content, Map<String, Object> metadata) {
        Document document = new Document(content, metadata);
        vectorStore.add(List.of(document));
    }

    // 批量添加
    public void addDocuments(List<String> contents) {
        List<Document> documents = contents.stream()
            .map(Document::new)
            .toList();
        vectorStore.add(documents);
    }

    // 带元数据批量添加
    public void addDocumentsWithMetadata(
            List<String> contents,
            List<Map<String, Object>> metadataList) {

        List<Document> documents = new ArrayList<>();
        for (int i = 0; i < contents.size(); i++) {
            Document doc = new Document(contents.get(i), metadataList.get(i));
            documents.add(doc);
        }
        vectorStore.add(documents);
    }
}
```

### 7.2 相似度搜索

```java
@Service
public class VectorSearchService {

    private final VectorStore vectorStore;

    // 基础搜索
    public List<Document> search(String query, int topK) {
        return vectorStore.similaritySearch(
            SearchRequest.query(query)
                .withTopK(topK)
        );
    }

    // 带阈值搜索
    public List<Document> searchWithThreshold(
            String query,
            int topK,
            double threshold) {

        return vectorStore.similaritySearch(
            SearchRequest.query(query)
                .withTopK(topK)
                .withSimilarityThreshold(threshold)
        );
    }

    // 带过滤搜索
    public List<Document> searchWithFilter(
            String query,
            String filterExpression) {

        return vectorStore.similaritySearch(
            SearchRequest.query(query)
                .withTopK(10)
                .withFilterExpression(filterExpression)
        );
    }
}
```

### 7.3 删除操作

```java
@Service
public class VectorDeleteService {

    private final VectorStore vectorStore;

    // 按ID删除
    public void deleteById(String id) {
        vectorStore.delete(List.of(id));
    }

    // 批量删除
    public void deleteByIds(List<String> ids) {
        vectorStore.delete(ids);
    }

    // 删除所有
    public void deleteAll() {
        vectorStore.delete(List.of());
    }
}
```

### 7.4 更新操作

```java
@Service
public class VectorUpdateService {

    private final VectorStore vectorStore;

    public void updateDocument(String id, String newContent) {
        // 1. 删除旧文档
        vectorStore.delete(List.of(id));

        // 2. 添加新文档
        Document newDoc = new Document(id, newContent);
        vectorStore.add(List.of(newDoc));
    }
}
```

---

## 8. 高级功能

### 8.1 自定义 Embedding

```java
@Configuration
public class EmbeddingConfig {

    @Bean
    public VectorStore vectorStore(EmbeddingModel embeddingModel) {
        return new SimpleVectorStore(
            embeddingModel,
            SimpleVectorStoreConfig.builder()
                .initializeSchema(true)
                .build()
        );
    }
}
```

### 8.2 批量操作

```java
@Service
public class BatchVectorService {

    private final VectorStore vectorStore;
    private static final int BATCH_SIZE = 100;

    public void batchAdd(List<Document> documents) {
        for (int i = 0; i < documents.size(); i += BATCH_SIZE) {
            int end = Math.min(i + BATCH_SIZE, documents.size());
            List<Document> batch = documents.subList(i, end);
            vectorStore.add(batch);
        }
    }
}
```

### 8.3 混合搜索

```java
@Service
public class HybridSearchService {

    private final VectorStore vectorStore;
    private final JdbcTemplate jdbcTemplate;

    public List<Document> hybridSearch(String query, String keyword) {
        // 1. 向量搜索
        List<Document> vectorResults = vectorStore.similaritySearch(query, 20);

        // 2. 关键词搜索
        List<Document> keywordResults = searchByKeyword(keyword);

        // 3. 合并和重排序
        return mergeAndRerank(vectorResults, keywordResults, 10);
    }

    private List<Document> searchByKeyword(String keyword) {
        String sql = "SELECT * FROM documents WHERE content LIKE ?";
        return jdbcTemplate.query(sql,
            (rs, rowNum) -> new Document(rs.getString("content")),
            "%" + keyword + "%"
        );
    }
}
```

---

## 9. 性能优化

### 9.1 索引优化

**PgVector 索引**

```sql
-- IVFFlat 索引
CREATE INDEX ON documents USING ivfflat (embedding vector_cosine_ops) WITH (lists = 100);

-- HNSW 索引（PostgreSQL 15+）
CREATE INDEX ON documents USING hnsw (embedding vector_cosine_ops) WITH (m = 16, ef_construction = 64);
```

### 9.2 连接池配置

```yaml
spring:
  datasource:
    hikari:
      maximum-pool-size: 10
      minimum-idle: 5
      connection-timeout: 30000
```

### 9.3 批量操作

```java
@Service
public class OptimizedVectorService {

    private final VectorStore vectorStore;

    @Async
    public CompletableFuture<Void> asyncAdd(List<Document> documents) {
        vectorStore.add(documents);
        return CompletableFuture.completedFuture(null);
    }

    public void parallelAdd(List<List<Document>> batches) {
        batches.parallelStream().forEach(batch -> {
            vectorStore.add(batch);
        });
    }
}
```

---

## 10. 完整示例

### 10.1 文档管理系统

```java
@Service
public class DocumentManagementService {

    private final VectorStore vectorStore;
    private final EmbeddingModel embeddingModel;

    // 上传文档
    public void uploadDocument(MultipartFile file) throws IOException {
        // 1. 读取文件内容
        String content = new String(file.getBytes());

        // 2. 创建文档
        Map<String, Object> metadata = new HashMap<>();
        metadata.put("filename", file.getOriginalFilename());
        metadata.put("size", file.getSize());
        metadata.put("uploadTime", Instant.now());

        Document document = new Document(content, metadata);

        // 3. 存储到向量数据库
        vectorStore.add(List.of(document));
    }

    // 搜索文档
    public List<Document> searchDocuments(String query) {
        return vectorStore.similaritySearch(
            SearchRequest.query(query)
                .withTopK(10)
                .withSimilarityThreshold(0.7)
        );
    }

    // 删除文档
    public void deleteDocument(String documentId) {
        vectorStore.delete(List.of(documentId));
    }
}
```

### 10.2 智能问答系统

```java
@Service
public class IntelligentQAService {

    private final ChatClient chatClient;
    private final VectorStore vectorStore;

    public String ask(String question) {
        // 1. 检索相关文档
        List<Document> relevantDocs = vectorStore.similaritySearch(question, 5);

        // 2. 构建上下文
        String context = buildContext(relevantDocs);

        // 3. 生成答案
        return chatClient.prompt()
            .system("你是基于知识库的智能助手")
            .user("""
                知识库内容：
                {context}

                问题：{question}

                请基于知识库内容准确回答问题。
                """.formatted(context, question))
            .call()
            .content();
    }

    private String buildContext(List<Document> docs) {
        return docs.stream()
            .map(doc -> {
                String source = (String) doc.getMetadata().get("filename");
                return String.format("[%s] %s", source, doc.getContent());
            })
            .collect(Collectors.joining("\n\n"));
    }
}
```

---

## 11. 最佳实践

### 11.1 选择合适的向量数据库

- **开发测试**: Simple Vector Store
- **小型项目** (< 10K 文档): Chroma, PgVector
- **中型项目** (10K-1M 文档): Redis, Milvus
- **大型项目** (> 1M 文档): Milvus, Qdrant

### 11.2 向量化策略

- 选择合适的分块大小（500-1000 tokens）
- 保留语义完整性
- 使用重叠避免信息丢失
- 添加有意义的元数据

### 11.3 检索优化

- 调整相似度阈值
- 使用混合检索
- 实现结果缓存
- 批量处理提高效率

### 11.4 监控和维护

- 定期监控性能指标
- 优化索引配置
- 清理无效数据
- 备份重要数据

---

© 2025 Spring AI 技术文档
