# Springai - Image Models

**Pages:** 12

---

## QianFan Image :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/image/qianfan-image.html

**Contents:**
- QianFan Image

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This functionality has been moved to the Spring AI Community repository.

Please visit github.com/spring-ai-community/qianfan for the latest version.

---

## Stability AI Image Generation :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/image/stabilityai-image.html

**Contents:**
- Stability AI Image Generation
- Prerequisites
- Auto-configuration
  - Image Generation Properties
- Runtime Options

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI supports Stability AI’s text to image generation model.

You will need to create an API key with Stability AI to access their AI models. Follow their Getting Started documentation to obtain your API key.

The Spring AI project defines a configuration property named spring.ai.stabilityai.api-key that you should set to the value of the API Key obtained from Stability AI.

You can set this configuration property in your application.properties file:

For enhanced security when handling sensitive information like API keys, you can use Spring Expression Language (SpEL) to reference a custom environment variable:

You can also set this configuration programmatically in your application code:

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Stability AI Image Generation Client. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The prefix spring.ai.stabilityai is used as the property prefix that lets you connect to Stability AI.

spring.ai.stabilityai.base-url

The URL to connect to

spring.ai.stabilityai.api-key

Enabling and disabling of the image auto-configurations are now configured via top level properties with the prefix spring.ai.model.image.

To enable, spring.ai.model.image=stabilityai (It is enabled by default)

To disable, spring.ai.model.image=none (or any value which doesn’t match stabilityai)

This change is done to allow configuration of multiple models.

The prefix spring.ai.stabilityai.image is the property prefix that lets you configure the ImageModel implementation for Stability AI.

spring.ai.stabilityai.image.enabled (Removed and no longer valid)

Enable Stability AI image model.

spring.ai.model.image

Enable Stability AI image model.

spring.ai.stabilityai.image.base-url

Optional overrides the spring.ai.openai.base-url to provide a specific url

https://api.stability.ai/v1

spring.ai.stabilityai.image.api-key

Optional overrides the spring.ai.openai.api-key to provide a specific api-key

spring.ai.stabilityai.image.option.n

The number of images to be generated. Must be between 1 and 10.

spring.ai.stabilityai.image.option.model

The engine/model to use in Stability AI. The model is passed in the URL as a path parameter.

stable-diffusion-v1-6

spring.ai.stabilityai.image.option.width

Width of the image to generate, in pixels, in an increment divisible by 64. Engine-specific dimension validation applies.

spring.ai.stabilityai.image.option.height

Height of the image to generate, in pixels, in an increment divisible by 64. Engine-specific dimension validation applies.

spring.ai.stabilityai.image.option.responseFormat

The format in which the generated images are returned. Must be "application/json" or "image/png".

spring.ai.stabilityai.image.option.cfg_scale

The strictness level of the diffusion process adherence to the prompt text. Range: 0 to 35.

spring.ai.stabilityai.image.option.clip_guidance_preset

Pass in a style preset to guide the image model towards a particular style. This list of style presets is subject to change.

spring.ai.stabilityai.image.option.sampler

Which sampler to use for the diffusion process. If this value is omitted, an appropriate sampler will be automatically selected.

spring.ai.stabilityai.image.option.seed

Random noise seed (omit this option or use 0 for a random seed). Valid range: 0 to 4294967295.

spring.ai.stabilityai.image.option.steps

Number of diffusion steps to run. Valid range: 10 to 50.

spring.ai.stabilityai.image.option.style_preset

Pass in a style preset to guide the image model towards a particular style. This list of style presets is subject to change.

The StabilityAiImageOptions.java provides model configurations, such as the model to use, the style, the size, etc.

On start-up, the default options can be configured with the StabilityAiImageModel(StabilityAiApi stabilityAiApi, StabilityAiImageOptions options) constructor. Alternatively, use the spring.ai.openai.image.options.* properties described previously.

At runtime, you can override the default options by adding new, request specific, options to the ImagePrompt call. For example to override the Stability AI specific options such as quality and the number of images to create, use the following code example:

**Examples:**

Example 1 (unknown):
```unknown
spring.ai.stabilityai.api-key=<your-stabilityai-api-key>
```

Example 2 (yaml):
```yaml
# In application.yml
spring:
  ai:
    stabilityai:
      api-key: ${STABILITYAI_API_KEY}
```

Example 3 (bash):
```bash
# In your environment or .env file
export STABILITYAI_API_KEY=<your-stabilityai-api-key>
```

Example 4 (java):
```java
// Retrieve API key from a secure source or environment variable
String apiKey = System.getenv("STABILITYAI_API_KEY");
```

---

## ZhiPuAI Image Generation :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/image/zhipuai-image.html

**Contents:**
- ZhiPuAI Image Generation
- Prerequisites
  - Add Repositories and BOM
- Auto-configuration
  - Image Generation Properties
    - Connection Properties
    - Configuration Properties
    - Retry Properties
- Runtime Options

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI supports CogView, the Image generation model from ZhiPuAI.

You will need to create an API with ZhiPuAI to access ZhiPu AI language models.

Create an account at ZhiPu AI registration page and generate the token on the API Keys page.

The Spring AI project defines a configuration property named spring.ai.zhipuai.api-key that you should set to the value of the API Key obtained from the API Keys page.

You can set this configuration property in your application.properties file:

For enhanced security when handling sensitive information like API keys, you can use Spring Expression Language (SpEL) to reference a custom environment variable:

You can also set this configuration programmatically in your application code:

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the ZhiPuAI Chat Client. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Enabling and disabling of the image auto-configurations are now configured via top level properties with the prefix spring.ai.model.image.

To enable, spring.ai.model.image=stabilityai (It is enabled by default)

To disable, spring.ai.model.image=none (or any value which doesn’t match stabilityai)

This change is done to allow configuration of multiple models.

The prefix spring.ai.zhipuai.image is the property prefix that lets you configure the ImageModel implementation for ZhiPuAI.

spring.ai.zhipuai.image.enabled (Removed and no longer valid)

Enable ZhiPuAI image model.

spring.ai.model.image

Enable ZhiPuAI image model.

spring.ai.zhipuai.image.base-url

Optional overrides the spring.ai.zhipuai.base-url to provide chat specific url

spring.ai.zhipuai.image.api-key

Optional overrides the spring.ai.zhipuai.api-key to provide chat specific api-key

spring.ai.zhipuai.image.options.model

The model to use for image generation.

spring.ai.zhipuai.image.options.user

A unique identifier representing your end-user, which can help ZhiPuAI to monitor and detect abuse.

The prefix spring.ai.zhipuai is used as the property prefix that lets you connect to ZhiPuAI.

spring.ai.zhipuai.base-url

The URL to connect to

open.bigmodel.cn/api/paas

spring.ai.zhipuai.api-key

The prefix spring.ai.retry is used as the property prefix that lets you configure the retry mechanism for the ZhiPuAI Image client.

spring.ai.retry.max-attempts

Maximum number of retry attempts.

spring.ai.retry.backoff.initial-interval

Initial sleep duration for the exponential backoff policy.

spring.ai.retry.backoff.multiplier

Backoff interval multiplier.

spring.ai.retry.backoff.max-interval

Maximum backoff duration.

spring.ai.retry.on-client-errors

If false, throw a NonTransientAiException, and do not attempt retry for 4xx client error codes

spring.ai.retry.exclude-on-http-codes

List of HTTP status codes that should not trigger a retry (e.g. to throw NonTransientAiException).

spring.ai.retry.on-http-codes

List of HTTP status codes that should trigger a retry (e.g. to throw TransientAiException).

The ZhiPuAiImageOptions.java provides model configurations, such as the model to use, the quality, the size, etc.

On start-up, the default options can be configured with the ZhiPuAiImageModel(ZhiPuAiImageApi zhiPuAiImageApi) constructor and the withDefaultOptions(ZhiPuAiImageOptions defaultOptions) method. Alternatively, use the spring.ai.zhipuai.image.options.* properties described previously.

At runtime you can override the default options by adding new, request specific, options to the ImagePrompt call. For example to override the ZhiPuAI specific options such as quality and the number of images to create, use the following code example:

**Examples:**

Example 1 (unknown):
```unknown
spring.ai.zhipuai.api-key=<your-zhipuai-api-key>
```

Example 2 (yaml):
```yaml
# In application.yml
spring:
  ai:
    zhipuai:
      api-key: ${ZHIPUAI_API_KEY}
```

Example 3 (bash):
```bash
# In your environment or .env file
export ZHIPUAI_API_KEY=<your-zhipuai-api-key>
```

Example 4 (java):
```java
// Retrieve API key from a secure source or environment variable
String apiKey = System.getenv("ZHIPUAI_API_KEY");
```

---

## ZhiPuAI Image Generation :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/image/zhipuai-image.html

**Contents:**
- ZhiPuAI Image Generation
- Prerequisites
  - Add Repositories and BOM
- Auto-configuration
  - Image Generation Properties
    - Connection Properties
    - Configuration Properties
    - Retry Properties
- Runtime Options

For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI supports CogView, the Image generation model from ZhiPuAI.

You will need to create an API with ZhiPuAI to access ZhiPu AI language models.

Create an account at ZhiPu AI registration page and generate the token on the API Keys page.

The Spring AI project defines a configuration property named spring.ai.zhipuai.api-key that you should set to the value of the API Key obtained from the API Keys page.

You can set this configuration property in your application.properties file:

For enhanced security when handling sensitive information like API keys, you can use Spring Expression Language (SpEL) to reference a custom environment variable:

You can also set this configuration programmatically in your application code:

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the ZhiPuAI Chat Client. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Enabling and disabling of the image auto-configurations are now configured via top level properties with the prefix spring.ai.model.image.

To enable, spring.ai.model.image=stabilityai (It is enabled by default)

To disable, spring.ai.model.image=none (or any value which doesn’t match stabilityai)

This change is done to allow configuration of multiple models.

The prefix spring.ai.zhipuai.image is the property prefix that lets you configure the ImageModel implementation for ZhiPuAI.

spring.ai.zhipuai.image.enabled (Removed and no longer valid)

Enable ZhiPuAI image model.

spring.ai.model.image

Enable ZhiPuAI image model.

spring.ai.zhipuai.image.base-url

Optional overrides the spring.ai.zhipuai.base-url to provide chat specific url

spring.ai.zhipuai.image.api-key

Optional overrides the spring.ai.zhipuai.api-key to provide chat specific api-key

spring.ai.zhipuai.image.options.model

The model to use for image generation.

spring.ai.zhipuai.image.options.user

A unique identifier representing your end-user, which can help ZhiPuAI to monitor and detect abuse.

The prefix spring.ai.zhipuai is used as the property prefix that lets you connect to ZhiPuAI.

spring.ai.zhipuai.base-url

The URL to connect to

open.bigmodel.cn/api/paas

spring.ai.zhipuai.api-key

The prefix spring.ai.retry is used as the property prefix that lets you configure the retry mechanism for the ZhiPuAI Image client.

spring.ai.retry.max-attempts

Maximum number of retry attempts.

spring.ai.retry.backoff.initial-interval

Initial sleep duration for the exponential backoff policy.

spring.ai.retry.backoff.multiplier

Backoff interval multiplier.

spring.ai.retry.backoff.max-interval

Maximum backoff duration.

spring.ai.retry.on-client-errors

If false, throw a NonTransientAiException, and do not attempt retry for 4xx client error codes

spring.ai.retry.exclude-on-http-codes

List of HTTP status codes that should not trigger a retry (e.g. to throw NonTransientAiException).

spring.ai.retry.on-http-codes

List of HTTP status codes that should trigger a retry (e.g. to throw TransientAiException).

The ZhiPuAiImageOptions.java provides model configurations, such as the model to use, the quality, the size, etc.

On start-up, the default options can be configured with the ZhiPuAiImageModel(ZhiPuAiImageApi zhiPuAiImageApi) constructor and the withDefaultOptions(ZhiPuAiImageOptions defaultOptions) method. Alternatively, use the spring.ai.zhipuai.image.options.* properties described previously.

At runtime you can override the default options by adding new, request specific, options to the ImagePrompt call. For example to override the ZhiPuAI specific options such as quality and the number of images to create, use the following code example:

**Examples:**

Example 1 (unknown):
```unknown
spring.ai.zhipuai.api-key=<your-zhipuai-api-key>
```

Example 2 (yaml):
```yaml
# In application.yml
spring:
  ai:
    zhipuai:
      api-key: ${ZHIPUAI_API_KEY}
```

Example 3 (bash):
```bash
# In your environment or .env file
export ZHIPUAI_API_KEY=<your-zhipuai-api-key>
```

Example 4 (java):
```java
// Retrieve API key from a secure source or environment variable
String apiKey = System.getenv("ZHIPUAI_API_KEY");
```

---

## Image Model API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/imageclient.html

**Contents:**
- Image Model API
- API Overview
- Image Model
  - ImagePrompt
    - ImageMessage
    - ImageOptions
  - ImageResponse
  - ImageGeneration
- Available Implementations
- API Docs

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The Spring Image Model API is designed to be a simple and portable interface for interacting with various AI Models specialized in image generation, allowing developers to switch between different image-related models with minimal code changes. This design aligns with Spring’s philosophy of modularity and interchangeability, ensuring developers can quickly adapt their applications to different AI capabilities related to image processing.

Additionally, with the support of companion classes like ImagePrompt for input encapsulation and ImageResponse for output handling, the Image Model API unifies the communication with AI Models dedicated to image generation. It manages the complexity of request preparation and response parsing, offering a direct and simplified API interaction for image-generation functionalities.

The Spring Image Model API is built on top of the Spring AI Generic Model API, providing image-specific abstractions and implementations.

This section provides a guide to the Spring Image Model API interface and associated classes.

Here is the ImageModel interface definition:

The ImagePrompt is a ModelRequest that encapsulates a list of ImageMessage objects and optional model request options. The following listing shows a truncated version of the ImagePrompt class, excluding constructors and other utility methods:

The ImageMessage class encapsulates the text to use and the weight that the text should have in influencing the generated image. For models that support weights, they can be positive or negative.

Represents the options that can be passed to the Image generation model. The ImageOptions interface extends the ModelOptions interface and is used to define few portable options that can be passed to the AI model.

The ImageOptions interface is defined as follows:

Additionally, every model specific ImageModel implementation can have its own options that can be passed to the AI model. For example, the OpenAI Image Generation model has its own options like quality, style, etc.

This is a powerful feature that allows developers to use model specific options when starting the application and then override them at runtime using the ImagePrompt.

The structure of the ImageResponse class is as follows:

The ImageResponse class holds the AI Model’s output, with each ImageGeneration instance containing one of potentially multiple outputs resulting from a single prompt.

The ImageResponse class also carries a ImageResponseMetadata object holding metadata about the AI Model’s response.

Finally, the ImageGeneration class extends from the ModelResult to represent the output response and related metadata about this result:

ImageModel implementations are provided for the following Model providers:

OpenAI Image Generation

Azure OpenAI Image Generation

QianFan Image Generation

StabilityAI Image Generation

ZhiPuAI Image Generation

You can find the Javadoc here.

The project’s GitHub discussions is a great place to send feedback.

**Examples:**

Example 1 (java):
```java
@FunctionalInterface
public interface ImageModel extends Model<ImagePrompt, ImageResponse> {

	ImageResponse call(ImagePrompt request);

}
```

Example 2 (java):
```java
public class ImagePrompt implements ModelRequest<List<ImageMessage>> {

    private final List<ImageMessage> messages;

	private ImageOptions imageModelOptions;

    @Override
	public List<ImageMessage> getInstructions() {...}

	@Override
	public ImageOptions getOptions() {...}

    // constructors and utility methods omitted
}
```

Example 3 (java):
```java
public class ImageMessage {

	private String text;

	private Float weight;

    public String getText() {...}

	public Float getWeight() {...}

   // constructors and utility methods omitted
}
```

Example 4 (java):
```java
public interface ImageOptions extends ModelOptions {

	Integer getN();

	String getModel();

	Integer getWidth();

	Integer getHeight();

	String getResponseFormat(); // openai - url or base64 : stability ai byte[] or base64

}
```

---

## Stability AI Image Generation :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/image/stabilityai-image.html

**Contents:**
- Stability AI Image Generation
- Prerequisites
- Auto-configuration
  - Image Generation Properties
- Runtime Options

For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI supports Stability AI’s text to image generation model.

You will need to create an API key with Stability AI to access their AI models. Follow their Getting Started documentation to obtain your API key.

The Spring AI project defines a configuration property named spring.ai.stabilityai.api-key that you should set to the value of the API Key obtained from Stability AI.

You can set this configuration property in your application.properties file:

For enhanced security when handling sensitive information like API keys, you can use Spring Expression Language (SpEL) to reference a custom environment variable:

You can also set this configuration programmatically in your application code:

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Stability AI Image Generation Client. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The prefix spring.ai.stabilityai is used as the property prefix that lets you connect to Stability AI.

spring.ai.stabilityai.base-url

The URL to connect to

spring.ai.stabilityai.api-key

Enabling and disabling of the image auto-configurations are now configured via top level properties with the prefix spring.ai.model.image.

To enable, spring.ai.model.image=stabilityai (It is enabled by default)

To disable, spring.ai.model.image=none (or any value which doesn’t match stabilityai)

This change is done to allow configuration of multiple models.

The prefix spring.ai.stabilityai.image is the property prefix that lets you configure the ImageModel implementation for Stability AI.

spring.ai.stabilityai.image.enabled (Removed and no longer valid)

Enable Stability AI image model.

spring.ai.model.image

Enable Stability AI image model.

spring.ai.stabilityai.image.base-url

Optional overrides the spring.ai.openai.base-url to provide a specific url

https://api.stability.ai/v1

spring.ai.stabilityai.image.api-key

Optional overrides the spring.ai.openai.api-key to provide a specific api-key

spring.ai.stabilityai.image.option.n

The number of images to be generated. Must be between 1 and 10.

spring.ai.stabilityai.image.option.model

The engine/model to use in Stability AI. The model is passed in the URL as a path parameter.

stable-diffusion-v1-6

spring.ai.stabilityai.image.option.width

Width of the image to generate, in pixels, in an increment divisible by 64. Engine-specific dimension validation applies.

spring.ai.stabilityai.image.option.height

Height of the image to generate, in pixels, in an increment divisible by 64. Engine-specific dimension validation applies.

spring.ai.stabilityai.image.option.responseFormat

The format in which the generated images are returned. Must be "application/json" or "image/png".

spring.ai.stabilityai.image.option.cfg_scale

The strictness level of the diffusion process adherence to the prompt text. Range: 0 to 35.

spring.ai.stabilityai.image.option.clip_guidance_preset

Pass in a style preset to guide the image model towards a particular style. This list of style presets is subject to change.

spring.ai.stabilityai.image.option.sampler

Which sampler to use for the diffusion process. If this value is omitted, an appropriate sampler will be automatically selected.

spring.ai.stabilityai.image.option.seed

Random noise seed (omit this option or use 0 for a random seed). Valid range: 0 to 4294967295.

spring.ai.stabilityai.image.option.steps

Number of diffusion steps to run. Valid range: 10 to 50.

spring.ai.stabilityai.image.option.style_preset

Pass in a style preset to guide the image model towards a particular style. This list of style presets is subject to change.

The StabilityAiImageOptions.java provides model configurations, such as the model to use, the style, the size, etc.

On start-up, the default options can be configured with the StabilityAiImageModel(StabilityAiApi stabilityAiApi, StabilityAiImageOptions options) constructor. Alternatively, use the spring.ai.openai.image.options.* properties described previously.

At runtime, you can override the default options by adding new, request specific, options to the ImagePrompt call. For example to override the Stability AI specific options such as quality and the number of images to create, use the following code example:

**Examples:**

Example 1 (unknown):
```unknown
spring.ai.stabilityai.api-key=<your-stabilityai-api-key>
```

Example 2 (yaml):
```yaml
# In application.yml
spring:
  ai:
    stabilityai:
      api-key: ${STABILITYAI_API_KEY}
```

Example 3 (bash):
```bash
# In your environment or .env file
export STABILITYAI_API_KEY=<your-stabilityai-api-key>
```

Example 4 (java):
```java
// Retrieve API key from a secure source or environment variable
String apiKey = System.getenv("STABILITYAI_API_KEY");
```

---

## QianFan Image :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/image/qianfan-image.html

**Contents:**
- QianFan Image

For the latest snapshot version, please use Spring AI 1.1.2!

This functionality has been moved to the Spring AI Community repository.

Please visit github.com/spring-ai-community/qianfan for the latest version.

---

## Image Model API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/imageclient.html

**Contents:**
- Image Model API
- API Overview
- Image Model
  - ImagePrompt
    - ImageMessage
    - ImageOptions
  - ImageResponse
  - ImageGeneration
- Available Implementations
- API Docs

For the latest snapshot version, please use Spring AI 1.1.2!

The Spring Image Model API is designed to be a simple and portable interface for interacting with various AI Models specialized in image generation, allowing developers to switch between different image-related models with minimal code changes. This design aligns with Spring’s philosophy of modularity and interchangeability, ensuring developers can quickly adapt their applications to different AI capabilities related to image processing.

Additionally, with the support of companion classes like ImagePrompt for input encapsulation and ImageResponse for output handling, the Image Model API unifies the communication with AI Models dedicated to image generation. It manages the complexity of request preparation and response parsing, offering a direct and simplified API interaction for image-generation functionalities.

The Spring Image Model API is built on top of the Spring AI Generic Model API, providing image-specific abstractions and implementations.

This section provides a guide to the Spring Image Model API interface and associated classes.

Here is the ImageModel interface definition:

The ImagePrompt is a ModelRequest that encapsulates a list of ImageMessage objects and optional model request options. The following listing shows a truncated version of the ImagePrompt class, excluding constructors and other utility methods:

The ImageMessage class encapsulates the text to use and the weight that the text should have in influencing the generated image. For models that support weights, they can be positive or negative.

Represents the options that can be passed to the Image generation model. The ImageOptions interface extends the ModelOptions interface and is used to define few portable options that can be passed to the AI model.

The ImageOptions interface is defined as follows:

Additionally, every model specific ImageModel implementation can have its own options that can be passed to the AI model. For example, the OpenAI Image Generation model has its own options like quality, style, etc.

This is a powerful feature that allows developers to use model specific options when starting the application and then override them at runtime using the ImagePrompt.

The structure of the ImageResponse class is as follows:

The ImageResponse class holds the AI Model’s output, with each ImageGeneration instance containing one of potentially multiple outputs resulting from a single prompt.

The ImageResponse class also carries a ImageResponseMetadata object holding metadata about the AI Model’s response.

Finally, the ImageGeneration class extends from the ModelResult to represent the output response and related metadata about this result:

ImageModel implementations are provided for the following Model providers:

OpenAI Image Generation

Azure OpenAI Image Generation

QianFan Image Generation

StabilityAI Image Generation

ZhiPuAI Image Generation

You can find the Javadoc here.

The project’s GitHub discussions is a great place to send feedback.

**Examples:**

Example 1 (java):
```java
@FunctionalInterface
public interface ImageModel extends Model<ImagePrompt, ImageResponse> {

	ImageResponse call(ImagePrompt request);

}
```

Example 2 (java):
```java
public class ImagePrompt implements ModelRequest<List<ImageMessage>> {

    private final List<ImageMessage> messages;

	private ImageOptions imageModelOptions;

    @Override
	public List<ImageMessage> getInstructions() {...}

	@Override
	public ImageOptions getOptions() {...}

    // constructors and utility methods omitted
}
```

Example 3 (java):
```java
public class ImageMessage {

	private String text;

	private Float weight;

    public String getText() {...}

	public Float getWeight() {...}

   // constructors and utility methods omitted
}
```

Example 4 (java):
```java
public interface ImageOptions extends ModelOptions {

	Integer getN();

	String getModel();

	Integer getWidth();

	Integer getHeight();

	String getResponseFormat(); // openai - url or base64 : stability ai byte[] or base64

}
```

---

## Image Model API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/imageclient.html

**Contents:**
- Image Model API
- API Overview
- Image Model
  - ImagePrompt
    - ImageMessage
    - ImageOptions
  - ImageResponse
  - ImageGeneration
- Available Implementations
- API Docs

The Spring Image Model API is designed to be a simple and portable interface for interacting with various AI Models specialized in image generation, allowing developers to switch between different image-related models with minimal code changes. This design aligns with Spring’s philosophy of modularity and interchangeability, ensuring developers can quickly adapt their applications to different AI capabilities related to image processing.

Additionally, with the support of companion classes like ImagePrompt for input encapsulation and ImageResponse for output handling, the Image Model API unifies the communication with AI Models dedicated to image generation. It manages the complexity of request preparation and response parsing, offering a direct and simplified API interaction for image-generation functionalities.

The Spring Image Model API is built on top of the Spring AI Generic Model API, providing image-specific abstractions and implementations.

This section provides a guide to the Spring Image Model API interface and associated classes.

Here is the ImageModel interface definition:

The ImagePrompt is a ModelRequest that encapsulates a list of ImageMessage objects and optional model request options. The following listing shows a truncated version of the ImagePrompt class, excluding constructors and other utility methods:

The ImageMessage class encapsulates the text to use and the weight that the text should have in influencing the generated image. For models that support weights, they can be positive or negative.

Represents the options that can be passed to the Image generation model. The ImageOptions interface extends the ModelOptions interface and is used to define few portable options that can be passed to the AI model.

The ImageOptions interface is defined as follows:

Additionally, every model specific ImageModel implementation can have its own options that can be passed to the AI model. For example, the OpenAI Image Generation model has its own options like quality, style, etc.

This is a powerful feature that allows developers to use model specific options when starting the application and then override them at runtime using the ImagePrompt.

The structure of the ImageResponse class is as follows:

The ImageResponse class holds the AI Model’s output, with each ImageGeneration instance containing one of potentially multiple outputs resulting from a single prompt.

The ImageResponse class also carries a ImageResponseMetadata object holding metadata about the AI Model’s response.

Finally, the ImageGeneration class extends from the ModelResult to represent the output response and related metadata about this result:

ImageModel implementations are provided for the following Model providers:

OpenAI Image Generation

Azure OpenAI Image Generation

QianFan Image Generation

StabilityAI Image Generation

ZhiPuAI Image Generation

You can find the Javadoc here.

The project’s GitHub discussions is a great place to send feedback.

**Examples:**

Example 1 (java):
```java
@FunctionalInterface
public interface ImageModel extends Model<ImagePrompt, ImageResponse> {

	ImageResponse call(ImagePrompt request);

}
```

Example 2 (java):
```java
public class ImagePrompt implements ModelRequest<List<ImageMessage>> {

    private final List<ImageMessage> messages;

	private ImageOptions imageModelOptions;

    @Override
	public List<ImageMessage> getInstructions() {...}

	@Override
	public ImageOptions getOptions() {...}

    // constructors and utility methods omitted
}
```

Example 3 (java):
```java
public class ImageMessage {

	private String text;

	private Float weight;

    public String getText() {...}

	public Float getWeight() {...}

   // constructors and utility methods omitted
}
```

Example 4 (java):
```java
public interface ImageOptions extends ModelOptions {

	Integer getN();

	String getModel();

	Integer getWidth();

	Integer getHeight();

	String getResponseFormat(); // openai - url or base64 : stability ai byte[] or base64

}
```

---

## ZhiPuAI Image Generation :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/image/zhipuai-image.html

**Contents:**
- ZhiPuAI Image Generation
- Prerequisites
  - Add Repositories and BOM
- Auto-configuration
  - Image Generation Properties
    - Connection Properties
    - Configuration Properties
    - Retry Properties
- Runtime Options

Spring AI supports CogView, the Image generation model from ZhiPuAI.

You will need to create an API with ZhiPuAI to access ZhiPu AI language models.

Create an account at ZhiPu AI registration page and generate the token on the API Keys page.

The Spring AI project defines a configuration property named spring.ai.zhipuai.api-key that you should set to the value of the API Key obtained from the API Keys page.

You can set this configuration property in your application.properties file:

For enhanced security when handling sensitive information like API keys, you can use Spring Expression Language (SpEL) to reference a custom environment variable:

You can also set this configuration programmatically in your application code:

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the ZhiPuAI Chat Client. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Enabling and disabling of the image auto-configurations are now configured via top level properties with the prefix spring.ai.model.image.

To enable, spring.ai.model.image=stabilityai (It is enabled by default)

To disable, spring.ai.model.image=none (or any value which doesn’t match stabilityai)

This change is done to allow configuration of multiple models.

The prefix spring.ai.zhipuai.image is the property prefix that lets you configure the ImageModel implementation for ZhiPuAI.

spring.ai.zhipuai.image.enabled (Removed and no longer valid)

Enable ZhiPuAI image model.

spring.ai.model.image

Enable ZhiPuAI image model.

spring.ai.zhipuai.image.base-url

Optional overrides the spring.ai.zhipuai.base-url to provide chat specific url

spring.ai.zhipuai.image.api-key

Optional overrides the spring.ai.zhipuai.api-key to provide chat specific api-key

spring.ai.zhipuai.image.options.model

The model to use for image generation.

spring.ai.zhipuai.image.options.user

A unique identifier representing your end-user, which can help ZhiPuAI to monitor and detect abuse.

The prefix spring.ai.zhipuai is used as the property prefix that lets you connect to ZhiPuAI.

spring.ai.zhipuai.base-url

The URL to connect to

open.bigmodel.cn/api/paas

spring.ai.zhipuai.api-key

The prefix spring.ai.retry is used as the property prefix that lets you configure the retry mechanism for the ZhiPuAI Image client.

spring.ai.retry.max-attempts

Maximum number of retry attempts.

spring.ai.retry.backoff.initial-interval

Initial sleep duration for the exponential backoff policy.

spring.ai.retry.backoff.multiplier

Backoff interval multiplier.

spring.ai.retry.backoff.max-interval

Maximum backoff duration.

spring.ai.retry.on-client-errors

If false, throw a NonTransientAiException, and do not attempt retry for 4xx client error codes

spring.ai.retry.exclude-on-http-codes

List of HTTP status codes that should not trigger a retry (e.g. to throw NonTransientAiException).

spring.ai.retry.on-http-codes

List of HTTP status codes that should trigger a retry (e.g. to throw TransientAiException).

The ZhiPuAiImageOptions.java provides model configurations, such as the model to use, the quality, the size, etc.

On start-up, the default options can be configured with the ZhiPuAiImageModel(ZhiPuAiImageApi zhiPuAiImageApi) constructor and the withDefaultOptions(ZhiPuAiImageOptions defaultOptions) method. Alternatively, use the spring.ai.zhipuai.image.options.* properties described previously.

At runtime you can override the default options by adding new, request specific, options to the ImagePrompt call. For example to override the ZhiPuAI specific options such as quality and the number of images to create, use the following code example:

**Examples:**

Example 1 (unknown):
```unknown
spring.ai.zhipuai.api-key=<your-zhipuai-api-key>
```

Example 2 (yaml):
```yaml
# In application.yml
spring:
  ai:
    zhipuai:
      api-key: ${ZHIPUAI_API_KEY}
```

Example 3 (bash):
```bash
# In your environment or .env file
export ZHIPUAI_API_KEY=<your-zhipuai-api-key>
```

Example 4 (java):
```java
// Retrieve API key from a secure source or environment variable
String apiKey = System.getenv("ZHIPUAI_API_KEY");
```

---

## Stability AI Image Generation :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/image/stabilityai-image.html

**Contents:**
- Stability AI Image Generation
- Prerequisites
- Auto-configuration
  - Image Generation Properties
- Runtime Options

Spring AI supports Stability AI’s text to image generation model.

You will need to create an API key with Stability AI to access their AI models. Follow their Getting Started documentation to obtain your API key.

The Spring AI project defines a configuration property named spring.ai.stabilityai.api-key that you should set to the value of the API Key obtained from Stability AI.

You can set this configuration property in your application.properties file:

For enhanced security when handling sensitive information like API keys, you can use Spring Expression Language (SpEL) to reference a custom environment variable:

You can also set this configuration programmatically in your application code:

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Stability AI Image Generation Client. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The prefix spring.ai.stabilityai is used as the property prefix that lets you connect to Stability AI.

spring.ai.stabilityai.base-url

The URL to connect to

spring.ai.stabilityai.api-key

Enabling and disabling of the image auto-configurations are now configured via top level properties with the prefix spring.ai.model.image.

To enable, spring.ai.model.image=stabilityai (It is enabled by default)

To disable, spring.ai.model.image=none (or any value which doesn’t match stabilityai)

This change is done to allow configuration of multiple models.

The prefix spring.ai.stabilityai.image is the property prefix that lets you configure the ImageModel implementation for Stability AI.

spring.ai.stabilityai.image.enabled (Removed and no longer valid)

Enable Stability AI image model.

spring.ai.model.image

Enable Stability AI image model.

spring.ai.stabilityai.image.base-url

Optional overrides the spring.ai.openai.base-url to provide a specific url

https://api.stability.ai/v1

spring.ai.stabilityai.image.api-key

Optional overrides the spring.ai.openai.api-key to provide a specific api-key

spring.ai.stabilityai.image.option.n

The number of images to be generated. Must be between 1 and 10.

spring.ai.stabilityai.image.option.model

The engine/model to use in Stability AI. The model is passed in the URL as a path parameter.

stable-diffusion-v1-6

spring.ai.stabilityai.image.option.width

Width of the image to generate, in pixels, in an increment divisible by 64. Engine-specific dimension validation applies.

spring.ai.stabilityai.image.option.height

Height of the image to generate, in pixels, in an increment divisible by 64. Engine-specific dimension validation applies.

spring.ai.stabilityai.image.option.responseFormat

The format in which the generated images are returned. Must be "application/json" or "image/png".

spring.ai.stabilityai.image.option.cfg_scale

The strictness level of the diffusion process adherence to the prompt text. Range: 0 to 35.

spring.ai.stabilityai.image.option.clip_guidance_preset

Pass in a style preset to guide the image model towards a particular style. This list of style presets is subject to change.

spring.ai.stabilityai.image.option.sampler

Which sampler to use for the diffusion process. If this value is omitted, an appropriate sampler will be automatically selected.

spring.ai.stabilityai.image.option.seed

Random noise seed (omit this option or use 0 for a random seed). Valid range: 0 to 4294967295.

spring.ai.stabilityai.image.option.steps

Number of diffusion steps to run. Valid range: 10 to 50.

spring.ai.stabilityai.image.option.style_preset

Pass in a style preset to guide the image model towards a particular style. This list of style presets is subject to change.

The StabilityAiImageOptions.java provides model configurations, such as the model to use, the style, the size, etc.

On start-up, the default options can be configured with the StabilityAiImageModel(StabilityAiApi stabilityAiApi, StabilityAiImageOptions options) constructor. Alternatively, use the spring.ai.openai.image.options.* properties described previously.

At runtime, you can override the default options by adding new, request specific, options to the ImagePrompt call. For example to override the Stability AI specific options such as quality and the number of images to create, use the following code example:

**Examples:**

Example 1 (unknown):
```unknown
spring.ai.stabilityai.api-key=<your-stabilityai-api-key>
```

Example 2 (yaml):
```yaml
# In application.yml
spring:
  ai:
    stabilityai:
      api-key: ${STABILITYAI_API_KEY}
```

Example 3 (bash):
```bash
# In your environment or .env file
export STABILITYAI_API_KEY=<your-stabilityai-api-key>
```

Example 4 (java):
```java
// Retrieve API key from a secure source or environment variable
String apiKey = System.getenv("STABILITYAI_API_KEY");
```

---

## QianFan Image :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/image/qianfan-image.html

**Contents:**
- QianFan Image

This functionality has been moved to the Spring AI Community repository.

Please visit github.com/spring-ai-community/qianfan for the latest version.

---
