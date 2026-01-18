# Spring AI 内容审核

**页数:** 1

---

## 1. 概述

Spring AI 提供内容审核功能，用于检测和处理有害内容。

---

## 2. 依赖配置

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-openai-spring-boot-starter</artifactId>
</dependency>
```

---

## 3. 使用 API

```java
@Service
public class ModerationService {
    private final OpenAiModerationModel moderationModel;

    // 检查内容是否安全
    public boolean isSafe(String content) {
        Moderation moderation = moderationModel.call(new Prompt(content))
            .getResult()
            .getOutput();

        return !moderation.isFlagged();
    }

    // 获取详细审核结果
    public ModerationResult checkContent(String content) {
        Moderation moderation = moderationModel.call(new Prompt(content))
            .getResult()
            .getOutput();

        return new ModerationResult(
            moderation.isFlagged(),
            moderation.getCategories(),
            moderation.getCategoryScores()
        );
    }
}
```

---

## 4. 审核类别

- **hate**: 仇恨内容
- **hate/threatening**: 仇恨威胁
- **self-harm**: 自残
- **sexual**: 色情
- **violence**: 暴力

---

## 5. 完整示例

```java
@Service
public class ContentFilterService {

    private final ModerationService moderationService;

    public void processContent(String content) {
        if (!moderationService.isSafe(content)) {
            throw new ContentPolicyException("内容违反社区规范");
        }

        // 处理安全内容
        processSafeContent(content);
    }
}
```

---

© 2025 Spring AI 技术文档
