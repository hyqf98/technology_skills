# Mybatis-Plus - 其他功能

**页数:** 22

---

## 多数据源支持

**URL:** https://baomidou.com/guides/dynamic-datasource/

### 功能概述

随着项目规模的扩大，单一数据源已无法满足复杂业务需求，多数据源（动态数据源）应运而生。本文介绍 MyBatis-Plus 的两种多数据源扩展方案：

1. **dynamic-datasource**（开源生态）- Spring Boot 多数据源启动器
2. **mybatis-mate**（企业级生态）- 付费企业组件，提供高级特性

### Dynamic-Datasource

#### 特性

- **数据源分组**: 适用于多数据源场景的读写分离、一主多从、纯多库等场景
- **敏感信息加密**: 支持数据库连接信息的加密存储
- **独立初始化表结构**: 每个数据源可独立初始化表结构
- **动态切换**: 通过注解动态切换数据源
- **事务支持**: 支持多数据源事务管理
- **SpEL 表达式**: 支持 SpEL 表达式进行数据源选择

#### 约定

1. **主数据源**: 配置中 `primary` 指定的数据源为主数据源
2. **默认数据源**: 未指定 `@DS` 注解时使用主数据源
3. **严格模式**: `strict` 配置控制未找到数据源时的行为

#### 依赖配置

**Spring Boot 2.x:**

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>dynamic-datasource-spring-boot-starter</artifactId>
    <version>${version}</version>
</dependency>
```

**Spring Boot 3.x:**

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>dynamic-datasource-spring-boot3-starter</artifactId>
    <version>${version}</version>
</dependency>
```

#### 配置示例

```yaml
spring:
  datasource:
    dynamic:
      # 设置默认数据源组
      primary: master
      # 严格模式，未匹配到数据源时抛出异常
      strict: false
      # 数据源配置
      datasource:
        # 主数据源
        master:
          url: jdbc:mysql://xx.xx.xx.xx:3306/dynamic
          username: root
          password: 123456
          driver-class-name: com.mysql.cj.jdbc.Driver
          # 连接池配置
          hikari:
            minimum-idle: 5
            maximum-pool-size: 20
            connection-timeout: 30000
            idle-timeout: 600000
            max-lifetime: 1800000

        # 从数据源 1
        slave_1:
          url: jdbc:mysql://xx.xx.xx.xx:3307/dynamic
          username: root
          password: 123456
          driver-class-name: com.mysql.cj.jdbc.Driver

        # 从数据源 2（加密配置）
        slave_2:
          url: ENC(xxxxx)
          username: ENC(xxxxx)
          password: ENC(xxxxx)
          driver-class-name: com.mysql.cj.jdbc.Driver
```

#### 使用示例

##### 1. 基础使用

```java
@Service
@DS("slave")  // 类级别指定数据源
public class UserServiceImpl implements UserService {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Autowired
    private UserMapper userMapper;

    // 使用类级别指定的 slave 数据源
    @Override
    public List<Map<String, Object>> selectAll() {
        return jdbcTemplate.queryForList("select * from user");
    }

    // 方法级别覆盖类级别配置
    @Override
    @DS("slave_1")  // 使用 slave_1 数据源
    public List<Map<String, Object>> selectByCondition() {
        return jdbcTemplate.queryForList(
            "select * from user where age > 10"
        );
    }

    // 未指定注解，使用主数据源 master
    @DS("master")
    public List<User> selectFromMaster() {
        return userMapper.selectList(null);
    }
}
```

##### 2. 动态切换数据源

```java
@Service
public class DynamicDataService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 根据用户ID动态选择数据源
     */
    public User getUserById(Long userId) {
        // 根据用户ID计算数据源
        int datasourceIndex = (int) (userId % 2);
        String dsName = "slave_" + datasourceIndex;

        // 使用 DynamicDataSourceContext 切换数据源
        DynamicDataSourceContextHolder.push(dsName);
        try {
            return userMapper.selectById(userId);
        } finally {
            DynamicDataSourceContextHolder.clear();
        }
    }

    /**
     * 使用 @DS 注解的 SpEL 表达式
     */
    @DS("#dataSourceName")
    public List<User> getUsersByDataSource(String dataSourceName) {
        return userMapper.selectList(null);
    }
}
```

##### 3. 多数据源事务管理

```java
@Service
public class MultiDataSourceTransactionService {

    @Autowired
    private UserMapper userMapper;

    @Autowired
    private OrderMapper orderMapper;

    /**
     * 跨数据源事务（需要 JTA 支持）
     * 注意：普通的 @Transactional 只支持单数据源事务
     */
    @DS("master")
    @Transactional(rollbackFor = Exception.class)
    public void transactionInMaster() {
        // 在 master 数据源执行事务操作
        User user = new User();
        user.setName("Test");
        userMapper.insert(user);
    }

    /**
     * 使用分布式事务管理器
     * 需要集成 Atomikos 或 Narayana
     */
    @GlobalTransactional  // 需要分布式事务框架支持
    public void crossDataSourceTransaction() {
        // 在 master 数据源插入
        DynamicDataSourceContextHolder.push("master");
        try {
            userMapper.insert(new User());
        } finally {
            DynamicDataSourceContextHolder.clear();
        }

        // 在 slave 数据源插入
        DynamicDataSourceContextHolder.push("slave_1");
        try {
            orderMapper.insert(new Order());
        } finally {
            DynamicDataSourceContextHolder.clear();
        }
    }
}
```

##### 4. 读写分离配置

```java
@Configuration
public class ReadWriteSplitConfig {

    /**
     * 配置读写分离策略
     */
    @Bean
    public DynamicDataSourceProvider dynamicDataSourceProvider() {
        Map<String, DataSourceProperty> dataSourceMap = new HashMap<>();

        // 主库（写）
        DataSourceProperty master = new DataSourceProperty();
        master.setUrl("jdbc:mysql://localhost:3306/master");
        master.setUsername("root");
        master.setPassword("123456");
        dataSourceMap.put("master", master);

        // 从库（读）
        DataSourceProperty slave = new DataSourceProperty();
        slave.setUrl("jdbc:mysql://localhost:3307/slave");
        slave.setUsername("root");
        slave.setPassword("123456");
        dataSourceMap.put("slave", slave);

        return new AbstractDataSourceProvider(dataSourceMap) {
            @Override
            public DataSource createDataSource(String dataSourceName) {
                return createDataSourceMap().get(dataSourceName);
            }
        };
    }
}
```

```java
@Service
public class ReadWriteSplitService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 写操作 - 使用主库
     */
    @DS("master")
    public void createUser(User user) {
        userMapper.insert(user);
    }

    /**
     * 读操作 - 使用从库
     */
    @DS("slave")
    public List<User> listUsers() {
        return userMapper.selectList(null);
    }

    /**
     * 读操作 - 轮询负载均衡
     */
    @DS("slave")
    @DSGroup("slaveGroup")  // 配置多个 slave 的负载均衡组
    public List<User> listUsersWithLoadBalance() {
        return userMapper.selectList(null);
    }
}
```

##### 5. 数据源健康检查

```java
@Component
public class DataSourceHealthChecker implements HealthIndicator {

    @Autowired
    private DataSource dataSource;

    @Override
    public Health health() {
        try (Connection connection = dataSource.getConnection()) {
            if (connection.isValid(1)) {
                return Health.up()
                    .withDetail("database", "connected")
                    .build();
            }
        } catch (SQLException e) {
            return Health.down()
                .withDetail("error", e.getMessage())
                .build();
        }
        return Health.down().build();
    }
}
```

### MyBatis-Mate

#### 特性

MyBatis-Mate 是一个付费的企业级高级特性组件，提供以下功能：

- **增强的多数据源支持**: 支持动态加载卸载数据源
- **分布式事务**: 支持 JTA Atomikos 事务管理
- **数据权限**: 基于注解的数据权限控制
- **字段加密**: 敏感字段自动加密解密
- **审计日志**: 数据变更自动记录
- **多租户**: 完善的多租户解决方案

#### 使用方法

参考 MyBatis-Mate 官方文档获取详细使用教程：

- **多数据源动态加载卸载**: [mybatis-mate-sharding-dynamic](https://baomidou.com)
- **多数据源事务（JTA Atomikos）**: [mybatis-mate-sharding-jta-atomikos](https://baomidou.com)

### 最佳实践

#### 1. 数据源配置建议

```yaml
spring:
  datasource:
    dynamic:
      # 生产环境建议开启严格模式
      strict: true

      # 配置数据源组
      datasource:
        # 主库配置
        master:
          url: jdbc:mysql://${DB_HOST:localhost}:3306/${DB_NAME:mydb}
          username: ${DB_USER:root}
          password: ${DB_PASSWORD:123456}
          driver-class-name: com.mysql.cj.jdbc.Driver
          # Hikari 连接池优化配置
          hikari:
            minimum-idle: 10
            maximum-pool-size: 50
            connection-timeout: 30000
            idle-timeout: 600000
            max-lifetime: 1800000
            connection-test-query: SELECT 1
            pool-name: MasterHikariCP
```

#### 2. 数据源切换规范

```java
/**
 * 数据源切换工具类
 */
public class DataSourceSwitcher {

    /**
     * 在指定数据源中执行操作
     */
    public static <T> T executeInDataSource(
        String dataSourceName,
        Supplier<T> supplier
    ) {
        DynamicDataSourceContextHolder.push(dataSourceName);
        try {
            return supplier.get();
        } finally {
            DynamicDataSourceContextHolder.clear();
        }
    }

    /**
     * 在主数据源执行
     */
    public static <T> T executeInMaster(Supplier<T> supplier) {
        return executeInDataSource("master", supplier);
    }

    /**
     * 在从数据源执行
     */
    public static <T> T executeInSlave(Supplier<T> supplier) {
        return executeInDataSource("slave", supplier);
    }
}

// 使用示例
@Service
public class OrderService {

    @Autowired
    private OrderMapper orderMapper;

    public void createOrder(Order order) {
        // 写操作使用主库
        DataSourceSwitcher.executeInMaster(() -> {
            orderMapper.insert(order);
            return null;
        });
    }

    public Order getOrder(Long orderId) {
        // 读操作使用从库
        return DataSourceSwitcher.executeInSlave(() -> {
            return orderMapper.selectById(orderId);
        });
    }
}
```

#### 3. 数据源路由策略

```java
/**
 * 基于 ID 哈希的数据源路由
 */
@Component
public class HashDataSourceRouter {

    private static final String[] DATA_SOURCES = {
        "slave_1", "slave_2", "slave_3"
    };

    /**
     * 根据 ID 路由到对应数据源
     */
    public String route(Long id) {
        int index = (int) (id % DATA_SOURCES.length);
        return DATA_SOURCES[Math.abs(index)];
    }

    /**
     * 批量路由到对应数据源
     */
    public Map<String, List<Long>> batchRoute(List<Long> ids) {
        return ids.stream()
            .collect(Collectors.groupingBy(this::route));
    }
}

// 使用示例
@Service
public class ProductService {

    @Autowired
    private ProductMapper productMapper;

    @Autowired
    private HashDataSourceRouter router;

    public List<Product> getProducts(List<Long> ids) {
        Map<String, List<Long>> routedIds = router.batchRoute(ids);
        List<Product> products = new ArrayList<>();

        routedIds.forEach((dataSource, idList) -> {
            DynamicDataSourceContextHolder.push(dataSource);
            try {
                products.addAll(productMapper.selectBatchIds(idList));
            } finally {
                DynamicDataSourceContextHolder.clear();
            }
        });

        return products;
    }
}
```

#### 4. 数据源监控

```java
@Component
public class DataSourceMonitor {

    @Autowired
    private DataSource dataSource;

    private final MeterRegistry meterRegistry;

    public DataSourceMonitor(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
        monitorDataSource();
    }

    /**
     * 监控数据源指标
     */
    private void monitorDataSource() {
        if (dataSource instanceof HikariDataSource) {
            HikariDataSource hikariDataSource = (HikariDataSource) dataSource;
            HikariPoolMXBean poolProxy = hikariDataSource.getHikariPoolMXBean();

            Gauge.builder("datasource.active.connections",
                    poolProxy, HikariPoolMXBean::getActiveConnections)
                .tag("pool", hikariDataSource.getPoolName())
                .register(meterRegistry);

            Gauge.builder("datasource.idle.connections",
                    poolProxy, HikariPoolMXBean::getIdleConnections)
                .tag("pool", hikariDataSource.getPoolName())
                .register(meterRegistry);

            Gauge.builder("datasource.waiting.threads",
                    poolProxy, HikariPoolMXBean::getThreadsAwaitingConnection)
                .tag("pool", hikariDataSource.getPoolName())
                .register(meterRegistry);
        }
    }
}
```

### 注意事项

1. **事务管理**: 跨数据源事务需要分布式事务支持，普通的 `@Transactional` 不支持
2. **连接池配置**: 不同数据源需要独立配置连接池参数
3. **性能考虑**: 频繁切换数据源会影响性能，建议批量操作时使用同一数据源
4. **异常处理**: 数据源切换失败时需要及时清理上下文
5. **测试**: 充分测试多数据源场景，确保数据一致性

---

## 插件主体

**URL:** https://baomidou.com/plugins/

### MybatisPlusInterceptor 概览

MyBatis-Plus 提供了一系列强大的插件来增强 MyBatis 的功能，这些插件通过 `MybatisPlusInterceptor` 来实现对 MyBatis 执行过程的拦截和增强。

#### 核心概念

`MybatisPlusInterceptor` 是 MyBatis-Plus 的核心插件机制，它代理了 MyBatis 的以下方法：

- `Executor#query`: 查询方法拦截
- `Executor#update`: 更新方法拦截
- `StatementHandler#prepare`: 语句准备拦截

通过这些拦截点，可以在 SQL 执行前后插入自定义逻辑。

#### 属性说明

```java
public class MybatisPlusInterceptor implements Interceptor {
    /**
     * 内部拦截器列表
     * 用于存储所有要应用的内部拦截器
     */
    private List<InnerInterceptor> interceptors = new ArrayList<>();
}
```

#### InnerInterceptor 接口

所有 MyBatis-Plus 插件都实现了 `InnerInterceptor` 接口：

```java
public interface InnerInterceptor {
    /**
     * 查询前拦截
     */
    default void beforeQuery(Executor executor, MappedStatement ms,
                            Object parameter, RowBounds rowBounds,
                            ResultHandler resultHandler, BoundSql boundSql)
                            throws SQLException {}

    /**
     * 更新前拦截
     */
    default void beforeUpdate(Executor executor, MappedStatement ms,
                             Object parameter) throws SQLException {}

    /**
     * 准备语句前拦截
     */
    default void beforePrepare(StatementHandler sh, Connection connection,
                              Integer transactionTimeout) throws SQLException {}
}
```

### 内置插件列表

MyBatis-Plus 提供以下内置插件：

1. **PaginationInnerInterceptor**: 分页插件
2. **TenantLineInnerInterceptor**: 多租户插件
3. **DynamicTableNameInnerInterceptor**: 动态表名插件
4. **BlockAttackInnerInterceptor**: 防全表更新删除插件
5. **IllegalSQLInnerInterceptor**: 非法 SQL 拦截插件
6. **OptimisticLockerInnerInterceptor**: 乐观锁插件
7. **DataChangeRecorderInnerInterceptor**: 数据变动记录插件

### 插件配置顺序

**重要**: 使用多个插件时，顺序非常重要。建议的顺序如下：

```java
@Configuration
public class MybatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        // 1. 第一层：对 SQL 进行单次改造的插件
        // 多租户插件（需要修改 SQL）
        interceptor.addInnerInterceptor(new TenantLineInnerInterceptor());

        // 动态表名插件（需要修改 SQL）
        interceptor.addInnerInterceptor(new DynamicTableNameInnerInterceptor());

        // 防全表更新删除插件（需要修改 SQL）
        interceptor.addInnerInterceptor(new BlockAttackInnerInterceptor());

        // 2. 第二层：分页插件（需要 COUNT 和优化 SQL）
        interceptor.addInnerInterceptor(
            new PaginationInnerInterceptor(DbType.MYSQL)
        );

        // 3. 第三层：不对 SQL 进行改造的插件
        // 乐观锁插件（不影响 SQL）
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());

        return interceptor;
    }
}
```

**顺序原则**:
- 对 SQL 进行单次改造的插件应优先放入
- 不对 SQL 进行改造的插件最后放入
- 分页插件通常放在中间，因为它需要 COUNT 查询

### 配置方式

#### 1. Spring Boot 配置

```java
@Configuration
@MapperScan("scan.your.mapper.package")
public class MybatisPlusConfig {

    /**
     * 配置 MyBatis-Plus 拦截器
     */
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        // 添加分页插件
        PaginationInnerInterceptor paginationInterceptor =
            new PaginationInnerInterceptor(DbType.MYSQL);

        // 设置单页最大限制数量
        paginationInterceptor.setMaxLimit(500L);

        // 设置溢出总页数后是否进行处理
        paginationInterceptor.setOverflow(false);

        // 优化 COUNT SQL
        paginationInterceptor.setOptimizeCountSql(true);

        interceptor.addInnerInterceptor(paginationInterceptor);

        return interceptor;
    }

    /**
     * 配置多插件组合
     */
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptorWithMulti() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        // 1. 多租户插件
        TenantLineInnerInterceptor tenantInterceptor =
            new TenantLineInnerInterceptor();
        tenantInterceptor.setTenantLineHandler(new CustomTenantHandler());
        interceptor.addInnerInterceptor(tenantInterceptor);

        // 2. 防全表更新删除插件
        interceptor.addInnerInterceptor(new BlockAttackInnerInterceptor());

        // 3. 分页插件
        interceptor.addInnerInterceptor(
            new PaginationInnerInterceptor(DbType.MYSQL)
        );

        // 4. 乐观锁插件
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());

        return interceptor;
    }
}
```

#### 2. Spring XML 配置

```xml
<!-- 配置 SqlSessionFactory -->
<bean id="sqlSessionFactory"
      class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
    <!-- 其他属性 略 -->
    <property name="configuration" ref="configuration"/>
    <property name="plugins">
        <array>
            <ref bean="mybatisPlusInterceptor"/>
        </array>
    </property>
</bean>

<!-- 配置 MybatisPlusInterceptor -->
<bean id="mybatisPlusInterceptor"
      class="com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor">
    <property name="interceptors">
        <list>
            <!-- 分页插件 -->
            <bean class="com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor">
                <!-- 对于单一数据库类型来说,都建议配置该值,避免每次分页都去抓取数据库类型 -->
                <constructor-arg name="dbType" value="MYSQL"/>
                <!-- 单页最大限制数量 -->
                <property name="maxLimit" value="500"/>
                <!-- 溢出总页数后是否进行处理 -->
                <property name="overflow" value="false"/>
            </bean>

            <!-- 防全表更新删除插件 -->
            <bean class="com.baomidou.mybatisplus.extension.plugins.inner.BlockAttackInnerInterceptor"/>

            <!-- 乐观锁插件 -->
            <bean class="com.baomidou.mybatisplus.extension.plugins.inner.OptimisticLockerInnerInterceptor"/>
        </list>
    </property>
</bean>
```

#### 3. mybatis-config.xml 配置

```xml
<configuration>
    <plugins>
        <plugin interceptor="com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor">
            <property name="@page"
                      value="com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor"/>
            <property name="page:dbType" value="mysql"/>
            <property name="page:maxLimit" value="500"/>
        </plugin>
    </plugins>
</configuration>
```

### 拦截忽略注解 @InterceptorIgnore

`@InterceptorIgnore` 注解可以用来忽略某些插件的拦截。

#### 注解属性

```java
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface InterceptorIgnore {
    /**
     * 忽略分页插件
     */
    String pagination() default "";

    /**
     * 忽略多租户插件
     */
    String tenantLine() default "";

    /**
     * 忽略防全表更新删除插件
     */
    String blockAttack() default "";

    /**
     * 忽略非法 SQL 拦截插件
     */
    String illegalSql() default "";

    /**
     * 忽略动态表名插件
     */
    String dynamicTableName() default "";
}
```

#### 使用示例

```java
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 忽略多租户插件，查询所有租户的用户
     */
    @InterceptorIgnore(tenantLine = "true")
    public List<User> listAllUsers() {
        return userMapper.selectList(null);
    }

    /**
     * 忽略防全表更新删除插件
     * 注意：这个操作很危险，需要谨慎使用
     */
    @InterceptorIgnore(blockAttack = "true")
    public void updateAllUsers() {
        User user = new User();
        user.setStatus("inactive");
        userMapper.update(user, null);
    }

    /**
     * 忽略多个插件
     */
    @InterceptorIgnore(
        tenantLine = "true",
        pagination = "true"
    )
    public List<User> listAllUsersWithoutPage() {
        return userMapper.selectList(null);
    }
}

/**
 * 在 Mapper 接口上使用
 */
public interface UserMapper extends BaseMapper<User> {

    /**
     * 忽略多租户插件
     */
    @InterceptorIgnore(tenantLine = "true")
    @Select("SELECT * FROM user")
    List<User> selectAll();
}
```

### 手动设置拦截器忽略执行策略

从 **3.5.3** 版本开始，可以手动设置拦截器的忽略执行策略，这比注解更加灵活。

```java
@Service
public class AdvancedUserService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 手动设置忽略租户插件
     */
    public List<User> listAllUsersManually() {
        // 请尽量使用 try finally 的方式来保证能正确得到关闭
        try {
            // 设置忽略租户插件
            InterceptorIgnoreHelper.handle(
                IgnoreStrategy.builder()
                    .tenantLine(true)
                    .build()
            );

            // 执行逻辑（此操作会忽略多租户插件）
            return userMapper.selectList(null);

        } finally {
            // 关闭忽略策略
            InterceptorIgnoreHelper.clearIgnoreStrategy();
        }
    }

    /**
     * 批量操作时忽略多个插件
     */
    public void batchProcessIgnoreMultiple() {
        try {
            // 设置忽略多个插件
            InterceptorIgnoreHelper.handle(
                IgnoreStrategy.builder()
                    .tenantLine(true)      // 忽略多租户
                    .blockAttack(true)     // 忽略防全表更新
                    .pagination(true)      // 忽略分页
                    .build()
            );

            // 执行批量操作
            batchOperationLogic();

        } finally {
            InterceptorIgnoreHelper.clearIgnoreStrategy();
        }
    }

    private void batchOperationLogic() {
        // 批量处理逻辑
    }
}
```

### SQL 解析缓存配置

MyBatis-Plus 支持本地缓存 SQL 解析，这对于分页、租户等插件特别有效。

#### 默认缓存配置

```java
static {
    // 默认支持序列化 FstSerialCaffeineJsqlParseCache，JdkSerialCaffeineJsqlParseCache
    JsqlParserGlobal.setJsqlParseCache(
        new JdkSerialCaffeineJsqlParseCache(
            (cache) -> cache
                .maximumSize(1024)                    // 最大缓存条目数
                .expireAfterWrite(5, TimeUnit.SECONDS) // 写入后 5 秒过期
        )
    );
}
```

#### 自定义缓存配置

```java
@Configuration
public class JsqlParserCacheConfig {

    static {
        // 配置 SQL 解析缓存
        JsqlParserGlobal.setJsqlParseCache(
            new JdkSerialCaffeineJsqlParseCache(
                cache -> cache
                    .maximumSize(2048)                      // 最大缓存 2048 条
                    .expireAfterAccess(10, TimeUnit.MINUTES) // 访问后 10 分钟过期
                    .recordStats()                          // 记录缓存统计
            )
        );
    }
}
```

### 线程池配置（3.5.6+）

从 **3.5.6** 开始，支持 JSQLParser(4.9) 的线程池解析复用，可减少重复创建线程池带来的性能开销。

#### 默认线程池

默认创建固定线程池，核心线程数:

```
(Runtime.getRuntime().availableProcessors() + 1) / 2
```

#### 自定义线程池

如果默认的线程池方式不太符合实际部署情况，可以指定自定义线程池：

```java
@Configuration
public class JsqlParserThreadPoolConfig {

    static {
        // 自定义线程池
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
            10,  // 核心线程数
            20,  // 最大线程数
            60L, // 空闲线程存活时间
            TimeUnit.SECONDS,
            new LinkedBlockingQueue<>(100), // 任务队列
            new ThreadFactoryBuilder()
                .setNameFormat("jsqlparser-%d")
                .setDaemon(true)
                .build(),
            new ThreadPoolExecutor.CallerRunsPolicy() // 拒绝策略
        );

        // 设置自定义线程池
        JsqlParserGlobal.setJsqlParserExecutor(executor);

        // 注意：自行创建的线程池需要注意自行关闭
        Runtime.getRuntime().addShutdownHook(new Thread(() -> {
            executor.shutdown();
            try {
                if (!executor.awaitTermination(5, TimeUnit.SECONDS)) {
                    executor.shutdownNow();
                }
            } catch (InterruptedException e) {
                executor.shutdownNow();
                Thread.currentThread().interrupt();
            }
        }));
    }
}
```

### SQL 预处理

如果需要对 JSQLParser 的 SQL 语句进行加工处理，可以通过以下方式指定：

```java
@Configuration
public class SqlPreProcessConfig {

    static {
        // 设置 SQL 预处理器
        JsqlParserGlobal.setSqlPreProcess(
            (sql, jsqlParser) -> {
                // 在这里对 SQL 进行预处理
                // 例如：格式化 SQL、去除注释等
                String processedSql = sql.trim();

                // 可以进行自定义的 SQL 转换
                // processedSql = customTransform(processedSql);

                return processedSql;
            }
        );
    }
}
```

### 自定义插件开发

如果需要开发自定义插件，可以实现 `InnerInterceptor` 接口：

```java
/**
 * 自定义 SQL 审计插件
 */
@Component
@Log4j2
public class SqlAuditInnerInterceptor implements InnerInterceptor {

    @Override
    public void beforeQuery(Executor executor, MappedStatement ms,
                           Object parameter, RowBounds rowBounds,
                           ResultHandler resultHandler, BoundSql boundSql)
                           throws SQLException {
        String sql = boundSql.getSql();
        log.info("执行查询 SQL: {}", sql);

        // 记录查询性能指标
        long startTime = System.currentTimeMillis();
        // 注意：这里只是记录开始时间，实际性能统计需要在 afterQuery 中完成
        // 但 InnerInterceptor 接口没有提供 afterQuery 方法
        // 可以考虑使用 AOP 切面来实现完整的性能监控
    }

    @Override
    public void beforeUpdate(Executor executor, MappedStatement ms,
                            Object parameter) throws SQLException {
        // 获取 SQL 信息
        BoundSql boundSql = ms.getBoundSql(parameter);
        String sql = boundSql.getSql();

        log.info("执行更新 SQL: {}", sql);
        log.info("参数: {}", parameter);

        // 可以在这里添加审计逻辑
        auditSqlExecution(ms.getId(), sql, parameter);
    }

    /**
     * 审计 SQL 执行
     */
    private void auditSqlExecution(String statementId, String sql, Object parameter) {
        // 实现审计逻辑
        // 例如：记录到日志、发送到审计系统等
        log.info("SQL 审计 - Statement: {}, SQL: {}, Parameter: {}",
                 statementId, sql, parameter);
    }
}
```

**配置自定义插件**:

```java
@Configuration
public class CustomPluginConfig {

    @Autowired
    private SqlAuditInnerInterceptor sqlAuditInterceptor;

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        // 添加内置插件
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));

        // 添加自定义插件
        interceptor.addInnerInterceptor(sqlAuditInterceptor);

        return interceptor;
    }
}
```

### 最佳实践

#### 1. 插件选择建议

```java
/**
 * 根据业务需求选择合适的插件组合
 */
@Configuration
public class PluginSelectionConfig {

    @Bean
    public MybatisPlusInterceptor basicPlugins() {
        // 基础配置：仅包含分页插件
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }

    @Bean
    public MybatisPlusInterceptor tenantPlugins() {
        // 多租户配置：包含多租户 + 分页
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new TenantLineInnerInterceptor());
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }

    @Bean
    public MybatisPlusInterceptor enterprisePlugins() {
        // 企业级配置：完整插件链
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new TenantLineInnerInterceptor());
        interceptor.addInnerInterceptor(new DynamicTableNameInnerInterceptor());
        interceptor.addInnerInterceptor(new BlockAttackInnerInterceptor());
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return interceptor;
    }
}
```

#### 2. 性能优化

```java
@Configuration
public class PluginPerformanceConfig {

    static {
        // 配置 SQL 解析缓存
        JsqlParserGlobal.setJsqlParseCache(
            new JdkSerialCaffeineJsqlParseCache(
                cache -> cache
                    .maximumSize(2048)
                    .expireAfterAccess(10, TimeUnit.MINUTES)
                    .recordStats()
            )
        );

        // 配置线程池
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
            Math.max(Runtime.getRuntime().availableProcessors() / 2, 4),
            Runtime.getRuntime().availableProcessors(),
            60L,
            TimeUnit.SECONDS,
            new LinkedBlockingQueue<>(200),
            new ThreadFactoryBuilder()
                .setNameFormat("jsqlparser-%d")
                .setDaemon(true)
                .build(),
            new ThreadPoolExecutor.CallerRunsPolicy()
        );

        JsqlParserGlobal.setJsqlParserExecutor(executor);
    }
}
```

#### 3. 插件监控

```java
@Component
@Log4j2
public class PluginPerformanceMonitor {

    private final AtomicLong queryCount = new AtomicLong(0);
    private final AtomicLong updateCount = new AtomicLong(0);
    private final ConcurrentHashMap<String, AtomicLong> methodStats = new ConcurrentHashMap<>();

    @Scheduled(fixedRate = 60000) // 每分钟输出一次统计
    public void reportStats() {
        log.info("插件性能统计:");
        log.info("查询次数: {}", queryCount.get());
        log.info("更新次数: {}", updateCount.get());

        methodStats.forEach((method, count) -> {
            log.info("方法 {} 调用次数: {}", method, count.get());
        });
    }
}
```

---

## SQL 注入器

**URL:** https://baomidou.com/guides/sql-injector/

### 功能概述

MyBatis-Plus 提供了灵活的机制来注入自定义的 SQL 方法，这通过 `sqlInjector` 全局配置实现。通过实现 `ISqlInjector` 接口或继承 `AbstractSqlInjector` 抽象类，可以注入自定义的通用方法到 MyBatis 容器中。

### 使用场景

SQL 注入器适用于以下场景：

1. **自定义查询方法**: 标准的 CRUD 操作无法满足复杂查询需求时
2. **复杂数据处理**: 需要多表联结、子查询、聚合函数等
3. **性能优化**: 针对特定查询场景进行性能优化
4. **数据权限控制**: 根据用户权限动态生成 SQL 语句
5. **遗留系统迁移**: 保留原有 SQL 语句结构，实现平滑过渡

### ISqlInjector 接口

```java
public interface ISqlInjector {
    /**
     * 检查 SQL 是否已经注入(已经注入过不再注入)
     *
     * @param builderAssistant mapper 构建助手
     * @param mapperClass      mapper 接口的 class 对象
     */
    void inspectInject(MapperBuilderAssistant builderAssistant, Class<?> mapperClass);
}
```

### 自定义全局方法攻略

#### 步骤 1: 定义 SQL

创建自定义方法类，继承 `AbstractMethod`:

```java
import com.baomidou.mybatisplus.core.injector.AbstractMethod;
import com.baomidou.mybatisplus.core.metadata.TableInfo;
import org.apache.ibatis.executor.keygen.Jdbc3KeyGenerator;
import org.apache.ibatis.executor.keygen.KeyGenerator;
import org.apache.ibatis.mapping.MappedStatement;
import org.apache.ibatis.mapping.SqlSource;
import org.apache.ibatis.mapping.SqlCommandType;

/**
 * MySQL 批量插入所有字段
 * 插入数据时，包含主键字段和所有普通字段
 */
public class MysqlInsertAllBatch extends AbstractMethod {

    /**
     * 构造函数，定义方法名
     */
    public MysqlInsertAllBatch() {
        super("mysqlInsertAllBatch");
    }

    @Override
    public MappedStatement injectMappedStatement(
        Class<?> mapperClass,
        Class<?> modelClass,
        TableInfo tableInfo
    ) {
        // 定义 SQL 语句: insert into table(id,field1,field2) values (...),(...),(...)
        String sql = "INSERT INTO " + tableInfo.getTableName() + "(" +
            tableInfo.getKeyColumn() + "," +  // 主键字段
            tableInfo.getFieldList()
                .stream()
                .map(TableFieldInfo::getColumn)
                .collect(Collectors.joining(",")) +
            ") VALUES ";

        // 定义值部分: (#{entity.id},#{entity.field1},#{entity.field2})
        String value = "(" +
            "#{" + ENTITY + DOT + tableInfo.getKeyProperty() + "}" + "," +
            tableInfo.getFieldList()
                .stream()
                .map(field -> "#{" + ENTITY + DOT + field.getProperty() + "}")
                .collect(Collectors.joining(",")) +
            ")";

        // 使用 foreach 标签生成批量插入
        String valuesScript = SqlScriptUtils.convertForeach(
            value, "list", null, ENTITY, COMMA
        );

        // 创建 SqlSource
        SqlSource sqlSource = super.createSqlSource(
            configuration,
            "<script>" + sql + valuesScript + "</script>",
            modelClass
        );

        // 设置主键生成器
        KeyGenerator keyGenerator = tableInfo.getIdType() == IdType.AUTO
            ? Jdbc3KeyGenerator.INSTANCE
            : NoKeyGenerator.INSTANCE;

        // 返回 MappedStatement
        // 第三个参数必须和 BaseMapper 的自定义方法名一致
        return this.addInsertMappedStatement(
            mapperClass,
            modelClass,
            this.methodName,
            sqlSource,
            keyGenerator,
            tableInfo.getKeyProperty(),
            tableInfo.getKeyColumn()
        );
    }
}
```

**更多自定义方法示例**:

```java
/**
 * 删除所有记录
 */
public class DeleteAll extends AbstractMethod {

    public DeleteAll() {
        super("deleteAll");
    }

    @Override
    public MappedStatement injectMappedStatement(
        Class<?> mapperClass,
        Class<?> modelClass,
        TableInfo tableInfo
    ) {
        String sql = "DELETE FROM " + tableInfo.getTableName();
        SqlSource sqlSource = super.createSqlSource(
            configuration,
            sql,
            modelClass
        );
        return this.addDeleteMappedStatement(
            mapperClass,
            this.methodName,
            sqlSource
        );
    }
}

/**
 * 插入所有字段（包含 null 值）
 */
public class MyInsertAll extends AbstractMethod {

    public MyInsertAll() {
        super("myInsertAll");
    }

    @Override
    public MappedStatement injectMappedStatement(
        Class<?> mapperClass,
        Class<?> modelClass,
        TableInfo tableInfo
    ) {
        // 构建插入 SQL，包含所有字段
        String columns = tableInfo.getKeyColumn() + "," +
            tableInfo.getFieldList()
                .stream()
                .map(TableFieldInfo::getColumn)
                .collect(Collectors.joining(","));

        String values = "#{" + ENTITY + DOT + tableInfo.getKeyProperty() + "}," +
            tableInfo.getFieldList()
                .stream()
                .map(field -> "#{" + ENTITY + DOT + field.getProperty() + "}")
                .collect(Collectors.joining(","));

        String sql = String.format(
            "INSERT INTO %s (%s) VALUES (%s)",
            tableInfo.getTableName(),
            columns,
            values
        );

        SqlSource sqlSource = super.createSqlSource(
            configuration,
            sql,
            modelClass
        );

        KeyGenerator keyGenerator = tableInfo.getIdType() == IdType.AUTO
            ? Jdbc3KeyGenerator.INSTANCE
            : NoKeyGenerator.INSTANCE;

        return this.addInsertMappedStatement(
            mapperClass,
            modelClass,
            this.methodName,
            sqlSource,
            keyGenerator,
            tableInfo.getKeyProperty(),
            tableInfo.getKeyColumn()
        );
    }
}
```

#### 步骤 2: 注册自定义方法

创建自定义 SQL 注入器，继承 `DefaultSqlInjector`:

```java
import com.baomidou.mybatisplus.core.injector.DefaultSqlInjector;
import com.baomidou.mybatisplus.core.injector.AbstractMethod;
import org.springframework.stereotype.Component;

import java.util.List;

/**
 * 自定义 SQL 注入器
 * 用于注册自定义的全局方法
 */
@Component
public class MyLogicSqlInjector extends DefaultSqlInjector {

    @Override
    public List<AbstractMethod> getMethodList(Class<?> mapperClass) {
        // 获取默认的方法列表（包含 BaseMapper 的所有方法）
        List<AbstractMethod> methodList = super.getMethodList(mapperClass);

        // 添加自定义方法
        methodList.add(new DeleteAll());              // 删除所有
        methodList.add(new MyInsertAll());            // 插入所有字段
        methodList.add(new MysqlInsertAllBatch());    // MySQL 批量插入

        return methodList;
    }
}
```

#### 步骤 3: 定义 BaseMapper

创建自定义的 BaseMapper 接口，定义自定义方法：

```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import org.apache.ibatis.annotations.Param;

import java.util.List;

/**
 * 自定义 BaseMapper
 * 扩展 MyBatis-Plus 的 BaseMapper，添加自定义方法
 */
public interface MyBaseMapper<T> extends BaseMapper<T> {

    /**
     * 删除所有记录
     * @return 删除的行数
     */
    Integer deleteAll();

    /**
     * 插入实体（包含所有字段）
     * @param entity 实体对象
     * @return 插入的行数
     */
    int myInsertAll(T entity);

    /**
     * MySQL 批量插入
     * @param batchList 批量实体列表
     * @return 插入的行数
     */
    int mysqlInsertAllBatch(@Param("list") List<T> batchList);
}
```

**使用自定义 BaseMapper**:

```java
/**
 * UserMapper 继承自定义 BaseMapper
 */
@Mapper
public interface UserMapper extends MyBaseMapper<User> {
    // 继承了 BaseMapper 的所有方法
    // 同时也继承了自定义的方法：deleteAll, myInsertAll, mysqlInsertAllBatch
}
```

#### 步骤 4: 配置 SqlInjector

**在 application.yml 中配置**:

```yaml
mybatis-plus:
  # 配置自定义 SQL 注入器
  global-config:
    db-config:
      # 其他配置...
    # 配置自定义 SQL 注入器
    sql-injector: com.example.mybatisplus.injector.MyLogicSqlInjector
```

**在 application.properties 中配置**:

```properties
# 配置自定义 SQL 注入器
mybatis-plus.global-config.sql-injector=com.example.mybatisplus.injector.MyLogicSqlInjector
```

**使用 @Component 注解**:

如果自定义注入器使用了 `@Component` 注解（推荐方式），则无需额外配置，Spring 会自动扫描并注入：

```java
@Component  // 使用 @Component 注解，无需配置
public class MyLogicSqlInjector extends DefaultSqlInjector {
    // ...
}
```

### 使用示例

#### 示例 1: 使用自定义方法

```java
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 批量插入用户
     */
    public void batchInsertUsers(List<User> users) {
        // 使用自定义的批量插入方法
        userMapper.mysqlInsertAllBatch(users);
    }

    /**
     * 插入用户（包含所有字段）
     */
    public void insertUserWithAllFields(User user) {
        // 使用自定义的插入所有字段方法
        userMapper.myInsertAll(user);
    }

    /**
     * 清空用户表
     */
    public void clearAllUsers() {
        // 使用自定义的删除所有方法
        userMapper.deleteAll();
    }
}
```

#### 示例 2: 复杂查询自定义方法

```java
/**
 * 批量更新方法
 */
public class UpdateBatchByIdWithSelective extends AbstractMethod {

    public UpdateBatchByIdWithSelective() {
        super("updateBatchByIdWithSelective");
    }

    @Override
    public MappedStatement injectMappedStatement(
        Class<?> mapperClass,
        Class<?> modelClass,
        TableInfo tableInfo
    ) {
        // 构建 SQL: UPDATE table SET field1 = CASE id WHEN ? THEN ? END, field2 = CASE id WHEN ? THEN ? END WHERE id IN (...)
        StringBuilder sql = new StringBuilder();
        sql.append("UPDATE ").append(tableInfo.getTableName()).append(" SET ");

        // 为每个字段生成 CASE WHEN 语句
        for (TableFieldInfo fieldInfo : tableInfo.getFieldList()) {
            sql.append(fieldInfo.getColumn()).append(" = CASE ");
            sql.append(tableInfo.getKeyColumn());

            sql.append(" ");
            sql.append(SqlScriptUtils.convertForeach(
                "WHEN #{" + ENTITY + "." + tableInfo.getKeyProperty() + "} " +
                "THEN #{" + ENTITY + DOT + fieldInfo.getProperty() + "}",
                "list",
                "item",
                ENTITY,
                ""
            ));

            sql.append(" END, ");
        }

        // 移除最后的逗号
        sql.setLength(sql.length() - 2);

        // 添加 WHERE 条件
        sql.append(" WHERE ").append(tableInfo.getKeyColumn()).append(" IN ");
        sql.append("<foreach collection=\"list\" item=\"item\" open=\"(\" separator=\",\" close=\")\">");
        sql.append("#{item.").append(tableInfo.getKeyProperty()).append("}");
        sql.append("</foreach>");

        SqlSource sqlSource = super.createSqlSource(
            configuration,
            sql.toString(),
            modelClass
        );

        return this.addUpdateMappedStatement(
            mapperClass,
            modelClass,
            this.methodName,
            sqlSource
        );
    }
}
```

#### 示例 3: 条件插入自定义方法

```java
/**
 * 插入或更新（Upsert）
 */
public class InsertOrUpdate extends AbstractMethod {

    public InsertOrUpdate() {
        super("insertOrUpdate");
    }

    @Override
    public MappedStatement injectMappedStatement(
        Class<?> mapperClass,
        Class<?> modelClass,
        TableInfo tableInfo
    ) {
        // 构建 MySQL 特有的 INSERT ... ON DUPLICATE KEY UPDATE 语句
        StringBuilder sql = new StringBuilder();
        sql.append("INSERT INTO ").append(tableInfo.getTableName()).append(" (");

        // 字段列表
        sql.append(tableInfo.getKeyColumn()).append(",");
        sql.append(
            tableInfo.getFieldList()
                .stream()
                .map(TableFieldInfo::getColumn)
                .collect(Collectors.joining(","))
        );
        sql.append(") VALUES (");

        // 值列表
        sql.append("#{").append(ENTITY).append(DOT).append(tableInfo.getKeyProperty()).append("},");
        sql.append(
            tableInfo.getFieldList()
                .stream()
                .map(field -> "#{" + ENTITY + DOT + field.getProperty() + "}")
                .collect(Collectors.joining(","))
        );
        sql.append(")");

        // ON DUPLICATE KEY UPDATE 子句
        sql.append(" ON DUPLICATE KEY UPDATE ");
        sql.append(
            tableInfo.getFieldList()
                .stream()
                .map(field -> field.getColumn() + " = VALUES(" + field.getColumn() + ")")
                .collect(Collectors.joining(","))
        );

        SqlSource sqlSource = super.createSqlSource(
            configuration,
            sql.toString(),
            modelClass
        );

        KeyGenerator keyGenerator = tableInfo.getIdType() == IdType.AUTO
            ? Jdbc3KeyGenerator.INSTANCE
            : NoKeyGenerator.INSTANCE;

        return this.addInsertMappedStatement(
            mapperClass,
            modelClass,
            this.methodName,
            sqlSource,
            keyGenerator,
            tableInfo.getKeyProperty(),
            tableInfo.getKeyColumn()
        );
    }
}
```

### 注意事项

1. **方法名必须一致**: 自定义方法的方法名必须与 BaseMapper 接口中定义的方法名一致
2. **SQL 注入风险**: 自定义 SQL 时要注意 SQL 注入风险，尽量使用参数化查询
3. **数据库兼容性**: 某些自定义方法可能只适用于特定数据库，如 MySQL 的 `INSERT ... ON DUPLICATE KEY UPDATE`
4. **性能考虑**: 批量操作时要注意性能，避免一次操作过多数据
5. **事务管理**: 自定义方法的事务管理与普通方法相同，使用 `@Transactional` 注解

### 最佳实践

#### 1. 分层设计

```java
/**
 * 基础自定义方法注入器
 */
@Component
public class BaseCustomSqlInjector extends DefaultSqlInjector {
    @Override
    public List<AbstractMethod> getMethodList(Class<?> mapperClass) {
        List<AbstractMethod> methodList = super.getMethodList(mapperClass);
        // 添加基础自定义方法
        methodList.add(new DeleteAll());
        methodList.add(new InsertOrUpdate());
        return methodList;
    }
}

/**
 * MySQL 专用方法注入器
 */
@Component
@ConditionalOnProperty(name = "spring.datasource.driver-class-name", havingValue = "com.mysql.cj.jdbc.Driver")
public class MysqlCustomSqlInjector extends BaseCustomSqlInjector {
    @Override
    public List<AbstractMethod> getMethodList(Class<?> mapperClass) {
        List<AbstractMethod> methodList = super.getMethodList(mapperClass);
        // 添加 MySQL 专用方法
        methodList.add(new MysqlInsertAllBatch());
        return methodList;
    }
}
```

#### 2. 方法复用

```java
/**
 * 通用批量操作方法
 */
public class CommonBatchMethod extends AbstractMethod {

    private final String sqlTemplate;

    public CommonBatchMethod(String methodName, String sqlTemplate) {
        super(methodName);
        this.sqlTemplate = sqlTemplate;
    }

    @Override
    public MappedStatement injectMappedStatement(
        Class<?> mapperClass,
        Class<?> modelClass,
        TableInfo tableInfo
    ) {
        String sql = String.format(
            sqlTemplate,
            tableInfo.getTableName(),
            tableInfo.getKeyColumn()
        );

        SqlSource sqlSource = super.createSqlSource(
            configuration,
            sql,
            modelClass
        );

        return this.addUpdateMappedStatement(
            mapperClass,
            modelClass,
            this.methodName,
            sqlSource
        );
    }
}
```

---

## 批量操作

**URL:** https://baomidou.com/guides/batch-operation/

### 功能概览

批量操作是一种高效处理大量数据的技术，允许开发者一次性执行多个数据库操作，从而减少与数据库的交互次数，提高数据处理的效率和性能。

### 类结构说明

#### MybatisBatch<T>

批量操作的核心类，用于管理批量操作的执行。

```java
public class MybatisBatch<T> {
    /**
     * 构造函数
     * @param sqlSessionFactory SqlSession 工厂
     * @param data 要处理的数据列表
     */
    public MybatisBatch(SqlSessionFactory sqlSessionFactory, List<T> data);

    /**
     * 执行批量操作
     * @param method 执行的方法
     * @return 批量执行结果
     */
    public List<BatchResult> execute(MybatisBatch.Method<T> method);
}
```

#### MybatisBatch.Method<T>

批量操作的方法定义，简化框架内部操作方法调用。

```java
public static class Method<T> {
    /**
     * 插入方法
     * @param mapperClass Mapper 接口类
     * @return 方法实例
     */
    public static <T> Method<T> insert(Class<?> mapperClass);

    /**
     * 根据 ID 插入
     * @param mapperClass Mapper 接口类
     * @param function 数据转换函数
     * @return 方法实例
     */
    public static <T> Method<T> insert(
        Class<?> mapperClass,
        Function<T, ?> function
    );

    /**
     * 更新方法
     * @param mapperClass Mapper 接口类
     * @return 方法实例
     */
    public static <T> Method<T> update(Class<?> mapperClass);

    /**
     * 删除方法
     * @param mapperClass Mapper 接口类
     * @return 方法实例
     */
    public static <T> Method<T> delete(Class<?> mapperClass);

    /**
     * 获取自定义方法
     * @param methodName 方法名
     * @return 方法实例
     */
    public Method<T> get(String methodName);

    /**
     * 获取带参数的自定义方法
     * @param methodName 方法名
     * @param function 参数转换函数
     * @return 方法实例
     */
    public Method<T> get(
        String methodName,
        Function<T, Map<String, Object>> function
    );
}
```

#### BatchResult<T>

批量操作的结果对象。

```java
public class BatchResult {
    /**
     * 获取影响行数
     */
    public int getUpdateCounts();

    /**
     * 获取执行的 MappedStatement
     */
    public MappedStatement getStatement();
}
```

### 使用步骤

#### 步骤 1: 准备数据

```java
// 准备要批量插入的数据
List<H2User> userList = Arrays.asList(
    new H2User(2000L, "张三"),
    new H2User(2001L, "李四"),
    new H2User(2002L, "王五")
);
```

#### 步骤 2: 创建 MybatisBatch 实例

```java
@Autowired
private SqlSessionFactory sqlSessionFactory;

// 创建批量操作实例
MybatisBatch<H2User> mybatisBatch = new MybatisBatch<>(
    sqlSessionFactory,
    userList
);
```

#### 步骤 3: 定义批量操作方法

```java
// 创建批量插入方法
MybatisBatch.Method<H2User> method = new MybatisBatch.Method<>(
    H2UserMapper.class
);
```

#### 步骤 4: 执行批量操作

```java
// 执行批量插入
List<BatchResult> results = mybatisBatch.execute(
    method.insert()
);

// 处理结果
results.forEach(result -> {
    System.out.println("影响行数: " + result.getUpdateCounts());
});
```

### 返回值说明

**返回类型**: `List<BatchResult>`

**返回内容**: 每次执行 MappedStatement + SQL 的操作结果分组。

**注意**:
- 例如批量根据 ID 更新，若 10 条数据中 5 条更新一个字段，5 条更新两个字段，则返回值为容量为 2 的 List
- 分别存储 5 条记录的更新情况
- 每个不同的 SQL 会产生一个 BatchResult

### 使用示例

#### 示例 1: 批量插入

```java
@Service
public class UserService {

    @Autowired
    private SqlSessionFactory sqlSessionFactory;

    /**
     * 批量插入用户
     */
    public void batchInsertUsers(List<User> users) {
        // 创建批量操作实例
        MybatisBatch<User> mybatisBatch = new MybatisBatch<>(
            sqlSessionFactory,
            users
        );

        // 执行批量插入
        List<BatchResult> results = mybatisBatch.execute(
            new MybatisBatch.Method<>(UserMapper.class).insert()
        );

        // 统计插入结果
        int totalRows = results.stream()
            .mapToInt(BatchResult::getUpdateCounts)
            .sum();

        System.out.println("成功插入 " + totalRows + " 条记录");
    }
}
```

#### 示例 2: 批量插入（带数据转换）

```java
/**
 * 批量插入，使用自定义数据转换
 */
public void batchInsertWithTransform(List<Long> userIds) {
    // 创建批量操作实例
    MybatisBatch<Long> mybatisBatch = new MybatisBatch<>(
        sqlSessionFactory,
        userIds
    );

    // 执行批量插入，使用函数转换数据
    List<BatchResult> results = mybatisBatch.execute(
        new MybatisBatch.Method<>(UserMapper.class).insert(id -> {
            User user = new User();
            user.setTestId(id);
            user.setName("用户" + id);
            return user;
        })
    );

    System.out.println("批量插入完成，结果数: " + results.size());
}
```

#### 示例 3: 批量更新

```java
/**
 * 批量更新用户
 */
public void batchUpdateUsers(List<User> users) {
    MybatisBatch<User> mybatisBatch = new MybatisBatch<>(
        sqlSessionFactory,
        users
    );

    // 执行批量更新
    List<BatchResult> results = mybatisBatch.execute(
        new MybatisBatch.Method<>(UserMapper.class).update()
    );

    results.forEach(result -> {
        System.out.println("更新影响行数: " + result.getUpdateCounts());
    });
}
```

#### 示例 4: 批量删除

```java
/**
 * 批量删除用户
 */
public void batchDeleteUsers(List<Long> userIds) {
    MybatisBatch<Long> mybatisBatch = new MybatisBatch<>(
        sqlSessionFactory,
        userIds
    );

    // 执行批量删除
    List<BatchResult> results = mybatisBatch.execute(
        new MybatisBatch.Method<>(UserMapper.class).delete()
    );

    System.out.println("批量删除完成");
}
```

#### 示例 5: 使用自定义 Mapper 方法

```java
public interface UserMapper extends BaseMapper<User> {
    /**
     * 自定义插入方法（无参数注解）
     */
    @Insert("insert into user(name, version) values(#{name}, #{version})")
    int myInsertWithoutParam(User user);

    /**
     * 自定义插入方法（带参数注解）
     */
    @Insert("insert into user(name, version) values(#{user.name}, #{user.version})")
    int myInsertWithParam(@Param("user") User user);
}

/**
 * 使用自定义方法批量插入
 */
public void batchInsertWithCustomMethod() {
    // 准备数据
    List<User> userList = new ArrayList<>();
    for (int i = 0; i < 1000; i++) {
        userList.add(new User("用户" + i));
    }

    MybatisBatch<User> mybatisBatch = new MybatisBatch<>(
        sqlSessionFactory,
        userList
    );
    MybatisBatch.Method<User> method = new MybatisBatch.Method<>(
        UserMapper.class
    );

    // 使用自定义方法（无参数注解）
    mybatisBatch.execute(method.get("myInsertWithoutParam"));

    // 使用自定义方法（带参数注解）
    mybatisBatch.execute(
        method.get("myInsertWithParam", (user) -> {
            Map<String, Object> map = new HashMap<>();
            map.put("user", user);
            return map;
        })
    );
}
```

#### 示例 6: 使用工具类简化调用

```java
/**
 * 框架提供的 MybatisBatchUtils 静态方法调用
 */
@Service
public class BatchService {

    @Autowired
    private SqlSessionFactory sqlSessionFactory;

    /**
     * 批量插入（使用工具类）
     */
    public void batchInsertWithUtils(List<User> users) {
        List<BatchResult> results = MybatisBatchUtils.execute(
            sqlSessionFactory,
            users,
            UserMapper.class,
            (mapper, list) -> {
                // 自定义批量逻辑
                return mapper.insertBatchSomeColumn(list);
            }
        );

        System.out.println("批量插入完成");
    }
}
```

### 高级用法

#### 1. 分批处理

```java
/**
 * 大数据量分批处理
 */
public void batchInsertLargeData(List<User> allUsers) {
    int batchSize = 1000; // 每批 1000 条

    for (int i = 0; i < allUsers.size(); i += batchSize) {
        int end = Math.min(i + batchSize, allUsers.size());
        List<User> batch = allUsers.subList(i, end);

        // 执行批量插入
        MybatisBatch<User> mybatisBatch = new MybatisBatch<>(
            sqlSessionFactory,
            batch
        );

        List<BatchResult> results = mybatisBatch.execute(
            new MybatisBatch.Method<>(UserMapper.class).insert()
        );

        System.out.printf(
            "已处理 %d/%d 条记录%n",
            end,
            allUsers.size()
        );
    }
}
```

#### 2. 批量操作与事务

```java
@Service
public class BatchTransactionService {

    @Autowired
    private SqlSessionFactory sqlSessionFactory;

    /**
     * 批量操作（带事务）
     */
    @Transactional(rollbackFor = Exception.class)
    public void batchInsertWithTransaction(List<User> users) {
        try {
            MybatisBatch<User> mybatisBatch = new MybatisBatch<>(
                sqlSessionFactory,
                users
            );

            List<BatchResult> results = mybatisBatch.execute(
                new MybatisBatch.Method<>(UserMapper.class).insert()
            );

            System.out.println("批量插入成功");

        } catch (Exception e) {
            System.err.println("批量插入失败: " + e.getMessage());
            throw e; // 触发事务回滚
        }
    }
}
```

#### 3. 批量操作性能优化

```java
/**
 * 批量操作性能优化配置
 */
@Configuration
public class BatchPerformanceConfig {

    @Bean
    public SqlSessionFactory sqlSessionFactory(
        DataSource dataSource
    ) throws Exception {
        MybatisSqlSessionFactoryBean factory =
            new MybatisSqlSessionFactoryBean();
        factory.setDataSource(dataSource);

        // 配置批量操作优化
        MybatisConfiguration configuration = new MybatisConfiguration();
        configuration.setDefaultExecutorType(ExecutorType.BATCH);
        configuration.setJdbcTypeForNull(JdbcType.NULL);

        factory.setConfiguration(configuration);
        return factory.getObject();
    }
}
```

#### 4. 批量操作异常处理

```java
/**
 * 批量操作异常处理
 */
public void batchInsertWithErrorHandling(List<User> users) {
    MybatisBatch<User> mybatisBatch = new MybatisBatch<>(
        sqlSessionFactory,
        users
    );

    try {
        List<BatchResult> results = mybatisBatch.execute(
            new MybatisBatch.Method<>(UserMapper.class).insert()
        );

        // 检查每个批次的结果
        for (BatchResult result : results) {
            if (result.getUpdateCounts() == 0) {
                System.err.println("警告：某些记录可能未成功插入");
            }
        }

    } catch (Exception e) {
        System.err.println("批量插入失败: " + e.getMessage());

        // 可以选择记录失败的记录
        // 或者进行重试等处理
    }
}
```

### 最佳实践

#### 1. 批量大小控制

```java
/**
 * 根据不同操作类型选择合适的批量大小
 */
public class BatchSizeController {

    // 插入操作：建议 500-1000 条/批
    private static final int INSERT_BATCH_SIZE = 1000;

    // 更新操作：建议 200-500 条/批
    private static final int UPDATE_BATCH_SIZE = 500;

    // 删除操作：建议 1000-2000 条/批
    private static final int DELETE_BATCH_SIZE = 2000;

    public void batchInsert(List<User> users) {
        List<List<User>> batches = Lists.partition(
            users,
            INSERT_BATCH_SIZE
        );

        batches.forEach(batch -> {
            MybatisBatch<User> mybatisBatch = new MybatisBatch<>(
                sqlSessionFactory,
                batch
            );
            mybatisBatch.execute(
                new MybatisBatch.Method<>(UserMapper.class).insert()
            );
        });
    }
}
```

#### 2. 批量操作监控

```java
/**
 * 批量操作性能监控
 */
@Component
public class BatchMonitor {

    private final MeterRegistry meterRegistry;

    public BatchMonitor(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
    }

    public <T> List<BatchResult> executeWithMetrics(
        List<T> data,
        MybatisBatch.Method<T> method
    ) {
        Timer.Sample sample = Timer.start(meterRegistry);

        try {
            List<BatchResult> results = /* 执行批量操作 */;

            sample.stop(
                Timer.builder("batch.operation.duration")
                    .tag("operation", "batch")
                    .tag("size", String.valueOf(data.size()))
                    .register(meterRegistry)
            );

            // 记录成功数量
            meterRegistry.counter("batch.operation.success",
                "size", String.valueOf(data.size())
            ).increment();

            return results;

        } catch (Exception e) {
            // 记录失败数量
            meterRegistry.counter("batch.operation.failure",
                "size", String.valueOf(data.size())
            ).increment();
            throw e;
        }
    }
}
```

#### 3. 使用 LOAD DATA 优化导入

```java
/**
 * 使用 MySQL 的 LOAD DATA INFILE 进行高性能导入
 */
@Service
public class HighPerformanceImportService {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    /**
     * 使用 LOAD DATA INFILE 导入 CSV
     */
    public void importFromCsv(String csvFilePath) {
        String sql = String.format(
            "LOAD DATA LOCAL INFILE '%s' " +
            "INTO TABLE user " +
            "FIELDS TERMINATED BY ',' " +
            "ENCLOSED BY '\"' " +
            "LINES TERMINATED BY '\\n' " +
            "IGNORE 1 ROWS " +
            "(id, name, email, create_time)",
            csvFilePath
        );

        jdbcTemplate.execute(sql);
        System.out.println("数据导入完成");
    }
}
```

### 注意事项

1. **跨 SqlSession 缓存**: 批量操作在跨 SqlSession 下需注意缓存和数据感知问题
2. **事务管理**: 批量操作建议使用事务，确保数据一致性
3. **性能考虑**:
   - 批量操作比循环单条操作性能更好
   - 但批量大小要适中，避免内存溢出
4. **错误处理**: 批量操作中某条记录失败不会影响其他记录
5. **性能要求高**: 如果对导入表有更高的性能要求，可以采用 SQL LOAD csv 的方式

---

## 数据安全保护

**URL:** https://baomidou.com/guides/security/

### 功能概述

MyBatis-Plus 提供了完善的数据安全保护功能，旨在防止因开发人员流动而导致的敏感信息泄露。从 **3.3.2** 版本开始，MyBatis-Plus 支持通过加密配置和数据安全措施来增强数据库的安全性。

### 配置安全

#### 1. YML 配置加密

MyBatis-Plus 允许使用加密后的字符串来配置数据库连接信息。在 YML 配置文件中，以 `mpw:` 开头的配置项将被视为加密内容。

```yaml
spring:
  datasource:
    # 使用加密的 URL
    url: mpw:qRhvCwF4GOqjessEB3G+a5okP+uXXr96wcucn2Pev6Bf1oEMZ1gVpPPhdDmjQqoM

    # 使用加密的用户名
    username: mpw:Xb+EgsyuYRXw7U7sBJjBpA==

    # 使用加密的密码
    password: mpw:Hzy5iliJbwDHhjLs1L0j6w==

    driver-class-name: com.mysql.cj.jdbc.Driver
```

#### 2. 密钥加密

使用 AES 算法生成随机密钥，并对敏感数据进行加密。

```java
import com.baomidou.mybatisplus.core.toolkit.AES;

/**
 * AES 加密工具使用示例
 */
public class AesEncryptionExample {

    /**
     * 生成 16 位随机 AES 密钥
     */
    public static void generateKey() {
        // 生成随机密钥
        String randomKey = AES.generateRandomKey();
        System.out.println("生成的密钥: " + randomKey);
        // 输出示例: d1104d7c3b616f0b
    }

    /**
     * 加密数据
     */
    public static void encryptData() {
        String data = "jdbc:mysql://localhost:3306/mydb";
        String key = "d1104d7c3b616f0b";

        // 使用随机密钥加密数据
        String encryptedData = AES.encrypt(data, key);
        System.out.println("加密后的数据: " + encryptedData);
        // 输出示例: mpw:qRhvCwF4GOqjessEB3G+a5okP+uXXr96wcucn2Pev6Bf1oEMZ1gVpPPhdDmjQqoM
    }

    /**
     * 解密数据
     */
    public static void decryptData() {
        String encryptedData = "mpw:qRhvCwF4GOqjessEB3G+a5okP+uXXr96wcucn2Pev6Bf1oEMZ1gVpPPhdDmjQqoM";
        String key = "d1104d7c3b616f0b";

        // 解密数据
        String decryptedData = AES.decrypt(encryptedData.substring(4), key);
        System.out.println("解密后的数据: " + decryptedData);
    }
}
```

#### 3. 如何使用

**步骤 1: 生成密钥并加密配置**

```java
/**
 * 配置加密工具类
 */
public class ConfigEncryptionUtil {

    public static void main(String[] args) {
        // 1. 生成密钥（只需执行一次）
        String randomKey = AES.generateRandomKey();
        System.out.println("请保存以下密钥: " + randomKey);

        // 2. 加密敏感配置
        String url = "jdbc:mysql://localhost:3306/mydb";
        String username = "root";
        String password = "123456";

        String encryptedUrl = AES.encrypt(url, randomKey);
        String encryptedUsername = AES.encrypt(username, randomKey);
        String encryptedPassword = AES.encrypt(password, randomKey);

        System.out.println("加密后的配置:");
        System.out.println("url: mpw:" + encryptedUrl);
        System.out.println("username: mpw:" + encryptedUsername);
        System.out.println("password: mpw:" + encryptedPassword);
    }
}
```

**步骤 2: 配置应用启动参数**

从 **3.5.10** 开始支持系统属性与环境变量传递密钥。

**方式 1: 使用 JVM 启动参数**

```bash
# Jar 启动参数示例（在 IDEA 中设置 Program arguments，或在服务器上设置为启动环境变量）
java -jar your-application.jar --mpw.key=d1104d7c3b616f0b
```

**方式 2: 使用环境变量**

```bash
# 设置环境变量
export MPW_KEY=d1104d7c3b616f0b

# 启动应用
java -jar your-application.jar
```

**方式 3: 在 IDEA 中配置**

1. 打开 Run/Debug Configurations
2. 选择你的 Spring Boot 应用
3. 在 "VM options" 或 "Program arguments" 中添加：
   ```
   --mpw.key=d1104d7c3b616f0b
   ```

**步骤 3: 使用加密配置**

```yaml
# application.yml
spring:
  datasource:
    url: mpw:qRhvCwF4GOqjessEB3G+a5okP+uXXr96wcucn2Pev6Bf1oEMZ1gVpPPhdDmjQqoM
    username: mpw:Xb+EgsyuYRXw7U7sBJjBpA==
    password: mpw:Hzy5iliJbwDHhjLs1L0j6w==
    driver-class-name: com.mysql.cj.jdbc.Driver
```

### 数据安全

#### 1. 字段加密

MyBatis-Plus 提供了字段加密解密功能，以保护存储在数据库中的敏感数据。

```java
import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableName;
import com.baomidou.mybatisplus.extension.handlers.JacksonTypeHandler;

/**
 * 用户实体类 - 敏感字段加密示例
 */
@TableName("user")
public class User {

    private Long id;

    private String username;

    /**
     * 身份证号：加密存储
     * 使用 @TableField 注解指定类型处理器
     */
    @TableField(typeHandler = EncryptTypeHandler.class)
    private String idCard;

    /**
     * 手机号：加密存储
     */
    @TableField(typeHandler = EncryptTypeHandler.class)
    private String phoneNumber;

    /**
     * 银行卡号：加密存储
     */
    @TableField(typeHandler = EncryptTypeHandler.class)
    private String bankCard;

    // getters and setters
}
```

**自定义加密类型处理器**:

```java
import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import com.baomidou.mybatisplus.extension.handlers.AbstractJsonTypeHandler;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedTypes;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

/**
 * 加密类型处理器
 */
@MappedTypes({String.class})
public class EncryptTypeHandler extends BaseTypeHandler<String> {

    private static final String AES_KEY = "d1104d7c3b616f0b";

    @Override
    public void setNonNullParameter(
        PreparedStatement ps,
        int i,
        String parameter,
        JdbcType jdbcType
    ) throws SQLException {
        // 加密后存储到数据库
        String encrypted = AES.encrypt(parameter, AES_KEY);
        ps.setString(i, encrypted);
    }

    @Override
    public String getNullableResult(
        ResultSet rs,
        String columnName
    ) throws SQLException {
        // 从数据库读取后解密
        String encrypted = rs.getString(columnName);
        return encrypted != null ? AES.decrypt(encrypted, AES_KEY) : null;
    }

    @Override
    public String getNullableResult(
        ResultSet rs,
        int columnIndex
    ) throws SQLException {
        String encrypted = rs.getString(columnIndex);
        return encrypted != null ? AES.decrypt(encrypted, AES_KEY) : null;
    }

    @Override
    public String getNullableResult(
        CallableStatement cs,
        int columnIndex
    ) throws SQLException {
        String encrypted = cs.getString(columnIndex);
        return encrypted != null ? AES.decrypt(encrypted, AES_KEY) : null;
    }
}
```

#### 2. 字段脱敏

除了加密，还可以对敏感字段进行脱敏处理：

```java
/**
 * 脱敏工具类
 */
public class DesensitizationUtil {

    /**
     * 手机号脱敏：保留前 3 位和后 4 位
     */
    public static String maskPhoneNumber(String phone) {
        if (phone == null || phone.length() != 11) {
            return phone;
        }
        return phone.replaceAll("(\\d{3})\\d{4}(\\d{4})", "$1****$2");
    }

    /**
     * 身份证号脱敏：保留前 6 位和后 4 位
     */
    public static String maskIdCard(String idCard) {
        if (idCard == null || idCard.length() != 18) {
            return idCard;
        }
        return idCard.replaceAll("(\\d{6})\\d{8}(\\d{4})", "$1********$2");
    }

    /**
     * 银行卡号脱敏：保留前 4 位和后 4 位
     */
    public static String maskBankCard(String bankCard) {
        if (bankCard == null || bankCard.length() < 8) {
            return bankCard;
        }
        int length = bankCard.length();
        String masked = bankCard.substring(0, 4);
        masked += "****";
        masked += bankCard.substring(length - 4);
        return masked;
    }

    /**
     * 邮箱脱敏：保留第一个字符和 @ 域名
     */
    public static String maskEmail(String email) {
        if (email == null || !email.contains("@")) {
            return email;
        }
        String[] parts = email.split("@");
        String name = parts[0];
        String domain = parts[1];

        if (name.length() > 1) {
            name = name.charAt(0) + "***";
        }

        return name + "@" + domain;
    }
}

/**
 * 在 Service 层使用脱敏
 */
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    public UserVO getUserById(Long id) {
        User user = userMapper.selectById(id);

        // 转换为 VO 并脱敏
        UserVO vo = new UserVO();
        vo.setId(user.getId());
        vo.setUsername(user.getUsername());

        // 脱敏处理
        vo.setIdCard(DesensitizationUtil.maskIdCard(user.getIdCard()));
        vo.setPhoneNumber(DesensitizationUtil.maskPhoneNumber(user.getPhoneNumber()));
        vo.setBankCard(DesensitizationUtil.maskBankCard(user.getBankCard()));

        return vo;
    }
}
```

### SQL 注入安全保护

MyBatis-Plus 提供了自动和手动两种方式来检查 SQL 注入风险。

#### 1. 自动检查（3.5.3.2+ 版本支持）

```java
import com.baomidou.mybatisplus.core.toolkit.Wrappers;

/**
 * 使用 Wrappers 自动检查 SQL 注入
 */
@Service
public class SafeQueryService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 安全查询 - 开启自动 SQL 注入检查
     */
    public List<User> safeSort(String orderByField) {
        // 开启自动检查 SQL 注入
        return userMapper.selectList(
            Wrappers.<User>query()
                .checkSqlInjection()  // 开启自动检查
                .orderBy(true, true, orderByField)
        );
    }

    /**
     * 推荐方式：使用白名单
     */
    private static final Set<String> ALLOWED_FIELDS = Set.of(
        "id", "username", "email", "create_time"
    );

    public List<User> safeSortWithWhitelist(String orderByField) {
        // 白名单验证
        if (!ALLOWED_FIELDS.contains(orderByField)) {
            throw new IllegalArgumentException("非法的排序字段");
        }

        return userMapper.selectList(
            Wrappers.<User>query()
                .orderBy(true, true, orderByField)
        );
    }
}
```

#### 2. 手动校验（3.4.3.2+ 版本支持）

```java
import com.baomidou.mybatisplus.core.toolkit.SqlInjectionUtils;

/**
 * 手动 SQL 注入检查
 */
@Service
public class ManualSqlInjectionCheckService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 手动校验 SQL 注入
     */
    public List<User> manualCheckSort(String orderByField) {
        // 手动校验字段
        SqlInjectionUtils.check(orderByField);

        // 如果检查通过，不会抛出异常，继续执行
        return userMapper.selectList(
            Wrappers.<User>query()
                .orderBy(true, true, orderByField)
        );
    }

    /**
     * 推荐方式：结合白名单和手动检查
     */
    private static final Set<String> ALLOWED_FIELDS = Set.of(
        "id", "username", "email", "create_time"
    );

    public List<User> bestPracticeSort(String orderByField) {
        // 1. 白名单验证
        if (!ALLOWED_FIELDS.contains(orderByField)) {
            throw new IllegalArgumentException("非法的排序字段");
        }

        // 2. 手动 SQL 注入检查（双重保险）
        SqlInjectionUtils.check(orderByField);

        // 3. 执行查询
        return userMapper.selectList(
            Wrappers.<User>query()
                .orderBy(true, true, orderByField)
        );
    }
}
```

### 最佳实践

#### 1. 配置安全最佳实践

```yaml
# application-prod.yml (生产环境配置)
spring:
  datasource:
    # 所有敏感信息都使用加密
    url: mpw:qRhvCwF4GOqjessEB3G+a5okP+uXXr96wcucn2Pev6Bf1oEMZ1gVpPPhdDmjQqoM
    username: mpw:Xb+EgsyuYRXw7U7sBJjBpA==
    password: mpw:Hzy5iliJbwDHhjLs1L0j6w==
    driver-class-name: com.mysql.cj.jdbc.Driver

# 启动脚本示例
#!/bin/bash
export MPW_KEY=${MPW_KEY:-d1104d7c3b616f0b}
java -jar your-application.jar --spring.profiles.active=prod
```

#### 2. 字段加密最佳实践

```java
/**
 * 敏感数据加密配置类
 */
@Configuration
public class EncryptionConfig {

    /**
     * 配置加密密钥
     * 从环境变量或系统属性读取
     */
    @Bean
    public String encryptionKey() {
        // 优先使用环境变量
        String key = System.getenv("ENCRYPTION_KEY");
        if (key == null) {
            // 其次使用系统属性
            key = System.getProperty("encryption.key");
        }
        if (key == null) {
            throw new IllegalStateException(
                "加密密钥未配置，请设置环境变量 ENCRYPTION_KEY 或系统属性 encryption.key"
            );
        }
        return key;
    }
}

/**
 * 敏感字段实体
 */
@TableName("sensitive_user")
public class SensitiveUser {

    @TableId
    private Long id;

    private String username;

    /**
     * 加密字段
     */
    @TableField(typeHandler = AesEncryptTypeHandler.class)
    private String idCard;

    @TableField(typeHandler = AesEncryptTypeHandler.class)
    private String phone;

    /**
     * 脱敏字段（查询时返回）
     */
    @TableField(select = false)  // 默认不查询
    private String idCardMasked;

    @TableField(select = false)
    private String phoneMasked;
}
```

#### 3. SQL 注入防护最佳实践

```java
/**
 * SQL 注入防护工具类
 */
@Component
public class SqlInjectionProtection {

    private static final Set<String> DANGEROUS_KEYWORDS = Set.of(
        "DROP", "DELETE", "TRUNCATE", "INSERT", "UPDATE",
        "EXEC", "EXECUTE", "SCRIPT", "JAVASCRIPT"
    );

    /**
     * 检查字段名是否安全
     */
    public boolean isSafeFieldName(String fieldName) {
        if (fieldName == null || fieldName.isEmpty()) {
            return false;
        }

        // 检查是否包含危险关键字
        String upperFieldName = fieldName.toUpperCase();
        for (String keyword : DANGEROUS_KEYWORDS) {
            if (upperFieldName.contains(keyword)) {
                return false;
            }
        }

        // 检查是否只包含字母、数字、下划线
        return fieldName.matches("^[a-zA-Z_][a-zA-Z0-9_]*$");
    }

    /**
     * 安全的排序字段处理
     */
    public String sanitizeSortField(String sortField, Set<String> allowedFields) {
        // 1. 白名单检查
        if (!allowedFields.contains(sortField)) {
            throw new SecurityException("非法的排序字段: " + sortField);
        }

        // 2. 字段名格式检查
        if (!isSafeFieldName(sortField)) {
            throw new SecurityException("排序字段格式非法: " + sortField);
        }

        // 3. SQL 注入检查
        SqlInjectionUtils.check(sortField);

        return sortField;
    }
}

/**
 * 在 Service 中使用
 */
@Service
public class SecureUserService {

    @Autowired
    private UserMapper userMapper;

    @Autowired
    private SqlInjectionProtection protection;

    private static final Set<String> ALLOWED_SORT_FIELDS = Set.of(
        "id", "username", "email", "create_time", "update_time"
    );

    public List<User> listUsersSafely(String sortBy) {
        // 安全的排序字段处理
        String safeSortBy = protection.sanitizeSortField(
            sortBy,
            ALLOWED_SORT_FIELDS
        );

        // 执行查询
        return userMapper.selectList(
            Wrappers.<User>query()
                .orderBy(true, true, safeSortBy)
        );
    }
}
```

### 注意事项

1. **密钥管理**:
   - 不要将密钥硬编码在代码中
   - 使用环境变量或密钥管理服务存储密钥
   - 定期更换密钥

2. **加密性能**:
   - 字段加密会增加查询开销
   - 对于频繁查询的字段，考虑是否需要加密

3. **SQL 注入防护**:
   - 最好的预防方式是不允许任何 SQL 片段由前端传到后台
   - 强烈建议使用白名单机制
   - 结合自动检查和手动检查双重保险

4. **数据脱敏**:
   - 脱敏只用于展示，加密用于存储
   - 根据业务需求选择合适的脱敏策略

---

## 防全表更新与删除插件

**URL:** https://baomidou.com/plugins/block-attack/

### 功能概述

`BlockAttackInnerInterceptor` 是 MyBatis-Plus 框架提供的一个安全插件，专门用于防止恶意的全表更新和删除操作。该插件通过拦截 update 和 delete 语句，确保这些操作不会无意中影响到整个数据表，从而保护数据的完整性和安全性。

### 功能特性

1. **全表更新拦截**: 阻止不带 WHERE 条件的 UPDATE 操作
2. **全表删除拦截**: 阻止不带 WHERE 条件的 DELETE 操作
3. **配置灵活**: 可以通过注解忽略某些方法的拦截
4. **异常提示**: 清晰的错误提示，帮助开发者快速定位问题

### 使用方法

#### 配置插件

```java
@Configuration
public class MybatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        // 添加防全表更新删除插件
        BlockAttackInnerInterceptor blockAttackInterceptor =
            new BlockAttackInnerInterceptor();
        interceptor.addInnerInterceptor(blockAttackInterceptor);

        return interceptor;
    }
}
```

### 测试示例

#### 全表更新测试

```java
@SpringBootTest
public class BlockAttackTest {

    @Autowired
    private UserService userService;

    /**
     * 测试全表更新 - 应该被拦截
     * SQL: UPDATE user SET name=?,email=?
     */
    @Test
    public void testFullUpdate() {
        User user = new User();
        user.setId(999L);
        user.setName("custom_name");
        user.setEmail("xxx@mail.com");

        // 由于没有指定更新条件，插件将抛出异常
        // com.baomidou.mybatisplus.core.exceptions.MybatisPlusException:
        // Prohibition of table update operation
        Assertions.assertThrows(MybatisPlusException.class, () -> {
            userService.saveOrUpdate(user, null);
        });
    }
}
```

#### 部分更新测试

```java
@SpringBootTest
public class BlockAttackTest {

    @Autowired
    private UserService userService;

    /**
     * 测试部分更新 - 应该正常执行
     * SQL: UPDATE user SET name=?, email=? WHERE id = ?
     */
    @Test
    public void testPartialUpdate() {
        LambdaUpdateWrapper<User> wrapper = new LambdaUpdateWrapper<>();
        wrapper.eq(User::getId, 1);

        User user = new User();
        user.setId(10L);
        user.setName("custom_name");
        user.setEmail("xxx@mail.com");

        // 由于指定了更新条件，插件不会拦截此操作
        userService.saveOrUpdate(user, wrapper);
    }
}
```

#### 忽略拦截测试

```java
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 使用 @InterceptorIgnore 注解忽略防全表更新拦截
     * 注意：这是一个危险操作，需要谨慎使用
     */
    @InterceptorIgnore(blockAttack = "true")
    public void updateAllUsers() {
        User user = new User();
        user.setStatus("inactive");

        // 这个操作不会被拦截，但会更新所有记录
        userMapper.update(user, null);
    }
}
```

### 注意事项

1. **生产环境**: 强烈建议在生产环境启用此插件
2. **批量操作**: 批量操作如果不当也可能触发拦截
3. **忽略拦截**: 使用 `@InterceptorIgnore` 注解时要非常谨慎
4. **测试**: 在开发阶段充分测试，确保不会误拦截正常操作

---

## 多租户插件

**URL:** https://baomidou.com/plugins/tenant/

### 功能概述

`TenantLineInnerInterceptor` 是 MyBatis-Plus 提供的一个插件，用于实现多租户的数据隔离。通过这个插件，可以确保每个租户只能访问自己的数据，从而实现数据的安全隔离。

### 属性介绍

#### TenantLineHandler 接口

```java
public interface TenantLineHandler {
    /**
     * 获取租户 ID 值表达式，只支持单个 ID 值
     * @return 租户 ID 值表达式
     */
    Expression getTenantId();

    /**
     * 获取租户字段名
     * 默认字段名叫: tenant_id
     * @return 租户字段名
     */
    default String getTenantIdColumn() {
        return "tenant_id";
    }

    /**
     * 根据表名判断是否忽略拼接多租户条件
     * 默认都要进行解析并拼接多租户条件
     * @param tableName 表名
     * @return 是否忽略, true:表示忽略，false:需要解析并拼接多租户条件
     */
    default boolean ignoreTable(String tableName) {
        return false;
    }

    /**
     * 忽略插入租户字段逻辑
     * @param columns 插入字段
     * @param tenantIdColumn 租户 ID 字段
     * @return
     */
    default boolean ignoreInsert(List<Column> columns, String tenantIdColumn) {
        return columns.stream()
            .map(Column::getColumnName)
            .anyMatch(i -> i.equalsIgnoreCase(tenantIdColumn));
    }
}
```

### 使用方法

#### 步骤 1: 实现租户处理器

```java
import com.baomidou.mybatisplus.extension.plugins.handler.TenantLineHandler;
import com.baomidou.mybatisplus.extension.plugins.inner.TenantLineInnerInterceptor;
import net.sf.jsqlparser.expression.Expression;
import net.sf.jsqlparser.expression.LongValue;
import org.springframework.stereotype.Component;

/**
 * 自定义租户处理器
 */
@Component
public class CustomTenantHandler implements TenantLineHandler {

    /**
     * 获取当前租户的 ID
     */
    @Override
    public Expression getTenantId() {
        // 假设有一个租户上下文，能够从中获取当前用户的租户
        Long tenantId = TenantContextHolder.getCurrentTenantId();

        // 返回租户 ID 的表达式，LongValue 是 JSQLParser 中表示 bigint 类型的 class
        return new LongValue(tenantId);
    }

    /**
     * 获取租户字段名
     */
    @Override
    public String getTenantIdColumn() {
        return "tenant_id";
    }

    /**
     * 判断是否忽略该表
     * 某些系统表可能不需要租户隔离
     */
    @Override
    public boolean ignoreTable(String tableName) {
        // 忽略系统表
        if ("sys_config".equalsIgnoreCase(tableName)) {
            return true;
        }

        // 忽略租户表本身
        if ("tenant".equalsIgnoreCase(tableName)) {
            return true;
        }

        return false;
    }
}
```

#### 步骤 2: 将租户处理器注入插件

```java
@Configuration
@MapperScan("com.yourpackage.mapper")
public class MybatisPlusConfig {

    @Autowired
    private CustomTenantHandler customTenantHandler;

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        // 创建多租户插件
        TenantLineInnerInterceptor tenantInterceptor =
            new TenantLineInnerInterceptor();

        // 设置租户处理器
        tenantInterceptor.setTenantLineHandler(customTenantHandler);

        // 添加多租户插件
        interceptor.addInnerInterceptor(tenantInterceptor);

        // 添加分页插件（注意顺序）
        interceptor.addInnerInterceptor(
            new PaginationInnerInterceptor(DbType.MYSQL)
        );

        return interceptor;
    }
}
```

### 本地缓存 SQL 解析

为了提高性能，MyBatis-Plus 支持本地缓存 SQL 解析：

```java
@Configuration
public class TenantConfig {

    static {
        // 默认支持序列化 FstSerialCaffeineJsqlParseCache，JdkSerialCaffeineJsqlParseCache
        JsqlParserGlobal.setJsqlParseCache(
            new JdkSerialCaffeineJsqlParseCache(
                (cache) -> cache
                    .maximumSize(1024)  // 最大缓存 1024 条
                    .expireAfterWrite(5, TimeUnit.SECONDS)  // 写入后 5 秒过期
            )
        );
    }
}
```

### 插入时自动添加租户字段

默认插入 SQL 需要判断租户条件，因此需要配合自动填充字段功能填充租户字段：

```java
@Component
public class MyMetaObjectHandler implements MetaObjectHandler {

    @Override
    public void insertFill(MetaObject metaObject) {
        // 自动填充租户 ID
        this.strictInsertFill(metaObject, "tenantId", Long.class,
            TenantContextHolder.getCurrentTenantId());
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        // 更新时不修改租户 ID
    }
}
```

### 使用示例

#### 基础使用

```java
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 查询用户 - 自动添加租户条件
     * SQL: SELECT * FROM user WHERE tenant_id = ? AND status = 'active'
     */
    public List<User> listActiveUsers() {
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(User::getStatus, "active");
        return userMapper.selectList(wrapper);
    }

    /**
     * 插入用户 - 自动填充租户 ID
     * SQL: INSERT INTO user (name, email, tenant_id) VALUES (?, ?, ?)
     */
    public void createUser(User user) {
        userMapper.insert(user);  // tenant_id 会自动填充
    }
}
```

#### 忽略租户拦截

```java
@Service
public class AdminService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 忽略租户插件，查询所有租户的用户
     * 需要管理员权限
     */
    @InterceptorIgnore(tenantLine = "true")
    public List<User> listAllUsers() {
        return userMapper.selectList(null);
    }
}
```

### 注意事项

1. **租户 ID 获取**: 实际应用中，获取租户 ID 的方式可能会根据应用架构和业务需求有所不同
2. **安全性**: 确保在处理租户 ID 时考虑到安全性，避免潜在的安全风险
3. **性能**: 使用本地缓存可以提高性能，但要注意缓存的更新策略
4. **自动填充**: 插入时需要配合自动填充字段功能填充租户字段

---

## 自动维护 DDL

**URL:** https://baomidou.com/guides/auto-ddl/

### 功能概述

在 MyBatis-Plus 的 **3.5.3+** 版本中，引入了一项强大的功能：数据库 DDL（数据定义语言）表结构的自动维护。这一功能通过执行 SQL 脚本来实现数据库模式的初始化和升级。

### 功能特性

1. **自动化执行**: 应用启动时自动执行 DDL 脚本
2. **分库分表支持**: 支持分表库的场景
3. **存储过程支持**: 从 3.5.3.2 版本开始支持执行存储过程
4. **自定义控制**: 可以自定义执行逻辑和错误处理
5. **事务管理**: 支持自动提交事务或手动控制

### 代码示例

#### 定义 DDL 组件

```java
import com.baomidou.mybatisplus.extension.ddl.IDdl;
import org.springframework.stereotype.Component;

import java.util.Arrays;
import java.util.List;

/**
 * MySQL DDL 组件
 */
@Component
public class MysqlDdl implements IDdl {

    /**
     * 获取要执行的 SQL 脚本文件列表
     */
    @Override
    public List<String> getSqlFiles() {
        return Arrays.asList(
            // 基础表结构
            "db/tag-schema.sql",

            // 从 3.5.3.2 版本开始，支持执行存储过程
            // 在文件名后追加 `#$$`，其中 `$$` 是自定义的完整 SQL 分隔符
            // 存储过程脚本以 `END` 结尾，并追加分隔符 `END;$$` 表示脚本结束
            "db/procedure.sql#$$",

            // 初始化数据
            "db/tag-data.sql",

            // 支持绝对路径
            "D:\\db\\tag-data.sql"
        );
    }
}
```

**存储过程脚本示例 (procedure.sql)**:

```sql
DELIMITER $$

CREATE PROCEDURE GetUserCount(IN userId INT)
BEGIN
    SELECT COUNT(*) FROM user WHERE id = userId;
END$$

DELIMITER ;
```

#### 手动执行 SQL 脚本

```java
@Service
public class DdlService {

    @Autowired
    private IDdl ddlScript;

    /**
     * 手动执行 SQL 脚本
     */
    public void executeManualSql() {
        // 切换到 mysql 从库（开源版本无此功能）
        ShardingKey.change("mysqlt2");

        // 执行 SQL 脚本
        ddlScript.run(new StringReader(
            "DELETE FROM user;\n" +
            "INSERT INTO user (id, username, password, sex, email) VALUES\n" +
            "(20, 'Duo', '123456', 0, 'Duo@baomidou.com');"
        ));
    }
}
```

### 自定义运行器

如果集成了 MyBatis-Plus 的 starter，会自动实例化一个 `DdlApplicationRunner` 实例来执行 DDL 脚本。

执行方式为：
- 自动提交事务
- 忽略错误继续执行

如果需要自定义控制，请自行注入一个 `DdlApplicationRunner` 实例至容器：

```java
@Configuration
public class DdlRunnerConfig {

    @Bean
    public DdlApplicationRunner ddlApplicationRunner(List<IDdl> ddlList) {
        DdlApplicationRunner ddlApplicationRunner = new DdlApplicationRunner(ddlList);

        // 下面属性自 3.5.11 开始支持

        // 设置是否自动提交 默认: true
        ddlApplicationRunner.setAutoCommit(false);

        // 设置脚本遇到错误的处理方式
        // 默认: 忽略错误,打印异常
        // 如果设置为抛出异常，会终止下一个 SQL 文件处理
        ddlApplicationRunner.setDdlScriptErrorHandler(
            DdlScriptErrorHandler.ThrowsErrorHandler.INSTANCE
        );

        // 是否抛出异常中断下个处理器处理 默认: false
        ddlApplicationRunner.setThrowException(true);

        // 自定义 ScriptRunner 配置
        ddlApplicationRunner.setScriptRunnerConsumer(scriptRunner -> {
            scriptRunner.setLogWriter(null);     // 关闭执行日志打印 默认: System.out
            scriptRunner.setErrorLogWriter(null); // 关闭错误日志打印 默认: System.err
            scriptRunner.setStopOnError(true);    // 遇到异常是否停止 默认: false
            scriptRunner.setRemoveCRs(false);     // 是否替换 \r\n 为 \n 默认: false
        });

        return ddlApplicationRunner;
    }
}
```

### 最佳实践

#### 1. 脚本组织

```
src/main/resources/
├── db/
│   ├── schema/              # 表结构脚本
│   │   ├── 01-create-user.sql
│   │   ├── 02-create-order.sql
│   │   └── 99-create-index.sql
│   ├── data/                # 数据脚本
│   │   ├── 01-init-user.sql
│   │   └── 02-init-order.sql
│   └── procedure/           # 存储过程脚本
│       └── 01-user-stats.sql#$$
```

#### 2. 版本控制

```java
@Component
public class VersionControlDdl implements IDdl {

    @Override
    public List<String> getSqlFiles() {
        String dbVersion = getDbVersion();

        return Arrays.asList(
            "db/schema/" + dbVersion + "/01-schema.sql",
            "db/data/" + dbVersion + "/01-data.sql"
        );
    }

    private String getDbVersion() {
        // 根据配置或环境决定数据库版本
        return "v1.0";
    }
}
```

---

## 更新日志

### v3.5.15 (2025.11.30)

- 升级 JSQLParser 到 4.9
- 优化分页插件性能
- 修复已知问题

### v3.5.14 (2025.08.29)

- 新增功能优化
- Bug 修复

### v3.5.13 (2025.08.29)

- 性能优化
- 新增特性

### v3.5.12 (2025.04.27)

- 功能增强
- 问题修复

### v3.5.11 (2025.03.23)

- 新增 DdlApplicationRunner 自定义配置支持
- 性能优化

### v3.5.10.1 (2025.01.13)

- Bug 修复

### v3.5.10 (2025.01.12)

- 支持系统属性与环境变量传递密钥
- 新增字段脱敏功能

### v3.5.9 (2024.10.23)

**重要变更**: 插件部分改为可选依赖

- 需要单独引入 `mybatis-plus-jsqlparser` 依赖
- 支持 JSQLParser 4.9 和 5.x 版本

### v3.5.8 (2024.09.18)

- Wrapper 查询支持 TypeHandler
- 优化连续换行语句处理

---

## 常见问题

### Q1: 如何选择多数据源方案？

**A**:
- **开源项目/成本敏感**: 选择 `dynamic-datasource`
- **企业级应用/需要高级特性**: 选择 `mybatis-mate`
- **特殊需求**: 可以结合两者使用

### Q2: 批量操作有什么性能建议？

**A**:
- 控制批量大小，建议 500-1000 条/批
- 使用事务确保数据一致性
- 大数据量导入考虑使用 `LOAD DATA INFILE`

### Q3: 如何防止 SQL 注入？

**A**:
- 使用 `LambdaQueryWrapper` 而不是 `QueryWrapper`
- 开启 SQL 注入检查：`.checkSqlInjection()`
- 使用白名单机制验证字段名
- 避免前端直接传入 SQL 片段

### Q4: 多租户插件会影响性能吗？

**A**:
- 会有一定性能开销，因为需要修改 SQL
- 使用本地缓存可以优化性能
- 对于不需要租户隔离的表，使用 `ignoreTable` 忽略

### Q5: 如何在开发环境禁用某些插件？

**A**: 使用 Profile 配置：

```java
@Profile("prod")
@Bean
public BlockAttackInnerInterceptor blockAttackInterceptor() {
    return new BlockAttackInnerInterceptor();
}
```

---

## 最佳实践总结

### 1. 数据源管理

- 使用连接池监控
- 定期检查连接泄漏
- 配置合理的连接池参数

### 2. 批量操作

- 根据操作类型选择合适的批量大小
- 使用事务确保数据一致性
- 注意内存使用，避免 OOM

### 3. 安全防护

- 生产环境启用防全表更新删除插件
- 配置文件使用加密
- 敏感字段加密存储
- 使用 SQL 注入检查

### 4. 多租户设计

- 合理设计租户隔离策略
- 使用本地缓存优化性能
- 注意租户 ID 的传递和获取

### 5. 插件配置

- 按照正确的顺序配置插件
- 根据业务需求选择性启用插件
- 定期审查插件配置的必要性

---

## 参考资源

### 官方资源

- **官方网站**: https://baomidou.com
- **GitHub**: https://github.com/baomidou/mybatis-plus
- **Gitee**: https://gitee.com/baomidou/mybatis-plus
- **API 文档**: https://baomidou.com/api/

### 开源生态

- **dynamic-datasource**: 多数据源支持
- **Awesome-MyBatis-Plus**: 生态资源汇总

### 企业级生态

- **MyBatis-Mate**: 企业级高级特性（付费）
- **PIG**: 微服务开发平台
- **Guns**: Java 应用开发框架
- **Snowy**: 国密前后分离快速开发平台

---

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097
