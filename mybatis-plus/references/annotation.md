# Mybatis-Plus - Annotation

**Pages:** 6

---

## 自动填充字段 | MyBatis-Plus

**URL:** https://baomidou.com/guides/auto-fill-field/

**Contents:**
- 自动填充字段
- 原理概述
- 使用步骤
  - 1. 定义实体类
  - 2. 实现 MetaObjectHandler
  - 3. 配置自动填充处理器
- 注意事项
- 参数填充示例
- 无法填充示例
- FieldFill 枚举

MyBatis-Plus 提供了一个便捷的自动填充功能，用于在插入或更新数据时自动填充某些字段，如创建时间、更新时间等。以下是如何使用这一功能的详细说明。

自动填充功能通过实现 com.baomidou.mybatisplus.core.handlers.MetaObjectHandler 接口来实现。你需要创建一个类来实现这个接口，并在其中定义插入和更新时的填充逻辑。

在实体类中，你需要使用 @TableField 注解来标记哪些字段需要自动填充，并指定填充的策略。

创建一个类来实现 MetaObjectHandler 接口，并重写 insertFill 和 updateFill 方法。

确保你的 MyMetaObjectHandler 类被 Spring 管理，可以通过 @Component 或 @Bean 注解来实现。

通过以上步骤，你可以轻松地在 MyBatis-Plus 中实现自动填充功能，提高开发效率。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (csharp):
```csharp
public class User {    @TableField(fill = FieldFill.INSERT)    private String createTime;
    @TableField(fill = FieldFill.UPDATE)    private String updateTime;
    // 其他字段...}
```

Example 2 (java):
```java
// java example@Slf4j@Componentpublic class MyMetaObjectHandler implements MetaObjectHandler {
    @Override    public void insertFill(MetaObject metaObject) {        log.info("开始插入填充...");        this.strictInsertFill(metaObject, "createUserId", Long.class, 123456L)        this.strictInsertFill(metaObject, "createTime", LocalDateTime.class, LocalDateTime.now());    }
    @Override    public void updateFill(MetaObject metaObject) {        log.info("开始更新填充...");        this.strictInsertFill(metaObject, "updateUserId", Long.class, 123456L)        this.strictUpdateFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now());    }}
```

Example 3 (java):
```java
// kotlin example@Componentclass MyMetaObjectHandler : MetaObjectHandler {
    private val log = LoggerFactory.getLogger(MyMetaObjectHandler::class.java)
    // 注意将kotlin类型转为java类型请使用 xxx::class.javaObjectType，防止部分类型使用xxx::class.java转换为基本类型导致类型不一致无法填充    override fun insertFill(metaObject: MetaObject) {        log.info("开始插入填充...");        this.strictInsertFill(metaObject, "createUserId", Long::class.javaObjectType, 123456L)        this.strictInsertFill(metaObject, "createTime", LocalDateTime::class.javaObjectType, LocalDateTime.now())    }
    override fun updateFill(metaObject: MetaObject) {        log.info("开始更新填充...");        this.strictUpdateFill(metaObject, "updateUserId", Long::class.javaObjectType, 123456L)        this.strictUpdateFill(metaObject, "updateTime", LocalDateTime::class.javaObjectType, LocalDateTime.now())    }
}
```

Example 4 (typescript):
```typescript
// 插入填充示例insertFillByCustomMethod1(H2User h2User);insertFillByCustomMethod8(H2User[] h2Users);insertFillByCustomMethod4(Collection<H2User> h2User);
// 更新填充示例updateFillByCustomMethod2(@Param("coll") Collection<Long> ids, @Param("et") H2User h2User);updateFillByCustomMethod4(@Param("colls") Collection<Long> ids, @Param("et") H2User h2User);
```

---

## 字段类型处理器 | MyBatis-Plus

**URL:** https://baomidou.com/guides/type-handler

**Contents:**
- 字段类型处理器
- JSON 字段类型处理器
  - 配置
  - XML 配置对应写法
  - Wrapper 查询中的 TypeHandler 使用
- 自定义类型处理器
  - 创建自定义类型处理器
  - 使用自定义类型处理器

在 MyBatis 中，类型处理器（TypeHandler）扮演着 JavaType 与 JdbcType 之间转换的桥梁角色。它们用于在执行 SQL 语句时，将 Java 对象的值设置到 PreparedStatement 中，或者从 ResultSet 或 CallableStatement 中取出值。

MyBatis-Plus 给大家提供了提供了一些内置的类型处理器，可以通过 TableField 注解快速注入到 MyBatis 容器中，从而简化开发过程。

示例工程：👉 mybatis-plus-sample-typehandler

MyBatis-Plus 内置了多种 JSON 类型处理器，包括 AbstractJsonTypeHandler 及其子类 Fastjson2TypeHandler、FastjsonTypeHandler、GsonTypeHandler、JacksonTypeHandler 等。这些处理器可以将 JSON 字符串与 Java 对象相互转换。

在 XML 映射文件中，可以使用 <result> 元素来指定字段的类型处理器。

从 MyBatis-Plus 3.5.3.2 版本开始，可以在 Wrapper 查询中直接使用 TypeHandler。

通过上述示例，你可以看到 MyBatis-Plus 提供了灵活且强大的类型处理器支持，使得在处理复杂数据类型时更加便捷。在使用时，请确保选择了正确的 JSON 处理器，并引入了相应的 JSON 解析库依赖。

在 MyBatis-Plus 中，除了使用内置的类型处理器外，开发者还可以根据需要自定义类型处理器。

例如，当使用 PostgreSQL 数据库时，可能会遇到 JSONB 类型的字段，这时可以创建一个自定义的类型处理器来处理 JSONB 数据。

以下是一个自定义的 JSONB 类型处理器的示例：

示例工程：👉 mybatis-plus-sample-jsonb

在实体类中，通过 TableField 注解指定自定义的类型处理器：

通过上述步骤，你可以在 MyBatis-Plus 中使用自定义的 JSONB 类型处理器来处理 PostgreSQL 数据库中的 JSONB 类型字段。自定义类型处理器提供了极大的灵活性，使得开发者可以根据具体的数据库特性和业务需求来定制数据处理逻辑。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (csharp):
```csharp
@Data@Accessors(chain = true)@TableName(autoResultMap = true)public class User {    private Long id;
    ...
    /**     * 必须开启映射注解     *     * @TableName(autoResultMap = true)     *     * 选择对应的 JSON 处理器，并确保存在对应的 JSON 解析依赖包     */    @TableField(typeHandler = JacksonTypeHandler.class)    // 或者使用 FastjsonTypeHandler    // @TableField(typeHandler = FastjsonTypeHandler.class)    private OtherInfo otherInfo;}
```

Example 2 (sql):
```sql
<!-- 单个字段的类型处理器配置 --><result column="other_info" jdbcType="VARCHAR" property="otherInfo" typeHandler="com.baomidou.mybatisplus.extension.handlers.JacksonTypeHandler" />
<!-- 多个字段中某个字段的类型处理器配置 --><resultMap id="departmentResultMap" type="com.baomidou...DepartmentVO">    <result property="director" column="director" typeHandler="com.baomidou.mybatisplus.extension.handlers.JacksonTypeHandler" /></resultMap><select id="selectPageVO" resultMap="departmentResultMap">   select id,name,director from department ...</select>
```

Example 3 (typescript):
```typescript
Wrappers.<H2User>lambdaQuery()    .apply("name={0,typeHandler=" + H2userNameJsonTypeHandler.class.getCanonicalName() + "}", "{\"id\":101,\"name\":\"Tomcat\"}"))
```

Example 4 (java):
```java
import com.baomidou.mybatisplus.extension.handlers.JacksonTypeHandler;import org.apache.ibatis.type.JdbcType;import org.apache.ibatis.type.MappedJdbcTypes;import org.apache.ibatis.type.MappedTypes;import org.postgresql.util.PGobject;
import java.sql.CallableStatement;import java.sql.PreparedStatement;import java.sql.ResultSet;import java.sql.SQLException;
@MappedTypes({Object.class})@MappedJdbcTypes(JdbcType.VARCHAR)public class JsonbTypeHandler<T> extends JacksonTypeHandler<T> {
    private final Class<T> clazz;
    public JsonbTypeHandler(Class<T> clazz) {        if (clazz == null) {            throw new IllegalArgumentException("Type argument cannot be null");        }        this.clazz = clazz;    }
    // 自3.5.6版本开始支持泛型,需要加上此构造.    public JsonbTypeHandler(Class<?> type, Field field) {        super(type, field);    }
    @Override    public void setNonNullParameter(PreparedStatement ps, int i, T parameter, JdbcType jdbcType) throws SQLException {        PGobject jsonbObject = new PGobject();        jsonbObject.setType("jsonb");        jsonObject.setValue(toJson(parameter));        ps.setObject(i, jsonbObject);    }}
```

---

## 自动映射枚举 | MyBatis-Plus

**URL:** https://baomidou.com/guides/auto-convert-enum/

**Contents:**
- 自动映射枚举
- 枚举声明
  - 方式一：注解标记
  - 方式二：实现接口
- 未声明枚举
  - 修改全局 defaultEnumTypeHandler
- 号外参考: 如何序列化枚举值为前端返回值
  - Jackson
    - 一、重写 toString 方法
      - Spring Boot

我们在 mybatis 的 EnumOrdinalTypeHandler(基于枚举常量序号) 和 EnumTypeHandler(基于枚举常量名) 之外 提供了更加灵活的枚举处理器 MybatisEnumTypeHandler(基于枚举常量属性) 只需要对枚举进行声明,即可实现枚举的自动映射 未进行声明的枚举则根据 mybatis的defaultEnumTypeHandler 的默认值EnumTypeHandler 来进行映射

声明该枚举使用 MybatisEnumTypeHandler(基于枚举常量属性) 进行映射

枚举属性使用 @EnumValue 注解，指定枚举值在数据库中存储的实际值。支持枚举类中的任意字段，如序号或编码。

实现 IEnum 接口，实现 getValue 方法，指定枚举值在数据库中存储的实际值。支持枚举类中的任意字段，如序号或编码。

未声明的枚举将使用 mybatis 的 defaultEnumTypeHandler 的默认值 EnumTypeHandler 进行映射 可以通过修改全局配置来变更,不过这对上面步骤声明的枚举无效

在枚举中重写 toString 方法，以上两种方式任选其一。

在枚举中重写 toString 方法，以上两种方式任选其一。

通过以上步骤，你可以优雅地在 MyBatis-Plus 中使用枚举属性，并且能够方便地将枚举值序列化为前端所需的格式。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (csharp):
```csharp
public class User {    private String name; // 名字    private AgeEnum age; // 年龄    private GradeEnum grade; // 年级}
```

Example 2 (typescript):
```typescript
@Getter@AllArgsConstructorpublic enum GradeEnum {    PRIMARY(1, "小学"),    SECONDARY(2, "中学"),    HIGH(3, "高中");
    @EnumValue // 标记数据库存的值是code    private final int code;    // 其他属性...}
```

Example 3 (java):
```java
@Getter@AllArgsConstructorpublic enum AgeEnum implements IEnum<Integer> {    ONE(1, "一岁"),    TWO(2, "二岁"),    THREE(3, "三岁");
    private final int value;    private final String desc;
    @Override    public Integer getValue() {        return this.value;    }}
```

Example 4 (unknown):
```unknown
mybatis-plus:  configuration:    default-enum-type-handler: xx.xx.xx.MyEnumTypeHandler
```

---

## 乐观锁插件 | MyBatis-Plus

**URL:** https://baomidou.com/plugins/optimistic-locker/

**Contents:**
- 乐观锁插件
- 乐观锁的实现原理
- 配置乐观锁插件
  - 1. 配置插件
    - Spring XML 方式
    - Spring Boot 注解方式
  - 2. 在实体类字段上添加 @Version 注解
- 注意事项
- 示例

乐观锁是一种并发控制机制，用于确保在更新记录时，该记录未被其他事务修改。MyBatis-Plus 提供了 OptimisticLockerInnerInterceptor 插件，使得在应用中实现乐观锁变得简单。

在实体类中，需要在表示版本号的字段上添加 @Version 注解：

以下是一个完整的 Spring Boot 配置示例：

通过以上配置和实体类中的 @Version 注解，你就可以在 MyBatis-Plus 应用中轻松实现乐观锁，有效防止并发更新时的数据冲突。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (jsx):
```jsx
<bean id="optimisticLockerInnerInterceptor" class="com.baomidou.mybatisplus.extension.plugins.inner.OptimisticLockerInnerInterceptor"/>
<bean id="mybatisPlusInterceptor" class="com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor">    <property name="interceptors">        <list>            <ref bean="optimisticLockerInnerInterceptor"/>        </list>    </property></bean>
```

Example 2 (java):
```java
@Configuration@MapperScan("按需修改")public class MybatisPlusConfig {
    @Bean    public MybatisPlusInterceptor mybatisPlusInterceptor() {        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());        return interceptor;    }}
```

Example 3 (csharp):
```csharp
import com.baomidou.mybatisplus.annotation.Version;
public class YourEntity {    @Version    private Integer version;    // 其他字段...}
```

Example 4 (java):
```java
@Configuration@MapperScan("com.yourpackage.mapper")public class MybatisPlusConfig {
    @Bean    public MybatisPlusInterceptor mybatisPlusInterceptor() {        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());        return interceptor;    }}
```

---

## 自动填充字段 | MyBatis-Plus

**URL:** https://baomidou.com/guides/auto-fill-field

**Contents:**
- 自动填充字段
- 原理概述
- 使用步骤
  - 1. 定义实体类
  - 2. 实现 MetaObjectHandler
  - 3. 配置自动填充处理器
- 注意事项
- 参数填充示例
- 无法填充示例
- FieldFill 枚举

MyBatis-Plus 提供了一个便捷的自动填充功能，用于在插入或更新数据时自动填充某些字段，如创建时间、更新时间等。以下是如何使用这一功能的详细说明。

自动填充功能通过实现 com.baomidou.mybatisplus.core.handlers.MetaObjectHandler 接口来实现。你需要创建一个类来实现这个接口，并在其中定义插入和更新时的填充逻辑。

在实体类中，你需要使用 @TableField 注解来标记哪些字段需要自动填充，并指定填充的策略。

创建一个类来实现 MetaObjectHandler 接口，并重写 insertFill 和 updateFill 方法。

确保你的 MyMetaObjectHandler 类被 Spring 管理，可以通过 @Component 或 @Bean 注解来实现。

通过以上步骤，你可以轻松地在 MyBatis-Plus 中实现自动填充功能，提高开发效率。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (csharp):
```csharp
public class User {    @TableField(fill = FieldFill.INSERT)    private String createTime;
    @TableField(fill = FieldFill.UPDATE)    private String updateTime;
    // 其他字段...}
```

Example 2 (java):
```java
// java example@Slf4j@Componentpublic class MyMetaObjectHandler implements MetaObjectHandler {
    @Override    public void insertFill(MetaObject metaObject) {        log.info("开始插入填充...");        this.strictInsertFill(metaObject, "createUserId", Long.class, 123456L)        this.strictInsertFill(metaObject, "createTime", LocalDateTime.class, LocalDateTime.now());    }
    @Override    public void updateFill(MetaObject metaObject) {        log.info("开始更新填充...");        this.strictInsertFill(metaObject, "updateUserId", Long.class, 123456L)        this.strictUpdateFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now());    }}
```

Example 3 (java):
```java
// kotlin example@Componentclass MyMetaObjectHandler : MetaObjectHandler {
    private val log = LoggerFactory.getLogger(MyMetaObjectHandler::class.java)
    // 注意将kotlin类型转为java类型请使用 xxx::class.javaObjectType，防止部分类型使用xxx::class.java转换为基本类型导致类型不一致无法填充    override fun insertFill(metaObject: MetaObject) {        log.info("开始插入填充...");        this.strictInsertFill(metaObject, "createUserId", Long::class.javaObjectType, 123456L)        this.strictInsertFill(metaObject, "createTime", LocalDateTime::class.javaObjectType, LocalDateTime.now())    }
    override fun updateFill(metaObject: MetaObject) {        log.info("开始更新填充...");        this.strictUpdateFill(metaObject, "updateUserId", Long::class.javaObjectType, 123456L)        this.strictUpdateFill(metaObject, "updateTime", LocalDateTime::class.javaObjectType, LocalDateTime.now())    }
}
```

Example 4 (typescript):
```typescript
// 插入填充示例insertFillByCustomMethod1(H2User h2User);insertFillByCustomMethod8(H2User[] h2Users);insertFillByCustomMethod4(Collection<H2User> h2User);
// 更新填充示例updateFillByCustomMethod2(@Param("coll") Collection<Long> ids, @Param("et") H2User h2User);updateFillByCustomMethod4(@Param("colls") Collection<Long> ids, @Param("et") H2User h2User);
```

---

## 字段类型处理器 | MyBatis-Plus

**URL:** https://baomidou.com/guides/type-handler/

**Contents:**
- 字段类型处理器
- JSON 字段类型处理器
  - 配置
  - XML 配置对应写法
  - Wrapper 查询中的 TypeHandler 使用
- 自定义类型处理器
  - 创建自定义类型处理器
  - 使用自定义类型处理器

在 MyBatis 中，类型处理器（TypeHandler）扮演着 JavaType 与 JdbcType 之间转换的桥梁角色。它们用于在执行 SQL 语句时，将 Java 对象的值设置到 PreparedStatement 中，或者从 ResultSet 或 CallableStatement 中取出值。

MyBatis-Plus 给大家提供了提供了一些内置的类型处理器，可以通过 TableField 注解快速注入到 MyBatis 容器中，从而简化开发过程。

示例工程：👉 mybatis-plus-sample-typehandler

MyBatis-Plus 内置了多种 JSON 类型处理器，包括 AbstractJsonTypeHandler 及其子类 Fastjson2TypeHandler、FastjsonTypeHandler、GsonTypeHandler、JacksonTypeHandler 等。这些处理器可以将 JSON 字符串与 Java 对象相互转换。

在 XML 映射文件中，可以使用 <result> 元素来指定字段的类型处理器。

从 MyBatis-Plus 3.5.3.2 版本开始，可以在 Wrapper 查询中直接使用 TypeHandler。

通过上述示例，你可以看到 MyBatis-Plus 提供了灵活且强大的类型处理器支持，使得在处理复杂数据类型时更加便捷。在使用时，请确保选择了正确的 JSON 处理器，并引入了相应的 JSON 解析库依赖。

在 MyBatis-Plus 中，除了使用内置的类型处理器外，开发者还可以根据需要自定义类型处理器。

例如，当使用 PostgreSQL 数据库时，可能会遇到 JSONB 类型的字段，这时可以创建一个自定义的类型处理器来处理 JSONB 数据。

以下是一个自定义的 JSONB 类型处理器的示例：

示例工程：👉 mybatis-plus-sample-jsonb

在实体类中，通过 TableField 注解指定自定义的类型处理器：

通过上述步骤，你可以在 MyBatis-Plus 中使用自定义的 JSONB 类型处理器来处理 PostgreSQL 数据库中的 JSONB 类型字段。自定义类型处理器提供了极大的灵活性，使得开发者可以根据具体的数据库特性和业务需求来定制数据处理逻辑。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (csharp):
```csharp
@Data@Accessors(chain = true)@TableName(autoResultMap = true)public class User {    private Long id;
    ...
    /**     * 必须开启映射注解     *     * @TableName(autoResultMap = true)     *     * 选择对应的 JSON 处理器，并确保存在对应的 JSON 解析依赖包     */    @TableField(typeHandler = JacksonTypeHandler.class)    // 或者使用 FastjsonTypeHandler    // @TableField(typeHandler = FastjsonTypeHandler.class)    private OtherInfo otherInfo;}
```

Example 2 (sql):
```sql
<!-- 单个字段的类型处理器配置 --><result column="other_info" jdbcType="VARCHAR" property="otherInfo" typeHandler="com.baomidou.mybatisplus.extension.handlers.JacksonTypeHandler" />
<!-- 多个字段中某个字段的类型处理器配置 --><resultMap id="departmentResultMap" type="com.baomidou...DepartmentVO">    <result property="director" column="director" typeHandler="com.baomidou.mybatisplus.extension.handlers.JacksonTypeHandler" /></resultMap><select id="selectPageVO" resultMap="departmentResultMap">   select id,name,director from department ...</select>
```

Example 3 (typescript):
```typescript
Wrappers.<H2User>lambdaQuery()    .apply("name={0,typeHandler=" + H2userNameJsonTypeHandler.class.getCanonicalName() + "}", "{\"id\":101,\"name\":\"Tomcat\"}"))
```

Example 4 (java):
```java
import com.baomidou.mybatisplus.extension.handlers.JacksonTypeHandler;import org.apache.ibatis.type.JdbcType;import org.apache.ibatis.type.MappedJdbcTypes;import org.apache.ibatis.type.MappedTypes;import org.postgresql.util.PGobject;
import java.sql.CallableStatement;import java.sql.PreparedStatement;import java.sql.ResultSet;import java.sql.SQLException;
@MappedTypes({Object.class})@MappedJdbcTypes(JdbcType.VARCHAR)public class JsonbTypeHandler<T> extends JacksonTypeHandler<T> {
    private final Class<T> clazz;
    public JsonbTypeHandler(Class<T> clazz) {        if (clazz == null) {            throw new IllegalArgumentException("Type argument cannot be null");        }        this.clazz = clazz;    }
    // 自3.5.6版本开始支持泛型,需要加上此构造.    public JsonbTypeHandler(Class<?> type, Field field) {        super(type, field);    }
    @Override    public void setNonNullParameter(PreparedStatement ps, int i, T parameter, JdbcType jdbcType) throws SQLException {        PGobject jsonbObject = new PGobject();        jsonbObject.setType("jsonb");        jsonObject.setValue(toJson(parameter));        ps.setObject(i, jsonbObject);    }}
```

---
