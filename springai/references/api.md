# Springai - Api

**Pages:** 33

---

## Recursive Advisors :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/advisors-recursive.html

**Contents:**
- Recursive Advisors
- What is a Recursive Advisor?
- Built-in Recursive Advisors
  - ToolCallAdvisor
    - Return Direct Functionality
  - StructuredOutputValidationAdvisor

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Recursive advisors are a special type of advisor that can loop through the downstream advisor chain multiple times. This pattern is useful when you need to repeatedly call the LLM until a certain condition is met, such as:

Executing tool calls in a loop until no more tools need to be called

Validating structured output and retrying if validation fails

Implementing Evaluation logic with modifications to the request

Implementing retry logic with modifications to the request

The CallAdvisorChain.copy(CallAdvisor after) method is the key utility that enables recursive advisor patterns. It creates a new advisor chain that contains only the advisors that come after the specified advisor in the original chain and allows the recursive advisor to call this sub-chain as needed. This approach ensures that:

The recursive advisor can loop through the remaining advisors in the chain

Other advisors in the chain can observe and intercept each iteration

The advisor chain maintains proper ordering and observability

The recursive advisor doesn’t re-execute advisors that came before it

Spring AI provides two built-in recursive advisors that demonstrate this pattern:

The ToolCallAdvisor implements the tool calling loop as part of the advisor chain, rather than relying on the model’s internal tool execution. This enables other advisors in the chain to intercept and observe the tool calling process.

Disables the model’s internal tool execution by setting setInternalToolExecutionEnabled(false)

Loops through the advisor chain until no more tool calls are present

Supports "return direct" functionality - when a tool execution has returnDirect=true, it interrupts the tool calling loop and returns the tool execution result directly to the client application instead of sending it back to the LLM

Uses callAdvisorChain.copy(this) to create a sub-chain for recursive calls

Includes null safety checks to handle cases where the chat response might be null

The "return direct" feature allows tools to bypass the LLM and return their results directly to the client application. This is useful when:

The tool’s output is the final answer and doesn’t need LLM processing

You want to reduce latency by avoiding an additional LLM call

The tool result should be returned as-is without interpretation

When a tool execution has returnDirect=true, the ToolCallAdvisor will:

Execute the tool call as normal

Detect the returnDirect flag in the ToolExecutionResult

Break out of the tool calling loop

Return the tool execution result directly to the client application as a ChatResponse with the tool’s output as the generation content

The StructuredOutputValidationAdvisor validates the structured JSON output against a generated JSON schema and retries the call if validation fails, up to a specified number of attempts.

Automatically generates a JSON schema from the expected output type

Validates the LLM response against the schema

Retries the call if validation fails, up to a configurable number of attempts

Augments the prompt with validation error messages on retry attempts to help the LLM correct its output

Uses callAdvisorChain.copy(this) to create a sub-chain for recursive calls

Optionally supports a custom ObjectMapper for JSON processing

**Examples:**

Example 1 (java):
```java
var toolCallAdvisor = ToolCallAdvisor.builder()
    .toolCallingManager(toolCallingManager)
    .advisorOrder(BaseAdvisor.HIGHEST_PRECEDENCE + 300)
    .build();

var chatClient = ChatClient.builder(chatModel)
    .defaultAdvisors(toolCallAdvisor)
    .build();
```

Example 2 (java):
```java
var validationAdvisor = StructuredOutputValidationAdvisor.builder()
    .outputType(MyResponseType.class)
    .maxRepeatAttempts(3)
    .advisorOrder(BaseAdvisor.HIGHEST_PRECEDENCE + 1000)
    .build();

var chatClient = ChatClient.builder(chatModel)
    .defaultAdvisors(validationAdvisor)
    .build();
```

---

## Multimodality API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/multimodality.html

**Contents:**
- Multimodality API
- Spring AI Multimodality

For the latest snapshot version, please use Spring AI 1.1.2!

"All things that are naturally connected ought to be taught in combination" - John Amos Comenius, "Orbis Sensualium Pictus", 1658

Humans process knowledge, simultaneously across multiple modes of data inputs. The way we learn, our experiences are all multimodal. We don’t have just vision, just audio and just text.

Contrary to those principles, the Machine Learning was often focused on specialized models tailored to process a single modality. For instance, we developed audio models for tasks like text-to-speech or speech-to-text, and computer vision models for tasks such as object detection and classification.

However, a new wave of multimodal large language models starts to emerge. Examples include OpenAI’s GPT-4o , Google’s Vertex AI Gemini 1.5, Anthropic’s Claude3, and open source offerings Llama3.2, LLaVA and BakLLaVA are able to accept multiple inputs, including text images, audio and video and generate text responses by integrating these inputs.

Multimodality refers to a model’s ability to simultaneously understand and process information from various sources, including text, images, audio, and other data formats.

The Spring AI Message API provides all necessary abstractions to support multimodal LLMs.

The UserMessage’s content field is used primarily for text inputs, while the optional media field allows adding one or more additional content of different modalities such as images, audio and video. The MimeType specifies the modality type. Depending on the used LLMs, the Media data field can be either the raw media content as a Resource object or a URI to the content.

For example, we can take the following picture (multimodal.test.png) as an input and ask the LLM to explain what it sees.

For most of the multimodal LLMs, the Spring AI code would look something like this:

or with the fluent ChatClient API:

and produce a response like:

This is an image of a fruit bowl with a simple design. The bowl is made of metal with curved wire edges that create an open structure, allowing the fruit to be visible from all angles. Inside the bowl, there are two yellow bananas resting on top of what appears to be a red apple. The bananas are slightly overripe, as indicated by the brown spots on their peels. The bowl has a metal ring at the top, likely to serve as a handle for carrying. The bowl is placed on a flat surface with a neutral-colored background that provides a clear view of the fruit inside.

Spring AI provides multimodal support for the following chat models:

Azure Open AI (e.g. GPT-4o models)

Mistral AI (e.g. Mistral Pixtral models)

Ollama (e.g. LLaVA, BakLLaVA, Llama3.2 models)

OpenAI (e.g. GPT-4 and GPT-4o models)

Vertex AI Gemini (e.g. gemini-1.5-pro-001, gemini-1.5-flash-001 models)

**Examples:**

Example 1 (java):
```java
var imageResource = new ClassPathResource("/multimodal.test.png");

var userMessage = UserMessage.builder()
    .text("Explain what do you see in this picture?") // content
    .media(new Media(MimeTypeUtils.IMAGE_PNG, this.imageResource)) // media
    .build();

ChatResponse response = chatModel.call(new Prompt(this.userMessage));
```

Example 2 (java):
```java
String response = ChatClient.create(chatModel).prompt()
		.user(u -> u.text("Explain what do you see on this picture?")
				    .media(MimeTypeUtils.IMAGE_PNG, new ClassPathResource("/multimodal.test.png")))
		.call()
		.content();
```

---

## Testcontainers :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/testcontainers.html

**Contents:**
- Testcontainers
- Service Connections

For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI provides Spring Boot auto-configuration for establishing a connection to a model service or vector store running via Testcontainers. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The following service connection factories are provided in the spring-ai-spring-boot-testcontainers module:

AwsOpenSearchConnectionDetails

Containers of type LocalStackContainer

ChromaConnectionDetails

Containers of type ChromaDBContainer

MilvusServiceClientConnectionDetails

Containers of type MilvusContainer

MongoConnectionDetails

Containers of type MongoDBAtlasLocalContainer

OllamaConnectionDetails

Containers of type OllamaContainer

OpenSearchConnectionDetails

Containers of type OpensearchContainer

QdrantConnectionDetails

Containers of type QdrantContainer

TypesenseConnectionDetails

Containers of type TypesenseContainer

WeaviateConnectionDetails

Containers of type WeaviateContainer

More service connections are provided by the spring boot module spring-boot-testcontainers. Refer to the Testcontainers Service Connections documentation page for the full list.

**Examples:**

Example 1 (xml):
```xml
<dependency>
   <groupId>org.springframework.ai</groupId>
   <artifactId>spring-ai-spring-boot-testcontainers</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-spring-boot-testcontainers'
}
```

---

## Migrating from FunctionCallback to ToolCallback API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/tools-migration.html

**Contents:**
- Migrating from FunctionCallback to ToolCallback API
- Overview of Changes
- Key Changes
- Migration Examples
  - 1. Basic Function Callback
  - 2. ChatClient Usage
  - 3. Method-Based Function Callbacks
  - 4. Options Configuration
  - 5. Default Functions in ChatClient Builder
  - 6. Spring Bean Configuration

For the latest snapshot version, please use Spring AI 1.1.2!

This guide helps you migrate from the deprecated FunctionCallback API to the new ToolCallback API in Spring AI. For more information about the new APIs, check out the Tools Calling documentation.

These changes are part of a broader effort to improve and extend the tool calling capabilities in Spring AI. Among the other things, the new API moves from "functions" to "tools" terminology to better align with industry conventions. This involves several API changes while maintaining backward compatibility through deprecated methods.

FunctionCallback → ToolCallback

FunctionCallback.builder().function() → FunctionToolCallback.builder()

FunctionCallback.builder().method() → MethodToolCallback.builder()

FunctionCallingOptions → ToolCallingChatOptions

ChatClient.builder().defaultFunctions() → ChatClient.builder().defaultTools()

ChatClient.functions() → ChatClient.tools()

FunctionCallingOptions.builder().functions() → ToolCallingChatOptions.builder().toolNames()

FunctionCallingOptions.builder().functionCallbacks() → ToolCallingChatOptions.builder().toolCallbacks()

Or with the declarative approach:

And you can use the same ChatClient#tools() API to register method-based tool callbacks:

Or with the declarative approach:

The method() configuration in function callbacks has been replaced with a more explicit method tool configuration using ToolDefinition and MethodToolCallback.

When using method-based callbacks, you now need to explicitly find the method using ReflectionUtils and provide it to the builder. Alternatively, you can use the declarative approach with the @Tool annotation.

For non-static methods, you must now provide both the method and the target object:

The following methods are deprecated and will be removed in a future release:

ChatClient.Builder.defaultFunctions(String…​)

ChatClient.Builder.defaultFunctions(FunctionCallback…​)

ChatClient.RequestSpec.functions()

Use their tools counterparts instead.

Now you can use the method-level annotation (@Tool) to register tools with Spring AI:

The new API provides better separation between tool definition and implementation.

Tool definitions can be reused across different implementations.

The builder pattern has been simplified for common use cases.

Better support for method-based tools with improved error handling.

The deprecated methods will be maintained for backward compatibility in the current milestone version but will be removed in the next milestone release. It’s recommended to migrate to the new API as soon as possible.

**Examples:**

Example 1 (java):
```java
FunctionCallback.builder()
    .function("getCurrentWeather", new MockWeatherService())
    .description("Get the weather in location")
    .inputType(MockWeatherService.Request.class)
    .build()
```

Example 2 (java):
```java
FunctionToolCallback.builder("getCurrentWeather", new MockWeatherService())
    .description("Get the weather in location")
    .inputType(MockWeatherService.Request.class)
    .build()
```

Example 3 (java):
```java
String response = ChatClient.create(chatModel)
    .prompt()
    .user("What's the weather like in San Francisco?")
    .functions(FunctionCallback.builder()
        .function("getCurrentWeather", new MockWeatherService())
        .description("Get the weather in location")
        .inputType(MockWeatherService.Request.class)
        .build())
    .call()
    .content();
```

Example 4 (java):
```java
String response = ChatClient.create(chatModel)
    .prompt()
    .user("What's the weather like in San Francisco?")
    .tools(FunctionToolCallback.builder("getCurrentWeather", new MockWeatherService())
        .description("Get the weather in location")
        .inputType(MockWeatherService.Request.class)
        .build())
    .call()
    .content();
```

---

## Multimodality API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/multimodality.html

**Contents:**
- Multimodality API
- Spring AI Multimodality

"All things that are naturally connected ought to be taught in combination" - John Amos Comenius, "Orbis Sensualium Pictus", 1658

Humans process knowledge, simultaneously across multiple modes of data inputs. The way we learn, our experiences are all multimodal. We don’t have just vision, just audio and just text.

Contrary to those principles, the Machine Learning was often focused on specialized models tailored to process a single modality. For instance, we developed audio models for tasks like text-to-speech or speech-to-text, and computer vision models for tasks such as object detection and classification.

However, a new wave of multimodal large language models starts to emerge. Examples include OpenAI’s GPT-4o , Google’s Vertex AI Gemini 1.5, Anthropic’s Claude3, and open source offerings Llama3.2, LLaVA and BakLLaVA are able to accept multiple inputs, including text images, audio and video and generate text responses by integrating these inputs.

Multimodality refers to a model’s ability to simultaneously understand and process information from various sources, including text, images, audio, and other data formats.

The Spring AI Message API provides all necessary abstractions to support multimodal LLMs.

The UserMessage’s content field is used primarily for text inputs, while the optional media field allows adding one or more additional content of different modalities such as images, audio and video. The MimeType specifies the modality type. Depending on the used LLMs, the Media data field can be either the raw media content as a Resource object or a URI to the content.

For example, we can take the following picture (multimodal.test.png) as an input and ask the LLM to explain what it sees.

For most of the multimodal LLMs, the Spring AI code would look something like this:

or with the fluent ChatClient API:

and produce a response like:

This is an image of a fruit bowl with a simple design. The bowl is made of metal with curved wire edges that create an open structure, allowing the fruit to be visible from all angles. Inside the bowl, there are two yellow bananas resting on top of what appears to be a red apple. The bananas are slightly overripe, as indicated by the brown spots on their peels. The bowl has a metal ring at the top, likely to serve as a handle for carrying. The bowl is placed on a flat surface with a neutral-colored background that provides a clear view of the fruit inside.

Spring AI provides multimodal support for the following chat models:

Azure Open AI (e.g. GPT-4o models)

Mistral AI (e.g. Mistral Pixtral models)

Ollama (e.g. LLaVA, BakLLaVA, Llama3.2 models)

OpenAI (e.g. GPT-4 and GPT-4o models)

Vertex AI Gemini (e.g. gemini-1.5-pro-001, gemini-1.5-flash-001 models)

**Examples:**

Example 1 (java):
```java
var imageResource = new ClassPathResource("/multimodal.test.png");

var userMessage = UserMessage.builder()
    .text("Explain what do you see in this picture?") // content
    .media(new Media(MimeTypeUtils.IMAGE_PNG, this.imageResource)) // media
    .build();

ChatResponse response = chatModel.call(new Prompt(this.userMessage));
```

Example 2 (java):
```java
String response = ChatClient.create(chatModel).prompt()
		.user(u -> u.text("Explain what do you see on this picture?")
				    .media(MimeTypeUtils.IMAGE_PNG, new ClassPathResource("/multimodal.test.png")))
		.call()
		.content();
```

---

## Upgrade Notes :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/upgrade-notes.html

**Contents:**
- Upgrade Notes
- Upgrading to 2.0.0-M1
  - Breaking Changes
    - Default Temperature Configuration Removed
      - Impact
      - Migration
- Upgrading to 1.1.0-RC1
  - Breaking Changes
    - Text-to-Speech (TTS) API Migration
      - Removed Classes

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI no longer provides default temperature values for chat model autoconfiguration properties. Previously, Spring AI set a default temperature of 0.7 for most chat models. This default has been removed to allow each AI provider’s native default temperature to be used.

If your application did not explicitly configure a temperature value and relied on Spring AI’s default of 0.7, you may notice different behavior after upgrading. The actual default will now be determined by each AI provider’s API, which may vary:

Some providers default to 1.0

Some providers default to 0.7

Some providers have model-specific defaults

If you want to maintain the previous behavior, explicitly set the temperature in your configuration:

Or programmatically when building requests:

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

Example 1 (markdown):
```markdown
# Example for OpenAI
spring.ai.openai.chat.options.temperature=0.7

# Example for Anthropic
spring.ai.anthropic.chat.options.temperature=0.7

# Example for Azure OpenAI
spring.ai.azure.openai.chat.options.temperature=0.7
```

Example 2 (java):
```java
ChatResponse response = chatModel.call(
    new Prompt("Your prompt here",
        OpenAiChatOptions.builder()
            .temperature(0.7)
            .build()));
```

Example 3 (yaml):
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

Example 4 (yaml):
```yaml
Find:    .speed(1.0f)
Replace: .speed(1.0)

Find:    Float speed
Replace: Double speed
```

---

## Evaluation Testing :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/testing.html

**Contents:**
- Evaluation Testing
- Relevancy Evaluator
- Usage in Integration Tests
  - Custom Template
- FactCheckingEvaluator
  - Usage
  - Example

For the latest snapshot version, please use Spring AI 1.1.2!

Testing AI applications requires evaluating the generated content to ensure the AI model has not produced a hallucinated response.

One method to evaluate the response is to use the AI model itself for evaluation. Select the best AI model for the evaluation, which may not be the same model used to generate the response.

The Spring AI interface for evaluating responses is Evaluator, defined as:

The input to the evaluation is the EvaluationRequest defined as

userText: The raw input from the user as a String

dataList: Contextual data, such as from Retrieval Augmented Generation, appended to the raw input.

responseContent: The AI model’s response content as a String

The RelevancyEvaluator is an implementation of the Evaluator interface, designed to assess the relevance of AI-generated responses against provided context. This evaluator helps assess the quality of a RAG flow by determining if the AI model’s response is relevant to the user’s input with respect to the retrieved context.

The evaluation is based on the user input, the AI model’s response, and the context information. It uses a prompt template to ask the AI model if the response is relevant to the user input and context.

This is the default prompt template used by the RelevancyEvaluator:

Here is an example of usage of the RelevancyEvaluator in an integration test, validating the result of a RAG flow using the RetrievalAugmentationAdvisor:

You can find several integration tests in the Spring AI project that use the RelevancyEvaluator to test the functionality of the QuestionAnswerAdvisor (see tests) and RetrievalAugmentationAdvisor (see tests).

The RelevancyEvaluator uses a default template to prompt the AI model for evaluation. You can customize this behavior by providing your own PromptTemplate object via the .promptTemplate() builder method.

The custom PromptTemplate can use any TemplateRenderer implementation (by default, it uses StPromptTemplate based on the StringTemplate engine). The important requirement is that the template must contain the following placeholders:

a query placeholder to receive the user question.

a response placeholder to receive the AI model’s response.

a context placeholder to receive the context information.

The FactCheckingEvaluator is another implementation of the Evaluator interface, designed to assess the factual accuracy of AI-generated responses against provided context. This evaluator helps detect and reduce hallucinations in AI outputs by verifying if a given statement (claim) is logically supported by the provided context (document).

The 'claim' and 'document' are presented to the AI model for evaluation. Smaller and more efficient AI models dedicated to this purpose are available, such as Bespoke’s Minicheck, which helps reduce the cost of performing these checks compared to flagship models like GPT-4. Minicheck is also available for use through Ollama.

The FactCheckingEvaluator constructor takes a ChatClient.Builder as a parameter:

The evaluator uses the following prompt template for fact-checking:

Where {document} is the context information, and {claim} is the AI model’s response to be evaluated.

Here’s an example of how to use the FactCheckingEvaluator with an Ollama-based ChatModel, specifically the Bespoke-Minicheck model:

**Examples:**

Example 1 (java):
```java
@FunctionalInterface
public interface Evaluator {
    EvaluationResponse evaluate(EvaluationRequest evaluationRequest);
}
```

Example 2 (java):
```java
public class EvaluationRequest {

	private final String userText;

	private final List<Content> dataList;

	private final String responseContent;

	public EvaluationRequest(String userText, List<Content> dataList, String responseContent) {
		this.userText = userText;
		this.dataList = dataList;
		this.responseContent = responseContent;
	}

  ...
}
```

Example 3 (json):
```json
Your task is to evaluate if the response for the query
is in line with the context information provided.

You have two options to answer. Either YES or NO.

Answer YES, if the response for the query
is in line with context information otherwise NO.

Query:
{query}

Response:
{response}

Context:
{context}

Answer:
```

Example 4 (java):
```java
@Test
void evaluateRelevancy() {
    String question = "Where does the adventure of Anacletus and Birba take place?";

    RetrievalAugmentationAdvisor ragAdvisor = RetrievalAugmentationAdvisor.builder()
        .documentRetriever(VectorStoreDocumentRetriever.builder()
            .vectorStore(pgVectorStore)
            .build())
        .build();

    ChatResponse chatResponse = ChatClient.builder(chatModel).build()
        .prompt(question)
        .advisors(ragAdvisor)
        .call()
        .chatResponse();

    EvaluationRequest evaluationRequest = new EvaluationRequest(
        // The original user question
        question,
        // The retrieved context from the RAG flow
        chatResponse.getMetadata().get(RetrievalAugmentationAdvisor.DOCUMENT_CONTEXT),
        // The AI model's response
        chatResponse.getResult().getOutput().getText()
    );

    RelevancyEvaluator evaluator = new RelevancyEvaluator(ChatClient.builder(chatModel));

    EvaluationResponse evaluationResponse = evaluator.evaluate(evaluationRequest);

    assertThat(evaluationResponse.isPass()).isTrue();
}
```

---

## Advisors API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/advisors.html

**Contents:**
- Advisors API
- Core Components
  - Advisor Order
- API Overview
- Implementing an Advisor
  - Examples
    - Logging Advisor
    - Re-Reading (Re2) Advisor
    - Spring AI Built-in Advisors
      - Chat Memory Advisors

For the latest snapshot version, please use Spring AI 1.1.2!

The Spring AI Advisors API provides a flexible and powerful way to intercept, modify, and enhance AI-driven interactions in your Spring applications. By leveraging the Advisors API, developers can create more sophisticated, reusable, and maintainable AI components.

The key benefits include encapsulating recurring Generative AI patterns, transforming data sent to and from Large Language Models (LLMs), and providing portability across various models and use cases.

You can configure existing advisors using the ChatClient API as shown in the following example:

It is recommend to register the advisors at build time using builder’s defaultAdvisors() method.

Advisors also participate in the Observability stack, so you can view metrics and traces related to their execution.

Learn about Question Answer Advisor

Learn about Chat Memory Advisor

The API consists of CallAdvisor and CallAdvisorChain for non-streaming scenarios, and StreamAdvisor and StreamAdvisorChain for streaming scenarios. It also includes ChatClientRequest to represent the unsealed Prompt request, ChatClientResponse for the Chat Completion response. Both hold an advise-context to share state across the advisor chain.

The adviseCall() and the adviseStream() are the key advisor methods, typically performing actions such as examining the unsealed Prompt data, customizing and augmenting the Prompt data, invoking the next entity in the advisor chain, optionally blocking the request, examining the chat completion response, and throwing exceptions to indicate processing errors.

In addition the getOrder() method determines advisor order in the chain, while getName() provides a unique advisor name.

The Advisor Chain, created by the Spring AI framework, allows sequential invocation of multiple advisors ordered by their getOrder() values. The lower values are executed first. The last advisor, added automatically, sends the request to the LLM.

Following flow diagram illustrates the interaction between the advisor chain and the Chat Model:

The Spring AI framework creates an ChatClientRequest from user’s Prompt along with an empty advisor context object.

Each advisor in the chain processes the request, potentially modifying it. Alternatively, it can choose to block the request by not making the call to invoke the next entity. In the latter case, the advisor is responsible for filling out the response.

The final advisor, provided by the framework, sends the request to the Chat Model.

The Chat Model’s response is then passed back through the advisor chain and converted into ChatClientResponse. Later includes the shared advisor context instance.

Each advisor can process or modify the response.

The final ChatClientResponse is returned to the client by extracting the ChatCompletion.

The execution order of advisors in the chain is determined by the getOrder() method. Key points to understand:

Advisors with lower order values are executed first.

The advisor chain operates as a stack:

The first advisor in the chain is the first to process the request.

It is also the last to process the response.

To control execution order:

Set the order close to Ordered.HIGHEST_PRECEDENCE to ensure an advisor is executed first in the chain (first for request processing, last for response processing).

Set the order close to Ordered.LOWEST_PRECEDENCE to ensure an advisor is executed last in the chain (last for request processing, first for response processing).

Higher values are interpreted as lower priority.

If multiple advisors have the same order value, their execution order is not guaranteed.

The seeming contradiction between order and execution sequence is due to the stack-like nature of the advisor chain:

An advisor with the highest precedence (lowest order value) is added to the top of the stack.

It will be the first to process the request as the stack unwinds.

It will be the last to process the response as the stack rewinds.

As a reminder, here are the semantics of the Spring Ordered interface:

For use cases that need to be first in the chain on both the input and output sides:

Use separate advisors for each side.

Configure them with different order values.

Use the advisor context to share state between them.

The main Advisor interfaces are located in the package org.springframework.ai.chat.client.advisor.api. Here are the key interfaces you’ll encounter when creating your own advisor:

The two sub-interfaces for synchronous and reactive Advisors are

To continue the chain of Advice, use CallAdvisorChain and StreamAdvisorChain in your Advice implementation:

To create an advisor, implement either CallAdvisor or StreamAdvisor (or both). The key method to implement is nextCall() for non-streaming or nextStream() for streaming advisors.

We will provide few hands-on examples to illustrate how to implement advisors for observing and augmenting use-cases.

We can implement a simple logging advisor that logs the ChatClientRequest before and the ChatClientResponse after the call to the next advisor in the chain. Note that the advisor only observes the request and response and does not modify them. This implementation support both non-streaming and streaming scenarios.

The "Re-Reading Improves Reasoning in Large Language Models" article introduces a technique called Re-Reading (Re2) that improves the reasoning capabilities of Large Language Models. The Re2 technique requires augmenting the input prompt like this:

Implementing an advisor that applies the Re2 technique to the user’s input query can be done like this:

Spring AI framework provides several built-in advisors to enhance your AI interactions. Here’s an overview of the available advisors:

These advisors manage conversation history in a chat memory store:

MessageChatMemoryAdvisor

Retrieves memory and adds it as a collection of messages to the prompt. This approach maintains the structure of the conversation history. Note, not all AI Models support this approach.

PromptChatMemoryAdvisor

Retrieves memory and incorporates it into the prompt’s system text.

VectorStoreChatMemoryAdvisor

Retrieves memory from a VectorStore and adds it into the prompt’s system text. This advisor is useful for efficiently searching and retrieving relevant information from large datasets.

QuestionAnswerAdvisor

This advisor uses a vector store to provide question-answering capabilities, implementing the Naive RAG (Retrieval-Augmented Generation) pattern.

RetrievalAugmentationAdvisor

Implements a re-reading strategy for LLM reasoning, dubbed RE2, to enhance understanding in the input phase. Based on the article: [Re-Reading Improves Reasoning in LLMs](arxiv.org/pdf/2309.06275).

A simple advisor designed to prevent the model from generating harmful or inappropriate content.

Non-streaming advisors work with complete requests and responses.

Streaming advisors handle requests and responses as continuous streams, using reactive programming concepts (e.g., Flux for responses).

Keep advisors focused on specific tasks for better modularity.

Use the adviseContext to share state between advisors when necessary.

Implement both streaming and non-streaming versions of your advisor for maximum flexibility.

Carefully consider the order of advisors in your chain to ensure proper data flow.

In 1.0 M2, there were separate RequestAdvisor and ResponseAdvisor interfaces.

RequestAdvisor was invoked before the ChatModel.call and ChatModel.stream methods.

ResponseAdvisor was called after these methods.

In 1.0 M3, these interfaces have been replaced with:

The StreamResponseMode, previously part of ResponseAdvisor, has been removed.

In 1.0.0 these interfaces have been replaced:

CallAroundAdvisor → CallAdvisor, StreamAroundAdvisor → StreamAdvisor, CallAroundAdvisorChain → CallAdvisorChain and StreamAroundAdvisorChain → StreamAdvisorChain.

AdvisedRequest → ChatClientRequest are AdivsedResponse → ChatClientResponse.

The context map was a separate method argument.

The map was mutable and passed along the chain.

The context map is now part of the AdvisedRequest and AdvisedResponse records.

The map is immutable.

To update the context, use the updateContext method, which creates a new unmodifiable map with the updated contents.

**Examples:**

Example 1 (java):
```java
ChatMemory chatMemory = ... // Initialize your chat memory store
VectorStore vectorStore = ... // Initialize your vector store

var chatClient = ChatClient.builder(chatModel)
    .defaultAdvisors(
        MessageChatMemoryAdvisor.builder(chatMemory).build(), // chat-memory advisor
        QuestionAnswerAdvisor.builder(vectorStore).build()    // RAG advisor
    )
    .build();

var conversationId = "678";

String response = this.chatClient.prompt()
    // Set advisor parameters at runtime
    .advisors(advisor -> advisor.param(ChatMemory.CONVERSATION_ID, conversationId))
    .user(userText)
    .call()
	.content();
```

Example 2 (java):
```java
public interface Ordered {

    /**
     * Constant for the highest precedence value.
     * @see java.lang.Integer#MIN_VALUE
     */
    int HIGHEST_PRECEDENCE = Integer.MIN_VALUE;

    /**
     * Constant for the lowest precedence value.
     * @see java.lang.Integer#MAX_VALUE
     */
    int LOWEST_PRECEDENCE = Integer.MAX_VALUE;

    /**
     * Get the order value of this object.
     * <p>Higher values are interpreted as lower priority. As a consequence,
     * the object with the lowest value has the highest priority (somewhat
     * analogous to Servlet {@code load-on-startup} values).
     * <p>Same order values will result in arbitrary sort positions for the
     * affected objects.
     * @return the order value
     * @see #HIGHEST_PRECEDENCE
     * @see #LOWEST_PRECEDENCE
     */
    int getOrder();
}
```

Example 3 (java):
```java
public interface Advisor extends Ordered {

	String getName();

}
```

Example 4 (java):
```java
public interface CallAdvisor extends Advisor {

	ChatClientResponse adviseCall(
		ChatClientRequest chatClientRequest, CallAdvisorChain callAdvisorChain);

}
```

---

## Advisors API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/advisors.html

**Contents:**
- Advisors API
- Core Components
  - Advisor Order
- API Overview
- Implementing an Advisor
  - Examples
    - Logging Advisor
    - Re-Reading (Re2) Advisor
    - Spring AI Built-in Advisors
      - Chat Memory Advisors

The Spring AI Advisors API provides a flexible and powerful way to intercept, modify, and enhance AI-driven interactions in your Spring applications. By leveraging the Advisors API, developers can create more sophisticated, reusable, and maintainable AI components.

The key benefits include encapsulating recurring Generative AI patterns, transforming data sent to and from Large Language Models (LLMs), and providing portability across various models and use cases.

You can configure existing advisors using the ChatClient API as shown in the following example:

It is recommend to register the advisors at build time using builder’s defaultAdvisors() method.

Advisors also participate in the Observability stack, so you can view metrics and traces related to their execution.

Learn about Question Answer Advisor

Learn about Chat Memory Advisor

The API consists of CallAdvisor and CallAdvisorChain for non-streaming scenarios, and StreamAdvisor and StreamAdvisorChain for streaming scenarios. It also includes ChatClientRequest to represent the unsealed Prompt request, ChatClientResponse for the Chat Completion response. Both hold an advise-context to share state across the advisor chain.

The adviseCall() and the adviseStream() are the key advisor methods, typically performing actions such as examining the unsealed Prompt data, customizing and augmenting the Prompt data, invoking the next entity in the advisor chain, optionally blocking the request, examining the chat completion response, and throwing exceptions to indicate processing errors.

In addition the getOrder() method determines advisor order in the chain, while getName() provides a unique advisor name.

The Advisor Chain, created by the Spring AI framework, allows sequential invocation of multiple advisors ordered by their getOrder() values. The lower values are executed first. The last advisor, added automatically, sends the request to the LLM.

Following flow diagram illustrates the interaction between the advisor chain and the Chat Model:

The Spring AI framework creates an ChatClientRequest from user’s Prompt along with an empty advisor context object.

Each advisor in the chain processes the request, potentially modifying it. Alternatively, it can choose to block the request by not making the call to invoke the next entity. In the latter case, the advisor is responsible for filling out the response.

The final advisor, provided by the framework, sends the request to the Chat Model.

The Chat Model’s response is then passed back through the advisor chain and converted into ChatClientResponse. Later includes the shared advisor context instance.

Each advisor can process or modify the response.

The final ChatClientResponse is returned to the client by extracting the ChatCompletion.

The execution order of advisors in the chain is determined by the getOrder() method. Key points to understand:

Advisors with lower order values are executed first.

The advisor chain operates as a stack:

The first advisor in the chain is the first to process the request.

It is also the last to process the response.

To control execution order:

Set the order close to Ordered.HIGHEST_PRECEDENCE to ensure an advisor is executed first in the chain (first for request processing, last for response processing).

Set the order close to Ordered.LOWEST_PRECEDENCE to ensure an advisor is executed last in the chain (last for request processing, first for response processing).

Higher values are interpreted as lower priority.

If multiple advisors have the same order value, their execution order is not guaranteed.

The seeming contradiction between order and execution sequence is due to the stack-like nature of the advisor chain:

An advisor with the highest precedence (lowest order value) is added to the top of the stack.

It will be the first to process the request as the stack unwinds.

It will be the last to process the response as the stack rewinds.

As a reminder, here are the semantics of the Spring Ordered interface:

For use cases that need to be first in the chain on both the input and output sides:

Use separate advisors for each side.

Configure them with different order values.

Use the advisor context to share state between them.

The main Advisor interfaces are located in the package org.springframework.ai.chat.client.advisor.api. Here are the key interfaces you’ll encounter when creating your own advisor:

The two sub-interfaces for synchronous and reactive Advisors are

To continue the chain of Advice, use CallAdvisorChain and StreamAdvisorChain in your Advice implementation:

To create an advisor, implement either CallAdvisor or StreamAdvisor (or both). The key method to implement is nextCall() for non-streaming or nextStream() for streaming advisors.

We will provide few hands-on examples to illustrate how to implement advisors for observing and augmenting use-cases.

We can implement a simple logging advisor that logs the ChatClientRequest before and the ChatClientResponse after the call to the next advisor in the chain. Note that the advisor only observes the request and response and does not modify them. This implementation support both non-streaming and streaming scenarios.

The "Re-Reading Improves Reasoning in Large Language Models" article introduces a technique called Re-Reading (Re2) that improves the reasoning capabilities of Large Language Models. The Re2 technique requires augmenting the input prompt like this:

Implementing an advisor that applies the Re2 technique to the user’s input query can be done like this:

Spring AI framework provides several built-in advisors to enhance your AI interactions. Here’s an overview of the available advisors:

These advisors manage conversation history in a chat memory store:

MessageChatMemoryAdvisor

Retrieves memory and adds it as a collection of messages to the prompt. This approach maintains the structure of the conversation history. Note, not all AI Models support this approach.

PromptChatMemoryAdvisor

Retrieves memory and incorporates it into the prompt’s system text.

VectorStoreChatMemoryAdvisor

Retrieves memory from a VectorStore and adds it into the prompt’s system text. This advisor is useful for efficiently searching and retrieving relevant information from large datasets.

QuestionAnswerAdvisor

This advisor uses a vector store to provide question-answering capabilities, implementing the Naive RAG (Retrieval-Augmented Generation) pattern.

RetrievalAugmentationAdvisor

Implements a re-reading strategy for LLM reasoning, dubbed RE2, to enhance understanding in the input phase. Based on the article: [Re-Reading Improves Reasoning in LLMs](arxiv.org/pdf/2309.06275).

A simple advisor designed to prevent the model from generating harmful or inappropriate content.

Non-streaming advisors work with complete requests and responses.

Streaming advisors handle requests and responses as continuous streams, using reactive programming concepts (e.g., Flux for responses).

Keep advisors focused on specific tasks for better modularity.

Use the adviseContext to share state between advisors when necessary.

Implement both streaming and non-streaming versions of your advisor for maximum flexibility.

Carefully consider the order of advisors in your chain to ensure proper data flow.

In 1.0 M2, there were separate RequestAdvisor and ResponseAdvisor interfaces.

RequestAdvisor was invoked before the ChatModel.call and ChatModel.stream methods.

ResponseAdvisor was called after these methods.

In 1.0 M3, these interfaces have been replaced with:

The StreamResponseMode, previously part of ResponseAdvisor, has been removed.

In 1.0.0 these interfaces have been replaced:

CallAroundAdvisor → CallAdvisor, StreamAroundAdvisor → StreamAdvisor, CallAroundAdvisorChain → CallAdvisorChain and StreamAroundAdvisorChain → StreamAdvisorChain.

AdvisedRequest → ChatClientRequest and AdivsedResponse → ChatClientResponse.

The context map was a separate method argument.

The map was mutable and passed along the chain.

The context map is now part of the AdvisedRequest and AdvisedResponse records.

The map is immutable.

To update the context, use the updateContext method, which creates a new unmodifiable map with the updated contents.

**Examples:**

Example 1 (java):
```java
ChatMemory chatMemory = ... // Initialize your chat memory store
VectorStore vectorStore = ... // Initialize your vector store

var chatClient = ChatClient.builder(chatModel)
    .defaultAdvisors(
        MessageChatMemoryAdvisor.builder(chatMemory).build(), // chat-memory advisor
        QuestionAnswerAdvisor.builder(vectorStore).build()    // RAG advisor
    )
    .build();

var conversationId = "678";

String response = this.chatClient.prompt()
    // Set advisor parameters at runtime
    .advisors(advisor -> advisor.param(ChatMemory.CONVERSATION_ID, conversationId))
    .user(userText)
    .call()
	.content();
```

Example 2 (java):
```java
public interface Ordered {

    /**
     * Constant for the highest precedence value.
     * @see java.lang.Integer#MIN_VALUE
     */
    int HIGHEST_PRECEDENCE = Integer.MIN_VALUE;

    /**
     * Constant for the lowest precedence value.
     * @see java.lang.Integer#MAX_VALUE
     */
    int LOWEST_PRECEDENCE = Integer.MAX_VALUE;

    /**
     * Get the order value of this object.
     * <p>Higher values are interpreted as lower priority. As a consequence,
     * the object with the lowest value has the highest priority (somewhat
     * analogous to Servlet {@code load-on-startup} values).
     * <p>Same order values will result in arbitrary sort positions for the
     * affected objects.
     * @return the order value
     * @see #HIGHEST_PRECEDENCE
     * @see #LOWEST_PRECEDENCE
     */
    int getOrder();
}
```

Example 3 (java):
```java
public interface Advisor extends Ordered {

	String getName();

}
```

Example 4 (java):
```java
public interface CallAdvisor extends Advisor {

	ChatClientResponse adviseCall(
		ChatClientRequest chatClientRequest, CallAdvisorChain callAdvisorChain);

}
```

---

## Cloud Bindings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/cloud-bindings.html

**Contents:**
- Cloud Bindings
- Available Cloud Bindings

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI provides support for cloud bindings based on the foundations in spring-cloud-bindings. This allows applications to specify a binding type for a provider and then express properties using a generic format. The spring-ai cloud bindings will process these properties and bind them to spring-ai native properties.

For example, when using OpenAi, the binding type is openai. Using the property spring.ai.cloud.bindings.openai.enabled, the binding processor can be enabled or disabled. By default, when specifying a binding type, this property will be enabled. Configuration for api-key, uri, username, password, etc. can be specified and spring-ai will map them to the corresponding properties in the supported system.

To enable cloud binding support, include the following dependency in the application.

or to your Gradle build.gradle build file.

The following are the components for which the cloud binding support is currently available in the spring-ai-spring-cloud-bindings module:

uri, username, password

spring.ai.vectorstore.chroma.client.host, spring.ai.vectorstore.chroma.client.port, spring.ai.vectorstore.chroma.client.username, spring.ai.vectorstore.chroma.client.host.password

spring.ai.mistralai.api-key, spring.ai.mistralai.base-url

spring.ai.ollama.base-url

spring.ai.openai.api-key, spring.ai.openai.base-url

spring.ai.vectorstore.weaviate.scheme, spring.ai.vectorstore.weaviate.host, spring.ai.vectorstore.weaviate.api-key

uri, api-key, model-capabilities (chat and embedding), model-name

spring.ai.openai.chat.base-url, spring.ai.openai.chat.api-key, spring.ai.openai.chat.options.model, spring.ai.openai.embedding.base-url, spring.ai.openai.embedding.api-key, spring.ai.openai.embedding.options.model

**Examples:**

Example 1 (xml):
```xml
<dependency>
   <groupId>org.springframework.ai</groupId>
   <artifactId>spring-ai-spring-cloud-bindings</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-spring-cloud-bindings'
}
```

---

## Evaluation Testing :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/testing.html

**Contents:**
- Evaluation Testing
- Relevancy Evaluator
- Usage in Integration Tests
  - Custom Template
- FactCheckingEvaluator
  - Usage
  - Example

Testing AI applications requires evaluating the generated content to ensure the AI model has not produced a hallucinated response.

One method to evaluate the response is to use the AI model itself for evaluation. Select the best AI model for the evaluation, which may not be the same model used to generate the response.

The Spring AI interface for evaluating responses is Evaluator, defined as:

The input to the evaluation is the EvaluationRequest defined as

userText: The raw input from the user as a String

dataList: Contextual data, such as from Retrieval Augmented Generation, appended to the raw input.

responseContent: The AI model’s response content as a String

The RelevancyEvaluator is an implementation of the Evaluator interface, designed to assess the relevance of AI-generated responses against provided context. This evaluator helps assess the quality of a RAG flow by determining if the AI model’s response is relevant to the user’s input with respect to the retrieved context.

The evaluation is based on the user input, the AI model’s response, and the context information. It uses a prompt template to ask the AI model if the response is relevant to the user input and context.

This is the default prompt template used by the RelevancyEvaluator:

Here is an example of usage of the RelevancyEvaluator in an integration test, validating the result of a RAG flow using the RetrievalAugmentationAdvisor:

You can find several integration tests in the Spring AI project that use the RelevancyEvaluator to test the functionality of the QuestionAnswerAdvisor (see tests) and RetrievalAugmentationAdvisor (see tests).

The RelevancyEvaluator uses a default template to prompt the AI model for evaluation. You can customize this behavior by providing your own PromptTemplate object via the .promptTemplate() builder method.

The custom PromptTemplate can use any TemplateRenderer implementation (by default, it uses StPromptTemplate based on the StringTemplate engine). The important requirement is that the template must contain the following placeholders:

a query placeholder to receive the user question.

a response placeholder to receive the AI model’s response.

a context placeholder to receive the context information.

The FactCheckingEvaluator is another implementation of the Evaluator interface, designed to assess the factual accuracy of AI-generated responses against provided context. This evaluator helps detect and reduce hallucinations in AI outputs by verifying if a given statement (claim) is logically supported by the provided context (document).

The 'claim' and 'document' are presented to the AI model for evaluation. Smaller and more efficient AI models dedicated to this purpose are available, such as Bespoke’s Minicheck, which helps reduce the cost of performing these checks compared to flagship models like GPT-4. Minicheck is also available for use through Ollama.

The FactCheckingEvaluator constructor takes a ChatClient.Builder as a parameter:

The evaluator uses the following prompt template for fact-checking:

Where {document} is the context information, and {claim} is the AI model’s response to be evaluated.

Here’s an example of how to use the FactCheckingEvaluator with an Ollama-based ChatModel, specifically the Bespoke-Minicheck model:

**Examples:**

Example 1 (java):
```java
@FunctionalInterface
public interface Evaluator {
    EvaluationResponse evaluate(EvaluationRequest evaluationRequest);
}
```

Example 2 (java):
```java
public class EvaluationRequest {

	private final String userText;

	private final List<Content> dataList;

	private final String responseContent;

	public EvaluationRequest(String userText, List<Content> dataList, String responseContent) {
		this.userText = userText;
		this.dataList = dataList;
		this.responseContent = responseContent;
	}

  ...
}
```

Example 3 (json):
```json
Your task is to evaluate if the response for the query
is in line with the context information provided.

You have two options to answer. Either YES or NO.

Answer YES, if the response for the query
is in line with context information otherwise NO.

Query:
{query}

Response:
{response}

Context:
{context}

Answer:
```

Example 4 (java):
```java
@Test
void evaluateRelevancy() {
    String question = "Where does the adventure of Anacletus and Birba take place?";

    RetrievalAugmentationAdvisor ragAdvisor = RetrievalAugmentationAdvisor.builder()
        .documentRetriever(VectorStoreDocumentRetriever.builder()
            .vectorStore(pgVectorStore)
            .build())
        .build();

    ChatResponse chatResponse = ChatClient.builder(chatModel).build()
        .prompt(question)
        .advisors(ragAdvisor)
        .call()
        .chatResponse();

    EvaluationRequest evaluationRequest = new EvaluationRequest(
        // The original user question
        question,
        // The retrieved context from the RAG flow
        chatResponse.getMetadata().get(RetrievalAugmentationAdvisor.DOCUMENT_CONTEXT),
        // The AI model's response
        chatResponse.getResult().getOutput().getText()
    );

    RelevancyEvaluator evaluator = new RelevancyEvaluator(ChatClient.builder(chatModel));

    EvaluationResponse evaluationResponse = evaluator.evaluate(evaluationRequest);

    assertThat(evaluationResponse.isPass()).isTrue();
}
```

---

## Prompts :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/prompt.html

**Contents:**
- Prompts
- API Overview
  - Prompt
  - Message
    - Roles
  - PromptTemplate
- Example Usage
  - Using a custom template renderer
  - Using resources instead of raw Strings
- Prompt Engineering

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Prompts are the inputs that guide an AI model to generate specific outputs. The design and phrasing of these prompts significantly influence the model’s responses.

At the lowest level of interaction with AI models in Spring AI, handling prompts in Spring AI is somewhat similar to managing the "View" in Spring MVC. This involves creating extensive text with placeholders for dynamic content. These placeholders are then replaced based on user requests or other code in the application. Another analogy is a SQL statement that contain placeholders for certain expressions.

As Spring AI evolves, it will introduce higher levels of abstraction for interacting with AI models. The foundational classes described in this section can be likened to JDBC in terms of their role and functionality. The ChatModel class, for instance, is analogous to the core JDBC library in the JDK. The ChatClient class can be likened to the JdbcClient, built on top of ChatModel and providing more advanced constructs via Advisor to consider past interactions with the model, augment the prompt with additional contextual documents, and introduce agentic behavior.

The structure of prompts has evolved over time within the AI field. Initially, prompts were simple strings. Over time, they grew to include placeholders for specific inputs, like "USER:", which the AI model recognizes. OpenAI have introduced even more structure to prompts by categorizing multiple message strings into distinct roles before they are processed by the AI model.

It is common to use the call() method of ChatModel that takes a Prompt instance and returns a ChatResponse.

The Prompt class functions as a container for an organized series of Message objects and a request ChatOptions. Every Message embodies a unique role within the prompt, differing in its content and intent. These roles can encompass a variety of elements, from user inquiries to AI-generated responses to relevant background information. This arrangement enables intricate and detailed interactions with AI models, as the prompt is constructed from multiple messages, each assigned a specific role to play in the dialogue.

Below is a truncated version of the Prompt class, with constructors and utility methods omitted for brevity:

The Message interface encapsulates a Prompt textual content, a collection of metadata attributes, and a categorization known as MessageType.

The interface is defined as follows:

The multimodal message types implement also the MediaContent interface providing a list of Media content objects.

Various implementations of the Message interface correspond to different categories of messages that an AI model can process. The Models distinguish between message categories based on conversational roles.

These roles are effectively mapped by the MessageType, as discussed below.

Each message is assigned a specific role. These roles categorize the messages, clarifying the context and purpose of each segment of the prompt for the AI model. This structured approach enhances the nuance and effectiveness of communication with the AI, as each part of the prompt plays a distinct and defined role in the interaction.

The primary roles are:

System Role: Guides the AI’s behavior and response style, setting parameters or rules for how the AI interprets and replies to the input. It’s akin to providing instructions to the AI before initiating a conversation.

User Role: Represents the user’s input – their questions, commands, or statements to the AI. This role is fundamental as it forms the basis of the AI’s response.

Assistant Role: The AI’s response to the user’s input. More than just an answer or reaction, it’s crucial for maintaining the flow of the conversation. By tracking the AI’s previous responses (its 'Assistant Role' messages), the system ensures coherent and contextually relevant interactions. The Assistant message may contain Function Tool Call request information as well. It’s like a special feature in the AI, used when needed to perform specific functions such as calculations, fetching data, or other tasks beyond just talking.

Tool/Function Role: The Tool/Function Role focuses on returning additional information in response to Tool Call Assistant Messages.

Roles are represented as an enumeration in Spring AI as shown below

A key component for prompt templating in Spring AI is the PromptTemplate class, designed to facilitate the creation of structured prompts that are then sent to the AI model for processing

This class uses the TemplateRenderer API to render templates. By default, Spring AI uses the StTemplateRenderer implementation, which is based on the open-source StringTemplate engine developed by Terence Parr. Template variables are identified by the {} syntax, but you can configure the delimiters to use other syntax as well.

Spring AI uses the TemplateRenderer interface to handle the actual substitution of variables into the template string. The default implementation uses [StringTemplate]. You can provide your own implementation of TemplateRenderer if you need custom logic. For scenarios where no template rendering is required (e.g., the template string is already complete), you can use the provided NoOpTemplateRenderer.

The interfaces implemented by this class support different aspects of prompt creation:

PromptTemplateStringActions focuses on creating and rendering prompt strings, representing the most basic form of prompt generation.

PromptTemplateMessageActions is tailored for prompt creation through the generation and manipulation of Message objects.

PromptTemplateActions is designed to return the Prompt object, which can be passed to ChatModel for generating a response.

While these interfaces might not be used extensively in many projects, they show the different approaches to prompt creation.

The implemented interfaces are

The method String render(): Renders a prompt template into a final string format without external input, suitable for templates without placeholders or dynamic content.

The method String render(Map<String, Object> model): Enhances rendering functionality to include dynamic content. It uses a Map<String, Object> where map keys are placeholder names in the prompt template, and values are the dynamic content to be inserted.

The method Message createMessage(): Creates a Message object without additional data, used for static or predefined message content.

The method Message createMessage(List<Media> mediaList): Creates a Message object with static textual and media content.

The method Message createMessage(Map<String, Object> model): Extends message creation to integrate dynamic content, accepting a Map<String, Object> where each entry represents a placeholder in the message template and its corresponding dynamic value.

The method Prompt create(): Generates a Prompt object without external data inputs, ideal for static or predefined prompts.

The method Prompt create(ChatOptions modelOptions): Generates a Prompt object without external data inputs and with specific options for the chat request.

The method Prompt create(Map<String, Object> model): Expands prompt creation capabilities to include dynamic content, taking a Map<String, Object> where each map entry is a placeholder in the prompt template and its associated dynamic value.

The method Prompt create(Map<String, Object> model, ChatOptions modelOptions): Expands prompt creation capabilities to include dynamic content, taking a Map<String, Object> where each map entry is a placeholder in the prompt template and its associated dynamic value, and specific options for the chat request.

A simple example taken from the AI Workshop on PromptTemplates is shown below.

Another example taken from the AI Workshop on Roles is shown below.

This shows how you can build up the Prompt instance by using the SystemPromptTemplate to create a Message with the system role passing in placeholder values. The message with the role user is then combined with the message of the role system to form the prompt. The prompt is then passed to the ChatModel to get a generative response.

You can use a custom template renderer by implementing the TemplateRenderer interface and passing it to the PromptTemplate constructor. You can also keep using the default StTemplateRenderer, but with a custom configuration.

By default, template variables are identified by the {} syntax. If you’re planning to include JSON in your prompt, you might want to use a different syntax to avoid conflicts with JSON syntax. For example, you can use the < and > delimiters.

Spring AI supports the org.springframework.core.io.Resource abstraction, so you can put prompt data in a file that can directly be used in a PromptTemplate. For example, you can define a field in your Spring managed component to retrieve the Resource.

and then pass that resource to the SystemPromptTemplate directly.

In generative AI, the creation of prompts is a crucial task for developers. The quality and structure of these prompts significantly influence the effectiveness of the AI’s output. Investing time and effort in designing thoughtful prompts can greatly improve the results from the AI.

Sharing and discussing prompts is a common practice in the AI community. This collaborative approach not only creates a shared learning environment but also leads to the identification and use of highly effective prompts.

Research in this area often involves analyzing and comparing different prompts to assess their effectiveness in various situations. For example, a significant study demonstrated that starting a prompt with "Take a deep breath and work on this problem step by step" significantly enhanced problem-solving efficiency. This highlights the impact that well-chosen language can have on generative AI systems' performance.

Grasping the most effective use of prompts, particularly with the rapid advancement of AI technologies, is a continuous challenge. You should recognize the importance of prompt engineering and consider using insights from the community and research to improve prompt creation strategies.

When developing prompts, it’s important to integrate several key components to ensure clarity and effectiveness:

Instructions: Offer clear and direct instructions to the AI, similar to how you would communicate with a person. This clarity is essential for helping the AI "understand" what is expected.

External Context: Include relevant background information or specific guidance for the AI’s response when necessary. This "external context" frames the prompt and aids the AI in grasping the overall scenario.

User Input: This is the straightforward part - the user’s direct request or question forming the core of the prompt.

Output Indicator: This aspect can be tricky. It involves specifying the desired format for the AI’s response, such as JSON. However, be aware that the AI might not always adhere strictly to this format. For instance, it might prepend a phrase like "here is your JSON" before the actual JSON data, or sometimes generate a JSON-like structure that is not accurate.

Providing the AI with examples of the anticipated question and answer format can be highly beneficial when crafting prompts. This practice helps the AI "understand" the structure and intent of your query, leading to more precise and relevant responses. While this documentation does not delve deeply into these techniques, they provide a starting point for further exploration in AI prompt engineering.

Following is a list of resources for further investigation.

Text Summarization: Reduces extensive text into concise summaries, capturing key points and main ideas while omitting less critical details.

Question Answering: Focuses on deriving specific answers from provided text, based on user-posed questions. It’s about pinpointing and extracting relevant information in response to queries.

Text Classification: Systematically categorizes text into predefined categories or groups, analyzing the text and assigning it to the most fitting category based on its content.

Conversation: Creates interactive dialogues where the AI can engage in back-and-forth communication with users, simulating a natural conversation flow.

Code Generation: Generates functional code snippets based on specific user requirements or descriptions, translating natural language instructions into executable code.

Zero-shot, Few-shot Learning: Enables the model to make accurate predictions or responses with minimal to no prior examples of the specific problem type, understanding and acting on new tasks using learned generalizations.

Chain-of-Thought: Links multiple AI responses to create a coherent and contextually aware conversation. It helps the AI maintain the thread of the discussion, ensuring relevance and continuity.

ReAct (Reason + Act): In this method, the AI first analyzes (reasons about) the input, then determines the most appropriate course of action or response. It combines understanding with decision-making.

Framework for Prompt Creation and Optimization: Microsoft offers a structured approach to developing and refining prompts. This framework guides users in creating effective prompts that elicit the desired responses from AI models, optimizing the interaction for clarity and efficiency.

Tokens are essential in how AI models process text, acting as a bridge that converts words (as we understand them) into a format that AI models can process. This conversion occurs in two stages: words are transformed into tokens upon input, and these tokens are then converted back into words in the output.

Tokenization, the process of breaking down text into tokens, is fundamental to how AI models comprehend and process language. The AI model works with this tokenized format to understand and respond to prompts.

To better understand tokens, think of them as portions of words. Typically, a token represents about three-quarters of a word. For instance, the complete works of Shakespeare, totaling roughly 900,000 words, would translate to around 1.2 million tokens.

Experiment with the OpenAI Tokenizer UI to see how words are converted into tokens.

Tokens have practical implications beyond their technical role in AI processing, especially regarding billing and model capabilities:

Billing: AI model services often bill based on token usage. Both the input (prompt) and the output (response) contribute to the total token count, making shorter prompts more cost-effective.

Model Limits: Different AI models have varying token limits, defining their "context window" – the maximum amount of information they can process at a time. For example, GPT-3’s limit is 4K tokens, while other models like Claude 2 and Meta Llama 2 have limits of 100K tokens, and some research models can handle up to 1 million tokens.

Context Window: A model’s token limit determines its context window. Inputs exceeding this limit are not processed by the model. It’s crucial to send only the minimal effective set of information for processing. For example, when inquiring about "Hamlet," there’s no need to include tokens from all of Shakespeare’s other works.

Response Metadata: The metadata of a response from an AI model includes the number of tokens used, a vital piece of information for managing usage and costs.

**Examples:**

Example 1 (java):
```java
public class Prompt implements ModelRequest<List<Message>> {

    private final List<Message> messages;

    private ChatOptions chatOptions;
}
```

Example 2 (java):
```java
public interface Content {

	String getContent();

	Map<String, Object> getMetadata();
}

public interface Message extends Content {

	MessageType getMessageType();
}
```

Example 3 (java):
```java
public interface MediaContent extends Content {

	Collection<Media> getMedia();

}
```

Example 4 (java):
```java
public enum MessageType {

	USER("user"),

	ASSISTANT("assistant"),

	SYSTEM("system"),

	TOOL("tool");

    ...
}
```

---

## Prompts :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/prompt.html

**Contents:**
- Prompts
- API Overview
  - Prompt
  - Message
    - Roles
  - PromptTemplate
- Example Usage
  - Using a custom template renderer
  - Using resources instead of raw Strings
- Prompt Engineering

Prompts are the inputs that guide an AI model to generate specific outputs. The design and phrasing of these prompts significantly influence the model’s responses.

At the lowest level of interaction with AI models in Spring AI, handling prompts in Spring AI is somewhat similar to managing the "View" in Spring MVC. This involves creating extensive text with placeholders for dynamic content. These placeholders are then replaced based on user requests or other code in the application. Another analogy is a SQL statement that contain placeholders for certain expressions.

As Spring AI evolves, it will introduce higher levels of abstraction for interacting with AI models. The foundational classes described in this section can be likened to JDBC in terms of their role and functionality. The ChatModel class, for instance, is analogous to the core JDBC library in the JDK. The ChatClient class can be likened to the JdbcClient, built on top of ChatModel and providing more advanced constructs via Advisor to consider past interactions with the model, augment the prompt with additional contextual documents, and introduce agentic behavior.

The structure of prompts has evolved over time within the AI field. Initially, prompts were simple strings. Over time, they grew to include placeholders for specific inputs, like "USER:", which the AI model recognizes. OpenAI have introduced even more structure to prompts by categorizing multiple message strings into distinct roles before they are processed by the AI model.

It is common to use the call() method of ChatModel that takes a Prompt instance and returns a ChatResponse.

The Prompt class functions as a container for an organized series of Message objects and a request ChatOptions. Every Message embodies a unique role within the prompt, differing in its content and intent. These roles can encompass a variety of elements, from user inquiries to AI-generated responses to relevant background information. This arrangement enables intricate and detailed interactions with AI models, as the prompt is constructed from multiple messages, each assigned a specific role to play in the dialogue.

Below is a truncated version of the Prompt class, with constructors and utility methods omitted for brevity:

The Message interface encapsulates a Prompt textual content, a collection of metadata attributes, and a categorization known as MessageType.

The interface is defined as follows:

The multimodal message types implement also the MediaContent interface providing a list of Media content objects.

Various implementations of the Message interface correspond to different categories of messages that an AI model can process. The Models distinguish between message categories based on conversational roles.

These roles are effectively mapped by the MessageType, as discussed below.

Each message is assigned a specific role. These roles categorize the messages, clarifying the context and purpose of each segment of the prompt for the AI model. This structured approach enhances the nuance and effectiveness of communication with the AI, as each part of the prompt plays a distinct and defined role in the interaction.

The primary roles are:

System Role: Guides the AI’s behavior and response style, setting parameters or rules for how the AI interprets and replies to the input. It’s akin to providing instructions to the AI before initiating a conversation.

User Role: Represents the user’s input – their questions, commands, or statements to the AI. This role is fundamental as it forms the basis of the AI’s response.

Assistant Role: The AI’s response to the user’s input. More than just an answer or reaction, it’s crucial for maintaining the flow of the conversation. By tracking the AI’s previous responses (its 'Assistant Role' messages), the system ensures coherent and contextually relevant interactions. The Assistant message may contain Function Tool Call request information as well. It’s like a special feature in the AI, used when needed to perform specific functions such as calculations, fetching data, or other tasks beyond just talking.

Tool/Function Role: The Tool/Function Role focuses on returning additional information in response to Tool Call Assistant Messages.

Roles are represented as an enumeration in Spring AI as shown below

A key component for prompt templating in Spring AI is the PromptTemplate class, designed to facilitate the creation of structured prompts that are then sent to the AI model for processing

This class uses the TemplateRenderer API to render templates. By default, Spring AI uses the StTemplateRenderer implementation, which is based on the open-source StringTemplate engine developed by Terence Parr. Template variables are identified by the {} syntax, but you can configure the delimiters to use other syntax as well.

Spring AI uses the TemplateRenderer interface to handle the actual substitution of variables into the template string. The default implementation uses [StringTemplate]. You can provide your own implementation of TemplateRenderer if you need custom logic. For scenarios where no template rendering is required (e.g., the template string is already complete), you can use the provided NoOpTemplateRenderer.

The interfaces implemented by this class support different aspects of prompt creation:

PromptTemplateStringActions focuses on creating and rendering prompt strings, representing the most basic form of prompt generation.

PromptTemplateMessageActions is tailored for prompt creation through the generation and manipulation of Message objects.

PromptTemplateActions is designed to return the Prompt object, which can be passed to ChatModel for generating a response.

While these interfaces might not be used extensively in many projects, they show the different approaches to prompt creation.

The implemented interfaces are

The method String render(): Renders a prompt template into a final string format without external input, suitable for templates without placeholders or dynamic content.

The method String render(Map<String, Object> model): Enhances rendering functionality to include dynamic content. It uses a Map<String, Object> where map keys are placeholder names in the prompt template, and values are the dynamic content to be inserted.

The method Message createMessage(): Creates a Message object without additional data, used for static or predefined message content.

The method Message createMessage(List<Media> mediaList): Creates a Message object with static textual and media content.

The method Message createMessage(Map<String, Object> model): Extends message creation to integrate dynamic content, accepting a Map<String, Object> where each entry represents a placeholder in the message template and its corresponding dynamic value.

The method Prompt create(): Generates a Prompt object without external data inputs, ideal for static or predefined prompts.

The method Prompt create(ChatOptions modelOptions): Generates a Prompt object without external data inputs and with specific options for the chat request.

The method Prompt create(Map<String, Object> model): Expands prompt creation capabilities to include dynamic content, taking a Map<String, Object> where each map entry is a placeholder in the prompt template and its associated dynamic value.

The method Prompt create(Map<String, Object> model, ChatOptions modelOptions): Expands prompt creation capabilities to include dynamic content, taking a Map<String, Object> where each map entry is a placeholder in the prompt template and its associated dynamic value, and specific options for the chat request.

A simple example taken from the AI Workshop on PromptTemplates is shown below.

Another example taken from the AI Workshop on Roles is shown below.

This shows how you can build up the Prompt instance by using the SystemPromptTemplate to create a Message with the system role passing in placeholder values. The message with the role user is then combined with the message of the role system to form the prompt. The prompt is then passed to the ChatModel to get a generative response.

You can use a custom template renderer by implementing the TemplateRenderer interface and passing it to the PromptTemplate constructor. You can also keep using the default StTemplateRenderer, but with a custom configuration.

By default, template variables are identified by the {} syntax. If you’re planning to include JSON in your prompt, you might want to use a different syntax to avoid conflicts with JSON syntax. For example, you can use the < and > delimiters.

Spring AI supports the org.springframework.core.io.Resource abstraction, so you can put prompt data in a file that can directly be used in a PromptTemplate. For example, you can define a field in your Spring managed component to retrieve the Resource.

and then pass that resource to the SystemPromptTemplate directly.

In generative AI, the creation of prompts is a crucial task for developers. The quality and structure of these prompts significantly influence the effectiveness of the AI’s output. Investing time and effort in designing thoughtful prompts can greatly improve the results from the AI.

Sharing and discussing prompts is a common practice in the AI community. This collaborative approach not only creates a shared learning environment but also leads to the identification and use of highly effective prompts.

Research in this area often involves analyzing and comparing different prompts to assess their effectiveness in various situations. For example, a significant study demonstrated that starting a prompt with "Take a deep breath and work on this problem step by step" significantly enhanced problem-solving efficiency. This highlights the impact that well-chosen language can have on generative AI systems' performance.

Grasping the most effective use of prompts, particularly with the rapid advancement of AI technologies, is a continuous challenge. You should recognize the importance of prompt engineering and consider using insights from the community and research to improve prompt creation strategies.

When developing prompts, it’s important to integrate several key components to ensure clarity and effectiveness:

Instructions: Offer clear and direct instructions to the AI, similar to how you would communicate with a person. This clarity is essential for helping the AI "understand" what is expected.

External Context: Include relevant background information or specific guidance for the AI’s response when necessary. This "external context" frames the prompt and aids the AI in grasping the overall scenario.

User Input: This is the straightforward part - the user’s direct request or question forming the core of the prompt.

Output Indicator: This aspect can be tricky. It involves specifying the desired format for the AI’s response, such as JSON. However, be aware that the AI might not always adhere strictly to this format. For instance, it might prepend a phrase like "here is your JSON" before the actual JSON data, or sometimes generate a JSON-like structure that is not accurate.

Providing the AI with examples of the anticipated question and answer format can be highly beneficial when crafting prompts. This practice helps the AI "understand" the structure and intent of your query, leading to more precise and relevant responses. While this documentation does not delve deeply into these techniques, they provide a starting point for further exploration in AI prompt engineering.

Following is a list of resources for further investigation.

Text Summarization: Reduces extensive text into concise summaries, capturing key points and main ideas while omitting less critical details.

Question Answering: Focuses on deriving specific answers from provided text, based on user-posed questions. It’s about pinpointing and extracting relevant information in response to queries.

Text Classification: Systematically categorizes text into predefined categories or groups, analyzing the text and assigning it to the most fitting category based on its content.

Conversation: Creates interactive dialogues where the AI can engage in back-and-forth communication with users, simulating a natural conversation flow.

Code Generation: Generates functional code snippets based on specific user requirements or descriptions, translating natural language instructions into executable code.

Zero-shot, Few-shot Learning: Enables the model to make accurate predictions or responses with minimal to no prior examples of the specific problem type, understanding and acting on new tasks using learned generalizations.

Chain-of-Thought: Links multiple AI responses to create a coherent and contextually aware conversation. It helps the AI maintain the thread of the discussion, ensuring relevance and continuity.

ReAct (Reason + Act): In this method, the AI first analyzes (reasons about) the input, then determines the most appropriate course of action or response. It combines understanding with decision-making.

Framework for Prompt Creation and Optimization: Microsoft offers a structured approach to developing and refining prompts. This framework guides users in creating effective prompts that elicit the desired responses from AI models, optimizing the interaction for clarity and efficiency.

Tokens are essential in how AI models process text, acting as a bridge that converts words (as we understand them) into a format that AI models can process. This conversion occurs in two stages: words are transformed into tokens upon input, and these tokens are then converted back into words in the output.

Tokenization, the process of breaking down text into tokens, is fundamental to how AI models comprehend and process language. The AI model works with this tokenized format to understand and respond to prompts.

To better understand tokens, think of them as portions of words. Typically, a token represents about three-quarters of a word. For instance, the complete works of Shakespeare, totaling roughly 900,000 words, would translate to around 1.2 million tokens.

Experiment with the OpenAI Tokenizer UI to see how words are converted into tokens.

Tokens have practical implications beyond their technical role in AI processing, especially regarding billing and model capabilities:

Billing: AI model services often bill based on token usage. Both the input (prompt) and the output (response) contribute to the total token count, making shorter prompts more cost-effective.

Model Limits: Different AI models have varying token limits, defining their "context window" – the maximum amount of information they can process at a time. For example, GPT-3’s limit is 4K tokens, while other models like Claude 2 and Meta Llama 2 have limits of 100K tokens, and some research models can handle up to 1 million tokens.

Context Window: A model’s token limit determines its context window. Inputs exceeding this limit are not processed by the model. It’s crucial to send only the minimal effective set of information for processing. For example, when inquiring about "Hamlet," there’s no need to include tokens from all of Shakespeare’s other works.

Response Metadata: The metadata of a response from an AI model includes the number of tokens used, a vital piece of information for managing usage and costs.

**Examples:**

Example 1 (java):
```java
public class Prompt implements ModelRequest<List<Message>> {

    private final List<Message> messages;

    private ChatOptions chatOptions;
}
```

Example 2 (java):
```java
public interface Content {

	String getContent();

	Map<String, Object> getMetadata();
}

public interface Message extends Content {

	MessageType getMessageType();
}
```

Example 3 (java):
```java
public interface MediaContent extends Content {

	Collection<Media> getMedia();

}
```

Example 4 (java):
```java
public enum MessageType {

	USER("user"),

	ASSISTANT("assistant"),

	SYSTEM("system"),

	TOOL("tool");

    ...
}
```

---

## Upgrade Notes :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/upgrade-notes.html

**Contents:**
- Upgrade Notes
- Upgrading to 1.0.0-SNAPSHOT
  - Overview
  - Add Snapshot Repositories
  - Update Dependency Management
  - Artifact ID, Package, and Module Changes
- Upgrading to 1.0.0-RC1
  - Breaking Changes
    - Chat Client and Advisors
      - Self-contained Templates in Advisors

For the latest snapshot version, please use Spring AI 1.1.2!

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

Example 1 (java):
```java
// Before
new MessageAggregator().aggregateChatClientResponse(chatClientResponses, aggregationHandler);

// After
new ChatClientMessageAggregator().aggregateChatClientResponse(chatClientResponses, aggregationHandler);
```

Example 2 (java):
```java
import org.springframework.ai.chat.client.ChatClientMessageAggregator;
```

Example 3 (java):
```java
// This worked in M7 but silently fails in M8
ChatClient chatClient = new OpenAiChatClient(api)
    .tools(List.of(
        new Tool("get_current_weather", "Get the current weather in a given location",
            new ToolSpecification.ToolParameter("location", "The city and state, e.g. San Francisco, CA", true))
    ))
    .toolCallbacks(List.of(
        new ToolCallback("get_current_weather", (toolName, params) -> {
            // Weather retrieval logic
            return Map.of("temperature", 72, "unit", "fahrenheit", "description", "Sunny");
        })
    ));
```

Example 4 (java):
```java
// This works in M8
ChatClient chatClient = new OpenAiChatClient(api)
    .toolSpecifications(List.of(
        new Tool("get_current_weather", "Get the current weather in a given location",
            new ToolSpecification.ToolParameter("location", "The city and state, e.g. San Francisco, CA", true))
    ))
    .toolCallbacks(List.of(
        new ToolCallback("get_current_weather", (toolName, params) -> {
            // Weather retrieval logic
            return Map.of("temperature", 72, "unit", "fahrenheit", "description", "Sunny");
        })
    ));
```

---

## Cloud Bindings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/cloud-bindings.html

**Contents:**
- Cloud Bindings
- Available Cloud Bindings

For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI provides support for cloud bindings based on the foundations in spring-cloud-bindings. This allows applications to specify a binding type for a provider and then express properties using a generic format. The spring-ai cloud bindings will process these properties and bind them to spring-ai native properties.

For example, when using OpenAi, the binding type is openai. Using the property spring.ai.cloud.bindings.openai.enabled, the binding processor can be enabled or disabled. By default, when specifying a binding type, this property will be enabled. Configuration for api-key, uri, username, password, etc. can be specified and spring-ai will map them to the corresponding properties in the supported system.

To enable cloud binding support, include the following dependency in the application.

or to your Gradle build.gradle build file.

The following are the components for which the cloud binding support is currently available in the spring-ai-spring-cloud-bindings module:

uri, username, password

spring.ai.vectorstore.chroma.client.host, spring.ai.vectorstore.chroma.client.port, spring.ai.vectorstore.chroma.client.username, spring.ai.vectorstore.chroma.client.host.password

spring.ai.mistralai.api-key, spring.ai.mistralai.base-url

spring.ai.ollama.base-url

spring.ai.openai.api-key, spring.ai.openai.base-url

spring.ai.vectorstore.weaviate.scheme, spring.ai.vectorstore.weaviate.host, spring.ai.vectorstore.weaviate.api-key

uri, api-key, model-capabilities (chat and embedding), model-name

spring.ai.openai.chat.base-url, spring.ai.openai.chat.api-key, spring.ai.openai.chat.options.model, spring.ai.openai.embedding.base-url, spring.ai.openai.embedding.api-key, spring.ai.openai.embedding.options.model

**Examples:**

Example 1 (xml):
```xml
<dependency>
   <groupId>org.springframework.ai</groupId>
   <artifactId>spring-ai-spring-cloud-bindings</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-spring-cloud-bindings'
}
```

---

## Migrating from FunctionCallback to ToolCallback API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/tools-migration.html

**Contents:**
- Migrating from FunctionCallback to ToolCallback API
- Overview of Changes
- Key Changes
- Migration Examples
  - 1. Basic Function Callback
  - 2. ChatClient Usage
  - 3. Method-Based Function Callbacks
  - 4. Options Configuration
  - 5. Default Functions in ChatClient Builder
  - 6. Spring Bean Configuration

This guide helps you migrate from the deprecated FunctionCallback API to the new ToolCallback API in Spring AI. For more information about the new APIs, check out the Tools Calling documentation.

These changes are part of a broader effort to improve and extend the tool calling capabilities in Spring AI. Among the other things, the new API moves from "functions" to "tools" terminology to better align with industry conventions. This involves several API changes while maintaining backward compatibility through deprecated methods.

FunctionCallback → ToolCallback

FunctionCallback.builder().function() → FunctionToolCallback.builder()

FunctionCallback.builder().method() → MethodToolCallback.builder()

FunctionCallingOptions → ToolCallingChatOptions

ChatClient.builder().defaultFunctions() → ChatClient.builder().defaultTools()

ChatClient.functions() → ChatClient.tools()

FunctionCallingOptions.builder().functions() → ToolCallingChatOptions.builder().toolNames()

FunctionCallingOptions.builder().functionCallbacks() → ToolCallingChatOptions.builder().toolCallbacks()

Or with the declarative approach:

And you can use the same ChatClient#tools() API to register method-based tool callbacks:

Or with the declarative approach:

The method() configuration in function callbacks has been replaced with a more explicit method tool configuration using ToolDefinition and MethodToolCallback.

When using method-based callbacks, you now need to explicitly find the method using ReflectionUtils and provide it to the builder. Alternatively, you can use the declarative approach with the @Tool annotation.

For non-static methods, you must now provide both the method and the target object:

The following methods are deprecated and will be removed in a future release:

ChatClient.Builder.defaultFunctions(String…​)

ChatClient.Builder.defaultFunctions(FunctionCallback…​)

ChatClient.RequestSpec.functions()

Use their tools counterparts instead.

Now you can use the method-level annotation (@Tool) to register tools with Spring AI:

The new API provides better separation between tool definition and implementation.

Tool definitions can be reused across different implementations.

The builder pattern has been simplified for common use cases.

Better support for method-based tools with improved error handling.

The deprecated methods will be maintained for backward compatibility in the current milestone version but will be removed in the next milestone release. It’s recommended to migrate to the new API as soon as possible.

**Examples:**

Example 1 (java):
```java
FunctionCallback.builder()
    .function("getCurrentWeather", new MockWeatherService())
    .description("Get the weather in location")
    .inputType(MockWeatherService.Request.class)
    .build()
```

Example 2 (java):
```java
FunctionToolCallback.builder("getCurrentWeather", new MockWeatherService())
    .description("Get the weather in location")
    .inputType(MockWeatherService.Request.class)
    .build()
```

Example 3 (java):
```java
String response = ChatClient.create(chatModel)
    .prompt()
    .user("What's the weather like in San Francisco?")
    .functions(FunctionCallback.builder()
        .function("getCurrentWeather", new MockWeatherService())
        .description("Get the weather in location")
        .inputType(MockWeatherService.Request.class)
        .build())
    .call()
    .content();
```

Example 4 (java):
```java
String response = ChatClient.create(chatModel)
    .prompt()
    .user("What's the weather like in San Francisco?")
    .tools(FunctionToolCallback.builder("getCurrentWeather", new MockWeatherService())
        .description("Get the weather in location")
        .inputType(MockWeatherService.Request.class)
        .build())
    .call()
    .content();
```

---

## Testcontainers :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/testcontainers.html

**Contents:**
- Testcontainers
- Service Connections

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI provides Spring Boot auto-configuration for establishing a connection to a model service or vector store running via Testcontainers. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The following service connection factories are provided in the spring-ai-spring-boot-testcontainers module:

AwsOpenSearchConnectionDetails

Containers of type LocalStackContainer

ChromaConnectionDetails

Containers of type ChromaDBContainer

McpSseClientConnectionDetails

Containers of type DockerMcpGatewayContainer

MilvusServiceClientConnectionDetails

Containers of type MilvusContainer

MongoConnectionDetails

Containers of type MongoDBAtlasLocalContainer

OllamaConnectionDetails

Containers of type OllamaContainer

OpenSearchConnectionDetails

Containers of type OpensearchContainer

QdrantConnectionDetails

Containers of type QdrantContainer

TypesenseConnectionDetails

Containers of type TypesenseContainer

WeaviateConnectionDetails

Containers of type WeaviateContainer

More service connections are provided by the spring boot module spring-boot-testcontainers. Refer to the Testcontainers Service Connections documentation page for the full list.

**Examples:**

Example 1 (xml):
```xml
<dependency>
   <groupId>org.springframework.ai</groupId>
   <artifactId>spring-ai-spring-boot-testcontainers</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-spring-boot-testcontainers'
}
```

---

## Migrating from FunctionCallback to ToolCallback API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/tools-migration.html

**Contents:**
- Migrating from FunctionCallback to ToolCallback API
- Overview of Changes
- Key Changes
- Migration Examples
  - 1. Basic Function Callback
  - 2. ChatClient Usage
  - 3. Method-Based Function Callbacks
  - 4. Options Configuration
  - 5. Default Functions in ChatClient Builder
  - 6. Spring Bean Configuration

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This guide helps you migrate from the deprecated FunctionCallback API to the new ToolCallback API in Spring AI. For more information about the new APIs, check out the Tools Calling documentation.

These changes are part of a broader effort to improve and extend the tool calling capabilities in Spring AI. Among the other things, the new API moves from "functions" to "tools" terminology to better align with industry conventions. This involves several API changes while maintaining backward compatibility through deprecated methods.

FunctionCallback → ToolCallback

FunctionCallback.builder().function() → FunctionToolCallback.builder()

FunctionCallback.builder().method() → MethodToolCallback.builder()

FunctionCallingOptions → ToolCallingChatOptions

ChatClient.builder().defaultFunctions() → ChatClient.builder().defaultTools()

ChatClient.functions() → ChatClient.tools()

FunctionCallingOptions.builder().functions() → ToolCallingChatOptions.builder().toolNames()

FunctionCallingOptions.builder().functionCallbacks() → ToolCallingChatOptions.builder().toolCallbacks()

Or with the declarative approach:

And you can use the same ChatClient#tools() API to register method-based tool callbacks:

Or with the declarative approach:

The method() configuration in function callbacks has been replaced with a more explicit method tool configuration using ToolDefinition and MethodToolCallback.

When using method-based callbacks, you now need to explicitly find the method using ReflectionUtils and provide it to the builder. Alternatively, you can use the declarative approach with the @Tool annotation.

For non-static methods, you must now provide both the method and the target object:

The following methods are deprecated and will be removed in a future release:

ChatClient.Builder.defaultFunctions(String…​)

ChatClient.Builder.defaultFunctions(FunctionCallback…​)

ChatClient.RequestSpec.functions()

Use their tools counterparts instead.

Now you can use the method-level annotation (@Tool) to register tools with Spring AI:

The new API provides better separation between tool definition and implementation.

Tool definitions can be reused across different implementations.

The builder pattern has been simplified for common use cases.

Better support for method-based tools with improved error handling.

The deprecated methods will be maintained for backward compatibility in the current milestone version but will be removed in the next milestone release. It’s recommended to migrate to the new API as soon as possible.

**Examples:**

Example 1 (java):
```java
FunctionCallback.builder()
    .function("getCurrentWeather", new MockWeatherService())
    .description("Get the weather in location")
    .inputType(MockWeatherService.Request.class)
    .build()
```

Example 2 (java):
```java
FunctionToolCallback.builder("getCurrentWeather", new MockWeatherService())
    .description("Get the weather in location")
    .inputType(MockWeatherService.Request.class)
    .build()
```

Example 3 (java):
```java
String response = ChatClient.create(chatModel)
    .prompt()
    .user("What's the weather like in San Francisco?")
    .functions(FunctionCallback.builder()
        .function("getCurrentWeather", new MockWeatherService())
        .description("Get the weather in location")
        .inputType(MockWeatherService.Request.class)
        .build())
    .call()
    .content();
```

Example 4 (java):
```java
String response = ChatClient.create(chatModel)
    .prompt()
    .user("What's the weather like in San Francisco?")
    .tools(FunctionToolCallback.builder("getCurrentWeather", new MockWeatherService())
        .description("Get the weather in location")
        .inputType(MockWeatherService.Request.class)
        .build())
    .call()
    .content();
```

---

## Tool Calling :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/tools.html

**Contents:**
- Tool Calling
- Quick Start
  - Information Retrieval
  - Taking Actions
- Overview
- Methods as Tools
  - Declarative Specification: @Tool
    - Adding Tools to ChatClient
    - Adding Default Tools to ChatClient
    - Adding Tools to ChatModel

Tool calling (also known as function calling) is a common pattern in AI applications allowing a model to interact with a set of APIs, or tools, augmenting its capabilities.

Tools are mainly used for:

Information Retrieval. Tools in this category can be used to retrieve information from external sources, such as a database, a web service, a file system, or a web search engine. The goal is to augment the knowledge of the model, allowing it to answer questions that it would not be able to answer otherwise. As such, they can be used in Retrieval Augmented Generation (RAG) scenarios. For example, a tool can be used to retrieve the current weather for a given location, to retrieve the latest news articles, or to query a database for a specific record.

Taking Action. Tools in this category can be used to take action in a software system, such as sending an email, creating a new record in a database, submitting a form, or triggering a workflow. The goal is to automate tasks that would otherwise require human intervention or explicit programming. For example, a tool can be used to book a flight for a customer interacting with a chatbot, to fill out a form on a web page, or to implement a Java class based on an automated test (TDD) in a code generation scenario.

Even though we typically refer to tool calling as a model capability, it is actually up to the client application to provide the tool calling logic. The model can only request a tool call and provide the input arguments, whereas the application is responsible for executing the tool call from the input arguments and returning the result. The model never gets access to any of the APIs provided as tools, which is a critical security consideration.

Spring AI provides convenient APIs to define tools, resolve tool call requests from a model, and execute the tool calls. The following sections provide an overview of the tool calling capabilities in Spring AI.

Let’s see how to start using tool calling in Spring AI. We’ll implement two simple tools: one for information retrieval and one for taking action. The information retrieval tool will be used to get the current date and time in the user’s time zone. The action tool will be used to set an alarm for a specified time.

AI models don’t have access to real-time information. Any question that assumes awareness of information such as the current date or weather forecast cannot be answered by the model. However, we can provide a tool that can retrieve this information, and let the model call this tool when access to real-time information is needed.

Let’s implement a tool to get the current date and time in the user’s time zone in a DateTimeTools class. The tool will take no argument. The LocaleContextHolder from Spring Framework can provide the user’s time zone. The tool will be defined as a method annotated with @Tool. To help the model understand if and when to call this tool, we’ll provide a detailed description of what the tools does.

Next, let’s make the tool available to the model. In this example, we’ll use the ChatClient to interact with the model. We’ll provide the tool to the model by passing an instance of DateTimeTools via the tools() method. When the model needs to know the current date and time, it will request the tool to be called. Internally, the ChatClient will call the tool and return the result to the model, which will then use the tool call result to generate the final response to the original question.

The output will be something like:

You can retry asking the same question again. This time, don’t provide the tool to the model. The output will be something like:

Without the tool, the model doesn’t know how to answer the question because it doesn’t have the ability to determine the current date and time.

AI models can be used to generate plans for accomplishing certain goals. For example, a model can generate a plan for booking a trip to Denmark. However, the model doesn’t have the ability to execute the plan. That’s where tools come in: they can be used to execute the plan that a model generates.

In the previous example, we used a tool to determine the current date and time. In this example, we’ll define a second tool for setting an alarm at a specific time. The goal is to set an alarm for 10 minutes from now, so we need to provide both tools to the model to accomplish this task.

We’ll add the new tool to the same DateTimeTools class as before. The new tool will take a single parameter, which is the time in ISO-8601 format. The tool will then print a message to the console indicating that the alarm has been set for the given time. Like before, the tool is defined as a method annotated with @Tool, which we also use to provide a detailed description to help the model understand when and how to use the tool.

Next, let’s make both tools available to the model. We’ll use the ChatClient to interact with the model. We’ll provide the tools to the model by passing an instance of DateTimeTools via the tools() method. When we ask to set up an alarm 10 minutes from now, the model will first need to know the current date and time. Then, it will use the current date and time to calculate the alarm time. Finally, it will use the alarm tool to set up the alarm. Internally, the ChatClient will handle any tool call request from the model and send back to it any tool call execution result, so that the model can generate the final response.

In the application logs, you can check the alarm has been set at the correct time.

Spring AI supports tool calling through a set of flexible abstractions that allow you to define, resolve, and execute tools in a consistent way. This section provides an overview of the main concepts and components of tool calling in Spring AI.

When we want to make a tool available to the model, we include its definition in the chat request. Each tool definition comprises of a name, a description, and the schema of the input parameters.

When the model decides to call a tool, it sends a response with the tool name and the input parameters modeled after the defined schema.

The application is responsible for using the tool name to identify and execute the tool with the provided input parameters.

The result of the tool call is processed by the application.

The application sends the tool call result back to the model.

The model generates the final response using the tool call result as additional context.

Tools are the building blocks of tool calling and they are modeled by the ToolCallback interface. Spring AI provides built-in support for specifying ToolCallback(s) from methods and functions, but you can always define your own ToolCallback implementations to support more use cases.

ChatModel implementations transparently dispatch tool call requests to the corresponding ToolCallback implementations and will send the tool call results back to the model, which will ultimately generate the final response. They do so using the ToolCallingManager interface, which is responsible for managing the tool execution lifecycle.

Both ChatClient and ChatModel accept a list of ToolCallback objects to make the tools available to the model and the ToolCallingManager that will eventually execute them.

Besides passing the ToolCallback objects directly, you can also pass a list of tool names, that will be resolved dynamically using the ToolCallbackResolver interface.

The following sections will go into more details about all these concepts and APIs, including how to customize and extend them to support more use cases.

Spring AI provides built-in support for specifying tools (i.e. ToolCallback(s)) from methods in two ways:

declaratively, using the @Tool annotation

programmatically, using the low-level MethodToolCallback implementation.

You can turn a method into a tool by annotating it with @Tool.

The @Tool annotation allows you to provide key information about the tool:

name: The name of the tool. If not provided, the method name will be used. AI models use this name to identify the tool when calling it. Therefore, it’s not allowed to have two tools with the same name in the same class. The name must be unique across all the tools available to the model for a specific chat request.

description: The description for the tool, which can be used by the model to understand when and how to call the tool. If not provided, the method name will be used as the tool description. However, it’s strongly recommended to provide a detailed description because that’s paramount for the model to understand the tool’s purpose and how to use it. Failing in providing a good description can lead to the model not using the tool when it should or using it incorrectly.

returnDirect: Whether the tool result should be returned directly to the client or passed back to the model. See Return Direct for more details.

resultConverter: The ToolCallResultConverter implementation to use for converting the result of a tool call to a String object to send back to the AI model. See Result Conversion for more details.

The method can be either static or instance, and it can have any visibility (public, protected, package-private, or private). The class that contains the method can be either a top-level class or a nested class, and it can also have any visibility (as long as it’s accessible where you’re planning to instantiate it).

You can define any number of arguments for the method (including no argument) with most types (primitives, POJOs, enums, lists, arrays, maps, and so on). Similarly, the method can return most types, including void. If the method returns a value, the return type must be a serializable type, as the result will be serialized and sent back to the model.

Spring AI will generate the JSON schema for the input parameters of the @Tool-annotated method automatically. The schema is used by the model to understand how to call the tool and prepare the tool request. The @ToolParam annotation can be used to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required.

The @ToolParam annotation allows you to provide key information about a tool parameter:

description: The description for the parameter, which can be used by the model to understand better how to use it. For example, what format the parameter should be in, what values are allowed, and so on.

required: Whether the parameter is required or optional. By default, all parameters are considered required.

If a parameter is annotated as @Nullable, it will be considered optional unless explicitly marked as required using the @ToolParam annotation.

Besides the @ToolParam annotation, you can also use the @Schema annotation from Swagger or @JsonProperty from Jackson. See JSON Schema for more details.

When using the declarative specification approach, you can pass the tool class instance to the tools() method when invoking a ChatClient. Such tools will only be available for the specific chat request they are added to.

Under the hood, the ChatClient will generate a ToolCallback from each @Tool-annotated method in the tool class instance and pass them to the model. In case you prefer to generate the ToolCallback(s) yourself, you can use the ToolCallbacks utility class.

When using the declarative specification approach, you can add default tools to a ChatClient.Builder by passing the tool class instance to the defaultTools() method. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

When using the declarative specification approach, you can pass the tool class instance to the toolCallbacks() method of the ToolCallingChatOptions you use to call a ChatModel. Such tools will only be available for the specific chat request they are added to.

When using the declarative specification approach, you can add default tools to ChatModel at construction time by passing the tool class instance to the toolCallbacks() method of the ToolCallingChatOptions instance used to create the ChatModel. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

You can turn a method into a tool by building a MethodToolCallback programmatically.

The MethodToolCallback.Builder allows you to build a MethodToolCallback instance and provide key information about the tool:

toolDefinition: The ToolDefinition instance that defines the tool name, description, and input schema. You can build it using the ToolDefinition.Builder class. Required.

toolMetadata: The ToolMetadata instance that defines additional settings such as whether the result should be returned directly to the client, and the result converter to use. You can build it using the ToolMetadata.Builder class.

toolMethod: The Method instance that represents the tool method. Required.

toolObject: The object instance that contains the tool method. If the method is static, you can omit this parameter.

toolCallResultConverter: The ToolCallResultConverter instance to use for converting the result of a tool call to a String object to send back to the AI model. If not provided, the default converter will be used (DefaultToolCallResultConverter).

The ToolDefinition.Builder allows you to build a ToolDefinition instance and define the tool name, description, and input schema:

name: The name of the tool. If not provided, the method name will be used. AI models use this name to identify the tool when calling it. Therefore, it’s not allowed to have two tools with the same name in the same class. The name must be unique across all the tools available to the model for a specific chat request.

description: The description for the tool, which can be used by the model to understand when and how to call the tool. If not provided, the method name will be used as the tool description. However, it’s strongly recommended to provide a detailed description because that’s paramount for the model to understand the tool’s purpose and how to use it. Failing in providing a good description can lead to the model not using the tool when it should or using it incorrectly.

inputSchema: The JSON schema for the input parameters of the tool. If not provided, the schema will be generated automatically based on the method parameters. You can use the @ToolParam annotation to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required. See JSON Schema for more details.

The ToolMetadata.Builder allows you to build a ToolMetadata instance and define additional settings for the tool:

returnDirect: Whether the tool result should be returned directly to the client or passed back to the model. See Return Direct for more details.

The method can be either static or instance, and it can have any visibility (public, protected, package-private, or private). The class that contains the method can be either a top-level class or a nested class, and it can also have any visibility (as long as it’s accessible where you’re planning to instantiate it).

You can define any number of arguments for the method (including no argument) with most types (primitives, POJOs, enums, lists, arrays, maps, and so on). Similarly, the method can return most types, including void. If the method returns a value, the return type must be a serializable type, as the result will be serialized and sent back to the model.

If the method is static, you can omit the toolObject() method, as it’s not needed.

Spring AI will generate the JSON schema for the input parameters of the method automatically. The schema is used by the model to understand how to call the tool and prepare the tool request. The @ToolParam annotation can be used to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required.

The @ToolParam annotation allows you to provide key information about a tool parameter:

description: The description for the parameter, which can be used by the model to understand better how to use it. For example, what format the parameter should be in, what values are allowed, and so on.

required: Whether the parameter is required or optional. By default, all parameters are considered required.

If a parameter is annotated as @Nullable, it will be considered optional unless explicitly marked as required using the @ToolParam annotation.

Besides the @ToolParam annotation, you can also use the @Schema annotation from Swagger or @JsonProperty from Jackson. See JSON Schema for more details.

When using the programmatic specification approach, you can pass the MethodToolCallback instance to the toolCallbacks() method of ChatClient. The tool will only be available for the specific chat request it’s added to.

When using the programmatic specification approach, you can add default tools to a ChatClient.Builder by passing the MethodToolCallback instance to the defaultToolCallbacks() method. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

When using the programmatic specification approach, you can pass the MethodToolCallback instance to the toolCallbacks() method of the ToolCallingChatOptions you use to call a ChatModel. The tool will only be available for the specific chat request it’s added to.

When using the programmatic specification approach, you can add default tools to a ChatModel at construction time by passing the MethodToolCallback instance to the toolCallbacks() method of the ToolCallingChatOptions instance used to create the ChatModel. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

The following types are not currently supported as parameters or return types for methods used as tools:

Asynchronous types (e.g. CompletableFuture, Future)

Reactive types (e.g. Flow, Mono, Flux)

Functional types (e.g. Function, Supplier, Consumer).

Functional types are supported using the function-based tool specification approach. See Functions as Tools for more details.

Spring AI provides built-in support for specifying tools from functions, either programmatically using the low-level FunctionToolCallback implementation or dynamically as @Bean(s) resolved at runtime.

You can turn a functional type (Function, Supplier, Consumer, or BiFunction) into a tool by building a FunctionToolCallback programmatically.

The FunctionToolCallback.Builder allows you to build a FunctionToolCallback instance and provide key information about the tool:

name: The name of the tool. AI models use this name to identify the tool when calling it. Therefore, it’s not allowed to have two tools with the same name in the same context. The name must be unique across all the tools available to the model for a specific chat request. Required.

toolFunction: The functional object that represents the tool method (Function, Supplier, Consumer, or BiFunction). Required.

description: The description for the tool, which can be used by the model to understand when and how to call the tool. If not provided, the method name will be used as the tool description. However, it’s strongly recommended to provide a detailed description because that’s paramount for the model to understand the tool’s purpose and how to use it. Failing in providing a good description can lead to the model not using the tool when it should or using it incorrectly.

inputType: The type of the function input. Required.

inputSchema: The JSON schema for the input parameters of the tool. If not provided, the schema will be generated automatically based on the inputType. You can use the @ToolParam annotation to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required. See JSON Schema for more details.

toolMetadata: The ToolMetadata instance that defines additional settings such as whether the result should be returned directly to the client, and the result converter to use. You can build it using the ToolMetadata.Builder class.

toolCallResultConverter: The ToolCallResultConverter instance to use for converting the result of a tool call to a String object to send back to the AI model. If not provided, the default converter will be used (DefaultToolCallResultConverter).

The ToolMetadata.Builder allows you to build a ToolMetadata instance and define additional settings for the tool:

returnDirect: Whether the tool result should be returned directly to the client or passed back to the model. See Return Direct for more details.

The function inputs and outputs can be either Void or POJOs. The input and output POJOs must be serializable, as the result will be serialized and sent back to the model. The function as well as the input and output types must be public.

When using the programmatic specification approach, you can pass the FunctionToolCallback instance to the toolCallbacks() method of ChatClient. The tool will only be available for the specific chat request it’s added to.

When using the programmatic specification approach, you can add default tools to a ChatClient.Builder by passing the FunctionToolCallback instance to the defaultToolCallbacks() method. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

When using the programmatic specification approach, you can pass the FunctionToolCallback instance to the toolCallbacks() method of ToolCallingChatOptions. The tool will only be available for the specific chat request it’s added to.

When using the programmatic specification approach, you can add default tools to a ChatModel at construction time by passing the FunctionToolCallback instance to the toolCallbacks() method of the ToolCallingChatOptions instance used to create the ChatModel. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

Instead of specifying tools programmatically, you can define tools as Spring beans and let Spring AI resolve them dynamically at runtime using the ToolCallbackResolver interface (via the SpringBeanToolCallbackResolver implementation). This option gives you the possibility to use any Function, Supplier, Consumer, or BiFunction bean as a tool. The bean name will be used as the tool name, and the @Description annotation from Spring Framework can be used to provide a description for the tool, used by the model to understand when and how to call the tool. If you don’t provide a description, the method name will be used as the tool description. However, it’s strongly recommended to provide a detailed description because that’s paramount for the model to understand the tool’s purpose and how to use it. Failing in providing a good description can lead to the model not using the tool when it should or using it incorrectly.

The JSON schema for the input parameters of the tool will be generated automatically. You can use the @ToolParam annotation to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required. See JSON Schema for more details.

This tool specification approach has the drawback of not guaranteeing type safety, as the tool resolution is done at runtime. To mitigate this, you can specify the tool name explicitly using the @Bean annotation and storing the value in a constant, so that you can use it in a chat request instead of hard-coding the tool name.

When using the dynamic specification approach, you can pass the tool name (i.e. the function bean name) to the toolNames() method of ChatClient. The tool will only be available for the specific chat request it’s added to.

When using the dynamic specification approach, you can add default tools to a ChatClient.Builder by passing the tool name to the defaultToolNames() method. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

When using the dynamic specification approach, you can pass the tool name to the toolNames() method of the ToolCallingChatOptions you use to call the ChatModel. The tool will only be available for the specific chat request it’s added to.

When using the dynamic specification approach, you can add default tools to ChatModel at construction time by passing the tool name to the toolNames() method of the ToolCallingChatOptions instance used to create the ChatModel. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

The following types are not currently supported as input or output types for functions used as tools:

Collection types (e.g. List, Map, Array, Set)

Asynchronous types (e.g. CompletableFuture, Future)

Reactive types (e.g. Flow, Mono, Flux).

Primitive types and collections are supported using the method-based tool specification approach. See Methods as Tools for more details.

In Spring AI, tools are modeled via the ToolCallback interface. In the previous sections, we’ve seen how to define tools from methods and functions using the built-in support provided by Spring AI (see Methods as Tools and Functions as Tools). This section will dive deeper into the tool specification and how to customize and extend it to support more use cases.

The ToolCallback interface provides a way to define a tool that can be called by the AI model, including both definition and execution logic. It’s the main interface to implement when you want to define a tool from scratch. For example, you can define a ToolCallback from an MCP Client (using the Model Context Protocol) or a ChatClient (to build a modular agentic application).

The interface provides the following methods:

Spring AI provides built-in implementations for tool methods (MethodToolCallback) and tool functions (FunctionToolCallback).

The ToolDefinition interface provides the required information for the AI model to know about the availability of the tool, including the tool name, description, and input schema. Each ToolCallback implementation must provide a ToolDefinition instance to define the tool.

The interface provides the following methods:

The ToolDefinition.Builder lets you build a ToolDefinition instance using the default implementation (DefaultToolDefinition).

When building tools from a method, the ToolDefinition is automatically generated for you. In case you prefer to generate the ToolDefinition yourself, you can use this convenient builder.

The ToolDefinition generated from a method includes the method name as the tool name, the method name as the tool description, and the JSON schema of the method input parameters. If the method is annotated with @Tool, the tool name and description will be taken from the annotation, if set.

If you’d rather provide some or all of the attributes explicitly, you can use the ToolDefinition.Builder to build a custom ToolDefinition instance.

When building tools from a function, the ToolDefinition is automatically generated for you. When you use the FunctionToolCallback.Builder to build a FunctionToolCallback instance, you can provide the tool name, description, and input schema that will be used to generate the ToolDefinition. See Functions as Tools for more details.

When providing a tool to the AI model, the model needs to know the schema of the input type for calling the tool. The schema is used to understand how to call the tool and prepare the tool request. Spring AI provides built-in support for generating the JSON Schema of the input type for a tool via the JsonSchemaGenerator class. The schema is provided as part of the ToolDefinition.

The JsonSchemaGenerator class is used under the hood to generate the JSON schema for the input parameters of a method or a function, using any of the strategies described in Methods as Tools and Functions as Tools. The JSON schema generation logic supports a series of annotations that you can use on the input parameters for methods and functions to customize the resulting schema.

This section describes two main options you can customize when generating the JSON schema for the input parameters of a tool: description and required status.

Besides providing a description for the tool itself, you can also provide a description for the input parameters of a tool. The description can be used to provide key information about the input parameters, such as what format the parameter should be in, what values are allowed, and so on. This is useful to help the model understand the input schema and how to use it. Spring AI provides built-in support for generating the description for an input parameter using one of the following annotations:

@ToolParam(description = "…​") from Spring AI

@JsonClassDescription(description = "…​") from Jackson

@JsonPropertyDescription(description = "…​") from Jackson

@Schema(description = "…​") from Swagger.

This approach works for both methods and functions, and you can use it recursively for nested types.

By default, each input parameter is considered required, which forces the AI model to provide a value for it when calling the tool. However, you can make an input parameter optional by using one of the following annotations, in this order of precedence:

@ToolParam(required = false) from Spring AI

@JsonProperty(required = false) from Jackson

@Schema(required = false) from Swagger

@Nullable from Spring Framework.

This approach works for both methods and functions, and you can use it recursively for nested types.

The result of a tool call is serialized using a ToolCallResultConverter and then sent back to the AI model. The ToolCallResultConverter interface provides a way to convert the result of a tool call to a String object.

The interface provides the following method:

The result must be a serializable type. By default, the result is serialized to JSON using Jackson (DefaultToolCallResultConverter), but you can customize the serialization process by providing your own ToolCallResultConverter implementation.

Spring AI relies on the ToolCallResultConverter in both method and function tools.

When building tools from a method with the declarative approach, you can provide a custom ToolCallResultConverter to use for the tool by setting the resultConverter() attribute of the @Tool annotation.

If using the programmatic approach, you can provide a custom ToolCallResultConverter to use for the tool by setting the resultConverter() attribute of the MethodToolCallback.Builder.

See Methods as Tools for more details.

When building tools from a function using the programmatic approach, you can provide a custom ToolCallResultConverter to use for the tool by setting the resultConverter() attribute of the FunctionToolCallback.Builder.

See Functions as Tools for more details.

Spring AI supports passing additional contextual information to tools through the ToolContext API. This feature allows you to provide extra, user-provided data that can be used within the tool execution along with the tool arguments passed by the AI model.

The ToolContext is populated with the data provided by the user when invoking ChatClient.

Similarly, you can define tool context data when invoking the ChatModel directly.

If the toolContext option is set both in the default options and in the runtime options, the resulting ToolContext will be the merge of the two, where the runtime options take precedence over the default options.

By default, the result of a tool call is sent back to the model as a response. Then, the model can use the result to continue the conversation.

There are cases where you’d rather return the result directly to the caller instead of sending it back to the model. For example, if you build an agent that relies on a RAG tool, you might want to return the result directly to the caller instead of sending it back to the model for unnecessary post-processing. Or perhaps you have certain tools that should end the reasoning loop of the agent.

Each ToolCallback implementation can define whether the result of a tool call should be returned directly to the caller or sent back to the model. By default, the result is sent back to the model. But you can change this behavior per tool.

The ToolCallingManager, responsible for managing the tool execution lifecycle, is in charge of handling the returnDirect attribute associated with the tool. If the attribute is set to true, the result of the tool call is returned directly to the caller. Otherwise, the result is sent back to the model.

When we want to make a tool available to the model, we include its definition in the chat request. If we want the result of the tool execution to be returned directly to the caller, we set the returnDirect attribute to true.

When the model decides to call a tool, it sends a response with the tool name and the input parameters modeled after the defined schema.

The application is responsible for using the tool name to identify and execute the tool with the provided input parameters.

The result of the tool call is processed by the application.

The application sends the tool call result directly to the caller, instead of sending it back to the model.

When building tools from a method with the declarative approach, you can mark a tool to return the result directly to the caller by setting the returnDirect attribute of the @Tool annotation to true.

If using the programmatic approach, you can set the returnDirect attribute via the ToolMetadata interface and pass it to the MethodToolCallback.Builder.

See Methods as Tools for more details.

When building tools from a function with the programmatic approach, you can set the returnDirect attribute via the ToolMetadata interface and pass it to the FunctionToolCallback.Builder.

See Functions as Tools for more details.

The tool execution is the process of calling the tool with the provided input arguments and returning the result. The tool execution is handled by the ToolCallingManager interface, which is responsible for managing the tool execution lifecycle.

If you’re using any of the Spring AI Spring Boot Starters, DefaultToolCallingManager is the autoconfigured implementation of the ToolCallingManager interface. You can customize the tool execution behavior by providing your own ToolCallingManager bean.

By default, Spring AI manages the tool execution lifecycle transparently for you from within each ChatModel implementation. But you have the possibility to opt-out of this behavior and control the tool execution yourself. This section describes these two scenarios.

When using the default behavior, Spring AI will automatically intercept any tool call request from the model, call the tool and return the result to the model. All of this is done transparently for you by each ChatModel implementation using a ToolCallingManager.

When we want to make a tool available to the model, we include its definition in the chat request (Prompt) and invoke the ChatModel API which sends the request to the AI model.

When the model decides to call a tool, it sends a response (ChatResponse) with the tool name and the input parameters modeled after the defined schema.

The ChatModel sends the tool call request to the ToolCallingManager API.

The ToolCallingManager is responsible for identifying the tool to call and executing it with the provided input parameters.

The result of the tool call is returned to the ToolCallingManager.

The ToolCallingManager returns the tool execution result back to the ChatModel.

The ChatModel sends the tool execution result back to the AI model (ToolResponseMessage).

The AI model generates the final response using the tool call result as additional context and sends it back to the caller (ChatResponse) via the ChatClient.

The logic determining whether a tool call is eligible for execution is handled by the ToolExecutionEligibilityPredicate interface. By default, the tool execution eligibility is determined by checking if the internalToolExecutionEnabled attribute of ToolCallingChatOptions is set to true (the default value), and if the ChatResponse contains any tool calls.

You can provide your custom implementation of ToolExecutionEligibilityPredicate when creating the ChatModel bean.

There are cases where you’d rather control the tool execution lifecycle yourself. You can do so by setting the internalToolExecutionEnabled attribute of ToolCallingChatOptions to false.

When you invoke a ChatModel with this option, the tool execution will be delegated to the caller, giving you full control over the tool execution lifecycle. It’s your responsibility checking for tool calls in the ChatResponse and executing them using the ToolCallingManager.

The following example demonstrates a minimal implementation of the user-controlled tool execution approach:

The next examples shows a minimal implementation of the user-controlled tool execution approach combined with the usage of the ChatMemory API:

When a tool call fails, the exception is propagated as a ToolExecutionException which can be caught to handle the error. A ToolExecutionExceptionProcessor can be used to handle a ToolExecutionException with two outcomes: either producing an error message to be sent back to the AI model or throwing an exception to be handled by the caller.

If you’re using any of the Spring AI Spring Boot Starters, DefaultToolExecutionExceptionProcessor is the autoconfigured implementation of the ToolExecutionExceptionProcessor interface. By default, the error message of RuntimeException is sent back to the model, while checked exceptions and Errors (e.g., IOException, OutOfMemoryError) are always thrown. The DefaultToolExecutionExceptionProcessor constructor lets you set the alwaysThrow attribute to true or false. If true, an exception will be thrown instead of sending an error message back to the model.

You can use the `spring.ai.tools.throw-exception-on-error property to control the behavior of the DefaultToolExecutionExceptionProcessor bean:

spring.ai.tools.throw-exception-on-error

If true, tool calling errors are thrown as exceptions for the caller to handle. If false, errors are converted to messages and sent back to the AI model, allowing it to process and respond to the error.

The ToolExecutionExceptionProcessor is used internally by the default ToolCallingManager (DefaultToolCallingManager) to handle exceptions during tool execution. See Tool Execution for more details about the tool execution lifecycle.

The main approach for passing tools to a model is by providing the ToolCallback(s) when invoking the ChatClient or the ChatModel, using one of the strategies described in Methods as Tools and Functions as Tools.

However, Spring AI also supports resolving tools dynamically at runtime using the ToolCallbackResolver interface.

When using this approach:

On the client-side, you provide the tool names to the ChatClient or the ChatModel instead of the ToolCallback(s).

On the server-side, a ToolCallbackResolver implementation is responsible for resolving the tool names to the corresponding ToolCallback instances.

By default, Spring AI relies on a DelegatingToolCallbackResolver that delegates the tool resolution to a list of ToolCallbackResolver instances:

The SpringBeanToolCallbackResolver resolves tools from Spring beans of type Function, Supplier, Consumer, or BiFunction. See Dynamic Specification: @Bean for more details.

The StaticToolCallbackResolver resolves tools from a static list of ToolCallback instances. When using the Spring Boot Autoconfiguration, this resolver is automatically configured with all the beans of type ToolCallback defined in the application context.

If you rely on the Spring Boot Autoconfiguration, you can customize the resolution logic by providing a custom ToolCallbackResolver bean.

The ToolCallbackResolver is used internally by the ToolCallingManager to resolve tools dynamically at runtime, supporting both Framework-Controlled Tool Execution and User-Controlled Tool Execution.

Tool calling includes observability support with spring.ai.tool observations that measure completion time and propagate tracing information. See Tool Calling Observability.

Optionally, Spring AI can export tool call arguments and results as span attributes, disabled by default for sensitivity reasons. Details: Tool Call Arguments and Result Data.

All the main operations of the tool calling features are logged at the DEBUG level. You can enable the logging by setting the log level to DEBUG for the org.springframework.ai package.

**Examples:**

Example 1 (java):
```java
import java.time.LocalDateTime;
import org.springframework.ai.tool.annotation.Tool;
import org.springframework.context.i18n.LocaleContextHolder;

class DateTimeTools {

    @Tool(description = "Get the current date and time in the user's timezone")
    String getCurrentDateTime() {
        return LocalDateTime.now().atZone(LocaleContextHolder.getTimeZone().toZoneId()).toString();
    }

}
```

Example 2 (java):
```java
ChatModel chatModel = ...

String response = ChatClient.create(chatModel)
        .prompt("What day is tomorrow?")
        .tools(new DateTimeTools())
        .call()
        .content();

System.out.println(response);
```

Example 3 (unknown):
```unknown
Tomorrow is 2015-10-21.
```

Example 4 (unknown):
```unknown
I am an AI and do not have access to real-time information. Please provide the current date so I can accurately determine what day tomorrow will be.
```

---

## Tool Calling :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/tools.html

**Contents:**
- Tool Calling
- Quick Start
  - Information Retrieval
  - Taking Actions
- Overview
- Methods as Tools
  - Declarative Specification: @Tool
    - Adding Tools to ChatClient
    - Adding Default Tools to ChatClient
    - Adding Tools to ChatModel

For the latest snapshot version, please use Spring AI 1.1.2!

Tool calling (also known as function calling) is a common pattern in AI applications allowing a model to interact with a set of APIs, or tools, augmenting its capabilities.

Tools are mainly used for:

Information Retrieval. Tools in this category can be used to retrieve information from external sources, such as a database, a web service, a file system, or a web search engine. The goal is to augment the knowledge of the model, allowing it to answer questions that it would not be able to answer otherwise. As such, they can be used in Retrieval Augmented Generation (RAG) scenarios. For example, a tool can be used to retrieve the current weather for a given location, to retrieve the latest news articles, or to query a database for a specific record.

Taking Action. Tools in this category can be used to take action in a software system, such as sending an email, creating a new record in a database, submitting a form, or triggering a workflow. The goal is to automate tasks that would otherwise require human intervention or explicit programming. For example, a tool can be used to book a flight for a customer interacting with a chatbot, to fill out a form on a web page, or to implement a Java class based on an automated test (TDD) in a code generation scenario.

Even though we typically refer to tool calling as a model capability, it is actually up to the client application to provide the tool calling logic. The model can only request a tool call and provide the input arguments, whereas the application is responsible for executing the tool call from the input arguments and returning the result. The model never gets access to any of the APIs provided as tools, which is a critical security consideration.

Spring AI provides convenient APIs to define tools, resolve tool call requests from a model, and execute the tool calls. The following sections provide an overview of the tool calling capabilities in Spring AI.

Let’s see how to start using tool calling in Spring AI. We’ll implement two simple tools: one for information retrieval and one for taking action. The information retrieval tool will be used to get the current date and time in the user’s time zone. The action tool will be used to set an alarm for a specified time.

AI models don’t have access to real-time information. Any question that assumes awareness of information such as the current date or weather forecast cannot be answered by the model. However, we can provide a tool that can retrieve this information, and let the model call this tool when access to real-time information is needed.

Let’s implement a tool to get the current date and time in the user’s time zone in a DateTimeTools class. The tool will take no argument. The LocaleContextHolder from Spring Framework can provide the user’s time zone. The tool will be defined as a method annotated with @Tool. To help the model understand if and when to call this tool, we’ll provide a detailed description of what the tools does.

Next, let’s make the tool available to the model. In this example, we’ll use the ChatClient to interact with the model. We’ll provide the tool to the model by passing an instance of DateTimeTools via the tools() method. When the model needs to know the current date and time, it will request the tool to be called. Internally, the ChatClient will call the tool and return the result to the model, which will then use the tool call result to generate the final response to the original question.

The output will be something like:

You can retry asking the same question again. This time, don’t provide the tool to the model. The output will be something like:

Without the tool, the model doesn’t know how to answer the question because it doesn’t have the ability to determine the current date and time.

AI models can be used to generate plans for accomplishing certain goals. For example, a model can generate a plan for booking a trip to Denmark. However, the model doesn’t have the ability to execute the plan. That’s where tools come in: they can be used to execute the plan that a model generates.

In the previous example, we used a tool to determine the current date and time. In this example, we’ll define a second tool for setting an alarm at a specific time. The goal is to set an alarm for 10 minutes from now, so we need to provide both tools to the model to accomplish this task.

We’ll add the new tool to the same DateTimeTools class as before. The new tool will take a single parameter, which is the time in ISO-8601 format. The tool will then print a message to the console indicating that the alarm has been set for the given time. Like before, the tool is defined as a method annotated with @Tool, which we also use to provide a detailed description to help the model understand when and how to use the tool.

Next, let’s make both tools available to the model. We’ll use the ChatClient to interact with the model. We’ll provide the tools to the model by passing an instance of DateTimeTools via the tools() method. When we ask to set up an alarm 10 minutes from now, the model will first need to know the current date and time. Then, it will use the current date and time to calculate the alarm time. Finally, it will use the alarm tool to set up the alarm. Internally, the ChatClient will handle any tool call request from the model and send back to it any tool call execution result, so that the model can generate the final response.

In the application logs, you can check the alarm has been set at the correct time.

Spring AI supports tool calling through a set of flexible abstractions that allow you to define, resolve, and execute tools in a consistent way. This section provides an overview of the main concepts and components of tool calling in Spring AI.

When we want to make a tool available to the model, we include its definition in the chat request. Each tool definition comprises of a name, a description, and the schema of the input parameters.

When the model decides to call a tool, it sends a response with the tool name and the input parameters modeled after the defined schema.

The application is responsible for using the tool name to identify and execute the tool with the provided input parameters.

The result of the tool call is processed by the application.

The application sends the tool call result back to the model.

The model generates the final response using the tool call result as additional context.

Tools are the building blocks of tool calling and they are modeled by the ToolCallback interface. Spring AI provides built-in support for specifying ToolCallback(s) from methods and functions, but you can always define your own ToolCallback implementations to support more use cases.

ChatModel implementations transparently dispatch tool call requests to the corresponding ToolCallback implementations and will send the tool call results back to the model, which will ultimately generate the final response. They do so using the ToolCallingManager interface, which is responsible for managing the tool execution lifecycle.

Both ChatClient and ChatModel accept a list of ToolCallback objects to make the tools available to the model and the ToolCallingManager that will eventually execute them.

Besides passing the ToolCallback objects directly, you can also pass a list of tool names, that will be resolved dynamically using the ToolCallbackResolver interface.

The following sections will go into more details about all these concepts and APIs, including how to customize and extend them to support more use cases.

Spring AI provides built-in support for specifying tools (i.e. ToolCallback(s)) from methods in two ways:

declaratively, using the @Tool annotation

programmatically, using the low-level MethodToolCallback implementation.

You can turn a method into a tool by annotating it with @Tool.

The @Tool annotation allows you to provide key information about the tool:

name: The name of the tool. If not provided, the method name will be used. AI models use this name to identify the tool when calling it. Therefore, it’s not allowed to have two tools with the same name in the same class. The name must be unique across all the tools available to the model for a specific chat request.

description: The description for the tool, which can be used by the model to understand when and how to call the tool. If not provided, the method name will be used as the tool description. However, it’s strongly recommended to provide a detailed description because that’s paramount for the model to understand the tool’s purpose and how to use it. Failing in providing a good description can lead to the model not using the tool when it should or using it incorrectly.

returnDirect: Whether the tool result should be returned directly to the client or passed back to the model. See Return Direct for more details.

resultConverter: The ToolCallResultConverter implementation to use for converting the result of a tool call to a String object to send back to the AI model. See Result Conversion for more details.

The method can be either static or instance, and it can have any visibility (public, protected, package-private, or private). The class that contains the method can be either a top-level class or a nested class, and it can also have any visibility (as long as it’s accessible where you’re planning to instantiate it).

You can define any number of arguments for the method (including no argument) with most types (primitives, POJOs, enums, lists, arrays, maps, and so on). Similarly, the method can return most types, including void. If the method returns a value, the return type must be a serializable type, as the result will be serialized and sent back to the model.

Spring AI will generate the JSON schema for the input parameters of the @Tool-annotated method automatically. The schema is used by the model to understand how to call the tool and prepare the tool request. The @ToolParam annotation can be used to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required.

The @ToolParam annotation allows you to provide key information about a tool parameter:

description: The description for the parameter, which can be used by the model to understand better how to use it. For example, what format the parameter should be in, what values are allowed, and so on.

required: Whether the parameter is required or optional. By default, all parameters are considered required.

If a parameter is annotated as @Nullable, it will be considered optional unless explicitly marked as required using the @ToolParam annotation.

Besides the @ToolParam annotation, you can also use the @Schema annotation from Swagger or @JsonProperty from Jackson. See JSON Schema for more details.

When using the declarative specification approach, you can pass the tool class instance to the tools() method when invoking a ChatClient. Such tools will only be available for the specific chat request they are added to.

Under the hood, the ChatClient will generate a ToolCallback from each @Tool-annotated method in the tool class instance and pass them to the model. In case you prefer to generate the ToolCallback(s) yourself, you can use the ToolCallbacks utility class.

When using the declarative specification approach, you can add default tools to a ChatClient.Builder by passing the tool class instance to the defaultTools() method. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

When using the declarative specification approach, you can pass the tool class instance to the toolCallbacks() method of the ToolCallingChatOptions you use to call a ChatModel. Such tools will only be available for the specific chat request they are added to.

When using the declarative specification approach, you can add default tools to ChatModel at construction time by passing the tool class instance to the toolCallbacks() method of the ToolCallingChatOptions instance used to create the ChatModel. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

You can turn a method into a tool by building a MethodToolCallback programmatically.

The MethodToolCallback.Builder allows you to build a MethodToolCallback instance and provide key information about the tool:

toolDefinition: The ToolDefinition instance that defines the tool name, description, and input schema. You can build it using the ToolDefinition.Builder class. Required.

toolMetadata: The ToolMetadata instance that defines additional settings such as whether the result should be returned directly to the client, and the result converter to use. You can build it using the ToolMetadata.Builder class.

toolMethod: The Method instance that represents the tool method. Required.

toolObject: The object instance that contains the tool method. If the method is static, you can omit this parameter.

toolCallResultConverter: The ToolCallResultConverter instance to use for converting the result of a tool call to a String object to send back to the AI model. If not provided, the default converter will be used (DefaultToolCallResultConverter).

The ToolDefinition.Builder allows you to build a ToolDefinition instance and define the tool name, description, and input schema:

name: The name of the tool. If not provided, the method name will be used. AI models use this name to identify the tool when calling it. Therefore, it’s not allowed to have two tools with the same name in the same class. The name must be unique across all the tools available to the model for a specific chat request.

description: The description for the tool, which can be used by the model to understand when and how to call the tool. If not provided, the method name will be used as the tool description. However, it’s strongly recommended to provide a detailed description because that’s paramount for the model to understand the tool’s purpose and how to use it. Failing in providing a good description can lead to the model not using the tool when it should or using it incorrectly.

inputSchema: The JSON schema for the input parameters of the tool. If not provided, the schema will be generated automatically based on the method parameters. You can use the @ToolParam annotation to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required. See JSON Schema for more details.

The ToolMetadata.Builder allows you to build a ToolMetadata instance and define additional settings for the tool:

returnDirect: Whether the tool result should be returned directly to the client or passed back to the model. See Return Direct for more details.

The method can be either static or instance, and it can have any visibility (public, protected, package-private, or private). The class that contains the method can be either a top-level class or a nested class, and it can also have any visibility (as long as it’s accessible where you’re planning to instantiate it).

You can define any number of arguments for the method (including no argument) with most types (primitives, POJOs, enums, lists, arrays, maps, and so on). Similarly, the method can return most types, including void. If the method returns a value, the return type must be a serializable type, as the result will be serialized and sent back to the model.

If the method is static, you can omit the toolObject() method, as it’s not needed.

Spring AI will generate the JSON schema for the input parameters of the method automatically. The schema is used by the model to understand how to call the tool and prepare the tool request. The @ToolParam annotation can be used to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required.

The @ToolParam annotation allows you to provide key information about a tool parameter:

description: The description for the parameter, which can be used by the model to understand better how to use it. For example, what format the parameter should be in, what values are allowed, and so on.

required: Whether the parameter is required or optional. By default, all parameters are considered required.

If a parameter is annotated as @Nullable, it will be considered optional unless explicitly marked as required using the @ToolParam annotation.

Besides the @ToolParam annotation, you can also use the @Schema annotation from Swagger or @JsonProperty from Jackson. See JSON Schema for more details.

When using the programmatic specification approach, you can pass the MethodToolCallback instance to the toolCallbacks() method of ChatClient. The tool will only be available for the specific chat request it’s added to.

When using the programmatic specification approach, you can add default tools to a ChatClient.Builder by passing the MethodToolCallback instance to the defaultToolCallbacks() method. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

When using the programmatic specification approach, you can pass the MethodToolCallback instance to the toolCallbacks() method of the ToolCallingChatOptions you use to call a ChatModel. The tool will only be available for the specific chat request it’s added to.

When using the programmatic specification approach, you can add default tools to a ChatModel at construction time by passing the MethodToolCallback instance to the toolCallbacks() method of the ToolCallingChatOptions instance used to create the ChatModel. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

The following types are not currently supported as parameters or return types for methods used as tools:

Asynchronous types (e.g. CompletableFuture, Future)

Reactive types (e.g. Flow, Mono, Flux)

Functional types (e.g. Function, Supplier, Consumer).

Functional types are supported using the function-based tool specification approach. See Functions as Tools for more details.

Spring AI provides built-in support for specifying tools from functions, either programmatically using the low-level FunctionToolCallback implementation or dynamically as @Bean(s) resolved at runtime.

You can turn a functional type (Function, Supplier, Consumer, or BiFunction) into a tool by building a FunctionToolCallback programmatically.

The FunctionToolCallback.Builder allows you to build a FunctionToolCallback instance and provide key information about the tool:

name: The name of the tool. AI models use this name to identify the tool when calling it. Therefore, it’s not allowed to have two tools with the same name in the same context. The name must be unique across all the tools available to the model for a specific chat request. Required.

toolFunction: The functional object that represents the tool method (Function, Supplier, Consumer, or BiFunction). Required.

description: The description for the tool, which can be used by the model to understand when and how to call the tool. If not provided, the method name will be used as the tool description. However, it’s strongly recommended to provide a detailed description because that’s paramount for the model to understand the tool’s purpose and how to use it. Failing in providing a good description can lead to the model not using the tool when it should or using it incorrectly.

inputType: The type of the function input. Required.

inputSchema: The JSON schema for the input parameters of the tool. If not provided, the schema will be generated automatically based on the inputType. You can use the @ToolParam annotation to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required. See JSON Schema for more details.

toolMetadata: The ToolMetadata instance that defines additional settings such as whether the result should be returned directly to the client, and the result converter to use. You can build it using the ToolMetadata.Builder class.

toolCallResultConverter: The ToolCallResultConverter instance to use for converting the result of a tool call to a String object to send back to the AI model. If not provided, the default converter will be used (DefaultToolCallResultConverter).

The ToolMetadata.Builder allows you to build a ToolMetadata instance and define additional settings for the tool:

returnDirect: Whether the tool result should be returned directly to the client or passed back to the model. See Return Direct for more details.

The function inputs and outputs can be either Void or POJOs. The input and output POJOs must be serializable, as the result will be serialized and sent back to the model. The function as well as the input and output types must be public.

When using the programmatic specification approach, you can pass the FunctionToolCallback instance to the toolCallbacks() method of ChatClient. The tool will only be available for the specific chat request it’s added to.

When using the programmatic specification approach, you can add default tools to a ChatClient.Builder by passing the FunctionToolCallback instance to the defaultToolCallbacks() method. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

When using the programmatic specification approach, you can pass the FunctionToolCallback instance to the toolCallbacks() method of ToolCallingChatOptions. The tool will only be available for the specific chat request it’s added to.

When using the programmatic specification approach, you can add default tools to a ChatModel at construction time by passing the FunctionToolCallback instance to the toolCallbacks() method of the ToolCallingChatOptions instance used to create the ChatModel. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

Instead of specifying tools programmatically, you can define tools as Spring beans and let Spring AI resolve them dynamically at runtime using the ToolCallbackResolver interface (via the SpringBeanToolCallbackResolver implementation). This option gives you the possibility to use any Function, Supplier, Consumer, or BiFunction bean as a tool. The bean name will be used as the tool name, and the @Description annotation from Spring Framework can be used to provide a description for the tool, used by the model to understand when and how to call the tool. If you don’t provide a description, the method name will be used as the tool description. However, it’s strongly recommended to provide a detailed description because that’s paramount for the model to understand the tool’s purpose and how to use it. Failing in providing a good description can lead to the model not using the tool when it should or using it incorrectly.

The JSON schema for the input parameters of the tool will be generated automatically. You can use the @ToolParam annotation to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required. See JSON Schema for more details.

This tool specification approach has the drawback of not guaranteeing type safety, as the tool resolution is done at runtime. To mitigate this, you can specify the tool name explicitly using the @Bean annotation and storing the value in a constant, so that you can use it in a chat request instead of hard-coding the tool name.

When using the dynamic specification approach, you can pass the tool name (i.e. the function bean name) to the toolNames() method of ChatClient. The tool will only be available for the specific chat request it’s added to.

When using the dynamic specification approach, you can add default tools to a ChatClient.Builder by passing the tool name to the defaultToolNames() method. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

When using the dynamic specification approach, you can pass the tool name to the toolNames() method of the ToolCallingChatOptions you use to call the ChatModel. The tool will only be available for the specific chat request it’s added to.

When using the dynamic specification approach, you can add default tools to ChatModel at construction time by passing the tool name to the toolNames() method of the ToolCallingChatOptions instance used to create the ChatModel. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

The following types are not currently supported as input or output types for functions used as tools:

Collection types (e.g. List, Map, Array, Set)

Asynchronous types (e.g. CompletableFuture, Future)

Reactive types (e.g. Flow, Mono, Flux).

Primitive types and collections are supported using the method-based tool specification approach. See Methods as Tools for more details.

In Spring AI, tools are modeled via the ToolCallback interface. In the previous sections, we’ve seen how to define tools from methods and functions using the built-in support provided by Spring AI (see Methods as Tools and Functions as Tools). This section will dive deeper into the tool specification and how to customize and extend it to support more use cases.

The ToolCallback interface provides a way to define a tool that can be called by the AI model, including both definition and execution logic. It’s the main interface to implement when you want to define a tool from scratch. For example, you can define a ToolCallback from an MCP Client (using the Model Context Protocol) or a ChatClient (to build a modular agentic application).

The interface provides the following methods:

Spring AI provides built-in implementations for tool methods (MethodToolCallback) and tool functions (FunctionToolCallback).

The ToolDefinition interface provides the required information for the AI model to know about the availability of the tool, including the tool name, description, and input schema. Each ToolCallback implementation must provide a ToolDefinition instance to define the tool.

The interface provides the following methods:

The ToolDefinition.Builder lets you build a ToolDefinition instance using the default implementation (DefaultToolDefinition).

When building tools from a method, the ToolDefinition is automatically generated for you. In case you prefer to generate the ToolDefinition yourself, you can use this convenient builder.

The ToolDefinition generated from a method includes the method name as the tool name, the method name as the tool description, and the JSON schema of the method input parameters. If the method is annotated with @Tool, the tool name and description will be taken from the annotation, if set.

If you’d rather provide some or all of the attributes explicitly, you can use the ToolDefinition.Builder to build a custom ToolDefinition instance.

When building tools from a function, the ToolDefinition is automatically generated for you. When you use the FunctionToolCallback.Builder to build a FunctionToolCallback instance, you can provide the tool name, description, and input schema that will be used to generate the ToolDefinition. See Functions as Tools for more details.

When providing a tool to the AI model, the model needs to know the schema of the input type for calling the tool. The schema is used to understand how to call the tool and prepare the tool request. Spring AI provides built-in support for generating the JSON Schema of the input type for a tool via the JsonSchemaGenerator class. The schema is provided as part of the ToolDefinition.

The JsonSchemaGenerator class is used under the hood to generate the JSON schema for the input parameters of a method or a function, using any of the strategies described in Methods as Tools and Functions as Tools. The JSON schema generation logic supports a series of annotations that you can use on the input parameters for methods and functions to customize the resulting schema.

This section describes two main options you can customize when generating the JSON schema for the input parameters of a tool: description and required status.

Besides providing a description for the tool itself, you can also provide a description for the input parameters of a tool. The description can be used to provide key information about the input parameters, such as what format the parameter should be in, what values are allowed, and so on. This is useful to help the model understand the input schema and how to use it. Spring AI provides built-in support for generating the description for an input parameter using one of the following annotations:

@ToolParam(description = "…​") from Spring AI

@JsonClassDescription(description = "…​") from Jackson

@JsonPropertyDescription(description = "…​") from Jackson

@Schema(description = "…​") from Swagger.

This approach works for both methods and functions, and you can use it recursively for nested types.

By default, each input parameter is considered required, which forces the AI model to provide a value for it when calling the tool. However, you can make an input parameter optional by using one of the following annotations, in this order of precedence:

@ToolParam(required = false) from Spring AI

@JsonProperty(required = false) from Jackson

@Schema(required = false) from Swagger

@Nullable from Spring Framework.

This approach works for both methods and functions, and you can use it recursively for nested types.

The result of a tool call is serialized using a ToolCallResultConverter and then sent back to the AI model. The ToolCallResultConverter interface provides a way to convert the result of a tool call to a String object.

The interface provides the following method:

The result must be a serializable type. By default, the result is serialized to JSON using Jackson (DefaultToolCallResultConverter), but you can customize the serialization process by providing your own ToolCallResultConverter implementation.

Spring AI relies on the ToolCallResultConverter in both method and function tools.

When building tools from a method with the declarative approach, you can provide a custom ToolCallResultConverter to use for the tool by setting the resultConverter() attribute of the @Tool annotation.

If using the programmatic approach, you can provide a custom ToolCallResultConverter to use for the tool by setting the resultConverter() attribute of the MethodToolCallback.Builder.

See Methods as Tools for more details.

When building tools from a function using the programmatic approach, you can provide a custom ToolCallResultConverter to use for the tool by setting the resultConverter() attribute of the FunctionToolCallback.Builder.

See Functions as Tools for more details.

Spring AI supports passing additional contextual information to tools through the ToolContext API. This feature allows you to provide extra, user-provided data that can be used within the tool execution along with the tool arguments passed by the AI model.

The ToolContext is populated with the data provided by the user when invoking ChatClient.

Similarly, you can define tool context data when invoking the ChatModel directly.

If the toolContext option is set both in the default options and in the runtime options, the resulting ToolContext will be the merge of the two, where the runtime options take precedence over the default options.

By default, the result of a tool call is sent back to the model as a response. Then, the model can use the result to continue the conversation.

There are cases where you’d rather return the result directly to the caller instead of sending it back to the model. For example, if you build an agent that relies on a RAG tool, you might want to return the result directly to the caller instead of sending it back to the model for unnecessary post-processing. Or perhaps you have certain tools that should end the reasoning loop of the agent.

Each ToolCallback implementation can define whether the result of a tool call should be returned directly to the caller or sent back to the model. By default, the result is sent back to the model. But you can change this behavior per tool.

The ToolCallingManager, responsible for managing the tool execution lifecycle, is in charge of handling the returnDirect attribute associated with the tool. If the attribute is set to true, the result of the tool call is returned directly to the caller. Otherwise, the result is sent back to the model.

When we want to make a tool available to the model, we include its definition in the chat request. If we want the result of the tool execution to be returned directly to the caller, we set the returnDirect attribute to true.

When the model decides to call a tool, it sends a response with the tool name and the input parameters modeled after the defined schema.

The application is responsible for using the tool name to identify and execute the tool with the provided input parameters.

The result of the tool call is processed by the application.

The application sends the tool call result directly to the caller, instead of sending it back to the model.

When building tools from a method with the declarative approach, you can mark a tool to return the result directly to the caller by setting the returnDirect attribute of the @Tool annotation to true.

If using the programmatic approach, you can set the returnDirect attribute via the ToolMetadata interface and pass it to the MethodToolCallback.Builder.

See Methods as Tools for more details.

When building tools from a function with the programmatic approach, you can set the returnDirect attribute via the ToolMetadata interface and pass it to the FunctionToolCallback.Builder.

See Functions as Tools for more details.

The tool execution is the process of calling the tool with the provided input arguments and returning the result. The tool execution is handled by the ToolCallingManager interface, which is responsible for managing the tool execution lifecycle.

If you’re using any of the Spring AI Spring Boot Starters, DefaultToolCallingManager is the autoconfigured implementation of the ToolCallingManager interface. You can customize the tool execution behavior by providing your own ToolCallingManager bean.

By default, Spring AI manages the tool execution lifecycle transparently for you from within each ChatModel implementation. But you have the possibility to opt-out of this behavior and control the tool execution yourself. This section describes these two scenarios.

When using the default behavior, Spring AI will automatically intercept any tool call request from the model, call the tool and return the result to the model. All of this is done transparently for you by each ChatModel implementation using a ToolCallingManager.

When we want to make a tool available to the model, we include its definition in the chat request (Prompt) and invoke the ChatModel API which sends the request to the AI model.

When the model decides to call a tool, it sends a response (ChatResponse) with the tool name and the input parameters modeled after the defined schema.

The ChatModel sends the tool call request to the ToolCallingManager API.

The ToolCallingManager is responsible for identifying the tool to call and executing it with the provided input parameters.

The result of the tool call is returned to the ToolCallingManager.

The ToolCallingManager returns the tool execution result back to the ChatModel.

The ChatModel sends the tool execution result back to the AI model (ToolResponseMessage).

The AI model generates the final response using the tool call result as additional context and sends it back to the caller (ChatResponse) via the ChatClient.

The logic determining whether a tool call is eligible for execution is handled by the ToolExecutionEligibilityPredicate interface. By default, the tool execution eligibility is determined by checking if the internalToolExecutionEnabled attribute of ToolCallingChatOptions is set to true (the default value), and if the ChatResponse contains any tool calls.

You can provide your custom implementation of ToolExecutionEligibilityPredicate when creating the ChatModel bean.

There are cases where you’d rather control the tool execution lifecycle yourself. You can do so by setting the internalToolExecutionEnabled attribute of ToolCallingChatOptions to false.

When you invoke a ChatModel with this option, the tool execution will be delegated to the caller, giving you full control over the tool execution lifecycle. It’s your responsibility checking for tool calls in the ChatResponse and executing them using the ToolCallingManager.

The following example demonstrates a minimal implementation of the user-controlled tool execution approach:

The next examples shows a minimal implementation of the user-controlled tool execution approach combined with the usage of the ChatMemory API:

When a tool call fails, the exception is propagated as a ToolExecutionException which can be caught to handle the error. A ToolExecutionExceptionProcessor can be used to handle a ToolExecutionException with two outcomes: either producing an error message to be sent back to the AI model or throwing an exception to be handled by the caller.

If you’re using any of the Spring AI Spring Boot Starters, DefaultToolExecutionExceptionProcessor is the autoconfigured implementation of the ToolExecutionExceptionProcessor interface. By default, the error message of RuntimeException is sent back to the model, while checked exceptions and Errors (e.g., IOException, OutOfMemoryError) are always thrown. The DefaultToolExecutionExceptionProcessor constructor lets you set the alwaysThrow attribute to true or false. If true, an exception will be thrown instead of sending an error message back to the model.

You can use the `spring.ai.tools.throw-exception-on-error property to control the behavior of the DefaultToolExecutionExceptionProcessor bean:

spring.ai.tools.throw-exception-on-error

If true, tool calling errors are thrown as exceptions for the caller to handle. If false, errors are converted to messages and sent back to the AI model, allowing it to process and respond to the error.

The ToolExecutionExceptionProcessor is used internally by the default ToolCallingManager (DefaultToolCallingManager) to handle exceptions during tool execution. See Tool Execution for more details about the tool execution lifecycle.

The main approach for passing tools to a model is by providing the ToolCallback(s) when invoking the ChatClient or the ChatModel, using one of the strategies described in Methods as Tools and Functions as Tools.

However, Spring AI also supports resolving tools dynamically at runtime using the ToolCallbackResolver interface.

When using this approach:

On the client-side, you provide the tool names to the ChatClient or the ChatModel instead of the ToolCallback(s).

On the server-side, a ToolCallbackResolver implementation is responsible for resolving the tool names to the corresponding ToolCallback instances.

By default, Spring AI relies on a DelegatingToolCallbackResolver that delegates the tool resolution to a list of ToolCallbackResolver instances:

The SpringBeanToolCallbackResolver resolves tools from Spring beans of type Function, Supplier, Consumer, or BiFunction. See Dynamic Specification: @Bean for more details.

The StaticToolCallbackResolver resolves tools from a static list of ToolCallback instances. When using the Spring Boot Autoconfiguration, this resolver is automatically configured with all the beans of type ToolCallback defined in the application context.

If you rely on the Spring Boot Autoconfiguration, you can customize the resolution logic by providing a custom ToolCallbackResolver bean.

The ToolCallbackResolver is used internally by the ToolCallingManager to resolve tools dynamically at runtime, supporting both Framework-Controlled Tool Execution and User-Controlled Tool Execution.

Tool calling includes observability support with spring.ai.tool observations that measure completion time and propagate tracing information. See Tool Calling Observability.

Optionally, Spring AI can export tool call arguments and results as span attributes, disabled by default for sensitivity reasons. Details: Tool Call Arguments and Result Data.

All the main operations of the tool calling features are logged at the DEBUG level. You can enable the logging by setting the log level to DEBUG for the org.springframework.ai package.

**Examples:**

Example 1 (java):
```java
import java.time.LocalDateTime;
import org.springframework.ai.tool.annotation.Tool;
import org.springframework.context.i18n.LocaleContextHolder;

class DateTimeTools {

    @Tool(description = "Get the current date and time in the user's timezone")
    String getCurrentDateTime() {
        return LocalDateTime.now().atZone(LocaleContextHolder.getTimeZone().toZoneId()).toString();
    }

}
```

Example 2 (java):
```java
ChatModel chatModel = ...

String response = ChatClient.create(chatModel)
        .prompt("What day is tomorrow?")
        .tools(new DateTimeTools())
        .call()
        .content();

System.out.println(response);
```

Example 3 (unknown):
```unknown
Tomorrow is 2015-10-21.
```

Example 4 (unknown):
```unknown
I am an AI and do not have access to real-time information. Please provide the current date so I can accurately determine what day tomorrow will be.
```

---

## Tool Calling :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/tools.html

**Contents:**
- Tool Calling
- Quick Start
  - Information Retrieval
  - Taking Actions
- Overview
- Methods as Tools
  - Declarative Specification: @Tool
    - Adding Tools to ChatClient
    - Adding Default Tools to ChatClient
    - Adding Tools to ChatModel

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Tool calling (also known as function calling) is a common pattern in AI applications allowing a model to interact with a set of APIs, or tools, augmenting its capabilities.

Tools are mainly used for:

Information Retrieval. Tools in this category can be used to retrieve information from external sources, such as a database, a web service, a file system, or a web search engine. The goal is to augment the knowledge of the model, allowing it to answer questions that it would not be able to answer otherwise. As such, they can be used in Retrieval Augmented Generation (RAG) scenarios. For example, a tool can be used to retrieve the current weather for a given location, to retrieve the latest news articles, or to query a database for a specific record.

Taking Action. Tools in this category can be used to take action in a software system, such as sending an email, creating a new record in a database, submitting a form, or triggering a workflow. The goal is to automate tasks that would otherwise require human intervention or explicit programming. For example, a tool can be used to book a flight for a customer interacting with a chatbot, to fill out a form on a web page, or to implement a Java class based on an automated test (TDD) in a code generation scenario.

Even though we typically refer to tool calling as a model capability, it is actually up to the client application to provide the tool calling logic. The model can only request a tool call and provide the input arguments, whereas the application is responsible for executing the tool call from the input arguments and returning the result. The model never gets access to any of the APIs provided as tools, which is a critical security consideration.

Spring AI provides convenient APIs to define tools, resolve tool call requests from a model, and execute the tool calls. The following sections provide an overview of the tool calling capabilities in Spring AI.

Let’s see how to start using tool calling in Spring AI. We’ll implement two simple tools: one for information retrieval and one for taking action. The information retrieval tool will be used to get the current date and time in the user’s time zone. The action tool will be used to set an alarm for a specified time.

AI models don’t have access to real-time information. Any question that assumes awareness of information such as the current date or weather forecast cannot be answered by the model. However, we can provide a tool that can retrieve this information, and let the model call this tool when access to real-time information is needed.

Let’s implement a tool to get the current date and time in the user’s time zone in a DateTimeTools class. The tool will take no argument. The LocaleContextHolder from Spring Framework can provide the user’s time zone. The tool will be defined as a method annotated with @Tool. To help the model understand if and when to call this tool, we’ll provide a detailed description of what the tools does.

Next, let’s make the tool available to the model. In this example, we’ll use the ChatClient to interact with the model. We’ll provide the tool to the model by passing an instance of DateTimeTools via the tools() method. When the model needs to know the current date and time, it will request the tool to be called. Internally, the ChatClient will call the tool and return the result to the model, which will then use the tool call result to generate the final response to the original question.

The output will be something like:

You can retry asking the same question again. This time, don’t provide the tool to the model. The output will be something like:

Without the tool, the model doesn’t know how to answer the question because it doesn’t have the ability to determine the current date and time.

AI models can be used to generate plans for accomplishing certain goals. For example, a model can generate a plan for booking a trip to Denmark. However, the model doesn’t have the ability to execute the plan. That’s where tools come in: they can be used to execute the plan that a model generates.

In the previous example, we used a tool to determine the current date and time. In this example, we’ll define a second tool for setting an alarm at a specific time. The goal is to set an alarm for 10 minutes from now, so we need to provide both tools to the model to accomplish this task.

We’ll add the new tool to the same DateTimeTools class as before. The new tool will take a single parameter, which is the time in ISO-8601 format. The tool will then print a message to the console indicating that the alarm has been set for the given time. Like before, the tool is defined as a method annotated with @Tool, which we also use to provide a detailed description to help the model understand when and how to use the tool.

Next, let’s make both tools available to the model. We’ll use the ChatClient to interact with the model. We’ll provide the tools to the model by passing an instance of DateTimeTools via the tools() method. When we ask to set up an alarm 10 minutes from now, the model will first need to know the current date and time. Then, it will use the current date and time to calculate the alarm time. Finally, it will use the alarm tool to set up the alarm. Internally, the ChatClient will handle any tool call request from the model and send back to it any tool call execution result, so that the model can generate the final response.

In the application logs, you can check the alarm has been set at the correct time.

Spring AI supports tool calling through a set of flexible abstractions that allow you to define, resolve, and execute tools in a consistent way. This section provides an overview of the main concepts and components of tool calling in Spring AI.

When we want to make a tool available to the model, we include its definition in the chat request. Each tool definition comprises of a name, a description, and the schema of the input parameters.

When the model decides to call a tool, it sends a response with the tool name and the input parameters modeled after the defined schema.

The application is responsible for using the tool name to identify and execute the tool with the provided input parameters.

The result of the tool call is processed by the application.

The application sends the tool call result back to the model.

The model generates the final response using the tool call result as additional context.

Tools are the building blocks of tool calling and they are modeled by the ToolCallback interface. Spring AI provides built-in support for specifying ToolCallback(s) from methods and functions, but you can always define your own ToolCallback implementations to support more use cases.

ChatModel implementations transparently dispatch tool call requests to the corresponding ToolCallback implementations and will send the tool call results back to the model, which will ultimately generate the final response. They do so using the ToolCallingManager interface, which is responsible for managing the tool execution lifecycle.

Both ChatClient and ChatModel accept a list of ToolCallback objects to make the tools available to the model and the ToolCallingManager that will eventually execute them.

Besides passing the ToolCallback objects directly, you can also pass a list of tool names, that will be resolved dynamically using the ToolCallbackResolver interface.

The following sections will go into more details about all these concepts and APIs, including how to customize and extend them to support more use cases.

Spring AI provides built-in support for specifying tools (i.e. ToolCallback(s)) from methods in two ways:

declaratively, using the @Tool annotation

programmatically, using the low-level MethodToolCallback implementation.

You can turn a method into a tool by annotating it with @Tool.

The @Tool annotation allows you to provide key information about the tool:

name: The name of the tool. If not provided, the method name will be used. AI models use this name to identify the tool when calling it. Therefore, it’s not allowed to have two tools with the same name in the same class. The name must be unique across all the tools available to the model for a specific chat request.

description: The description for the tool, which can be used by the model to understand when and how to call the tool. If not provided, the method name will be used as the tool description. However, it’s strongly recommended to provide a detailed description because that’s paramount for the model to understand the tool’s purpose and how to use it. Failing in providing a good description can lead to the model not using the tool when it should or using it incorrectly.

returnDirect: Whether the tool result should be returned directly to the client or passed back to the model. See Return Direct for more details.

resultConverter: The ToolCallResultConverter implementation to use for converting the result of a tool call to a String object to send back to the AI model. See Result Conversion for more details.

The method can be either static or instance, and it can have any visibility (public, protected, package-private, or private). The class that contains the method can be either a top-level class or a nested class, and it can also have any visibility (as long as it’s accessible where you’re planning to instantiate it).

You can define any number of arguments for the method (including no argument) with most types (primitives, POJOs, enums, lists, arrays, maps, and so on). Similarly, the method can return most types, including void. If the method returns a value, the return type must be a serializable type, as the result will be serialized and sent back to the model.

Spring AI will generate the JSON schema for the input parameters of the @Tool-annotated method automatically. The schema is used by the model to understand how to call the tool and prepare the tool request. The @ToolParam annotation can be used to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required.

The @ToolParam annotation allows you to provide key information about a tool parameter:

description: The description for the parameter, which can be used by the model to understand better how to use it. For example, what format the parameter should be in, what values are allowed, and so on.

required: Whether the parameter is required or optional. By default, all parameters are considered required.

If a parameter is annotated as @Nullable, it will be considered optional unless explicitly marked as required using the @ToolParam annotation.

Besides the @ToolParam annotation, you can also use the @Schema annotation from Swagger or @JsonProperty from Jackson. See JSON Schema for more details.

When using the declarative specification approach, you can pass the tool class instance to the tools() method when invoking a ChatClient. Such tools will only be available for the specific chat request they are added to.

Under the hood, the ChatClient will generate a ToolCallback from each @Tool-annotated method in the tool class instance and pass them to the model. In case you prefer to generate the ToolCallback(s) yourself, you can use the ToolCallbacks utility class.

When using the declarative specification approach, you can add default tools to a ChatClient.Builder by passing the tool class instance to the defaultTools() method. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

When using the declarative specification approach, you can pass the tool class instance to the toolCallbacks() method of the ToolCallingChatOptions you use to call a ChatModel. Such tools will only be available for the specific chat request they are added to.

When using the declarative specification approach, you can add default tools to ChatModel at construction time by passing the tool class instance to the toolCallbacks() method of the ToolCallingChatOptions instance used to create the ChatModel. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

You can turn a method into a tool by building a MethodToolCallback programmatically.

The MethodToolCallback.Builder allows you to build a MethodToolCallback instance and provide key information about the tool:

toolDefinition: The ToolDefinition instance that defines the tool name, description, and input schema. You can build it using the ToolDefinition.Builder class. Required.

toolMetadata: The ToolMetadata instance that defines additional settings such as whether the result should be returned directly to the client, and the result converter to use. You can build it using the ToolMetadata.Builder class.

toolMethod: The Method instance that represents the tool method. Required.

toolObject: The object instance that contains the tool method. If the method is static, you can omit this parameter.

toolCallResultConverter: The ToolCallResultConverter instance to use for converting the result of a tool call to a String object to send back to the AI model. If not provided, the default converter will be used (DefaultToolCallResultConverter).

The ToolDefinition.Builder allows you to build a ToolDefinition instance and define the tool name, description, and input schema:

name: The name of the tool. If not provided, the method name will be used. AI models use this name to identify the tool when calling it. Therefore, it’s not allowed to have two tools with the same name in the same class. The name must be unique across all the tools available to the model for a specific chat request.

description: The description for the tool, which can be used by the model to understand when and how to call the tool. If not provided, the method name will be used as the tool description. However, it’s strongly recommended to provide a detailed description because that’s paramount for the model to understand the tool’s purpose and how to use it. Failing in providing a good description can lead to the model not using the tool when it should or using it incorrectly.

inputSchema: The JSON schema for the input parameters of the tool. If not provided, the schema will be generated automatically based on the method parameters. You can use the @ToolParam annotation to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required. See JSON Schema for more details.

The ToolMetadata.Builder allows you to build a ToolMetadata instance and define additional settings for the tool:

returnDirect: Whether the tool result should be returned directly to the client or passed back to the model. See Return Direct for more details.

The method can be either static or instance, and it can have any visibility (public, protected, package-private, or private). The class that contains the method can be either a top-level class or a nested class, and it can also have any visibility (as long as it’s accessible where you’re planning to instantiate it).

You can define any number of arguments for the method (including no argument) with most types (primitives, POJOs, enums, lists, arrays, maps, and so on). Similarly, the method can return most types, including void. If the method returns a value, the return type must be a serializable type, as the result will be serialized and sent back to the model.

If the method is static, you can omit the toolObject() method, as it’s not needed.

Spring AI will generate the JSON schema for the input parameters of the method automatically. The schema is used by the model to understand how to call the tool and prepare the tool request. The @ToolParam annotation can be used to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required.

The @ToolParam annotation allows you to provide key information about a tool parameter:

description: The description for the parameter, which can be used by the model to understand better how to use it. For example, what format the parameter should be in, what values are allowed, and so on.

required: Whether the parameter is required or optional. By default, all parameters are considered required.

If a parameter is annotated as @Nullable, it will be considered optional unless explicitly marked as required using the @ToolParam annotation.

Besides the @ToolParam annotation, you can also use the @Schema annotation from Swagger or @JsonProperty from Jackson. See JSON Schema for more details.

When using the programmatic specification approach, you can pass the MethodToolCallback instance to the toolCallbacks() method of ChatClient. The tool will only be available for the specific chat request it’s added to.

When using the programmatic specification approach, you can add default tools to a ChatClient.Builder by passing the MethodToolCallback instance to the defaultToolCallbacks() method. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

When using the programmatic specification approach, you can pass the MethodToolCallback instance to the toolCallbacks() method of the ToolCallingChatOptions you use to call a ChatModel. The tool will only be available for the specific chat request it’s added to.

When using the programmatic specification approach, you can add default tools to a ChatModel at construction time by passing the MethodToolCallback instance to the toolCallbacks() method of the ToolCallingChatOptions instance used to create the ChatModel. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

The following types are not currently supported as parameters or return types for methods used as tools:

Asynchronous types (e.g. CompletableFuture, Future)

Reactive types (e.g. Flow, Mono, Flux)

Functional types (e.g. Function, Supplier, Consumer).

Functional types are supported using the function-based tool specification approach. See Functions as Tools for more details.

Spring AI provides built-in support for specifying tools from functions, either programmatically using the low-level FunctionToolCallback implementation or dynamically as @Bean(s) resolved at runtime.

You can turn a functional type (Function, Supplier, Consumer, or BiFunction) into a tool by building a FunctionToolCallback programmatically.

The FunctionToolCallback.Builder allows you to build a FunctionToolCallback instance and provide key information about the tool:

name: The name of the tool. AI models use this name to identify the tool when calling it. Therefore, it’s not allowed to have two tools with the same name in the same context. The name must be unique across all the tools available to the model for a specific chat request. Required.

toolFunction: The functional object that represents the tool method (Function, Supplier, Consumer, or BiFunction). Required.

description: The description for the tool, which can be used by the model to understand when and how to call the tool. If not provided, the method name will be used as the tool description. However, it’s strongly recommended to provide a detailed description because that’s paramount for the model to understand the tool’s purpose and how to use it. Failing in providing a good description can lead to the model not using the tool when it should or using it incorrectly.

inputType: The type of the function input. Required.

inputSchema: The JSON schema for the input parameters of the tool. If not provided, the schema will be generated automatically based on the inputType. You can use the @ToolParam annotation to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required. See JSON Schema for more details.

toolMetadata: The ToolMetadata instance that defines additional settings such as whether the result should be returned directly to the client, and the result converter to use. You can build it using the ToolMetadata.Builder class.

toolCallResultConverter: The ToolCallResultConverter instance to use for converting the result of a tool call to a String object to send back to the AI model. If not provided, the default converter will be used (DefaultToolCallResultConverter).

The ToolMetadata.Builder allows you to build a ToolMetadata instance and define additional settings for the tool:

returnDirect: Whether the tool result should be returned directly to the client or passed back to the model. See Return Direct for more details.

The function inputs and outputs can be either Void or POJOs. The input and output POJOs must be serializable, as the result will be serialized and sent back to the model. The function as well as the input and output types must be public.

When using the programmatic specification approach, you can pass the FunctionToolCallback instance to the toolCallbacks() method of ChatClient. The tool will only be available for the specific chat request it’s added to.

When using the programmatic specification approach, you can add default tools to a ChatClient.Builder by passing the FunctionToolCallback instance to the defaultToolCallbacks() method. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

When using the programmatic specification approach, you can pass the FunctionToolCallback instance to the toolCallbacks() method of ToolCallingChatOptions. The tool will only be available for the specific chat request it’s added to.

When using the programmatic specification approach, you can add default tools to a ChatModel at construction time by passing the FunctionToolCallback instance to the toolCallbacks() method of the ToolCallingChatOptions instance used to create the ChatModel. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

Instead of specifying tools programmatically, you can define tools as Spring beans and let Spring AI resolve them dynamically at runtime using the ToolCallbackResolver interface (via the SpringBeanToolCallbackResolver implementation). This option gives you the possibility to use any Function, Supplier, Consumer, or BiFunction bean as a tool. The bean name will be used as the tool name, and the @Description annotation from Spring Framework can be used to provide a description for the tool, used by the model to understand when and how to call the tool. If you don’t provide a description, the method name will be used as the tool description. However, it’s strongly recommended to provide a detailed description because that’s paramount for the model to understand the tool’s purpose and how to use it. Failing in providing a good description can lead to the model not using the tool when it should or using it incorrectly.

The JSON schema for the input parameters of the tool will be generated automatically. You can use the @ToolParam annotation to provide additional information about the input parameters, such as a description or whether the parameter is required or optional. By default, all input parameters are considered required. See JSON Schema for more details.

This tool specification approach has the drawback of not guaranteeing type safety, as the tool resolution is done at runtime. To mitigate this, you can specify the tool name explicitly using the @Bean annotation and storing the value in a constant, so that you can use it in a chat request instead of hard-coding the tool name.

When using the dynamic specification approach, you can pass the tool name (i.e. the function bean name) to the toolNames() method of ChatClient. The tool will only be available for the specific chat request it’s added to.

When using the dynamic specification approach, you can add default tools to a ChatClient.Builder by passing the tool name to the defaultToolNames() method. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

When using the dynamic specification approach, you can pass the tool name to the toolNames() method of the ToolCallingChatOptions you use to call the ChatModel. The tool will only be available for the specific chat request it’s added to.

When using the dynamic specification approach, you can add default tools to ChatModel at construction time by passing the tool name to the toolNames() method of the ToolCallingChatOptions instance used to create the ChatModel. If both default and runtime tools are provided, the runtime tools will completely override the default tools.

The following types are not currently supported as input or output types for functions used as tools:

Collection types (e.g. List, Map, Array, Set)

Asynchronous types (e.g. CompletableFuture, Future)

Reactive types (e.g. Flow, Mono, Flux).

Primitive types and collections are supported using the method-based tool specification approach. See Methods as Tools for more details.

In Spring AI, tools are modeled via the ToolCallback interface. In the previous sections, we’ve seen how to define tools from methods and functions using the built-in support provided by Spring AI (see Methods as Tools and Functions as Tools). This section will dive deeper into the tool specification and how to customize and extend it to support more use cases.

The ToolCallback interface provides a way to define a tool that can be called by the AI model, including both definition and execution logic. It’s the main interface to implement when you want to define a tool from scratch. For example, you can define a ToolCallback from an MCP Client (using the Model Context Protocol) or a ChatClient (to build a modular agentic application).

The interface provides the following methods:

Spring AI provides built-in implementations for tool methods (MethodToolCallback) and tool functions (FunctionToolCallback).

The ToolDefinition interface provides the required information for the AI model to know about the availability of the tool, including the tool name, description, and input schema. Each ToolCallback implementation must provide a ToolDefinition instance to define the tool.

The interface provides the following methods:

The ToolDefinition.Builder lets you build a ToolDefinition instance using the default implementation (DefaultToolDefinition).

When building tools from a method, the ToolDefinition is automatically generated for you. In case you prefer to generate the ToolDefinition yourself, you can use this convenient builder.

The ToolDefinition generated from a method includes the method name as the tool name, the method name as the tool description, and the JSON schema of the method input parameters. If the method is annotated with @Tool, the tool name and description will be taken from the annotation, if set.

If you’d rather provide some or all of the attributes explicitly, you can use the ToolDefinition.Builder to build a custom ToolDefinition instance.

When building tools from a function, the ToolDefinition is automatically generated for you. When you use the FunctionToolCallback.Builder to build a FunctionToolCallback instance, you can provide the tool name, description, and input schema that will be used to generate the ToolDefinition. See Functions as Tools for more details.

When providing a tool to the AI model, the model needs to know the schema of the input type for calling the tool. The schema is used to understand how to call the tool and prepare the tool request. Spring AI provides built-in support for generating the JSON Schema of the input type for a tool via the JsonSchemaGenerator class. The schema is provided as part of the ToolDefinition.

The JsonSchemaGenerator class is used under the hood to generate the JSON schema for the input parameters of a method or a function, using any of the strategies described in Methods as Tools and Functions as Tools. The JSON schema generation logic supports a series of annotations that you can use on the input parameters for methods and functions to customize the resulting schema.

This section describes two main options you can customize when generating the JSON schema for the input parameters of a tool: description and required status.

Besides providing a description for the tool itself, you can also provide a description for the input parameters of a tool. The description can be used to provide key information about the input parameters, such as what format the parameter should be in, what values are allowed, and so on. This is useful to help the model understand the input schema and how to use it. Spring AI provides built-in support for generating the description for an input parameter using one of the following annotations:

@ToolParam(description = "…​") from Spring AI

@JsonClassDescription(description = "…​") from Jackson

@JsonPropertyDescription(description = "…​") from Jackson

@Schema(description = "…​") from Swagger.

This approach works for both methods and functions, and you can use it recursively for nested types.

By default, each input parameter is considered required, which forces the AI model to provide a value for it when calling the tool. However, you can make an input parameter optional by using one of the following annotations, in this order of precedence:

@ToolParam(required = false) from Spring AI

@JsonProperty(required = false) from Jackson

@Schema(required = false) from Swagger

@Nullable from Spring Framework.

This approach works for both methods and functions, and you can use it recursively for nested types.

The result of a tool call is serialized using a ToolCallResultConverter and then sent back to the AI model. The ToolCallResultConverter interface provides a way to convert the result of a tool call to a String object.

The interface provides the following method:

The result must be a serializable type. By default, the result is serialized to JSON using Jackson (DefaultToolCallResultConverter), but you can customize the serialization process by providing your own ToolCallResultConverter implementation.

Spring AI relies on the ToolCallResultConverter in both method and function tools.

When building tools from a method with the declarative approach, you can provide a custom ToolCallResultConverter to use for the tool by setting the resultConverter() attribute of the @Tool annotation.

If using the programmatic approach, you can provide a custom ToolCallResultConverter to use for the tool by setting the resultConverter() attribute of the MethodToolCallback.Builder.

See Methods as Tools for more details.

When building tools from a function using the programmatic approach, you can provide a custom ToolCallResultConverter to use for the tool by setting the resultConverter() attribute of the FunctionToolCallback.Builder.

See Functions as Tools for more details.

Spring AI supports passing additional contextual information to tools through the ToolContext API. This feature allows you to provide extra, user-provided data that can be used within the tool execution along with the tool arguments passed by the AI model.

The ToolContext is populated with the data provided by the user when invoking ChatClient.

Similarly, you can define tool context data when invoking the ChatModel directly.

If the toolContext option is set both in the default options and in the runtime options, the resulting ToolContext will be the merge of the two, where the runtime options take precedence over the default options.

By default, the result of a tool call is sent back to the model as a response. Then, the model can use the result to continue the conversation.

There are cases where you’d rather return the result directly to the caller instead of sending it back to the model. For example, if you build an agent that relies on a RAG tool, you might want to return the result directly to the caller instead of sending it back to the model for unnecessary post-processing. Or perhaps you have certain tools that should end the reasoning loop of the agent.

Each ToolCallback implementation can define whether the result of a tool call should be returned directly to the caller or sent back to the model. By default, the result is sent back to the model. But you can change this behavior per tool.

The ToolCallingManager, responsible for managing the tool execution lifecycle, is in charge of handling the returnDirect attribute associated with the tool. If the attribute is set to true, the result of the tool call is returned directly to the caller. Otherwise, the result is sent back to the model.

When we want to make a tool available to the model, we include its definition in the chat request. If we want the result of the tool execution to be returned directly to the caller, we set the returnDirect attribute to true.

When the model decides to call a tool, it sends a response with the tool name and the input parameters modeled after the defined schema.

The application is responsible for using the tool name to identify and execute the tool with the provided input parameters.

The result of the tool call is processed by the application.

The application sends the tool call result directly to the caller, instead of sending it back to the model.

When building tools from a method with the declarative approach, you can mark a tool to return the result directly to the caller by setting the returnDirect attribute of the @Tool annotation to true.

If using the programmatic approach, you can set the returnDirect attribute via the ToolMetadata interface and pass it to the MethodToolCallback.Builder.

See Methods as Tools for more details.

When building tools from a function with the programmatic approach, you can set the returnDirect attribute via the ToolMetadata interface and pass it to the FunctionToolCallback.Builder.

See Functions as Tools for more details.

The tool execution is the process of calling the tool with the provided input arguments and returning the result. The tool execution is handled by the ToolCallingManager interface, which is responsible for managing the tool execution lifecycle.

If you’re using any of the Spring AI Spring Boot Starters, DefaultToolCallingManager is the autoconfigured implementation of the ToolCallingManager interface. You can customize the tool execution behavior by providing your own ToolCallingManager bean.

By default, Spring AI manages the tool execution lifecycle transparently for you from within each ChatModel implementation. But you have the possibility to opt-out of this behavior and control the tool execution yourself. This section describes these two scenarios.

When using the default behavior, Spring AI will automatically intercept any tool call request from the model, call the tool and return the result to the model. All of this is done transparently for you by each ChatModel implementation using a ToolCallingManager.

When we want to make a tool available to the model, we include its definition in the chat request (Prompt) and invoke the ChatModel API which sends the request to the AI model.

When the model decides to call a tool, it sends a response (ChatResponse) with the tool name and the input parameters modeled after the defined schema.

The ChatModel sends the tool call request to the ToolCallingManager API.

The ToolCallingManager is responsible for identifying the tool to call and executing it with the provided input parameters.

The result of the tool call is returned to the ToolCallingManager.

The ToolCallingManager returns the tool execution result back to the ChatModel.

The ChatModel sends the tool execution result back to the AI model (ToolResponseMessage).

The AI model generates the final response using the tool call result as additional context and sends it back to the caller (ChatResponse) via the ChatClient.

The logic determining whether a tool call is eligible for execution is handled by the ToolExecutionEligibilityPredicate interface. By default, the tool execution eligibility is determined by checking if the internalToolExecutionEnabled attribute of ToolCallingChatOptions is set to true (the default value), and if the ChatResponse contains any tool calls.

You can provide your custom implementation of ToolExecutionEligibilityPredicate when creating the ChatModel bean.

There are cases where you’d rather control the tool execution lifecycle yourself. You can do so by setting the internalToolExecutionEnabled attribute of ToolCallingChatOptions to false.

When you invoke a ChatModel with this option, the tool execution will be delegated to the caller, giving you full control over the tool execution lifecycle. It’s your responsibility checking for tool calls in the ChatResponse and executing them using the ToolCallingManager.

The following example demonstrates a minimal implementation of the user-controlled tool execution approach:

The next examples shows a minimal implementation of the user-controlled tool execution approach combined with the usage of the ChatMemory API:

When a tool call fails, the exception is propagated as a ToolExecutionException which can be caught to handle the error. A ToolExecutionExceptionProcessor can be used to handle a ToolExecutionException with two outcomes: either producing an error message to be sent back to the AI model or throwing an exception to be handled by the caller.

If you’re using any of the Spring AI Spring Boot Starters, DefaultToolExecutionExceptionProcessor is the autoconfigured implementation of the ToolExecutionExceptionProcessor interface. By default, the error message of RuntimeException is sent back to the model, while checked exceptions and Errors (e.g., IOException, OutOfMemoryError) are always thrown. The DefaultToolExecutionExceptionProcessor constructor lets you set the alwaysThrow attribute to true or false. If true, an exception will be thrown instead of sending an error message back to the model.

You can use the `spring.ai.tools.throw-exception-on-error property to control the behavior of the DefaultToolExecutionExceptionProcessor bean:

spring.ai.tools.throw-exception-on-error

If true, tool calling errors are thrown as exceptions for the caller to handle. If false, errors are converted to messages and sent back to the AI model, allowing it to process and respond to the error.

The ToolExecutionExceptionProcessor is used internally by the default ToolCallingManager (DefaultToolCallingManager) to handle exceptions during tool execution. See Tool Execution for more details about the tool execution lifecycle.

The main approach for passing tools to a model is by providing the ToolCallback(s) when invoking the ChatClient or the ChatModel, using one of the strategies described in Methods as Tools and Functions as Tools.

However, Spring AI also supports resolving tools dynamically at runtime using the ToolCallbackResolver interface.

When using this approach:

On the client-side, you provide the tool names to the ChatClient or the ChatModel instead of the ToolCallback(s).

On the server-side, a ToolCallbackResolver implementation is responsible for resolving the tool names to the corresponding ToolCallback instances.

By default, Spring AI relies on a DelegatingToolCallbackResolver that delegates the tool resolution to a list of ToolCallbackResolver instances:

The SpringBeanToolCallbackResolver resolves tools from Spring beans of type Function, Supplier, Consumer, or BiFunction. See Dynamic Specification: @Bean for more details.

The StaticToolCallbackResolver resolves tools from a static list of ToolCallback instances. When using the Spring Boot Autoconfiguration, this resolver is automatically configured with all the beans of type ToolCallback defined in the application context.

If you rely on the Spring Boot Autoconfiguration, you can customize the resolution logic by providing a custom ToolCallbackResolver bean.

The ToolCallbackResolver is used internally by the ToolCallingManager to resolve tools dynamically at runtime, supporting both Framework-Controlled Tool Execution and User-Controlled Tool Execution.

Tool calling includes observability support with spring.ai.tool observations that measure completion time and propagate tracing information. See Tool Calling Observability.

Optionally, Spring AI can export tool call arguments and results as span attributes, disabled by default for sensitivity reasons. Details: Tool Call Arguments and Result Data.

All the main operations of the tool calling features are logged at the DEBUG level. You can enable the logging by setting the log level to DEBUG for the org.springframework.ai package.

**Examples:**

Example 1 (java):
```java
import java.time.LocalDateTime;
import org.springframework.ai.tool.annotation.Tool;
import org.springframework.context.i18n.LocaleContextHolder;

class DateTimeTools {

    @Tool(description = "Get the current date and time in the user's timezone")
    String getCurrentDateTime() {
        return LocalDateTime.now().atZone(LocaleContextHolder.getTimeZone().toZoneId()).toString();
    }

}
```

Example 2 (java):
```java
ChatModel chatModel = ...

String response = ChatClient.create(chatModel)
        .prompt("What day is tomorrow?")
        .tools(new DateTimeTools())
        .call()
        .content();

System.out.println(response);
```

Example 3 (unknown):
```unknown
Tomorrow is 2015-10-21.
```

Example 4 (unknown):
```unknown
I am an AI and do not have access to real-time information. Please provide the current date so I can accurately determine what day tomorrow will be.
```

---

## Docker Compose :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/docker-compose.html

**Contents:**
- Docker Compose
- Service Connections

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI provides Spring Boot auto-configuration for establishing a connection to a model service or vector store running via Docker Compose. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The following service connection factories are provided in the spring-ai-spring-boot-docker-compose module:

AwsOpenSearchConnectionDetails

Containers named localstack/localstack

ChromaConnectionDetails

Containers named chromadb/chroma, ghcr.io/chroma-core/chroma

MongoConnectionDetails

Containers named mongodb/mongodb-atlas-local

OllamaConnectionDetails

Containers named ollama/ollama

OpenSearchConnectionDetails

Containers named opensearchproject/opensearch

QdrantConnectionDetails

Containers named qdrant/qdrant

TypesenseConnectionDetails

Containers named typesense/typesense

WeaviateConnectionDetails

Containers named semitechnologies/weaviate, cr.weaviate.io/semitechnologies/weaviate

McpSseClientConnectionDetails

Containers named docker/mcp-gateway

More service connections are provided by the spring boot module spring-boot-docker-compose. Refer to the Docker Compose Support documentation page for the full list.

**Examples:**

Example 1 (xml):
```xml
<dependency>
   <groupId>org.springframework.ai</groupId>
   <artifactId>spring-ai-spring-boot-docker-compose</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-spring-boot-docker-compose'
}
```

---

## Docker Compose :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/docker-compose.html

**Contents:**
- Docker Compose
- Service Connections

For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI provides Spring Boot auto-configuration for establishing a connection to a model service or vector store running via Docker Compose. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The following service connection factories are provided in the spring-ai-spring-boot-docker-compose module:

AwsOpenSearchConnectionDetails

Containers named localstack/localstack

ChromaConnectionDetails

Containers named chromadb/chroma, ghcr.io/chroma-core/chroma

MongoConnectionDetails

Containers named mongodb/mongodb-atlas-local

OllamaConnectionDetails

Containers named ollama/ollama

OpenSearchConnectionDetails

Containers named opensearchproject/opensearch

QdrantConnectionDetails

Containers named qdrant/qdrant

TypesenseConnectionDetails

Containers named typesense/typesense

WeaviateConnectionDetails

Containers named semitechnologies/weaviate, cr.weaviate.io/semitechnologies/weaviate

More service connections are provided by the spring boot module spring-boot-docker-compose. Refer to the Docker Compose Support documentation page for the full list.

**Examples:**

Example 1 (xml):
```xml
<dependency>
   <groupId>org.springframework.ai</groupId>
   <artifactId>spring-ai-spring-boot-docker-compose</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-spring-boot-docker-compose'
}
```

---

## Structured Output Converter :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/structured-output-converter.html

**Contents:**
- Structured Output Converter
- Structured Output API
  - Available Converters
- Using Converters
  - Bean Output Converter
  - Property Ordering in Generated Schema
    - Generic Bean Types
  - Map Output Converter
  - List Output Converter
- Native Structured Output

The ability of LLMs to produce structured outputs is important for downstream applications that rely on reliably parsing output values. Developers want to quickly turn results from an AI model into data types, such as JSON, XML or Java classes, that can be passed to other application functions and methods.

The Spring AI Structured Output Converters help to convert the LLM output into a structured format. As shown in the following diagram, this approach operates around the LLM text completion endpoint:

Generating structured outputs from Large Language Models (LLMs) using generic completion APIs requires careful handling of inputs and outputs. The structured output converter plays a crucial role before and after the LLM call, ensuring the desired output structure is achieved.

Before the LLM call, the converter appends format instructions to the prompt, providing explicit guidance to the models on generating the desired output structure. These instructions act as a blueprint, shaping the model’s response to conform to the specified format.

After the LLM call, the converter takes the model’s output text and transforms it into instances of the structured type. This conversion process involves parsing the raw text output and mapping it to the corresponding structured data representation, such as JSON, XML, or domain-specific data structures.

The StructuredOutputConverter interface allows you to obtain structured output, such as mapping the output to a Java class or an array of values from the text-based AI Model output. The interface definition is:

It combines the Spring Converter<String, T> interface and the FormatProvider interface

The following diagram shows the data flow when using the structured output API.

The FormatProvider supplies specific formatting guidelines to the AI Model, enabling it to produce text outputs that can be converted into the designated target type T using the Converter. Here is an example of such formatting instructions:

The format instructions are most often appended to the end of the user input using the PromptTemplate like this:

The Converter<String, T> is responsible to transform output text from the model into instances of the specified type T.

Currently, Spring AI provides AbstractConversionServiceOutputConverter, AbstractMessageOutputConverter, BeanOutputConverter, MapOutputConverter and ListOutputConverter implementations:

AbstractConversionServiceOutputConverter<T> - Offers a pre-configured GenericConversionService for transforming LLM output into the desired format. No default FormatProvider implementation is provided.

AbstractMessageOutputConverter<T> - Supplies a pre-configured MessageConverter for converting LLM output into the desired format. No default FormatProvider implementation is provided.

BeanOutputConverter<T> - Configured with a designated Java class (e.g., Bean) or a ParameterizedTypeReference, this converter employs a FormatProvider implementation that directs the AI Model to produce a JSON response compliant with a DRAFT_2020_12, JSON Schema derived from the specified Java class. Subsequently, it utilizes an ObjectMapper to deserialize the JSON output into a Java object instance of the target class.

MapOutputConverter - Extends the functionality of AbstractMessageOutputConverter with a FormatProvider implementation that guides the AI Model to generate an RFC8259 compliant JSON response. Additionally, it incorporates a converter implementation that utilizes the provided MessageConverter to translate the JSON payload into a java.util.Map<String, Object> instance.

ListOutputConverter - Extends the AbstractConversionServiceOutputConverter and includes a FormatProvider implementation tailored for comma-delimited list output. The converter implementation employs the provided ConversionService to transform the model text output into a java.util.List.

The following sections provide guides how to use the available converters to generate structured outputs.

The following example shows how to use BeanOutputConverter to generate the filmography for an actor.

The target record representing actor’s filmography:

Here is how to apply the BeanOutputConverter using the high-level, fluent ChatClient API:

or using the low-level ChatModel API directly:

The BeanOutputConverter supports custom property ordering in the generated JSON schema through the @JsonPropertyOrder annotation. This annotation allows you to specify the exact sequence in which properties should appear in the schema, regardless of their declaration order in the class or record.

For example, to ensure specific ordering of properties in the ActorsFilms record:

This annotation works with both records and regular Java classes.

Use the ParameterizedTypeReference constructor to specify a more complex target class structure. For example, to represent a list of actors and their filmographies:

or using the low-level ChatModel API directly:

The following snippet shows how to use MapOutputConverter to convert the model output to a list of numbers in a map.

or using the low-level ChatModel API directly:

The following snippet shows how to use ListOutputConverter to convert the model output into a list of ice cream flavors.

or using the low-level ChatModel API directly:

Many modern AI models now provide native support for structured output, which offers more reliable results compared to prompt-based formatting. Spring AI supports this through the Native Structured Output feature.

When using native structured output, the JSON schema generated by BeanOutputConverter is sent directly to the model’s structured output API, eliminating the need for format instructions in the prompt. This approach provides:

Higher reliability: The model guarantees output conforming to the schema

Cleaner prompts: No need to append format instructions

Better performance: Models can optimize for structured output internally

To enable native structured output, use the AdvisorParams.ENABLE_NATIVE_STRUCTURED_OUTPUT parameter:

You can also set this globally using defaultAdvisors() on the ChatClient.Builder:

The following models currently support native structured output:

OpenAI: GPT-4o and later models with JSON Schema support

Anthropic: Claude 3.5 Sonnet and later models

Vertex AI Gemini: Gemini 1.5 Pro and later models

Some AI Models provide dedicated configuration options to generate structured (usually JSON) output.

OpenAI Structured Outputs can ensure your model generates responses conforming strictly to your provided JSON Schema. You can choose between the JSON_OBJECT that guarantees the message the model generates is valid JSON or JSON_SCHEMA with a supplied schema that guarantees the model will generate a response that matches your supplied schema (spring.ai.openai.chat.options.responseFormat option).

Azure OpenAI - provides a spring.ai.azure.openai.chat.options.responseFormat options specifying the format that the model must output. Setting to { "type": "json_object" } enables JSON mode, which guarantees the message the model generates is valid JSON.

Ollama - provides a spring.ai.ollama.chat.options.format option to specify the format to return a response in. Currently, the only accepted value is json.

Mistral AI - provides a spring.ai.mistralai.chat.options.responseFormat option to specify the format to return a response in. Setting it to { "type": "json_object" } enables JSON mode, which guarantees the message the model generates is valid JSON.

**Examples:**

Example 1 (java):
```java
public interface StructuredOutputConverter<T> extends Converter<String, T>, FormatProvider {

}
```

Example 2 (java):
```java
public interface FormatProvider {
	String getFormat();
}
```

Example 3 (java):
```java
StructuredOutputConverter outputConverter = ...
    String userInputTemplate = """
        ... user text input ....
        {format}
        """; // user input with a "format" placeholder.
    Prompt prompt = new Prompt(
            PromptTemplate.builder()
						.template(this.userInputTemplate)
						.variables(Map.of(..., "format", this.outputConverter.getFormat())) // replace the "format" placeholder with the converter's format.
						.build().createMessage()
    );
```

Example 4 (java):
```java
record ActorsFilms(String actor, List<String> movies) {
}
```

---

## Prompts :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/prompt.html

**Contents:**
- Prompts
- API Overview
  - Prompt
  - Message
    - Roles
  - PromptTemplate
- Example Usage
  - Using a custom template renderer
  - Using resources instead of raw Strings
- Prompt Engineering

For the latest snapshot version, please use Spring AI 1.1.2!

Prompts are the inputs that guide an AI model to generate specific outputs. The design and phrasing of these prompts significantly influence the model’s responses.

At the lowest level of interaction with AI models in Spring AI, handling prompts in Spring AI is somewhat similar to managing the "View" in Spring MVC. This involves creating extensive text with placeholders for dynamic content. These placeholders are then replaced based on user requests or other code in the application. Another analogy is a SQL statement that contain placeholders for certain expressions.

As Spring AI evolves, it will introduce higher levels of abstraction for interacting with AI models. The foundational classes described in this section can be likened to JDBC in terms of their role and functionality. The ChatModel class, for instance, is analogous to the core JDBC library in the JDK. The ChatClient class can be likened to the JdbcClient, built on top of ChatModel and providing more advanced constructs via Advisor to consider past interactions with the model, augment the prompt with additional contextual documents, and introduce agentic behavior.

The structure of prompts has evolved over time within the AI field. Initially, prompts were simple strings. Over time, they grew to include placeholders for specific inputs, like "USER:", which the AI model recognizes. OpenAI have introduced even more structure to prompts by categorizing multiple message strings into distinct roles before they are processed by the AI model.

It is common to use the call() method of ChatModel that takes a Prompt instance and returns a ChatResponse.

The Prompt class functions as a container for an organized series of Message objects and a request ChatOptions. Every Message embodies a unique role within the prompt, differing in its content and intent. These roles can encompass a variety of elements, from user inquiries to AI-generated responses to relevant background information. This arrangement enables intricate and detailed interactions with AI models, as the prompt is constructed from multiple messages, each assigned a specific role to play in the dialogue.

Below is a truncated version of the Prompt class, with constructors and utility methods omitted for brevity:

The Message interface encapsulates a Prompt textual content, a collection of metadata attributes, and a categorization known as MessageType.

The interface is defined as follows:

The multimodal message types implement also the MediaContent interface providing a list of Media content objects.

Various implementations of the Message interface correspond to different categories of messages that an AI model can process. The Models distinguish between message categories based on conversational roles.

These roles are effectively mapped by the MessageType, as discussed below.

Each message is assigned a specific role. These roles categorize the messages, clarifying the context and purpose of each segment of the prompt for the AI model. This structured approach enhances the nuance and effectiveness of communication with the AI, as each part of the prompt plays a distinct and defined role in the interaction.

The primary roles are:

System Role: Guides the AI’s behavior and response style, setting parameters or rules for how the AI interprets and replies to the input. It’s akin to providing instructions to the AI before initiating a conversation.

User Role: Represents the user’s input – their questions, commands, or statements to the AI. This role is fundamental as it forms the basis of the AI’s response.

Assistant Role: The AI’s response to the user’s input. More than just an answer or reaction, it’s crucial for maintaining the flow of the conversation. By tracking the AI’s previous responses (its 'Assistant Role' messages), the system ensures coherent and contextually relevant interactions. The Assistant message may contain Function Tool Call request information as well. It’s like a special feature in the AI, used when needed to perform specific functions such as calculations, fetching data, or other tasks beyond just talking.

Tool/Function Role: The Tool/Function Role focuses on returning additional information in response to Tool Call Assistant Messages.

Roles are represented as an enumeration in Spring AI as shown below

A key component for prompt templating in Spring AI is the PromptTemplate class, designed to facilitate the creation of structured prompts that are then sent to the AI model for processing

This class uses the TemplateRenderer API to render templates. By default, Spring AI uses the StTemplateRenderer implementation, which is based on the open-source StringTemplate engine developed by Terence Parr. Template variables are identified by the {} syntax, but you can configure the delimiters to use other syntax as well.

Spring AI uses the TemplateRenderer interface to handle the actual substitution of variables into the template string. The default implementation uses [StringTemplate]. You can provide your own implementation of TemplateRenderer if you need custom logic. For scenarios where no template rendering is required (e.g., the template string is already complete), you can use the provided NoOpTemplateRenderer.

The interfaces implemented by this class support different aspects of prompt creation:

PromptTemplateStringActions focuses on creating and rendering prompt strings, representing the most basic form of prompt generation.

PromptTemplateMessageActions is tailored for prompt creation through the generation and manipulation of Message objects.

PromptTemplateActions is designed to return the Prompt object, which can be passed to ChatModel for generating a response.

While these interfaces might not be used extensively in many projects, they show the different approaches to prompt creation.

The implemented interfaces are

The method String render(): Renders a prompt template into a final string format without external input, suitable for templates without placeholders or dynamic content.

The method String render(Map<String, Object> model): Enhances rendering functionality to include dynamic content. It uses a Map<String, Object> where map keys are placeholder names in the prompt template, and values are the dynamic content to be inserted.

The method Message createMessage(): Creates a Message object without additional data, used for static or predefined message content.

The method Message createMessage(List<Media> mediaList): Creates a Message object with static textual and media content.

The method Message createMessage(Map<String, Object> model): Extends message creation to integrate dynamic content, accepting a Map<String, Object> where each entry represents a placeholder in the message template and its corresponding dynamic value.

The method Prompt create(): Generates a Prompt object without external data inputs, ideal for static or predefined prompts.

The method Prompt create(ChatOptions modelOptions): Generates a Prompt object without external data inputs and with specific options for the chat request.

The method Prompt create(Map<String, Object> model): Expands prompt creation capabilities to include dynamic content, taking a Map<String, Object> where each map entry is a placeholder in the prompt template and its associated dynamic value.

The method Prompt create(Map<String, Object> model, ChatOptions modelOptions): Expands prompt creation capabilities to include dynamic content, taking a Map<String, Object> where each map entry is a placeholder in the prompt template and its associated dynamic value, and specific options for the chat request.

A simple example taken from the AI Workshop on PromptTemplates is shown below.

Another example taken from the AI Workshop on Roles is shown below.

This shows how you can build up the Prompt instance by using the SystemPromptTemplate to create a Message with the system role passing in placeholder values. The message with the role user is then combined with the message of the role system to form the prompt. The prompt is then passed to the ChatModel to get a generative response.

You can use a custom template renderer by implementing the TemplateRenderer interface and passing it to the PromptTemplate constructor. You can also keep using the default StTemplateRenderer, but with a custom configuration.

By default, template variables are identified by the {} syntax. If you’re planning to include JSON in your prompt, you might want to use a different syntax to avoid conflicts with JSON syntax. For example, you can use the < and > delimiters.

Spring AI supports the org.springframework.core.io.Resource abstraction, so you can put prompt data in a file that can directly be used in a PromptTemplate. For example, you can define a field in your Spring managed component to retrieve the Resource.

and then pass that resource to the SystemPromptTemplate directly.

In generative AI, the creation of prompts is a crucial task for developers. The quality and structure of these prompts significantly influence the effectiveness of the AI’s output. Investing time and effort in designing thoughtful prompts can greatly improve the results from the AI.

Sharing and discussing prompts is a common practice in the AI community. This collaborative approach not only creates a shared learning environment but also leads to the identification and use of highly effective prompts.

Research in this area often involves analyzing and comparing different prompts to assess their effectiveness in various situations. For example, a significant study demonstrated that starting a prompt with "Take a deep breath and work on this problem step by step" significantly enhanced problem-solving efficiency. This highlights the impact that well-chosen language can have on generative AI systems' performance.

Grasping the most effective use of prompts, particularly with the rapid advancement of AI technologies, is a continuous challenge. You should recognize the importance of prompt engineering and consider using insights from the community and research to improve prompt creation strategies.

When developing prompts, it’s important to integrate several key components to ensure clarity and effectiveness:

Instructions: Offer clear and direct instructions to the AI, similar to how you would communicate with a person. This clarity is essential for helping the AI "understand" what is expected.

External Context: Include relevant background information or specific guidance for the AI’s response when necessary. This "external context" frames the prompt and aids the AI in grasping the overall scenario.

User Input: This is the straightforward part - the user’s direct request or question forming the core of the prompt.

Output Indicator: This aspect can be tricky. It involves specifying the desired format for the AI’s response, such as JSON. However, be aware that the AI might not always adhere strictly to this format. For instance, it might prepend a phrase like "here is your JSON" before the actual JSON data, or sometimes generate a JSON-like structure that is not accurate.

Providing the AI with examples of the anticipated question and answer format can be highly beneficial when crafting prompts. This practice helps the AI "understand" the structure and intent of your query, leading to more precise and relevant responses. While this documentation does not delve deeply into these techniques, they provide a starting point for further exploration in AI prompt engineering.

Following is a list of resources for further investigation.

Text Summarization: Reduces extensive text into concise summaries, capturing key points and main ideas while omitting less critical details.

Question Answering: Focuses on deriving specific answers from provided text, based on user-posed questions. It’s about pinpointing and extracting relevant information in response to queries.

Text Classification: Systematically categorizes text into predefined categories or groups, analyzing the text and assigning it to the most fitting category based on its content.

Conversation: Creates interactive dialogues where the AI can engage in back-and-forth communication with users, simulating a natural conversation flow.

Code Generation: Generates functional code snippets based on specific user requirements or descriptions, translating natural language instructions into executable code.

Zero-shot, Few-shot Learning: Enables the model to make accurate predictions or responses with minimal to no prior examples of the specific problem type, understanding and acting on new tasks using learned generalizations.

Chain-of-Thought: Links multiple AI responses to create a coherent and contextually aware conversation. It helps the AI maintain the thread of the discussion, ensuring relevance and continuity.

ReAct (Reason + Act): In this method, the AI first analyzes (reasons about) the input, then determines the most appropriate course of action or response. It combines understanding with decision-making.

Framework for Prompt Creation and Optimization: Microsoft offers a structured approach to developing and refining prompts. This framework guides users in creating effective prompts that elicit the desired responses from AI models, optimizing the interaction for clarity and efficiency.

Tokens are essential in how AI models process text, acting as a bridge that converts words (as we understand them) into a format that AI models can process. This conversion occurs in two stages: words are transformed into tokens upon input, and these tokens are then converted back into words in the output.

Tokenization, the process of breaking down text into tokens, is fundamental to how AI models comprehend and process language. The AI model works with this tokenized format to understand and respond to prompts.

To better understand tokens, think of them as portions of words. Typically, a token represents about three-quarters of a word. For instance, the complete works of Shakespeare, totaling roughly 900,000 words, would translate to around 1.2 million tokens.

Experiment with the OpenAI Tokenizer UI to see how words are converted into tokens.

Tokens have practical implications beyond their technical role in AI processing, especially regarding billing and model capabilities:

Billing: AI model services often bill based on token usage. Both the input (prompt) and the output (response) contribute to the total token count, making shorter prompts more cost-effective.

Model Limits: Different AI models have varying token limits, defining their "context window" – the maximum amount of information they can process at a time. For example, GPT-3’s limit is 4K tokens, while other models like Claude 2 and Meta Llama 2 have limits of 100K tokens, and some research models can handle up to 1 million tokens.

Context Window: A model’s token limit determines its context window. Inputs exceeding this limit are not processed by the model. It’s crucial to send only the minimal effective set of information for processing. For example, when inquiring about "Hamlet," there’s no need to include tokens from all of Shakespeare’s other works.

Response Metadata: The metadata of a response from an AI model includes the number of tokens used, a vital piece of information for managing usage and costs.

**Examples:**

Example 1 (java):
```java
public class Prompt implements ModelRequest<List<Message>> {

    private final List<Message> messages;

    private ChatOptions chatOptions;
}
```

Example 2 (java):
```java
public interface Content {

	String getContent();

	Map<String, Object> getMetadata();
}

public interface Message extends Content {

	MessageType getMessageType();
}
```

Example 3 (java):
```java
public interface MediaContent extends Content {

	Collection<Media> getMedia();

}
```

Example 4 (java):
```java
public enum MessageType {

	USER("user"),

	ASSISTANT("assistant"),

	SYSTEM("system"),

	TOOL("tool");

    ...
}
```

---

## Recursive Advisors :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/advisors-recursive.html

**Contents:**
- Recursive Advisors
- What is a Recursive Advisor?
- Built-in Recursive Advisors
  - ToolCallAdvisor
    - Return Direct Functionality
  - StructuredOutputValidationAdvisor

Recursive advisors are a special type of advisor that can loop through the downstream advisor chain multiple times. This pattern is useful when you need to repeatedly call the LLM until a certain condition is met, such as:

Executing tool calls in a loop until no more tools need to be called

Validating structured output and retrying if validation fails

Implementing Evaluation logic with modifications to the request

Implementing retry logic with modifications to the request

The CallAdvisorChain.copy(CallAdvisor after) method is the key utility that enables recursive advisor patterns. It creates a new advisor chain that contains only the advisors that come after the specified advisor in the original chain and allows the recursive advisor to call this sub-chain as needed. This approach ensures that:

The recursive advisor can loop through the remaining advisors in the chain

Other advisors in the chain can observe and intercept each iteration

The advisor chain maintains proper ordering and observability

The recursive advisor doesn’t re-execute advisors that came before it

Spring AI provides two built-in recursive advisors that demonstrate this pattern:

The ToolCallAdvisor implements the tool calling loop as part of the advisor chain, rather than relying on the model’s internal tool execution. This enables other advisors in the chain to intercept and observe the tool calling process.

Disables the model’s internal tool execution by setting setInternalToolExecutionEnabled(false)

Loops through the advisor chain until no more tool calls are present

Supports "return direct" functionality - when a tool execution has returnDirect=true, it interrupts the tool calling loop and returns the tool execution result directly to the client application instead of sending it back to the LLM

Uses callAdvisorChain.copy(this) to create a sub-chain for recursive calls

Includes null safety checks to handle cases where the chat response might be null

The "return direct" feature allows tools to bypass the LLM and return their results directly to the client application. This is useful when:

The tool’s output is the final answer and doesn’t need LLM processing

You want to reduce latency by avoiding an additional LLM call

The tool result should be returned as-is without interpretation

When a tool execution has returnDirect=true, the ToolCallAdvisor will:

Execute the tool call as normal

Detect the returnDirect flag in the ToolExecutionResult

Break out of the tool calling loop

Return the tool execution result directly to the client application as a ChatResponse with the tool’s output as the generation content

The StructuredOutputValidationAdvisor validates the structured JSON output against a generated JSON schema and retries the call if validation fails, up to a specified number of attempts.

Automatically generates a JSON schema from the expected output type

Validates the LLM response against the schema

Retries the call if validation fails, up to a configurable number of attempts

Augments the prompt with validation error messages on retry attempts to help the LLM correct its output

Uses callAdvisorChain.copy(this) to create a sub-chain for recursive calls

Optionally supports a custom ObjectMapper for JSON processing

**Examples:**

Example 1 (java):
```java
var toolCallAdvisor = ToolCallAdvisor.builder()
    .toolCallingManager(toolCallingManager)
    .advisorOrder(BaseAdvisor.HIGHEST_PRECEDENCE + 300)
    .build();

var chatClient = ChatClient.builder(chatModel)
    .defaultAdvisors(toolCallAdvisor)
    .build();
```

Example 2 (java):
```java
var validationAdvisor = StructuredOutputValidationAdvisor.builder()
    .outputType(MyResponseType.class)
    .maxRepeatAttempts(3)
    .advisorOrder(BaseAdvisor.HIGHEST_PRECEDENCE + 1000)
    .build();

var chatClient = ChatClient.builder(chatModel)
    .defaultAdvisors(validationAdvisor)
    .build();
```

---

## Advisors API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/advisors.html

**Contents:**
- Advisors API
- Core Components
  - Advisor Order
- API Overview
- Implementing an Advisor
  - Examples
    - Logging Advisor
    - Re-Reading (Re2) Advisor
    - Spring AI Built-in Advisors
      - Chat Memory Advisors

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The Spring AI Advisors API provides a flexible and powerful way to intercept, modify, and enhance AI-driven interactions in your Spring applications. By leveraging the Advisors API, developers can create more sophisticated, reusable, and maintainable AI components.

The key benefits include encapsulating recurring Generative AI patterns, transforming data sent to and from Large Language Models (LLMs), and providing portability across various models and use cases.

You can configure existing advisors using the ChatClient API as shown in the following example:

It is recommend to register the advisors at build time using builder’s defaultAdvisors() method.

Advisors also participate in the Observability stack, so you can view metrics and traces related to their execution.

Learn about Question Answer Advisor

Learn about Chat Memory Advisor

The API consists of CallAdvisor and CallAdvisorChain for non-streaming scenarios, and StreamAdvisor and StreamAdvisorChain for streaming scenarios. It also includes ChatClientRequest to represent the unsealed Prompt request, ChatClientResponse for the Chat Completion response. Both hold an advise-context to share state across the advisor chain.

The adviseCall() and the adviseStream() are the key advisor methods, typically performing actions such as examining the unsealed Prompt data, customizing and augmenting the Prompt data, invoking the next entity in the advisor chain, optionally blocking the request, examining the chat completion response, and throwing exceptions to indicate processing errors.

In addition the getOrder() method determines advisor order in the chain, while getName() provides a unique advisor name.

The Advisor Chain, created by the Spring AI framework, allows sequential invocation of multiple advisors ordered by their getOrder() values. The lower values are executed first. The last advisor, added automatically, sends the request to the LLM.

Following flow diagram illustrates the interaction between the advisor chain and the Chat Model:

The Spring AI framework creates an ChatClientRequest from user’s Prompt along with an empty advisor context object.

Each advisor in the chain processes the request, potentially modifying it. Alternatively, it can choose to block the request by not making the call to invoke the next entity. In the latter case, the advisor is responsible for filling out the response.

The final advisor, provided by the framework, sends the request to the Chat Model.

The Chat Model’s response is then passed back through the advisor chain and converted into ChatClientResponse. Later includes the shared advisor context instance.

Each advisor can process or modify the response.

The final ChatClientResponse is returned to the client by extracting the ChatCompletion.

The execution order of advisors in the chain is determined by the getOrder() method. Key points to understand:

Advisors with lower order values are executed first.

The advisor chain operates as a stack:

The first advisor in the chain is the first to process the request.

It is also the last to process the response.

To control execution order:

Set the order close to Ordered.HIGHEST_PRECEDENCE to ensure an advisor is executed first in the chain (first for request processing, last for response processing).

Set the order close to Ordered.LOWEST_PRECEDENCE to ensure an advisor is executed last in the chain (last for request processing, first for response processing).

Higher values are interpreted as lower priority.

If multiple advisors have the same order value, their execution order is not guaranteed.

The seeming contradiction between order and execution sequence is due to the stack-like nature of the advisor chain:

An advisor with the highest precedence (lowest order value) is added to the top of the stack.

It will be the first to process the request as the stack unwinds.

It will be the last to process the response as the stack rewinds.

As a reminder, here are the semantics of the Spring Ordered interface:

For use cases that need to be first in the chain on both the input and output sides:

Use separate advisors for each side.

Configure them with different order values.

Use the advisor context to share state between them.

The main Advisor interfaces are located in the package org.springframework.ai.chat.client.advisor.api. Here are the key interfaces you’ll encounter when creating your own advisor:

The two sub-interfaces for synchronous and reactive Advisors are

To continue the chain of Advice, use CallAdvisorChain and StreamAdvisorChain in your Advice implementation:

To create an advisor, implement either CallAdvisor or StreamAdvisor (or both). The key method to implement is nextCall() for non-streaming or nextStream() for streaming advisors.

We will provide few hands-on examples to illustrate how to implement advisors for observing and augmenting use-cases.

We can implement a simple logging advisor that logs the ChatClientRequest before and the ChatClientResponse after the call to the next advisor in the chain. Note that the advisor only observes the request and response and does not modify them. This implementation support both non-streaming and streaming scenarios.

The "Re-Reading Improves Reasoning in Large Language Models" article introduces a technique called Re-Reading (Re2) that improves the reasoning capabilities of Large Language Models. The Re2 technique requires augmenting the input prompt like this:

Implementing an advisor that applies the Re2 technique to the user’s input query can be done like this:

Spring AI framework provides several built-in advisors to enhance your AI interactions. Here’s an overview of the available advisors:

These advisors manage conversation history in a chat memory store:

MessageChatMemoryAdvisor

Retrieves memory and adds it as a collection of messages to the prompt. This approach maintains the structure of the conversation history. Note, not all AI Models support this approach.

PromptChatMemoryAdvisor

Retrieves memory and incorporates it into the prompt’s system text.

VectorStoreChatMemoryAdvisor

Retrieves memory from a VectorStore and adds it into the prompt’s system text. This advisor is useful for efficiently searching and retrieving relevant information from large datasets.

QuestionAnswerAdvisor

This advisor uses a vector store to provide question-answering capabilities, implementing the Naive RAG (Retrieval-Augmented Generation) pattern.

RetrievalAugmentationAdvisor

Implements a re-reading strategy for LLM reasoning, dubbed RE2, to enhance understanding in the input phase. Based on the article: [Re-Reading Improves Reasoning in LLMs](arxiv.org/pdf/2309.06275).

A simple advisor designed to prevent the model from generating harmful or inappropriate content.

Non-streaming advisors work with complete requests and responses.

Streaming advisors handle requests and responses as continuous streams, using reactive programming concepts (e.g., Flux for responses).

Keep advisors focused on specific tasks for better modularity.

Use the adviseContext to share state between advisors when necessary.

Implement both streaming and non-streaming versions of your advisor for maximum flexibility.

Carefully consider the order of advisors in your chain to ensure proper data flow.

In 1.0 M2, there were separate RequestAdvisor and ResponseAdvisor interfaces.

RequestAdvisor was invoked before the ChatModel.call and ChatModel.stream methods.

ResponseAdvisor was called after these methods.

In 1.0 M3, these interfaces have been replaced with:

The StreamResponseMode, previously part of ResponseAdvisor, has been removed.

In 1.0.0 these interfaces have been replaced:

CallAroundAdvisor → CallAdvisor, StreamAroundAdvisor → StreamAdvisor, CallAroundAdvisorChain → CallAdvisorChain and StreamAroundAdvisorChain → StreamAdvisorChain.

AdvisedRequest → ChatClientRequest and AdivsedResponse → ChatClientResponse.

The context map was a separate method argument.

The map was mutable and passed along the chain.

The context map is now part of the AdvisedRequest and AdvisedResponse records.

The map is immutable.

To update the context, use the updateContext method, which creates a new unmodifiable map with the updated contents.

**Examples:**

Example 1 (java):
```java
ChatMemory chatMemory = ... // Initialize your chat memory store
VectorStore vectorStore = ... // Initialize your vector store

var chatClient = ChatClient.builder(chatModel)
    .defaultAdvisors(
        MessageChatMemoryAdvisor.builder(chatMemory).build(), // chat-memory advisor
        QuestionAnswerAdvisor.builder(vectorStore).build()    // RAG advisor
    )
    .build();

var conversationId = "678";

String response = this.chatClient.prompt()
    // Set advisor parameters at runtime
    .advisors(advisor -> advisor.param(ChatMemory.CONVERSATION_ID, conversationId))
    .user(userText)
    .call()
	.content();
```

Example 2 (java):
```java
public interface Ordered {

    /**
     * Constant for the highest precedence value.
     * @see java.lang.Integer#MIN_VALUE
     */
    int HIGHEST_PRECEDENCE = Integer.MIN_VALUE;

    /**
     * Constant for the lowest precedence value.
     * @see java.lang.Integer#MAX_VALUE
     */
    int LOWEST_PRECEDENCE = Integer.MAX_VALUE;

    /**
     * Get the order value of this object.
     * <p>Higher values are interpreted as lower priority. As a consequence,
     * the object with the lowest value has the highest priority (somewhat
     * analogous to Servlet {@code load-on-startup} values).
     * <p>Same order values will result in arbitrary sort positions for the
     * affected objects.
     * @return the order value
     * @see #HIGHEST_PRECEDENCE
     * @see #LOWEST_PRECEDENCE
     */
    int getOrder();
}
```

Example 3 (java):
```java
public interface Advisor extends Ordered {

	String getName();

}
```

Example 4 (java):
```java
public interface CallAdvisor extends Advisor {

	ChatClientResponse adviseCall(
		ChatClientRequest chatClientRequest, CallAdvisorChain callAdvisorChain);

}
```

---

## Spring Projects :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/spring-projects.html

---

## Multimodality API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/multimodality.html

**Contents:**
- Multimodality API
- Spring AI Multimodality

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

"All things that are naturally connected ought to be taught in combination" - John Amos Comenius, "Orbis Sensualium Pictus", 1658

Humans process knowledge, simultaneously across multiple modes of data inputs. The way we learn, our experiences are all multimodal. We don’t have just vision, just audio and just text.

Contrary to those principles, the Machine Learning was often focused on specialized models tailored to process a single modality. For instance, we developed audio models for tasks like text-to-speech or speech-to-text, and computer vision models for tasks such as object detection and classification.

However, a new wave of multimodal large language models starts to emerge. Examples include OpenAI’s GPT-4o , Google’s Vertex AI Gemini 1.5, Anthropic’s Claude3, and open source offerings Llama3.2, LLaVA and BakLLaVA are able to accept multiple inputs, including text images, audio and video and generate text responses by integrating these inputs.

Multimodality refers to a model’s ability to simultaneously understand and process information from various sources, including text, images, audio, and other data formats.

The Spring AI Message API provides all necessary abstractions to support multimodal LLMs.

The UserMessage’s content field is used primarily for text inputs, while the optional media field allows adding one or more additional content of different modalities such as images, audio and video. The MimeType specifies the modality type. Depending on the used LLMs, the Media data field can be either the raw media content as a Resource object or a URI to the content.

For example, we can take the following picture (multimodal.test.png) as an input and ask the LLM to explain what it sees.

For most of the multimodal LLMs, the Spring AI code would look something like this:

or with the fluent ChatClient API:

and produce a response like:

This is an image of a fruit bowl with a simple design. The bowl is made of metal with curved wire edges that create an open structure, allowing the fruit to be visible from all angles. Inside the bowl, there are two yellow bananas resting on top of what appears to be a red apple. The bananas are slightly overripe, as indicated by the brown spots on their peels. The bowl has a metal ring at the top, likely to serve as a handle for carrying. The bowl is placed on a flat surface with a neutral-colored background that provides a clear view of the fruit inside.

Spring AI provides multimodal support for the following chat models:

Azure Open AI (e.g. GPT-4o models)

Mistral AI (e.g. Mistral Pixtral models)

Ollama (e.g. LLaVA, BakLLaVA, Llama3.2 models)

OpenAI (e.g. GPT-4 and GPT-4o models)

Vertex AI Gemini (e.g. gemini-1.5-pro-001, gemini-1.5-flash-001 models)

**Examples:**

Example 1 (java):
```java
var imageResource = new ClassPathResource("/multimodal.test.png");

var userMessage = UserMessage.builder()
    .text("Explain what do you see in this picture?") // content
    .media(new Media(MimeTypeUtils.IMAGE_PNG, this.imageResource)) // media
    .build();

ChatResponse response = chatModel.call(new Prompt(this.userMessage));
```

Example 2 (java):
```java
String response = ChatClient.create(chatModel).prompt()
		.user(u -> u.text("Explain what do you see on this picture?")
				    .media(MimeTypeUtils.IMAGE_PNG, new ClassPathResource("/multimodal.test.png")))
		.call()
		.content();
```

---

## Evaluation Testing :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/testing.html

**Contents:**
- Evaluation Testing
- Relevancy Evaluator
- Usage in Integration Tests
  - Custom Template
- FactCheckingEvaluator
  - Usage
  - Example

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Testing AI applications requires evaluating the generated content to ensure the AI model has not produced a hallucinated response.

One method to evaluate the response is to use the AI model itself for evaluation. Select the best AI model for the evaluation, which may not be the same model used to generate the response.

The Spring AI interface for evaluating responses is Evaluator, defined as:

The input to the evaluation is the EvaluationRequest defined as

userText: The raw input from the user as a String

dataList: Contextual data, such as from Retrieval Augmented Generation, appended to the raw input.

responseContent: The AI model’s response content as a String

The RelevancyEvaluator is an implementation of the Evaluator interface, designed to assess the relevance of AI-generated responses against provided context. This evaluator helps assess the quality of a RAG flow by determining if the AI model’s response is relevant to the user’s input with respect to the retrieved context.

The evaluation is based on the user input, the AI model’s response, and the context information. It uses a prompt template to ask the AI model if the response is relevant to the user input and context.

This is the default prompt template used by the RelevancyEvaluator:

Here is an example of usage of the RelevancyEvaluator in an integration test, validating the result of a RAG flow using the RetrievalAugmentationAdvisor:

You can find several integration tests in the Spring AI project that use the RelevancyEvaluator to test the functionality of the QuestionAnswerAdvisor (see tests) and RetrievalAugmentationAdvisor (see tests).

The RelevancyEvaluator uses a default template to prompt the AI model for evaluation. You can customize this behavior by providing your own PromptTemplate object via the .promptTemplate() builder method.

The custom PromptTemplate can use any TemplateRenderer implementation (by default, it uses StPromptTemplate based on the StringTemplate engine). The important requirement is that the template must contain the following placeholders:

a query placeholder to receive the user question.

a response placeholder to receive the AI model’s response.

a context placeholder to receive the context information.

The FactCheckingEvaluator is another implementation of the Evaluator interface, designed to assess the factual accuracy of AI-generated responses against provided context. This evaluator helps detect and reduce hallucinations in AI outputs by verifying if a given statement (claim) is logically supported by the provided context (document).

The 'claim' and 'document' are presented to the AI model for evaluation. Smaller and more efficient AI models dedicated to this purpose are available, such as Bespoke’s Minicheck, which helps reduce the cost of performing these checks compared to flagship models like GPT-4. Minicheck is also available for use through Ollama.

The FactCheckingEvaluator constructor takes a ChatClient.Builder as a parameter:

The evaluator uses the following prompt template for fact-checking:

Where {document} is the context information, and {claim} is the AI model’s response to be evaluated.

Here’s an example of how to use the FactCheckingEvaluator with an Ollama-based ChatModel, specifically the Bespoke-Minicheck model:

**Examples:**

Example 1 (java):
```java
@FunctionalInterface
public interface Evaluator {
    EvaluationResponse evaluate(EvaluationRequest evaluationRequest);
}
```

Example 2 (java):
```java
public class EvaluationRequest {

	private final String userText;

	private final List<Content> dataList;

	private final String responseContent;

	public EvaluationRequest(String userText, List<Content> dataList, String responseContent) {
		this.userText = userText;
		this.dataList = dataList;
		this.responseContent = responseContent;
	}

  ...
}
```

Example 3 (json):
```json
Your task is to evaluate if the response for the query
is in line with the context information provided.

You have two options to answer. Either YES or NO.

Answer YES, if the response for the query
is in line with context information otherwise NO.

Query:
{query}

Response:
{response}

Context:
{context}

Answer:
```

Example 4 (java):
```java
@Test
void evaluateRelevancy() {
    String question = "Where does the adventure of Anacletus and Birba take place?";

    RetrievalAugmentationAdvisor ragAdvisor = RetrievalAugmentationAdvisor.builder()
        .documentRetriever(VectorStoreDocumentRetriever.builder()
            .vectorStore(pgVectorStore)
            .build())
        .build();

    ChatResponse chatResponse = ChatClient.builder(chatModel).build()
        .prompt(question)
        .advisors(ragAdvisor)
        .call()
        .chatResponse();

    EvaluationRequest evaluationRequest = new EvaluationRequest(
        // The original user question
        question,
        // The retrieved context from the RAG flow
        chatResponse.getMetadata().get(RetrievalAugmentationAdvisor.DOCUMENT_CONTEXT),
        // The AI model's response
        chatResponse.getResult().getOutput().getText()
    );

    RelevancyEvaluator evaluator = new RelevancyEvaluator(ChatClient.builder(chatModel));

    EvaluationResponse evaluationResponse = evaluator.evaluate(evaluationRequest);

    assertThat(evaluationResponse.isPass()).isTrue();
}
```

---

## Structured Output Converter :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/structured-output-converter.html

**Contents:**
- Structured Output Converter
- Structured Output API
  - Available Converters
- Using Converters
  - Bean Output Converter
  - Property Ordering in Generated Schema
    - Generic Bean Types
  - Map Output Converter
  - List Output Converter
- Supported AI Models

For the latest snapshot version, please use Spring AI 1.1.2!

The ability of LLMs to produce structured outputs is important for downstream applications that rely on reliably parsing output values. Developers want to quickly turn results from an AI model into data types, such as JSON, XML or Java classes, that can be passed to other application functions and methods.

The Spring AI Structured Output Converters help to convert the LLM output into a structured format. As shown in the following diagram, this approach operates around the LLM text completion endpoint:

Generating structured outputs from Large Language Models (LLMs) using generic completion APIs requires careful handling of inputs and outputs. The structured output converter plays a crucial role before and after the LLM call, ensuring the desired output structure is achieved.

Before the LLM call, the converter appends format instructions to the prompt, providing explicit guidance to the models on generating the desired output structure. These instructions act as a blueprint, shaping the model’s response to conform to the specified format.

After the LLM call, the converter takes the model’s output text and transforms it into instances of the structured type. This conversion process involves parsing the raw text output and mapping it to the corresponding structured data representation, such as JSON, XML, or domain-specific data structures.

The StructuredOutputConverter interface allows you to obtain structured output, such as mapping the output to a Java class or an array of values from the text-based AI Model output. The interface definition is:

It combines the Spring Converter<String, T> interface and the FormatProvider interface

The following diagram shows the data flow when using the structured output API.

The FormatProvider supplies specific formatting guidelines to the AI Model, enabling it to produce text outputs that can be converted into the designated target type T using the Converter. Here is an example of such formatting instructions:

The format instructions are most often appended to the end of the user input using the PromptTemplate like this:

The Converter<String, T> is responsible to transform output text from the model into instances of the specified type T.

Currently, Spring AI provides AbstractConversionServiceOutputConverter, AbstractMessageOutputConverter, BeanOutputConverter, MapOutputConverter and ListOutputConverter implementations:

AbstractConversionServiceOutputConverter<T> - Offers a pre-configured GenericConversionService for transforming LLM output into the desired format. No default FormatProvider implementation is provided.

AbstractMessageOutputConverter<T> - Supplies a pre-configured MessageConverter for converting LLM output into the desired format. No default FormatProvider implementation is provided.

BeanOutputConverter<T> - Configured with a designated Java class (e.g., Bean) or a ParameterizedTypeReference, this converter employs a FormatProvider implementation that directs the AI Model to produce a JSON response compliant with a DRAFT_2020_12, JSON Schema derived from the specified Java class. Subsequently, it utilizes an ObjectMapper to deserialize the JSON output into a Java object instance of the target class.

MapOutputConverter - Extends the functionality of AbstractMessageOutputConverter with a FormatProvider implementation that guides the AI Model to generate an RFC8259 compliant JSON response. Additionally, it incorporates a converter implementation that utilizes the provided MessageConverter to translate the JSON payload into a java.util.Map<String, Object> instance.

ListOutputConverter - Extends the AbstractConversionServiceOutputConverter and includes a FormatProvider implementation tailored for comma-delimited list output. The converter implementation employs the provided ConversionService to transform the model text output into a java.util.List.

The following sections provide guides how to use the available converters to generate structured outputs.

The following example shows how to use BeanOutputConverter to generate the filmography for an actor.

The target record representing actor’s filmography:

Here is how to apply the BeanOutputConverter using the high-level, fluent ChatClient API:

or using the low-level ChatModel API directly:

The BeanOutputConverter supports custom property ordering in the generated JSON schema through the @JsonPropertyOrder annotation. This annotation allows you to specify the exact sequence in which properties should appear in the schema, regardless of their declaration order in the class or record.

For example, to ensure specific ordering of properties in the ActorsFilms record:

This annotation works with both records and regular Java classes.

Use the ParameterizedTypeReference constructor to specify a more complex target class structure. For example, to represent a list of actors and their filmographies:

or using the low-level ChatModel API directly:

The following snippet shows how to use MapOutputConverter to convert the model output to a list of numbers in a map.

or using the low-level ChatModel API directly:

The following snippet shows how to use ListOutputConverter to convert the model output into a list of ice cream flavors.

or using the low-level ChatModel API directly:

The following AI Models have been tested to support List, Map and Bean structured outputs.

Integration Tests / Samples

AnthropicChatModelIT.java

AzureOpenAiChatModelIT.java

MistralAiChatModelIT.java

OllamaChatModelIT.java

VertexAiGeminiChatModelIT.java

Some AI Models provide dedicated configuration options to generate structured (usually JSON) output.

OpenAI Structured Outputs can ensure your model generates responses conforming strictly to your provided JSON Schema. You can choose between the JSON_OBJECT that guarantees the message the model generates is valid JSON or JSON_SCHEMA with a supplied schema that guarantees the model will generate a response that matches your supplied schema (spring.ai.openai.chat.options.responseFormat option).

Azure OpenAI - provides a spring.ai.azure.openai.chat.options.responseFormat options specifying the format that the model must output. Setting to { "type": "json_object" } enables JSON mode, which guarantees the message the model generates is valid JSON.

Ollama - provides a spring.ai.ollama.chat.options.format option to specify the format to return a response in. Currently, the only accepted value is json.

Mistral AI - provides a spring.ai.mistralai.chat.options.responseFormat option to specify the format to return a response in. Setting it to { "type": "json_object" } enables JSON mode, which guarantees the message the model generates is valid JSON.

**Examples:**

Example 1 (java):
```java
public interface StructuredOutputConverter<T> extends Converter<String, T>, FormatProvider {

}
```

Example 2 (java):
```java
public interface FormatProvider {
	String getFormat();
}
```

Example 3 (java):
```java
StructuredOutputConverter outputConverter = ...
    String userInputTemplate = """
        ... user text input ....
        {format}
        """; // user input with a "format" placeholder.
    Prompt prompt = new Prompt(
            PromptTemplate.builder()
						.template(this.userInputTemplate)
						.variables(Map.of(..., "format", this.outputConverter.getFormat())) // replace the "format" placeholder with the converter's format.
						.build().createMessage()
    );
```

Example 4 (java):
```java
record ActorsFilms(String actor, List<String> movies) {
}
```

---

## Structured Output Converter :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/structured-output-converter.html

**Contents:**
- Structured Output Converter
- Structured Output API
  - Available Converters
- Using Converters
  - Bean Output Converter
  - Property Ordering in Generated Schema
    - Generic Bean Types
  - Map Output Converter
  - List Output Converter
- Native Structured Output

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The ability of LLMs to produce structured outputs is important for downstream applications that rely on reliably parsing output values. Developers want to quickly turn results from an AI model into data types, such as JSON, XML or Java classes, that can be passed to other application functions and methods.

The Spring AI Structured Output Converters help to convert the LLM output into a structured format. As shown in the following diagram, this approach operates around the LLM text completion endpoint:

Generating structured outputs from Large Language Models (LLMs) using generic completion APIs requires careful handling of inputs and outputs. The structured output converter plays a crucial role before and after the LLM call, ensuring the desired output structure is achieved.

Before the LLM call, the converter appends format instructions to the prompt, providing explicit guidance to the models on generating the desired output structure. These instructions act as a blueprint, shaping the model’s response to conform to the specified format.

After the LLM call, the converter takes the model’s output text and transforms it into instances of the structured type. This conversion process involves parsing the raw text output and mapping it to the corresponding structured data representation, such as JSON, XML, or domain-specific data structures.

The StructuredOutputConverter interface allows you to obtain structured output, such as mapping the output to a Java class or an array of values from the text-based AI Model output. The interface definition is:

It combines the Spring Converter<String, T> interface and the FormatProvider interface

The following diagram shows the data flow when using the structured output API.

The FormatProvider supplies specific formatting guidelines to the AI Model, enabling it to produce text outputs that can be converted into the designated target type T using the Converter. Here is an example of such formatting instructions:

The format instructions are most often appended to the end of the user input using the PromptTemplate like this:

The Converter<String, T> is responsible to transform output text from the model into instances of the specified type T.

Currently, Spring AI provides AbstractConversionServiceOutputConverter, AbstractMessageOutputConverter, BeanOutputConverter, MapOutputConverter and ListOutputConverter implementations:

AbstractConversionServiceOutputConverter<T> - Offers a pre-configured GenericConversionService for transforming LLM output into the desired format. No default FormatProvider implementation is provided.

AbstractMessageOutputConverter<T> - Supplies a pre-configured MessageConverter for converting LLM output into the desired format. No default FormatProvider implementation is provided.

BeanOutputConverter<T> - Configured with a designated Java class (e.g., Bean) or a ParameterizedTypeReference, this converter employs a FormatProvider implementation that directs the AI Model to produce a JSON response compliant with a DRAFT_2020_12, JSON Schema derived from the specified Java class. Subsequently, it utilizes an ObjectMapper to deserialize the JSON output into a Java object instance of the target class.

MapOutputConverter - Extends the functionality of AbstractMessageOutputConverter with a FormatProvider implementation that guides the AI Model to generate an RFC8259 compliant JSON response. Additionally, it incorporates a converter implementation that utilizes the provided MessageConverter to translate the JSON payload into a java.util.Map<String, Object> instance.

ListOutputConverter - Extends the AbstractConversionServiceOutputConverter and includes a FormatProvider implementation tailored for comma-delimited list output. The converter implementation employs the provided ConversionService to transform the model text output into a java.util.List.

The following sections provide guides how to use the available converters to generate structured outputs.

The following example shows how to use BeanOutputConverter to generate the filmography for an actor.

The target record representing actor’s filmography:

Here is how to apply the BeanOutputConverter using the high-level, fluent ChatClient API:

or using the low-level ChatModel API directly:

The BeanOutputConverter supports custom property ordering in the generated JSON schema through the @JsonPropertyOrder annotation. This annotation allows you to specify the exact sequence in which properties should appear in the schema, regardless of their declaration order in the class or record.

For example, to ensure specific ordering of properties in the ActorsFilms record:

This annotation works with both records and regular Java classes.

Use the ParameterizedTypeReference constructor to specify a more complex target class structure. For example, to represent a list of actors and their filmographies:

or using the low-level ChatModel API directly:

The following snippet shows how to use MapOutputConverter to convert the model output to a list of numbers in a map.

or using the low-level ChatModel API directly:

The following snippet shows how to use ListOutputConverter to convert the model output into a list of ice cream flavors.

or using the low-level ChatModel API directly:

Many modern AI models now provide native support for structured output, which offers more reliable results compared to prompt-based formatting. Spring AI supports this through the Native Structured Output feature.

When using native structured output, the JSON schema generated by BeanOutputConverter is sent directly to the model’s structured output API, eliminating the need for format instructions in the prompt. This approach provides:

Higher reliability: The model guarantees output conforming to the schema

Cleaner prompts: No need to append format instructions

Better performance: Models can optimize for structured output internally

To enable native structured output, use the AdvisorParams.ENABLE_NATIVE_STRUCTURED_OUTPUT parameter:

You can also set this globally using defaultAdvisors() on the ChatClient.Builder:

The following models currently support native structured output:

OpenAI: GPT-4o and later models with JSON Schema support

Anthropic: Claude 3.5 Sonnet and later models

Vertex AI Gemini: Gemini 1.5 Pro and later models

Some AI Models provide dedicated configuration options to generate structured (usually JSON) output.

OpenAI Structured Outputs can ensure your model generates responses conforming strictly to your provided JSON Schema. You can choose between the JSON_OBJECT that guarantees the message the model generates is valid JSON or JSON_SCHEMA with a supplied schema that guarantees the model will generate a response that matches your supplied schema (spring.ai.openai.chat.options.responseFormat option).

Azure OpenAI - provides a spring.ai.azure.openai.chat.options.responseFormat options specifying the format that the model must output. Setting to { "type": "json_object" } enables JSON mode, which guarantees the message the model generates is valid JSON.

Ollama - provides a spring.ai.ollama.chat.options.format option to specify the format to return a response in. Currently, the only accepted value is json.

Mistral AI - provides a spring.ai.mistralai.chat.options.responseFormat option to specify the format to return a response in. Setting it to { "type": "json_object" } enables JSON mode, which guarantees the message the model generates is valid JSON.

**Examples:**

Example 1 (java):
```java
public interface StructuredOutputConverter<T> extends Converter<String, T>, FormatProvider {

}
```

Example 2 (java):
```java
public interface FormatProvider {
	String getFormat();
}
```

Example 3 (java):
```java
StructuredOutputConverter outputConverter = ...
    String userInputTemplate = """
        ... user text input ....
        {format}
        """; // user input with a "format" placeholder.
    Prompt prompt = new Prompt(
            PromptTemplate.builder()
						.template(this.userInputTemplate)
						.variables(Map.of(..., "format", this.outputConverter.getFormat())) // replace the "format" placeholder with the converter's format.
						.build().createMessage()
    );
```

Example 4 (java):
```java
record ActorsFilms(String actor, List<String> movies) {
}
```

---

## Cloud Bindings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/cloud-bindings.html

**Contents:**
- Cloud Bindings
- Available Cloud Bindings

Spring AI provides support for cloud bindings based on the foundations in spring-cloud-bindings. This allows applications to specify a binding type for a provider and then express properties using a generic format. The spring-ai cloud bindings will process these properties and bind them to spring-ai native properties.

For example, when using OpenAi, the binding type is openai. Using the property spring.ai.cloud.bindings.openai.enabled, the binding processor can be enabled or disabled. By default, when specifying a binding type, this property will be enabled. Configuration for api-key, uri, username, password, etc. can be specified and spring-ai will map them to the corresponding properties in the supported system.

To enable cloud binding support, include the following dependency in the application.

or to your Gradle build.gradle build file.

The following are the components for which the cloud binding support is currently available in the spring-ai-spring-cloud-bindings module:

uri, username, password

spring.ai.vectorstore.chroma.client.host, spring.ai.vectorstore.chroma.client.port, spring.ai.vectorstore.chroma.client.username, spring.ai.vectorstore.chroma.client.host.password

spring.ai.mistralai.api-key, spring.ai.mistralai.base-url

spring.ai.ollama.base-url

spring.ai.openai.api-key, spring.ai.openai.base-url

spring.ai.vectorstore.weaviate.scheme, spring.ai.vectorstore.weaviate.host, spring.ai.vectorstore.weaviate.api-key

uri, api-key, model-capabilities (chat and embedding), model-name

spring.ai.openai.chat.base-url, spring.ai.openai.chat.api-key, spring.ai.openai.chat.options.model, spring.ai.openai.embedding.base-url, spring.ai.openai.embedding.api-key, spring.ai.openai.embedding.options.model

**Examples:**

Example 1 (xml):
```xml
<dependency>
   <groupId>org.springframework.ai</groupId>
   <artifactId>spring-ai-spring-cloud-bindings</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-spring-cloud-bindings'
}
```

---
