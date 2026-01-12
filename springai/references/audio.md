# Springai - Audio

**Pages:** 11

---

## Transcription API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/audio/transcriptions.html

**Contents:**
- Transcription API
- Supported Providers
- Common Interface
  - TranscriptionModel
  - AudioTranscriptionPrompt
  - AudioTranscriptionResponse
- Writing Provider-Agnostic Code
  - Basic Service Example
- Provider-Specific Features

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI provides a unified API for Speech-to-Text transcription through the TranscriptionModel interface. This allows you to write portable code that works across different transcription providers.

Azure OpenAI Whisper API

All transcription providers implement the following shared interface:

The TranscriptionModel interface provides methods for converting audio to text:

The AudioTranscriptionPrompt class encapsulates the input audio and options:

The AudioTranscriptionResponse class contains the transcribed text and metadata:

One of the key benefits of the shared transcription interface is the ability to write code that works with any transcription provider without modification. The actual provider (OpenAI, Azure OpenAI, etc.) is determined by your Spring Boot configuration, allowing you to switch providers without changing application code.

The shared interface allows you to write code that works with any transcription provider:

This service works seamlessly with OpenAI, Azure OpenAI, or any other transcription provider, with the actual implementation determined by your Spring Boot configuration.

While the shared interface provides portability, each provider also offers specific features through provider-specific options classes (e.g., OpenAiAudioTranscriptionOptions, AzureOpenAiAudioTranscriptionOptions). These classes implement the AudioTranscriptionOptions interface while adding provider-specific capabilities.

For detailed information about provider-specific features, see the individual provider documentation pages.

**Examples:**

Example 1 (java):
```java
public interface TranscriptionModel extends Model<AudioTranscriptionPrompt, AudioTranscriptionResponse> {

    /**
     * Transcribes the audio from the given prompt.
     */
    AudioTranscriptionResponse call(AudioTranscriptionPrompt transcriptionPrompt);

    /**
     * A convenience method for transcribing an audio resource.
     */
    default String transcribe(Resource resource) {
        AudioTranscriptionPrompt prompt = new AudioTranscriptionPrompt(resource);
        return this.call(prompt).getResult().getOutput();
    }

    /**
     * A convenience method for transcribing an audio resource with options.
     */
    default String transcribe(Resource resource, AudioTranscriptionOptions options) {
        AudioTranscriptionPrompt prompt = new AudioTranscriptionPrompt(resource, options);
        return this.call(prompt).getResult().getOutput();
    }
}
```

Example 2 (java):
```java
Resource audioFile = new FileSystemResource("/path/to/audio.mp3");
AudioTranscriptionPrompt prompt = new AudioTranscriptionPrompt(
    audioFile,
    options
);
```

Example 3 (java):
```java
AudioTranscriptionResponse response = model.call(prompt);
String transcribedText = response.getResult().getOutput();
AudioTranscriptionResponseMetadata metadata = response.getMetadata();
```

Example 4 (java):
```java
@Service
public class TranscriptionService {

    private final TranscriptionModel transcriptionModel;

    public TranscriptionService(TranscriptionModel transcriptionModel) {
        this.transcriptionModel = transcriptionModel;
    }

    public String transcribeAudio(Resource audioFile) {
        return transcriptionModel.transcribe(audioFile);
    }

    public String transcribeWithOptions(Resource audioFile, AudioTranscriptionOptions options) {
        AudioTranscriptionPrompt prompt = new AudioTranscriptionPrompt(audioFile, options);
        AudioTranscriptionResponse response = transcriptionModel.call(prompt);
        return response.getResult().getOutput();
    }
}
```

---

## ElevenLabs Text-to-Speech (TTS) :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/audio/speech/elevenlabs-speech.html

**Contents:**
- ElevenLabs Text-to-Speech (TTS)
- Introduction
- Prerequisites
- Auto-configuration
- Speech Properties
  - Connection Properties
  - Configuration Properties
- Runtime Options
  - Using Voice Settings
- Manual Configuration

ElevenLabs provides natural-sounding speech synthesis software using deep learning. Its AI audio models generate realistic, versatile, and contextually-aware speech, voices, and sound effects across 32 languages. The ElevenLabs Text-to-Speech API enables users to bring any book, article, PDF, newsletter, or text to life with ultra-realistic AI narration.

Create an ElevenLabs account and obtain an API key. You can sign up at the ElevenLabs signup page. Your API key can be found on your profile page after logging in.

Add the spring-ai-elevenlabs dependency to your project’s build file. For more information, refer to the Dependency Management section.

Spring AI provides Spring Boot auto-configuration for the ElevenLabs Text-to-Speech Client. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

The prefix spring.ai.elevenlabs is used as the property prefix for all ElevenLabs related configurations (both connection and TTS specific settings). This is defined in ElevenLabsConnectionProperties.

spring.ai.elevenlabs.base-url

The base URL for the ElevenLabs API.

spring.ai.elevenlabs.api-key

Your ElevenLabs API key.

Enabling and disabling of the audio speech auto-configurations are now configured via top level properties with the prefix spring.ai.model.audio.speech.

To enable, spring.ai.model.audio.speech=elevenlabs (It is enabled by default)

To disable, spring.ai.model.audio.speech=none (or any value which doesn’t match elevenlabs)

This change is done to allow configuration of multiple models.

The prefix spring.ai.elevenlabs.tts is used as the property prefix to configure the ElevenLabs Text-to-Speech client, specifically. This is defined in ElevenLabsSpeechProperties.

spring.ai.model.audio.speech

Enable Audio Speech Model

spring.ai.elevenlabs.tts.options.model-id

The ID of the model to use.

spring.ai.elevenlabs.tts.options.voice-id

The ID of the voice to use. This is the voice ID, not the voice name.

spring.ai.elevenlabs.tts.options.output-format

The output format for the generated audio. See Output Formats below.

MP3, 22.05 kHz, 32 kbps

MP3, 44.1 kHz, 32 kbps

MP3, 44.1 kHz, 64 kbps

MP3, 44.1 kHz, 96 kbps

MP3, 44.1 kHz, 128 kbps

MP3, 44.1 kHz, 192 kbps

Opus, 48 kHz, 32 kbps

Opus, 48 kHz, 64 kbps

Opus, 48 kHz, 96 kbps

Opus, 48 kHz, 128 kbps

Opus, 48 kHz, 192 kbps

The ElevenLabsTextToSpeechOptions class provides options to use when making a text-to-speech request. On start-up, the options specified by spring.ai.elevenlabs.tts are used, but you can override these at runtime. The following options are available:

modelId: The ID of the model to use.

voiceId: The ID of the voice to use.

outputFormat: The output format of the generated audio.

voiceSettings: An object containing voice settings such as stability, similarityBoost, style, useSpeakerBoost, and speed.

enableLogging: A boolean to enable or disable logging.

languageCode: The language code of the input text (e.g., "en" for English).

pronunciationDictionaryLocators: A list of pronunciation dictionary locators.

seed: A seed for random number generation, for reproducibility.

previousText: Text before the main text, for context in multi-turn conversations.

nextText: Text after the main text, for context in multi-turn conversations.

previousRequestIds: Request IDs from previous turns in a conversation.

nextRequestIds: Request IDs for subsequent turns in a conversation.

applyTextNormalization: Apply text normalization ("auto", "on", or "off").

applyLanguageTextNormalization: Apply language text normalization.

You can customize the voice output by providing VoiceSettings in the options. This allows you to control properties like stability and similarity.

Add the spring-ai-elevenlabs dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

Next, create an ElevenLabsTextToSpeechModel:

The ElevenLabs Speech API supports real-time audio streaming using chunk transfer encoding. This allows audio playback to begin before the entire audio file is generated.

The ElevenLabs Voices API allows you to retrieve information about available voices, their settings, and default voice settings. You can use this API to discover the `voiceId`s to use in your speech requests.

To use the Voices API, you’ll need to create an instance of ElevenLabsVoicesApi:

You can then use the following methods:

getVoices(): Retrieves a list of all available voices.

getDefaultVoiceSettings(): Gets the default settings for voices.

getVoiceSettings(String voiceId): Returns the settings for a specific voice.

getVoice(String voiceId): Returns metadata about a specific voice.

The ElevenLabsTextToSpeechModelIT.java test provides some general examples of how to use the library.

The ElevenLabsApiIT.java test provides examples of using the low-level ElevenLabsApi.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-elevenlabs</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-elevenlabs'
}
```

Example 3 (java):
```java
ElevenLabsTextToSpeechOptions speechOptions = ElevenLabsTextToSpeechOptions.builder()
    .model("eleven_multilingual_v2")
    .voiceId("your_voice_id")
    .outputFormat(ElevenLabsApi.OutputFormat.MP3_44100_128.getValue())
    .build();

TextToSpeechPrompt speechPrompt = new TextToSpeechPrompt("Hello, this is a text-to-speech example.", speechOptions);
TextToSpeechResponse response = elevenLabsTextToSpeechModel.call(speechPrompt);
```

Example 4 (java):
```java
var voiceSettings = new ElevenLabsApi.SpeechRequest.VoiceSettings(0.75f, 0.75f, 0.0f, true);

ElevenLabsTextToSpeechOptions speechOptions = ElevenLabsTextToSpeechOptions.builder()
    .model("eleven_multilingual_v2")
    .voiceId("your_voice_id")
    .voiceSettings(voiceSettings)
    .build();

TextToSpeechPrompt speechPrompt = new TextToSpeechPrompt("This is a test with custom voice settings!", speechOptions);
TextToSpeechResponse response = elevenLabsTextToSpeechModel.call(speechPrompt);
```

---

## Text-To-Speech (TTS) API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/audio/speech.html

**Contents:**
- Text-To-Speech (TTS) API
- Supported Providers
- Common Interface
  - TextToSpeechModel
  - StreamingTextToSpeechModel
  - TextToSpeechPrompt
  - TextToSpeechResponse
- Writing Provider-Agnostic Code
  - Basic Service Example
  - Advanced Example: Multi-Provider Support

Spring AI provides a unified API for Text-To-Speech (TTS) through the TextToSpeechModel and StreamingTextToSpeechModel interfaces. This allows you to write portable code that works across different TTS providers.

Eleven Labs Text-To-Speech API

All TTS providers implement the following shared interfaces:

The TextToSpeechModel interface provides methods for converting text to speech:

The StreamingTextToSpeechModel interface provides methods for streaming audio in real-time:

The TextToSpeechPrompt class encapsulates the input text and options:

The TextToSpeechResponse class contains the generated audio and metadata:

One of the key benefits of the shared TTS interfaces is the ability to write code that works with any TTS provider without modification. The actual provider (OpenAI, ElevenLabs, etc.) is determined by your Spring Boot configuration, allowing you to switch providers without changing application code.

The shared interfaces allow you to write code that works with any TTS provider:

This service works seamlessly with OpenAI, ElevenLabs, or any other TTS provider, with the actual implementation determined by your Spring Boot configuration.

You can build applications that support multiple TTS providers simultaneously:

The shared interfaces also support streaming for real-time audio generation:

Building a REST API with provider-agnostic TTS:

Switch between providers using Spring profiles or properties:

Then activate the desired provider:

For maximum portability, use only the common TextToSpeechOptions interface methods:

When you need provider-specific features, you can still use them while maintaining a portable codebase:

Depend on Interfaces: Always inject TextToSpeechModel rather than concrete implementations

Use Common Options: Stick to TextToSpeechOptions interface methods for maximum portability

Handle Metadata Gracefully: Different providers return different metadata; handle it generically

Test with Multiple Providers: Ensure your code works with at least two TTS providers

Document Provider Assumptions: If you rely on specific provider behavior, document it clearly

While the shared interfaces provide portability, each provider also offers specific features through provider-specific options classes (e.g., OpenAiAudioSpeechOptions, ElevenLabsSpeechOptions). These classes implement the TextToSpeechOptions interface while adding provider-specific capabilities.

**Examples:**

Example 1 (java):
```java
public interface TextToSpeechModel extends Model<TextToSpeechPrompt, TextToSpeechResponse>, StreamingTextToSpeechModel {

    /**
     * Converts text to speech with default options.
     */
    default byte[] call(String text) {
        // Default implementation
    }

    /**
     * Converts text to speech with custom options.
     */
    TextToSpeechResponse call(TextToSpeechPrompt prompt);

    /**
     * Returns the default options for this model.
     */
    default TextToSpeechOptions getDefaultOptions() {
        // Default implementation
    }
}
```

Example 2 (java):
```java
@FunctionalInterface
public interface StreamingTextToSpeechModel extends StreamingModel<TextToSpeechPrompt, TextToSpeechResponse> {

    /**
     * Streams text-to-speech responses with metadata.
     */
    Flux<TextToSpeechResponse> stream(TextToSpeechPrompt prompt);

    /**
     * Streams audio bytes for the given text.
     */
    default Flux<byte[]> stream(String text) {
        // Default implementation
    }
}
```

Example 3 (java):
```java
TextToSpeechPrompt prompt = new TextToSpeechPrompt(
    "Hello, this is a text-to-speech example.",
    options
);
```

Example 4 (java):
```java
TextToSpeechResponse response = model.call(prompt);
byte[] audioBytes = response.getResult().getOutput();
TextToSpeechResponseMetadata metadata = response.getMetadata();
```

---

## Text-To-Speech (TTS) API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/audio/speech.html

**Contents:**
- Text-To-Speech (TTS) API

For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI provides support for OpenAI’s Speech API. When additional providers for Speech are implemented, a common SpeechModel and StreamingSpeechModel interface will be extracted.

---

## Text-To-Speech (TTS) API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/audio/speech.html

**Contents:**
- Text-To-Speech (TTS) API
- Supported Providers
- Common Interface
  - TextToSpeechModel
  - StreamingTextToSpeechModel
  - TextToSpeechPrompt
  - TextToSpeechResponse
- Writing Provider-Agnostic Code
  - Basic Service Example
  - Advanced Example: Multi-Provider Support

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI provides a unified API for Text-To-Speech (TTS) through the TextToSpeechModel and StreamingTextToSpeechModel interfaces. This allows you to write portable code that works across different TTS providers.

Eleven Labs Text-To-Speech API

All TTS providers implement the following shared interfaces:

The TextToSpeechModel interface provides methods for converting text to speech:

The StreamingTextToSpeechModel interface provides methods for streaming audio in real-time:

The TextToSpeechPrompt class encapsulates the input text and options:

The TextToSpeechResponse class contains the generated audio and metadata:

One of the key benefits of the shared TTS interfaces is the ability to write code that works with any TTS provider without modification. The actual provider (OpenAI, ElevenLabs, etc.) is determined by your Spring Boot configuration, allowing you to switch providers without changing application code.

The shared interfaces allow you to write code that works with any TTS provider:

This service works seamlessly with OpenAI, ElevenLabs, or any other TTS provider, with the actual implementation determined by your Spring Boot configuration.

You can build applications that support multiple TTS providers simultaneously:

The shared interfaces also support streaming for real-time audio generation:

Building a REST API with provider-agnostic TTS:

Switch between providers using Spring profiles or properties:

Then activate the desired provider:

For maximum portability, use only the common TextToSpeechOptions interface methods:

When you need provider-specific features, you can still use them while maintaining a portable codebase:

Depend on Interfaces: Always inject TextToSpeechModel rather than concrete implementations

Use Common Options: Stick to TextToSpeechOptions interface methods for maximum portability

Handle Metadata Gracefully: Different providers return different metadata; handle it generically

Test with Multiple Providers: Ensure your code works with at least two TTS providers

Document Provider Assumptions: If you rely on specific provider behavior, document it clearly

While the shared interfaces provide portability, each provider also offers specific features through provider-specific options classes (e.g., OpenAiAudioSpeechOptions, ElevenLabsSpeechOptions). These classes implement the TextToSpeechOptions interface while adding provider-specific capabilities.

**Examples:**

Example 1 (java):
```java
public interface TextToSpeechModel extends Model<TextToSpeechPrompt, TextToSpeechResponse>, StreamingTextToSpeechModel {

    /**
     * Converts text to speech with default options.
     */
    default byte[] call(String text) {
        // Default implementation
    }

    /**
     * Converts text to speech with custom options.
     */
    TextToSpeechResponse call(TextToSpeechPrompt prompt);

    /**
     * Returns the default options for this model.
     */
    default TextToSpeechOptions getDefaultOptions() {
        // Default implementation
    }
}
```

Example 2 (java):
```java
@FunctionalInterface
public interface StreamingTextToSpeechModel extends StreamingModel<TextToSpeechPrompt, TextToSpeechResponse> {

    /**
     * Streams text-to-speech responses with metadata.
     */
    Flux<TextToSpeechResponse> stream(TextToSpeechPrompt prompt);

    /**
     * Streams audio bytes for the given text.
     */
    default Flux<byte[]> stream(String text) {
        // Default implementation
    }
}
```

Example 3 (java):
```java
TextToSpeechPrompt prompt = new TextToSpeechPrompt(
    "Hello, this is a text-to-speech example.",
    options
);
```

Example 4 (java):
```java
TextToSpeechResponse response = model.call(prompt);
byte[] audioBytes = response.getResult().getOutput();
TextToSpeechResponseMetadata metadata = response.getMetadata();
```

---

## Upgrade Notes :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/upgrade-notes.html

**Contents:**
- Upgrade Notes
- Upgrading to 1.1.0-RC1
  - Breaking Changes
    - Text-to-Speech (TTS) API Migration
      - Removed Classes
      - Migration Steps
      - Benefits
      - Additional Resources
- Upgrading to 1.0.0-SNAPSHOT
  - Overview

The OpenAI Text-to-Speech implementation has been migrated from provider-specific classes to shared interfaces. This enables writing portable code that works across multiple TTS providers (OpenAI, ElevenLabs, and future providers).

The following deprecated classes have been removed from the org.springframework.ai.openai.audio.speech package:

SpeechModel → Use TextToSpeechModel (from org.springframework.ai.audio.tts)

StreamingSpeechModel → Use StreamingTextToSpeechModel (from org.springframework.ai.audio.tts)

SpeechPrompt → Use TextToSpeechPrompt (from org.springframework.ai.audio.tts)

SpeechResponse → Use TextToSpeechResponse (from org.springframework.ai.audio.tts)

SpeechMessage → Use TextToSpeechMessage (from org.springframework.ai.audio.tts)

Speech (in org.springframework.ai.openai.audio.speech) → Use Speech (from org.springframework.ai.audio.tts)

Additionally, the speed parameter type changed from Float to Double across all OpenAI TTS components for consistency with other TTS providers.

Update Imports: Replace all imports from org.springframework.ai.openai.audio.speech. with org.springframework.ai.audio.tts.

Update Type References: Replace all occurrences of the old class names with the new ones:

Update Speed Parameter: Change from Float to Double:

Update Dependency Injection: If you inject SpeechModel, update to TextToSpeechModel:

Portability: Write code once, switch between OpenAI, ElevenLabs, or other TTS providers easily

Consistency: Same patterns as ChatModel and other Spring AI abstractions

Type Safety: Improved type hierarchy with proper interface implementations

Future-Proof: New TTS providers will automatically work with your existing code

For a comprehensive migration guide with detailed code examples, see:

OpenAI TTS Migration Guide

Writing Provider-Agnostic TTS Code

The 1.0.0-SNAPSHOT version includes significant changes to artifact IDs, package names, and module structure. This section provides guidance specific to using the SNAPSHOT version.

To use the 1.0.0-SNAPSHOT version, you need to add the snapshot repositories to your build file. For detailed instructions, refer to the Snapshots - Add Snapshot Repositories section in the Getting Started guide.

Update your Spring AI BOM version to 1.0.0-SNAPSHOT in your build configuration. For detailed instructions on configuring dependency management, refer to the Dependency Management section in the Getting Started guide.

The 1.0.0-SNAPSHOT includes changes to artifact IDs, package names, and module structure.

For details, refer to: - Common Artifact ID Changes - Common Package Changes - Common Module Structure

You can automate the upgrade process to 1.0.0-RC1 using an OpenRewrite recipe. This recipe helps apply many of the necessary code changes for this version. Find the recipe and usage instructions at Arconia Spring AI Migrations.

The main changes that impact end user code are:

In VectorStoreChatMemoryAdvisor:

The constant CHAT_MEMORY_RETRIEVE_SIZE_KEY has been renamed to TOP_K.

The constant DEFAULT_CHAT_MEMORY_RESPONSE_SIZE (value: 100) has been renamed to DEFAULT_TOP_K with a new default value of 20.

The constant CHAT_MEMORY_CONVERSATION_ID_KEY has been renamed to CONVERSATION_ID and moved from AbstractChatMemoryAdvisor to the ChatMemory interface. Update your imports to use org.springframework.ai.chat.memory.ChatMemory.CONVERSATION_ID.

The built-in advisors that perform prompt augmentation have been updated to use self-contained templates. The goal is for each advisor to be able to perform templating operations without affecting nor being affected by templating and prompt decisions in other advisors.

If you were providing custom templates for the following advisors, you’ll need to update them to ensure all expected placeholders are included.

The QuestionAnswerAdvisor expects a template with the following placeholders (see more details):

a query placeholder to receive the user question.

a question_answer_context placeholder to receive the retrieved context.

The PromptChatMemoryAdvisor expects a template with the following placeholders (see more details):

an instructions placeholder to receive the original system message.

a memory placeholder to receive the retrieved conversation memory.

The VectorStoreChatMemoryAdvisor expects a template with the following placeholders (see more details):

an instructions placeholder to receive the original system message.

a long_term_memory placeholder to receive the retrieved conversation memory.

Refactored content observation to use logging instead of tracing (ca843e8)

Replaced content observation filters with logging handlers

Renamed configuration properties to better reflect their purpose:

include-prompt → log-prompt

include-completion → log-completion

include-query-response → log-query-response

Added TracingAwareLoggingObservationHandler for trace-aware logging

Replaced micrometer-tracing-bridge-otel with micrometer-tracing

Removed event-based tracing in favor of direct logging

Removed direct dependency on the OTel SDK

Renamed includePrompt to logPrompt in observation properties (in ChatClientBuilderProperties, ChatObservationProperties, and ImageObservationProperties)

We’ve standardized the naming pattern for chat memory components by adding the repository suffix throughout the codebase. This change affects Cassandra, JDBC, and Neo4j implementations, impacting artifact IDs, Java package names, and class names for clarity.

All memory-related artifacts now follow a consistent pattern:

spring-ai-model-chat-memory- → spring-ai-model-chat-memory-repository-

spring-ai-autoconfigure-model-chat-memory- → spring-ai-autoconfigure-model-chat-memory-repository-

spring-ai-starter-model-chat-memory- → spring-ai-starter-model-chat-memory-repository-

Package paths now include .repository. segment

Example: org.springframework.ai.chat.memory.jdbc → org.springframework.ai.chat.memory.repository.jdbc

Main autoconfiguration classes now use the Repository suffix

Example: JdbcChatMemoryAutoConfiguration → JdbcChatMemoryRepositoryAutoConfiguration

Configuration properties renamed from spring.ai.chat.memory.<storage>…​ to spring.ai.chat.memory.repository.<storage>…​

Migration Required: - Update your Maven/Gradle dependencies to use the new artifact IDs. - Update any imports, class references, or configuration that used the old package or class names.

MessageAggregator class has been moved from org.springframework.ai.chat.model package in the spring-ai-client-chat module to the spring-ai-model module (same package name)

The aggregateChatClientResponse method has been removed from MessageAggregator and moved to a new class ChatClientMessageAggregator in the org.springframework.ai.chat.client package

If you were directly using the aggregateChatClientResponse method from MessageAggregator, you need to use the new ChatClientMessageAggregator class instead:

Don’t forget to add the appropriate import:

The Watson AI model was removed as it was based on the older text generation that is considered outdated as there is a new chat generation model available. Hopefully Watson will reappear in a future version of Spring AI

Moonshot and Qianfan have been removed since they are not accessible from outside China. These have been moved to the Spring AI Community repository.

Removed HanaDB vector store autoconfiguration (f3b4624)

Removed CassandraChatMemory implementation (11e3c8f)

Simplified chat memory advisor hierarchy and removed deprecated API (848a3fd)

Removed deprecations in JdbcChatMemory (356a68f)

Refactored chat memory repository artifacts for clarity (2d517ee)

Refactored chat memory repository autoconfigurations and Spring Boot starters for clarity (f6dba1b)

Removed deprecated UserMessage constructors (06edee4)

Removed deprecated PromptTemplate constructors (722c77e)

Removed deprecated methods from Media (228ef10)

Refactored StTemplateRenderer: renamed supportStFunctions to validateStFunctions (0e15197)

Removed left over TemplateRender interface after moving it (52675d8)

Removed deprecations in ChatClient and Advisors (4fe74d8)

Removed deprecations from OllamaApi and AnthropicApi (46be898)

Removed inter-package dependency cycles in spring-ai-model (ebfa5b9)

Moved MessageAggregator to spring-ai-model module (54e5c07)

Removed unused json-path dependency in spring-ai-openai (9de13d1)

Added Entra ID identity management for Azure OpenAI with clean autoconfiguration (3dc86d3)

Removed all code deprecations (76bee8c) and (b6ce7f3)

You can automate the upgrade process to 1.0.0-M8 using an OpenRewrite recipe. This recipe helps apply many of the necessary code changes for this version. Find the recipe and usage instructions at Arconia Spring AI Migrations.

When upgrading from Spring AI 1.0 M7 to 1.0 M8, users who previously registered tool callbacks are encountering breaking changes that cause tool calling functionality to silently fail. This is specifically impacting code that used the deprecated tools() method.

Here’s an example of code that worked in M7 but no longer functions as expected in M8:

The solution is to use the toolSpecifications() method instead of the deprecated tools() method:

Removed CassandraChatMemory implementation (11e3c8f)

Simplified chat memory advisor hierarchy and removed deprecated API (848a3fd)

Removed deprecations in JdbcChatMemory (356a68f)

Refactored chat memory repository artifacts for clarity (2d517ee)

Refactored chat memory repository autoconfigurations and Spring Boot starters for clarity (f6dba1b)

Removed deprecations in ChatClient and Advisors (4fe74d8)

Breaking changes to chatclient tool calling (5b7849d)

Removed deprecations from OllamaApi and AnthropicApi (46be898)

Removed deprecated UserMessage constructors (06edee4)

Removed deprecated PromptTemplate constructors (722c77e)

Removed deprecated methods from Media (228ef10)

Refactored StTemplateRenderer: renamed supportStFunctions to validateStFunctions (0e15197)

Removed left over TemplateRender interface after moving it (52675d8)

Removed Watson text generation model (9e71b16)

Removed Qianfan code (bfcaad7)

Removed HanaDB vector store autoconfiguration (f3b4624)

Removed deepseek options from OpenAiApi (59b36d1)

Removed inter-package dependency cycles in spring-ai-model (ebfa5b9)

Moved MessageAggregator to spring-ai-model module (54e5c07)

Removed unused json-path dependency in spring-ai-openai (9de13d1)

Refactored content observation to use logging instead of tracing (ca843e8)

Replaced content observation filters with logging handlers

Renamed configuration properties to better reflect their purpose:

include-prompt → log-prompt

include-completion → log-completion

include-query-response → log-query-response

Added TracingAwareLoggingObservationHandler for trace-aware logging

Replaced micrometer-tracing-bridge-otel with micrometer-tracing

Removed event-based tracing in favor of direct logging

Removed direct dependency on the OTel SDK

Renamed includePrompt to logPrompt in observation properties (in ChatClientBuilderProperties, ChatObservationProperties, and ImageObservationProperties)

Added Entra ID identity management for Azure OpenAI with clean autoconfiguration (3dc86d3)

Removed all deprecations from 1.0.0-M8 (76bee8c)

General deprecation cleanup (b6ce7f3)

Spring AI 1.0.0-M7 is the last milestone release before the RC1 and GA releases. It introduces several important changes to artifact IDs, package names, and module structure that will be maintained in the final release.

The 1.0.0-M7 includes the same structural changes as 1.0.0-SNAPSHOT.

For details, refer to: - Common Artifact ID Changes - Common Package Changes - Common Module Structure

Spring AI 1.0.0-M7 now uses MCP Java SDK version 0.9.0, which includes significant changes from previous versions. If you’re using MCP in your applications, you’ll need to update your code to accommodate these changes.

ClientMcpTransport → McpClientTransport

ServerMcpTransport → McpServerTransport

DefaultMcpSession → McpClientSession or McpServerSession

All *Registration classes → *Specification classes

Use McpServerTransportProvider instead of ServerMcpTransport

All handlers now receive an exchange parameter as their first argument:

Methods previously available on the server are now accessed through the exchange object:

For a complete guide to migrating MCP code, refer to the MCP Migration Guide.

The previous configuration properties for enabling/disabling model auto-configuration have been removed:

spring.ai.<provider>.chat.enabled

spring.ai.<provider>.embedding.enabled

spring.ai.<provider>.image.enabled

spring.ai.<provider>.moderation.enabled

By default, if a model provider (e.g., OpenAI, Ollama) is found on the classpath, its corresponding auto-configuration for relevant model types (chat, embedding, etc.) is enabled. If multiple providers for the same model type are present (e.g., both spring-ai-openai-spring-boot-starter and spring-ai-ollama-spring-boot-starter), you can use the following properties to select which provider’s auto-configuration should be active, effectively disabling the others for that specific model type.

To disable auto-configuration for a specific model type entirely, even if only one provider is present, set the corresponding property to a value that does not match any provider on the classpath (e.g., none or disabled).

You can refer to the SpringAIModels enumeration for a list of well-known provider values.

spring.ai.model.audio.speech=<model-provider|none>

spring.ai.model.audio.transcription=<model-provider|none>

spring.ai.model.chat=<model-provider|none>

spring.ai.model.embedding=<model-provider|none>

spring.ai.model.embedding.multimodal=<model-provider|none>

spring.ai.model.embedding.text=<model-provider|none>

spring.ai.model.image=<model-provider|none>

spring.ai.model.moderation=<model-provider|none>

You can automate the upgrade process to 1.0.0-M7 using the Claude Code CLI tool with a provided prompt:

Download the Claude Code CLI tool

Copy the prompt from the update-to-m7.txt file

Paste the prompt into the Claude Code CLI

The AI will analyze your project and make the necessary changes

The naming pattern for Spring AI starter artifacts has changed. You’ll need to update your dependencies according to the following patterns:

Model starters: spring-ai-{model}-spring-boot-starter → spring-ai-starter-model-{model}

Vector Store starters: spring-ai-{store}-store-spring-boot-starter → spring-ai-starter-vector-store-{store}

MCP starters: spring-ai-mcp-{type}-spring-boot-starter → spring-ai-starter-mcp-{type}

The Spring AI autoconfiguration has changed from a single monolithic artifact to individual autoconfiguration artifacts per model, vector store, and other components. This change was made to minimize the impact of different versions of dependent libraries conflicting, such as Google Protocol Buffers, Google RPC, and others. By separating autoconfiguration into component-specific artifacts, you can avoid pulling in unnecessary dependencies and reduce the risk of version conflicts in your application.

The original monolithic artifact is no longer available:

Instead, each component now has its own autoconfiguration artifact following these patterns:

Model autoconfiguration: spring-ai-autoconfigure-model-{model}

Vector Store autoconfiguration: spring-ai-autoconfigure-vector-store-{store}

MCP autoconfiguration: spring-ai-autoconfigure-mcp-{type}

Your IDE should assist with refactoring to the new package locations.

KeywordMetadataEnricher and SummaryMetadataEnricher have moved from org.springframework.ai.transformer to org.springframework.ai.chat.transformer.

Content, MediaContent, and Media have moved from org.springframework.ai.model to org.springframework.ai.content.

The project has undergone significant changes to its module and artifact structure. Previously, spring-ai-core contained all central interfaces, but this has now been split into specialized domain modules to reduce unnecessary dependencies in your applications.

Base module with no dependencies on other Spring AI modules. Contains: - Core domain models (Document, TextSplitter) - JSON utilities and resource handling - Structured logging and observability support

Provides AI capability abstractions: - Interfaces like ChatModel, EmbeddingModel, and ImageModel - Message types and prompt templates - Function-calling framework (ToolDefinition, ToolCallback) - Content filtering and observation support

Unified vector database abstraction: - VectorStore interface for similarity search - Advanced filtering with SQL-like expressions - SimpleVectorStore for in-memory usage - Batching support for embeddings

High-level conversational AI APIs: - ChatClient interface - Conversation persistence via ChatMemory - Response conversion with OutputConverter - Advisor-based interception - Synchronous and reactive streaming support

Bridges chat with vector stores for RAG: - QuestionAnswerAdvisor: injects context into prompts - VectorStoreChatMemoryAdvisor: stores/retrieves conversation history

Apache Cassandra persistence for ChatMemory: - CassandraChatMemory implementation - Type-safe CQL with Cassandra’s QueryBuilder ==== spring-ai-model-chat-memory-neo4j

Neo4j graph database persistence for chat conversations.

Comprehensive framework for Retrieval Augmented Generation: - Modular architecture for RAG pipelines - RetrievalAugmentationAdvisor as main entry point - Functional programming principles with composable components

The dependency hierarchy can be summarized as:

spring-ai-commons (foundation)

spring-ai-model (depends on commons)

spring-ai-vector-store and spring-ai-client-chat (both depend on model)

spring-ai-advisors-vector-store and spring-ai-rag (depend on both client-chat and vector-store)

spring-ai-model-chat-memory-* modules (depend on client-chat)

The ToolContext class has been enhanced to support both explicit and implicit tool resolution. Tools can now be:

Explicitly Included: Tools that are explicitly requested in the prompt and included in the call to the model.

Implicitly Available: Tools that are made available for runtime dynamic resolution, but never included in any call to the model unless explicitly requested.

Starting with 1.0.0-M7, tools are only included in the call to the model if they are explicitly requested in the prompt or explicitly included in the call.

Additionally, the ToolContext class has now been marked as final and cannot be extended anymore. It was never supposed to be subclassed. You can add all the contextual data you need when instantiating a ToolContext, in the form of a Map<String, Object>. For more information, check the [documentation](docs.spring.io/spring-ai/reference/api/tools.html#_tool_context).

The Usage interface and its default implementation DefaultUsage have undergone the following changes:

getGenerationTokens() is now getCompletionTokens()

All token count fields in DefaultUsage changed from Long to Integer:

completionTokens (formerly generationTokens)

Replace all calls to getGenerationTokens() with getCompletionTokens()

Update DefaultUsage constructor calls:

While M6 maintains backward compatibility for JSON deserialization of the generationTokens field, this field will be removed in M7. Any persisted JSON documents using the old field name should be updated to use completionTokens.

Example of the new JSON format:

Each ChatModel instance, at construction time, accepts an optional ChatOptions or FunctionCallingOptions instance that can be used to configure default tools used for calling the model.

any tool passed via the functions() method of the default FunctionCallingOptions instance was included in each call to the model from that ChatModel instance, possibly overwritten by runtime options.

any tool passed via the functionCallbacks() method of the default FunctionCallingOptions instance was only made available for runtime dynamic resolution (see Tool Resolution), but never included in any call to the model unless explicitly requested.

any tool passed via the functions() method or the functionCallbacks() of the default FunctionCallingOptions instance is now handled in the same way: it is included in each call to the model from that ChatModel instance, possibly overwritten by runtime options. With that, there is consistency in the way tools are included in calls to the model and prevents any confusion due to a difference in behavior between functionCallbacks() and all the other options.

If you want to make a tool available for runtime dynamic resolution and include it in a chat request to the model only when explicitly requested, you can use one of the strategies described in Tool Resolution.

Starting 1.0.0-M6, Spring AI transitioned to using Amazon Bedrock’s Converse API for all Chat conversation implementations in Spring AI. All the Amazon Bedrock Chat models are removed except the Embedding models for Cohere and Titan.

Spring AI updates to use Spring Boot 3.4.2 for the dependency management. You can refer here for the dependencies managed by Spring Boot 3.4.2

If you are upgrading to Spring Boot 3.4.2, please make sure to refer to this documentation for the changes required to configure the REST Client. Notably, if you don’t have an HTTP client library on the classpath, this will likely result in the use of JdkClientHttpRequestFactory where SimpleClientHttpRequestFactory would have been used previously. To switch to use SimpleClientHttpRequestFactory, you need to set spring.http.client.factory=simple.

If you are using a different version of Spring Boot (say Spring Boot 3.3.x) and need a specific version of a dependency, you can override it in your build configuration.

In version 1.0.0-M6, the delete method in the VectorStore interface has been modified to be a void operation instead of returning an Optional<Boolean>. If your code previously checked the return value of the delete operation, you’ll need to remove this check. The operation now throws an exception if the deletion fails, providing more direct error handling.

Vector Builders have been refactored for consistency.

Current VectorStore implementation constructors have been deprecated, use the builder pattern.

VectorStore implementation packages have been moved into unique package names, avoiding conflicts across artifact. For example org.springframework.ai.vectorstore to org.springframework.ai.pgvector.vectorstore.

The type of the portable chat options (frequencyPenalty, presencePenalty, temperature, topP) has been changed from Float to Double.

The configuration prefix for the Chroma Vector Store has been changes from spring.ai.vectorstore.chroma.store to spring.ai.vectorstore.chroma in order to align with the naming conventions of other vector stores.

The default value of the initialize-schema property on vector stores capable of initializing a schema is now set to false. This implies that the applications now need to explicitly opt-in for schema initialization on supported vector stores, if the schema is expected to be created at application startup. Not all vector stores support this property. See the corresponding vector store documentation for more details. The following are the vector stores that currently don’t support the initialize-schema property.

In Bedrock Jurassic 2, the chat options countPenalty, frequencyPenalty, and presencePenalty have been renamed to countPenaltyOptions, frequencyPenaltyOptions, and presencePenaltyOptions. Furthermore, the type of the chat option stopSequences have been changed from String[] to List<String>.

In Azure OpenAI, the type of the chat options frequencyPenalty and presencePenalty has been changed from Double to Float, consistently with all the other implementations.

On our march to release 1.0.0 M1 we have made several breaking changes. Apologies, it is for the best!

A major change was made that took the 'old' ChatClient and moved the functionality into ChatModel. The 'new' ChatClient now takes an instance of ChatModel. This was done to support a fluent API for creating and executing prompts in a style similar to other client classes in the Spring ecosystem, such as RestClient, WebClient, and JdbcClient. Refer to the [JavaDoc](docs.spring.io/spring-ai/docs/api) for more information on the Fluent API, proper reference documentation is coming shortly.

We renamed the 'old' ModelClient to Model and renamed implementing classes, for example ImageClient was renamed to ImageModel. The Model implementation represents the portability layer that converts between the Spring AI API and the underlying AI Model API.

A new package model that contains interfaces and base classes to support creating AI Model Clients for any input/output data type combination. At the moment, the chat and image model packages implement this. We will be updating the embedding package to this new model soon.

A new "portable options" design pattern. We wanted to provide as much portability in the ModelCall as possible across different chat based AI Models. There is a common set of generation options and then those that are specific to a model provider. A sort of "duck typing" approach is used. ModelOptions in the model package is a marker interface indicating implementations of this class will provide the options for a model. See ImageOptions, a subinterface that defines portable options across all text→image ImageModel implementations. Then StabilityAiImageOptions and OpenAiImageOptions provide the options specific to each model provider. All options classes are created via a fluent API builder, all can be passed into the portable ImageModel API. These option data types are used in autoconfiguration/configuration properties for the ImageModel implementations.

Renamed POM artifact names: - spring-ai-qdrant → spring-ai-qdrant-store - spring-ai-cassandra → spring-ai-cassandra-store - spring-ai-pinecone → spring-ai-pinecone-store - spring-ai-redis → spring-ai-redis-store - spring-ai-qdrant → spring-ai-qdrant-store - spring-ai-gemfire → spring-ai-gemfire-store - spring-ai-azure-vector-store-spring-boot-starter → spring-ai-azure-store-spring-boot-starter - spring-ai-redis-spring-boot-starter → spring-ai-starter-vector-store-redis

Former spring-ai-vertex-ai has been renamed to spring-ai-vertex-ai-palm2 and spring-ai-vertex-ai-spring-boot-starter has been renamed to spring-ai-vertex-ai-palm2-spring-boot-starter.

So, you need to change the dependency from

and the related Boot starter for the Palm2 model has changed from

Renamed Classes (01.03.2024)

VertexAiApi → VertexAiPalm2Api

VertexAiClientChat → VertexAiPalm2ChatClient

VertexAiEmbeddingClient → VertexAiPalm2EmbeddingClient

VertexAiChatOptions → VertexAiPalm2ChatOptions

Moving the prompt and messages and metadata packages to subpackages of org.springframework.ai.chat

New functionality is text to image clients. Classes are OpenAiImageModel and StabilityAiImageModel. See the integration tests for usage, docs are coming soon.

A new package model that contains interfaces and base classes to support creating AI Model Clients for any input/output data type combination. At the moment, the chat and image model packages implement this. We will be updating the embedding package to this new model soon.

A new "portable options" design pattern. We wanted to provide as much portability in the ModelCall as possible across different chat based AI Models. There is a common set of generation options and then those that are specific to a model provider. A sort of "duck typing" approach is used. ModelOptions in the model package is a marker interface indicating implementations of this class will provide the options for a model. See ImageOptions, a subinterface that defines portable options across all text→image ImageModel implementations. Then StabilityAiImageOptions and OpenAiImageOptions provide the options specific to each model provider. All options classes are created via a fluent API builder, all can be passed into the portable ImageModel API. These option data types are used in autoconfiguration/configuration properties for the ImageModel implementations.

The following OpenAi Autoconfiguration chat properties have changed

from spring.ai.openai.model to spring.ai.openai.chat.options.model.

from spring.ai.openai.temperature to spring.ai.openai.chat.options.temperature.

Find updated documentation about the OpenAi properties: docs.spring.io/spring-ai/reference/api/chat/openai-chat.html

Merge SimplePersistentVectorStore and InMemoryVectorStore into SimpleVectorStore * Replace InMemoryVectorStore with SimpleVectorStore

Refactor the Ollama client and related classes and package names

Replace the org.springframework.ai.ollama.client.OllamaClient by org.springframework.ai.ollama.OllamaModelCall.

The OllamaChatClient method signatures have changed.

Rename the org.springframework.ai.autoconfigure.ollama.OllamaProperties into org.springframework.ai.model.ollama.autoconfigure.OllamaChatProperties and change the suffix to: spring.ai.ollama.chat. Some of the properties have changed as well.

Renaming of AiClient and related classes and package names

Rename AiClient to ChatClient

Rename AiResponse to ChatResponse

Rename AiStreamClient to StreamingChatClient

Rename package org.sf.ai.client to org.sf.ai.chat

Rename artifact ID of

transformers-embedding to spring-ai-transformers

Moved Maven modules from top-level directory and embedding-clients subdirectory to all be under a single models directory.

We are transitioning the project’s Group ID:

FROM: org.springframework.experimental.ai

TO: org.springframework.ai

Artifacts will still be hosted in the snapshot repository as shown below.

The main branch will move to the version 0.8.0-SNAPSHOT. It will be unstable for a week or two. Please use the 0.7.1-SNAPSHOT if you don’t want to be on the bleeding edge.

You can access 0.7.1-SNAPSHOT artifacts as before and still access 0.7.1-SNAPSHOT Documentation.

**Examples:**

Example 1 (yaml):
```yaml
Find:    SpeechModel
Replace: TextToSpeechModel

Find:    StreamingSpeechModel
Replace: StreamingTextToSpeechModel

Find:    SpeechPrompt
Replace: TextToSpeechPrompt

Find:    SpeechResponse
Replace: TextToSpeechResponse

Find:    SpeechMessage
Replace: TextToSpeechMessage
```

Example 2 (yaml):
```yaml
Find:    .speed(1.0f)
Replace: .speed(1.0)

Find:    Float speed
Replace: Double speed
```

Example 3 (java):
```java
// Before
public MyService(SpeechModel speechModel) { ... }

// After
public MyService(TextToSpeechModel textToSpeechModel) { ... }
```

Example 4 (java):
```java
// Before
new MessageAggregator().aggregateChatClientResponse(chatClientResponses, aggregationHandler);

// After
new ChatClientMessageAggregator().aggregateChatClientResponse(chatClientResponses, aggregationHandler);
```

---

## Transcription API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/audio/transcriptions.html

**Contents:**
- Transcription API
- Supported Providers
- Common Interface
  - TranscriptionModel
  - AudioTranscriptionPrompt
  - AudioTranscriptionResponse
- Writing Provider-Agnostic Code
  - Basic Service Example
- Provider-Specific Features

Spring AI provides a unified API for Speech-to-Text transcription through the TranscriptionModel interface. This allows you to write portable code that works across different transcription providers.

Azure OpenAI Whisper API

All transcription providers implement the following shared interface:

The TranscriptionModel interface provides methods for converting audio to text:

The AudioTranscriptionPrompt class encapsulates the input audio and options:

The AudioTranscriptionResponse class contains the transcribed text and metadata:

One of the key benefits of the shared transcription interface is the ability to write code that works with any transcription provider without modification. The actual provider (OpenAI, Azure OpenAI, etc.) is determined by your Spring Boot configuration, allowing you to switch providers without changing application code.

The shared interface allows you to write code that works with any transcription provider:

This service works seamlessly with OpenAI, Azure OpenAI, or any other transcription provider, with the actual implementation determined by your Spring Boot configuration.

While the shared interface provides portability, each provider also offers specific features through provider-specific options classes (e.g., OpenAiAudioTranscriptionOptions, AzureOpenAiAudioTranscriptionOptions). These classes implement the AudioTranscriptionOptions interface while adding provider-specific capabilities.

For detailed information about provider-specific features, see the individual provider documentation pages.

**Examples:**

Example 1 (java):
```java
public interface TranscriptionModel extends Model<AudioTranscriptionPrompt, AudioTranscriptionResponse> {

    /**
     * Transcribes the audio from the given prompt.
     */
    AudioTranscriptionResponse call(AudioTranscriptionPrompt transcriptionPrompt);

    /**
     * A convenience method for transcribing an audio resource.
     */
    default String transcribe(Resource resource) {
        AudioTranscriptionPrompt prompt = new AudioTranscriptionPrompt(resource);
        return this.call(prompt).getResult().getOutput();
    }

    /**
     * A convenience method for transcribing an audio resource with options.
     */
    default String transcribe(Resource resource, AudioTranscriptionOptions options) {
        AudioTranscriptionPrompt prompt = new AudioTranscriptionPrompt(resource, options);
        return this.call(prompt).getResult().getOutput();
    }
}
```

Example 2 (java):
```java
Resource audioFile = new FileSystemResource("/path/to/audio.mp3");
AudioTranscriptionPrompt prompt = new AudioTranscriptionPrompt(
    audioFile,
    options
);
```

Example 3 (java):
```java
AudioTranscriptionResponse response = model.call(prompt);
String transcribedText = response.getResult().getOutput();
AudioTranscriptionResponseMetadata metadata = response.getMetadata();
```

Example 4 (java):
```java
@Service
public class TranscriptionService {

    private final TranscriptionModel transcriptionModel;

    public TranscriptionService(TranscriptionModel transcriptionModel) {
        this.transcriptionModel = transcriptionModel;
    }

    public String transcribeAudio(Resource audioFile) {
        return transcriptionModel.transcribe(audioFile);
    }

    public String transcribeWithOptions(Resource audioFile, AudioTranscriptionOptions options) {
        AudioTranscriptionPrompt prompt = new AudioTranscriptionPrompt(audioFile, options);
        AudioTranscriptionResponse response = transcriptionModel.call(prompt);
        return response.getResult().getOutput();
    }
}
```

---

## Transcription API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/audio/transcriptions.html

**Contents:**
- Transcription API

For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI provides support for OpenAI’s Transcription API. When additional providers for Transcription are implemented, a common AudioTranscriptionModel interface will be extracted.

---

## Spring AI API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/index.html

**Contents:**
- Spring AI API
- Introduction
  - AI Model API
  - Vector Store API
  - Tool Calling API
  - Auto Configuration
  - ETL Data Engineering
- Feedback and Contributions

For the latest snapshot version, please use Spring AI 1.1.2!

The Spring AI API covers a wide range of functionalities. Each major feature is detailed in its own dedicated section. To provide an overview, the following key functionalities are available:

Portable Model API across AI providers for Chat, Text to Image, Audio Transcription, Text to Speech, and Embedding models. Both synchronous and stream API options are supported. Dropping down to access model specific features is also supported.

With support for AI Models from OpenAI, Microsoft, Amazon, Google, Amazon Bedrock, Hugging Face and more.

Portable Vector Store API across multiple providers, including a novel SQL-like metadata filter API that is also portable. Support for 14 vector databases are available.

Spring AI makes it easy to have the AI model invoke your services as @Tool-annotated methods or POJO java.util.Function objects.

Check the Spring AI Tool Calling documentation.

Spring Boot Auto Configuration and Starters for AI Models and Vector Stores.

ETL framework for Data Engineering. This provides the basis of loading data into a vector database, helping implement the Retrieval Augmented Generation pattern that enables you to bring your data to the AI model to incorporate into its response.

The project’s GitHub discussions is a great place to send feedback.

---

## ElevenLabs Text-to-Speech (TTS) :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/audio/speech/elevenlabs-speech.html

**Contents:**
- ElevenLabs Text-to-Speech (TTS)
- Introduction
- Prerequisites
- Auto-configuration
- Speech Properties
  - Connection Properties
  - Configuration Properties
- Runtime Options
  - Using Voice Settings
- Manual Configuration

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

ElevenLabs provides natural-sounding speech synthesis software using deep learning. Its AI audio models generate realistic, versatile, and contextually-aware speech, voices, and sound effects across 32 languages. The ElevenLabs Text-to-Speech API enables users to bring any book, article, PDF, newsletter, or text to life with ultra-realistic AI narration.

Create an ElevenLabs account and obtain an API key. You can sign up at the ElevenLabs signup page. Your API key can be found on your profile page after logging in.

Add the spring-ai-elevenlabs dependency to your project’s build file. For more information, refer to the Dependency Management section.

Spring AI provides Spring Boot auto-configuration for the ElevenLabs Text-to-Speech Client. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

The prefix spring.ai.elevenlabs is used as the property prefix for all ElevenLabs related configurations (both connection and TTS specific settings). This is defined in ElevenLabsConnectionProperties.

spring.ai.elevenlabs.base-url

The base URL for the ElevenLabs API.

spring.ai.elevenlabs.api-key

Your ElevenLabs API key.

Enabling and disabling of the audio speech auto-configurations are now configured via top level properties with the prefix spring.ai.model.audio.speech.

To enable, spring.ai.model.audio.speech=elevenlabs (It is enabled by default)

To disable, spring.ai.model.audio.speech=none (or any value which doesn’t match elevenlabs)

This change is done to allow configuration of multiple models.

The prefix spring.ai.elevenlabs.tts is used as the property prefix to configure the ElevenLabs Text-to-Speech client, specifically. This is defined in ElevenLabsSpeechProperties.

spring.ai.model.audio.speech

Enable Audio Speech Model

spring.ai.elevenlabs.tts.options.model-id

The ID of the model to use.

spring.ai.elevenlabs.tts.options.voice-id

The ID of the voice to use. This is the voice ID, not the voice name.

spring.ai.elevenlabs.tts.options.output-format

The output format for the generated audio. See Output Formats below.

MP3, 22.05 kHz, 32 kbps

MP3, 44.1 kHz, 32 kbps

MP3, 44.1 kHz, 64 kbps

MP3, 44.1 kHz, 96 kbps

MP3, 44.1 kHz, 128 kbps

MP3, 44.1 kHz, 192 kbps

Opus, 48 kHz, 32 kbps

Opus, 48 kHz, 64 kbps

Opus, 48 kHz, 96 kbps

Opus, 48 kHz, 128 kbps

Opus, 48 kHz, 192 kbps

The ElevenLabsTextToSpeechOptions class provides options to use when making a text-to-speech request. On start-up, the options specified by spring.ai.elevenlabs.tts are used, but you can override these at runtime. The following options are available:

modelId: The ID of the model to use.

voiceId: The ID of the voice to use.

outputFormat: The output format of the generated audio.

voiceSettings: An object containing voice settings such as stability, similarityBoost, style, useSpeakerBoost, and speed.

enableLogging: A boolean to enable or disable logging.

languageCode: The language code of the input text (e.g., "en" for English).

pronunciationDictionaryLocators: A list of pronunciation dictionary locators.

seed: A seed for random number generation, for reproducibility.

previousText: Text before the main text, for context in multi-turn conversations.

nextText: Text after the main text, for context in multi-turn conversations.

previousRequestIds: Request IDs from previous turns in a conversation.

nextRequestIds: Request IDs for subsequent turns in a conversation.

applyTextNormalization: Apply text normalization ("auto", "on", or "off").

applyLanguageTextNormalization: Apply language text normalization.

You can customize the voice output by providing VoiceSettings in the options. This allows you to control properties like stability and similarity.

Add the spring-ai-elevenlabs dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

Next, create an ElevenLabsTextToSpeechModel:

The ElevenLabs Speech API supports real-time audio streaming using chunk transfer encoding. This allows audio playback to begin before the entire audio file is generated.

The ElevenLabs Voices API allows you to retrieve information about available voices, their settings, and default voice settings. You can use this API to discover the `voiceId`s to use in your speech requests.

To use the Voices API, you’ll need to create an instance of ElevenLabsVoicesApi:

You can then use the following methods:

getVoices(): Retrieves a list of all available voices.

getDefaultVoiceSettings(): Gets the default settings for voices.

getVoiceSettings(String voiceId): Returns the settings for a specific voice.

getVoice(String voiceId): Returns metadata about a specific voice.

The ElevenLabsTextToSpeechModelIT.java test provides some general examples of how to use the library.

The ElevenLabsApiIT.java test provides examples of using the low-level ElevenLabsApi.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-elevenlabs</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-elevenlabs'
}
```

Example 3 (java):
```java
ElevenLabsTextToSpeechOptions speechOptions = ElevenLabsTextToSpeechOptions.builder()
    .model("eleven_multilingual_v2")
    .voiceId("your_voice_id")
    .outputFormat(ElevenLabsApi.OutputFormat.MP3_44100_128.getValue())
    .build();

TextToSpeechPrompt speechPrompt = new TextToSpeechPrompt("Hello, this is a text-to-speech example.", speechOptions);
TextToSpeechResponse response = elevenLabsTextToSpeechModel.call(speechPrompt);
```

Example 4 (java):
```java
var voiceSettings = new ElevenLabsApi.SpeechRequest.VoiceSettings(0.75f, 0.75f, 0.0f, true);

ElevenLabsTextToSpeechOptions speechOptions = ElevenLabsTextToSpeechOptions.builder()
    .model("eleven_multilingual_v2")
    .voiceId("your_voice_id")
    .voiceSettings(voiceSettings)
    .build();

TextToSpeechPrompt speechPrompt = new TextToSpeechPrompt("This is a test with custom voice settings!", speechOptions);
TextToSpeechResponse response = elevenLabsTextToSpeechModel.call(speechPrompt);
```

---

## Spring AI API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/index.html

**Contents:**
- Spring AI API
- Introduction
  - AI Model API
  - Vector Store API
  - Tool Calling API
  - Auto Configuration
  - ETL Data Engineering
- Feedback and Contributions

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The Spring AI API covers a wide range of functionalities. Each major feature is detailed in its own dedicated section. To provide an overview, the following key functionalities are available:

Portable Model API across AI providers for Chat, Text to Image, Audio Transcription, Text to Speech, and Embedding models. Both synchronous and stream API options are supported. Dropping down to access model specific features is also supported.

With support for AI Models from OpenAI, Microsoft, Amazon, Google, Amazon Bedrock, Hugging Face and more.

Portable Vector Store API across multiple providers, including a novel SQL-like metadata filter API that is also portable. Support for 14 vector databases are available.

Spring AI makes it easy to have the AI model invoke your services as @Tool-annotated methods or POJO java.util.Function objects.

Check the Spring AI Tool Calling documentation.

Spring Boot Auto Configuration and Starters for AI Models and Vector Stores.

ETL framework for Data Engineering. This provides the basis of loading data into a vector database, helping implement the Retrieval Augmented Generation pattern that enables you to bring your data to the AI model to incorporate into its response.

The project’s GitHub discussions is a great place to send feedback.

---
