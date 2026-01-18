# Spring AI 快速入门

**页数:** 10

---

## 1. 项目配置

### 1.1 Maven 依赖

**Spring Boot 3.x**

```xml
<dependencies>
    <!-- OpenAI 集成 -->
    <dependency>
        <groupId>org.springframework.ai</groupId>
        <artifactId>spring-ai-openai-spring-boot-starter</artifactId>
        <version>1.0.0-M4</version>
    </dependency>

    <!-- Azure OpenAI 集成 -->
    <dependency>
        <groupId>org.springframework.ai</groupId>
        <artifactId>spring-ai-azure-openai-spring-boot-starter</artifactId>
        <version>1.0.0-M4</version>
    </dependency>

    <!-- 向量数据库（以 Simple Vector Store 为例）-->
    <dependency>
        <groupId>org.springframework.ai</groupId>
        <artifactId>spring-ai-simple-vector-store</artifactId>
        <version>1.0.0-M4</version>
    </dependency>
</dependencies>
```

### 1.2 Gradle 依赖

```gradle
dependencies {
    // OpenAI 集成
    implementation 'org.springframework.ai:spring-ai-openai-spring-boot-starter:1.0.0-M4'

    // Azure OpenAI 集成
    implementation 'org.springframework.ai:spring-ai-azure-openai-spring-boot-starter:1.0.0-M4'

    // 向量数据库
    implementation 'org.springframework.ai:spring-ai-simple-vector-store:1.0.0-M4'
}
```

### 1.3 仓库配置

**Maven**

```xml
<repositories>
    <repository>
        <id>spring-milestones</id>
        <name>Spring Milestones</name>
        <url>https://repo.spring.io/milestone</url>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </repository>
</repositories>
```

**Gradle**

```gradle
repositories {
    maven { url 'https://repo.spring.io/milestone' }
}
```

---

## 2. API 配置

### 2.1 OpenAI 配置

**application.yml**

```yaml
spring:
  ai:
    openai:
      api-key: ${OPENAI_API_KEY}  # 从环境变量读取
      chat:
        options:
          model: gpt-4o
          temperature: 0.7
      embedding:
        options:
          model: text-embedding-3-small
```

**环境变量**

```bash
export OPENAI_API_KEY=your-api-key-here
```

### 2.2 Azure OpenAI 配置

**application.yml**

```yaml
spring:
  ai:
    azure:
      openai:
        api-key: ${AZURE_OPENAI_API_KEY}
        endpoint: ${AZURE_OPENAI_ENDPOINT}
        deployment-name: ${AZURE_OPENAI_DEPLOYMENT_NAME}
```

---

## 3. 基础 API 使用

### 3.1 ChatClient - 聊天对话

**简单对话**

```java
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.stereotype.Service;

@Service
public class ChatService {

    private final ChatClient chatClient;

    public ChatService(ChatClient.Builder chatClientBuilder) {
        this.chatClient = chatClientBuilder.build();
    }

    public String chat(String message) {
        return chatClient.prompt()
            .user(message)
            .call()
            .content();
    }
}
```

**带系统提示的对话**

```java
public String chatWithSystem(String userMessage) {
    return chatClient.prompt()
        .system("你是一个专业的技术顾问，请用简洁专业的语言回答问题")
        .user(userMessage)
        .call()
        .content();
}
```

**流式响应**

```java
public void streamChat(String message) {
    chatClient.prompt()
        .user(message)
        .stream()
        .content()
        .forEach(System.out::print);
}
```

### 3.2 结构化输出

**返回指定类型**

```java
record Actor(String name, int age);

public Actor getActor(String description) {
    return chatClient.prompt()
        .user("根据描述创建演员信息: " + description)
        .call()
        .entity(Actor.class);
}
```

**返回列表**

```java
public List<Actor> getActors(String description) {
    return chatClient.prompt()
        .user("根据描述创建3个演员信息: " + description)
        .call()
        .entityList(Actor.class);
}
```

### 3.3 函数调用

**定义函数**

```java
import java.util.function.Function;

@Service
public class WeatherService implements Function<WeatherService.Request, WeatherService.Response> {

    public record Request(String location, String unit) {}
    public record Response(double temperature, String condition) {}

    @Override
    public Response apply(Request request) {
        // 实际调用天气 API
        return new Response(25.5, "晴朗");
    }
}
```

**启用函数调用**

```java
@Autowired
private WeatherService weatherService;

public String getWeather(String location) {
    return chatClient.prompt()
        .user("今天" + location + "的天气怎么样？")
        .functions(weatherService)  // 启用函数
        .call()
        .content();
}
```

---

## 4. Embedding API - 文本向量化

### 4.1 基础使用

```java
import org.springframework.ai.embedding.EmbeddingModel;
import org.springframework.ai.embedding.EmbeddingResponse;

@Service
public class EmbeddingService {

    private final EmbeddingModel embeddingModel;

    public EmbeddingService(EmbeddingModel embeddingModel) {
        this.embeddingModel = embeddingModel;
    }

    public float[] embed(String text) {
        return embeddingModel.embed(text);
    }

    public List<float[]> embedBatch(List<String> texts) {
        EmbeddingResponse response = embeddingModel.embedForResponse(
            List.of(text1, text2, text3)
        );
        return response.getResults().stream()
            .map(result -> result.getOutput())
            .toList();
    }
}
```

---

## 5. 向量存储

### 5.1 SimpleVectorStore 配置

**application.yml**

```yaml
spring:
  ai:
    vectorstore:
      simple:
        initialize-schema: true
```

### 5.2 存储文档

```java
import org.springframework.ai.document.Document;
import org.springframework.ai.vectorstore.VectorStore;
import org.springframework.stereotype.Service;

@Service
public class DocumentService {

    private final VectorStore vectorStore;

    public void storeDocuments() {
        List<Document> documents = List.of(
            new Document("Spring AI 是一个用于构建 AI 应用的框架"),
            new Document("它支持多种 AI 模型和向量数据库"),
            new Document("可以轻松实现 RAG 应用")
        );

        vectorStore.add(documents);
    }
}
```

### 5.3 相似度搜索

```java
public List<Document> searchSimilar(String query) {
    return vectorStore.similaritySearch(
        SearchRequest.query(query).withTopK(5)
    );
}
```

---

## 6. RAG 实现

### 6.1 基础 RAG

```java
import org.springframework.ai.rag.preretrieval.query.TransformationQueryTransformer;
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.vectorstore.VectorStore;

@Service
public class RagService {

    private final ChatClient chatClient;
    private final VectorStore vectorStore;

    public String ragQuery(String question) {
        // 1. 检索相关文档
        List<Document> docs = vectorStore.similaritySearch(question);

        // 2. 构建增强提示
        String context = docs.stream()
            .map(Document::getContent)
            .collect(Collectors.joining("\n"));

        String enhancedPrompt = """
            基于以下上下文信息回答问题：

            上下文：
            %s

            问题：%s
            """.formatted(context, question);

        // 3. 生成回答
        return chatClient.prompt()
            .user(enhancedPrompt)
            .call()
            .content();
    }
}
```

### 6.2 使用 RetrievalAugmentationAdvisor

**配置 RAG Advisor**

```java
import org.springframework.ai.rag.advisor.RetrievalAugmentationAdvisor;
import org.springframework.ai.rag.retrieval.search.VectorStoreDocumentRetriever;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

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
            .build();
    }
}
```

**使用 RAG**

```java
@Service
public class RagChatService {

    private final ChatClient chatClient;

    public RagChatService(
            ChatClient.Builder chatClientBuilder,
            RetrievalAugmentationAdvisor ragAdvisor) {
        this.chatClient = chatClientBuilder
            .defaultAdvisors(ragAdvisor)
            .build();
    }

    public String ask(String question) {
        return chatClient.prompt()
            .user(question)
            .call()
            .content();
    }
}
```

---

## 7. Prompt 模板

### 7.1 使用 PromptTemplate

```java
import org.springframework.ai.prompt.prompt.PromptTemplate;

@Service
public class PromptService {

    public String generatePrompt(String topic, String style) {
        PromptTemplate promptTemplate = new PromptTemplate("""
            请以{style}的风格写一篇关于{topic}的文章。
            要求：
            1. 字数不少于500字
            2. 结构清晰，逻辑严密
            3. 内容原创，不得抄袭
            """);

        return promptTemplate.create()
            .add("topic", topic)
            .add("style", style)
            .getText();
    }
}
```

### 7.2 使用 ChatClient 便捷方法

```java
public String chatWithTemplate(String topic) {
    return chatClient.prompt()
        .user(u -> u
            .text("请详细介绍{topic}")
            .param("topic", topic)
        )
        .call()
        .content();
}
```

---

## 8. 错误处理

### 8.1 重试配置

**application.yml**

```yaml
spring:
  ai:
    retry:
      max-attempts: 3
      backoff:
        initial-interval: 1000
        multiplier: 2
        max-interval: 10000
```

### 8.2 异常处理

```java
import org.springframework.ai.retry.RetryExhaustedException;

@Service
public class RobustChatService {

    public String safeChat(String message) {
        try {
            return chatClient.prompt()
                .user(message)
                .call()
                .content();
        } catch (RetryExhaustedException e) {
            // 重试失败后的处理
            return "抱歉，服务暂时不可用，请稍后再试";
        } catch (Exception e) {
            // 其他异常处理
            return "发生错误: " + e.getMessage();
        }
    }
}
```

---

## 9. 完整示例

### 9.1 Controller 层

```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/ai")
public class AiController {

    private final ChatService chatService;
    private final RagService ragService;

    @PostMapping("/chat")
    public String chat(@RequestBody String message) {
        return chatService.chat(message);
    }

    @PostMapping("/rag")
    public String rag(@RequestBody String question) {
        return ragService.ragQuery(question);
    }

    @GetMapping("/stream")
    public Flux<String> streamChat(@RequestParam String message) {
        return chatService.streamChat(message);
    }
}
```

### 9.2 配置类

```java
@Configuration
public class SpringAiConfig {

    @Bean
    public ChatClient chatClient(ChatModel chatModel) {
        return ChatClient.builder(chatModel)
            .defaultOptions(OpenAiChatOptions.builder()
                .model("gpt-4o")
                .temperature(0.7)
                .build())
            .build();
    }
}
```

---

## 10. 最佳实践

### 10.1 API Key 管理

**使用环境变量**

```yaml
spring:
  ai:
    openai:
      api-key: ${OPENAI_API_KEY}
```

**使用配置文件（不推荐提交到版本控制）**

```yaml
# application-local.yml
spring:
  ai:
    openai:
      api-key: sk-your-actual-key-here
```

**application.yml**

```yaml
spring:
  profiles:
    active: local
```

**.gitignore**

```
application-local.yml
```

### 10.2 成本优化

**使用更便宜的模型**

```java
// 简单任务使用 gpt-3.5
String simpleResponse = chatClient.prompt()
    .user("简单问题")
    .options(OpenAiChatOptions.builder()
        .model("gpt-3.5-turbo")
        .build())
    .call()
    .content();

// 复杂任务使用 gpt-4
String complexResponse = chatClient.prompt()
    .user("复杂问题")
    .options(OpenAiChatOptions.builder()
        .model("gpt-4o")
        .build())
    .call()
    .content();
```

### 10.3 性能优化

**批量处理**

```java
public List<String> batchProcess(List<String> inputs) {
    return inputs.parallelStream()
        .map(this::processSingle)
        .toList();
}
```

**缓存结果**

```java
@Cacheable("chat-cache")
public String getCachedResponse(String message) {
    return chatClient.prompt().user(message).call().content();
}
```

---

## 相关资源

- **官方文档**: https://docs.spring.io/spring-ai/reference/
- **GitHub**: https://github.com/spring-projects/spring-ai
- **示例代码**: https://github.com/spring-projects/spring-ai-examples

---

© 2025 Spring AI 技术文档
