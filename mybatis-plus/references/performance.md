# Mybatis-Plus - Performance 性能优化

**Pages:** 1

---

## 非法 SQL 拦截插件 | MyBatis-Plus

**URL:** https://baomidou.com/plugins/illegal-sql-intercept/

### 功能概述

IllegalSQLInnerInterceptor 是 MyBatis-Plus 框架中的一个安全控制插件，用于拦截和检查非法 SQL 语句。该插件旨在帮助开发者在 SQL 执行前发现并解决潜在的安全问题，如：

- **全表更新**：防止不带 WHERE 条件的 UPDATE 操作
- **全表删除**：防止不带 WHERE 条件的 DELETE 操作
- **性能问题 SQL**：识别可能导致性能问题的查询
- **索引检查**：检查 SQL 是否使用了索引

### 功能特性

1. **自动拦截**：自动检测并拦截非法 SQL
2. **可配置**：支持自定义拦截规则
3. **性能监控**：记录 SQL 执行信息
4. **安全加固**：防止误操作导致的数据损失

### 应用场景

- **生产环境保护**：防止误操作导致的全表更新/删除
- **代码审查**：在开发阶段发现潜在的 SQL 问题
- **性能优化**：识别慢查询和未使用索引的查询
- **安全审计**：记录所有 SQL 操作，便于审计

### 使用方法

#### Spring Boot 配置

```java
import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.BlockAttackInnerInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.IllegalSQLInnerInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.OptimisticLockerInnerInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MybatisPlusConfig {

    /**
     * 配置 MyBatis-Plus 拦截器
     */
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        // 1. 添加非法 SQL 拦截器
        IllegalSQLInnerInterceptor illegalSQLInterceptor = new IllegalSQLInnerInterceptor();
        interceptor.addInnerInterceptor(illegalSQLInterceptor);

        // 2. 添加防全表更新删除拦截器
        BlockAttackInnerInterceptor blockAttackInterceptor = new BlockAttackInnerInterceptor();
        interceptor.addInnerInterceptor(blockAttackInterceptor);

        // 3. 添加分页插件
        PaginationInnerInterceptor paginationInterceptor = new PaginationInnerInterceptor(DbType.MYSQL);
        interceptor.addInnerInterceptor(paginationInterceptor);

        return interceptor;
    }
}
```

#### Spring XML 配置

```xml
<bean id="mybatisPlusInterceptor" class="com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor">
    <property name="interceptors">
        <list>
            <!-- 非法 SQL 拦截器 -->
            <bean class="com.baomidou.mybatisplus.extension.plugins.inner.IllegalSQLInnerInterceptor"/>

            <!-- 防全表更新删除拦截器 -->
            <bean class="com.baomidou.mybatisplus.extension.plugins.inner.BlockAttackInnerInterceptor"/>

            <!-- 分页插件 -->
            <bean class="com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor">
                <constructor-arg name="dbType" value="MYSQL"/>
            </bean>
        </list>
    </property>
</bean>
```

### 自定义拦截规则

```java
import com.baomidou.mybatisplus.extension.plugins.inner.IllegalSQLInnerInterceptor;
import net.sf.jsqlparser.statement.Statement;
import net.sf.jsqlparser.statement.update.Update;
import net.sf.jsqlparser.statement.delete.Delete;

/**
 * 自定义非法 SQL 拦截器
 */
public class CustomIllegalSQLInterceptor extends IllegalSQLInnerInterceptor {

    @Override
    public void beforeQuery(Executor executor, MappedStatement ms, Object parameter,
                            RowBounds rowBounds, ResultHandler resultHandler,
                            BoundSql boundSql) throws SQLException {
        // 在查询前检查 SQL
        Statement statement = parseSQL(boundSql.getSql());
        checkStatement(statement);
    }

    @Override
    public void beforeUpdate(Executor executor, MappedStatement ms, Object parameter) throws SQLException {
        // 在更新前检查 SQL
        // 可以在这里添加自定义的检查逻辑
    }

    @Override
    public void beforeDelete(Executor executor, MappedStatement ms, Object parameter) throws SQLException {
        // 在删除前检查 SQL
        // 可以在这里添加自定义的检查逻辑
    }

    /**
     * 检查 SQL 语句是否合法
     */
    private void checkStatement(Statement statement) {
        if (statement instanceof Update) {
            Update update = (Update) statement;
            if (update.getWhere() == null) {
                throw new RuntimeException("不允许执行不带 WHERE 条件的 UPDATE 操作");
            }
        } else if (statement instanceof Delete) {
            Delete delete = (Delete) statement;
            if (delete.getWhere() == null) {
                throw new RuntimeException("不允许执行不带 WHERE 条件的 DELETE 操作");
            }
        }
    }

    /**
     * 解析 SQL 语句
     */
    private Statement parseSQL(String sql) {
        try {
            return CCJSqlParserUtil.parse(sql);
        } catch (JSQLParserException e) {
            throw new RuntimeException("SQL 解析失败：" + sql, e);
        }
    }
}
```

### 性能优化建议

#### 1. 索引优化

确保查询字段有合适的索引：

```sql
-- 创建索引
CREATE INDEX idx_user_name ON user(name);
CREATE INDEX idx_user_age ON user(age);
CREATE INDEX idx_user_status ON user(status);

-- 创建复合索引
CREATE INDEX idx_user_status_age ON user(status, age);
```

#### 2. 批量操作优化

使用批量操作提高性能：

```java
// ✅ 推荐：使用批量插入
userMapper.insertBatchSomeColumn(users);

// ✅ 推荐：使用批量更新
userMapper.updateBatchById(users);

// ❌ 避免：循环单条操作
for (User user : users) {
    userMapper.insert(user);  // 性能差
}
```

#### 3. 缓存策略

合理使用 MyBatis 的缓存机制：

```yaml
mybatis-plus:
  configuration:
    # 开启二级缓存
    cache-enabled: true
    # 延迟加载
    lazy-loading-enabled: true
    agressive-lazy-loading: false
```

#### 4. 分页优化

对于大数据量查询，使用分页：

```java
// ✅ 推荐：使用分页
Page<User> page = new Page<>(1, 100);
IPage<User> userPage = userMapper.selectPage(page, wrapper);

// ❌ 避免：一次性加载大量数据
List<User> users = userMapper.selectList(wrapper);  // 可能导致 OOM
```

#### 5. SQL 优化

避免常见的 SQL 性能问题：

```java
// ❌ 避免：SELECT *
userMapper.selectList(wrapper);  // 查询所有字段

// ✅ 推荐：只查询需要的字段
wrapper.select(User::getId, User::getName, User::getEmail);

// ❌ 避免：在 WHERE 中使用函数
wrapper.apply("DATE_FORMAT(create_time, '%Y-%m-%d') = '2024-01-01'");

// ✅ 推荐：使用范围查询
wrapper.between(User::getCreateTime, startDate, endDate);

// ❌ 避免：LIKE 前缀模糊查询
wrapper.like(User::getName, "%keyword");  // 无法使用索引

// ✅ 推荐：使用后缀模糊查询
wrapper.likeRight(User::getName, "keyword");  // 可以使用索引
```

### 监控和诊断

#### 慢 SQL 监控

使用 p6spy 监控慢 SQL：

```yaml
spring:
  datasource:
    driver-class-name: com.p6spy.engine.spy.P6SpyDriver
    url: jdbc:p6spy:mysql://localhost:3306/mydb
```

#### SQL 性能分析

```java
@Service
public class SQLPerformanceAnalyzer {

    @Autowired
    private UserMapper userMapper;

    /**
     * 分析 SQL 性能
     */
    public void analyzeSQLPerformance() {
        long startTime = System.currentTimeMillis();

        List<User> users = userMapper.selectList(wrapper);

        long endTime = System.currentTimeMillis();
        long duration = endTime - startTime;

        if (duration > 1000) {  // 超过 1 秒
            log.warn("慢 SQL 检测！执行时间：{} ms", duration);
            log.warn("SQL：{}", wrapper.getCustomSqlSegment());
        }
    }
}
```

### 性能测试

#### 压力测试

使用 JMeter 或 Gatling 进行压力测试：

```java
@RestController
@RequestMapping("/api/user")
public class UserController {

    @Autowired
    private UserService userService;

    /**
     * 测试接口性能
     */
    @GetMapping("/list")
    public Result<List<User>> list() {
        long startTime = System.currentTimeMillis();

        List<User> users = userService.list();

        long duration = System.currentTimeMillis() - startTime;
        if (duration > 500) {
            log.warn("接口响应慢：{} ms", duration);
        }

        return Result.success(users);
    }
}
```

#### 性能指标

监控关键性能指标：

- **响应时间**：接口响应时间应 < 200ms
- **吞吐量**：QPS 应满足业务需求
- **错误率**：错误率应 < 0.1%
- **CPU 使用率**：< 70%
- **内存使用率**：< 80%
- **数据库连接数**：< 最大连接数的 80%

### 最佳实践总结

1. **索引优化**
   - 为常用查询字段创建索引
   - 使用复合索引优化多条件查询
   - 定期分析慢查询日志

2. **批量操作**
   - 使用批量插入和更新
   - 控制批量大小（建议 100-1000 条/批）
   - 使用事务批量提交

3. **缓存策略**
   - 合理使用 MyBatis 二级缓存
   - 考虑使用 Redis 缓存热点数据
   - 设置合理的缓存过期时间

4. **分页查询**
   - 大数据量查询必须分页
   - 控制每页数据量（建议 50-100 条/页）
   - 使用游标分页避免深分页问题

5. **SQL 优化**
   - 避免 SELECT *
   - 避免在 WHERE 中使用函数
   - 合理使用 LIKE 查询
   - 使用 EXISTS 替代 IN（子查询数据量大时）

---

## 性能优化工具

### 1. 批量操作工具

```java
import com.baomidou.mybatisplus.extension.toolkit.SqlHelper;
import com.baomidou.mybatisplus.extension.toolkit.SqlRunner;

/**
 * 批量操作工具类
 */
public class BatchOperationUtil {

    /**
     * 批量插入（优化版）
     */
    public static void batchInsert(List<User> users) {
        int batchSize = 1000;
        int total = users.size();

        for (int i = 0; i < total; i += batchSize) {
            int end = Math.min(i + batchSize, total);
            List<User> batch = users.subList(i, end);

            // 使用批量插入
            SqlHelper.executeBatch(
                UserMapper.class,
                batch,
                (mapper, list) -> mapper.insertBatchSomeColumn(list)
            );
        }
    }
}
```

### 2. 性能监控工具

```java
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

/**
 * 性能监控切面
 */
@Aspect
@Component
public class PerformanceMonitorAspect {

    @Around("execution(* com.example.mapper.*.*(..))")
    public Object monitorPerformance(ProceedingJoinPoint joinPoint) throws Throwable {
        long startTime = System.currentTimeMillis();
        String methodName = joinPoint.getSignature().getName();

        Object result = joinPoint.proceed();

        long duration = System.currentTimeMillis() - startTime;

        if (duration > 1000) {
            log.warn("慢方法检测！方法：{}，耗时：{} ms", methodName, duration);
        }

        return result;
    }
}
```

---

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097
