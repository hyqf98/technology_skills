---
name: springai
description: Spring AI framework for building AI-powered applications with Spring Boot. Use this skill when working with Spring AI, integrating AI models (OpenAI, Anthropic, Ollama, etc.), implementing RAG patterns, vector databases, embeddings, chat models, and AI agents in Spring applications.
---

# Springai Skill

Comprehensive assistance with springai development, generated from official documentation.

## When to Use This Skill

This skill should be triggered when:
- Working with springai
- Asking about springai features or APIs
- Implementing springai solutions
- Debugging springai code
- Learning springai best practices

## Quick Reference

### Common Patterns

**Pattern 1:** Manual Configuration Instead of using Spring Boot auto-configuration, you can manually configure the WeaviateVectorStore using the builder pattern: @Bean public WeaviateClient weaviateClient() { return new WeaviateClient(new Config("http", "localhost:8080")); } @Bean public VectorStore vectorStore(WeaviateClient weaviateClient, EmbeddingModel embeddingModel) { return WeaviateVectorStore.builder(weaviateClient, embeddingModel) .options(options) // Optional: use custom options .consistencyLevel(ConsistentLevel.QUORUM) // Optional: defaults to ConsistentLevel.ONE .filterMetadataFields(List.of( // Optional: fields that can be used in filters MetadataField.text("country"), MetadataField.number("year"))) .build(); }

```
WeaviateVectorStore
```

**Pattern 2:** Runtime Options The OpenAiAudioSpeechOptions class provides the options to use when making a text-to-speech request. On start-up, the options specified by spring.ai.openai.audio.speech are used but you can override these at runtime. The OpenAiAudioSpeechOptions class implements the TextToSpeechOptions interface, providing both portable and OpenAI-specific configuration options. For example: OpenAiAudioSpeechOptions speechOptions = OpenAiAudioSpeechOptions.builder() .model("gpt-4o-mini-tts") .voice(OpenAiAudioApi.SpeechRequest.Voice.ALLOY) .responseFormat(OpenAiAudioApi.SpeechRequest.AudioResponseFormat.MP3) .speed(1.0) .build(); TextToSpeechPrompt speechPrompt = new TextToSpeechPrompt("Hello, this is a text-to-speech example.", speechOptions); TextToSpeechResponse response = openAiAudioSpeechModel.call(speechPrompt);

```
OpenAiAudioSpeechOptions
```

**Pattern 3:** Built-in Recursive Advisors Spring AI provides two built-in recursive advisors that demonstrate this pattern: ToolCallAdvisor The ToolCallAdvisor implements the tool calling loop as part of the advisor chain, rather than relying on the model’s internal tool execution. This enables other advisors in the chain to intercept and observe the tool calling process. Key features: Disables the model’s internal tool execution by setting setInternalToolExecutionEnabled(false) Loops through the advisor chain until no more tool calls are present Supports "return direct" functionality - when a tool execution has returnDirect=true, it interrupts the tool calling loop and returns the tool execution result directly to the client application instead of sending it back to the LLM Uses callAdvisorChain.copy(this) to create a sub-chain for recursive calls Includes null safety checks to handle cases where the chat response might be null Example usage: var toolCallAdvisor = ToolCallAdvisor.builder() .toolCallingManager(toolCallingManager) .advisorOrder(BaseAdvisor.HIGHEST_PRECEDENCE + 300) .build(); var chatClient = ChatClient.builder(chatModel) .defaultAdvisors(toolCallAdvisor) .build(); Return Direct Functionality The "return direct" feature allows tools to bypass the LLM and return their results directly to the client application. This is useful when: The tool’s output is the final answer and doesn’t need LLM processing You want to reduce latency by avoiding an additional LLM call The tool result should be returned as-is without interpretation When a tool execution has returnDirect=true, the ToolCallAdvisor will: Execute the tool call as normal Detect the returnDirect flag in the ToolExecutionResult Break out of the tool calling loop Return the tool execution result directly to the client application as a ChatResponse with the tool’s output as the generation content StructuredOutputValidationAdvisor The StructuredOutputValidationAdvisor validates the structured JSON output against a generated JSON schema and retries the call if validation fails, up to a specified number of attempts. Key features: Automatically generates a JSON schema from the expected output type Validates the LLM response against the schema Retries the call if validation fails, up to a configurable number of attempts Augments the prompt with validation error messages on retry attempts to help the LLM correct its output Uses callAdvisorChain.copy(this) to create a sub-chain for recursive calls Optionally supports a custom ObjectMapper for JSON processing Example usage: var validationAdvisor = StructuredOutputValidationAdvisor.builder() .outputType(MyResponseType.class) .maxRepeatAttempts(3) .advisorOrder(BaseAdvisor.HIGHEST_PRECEDENCE + 1000) .build(); var chatClient = ChatClient.builder(chatModel) .defaultAdvisors(validationAdvisor) .build();

```
ToolCallAdvisor
```

**Pattern 4:** Manual Configuration Instead of using the Spring Boot auto-configuration, you can manually configure the Neo4j vector store. For this you need to add the spring-ai-neo4j-store to your project: <dependency> <groupId>org.springframework.ai</groupId> <artifactId>spring-ai-neo4j-store</artifactId> </dependency> or to your Gradle build.gradle build file. dependencies { implementation 'org.springframework.ai:spring-ai-neo4j-store' } Refer to the Dependency Management section to add the Spring AI BOM to your build file. Create a Neo4j Driver bean. Read the Neo4j Documentation for more in-depth information about the configuration of a custom driver. @Bean public Driver driver() { return GraphDatabase.driver("neo4j://<host>:<bolt-port>", AuthTokens.basic("<username>", "<password>")); } Then create the Neo4jVectorStore bean using the builder pattern: @Bean public VectorStore vectorStore(Driver driver, EmbeddingModel embeddingModel) { return Neo4jVectorStore.builder(driver, embeddingModel) .databaseName("neo4j") // Optional: defaults to "neo4j" .distanceType(Neo4jDistanceType.COSINE) // Optional: defaults to COSINE .embeddingDimension(1536) // Optional: defaults to 1536 .label("Document") // Optional: defaults to "Document" .embeddingProperty("embedding") // Optional: defaults to "embedding" .indexName("custom-index") // Optional: defaults to "spring-ai-document-index" .initializeSchema(true) // Optional: defaults to false .batchingStrategy(new TokenCountBatchingStrategy()) // Optional: defaults to TokenCountBatchingStrategy .build(); } // This can be any EmbeddingModel implementation @Bean public EmbeddingModel embeddingModel() { return new OpenAiEmbeddingModel(new OpenAiApi(System.getenv("OPENAI_API_KEY"))); }

```
spring-ai-neo4j-store
```

**Pattern 5:** Then create the Neo4jVectorStore bean using the builder pattern:

```
Neo4jVectorStore
```

**Pattern 6:** Runtime Options The OpenAiImageOptions.java provides model configurations, such as the model to use, the quality, the size, etc. On start-up, the default options can be configured with the AzureOpenAiImageModel(OpenAiImageApi openAiImageApi) constructor and the withDefaultOptions(OpenAiImageOptions defaultOptions) method. Alternatively, use the spring.ai.azure.openai.image.options.* properties described previously. At runtime you can override the default options by adding new, request specific, options to the ImagePrompt call. For example to override the OpenAI specific options such as quality and the number of images to create, use the following code example: ImageResponse response = azureOpenaiImageModel.call( new ImagePrompt("A light cream colored mini golden doodle", OpenAiImageOptions.builder() .quality("hd") .N(4) .height(1024) .width(1024).build()) ); In addition to the model specific AzureOpenAiImageOptions you can use a portable ImageOptions instance, created with the ImageOptionsBuilder#builder().

```
AzureOpenAiImageModel(OpenAiImageApi openAiImageApi)
```

**Pattern 7:** At runtime you can override the default options by adding new, request specific, options to the ImagePrompt call. For example to override the OpenAI specific options such as quality and the number of images to create, use the following code example:

```
ImagePrompt
```

**Pattern 8:** Advanced Example: Vector Store on top of Wikipedia Dataset The following example demonstrates how to use the store on an existing schema. Here we use the schema from the github.com/datastax-labs/colbert-wikipedia-data project which comes with the full wikipedia dataset ready vectorized for you. First, create the schema in the Cassandra database: wget https://s.apache.org/colbert-wikipedia-schema-cql -O colbert-wikipedia-schema.cql cqlsh -f colbert-wikipedia-schema.cql Then configure the store using the builder pattern: @Bean public VectorStore vectorStore(CqlSession session, EmbeddingModel embeddingModel) { List<SchemaColumn> partitionColumns = List.of( new SchemaColumn("wiki", DataTypes.TEXT), new SchemaColumn("language", DataTypes.TEXT), new SchemaColumn("title", DataTypes.TEXT) ); List<SchemaColumn> clusteringColumns = List.of( new SchemaColumn("chunk_no", DataTypes.INT), new SchemaColumn("bert_embedding_no", DataTypes.INT) ); List<SchemaColumn> extraColumns = List.of( new SchemaColumn("revision", DataTypes.INT), new SchemaColumn("id", DataTypes.INT) ); return CassandraVectorStore.builder() .session(session) .embeddingModel(embeddingModel) .keyspace("wikidata") .table("articles") .partitionKeys(partitionColumns) .clusteringKeys(clusteringColumns) .contentColumnName("body") .embeddingColumnName("all_minilm_l6_v2_embedding") .indexName("all_minilm_l6_v2_ann") .initializeSchema(false) .addMetadataColumns(extraColumns) .primaryKeyTranslator((List<Object> primaryKeys) -> { if (primaryKeys.isEmpty()) { return "test§¶0"; } return String.format("%s§¶%s", primaryKeys.get(2), primaryKeys.get(3)); }) .documentIdTranslator((id) -> { String[] parts = id.split("§¶"); String title = parts[0]; int chunk_no = parts.length > 1 ? Integer.parseInt(parts[1]) : 0; return List.of("simplewiki", "en", title, chunk_no, 0); }) .build(); } @Bean public EmbeddingModel embeddingModel() { // default is ONNX all-MiniLM-L6-v2 which is what we want return new TransformersEmbeddingModel(); } Loading the Complete Wikipedia Dataset To load the full wikipedia dataset: Download simplewiki-sstable.tar from s.apache.org/simplewiki-sstable-tar (this will take a while, the file is tens of GBs) Load the data: tar -xf simplewiki-sstable.tar -C ${CASSANDRA_DATA}/data/wikidata/articles-*/ nodetool import wikidata articles ${CASSANDRA_DATA}/data/wikidata/articles-*/ If you have existing data in this table, check the tarball’s files don’t clobber existing sstables when doing the tar. An alternative to nodetool import is to just restart Cassandra. If there are any failures in the indexes they will be rebuilt automatically.

```
wget https://s.apache.org/colbert-wikipedia-schema-cql -O colbert-wikipedia-schema.cql
cqlsh -f colbert-wikipedia-schema.cql
```

### Example Code Patterns

**Example 1** (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webmvc</artifactId>
</dependency>
```

**Example 2** (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

**Example 3** (java):
```java
@Autowired
ChatMemory chatMemory;
```

**Example 4** (java):
```java
MessageWindowChatMemory memory = MessageWindowChatMemory.builder()
    .maxMessages(10)
    .build();
```

**Example 5** (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-postgresml-embedding</artifactId>
</dependency>
```

## Reference Files

This skill includes comprehensive documentation in `references/`:

- **agents.md** - Agents documentation
- **api.md** - Api documentation
- **audio.md** - Audio documentation
- **chat_models.md** - Chat Models documentation
- **embeddings.md** - Embeddings documentation
- **getting_started.md** - Getting Started documentation
- **image_models.md** - Image Models documentation
- **mcp.md** - Mcp documentation
- **moderation.md** - Moderation documentation
- **observability.md** - Observability documentation
- **rag.md** - Rag documentation
- **vector_databases.md** - Vector Databases documentation

Use `view` to read specific reference files when detailed information is needed.

## Working with This Skill

### For Beginners
Start with the getting_started or tutorials reference files for foundational concepts.

### For Specific Features
Use the appropriate category reference file (api, guides, etc.) for detailed information.

### For Code Examples
The quick reference section above contains common patterns extracted from the official docs.

## Resources

### references/
Organized documentation extracted from official sources. These files contain:
- Detailed explanations
- Code examples with language annotations
- Links to original documentation
- Table of contents for quick navigation

### scripts/
Add helper scripts here for common automation tasks.

### assets/
Add templates, boilerplate, or example projects here.

## Notes

- This skill was automatically generated from official documentation
- Reference files preserve the structure and examples from source docs
- Code examples include language detection for better syntax highlighting
- Quick reference patterns are extracted from common usage examples in the docs

## Updating

To refresh this skill with updated documentation:
1. Re-run the scraper with the same configuration
2. The skill will be rebuilt with the latest information
