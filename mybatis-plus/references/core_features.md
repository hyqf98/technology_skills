# Mybatis-Plus - Core Features

**Pages:** 7

---

## 代码生成器 | MyBatis-Plus

**URL:** https://baomidou.com/guides/code-generator/

**Contents:**
- 代码生成器
- 使用教程
  - 添加依赖
  - 编写配置
- 自定义模板引擎
- 自定义代码模板
- 自定义属性注入
- 字段其他信息查询注入

AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、Mapper XML、Service、Controller 等各个模块的代码，极大的提升了开发效率。

老代码生成器适用于 3.5.1 以下版本，如果您用的是 3.5.1 以上的版本，请参考 新代码生成器 进行配置与使用，新的代码生成器更加的简洁与强大，推荐大家都升级到新的代码生成器。

MyBatis-Plus 从 3.0.3 之后移除了代码生成器与模板引擎的默认依赖，需要手动添加相关依赖：

添加 模板引擎 依赖，MyBatis-Plus 支持 Velocity（默认）、Freemarker、Beetl，用户可以选择自己熟悉的模板引擎，如果都不满足您的要求，可以采用自定义模板引擎。

注意！如果您选择了非默认引擎，需要在 AutoGenerator 中 设置模板引擎。

MyBatis-Plus 的代码生成器提供了大量的自定义参数供用户选择，能够满足绝大部分人的使用需求。

更多生成器配置请移步至 代码生成器配置 查看。

请继承类 com.baomidou.mybatisplus.generator.engine.AbstractTemplateEngine

自定义模板有哪些可用参数？AbstractTemplateEngine 类中方法 getObjectMap 返回 objectMap 的所有值都可用。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (java):
```java
// 演示例子，执行 main 方法控制台输入模块表名回车自动生成对应项目目录中public class CodeGenerator {
    /**     * <p>     * 读取控制台内容     * </p>     */    public static String scanner(String tip) {        Scanner scanner = new Scanner(System.in);        StringBuilder help = new StringBuilder();        help.append("请输入" + tip + "：");        System.out.println(help.toString());        if (scanner.hasNext()) {            String ipt = scanner.next();            if (StringUtils.isNotBlank(ipt)) {                return ipt;            }        }        throw new MybatisPlusException("请输入正确的" + tip + "！");    }
    public static void main(String[] args) {        // 代码生成器        AutoGenerator mpg = new AutoGenerator();
        // 全局配置        GlobalConfig gc = new GlobalConfig();        String projectPath = System.getProperty("user.dir");        gc.setOutputDir(projectPath + "/src/main/java");        gc.setAuthor("jobob");        gc.setOpen(false);        // gc.setSwagger2(true); 实体属性 Swagger2 注解        mpg.setGlobalConfig(gc);
        // 数据源配置        DataSourceConfig dsc = new DataSourceConfig();        dsc.setUrl("jdbc:mysql://localhost:3306/ant?useUnicode=true&useSSL=false&characterEncoding=utf8");        // dsc.setSchemaName("public");        dsc.setDriverName("com.mysql.jdbc.Driver");        dsc.setUsername("root");        dsc.setPassword("密码");        mpg.setDataSource(dsc);
        // 包配置        PackageConfig pc = new PackageConfig();        pc.setModuleName(scanner("模块名"));        pc.setParent("com.baomidou.ant");        mpg.setPackageInfo(pc);
        // 自定义配置        InjectionConfig cfg = new InjectionConfig() {            @Override            public void initMap() {                // to do nothing            }        };
        // 如果模板引擎是 freemarker        String templatePath = "/templates/mapper.xml.ftl";        // 如果模板引擎是 velocity        // String templatePath = "/templates/mapper.xml.vm";
        // 自定义输出配置        List<FileOutConfig> focList = new ArrayList<>();        // 自定义配置会被优先输出        focList.add(new FileOutConfig(templatePath) {            @Override            public String outputFile(TableInfo tableInfo) {                // 自定义输出文件名 ， 如果你 Entity 设置了前后缀、此处注意 xml 的名称会跟着发生变化！！                return projectPath + "/src/main/resources/mapper/" + pc.getModuleName()                        + "/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;            }        });        /*        cfg.setFileCreate(new IFileCreate() {            @Override            public boolean isCreate(ConfigBuilder configBuilder, FileType fileType, String filePath) {                // 判断自定义文件夹是否需要创建                checkDir("调用默认方法创建的目录，自定义目录用");                if (fileType == FileType.MAPPER) {                    // 已经生成 mapper 文件判断存在，不想重新生成返回 false                    return !new File(filePath).exists();                }                // 允许生成模板文件                return true;            }        });        */        cfg.setFileOutConfigList(focList);        mpg.setCfg(cfg);
        // 配置模板        TemplateConfig templateConfig = new TemplateConfig();
        // 配置自定义输出模板        //指定自定义模板路径，注意不要带上.ftl/.vm, 会根据使用的模板引擎自动识别        // templateConfig.setEntity("templates/entity2.java");        // templateConfig.setService();        // templateConfig.setController();
        templateConfig.setXml(null);        mpg.setTemplate(templateConfig);
        // 策略配置        StrategyConfig strategy = new StrategyConfig();        strategy.setNaming(NamingStrategy.underline_to_camel);        strategy.setColumnNaming(NamingStrategy.underline_to_camel);        strategy.setSuperEntityClass("你自己的父类实体,没有就不用设置!");        strategy.setEntityLombokModel(true);        strategy.setRestControllerStyle(true);        // 公共父类        strategy.setSuperControllerClass("你自己的父类控制器,没有就不用设置!");        // 写于父类中的公共字段        strategy.setSuperEntityColumns("id");        strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));        strategy.setControllerMappingHyphenStyle(true);        strategy.setTablePrefix(pc.getModuleName() + "_");        mpg.setStrategy(strategy);        mpg.setTemplateEngine(new FreemarkerTemplateEngine());        mpg.execute();    }
}
```

Example 2 (typescript):
```typescript
<dependency>    <groupId>com.baomidou</groupId>    <artifactId>mybatis-plus-generator</artifactId>    <version>3.5.0</version></dependency>
```

Example 3 (typescript):
```typescript
<dependency>    <groupId>org.apache.velocity</groupId>    <artifactId>velocity-engine-core</artifactId>    <version>最新版本</version></dependency>
```

Example 4 (typescript):
```typescript
<dependency>    <groupId>org.freemarker</groupId>    <artifactId>freemarker</artifactId>    <version>最新版本</version></dependency>
```

---

## 代码生成器配置 | MyBatis-Plus

**URL:** https://baomidou.com/reference/code-generator-configuration

**Contents:**
- 代码生成器配置
  - 主要特点
  - 使用场景
  - 如何使用
  - 示例代码
- 基本配置
  - 数据源配置（dataSource）
  - 数据库表配置（strategy）
  - 包名配置（packageInfo）
  - 模板配置（template）

MyBatis-Plus 代码生成器是一个强大的工具，它能够根据数据库表结构自动生成对应的实体类、Mapper接口、XML映射文件以及Service层代码。这个工具极大地简化了基于MyBatis框架的开发流程，提高了开发效率，尤其是在需要处理大量数据库表时。

通过MyBatis-Plus代码生成器，开发者可以更加专注于业务逻辑的实现，而不是繁琐的CRUD代码编写，从而提升开发效率和代码质量。

数据源配置用于指定需要生成代码的具体数据库。通过配置数据源，代码生成器能够连接到数据库并获取表结构信息，以便生成相应的代码。

数据库表配置用于指定需要生成哪些表的代码或者排除哪些表。通过策略配置，可以灵活地控制代码生成的范围。

包名配置用于指定生成代码的包路径。通过配置包名，可以确保生成的代码放置在正确的目录结构中。

模板配置允许自定义代码生成的模板，实现个性化操作。通过模板配置，可以定制生成代码的格式和内容。

全局策略配置提供了一些全局的设置，如作者信息、生成路径等。

注入配置允许注入自定义参数等操作以实现个性化操作。通过注入配置，可以在代码生成过程中添加额外的逻辑。

通过以上配置，MyBatis-Plus 代码生成器可以根据你的需求生成符合项目结构的代码，大大提高了开发效率。记得根据实际项目需求调整配置参数，以达到最佳的代码生成效果。

数据源配置是 MyBatis-Plus 代码生成器的关键部分，它定义了如何连接到数据库以及如何查询数据库信息。以下是数据源配置的详细说明和示例。

在这个示例中，我们配置了一个 MySQL 数据库的数据源。我们指定了数据库类型、连接 URL、用户名、密码和驱动名称。这些信息将用于建立与数据库的连接，并从中获取表结构信息，以便生成相应的 Java 代码。

请根据你的实际数据库配置调整这些参数，确保它们与你的数据库环境相匹配。

数据库表配置用于定义生成代码时如何处理数据库表和字段。通过策略配置，可以指定生成哪些表的代码、如何命名实体类和字段、以及是否包含特定的注解或属性。

在这个示例中，我们配置了一个策略，指定了大写命名模式、使用 Lombok 模型、生成 REST 风格的控制器，并指定了需要包含和排除的表。我们还设置了表前缀和驼峰转连字符的控制器映射风格。

请根据你的项目需求调整这些配置参数，以确保生成的代码符合你的期望。

包名配置用于定义生成代码的包结构，确保生成的代码放置在正确的目录中。通过配置包名，可以控制代码的组织方式，使其符合项目的架构设计。

在这个示例中，我们配置了一个包结构，其中父包名为 com.example，每个子包名都根据其功能进行了设置。例如，实体类将放置在 com.example.mybatisplus.entity 包中，服务接口将放置在 com.example.mybatisplus.service 包中，以此类推。

请根据你的项目结构和组织习惯调整这些配置参数，以确保生成的代码能够正确地集成到你的项目中。

模板配置允许开发者自定义代码生成器使用的模板，以生成符合特定项目风格和需求的代码。MyBatis-Plus 代码生成器支持多种类型的模板，包括实体类、服务类、Mapper 接口、XML 映射文件和控制器类等。

在这个示例中，我们配置了不同类型的模板路径。例如，实体类模板路径设置为 templates/entity.java.vm，服务类模板路径设置为 templates/service.java.vm，以此类推。这些模板路径指向了项目中自定义的模板文件，代码生成器将使用这些模板来生成相应的代码。

请确保你的模板文件路径正确，并且模板文件遵循 Velocity 或 Freemarker 等模板引擎的语法。通过自定义模板，你可以控制生成的代码的结构、注释、命名风格等，以满足项目的特定需求。

在实际使用中，你可能需要根据项目的具体情况调整模板配置，例如，如果你的项目使用 Kotlin 语言，则需要配置 entityKt 模板路径。如果你的项目不需要生成某些类型的代码（如 XML 映射文件），则可以不配置相应的模板。

全局策略配置提供了一些全局的设置，如输出目录、文件覆盖、开发者信息等，以及一些高级选项，如 Kotlin 模式、Swagger2 集成、ActiveRecord 模式等。

在这个示例中，我们配置了全局策略，指定了输出目录、文件覆盖、开发者信息等，并设置了各种命名方式和主键ID类型。这些配置将影响生成的代码的结构和内容。

请根据你的项目需求和偏好调整这些配置参数，以确保生成的代码符合你的期望。例如，如果你希望生成的实体类名以 Entity 结尾，可以将 entityName 设置为 %sEntity。如果你希望在生成的 XML 文件中包含二级缓存配置，可以将 enableCache 设置为 true。

注入配置允许开发者自定义代码生成器的行为，包括自定义返回配置、自定义输出文件、自定义文件创建逻辑等。这些配置提供了灵活性，使得代码生成器能够适应更复杂的项目需求。

在这个示例中，我们配置了注入配置，包括自定义 Map 对象、自定义输出文件配置和自定义文件创建逻辑。

请根据你的项目需求调整这些配置参数，以确保生成的代码符合你的期望。例如，如果你需要在模板中访问额外的配置信息，可以在 initMap 方法中添加这些信息。如果你需要生成特定格式的文件，可以在 getFileOutConfig 方法中指定相应的模板和输出路径。如果你需要自定义文件创建逻辑，可以在 getFileCreate 方法中实现相应的判断逻辑。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (java):
```java
import com.baomidou.mybatisplus.generator.AutoGenerator;import com.baomidou.mybatisplus.generator.config.*;import com.baomidou.mybatisplus.generator.config.rules.DbType;import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
public class CodeGenerator {
    public static void main(String[] args) {        // 全局配置        GlobalConfig globalConfig = new GlobalConfig();        globalConfig.setOutputDir(System.getProperty("user.dir") + "/src/main/java") // 设置输出目录                .setAuthor("Your Name") // 设置作者                .setOpen(false) // 设置生成后是否自动打开目录                .setFileOverride(true) // 设置文件存在时是否覆盖                .setServiceName("%sService") // 设置Service接口名后缀                .setIdType(IdType.AUTO) // 设置主键生成策略                .setSwagger2(true); // 设置是否生成Swagger注解
        // 数据源配置        DataSourceConfig dataSourceConfig = new DataSourceConfig();        dataSourceConfig.setDbType(DbType.MYSQL) // 设置数据库类型                .setUrl("jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&serverTimezone=UTC") // 数据库连接URL                .setUsername("root") // 数据库用户名                .setPassword("password") // 数据库密码                .setDriverName("com.mysql.cj.jdbc.Driver"); // 数据库驱动类名
        // 策略配置        StrategyConfig strategyConfig = new StrategyConfig();        strategyConfig.setInclude("user", "order") // 指定需要生成代码的表名                .setNaming(NamingStrategy.underline_to_camel) // 设置表名转类名策略                .setColumnNaming(NamingStrategy.underline_to_camel) // 设置列名转属性名策略                .setEntityLombokModel(true) // 设置实体类使用Lombok模型                .setRestControllerStyle(true) // 设置Controller使用REST风格                .setTablePrefix(new String[]{"tbl_"}); // 设置表名前缀
        // 包配置        PackageConfig packageConfig = new PackageConfig();        packageConfig.setParent("com.example") // 设置父包名                .setMapper("mapper") // 设置Mapper接口所在的子包名                .setEntity("entity") // 设置实体类所在的子包名                .setController("controller") // 设置Controller所在的子包名                .setService("service") // 设置Service所在的子包名                .setXml("mapper"); // 设置Mapper XML文件所在的子包名
        // 模板配置        TemplateConfig templateConfig = new TemplateConfig();        templateConfig.setXml(null) // 不生成XML文件                .setController("templates/controller.java.vm") // 设置Controller模板路径                .setEntity("templates/entity.java.vm") // 设置实体类模板路径                .setMapper("templates/mapper.java.vm"); // 设置Mapper接口模板路径
        // 整合配置        AutoGenerator autoGenerator = new AutoGenerator();        autoGenerator.setGlobalConfig(globalConfig)                .setDataSource(dataSourceConfig)                .setStrategy(strategyConfig)                .setPackageInfo(packageConfig)                .setTemplate(templateConfig);
        // 执行生成        autoGenerator.execute();    }}
```

Example 2 (json):
```json
DataSourceConfig dataSourceConfig = new DataSourceConfig();dataSourceConfig.setDbType(DbType.MYSQL) // 设置数据库类型，如MySQL、Oracle等    .setUrl("jdbc:mysql://localhost:3306/mybatis_plus") // 数据库连接URL    .setUsername("root") // 数据库用户名    .setPassword("password") // 数据库密码    .setDriverName("com.mysql.cj.jdbc.Driver"); // 数据库驱动类名
```

Example 3 (unknown):
```unknown
StrategyConfig strategyConfig = new StrategyConfig();strategyConfig.setInclude("user", "order") // 指定需要生成代码的表名    .setExclude("user_detail") // 排除不需要生成代码的表名    .setEntityLombokModel(true) // 设置实体类使用Lombok模型    .setRestControllerStyle(true); // 设置Controller使用REST风格
```

Example 4 (unknown):
```unknown
PackageConfig packageConfig = new PackageConfig();packageConfig.setParent("com.example") // 设置父包名    .setMapper("mapper") // 设置Mapper接口所在的子包名    .setEntity("entity") // 设置实体类所在的子包名    .setController("controller"); // 设置Controller所在的子包名
```

---

## 高级特性 | MyBatis-Plus

**URL:** https://baomidou.com/guides/advanced-features/

**Contents:**
- 高级特性
- 数据审计（对账）
- 数据敏感词过滤
- 数据范围（数据权限）
- 表结构自动维护
- 字段数据绑定（字典回写）
- 虚拟属性绑定
- 字段加密解密
- 字段脱敏
- 多数据源分库分表（读写分离）

Mybatis-Mate 是为 MyBatis-Plus 提供的企业级模块，旨在更敏捷优雅处理数据。

请注意必须注入 IDataScopeProvider 实现类处理数据权限，关于数据传参支持 2 种方式： 1，自定义 mapper 方法通过方法参数传递，在 setWhere 方法 Object[] args 参数中获取 2，利用 ThreadLocal 传递参数，你可以拦截 controller 层或者 service 层设置数据权限处理参数，更多可以 👉参考

使用示例：👉 mybatis-mate-encrypt

注解 FieldEncrypt 实现数据加解密，支持多种加密算法

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (typescript):
```typescript
// 1，异步回调，注意 @EnableAsync 开启异步applicationEventPublisher.publishEvent(new DataAuditEvent((t) -> {    List<Change> changes = t.apply(newVersion, oldVersion);    for (Change valueChange : changes) {        ValueChange change = (ValueChange) valueChange;        System.err.println(String.format("%s不匹配，期望值 %s 实际值 %s", change.getPropertyName(), change.getLeft(), change.getRight()));    }}));
// 2，手动调用对比DataAuditor.compare(obj1, obj2);
```

Example 2 (sql):
```sql
// 测试 test 类型数据权限范围，混合分页模式@DataScope(type = "test", value = {        // 关联表 user 别名 u 指定部门字段权限        @DataColumn(alias = "u", name = "department_id"),        // 关联表 user 别名 u 指定手机号字段（自己判断处理）        @DataColumn(alias = "u", name = "mobile")})@Select("select u.* from user u")List<User> selectTestList(IPage<User> page, Long id, @Param("name") String username);
// 测试数据权限，最终执行 SQL 语句SELECT u.* FROM user u WHERE (u.department_id IN ('1', '2', '3', '5')) AND u.mobile LIKE '%1533%' LIMIT 1,10
```

Example 3 (sql):
```sql
@Componentpublic class MysqlDdl implements IDdl {
    /**     * 执行 SQL 脚本方式     */    @Override    public List<String> getSqlFiles() {        return Arrays.asList(                "db/tag-schema.sql",                "D:\\db\\tag-data.sql"        );    }}
// 切换到 mysql 从库，执行 SQL 脚本ShardingKey.change("mysqlt2");ddlScript.run(new StringReader("DELETE FROM user;\n" +        "INSERT INTO user (id, username, password, sex, email) VALUES\n" +        "(20, 'Duo', '123456', 0, 'Duo@baomidou.com');"));
```

Example 4 (csharp):
```csharp
@FieldBind(type = "user_sex", target = "sexText")private Integer sex;// 绑定显示属性，非表字典（排除）@TableField(exist = false)private String sexText;
```

---

## 持久层接口 | MyBatis-Plus

**URL:** https://baomidou.com/guides/data-interface/

**Contents:**
- 持久层接口
- Service Interface
  - save
  - saveOrUpdate
  - remove
  - update
  - get
  - list
  - page
  - count

本文详细介绍了 MyBatis-Plus 进行持久化操作的各种方法，包括插入、更新、删除、查询和分页等。通过本文，您可以了解到 MyBatis-Plus 提供的各种方法是如何进行数据操作的，以及它们对应的 SQL 语句。

IService 是 MyBatis-Plus 提供的一个通用 Service 层接口，它封装了常见的 CRUD 操作，包括插入、删除、查询和分页等。通过继承 IService 接口，可以快速实现对数据库的基本操作，同时保持代码的简洁性和可维护性。

IService 接口中的方法命名遵循了一定的规范，如 get 用于查询单行，remove 用于删除，list 用于查询集合，page 用于分页查询，这样可以避免与 Mapper 层的方法混淆。

功能描述： 插入记录，根据实体对象的字段进行策略性插入。 返回值： boolean，表示插入操作是否成功。 参数说明： 类型参数名描述Tentity实体对象Collection<T>entityList实体对象集合intbatchSize插入批次数量

功能描述： 插入记录，根据实体对象的字段进行策略性插入。 返回值： boolean，表示插入操作是否成功。 参数说明：

生成的 SQL（假设默认批次大小为 3）:

示例（saveBatch 指定批次大小）：

通过上述示例，我们可以看到 save 系列方法是如何在 Service 层进行批量插入操作的，以及它们对应的 SQL 语句。这些方法大大简化了插入操作的代码编写，提高了开发效率。

功能描述： 根据实体对象的主键 ID 进行判断，存在则更新记录，否则插入记录。 返回值： boolean，表示插入或更新操作是否成功。 参数说明： 类型参数名描述Tentity实体对象Wrapper<T>updateWrapper实体对象封装操作类 UpdateWrapperCollection<T>entityList实体对象集合intbatchSize插入批次数量

功能描述： 根据实体对象的主键 ID 进行判断，存在则更新记录，否则插入记录。 返回值： boolean，表示插入或更新操作是否成功。 参数说明：

生成的 SQL（假设 id 为 1 的记录已存在）:

生成的 SQL（假设 id 为 1 的记录不存在）:

示例（saveOrUpdateBatch）：

生成的 SQL（假设 id 为 1 和 2 的记录已存在，id 为 3 的记录不存在）:

示例（saveOrUpdateBatch 指定批次大小）：

生成的 SQL（假设指定批次大小为 2）:

通过上述示例，我们可以看到 saveOrUpdate 系列方法是如何在 Service 层进行批量修改插入操作的，以及它们对应的 SQL 语句。这些方法提供了高效的数据操作方式，可以根据不同的条件进行更新或插入操作。

功能描述： 通过指定条件删除符合条件的记录。 返回值： boolean，表示删除操作是否成功。 参数说明： 类型参数名描述Wrapper<T>queryWrapper实体包装类 QueryWrapperSerializableid主键 IDMap<String, Object>columnMap表字段 map 对象Collection<? extends Serializable>idList主键 ID 列表

功能描述： 通过指定条件删除符合条件的记录。 返回值： boolean，表示删除操作是否成功。 参数说明：

通过上述示例，我们可以看到 remove 系列方法是如何在 Service 层进行删除操作的，以及它们对应的 SQL 语句。这些方法提供了灵活的数据操作方式，可以根据不同的条件进行删除操作。

功能描述： 通过指定条件更新符合条件的记录。 返回值： boolean，表示更新操作是否成功。 参数说明： 类型参数名描述Wrapper<T>updateWrapper实体对象封装操作类 UpdateWrapperTentity实体对象Collection<T>entityList实体对象集合intbatchSize更新批次数量

功能描述： 通过指定条件更新符合条件的记录。 返回值： boolean，表示更新操作是否成功。 参数说明：

示例（update UpdateWrapper 形式）：

示例（update WhereWrapper 形式）：

生成的 SQL（假设默认批次大小为 2）:

示例（updateBatchById 指定批次大小）：

生成的 SQL（假设指定批次大小为 1）:

通过上述示例，我们可以看到 update 系列方法是如何在 Service 层进行更新操作的，以及它们对应的 SQL 语句。这些方法提供了灵活的数据操作方式，可以根据不同的条件进行更新操作。

功能描述： 根据指定条件查询符合条件的记录。 返回值： 查询结果，可能是实体对象、Map 对象或其他类型。 参数说明： 类型参数名描述Serializableid主键 IDWrapper<T>queryWrapper实体对象封装操作类 QueryWrapperbooleanthrowEx有多个 result 是否抛出异常Tentity实体对象Function<? super Object, V>mapper转换函数

功能描述： 根据指定条件查询符合条件的记录。 返回值： 查询结果，可能是实体对象、Map 对象或其他类型。 参数说明：

通过上述示例，我们可以看到 get 系列方法是如何在 Service 层进行查询操作的，以及它们对应的 SQL 语句。这些方法提供了灵活的数据查询方式，可以根据不同的条件进行查询操作。

功能描述： 查询符合条件的记录。 返回值： 查询结果，可能是实体对象、Map 对象或其他类型。 参数说明： 类型参数名描述Wrapper<T>queryWrapper实体对象封装操作类 QueryWrapperCollection<? extends Serializable>idList主键 ID 列表Map<String, Object>columnMap表字段 map 对象Function<? super Object, V>mapper转换函数

功能描述： 查询符合条件的记录。 返回值： 查询结果，可能是实体对象、Map 对象或其他类型。 参数说明：

示例（list QueryWrapper 形式）：

示例（listMaps QueryWrapper 形式）：

示例（listObjs QueryWrapper 形式）：

通过上述示例，我们可以看到 list 系列方法是如何在 Service 层进行查询操作的，以及它们对应的 SQL 语句。这些方法提供了灵活的数据查询方式，可以根据不同的条件进行查询操作。

功能描述： 分页查询符合条件的记录。 返回值： 分页查询结果，包含记录列表和总记录数。 参数说明： 类型参数名描述IPage<T>page翻页对象Wrapper<T>queryWrapper实体对象封装操作类 QueryWrapper

功能描述： 分页查询符合条件的记录。 返回值： 分页查询结果，包含记录列表和总记录数。 参数说明：

示例（page QueryWrapper 形式）：

示例（pageMaps QueryWrapper 形式）：

通过上述示例，我们可以看到 page 系列方法是如何在 Service 层进行分页查询操作的，以及它们对应的 SQL 语句。这些方法提供了灵活的数据查询方式，可以根据不同的条件进行分页查询操作。

功能描述： 查询符合条件的记录总数。 返回值： 符合条件的记录总数。 参数说明： 类型参数名描述Wrapper<T>queryWrapper实体对象封装操作类 QueryWrapper

功能描述： 查询符合条件的记录总数。 返回值： 符合条件的记录总数。 参数说明：

示例（count QueryWrapper 形式）：

通过上述示例，我们可以看到 count 方法是如何在 Service 层进行记录数统计操作的，以及它们对应的 SQL 语句。这些方法提供了灵活的数据统计方式，可以根据不同的条件进行记录数统计。

BaseMapper 是 Mybatis-Plus 提供的一个通用 Mapper 接口，它封装了一系列常用的数据库操作方法，包括增、删、改、查等。通过继承 BaseMapper，开发者可以快速地对数据库进行操作，而无需编写繁琐的 SQL 语句。

功能描述： 插入一条记录。 返回值： int，表示插入操作影响的行数，通常为 1，表示插入成功。 参数说明： 类型参数名描述Tentity实体对象

功能描述： 插入一条记录。 返回值： int，表示插入操作影响的行数，通常为 1，表示插入成功。 参数说明：

通过上述示例，我们可以看到 insert 方法是如何在 Mapper 层进行插入操作的，以及它对应的 SQL 语句。这个方法简化了插入操作的实现，使得开发者无需手动编写 SQL 语句。

功能描述： 删除符合条件的记录。 返回值： int，表示删除操作影响的行数，通常为 1，表示删除成功。 参数说明： 类型参数名描述Wrapper<T>wrapper实体对象封装操作类（可以为 null）Collection<? extends Serializable>idList主键 ID 列表(不能为 null 以及 empty)Serializableid主键 IDMap<String, Object>columnMap表字段 map 对象

功能描述： 删除符合条件的记录。 返回值： int，表示删除操作影响的行数，通常为 1，表示删除成功。 参数说明：

通过上述示例，我们可以看到 delete 系列方法是如何在 Mapper 层进行删除操作的，以及它们对应的 SQL 语句。这些方法提供了灵活的数据删除方式，可以根据不同的条件进行删除操作。

功能描述： 更新符合条件的记录。 返回值： int，表示更新操作影响的行数，通常为 1，表示更新成功。 参数说明： 类型参数名描述Tentity实体对象 (set 条件值,可为 null)Wrapper<T>updateWrapper实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句）

功能描述： 更新符合条件的记录。 返回值： int，表示更新操作影响的行数，通常为 1，表示更新成功。 参数说明：

通过上述示例，我们可以看到 update 系列方法是如何在 Mapper 层进行更新操作的，以及它们对应的 SQL 语句。这些方法提供了灵活的数据更新方式，可以根据不同的条件进行更新操作。

功能描述： 查询符合条件的记录。 返回值： 查询结果，可能是实体对象、Map 对象或其他类型。 参数说明： 类型参数名描述Serializableid主键 IDWrapper<T>queryWrapper实体对象封装操作类（可以为 null）Collection<? extends Serializable>idList主键 ID 列表(不能为 null 以及 empty)Map<String, Object>columnMap表字段 map 对象IPage<T>page分页查询条件（可以为 RowBounds.DEFAULT）

功能描述： 查询符合条件的记录。 返回值： 查询结果，可能是实体对象、Map 对象或其他类型。 参数说明：

通过上述示例，我们可以看到 select 系列方法是如何在 Mapper 层进行查询操作的，以及它们对应的 SQL 语句。这些方法提供了灵活的数据查询方式，可以根据不同的条件进行查询操作，包括单条记录查询、批量查询、条件查询、分页查询等。

选装件是 Mybatis-Plus 提供的一些扩展方法，它们位于 com.baomidou.mybatisplus.extension.injector.methods 包下。这些方法需要配合Sql 注入器使用，以扩展 Mapper 接口的功能。

使用这些选装件前，需要确保已经正确配置了 Sql 注入器。更多使用案例和详细信息，可以参考官方案例和源码注释。

源码：alwaysUpdateSomeColumnById 功能：这个方法用于在更新操作时，无论实体对象的某些字段是否有变化，都会强制更新这些字段。这在某些业务场景下非常有用，比如更新时间戳字段，确保每次更新操作都会更新该字段。 使用场景：当你需要在每次更新记录时，都更新某些特定的字段（如更新时间、版本号等），即使这些字段在实体对象中没有变化。

源码：alwaysUpdateSomeColumnById 功能：这个方法用于在更新操作时，无论实体对象的某些字段是否有变化，都会强制更新这些字段。这在某些业务场景下非常有用，比如更新时间戳字段，确保每次更新操作都会更新该字段。 使用场景：当你需要在每次更新记录时，都更新某些特定的字段（如更新时间、版本号等），即使这些字段在实体对象中没有变化。

源码：insertBatchSomeColumn 功能：这个方法用于批量插入实体对象，但只插入实体对象中指定的某些字段。这在需要批量插入数据，但又不希望插入所有字段时非常有用。 使用场景：当你需要批量插入数据，并且希望只插入实体对象中的部分字段，以提高插入效率或保护敏感数据。

源码：insertBatchSomeColumn 功能：这个方法用于批量插入实体对象，但只插入实体对象中指定的某些字段。这在需要批量插入数据，但又不希望插入所有字段时非常有用。 使用场景：当你需要批量插入数据，并且希望只插入实体对象中的部分字段，以提高插入效率或保护敏感数据。

源码：logicDeleteByIdWithFill 功能：这个方法用于逻辑删除记录，并填充实体对象中的某些字段。逻辑删除意味着不是真正从数据库中删除记录，而是通过更新某个字段（如 deleted 字段）来标记记录已被删除。 使用场景：当你需要实现逻辑删除功能，并且希望在删除操作时自动填充实体对象中的某些字段（如删除时间、删除人等）。

源码：logicDeleteByIdWithFill 功能：这个方法用于逻辑删除记录，并填充实体对象中的某些字段。逻辑删除意味着不是真正从数据库中删除记录，而是通过更新某个字段（如 deleted 字段）来标记记录已被删除。 使用场景：当你需要实现逻辑删除功能，并且希望在删除操作时自动填充实体对象中的某些字段（如删除时间、删除人等）。

通过使用这些选装件，可以进一步扩展 Mybatis-Plus 的功能，满足更多样化的业务需求。

Chain 是 Mybatis-Plus 提供的一种链式编程风格，它允许开发者以更加简洁和直观的方式编写数据库操作代码。Chain 分为 query 和 update 两大类，分别用于查询和更新操作。每类又分为普通链式和 lambda 链式两种风格，其中 lambda 链式提供了类型安全的查询条件构造，但不支持 Kotlin。

提供链式查询操作，可以连续调用方法来构建查询条件。

提供链式更新操作，可以连续调用方法来构建更新条件。

通过使用 Chain，开发者可以更加高效地编写数据库操作代码，同时保持代码的清晰和可维护性。

ActiveRecord 模式是一种设计模式，它允许实体类直接与数据库进行交互，实体类既是领域模型又是数据访问对象。在 Mybatis-Plus 中，实体类只需继承 Model 类即可获得强大的 CRUD 操作能力。

使用 ActiveRecord 模式前，需要确保项目中已注入对应实体的 BaseMapper。

通过使用 ActiveRecord 模式，开发者可以更加简洁地编写数据库操作代码，同时保持代码的清晰和可维护性。这种模式尤其适合于简单的 CRUD 操作，可以大大减少重复代码的编写。

SimpleQuery 是 Mybatis-Plus 提供的一个工具类，它对 selectList 查询后的结果进行了封装，使其可以通过 Stream 流的方式进行处理，从而简化了 API 的调用。

SimpleQuery 的一个特点是它的 peeks 参数，这是一个可变参数，类型为 Consumer...，意味着你可以连续添加多个操作，这些操作会在查询结果被处理时依次执行。

SimpleQuery 的使用方式可以参考官方测试用例。

使用 SimpleQuery 前，需要确保项目中已注入对应实体的 BaseMapper。

通过使用 SimpleQuery 工具类，开发者可以更加高效地处理查询结果，同时保持代码的简洁性和可读性。这种工具类尤其适合于需要对查询结果进行复杂处理的场景。

SimpleQuery 的 keyMap 方法提供了一种便捷的方式来查询数据库，并将查询结果封装成一个 Map，其中实体的某个属性作为键（key），实体本身作为值（value）。这个方法还支持在处理查询结果时执行额外的副作用操作，如打印日志或更新缓存。

通过使用 SimpleQuery 的 keyMap 方法，开发者可以更加高效地处理查询结果，并将其封装成易于使用的数据结构，同时还可以执行额外的副作用操作，使代码更加简洁和灵活。

SimpleQuery 的 map 方法提供了一种便捷的方式来查询数据库，并将查询结果封装成一个 Map，其中实体的某个属性作为键（key），另一个属性作为值（value）。这个方法还支持在处理查询结果时执行额外的副作用操作，如打印日志或更新缓存。

通过使用 SimpleQuery 的 map 方法，开发者可以更加高效地处理查询结果，并将其封装成易于使用的数据结构，同时还可以执行额外的副作用操作，使代码更加简洁和灵活。

SimpleQuery 的 group 方法提供了一种便捷的方式来查询数据库，并将查询结果按照实体的某个属性进行分组，封装成一个 Map。这个方法还支持在处理查询结果时执行额外的副作用操作，如打印日志或更新缓存。此外，它还允许你使用 Collector 对分组后的集合进行进一步的处理。

通过使用 SimpleQuery 的 group 方法，开发者可以更加高效地处理查询结果，并将其按照特定属性进行分组，同时还可以执行额外的副作用操作，使代码更加简洁和灵活。

SimpleQuery 的 list 方法提供了一种便捷的方式来查询数据库，并将查询结果封装成一个 List，其中列表的元素是实体的某个属性。这个方法还支持在处理查询结果时执行额外的副作用操作，如打印日志或更新缓存。

通过使用 SimpleQuery 的 list 方法，开发者可以更加高效地处理查询结果，并将其封装成易于使用的数据结构，同时还可以执行额外的副作用操作，使代码更加简洁和灵活。

Db Kit 是 Mybatis-Plus 提供的一个工具类，它允许开发者通过静态调用的方式执行 CRUD 操作，从而避免了在 Spring 环境下可能出现的 Service 循环注入问题，简化了代码，提升了开发效率。

Db Kit 的完整使用方式可以参考官方测试用例。

通过使用 Db Kit，开发者可以更加高效地执行数据库操作，同时保持代码的简洁性和可读性。这种工具类尤其适合于简单的 CRUD 操作，可以大大减少重复代码的编写。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (typescript):
```typescript
// 插入一条记录（选择字段，策略插入）boolean save(T entity);// 插入（批量）boolean saveBatch(Collection<T> entityList);// 插入（批量）boolean saveBatch(Collection<T> entityList, int batchSize);
```

Example 2 (java):
```java
// 假设有一个 User 实体对象User user = new User();user.setName("John Doe");user.setEmail("john.doe@example.com");boolean result = userService.save(user); // 调用 save 方法if (result) {    System.out.println("User saved successfully.");} else {    System.out.println("Failed to save user.");}
```

Example 3 (sql):
```sql
INSERT INTO user (name, email) VALUES ('John Doe', 'john.doe@example.com')
```

Example 4 (java):
```java
// 假设有一组 User 实体对象List<User> users = Arrays.asList(    new User("Alice", "alice@example.com"),    new User("Bob", "bob@example.com"),    new User("Charlie", "charlie@example.com"));// 使用默认批次大小进行批量插入boolean result = userService.saveBatch(users); // 调用 saveBatch 方法，默认批次大小if (result) {    System.out.println("Users saved successfully.");} else {    System.out.println("Failed to save users.");}
```

---

## 条件构造器 | MyBatis-Plus

**URL:** https://baomidou.com/guides/wrapper/

**Contents:**
- 条件构造器
- 功能详解
  - allEq
    - 使用范围
    - 方法签名
    - 参数说明
    - 示例
  - eq
    - 使用范围
    - 方法签名

MyBatis-Plus 提供了一套强大的条件构造器（Wrapper），用于构建复杂的数据库查询条件。Wrapper 类允许开发者以链式调用的方式构造查询条件，无需编写繁琐的 SQL 语句，从而提高开发效率并减少 SQL 注入的风险。

在 MyBatis-Plus 中，Wrapper 类是构建查询和更新条件的核心工具。以下是主要的 Wrapper 类及其功能：

AbstractWrapper：这是一个抽象基类，提供了所有 Wrapper 类共有的方法和属性。它定义了条件构造的基本逻辑，包括字段（column）、值（value）、操作符（condition）等。所有的 QueryWrapper、UpdateWrapper、LambdaQueryWrapper 和 LambdaUpdateWrapper 都继承自 AbstractWrapper。

QueryWrapper：专门用于构造查询条件，支持基本的等于、不等于、大于、小于等各种常见操作。它允许你以链式调用的方式添加多个查询条件，并且可以组合使用 and 和 or 逻辑。

UpdateWrapper：用于构造更新条件，可以在更新数据时指定条件。与 QueryWrapper 类似，它也支持链式调用和逻辑组合。使用 UpdateWrapper 可以在不创建实体对象的情况下，直接设置更新字段和条件。

LambdaQueryWrapper：这是一个基于 Lambda 表达式的查询条件构造器，它通过 Lambda 表达式来引用实体类的属性，从而避免了硬编码字段名。这种方式提高了代码的可读性和可维护性，尤其是在字段名可能发生变化的情况下。

LambdaUpdateWrapper：类似于 LambdaQueryWrapper，LambdaUpdateWrapper 是基于 Lambda 表达式的更新条件构造器。它允许你使用 Lambda 表达式来指定更新字段和条件，同样避免了硬编码字段名的问题。

MyBatis-Plus 的 Wrapper 类是构建复杂查询和更新条件的关键工具。它允许开发者以链式调用的方式构造 SQL 的 WHERE 子句，提供了极大的灵活性和便利性。

以下是对 Wrapper 功能的提示和注意事项。

条件判断：Wrapper 方法通常接受一个 boolean 类型的参数，用于决定是否将该条件加入到最终的 SQL 中。例如：

默认行为：如果某个方法没有显式提供 boolean 类型的参数，则默认为 true，即条件总是会被加入到 SQL 中。

泛型参数：Wrapper 类是泛型类，其中 Param 通常指的是 Wrapper 的子类实例，如 QueryWrapper、UpdateWrapper 等。

字段引用：在 LambdaWrapper 中，R 代表的是一个函数，用于引用实体类的属性，例如 Entity::getId。而在普通 Wrapper 中，R 代表的是数据库字段名。

字段名注意事项：当 R 具体类型为 String 时，表示的是数据库字段名，而不是实体类数据字段名。如果字段名是数据库关键字，需要使用转义符包裹。

集合参数：如果方法的参数是 Map 或 List，当它们为空时，对应的 SQL 条件不会被加入到最终的 SQL 中。

学习资源：对于不熟悉的函数式编程概念，可以参考学习资源进行学习。

RPC 调用中的 Wrapper：不支持也不赞成在 RPC 调用中传输 Wrapper 对象。Wrapper 对象通常包含大量信息，不适合作为传输对象。正确的做法是定义一个 DTO（数据传输对象）进行传输，然后在被调用方根据 DTO 执行相应的操作。

维护性：避免在 Controller 层使用 Map 接收值，这种做法虽然开发时方便，但会给后续的维护带来困难。

问题反馈：不接受任何关于 RPC 传输 Wrapper 报错相关的 issue 或 pr。

安全性： QueryWrapper UpdateWrapper 字段部分，如有允许 前端传入 SQL 片段 这可能会导致 SQL 注入风险 需要校验，更多查看 预防安全漏洞。

QueryWrapper(LambdaQueryWrapper) 和 UpdateWrapper(LambdaUpdateWrapper) 的父类 用于生成 sql 的 where 条件, entity 属性也用于生成 sql 的 where 条件 注意：entity 生成的 where 条件与 使用各个 api 生成的 where 条件没有任何关联行为

allEq 方法是 MyBatis-Plus 中用于构建查询条件的方法之一，它允许我们通过一个 Map 来设置多个字段的相等条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

带过滤器的普通 Wrapper (QueryWrapper)：

带过滤器的 Lambda Wrapper (LambdaQueryWrapper)：

eq 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的相等条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

ne 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的不相等条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

gt 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的大于条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

ge 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的大于等于条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

lt 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的小于条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

le 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的小于等于条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

between 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的 BETWEEN 条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

notBetween 方法是 MyBatis-Plus 中用于构建查询条件的另一个基本方法，它用于设置单个字段的 NOT BETWEEN 条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

like 方法是 MyBatis-Plus 中用于构建模糊查询条件的基本方法之一，它用于设置单个字段的 LIKE 条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

notLike 方法是 MyBatis-Plus 中用于构建模糊查询条件的另一个基本方法，它用于设置单个字段的 NOT LIKE 条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

likeLeft 方法是 MyBatis-Plus 中用于构建模糊查询条件的基本方法之一，它用于设置单个字段的右模糊匹配条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

通过上述优化，likeLeft 方法的文档更加清晰地展示了其用法、参数说明、示例以及注意事项，使得开发者能够更容易理解和正确使用该方法。

likeRight 方法是 MyBatis-Plus 中用于构建模糊查询条件的基本方法之一，它用于设置单个字段的左模糊匹配条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

notLikeLeft 方法是 MyBatis-Plus 中用于构建模糊查询条件的另一个基本方法，它用于设置单个字段的非右模糊匹配条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

notLikeRight 方法是 MyBatis-Plus 中用于构建模糊查询条件的另一个基本方法，它用于设置单个字段的非左模糊匹配条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

通过上述优化，notLikeRight 方法的文档更加清晰地展示了其用法、参数说明、示例以及注意事项，使得开发者能够更容易理解和正确使用该方法。

isNull 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的 IS NULL 条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

in 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的 IN 条件，即字段的值在给定的集合中。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

notIn 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的 NOT IN 条件，即字段的值不在给定的集合中。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

inSql 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置单个字段的 IN 条件，但与 in 方法不同的是，inSql 允许你直接使用 SQL 语句来生成 IN 子句中的值集合。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

notInSql 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置单个字段的 NOT IN 条件，但与 notIn 方法不同的是，notInSql 允许你直接使用 SQL 语句来生成 NOT IN 子句中的值集合。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

eqSql 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，允许你设置一个字段等于（EQ）某个 SQL 语句的结果。这个方法特别适用于需要将字段值与子查询结果进行比较的场景。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

gtSql 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，允许你设置一个字段大于（GT）某个 SQL 语句的结果。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

geSql 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，允许你设置一个字段大于等于（GE）某个 SQL 语句的结果。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

ltSql 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，允许你设置一个字段小于（LT）某个 SQL 语句的结果。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

leSql 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，允许你设置一个字段小于等于（LE）某个 SQL 语句的结果。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

groupBy 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置查询结果的分组条件。通过指定一个或多个字段，groupBy 方法可以生成 SQL 语句中的 GROUP BY 子句。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

orderByAsc 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置查询结果的升序排序条件。通过指定一个或多个字段，orderByAsc 方法可以生成 SQL 语句中的 ORDER BY 子句，并指定升序排序。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

orderByDesc 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置查询结果的降序排序条件。通过指定一个或多个字段，orderByDesc 方法可以生成 SQL 语句中的 ORDER BY 子句，并指定降序排序。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

orderBy 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置查询结果的排序条件。通过指定一个或多个字段以及排序方向（升序或降序），orderBy 方法可以生成 SQL 语句中的 ORDER BY 子句。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

having 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置 HAVING 子句，通常与 GROUP BY 一起使用，用于对分组后的数据进行条件筛选。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

func 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它提供了一种在链式调用中根据条件执行不同查询操作的机制。通过传入一个 Consumer 函数式接口，func 方法允许你在不中断链式调用的情况下，根据条件执行不同的查询构建逻辑。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

or 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于在查询条件中添加 OR 逻辑。通过调用 or 方法，可以改变后续查询条件的连接方式，从默认的 AND 连接变为 OR 连接。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

and 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于在查询条件中添加 AND 逻辑。通过调用 and 方法，可以创建 AND 嵌套条件，即在一个 AND 逻辑块中包含多个查询条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

nested 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于创建一个独立的查询条件块，不带默认的 AND 或 OR 逻辑。通过调用 nested 方法，可以在查询条件中添加一个嵌套的子句，该子句可以包含多个查询条件，并且可以被外部查询条件通过 AND 或 OR 连接。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

apply 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它允许你直接拼接 SQL 片段到查询条件中。这个方法特别适用于需要使用数据库函数或其他复杂 SQL 构造的场景。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

last 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它允许你直接在查询的最后添加一个 SQL 片段，而不受 MyBatis-Plus 的查询优化规则影响。这个方法应该谨慎使用，因为它可能会绕过 MyBatis-Plus 的查询优化。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

exists 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于在查询中添加一个 EXISTS 子查询。通过调用 exists 方法，可以将一个完整的 SQL 子查询作为 EXISTS 条件添加到主查询中。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

notExists 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于在查询中添加一个 NOT EXISTS 子查询。通过调用 notExists 方法，可以将一个完整的 SQL 子查询作为 NOT EXISTS 条件添加到主查询中。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

select 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置查询的字段。通过调用 select 方法，可以指定在查询结果中包含哪些字段，从而实现字段级别的查询定制。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

使用 Predicate 过滤字段的示例：

set 方法是 MyBatis-Plus 中用于构建更新操作的高级方法之一，它用于设置更新语句中的 SET 字段。通过调用 set 方法，可以指定在更新操作中要修改的字段及其新值。

普通 Wrapper (UpdateWrapper)：

Lambda Wrapper (LambdaUpdateWrapper)：

setSql 方法是 MyBatis-Plus 中用于构建更新操作的高级方法之一，它允许你直接设置更新语句中的 SET 部分 SQL。通过调用 setSql 方法，可以将一个自定义的 SQL 片段作为 SET 子句添加到更新语句中。

setIncrBy 方法是 MyBatis-Plus 中用于更新操作的高级方法之一，它允许你指定一个字段，并使其在数据库中的值增加指定的数值。这个方法特别适用于需要对数值字段进行增量操作的场景。

普通 Wrapper (UpdateWrapper)：

Lambda Wrapper (LambdaUpdateWrapper)：

setDecrBy 方法是 MyBatis-Plus 中用于更新操作的高级方法之一，它允许你指定一个字段，并使其在数据库中的值减少指定的数值。这个方法特别适用于需要对数值字段进行减量操作的场景。

普通 Wrapper (UpdateWrapper)：

Lambda Wrapper (LambdaUpdateWrapper)：

lambda 方法是一个便捷的方法，它允许你从 QueryWrapper 或 UpdateWrapper 对象中获取对应的 LambdaQueryWrapper 或 LambdaUpdateWrapper 对象。这样，你就可以使用 Lambda 表达式来构建查询或更新条件，使得代码更加简洁和类型安全。

从 QueryWrapper 获取 LambdaQueryWrapper：

从 UpdateWrapper 获取 LambdaUpdateWrapper：

在 wrapper 中使用 typeHandler 需要特殊处理利用 formatSqlMaybeWithParam 方法

通过使用 MyBatis-Plus 的 Wrapper 条件构造器，开发者可以更加高效地构建复杂的数据库查询条件，同时保持代码的简洁性和安全性。以下是一些注意事项与推荐做法：

MyBatis-Plus 提供了 Wrappers 类，它是一个静态工厂类，用于快速创建 QueryWrapper、UpdateWrapper、LambdaQueryWrapper 和 LambdaUpdateWrapper 的实例。使用 Wrappers 可以减少代码量，提高开发效率。

Wrapper 实例不是线程安全的，因此建议在每次使用时创建新的 Wrapper 实例。这样可以避免多线程环境下的数据竞争和潜在的错误。

通过遵循这些最佳实践，开发者可以更加安全、高效地使用 MyBatis-Plus 的 Wrapper 条件构造器，构建出既安全又易于维护的数据库操作代码。

MyBatis-Plus 提供了强大的 Wrapper 条件构造器，允许开发者自定义 SQL 语句，以满足更复杂的数据库查询需求。为了使用这一功能，请确保你的 mybatis-plus 版本不低于 3.0.7。

以下是一个使用 Wrapper 自定义 SQL 的示例：

在上述示例中，我们定义了一个 selectByCustomSql 方法，它使用了一个自定义的 SQL 语句，并通过 ${ew.customSqlSegment} 引入了 Wrapper 对象生成的 SQL 片段。

要使用自定义 SQL，只需调用上述方法并传入一个 Wrapper 对象：

在这个例子中，selectByCustomSql 方法将执行一个带有 where 条件的查询，该条件由传入的 queryWrapper 对象生成。

通过这种方式，你可以灵活地结合 MyBatis-Plus 的 Wrapper 功能和自定义 SQL，以满足各种复杂的数据库操作需求。

在Kotlin中定义持久化对象时，我们应当遵循一些最佳实践，以确保代码的清晰性和可维护性。以下是一个使用MyBatis-Plus的示例，展示了如何定义一个持久化对象：

注意：上述代码中的@TableId和@TableField注解是为了展示MyBatis-Plus的使用，并非必须。所有成员变量都应定义为可空类型，并赋予初始值null，以便在类似Java中的updateSelective场景中使用。

不推荐使用data class或全参数构造方法，因为这可能导致在创建空对象时需要提供不必要的null值。

Kotlin支持QueryWrapper和UpdateWrapper，但不支持LambdaQueryWrapper和LambdaUpdateWrapper。如果需要使用Lambda风格的Wrapper，可以使用KtQueryWrapper和KtUpdateWrapper。

MyBatis-Plus提供了两种风格的链式调用：普通链式调用和Lambda式链式调用。需要注意的是，Lambda式链式调用不支持Kotlin。

通过遵循这些最佳实践，我们可以确保Kotlin中的持久化对象定义既清晰又易于维护，同时充分利用MyBatis-Plus提供的功能。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (julia):
```julia
queryWrapper.like(StringUtils.isNotBlank(name), Entity::getName, name)            .eq(age != null && age >= 0, Entity::getAge, age);
```

Example 2 (typescript):
```typescript
// 设置所有字段的相等条件，如果字段值为null，则根据null2IsNull参数决定是否设置为IS NULLallEq(Map<String, Object> params)allEq(Map<String, Object> params, boolean null2IsNull)allEq(boolean condition, Map<String, Object> params, boolean null2IsNull)
// 设置所有字段的相等条件，通过filter过滤器决定哪些字段应该被包含，如果字段值为null，则根据null2IsNull参数决定是否设置为IS NULLallEq(BiPredicate<String, Object> filter, Map<String, Object> params)allEq(BiPredicate<String, Object> filter, Map<String, Object> params, boolean null2IsNull)allEq(boolean condition, BiPredicate<String, Object> filter, Map<String, Object> params, boolean null2IsNull)
```

Example 3 (typescript):
```typescript
QueryWrapper<User> queryWrapper = new QueryWrapper<>();queryWrapper.allEq(Map.of("id", 1, "name", "老王", "age", null));
```

Example 4 (typescript):
```typescript
LambdaQueryWrapper<User> lambdaQueryWrapper = new LambdaQueryWrapper<>();lambdaQueryWrapper.allEq(Map.of("id", 1, "name", "老王", "age", null));
```

---

## 代码生成器配置 | MyBatis-Plus

**URL:** https://baomidou.com/reference/code-generator-configuration/

**Contents:**
- 代码生成器配置
  - 主要特点
  - 使用场景
  - 如何使用
  - 示例代码
- 基本配置
  - 数据源配置（dataSource）
  - 数据库表配置（strategy）
  - 包名配置（packageInfo）
  - 模板配置（template）

MyBatis-Plus 代码生成器是一个强大的工具，它能够根据数据库表结构自动生成对应的实体类、Mapper接口、XML映射文件以及Service层代码。这个工具极大地简化了基于MyBatis框架的开发流程，提高了开发效率，尤其是在需要处理大量数据库表时。

通过MyBatis-Plus代码生成器，开发者可以更加专注于业务逻辑的实现，而不是繁琐的CRUD代码编写，从而提升开发效率和代码质量。

数据源配置用于指定需要生成代码的具体数据库。通过配置数据源，代码生成器能够连接到数据库并获取表结构信息，以便生成相应的代码。

数据库表配置用于指定需要生成哪些表的代码或者排除哪些表。通过策略配置，可以灵活地控制代码生成的范围。

包名配置用于指定生成代码的包路径。通过配置包名，可以确保生成的代码放置在正确的目录结构中。

模板配置允许自定义代码生成的模板，实现个性化操作。通过模板配置，可以定制生成代码的格式和内容。

全局策略配置提供了一些全局的设置，如作者信息、生成路径等。

注入配置允许注入自定义参数等操作以实现个性化操作。通过注入配置，可以在代码生成过程中添加额外的逻辑。

通过以上配置，MyBatis-Plus 代码生成器可以根据你的需求生成符合项目结构的代码，大大提高了开发效率。记得根据实际项目需求调整配置参数，以达到最佳的代码生成效果。

数据源配置是 MyBatis-Plus 代码生成器的关键部分，它定义了如何连接到数据库以及如何查询数据库信息。以下是数据源配置的详细说明和示例。

在这个示例中，我们配置了一个 MySQL 数据库的数据源。我们指定了数据库类型、连接 URL、用户名、密码和驱动名称。这些信息将用于建立与数据库的连接，并从中获取表结构信息，以便生成相应的 Java 代码。

请根据你的实际数据库配置调整这些参数，确保它们与你的数据库环境相匹配。

数据库表配置用于定义生成代码时如何处理数据库表和字段。通过策略配置，可以指定生成哪些表的代码、如何命名实体类和字段、以及是否包含特定的注解或属性。

在这个示例中，我们配置了一个策略，指定了大写命名模式、使用 Lombok 模型、生成 REST 风格的控制器，并指定了需要包含和排除的表。我们还设置了表前缀和驼峰转连字符的控制器映射风格。

请根据你的项目需求调整这些配置参数，以确保生成的代码符合你的期望。

包名配置用于定义生成代码的包结构，确保生成的代码放置在正确的目录中。通过配置包名，可以控制代码的组织方式，使其符合项目的架构设计。

在这个示例中，我们配置了一个包结构，其中父包名为 com.example，每个子包名都根据其功能进行了设置。例如，实体类将放置在 com.example.mybatisplus.entity 包中，服务接口将放置在 com.example.mybatisplus.service 包中，以此类推。

请根据你的项目结构和组织习惯调整这些配置参数，以确保生成的代码能够正确地集成到你的项目中。

模板配置允许开发者自定义代码生成器使用的模板，以生成符合特定项目风格和需求的代码。MyBatis-Plus 代码生成器支持多种类型的模板，包括实体类、服务类、Mapper 接口、XML 映射文件和控制器类等。

在这个示例中，我们配置了不同类型的模板路径。例如，实体类模板路径设置为 templates/entity.java.vm，服务类模板路径设置为 templates/service.java.vm，以此类推。这些模板路径指向了项目中自定义的模板文件，代码生成器将使用这些模板来生成相应的代码。

请确保你的模板文件路径正确，并且模板文件遵循 Velocity 或 Freemarker 等模板引擎的语法。通过自定义模板，你可以控制生成的代码的结构、注释、命名风格等，以满足项目的特定需求。

在实际使用中，你可能需要根据项目的具体情况调整模板配置，例如，如果你的项目使用 Kotlin 语言，则需要配置 entityKt 模板路径。如果你的项目不需要生成某些类型的代码（如 XML 映射文件），则可以不配置相应的模板。

全局策略配置提供了一些全局的设置，如输出目录、文件覆盖、开发者信息等，以及一些高级选项，如 Kotlin 模式、Swagger2 集成、ActiveRecord 模式等。

在这个示例中，我们配置了全局策略，指定了输出目录、文件覆盖、开发者信息等，并设置了各种命名方式和主键ID类型。这些配置将影响生成的代码的结构和内容。

请根据你的项目需求和偏好调整这些配置参数，以确保生成的代码符合你的期望。例如，如果你希望生成的实体类名以 Entity 结尾，可以将 entityName 设置为 %sEntity。如果你希望在生成的 XML 文件中包含二级缓存配置，可以将 enableCache 设置为 true。

注入配置允许开发者自定义代码生成器的行为，包括自定义返回配置、自定义输出文件、自定义文件创建逻辑等。这些配置提供了灵活性，使得代码生成器能够适应更复杂的项目需求。

在这个示例中，我们配置了注入配置，包括自定义 Map 对象、自定义输出文件配置和自定义文件创建逻辑。

请根据你的项目需求调整这些配置参数，以确保生成的代码符合你的期望。例如，如果你需要在模板中访问额外的配置信息，可以在 initMap 方法中添加这些信息。如果你需要生成特定格式的文件，可以在 getFileOutConfig 方法中指定相应的模板和输出路径。如果你需要自定义文件创建逻辑，可以在 getFileCreate 方法中实现相应的判断逻辑。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (java):
```java
import com.baomidou.mybatisplus.generator.AutoGenerator;import com.baomidou.mybatisplus.generator.config.*;import com.baomidou.mybatisplus.generator.config.rules.DbType;import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
public class CodeGenerator {
    public static void main(String[] args) {        // 全局配置        GlobalConfig globalConfig = new GlobalConfig();        globalConfig.setOutputDir(System.getProperty("user.dir") + "/src/main/java") // 设置输出目录                .setAuthor("Your Name") // 设置作者                .setOpen(false) // 设置生成后是否自动打开目录                .setFileOverride(true) // 设置文件存在时是否覆盖                .setServiceName("%sService") // 设置Service接口名后缀                .setIdType(IdType.AUTO) // 设置主键生成策略                .setSwagger2(true); // 设置是否生成Swagger注解
        // 数据源配置        DataSourceConfig dataSourceConfig = new DataSourceConfig();        dataSourceConfig.setDbType(DbType.MYSQL) // 设置数据库类型                .setUrl("jdbc:mysql://localhost:3306/mybatis_plus?useSSL=false&serverTimezone=UTC") // 数据库连接URL                .setUsername("root") // 数据库用户名                .setPassword("password") // 数据库密码                .setDriverName("com.mysql.cj.jdbc.Driver"); // 数据库驱动类名
        // 策略配置        StrategyConfig strategyConfig = new StrategyConfig();        strategyConfig.setInclude("user", "order") // 指定需要生成代码的表名                .setNaming(NamingStrategy.underline_to_camel) // 设置表名转类名策略                .setColumnNaming(NamingStrategy.underline_to_camel) // 设置列名转属性名策略                .setEntityLombokModel(true) // 设置实体类使用Lombok模型                .setRestControllerStyle(true) // 设置Controller使用REST风格                .setTablePrefix(new String[]{"tbl_"}); // 设置表名前缀
        // 包配置        PackageConfig packageConfig = new PackageConfig();        packageConfig.setParent("com.example") // 设置父包名                .setMapper("mapper") // 设置Mapper接口所在的子包名                .setEntity("entity") // 设置实体类所在的子包名                .setController("controller") // 设置Controller所在的子包名                .setService("service") // 设置Service所在的子包名                .setXml("mapper"); // 设置Mapper XML文件所在的子包名
        // 模板配置        TemplateConfig templateConfig = new TemplateConfig();        templateConfig.setXml(null) // 不生成XML文件                .setController("templates/controller.java.vm") // 设置Controller模板路径                .setEntity("templates/entity.java.vm") // 设置实体类模板路径                .setMapper("templates/mapper.java.vm"); // 设置Mapper接口模板路径
        // 整合配置        AutoGenerator autoGenerator = new AutoGenerator();        autoGenerator.setGlobalConfig(globalConfig)                .setDataSource(dataSourceConfig)                .setStrategy(strategyConfig)                .setPackageInfo(packageConfig)                .setTemplate(templateConfig);
        // 执行生成        autoGenerator.execute();    }}
```

Example 2 (json):
```json
DataSourceConfig dataSourceConfig = new DataSourceConfig();dataSourceConfig.setDbType(DbType.MYSQL) // 设置数据库类型，如MySQL、Oracle等    .setUrl("jdbc:mysql://localhost:3306/mybatis_plus") // 数据库连接URL    .setUsername("root") // 数据库用户名    .setPassword("password") // 数据库密码    .setDriverName("com.mysql.cj.jdbc.Driver"); // 数据库驱动类名
```

Example 3 (unknown):
```unknown
StrategyConfig strategyConfig = new StrategyConfig();strategyConfig.setInclude("user", "order") // 指定需要生成代码的表名    .setExclude("user_detail") // 排除不需要生成代码的表名    .setEntityLombokModel(true) // 设置实体类使用Lombok模型    .setRestControllerStyle(true); // 设置Controller使用REST风格
```

Example 4 (unknown):
```unknown
PackageConfig packageConfig = new PackageConfig();packageConfig.setParent("com.example") // 设置父包名    .setMapper("mapper") // 设置Mapper接口所在的子包名    .setEntity("entity") // 设置实体类所在的子包名    .setController("controller"); // 设置Controller所在的子包名
```

---

## 条件构造器 | MyBatis-Plus

**URL:** https://baomidou.com/guides/wrapper

**Contents:**
- 条件构造器
- 功能详解
  - allEq
    - 使用范围
    - 方法签名
    - 参数说明
    - 示例
  - eq
    - 使用范围
    - 方法签名

MyBatis-Plus 提供了一套强大的条件构造器（Wrapper），用于构建复杂的数据库查询条件。Wrapper 类允许开发者以链式调用的方式构造查询条件，无需编写繁琐的 SQL 语句，从而提高开发效率并减少 SQL 注入的风险。

在 MyBatis-Plus 中，Wrapper 类是构建查询和更新条件的核心工具。以下是主要的 Wrapper 类及其功能：

AbstractWrapper：这是一个抽象基类，提供了所有 Wrapper 类共有的方法和属性。它定义了条件构造的基本逻辑，包括字段（column）、值（value）、操作符（condition）等。所有的 QueryWrapper、UpdateWrapper、LambdaQueryWrapper 和 LambdaUpdateWrapper 都继承自 AbstractWrapper。

QueryWrapper：专门用于构造查询条件，支持基本的等于、不等于、大于、小于等各种常见操作。它允许你以链式调用的方式添加多个查询条件，并且可以组合使用 and 和 or 逻辑。

UpdateWrapper：用于构造更新条件，可以在更新数据时指定条件。与 QueryWrapper 类似，它也支持链式调用和逻辑组合。使用 UpdateWrapper 可以在不创建实体对象的情况下，直接设置更新字段和条件。

LambdaQueryWrapper：这是一个基于 Lambda 表达式的查询条件构造器，它通过 Lambda 表达式来引用实体类的属性，从而避免了硬编码字段名。这种方式提高了代码的可读性和可维护性，尤其是在字段名可能发生变化的情况下。

LambdaUpdateWrapper：类似于 LambdaQueryWrapper，LambdaUpdateWrapper 是基于 Lambda 表达式的更新条件构造器。它允许你使用 Lambda 表达式来指定更新字段和条件，同样避免了硬编码字段名的问题。

MyBatis-Plus 的 Wrapper 类是构建复杂查询和更新条件的关键工具。它允许开发者以链式调用的方式构造 SQL 的 WHERE 子句，提供了极大的灵活性和便利性。

以下是对 Wrapper 功能的提示和注意事项。

条件判断：Wrapper 方法通常接受一个 boolean 类型的参数，用于决定是否将该条件加入到最终的 SQL 中。例如：

默认行为：如果某个方法没有显式提供 boolean 类型的参数，则默认为 true，即条件总是会被加入到 SQL 中。

泛型参数：Wrapper 类是泛型类，其中 Param 通常指的是 Wrapper 的子类实例，如 QueryWrapper、UpdateWrapper 等。

字段引用：在 LambdaWrapper 中，R 代表的是一个函数，用于引用实体类的属性，例如 Entity::getId。而在普通 Wrapper 中，R 代表的是数据库字段名。

字段名注意事项：当 R 具体类型为 String 时，表示的是数据库字段名，而不是实体类数据字段名。如果字段名是数据库关键字，需要使用转义符包裹。

集合参数：如果方法的参数是 Map 或 List，当它们为空时，对应的 SQL 条件不会被加入到最终的 SQL 中。

学习资源：对于不熟悉的函数式编程概念，可以参考学习资源进行学习。

RPC 调用中的 Wrapper：不支持也不赞成在 RPC 调用中传输 Wrapper 对象。Wrapper 对象通常包含大量信息，不适合作为传输对象。正确的做法是定义一个 DTO（数据传输对象）进行传输，然后在被调用方根据 DTO 执行相应的操作。

维护性：避免在 Controller 层使用 Map 接收值，这种做法虽然开发时方便，但会给后续的维护带来困难。

问题反馈：不接受任何关于 RPC 传输 Wrapper 报错相关的 issue 或 pr。

安全性： QueryWrapper UpdateWrapper 字段部分，如有允许 前端传入 SQL 片段 这可能会导致 SQL 注入风险 需要校验，更多查看 预防安全漏洞。

QueryWrapper(LambdaQueryWrapper) 和 UpdateWrapper(LambdaUpdateWrapper) 的父类 用于生成 sql 的 where 条件, entity 属性也用于生成 sql 的 where 条件 注意：entity 生成的 where 条件与 使用各个 api 生成的 where 条件没有任何关联行为

allEq 方法是 MyBatis-Plus 中用于构建查询条件的方法之一，它允许我们通过一个 Map 来设置多个字段的相等条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

带过滤器的普通 Wrapper (QueryWrapper)：

带过滤器的 Lambda Wrapper (LambdaQueryWrapper)：

eq 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的相等条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

ne 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的不相等条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

gt 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的大于条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

ge 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的大于等于条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

lt 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的小于条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

le 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的小于等于条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

between 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的 BETWEEN 条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

notBetween 方法是 MyBatis-Plus 中用于构建查询条件的另一个基本方法，它用于设置单个字段的 NOT BETWEEN 条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

like 方法是 MyBatis-Plus 中用于构建模糊查询条件的基本方法之一，它用于设置单个字段的 LIKE 条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

notLike 方法是 MyBatis-Plus 中用于构建模糊查询条件的另一个基本方法，它用于设置单个字段的 NOT LIKE 条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

likeLeft 方法是 MyBatis-Plus 中用于构建模糊查询条件的基本方法之一，它用于设置单个字段的右模糊匹配条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

通过上述优化，likeLeft 方法的文档更加清晰地展示了其用法、参数说明、示例以及注意事项，使得开发者能够更容易理解和正确使用该方法。

likeRight 方法是 MyBatis-Plus 中用于构建模糊查询条件的基本方法之一，它用于设置单个字段的左模糊匹配条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

notLikeLeft 方法是 MyBatis-Plus 中用于构建模糊查询条件的另一个基本方法，它用于设置单个字段的非右模糊匹配条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

notLikeRight 方法是 MyBatis-Plus 中用于构建模糊查询条件的另一个基本方法，它用于设置单个字段的非左模糊匹配条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

通过上述优化，notLikeRight 方法的文档更加清晰地展示了其用法、参数说明、示例以及注意事项，使得开发者能够更容易理解和正确使用该方法。

isNull 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的 IS NULL 条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

in 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的 IN 条件，即字段的值在给定的集合中。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

notIn 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于设置单个字段的 NOT IN 条件，即字段的值不在给定的集合中。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

inSql 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置单个字段的 IN 条件，但与 in 方法不同的是，inSql 允许你直接使用 SQL 语句来生成 IN 子句中的值集合。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

notInSql 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置单个字段的 NOT IN 条件，但与 notIn 方法不同的是，notInSql 允许你直接使用 SQL 语句来生成 NOT IN 子句中的值集合。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

eqSql 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，允许你设置一个字段等于（EQ）某个 SQL 语句的结果。这个方法特别适用于需要将字段值与子查询结果进行比较的场景。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

gtSql 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，允许你设置一个字段大于（GT）某个 SQL 语句的结果。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

geSql 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，允许你设置一个字段大于等于（GE）某个 SQL 语句的结果。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

ltSql 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，允许你设置一个字段小于（LT）某个 SQL 语句的结果。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

leSql 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，允许你设置一个字段小于等于（LE）某个 SQL 语句的结果。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

groupBy 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置查询结果的分组条件。通过指定一个或多个字段，groupBy 方法可以生成 SQL 语句中的 GROUP BY 子句。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

orderByAsc 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置查询结果的升序排序条件。通过指定一个或多个字段，orderByAsc 方法可以生成 SQL 语句中的 ORDER BY 子句，并指定升序排序。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

orderByDesc 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置查询结果的降序排序条件。通过指定一个或多个字段，orderByDesc 方法可以生成 SQL 语句中的 ORDER BY 子句，并指定降序排序。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

orderBy 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置查询结果的排序条件。通过指定一个或多个字段以及排序方向（升序或降序），orderBy 方法可以生成 SQL 语句中的 ORDER BY 子句。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

having 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置 HAVING 子句，通常与 GROUP BY 一起使用，用于对分组后的数据进行条件筛选。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

func 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它提供了一种在链式调用中根据条件执行不同查询操作的机制。通过传入一个 Consumer 函数式接口，func 方法允许你在不中断链式调用的情况下，根据条件执行不同的查询构建逻辑。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

or 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于在查询条件中添加 OR 逻辑。通过调用 or 方法，可以改变后续查询条件的连接方式，从默认的 AND 连接变为 OR 连接。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

and 方法是 MyBatis-Plus 中用于构建查询条件的基本方法之一，它用于在查询条件中添加 AND 逻辑。通过调用 and 方法，可以创建 AND 嵌套条件，即在一个 AND 逻辑块中包含多个查询条件。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

nested 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于创建一个独立的查询条件块，不带默认的 AND 或 OR 逻辑。通过调用 nested 方法，可以在查询条件中添加一个嵌套的子句，该子句可以包含多个查询条件，并且可以被外部查询条件通过 AND 或 OR 连接。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

apply 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它允许你直接拼接 SQL 片段到查询条件中。这个方法特别适用于需要使用数据库函数或其他复杂 SQL 构造的场景。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

last 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它允许你直接在查询的最后添加一个 SQL 片段，而不受 MyBatis-Plus 的查询优化规则影响。这个方法应该谨慎使用，因为它可能会绕过 MyBatis-Plus 的查询优化。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

exists 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于在查询中添加一个 EXISTS 子查询。通过调用 exists 方法，可以将一个完整的 SQL 子查询作为 EXISTS 条件添加到主查询中。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

notExists 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于在查询中添加一个 NOT EXISTS 子查询。通过调用 notExists 方法，可以将一个完整的 SQL 子查询作为 NOT EXISTS 条件添加到主查询中。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

select 方法是 MyBatis-Plus 中用于构建查询条件的高级方法之一，它用于设置查询的字段。通过调用 select 方法，可以指定在查询结果中包含哪些字段，从而实现字段级别的查询定制。

普通 Wrapper (QueryWrapper)：

Lambda Wrapper (LambdaQueryWrapper)：

使用 Predicate 过滤字段的示例：

set 方法是 MyBatis-Plus 中用于构建更新操作的高级方法之一，它用于设置更新语句中的 SET 字段。通过调用 set 方法，可以指定在更新操作中要修改的字段及其新值。

普通 Wrapper (UpdateWrapper)：

Lambda Wrapper (LambdaUpdateWrapper)：

setSql 方法是 MyBatis-Plus 中用于构建更新操作的高级方法之一，它允许你直接设置更新语句中的 SET 部分 SQL。通过调用 setSql 方法，可以将一个自定义的 SQL 片段作为 SET 子句添加到更新语句中。

setIncrBy 方法是 MyBatis-Plus 中用于更新操作的高级方法之一，它允许你指定一个字段，并使其在数据库中的值增加指定的数值。这个方法特别适用于需要对数值字段进行增量操作的场景。

普通 Wrapper (UpdateWrapper)：

Lambda Wrapper (LambdaUpdateWrapper)：

setDecrBy 方法是 MyBatis-Plus 中用于更新操作的高级方法之一，它允许你指定一个字段，并使其在数据库中的值减少指定的数值。这个方法特别适用于需要对数值字段进行减量操作的场景。

普通 Wrapper (UpdateWrapper)：

Lambda Wrapper (LambdaUpdateWrapper)：

lambda 方法是一个便捷的方法，它允许你从 QueryWrapper 或 UpdateWrapper 对象中获取对应的 LambdaQueryWrapper 或 LambdaUpdateWrapper 对象。这样，你就可以使用 Lambda 表达式来构建查询或更新条件，使得代码更加简洁和类型安全。

从 QueryWrapper 获取 LambdaQueryWrapper：

从 UpdateWrapper 获取 LambdaUpdateWrapper：

在 wrapper 中使用 typeHandler 需要特殊处理利用 formatSqlMaybeWithParam 方法

通过使用 MyBatis-Plus 的 Wrapper 条件构造器，开发者可以更加高效地构建复杂的数据库查询条件，同时保持代码的简洁性和安全性。以下是一些注意事项与推荐做法：

MyBatis-Plus 提供了 Wrappers 类，它是一个静态工厂类，用于快速创建 QueryWrapper、UpdateWrapper、LambdaQueryWrapper 和 LambdaUpdateWrapper 的实例。使用 Wrappers 可以减少代码量，提高开发效率。

Wrapper 实例不是线程安全的，因此建议在每次使用时创建新的 Wrapper 实例。这样可以避免多线程环境下的数据竞争和潜在的错误。

通过遵循这些最佳实践，开发者可以更加安全、高效地使用 MyBatis-Plus 的 Wrapper 条件构造器，构建出既安全又易于维护的数据库操作代码。

MyBatis-Plus 提供了强大的 Wrapper 条件构造器，允许开发者自定义 SQL 语句，以满足更复杂的数据库查询需求。为了使用这一功能，请确保你的 mybatis-plus 版本不低于 3.0.7。

以下是一个使用 Wrapper 自定义 SQL 的示例：

在上述示例中，我们定义了一个 selectByCustomSql 方法，它使用了一个自定义的 SQL 语句，并通过 ${ew.customSqlSegment} 引入了 Wrapper 对象生成的 SQL 片段。

要使用自定义 SQL，只需调用上述方法并传入一个 Wrapper 对象：

在这个例子中，selectByCustomSql 方法将执行一个带有 where 条件的查询，该条件由传入的 queryWrapper 对象生成。

通过这种方式，你可以灵活地结合 MyBatis-Plus 的 Wrapper 功能和自定义 SQL，以满足各种复杂的数据库操作需求。

在Kotlin中定义持久化对象时，我们应当遵循一些最佳实践，以确保代码的清晰性和可维护性。以下是一个使用MyBatis-Plus的示例，展示了如何定义一个持久化对象：

注意：上述代码中的@TableId和@TableField注解是为了展示MyBatis-Plus的使用，并非必须。所有成员变量都应定义为可空类型，并赋予初始值null，以便在类似Java中的updateSelective场景中使用。

不推荐使用data class或全参数构造方法，因为这可能导致在创建空对象时需要提供不必要的null值。

Kotlin支持QueryWrapper和UpdateWrapper，但不支持LambdaQueryWrapper和LambdaUpdateWrapper。如果需要使用Lambda风格的Wrapper，可以使用KtQueryWrapper和KtUpdateWrapper。

MyBatis-Plus提供了两种风格的链式调用：普通链式调用和Lambda式链式调用。需要注意的是，Lambda式链式调用不支持Kotlin。

通过遵循这些最佳实践，我们可以确保Kotlin中的持久化对象定义既清晰又易于维护，同时充分利用MyBatis-Plus提供的功能。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (julia):
```julia
queryWrapper.like(StringUtils.isNotBlank(name), Entity::getName, name)            .eq(age != null && age >= 0, Entity::getAge, age);
```

Example 2 (typescript):
```typescript
// 设置所有字段的相等条件，如果字段值为null，则根据null2IsNull参数决定是否设置为IS NULLallEq(Map<String, Object> params)allEq(Map<String, Object> params, boolean null2IsNull)allEq(boolean condition, Map<String, Object> params, boolean null2IsNull)
// 设置所有字段的相等条件，通过filter过滤器决定哪些字段应该被包含，如果字段值为null，则根据null2IsNull参数决定是否设置为IS NULLallEq(BiPredicate<String, Object> filter, Map<String, Object> params)allEq(BiPredicate<String, Object> filter, Map<String, Object> params, boolean null2IsNull)allEq(boolean condition, BiPredicate<String, Object> filter, Map<String, Object> params, boolean null2IsNull)
```

Example 3 (typescript):
```typescript
QueryWrapper<User> queryWrapper = new QueryWrapper<>();queryWrapper.allEq(Map.of("id", 1, "name", "老王", "age", null));
```

Example 4 (typescript):
```typescript
LambdaQueryWrapper<User> lambdaQueryWrapper = new LambdaQueryWrapper<>();lambdaQueryWrapper.allEq(Map.of("id", 1, "name", "老王", "age", null));
```

---
