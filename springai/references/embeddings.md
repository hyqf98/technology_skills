# Spring AI Embedding API

**页数:** 3

---

## 1. 依赖配置

### 1.1 OpenAI Embedding

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-openai-spring-boot-starter</artifactId>
</dependency>
```

**配置**

```yaml
spring:
  ai:
    openai:
      api-key: ${OPENAI_API_KEY}
      embedding:
        options:
          model: text-embedding-3-small
```

### 1.2 其他提供商

```yaml
# Azure OpenAI
spring:
  ai:
    azure:
      openai:
        embedding:
          options:
            model: text-embedding-ada-002

# Ollama (本地)
spring:
  ai:
    ollama:
      base-url: http://localhost:11434
      embedding:
        options:
          model: nomic-embed-text
```

---

## 2. EmbeddingModel API

### 2.1 基础使用

```java
@Service
public class EmbeddingService {
    private final EmbeddingModel embeddingModel;

    // 单个文本
    public float[] embed(String text) {
        return embeddingModel.embed(text);
    }

    // 批量文本
    public List<float[]> embedBatch(List<String> texts) {
        EmbeddingResponse response = embeddingModel.embedForResponse(texts);
        return response.getResults().stream()
            .map(Embedding::getOutput)
            .toList();
    }
}
```

### 2.2 带选项

```java
public float[] embedWithOptions(String text) {
    EmbeddingOptions options = EmbeddingOptions.builder()
        .model("text-embedding-3-small")
        .build();

    return embeddingModel.embed(text, options);
}
```

---

## 3. 文档向量化

### 3.1 批量处理

```java
@Service
public class DocumentEmbeddingService {
    private final EmbeddingModel embeddingModel;
    private final VectorStore vectorStore;

    public void embedAndStore(List<String> texts) {
        // 1. 转换为文档
        List<Document> documents = texts.stream()
            .map(Document::new)
            .toList();

        // 2. 添加到向量存储（自动嵌入）
        vectorStore.add(documents);
    }
}
```

### 3.2 增量更新

```java
public void updateDocument(String docId, String newContent) {
    // 1. 删除旧文档
    vectorStore.delete(List.of(docId));

    // 2. 添加新文档
    Document newDoc = new Document(docId, newContent);
    vectorStore.add(List.of(newDoc));
}
```

---

## 4. 性能优化

### 4.1 批量处理

```java
public List<float[]> batchEmbed(List<String> texts, int batchSize) {
    List<float[]> allEmbeddings = new ArrayList<>();

    for (int i = 0; i < texts.size(); i += batchSize) {
        int end = Math.min(i + batchSize, texts.size());
        List<String> batch = texts.subList(i, end);

        List<float[]> batchEmbeddings = embeddingModel.embedForResponse(batch)
            .getResults().stream()
            .map(Embedding::getOutput)
            .toList();

        allEmbeddings.addAll(batchEmbeddings);
    }

    return allEmbeddings;
}
```

### 4.2 缓存

```java
@Service
public class CachedEmbeddingService {

    private final EmbeddingModel embeddingModel;
    private final Map<String, float[]> cache = new ConcurrentHashMap<>();

    public float[] embed(String text) {
        return cache.computeIfAbsent(text, t -> {
            log.info("Computing embedding for: {}", t);
            return embeddingModel.embed(t);
        });
    }
}
```

---

## 5. 常用模型

| 模型 | 维度 | 用途 |
|------|------|------|
| text-embedding-3-small | 1536 | 通用、快速 |
| text-embedding-3-large | 3072 | 高精度 |
| text-embedding-ada-002 | 1536 | 经济实用 |

---

## 6. 完整示例

```java
@Service
public class SemanticSearchService {

    private final EmbeddingModel embeddingModel;
    private final VectorStore vectorStore;

    // 建立索引
    public void indexDocuments(List<String> documents) {
        List<Document> docs = documents.stream()
            .map(content -> {
                Map<String, Object> metadata = new HashMap<>();
                metadata.put("timestamp", Instant.now());
                return new Document(content, metadata);
            })
            .toList();

        vectorStore.add(docs);
    }

    // 语义搜索
    public List<Document> semanticSearch(String query, int topK) {
        return vectorStore.similaritySearch(
            SearchRequest.query(query)
                .withTopK(topK)
                .withSimilarityThreshold(0.7)
        );
    }

    // 相似度计算
    public double calculateSimilarity(String text1, String text2) {
        float[] embedding1 = embeddingModel.embed(text1);
        float[] embedding2 = embeddingModel.embed(text2);

        return cosineSimilarity(embedding1, embedding2);
    }

    private double cosineSimilarity(float[] v1, float[] v2) {
        double dot = 0.0;
        double norm1 = 0.0;
        double norm2 = 0.0;

        for (int i = 0; i < v1.length; i++) {
            dot += v1[i] * v2[i];
            norm1 += v1[i] * v1[i];
            norm2 += v2[i] * v2[i];
        }

        return dot / (Math.sqrt(norm1) * Math.sqrt(norm2));
    }
}
```

---

© 2025 Spring AI 技术文档
