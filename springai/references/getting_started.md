# Springai - Getting Started

**Pages:** 17

---

## Getting Started :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/getting-started.html

**Contents:**
- Getting Started
- Spring Initializr
- Artifact Repositories
  - Releases - Use Maven Central
  - Snapshots - Add Snapshot Repositories
- Dependency Management
- Add dependencies for specific components
- Spring AI samples

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section offers jumping off points for how to get started using Spring AI.

You should follow the steps in each of the following sections according to your needs.

Head on over to start.spring.io and select the AI Models and Vector Stores that you want to use in your new applications.

Spring AI 1.0.0 and later versions are available in Maven Central. No additional repository configuration is required. Just make sure you have Maven Central enabled in your build file.

To use the latest development versions (e.g. 1.1.0-SNAPSHOT) or older milestone versions before 1.0.0, you need to add the following snapshot repositories in your build file.

Add the following repository definitions to your Maven or Gradle build file:

NOTE: When using Maven with Spring AI snapshots, pay attention to your Maven mirror configuration. If you have configured a mirror in your settings.xml like this:

The wildcard * will redirect all repository requests to your mirror, preventing access to Spring snapshot repositories. To fix this, modify the mirrorOf configuration to exclude Spring repositories:

This configuration allows Maven to access Spring snapshot repositories directly while still using your mirror for other dependencies.

The Spring AI Bill of Materials (BOM) declares the recommended versions of all the dependencies used by a given release of Spring AI. This is a BOM-only version and it just contains dependency management and no plugin declarations or direct references to Spring or Spring Boot. You can use the Spring Boot parent POM, or use the BOM from Spring Boot (spring-boot-dependencies) to manage Spring Boot versions.

Add the BOM to your project:

Gradle users can also use the Spring AI BOM by leveraging Gradle (5.0+) native support for declaring dependency constraints using a Maven BOM. This is implemented by adding a 'platform' dependency handler method to the dependencies section of your Gradle build script.

Each of the following sections in the documentation shows which dependencies you need to add to your project build system.

Image Generation Models

Text-To-Speech (TTS) Models

Please refer to this page for more resources and samples related to Spring AI.

**Examples:**

Example 1 (xml):
```xml
<!-- Maven Central is included by default in Maven builds.
     You usually don’t need to configure it explicitly,
     but it's shown here for clarity. -->
<repositories>
    <repository>
        <id>central</id>
        <url>https://repo.maven.apache.org/maven2</url>
    </repository>
</repositories>
```

Example 2 (unknown):
```unknown
repositories {
    mavenCentral()
}
```

Example 3 (xml):
```xml
<repositories>
  <repository>
    <id>spring-snapshots</id>
    <name>Spring Snapshots</name>
    <url>https://repo.spring.io/snapshot</url>
    <releases>
      <enabled>false</enabled>
    </releases>
  </repository>
  <repository>
    <name>Central Portal Snapshots</name>
    <id>central-portal-snapshots</id>
    <url>https://central.sonatype.com/repository/maven-snapshots/</url>
    <releases>
      <enabled>false</enabled>
    </releases>
    <snapshots>
      <enabled>true</enabled>
    </snapshots>
  </repository>
</repositories>
```

Example 4 (unknown):
```unknown
repositories {
  mavenCentral()
  maven { url 'https://repo.spring.io/milestone' }
  maven { url 'https://repo.spring.io/snapshot' }
  maven {
    name = 'Central Portal Snapshots'
    url = 'https://central.sonatype.com/repository/maven-snapshots/'
  }
}
```

---

## Model Context Protocol (MCP) :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/mcp/mcp-overview.html

**Contents:**
- Model Context Protocol (MCP)
- MCP Java SDK Architecture
  - Client/Server Layer (Top)
  - Session Layer (Middle)
  - Transport Layer (Bottom)
- Spring AI MCP Integration
  - Client Starters
  - Server Starters
    - STDIO
    - WebMVC

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The Model Context Protocol (MCP) is a standardized protocol that enables AI models to interact with external tools and resources in a structured way. Think of it as a bridge between your AI models and the real world - allowing them to access databases, APIs, file systems, and other external services through a consistent interface. It supports multiple transport mechanisms to provide flexibility across different environments.

The MCP Java SDK provides a Java implementation of the Model Context Protocol, enabling standardized interaction with AI models and tools through both synchronous and asynchronous communication patterns.

Spring AI embraces MCP with comprehensive support through dedicated Boot Starters and MCP Java Annotations, making it easier than ever to build sophisticated AI-powered applications that can seamlessly connect to external systems. This means Spring developers can participate in both sides of the MCP ecosystem - building AI applications that consume MCP servers and creating MCP servers that expose Spring-based services to the wider AI community. Bootstrap your AI applications with MCP support using Spring Initializer.

The Java MCP implementation follows a three-layer architecture that separates concerns for maintainability and flexibility:

The top layer handles the main application logic and protocol operations:

McpClient - Manages client-side operations and server connections

McpServer - Handles server-side protocol operations and client requests

Both components utilize the session layer below for communication management

The middle layer manages communication patterns and maintains connection state:

McpSession - Core session management interface

McpClientSession - Client-specific session implementation

McpServerSession - Server-specific session implementation

The bottom layer handles the actual message transport and serialization:

McpTransport - Manages JSON-RPC message serialization and deserialization

Supports multiple transport implementations (STDIO, HTTP/SSE, Streamable-HTTP, etc.)

Provides the foundation for all higher-level communication

The MCP Client is a key component in the Model Context Protocol (MCP) architecture, responsible for establishing and managing connections with MCP servers. It implements the client-side of the protocol, handling:

Protocol version negotiation to ensure compatibility with servers

Capability negotiation to determine available features

Message transport and JSON-RPC communication

Tool discovery and execution

Resource access and management

Prompt system interactions

Synchronous and asynchronous operations

Stdio-based transport for process-based communication

Java HttpClient-based SSE client transport

WebFlux SSE client transport for reactive HTTP streaming

The MCP Server is a foundational component in the Model Context Protocol (MCP) architecture that provides tools, resources, and capabilities to clients. It implements the server-side of the protocol, responsible for:

Server-side protocol operations implementation

Tool exposure and discovery

Resource management with URI-based access

Prompt template provision and handling

Capability negotiation with clients

Structured logging and notifications

Concurrent client connection management

Synchronous and Asynchronous API support

Transport implementations:

Stdio, Streamable-HTTP, Stateless Streamable-HTTP, SSE

For detailed implementation guidance, using the low-level MCP Client/Server APIs, refer to the MCP Java SDK documentation. For simplified setup using Spring Boot, use the MCP Boot Starters described below.

Spring AI provides MCP integration through the following Spring Boot starters:

spring-ai-starter-mcp-client - Core starter providing STDIO, Servlet-based Streamable-HTTP, Stateless Streamable-HTTP and SSE support

spring-ai-starter-mcp-client-webflux - WebFlux-based Streamable-HTTP, Stateless Streamable-HTTP and SSE transport implementation

Standard Input/Output (STDIO)

spring-ai-starter-mcp-server

spring.ai.mcp.server.stdio=true

spring-ai-starter-mcp-server-webmvc

spring.ai.mcp.server.protocol=SSE or empty

Streamable-HTTP WebMVC

spring-ai-starter-mcp-server-webmvc

spring.ai.mcp.server.protocol=STREAMABLE

Stateless Streamable-HTTP WebMVC

spring-ai-starter-mcp-server-webmvc

spring.ai.mcp.server.protocol=STATELESS

spring-ai-starter-mcp-server-webflux

spring.ai.mcp.server.protocol=SSE or empty

Streamable-HTTP WebFlux

spring-ai-starter-mcp-server-webflux

spring.ai.mcp.server.protocol=STREAMABLE

Stateless Streamable-HTTP WebFlux

spring-ai-starter-mcp-server-webflux

spring.ai.mcp.server.protocol=STATELESS

In addition to the programmatic MCP client & server configuration, Spring AI provides annotation-based method handling for MCP servers and clients through the MCP Annotations module. This approach simplifies the creation and registration of MCP operations using a clean, declarative programming model with Java annotations.

The MCP Annotations module enables developers to:

Create MCP tools, resources, and prompts using simple annotations

Handle client-side notifications and requests declaratively

Reduce boilerplate code and improve maintainability

Automatically generate JSON schemas for tool parameters

Access special parameters and context information

Key features include:

Server Annotations: @McpTool, @McpResource, @McpPrompt, @McpComplete

Client Annotations: @McpLogging, @McpSampling, @McpElicitation, @McpProgress

Special Parameters: McpSyncServerExchange, McpAsyncServerExchange, McpTransportContext, McpMeta

Automatic Discovery: Annotation scanning with configurable package inclusion/exclusion

Spring Boot Integration: Seamless integration with MCP Boot Starters

MCP Annotations Documentation

MCP Client Boot Starters Documentation

MCP Server Boot Starters Documentation

MCP Utilities Documentation

Model Context Protocol Specification

---

## Getting Started with Model Context Protocol (MCP) :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/guides/getting-started-mcp.html

**Contents:**
- Getting Started with Model Context Protocol (MCP)
- Introduction Video
- Complete Tutorial and Source Code
- Quick Start
  - Simple MCP Server
  - Simple MCP Client
- Learning Resources
  - Implementation Video
- Additional Examples Repository
  - Recommended Starting Points

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The Model Context Protocol (MCP) standardizes how AI applications interact with external tools and resources.

Spring joined the MCP ecosystem early as a key contributor, helping to develop and maintain the official MCP Java SDK that serves as the foundation for Java-based MCP implementations. Building on this contribution, Spring AI provides MCP support through Boot Starters and annotations, making it easy to build both MCP servers and clients.

Introduction to Model Context Protocol (MCP) - YouTube

Start here for an introductory overview of the Model Context Protocol, explaining core concepts and architecture.

📖 Blog Tutorial: Connect Your AI to Everything

💻 Complete Source Code: MCP Weather Example Repository

The tutorial covers the essentials of MCP development with Spring AI, including advanced features, and deployment patterns. All code examples below are taken from this tutorial.

The fastest way to get started is with Spring AI’s annotation-based approach. The following examples are from the blog tutorial:

Add the dependency and configure:

Add the dependency and configure:

Spring AI Model Context Protocol (MCP) Integration - YouTube

A video walkthrough of Spring AI’s MCP integration, covering both server and client implementations.

Beyond the tutorial examples, the Spring AI Examples repository contains numerous MCP implementations.

Annotation-based examples

Complete Annotations Example - All annotation features (Client & Server)

Sampling with Annotations - Advanced bidirectional AI (Client & Server)

MCP Weather Tutorial - Full tutorial source code (Client & Server)

WebFlux Weather Server

OAuth2 Secured Weather Server

Filesystem Access Server

Awesome Spring AI - Community examples and resources

Official MCP Specification

Official MCP Java SDK - Java SDK developed by the Spring team

MCP Java SDK Documentation

MCP Overview and Architecture

MCP Annotations Guide

**Examples:**

Example 1 (java):
```java
@Service
public class WeatherService {

    @McpTool(description = "Get current temperature for a location")
    public String getTemperature(
            @McpToolParam(description = "City name", required = true) String city) {
        return String.format("Current temperature in %s: 22°C", city);
    }
}
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webmvc</artifactId>
</dependency>
```

Example 3 (unknown):
```unknown
spring.ai.mcp.server.protocol=STREAMABLE
```

Example 4 (java):
```java
@Bean
public CommandLineRunner demo(ChatClient chatClient, ToolCallbackProvider mcpTools) {
    return args -> {
        String response = chatClient
            .prompt("What's the weather like in Paris?")
            .toolCallbacks(mcpTools)
            .call()
            .content();
        System.out.println(response);
    };
}
```

---

## Getting Started :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/getting-started.html

**Contents:**
- Getting Started
- Spring Initializr
- Artifact Repositories
  - Releases - Use Maven Central
  - Snapshots - Add Snapshot Repositories
- Dependency Management
- Add dependencies for specific components
- Spring AI samples

This section offers jumping off points for how to get started using Spring AI.

You should follow the steps in each of the following sections according to your needs.

Head on over to start.spring.io and select the AI Models and Vector Stores that you want to use in your new applications.

Spring AI 1.0.0 and later versions are available in Maven Central. No additional repository configuration is required. Just make sure you have Maven Central enabled in your build file.

To use the latest development versions (e.g. 1.1.0-SNAPSHOT) or older milestone versions before 1.0.0, you need to add the following snapshot repositories in your build file.

Add the following repository definitions to your Maven or Gradle build file:

NOTE: When using Maven with Spring AI snapshots, pay attention to your Maven mirror configuration. If you have configured a mirror in your settings.xml like this:

The wildcard * will redirect all repository requests to your mirror, preventing access to Spring snapshot repositories. To fix this, modify the mirrorOf configuration to exclude Spring repositories:

This configuration allows Maven to access Spring snapshot repositories directly while still using your mirror for other dependencies.

The Spring AI Bill of Materials (BOM) declares the recommended versions of all the dependencies used by a given release of Spring AI. This is a BOM-only version and it just contains dependency management and no plugin declarations or direct references to Spring or Spring Boot. You can use the Spring Boot parent POM, or use the BOM from Spring Boot (spring-boot-dependencies) to manage Spring Boot versions.

Add the BOM to your project:

Gradle users can also use the Spring AI BOM by leveraging Gradle (5.0+) native support for declaring dependency constraints using a Maven BOM. This is implemented by adding a 'platform' dependency handler method to the dependencies section of your Gradle build script.

Each of the following sections in the documentation shows which dependencies you need to add to your project build system.

Image Generation Models

Text-To-Speech (TTS) Models

Please refer to this page for more resources and samples related to Spring AI.

**Examples:**

Example 1 (xml):
```xml
<!-- Maven Central is included by default in Maven builds.
     You usually don’t need to configure it explicitly,
     but it's shown here for clarity. -->
<repositories>
    <repository>
        <id>central</id>
        <url>https://repo.maven.apache.org/maven2</url>
    </repository>
</repositories>
```

Example 2 (unknown):
```unknown
repositories {
    mavenCentral()
}
```

Example 3 (xml):
```xml
<repositories>
  <repository>
    <id>spring-snapshots</id>
    <name>Spring Snapshots</name>
    <url>https://repo.spring.io/snapshot</url>
    <releases>
      <enabled>false</enabled>
    </releases>
  </repository>
  <repository>
    <name>Central Portal Snapshots</name>
    <id>central-portal-snapshots</id>
    <url>https://central.sonatype.com/repository/maven-snapshots/</url>
    <releases>
      <enabled>false</enabled>
    </releases>
    <snapshots>
      <enabled>true</enabled>
    </snapshots>
  </repository>
</repositories>
```

Example 4 (unknown):
```unknown
repositories {
  mavenCentral()
  maven { url 'https://repo.spring.io/milestone' }
  maven { url 'https://repo.spring.io/snapshot' }
  maven {
    name = 'Central Portal Snapshots'
    url = 'https://central.sonatype.com/repository/maven-snapshots/'
  }
}
```

---

## MCP Annotations :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/mcp/mcp-annotations-overview.html

**Contents:**
- MCP Annotations
- Architecture
  - Server Annotations
  - Client Annotations
  - Special Parameters and Annotations
- Getting Started
  - Dependencies
  - Configuration
    - Client Annotation Scanner
    - Server Annotation Scanner

The Spring AI MCP Annotations module provides annotation-based method handling for Model Context Protocol (MCP) servers and clients in Java. It simplifies the creation and registration of MCP server methods and client handlers through a clean, declarative approach using Java annotations.

This library builds on top of the MCP Java SDK to provide a higher-level, annotation-based programming model for implementing MCP servers and clients.

The MCP Annotations module consists of:

For MCP Servers, the following annotations are provided:

@McpTool - Implements MCP tools with automatic JSON schema generation

@McpResource - Provides access to resources via URI templates

@McpPrompt - Generates prompt messages

@McpComplete - Provides auto-completion functionality

For MCP Clients, the following annotations are provided:

@McpLogging - Handles logging message notifications

@McpSampling - Handles sampling requests

@McpElicitation - Handles elicitation requests for gathering additional information

@McpProgress - Handles progress notifications during long-running operations

@McpToolListChanged - Handles tool list change notifications

@McpResourceListChanged - Handles resource list change notifications

@McpPromptListChanged - Handles prompt list change notifications

McpSyncRequestContext - Special parameter type for synchronous operations that provides a unified interface for accessing MCP request context, including the original request, server exchange (for stateful operations), transport context (for stateless operations), and convenient methods for logging, progress, sampling, and elicitation. This parameter is automatically injected and excluded from JSON schema generation. Supported in Complete, Prompt, Resource, and Tool methods.

McpAsyncRequestContext - Special parameter type for asynchronous operations that provides the same unified interface as McpSyncRequestContext but with reactive (Mono-based) return types. This parameter is automatically injected and excluded from JSON schema generation. Supported in Complete, Prompt, Resource, and Tool methods.

McpTransportContext - Special parameter type for stateless operations that provides lightweight access to transport-level context without full server exchange functionality. This parameter is automatically injected and excluded from JSON schema generation

@McpProgressToken - Marks a method parameter to receive the progress token from the request. This parameter is automatically injected and excluded from the generated JSON schema. Note: When using McpSyncRequestContext or McpAsyncRequestContext, the progress token can be accessed via ctx.request().progressToken() instead of using this annotation.

McpMeta - Special parameter type that provides access to metadata from MCP requests, notifications, and results. This parameter is automatically injected and excluded from parameter count limits and JSON schema generation. Note: When using McpSyncRequestContext or McpAsyncRequestContext, metadata can be obtained via ctx.requestMeta() instead.

Add the MCP annotations dependency to your project:

The MCP annotations are automatically included when you use any of the MCP Boot Starters:

spring-ai-starter-mcp-client

spring-ai-starter-mcp-client-webflux

spring-ai-starter-mcp-server

spring-ai-starter-mcp-server-webflux

spring-ai-starter-mcp-server-webmvc

The annotation scanning is enabled by default when using the MCP Boot Starters. You can configure the scanning behavior using the following properties:

Here’s a simple example of using MCP annotations to create a calculator tool:

And a simple client handler for logging:

With Spring Boot auto-configuration, these annotated beans are automatically detected and registered with the MCP server or client.

Client Annotations - Detailed guide for client-side annotations

Server Annotations - Detailed guide for server-side annotations

Special Parameters - Guide for special parameter types

Examples - Comprehensive examples and use cases

MCP Client Boot Starter

MCP Server Boot Starter

Model Context Protocol Specification

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-mcp-annotations</artifactId>
</dependency>
```

Example 2 (yaml):
```yaml
spring:
  ai:
    mcp:
      client:
        annotation-scanner:
          enabled: true  # Enable/disable annotation scanning
```

Example 3 (yaml):
```yaml
spring:
  ai:
    mcp:
      server:
        annotation-scanner:
          enabled: true  # Enable/disable annotation scanning
```

Example 4 (java):
```java
@Component
public class CalculatorTools {

    @McpTool(name = "add", description = "Add two numbers together")
    public int add(
            @McpToolParam(description = "First number", required = true) int a,
            @McpToolParam(description = "Second number", required = true) int b) {
        return a + b;
    }

    @McpTool(name = "multiply", description = "Multiply two numbers")
    public double multiply(
            @McpToolParam(description = "First number", required = true) double x,
            @McpToolParam(description = "Second number", required = true) double y) {
        return x * y;
    }
}
```

---

## MCP Annotations :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/mcp/mcp-annotations-overview.html

**Contents:**
- MCP Annotations
- Architecture
  - Server Annotations
  - Client Annotations
  - Special Parameters and Annotations
- Getting Started
  - Dependencies
  - Configuration
    - Client Annotation Scanner
    - Server Annotation Scanner

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The Spring AI MCP Annotations module provides annotation-based method handling for Model Context Protocol (MCP) servers and clients in Java. It simplifies the creation and registration of MCP server methods and client handlers through a clean, declarative approach using Java annotations.

This library builds on top of the MCP Java SDK to provide a higher-level, annotation-based programming model for implementing MCP servers and clients.

The MCP Annotations module consists of:

For MCP Servers, the following annotations are provided:

@McpTool - Implements MCP tools with automatic JSON schema generation

@McpResource - Provides access to resources via URI templates

@McpPrompt - Generates prompt messages

@McpComplete - Provides auto-completion functionality

For MCP Clients, the following annotations are provided:

@McpLogging - Handles logging message notifications

@McpSampling - Handles sampling requests

@McpElicitation - Handles elicitation requests for gathering additional information

@McpProgress - Handles progress notifications during long-running operations

@McpToolListChanged - Handles tool list change notifications

@McpResourceListChanged - Handles resource list change notifications

@McpPromptListChanged - Handles prompt list change notifications

McpSyncRequestContext - Special parameter type for synchronous operations that provides a unified interface for accessing MCP request context, including the original request, server exchange (for stateful operations), transport context (for stateless operations), and convenient methods for logging, progress, sampling, and elicitation. This parameter is automatically injected and excluded from JSON schema generation. Supported in Complete, Prompt, Resource, and Tool methods.

McpAsyncRequestContext - Special parameter type for asynchronous operations that provides the same unified interface as McpSyncRequestContext but with reactive (Mono-based) return types. This parameter is automatically injected and excluded from JSON schema generation. Supported in Complete, Prompt, Resource, and Tool methods.

McpTransportContext - Special parameter type for stateless operations that provides lightweight access to transport-level context without full server exchange functionality. This parameter is automatically injected and excluded from JSON schema generation

@McpProgressToken - Marks a method parameter to receive the progress token from the request. This parameter is automatically injected and excluded from the generated JSON schema. Note: When using McpSyncRequestContext or McpAsyncRequestContext, the progress token can be accessed via ctx.request().progressToken() instead of using this annotation.

McpMeta - Special parameter type that provides access to metadata from MCP requests, notifications, and results. This parameter is automatically injected and excluded from parameter count limits and JSON schema generation. Note: When using McpSyncRequestContext or McpAsyncRequestContext, metadata can be obtained via ctx.requestMeta() instead.

Add the MCP annotations dependency to your project:

The MCP annotations are automatically included when you use any of the MCP Boot Starters:

spring-ai-starter-mcp-client

spring-ai-starter-mcp-client-webflux

spring-ai-starter-mcp-server

spring-ai-starter-mcp-server-webflux

spring-ai-starter-mcp-server-webmvc

The annotation scanning is enabled by default when using the MCP Boot Starters. You can configure the scanning behavior using the following properties:

Here’s a simple example of using MCP annotations to create a calculator tool:

And a simple client handler for logging:

With Spring Boot auto-configuration, these annotated beans are automatically detected and registered with the MCP server or client.

Client Annotations - Detailed guide for client-side annotations

Server Annotations - Detailed guide for server-side annotations

Special Parameters - Guide for special parameter types

Examples - Comprehensive examples and use cases

MCP Client Boot Starter

MCP Server Boot Starter

Model Context Protocol Specification

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-mcp-annotations</artifactId>
</dependency>
```

Example 2 (yaml):
```yaml
spring:
  ai:
    mcp:
      client:
        annotation-scanner:
          enabled: true  # Enable/disable annotation scanning
```

Example 3 (yaml):
```yaml
spring:
  ai:
    mcp:
      server:
        annotation-scanner:
          enabled: true  # Enable/disable annotation scanning
```

Example 4 (java):
```java
@Component
public class CalculatorTools {

    @McpTool(name = "add", description = "Add two numbers together")
    public int add(
            @McpToolParam(description = "First number", required = true) int a,
            @McpToolParam(description = "Second number", required = true) int b) {
        return a + b;
    }

    @McpTool(name = "multiply", description = "Multiply two numbers")
    public double multiply(
            @McpToolParam(description = "First number", required = true) double x,
            @McpToolParam(description = "Second number", required = true) double y) {
        return x * y;
    }
}
```

---

## Introduction :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/

**Contents:**
- Introduction

The Spring AI project aims to streamline the development of applications that incorporate artificial intelligence functionality without unnecessary complexity.

The project draws inspiration from notable Python projects, such as LangChain and LlamaIndex, but Spring AI is not a direct port of those projects. The project was founded with the belief that the next wave of Generative AI applications will not be only for Python developers but will be ubiquitous across many programming languages.

Spring AI provides abstractions that serve as the foundation for developing AI applications. These abstractions have multiple implementations, enabling easy component swapping with minimal code changes.

Spring AI provides the following features:

Portable API support across AI providers for Chat, text-to-image, and Embedding models. Both synchronous and streaming API options are supported. Access to model-specific features is also available.

Support for all major AI Model providers such as Anthropic, OpenAI, Microsoft, Amazon, Google, and Ollama. Supported model types include:

Structured Outputs - Mapping of AI Model output to POJOs.

Support for all major Vector Database providers such as Apache Cassandra, Azure Cosmos DB, Azure Vector Search, Chroma, Elasticsearch, GemFire, MariaDB, Milvus, MongoDB Atlas, Neo4j, OpenSearch, Oracle, PostgreSQL/PGVector, Pinecone, Qdrant, Redis, SAP Hana, Typesense and Weaviate.

Portable API across Vector Store providers, including a novel SQL-like metadata filter API.

Tools/Function Calling - Permits the model to request the execution of client-side tools and functions, thereby accessing necessary real-time information as required and taking action.

Observability - Provides insights into AI-related operations.

Document ingestion ETL framework for Data Engineering.

AI Model Evaluation - Utilities to help evaluate generated content and protect against hallucinated response.

Spring Boot Auto Configuration and Starters for AI Models and Vector Stores.

ChatClient API - Fluent API for communicating with AI Chat Models, idiomatically similar to the WebClient and RestClient APIs.

Advisors API - Encapsulates recurring Generative AI patterns, transforms data sent to and from Language Models (LLMs), and provides portability across various models and use cases.

Support for Chat Conversation Memory and Retrieval Augmented Generation (RAG).

This feature set lets you implement common use cases, such as “Q&A over your documentation” or “Chat with your documentation.”

The concepts section provides a high-level overview of AI concepts and their representation in Spring AI.

The Getting Started section shows you how to create your first AI application. Subsequent sections delve into each component and common use cases with a code-focused approach.

---

## Introduction :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/index.html

**Contents:**
- Introduction

For the latest snapshot version, please use Spring AI 1.1.2!

The Spring AI project aims to streamline the development of applications that incorporate artificial intelligence functionality without unnecessary complexity.

The project draws inspiration from notable Python projects, such as LangChain and LlamaIndex, but Spring AI is not a direct port of those projects. The project was founded with the belief that the next wave of Generative AI applications will not be only for Python developers but will be ubiquitous across many programming languages.

Spring AI provides abstractions that serve as the foundation for developing AI applications. These abstractions have multiple implementations, enabling easy component swapping with minimal code changes.

Spring AI provides the following features:

Portable API support across AI providers for Chat, text-to-image, and Embedding models. Both synchronous and streaming API options are supported. Access to model-specific features is also available.

Support for all major AI Model providers such as Anthropic, OpenAI, Microsoft, Amazon, Google, and Ollama. Supported model types include:

Structured Outputs - Mapping of AI Model output to POJOs.

Support for all major Vector Database providers such as Apache Cassandra, Azure Cosmos DB, Azure Vector Search, Chroma, Elasticsearch, GemFire, MariaDB, Milvus, MongoDB Atlas, Neo4j, OpenSearch, Oracle, PostgreSQL/PGVector, PineCone, Qdrant, Redis, SAP Hana, Typesense and Weaviate.

Portable API across Vector Store providers, including a novel SQL-like metadata filter API.

Tools/Function Calling - Permits the model to request the execution of client-side tools and functions, thereby accessing necessary real-time information as required and taking action.

Observability - Provides insights into AI-related operations.

Document ingestion ETL framework for Data Engineering.

AI Model Evaluation - Utilities to help evaluate generated content and protect against hallucinated response.

Spring Boot Auto Configuration and Starters for AI Models and Vector Stores.

ChatClient API - Fluent API for communicating with AI Chat Models, idiomatically similar to the WebClient and RestClient APIs.

Advisors API - Encapsulates recurring Generative AI patterns, transforms data sent to and from Language Models (LLMs), and provides portability across various models and use cases.

Support for Chat Conversation Memory and Retrieval Augmented Generation (RAG).

This feature set lets you implement common use cases, such as “Q&A over your documentation” or “Chat with your documentation.”

The concepts section provides a high-level overview of AI concepts and their representation in Spring AI.

The Getting Started section shows you how to create your first AI application. Subsequent sections delve into each component and common use cases with a code-focused approach.

---

## AI Concepts :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/concepts.html

**Contents:**
- AI Concepts
- Models
- Prompts
  - Prompt Templates
- Embeddings
- Tokens
- Structured Output
- Bringing Your Data & APIs to the AI Model
  - Retrieval Augmented Generation
  - Tool Calling

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section describes core concepts that Spring AI uses. We recommend reading it closely to understand the ideas behind how Spring AI is implemented.

AI models are algorithms designed to process and generate information, often mimicking human cognitive functions. By learning patterns and insights from large datasets, these models can make predictions, text, images, or other outputs, enhancing various applications across industries.

There are many different types of AI models, each suited for a specific use case. While ChatGPT and its generative AI capabilities have captivated users through text input and output, many models and companies offer diverse inputs and outputs. Before ChatGPT, many people were fascinated by text-to-image generation models such as Midjourney and Stable Diffusion.

The following table categorizes several models based on their input and output types:

Spring AI currently supports models that process input and output as language, image, and audio. The last row in the previous table, which accepts text as input and outputs numbers, is more commonly known as embedding text and represents the internal data structures used in an AI model. Spring AI has support for embeddings to enable more advanced use cases.

What sets models like GPT apart is their pre-trained nature, as indicated by the "P" in GPT—Chat Generative Pre-trained Transformer. This pre-training feature transforms AI into a general developer tool that does not require an extensive machine learning or model training background.

Prompts serve as the foundation for the language-based inputs that guide an AI model to produce specific outputs. For those familiar with ChatGPT, a prompt might seem like merely the text entered into a dialog box that is sent to the API. However, it encompasses much more than that. In many AI Models, the text for the prompt is not just a simple string.

ChatGPT’s API has multiple text inputs within a prompt, with each text input being assigned a role. For example, there is the system role, which tells the model how to behave and sets the context for the interaction. There is also the user role, which is typically the input from the user.

Crafting effective prompts is both an art and a science. ChatGPT was designed for human conversations. This is quite a departure from using something like SQL to "ask a question". One must communicate with the AI model akin to conversing with another person.

Such is the importance of this interaction style that the term "Prompt Engineering" has emerged as its own discipline. There is a burgeoning collection of techniques that improve the effectiveness of prompts. Investing time in crafting a prompt can drastically improve the resulting output.

Sharing prompts has become a communal practice, and there is active academic research being done on this subject. As an example of how counter-intuitive it can be to create an effective prompt (for example, contrasting with SQL), a recent research paper found that one of the most effective prompts you can use starts with the phrase, “Take a deep breath and work on this step by step.” That should give you an indication of why language is so important. We do not yet fully understand how to make the most effective use of previous iterations of this technology, such as ChatGPT 3.5, let alone new versions that are being developed.

Creating effective prompts involves establishing the context of the request and substituting parts of the request with values specific to the user’s input.

This process uses traditional text-based template engines for prompt creation and management. Spring AI employs the OSS library StringTemplate for this purpose.

For instance, consider the simple prompt template:

In Spring AI, prompt templates can be likened to the "View" in Spring MVC architecture. A model object, typically a java.util.Map, is provided to populate placeholders within the template. The "rendered" string becomes the content of the prompt supplied to the AI model.

There is considerable variability in the specific data format of the prompt sent to the model. Initially starting as simple strings, prompts have evolved to include multiple messages, where each string in each message represents a distinct role for the model.

Embeddings are numerical representations of text, images, or videos that capture relationships between inputs.

Embeddings work by converting text, image, and video into arrays of floating point numbers, called vectors. These vectors are designed to capture the meaning of the text, images, and videos. The length of the embedding array is called the vector’s dimensionality.

By calculating the numerical distance between the vector representations of two pieces of text, an application can determine the similarity between the objects used to generate the embedding vectors.

As a Java developer exploring AI, it’s not necessary to comprehend the intricate mathematical theories or the specific implementations behind these vector representations. A basic understanding of their role and function within AI systems suffices, particularly when you’re integrating AI functionalities into your applications.

Embeddings are particularly relevant in practical applications like the Retrieval Augmented Generation (RAG) pattern. They enable the representation of data as points in a semantic space, which is akin to the 2-D space of Euclidean geometry, but in higher dimensions. This means just like how points on a plane in Euclidean geometry can be close or far based on their coordinates, in a semantic space, the proximity of points reflects the similarity in meaning. Sentences about similar topics are positioned closer in this multi-dimensional space, much like points lying close to each other on a graph. This proximity aids in tasks like text classification, semantic search, and even product recommendations, as it allows the AI to discern and group related concepts based on their "location" in this expanded semantic landscape.

You can think of this semantic space as a vector.

Tokens serve as the building blocks of how an AI model works. On input, models convert words to tokens. On output, they convert tokens back to words.

In English, one token roughly corresponds to 75% of a word. For reference, Shakespeare’s complete works, totaling around 900,000 words, translate to approximately 1.2 million tokens.

Perhaps more important is that Tokens = Money. In the context of hosted AI models, your charges are determined by the number of tokens used. Both input and output contribute to the overall token count.

Also, models are subject to token limits, which restrict the amount of text processed in a single API call. This threshold is often referred to as the "context window". The model does not process any text that exceeds this limit.

For instance, ChatGPT3 has a 4K token limit, while GPT4 offers varying options, such as 8K, 16K, and 32K. Anthropic’s Claude AI model features a 100K token limit, and Meta’s recent research yielded a 1M token limit model.

To summarize the collected works of Shakespeare with GPT4, you need to devise software engineering strategies to chop up the data and present the data within the model’s context window limits. The Spring AI project helps you with this task.

The output of AI models traditionally arrives as a java.lang.String, even if you ask for the reply to be in JSON. It may be a correct JSON, but it is not a JSON data structure. It is just a string. Also, asking “for JSON” as part of the prompt is not 100% accurate.

This intricacy has led to the emergence of a specialized field involving the creation of prompts to yield the intended output, followed by converting the resulting simple string into a usable data structure for application integration.

The Structured output conversion employs meticulously crafted prompts, often necessitating multiple interactions with the model to achieve the desired formatting.

How can you equip the AI model with information on which it has not been trained?

Note that the GPT 3.5/4.0 dataset extends only until September 2021. Consequently, the model says that it does not know the answer to questions that require knowledge beyond that date. An interesting bit of trivia is that this dataset is around 650GB.

Three techniques exist for customizing the AI model to incorporate your data:

Fine Tuning: This traditional machine learning technique involves tailoring the model and changing its internal weighting. However, it is a challenging process for machine learning experts and extremely resource-intensive for models like GPT due to their size. Additionally, some models might not offer this option.

Prompt Stuffing: A more practical alternative involves embedding your data within the prompt provided to the model. Given a model’s token limits, techniques are required to present relevant data within the model’s context window. This approach is colloquially referred to as “stuffing the prompt.” The Spring AI library helps you implement solutions based on the “stuffing the prompt” technique otherwise known as Retrieval Augmented Generation (RAG).

Tool Calling: This technique allows registering tools (user-defined services) that connect the large language models to the APIs of external systems. Spring AI greatly simplifies code you need to write to support tool calling.

A technique termed Retrieval Augmented Generation (RAG) has emerged to address the challenge of incorporating relevant data into prompts for accurate AI model responses.

The approach involves a batch processing style programming model, where the job reads unstructured data from your documents, transforms it, and then writes it into a vector database. At a high level, this is an ETL (Extract, Transform and Load) pipeline. The vector database is used in the retrieval part of RAG technique.

As part of loading the unstructured data into the vector database, one of the most important transformations is to split the original document into smaller pieces. The procedure of splitting the original document into smaller pieces has two important steps:

Split the document into parts while preserving the semantic boundaries of the content. For example, for a document with paragraphs and tables, one should avoid splitting the document in the middle of a paragraph or table. For code, avoid splitting the code in the middle of a method’s implementation.

Split the document’s parts further into parts whose size is a small percentage of the AI Model’s token limit.

The next phase in RAG is processing user input. When a user’s question is to be answered by an AI model, the question and all the “similar” document pieces are placed into the prompt that is sent to the AI model. This is the reason to use a vector database. It is very good at finding similar content.

The ETL Pipeline provides further information about orchestrating the flow of extracting data from data sources and storing it in a structured vector store, ensuring data is in the optimal format for retrieval when passing it to the AI model.

The ChatClient - RAG explains how to use the QuestionAnswerAdvisor to enable the RAG capability in your application.

Large Language Models (LLMs) are frozen after training, leading to stale knowledge, and they are unable to access or modify external data.

The Tool Calling mechanism addresses these shortcomings. It allows you to register your own services as tools to connect the large language models to the APIs of external systems. These systems can provide LLMs with real-time data and perform data processing actions on their behalf.

Spring AI greatly simplifies code you need to write to support tool invocation. It handles the tool invocation conversation for you. You can provide your tool as a @Tool-annotated method and provide it in your prompt options to make it available to the model. Additionally, you can define and reference multiple tools in a single prompt.

When we want to make a tool available to the model, we include its definition in the chat request. Each tool definition comprises of a name, a description, and the schema of the input parameters.

When the model decides to call a tool, it sends a response with the tool name and the input parameters modeled after the defined schema.

The application is responsible for using the tool name to identify and execute the tool with the provided input parameters.

The result of the tool call is processed by the application.

The application sends the tool call result back to the model.

The model generates the final response using the tool call result as additional context.

Follow the Tool Calling documentation for further information on how to use this feature with different AI models.

Effectively evaluating the output of an AI system in response to user requests is very important to ensuring the accuracy and usefulness of the final application. Several emerging techniques enable the use of the pre-trained model itself for this purpose.

This evaluation process involves analyzing whether the generated response aligns with the user’s intent and the context of the query. Metrics such as relevance, coherence, and factual correctness are used to gauge the quality of the AI-generated response.

One approach involves presenting both the user’s request and the AI model’s response to the model, querying whether the response aligns with the provided data.

Furthermore, leveraging the information stored in the vector database as supplementary data can enhance the evaluation process, aiding in the determination of response relevance.

The Spring AI project provides an Evaluator API which currently gives access to basic strategies to evaluate model responses. Follow the Evaluation Testing documentation for further information.

**Examples:**

Example 1 (unknown):
```unknown
Tell me a {adjective} joke about {content}.
```

---

## AI Concepts :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/concepts.html

**Contents:**
- AI Concepts
- Models
- Prompts
  - Prompt Templates
- Embeddings
- Tokens
- Structured Output
- Bringing Your Data & APIs to the AI Model
  - Retrieval Augmented Generation
  - Tool Calling

For the latest snapshot version, please use Spring AI 1.1.2!

This section describes core concepts that Spring AI uses. We recommend reading it closely to understand the ideas behind how Spring AI is implemented.

AI models are algorithms designed to process and generate information, often mimicking human cognitive functions. By learning patterns and insights from large datasets, these models can make predictions, text, images, or other outputs, enhancing various applications across industries.

There are many different types of AI models, each suited for a specific use case. While ChatGPT and its generative AI capabilities have captivated users through text input and output, many models and companies offer diverse inputs and outputs. Before ChatGPT, many people were fascinated by text-to-image generation models such as Midjourney and Stable Diffusion.

The following table categorizes several models based on their input and output types:

Spring AI currently supports models that process input and output as language, image, and audio. The last row in the previous table, which accepts text as input and outputs numbers, is more commonly known as embedding text and represents the internal data structures used in an AI model. Spring AI has support for embeddings to enable more advanced use cases.

What sets models like GPT apart is their pre-trained nature, as indicated by the "P" in GPT—Chat Generative Pre-trained Transformer. This pre-training feature transforms AI into a general developer tool that does not require an extensive machine learning or model training background.

Prompts serve as the foundation for the language-based inputs that guide an AI model to produce specific outputs. For those familiar with ChatGPT, a prompt might seem like merely the text entered into a dialog box that is sent to the API. However, it encompasses much more than that. In many AI Models, the text for the prompt is not just a simple string.

ChatGPT’s API has multiple text inputs within a prompt, with each text input being assigned a role. For example, there is the system role, which tells the model how to behave and sets the context for the interaction. There is also the user role, which is typically the input from the user.

Crafting effective prompts is both an art and a science. ChatGPT was designed for human conversations. This is quite a departure from using something like SQL to "ask a question". One must communicate with the AI model akin to conversing with another person.

Such is the importance of this interaction style that the term "Prompt Engineering" has emerged as its own discipline. There is a burgeoning collection of techniques that improve the effectiveness of prompts. Investing time in crafting a prompt can drastically improve the resulting output.

Sharing prompts has become a communal practice, and there is active academic research being done on this subject. As an example of how counter-intuitive it can be to create an effective prompt (for example, contrasting with SQL), a recent research paper found that one of the most effective prompts you can use starts with the phrase, “Take a deep breath and work on this step by step.” That should give you an indication of why language is so important. We do not yet fully understand how to make the most effective use of previous iterations of this technology, such as ChatGPT 3.5, let alone new versions that are being developed.

Creating effective prompts involves establishing the context of the request and substituting parts of the request with values specific to the user’s input.

This process uses traditional text-based template engines for prompt creation and management. Spring AI employs the OSS library StringTemplate for this purpose.

For instance, consider the simple prompt template:

In Spring AI, prompt templates can be likened to the "View" in Spring MVC architecture. A model object, typically a java.util.Map, is provided to populate placeholders within the template. The "rendered" string becomes the content of the prompt supplied to the AI model.

There is considerable variability in the specific data format of the prompt sent to the model. Initially starting as simple strings, prompts have evolved to include multiple messages, where each string in each message represents a distinct role for the model.

Embeddings are numerical representations of text, images, or videos that capture relationships between inputs.

Embeddings work by converting text, image, and video into arrays of floating point numbers, called vectors. These vectors are designed to capture the meaning of the text, images, and videos. The length of the embedding array is called the vector’s dimensionality.

By calculating the numerical distance between the vector representations of two pieces of text, an application can determine the similarity between the objects used to generate the embedding vectors.

As a Java developer exploring AI, it’s not necessary to comprehend the intricate mathematical theories or the specific implementations behind these vector representations. A basic understanding of their role and function within AI systems suffices, particularly when you’re integrating AI functionalities into your applications.

Embeddings are particularly relevant in practical applications like the Retrieval Augmented Generation (RAG) pattern. They enable the representation of data as points in a semantic space, which is akin to the 2-D space of Euclidean geometry, but in higher dimensions. This means just like how points on a plane in Euclidean geometry can be close or far based on their coordinates, in a semantic space, the proximity of points reflects the similarity in meaning. Sentences about similar topics are positioned closer in this multi-dimensional space, much like points lying close to each other on a graph. This proximity aids in tasks like text classification, semantic search, and even product recommendations, as it allows the AI to discern and group related concepts based on their "location" in this expanded semantic landscape.

You can think of this semantic space as a vector.

Tokens serve as the building blocks of how an AI model works. On input, models convert words to tokens. On output, they convert tokens back to words.

In English, one token roughly corresponds to 75% of a word. For reference, Shakespeare’s complete works, totaling around 900,000 words, translate to approximately 1.2 million tokens.

Perhaps more important is that Tokens = Money. In the context of hosted AI models, your charges are determined by the number of tokens used. Both input and output contribute to the overall token count.

Also, models are subject to token limits, which restrict the amount of text processed in a single API call. This threshold is often referred to as the "context window". The model does not process any text that exceeds this limit.

For instance, ChatGPT3 has a 4K token limit, while GPT4 offers varying options, such as 8K, 16K, and 32K. Anthropic’s Claude AI model features a 100K token limit, and Meta’s recent research yielded a 1M token limit model.

To summarize the collected works of Shakespeare with GPT4, you need to devise software engineering strategies to chop up the data and present the data within the model’s context window limits. The Spring AI project helps you with this task.

The output of AI models traditionally arrives as a java.lang.String, even if you ask for the reply to be in JSON. It may be a correct JSON, but it is not a JSON data structure. It is just a string. Also, asking “for JSON” as part of the prompt is not 100% accurate.

This intricacy has led to the emergence of a specialized field involving the creation of prompts to yield the intended output, followed by converting the resulting simple string into a usable data structure for application integration.

The Structured output conversion employs meticulously crafted prompts, often necessitating multiple interactions with the model to achieve the desired formatting.

How can you equip the AI model with information on which it has not been trained?

Note that the GPT 3.5/4.0 dataset extends only until September 2021. Consequently, the model says that it does not know the answer to questions that require knowledge beyond that date. An interesting bit of trivia is that this dataset is around 650GB.

Three techniques exist for customizing the AI model to incorporate your data:

Fine Tuning: This traditional machine learning technique involves tailoring the model and changing its internal weighting. However, it is a challenging process for machine learning experts and extremely resource-intensive for models like GPT due to their size. Additionally, some models might not offer this option.

Prompt Stuffing: A more practical alternative involves embedding your data within the prompt provided to the model. Given a model’s token limits, techniques are required to present relevant data within the model’s context window. This approach is colloquially referred to as “stuffing the prompt.” The Spring AI library helps you implement solutions based on the “stuffing the prompt” technique otherwise known as Retrieval Augmented Generation (RAG).

Tool Calling: This technique allows registering tools (user-defined services) that connect the large language models to the APIs of external systems. Spring AI greatly simplifies code you need to write to support tool calling.

A technique termed Retrieval Augmented Generation (RAG) has emerged to address the challenge of incorporating relevant data into prompts for accurate AI model responses.

The approach involves a batch processing style programming model, where the job reads unstructured data from your documents, transforms it, and then writes it into a vector database. At a high level, this is an ETL (Extract, Transform and Load) pipeline. The vector database is used in the retrieval part of RAG technique.

As part of loading the unstructured data into the vector database, one of the most important transformations is to split the original document into smaller pieces. The procedure of splitting the original document into smaller pieces has two important steps:

Split the document into parts while preserving the semantic boundaries of the content. For example, for a document with paragraphs and tables, one should avoid splitting the document in the middle of a paragraph or table. For code, avoid splitting the code in the middle of a method’s implementation.

Split the document’s parts further into parts whose size is a small percentage of the AI Model’s token limit.

The next phase in RAG is processing user input. When a user’s question is to be answered by an AI model, the question and all the “similar” document pieces are placed into the prompt that is sent to the AI model. This is the reason to use a vector database. It is very good at finding similar content.

The ETL Pipeline provides further information about orchestrating the flow of extracting data from data sources and storing it in a structured vector store, ensuring data is in the optimal format for retrieval when passing it to the AI model.

The ChatClient - RAG explains how to use the QuestionAnswerAdvisor to enable the RAG capability in your application.

Large Language Models (LLMs) are frozen after training, leading to stale knowledge, and they are unable to access or modify external data.

The Tool Calling mechanism addresses these shortcomings. It allows you to register your own services as tools to connect the large language models to the APIs of external systems. These systems can provide LLMs with real-time data and perform data processing actions on their behalf.

Spring AI greatly simplifies code you need to write to support tool invocation. It handles the tool invocation conversation for you. You can provide your tool as a @Tool-annotated method and provide it in your prompt options to make it available to the model. Additionally, you can define and reference multiple tools in a single prompt.

When we want to make a tool available to the model, we include its definition in the chat request. Each tool definition comprises of a name, a description, and the schema of the input parameters.

When the model decides to call a tool, it sends a response with the tool name and the input parameters modeled after the defined schema.

The application is responsible for using the tool name to identify and execute the tool with the provided input parameters.

The result of the tool call is processed by the application.

The application sends the tool call result back to the model.

The model generates the final response using the tool call result as additional context.

Follow the Tool Calling documentation for further information on how to use this feature with different AI models.

Effectively evaluating the output of an AI system in response to user requests is very important to ensuring the accuracy and usefulness of the final application. Several emerging techniques enable the use of the pre-trained model itself for this purpose.

This evaluation process involves analyzing whether the generated response aligns with the user’s intent and the context of the query. Metrics such as relevance, coherence, and factual correctness are used to gauge the quality of the AI-generated response.

One approach involves presenting both the user’s request and the AI model’s response to the model, querying whether the response aligns with the provided data.

Furthermore, leveraging the information stored in the vector database as supplementary data can enhance the evaluation process, aiding in the determination of response relevance.

The Spring AI project provides an Evaluator API which currently gives access to basic strategies to evaluate model responses. Follow the Evaluation Testing documentation for further information.

**Examples:**

Example 1 (unknown):
```unknown
Tell me a {adjective} joke about {content}.
```

---

## Introduction :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/index.html

**Contents:**
- Introduction

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The Spring AI project aims to streamline the development of applications that incorporate artificial intelligence functionality without unnecessary complexity.

The project draws inspiration from notable Python projects, such as LangChain and LlamaIndex, but Spring AI is not a direct port of those projects. The project was founded with the belief that the next wave of Generative AI applications will not be only for Python developers but will be ubiquitous across many programming languages.

Spring AI provides abstractions that serve as the foundation for developing AI applications. These abstractions have multiple implementations, enabling easy component swapping with minimal code changes.

Spring AI provides the following features:

Portable API support across AI providers for Chat, text-to-image, and Embedding models. Both synchronous and streaming API options are supported. Access to model-specific features is also available.

Support for all major AI Model providers such as Anthropic, OpenAI, Microsoft, Amazon, Google, and Ollama. Supported model types include:

Structured Outputs - Mapping of AI Model output to POJOs.

Support for all major Vector Database providers such as Apache Cassandra, Azure Cosmos DB, Azure Vector Search, Chroma, Elasticsearch, GemFire, MariaDB, Milvus, MongoDB Atlas, Neo4j, OpenSearch, Oracle, PostgreSQL/PGVector, Pinecone, Qdrant, Redis, SAP Hana, Typesense and Weaviate.

Portable API across Vector Store providers, including a novel SQL-like metadata filter API.

Tools/Function Calling - Permits the model to request the execution of client-side tools and functions, thereby accessing necessary real-time information as required and taking action.

Observability - Provides insights into AI-related operations.

Document ingestion ETL framework for Data Engineering.

AI Model Evaluation - Utilities to help evaluate generated content and protect against hallucinated response.

Spring Boot Auto Configuration and Starters for AI Models and Vector Stores.

ChatClient API - Fluent API for communicating with AI Chat Models, idiomatically similar to the WebClient and RestClient APIs.

Advisors API - Encapsulates recurring Generative AI patterns, transforms data sent to and from Language Models (LLMs), and provides portability across various models and use cases.

Support for Chat Conversation Memory and Retrieval Augmented Generation (RAG).

This feature set lets you implement common use cases, such as “Q&A over your documentation” or “Chat with your documentation.”

The concepts section provides a high-level overview of AI concepts and their representation in Spring AI.

The Getting Started section shows you how to create your first AI application. Subsequent sections delve into each component and common use cases with a code-focused approach.

---

## Introduction :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/index.html

**Contents:**
- Introduction

The Spring AI project aims to streamline the development of applications that incorporate artificial intelligence functionality without unnecessary complexity.

The project draws inspiration from notable Python projects, such as LangChain and LlamaIndex, but Spring AI is not a direct port of those projects. The project was founded with the belief that the next wave of Generative AI applications will not be only for Python developers but will be ubiquitous across many programming languages.

Spring AI provides abstractions that serve as the foundation for developing AI applications. These abstractions have multiple implementations, enabling easy component swapping with minimal code changes.

Spring AI provides the following features:

Portable API support across AI providers for Chat, text-to-image, and Embedding models. Both synchronous and streaming API options are supported. Access to model-specific features is also available.

Support for all major AI Model providers such as Anthropic, OpenAI, Microsoft, Amazon, Google, and Ollama. Supported model types include:

Structured Outputs - Mapping of AI Model output to POJOs.

Support for all major Vector Database providers such as Apache Cassandra, Azure Cosmos DB, Azure Vector Search, Chroma, Elasticsearch, GemFire, MariaDB, Milvus, MongoDB Atlas, Neo4j, OpenSearch, Oracle, PostgreSQL/PGVector, Pinecone, Qdrant, Redis, SAP Hana, Typesense and Weaviate.

Portable API across Vector Store providers, including a novel SQL-like metadata filter API.

Tools/Function Calling - Permits the model to request the execution of client-side tools and functions, thereby accessing necessary real-time information as required and taking action.

Observability - Provides insights into AI-related operations.

Document ingestion ETL framework for Data Engineering.

AI Model Evaluation - Utilities to help evaluate generated content and protect against hallucinated response.

Spring Boot Auto Configuration and Starters for AI Models and Vector Stores.

ChatClient API - Fluent API for communicating with AI Chat Models, idiomatically similar to the WebClient and RestClient APIs.

Advisors API - Encapsulates recurring Generative AI patterns, transforms data sent to and from Language Models (LLMs), and provides portability across various models and use cases.

Support for Chat Conversation Memory and Retrieval Augmented Generation (RAG).

This feature set lets you implement common use cases, such as “Q&A over your documentation” or “Chat with your documentation.”

The concepts section provides a high-level overview of AI concepts and their representation in Spring AI.

The Getting Started section shows you how to create your first AI application. Subsequent sections delve into each component and common use cases with a code-focused approach.

---

## Model Context Protocol (MCP) :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/mcp/mcp-overview.html

**Contents:**
- Model Context Protocol (MCP)
- MCP Java SDK Architecture
  - Client/Server Layer (Top)
  - Session Layer (Middle)
  - Transport Layer (Bottom)
- Spring AI MCP Integration
  - Client Starters
  - Server Starters
    - STDIO
    - WebMVC

The Model Context Protocol (MCP) is a standardized protocol that enables AI models to interact with external tools and resources in a structured way. Think of it as a bridge between your AI models and the real world - allowing them to access databases, APIs, file systems, and other external services through a consistent interface. It supports multiple transport mechanisms to provide flexibility across different environments.

The MCP Java SDK provides a Java implementation of the Model Context Protocol, enabling standardized interaction with AI models and tools through both synchronous and asynchronous communication patterns.

Spring AI embraces MCP with comprehensive support through dedicated Boot Starters and MCP Java Annotations, making it easier than ever to build sophisticated AI-powered applications that can seamlessly connect to external systems. This means Spring developers can participate in both sides of the MCP ecosystem - building AI applications that consume MCP servers and creating MCP servers that expose Spring-based services to the wider AI community. Bootstrap your AI applications with MCP support using Spring Initializer.

The Java MCP implementation follows a three-layer architecture that separates concerns for maintainability and flexibility:

The top layer handles the main application logic and protocol operations:

McpClient - Manages client-side operations and server connections

McpServer - Handles server-side protocol operations and client requests

Both components utilize the session layer below for communication management

The middle layer manages communication patterns and maintains connection state:

McpSession - Core session management interface

McpClientSession - Client-specific session implementation

McpServerSession - Server-specific session implementation

The bottom layer handles the actual message transport and serialization:

McpTransport - Manages JSON-RPC message serialization and deserialization

Supports multiple transport implementations (STDIO, HTTP/SSE, Streamable-HTTP, etc.)

Provides the foundation for all higher-level communication

The MCP Client is a key component in the Model Context Protocol (MCP) architecture, responsible for establishing and managing connections with MCP servers. It implements the client-side of the protocol, handling:

Protocol version negotiation to ensure compatibility with servers

Capability negotiation to determine available features

Message transport and JSON-RPC communication

Tool discovery and execution

Resource access and management

Prompt system interactions

Synchronous and asynchronous operations

Stdio-based transport for process-based communication

Java HttpClient-based SSE client transport

WebFlux SSE client transport for reactive HTTP streaming

The MCP Server is a foundational component in the Model Context Protocol (MCP) architecture that provides tools, resources, and capabilities to clients. It implements the server-side of the protocol, responsible for:

Server-side protocol operations implementation

Tool exposure and discovery

Resource management with URI-based access

Prompt template provision and handling

Capability negotiation with clients

Structured logging and notifications

Concurrent client connection management

Synchronous and Asynchronous API support

Transport implementations:

Stdio, Streamable-HTTP, Stateless Streamable-HTTP, SSE

For detailed implementation guidance, using the low-level MCP Client/Server APIs, refer to the MCP Java SDK documentation. For simplified setup using Spring Boot, use the MCP Boot Starters described below.

Spring AI provides MCP integration through the following Spring Boot starters:

spring-ai-starter-mcp-client - Core starter providing STDIO, Servlet-based Streamable-HTTP, Stateless Streamable-HTTP and SSE support

spring-ai-starter-mcp-client-webflux - WebFlux-based Streamable-HTTP, Stateless Streamable-HTTP and SSE transport implementation

Standard Input/Output (STDIO)

spring-ai-starter-mcp-server

spring.ai.mcp.server.stdio=true

spring-ai-starter-mcp-server-webmvc

spring.ai.mcp.server.protocol=SSE or empty

Streamable-HTTP WebMVC

spring-ai-starter-mcp-server-webmvc

spring.ai.mcp.server.protocol=STREAMABLE

Stateless Streamable-HTTP WebMVC

spring-ai-starter-mcp-server-webmvc

spring.ai.mcp.server.protocol=STATELESS

spring-ai-starter-mcp-server-webflux

spring.ai.mcp.server.protocol=SSE or empty

Streamable-HTTP WebFlux

spring-ai-starter-mcp-server-webflux

spring.ai.mcp.server.protocol=STREAMABLE

Stateless Streamable-HTTP WebFlux

spring-ai-starter-mcp-server-webflux

spring.ai.mcp.server.protocol=STATELESS

In addition to the programmatic MCP client & server configuration, Spring AI provides annotation-based method handling for MCP servers and clients through the MCP Annotations module. This approach simplifies the creation and registration of MCP operations using a clean, declarative programming model with Java annotations.

The MCP Annotations module enables developers to:

Create MCP tools, resources, and prompts using simple annotations

Handle client-side notifications and requests declaratively

Reduce boilerplate code and improve maintainability

Automatically generate JSON schemas for tool parameters

Access special parameters and context information

Key features include:

Server Annotations: @McpTool, @McpResource, @McpPrompt, @McpComplete

Client Annotations: @McpLogging, @McpSampling, @McpElicitation, @McpProgress

Special Parameters: McpSyncServerExchange, McpAsyncServerExchange, McpTransportContext, McpMeta

Automatic Discovery: Annotation scanning with configurable package inclusion/exclusion

Spring Boot Integration: Seamless integration with MCP Boot Starters

MCP Annotations Documentation

MCP Client Boot Starters Documentation

MCP Server Boot Starters Documentation

MCP Utilities Documentation

Model Context Protocol Specification

---

## Model Context Protocol (MCP) :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/mcp/mcp-overview.html

**Contents:**
- Model Context Protocol (MCP)
- MCP Java SDK Architecture
- Spring AI MCP Integration
  - Client Starters
  - Server Starters
- Additional Resources

For the latest snapshot version, please use Spring AI 1.1.2!

The Model Context Protocol (MCP) is a standardized protocol that enables AI models to interact with external tools and resources in a structured way. It supports multiple transport mechanisms to provide flexibility across different environments.

The MCP Java SDK provides a Java implementation of the Model Context Protocol, enabling standardized interaction with AI models and tools through both synchronous and asynchronous communication patterns.

Spring AI MCP extends the MCP Java SDK with Spring Boot integration, providing both client and server starters. Bootstrap your AI applications with MCP support using Spring Initializer.

Breaking Changes in MCP Java SDK 0.8.0 ⚠️

MCP Java SDK version 0.8.0 introduces several breaking changes including a new session-based architecture. If you’re upgrading from Java SDK 0.7.0, please refer to the Migration Guide for detailed instructions.

The Java MCP implementation follows a three-layer architecture:

Client/Server Layer: The McpClient handles client-side operations while the McpServer manages server-side protocol operations. Both utilize McpSession for communication management.

Session Layer (McpSession): Manages communication patterns and state through the McpClientSession and McpServerSession implementations.

Transport Layer (McpTransport): Handles JSON-RPC message serialization and deserialization with support for multiple transport implementations.

The MCP Client is a key component in the Model Context Protocol (MCP) architecture, responsible for establishing and managing connections with MCP servers. It implements the client-side of the protocol, handling:

Protocol version negotiation to ensure compatibility with servers

Capability negotiation to determine available features

Message transport and JSON-RPC communication

Tool discovery and execution

Resource access and management

Prompt system interactions

Synchronous and asynchronous operations

Stdio-based transport for process-based communication

Java HttpClient-based SSE client transport

WebFlux SSE client transport for reactive HTTP streaming

The MCP Server is a foundational component in the Model Context Protocol (MCP) architecture that provides tools, resources, and capabilities to clients. It implements the server-side of the protocol, responsible for:

Server-side protocol operations implementation

Tool exposure and discovery

Resource management with URI-based access

Prompt template provision and handling

Capability negotiation with clients

Structured logging and notifications

Concurrent client connection management

Synchronous and Asynchronous API support

Transport implementations:

Stdio-based transport for process-based communication

Servlet-based SSE server transport

WebFlux SSE server transport for reactive HTTP streaming

WebMVC SSE server transport for servlet-based HTTP streaming

For detailed implementation guidance, using the low-level MCP Client/Server APIs, refer to the MCP Java SDK documentation. For simplified setup using Spring Boot, use the MCP Boot Starters described below.

Spring AI provides MCP integration through the following Spring Boot starters:

spring-ai-starter-mcp-client - Core starter providing STDIO and HTTP-based SSE support

spring-ai-starter-mcp-client-webflux - WebFlux-based SSE transport implementation

spring-ai-starter-mcp-server - Core server with STDIO transport support

spring-ai-starter-mcp-server-webmvc - Spring MVC-based SSE transport implementation

spring-ai-starter-mcp-server-webflux - WebFlux-based SSE transport implementation

MCP Client Boot Starters Documentation

MCP Server Boot Starters Documentation

MCP Utilities Documentation

Model Context Protocol Specification

---

## Getting Started :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/getting-started.html

**Contents:**
- Getting Started
- Spring Initializr
- Artifact Repositories
  - Milestones - Use Maven Central
  - Snapshots - Add Snapshot Repositories
- Dependency Management
- Add dependencies for specific components
- Spring AI samples

For the latest snapshot version, please use Spring AI 1.1.2!

This section offers jumping off points for how to get started using Spring AI.

You should follow the steps in each of the following sections according to your needs.

Head on over to start.spring.io and select the AI Models and Vector Stores that you want to use in your new applications.

As of 1.0.0-M6, releases are available in Maven Central. No changes to your build file are required.

To use the Snapshot (and pre 1.0.0-M6 milestone) versions, you need to add the following snapshot repositories in your build file.

Add the following repository definitions to your Maven or Gradle build file:

NOTE: When using Maven with Spring AI snapshots, pay attention to your Maven mirror configuration. If you have configured a mirror in your settings.xml like this:

The wildcard * will redirect all repository requests to your mirror, preventing access to Spring snapshot repositories. To fix this, modify the mirrorOf configuration to exclude Spring repositories:

This configuration allows Maven to access Spring snapshot repositories directly while still using your mirror for other dependencies.

The Spring AI Bill of Materials (BOM) declares the recommended versions of all the dependencies used by a given release of Spring AI. This is a BOM-only version and it just contains dependency management and no plugin declarations or direct references to Spring or Spring Boot. You can use the Spring Boot parent POM, or use the BOM from Spring Boot (spring-boot-dependencies) to manage Spring Boot versions.

Add the BOM to your project:

Gradle users can also use the Spring AI BOM by leveraging Gradle (5.0+) native support for declaring dependency constraints using a Maven BOM. This is implemented by adding a 'platform' dependency handler method to the dependencies section of your Gradle build script.

Each of the following sections in the documentation shows which dependencies you need to add to your project build system.

Image Generation Models

Text-To-Speech (TTS) Models

Please refer to this page for more resources and samples related to Spring AI.

**Examples:**

Example 1 (xml):
```xml
<repositories>
  <repository>
    <id>spring-snapshots</id>
    <name>Spring Snapshots</name>
    <url>https://repo.spring.io/snapshot</url>
    <releases>
      <enabled>false</enabled>
    </releases>
  </repository>
  <repository>
    <name>Central Portal Snapshots</name>
    <id>central-portal-snapshots</id>
    <url>https://central.sonatype.com/repository/maven-snapshots/</url>
    <releases>
      <enabled>false</enabled>
    </releases>
    <snapshots>
      <enabled>true</enabled>
    </snapshots>
  </repository>
</repositories>
```

Example 2 (unknown):
```unknown
repositories {
  mavenCentral()
  maven { url 'https://repo.spring.io/milestone' }
  maven { url 'https://repo.spring.io/snapshot' }
  maven {
    name = 'Central Portal Snapshots'
    url = 'https://central.sonatype.com/repository/maven-snapshots/'
  }
}
```

Example 3 (xml):
```xml
<mirror>
    <id>my-mirror</id>
    <mirrorOf>*</mirrorOf>
    <url>https://my-company-repository.com/maven</url>
</mirror>
```

Example 4 (xml):
```xml
<mirror>
    <id>my-mirror</id>
    <mirrorOf>*,!spring-snapshots,!central-portal-snapshots</mirrorOf>
    <url>https://my-company-repository.com/maven</url>
</mirror>
```

---

## AI Concepts :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/concepts.html

**Contents:**
- AI Concepts
- Models
- Prompts
  - Prompt Templates
- Embeddings
- Tokens
- Structured Output
- Bringing Your Data & APIs to the AI Model
  - Retrieval Augmented Generation
  - Tool Calling

This section describes core concepts that Spring AI uses. We recommend reading it closely to understand the ideas behind how Spring AI is implemented.

AI models are algorithms designed to process and generate information, often mimicking human cognitive functions. By learning patterns and insights from large datasets, these models can make predictions, text, images, or other outputs, enhancing various applications across industries.

There are many different types of AI models, each suited for a specific use case. While ChatGPT and its generative AI capabilities have captivated users through text input and output, many models and companies offer diverse inputs and outputs. Before ChatGPT, many people were fascinated by text-to-image generation models such as Midjourney and Stable Diffusion.

The following table categorizes several models based on their input and output types:

Spring AI currently supports models that process input and output as language, image, and audio. The last row in the previous table, which accepts text as input and outputs numbers, is more commonly known as embedding text and represents the internal data structures used in an AI model. Spring AI has support for embeddings to enable more advanced use cases.

What sets models like GPT apart is their pre-trained nature, as indicated by the "P" in GPT—Chat Generative Pre-trained Transformer. This pre-training feature transforms AI into a general developer tool that does not require an extensive machine learning or model training background.

Prompts serve as the foundation for the language-based inputs that guide an AI model to produce specific outputs. For those familiar with ChatGPT, a prompt might seem like merely the text entered into a dialog box that is sent to the API. However, it encompasses much more than that. In many AI Models, the text for the prompt is not just a simple string.

ChatGPT’s API has multiple text inputs within a prompt, with each text input being assigned a role. For example, there is the system role, which tells the model how to behave and sets the context for the interaction. There is also the user role, which is typically the input from the user.

Crafting effective prompts is both an art and a science. ChatGPT was designed for human conversations. This is quite a departure from using something like SQL to "ask a question". One must communicate with the AI model akin to conversing with another person.

Such is the importance of this interaction style that the term "Prompt Engineering" has emerged as its own discipline. There is a burgeoning collection of techniques that improve the effectiveness of prompts. Investing time in crafting a prompt can drastically improve the resulting output.

Sharing prompts has become a communal practice, and there is active academic research being done on this subject. As an example of how counter-intuitive it can be to create an effective prompt (for example, contrasting with SQL), a recent research paper found that one of the most effective prompts you can use starts with the phrase, “Take a deep breath and work on this step by step.” That should give you an indication of why language is so important. We do not yet fully understand how to make the most effective use of previous iterations of this technology, such as ChatGPT 3.5, let alone new versions that are being developed.

Creating effective prompts involves establishing the context of the request and substituting parts of the request with values specific to the user’s input.

This process uses traditional text-based template engines for prompt creation and management. Spring AI employs the OSS library StringTemplate for this purpose.

For instance, consider the simple prompt template:

In Spring AI, prompt templates can be likened to the "View" in Spring MVC architecture. A model object, typically a java.util.Map, is provided to populate placeholders within the template. The "rendered" string becomes the content of the prompt supplied to the AI model.

There is considerable variability in the specific data format of the prompt sent to the model. Initially starting as simple strings, prompts have evolved to include multiple messages, where each string in each message represents a distinct role for the model.

Embeddings are numerical representations of text, images, or videos that capture relationships between inputs.

Embeddings work by converting text, image, and video into arrays of floating point numbers, called vectors. These vectors are designed to capture the meaning of the text, images, and videos. The length of the embedding array is called the vector’s dimensionality.

By calculating the numerical distance between the vector representations of two pieces of text, an application can determine the similarity between the objects used to generate the embedding vectors.

As a Java developer exploring AI, it’s not necessary to comprehend the intricate mathematical theories or the specific implementations behind these vector representations. A basic understanding of their role and function within AI systems suffices, particularly when you’re integrating AI functionalities into your applications.

Embeddings are particularly relevant in practical applications like the Retrieval Augmented Generation (RAG) pattern. They enable the representation of data as points in a semantic space, which is akin to the 2-D space of Euclidean geometry, but in higher dimensions. This means just like how points on a plane in Euclidean geometry can be close or far based on their coordinates, in a semantic space, the proximity of points reflects the similarity in meaning. Sentences about similar topics are positioned closer in this multi-dimensional space, much like points lying close to each other on a graph. This proximity aids in tasks like text classification, semantic search, and even product recommendations, as it allows the AI to discern and group related concepts based on their "location" in this expanded semantic landscape.

You can think of this semantic space as a vector.

Tokens serve as the building blocks of how an AI model works. On input, models convert words to tokens. On output, they convert tokens back to words.

In English, one token roughly corresponds to 75% of a word. For reference, Shakespeare’s complete works, totaling around 900,000 words, translate to approximately 1.2 million tokens.

Perhaps more important is that Tokens = Money. In the context of hosted AI models, your charges are determined by the number of tokens used. Both input and output contribute to the overall token count.

Also, models are subject to token limits, which restrict the amount of text processed in a single API call. This threshold is often referred to as the "context window". The model does not process any text that exceeds this limit.

For instance, ChatGPT3 has a 4K token limit, while GPT4 offers varying options, such as 8K, 16K, and 32K. Anthropic’s Claude AI model features a 100K token limit, and Meta’s recent research yielded a 1M token limit model.

To summarize the collected works of Shakespeare with GPT4, you need to devise software engineering strategies to chop up the data and present the data within the model’s context window limits. The Spring AI project helps you with this task.

The output of AI models traditionally arrives as a java.lang.String, even if you ask for the reply to be in JSON. It may be a correct JSON, but it is not a JSON data structure. It is just a string. Also, asking “for JSON” as part of the prompt is not 100% accurate.

This intricacy has led to the emergence of a specialized field involving the creation of prompts to yield the intended output, followed by converting the resulting simple string into a usable data structure for application integration.

The Structured output conversion employs meticulously crafted prompts, often necessitating multiple interactions with the model to achieve the desired formatting.

How can you equip the AI model with information on which it has not been trained?

Note that the GPT 3.5/4.0 dataset extends only until September 2021. Consequently, the model says that it does not know the answer to questions that require knowledge beyond that date. An interesting bit of trivia is that this dataset is around 650GB.

Three techniques exist for customizing the AI model to incorporate your data:

Fine Tuning: This traditional machine learning technique involves tailoring the model and changing its internal weighting. However, it is a challenging process for machine learning experts and extremely resource-intensive for models like GPT due to their size. Additionally, some models might not offer this option.

Prompt Stuffing: A more practical alternative involves embedding your data within the prompt provided to the model. Given a model’s token limits, techniques are required to present relevant data within the model’s context window. This approach is colloquially referred to as “stuffing the prompt.” The Spring AI library helps you implement solutions based on the “stuffing the prompt” technique otherwise known as Retrieval Augmented Generation (RAG).

Tool Calling: This technique allows registering tools (user-defined services) that connect the large language models to the APIs of external systems. Spring AI greatly simplifies code you need to write to support tool calling.

A technique termed Retrieval Augmented Generation (RAG) has emerged to address the challenge of incorporating relevant data into prompts for accurate AI model responses.

The approach involves a batch processing style programming model, where the job reads unstructured data from your documents, transforms it, and then writes it into a vector database. At a high level, this is an ETL (Extract, Transform and Load) pipeline. The vector database is used in the retrieval part of RAG technique.

As part of loading the unstructured data into the vector database, one of the most important transformations is to split the original document into smaller pieces. The procedure of splitting the original document into smaller pieces has two important steps:

Split the document into parts while preserving the semantic boundaries of the content. For example, for a document with paragraphs and tables, one should avoid splitting the document in the middle of a paragraph or table. For code, avoid splitting the code in the middle of a method’s implementation.

Split the document’s parts further into parts whose size is a small percentage of the AI Model’s token limit.

The next phase in RAG is processing user input. When a user’s question is to be answered by an AI model, the question and all the “similar” document pieces are placed into the prompt that is sent to the AI model. This is the reason to use a vector database. It is very good at finding similar content.

The ETL Pipeline provides further information about orchestrating the flow of extracting data from data sources and storing it in a structured vector store, ensuring data is in the optimal format for retrieval when passing it to the AI model.

The ChatClient - RAG explains how to use the QuestionAnswerAdvisor to enable the RAG capability in your application.

Large Language Models (LLMs) are frozen after training, leading to stale knowledge, and they are unable to access or modify external data.

The Tool Calling mechanism addresses these shortcomings. It allows you to register your own services as tools to connect the large language models to the APIs of external systems. These systems can provide LLMs with real-time data and perform data processing actions on their behalf.

Spring AI greatly simplifies code you need to write to support tool invocation. It handles the tool invocation conversation for you. You can provide your tool as a @Tool-annotated method and provide it in your prompt options to make it available to the model. Additionally, you can define and reference multiple tools in a single prompt.

When we want to make a tool available to the model, we include its definition in the chat request. Each tool definition comprises of a name, a description, and the schema of the input parameters.

When the model decides to call a tool, it sends a response with the tool name and the input parameters modeled after the defined schema.

The application is responsible for using the tool name to identify and execute the tool with the provided input parameters.

The result of the tool call is processed by the application.

The application sends the tool call result back to the model.

The model generates the final response using the tool call result as additional context.

Follow the Tool Calling documentation for further information on how to use this feature with different AI models.

Effectively evaluating the output of an AI system in response to user requests is very important to ensuring the accuracy and usefulness of the final application. Several emerging techniques enable the use of the pre-trained model itself for this purpose.

This evaluation process involves analyzing whether the generated response aligns with the user’s intent and the context of the query. Metrics such as relevance, coherence, and factual correctness are used to gauge the quality of the AI-generated response.

One approach involves presenting both the user’s request and the AI model’s response to the model, querying whether the response aligns with the provided data.

Furthermore, leveraging the information stored in the vector database as supplementary data can enhance the evaluation process, aiding in the determination of response relevance.

The Spring AI project provides an Evaluator API which currently gives access to basic strategies to evaluate model responses. Follow the Evaluation Testing documentation for further information.

**Examples:**

Example 1 (unknown):
```unknown
Tell me a {adjective} joke about {content}.
```

---

## Getting Started with Model Context Protocol (MCP) :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/guides/getting-started-mcp.html

**Contents:**
- Getting Started with Model Context Protocol (MCP)
- Introduction Video
- Complete Tutorial and Source Code
- Quick Start
  - Simple MCP Server
  - Simple MCP Client
- Learning Resources
  - Implementation Video
- Additional Examples Repository
  - Recommended Starting Points

The Model Context Protocol (MCP) standardizes how AI applications interact with external tools and resources.

Spring joined the MCP ecosystem early as a key contributor, helping to develop and maintain the official MCP Java SDK that serves as the foundation for Java-based MCP implementations. Building on this contribution, Spring AI provides MCP support through Boot Starters and annotations, making it easy to build both MCP servers and clients.

Introduction to Model Context Protocol (MCP) - YouTube

Start here for an introductory overview of the Model Context Protocol, explaining core concepts and architecture.

📖 Blog Tutorial: Connect Your AI to Everything

💻 Complete Source Code: MCP Weather Example Repository

The tutorial covers the essentials of MCP development with Spring AI, including advanced features, and deployment patterns. All code examples below are taken from this tutorial.

The fastest way to get started is with Spring AI’s annotation-based approach. The following examples are from the blog tutorial:

Add the dependency and configure:

Add the dependency and configure:

Spring AI Model Context Protocol (MCP) Integration - YouTube

A video walkthrough of Spring AI’s MCP integration, covering both server and client implementations.

Beyond the tutorial examples, the Spring AI Examples repository contains numerous MCP implementations.

Annotation-based examples

Complete Annotations Example - All annotation features (Client & Server)

Sampling with Annotations - Advanced bidirectional AI (Client & Server)

MCP Weather Tutorial - Full tutorial source code (Client & Server)

WebFlux Weather Server

OAuth2 Secured Weather Server

Filesystem Access Server

Awesome Spring AI - Community examples and resources

Official MCP Specification

Official MCP Java SDK - Java SDK developed by the Spring team

MCP Java SDK Documentation

MCP Overview and Architecture

MCP Annotations Guide

**Examples:**

Example 1 (java):
```java
@Service
public class WeatherService {

    @McpTool(description = "Get current temperature for a location")
    public String getTemperature(
            @McpToolParam(description = "City name", required = true) String city) {
        return String.format("Current temperature in %s: 22°C", city);
    }
}
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webmvc</artifactId>
</dependency>
```

Example 3 (unknown):
```unknown
spring.ai.mcp.server.protocol=STREAMABLE
```

Example 4 (java):
```java
@Bean
public CommandLineRunner demo(ChatClient chatClient, ToolCallbackProvider mcpTools) {
    return args -> {
        String response = chatClient
            .prompt("What's the weather like in Paris?")
            .toolCallbacks(mcpTools)
            .call()
            .content();
        System.out.println(response);
    };
}
```

---
