# Mybatis-Plus - Configuration

**Pages:** 11

---

## 分页插件 | MyBatis-Plus

**URL:** https://baomidou.com/plugins/pagination/

**Contents:**
- 分页插件
- 支持的数据库
- 配置方法
- 属性介绍
- 自定义 Mapper 方法中使用分页
- 其他注意事项
- Page 类

MyBatis-Plus 的分页插件 PaginationInnerInterceptor 提供了强大的分页功能，支持多种数据库，使得分页查询变得简单高效。

于 v3.5.9 起，PaginationInnerInterceptor 已分离出来。如需使用，则需单独引入 mybatis-plus-jsqlparser 依赖 ， 具体请查看 安装 一章。

PaginationInnerInterceptor 支持广泛的数据库，包括但不限于：

如果你需要支持的数据库不在列表中，可以通过 Pull Request 请求添加。

在 Spring Boot 项目中，你可以通过 Java 配置来添加分页插件：

PaginationInnerInterceptor 提供了以下属性来定制分页行为：

你可以通过以下方式在 Mapper 方法中使用分页：

如果返回类型是 IPage，则入参的 IPage 不能为 null。如果想临时不分页，可以在初始化 IPage 时 size 参数传入小于 0 的值。 如果返回类型是 List，则入参的 IPage 可以为 null，但需要手动设置入参的 IPage.setRecords(返回的 List)。 如果 XML 需要从 page 里取值，需要使用 page.属性 获取。

Page 类继承了 IPage 类，实现了简单分页模型。如果你需要实现自己的分页模型，可以继承 Page 类或实现 IPage 类。

通过这些配置和使用方法，你可以轻松地在 MyBatis-Plus 中实现分页查询，提高应用的性能和用户体验。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (java):
```java
@Configuration@MapperScan("scan.your.mapper.package")public class MybatisPlusConfig {
    /**     * 添加分页插件     */    @Bean    public MybatisPlusInterceptor mybatisPlusInterceptor() {        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL)); // 如果配置多个插件, 切记分页最后添加        // 如果有多数据源可以不配具体类型, 否则都建议配上具体的 DbType        return interceptor;    }}
```

Example 2 (typescript):
```typescript
IPage<UserVo> selectPageVo(IPage<?> page, Integer state);// 或者自定义分页类MyPage selectPageVo(MyPage page);// 或者返回 ListList<UserVo> selectPageVo(IPage<UserVo> page, Integer state);
```

Example 3 (sql):
```sql
<select id="selectPageVo" resultType="xxx.xxx.xxx.UserVo">    SELECT id,name FROM user WHERE state=#{state}</select>
```

---

## 常见问题 | MyBatis-Plus

**URL:** https://baomidou.com/reference/question/

**Contents:**
- 常见问题
- 如何排除非表中字段？
- 如何排除实体父类属性？
- 出现 Invalid bound statement (not found) 异常
- 自定义 SQL 无法执行
- 启动异常时异常问题排查
- 关于 Long 型主键填充不生效的问题
- 生成主键太长导致 JS 精度丢失
- 插入或更新的字段有 空字符串 或者 null
- 字段类型为 bit、tinyint(1) 时映射为 boolean 类型

本文档收录了 MyBatis-Plus 使用时遇到的各种常见问题，如果您在使用 MyBatis-Plus 的时候遇到了问题，请您优先查看本文档。

使用 transient 修饰需要排除的父类属性

出现该异常通常是由于配置不正确或者 Mapper 没有被正确扫描到导致的。解决方案如下：

检查 Mapper.java 的扫描路径：

方法一：在 Configuration 类上使用注解 MapperScan

方法二：在 Configuration 类中配置 MapperScannerConfigurer（查看示例）

检查是否指定了主键。如果未指定，将导致 selectById 相关 ID 无法操作。请使用注解 @TableId 注解表 ID 主键。当然，@TableId 注解可以省略，但是你的主键必须叫 id（忽略大小写）。

不要使用原生的 SqlSessionFactory，请使用 MybatisSqlSessionFactory。

检查是否自定义了 SqlInjector，是否复写了 getMethodList() 方法。在该方法中是否注入了你需要的方法（可参考 DefaultSqlInjector）。

IDEA 默认的 build 步骤可能会导致 mapper 文件无法正常编译到对应的 resources 文件夹中。请检查 build 后相关资源文件夹是否有对应的 XML 文件。如果没有，请调整 IDEA 的 build 设置。推荐调整为 Maven 或 Gradle 的 build。

问题描述：指在 XML 中里面自定义 SQL，却无法调用。本功能同 MyBatis 一样需要配置 XML 扫描路径：

对于IDEA系列编辑器，XML 文件是不能放在 java 文件夹中的，IDEA 默认不会编译源码文件夹中的 XML 文件，可以参照以下方式解决：

注意！Maven 多模块项目的扫描路径需以 classpath*: 开头 （即加载多个 jar 包下的 XML 文件）

MapperScan 需要排除 com.baomidou.mybatisplus.mapper.BaseMapper 类 及其 子类（自定义公共 Mapper），比如：

原因：低版本不支持泛型注入，请升级 Spring 版本到 4+ 以上。

版本引入问题：3.4.1 版本里没有，3.4.2 里面才有！

long类型默认值为 0，而 MP 只会判断是否为 null

JavaScript 无法处理 Java 的长整型 Long 导致精度丢失，具体表现为主键最后两位永远为 0，解决思路： Long 转为 String 返回

比较一般的处理方式：增加一个 public String getIdStr() 方法，前台获取 idStr

当用户有更新字段为 空字符串 或者 null 的需求时，需要对 FieldStrategy 策略进行调整：

注入配置 GlobalConfiguration 属性 fieldStrategy

根据具体情况，在需要更新的字段中调整验证注解，如验证非空：

方式三：使用 UpdateWrapper (3.x)

默认mysql驱动会把tinyint(1)字段映射为boolean: 0=false, 非0=true

MyBatis 是不会自动处理该映射，如果不想把tinyint(1)映射为boolean类型:

原因：配了 2 个分页拦截器! 检查配置文件或者代码，只留一个！

insert 后主键会自动 set 到实体的 ID 字段，所以你只需要 getId() 就好

EntityWrapper.sqlSelect 配置你想要查询的字段

我们建议缓存放到 service 层，你可以自定义自己的 BaseServiceImpl 重写注解父类方法，继承自己的实现。

如果你按照 mybatis 的方式配置第三方二级缓存，并且使用 2.0.9 以上的版本，则会发现自带的方法无法更新缓存内容，那么请按如下方式解决（二选一）：

1.在代码中 mybatis 的 mapper 层添加缓存注释，声明 implementation 或 eviction 的值为 cache 接口的实现类

2.在对应的 mapper.xml 中将原有注释修改为链接式声明，以保证 xml 文件里的缓存能够正常

在 MyBatis-Plus 中, jdbcTypeForNull 是一个重要的配置选项, 它决定了如何将Java的 null 值映射到数据库中.

默认情况下, 当某个字段在 Java 中为 null 时, MyBatis-Plus 会将其映射为数据库中的 JdbcType.OTHER 值。但某些 JDBC 驱动程序（例如 Oracle） 不支持 JdbcType.OTHER ，因此默认设置会导致错误: Cause: org.apache.ibatis.type.TypeException:Error setting null for parameter #1 with JdbcType OTHER . Try setting a different JdbcType for this parameter or a different jdbcTypeForNull configuration property. Cause: java.sql.SQLException: 无效的列类型: 1111.

要设置 jdbcTypeForNull , 你需要在 MyBatis-Plus 的配置类中添加以下代码（任选其一即可）:

需要注意的是, 如果你在项目中自定义 SqlSessionFactory 可能导致 jdbc-type-for-null 配置失效.

在自定义 SQL 中，由于 Page 对象继承自 RowBounds，在 Mapper 中无法直接获取。为了解决这个问题，请考虑以下替代方案：

这些方法可以帮助您在自定义 SQL 中正确传递参数，确保代码的顺利运行。

当使用 resultType="java.util.Map" 时，您可以通过以下步骤在 Spring Boot 中实现下划线自动转换为驼峰：

在您的 Spring Boot 项目中创建一个配置类。

通过这样配置，您就可以实现自动将 Map 中的下划线转换为驼峰形式。这样，在 MyBatis 查询结果映射到 Map 对象时，键名会自动进行转换，使得您在代码中更加便捷地访问数据。

您可以通过以下方式在 Wrapper 中使用 limit 限制 SQL 结果集：

这段代码会在 SQL 语句末尾添加 limit 1，以限制结果集返回的行数为 1。

将通用的批量插入操作放在 Service 层处理有以下原因：

如果您想使用单条 SQL 插入方案，可以自行注入选装方法 insertbatchsomecolumn，或查看SQL 注入器中提供的方法。

在 MyBatis Plus 3.x 中，不再提供自动识别关键字进行处理的功能。处理数据库关键字的方法有以下几种：

不同数据库对关键字的处理方式不同，因此很难维护。在数据库设计时，建议避免使用关键字作为字段名或表名。

如果必须使用关键字，可以通过在字段或表名前后添加反引号（`）来进行处理，如下所示：

综上所述，为了避免出现问题，建议尽量避免在数据库设计中使用关键字。

针对 MyBatis Plus 3.1.1 及更高版本出现的问题：

现象：在单元测试中没有问题，但是在启动服务器进行调试时出现该异常。

原因：在 3.1.1 版本及以后的版本中，针对字段缓存进行了优化，使用 .class 作为键来替换了原来的类名（className）。然而，当使用 dev-tools 时，可能会导致 .class 使用不同的类加载器加载，从而导致出现找不到属性的情况。

解决方案：移除 dev-tools 插件。这样可以避免使用不同的类加载器加载 .class，从而解决该异常问题。

针对 MyBatis Plus 3.1.1 及更高版本出现的问题：

现象：在集成 Druid 数据源时，升级到 3.1.1 版本及之后的版本后，出现错误：java.sql.SQLFeatureNotSupportedException。而在 3.1.0 版本之前没有此问题。

原因：MyBatis Plus 3.1.1 版本及更高版本采用了新版 JDBC，对于新的日期类型（如 LocalDateTime）处理方式进行了升级。然而，Druid 在 1.1.21 版本之前不支持此特性，导致出现此异常。详细信息请参考相关问题。

如果您将 MyBatis Plus 从 3.1.0 及以下版本升级到较高版本，且遇到新日期类型（如 LocalDateTime）无法映射的报错，可能是由于以下原因：

MP_3.1.0 及之前版本依赖的是 MyBatis 3.5.0。而 MP_3.1.1 升级了 MyBatis 的依赖到 3.5.1，而在 MyBatis 3.5.1 中，新日期类型需要 JDBC 驱动支持 JDBC 4.2 API。

如果您的 JDBC 驱动版本不支持 JDBC 4.2 API，就会出现无法映射新日期类型的报错。

如果您在将 Spring Boot 版本从 2.2.0 升级到更高版本时遇到此问题，可能是由于以下原因：

现象：在本地启动时没有问题，但是将打成 war 包部署到服务器时出现此问题。

原因：在 Spring Boot 2.2.0 中存在构造器注入的问题，导致 MyBatis 的私有构造器无法正确绑定属性，进而导致依赖 MyBatis 的框架（如 MyBatis Plus）报错。详细信息请参考 相关 issue。此问题已在 Spring Boot 2.2.1 中得到修复。

现象：在开发工具中运行没有问题，但是将项目打包部署到服务器后，执行 Lambda 表达式时出现 ClassNotFoundException。

针对 MyBatis Plus 3.3.2 以下版本，如果在分离打包部署时出现 ClassNotFoundException 的问题，可能是因为在执行反序列化操作时，类加载器发生了错误。

您可以通过以下两种方式来启用 MyBatis 内部的日志记录：

方式一： 在您的 application.yml 或 application.properties 文件中添加以下配置：

这将使用 MyBatis 内置的 StdOutImpl 日志记录实现将日志输出到控制台。

方式二： 在您的 application.yml 或 application.properties 文件中增加日志级别配置，以指定特定包的日志级别。例如：

这将指定 com.baomidou.example.mapper 包下的日志级别为 debug。您可以根据需要调整级别。

通过以上配置，您可以启用 MyBatis 内部的日志记录，并根据需要调整日志级别。

如果您想要在更新操作时对某个字段进行自增操作，您可以使用 MyBatis Plus 提供的 Wrapper 进行更新。以下是一种可行的解决方案：

这样可以在更新时对字段进行自增操作。请注意，需要在 setSql 方法中直接指定 SQL 更新语句。

如果您想要全局处理数据库关键词，可以使用 MyBatis Plus 提供的全局配置。以下是配置示例（以 MySQL 为例）：

这样配置后，MyBatis Plus 将会在生成 SQL 语句时，对数据库字段名称使用反引号（“）进行包裹，以确保不与数据库关键词冲突。

通过以上配置，您可以全局处理数据库关键词，确保生成的 SQL 语句不会受到关键词影响。

如果您想要在 XML 中根据数据库类型选择不同的 SQL 片段，您可以使用 MyBatis Plus 提供的 database-id 参数。以下是配置示例（以 MySQL 为例）：

这样配置后，MyBatis Plus 将会在执行 SQL 语句时，根据 database-id 参数选择不同的 SQL 片段。

您可以根据不同的写法在 XML 中进行判断：

通过以上配置，您可以根据不同的数据库类型选择不同的 SQL 片段。

MyBatis Plus 不支持复合主键并强制使用唯一的 ID，这是出于以下考虑：

增加了表与表之间的相互依赖性：使用复合主键会使表与表之间的关系更加复杂，增加了维护和管理的难度。

增加了数据复杂的约束、规则：复合主键会增加数据的约束和规则，例如需要约束唯一性，而完全可以使用联合索引来实现。

增加了更新数据的限制：在更新数据时，需要更新所有复合主键的值，这增加了更新操作的限制和复杂性。

严重的数据冗余和更新异常问题：复合主键可能导致数据冗余和更新异常的问题，特别是在大型系统中，可能会出现更新异常的情况。

性能问题：使用复合主键时，查询某个 ID 时无法使用索引，会导致性能下降。

综上所述，虽然使用复合主键可以省去一个 ID 字段，但是这种做法的缺点大于优点，不建议也不推荐这样做。MyBatis Plus 坚持使用唯一的 ID，以保证数据管理的简单性、可维护性和性能。

碰到过使用hikari在linux系统下启动缓慢

在java启动命令中指定 -Djava.security.egd=file:/dev/urandom把获取随机数的方式从 /dev/random改为/dev/urandom

示例: java -Djava.security.egd=file:/dev/urandom -jar xxxx.jar

解决方案：驱动连接去掉 rewriteBatchedStatements=true 配置

解决方案：配置returnInstanceForEmptyRow 为true

解决方案：配置callSettersOnNulls 为true

重写接口方法请区分default方法和抽象接口方法,重写的方法需要以最终调用的实际方法为准.

抽象接口方法: 直接在XML重写此方法可完成

default方法: 直接重写真实调用的方法或者把原default重写为真实接口方法,然后在XML或注解的方式重写执行语句.

解决方案：配置编译插件的编译参数 -Xjvm-default=all

原因：jsqlParser4.9，连续的换行会认为是结束语句会抛出Encountered unexpected token解析错误，5.0开始如果碰到连续\n\n会把后面的语句截断导致在mybatis层出现Could not set parameters for mapping错误。

参考链接: https://github.com/JSQLParser/JSqlParser/issues/1988

mybatis-plus自3.5.3.2开始处理了框架内置注入的sql换行处理，但项目里自行编写的得语句需要自行处理。

注意: 3.5.10和3.5.10.1版本虽然处理项目里的换行情况，但对项目里使用单行注释(--或#这种语句处理无法支持)，3.5.11版本后将不再处理，如果你需要处理连续换行语句可通过下面的方式进行处理。

方式二: 开启mybatis的去除换行语句(通用性可能没那么好,无法处理单行注释—或#这样的)

后续不再维护Service与Repository，建议不要在继续使用，如果需要生成，请按如下步骤将service转换为repository。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (csharp):
```csharp
private transient String noColumn;
```

Example 2 (csharp):
```csharp
private static String noColumn;
```

Example 3 (csharp):
```csharp
@TableField(exist=false)private String noColumn;
```

Example 4 (csharp):
```csharp
/** * 忽略父类 createTime 字段映射 */private transient String createTime;
```

---

## 数据变动记录插件 | MyBatis-Plus

**URL:** https://baomidou.com/plugins/data-change-recorder/

**Contents:**
- 数据变动记录插件
- 插件简介
- 如何使用
  - 配置拦截器
  - 使用插件
- 注意事项

在数据库操作中，记录数据变动和控制操作的安全性是非常重要的。MyBatis-Plus 提供了一个数据变动记录插件 DataChangeRecorderInnerInterceptor，它能够自动记录操作日志，还支持批量更新或删除的安全阈值控制。

DataChangeRecorderInnerInterceptor 是 MyBatis-Plus 提供的一个拦截器，它可以在执行数据库操作时自动记录数据变动，并且可以根据配置的安全阈值来控制操作，比如限制一次批量更新或删除的记录数不超过 1000 条。

为了更好地理解如何使用 DataChangeRecorderInnerInterceptor，你可以参考官方提供的测试用例：

这个测试用例展示了如何使用插件进行数据变动记录和安全控制。

在你的 Spring Boot 配置类中，添加 DataChangeRecorderInnerInterceptor 到拦截器链中，并根据需要配置安全阈值：

在这个例子中，我们设置了批量更新或删除的安全阈值为 1000 条记录。

配置好插件之后，通过 MyBatis-Plus 执行操作，插件会自动记录数据变动并执行安全控制：

当执行批量更新或删除操作时，如果操作的记录数超过了配置的安全阈值，插件会抛出异常。

DataChangeRecorderInnerInterceptor 是一个强大的工具，它可以帮助你自动记录数据变动并控制操作的安全性。通过合理配置，你可以确保数据库操作的安全性和数据的完整性。记得在使用时遵循最佳实践，确保系统的安全性和稳定性。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (python):
```python
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;import com.baomidou.mybatisplus.extension.plugins.inner.DataChangeRecorderInnerInterceptor;import org.springframework.context.annotation.Bean;import org.springframework.context.annotation.Configuration;
@Configurationpublic class MybatisPlusConfig {
    @Bean    public MybatisPlusInterceptor mybatisPlusInterceptor() {        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();        DataChangeRecorderInnerInterceptor dataChangeRecorderInnerInterceptor = new DataChangeRecorderInnerInterceptor();        // 配置安全阈值，限制批量更新或删除的记录数不超过 1000 条        dataChangeRecorderInnerInterceptor.setBatchUpdateLimit(1000).openBatchUpdateLimitation();        interceptor.addInnerInterceptor(dataChangeRecorderInnerInterceptor);        return interceptor;    }}
```

---

## 逻辑删除支持 | MyBatis-Plus

**URL:** https://baomidou.com/guides/logic-delete/

**Contents:**
- 逻辑删除支持
- 逻辑删除的工作原理
- 支持的数据类型
- 使用方法
  - 方法 1: 配置全局逻辑删除属性
  - 方法 2: 如不想使用全局配置，可以在实体类中使用 @TableLogic 注解，对类单独进行配置
- 常见问题解答
  - 1. 如何处理插入操作？
  - 2. 删除接口自动填充功能失效怎么办？

逻辑删除是一种优雅的数据管理策略，它通过在数据库中标记记录为“已删除”而非物理删除，来保留数据的历史痕迹，同时确保查询结果的整洁性。MyBatis-Plus 提供了便捷的逻辑删除支持，使得这一策略的实施变得简单高效。

MyBatis-Plus 的逻辑删除功能会在执行数据库操作时自动处理逻辑删除字段。以下是它的工作方式：

逻辑删除字段支持所有数据类型，但推荐使用 Integer、Boolean 或 LocalDateTime。 如果使用 datetime 类型，可以配置逻辑未删除值为 null(长度为4的字符串,在yaml中需使用转义符(单引号)包裹)，已删除值可以使用函数如 now() 来获取当前时间。 如果使用 bigint 类型，可以配置逻辑未删除值为 0，已删除值可以使用函数如 UNIX_TIMESTAMP() 来获取当前时间戳作为删除标识。适合用于将删除字段作为唯一索引组成列,可以多次逻辑删除.

在 application.yml 中配置 MyBatis-Plus 的全局逻辑删除属性：

在实体类中，对应数据库表的逻辑删除字段上添加 @TableLogic 注解：

同样，逻辑未删除值默认为 0，逻辑已删除值默认为 1。这两个值可以通过设置 @TableLogic 注解的 value 属性和 delval 属性的值来进行修改。

通过以上步骤，你可以轻松地在 MyBatis-Plus 中实现逻辑删除功能，提高数据管理的灵活性和安全性。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (json):
```json
mybatis-plus:  global-config:    db-config:      logic-delete-field: deleted # 全局逻辑删除字段名(deleted为实体类属性名称)      logic-delete-value: 1 # 逻辑已删除值。可选，默认值为 1      logic-not-delete-value: 0 # 逻辑未删除值。可选，默认值为 0
```

Example 2 (csharp):
```csharp
import com.baomidou.mybatisplus.annotation.TableLogic;
public class User {    // 其他字段...
    @TableLogic    private Integer deleted;}
```

---

## SQL分析与打印 | MyBatis-Plus

**URL:** https://baomidou.com/guides/p6spy/

**Contents:**
- SQL分析与打印
- p6spy简介
- 示例工程
- 依赖引入
- 配置
  - application.yml
  - spy.properties
- Spring Boot集成
  - 依赖
  - 配置

MyBatis-Plus提供了SQL分析与打印的功能，通过集成p6spy组件，可以方便地输出SQL语句及其执行时长。本功能适用于MyBatis-Plus 3.1.0及以上版本。

p6spy 是一个针对数据库访问进行拦截和记录的工具，它通过代理JDBC驱动程序来工作。这意味着你的应用程序可以像往常一样使用JDBC，而p6spy会在幕后记录所有的SQL语句及其执行时间。这对于开发和调试过程中的SQL优化非常有用。

p6spy不仅限于记录SQL日志，它还提供了一些高级功能，如：

p6spy是一个强大的工具，它为MyBatis-Plus用户提供了便捷的SQL分析与打印功能。通过合理配置，你可以在开发和测试阶段有效地监控和优化SQL语句。然而，由于性能损耗，建议在生产环境中谨慎使用。

为了更好地理解如何使用这一功能，可以参考官方提供的示例工程：

首先，需要在项目中引入p6spy依赖。以下是Maven和Gradle两种构建工具的引入方式：

接下来，需要在application.yml或application.properties中进行相应的配置。

p6spy的配置文件spy.properties包含了多个配置项，以下是一些关键配置的示例：

对于Spring Boot项目，可以使用p6spy-spring-boot-starter来简化集成过程。

更多关于p6spy-spring-boot-starter的配置信息，请参考GitHub。

通过以上步骤，你就可以在开发过程中方便地分析和打印SQL语句了。记得根据实际需要调整配置，以达到最佳的使用效果。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (typescript):
```typescript
<dependency>    <groupId>p6spy</groupId>    <artifactId>p6spy</artifactId>    <version>3.9.1</version></dependency>
```

Example 2 (json):
```json
implementation 'p6spy:p6spy:3.9.1'
```

Example 3 (yaml):
```yaml
spring:  datasource:    driver-class-name: com.p6spy.engine.spy.P6SpyDriver    url: jdbc:p6spy:h2:mem:test    # 其他数据库配置...
```

Example 4 (markdown):
```markdown
# 模块列表，根据版本选择合适的配置modulelist=com.baomidou.mybatisplus.extension.p6spy.MybatisPlusLogFactory,com.p6spy.engine.outage.P6OutageFactory
# 自定义日志格式logMessageFormat=com.baomidou.mybatisplus.extension.p6spy.P6SpyLogger
# 日志输出到控制台appender=com.baomidou.mybatisplus.extension.p6spy.StdoutLogger
# 取消JDBC驱动注册deregisterdrivers=true
# 使用前缀useprefix=true
# 排除的日志类别excludecategories=info,debug,result,commit,resultset
# 日期格式dateformat=yyyy-MM-dd HH:mm:ss
# 实际驱动列表# driverlist=org.h2.Driver
# 开启慢SQL记录outagedetection=true
# 慢SQL记录标准（单位：秒）outagedetectioninterval=2
# 过滤 flw_ 开头的表 SQL 打印filter=trueexclude=flw_*
```

---

## 使用配置 | MyBatis-Plus

**URL:** https://baomidou.com/reference/

**Contents:**
- 使用配置
- 使用方式
  - Spring Boot 配置
  - Spring MVC 配置
- Base
  - configLocation
  - mapperLocations
  - typeAliasesPackage
  - typeAliasesSuperType
  - typeHandlersPackage

MyBatis-Plus 提供了丰富的配置选项，以满足不同用户的需求。这些配置中，一部分继承自 MyBatis 原生支持的配置，另一部分则是 MyBatis-Plus 特有的扩展配置。

在 Spring Boot 项目中，可以通过 application.yml 或 application.properties 文件来配置 MyBatis-Plus。

在传统的 Spring MVC 项目中，可以通过 XML 配置文件来配置 MyBatis-Plus。

指定 MyBatis 配置文件的位置。如果有单独的 MyBatis 配置文件，应将其路径配置到 configLocation。

指定 MyBatis Mapper 对应的 XML 文件位置。如果在 Mapper 中有自定义方法，需要配置此项。

对于 Maven 多模块项目，扫描路径应以 classpath*: 开头，以加载多个 JAR 包中的 XML 文件。

指定 MyBatis 别名包扫描路径，用于给包中的类注册别名。注册后，在 Mapper 对应的 XML 文件中可以直接使用类名，无需使用全限定类名。

与 typeAliasesPackage 一起使用，仅扫描指定父类的子类。

指定 TypeHandler 扫描路径，用于注册自定义类型转换器。

TypeHandler 用于自定义类型转换。

从 3.5.2 版本开始，该配置无效，通用枚举功能无需配置即可使用。

指定启动时是否检查 MyBatis XML 文件的存在，默认不检查。

指定 MyBatis 的执行器类型，包括 SIMPLE、REUSE 和 BATCH。

指定外部化 MyBatis Properties 配置，用于抽离配置，实现不同环境的配置部署。

原生 MyBatis 所支持的配置，具体请查看 Configuration。

MyBatis-Plus 全局策略配置，具体请查看 GlobalConfig。

MyBatis-Plus 的 Configuration 配置继承自 MyBatis 原生支持的配置，这意味着您可以通过 MyBatis XML 配置文件的形式进行配置，也可以通过 Spring Boot 或 Spring MVC 的配置文件进行设置。

开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN（下划线命名） 到经典 Java 属性名 aColumn（驼峰命名） 的类似映射。

在 MyBatis-Plus 中，此属性也将用于生成最终的 SQL 的 select body。如果您的数据库命名符合规则，无需使用 @TableField 注解指定数据库字段名。

默认枚举处理类，如果配置了该属性，枚举将统一使用指定处理器进行处理。

MyBatis-Plus 支持多种枚举处理方式，包括存储枚举名称、索引或自定义处理。从 3.5.2 开始，默认枚举处理器为 CompositeEnumTypeHandler，它会根据枚举是否为通用枚举来选择合适的处理方式。

从 3.5.2 开始，默认枚举处理器为 CompositeEnumTypeHandler，会对定义为 MyBatis-Plus 通用枚举的枚举(实现IEnum了或加了EnumValue注解) 在内部使用MybatisEnumTypeHandler处理枚举。

其他的枚举使用内部属性 defaultEnumTypeHandler(默认为org.apache.ibatis.type.EnumTypeHandler)进行处理。

此配置仅改变 CompositeEnumTypeHandler#defaultEnumTypeHandler的值

当设置为 true 时，懒加载的对象可能被任何懒属性全部加载，否则，每个属性都按需加载。需要和 lazyLoadingEnabled 一起使用。

MyBatis 自动映射策略，通过该配置可指定 MyBatis 是否并且如何来自动映射数据表字段与对象的属性。

MyBatis 自动映射时未知列或未知属性处理策略，通过该配置可指定 MyBatis 在自动映射过程中遇到未知列或者未知属性时如何处理。

Mybatis 一级缓存，默认为 SESSION。

在单服务架构中（仅有一个程序提供相同服务），开启一级缓存不会影响业务，只会提高性能。在微服务架构中需要关闭一级缓存，原因是：Service1 查询数据后，如果 Service2 修改了数据，Service1 再次查询时可能会得到过期数据。

指定当结果集中值为 null 时是否调用映射对象的 Setter 方法（Map 对象时为 put 方法）。通常用于有 Map.keySet() 依赖或 null 值初始化的情况。

基本类型（int、boolean 等）是不能设置成 null 的。

指定一个提供 Configuration 实例的工厂类。该工厂生产的实例将用来加载已被反序列化对象的懒加载属性值。工厂类必须包含一个签名方法 static Configuration getConfiguration()。

GlobalConfig 是 MyBatis-Plus 提供的全局策略配置，它允许开发者对 MyBatis-Plus 的行为进行全局性的定制。

控制是否在控制台打印 MyBatis-Plus 的 LOGO。

控制是否初始化 SqlRunner（com.baomidou.mybatisplus.extension.toolkit.SqlRunner）。

SQL 注入器，用于注入 MyBatis-Plus 提供的通用方法。Starter 下支持@Bean注入。

通用 Mapper 父类，只有该父类的子类 Mapper 才会注入 sqlInjector 内的方法。

元对象字段填充控制器，用于自动填充实体类的字段。Starter 下支持@Bean注入。

Id 生成器，用于生成实体类的唯一标识。Starter 下支持@Bean注入。

MyBatis-Plus 全局策略中的 DB 策略配置，具体请查看 DbConfig。

指定数据库的 Schema 名称，通常不用设置。

用于在生成 SQL 时对字段名进行格式化，例如添加前缀或后缀，对主键无效，例: %s。

在生成 SQL 时对表名进行格式化，例: %s。

用于在 Entity 的字段映射到数据库字段时进行格式化，只有在 column as property 这种情况下生效，对主键无效，例: %s。

自定义表主键生成器。Starter 下支持@Bean注入。

全局的 Entity 逻辑删除字段属性名，仅在逻辑删除功能打开时有效。

逻辑已删除值，仅在逻辑删除功能打开时有效。

逻辑未删除值，仅在逻辑删除功能打开时有效。

控制字段在 Insert 时的字段验证策略。

控制字段在 Update 时的字段验证策略。

控制字段在 Update 时的字段验证策略。既 Wrapper 根据内部 Entity 生成的 Where 条件。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (unknown):
```unknown
mybatis-plus:  configuration:    # MyBatis 配置    map-underscore-to-camel-case: true  global-config:    # 全局配置    db-config:      # 数据库配置      id-type: auto
```

Example 2 (jsx):
```jsx
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">    <property name="dataSource" ref="dataSource"/>    <property name="mapperLocations" value="classpath*:mapper/**/*.xml"/>    <property name="typeAliasesPackage" value="com.your.domain"/>    <!-- 其他配置 --></bean>
```

Example 3 (unknown):
```unknown
mybatis-plus:  config-location: classpath:/mybatis-config.xml
```

Example 4 (unknown):
```unknown
mybatis-plus:  mapper-locations: classpath:/mapper/**.xml
```

---

## 注解配置 | MyBatis-Plus

**URL:** https://baomidou.com/reference/annotation/

**Contents:**
- 注解配置
- @TableName
  - value
  - schema
  - keepGlobalPrefix
  - resultMap
  - autoResultMap
  - excludeProperty
- @TableId
  - value

本文详细介绍了 MyBatisPlus 注解的用法及属性，提供了源码链接以便深入理解。欢迎通过下方链接查看注解类的源码。

该注解用于指定实体类对应的数据库表名。当实体类名与数据库表名不一致，或者实体类名不是数据库表名的驼峰写法时，您需要使用这个注解来明确指定表名。

指定实体类对应的数据库表名。如果实体类名与表名不一致，使用这个属性来指定正确的表名。

指定数据库的 Schema 名称。通常情况下，如果你的数据库没有使用 Schema 来组织表，这个属性可以不填写。

类型： boolean 默认值： false

类型： boolean 默认值： false

当全局配置了 tablePrefix 时，这个属性决定是否保持使用全局的表前缀。如果设置为 true，即使注解中指定了表名，也会自动加上全局的表前缀。

指定在 XML 中定义的 ResultMap 的 ID，用于将查询结果映射到特定类型的实体类对象。

类型： boolean 默认值： false

类型： boolean 默认值： false

是否自动构建 resultMap。如果已经设置了 resultMap，这个属性不会生效。

MyBatis-Plus 会自动构建一个 resultMap 并注入到 MyBatis 中。但是，一旦注入完成，生成的内容就是静态的，类似于 XML 配置中的内容。在使用与 resultMap 相关的操作时，请注意 typeHandler 的处理。

MyBatis 只支持将 typeHandler 写在两个地方：

除了以上两种直接指定 typeHandler 的形式，MyBatis 还有一个全局扫描自定义 typeHandler 包的配置，原理是根据您的属性类型去找其对应的 typeHandler 并使用。

类型： String[] 默认值： {} 添加于： @since 3.3.1

类型： String[] 默认值： {} 添加于： @since 3.3.1

指定在映射时需要排除的属性名。这些属性将不会被包含在生成的 SQL 语句中。

该注解用于标记实体类中的主键字段。如果你的主键字段名为 id，你可以省略这个注解。

指定数据库表的主键字段名。如果不设置，MyBatis-Plus 将使用实体类中的字段名作为数据库表的主键字段名。

类型： Enum 默认值： IdType.NONE

类型： Enum 默认值： IdType.NONE

请注意，已弃用的ID类型（如 ID_WORKER, UUID, ID_WORKER_STR）应避免使用，并使用 ASSIGN_ID 或 ASSIGN_UUID 代替。这些新的策略提供了更好的灵活性和兼容性。

该注解用于标记实体类中的非主键字段，它告诉 MyBatis-Plus 如何映射实体类字段到数据库表字段。如果你的实体类字段名遵循驼峰命名规则，并且与数据库表字段名一致，你可以省略这个注解。

指定数据库中的字段名。如果你的实体类字段名与数据库字段名不同，使用这个属性来指定正确的数据库字段名。

类型： boolean 默认值： true

类型： boolean 默认值： true

指示这个字段是否存在于数据库表中。如果设置为 false，MyBatis-Plus 在生成 SQL 时会忽略这个字段。

在执行实体查询（EntityQuery）时，指定字段的条件表达式。这允许你自定义字段在 WHERE 子句中的比较方式。如果该项有值则按设置的值为准，无值则默认为全局的 %s=#{%s} 为准。

EntityQuery 是指在构建查询条件时，直接使用实体类的字段来设置查询条件，而不是手动编写 SQL 片段。

假设我们有一个实体类 User，它有 id、name 和 age 三个字段。我们想要查询所有年龄大于 18 岁的用户，我们可以使用 QueryWrapper 来构建这个查询，直接传递 User 实体类实例：

在这个例子中，我们通过 @TableField(condition = "%s > #{%s}") 注解为 age 字段设置了自定义的条件表达式。当构建查询时，我们创建了一个 User 实例，并设置了 age 字段的值为 18。然后，我们使用这个实例来创建 QueryWrapper，MyBatis-Plus 会根据实体类上的注解自动生成相应的 SQL 查询条件。

执行 findUserAgeOver18 方法时，MyBatis-Plus 会生成类似以下的 SQL 语句：

通过这种方式，condition 属性允许我们自定义字段在查询时的行为，使得查询更加灵活和符合特定需求，同时避免了手动编写 SQL 片段的繁琐。

在执行更新操作时，指定字段在 SET 子句中的表达式。这个属性的优先级高于 el 属性，允许你自定义字段的更新逻辑。

假设我们有一个实体类 User，其中包含一个 version 字段，我们希望在每次更新用户信息时，自动将 version 字段的值增加 1。我们可以使用 @TableField 注解的 update 属性来实现这个功能：

在这个例子中，@TableField(update="%s+1") 注解告诉 MyBatis-Plus，在执行更新操作时，对于 version 字段，应该使用 version = version + 1 的表达式。这意味着，每次更新用户信息时，version 字段的值都会自动增加 1。

MyBatis-Plus 会自动生成类似以下的 SQL 语句：

通过这种方式，update 属性允许我们自定义字段在更新时的行为，使得更新操作更加灵活和符合特定需求，同时避免了手动编写 SQL 片段的繁琐。

类型： Enum 默认值： FieldStrategy.DEFAULT

类型： Enum 默认值： FieldStrategy.DEFAULT

定义在插入新记录时，如何处理字段的值。这个属性允许你控制字段是否应该包含在 INSERT 语句中，以及在什么条件下包含。

假设我们有一个实体类 User，其中包含一个 nickname 字段，我们希望在插入新用户时，只有当 nickname 不为空时才插入该字段。我们可以使用 @TableField 注解的 insertStrategy 属性来实现这个功能：

在这个例子中，@TableField(insertStrategy = FieldStrategy.NOT_EMPTY) 注解告诉 MyBatis-Plus，在插入新用户时，只有当 nickname 字段不为空时才将其包含在 INSERT 语句中。

MyBatis-Plus 会自动生成类似以下的 SQL 语句：

如果 nickname 字段为空，生成的 SQL 将不包含 nickname 字段：

其效果等同于如下 MyBatis 的自定义 XML 配置：

通过这种方式，insertStrategy 属性允许我们自定义字段在插入时的行为，使得插入操作更加灵活和符合特定需求，同时避免了手动编写 SQL 片段的繁琐。

类型： Enum 默认值： FieldStrategy.DEFAULT

类型： Enum 默认值： FieldStrategy.DEFAULT

定义在更新记录时，如何处理字段的值。这个属性允许你控制字段是否应该包含在 UPDATE 语句的 SET 子句中，以及在什么条件下包含。

参见 insertStrategy 属性以获取更多关于 FieldStrategy 枚举类型的详细信息。

假设我们有一个实体类 User，其中包含一个 nickname 字段，我们希望在更新用户信息时，总是更新 nickname 字段，无论其值是否为空。我们可以使用 @TableField 注解的 updateStrategy 属性来实现这个功能：

在这个例子中，@TableField(updateStrategy = FieldStrategy.ALWAYS) 注解告诉 MyBatis-Plus，在更新用户信息时，总是将 nickname 字段包含在 UPDATE 语句的 SET 子句中，忽略其值的检查。

MyBatis-Plus 会自动生成类似以下的 SQL 语句：

无论 nickname 字段的值是否为空，生成的 SQL 都会包含 nickname 字段。也就是说，即使 nickname 字段的值为空，生成的 SQL 也会更新 nickname 字段为 NULL。

通过这种方式，updateStrategy 属性允许我们自定义字段在更新时的行为，使得更新操作更加灵活和符合特定需求，同时避免了手动编写 SQL 片段的繁琐。

类型： Enum 默认值： FieldStrategy.DEFAULT

类型： Enum 默认值： FieldStrategy.DEFAULT

定义在生成更新语句的 WHERE 子句时，如何处理字段的值。这个属性允许你控制字段是否应该包含在 WHERE 子句中，以及在什么条件下包含。

参见 insertStrategy 和 updateStrategy 属性以获取更多关于 FieldStrategy 枚举类型的详细信息。

假设我们有一个实体类 User，其中包含一个 nickname 字段，我们希望在更新用户信息时，只有当 nickname 字段不为空时，才将其作为 WHERE 子句的条件。我们可以使用 @TableField 注解的 whereStrategy 属性来实现这个功能：

在这个例子中，@TableField(whereStrategy = FieldStrategy.NOT_EMPTY) 注解告诉 MyBatis-Plus，在使用 whereEntity 生成更新语句的 WHERE 子句时，只有当 nickname 字段不为空时，才将其包含在 WHERE 子句中。

MyBatis-Plus 会自动生成类似以下的 SQL 语句：

如果 nickname 字段为空，生成的 SQL 将不包含 nickname 字段：

其效果等同于如下 MyBatis 的自定义 XML 配置：

通过这种方式，whereStrategy 属性允许我们自定义字段在 WHERE 子句中的行为，使得更新操作更加灵活和符合特定需求，同时避免了手动编写 SQL 片段的繁琐。参见 insertStrategy 和 updateStrategy 属性以获取更多关于 FieldStrategy 枚举类型的详细信息。

类型： Enum 默认值： FieldFill.DEFAULT

类型： Enum 默认值： FieldFill.DEFAULT

字段自动填充策略。该属性用于指定在执行数据库操作（如插入、更新）时，如何自动填充字段的值。通过使用 FieldFill 枚举，可以灵活地控制字段的填充行为。

FieldFill.DEFAULT：默认不进行填充，依赖于数据库的默认值或手动设置。

FieldFill.INSERT：在插入操作时自动填充字段值。

FieldFill.UPDATE：在更新操作时自动填充字段值。

FieldFill.INSERT_UPDATE：在插入和更新操作时都会自动填充字段值。

假设有一个 User 实体类，其中包含 createTime 和 updateTime 字段，我们希望在创建用户时自动填充创建时间，在更新用户信息时自动填充更新时间。

在这个示例中，createTime 字段会在插入操作时自动填充当前时间，而 updateTime 字段会在更新操作时自动填充当前时间。这样，我们就不需要在每次数据库操作时手动设置这些时间字段的值了。

请注意，为了使自动填充功能正常工作，您需要在 MyBatis-Plus 的配置中设置相应的自动填充处理器，并且确保在实体类对应的 Mapper 接口中定义了相应的插入和更新方法。

类型： boolean 默认值： true

类型： boolean 默认值： true

指示在执行查询操作时，该字段是否应该包含在 SELECT 语句中。这个属性允许您控制查询结果中包含哪些字段，从而提供更细粒度的数据访问控制。

当 select 属性设置为 true（默认值）时，该字段将包含在查询结果中。

当 select 属性设置为 false 时，即使该字段存在于数据库表中，它也不会包含在查询结果中。这在需要保护敏感数据或优化查询性能时非常有用。

假设有一个 User 实体类，其中包含 password 字段，我们希望在查询用户信息时排除密码字段，以保护用户隐私。

在这个示例中，当执行查询操作时，password 字段不会被包含在 SELECT 语句中，因此不会出现在查询结果中。这样，即使数据库中存储了密码信息，也不会在常规查询中泄露。

请注意，@TableField 注解的 select 属性仅影响 MyBatis-Plus 生成的查询语句，不会影响其他框架或手动编写的 SQL 语句。此外，如果使用了 select = false 的字段，那么在自定义查询或使用其他方式访问该字段时，需要特别注意数据的安全性和完整性。

类型： boolean 默认值： false

类型： boolean 默认值： false

指示在处理字段时是否保持使用全局 DbConfig 中定义的 columnFormat 规则。这个属性用于控制字段值在数据库操作中是否应用全局的列格式化规则。

类型： JdbcType 默认值： JdbcType.UNDEFINED

类型： JdbcType 默认值： JdbcType.UNDEFINED

JDBC类型，用于指定字段在数据库中的数据类型。这个属性允许您显式地设置字段的数据库类型，以确保与数据库的兼容性，特别是在处理特殊类型或自定义类型时。

当 jdbcType 属性设置为 JdbcType.UNDEFINED（默认值）时，MyBatis-Plus 将根据字段的 Java 类型自动推断其 JDBC 类型。

当 jdbcType 属性设置为特定的 JdbcType 枚举值时，该字段将使用指定的 JDBC 类型进行数据库操作。这可以用于解决类型映射问题，或者在需要精确控制数据库类型时使用。

假设有一个 CustomType 实体类，其中包含一个自定义类型 MyCustomType 的字段，我们希望在数据库中以特定的 JDBC 类型存储。

在这个示例中，myCustomField 字段将使用 VARCHAR JDBC 类型进行数据库操作。这样，即使 MyCustomType 是一个自定义类型，它也会被转换为 VARCHAR 类型存储在数据库中。

请注意，jdbcType 属性仅在特定情况下需要设置，例如当 Java 类型与数据库类型之间存在不明确的映射关系时。在大多数情况下，MyBatis-Plus 能够自动处理类型映射，因此不需要显式设置 jdbcType。此外，jdbcType 属性仅影响 MyBatis-Plus 生成的 SQL 语句，不会影响其他框架或手动编写的 SQL 语句。

类型： Class<? extends TypeHandler> 默认值： UnknownTypeHandler.class

类型： Class<? extends TypeHandler> 默认值： UnknownTypeHandler.class

类型处理器，用于指定在数据库操作中如何处理特定字段的值。这个属性允许您自定义字段值的转换逻辑，以适应特定的数据类型或业务需求。

当 typeHandler 属性未设置（即使用默认值 UnknownTypeHandler.class）时，MyBatis-Plus 将使用默认的类型处理器来处理字段值。

当 typeHandler 属性设置为特定的 TypeHandler 子类时，该字段将使用指定的类型处理器进行数据库操作。这可以用于处理自定义类型、特殊数据格式或非标准的数据库类型。

假设我们有一个 User 实体类，其中包含一个 birthDate 字段，我们希望使用自定义的类型处理器来处理日期格式，该处理器能够将日期按照特定的格式存储在数据库中。

在这个示例中，birthDate 字段将使用 CustomDateTypeHandler 类型处理器进行数据库操作。这样，无论是在查询时将数据库中的日期值转换为 LocalDate 对象，还是在更新时将 LocalDate 对象的日期值转换为数据库中的特定日期格式，都会使用 CustomDateTypeHandler 来处理。

CustomDateTypeHandler 可能实现如下：

在这个自定义类型处理器中，我们定义了一个 DateTimeFormatter 来指定日期的格式，并在 setNonNullParameter 和 getNullableResult 方法中实现了日期值的转换逻辑。

请注意，为了使自定义类型处理器生效，您需要确保它在 MyBatis-Plus 的配置中被正确注册，并且能够在运行时被加载。此外，自定义类型处理器的使用应当谨慎，确保其正确性和性能。更多详细信息，请参考 字段类型处理器 的内容。

指定小数点后保留的位数，该属性仅在执行 update 操作时生效。它用于控制数值类型字段在更新时的小数精度。

示例说明 假设有一个 Product 实体类，其中包含一个 price 字段，我们希望在更新价格时确保小数点后保留两位。

在这个示例中，price 字段在执行 update 操作时将确保小数点后保留两位。这意味着在更新价格时，无论传入的价格值小数位数是多少，都会被格式化为两位小数。

请注意，为了使 numericScale 属性生效，您需要确保数据库支持指定的小数位数，并且在执行 update 操作时，传入的数值类型字段值会被正确处理。此外，numericScale 属性仅影响 MyBatis-Plus 生成的 SQL 语句，不会影响其他框架或手动编写的 SQL 语句。

关于 numericScale、jdbcType、typeHandler 的说明

该注解用于标记实体类中的字段作为乐观锁版本号字段。乐观锁是一种并发控制机制，它假设多个事务可以同时进行而不会互相干扰，只在提交事务时检查是否有冲突。通过在实体类中使用@Version注解，MyBatis-Plus 会在更新操作时自动检查版本号，确保在更新过程中数据没有被其他事务修改。

在上面的示例中，version字段被标记为乐观锁版本号。当执行更新操作时，MyBatis-Plus 会检查该字段的值是否与数据库中的值一致。如果不一致，说明在读取数据后有其他事务修改了数据，此时会抛出乐观锁异常，提示开发者进行相应的处理。

使用@Version注解可以有效地防止并发更新时出现的数据不一致问题，提高系统的并发性能和数据完整性。开发者无需手动编写版本号检查的代码，MyBatis-Plus 会自动处理这一过程。

该注解用于标记枚举类中的字段，指定在数据库中存储的枚举值。当实体类中的某个字段是枚举类型时，使用@EnumValue注解可以告诉MyBatis-Plus在数据库中存储枚举值的哪个属性。

在上面的示例中，Gender枚举类中的code字段被标记为@EnumValue，这意味着在数据库中存储User实体类的gender字段时，将存储Gender枚举的code值，而不是枚举常量本身。

使用@EnumValue注解可以灵活地控制枚举类型在数据库中的存储方式，使得数据库中的数据更加紧凑和易于处理。同时，它也简化了从数据库读取枚举值时的转换过程，因为MyBatis-Plus会自动根据@EnumValue注解的配置将数据库中的值转换为对应的枚举实例。

该注解用于标记实体类中的字段作为逻辑删除字段。逻辑删除是一种数据管理策略，它不是真正地从数据库中删除记录，而是在记录中标记该记录为已删除状态。通过使用@TableLogic注解，MyBatis-Plus 可以在查询、更新和删除操作中自动处理逻辑删除字段的值。

在上面的示例中，deleted字段被标记为逻辑删除字段。@TableLogic注解的value属性指定了逻辑未删除的值（在这个例子中是0），而delval属性指定了逻辑删除的值（在这个例子中是1）。

当执行查询操作时，MyBatis-Plus 会自动过滤掉标记为逻辑删除的记录，只返回未删除的记录。在执行更新操作时，如果更新操作会导致逻辑删除字段的值变为逻辑删除值，MyBatis-Plus 会自动将该记录标记为已删除。在执行删除操作时，MyBatis-Plus 会自动将逻辑删除字段的值更新为逻辑删除值，而不是物理删除记录。

使用@TableLogic注解可以实现数据的逻辑删除，有助于维护数据的完整性和可追溯性，同时避免了物理删除操作可能带来的数据丢失风险。开发者无需手动编写逻辑删除的代码，MyBatis-Plus 会自动处理这一过程。

该注解用于指定 Oracle 数据库中序列（Sequence）的名称，以便在实体类中生成主键值。在 Oracle 数据库中，主键通常是通过序列来生成的，而不是像其他数据库那样使用自增字段。@KeySequence注解告诉 MyBatis-Plus 使用特定的序列来生成主键。

在上面的示例中，@KeySequence注解被应用于实体类User，并指定了序列名称为”SEQ_USER_ID”。这意味着在插入新记录时，MyBatis-Plus 将使用这个序列来生成id字段的值。

@KeySequence注解的value属性用于指定序列的名称，而dbType属性用于指定数据库类型。如果未指定dbType，MyBatis-Plus 将默认使用注入的IKeyGenerator实现。如果有多个IKeyGenerator实现，则必须指定dbType。

使用@KeySequence注解可以确保在 Oracle 数据库中正确地生成主键值，同时简化了主键生成的配置过程。开发者无需手动编写获取序列值的代码，MyBatis-Plus 会自动处理这一过程。

该注解用于指定Mapper的某个method(注解在method上)或者所有method(注解在Mapper上)在执行时是否忽略特定的插件(比如多租户)

在上面的示例中，在执行selectUsers方法时,多租户拦截器将被忽略

MyBatis-Plus 提供的插件在注解里都有对应的属性,比如多租户插件为tenantLine属性,如果属性对应的值为”1”、“yes”、“on”,则表示对应的插件将被忽略,如果属性对应的值为”0”、“false”、“off”或为空,则插件将正常执行。

该注解用于指定实体类中的字段在执行查询操作时的默认排序方式。通过在实体类字段上使用@OrderBy注解，可以确保在执行查询时，如果没有显式指定排序条件，MyBatis-Plus 将按照注解中定义的排序规则返回结果。

在上面的示例中，age字段被标记为@OrderBy，并设置了asc属性为false，表示默认排序为倒序（降序），sort属性设置为10，表示该排序规则的优先级。

@OrderBy注解的asc属性用于指定排序是否为升序，如果设置为true，则表示升序排序；如果设置为false，则表示降序排序。sort属性用于指定排序规则的优先级，数字越小，优先级越高，即越先被应用。

需要注意的是，@OrderBy注解的排序规则优先级低于在查询时通过Wrapper条件查询对象显式指定的排序条件。如果在Wrapper中指定了排序条件，那么@OrderBy注解中定义的默认排序将被覆盖。

使用@OrderBy注解可以简化代码，避免在每次查询时都显式指定排序条件，同时提供了一种默认的排序方式，有助于提高代码的可读性和维护性。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (csharp):
```csharp
@TableName("sys_user")public class User {    private Long id;    private String name;    private Integer age;    private String email;}
```

Example 2 (csharp):
```csharp
@TableName("sys_user")public class User {    @TableId    private Long id;    private String name;    private Integer age;    private String email;}
```

Example 3 (csharp):
```csharp
@TableName("sys_user")public class User {    @TableId    private Long id;    @TableField("nickname") // 映射到数据库字段 "nickname"    private String name;    private Integer age;    private String email;}
```

Example 4 (csharp):
```csharp
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;import com.baomidou.mybatisplus.annotation.TableField;import com.baomidou.mybatisplus.annotation.SqlCondition;
// 实体类定义@TableName("sys_user")public class User {    @TableId    private Long id;
    private String name;
    @TableField(condition = "%s > #{%s}") // 自定义 age 字段的条件表达式    private Integer age;
    private String email;}
// 使用 EntityQuery 构建查询public List<User> findUserAgeOver18() {    // 创建 User 实例，用于设置查询条件    User queryEntity = new User();    queryEntity.setAge(18); // 设置 age 字段的值
    // 创建 QueryWrapper 实例，并传递 User 实例    QueryWrapper<User> queryWrapper = new QueryWrapper<>(queryEntity);
    // 执行查询    List<User> userList = userMapper.selectList(queryWrapper);
    return userList;}
```

---

## 动态表名插件 | MyBatis-Plus

**URL:** https://baomidou.com/plugins/dynamic-table-name/

**Contents:**
- 动态表名插件
- 插件简介
- 快速开始
  - 配置拦截器
  - 使用动态表名
- 注意事项
- 示例项目

在数据库应用程序开发中，我们有时需要根据不同的条件查询不同的表。MyBatis-Plus 提供了一个动态表名插件 DynamicTableNameInnerInterceptor，它允许我们在运行时动态地改变 SQL 语句中的表名，这对于处理分表逻辑非常有用。

DynamicTableNameInnerInterceptor 是 MyBatis-Plus 提供的一个拦截器，它可以在执行 SQL 语句之前，根据配置的规则动态地替换表名。这个功能在处理分表逻辑时非常有用，比如根据日期将数据存储在不同的表中。

在你的 Spring Boot 配置类中，添加 DynamicTableNameInnerInterceptor 到拦截器链中，并配置表名处理器：

在这个例子中，我们定义了一个表名处理器，它会根据随机数决定将表名后缀设置为 _2018 或 _2019。

在你的 Mapper 接口中，不需要特别指定动态表名，因为表名将在运行时由拦截器动态处理。

当执行查询时，MyBatis-Plus 会自动将表名替换为实际的表名。

为了更好地理解如何使用 DynamicTableNameInnerInterceptor，你可以参考官方提供的示例项目：

这个示例项目展示了如何根据年份动态地查询不同的用户表。

DynamicTableNameInnerInterceptor 是一个强大的工具，它可以帮助你轻松地处理动态表名的场景。通过合理配置，你可以让 MyBatis-Plus 自动为你处理复杂的分表逻辑，从而提高开发效率。记得在使用时遵循最佳实践，确保系统的安全性和稳定性。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (php):
```php
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;import com.baomidou.mybatisplus.extension.plugins.inner.DynamicTableNameInnerInterceptor;import org.springframework.context.annotation.Bean;import org.springframework.context.annotation.Configuration;
@Configurationpublic class MybatisPlusConfig {
    @Bean    public MybatisPlusInterceptor mybatisPlusInterceptor() {        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();        DynamicTableNameInnerInterceptor dynamicTableNameInnerInterceptor = new DynamicTableNameInnerInterceptor();        dynamicTableNameInnerInterceptor.setTableNameHandler((sql, tableName) -> {            // 获取参数方法            Map<String, Object> paramMap = RequestDataHelper.getRequestData();            paramMap.forEach((k, v) -> System.err.println(k + "----" + v));
            String year = "_2018";            int random = new Random().nextInt(10);            if (random % 2 == 1) {                year = "_2019";            }            return tableName + year;        });        interceptor.addInnerInterceptor(dynamicTableNameInnerInterceptor);        return interceptor;    }}
```

Example 2 (gdscript):
```gdscript
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
public interface UserMapper extends BaseMapper<User> {    // ...}
```

---

## 代码生成器配置 | MyBatis-Plus

**URL:** https://baomidou.com/reference/new-code-generator-configuration/

**Contents:**
- 代码生成器配置
  - 特点说明
  - 示例配置
  - 数据库配置 (DataSourceConfig)
    - 基础配置
    - 可选配置
- 全局配置 (GlobalConfig)
  - 方法说明
  - 示例配置
- 包配置 (PackageConfig)

MyBatis-Plus 全新代码生成器在继承原有功能的基础上，引入了更加灵活和高效的 builder 模式，使得开发者能够快速生成符合需求的代码，同时保持代码的优雅和整洁。这个新特性旨在进一步提升开发效率，减少重复劳动，让开发者能够更加专注于业务逻辑的实现。

Builder 模式：通过 builder 模式，开发者可以链式调用配置方法，直观地构建代码生成器的配置，使得代码更加清晰易读。

快速配置：新代码生成器提供了快速配置选项，如全局配置、包配置、策略配置等，可以一键设置常用选项，快速启动代码生成过程。

模板引擎：支持 Freemarker 等模板引擎，允许开发者自定义代码模板，以生成符合项目特定风格的代码。

Lombok 集成：新代码生成器默认启用 Lombok，减少了样板代码的编写，提高了代码的可读性和维护性。

多数据库支持：支持多种数据库类型，如 MySQL、Oracle、SQL Server 等，只需配置相应的数据库连接信息即可。

灵活的数据源配置：提供了丰富的数据源配置选项，包括数据库查询方式、类型转换器、关键字处理器等，满足不同数据库的需求。

全局配置提供了对代码生成器整体行为的设置，包括输出目录、作者信息、Kotlin 模式、Swagger 集成、时间类型策略等。

包配置用于定义生成代码的包结构，包括父包名、模块名、实体类包名、服务层包名等。

注意：自 MyBatis-Plus 3.5.6 版本开始，模板配置已迁移至 StrategyConfig 中。以下是迁移后的配置方式。

注入配置允许开发者自定义代码生成器的行为，包括在输出文件之前执行的逻辑、自定义配置 Map 对象、自定义配置模板文件等。

通过上述配置，开发者可以根据自己的需求，灵活地定制代码生成器的行为。例如，在生成文件之前执行特定的逻辑，或者使用自定义的模板文件来生成代码。这些配置选项提供了极大的灵活性，使得 MyBatis-Plus 代码生成器能够适应各种复杂的项目需求。

策略配置是 MyBatis-Plus 代码生成器的核心部分，它允许开发者根据项目需求定制代码生成的规则，包括命名模式、表和字段的过滤、以及各个代码模块的生成策略。

实体策略配置用于定制实体类的生成规则，包括父类、序列化版本 UID、文件覆盖、字段常量、链式模型、Lombok 模型等。

Controller 策略配置用于定制 Controller 类的生成规则，包括父类、文件覆盖、驼峰转连字符、RestController 注解等。

Service 策略配置用于定制 Service 接口和实现类的生成规则，包括父类、文件覆盖、文件名称转换等。

Mapper 策略配置用于定制 Mapper 接口和对应的 XML 映射文件的生成规则，包括父类、文件覆盖、Mapper 注解、结果映射、列列表、缓存实现类等。

MyBatis-Plus 代码生成器支持自定义模板，如 DTO (Data Transfer Object) 和 VO (Value Object) 等。

在上面的示例中，我们定义了一个名为 entityDTO.java.ftl 的自定义 Freemarker 模板，并将其路径添加到 customFile 映射中。在生成代码时，代码生成器将使用这个模板来生成 DTO 类。

通过上述配置，开发者可以根据项目需求自定义代码生成器的模板，从而生成符合特定项目结构的代码文件。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (php):
```php
// 使用 FastAutoGenerator 快速配置代码生成器FastAutoGenerator.create("jdbc:mysql://localhost:3306/mybatis_plus?serverTimezone=GMT%2B8", "root", "password")    .globalConfig(builder -> {        builder.author("Your Name") // 设置作者            .outputDir("src/main/java"); // 输出目录    })    .packageConfig(builder -> {        builder.parent("com.example") // 设置父包名            .entity("model") // 设置实体类包名            .mapper("dao") // 设置 Mapper 接口包名            .service("service") // 设置 Service 接口包名            .serviceImpl("service.impl") // 设置 Service 实现类包名            .xml("mappers"); // 设置 Mapper XML 文件包名    })    .strategyConfig(builder -> {        builder.addInclude("table1", "table2") // 设置需要生成的表名            .entityBuilder()            .enableLombok() // 启用 Lombok            .enableTableFieldAnnotation() // 启用字段注解            .controllerBuilder()            .enableRestStyle(); // 启用 REST 风格    })    .templateEngine(new FreemarkerTemplateEngine()) // 使用 Freemarker 模板引擎    .execute(); // 执行生成
```

Example 2 (json):
```json
DataSourceConfig dataSourceConfig = new DataSourceConfig.Builder("jdbc:mysql://127.0.0.1:3306/mybatis-plus", "root", "123456").build();
```

Example 3 (json):
```json
// 使用SQL查询的方式生成代码,属于旧的代码生成方式,通用性不是好,老的代码可以继续使用,适配数据库需要完成dbQuery和typeConvert的扩展,后期不再维护这种方式DataSourceConfig dataSourceConfig = new DataSourceConfig.Builder("jdbc:mysql://127.0.0.1:3306/mybatis-plus", "root", "123456")    .dbQuery(new MySqlQuery())    .schema("mybatis-plus")    .typeConvert(new MySqlTypeConvert())    .keyWordsHandler(new MySqlKeyWordsHandler())    .databaseQueryClass(SQLQuery.class)    .build();
// 使用元数据查询的方式生成代码,默认已经根据jdbcType来适配java类型,支持使用typeConvertHandler来转换需要映射的类型映射DataSourceConfig dataSourceConfig = new DataSourceConfig.Builder("jdbc:mysql://127.0.0.1:3306/mybatis-plus", "root", "123456")    .schema("mybatis-plus")    .keyWordsHandler(new MySqlKeyWordsHandler())    .build();
```

Example 4 (unknown):
```unknown
GlobalConfig globalConfig = new GlobalConfig.Builder()    .disableOpenDir(false) // 允许自动打开输出目录    .outputDir("/path/to/output") // 设置输出目录    .author("Your Name") // 设置作者名    .enableKotlin(true) // 开启 Kotlin 模式    .enableSwagger(true) // 开启 Swagger 模式    .dateType(DateType.ONLY_DATE) // 设置时间类型策略    .commentDate("yyyy-MM-dd") // 设置注释日期格式    .build();
```

---

## Mybatis X 插件 | MyBatis-Plus

**URL:** https://baomidou.com/guides/mybatis-x/

**Contents:**
- Mybatis X 插件
  - 安装指南
- 核心功能
  - XML 映射跳转
  - 代码生成
  - 重置模板
  - JPA 风格提示
- 常见问题解答
  - JPA 提示功能无法使用？
  - 生成的表名与预期不符？

MybatisX 是一款专为 IntelliJ IDEA 设计的快速开发插件，旨在提升 MyBatis 与 MyBatis-Plus 框架的开发效率。

如果您觉得 MybatisX 插件对您有帮助，请在插件页面给予五分好评，以支持开发者持续改进。

也欢迎大家参与 MyBatisX 插件的贡献，源码地址：MybatisX 源码

MybatisX 提供了便捷的 XML 映射文件与 Java 接口之间的跳转功能，让开发者能够快速地在两者之间切换，提高开发效率。

通过 MybatisX，您可以轻松地根据数据库表结构生成对应的 Java 实体类、Mapper 接口及 XML 映射文件。

MybatisX 允许您重置代码生成模板，以恢复到默认设置或自定义模板内容。

MybatisX 支持 JPA 风格的代码提示，包括新增、查询、修改和删除操作的自动代码生成。

JPA 提示功能依赖于 Mapper 接口与实体类之间的关联。确保您的 Mapper 满足以下任一条件：

MybatisX 提供了灵活的模板配置选项，允许开发者根据需要自定义代码生成模板。

在 Scratches and Consoles -> Extensions -> MybatisX 目录下，您可以找到默认提供的模板，如 default-all、default、mybatis-plus2、mybatis-plus3 等。

如需重置模板到默认设置，右键点击 MybatisX 目录，选择 Restore Default Extensions。

MybatisX 允许您根据项目需求自定义模板内容，包括实体类、表名、字段信息等。

通过 MybatisX 插件，您可以大幅提升 MyBatis 与 MyBatis-Plus 框架的开发效率，同时享受便捷的代码生成和模板自定义功能。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

---

## 主键生成策略 | MyBatis-Plus

**URL:** https://baomidou.com/guides/key-generator/

**Contents:**
- 主键生成策略
- 主键生成策略概述
- 示例
- Spring Boot 配置
  - 方式一：使用配置类
  - 方式二：通过 MybatisPlusPropertiesCustomizer 自定义
- Spring 配置
  - 方式一: XML 配置
  - 方式二：注解配置

在 MyBatis-Plus 中，主键生成策略是一个重要的概念，它决定了如何为数据库表中的记录生成唯一的主键值。以下是关于主键生成策略的详细说明和配置方法。

主键生成策略必须使用 INPUT 类型，这意味着主键值需要由用户在插入数据时提供。MyBatis-Plus 支持在父类中定义 @KeySequence 注解，子类可以继承使用。

从版本 3.3.0 开始，MyBatis-Plus 会自动识别主键类型，因此不再需要手动指定主键类型。

MyBatis-Plus 内置支持多种数据库的主键生成策略，包括：

如果内置的主键生成策略不能满足需求，可以通过实现 IKeyGenerator 接口来扩展自定义的主键生成策略。

下面是一个使用 @KeySequence 注解的实体类示例：

在这个示例中，YourEntity 类使用了 @KeySequence 注解来指定 Oracle 数据库中的序列 SEQ_ORACLE_STRING_KEY 来生成主键值，主键类型为 String。

在 Spring Boot 应用中，可以通过配置类来设置主键生成策略：

也可以通过 MybatisPlusPropertiesCustomizer 来自定义主键生成策略：

在传统的 Spring 应用中，可以通过 XML 配置来设置主键生成策略：

以上配置方法可以根据实际项目需求选择合适的方式来设置主键生成策略。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (csharp):
```csharp
@KeySequence(value = "SEQ_ORACLE_STRING_KEY", clazz = String.class)public class YourEntity {
    @TableId(value = "ID_STR", type = IdType.INPUT)    private String idStr;
    // 其他字段和方法...}
```

Example 2 (python):
```python
@Beanpublic IKeyGenerator keyGenerator() {    return new H2KeyGenerator();}
```

Example 3 (php):
```php
@Beanpublic MybatisPlusPropertiesCustomizer plusPropertiesCustomizer() {    return plusProperties -> plusProperties.getGlobalConfig().getDbConfig().setKeyGenerator(new H2KeyGenerator());}
```

Example 4 (jsx):
```jsx
<bean id="globalConfig" class="com.baomidou.mybatisplus.core.config.GlobalConfig">   <property name="dbConfig" ref="dbConfig"/></bean>
<bean id="dbConfig" class="com.baomidou.mybatisplus.core.config.GlobalConfig.DbConfig">   <property name="keyGenerator" ref="keyGenerator"/></bean>
<bean id="keyGenerator" class="com.baomidou.mybatisplus.extension.incrementer.H2KeyGenerator"/>
```

---
