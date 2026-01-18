# Spring AI 音频处理 API

**页数:** 2

---

## 1. 语音转文字 (STT)

### 1.1 依赖配置

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
      audio:
        transcription:
          options:
            model: whisper-1
```

### 1.2 使用 API

```java
@Service
public class TranscriptionService {
    private final OpenAiAudioTranscriptionModel transcriptionModel;

    public String transcribe(String audioFilePath) {
        Resource audioResource = new FileSystemResource(audioFilePath);

        return transcriptionModel.call(new Prompt(audioResource))
            .getResult()
            .getOutput()
            .getText();
    }

    // 带选项
    public String transcribeWithOptions(String audioFilePath, String language) {
        Resource audioResource = new FileSystemResource(audioFilePath);

        TranscriptionOptions options = TranscriptionOptions.builder()
            .language(language)
            .prompt("这是一个关于技术的讨论")
            .build();

        return transcriptionModel.call(audioResource, options)
            .getResult()
            .getOutput()
            .getText();
    }
}
```

---

## 2. 文字转语音 (TTS)

### 2.1 依赖配置

```yaml
spring:
  ai:
    openai:
      audio:
        speech:
          options:
            model: tts-1
            voice: alloy
```

### 2.2 使用 API

```java
@Service
public class SpeechService {
    private final OpenAiAudioSpeechModel speechModel;

    public byte[] textToSpeech(String text) {
        return speechModel.speech(SpeechPrompt.builder()
            .text(text)
            .voice(SpeechModel.AudioVoice.ALLOY)
            .responseFormat(SpeechModel.AudioResponseFormat.MP3)
            .build())
            .getResult()
            .getOutput();
    }

    // 保存到文件
    public void saveSpeech(String text, String outputPath) {
        byte[] audio = textToSpeech(text);
        try (FileOutputStream fos = new FileOutputStream(outputPath)) {
            fos.write(audio);
        }
    }
}
```

---

## 3. 支持的声音

```java
// 可用声音
ALLOY, ECHO, FABLE, ONYX, NOVA, SHIMMER

// 使用示例
byte[] audio = speechModel.speech(SpeechPrompt.builder()
    .text("Hello")
    .voice(SpeechModel.AudioVoice.NOVA)
    .build())
.getResult()
.getOutput();
```

---

## 4. 完整示例

```java
@RestController
@RequestMapping("/api/audio")
public class AudioController {

    @PostMapping("/transcribe")
    public String transcribe(@RequestParam String audioPath) {
        return transcriptionService.transcribe(audioPath);
    }

    @GetMapping(value = "/speak", produces = MediaType.APPLICATION_OCTET_STREAM_VALUE)
    public byte[] speech(@RequestParam String text) {
        return speechService.textToSpeech(text);
    }
}
```

---

© 2025 Spring AI 技术文档
