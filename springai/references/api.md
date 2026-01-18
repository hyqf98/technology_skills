# Spring AI API 参考

**页数:** 5

---

## 1. ChatClient API

### 1.1 创建 ChatClient

**自动配置**

```java
@Service
public class ChatService {
    private final ChatClient chatClient;

    public ChatService(ChatClient.Builder chatClientBuilder) {
        this.chatClient = chatClientBuilder.build();
    }
}
```

**手动配置**

```java
@Configuration
public class ChatConfig {
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

### 1.2 基础方法

```java
// 简单调用
String response = chatClient.prompt()
    .user("Hello")
    .call()
    .content();

// 带系统提示
String response = chatClient.prompt()
    .system("You are a helpful assistant")
    .user("Hello")
    .call()
    .content();

// 流式响应
Flux<String> stream = chatClient.prompt()
    .user("Tell me a story")
    .stream()
    .content();
```

### 1.3 结构化输出

```java
// 单个对象
record Person(String name, int age);
Person person = chatClient.prompt()
    .user("Extract person info from: John is 30 years old")
    .call()
    .entity(Person.class);

// 对象列表
List<Person> people = chatClient.prompt()
    .user("Extract all people from the text")
    .call()
    .entityList(Person.class);
```

---

## 2. ChatModel API

### 2.1 基础调用

```java
@Service
public class ModelService {
    private final ChatModel chatModel;

    public ChatResponse chat(String message) {
        Prompt prompt = new Prompt(message);
        return chatModel.call(prompt);
    }
}
```

### 2.2 带选项调用

```java
public ChatResponse chatWithOptions(String message) {
    ChatOptions options = OpenAiChatOptions.builder()
        .model("gpt-4o")
        .temperature(0.7)
        .maxTokens(2000)
        .build();

    Prompt prompt = new Prompt(message, options);
    return chatModel.call(prompt);
}
```

---

## 3. EmbeddingModel API

### 3.1 基础使用

```java
@Service
public class EmbeddingService {
    private final EmbeddingModel embeddingModel;

    // 单个文本嵌入
    public float[] embed(String text) {
        return embeddingModel.embed(text);
    }

    // 批量嵌入
    public List<float[]> embedBatch(List<String> texts) {
        EmbeddingResponse response = embeddingModel.embedForResponse(texts);
        return response.getResults().stream()
            .map(Embedding::getOutput)
            .toList();
    }
}
```

---

## 4. VectorStore API

### 4.1 添加文档

```java
@Service
public class VectorStoreService {
    private final VectorStore vectorStore;

    // 添加单个文档
    public void addDocument(String content) {
        Document document = new Document(content);
        vectorStore.add(List.of(document));
    }

    // 批量添加
    public void addDocuments(List<String> contents) {
        List<Document> documents = contents.stream()
            .map(Document::new)
            .toList();
        vectorStore.add(documents);
    }
}
```

### 4.2 相似度搜索

```java
// 基础搜索
public List<Document> search(String query) {
    return vectorStore.similaritySearch(query);
}

// 带参数搜索
public List<Document> search(String query, int topK, double threshold) {
    return vectorStore.similaritySearch(
        SearchRequest.query(query)
            .withTopK(topK)
            .withSimilarityThreshold(threshold)
    );
}
```

### 4.3 删除操作

```java
// 按ID删除
public void delete(String id) {
    vectorStore.delete(List.of(id));
}

// 删除所有
public void deleteAll() {
    vectorStore.delete(List.of());
}
```

---

## 5. Document API

### 5.1 创建文档

```java
// 简单文档
Document doc = new Document("文档内容");

// 带元数据
Map<String, Object> metadata = new HashMap<>();
metadata.put("source", "file.txt");
metadata.put("page", 1);
Document doc = new Document("文档内容", metadata);

// 带ID
Document doc = new Document("id", "文档内容", metadata);
```

### 5.2 文档处理

```java
// 读取器
JsonReader jsonReader = new JsonReader(
    new FileSystemResource("data.json"),
    "content",
    "metadata"
);
List<Document> documents = jsonReader.get();

// 分割器
TokenTextSplitter splitter = new TokenTextSplitter(500, 50, 5, 10000);
List<Document> chunks = splitter.apply(documents);
```

---

## 6. Advisor API

### 6.1 内置 Advisor

```java
@Configuration
public class AdvisorConfig {

    // 重试 Advisor
    @Bean
    public RetryAdvisor retryAdvisor() {
        return new RetryAdvisor(
            3, // max attempts
            Duration.ofSeconds(1) // initial backoff
        );
    }

    // 向量搜索 Advisor
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

    // 日志 Advisor
    @Bean
    public RequestLoggerAdvisor requestLoggerAdvisor() {
        return new RequestLoggerAdvisor();
    }
}
```

### 6.2 自定义 Advisor

```java
public class CustomAdvisor implements RequestAdvisor {

    @Override
    public AdvisedRequest adviseRequest(AdvisedRequest request, Map<String, Object> context) {
        // 修改请求
        String userText = request.userText();
        String enhancedText = enhance(userText);

        return AdvisedRequest.from(request)
            .withUserText(enhancedText);
    }

    @Override
    public AdvisedResponse adviseResponse(AdvisedResponse response, Map<String, Object> context) {
        // 修改响应
        return response;
    }

    private String enhance(String text) {
        return "请详细回答：" + text;
    }
}
```

---

## 7. 工具调用 API

### 7.1 定义工具

```java
@Service
public class WeatherService implements Function<WeatherService.Request, WeatherService.Response> {

    public record Request(String location) {}
    public record Response(double temperature, String condition) {}

    @Override
    public Response apply(Request request) {
        // 调用天气 API
        return new Response(25.5, "晴朗");
    }
}
```

### 7.2 使用工具

```java
@Service
public class ToolService {
    private final ChatClient chatClient;
    private final WeatherService weatherService;

    public String queryWeather(String location) {
        return chatClient.prompt()
            .user("今天" + location + "的天气怎么样？")
            .functions(weatherService)
            .call()
            .content();
    }
}
```

---

## 8. Prompt API

### 8.1 PromptTemplate

```java
@Service
public class PromptService {

    public String createPrompt(String topic) {
        PromptTemplate template = new PromptTemplate("""
            请写一篇关于{topic}的文章。

            要求：
            1. 字数不少于500字
            2. 结构清晰
            3. 内容原创
            """);

        return template.create()
            .add("topic", topic)
            .getText();
    }
}
```

### 8.2 ChatClient 便捷方法

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

## 9. 流式 API

### 9.1 基础流式

```java
@Service
public class StreamService {
    private final ChatClient chatClient;

    public Flux<String> streamChat(String message) {
        return chatClient.prompt()
            .user(message)
            .stream()
            .content();
    }
}
```

### 9.2 WebFlux Controller

```java
@RestController
public class StreamController {

    @GetMapping(value = "/stream", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<String> stream(@RequestParam String message) {
        return chatClient.prompt()
            .user(message)
            .stream()
            .content();
    }
}
```

---

## 10. 异常处理 API

### 10.1 重试配置

```yaml
spring:
  ai:
    retry:
      max-attempts: 3
      backoff:
        initial-interval: 1000
        multiplier: 2
```

### 10.2 代码配置

```java
@Configuration
public class RetryConfig {

    @Bean
    public ChatClient chatClientWithRetry(ChatModel chatModel) {
        return ChatClient.builder(chatModel)
            .defaultOptions(OpenAiChatOptions.builder()
                .retryOptions(RetryOptions.builder()
                    .maxAttempts(3)
                    .backoff(Backoff.builder()
                        .initialInterval(Duration.ofSeconds(1))
                        .multiplier(2.0)
                        .build())
                    .build())
                .build())
            .build();
    }
}
```

---

## 11. 监控 API

### 11.1 指标收集

```java
@Configuration
public class MonitoringConfig {

    @Bean
    public RequestLoggerAdvisor requestLoggerAdvisor() {
        return new RequestLoggerAdvisor(); // 记录所有请求和响应
    }

    @Bean
    public MeterRegistry meterRegistry() {
        return new SimpleMeterRegistry();
    }
}
```

### 11.2 自定义监听器

```java
@Component
public class ChatListener {

    @EventListener
    public void onChatEvent(ChatEvent event) {
        log.info("Chat event: {}", event);
        // 发送到监控系统
    }
}
```

---

## 12. 常用代码片段

### 12.1 配置类

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
            .defaultAdvisors(
                new RetryAdvisor(3, Duration.ofSeconds(1)),
                new RequestLoggerAdvisor()
            )
            .build();
    }
}
```

### 12.2 Service 层

```java
@Service
public class AIService {

    private final ChatClient chatClient;
    private final VectorStore vectorStore;

    // 聊天
    public String chat(String message) {
        return chatClient.prompt()
            .user(message)
            .call()
            .content();
    }

    // RAG
    public String rag(String query) {
        List<Document> docs = vectorStore.similaritySearch(query, 3);
        String context = docs.stream()
            .map(Document::getContent)
            .collect(Collectors.joining("\n"));

        return chatClient.prompt()
            .user(String.format("上下文：%s\n问题：%s", context, query))
            .call()
            .content();
    }
}
```

### 12.3 Controller 层

```java
@RestController
@RequestMapping("/api/ai")
public class AiController {

    private final AIService aiService;

    @PostMapping("/chat")
    public String chat(@RequestBody String message) {
        return aiService.chat(message);
    }

    @PostMapping("/rag")
    public String rag(@RequestBody String query) {
        return aiService.rag(query);
    }

    @GetMapping("/stream")
    public Flux<String> stream(@RequestParam String message) {
        return aiService.stream(message);
    }
}
```

---

## 13. 最佳实践

### 13.1 资源管理

- 使用连接池管理 ChatModel
- 合理设置超时时间
- 实现请求限流

### 13.2 性能优化

- 批量处理 Embedding
- 缓存常用查询结果
- 使用流式响应减少延迟

### 13.3 安全建议

- API Key 使用环境变量
- 实现请求验证
- 记录访问日志

---

## 14. 相关资源

- **API 文档**: https://docs.spring.io/spring-ai/api/
- **示例代码**: https://github.com/spring-projects/spring-ai-examples
- **GitHub**: https://github.com/spring-projects/spring-ai

---

© 2025 Spring AI 技术文档
