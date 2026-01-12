# Springai - Vector Databases

**Pages:** 71

---

## Weaviate :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/weaviate.html

**Contents:**
- Weaviate
- Prerequisites
- Dependencies
- Configuration
  - Option 1: Using Spring Expression Language (SpEL)
  - Option 2: Accessing Environment Variables Programmatically
- Auto-configuration
- Manual Configuration
- Metadata filtering
- Run Weaviate in Docker

This section walks you through setting up the Weaviate VectorStore to store document embeddings and perform similarity searches.

Weaviate is an open-source vector database that allows you to store data objects and vector embeddings from your favorite ML-models and scale seamlessly into billions of data objects. It provides tools to store document embeddings, content, and metadata and to search through those embeddings, including metadata filtering.

A running Weaviate instance. The following options are available:

Weaviate Cloud Service (requires account creation and API key)

If required, an API key for the EmbeddingModel to generate the embeddings stored by the WeaviateVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add the Weaviate Vector Store dependency to your project:

or to your Gradle build.gradle build file.

To connect to Weaviate and use the WeaviateVectorStore, you need to provide access details for your instance. Configuration can be provided via Spring Boot’s application.properties:

If you prefer to use environment variables for sensitive information like API keys, you have multiple options:

You can use custom environment variable names and reference them in your application configuration:

Alternatively, you can access environment variables in your Java code:

Spring AI provides Spring Boot auto-configuration for the Weaviate Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Here is an example of the required bean:

Now you can auto-wire the WeaviateVectorStore as a vector store in your application.

Instead of using Spring Boot auto-configuration, you can manually configure the WeaviateVectorStore using the builder pattern:

You can leverage the generic, portable metadata filters with Weaviate store as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Weaviate GraphQL filter format:

To quickly get started with a local Weaviate instance, you can run it in Docker:

This starts a Weaviate instance accessible at localhost:8080.

You can use the following properties in your Spring Boot configuration to customize the Weaviate vector store.

spring.ai.vectorstore.weaviate.host

The host of the Weaviate server

spring.ai.vectorstore.weaviate.scheme

spring.ai.vectorstore.weaviate.api-key

The API key for authentication

spring.ai.vectorstore.weaviate.object-class

The class name for storing documents.

spring.ai.vectorstore.weaviate.content-field-name

The field name for content

spring.ai.vectorstore.weaviate.meta-field-prefix

The field prefix for metadata

spring.ai.vectorstore.weaviate.consistency-level

Desired tradeoff between consistency and speed

spring.ai.vectorstore.weaviate.filter-field

Configures metadata fields that can be used in filters. Format: spring.ai.vectorstore.weaviate.filter-field.<field-name>=<field-type>

The Weaviate Vector Store implementation provides access to the underlying native Weaviate client (WeaviateClient) through the getNativeClient() method:

The native client gives you access to Weaviate-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-weaviate-store</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-weaviate-store'
}
```

Example 3 (markdown):
```markdown
spring.ai.vectorstore.weaviate.host=<host_of_your_weaviate_instance>
spring.ai.vectorstore.weaviate.scheme=<http_or_https>
spring.ai.vectorstore.weaviate.api-key=<your_api_key>
# API key if needed, e.g. OpenAI
spring.ai.openai.api-key=<api-key>
```

Example 4 (yaml):
```yaml
# In application.yml
spring:
  ai:
    vectorstore:
      weaviate:
        host: ${WEAVIATE_HOST}
        scheme: ${WEAVIATE_SCHEME}
        api-key: ${WEAVIATE_API_KEY}
    openai:
      api-key: ${OPENAI_API_KEY}
```

---

## Understanding Vectors :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/understand-vectordbs.html

**Contents:**
- Understanding Vectors
- Similarity

Vectors have dimensionality and a direction. For example, the following image depicts a two-dimensional vector in the cartesian coordinate system pictured as an arrow.

The head of the vector is at the point . The x coordinate value is and the y coordinate value is . The coordinates are also referred to as the components of the vector.

Several mathematical formulas can be used to determine if two vectors are similar. One of the most intuitive to visualize and understand is cosine similarity. Consider the following images that show three sets of graphs:

The vectors and are considered similar, when they are pointing close to each other, as in the first diagram. The vectors are considered unrelated when pointing perpendicular to each other and opposite when they point away from each other.

The angle between them, , is a good measure of their similarity. How can the angle be computed?

We are all familiar with the Pythagorean Theorem.

What about when the angle between a and b is not 90 degrees?

Enter the Law of cosines.

The following image shows this approach as a vector diagram:

The magnitude of this vector is defined in terms of its components as:

The dot product between two vectors and is defined in terms of its components as:

Rewriting the Law of Cosines with vector magnitudes and dot products gives the following:

Replacing with gives the following:

Expanding this out gives us the formula for Cosine Similarity.

This formula works for dimensions higher than 2 or 3, though it is hard to visualize. However, it can be visualized to some extent. It is common for vectors in AI/ML applications to have hundreds or even thousands of dimensions.

The similarity function in higher dimensions using the components of the vector is shown below. It expands the two-dimensional definitions of Magnitude and Dot Product given previously to N dimensions by using Summation mathematical syntax.

This is the key formula used in the simple implementation of a vector store and can be found in the SimpleVectorStore implementation.

---

## Neo4j :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/neo4j.html

**Contents:**
- Neo4j
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Accessing the Native Client

This section walks you through setting up Neo4jVectorStore to store document embeddings and perform similarity searches.

Neo4j is an open-source NoSQL graph database. It is a fully transactional database (ACID) that stores data structured as graphs consisting of nodes, connected by relationships. Inspired by the structure of the real world, it allows for high query performance on complex data while remaining intuitive and simple for the developer.

The Neo4j’s Vector Search allows users to query vector embeddings from large datasets. An embedding is a numerical representation of a data object, such as text, image, audio, or document. Embeddings can be stored on Node properties and can be queried with the db.index.vector.queryNodes() function. Those indexes are powered by Lucene using a Hierarchical Navigable Small World Graph (HNSW) to perform a k approximate nearest neighbors (k-ANN) query over the vector fields.

A running Neo4j (5.15+) instance. The following options are available:

Neo4j Server instance

If required, an API key for the EmbeddingModel to generate the embeddings stored by the Neo4jVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Neo4j Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Please have a look at the list of Configuration Properties for the vector store to learn about the default values and configuration options.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the Neo4jVectorStore as a vector store in your application.

To connect to Neo4j and use the Neo4jVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

The Spring Boot properties starting with spring.neo4j.* are used to configure the Neo4j client:

URI for connecting to the Neo4j instance

neo4j://localhost:7687

spring.neo4j.authentication.username

Username for authentication with Neo4j

spring.neo4j.authentication.password

Password for authentication with Neo4j

Properties starting with spring.ai.vectorstore.neo4j.* are used to configure the Neo4jVectorStore:

spring.ai.vectorstore.neo4j.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.neo4j.database-name

The name of the Neo4j database to use

spring.ai.vectorstore.neo4j.index-name

The name of the index to store the vectors

spring-ai-document-index

spring.ai.vectorstore.neo4j.embedding-dimension

The number of dimensions in the vector

spring.ai.vectorstore.neo4j.distance-type

The distance function to use

spring.ai.vectorstore.neo4j.label

The label used for document nodes

spring.ai.vectorstore.neo4j.embedding-property

The property name used to store embeddings

The following distance functions are available:

cosine - Default, suitable for most use cases. Measures cosine similarity between vectors.

euclidean - Euclidean distance between vectors. Lower values indicate higher similarity.

Instead of using the Spring Boot auto-configuration, you can manually configure the Neo4j vector store. For this you need to add the spring-ai-neo4j-store to your project:

or to your Gradle build.gradle build file.

Create a Neo4j Driver bean. Read the Neo4j Documentation for more in-depth information about the configuration of a custom driver.

Then create the Neo4jVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with Neo4j store as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Neo4j filter format:

The Neo4j Vector Store implementation provides access to the underlying native Neo4j client (Driver) through the getNativeClient() method:

The native client gives you access to Neo4j-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-neo4j</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-neo4j'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Neo4j
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  neo4j:
    uri: <neo4j instance URI>
    authentication:
      username: <neo4j username>
      password: <neo4j password>
  ai:
    vectorstore:
      neo4j:
        initialize-schema: true
        database-name: neo4j
        index-name: custom-index
        embedding-dimension: 1536
        distance-type: cosine
```

---

## Untitled :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/hanadb-provision-a-trial-account.html

**Contents:**
- Provision SAP HANA Cloud trial account

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Below are the steps to provision SAP Hana Database using a trial account

Let’s start with creating a temporary email for registration purposes

Go to sap.com and navigate to products → Trials and Demos

Click Advanced Trials

Click Start your free 90-day trial

Paste the temporary email id that we created in the first step, and click Next

We fill in our details and click Submit

It’s time to check the inbox of our temporary email account

Notice that there is an email received in our temporary email account

Open the email and click to activate the trial account

It will prompt to create a password. Provide a password and click Submit

The trial account is now created. Click to start the trial

Provide your phone number and click Continue

We receive an OTP on the phone number. Provide the code and click continue

Select the region as US East (VA) - AWS

The SAP BTP trial account is ready. Click Go to your Trial account

Click the Trial sub-account

Open Instances and Subscriptions

It’s time to create a subscription. Click the Create button

While creating a subscription, Select service as SAP Hana Cloud and Plan as tools and click Create

Notice that SAP Hana Cloud subscription is now created. Click Users on the left panel

Select the username (temporary email that we supplied earlier) and click Assign Role Collection

Search hana and select all the 3 role collections that gets displayed. Click Assign Role Collection

Our user now has all the 3 role collections. Click Instances and Subscriptions

Now, click SAP Hana Cloud application under subscriptions

There are no instances yet. Let’s click Create Instance

Select Type as SAP HANA Cloud, SAP HANA Database. Click Next Step

Provide Instance Name, Description, password for DBADMIN administrator. Select the latest version 2024.2 (QRC 1/2024). Click Next Step

Keep everything as default. Click Next Step

Select Allow all IP addresses and click Next Step

Click Review and Create

Click Create Instance

Notice that the provisioning of SAP Hana Database instance has started. It takes some time to provision - please be patient.

Once the instance is provisioned (status is displayed as Running) we can get the datasource url (SQL Endpoint) by clicking the instance and selecting Connections

We navigate to SAP Hana Database Explorer by click the …​

Provide the administrator credentials and click OK

Open SQL console and create the table CRICKET_WORLD_CUP using the following DDL statement:

Navigate to hana_dev_db → Catalog → Tables to find our table CRICKET_WORLD_CUP

Right-click on the table and click Open Data

Notice that the table data is now displayed. There are now rows as we didn’t create any embeddings yet.

Next steps: SAP Hana Vector Engine

---

## Apache Cassandra Vector Store :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/apache-cassandra.html

**Contents:**
- Apache Cassandra Vector Store
- What is Apache Cassandra?
- What is JVector?
- Prerequisites
- Dependencies
- Configuration Properties
- Usage
  - Basic Usage
  - Advanced Configuration
  - Connection Configuration

For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up CassandraVectorStore to store document embeddings and perform similarity searches.

Apache Cassandra® is a true open source distributed database renowned for linear scalability, proven fault-tolerance and low latency, making it the perfect platform for mission-critical transactional data.

Its Vector Similarity Search (VSS) is based on the JVector library that ensures best-in-class performance and relevancy.

A vector search in Apache Cassandra is done as simply as:

More docs on this can be read here.

This Spring AI Vector Store is designed to work for both brand-new RAG applications and be able to be retrofitted on top of existing data and tables.

The store can also be used for non-RAG use-cases in an existing database, e.g. semantic searches, geo-proximity searches, etc.

The store will automatically create, or enhance, the schema as needed according to its configuration. If you don’t want the schema modifications, configure the store with initializeSchema.

When using spring-boot-autoconfigure initializeSchema defaults to false, per Spring Boot standards, and you must opt-in to schema creation/modifications by setting …​initialize-schema=true in the application.properties file.

JVector is a pure Java embedded vector search engine.

It stands out from other HNSW Vector Similarity Search implementations by being:

Algorithmic-fast. JVector uses state of the art graph algorithms inspired by DiskANN and related research that offer high recall and low latency.

Implementation-fast. JVector uses the Panama SIMD API to accelerate index build and queries.

Memory efficient. JVector compresses vectors using product quantization so they can stay in memory during searches.

Disk-aware. JVector’s disk layout is designed to do the minimum necessary iops at query time.

Concurrent. Index builds scale linearly to at least 32 threads. Double the threads, half the build time.

Incremental. Query your index as you build it. No delay between adding a vector and being able to find it in search results.

Easy to embed. API designed for easy embedding, by people using it in production.

A EmbeddingModel instance to compute the document embeddings. This is usually configured as a Spring Bean. Several options are available:

Transformers Embedding - computes the embedding in your local environment. The default is via ONNX and the all-MiniLM-L6-v2 Sentence Transformers. This just works.

If you want to use OpenAI’s Embeddings - uses the OpenAI embedding endpoint. You need to create an account at OpenAI Signup and generate the api-key token at API Keys.

There are many more choices, see Embeddings API docs.

An Apache Cassandra instance, from version 5.0-beta1

For a managed offering Astra DB offers a healthy free tier offering.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add these dependencies to your project:

For just the Cassandra Vector Store:

Or, for everything you need in a RAG application (using the default ONNX Embedding Model):

You can use the following properties in your Spring Boot configuration to customize the Apache Cassandra vector store.

spring.ai.vectorstore.cassandra.keyspace

spring.ai.vectorstore.cassandra.table

spring.ai.vectorstore.cassandra.initialize-schema

spring.ai.vectorstore.cassandra.index-name

spring.ai.vectorstore.cassandra.content-column-name

spring.ai.vectorstore.cassandra.embedding-column-name

spring.ai.vectorstore.cassandra.fixed-thread-pool-executor-size

Create a CassandraVectorStore instance as a Spring Bean:

Once you have the vector store instance, you can add documents and perform searches:

For more complex use cases, you can configure additional settings in your Spring Bean:

There are two ways to configure the connection to Cassandra:

Using an injected CqlSession (recommended):

Using connection details directly in the builder:

You can leverage the generic, portable metadata filters with the CassandraVectorStore. For metadata columns to be searchable they must be either primary keys or SAI indexed. To make non-primary-key columns indexed, configure the metadata column with the SchemaColumnTags.INDEXED.

For example, you can use either the text expression language:

or programmatically using the expression DSL:

The portable filter expressions get automatically converted into CQL queries.

The following example demonstrates how to use the store on an existing schema. Here we use the schema from the github.com/datastax-labs/colbert-wikipedia-data project which comes with the full wikipedia dataset ready vectorized for you.

First, create the schema in the Cassandra database:

Then configure the store using the builder pattern:

To load the full wikipedia dataset:

Download simplewiki-sstable.tar from s.apache.org/simplewiki-sstable-tar (this will take a while, the file is tens of GBs)

If you have existing data in this table, check the tarball’s files don’t clobber existing sstables when doing the tar.

An alternative to nodetool import is to just restart Cassandra.

If there are any failures in the indexes they will be rebuilt automatically.

The Cassandra Vector Store implementation provides access to the underlying native Cassandra client (CqlSession) through the getNativeClient() method:

The native client gives you access to Cassandra-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (sql):
```sql
SELECT content FROM table ORDER BY content_vector ANN OF query_embedding;
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-cassandra-store</artifactId>
</dependency>
```

Example 3 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-cassandra</artifactId>
</dependency>
```

Example 4 (java):
```java
@Bean
public VectorStore vectorStore(CqlSession session, EmbeddingModel embeddingModel) {
    return CassandraVectorStore.builder(embeddingModel)
        .session(session)
        .keyspace("my_keyspace")
        .table("my_vectors")
        .build();
}
```

---

## Neo4j :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/neo4j.html

**Contents:**
- Neo4j
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Accessing the Native Client

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up Neo4jVectorStore to store document embeddings and perform similarity searches.

Neo4j is an open-source NoSQL graph database. It is a fully transactional database (ACID) that stores data structured as graphs consisting of nodes, connected by relationships. Inspired by the structure of the real world, it allows for high query performance on complex data while remaining intuitive and simple for the developer.

The Neo4j’s Vector Search allows users to query vector embeddings from large datasets. An embedding is a numerical representation of a data object, such as text, image, audio, or document. Embeddings can be stored on Node properties and can be queried with the db.index.vector.queryNodes() function. Those indexes are powered by Lucene using a Hierarchical Navigable Small World Graph (HNSW) to perform a k approximate nearest neighbors (k-ANN) query over the vector fields.

A running Neo4j (5.15+) instance. The following options are available:

Neo4j Server instance

If required, an API key for the EmbeddingModel to generate the embeddings stored by the Neo4jVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Neo4j Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Please have a look at the list of Configuration Properties for the vector store to learn about the default values and configuration options.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the Neo4jVectorStore as a vector store in your application.

To connect to Neo4j and use the Neo4jVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

The Spring Boot properties starting with spring.neo4j.* are used to configure the Neo4j client:

URI for connecting to the Neo4j instance

neo4j://localhost:7687

spring.neo4j.authentication.username

Username for authentication with Neo4j

spring.neo4j.authentication.password

Password for authentication with Neo4j

Properties starting with spring.ai.vectorstore.neo4j.* are used to configure the Neo4jVectorStore:

spring.ai.vectorstore.neo4j.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.neo4j.database-name

The name of the Neo4j database to use

spring.ai.vectorstore.neo4j.index-name

The name of the index to store the vectors

spring-ai-document-index

spring.ai.vectorstore.neo4j.embedding-dimension

The number of dimensions in the vector

spring.ai.vectorstore.neo4j.distance-type

The distance function to use

spring.ai.vectorstore.neo4j.label

The label used for document nodes

spring.ai.vectorstore.neo4j.embedding-property

The property name used to store embeddings

The following distance functions are available:

cosine - Default, suitable for most use cases. Measures cosine similarity between vectors.

euclidean - Euclidean distance between vectors. Lower values indicate higher similarity.

Instead of using the Spring Boot auto-configuration, you can manually configure the Neo4j vector store. For this you need to add the spring-ai-neo4j-store to your project:

or to your Gradle build.gradle build file.

Create a Neo4j Driver bean. Read the Neo4j Documentation for more in-depth information about the configuration of a custom driver.

Then create the Neo4jVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with Neo4j store as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Neo4j filter format:

The Neo4j Vector Store implementation provides access to the underlying native Neo4j client (Driver) through the getNativeClient() method:

The native client gives you access to Neo4j-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-neo4j</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-neo4j'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Neo4j
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  neo4j:
    uri: <neo4j instance URI>
    authentication:
      username: <neo4j username>
      password: <neo4j password>
  ai:
    vectorstore:
      neo4j:
        initialize-schema: true
        database-name: neo4j
        index-name: custom-index
        embedding-dimension: 1536
        distance-type: cosine
```

---

## Qdrant :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/qdrant.html

**Contents:**
- Qdrant
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Accessing the Native Client

This section walks you through setting up the Qdrant VectorStore to store document embeddings and perform similarity searches.

Qdrant is an open-source, high-performance vector search engine/database. It uses HNSW (Hierarchical Navigable Small World) algorithm for efficient k-NN search operations and provides advanced filtering capabilities for metadata-based queries.

Qdrant Instance: Set up a Qdrant instance by following the installation instructions in the Qdrant documentation.

If required, an API key for the EmbeddingModel to generate the embeddings stored by the QdrantVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Qdrant Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the builder or by setting …​initialize-schema=true in the application.properties file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the QdrantVectorStore as a vector store in your application.

To connect to Qdrant and use the QdrantVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.qdrant.* are used to configure the QdrantVectorStore:

spring.ai.vectorstore.qdrant.host

The host of the Qdrant server

spring.ai.vectorstore.qdrant.port

The gRPC port of the Qdrant server

spring.ai.vectorstore.qdrant.api-key

The API key to use for authentication

spring.ai.vectorstore.qdrant.collection-name

The name of the collection to use

spring.ai.vectorstore.qdrant.use-tls

Whether to use TLS(HTTPS)

spring.ai.vectorstore.qdrant.initialize-schema

Whether to initialize the schema

Instead of using the Spring Boot auto-configuration, you can manually configure the Qdrant vector store. For this you need to add the spring-ai-qdrant-store to your project:

or to your Gradle build.gradle build file.

Create a Qdrant client bean:

Then create the QdrantVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with Qdrant store as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

The Qdrant Vector Store implementation provides access to the underlying native Qdrant client (QdrantClient) through the getNativeClient() method:

The native client gives you access to Qdrant-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-qdrant</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-qdrant'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Qdrant
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  ai:
    vectorstore:
      qdrant:
        host: <qdrant host>
        port: <qdrant grpc port>
        api-key: <qdrant api key>
        collection-name: <collection name>
        use-tls: false
        initialize-schema: true
```

---

## OpenSearch :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/opensearch.html

**Contents:**
- OpenSearch
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Accessing the Native Client

This section walks you through setting up OpenSearchVectorStore to store document embeddings and perform similarity searches.

OpenSearch is an open-source search and analytics engine originally forked from Elasticsearch, distributed under the Apache License 2.0. It enhances AI application development by simplifying the integration and management of AI-generated assets. OpenSearch supports vector, lexical, and hybrid search capabilities, leveraging advanced vector database functionalities to facilitate low-latency queries and similarity searches as detailed on the vector database page.

The OpenSearch k-NN functionality allows users to query vector embeddings from large datasets. An embedding is a numerical representation of a data object, such as text, image, audio, or document. Embeddings can be stored in the index and queried using various similarity functions.

A running OpenSearch instance. The following options are available:

Self-Managed OpenSearch

Amazon OpenSearch Service

If required, an API key for the EmbeddingModel to generate the embeddings stored by the OpenSearchVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the OpenSearch Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the OpenSearchVectorStore as a vector store in your application:

To connect to OpenSearch and use the OpenSearchVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.opensearch.* are used to configure the OpenSearchVectorStore:

spring.ai.vectorstore.opensearch.uris

URIs of the OpenSearch cluster endpoints

spring.ai.vectorstore.opensearch.username

Username for accessing the OpenSearch cluster

spring.ai.vectorstore.opensearch.password

Password for the specified username

spring.ai.vectorstore.opensearch.index-name

Name of the index to store vectors

spring-ai-document-index

spring.ai.vectorstore.opensearch.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.opensearch.similarity-function

The similarity function to use (cosinesimil, l1, l2, linf, innerproduct)

spring.ai.vectorstore.opensearch.use-approximate-knn

Whether to use approximate k-NN for faster searches. If true, uses HNSW-based approximate search. If false, uses exact brute-force k-NN. See Approximate k-NN and Exact k-NN

spring.ai.vectorstore.opensearch.dimensions

Number of dimensions for vector embeddings. Used when creating index mapping for approximate k-NN. If not set, uses the embedding model’s dimensions.

spring.ai.vectorstore.opensearch.mapping-json

Custom JSON mapping for the index. Overrides default mapping generation.

spring.ai.vectorstore.opensearch.read-timeout

Time to wait for response from the opposite endpoint. 0 - infinity.

spring.ai.vectorstore.opensearch.connect-timeout

Time to wait until connection established. 0 - infinity.

spring.ai.vectorstore.opensearch.path-prefix

Path prefix for OpenSearch API endpoints. Useful when OpenSearch is behind a reverse proxy with a non-root path.

spring.ai.vectorstore.opensearch.ssl-bundle

Name of the SSL Bundle to use in case of SSL connection

spring.ai.vectorstore.opensearch.aws.host

Hostname of the OpenSearch instance

spring.ai.vectorstore.opensearch.aws.service-name

spring.ai.vectorstore.opensearch.aws.access-key

spring.ai.vectorstore.opensearch.aws.secret-key

spring.ai.vectorstore.opensearch.aws.region

You can control whether the AWS-specific OpenSearch auto-configuration is enabled using the spring.ai.vectorstore.opensearch.aws.enabled property.

If this property is set to false, the non-AWS OpenSearch configuration is activated, even if AWS SDK classes are present on the classpath. This allows you to use self-managed or third-party OpenSearch clusters in environments where AWS SDKs are present for other services.

If AWS SDK classes are not present, the non-AWS configuration is always used.

If AWS SDK classes are present and the property is not set or set to true, the AWS-specific configuration is used by default.

This fallback logic ensures that users have explicit control over the type of OpenSearch integration, preventing accidental activation of AWS-specific logic when not desired.

The path-prefix property allows you to specify a custom path prefix when OpenSearch is running behind a reverse proxy that uses a non-root path. For example, if your OpenSearch instance is accessible at example.com/opensearch/ instead of example.com/, you would set path-prefix: /opensearch.

The following similarity functions are available:

cosinesimil - Default, suitable for most use cases. Measures cosine similarity between vectors.

l1 - Manhattan distance between vectors.

l2 - Euclidean distance between vectors.

linf - Chebyshev distance between vectors.

Instead of using the Spring Boot auto-configuration, you can manually configure the OpenSearch vector store. For this you need to add the spring-ai-opensearch-store to your project:

or to your Gradle build.gradle build file:

Create an OpenSearch client bean:

Then create the OpenSearchVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with OpenSearch as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary OpenSearch filter format:

The OpenSearch Vector Store implementation provides access to the underlying native OpenSearch client (OpenSearchClient) through the getNativeClient() method:

The native client gives you access to OpenSearch-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-opensearch</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-opensearch'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to OpenSearch
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  ai:
    vectorstore:
      opensearch:
        uris: <opensearch instance URIs>
        username: <opensearch username>
        password: <opensearch password>
        index-name: spring-ai-document-index
        initialize-schema: true
        similarity-function: cosinesimil
        read-timeout: <time to wait for response>
        connect-timeout: <time to wait until connection established>
        path-prefix: <custom path prefix>
        ssl-bundle: <name of SSL bundle>
        aws:  # Only for Amazon OpenSearch Service
          host: <aws opensearch host>
          service-name: <aws service name>
          access-key: <aws access key>
          secret-key: <aws secret key>
          region: <aws region>
```

---

## Typesense :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/typesense.html

**Contents:**
- Typesense
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Accessing the Native Client

For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up TypesenseVectorStore to store document embeddings and perform similarity searches.

Typesense is an open source typo tolerant search engine that is optimized for instant sub-50ms searches while providing an intuitive developer experience. It provides vector search capabilities that allow you to store and query high-dimensional vectors alongside your regular search data.

A running Typesense instance. The following options are available:

Typesense Cloud (recommended)

Docker image typesense/typesense:latest

If required, an API key for the EmbeddingModel to generate the embeddings stored by the TypesenseVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Typesense Vector Store. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

The vector store implementation can initialize the requisite schema for you but you must opt-in by setting …​initialize-schema=true in the application.properties file.

Additionally you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the TypesenseVectorStore as a vector store in your application:

To connect to Typesense and use the TypesenseVectorStore you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.typesense.* are used to configure the TypesenseVectorStore:

spring.ai.vectorstore.typesense.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.typesense.collection-name

The name of the collection to store vectors

spring.ai.vectorstore.typesense.embedding-dimension

The number of dimensions in the vector

spring.ai.vectorstore.typesense.client.protocol

spring.ai.vectorstore.typesense.client.host

spring.ai.vectorstore.typesense.client.port

spring.ai.vectorstore.typesense.client.api-key

Instead of using the Spring Boot auto-configuration you can manually configure the Typesense vector store. For this you need to add the spring-ai-typesense-store to your project:

or to your Gradle build.gradle build file.

Create a Typesense Client bean:

Then create the TypesenseVectorStore bean using the builder pattern:

You can leverage the generic portable metadata filters with Typesense store as well.

For example you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example this portable filter expression:

is converted into the proprietary Typesense filter format:

If you are not retrieving the documents in the expected order or the search results are not as expected, check the embedding model you are using.

Embedding models can have a significant impact on the search results (i.e. make sure if your data is in Spanish to use a Spanish or multilingual embedding model).

The Typesense Vector Store implementation provides access to the underlying native Typesense client (Client) through the getNativeClient() method:

The native client gives you access to Typesense-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-typesense</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-typesense'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Typesense
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  ai:
    vectorstore:
      typesense:
        initialize-schema: true
        collection-name: vector_store
        embedding-dimension: 1536
        client:
          protocol: http
          host: localhost
          port: 8108
          api-key: xyz
```

---

## Understanding Vectors :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/understand-vectordbs.html

**Contents:**
- Understanding Vectors
- Similarity

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Vectors have dimensionality and a direction. For example, the following image depicts a two-dimensional vector in the cartesian coordinate system pictured as an arrow.

The head of the vector is at the point . The x coordinate value is and the y coordinate value is . The coordinates are also referred to as the components of the vector.

Several mathematical formulas can be used to determine if two vectors are similar. One of the most intuitive to visualize and understand is cosine similarity. Consider the following images that show three sets of graphs:

The vectors and are considered similar, when they are pointing close to each other, as in the first diagram. The vectors are considered unrelated when pointing perpendicular to each other and opposite when they point away from each other.

The angle between them, , is a good measure of their similarity. How can the angle be computed?

We are all familiar with the Pythagorean Theorem.

What about when the angle between a and b is not 90 degrees?

Enter the Law of cosines.

The following image shows this approach as a vector diagram:

The magnitude of this vector is defined in terms of its components as:

The dot product between two vectors and is defined in terms of its components as:

Rewriting the Law of Cosines with vector magnitudes and dot products gives the following:

Replacing with gives the following:

Expanding this out gives us the formula for Cosine Similarity.

This formula works for dimensions higher than 2 or 3, though it is hard to visualize. However, it can be visualized to some extent. It is common for vectors in AI/ML applications to have hundreds or even thousands of dimensions.

The similarity function in higher dimensions using the components of the vector is shown below. It expands the two-dimensional definitions of Magnitude and Dot Product given previously to N dimensions by using Summation mathematical syntax.

This is the key formula used in the simple implementation of a vector store and can be found in the SimpleVectorStore implementation.

---

## Apache Cassandra Vector Store :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/apache-cassandra.html

**Contents:**
- Apache Cassandra Vector Store
- What is Apache Cassandra?
- What is JVector?
- Prerequisites
- Dependencies
- Configuration Properties
- Usage
  - Basic Usage
  - Advanced Configuration
  - Connection Configuration

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up CassandraVectorStore to store document embeddings and perform similarity searches.

Apache Cassandra® is a true open source distributed database renowned for linear scalability, proven fault-tolerance and low latency, making it the perfect platform for mission-critical transactional data.

Its Vector Similarity Search (VSS) is based on the JVector library that ensures best-in-class performance and relevancy.

A vector search in Apache Cassandra is done as simply as:

More docs on this can be read here.

This Spring AI Vector Store is designed to work for both brand-new RAG applications and be able to be retrofitted on top of existing data and tables.

The store can also be used for non-RAG use-cases in an existing database, e.g. semantic searches, geo-proximity searches, etc.

The store will automatically create, or enhance, the schema as needed according to its configuration. If you don’t want the schema modifications, configure the store with initializeSchema.

When using spring-boot-autoconfigure initializeSchema defaults to false, per Spring Boot standards, and you must opt-in to schema creation/modifications by setting …​initialize-schema=true in the application.properties file.

JVector is a pure Java embedded vector search engine.

It stands out from other HNSW Vector Similarity Search implementations by being:

Algorithmic-fast. JVector uses state of the art graph algorithms inspired by DiskANN and related research that offer high recall and low latency.

Implementation-fast. JVector uses the Panama SIMD API to accelerate index build and queries.

Memory efficient. JVector compresses vectors using product quantization so they can stay in memory during searches.

Disk-aware. JVector’s disk layout is designed to do the minimum necessary iops at query time.

Concurrent. Index builds scale linearly to at least 32 threads. Double the threads, half the build time.

Incremental. Query your index as you build it. No delay between adding a vector and being able to find it in search results.

Easy to embed. API designed for easy embedding, by people using it in production.

A EmbeddingModel instance to compute the document embeddings. This is usually configured as a Spring Bean. Several options are available:

Transformers Embedding - computes the embedding in your local environment. The default is via ONNX and the all-MiniLM-L6-v2 Sentence Transformers. This just works.

If you want to use OpenAI’s Embeddings - uses the OpenAI embedding endpoint. You need to create an account at OpenAI Signup and generate the api-key token at API Keys.

There are many more choices, see Embeddings API docs.

An Apache Cassandra instance, from version 5.0-beta1

For a managed offering Astra DB offers a healthy free tier offering.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add these dependencies to your project:

For just the Cassandra Vector Store:

Or, for everything you need in a RAG application (using the default ONNX Embedding Model):

You can use the following properties in your Spring Boot configuration to customize the Apache Cassandra vector store.

spring.ai.vectorstore.cassandra.keyspace

spring.ai.vectorstore.cassandra.table

spring.ai.vectorstore.cassandra.initialize-schema

spring.ai.vectorstore.cassandra.index-name

spring.ai.vectorstore.cassandra.content-column-name

spring.ai.vectorstore.cassandra.embedding-column-name

spring.ai.vectorstore.cassandra.fixed-thread-pool-executor-size

Create a CassandraVectorStore instance as a Spring Bean:

Once you have the vector store instance, you can add documents and perform searches:

For more complex use cases, you can configure additional settings in your Spring Bean:

There are two ways to configure the connection to Cassandra:

Using an injected CqlSession (recommended):

Using connection details directly in the builder:

You can leverage the generic, portable metadata filters with the CassandraVectorStore. For metadata columns to be searchable they must be either primary keys or SAI indexed. To make non-primary-key columns indexed, configure the metadata column with the SchemaColumnTags.INDEXED.

For example, you can use either the text expression language:

or programmatically using the expression DSL:

The portable filter expressions get automatically converted into CQL queries.

The following example demonstrates how to use the store on an existing schema. Here we use the schema from the github.com/datastax-labs/colbert-wikipedia-data project which comes with the full wikipedia dataset ready vectorized for you.

First, create the schema in the Cassandra database:

Then configure the store using the builder pattern:

To load the full wikipedia dataset:

Download simplewiki-sstable.tar from s.apache.org/simplewiki-sstable-tar (this will take a while, the file is tens of GBs)

If you have existing data in this table, check the tarball’s files don’t clobber existing sstables when doing the tar.

An alternative to nodetool import is to just restart Cassandra.

If there are any failures in the indexes they will be rebuilt automatically.

The Cassandra Vector Store implementation provides access to the underlying native Cassandra client (CqlSession) through the getNativeClient() method:

The native client gives you access to Cassandra-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (sql):
```sql
SELECT content FROM table ORDER BY content_vector ANN OF query_embedding;
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-cassandra-store</artifactId>
</dependency>
```

Example 3 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-cassandra</artifactId>
</dependency>
```

Example 4 (java):
```java
@Bean
public VectorStore vectorStore(CqlSession session, EmbeddingModel embeddingModel) {
    return CassandraVectorStore.builder(embeddingModel)
        .session(session)
        .keyspace("my_keyspace")
        .table("my_vectors")
        .build();
}
```

---

## Chroma :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/chroma.html

**Contents:**
- Chroma
- Prerequisites
- Auto-configuration
  - Configuration properties
  - Chroma Cloud Configuration
- Metadata filtering
- Manual Configuration
  - Sample Code
  - Run Chroma Locally

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section will walk you through setting up the Chroma VectorStore to store document embeddings and perform similarity searches.

Chroma is the open-source embedding database. It gives you the tools to store document embeddings, content, and metadata and to search through those embeddings, including metadata filtering.

Access to ChromaDB. Compatible with Chroma Cloud, or setup local ChromaDB in the appendix shows how to set up a DB locally with a Docker container.

For Chroma Cloud: You’ll need your API key, tenant name, and database name from your Chroma Cloud dashboard.

For local ChromaDB: No additional configuration required beyond starting the container.

EmbeddingModel instance to compute the document embeddings. Several options are available:

If required, an API key for the EmbeddingModel to generate the embeddings stored by the ChromaVectorStore.

On startup, the ChromaVectorStore creates the required collection if one is not provisioned already.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Chroma Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Here is an example of the needed bean:

To connect to Chroma you need to provide access details for your instance. A simple configuration can either be provided via Spring Boot’s application.properties,

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Now you can auto-wire the Chroma Vector Store in your application and use it

You can use the following properties in your Spring Boot configuration to customize the vector store.

spring.ai.vectorstore.chroma.client.host

Server connection host

spring.ai.vectorstore.chroma.client.port

Server connection port

spring.ai.vectorstore.chroma.client.key-token

Access token (if configured)

spring.ai.vectorstore.chroma.client.username

Access username (if configured)

spring.ai.vectorstore.chroma.client.password

Access password (if configured)

spring.ai.vectorstore.chroma.tenant-name

Tenant (required for Chroma Cloud)

spring.ai.vectorstore.chroma.database-name

Database name (required for Chroma Cloud)

spring.ai.vectorstore.chroma.collection-name

spring.ai.vectorstore.chroma.initialize-schema

Whether to initialize the required schema (creates tenant/database/collection if they don’t exist)

For ChromaDB secured with Static API Token Authentication use the ChromaApi#withKeyToken(<Your Token Credentials>) method to set your credentials. Check the ChromaWhereIT for an example.

For ChromaDB secured with Basic Authentication use the ChromaApi#withBasicAuth(<your user>, <your password>) method to set your credentials. Check the BasicAuthChromaWhereIT for an example.

For Chroma Cloud, you need to provide the tenant and database names from your Chroma Cloud instance. Here’s an example configuration:

For Chroma Cloud: - The host should be api.trychroma.com - The port should be 443 (HTTPS) - You must provide your API key via key-token - The tenant and database names must match your Chroma Cloud configuration - Set initialize-schema=true to automatically create the collection if it doesn’t exist (it won’t recreate existing tenant/database)

You can leverage the generic, portable metadata filters with ChromaVector store as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Chroma format

If you prefer to configure the Chroma Vector Store manually, you can do so by creating a ChromaVectorStore bean in your Spring Boot application.

Add these dependencies to your project: * Chroma VectorStore.

OpenAI: Required for calculating embeddings. You can use any other embedding model implementation.

Create a RestClient.Builder instance with proper ChromaDB authorization configurations and Use it to create a ChromaApi instance:

Integrate with OpenAI’s embeddings by adding the Spring Boot OpenAI starter to your project. This provides you with an implementation of the Embeddings client:

In your main code, create some documents:

Add the documents to your vector store:

And finally, retrieve documents similar to a query:

If all goes well, you should retrieve the document containing the text "Spring AI rocks!!".

Starts a chroma store at localhost:8000/api/v1

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-chroma</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-chroma'
}
```

Example 3 (java):
```java
@Bean
public EmbeddingModel embeddingModel() {
    // Can be any other EmbeddingModel implementation.
    return new OpenAiEmbeddingModel(OpenAiApi.builder().apiKey(System.getenv("OPENAI_API_KEY")).build());
}
```

Example 4 (jsx):
```jsx
# Chroma Vector Store connection properties
spring.ai.vectorstore.chroma.client.host=<your Chroma instance host>  // for Chroma Cloud: api.trychroma.com
spring.ai.vectorstore.chroma.client.port=<your Chroma instance port> // for Chroma Cloud: 443
spring.ai.vectorstore.chroma.client.key-token=<your access token (if configure)> // for Chroma Cloud: use the API key
spring.ai.vectorstore.chroma.client.username=<your username (if configure)>
spring.ai.vectorstore.chroma.client.password=<your password (if configure)>

# Chroma Vector Store tenant and database properties (required for Chroma Cloud)
spring.ai.vectorstore.chroma.tenant-name=<your tenant name> // default: SpringAiTenant
spring.ai.vectorstore.chroma.database-name=<your database name> // default: SpringAiDatabase

# Chroma Vector Store collection properties
spring.ai.vectorstore.chroma.initialize-schema=<true or false>
spring.ai.vectorstore.chroma.collection-name=<your collection name>

# Chroma Vector Store configuration properties

# OpenAI API key if the OpenAI auto-configuration is used.
spring.ai.openai.api.key=<OpenAI Api-key>
```

---

## MariaDB Vector Store :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/mariadb.html

**Contents:**
- MariaDB Vector Store
- Prerequisites
- Auto-Configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Similarity Scores
  - Score Calculation
  - Accessing Scores
  - Search Results Ordering

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up MariaDBVectorStore to store document embeddings and perform similarity searches.

MariaDB Vector is part of MariaDB 11.7 and enables storing and searching over machine learning-generated embeddings. It provides efficient vector similarity search capabilities using vector indexes, supporting both cosine similarity and Euclidean distance metrics.

A running MariaDB (11.7+) instance. The following options are available:

If required, an API key for the EmbeddingModel to generate the embeddings stored by the MariaDBVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the MariaDB Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the required schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

For example, to use the OpenAI EmbeddingModel, add the following dependency:

Now you can auto-wire the MariaDBVectorStore in your application:

To connect to MariaDB and use the MariaDBVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.mariadb.* are used to configure the MariaDBVectorStore:

spring.ai.vectorstore.mariadb.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.mariadb.distance-type

Search distance type. Use COSINE (default) or EUCLIDEAN. If vectors are normalized to length 1, you can use EUCLIDEAN for best performance.

spring.ai.vectorstore.mariadb.dimensions

Embeddings dimension. If not specified explicitly, will retrieve dimensions from the provided EmbeddingModel.

spring.ai.vectorstore.mariadb.remove-existing-vector-store-table

Deletes the existing vector store table on startup.

spring.ai.vectorstore.mariadb.schema-name

Vector store schema name

spring.ai.vectorstore.mariadb.table-name

Vector store table name

spring.ai.vectorstore.mariadb.schema-validation

Enables schema and table name validation to ensure they are valid and existing objects.

Instead of using the Spring Boot auto-configuration, you can manually configure the MariaDB vector store. For this you need to add the following dependencies to your project:

Then create the MariaDBVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with MariaDB Vector store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

The MariaDB Vector Store automatically calculates similarity scores for documents returned from similarity searches. These scores provide a normalized measure of how closely each document matches your search query.

Similarity scores are calculated using the formula score = 1.0 - distance, where:

Score: A value between 0.0 and 1.0, where 1.0 indicates perfect similarity and 0.0 indicates no similarity

Distance: The raw distance value calculated using the configured distance type (COSINE or EUCLIDEAN)

This means that documents with smaller distances (more similar) will have higher scores, making the results more intuitive to interpret.

You can access the similarity score for each document through the getScore() method:

Search results are automatically ordered by similarity score in descending order (highest score first). This ensures that the most relevant documents appear at the top of your results.

In addition to the similarity score, the raw distance value is still available in the document metadata:

When using similarity thresholds in your search requests, specify the threshold as a score value (0.0 to 1.0) rather than a distance:

This makes threshold values consistent and intuitive - higher values mean more restrictive searches that only return highly similar documents.

The MariaDB Vector Store implementation provides access to the underlying native JDBC client (JdbcTemplate) through the getNativeClient() method:

The native client gives you access to MariaDB-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-mariadb</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-mariadb'
}
```

Example 3 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

Example 4 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to MariaDB
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

---

## Azure Cosmos DB :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/azure-cosmos-db.html

**Contents:**
- Azure Cosmos DB
- What is Azure Cosmos DB?
- What is DiskANN?
- Setting up Azure Cosmos DB Vector Store with Auto Configuration
- Auto Configuration
- Configuration Properties
- Complex Searches with Filters
- Setting up Azure Cosmos DB Vector Store without Auto Configuration
- Manual Dependency Setup
- Accessing the Native Client

This section walks you through setting up CosmosDBVectorStore to store document embeddings and perform similarity searches.

Azure Cosmos DB is Microsoft’s globally distributed cloud-native database service designed for mission-critical applications. It offers high availability, low latency, and the ability to scale horizontally to meet modern application demands. It was built from the ground up with global distribution, fine-grained multi-tenancy, and horizontal scalability at its core. It is a foundational service in Azure, used by most of Microsoft’s mission critical applications at global scale, including Teams, Skype, Xbox Live, Office 365, Bing, Azure Active Directory, Azure Portal, Microsoft Store, and many others. It is also used by thousands of external customers including OpenAI for ChatGPT and other mission-critical AI applications that require elastic scale, turnkey global distribution, and low latency and high availability across the planet.

DiskANN (Disk-based Approximate Nearest Neighbor Search) is an innovative technology used in Azure Cosmos DB to enhance the performance of vector searches. It enables efficient and scalable similarity searches across high-dimensional data by indexing embeddings stored in Cosmos DB.

DiskANN provides the following benefits:

Efficiency: By utilizing disk-based structures, DiskANN significantly reduces the time required to find nearest neighbors compared to traditional methods.

Scalability: It can handle large datasets that exceed memory capacity, making it suitable for various applications, including machine learning and AI-driven solutions.

Low Latency: DiskANN minimizes latency during search operations, ensuring that applications can retrieve results quickly even with substantial data volumes.

In the context of Spring AI for Azure Cosmos DB, vector searches will create and leverage DiskANN indexes to ensure optimal performance for similarity queries.

The following code demonstrates how to set up the CosmosDBVectorStore with auto-configuration:

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add the following dependency to your Maven project:

The following configuration properties are available for the Cosmos DB vector store:

spring.ai.vectorstore.cosmosdb.databaseName

The name of the Cosmos DB database to use.

spring.ai.vectorstore.cosmosdb.containerName

The name of the Cosmos DB container to use.

spring.ai.vectorstore.cosmosdb.partitionKeyPath

The path for the partition key.

spring.ai.vectorstore.cosmosdb.metadataFields

Comma-separated list of metadata fields.

spring.ai.vectorstore.cosmosdb.vectorStoreThroughput

The throughput for the vector store.

spring.ai.vectorstore.cosmosdb.vectorDimensions

The number of dimensions for the vectors.

spring.ai.vectorstore.cosmosdb.endpoint

The endpoint for the Cosmos DB.

spring.ai.vectorstore.cosmosdb.key

The key for the Cosmos DB (if key is not present, [DefaultAzureCredential](learn.microsoft.com/azure/developer/java/sdk/authentication/credential-chains#defaultazurecredential-overview) will be used).

You can perform more complex searches using filters in the Cosmos DB vector store. Below is a sample demonstrating how to use filters in your search queries.

The following code demonstrates how to set up the CosmosDBVectorStore without relying on auto-configuration. [DefaultAzureCredential](learn.microsoft.com/azure/developer/java/sdk/authentication/credential-chains#defaultazurecredential-overview) is recommended for authentication to Azure Cosmos DB.

This configuration shows all the available builder options:

databaseName: The name of your Cosmos DB database

containerName: The name of your container within the database

partitionKeyPath: The path for the partition key (e.g., "/id")

metadataFields: List of metadata fields that will be used for filtering

vectorStoreThroughput: The throughput (RU/s) for the vector store container

vectorDimensions: The number of dimensions for your vectors (should match your embedding model)

batchingStrategy: Strategy for batching document operations (optional)

Add the following dependency in your Maven project:

The Azure Cosmos DB Vector Store implementation provides access to the underlying native Azure Cosmos DB client (CosmosClient) through the getNativeClient() method:

The native client gives you access to Azure Cosmos DB-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (java):
```java
package com.example.demo;

import io.micrometer.observation.ObservationRegistry;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.ai.document.Document;
import org.springframework.ai.vectorstore.SearchRequest;
import org.springframework.ai.vectorstore.VectorStore;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Lazy;

import java.util.List;
import java.util.Map;
import java.util.UUID;

import static org.assertj.core.api.Assertions.assertThat;

@SpringBootApplication
@EnableAutoConfiguration
public class DemoApplication implements CommandLineRunner {

    private static final Logger log = LoggerFactory.getLogger(DemoApplication.class);

    @Lazy
    @Autowired
    private VectorStore vectorStore;

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

    @Override
    public void run(String... args) throws Exception {
        Document document1 = new Document(UUID.randomUUID().toString(), "Sample content1", Map.of("key1", "value1"));
        Document document2 = new Document(UUID.randomUUID().toString(), "Sample content2", Map.of("key2", "value2"));
		this.vectorStore.add(List.of(document1, document2));
        List<Document> results = this.vectorStore.similaritySearch(SearchRequest.builder().query("Sample content").topK(1).build());

        log.info("Search results: {}", results);

        // Remove the documents from the vector store
		this.vectorStore.delete(List.of(document1.getId(), document2.getId()));
    }

    @Bean
    public ObservationRegistry observationRegistry() {
        return ObservationRegistry.create();
    }
}
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-azure-cosmos-db</artifactId>
</dependency>
```

Example 3 (java):
```java
Map<String, Object> metadata1 = new HashMap<>();
metadata1.put("country", "UK");
metadata1.put("year", 2021);
metadata1.put("city", "London");

Map<String, Object> metadata2 = new HashMap<>();
metadata2.put("country", "NL");
metadata2.put("year", 2022);
metadata2.put("city", "Amsterdam");

Document document1 = new Document("1", "A document about the UK", this.metadata1);
Document document2 = new Document("2", "A document about the Netherlands", this.metadata2);

vectorStore.add(List.of(document1, document2));

FilterExpressionBuilder builder = new FilterExpressionBuilder();
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("The World")
    .topK(10)
    .filterExpression((this.builder.in("country", "UK", "NL")).build()).build());
```

Example 4 (java):
```java
@Bean
public VectorStore vectorStore(ObservationRegistry observationRegistry) {
    // Create the Cosmos DB client
    CosmosAsyncClient cosmosClient = new CosmosClientBuilder()
            .endpoint(System.getenv("COSMOSDB_AI_ENDPOINT"))
            .credential(new DefaultAzureCredentialBuilder().build())
            .userAgentSuffix("SpringAI-CDBNoSQL-VectorStore")
            .gatewayMode()
            .buildAsyncClient();

    // Create and configure the vector store
    return CosmosDBVectorStore.builder(cosmosClient, embeddingModel)
            .databaseName("test-database")
            .containerName("test-container")
            // Configure metadata fields for filtering
            .metadataFields(List.of("country", "year", "city"))
            // Set the partition key path (optional)
            .partitionKeyPath("/id")
            // Configure performance settings
            .vectorStoreThroughput(1000)
            .vectorDimensions(1536)  // Match your embedding model's dimensions
            // Add custom batching strategy (optional)
            .batchingStrategy(new TokenCountBatchingStrategy())
            // Add observation registry for metrics
            .observationRegistry(observationRegistry)
            .build();
}

@Bean
public EmbeddingModel embeddingModel() {
    return new TransformersEmbeddingModel();
}
```

---

## Pinecone :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/pinecone.html

**Contents:**
- Pinecone
- Prerequisites
- Auto-configuration
  - Configuration properties
- Metadata filtering
- Manual Configuration
  - Sample Code
- Accessing the Native Client

This section walks you through setting up the Pinecone VectorStore to store document embeddings and perform similarity searches.

Pinecone is a popular cloud-based vector database, which allows you to store and search vectors efficiently.

Pinecone Account: Before you start, sign up for a Pinecone account.

Pinecone Project: Once registered, generate an API key and create and index. You’ll need these details for configuration.

EmbeddingModel instance to compute the document embeddings. Several options are available:

If required, an API key for the EmbeddingModel to generate the embeddings stored by the PineconeVectorStore.

To set up PineconeVectorStore, gather the following details from your Pinecone account:

This information is available to you in the Pinecone UI portal. The namespace support is not available in the Pinecone free tier.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Pinecone Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Here is an example of the needed bean:

To connect to Pinecone you need to provide access details for your instance. A simple configuration can either be provided via Spring Boot’s application.properties,

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Now you can Auto-wire the Pinecone Vector Store in your application and use it

You can use the following properties in your Spring Boot configuration to customize the Pinecone vector store.

spring.ai.vectorstore.pinecone.api-key

spring.ai.vectorstore.pinecone.index-name

spring.ai.vectorstore.pinecone.namespace

spring.ai.vectorstore.pinecone.content-field-name

Pinecone metadata field name used to store the original text content.

spring.ai.vectorstore.pinecone.distance-metadata-field-name

Pinecone metadata field name used to store the computed distance.

spring.ai.vectorstore.pinecone.server-side-timeout

You can leverage the generic, portable metadata filters with the Pinecone store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

If you prefer to configure PineconeVectorStore manually, you can do so by using the PineconeVectorStore#Builder.

Add these dependencies to your project:

OpenAI: Required for calculating embeddings.

To configure Pinecone in your application, you can use the following setup:

In your main code, create some documents:

Add the documents to Pinecone:

And finally, retrieve documents similar to a query:

If all goes well, you should retrieve the document containing the text "Spring AI rocks!!".

The Pinecone Vector Store implementation provides access to the underlying native Pinecone client (PineconeConnection) through the getNativeClient() method:

The native client gives you access to Pinecone-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-pinecone</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-pinecone'
}
```

Example 3 (java):
```java
@Bean
public EmbeddingModel embeddingModel() {
    // Can be any other EmbeddingModel implementation.
    return new OpenAiEmbeddingModel(new OpenAiApi(System.getenv("OPENAI_API_KEY")));
}
```

Example 4 (jsx):
```jsx
spring.ai.vectorstore.pinecone.apiKey=<your api key>
spring.ai.vectorstore.pinecone.index-name=<your index name>

# API key if needed, e.g. OpenAI
spring.ai.openai.api.key=<api-key>
```

---

## Oracle Database 23ai - AI Vector Search :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/oracle.html

**Contents:**
- Oracle Database 23ai - AI Vector Search
- Auto-Configuration
  - Configuration properties
- Metadata filtering
- Manual Configuration
- Run Oracle Database 23ai locally
- Accessing the Native Client

For the latest snapshot version, please use Spring AI 1.1.2!

The AI Vector Search capabilities of the Oracle Database 23ai (23.4+) are available as a Spring AI VectorStore to help you to store document embeddings and perform similarity searches. Of course, all other features are also available.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Start by adding the Oracle Vector Store boot starter dependency to your project:

or to your Gradle build.gradle build file.

If you need this vector store to initialize the schema for you then you’ll need to pass true for the initializeSchema boolean parameter in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

The Vector Store, also requires an EmbeddingModel instance to calculate embeddings for the documents. You can pick one of the available EmbeddingModel Implementations.

For example to use the OpenAI EmbeddingModel add the following dependency to your project:

or to your Gradle build.gradle build file.

To connect to and configure the OracleVectorStore, you need to provide access details for your database. A simple configuration can either be provided via Spring Boot’s application.yml

Now you can Auto-wire the OracleVectorStore in your application and use it:

You can use the following properties in your Spring Boot configuration to customize the OracleVectorStore.

spring.ai.vectorstore.oracle.index-type

Nearest neighbor search index type. Options are NONE - exact nearest neighbor search, IVF - Inverted Flat File index. It has faster build times and uses less memory than HNSW, but has lower query performance (in terms of speed-recall tradeoff). HNSW - creates a multilayer graph. It has slower build times and uses more memory than IVF, but has better query performance (in terms of speed-recall tradeoff).

spring.ai.vectorstore.oracle.distance-type

Search distance type among COSINE (default), DOT, EUCLIDEAN, EUCLIDEAN_SQUARED, and MANHATTAN.

NOTE: If vectors are normalized, you can use DOT or COSINE for best performance.

spring.ai.vectorstore.oracle.forced-normalization

Allows enabling vector normalization (if true) before insertion and for similarity search.

CAUTION: Setting this to true is a requirement to allow for search request similarity threshold.

NOTE: If vectors are normalized, you can use DOT or COSINE for best performance.

spring.ai.vectorstore.oracle.dimensions

Embeddings dimension. If not specified explicitly the OracleVectorStore will allow the maximum: 65535. Dimensions are set to the embedding column on table creation. If you change the dimensions your would have to re-create the table as well.

spring.ai.vectorstore.oracle.remove-existing-vector-store-table

Drops the existing table on start up.

spring.ai.vectorstore.oracle.initialize-schema

Whether to initialize the required schema.

spring.ai.vectorstore.oracle.search-accuracy

Denote the requested accuracy target in the presence of index. Disabled by default. You need to provide an integer in the range [1,100] to override the default index accuracy (95). Using lower accuracy provides approximate similarity search trading off speed versus accuracy.

-1 (DEFAULT_SEARCH_ACCURACY)

You can leverage the generic, portable metadata filters with the OracleVectorStore.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

Instead of using the Spring Boot auto-configuration, you can manually configure the OracleVectorStore. For this you need to add the Oracle JDBC driver and JdbcTemplate auto-configuration dependencies to your project:

To configure the OracleVectorStore in your application, you can use the following setup:

You can then connect to the database using:

The Oracle Vector Store implementation provides access to the underlying native Oracle client (OracleConnection) through the getNativeClient() method:

The native client gives you access to Oracle-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-vector-store-oracle</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-oracle'
}
```

Example 3 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

Example 4 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-openai'
}
```

---

## Oracle Database 23ai - AI Vector Search :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/oracle.html

**Contents:**
- Oracle Database 23ai - AI Vector Search
- Auto-Configuration
  - Configuration properties
- Metadata filtering
- Manual Configuration
- Run Oracle Database 23ai locally
- Accessing the Native Client

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

The AI Vector Search capabilities of the Oracle Database 23ai (23.4+) are available as a Spring AI VectorStore to help you to store document embeddings and perform similarity searches. Of course, all other features are also available.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Start by adding the Oracle Vector Store boot starter dependency to your project:

or to your Gradle build.gradle build file.

If you need this vector store to initialize the schema for you then you’ll need to pass true for the initializeSchema boolean parameter in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

The Vector Store, also requires an EmbeddingModel instance to calculate embeddings for the documents. You can pick one of the available EmbeddingModel Implementations.

For example to use the OpenAI EmbeddingModel add the following dependency to your project:

or to your Gradle build.gradle build file.

To connect to and configure the OracleVectorStore, you need to provide access details for your database. A simple configuration can either be provided via Spring Boot’s application.yml

Now you can Auto-wire the OracleVectorStore in your application and use it:

You can use the following properties in your Spring Boot configuration to customize the OracleVectorStore.

spring.ai.vectorstore.oracle.index-type

Nearest neighbor search index type. Options are NONE - exact nearest neighbor search, IVF - Inverted Flat File index. It has faster build times and uses less memory than HNSW, but has lower query performance (in terms of speed-recall tradeoff). HNSW - creates a multilayer graph. It has slower build times and uses more memory than IVF, but has better query performance (in terms of speed-recall tradeoff).

spring.ai.vectorstore.oracle.distance-type

Search distance type among COSINE (default), DOT, EUCLIDEAN, EUCLIDEAN_SQUARED, and MANHATTAN.

NOTE: If vectors are normalized, you can use DOT or COSINE for best performance.

spring.ai.vectorstore.oracle.forced-normalization

Allows enabling vector normalization (if true) before insertion and for similarity search.

CAUTION: Setting this to true is a requirement to allow for search request similarity threshold.

NOTE: If vectors are normalized, you can use DOT or COSINE for best performance.

spring.ai.vectorstore.oracle.dimensions

Embeddings dimension. If not specified explicitly the OracleVectorStore will allow the maximum: 65535. Dimensions are set to the embedding column on table creation. If you change the dimensions your would have to re-create the table as well.

spring.ai.vectorstore.oracle.remove-existing-vector-store-table

Drops the existing table on start up.

spring.ai.vectorstore.oracle.initialize-schema

Whether to initialize the required schema.

spring.ai.vectorstore.oracle.search-accuracy

Denote the requested accuracy target in the presence of index. Disabled by default. You need to provide an integer in the range [1,100] to override the default index accuracy (95). Using lower accuracy provides approximate similarity search trading off speed versus accuracy.

-1 (DEFAULT_SEARCH_ACCURACY)

You can leverage the generic, portable metadata filters with the OracleVectorStore.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

Instead of using the Spring Boot auto-configuration, you can manually configure the OracleVectorStore. For this you need to add the Oracle JDBC driver and JdbcTemplate auto-configuration dependencies to your project:

To configure the OracleVectorStore in your application, you can use the following setup:

You can then connect to the database using:

The Oracle Vector Store implementation provides access to the underlying native Oracle client (OracleConnection) through the getNativeClient() method:

The native client gives you access to Oracle-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-vector-store-oracle</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-oracle'
}
```

Example 3 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

Example 4 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-openai'
}
```

---

## MariaDB Vector Store :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/mariadb.html

**Contents:**
- MariaDB Vector Store
- Prerequisites
- Auto-Configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Similarity Scores
  - Score Calculation
  - Accessing Scores
  - Search Results Ordering

This section walks you through setting up MariaDBVectorStore to store document embeddings and perform similarity searches.

MariaDB Vector is part of MariaDB 11.7 and enables storing and searching over machine learning-generated embeddings. It provides efficient vector similarity search capabilities using vector indexes, supporting both cosine similarity and Euclidean distance metrics.

A running MariaDB (11.7+) instance. The following options are available:

If required, an API key for the EmbeddingModel to generate the embeddings stored by the MariaDBVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the MariaDB Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the required schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

For example, to use the OpenAI EmbeddingModel, add the following dependency:

Now you can auto-wire the MariaDBVectorStore in your application:

To connect to MariaDB and use the MariaDBVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.mariadb.* are used to configure the MariaDBVectorStore:

spring.ai.vectorstore.mariadb.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.mariadb.distance-type

Search distance type. Use COSINE (default) or EUCLIDEAN. If vectors are normalized to length 1, you can use EUCLIDEAN for best performance.

spring.ai.vectorstore.mariadb.dimensions

Embeddings dimension. If not specified explicitly, will retrieve dimensions from the provided EmbeddingModel.

spring.ai.vectorstore.mariadb.remove-existing-vector-store-table

Deletes the existing vector store table on startup.

spring.ai.vectorstore.mariadb.schema-name

Vector store schema name

spring.ai.vectorstore.mariadb.table-name

Vector store table name

spring.ai.vectorstore.mariadb.schema-validation

Enables schema and table name validation to ensure they are valid and existing objects.

Instead of using the Spring Boot auto-configuration, you can manually configure the MariaDB vector store. For this you need to add the following dependencies to your project:

Then create the MariaDBVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with MariaDB Vector store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

The MariaDB Vector Store automatically calculates similarity scores for documents returned from similarity searches. These scores provide a normalized measure of how closely each document matches your search query.

Similarity scores are calculated using the formula score = 1.0 - distance, where:

Score: A value between 0.0 and 1.0, where 1.0 indicates perfect similarity and 0.0 indicates no similarity

Distance: The raw distance value calculated using the configured distance type (COSINE or EUCLIDEAN)

This means that documents with smaller distances (more similar) will have higher scores, making the results more intuitive to interpret.

You can access the similarity score for each document through the getScore() method:

Search results are automatically ordered by similarity score in descending order (highest score first). This ensures that the most relevant documents appear at the top of your results.

In addition to the similarity score, the raw distance value is still available in the document metadata:

When using similarity thresholds in your search requests, specify the threshold as a score value (0.0 to 1.0) rather than a distance:

This makes threshold values consistent and intuitive - higher values mean more restrictive searches that only return highly similar documents.

The MariaDB Vector Store implementation provides access to the underlying native JDBC client (JdbcTemplate) through the getNativeClient() method:

The native client gives you access to MariaDB-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-mariadb</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-mariadb'
}
```

Example 3 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

Example 4 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to MariaDB
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

---

## OpenSearch :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/opensearch.html

**Contents:**
- OpenSearch
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Accessing the Native Client

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up OpenSearchVectorStore to store document embeddings and perform similarity searches.

OpenSearch is an open-source search and analytics engine originally forked from Elasticsearch, distributed under the Apache License 2.0. It enhances AI application development by simplifying the integration and management of AI-generated assets. OpenSearch supports vector, lexical, and hybrid search capabilities, leveraging advanced vector database functionalities to facilitate low-latency queries and similarity searches as detailed on the vector database page.

The OpenSearch k-NN functionality allows users to query vector embeddings from large datasets. An embedding is a numerical representation of a data object, such as text, image, audio, or document. Embeddings can be stored in the index and queried using various similarity functions.

A running OpenSearch instance. The following options are available:

Self-Managed OpenSearch

Amazon OpenSearch Service

If required, an API key for the EmbeddingModel to generate the embeddings stored by the OpenSearchVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the OpenSearch Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the OpenSearchVectorStore as a vector store in your application:

To connect to OpenSearch and use the OpenSearchVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.opensearch.* are used to configure the OpenSearchVectorStore:

spring.ai.vectorstore.opensearch.uris

URIs of the OpenSearch cluster endpoints

spring.ai.vectorstore.opensearch.username

Username for accessing the OpenSearch cluster

spring.ai.vectorstore.opensearch.password

Password for the specified username

spring.ai.vectorstore.opensearch.index-name

Name of the index to store vectors

spring-ai-document-index

spring.ai.vectorstore.opensearch.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.opensearch.similarity-function

The similarity function to use (cosinesimil, l1, l2, linf, innerproduct)

spring.ai.vectorstore.opensearch.use-approximate-knn

Whether to use approximate k-NN for faster searches. If true, uses HNSW-based approximate search. If false, uses exact brute-force k-NN. See Approximate k-NN and Exact k-NN

spring.ai.vectorstore.opensearch.dimensions

Number of dimensions for vector embeddings. Used when creating index mapping for approximate k-NN. If not set, uses the embedding model’s dimensions.

spring.ai.vectorstore.opensearch.mapping-json

Custom JSON mapping for the index. Overrides default mapping generation.

spring.ai.vectorstore.opensearch.read-timeout

Time to wait for response from the opposite endpoint. 0 - infinity.

spring.ai.vectorstore.opensearch.connect-timeout

Time to wait until connection established. 0 - infinity.

spring.ai.vectorstore.opensearch.path-prefix

Path prefix for OpenSearch API endpoints. Useful when OpenSearch is behind a reverse proxy with a non-root path.

spring.ai.vectorstore.opensearch.ssl-bundle

Name of the SSL Bundle to use in case of SSL connection

spring.ai.vectorstore.opensearch.aws.host

Hostname of the OpenSearch instance

spring.ai.vectorstore.opensearch.aws.service-name

spring.ai.vectorstore.opensearch.aws.access-key

spring.ai.vectorstore.opensearch.aws.secret-key

spring.ai.vectorstore.opensearch.aws.region

You can control whether the AWS-specific OpenSearch auto-configuration is enabled using the spring.ai.vectorstore.opensearch.aws.enabled property.

If this property is set to false, the non-AWS OpenSearch configuration is activated, even if AWS SDK classes are present on the classpath. This allows you to use self-managed or third-party OpenSearch clusters in environments where AWS SDKs are present for other services.

If AWS SDK classes are not present, the non-AWS configuration is always used.

If AWS SDK classes are present and the property is not set or set to true, the AWS-specific configuration is used by default.

This fallback logic ensures that users have explicit control over the type of OpenSearch integration, preventing accidental activation of AWS-specific logic when not desired.

The path-prefix property allows you to specify a custom path prefix when OpenSearch is running behind a reverse proxy that uses a non-root path. For example, if your OpenSearch instance is accessible at example.com/opensearch/ instead of example.com/, you would set path-prefix: /opensearch.

The following similarity functions are available:

cosinesimil - Default, suitable for most use cases. Measures cosine similarity between vectors.

l1 - Manhattan distance between vectors.

l2 - Euclidean distance between vectors.

linf - Chebyshev distance between vectors.

Instead of using the Spring Boot auto-configuration, you can manually configure the OpenSearch vector store. For this you need to add the spring-ai-opensearch-store to your project:

or to your Gradle build.gradle build file:

Create an OpenSearch client bean:

Then create the OpenSearchVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with OpenSearch as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary OpenSearch filter format:

The OpenSearch Vector Store implementation provides access to the underlying native OpenSearch client (OpenSearchClient) through the getNativeClient() method:

The native client gives you access to OpenSearch-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-opensearch</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-opensearch'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to OpenSearch
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  ai:
    vectorstore:
      opensearch:
        uris: <opensearch instance URIs>
        username: <opensearch username>
        password: <opensearch password>
        index-name: spring-ai-document-index
        initialize-schema: true
        similarity-function: cosinesimil
        read-timeout: <time to wait for response>
        connect-timeout: <time to wait until connection established>
        path-prefix: <custom path prefix>
        ssl-bundle: <name of SSL bundle>
        aws:  # Only for Amazon OpenSearch Service
          host: <aws opensearch host>
          service-name: <aws service name>
          access-key: <aws access key>
          secret-key: <aws secret key>
          region: <aws region>
```

---

## Typesense :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/typesense.html

**Contents:**
- Typesense
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Accessing the Native Client

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up TypesenseVectorStore to store document embeddings and perform similarity searches.

Typesense is an open source typo tolerant search engine that is optimized for instant sub-50ms searches while providing an intuitive developer experience. It provides vector search capabilities that allow you to store and query high-dimensional vectors alongside your regular search data.

A running Typesense instance. The following options are available:

Typesense Cloud (recommended)

Docker image typesense/typesense:latest

If required, an API key for the EmbeddingModel to generate the embeddings stored by the TypesenseVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Typesense Vector Store. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

The vector store implementation can initialize the requisite schema for you but you must opt-in by setting …​initialize-schema=true in the application.properties file.

Additionally you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the TypesenseVectorStore as a vector store in your application:

To connect to Typesense and use the TypesenseVectorStore you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.typesense.* are used to configure the TypesenseVectorStore:

spring.ai.vectorstore.typesense.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.typesense.collection-name

The name of the collection to store vectors

spring.ai.vectorstore.typesense.embedding-dimension

The number of dimensions in the vector

spring.ai.vectorstore.typesense.client.protocol

spring.ai.vectorstore.typesense.client.host

spring.ai.vectorstore.typesense.client.port

spring.ai.vectorstore.typesense.client.api-key

Instead of using the Spring Boot auto-configuration you can manually configure the Typesense vector store. For this you need to add the spring-ai-typesense-store to your project:

or to your Gradle build.gradle build file.

Create a Typesense Client bean:

Then create the TypesenseVectorStore bean using the builder pattern:

You can leverage the generic portable metadata filters with Typesense store as well.

For example you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example this portable filter expression:

is converted into the proprietary Typesense filter format:

If you are not retrieving the documents in the expected order or the search results are not as expected, check the embedding model you are using.

Embedding models can have a significant impact on the search results (i.e. make sure if your data is in Spanish to use a Spanish or multilingual embedding model).

The Typesense Vector Store implementation provides access to the underlying native Typesense client (Client) through the getNativeClient() method:

The native client gives you access to Typesense-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-typesense</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-typesense'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Typesense
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  ai:
    vectorstore:
      typesense:
        initialize-schema: true
        collection-name: vector_store
        embedding-dimension: 1536
        client:
          protocol: http
          host: localhost
          port: 8108
          api-key: xyz
```

---

## Testcontainers :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/testcontainers.html

**Contents:**
- Testcontainers
- Service Connections

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

## Milvus :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/milvus.html

**Contents:**
- Milvus
- Prerequisites
- Dependencies
  - Manual Configuration
- Metadata filtering
- Using MilvusSearchRequest
- Importance of nativeExpression and searchParamsJson in MilvusSearchRequest
- Milvus VectorStore properties
- Starting Milvus Store
- Troubleshooting

Milvus is an open-source vector database that has garnered significant attention in the fields of data science and machine learning. One of its standout features lies in its robust support for vector indexing and querying. Milvus employs state-of-the-art, cutting-edge algorithms to accelerate the search process, making it exceptionally efficient at retrieving similar vectors, even when handling extensive datasets.

A running Milvus instance. The following options are available:

Milvus Standalone: Docker, Operator, Helm,DEB/RPM, Docker Compose.

Milvus Cluster: Operator, Helm.

If required, an API key for the EmbeddingModel to generate the embeddings stored by the MilvusVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Then add the Milvus VectorStore boot starter dependency to your project:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

The Vector Store, also requires an EmbeddingModel instance to calculate embeddings for the documents. You can pick one of the available EmbeddingModel Implementations.

To connect to and configure the MilvusVectorStore, you need to provide access details for your instance. A simple configuration can either be provided via Spring Boot’s application.yml

Now you can Auto-wire the Milvus Vector Store in your application and use it

Instead of using the Spring Boot auto-configuration, you can manually configure the MilvusVectorStore. To add the following dependencies to your project:

To configure MilvusVectorStore in your application, you can use the following setup:

You can leverage the generic, portable metadata filters with the Milvus store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

MilvusSearchRequest extends SearchRequest, allowing you to use Milvus-specific search parameters such as native expressions and search parameter JSON.

This allows greater flexibility when using Milvus-specific search features.

These two parameters enhance Milvus search precision and ensure optimal query performance:

nativeExpression: Enables additional filtering capabilities using Milvus' native filtering expressions. Milvus Filtering

searchParamsJson: Essential for tuning search behavior when using IVF_FLAT, Milvus' default index. Milvus Vector Index

By default, IVF_FLAT requires nprobe to be set for accurate results. If not specified, nprobe defaults to 1, which can lead to poor recall or even zero search results.

Using nativeExpression ensures advanced filtering, while searchParamsJson prevents ineffective searches caused by a low default nprobe value.

You can use the following properties in your Spring Boot configuration to customize the Milvus vector store.

spring.ai.vectorstore.milvus.database-name

The name of the Milvus database to use.

spring.ai.vectorstore.milvus.collection-name

Milvus collection name to store the vectors

spring.ai.vectorstore.milvus.initialize-schema

whether to initialize Milvus' backend

spring.ai.vectorstore.milvus.embedding-dimension

The dimension of the vectors to be stored in the Milvus collection.

spring.ai.vectorstore.milvus.index-type

The type of the index to be created for the Milvus collection.

spring.ai.vectorstore.milvus.metric-type

The metric type to be used for the Milvus collection.

spring.ai.vectorstore.milvus.index-parameters

The index parameters to be used for the Milvus collection.

spring.ai.vectorstore.milvus.id-field-name

The ID field name for the collection

spring.ai.vectorstore.milvus.auto-id

Boolean flag to indicate if the auto-id is used for the ID field

spring.ai.vectorstore.milvus.content-field-name

The content field name for the collection

spring.ai.vectorstore.milvus.metadata-field-name

The metadata field name for the collection

spring.ai.vectorstore.milvus.embedding-field-name

The embedding field name for the collection

spring.ai.vectorstore.milvus.client.host

The name or address of the host.

spring.ai.vectorstore.milvus.client.port

spring.ai.vectorstore.milvus.client.uri

The uri of Milvus instance

spring.ai.vectorstore.milvus.client.token

Token serving as the key for identification and authentication purposes.

spring.ai.vectorstore.milvus.client.connect-timeout-ms

Connection timeout value of client channel. The timeout value must be greater than zero .

spring.ai.vectorstore.milvus.client.keep-alive-time-ms

Keep-alive time value of client channel. The keep-alive value must be greater than zero.

spring.ai.vectorstore.milvus.client.keep-alive-timeout-ms

The keep-alive timeout value of client channel. The timeout value must be greater than zero.

spring.ai.vectorstore.milvus.client.rpc-deadline-ms

Deadline for how long you are willing to wait for a reply from the server. With a deadline setting, the client will wait when encounter fast RPC fail caused by network fluctuations. The deadline value must be larger than or equal to zero.

spring.ai.vectorstore.milvus.client.client-key-path

The client.key path for tls two-way authentication, only takes effect when "secure" is true

spring.ai.vectorstore.milvus.client.client-pem-path

The client.pem path for tls two-way authentication, only takes effect when "secure" is true

spring.ai.vectorstore.milvus.client.ca-pem-path

The ca.pem path for tls two-way authentication, only takes effect when "secure" is true

spring.ai.vectorstore.milvus.client.server-pem-path

server.pem path for tls one-way authentication, only takes effect when "secure" is true.

spring.ai.vectorstore.milvus.client.server-name

Sets the target name override for SSL host name checking, only takes effect when "secure" is True. Note: this value is passed to grpc.ssl_target_name_override

spring.ai.vectorstore.milvus.client.secure

Secure the authorization for this connection, set to True to enable TLS.

spring.ai.vectorstore.milvus.client.idle-timeout-ms

Idle timeout value of client channel. The timeout value must be larger than zero.

spring.ai.vectorstore.milvus.client.username

The username and password for this connection.

spring.ai.vectorstore.milvus.client.password

The password for this connection.

From within the src/test/resources/ folder run:

To clean the environment:

Then connect to the vector store on http://localhost:19530 or for management http://localhost:9001 (user: minioadmin, pass: minioadmin)

If Docker complains about resources, then execute:

The Milvus Vector Store implementation provides access to the underlying native Milvus client (MilvusServiceClient) through the getNativeClient() method:

The native client gives you access to Milvus-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-vector-store-milvus</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-milvus'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List <Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Milvus Vector Store
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = this.vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-milvus-store</artifactId>
</dependency>
```

---

## GemFire Vector Store :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/gemfire.html

**Contents:**
- GemFire Vector Store
- Prerequisites
- Auto-configuration
  - Configuration properties
- Manual Configuration
- Usage
- Metadata Filtering

For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up the GemFireVectorStore to store document embeddings and perform similarity searches.

GemFire is a distributed, in-memory, key-value store performing read and write operations at blazingly fast speeds. It offers highly available parallel message queues, continuous availability, and an event-driven architecture you can scale dynamically without downtime. As your data size requirements increase to support high-performance, real-time apps, GemFire can easily scale linearly.

GemFire VectorDB extends GemFire’s capabilities, serving as a versatile vector database that efficiently stores, retrieves, and performs vector similarity searches.

A GemFire cluster with the GemFire VectorDB extension enabled

Install GemFire VectorDB extension

An EmbeddingModel bean to compute the document embeddings. Refer to the EmbeddingModel section for more information. An option that runs locally on your machine is ONNX and the all-MiniLM-L6-v2 Sentence Transformers.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add the GemFire VectorStore Spring Boot starter to you project’s Maven build file pom.xml:

or to your Gradle build.gradle file

You can use the following properties in your Spring Boot configuration to further configure the GemFireVectorStore.

spring.ai.vectorstore.gemfire.host

spring.ai.vectorstore.gemfire.port

spring.ai.vectorstore.gemfire.initialize-schema

spring.ai.vectorstore.gemfire.index-name

spring-ai-gemfire-store

spring.ai.vectorstore.gemfire.beam-width

spring.ai.vectorstore.gemfire.max-connections

spring.ai.vectorstore.gemfire.vector-similarity-function

spring.ai.vectorstore.gemfire.fields

spring.ai.vectorstore.gemfire.buckets

To use just the GemFireVectorStore, without Spring Boot’s Auto-configuration add the following dependency to your project’s Maven pom.xml:

For Gradle users, add the following to your build.gradle file under the dependencies block to use just the GemFireVectorStore:

Here is a sample that creates an instance of the GemfireVectorStore instead of using AutoConfiguration

The default configuration connects to a GemFire cluster at localhost:8080

In your application, create a few documents:

Add the documents to the vector store:

And to retrieve documents using similarity search:

You should retrieve the document containing the text "Spring AI rocks!!".

You can also limit the number of results using a similarity threshold:

You can leverage the generic, portable metadata filters with GemFire VectorStore as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary GemFire VectorDB filter format:

The GemFire VectorStore supports a wide range of filter operations:

Equality: country == 'BG' → country:BG

Inequality: city != 'Sofia' → city: NOT Sofia

Greater Than: year > 2020 → year:{2020 TO *]

Greater Than or Equal: year >= 2020 → year:[2020 TO *]

Less Than: year < 2025 → year:[* TO 2025}

Less Than or Equal: year ⇐ 2025 → year:[* TO 2025]

IN: country in ['BG', 'NL'] → country:(BG OR NL)

NOT IN: country nin ['BG', 'NL'] → NOT country:(BG OR NL)

AND/OR: Logical operators for combining conditions

Grouping: Use parentheses for complex expressions

Date Filtering: Date values in ISO 8601 format (e.g., 2024-01-07T14:29:12Z)

To use metadata filtering with GemFire VectorStore, you must specify the metadata fields that can be filtered when creating the vector store. This is done using the fields parameter in the builder:

Or via configuration properties:

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-gemfire</artifactId>
</dependency>
```

Example 2 (xml):
```xml
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-gemfire'
}
```

Example 3 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-gemfire-store</artifactId>
</dependency>
```

Example 4 (java):
```java
@Bean
public GemFireVectorStore vectorStore(EmbeddingModel embeddingModel) {
    return GemFireVectorStore.builder(embeddingModel)
        .host("localhost")
        .port(7071)
        .indexName("my-vector-index")
        .fields(new String[] {"country", "year", "activationDate"}) // Optional: fields for metadata filtering
        .initializeSchema(true)
        .build();
}
```

---

## PGvector :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/pgvector.html

**Contents:**
- PGvector
- Prerequisites
- Auto-Configuration
  - Configuration properties
- Metadata filtering
- Manual Configuration
- Run Postgres & PGVector DB locally
- Accessing the Native Client

For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up the PGvector VectorStore to store document embeddings and perform similarity searches.

PGvector is an open-source extension for PostgreSQL that enables storing and searching over machine learning-generated embeddings. It provides different capabilities that let users identify both exact and approximate nearest neighbors. It is designed to work seamlessly with other PostgreSQL features, including indexing and querying.

First you need access to PostgreSQL instance with enabled vector, hstore and uuid-ossp extensions.

On startup, the PgVectorStore will attempt to install the required database extensions and create the required vector_store table with an index if not existing.

Optionally, you can do this manually like so:

Next, if required, an API key for the EmbeddingModel to generate the embeddings stored by the PgVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Then add the PgVectorStore boot starter dependency to your project:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the required schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

The Vector Store also requires an EmbeddingModel instance to calculate embeddings for the documents. You can pick one of the available EmbeddingModel Implementations.

For example, to use the OpenAI EmbeddingModel, add the following dependency to your project:

or to your Gradle build.gradle build file.

To connect to and configure the PgVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml.

Now you can auto-wire the VectorStore in your application and use it

You can use the following properties in your Spring Boot configuration to customize the PGVector vector store.

spring.ai.vectorstore.pgvector.index-type

Nearest neighbor search index type. Options are NONE - exact nearest neighbor search, IVFFlat - index divides vectors into lists, and then searches a subset of those lists that are closest to the query vector. It has faster build times and uses less memory than HNSW, but has lower query performance (in terms of speed-recall tradeoff). HNSW - creates a multilayer graph. It has slower build times and uses more memory than IVFFlat, but has better query performance (in terms of speed-recall tradeoff). There’s no training step like IVFFlat, so the index can be created without any data in the table.

spring.ai.vectorstore.pgvector.distance-type

Search distance type. Defaults to COSINE_DISTANCE. But if vectors are normalized to length 1, you can use EUCLIDEAN_DISTANCE or NEGATIVE_INNER_PRODUCT for best performance.

spring.ai.vectorstore.pgvector.dimensions

Embeddings dimension. If not specified explicitly the PgVectorStore will retrieve the dimensions form the provided EmbeddingModel. Dimensions are set to the embedding column the on table creation. If you change the dimensions your would have to re-create the vector_store table as well.

spring.ai.vectorstore.pgvector.remove-existing-vector-store-table

Deletes the existing vector_store table on start up.

spring.ai.vectorstore.pgvector.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.pgvector.schema-name

Vector store schema name

spring.ai.vectorstore.pgvector.table-name

Vector store table name

spring.ai.vectorstore.pgvector.schema-validation

Enables schema and table name validation to ensure they are valid and existing objects.

spring.ai.vectorstore.pgvector.max-document-batch-size

Maximum number of documents to process in a single batch.

You can leverage the generic, portable metadata filters with the PgVector store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

Instead of using the Spring Boot auto-configuration, you can manually configure the PgVectorStore. For this you need to add the PostgreSQL connection and JdbcTemplate auto-configuration dependencies to your project:

To configure PgVector in your application, you can use the following setup:

You can connect to this server like this:

The PGVector Store implementation provides access to the underlying native JDBC client (JdbcTemplate) through the getNativeClient() method:

The native client gives you access to PostgreSQL-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-vector-store-pgvector</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-pgvector'
}
```

Example 3 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

Example 4 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-openai'
}
```

---

## Untitled :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/hanadb-provision-a-trial-account.html

**Contents:**
- Provision SAP HANA Cloud trial account

For the latest snapshot version, please use Spring AI 1.1.2!

Below are the steps to provision SAP Hana Database using a trial account

Let’s start with creating a temporary email for registration purposes

Go to sap.com and navigate to products → Trials and Demos

Click Advanced Trials

Click Start your free 90-day trial

Paste the temporary email id that we created in the first step, and click Next

We fill in our details and click Submit

It’s time to check the inbox of our temporary email account

Notice that there is an email received in our temporary email account

Open the email and click to activate the trial account

It will prompt to create a password. Provide a password and click Submit

The trial account is now created. Click to start the trial

Provide your phone number and click Continue

We receive an OTP on the phone number. Provide the code and click continue

Select the region as US East (VA) - AWS

The SAP BTP trial account is ready. Click Go to your Trial account

Click the Trial sub-account

Open Instances and Subscriptions

It’s time to create a subscription. Click the Create button

While creating a subscription, Select service as SAP Hana Cloud and Plan as tools and click Create

Notice that SAP Hana Cloud subscription is now created. Click Users on the left panel

Select the username (temporary email that we supplied earlier) and click Assign Role Collection

Search hana and select all the 3 role collections that gets displayed. Click Assign Role Collection

Our user now has all the 3 role collections. Click Instances and Subscriptions

Now, click SAP Hana Cloud application under subscriptions

There are no instances yet. Let’s click Create Instance

Select Type as SAP HANA Cloud, SAP HANA Database. Click Next Step

Provide Instance Name, Description, password for DBADMIN administrator. Select the latest version 2024.2 (QRC 1/2024). Click Next Step

Keep everything as default. Click Next Step

Select Allow all IP addresses and click Next Step

Click Review and Create

Click Create Instance

Notice that the provisioning of SAP Hana Database instance has started. It takes some time to provision - please be patient.

Once the instance is provisioned (status is displayed as Running) we can get the datasource url (SQL Endpoint) by clicking the instance and selecting Connections

We navigate to SAP Hana Database Explorer by click the …​

Provide the administrator credentials and click OK

Open SQL console and create the table CRICKET_WORLD_CUP using the following DDL statement:

Navigate to hana_dev_db → Catalog → Tables to find our table CRICKET_WORLD_CUP

Right-click on the table and click Open Data

Notice that the table data is now displayed. There are now rows as we didn’t create any embeddings yet.

Next steps: SAP Hana Vector Engine

---

## Qdrant :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/qdrant.html

**Contents:**
- Qdrant
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Accessing the Native Client

For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up the Qdrant VectorStore to store document embeddings and perform similarity searches.

Qdrant is an open-source, high-performance vector search engine/database. It uses HNSW (Hierarchical Navigable Small World) algorithm for efficient k-NN search operations and provides advanced filtering capabilities for metadata-based queries.

Qdrant Instance: Set up a Qdrant instance by following the installation instructions in the Qdrant documentation.

If required, an API key for the EmbeddingModel to generate the embeddings stored by the QdrantVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Qdrant Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the builder or by setting …​initialize-schema=true in the application.properties file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the QdrantVectorStore as a vector store in your application.

To connect to Qdrant and use the QdrantVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.qdrant.* are used to configure the QdrantVectorStore:

spring.ai.vectorstore.qdrant.host

The host of the Qdrant server

spring.ai.vectorstore.qdrant.port

The gRPC port of the Qdrant server

spring.ai.vectorstore.qdrant.api-key

The API key to use for authentication

spring.ai.vectorstore.qdrant.collection-name

The name of the collection to use

spring.ai.vectorstore.qdrant.use-tls

Whether to use TLS(HTTPS)

spring.ai.vectorstore.qdrant.initialize-schema

Whether to initialize the schema

Instead of using the Spring Boot auto-configuration, you can manually configure the Qdrant vector store. For this you need to add the spring-ai-qdrant-store to your project:

or to your Gradle build.gradle build file.

Create a Qdrant client bean:

Then create the QdrantVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with Qdrant store as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

The Qdrant Vector Store implementation provides access to the underlying native Qdrant client (QdrantClient) through the getNativeClient() method:

The native client gives you access to Qdrant-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-qdrant</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-qdrant'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Qdrant
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  ai:
    vectorstore:
      qdrant:
        host: <qdrant host>
        port: <qdrant grpc port>
        api-key: <qdrant api key>
        collection-name: <collection name>
        use-tls: false
        initialize-schema: true
```

---

## SAP HANA Cloud :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/hana.html

**Contents:**
- SAP HANA Cloud
- Prerequisites
- Auto-configuration
- HanaCloudVectorStore Properties
- Build a Sample RAG application
  - Create an Entity class named CricketWorldCup that extends from HanaVectorEntity:

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

You need a SAP HANA Cloud vector engine account - Refer SAP HANA Cloud vector engine - provision a trial account guide to create a trial account.

If required, an API key for the EmbeddingModel to generate the embeddings stored by the vector store.

Spring AI does not provide a dedicated module for SAP Hana vector store. Users are expected to provide their own configuration in the applications using the standard vector store module for SAP Hana vector store in Spring AI - spring-ai-hanadb-store.

Please have a look at the list of HanaCloudVectorStore Properties for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

You can use the following properties in your Spring Boot configuration to customize the SAP Hana vector store. It uses spring.datasource. properties to configure the Hana datasource and the spring.ai.vectorstore.hanadb. properties to configure the Hana vector store.

spring.datasource.driver-class-name

com.sap.db.jdbc.Driver

spring.datasource.url

spring.datasource.username

Hana datasource username

spring.datasource.password

Hana datasource password

spring.ai.vectorstore.hanadb.top-k

spring.ai.vectorstore.hanadb.table-name

spring.ai.vectorstore.hanadb.initialize-schema

whether to initialize the required schema

Shows how to setup a project that uses SAP Hana Cloud as the vector DB and leverage OpenAI to implement RAG pattern

Create a table CRICKET_WORLD_CUP in SAP Hana DB:

Add the following dependencies in your pom.xml

You may set the property spring-ai-version as <spring-ai-version>1.0.0-SNAPSHOT</spring-ai-version>:

Add the following properties in application.properties file:

Create a Repository named CricketWorldCupRepository that implements HanaVectorRepository interface:

Now, create a REST Controller class CricketWorldCupHanaController, and autowire ChatModel and VectorStore as dependencies In this controller class, create the following REST endpoints:

/ai/hana-vector-store/cricket-world-cup/purge-embeddings - to purge all the embeddings from the Vector Store

/ai/hana-vector-store/cricket-world-cup/upload - to upload the Cricket_World_Cup.pdf so that its data gets stored in SAP Hana Cloud Vector DB as embeddings

/ai/hana-vector-store/cricket-world-cup - to implement RAG using Cosine_Similarity in SAP Hana DB

Since HanaDB vector store support does not provide the autoconfiguration module, you also need to provide the vector store bean in your application, as shown below, as an example.

Use a contextual pdf file from wikipedia

Go to wikipedia and download Cricket World Cup page as a PDF file.

Upload this PDF file using the file-upload REST endpoint that we created in the previous step.

**Examples:**

Example 1 (xml):
```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-bom</artifactId>
            <version>${spring-ai-version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-pdf-document-reader</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-hana</artifactId>
</dependency>

<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.30</version>
    <scope>provided</scope>
</dependency>
```

Example 2 (java):
```java
package com.interviewpedia.spring.ai.hana;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.Table;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.extern.jackson.Jacksonized;
import org.springframework.ai.vectorstore.hanadb.HanaVectorEntity;

@Entity
@Table(name = "CRICKET_WORLD_CUP")
@Data
@Jacksonized
@NoArgsConstructor
public class CricketWorldCup extends HanaVectorEntity {
    @Column(name = "content")
    private String content;
}
```

Example 3 (java):
```java
package com.interviewpedia.spring.ai.hana;

import jakarta.persistence.EntityManager;
import jakarta.persistence.PersistenceContext;
import jakarta.transaction.Transactional;
import org.springframework.ai.vectorstore.hanadb.HanaVectorRepository;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public class CricketWorldCupRepository implements HanaVectorRepository<CricketWorldCup> {
    @PersistenceContext
    private EntityManager entityManager;

    @Override
    @Transactional
    public void save(String tableName, String id, String embedding, String content) {
        String sql = String.format("""
                INSERT INTO %s (_ID, EMBEDDING, CONTENT)
                VALUES(:_id, TO_REAL_VECTOR(:embedding), :content)
                """, tableName);

		this.entityManager.createNativeQuery(sql)
                .setParameter("_id", id)
                .setParameter("embedding", embedding)
                .setParameter("content", content)
                .executeUpdate();
    }

    @Override
    @Transactional
    public int deleteEmbeddingsById(String tableName, List<String> idList) {
        String sql = String.format("""
                DELETE FROM %s WHERE _ID IN (:ids)
                """, tableName);

        return this.entityManager.createNativeQuery(sql)
                .setParameter("ids", idList)
                .executeUpdate();
    }

    @Override
    @Transactional
    public int deleteAllEmbeddings(String tableName) {
        String sql = String.format("""
                DELETE FROM %s
                """, tableName);

        return this.entityManager.createNativeQuery(sql).executeUpdate();
    }

    @Override
    public List<CricketWorldCup> cosineSimilaritySearch(String tableName, int topK, String queryEmbedding) {
        String sql = String.format("""
                SELECT TOP :topK * FROM %s
                ORDER BY COSINE_SIMILARITY(EMBEDDING, TO_REAL_VECTOR(:queryEmbedding)) DESC
                """, tableName);

        return this.entityManager.createNativeQuery(sql, CricketWorldCup.class)
                .setParameter("topK", topK)
                .setParameter("queryEmbedding", queryEmbedding)
                .getResultList();
    }
}
```

Example 4 (java):
```java
package com.interviewpedia.spring.ai.hana;

import lombok.extern.slf4j.Slf4j;
import org.springframework.ai.chat.model.ChatModel;
import org.springframework.ai.chat.messages.UserMessage;
import org.springframework.ai.chat.prompt.Prompt;
import org.springframework.ai.chat.prompt.SystemPromptTemplate;
import org.springframework.ai.document.Document;
import org.springframework.ai.reader.pdf.PagePdfDocumentReader;
import org.springframework.ai.transformer.splitter.TokenTextSplitter;
import org.springframework.ai.vectorstore.hanadb.HanaCloudVectorStore;
import org.springframework.ai.vectorstore.VectorStore;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.io.Resource;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;
import java.util.List;
import java.util.Map;
import java.util.function.Function;
import java.util.function.Supplier;
import java.util.stream.Collectors;

@RestController
@Slf4j
public class CricketWorldCupHanaController {
    private final VectorStore hanaCloudVectorStore;
    private final ChatModel chatModel;

    @Autowired
    public CricketWorldCupHanaController(ChatModel chatModel, VectorStore hanaCloudVectorStore) {
        this.chatModel = chatModel;
        this.hanaCloudVectorStore = hanaCloudVectorStore;
    }

    @PostMapping("/ai/hana-vector-store/cricket-world-cup/purge-embeddings")
    public ResponseEntity<String> purgeEmbeddings() {
        int deleteCount = ((HanaCloudVectorStore) this.hanaCloudVectorStore).purgeEmbeddings();
        log.info("{} embeddings purged from CRICKET_WORLD_CUP table in Hana DB", deleteCount);
        return ResponseEntity.ok().body(String.format("%d embeddings purged from CRICKET_WORLD_CUP table in Hana DB", deleteCount));
    }

    @PostMapping("/ai/hana-vector-store/cricket-world-cup/upload")
    public ResponseEntity<String> handleFileUpload(@RequestParam("pdf") MultipartFile file) throws IOException {
        Resource pdf = file.getResource();
        Supplier<List<Document>> reader = new PagePdfDocumentReader(pdf);
        Function<List<Document>, List<Document>> splitter = new TokenTextSplitter();
        List<Document> documents = splitter.apply(reader.get());
        log.info("{} documents created from pdf file: {}", documents.size(), pdf.getFilename());
		this.hanaCloudVectorStore.accept(documents);
        return ResponseEntity.ok().body(String.format("%d documents created from pdf file: %s",
                documents.size(), pdf.getFilename()));
    }

    @GetMapping("/ai/hana-vector-store/cricket-world-cup")
    public Map<String, String> hanaVectorStoreSearch(@RequestParam(value = "message") String message) {
        var documents = this.hanaCloudVectorStore.similaritySearch(message);
        var inlined = documents.stream().map(Document::getText).collect(Collectors.joining(System.lineSeparator()));
        var similarDocsMessage = new SystemPromptTemplate("Based on the following: {documents}")
                .createMessage(Map.of("documents", inlined));

        var userMessage = new UserMessage(message);
        Prompt prompt = new Prompt(List.of(similarDocsMessage, userMessage));
        String generation = this.chatModel.call(prompt).getResult().getOutput().getContent();
        log.info("Generation: {}", generation);
        return Map.of("generation", generation);
    }
}
```

---

## Redis :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/redis.html

**Contents:**
- Redis
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Metadata Filtering
- Manual Configuration
- Accessing the Native Client

For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up RedisVectorStore to store document embeddings and perform similarity searches.

Redis is an open source (BSD licensed), in-memory data structure store used as a database, cache, message broker, and streaming engine. Redis provides data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes, and streams.

Redis Search and Query extends the core features of Redis OSS and allows you to use Redis as a vector database:

Store vectors and the associated metadata within hashes or JSON documents

Perform vector searches

A Redis Stack instance

Redis Cloud (recommended)

Docker image redis/redis-stack:latest

EmbeddingModel instance to compute the document embeddings. Several options are available:

If required, an API key for the EmbeddingModel to generate the embeddings stored by the RedisVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Redis Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the RedisVectorStore as a vector store in your application.

To connect to Redis and use the RedisVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml,

For redis connection configuration, alternatively, a simple configuration can be provided via Spring Boot’s application.properties.

Properties starting with spring.ai.vectorstore.redis.* are used to configure the RedisVectorStore:

spring.ai.vectorstore.redis.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.redis.index-name

The name of the index to store the vectors

spring.ai.vectorstore.redis.prefix

The prefix for Redis keys

You can leverage the generic, portable metadata filters with Redis as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Redis filter format:

Instead of using the Spring Boot auto-configuration, you can manually configure the Redis vector store. For this you need to add the spring-ai-redis-store to your project:

or to your Gradle build.gradle build file.

Create a JedisPooled bean:

Then create the RedisVectorStore bean using the builder pattern:

You must list explicitly all metadata field names and types (TAG, TEXT, or NUMERIC) for any metadata field used in filter expressions. The metadataFields above registers filterable metadata fields: country of type TAG, year of type NUMERIC.

The Redis Vector Store implementation provides access to the underlying native Redis client (JedisPooled) through the getNativeClient() method:

The native client gives you access to Redis-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-redis</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-redis'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List <Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Redis
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = this.vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  data:
    redis:
      url: <redis instance url>
  ai:
    vectorstore:
      redis:
        initialize-schema: true
        index-name: custom-index
        prefix: custom-prefix
```

---

## OpenSearch :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/opensearch.html

**Contents:**
- OpenSearch
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Accessing the Native Client

For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up OpenSearchVectorStore to store document embeddings and perform similarity searches.

OpenSearch is an open-source search and analytics engine originally forked from Elasticsearch, distributed under the Apache License 2.0. It enhances AI application development by simplifying the integration and management of AI-generated assets. OpenSearch supports vector, lexical, and hybrid search capabilities, leveraging advanced vector database functionalities to facilitate low-latency queries and similarity searches as detailed on the vector database page.

The OpenSearch k-NN functionality allows users to query vector embeddings from large datasets. An embedding is a numerical representation of a data object, such as text, image, audio, or document. Embeddings can be stored in the index and queried using various similarity functions.

A running OpenSearch instance. The following options are available:

Self-Managed OpenSearch

Amazon OpenSearch Service

If required, an API key for the EmbeddingModel to generate the embeddings stored by the OpenSearchVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the OpenSearch Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the OpenSearchVectorStore as a vector store in your application:

To connect to OpenSearch and use the OpenSearchVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.opensearch.* are used to configure the OpenSearchVectorStore:

spring.ai.vectorstore.opensearch.uris

URIs of the OpenSearch cluster endpoints

spring.ai.vectorstore.opensearch.username

Username for accessing the OpenSearch cluster

spring.ai.vectorstore.opensearch.password

Password for the specified username

spring.ai.vectorstore.opensearch.index-name

Name of the index to store vectors

spring-ai-document-index

spring.ai.vectorstore.opensearch.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.opensearch.similarity-function

The similarity function to use

spring.ai.vectorstore.opensearch.read-timeout

Time to wait for response from the opposite endpoint. 0 - infinity.

spring.ai.vectorstore.opensearch.connect-timeout

Time to wait until connection established. 0 - infinity.

spring.ai.vectorstore.opensearch.path-prefix

Path prefix for OpenSearch API endpoints. Useful when OpenSearch is behind a reverse proxy with a non-root path.

spring.ai.vectorstore.opensearch.ssl-bundle

Name of the SSL Bundle to use in case of SSL connection

spring.ai.vectorstore.opensearch.aws.host

Hostname of the OpenSearch instance

spring.ai.vectorstore.opensearch.aws.service-name

spring.ai.vectorstore.opensearch.aws.access-key

spring.ai.vectorstore.opensearch.aws.secret-key

spring.ai.vectorstore.opensearch.aws.region

You can control whether the AWS-specific OpenSearch auto-configuration is enabled using the spring.ai.vectorstore.opensearch.aws.enabled property.

If this property is set to false, the non-AWS OpenSearch configuration is activated, even if AWS SDK classes are present on the classpath. This allows you to use self-managed or third-party OpenSearch clusters in environments where AWS SDKs are present for other services.

If AWS SDK classes are not present, the non-AWS configuration is always used.

If AWS SDK classes are present and the property is not set or set to true, the AWS-specific configuration is used by default.

This fallback logic ensures that users have explicit control over the type of OpenSearch integration, preventing accidental activation of AWS-specific logic when not desired.

The path-prefix property allows you to specify a custom path prefix when OpenSearch is running behind a reverse proxy that uses a non-root path. For example, if your OpenSearch instance is accessible at example.com/opensearch/ instead of example.com/, you would set path-prefix: /opensearch.

The following similarity functions are available:

cosinesimil - Default, suitable for most use cases. Measures cosine similarity between vectors.

l1 - Manhattan distance between vectors.

l2 - Euclidean distance between vectors.

linf - Chebyshev distance between vectors.

Instead of using the Spring Boot auto-configuration, you can manually configure the OpenSearch vector store. For this you need to add the spring-ai-opensearch-store to your project:

or to your Gradle build.gradle build file:

Create an OpenSearch client bean:

Then create the OpenSearchVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with OpenSearch as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary OpenSearch filter format:

The OpenSearch Vector Store implementation provides access to the underlying native OpenSearch client (OpenSearchClient) through the getNativeClient() method:

The native client gives you access to OpenSearch-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-opensearch</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-opensearch'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to OpenSearch
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  ai:
    vectorstore:
      opensearch:
        uris: <opensearch instance URIs>
        username: <opensearch username>
        password: <opensearch password>
        index-name: spring-ai-document-index
        initialize-schema: true
        similarity-function: cosinesimil
        read-timeout: <time to wait for response>
        connect-timeout: <time to wait until connection established>
        path-prefix: <custom path prefix>
        ssl-bundle: <name of SSL bundle>
        aws:  # Only for Amazon OpenSearch Service
          host: <aws opensearch host>
          service-name: <aws service name>
          access-key: <aws access key>
          secret-key: <aws secret key>
          region: <aws region>
```

---

## Typesense :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/typesense.html

**Contents:**
- Typesense
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Accessing the Native Client

This section walks you through setting up TypesenseVectorStore to store document embeddings and perform similarity searches.

Typesense is an open source typo tolerant search engine that is optimized for instant sub-50ms searches while providing an intuitive developer experience. It provides vector search capabilities that allow you to store and query high-dimensional vectors alongside your regular search data.

A running Typesense instance. The following options are available:

Typesense Cloud (recommended)

Docker image typesense/typesense:latest

If required, an API key for the EmbeddingModel to generate the embeddings stored by the TypesenseVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Typesense Vector Store. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

The vector store implementation can initialize the requisite schema for you but you must opt-in by setting …​initialize-schema=true in the application.properties file.

Additionally you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the TypesenseVectorStore as a vector store in your application:

To connect to Typesense and use the TypesenseVectorStore you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.typesense.* are used to configure the TypesenseVectorStore:

spring.ai.vectorstore.typesense.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.typesense.collection-name

The name of the collection to store vectors

spring.ai.vectorstore.typesense.embedding-dimension

The number of dimensions in the vector

spring.ai.vectorstore.typesense.client.protocol

spring.ai.vectorstore.typesense.client.host

spring.ai.vectorstore.typesense.client.port

spring.ai.vectorstore.typesense.client.api-key

Instead of using the Spring Boot auto-configuration you can manually configure the Typesense vector store. For this you need to add the spring-ai-typesense-store to your project:

or to your Gradle build.gradle build file.

Create a Typesense Client bean:

Then create the TypesenseVectorStore bean using the builder pattern:

You can leverage the generic portable metadata filters with Typesense store as well.

For example you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example this portable filter expression:

is converted into the proprietary Typesense filter format:

If you are not retrieving the documents in the expected order or the search results are not as expected, check the embedding model you are using.

Embedding models can have a significant impact on the search results (i.e. make sure if your data is in Spanish to use a Spanish or multilingual embedding model).

The Typesense Vector Store implementation provides access to the underlying native Typesense client (Client) through the getNativeClient() method:

The native client gives you access to Typesense-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-typesense</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-typesense'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Typesense
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  ai:
    vectorstore:
      typesense:
        initialize-schema: true
        collection-name: vector_store
        embedding-dimension: 1536
        client:
          protocol: http
          host: localhost
          port: 8108
          api-key: xyz
```

---

## Elasticsearch :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/elasticsearch.html

**Contents:**
- Elasticsearch
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Metadata Filtering
- Manual Configuration
- Accessing the Native Client

For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up the Elasticsearch VectorStore to store document embeddings and perform similarity searches.

Elasticsearch is an open source search and analytics engine based on the Apache Lucene library.

A running Elasticsearch instance. The following options are available:

Self-Managed Elasticsearch

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Elasticsearch Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml or Gradle build.gradle build files:

For spring-boot versions pre 3.3.0 it’s necessary to explicitly add the elasticsearch-java dependency with version > 8.13.3, otherwise the older version used will be incompatible with the queries performed:

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file. Alternatively you can opt-out the initialization and create the index manually using the Elasticsearch client, which can be useful if the index needs advanced mapping or additional configuration.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options. These properties can be also set by configuring the ElasticsearchVectorStoreOptions bean.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the ElasticsearchVectorStore as a vector store in your application.

To connect to Elasticsearch and use the ElasticsearchVectorStore, you need to provide access details for your instance. A simple configuration can either be provided via Spring Boot’s application.yml,

The Spring Boot properties starting with spring.elasticsearch.* are used to configure the Elasticsearch client:

spring.elasticsearch.connection-timeout

Connection timeout used when communicating with Elasticsearch.

spring.elasticsearch.password

Password for authentication with Elasticsearch.

spring.elasticsearch.username

Username for authentication with Elasticsearch.

spring.elasticsearch.uris

Comma-separated list of the Elasticsearch instances to use.

http://localhost:9200

spring.elasticsearch.path-prefix

Prefix added to the path of every request sent to Elasticsearch.

spring.elasticsearch.restclient.sniffer.delay-after-failure

Delay of a sniff execution scheduled after a failure.

spring.elasticsearch.restclient.sniffer.interval

Interval between consecutive ordinary sniff executions.

spring.elasticsearch.restclient.ssl.bundle

spring.elasticsearch.socket-keep-alive

Whether to enable socket keep alive between client and Elasticsearch.

spring.elasticsearch.socket-timeout

Socket timeout used when communicating with Elasticsearch.

Properties starting with spring.ai.vectorstore.elasticsearch.* are used to configure the ElasticsearchVectorStore:

spring.ai.vectorstore.elasticsearch.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.elasticsearch.index-name

The name of the index to store the vectors

spring-ai-document-index

spring.ai.vectorstore.elasticsearch.dimensions

The number of dimensions in the vector

spring.ai.vectorstore.elasticsearch.similarity

The similarity function to use

spring.ai.vectorstore.elasticsearch.embedding-field-name

The name of the vector field to search against

The following similarity functions are available:

cosine - Default, suitable for most use cases. Measures cosine similarity between vectors.

l2_norm - Euclidean distance between vectors. Lower values indicate higher similarity.

dot_product - Best performance for normalized vectors (e.g., OpenAI embeddings).

More details about each in the Elasticsearch Documentation on dense vectors.

You can leverage the generic, portable metadata filters with Elasticsearch as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Elasticsearch filter format:

Instead of using the Spring Boot auto-configuration, you can manually configure the Elasticsearch vector store. For this you need to add the spring-ai-elasticsearch-store to your project:

or to your Gradle build.gradle build file.

Create an Elasticsearch RestClient bean. Read the Elasticsearch Documentation for more in-depth information about the configuration of a custom RestClient.

Then create the ElasticsearchVectorStore bean using the builder pattern:

The Elasticsearch Vector Store implementation provides access to the underlying native Elasticsearch client (ElasticsearchClient) through the getNativeClient() method:

The native client gives you access to Elasticsearch-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-elasticsearch</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-elasticsearch'
}
```

Example 3 (xml):
```xml
<dependency>
    <groupId>co.elastic.clients</groupId>
    <artifactId>elasticsearch-java</artifactId>
    <version>8.13.3</version>
</dependency>
```

Example 4 (json):
```json
dependencies {
    implementation 'co.elastic.clients:elasticsearch-java:8.13.3'
}
```

---

## Elasticsearch :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/elasticsearch.html

**Contents:**
- Elasticsearch
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Metadata Filtering
- Manual Configuration
- Accessing the Native Client

This section walks you through setting up the Elasticsearch VectorStore to store document embeddings and perform similarity searches.

Elasticsearch is an open source search and analytics engine based on the Apache Lucene library.

A running Elasticsearch instance. The following options are available:

Self-Managed Elasticsearch

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Elasticsearch Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml or Gradle build.gradle build files:

For spring-boot versions pre 3.3.0 it’s necessary to explicitly add the elasticsearch-java dependency with version > 8.13.3, otherwise the older version used will be incompatible with the queries performed:

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file. Alternatively you can opt-out the initialization and create the index manually using the Elasticsearch client, which can be useful if the index needs advanced mapping or additional configuration.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options. These properties can be also set by configuring the ElasticsearchVectorStoreOptions bean.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the ElasticsearchVectorStore as a vector store in your application.

To connect to Elasticsearch and use the ElasticsearchVectorStore, you need to provide access details for your instance. A simple configuration can either be provided via Spring Boot’s application.yml,

The Spring Boot properties starting with spring.elasticsearch.* are used to configure the Elasticsearch client:

spring.elasticsearch.connection-timeout

Connection timeout used when communicating with Elasticsearch.

spring.elasticsearch.password

Password for authentication with Elasticsearch.

spring.elasticsearch.username

Username for authentication with Elasticsearch.

spring.elasticsearch.uris

Comma-separated list of the Elasticsearch instances to use.

http://localhost:9200

spring.elasticsearch.path-prefix

Prefix added to the path of every request sent to Elasticsearch.

spring.elasticsearch.restclient.sniffer.delay-after-failure

Delay of a sniff execution scheduled after a failure.

spring.elasticsearch.restclient.sniffer.interval

Interval between consecutive ordinary sniff executions.

spring.elasticsearch.restclient.ssl.bundle

spring.elasticsearch.socket-keep-alive

Whether to enable socket keep alive between client and Elasticsearch.

spring.elasticsearch.socket-timeout

Socket timeout used when communicating with Elasticsearch.

Properties starting with spring.ai.vectorstore.elasticsearch.* are used to configure the ElasticsearchVectorStore:

spring.ai.vectorstore.elasticsearch.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.elasticsearch.index-name

The name of the index to store the vectors

spring-ai-document-index

spring.ai.vectorstore.elasticsearch.dimensions

The number of dimensions in the vector

spring.ai.vectorstore.elasticsearch.similarity

The similarity function to use

spring.ai.vectorstore.elasticsearch.embedding-field-name

The name of the vector field to search against

The following similarity functions are available:

cosine - Default, suitable for most use cases. Measures cosine similarity between vectors.

l2_norm - Euclidean distance between vectors. Lower values indicate higher similarity.

dot_product - Best performance for normalized vectors (e.g., OpenAI embeddings).

More details about each in the Elasticsearch Documentation on dense vectors.

You can leverage the generic, portable metadata filters with Elasticsearch as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Elasticsearch filter format:

Instead of using the Spring Boot auto-configuration, you can manually configure the Elasticsearch vector store. For this you need to add the spring-ai-elasticsearch-store to your project:

or to your Gradle build.gradle build file.

Create an Elasticsearch RestClient bean. Read the Elasticsearch Documentation for more in-depth information about the configuration of a custom RestClient.

Then create the ElasticsearchVectorStore bean using the builder pattern:

The Elasticsearch Vector Store implementation provides access to the underlying native Elasticsearch client (ElasticsearchClient) through the getNativeClient() method:

The native client gives you access to Elasticsearch-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-elasticsearch</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-elasticsearch'
}
```

Example 3 (xml):
```xml
<dependency>
    <groupId>co.elastic.clients</groupId>
    <artifactId>elasticsearch-java</artifactId>
    <version>8.13.3</version>
</dependency>
```

Example 4 (json):
```json
dependencies {
    implementation 'co.elastic.clients:elasticsearch-java:8.13.3'
}
```

---

## GemFire Vector Store :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/gemfire.html

**Contents:**
- GemFire Vector Store
- Prerequisites
- Auto-configuration
  - Configuration properties
- Manual Configuration
- Usage
- Metadata Filtering

This section walks you through setting up the GemFireVectorStore to store document embeddings and perform similarity searches.

GemFire is a distributed, in-memory, key-value store performing read and write operations at blazingly fast speeds. It offers highly available parallel message queues, continuous availability, and an event-driven architecture you can scale dynamically without downtime. As your data size requirements increase to support high-performance, real-time apps, GemFire can easily scale linearly.

GemFire VectorDB extends GemFire’s capabilities, serving as a versatile vector database that efficiently stores, retrieves, and performs vector similarity searches.

A GemFire cluster with the GemFire VectorDB extension enabled

Install GemFire VectorDB extension

An EmbeddingModel bean to compute the document embeddings. Refer to the EmbeddingModel section for more information. An option that runs locally on your machine is ONNX and the all-MiniLM-L6-v2 Sentence Transformers.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add the GemFire VectorStore Spring Boot starter to you project’s Maven build file pom.xml:

or to your Gradle build.gradle file

You can use the following properties in your Spring Boot configuration to further configure the GemFireVectorStore.

spring.ai.vectorstore.gemfire.host

spring.ai.vectorstore.gemfire.port

spring.ai.vectorstore.gemfire.initialize-schema

spring.ai.vectorstore.gemfire.index-name

spring-ai-gemfire-store

spring.ai.vectorstore.gemfire.beam-width

spring.ai.vectorstore.gemfire.max-connections

spring.ai.vectorstore.gemfire.vector-similarity-function

spring.ai.vectorstore.gemfire.fields

spring.ai.vectorstore.gemfire.buckets

spring.ai.vectorstore.gemfire.username

spring.ai.vectorstore.gemfire.password

spring.ai.vectorstore.gemfire.token

To use just the GemFireVectorStore, without Spring Boot’s Auto-configuration add the following dependency to your project’s Maven pom.xml:

For Gradle users, add the following to your build.gradle file under the dependencies block to use just the GemFireVectorStore:

Here is a sample that creates an instance of the GemfireVectorStore instead of using AutoConfiguration

The default configuration connects to a GemFire cluster at localhost:8080

In your application, create a few documents:

Add the documents to the vector store:

And to retrieve documents using similarity search:

You should retrieve the document containing the text "Spring AI rocks!!".

You can also limit the number of results using a similarity threshold:

You can leverage the generic, portable metadata filters with GemFire VectorStore as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary GemFire VectorDB filter format:

The GemFire VectorStore supports a wide range of filter operations:

Equality: country == 'BG' → country:BG

Inequality: city != 'Sofia' → city: NOT Sofia

Greater Than: year > 2020 → year:{2020 TO *]

Greater Than or Equal: year >= 2020 → year:[2020 TO *]

Less Than: year < 2025 → year:[* TO 2025}

Less Than or Equal: year ⇐ 2025 → year:[* TO 2025]

IN: country in ['BG', 'NL'] → country:(BG OR NL)

NOT IN: country nin ['BG', 'NL'] → NOT country:(BG OR NL)

AND/OR: Logical operators for combining conditions

Grouping: Use parentheses for complex expressions

Date Filtering: Date values in ISO 8601 format (e.g., 2024-01-07T14:29:12Z)

To use metadata filtering with GemFire VectorStore, you must specify the metadata fields that can be filtered when creating the vector store. This is done using the fields parameter in the builder:

Or via configuration properties:

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-gemfire</artifactId>
</dependency>
```

Example 2 (xml):
```xml
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-gemfire'
}
```

Example 3 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-gemfire-store</artifactId>
</dependency>
```

Example 4 (java):
```java
@Bean
public GemFireVectorStore vectorStore(EmbeddingModel embeddingModel) {
    return GemFireVectorStore.builder(embeddingModel)
        .host("localhost")
        .port(7071)
        .username("my-user-name")
        .password("my-password")
        .indexName("my-vector-index")
        .fields(new String[] {"country", "year", "activationDate"}) // Optional: fields for metadata filtering
        .initializeSchema(true)
        .build();
}
```

---

## Vector Databases :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs.html

**Contents:**
- Vector Databases
- API Overview
  - VectorStoreRetriever Interface
  - VectorStore Interface
  - SearchRequest Builder
- Schema Initialization
- Batching Strategy
  - Default Implementation
  - Working with Auto-Truncation
    - Configuration for Auto-Truncation

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

A vector database is a specialized type of database that plays an essential role in AI applications.

In vector databases, queries differ from traditional relational databases. Instead of exact matches, they perform similarity searches. When given a vector as a query, a vector database returns vectors that are “similar” to the query vector. Further details on how this similarity is calculated at a high-level is provided in a Vector Similarity.

Vector databases are used to integrate your data with AI models. The first step in their usage is to load your data into a vector database. Then, when a user query is to be sent to the AI model, a set of similar documents is first retrieved. These documents then serve as the context for the user’s question and are sent to the AI model, along with the user’s query. This technique is known as Retrieval Augmented Generation (RAG).

The following sections describe the Spring AI interface for using multiple vector database implementations and some high-level sample usage.

The last section is intended to demystify the underlying approach of similarity searching in vector databases.

This section serves as a guide to the VectorStore interface and its associated classes within the Spring AI framework.

Spring AI offers an abstracted API for interacting with vector databases through the VectorStore interface and its read-only counterpart, the VectorStoreRetriever interface.

Spring AI provides a read-only interface called VectorStoreRetriever that exposes only the document retrieval functionality:

This functional interface is designed for use cases where you only need to retrieve documents from a vector store without performing any mutation operations. It follows the principle of least privilege by exposing only the necessary functionality for document retrieval.

The VectorStore interface extends VectorStoreRetriever and adds mutation capabilities:

The VectorStore interface combines both read and write operations, allowing you to add, delete, and search for documents in a vector database.

To insert data into the vector database, encapsulate it within a Document object. The Document class encapsulates content from a data source, such as a PDF or Word document, and includes text represented as a string. It also contains metadata in the form of key-value pairs, including details such as the filename.

Upon insertion into the vector database, the text content is transformed into a numerical array, or a float[], known as vector embeddings, using an embedding model. Embedding models, such as Word2Vec, GLoVE, and BERT, or OpenAI’s text-embedding-ada-002, are used to convert words, sentences, or paragraphs into these vector embeddings.

The vector database’s role is to store and facilitate similarity searches for these embeddings. It does not generate the embeddings itself. For creating vector embeddings, the EmbeddingModel should be utilized.

The similaritySearch methods in the interface allow for retrieving documents similar to a given query string. These methods can be fine-tuned by using the following parameters:

k: An integer that specifies the maximum number of similar documents to return. This is often referred to as a 'top K' search, or 'K nearest neighbors' (KNN).

threshold: A double value ranging from 0 to 1, where values closer to 1 indicate higher similarity. By default, if you set a threshold of 0.75, for instance, only documents with a similarity above this value are returned.

Filter.Expression: A class used for passing a fluent DSL (Domain-Specific Language) expression that functions similarly to a 'where' clause in SQL, but it applies exclusively to the metadata key-value pairs of a Document.

filterExpression: An external DSL based on ANTLR4 that accepts filter expressions as strings. For example, with metadata keys like country, year, and isActive, you could use an expression such as: country == 'UK' && year >= 2020 && isActive == true.

Find more information on the Filter.Expression in the Metadata Filters section.

Some vector stores require their backend schema to be initialized before usage. It will not be initialized for you by default. You must opt-in, by passing a boolean for the appropriate constructor argument or, if using Spring Boot, setting the appropriate initialize-schema property to true in application.properties or application.yml. Check the documentation for the vector store you are using for the specific property name.

When working with vector stores, it’s often necessary to embed large numbers of documents. While it might seem straightforward to make a single call to embed all documents at once, this approach can lead to issues. Embedding models process text as tokens and have a maximum token limit, often referred to as the context window size. This limit restricts the amount of text that can be processed in a single embedding request. Attempting to embed too many tokens in one call can result in errors or truncated embeddings.

To address this token limit, Spring AI implements a batching strategy. This approach breaks down large sets of documents into smaller batches that fit within the embedding model’s maximum context window. Batching not only solves the token limit issue but can also lead to improved performance and more efficient use of API rate limits.

Spring AI provides this functionality through the BatchingStrategy interface, which allows for processing documents in sub-batches based on their token counts.

The core BatchingStrategy interface is defined as follows:

This interface defines a single method, batch, which takes a list of documents and returns a list of document batches.

Spring AI provides a default implementation called TokenCountBatchingStrategy. This strategy batches documents based on their token counts, ensuring that each batch does not exceed a calculated maximum input token count.

Key features of TokenCountBatchingStrategy:

Uses OpenAI’s max input token count (8191) as the default upper limit.

Incorporates a reserve percentage (default 10%) to provide a buffer for potential overhead.

Calculates the actual max input token count as: actualMaxInputTokenCount = originalMaxInputTokenCount * (1 - RESERVE_PERCENTAGE)

The strategy estimates the token count for each document, groups them into batches without exceeding the max input token count, and throws an exception if a single document exceeds this limit.

You can also customize the TokenCountBatchingStrategy to better suit your specific requirements. This can be done by creating a new instance with custom parameters in a Spring Boot @Configuration class.

Here’s an example of how to create a custom TokenCountBatchingStrategy bean:

In this configuration:

EncodingType.CL100K_BASE: Specifies the encoding type used for tokenization. This encoding type is used by the JTokkitTokenCountEstimator to accurately estimate token counts.

8000: Sets the maximum input token count. This value should be less than or equal to the maximum context window size of your embedding model.

0.1: Sets the reserve percentage. The percentage of tokens to reserve from the max input token count. This creates a buffer for potential token count increases during processing.

By default, this constructor uses Document.DEFAULT_CONTENT_FORMATTER for content formatting and MetadataMode.NONE for metadata handling. If you need to customize these parameters, you can use the full constructor with additional parameters.

Once defined, this custom TokenCountBatchingStrategy bean will be automatically used by the EmbeddingModel implementations in your application, replacing the default strategy.

The TokenCountBatchingStrategy internally uses a TokenCountEstimator (specifically, JTokkitTokenCountEstimator) to calculate token counts for efficient batching. This ensures accurate token estimation based on the specified encoding type.

Additionally, TokenCountBatchingStrategy provides flexibility by allowing you to pass in your own implementation of the TokenCountEstimator interface. This feature enables you to use custom token counting strategies tailored to your specific needs. For example:

Some embedding models, such as Vertex AI text embedding, support an auto_truncate feature. When enabled, the model silently truncates text inputs that exceed the maximum size and continues processing; when disabled, it throws an explicit error for inputs that are too large.

When using auto-truncation with the batching strategy, you must configure your batching strategy with a much higher input token count than the model’s actual maximum. This prevents the batching strategy from raising exceptions for large documents, allowing the embedding model to handle truncation internally.

When enabling auto-truncation, set your batching strategy’s maximum input token count much higher than the model’s actual limit. This prevents the batching strategy from raising exceptions for large documents, allowing the embedding model to handle truncation internally.

Here’s an example configuration for using Vertex AI with auto-truncation and custom BatchingStrategy and then using them in the PgVectorStore:

In this configuration:

The embedding model has auto-truncation enabled, allowing it to handle oversized inputs gracefully.

The batching strategy uses an artificially high token limit (132,900) that’s much larger than the actual model limit (20,000).

The vector store uses the configured embedding model and the custom BatchingStrategy bean.

This approach works because:

The TokenCountBatchingStrategy checks if any single document exceeds the configured maximum and throws an IllegalArgumentException if it does.

By setting a very high limit in the batching strategy, we ensure that this check never fails.

Documents or batches exceeding the model’s limit are silently truncated and processed by the embedding model’s auto-truncation feature.

When using auto-truncation:

Set the batching strategy’s max input token count to be at least 5-10x larger than the model’s actual limit to avoid premature exceptions from the batching strategy.

Monitor your logs for truncation warnings from the embedding model (note: not all models log truncation events).

Consider the implications of silent truncation on your embedding quality.

Test with sample documents to ensure truncated embeddings still meet your requirements.

Document this configuration for future maintainers, as it is non-standard.

If you’re using Spring Boot auto-configuration, you must provide a custom BatchingStrategy bean to override the default one that comes with Spring AI:

The presence of this bean in your application context will automatically replace the default batching strategy used by all vector stores.

While TokenCountBatchingStrategy provides a robust default implementation, you can customize the batching strategy to fit your specific needs. This can be done through Spring Boot’s auto-configuration.

To customize the batching strategy, define a BatchingStrategy bean in your Spring Boot application:

This custom BatchingStrategy will then be automatically used by the EmbeddingModel implementations in your application.

These are the available implementations of the VectorStore interface:

Azure Vector Search - The Azure vector store.

Apache Cassandra - The Apache Cassandra vector store.

Chroma Vector Store - The Chroma vector store.

Elasticsearch Vector Store - The Elasticsearch vector store.

GemFire Vector Store - The GemFire vector store.

MariaDB Vector Store - The MariaDB vector store.

Milvus Vector Store - The Milvus vector store.

MongoDB Atlas Vector Store - The MongoDB Atlas vector store.

Neo4j Vector Store - The Neo4j vector store.

OpenSearch Vector Store - The OpenSearch vector store.

Oracle Vector Store - The Oracle Database vector store.

PgVector Store - The PostgreSQL/PGVector vector store.

Pinecone Vector Store - Pinecone vector store.

Qdrant Vector Store - Qdrant vector store.

Redis Vector Store - The Redis vector store.

SAP Hana Vector Store - The SAP HANA vector store.

Typesense Vector Store - The Typesense vector store.

Weaviate Vector Store - The Weaviate vector store.

SimpleVectorStore - A simple implementation of persistent vector storage, good for educational purposes.

More implementations may be supported in future releases.

If you have a vector database that needs to be supported by Spring AI, open an issue on GitHub or, even better, submit a pull request with an implementation.

Information on each of the VectorStore implementations can be found in the subsections of this chapter.

To compute the embeddings for a vector database, you need to pick an embedding model that matches the higher-level AI model being used.

For example, with OpenAI’s ChatGPT, we use the OpenAiEmbeddingModel and a model named text-embedding-ada-002.

The Spring Boot starter’s auto-configuration for OpenAI makes an implementation of EmbeddingModel available in the Spring application context for dependency injection.

The general usage of loading data into a vector store is something you would do in a batch-like job, by first loading data into Spring AI’s Document class and then calling the add method on the VectorStore interface.

Given a String reference to a source file that represents a JSON file with data we want to load into the vector database, we use Spring AI’s JsonReader to load specific fields in the JSON, which splits them up into small pieces and then passes those small pieces to the vector store implementation. The VectorStore implementation computes the embeddings and stores the JSON and the embedding in the vector database:

Later, when a user question is passed into the AI model, a similarity search is done to retrieve similar documents, which are then "stuffed" into the prompt as context for the user’s question.

For read-only operations, you can use either the VectorStore interface or the more focused VectorStoreRetriever interface:

Additional options can be passed into the similaritySearch method to define how many documents to retrieve and a threshold of the similarity search.

Using the separate interfaces allows you to clearly define which components need write access and which only need read access:

This separation of concerns helps create more maintainable and secure applications by limiting access to mutation operations only to components that truly need them.

The VectorStoreRetriever interface provides a read-only view of a vector store, exposing only the similarity search functionality. This follows the principle of least privilege and is particularly useful in RAG (Retrieval-Augmented Generation) applications where you only need to retrieve documents without modifying the underlying data.

Separation of Concerns: Clearly separates read operations from write operations.

Interface Segregation: Clients that only need retrieval functionality aren’t exposed to mutation methods.

Functional Interface: Can be implemented with lambda expressions or method references for simple use cases.

Reduced Dependencies: Components that only need to perform searches don’t need to depend on the full VectorStore interface.

You can use VectorStoreRetriever directly when you only need to perform similarity searches:

In this example, the service only depends on the VectorStoreRetriever interface, making it clear that it only performs retrieval operations and doesn’t modify the vector store.

The VectorStoreRetriever interface is particularly useful in RAG applications, where you need to retrieve relevant documents to provide context for an AI model:

This pattern allows for a clean separation between the retrieval component and the generation component in RAG applications.

This section describes various filters that you can use against the results of a query.

You can pass in an SQL-like filter expressions as a String to one of the similaritySearch overloads.

Consider the following examples:

"genre == 'drama' && year >= 2020"

"genre in ['comedy', 'documentary', 'drama']"

You can create an instance of Filter.Expression with a FilterExpressionBuilder that exposes a fluent API. A simple example is as follows:

You can build up sophisticated expressions by using the following operators:

You can combine expressions by using the following operators:

Considering the following example:

You can also use the following operators:

Consider the following example:

You can also use the following operators:

Consider the following example:

The Vector Store interface provides multiple methods for deleting documents, allowing you to remove data either by specific document IDs or using filter expressions.

The simplest way to delete documents is by providing a list of document IDs:

This method removes all documents whose IDs match those in the provided list. If any ID in the list doesn’t exist in the store, it will be ignored.

For more complex deletion criteria, you can use filter expressions:

This method accepts a Filter.Expression object that defines the criteria for which documents should be deleted. It’s particularly useful when you need to delete documents based on their metadata properties.

For convenience, you can also delete documents using a string-based filter expression:

This method converts the provided string filter into a Filter.Expression object internally. It’s useful when you have filter criteria in string format.

All deletion methods may throw exceptions in case of errors:

The best practice is to wrap delete operations in try-catch blocks:

A common scenario is managing document versions where you need to upload a new version of a document while removing the old version. Here’s how to handle this using filter expressions:

You can also accomplish the same using the string filter expression:

Deleting by ID list is generally faster when you know exactly which documents to remove.

Filter-based deletion may require scanning the index to find matching documents; however, this is vector store implementation-specific.

Large deletion operations should be batched to avoid overwhelming the system.

Consider using filter expressions when deleting based on document properties rather than collecting IDs first.

Understanding Vectors

**Examples:**

Example 1 (java):
```java
@FunctionalInterface
public interface VectorStoreRetriever {

    List<Document> similaritySearch(SearchRequest request);

    default List<Document> similaritySearch(String query) {
        return this.similaritySearch(SearchRequest.builder().query(query).build());
    }
}
```

Example 2 (java):
```java
public interface VectorStore extends DocumentWriter, VectorStoreRetriever {

    default String getName() {
		return this.getClass().getSimpleName();
	}

    void add(List<Document> documents);

    void delete(List<String> idList);

    void delete(Filter.Expression filterExpression);

    default void delete(String filterExpression) { ... }

    default <T> Optional<T> getNativeClient() {
		return Optional.empty();
	}
}
```

Example 3 (java):
```java
public class SearchRequest {

	public static final double SIMILARITY_THRESHOLD_ACCEPT_ALL = 0.0;

	public static final int DEFAULT_TOP_K = 4;

	private String query = "";

	private int topK = DEFAULT_TOP_K;

	private double similarityThreshold = SIMILARITY_THRESHOLD_ACCEPT_ALL;

	@Nullable
	private Filter.Expression filterExpression;

    public static Builder from(SearchRequest originalSearchRequest) {
		return builder().query(originalSearchRequest.getQuery())
			.topK(originalSearchRequest.getTopK())
			.similarityThreshold(originalSearchRequest.getSimilarityThreshold())
			.filterExpression(originalSearchRequest.getFilterExpression());
	}

	public static class Builder {

		private final SearchRequest searchRequest = new SearchRequest();

		public Builder query(String query) {
			Assert.notNull(query, "Query can not be null.");
			this.searchRequest.query = query;
			return this;
		}

		public Builder topK(int topK) {
			Assert.isTrue(topK >= 0, "TopK should be positive.");
			this.searchRequest.topK = topK;
			return this;
		}

		public Builder similarityThreshold(double threshold) {
			Assert.isTrue(threshold >= 0 && threshold <= 1, "Similarity threshold must be in [0,1] range.");
			this.searchRequest.similarityThreshold = threshold;
			return this;
		}

		public Builder similarityThresholdAll() {
			this.searchRequest.similarityThreshold = 0.0;
			return this;
		}

		public Builder filterExpression(@Nullable Filter.Expression expression) {
			this.searchRequest.filterExpression = expression;
			return this;
		}

		public Builder filterExpression(@Nullable String textExpression) {
			this.searchRequest.filterExpression = (textExpression != null)
					? new FilterExpressionTextParser().parse(textExpression) : null;
			return this;
		}

		public SearchRequest build() {
			return this.searchRequest;
		}

	}

	public String getQuery() {...}
	public int getTopK() {...}
	public double getSimilarityThreshold() {...}
	public Filter.Expression getFilterExpression() {...}
}
```

Example 4 (java):
```java
public interface BatchingStrategy {
    List<List<Document>> batch(List<Document> documents);
}
```

---

## Milvus :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/milvus.html

**Contents:**
- Milvus
- Prerequisites
- Dependencies
  - Manual Configuration
- Metadata filtering
- Using MilvusSearchRequest
- Importance of nativeExpression and searchParamsJson in MilvusSearchRequest
- Milvus VectorStore properties
- Starting Milvus Store
- Troubleshooting

For the latest snapshot version, please use Spring AI 1.1.2!

Milvus is an open-source vector database that has garnered significant attention in the fields of data science and machine learning. One of its standout features lies in its robust support for vector indexing and querying. Milvus employs state-of-the-art, cutting-edge algorithms to accelerate the search process, making it exceptionally efficient at retrieving similar vectors, even when handling extensive datasets.

A running Milvus instance. The following options are available:

Milvus Standalone: Docker, Operator, Helm,DEB/RPM, Docker Compose.

Milvus Cluster: Operator, Helm.

If required, an API key for the EmbeddingModel to generate the embeddings stored by the MilvusVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Then add the Milvus VectorStore boot starter dependency to your project:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

The Vector Store, also requires an EmbeddingModel instance to calculate embeddings for the documents. You can pick one of the available EmbeddingModel Implementations.

To connect to and configure the MilvusVectorStore, you need to provide access details for your instance. A simple configuration can either be provided via Spring Boot’s application.yml

Now you can Auto-wire the Milvus Vector Store in your application and use it

Instead of using the Spring Boot auto-configuration, you can manually configure the MilvusVectorStore. To add the following dependencies to your project:

To configure MilvusVectorStore in your application, you can use the following setup:

You can leverage the generic, portable metadata filters with the Milvus store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

MilvusSearchRequest extends SearchRequest, allowing you to use Milvus-specific search parameters such as native expressions and search parameter JSON.

This allows greater flexibility when using Milvus-specific search features.

These two parameters enhance Milvus search precision and ensure optimal query performance:

nativeExpression: Enables additional filtering capabilities using Milvus' native filtering expressions. Milvus Filtering

searchParamsJson: Essential for tuning search behavior when using IVF_FLAT, Milvus' default index. Milvus Vector Index

By default, IVF_FLAT requires nprobe to be set for accurate results. If not specified, nprobe defaults to 1, which can lead to poor recall or even zero search results.

Using nativeExpression ensures advanced filtering, while searchParamsJson prevents ineffective searches caused by a low default nprobe value.

You can use the following properties in your Spring Boot configuration to customize the Milvus vector store.

spring.ai.vectorstore.milvus.database-name

The name of the Milvus database to use.

spring.ai.vectorstore.milvus.collection-name

Milvus collection name to store the vectors

spring.ai.vectorstore.milvus.initialize-schema

whether to initialize Milvus' backend

spring.ai.vectorstore.milvus.embedding-dimension

The dimension of the vectors to be stored in the Milvus collection.

spring.ai.vectorstore.milvus.index-type

The type of the index to be created for the Milvus collection.

spring.ai.vectorstore.milvus.metric-type

The metric type to be used for the Milvus collection.

spring.ai.vectorstore.milvus.index-parameters

The index parameters to be used for the Milvus collection.

spring.ai.vectorstore.milvus.id-field-name

The ID field name for the collection

spring.ai.vectorstore.milvus.auto-id

Boolean flag to indicate if the auto-id is used for the ID field

spring.ai.vectorstore.milvus.content-field-name

The content field name for the collection

spring.ai.vectorstore.milvus.metadata-field-name

The metadata field name for the collection

spring.ai.vectorstore.milvus.embedding-field-name

The embedding field name for the collection

spring.ai.vectorstore.milvus.client.host

The name or address of the host.

spring.ai.vectorstore.milvus.client.port

spring.ai.vectorstore.milvus.client.uri

The uri of Milvus instance

spring.ai.vectorstore.milvus.client.token

Token serving as the key for identification and authentication purposes.

spring.ai.vectorstore.milvus.client.connect-timeout-ms

Connection timeout value of client channel. The timeout value must be greater than zero .

spring.ai.vectorstore.milvus.client.keep-alive-time-ms

Keep-alive time value of client channel. The keep-alive value must be greater than zero.

spring.ai.vectorstore.milvus.client.keep-alive-timeout-ms

The keep-alive timeout value of client channel. The timeout value must be greater than zero.

spring.ai.vectorstore.milvus.client.rpc-deadline-ms

Deadline for how long you are willing to wait for a reply from the server. With a deadline setting, the client will wait when encounter fast RPC fail caused by network fluctuations. The deadline value must be larger than or equal to zero.

spring.ai.vectorstore.milvus.client.client-key-path

The client.key path for tls two-way authentication, only takes effect when "secure" is true

spring.ai.vectorstore.milvus.client.client-pem-path

The client.pem path for tls two-way authentication, only takes effect when "secure" is true

spring.ai.vectorstore.milvus.client.ca-pem-path

The ca.pem path for tls two-way authentication, only takes effect when "secure" is true

spring.ai.vectorstore.milvus.client.server-pem-path

server.pem path for tls one-way authentication, only takes effect when "secure" is true.

spring.ai.vectorstore.milvus.client.server-name

Sets the target name override for SSL host name checking, only takes effect when "secure" is True. Note: this value is passed to grpc.ssl_target_name_override

spring.ai.vectorstore.milvus.client.secure

Secure the authorization for this connection, set to True to enable TLS.

spring.ai.vectorstore.milvus.client.idle-timeout-ms

Idle timeout value of client channel. The timeout value must be larger than zero.

spring.ai.vectorstore.milvus.client.username

The username and password for this connection.

spring.ai.vectorstore.milvus.client.password

The password for this connection.

From within the src/test/resources/ folder run:

To clean the environment:

Then connect to the vector store on http://localhost:19530 or for management http://localhost:9001 (user: minioadmin, pass: minioadmin)

If Docker complains about resources, then execute:

The Milvus Vector Store implementation provides access to the underlying native Milvus client (MilvusServiceClient) through the getNativeClient() method:

The native client gives you access to Milvus-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-vector-store-milvus</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-milvus'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List <Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Milvus Vector Store
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = this.vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-milvus-store</artifactId>
</dependency>
```

---

## Weaviate :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/weaviate.html

**Contents:**
- Weaviate
- Prerequisites
- Dependencies
- Configuration
  - Option 1: Using Spring Expression Language (SpEL)
  - Option 2: Accessing Environment Variables Programmatically
- Auto-configuration
- Manual Configuration
- Metadata filtering
- Run Weaviate in Docker

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up the Weaviate VectorStore to store document embeddings and perform similarity searches.

Weaviate is an open-source vector database that allows you to store data objects and vector embeddings from your favorite ML-models and scale seamlessly into billions of data objects. It provides tools to store document embeddings, content, and metadata and to search through those embeddings, including metadata filtering.

A running Weaviate instance. The following options are available:

Weaviate Cloud Service (requires account creation and API key)

If required, an API key for the EmbeddingModel to generate the embeddings stored by the WeaviateVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add the Weaviate Vector Store dependency to your project:

or to your Gradle build.gradle build file.

To connect to Weaviate and use the WeaviateVectorStore, you need to provide access details for your instance. Configuration can be provided via Spring Boot’s application.properties:

If you prefer to use environment variables for sensitive information like API keys, you have multiple options:

You can use custom environment variable names and reference them in your application configuration:

Alternatively, you can access environment variables in your Java code:

Spring AI provides Spring Boot auto-configuration for the Weaviate Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Here is an example of the required bean:

Now you can auto-wire the WeaviateVectorStore as a vector store in your application.

Instead of using Spring Boot auto-configuration, you can manually configure the WeaviateVectorStore using the builder pattern:

You can leverage the generic, portable metadata filters with Weaviate store as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Weaviate GraphQL filter format:

To quickly get started with a local Weaviate instance, you can run it in Docker:

This starts a Weaviate instance accessible at localhost:8080.

You can use the following properties in your Spring Boot configuration to customize the Weaviate vector store.

spring.ai.vectorstore.weaviate.host

The host of the Weaviate server

spring.ai.vectorstore.weaviate.scheme

spring.ai.vectorstore.weaviate.api-key

The API key for authentication

spring.ai.vectorstore.weaviate.object-class

The class name for storing documents.

spring.ai.vectorstore.weaviate.content-field-name

The field name for content

spring.ai.vectorstore.weaviate.meta-field-prefix

The field prefix for metadata

spring.ai.vectorstore.weaviate.consistency-level

Desired tradeoff between consistency and speed

spring.ai.vectorstore.weaviate.filter-field

Configures metadata fields that can be used in filters. Format: spring.ai.vectorstore.weaviate.filter-field.<field-name>=<field-type>

The Weaviate Vector Store implementation provides access to the underlying native Weaviate client (WeaviateClient) through the getNativeClient() method:

The native client gives you access to Weaviate-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-weaviate-store</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-weaviate-store'
}
```

Example 3 (markdown):
```markdown
spring.ai.vectorstore.weaviate.host=<host_of_your_weaviate_instance>
spring.ai.vectorstore.weaviate.scheme=<http_or_https>
spring.ai.vectorstore.weaviate.api-key=<your_api_key>
# API key if needed, e.g. OpenAI
spring.ai.openai.api-key=<api-key>
```

Example 4 (yaml):
```yaml
# In application.yml
spring:
  ai:
    vectorstore:
      weaviate:
        host: ${WEAVIATE_HOST}
        scheme: ${WEAVIATE_SCHEME}
        api-key: ${WEAVIATE_API_KEY}
    openai:
      api-key: ${OPENAI_API_KEY}
```

---

## Redis :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/redis.html

**Contents:**
- Redis
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Metadata Filtering
- Manual Configuration
- Accessing the Native Client

This section walks you through setting up RedisVectorStore to store document embeddings and perform similarity searches.

Redis is an open source (BSD licensed), in-memory data structure store used as a database, cache, message broker, and streaming engine. Redis provides data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes, and streams.

Redis Search and Query extends the core features of Redis OSS and allows you to use Redis as a vector database:

Store vectors and the associated metadata within hashes or JSON documents

Perform vector searches

A Redis Stack instance

Redis Cloud (recommended)

Docker image redis/redis-stack:latest

EmbeddingModel instance to compute the document embeddings. Several options are available:

If required, an API key for the EmbeddingModel to generate the embeddings stored by the RedisVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Redis Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the RedisVectorStore as a vector store in your application.

To connect to Redis and use the RedisVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml,

For redis connection configuration, alternatively, a simple configuration can be provided via Spring Boot’s application.properties.

Properties starting with spring.ai.vectorstore.redis.* are used to configure the RedisVectorStore:

spring.ai.vectorstore.redis.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.redis.index-name

The name of the index to store the vectors

spring.ai.vectorstore.redis.prefix

The prefix for Redis keys

You can leverage the generic, portable metadata filters with Redis as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Redis filter format:

Instead of using the Spring Boot auto-configuration, you can manually configure the Redis vector store. For this you need to add the spring-ai-redis-store to your project:

or to your Gradle build.gradle build file.

Create a JedisPooled bean:

Then create the RedisVectorStore bean using the builder pattern:

You must list explicitly all metadata field names and types (TAG, TEXT, or NUMERIC) for any metadata field used in filter expressions. The metadataFields above registers filterable metadata fields: country of type TAG, year of type NUMERIC.

The Redis Vector Store implementation provides access to the underlying native Redis client (JedisPooled) through the getNativeClient() method:

The native client gives you access to Redis-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-redis</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-redis'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List <Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Redis
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = this.vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  data:
    redis:
      url: <redis instance url>
  ai:
    vectorstore:
      redis:
        initialize-schema: true
        index-name: custom-index
        prefix: custom-prefix
```

---

## Oracle Database 23ai - AI Vector Search :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/oracle.html

**Contents:**
- Oracle Database 23ai - AI Vector Search
- Auto-Configuration
  - Configuration properties
- Metadata filtering
- Manual Configuration
- Run Oracle Database 23ai locally
- Accessing the Native Client

The AI Vector Search capabilities of the Oracle Database 23ai (23.4+) are available as a Spring AI VectorStore to help you to store document embeddings and perform similarity searches. Of course, all other features are also available.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Start by adding the Oracle Vector Store boot starter dependency to your project:

or to your Gradle build.gradle build file.

If you need this vector store to initialize the schema for you then you’ll need to pass true for the initializeSchema boolean parameter in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

The Vector Store, also requires an EmbeddingModel instance to calculate embeddings for the documents. You can pick one of the available EmbeddingModel Implementations.

For example to use the OpenAI EmbeddingModel add the following dependency to your project:

or to your Gradle build.gradle build file.

To connect to and configure the OracleVectorStore, you need to provide access details for your database. A simple configuration can either be provided via Spring Boot’s application.yml

Now you can Auto-wire the OracleVectorStore in your application and use it:

You can use the following properties in your Spring Boot configuration to customize the OracleVectorStore.

spring.ai.vectorstore.oracle.index-type

Nearest neighbor search index type. Options are NONE - exact nearest neighbor search, IVF - Inverted Flat File index. It has faster build times and uses less memory than HNSW, but has lower query performance (in terms of speed-recall tradeoff). HNSW - creates a multilayer graph. It has slower build times and uses more memory than IVF, but has better query performance (in terms of speed-recall tradeoff).

spring.ai.vectorstore.oracle.distance-type

Search distance type among COSINE (default), DOT, EUCLIDEAN, EUCLIDEAN_SQUARED, and MANHATTAN.

NOTE: If vectors are normalized, you can use DOT or COSINE for best performance.

spring.ai.vectorstore.oracle.forced-normalization

Allows enabling vector normalization (if true) before insertion and for similarity search.

CAUTION: Setting this to true is a requirement to allow for search request similarity threshold.

NOTE: If vectors are normalized, you can use DOT or COSINE for best performance.

spring.ai.vectorstore.oracle.dimensions

Embeddings dimension. If not specified explicitly the OracleVectorStore will allow the maximum: 65535. Dimensions are set to the embedding column on table creation. If you change the dimensions your would have to re-create the table as well.

spring.ai.vectorstore.oracle.remove-existing-vector-store-table

Drops the existing table on start up.

spring.ai.vectorstore.oracle.initialize-schema

Whether to initialize the required schema.

spring.ai.vectorstore.oracle.search-accuracy

Denote the requested accuracy target in the presence of index. Disabled by default. You need to provide an integer in the range [1,100] to override the default index accuracy (95). Using lower accuracy provides approximate similarity search trading off speed versus accuracy.

-1 (DEFAULT_SEARCH_ACCURACY)

You can leverage the generic, portable metadata filters with the OracleVectorStore.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

Instead of using the Spring Boot auto-configuration, you can manually configure the OracleVectorStore. For this you need to add the Oracle JDBC driver and JdbcTemplate auto-configuration dependencies to your project:

To configure the OracleVectorStore in your application, you can use the following setup:

You can then connect to the database using:

The Oracle Vector Store implementation provides access to the underlying native Oracle client (OracleConnection) through the getNativeClient() method:

The native client gives you access to Oracle-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-vector-store-oracle</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-oracle'
}
```

Example 3 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

Example 4 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-openai'
}
```

---

## Azure Cosmos DB :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/azure-cosmos-db.html

**Contents:**
- Azure Cosmos DB
- What is Azure Cosmos DB?
- What is DiskANN?
- Setting up Azure Cosmos DB Vector Store with Auto Configuration
- Auto Configuration
- Configuration Properties
- Complex Searches with Filters
- Setting up Azure Cosmos DB Vector Store without Auto Configuration
- Manual Dependency Setup
- Accessing the Native Client

For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up CosmosDBVectorStore to store document embeddings and perform similarity searches.

Azure Cosmos DB is Microsoft’s globally distributed cloud-native database service designed for mission-critical applications. It offers high availability, low latency, and the ability to scale horizontally to meet modern application demands. It was built from the ground up with global distribution, fine-grained multi-tenancy, and horizontal scalability at its core. It is a foundational service in Azure, used by most of Microsoft’s mission critical applications at global scale, including Teams, Skype, Xbox Live, Office 365, Bing, Azure Active Directory, Azure Portal, Microsoft Store, and many others. It is also used by thousands of external customers including OpenAI for ChatGPT and other mission-critical AI applications that require elastic scale, turnkey global distribution, and low latency and high availability across the planet.

DiskANN (Disk-based Approximate Nearest Neighbor Search) is an innovative technology used in Azure Cosmos DB to enhance the performance of vector searches. It enables efficient and scalable similarity searches across high-dimensional data by indexing embeddings stored in Cosmos DB.

DiskANN provides the following benefits:

Efficiency: By utilizing disk-based structures, DiskANN significantly reduces the time required to find nearest neighbors compared to traditional methods.

Scalability: It can handle large datasets that exceed memory capacity, making it suitable for various applications, including machine learning and AI-driven solutions.

Low Latency: DiskANN minimizes latency during search operations, ensuring that applications can retrieve results quickly even with substantial data volumes.

In the context of Spring AI for Azure Cosmos DB, vector searches will create and leverage DiskANN indexes to ensure optimal performance for similarity queries.

The following code demonstrates how to set up the CosmosDBVectorStore with auto-configuration:

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add the following dependency to your Maven project:

The following configuration properties are available for the Cosmos DB vector store:

spring.ai.vectorstore.cosmosdb.databaseName

The name of the Cosmos DB database to use.

spring.ai.vectorstore.cosmosdb.containerName

The name of the Cosmos DB container to use.

spring.ai.vectorstore.cosmosdb.partitionKeyPath

The path for the partition key.

spring.ai.vectorstore.cosmosdb.metadataFields

Comma-separated list of metadata fields.

spring.ai.vectorstore.cosmosdb.vectorStoreThroughput

The throughput for the vector store.

spring.ai.vectorstore.cosmosdb.vectorDimensions

The number of dimensions for the vectors.

spring.ai.vectorstore.cosmosdb.endpoint

The endpoint for the Cosmos DB.

spring.ai.vectorstore.cosmosdb.key

The key for the Cosmos DB (if key is not present, [DefaultAzureCredential](learn.microsoft.com/azure/developer/java/sdk/authentication/credential-chains#defaultazurecredential-overview) will be used).

You can perform more complex searches using filters in the Cosmos DB vector store. Below is a sample demonstrating how to use filters in your search queries.

The following code demonstrates how to set up the CosmosDBVectorStore without relying on auto-configuration. [DefaultAzureCredential](learn.microsoft.com/azure/developer/java/sdk/authentication/credential-chains#defaultazurecredential-overview) is recommended for authentication to Azure Cosmos DB.

This configuration shows all the available builder options:

databaseName: The name of your Cosmos DB database

containerName: The name of your container within the database

partitionKeyPath: The path for the partition key (e.g., "/id")

metadataFields: List of metadata fields that will be used for filtering

vectorStoreThroughput: The throughput (RU/s) for the vector store container

vectorDimensions: The number of dimensions for your vectors (should match your embedding model)

batchingStrategy: Strategy for batching document operations (optional)

Add the following dependency in your Maven project:

The Azure Cosmos DB Vector Store implementation provides access to the underlying native Azure Cosmos DB client (CosmosClient) through the getNativeClient() method:

The native client gives you access to Azure Cosmos DB-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (java):
```java
package com.example.demo;

import io.micrometer.observation.ObservationRegistry;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.ai.document.Document;
import org.springframework.ai.vectorstore.SearchRequest;
import org.springframework.ai.vectorstore.VectorStore;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Lazy;

import java.util.List;
import java.util.Map;
import java.util.UUID;

import static org.assertj.core.api.Assertions.assertThat;

@SpringBootApplication
@EnableAutoConfiguration
public class DemoApplication implements CommandLineRunner {

    private static final Logger log = LoggerFactory.getLogger(DemoApplication.class);

    @Lazy
    @Autowired
    private VectorStore vectorStore;

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

    @Override
    public void run(String... args) throws Exception {
        Document document1 = new Document(UUID.randomUUID().toString(), "Sample content1", Map.of("key1", "value1"));
        Document document2 = new Document(UUID.randomUUID().toString(), "Sample content2", Map.of("key2", "value2"));
		this.vectorStore.add(List.of(document1, document2));
        List<Document> results = this.vectorStore.similaritySearch(SearchRequest.builder().query("Sample content").topK(1).build());

        log.info("Search results: {}", results);

        // Remove the documents from the vector store
		this.vectorStore.delete(List.of(document1.getId(), document2.getId()));
    }

    @Bean
    public ObservationRegistry observationRegistry() {
        return ObservationRegistry.create();
    }
}
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-azure-cosmos-db</artifactId>
</dependency>
```

Example 3 (java):
```java
Map<String, Object> metadata1 = new HashMap<>();
metadata1.put("country", "UK");
metadata1.put("year", 2021);
metadata1.put("city", "London");

Map<String, Object> metadata2 = new HashMap<>();
metadata2.put("country", "NL");
metadata2.put("year", 2022);
metadata2.put("city", "Amsterdam");

Document document1 = new Document("1", "A document about the UK", this.metadata1);
Document document2 = new Document("2", "A document about the Netherlands", this.metadata2);

vectorStore.add(List.of(document1, document2));

FilterExpressionBuilder builder = new FilterExpressionBuilder();
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("The World")
    .topK(10)
    .filterExpression((this.builder.in("country", "UK", "NL")).build()).build());
```

Example 4 (java):
```java
@Bean
public VectorStore vectorStore(ObservationRegistry observationRegistry) {
    // Create the Cosmos DB client
    CosmosAsyncClient cosmosClient = new CosmosClientBuilder()
            .endpoint(System.getenv("COSMOSDB_AI_ENDPOINT"))
            .credential(new DefaultAzureCredentialBuilder().build())
            .userAgentSuffix("SpringAI-CDBNoSQL-VectorStore")
            .gatewayMode()
            .buildAsyncClient();

    // Create and configure the vector store
    return CosmosDBVectorStore.builder(cosmosClient, embeddingModel)
            .databaseName("test-database")
            .containerName("test-container")
            // Configure metadata fields for filtering
            .metadataFields(List.of("country", "year", "city"))
            // Set the partition key path (optional)
            .partitionKeyPath("/id")
            // Configure performance settings
            .vectorStoreThroughput(1000)
            .vectorDimensions(1536)  // Match your embedding model's dimensions
            // Add custom batching strategy (optional)
            .batchingStrategy(new TokenCountBatchingStrategy())
            // Add observation registry for metrics
            .observationRegistry(observationRegistry)
            .build();
}

@Bean
public EmbeddingModel embeddingModel() {
    return new TransformersEmbeddingModel();
}
```

---

## Vector Databases :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs.html

**Contents:**
- Vector Databases
- API Overview
- Schema Initialization
- Batching Strategy
  - Default Implementation
  - Working with Auto-Truncation
    - Configuration for Auto-Truncation
    - Why This Works
    - Best Practices
    - Spring Boot Auto-Configuration

For the latest snapshot version, please use Spring AI 1.1.2!

A vector database is a specialized type of database that plays an essential role in AI applications.

In vector databases, queries differ from traditional relational databases. Instead of exact matches, they perform similarity searches. When given a vector as a query, a vector database returns vectors that are “similar” to the query vector. Further details on how this similarity is calculated at a high-level is provided in a Vector Similarity.

Vector databases are used to integrate your data with AI models. The first step in their usage is to load your data into a vector database. Then, when a user query is to be sent to the AI model, a set of similar documents is first retrieved. These documents then serve as the context for the user’s question and are sent to the AI model, along with the user’s query. This technique is known as Retrieval Augmented Generation (RAG).

The following sections describe the Spring AI interface for using multiple vector database implementations and some high-level sample usage.

The last section is intended to demystify the underlying approach of similarity searching in vector databases.

This section serves as a guide to the VectorStore interface and its associated classes within the Spring AI framework.

Spring AI offers an abstracted API for interacting with vector databases through the VectorStore interface.

Here is the VectorStore interface definition:

and the related SearchRequest builder:

To insert data into the vector database, encapsulate it within a Document object. The Document class encapsulates content from a data source, such as a PDF or Word document, and includes text represented as a string. It also contains metadata in the form of key-value pairs, including details such as the filename.

Upon insertion into the vector database, the text content is transformed into a numerical array, or a float[], known as vector embeddings, using an embedding model. Embedding models, such as Word2Vec, GLoVE, and BERT, or OpenAI’s text-embedding-ada-002, are used to convert words, sentences, or paragraphs into these vector embeddings.

The vector database’s role is to store and facilitate similarity searches for these embeddings. It does not generate the embeddings itself. For creating vector embeddings, the EmbeddingModel should be utilized.

The similaritySearch methods in the interface allow for retrieving documents similar to a given query string. These methods can be fine-tuned by using the following parameters:

k: An integer that specifies the maximum number of similar documents to return. This is often referred to as a 'top K' search, or 'K nearest neighbors' (KNN).

threshold: A double value ranging from 0 to 1, where values closer to 1 indicate higher similarity. By default, if you set a threshold of 0.75, for instance, only documents with a similarity above this value are returned.

Filter.Expression: A class used for passing a fluent DSL (Domain-Specific Language) expression that functions similarly to a 'where' clause in SQL, but it applies exclusively to the metadata key-value pairs of a Document.

filterExpression: An external DSL based on ANTLR4 that accepts filter expressions as strings. For example, with metadata keys like country, year, and isActive, you could use an expression such as: country == 'UK' && year >= 2020 && isActive == true.

Find more information on the Filter.Expression in the Metadata Filters section.

Some vector stores require their backend schema to be initialized before usage. It will not be initialized for you by default. You must opt-in, by passing a boolean for the appropriate constructor argument or, if using Spring Boot, setting the appropriate initialize-schema property to true in application.properties or application.yml. Check the documentation for the vector store you are using for the specific property name.

When working with vector stores, it’s often necessary to embed large numbers of documents. While it might seem straightforward to make a single call to embed all documents at once, this approach can lead to issues. Embedding models process text as tokens and have a maximum token limit, often referred to as the context window size. This limit restricts the amount of text that can be processed in a single embedding request. Attempting to embed too many tokens in one call can result in errors or truncated embeddings.

To address this token limit, Spring AI implements a batching strategy. This approach breaks down large sets of documents into smaller batches that fit within the embedding model’s maximum context window. Batching not only solves the token limit issue but can also lead to improved performance and more efficient use of API rate limits.

Spring AI provides this functionality through the BatchingStrategy interface, which allows for processing documents in sub-batches based on their token counts.

The core BatchingStrategy interface is defined as follows:

This interface defines a single method, batch, which takes a list of documents and returns a list of document batches.

Spring AI provides a default implementation called TokenCountBatchingStrategy. This strategy batches documents based on their token counts, ensuring that each batch does not exceed a calculated maximum input token count.

Key features of TokenCountBatchingStrategy:

Uses OpenAI’s max input token count (8191) as the default upper limit.

Incorporates a reserve percentage (default 10%) to provide a buffer for potential overhead.

Calculates the actual max input token count as: actualMaxInputTokenCount = originalMaxInputTokenCount * (1 - RESERVE_PERCENTAGE)

The strategy estimates the token count for each document, groups them into batches without exceeding the max input token count, and throws an exception if a single document exceeds this limit.

You can also customize the TokenCountBatchingStrategy to better suit your specific requirements. This can be done by creating a new instance with custom parameters in a Spring Boot @Configuration class.

Here’s an example of how to create a custom TokenCountBatchingStrategy bean:

In this configuration:

EncodingType.CL100K_BASE: Specifies the encoding type used for tokenization. This encoding type is used by the JTokkitTokenCountEstimator to accurately estimate token counts.

8000: Sets the maximum input token count. This value should be less than or equal to the maximum context window size of your embedding model.

0.1: Sets the reserve percentage. The percentage of tokens to reserve from the max input token count. This creates a buffer for potential token count increases during processing.

By default, this constructor uses Document.DEFAULT_CONTENT_FORMATTER for content formatting and MetadataMode.NONE for metadata handling. If you need to customize these parameters, you can use the full constructor with additional parameters.

Once defined, this custom TokenCountBatchingStrategy bean will be automatically used by the EmbeddingModel implementations in your application, replacing the default strategy.

The TokenCountBatchingStrategy internally uses a TokenCountEstimator (specifically, JTokkitTokenCountEstimator) to calculate token counts for efficient batching. This ensures accurate token estimation based on the specified encoding type.

Additionally, TokenCountBatchingStrategy provides flexibility by allowing you to pass in your own implementation of the TokenCountEstimator interface. This feature enables you to use custom token counting strategies tailored to your specific needs. For example:

Some embedding models, such as Vertex AI text embedding, support an auto_truncate feature. When enabled, the model silently truncates text inputs that exceed the maximum size and continues processing; when disabled, it throws an explicit error for inputs that are too large.

When using auto-truncation with the batching strategy, you must configure your batching strategy with a much higher input token count than the model’s actual maximum. This prevents the batching strategy from raising exceptions for large documents, allowing the embedding model to handle truncation internally.

When enabling auto-truncation, set your batching strategy’s maximum input token count much higher than the model’s actual limit. This prevents the batching strategy from raising exceptions for large documents, allowing the embedding model to handle truncation internally.

Here’s an example configuration for using Vertex AI with auto-truncation and custom BatchingStrategy and then using them in the PgVectorStore:

In this configuration:

The embedding model has auto-truncation enabled, allowing it to handle oversized inputs gracefully.

The batching strategy uses an artificially high token limit (132,900) that’s much larger than the actual model limit (20,000).

The vector store uses the configured embedding model and the custom BatchingStrategy bean.

This approach works because:

The TokenCountBatchingStrategy checks if any single document exceeds the configured maximum and throws an IllegalArgumentException if it does.

By setting a very high limit in the batching strategy, we ensure that this check never fails.

Documents or batches exceeding the model’s limit are silently truncated and processed by the embedding model’s auto-truncation feature.

When using auto-truncation:

Set the batching strategy’s max input token count to be at least 5-10x larger than the model’s actual limit to avoid premature exceptions from the batching strategy.

Monitor your logs for truncation warnings from the embedding model (note: not all models log truncation events).

Consider the implications of silent truncation on your embedding quality.

Test with sample documents to ensure truncated embeddings still meet your requirements.

Document this configuration for future maintainers, as it is non-standard.

If you’re using Spring Boot auto-configuration, you must provide a custom BatchingStrategy bean to override the default one that comes with Spring AI:

The presence of this bean in your application context will automatically replace the default batching strategy used by all vector stores.

While TokenCountBatchingStrategy provides a robust default implementation, you can customize the batching strategy to fit your specific needs. This can be done through Spring Boot’s auto-configuration.

To customize the batching strategy, define a BatchingStrategy bean in your Spring Boot application:

This custom BatchingStrategy will then be automatically used by the EmbeddingModel implementations in your application.

These are the available implementations of the VectorStore interface:

Azure Vector Search - The Azure vector store.

Apache Cassandra - The Apache Cassandra vector store.

Chroma Vector Store - The Chroma vector store.

Elasticsearch Vector Store - The Elasticsearch vector store.

GemFire Vector Store - The GemFire vector store.

MariaDB Vector Store - The MariaDB vector store.

Milvus Vector Store - The Milvus vector store.

MongoDB Atlas Vector Store - The MongoDB Atlas vector store.

Neo4j Vector Store - The Neo4j vector store.

OpenSearch Vector Store - The OpenSearch vector store.

Oracle Vector Store - The Oracle Database vector store.

PgVector Store - The PostgreSQL/PGVector vector store.

Pinecone Vector Store - PineCone vector store.

Qdrant Vector Store - Qdrant vector store.

Redis Vector Store - The Redis vector store.

SAP Hana Vector Store - The SAP HANA vector store.

Typesense Vector Store - The Typesense vector store.

Weaviate Vector Store - The Weaviate vector store.

SimpleVectorStore - A simple implementation of persistent vector storage, good for educational purposes.

More implementations may be supported in future releases.

If you have a vector database that needs to be supported by Spring AI, open an issue on GitHub or, even better, submit a pull request with an implementation.

Information on each of the VectorStore implementations can be found in the subsections of this chapter.

To compute the embeddings for a vector database, you need to pick an embedding model that matches the higher-level AI model being used.

For example, with OpenAI’s ChatGPT, we use the OpenAiEmbeddingModel and a model named text-embedding-ada-002.

The Spring Boot starter’s auto-configuration for OpenAI makes an implementation of EmbeddingModel available in the Spring application context for dependency injection.

The general usage of loading data into a vector store is something you would do in a batch-like job, by first loading data into Spring AI’s Document class and then calling the save method.

Given a String reference to a source file that represents a JSON file with data we want to load into the vector database, we use Spring AI’s JsonReader to load specific fields in the JSON, which splits them up into small pieces and then passes those small pieces to the vector store implementation. The VectorStore implementation computes the embeddings and stores the JSON and the embedding in the vector database:

Later, when a user question is passed into the AI model, a similarity search is done to retrieve similar documents, which are then "'stuffed'" into the prompt as context for the user’s question.

Additional options can be passed into the similaritySearch method to define how many documents to retrieve and a threshold of the similarity search.

This section describes various filters that you can use against the results of a query.

You can pass in an SQL-like filter expressions as a String to one of the similaritySearch overloads.

Consider the following examples:

"genre == 'drama' && year >= 2020"

"genre in ['comedy', 'documentary', 'drama']"

You can create an instance of Filter.Expression with a FilterExpressionBuilder that exposes a fluent API. A simple example is as follows:

You can build up sophisticated expressions by using the following operators:

You can combine expressions by using the following operators:

Considering the following example:

You can also use the following operators:

Consider the following example:

The Vector Store interface provides multiple methods for deleting documents, allowing you to remove data either by specific document IDs or using filter expressions.

The simplest way to delete documents is by providing a list of document IDs:

This method removes all documents whose IDs match those in the provided list. If any ID in the list doesn’t exist in the store, it will be ignored.

For more complex deletion criteria, you can use filter expressions:

This method accepts a Filter.Expression object that defines the criteria for which documents should be deleted. It’s particularly useful when you need to delete documents based on their metadata properties.

For convenience, you can also delete documents using a string-based filter expression:

This method converts the provided string filter into a Filter.Expression object internally. It’s useful when you have filter criteria in string format.

All deletion methods may throw exceptions in case of errors:

The best practice is to wrap delete operations in try-catch blocks:

A common scenario is managing document versions where you need to upload a new version of a document while removing the old version. Here’s how to handle this using filter expressions:

You can also accomplish the same using the string filter expression:

Deleting by ID list is generally faster when you know exactly which documents to remove.

Filter-based deletion may require scanning the index to find matching documents; however, this is vector store implementation-specific.

Large deletion operations should be batched to avoid overwhelming the system.

Consider using filter expressions when deleting based on document properties rather than collecting IDs first.

Understanding Vectors

**Examples:**

Example 1 (java):
```java
public interface VectorStore extends DocumentWriter {

    default String getName() {
		return this.getClass().getSimpleName();
	}

    void add(List<Document> documents);

    void delete(List<String> idList);

    void delete(Filter.Expression filterExpression);

    default void delete(String filterExpression) { ... };

    List<Document> similaritySearch(String query);

    List<Document> similaritySearch(SearchRequest request);

    default <T> Optional<T> getNativeClient() {
		return Optional.empty();
	}
}
```

Example 2 (java):
```java
public class SearchRequest {

	public static final double SIMILARITY_THRESHOLD_ACCEPT_ALL = 0.0;

	public static final int DEFAULT_TOP_K = 4;

	private String query = "";

	private int topK = DEFAULT_TOP_K;

	private double similarityThreshold = SIMILARITY_THRESHOLD_ACCEPT_ALL;

	@Nullable
	private Filter.Expression filterExpression;

    public static Builder from(SearchRequest originalSearchRequest) {
		return builder().query(originalSearchRequest.getQuery())
			.topK(originalSearchRequest.getTopK())
			.similarityThreshold(originalSearchRequest.getSimilarityThreshold())
			.filterExpression(originalSearchRequest.getFilterExpression());
	}

	public static class Builder {

		private final SearchRequest searchRequest = new SearchRequest();

		public Builder query(String query) {
			Assert.notNull(query, "Query can not be null.");
			this.searchRequest.query = query;
			return this;
		}

		public Builder topK(int topK) {
			Assert.isTrue(topK >= 0, "TopK should be positive.");
			this.searchRequest.topK = topK;
			return this;
		}

		public Builder similarityThreshold(double threshold) {
			Assert.isTrue(threshold >= 0 && threshold <= 1, "Similarity threshold must be in [0,1] range.");
			this.searchRequest.similarityThreshold = threshold;
			return this;
		}

		public Builder similarityThresholdAll() {
			this.searchRequest.similarityThreshold = 0.0;
			return this;
		}

		public Builder filterExpression(@Nullable Filter.Expression expression) {
			this.searchRequest.filterExpression = expression;
			return this;
		}

		public Builder filterExpression(@Nullable String textExpression) {
			this.searchRequest.filterExpression = (textExpression != null)
					? new FilterExpressionTextParser().parse(textExpression) : null;
			return this;
		}

		public SearchRequest build() {
			return this.searchRequest;
		}

	}

	public String getQuery() {...}
	public int getTopK() {...}
	public double getSimilarityThreshold() {...}
	public Filter.Expression getFilterExpression() {...}
}
```

Example 3 (java):
```java
public interface BatchingStrategy {
    List<List<Document>> batch(List<Document> documents);
}
```

Example 4 (java):
```java
@Configuration
public class EmbeddingConfig {
    @Bean
    public BatchingStrategy customTokenCountBatchingStrategy() {
        return new TokenCountBatchingStrategy(
            EncodingType.CL100K_BASE,  // Specify the encoding type
            8000,                      // Set the maximum input token count
            0.1                        // Set the reserve percentage
        );
    }
}
```

---

## Elasticsearch :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/elasticsearch.html

**Contents:**
- Elasticsearch
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Metadata Filtering
- Manual Configuration
- Accessing the Native Client

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up the Elasticsearch VectorStore to store document embeddings and perform similarity searches.

Elasticsearch is an open source search and analytics engine based on the Apache Lucene library.

A running Elasticsearch instance. The following options are available:

Self-Managed Elasticsearch

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Elasticsearch Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml or Gradle build.gradle build files:

For spring-boot versions pre 3.3.0 it’s necessary to explicitly add the elasticsearch-java dependency with version > 8.13.3, otherwise the older version used will be incompatible with the queries performed:

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file. Alternatively you can opt-out the initialization and create the index manually using the Elasticsearch client, which can be useful if the index needs advanced mapping or additional configuration.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options. These properties can be also set by configuring the ElasticsearchVectorStoreOptions bean.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the ElasticsearchVectorStore as a vector store in your application.

To connect to Elasticsearch and use the ElasticsearchVectorStore, you need to provide access details for your instance. A simple configuration can either be provided via Spring Boot’s application.yml,

The Spring Boot properties starting with spring.elasticsearch.* are used to configure the Elasticsearch client:

spring.elasticsearch.connection-timeout

Connection timeout used when communicating with Elasticsearch.

spring.elasticsearch.password

Password for authentication with Elasticsearch.

spring.elasticsearch.username

Username for authentication with Elasticsearch.

spring.elasticsearch.uris

Comma-separated list of the Elasticsearch instances to use.

http://localhost:9200

spring.elasticsearch.path-prefix

Prefix added to the path of every request sent to Elasticsearch.

spring.elasticsearch.restclient.sniffer.delay-after-failure

Delay of a sniff execution scheduled after a failure.

spring.elasticsearch.restclient.sniffer.interval

Interval between consecutive ordinary sniff executions.

spring.elasticsearch.restclient.ssl.bundle

spring.elasticsearch.socket-keep-alive

Whether to enable socket keep alive between client and Elasticsearch.

spring.elasticsearch.socket-timeout

Socket timeout used when communicating with Elasticsearch.

Properties starting with spring.ai.vectorstore.elasticsearch.* are used to configure the ElasticsearchVectorStore:

spring.ai.vectorstore.elasticsearch.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.elasticsearch.index-name

The name of the index to store the vectors

spring-ai-document-index

spring.ai.vectorstore.elasticsearch.dimensions

The number of dimensions in the vector

spring.ai.vectorstore.elasticsearch.similarity

The similarity function to use

spring.ai.vectorstore.elasticsearch.embedding-field-name

The name of the vector field to search against

The following similarity functions are available:

cosine - Default, suitable for most use cases. Measures cosine similarity between vectors.

l2_norm - Euclidean distance between vectors. Lower values indicate higher similarity.

dot_product - Best performance for normalized vectors (e.g., OpenAI embeddings).

More details about each in the Elasticsearch Documentation on dense vectors.

You can leverage the generic, portable metadata filters with Elasticsearch as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Elasticsearch filter format:

Instead of using the Spring Boot auto-configuration, you can manually configure the Elasticsearch vector store. For this you need to add the spring-ai-elasticsearch-store to your project:

or to your Gradle build.gradle build file.

Create an Elasticsearch RestClient bean. Read the Elasticsearch Documentation for more in-depth information about the configuration of a custom RestClient.

Then create the ElasticsearchVectorStore bean using the builder pattern:

The Elasticsearch Vector Store implementation provides access to the underlying native Elasticsearch client (ElasticsearchClient) through the getNativeClient() method:

The native client gives you access to Elasticsearch-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-elasticsearch</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-elasticsearch'
}
```

Example 3 (xml):
```xml
<dependency>
    <groupId>co.elastic.clients</groupId>
    <artifactId>elasticsearch-java</artifactId>
    <version>8.13.3</version>
</dependency>
```

Example 4 (json):
```json
dependencies {
    implementation 'co.elastic.clients:elasticsearch-java:8.13.3'
}
```

---

## MongoDB Atlas :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/mongodb.html

**Contents:**
- MongoDB Atlas
- What is MongoDB Atlas?
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Tutorials and Code Examples
- Accessing the Native Client

For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up MongoDB Atlas as a vector store to use with Spring AI.

MongoDB Atlas is the fully-managed cloud database from MongoDB available in AWS, Azure, and GCP. Atlas supports native Vector Search and full text search on your MongoDB document data.

MongoDB Atlas Vector Search allows you to store your embeddings in MongoDB documents, create vector search indexes, and perform KNN searches with an approximate nearest neighbor algorithm (Hierarchical Navigable Small Worlds). You can use the $vectorSearch aggregation operator in a MongoDB aggregation stage to perform a search on your vector embeddings.

An Atlas cluster running MongoDB version 6.0.11, 7.0.2, or later. To get started with MongoDB Atlas, you can follow the instructions here. Ensure that your IP address is included in your Atlas project’s access list.

A running MongoDB Atlas instance with Vector Search enabled

Collection with vector search index configured

Collection schema with id (string), content (string), metadata (document), and embedding (vector) fields

Proper access permissions for index and collection operations

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the MongoDB Atlas Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

The vector store implementation can initialize the requisite schema for you, but you must opt-in by setting spring.ai.vectorstore.mongodb.initialize-schema=true in the application.properties file. Alternatively you can opt-out the initialization and create the index manually using the MongoDB Atlas UI, Atlas Administration API, or Atlas CLI, which can be useful if the index needs advanced mapping or additional configuration.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the MongoDBAtlasVectorStore as a vector store in your application:

To connect to MongoDB Atlas and use the MongoDBAtlasVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.mongodb.* are used to configure the MongoDBAtlasVectorStore:

spring.ai.vectorstore.mongodb.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.mongodb.collection-name

The name of the collection to store the vectors

spring.ai.vectorstore.mongodb.index-name

The name of the vector search index

spring.ai.vectorstore.mongodb.path-name

The path where vectors are stored

spring.ai.vectorstore.mongodb.metadata-fields-to-filter

Comma-separated list of metadata fields that can be used for filtering

Instead of using the Spring Boot auto-configuration, you can manually configure the MongoDB Atlas vector store. For this you need to add the spring-ai-mongodb-atlas-store to your project:

or to your Gradle build.gradle build file:

Create a MongoTemplate bean:

Then create the MongoDBAtlasVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with MongoDB Atlas as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary MongoDB Atlas filter format:

To get started with Spring AI and MongoDB:

See the Getting Started guide for Spring AI Integration.

For a comprehensive code example demonstrating Retrieval Augmented Generation (RAG) with Spring AI and MongoDB, refer to this detailed tutorial.

The MongoDB Atlas Vector Store implementation provides access to the underlying native MongoDB client (MongoClient) through the getNativeClient() method:

The native client gives you access to MongoDB-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-mongodb-atlas</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-mongodb-atlas'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to MongoDB Atlas
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  data:
    mongodb:
      uri: <mongodb atlas connection string>
      database: <database name>
  ai:
    vectorstore:
      mongodb:
        initialize-schema: true
        collection-name: custom_vector_store
        index-name: custom_vector_index
        path-name: custom_embedding
        metadata-fields-to-filter: author,year
```

---

## Azure Cosmos DB :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/azure-cosmos-db.html

**Contents:**
- Azure Cosmos DB
- What is Azure Cosmos DB?
- What is DiskANN?
- Setting up Azure Cosmos DB Vector Store with Auto Configuration
- Auto Configuration
- Configuration Properties
- Complex Searches with Filters
- Setting up Azure Cosmos DB Vector Store without Auto Configuration
- Manual Dependency Setup
- Accessing the Native Client

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up CosmosDBVectorStore to store document embeddings and perform similarity searches.

Azure Cosmos DB is Microsoft’s globally distributed cloud-native database service designed for mission-critical applications. It offers high availability, low latency, and the ability to scale horizontally to meet modern application demands. It was built from the ground up with global distribution, fine-grained multi-tenancy, and horizontal scalability at its core. It is a foundational service in Azure, used by most of Microsoft’s mission critical applications at global scale, including Teams, Skype, Xbox Live, Office 365, Bing, Azure Active Directory, Azure Portal, Microsoft Store, and many others. It is also used by thousands of external customers including OpenAI for ChatGPT and other mission-critical AI applications that require elastic scale, turnkey global distribution, and low latency and high availability across the planet.

DiskANN (Disk-based Approximate Nearest Neighbor Search) is an innovative technology used in Azure Cosmos DB to enhance the performance of vector searches. It enables efficient and scalable similarity searches across high-dimensional data by indexing embeddings stored in Cosmos DB.

DiskANN provides the following benefits:

Efficiency: By utilizing disk-based structures, DiskANN significantly reduces the time required to find nearest neighbors compared to traditional methods.

Scalability: It can handle large datasets that exceed memory capacity, making it suitable for various applications, including machine learning and AI-driven solutions.

Low Latency: DiskANN minimizes latency during search operations, ensuring that applications can retrieve results quickly even with substantial data volumes.

In the context of Spring AI for Azure Cosmos DB, vector searches will create and leverage DiskANN indexes to ensure optimal performance for similarity queries.

The following code demonstrates how to set up the CosmosDBVectorStore with auto-configuration:

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add the following dependency to your Maven project:

The following configuration properties are available for the Cosmos DB vector store:

spring.ai.vectorstore.cosmosdb.databaseName

The name of the Cosmos DB database to use.

spring.ai.vectorstore.cosmosdb.containerName

The name of the Cosmos DB container to use.

spring.ai.vectorstore.cosmosdb.partitionKeyPath

The path for the partition key.

spring.ai.vectorstore.cosmosdb.metadataFields

Comma-separated list of metadata fields.

spring.ai.vectorstore.cosmosdb.vectorStoreThroughput

The throughput for the vector store.

spring.ai.vectorstore.cosmosdb.vectorDimensions

The number of dimensions for the vectors.

spring.ai.vectorstore.cosmosdb.endpoint

The endpoint for the Cosmos DB.

spring.ai.vectorstore.cosmosdb.key

The key for the Cosmos DB (if key is not present, [DefaultAzureCredential](learn.microsoft.com/azure/developer/java/sdk/authentication/credential-chains#defaultazurecredential-overview) will be used).

You can perform more complex searches using filters in the Cosmos DB vector store. Below is a sample demonstrating how to use filters in your search queries.

The following code demonstrates how to set up the CosmosDBVectorStore without relying on auto-configuration. [DefaultAzureCredential](learn.microsoft.com/azure/developer/java/sdk/authentication/credential-chains#defaultazurecredential-overview) is recommended for authentication to Azure Cosmos DB.

This configuration shows all the available builder options:

databaseName: The name of your Cosmos DB database

containerName: The name of your container within the database

partitionKeyPath: The path for the partition key (e.g., "/id")

metadataFields: List of metadata fields that will be used for filtering

vectorStoreThroughput: The throughput (RU/s) for the vector store container

vectorDimensions: The number of dimensions for your vectors (should match your embedding model)

batchingStrategy: Strategy for batching document operations (optional)

Add the following dependency in your Maven project:

The Azure Cosmos DB Vector Store implementation provides access to the underlying native Azure Cosmos DB client (CosmosClient) through the getNativeClient() method:

The native client gives you access to Azure Cosmos DB-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (java):
```java
package com.example.demo;

import io.micrometer.observation.ObservationRegistry;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.ai.document.Document;
import org.springframework.ai.vectorstore.SearchRequest;
import org.springframework.ai.vectorstore.VectorStore;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Lazy;

import java.util.List;
import java.util.Map;
import java.util.UUID;

import static org.assertj.core.api.Assertions.assertThat;

@SpringBootApplication
@EnableAutoConfiguration
public class DemoApplication implements CommandLineRunner {

    private static final Logger log = LoggerFactory.getLogger(DemoApplication.class);

    @Lazy
    @Autowired
    private VectorStore vectorStore;

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

    @Override
    public void run(String... args) throws Exception {
        Document document1 = new Document(UUID.randomUUID().toString(), "Sample content1", Map.of("key1", "value1"));
        Document document2 = new Document(UUID.randomUUID().toString(), "Sample content2", Map.of("key2", "value2"));
		this.vectorStore.add(List.of(document1, document2));
        List<Document> results = this.vectorStore.similaritySearch(SearchRequest.builder().query("Sample content").topK(1).build());

        log.info("Search results: {}", results);

        // Remove the documents from the vector store
		this.vectorStore.delete(List.of(document1.getId(), document2.getId()));
    }

    @Bean
    public ObservationRegistry observationRegistry() {
        return ObservationRegistry.create();
    }
}
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-azure-cosmos-db</artifactId>
</dependency>
```

Example 3 (java):
```java
Map<String, Object> metadata1 = new HashMap<>();
metadata1.put("country", "UK");
metadata1.put("year", 2021);
metadata1.put("city", "London");

Map<String, Object> metadata2 = new HashMap<>();
metadata2.put("country", "NL");
metadata2.put("year", 2022);
metadata2.put("city", "Amsterdam");

Document document1 = new Document("1", "A document about the UK", this.metadata1);
Document document2 = new Document("2", "A document about the Netherlands", this.metadata2);

vectorStore.add(List.of(document1, document2));

FilterExpressionBuilder builder = new FilterExpressionBuilder();
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("The World")
    .topK(10)
    .filterExpression((this.builder.in("country", "UK", "NL")).build()).build());
```

Example 4 (java):
```java
@Bean
public VectorStore vectorStore(ObservationRegistry observationRegistry) {
    // Create the Cosmos DB client
    CosmosAsyncClient cosmosClient = new CosmosClientBuilder()
            .endpoint(System.getenv("COSMOSDB_AI_ENDPOINT"))
            .credential(new DefaultAzureCredentialBuilder().build())
            .userAgentSuffix("SpringAI-CDBNoSQL-VectorStore")
            .gatewayMode()
            .buildAsyncClient();

    // Create and configure the vector store
    return CosmosDBVectorStore.builder(cosmosClient, embeddingModel)
            .databaseName("test-database")
            .containerName("test-container")
            // Configure metadata fields for filtering
            .metadataFields(List.of("country", "year", "city"))
            // Set the partition key path (optional)
            .partitionKeyPath("/id")
            // Configure performance settings
            .vectorStoreThroughput(1000)
            .vectorDimensions(1536)  // Match your embedding model's dimensions
            // Add custom batching strategy (optional)
            .batchingStrategy(new TokenCountBatchingStrategy())
            // Add observation registry for metrics
            .observationRegistry(observationRegistry)
            .build();
}

@Bean
public EmbeddingModel embeddingModel() {
    return new TransformersEmbeddingModel();
}
```

---

## Untitled :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/hanadb-provision-a-trial-account.html

**Contents:**
- Provision SAP HANA Cloud trial account

Below are the steps to provision SAP Hana Database using a trial account

Let’s start with creating a temporary email for registration purposes

Go to sap.com and navigate to products → Trials and Demos

Click Advanced Trials

Click Start your free 90-day trial

Paste the temporary email id that we created in the first step, and click Next

We fill in our details and click Submit

It’s time to check the inbox of our temporary email account

Notice that there is an email received in our temporary email account

Open the email and click to activate the trial account

It will prompt to create a password. Provide a password and click Submit

The trial account is now created. Click to start the trial

Provide your phone number and click Continue

We receive an OTP on the phone number. Provide the code and click continue

Select the region as US East (VA) - AWS

The SAP BTP trial account is ready. Click Go to your Trial account

Click the Trial sub-account

Open Instances and Subscriptions

It’s time to create a subscription. Click the Create button

While creating a subscription, Select service as SAP Hana Cloud and Plan as tools and click Create

Notice that SAP Hana Cloud subscription is now created. Click Users on the left panel

Select the username (temporary email that we supplied earlier) and click Assign Role Collection

Search hana and select all the 3 role collections that gets displayed. Click Assign Role Collection

Our user now has all the 3 role collections. Click Instances and Subscriptions

Now, click SAP Hana Cloud application under subscriptions

There are no instances yet. Let’s click Create Instance

Select Type as SAP HANA Cloud, SAP HANA Database. Click Next Step

Provide Instance Name, Description, password for DBADMIN administrator. Select the latest version 2024.2 (QRC 1/2024). Click Next Step

Keep everything as default. Click Next Step

Select Allow all IP addresses and click Next Step

Click Review and Create

Click Create Instance

Notice that the provisioning of SAP Hana Database instance has started. It takes some time to provision - please be patient.

Once the instance is provisioned (status is displayed as Running) we can get the datasource url (SQL Endpoint) by clicking the instance and selecting Connections

We navigate to SAP Hana Database Explorer by click the …​

Provide the administrator credentials and click OK

Open SQL console and create the table CRICKET_WORLD_CUP using the following DDL statement:

Navigate to hana_dev_db → Catalog → Tables to find our table CRICKET_WORLD_CUP

Right-click on the table and click Open Data

Notice that the table data is now displayed. There are now rows as we didn’t create any embeddings yet.

Next steps: SAP Hana Vector Engine

---

## Couchbase :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/couchbase.html

**Contents:**
- Couchbase
- Prerequisites
- Auto-configuration
  - Configuration Properties
    - Option 1: Using Spring Expression Language (SpEL)
    - Option 2: Accessing Environment Variables Programmatically
- Metadata Filtering
- Manual Configuration
- Limitations

For the latest snapshot version, please use Spring AI 1.1.2!

This section will walk you through setting up the CouchbaseSearchVectorStore to store document embeddings and perform similarity searches using Couchbase.

Couchbase is a distributed, JSON document database, with all the desired capabilities of a relational DBMS. Among other features, it allows users to query information using vector-based storage and retrieval.

A running Couchbase instance. The following options are available: Couchbase * Docker * Capella - Couchbase as a Service * Install Couchbase locally * Couchbase Kubernetes Operator

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Couchbase Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the configured bucket, scope, collection and search index for you, with default options, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the CouchbaseSearchVectorStore as a vector store in your application.

To connect to Couchbase and use the CouchbaseSearchVectorStore, you need to provide access details for your instance. Configuration can be provided via Spring Boot’s application.properties:

If you prefer to use environment variables for sensitive information like passwords or API keys, you have multiple options:

You can use custom environment variable names and reference them in your application configuration using SpEL:

Alternatively, you can access environment variables in your Java code:

This approach gives you flexibility in naming your environment variables while keeping sensitive information out of your application configuration files.

Spring Boot’s auto-configuration feature for the Couchbase Cluster will create a bean instance that will be used by the CouchbaseSearchVectorStore.

The Spring Boot properties starting with spring.couchbase.* are used to configure the Couchbase cluster instance:

spring.couchbase.connection-string

A couchbase connection string

couchbase://localhost

spring.couchbase.password

Password for authentication with Couchbase.

spring.couchbase.username

Username for authentication with Couchbase.

spring.couchbase.env.io.minEndpoints

Minimum number of sockets per node.

spring.couchbase.env.io.maxEndpoints

Maximum number of sockets per node.

spring.couchbase.env.io.idleHttpConnectionTimeout

Length of time an HTTP connection may remain idle before it is closed and removed from the pool.

spring.couchbase.env.ssl.enabled

Whether to enable SSL support. Enabled automatically if a "bundle" is provided unless specified otherwise.

spring.couchbase.env.ssl.bundle

spring.couchbase.env.timeouts.connect

Bucket connect timeout.

spring.couchbase.env.timeouts.disconnect

Bucket disconnect timeout.

spring.couchbase.env.timeouts.key-value

Timeout for operations on a specific key-value.

spring.couchbase.env.timeouts.key-value

Timeout for operations on a specific key-value with a durability level.

spring.couchbase.env.timeouts.key-value-durable

Timeout for operations on a specific key-value with a durability level.

spring.couchbase.env.timeouts.query

SQL++ query operations timeout.

spring.couchbase.env.timeouts.view

Regular and geospatial view operations timeout.

spring.couchbase.env.timeouts.search

Timeout for the search service.

spring.couchbase.env.timeouts.analytics

Timeout for the analytics service.

spring.couchbase.env.timeouts.management

Timeout for the management operations.

Properties starting with the spring.ai.vectorstore.couchbase.* prefix are used to configure CouchbaseSearchVectorStore.

spring.ai.vectorstore.couchbase.index-name

The name of the index to store the vectors.

spring-ai-document-index

spring.ai.vectorstore.couchbase.bucket-name

The name of the Couchbase Bucket, parent of the scope.

spring.ai.vectorstore.couchbase.scope-name

The name of the Couchbase scope, parent of the collection. Search queries will be executed in the scope context.

spring.ai.vectorstore.couchbase.collection-name

The name of the Couchbase collection to store the Documents.

spring.ai.vectorstore.couchbase.dimensions

The number of dimensions in the vector.

spring.ai.vectorstore.couchbase.similarity

The similarity function to use.

spring.ai.vectorstore.couchbase.optimization

The similarity function to use.

spring.ai.vectorstore.couchbase.initialize-schema

whether to initialize the required schema

The following similarity functions are available:

The following index optimizations are available:

More details about each in the Couchbase Documentation on vector searches.

You can leverage the generic, portable metadata filters with the Couchbase store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

Instead of using the Spring Boot auto-configuration, you can manually configure the Couchbase vector store. For this you need to add the spring-ai-couchbase-store to your project:

or to your Gradle build.gradle build file.

Create a Couchbase Cluster bean. Read the Couchbase Documentation for more in-depth information about the configuration of a custom Cluster instance.

and then create the CouchbaseSearchVectorStore bean using the builder pattern:

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-couchbase</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-couchbase-store-spring-boot-starter'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List <Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Qdrant
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.query("Spring").withTopK(5));
```

Example 4 (typescript):
```typescript
spring.ai.openai.api-key=<key>
spring.couchbase.connection-string=<conn_string>
spring.couchbase.username=<username>
spring.couchbase.password=<password>
```

---

## Milvus :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/milvus.html

**Contents:**
- Milvus
- Prerequisites
- Dependencies
  - Manual Configuration
- Metadata filtering
- Using MilvusSearchRequest
- Importance of nativeExpression and searchParamsJson in MilvusSearchRequest
- Milvus VectorStore properties
- Starting Milvus Store
- Troubleshooting

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Milvus is an open-source vector database that has garnered significant attention in the fields of data science and machine learning. One of its standout features lies in its robust support for vector indexing and querying. Milvus employs state-of-the-art, cutting-edge algorithms to accelerate the search process, making it exceptionally efficient at retrieving similar vectors, even when handling extensive datasets.

A running Milvus instance. The following options are available:

Milvus Standalone: Docker, Operator, Helm,DEB/RPM, Docker Compose.

Milvus Cluster: Operator, Helm.

If required, an API key for the EmbeddingModel to generate the embeddings stored by the MilvusVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Then add the Milvus VectorStore boot starter dependency to your project:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

The Vector Store, also requires an EmbeddingModel instance to calculate embeddings for the documents. You can pick one of the available EmbeddingModel Implementations.

To connect to and configure the MilvusVectorStore, you need to provide access details for your instance. A simple configuration can either be provided via Spring Boot’s application.yml

Now you can Auto-wire the Milvus Vector Store in your application and use it

Instead of using the Spring Boot auto-configuration, you can manually configure the MilvusVectorStore. To add the following dependencies to your project:

To configure MilvusVectorStore in your application, you can use the following setup:

You can leverage the generic, portable metadata filters with the Milvus store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

MilvusSearchRequest extends SearchRequest, allowing you to use Milvus-specific search parameters such as native expressions and search parameter JSON.

This allows greater flexibility when using Milvus-specific search features.

These two parameters enhance Milvus search precision and ensure optimal query performance:

nativeExpression: Enables additional filtering capabilities using Milvus' native filtering expressions. Milvus Filtering

searchParamsJson: Essential for tuning search behavior when using IVF_FLAT, Milvus' default index. Milvus Vector Index

By default, IVF_FLAT requires nprobe to be set for accurate results. If not specified, nprobe defaults to 1, which can lead to poor recall or even zero search results.

Using nativeExpression ensures advanced filtering, while searchParamsJson prevents ineffective searches caused by a low default nprobe value.

You can use the following properties in your Spring Boot configuration to customize the Milvus vector store.

spring.ai.vectorstore.milvus.database-name

The name of the Milvus database to use.

spring.ai.vectorstore.milvus.collection-name

Milvus collection name to store the vectors

spring.ai.vectorstore.milvus.initialize-schema

whether to initialize Milvus' backend

spring.ai.vectorstore.milvus.embedding-dimension

The dimension of the vectors to be stored in the Milvus collection.

spring.ai.vectorstore.milvus.index-type

The type of the index to be created for the Milvus collection.

spring.ai.vectorstore.milvus.metric-type

The metric type to be used for the Milvus collection.

spring.ai.vectorstore.milvus.index-parameters

The index parameters to be used for the Milvus collection.

spring.ai.vectorstore.milvus.id-field-name

The ID field name for the collection

spring.ai.vectorstore.milvus.auto-id

Boolean flag to indicate if the auto-id is used for the ID field

spring.ai.vectorstore.milvus.content-field-name

The content field name for the collection

spring.ai.vectorstore.milvus.metadata-field-name

The metadata field name for the collection

spring.ai.vectorstore.milvus.embedding-field-name

The embedding field name for the collection

spring.ai.vectorstore.milvus.client.host

The name or address of the host.

spring.ai.vectorstore.milvus.client.port

spring.ai.vectorstore.milvus.client.uri

The uri of Milvus instance

spring.ai.vectorstore.milvus.client.token

Token serving as the key for identification and authentication purposes.

spring.ai.vectorstore.milvus.client.connect-timeout-ms

Connection timeout value of client channel. The timeout value must be greater than zero .

spring.ai.vectorstore.milvus.client.keep-alive-time-ms

Keep-alive time value of client channel. The keep-alive value must be greater than zero.

spring.ai.vectorstore.milvus.client.keep-alive-timeout-ms

The keep-alive timeout value of client channel. The timeout value must be greater than zero.

spring.ai.vectorstore.milvus.client.rpc-deadline-ms

Deadline for how long you are willing to wait for a reply from the server. With a deadline setting, the client will wait when encounter fast RPC fail caused by network fluctuations. The deadline value must be larger than or equal to zero.

spring.ai.vectorstore.milvus.client.client-key-path

The client.key path for tls two-way authentication, only takes effect when "secure" is true

spring.ai.vectorstore.milvus.client.client-pem-path

The client.pem path for tls two-way authentication, only takes effect when "secure" is true

spring.ai.vectorstore.milvus.client.ca-pem-path

The ca.pem path for tls two-way authentication, only takes effect when "secure" is true

spring.ai.vectorstore.milvus.client.server-pem-path

server.pem path for tls one-way authentication, only takes effect when "secure" is true.

spring.ai.vectorstore.milvus.client.server-name

Sets the target name override for SSL host name checking, only takes effect when "secure" is True. Note: this value is passed to grpc.ssl_target_name_override

spring.ai.vectorstore.milvus.client.secure

Secure the authorization for this connection, set to True to enable TLS.

spring.ai.vectorstore.milvus.client.idle-timeout-ms

Idle timeout value of client channel. The timeout value must be larger than zero.

spring.ai.vectorstore.milvus.client.username

The username and password for this connection.

spring.ai.vectorstore.milvus.client.password

The password for this connection.

From within the src/test/resources/ folder run:

To clean the environment:

Then connect to the vector store on http://localhost:19530 or for management http://localhost:9001 (user: minioadmin, pass: minioadmin)

If Docker complains about resources, then execute:

The Milvus Vector Store implementation provides access to the underlying native Milvus client (MilvusServiceClient) through the getNativeClient() method:

The native client gives you access to Milvus-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-vector-store-milvus</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-milvus'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List <Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Milvus Vector Store
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = this.vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-milvus-store</artifactId>
</dependency>
```

---

## Couchbase :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/couchbase.html

**Contents:**
- Couchbase
- Prerequisites
- Auto-configuration
  - Configuration Properties
    - Option 1: Using Spring Expression Language (SpEL)
    - Option 2: Accessing Environment Variables Programmatically
- Metadata Filtering
- Manual Configuration
- Limitations

This section will walk you through setting up the CouchbaseSearchVectorStore to store document embeddings and perform similarity searches using Couchbase.

Couchbase is a distributed, JSON document database, with all the desired capabilities of a relational DBMS. Among other features, it allows users to query information using vector-based storage and retrieval.

A running Couchbase instance. The following options are available: Couchbase * Docker * Capella - Couchbase as a Service * Install Couchbase locally * Couchbase Kubernetes Operator

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Couchbase Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the configured bucket, scope, collection and search index for you, with default options, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the CouchbaseSearchVectorStore as a vector store in your application.

To connect to Couchbase and use the CouchbaseSearchVectorStore, you need to provide access details for your instance. Configuration can be provided via Spring Boot’s application.properties:

If you prefer to use environment variables for sensitive information like passwords or API keys, you have multiple options:

You can use custom environment variable names and reference them in your application configuration using SpEL:

Alternatively, you can access environment variables in your Java code:

This approach gives you flexibility in naming your environment variables while keeping sensitive information out of your application configuration files.

Spring Boot’s auto-configuration feature for the Couchbase Cluster will create a bean instance that will be used by the CouchbaseSearchVectorStore.

The Spring Boot properties starting with spring.couchbase.* are used to configure the Couchbase cluster instance:

spring.couchbase.connection-string

A couchbase connection string

couchbase://localhost

spring.couchbase.password

Password for authentication with Couchbase.

spring.couchbase.username

Username for authentication with Couchbase.

spring.couchbase.env.io.minEndpoints

Minimum number of sockets per node.

spring.couchbase.env.io.maxEndpoints

Maximum number of sockets per node.

spring.couchbase.env.io.idleHttpConnectionTimeout

Length of time an HTTP connection may remain idle before it is closed and removed from the pool.

spring.couchbase.env.ssl.enabled

Whether to enable SSL support. Enabled automatically if a "bundle" is provided unless specified otherwise.

spring.couchbase.env.ssl.bundle

spring.couchbase.env.timeouts.connect

Bucket connect timeout.

spring.couchbase.env.timeouts.disconnect

Bucket disconnect timeout.

spring.couchbase.env.timeouts.key-value

Timeout for operations on a specific key-value.

spring.couchbase.env.timeouts.key-value

Timeout for operations on a specific key-value with a durability level.

spring.couchbase.env.timeouts.key-value-durable

Timeout for operations on a specific key-value with a durability level.

spring.couchbase.env.timeouts.query

SQL++ query operations timeout.

spring.couchbase.env.timeouts.view

Regular and geospatial view operations timeout.

spring.couchbase.env.timeouts.search

Timeout for the search service.

spring.couchbase.env.timeouts.analytics

Timeout for the analytics service.

spring.couchbase.env.timeouts.management

Timeout for the management operations.

Properties starting with the spring.ai.vectorstore.couchbase.* prefix are used to configure CouchbaseSearchVectorStore.

spring.ai.vectorstore.couchbase.index-name

The name of the index to store the vectors.

spring-ai-document-index

spring.ai.vectorstore.couchbase.bucket-name

The name of the Couchbase Bucket, parent of the scope.

spring.ai.vectorstore.couchbase.scope-name

The name of the Couchbase scope, parent of the collection. Search queries will be executed in the scope context.

spring.ai.vectorstore.couchbase.collection-name

The name of the Couchbase collection to store the Documents.

spring.ai.vectorstore.couchbase.dimensions

The number of dimensions in the vector.

spring.ai.vectorstore.couchbase.similarity

The similarity function to use.

spring.ai.vectorstore.couchbase.optimization

The similarity function to use.

spring.ai.vectorstore.couchbase.initialize-schema

whether to initialize the required schema

The following similarity functions are available:

The following index optimizations are available:

More details about each in the Couchbase Documentation on vector searches.

You can leverage the generic, portable metadata filters with the Couchbase store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

Instead of using the Spring Boot auto-configuration, you can manually configure the Couchbase vector store. For this you need to add the spring-ai-couchbase-store to your project:

or to your Gradle build.gradle build file.

Create a Couchbase Cluster bean. Read the Couchbase Documentation for more in-depth information about the configuration of a custom Cluster instance.

and then create the CouchbaseSearchVectorStore bean using the builder pattern:

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-couchbase</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-couchbase-store-spring-boot-starter'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List <Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Qdrant
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.query("Spring").withTopK(5));
```

Example 4 (typescript):
```typescript
spring.ai.openai.api-key=<key>
spring.couchbase.connection-string=<conn_string>
spring.couchbase.username=<username>
spring.couchbase.password=<password>
```

---

## Weaviate :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/weaviate.html

**Contents:**
- Weaviate
- Prerequisites
- Dependencies
- Configuration
  - Option 1: Using Spring Expression Language (SpEL)
  - Option 2: Accessing Environment Variables Programmatically
- Auto-configuration
- Manual Configuration
- Metadata filtering
- Run Weaviate in Docker

For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up the Weaviate VectorStore to store document embeddings and perform similarity searches.

Weaviate is an open-source vector database that allows you to store data objects and vector embeddings from your favorite ML-models and scale seamlessly into billions of data objects. It provides tools to store document embeddings, content, and metadata and to search through those embeddings, including metadata filtering.

A running Weaviate instance. The following options are available:

Weaviate Cloud Service (requires account creation and API key)

If required, an API key for the EmbeddingModel to generate the embeddings stored by the WeaviateVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add the Weaviate Vector Store dependency to your project:

or to your Gradle build.gradle build file.

To connect to Weaviate and use the WeaviateVectorStore, you need to provide access details for your instance. Configuration can be provided via Spring Boot’s application.properties:

If you prefer to use environment variables for sensitive information like API keys, you have multiple options:

You can use custom environment variable names and reference them in your application configuration:

Alternatively, you can access environment variables in your Java code:

Spring AI provides Spring Boot auto-configuration for the Weaviate Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Here is an example of the required bean:

Now you can auto-wire the WeaviateVectorStore as a vector store in your application.

Instead of using Spring Boot auto-configuration, you can manually configure the WeaviateVectorStore using the builder pattern:

You can leverage the generic, portable metadata filters with Weaviate store as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Weaviate GraphQL filter format:

To quickly get started with a local Weaviate instance, you can run it in Docker:

This starts a Weaviate instance accessible at localhost:8080.

You can use the following properties in your Spring Boot configuration to customize the Weaviate vector store.

spring.ai.vectorstore.weaviate.host

The host of the Weaviate server

spring.ai.vectorstore.weaviate.scheme

spring.ai.vectorstore.weaviate.api-key

The API key for authentication

spring.ai.vectorstore.weaviate.object-class

The class name for storing documents

spring.ai.vectorstore.weaviate.consistency-level

Desired tradeoff between consistency and speed

spring.ai.vectorstore.weaviate.filter-field

Configures metadata fields that can be used in filters. Format: spring.ai.vectorstore.weaviate.filter-field.<field-name>=<field-type>

The Weaviate Vector Store implementation provides access to the underlying native Weaviate client (WeaviateClient) through the getNativeClient() method:

The native client gives you access to Weaviate-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-weaviate-store</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-weaviate-store'
}
```

Example 3 (markdown):
```markdown
spring.ai.vectorstore.weaviate.host=<host_of_your_weaviate_instance>
spring.ai.vectorstore.weaviate.scheme=<http_or_https>
spring.ai.vectorstore.weaviate.api-key=<your_api_key>
# API key if needed, e.g. OpenAI
spring.ai.openai.api-key=<api-key>
```

Example 4 (yaml):
```yaml
# In application.yml
spring:
  ai:
    vectorstore:
      weaviate:
        host: ${WEAVIATE_HOST}
        scheme: ${WEAVIATE_SCHEME}
        api-key: ${WEAVIATE_API_KEY}
    openai:
      api-key: ${OPENAI_API_KEY}
```

---

## MariaDB Vector Store :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/mariadb.html

**Contents:**
- MariaDB Vector Store
- Prerequisites
- Auto-Configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Similarity Scores
  - Score Calculation
  - Accessing Scores
  - Search Results Ordering

For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up MariaDBVectorStore to store document embeddings and perform similarity searches.

MariaDB Vector is part of MariaDB 11.7 and enables storing and searching over machine learning-generated embeddings. It provides efficient vector similarity search capabilities using vector indexes, supporting both cosine similarity and Euclidean distance metrics.

A running MariaDB (11.7+) instance. The following options are available:

If required, an API key for the EmbeddingModel to generate the embeddings stored by the MariaDBVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the MariaDB Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the required schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

For example, to use the OpenAI EmbeddingModel, add the following dependency:

Now you can auto-wire the MariaDBVectorStore in your application:

To connect to MariaDB and use the MariaDBVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.mariadb.* are used to configure the MariaDBVectorStore:

spring.ai.vectorstore.mariadb.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.mariadb.distance-type

Search distance type. Use COSINE (default) or EUCLIDEAN. If vectors are normalized to length 1, you can use EUCLIDEAN for best performance.

spring.ai.vectorstore.mariadb.dimensions

Embeddings dimension. If not specified explicitly, will retrieve dimensions from the provided EmbeddingModel.

spring.ai.vectorstore.mariadb.remove-existing-vector-store-table

Deletes the existing vector store table on startup.

spring.ai.vectorstore.mariadb.schema-name

Vector store schema name

spring.ai.vectorstore.mariadb.table-name

Vector store table name

spring.ai.vectorstore.mariadb.schema-validation

Enables schema and table name validation to ensure they are valid and existing objects.

Instead of using the Spring Boot auto-configuration, you can manually configure the MariaDB vector store. For this you need to add the following dependencies to your project:

Then create the MariaDBVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with MariaDB Vector store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

The MariaDB Vector Store automatically calculates similarity scores for documents returned from similarity searches. These scores provide a normalized measure of how closely each document matches your search query.

Similarity scores are calculated using the formula score = 1.0 - distance, where:

Score: A value between 0.0 and 1.0, where 1.0 indicates perfect similarity and 0.0 indicates no similarity

Distance: The raw distance value calculated using the configured distance type (COSINE or EUCLIDEAN)

This means that documents with smaller distances (more similar) will have higher scores, making the results more intuitive to interpret.

You can access the similarity score for each document through the getScore() method:

Search results are automatically ordered by similarity score in descending order (highest score first). This ensures that the most relevant documents appear at the top of your results.

In addition to the similarity score, the raw distance value is still available in the document metadata:

When using similarity thresholds in your search requests, specify the threshold as a score value (0.0 to 1.0) rather than a distance:

This makes threshold values consistent and intuitive - higher values mean more restrictive searches that only return highly similar documents.

The MariaDB Vector Store implementation provides access to the underlying native JDBC client (JdbcTemplate) through the getNativeClient() method:

The native client gives you access to MariaDB-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-mariadb</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-mariadb'
}
```

Example 3 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

Example 4 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to MariaDB
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

---

## Azure AI Service :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/azure.html

**Contents:**
- Azure AI Service
- Prerequisites
- Configuration
- Dependencies
  - 1. Select an Embeddings interface implementation. You can choose between:
  - 2. Azure (AI Search) Vector Store
- Configuration Properties
- Sample Code
  - Metadata filtering
- Accessing the Native Client

For the latest snapshot version, please use Spring AI 1.1.2!

This section will walk you through setting up the AzureVectorStore to store document embeddings and perform similarity searches using the Azure AI Search Service.

Azure AI Search is a versatile cloud-hosted cloud information retrieval system that is part of Microsoft’s larger AI platform. Among other features, it allows users to query information using vector-based storage and retrieval.

Azure Subscription: You will need an Azure subscription to use any Azure service.

Azure AI Search Service: Create an AI Search service. Once the service is created, obtain the admin apiKey from the Keys section under Settings and retrieve the endpoint from the Url field under the Overview section.

(Optional) Azure OpenAI Service: Create an Azure OpenAI service. NOTE: You may have to fill out a separate form to gain access to Azure Open AI services. Once the service is created, obtain the endpoint and apiKey from the Keys and Endpoint section under Resource Management.

On startup, the AzureVectorStore can attempt to create a new index within your AI Search service instance if you’ve opted in by setting the relevant initialize-schema boolean property to true in the constructor or, if using Spring Boot, setting …​initialize-schema=true in your application.properties file.

Alternatively, you can create the index manually.

To set up an AzureVectorStore, you will need the settings retrieved from the prerequisites above along with your index name:

Azure AI Search Endpoint

(optional) Azure OpenAI API Endpoint

(optional) Azure OpenAI API Key

You can provide these values as OS environment variables.

You can replace Azure Open AI implementation with any valid OpenAI implementation that supports the Embeddings interface. For example, you could use Spring AI’s Open AI or TransformersEmbedding implementations for embeddings instead of the Azure implementation.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add these dependencies to your project:

Local Sentence Transformers Embedding

You can use the following properties in your Spring Boot configuration to customize the Azure vector store.

spring.ai.vectorstore.azure.url

spring.ai.vectorstore.azure.api-key

spring.ai.vectorstore.azure.useKeylessAuth

spring.ai.vectorstore.azure.initialize-schema

spring.ai.vectorstore.azure.index-name

spring_ai_azure_vector_store

spring.ai.vectorstore.azure.default-top-k

spring.ai.vectorstore.azure.default-similarity-threshold

spring.ai.vectorstore.azure.embedding-property

spring.ai.vectorstore.azure.index-name

spring-ai-document-index

To configure an Azure SearchIndexClient in your application, you can use the following code:

To create a vector store, you can use the following code by injecting the SearchIndexClient bean created in the above sample along with an EmbeddingModel provided by the Spring AI library that implements the desired Embeddings interface.

You must list explicitly all metadata field names and types for any metadata key used in the filter expression. The list above registers filterable metadata fields: country of type TEXT, year of type INT64, and active of type BOOLEAN.

If the filterable metadata fields are expanded with new entries, you have to (re)upload/update the documents with this metadata.

In your main code, create some documents:

Add the documents to your vector store:

And finally, retrieve documents similar to a query:

If all goes well, you should retrieve the document containing the text "Spring AI rocks!!".

You can leverage the generic, portable metadata filters with AzureVectorStore as well.

For example, you can use either the text expression language:

or programmatically using the expression DSL:

The portable filter expressions get automatically converted into the proprietary Azure Search OData filters. For example, the following portable filter expression:

is converted into the following Azure OData filter expression:

The Azure Vector Store implementation provides access to the underlying native Azure Search client (SearchClient) through the getNativeClient() method:

The native client gives you access to Azure Search-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (bash):
```bash
export AZURE_AI_SEARCH_API_KEY=<My AI Search API Key>
export AZURE_AI_SEARCH_ENDPOINT=<My AI Search Index>
export OPENAI_API_KEY=<My Azure AI API Key> (Optional)
```

Example 2 (xml):
```xml
<dependency>
   <groupId>org.springframework.ai</groupId>
   <artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

Example 3 (xml):
```xml
<dependency>
 <groupId>org.springframework.ai</groupId>
 <artifactId>spring-ai-starter-model-azure-openai</artifactId>
</dependency>
```

Example 4 (xml):
```xml
<dependency>
 <groupId>org.springframework.ai</groupId>
 <artifactId>spring-ai-starter-model-transformers</artifactId>
</dependency>
```

---

## Vector Databases :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs.html

**Contents:**
- Vector Databases
- API Overview
  - VectorStoreRetriever Interface
  - VectorStore Interface
  - SearchRequest Builder
- Schema Initialization
- Batching Strategy
  - Default Implementation
  - Working with Auto-Truncation
    - Configuration for Auto-Truncation

A vector database is a specialized type of database that plays an essential role in AI applications.

In vector databases, queries differ from traditional relational databases. Instead of exact matches, they perform similarity searches. When given a vector as a query, a vector database returns vectors that are “similar” to the query vector. Further details on how this similarity is calculated at a high-level is provided in a Vector Similarity.

Vector databases are used to integrate your data with AI models. The first step in their usage is to load your data into a vector database. Then, when a user query is to be sent to the AI model, a set of similar documents is first retrieved. These documents then serve as the context for the user’s question and are sent to the AI model, along with the user’s query. This technique is known as Retrieval Augmented Generation (RAG).

The following sections describe the Spring AI interface for using multiple vector database implementations and some high-level sample usage.

The last section is intended to demystify the underlying approach of similarity searching in vector databases.

This section serves as a guide to the VectorStore interface and its associated classes within the Spring AI framework.

Spring AI offers an abstracted API for interacting with vector databases through the VectorStore interface and its read-only counterpart, the VectorStoreRetriever interface.

Spring AI provides a read-only interface called VectorStoreRetriever that exposes only the document retrieval functionality:

This functional interface is designed for use cases where you only need to retrieve documents from a vector store without performing any mutation operations. It follows the principle of least privilege by exposing only the necessary functionality for document retrieval.

The VectorStore interface extends VectorStoreRetriever and adds mutation capabilities:

The VectorStore interface combines both read and write operations, allowing you to add, delete, and search for documents in a vector database.

To insert data into the vector database, encapsulate it within a Document object. The Document class encapsulates content from a data source, such as a PDF or Word document, and includes text represented as a string. It also contains metadata in the form of key-value pairs, including details such as the filename.

Upon insertion into the vector database, the text content is transformed into a numerical array, or a float[], known as vector embeddings, using an embedding model. Embedding models, such as Word2Vec, GLoVE, and BERT, or OpenAI’s text-embedding-ada-002, are used to convert words, sentences, or paragraphs into these vector embeddings.

The vector database’s role is to store and facilitate similarity searches for these embeddings. It does not generate the embeddings itself. For creating vector embeddings, the EmbeddingModel should be utilized.

The similaritySearch methods in the interface allow for retrieving documents similar to a given query string. These methods can be fine-tuned by using the following parameters:

k: An integer that specifies the maximum number of similar documents to return. This is often referred to as a 'top K' search, or 'K nearest neighbors' (KNN).

threshold: A double value ranging from 0 to 1, where values closer to 1 indicate higher similarity. By default, if you set a threshold of 0.75, for instance, only documents with a similarity above this value are returned.

Filter.Expression: A class used for passing a fluent DSL (Domain-Specific Language) expression that functions similarly to a 'where' clause in SQL, but it applies exclusively to the metadata key-value pairs of a Document.

filterExpression: An external DSL based on ANTLR4 that accepts filter expressions as strings. For example, with metadata keys like country, year, and isActive, you could use an expression such as: country == 'UK' && year >= 2020 && isActive == true.

Find more information on the Filter.Expression in the Metadata Filters section.

Some vector stores require their backend schema to be initialized before usage. It will not be initialized for you by default. You must opt-in, by passing a boolean for the appropriate constructor argument or, if using Spring Boot, setting the appropriate initialize-schema property to true in application.properties or application.yml. Check the documentation for the vector store you are using for the specific property name.

When working with vector stores, it’s often necessary to embed large numbers of documents. While it might seem straightforward to make a single call to embed all documents at once, this approach can lead to issues. Embedding models process text as tokens and have a maximum token limit, often referred to as the context window size. This limit restricts the amount of text that can be processed in a single embedding request. Attempting to embed too many tokens in one call can result in errors or truncated embeddings.

To address this token limit, Spring AI implements a batching strategy. This approach breaks down large sets of documents into smaller batches that fit within the embedding model’s maximum context window. Batching not only solves the token limit issue but can also lead to improved performance and more efficient use of API rate limits.

Spring AI provides this functionality through the BatchingStrategy interface, which allows for processing documents in sub-batches based on their token counts.

The core BatchingStrategy interface is defined as follows:

This interface defines a single method, batch, which takes a list of documents and returns a list of document batches.

Spring AI provides a default implementation called TokenCountBatchingStrategy. This strategy batches documents based on their token counts, ensuring that each batch does not exceed a calculated maximum input token count.

Key features of TokenCountBatchingStrategy:

Uses OpenAI’s max input token count (8191) as the default upper limit.

Incorporates a reserve percentage (default 10%) to provide a buffer for potential overhead.

Calculates the actual max input token count as: actualMaxInputTokenCount = originalMaxInputTokenCount * (1 - RESERVE_PERCENTAGE)

The strategy estimates the token count for each document, groups them into batches without exceeding the max input token count, and throws an exception if a single document exceeds this limit.

You can also customize the TokenCountBatchingStrategy to better suit your specific requirements. This can be done by creating a new instance with custom parameters in a Spring Boot @Configuration class.

Here’s an example of how to create a custom TokenCountBatchingStrategy bean:

In this configuration:

EncodingType.CL100K_BASE: Specifies the encoding type used for tokenization. This encoding type is used by the JTokkitTokenCountEstimator to accurately estimate token counts.

8000: Sets the maximum input token count. This value should be less than or equal to the maximum context window size of your embedding model.

0.1: Sets the reserve percentage. The percentage of tokens to reserve from the max input token count. This creates a buffer for potential token count increases during processing.

By default, this constructor uses Document.DEFAULT_CONTENT_FORMATTER for content formatting and MetadataMode.NONE for metadata handling. If you need to customize these parameters, you can use the full constructor with additional parameters.

Once defined, this custom TokenCountBatchingStrategy bean will be automatically used by the EmbeddingModel implementations in your application, replacing the default strategy.

The TokenCountBatchingStrategy internally uses a TokenCountEstimator (specifically, JTokkitTokenCountEstimator) to calculate token counts for efficient batching. This ensures accurate token estimation based on the specified encoding type.

Additionally, TokenCountBatchingStrategy provides flexibility by allowing you to pass in your own implementation of the TokenCountEstimator interface. This feature enables you to use custom token counting strategies tailored to your specific needs. For example:

Some embedding models, such as Vertex AI text embedding, support an auto_truncate feature. When enabled, the model silently truncates text inputs that exceed the maximum size and continues processing; when disabled, it throws an explicit error for inputs that are too large.

When using auto-truncation with the batching strategy, you must configure your batching strategy with a much higher input token count than the model’s actual maximum. This prevents the batching strategy from raising exceptions for large documents, allowing the embedding model to handle truncation internally.

When enabling auto-truncation, set your batching strategy’s maximum input token count much higher than the model’s actual limit. This prevents the batching strategy from raising exceptions for large documents, allowing the embedding model to handle truncation internally.

Here’s an example configuration for using Vertex AI with auto-truncation and custom BatchingStrategy and then using them in the PgVectorStore:

In this configuration:

The embedding model has auto-truncation enabled, allowing it to handle oversized inputs gracefully.

The batching strategy uses an artificially high token limit (132,900) that’s much larger than the actual model limit (20,000).

The vector store uses the configured embedding model and the custom BatchingStrategy bean.

This approach works because:

The TokenCountBatchingStrategy checks if any single document exceeds the configured maximum and throws an IllegalArgumentException if it does.

By setting a very high limit in the batching strategy, we ensure that this check never fails.

Documents or batches exceeding the model’s limit are silently truncated and processed by the embedding model’s auto-truncation feature.

When using auto-truncation:

Set the batching strategy’s max input token count to be at least 5-10x larger than the model’s actual limit to avoid premature exceptions from the batching strategy.

Monitor your logs for truncation warnings from the embedding model (note: not all models log truncation events).

Consider the implications of silent truncation on your embedding quality.

Test with sample documents to ensure truncated embeddings still meet your requirements.

Document this configuration for future maintainers, as it is non-standard.

If you’re using Spring Boot auto-configuration, you must provide a custom BatchingStrategy bean to override the default one that comes with Spring AI:

The presence of this bean in your application context will automatically replace the default batching strategy used by all vector stores.

While TokenCountBatchingStrategy provides a robust default implementation, you can customize the batching strategy to fit your specific needs. This can be done through Spring Boot’s auto-configuration.

To customize the batching strategy, define a BatchingStrategy bean in your Spring Boot application:

This custom BatchingStrategy will then be automatically used by the EmbeddingModel implementations in your application.

These are the available implementations of the VectorStore interface:

Azure Vector Search - The Azure vector store.

Apache Cassandra - The Apache Cassandra vector store.

Chroma Vector Store - The Chroma vector store.

Elasticsearch Vector Store - The Elasticsearch vector store.

GemFire Vector Store - The GemFire vector store.

MariaDB Vector Store - The MariaDB vector store.

Milvus Vector Store - The Milvus vector store.

MongoDB Atlas Vector Store - The MongoDB Atlas vector store.

Neo4j Vector Store - The Neo4j vector store.

OpenSearch Vector Store - The OpenSearch vector store.

Oracle Vector Store - The Oracle Database vector store.

PgVector Store - The PostgreSQL/PGVector vector store.

Pinecone Vector Store - Pinecone vector store.

Qdrant Vector Store - Qdrant vector store.

Redis Vector Store - The Redis vector store.

SAP Hana Vector Store - The SAP HANA vector store.

Typesense Vector Store - The Typesense vector store.

Weaviate Vector Store - The Weaviate vector store.

SimpleVectorStore - A simple implementation of persistent vector storage, good for educational purposes.

More implementations may be supported in future releases.

If you have a vector database that needs to be supported by Spring AI, open an issue on GitHub or, even better, submit a pull request with an implementation.

Information on each of the VectorStore implementations can be found in the subsections of this chapter.

To compute the embeddings for a vector database, you need to pick an embedding model that matches the higher-level AI model being used.

For example, with OpenAI’s ChatGPT, we use the OpenAiEmbeddingModel and a model named text-embedding-ada-002.

The Spring Boot starter’s auto-configuration for OpenAI makes an implementation of EmbeddingModel available in the Spring application context for dependency injection.

The general usage of loading data into a vector store is something you would do in a batch-like job, by first loading data into Spring AI’s Document class and then calling the add method on the VectorStore interface.

Given a String reference to a source file that represents a JSON file with data we want to load into the vector database, we use Spring AI’s JsonReader to load specific fields in the JSON, which splits them up into small pieces and then passes those small pieces to the vector store implementation. The VectorStore implementation computes the embeddings and stores the JSON and the embedding in the vector database:

Later, when a user question is passed into the AI model, a similarity search is done to retrieve similar documents, which are then "stuffed" into the prompt as context for the user’s question.

For read-only operations, you can use either the VectorStore interface or the more focused VectorStoreRetriever interface:

Additional options can be passed into the similaritySearch method to define how many documents to retrieve and a threshold of the similarity search.

Using the separate interfaces allows you to clearly define which components need write access and which only need read access:

This separation of concerns helps create more maintainable and secure applications by limiting access to mutation operations only to components that truly need them.

The VectorStoreRetriever interface provides a read-only view of a vector store, exposing only the similarity search functionality. This follows the principle of least privilege and is particularly useful in RAG (Retrieval-Augmented Generation) applications where you only need to retrieve documents without modifying the underlying data.

Separation of Concerns: Clearly separates read operations from write operations.

Interface Segregation: Clients that only need retrieval functionality aren’t exposed to mutation methods.

Functional Interface: Can be implemented with lambda expressions or method references for simple use cases.

Reduced Dependencies: Components that only need to perform searches don’t need to depend on the full VectorStore interface.

You can use VectorStoreRetriever directly when you only need to perform similarity searches:

In this example, the service only depends on the VectorStoreRetriever interface, making it clear that it only performs retrieval operations and doesn’t modify the vector store.

The VectorStoreRetriever interface is particularly useful in RAG applications, where you need to retrieve relevant documents to provide context for an AI model:

This pattern allows for a clean separation between the retrieval component and the generation component in RAG applications.

This section describes various filters that you can use against the results of a query.

You can pass in an SQL-like filter expressions as a String to one of the similaritySearch overloads.

Consider the following examples:

"genre == 'drama' && year >= 2020"

"genre in ['comedy', 'documentary', 'drama']"

You can create an instance of Filter.Expression with a FilterExpressionBuilder that exposes a fluent API. A simple example is as follows:

You can build up sophisticated expressions by using the following operators:

You can combine expressions by using the following operators:

Considering the following example:

You can also use the following operators:

Consider the following example:

You can also use the following operators:

Consider the following example:

The Vector Store interface provides multiple methods for deleting documents, allowing you to remove data either by specific document IDs or using filter expressions.

The simplest way to delete documents is by providing a list of document IDs:

This method removes all documents whose IDs match those in the provided list. If any ID in the list doesn’t exist in the store, it will be ignored.

For more complex deletion criteria, you can use filter expressions:

This method accepts a Filter.Expression object that defines the criteria for which documents should be deleted. It’s particularly useful when you need to delete documents based on their metadata properties.

For convenience, you can also delete documents using a string-based filter expression:

This method converts the provided string filter into a Filter.Expression object internally. It’s useful when you have filter criteria in string format.

All deletion methods may throw exceptions in case of errors:

The best practice is to wrap delete operations in try-catch blocks:

A common scenario is managing document versions where you need to upload a new version of a document while removing the old version. Here’s how to handle this using filter expressions:

You can also accomplish the same using the string filter expression:

Deleting by ID list is generally faster when you know exactly which documents to remove.

Filter-based deletion may require scanning the index to find matching documents; however, this is vector store implementation-specific.

Large deletion operations should be batched to avoid overwhelming the system.

Consider using filter expressions when deleting based on document properties rather than collecting IDs first.

Understanding Vectors

**Examples:**

Example 1 (java):
```java
@FunctionalInterface
public interface VectorStoreRetriever {

    List<Document> similaritySearch(SearchRequest request);

    default List<Document> similaritySearch(String query) {
        return this.similaritySearch(SearchRequest.builder().query(query).build());
    }
}
```

Example 2 (java):
```java
public interface VectorStore extends DocumentWriter, VectorStoreRetriever {

    default String getName() {
		return this.getClass().getSimpleName();
	}

    void add(List<Document> documents);

    void delete(List<String> idList);

    void delete(Filter.Expression filterExpression);

    default void delete(String filterExpression) { ... }

    default <T> Optional<T> getNativeClient() {
		return Optional.empty();
	}
}
```

Example 3 (java):
```java
public class SearchRequest {

	public static final double SIMILARITY_THRESHOLD_ACCEPT_ALL = 0.0;

	public static final int DEFAULT_TOP_K = 4;

	private String query = "";

	private int topK = DEFAULT_TOP_K;

	private double similarityThreshold = SIMILARITY_THRESHOLD_ACCEPT_ALL;

	@Nullable
	private Filter.Expression filterExpression;

    public static Builder from(SearchRequest originalSearchRequest) {
		return builder().query(originalSearchRequest.getQuery())
			.topK(originalSearchRequest.getTopK())
			.similarityThreshold(originalSearchRequest.getSimilarityThreshold())
			.filterExpression(originalSearchRequest.getFilterExpression());
	}

	public static class Builder {

		private final SearchRequest searchRequest = new SearchRequest();

		public Builder query(String query) {
			Assert.notNull(query, "Query can not be null.");
			this.searchRequest.query = query;
			return this;
		}

		public Builder topK(int topK) {
			Assert.isTrue(topK >= 0, "TopK should be positive.");
			this.searchRequest.topK = topK;
			return this;
		}

		public Builder similarityThreshold(double threshold) {
			Assert.isTrue(threshold >= 0 && threshold <= 1, "Similarity threshold must be in [0,1] range.");
			this.searchRequest.similarityThreshold = threshold;
			return this;
		}

		public Builder similarityThresholdAll() {
			this.searchRequest.similarityThreshold = 0.0;
			return this;
		}

		public Builder filterExpression(@Nullable Filter.Expression expression) {
			this.searchRequest.filterExpression = expression;
			return this;
		}

		public Builder filterExpression(@Nullable String textExpression) {
			this.searchRequest.filterExpression = (textExpression != null)
					? new FilterExpressionTextParser().parse(textExpression) : null;
			return this;
		}

		public SearchRequest build() {
			return this.searchRequest;
		}

	}

	public String getQuery() {...}
	public int getTopK() {...}
	public double getSimilarityThreshold() {...}
	public Filter.Expression getFilterExpression() {...}
}
```

Example 4 (java):
```java
public interface BatchingStrategy {
    List<List<Document>> batch(List<Document> documents);
}
```

---

## PGvector :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/pgvector.html

**Contents:**
- PGvector
- Prerequisites
- Auto-Configuration
  - Configuration properties
- Metadata filtering
- Manual Configuration
- Run Postgres & PGVector DB locally
- Accessing the Native Client

This section walks you through setting up the PGvector VectorStore to store document embeddings and perform similarity searches.

PGvector is an open-source extension for PostgreSQL that enables storing and searching over machine learning-generated embeddings. It provides different capabilities that let users identify both exact and approximate nearest neighbors. It is designed to work seamlessly with other PostgreSQL features, including indexing and querying.

First you need access to PostgreSQL instance with enabled vector, hstore and uuid-ossp extensions.

On startup with the schema initialization feature explicitly enabled, the PgVectorStore will attempt to install the required database extensions and create the required vector_store table with an index if not existing.

Optionally, you can do this manually like so:

Next, if required, an API key for the EmbeddingModel to generate the embeddings stored by the PgVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Then add the PgVectorStore boot starter dependency to your project:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the required schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

The Vector Store also requires an EmbeddingModel instance to calculate embeddings for the documents. You can pick one of the available EmbeddingModel Implementations.

For example, to use the OpenAI EmbeddingModel, add the following dependency to your project:

or to your Gradle build.gradle build file.

To connect to and configure the PgVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml.

Now you can auto-wire the VectorStore in your application and use it

You can use the following properties in your Spring Boot configuration to customize the PGVector vector store.

spring.ai.vectorstore.pgvector.index-type

Nearest neighbor search index type. Options are NONE - exact nearest neighbor search, IVFFlat - index divides vectors into lists, and then searches a subset of those lists that are closest to the query vector. It has faster build times and uses less memory than HNSW, but has lower query performance (in terms of speed-recall tradeoff). HNSW - creates a multilayer graph. It has slower build times and uses more memory than IVFFlat, but has better query performance (in terms of speed-recall tradeoff). There’s no training step like IVFFlat, so the index can be created without any data in the table.

spring.ai.vectorstore.pgvector.distance-type

Search distance type. Defaults to COSINE_DISTANCE. But if vectors are normalized to length 1, you can use EUCLIDEAN_DISTANCE or NEGATIVE_INNER_PRODUCT for best performance.

spring.ai.vectorstore.pgvector.dimensions

Embeddings dimension. If not specified explicitly the PgVectorStore will retrieve the dimensions form the provided EmbeddingModel. Dimensions are set to the embedding column the on table creation. If you change the dimensions your would have to re-create the vector_store table as well.

spring.ai.vectorstore.pgvector.remove-existing-vector-store-table

Deletes the existing vector_store table on start up.

spring.ai.vectorstore.pgvector.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.pgvector.schema-name

Vector store schema name

spring.ai.vectorstore.pgvector.table-name

Vector store table name

spring.ai.vectorstore.pgvector.schema-validation

Enables schema and table name validation to ensure they are valid and existing objects.

spring.ai.vectorstore.pgvector.max-document-batch-size

Maximum number of documents to process in a single batch.

You can leverage the generic, portable metadata filters with the PgVector store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

Instead of using the Spring Boot auto-configuration, you can manually configure the PgVectorStore. For this you need to add the PostgreSQL connection and JdbcTemplate auto-configuration dependencies to your project:

To configure PgVector in your application, you can use the following setup:

You can connect to this server like this:

The PGVector Store implementation provides access to the underlying native JDBC client (JdbcTemplate) through the getNativeClient() method:

The native client gives you access to PostgreSQL-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-vector-store-pgvector</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-pgvector'
}
```

Example 3 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

Example 4 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-openai'
}
```

---

## Redis :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/redis.html

**Contents:**
- Redis
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Metadata Filtering
- Manual Configuration
- Accessing the Native Client
- Distance Metrics
- HNSW Algorithm Configuration
- Text Search

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up RedisVectorStore to store document embeddings and perform similarity searches.

Redis is an open source (BSD licensed), in-memory data structure store used as a database, cache, message broker, and streaming engine. Redis provides data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes, and streams.

Redis Search and Query extends the core features of Redis OSS and allows you to use Redis as a vector database:

Store vectors and the associated metadata within hashes or JSON documents

Perform vector similarity searches (KNN)

Perform range-based vector searches with radius threshold

Perform full-text searches on TEXT fields

Support for multiple distance metrics (COSINE, L2, IP) and vector algorithms (HNSW, FLAT)

A Redis Stack instance

Redis Cloud (recommended)

Docker image redis/redis-stack:latest

EmbeddingModel instance to compute the document embeddings. Several options are available:

If required, an API key for the EmbeddingModel to generate the embeddings stored by the RedisVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Redis Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the RedisVectorStore as a vector store in your application.

To connect to Redis and use the RedisVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml,

For redis connection configuration, alternatively, a simple configuration can be provided via Spring Boot’s application.properties.

Properties starting with spring.ai.vectorstore.redis.* are used to configure the RedisVectorStore:

spring.ai.vectorstore.redis.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.redis.index-name

The name of the index to store the vectors

spring.ai.vectorstore.redis.prefix

The prefix for Redis keys

spring.ai.vectorstore.redis.distance-metric

Distance metric for vector similarity (COSINE, L2, IP)

spring.ai.vectorstore.redis.vector-algorithm

Vector indexing algorithm (HNSW, FLAT)

spring.ai.vectorstore.redis.hnsw-m

HNSW: Number of maximum outgoing connections

spring.ai.vectorstore.redis.hnsw-ef-construction

HNSW: Number of maximum connections during index building

spring.ai.vectorstore.redis.hnsw-ef-runtime

HNSW: Number of connections to consider during search

spring.ai.vectorstore.redis.default-range-threshold

Default radius threshold for range searches

spring.ai.vectorstore.redis.text-scorer

Text scoring algorithm (BM25, TFIDF, BM25STD, DISMAX, DOCSCORE)

You can leverage the generic, portable metadata filters with Redis as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Redis filter format:

Instead of using the Spring Boot auto-configuration, you can manually configure the Redis vector store. For this you need to add the spring-ai-redis-store to your project:

or to your Gradle build.gradle build file.

Create a JedisPooled bean:

Then create the RedisVectorStore bean using the builder pattern:

You must list explicitly all metadata field names and types (TAG, TEXT, or NUMERIC) for any metadata field used in filter expressions. The metadataFields above registers filterable metadata fields: country of type TAG, year of type NUMERIC.

The Redis Vector Store implementation provides access to the underlying native Redis client (JedisPooled) through the getNativeClient() method:

The native client gives you access to Redis-specific features and operations that might not be exposed through the VectorStore interface.

The Redis Vector Store supports three distance metrics for vector similarity:

COSINE: Cosine similarity (default) - measures the cosine of the angle between vectors

L2: Euclidean distance - measures the straight-line distance between vectors

IP: Inner Product - measures the dot product between vectors

Each metric is automatically normalized to a 0-1 similarity score, where 1 is most similar.

The Redis Vector Store uses the HNSW (Hierarchical Navigable Small World) algorithm by default for efficient approximate nearest neighbor search. You can tune the HNSW parameters for your specific use case:

Parameter guidelines:

M: Higher values improve recall but increase memory usage and index time. Typical values: 12-48.

EF_CONSTRUCTION: Higher values improve index quality but increase build time. Typical values: 100-500.

EF_RUNTIME: Higher values improve search accuracy but increase latency. Typical values: 10-100.

For smaller datasets or when exact results are required, use the FLAT algorithm instead:

The Redis Vector Store provides text search capabilities using Redis Query Engine’s full-text search features. This allows you to find documents based on keywords and phrases in TEXT fields:

Text search supports:

Phrase searches with exact matching when inOrder is true

Term-based searches with OR semantics when inOrder is false

Stopword filtering to ignore common words

Multiple text scoring algorithms

Configure text search behavior at construction time:

Several text scoring algorithms are available:

BM25: Modern version of TF-IDF with term saturation (default)

TFIDF: Classic term frequency-inverse document frequency

BM25STD: Standardized BM25

DISMAX: Disjunction max

DOCSCORE: Document score

Scores are normalized to a 0-1 range for consistency with vector similarity scores.

The range search returns all documents within a specified radius threshold, rather than a fixed number of nearest neighbors:

You can also set a default range threshold at construction time:

Range search is useful when you want to retrieve all relevant documents above a similarity threshold, rather than limiting to a specific count.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-redis</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-redis'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List <Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Redis
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = this.vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  data:
    redis:
      url: <redis instance url>
  ai:
    vectorstore:
      redis:
        initialize-schema: true
        index-name: custom-index
        prefix: custom-prefix
```

---

## Neo4j :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/neo4j.html

**Contents:**
- Neo4j
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Accessing the Native Client

For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up Neo4jVectorStore to store document embeddings and perform similarity searches.

Neo4j is an open-source NoSQL graph database. It is a fully transactional database (ACID) that stores data structured as graphs consisting of nodes, connected by relationships. Inspired by the structure of the real world, it allows for high query performance on complex data while remaining intuitive and simple for the developer.

The Neo4j’s Vector Search allows users to query vector embeddings from large datasets. An embedding is a numerical representation of a data object, such as text, image, audio, or document. Embeddings can be stored on Node properties and can be queried with the db.index.vector.queryNodes() function. Those indexes are powered by Lucene using a Hierarchical Navigable Small World Graph (HNSW) to perform a k approximate nearest neighbors (k-ANN) query over the vector fields.

A running Neo4j (5.15+) instance. The following options are available:

Neo4j Server instance

If required, an API key for the EmbeddingModel to generate the embeddings stored by the Neo4jVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Neo4j Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Please have a look at the list of Configuration Properties for the vector store to learn about the default values and configuration options.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the Neo4jVectorStore as a vector store in your application.

To connect to Neo4j and use the Neo4jVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

The Spring Boot properties starting with spring.neo4j.* are used to configure the Neo4j client:

URI for connecting to the Neo4j instance

neo4j://localhost:7687

spring.neo4j.authentication.username

Username for authentication with Neo4j

spring.neo4j.authentication.password

Password for authentication with Neo4j

Properties starting with spring.ai.vectorstore.neo4j.* are used to configure the Neo4jVectorStore:

spring.ai.vectorstore.neo4j.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.neo4j.database-name

The name of the Neo4j database to use

spring.ai.vectorstore.neo4j.index-name

The name of the index to store the vectors

spring-ai-document-index

spring.ai.vectorstore.neo4j.embedding-dimension

The number of dimensions in the vector

spring.ai.vectorstore.neo4j.distance-type

The distance function to use

spring.ai.vectorstore.neo4j.label

The label used for document nodes

spring.ai.vectorstore.neo4j.embedding-property

The property name used to store embeddings

The following distance functions are available:

cosine - Default, suitable for most use cases. Measures cosine similarity between vectors.

euclidean - Euclidean distance between vectors. Lower values indicate higher similarity.

Instead of using the Spring Boot auto-configuration, you can manually configure the Neo4j vector store. For this you need to add the spring-ai-neo4j-store to your project:

or to your Gradle build.gradle build file.

Create a Neo4j Driver bean. Read the Neo4j Documentation for more in-depth information about the configuration of a custom driver.

Then create the Neo4jVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with Neo4j store as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Neo4j filter format:

The Neo4j Vector Store implementation provides access to the underlying native Neo4j client (Driver) through the getNativeClient() method:

The native client gives you access to Neo4j-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-neo4j</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-neo4j'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Neo4j
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  neo4j:
    uri: <neo4j instance URI>
    authentication:
      username: <neo4j username>
      password: <neo4j password>
  ai:
    vectorstore:
      neo4j:
        initialize-schema: true
        database-name: neo4j
        index-name: custom-index
        embedding-dimension: 1536
        distance-type: cosine
```

---

## MongoDB Atlas :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/mongodb.html

**Contents:**
- MongoDB Atlas
- What is MongoDB Atlas?
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Tutorials and Code Examples
- Accessing the Native Client

This section walks you through setting up MongoDB Atlas as a vector store to use with Spring AI.

MongoDB Atlas is the fully-managed cloud database from MongoDB available in AWS, Azure, and GCP. Atlas supports native Vector Search and full text search on your MongoDB document data.

MongoDB Atlas Vector Search allows you to store your embeddings in MongoDB documents, create vector search indexes, and perform KNN searches with an approximate nearest neighbor algorithm (Hierarchical Navigable Small Worlds). You can use the $vectorSearch aggregation operator in a MongoDB aggregation stage to perform a search on your vector embeddings.

An Atlas cluster running MongoDB version 6.0.11, 7.0.2, or later. To get started with MongoDB Atlas, you can follow the instructions here. Ensure that your IP address is included in your Atlas project’s access list.

A running MongoDB Atlas instance with Vector Search enabled

Collection with vector search index configured

Collection schema with id (string), content (string), metadata (document), and embedding (vector) fields

Proper access permissions for index and collection operations

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the MongoDB Atlas Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

The vector store implementation can initialize the requisite schema for you, but you must opt-in by setting spring.ai.vectorstore.mongodb.initialize-schema=true in the application.properties file. Alternatively you can opt-out the initialization and create the index manually using the MongoDB Atlas UI, Atlas Administration API, or Atlas CLI, which can be useful if the index needs advanced mapping or additional configuration.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the MongoDBAtlasVectorStore as a vector store in your application:

To connect to MongoDB Atlas and use the MongoDBAtlasVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.mongodb.* are used to configure the MongoDBAtlasVectorStore:

spring.ai.vectorstore.mongodb.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.mongodb.collection-name

The name of the collection to store the vectors

spring.ai.vectorstore.mongodb.index-name

The name of the vector search index

spring.ai.vectorstore.mongodb.path-name

The path where vectors are stored

spring.ai.vectorstore.mongodb.metadata-fields-to-filter

Comma-separated list of metadata fields that can be used for filtering

Instead of using the Spring Boot auto-configuration, you can manually configure the MongoDB Atlas vector store. For this you need to add the spring-ai-mongodb-atlas-store to your project:

or to your Gradle build.gradle build file:

Create a MongoTemplate bean:

Then create the MongoDBAtlasVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with MongoDB Atlas as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary MongoDB Atlas filter format:

To get started with Spring AI and MongoDB:

See the Getting Started guide for Spring AI Integration.

For a comprehensive code example demonstrating Retrieval Augmented Generation (RAG) with Spring AI and MongoDB, refer to this detailed tutorial.

The MongoDB Atlas Vector Store implementation provides access to the underlying native MongoDB client (MongoClient) through the getNativeClient() method:

The native client gives you access to MongoDB-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-mongodb-atlas</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-mongodb-atlas'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to MongoDB Atlas
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  data:
    mongodb:
      uri: <mongodb atlas connection string>
      database: <database name>
  ai:
    vectorstore:
      mongodb:
        initialize-schema: true
        collection-name: custom_vector_store
        index-name: custom_vector_index
        path-name: custom_embedding
        metadata-fields-to-filter: author,year
```

---

## Qdrant :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/qdrant.html

**Contents:**
- Qdrant
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Accessing the Native Client

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up the Qdrant VectorStore to store document embeddings and perform similarity searches.

Qdrant is an open-source, high-performance vector search engine/database. It uses HNSW (Hierarchical Navigable Small World) algorithm for efficient k-NN search operations and provides advanced filtering capabilities for metadata-based queries.

Qdrant Instance: Set up a Qdrant instance by following the installation instructions in the Qdrant documentation.

If required, an API key for the EmbeddingModel to generate the embeddings stored by the QdrantVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Qdrant Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the builder or by setting …​initialize-schema=true in the application.properties file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the QdrantVectorStore as a vector store in your application.

To connect to Qdrant and use the QdrantVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.qdrant.* are used to configure the QdrantVectorStore:

spring.ai.vectorstore.qdrant.host

The host of the Qdrant server

spring.ai.vectorstore.qdrant.port

The gRPC port of the Qdrant server

spring.ai.vectorstore.qdrant.api-key

The API key to use for authentication

spring.ai.vectorstore.qdrant.collection-name

The name of the collection to use

spring.ai.vectorstore.qdrant.use-tls

Whether to use TLS(HTTPS)

spring.ai.vectorstore.qdrant.initialize-schema

Whether to initialize the schema

Instead of using the Spring Boot auto-configuration, you can manually configure the Qdrant vector store. For this you need to add the spring-ai-qdrant-store to your project:

or to your Gradle build.gradle build file.

Create a Qdrant client bean:

Then create the QdrantVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with Qdrant store as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

The Qdrant Vector Store implementation provides access to the underlying native Qdrant client (QdrantClient) through the getNativeClient() method:

The native client gives you access to Qdrant-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-qdrant</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-qdrant'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Qdrant
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  ai:
    vectorstore:
      qdrant:
        host: <qdrant host>
        port: <qdrant grpc port>
        api-key: <qdrant api key>
        collection-name: <collection name>
        use-tls: false
        initialize-schema: true
```

---

## SAP HANA Cloud :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/hana.html

**Contents:**
- SAP HANA Cloud
- Prerequisites
- Auto-configuration
- HanaCloudVectorStore Properties
- Build a Sample RAG application
  - Create an Entity class named CricketWorldCup that extends from HanaVectorEntity:

For the latest snapshot version, please use Spring AI 1.1.2!

You need a SAP HANA Cloud vector engine account - Refer SAP HANA Cloud vector engine - provision a trial account guide to create a trial account.

If required, an API key for the EmbeddingModel to generate the embeddings stored by the vector store.

Spring AI does not provide a dedicated module for SAP Hana vector store. Users are expected to provide their own configuration in the applications using the standard vector store module for SAP Hana vector store in Spring AI - spring-ai-hanadb-store.

Please have a look at the list of HanaCloudVectorStore Properties for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

You can use the following properties in your Spring Boot configuration to customize the SAP Hana vector store. It uses spring.datasource. properties to configure the Hana datasource and the spring.ai.vectorstore.hanadb. properties to configure the Hana vector store.

spring.datasource.driver-class-name

com.sap.db.jdbc.Driver

spring.datasource.url

spring.datasource.username

Hana datasource username

spring.datasource.password

Hana datasource password

spring.ai.vectorstore.hanadb.top-k

spring.ai.vectorstore.hanadb.table-name

spring.ai.vectorstore.hanadb.initialize-schema

whether to initialize the required schema

Shows how to setup a project that uses SAP Hana Cloud as the vector DB and leverage OpenAI to implement RAG pattern

Create a table CRICKET_WORLD_CUP in SAP Hana DB:

Add the following dependencies in your pom.xml

You may set the property spring-ai-version as <spring-ai-version>1.0.0-SNAPSHOT</spring-ai-version>:

Add the following properties in application.properties file:

Create a Repository named CricketWorldCupRepository that implements HanaVectorRepository interface:

Now, create a REST Controller class CricketWorldCupHanaController, and autowire ChatModel and VectorStore as dependencies In this controller class, create the following REST endpoints:

/ai/hana-vector-store/cricket-world-cup/purge-embeddings - to purge all the embeddings from the Vector Store

/ai/hana-vector-store/cricket-world-cup/upload - to upload the Cricket_World_Cup.pdf so that its data gets stored in SAP Hana Cloud Vector DB as embeddings

/ai/hana-vector-store/cricket-world-cup - to implement RAG using Cosine_Similarity in SAP Hana DB

Since HanaDB vector store support does not provide the autoconfiguration module, you also need to provide the vector store bean in your application, as shown below, as an example.

Use a contextual pdf file from wikipedia

Go to wikipedia and download Cricket World Cup page as a PDF file.

Upload this PDF file using the file-upload REST endpoint that we created in the previous step.

**Examples:**

Example 1 (xml):
```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-bom</artifactId>
            <version>${spring-ai-version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-pdf-document-reader</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-hana</artifactId>
</dependency>

<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.30</version>
    <scope>provided</scope>
</dependency>
```

Example 2 (java):
```java
package com.interviewpedia.spring.ai.hana;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.Table;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.extern.jackson.Jacksonized;
import org.springframework.ai.vectorstore.hanadb.HanaVectorEntity;

@Entity
@Table(name = "CRICKET_WORLD_CUP")
@Data
@Jacksonized
@NoArgsConstructor
public class CricketWorldCup extends HanaVectorEntity {
    @Column(name = "content")
    private String content;
}
```

Example 3 (java):
```java
package com.interviewpedia.spring.ai.hana;

import jakarta.persistence.EntityManager;
import jakarta.persistence.PersistenceContext;
import jakarta.transaction.Transactional;
import org.springframework.ai.vectorstore.hanadb.HanaVectorRepository;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public class CricketWorldCupRepository implements HanaVectorRepository<CricketWorldCup> {
    @PersistenceContext
    private EntityManager entityManager;

    @Override
    @Transactional
    public void save(String tableName, String id, String embedding, String content) {
        String sql = String.format("""
                INSERT INTO %s (_ID, EMBEDDING, CONTENT)
                VALUES(:_id, TO_REAL_VECTOR(:embedding), :content)
                """, tableName);

		this.entityManager.createNativeQuery(sql)
                .setParameter("_id", id)
                .setParameter("embedding", embedding)
                .setParameter("content", content)
                .executeUpdate();
    }

    @Override
    @Transactional
    public int deleteEmbeddingsById(String tableName, List<String> idList) {
        String sql = String.format("""
                DELETE FROM %s WHERE _ID IN (:ids)
                """, tableName);

        return this.entityManager.createNativeQuery(sql)
                .setParameter("ids", idList)
                .executeUpdate();
    }

    @Override
    @Transactional
    public int deleteAllEmbeddings(String tableName) {
        String sql = String.format("""
                DELETE FROM %s
                """, tableName);

        return this.entityManager.createNativeQuery(sql).executeUpdate();
    }

    @Override
    public List<CricketWorldCup> cosineSimilaritySearch(String tableName, int topK, String queryEmbedding) {
        String sql = String.format("""
                SELECT TOP :topK * FROM %s
                ORDER BY COSINE_SIMILARITY(EMBEDDING, TO_REAL_VECTOR(:queryEmbedding)) DESC
                """, tableName);

        return this.entityManager.createNativeQuery(sql, CricketWorldCup.class)
                .setParameter("topK", topK)
                .setParameter("queryEmbedding", queryEmbedding)
                .getResultList();
    }
}
```

Example 4 (java):
```java
package com.interviewpedia.spring.ai.hana;

import lombok.extern.slf4j.Slf4j;
import org.springframework.ai.chat.model.ChatModel;
import org.springframework.ai.chat.messages.UserMessage;
import org.springframework.ai.chat.prompt.Prompt;
import org.springframework.ai.chat.prompt.SystemPromptTemplate;
import org.springframework.ai.document.Document;
import org.springframework.ai.reader.pdf.PagePdfDocumentReader;
import org.springframework.ai.transformer.splitter.TokenTextSplitter;
import org.springframework.ai.vectorstore.hanadb.HanaCloudVectorStore;
import org.springframework.ai.vectorstore.VectorStore;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.io.Resource;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;
import java.util.List;
import java.util.Map;
import java.util.function.Function;
import java.util.function.Supplier;
import java.util.stream.Collectors;

@RestController
@Slf4j
public class CricketWorldCupHanaController {
    private final VectorStore hanaCloudVectorStore;
    private final ChatModel chatModel;

    @Autowired
    public CricketWorldCupHanaController(ChatModel chatModel, VectorStore hanaCloudVectorStore) {
        this.chatModel = chatModel;
        this.hanaCloudVectorStore = hanaCloudVectorStore;
    }

    @PostMapping("/ai/hana-vector-store/cricket-world-cup/purge-embeddings")
    public ResponseEntity<String> purgeEmbeddings() {
        int deleteCount = ((HanaCloudVectorStore) this.hanaCloudVectorStore).purgeEmbeddings();
        log.info("{} embeddings purged from CRICKET_WORLD_CUP table in Hana DB", deleteCount);
        return ResponseEntity.ok().body(String.format("%d embeddings purged from CRICKET_WORLD_CUP table in Hana DB", deleteCount));
    }

    @PostMapping("/ai/hana-vector-store/cricket-world-cup/upload")
    public ResponseEntity<String> handleFileUpload(@RequestParam("pdf") MultipartFile file) throws IOException {
        Resource pdf = file.getResource();
        Supplier<List<Document>> reader = new PagePdfDocumentReader(pdf);
        Function<List<Document>, List<Document>> splitter = new TokenTextSplitter();
        List<Document> documents = splitter.apply(reader.get());
        log.info("{} documents created from pdf file: {}", documents.size(), pdf.getFilename());
		this.hanaCloudVectorStore.accept(documents);
        return ResponseEntity.ok().body(String.format("%d documents created from pdf file: %s",
                documents.size(), pdf.getFilename()));
    }

    @GetMapping("/ai/hana-vector-store/cricket-world-cup")
    public Map<String, String> hanaVectorStoreSearch(@RequestParam(value = "message") String message) {
        var documents = this.hanaCloudVectorStore.similaritySearch(message);
        var inlined = documents.stream().map(Document::getText).collect(Collectors.joining(System.lineSeparator()));
        var similarDocsMessage = new SystemPromptTemplate("Based on the following: {documents}")
                .createMessage(Map.of("documents", inlined));

        var userMessage = new UserMessage(message);
        Prompt prompt = new Prompt(List.of(similarDocsMessage, userMessage));
        String generation = this.chatModel.call(prompt).getResult().getOutput().getContent();
        log.info("Generation: {}", generation);
        return Map.of("generation", generation);
    }
}
```

---

## Pinecone :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/pinecone.html

**Contents:**
- Pinecone
- Prerequisites
- Auto-configuration
  - Configuration properties
- Metadata filtering
- Manual Configuration
  - Sample Code
- Accessing the Native Client

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up the Pinecone VectorStore to store document embeddings and perform similarity searches.

Pinecone is a popular cloud-based vector database, which allows you to store and search vectors efficiently.

Pinecone Account: Before you start, sign up for a Pinecone account.

Pinecone Project: Once registered, generate an API key and create and index. You’ll need these details for configuration.

EmbeddingModel instance to compute the document embeddings. Several options are available:

If required, an API key for the EmbeddingModel to generate the embeddings stored by the PineconeVectorStore.

To set up PineconeVectorStore, gather the following details from your Pinecone account:

This information is available to you in the Pinecone UI portal. The namespace support is not available in the Pinecone free tier.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Pinecone Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Here is an example of the needed bean:

To connect to Pinecone you need to provide access details for your instance. A simple configuration can either be provided via Spring Boot’s application.properties,

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Now you can Auto-wire the Pinecone Vector Store in your application and use it

You can use the following properties in your Spring Boot configuration to customize the Pinecone vector store.

spring.ai.vectorstore.pinecone.api-key

spring.ai.vectorstore.pinecone.index-name

spring.ai.vectorstore.pinecone.namespace

spring.ai.vectorstore.pinecone.content-field-name

Pinecone metadata field name used to store the original text content.

spring.ai.vectorstore.pinecone.distance-metadata-field-name

Pinecone metadata field name used to store the computed distance.

spring.ai.vectorstore.pinecone.server-side-timeout

You can leverage the generic, portable metadata filters with the Pinecone store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

If you prefer to configure PineconeVectorStore manually, you can do so by using the PineconeVectorStore#Builder.

Add these dependencies to your project:

OpenAI: Required for calculating embeddings.

To configure Pinecone in your application, you can use the following setup:

In your main code, create some documents:

Add the documents to Pinecone:

And finally, retrieve documents similar to a query:

If all goes well, you should retrieve the document containing the text "Spring AI rocks!!".

The Pinecone Vector Store implementation provides access to the underlying native Pinecone client (PineconeConnection) through the getNativeClient() method:

The native client gives you access to Pinecone-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-pinecone</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-pinecone'
}
```

Example 3 (java):
```java
@Bean
public EmbeddingModel embeddingModel() {
    // Can be any other EmbeddingModel implementation.
    return new OpenAiEmbeddingModel(new OpenAiApi(System.getenv("OPENAI_API_KEY")));
}
```

Example 4 (jsx):
```jsx
spring.ai.vectorstore.pinecone.apiKey=<your api key>
spring.ai.vectorstore.pinecone.index-name=<your index name>

# API key if needed, e.g. OpenAI
spring.ai.openai.api.key=<api-key>
```

---

## Azure AI Service :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/azure.html

**Contents:**
- Azure AI Service
- Prerequisites
- Configuration
- Dependencies
  - 1. Select an Embeddings interface implementation. You can choose between:
  - 2. Azure (AI Search) Vector Store
- Configuration Properties
- Sample Code
  - Metadata filtering
- Accessing the Native Client

This section will walk you through setting up the AzureVectorStore to store document embeddings and perform similarity searches using the Azure AI Search Service.

Azure AI Search is a versatile cloud-hosted cloud information retrieval system that is part of Microsoft’s larger AI platform. Among other features, it allows users to query information using vector-based storage and retrieval.

Azure Subscription: You will need an Azure subscription to use any Azure service.

Azure AI Search Service: Create an AI Search service. Once the service is created, obtain the admin apiKey from the Keys section under Settings and retrieve the endpoint from the Url field under the Overview section.

(Optional) Azure OpenAI Service: Create an Azure OpenAI service. NOTE: You may have to fill out a separate form to gain access to Azure Open AI services. Once the service is created, obtain the endpoint and apiKey from the Keys and Endpoint section under Resource Management.

On startup, the AzureVectorStore can attempt to create a new index within your AI Search service instance if you’ve opted in by setting the relevant initialize-schema boolean property to true in the constructor or, if using Spring Boot, setting …​initialize-schema=true in your application.properties file.

Alternatively, you can create the index manually.

To set up an AzureVectorStore, you will need the settings retrieved from the prerequisites above along with your index name:

Azure AI Search Endpoint

(optional) Azure OpenAI API Endpoint

(optional) Azure OpenAI API Key

You can provide these values as OS environment variables.

You can replace Azure Open AI implementation with any valid OpenAI implementation that supports the Embeddings interface. For example, you could use Spring AI’s Open AI or TransformersEmbedding implementations for embeddings instead of the Azure implementation.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add these dependencies to your project:

Local Sentence Transformers Embedding

You can use the following properties in your Spring Boot configuration to customize the Azure vector store.

spring.ai.vectorstore.azure.url

spring.ai.vectorstore.azure.api-key

spring.ai.vectorstore.azure.useKeylessAuth

spring.ai.vectorstore.azure.initialize-schema

spring.ai.vectorstore.azure.index-name

spring_ai_azure_vector_store

spring.ai.vectorstore.azure.default-top-k

spring.ai.vectorstore.azure.default-similarity-threshold

spring.ai.vectorstore.azure.embedding-property

spring.ai.vectorstore.azure.index-name

spring-ai-document-index

To configure an Azure SearchIndexClient in your application, you can use the following code:

To create a vector store, you can use the following code by injecting the SearchIndexClient bean created in the above sample along with an EmbeddingModel provided by the Spring AI library that implements the desired Embeddings interface.

You must list explicitly all metadata field names and types for any metadata key used in the filter expression. The list above registers filterable metadata fields: country of type TEXT, year of type INT64, and active of type BOOLEAN.

If the filterable metadata fields are expanded with new entries, you have to (re)upload/update the documents with this metadata.

In your main code, create some documents:

Add the documents to your vector store:

And finally, retrieve documents similar to a query:

If all goes well, you should retrieve the document containing the text "Spring AI rocks!!".

You can leverage the generic, portable metadata filters with AzureVectorStore as well.

For example, you can use either the text expression language:

or programmatically using the expression DSL:

The portable filter expressions get automatically converted into the proprietary Azure Search OData filters. For example, the following portable filter expression:

is converted into the following Azure OData filter expression:

The Azure Vector Store implementation provides access to the underlying native Azure Search client (SearchClient) through the getNativeClient() method:

The native client gives you access to Azure Search-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (bash):
```bash
export AZURE_AI_SEARCH_API_KEY=<My AI Search API Key>
export AZURE_AI_SEARCH_ENDPOINT=<My AI Search Index>
export OPENAI_API_KEY=<My Azure AI API Key> (Optional)
```

Example 2 (xml):
```xml
<dependency>
   <groupId>org.springframework.ai</groupId>
   <artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

Example 3 (xml):
```xml
<dependency>
 <groupId>org.springframework.ai</groupId>
 <artifactId>spring-ai-starter-model-azure-openai</artifactId>
</dependency>
```

Example 4 (xml):
```xml
<dependency>
 <groupId>org.springframework.ai</groupId>
 <artifactId>spring-ai-starter-model-transformers</artifactId>
</dependency>
```

---

## Understanding Vectors :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/understand-vectordbs.html

**Contents:**
- Understanding Vectors
- Similarity

For the latest snapshot version, please use Spring AI 1.1.2!

Vectors have dimensionality and a direction. For example, the following image depicts a two-dimensional vector in the cartesian coordinate system pictured as an arrow.

The head of the vector is at the point . The x coordinate value is and the y coordinate value is . The coordinates are also referred to as the components of the vector.

Several mathematical formulas can be used to determine if two vectors are similar. One of the most intuitive to visualize and understand is cosine similarity. Consider the following images that show three sets of graphs:

The vectors and are considered similar, when they are pointing close to each other, as in the first diagram. The vectors are considered unrelated when pointing perpendicular to each other and opposite when they point away from each other.

The angle between them, , is a good measure of their similarity. How can the angle be computed?

We are all familiar with the Pythagorean Theorem.

What about when the angle between a and b is not 90 degrees?

Enter the Law of cosines.

The following image shows this approach as a vector diagram:

The magnitude of this vector is defined in terms of its components as:

The dot product between two vectors and is defined in terms of its components as:

Rewriting the Law of Cosines with vector magnitudes and dot products gives the following:

Replacing with gives the following:

Expanding this out gives us the formula for Cosine Similarity.

This formula works for dimensions higher than 2 or 3, though it is hard to visualize. However, it can be visualized to some extent. It is common for vectors in AI/ML applications to have hundreds or even thousands of dimensions.

The similarity function in higher dimensions using the components of the vector is shown below. It expands the two-dimensional definitions of Magnitude and Dot Product given previously to N dimensions by using Summation mathematical syntax.

This is the key formula used in the simple implementation of a vector store and can be found in the SimpleVectorStore implementation.

---

## MongoDB Atlas :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/mongodb.html

**Contents:**
- MongoDB Atlas
- What is MongoDB Atlas?
- Prerequisites
- Auto-configuration
  - Configuration Properties
- Manual Configuration
- Metadata Filtering
- Tutorials and Code Examples
- Accessing the Native Client

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up MongoDB Atlas as a vector store to use with Spring AI.

MongoDB Atlas is the fully-managed cloud database from MongoDB available in AWS, Azure, and GCP. Atlas supports native Vector Search and full text search on your MongoDB document data.

MongoDB Atlas Vector Search allows you to store your embeddings in MongoDB documents, create vector search indexes, and perform KNN searches with an approximate nearest neighbor algorithm (Hierarchical Navigable Small Worlds). You can use the $vectorSearch aggregation operator in a MongoDB aggregation stage to perform a search on your vector embeddings.

An Atlas cluster running MongoDB version 6.0.11, 7.0.2, or later. To get started with MongoDB Atlas, you can follow the instructions here. Ensure that your IP address is included in your Atlas project’s access list.

A running MongoDB Atlas instance with Vector Search enabled

Collection with vector search index configured

Collection schema with id (string), content (string), metadata (document), and embedding (vector) fields

Proper access permissions for index and collection operations

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the MongoDB Atlas Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

The vector store implementation can initialize the requisite schema for you, but you must opt-in by setting spring.ai.vectorstore.mongodb.initialize-schema=true in the application.properties file. Alternatively you can opt-out the initialization and create the index manually using the MongoDB Atlas UI, Atlas Administration API, or Atlas CLI, which can be useful if the index needs advanced mapping or additional configuration.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the MongoDBAtlasVectorStore as a vector store in your application:

To connect to MongoDB Atlas and use the MongoDBAtlasVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml:

Properties starting with spring.ai.vectorstore.mongodb.* are used to configure the MongoDBAtlasVectorStore:

spring.ai.vectorstore.mongodb.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.mongodb.collection-name

The name of the collection to store the vectors

spring.ai.vectorstore.mongodb.index-name

The name of the vector search index

spring.ai.vectorstore.mongodb.path-name

The path where vectors are stored

spring.ai.vectorstore.mongodb.metadata-fields-to-filter

Comma-separated list of metadata fields that can be used for filtering

Instead of using the Spring Boot auto-configuration, you can manually configure the MongoDB Atlas vector store. For this you need to add the spring-ai-mongodb-atlas-store to your project:

or to your Gradle build.gradle build file:

Create a MongoTemplate bean:

Then create the MongoDBAtlasVectorStore bean using the builder pattern:

You can leverage the generic, portable metadata filters with MongoDB Atlas as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary MongoDB Atlas filter format:

To get started with Spring AI and MongoDB:

See the Getting Started guide for Spring AI Integration.

For a comprehensive code example demonstrating Retrieval Augmented Generation (RAG) with Spring AI and MongoDB, refer to this detailed tutorial.

The MongoDB Atlas Vector Store implementation provides access to the underlying native MongoDB client (MongoClient) through the getNativeClient() method:

The native client gives you access to MongoDB-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-mongodb-atlas</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-mongodb-atlas'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List<Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to MongoDB Atlas
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.builder().query("Spring").topK(5).build());
```

Example 4 (yaml):
```yaml
spring:
  data:
    mongodb:
      uri: <mongodb atlas connection string>
      database: <database name>
  ai:
    vectorstore:
      mongodb:
        initialize-schema: true
        collection-name: custom_vector_store
        index-name: custom_vector_index
        path-name: custom_embedding
        metadata-fields-to-filter: author,year
```

---

## SAP HANA Cloud :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/hana.html

**Contents:**
- SAP HANA Cloud
- Prerequisites
- Auto-configuration
- HanaCloudVectorStore Properties
- Build a Sample RAG application
  - Create an Entity class named CricketWorldCup that extends from HanaVectorEntity:

You need a SAP HANA Cloud vector engine account - Refer SAP HANA Cloud vector engine - provision a trial account guide to create a trial account.

If required, an API key for the EmbeddingModel to generate the embeddings stored by the vector store.

Spring AI does not provide a dedicated module for SAP Hana vector store. Users are expected to provide their own configuration in the applications using the standard vector store module for SAP Hana vector store in Spring AI - spring-ai-hanadb-store.

Please have a look at the list of HanaCloudVectorStore Properties for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

You can use the following properties in your Spring Boot configuration to customize the SAP Hana vector store. It uses spring.datasource. properties to configure the Hana datasource and the spring.ai.vectorstore.hanadb. properties to configure the Hana vector store.

spring.datasource.driver-class-name

com.sap.db.jdbc.Driver

spring.datasource.url

spring.datasource.username

Hana datasource username

spring.datasource.password

Hana datasource password

spring.ai.vectorstore.hanadb.top-k

spring.ai.vectorstore.hanadb.table-name

spring.ai.vectorstore.hanadb.initialize-schema

whether to initialize the required schema

Shows how to setup a project that uses SAP Hana Cloud as the vector DB and leverage OpenAI to implement RAG pattern

Create a table CRICKET_WORLD_CUP in SAP Hana DB:

Add the following dependencies in your pom.xml

You may set the property spring-ai-version as <spring-ai-version>1.0.0-SNAPSHOT</spring-ai-version>:

Add the following properties in application.properties file:

Create a Repository named CricketWorldCupRepository that implements HanaVectorRepository interface:

Now, create a REST Controller class CricketWorldCupHanaController, and autowire ChatModel and VectorStore as dependencies In this controller class, create the following REST endpoints:

/ai/hana-vector-store/cricket-world-cup/purge-embeddings - to purge all the embeddings from the Vector Store

/ai/hana-vector-store/cricket-world-cup/upload - to upload the Cricket_World_Cup.pdf so that its data gets stored in SAP Hana Cloud Vector DB as embeddings

/ai/hana-vector-store/cricket-world-cup - to implement RAG using Cosine_Similarity in SAP Hana DB

Since HanaDB vector store support does not provide the autoconfiguration module, you also need to provide the vector store bean in your application, as shown below, as an example.

Use a contextual pdf file from wikipedia

Go to wikipedia and download Cricket World Cup page as a PDF file.

Upload this PDF file using the file-upload REST endpoint that we created in the previous step.

**Examples:**

Example 1 (xml):
```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-bom</artifactId>
            <version>${spring-ai-version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-pdf-document-reader</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-hana</artifactId>
</dependency>

<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.30</version>
    <scope>provided</scope>
</dependency>
```

Example 2 (java):
```java
package com.interviewpedia.spring.ai.hana;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.Table;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.extern.jackson.Jacksonized;
import org.springframework.ai.vectorstore.hanadb.HanaVectorEntity;

@Entity
@Table(name = "CRICKET_WORLD_CUP")
@Data
@Jacksonized
@NoArgsConstructor
public class CricketWorldCup extends HanaVectorEntity {
    @Column(name = "content")
    private String content;
}
```

Example 3 (java):
```java
package com.interviewpedia.spring.ai.hana;

import jakarta.persistence.EntityManager;
import jakarta.persistence.PersistenceContext;
import jakarta.transaction.Transactional;
import org.springframework.ai.vectorstore.hanadb.HanaVectorRepository;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public class CricketWorldCupRepository implements HanaVectorRepository<CricketWorldCup> {
    @PersistenceContext
    private EntityManager entityManager;

    @Override
    @Transactional
    public void save(String tableName, String id, String embedding, String content) {
        String sql = String.format("""
                INSERT INTO %s (_ID, EMBEDDING, CONTENT)
                VALUES(:_id, TO_REAL_VECTOR(:embedding), :content)
                """, tableName);

		this.entityManager.createNativeQuery(sql)
                .setParameter("_id", id)
                .setParameter("embedding", embedding)
                .setParameter("content", content)
                .executeUpdate();
    }

    @Override
    @Transactional
    public int deleteEmbeddingsById(String tableName, List<String> idList) {
        String sql = String.format("""
                DELETE FROM %s WHERE _ID IN (:ids)
                """, tableName);

        return this.entityManager.createNativeQuery(sql)
                .setParameter("ids", idList)
                .executeUpdate();
    }

    @Override
    @Transactional
    public int deleteAllEmbeddings(String tableName) {
        String sql = String.format("""
                DELETE FROM %s
                """, tableName);

        return this.entityManager.createNativeQuery(sql).executeUpdate();
    }

    @Override
    public List<CricketWorldCup> cosineSimilaritySearch(String tableName, int topK, String queryEmbedding) {
        String sql = String.format("""
                SELECT TOP :topK * FROM %s
                ORDER BY COSINE_SIMILARITY(EMBEDDING, TO_REAL_VECTOR(:queryEmbedding)) DESC
                """, tableName);

        return this.entityManager.createNativeQuery(sql, CricketWorldCup.class)
                .setParameter("topK", topK)
                .setParameter("queryEmbedding", queryEmbedding)
                .getResultList();
    }
}
```

Example 4 (java):
```java
package com.interviewpedia.spring.ai.hana;

import lombok.extern.slf4j.Slf4j;
import org.springframework.ai.chat.model.ChatModel;
import org.springframework.ai.chat.messages.UserMessage;
import org.springframework.ai.chat.prompt.Prompt;
import org.springframework.ai.chat.prompt.SystemPromptTemplate;
import org.springframework.ai.document.Document;
import org.springframework.ai.reader.pdf.PagePdfDocumentReader;
import org.springframework.ai.transformer.splitter.TokenTextSplitter;
import org.springframework.ai.vectorstore.hanadb.HanaCloudVectorStore;
import org.springframework.ai.vectorstore.VectorStore;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.io.Resource;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;
import java.util.List;
import java.util.Map;
import java.util.function.Function;
import java.util.function.Supplier;
import java.util.stream.Collectors;

@RestController
@Slf4j
public class CricketWorldCupHanaController {
    private final VectorStore hanaCloudVectorStore;
    private final ChatModel chatModel;

    @Autowired
    public CricketWorldCupHanaController(ChatModel chatModel, VectorStore hanaCloudVectorStore) {
        this.chatModel = chatModel;
        this.hanaCloudVectorStore = hanaCloudVectorStore;
    }

    @PostMapping("/ai/hana-vector-store/cricket-world-cup/purge-embeddings")
    public ResponseEntity<String> purgeEmbeddings() {
        int deleteCount = ((HanaCloudVectorStore) this.hanaCloudVectorStore).purgeEmbeddings();
        log.info("{} embeddings purged from CRICKET_WORLD_CUP table in Hana DB", deleteCount);
        return ResponseEntity.ok().body(String.format("%d embeddings purged from CRICKET_WORLD_CUP table in Hana DB", deleteCount));
    }

    @PostMapping("/ai/hana-vector-store/cricket-world-cup/upload")
    public ResponseEntity<String> handleFileUpload(@RequestParam("pdf") MultipartFile file) throws IOException {
        Resource pdf = file.getResource();
        Supplier<List<Document>> reader = new PagePdfDocumentReader(pdf);
        Function<List<Document>, List<Document>> splitter = new TokenTextSplitter();
        List<Document> documents = splitter.apply(reader.get());
        log.info("{} documents created from pdf file: {}", documents.size(), pdf.getFilename());
		this.hanaCloudVectorStore.accept(documents);
        return ResponseEntity.ok().body(String.format("%d documents created from pdf file: %s",
                documents.size(), pdf.getFilename()));
    }

    @GetMapping("/ai/hana-vector-store/cricket-world-cup")
    public Map<String, String> hanaVectorStoreSearch(@RequestParam(value = "message") String message) {
        var documents = this.hanaCloudVectorStore.similaritySearch(message);
        var inlined = documents.stream().map(Document::getText).collect(Collectors.joining(System.lineSeparator()));
        var similarDocsMessage = new SystemPromptTemplate("Based on the following: {documents}")
                .createMessage(Map.of("documents", inlined));

        var userMessage = new UserMessage(message);
        Prompt prompt = new Prompt(List.of(similarDocsMessage, userMessage));
        String generation = this.chatModel.call(prompt).getResult().getOutput().getContent();
        log.info("Generation: {}", generation);
        return Map.of("generation", generation);
    }
}
```

---

## Docker Compose :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/docker-compose.html

**Contents:**
- Docker Compose
- Service Connections

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

## Chroma :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/chroma.html

**Contents:**
- Chroma
- Prerequisites
- Auto-configuration
  - Configuration properties
  - Chroma Cloud Configuration
- Metadata filtering
- Manual Configuration
  - Sample Code
  - Run Chroma Locally

For the latest snapshot version, please use Spring AI 1.1.2!

This section will walk you through setting up the Chroma VectorStore to store document embeddings and perform similarity searches.

Chroma is the open-source embedding database. It gives you the tools to store document embeddings, content, and metadata and to search through those embeddings, including metadata filtering.

Access to ChromaDB. Compatible with Chroma Cloud, or setup local ChromaDB in the appendix shows how to set up a DB locally with a Docker container.

For Chroma Cloud: You’ll need your API key, tenant name, and database name from your Chroma Cloud dashboard.

For local ChromaDB: No additional configuration required beyond starting the container.

EmbeddingModel instance to compute the document embeddings. Several options are available:

If required, an API key for the EmbeddingModel to generate the embeddings stored by the ChromaVectorStore.

On startup, the ChromaVectorStore creates the required collection if one is not provisioned already.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Chroma Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Here is an example of the needed bean:

To connect to Chroma you need to provide access details for your instance. A simple configuration can either be provided via Spring Boot’s application.properties,

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Now you can auto-wire the Chroma Vector Store in your application and use it

You can use the following properties in your Spring Boot configuration to customize the vector store.

spring.ai.vectorstore.chroma.client.host

Server connection host

spring.ai.vectorstore.chroma.client.port

Server connection port

spring.ai.vectorstore.chroma.client.key-token

Access token (if configured)

spring.ai.vectorstore.chroma.client.username

Access username (if configured)

spring.ai.vectorstore.chroma.client.password

Access password (if configured)

spring.ai.vectorstore.chroma.tenant-name

Tenant (required for Chroma Cloud)

spring.ai.vectorstore.chroma.database-name

Database name (required for Chroma Cloud)

spring.ai.vectorstore.chroma.collection-name

spring.ai.vectorstore.chroma.initialize-schema

Whether to initialize the required schema (creates tenant/database/collection if they don’t exist)

For ChromaDB secured with Static API Token Authentication use the ChromaApi#withKeyToken(<Your Token Credentials>) method to set your credentials. Check the ChromaWhereIT for an example.

For ChromaDB secured with Basic Authentication use the ChromaApi#withBasicAuth(<your user>, <your password>) method to set your credentials. Check the BasicAuthChromaWhereIT for an example.

For Chroma Cloud, you need to provide the tenant and database names from your Chroma Cloud instance. Here’s an example configuration:

For Chroma Cloud: - The host should be api.trychroma.com - The port should be 443 (HTTPS) - You must provide your API key via key-token - The tenant and database names must match your Chroma Cloud configuration - Set initialize-schema=true to automatically create the collection if it doesn’t exist (it won’t recreate existing tenant/database)

You can leverage the generic, portable metadata filters with ChromaVector store as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Chroma format

If you prefer to configure the Chroma Vector Store manually, you can do so by creating a ChromaVectorStore bean in your Spring Boot application.

Add these dependencies to your project: * Chroma VectorStore.

OpenAI: Required for calculating embeddings. You can use any other embedding model implementation.

Create a RestClient.Builder instance with proper ChromaDB authorization configurations and Use it to create a ChromaApi instance:

Integrate with OpenAI’s embeddings by adding the Spring Boot OpenAI starter to your project. This provides you with an implementation of the Embeddings client:

In your main code, create some documents:

Add the documents to your vector store:

And finally, retrieve documents similar to a query:

If all goes well, you should retrieve the document containing the text "Spring AI rocks!!".

Starts a chroma store at localhost:8000/api/v1

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-chroma</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-chroma'
}
```

Example 3 (java):
```java
@Bean
public EmbeddingModel embeddingModel() {
    // Can be any other EmbeddingModel implementation.
    return new OpenAiEmbeddingModel(OpenAiApi.builder().apiKey(System.getenv("OPENAI_API_KEY")).build());
}
```

Example 4 (jsx):
```jsx
# Chroma Vector Store connection properties
spring.ai.vectorstore.chroma.client.host=<your Chroma instance host>  // for Chroma Cloud: api.trychroma.com
spring.ai.vectorstore.chroma.client.port=<your Chroma instance port> // for Chroma Cloud: 443
spring.ai.vectorstore.chroma.client.key-token=<your access token (if configure)> // for Chroma Cloud: use the API key
spring.ai.vectorstore.chroma.client.username=<your username (if configure)>
spring.ai.vectorstore.chroma.client.password=<your password (if configure)>

# Chroma Vector Store tenant and database properties (required for Chroma Cloud)
spring.ai.vectorstore.chroma.tenant-name=<your tenant name> // default: SpringAiTenant
spring.ai.vectorstore.chroma.database-name=<your database name> // default: SpringAiDatabase

# Chroma Vector Store collection properties
spring.ai.vectorstore.chroma.initialize-schema=<true or false>
spring.ai.vectorstore.chroma.collection-name=<your collection name>

# Chroma Vector Store configuration properties

# OpenAI API key if the OpenAI auto-configuration is used.
spring.ai.openai.api.key=<OpenAI Api-key>
```

---

## Chroma :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/chroma.html

**Contents:**
- Chroma
- Prerequisites
- Auto-configuration
  - Configuration properties
  - Chroma Cloud Configuration
- Metadata filtering
- Manual Configuration
  - Sample Code
  - Run Chroma Locally

This section will walk you through setting up the Chroma VectorStore to store document embeddings and perform similarity searches.

Chroma is the open-source embedding database. It gives you the tools to store document embeddings, content, and metadata and to search through those embeddings, including metadata filtering.

Access to ChromaDB. Compatible with Chroma Cloud, or setup local ChromaDB in the appendix shows how to set up a DB locally with a Docker container.

For Chroma Cloud: You’ll need your API key, tenant name, and database name from your Chroma Cloud dashboard.

For local ChromaDB: No additional configuration required beyond starting the container.

EmbeddingModel instance to compute the document embeddings. Several options are available:

If required, an API key for the EmbeddingModel to generate the embeddings stored by the ChromaVectorStore.

On startup, the ChromaVectorStore creates the required collection if one is not provisioned already.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Chroma Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the requisite schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Here is an example of the needed bean:

To connect to Chroma you need to provide access details for your instance. A simple configuration can either be provided via Spring Boot’s application.properties,

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Now you can auto-wire the Chroma Vector Store in your application and use it

You can use the following properties in your Spring Boot configuration to customize the vector store.

spring.ai.vectorstore.chroma.client.host

Server connection host

spring.ai.vectorstore.chroma.client.port

Server connection port

spring.ai.vectorstore.chroma.client.key-token

Access token (if configured)

spring.ai.vectorstore.chroma.client.username

Access username (if configured)

spring.ai.vectorstore.chroma.client.password

Access password (if configured)

spring.ai.vectorstore.chroma.tenant-name

Tenant (required for Chroma Cloud)

spring.ai.vectorstore.chroma.database-name

Database name (required for Chroma Cloud)

spring.ai.vectorstore.chroma.collection-name

spring.ai.vectorstore.chroma.initialize-schema

Whether to initialize the required schema (creates tenant/database/collection if they don’t exist)

For ChromaDB secured with Static API Token Authentication use the ChromaApi#withKeyToken(<Your Token Credentials>) method to set your credentials. Check the ChromaWhereIT for an example.

For ChromaDB secured with Basic Authentication use the ChromaApi#withBasicAuth(<your user>, <your password>) method to set your credentials. Check the BasicAuthChromaWhereIT for an example.

For Chroma Cloud, you need to provide the tenant and database names from your Chroma Cloud instance. Here’s an example configuration:

For Chroma Cloud: - The host should be api.trychroma.com - The port should be 443 (HTTPS) - You must provide your API key via key-token - The tenant and database names must match your Chroma Cloud configuration - Set initialize-schema=true to automatically create the collection if it doesn’t exist (it won’t recreate existing tenant/database)

You can leverage the generic, portable metadata filters with ChromaVector store as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary Chroma format

If you prefer to configure the Chroma Vector Store manually, you can do so by creating a ChromaVectorStore bean in your Spring Boot application.

Add these dependencies to your project: * Chroma VectorStore.

OpenAI: Required for calculating embeddings. You can use any other embedding model implementation.

Create a RestClient.Builder instance with proper ChromaDB authorization configurations and Use it to create a ChromaApi instance:

Integrate with OpenAI’s embeddings by adding the Spring Boot OpenAI starter to your project. This provides you with an implementation of the Embeddings client:

In your main code, create some documents:

Add the documents to your vector store:

And finally, retrieve documents similar to a query:

If all goes well, you should retrieve the document containing the text "Spring AI rocks!!".

Starts a chroma store at localhost:8000/api/v1

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-chroma</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-chroma'
}
```

Example 3 (java):
```java
@Bean
public EmbeddingModel embeddingModel() {
    // Can be any other EmbeddingModel implementation.
    return new OpenAiEmbeddingModel(OpenAiApi.builder().apiKey(System.getenv("OPENAI_API_KEY")).build());
}
```

Example 4 (jsx):
```jsx
# Chroma Vector Store connection properties
spring.ai.vectorstore.chroma.client.host=<your Chroma instance host>  // for Chroma Cloud: api.trychroma.com
spring.ai.vectorstore.chroma.client.port=<your Chroma instance port> // for Chroma Cloud: 443
spring.ai.vectorstore.chroma.client.key-token=<your access token (if configure)> // for Chroma Cloud: use the API key
spring.ai.vectorstore.chroma.client.username=<your username (if configure)>
spring.ai.vectorstore.chroma.client.password=<your password (if configure)>

# Chroma Vector Store tenant and database properties (required for Chroma Cloud)
spring.ai.vectorstore.chroma.tenant-name=<your tenant name> // default: SpringAiTenant
spring.ai.vectorstore.chroma.database-name=<your database name> // default: SpringAiDatabase

# Chroma Vector Store collection properties
spring.ai.vectorstore.chroma.initialize-schema=<true or false>
spring.ai.vectorstore.chroma.collection-name=<your collection name>

# Chroma Vector Store configuration properties

# OpenAI API key if the OpenAI auto-configuration is used.
spring.ai.openai.api.key=<OpenAI Api-key>
```

---

## Azure AI Service :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/azure.html

**Contents:**
- Azure AI Service
- Prerequisites
- Configuration
- Dependencies
  - 1. Select an Embeddings interface implementation. You can choose between:
  - 2. Azure (AI Search) Vector Store
- Configuration Properties
- Sample Code
  - Metadata filtering
- Custom Field Names

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section will walk you through setting up the AzureVectorStore to store document embeddings and perform similarity searches using the Azure AI Search Service.

Azure AI Search is a versatile cloud-hosted cloud information retrieval system that is part of Microsoft’s larger AI platform. Among other features, it allows users to query information using vector-based storage and retrieval.

Azure Subscription: You will need an Azure subscription to use any Azure service.

Azure AI Search Service: Create an AI Search service. Once the service is created, obtain the admin apiKey from the Keys section under Settings and retrieve the endpoint from the Url field under the Overview section.

(Optional) Azure OpenAI Service: Create an Azure OpenAI service. NOTE: You may have to fill out a separate form to gain access to Azure Open AI services. Once the service is created, obtain the endpoint and apiKey from the Keys and Endpoint section under Resource Management.

On startup, the AzureVectorStore can attempt to create a new index within your AI Search service instance if you’ve opted in by setting the relevant initialize-schema boolean property to true in the constructor or, if using Spring Boot, setting …​initialize-schema=true in your application.properties file.

Alternatively, you can create the index manually.

To set up an AzureVectorStore, you will need the settings retrieved from the prerequisites above along with your index name:

Azure AI Search Endpoint

(optional) Azure OpenAI API Endpoint

(optional) Azure OpenAI API Key

You can provide these values as OS environment variables.

You can replace Azure Open AI implementation with any valid OpenAI implementation that supports the Embeddings interface. For example, you could use Spring AI’s Open AI or TransformersEmbedding implementations for embeddings instead of the Azure implementation.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add these dependencies to your project:

Local Sentence Transformers Embedding

You can use the following properties in your Spring Boot configuration to customize the Azure vector store.

spring.ai.vectorstore.azure.url

spring.ai.vectorstore.azure.api-key

spring.ai.vectorstore.azure.useKeylessAuth

spring.ai.vectorstore.azure.initialize-schema

spring.ai.vectorstore.azure.index-name

spring_ai_azure_vector_store

spring.ai.vectorstore.azure.default-top-k

spring.ai.vectorstore.azure.default-similarity-threshold

spring.ai.vectorstore.azure.content-field-name

spring.ai.vectorstore.azure.embedding-field-name

spring.ai.vectorstore.azure.metadata-field-name

To configure an Azure SearchIndexClient in your application, you can use the following code:

To create a vector store, you can use the following code by injecting the SearchIndexClient bean created in the above sample along with an EmbeddingModel provided by the Spring AI library that implements the desired Embeddings interface.

You must list explicitly all metadata field names and types for any metadata key used in the filter expression. The list above registers filterable metadata fields: country of type TEXT, year of type INT64, and active of type BOOLEAN.

If the filterable metadata fields are expanded with new entries, you have to (re)upload/update the documents with this metadata.

In your main code, create some documents:

Add the documents to your vector store:

And finally, retrieve documents similar to a query:

If all goes well, you should retrieve the document containing the text "Spring AI rocks!!".

You can leverage the generic, portable metadata filters with AzureVectorStore as well.

For example, you can use either the text expression language:

or programmatically using the expression DSL:

The portable filter expressions get automatically converted into the proprietary Azure Search OData filters. For example, the following portable filter expression:

is converted into the following Azure OData filter expression:

By default, the Azure Vector Store uses the following field names in the Azure AI Search index:

content - for document text

embedding - for vector embeddings

metadata - for document metadata

However, when working with existing Azure AI Search indexes that use different field names, you can configure custom field names to match your index schema. This allows you to integrate Spring AI with pre-existing indexes without needing to modify them.

Custom field names are particularly useful when:

Integrating with existing indexes: Your organization already has Azure AI Search indexes with established field naming conventions (e.g., chunk_text, vector, meta_data).

Following naming standards: Your team follows specific naming conventions that differ from the defaults.

Migrating from other systems: You’re migrating from another vector database or search system and want to maintain consistent field names.

You can configure custom field names using Spring Boot application properties:

Alternatively, you can configure custom field names programmatically using the builder API:

Here’s a complete example showing how to use Spring AI with an existing Azure AI Search index that has custom field names:

You can then use the vector store as normal:

You can also create a new index with custom field names by setting initializeSchema=true:

This will create a new Azure AI Search index with your custom field names, allowing you to establish your own naming conventions from the start.

The Azure Vector Store implementation provides access to the underlying native Azure Search client (SearchClient) through the getNativeClient() method:

The native client gives you access to Azure Search-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (bash):
```bash
export AZURE_AI_SEARCH_API_KEY=<My AI Search API Key>
export AZURE_AI_SEARCH_ENDPOINT=<My AI Search Index>
export OPENAI_API_KEY=<My Azure AI API Key> (Optional)
```

Example 2 (xml):
```xml
<dependency>
   <groupId>org.springframework.ai</groupId>
   <artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

Example 3 (xml):
```xml
<dependency>
 <groupId>org.springframework.ai</groupId>
 <artifactId>spring-ai-starter-model-azure-openai</artifactId>
</dependency>
```

Example 4 (xml):
```xml
<dependency>
 <groupId>org.springframework.ai</groupId>
 <artifactId>spring-ai-starter-model-transformers</artifactId>
</dependency>
```

---

## Pinecone :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/vectordbs/pinecone.html

**Contents:**
- Pinecone
- Prerequisites
- Auto-configuration
  - Configuration properties
- Metadata filtering
- Manual Configuration
  - Sample Code
- Accessing the Native Client

For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up the Pinecone VectorStore to store document embeddings and perform similarity searches.

Pinecone is a popular cloud-based vector database, which allows you to store and search vectors efficiently.

Pinecone Account: Before you start, sign up for a Pinecone account.

Pinecone Project: Once registered, generate an API key and create and index. You’ll need these details for configuration.

EmbeddingModel instance to compute the document embeddings. Several options are available:

If required, an API key for the EmbeddingModel to generate the embeddings stored by the PineconeVectorStore.

To set up PineconeVectorStore, gather the following details from your Pinecone account:

This information is available to you in the Pinecone UI portal. The namespace support is not available in the Pinecone free tier.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Pinecone Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Here is an example of the needed bean:

To connect to Pinecone you need to provide access details for your instance. A simple configuration can either be provided via Spring Boot’s application.properties,

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Now you can Auto-wire the Pinecone Vector Store in your application and use it

You can use the following properties in your Spring Boot configuration to customize the Pinecone vector store.

spring.ai.vectorstore.pinecone.api-key

spring.ai.vectorstore.pinecone.index-name

spring.ai.vectorstore.pinecone.namespace

spring.ai.vectorstore.pinecone.content-field-name

Pinecone metadata field name used to store the original text content.

spring.ai.vectorstore.pinecone.distance-metadata-field-name

Pinecone metadata field name used to store the computed distance.

spring.ai.vectorstore.pinecone.server-side-timeout

You can leverage the generic, portable metadata filters with the Pinecone store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

If you prefer to configure PineconeVectorStore manually, you can do so by using the PineconeVectorStore#Builder.

Add these dependencies to your project:

OpenAI: Required for calculating embeddings.

To configure Pinecone in your application, you can use the following setup:

In your main code, create some documents:

Add the documents to Pinecone:

And finally, retrieve documents similar to a query:

If all goes well, you should retrieve the document containing the text "Spring AI rocks!!".

The Pinecone Vector Store implementation provides access to the underlying native Pinecone client (PineconeConnection) through the getNativeClient() method:

The native client gives you access to Pinecone-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-pinecone</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-pinecone'
}
```

Example 3 (java):
```java
@Bean
public EmbeddingModel embeddingModel() {
    // Can be any other EmbeddingModel implementation.
    return new OpenAiEmbeddingModel(new OpenAiApi(System.getenv("OPENAI_API_KEY")));
}
```

Example 4 (jsx):
```jsx
spring.ai.vectorstore.pinecone.apiKey=<your api key>
spring.ai.vectorstore.pinecone.index-name=<your index name>

# API key if needed, e.g. OpenAI
spring.ai.openai.api.key=<api-key>
```

---

## Apache Cassandra Vector Store :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/vectordbs/apache-cassandra.html

**Contents:**
- Apache Cassandra Vector Store
- What is Apache Cassandra?
- What is JVector?
- Prerequisites
- Dependencies
- Configuration Properties
- Usage
  - Basic Usage
  - Advanced Configuration
  - Connection Configuration

This section walks you through setting up CassandraVectorStore to store document embeddings and perform similarity searches.

Apache Cassandra® is a true open source distributed database renowned for linear scalability, proven fault-tolerance and low latency, making it the perfect platform for mission-critical transactional data.

Its Vector Similarity Search (VSS) is based on the JVector library that ensures best-in-class performance and relevancy.

A vector search in Apache Cassandra is done as simply as:

More docs on this can be read here.

This Spring AI Vector Store is designed to work for both brand-new RAG applications and be able to be retrofitted on top of existing data and tables.

The store can also be used for non-RAG use-cases in an existing database, e.g. semantic searches, geo-proximity searches, etc.

The store will automatically create, or enhance, the schema as needed according to its configuration. If you don’t want the schema modifications, configure the store with initializeSchema.

When using spring-boot-autoconfigure initializeSchema defaults to false, per Spring Boot standards, and you must opt-in to schema creation/modifications by setting …​initialize-schema=true in the application.properties file.

JVector is a pure Java embedded vector search engine.

It stands out from other HNSW Vector Similarity Search implementations by being:

Algorithmic-fast. JVector uses state of the art graph algorithms inspired by DiskANN and related research that offer high recall and low latency.

Implementation-fast. JVector uses the Panama SIMD API to accelerate index build and queries.

Memory efficient. JVector compresses vectors using product quantization so they can stay in memory during searches.

Disk-aware. JVector’s disk layout is designed to do the minimum necessary iops at query time.

Concurrent. Index builds scale linearly to at least 32 threads. Double the threads, half the build time.

Incremental. Query your index as you build it. No delay between adding a vector and being able to find it in search results.

Easy to embed. API designed for easy embedding, by people using it in production.

A EmbeddingModel instance to compute the document embeddings. This is usually configured as a Spring Bean. Several options are available:

Transformers Embedding - computes the embedding in your local environment. The default is via ONNX and the all-MiniLM-L6-v2 Sentence Transformers. This just works.

If you want to use OpenAI’s Embeddings - uses the OpenAI embedding endpoint. You need to create an account at OpenAI Signup and generate the api-key token at API Keys.

There are many more choices, see Embeddings API docs.

An Apache Cassandra instance, from version 5.0-beta1

For a managed offering Astra DB offers a healthy free tier offering.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add these dependencies to your project:

For just the Cassandra Vector Store:

Or, for everything you need in a RAG application (using the default ONNX Embedding Model):

You can use the following properties in your Spring Boot configuration to customize the Apache Cassandra vector store.

spring.ai.vectorstore.cassandra.keyspace

spring.ai.vectorstore.cassandra.table

spring.ai.vectorstore.cassandra.initialize-schema

spring.ai.vectorstore.cassandra.index-name

spring.ai.vectorstore.cassandra.content-column-name

spring.ai.vectorstore.cassandra.embedding-column-name

spring.ai.vectorstore.cassandra.fixed-thread-pool-executor-size

Create a CassandraVectorStore instance as a Spring Bean:

Once you have the vector store instance, you can add documents and perform searches:

For more complex use cases, you can configure additional settings in your Spring Bean:

There are two ways to configure the connection to Cassandra:

Using an injected CqlSession (recommended):

Using connection details directly in the builder:

You can leverage the generic, portable metadata filters with the CassandraVectorStore. For metadata columns to be searchable they must be either primary keys or SAI indexed. To make non-primary-key columns indexed, configure the metadata column with the SchemaColumnTags.INDEXED.

For example, you can use either the text expression language:

or programmatically using the expression DSL:

The portable filter expressions get automatically converted into CQL queries.

The following example demonstrates how to use the store on an existing schema. Here we use the schema from the github.com/datastax-labs/colbert-wikipedia-data project which comes with the full wikipedia dataset ready vectorized for you.

First, create the schema in the Cassandra database:

Then configure the store using the builder pattern:

To load the full wikipedia dataset:

Download simplewiki-sstable.tar from s.apache.org/simplewiki-sstable-tar (this will take a while, the file is tens of GBs)

If you have existing data in this table, check the tarball’s files don’t clobber existing sstables when doing the tar.

An alternative to nodetool import is to just restart Cassandra.

If there are any failures in the indexes they will be rebuilt automatically.

The Cassandra Vector Store implementation provides access to the underlying native Cassandra client (CqlSession) through the getNativeClient() method:

The native client gives you access to Cassandra-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (sql):
```sql
SELECT content FROM table ORDER BY content_vector ANN OF query_embedding;
```

Example 2 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-cassandra-store</artifactId>
</dependency>
```

Example 3 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-cassandra</artifactId>
</dependency>
```

Example 4 (java):
```java
@Bean
public VectorStore vectorStore(CqlSession session, EmbeddingModel embeddingModel) {
    return CassandraVectorStore.builder(embeddingModel)
        .session(session)
        .keyspace("my_keyspace")
        .table("my_vectors")
        .build();
}
```

---

## GemFire Vector Store :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/gemfire.html

**Contents:**
- GemFire Vector Store
- Prerequisites
- Auto-configuration
  - Configuration properties
- Manual Configuration
- Usage
- Metadata Filtering

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up the GemFireVectorStore to store document embeddings and perform similarity searches.

GemFire is a distributed, in-memory, key-value store performing read and write operations at blazingly fast speeds. It offers highly available parallel message queues, continuous availability, and an event-driven architecture you can scale dynamically without downtime. As your data size requirements increase to support high-performance, real-time apps, GemFire can easily scale linearly.

GemFire VectorDB extends GemFire’s capabilities, serving as a versatile vector database that efficiently stores, retrieves, and performs vector similarity searches.

A GemFire cluster with the GemFire VectorDB extension enabled

Install GemFire VectorDB extension

An EmbeddingModel bean to compute the document embeddings. Refer to the EmbeddingModel section for more information. An option that runs locally on your machine is ONNX and the all-MiniLM-L6-v2 Sentence Transformers.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Add the GemFire VectorStore Spring Boot starter to you project’s Maven build file pom.xml:

or to your Gradle build.gradle file

You can use the following properties in your Spring Boot configuration to further configure the GemFireVectorStore.

spring.ai.vectorstore.gemfire.host

spring.ai.vectorstore.gemfire.port

spring.ai.vectorstore.gemfire.initialize-schema

spring.ai.vectorstore.gemfire.index-name

spring-ai-gemfire-store

spring.ai.vectorstore.gemfire.beam-width

spring.ai.vectorstore.gemfire.max-connections

spring.ai.vectorstore.gemfire.vector-similarity-function

spring.ai.vectorstore.gemfire.fields

spring.ai.vectorstore.gemfire.buckets

spring.ai.vectorstore.gemfire.username

spring.ai.vectorstore.gemfire.password

spring.ai.vectorstore.gemfire.token

To use just the GemFireVectorStore, without Spring Boot’s Auto-configuration add the following dependency to your project’s Maven pom.xml:

For Gradle users, add the following to your build.gradle file under the dependencies block to use just the GemFireVectorStore:

Here is a sample that creates an instance of the GemfireVectorStore instead of using AutoConfiguration

The default configuration connects to a GemFire cluster at localhost:8080

In your application, create a few documents:

Add the documents to the vector store:

And to retrieve documents using similarity search:

You should retrieve the document containing the text "Spring AI rocks!!".

You can also limit the number of results using a similarity threshold:

You can leverage the generic, portable metadata filters with GemFire VectorStore as well.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

For example, this portable filter expression:

is converted into the proprietary GemFire VectorDB filter format:

The GemFire VectorStore supports a wide range of filter operations:

Equality: country == 'BG' → country:BG

Inequality: city != 'Sofia' → city: NOT Sofia

Greater Than: year > 2020 → year:{2020 TO *]

Greater Than or Equal: year >= 2020 → year:[2020 TO *]

Less Than: year < 2025 → year:[* TO 2025}

Less Than or Equal: year ⇐ 2025 → year:[* TO 2025]

IN: country in ['BG', 'NL'] → country:(BG OR NL)

NOT IN: country nin ['BG', 'NL'] → NOT country:(BG OR NL)

AND/OR: Logical operators for combining conditions

Grouping: Use parentheses for complex expressions

Date Filtering: Date values in ISO 8601 format (e.g., 2024-01-07T14:29:12Z)

To use metadata filtering with GemFire VectorStore, you must specify the metadata fields that can be filtered when creating the vector store. This is done using the fields parameter in the builder:

Or via configuration properties:

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-gemfire</artifactId>
</dependency>
```

Example 2 (xml):
```xml
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-gemfire'
}
```

Example 3 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-gemfire-store</artifactId>
</dependency>
```

Example 4 (java):
```java
@Bean
public GemFireVectorStore vectorStore(EmbeddingModel embeddingModel) {
    return GemFireVectorStore.builder(embeddingModel)
        .host("localhost")
        .port(7071)
        .username("my-user-name")
        .password("my-password")
        .indexName("my-vector-index")
        .fields(new String[] {"country", "year", "activationDate"}) // Optional: fields for metadata filtering
        .initializeSchema(true)
        .build();
}
```

---

## PGvector :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/pgvector.html

**Contents:**
- PGvector
- Prerequisites
- Auto-Configuration
  - Configuration properties
- Metadata filtering
- Manual Configuration
- Run Postgres & PGVector DB locally
- Accessing the Native Client

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section walks you through setting up the PGvector VectorStore to store document embeddings and perform similarity searches.

PGvector is an open-source extension for PostgreSQL that enables storing and searching over machine learning-generated embeddings. It provides different capabilities that let users identify both exact and approximate nearest neighbors. It is designed to work seamlessly with other PostgreSQL features, including indexing and querying.

First you need access to PostgreSQL instance with enabled vector, hstore and uuid-ossp extensions.

On startup with the schema initialization feature explicitly enabled, the PgVectorStore will attempt to install the required database extensions and create the required vector_store table with an index if not existing.

Optionally, you can do this manually like so:

Next, if required, an API key for the EmbeddingModel to generate the embeddings stored by the PgVectorStore.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Then add the PgVectorStore boot starter dependency to your project:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the required schema for you, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor or by setting …​initialize-schema=true in the application.properties file.

The Vector Store also requires an EmbeddingModel instance to calculate embeddings for the documents. You can pick one of the available EmbeddingModel Implementations.

For example, to use the OpenAI EmbeddingModel, add the following dependency to your project:

or to your Gradle build.gradle build file.

To connect to and configure the PgVectorStore, you need to provide access details for your instance. A simple configuration can be provided via Spring Boot’s application.yml.

Now you can auto-wire the VectorStore in your application and use it

You can use the following properties in your Spring Boot configuration to customize the PGVector vector store.

spring.ai.vectorstore.pgvector.index-type

Nearest neighbor search index type. Options are NONE - exact nearest neighbor search, IVFFlat - index divides vectors into lists, and then searches a subset of those lists that are closest to the query vector. It has faster build times and uses less memory than HNSW, but has lower query performance (in terms of speed-recall tradeoff). HNSW - creates a multilayer graph. It has slower build times and uses more memory than IVFFlat, but has better query performance (in terms of speed-recall tradeoff). There’s no training step like IVFFlat, so the index can be created without any data in the table.

spring.ai.vectorstore.pgvector.distance-type

Search distance type. Defaults to COSINE_DISTANCE. But if vectors are normalized to length 1, you can use EUCLIDEAN_DISTANCE or NEGATIVE_INNER_PRODUCT for best performance.

spring.ai.vectorstore.pgvector.dimensions

Embeddings dimension. If not specified explicitly the PgVectorStore will retrieve the dimensions form the provided EmbeddingModel. Dimensions are set to the embedding column the on table creation. If you change the dimensions your would have to re-create the vector_store table as well.

spring.ai.vectorstore.pgvector.remove-existing-vector-store-table

Deletes the existing vector_store table on start up.

spring.ai.vectorstore.pgvector.initialize-schema

Whether to initialize the required schema

spring.ai.vectorstore.pgvector.schema-name

Vector store schema name

spring.ai.vectorstore.pgvector.table-name

Vector store table name

spring.ai.vectorstore.pgvector.schema-validation

Enables schema and table name validation to ensure they are valid and existing objects.

spring.ai.vectorstore.pgvector.max-document-batch-size

Maximum number of documents to process in a single batch.

You can leverage the generic, portable metadata filters with the PgVector store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

Instead of using the Spring Boot auto-configuration, you can manually configure the PgVectorStore. For this you need to add the PostgreSQL connection and JdbcTemplate auto-configuration dependencies to your project:

To configure PgVector in your application, you can use the following setup:

You can connect to this server like this:

The PGVector Store implementation provides access to the underlying native JDBC client (JdbcTemplate) through the getNativeClient() method:

The native client gives you access to PostgreSQL-specific features and operations that might not be exposed through the VectorStore interface.

**Examples:**

Example 1 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-vector-store-pgvector</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-vector-store-pgvector'
}
```

Example 3 (xml):
```xml
<dependency>
	<groupId>org.springframework.ai</groupId>
	<artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

Example 4 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-openai'
}
```

---

## Couchbase :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/vectordbs/couchbase.html

**Contents:**
- Couchbase
- Prerequisites
- Auto-configuration
  - Configuration Properties
    - Option 1: Using Spring Expression Language (SpEL)
    - Option 2: Accessing Environment Variables Programmatically
- Metadata Filtering
- Manual Configuration
- Limitations

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

This section will walk you through setting up the CouchbaseSearchVectorStore to store document embeddings and perform similarity searches using Couchbase.

Couchbase is a distributed, JSON document database, with all the desired capabilities of a relational DBMS. Among other features, it allows users to query information using vector-based storage and retrieval.

A running Couchbase instance. The following options are available: Couchbase * Docker * Capella - Couchbase as a Service * Install Couchbase locally * Couchbase Kubernetes Operator

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Couchbase Vector Store. To enable it, add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file.

The vector store implementation can initialize the configured bucket, scope, collection and search index for you, with default options, but you must opt-in by specifying the initializeSchema boolean in the appropriate constructor.

Please have a look at the list of configuration parameters for the vector store to learn about the default values and configuration options.

Additionally, you will need a configured EmbeddingModel bean. Refer to the EmbeddingModel section for more information.

Now you can auto-wire the CouchbaseSearchVectorStore as a vector store in your application.

To connect to Couchbase and use the CouchbaseSearchVectorStore, you need to provide access details for your instance. Configuration can be provided via Spring Boot’s application.properties:

If you prefer to use environment variables for sensitive information like passwords or API keys, you have multiple options:

You can use custom environment variable names and reference them in your application configuration using SpEL:

Alternatively, you can access environment variables in your Java code:

This approach gives you flexibility in naming your environment variables while keeping sensitive information out of your application configuration files.

Spring Boot’s auto-configuration feature for the Couchbase Cluster will create a bean instance that will be used by the CouchbaseSearchVectorStore.

The Spring Boot properties starting with spring.couchbase.* are used to configure the Couchbase cluster instance:

spring.couchbase.connection-string

A couchbase connection string

couchbase://localhost

spring.couchbase.password

Password for authentication with Couchbase.

spring.couchbase.username

Username for authentication with Couchbase.

spring.couchbase.env.io.minEndpoints

Minimum number of sockets per node.

spring.couchbase.env.io.maxEndpoints

Maximum number of sockets per node.

spring.couchbase.env.io.idleHttpConnectionTimeout

Length of time an HTTP connection may remain idle before it is closed and removed from the pool.

spring.couchbase.env.ssl.enabled

Whether to enable SSL support. Enabled automatically if a "bundle" is provided unless specified otherwise.

spring.couchbase.env.ssl.bundle

spring.couchbase.env.timeouts.connect

Bucket connect timeout.

spring.couchbase.env.timeouts.disconnect

Bucket disconnect timeout.

spring.couchbase.env.timeouts.key-value

Timeout for operations on a specific key-value.

spring.couchbase.env.timeouts.key-value

Timeout for operations on a specific key-value with a durability level.

spring.couchbase.env.timeouts.key-value-durable

Timeout for operations on a specific key-value with a durability level.

spring.couchbase.env.timeouts.query

SQL++ query operations timeout.

spring.couchbase.env.timeouts.view

Regular and geospatial view operations timeout.

spring.couchbase.env.timeouts.search

Timeout for the search service.

spring.couchbase.env.timeouts.analytics

Timeout for the analytics service.

spring.couchbase.env.timeouts.management

Timeout for the management operations.

Properties starting with the spring.ai.vectorstore.couchbase.* prefix are used to configure CouchbaseSearchVectorStore.

spring.ai.vectorstore.couchbase.index-name

The name of the index to store the vectors.

spring-ai-document-index

spring.ai.vectorstore.couchbase.bucket-name

The name of the Couchbase Bucket, parent of the scope.

spring.ai.vectorstore.couchbase.scope-name

The name of the Couchbase scope, parent of the collection. Search queries will be executed in the scope context.

spring.ai.vectorstore.couchbase.collection-name

The name of the Couchbase collection to store the Documents.

spring.ai.vectorstore.couchbase.dimensions

The number of dimensions in the vector.

spring.ai.vectorstore.couchbase.similarity

The similarity function to use.

spring.ai.vectorstore.couchbase.optimization

The similarity function to use.

spring.ai.vectorstore.couchbase.initialize-schema

whether to initialize the required schema

The following similarity functions are available:

The following index optimizations are available:

More details about each in the Couchbase Documentation on vector searches.

You can leverage the generic, portable metadata filters with the Couchbase store.

For example, you can use either the text expression language:

or programmatically using the Filter.Expression DSL:

Instead of using the Spring Boot auto-configuration, you can manually configure the Couchbase vector store. For this you need to add the spring-ai-couchbase-store to your project:

or to your Gradle build.gradle build file.

Create a Couchbase Cluster bean. Read the Couchbase Documentation for more in-depth information about the configuration of a custom Cluster instance.

and then create the CouchbaseSearchVectorStore bean using the builder pattern:

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-vector-store-couchbase</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-couchbase-store-spring-boot-starter'
}
```

Example 3 (java):
```java
@Autowired VectorStore vectorStore;

// ...

List <Document> documents = List.of(
    new Document("Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!! Spring AI rocks!!", Map.of("meta1", "meta1")),
    new Document("The World is Big and Salvation Lurks Around the Corner"),
    new Document("You walk forward facing the past and you turn back toward the future.", Map.of("meta2", "meta2")));

// Add the documents to Qdrant
vectorStore.add(documents);

// Retrieve documents similar to a query
List<Document> results = vectorStore.similaritySearch(SearchRequest.query("Spring").withTopK(5));
```

Example 4 (typescript):
```typescript
spring.ai.openai.api-key=<key>
spring.couchbase.connection-string=<conn_string>
spring.couchbase.username=<username>
spring.couchbase.password=<password>
```

---
