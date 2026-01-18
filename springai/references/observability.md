# Spring AI 可观测性

**页数:** 1

---

## 1. 概述

Spring AI 提供完善的可观测性支持，包括指标收集、日志记录和链路追踪。

---

## 2. 指标收集

### 2.1 配置

```yaml
# application.yml
management:
  metrics:
    export:
      prometheus:
        enabled: true
    tags:
      application: ${spring.application.name}
```

### 2.2 内置指标

```java
@Configuration
public class MetricsConfig {

    @Bean
    public MeterRegistryCustomizer<MeterRegistry> metricsCommonTags() {
        return registry -> registry.config()
            .commonTags("application", "spring-ai-app");
    }

    @Bean
    public TimedAspect timedAspect(MeterRegistry registry) {
        return new TimedAspect(registry);
    }
}
```

---

## 3. 日志记录

### 3.1 请求日志

```java
@Configuration
public class LoggingConfig {

    @Bean
    public RequestLoggerAdvisor requestLoggerAdvisor() {
        return new RequestLoggerAdvisor();
    }

    @Bean
    public LoggingAdvisor loggingAdvisor() {
        return new LoggingAdvisor();
    }
}
```

### 3.2 自定义日志

```java
@Aspect
@Component
public class ChatAspect {

    @Around("execution(* org.springframework.ai.chat.client.ChatClient.*(..))")
    public Object logChat(ProceedingJoinPoint pjp) throws Throwable {
        long start = System.currentTimeMillis();
        log.info("Chat request started");

        try {
            Object result = pjp.proceed();
            long duration = System.currentTimeMillis() - start;

            log.info("Chat request completed in {}ms", duration);
            return result;
        } catch (Exception e) {
            log.error("Chat request failed", e);
            throw e;
        }
    }
}
```

---

## 4. 链路追踪

### 4.1 配置

```yaml
# application.yml
management:
  tracing:
    sampling:
      probability: 1.0
  zipkin:
    tracing:
      endpoint: http://localhost:9411/api/v2/spans
```

### 4.2 自定义追踪

```java
@Service
public class TracedChatService {

    private final ChatClient chatClient;
    private final ObservationRegistry observationRegistry;

    public String chat(String message) {
        return Observation.createNotStarted("spring.ai.chat", observationRegistry)
            .observe(() -> {
                return chatClient.prompt()
                    .user(message)
                    .call()
                    .content();
            });
    }
}
```

---

## 5. 完整示例

```java
@Configuration
public class ObservabilityConfig {

    @Bean
    public ChatClient observableChatClient(ChatModel chatModel) {
        return ChatClient.builder(chatModel)
            .defaultAdvisors(
                new RequestLoggerAdvisor(),
                new LoggingAdvisor()
            )
            .build();
    }
}
```

---

© 2025 Spring AI 技术文档
