# Spring AI 其他功能

**页数:** 2

---

## 1. MCP (模型上下文协议)

### 1.1 概述

MCP 允许模型通过标准化接口与外部工具和服务交互。

### 1.2 使用示例

```java
@Configuration
public class McpConfig {

    @Bean
    public ChatClient chatClientWithMcp(ChatModel chatModel) {
        return ChatClient.builder(chatModel)
            .defaultFunctions(mcpFunctions())
            .build();
    }

    private List<Function> mcpFunctions() {
        return List.of(
            new FileSystemFunction(),
            new DatabaseFunction(),
            new WebSearchFunction()
        );
    }
}
```

---

## 2. 内容审核

### 2.1 依赖配置

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-openai-spring-boot-starter</artifactId>
</dependency>
```

### 2.2 使用 API

```java
@Service
public class ModerationService {
    private final OpenAiModerationModel moderationModel;

    public boolean isSafe(String content) {
        ModerationResponse response = moderationModel.call(
            new Prompt(content)
        );

        return response.getResult().getOutput().isFlagged() == false;
    }

    public Moderation checkContent(String content) {
        return moderationModel.call(new Prompt(content))
            .getResult()
            .getOutput();
    }
}
```

---

## 3. 可观测性

### 3.1 指标收集

```yaml
management:
  metrics:
    export:
      prometheus:
        enabled: true
  tracing:
    sampling:
      probability: 1.0
```

### 3.2 日志

```java
@Configuration
public class ObservabilityConfig {

    @Bean
    public RequestLoggerAdvisor requestLoggerAdvisor() {
        return new RequestLoggerAdvisor();
    }

    @Bean
    public MeterRegistry meterRegistry() {
        return new PrometheusMeterRegistry(PrometheusConfig.DEFAULT);
    }
}
```

---

## 4. 完整示例

```java
@Service
public class SafeChatService {

    private final ChatClient chatClient;
    private final ModerationService moderationService;

    public String safeChat(String message) {
        // 1. 内容审核
        if (!moderationService.isSafe(message)) {
            throw new IllegalArgumentException("内容不合规");
        }

        // 2. 执行聊天
        return chatClient.prompt()
            .user(message)
            .call()
            .content();
    }
}
```

---

© 2025 Spring AI 技术文档
