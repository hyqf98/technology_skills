# Springai - Observability

**Pages:** 3

---

## Observability :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/observability/index.html

**Contents:**
- Observability
- Chat Client
  - Prompt Content
  - Input Data (Deprecated)
  - Chat Client Advisors
- Chat Model
  - Chat Prompt and Completion Data
- Tool Calling
  - Tool Call Arguments and Result Data
- EmbeddingModel

For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI builds upon the observability features in the Spring ecosystem to provide insights into AI-related operations. Spring AI provides metrics and tracing capabilities for its core components: ChatClient (including Advisor), ChatModel, EmbeddingModel, ImageModel, and VectorStore.

1.0.0-RC1 Breaking Changes

Following configuration properties have been renamed to better reflect their purpose:

spring.ai.chat.client.observations.include-prompt → spring.ai.chat.client.observations.log-prompt

spring.ai.chat.observations.include-prompt → spring.ai.chat.observations.log-prompt

spring.ai.chat.observations.include-completion → spring.ai.chat.observations.log-completion

spring.ai.image.observations.include-prompt → spring.ai.image.observations.log-prompt

spring.ai.vectorstore.observations.include-query-response → spring.ai.vectorstore.observations.log-query-response

The spring.ai.chat.client observations are recorded when a ChatClient call() or stream() operations are invoked. They measure the time spent performing the invocation and propagate the related tracing information.

gen_ai.operation.name

spring.ai.chat.client.stream

Is the chat model response a stream - true or false

The kind of framework API in Spring AI: chat_client.

The content of the prompt sent via the chat client. Optional.

spring.ai.chat.client.advisor.params (deprecated)

Map of advisor parameters. The conversation ID is now included in spring.ai.chat.client.conversation.id.

spring.ai.chat.client.advisors

List of configured chat client advisors.

spring.ai.chat.client.conversation.id

Identifier of the conversation when using the chat memory.

spring.ai.chat.client.system.params (deprecated)

Chat client system parameters. Optional. Superseded by gen_ai.prompt.

spring.ai.chat.client.system.text (deprecated)

Chat client system text. Optional. Superseded by gen_ai.prompt.

spring.ai.chat.client.tool.function.names (deprecated)

Enabled tool function names. Superseded by spring.ai.chat.client.tool.names.

spring.ai.chat.client.tool.function.callbacks (deprecated)

List of configured chat client function callbacks. Superseded by spring.ai.chat.client.tool.names.

spring.ai.chat.client.tool.names

Names of the tools passed to the chat client.

spring.ai.chat.client.user.params (deprecated)

Chat client user parameters. Optional. Superseded by gen_ai.prompt.

spring.ai.chat.client.user.text (deprecated)

Chat client user text. Optional. Superseded by gen_ai.prompt.

The ChatClient prompt content is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging the prompt content to help with debugging and troubleshooting.

spring.ai.chat.client.observations.log-prompt

Whether to log the chat client prompt content.

The ChatClient input data is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging input data to help with debugging and troubleshooting.

spring.ai.chat.client.observations.include-input

Whether to include the input content in the observations.

The spring.ai.advisor observations are recorded when an advisor is executed. They measure the time spent in the advisor (including the time spend on the inner advisors) and propagate the related tracing information.

gen_ai.operation.name

spring.ai.advisor.type (deprecated)

Where the advisor applies it’s logic in the request processing, one of BEFORE, AFTER, or AROUND. This distinction doesn’t apply anymore since all Advisors are always of the same type.

The kind of framework API in Spring AI: advisor.

spring.ai.advisor.name

spring.ai.advisor.order

Advisor order in the advisor chain.

The gen_ai.client.operation observations are recorded when calling the ChatModel call or stream methods. They measure the time spent on method completion and propagate the related tracing information.

gen_ai.operation.name

The name of the operation being performed.

The model provider as identified by the client instrumentation.

The name of the model a request is being made to.

gen_ai.response.model

The name of the model that generated the response.

gen_ai.request.frequency_penalty

The frequency penalty setting for the model request.

gen_ai.request.max_tokens

The maximum number of tokens the model generates for a request.

gen_ai.request.presence_penalty

The presence penalty setting for the model request.

gen_ai.request.stop_sequences

List of sequences that the model will use to stop generating further tokens.

gen_ai.request.temperature

The temperature setting for the model request.

The top_k sampling setting for the model request.

The top_p sampling setting for the model request.

gen_ai.response.finish_reasons

Reasons the model stopped generating tokens, corresponding to each generation received.

The unique identifier for the AI response.

gen_ai.usage.input_tokens

The number of tokens used in the model input (prompt).

gen_ai.usage.output_tokens

The number of tokens used in the model output (completion).

gen_ai.usage.total_tokens

The total number of tokens used in the model exchange.

The full prompt sent to the model. Optional.

The full response received from the model. Optional.

spring.ai.model.request.tool.names

List of tool definitions provided to the model in the request.

The chat prompt and completion data is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging chat prompt and completion data, useful for troubleshooting scenarios. When tracing is available, the logs will include trace information for better correlation.

spring.ai.chat.observations.log-prompt

Log the prompt content. true or false

spring.ai.chat.observations.log-completion

Log the completion content. true or false

spring.ai.chat.observations.include-error-logging

Include error logging in observations. true or false

The spring.ai.tool observations are recorded when performing tool calling in the context of a chat model interaction. They measure the time spent on toll call completion and propagate the related tracing information.

gen_ai.operation.name

The name of the operation being performed. It’s always framework.

The provider responsible for the operation. It’s always spring_ai.

The kind of operation performed by Spring AI. It’s always tool_call.

spring.ai.tool.definition.name

The name of the tool.

spring.ai.tool.definition.description

Description of the tool.

spring.ai.tool.definition.schema

Schema of the parameters used to call the tool.

spring.ai.tool.call.arguments

The input arguments to the tool call. (Only when enabled)

spring.ai.tool.call.result

Schema of the parameters used to call the tool. (Only when enabled)

The input arguments and result from the tool call are not exported by default, as they can be potentially sensitive.

Spring AI supports exporting tool call arguments and result data as span attributes.

spring.ai.tools.observations.include-content

Include the tool call content in observations. true or false

The gen_ai.client.operation observations are recorded on embedding model method calls. They measure the time spent on method completion and propagate the related tracing information.

gen_ai.operation.name

The name of the operation being performed.

The model provider as identified by the client instrumentation.

The name of the model a request is being made to.

gen_ai.response.model

The name of the model that generated the response.

gen_ai.request.embedding.dimensions

The number of dimensions the resulting output embeddings have.

gen_ai.usage.input_tokens

The number of tokens used in the model input.

gen_ai.usage.total_tokens

The total number of tokens used in the model exchange.

The gen_ai.client.operation observations are recorded on image model method calls. They measure the time spent on method completion and propagate the related tracing information.

gen_ai.operation.name

The name of the operation being performed.

The model provider as identified by the client instrumentation.

The name of the model a request is being made to.

gen_ai.request.image.response_format

The format in which the generated image is returned.

gen_ai.request.image.size

The size of the image to generate.

gen_ai.request.image.style

The style of the image to generate.

The unique identifier for the AI response.

gen_ai.response.model

The name of the model that generated the response.

gen_ai.usage.input_tokens

The number of tokens used in the model input (prompt).

gen_ai.usage.output_tokens

The number of tokens used in the model output (generation).

gen_ai.usage.total_tokens

The total number of tokens used in the model exchange.

The full prompt sent to the model. Optional.

The image prompt data is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging image prompt data, useful for troubleshooting scenarios. When tracing is available, the logs will include trace information for better correlation.

spring.ai.image.observations.log-prompt

Log the image prompt content. true or false

All vector store implementations in Spring AI are instrumented to provide metrics and distributed tracing data through Micrometer.

The db.vector.client.operation observations are recorded when interacting with the Vector Store. They measure the time spent on the query, add and remove operations and propagate the related tracing information.

The name of the operation or command being executed. One of add, delete, or query.

The database management system (DBMS) product as identified by the client instrumentation. One of pg_vector, azure, cassandra, chroma, elasticsearch, milvus, neo4j, opensearch, qdrant, redis, typesense, weaviate, pinecone, oracle, mongodb, gemfire, hana, simple.

The kind of framework API in Spring AI: vector_store.

The name of a collection (table, container) within the database.

The name of the database, fully qualified within the server address and port.

The record identifier if present.

db.search.similarity_metric

The metric used in similarity search.

db.vector.dimension_count

The dimension of the vector.

The name field as of the vector (e.g. a field name).

db.vector.query.content

The content of the search query being executed.

db.vector.query.filter

The metadata filters used in the search query.

db.vector.query.response.documents

Returned documents from a similarity search query. Optional.

db.vector.query.similarity_threshold

Similarity threshold that accepts all search scores. A threshold value of 0.0 means any similarity is accepted or disable the similarity threshold filtering. A threshold value of 1.0 means an exact match is required.

db.vector.query.top_k

The top-k most similar vectors returned by a query.

The vector search response data is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging vector search response data, useful for troubleshooting scenarios. When tracing is available, the logs will include trace information for better correlation.

spring.ai.vectorstore.observations.log-query-response

Log the vector store query response content. true or false

This section documents the metrics emitted by Spring AI components as they appear in Prometheus.

Spring AI uses Micrometer. Base metric names use dots (e.g., gen_ai.client.operation), which Prometheus exports with underscores and standard suffixes:

Timers → <base>_seconds_count, <base>_seconds_sum, <base>_seconds_max, and (when supported) <base>_active_count

Counters → <base>_total (monotonic)

The following shows how base metric names expand to Prometheus time series.

gen_ai.client.operation

gen_ai_client_operation_seconds_count gen_ai_client_operation_seconds_sum gen_ai_client_operation_seconds_max gen_ai_client_operation_active_count

db.vector.client.operation

db_vector_client_operation_seconds_count db_vector_client_operation_seconds_sum db_vector_client_operation_seconds_max db_vector_client_operation_active_count

OpenTelemetry — Semantic Conventions for Generative AI (overview)

Micrometer — Naming Meters

gen_ai_chat_client_operation_seconds_sum

Total time spent in ChatClient operations (call/stream)

gen_ai_chat_client_operation_seconds_count

Number of completed ChatClient operations

gen_ai_chat_client_operation_seconds_max

Maximum observed duration of ChatClient operations

gen_ai_chat_client_operation_active_count

Number of ChatClient operations currently in flight

Active vs Completed: active_count shows in-flight calls; the _seconds series reflect only completed calls.

gen_ai_client_operation_seconds_sum

Total time executing chat model operations

gen_ai_client_operation_seconds_count

Number of completed chat model operations

gen_ai_client_operation_seconds_max

Maximum observed duration for chat model operations

gen_ai_client_operation_active_count

Number of chat model operations currently in flight

gen_ai_client_token_usage_total

Total tokens consumed, labeled by token type

gen_ai_token_type=input

Prompt tokens sent to the model

gen_ai_token_type=output

Completion tokens returned by the model

gen_ai_token_type=total

db_vector_client_operation_seconds_sum

Total time spent in vector store operations (add/delete/query)

db_vector_client_operation_seconds_count

Number of completed vector store operations

db_vector_client_operation_seconds_max

Maximum observed duration for vector store operations

db_vector_client_operation_active_count

Number of vector store operations currently in flight

Operation type (add, delete, query)

Vector DB/provider (redis, chroma, pgvector, …)

Active (*_active_count) — instantaneous gauge of in-progress operations (concurrency/load).

Completed (*_seconds_sum|count|max) — statistics for operations that have finished:

_seconds_sum / _seconds_count → average latency

_seconds_max → high-water mark since last scrape (subject to registry behavior)

---

## Observability :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/observability/index.html

**Contents:**
- Observability
- Chat Client
  - Prompt and Completion Data
  - Input Data (Deprecated)
  - Chat Client Advisors
- Chat Model
  - Chat Prompt and Completion Data
- Tool Calling
  - Tool Call Arguments and Result Data
- EmbeddingModel

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI builds upon the observability features in the Spring ecosystem to provide insights into AI-related operations.

The spring-boot-actuator module is required for enabling observability. Add the Spring Boot Actuator dependency to your project’s Maven pom.xml build file:

or to your Gradle build.gradle build file.

Spring AI provides metrics and tracing capabilities for its core components: ChatClient (including Advisor), ChatModel, EmbeddingModel, ImageModel, and VectorStore.

1.0.0-RC1 Breaking Changes

Following configuration properties have been renamed to better reflect their purpose:

spring.ai.chat.client.observations.include-prompt → spring.ai.chat.client.observations.log-prompt

spring.ai.chat.observations.include-prompt → spring.ai.chat.observations.log-prompt

spring.ai.chat.observations.include-completion → spring.ai.chat.observations.log-completion

spring.ai.image.observations.include-prompt → spring.ai.image.observations.log-prompt

spring.ai.vectorstore.observations.include-query-response → spring.ai.vectorstore.observations.log-query-response

The spring.ai.chat.client observations are recorded when a ChatClient call() or stream() operations are invoked. They measure the time spent performing the invocation and propagate the related tracing information.

gen_ai.operation.name

spring.ai.chat.client.stream

Is the chat model response a stream - true or false

The kind of framework API in Spring AI: chat_client.

The content of the prompt sent via the chat client. Optional.

spring.ai.chat.client.advisor.params (deprecated)

Map of advisor parameters. The conversation ID is now included in spring.ai.chat.client.conversation.id.

spring.ai.chat.client.advisors

List of configured chat client advisors.

spring.ai.chat.client.conversation.id

Identifier of the conversation when using the chat memory.

spring.ai.chat.client.system.params (deprecated)

Chat client system parameters. Optional. Superseded by gen_ai.prompt.

spring.ai.chat.client.system.text (deprecated)

Chat client system text. Optional. Superseded by gen_ai.prompt.

spring.ai.chat.client.tool.function.names (deprecated)

Enabled tool function names. Superseded by spring.ai.chat.client.tool.names.

spring.ai.chat.client.tool.function.callbacks (deprecated)

List of configured chat client function callbacks. Superseded by spring.ai.chat.client.tool.names.

spring.ai.chat.client.tool.names

Names of the tools passed to the chat client.

spring.ai.chat.client.user.params (deprecated)

Chat client user parameters. Optional. Superseded by gen_ai.prompt.

spring.ai.chat.client.user.text (deprecated)

Chat client user text. Optional. Superseded by gen_ai.prompt.

The ChatClient prompt and completion data is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging the prompt and completion data to help with debugging and troubleshooting.

spring.ai.chat.client.observations.log-prompt

Whether to log the chat client prompt content.

spring.ai.chat.client.observations.log-completion

Whether to log the chat client completion content.

The ChatClient input data is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging input data to help with debugging and troubleshooting.

spring.ai.chat.client.observations.include-input

Whether to include the input content in the observations.

The spring.ai.advisor observations are recorded when an advisor is executed. They measure the time spent in the advisor (including the time spend on the inner advisors) and propagate the related tracing information.

gen_ai.operation.name

spring.ai.advisor.type (deprecated)

Where the advisor applies it’s logic in the request processing, one of BEFORE, AFTER, or AROUND. This distinction doesn’t apply anymore since all Advisors are always of the same type.

The kind of framework API in Spring AI: advisor.

spring.ai.advisor.name

spring.ai.advisor.order

Advisor order in the advisor chain.

The gen_ai.client.operation observations are recorded when calling the ChatModel call or stream methods. They measure the time spent on method completion and propagate the related tracing information.

gen_ai.operation.name

The name of the operation being performed.

The model provider as identified by the client instrumentation.

The name of the model a request is being made to.

gen_ai.response.model

The name of the model that generated the response.

gen_ai.request.frequency_penalty

The frequency penalty setting for the model request.

gen_ai.request.max_tokens

The maximum number of tokens the model generates for a request.

gen_ai.request.presence_penalty

The presence penalty setting for the model request.

gen_ai.request.stop_sequences

List of sequences that the model will use to stop generating further tokens.

gen_ai.request.temperature

The temperature setting for the model request.

The top_k sampling setting for the model request.

The top_p sampling setting for the model request.

gen_ai.response.finish_reasons

Reasons the model stopped generating tokens, corresponding to each generation received.

The unique identifier for the AI response.

gen_ai.usage.input_tokens

The number of tokens used in the model input (prompt).

gen_ai.usage.output_tokens

The number of tokens used in the model output (completion).

gen_ai.usage.total_tokens

The total number of tokens used in the model exchange.

The full prompt sent to the model. Optional.

The full response received from the model. Optional.

spring.ai.model.request.tool.names

List of tool definitions provided to the model in the request.

The chat prompt and completion data is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging chat prompt and completion data, useful for troubleshooting scenarios. When tracing is available, the logs will include trace information for better correlation.

spring.ai.chat.observations.log-prompt

Log the prompt content. true or false

spring.ai.chat.observations.log-completion

Log the completion content. true or false

spring.ai.chat.observations.include-error-logging

Include error logging in observations. true or false

The spring.ai.tool observations are recorded when performing tool calling in the context of a chat model interaction. They measure the time spent on toll call completion and propagate the related tracing information.

gen_ai.operation.name

The name of the operation being performed. It’s always framework.

The provider responsible for the operation. It’s always spring_ai.

The kind of operation performed by Spring AI. It’s always tool_call.

spring.ai.tool.definition.name

The name of the tool.

spring.ai.tool.definition.description

Description of the tool.

spring.ai.tool.definition.schema

Schema of the parameters used to call the tool.

spring.ai.tool.call.arguments

The input arguments to the tool call. (Only when enabled)

spring.ai.tool.call.result

Schema of the parameters used to call the tool. (Only when enabled)

The input arguments and result from the tool call are not exported by default, as they can be potentially sensitive.

Spring AI supports exporting tool call arguments and result data as span attributes.

spring.ai.tools.observations.include-content

Include the tool call content in observations. true or false

The gen_ai.client.operation observations are recorded on embedding model method calls. They measure the time spent on method completion and propagate the related tracing information.

gen_ai.operation.name

The name of the operation being performed.

The model provider as identified by the client instrumentation.

The name of the model a request is being made to.

gen_ai.response.model

The name of the model that generated the response.

gen_ai.request.embedding.dimensions

The number of dimensions the resulting output embeddings have.

gen_ai.usage.input_tokens

The number of tokens used in the model input.

gen_ai.usage.total_tokens

The total number of tokens used in the model exchange.

The gen_ai.client.operation observations are recorded on image model method calls. They measure the time spent on method completion and propagate the related tracing information.

gen_ai.operation.name

The name of the operation being performed.

The model provider as identified by the client instrumentation.

The name of the model a request is being made to.

gen_ai.request.image.response_format

The format in which the generated image is returned.

gen_ai.request.image.size

The size of the image to generate.

gen_ai.request.image.style

The style of the image to generate.

The unique identifier for the AI response.

gen_ai.response.model

The name of the model that generated the response.

gen_ai.usage.input_tokens

The number of tokens used in the model input (prompt).

gen_ai.usage.output_tokens

The number of tokens used in the model output (generation).

gen_ai.usage.total_tokens

The total number of tokens used in the model exchange.

The full prompt sent to the model. Optional.

The image prompt data is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging image prompt data, useful for troubleshooting scenarios. When tracing is available, the logs will include trace information for better correlation.

spring.ai.image.observations.log-prompt

Log the image prompt content. true or false

All vector store implementations in Spring AI are instrumented to provide metrics and distributed tracing data through Micrometer.

The db.vector.client.operation observations are recorded when interacting with the Vector Store. They measure the time spent on the query, add and remove operations and propagate the related tracing information.

The name of the operation or command being executed. One of add, delete, or query.

The database management system (DBMS) product as identified by the client instrumentation. One of pg_vector, azure, cassandra, chroma, elasticsearch, milvus, neo4j, opensearch, qdrant, redis, typesense, weaviate, pinecone, oracle, mongodb, gemfire, hana, simple.

The kind of framework API in Spring AI: vector_store.

The name of a collection (table, container) within the database.

The name of the database, fully qualified within the server address and port.

The record identifier if present.

db.search.similarity_metric

The metric used in similarity search.

db.vector.dimension_count

The dimension of the vector.

The name field as of the vector (e.g. a field name).

db.vector.query.content

The content of the search query being executed.

db.vector.query.filter

The metadata filters used in the search query.

db.vector.query.response.documents

Returned documents from a similarity search query. Optional.

db.vector.query.similarity_threshold

Similarity threshold that accepts all search scores. A threshold value of 0.0 means any similarity is accepted or disable the similarity threshold filtering. A threshold value of 1.0 means an exact match is required.

db.vector.query.top_k

The top-k most similar vectors returned by a query.

The vector search response data is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging vector search response data, useful for troubleshooting scenarios. When tracing is available, the logs will include trace information for better correlation.

spring.ai.vectorstore.observations.log-query-response

Log the vector store query response content. true or false

This section documents the metrics emitted by Spring AI components as they appear in Prometheus.

Spring AI uses Micrometer. Base metric names use dots (e.g., gen_ai.client.operation), which Prometheus exports with underscores and standard suffixes:

Timers → <base>_seconds_count, <base>_seconds_sum, <base>_seconds_max, and (when supported) <base>_active_count

Counters → <base>_total (monotonic)

The following shows how base metric names expand to Prometheus time series.

gen_ai.client.operation

gen_ai_client_operation_seconds_count gen_ai_client_operation_seconds_sum gen_ai_client_operation_seconds_max gen_ai_client_operation_active_count

db.vector.client.operation

db_vector_client_operation_seconds_count db_vector_client_operation_seconds_sum db_vector_client_operation_seconds_max db_vector_client_operation_active_count

OpenTelemetry — Semantic Conventions for Generative AI (overview)

Micrometer — Naming Meters

gen_ai_chat_client_operation_seconds_sum

Total time spent in ChatClient operations (call/stream)

gen_ai_chat_client_operation_seconds_count

Number of completed ChatClient operations

gen_ai_chat_client_operation_seconds_max

Maximum observed duration of ChatClient operations

gen_ai_chat_client_operation_active_count

Number of ChatClient operations currently in flight

Active vs Completed: active_count shows in-flight calls; the _seconds series reflect only completed calls.

gen_ai_client_operation_seconds_sum

Total time executing chat model operations

gen_ai_client_operation_seconds_count

Number of completed chat model operations

gen_ai_client_operation_seconds_max

Maximum observed duration for chat model operations

gen_ai_client_operation_active_count

Number of chat model operations currently in flight

gen_ai_client_token_usage_total

Total tokens consumed, labeled by token type

gen_ai_token_type=input

Prompt tokens sent to the model

gen_ai_token_type=output

Completion tokens returned by the model

gen_ai_token_type=total

db_vector_client_operation_seconds_sum

Total time spent in vector store operations (add/delete/query)

db_vector_client_operation_seconds_count

Number of completed vector store operations

db_vector_client_operation_seconds_max

Maximum observed duration for vector store operations

db_vector_client_operation_active_count

Number of vector store operations currently in flight

Operation type (add, delete, query)

Vector DB/provider (redis, chroma, pgvector, …)

Active (*_active_count) — instantaneous gauge of in-progress operations (concurrency/load).

Completed (*_seconds_sum|count|max) — statistics for operations that have finished:

_seconds_sum / _seconds_count → average latency

_seconds_max → high-water mark since last scrape (subject to registry behavior)

**Examples:**

Example 1 (xml):
```xml
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
}
```

---

## Observability :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/observability/index.html

**Contents:**
- Observability
- Chat Client
  - Prompt and Completion Data
  - Input Data (Deprecated)
  - Chat Client Advisors
- Chat Model
  - Chat Prompt and Completion Data
- Tool Calling
  - Tool Call Arguments and Result Data
- EmbeddingModel

Spring AI builds upon the observability features in the Spring ecosystem to provide insights into AI-related operations.

The spring-boot-actuator module is required for enabling observability. Add the Spring Boot Actuator dependency to your project’s Maven pom.xml build file:

or to your Gradle build.gradle build file.

Spring AI provides metrics and tracing capabilities for its core components: ChatClient (including Advisor), ChatModel, EmbeddingModel, ImageModel, and VectorStore.

1.0.0-RC1 Breaking Changes

Following configuration properties have been renamed to better reflect their purpose:

spring.ai.chat.client.observations.include-prompt → spring.ai.chat.client.observations.log-prompt

spring.ai.chat.observations.include-prompt → spring.ai.chat.observations.log-prompt

spring.ai.chat.observations.include-completion → spring.ai.chat.observations.log-completion

spring.ai.image.observations.include-prompt → spring.ai.image.observations.log-prompt

spring.ai.vectorstore.observations.include-query-response → spring.ai.vectorstore.observations.log-query-response

The spring.ai.chat.client observations are recorded when a ChatClient call() or stream() operations are invoked. They measure the time spent performing the invocation and propagate the related tracing information.

gen_ai.operation.name

spring.ai.chat.client.stream

Is the chat model response a stream - true or false

The kind of framework API in Spring AI: chat_client.

The content of the prompt sent via the chat client. Optional.

spring.ai.chat.client.advisor.params (deprecated)

Map of advisor parameters. The conversation ID is now included in spring.ai.chat.client.conversation.id.

spring.ai.chat.client.advisors

List of configured chat client advisors.

spring.ai.chat.client.conversation.id

Identifier of the conversation when using the chat memory.

spring.ai.chat.client.system.params (deprecated)

Chat client system parameters. Optional. Superseded by gen_ai.prompt.

spring.ai.chat.client.system.text (deprecated)

Chat client system text. Optional. Superseded by gen_ai.prompt.

spring.ai.chat.client.tool.function.names (deprecated)

Enabled tool function names. Superseded by spring.ai.chat.client.tool.names.

spring.ai.chat.client.tool.function.callbacks (deprecated)

List of configured chat client function callbacks. Superseded by spring.ai.chat.client.tool.names.

spring.ai.chat.client.tool.names

Names of the tools passed to the chat client.

spring.ai.chat.client.user.params (deprecated)

Chat client user parameters. Optional. Superseded by gen_ai.prompt.

spring.ai.chat.client.user.text (deprecated)

Chat client user text. Optional. Superseded by gen_ai.prompt.

The ChatClient prompt and completion data is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging the prompt and completion data to help with debugging and troubleshooting.

spring.ai.chat.client.observations.log-prompt

Whether to log the chat client prompt content.

spring.ai.chat.client.observations.log-completion

Whether to log the chat client completion content.

The ChatClient input data is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging input data to help with debugging and troubleshooting.

spring.ai.chat.client.observations.include-input

Whether to include the input content in the observations.

The spring.ai.advisor observations are recorded when an advisor is executed. They measure the time spent in the advisor (including the time spend on the inner advisors) and propagate the related tracing information.

gen_ai.operation.name

spring.ai.advisor.type (deprecated)

Where the advisor applies it’s logic in the request processing, one of BEFORE, AFTER, or AROUND. This distinction doesn’t apply anymore since all Advisors are always of the same type.

The kind of framework API in Spring AI: advisor.

spring.ai.advisor.name

spring.ai.advisor.order

Advisor order in the advisor chain.

The gen_ai.client.operation observations are recorded when calling the ChatModel call or stream methods. They measure the time spent on method completion and propagate the related tracing information.

gen_ai.operation.name

The name of the operation being performed.

The model provider as identified by the client instrumentation.

The name of the model a request is being made to.

gen_ai.response.model

The name of the model that generated the response.

gen_ai.request.frequency_penalty

The frequency penalty setting for the model request.

gen_ai.request.max_tokens

The maximum number of tokens the model generates for a request.

gen_ai.request.presence_penalty

The presence penalty setting for the model request.

gen_ai.request.stop_sequences

List of sequences that the model will use to stop generating further tokens.

gen_ai.request.temperature

The temperature setting for the model request.

The top_k sampling setting for the model request.

The top_p sampling setting for the model request.

gen_ai.response.finish_reasons

Reasons the model stopped generating tokens, corresponding to each generation received.

The unique identifier for the AI response.

gen_ai.usage.input_tokens

The number of tokens used in the model input (prompt).

gen_ai.usage.output_tokens

The number of tokens used in the model output (completion).

gen_ai.usage.total_tokens

The total number of tokens used in the model exchange.

The full prompt sent to the model. Optional.

The full response received from the model. Optional.

spring.ai.model.request.tool.names

List of tool definitions provided to the model in the request.

The chat prompt and completion data is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging chat prompt and completion data, useful for troubleshooting scenarios. When tracing is available, the logs will include trace information for better correlation.

spring.ai.chat.observations.log-prompt

Log the prompt content. true or false

spring.ai.chat.observations.log-completion

Log the completion content. true or false

spring.ai.chat.observations.include-error-logging

Include error logging in observations. true or false

The spring.ai.tool observations are recorded when performing tool calling in the context of a chat model interaction. They measure the time spent on toll call completion and propagate the related tracing information.

gen_ai.operation.name

The name of the operation being performed. It’s always framework.

The provider responsible for the operation. It’s always spring_ai.

The kind of operation performed by Spring AI. It’s always tool_call.

spring.ai.tool.definition.name

The name of the tool.

spring.ai.tool.definition.description

Description of the tool.

spring.ai.tool.definition.schema

Schema of the parameters used to call the tool.

spring.ai.tool.call.arguments

The input arguments to the tool call. (Only when enabled)

spring.ai.tool.call.result

Schema of the parameters used to call the tool. (Only when enabled)

The input arguments and result from the tool call are not exported by default, as they can be potentially sensitive.

Spring AI supports exporting tool call arguments and result data as span attributes.

spring.ai.tools.observations.include-content

Include the tool call content in observations. true or false

The gen_ai.client.operation observations are recorded on embedding model method calls. They measure the time spent on method completion and propagate the related tracing information.

gen_ai.operation.name

The name of the operation being performed.

The model provider as identified by the client instrumentation.

The name of the model a request is being made to.

gen_ai.response.model

The name of the model that generated the response.

gen_ai.request.embedding.dimensions

The number of dimensions the resulting output embeddings have.

gen_ai.usage.input_tokens

The number of tokens used in the model input.

gen_ai.usage.total_tokens

The total number of tokens used in the model exchange.

The gen_ai.client.operation observations are recorded on image model method calls. They measure the time spent on method completion and propagate the related tracing information.

gen_ai.operation.name

The name of the operation being performed.

The model provider as identified by the client instrumentation.

The name of the model a request is being made to.

gen_ai.request.image.response_format

The format in which the generated image is returned.

gen_ai.request.image.size

The size of the image to generate.

gen_ai.request.image.style

The style of the image to generate.

The unique identifier for the AI response.

gen_ai.response.model

The name of the model that generated the response.

gen_ai.usage.input_tokens

The number of tokens used in the model input (prompt).

gen_ai.usage.output_tokens

The number of tokens used in the model output (generation).

gen_ai.usage.total_tokens

The total number of tokens used in the model exchange.

The full prompt sent to the model. Optional.

The image prompt data is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging image prompt data, useful for troubleshooting scenarios. When tracing is available, the logs will include trace information for better correlation.

spring.ai.image.observations.log-prompt

Log the image prompt content. true or false

All vector store implementations in Spring AI are instrumented to provide metrics and distributed tracing data through Micrometer.

The db.vector.client.operation observations are recorded when interacting with the Vector Store. They measure the time spent on the query, add and remove operations and propagate the related tracing information.

The name of the operation or command being executed. One of add, delete, or query.

The database management system (DBMS) product as identified by the client instrumentation. One of pg_vector, azure, cassandra, chroma, elasticsearch, milvus, neo4j, opensearch, qdrant, redis, typesense, weaviate, pinecone, oracle, mongodb, gemfire, hana, simple.

The kind of framework API in Spring AI: vector_store.

The name of a collection (table, container) within the database.

The name of the database, fully qualified within the server address and port.

The record identifier if present.

db.search.similarity_metric

The metric used in similarity search.

db.vector.dimension_count

The dimension of the vector.

The name field as of the vector (e.g. a field name).

db.vector.query.content

The content of the search query being executed.

db.vector.query.filter

The metadata filters used in the search query.

db.vector.query.response.documents

Returned documents from a similarity search query. Optional.

db.vector.query.similarity_threshold

Similarity threshold that accepts all search scores. A threshold value of 0.0 means any similarity is accepted or disable the similarity threshold filtering. A threshold value of 1.0 means an exact match is required.

db.vector.query.top_k

The top-k most similar vectors returned by a query.

The vector search response data is typically big and possibly containing sensitive information. For those reasons, it is not exported by default.

Spring AI supports logging vector search response data, useful for troubleshooting scenarios. When tracing is available, the logs will include trace information for better correlation.

spring.ai.vectorstore.observations.log-query-response

Log the vector store query response content. true or false

This section documents the metrics emitted by Spring AI components as they appear in Prometheus.

Spring AI uses Micrometer. Base metric names use dots (e.g., gen_ai.client.operation), which Prometheus exports with underscores and standard suffixes:

Timers → <base>_seconds_count, <base>_seconds_sum, <base>_seconds_max, and (when supported) <base>_active_count

Counters → <base>_total (monotonic)

The following shows how base metric names expand to Prometheus time series.

gen_ai.client.operation

gen_ai_client_operation_seconds_count gen_ai_client_operation_seconds_sum gen_ai_client_operation_seconds_max gen_ai_client_operation_active_count

db.vector.client.operation

db_vector_client_operation_seconds_count db_vector_client_operation_seconds_sum db_vector_client_operation_seconds_max db_vector_client_operation_active_count

OpenTelemetry — Semantic Conventions for Generative AI (overview)

Micrometer — Naming Meters

gen_ai_chat_client_operation_seconds_sum

Total time spent in ChatClient operations (call/stream)

gen_ai_chat_client_operation_seconds_count

Number of completed ChatClient operations

gen_ai_chat_client_operation_seconds_max

Maximum observed duration of ChatClient operations

gen_ai_chat_client_operation_active_count

Number of ChatClient operations currently in flight

Active vs Completed: active_count shows in-flight calls; the _seconds series reflect only completed calls.

gen_ai_client_operation_seconds_sum

Total time executing chat model operations

gen_ai_client_operation_seconds_count

Number of completed chat model operations

gen_ai_client_operation_seconds_max

Maximum observed duration for chat model operations

gen_ai_client_operation_active_count

Number of chat model operations currently in flight

gen_ai_client_token_usage_total

Total tokens consumed, labeled by token type

gen_ai_token_type=input

Prompt tokens sent to the model

gen_ai_token_type=output

Completion tokens returned by the model

gen_ai_token_type=total

db_vector_client_operation_seconds_sum

Total time spent in vector store operations (add/delete/query)

db_vector_client_operation_seconds_count

Number of completed vector store operations

db_vector_client_operation_seconds_max

Maximum observed duration for vector store operations

db_vector_client_operation_active_count

Number of vector store operations currently in flight

Operation type (add, delete, query)

Vector DB/provider (redis, chroma, pgvector, …)

Active (*_active_count) — instantaneous gauge of in-progress operations (concurrency/load).

Completed (*_seconds_sum|count|max) — statistics for operations that have finished:

_seconds_sum / _seconds_count → average latency

_seconds_max → high-water mark since last scrape (subject to registry behavior)

**Examples:**

Example 1 (xml):
```xml
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
}
```

---
