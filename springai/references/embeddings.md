# Springai - Embeddings

**Pages:** 23

---

## PostgresML Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/embeddings/postgresml-embeddings.html

**Contents:**
- PostgresML Embeddings
- Add Repositories and BOM
- Auto-configuration
  - Embedding Properties
- Runtime Options
- Sample Controller
- Manual configuration

Spring AI supports the PostgresML text embeddings models.

Embeddings are a numeric representation of text. They are used to represent words and sentences as vectors, an array of numbers. Embeddings can be used to find similar pieces of text, by comparing the similarity of the numeric vectors using a distance measure, or they can be used as input features for other machine learning models, since most algorithms can’t use text directly.

Many pre-trained LLMs can be used to generate embeddings from text within PostgresML. You can browse all the models available to find the best solution on Hugging Face.

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Azure PostgresML Embedding Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Use the spring.ai.postgresml.embedding.options.* properties to configure your PostgresMlEmbeddingModel. links

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=postgresml (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match postgresml)

This change is done to allow configuration of multiple models.

The prefix spring.ai.postgresml.embedding is property prefix that configures the EmbeddingModel implementation for PostgresML embeddings.

spring.ai.postgresml.embedding.enabled (Removed and no longer valid)

Enable PostgresML embedding model.

spring.ai.model.embedding

Enable PostgresML embedding model.

spring.ai.postgresml.embedding.create-extension

Execute the SQL 'CREATE EXTENSION IF NOT EXISTS pgml' to enable the extension

spring.ai.postgresml.embedding.options.transformer

The Hugging Face transformer model to use for the embedding.

distilbert-base-uncased

spring.ai.postgresml.embedding.options.kwargs

Additional transformer specific options.

spring.ai.postgresml.embedding.options.vectorType

PostgresML vector type to use for the embedding. Two options are supported: PG_ARRAY and PG_VECTOR.

spring.ai.postgresml.embedding.options.metadataMode

Document metadata aggregation mode

Use the PostgresMlEmbeddingOptions.java to configure the PostgresMlEmbeddingModel with options, such as the model to use and etc.

On start you can pass a PostgresMlEmbeddingOptions to the PostgresMlEmbeddingModel constructor to configure the default options used for all embedding requests.

At run-time you can override the default options, using a PostgresMlEmbeddingOptions in your EmbeddingRequest.

For example to override the default model name for a specific request:

This will create a EmbeddingModel implementation that you can inject into your class. Here is an example of a simple @Controller class that uses the EmbeddingModel implementation.

Instead of using the Spring Boot auto-configuration, you can create the PostgresMlEmbeddingModel manually. For this add the spring-ai-postgresml dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Next, create an PostgresMlEmbeddingModel instance and use it to compute the similarity between two input texts:

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-postgresml-embedding</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-postgresml-embedding'
}
```

Example 3 (java):
```java
EmbeddingResponse embeddingResponse = embeddingModel.call(
    new EmbeddingRequest(List.of("Hello World", "World is big and salvation is near"),
            PostgresMlEmbeddingOptions.builder()
                .transformer("intfloat/e5-small")
                .vectorType(VectorType.PG_ARRAY)
                .kwargs(Map.of("device", "gpu"))
                .build()));
```

Example 4 (unknown):
```unknown
spring.ai.postgresml.embedding.options.transformer=distilbert-base-uncased
spring.ai.postgresml.embedding.options.vectorType=PG_ARRAY
spring.ai.postgresml.embedding.options.metadataMode=EMBED
spring.ai.postgresml.embedding.options.kwargs.device=cpu
```

---

## ZhiPuAI Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/embeddings/zhipuai-embeddings.html

**Contents:**
- ZhiPuAI Embeddings
- Prerequisites
  - Add Repositories and BOM
- Auto-configuration
  - Embedding Properties
    - Retry Properties
    - Connection Properties
    - Configuration Properties
- Runtime Options
- Sample Controller

For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI supports the ZhiPuAI’s text embeddings models. ZhiPuAI’s text embeddings measure the relatedness of text strings. An embedding is a vector (list) of floating point numbers. The distance between two vectors measures their relatedness. Small distances suggest high relatedness and large distances suggest low relatedness.

You will need to create an API with ZhiPuAI to access ZhiPu AI language models.

Create an account at ZhiPu AI registration page and generate the token on the API Keys page.

The Spring AI project defines a configuration property named spring.ai.zhipu.api-key that you should set to the value of the API Key obtained from the API Keys page.

You can set this configuration property in your application.properties file:

For enhanced security when handling sensitive information like API keys, you can use Spring Expression Language (SpEL) to reference an environment variable:

You can also set this configuration programmatically in your application code:

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Azure ZhiPuAI Embedding Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The prefix spring.ai.retry is used as the property prefix that lets you configure the retry mechanism for the ZhiPuAI Embedding model.

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

The prefix spring.ai.zhipuai is used as the property prefix that lets you connect to ZhiPuAI.

spring.ai.zhipuai.base-url

The URL to connect to

open.bigmodel.cn/api/paas

spring.ai.zhipuai.api-key

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=zhipuai (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match zhipuai)

This change is done to allow configuration of multiple models.

The prefix spring.ai.zhipuai.embedding is property prefix that configures the EmbeddingModel implementation for ZhiPuAI.

spring.ai.zhipuai.embedding.enabled (Removed and no longer valid)

Enable ZhiPuAI embedding model.

spring.ai.model.embedding

Enable ZhiPuAI embedding model.

spring.ai.zhipuai.embedding.base-url

Optional overrides the spring.ai.zhipuai.base-url to provide embedding specific url

spring.ai.zhipuai.embedding.api-key

Optional overrides the spring.ai.zhipuai.api-key to provide embedding specific api-key

spring.ai.zhipuai.embedding.options.model

spring.ai.zhipuai.embedding.options.dimensions

The number of dimensions, the default value is 2048 when the model is embedding-3

The ZhiPuAiEmbeddingOptions.java provides the ZhiPuAI configurations, such as the model to use and etc.

The default options can be configured using the spring.ai.zhipuai.embedding.options properties as well.

At start-time use the ZhiPuAiEmbeddingModel constructor to set the default options used for all embedding requests. At run-time you can override the default options, using a ZhiPuAiEmbeddingOptions instance as part of your EmbeddingRequest.

For example to override the default model name for a specific request:

This will create a EmbeddingModel implementation that you can inject into your class. Here is an example of a simple @Controller class that uses the EmbeddingModel implementation.

If you are not using Spring Boot, you can manually configure the ZhiPuAI Embedding Model. For this add the spring-ai-zhipuai dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Next, create an ZhiPuAiEmbeddingModel instance and use it to compute the similarity between two input texts:

The ZhiPuAiEmbeddingOptions provides the configuration information for the embedding requests. The options class offers a builder() for easy options creation.

**Examples:**

Example 1 (unknown):
```unknown
spring.ai.zhipu.api-key=<your-zhipu-api-key>
```

Example 2 (yaml):
```yaml
# In application.yml
spring:
  ai:
    zhipu:
      api-key: ${ZHIPU_API_KEY}
```

Example 3 (bash):
```bash
# In your environment or .env file
export ZHIPU_API_KEY=<your-zhipu-api-key>
```

Example 4 (java):
```java
// Retrieve API key from a secure source or environment variable
String apiKey = System.getenv("ZHIPU_API_KEY");
```

---

## Mistral AI Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/embeddings/mistralai-embeddings.html

**Contents:**
- Mistral AI Embeddings
- Prerequisites
  - Add Repositories and BOM
- Auto-configuration
  - Embedding Properties
    - Retry Properties
    - Connection Properties
    - Configuration Properties
- Runtime Options
- Sample Controller

For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI supports the Mistral AI’s text embeddings models. Embeddings are vectorial representations of text that capture the semantic meaning of paragraphs through their position in a high dimensional vector space. Mistral AI Embeddings API offers cutting-edge, state-of-the-art embeddings for text, which can be used for many NLP tasks.

You will need to create an API with MistralAI to access MistralAI embeddings models.

Create an account at MistralAI registration page and generate the token on the API Keys page.

The Spring AI project defines a configuration property named spring.ai.mistralai.api-key that you should set to the value of the API Key obtained from console.mistral.ai.

You can set this configuration property in your application.properties file:

For enhanced security when handling sensitive information like API keys, you can use Spring Expression Language (SpEL) to reference an environment variable:

You can also set this configuration programmatically in your application code:

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the MistralAI Embedding Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The prefix spring.ai.retry is used as the property prefix that lets you configure the retry mechanism for the Mistral AI Embedding model.

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

The prefix spring.ai.mistralai is used as the property prefix that lets you connect to MistralAI.

spring.ai.mistralai.base-url

The URL to connect to

spring.ai.mistralai.api-key

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=mistral (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match mistral)

This change is done to allow configuration of multiple models.

The prefix spring.ai.mistralai.embedding is property prefix that configures the EmbeddingModel implementation for MistralAI.

spring.ai.mistralai.embedding.enabled (Removed and no longer valid)

Enable OpenAI embedding model.

spring.ai.model.embedding

Enable OpenAI embedding model.

spring.ai.mistralai.embedding.base-url

Optional overrides the spring.ai.mistralai.base-url to provide embedding specific url

spring.ai.mistralai.embedding.api-key

Optional overrides the spring.ai.mistralai.api-key to provide embedding specific api-key

spring.ai.mistralai.embedding.metadata-mode

Document content extraction mode.

spring.ai.mistralai.embedding.options.model

spring.ai.mistralai.embedding.options.encodingFormat

The format to return the embeddings in. Can be either float or base64.

The MistralAiEmbeddingOptions.java provides the MistralAI configurations, such as the model to use and etc.

The default options can be configured using the spring.ai.mistralai.embedding.options properties as well.

At start-time use the MistralAiEmbeddingModel constructor to set the default options used for all embedding requests. At run-time you can override the default options, using a MistralAiEmbeddingOptions instance as part of your EmbeddingRequest.

For example to override the default model name for a specific request:

This will create a EmbeddingModel implementation that you can inject into your class. Here is an example of a simple @Controller class that uses the EmbeddingModel implementation.

If you are not using Spring Boot, you can manually configure the OpenAI Embedding Model. For this add the spring-ai-mistral-ai dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Next, create an MistralAiEmbeddingModel instance and use it to compute the similarity between two input texts:

The MistralAiEmbeddingOptions provides the configuration information for the embedding requests. The options class offers a builder() for easy options creation.

**Examples:**

Example 1 (unknown):
```unknown
spring.ai.mistralai.api-key=<your-mistralai-api-key>
```

Example 2 (yaml):
```yaml
# In application.yml
spring:
  ai:
    mistralai:
      api-key: ${MISTRALAI_API_KEY}
```

Example 3 (bash):
```bash
# In your environment or .env file
export MISTRALAI_API_KEY=<your-mistralai-api-key>
```

Example 4 (java):
```java
// Retrieve API key from a secure source or environment variable
String apiKey = System.getenv("MISTRALAI_API_KEY");
```

---

## ZhiPuAI Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/embeddings/zhipuai-embeddings.html

**Contents:**
- ZhiPuAI Embeddings
- Prerequisites
  - Add Repositories and BOM
- Auto-configuration
  - Embedding Properties
    - Retry Properties
    - Connection Properties
    - Configuration Properties
- Runtime Options
- Sample Controller

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI supports the ZhiPuAI’s text embeddings models. ZhiPuAI’s text embeddings measure the relatedness of text strings. An embedding is a vector (list) of floating point numbers. The distance between two vectors measures their relatedness. Small distances suggest high relatedness and large distances suggest low relatedness.

You will need to create an API with ZhiPuAI to access ZhiPu AI language models.

Create an account at ZhiPu AI registration page and generate the token on the API Keys page.

The Spring AI project defines a configuration property named spring.ai.zhipuai.api-key that you should set to the value of the API Key obtained from the API Keys page.

You can set this configuration property in your application.properties file:

For enhanced security when handling sensitive information like API keys, you can use Spring Expression Language (SpEL) to reference an environment variable:

You can also set this configuration programmatically in your application code:

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Azure ZhiPuAI Embedding Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The prefix spring.ai.retry is used as the property prefix that lets you configure the retry mechanism for the ZhiPuAI Embedding model.

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

The prefix spring.ai.zhipuai is used as the property prefix that lets you connect to ZhiPuAI.

spring.ai.zhipuai.base-url

The URL to connect to

open.bigmodel.cn/api/paas

spring.ai.zhipuai.api-key

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=zhipuai (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match zhipuai)

This change is done to allow configuration of multiple models.

The prefix spring.ai.zhipuai.embedding is property prefix that configures the EmbeddingModel implementation for ZhiPuAI.

spring.ai.zhipuai.embedding.enabled (Removed and no longer valid)

Enable ZhiPuAI embedding model.

spring.ai.model.embedding

Enable ZhiPuAI embedding model.

spring.ai.zhipuai.embedding.base-url

Optional overrides the spring.ai.zhipuai.base-url to provide embedding specific url

spring.ai.zhipuai.embedding.api-key

Optional overrides the spring.ai.zhipuai.api-key to provide embedding specific api-key

spring.ai.zhipuai.embedding.options.model

spring.ai.zhipuai.embedding.options.dimensions

The number of dimensions, the default value is 2048 when the model is embedding-3

The ZhiPuAiEmbeddingOptions.java provides the ZhiPuAI configurations, such as the model to use and etc.

The default options can be configured using the spring.ai.zhipuai.embedding.options properties as well.

At start-time use the ZhiPuAiEmbeddingModel constructor to set the default options used for all embedding requests. At run-time you can override the default options, using a ZhiPuAiEmbeddingOptions instance as part of your EmbeddingRequest.

For example to override the default model name for a specific request:

This will create a EmbeddingModel implementation that you can inject into your class. Here is an example of a simple @Controller class that uses the EmbeddingModel implementation.

If you are not using Spring Boot, you can manually configure the ZhiPuAI Embedding Model. For this add the spring-ai-zhipuai dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Next, create an ZhiPuAiEmbeddingModel instance and use it to compute the similarity between two input texts:

The ZhiPuAiEmbeddingOptions provides the configuration information for the embedding requests. The options class offers a builder() for easy options creation.

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

## QianFan Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/embeddings/qianfan-embeddings.html

**Contents:**
- QianFan Embeddings

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This functionality has been moved to the Spring AI Community repository.

Please visit github.com/spring-ai-community/qianfan for the latest version.

---

## Transformers (ONNX) Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/embeddings/onnx.html

**Contents:**
- Transformers (ONNX) Embeddings
- Prerequisites
- Auto-configuration
  - Embedding Properties
  - Errors and special cases
- Manual Configuration

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The TransformersEmbeddingModel is an EmbeddingModel implementation that locally computes sentence embeddings using a selected sentence transformer.

You can use any HuggingFace Embedding model.

It uses pre-trained transformer models, serialized into the Open Neural Network Exchange (ONNX) format.

The Deep Java Library and the Microsoft ONNX Java Runtime libraries are applied to run the ONNX models and compute the embeddings in Java.

To run things in Java, we need to serialize the Tokenizer and the Transformer Model into ONNX format.

Serialize with optimum-cli - One, quick, way to achieve this, is to use the optimum-cli command line tool. The following snippet prepares a python virtual environment, installs the required packages and serializes (e.g. exports) the specified model using optimum-cli :

The snippet exports the sentence-transformers/all-MiniLM-L6-v2 transformer into the onnx-output-folder folder. The latter includes the tokenizer.json and model.onnx files used by the embedding model.

In place of the all-MiniLM-L6-v2 you can pick any huggingface transformer identifier or provide direct file path.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the ONNX Transformer Embedding Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

To configure it, use the spring.ai.embedding.transformer.* properties.

For example, add this to your application.properties file to configure the client with the intfloat/e5-small-v2 text embedding model:

The complete list of supported properties are:

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=transformers (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match transformers)

This change is done to allow configuration of multiple models.

spring.ai.embedding.transformer.enabled (Removed and no longer valid)

Enable the Transformer Embedding model.

spring.ai.model.embedding

Enable the Transformer Embedding model.

spring.ai.embedding.transformer.tokenizer.uri

URI of a pre-trained HuggingFaceTokenizer created by the ONNX engine (e.g. tokenizer.json).

onnx/all-MiniLM-L6-v2/tokenizer.json

spring.ai.embedding.transformer.tokenizer.options

HuggingFaceTokenizer options such as ‘addSpecialTokens’, ‘modelMaxLength’, ‘truncation’, ‘padding’, ‘maxLength’, ‘stride’, ‘padToMultipleOf’. Leave empty to fallback to the defaults.

spring.ai.embedding.transformer.cache.enabled

Enable remote Resource caching.

spring.ai.embedding.transformer.cache.directory

Directory path to cache remote resources, such as the ONNX models

${java.io.tmpdir}/spring-ai-onnx-model

spring.ai.embedding.transformer.onnx.modelUri

Existing, pre-trained ONNX model.

onnx/all-MiniLM-L6-v2/model.onnx

spring.ai.embedding.transformer.onnx.modelOutputName

The ONNX model’s output node name, which we’ll use for embedding calculation.

spring.ai.embedding.transformer.onnx.gpuDeviceId

The GPU device ID to execute on. Only applicable if >= 0. Ignored otherwise.(Requires additional onnxruntime_gpu dependency)

spring.ai.embedding.transformer.metadataMode

Specifies what parts of the Documents content and metadata will be used for computing the embeddings.

If you see an error like Caused by: ai.onnxruntime.OrtException: Supplied array is ragged,.., you need to also enable the tokenizer padding in application.properties as follows:

If you get an error like The generative output names don’t contain expected: last_hidden_state. Consider one of the available model outputs: token_embeddings, …​., you need to set the model output name to a correct value per your models. Consider the names listed in the error message. For example:

If you get an error like ai.onnxruntime.OrtException: Error code - ORT_FAIL - message: Deserialize tensor onnx::MatMul_10319 failed.GetFileLength for ./model.onnx_data failed:Invalid fd was supplied: -1, that means that you model is larger than 2GB and is serialized in two files: model.onnx and model.onnx_data.

The model.onnx_data is called External Data and is expected to be under the same directory of the model.onnx.

Currently the only workaround is to copy the large model.onnx_data in the folder you run your Boot application.

If you get an error like ai.onnxruntime.OrtException: Error code - ORT_EP_FAIL - message: Failed to find CUDA shared provider, that means that you are using the GPU parameters spring.ai.embedding.transformer.onnx.gpuDeviceId , but the onnxruntime_gpu dependency are missing.

Please select the appropriate onnxruntime_gpu version based on the CUDA version(ONNX Java Runtime).

If you are not using Spring Boot, you can manually configure the Onnx Transformers Embedding Model. For this add the spring-ai-transformers dependency to your project’s Maven pom.xml file:

then create a new TransformersEmbeddingModel instance and use the setTokenizerResource(tokenizerJsonUri) and setModelResource(modelOnnxUri) methods to set the URIs of the exported tokenizer.json and model.onnx files. (classpath:, file: or https: URI schemas are supported).

If the model is not explicitly set, TransformersEmbeddingModel defaults to sentence-transformers/all-MiniLM-L6-v2:

The following snippet illustrates how to use the TransformersEmbeddingModel manually:

The first embed() call downloads the large ONNX model and caches it on the local file system. Therefore, the first call might take longer than usual. Use the #setResourceCacheDirectory(<path>) method to set the local folder where the ONNX models as stored. The default cache folder is ${java.io.tmpdir}/spring-ai-onnx-model.

It is more convenient (and preferred) to create the TransformersEmbeddingModel as a Bean. Then you don’t have to call the afterPropertiesSet() manually.

**Examples:**

Example 1 (bash):
```bash
python3 -m venv venv
source ./venv/bin/activate
(venv) pip install --upgrade pip
(venv) pip install optimum onnx onnxruntime sentence-transformers
(venv) optimum-cli export onnx --model sentence-transformers/all-MiniLM-L6-v2 onnx-output-folder
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-transformers</artifactId>
</dependency>
```

Example 3 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-transformers'
}
```

Example 4 (xml):
```xml
<dependency>
  <groupId>org.springframework.ai</groupId>
  <artifactId>spring-ai-transformers</artifactId>
</dependency>
```

---

## Transformers (ONNX) Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/embeddings/onnx.html

**Contents:**
- Transformers (ONNX) Embeddings
- Prerequisites
- Auto-configuration
  - Embedding Properties
  - Errors and special cases
- Manual Configuration

The TransformersEmbeddingModel is an EmbeddingModel implementation that locally computes sentence embeddings using a selected sentence transformer.

You can use any HuggingFace Embedding model.

It uses pre-trained transformer models, serialized into the Open Neural Network Exchange (ONNX) format.

The Deep Java Library and the Microsoft ONNX Java Runtime libraries are applied to run the ONNX models and compute the embeddings in Java.

To run things in Java, we need to serialize the Tokenizer and the Transformer Model into ONNX format.

Serialize with optimum-cli - One, quick, way to achieve this, is to use the optimum-cli command line tool. The following snippet prepares a python virtual environment, installs the required packages and serializes (e.g. exports) the specified model using optimum-cli :

The snippet exports the sentence-transformers/all-MiniLM-L6-v2 transformer into the onnx-output-folder folder. The latter includes the tokenizer.json and model.onnx files used by the embedding model.

In place of the all-MiniLM-L6-v2 you can pick any huggingface transformer identifier or provide direct file path.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the ONNX Transformer Embedding Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

To configure it, use the spring.ai.embedding.transformer.* properties.

For example, add this to your application.properties file to configure the client with the intfloat/e5-small-v2 text embedding model:

The complete list of supported properties are:

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=transformers (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match transformers)

This change is done to allow configuration of multiple models.

spring.ai.embedding.transformer.enabled (Removed and no longer valid)

Enable the Transformer Embedding model.

spring.ai.model.embedding

Enable the Transformer Embedding model.

spring.ai.embedding.transformer.tokenizer.uri

URI of a pre-trained HuggingFaceTokenizer created by the ONNX engine (e.g. tokenizer.json).

onnx/all-MiniLM-L6-v2/tokenizer.json

spring.ai.embedding.transformer.tokenizer.options

HuggingFaceTokenizer options such as ‘addSpecialTokens’, ‘modelMaxLength’, ‘truncation’, ‘padding’, ‘maxLength’, ‘stride’, ‘padToMultipleOf’. Leave empty to fallback to the defaults.

spring.ai.embedding.transformer.cache.enabled

Enable remote Resource caching.

spring.ai.embedding.transformer.cache.directory

Directory path to cache remote resources, such as the ONNX models

${java.io.tmpdir}/spring-ai-onnx-model

spring.ai.embedding.transformer.onnx.modelUri

Existing, pre-trained ONNX model.

onnx/all-MiniLM-L6-v2/model.onnx

spring.ai.embedding.transformer.onnx.modelOutputName

The ONNX model’s output node name, which we’ll use for embedding calculation.

spring.ai.embedding.transformer.onnx.gpuDeviceId

The GPU device ID to execute on. Only applicable if >= 0. Ignored otherwise.(Requires additional onnxruntime_gpu dependency)

spring.ai.embedding.transformer.metadataMode

Specifies what parts of the Documents content and metadata will be used for computing the embeddings.

If you see an error like Caused by: ai.onnxruntime.OrtException: Supplied array is ragged,.., you need to also enable the tokenizer padding in application.properties as follows:

If you get an error like The generative output names don’t contain expected: last_hidden_state. Consider one of the available model outputs: token_embeddings, …​., you need to set the model output name to a correct value per your models. Consider the names listed in the error message. For example:

If you get an error like ai.onnxruntime.OrtException: Error code - ORT_FAIL - message: Deserialize tensor onnx::MatMul_10319 failed.GetFileLength for ./model.onnx_data failed:Invalid fd was supplied: -1, that means that you model is larger than 2GB and is serialized in two files: model.onnx and model.onnx_data.

The model.onnx_data is called External Data and is expected to be under the same directory of the model.onnx.

Currently the only workaround is to copy the large model.onnx_data in the folder you run your Boot application.

If you get an error like ai.onnxruntime.OrtException: Error code - ORT_EP_FAIL - message: Failed to find CUDA shared provider, that means that you are using the GPU parameters spring.ai.embedding.transformer.onnx.gpuDeviceId , but the onnxruntime_gpu dependency are missing.

Please select the appropriate onnxruntime_gpu version based on the CUDA version(ONNX Java Runtime).

If you are not using Spring Boot, you can manually configure the Onnx Transformers Embedding Model. For this add the spring-ai-transformers dependency to your project’s Maven pom.xml file:

then create a new TransformersEmbeddingModel instance and use the setTokenizerResource(tokenizerJsonUri) and setModelResource(modelOnnxUri) methods to set the URIs of the exported tokenizer.json and model.onnx files. (classpath:, file: or https: URI schemas are supported).

If the model is not explicitly set, TransformersEmbeddingModel defaults to sentence-transformers/all-MiniLM-L6-v2:

The following snippet illustrates how to use the TransformersEmbeddingModel manually:

The first embed() call downloads the large ONNX model and caches it on the local file system. Therefore, the first call might take longer than usual. Use the #setResourceCacheDirectory(<path>) method to set the local folder where the ONNX models as stored. The default cache folder is ${java.io.tmpdir}/spring-ai-onnx-model.

It is more convenient (and preferred) to create the TransformersEmbeddingModel as a Bean. Then you don’t have to call the afterPropertiesSet() manually.

**Examples:**

Example 1 (bash):
```bash
python3 -m venv venv
source ./venv/bin/activate
(venv) pip install --upgrade pip
(venv) pip install optimum onnx onnxruntime sentence-transformers
(venv) optimum-cli export onnx --model sentence-transformers/all-MiniLM-L6-v2 onnx-output-folder
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-transformers</artifactId>
</dependency>
```

Example 3 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-transformers'
}
```

Example 4 (xml):
```xml
<dependency>
  <groupId>org.springframework.ai</groupId>
  <artifactId>spring-ai-transformers</artifactId>
</dependency>
```

---

## Oracle Cloud Infrastructure (OCI) GenAI Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/embeddings/oci-genai-embeddings.html

**Contents:**
- Oracle Cloud Infrastructure (OCI) GenAI Embeddings
- Prerequisites
  - Add Repositories and BOM
- Auto-configuration
  - Embedding Properties
- Runtime Options
- Sample Code
- Manual Configuration

For the latest snapshot version, please use Spring AI 1.1.2!

OCI GenAI Service offers text embedding with on-demand models, or dedicated AI clusters.

The OCI Embedding Models Page and OCI Text Embeddings Page provide detailed information about using and hosting embedding models on OCI.

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the OCI GenAI Embedding Client. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The prefix spring.ai.oci.genai is the property prefix to configure the connection to OCI GenAI.

spring.ai.oci.genai.authenticationType

The type of authentication to use when authenticating to OCI. May be file, instance-principal, workload-identity, or simple.

spring.ai.oci.genai.region

spring.ai.oci.genai.tenantId

OCI tenant OCID, used when authenticating with simple auth.

spring.ai.oci.genai.userId

OCI user OCID, used when authenticating with simple auth.

spring.ai.oci.genai.fingerprint

Private key fingerprint, used when authenticating with simple auth.

spring.ai.oci.genai.privateKey

Private key content, used when authenticating with simple auth.

spring.ai.oci.genai.passPhrase

Optional private key passphrase, used when authenticating with simple auth and a passphrase protected private key.

spring.ai.oci.genai.file

Path to OCI config file. Used when authenticating with file auth.

<user’s home directory>/.oci/config

spring.ai.oci.genai.profile

OCI profile name. Used when authenticating with file auth.

spring.ai.oci.genai.endpoint

Optional OCI GenAI endpoint.

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=oci-genai (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match oci-genai)

This change is done to allow configuration of multiple models.

The prefix spring.ai.oci.genai.embedding is the property prefix that configures the EmbeddingModel implementation for OCI GenAI

spring.ai.oci.genai.embedding.enabled (Removed and no longer valid)

Enable OCI GenAI embedding model.

spring.ai.model.embedding

Enable OCI GenAI embedding model.

spring.ai.oci.genai.embedding.compartment

Model compartment OCID.

spring.ai.oci.genai.embedding.servingMode

The model serving mode to be used. May be on-demand, or dedicated.

spring.ai.oci.genai.embedding.truncate

How to truncate text if it overruns the embedding context. May be START, or END.

spring.ai.oci.genai.embedding.model

The model or model endpoint used for embeddings.

The OCIEmbeddingOptions provides the configuration information for the embedding requests. The OCIEmbeddingOptions offers a builder to create the options.

At start time use the OCIEmbeddingOptions constructor to set the default options used for all embedding requests. At run-time you can override the default options, by passing a OCIEmbeddingOptions instance with your to the EmbeddingRequest request.

For example to override the default model name for a specific request:

This will create a EmbeddingModel implementation that you can inject into your class. Here is an example of a simple @Controller class that uses the EmbeddingModel implementation.

If you prefer not to use the Spring Boot auto-configuration, you can manually configure the OCIEmbeddingModel in your application. For this add the spring-oci-genai-openai dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Next, create an OCIEmbeddingModel instance and use it to compute the similarity between two input texts:

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-oci-genai</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-oci-genai'
}
```

Example 3 (java):
```java
EmbeddingResponse embeddingResponse = embeddingModel.call(
    new EmbeddingRequest(List.of("Hello World", "World is big and salvation is near"),
        OCIEmbeddingOptions.builder()
            .model("my-other-embedding-model")
            .build()
));
```

Example 4 (jsx):
```jsx
spring.ai.oci.genai.embedding.model=<your model>
spring.ai.oci.genai.embedding.compartment=<your model compartment>
```

---

## Mistral AI Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/embeddings/mistralai-embeddings.html

**Contents:**
- Mistral AI Embeddings
- Available Models
- Prerequisites
  - Add Repositories and BOM
- Auto-configuration
  - Embedding Properties
    - Retry Properties
    - Connection Properties
    - Configuration Properties
- Runtime Options

Spring AI supports the Mistral AI’s text embeddings models. Embeddings are vectorial representations of text that capture the semantic meaning of paragraphs through their position in a high dimensional vector space. Mistral AI Embeddings API offers cutting-edge, state-of-the-art embeddings for text, which can be used for many NLP tasks.

Mistral AI provides two embedding models, each optimized for different use cases:

General-purpose embedding model suitable for semantic search, clustering, and text similarity tasks. Ideal for natural language content.

Specialized embedding model optimized for code similarity, code search, and retrieval-augmented generation (RAG) with code repositories. Provides higher-dimensional embeddings specifically designed for understanding code semantics.

When choosing a model:

Use mistral-embed for general text content such as documents, articles, or user queries

Use codestral-embed when working with code, technical documentation, or building code-aware RAG systems

You will need to create an API with MistralAI to access MistralAI embeddings models.

Create an account at MistralAI registration page and generate the token on the API Keys page.

The Spring AI project defines a configuration property named spring.ai.mistralai.api-key that you should set to the value of the API Key obtained from console.mistral.ai.

You can set this configuration property in your application.properties file:

For enhanced security when handling sensitive information like API keys, you can use Spring Expression Language (SpEL) to reference an environment variable:

You can also set this configuration programmatically in your application code:

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the MistralAI Embedding Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The prefix spring.ai.retry is used as the property prefix that lets you configure the retry mechanism for the Mistral AI Embedding model.

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

The prefix spring.ai.mistralai is used as the property prefix that lets you connect to MistralAI.

spring.ai.mistralai.base-url

The URL to connect to

spring.ai.mistralai.api-key

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=mistral (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match mistral)

This change is done to allow configuration of multiple models.

The prefix spring.ai.mistralai.embedding is property prefix that configures the EmbeddingModel implementation for MistralAI.

spring.ai.mistralai.embedding.enabled (Removed and no longer valid)

Enable OpenAI embedding model.

spring.ai.model.embedding

Enable OpenAI embedding model.

spring.ai.mistralai.embedding.base-url

Optional overrides the spring.ai.mistralai.base-url to provide embedding specific url

spring.ai.mistralai.embedding.api-key

Optional overrides the spring.ai.mistralai.api-key to provide embedding specific api-key

spring.ai.mistralai.embedding.metadata-mode

Document content extraction mode.

spring.ai.mistralai.embedding.options.model

spring.ai.mistralai.embedding.options.encodingFormat

The format to return the embeddings in. Can be either float or base64.

The MistralAiEmbeddingOptions.java provides the MistralAI configurations, such as the model to use and etc.

The default options can be configured using the spring.ai.mistralai.embedding.options properties as well.

At start-time use the MistralAiEmbeddingModel constructor to set the default options used for all embedding requests. At run-time you can override the default options, using a MistralAiEmbeddingOptions instance as part of your EmbeddingRequest.

For example to override the default model name for a specific request:

This will create a EmbeddingModel implementation that you can inject into your class. Here is an example of a simple @Controller class that uses the EmbeddingModel implementation.

If you are not using Spring Boot, you can manually configure the OpenAI Embedding Model. For this add the spring-ai-mistral-ai dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Next, create an MistralAiEmbeddingModel instance and use it to compute the similarity between two input texts:

The MistralAiEmbeddingOptions provides the configuration information for the embedding requests. The options class offers a builder() for easy options creation.

**Examples:**

Example 1 (unknown):
```unknown
spring.ai.mistralai.api-key=<your-mistralai-api-key>
```

Example 2 (yaml):
```yaml
# In application.yml
spring:
  ai:
    mistralai:
      api-key: ${MISTRALAI_API_KEY}
```

Example 3 (bash):
```bash
# In your environment or .env file
export MISTRALAI_API_KEY=<your-mistralai-api-key>
```

Example 4 (java):
```java
// Retrieve API key from a secure source or environment variable
String apiKey = System.getenv("MISTRALAI_API_KEY");
```

---

## Embeddings Model API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/embeddings.html

**Contents:**
- Embeddings Model API
- API Overview
  - EmbeddingModel
    - EmbeddingRequest
    - EmbeddingResponse
    - Embedding
- Available Implementations

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Embeddings are numerical representations of text, images, or videos that capture relationships between inputs.

Embeddings work by converting text, image, and video into arrays of floating point numbers, called vectors. These vectors are designed to capture the meaning of the text, images, and videos. The length of the embedding array is called the vector’s dimensionality.

By calculating the numerical distance between the vector representations of two pieces of text, an application can determine the similarity between the objects used to generate the embedding vectors.

The EmbeddingModel interface is designed for straightforward integration with embedding models in AI and machine learning. Its primary function is to convert text into numerical vectors, commonly referred to as embeddings. These embeddings are crucial for various tasks such as semantic analysis and text classification.

The design of the EmbeddingModel interface centers around two primary goals:

Portability: This interface ensures easy adaptability across various embedding models. It allows developers to switch between different embedding techniques or models with minimal code changes. This design aligns with Spring’s philosophy of modularity and interchangeability.

Simplicity: EmbeddingModel simplifies the process of converting text to embeddings. By providing straightforward methods like embed(String text) and embed(Document document), it takes the complexity out of dealing with raw text data and embedding algorithms. This design choice makes it easier for developers, especially those new to AI, to utilize embeddings in their applications without delving deep into the underlying mechanics.

The Embedding Model API is built on top of the generic Spring AI Model API, which is a part of the Spring AI library. As such, the EmbeddingModel interface extends the Model interface, which provides a standard set of methods for interacting with AI models. The EmbeddingRequest and EmbeddingResponse classes extend from the ModelRequest and ModelResponse are used to encapsulate the input and output of the embedding models, respectively.

The Embedding API in turn is used by higher-level components to implement Embedding Models for specific embedding models, such as OpenAI, Titan, Azure OpenAI, Ollie, and others.

Following diagram illustrates the Embedding API and its relationship with the Spring AI Model API and the Embedding Models:

This section provides a guide to the EmbeddingModel interface and associated classes.

The embed methods offer various options for converting text into embeddings, accommodating single strings, structured Document objects, or batches of text.

Multiple shortcut methods are provided for embedding text, including the embed(String text) method, which takes a single string and returns the corresponding embedding vector. All shortcuts are implemented around the call method, which is the primary method for invoking the embedding model.

Typically the embedding returns a lists of floats, representing the embeddings in a numerical vector format.

The embedForResponse method provides a more comprehensive output, potentially including additional information about the embeddings.

The dimensions method is a handy tool for developers to quickly ascertain the size of the embedding vectors, which is important for understanding the embedding space and for subsequent processing steps.

The EmbeddingRequest is a ModelRequest that takes a list of text objects and optional embedding request options. The following listing shows a truncated version of the EmbeddingRequest class, excluding constructors and other utility methods:

The structure of the EmbeddingResponse class is as follows:

The EmbeddingResponse class holds the AI Model’s output, with each Embedding instance containing the result vector data from a single text input.

The EmbeddingResponse class also carries a EmbeddingResponseMetadata metadata about the AI Model’s response.

The Embedding represents a single embedding vector.

Internally the various EmbeddingModel implementations use different low-level libraries and APIs to perform the embedding tasks. The following are some of the available implementations of the EmbeddingModel implementations:

Spring AI OpenAI Embeddings

Spring AI Azure OpenAI Embeddings

Spring AI Ollama Embeddings

Spring AI Transformers (ONNX) Embeddings

Spring AI PostgresML Embeddings

Spring AI Bedrock Cohere Embeddings

Spring AI Bedrock Titan Embeddings

Spring AI VertexAI Embeddings

Spring AI Mistral AI Embeddings

Spring AI Oracle Cloud Infrastructure GenAI Embeddings

**Examples:**

Example 1 (java):
```java
public interface EmbeddingModel extends Model<EmbeddingRequest, EmbeddingResponse> {

	@Override
	EmbeddingResponse call(EmbeddingRequest request);


	/**
	 * Embeds the given document's content into a vector.
	 * @param document the document to embed.
	 * @return the embedded vector.
	 */
	float[] embed(Document document);

	/**
	 * Embeds the given text into a vector.
	 * @param text the text to embed.
	 * @return the embedded vector.
	 */
	default float[] embed(String text) {
		Assert.notNull(text, "Text must not be null");
		return this.embed(List.of(text)).iterator().next();
	}

	/**
	 * Embeds a batch of texts into vectors.
	 * @param texts list of texts to embed.
	 * @return list of list of embedded vectors.
	 */
	default List<float[]> embed(List<String> texts) {
		Assert.notNull(texts, "Texts must not be null");
		return this.call(new EmbeddingRequest(texts, EmbeddingOptions.EMPTY))
			.getResults()
			.stream()
			.map(Embedding::getOutput)
			.toList();
	}

	/**
	 * Embeds a batch of texts into vectors and returns the {@link EmbeddingResponse}.
	 * @param texts list of texts to embed.
	 * @return the embedding response.
	 */
	default EmbeddingResponse embedForResponse(List<String> texts) {
		Assert.notNull(texts, "Texts must not be null");
		return this.call(new EmbeddingRequest(texts, EmbeddingOptions.EMPTY));
	}

	/**
	 * @return the number of dimensions of the embedded vectors. It is generative
	 * specific.
	 */
	default int dimensions() {
		return embed("Test String").size();
	}

}
```

Example 2 (java):
```java
public class EmbeddingRequest implements ModelRequest<List<String>> {
	private final List<String> inputs;
	private final EmbeddingOptions options;
	// other methods omitted
}
```

Example 3 (java):
```java
public class EmbeddingResponse implements ModelResponse<Embedding> {

	private List<Embedding> embeddings;
	private EmbeddingResponseMetadata metadata = new EmbeddingResponseMetadata();
	// other methods omitted
}
```

Example 4 (java):
```java
public class Embedding implements ModelResult<float[]> {
	private float[] embedding;
	private Integer index;
	private EmbeddingResultMetadata metadata;
	// other methods omitted
}
```

---

## Oracle Cloud Infrastructure (OCI) GenAI Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/embeddings/oci-genai-embeddings.html

**Contents:**
- Oracle Cloud Infrastructure (OCI) GenAI Embeddings
- Prerequisites
  - Add Repositories and BOM
- Auto-configuration
  - Embedding Properties
- Runtime Options
- Sample Code
- Manual Configuration

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

OCI GenAI Service offers text embedding with on-demand models, or dedicated AI clusters.

The OCI Embedding Models Page and OCI Text Embeddings Page provide detailed information about using and hosting embedding models on OCI.

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the OCI GenAI Embedding Client. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The prefix spring.ai.oci.genai is the property prefix to configure the connection to OCI GenAI.

spring.ai.oci.genai.authenticationType

The type of authentication to use when authenticating to OCI. May be file, instance-principal, workload-identity, or simple.

spring.ai.oci.genai.region

spring.ai.oci.genai.tenantId

OCI tenant OCID, used when authenticating with simple auth.

spring.ai.oci.genai.userId

OCI user OCID, used when authenticating with simple auth.

spring.ai.oci.genai.fingerprint

Private key fingerprint, used when authenticating with simple auth.

spring.ai.oci.genai.privateKey

Private key content, used when authenticating with simple auth.

spring.ai.oci.genai.passPhrase

Optional private key passphrase, used when authenticating with simple auth and a passphrase protected private key.

spring.ai.oci.genai.file

Path to OCI config file. Used when authenticating with file auth.

<user’s home directory>/.oci/config

spring.ai.oci.genai.profile

OCI profile name. Used when authenticating with file auth.

spring.ai.oci.genai.endpoint

Optional OCI GenAI endpoint.

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=oci-genai (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match oci-genai)

This change is done to allow configuration of multiple models.

The prefix spring.ai.oci.genai.embedding is the property prefix that configures the EmbeddingModel implementation for OCI GenAI

spring.ai.oci.genai.embedding.enabled (Removed and no longer valid)

Enable OCI GenAI embedding model.

spring.ai.model.embedding

Enable OCI GenAI embedding model.

spring.ai.oci.genai.embedding.compartment

Model compartment OCID.

spring.ai.oci.genai.embedding.servingMode

The model serving mode to be used. May be on-demand, or dedicated.

spring.ai.oci.genai.embedding.truncate

How to truncate text if it overruns the embedding context. May be START, or END.

spring.ai.oci.genai.embedding.model

The model or model endpoint used for embeddings.

The OCIEmbeddingOptions provides the configuration information for the embedding requests. The OCIEmbeddingOptions offers a builder to create the options.

At start time use the OCIEmbeddingOptions constructor to set the default options used for all embedding requests. At run-time you can override the default options, by passing a OCIEmbeddingOptions instance with your to the EmbeddingRequest request.

For example to override the default model name for a specific request:

This will create a EmbeddingModel implementation that you can inject into your class. Here is an example of a simple @Controller class that uses the EmbeddingModel implementation.

If you prefer not to use the Spring Boot auto-configuration, you can manually configure the OCIEmbeddingModel in your application. For this add the spring-oci-genai-openai dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Next, create an OCIEmbeddingModel instance and use it to compute the similarity between two input texts:

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-oci-genai</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-oci-genai'
}
```

Example 3 (java):
```java
EmbeddingResponse embeddingResponse = embeddingModel.call(
    new EmbeddingRequest(List.of("Hello World", "World is big and salvation is near"),
        OCIEmbeddingOptions.builder()
            .model("my-other-embedding-model")
            .build()
));
```

Example 4 (jsx):
```jsx
spring.ai.oci.genai.embedding.model=<your model>
spring.ai.oci.genai.embedding.compartment=<your model compartment>
```

---

## ZhiPuAI Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/embeddings/zhipuai-embeddings.html

**Contents:**
- ZhiPuAI Embeddings
- Prerequisites
  - Add Repositories and BOM
- Auto-configuration
  - Embedding Properties
    - Retry Properties
    - Connection Properties
    - Configuration Properties
- Runtime Options
- Sample Controller

Spring AI supports the ZhiPuAI’s text embeddings models. ZhiPuAI’s text embeddings measure the relatedness of text strings. An embedding is a vector (list) of floating point numbers. The distance between two vectors measures their relatedness. Small distances suggest high relatedness and large distances suggest low relatedness.

You will need to create an API with ZhiPuAI to access ZhiPu AI language models.

Create an account at ZhiPu AI registration page and generate the token on the API Keys page.

The Spring AI project defines a configuration property named spring.ai.zhipuai.api-key that you should set to the value of the API Key obtained from the API Keys page.

You can set this configuration property in your application.properties file:

For enhanced security when handling sensitive information like API keys, you can use Spring Expression Language (SpEL) to reference an environment variable:

You can also set this configuration programmatically in your application code:

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Azure ZhiPuAI Embedding Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The prefix spring.ai.retry is used as the property prefix that lets you configure the retry mechanism for the ZhiPuAI Embedding model.

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

The prefix spring.ai.zhipuai is used as the property prefix that lets you connect to ZhiPuAI.

spring.ai.zhipuai.base-url

The URL to connect to

open.bigmodel.cn/api/paas

spring.ai.zhipuai.api-key

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=zhipuai (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match zhipuai)

This change is done to allow configuration of multiple models.

The prefix spring.ai.zhipuai.embedding is property prefix that configures the EmbeddingModel implementation for ZhiPuAI.

spring.ai.zhipuai.embedding.enabled (Removed and no longer valid)

Enable ZhiPuAI embedding model.

spring.ai.model.embedding

Enable ZhiPuAI embedding model.

spring.ai.zhipuai.embedding.base-url

Optional overrides the spring.ai.zhipuai.base-url to provide embedding specific url

spring.ai.zhipuai.embedding.api-key

Optional overrides the spring.ai.zhipuai.api-key to provide embedding specific api-key

spring.ai.zhipuai.embedding.options.model

spring.ai.zhipuai.embedding.options.dimensions

The number of dimensions, the default value is 2048 when the model is embedding-3

The ZhiPuAiEmbeddingOptions.java provides the ZhiPuAI configurations, such as the model to use and etc.

The default options can be configured using the spring.ai.zhipuai.embedding.options properties as well.

At start-time use the ZhiPuAiEmbeddingModel constructor to set the default options used for all embedding requests. At run-time you can override the default options, using a ZhiPuAiEmbeddingOptions instance as part of your EmbeddingRequest.

For example to override the default model name for a specific request:

This will create a EmbeddingModel implementation that you can inject into your class. Here is an example of a simple @Controller class that uses the EmbeddingModel implementation.

If you are not using Spring Boot, you can manually configure the ZhiPuAI Embedding Model. For this add the spring-ai-zhipuai dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Next, create an ZhiPuAiEmbeddingModel instance and use it to compute the similarity between two input texts:

The ZhiPuAiEmbeddingOptions provides the configuration information for the embedding requests. The options class offers a builder() for easy options creation.

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

## Oracle Cloud Infrastructure (OCI) GenAI Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/embeddings/oci-genai-embeddings.html

**Contents:**
- Oracle Cloud Infrastructure (OCI) GenAI Embeddings
- Prerequisites
  - Add Repositories and BOM
- Auto-configuration
  - Embedding Properties
- Runtime Options
- Sample Code
- Manual Configuration

OCI GenAI Service offers text embedding with on-demand models, or dedicated AI clusters.

The OCI Embedding Models Page and OCI Text Embeddings Page provide detailed information about using and hosting embedding models on OCI.

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the OCI GenAI Embedding Client. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The prefix spring.ai.oci.genai is the property prefix to configure the connection to OCI GenAI.

spring.ai.oci.genai.authenticationType

The type of authentication to use when authenticating to OCI. May be file, instance-principal, workload-identity, or simple.

spring.ai.oci.genai.region

spring.ai.oci.genai.tenantId

OCI tenant OCID, used when authenticating with simple auth.

spring.ai.oci.genai.userId

OCI user OCID, used when authenticating with simple auth.

spring.ai.oci.genai.fingerprint

Private key fingerprint, used when authenticating with simple auth.

spring.ai.oci.genai.privateKey

Private key content, used when authenticating with simple auth.

spring.ai.oci.genai.passPhrase

Optional private key passphrase, used when authenticating with simple auth and a passphrase protected private key.

spring.ai.oci.genai.file

Path to OCI config file. Used when authenticating with file auth.

<user’s home directory>/.oci/config

spring.ai.oci.genai.profile

OCI profile name. Used when authenticating with file auth.

spring.ai.oci.genai.endpoint

Optional OCI GenAI endpoint.

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=oci-genai (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match oci-genai)

This change is done to allow configuration of multiple models.

The prefix spring.ai.oci.genai.embedding is the property prefix that configures the EmbeddingModel implementation for OCI GenAI

spring.ai.oci.genai.embedding.enabled (Removed and no longer valid)

Enable OCI GenAI embedding model.

spring.ai.model.embedding

Enable OCI GenAI embedding model.

spring.ai.oci.genai.embedding.compartment

Model compartment OCID.

spring.ai.oci.genai.embedding.servingMode

The model serving mode to be used. May be on-demand, or dedicated.

spring.ai.oci.genai.embedding.truncate

How to truncate text if it overruns the embedding context. May be START, or END.

spring.ai.oci.genai.embedding.model

The model or model endpoint used for embeddings.

The OCIEmbeddingOptions provides the configuration information for the embedding requests. The OCIEmbeddingOptions offers a builder to create the options.

At start time use the OCIEmbeddingOptions constructor to set the default options used for all embedding requests. At run-time you can override the default options, by passing a OCIEmbeddingOptions instance with your to the EmbeddingRequest request.

For example to override the default model name for a specific request:

This will create a EmbeddingModel implementation that you can inject into your class. Here is an example of a simple @Controller class that uses the EmbeddingModel implementation.

If you prefer not to use the Spring Boot auto-configuration, you can manually configure the OCIEmbeddingModel in your application. For this add the spring-oci-genai-openai dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Next, create an OCIEmbeddingModel instance and use it to compute the similarity between two input texts:

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-oci-genai</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-oci-genai'
}
```

Example 3 (java):
```java
EmbeddingResponse embeddingResponse = embeddingModel.call(
    new EmbeddingRequest(List.of("Hello World", "World is big and salvation is near"),
        OCIEmbeddingOptions.builder()
            .model("my-other-embedding-model")
            .build()
));
```

Example 4 (jsx):
```jsx
spring.ai.oci.genai.embedding.model=<your model>
spring.ai.oci.genai.embedding.compartment=<your model compartment>
```

---

## Google GenAI Text Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/embeddings/google-genai-embeddings-text.html

**Contents:**
- Google GenAI Text Embeddings
- Prerequisites
  - Option 1: Gemini Developer API (API Key)
  - Option 2: Vertex AI (Google Cloud)
  - Add Repositories and BOM
- Auto-configuration
  - Embedding Properties
    - Connection Properties
    - Text Embedding Properties
- Sample Controller

The Google GenAI Embeddings API provides text embedding generation using Google’s embedding models through either the Gemini Developer API or Vertex AI. This document describes how to create text embeddings using the Google GenAI Text embeddings API.

Google GenAI text embeddings API uses dense vector representations. Unlike sparse vectors, which tend to directly map words to numbers, dense vectors are designed to better represent the meaning of a piece of text. The benefit of using dense vector embeddings in generative AI is that instead of searching for direct word or syntax matches, you can better search for passages that align to the meaning of the query, even if the passages don’t use the same language.

Currently, the Google GenAI SDK supports text embeddings only. Multimodal embeddings support is pending and will be added when available in the SDK.

This implementation provides two authentication modes:

Gemini Developer API: Use an API key for quick prototyping and development

Vertex AI: Use Google Cloud credentials for production deployments with enterprise features

Choose one of the following authentication methods:

Obtain an API key from the Google AI Studio

Set the API key as an environment variable or in your application properties

Install the gcloud CLI, appropriate for your OS.

Authenticate by running the following command. Replace PROJECT_ID with your Google Cloud project ID and ACCOUNT with your Google Cloud username.

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Google GenAI Embedding Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The prefix spring.ai.google.genai.embedding is used as the property prefix that lets you connect to Google GenAI Embedding API.

The connection properties are shared with the Google GenAI Chat module. If you’re using both chat and embeddings, you only need to configure the connection once using either spring.ai.google.genai prefix (for chat) or spring.ai.google.genai.embedding prefix (for embeddings).

spring.ai.google.genai.embedding.api-key

API key for Gemini Developer API. When provided, the client uses the Gemini Developer API instead of Vertex AI.

spring.ai.google.genai.embedding.project-id

Google Cloud Platform project ID (required for Vertex AI mode)

spring.ai.google.genai.embedding.location

Google Cloud region (required for Vertex AI mode)

spring.ai.google.genai.embedding.credentials-uri

URI to Google Cloud credentials. When provided it is used to create a GoogleCredentials instance for authentication.

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding.text=google-genai (It is enabled by default)

To disable, spring.ai.model.embedding.text=none (or any value which doesn’t match google-genai)

This change is done to allow configuration of multiple models.

The prefix spring.ai.google.genai.embedding.text is the property prefix that lets you configure the embedding model implementation for Google GenAI Text Embedding.

spring.ai.model.embedding.text

Enable Google GenAI Embedding API model.

spring.ai.google.genai.embedding.text.options.model

The Google GenAI Text Embedding model to use. Supported models include text-embedding-004 and text-multilingual-embedding-002

spring.ai.google.genai.embedding.text.options.task-type

The intended downstream application to help the model produce better quality embeddings. Available task-types: RETRIEVAL_QUERY, RETRIEVAL_DOCUMENT, SEMANTIC_SIMILARITY, CLASSIFICATION, CLUSTERING, QUESTION_ANSWERING, FACT_VERIFICATION

spring.ai.google.genai.embedding.text.options.title

Optional title, only valid with task_type=RETRIEVAL_DOCUMENT.

spring.ai.google.genai.embedding.text.options.dimensions

The number of dimensions the resulting output embeddings should have. Supported for model version 004 and later. You can use this parameter to reduce the embedding size, for example, for storage optimization.

spring.ai.google.genai.embedding.text.options.auto-truncate

When set to true, input text will be truncated. When set to false, an error is returned if the input text is longer than the maximum length supported by the model.

Create a new Spring Boot project and add the spring-ai-starter-model-google-genai-embedding to your pom (or gradle) dependencies.

Add a application.properties file, under the src/main/resources directory, to enable and configure the Google GenAI embedding model:

This will create a GoogleGenAiTextEmbeddingModel implementation that you can inject into your class. Here is an example of a simple @Controller class that uses the embedding model for embeddings generations.

The GoogleGenAiTextEmbeddingModel implements the EmbeddingModel.

Add the spring-ai-google-genai-embedding dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Next, create a GoogleGenAiTextEmbeddingModel and use it for text embeddings:

The Google GenAI embeddings API supports different task types to optimize embeddings for specific use cases:

RETRIEVAL_QUERY: Optimized for search queries in retrieval systems

RETRIEVAL_DOCUMENT: Optimized for documents in retrieval systems

SEMANTIC_SIMILARITY: Optimized for measuring semantic similarity between texts

CLASSIFICATION: Optimized for text classification tasks

CLUSTERING: Optimized for clustering similar texts

QUESTION_ANSWERING: Optimized for question-answering systems

FACT_VERIFICATION: Optimized for fact verification tasks

Example of using different task types:

For model version 004 and later, you can reduce the embedding dimensions for storage optimization:

If you’re currently using the Vertex AI Text Embeddings implementation (spring-ai-vertex-ai-embedding), you can migrate to Google GenAI with minimal changes:

SDK: Google GenAI uses the new com.google.genai.Client instead of Vertex AI SDK

Authentication: Supports both API key and Google Cloud credentials

Package Names: Classes are in org.springframework.ai.google.genai.text instead of org.springframework.ai.vertexai.embedding

Property Prefix: Uses spring.ai.google.genai.embedding instead of spring.ai.vertex.ai.embedding

Connection Details: Uses GoogleGenAiEmbeddingConnectionDetails instead of VertexAiEmbeddingConnectionDetails

Use Google GenAI Embeddings when: - You want quick prototyping with API keys - You need the latest embedding features from the Developer API - You want flexibility to switch between API key and Vertex AI modes - You’re already using Google GenAI for chat

Use Vertex AI Text Embeddings when: - You have existing Vertex AI infrastructure - You need multimodal embeddings (currently only available in Vertex AI) - Your organization requires Google Cloud-only deployment

**Examples:**

Example 1 (typescript):
```typescript
gcloud config set project <PROJECT_ID> &&
gcloud auth application-default login <ACCOUNT>
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-google-genai-embedding</artifactId>
</dependency>
```

Example 3 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-google-genai-embedding'
}
```

Example 4 (unknown):
```unknown
spring.ai.google.genai.embedding.api-key=YOUR_API_KEY
spring.ai.google.genai.embedding.text.options.model=text-embedding-004
```

---

## QianFan Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/embeddings/qianfan-embeddings.html

**Contents:**
- QianFan Embeddings

For the latest snapshot version, please use Spring AI 1.1.2!

This functionality has been moved to the Spring AI Community repository.

Please visit github.com/spring-ai-community/qianfan for the latest version.

---

## Embeddings Model API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/embeddings.html

**Contents:**
- Embeddings Model API
- API Overview
  - EmbeddingModel
    - EmbeddingRequest
    - EmbeddingResponse
    - Embedding
- Available Implementations

Embeddings are numerical representations of text, images, or videos that capture relationships between inputs.

Embeddings work by converting text, image, and video into arrays of floating point numbers, called vectors. These vectors are designed to capture the meaning of the text, images, and videos. The length of the embedding array is called the vector’s dimensionality.

By calculating the numerical distance between the vector representations of two pieces of text, an application can determine the similarity between the objects used to generate the embedding vectors.

The EmbeddingModel interface is designed for straightforward integration with embedding models in AI and machine learning. Its primary function is to convert text into numerical vectors, commonly referred to as embeddings. These embeddings are crucial for various tasks such as semantic analysis and text classification.

The design of the EmbeddingModel interface centers around two primary goals:

Portability: This interface ensures easy adaptability across various embedding models. It allows developers to switch between different embedding techniques or models with minimal code changes. This design aligns with Spring’s philosophy of modularity and interchangeability.

Simplicity: EmbeddingModel simplifies the process of converting text to embeddings. By providing straightforward methods like embed(String text) and embed(Document document), it takes the complexity out of dealing with raw text data and embedding algorithms. This design choice makes it easier for developers, especially those new to AI, to utilize embeddings in their applications without delving deep into the underlying mechanics.

The Embedding Model API is built on top of the generic Spring AI Model API, which is a part of the Spring AI library. As such, the EmbeddingModel interface extends the Model interface, which provides a standard set of methods for interacting with AI models. The EmbeddingRequest and EmbeddingResponse classes extend from the ModelRequest and ModelResponse are used to encapsulate the input and output of the embedding models, respectively.

The Embedding API in turn is used by higher-level components to implement Embedding Models for specific embedding models, such as OpenAI, Titan, Azure OpenAI, Ollie, and others.

Following diagram illustrates the Embedding API and its relationship with the Spring AI Model API and the Embedding Models:

This section provides a guide to the EmbeddingModel interface and associated classes.

The embed methods offer various options for converting text into embeddings, accommodating single strings, structured Document objects, or batches of text.

Multiple shortcut methods are provided for embedding text, including the embed(String text) method, which takes a single string and returns the corresponding embedding vector. All shortcuts are implemented around the call method, which is the primary method for invoking the embedding model.

Typically the embedding returns a lists of floats, representing the embeddings in a numerical vector format.

The embedForResponse method provides a more comprehensive output, potentially including additional information about the embeddings.

The dimensions method is a handy tool for developers to quickly ascertain the size of the embedding vectors, which is important for understanding the embedding space and for subsequent processing steps.

The EmbeddingRequest is a ModelRequest that takes a list of text objects and optional embedding request options. The following listing shows a truncated version of the EmbeddingRequest class, excluding constructors and other utility methods:

The structure of the EmbeddingResponse class is as follows:

The EmbeddingResponse class holds the AI Model’s output, with each Embedding instance containing the result vector data from a single text input.

The EmbeddingResponse class also carries a EmbeddingResponseMetadata metadata about the AI Model’s response.

The Embedding represents a single embedding vector.

Internally the various EmbeddingModel implementations use different low-level libraries and APIs to perform the embedding tasks. The following are some of the available implementations of the EmbeddingModel implementations:

Spring AI OpenAI Embeddings

Spring AI Azure OpenAI Embeddings

Spring AI Ollama Embeddings

Spring AI Transformers (ONNX) Embeddings

Spring AI PostgresML Embeddings

Spring AI Bedrock Cohere Embeddings

Spring AI Bedrock Titan Embeddings

Spring AI VertexAI Embeddings

Spring AI Mistral AI Embeddings

Spring AI Oracle Cloud Infrastructure GenAI Embeddings

**Examples:**

Example 1 (java):
```java
public interface EmbeddingModel extends Model<EmbeddingRequest, EmbeddingResponse> {

	@Override
	EmbeddingResponse call(EmbeddingRequest request);


	/**
	 * Embeds the given document's content into a vector.
	 * @param document the document to embed.
	 * @return the embedded vector.
	 */
	float[] embed(Document document);

	/**
	 * Embeds the given text into a vector.
	 * @param text the text to embed.
	 * @return the embedded vector.
	 */
	default float[] embed(String text) {
		Assert.notNull(text, "Text must not be null");
		return this.embed(List.of(text)).iterator().next();
	}

	/**
	 * Embeds a batch of texts into vectors.
	 * @param texts list of texts to embed.
	 * @return list of list of embedded vectors.
	 */
	default List<float[]> embed(List<String> texts) {
		Assert.notNull(texts, "Texts must not be null");
		return this.call(new EmbeddingRequest(texts, EmbeddingOptions.EMPTY))
			.getResults()
			.stream()
			.map(Embedding::getOutput)
			.toList();
	}

	/**
	 * Embeds a batch of texts into vectors and returns the {@link EmbeddingResponse}.
	 * @param texts list of texts to embed.
	 * @return the embedding response.
	 */
	default EmbeddingResponse embedForResponse(List<String> texts) {
		Assert.notNull(texts, "Texts must not be null");
		return this.call(new EmbeddingRequest(texts, EmbeddingOptions.EMPTY));
	}

	/**
	 * @return the number of dimensions of the embedded vectors. It is generative
	 * specific.
	 */
	default int dimensions() {
		return embed("Test String").size();
	}

}
```

Example 2 (java):
```java
public class EmbeddingRequest implements ModelRequest<List<String>> {
	private final List<String> inputs;
	private final EmbeddingOptions options;
	// other methods omitted
}
```

Example 3 (java):
```java
public class EmbeddingResponse implements ModelResponse<Embedding> {

	private List<Embedding> embeddings;
	private EmbeddingResponseMetadata metadata = new EmbeddingResponseMetadata();
	// other methods omitted
}
```

Example 4 (java):
```java
public class Embedding implements ModelResult<float[]> {
	private float[] embedding;
	private Integer index;
	private EmbeddingResultMetadata metadata;
	// other methods omitted
}
```

---

## QianFan Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/embeddings/qianfan-embeddings.html

**Contents:**
- QianFan Embeddings

This functionality has been moved to the Spring AI Community repository.

Please visit github.com/spring-ai-community/qianfan for the latest version.

---

## PostgresML Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/embeddings/postgresml-embeddings.html

**Contents:**
- PostgresML Embeddings
- Add Repositories and BOM
- Auto-configuration
  - Embedding Properties
- Runtime Options
- Sample Controller
- Manual configuration

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI supports the PostgresML text embeddings models.

Embeddings are a numeric representation of text. They are used to represent words and sentences as vectors, an array of numbers. Embeddings can be used to find similar pieces of text, by comparing the similarity of the numeric vectors using a distance measure, or they can be used as input features for other machine learning models, since most algorithms can’t use text directly.

Many pre-trained LLMs can be used to generate embeddings from text within PostgresML. You can browse all the models available to find the best solution on Hugging Face.

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Azure PostgresML Embedding Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Use the spring.ai.postgresml.embedding.options.* properties to configure your PostgresMlEmbeddingModel. links

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=postgresml (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match postgresml)

This change is done to allow configuration of multiple models.

The prefix spring.ai.postgresml.embedding is property prefix that configures the EmbeddingModel implementation for PostgresML embeddings.

spring.ai.postgresml.embedding.enabled (Removed and no longer valid)

Enable PostgresML embedding model.

spring.ai.model.embedding

Enable PostgresML embedding model.

spring.ai.postgresml.embedding.create-extension

Execute the SQL 'CREATE EXTENSION IF NOT EXISTS pgml' to enable the extension

spring.ai.postgresml.embedding.options.transformer

The Hugging Face transformer model to use for the embedding.

distilbert-base-uncased

spring.ai.postgresml.embedding.options.kwargs

Additional transformer specific options.

spring.ai.postgresml.embedding.options.vectorType

PostgresML vector type to use for the embedding. Two options are supported: PG_ARRAY and PG_VECTOR.

spring.ai.postgresml.embedding.options.metadataMode

Document metadata aggregation mode

Use the PostgresMlEmbeddingOptions.java to configure the PostgresMlEmbeddingModel with options, such as the model to use and etc.

On start you can pass a PostgresMlEmbeddingOptions to the PostgresMlEmbeddingModel constructor to configure the default options used for all embedding requests.

At run-time you can override the default options, using a PostgresMlEmbeddingOptions in your EmbeddingRequest.

For example to override the default model name for a specific request:

This will create a EmbeddingModel implementation that you can inject into your class. Here is an example of a simple @Controller class that uses the EmbeddingModel implementation.

Instead of using the Spring Boot auto-configuration, you can create the PostgresMlEmbeddingModel manually. For this add the spring-ai-postgresml dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Next, create an PostgresMlEmbeddingModel instance and use it to compute the similarity between two input texts:

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-postgresml-embedding</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-postgresml-embedding'
}
```

Example 3 (java):
```java
EmbeddingResponse embeddingResponse = embeddingModel.call(
    new EmbeddingRequest(List.of("Hello World", "World is big and salvation is near"),
            PostgresMlEmbeddingOptions.builder()
                .transformer("intfloat/e5-small")
                .vectorType(VectorType.PG_ARRAY)
                .kwargs(Map.of("device", "gpu"))
                .build()));
```

Example 4 (unknown):
```unknown
spring.ai.postgresml.embedding.options.transformer=distilbert-base-uncased
spring.ai.postgresml.embedding.options.vectorType=PG_ARRAY
spring.ai.postgresml.embedding.options.metadataMode=EMBED
spring.ai.postgresml.embedding.options.kwargs.device=cpu
```

---

## Embeddings Model API :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/embeddings.html

**Contents:**
- Embeddings Model API
- API Overview
  - EmbeddingModel
    - EmbeddingRequest
    - EmbeddingResponse
    - Embedding
- Available Implementations

For the latest snapshot version, please use Spring AI 1.1.2!

Embeddings are numerical representations of text, images, or videos that capture relationships between inputs.

Embeddings work by converting text, image, and video into arrays of floating point numbers, called vectors. These vectors are designed to capture the meaning of the text, images, and videos. The length of the embedding array is called the vector’s dimensionality.

By calculating the numerical distance between the vector representations of two pieces of text, an application can determine the similarity between the objects used to generate the embedding vectors.

The EmbeddingModel interface is designed for straightforward integration with embedding models in AI and machine learning. Its primary function is to convert text into numerical vectors, commonly referred to as embeddings. These embeddings are crucial for various tasks such as semantic analysis and text classification.

The design of the EmbeddingModel interface centers around two primary goals:

Portability: This interface ensures easy adaptability across various embedding models. It allows developers to switch between different embedding techniques or models with minimal code changes. This design aligns with Spring’s philosophy of modularity and interchangeability.

Simplicity: EmbeddingModel simplifies the process of converting text to embeddings. By providing straightforward methods like embed(String text) and embed(Document document), it takes the complexity out of dealing with raw text data and embedding algorithms. This design choice makes it easier for developers, especially those new to AI, to utilize embeddings in their applications without delving deep into the underlying mechanics.

The Embedding Model API is built on top of the generic Spring AI Model API, which is a part of the Spring AI library. As such, the EmbeddingModel interface extends the Model interface, which provides a standard set of methods for interacting with AI models. The EmbeddingRequest and EmbeddingResponse classes extend from the ModelRequest and ModelResponse are used to encapsulate the input and output of the embedding models, respectively.

The Embedding API in turn is used by higher-level components to implement Embedding Models for specific embedding models, such as OpenAI, Titan, Azure OpenAI, Ollie, and others.

Following diagram illustrates the Embedding API and its relationship with the Spring AI Model API and the Embedding Models:

This section provides a guide to the EmbeddingModel interface and associated classes.

The embed methods offer various options for converting text into embeddings, accommodating single strings, structured Document objects, or batches of text.

Multiple shortcut methods are provided for embedding text, including the embed(String text) method, which takes a single string and returns the corresponding embedding vector. All shortcuts are implemented around the call method, which is the primary method for invoking the embedding model.

Typically the embedding returns a lists of floats, representing the embeddings in a numerical vector format.

The embedForResponse method provides a more comprehensive output, potentially including additional information about the embeddings.

The dimensions method is a handy tool for developers to quickly ascertain the size of the embedding vectors, which is important for understanding the embedding space and for subsequent processing steps.

The EmbeddingRequest is a ModelRequest that takes a list of text objects and optional embedding request options. The following listing shows a truncated version of the EmbeddingRequest class, excluding constructors and other utility methods:

The structure of the EmbeddingResponse class is as follows:

The EmbeddingResponse class holds the AI Model’s output, with each Embedding instance containing the result vector data from a single text input.

The EmbeddingResponse class also carries a EmbeddingResponseMetadata metadata about the AI Model’s response.

The Embedding represents a single embedding vector.

Internally the various EmbeddingModel implementations use different low-level libraries and APIs to perform the embedding tasks. The following are some of the available implementations of the EmbeddingModel implementations:

Spring AI OpenAI Embeddings

Spring AI Azure OpenAI Embeddings

Spring AI Ollama Embeddings

Spring AI Transformers (ONNX) Embeddings

Spring AI PostgresML Embeddings

Spring AI Bedrock Cohere Embeddings

Spring AI Bedrock Titan Embeddings

Spring AI VertexAI Embeddings

Spring AI Mistral AI Embeddings

Spring AI Oracle Cloud Infrastructure GenAI Embeddings

**Examples:**

Example 1 (java):
```java
public interface EmbeddingModel extends Model<EmbeddingRequest, EmbeddingResponse> {

	@Override
	EmbeddingResponse call(EmbeddingRequest request);


	/**
	 * Embeds the given document's content into a vector.
	 * @param document the document to embed.
	 * @return the embedded vector.
	 */
	float[] embed(Document document);

	/**
	 * Embeds the given text into a vector.
	 * @param text the text to embed.
	 * @return the embedded vector.
	 */
	default float[] embed(String text) {
		Assert.notNull(text, "Text must not be null");
		return this.embed(List.of(text)).iterator().next();
	}

	/**
	 * Embeds a batch of texts into vectors.
	 * @param texts list of texts to embed.
	 * @return list of list of embedded vectors.
	 */
	default List<float[]> embed(List<String> texts) {
		Assert.notNull(texts, "Texts must not be null");
		return this.call(new EmbeddingRequest(texts, EmbeddingOptions.EMPTY))
			.getResults()
			.stream()
			.map(Embedding::getOutput)
			.toList();
	}

	/**
	 * Embeds a batch of texts into vectors and returns the {@link EmbeddingResponse}.
	 * @param texts list of texts to embed.
	 * @return the embedding response.
	 */
	default EmbeddingResponse embedForResponse(List<String> texts) {
		Assert.notNull(texts, "Texts must not be null");
		return this.call(new EmbeddingRequest(texts, EmbeddingOptions.EMPTY));
	}

	/**
	 * @return the number of dimensions of the embedded vectors. It is generative
	 * specific.
	 */
	default int dimensions() {
		return embed("Test String").size();
	}

}
```

Example 2 (java):
```java
public class EmbeddingRequest implements ModelRequest<List<String>> {
	private final List<String> inputs;
	private final EmbeddingOptions options;
	// other methods omitted
}
```

Example 3 (java):
```java
public class EmbeddingResponse implements ModelResponse<Embedding> {

	private List<Embedding> embeddings;
	private EmbeddingResponseMetadata metadata = new EmbeddingResponseMetadata();
	// other methods omitted
}
```

Example 4 (java):
```java
public class Embedding implements ModelResult<float[]> {
	private float[] embedding;
	private Integer index;
	private EmbeddingResultMetadata metadata;
	// other methods omitted
}
```

---

## Google GenAI Text Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/embeddings/google-genai-embeddings-text.html

**Contents:**
- Google GenAI Text Embeddings
- Prerequisites
  - Option 1: Gemini Developer API (API Key)
  - Option 2: Vertex AI (Google Cloud)
  - Add Repositories and BOM
- Auto-configuration
  - Embedding Properties
    - Connection Properties
    - Text Embedding Properties
- Sample Controller

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The Google GenAI Embeddings API provides text embedding generation using Google’s embedding models through either the Gemini Developer API or Vertex AI. This document describes how to create text embeddings using the Google GenAI Text embeddings API.

Google GenAI text embeddings API uses dense vector representations. Unlike sparse vectors, which tend to directly map words to numbers, dense vectors are designed to better represent the meaning of a piece of text. The benefit of using dense vector embeddings in generative AI is that instead of searching for direct word or syntax matches, you can better search for passages that align to the meaning of the query, even if the passages don’t use the same language.

Currently, the Google GenAI SDK supports text embeddings only. Multimodal embeddings support is pending and will be added when available in the SDK.

This implementation provides two authentication modes:

Gemini Developer API: Use an API key for quick prototyping and development

Vertex AI: Use Google Cloud credentials for production deployments with enterprise features

Choose one of the following authentication methods:

Obtain an API key from the Google AI Studio

Set the API key as an environment variable or in your application properties

Install the gcloud CLI, appropriate for your OS.

Authenticate by running the following command. Replace PROJECT_ID with your Google Cloud project ID and ACCOUNT with your Google Cloud username.

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Google GenAI Embedding Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The prefix spring.ai.google.genai.embedding is used as the property prefix that lets you connect to Google GenAI Embedding API.

The connection properties are shared with the Google GenAI Chat module. If you’re using both chat and embeddings, you only need to configure the connection once using either spring.ai.google.genai prefix (for chat) or spring.ai.google.genai.embedding prefix (for embeddings).

spring.ai.google.genai.embedding.api-key

API key for Gemini Developer API. When provided, the client uses the Gemini Developer API instead of Vertex AI.

spring.ai.google.genai.embedding.project-id

Google Cloud Platform project ID (required for Vertex AI mode)

spring.ai.google.genai.embedding.location

Google Cloud region (required for Vertex AI mode)

spring.ai.google.genai.embedding.credentials-uri

URI to Google Cloud credentials. When provided it is used to create a GoogleCredentials instance for authentication.

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding.text=google-genai (It is enabled by default)

To disable, spring.ai.model.embedding.text=none (or any value which doesn’t match google-genai)

This change is done to allow configuration of multiple models.

The prefix spring.ai.google.genai.embedding.text is the property prefix that lets you configure the embedding model implementation for Google GenAI Text Embedding.

spring.ai.model.embedding.text

Enable Google GenAI Embedding API model.

spring.ai.google.genai.embedding.text.options.model

The Google GenAI Text Embedding model to use. Supported models include text-embedding-004 and text-multilingual-embedding-002

spring.ai.google.genai.embedding.text.options.task-type

The intended downstream application to help the model produce better quality embeddings. Available task-types: RETRIEVAL_QUERY, RETRIEVAL_DOCUMENT, SEMANTIC_SIMILARITY, CLASSIFICATION, CLUSTERING, QUESTION_ANSWERING, FACT_VERIFICATION

spring.ai.google.genai.embedding.text.options.title

Optional title, only valid with task_type=RETRIEVAL_DOCUMENT.

spring.ai.google.genai.embedding.text.options.dimensions

The number of dimensions the resulting output embeddings should have. Supported for model version 004 and later. You can use this parameter to reduce the embedding size, for example, for storage optimization.

spring.ai.google.genai.embedding.text.options.auto-truncate

When set to true, input text will be truncated. When set to false, an error is returned if the input text is longer than the maximum length supported by the model.

Create a new Spring Boot project and add the spring-ai-starter-model-google-genai-embedding to your pom (or gradle) dependencies.

Add a application.properties file, under the src/main/resources directory, to enable and configure the Google GenAI embedding model:

This will create a GoogleGenAiTextEmbeddingModel implementation that you can inject into your class. Here is an example of a simple @Controller class that uses the embedding model for embeddings generations.

The GoogleGenAiTextEmbeddingModel implements the EmbeddingModel.

Add the spring-ai-google-genai-embedding dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Next, create a GoogleGenAiTextEmbeddingModel and use it for text embeddings:

The Google GenAI embeddings API supports different task types to optimize embeddings for specific use cases:

RETRIEVAL_QUERY: Optimized for search queries in retrieval systems

RETRIEVAL_DOCUMENT: Optimized for documents in retrieval systems

SEMANTIC_SIMILARITY: Optimized for measuring semantic similarity between texts

CLASSIFICATION: Optimized for text classification tasks

CLUSTERING: Optimized for clustering similar texts

QUESTION_ANSWERING: Optimized for question-answering systems

FACT_VERIFICATION: Optimized for fact verification tasks

Example of using different task types:

For model version 004 and later, you can reduce the embedding dimensions for storage optimization:

If you’re currently using the Vertex AI Text Embeddings implementation (spring-ai-vertex-ai-embedding), you can migrate to Google GenAI with minimal changes:

SDK: Google GenAI uses the new com.google.genai.Client instead of Vertex AI SDK

Authentication: Supports both API key and Google Cloud credentials

Package Names: Classes are in org.springframework.ai.google.genai.text instead of org.springframework.ai.vertexai.embedding

Property Prefix: Uses spring.ai.google.genai.embedding instead of spring.ai.vertex.ai.embedding

Connection Details: Uses GoogleGenAiEmbeddingConnectionDetails instead of VertexAiEmbeddingConnectionDetails

Use Google GenAI Embeddings when: - You want quick prototyping with API keys - You need the latest embedding features from the Developer API - You want flexibility to switch between API key and Vertex AI modes - You’re already using Google GenAI for chat

Use Vertex AI Text Embeddings when: - You have existing Vertex AI infrastructure - You need multimodal embeddings (currently only available in Vertex AI) - Your organization requires Google Cloud-only deployment

**Examples:**

Example 1 (typescript):
```typescript
gcloud config set project <PROJECT_ID> &&
gcloud auth application-default login <ACCOUNT>
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-google-genai-embedding</artifactId>
</dependency>
```

Example 3 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-google-genai-embedding'
}
```

Example 4 (unknown):
```unknown
spring.ai.google.genai.embedding.api-key=YOUR_API_KEY
spring.ai.google.genai.embedding.text.options.model=text-embedding-004
```

---

## Mistral AI Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/embeddings/mistralai-embeddings.html

**Contents:**
- Mistral AI Embeddings
- Available Models
- Prerequisites
  - Add Repositories and BOM
- Auto-configuration
  - Embedding Properties
    - Retry Properties
    - Connection Properties
    - Configuration Properties
- Runtime Options

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI supports the Mistral AI’s text embeddings models. Embeddings are vectorial representations of text that capture the semantic meaning of paragraphs through their position in a high dimensional vector space. Mistral AI Embeddings API offers cutting-edge, state-of-the-art embeddings for text, which can be used for many NLP tasks.

Mistral AI provides two embedding models, each optimized for different use cases:

General-purpose embedding model suitable for semantic search, clustering, and text similarity tasks. Ideal for natural language content.

Specialized embedding model optimized for code similarity, code search, and retrieval-augmented generation (RAG) with code repositories. Provides higher-dimensional embeddings specifically designed for understanding code semantics.

When choosing a model:

Use mistral-embed for general text content such as documents, articles, or user queries

Use codestral-embed when working with code, technical documentation, or building code-aware RAG systems

You will need to create an API with MistralAI to access MistralAI embeddings models.

Create an account at MistralAI registration page and generate the token on the API Keys page.

The Spring AI project defines a configuration property named spring.ai.mistralai.api-key that you should set to the value of the API Key obtained from console.mistral.ai.

You can set this configuration property in your application.properties file:

For enhanced security when handling sensitive information like API keys, you can use Spring Expression Language (SpEL) to reference an environment variable:

You can also set this configuration programmatically in your application code:

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the MistralAI Embedding Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The prefix spring.ai.retry is used as the property prefix that lets you configure the retry mechanism for the Mistral AI Embedding model.

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

The prefix spring.ai.mistralai is used as the property prefix that lets you connect to MistralAI.

spring.ai.mistralai.base-url

The URL to connect to

spring.ai.mistralai.api-key

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=mistral (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match mistral)

This change is done to allow configuration of multiple models.

The prefix spring.ai.mistralai.embedding is property prefix that configures the EmbeddingModel implementation for MistralAI.

spring.ai.mistralai.embedding.enabled (Removed and no longer valid)

Enable OpenAI embedding model.

spring.ai.model.embedding

Enable OpenAI embedding model.

spring.ai.mistralai.embedding.base-url

Optional overrides the spring.ai.mistralai.base-url to provide embedding specific url

spring.ai.mistralai.embedding.api-key

Optional overrides the spring.ai.mistralai.api-key to provide embedding specific api-key

spring.ai.mistralai.embedding.metadata-mode

Document content extraction mode.

spring.ai.mistralai.embedding.options.model

spring.ai.mistralai.embedding.options.encodingFormat

The format to return the embeddings in. Can be either float or base64.

The MistralAiEmbeddingOptions.java provides the MistralAI configurations, such as the model to use and etc.

The default options can be configured using the spring.ai.mistralai.embedding.options properties as well.

At start-time use the MistralAiEmbeddingModel constructor to set the default options used for all embedding requests. At run-time you can override the default options, using a MistralAiEmbeddingOptions instance as part of your EmbeddingRequest.

For example to override the default model name for a specific request:

This will create a EmbeddingModel implementation that you can inject into your class. Here is an example of a simple @Controller class that uses the EmbeddingModel implementation.

If you are not using Spring Boot, you can manually configure the OpenAI Embedding Model. For this add the spring-ai-mistral-ai dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Next, create an MistralAiEmbeddingModel instance and use it to compute the similarity between two input texts:

The MistralAiEmbeddingOptions provides the configuration information for the embedding requests. The options class offers a builder() for easy options creation.

**Examples:**

Example 1 (unknown):
```unknown
spring.ai.mistralai.api-key=<your-mistralai-api-key>
```

Example 2 (yaml):
```yaml
# In application.yml
spring:
  ai:
    mistralai:
      api-key: ${MISTRALAI_API_KEY}
```

Example 3 (bash):
```bash
# In your environment or .env file
export MISTRALAI_API_KEY=<your-mistralai-api-key>
```

Example 4 (java):
```java
// Retrieve API key from a secure source or environment variable
String apiKey = System.getenv("MISTRALAI_API_KEY");
```

---

## PostgresML Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/embeddings/postgresml-embeddings.html

**Contents:**
- PostgresML Embeddings
- Add Repositories and BOM
- Auto-configuration
  - Embedding Properties
- Runtime Options
- Sample Controller
- Manual configuration

For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI supports the PostgresML text embeddings models.

Embeddings are a numeric representation of text. They are used to represent words and sentences as vectors, an array of numbers. Embeddings can be used to find similar pieces of text, by comparing the similarity of the numeric vectors using a distance measure, or they can be used as input features for other machine learning models, since most algorithms can’t use text directly.

Many pre-trained LLMs can be used to generate embeddings from text within PostgresML. You can browse all the models available to find the best solution on Hugging Face.

Spring AI artifacts are published in Maven Central and Spring Snapshot repositories. Refer to the Artifact Repositories section to add these repositories to your build system.

To help with dependency management, Spring AI provides a BOM (bill of materials) to ensure that a consistent version of Spring AI is used throughout the entire project. Refer to the Dependency Management section to add the Spring AI BOM to your build system.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Azure PostgresML Embedding Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Use the spring.ai.postgresml.embedding.options.* properties to configure your PostgresMlEmbeddingModel. links

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=postgresml (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match postgresml)

This change is done to allow configuration of multiple models.

The prefix spring.ai.postgresml.embedding is property prefix that configures the EmbeddingModel implementation for PostgresML embeddings.

spring.ai.postgresml.embedding.enabled (Removed and no longer valid)

Enable PostgresML embedding model.

spring.ai.model.embedding

Enable PostgresML embedding model.

spring.ai.postgresml.embedding.create-extension

Execute the SQL 'CREATE EXTENSION IF NOT EXISTS pgml' to enable the extesnion

spring.ai.postgresml.embedding.options.transformer

The Hugging Face transformer model to use for the embedding.

distilbert-base-uncased

spring.ai.postgresml.embedding.options.kwargs

Additional transformer specific options.

spring.ai.postgresml.embedding.options.vectorType

PostgresML vector type to use for the embedding. Two options are supported: PG_ARRAY and PG_VECTOR.

spring.ai.postgresml.embedding.options.metadataMode

Document metadata aggregation mode

Use the PostgresMlEmbeddingOptions.java to configure the PostgresMlEmbeddingModel with options, such as the model to use and etc.

On start you can pass a PostgresMlEmbeddingOptions to the PostgresMlEmbeddingModel constructor to configure the default options used for all embedding requests.

At run-time you can override the default options, using a PostgresMlEmbeddingOptions in your EmbeddingRequest.

For example to override the default model name for a specific request:

This will create a EmbeddingModel implementation that you can inject into your class. Here is an example of a simple @Controller class that uses the EmbeddingModel implementation.

Instead of using the Spring Boot auto-configuration, you can create the PostgresMlEmbeddingModel manually. For this add the spring-ai-postgresml dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Next, create an PostgresMlEmbeddingModel instance and use it to compute the similarity between two input texts:

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-postgresml-embedding</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-postgresml-embedding'
}
```

Example 3 (java):
```java
EmbeddingResponse embeddingResponse = embeddingModel.call(
    new EmbeddingRequest(List.of("Hello World", "World is big and salvation is near"),
            PostgresMlEmbeddingOptions.builder()
                .transformer("intfloat/e5-small")
                .vectorType(VectorType.PG_ARRAY)
                .kwargs(Map.of("device", "gpu"))
                .build()));
```

Example 4 (unknown):
```unknown
spring.ai.postgresml.embedding.options.transformer=distilbert-base-uncased
spring.ai.postgresml.embedding.options.vectorType=PG_ARRAY
spring.ai.postgresml.embedding.options.metadataMode=EMBED
spring.ai.postgresml.embedding.options.kwargs.device=cpu
```

---

## Transformers (ONNX) Embeddings :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/embeddings/onnx.html

**Contents:**
- Transformers (ONNX) Embeddings
- Prerequisites
- Auto-configuration
  - Embedding Properties
  - Errors and special cases
- Manual Configuration

For the latest snapshot version, please use Spring AI 1.1.2!

The TransformersEmbeddingModel is an EmbeddingModel implementation that locally computes sentence embeddings using a selected sentence transformer.

You can use any HuggingFace Embedding model.

It uses pre-trained transformer models, serialized into the Open Neural Network Exchange (ONNX) format.

The Deep Java Library and the Microsoft ONNX Java Runtime libraries are applied to run the ONNX models and compute the embeddings in Java.

To run things in Java, we need to serialize the Tokenizer and the Transformer Model into ONNX format.

Serialize with optimum-cli - One, quick, way to achieve this, is to use the optimum-cli command line tool. The following snippet prepares a python virtual environment, installs the required packages and serializes (e.g. exports) the specified model using optimum-cli :

The snippet exports the sentence-transformers/all-MiniLM-L6-v2 transformer into the onnx-output-folder folder. The latter includes the tokenizer.json and model.onnx files used by the embedding model.

In place of the all-MiniLM-L6-v2 you can pick any huggingface transformer identifier or provide direct file path.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the ONNX Transformer Embedding Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

To configure it, use the spring.ai.embedding.transformer.* properties.

For example, add this to your application.properties file to configure the client with the intfloat/e5-small-v2 text embedding model:

The complete list of supported properties are:

Enabling and disabling of the embedding auto-configurations are now configured via top level properties with the prefix spring.ai.model.embedding.

To enable, spring.ai.model.embedding=transformers (It is enabled by default)

To disable, spring.ai.model.embedding=none (or any value which doesn’t match transformers)

This change is done to allow configuration of multiple models.

spring.ai.embedding.transformer.enabled (Removed and no longer valid)

Enable the Transformer Embedding model.

spring.ai.model.embedding

Enable the Transformer Embedding model.

spring.ai.embedding.transformer.tokenizer.uri

URI of a pre-trained HuggingFaceTokenizer created by the ONNX engine (e.g. tokenizer.json).

onnx/all-MiniLM-L6-v2/tokenizer.json

spring.ai.embedding.transformer.tokenizer.options

HuggingFaceTokenizer options such as ‘addSpecialTokens’, ‘modelMaxLength’, ‘truncation’, ‘padding’, ‘maxLength’, ‘stride’, ‘padToMultipleOf’. Leave empty to fallback to the defaults.

spring.ai.embedding.transformer.cache.enabled

Enable remote Resource caching.

spring.ai.embedding.transformer.cache.directory

Directory path to cache remote resources, such as the ONNX models

${java.io.tmpdir}/spring-ai-onnx-model

spring.ai.embedding.transformer.onnx.modelUri

Existing, pre-trained ONNX model.

onnx/all-MiniLM-L6-v2/model.onnx

spring.ai.embedding.transformer.onnx.modelOutputName

The ONNX model’s output node name, which we’ll use for embedding calculation.

spring.ai.embedding.transformer.onnx.gpuDeviceId

The GPU device ID to execute on. Only applicable if >= 0. Ignored otherwise.(Requires additional onnxruntime_gpu dependency)

spring.ai.embedding.transformer.metadataMode

Specifies what parts of the Documents content and metadata will be used for computing the embeddings.

If you see an error like Caused by: ai.onnxruntime.OrtException: Supplied array is ragged,.., you need to also enable the tokenizer padding in application.properties as follows:

If you get an error like The generative output names don’t contain expected: last_hidden_state. Consider one of the available model outputs: token_embeddings, …​., you need to set the model output name to a correct value per your models. Consider the names listed in the error message. For example:

If you get an error like ai.onnxruntime.OrtException: Error code - ORT_FAIL - message: Deserialize tensor onnx::MatMul_10319 failed.GetFileLength for ./model.onnx_data failed:Invalid fd was supplied: -1, that means that you model is larger than 2GB and is serialized in two files: model.onnx and model.onnx_data.

The model.onnx_data is called External Data and is expected to be under the same directory of the model.onnx.

Currently the only workaround is to copy the large model.onnx_data in the folder you run your Boot application.

If you get an error like ai.onnxruntime.OrtException: Error code - ORT_EP_FAIL - message: Failed to find CUDA shared provider, that means that you are using the GPU parameters spring.ai.embedding.transformer.onnx.gpuDeviceId , but the onnxruntime_gpu dependency are missing.

Please select the appropriate onnxruntime_gpu version based on the CUDA version(ONNX Java Runtime).

If you are not using Spring Boot, you can manually configure the Onnx Transformers Embedding Model. For this add the spring-ai-transformers dependency to your project’s Maven pom.xml file:

then create a new TransformersEmbeddingModel instance and use the setTokenizerResource(tokenizerJsonUri) and setModelResource(modelOnnxUri) methods to set the URIs of the exported tokenizer.json and model.onnx files. (classpath:, file: or https: URI schemas are supported).

If the model is not explicitly set, TransformersEmbeddingModel defaults to sentence-transformers/all-MiniLM-L6-v2:

The following snippet illustrates how to use the TransformersEmbeddingModel manually:

The first embed() call downloads the large ONNX model and caches it on the local file system. Therefore, the first call might take longer than usual. Use the #setResourceCacheDirectory(<path>) method to set the local folder where the ONNX models as stored. The default cache folder is ${java.io.tmpdir}/spring-ai-onnx-model.

It is more convenient (and preferred) to create the TransformersEmbeddingModel as a Bean. Then you don’t have to call the afterPropertiesSet() manually.

**Examples:**

Example 1 (bash):
```bash
python3 -m venv venv
source ./venv/bin/activate
(venv) pip install --upgrade pip
(venv) pip install optimum onnx onnxruntime sentence-transformers
(venv) optimum-cli export onnx --model sentence-transformers/all-MiniLM-L6-v2 onnx-output-folder
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-transformers</artifactId>
</dependency>
```

Example 3 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-transformers'
}
```

Example 4 (xml):
```xml
<dependency>
  <groupId>org.springframework.ai</groupId>
  <artifactId>spring-ai-transformers</artifactId>
</dependency>
```

---
