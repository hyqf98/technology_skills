# Springai - Rag

**Pages:** 6

---

## ETL Pipeline :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/etl-pipeline.html

**Contents:**
- ETL Pipeline
- API Overview
- ETL Interfaces
  - DocumentReader
  - DocumentTransformer
  - DocumentWriter
  - ETL Class Diagram
- DocumentReaders
  - JSON
    - Example

For the latest snapshot version, please use Spring AI 1.1.2!

The Extract, Transform, and Load (ETL) framework serves as the backbone of data processing within the Retrieval Augmented Generation (RAG) use case.

The ETL pipeline orchestrates the flow from raw data sources to a structured vector store, ensuring data is in the optimal format for retrieval by the AI model.

The RAG use case is text to augment the capabilities of generative models by retrieving relevant information from a body of data to enhance the quality and relevance of the generated output.

The ETL pipelines creates, transforms and stores Document instances.

The Document class contains text, metadata and optionally additional media types like images, audio and video.

There are three main components of the ETL pipeline,

DocumentReader that implements Supplier<List<Document>>

DocumentTransformer that implements Function<List<Document>, List<Document>>

DocumentWriter that implements Consumer<List<Document>>

The Document class content is created from PDFs, text files and other document types with the help of DocumentReader.

To construct a simple ETL pipeline, you can chain together an instance of each type.

Let’s say we have the following instances of those three ETL types

PagePdfDocumentReader an implementation of DocumentReader

TokenTextSplitter an implementation of DocumentTransformer

VectorStore an implementation of DocumentWriter

To perform the basic loading of data into a Vector Database for use with the Retrieval Augmented Generation pattern, use the following code in Java function style syntax.

Alternatively, you can use method names that are more naturally expressive for the domain

The ETL pipeline is composed of the following interfaces and implementations. Detailed ETL class diagram is shown in the ETL Class Diagram section.

Provides a source of documents from diverse origins.

Transforms a batch of documents as part of the processing workflow.

Manages the final stage of the ETL process, preparing documents for storage.

The following class diagram illustrates the ETL interfaces and implementations.

The JsonReader processes JSON documents, converting them into a list of Document objects.

The JsonReader provides several constructor options:

JsonReader(Resource resource)

JsonReader(Resource resource, String…​ jsonKeysToUse)

JsonReader(Resource resource, JsonMetadataGenerator jsonMetadataGenerator, String…​ jsonKeysToUse)

resource: A Spring Resource object pointing to the JSON file.

jsonKeysToUse: An array of keys from the JSON that should be used as the text content in the resulting Document objects.

jsonMetadataGenerator: An optional JsonMetadataGenerator to create metadata for each Document.

The JsonReader processes JSON content as follows:

It can handle both JSON arrays and single JSON objects.

For each JSON object (either in an array or a single object):

It extracts the content based on the specified jsonKeysToUse.

If no keys are specified, it uses the entire JSON object as content.

It generates metadata using the provided JsonMetadataGenerator (or an empty one if not provided).

It creates a Document object with the extracted content and metadata.

The JsonReader now supports retrieving specific parts of a JSON document using JSON Pointers. This feature allows you to easily extract nested data from complex JSON structures.

This method allows you to use a JSON Pointer to retrieve a specific part of the JSON document.

pointer: A JSON Pointer string (as defined in RFC 6901) to locate the desired element within the JSON structure.

Returns a List<Document> containing the documents parsed from the JSON element located by the pointer.

The method uses the provided JSON Pointer to navigate to a specific location in the JSON structure.

If the pointer is valid and points to an existing element:

For a JSON object: it returns a list with a single Document.

For a JSON array: it returns a list of Documents, one for each element in the array.

If the pointer is invalid or points to a non-existent element, it throws an IllegalArgumentException.

In this example, if the JsonReader is configured with "description" as the jsonKeysToUse, it will create Document objects where the content is the value of the "description" field for each bike in the array.

The JsonReader uses Jackson for JSON parsing.

It can handle large JSON files efficiently by using streaming for arrays.

If multiple keys are specified in jsonKeysToUse, the content will be a concatenation of the values for those keys.

The reader is flexible and can be adapted to various JSON structures by customizing the jsonKeysToUse and JsonMetadataGenerator.

The TextReader processes plain text documents, converting them into a list of Document objects.

The TextReader provides two constructor options:

TextReader(String resourceUrl)

TextReader(Resource resource)

resourceUrl: A string representing the URL of the resource to be read.

resource: A Spring Resource object pointing to the text file.

setCharset(Charset charset): Sets the character set used for reading the text file. Default is UTF-8.

getCustomMetadata(): Returns a mutable map where you can add custom metadata for the documents.

The TextReader processes text content as follows:

It reads the entire content of the text file into a single Document object.

The content of the file becomes the content of the Document.

Metadata is automatically added to the Document:

charset: The character set used to read the file (default: "UTF-8").

source: The filename of the source text file.

Any custom metadata added via getCustomMetadata() is included in the Document.

The TextReader reads the entire file content into memory, so it may not be suitable for very large files.

If you need to split the text into smaller chunks, you can use a text splitter like TokenTextSplitter after reading the document:

The reader uses Spring’s Resource abstraction, allowing it to read from various sources (classpath, file system, URL, etc.).

Custom metadata can be added to all documents created by the reader using the getCustomMetadata() method.

The JsoupDocumentReader processes HTML documents, converting them into a list of Document objects using the JSoup library.

The JsoupDocumentReaderConfig allows you to customize the behavior of the JsoupDocumentReader:

charset: Specifies the character encoding of the HTML document (defaults to "UTF-8").

selector: A JSoup CSS selector to specify which elements to extract text from (defaults to "body").

separator: The string used to join text from multiple selected elements (defaults to "\n").

allElements: If true, extracts all text from the <body> element, ignoring the selector (defaults to false).

groupByElement: If true, creates a separate Document for each element matched by the selector (defaults to false).

includeLinkUrls: If true, extracts absolute link URLs and adds them to the metadata (defaults to false).

metadataTags: A list of <meta> tag names to extract content from (defaults to ["description", "keywords"]).

additionalMetadata: Allows you to add custom metadata to all created Document objects.

The JsoupDocumentReader processes the HTML content and creates Document objects based on the configuration:

The selector determines which elements are used for text extraction.

If allElements is true, all text within the <body> is extracted into a single Document.

If groupByElement is true, each element matching the selector creates a separate Document.

If neither allElements nor groupByElement is true, text from all elements matching the selector is joined using the separator.

The document title, content from specified <meta> tags, and (optionally) link URLs are added to the Document metadata.

The base URI, for resolving relative links, will be extracted from URL resources.

The reader preserves the text content of the selected elements, but removes any HTML tags within them.

The MarkdownDocumentReader processes Markdown documents, converting them into a list of Document objects.

The MarkdownDocumentReaderConfig allows you to customize the behavior of the MarkdownDocumentReader:

horizontalRuleCreateDocument: When set to true, horizontal rules in the Markdown will create new Document objects.

includeCodeBlock: When set to true, code blocks will be included in the same Document as the surrounding text. When false, code blocks create separate Document objects.

includeBlockquote: When set to true, blockquotes will be included in the same Document as the surrounding text. When false, blockquotes create separate Document objects.

additionalMetadata: Allows you to add custom metadata to all created Document objects.

Behavior: The MarkdownDocumentReader processes the Markdown content and creates Document objects based on the configuration:

Headers become metadata in the Document objects.

Paragraphs become the content of Document objects.

Code blocks can be separated into their own Document objects or included with surrounding text.

Blockquotes can be separated into their own Document objects or included with surrounding text.

Horizontal rules can be used to split the content into separate Document objects.

The reader preserves formatting like inline code, lists, and text styling within the content of the Document objects.

The PagePdfDocumentReader uses Apache PdfBox library to parse PDF documents

Add the dependency to your project using Maven or Gradle.

or to your Gradle build.gradle build file.

The ParagraphPdfDocumentReader uses the PDF catalog (e.g. TOC) information to split the input PDF into text paragraphs and output a single Document per paragraph. NOTE: Not all PDF documents contain the PDF catalog.

Add the dependency to your project using Maven or Gradle.

or to your Gradle build.gradle build file.

The TikaDocumentReader uses Apache Tika to extract text from a variety of document formats, such as PDF, DOC/DOCX, PPT/PPTX, and HTML. For a comprehensive list of supported formats, refer to the Tika documentation.

or to your Gradle build.gradle build file.

The TextSplitter an abstract base class that helps divides documents to fit the AI model’s context window.

The TokenTextSplitter is an implementation of TextSplitter that splits text into chunks based on token count, using the CL100K_BASE encoding.

The TokenTextSplitter provides two constructor options:

TokenTextSplitter(): Creates a splitter with default settings.

TokenTextSplitter(int defaultChunkSize, int minChunkSizeChars, int minChunkLengthToEmbed, int maxNumChunks, boolean keepSeparator)

defaultChunkSize: The target size of each text chunk in tokens (default: 800).

minChunkSizeChars: The minimum size of each text chunk in characters (default: 350).

minChunkLengthToEmbed: The minimum length of a chunk to be included (default: 5).

maxNumChunks: The maximum number of chunks to generate from a text (default: 10000).

keepSeparator: Whether to keep separators (like newlines) in the chunks (default: true).

The TokenTextSplitter processes text content as follows:

It encodes the input text into tokens using the CL100K_BASE encoding.

It splits the encoded text into chunks based on the defaultChunkSize.

It decodes the chunk back into text.

It attempts to find a suitable break point (period, question mark, exclamation mark, or newline) after the minChunkSizeChars.

If a break point is found, it truncates the chunk at that point.

It trims the chunk and optionally removes newline characters based on the keepSeparator setting.

If the resulting chunk is longer than minChunkLengthToEmbed, it’s added to the output.

This process continues until all tokens are processed or maxNumChunks is reached.

Any remaining text is added as a final chunk if it’s longer than minChunkLengthToEmbed.

The TokenTextSplitter uses the CL100K_BASE encoding from the jtokkit library, which is compatible with newer OpenAI models.

The splitter attempts to create semantically meaningful chunks by breaking at sentence boundaries where possible.

Metadata from the original documents is preserved and copied to all chunks derived from that document.

The content formatter (if set) from the original document is also copied to the derived chunks if copyContentFormatter is set to true (default behavior).

This splitter is particularly useful for preparing text for large language models that have token limits, ensuring that each chunk is within the model’s processing capacity.

Ensures uniform content formats across all documents.

The KeywordMetadataEnricher is a DocumentTransformer that uses a generative AI model to extract keywords from document content and add them as metadata.

The KeywordMetadataEnricher provides two constructor options:

KeywordMetadataEnricher(ChatModel chatModel, int keywordCount): To use the default template and extract a specified number of keywords.

KeywordMetadataEnricher(ChatModel chatModel, PromptTemplate keywordsTemplate): To use a custom template for keyword extraction.

The KeywordMetadataEnricher processes documents as follows:

For each input document, it creates a prompt using the document’s content.

It sends this prompt to the provided ChatModel to generate keywords.

The generated keywords are added to the document’s metadata under the key "excerpt_keywords".

The enriched documents are returned.

You can use the default template or customize the template through the keywordsTemplate parameter. The default template is:

Where {context_str} is replaced with the document content, and %s is replaced with the specified keyword count.

The KeywordMetadataEnricher requires a functioning ChatModel to generate keywords.

The keyword count must be 1 or greater.

The enricher adds the "excerpt_keywords" metadata field to each processed document.

The generated keywords are returned as a comma-separated string.

This enricher is particularly useful for improving document searchability and for generating tags or categories for documents.

In the Builder pattern, if the keywordsTemplate parameter is set, the keywordCount parameter will be ignored.

The SummaryMetadataEnricher is a DocumentTransformer that uses a generative AI model to create summaries for documents and add them as metadata. It can generate summaries for the current document, as well as adjacent documents (previous and next).

The SummaryMetadataEnricher provides two constructors:

SummaryMetadataEnricher(ChatModel chatModel, List<SummaryType> summaryTypes)

SummaryMetadataEnricher(ChatModel chatModel, List<SummaryType> summaryTypes, String summaryTemplate, MetadataMode metadataMode)

chatModel: The AI model used for generating summaries.

summaryTypes: A list of SummaryType enum values indicating which summaries to generate (PREVIOUS, CURRENT, NEXT).

summaryTemplate: A custom template for summary generation (optional).

metadataMode: Specifies how to handle document metadata when generating summaries (optional).

The SummaryMetadataEnricher processes documents as follows:

For each input document, it creates a prompt using the document’s content and the specified summary template.

It sends this prompt to the provided ChatModel to generate a summary.

Depending on the specified summaryTypes, it adds the following metadata to each document:

section_summary: Summary of the current document.

prev_section_summary: Summary of the previous document (if available and requested).

next_section_summary: Summary of the next document (if available and requested).

The enriched documents are returned.

The summary generation prompt can be customized by providing a custom summaryTemplate. The default template is:

The provided example demonstrates the expected behavior:

For a list of two documents, both documents receive a section_summary.

The first document receives a next_section_summary but no prev_section_summary.

The second document receives a prev_section_summary but no next_section_summary.

The section_summary of the first document matches the prev_section_summary of the second document.

The next_section_summary of the first document matches the section_summary of the second document.

The SummaryMetadataEnricher requires a functioning ChatModel to generate summaries.

The enricher can handle document lists of any size, properly handling edge cases for the first and last documents.

This enricher is particularly useful for creating context-aware summaries, allowing for better understanding of document relationships in a sequence.

The MetadataMode parameter allows control over how existing metadata is incorporated into the summary generation process.

The FileDocumentWriter is a DocumentWriter implementation that writes the content of a list of Document objects into a file.

The FileDocumentWriter provides three constructors:

FileDocumentWriter(String fileName)

FileDocumentWriter(String fileName, boolean withDocumentMarkers)

FileDocumentWriter(String fileName, boolean withDocumentMarkers, MetadataMode metadataMode, boolean append)

fileName: The name of the file to write the documents to.

withDocumentMarkers: Whether to include document markers in the output (default: false).

metadataMode: Specifies what document content to be written to the file (default: MetadataMode.NONE).

append: If true, data will be written to the end of the file rather than the beginning (default: false).

The FileDocumentWriter processes documents as follows:

It opens a FileWriter for the specified file name.

For each document in the input list:

If withDocumentMarkers is true, it writes a document marker including the document index and page numbers.

It writes the formatted content of the document based on the specified metadataMode.

The file is closed after all documents have been written.

When withDocumentMarkers is set to true, the writer includes markers for each document in the following format:

The writer uses two specific metadata keys:

page_number: Represents the starting page number of the document.

end_page_number: Represents the ending page number of the document.

These are used when writing document markers.

This will write all documents to "output.txt", including document markers, using all available metadata, and appending to the file if it already exists.

The writer uses FileWriter, so it writes text files with the default character encoding of the operating system.

If an error occurs during writing, a RuntimeException is thrown with the original exception as its cause.

The metadataMode parameter allows control over how existing metadata is incorporated into the written content.

This writer is particularly useful for debugging or creating human-readable outputs of document collections.

Provides integration with various vector stores. See Vector DB Documentation for a full listing.

**Examples:**

Example 1 (java):
```java
vectorStore.accept(tokenTextSplitter.apply(pdfReader.get()));
```

Example 2 (java):
```java
vectorStore.write(tokenTextSplitter.split(pdfReader.read()));
```

Example 3 (java):
```java
public interface DocumentReader extends Supplier<List<Document>> {

    default List<Document> read() {
		return get();
	}
}
```

Example 4 (java):
```java
public interface DocumentTransformer extends Function<List<Document>, List<Document>> {

    default List<Document> transform(List<Document> transform) {
		return apply(transform);
	}
}
```

---

## Retrieval Augmented Generation :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/retrieval-augmented-generation.html

**Contents:**
- Retrieval Augmented Generation
- Advisors
  - QuestionAnswerAdvisor
    - Dynamic Filter Expressions
    - Custom Template
  - RetrievalAugmentationAdvisor
    - Sequential RAG Flows
      - Naive RAG
      - Advanced RAG
- Modules

For the latest snapshot version, please use Spring AI 1.1.2!

Retrieval Augmented Generation (RAG) is a technique useful to overcome the limitations of large language models that struggle with long-form content, factual accuracy, and context-awareness.

Spring AI supports RAG by providing a modular architecture that allows you to build custom RAG flows yourself or use out-of-the-box RAG flows using the Advisor API.

Spring AI provides out-of-the-box support for common RAG flows using the Advisor API.

To use the QuestionAnswerAdvisor or VectorStoreChatMemoryAdvisor, you need to add the spring-ai-advisors-vector-store dependency to your project:

A vector database stores data that the AI model is unaware of. When a user question is sent to the AI model, a QuestionAnswerAdvisor queries the vector database for documents related to the user question.

The response from the vector database is appended to the user text to provide context for the AI model to generate a response.

Assuming you have already loaded data into a VectorStore, you can perform Retrieval Augmented Generation (RAG) by providing an instance of QuestionAnswerAdvisor to the ChatClient.

In this example, the QuestionAnswerAdvisor will perform a similarity search over all documents in the Vector Database. To restrict the types of documents that are searched, the SearchRequest takes an SQL like filter expression that is portable across all VectorStores.

This filter expression can be configured when creating the QuestionAnswerAdvisor and hence will always apply to all ChatClient requests, or it can be provided at runtime per request.

Here is how to create an instance of QuestionAnswerAdvisor where the threshold is 0.8 and to return the top 6 results.

Update the SearchRequest filter expression at runtime using the FILTER_EXPRESSION advisor context parameter:

The FILTER_EXPRESSION parameter allows you to dynamically filter the search results based on the provided expression.

The QuestionAnswerAdvisor uses a default template to augment the user question with the retrieved documents. You can customize this behavior by providing your own PromptTemplate object via the .promptTemplate() builder method.

The custom PromptTemplate can use any TemplateRenderer implementation (by default, it uses StPromptTemplate based on the StringTemplate engine). The important requirement is that the template must contain the following two placeholders:

a query placeholder to receive the user question.

a question_answer_context placeholder to receive the retrieved context.

Spring AI includes a library of RAG modules that you can use to build your own RAG flows. The RetrievalAugmentationAdvisor is an Advisor providing an out-of-the-box implementation for the most common RAG flows, based on a modular architecture.

To use the RetrievalAugmentationAdvisor, you need to add the spring-ai-rag dependency to your project:

By default, the RetrievalAugmentationAdvisor does not allow the retrieved context to be empty. When that happens, it instructs the model not to answer the user query. You can allow empty context as follows.

The VectorStoreDocumentRetriever accepts a FilterExpression to filter the search results based on metadata. You can provide one when instantiating the VectorStoreDocumentRetriever or at runtime per request, using the FILTER_EXPRESSION advisor context parameter.

See VectorStoreDocumentRetriever for more information.

You can also use the DocumentPostProcessor API to post-process the retrieved documents before passing them to the model. For example, you can use such an interface to perform re-ranking of the retrieved documents based on their relevance to the query, remove irrelevant or redundant documents, or compress the content of each document to reduce noise and redundancy.

Spring AI implements a Modular RAG architecture inspired by the concept of modularity detailed in the paper "Modular RAG: Transforming RAG Systems into LEGO-like Reconfigurable Frameworks".

Pre-Retrieval modules are responsible for processing the user query to achieve the best possible retrieval results.

A component for transforming the input query to make it more effective for retrieval tasks, addressing challenges such as poorly formed queries, ambiguous terms, complex vocabulary, or unsupported languages.

A CompressionQueryTransformer uses a large language model to compress a conversation history and a follow-up query into a standalone query that captures the essence of the conversation.

This transformer is useful when the conversation history is long and the follow-up query is related to the conversation context.

The prompt used by this component can be customized via the promptTemplate() method available in the builder.

A RewriteQueryTransformer uses a large language model to rewrite a user query to provide better results when querying a target system, such as a vector store or a web search engine.

This transformer is useful when the user query is verbose, ambiguous, or contains irrelevant information that may affect the quality of the search results.

The prompt used by this component can be customized via the promptTemplate() method available in the builder.

A TranslationQueryTransformer uses a large language model to translate a query to a target language that is supported by the embedding model used to generate the document embeddings. If the query is already in the target language, it is returned unchanged. If the language of the query is unknown, it is also returned unchanged.

This transformer is useful when the embedding model is trained on a specific language and the user query is in a different language.

The prompt used by this component can be customized via the promptTemplate() method available in the builder.

A component for expanding the input query into a list of queries, addressing challenges such as poorly formed queries by providing alternative query formulations, or by breaking down complex problems into simpler sub-queries.

A MultiQueryExpander uses a large language model to expand a query into multiple semantically diverse variations to capture different perspectives, useful for retrieving additional contextual information and increasing the chances of finding relevant results.

By default, the MultiQueryExpander includes the original query in the list of expanded queries. You can disable this behavior via the includeOriginal method in the builder.

The prompt used by this component can be customized via the promptTemplate() method available in the builder.

Retrieval modules are responsible for querying data systems like vector store and retrieving the most relevant documents.

Component responsible for retrieving Documents from an underlying data source, such as a search engine, a vector store, a database, or a knowledge graph.

A VectorStoreDocumentRetriever retrieves documents from a vector store that are semantically similar to the input query. It supports filtering based on metadata, similarity threshold, and top-k results.

The filter expression can be static or dynamic. For dynamic filter expressions, you can pass a Supplier.

You can also provide a request-specific filter expression via the Query API, using the FILTER_EXPRESSION parameter. If both the request-specific and the retriever-specific filter expressions are provided, the request-specific filter expression takes precedence.

A component for combining documents retrieved based on multiple queries and from multiple data sources into a single collection of documents. As part of the joining process, it can also handle duplicate documents and reciprocal ranking strategies.

A ConcatenationDocumentJoiner combines documents retrieved based on multiple queries and from multiple data sources by concatenating them into a single collection of documents. In case of duplicate documents, the first occurrence is kept. The score of each document is kept as is.

Post-Retrieval modules are responsible for processing the retrieved documents to achieve the best possible generation results.

A component for post-processing retrieved documents based on a query, addressing challenges such as lost-in-the-middle, context length restrictions from the model, and the need to reduce noise and redundancy in the retrieved information.

For example, it could rank documents based on their relevance to the query, remove irrelevant or redundant documents, or compress the content of each document to reduce noise and redundancy.

Generation modules are responsible for generating the final response based on the user query and retrieved documents.

A component for augmenting an input query with additional data, useful to provide a large language model with the necessary context to answer the user query.

The ContextualQueryAugmenter augments the user query with contextual data from the content of the provided documents.

By default, the ContextualQueryAugmenter does not allow the retrieved context to be empty. When that happens, it instructs the model not to answer the user query.

You can enable the allowEmptyContext option to allow the model to generate a response even when the retrieved context is empty.

The prompts used by this component can be customized via the promptTemplate() and emptyContextPromptTemplate() methods available in the builder.

**Examples:**

Example 1 (xml):
```xml
<dependency>
   <groupId>org.springframework.ai</groupId>
   <artifactId>spring-ai-advisors-vector-store</artifactId>
</dependency>
```

Example 2 (java):
```java
ChatResponse response = ChatClient.builder(chatModel)
        .build().prompt()
        .advisors(new QuestionAnswerAdvisor(vectorStore))
        .user(userText)
        .call()
        .chatResponse();
```

Example 3 (java):
```java
var qaAdvisor = QuestionAnswerAdvisor.builder(vectorStore)
        .searchRequest(SearchRequest.builder().similarityThreshold(0.8d).topK(6).build())
        .build();
```

Example 4 (java):
```java
ChatClient chatClient = ChatClient.builder(chatModel)
    .defaultAdvisors(QuestionAnswerAdvisor.builder(vectorStore)
        .searchRequest(SearchRequest.builder().build())
        .build())
    .build();

// Update filter expression at runtime
String content = this.chatClient.prompt()
    .user("Please answer my question XYZ")
    .advisors(a -> a.param(QuestionAnswerAdvisor.FILTER_EXPRESSION, "type == 'Spring'"))
    .call()
    .content();
```

---

## Retrieval Augmented Generation :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/retrieval-augmented-generation.html

**Contents:**
- Retrieval Augmented Generation
- Advisors
  - QuestionAnswerAdvisor
    - Dynamic Filter Expressions
    - Custom Template
  - RetrievalAugmentationAdvisor
    - Sequential RAG Flows
      - Naive RAG
      - Advanced RAG
- Modules

Retrieval Augmented Generation (RAG) is a technique useful to overcome the limitations of large language models that struggle with long-form content, factual accuracy, and context-awareness.

Spring AI supports RAG by providing a modular architecture that allows you to build custom RAG flows yourself or use out-of-the-box RAG flows using the Advisor API.

Spring AI provides out-of-the-box support for common RAG flows using the Advisor API.

To use the QuestionAnswerAdvisor or VectorStoreChatMemoryAdvisor, you need to add the spring-ai-advisors-vector-store dependency to your project:

A vector database stores data that the AI model is unaware of. When a user question is sent to the AI model, a QuestionAnswerAdvisor queries the vector database for documents related to the user question.

The response from the vector database is appended to the user text to provide context for the AI model to generate a response.

Assuming you have already loaded data into a VectorStore, you can perform Retrieval Augmented Generation (RAG) by providing an instance of QuestionAnswerAdvisor to the ChatClient.

In this example, the QuestionAnswerAdvisor will perform a similarity search over all documents in the Vector Database. To restrict the types of documents that are searched, the SearchRequest takes an SQL like filter expression that is portable across all VectorStores.

This filter expression can be configured when creating the QuestionAnswerAdvisor and hence will always apply to all ChatClient requests, or it can be provided at runtime per request.

Here is how to create an instance of QuestionAnswerAdvisor where the threshold is 0.8 and to return the top 6 results.

Update the SearchRequest filter expression at runtime using the FILTER_EXPRESSION advisor context parameter:

The FILTER_EXPRESSION parameter allows you to dynamically filter the search results based on the provided expression.

The QuestionAnswerAdvisor uses a default template to augment the user question with the retrieved documents. You can customize this behavior by providing your own PromptTemplate object via the .promptTemplate() builder method.

The custom PromptTemplate can use any TemplateRenderer implementation (by default, it uses StPromptTemplate based on the StringTemplate engine). The important requirement is that the template must contain the following two placeholders:

a query placeholder to receive the user question.

a question_answer_context placeholder to receive the retrieved context.

Spring AI includes a library of RAG modules that you can use to build your own RAG flows. The RetrievalAugmentationAdvisor is an Advisor providing an out-of-the-box implementation for the most common RAG flows, based on a modular architecture.

To use the RetrievalAugmentationAdvisor, you need to add the spring-ai-rag dependency to your project:

By default, the RetrievalAugmentationAdvisor does not allow the retrieved context to be empty. When that happens, it instructs the model not to answer the user query. You can allow empty context as follows.

The VectorStoreDocumentRetriever accepts a FilterExpression to filter the search results based on metadata. You can provide one when instantiating the VectorStoreDocumentRetriever or at runtime per request, using the FILTER_EXPRESSION advisor context parameter.

See VectorStoreDocumentRetriever for more information.

You can also use the DocumentPostProcessor API to post-process the retrieved documents before passing them to the model. For example, you can use such an interface to perform re-ranking of the retrieved documents based on their relevance to the query, remove irrelevant or redundant documents, or compress the content of each document to reduce noise and redundancy.

Spring AI implements a Modular RAG architecture inspired by the concept of modularity detailed in the paper "Modular RAG: Transforming RAG Systems into LEGO-like Reconfigurable Frameworks".

Pre-Retrieval modules are responsible for processing the user query to achieve the best possible retrieval results.

A component for transforming the input query to make it more effective for retrieval tasks, addressing challenges such as poorly formed queries, ambiguous terms, complex vocabulary, or unsupported languages.

A CompressionQueryTransformer uses a large language model to compress a conversation history and a follow-up query into a standalone query that captures the essence of the conversation.

This transformer is useful when the conversation history is long and the follow-up query is related to the conversation context.

The prompt used by this component can be customized via the promptTemplate() method available in the builder.

A RewriteQueryTransformer uses a large language model to rewrite a user query to provide better results when querying a target system, such as a vector store or a web search engine.

This transformer is useful when the user query is verbose, ambiguous, or contains irrelevant information that may affect the quality of the search results.

The prompt used by this component can be customized via the promptTemplate() method available in the builder.

A TranslationQueryTransformer uses a large language model to translate a query to a target language that is supported by the embedding model used to generate the document embeddings. If the query is already in the target language, it is returned unchanged. If the language of the query is unknown, it is also returned unchanged.

This transformer is useful when the embedding model is trained on a specific language and the user query is in a different language.

The prompt used by this component can be customized via the promptTemplate() method available in the builder.

A component for expanding the input query into a list of queries, addressing challenges such as poorly formed queries by providing alternative query formulations, or by breaking down complex problems into simpler sub-queries.

A MultiQueryExpander uses a large language model to expand a query into multiple semantically diverse variations to capture different perspectives, useful for retrieving additional contextual information and increasing the chances of finding relevant results.

By default, the MultiQueryExpander includes the original query in the list of expanded queries. You can disable this behavior via the includeOriginal method in the builder.

The prompt used by this component can be customized via the promptTemplate() method available in the builder.

Retrieval modules are responsible for querying data systems like vector store and retrieving the most relevant documents.

Component responsible for retrieving Documents from an underlying data source, such as a search engine, a vector store, a database, or a knowledge graph.

A VectorStoreDocumentRetriever retrieves documents from a vector store that are semantically similar to the input query. It supports filtering based on metadata, similarity threshold, and top-k results.

The filter expression can be static or dynamic. For dynamic filter expressions, you can pass a Supplier.

You can also provide a request-specific filter expression via the Query API, using the FILTER_EXPRESSION parameter. If both the request-specific and the retriever-specific filter expressions are provided, the request-specific filter expression takes precedence.

A component for combining documents retrieved based on multiple queries and from multiple data sources into a single collection of documents. As part of the joining process, it can also handle duplicate documents and reciprocal ranking strategies.

A ConcatenationDocumentJoiner combines documents retrieved based on multiple queries and from multiple data sources by concatenating them into a single collection of documents. In case of duplicate documents, the first occurrence is kept. The score of each document is kept as is.

Post-Retrieval modules are responsible for processing the retrieved documents to achieve the best possible generation results.

A component for post-processing retrieved documents based on a query, addressing challenges such as lost-in-the-middle, context length restrictions from the model, and the need to reduce noise and redundancy in the retrieved information.

For example, it could rank documents based on their relevance to the query, remove irrelevant or redundant documents, or compress the content of each document to reduce noise and redundancy.

Generation modules are responsible for generating the final response based on the user query and retrieved documents.

A component for augmenting an input query with additional data, useful to provide a large language model with the necessary context to answer the user query.

The ContextualQueryAugmenter augments the user query with contextual data from the content of the provided documents.

By default, the ContextualQueryAugmenter does not allow the retrieved context to be empty. When that happens, it instructs the model not to answer the user query.

You can enable the allowEmptyContext option to allow the model to generate a response even when the retrieved context is empty.

The prompts used by this component can be customized via the promptTemplate() and emptyContextPromptTemplate() methods available in the builder.

**Examples:**

Example 1 (xml):
```xml
<dependency>
   <groupId>org.springframework.ai</groupId>
   <artifactId>spring-ai-advisors-vector-store</artifactId>
</dependency>
```

Example 2 (java):
```java
ChatResponse response = ChatClient.builder(chatModel)
        .build().prompt()
        .advisors(QuestionAnswerAdvisor.builder(vectorStore).build())
        .user(userText)
        .call()
        .chatResponse();
```

Example 3 (java):
```java
var qaAdvisor = QuestionAnswerAdvisor.builder(vectorStore)
        .searchRequest(SearchRequest.builder().similarityThreshold(0.8d).topK(6).build())
        .build();
```

Example 4 (java):
```java
ChatClient chatClient = ChatClient.builder(chatModel)
    .defaultAdvisors(QuestionAnswerAdvisor.builder(vectorStore)
        .searchRequest(SearchRequest.builder().build())
        .build())
    .build();

// Update filter expression at runtime
String content = this.chatClient.prompt()
    .user("Please answer my question XYZ")
    .advisors(a -> a.param(QuestionAnswerAdvisor.FILTER_EXPRESSION, "type == 'Spring'"))
    .call()
    .content();
```

---

## Retrieval Augmented Generation :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/retrieval-augmented-generation.html

**Contents:**
- Retrieval Augmented Generation
- Advisors
  - QuestionAnswerAdvisor
    - Dynamic Filter Expressions
    - Custom Template
  - RetrievalAugmentationAdvisor
    - Sequential RAG Flows
      - Naive RAG
      - Advanced RAG
- Modules

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Retrieval Augmented Generation (RAG) is a technique useful to overcome the limitations of large language models that struggle with long-form content, factual accuracy, and context-awareness.

Spring AI supports RAG by providing a modular architecture that allows you to build custom RAG flows yourself or use out-of-the-box RAG flows using the Advisor API.

Spring AI provides out-of-the-box support for common RAG flows using the Advisor API.

To use the QuestionAnswerAdvisor or VectorStoreChatMemoryAdvisor, you need to add the spring-ai-advisors-vector-store dependency to your project:

A vector database stores data that the AI model is unaware of. When a user question is sent to the AI model, a QuestionAnswerAdvisor queries the vector database for documents related to the user question.

The response from the vector database is appended to the user text to provide context for the AI model to generate a response.

Assuming you have already loaded data into a VectorStore, you can perform Retrieval Augmented Generation (RAG) by providing an instance of QuestionAnswerAdvisor to the ChatClient.

In this example, the QuestionAnswerAdvisor will perform a similarity search over all documents in the Vector Database. To restrict the types of documents that are searched, the SearchRequest takes an SQL like filter expression that is portable across all VectorStores.

This filter expression can be configured when creating the QuestionAnswerAdvisor and hence will always apply to all ChatClient requests, or it can be provided at runtime per request.

Here is how to create an instance of QuestionAnswerAdvisor where the threshold is 0.8 and to return the top 6 results.

Update the SearchRequest filter expression at runtime using the FILTER_EXPRESSION advisor context parameter:

The FILTER_EXPRESSION parameter allows you to dynamically filter the search results based on the provided expression.

The QuestionAnswerAdvisor uses a default template to augment the user question with the retrieved documents. You can customize this behavior by providing your own PromptTemplate object via the .promptTemplate() builder method.

The custom PromptTemplate can use any TemplateRenderer implementation (by default, it uses StPromptTemplate based on the StringTemplate engine). The important requirement is that the template must contain the following two placeholders:

a query placeholder to receive the user question.

a question_answer_context placeholder to receive the retrieved context.

Spring AI includes a library of RAG modules that you can use to build your own RAG flows. The RetrievalAugmentationAdvisor is an Advisor providing an out-of-the-box implementation for the most common RAG flows, based on a modular architecture.

To use the RetrievalAugmentationAdvisor, you need to add the spring-ai-rag dependency to your project:

By default, the RetrievalAugmentationAdvisor does not allow the retrieved context to be empty. When that happens, it instructs the model not to answer the user query. You can allow empty context as follows.

The VectorStoreDocumentRetriever accepts a FilterExpression to filter the search results based on metadata. You can provide one when instantiating the VectorStoreDocumentRetriever or at runtime per request, using the FILTER_EXPRESSION advisor context parameter.

See VectorStoreDocumentRetriever for more information.

You can also use the DocumentPostProcessor API to post-process the retrieved documents before passing them to the model. For example, you can use such an interface to perform re-ranking of the retrieved documents based on their relevance to the query, remove irrelevant or redundant documents, or compress the content of each document to reduce noise and redundancy.

Spring AI implements a Modular RAG architecture inspired by the concept of modularity detailed in the paper "Modular RAG: Transforming RAG Systems into LEGO-like Reconfigurable Frameworks".

Pre-Retrieval modules are responsible for processing the user query to achieve the best possible retrieval results.

A component for transforming the input query to make it more effective for retrieval tasks, addressing challenges such as poorly formed queries, ambiguous terms, complex vocabulary, or unsupported languages.

A CompressionQueryTransformer uses a large language model to compress a conversation history and a follow-up query into a standalone query that captures the essence of the conversation.

This transformer is useful when the conversation history is long and the follow-up query is related to the conversation context.

The prompt used by this component can be customized via the promptTemplate() method available in the builder.

A RewriteQueryTransformer uses a large language model to rewrite a user query to provide better results when querying a target system, such as a vector store or a web search engine.

This transformer is useful when the user query is verbose, ambiguous, or contains irrelevant information that may affect the quality of the search results.

The prompt used by this component can be customized via the promptTemplate() method available in the builder.

A TranslationQueryTransformer uses a large language model to translate a query to a target language that is supported by the embedding model used to generate the document embeddings. If the query is already in the target language, it is returned unchanged. If the language of the query is unknown, it is also returned unchanged.

This transformer is useful when the embedding model is trained on a specific language and the user query is in a different language.

The prompt used by this component can be customized via the promptTemplate() method available in the builder.

A component for expanding the input query into a list of queries, addressing challenges such as poorly formed queries by providing alternative query formulations, or by breaking down complex problems into simpler sub-queries.

A MultiQueryExpander uses a large language model to expand a query into multiple semantically diverse variations to capture different perspectives, useful for retrieving additional contextual information and increasing the chances of finding relevant results.

By default, the MultiQueryExpander includes the original query in the list of expanded queries. You can disable this behavior via the includeOriginal method in the builder.

The prompt used by this component can be customized via the promptTemplate() method available in the builder.

Retrieval modules are responsible for querying data systems like vector store and retrieving the most relevant documents.

Component responsible for retrieving Documents from an underlying data source, such as a search engine, a vector store, a database, or a knowledge graph.

A VectorStoreDocumentRetriever retrieves documents from a vector store that are semantically similar to the input query. It supports filtering based on metadata, similarity threshold, and top-k results.

The filter expression can be static or dynamic. For dynamic filter expressions, you can pass a Supplier.

You can also provide a request-specific filter expression via the Query API, using the FILTER_EXPRESSION parameter. If both the request-specific and the retriever-specific filter expressions are provided, the request-specific filter expression takes precedence.

A component for combining documents retrieved based on multiple queries and from multiple data sources into a single collection of documents. As part of the joining process, it can also handle duplicate documents and reciprocal ranking strategies.

A ConcatenationDocumentJoiner combines documents retrieved based on multiple queries and from multiple data sources by concatenating them into a single collection of documents. In case of duplicate documents, the first occurrence is kept. The score of each document is kept as is.

Post-Retrieval modules are responsible for processing the retrieved documents to achieve the best possible generation results.

A component for post-processing retrieved documents based on a query, addressing challenges such as lost-in-the-middle, context length restrictions from the model, and the need to reduce noise and redundancy in the retrieved information.

For example, it could rank documents based on their relevance to the query, remove irrelevant or redundant documents, or compress the content of each document to reduce noise and redundancy.

Generation modules are responsible for generating the final response based on the user query and retrieved documents.

A component for augmenting an input query with additional data, useful to provide a large language model with the necessary context to answer the user query.

The ContextualQueryAugmenter augments the user query with contextual data from the content of the provided documents.

By default, the ContextualQueryAugmenter does not allow the retrieved context to be empty. When that happens, it instructs the model not to answer the user query.

You can enable the allowEmptyContext option to allow the model to generate a response even when the retrieved context is empty.

The prompts used by this component can be customized via the promptTemplate() and emptyContextPromptTemplate() methods available in the builder.

**Examples:**

Example 1 (xml):
```xml
<dependency>
   <groupId>org.springframework.ai</groupId>
   <artifactId>spring-ai-advisors-vector-store</artifactId>
</dependency>
```

Example 2 (java):
```java
ChatResponse response = ChatClient.builder(chatModel)
        .build().prompt()
        .advisors(QuestionAnswerAdvisor.builder(vectorStore).build())
        .user(userText)
        .call()
        .chatResponse();
```

Example 3 (java):
```java
var qaAdvisor = QuestionAnswerAdvisor.builder(vectorStore)
        .searchRequest(SearchRequest.builder().similarityThreshold(0.8d).topK(6).build())
        .build();
```

Example 4 (java):
```java
ChatClient chatClient = ChatClient.builder(chatModel)
    .defaultAdvisors(QuestionAnswerAdvisor.builder(vectorStore)
        .searchRequest(SearchRequest.builder().build())
        .build())
    .build();

// Update filter expression at runtime
String content = this.chatClient.prompt()
    .user("Please answer my question XYZ")
    .advisors(a -> a.param(QuestionAnswerAdvisor.FILTER_EXPRESSION, "type == 'Spring'"))
    .call()
    .content();
```

---

## ETL Pipeline :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/etl-pipeline.html

**Contents:**
- ETL Pipeline
- API Overview
- ETL Interfaces
  - DocumentReader
  - DocumentTransformer
  - DocumentWriter
  - ETL Class Diagram
- DocumentReaders
  - JSON
    - Example

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The Extract, Transform, and Load (ETL) framework serves as the backbone of data processing within the Retrieval Augmented Generation (RAG) use case.

The ETL pipeline orchestrates the flow from raw data sources to a structured vector store, ensuring data is in the optimal format for retrieval by the AI model.

The RAG use case is text to augment the capabilities of generative models by retrieving relevant information from a body of data to enhance the quality and relevance of the generated output.

The ETL pipelines creates, transforms and stores Document instances.

The Document class contains text, metadata and optionally additional media types like images, audio and video.

There are three main components of the ETL pipeline,

DocumentReader that implements Supplier<List<Document>>

DocumentTransformer that implements Function<List<Document>, List<Document>>

DocumentWriter that implements Consumer<List<Document>>

The Document class content is created from PDFs, text files and other document types with the help of DocumentReader.

To construct a simple ETL pipeline, you can chain together an instance of each type.

Let’s say we have the following instances of those three ETL types

PagePdfDocumentReader an implementation of DocumentReader

TokenTextSplitter an implementation of DocumentTransformer

VectorStore an implementation of DocumentWriter

To perform the basic loading of data into a Vector Database for use with the Retrieval Augmented Generation pattern, use the following code in Java function style syntax.

Alternatively, you can use method names that are more naturally expressive for the domain

The ETL pipeline is composed of the following interfaces and implementations. Detailed ETL class diagram is shown in the ETL Class Diagram section.

Provides a source of documents from diverse origins.

Transforms a batch of documents as part of the processing workflow.

Manages the final stage of the ETL process, preparing documents for storage.

The following class diagram illustrates the ETL interfaces and implementations.

The JsonReader processes JSON documents, converting them into a list of Document objects.

The JsonReader provides several constructor options:

JsonReader(Resource resource)

JsonReader(Resource resource, String…​ jsonKeysToUse)

JsonReader(Resource resource, JsonMetadataGenerator jsonMetadataGenerator, String…​ jsonKeysToUse)

resource: A Spring Resource object pointing to the JSON file.

jsonKeysToUse: An array of keys from the JSON that should be used as the text content in the resulting Document objects.

jsonMetadataGenerator: An optional JsonMetadataGenerator to create metadata for each Document.

The JsonReader processes JSON content as follows:

It can handle both JSON arrays and single JSON objects.

For each JSON object (either in an array or a single object):

It extracts the content based on the specified jsonKeysToUse.

If no keys are specified, it uses the entire JSON object as content.

It generates metadata using the provided JsonMetadataGenerator (or an empty one if not provided).

It creates a Document object with the extracted content and metadata.

The JsonReader now supports retrieving specific parts of a JSON document using JSON Pointers. This feature allows you to easily extract nested data from complex JSON structures.

This method allows you to use a JSON Pointer to retrieve a specific part of the JSON document.

pointer: A JSON Pointer string (as defined in RFC 6901) to locate the desired element within the JSON structure.

Returns a List<Document> containing the documents parsed from the JSON element located by the pointer.

The method uses the provided JSON Pointer to navigate to a specific location in the JSON structure.

If the pointer is valid and points to an existing element:

For a JSON object: it returns a list with a single Document.

For a JSON array: it returns a list of Documents, one for each element in the array.

If the pointer is invalid or points to a non-existent element, it throws an IllegalArgumentException.

In this example, if the JsonReader is configured with "description" as the jsonKeysToUse, it will create Document objects where the content is the value of the "description" field for each bike in the array.

The JsonReader uses Jackson for JSON parsing.

It can handle large JSON files efficiently by using streaming for arrays.

If multiple keys are specified in jsonKeysToUse, the content will be a concatenation of the values for those keys.

The reader is flexible and can be adapted to various JSON structures by customizing the jsonKeysToUse and JsonMetadataGenerator.

The TextReader processes plain text documents, converting them into a list of Document objects.

The TextReader provides two constructor options:

TextReader(String resourceUrl)

TextReader(Resource resource)

resourceUrl: A string representing the URL of the resource to be read.

resource: A Spring Resource object pointing to the text file.

setCharset(Charset charset): Sets the character set used for reading the text file. Default is UTF-8.

getCustomMetadata(): Returns a mutable map where you can add custom metadata for the documents.

The TextReader processes text content as follows:

It reads the entire content of the text file into a single Document object.

The content of the file becomes the content of the Document.

Metadata is automatically added to the Document:

charset: The character set used to read the file (default: "UTF-8").

source: The filename of the source text file.

Any custom metadata added via getCustomMetadata() is included in the Document.

The TextReader reads the entire file content into memory, so it may not be suitable for very large files.

If you need to split the text into smaller chunks, you can use a text splitter like TokenTextSplitter after reading the document:

The reader uses Spring’s Resource abstraction, allowing it to read from various sources (classpath, file system, URL, etc.).

Custom metadata can be added to all documents created by the reader using the getCustomMetadata() method.

The JsoupDocumentReader processes HTML documents, converting them into a list of Document objects using the JSoup library.

The JsoupDocumentReaderConfig allows you to customize the behavior of the JsoupDocumentReader:

charset: Specifies the character encoding of the HTML document (defaults to "UTF-8").

selector: A JSoup CSS selector to specify which elements to extract text from (defaults to "body").

separator: The string used to join text from multiple selected elements (defaults to "\n").

allElements: If true, extracts all text from the <body> element, ignoring the selector (defaults to false).

groupByElement: If true, creates a separate Document for each element matched by the selector (defaults to false).

includeLinkUrls: If true, extracts absolute link URLs and adds them to the metadata (defaults to false).

metadataTags: A list of <meta> tag names to extract content from (defaults to ["description", "keywords"]).

additionalMetadata: Allows you to add custom metadata to all created Document objects.

The JsoupDocumentReader processes the HTML content and creates Document objects based on the configuration:

The selector determines which elements are used for text extraction.

If allElements is true, all text within the <body> is extracted into a single Document.

If groupByElement is true, each element matching the selector creates a separate Document.

If neither allElements nor groupByElement is true, text from all elements matching the selector is joined using the separator.

The document title, content from specified <meta> tags, and (optionally) link URLs are added to the Document metadata.

The base URI, for resolving relative links, will be extracted from URL resources.

The reader preserves the text content of the selected elements, but removes any HTML tags within them.

The MarkdownDocumentReader processes Markdown documents, converting them into a list of Document objects.

The MarkdownDocumentReaderConfig allows you to customize the behavior of the MarkdownDocumentReader:

horizontalRuleCreateDocument: When set to true, horizontal rules in the Markdown will create new Document objects.

includeCodeBlock: When set to true, code blocks will be included in the same Document as the surrounding text. When false, code blocks create separate Document objects.

includeBlockquote: When set to true, blockquotes will be included in the same Document as the surrounding text. When false, blockquotes create separate Document objects.

additionalMetadata: Allows you to add custom metadata to all created Document objects.

Behavior: The MarkdownDocumentReader processes the Markdown content and creates Document objects based on the configuration:

Headers become metadata in the Document objects.

Paragraphs become the content of Document objects.

Code blocks can be separated into their own Document objects or included with surrounding text.

Blockquotes can be separated into their own Document objects or included with surrounding text.

Horizontal rules can be used to split the content into separate Document objects.

The reader preserves formatting like inline code, lists, and text styling within the content of the Document objects.

The PagePdfDocumentReader uses Apache PdfBox library to parse PDF documents

Add the dependency to your project using Maven or Gradle.

or to your Gradle build.gradle build file.

The ParagraphPdfDocumentReader uses the PDF catalog (e.g. TOC) information to split the input PDF into text paragraphs and output a single Document per paragraph. NOTE: Not all PDF documents contain the PDF catalog.

Add the dependency to your project using Maven or Gradle.

or to your Gradle build.gradle build file.

The TikaDocumentReader uses Apache Tika to extract text from a variety of document formats, such as PDF, DOC/DOCX, PPT/PPTX, and HTML. For a comprehensive list of supported formats, refer to the Tika documentation.

or to your Gradle build.gradle build file.

The TextSplitter an abstract base class that helps divides documents to fit the AI model’s context window.

The TokenTextSplitter is an implementation of TextSplitter that splits text into chunks based on token count, using the CL100K_BASE encoding.

The recommended way to create a TokenTextSplitter is using the builder pattern, which provides a more readable and flexible API:

You can customize the punctuation marks used for splitting text into semantically meaningful chunks. This is particularly useful for internationalization:

The TokenTextSplitter provides three constructor options:

TokenTextSplitter(): Creates a splitter with default settings.

TokenTextSplitter(boolean keepSeparator): Creates a splitter with custom separator behavior.

TokenTextSplitter(int chunkSize, int minChunkSizeChars, int minChunkLengthToEmbed, int maxNumChunks, boolean keepSeparator, List<Character> punctuationMarks): Full constructor with all customization options.

chunkSize: The target size of each text chunk in tokens (default: 800).

minChunkSizeChars: The minimum size of each text chunk in characters (default: 350).

minChunkLengthToEmbed: The minimum length of a chunk to be included (default: 5).

maxNumChunks: The maximum number of chunks to generate from a text (default: 10000).

keepSeparator: Whether to keep separators (like newlines) in the chunks (default: true).

punctuationMarks: List of characters to use as sentence boundaries for splitting (default: ., ?, !, \n).

The TokenTextSplitter processes text content as follows:

It encodes the input text into tokens using the CL100K_BASE encoding.

It splits the encoded text into chunks based on the chunkSize.

It decodes the chunk back into text.

Only if the total token count exceeds the chunk size, it attempts to find a suitable break point (using the configured punctuationMarks) after the minChunkSizeChars.

If a break point is found, it truncates the chunk at that point.

It trims the chunk and optionally removes newline characters based on the keepSeparator setting.

If the resulting chunk is longer than minChunkLengthToEmbed, it’s added to the output.

This process continues until all tokens are processed or maxNumChunks is reached.

Any remaining text is added as a final chunk if it’s longer than minChunkLengthToEmbed.

The TokenTextSplitter uses the CL100K_BASE encoding from the jtokkit library, which is compatible with newer OpenAI models.

The splitter attempts to create semantically meaningful chunks by breaking at sentence boundaries where possible.

Metadata from the original documents is preserved and copied to all chunks derived from that document.

The content formatter (if set) from the original document is also copied to the derived chunks if copyContentFormatter is set to true (default behavior).

This splitter is particularly useful for preparing text for large language models that have token limits, ensuring that each chunk is within the model’s processing capacity.

Custom Punctuation Marks: The default punctuation marks (., ?, !, \n) work well for English text. For other languages or specialized content, customize the punctuation marks using the builder’s withPunctuationMarks() method.

Performance Consideration: While the splitter can handle any number of punctuation marks, it’s recommended to keep the list reasonably small (under 20 characters) for optimal performance, as each mark is checked for every chunk.

Extensibility: The getLastPunctuationIndex(String) method is protected, allowing subclasses to override the punctuation detection logic for specialized use cases.

Small Text Handling: As of version 2.0, small texts (with token count at or below the chunk size) are no longer split at punctuation marks, preventing unnecessary fragmentation of content that already fits within the size limits.

Ensures uniform content formats across all documents.

The KeywordMetadataEnricher is a DocumentTransformer that uses a generative AI model to extract keywords from document content and add them as metadata.

The KeywordMetadataEnricher provides two constructor options:

KeywordMetadataEnricher(ChatModel chatModel, int keywordCount): To use the default template and extract a specified number of keywords.

KeywordMetadataEnricher(ChatModel chatModel, PromptTemplate keywordsTemplate): To use a custom template for keyword extraction.

The KeywordMetadataEnricher processes documents as follows:

For each input document, it creates a prompt using the document’s content.

It sends this prompt to the provided ChatModel to generate keywords.

The generated keywords are added to the document’s metadata under the key "excerpt_keywords".

The enriched documents are returned.

You can use the default template or customize the template through the keywordsTemplate parameter. The default template is:

Where {context_str} is replaced with the document content, and %s is replaced with the specified keyword count.

The KeywordMetadataEnricher requires a functioning ChatModel to generate keywords.

The keyword count must be 1 or greater.

The enricher adds the "excerpt_keywords" metadata field to each processed document.

The generated keywords are returned as a comma-separated string.

This enricher is particularly useful for improving document searchability and for generating tags or categories for documents.

In the Builder pattern, if the keywordsTemplate parameter is set, the keywordCount parameter will be ignored.

The SummaryMetadataEnricher is a DocumentTransformer that uses a generative AI model to create summaries for documents and add them as metadata. It can generate summaries for the current document, as well as adjacent documents (previous and next).

The SummaryMetadataEnricher provides two constructors:

SummaryMetadataEnricher(ChatModel chatModel, List<SummaryType> summaryTypes)

SummaryMetadataEnricher(ChatModel chatModel, List<SummaryType> summaryTypes, String summaryTemplate, MetadataMode metadataMode)

chatModel: The AI model used for generating summaries.

summaryTypes: A list of SummaryType enum values indicating which summaries to generate (PREVIOUS, CURRENT, NEXT).

summaryTemplate: A custom template for summary generation (optional).

metadataMode: Specifies how to handle document metadata when generating summaries (optional).

The SummaryMetadataEnricher processes documents as follows:

For each input document, it creates a prompt using the document’s content and the specified summary template.

It sends this prompt to the provided ChatModel to generate a summary.

Depending on the specified summaryTypes, it adds the following metadata to each document:

section_summary: Summary of the current document.

prev_section_summary: Summary of the previous document (if available and requested).

next_section_summary: Summary of the next document (if available and requested).

The enriched documents are returned.

The summary generation prompt can be customized by providing a custom summaryTemplate. The default template is:

The provided example demonstrates the expected behavior:

For a list of two documents, both documents receive a section_summary.

The first document receives a next_section_summary but no prev_section_summary.

The second document receives a prev_section_summary but no next_section_summary.

The section_summary of the first document matches the prev_section_summary of the second document.

The next_section_summary of the first document matches the section_summary of the second document.

The SummaryMetadataEnricher requires a functioning ChatModel to generate summaries.

The enricher can handle document lists of any size, properly handling edge cases for the first and last documents.

This enricher is particularly useful for creating context-aware summaries, allowing for better understanding of document relationships in a sequence.

The MetadataMode parameter allows control over how existing metadata is incorporated into the summary generation process.

The FileDocumentWriter is a DocumentWriter implementation that writes the content of a list of Document objects into a file.

The FileDocumentWriter provides three constructors:

FileDocumentWriter(String fileName)

FileDocumentWriter(String fileName, boolean withDocumentMarkers)

FileDocumentWriter(String fileName, boolean withDocumentMarkers, MetadataMode metadataMode, boolean append)

fileName: The name of the file to write the documents to.

withDocumentMarkers: Whether to include document markers in the output (default: false).

metadataMode: Specifies what document content to be written to the file (default: MetadataMode.NONE).

append: If true, data will be written to the end of the file rather than the beginning (default: false).

The FileDocumentWriter processes documents as follows:

It opens a FileWriter for the specified file name.

For each document in the input list:

If withDocumentMarkers is true, it writes a document marker including the document index and page numbers.

It writes the formatted content of the document based on the specified metadataMode.

The file is closed after all documents have been written.

When withDocumentMarkers is set to true, the writer includes markers for each document in the following format:

The writer uses two specific metadata keys:

page_number: Represents the starting page number of the document.

end_page_number: Represents the ending page number of the document.

These are used when writing document markers.

This will write all documents to "output.txt", including document markers, using all available metadata, and appending to the file if it already exists.

The writer uses FileWriter, so it writes text files with the default character encoding of the operating system.

If an error occurs during writing, a RuntimeException is thrown with the original exception as its cause.

The metadataMode parameter allows control over how existing metadata is incorporated into the written content.

This writer is particularly useful for debugging or creating human-readable outputs of document collections.

Provides integration with various vector stores. See Vector DB Documentation for a full listing.

**Examples:**

Example 1 (java):
```java
vectorStore.accept(tokenTextSplitter.apply(pdfReader.get()));
```

Example 2 (java):
```java
vectorStore.write(tokenTextSplitter.split(pdfReader.read()));
```

Example 3 (java):
```java
public interface DocumentReader extends Supplier<List<Document>> {

    default List<Document> read() {
		return get();
	}
}
```

Example 4 (java):
```java
public interface DocumentTransformer extends Function<List<Document>, List<Document>> {

    default List<Document> transform(List<Document> transform) {
		return apply(transform);
	}
}
```

---

## ETL Pipeline :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/etl-pipeline.html

**Contents:**
- ETL Pipeline
- API Overview
- ETL Interfaces
  - DocumentReader
  - DocumentTransformer
  - DocumentWriter
  - ETL Class Diagram
- DocumentReaders
  - JSON
    - Example

The Extract, Transform, and Load (ETL) framework serves as the backbone of data processing within the Retrieval Augmented Generation (RAG) use case.

The ETL pipeline orchestrates the flow from raw data sources to a structured vector store, ensuring data is in the optimal format for retrieval by the AI model.

The RAG use case is text to augment the capabilities of generative models by retrieving relevant information from a body of data to enhance the quality and relevance of the generated output.

The ETL pipelines creates, transforms and stores Document instances.

The Document class contains text, metadata and optionally additional media types like images, audio and video.

There are three main components of the ETL pipeline,

DocumentReader that implements Supplier<List<Document>>

DocumentTransformer that implements Function<List<Document>, List<Document>>

DocumentWriter that implements Consumer<List<Document>>

The Document class content is created from PDFs, text files and other document types with the help of DocumentReader.

To construct a simple ETL pipeline, you can chain together an instance of each type.

Let’s say we have the following instances of those three ETL types

PagePdfDocumentReader an implementation of DocumentReader

TokenTextSplitter an implementation of DocumentTransformer

VectorStore an implementation of DocumentWriter

To perform the basic loading of data into a Vector Database for use with the Retrieval Augmented Generation pattern, use the following code in Java function style syntax.

Alternatively, you can use method names that are more naturally expressive for the domain

The ETL pipeline is composed of the following interfaces and implementations. Detailed ETL class diagram is shown in the ETL Class Diagram section.

Provides a source of documents from diverse origins.

Transforms a batch of documents as part of the processing workflow.

Manages the final stage of the ETL process, preparing documents for storage.

The following class diagram illustrates the ETL interfaces and implementations.

The JsonReader processes JSON documents, converting them into a list of Document objects.

The JsonReader provides several constructor options:

JsonReader(Resource resource)

JsonReader(Resource resource, String…​ jsonKeysToUse)

JsonReader(Resource resource, JsonMetadataGenerator jsonMetadataGenerator, String…​ jsonKeysToUse)

resource: A Spring Resource object pointing to the JSON file.

jsonKeysToUse: An array of keys from the JSON that should be used as the text content in the resulting Document objects.

jsonMetadataGenerator: An optional JsonMetadataGenerator to create metadata for each Document.

The JsonReader processes JSON content as follows:

It can handle both JSON arrays and single JSON objects.

For each JSON object (either in an array or a single object):

It extracts the content based on the specified jsonKeysToUse.

If no keys are specified, it uses the entire JSON object as content.

It generates metadata using the provided JsonMetadataGenerator (or an empty one if not provided).

It creates a Document object with the extracted content and metadata.

The JsonReader now supports retrieving specific parts of a JSON document using JSON Pointers. This feature allows you to easily extract nested data from complex JSON structures.

This method allows you to use a JSON Pointer to retrieve a specific part of the JSON document.

pointer: A JSON Pointer string (as defined in RFC 6901) to locate the desired element within the JSON structure.

Returns a List<Document> containing the documents parsed from the JSON element located by the pointer.

The method uses the provided JSON Pointer to navigate to a specific location in the JSON structure.

If the pointer is valid and points to an existing element:

For a JSON object: it returns a list with a single Document.

For a JSON array: it returns a list of Documents, one for each element in the array.

If the pointer is invalid or points to a non-existent element, it throws an IllegalArgumentException.

In this example, if the JsonReader is configured with "description" as the jsonKeysToUse, it will create Document objects where the content is the value of the "description" field for each bike in the array.

The JsonReader uses Jackson for JSON parsing.

It can handle large JSON files efficiently by using streaming for arrays.

If multiple keys are specified in jsonKeysToUse, the content will be a concatenation of the values for those keys.

The reader is flexible and can be adapted to various JSON structures by customizing the jsonKeysToUse and JsonMetadataGenerator.

The TextReader processes plain text documents, converting them into a list of Document objects.

The TextReader provides two constructor options:

TextReader(String resourceUrl)

TextReader(Resource resource)

resourceUrl: A string representing the URL of the resource to be read.

resource: A Spring Resource object pointing to the text file.

setCharset(Charset charset): Sets the character set used for reading the text file. Default is UTF-8.

getCustomMetadata(): Returns a mutable map where you can add custom metadata for the documents.

The TextReader processes text content as follows:

It reads the entire content of the text file into a single Document object.

The content of the file becomes the content of the Document.

Metadata is automatically added to the Document:

charset: The character set used to read the file (default: "UTF-8").

source: The filename of the source text file.

Any custom metadata added via getCustomMetadata() is included in the Document.

The TextReader reads the entire file content into memory, so it may not be suitable for very large files.

If you need to split the text into smaller chunks, you can use a text splitter like TokenTextSplitter after reading the document:

The reader uses Spring’s Resource abstraction, allowing it to read from various sources (classpath, file system, URL, etc.).

Custom metadata can be added to all documents created by the reader using the getCustomMetadata() method.

The JsoupDocumentReader processes HTML documents, converting them into a list of Document objects using the JSoup library.

The JsoupDocumentReaderConfig allows you to customize the behavior of the JsoupDocumentReader:

charset: Specifies the character encoding of the HTML document (defaults to "UTF-8").

selector: A JSoup CSS selector to specify which elements to extract text from (defaults to "body").

separator: The string used to join text from multiple selected elements (defaults to "\n").

allElements: If true, extracts all text from the <body> element, ignoring the selector (defaults to false).

groupByElement: If true, creates a separate Document for each element matched by the selector (defaults to false).

includeLinkUrls: If true, extracts absolute link URLs and adds them to the metadata (defaults to false).

metadataTags: A list of <meta> tag names to extract content from (defaults to ["description", "keywords"]).

additionalMetadata: Allows you to add custom metadata to all created Document objects.

The JsoupDocumentReader processes the HTML content and creates Document objects based on the configuration:

The selector determines which elements are used for text extraction.

If allElements is true, all text within the <body> is extracted into a single Document.

If groupByElement is true, each element matching the selector creates a separate Document.

If neither allElements nor groupByElement is true, text from all elements matching the selector is joined using the separator.

The document title, content from specified <meta> tags, and (optionally) link URLs are added to the Document metadata.

The base URI, for resolving relative links, will be extracted from URL resources.

The reader preserves the text content of the selected elements, but removes any HTML tags within them.

The MarkdownDocumentReader processes Markdown documents, converting them into a list of Document objects.

The MarkdownDocumentReaderConfig allows you to customize the behavior of the MarkdownDocumentReader:

horizontalRuleCreateDocument: When set to true, horizontal rules in the Markdown will create new Document objects.

includeCodeBlock: When set to true, code blocks will be included in the same Document as the surrounding text. When false, code blocks create separate Document objects.

includeBlockquote: When set to true, blockquotes will be included in the same Document as the surrounding text. When false, blockquotes create separate Document objects.

additionalMetadata: Allows you to add custom metadata to all created Document objects.

Behavior: The MarkdownDocumentReader processes the Markdown content and creates Document objects based on the configuration:

Headers become metadata in the Document objects.

Paragraphs become the content of Document objects.

Code blocks can be separated into their own Document objects or included with surrounding text.

Blockquotes can be separated into their own Document objects or included with surrounding text.

Horizontal rules can be used to split the content into separate Document objects.

The reader preserves formatting like inline code, lists, and text styling within the content of the Document objects.

The PagePdfDocumentReader uses Apache PdfBox library to parse PDF documents

Add the dependency to your project using Maven or Gradle.

or to your Gradle build.gradle build file.

The ParagraphPdfDocumentReader uses the PDF catalog (e.g. TOC) information to split the input PDF into text paragraphs and output a single Document per paragraph. NOTE: Not all PDF documents contain the PDF catalog.

Add the dependency to your project using Maven or Gradle.

or to your Gradle build.gradle build file.

The TikaDocumentReader uses Apache Tika to extract text from a variety of document formats, such as PDF, DOC/DOCX, PPT/PPTX, and HTML. For a comprehensive list of supported formats, refer to the Tika documentation.

or to your Gradle build.gradle build file.

The TextSplitter an abstract base class that helps divides documents to fit the AI model’s context window.

The TokenTextSplitter is an implementation of TextSplitter that splits text into chunks based on token count, using the CL100K_BASE encoding.

The TokenTextSplitter provides two constructor options:

TokenTextSplitter(): Creates a splitter with default settings.

TokenTextSplitter(int defaultChunkSize, int minChunkSizeChars, int minChunkLengthToEmbed, int maxNumChunks, boolean keepSeparator)

defaultChunkSize: The target size of each text chunk in tokens (default: 800).

minChunkSizeChars: The minimum size of each text chunk in characters (default: 350).

minChunkLengthToEmbed: The minimum length of a chunk to be included (default: 5).

maxNumChunks: The maximum number of chunks to generate from a text (default: 10000).

keepSeparator: Whether to keep separators (like newlines) in the chunks (default: true).

The TokenTextSplitter processes text content as follows:

It encodes the input text into tokens using the CL100K_BASE encoding.

It splits the encoded text into chunks based on the defaultChunkSize.

It decodes the chunk back into text.

It attempts to find a suitable break point (period, question mark, exclamation mark, or newline) after the minChunkSizeChars.

If a break point is found, it truncates the chunk at that point.

It trims the chunk and optionally removes newline characters based on the keepSeparator setting.

If the resulting chunk is longer than minChunkLengthToEmbed, it’s added to the output.

This process continues until all tokens are processed or maxNumChunks is reached.

Any remaining text is added as a final chunk if it’s longer than minChunkLengthToEmbed.

The TokenTextSplitter uses the CL100K_BASE encoding from the jtokkit library, which is compatible with newer OpenAI models.

The splitter attempts to create semantically meaningful chunks by breaking at sentence boundaries where possible.

Metadata from the original documents is preserved and copied to all chunks derived from that document.

The content formatter (if set) from the original document is also copied to the derived chunks if copyContentFormatter is set to true (default behavior).

This splitter is particularly useful for preparing text for large language models that have token limits, ensuring that each chunk is within the model’s processing capacity.

Ensures uniform content formats across all documents.

The KeywordMetadataEnricher is a DocumentTransformer that uses a generative AI model to extract keywords from document content and add them as metadata.

The KeywordMetadataEnricher provides two constructor options:

KeywordMetadataEnricher(ChatModel chatModel, int keywordCount): To use the default template and extract a specified number of keywords.

KeywordMetadataEnricher(ChatModel chatModel, PromptTemplate keywordsTemplate): To use a custom template for keyword extraction.

The KeywordMetadataEnricher processes documents as follows:

For each input document, it creates a prompt using the document’s content.

It sends this prompt to the provided ChatModel to generate keywords.

The generated keywords are added to the document’s metadata under the key "excerpt_keywords".

The enriched documents are returned.

You can use the default template or customize the template through the keywordsTemplate parameter. The default template is:

Where {context_str} is replaced with the document content, and %s is replaced with the specified keyword count.

The KeywordMetadataEnricher requires a functioning ChatModel to generate keywords.

The keyword count must be 1 or greater.

The enricher adds the "excerpt_keywords" metadata field to each processed document.

The generated keywords are returned as a comma-separated string.

This enricher is particularly useful for improving document searchability and for generating tags or categories for documents.

In the Builder pattern, if the keywordsTemplate parameter is set, the keywordCount parameter will be ignored.

The SummaryMetadataEnricher is a DocumentTransformer that uses a generative AI model to create summaries for documents and add them as metadata. It can generate summaries for the current document, as well as adjacent documents (previous and next).

The SummaryMetadataEnricher provides two constructors:

SummaryMetadataEnricher(ChatModel chatModel, List<SummaryType> summaryTypes)

SummaryMetadataEnricher(ChatModel chatModel, List<SummaryType> summaryTypes, String summaryTemplate, MetadataMode metadataMode)

chatModel: The AI model used for generating summaries.

summaryTypes: A list of SummaryType enum values indicating which summaries to generate (PREVIOUS, CURRENT, NEXT).

summaryTemplate: A custom template for summary generation (optional).

metadataMode: Specifies how to handle document metadata when generating summaries (optional).

The SummaryMetadataEnricher processes documents as follows:

For each input document, it creates a prompt using the document’s content and the specified summary template.

It sends this prompt to the provided ChatModel to generate a summary.

Depending on the specified summaryTypes, it adds the following metadata to each document:

section_summary: Summary of the current document.

prev_section_summary: Summary of the previous document (if available and requested).

next_section_summary: Summary of the next document (if available and requested).

The enriched documents are returned.

The summary generation prompt can be customized by providing a custom summaryTemplate. The default template is:

The provided example demonstrates the expected behavior:

For a list of two documents, both documents receive a section_summary.

The first document receives a next_section_summary but no prev_section_summary.

The second document receives a prev_section_summary but no next_section_summary.

The section_summary of the first document matches the prev_section_summary of the second document.

The next_section_summary of the first document matches the section_summary of the second document.

The SummaryMetadataEnricher requires a functioning ChatModel to generate summaries.

The enricher can handle document lists of any size, properly handling edge cases for the first and last documents.

This enricher is particularly useful for creating context-aware summaries, allowing for better understanding of document relationships in a sequence.

The MetadataMode parameter allows control over how existing metadata is incorporated into the summary generation process.

The FileDocumentWriter is a DocumentWriter implementation that writes the content of a list of Document objects into a file.

The FileDocumentWriter provides three constructors:

FileDocumentWriter(String fileName)

FileDocumentWriter(String fileName, boolean withDocumentMarkers)

FileDocumentWriter(String fileName, boolean withDocumentMarkers, MetadataMode metadataMode, boolean append)

fileName: The name of the file to write the documents to.

withDocumentMarkers: Whether to include document markers in the output (default: false).

metadataMode: Specifies what document content to be written to the file (default: MetadataMode.NONE).

append: If true, data will be written to the end of the file rather than the beginning (default: false).

The FileDocumentWriter processes documents as follows:

It opens a FileWriter for the specified file name.

For each document in the input list:

If withDocumentMarkers is true, it writes a document marker including the document index and page numbers.

It writes the formatted content of the document based on the specified metadataMode.

The file is closed after all documents have been written.

When withDocumentMarkers is set to true, the writer includes markers for each document in the following format:

The writer uses two specific metadata keys:

page_number: Represents the starting page number of the document.

end_page_number: Represents the ending page number of the document.

These are used when writing document markers.

This will write all documents to "output.txt", including document markers, using all available metadata, and appending to the file if it already exists.

The writer uses FileWriter, so it writes text files with the default character encoding of the operating system.

If an error occurs during writing, a RuntimeException is thrown with the original exception as its cause.

The metadataMode parameter allows control over how existing metadata is incorporated into the written content.

This writer is particularly useful for debugging or creating human-readable outputs of document collections.

Provides integration with various vector stores. See Vector DB Documentation for a full listing.

**Examples:**

Example 1 (java):
```java
vectorStore.accept(tokenTextSplitter.apply(pdfReader.get()));
```

Example 2 (java):
```java
vectorStore.write(tokenTextSplitter.split(pdfReader.read()));
```

Example 3 (java):
```java
public interface DocumentReader extends Supplier<List<Document>> {

    default List<Document> read() {
		return get();
	}
}
```

Example 4 (java):
```java
public interface DocumentTransformer extends Function<List<Document>, List<Document>> {

    default List<Document> transform(List<Document> transform) {
		return apply(transform);
	}
}
```

---
