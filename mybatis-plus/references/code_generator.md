# Mybatis-Plus - Code Generator

**Pages:** 2

---

## 自定义ID生成器 | MyBatis-Plus

**URL:** https://baomidou.com/guides/id-generator/

**Contents:**
- 自定义ID生成器
- 如何自定义
  - Spring Boot 集成
    - 方式一：声明为Bean供Spring扫描注入
    - 方式二：使用配置类
    - 方式三：通过MybatisPlusPropertiesCustomizer自定义
  - Spring 集成
    - 方式一：XML配置
    - 方式二：注解配置
- 与KeyGenerator的差异

MyBatis-Plus 提供了灵活的自定义ID生成器功能，允许开发者根据业务需求定制ID生成策略。从3.3.0版本开始，默认使用雪花算法结合不含中划线的UUID作为ID生成方式。

MyBatis-Plus自带主键生成策略对比

MyBatis-Plus 提供了多种方式来实现自定义ID生成器，以下是一些示例工程和配置方法。

MyBatis-Plus的IdentifierGenerator主要用于生成数据库表的主键ID，而KeyGenerator是MyBatis框架中的一个接口，用于在执行SQL语句时生成键值，通常用于生成自增主键或者在执行INSERT语句后获取新生成的ID。

IdentifierGenerator更加专注于主键ID的生成，而KeyGenerator则更加通用，可以用于多种键值生成场景。在使用MyBatis-Plus时，通常推荐使用IdentifierGenerator来生成主键ID，因为它与MyBatis-Plus的集成更加紧密，提供了更多的便利性和功能。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (java):
```java
@Componentpublic class CustomIdGenerator implements IdentifierGenerator {    @Override    public Long nextId(Object entity) {        // 使用实体类名作为业务键，或者提取参数生成业务键        String bizKey = entity.getClass().getName();        // 根据业务键调用分布式ID生成服务        long id = ...; // 调用分布式ID生成逻辑        // 返回生成的ID值        return id;    }}
```

Example 2 (python):
```python
@Beanpublic IdentifierGenerator idGenerator() {    return new CustomIdGenerator();}
```

Example 3 (php):
```php
@Beanpublic MybatisPlusPropertiesCustomizer plusPropertiesCustomizer() {    return plusProperties -> plusProperties.getGlobalConfig().setIdentifierGenerator(new CustomIdGenerator());}
```

Example 4 (jsx):
```jsx
<bean name="customIdGenerator" class="com.example.CustomIdGenerator"/><bean id="globalConfig" class="com.baomidou.mybatisplus.core.config.GlobalConfig">    <property name="identifierGenerator" ref="customIdGenerator"/></bean>
```

---

## 代码生成器 | MyBatis-Plus

**URL:** https://baomidou.com/guides/new-code-generator/

**Contents:**
- 代码生成器
- 安装
- 生成方式
- 使用
  - 快速生成
  - 交互式生成
- 配置
- 资源

全新的 MyBatis-Plus 代码生成器，通过 builder 模式可以快速生成你想要的代码，快速且优雅，跟随下面的代码一睹为快。

全新的代码生成器添加于 3.5.1 版本，且对历史版本不兼容！如果您用的是 3.5.1 以下的版本，请参考 代码生成器 进行配置与使用。

由于代码生成器用到了模板引擎，请自行引入您喜好的模板引擎。MyBatis-Plus Generator 支持如下模板引擎：

如果您还想使用或适配其他模板引擎，可自行继承 AbstractTemplateEngine 并参考其他模板引擎实现自定义。

如果是已知数据库（无版本兼容问题），请继续按照原有的SQL查询方式继续使用，示例代码如下：

当字段长度为 1 时，无法转换成 Boolean 字段，建议在指定数据库连接时添加 &tinyInt1isBit=true。

当字段长度大于 1 时，默认转换成 Byte，如果想继续转换成 Integer，可使用如下代码：

在 CodeGenerator 中的 main 方法中直接添加生成器代码，并进行相关配置，然后直接运行即可生成代码。

交互式生成在运行之后，会提示您输入相应的内容，等待配置输入完整之后就自动生成相关代码。

如果您需要更多例子可查看 test 包下面的 samples。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (php):
```php
FastAutoGenerator.create("url", "username", "password")        .globalConfig(builder -> builder                .author("Baomidou")                .outputDir(Paths.get(System.getProperty("user.dir")) + "/src/main/java")                .commentDate("yyyy-MM-dd")        )        .packageConfig(builder -> builder                .parent("com.baomidou.mybatisplus")                .entity("entity")                .mapper("mapper")                .service("service")                .serviceImpl("service.impl")                .xml("mapper.xml")        )        .strategyConfig(builder -> builder                .entityBuilder()                .enableLombok()        )        .templateEngine(new FreemarkerTemplateEngine())        .execute();
```

Example 2 (typescript):
```typescript
<dependency>    <groupId>com.baomidou</groupId>    <artifactId>mybatis-plus-generator</artifactId>    <version>3.5.15</version></dependency>
```

Example 3 (json):
```json
implementation 'com.baomidou:mybatis-plus-generator:3.5.15'
```

Example 4 (php):
```php
// MYSQL 示例 切换至SQL查询方式,需要指定好 dbQuery 与 typeConvert 来生成FastAutoGenerator.create("url", "username", "password")                .dataSourceConfig(builder ->                        builder.databaseQueryClass(SQLQuery.class)                                .typeConvert(new MySqlTypeConvert())                                .dbQuery(new MySqlQuery())                )                // Other Config ...
```

---
