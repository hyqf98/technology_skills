# Springai - Mcp

**Pages:** 25

---

## MCP Annotations Special Parameters :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/mcp/mcp-annotations-special-params.html

**Contents:**
- MCP Annotations Special Parameters
- Special Parameter Types
  - McpMeta
    - Overview
    - Usage in Tools
    - Usage in Resources
    - Usage in Prompts
  - @McpProgressToken
    - Overview
    - Usage in Tools

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The MCP Annotations support several special parameter types that provide additional context and functionality to annotated methods. These parameters are automatically injected by the framework and are excluded from JSON schema generation.

The McpMeta class provides access to metadata from MCP requests, notifications, and results.

Automatically injected when used as a method parameter

Excluded from parameter count limits and JSON schema generation

Provides convenient access to metadata through the get(String key) method

If no metadata is present in the request, an empty McpMeta object is injected

The @McpProgressToken annotation marks a parameter to receive progress tokens from MCP requests.

Parameter type should be String

Automatically receives the progress token value from the request

Excluded from the generated JSON schema

If no progress token is present, null is injected

Used for tracking long-running operations

Request context objects provide unified access to MCP request information and server-side operations.

Provides unified interface for both stateful and stateless operations

Automatically injected when used as a parameter

Excluded from JSON schema generation

Enables advanced features like logging, progress notifications, sampling, and elicitation

Works with both stateful (server exchange) and stateless (transport context) modes

Lightweight context for stateless operations.

Provides minimal context without full server exchange

Used in stateless implementations

Automatically injected when used as a parameter

Excluded from JSON schema generation

Special parameter for tools that need access to the full request with dynamic schema.

Provides access to the complete tool request

Enables dynamic schema handling at runtime

Automatically injected and excluded from schema generation

Useful for flexible tools that adapt to different input schemas

The following parameters are automatically injected by the framework:

McpMeta - Metadata from the request

@McpProgressToken String - Progress token if available

McpSyncServerExchange / McpAsyncServerExchange - Server exchange context

McpTransportContext - Transport context for stateless operations

CallToolRequest - Full tool request for dynamic schema

Special parameters are excluded from JSON schema generation:

They don’t appear in the tool’s input schema

They don’t count towards parameter limits

They’re not visible to MCP clients

McpMeta - Never null, empty object if no metadata

@McpProgressToken - Can be null if no token provided

Server exchanges - Never null when properly configured

CallToolRequest - Never null for tool methods

Use McpSyncRequestContext / McpAsyncRequestContext for unified access to request context, supporting both stateful and stateless operations with convenient helper methods

Use McpTransportContext for simple stateless operations when you only need transport-level context

Omit context parameters entirely for the simplest cases

Always check capability support before using client features:

MCP Annotations Overview

**Examples:**

Example 1 (java):
```java
@McpTool(name = "contextual-tool", description = "Tool with metadata access")
public String processWithContext(
        @McpToolParam(description = "Input data", required = true) String data,
        McpMeta meta) {

    // Access metadata from the request
    String userId = (String) meta.get("userId");
    String sessionId = (String) meta.get("sessionId");
    String userRole = (String) meta.get("userRole");

    // Use metadata to customize behavior
    if ("admin".equals(userRole)) {
        return processAsAdmin(data, userId);
    } else {
        return processAsUser(data, userId);
    }
}
```

Example 2 (java):
```java
@McpResource(uri = "secure-data://{id}", name = "Secure Data")
public ReadResourceResult getSecureData(String id, McpMeta meta) {

    String requestingUser = (String) meta.get("requestingUser");
    String accessLevel = (String) meta.get("accessLevel");

    // Check access permissions using metadata
    if (!"admin".equals(accessLevel)) {
        return new ReadResourceResult(List.of(
            new TextResourceContents("secure-data://" + id,
                "text/plain", "Access denied")
        ));
    }

    String data = loadSecureData(id);
    return new ReadResourceResult(List.of(
        new TextResourceContents("secure-data://" + id,
            "text/plain", data)
    ));
}
```

Example 3 (java):
```java
@McpPrompt(name = "localized-prompt", description = "Localized prompt generation")
public GetPromptResult localizedPrompt(
        @McpArg(name = "topic", required = true) String topic,
        McpMeta meta) {

    String language = (String) meta.get("language");
    String region = (String) meta.get("region");

    // Generate localized content based on metadata
    String message = generateLocalizedMessage(topic, language, region);

    return new GetPromptResult("Localized Prompt",
        List.of(new PromptMessage(Role.ASSISTANT, new TextContent(message)))
    );
}
```

Example 4 (java):
```java
@McpTool(name = "long-operation", description = "Long-running operation with progress")
public String performLongOperation(
        @McpProgressToken String progressToken,
        @McpToolParam(description = "Operation name", required = true) String operation,
        @McpToolParam(description = "Duration in seconds", required = true) int duration,
        McpSyncServerExchange exchange) {

    if (progressToken != null) {
        // Send initial progress
        exchange.progressNotification(new ProgressNotification(
            progressToken, 0.0, 1.0, "Starting " + operation));

        // Simulate work with progress updates
        for (int i = 1; i <= duration; i++) {
            Thread.sleep(1000);
            double progress = (double) i / duration;

            exchange.progressNotification(new ProgressNotification(
                progressToken, progress, 1.0,
                String.format("Processing... %d%%", (int)(progress * 100))));
        }
    }

    return "Operation " + operation + " completed";
}
```

---

## MCP Server Boot Starter :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/mcp/mcp-server-boot-starter-docs.html

**Contents:**
- MCP Server Boot Starter
- Starters
  - Standard MCP Server
  - WebMVC Server Transport
  - WebFlux Server Transport
- Sync/Async Server Types
- Server Capabilities
- Transport Options
- Features and Capabilities
  - Tools

For the latest snapshot version, please use Spring AI 1.1.2!

The Spring AI MCP (Model Context Protocol) Server Boot Starter provides auto-configuration for setting up an MCP server in Spring Boot applications. It enables seamless integration of MCP server capabilities with Spring Boot’s auto-configuration system.

The MCP Server Boot Starter offers:

Automatic configuration of MCP server components

Support for both synchronous and asynchronous operation modes

Multiple transport layer options

Flexible tool, resource, and prompt specification

Change notification capabilities

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Choose one of the following starters based on your transport requirements:

Full MCP Server features support with STDIO server transport.

Suitable for command-line and desktop tools

No additional web dependencies required

The starter activates the McpServerAutoConfiguration auto-configuration responsible for:

Configuring the basic server components

Handling tool, resource, and prompt specifications

Managing server capabilities and change notifications

Providing both sync and async server implementations

Full MCP Server features support with SSE (Server-Sent Events) server transport based on Spring MVC and an optional STDIO transport.

The starter activates the McpWebMvcServerAutoConfiguration and McpServerAutoConfiguration auto-configurations to provide:

HTTP-based transport using Spring MVC (WebMvcSseServerTransportProvider)

Automatically configured SSE endpoints

Optional STDIO transport (enabled by setting spring.ai.mcp.server.stdio=true)

Included spring-boot-starter-web and mcp-spring-webmvc dependencies

Full MCP Server features support with SSE (Server-Sent Events) server transport based on Spring WebFlux and an optional STDIO transport.

The starter activates the McpWebFluxServerAutoConfiguration and McpServerAutoConfiguration auto-configurations to provide:

Reactive transport using Spring WebFlux (WebFluxSseServerTransportProvider)

Automatically configured reactive SSE endpoints

Optional STDIO transport (enabled by setting spring.ai.mcp.server.stdio=true)

Included spring-boot-starter-webflux and mcp-spring-webflux dependencies

Due to Spring Boot’s default behavior, when both org.springframework.web.servlet.DispatcherServlet and org.springframework.web.reactive.DispatcherHandler are present on the classpath, Spring Boot will prioritize DispatcherServlet. As a result, if your project uses spring-boot-starter-web, it is recommended to use spring-ai-starter-mcp-server-webmvc instead of spring-ai-starter-mcp-server-webflux.

Configuration Properties

All properties are prefixed with spring.ai.mcp.server:

Enable/disable the MCP server

Enable/disable stdio transport

Server name for identification

Optional instructions to provide guidance to the client on how to interact with this server

Server type (SYNC/ASYNC)

capabilities.resource

Enable/disable resource capabilities

Enable/disable tool capabilities

Enable/disable prompt capabilities

capabilities.completion

Enable/disable completion capabilities

resource-change-notification

Enable resource change notifications

prompt-change-notification

Enable prompt change notifications

tool-change-notification

Enable tool change notifications

tool-response-mime-type

(optional) response MIME type per tool name. For example spring.ai.mcp.server.tool-response-mime-type.generateImage=image/png will associate the image/png mime type with the generateImage() tool name

Custom SSE Message endpoint path for web transport to be used by the client to send messages

Custom SSE endpoint path for web transport

Optional URL prefix. For example base-url=/api/v1 means that the client should access the sse endpoint at /api/v1 + sse-endpoint and the message endpoint is /api/v1 + sse-message-endpoint

Duration to wait for server responses before timing out requests. Applies to all requests made through the client, including tool calls, resource access, and prompt operations.

Synchronous Server - The default server type implemented using McpSyncServer. It is designed for straightforward request-response patterns in your applications. To enable this server type, set spring.ai.mcp.server.type=SYNC in your configuration. When activated, it automatically handles the configuration of synchronous tool specifications.

Asynchronous Server - The asynchronous server implementation uses McpAsyncServer and is optimized for non-blocking operations. To enable this server type, configure your application with spring.ai.mcp.server.type=ASYNC. This server type automatically sets up asynchronous tool specifications with built-in Project Reactor support.

The MCP Server supports four main capability types that can be individually enabled or disabled:

Tools - Enable/disable tool capabilities with spring.ai.mcp.server.capabilities.tool=true|false

Resources - Enable/disable resource capabilities with spring.ai.mcp.server.capabilities.resource=true|false

Prompts - Enable/disable prompt capabilities with spring.ai.mcp.server.capabilities.prompt=true|false

Completions - Enable/disable completion capabilities with spring.ai.mcp.server.capabilities.completion=true|false

All capabilities are enabled by default. Disabling a capability will prevent the server from registering and exposing the corresponding features to clients.

The MCP Server supports three transport mechanisms, each with its dedicated starter:

Standard Input/Output (STDIO) - spring-ai-starter-mcp-server

Spring MVC (Server-Sent Events) - spring-ai-starter-mcp-server-webmvc

Spring WebFlux (Reactive SSE) - spring-ai-starter-mcp-server-webflux

The MCP Server Boot Starter allows servers to expose tools, resources, and prompts to clients. It automatically converts custom capability handlers registered as Spring beans to sync/async specifications based on server type:

Allows servers to expose tools that can be invoked by language models. The MCP Server Boot Starter provides:

Change notification support

Spring AI Tools are automatically converted to sync/async specifications based on server type

Automatic tool specification through Spring beans:

or using the low-level API:

The auto-configuration will automatically detect and register all tool callbacks from: * Individual ToolCallback beans * Lists of ToolCallback beans * ToolCallbackProvider beans

Tools are de-duplicated by name, with the first occurrence of each tool name being used.

The ToolContext is supported, allowing contextual information to be passed to tool calls. It contains an McpSyncServerExchange instance under the exchange key, accessible via McpToolUtils.getMcpExchange(toolContext). See this example demonstrating exchange.loggingNotification(…​) and exchange.createMessage(…​).

Provides a standardized way for servers to expose resources to clients.

Static and dynamic resource specifications

Optional change notifications

Support for resource templates

Automatic conversion between sync/async resource specifications

Automatic resource specification through Spring beans:

Provides a standardized way for servers to expose prompt templates to clients.

Change notification support

Automatic conversion between sync/async prompt specifications

Automatic prompt specification through Spring beans:

Provides a standardized way for servers to expose completion capabilities to clients.

Support for both sync and async completion specifications

Automatic registration through Spring beans:

When roots change, clients that support listChanged send a Root Change notification.

Support for monitoring root changes

Automatic conversion to async consumers for reactive applications

Optional registration through Spring beans

The auto-configuration will automatically register the tool callbacks as MCP tools. You can have multiple beans producing ToolCallbacks. The auto-configuration will merge them.

Weather Server (WebFlux) - Spring AI MCP Server Boot Starter with WebFlux transport.

Weather Server (STDIO) - Spring AI MCP Server Boot Starter with STDIO transport.

Weather Server Manual Configuration - Spring AI MCP Server Boot Starter that doesn’t use auto-configuration but the Java SDK to configure the server manually.

Spring AI Documentation

Model Context Protocol Specification

Spring Boot Auto-configuration

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server</artifactId>
</dependency>
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webmvc</artifactId>
</dependency>
```

Example 3 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webflux</artifactId>
</dependency>
```

Example 4 (java):
```java
@Bean
public ToolCallbackProvider myTools(...) {
    List<ToolCallback> tools = ...
    return ToolCallbackProvider.from(tools);
}
```

---

## MCP Security :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/mcp/mcp-security.html

**Contents:**
- MCP Security
- Overview
- MCP Server Security
  - Dependencies
  - OAuth 2.0 Configuration
    - Basic OAuth 2.0 Setup
    - Securing Tool Calls Only
  - API Key Authentication
  - Known Limitations
- MCP Client Security

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The Spring AI MCP Security module provides comprehensive OAuth 2.0 and API key-based security support for Model Context Protocol implementations in Spring AI. This community-driven project enables developers to secure both MCP servers and clients with industry-standard authentication and authorization mechanisms.

The MCP Security module provides three main components:

MCP Server Security - OAuth 2.0 resource server and API key authentication for Spring AI MCP servers

MCP Client Security - OAuth 2.0 client support for Spring AI MCP clients

MCP Authorization Server - Enhanced Spring Authorization Server with MCP-specific features

The project enables developers to:

Secure MCP servers with OAuth 2.0 authentication and API key-based access

Configure MCP clients with OAuth 2.0 authorization flows

Set up authorization servers specifically designed for MCP workflows

Implement fine-grained access control for MCP tools and resources

The MCP Server Security module provides OAuth 2.0 resource server capabilities for Spring AI’s MCP servers. It also provides basic support for API-key based authentication.

Add the following dependencies to your project:

First, enable the MCP server in your application.properties:

Then, configure security using Spring Security’s standard APIs with the provided MCP configurer:

You can configure the server to secure only tool calls while leaving other MCP operations (like initialize and tools/list) public:

Then, secure your tool calls using the @PreAuthorize annotation with method security:

You can also access the current authentication directly from the tool method using SecurityContextHolder:

The MCP Server Security module also supports API key-based authentication. You need to provide your own implementation of ApiKeyEntityRepository for storing ApiKeyEntity objects.

A sample implementation is available with InMemoryApiKeyEntityRepository along with a default ApiKeyEntityImpl:

With this configuration, you can call your MCP server with a header X-API-key: api01.mycustomapikey.

The deprecated SSE transport is not supported. Use Streamable HTTP or stateless transport.

WebFlux-based servers are not supported.

Opaque tokens are not supported. Use JWT.

The MCP Client Security module provides OAuth 2.0 support for Spring AI’s MCP clients, supporting both HttpClient-based clients (from spring-ai-starter-mcp-client) and WebClient-based clients (from spring-ai-starter-mcp-client-webflux).

Three OAuth 2.0 flows are available for obtaining tokens:

Authorization Code Flow - For user-level permissions when every MCP request is made within the context of a user request

Client Credentials Flow - For machine-to-machine use cases where no human is in the loop

Hybrid Flow - Combines both flows for scenarios where some operations (like initialize or tools/list) happen without a user present, but tool calls require user-level permissions

For all flows, activate Spring Security’s OAuth2 client support in your application.properties:

Then, create a configuration class activating OAuth2 client capabilities:

When using spring-ai-starter-mcp-client, configure a McpSyncHttpClientRequestCustomizer bean:

Available customizers:

OAuth2AuthorizationCodeSyncHttpRequestCustomizer - For authorization code flow

OAuth2ClientCredentialsSyncHttpRequestCustomizer - For client credentials flow

OAuth2HybridSyncHttpRequestCustomizer - For hybrid flow

When using spring-ai-starter-mcp-client-webflux, configure a WebClient.Builder with an MCP ExchangeFilterFunction:

Available filter functions:

McpOAuth2AuthorizationCodeExchangeFilterFunction - For authorization code flow

McpOAuth2ClientCredentialsExchangeFilterFunction - For client credentials flow

McpOAuth2HybridExchangeFilterFunction - For hybrid flow

Spring AI’s autoconfiguration initializes MCP clients at startup, which can cause issues with user-based authentication. To avoid this:

Disable Spring AI’s @Tool autoconfiguration by publishing an empty ToolCallbackResolver bean:

Configure MCP clients programmatically instead of using Spring Boot properties. For HttpClient-based clients:

For WebClient-based clients:

Then add the client to your chat client:

Spring WebFlux servers are not supported.

Spring AI autoconfiguration initializes MCP clients at app start, requiring workarounds for user-based authentication.

Unlike the server module, the client implementation supports the SSE transport with both HttpClient and WebClient.

The MCP Authorization Server module enhances Spring Security’s OAuth 2.0 Authorization Server with features relevant to the MCP authorization spec, such as Dynamic Client Registration and Resource Indicators.

Configure the authorization server in your application.yml:

Then activate the authorization server capabilities with a security filter chain:

Spring WebFlux servers are not supported.

Every client supports ALL resource identifiers.

The samples directory contains working examples for all modules in this project, including integration tests.

With mcp-server-security and a supporting mcp-authorization-server, you can integrate with:

MCP Authorization Specification

MCP Security GitHub Repository

MCP Authorization Specification

Spring Security OAuth 2.0 Resource Server

Spring Security OAuth 2.0 Client

Spring Authorization Server

**Examples:**

Example 1 (xml):
```xml
<dependencies>
    <dependency>
        <groupId>org.springaicommunity</groupId>
        <artifactId>mcp-server-security</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>

    <!-- OPTIONAL: For OAuth2 support -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-oauth2-resource-server</artifactId>
    </dependency>
</dependencies>
```

Example 2 (unknown):
```unknown
implementation 'org.springaicommunity:mcp-server-security'
implementation 'org.springframework.boot:spring-boot-starter-security'

// OPTIONAL: For OAuth2 support
implementation 'org.springframework.boot:spring-boot-starter-oauth2-resource-server'
```

Example 3 (markdown):
```markdown
spring.ai.mcp.server.name=my-cool-mcp-server
# Supported protocols: STREAMABLE, STATELESS
spring.ai.mcp.server.protocol=STREAMABLE
```

Example 4 (java):
```java
@Configuration
@EnableWebSecurity
class McpServerConfiguration {

    @Value("${spring.security.oauth2.resourceserver.jwt.issuer-uri}")
    private String issuerUrl;

    @Bean
    SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http
                // Enforce authentication with token on EVERY request
                .authorizeHttpRequests(auth -> auth.anyRequest().authenticated())
                // Configure OAuth2 on the MCP server
                .with(
                        McpServerOAuth2Configurer.mcpServerOAuth2(),
                        (mcpAuthorization) -> {
                            // REQUIRED: the issuerURI
                            mcpAuthorization.authorizationServer(issuerUrl);
                            // OPTIONAL: enforce the `aud` claim in the JWT token.
                            // Not all authorization servers support resource indicators,
                            // so it may be absent. Defaults to `false`.
                            // See RFC 8707 Resource Indicators for OAuth 2.0
                            // https://www.rfc-editor.org/rfc/rfc8707.html
                            mcpAuthorization.validateAudienceClaim(true);
                        }
                )
                .build();
    }
}
```

---

## Untitled :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/mcp/mcp-streamable-http-server-boot-starter-docs.html

**Contents:**
- Streamable-HTTP MCP Servers
  - Streamable-HTTP WebMVC Server
  - Streamable-HTTP WebFlux Server
- Configuration Properties
  - Common Properties
  - MCP Annotations Properties
  - Streamable-HTTP Properties
- Features and Capabilities
  - Tools
    - Tool Context Support

The Streamable HTTP transport allows MCP servers to operate as independent processes that can handle multiple client connections using HTTP POST and GET requests, with optional Server-Sent Events (SSE) streaming for multiple server messages. It replaces the SSE transport.

These servers, introduced with spec version 2025-03-26, are ideal for applications that need to notify clients about dynamic changes to tools, resources, or prompts.

Use the spring-ai-starter-mcp-server-webmvc dependency:

and set the spring.ai.mcp.server.protocol property to STREAMABLE.

Full MCP server capabilities with Spring MVC Streamable transport

Support for tools, resources, prompts, completion, logging, progression, ping, root-changes capabilities

Persistent connection management

Use the spring-ai-starter-mcp-server-webflux dependency:

and set the spring.ai.mcp.server.protocol property to STREAMABLE.

Reactive MCP server with WebFlux Streamable transport

Support for tools, resources, prompts, completion, logging, progression, ping, root-changes capabilities

Non-blocking, persistent connection management

All common properties are prefixed with spring.ai.mcp.server:

Enable/disable the streamable MCP server

Must be set to STREAMABLE to enable the streamable server

tool-callback-converter

Enable/disable the conversion of Spring AI ToolCallbacks into MCP Tool specs

Server name for identification

Optional instructions for client interaction

Server type (SYNC/ASYNC)

capabilities.resource

Enable/disable resource capabilities

Enable/disable tool capabilities

Enable/disable prompt capabilities

capabilities.completion

Enable/disable completion capabilities

resource-change-notification

Enable resource change notifications

prompt-change-notification

Enable prompt change notifications

tool-change-notification

Enable tool change notifications

tool-response-mime-type

Response MIME type per tool name

Request timeout duration

MCP Server Annotations provide a declarative way to implement MCP server handlers using Java annotations.

The server mcp-annotations properties are prefixed with spring.ai.mcp.server.annotation-scanner:

Enable/disable the MCP server annotations auto-scanning

All streamable-HTTP properties are prefixed with spring.ai.mcp.server.streamable-http:

Custom MCP endpoint path

Connection keep-alive interval

Disallow delete operations

The MCP Server supports four main capability types that can be individually enabled or disabled:

Tools - Enable/disable tool capabilities with spring.ai.mcp.server.capabilities.tool=true|false

Resources - Enable/disable resource capabilities with spring.ai.mcp.server.capabilities.resource=true|false

Prompts - Enable/disable prompt capabilities with spring.ai.mcp.server.capabilities.prompt=true|false

Completions - Enable/disable completion capabilities with spring.ai.mcp.server.capabilities.completion=true|false

All capabilities are enabled by default. Disabling a capability will prevent the server from registering and exposing the corresponding features to clients.

The MCP Server Boot Starter allows servers to expose tools, resources, and prompts to clients. It automatically converts custom capability handlers registered as Spring beans to sync/async specifications based on the server type:

Allows servers to expose tools that can be invoked by language models. The MCP Server Boot Starter provides:

Change notification support

Spring AI Tools are automatically converted to sync/async specifications based on the server type

Automatic tool specification through Spring beans:

or using the low-level API:

The auto-configuration will automatically detect and register all tool callbacks from:

Individual ToolCallback beans

Lists of ToolCallback beans

ToolCallbackProvider beans

Tools are de-duplicated by name, with the first occurrence of each tool name being used.

The ToolContext is supported, allowing contextual information to be passed to tool calls. It contains an McpSyncServerExchange instance under the exchange key, accessible via McpToolUtils.getMcpExchange(toolContext). See this example demonstrating exchange.loggingNotification(…​) and exchange.createMessage(…​).

Provides a standardized way for servers to expose resources to clients.

Static and dynamic resource specifications

Optional change notifications

Support for resource templates

Automatic conversion between sync/async resource specifications

Automatic resource specification through Spring beans:

Provides a standardized way for servers to expose prompt templates to clients.

Change notification support

Automatic conversion between sync/async prompt specifications

Automatic prompt specification through Spring beans:

Provides a standardized way for servers to expose completion capabilities to clients.

Support for both sync and async completion specifications

Automatic registration through Spring beans:

Provides a standardized way for servers to send structured log messages to clients. From within the tool, resource, prompt or completion call handler use the provided McpSyncServerExchange/McpAsyncServerExchange exchange object to send logging messages:

On the MCP client you can register logging consumers to handle these messages:

Provides a standardized way for servers to send progress updates to clients. From within the tool, resource, prompt or completion call handler use the provided McpSyncServerExchange/McpAsyncServerExchange exchange object to send progress notifications:

The Mcp Client can receive progress notifications and update its UI accordingly. For this it needs to register a progress consumer.

When roots change, clients that support listChanged send a root change notification.

Support for monitoring root changes

Automatic conversion to async consumers for reactive applications

Optional registration through Spring beans

Ping mechanism for the server to verify that its clients are still alive. From within the tool, resource, prompt or completion call handler use the provided McpSyncServerExchange/McpAsyncServerExchange exchange object to send ping messages:

Server can optionally, periodically issue pings to connected clients to verify connection health.

By default, keep-alive is disabled. To enable keep-alive, set the keep-alive-interval property in your configuration:

The auto-configuration will automatically register the tool callbacks as MCP tools. You can have multiple beans producing ToolCallbacks, and the auto-configuration will merge them.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webmvc</artifactId>
</dependency>
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webflux</artifactId>
</dependency>
```

Example 3 (java):
```java
@Bean
public ToolCallbackProvider myTools(...) {
    List<ToolCallback> tools = ...
    return ToolCallbackProvider.from(tools);
}
```

Example 4 (java):
```java
@Bean
public List<McpServerFeatures.SyncToolSpecification> myTools(...) {
    List<McpServerFeatures.SyncToolSpecification> tools = ...
    return tools;
}
```

---

## Untitled :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/mcp/mcp-streamable-http-server-boot-starter-docs.html

**Contents:**
- Streamable-HTTP MCP Servers
  - Streamable-HTTP WebMVC Server
  - Streamable-HTTP WebFlux Server
- Configuration Properties
  - Common Properties
  - MCP Annotations Properties
  - Streamable-HTTP Properties
- Features and Capabilities
  - Tools
    - Tool Context Support

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The Streamable HTTP transport allows MCP servers to operate as independent processes that can handle multiple client connections using HTTP POST and GET requests, with optional Server-Sent Events (SSE) streaming for multiple server messages. It replaces the SSE transport.

These servers, introduced with spec version 2025-03-26, are ideal for applications that need to notify clients about dynamic changes to tools, resources, or prompts.

Use the spring-ai-starter-mcp-server-webmvc dependency:

and set the spring.ai.mcp.server.protocol property to STREAMABLE.

Full MCP server capabilities with Spring MVC Streamable transport

Support for tools, resources, prompts, completion, logging, progression, ping, root-changes capabilities

Persistent connection management

Use the spring-ai-starter-mcp-server-webflux dependency:

and set the spring.ai.mcp.server.protocol property to STREAMABLE.

Reactive MCP server with WebFlux Streamable transport

Support for tools, resources, prompts, completion, logging, progression, ping, root-changes capabilities

Non-blocking, persistent connection management

All common properties are prefixed with spring.ai.mcp.server:

Enable/disable the streamable MCP server

Must be set to STREAMABLE to enable the streamable server

tool-callback-converter

Enable/disable the conversion of Spring AI ToolCallbacks into MCP Tool specs

Server name for identification

Optional instructions for client interaction

Server type (SYNC/ASYNC)

capabilities.resource

Enable/disable resource capabilities

Enable/disable tool capabilities

Enable/disable prompt capabilities

capabilities.completion

Enable/disable completion capabilities

resource-change-notification

Enable resource change notifications

prompt-change-notification

Enable prompt change notifications

tool-change-notification

Enable tool change notifications

tool-response-mime-type

Response MIME type per tool name

Request timeout duration

MCP Server Annotations provide a declarative way to implement MCP server handlers using Java annotations.

The server mcp-annotations properties are prefixed with spring.ai.mcp.server.annotation-scanner:

Enable/disable the MCP server annotations auto-scanning

All streamable-HTTP properties are prefixed with spring.ai.mcp.server.streamable-http:

Custom MCP endpoint path

Connection keep-alive interval

Disallow delete operations

The MCP Server supports four main capability types that can be individually enabled or disabled:

Tools - Enable/disable tool capabilities with spring.ai.mcp.server.capabilities.tool=true|false

Resources - Enable/disable resource capabilities with spring.ai.mcp.server.capabilities.resource=true|false

Prompts - Enable/disable prompt capabilities with spring.ai.mcp.server.capabilities.prompt=true|false

Completions - Enable/disable completion capabilities with spring.ai.mcp.server.capabilities.completion=true|false

All capabilities are enabled by default. Disabling a capability will prevent the server from registering and exposing the corresponding features to clients.

The MCP Server Boot Starter allows servers to expose tools, resources, and prompts to clients. It automatically converts custom capability handlers registered as Spring beans to sync/async specifications based on the server type:

Allows servers to expose tools that can be invoked by language models. The MCP Server Boot Starter provides:

Change notification support

Spring AI Tools are automatically converted to sync/async specifications based on the server type

Automatic tool specification through Spring beans:

or using the low-level API:

The auto-configuration will automatically detect and register all tool callbacks from:

Individual ToolCallback beans

Lists of ToolCallback beans

ToolCallbackProvider beans

Tools are de-duplicated by name, with the first occurrence of each tool name being used.

The ToolContext is supported, allowing contextual information to be passed to tool calls. It contains an McpSyncServerExchange instance under the exchange key, accessible via McpToolUtils.getMcpExchange(toolContext). See this example demonstrating exchange.loggingNotification(…​) and exchange.createMessage(…​).

Provides a standardized way for servers to expose resources to clients.

Static and dynamic resource specifications

Optional change notifications

Support for resource templates

Automatic conversion between sync/async resource specifications

Automatic resource specification through Spring beans:

Provides a standardized way for servers to expose prompt templates to clients.

Change notification support

Automatic conversion between sync/async prompt specifications

Automatic prompt specification through Spring beans:

Provides a standardized way for servers to expose completion capabilities to clients.

Support for both sync and async completion specifications

Automatic registration through Spring beans:

Provides a standardized way for servers to send structured log messages to clients. From within the tool, resource, prompt or completion call handler use the provided McpSyncServerExchange/McpAsyncServerExchange exchange object to send logging messages:

On the MCP client you can register logging consumers to handle these messages:

Provides a standardized way for servers to send progress updates to clients. From within the tool, resource, prompt or completion call handler use the provided McpSyncServerExchange/McpAsyncServerExchange exchange object to send progress notifications:

The Mcp Client can receive progress notifications and update its UI accordingly. For this it needs to register a progress consumer.

When roots change, clients that support listChanged send a root change notification.

Support for monitoring root changes

Automatic conversion to async consumers for reactive applications

Optional registration through Spring beans

Ping mechanism for the server to verify that its clients are still alive. From within the tool, resource, prompt or completion call handler use the provided McpSyncServerExchange/McpAsyncServerExchange exchange object to send ping messages:

Server can optionally, periodically issue pings to connected clients to verify connection health.

By default, keep-alive is disabled. To enable keep-alive, set the keep-alive-interval property in your configuration:

The auto-configuration will automatically register the tool callbacks as MCP tools. You can have multiple beans producing ToolCallbacks, and the auto-configuration will merge them.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webmvc</artifactId>
</dependency>
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webflux</artifactId>
</dependency>
```

Example 3 (java):
```java
@Bean
public ToolCallbackProvider myTools(...) {
    List<ToolCallback> tools = ...
    return ToolCallbackProvider.from(tools);
}
```

Example 4 (java):
```java
@Bean
public List<McpServerFeatures.SyncToolSpecification> myTools(...) {
    List<McpServerFeatures.SyncToolSpecification> tools = ...
    return tools;
}
```

---

## MCP Annotations Special Parameters :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/mcp/mcp-annotations-special-params.html

**Contents:**
- MCP Annotations Special Parameters
- Special Parameter Types
  - McpMeta
    - Overview
    - Usage in Tools
    - Usage in Resources
    - Usage in Prompts
  - @McpProgressToken
    - Overview
    - Usage in Tools

The MCP Annotations support several special parameter types that provide additional context and functionality to annotated methods. These parameters are automatically injected by the framework and are excluded from JSON schema generation.

The McpMeta class provides access to metadata from MCP requests, notifications, and results.

Automatically injected when used as a method parameter

Excluded from parameter count limits and JSON schema generation

Provides convenient access to metadata through the get(String key) method

If no metadata is present in the request, an empty McpMeta object is injected

The @McpProgressToken annotation marks a parameter to receive progress tokens from MCP requests.

Parameter type should be String

Automatically receives the progress token value from the request

Excluded from the generated JSON schema

If no progress token is present, null is injected

Used for tracking long-running operations

Request context objects provide unified access to MCP request information and server-side operations.

Provides unified interface for both stateful and stateless operations

Automatically injected when used as a parameter

Excluded from JSON schema generation

Enables advanced features like logging, progress notifications, sampling, and elicitation

Works with both stateful (server exchange) and stateless (transport context) modes

Lightweight context for stateless operations.

Provides minimal context without full server exchange

Used in stateless implementations

Automatically injected when used as a parameter

Excluded from JSON schema generation

Special parameter for tools that need access to the full request with dynamic schema.

Provides access to the complete tool request

Enables dynamic schema handling at runtime

Automatically injected and excluded from schema generation

Useful for flexible tools that adapt to different input schemas

The following parameters are automatically injected by the framework:

McpMeta - Metadata from the request

@McpProgressToken String - Progress token if available

McpSyncServerExchange / McpAsyncServerExchange - Server exchange context

McpTransportContext - Transport context for stateless operations

CallToolRequest - Full tool request for dynamic schema

Special parameters are excluded from JSON schema generation:

They don’t appear in the tool’s input schema

They don’t count towards parameter limits

They’re not visible to MCP clients

McpMeta - Never null, empty object if no metadata

@McpProgressToken - Can be null if no token provided

Server exchanges - Never null when properly configured

CallToolRequest - Never null for tool methods

Use McpSyncRequestContext / McpAsyncRequestContext for unified access to request context, supporting both stateful and stateless operations with convenient helper methods

Use McpTransportContext for simple stateless operations when you only need transport-level context

Omit context parameters entirely for the simplest cases

Always check capability support before using client features:

MCP Annotations Overview

**Examples:**

Example 1 (java):
```java
@McpTool(name = "contextual-tool", description = "Tool with metadata access")
public String processWithContext(
        @McpToolParam(description = "Input data", required = true) String data,
        McpMeta meta) {

    // Access metadata from the request
    String userId = (String) meta.get("userId");
    String sessionId = (String) meta.get("sessionId");
    String userRole = (String) meta.get("userRole");

    // Use metadata to customize behavior
    if ("admin".equals(userRole)) {
        return processAsAdmin(data, userId);
    } else {
        return processAsUser(data, userId);
    }
}
```

Example 2 (java):
```java
@McpResource(uri = "secure-data://{id}", name = "Secure Data")
public ReadResourceResult getSecureData(String id, McpMeta meta) {

    String requestingUser = (String) meta.get("requestingUser");
    String accessLevel = (String) meta.get("accessLevel");

    // Check access permissions using metadata
    if (!"admin".equals(accessLevel)) {
        return new ReadResourceResult(List.of(
            new TextResourceContents("secure-data://" + id,
                "text/plain", "Access denied")
        ));
    }

    String data = loadSecureData(id);
    return new ReadResourceResult(List.of(
        new TextResourceContents("secure-data://" + id,
            "text/plain", data)
    ));
}
```

Example 3 (java):
```java
@McpPrompt(name = "localized-prompt", description = "Localized prompt generation")
public GetPromptResult localizedPrompt(
        @McpArg(name = "topic", required = true) String topic,
        McpMeta meta) {

    String language = (String) meta.get("language");
    String region = (String) meta.get("region");

    // Generate localized content based on metadata
    String message = generateLocalizedMessage(topic, language, region);

    return new GetPromptResult("Localized Prompt",
        List.of(new PromptMessage(Role.ASSISTANT, new TextContent(message)))
    );
}
```

Example 4 (java):
```java
@McpTool(name = "long-operation", description = "Long-running operation with progress")
public String performLongOperation(
        @McpProgressToken String progressToken,
        @McpToolParam(description = "Operation name", required = true) String operation,
        @McpToolParam(description = "Duration in seconds", required = true) int duration,
        McpSyncServerExchange exchange) {

    if (progressToken != null) {
        // Send initial progress
        exchange.progressNotification(new ProgressNotification(
            progressToken, 0.0, 1.0, "Starting " + operation));

        // Simulate work with progress updates
        for (int i = 1; i <= duration; i++) {
            Thread.sleep(1000);
            double progress = (double) i / duration;

            exchange.progressNotification(new ProgressNotification(
                progressToken, progress, 1.0,
                String.format("Processing... %d%%", (int)(progress * 100))));
        }
    }

    return "Operation " + operation + " completed";
}
```

---

## MCP Annotations Examples :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/mcp/mcp-annotations-examples.html

**Contents:**
- MCP Annotations Examples
- Complete Application Examples
  - Simple Calculator Server
  - Document Processing Server
  - MCP Client with Handlers
- Async Examples
  - Async Tool Server
  - Async Client Handlers
- Stateless Server Examples
- MCP Sampling with Multiple LLM Providers

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This page provides comprehensive examples of using MCP annotations in Spring AI applications.

A complete example of an MCP server providing calculator tools:

An example of a document processing server with resources and prompts:

A complete MCP client application with various handlers:

This example demonstrates how to use MCP Sampling to generate creative content from multiple LLM providers, showcasing the annotation-based approach for both server and client implementations.

The server provides a weather tool that uses MCP Sampling to generate poems from different LLM providers:

The client handles sampling requests by routing them to appropriate LLM providers based on model hints:

Register the MCP tools and handlers in the client application:

Multi-Model Sampling: Server requests content from multiple LLM providers using model hints

Annotation-Based Handlers: Client uses @McpSampling, @McpLogging, and @McpProgress annotations

Stateless HTTP Transport: Uses the streamable protocol for communication

Creative Content Generation: Generates poems about weather data from different models

Unified Response Handling: Combines responses from multiple providers into a single result

When running the client, you’ll see output like:

Example showing MCP tools integrated with Spring AI’s function calling:

MCP Annotations Overview

Server Annotations Reference

Client Annotations Reference

Special Parameters Reference

Spring AI MCP Examples on GitHub

**Examples:**

Example 1 (java):
```java
@SpringBootApplication
public class CalculatorServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(CalculatorServerApplication.class, args);
    }
}

@Component
public class CalculatorTools {

    @McpTool(name = "add", description = "Add two numbers")
    public double add(
            @McpToolParam(description = "First number", required = true) double a,
            @McpToolParam(description = "Second number", required = true) double b) {
        return a + b;
    }

    @McpTool(name = "subtract", description = "Subtract two numbers")
    public double subtract(
            @McpToolParam(description = "First number", required = true) double a,
            @McpToolParam(description = "Second number", required = true) double b) {
        return a - b;
    }

    @McpTool(name = "multiply", description = "Multiply two numbers")
    public double multiply(
            @McpToolParam(description = "First number", required = true) double a,
            @McpToolParam(description = "Second number", required = true) double b) {
        return a * b;
    }

    @McpTool(name = "divide", description = "Divide two numbers")
    public double divide(
            @McpToolParam(description = "Dividend", required = true) double dividend,
            @McpToolParam(description = "Divisor", required = true) double divisor) {
        if (divisor == 0) {
            throw new IllegalArgumentException("Division by zero");
        }
        return dividend / divisor;
    }

    @McpTool(name = "calculate-expression",
             description = "Calculate a complex mathematical expression")
    public CallToolResult calculateExpression(
            CallToolRequest request,
            McpSyncRequestContext context) {

        Map<String, Object> args = request.arguments();
        String expression = (String) args.get("expression");

        // Use convenient logging method
        context.info("Calculating: " + expression);

        try {
            double result = evaluateExpression(expression);
            return CallToolResult.builder()
                .addTextContent("Result: " + result)
                .build();
        } catch (Exception e) {
            return CallToolResult.builder()
                .isError(true)
                .addTextContent("Error: " + e.getMessage())
                .build();
        }
    }
}
```

Example 2 (yaml):
```yaml
spring:
  ai:
    mcp:
      server:
        name: calculator-server
        version: 1.0.0
        type: SYNC
        protocol: SSE  # or STDIO, STREAMABLE
        capabilities:
          tool: true
          resource: true
          prompt: true
          completion: true
```

Example 3 (java):
```java
@Component
public class DocumentServer {

    private final Map<String, Document> documents = new ConcurrentHashMap<>();

    @McpResource(
        uri = "document://{id}",
        name = "Document",
        description = "Access stored documents")
    public ReadResourceResult getDocument(String id, McpMeta meta) {
        Document doc = documents.get(id);

        if (doc == null) {
            return new ReadResourceResult(List.of(
                new TextResourceContents("document://" + id,
                    "text/plain", "Document not found")
            ));
        }

        // Check access permissions from metadata
        String accessLevel = (String) meta.get("accessLevel");
        if ("restricted".equals(doc.getClassification()) &&
            !"admin".equals(accessLevel)) {
            return new ReadResourceResult(List.of(
                new TextResourceContents("document://" + id,
                    "text/plain", "Access denied")
            ));
        }

        return new ReadResourceResult(List.of(
            new TextResourceContents("document://" + id,
                doc.getMimeType(), doc.getContent())
        ));
    }

    @McpTool(name = "analyze-document",
             description = "Analyze document content")
    public String analyzeDocument(
            McpSyncRequestContext context,
            @McpToolParam(description = "Document ID", required = true) String docId,
            @McpToolParam(description = "Analysis type", required = false) String type) {

        Document doc = documents.get(docId);
        if (doc == null) {
            return "Document not found";
        }

        // Access progress token from context
        String progressToken = context.request().progressToken();

        if (progressToken != null) {
            context.progress(p -> p.progress(0.0).total(1.0).message("Starting analysis"));
        }

        // Perform analysis
        String analysisType = type != null ? type : "summary";
        String result = performAnalysis(doc, analysisType);

        if (progressToken != null) {
            context.progress(p -> p.progress(1.0).total(1.0).message("Analysis complete"));
        }

        return result;
    }

    @McpPrompt(
        name = "document-summary",
        description = "Generate document summary prompt")
    public GetPromptResult documentSummaryPrompt(
            @McpArg(name = "docId", required = true) String docId,
            @McpArg(name = "length", required = false) String length) {

        Document doc = documents.get(docId);
        if (doc == null) {
            return new GetPromptResult("Error",
                List.of(new PromptMessage(Role.SYSTEM,
                    new TextContent("Document not found"))));
        }

        String promptText = String.format(
            "Please summarize the following document in %s:\n\n%s",
            length != null ? length : "a few paragraphs",
            doc.getContent()
        );

        return new GetPromptResult("Document Summary",
            List.of(new PromptMessage(Role.USER, new TextContent(promptText))));
    }

    @McpComplete(prompt = "document-summary")
    public List<String> completeDocumentId(String prefix) {
        return documents.keySet().stream()
            .filter(id -> id.startsWith(prefix))
            .sorted()
            .limit(10)
            .toList();
    }
}
```

Example 4 (java):
```java
@SpringBootApplication
public class McpClientApplication {
    public static void main(String[] args) {
        SpringApplication.run(McpClientApplication.class, args);
    }
}

@Component
public class ClientHandlers {

    private final Logger logger = LoggerFactory.getLogger(ClientHandlers.class);
    private final ProgressTracker progressTracker = new ProgressTracker();
    private final ChatModel chatModel;

    public ClientHandlers(@Lazy ChatModel chatModel) {
        this.chatModel = chatModel;
    }

    @McpLogging(clients = "server1")
    public void handleLogging(LoggingMessageNotification notification) {
        switch (notification.level()) {
            case ERROR:
                logger.error("[MCP] {} - {}", notification.logger(), notification.data());
                break;
            case WARNING:
                logger.warn("[MCP] {} - {}", notification.logger(), notification.data());
                break;
            case INFO:
                logger.info("[MCP] {} - {}", notification.logger(), notification.data());
                break;
            default:
                logger.debug("[MCP] {} - {}", notification.logger(), notification.data());
        }
    }

    @McpSampling(clients = "server1")
    public CreateMessageResult handleSampling(CreateMessageRequest request) {
        // Use Spring AI ChatModel for sampling
        List<Message> messages = request.messages().stream()
            .map(msg -> {
                if (msg.role() == Role.USER) {
                    return new UserMessage(((TextContent) msg.content()).text());
                } else {
                    return AssistantMessage.builder()
                        .content(((TextContent) msg.content()).text())
                        .build();
                }
            })
            .toList();

        ChatResponse response = chatModel.call(new Prompt(messages));

        return CreateMessageResult.builder()
            .role(Role.ASSISTANT)
            .content(new TextContent(response.getResult().getOutput().getContent()))
            .model(request.modelPreferences().hints().get(0).name())
            .build();
    }

    @McpElicitation(clients = "server1")
    public ElicitResult handleElicitation(ElicitRequest request) {
        // In a real application, this would show a UI dialog
        Map<String, Object> userData = new HashMap<>();

        logger.info("Elicitation requested: {}", request.message());

        // Simulate user input based on schema
        Map<String, Object> schema = request.requestedSchema();
        if (schema != null && schema.containsKey("properties")) {
            Map<String, Object> properties = (Map<String, Object>) schema.get("properties");

            properties.forEach((key, value) -> {
                // In real app, prompt user for each field
                userData.put(key, getDefaultValueForProperty(key, value));
            });
        }

        return new ElicitResult(ElicitResult.Action.ACCEPT, userData);
    }

    @McpProgress(clients = "server1")
    public void handleProgress(ProgressNotification notification) {
        progressTracker.update(
            notification.progressToken(),
            notification.progress(),
            notification.total(),
            notification.message()
        );

        // Update UI or send websocket notification
        broadcastProgress(notification);
    }

    @McpToolListChanged(clients = "server1")
    public void handleServer1ToolsChanged(List<McpSchema.Tool> tools) {
        logger.info("Server1 tools updated: {} tools available", tools.size());

        // Update tool registry
        toolRegistry.updateServerTools("server1", tools);

        // Notify UI to refresh tool list
        eventBus.publish(new ToolsUpdatedEvent("server1", tools));
    }

    @McpResourceListChanged(clients = "server1")
    public void handleServer1ResourcesChanged(List<McpSchema.Resource> resources) {
        logger.info("Server1 resources updated: {} resources available", resources.size());

        // Clear resource cache for this server
        resourceCache.clearServer("server1");

        // Register new resources
        resources.forEach(resource ->
            resourceCache.register("server1", resource));
    }
}
```

---

## MCP Utilities :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/mcp/mcp-helpers.html

**Contents:**
- MCP Utilities
- ToolCallback Utility
  - Tool Callback Adapter
  - Tool Callback Providers
- McpToolUtils
  - ToolCallbacks to ToolSpecifications
  - MCP Clients to ToolCallbacks
- Native Image Support

The MCP utilities provide foundational support for integrating Model Context Protocol with Spring AI applications. These utilities enable seamless communication between Spring AI’s tool system and MCP servers, supporting both synchronous and asynchronous operations. They are typically used for programmatic MCP Client and Server configuration and interaction. For a more streamlined configuration, consider using the boot starters.

Adapts MCP tools to Spring AI’s tool interface with both synchronous and asynchronous execution support.

Discovers and provides MCP tools from MCP clients.

For multiple clients:

For dynamic selection of a subset of clients

For multiple clients:

Converting Spring AI tool callbacks to MCP tool specifications:

then you can use the McpServer.SyncSpecification to register the tool specifications:

then you can use the McpServer.AsyncSpecification to register the tool specifications:

Getting tool callbacks from MCP clients

The McpHints class provides GraalVM native image hints for MCP schema classes. This class automatically registers all necessary reflection hints for MCP schema classes when building native images.

**Examples:**

Example 1 (java):
```java
McpSyncClient mcpClient = // obtain MCP client
Tool mcpTool = // obtain MCP tool definition
ToolCallback callback = new SyncMcpToolCallback(mcpClient, mcpTool);

// Use the tool through Spring AI's interfaces
ToolDefinition definition = callback.getToolDefinition();
String result = callback.call("{\"param\": \"value\"}");
```

Example 2 (java):
```java
McpAsyncClient mcpClient = // obtain MCP client
Tool mcpTool = // obtain MCP tool definition
ToolCallback callback = new AsyncMcpToolCallback(mcpClient, mcpTool);

// Use the tool through Spring AI's interfaces
ToolDefinition definition = callback.getToolDefinition();
String result = callback.call("{\"param\": \"value\"}");
```

Example 3 (java):
```java
McpSyncClient mcpClient = // obtain MCP client
ToolCallbackProvider provider = new SyncMcpToolCallbackProvider(mcpClient);

// Get all available tools
ToolCallback[] tools = provider.getToolCallbacks();
```

Example 4 (java):
```java
List<McpSyncClient> clients = // obtain list of clients
List<ToolCallback> callbacks = SyncMcpToolCallbackProvider.syncToolCallbacks(clients);
```

---

## MCP Server Boot Starter :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/mcp/mcp-server-boot-starter-docs.html

**Contents:**
- MCP Server Boot Starter
- MCP Server Boot Starters
  - STDIO
  - WebMVC
  - WebMVC (Reactive)
- Server Capabilities
- Server Protocols
- Sync/Async Server API Options
- MCP Server Annotations
  - Key Annotations

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Model Context Protocol (MCP) Servers are programs that expose specific capabilities to AI applications through standardized protocol interfaces. Each server provides focused functionality for a particular domain.

The Spring AI MCP Server Boot Starters provide auto-configuration for setting up MCP Servers in Spring Boot applications. They enable seamless integration of MCP server capabilities with Spring Boot’s auto-configuration system.

The MCP Server Boot Starters offer:

Automatic configuration of MCP server components, including tools, resources, and prompts

Support for different MCP protocol versions, including STDIO, SSE, Streamable-HTTP, and stateless servers

Support for both synchronous and asynchronous operation modes

Multiple transport layer options

Flexible tool, resource, and prompt specification

Change notification capabilities

Annotation-based server development with automatic bean scanning and registration

MCP Servers support multiple protocol and transport mechanisms. Use the dedicated starter and the correct spring.ai.mcp.server.protocol property to configure your server:

Standard Input/Output (STDIO)

spring-ai-starter-mcp-server

spring.ai.mcp.server.stdio=true

spring-ai-starter-mcp-server-webmvc

spring.ai.mcp.server.protocol=SSE or empty

Streamable-HTTP WebMVC

spring-ai-starter-mcp-server-webmvc

spring.ai.mcp.server.protocol=STREAMABLE

spring-ai-starter-mcp-server-webmvc

spring.ai.mcp.server.protocol=STATELESS

spring-ai-starter-mcp-server-webflux

spring.ai.mcp.server.protocol=SSE or empty

Streamable-HTTP WebFlux

spring-ai-starter-mcp-server-webflux

spring.ai.mcp.server.protocol=STREAMABLE

spring-ai-starter-mcp-server-webflux

spring.ai.mcp.server.protocol=STATELESS

Depending on the server and transport types, MCP Servers can support various capabilities, such as:

Tools - Allows servers to expose tools that can be invoked by language models

Resources - Provides a standardized way for servers to expose resources to clients

Prompts - Provides a standardized way for servers to expose prompt templates to clients

Utility/Completions - Provides a standardized way for servers to offer argument autocompletion suggestions for prompts and resource URIs

Utility/Logging - Provides a standardized way for servers to send structured log messages to clients

Utility/Progress - Optional progress tracking for long-running operations through notification messages

Utility/Ping - Optional health check mechanism for the server to report its status

All capabilities are enabled by default. Disabling a capability will prevent the server from registering and exposing the corresponding features to clients.

MCP provides several protocol types including:

STDIO - In process (e.g. server runs inside the host application) protocol. Communication is over standard in and standard out. To enable the STDIO set spring.ai.mcp.server.stdio=true.

SSE - Server-sent events protocol for real-time updates. The server operates as an independent process that can handle multiple client connections.

Streamable-HTTP - The Streamable HTTP transport allows MCP servers to operate as independent processes that can handle multiple client connections using HTTP POST and GET requests, with optional Server-Sent Events (SSE) streaming for multiple server messages. It replaces the SSE transport. To enable the STREAMABLE protocol, set spring.ai.mcp.server.protocol=STREAMABLE.

Stateless - Stateless MCP servers are designed for simplified deployments where session state is not maintained between requests. They are ideal for microservices architectures and cloud-native deployments. To enable the STATELESS protocol, set spring.ai.mcp.server.protocol=STATELESS.

The MCP Server API supports imperative (i.e. synchronous) and reactive (e.g. asynchronous) programming models.

Synchronous Server - The default server type implemented using McpSyncServer. It is designed for straightforward request-response patterns in your applications. To enable this server type, set spring.ai.mcp.server.type=SYNC in your configuration. When activated, it automatically handles the configuration of synchronous tool specifications.

NOTE: The SYNC server will register only synchronous MCP annotated methods. Asynchronous methods will be ignored.

Asynchronous Server - The asynchronous server implementation uses McpAsyncServer and is optimized for non-blocking operations. To enable this server type, configure your application with spring.ai.mcp.server.type=ASYNC. This server type automatically sets up asynchronous tool specifications with built-in Project Reactor support.

NOTE: The ASYNC server will register only asynchronous MCP annotated methods. Synchronous methods will be ignored.

The MCP Server Boot Starters provide comprehensive support for annotation-based server development, allowing you to create MCP servers using declarative Java annotations instead of manual configuration.

@McpTool - Mark methods as MCP tools with automatic JSON schema generation

@McpResource - Provide access to resources via URI templates

@McpPrompt - Generate prompt messages for AI interactions

@McpComplete - Provide auto-completion functionality for prompts

The annotation system supports special parameter types that provide additional context:

McpMeta - Access metadata from MCP requests

@McpProgressToken - Receive progress tokens for long-running operations

McpSyncServerExchange/McpAsyncServerExchange - Full server context for advanced operations

McpTransportContext - Lightweight context for stateless operations

CallToolRequest - Dynamic schema support for flexible tools

With Spring Boot auto-configuration, annotated beans are automatically detected and registered:

The auto-configuration will:

Scan for beans with MCP annotations

Create appropriate specifications

Register them with the MCP server

Handle both sync and async implementations based on configuration

Configure the server annotation scanner:

Server Annotations Reference - Complete guide to server annotations

Special Parameters - Advanced parameter injection

Examples - Comprehensive examples and use cases

Weather Server (SSE WebFlux) - Spring AI MCP Server Boot Starter with WebFlux transport

Weather Server (STDIO) - Spring AI MCP Server Boot Starter with STDIO transport

Weather Server Manual Configuration - Spring AI MCP Server Boot Starter that doesn’t use auto-configuration but uses the Java SDK to configure the server manually

Streamable-HTTP WebFlux/WebMVC Example - TODO

Stateless WebFlux/WebMVC Example - TODO

MCP Server Annotations - Declarative server development with annotations

Special Parameters - Advanced parameter injection and context access

MCP Annotations Examples - Comprehensive examples and use cases

Spring AI Documentation

Model Context Protocol Specification

Spring Boot Auto-configuration

**Examples:**

Example 1 (java):
```java
@Component
public class CalculatorTools {

    @McpTool(name = "add", description = "Add two numbers together")
    public int add(
            @McpToolParam(description = "First number", required = true) int a,
            @McpToolParam(description = "Second number", required = true) int b) {
        return a + b;
    }

    @McpResource(uri = "config://{key}", name = "Configuration")
    public String getConfig(String key) {
        return configData.get(key);
    }
}
```

Example 2 (java):
```java
@SpringBootApplication
public class McpServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(McpServerApplication.class, args);
    }
}
```

Example 3 (yaml):
```yaml
spring:
  ai:
    mcp:
      server:
        type: SYNC  # or ASYNC
        annotation-scanner:
          enabled: true
```

---

## Untitled :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/mcp/mcp-stdio-sse-server-boot-starter-docs.html

**Contents:**
- STDIO and SSE MCP Servers
  - STDIO MCP Server
  - SSE WebMVC Server
  - SSE WebFlux Server
- Configuration Properties
  - Common Properties
  - MCP Annotations Properties
  - SSE Properties
- Features and Capabilities
  - Tools

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The STDIO and SSE MCP Servers support multiple transport mechanisms, each with its dedicated starter.

Full MCP Server feature support with STDIO server transport.

Suitable for command-line and desktop tools

No additional web dependencies required

Configuration of basic server components

Handling of tool, resource, and prompt specifications

Management of server capabilities and change notifications

Support for both sync and async server implementations

Full MCP Server feature support with SSE (Server-Sent Events) server transport based on Spring MVC and an optional STDIO transport.

HTTP-based transport using Spring MVC (WebMvcSseServerTransportProvider)

Automatically configured SSE endpoints

Optional STDIO transport (enabled by setting spring.ai.mcp.server.stdio=true)

Includes spring-boot-starter-web and mcp-spring-webmvc dependencies

Full MCP Server feature support with SSE (Server-Sent Events) server transport based on Spring WebFlux and an optional STDIO transport.

The starter activates the McpWebFluxServerAutoConfiguration and McpServerAutoConfiguration auto-configurations to provide:

Reactive transport using Spring WebFlux (WebFluxSseServerTransportProvider)

Automatically configured reactive SSE endpoints

Optional STDIO transport (enabled by setting spring.ai.mcp.server.stdio=true)

Includes spring-boot-starter-webflux and mcp-spring-webflux dependencies

Due to Spring Boot’s default behavior, when both org.springframework.web.servlet.DispatcherServlet and org.springframework.web.reactive.DispatcherHandler are present on the classpath, Spring Boot will prioritize DispatcherServlet. As a result, if your project uses spring-boot-starter-web, it is recommended to use spring-ai-starter-mcp-server-webmvc instead of spring-ai-starter-mcp-server-webflux.

All Common properties are prefixed with spring.ai.mcp.server:

Enable/disable the MCP server

tool-callback-converter

Enable/disable the conversion of Spring AI ToolCallbacks into MCP Tool specs

Enable/disable STDIO transport

Server name for identification

Optional instructions to provide guidance to the client on how to interact with this server

Server type (SYNC/ASYNC)

capabilities.resource

Enable/disable resource capabilities

Enable/disable tool capabilities

Enable/disable prompt capabilities

capabilities.completion

Enable/disable completion capabilities

resource-change-notification

Enable resource change notifications

prompt-change-notification

Enable prompt change notifications

tool-change-notification

Enable tool change notifications

tool-response-mime-type

Optional response MIME type per tool name. For example, spring.ai.mcp.server.tool-response-mime-type.generateImage=image/png will associate the image/png MIME type with the generateImage() tool name

Duration to wait for server responses before timing out requests. Applies to all requests made through the client, including tool calls, resource access, and prompt operations

MCP Server Annotations provide a declarative way to implement MCP server handlers using Java annotations.

The server mcp-annotations properties are prefixed with spring.ai.mcp.server.annotation-scanner:

Enable/disable the MCP server annotations auto-scanning

All SSE properties are prefixed with spring.ai.mcp.server:

Custom SSE message endpoint path for web transport to be used by the client to send messages

Custom SSE endpoint path for web transport

Optional URL prefix. For example, base-url=/api/v1 means that the client should access the SSE endpoint at /api/v1 + sse-endpoint and the message endpoint is /api/v1 + sse-message-endpoint

Connection keep-alive interval

The MCP Server Boot Starter allows servers to expose tools, resources, and prompts to clients. It automatically converts custom capability handlers registered as Spring beans to sync/async specifications based on the server type:

Allows servers to expose tools that can be invoked by language models. The MCP Server Boot Starter provides:

Change notification support

Spring AI Tools are automatically converted to sync/async specifications based on the server type

Automatic tool specification through Spring beans:

or using the low-level API:

The auto-configuration will automatically detect and register all tool callbacks from:

Individual ToolCallback beans

Lists of ToolCallback beans

ToolCallbackProvider beans

Tools are de-duplicated by name, with the first occurrence of each tool name being used.

The ToolContext is supported, allowing contextual information to be passed to tool calls. It contains an McpSyncServerExchange instance under the exchange key, accessible via McpToolUtils.getMcpExchange(toolContext). See this example demonstrating exchange.loggingNotification(…​) and exchange.createMessage(…​).

Provides a standardized way for servers to expose resources to clients.

Static and dynamic resource specifications

Optional change notifications

Support for resource templates

Automatic conversion between sync/async resource specifications

Automatic resource specification through Spring beans:

Provides a standardized way for servers to expose prompt templates to clients.

Change notification support

Automatic conversion between sync/async prompt specifications

Automatic prompt specification through Spring beans:

Provides a standardized way for servers to expose completion capabilities to clients.

Support for both sync and async completion specifications

Automatic registration through Spring beans:

Provides a standardized way for servers to send structured log messages to clients. From within the tool, resource, prompt or completion call handler use the provided McpSyncServerExchange/McpAsyncServerExchange exchange object to send logging messages:

On the MCP client you can register logging consumers to handle these messages:

Provides a standardized way for servers to send progress updates to clients. From within the tool, resource, prompt or completion call handler use the provided McpSyncServerExchange/McpAsyncServerExchange exchange object to send progress notifications:

The Mcp Client can receive progress notifications and update its UI accordingly. For this it needs to register a progress consumer.

When roots change, clients that support listChanged send a root change notification.

Support for monitoring root changes

Automatic conversion to async consumers for reactive applications

Optional registration through Spring beans

Ping mechanism for the server to verify that its clients are still alive. From within the tool, resource, prompt or completion call handler use the provided McpSyncServerExchange/McpAsyncServerExchange exchange object to send ping messages:

Server can optionally, periodically issue pings to connected clients to verify connection health.

By default, keep-alive is disabled. To enable keep-alive, set the keep-alive-interval property in your configuration:

The auto-configuration will automatically register the tool callbacks as MCP tools. You can have multiple beans producing ToolCallbacks, and the auto-configuration will merge them.

Weather Server (WebFlux) - Spring AI MCP Server Boot Starter with WebFlux transport

Weather Server (STDIO) - Spring AI MCP Server Boot Starter with STDIO transport

Weather Server Manual Configuration - Spring AI MCP Server Boot Starter that doesn’t use auto-configuration but uses the Java SDK to configure the server manually

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server</artifactId>
</dependency>
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webmvc</artifactId>
</dependency>
```

Example 3 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webflux</artifactId>
</dependency>
```

Example 4 (java):
```java
@Bean
public ToolCallbackProvider myTools(...) {
    List<ToolCallback> tools = ...
    return ToolCallbackProvider.from(tools);
}
```

---

## Untitled :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/mcp/mcp-stateless-server-boot-starter-docs.html

**Contents:**
- Stateless Streamable-HTTP MCP Servers
  - Stateless WebMVC Server
  - Stateless WebFlux Server
- Configuration Properties
  - Common Properties
  - MCP Annotations Properties
  - Stateless Connection Properties
- Features and Capabilities
  - Tools
  - Resources

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Stateless Streamable-HTTP MCP servers are designed for simplified deployments where session state is not maintained between requests. These servers are ideal for microservices architectures and cloud-native deployments.

Use the spring-ai-starter-mcp-server-webmvc dependency:

and set the spring.ai.mcp.server.protocol property to STATELESS.

Stateless operation with Spring MVC transport

No session state management

Simplified deployment model

Optimized for cloud-native environments

Use the spring-ai-starter-mcp-server-webflux dependency:

and set the spring.ai.mcp.server.protocol property to STATELESS.

Reactive stateless operation with WebFlux transport

No session state management

Non-blocking request processing

Optimized for high-throughput scenarios

All Common properties are prefixed with spring.ai.mcp.server:

Enable/disable the stateless MCP server

Must be set to STATELESS to enable the stateless server

tool-callback-converter

Enable/disable the conversion of Spring AI ToolCallbacks into MCP Tool specs

Server name for identification

Optional instructions for client interaction

Server type (SYNC/ASYNC)

capabilities.resource

Enable/disable resource capabilities

Enable/disable tool capabilities

Enable/disable prompt capabilities

capabilities.completion

Enable/disable completion capabilities

tool-response-mime-type

Response MIME type per tool name

Request timeout duration

MCP Server Annotations provide a declarative way to implement MCP server handlers using Java annotations.

The server mcp-annotations properties are prefixed with spring.ai.mcp.server.annotation-scanner:

Enable/disable the MCP server annotations auto-scanning

All connection properties are prefixed with spring.ai.mcp.server.stateless:

Custom MCP endpoint path

Disallow delete operations

The MCP Server Boot Starter allows servers to expose tools, resources, and prompts to clients. It automatically converts custom capability handlers registered as Spring beans to sync/async specifications based on the server type:

Allows servers to expose tools that can be invoked by language models. The MCP Server Boot Starter provides:

Change notification support

Spring AI Tools are automatically converted to sync/async specifications based on the server type

Automatic tool specification through Spring beans:

or using the low-level API:

The auto-configuration will automatically detect and register all tool callbacks from:

Individual ToolCallback beans

Lists of ToolCallback beans

ToolCallbackProvider beans

Tools are de-duplicated by name, with the first occurrence of each tool name being used.

Provides a standardized way for servers to expose resources to clients.

Static and dynamic resource specifications

Optional change notifications

Support for resource templates

Automatic conversion between sync/async resource specifications

Automatic resource specification through Spring beans:

Provides a standardized way for servers to expose prompt templates to clients.

Change notification support

Automatic conversion between sync/async prompt specifications

Automatic prompt specification through Spring beans:

Provides a standardized way for servers to expose completion capabilities to clients.

Support for both sync and async completion specifications

Automatic registration through Spring beans:

The auto-configuration will automatically register the tool callbacks as MCP tools. You can have multiple beans producing ToolCallbacks, and the auto-configuration will merge them.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webmvc</artifactId>
</dependency>
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webflux</artifactId>
</dependency>
```

Example 3 (java):
```java
@Bean
public ToolCallbackProvider myTools(...) {
    List<ToolCallback> tools = ...
    return ToolCallbackProvider.from(tools);
}
```

Example 4 (java):
```java
@Bean
public List<McpStatelessServerFeatures.SyncToolSpecification> myTools(...) {
    List<McpStatelessServerFeatures.SyncToolSpecification> tools = ...
    return tools;
}
```

---

## MCP Server Annotations :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/mcp/mcp-annotations-server.html

**Contents:**
- MCP Server Annotations
- Server Annotations
  - @McpTool
    - Basic Usage
    - Advanced Features
    - With Request Context
    - Dynamic Schema Support
    - Progress Tracking
  - @McpResource
    - Basic Usage

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The MCP Server Annotations provide a declarative way to implement MCP server functionality using Java annotations. These annotations simplify the creation of tools, resources, prompts, and completion handlers.

The @McpTool annotation marks a method as an MCP tool implementation with automatic JSON schema generation.

Tools can access the request context for advanced operations:

Tools can accept CallToolRequest for runtime schema handling:

Tools can receive progress tokens for tracking long-running operations:

The @McpResource annotation provides access to resources via URI templates.

The @McpPrompt annotation generates prompt messages for AI interactions.

The @McpComplete annotation provides auto-completion functionality for prompts.

Use McpSyncRequestContext or McpAsyncRequestContext for a unified interface that works with both stateful and stateless operations:

For simple operations, you can omit context parameters entirely:

For stateless operations where you need minimal transport context:

Therefore methods using McpSyncRequestContext or McpAsyncRequestContext in stateless mode are ignored.

The MCP annotations framework automatically filters annotated methods based on the server type and method characteristics. This ensures that only appropriate methods are registered for each server configuration. A warning is logged for each filtered method to help with debugging.

Synchronous servers (configured with spring.ai.mcp.server.type=SYNC) use synchronous providers that:

Accept methods with non-reactive return types:

Primitive types (int, double, boolean)

Object types (String, Integer, custom POJOs)

MCP types (CallToolResult, ReadResourceResult, GetPromptResult, CompleteResult)

Collections (List<String>, Map<String, Object>)

Filter out methods with reactive return types:

Asynchronous servers (configured with spring.ai.mcp.server.type=ASYNC) use asynchronous providers that:

Accept methods with reactive return types:

Mono<T> (for single results)

Flux<T> (for streaming results)

Publisher<T> (generic reactive type)

Filter out methods with non-reactive return types:

Stateful servers support bidirectional communication and accept methods with:

Bidirectional context parameters:

McpSyncRequestContext (for sync operations)

McpAsyncRequestContext (for async operations)

McpSyncServerExchange (legacy, for sync operations)

McpAsyncServerExchange (legacy, for async operations)

Support for bidirectional operations:

roots() - Access root directories

elicit() - Request user input

sample() - Request LLM sampling

Stateless servers are optimized for simple request-response patterns and:

Filter out methods with bidirectional context parameters:

Methods with McpSyncRequestContext are skipped

Methods with McpAsyncRequestContext are skipped

Methods with McpSyncServerExchange are skipped

Methods with McpAsyncServerExchange are skipped

A warning is logged for each filtered method

McpTransportContext (lightweight stateless context)

No context parameter at all

Only regular @McpToolParam parameters

Do not support bidirectional operations:

roots() - Not available

elicit() - Not available

sample() - Not available

Non-reactive returns + bidirectional context

Reactive returns (Mono/Flux)

Reactive returns (Mono/Flux) + bidirectional context

Non-reactive returns + no bidirectional context

Reactive returns OR bidirectional context parameters

Reactive returns (Mono/Flux) + no bidirectional context

Non-reactive returns OR bidirectional context parameters

Keep methods aligned with your server type - use sync methods for sync servers, async for async servers

Separate stateful and stateless implementations into different classes for clarity

Check logs during startup for filtered method warnings

Use the right context - McpSyncRequestContext/McpAsyncRequestContext for stateful, McpTransportContext for stateless

Test both modes if you support both stateful and stateless deployments

All server annotations support asynchronous implementations using Reactor:

With Spring Boot auto-configuration, annotated beans are automatically detected and registered:

The auto-configuration will:

Scan for beans with MCP annotations

Create appropriate specifications

Register them with the MCP server

Handle both sync and async implementations based on configuration

Configure the server annotation scanner:

MCP Annotations Overview

MCP Server Boot Starter

**Examples:**

Example 1 (java):
```java
@Component
public class CalculatorTools {

    @McpTool(name = "add", description = "Add two numbers together")
    public int add(
            @McpToolParam(description = "First number", required = true) int a,
            @McpToolParam(description = "Second number", required = true) int b) {
        return a + b;
    }
}
```

Example 2 (java):
```java
@McpTool(name = "calculate-area",
         description = "Calculate the area of a rectangle",
         annotations = McpTool.McpAnnotations(
             title = "Rectangle Area Calculator",
             readOnlyHint = true,
             destructiveHint = false,
             idempotentHint = true
         ))
public AreaResult calculateRectangleArea(
        @McpToolParam(description = "Width", required = true) double width,
        @McpToolParam(description = "Height", required = true) double height) {

    return new AreaResult(width * height, "square units");
}
```

Example 3 (java):
```java
@McpTool(name = "process-data", description = "Process data with request context")
public String processData(
        McpSyncRequestContext context,
        @McpToolParam(description = "Data to process", required = true) String data) {

    // Send logging notification
    context.info("Processing data: " + data);

    // Send progress notification (using convenient method)
    context.progress(p -> p.progress(0.5).total(1.0).message("Processing..."));

    // Ping the client
    context.ping();

    return "Processed: " + data.toUpperCase();
}
```

Example 4 (java):
```java
@McpTool(name = "flexible-tool", description = "Process dynamic schema")
public CallToolResult processDynamic(CallToolRequest request) {
    Map<String, Object> args = request.arguments();

    // Process based on runtime schema
    String result = "Processed " + args.size() + " arguments dynamically";

    return CallToolResult.builder()
        .addTextContent(result)
        .build();
}
```

---

## Untitled :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/mcp/mcp-stateless-server-boot-starter-docs.html

**Contents:**
- Stateless Streamable-HTTP MCP Servers
  - Stateless WebMVC Server
  - Stateless WebFlux Server
- Configuration Properties
  - Common Properties
  - MCP Annotations Properties
  - Stateless Connection Properties
- Features and Capabilities
  - Tools
  - Resources

Stateless Streamable-HTTP MCP servers are designed for simplified deployments where session state is not maintained between requests. These servers are ideal for microservices architectures and cloud-native deployments.

Use the spring-ai-starter-mcp-server-webmvc dependency:

and set the spring.ai.mcp.server.protocol property to STATELESS.

Stateless operation with Spring MVC transport

No session state management

Simplified deployment model

Optimized for cloud-native environments

Use the spring-ai-starter-mcp-server-webflux dependency:

and set the spring.ai.mcp.server.protocol property to STATELESS.

Reactive stateless operation with WebFlux transport

No session state management

Non-blocking request processing

Optimized for high-throughput scenarios

All Common properties are prefixed with spring.ai.mcp.server:

Enable/disable the stateless MCP server

Must be set to STATELESS to enable the stateless server

tool-callback-converter

Enable/disable the conversion of Spring AI ToolCallbacks into MCP Tool specs

Server name for identification

Optional instructions for client interaction

Server type (SYNC/ASYNC)

capabilities.resource

Enable/disable resource capabilities

Enable/disable tool capabilities

Enable/disable prompt capabilities

capabilities.completion

Enable/disable completion capabilities

tool-response-mime-type

Response MIME type per tool name

Request timeout duration

MCP Server Annotations provide a declarative way to implement MCP server handlers using Java annotations.

The server mcp-annotations properties are prefixed with spring.ai.mcp.server.annotation-scanner:

Enable/disable the MCP server annotations auto-scanning

All connection properties are prefixed with spring.ai.mcp.server.stateless:

Custom MCP endpoint path

Disallow delete operations

The MCP Server Boot Starter allows servers to expose tools, resources, and prompts to clients. It automatically converts custom capability handlers registered as Spring beans to sync/async specifications based on the server type:

Allows servers to expose tools that can be invoked by language models. The MCP Server Boot Starter provides:

Change notification support

Spring AI Tools are automatically converted to sync/async specifications based on the server type

Automatic tool specification through Spring beans:

or using the low-level API:

The auto-configuration will automatically detect and register all tool callbacks from:

Individual ToolCallback beans

Lists of ToolCallback beans

ToolCallbackProvider beans

Tools are de-duplicated by name, with the first occurrence of each tool name being used.

Provides a standardized way for servers to expose resources to clients.

Static and dynamic resource specifications

Optional change notifications

Support for resource templates

Automatic conversion between sync/async resource specifications

Automatic resource specification through Spring beans:

Provides a standardized way for servers to expose prompt templates to clients.

Change notification support

Automatic conversion between sync/async prompt specifications

Automatic prompt specification through Spring beans:

Provides a standardized way for servers to expose completion capabilities to clients.

Support for both sync and async completion specifications

Automatic registration through Spring beans:

The auto-configuration will automatically register the tool callbacks as MCP tools. You can have multiple beans producing ToolCallbacks, and the auto-configuration will merge them.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webmvc</artifactId>
</dependency>
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webflux</artifactId>
</dependency>
```

Example 3 (java):
```java
@Bean
public ToolCallbackProvider myTools(...) {
    List<ToolCallback> tools = ...
    return ToolCallbackProvider.from(tools);
}
```

Example 4 (java):
```java
@Bean
public List<McpStatelessServerFeatures.SyncToolSpecification> myTools(...) {
    List<McpStatelessServerFeatures.SyncToolSpecification> tools = ...
    return tools;
}
```

---

## MCP Annotations Examples :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/mcp/mcp-annotations-examples.html

**Contents:**
- MCP Annotations Examples
- Complete Application Examples
  - Simple Calculator Server
  - Document Processing Server
  - MCP Client with Handlers
- Async Examples
  - Async Tool Server
  - Async Client Handlers
- Stateless Server Examples
- MCP Sampling with Multiple LLM Providers

This page provides comprehensive examples of using MCP annotations in Spring AI applications.

A complete example of an MCP server providing calculator tools:

An example of a document processing server with resources and prompts:

A complete MCP client application with various handlers:

This example demonstrates how to use MCP Sampling to generate creative content from multiple LLM providers, showcasing the annotation-based approach for both server and client implementations.

The server provides a weather tool that uses MCP Sampling to generate poems from different LLM providers:

The client handles sampling requests by routing them to appropriate LLM providers based on model hints:

Register the MCP tools and handlers in the client application:

Multi-Model Sampling: Server requests content from multiple LLM providers using model hints

Annotation-Based Handlers: Client uses @McpSampling, @McpLogging, and @McpProgress annotations

Stateless HTTP Transport: Uses the streamable protocol for communication

Creative Content Generation: Generates poems about weather data from different models

Unified Response Handling: Combines responses from multiple providers into a single result

When running the client, you’ll see output like:

Example showing MCP tools integrated with Spring AI’s function calling:

MCP Annotations Overview

Server Annotations Reference

Client Annotations Reference

Special Parameters Reference

Spring AI MCP Examples on GitHub

**Examples:**

Example 1 (java):
```java
@SpringBootApplication
public class CalculatorServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(CalculatorServerApplication.class, args);
    }
}

@Component
public class CalculatorTools {

    @McpTool(name = "add", description = "Add two numbers")
    public double add(
            @McpToolParam(description = "First number", required = true) double a,
            @McpToolParam(description = "Second number", required = true) double b) {
        return a + b;
    }

    @McpTool(name = "subtract", description = "Subtract two numbers")
    public double subtract(
            @McpToolParam(description = "First number", required = true) double a,
            @McpToolParam(description = "Second number", required = true) double b) {
        return a - b;
    }

    @McpTool(name = "multiply", description = "Multiply two numbers")
    public double multiply(
            @McpToolParam(description = "First number", required = true) double a,
            @McpToolParam(description = "Second number", required = true) double b) {
        return a * b;
    }

    @McpTool(name = "divide", description = "Divide two numbers")
    public double divide(
            @McpToolParam(description = "Dividend", required = true) double dividend,
            @McpToolParam(description = "Divisor", required = true) double divisor) {
        if (divisor == 0) {
            throw new IllegalArgumentException("Division by zero");
        }
        return dividend / divisor;
    }

    @McpTool(name = "calculate-expression",
             description = "Calculate a complex mathematical expression")
    public CallToolResult calculateExpression(
            CallToolRequest request,
            McpSyncRequestContext context) {

        Map<String, Object> args = request.arguments();
        String expression = (String) args.get("expression");

        // Use convenient logging method
        context.info("Calculating: " + expression);

        try {
            double result = evaluateExpression(expression);
            return CallToolResult.builder()
                .addTextContent("Result: " + result)
                .build();
        } catch (Exception e) {
            return CallToolResult.builder()
                .isError(true)
                .addTextContent("Error: " + e.getMessage())
                .build();
        }
    }
}
```

Example 2 (yaml):
```yaml
spring:
  ai:
    mcp:
      server:
        name: calculator-server
        version: 1.0.0
        type: SYNC
        protocol: SSE  # or STDIO, STREAMABLE
        capabilities:
          tool: true
          resource: true
          prompt: true
          completion: true
```

Example 3 (java):
```java
@Component
public class DocumentServer {

    private final Map<String, Document> documents = new ConcurrentHashMap<>();

    @McpResource(
        uri = "document://{id}",
        name = "Document",
        description = "Access stored documents")
    public ReadResourceResult getDocument(String id, McpMeta meta) {
        Document doc = documents.get(id);

        if (doc == null) {
            return new ReadResourceResult(List.of(
                new TextResourceContents("document://" + id,
                    "text/plain", "Document not found")
            ));
        }

        // Check access permissions from metadata
        String accessLevel = (String) meta.get("accessLevel");
        if ("restricted".equals(doc.getClassification()) &&
            !"admin".equals(accessLevel)) {
            return new ReadResourceResult(List.of(
                new TextResourceContents("document://" + id,
                    "text/plain", "Access denied")
            ));
        }

        return new ReadResourceResult(List.of(
            new TextResourceContents("document://" + id,
                doc.getMimeType(), doc.getContent())
        ));
    }

    @McpTool(name = "analyze-document",
             description = "Analyze document content")
    public String analyzeDocument(
            McpSyncRequestContext context,
            @McpToolParam(description = "Document ID", required = true) String docId,
            @McpToolParam(description = "Analysis type", required = false) String type) {

        Document doc = documents.get(docId);
        if (doc == null) {
            return "Document not found";
        }

        // Access progress token from context
        String progressToken = context.request().progressToken();

        if (progressToken != null) {
            context.progress(p -> p.progress(0.0).total(1.0).message("Starting analysis"));
        }

        // Perform analysis
        String analysisType = type != null ? type : "summary";
        String result = performAnalysis(doc, analysisType);

        if (progressToken != null) {
            context.progress(p -> p.progress(1.0).total(1.0).message("Analysis complete"));
        }

        return result;
    }

    @McpPrompt(
        name = "document-summary",
        description = "Generate document summary prompt")
    public GetPromptResult documentSummaryPrompt(
            @McpArg(name = "docId", required = true) String docId,
            @McpArg(name = "length", required = false) String length) {

        Document doc = documents.get(docId);
        if (doc == null) {
            return new GetPromptResult("Error",
                List.of(new PromptMessage(Role.SYSTEM,
                    new TextContent("Document not found"))));
        }

        String promptText = String.format(
            "Please summarize the following document in %s:\n\n%s",
            length != null ? length : "a few paragraphs",
            doc.getContent()
        );

        return new GetPromptResult("Document Summary",
            List.of(new PromptMessage(Role.USER, new TextContent(promptText))));
    }

    @McpComplete(prompt = "document-summary")
    public List<String> completeDocumentId(String prefix) {
        return documents.keySet().stream()
            .filter(id -> id.startsWith(prefix))
            .sorted()
            .limit(10)
            .toList();
    }
}
```

Example 4 (java):
```java
@SpringBootApplication
public class McpClientApplication {
    public static void main(String[] args) {
        SpringApplication.run(McpClientApplication.class, args);
    }
}

@Component
public class ClientHandlers {

    private final Logger logger = LoggerFactory.getLogger(ClientHandlers.class);
    private final ProgressTracker progressTracker = new ProgressTracker();
    private final ChatModel chatModel;

    public ClientHandlers(@Lazy ChatModel chatModel) {
        this.chatModel = chatModel;
    }

    @McpLogging(clients = "server1")
    public void handleLogging(LoggingMessageNotification notification) {
        switch (notification.level()) {
            case ERROR:
                logger.error("[MCP] {} - {}", notification.logger(), notification.data());
                break;
            case WARNING:
                logger.warn("[MCP] {} - {}", notification.logger(), notification.data());
                break;
            case INFO:
                logger.info("[MCP] {} - {}", notification.logger(), notification.data());
                break;
            default:
                logger.debug("[MCP] {} - {}", notification.logger(), notification.data());
        }
    }

    @McpSampling(clients = "server1")
    public CreateMessageResult handleSampling(CreateMessageRequest request) {
        // Use Spring AI ChatModel for sampling
        List<Message> messages = request.messages().stream()
            .map(msg -> {
                if (msg.role() == Role.USER) {
                    return new UserMessage(((TextContent) msg.content()).text());
                } else {
                    return AssistantMessage.builder()
                        .content(((TextContent) msg.content()).text())
                        .build();
                }
            })
            .toList();

        ChatResponse response = chatModel.call(new Prompt(messages));

        return CreateMessageResult.builder()
            .role(Role.ASSISTANT)
            .content(new TextContent(response.getResult().getOutput().getContent()))
            .model(request.modelPreferences().hints().get(0).name())
            .build();
    }

    @McpElicitation(clients = "server1")
    public ElicitResult handleElicitation(ElicitRequest request) {
        // In a real application, this would show a UI dialog
        Map<String, Object> userData = new HashMap<>();

        logger.info("Elicitation requested: {}", request.message());

        // Simulate user input based on schema
        Map<String, Object> schema = request.requestedSchema();
        if (schema != null && schema.containsKey("properties")) {
            Map<String, Object> properties = (Map<String, Object>) schema.get("properties");

            properties.forEach((key, value) -> {
                // In real app, prompt user for each field
                userData.put(key, getDefaultValueForProperty(key, value));
            });
        }

        return new ElicitResult(ElicitResult.Action.ACCEPT, userData);
    }

    @McpProgress(clients = "server1")
    public void handleProgress(ProgressNotification notification) {
        progressTracker.update(
            notification.progressToken(),
            notification.progress(),
            notification.total(),
            notification.message()
        );

        // Update UI or send websocket notification
        broadcastProgress(notification);
    }

    @McpToolListChanged(clients = "server1")
    public void handleServer1ToolsChanged(List<McpSchema.Tool> tools) {
        logger.info("Server1 tools updated: {} tools available", tools.size());

        // Update tool registry
        toolRegistry.updateServerTools("server1", tools);

        // Notify UI to refresh tool list
        eventBus.publish(new ToolsUpdatedEvent("server1", tools));
    }

    @McpResourceListChanged(clients = "server1")
    public void handleServer1ResourcesChanged(List<McpSchema.Resource> resources) {
        logger.info("Server1 resources updated: {} resources available", resources.size());

        // Clear resource cache for this server
        resourceCache.clearServer("server1");

        // Register new resources
        resources.forEach(resource ->
            resourceCache.register("server1", resource));
    }
}
```

---

## MCP Utilities :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/mcp/mcp-helpers.html

**Contents:**
- MCP Utilities
- ToolCallback Utility
  - Tool Callback Adapter
  - Tool Callback Providers
- McpToolUtils
  - ToolCallbacks to ToolSpecifications
  - MCP Clients to ToolCallbacks
- Native Image Support

For the latest snapshot version, please use Spring AI 1.1.2!

The MCP utilities provide foundational support for integrating Model Context Protocol with Spring AI applications. These utilities enable seamless communication between Spring AI’s tool system and MCP servers, supporting both synchronous and asynchronous operations. They are typically used for programmatic MCP Client and Server configuration and interaction. For a more streamlined configuration, consider using the boot starters.

Adapts MCP tools to Spring AI’s tool interface with both synchronous and asynchronous execution support.

Discovers and provides MCP tools from MCP clients.

For multiple clients:

For dynamic selection of a subset of clients

For multiple clients:

Converting Spring AI tool callbacks to MCP tool specifications:

then you can use the McpServer.SyncSpecification to register the tool specifications:

then you can use the McpServer.AsyncSpecification to register the tool specifications:

Getting tool callbacks from MCP clients

The McpHints class provides GraalVM native image hints for MCP schema classes. This class automatically registers all necessary reflection hints for MCP schema classes when building native images.

**Examples:**

Example 1 (java):
```java
McpSyncClient mcpClient = // obtain MCP client
Tool mcpTool = // obtain MCP tool definition
ToolCallback callback = new SyncMcpToolCallback(mcpClient, mcpTool);

// Use the tool through Spring AI's interfaces
ToolDefinition definition = callback.getToolDefinition();
String result = callback.call("{\"param\": \"value\"}");
```

Example 2 (java):
```java
McpAsyncClient mcpClient = // obtain MCP client
Tool mcpTool = // obtain MCP tool definition
ToolCallback callback = new AsyncMcpToolCallback(mcpClient, mcpTool);

// Use the tool through Spring AI's interfaces
ToolDefinition definition = callback.getToolDefinition();
String result = callback.call("{\"param\": \"value\"}");
```

Example 3 (java):
```java
McpSyncClient mcpClient = // obtain MCP client
ToolCallbackProvider provider = new SyncMcpToolCallbackProvider(mcpClient);

// Get all available tools
ToolCallback[] tools = provider.getToolCallbacks();
```

Example 4 (java):
```java
List<McpSyncClient> clients = // obtain list of clients
List<ToolCallback> callbacks = SyncMcpToolCallbackProvider.syncToolCallbacks(clients);
```

---

## MCP Client Annotations :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/mcp/mcp-annotations-client.html

**Contents:**
- MCP Client Annotations
- Client Annotations
  - @McpLogging
    - Basic Usage
    - With Individual Parameters
  - @McpSampling
    - Synchronous Implementation
    - Asynchronous Implementation
  - @McpElicitation
    - Basic Usage

The MCP Client Annotations provide a declarative way to implement MCP client handlers using Java annotations. These annotations simplify the handling of server notifications and client-side operations.

The @McpLogging annotation handles logging message notifications from MCP servers.

The @McpSampling annotation handles sampling requests from MCP servers for LLM completions.

The @McpElicitation annotation handles elicitation requests to gather additional information from users.

The @McpProgress annotation handles progress notifications for long-running operations.

The @McpToolListChanged annotation handles notifications when the server’s tool list changes.

The @McpResourceListChanged annotation handles notifications when the server’s resource list changes.

The @McpPromptListChanged annotation handles notifications when the server’s prompt list changes.

With Spring Boot auto-configuration, client handlers are automatically detected and registered:

The auto-configuration will:

Scan for beans with MCP client annotations

Create appropriate specifications

Register them with the MCP client

Support both sync and async implementations

Handle multiple clients with client-specific handlers

Configure the client annotation scanner and client connections:

The annotated handlers are automatically integrated with the MCP client:

For each MCP client connection, handlers with matching clients will be automatically registered and invoked when the corresponding events occur.

MCP Annotations Overview

MCP Client Boot Starter

**Examples:**

Example 1 (java):
```java
@Component
public class LoggingHandler {

    @McpLogging(clients = "my-mcp-server")
    public void handleLoggingMessage(LoggingMessageNotification notification) {
        System.out.println("Received log: " + notification.level() +
                          " - " + notification.data());
    }
}
```

Example 2 (java):
```java
@McpLogging(clients = "my-mcp-server")
public void handleLoggingWithParams(LoggingLevel level, String logger, String data) {
    System.out.println(String.format("[%s] %s: %s", level, logger, data));
}
```

Example 3 (java):
```java
@Component
public class SamplingHandler {

    @McpSampling(clients = "llm-server")
    public CreateMessageResult handleSamplingRequest(CreateMessageRequest request) {
        // Process the request and generate a response
        String response = generateLLMResponse(request);

        return CreateMessageResult.builder()
            .role(Role.ASSISTANT)
            .content(new TextContent(response))
            .model("gpt-4")
            .build();
    }
}
```

Example 4 (java):
```java
@Component
public class AsyncSamplingHandler {

    @McpSampling(clients = "llm-server")
    public Mono<CreateMessageResult> handleAsyncSampling(CreateMessageRequest request) {
        return Mono.fromCallable(() -> {
            String response = generateLLMResponse(request);

            return CreateMessageResult.builder()
                .role(Role.ASSISTANT)
                .content(new TextContent(response))
                .model("gpt-4")
                .build();
        }).subscribeOn(Schedulers.boundedElastic());
    }
}
```

---

## MCP Client Annotations :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/mcp/mcp-annotations-client.html

**Contents:**
- MCP Client Annotations
- Client Annotations
  - @McpLogging
    - Basic Usage
    - With Individual Parameters
  - @McpSampling
    - Synchronous Implementation
    - Asynchronous Implementation
  - @McpElicitation
    - Basic Usage

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The MCP Client Annotations provide a declarative way to implement MCP client handlers using Java annotations. These annotations simplify the handling of server notifications and client-side operations.

The @McpLogging annotation handles logging message notifications from MCP servers.

The @McpSampling annotation handles sampling requests from MCP servers for LLM completions.

The @McpElicitation annotation handles elicitation requests to gather additional information from users.

The @McpProgress annotation handles progress notifications for long-running operations.

The @McpToolListChanged annotation handles notifications when the server’s tool list changes.

The @McpResourceListChanged annotation handles notifications when the server’s resource list changes.

The @McpPromptListChanged annotation handles notifications when the server’s prompt list changes.

With Spring Boot auto-configuration, client handlers are automatically detected and registered:

The auto-configuration will:

Scan for beans with MCP client annotations

Create appropriate specifications

Register them with the MCP client

Support both sync and async implementations

Handle multiple clients with client-specific handlers

Configure the client annotation scanner and client connections:

The annotated handlers are automatically integrated with the MCP client:

For each MCP client connection, handlers with matching clients will be automatically registered and invoked when the corresponding events occur.

MCP Annotations Overview

MCP Client Boot Starter

**Examples:**

Example 1 (java):
```java
@Component
public class LoggingHandler {

    @McpLogging(clients = "my-mcp-server")
    public void handleLoggingMessage(LoggingMessageNotification notification) {
        System.out.println("Received log: " + notification.level() +
                          " - " + notification.data());
    }
}
```

Example 2 (java):
```java
@McpLogging(clients = "my-mcp-server")
public void handleLoggingWithParams(LoggingLevel level, String logger, String data) {
    System.out.println(String.format("[%s] %s: %s", level, logger, data));
}
```

Example 3 (java):
```java
@Component
public class SamplingHandler {

    @McpSampling(clients = "llm-server")
    public CreateMessageResult handleSamplingRequest(CreateMessageRequest request) {
        // Process the request and generate a response
        String response = generateLLMResponse(request);

        return CreateMessageResult.builder()
            .role(Role.ASSISTANT)
            .content(new TextContent(response))
            .model("gpt-4")
            .build();
    }
}
```

Example 4 (java):
```java
@Component
public class AsyncSamplingHandler {

    @McpSampling(clients = "llm-server")
    public Mono<CreateMessageResult> handleAsyncSampling(CreateMessageRequest request) {
        return Mono.fromCallable(() -> {
            String response = generateLLMResponse(request);

            return CreateMessageResult.builder()
                .role(Role.ASSISTANT)
                .content(new TextContent(response))
                .model("gpt-4")
                .build();
        }).subscribeOn(Schedulers.boundedElastic());
    }
}
```

---

## MCP Client Boot Starter :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/mcp/mcp-client-boot-starter-docs.html

**Contents:**
- MCP Client Boot Starter
- Starters
  - Standard MCP Client
  - WebFlux Client
- Configuration Properties
  - Common Properties
  - MCP Annotations Properties
  - Stdio Transport Properties
  - Windows STDIO Configuration
    - Why Windows Needs Special Handling

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The Spring AI MCP (Model Context Protocol) Client Boot Starter provides auto-configuration for MCP client functionality in Spring Boot applications. It supports both synchronous and asynchronous client implementations with various transport options.

The MCP Client Boot Starter provides:

Management of multiple client instances

Automatic client initialization (if enabled)

Support for multiple named transports (STDIO, Http/SSE and Streamable HTTP)

Integration with Spring AI’s tool execution framework

Tool filtering capabilities for selective tool inclusion/exclusion

Customizable tool name prefix generation for avoiding naming conflicts

Proper lifecycle management with automatic cleanup of resources when the application context is closed

Customizable client creation through customizers

The standard starter connects simultaneously to one or more MCP servers over STDIO (in-process), SSE, Streamable-HTTP and Stateless Streamable-HTTP transports. The SSE and Streamable-Http transports use the JDK HttpClient-based transport implementation. Each connection to an MCP server creates a new MCP client instance. You can choose either SYNC or ASYNC MCP clients (note: you cannot mix sync and async clients). For production deployment, we recommend using the WebFlux-based SSE & StreamableHttp connection with the spring-ai-starter-mcp-client-webflux.

The WebFlux starter provides similar functionality to the standard starter but uses a WebFlux-based Streamable-Http, Stateless Streamable-Http and SSE transport implementation.

The common properties are prefixed with spring.ai.mcp.client:

Enable/disable the MCP client

Name of the MCP client instance

Version of the MCP client instance

Whether to initialize clients on creation

Timeout duration for MCP client requests

Client type (SYNC or ASYNC). All clients must be either sync or async; mixing is not supported

root-change-notification

Enable/disable root change notifications for all clients

Enable/disable the MCP tool callback integration with Spring AI’s tool execution framework

MCP Client Annotations provide a declarative way to implement MCP client handlers using Java annotations. The client mcp-annotations properties are prefixed with spring.ai.mcp.client.annotation-scanner:

Enable/disable the MCP client annotations auto-scanning

Properties for Standard I/O transport are prefixed with spring.ai.mcp.client.stdio:

servers-configuration

Resource containing the MCP servers configuration in JSON format

Map of named stdio connection configurations

connections.[name].command

The command to execute for the MCP server

connections.[name].args

List of command arguments

connections.[name].env

Map of environment variables for the server process

Example configuration:

Alternatively, you can configure stdio connections using an external JSON file using the Claude Desktop format:

The Claude Desktop format looks like this:

When Java’s ProcessBuilder (used internally by StdioClientTransport) attempts to spawn a process on Windows, it can only execute:

Native executables (.exe files)

System commands available to cmd.exe

Windows batch files like npx.cmd, npm.cmd, and even python.cmd (from the Microsoft Store) require the cmd.exe shell to execute them.

Wrap batch file commands with cmd.exe /c:

Windows Configuration:

Linux/macOS Configuration:

For applications that need to work across platforms without separate configuration files, use OS detection in your Spring Boot application:

Relative paths (recommended for portability):

The MCP server resolves relative paths based on the application’s working directory.

Absolute paths (Windows requires backslashes or escaped forward slashes):

npx.cmd, npm.cmd - Node package managers

python.cmd - Python (Microsoft Store installation)

pip.cmd - Python package manager

mvn.cmd - Maven wrapper

gradle.cmd - Gradle wrapper

Custom .cmd or .bat scripts

See Spring AI Examples - Filesystem for a complete cross-platform MCP client implementation that automatically detects the OS and configures the client appropriately.

Used for connecting to Streamable-HTTP and Stateless Streamable-HTTP MCP servers.

Properties for Streamable-HTTP transport are prefixed with spring.ai.mcp.client.streamable-http:

Map of named Streamable-HTTP connection configurations

connections.[name].url

Base URL endpoint for Streamable-Http communication with the MCP server

connections.[name].endpoint

the streamable-http endpoint (as url suffix) to use for the connection

Example configuration:

Properties for Server-Sent Events (SSE) transport are prefixed with spring.ai.mcp.client.sse:

Map of named SSE connection configurations

connections.[name].url

Base URL endpoint for SSE communication with the MCP server

connections.[name].sse-endpoint

the sse endpoint (as url suffix) to use for the connection

Example configurations:

When you have a full SSE URL, split it into base URL and endpoint path:

http://localhost:3000/mcp-hub/sse/token123

url: localhost:3000 sse-endpoint: /mcp-hub/sse/token123

https://api.service.com/v2/events?key=secret

url: api.service.com sse-endpoint: /v2/events?key=secret

http://localhost:8080/sse

url: localhost:8080 sse-endpoint: /sse (or omit for default)

404 Not Found Errors:

Verify URL splitting: ensure the base url contains only the scheme, host, and port

Check the sse-endpoint starts with / and includes the full path and query parameters

Test the full URL directly in a browser or curl to confirm it’s accessible

Properties for Streamable Http transport are prefixed with spring.ai.mcp.client.streamable-http:

Map of named Streamable Http connection configurations

connections.[name].url

Base URL endpoint for Streamable-Http communication with the MCP server

connections.[name].endpoint

the streamable-http endpoint (as url suffix) to use for the connection

Example configuration:

The starter supports two types of clients:

Synchronous - default client type (spring.ai.mcp.client.type=SYNC), suitable for traditional request-response patterns with blocking operations

NOTE: The SYNC client will register only synchronous MCP annotated methods. Asynchronous methods will be ignored.

Asynchronous - suitable for reactive applications with non-blocking operations, configured using spring.ai.mcp.client.type=ASYNC

NOTE: The ASYNC client will register only asynchronous MCP annotated methods. Synchronous methods will be ignored.

The auto-configuration provides extensive client spec customization capabilities through callback interfaces. These customizers allow you to configure various aspects of the MCP client behavior, from request timeouts to event handling and message processing.

The following customization options are available:

Request Configuration - Set custom request timeouts

Custom Sampling Handlers - standardized way for servers to request LLM sampling (completions or generations) from LLMs via clients. This flow allows clients to maintain control over model access, selection, and permissions while enabling servers to leverage AI capabilities — with no server API keys necessary.

File system (Roots) Access - standardized way for clients to expose filesystem roots to servers. Roots define the boundaries of where servers can operate within the filesystem, allowing them to understand which directories and files they have access to. Servers can request the list of roots from supporting clients and receive notifications when that list changes.

Elicitation Handlers - standardized way for servers to request additional information from users through the client during interactions.

Event Handlers - client’s handler to be notified when a certain server event occurs:

Tools change notifications - when the list of available server tools changes

Resources change notifications - when the list of available server resources changes.

Prompts change notifications - when the list of available server prompts changes.

Logging Handlers - standardized way for servers to send structured log messages to clients.

Progress Handlers - standardized way for servers to send structured progress messages to clients.

Clients can control logging verbosity by setting minimum log levels

You can implement either McpSyncClientCustomizer for synchronous clients or McpAsyncClientCustomizer for asynchronous clients, depending on your application’s needs.

The serverConfigurationName parameter is the name of the server configuration that the customizer is being applied to and the MCP Client is created for.

The MCP client auto-configuration automatically detects and applies any customizers found in the application context.

The auto-configuration supports multiple transport types:

Standard I/O (Stdio) (activated by the spring-ai-starter-mcp-client and spring-ai-starter-mcp-client-webflux)

(HttpClient) HTTP/SSE and Streamable-HTTP (activated by the spring-ai-starter-mcp-client)

(WebFlux) HTTP/SSE and Streamable-HTTP (activated by the spring-ai-starter-mcp-client-webflux)

The MCP Client Boot Starter supports filtering of discovered tools through the McpToolFilter interface. This allows you to selectively include or exclude tools based on custom criteria such as the MCP connection information or tool properties.

To implement tool filtering, create a bean that implements the McpToolFilter interface:

The McpConnectionInfo record provides access to:

clientCapabilities - The capabilities of the MCP client

clientInfo - Information about the MCP client (name and version)

initializeResult - The initialization result from the MCP server

The filter is automatically detected and applied to both synchronous and asynchronous MCP tool callback providers. If no custom filter is provided, all discovered tools are included by default.

Note: Only one McpToolFilter bean should be defined in the application context. If multiple filters are needed, combine them into a single composite filter implementation.

The MCP Client Boot Starter supports customizable tool name prefix generation through the McpToolNamePrefixGenerator interface. This feature helps avoid naming conflicts when integrating tools from multiple MCP servers by adding unique prefixes to tool names.

By default, if no custom McpToolNamePrefixGenerator bean is provided, the starter uses DefaultMcpToolNamePrefixGenerator which ensures unique tool names across all MCP client connections. The default generator:

Tracks all existing connections and tool names to ensure uniqueness

Formats tool names by replacing non-alphanumeric characters with underscores (e.g., my-tool becomes my_tool)

When duplicate tool names are detected across different connections, adds a counter prefix (e.g., alt_1_toolName, alt_2_toolName)

Is thread-safe and maintains idempotency - the same combination of (client, server, tool) always gets the same unique name

Ensures the final name doesn’t exceed 64 characters (truncating from the beginning if necessary)

For example: * First occurrence of tool search → search * Second occurrence of tool search from a different connection → alt_1_search * Tool with special characters my-special-tool → my_special_tool

You can customize this behavior by providing your own implementation:

The McpConnectionInfo record provides comprehensive information about the MCP connection:

clientCapabilities - The capabilities of the MCP client

clientInfo - Information about the MCP client (name, title, and version)

initializeResult - The initialization result from the MCP server, including server information

The framework provides several built-in prefix generators:

DefaultMcpToolNamePrefixGenerator - Ensures unique tool names by tracking duplicates and adding counter prefixes when needed (used by default if no custom bean is provided)

McpToolNamePrefixGenerator.noPrefix() - Returns tool names without any prefix (may cause conflicts if multiple servers provide tools with the same name)

To disable prefixing entirely and use raw tool names (not recommended if using multiple MCP servers), register the no-prefix generator as a bean:

The prefix generator is automatically detected and applied to both synchronous and asynchronous MCP tool callback providers through Spring’s ObjectProvider mechanism. If no custom generator bean is provided, the DefaultMcpToolNamePrefixGenerator is used automatically.

The MCP Client Boot Starter supports customizable conversion of Spring AI’s ToolContext to MCP tool-call metadata through the ToolContextToMcpMetaConverter interface. This feature allows you to pass additional contextual information (e.g. user id, secrets token) as metadata along with the LLM’s generated call arguments.

For example you can pass the MCP progressToken to your MCP Progress Flow in the tool context to track the progress of long-running operations:

By default, if no custom converter bean is provided, the starter uses ToolContextToMcpMetaConverter.defaultConverter() which:

Filters out the MCP exchange key (McpToolUtils.TOOL_CONTEXT_MCP_EXCHANGE_KEY)

Filters out entries with null values

Passes through all other context entries as metadata

You can customize this behavior by providing your own implementation:

The framework provides built-in converters:

ToolContextToMcpMetaConverter.defaultConverter() - Filters out MCP exchange key and null values (used by default if no custom bean is provided)

ToolContextToMcpMetaConverter.noOp() - Returns an empty map, effectively disabling context-to-metadata conversion

To disable context-to-metadata conversion entirely:

The converter is automatically detected and applied to both synchronous and asynchronous MCP tool callbacks through Spring’s ObjectProvider mechanism. If no custom converter bean is provided, the default converter is used automatically.

The MCP ToolCallback auto-configuration is enabled by default, but can be disabled with the spring.ai.mcp.client.toolcallback.enabled=false property.

When disabled, no ToolCallbackProvider bean is created from the available MCP tools.

The MCP Client Boot Starter automatically detects and registers annotated methods for handling various MCP client operations:

@McpLogging - Handles logging message notifications from MCP servers

@McpSampling - Handles sampling requests from MCP servers for LLM completions

@McpElicitation - Handles elicitation requests to gather additional information from users

@McpProgress - Handles progress notifications for long-running operations

@McpToolListChanged - Handles notifications when the server’s tool list changes

@McpResourceListChanged - Handles notifications when the server’s resource list changes

@McpPromptListChanged - Handles notifications when the server’s prompt list changes

The annotations support both synchronous and asynchronous implementations, and can be configured for specific clients using the clients parameter:

For detailed information about all available annotations and their usage patterns, see the MCP Client Annotations documentation.

Add the appropriate starter dependency to your project and configure the client in application.properties or application.yml:

The MCP client beans will be automatically configured and available for injection:

When tool callbacks are enabled (the default behavior), the registered MCP Tools with all MCP clients are provided as a ToolCallbackProvider instance:

Brave Web Search Chatbot - A chatbot that uses the Model Context Protocol to interact with a web search server.

Default MCP Client Starter - A simple example of using the default spring-ai-starter-mcp-client MCP Client Boot Starter.

WebFlux MCP Client Starter - A simple example of using the spring-ai-starter-mcp-client-webflux MCP Client Boot Starter.

Spring AI Documentation

Model Context Protocol Specification

Spring Boot Auto-configuration

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-client</artifactId>
</dependency>
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-client-webflux</artifactId>
</dependency>
```

Example 3 (yaml):
```yaml
spring:
  ai:
    mcp:
      client:
        stdio:
          root-change-notification: true
          connections:
            server1:
              command: /path/to/server
              args:
                - --port=8080
                - --mode=production
              env:
                API_KEY: your-api-key
                DEBUG: "true"
```

Example 4 (yaml):
```yaml
spring:
  ai:
    mcp:
      client:
        stdio:
          servers-configuration: classpath:mcp-servers.json
```

---

## MCP Server Boot Starter :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/mcp/mcp-server-boot-starter-docs.html

**Contents:**
- MCP Server Boot Starter
- MCP Server Boot Starters
  - STDIO
  - WebMVC
  - WebMVC (Reactive)
- Server Capabilities
- Server Protocols
- Sync/Async Server API Options
- MCP Server Annotations
  - Key Annotations

Model Context Protocol (MCP) Servers are programs that expose specific capabilities to AI applications through standardized protocol interfaces. Each server provides focused functionality for a particular domain.

The Spring AI MCP Server Boot Starters provide auto-configuration for setting up MCP Servers in Spring Boot applications. They enable seamless integration of MCP server capabilities with Spring Boot’s auto-configuration system.

The MCP Server Boot Starters offer:

Automatic configuration of MCP server components, including tools, resources, and prompts

Support for different MCP protocol versions, including STDIO, SSE, Streamable-HTTP, and stateless servers

Support for both synchronous and asynchronous operation modes

Multiple transport layer options

Flexible tool, resource, and prompt specification

Change notification capabilities

Annotation-based server development with automatic bean scanning and registration

MCP Servers support multiple protocol and transport mechanisms. Use the dedicated starter and the correct spring.ai.mcp.server.protocol property to configure your server:

Standard Input/Output (STDIO)

spring-ai-starter-mcp-server

spring.ai.mcp.server.stdio=true

spring-ai-starter-mcp-server-webmvc

spring.ai.mcp.server.protocol=SSE or empty

Streamable-HTTP WebMVC

spring-ai-starter-mcp-server-webmvc

spring.ai.mcp.server.protocol=STREAMABLE

spring-ai-starter-mcp-server-webmvc

spring.ai.mcp.server.protocol=STATELESS

spring-ai-starter-mcp-server-webflux

spring.ai.mcp.server.protocol=SSE or empty

Streamable-HTTP WebFlux

spring-ai-starter-mcp-server-webflux

spring.ai.mcp.server.protocol=STREAMABLE

spring-ai-starter-mcp-server-webflux

spring.ai.mcp.server.protocol=STATELESS

Depending on the server and transport types, MCP Servers can support various capabilities, such as:

Tools - Allows servers to expose tools that can be invoked by language models

Resources - Provides a standardized way for servers to expose resources to clients

Prompts - Provides a standardized way for servers to expose prompt templates to clients

Utility/Completions - Provides a standardized way for servers to offer argument autocompletion suggestions for prompts and resource URIs

Utility/Logging - Provides a standardized way for servers to send structured log messages to clients

Utility/Progress - Optional progress tracking for long-running operations through notification messages

Utility/Ping - Optional health check mechanism for the server to report its status

All capabilities are enabled by default. Disabling a capability will prevent the server from registering and exposing the corresponding features to clients.

MCP provides several protocol types including:

STDIO - In process (e.g. server runs inside the host application) protocol. Communication is over standard in and standard out. To enable the STDIO set spring.ai.mcp.server.stdio=true.

SSE - Server-sent events protocol for real-time updates. The server operates as an independent process that can handle multiple client connections.

Streamable-HTTP - The Streamable HTTP transport allows MCP servers to operate as independent processes that can handle multiple client connections using HTTP POST and GET requests, with optional Server-Sent Events (SSE) streaming for multiple server messages. It replaces the SSE transport. To enable the STREAMABLE protocol, set spring.ai.mcp.server.protocol=STREAMABLE.

Stateless - Stateless MCP servers are designed for simplified deployments where session state is not maintained between requests. They are ideal for microservices architectures and cloud-native deployments. To enable the STATELESS protocol, set spring.ai.mcp.server.protocol=STATELESS.

The MCP Server API supports imperative (i.e. synchronous) and reactive (e.g. asynchronous) programming models.

Synchronous Server - The default server type implemented using McpSyncServer. It is designed for straightforward request-response patterns in your applications. To enable this server type, set spring.ai.mcp.server.type=SYNC in your configuration. When activated, it automatically handles the configuration of synchronous tool specifications.

NOTE: The SYNC server will register only synchronous MCP annotated methods. Asynchronous methods will be ignored.

Asynchronous Server - The asynchronous server implementation uses McpAsyncServer and is optimized for non-blocking operations. To enable this server type, configure your application with spring.ai.mcp.server.type=ASYNC. This server type automatically sets up asynchronous tool specifications with built-in Project Reactor support.

NOTE: The ASYNC server will register only asynchronous MCP annotated methods. Synchronous methods will be ignored.

The MCP Server Boot Starters provide comprehensive support for annotation-based server development, allowing you to create MCP servers using declarative Java annotations instead of manual configuration.

@McpTool - Mark methods as MCP tools with automatic JSON schema generation

@McpResource - Provide access to resources via URI templates

@McpPrompt - Generate prompt messages for AI interactions

@McpComplete - Provide auto-completion functionality for prompts

The annotation system supports special parameter types that provide additional context:

McpMeta - Access metadata from MCP requests

@McpProgressToken - Receive progress tokens for long-running operations

McpSyncServerExchange/McpAsyncServerExchange - Full server context for advanced operations

McpTransportContext - Lightweight context for stateless operations

CallToolRequest - Dynamic schema support for flexible tools

With Spring Boot auto-configuration, annotated beans are automatically detected and registered:

The auto-configuration will:

Scan for beans with MCP annotations

Create appropriate specifications

Register them with the MCP server

Handle both sync and async implementations based on configuration

Configure the server annotation scanner:

Server Annotations Reference - Complete guide to server annotations

Special Parameters - Advanced parameter injection

Examples - Comprehensive examples and use cases

Weather Server (SSE WebFlux) - Spring AI MCP Server Boot Starter with WebFlux transport

Weather Server (STDIO) - Spring AI MCP Server Boot Starter with STDIO transport

Weather Server Manual Configuration - Spring AI MCP Server Boot Starter that doesn’t use auto-configuration but uses the Java SDK to configure the server manually

Streamable-HTTP WebFlux/WebMVC Example - TODO

Stateless WebFlux/WebMVC Example - TODO

MCP Server Annotations - Declarative server development with annotations

Special Parameters - Advanced parameter injection and context access

MCP Annotations Examples - Comprehensive examples and use cases

Spring AI Documentation

Model Context Protocol Specification

Spring Boot Auto-configuration

**Examples:**

Example 1 (java):
```java
@Component
public class CalculatorTools {

    @McpTool(name = "add", description = "Add two numbers together")
    public int add(
            @McpToolParam(description = "First number", required = true) int a,
            @McpToolParam(description = "Second number", required = true) int b) {
        return a + b;
    }

    @McpResource(uri = "config://{key}", name = "Configuration")
    public String getConfig(String key) {
        return configData.get(key);
    }
}
```

Example 2 (java):
```java
@SpringBootApplication
public class McpServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(McpServerApplication.class, args);
    }
}
```

Example 3 (yaml):
```yaml
spring:
  ai:
    mcp:
      server:
        type: SYNC  # or ASYNC
        annotation-scanner:
          enabled: true
```

---

## MCP Client Boot Starter :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/mcp/mcp-client-boot-starter-docs.html

**Contents:**
- MCP Client Boot Starter
- Starters
  - Standard MCP Client
  - WebFlux Client
- Configuration Properties
  - Common Properties
  - Stdio Transport Properties
  - SSE Transport Properties
- Features
  - Sync/Async Client Types

For the latest snapshot version, please use Spring AI 1.1.2!

The Spring AI MCP (Model Context Protocol) Client Boot Starter provides auto-configuration for MCP client functionality in Spring Boot applications. It supports both synchronous and asynchronous client implementations with various transport options.

The MCP Client Boot Starter provides:

Management of multiple client instances

Automatic client initialization (if enabled)

Support for multiple named transports

Integration with Spring AI’s tool execution framework

Proper lifecycle management with automatic cleanup of resources when the application context is closed

Customizable client creation through customizers

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

The standard starter connects simultaneously to one or more MCP servers over STDIO (in-process) and/or SSE (remote) transports. The SSE connection uses the HttpClient-based transport implementation. Each connection to an MCP server creates a new MCP client instance. You can choose either SYNC or ASYNC MCP clients (note: you cannot mix sync and async clients). For production deployment, we recommend using the WebFlux-based SSE connection with the spring-ai-starter-mcp-client-webflux.

The WebFlux starter provides similar functionality to the standard starter but uses a WebFlux-based SSE transport implementation.

The common properties are prefixed with spring.ai.mcp.client:

Enable/disable the MCP client

Name of the MCP client instance

Version of the MCP client instance

Whether to initialize clients on creation

Timeout duration for MCP client requests

Client type (SYNC or ASYNC). All clients must be either sync or async; mixing is not supported

root-change-notification

Enable/disable root change notifications for all clients

Enable/disable the MCP tool callback integration with Spring AI’s tool execution framework

Properties for Standard I/O transport are prefixed with spring.ai.mcp.client.stdio:

servers-configuration

Resource containing the MCP servers configuration in JSON format

Map of named stdio connection configurations

connections.[name].command

The command to execute for the MCP server

connections.[name].args

List of command arguments

connections.[name].env

Map of environment variables for the server process

Example configuration:

Alternatively, you can configure stdio connections using an external JSON file using the Claude Desktop format:

The Claude Desktop format looks like this:

Currently, the Claude Desktop format supports only STDIO connection types.

Properties for Server-Sent Events (SSE) transport are prefixed with spring.ai.mcp.client.sse:

Map of named SSE connection configurations

connections.[name].url

Base URL endpoint for SSE communication with the MCP server

connections.[name].sse-endpoint

the sse endpoint (as url suffix) to use for the connection

Example configuration:

The starter supports two types of clients:

Synchronous - default client type, suitable for traditional request-response patterns with blocking operations

Asynchronous - suitable for reactive applications with non-blocking operations, configured using spring.ai.mcp.client.type=ASYNC

The auto-configuration provides extensive client spec customization capabilities through callback interfaces. These customizers allow you to configure various aspects of the MCP client behavior, from request timeouts to event handling and message processing.

The following customization options are available:

Request Configuration - Set custom request timeouts

Custom Sampling Handlers - standardized way for servers to request LLM sampling (completions or generations) from LLMs via clients. This flow allows clients to maintain control over model access, selection, and permissions while enabling servers to leverage AI capabilities — with no server API keys necessary.

File system (Roots) Access - standardized way for clients to expose filesystem roots to servers. Roots define the boundaries of where servers can operate within the filesystem, allowing them to understand which directories and files they have access to. Servers can request the list of roots from supporting clients and receive notifications when that list changes.

Event Handlers - client’s handler to be notified when a certain server event occurs:

Tools change notifications - when the list of available server tools changes

Resources change notifications - when the list of available server resources changes.

Prompts change notifications - when the list of available server prompts changes.

Logging Handlers - standardized way for servers to send structured log messages to clients. Clients can control logging verbosity by setting minimum log levels

You can implement either McpSyncClientCustomizer for synchronous clients or McpAsyncClientCustomizer for asynchronous clients, depending on your application’s needs.

The serverConfigurationName parameter is the name of the server configuration that the customizer is being applied to and the MCP Client is created for.

The MCP client auto-configuration automatically detects and applies any customizers found in the application context.

The auto-configuration supports multiple transport types:

Standard I/O (Stdio) (activated by the spring-ai-starter-mcp-client)

SSE HTTP (activated by the spring-ai-starter-mcp-client)

SSE WebFlux (activated by the spring-ai-starter-mcp-client-webflux)

The starter can configure tool callbacks that integrate with Spring AI’s tool execution framework, allowing MCP tools to be used as part of AI interactions. This integration is enabled by default and can be disabled by setting the spring.ai.mcp.client.toolcallback.enabled=false property.

Add the appropriate starter dependency to your project and configure the client in application.properties or application.yml:

The MCP client beans will be automatically configured and available for injection:

When tool callbacks are enabled (the default behavior), the registered MCP Tools with all MCP clients are provided as a ToolCallbackProvider instance:

Brave Web Search Chatbot - A chatbot that uses the Model Context Protocol to interact with a web search server.

Default MCP Client Starter - A simple example of using the default spring-ai-starter-mcp-client MCP Client Boot Starter.

WebFlux MCP Client Starter - A simple example of using the spring-ai-starter-mcp-client-webflux MCP Client Boot Starter.

Spring AI Documentation

Model Context Protocol Specification

Spring Boot Auto-configuration

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-client</artifactId>
</dependency>
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-client-webflux</artifactId>
</dependency>
```

Example 3 (yaml):
```yaml
spring:
  ai:
    mcp:
      client:
        stdio:
          root-change-notification: true
          connections:
            server1:
              command: /path/to/server
              args:
                - --port=8080
                - --mode=production
              env:
                API_KEY: your-api-key
                DEBUG: "true"
```

Example 4 (yaml):
```yaml
spring:
  ai:
    mcp:
      client:
        stdio:
          servers-configuration: classpath:mcp-servers.json
```

---

## MCP Client Boot Starter :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/mcp/mcp-client-boot-starter-docs.html

**Contents:**
- MCP Client Boot Starter
- Starters
  - Standard MCP Client
  - WebFlux Client
- Configuration Properties
  - Common Properties
  - MCP Annotations Properties
  - Stdio Transport Properties
  - Windows STDIO Configuration
    - Why Windows Needs Special Handling

The Spring AI MCP (Model Context Protocol) Client Boot Starter provides auto-configuration for MCP client functionality in Spring Boot applications. It supports both synchronous and asynchronous client implementations with various transport options.

The MCP Client Boot Starter provides:

Management of multiple client instances

Automatic client initialization (if enabled)

Support for multiple named transports (STDIO, Http/SSE and Streamable HTTP)

Integration with Spring AI’s tool execution framework

Tool filtering capabilities for selective tool inclusion/exclusion

Customizable tool name prefix generation for avoiding naming conflicts

Proper lifecycle management with automatic cleanup of resources when the application context is closed

Customizable client creation through customizers

The standard starter connects simultaneously to one or more MCP servers over STDIO (in-process), SSE, Streamable-HTTP and Stateless Streamable-HTTP transports. The SSE and Streamable-Http transports use the JDK HttpClient-based transport implementation. Each connection to an MCP server creates a new MCP client instance. You can choose either SYNC or ASYNC MCP clients (note: you cannot mix sync and async clients). For production deployment, we recommend using the WebFlux-based SSE & StreamableHttp connection with the spring-ai-starter-mcp-client-webflux.

The WebFlux starter provides similar functionality to the standard starter but uses a WebFlux-based Streamable-Http, Stateless Streamable-Http and SSE transport implementation.

The common properties are prefixed with spring.ai.mcp.client:

Enable/disable the MCP client

Name of the MCP client instance

Version of the MCP client instance

Whether to initialize clients on creation

Timeout duration for MCP client requests

Client type (SYNC or ASYNC). All clients must be either sync or async; mixing is not supported

root-change-notification

Enable/disable root change notifications for all clients

Enable/disable the MCP tool callback integration with Spring AI’s tool execution framework

MCP Client Annotations provide a declarative way to implement MCP client handlers using Java annotations. The client mcp-annotations properties are prefixed with spring.ai.mcp.client.annotation-scanner:

Enable/disable the MCP client annotations auto-scanning

Properties for Standard I/O transport are prefixed with spring.ai.mcp.client.stdio:

servers-configuration

Resource containing the MCP servers configuration in JSON format

Map of named stdio connection configurations

connections.[name].command

The command to execute for the MCP server

connections.[name].args

List of command arguments

connections.[name].env

Map of environment variables for the server process

Example configuration:

Alternatively, you can configure stdio connections using an external JSON file using the Claude Desktop format:

The Claude Desktop format looks like this:

When Java’s ProcessBuilder (used internally by StdioClientTransport) attempts to spawn a process on Windows, it can only execute:

Native executables (.exe files)

System commands available to cmd.exe

Windows batch files like npx.cmd, npm.cmd, and even python.cmd (from the Microsoft Store) require the cmd.exe shell to execute them.

Wrap batch file commands with cmd.exe /c:

Windows Configuration:

Linux/macOS Configuration:

For applications that need to work across platforms without separate configuration files, use OS detection in your Spring Boot application:

Relative paths (recommended for portability):

The MCP server resolves relative paths based on the application’s working directory.

Absolute paths (Windows requires backslashes or escaped forward slashes):

npx.cmd, npm.cmd - Node package managers

python.cmd - Python (Microsoft Store installation)

pip.cmd - Python package manager

mvn.cmd - Maven wrapper

gradle.cmd - Gradle wrapper

Custom .cmd or .bat scripts

See Spring AI Examples - Filesystem for a complete cross-platform MCP client implementation that automatically detects the OS and configures the client appropriately.

Used for connecting to Streamable-HTTP and Stateless Streamable-HTTP MCP servers.

Properties for Streamable-HTTP transport are prefixed with spring.ai.mcp.client.streamable-http:

Map of named Streamable-HTTP connection configurations

connections.[name].url

Base URL endpoint for Streamable-Http communication with the MCP server

connections.[name].endpoint

the streamable-http endpoint (as url suffix) to use for the connection

Example configuration:

Properties for Server-Sent Events (SSE) transport are prefixed with spring.ai.mcp.client.sse:

Map of named SSE connection configurations

connections.[name].url

Base URL endpoint for SSE communication with the MCP server

connections.[name].sse-endpoint

the sse endpoint (as url suffix) to use for the connection

Example configurations:

When you have a full SSE URL, split it into base URL and endpoint path:

http://localhost:3000/mcp-hub/sse/token123

url: localhost:3000 sse-endpoint: /mcp-hub/sse/token123

https://api.service.com/v2/events?key=secret

url: api.service.com sse-endpoint: /v2/events?key=secret

http://localhost:8080/sse

url: localhost:8080 sse-endpoint: /sse (or omit for default)

404 Not Found Errors:

Verify URL splitting: ensure the base url contains only the scheme, host, and port

Check the sse-endpoint starts with / and includes the full path and query parameters

Test the full URL directly in a browser or curl to confirm it’s accessible

Properties for Streamable Http transport are prefixed with spring.ai.mcp.client.streamable-http:

Map of named Streamable Http connection configurations

connections.[name].url

Base URL endpoint for Streamable-Http communication with the MCP server

connections.[name].endpoint

the streamable-http endpoint (as url suffix) to use for the connection

Example configuration:

The starter supports two types of clients:

Synchronous - default client type (spring.ai.mcp.client.type=SYNC), suitable for traditional request-response patterns with blocking operations

NOTE: The SYNC client will register only synchronous MCP annotated methods. Asynchronous methods will be ignored.

Asynchronous - suitable for reactive applications with non-blocking operations, configured using spring.ai.mcp.client.type=ASYNC

NOTE: The ASYNC client will register only asynchronous MCP annotated methods. Synchronous methods will be ignored.

The auto-configuration provides extensive client spec customization capabilities through callback interfaces. These customizers allow you to configure various aspects of the MCP client behavior, from request timeouts to event handling and message processing.

The following customization options are available:

Request Configuration - Set custom request timeouts

Custom Sampling Handlers - standardized way for servers to request LLM sampling (completions or generations) from LLMs via clients. This flow allows clients to maintain control over model access, selection, and permissions while enabling servers to leverage AI capabilities — with no server API keys necessary.

File system (Roots) Access - standardized way for clients to expose filesystem roots to servers. Roots define the boundaries of where servers can operate within the filesystem, allowing them to understand which directories and files they have access to. Servers can request the list of roots from supporting clients and receive notifications when that list changes.

Elicitation Handlers - standardized way for servers to request additional information from users through the client during interactions.

Event Handlers - client’s handler to be notified when a certain server event occurs:

Tools change notifications - when the list of available server tools changes

Resources change notifications - when the list of available server resources changes.

Prompts change notifications - when the list of available server prompts changes.

Logging Handlers - standardized way for servers to send structured log messages to clients.

Progress Handlers - standardized way for servers to send structured progress messages to clients.

Clients can control logging verbosity by setting minimum log levels

You can implement either McpSyncClientCustomizer for synchronous clients or McpAsyncClientCustomizer for asynchronous clients, depending on your application’s needs.

The serverConfigurationName parameter is the name of the server configuration that the customizer is being applied to and the MCP Client is created for.

The MCP client auto-configuration automatically detects and applies any customizers found in the application context.

The auto-configuration supports multiple transport types:

Standard I/O (Stdio) (activated by the spring-ai-starter-mcp-client and spring-ai-starter-mcp-client-webflux)

(HttpClient) HTTP/SSE and Streamable-HTTP (activated by the spring-ai-starter-mcp-client)

(WebFlux) HTTP/SSE and Streamable-HTTP (activated by the spring-ai-starter-mcp-client-webflux)

The MCP Client Boot Starter supports filtering of discovered tools through the McpToolFilter interface. This allows you to selectively include or exclude tools based on custom criteria such as the MCP connection information or tool properties.

To implement tool filtering, create a bean that implements the McpToolFilter interface:

The McpConnectionInfo record provides access to:

clientCapabilities - The capabilities of the MCP client

clientInfo - Information about the MCP client (name and version)

initializeResult - The initialization result from the MCP server

The filter is automatically detected and applied to both synchronous and asynchronous MCP tool callback providers. If no custom filter is provided, all discovered tools are included by default.

Note: Only one McpToolFilter bean should be defined in the application context. If multiple filters are needed, combine them into a single composite filter implementation.

The MCP Client Boot Starter supports customizable tool name prefix generation through the McpToolNamePrefixGenerator interface. This feature helps avoid naming conflicts when integrating tools from multiple MCP servers by adding unique prefixes to tool names.

By default, if no custom McpToolNamePrefixGenerator bean is provided, the starter uses DefaultMcpToolNamePrefixGenerator which ensures unique tool names across all MCP client connections. The default generator:

Tracks all existing connections and tool names to ensure uniqueness

Formats tool names by replacing non-alphanumeric characters with underscores (e.g., my-tool becomes my_tool)

When duplicate tool names are detected across different connections, adds a counter prefix (e.g., alt_1_toolName, alt_2_toolName)

Is thread-safe and maintains idempotency - the same combination of (client, server, tool) always gets the same unique name

Ensures the final name doesn’t exceed 64 characters (truncating from the beginning if necessary)

For example: * First occurrence of tool search → search * Second occurrence of tool search from a different connection → alt_1_search * Tool with special characters my-special-tool → my_special_tool

You can customize this behavior by providing your own implementation:

The McpConnectionInfo record provides comprehensive information about the MCP connection:

clientCapabilities - The capabilities of the MCP client

clientInfo - Information about the MCP client (name, title, and version)

initializeResult - The initialization result from the MCP server, including server information

The framework provides several built-in prefix generators:

DefaultMcpToolNamePrefixGenerator - Ensures unique tool names by tracking duplicates and adding counter prefixes when needed (used by default if no custom bean is provided)

McpToolNamePrefixGenerator.noPrefix() - Returns tool names without any prefix (may cause conflicts if multiple servers provide tools with the same name)

To disable prefixing entirely and use raw tool names (not recommended if using multiple MCP servers), register the no-prefix generator as a bean:

The prefix generator is automatically detected and applied to both synchronous and asynchronous MCP tool callback providers through Spring’s ObjectProvider mechanism. If no custom generator bean is provided, the DefaultMcpToolNamePrefixGenerator is used automatically.

The MCP Client Boot Starter supports customizable conversion of Spring AI’s ToolContext to MCP tool-call metadata through the ToolContextToMcpMetaConverter interface. This feature allows you to pass additional contextual information (e.g. user id, secrets token) as metadata along with the LLM’s generated call arguments.

For example you can pass the MCP progressToken to your MCP Progress Flow in the tool context to track the progress of long-running operations:

By default, if no custom converter bean is provided, the starter uses ToolContextToMcpMetaConverter.defaultConverter() which:

Filters out the MCP exchange key (McpToolUtils.TOOL_CONTEXT_MCP_EXCHANGE_KEY)

Filters out entries with null values

Passes through all other context entries as metadata

You can customize this behavior by providing your own implementation:

The framework provides built-in converters:

ToolContextToMcpMetaConverter.defaultConverter() - Filters out MCP exchange key and null values (used by default if no custom bean is provided)

ToolContextToMcpMetaConverter.noOp() - Returns an empty map, effectively disabling context-to-metadata conversion

To disable context-to-metadata conversion entirely:

The converter is automatically detected and applied to both synchronous and asynchronous MCP tool callbacks through Spring’s ObjectProvider mechanism. If no custom converter bean is provided, the default converter is used automatically.

The MCP ToolCallback auto-configuration is enabled by default, but can be disabled with the spring.ai.mcp.client.toolcallback.enabled=false property.

When disabled, no ToolCallbackProvider bean is created from the available MCP tools.

The MCP Client Boot Starter automatically detects and registers annotated methods for handling various MCP client operations:

@McpLogging - Handles logging message notifications from MCP servers

@McpSampling - Handles sampling requests from MCP servers for LLM completions

@McpElicitation - Handles elicitation requests to gather additional information from users

@McpProgress - Handles progress notifications for long-running operations

@McpToolListChanged - Handles notifications when the server’s tool list changes

@McpResourceListChanged - Handles notifications when the server’s resource list changes

@McpPromptListChanged - Handles notifications when the server’s prompt list changes

The annotations support both synchronous and asynchronous implementations, and can be configured for specific clients using the clients parameter:

For detailed information about all available annotations and their usage patterns, see the MCP Client Annotations documentation.

Add the appropriate starter dependency to your project and configure the client in application.properties or application.yml:

The MCP client beans will be automatically configured and available for injection:

When tool callbacks are enabled (the default behavior), the registered MCP Tools with all MCP clients are provided as a ToolCallbackProvider instance:

Brave Web Search Chatbot - A chatbot that uses the Model Context Protocol to interact with a web search server.

Default MCP Client Starter - A simple example of using the default spring-ai-starter-mcp-client MCP Client Boot Starter.

WebFlux MCP Client Starter - A simple example of using the spring-ai-starter-mcp-client-webflux MCP Client Boot Starter.

Spring AI Documentation

Model Context Protocol Specification

Spring Boot Auto-configuration

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-client</artifactId>
</dependency>
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-client-webflux</artifactId>
</dependency>
```

Example 3 (yaml):
```yaml
spring:
  ai:
    mcp:
      client:
        stdio:
          root-change-notification: true
          connections:
            server1:
              command: /path/to/server
              args:
                - --port=8080
                - --mode=production
              env:
                API_KEY: your-api-key
                DEBUG: "true"
```

Example 4 (yaml):
```yaml
spring:
  ai:
    mcp:
      client:
        stdio:
          servers-configuration: classpath:mcp-servers.json
```

---

## MCP Server Annotations :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/mcp/mcp-annotations-server.html

**Contents:**
- MCP Server Annotations
- Server Annotations
  - @McpTool
    - Basic Usage
    - Advanced Features
    - With Request Context
    - Dynamic Schema Support
    - Progress Tracking
  - @McpResource
    - Basic Usage

The MCP Server Annotations provide a declarative way to implement MCP server functionality using Java annotations. These annotations simplify the creation of tools, resources, prompts, and completion handlers.

The @McpTool annotation marks a method as an MCP tool implementation with automatic JSON schema generation.

Tools can access the request context for advanced operations:

Tools can accept CallToolRequest for runtime schema handling:

Tools can receive progress tokens for tracking long-running operations:

The @McpResource annotation provides access to resources via URI templates.

The @McpPrompt annotation generates prompt messages for AI interactions.

The @McpComplete annotation provides auto-completion functionality for prompts.

Use McpSyncRequestContext or McpAsyncRequestContext for a unified interface that works with both stateful and stateless operations:

For simple operations, you can omit context parameters entirely:

For stateless operations where you need minimal transport context:

Therefore methods using McpSyncRequestContext or McpAsyncRequestContext in stateless mode are ignored.

The MCP annotations framework automatically filters annotated methods based on the server type and method characteristics. This ensures that only appropriate methods are registered for each server configuration. A warning is logged for each filtered method to help with debugging.

Synchronous servers (configured with spring.ai.mcp.server.type=SYNC) use synchronous providers that:

Accept methods with non-reactive return types:

Primitive types (int, double, boolean)

Object types (String, Integer, custom POJOs)

MCP types (CallToolResult, ReadResourceResult, GetPromptResult, CompleteResult)

Collections (List<String>, Map<String, Object>)

Filter out methods with reactive return types:

Asynchronous servers (configured with spring.ai.mcp.server.type=ASYNC) use asynchronous providers that:

Accept methods with reactive return types:

Mono<T> (for single results)

Flux<T> (for streaming results)

Publisher<T> (generic reactive type)

Filter out methods with non-reactive return types:

Stateful servers support bidirectional communication and accept methods with:

Bidirectional context parameters:

McpSyncRequestContext (for sync operations)

McpAsyncRequestContext (for async operations)

McpSyncServerExchange (legacy, for sync operations)

McpAsyncServerExchange (legacy, for async operations)

Support for bidirectional operations:

roots() - Access root directories

elicit() - Request user input

sample() - Request LLM sampling

Stateless servers are optimized for simple request-response patterns and:

Filter out methods with bidirectional context parameters:

Methods with McpSyncRequestContext are skipped

Methods with McpAsyncRequestContext are skipped

Methods with McpSyncServerExchange are skipped

Methods with McpAsyncServerExchange are skipped

A warning is logged for each filtered method

McpTransportContext (lightweight stateless context)

No context parameter at all

Only regular @McpToolParam parameters

Do not support bidirectional operations:

roots() - Not available

elicit() - Not available

sample() - Not available

Non-reactive returns + bidirectional context

Reactive returns (Mono/Flux)

Reactive returns (Mono/Flux) + bidirectional context

Non-reactive returns + no bidirectional context

Reactive returns OR bidirectional context parameters

Reactive returns (Mono/Flux) + no bidirectional context

Non-reactive returns OR bidirectional context parameters

Keep methods aligned with your server type - use sync methods for sync servers, async for async servers

Separate stateful and stateless implementations into different classes for clarity

Check logs during startup for filtered method warnings

Use the right context - McpSyncRequestContext/McpAsyncRequestContext for stateful, McpTransportContext for stateless

Test both modes if you support both stateful and stateless deployments

All server annotations support asynchronous implementations using Reactor:

With Spring Boot auto-configuration, annotated beans are automatically detected and registered:

The auto-configuration will:

Scan for beans with MCP annotations

Create appropriate specifications

Register them with the MCP server

Handle both sync and async implementations based on configuration

Configure the server annotation scanner:

MCP Annotations Overview

MCP Server Boot Starter

**Examples:**

Example 1 (java):
```java
@Component
public class CalculatorTools {

    @McpTool(name = "add", description = "Add two numbers together")
    public int add(
            @McpToolParam(description = "First number", required = true) int a,
            @McpToolParam(description = "Second number", required = true) int b) {
        return a + b;
    }
}
```

Example 2 (java):
```java
@McpTool(name = "calculate-area",
         description = "Calculate the area of a rectangle",
         annotations = McpTool.McpAnnotations(
             title = "Rectangle Area Calculator",
             readOnlyHint = true,
             destructiveHint = false,
             idempotentHint = true
         ))
public AreaResult calculateRectangleArea(
        @McpToolParam(description = "Width", required = true) double width,
        @McpToolParam(description = "Height", required = true) double height) {

    return new AreaResult(width * height, "square units");
}
```

Example 3 (java):
```java
@McpTool(name = "process-data", description = "Process data with request context")
public String processData(
        McpSyncRequestContext context,
        @McpToolParam(description = "Data to process", required = true) String data) {

    // Send logging notification
    context.info("Processing data: " + data);

    // Send progress notification (using convenient method)
    context.progress(p -> p.progress(0.5).total(1.0).message("Processing..."));

    // Ping the client
    context.ping();

    return "Processed: " + data.toUpperCase();
}
```

Example 4 (java):
```java
@McpTool(name = "flexible-tool", description = "Process dynamic schema")
public CallToolResult processDynamic(CallToolRequest request) {
    Map<String, Object> args = request.arguments();

    // Process based on runtime schema
    String result = "Processed " + args.size() + " arguments dynamically";

    return CallToolResult.builder()
        .addTextContent(result)
        .build();
}
```

---

## MCP Security :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/mcp/mcp-security.html

**Contents:**
- MCP Security
- Overview
- MCP Server Security
  - Dependencies
  - OAuth 2.0 Configuration
    - Basic OAuth 2.0 Setup
    - Securing Tool Calls Only
  - API Key Authentication
  - Known Limitations
- MCP Client Security

The Spring AI MCP Security module provides comprehensive OAuth 2.0 and API key-based security support for Model Context Protocol implementations in Spring AI. This community-driven project enables developers to secure both MCP servers and clients with industry-standard authentication and authorization mechanisms.

The MCP Security module provides three main components:

MCP Server Security - OAuth 2.0 resource server and API key authentication for Spring AI MCP servers

MCP Client Security - OAuth 2.0 client support for Spring AI MCP clients

MCP Authorization Server - Enhanced Spring Authorization Server with MCP-specific features

The project enables developers to:

Secure MCP servers with OAuth 2.0 authentication and API key-based access

Configure MCP clients with OAuth 2.0 authorization flows

Set up authorization servers specifically designed for MCP workflows

Implement fine-grained access control for MCP tools and resources

The MCP Server Security module provides OAuth 2.0 resource server capabilities for Spring AI’s MCP servers. It also provides basic support for API-key based authentication.

Add the following dependencies to your project:

First, enable the MCP server in your application.properties:

Then, configure security using Spring Security’s standard APIs with the provided MCP configurer:

You can configure the server to secure only tool calls while leaving other MCP operations (like initialize and tools/list) public:

Then, secure your tool calls using the @PreAuthorize annotation with method security:

You can also access the current authentication directly from the tool method using SecurityContextHolder:

The MCP Server Security module also supports API key-based authentication. You need to provide your own implementation of ApiKeyEntityRepository for storing ApiKeyEntity objects.

A sample implementation is available with InMemoryApiKeyEntityRepository along with a default ApiKeyEntityImpl:

With this configuration, you can call your MCP server with a header X-API-key: api01.mycustomapikey.

The deprecated SSE transport is not supported. Use Streamable HTTP or stateless transport.

WebFlux-based servers are not supported.

Opaque tokens are not supported. Use JWT.

The MCP Client Security module provides OAuth 2.0 support for Spring AI’s MCP clients, supporting both HttpClient-based clients (from spring-ai-starter-mcp-client) and WebClient-based clients (from spring-ai-starter-mcp-client-webflux).

Three OAuth 2.0 flows are available for obtaining tokens:

Authorization Code Flow - For user-level permissions when every MCP request is made within the context of a user request

Client Credentials Flow - For machine-to-machine use cases where no human is in the loop

Hybrid Flow - Combines both flows for scenarios where some operations (like initialize or tools/list) happen without a user present, but tool calls require user-level permissions

For all flows, activate Spring Security’s OAuth2 client support in your application.properties:

Then, create a configuration class activating OAuth2 client capabilities:

When using spring-ai-starter-mcp-client, configure a McpSyncHttpClientRequestCustomizer bean:

Available customizers:

OAuth2AuthorizationCodeSyncHttpRequestCustomizer - For authorization code flow

OAuth2ClientCredentialsSyncHttpRequestCustomizer - For client credentials flow

OAuth2HybridSyncHttpRequestCustomizer - For hybrid flow

When using spring-ai-starter-mcp-client-webflux, configure a WebClient.Builder with an MCP ExchangeFilterFunction:

Available filter functions:

McpOAuth2AuthorizationCodeExchangeFilterFunction - For authorization code flow

McpOAuth2ClientCredentialsExchangeFilterFunction - For client credentials flow

McpOAuth2HybridExchangeFilterFunction - For hybrid flow

Spring AI’s autoconfiguration initializes MCP clients at startup, which can cause issues with user-based authentication. To avoid this:

Disable Spring AI’s @Tool autoconfiguration by publishing an empty ToolCallbackResolver bean:

Configure MCP clients programmatically instead of using Spring Boot properties. For HttpClient-based clients:

For WebClient-based clients:

Then add the client to your chat client:

Spring WebFlux servers are not supported.

Spring AI autoconfiguration initializes MCP clients at app start, requiring workarounds for user-based authentication.

Unlike the server module, the client implementation supports the SSE transport with both HttpClient and WebClient.

The MCP Authorization Server module enhances Spring Security’s OAuth 2.0 Authorization Server with features relevant to the MCP authorization spec, such as Dynamic Client Registration and Resource Indicators.

Configure the authorization server in your application.yml:

Then activate the authorization server capabilities with a security filter chain:

Spring WebFlux servers are not supported.

Every client supports ALL resource identifiers.

The samples directory contains working examples for all modules in this project, including integration tests.

With mcp-server-security and a supporting mcp-authorization-server, you can integrate with:

MCP Authorization Specification

MCP Security GitHub Repository

MCP Authorization Specification

Spring Security OAuth 2.0 Resource Server

Spring Security OAuth 2.0 Client

Spring Authorization Server

**Examples:**

Example 1 (xml):
```xml
<dependencies>
    <dependency>
        <groupId>org.springaicommunity</groupId>
        <artifactId>mcp-server-security</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>

    <!-- OPTIONAL: For OAuth2 support -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-oauth2-resource-server</artifactId>
    </dependency>
</dependencies>
```

Example 2 (unknown):
```unknown
implementation 'org.springaicommunity:mcp-server-security'
implementation 'org.springframework.boot:spring-boot-starter-security'

// OPTIONAL: For OAuth2 support
implementation 'org.springframework.boot:spring-boot-starter-oauth2-resource-server'
```

Example 3 (markdown):
```markdown
spring.ai.mcp.server.name=my-cool-mcp-server
# Supported protocols: STREAMABLE, STATELESS
spring.ai.mcp.server.protocol=STREAMABLE
```

Example 4 (java):
```java
@Configuration
@EnableWebSecurity
class McpServerConfiguration {

    @Value("${spring.security.oauth2.resourceserver.jwt.issuer-uri}")
    private String issuerUrl;

    @Bean
    SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http
                // Enforce authentication with token on EVERY request
                .authorizeHttpRequests(auth -> auth.anyRequest().authenticated())
                // Configure OAuth2 on the MCP server
                .with(
                        McpServerOAuth2Configurer.mcpServerOAuth2(),
                        (mcpAuthorization) -> {
                            // REQUIRED: the issuerURI
                            mcpAuthorization.authorizationServer(issuerUrl);
                            // OPTIONAL: enforce the `aud` claim in the JWT token.
                            // Not all authorization servers support resource indicators,
                            // so it may be absent. Defaults to `false`.
                            // See RFC 8707 Resource Indicators for OAuth 2.0
                            // https://www.rfc-editor.org/rfc/rfc8707.html
                            mcpAuthorization.validateAudienceClaim(true);
                        }
                )
                .build();
    }
}
```

---

## Untitled :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/mcp/mcp-stdio-sse-server-boot-starter-docs.html

**Contents:**
- STDIO and SSE MCP Servers
  - STDIO MCP Server
  - SSE WebMVC Server
  - SSE WebFlux Server
- Configuration Properties
  - Common Properties
  - MCP Annotations Properties
  - SSE Properties
- Features and Capabilities
  - Tools

The STDIO and SSE MCP Servers support multiple transport mechanisms, each with its dedicated starter.

Full MCP Server feature support with STDIO server transport.

Suitable for command-line and desktop tools

No additional web dependencies required

Configuration of basic server components

Handling of tool, resource, and prompt specifications

Management of server capabilities and change notifications

Support for both sync and async server implementations

Full MCP Server feature support with SSE (Server-Sent Events) server transport based on Spring MVC and an optional STDIO transport.

HTTP-based transport using Spring MVC (WebMvcSseServerTransportProvider)

Automatically configured SSE endpoints

Optional STDIO transport (enabled by setting spring.ai.mcp.server.stdio=true)

Includes spring-boot-starter-web and mcp-spring-webmvc dependencies

Full MCP Server feature support with SSE (Server-Sent Events) server transport based on Spring WebFlux and an optional STDIO transport.

The starter activates the McpWebFluxServerAutoConfiguration and McpServerAutoConfiguration auto-configurations to provide:

Reactive transport using Spring WebFlux (WebFluxSseServerTransportProvider)

Automatically configured reactive SSE endpoints

Optional STDIO transport (enabled by setting spring.ai.mcp.server.stdio=true)

Includes spring-boot-starter-webflux and mcp-spring-webflux dependencies

Due to Spring Boot’s default behavior, when both org.springframework.web.servlet.DispatcherServlet and org.springframework.web.reactive.DispatcherHandler are present on the classpath, Spring Boot will prioritize DispatcherServlet. As a result, if your project uses spring-boot-starter-web, it is recommended to use spring-ai-starter-mcp-server-webmvc instead of spring-ai-starter-mcp-server-webflux.

All Common properties are prefixed with spring.ai.mcp.server:

Enable/disable the MCP server

tool-callback-converter

Enable/disable the conversion of Spring AI ToolCallbacks into MCP Tool specs

Enable/disable STDIO transport

Server name for identification

Optional instructions to provide guidance to the client on how to interact with this server

Server type (SYNC/ASYNC)

capabilities.resource

Enable/disable resource capabilities

Enable/disable tool capabilities

Enable/disable prompt capabilities

capabilities.completion

Enable/disable completion capabilities

resource-change-notification

Enable resource change notifications

prompt-change-notification

Enable prompt change notifications

tool-change-notification

Enable tool change notifications

tool-response-mime-type

Optional response MIME type per tool name. For example, spring.ai.mcp.server.tool-response-mime-type.generateImage=image/png will associate the image/png MIME type with the generateImage() tool name

Duration to wait for server responses before timing out requests. Applies to all requests made through the client, including tool calls, resource access, and prompt operations

MCP Server Annotations provide a declarative way to implement MCP server handlers using Java annotations.

The server mcp-annotations properties are prefixed with spring.ai.mcp.server.annotation-scanner:

Enable/disable the MCP server annotations auto-scanning

All SSE properties are prefixed with spring.ai.mcp.server:

Custom SSE message endpoint path for web transport to be used by the client to send messages

Custom SSE endpoint path for web transport

Optional URL prefix. For example, base-url=/api/v1 means that the client should access the SSE endpoint at /api/v1 + sse-endpoint and the message endpoint is /api/v1 + sse-message-endpoint

Connection keep-alive interval

The MCP Server Boot Starter allows servers to expose tools, resources, and prompts to clients. It automatically converts custom capability handlers registered as Spring beans to sync/async specifications based on the server type:

Allows servers to expose tools that can be invoked by language models. The MCP Server Boot Starter provides:

Change notification support

Spring AI Tools are automatically converted to sync/async specifications based on the server type

Automatic tool specification through Spring beans:

or using the low-level API:

The auto-configuration will automatically detect and register all tool callbacks from:

Individual ToolCallback beans

Lists of ToolCallback beans

ToolCallbackProvider beans

Tools are de-duplicated by name, with the first occurrence of each tool name being used.

The ToolContext is supported, allowing contextual information to be passed to tool calls. It contains an McpSyncServerExchange instance under the exchange key, accessible via McpToolUtils.getMcpExchange(toolContext). See this example demonstrating exchange.loggingNotification(…​) and exchange.createMessage(…​).

Provides a standardized way for servers to expose resources to clients.

Static and dynamic resource specifications

Optional change notifications

Support for resource templates

Automatic conversion between sync/async resource specifications

Automatic resource specification through Spring beans:

Provides a standardized way for servers to expose prompt templates to clients.

Change notification support

Automatic conversion between sync/async prompt specifications

Automatic prompt specification through Spring beans:

Provides a standardized way for servers to expose completion capabilities to clients.

Support for both sync and async completion specifications

Automatic registration through Spring beans:

Provides a standardized way for servers to send structured log messages to clients. From within the tool, resource, prompt or completion call handler use the provided McpSyncServerExchange/McpAsyncServerExchange exchange object to send logging messages:

On the MCP client you can register logging consumers to handle these messages:

Provides a standardized way for servers to send progress updates to clients. From within the tool, resource, prompt or completion call handler use the provided McpSyncServerExchange/McpAsyncServerExchange exchange object to send progress notifications:

The Mcp Client can receive progress notifications and update its UI accordingly. For this it needs to register a progress consumer.

When roots change, clients that support listChanged send a root change notification.

Support for monitoring root changes

Automatic conversion to async consumers for reactive applications

Optional registration through Spring beans

Ping mechanism for the server to verify that its clients are still alive. From within the tool, resource, prompt or completion call handler use the provided McpSyncServerExchange/McpAsyncServerExchange exchange object to send ping messages:

Server can optionally, periodically issue pings to connected clients to verify connection health.

By default, keep-alive is disabled. To enable keep-alive, set the keep-alive-interval property in your configuration:

The auto-configuration will automatically register the tool callbacks as MCP tools. You can have multiple beans producing ToolCallbacks, and the auto-configuration will merge them.

Weather Server (WebFlux) - Spring AI MCP Server Boot Starter with WebFlux transport

Weather Server (STDIO) - Spring AI MCP Server Boot Starter with STDIO transport

Weather Server Manual Configuration - Spring AI MCP Server Boot Starter that doesn’t use auto-configuration but uses the Java SDK to configure the server manually

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server</artifactId>
</dependency>
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webmvc</artifactId>
</dependency>
```

Example 3 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webflux</artifactId>
</dependency>
```

Example 4 (java):
```java
@Bean
public ToolCallbackProvider myTools(...) {
    List<ToolCallback> tools = ...
    return ToolCallbackProvider.from(tools);
}
```

---

## MCP Utilities :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/mcp/mcp-helpers.html

**Contents:**
- MCP Utilities
- ToolCallback Utility
  - Tool Callback Adapter
  - Tool Callback Providers
- McpToolUtils
  - ToolCallbacks to ToolSpecifications
  - MCP Clients to ToolCallbacks
- Native Image Support

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The MCP utilities provide foundational support for integrating Model Context Protocol with Spring AI applications. These utilities enable seamless communication between Spring AI’s tool system and MCP servers, supporting both synchronous and asynchronous operations. They are typically used for programmatic MCP Client and Server configuration and interaction. For a more streamlined configuration, consider using the boot starters.

Adapts MCP tools to Spring AI’s tool interface with both synchronous and asynchronous execution support.

Discovers and provides MCP tools from MCP clients.

For multiple clients:

For dynamic selection of a subset of clients

For multiple clients:

Converting Spring AI tool callbacks to MCP tool specifications:

then you can use the McpServer.SyncSpecification to register the tool specifications:

then you can use the McpServer.AsyncSpecification to register the tool specifications:

Getting tool callbacks from MCP clients

The McpHints class provides GraalVM native image hints for MCP schema classes. This class automatically registers all necessary reflection hints for MCP schema classes when building native images.

**Examples:**

Example 1 (java):
```java
McpSyncClient mcpClient = // obtain MCP client
Tool mcpTool = // obtain MCP tool definition
ToolCallback callback = new SyncMcpToolCallback(mcpClient, mcpTool);

// Use the tool through Spring AI's interfaces
ToolDefinition definition = callback.getToolDefinition();
String result = callback.call("{\"param\": \"value\"}");
```

Example 2 (java):
```java
McpAsyncClient mcpClient = // obtain MCP client
Tool mcpTool = // obtain MCP tool definition
ToolCallback callback = new AsyncMcpToolCallback(mcpClient, mcpTool);

// Use the tool through Spring AI's interfaces
ToolDefinition definition = callback.getToolDefinition();
String result = callback.call("{\"param\": \"value\"}");
```

Example 3 (java):
```java
McpSyncClient mcpClient = // obtain MCP client
ToolCallbackProvider provider = new SyncMcpToolCallbackProvider(mcpClient);

// Get all available tools
ToolCallback[] tools = provider.getToolCallbacks();
```

Example 4 (java):
```java
List<McpSyncClient> clients = // obtain list of clients
List<ToolCallback> callbacks = SyncMcpToolCallbackProvider.syncToolCallbacks(clients);
```

---
