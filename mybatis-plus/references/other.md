# Mybatis-Plus - Other

**Pages:** 22

---

## 多数据源支持 | MyBatis-Plus

**URL:** https://baomidou.com/guides/dynamic-datasource/

**Contents:**
- 多数据源支持
- dynamic-datasource
  - 特性
  - 约定
  - 使用方法
- mybatis-mate
  - 特性
  - 使用方法

随着项目规模的扩大，单一数据源已无法满足复杂业务需求，多数据源（动态数据源）应运而生。本文将介绍两种 MyBatis-Plus 的多数据源扩展插件：开源生态的 dynamic-datasource 和 企业级生态的 mybatis-mate。

dynamic-datasource 是一个开源的 Spring Boot 多数据源启动器，提供了丰富的功能，包括数据源分组、敏感信息加密、独立初始化表结构等。

更多使用教程请参考Dynamic-Datasource 官网

mybatis-mate 是一个 MyBatis-Plus 的付费企业组件，内置很多好用的高级特性，其中包括多数据源扩展组件，提供了高效简单的多数据源支持。

多数据源动态加载卸载：👉 mybatis-mate-sharding-dynamic

多数据源事务（jta atomikos）：👉 mybatis-mate-sharding-jta-atomikos

通过上述介绍，我们可以看到 dynamic-datasource 和 mybatis-mate 都提供了强大的多数据源支持，开发者可以根据项目需求选择合适的插件来实现数据源的灵活管理。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (typescript):
```typescript
<dependency>  <groupId>com.baomidou</groupId>  <artifactId>dynamic-datasource-spring-boot-starter</artifactId>  <version>${version}</version></dependency>
```

Example 2 (typescript):
```typescript
<dependency> <groupId>com.baomidou</groupId> <artifactId>dynamic-datasource-spring-boot3-starter</artifactId> <version>${version}</version></dependency>
```

Example 3 (yaml):
```yaml
spring:  datasource:    dynamic:      primary: master      strict: false      datasource:        master:          url: jdbc:mysql://xx.xx.xx.xx:3306/dynamic          username: root          password: 123456          driver-class-name: com.mysql.jdbc.Driver        slave_1:          url: jdbc:mysql://xx.xx.xx.xx:3307/dynamic          username: root          password: 123456          driver-class-name: com.mysql.jdbc.Driver        slave_2:          url: ENC(xxxxx)          username: ENC(xxxxx)          password: ENC(xxxxx)          driver-class-name: com.mysql.jdbc.Driver
```

Example 4 (java):
```java
@Service@DS("slave")public class UserServiceImpl implements UserService {
  @Autowired  private JdbcTemplate jdbcTemplate;
  @Override  @DS("slave_1")  public List selectByCondition() {    return jdbcTemplate.queryForList("select * from user where age >10");  }}
```

---

## PIG 微服务开发平台 | MyBatis-Plus

**URL:** https://baomidou.com/resources/pig

**Contents:**
- PIG 微服务开发平台
- 联系信息
  - 产品荣誉
  - 产品概述
  - 产品交付
  - 限时优惠

PIG 商业版应用微服务、容器、DevOps 等云原生技术，封装了大量技术开发包、技术应用组件、技术场景实现能力，并支持 SaaS 模式应用，提供了一个可支持企业各业务系统或产品快速开发实现的微服务应用数字化融合平台，富含各类开箱即用的组件、微服务业务系统，助力企业跨越 Cloud（IaaS/PaaS）与自身数字化的鸿沟，共享业务服务的组合重用，为企业服务化中台整合、数字化转型提供强力支撑，也为企业提供了最佳架构实践。

基于广泛的企业业务场景，沉淀与提供面向业务场景的可复用技术应用能力，以产品的思维来打造的为企业提供能力复用的企业数字化中台。我们具备如下优势：

面向企业级应用的成熟技术平台：平台采用了 Java 主流的微服务技术栈，采用的技术组件成熟度较高，市面上人员储备丰富，便于招募技术人员。同时基于平台做了很多面向企业级应用的业务中台、实施了很多项目，本身有大量实践经验，应用上很成熟。

大量的业务场景落地沉淀：通过基于平台的产品、开发项目的实施，在供应商关系管理、合同管理、人力资源管理、项目管理、资产管理、订单管理、电商商城等众多业务领域获得了大量的落地经验，沉淀了很多共享业务中台服务。

已沉淀可复用的技术应用能力：通过大量的项目实施、业务场景落地，沉淀了大量通用的技术应用组件/服务，如支付服务、消息服务、连接服务等能力，并能够快速配置、复用到新的业务场景中。

多个行业领域实践：平台已在零售、汽车、钢铁、电商、房地产等行业具有众多的落地实施经验，并持续在更多领域进行应用。

MyBatis-Mate + PigX 联合授权限时折扣，一份钱享受双倍快乐，欢迎微信扫码咨询

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

---

## 支持与赞助 | MyBatis-Plus

**URL:** https://baomidou.com/resources/support/

**Contents:**
- 支持与赞助
- 用爱发电
  - 成为赞助商
- 致谢

如果您正在使用这个项目并感觉良好，或者是想支持我们继续开发，您可以通过如下任意方式支持我们：

感谢给予支持的朋友，您的支持是我们前进的动力 🎉

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

---

## 插件主体 | MyBatis-Plus

**URL:** https://baomidou.com/plugins/

**Contents:**
- 插件主体
- MybatisPlusInterceptor 概览
  - 属性
  - InnerInterceptor 接口
- 使用示例
  - Spring 配置
  - Spring Boot 配置
  - mybatis-config.xml 配置
- 拦截忽略注解 @InterceptorIgnore
- 手动设置拦截器忽略执行策略

MyBatis-Plus 提供了一系列强大的插件来增强 MyBatis 的功能，这些插件通过 MybatisPlusInterceptor 来实现对 MyBatis 执行过程的拦截和增强。以下是这些插件的详细介绍和使用方法。

MybatisPlusInterceptor 是 MyBatis-Plus 的核心插件，它代理了 MyBatis 的 Executor#query、Executor#update 和 StatementHandler#prepare 方法，允许在这些方法执行前后插入自定义逻辑。

MybatisPlusInterceptor 有一个关键属性 interceptors，它是一个 List<InnerInterceptor> 类型的集合，用于存储所有要应用的内部拦截器。

所有 MyBatis-Plus 提供的插件都实现了 InnerInterceptor 接口，这个接口定义了插件的基本行为。目前，MyBatis-Plus 提供了以下插件：

使用多个插件时，需要注意它们的顺序。建议的顺序是：

总结：对 SQL 进行单次改造的插件应优先放入，不对 SQL 进行改造的插件最后放入。

在 Spring 配置中，你需要创建 MybatisPlusInterceptor 的实例，并将它添加到 MyBatis 的插件列表中。以下是一个分页插件的配置示例：

在 Spring Boot 项目中，你可以通过 Java 配置来添加插件：

如果你使用的是 XML 配置，可以在 mybatis-config.xml 中添加插件：

@InterceptorIgnore 注解可以用来忽略某些插件的拦截。该注解有多个属性，分别对应不同的插件。如果某个属性没有值，则默认为 false，表示不忽略该插件；如果设置为 true，则忽略对应的插件。

从 3.5.3 版本开始，你可以手动设置拦截器的忽略执行策略，这比注解更加灵活。但是，你需要手动关闭调用方法。

MyBatis-Plus 支持本地缓存 SQL 解析，这对于分页、租户等插件特别有效。你可以通过静态代码块来设置缓存处理类：

自3.5.6开始，对JSQLParser(4.9) 支持了线程池解析复用，可减少重复创建线程池带来的性能开销。

默认创建固定线程池核心数: (Runtime.getRuntime().availableProcessors() + 1) / 2

如果默认的线程池方式不太符合你实际部署情况，请用下面的方式指定你的自定义线程池，自行创建的线程池需要注意自行关闭。

如果需要JsqlParser的sql语句进行加工处理，请通过下面的方式进行指定，处理完成 sql 字符串再交由解析器进行解析。

以上是 MyBatis-Plus 插件主体的详细介绍和使用方法。通过这些插件，你可以大大增强 MyBatis 的功能，提高开发效率。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (jsx):
```jsx
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">    <!-- 其他属性 略 -->    <property name="configuration" ref="configuration"/>    <property name="plugins">        <array>            <ref bean="mybatisPlusInterceptor"/>        </array>    </property></bean>
<bean id="mybatisPlusInterceptor" class="com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor">    <property name="interceptors">        <list>            <ref bean="paginationInnerInterceptor"/>        </list>    </property></bean>
<bean id="paginationInnerInterceptor" class="com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor">    <!-- 对于单一数据库类型来说,都建议配置该值,避免每次分页都去抓取数据库类型 -->    <constructor-arg name="dbType" value="H2"/></bean>
```

Example 2 (java):
```java
@Configuration@MapperScan("scan.your.mapper.package")public class MybatisPlusConfig {
    /**     * 添加分页插件     */    @Bean    public MybatisPlusInterceptor mybatisPlusInterceptor() {        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.H2));        return interceptor;    }}
```

Example 3 (jsx):
```jsx
<plugins>  <plugin interceptor="com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor">    <property name="@page" value="com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor"/>    <property name="page:dbType" value="h2"/>  </plugin></plugins>
```

Example 4 (unknown):
```unknown
// 请尽量使用 try finally 的方式来保证能正确得到关闭try {    // 设置忽略租户插件    InterceptorIgnoreHelper.handle(IgnoreStrategy.builder().tenantLine(true).build());    // 执行逻辑 ..} finally {    // 关闭忽略策略  InterceptorIgnoreHelper.clearIgnoreStrategy();}
```

---

## 插件主体 | MyBatis-Plus

**URL:** https://baomidou.com/plugins

**Contents:**
- 插件主体
- MybatisPlusInterceptor 概览
  - 属性
  - InnerInterceptor 接口
- 使用示例
  - Spring 配置
  - Spring Boot 配置
  - mybatis-config.xml 配置
- 拦截忽略注解 @InterceptorIgnore
- 手动设置拦截器忽略执行策略

MyBatis-Plus 提供了一系列强大的插件来增强 MyBatis 的功能，这些插件通过 MybatisPlusInterceptor 来实现对 MyBatis 执行过程的拦截和增强。以下是这些插件的详细介绍和使用方法。

MybatisPlusInterceptor 是 MyBatis-Plus 的核心插件，它代理了 MyBatis 的 Executor#query、Executor#update 和 StatementHandler#prepare 方法，允许在这些方法执行前后插入自定义逻辑。

MybatisPlusInterceptor 有一个关键属性 interceptors，它是一个 List<InnerInterceptor> 类型的集合，用于存储所有要应用的内部拦截器。

所有 MyBatis-Plus 提供的插件都实现了 InnerInterceptor 接口，这个接口定义了插件的基本行为。目前，MyBatis-Plus 提供了以下插件：

使用多个插件时，需要注意它们的顺序。建议的顺序是：

总结：对 SQL 进行单次改造的插件应优先放入，不对 SQL 进行改造的插件最后放入。

在 Spring 配置中，你需要创建 MybatisPlusInterceptor 的实例，并将它添加到 MyBatis 的插件列表中。以下是一个分页插件的配置示例：

在 Spring Boot 项目中，你可以通过 Java 配置来添加插件：

如果你使用的是 XML 配置，可以在 mybatis-config.xml 中添加插件：

@InterceptorIgnore 注解可以用来忽略某些插件的拦截。该注解有多个属性，分别对应不同的插件。如果某个属性没有值，则默认为 false，表示不忽略该插件；如果设置为 true，则忽略对应的插件。

从 3.5.3 版本开始，你可以手动设置拦截器的忽略执行策略，这比注解更加灵活。但是，你需要手动关闭调用方法。

MyBatis-Plus 支持本地缓存 SQL 解析，这对于分页、租户等插件特别有效。你可以通过静态代码块来设置缓存处理类：

自3.5.6开始，对JSQLParser(4.9) 支持了线程池解析复用，可减少重复创建线程池带来的性能开销。

默认创建固定线程池核心数: (Runtime.getRuntime().availableProcessors() + 1) / 2

如果默认的线程池方式不太符合你实际部署情况，请用下面的方式指定你的自定义线程池，自行创建的线程池需要注意自行关闭。

如果需要JsqlParser的sql语句进行加工处理，请通过下面的方式进行指定，处理完成 sql 字符串再交由解析器进行解析。

以上是 MyBatis-Plus 插件主体的详细介绍和使用方法。通过这些插件，你可以大大增强 MyBatis 的功能，提高开发效率。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (jsx):
```jsx
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">    <!-- 其他属性 略 -->    <property name="configuration" ref="configuration"/>    <property name="plugins">        <array>            <ref bean="mybatisPlusInterceptor"/>        </array>    </property></bean>
<bean id="mybatisPlusInterceptor" class="com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor">    <property name="interceptors">        <list>            <ref bean="paginationInnerInterceptor"/>        </list>    </property></bean>
<bean id="paginationInnerInterceptor" class="com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor">    <!-- 对于单一数据库类型来说,都建议配置该值,避免每次分页都去抓取数据库类型 -->    <constructor-arg name="dbType" value="H2"/></bean>
```

Example 2 (java):
```java
@Configuration@MapperScan("scan.your.mapper.package")public class MybatisPlusConfig {
    /**     * 添加分页插件     */    @Bean    public MybatisPlusInterceptor mybatisPlusInterceptor() {        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.H2));        return interceptor;    }}
```

Example 3 (jsx):
```jsx
<plugins>  <plugin interceptor="com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor">    <property name="@page" value="com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor"/>    <property name="page:dbType" value="h2"/>  </plugin></plugins>
```

Example 4 (unknown):
```unknown
// 请尽量使用 try finally 的方式来保证能正确得到关闭try {    // 设置忽略租户插件    InterceptorIgnoreHelper.handle(IgnoreStrategy.builder().tenantLine(true).build());    // 执行逻辑 ..} finally {    // 关闭忽略策略  InterceptorIgnoreHelper.clearIgnoreStrategy();}
```

---

## MyBatis-Plus 🚀 为简化开发而生

**URL:** https://baomidou.com/

**Contents:**
- MyBatis-Plus
  - 特性
  - 赞助商
  - 当前最新版本
  - 苞米豆生态圈
  - 致谢
  - 代码托管
  - 参与贡献
  - 教程、案例、使用者名单
  - 友情链接

只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑。

只需简单配置，即可快速进行单表 CRUD 操作，从而节省大量时间。

代码生成、自动分页、逻辑删除、自动填充、拦截器等功能一应俱全。

连续 5 年获得开源中国年度最佳开源项目殊荣，Github 累计 16K Star。

为中国特色审批流打造的国产JSON流程引擎

MyBatis-Plus 已连续 5 年（2017、2018、2019、2020、2021）获得“OSC 年度最受欢迎中国开源软件”殊荣，感谢各位支持者的一路同行，我们会秉承 【为简化开发而生】 这一理念砥砺前行！

欢迎各路好汉一起来参与完善 MyBatis-Plus，我们期待你的 PR！

请移步至 Awesome-MyBatis-Plus 查看。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (typescript):
```typescript
<dependency>    <groupId>com.baomidou</groupId>    <artifactId>mybatis-plus</artifactId>    <version>3.5.15</version></dependency>
```

Example 2 (json):
```json
implementation 'com.baomidou:mybatis-plus:3.5.15'
```

---

## 防全表更新与删除插件 | MyBatis-Plus

**URL:** https://baomidou.com/plugins/block-attack/

**Contents:**
- 防全表更新与删除插件
- 功能特性
- 使用方法
- 测试示例
  - 全表更新测试
  - 部分更新测试

BlockAttackInnerInterceptor 是 MyBatis-Plus 框架提供的一个安全插件，专门用于防止恶意的全表更新和删除操作。该插件通过拦截 update 和 delete 语句，确保这些操作不会无意中影响到整个数据表，从而保护数据的完整性和安全性。

以下测试示例展示了如何使用 BlockAttackInnerInterceptor 来防止全表更新操作。

以下测试示例展示了如何正确地执行部分更新操作，插件不会对此类操作进行拦截。

BlockAttackInnerInterceptor 插件是 MyBatis-Plus 提供的一个重要的安全工具，它能够有效地防止全表更新和删除操作，保护数据库免受意外或恶意的数据破坏。通过合理配置和使用该插件，可以显著提高应用程序的数据安全性。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (php):
```php
@Configurationpublic class MybatisPlusConfig {
    @Bean    public MybatisPlusInterceptor mybatisPlusInterceptor() {        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();        interceptor.addInnerInterceptor(new BlockAttackInnerInterceptor());        return interceptor;    }}
```

Example 2 (java):
```java
@SpringBootTestpublic class QueryWrapperTest {
    @Autowired    private UserService userService;
    /**     * SQL：UPDATE user  SET name=?,email=?;     */    @Test    public void testFullUpdate() {        User user = new User();        user.setId(999L);        user.setName("custom_name");        user.setEmail("xxx@mail.com");        // 由于没有指定更新条件，插件将抛出异常        // com.baomidou.mybatisplus.core.exceptions.MybatisPlusException: Prohibition of table update operation        Assertions.assertThrows(MybatisPlusException.class, () -> {            userService.saveOrUpdate(user, null);        });    }}
```

Example 3 (java):
```java
@SpringBootTestpublic class QueryWrapperTest {
    @Autowired    private UserService userService;
    /**     * SQL：UPDATE user  SET name=?, email=? WHERE id = ?;     */    @Test    public void testPartialUpdate() {        LambdaUpdateWrapper<User> wrapper = new LambdaUpdateWrapper<>();        wrapper.eq(User::getId, 1);        User user = new User();        user.setId(10L);        user.setName("custom_name");        user.setEmail("xxx@mail.com");        // 由于指定了更新条件，插件不会拦截此操作        userService.saveOrUpdate(user, wrapper);    }}
```

---

## 开源生态 | MyBatis-Plus

**URL:** https://baomidou.com/resources/eco-system/

**Contents:**
- 开源生态
- Baomidou 开源团队其余作品
- 更多生态资源
- 团队荣耀

请移步至 Awesome-MyBatis-Plus 查看。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

---

## MyBatis-Mate | MyBatis-Plus

**URL:** https://baomidou.com/resources/mybatis-mate

**Contents:**
- MyBatis-Mate

提供企业级的高级特性，旨在更敏捷优雅处理数据。使用教程

MyBatis-Mate + PigX 联合授权限时折扣，一份钱享受双倍快乐，欢迎微信扫码咨询

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

---

## Gmail - Request for use MyBatis Ninja LOGO which with blue color

**URL:** https://baomidou.com/document/logo-request.html

---

## 更新日志 | MyBatis-Plus

**URL:** https://baomidou.com/resources/changlog/

**Contents:**
- 更新日志
- [v3.5.15] 2025.11.30
- [v3.5.14] 2025.08.29
- [v3.5.13] 2025.08.29
- [v3.5.12] 2025.04.27
- [v3.5.11] 2025.03.23
- [v3.5.10.1] 2025.01.13
- [v3.5.10] 2025.01.12
- [v3.5.9] 2024.10.23
- [v3.5.8] 2024.09.18

由于jsqlParser5.0版本与5.1版本升级不兼容性不是很大，计划后期移除mybatis-plus-jsqlparser-5.0支持模块。 多版本支持相对来说比较麻烦，后期只维护mybatis-plus-jsqlparser-4.9 与 mybatis-plus-jsqlparser(保持最新版跟进,直到再提升jdk)

####上个版本（2.0.9）升级导致的问题

###Mybatis-Plus-Boot-Start [1.0.4]

###Mybaits-Plus ####主体功能

###Mybatis-Plus-Boot-Start [1.0.2] 代号：清风 ####主体功能

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (unknown):
```unknown
// 不进行 count sql 优化page.setOptimizeCountSql(false);
```

Example 2 (sql):
```sql
例如：@TableField(.. , update="%s+1") 其中 %s 会填充为字段 输出 SQL 为：update 表 set 字段=字段+1 where ...
```

Example 3 (sql):
```sql
例如：@TableField(.. , update="now()") 使用数据库时间 输出 SQL 为：update 表 set 字段=now() where ...
```

Example 4 (sql):
```sql
@TableField(condition = SqlCondition.LIKE)private String name;输出 SQL 为：select 表 where name LIKE CONCAT('%',值,'%')
```

---

## 多租户插件 | MyBatis-Plus

**URL:** https://baomidou.com/plugins/tenant/

**Contents:**
- 多租户插件
- 示例工程
- 属性介绍
- 使用方法
  - 步骤 1：实现租户处理器
  - 步骤 2：将租户处理器注入插件
- 本地缓存 SQL 解析
- 插入时自动添加租户字段
- 注意事项

TenantLineInnerInterceptor 是 MyBatis-Plus 提供的一个插件，用于实现多租户的数据隔离。通过这个插件，可以确保每个租户只能访问自己的数据，从而实现数据的安全隔离。

为了更好地理解如何使用 TenantLineInnerInterceptor，你可以参考官方提供的示例工程：👉 mybatis-plus-sample-tenant

TenantLineInnerInterceptor 的关键属性是 tenantLineHandler，它是一个 TenantLineHandler 接口的实例，用于处理租户相关的逻辑。

TenantLineHandler 接口定义了以下方法：

实现 TenantLineHandler 接口，创建一个租户处理器。在这个例子中，我们假设每个租户都有一个唯一的 tenantId，并且我们通过请求头来获取当前租户的 tenantId。

将自定义的租户处理器注入到 TenantLineInnerInterceptor 中：

通过以上步骤，你已经成功地在 Spring Boot 项目中配置了多租户插件，并实现了一个简单的租户处理器。现在，你的应用将能够根据当前请求的租户ID自动处理多租户数据隔离。

请注意，实际应用中，获取租户ID的方式可能会有所不同，这取决于你的应用架构和业务需求。此外，确保在处理租户ID时考虑到安全性，避免潜在的安全风险。

为了提高性能，MyBatis-Plus 支持本地缓存 SQL 解析。你可以通过以下方式设置缓存处理类：

默认插入 SQL 是需要判断租户条件，因此需要配合自动填充字段功能填充租户字段，否则租户字段不会自动保存到数据库。

通过以上配置和使用方法，你可以在 MyBatis-Plus 应用中实现多租户的数据隔离，确保每个租户的数据安全。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (typescript):
```typescript
public interface TenantLineHandler {
    /**     * 获取租户 ID 值表达式，只支持单个 ID 值     *     * @return 租户 ID 值表达式     */    Expression getTenantId();
    /**     * 获取租户字段名     * 默认字段名叫: tenant_id     *     * @return 租户字段名     */    default String getTenantIdColumn() {        return "tenant_id";    }
    /**     * 根据表名判断是否忽略拼接多租户条件     * 默认都要进行解析并拼接多租户条件     *     * @param tableName 表名     * @return 是否忽略, true:表示忽略，false:需要解析并拼接多租户条件     */    default boolean ignoreTable(String tableName) {        return false;    }
    /**     * 忽略插入租户字段逻辑     *     * @param columns        插入字段     * @param tenantIdColumn 租户 ID 字段     * @return     */    default boolean ignoreInsert(List<Column> columns, String tenantIdColumn) {        return columns.stream().map(Column::getColumnName).anyMatch(i -> i.equalsIgnoreCase(tenantIdColumn));    }}
```

Example 2 (java):
```java
@Componentpublic class CustomTenantHandler implements TenantLineHandler {
    @Override    public Expression getTenantId() {        // 假设有一个租户上下文，能够从中获取当前用户的租户         Long tenantId = TenantContextHolder.getCurrentTenantId();        // 返回租户ID的表达式，LongValue 是 JSQLParser 中表示 bigint 类型的 class        return new LongValue(tenantId);;    }
    @Override    public String getTenantIdColumn() {        return "tenant_id";    }
    @Override    public boolean ignoreTable(String tableName) {        // 根据需要返回是否忽略该表        return false;    }
}
```

Example 3 (java):
```java
@Configuration@MapperScan("com.yourpackage.mapper")public class MybatisPlusConfig {
    @Autowired    private CustomTenantHandler customTenantHandler;
    @Bean    public MybatisPlusInterceptor mybatisPlusInterceptor() {        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();        TenantLineInnerInterceptor tenantInterceptor = new TenantLineInnerInterceptor();        tenantInterceptor.setTenantLineHandler(customTenantHandler);        interceptor.addInnerInterceptor(tenantInterceptor);        return interceptor;    }}
```

Example 4 (php):
```php
static {    // 默认支持序列化 FstSerialCaffeineJsqlParseCache，JdkSerialCaffeineJsqlParseCache    JsqlParserGlobal.setJsqlParseCache(new JdkSerialCaffeineJsqlParseCache(      (cache) -> cache.maximumSize(1024)      .expireAfterWrite(5, TimeUnit.SECONDS))    );}
```

---

## 企业级生态 | MyBatis-Plus

**URL:** https://baomidou.com/resources/enterprise-eco-system/

**Contents:**
- 企业级生态
- 企业级平台朋友圈

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

---

## SQL注入器 | MyBatis-Plus

**URL:** https://baomidou.com/guides/sql-injector

**Contents:**
- SQL注入器
- 注入器配置
- 自定义全局方法攻略
  - 步骤 1: 定义SQL
  - 步骤 2: 注册自定义方法
  - 步骤 3: 定义BaseMapper
  - 步骤 4: 配置SqlInjector
    - 在 application.yml 中配置
    - 在 application.properties 中配置
  - 注意事项

MyBatis-Plus 提供了灵活的机制来注入自定义的 SQL 方法，这通过 sqlInjector 全局配置实现。通过实现 ISqlInjector 接口或继承 AbstractSqlInjector 抽象类，你可以注入自定义的通用方法到 MyBatis 容器中。

SQL注入器允许开发者扩展和定制SQL语句的生成，以适应特定的业务逻辑和查询需求。以下是SQL注入器的一些示例使用场景和它能实现的功能：

自定义查询方法：当标准的CRUD操作无法满足复杂的查询需求时，可以通过SQL注入器添加自定义的查询方法。

复杂数据处理：在需要进行复杂的数据处理，如多表联结、子查询、聚合函数等时，SQL注入器可以帮助生成相应的SQL语句。

性能优化：通过自定义SQL语句，可以针对特定的查询场景进行性能优化。

数据权限控制：在需要根据用户权限动态生成SQL语句时，SQL注入器可以用来实现数据权限的控制。

遗留系统迁移：在将遗留系统迁移到MyBatis-Plus时，可能需要保留原有的SQL语句结构，SQL注入器可以帮助实现这一过渡。

注入自定义SQL方法：通过实现ISqlInjector接口，可以注入自定义的SQL方法到MyBatis容器中，这些方法可以是任何复杂的SQL查询。

扩展BaseMapper：可以在继承BaseMapper的基础上，通过SQL注入器添加额外的查询方法，这些方法将自动被MyBatis-Plus识别和使用。

灵活的SQL生成：SQL注入器提供了灵活的SQL生成机制，可以根据业务需求生成各种SQL语句，包括但不限于SELECT、INSERT、UPDATE、DELETE等。

集成第三方数据库功能：如果需要使用数据库的特定功能，如存储过程、触发器等，SQL注入器可以帮助生成调用这些功能的SQL语句。

动态SQL支持：在某些场景下，SQL语句需要根据运行时的条件动态生成，SQL注入器可以支持这种动态SQL的生成。

通过SQL注入器，MyBatis-Plus提供了一个强大的扩展点，使得开发者能够根据项目的具体需求，灵活地定制和优化SQL语句，从而提高应用的性能和适应性。

在MyBatis-Plus中，sqlInjector 配置是一个全局配置项，用于指定一个实现了 ISqlInjector 接口的类，该类负责将自定义的SQL方法注入到MyBatis的Mapper接口中。

默认的注入器实现是 DefaultSqlInjector，你可以参考它来创建自己的注入器。

以下是如何配置 sqlInjector 的示例。

根据提供的参考信息，我们可以看到如何在MyBatis-Plus中实现自定义的全局方法，包括逻辑删除、自动填充以及自定义的insert和insertBatch方法。下面是一个更详细的步骤说明和示例代码：

首先，你需要定义自定义方法的SQL语句。这通常在继承了AbstractMethod的类中完成，例如MysqlInsertAllBatch。

接下来，你需要创建一个类来继承DefaultSqlInjector，并重写getMethodList方法来注册你的自定义方法。

然后，你需要在你的BaseMapper接口中定义自定义的方法。

最后，你需要在配置文件中指定你的自定义SQL注入器。

通过以上步骤，你就可以在MyBatis-Plus中成功地实现自定义的全局方法了。记得在实际使用中，根据你的业务需求调整SQL语句和方法的实现。

参考 自定义 BaseMapper 示例，你可以找到如何创建自定义的 SQL 注入器和如何在项目中使用它们的详细步骤。

通过这种方式，MyBatis-Plus 允许你扩展其功能，以满足特定的业务需求，同时保持代码的整洁和可维护性。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (typescript):
```typescript
public interface ISqlInjector {
    /**     * 检查SQL是否已经注入(已经注入过不再注入)     *     * @param builderAssistant mapper 构建助手     * @param mapperClass      mapper 接口的 class 对象     */    void inspectInject(MapperBuilderAssistant builderAssistant, Class<?> mapperClass);}
```

Example 2 (java):
```java
public class MysqlInsertAllBatch extends AbstractMethod {
    /**     * @since 3.5.0     */    public MysqlInsertAllBatch() {        super("mysqlInsertAllBatch");    }
    @Override    public MappedStatement injectMappedStatement(Class<?> mapperClass, Class<?> modelClass, TableInfo tableInfo) {     // 定义SQL语句 (主键+普通字段)  insert into table(....) values(....),(....)        String sql = "INSERT INTO " + tableInfo.getTableName() + "(" + tableInfo.getKeyColumn() + "," +                tableInfo.getFieldList().stream().map(TableFieldInfo::getColumn).collect(Collectors.joining(",")) + ") VALUES ";        String value = "(" + "#{" + ENTITY + DOT + tableInfo.getKeyProperty() + "}" + ","                + tableInfo.getFieldList().stream().map(tableFieldInfo -> "#{" + ENTITY + DOT + tableFieldInfo.getProperty() + "}")                .collect(Collectors.joining(",")) + ")";        String valuesScript = SqlScriptUtils.convertForeach(value, "list", null, ENTITY, COMMA);        SqlSource sqlSource = super.createSqlSource(configuration, "<script>" + sql + valuesScript + "</script>", modelClass);        KeyGenerator keyGenerator = tableInfo.getIdType() == IdType.AUTO ? Jdbc3KeyGenerator.INSTANCE : NoKeyGenerator.INSTANCE;        // 第三个参数必须和baseMapper的自定义方法名一致        return this.addInsertMappedStatement(mapperClass, modelClass, this.methodName, sqlSource, keyGenerator,tableInfo.getKeyProperty(), tableInfo.getKeyColumn());    }}
```

Example 3 (java):
```java
public class MyLogicSqlInjector extends DefaultSqlInjector {
    @Override    public List<AbstractMethod> getMethodList(Class<?> mapperClass) {        List<AbstractMethod> methodList = super.getMethodList(mapperClass);        methodList.add(new DeleteAll());        methodList.add(new MyInsertAll());        methodList.add(new MysqlInsertAllBatch());        return methodList;    }}
```

Example 4 (gdscript):
```gdscript
public interface MyBaseMapper<T> extends BaseMapper<T> {
    Integer deleteAll();
    int myInsertAll(T entity);
    int mysqlInsertAllBatch(@Param("list") List<T> batchList);}
```

---

## 支持与赞助 | MyBatis-Plus

**URL:** https://baomidou.com/resources/support

**Contents:**
- 支持与赞助
- 用爱发电
  - 成为赞助商
- 致谢

如果您正在使用这个项目并感觉良好，或者是想支持我们继续开发，您可以通过如下任意方式支持我们：

感谢给予支持的朋友，您的支持是我们前进的动力 🎉

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

---

## SpringBoot低代码快速开发平台 | MyBatis-Plus

**URL:** https://baomidou.com/resources/snowy

**Contents:**
- SpringBoot低代码快速开发平台
  - 产品介绍
  - 快速链接
  - 架构特性
  - 安全特性
  - 获得权益

Snowy是国内首个国密前后分离快速开发平台，采用Vue3+AntDesignVue3 + Vite+SpringBoot+Mp+HuTool+SaToken。集成国密加解密插件，在前后分离框架中，真正做到：前后分离“密”不可分；同时实现国产化机型、中间件、数据库适配，是您的不二之选；

官网提供企业版工作流、多租户、多数据源、Vue3表单设计器等丰富插件灵活使用。

PS：联合版（Mp企业版+Snowy企业版）皆是指双方联合授权，比单一购买更划算

在线演示: https://snowy.xiaonuo.vip

产品文档: https://www.xiaonuo.vip/doc

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

---

## 预防安全漏洞 | MyBatis-Plus

**URL:** https://baomidou.com/reference/about-cve/

**Contents:**
- 预防安全漏洞
- 什么是漏洞？
- 如何预防漏洞
  - 表字段部分
  - 字段参数/变量部分
  - 使用工具类预防
- 关于恶意漏洞的说明
  - CVE-2024-35548
  - CVE-2023-25330
  - CVE-2022-25517

软件漏洞可以对系统造成严重危害，如果被人恶意利用，会导致病毒感染、数据泄漏或损坏的风险，还可能面临直接或间接的经济损失。那么，我们应该如何预防这些漏洞呢？在深入探讨漏洞问题之前，首先需要明确什么是漏洞。

漏洞是指软件、系统或网络中存在的安全弱点或错误，这些弱点可能导致系统遭受攻击或被不当使用。在计算机安全领域，漏洞通常源于编程错误、设计缺陷或配置失误。

对于对象关系映射（ORM）框架来说，漏洞通常指的是设计或实施中的安全问题，这些问题可能让应用程序面临SQL注入攻击的风险。

如果ORM框架在执行SQL操作时没有正确过滤或转义用户输入，攻击者可以利用输入的恶意数据来执行未经授权的数据库操作，从而造成数据泄露、损坏或篡改。

什么情况下会引起 SQL 注入攻击呢？通常是在以下情况：

通常 ORM 中 SQL 注入漏洞的发生都是因为以上两个部分允许从前台传参导致的。

明白漏洞产生的主要原因，那么我们只需要控制好表结构、参数相关的数据，避免他们暴露到前端既可避免漏洞攻击。

对于表字段部分通常来说应该由后端控制，但有些系统为了保持足够大的灵活性，允许前端动态传入数据库字段名称。这种做法虽然满足了系统的灵活性要求，却面临着极大的 SQL 注入风险。

如果想要规避风险，就必须要求系统设计者或开发人员自行控制字段的安全性，绝对不能让前端任意传入字符串直接转换为 SQL 字段，应通过字段映射逻辑来阻断攻击，避免前端接口传入的字段内容直接进入 SQL 编译阶段中生成最终 SQL。

对于字段参数部分，ORM 框架通常有做预编译防止 SQL 注入攻击的逻辑，在 MyBatis 中，通过使用 # 占位符，而不是 $ 占位符来避免 SQL 注入攻击。

MyBatis-Plus 在生成相关的 SQL 时底层能力同样来自 MyBatis，所以一样的可以是用 # 占位符来避免攻击，只不过这一个步骤由 MyBatis-Plus 自动完成了。

一般来说，通过上面的处理就可以避免 SQL 注入攻击了，如果您还不放心，可以使用 MyBatis-Plus 提供的工具类 SqlInjectionUtils.check(内容) 来验证字符串是否存在 SQL 注入，如果存在则会抛出对应的异常，

最好的预防方式仍旧是不允许任何SQL片段由前端传到后台，我们强烈建议不要开放给前端太多的动态 SQL，这样最安全。

MyBatis-Plus 相关的代码和 Jar 包被别有用心的人提交了 CVE 漏洞，下面对这些漏洞进行一下官方的声明。

提醒！ 请您注意这种不被官方认可的 "CVE 漏洞" 对框架本身、对用户、对项目的交付都会产生非常大的影响，您的 损人不利己的行为 给别人带来非常大的经济损失。

如果是不安全的设计，最好的办法是 issue 或 pull request 协助官方尽快修复。

官方文档也 一而再 再而三 的强调 SQL 片段 必须检查安全，任何 ORM 框架，包括 JDBC 都是允许 字符串直接拼接SQL 的情况，因此，我们建议最好不要允许前端传入 SQL 片段。

该“漏洞”也是前端端传入 SQL 片段 导致 SQL 注入攻击。框架 QueryWrapper UpdateWrapper 字段部分是允许子查询的因此不能人为允许前端传入 SQL 片段。

如果使用者有这种需求，可以使用 SqlInjectionUtils.check(内容) 或 xxWrapper.checkSqlInjection() 方法来检查，如果检查通过，则不会抛出异常。

框架也提供了非常严格的条件构造器 LambdaQueryWrapper LambdaUpdateWrapper 推荐使用。

该“漏洞”描述了一个所谓的多租户插件引起的漏洞，说多租户插件会引起 SQL 注入攻击。让我们看看这是怎么操作的？

该“漏洞”提交者恶意暴露租户 ID 给前端，允许前端传入租户 ID，并保持在上下文中，在插件运行阶段直接读取并使用。

如果我们做过租户隔离相关的需求，就明白我们通常的做法都是用户登录了之后，后端自己查询用户所对应的租户，并自行保持上下文，保障多租户插件的正常运行。

即便要做切换租户的操作，前端传入的切换租户的 ID 也不可能说直接就拿给插件用了，而是要检测能不能切的。

如果硬要说是问题，那这也是由于使用的时候考虑不当引起的，作为一个底层框架是无法约束使用者到底怎么去使用这些功能，如果什么都需要底层框架去兜底，那人人都去当伸手党吧，别搞什么开源了。

详情链接：CVE-2022-25517，由于原漏洞仓库已经被删除，可以点此观看详情分析

该“漏洞”更加搞笑，把表字段作为前端可以传入的一部分直接拿去拼接，然后硬说这个是漏洞。理由是因为 MyBatis-Plus 开放了 String 类型的字段参数，就可以拿去传递 SQL 攻击脚本。

我们都知道 MyBatis-Plus 提供了 LamdbaQueryWrapper，可以用 LamdbaQueryWrapper 执行 Type-Safe 的查询，我们相信绝大多数人也是这样去使用的，即便用了普通的 QueryWrapper，有 String 类型的字段，也绝对不是前端传递给我们的，那字段都由前端来传，还要后端干啥？干脆让前端直接写 SQL 得了。

如果是真正的漏洞问题，我们一定积极修正，但是上面两个如此低级的错误，在没跟我们预先进行沟通的情况下，直接提交了 CVE 漏洞申请，很难让我们相信这些漏洞是好心人善意的提醒，在我们看来，这就是存粹的坏心思。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (unknown):
```unknown
// 开启自动检查 SQL 注入 (3.5.3.2+ 版本支持Wrappers.query().checkSqlInjection().orderByDesc("任意前端传入字段，我们推荐最好是白名单处理，因为可能存在检查覆盖不全情况")​// 手动校验方式 (3.4.3.2+ 版本支持)SqlInjectionUtils.check("任意前端传入字段，我们推荐最好是白名单处理，因为可能存在检查覆盖不全情况")
```

---

## SQL注入器 | MyBatis-Plus

**URL:** https://baomidou.com/guides/sql-injector/

**Contents:**
- SQL注入器
- 注入器配置
- 自定义全局方法攻略
  - 步骤 1: 定义SQL
  - 步骤 2: 注册自定义方法
  - 步骤 3: 定义BaseMapper
  - 步骤 4: 配置SqlInjector
    - 在 application.yml 中配置
    - 在 application.properties 中配置
  - 注意事项

MyBatis-Plus 提供了灵活的机制来注入自定义的 SQL 方法，这通过 sqlInjector 全局配置实现。通过实现 ISqlInjector 接口或继承 AbstractSqlInjector 抽象类，你可以注入自定义的通用方法到 MyBatis 容器中。

SQL注入器允许开发者扩展和定制SQL语句的生成，以适应特定的业务逻辑和查询需求。以下是SQL注入器的一些示例使用场景和它能实现的功能：

自定义查询方法：当标准的CRUD操作无法满足复杂的查询需求时，可以通过SQL注入器添加自定义的查询方法。

复杂数据处理：在需要进行复杂的数据处理，如多表联结、子查询、聚合函数等时，SQL注入器可以帮助生成相应的SQL语句。

性能优化：通过自定义SQL语句，可以针对特定的查询场景进行性能优化。

数据权限控制：在需要根据用户权限动态生成SQL语句时，SQL注入器可以用来实现数据权限的控制。

遗留系统迁移：在将遗留系统迁移到MyBatis-Plus时，可能需要保留原有的SQL语句结构，SQL注入器可以帮助实现这一过渡。

注入自定义SQL方法：通过实现ISqlInjector接口，可以注入自定义的SQL方法到MyBatis容器中，这些方法可以是任何复杂的SQL查询。

扩展BaseMapper：可以在继承BaseMapper的基础上，通过SQL注入器添加额外的查询方法，这些方法将自动被MyBatis-Plus识别和使用。

灵活的SQL生成：SQL注入器提供了灵活的SQL生成机制，可以根据业务需求生成各种SQL语句，包括但不限于SELECT、INSERT、UPDATE、DELETE等。

集成第三方数据库功能：如果需要使用数据库的特定功能，如存储过程、触发器等，SQL注入器可以帮助生成调用这些功能的SQL语句。

动态SQL支持：在某些场景下，SQL语句需要根据运行时的条件动态生成，SQL注入器可以支持这种动态SQL的生成。

通过SQL注入器，MyBatis-Plus提供了一个强大的扩展点，使得开发者能够根据项目的具体需求，灵活地定制和优化SQL语句，从而提高应用的性能和适应性。

在MyBatis-Plus中，sqlInjector 配置是一个全局配置项，用于指定一个实现了 ISqlInjector 接口的类，该类负责将自定义的SQL方法注入到MyBatis的Mapper接口中。

默认的注入器实现是 DefaultSqlInjector，你可以参考它来创建自己的注入器。

以下是如何配置 sqlInjector 的示例。

根据提供的参考信息，我们可以看到如何在MyBatis-Plus中实现自定义的全局方法，包括逻辑删除、自动填充以及自定义的insert和insertBatch方法。下面是一个更详细的步骤说明和示例代码：

首先，你需要定义自定义方法的SQL语句。这通常在继承了AbstractMethod的类中完成，例如MysqlInsertAllBatch。

接下来，你需要创建一个类来继承DefaultSqlInjector，并重写getMethodList方法来注册你的自定义方法。

然后，你需要在你的BaseMapper接口中定义自定义的方法。

最后，你需要在配置文件中指定你的自定义SQL注入器。

通过以上步骤，你就可以在MyBatis-Plus中成功地实现自定义的全局方法了。记得在实际使用中，根据你的业务需求调整SQL语句和方法的实现。

参考 自定义 BaseMapper 示例，你可以找到如何创建自定义的 SQL 注入器和如何在项目中使用它们的详细步骤。

通过这种方式，MyBatis-Plus 允许你扩展其功能，以满足特定的业务需求，同时保持代码的整洁和可维护性。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (typescript):
```typescript
public interface ISqlInjector {
    /**     * 检查SQL是否已经注入(已经注入过不再注入)     *     * @param builderAssistant mapper 构建助手     * @param mapperClass      mapper 接口的 class 对象     */    void inspectInject(MapperBuilderAssistant builderAssistant, Class<?> mapperClass);}
```

Example 2 (java):
```java
public class MysqlInsertAllBatch extends AbstractMethod {
    /**     * @since 3.5.0     */    public MysqlInsertAllBatch() {        super("mysqlInsertAllBatch");    }
    @Override    public MappedStatement injectMappedStatement(Class<?> mapperClass, Class<?> modelClass, TableInfo tableInfo) {     // 定义SQL语句 (主键+普通字段)  insert into table(....) values(....),(....)        String sql = "INSERT INTO " + tableInfo.getTableName() + "(" + tableInfo.getKeyColumn() + "," +                tableInfo.getFieldList().stream().map(TableFieldInfo::getColumn).collect(Collectors.joining(",")) + ") VALUES ";        String value = "(" + "#{" + ENTITY + DOT + tableInfo.getKeyProperty() + "}" + ","                + tableInfo.getFieldList().stream().map(tableFieldInfo -> "#{" + ENTITY + DOT + tableFieldInfo.getProperty() + "}")                .collect(Collectors.joining(",")) + ")";        String valuesScript = SqlScriptUtils.convertForeach(value, "list", null, ENTITY, COMMA);        SqlSource sqlSource = super.createSqlSource(configuration, "<script>" + sql + valuesScript + "</script>", modelClass);        KeyGenerator keyGenerator = tableInfo.getIdType() == IdType.AUTO ? Jdbc3KeyGenerator.INSTANCE : NoKeyGenerator.INSTANCE;        // 第三个参数必须和baseMapper的自定义方法名一致        return this.addInsertMappedStatement(mapperClass, modelClass, this.methodName, sqlSource, keyGenerator,tableInfo.getKeyProperty(), tableInfo.getKeyColumn());    }}
```

Example 3 (java):
```java
public class MyLogicSqlInjector extends DefaultSqlInjector {
    @Override    public List<AbstractMethod> getMethodList(Class<?> mapperClass) {        List<AbstractMethod> methodList = super.getMethodList(mapperClass);        methodList.add(new DeleteAll());        methodList.add(new MyInsertAll());        methodList.add(new MysqlInsertAllBatch());        return methodList;    }}
```

Example 4 (gdscript):
```gdscript
public interface MyBaseMapper<T> extends BaseMapper<T> {
    Integer deleteAll();
    int myInsertAll(T entity);
    int mysqlInsertAllBatch(@Param("list") List<T> batchList);}
```

---

## 自动维护DDL | MyBatis-Plus

**URL:** https://baomidou.com/guides/auto-ddl/

**Contents:**
- 自动维护DDL
- 功能概述
- 注意事项
- 代码示例
- 自定义运行器

在MyBatis-Plus的3.5.3+版本中，引入了一项强大的功能：数据库DDL（数据定义语言）表结构的自动维护。这一功能通过执行SQL脚本来实现数据库模式的初始化和升级，与传统的flyway工具相比，它不仅支持分表库，还能够控制代码执行SQL脚本的过程。

以下是一个使用MyBatis-Plus自动维护DDL的Java组件示例：

在这个示例中，我们定义了一个MysqlDdl组件，它实现了IDdl接口，并提供了要执行的SQL脚本文件列表。通过调用ShardingKey.change方法，我们可以切换到mysql的从库，并使用ddlScript.run方法执行特定的SQL脚本。

通过这种方式，MyBatis-Plus提供了一个高效且自动化的方式来管理数据库的DDL操作，极大地简化了数据库结构的管理和维护工作。

如果集成了MyBatis-Plus的starter的话，会自动实例化一个 DdlApplicationRunner 实例来执行 DDL 脚本。

执行方式为自动提交事务，且忽略错误继续执行（其他脚本参数见如下）。

如果需要自定义控制，请自行注入一个DdlApplicationRunner实例至容器。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (sql):
```sql
@Componentpublic class MysqlDdl implements IDdl {
    /**     * 获取要执行的SQL脚本文件列表     */    @Override    public List<String> getSqlFiles() {        return Arrays.asList(                "db/tag-schema.sql",                // 从`3.5.3.2`版本开始，支持执行存储过程。在文件名后追加`#$$`，其中`$$`是自定义的完整SQL分隔符。                // 存储过程脚本以`END`结尾，并追加分隔符`END;$$`表示脚本结束。                "db/procedure.sql#$$",                "D:\\db\\tag-data.sql"        );    }}
// 切换到mysql从库，执行SQL脚本 (开源版本无此功能)ShardingKey.change("mysqlt2");ddlScript.run(new StringReader("DELETE FROM user;\n" +        "INSERT INTO user (id, username, password, sex, email) VALUES\n" +        "(20, 'Duo', '123456', 0, 'Duo@baomidou.com');"));
```

Example 2 (typescript):
```typescript
@Bean    public DdlApplicationRunner ddlApplicationRunner(List<IDdl> ddlList) {          DdlApplicationRunner ddlApplicationRunner = new DdlApplicationRunner(ddlList);        // 下面属性自 3.5.11 开始 ...        // 设置是否自动提交 默认: true        ddlApplicationRunner.setAutoCommit(false);        // 设置脚本遇到错误的处理方式 默认: 忽略错误,打印异常 (如果设置为抛出异常,那会终止下一个sql文件处理)        ddlApplicationRunner.setDdlScriptErrorHandler(DdlScriptErrorHandler.ThrowsErrorHandler.INSTANCE);        //是否抛出异常中断下个处理器处理 默认: false        ddlApplicationRunner.setThrowException(true);        ddlApplicationRunner.setScriptRunnerConsumer(scriptRunner -> {            scriptRunner.setLogWriter(null);   // 关闭执行日志打印 默认: System.out            scriptRunner.setErrorLogWriter(null); // 关闭错误日志打印  默认:System.err            scriptRunner.setStopOnError(true); // 遇到异常是否停止            scriptRunner.setRemoveCRs(false); //  是否替换\r\n 为 \n 默认: false        });        return ddlApplicationRunner;    }
```

---

## 批量操作 | MyBatis-Plus

**URL:** https://baomidou.com/guides/batch-operation/

**Contents:**
- 批量操作
- 功能概览
- 类结构说明
  - MybatisBatch<?>
  - MybatisBatch.Method<?>
  - BatchMethod<?>
- 使用步骤
- 返回值说明
- 使用示例
  - execute方法

批量操作是一种高效处理大量数据的技术，它允许开发者一次性执行多个数据库操作，从而减少与数据库的交互次数，提高数据处理的效率和性能。在MyBatis-Plus中，批量操作主要用于以下几个方面：

实际为BatchMethod，简化框架内部操作方法调用。

返回类型：List<BatchResult>

返回内容：每次执行MappedStatement + SQL的操作结果分组。

注意：例如批量根据ID更新，若10条数据中5条更新一个字段，5条更新两个字段，则返回值为容量为2的List，分别存储5条记录的更新情况。

框架提供MybatisBatchUtils进行静态方法调用。

适用于insert, update, delete操作。

注意：跨sqlSession下需注意缓存和数据感知问题。

如果对导入表有更高的性能要求，可以采用执行 SQL LOAD csv 的方式，如下为 MySQL 的示例：

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (typescript):
```typescript
List<H2User> userList = Arrays.asList(new H2User(2000L, "测试"), new H2User(2001L, "测试"));MybatisBatch<H2User> mybatisBatch = new MybatisBatch<>(sqlSessionFactory, userList);MybatisBatch.Method<H2User> method = new MybatisBatch.Method<>(H2UserMapper.class);mybatisBatch.execute(method.insert());
```

Example 2 (typescript):
```typescript
List<Long> ids = Arrays.asList(120000L, 120001L);MybatisBatch<Long> mybatisBatch = new MybatisBatch<>(sqlSessionFactory, ids);MybatisBatch.Method<H2User> method = new MybatisBatch.Method<>(H2UserMapper.class);mybatisBatch.execute(method.insert(id -> {    H2User h2User = new H2User();    h2User.setTestId(id);    return h2User;}));
```

Example 3 (sql):
```sql
// mapper方法定义@Insert("insert into h2user(name,version) values( #{name}, #{version})")int myInsertWithoutParam(H2User user1);
// 准备数据List<H2User> h2UserList = new ArrayList<>();for (int i = 0; i < 1000; i++) {    h2UserList.add(new H2User("myInsertWithoutParam" + i));}
MybatisBatch<H2User> mybatisBatch = new MybatisBatch<>(sqlSessionFactory, h2UserList);MybatisBatch.Method<H2User> method = new MybatisBatch.Method<>(H2UserMapper.class);mybatisBatch.execute(method.get("myInsertWithoutParam"));
```

Example 4 (sql):
```sql
// 带注解的mapper方法定义@Insert("insert into h2user(name,version) values( #{user1.name}, #{user1.version})")int myInsertWithParam(@Param("user1") H2User user1);
// 准备数据List<H2User> h2UserList = new ArrayList<>();for (int i = 0; i < 1000; i++) {    h2UserList.add(new H2User("myInsertWithParam" + i));}
MybatisBatch<H2User> mybatisBatch = new MybatisBatch<>(sqlSessionFactory, h2UserList);MybatisBatch.Method<H2User> method = new MybatisBatch.Method<>(H2UserMapper.class);mybatisBatch.execute(method.get("myInsertWithParam", (user) -> {    Map<String, Object> map = new HashMap<>();    map.put("user1", user);    return map;}));
```

---

## 数据安全保护 | MyBatis-Plus

**URL:** https://baomidou.com/guides/security/

**Contents:**
- 数据安全保护
- 配置安全
  - YML 配置加密
  - 密钥加密
  - 如何使用
- 数据安全
- SQL 注入安全保护
  - 自动检查
  - 手动校验

MyBatis-Plus 提供了数据安全保护功能，旨在防止因开发人员流动而导致的敏感信息泄露。从3.3.2版本开始，MyBatis-Plus 支持通过加密配置和数据安全措施来增强数据库的安全性。

MyBatis-Plus 允许你使用加密后的字符串来配置数据库连接信息。在 YML 配置文件中，以 mpw: 开头的配置项将被视为加密内容。

使用 AES 算法生成随机密钥，并对敏感数据进行加密。

自3.5.10开始支持系统属性与环境变量传递密钥.

MyBatis-Plus 提供了字段加密解密和字段脱敏功能，以保护存储在数据库中的敏感数据。

MyBatis-Plus 提供了自动和手动两种方式来检查 SQL 注入风险。

使用 Wrappers.query() 方法时，可以通过 .checkSqlInjection() 开启自动检查。

使用 SqlInjectionUtils.check() 方法进行手动校验。

最好的预防方式仍旧是不允许任何SQL片段由前端传到后台，我们强烈建议不要开放给前端太多的动态 SQL，这样最安全。

通过上述措施，MyBatis-Plus 帮助你构建了一个更加安全的数据库环境，保护了敏感数据不被泄露。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (typescript):
```typescript
spring:  datasource:    url: mpw:qRhvCwF4GOqjessEB3G+a5okP+uXXr96wcucn2Pev6Bf1oEMZ1gVpPPhdDmjQqoM    password: mpw:Hzy5iliJbwDHhjLs1L0j6w==    username: mpw:Xb+EgsyuYRXw7U7sBJjBpA==
```

Example 2 (unknown):
```unknown
// 生成16位随机AES密钥String randomKey = AES.generateRandomKey();
// 使用随机密钥加密数据String encryptedData = AES.encrypt(data, randomKey);
```

Example 3 (unknown):
```unknown
// Jar 启动参数示例（在IDEA中设置Program arguments，或在服务器上设置为启动环境变量）--mpw.key=d1104d7c3b616f0b
```

Example 4 (unknown):
```unknown
Wrappers.query()// 开启自动检查 SQL 注入.checkSqlInjection().orderByDesc("任意前端传入字段，我们推荐最好是白名单处理，因为可能存在检查覆盖不全情况")
```

---

## Guns | MyBatis-Plus

**URL:** https://baomidou.com/resources/guns

**Contents:**
- Guns
- 产品优势

MyBatis-Mate + Guns 联合授权限时折扣，一份钱享受双倍快乐，更多咨询微信 wx153666

Guns是一个现代化的Java应用开发框架，基于主流技术Spring Boot2，同时支持单体和微服务架构，Guns基于插件化架构，可以自由组合细粒度模块依赖，实现不同功能的组合和剔除，目前企业版支持包括微服务、SSO统一认证、SaaS多租户、工作流、报表、支付、代码加密混淆等数十种插件功能。

官网：https://javaguns.com/enterprise

在线演示: https://vue3.javaguns.com/

Guns历经6年积累和沉淀，已形成一套独具特点的项目架构方案，Guns的架构可以直接应用到任何软件或信息化公司自身的技术体系建设中，帮助企业解决规范问题，解决复用问题，解决架构问题，使用Guns可以快速开发出各类信息化管理系统，例如OA办公系统、项目管理系统、商城系统、供应链系统、客户关系管理系统、微信公众平台管理系统、小程序管理后台，同时可开发移动程序服务端，例如App Server服务器、小程序Server服务器等。

最新Guns推出DevOps一体化开发平台，提供更人性化、更高效的项目开发体验，平台包含4大核心（DevOps运维平台、代码生成平台、Api接口平台、知识库平台）。

DevOps运维平台：提供主机管理、Shell远程终端控制、远程文件管理、持续集成、一键部署、基础中间件部署，可以方便公司任何前后端项目的快速部署和实时CI/CD任务管理。

代码生成平台：提供模板管理、支持多个框架的代码生成、支持在线定制化模板配置，提供元数据管理和元数据组合，支持自定义包名的代码生成和项目生成，减少重复代码编写。

Api接口平台：超越Postman和Swagger的接口管理体验，接口平台直接打通Guns系统，更高效的接口维护体验，超越其他Api管理工具，接口在线化管理，支持一键导出Api文档。注：Api接口平台只适用于Guns。

知识库平台：为公司内部提供各类文档资料的录入、检索、查看和管理，使用知识库平台，方便员工随时检索内部技术资料，更容易形成企业内部知识库沉淀。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

---
