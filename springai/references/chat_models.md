# Spring AI 聊天模型 API

**页数:** 8

---

## 1. 模型提供商配置

### 1.1 OpenAI

**依赖**

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
      chat:
        options:
          model: gpt-4o
          temperature: 0.7
          max-tokens: 2000
```

### 1.2 Azure OpenAI

**依赖**

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-azure-openai-spring-boot-starter</artifactId>
</dependency>
```

**配置**

```yaml
spring:
  ai:
    azure:
      openai:
        api-key: ${AZURE_OPENAI_API_KEY}
        endpoint: ${AZURE_OPENAI_ENDPOINT}
        deployment-name: ${AZURE_OPENAI_DEPLOYMENT_NAME}
```

### 1.3 Anthropic Claude

**依赖**

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-anthropic-spring-boot-starter</artifactId>
</dependency>
```

**配置**

```yaml
spring:
  ai:
    anthropic:
      api-key: ${ANTHROPIC_API_KEY}
      chat:
        options:
          model: claude-3-5-sonnet-20241022
          temperature: 0.7
```

### 1.4 Ollama（本地模型）

**依赖**

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-ollama-spring-boot-starter</artifactId>
</dependency>
```

**配置**

```yaml
spring:
  ai:
    ollama:
      base-url: http://localhost:11434
      chat:
        options:
          model: llama3.2
```

---

## 2. ChatClient API

### 2.1 基础对话

**简单调用**

```java
@Service
public class ChatService {
    private final ChatClient chatClient;

    public String chat(String message) {
        return chatClient.prompt()
            .user(message)
            .call()
            .content();
    }
}
```

**多轮对话**

```java
public String multiTurnChat(String userMessage) {
    return chatClient.prompt()
        .user(userMessage)
        .call()
        .content();
}

// 带历史记录
public String chatWithHistory(List<Message> history, String newMessage) {
    return chatClient.prompt()
        .messages(history)
        .user(newMessage)
        .call()
        .content();
}
```

### 2.2 系统提示

**设置系统角色**

```java
public String chatWithSystem(String userMessage) {
    return chatClient.prompt()
        .system("你是一个专业的Java技术顾问")
        .user(userMessage)
        .call()
        .content();
}
```

**动态系统提示**

```java
public String chatWithDynamicSystem(String role, String message) {
    return chatClient.prompt()
        .system("你是" + role + "，请用专业的态度回答")
        .user(message)
        .call()
        .content();
}
```

### 2.3 流式响应

**基础流式**

```java
public Flux<String> streamChat(String message) {
    return chatClient.prompt()
        .user(message)
        .stream()
        .content();
}
```

**WebFlux Controller**

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

### 2.4 结构化输出

**单个对象**

```java
public record User(String name, int age, String email);

public User extractUser(String description) {
    return chatClient.prompt()
        .user("从以下描述中提取用户信息: " + description)
        .call()
        .entity(User.class);
}
```

**对象列表**

```java
public List<User> extractUsers(String text) {
    return chatClient.prompt()
        .user("从文本中提取所有用户信息: " + text)
        .call()
        .entityList(User.class);
}
```

**自定义输出格式**

```java
public String getJsonResponse(String prompt) {
    return chatClient.prompt()
        .user(prompt)
        .options(OpenAiChatOptions.builder()
            .responseFormat(new ResponseFormat("json_object"))
            .build())
        .call()
        .content();
}
```

---

## 3. 函数调用

### 3.1 定义函数

**Java Function**

```java
@Service
public class WeatherService implements Function<WeatherService.Request, WeatherService.Response> {

    public record Request(String location, String unit) {}
    public record Response(double temperature, String condition, String humidity) {}

    @Override
    public Response apply(Request request) {
        // 调用实际天气API
        return new Response(25.5, "晴朗", "65%");
    }
}
```

### 3.2 启用函数调用

**单个函数**

```java
@Autowired
private WeatherService weatherService;

public String getWeather(String location) {
    return chatClient.prompt()
        .user("今天" + location + "的天气怎么样？")
        .functions(weatherService)
        .call()
        .content();
}
```

**多个函数**

```java
public String multiFunctionQuery(String query) {
    return chatClient.prompt()
        .user(query)
        .functions(weatherService, timeService, calculatorService)
        .call()
        .content();
}
```

### 3.3 函数选项

**自定义函数描述**

```java
@Bean
@Description("获取指定城市的天气信息")
public Function<WeatherRequest, WeatherResponse> weatherFunction() {
    return request -> {
        // 实现逻辑
        return new WeatherResponse(25.5, "晴朗");
    };
}
```

---

## 4. 多模态支持

### 4.1 图像理解

```java
@Service
public class VisionService {
    private final ChatClient chatClient;

    public String describeImage(String imageUrl) {
        return chatClient.prompt()
            .user(user -> user
                .text("请描述这张图片")
                .media(MediaClient.Builder.builder()
                    .url(imageUrl)
                    .build())
            )
            .call()
            .content();
    }
}
```

### 4.2 音频处理

**语音转文字**

```java
@Service
public class TranscriptionService {
    private final OpenAiAudioSpeechModel speechModel;

    public String transcribe(String audioFilePath) {
        Resource audioResource = new FileSystemResource(audioFilePath);
        return speechModel.call(new Prompt(audioResource)).getResult().getOutput().getText();
    }
}
```

**文字转语音**

```java
public byte[] textToSpeech(String text) {
    return speechModel.speech(SpeechPrompt.builder()
        .text(text)
        .voice(SpeechModel.AudioVoice.ALLOY)
        .responseFormat(SpeechModel.AudioResponseFormat.MP3)
        .build())
        .getResult()
        .getOutput();
}
```

---

## 5. 高级配置

### 5.1 Chat Options

**OpenAI 特定选项**

```java
public String chatWithOptions(String message) {
    return chatClient.prompt()
        .user(message)
        .options(OpenAiChatOptions.builder()
            .model("gpt-4o")
            .temperature(0.7)
            .maxTokens(2000)
            .topP(0.9)
            .frequencyPenalty(0.5)
            .presencePenalty(0.3)
            .build())
        .call()
        .content();
}
```

**常用选项**

| 参数 | 说明 | 范围 |
|------|------|------|
| `temperature` | 控制随机性 | 0.0-2.0 |
| `maxTokens` | 最大令牌数 | 1-128000 |
| `topP` | 核采样 | 0.0-1.0 |
| `frequencyPenalty` | 频率惩罚 | -2.0-2.0 |
| `presencePenalty` | 存在惩罚 | -2.0-2.0 |

### 5.2 动态选项

**运行时修改**

```java
public String chatWithDynamicOptions(String message, double temperature) {
    return chatClient.prompt()
        .user(message)
        .options(OpenAiChatOptions.builder()
            .temperature(temperature)
            .build())
        .call()
        .content();
}
```

### 5.3 默认选项

**全局配置**

```java
@Configuration
public class ChatConfig {

    @Bean
    public ChatClient chatClient(ChatModel chatModel) {
        return ChatClient.builder(chatModel)
            .defaultOptions(OpenAiChatOptions.builder()
                .model("gpt-4o")
                .temperature(0.7)
                .maxTokens(2000)
                .build())
            .build();
    }
}
```

---

## 6. 记忆管理

### 6.1 Chat Memory

**简单记忆**

```java
@Service
public class MemoryChatService {
    private final ChatClient chatClient;
    private final List<Message> history = new ArrayList<>();

    public String chat(String message) {
        // 添加用户消息
        history.add(new UserMessage(message));

        // 调用模型
        String response = chatClient.prompt()
            .messages(history)
            .call()
            .content();

        // 添加助手响应
        history.add(new AssistantMessage(response));

        return response;
    }
}
```

### 6.2 持久化记忆

**使用 ChatMemory Repository**

```java
@Service
public class PersistentChatService {
    private final ChatClient chatClient;
    private final ChatMemoryRepository chatMemoryRepository;

    public String chat(String sessionId, String message) {
        // 获取历史记录
        List<Message> history = chatMemoryRepository.findBySessionId(sessionId);

        // 添加新消息
        history.add(new UserMessage(message));

        // 调用模型
        String response = chatClient.prompt()
            .messages(history)
            .call()
            .content();

        // 保存历史
        history.add(new AssistantMessage(response));
        chatMemoryRepository.save(sessionId, history);

        return response;
    }
}
```

### 6.3 窗口记忆

**滑动窗口**

```java
public String chatWithWindow(String message, int windowSize) {
    // 只保留最近N条消息
    List<Message> recentHistory = history.stream()
        .skip(Math.max(0, history.size() - windowSize))
        .toList();

    return chatClient.prompt()
        .messages(recentHistory)
        .user(message)
        .call()
        .content();
}
```

---

## 7. 提示工程

### 7.1 Few-Shot Prompting

```java
public String fewShotPrompt(String task) {
    String prompt = """
        任务：根据用户描述分类

        示例1：
        描述：我的电脑无法启动
        分类：技术问题

        示例2：
        我想退款
        分类：账单问题

        现在请分类以下描述：
        %s
        """.formatted(task);

    return chatClient.prompt()
        .user(prompt)
        .call()
        .content();
}
```

### 7.2 Chain of Thought

```java
public String chainOfThought(String problem) {
    String prompt = """
        请一步步思考以下问题，展示你的推理过程：

        问题：%s

        请按照以下格式回答：
        1. 理解问题
        2. 分析关键信息
        3. 逐步推理
        4. 得出结论
        """.formatted(problem);

    return chatClient.prompt()
        .user(prompt)
        .call()
        .content();
}
```

### 7.3 ReAct 模式

```java
public String react(String query) {
    String prompt = """
        使用ReAct模式回答：

        Thought: [你的思考过程]
        Action: [要执行的动作]
        Observation: [动作的结果]
        ... (重复Thought/Action/Observation)
        Answer: [最终答案]

        问题：%s
        """.formatted(query);

    return chatClient.prompt()
        .user(prompt)
        .call()
        .content();
}
```

---

## 8. 错误处理和重试

### 8.1 自动重试

**配置重试**

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

**代码配置**

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

### 8.2 异常处理

```java
@Service
public class RobustChatService {

    public String safeChat(String message) {
        try {
            return chatClient.prompt()
                .user(message)
                .call()
                .content();
        } catch (RetryExhaustedException e) {
            log.error("重试失败: {}", e.getMessage());
            return "服务暂时不可用，请稍后再试";
        } catch (RateLimitExceededException e) {
            log.warn("速率限制: {}", e.getMessage());
            return "请求过于频繁，请稍后再试";
        } catch (Exception e) {
            log.error("未知错误: {}", e.getMessage());
            return "发生错误，请联系管理员";
        }
    }
}
```

---

## 9. 性能优化

### 9.1 批量处理

```java
public List<String> batchChat(List<String> messages) {
    return messages.parallelStream()
        .map(this::chat)
        .toList();
}
```

### 9.2 缓存

```java
@Cacheable(value = "responses", key = "#message")
public String cachedChat(String message) {
    return chatClient.prompt()
        .user(message)
        .call()
        .content();
}
```

### 9.3 连接池

```java
@Configuration
public class OpenAiConfig {

    @Bean
    public OpenAiChatModel openAiChatModel(OpenAiApi openAiApi) {
        return new OpenAiChatModel(
            openAiApi,
            OpenAiChatOptions.builder()
                .build()
        );
    }
}
```

---

## 10. 完整示例

### 10.1 智能客服

```java
@Service
public class CustomerServiceBot {

    private final ChatClient chatClient;
    private final VectorStore vectorStore;

    public String handleCustomerQuery(String query) {
        // 1. 检索相关知识
        List<Document> relevantDocs = vectorStore.similaritySearch(query, 3);

        // 2. 构建上下文
        String context = relevantDocs.stream()
            .map(Document::getContent)
            .collect(Collectors.joining("\n"));

        // 3. 生成回答
        return chatClient.prompt()
            .system("你是专业的客服代表，基于以下知识库回答客户问题")
            .user("""
                知识库：
                {context}

                客户问题：{query}

                请提供专业、友好的回答。
                """.formatted(context, query))
            .call()
            .content();
    }
}
```

---

© 2025 Spring AI 技术文档
