# Springai - Moderation

**Pages:** 3

---

## Moderation :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/moderation/mistral-ai-moderation.html

**Contents:**
- Moderation
- Introduction
- Prerequisites
- Auto-configuration
- Moderation Properties
  - Connection Properties
  - Configuration Properties
- Runtime Options
- Manual Configuration
- Example Code

For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI supports the new moderation service introduced by Mistral AI and powered by the Mistral Moderation model. It enables the detection of harmful text content along several policy dimensions. Follow this link for more information on the Mistral AI moderation model.

Create an Mistral AI account and obtain an API key. You can sign up at Mistral AI registration page and generate an API key on the API Keys page.

Add the spring-ai-mistral-ai dependency to your project’s build file. For more information, refer to the Dependency Management section.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Mistral AI Moderation Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

The prefix spring.ai.mistralai is used as the property prefix that lets you connect to Mistral AI.

spring.ai.mistralai.base-url

The URL to connect to

spring.ai.mistralai.api-key

Enabling and disabling of the moderation auto-configurations are now configured via top level properties with the prefix spring.ai.model.moderation.

To enable, spring.ai.model.moderation=mistral (It is enabled by default)

To disable, spring.ai.model.moderation=none (or any value which doesn’t match mistral)

This change is done to allow configuration of multiple models.

The prefix spring.ai.mistralai.moderation is used as the property prefix for configuring the Mistral AI moderation model.

spring.ai.model.moderation

Enable Moderation model

spring.ai.mistralai.moderation.base-url

The URL to connect to

spring.ai.mistralai.moderation.api-key

spring.ai.mistralai.moderation.options.model

ID of the model to use for moderation.

mistral-moderation-latest

The MistralAiModerationOptions class provides the options to use when making a moderation request. On start-up, the options specified by spring.ai.mistralai.moderation are used, but you can override these at runtime.

Add the spring-ai-mistral-ai dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

Next, create an MistralAiModerationModel:

The MistralAiModerationModelIT test provides some general examples of how to use the library. You can refer to this test for more detailed usage examples.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-mistral-ai</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-mistral-ai'
}
```

Example 3 (java):
```java
MistralAiModerationOptions moderationOptions = MistralAiModerationOptions.builder()
    .model("mistral-moderation-latest")
    .build();

ModerationPrompt moderationPrompt = new ModerationPrompt("Text to be moderated", this.moderationOptions);
ModerationResponse response = mistralAiModerationModel.call(this.moderationPrompt);

// Access the moderation results
Moderation moderation = moderationResponse.getResult().getOutput();

// Print general information
System.out.println("Moderation ID: " + moderation.getId());
System.out.println("Model used: " + moderation.getModel());

// Access the moderation results (there's usually only one, but it's a list)
for (ModerationResult result : moderation.getResults()) {
    System.out.println("\nModeration Result:");
    System.out.println("Flagged: " + result.isFlagged());

    // Access categories
    Categories categories = this.result.getCategories();
    System.out.println("\nCategories:");
    System.out.println("Law: " + categories.isLaw());
    System.out.println("Financial: " + categories.isFinancial());
    System.out.println("PII: " + categories.isPii());
    System.out.println("Sexual: " + categories.isSexual());
    System.out.println("Hate: " + categories.isHate());
    System.out.println("Harassment: " + categories.isHarassment());
    System.out.println("Self-Harm: " + categories.isSelfHarm());
    System.out.println("Sexual/Minors: " + categories.isSexualMinors());
    System.out.println("Hate/Threatening: " + categories.isHateThreatening());
    System.out.println("Violence/Graphic: " + categories.isViolenceGraphic());
    System.out.println("Self-Harm/Intent: " + categories.isSelfHarmIntent());
    System.out.println("Self-Harm/Instructions: " + categories.isSelfHarmInstructions());
    System.out.println("Harassment/Threatening: " + categories.isHarassmentThreatening());
    System.out.println("Violence: " + categories.isViolence());

    // Access category scores
    CategoryScores scores = this.result.getCategoryScores();
    System.out.println("\nCategory Scores:");
    System.out.println("Law: " + scores.getLaw());
    System.out.println("Financial: " + scores.getFinancial());
    System.out.println("PII: " + scores.getPii());
    System.out.println("Sexual: " + scores.getSexual());
    System.out.println("Hate: " + scores.getHate());
    System.out.println("Harassment: " + scores.getHarassment());
    System.out.println("Self-Harm: " + scores.getSelfHarm());
    System.out.println("Sexual/Minors: " + scores.getSexualMinors());
    System.out.println("Hate/Threatening: " + scores.getHateThreatening());
    System.out.println("Violence/Graphic: " + scores.getViolenceGraphic());
    System.out.println("Self-Harm/Intent: " + scores.getSelfHarmIntent());
    System.out.println("Self-Harm/Instructions: " + scores.getSelfHarmInstructions());
    System.out.println("Harassment/Threatening: " + scores.getHarassmentThreatening());
    System.out.println("Violence: " + scores.getViolence());
}
```

Example 4 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-mistral-ai</artifactId>
</dependency>
```

---

## Moderation :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/moderation/mistral-ai-moderation.html

**Contents:**
- Moderation
- Introduction
- Prerequisites
- Auto-configuration
- Moderation Properties
  - Connection Properties
  - Configuration Properties
- Runtime Options
- Manual Configuration
- Example Code

This version is still in development and is not considered stable yet. For the latest snapshot version, please use Spring AI 1.1.2!

Spring AI supports the new moderation service introduced by Mistral AI and powered by the Mistral Moderation model. It enables the detection of harmful text content along several policy dimensions. Follow this link for more information on the Mistral AI moderation model.

Create an Mistral AI account and obtain an API key. You can sign up at Mistral AI registration page and generate an API key on the API Keys page.

Add the spring-ai-mistral-ai dependency to your project’s build file. For more information, refer to the Dependency Management section.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Mistral AI Moderation Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

The prefix spring.ai.mistralai is used as the property prefix that lets you connect to Mistral AI.

spring.ai.mistralai.base-url

The URL to connect to

spring.ai.mistralai.api-key

Enabling and disabling of the moderation auto-configurations are now configured via top level properties with the prefix spring.ai.model.moderation.

To enable, spring.ai.model.moderation=mistral (It is enabled by default)

To disable, spring.ai.model.moderation=none (or any value which doesn’t match mistral)

This change is done to allow configuration of multiple models.

The prefix spring.ai.mistralai.moderation is used as the property prefix for configuring the Mistral AI moderation model.

spring.ai.model.moderation

Enable Moderation model

spring.ai.mistralai.moderation.base-url

The URL to connect to

spring.ai.mistralai.moderation.api-key

spring.ai.mistralai.moderation.options.model

ID of the model to use for moderation.

mistral-moderation-latest

The MistralAiModerationOptions class provides the options to use when making a moderation request. On start-up, the options specified by spring.ai.mistralai.moderation are used, but you can override these at runtime.

Add the spring-ai-mistral-ai dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

Next, create an MistralAiModerationModel:

The MistralAiModerationModelIT test provides some general examples of how to use the library. You can refer to this test for more detailed usage examples.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-mistral-ai</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-mistral-ai'
}
```

Example 3 (java):
```java
MistralAiModerationOptions moderationOptions = MistralAiModerationOptions.builder()
    .model("mistral-moderation-latest")
    .build();

ModerationPrompt moderationPrompt = new ModerationPrompt("Text to be moderated", this.moderationOptions);
ModerationResponse response = mistralAiModerationModel.call(this.moderationPrompt);

// Access the moderation results
Moderation moderation = moderationResponse.getResult().getOutput();

// Print general information
System.out.println("Moderation ID: " + moderation.getId());
System.out.println("Model used: " + moderation.getModel());

// Access the moderation results (there's usually only one, but it's a list)
for (ModerationResult result : moderation.getResults()) {
    System.out.println("\nModeration Result:");
    System.out.println("Flagged: " + result.isFlagged());

    // Access categories
    Categories categories = this.result.getCategories();
    System.out.println("\nCategories:");
    System.out.println("Law: " + categories.isLaw());
    System.out.println("Financial: " + categories.isFinancial());
    System.out.println("PII: " + categories.isPii());
    System.out.println("Sexual: " + categories.isSexual());
    System.out.println("Hate: " + categories.isHate());
    System.out.println("Harassment: " + categories.isHarassment());
    System.out.println("Self-Harm: " + categories.isSelfHarm());
    System.out.println("Sexual/Minors: " + categories.isSexualMinors());
    System.out.println("Hate/Threatening: " + categories.isHateThreatening());
    System.out.println("Violence/Graphic: " + categories.isViolenceGraphic());
    System.out.println("Self-Harm/Intent: " + categories.isSelfHarmIntent());
    System.out.println("Self-Harm/Instructions: " + categories.isSelfHarmInstructions());
    System.out.println("Harassment/Threatening: " + categories.isHarassmentThreatening());
    System.out.println("Violence: " + categories.isViolence());

    // Access category scores
    CategoryScores scores = this.result.getCategoryScores();
    System.out.println("\nCategory Scores:");
    System.out.println("Law: " + scores.getLaw());
    System.out.println("Financial: " + scores.getFinancial());
    System.out.println("PII: " + scores.getPii());
    System.out.println("Sexual: " + scores.getSexual());
    System.out.println("Hate: " + scores.getHate());
    System.out.println("Harassment: " + scores.getHarassment());
    System.out.println("Self-Harm: " + scores.getSelfHarm());
    System.out.println("Sexual/Minors: " + scores.getSexualMinors());
    System.out.println("Hate/Threatening: " + scores.getHateThreatening());
    System.out.println("Violence/Graphic: " + scores.getViolenceGraphic());
    System.out.println("Self-Harm/Intent: " + scores.getSelfHarmIntent());
    System.out.println("Self-Harm/Instructions: " + scores.getSelfHarmInstructions());
    System.out.println("Harassment/Threatening: " + scores.getHarassmentThreatening());
    System.out.println("Violence: " + scores.getViolence());
}
```

Example 4 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-mistral-ai</artifactId>
</dependency>
```

---

## Moderation :: Spring AI Reference

**URL:** https://docs.spring.io/spring-ai/reference/api/moderation/mistral-ai-moderation.html

**Contents:**
- Moderation
- Introduction
- Prerequisites
- Auto-configuration
- Moderation Properties
  - Connection Properties
  - Configuration Properties
- Runtime Options
- Manual Configuration
- Example Code

Spring AI supports the new moderation service introduced by Mistral AI and powered by the Mistral Moderation model. It enables the detection of harmful text content along several policy dimensions. Follow this link for more information on the Mistral AI moderation model.

Create an Mistral AI account and obtain an API key. You can sign up at Mistral AI registration page and generate an API key on the API Keys page.

Add the spring-ai-mistral-ai dependency to your project’s build file. For more information, refer to the Dependency Management section.

There has been a significant change in the Spring AI auto-configuration, starter modules' artifact names. Please refer to the upgrade notes for more information.

Spring AI provides Spring Boot auto-configuration for the Mistral AI Moderation Model. To enable it add the following dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

The prefix spring.ai.mistralai is used as the property prefix that lets you connect to Mistral AI.

spring.ai.mistralai.base-url

The URL to connect to

spring.ai.mistralai.api-key

Enabling and disabling of the moderation auto-configurations are now configured via top level properties with the prefix spring.ai.model.moderation.

To enable, spring.ai.model.moderation=mistral (It is enabled by default)

To disable, spring.ai.model.moderation=none (or any value which doesn’t match mistral)

This change is done to allow configuration of multiple models.

The prefix spring.ai.mistralai.moderation is used as the property prefix for configuring the Mistral AI moderation model.

spring.ai.model.moderation

Enable Moderation model

spring.ai.mistralai.moderation.base-url

The URL to connect to

spring.ai.mistralai.moderation.api-key

spring.ai.mistralai.moderation.options.model

ID of the model to use for moderation.

mistral-moderation-latest

The MistralAiModerationOptions class provides the options to use when making a moderation request. On start-up, the options specified by spring.ai.mistralai.moderation are used, but you can override these at runtime.

Add the spring-ai-mistral-ai dependency to your project’s Maven pom.xml file:

or to your Gradle build.gradle build file:

Next, create an MistralAiModerationModel:

The MistralAiModerationModelIT test provides some general examples of how to use the library. You can refer to this test for more detailed usage examples.

**Examples:**

Example 1 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-mistral-ai</artifactId>
</dependency>
```

Example 2 (unknown):
```unknown
dependencies {
    implementation 'org.springframework.ai:spring-ai-starter-model-mistral-ai'
}
```

Example 3 (java):
```java
MistralAiModerationOptions moderationOptions = MistralAiModerationOptions.builder()
    .model("mistral-moderation-latest")
    .build();

ModerationPrompt moderationPrompt = new ModerationPrompt("Text to be moderated", this.moderationOptions);
ModerationResponse response = mistralAiModerationModel.call(this.moderationPrompt);

// Access the moderation results
Moderation moderation = moderationResponse.getResult().getOutput();

// Print general information
System.out.println("Moderation ID: " + moderation.getId());
System.out.println("Model used: " + moderation.getModel());

// Access the moderation results (there's usually only one, but it's a list)
for (ModerationResult result : moderation.getResults()) {
    System.out.println("\nModeration Result:");
    System.out.println("Flagged: " + result.isFlagged());

    // Access categories
    Categories categories = this.result.getCategories();
    System.out.println("\nCategories:");
    System.out.println("Law: " + categories.isLaw());
    System.out.println("Financial: " + categories.isFinancial());
    System.out.println("PII: " + categories.isPii());
    System.out.println("Sexual: " + categories.isSexual());
    System.out.println("Hate: " + categories.isHate());
    System.out.println("Harassment: " + categories.isHarassment());
    System.out.println("Self-Harm: " + categories.isSelfHarm());
    System.out.println("Sexual/Minors: " + categories.isSexualMinors());
    System.out.println("Hate/Threatening: " + categories.isHateThreatening());
    System.out.println("Violence/Graphic: " + categories.isViolenceGraphic());
    System.out.println("Self-Harm/Intent: " + categories.isSelfHarmIntent());
    System.out.println("Self-Harm/Instructions: " + categories.isSelfHarmInstructions());
    System.out.println("Harassment/Threatening: " + categories.isHarassmentThreatening());
    System.out.println("Violence: " + categories.isViolence());

    // Access category scores
    CategoryScores scores = this.result.getCategoryScores();
    System.out.println("\nCategory Scores:");
    System.out.println("Law: " + scores.getLaw());
    System.out.println("Financial: " + scores.getFinancial());
    System.out.println("PII: " + scores.getPii());
    System.out.println("Sexual: " + scores.getSexual());
    System.out.println("Hate: " + scores.getHate());
    System.out.println("Harassment: " + scores.getHarassment());
    System.out.println("Self-Harm: " + scores.getSelfHarm());
    System.out.println("Sexual/Minors: " + scores.getSexualMinors());
    System.out.println("Hate/Threatening: " + scores.getHateThreatening());
    System.out.println("Violence/Graphic: " + scores.getViolenceGraphic());
    System.out.println("Self-Harm/Intent: " + scores.getSelfHarmIntent());
    System.out.println("Self-Harm/Instructions: " + scores.getSelfHarmInstructions());
    System.out.println("Harassment/Threatening: " + scores.getHarassmentThreatening());
    System.out.println("Violence: " + scores.getViolence());
}
```

Example 4 (xml):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-mistral-ai</artifactId>
</dependency>
```

---
