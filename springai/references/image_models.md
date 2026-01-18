# Spring AI 图像模型 API

**页数:** 2

---

## 1. 依赖配置

### 1.1 OpenAI DALL-E

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
      image:
        options:
          model: dall-e-3
          n: 1
          size: 1024x1024
```

### 1.2 Stability AI

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-stability-ai-spring-boot-starter</artifactId>
</dependency>
```

---

## 2. 图像生成 API

### 2.1 基础使用

```java
@Service
public class ImageService {
    private final OpenAiImageModel imageModel;

    public String generateImage(String prompt) {
        ImagePrompt imagePrompt = new ImagePrompt(prompt);

        ImageResponse response = imageModel.call(imagePrompt);
        ImageGeneration result = response.getResult();

        return result.getOutput().getUrl();
    }
}
```

### 2.2 带选项生成

```java
public String generateImageWithOptions(String prompt) {
    ImageOptions options = OpenAiImageOptions.builder()
        .model("dall-e-3")
        .quality("hd")
        .style("vivid")
        .n(1)
        .size("1024x1024")
        .build();

    ImagePrompt imagePrompt = new ImagePrompt(prompt, options);

    return imageModel.call(imagePrompt)
        .getResult()
        .getOutput()
        .getUrl();
}
```

---

## 3. 图像编辑

```java
public String editImage(String originalPath, String maskPath, String prompt) {
    Resource original = new FileSystemResource(originalPath);
    Resource mask = new FileSystemResource(maskPath);

    ImageEditRequest editRequest = new ImageEditRequest(
        original, mask, prompt, "dall-e-2", "1024x1024", 1
    );

    return imageModel.edit(editRequest)
        .getResult()
        .getOutput()
        .getUrl();
}
```

---

## 4. 图像变体

```java
public String createVariation(String imagePath) {
    Resource image = new FileSystemResource(imagePath);

    ImageVariationRequest request = new ImageVariationRequest(
        image, "dall-e-2", "1024x1024", 1
    );

    return imageModel.variation(request)
        .getResult()
        .getOutput()
        .getUrl();
}
```

---

## 5. 完整示例

```java
@RestController
@RequestMapping("/api/images")
public class ImageController {

    @PostMapping("/generate")
    public String generate(@RequestParam String prompt) {
        return imageService.generateImage(prompt);
    }

    @PostMapping("/edit")
    public String edit(
        @RequestParam String original,
        @RequestParam String mask,
        @RequestParam String prompt) {
        return imageService.editImage(original, mask, prompt);
    }

    @PostMapping("/variation")
    public String variation(@RequestParam String imagePath) {
        return imageService.createVariation(imagePath);
    }
}
```

---

## 6. 支持的尺寸

| 模型 | 支持尺寸 |
|------|---------|
| dall-e-3 | 1024x1024, 1792x1024, 1024x1792 |
| dall-e-2 | 256x256, 512x512, 1024x1024 |

---

© 2025 Spring AI 技术文档
