# Mybatis-Plus - Pagination 分页功能

**Pages:** 1

---

## 流式查询 | MyBatis-Plus

**URL:** https://baomidou.com/guides/stream-query/

### 功能概述

MyBatis-Plus 从 **3.5.4 版本**开始支持流式查询，这是 MyBatis 的原生功能，通过 `ResultHandler` 接口实现结果集的流式查询。这种查询方式特别适用于以下场景：

- **数据跑批**：处理大量数据，如数据同步、数据迁移
- **大数据处理**：避免一次性加载大量数据到内存
- **实时处理**：边查询边处理，提高响应速度
- **内存优化**：减少内存占用，防止 OOM

### 版本兼容性

- **支持版本**: MyBatis-Plus 3.5.4+
- **依赖要求**: 无额外依赖，使用 MyBatis 原生功能

### 核心优势

1. **内存友好**: 不会一次性加载所有数据到内存
2. **实时处理**: 可以边查询边处理数据
3. **灵活性高**: 可以结合分页使用
4. **性能优秀**: 适合大数据量处理场景

### 常用方法

在 `BaseMapper` 中，新增了多个重载方法，支持流式查询：

```java
// 查询列表（流式）
void selectList(Wrapper<T> queryWrapper, ResultHandler<T> resultHandler);

// 查询列表（带分页，流式）
void selectList(IPage<T> page, Wrapper<T> queryWrapper, ResultHandler<T> resultHandler);

// 根据 Map 查询（流式）
void selectByMap(Map<String, Object> map, ResultHandler<T> resultHandler);

// 根据 ID 批量查询（流式）
void selectBatchIds(Collection<? extends Serializable> idList, ResultHandler<T> resultHandler);

// 查询 Map 列表（流式）
void selectMaps(Wrapper<T> queryWrapper, ResultHandler<Map<String, Object>> resultHandler);

// 查询对象列表（流式）
void selectObjs(Wrapper<T> queryWrapper, ResultHandler<Object> resultHandler);
```

### 使用步骤

#### 1. 定义 ResultHandler

`ResultHandler` 是 MyBatis 提供的接口，用于处理查询结果的每一行数据：

```java
import org.apache.ibatis.session.ResultContext;
import org.apache.ibatis.session.ResultHandler;

public abstract class StreamingResultHandler<T> implements ResultHandler<T> {

    private int count = 0;
    private long startTime = System.currentTimeMillis();

    @Override
    public void handleResult(ResultContext<? extends T> resultContext) {
        T result = resultContext.getResultObject();
        count++;

        // 处理当前行数据
        processResult(result, count);

        // 可选：定期输出进度
        if (count % 1000 == 0) {
            long elapsed = System.currentTimeMillis() - startTime;
            System.out.printf("已处理 %d 条记录，耗时 %d ms%n", count, elapsed);
        }
    }

    /**
     * 处理单条记录，由子类实现
     */
    protected abstract void processResult(T result, int count);

    /**
     * 获取处理的总记录数
     */
    public int getCount() {
        return count;
    }

    /**
     * 获取总耗时（毫秒）
     */
    public long getElapsedTime() {
        return System.currentTimeMillis() - startTime;
    }
}
```

#### 2. 实现具体的业务处理逻辑

```java
import com.example.mybatisplus.entity.User;
import org.springframework.stereotype.Component;

@Component
public class UserStreamingProcessor extends StreamingResultHandler<User> {

    @Override
    protected void processResult(User user, int count) {
        // 示例1：数据导出
        exportToCsv(user);

        // 示例2：数据同步
        syncToOtherSystem(user);

        // 示例3：数据清洗
        cleanUserData(user);
    }

    /**
     * 导出为 CSV
     */
    private void exportToCsv(User user) {
        // 实现导出逻辑
        System.out.printf("导出用户：%s%n", user.getName());
    }

    /**
     * 同步到其他系统
     */
    private void syncToOtherSystem(User user) {
        // 实现同步逻辑
    }

    /**
     * 清洗数据
     */
    private void cleanUserData(User user) {
        // 实现数据清洗逻辑
    }
}
```

### 使用示例

#### 示例 1：基础流式查询

查询所有用户并逐条处理：

```java
import com.baomidou.mybatisplus.core.toolkit.Wrappers;
import org.apache.ibatis.session.ResultContext;
import org.apache.ibatis.session.ResultHandler;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 流式查询所有用户
     */
    public void processAllUsers() {
        System.out.println("开始流式查询所有用户...");

        userMapper.selectList(
            Wrappers.emptyWrapper(),  // 无查询条件
            new ResultHandler<User>() {
                private int count = 0;

                @Override
                public void handleResult(ResultContext<? extends User> resultContext) {
                    User user = resultContext.getResultObject();
                    count++;

                    // 处理当前用户
                    System.out.printf("处理第 %d 条记录：%s%n", count, user.getName());

                    // 业务逻辑：数据处理、导出等
                    processUser(user);
                }
            }
        );

        System.out.println("流式查询完成！");
    }

    private void processUser(User user) {
        // 具体的业务处理逻辑
    }
}
```

#### 示例 2：结合分页的流式查询

分批从数据库拉取数据进行处理：

```java
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import org.apache.ibatis.session.ResultContext;
import org.apache.ibatis.session.ResultHandler;

@Service
public class BatchProcessingService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 分批流式查询处理
     * 适用于大数据量场景，如从数据库获取 10 万记录进行处理
     */
    public void processUsersInBatches() {
        int pageSize = 10000;  // 每批 1 万条
        int currentPage = 1;

        while (true) {
            // 创建分页对象
            Page<User> page = new Page<>(currentPage, pageSize);

            // 使用 count 数组来跟踪是否还有数据
            final int[] count = {0};

            userMapper.selectList(
                page,
                Wrappers.emptyWrapper(),
                new ResultHandler<User>() {
                    @Override
                    public void handleResult(ResultContext<? extends User> resultContext) {
                        User user = resultContext.getResultObject();
                        count[0]++;

                        // 处理当前记录
                        processUser(user);
                    }
                }
            );

            // 如果本批次没有数据，退出循环
            if (count[0] == 0) {
                break;
            }

            System.out.printf("第 %d 批处理完成，共 %d 条记录%n", currentPage, count[0]);
            currentPage++;
        }
    }
}
```

#### 示例 3：条件流式查询

带查询条件的流式处理：

```java
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import org.apache.ibatis.session.ResultContext;
import org.apache.ibatis.session.ResultHandler;

@Service
public class ConditionalStreamingService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 流式查询并处理特定条件的用户
     */
    public void processActiveUsers() {
        // 创建查询条件
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.eq("status", "active")
                   .ge("age", 18)
                   .orderByAsc("create_time");

        userMapper.selectList(
            queryWrapper,
            new ResultHandler<User>() {
                private int count = 0;
                private int activeCount = 0;

                @Override
                public void handleResult(ResultContext<? extends User> resultContext) {
                    User user = resultContext.getResultObject();
                    count++;

                    if (user.getStatus().equals("active")) {
                        activeCount++;
                        // 处理活跃用户
                        sendNotification(user);
                    }

                    // 每 100 条输出一次进度
                    if (count % 100 == 0) {
                        System.out.printf("已处理 %d 条记录，其中活跃用户 %d 条%n",
                                          count, activeCount);
                    }
                }
            }
        );
    }

    private void sendNotification(User user) {
        // 发送通知逻辑
    }
}
```

#### 示例 4：流式查询 Map 结果

```java
import org.apache.ibatis.session.ResultContext;
import org.apache.ibatis.session.ResultHandler;

@Service
public class MapStreamingService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 流式查询 Map 结果
     */
    public void processUserMaps() {
        userMapper.selectMaps(
            Wrappers.<User>query().orderByDesc("create_time"),
            new ResultHandler<Map<String, Object>>() {
                @Override
                public void handleResult(ResultContext<? extends Map<String, Object>> resultContext) {
                    Map<String, Object> userMap = resultContext.getResultObject();

                    // 处理 Map 数据
                    Long id = (Long) userMap.get("id");
                    String name = (String) userMap.get("name");

                    // 业务逻辑
                    analyzeUserData(id, name);
                }
            }
        );
    }

    private void analyzeUserData(Long id, String name) {
        // 数据分析逻辑
    }
}
```

### 最佳实践

#### 1. 内存管理

虽然是流式查询，但仍需注意内存管理：

```java
// ✅ 推荐：每批处理完成后清理缓存
userMapper.selectList(page, wrapper, new ResultHandler<User>() {
    private List<User> batch = new ArrayList<>(BATCH_SIZE);

    @Override
    public void handleResult(ResultContext<? extends User> resultContext) {
        User user = resultContext.getResultObject();
        batch.add(user);

        if (batch.size() >= BATCH_SIZE) {
            // 处理当前批次
            processBatch(batch);
            // 清空批次，释放内存
            batch.clear();
        }
    }
});

// ❌ 避免：累积大量数据
List<User> allUsers = new ArrayList<>();
userMapper.selectList(wrapper, new ResultHandler<User>() {
    @Override
    public void handleResult(ResultContext<? extends User> resultContext) {
        allUsers.add(resultContext.getResultObject());  // 可能导致 OOM
    }
});
```

#### 2. 异常处理

流式查询中的异常处理：

```java
userMapper.selectList(wrapper, new ResultHandler<User>() {
    private int successCount = 0;
    private int errorCount = 0;
    private List<Long> errorIds = new ArrayList<>();

    @Override
    public void handleResult(ResultContext<? extends User> resultContext) {
        User user = resultContext.getResultObject();

        try {
            // 处理业务逻辑
            processUser(user);
            successCount++;
        } catch (Exception e) {
            errorCount++;
            errorIds.add(user.getId());
            log.error("处理用户 {} 失败", user.getId(), e);

            // 可选：停止处理
            // resultContext.stop();
        }
    }

    @Override
    public void end() {
        // 处理完成后输出统计信息
        System.out.printf("处理完成：成功 %d 条，失败 %d 条%n",
                         successCount, errorCount);
        if (!errorIds.isEmpty()) {
            System.out.println("失败的ID: " + errorIds);
        }
    }
});
```

#### 3. 进度监控

实现进度监控和报告：

```java
import org.apache.ibatis.session.ResultContext;

public class ProgressMonitoringResultHandler<T> implements ResultHandler<T> {

    private final int totalExpected;  // 预期总数
    private final long reportInterval;  // 报告间隔（毫秒）

    private int processedCount = 0;
    private long lastReportTime = System.currentTimeMillis();

    public ProgressMonitoringResultHandler(int totalExpected, long reportInterval) {
        this.totalExpected = totalExpected;
        this.reportInterval = reportInterval;
    }

    @Override
    public void handleResult(ResultContext<? extends T> resultContext) {
        T result = resultContext.getResultObject();
        processedCount++;

        // 处理当前记录
        processResult(result);

        // 定期报告进度
        long currentTime = System.currentTimeMillis();
        if (currentTime - lastReportTime >= reportInterval) {
            reportProgress();
            lastReportTime = currentTime;
        }
    }

    private void processResult(T result) {
        // 具体处理逻辑
    }

    private void reportProgress() {
        double percentage = totalExpected > 0
            ? (processedCount * 100.0 / totalExpected)
            : 0;

        System.out.printf("进度：%d/%d (%.2f%%)%n",
                         processedCount, totalExpected, percentage);
    }
}
```

#### 4. 事务管理

流式查询中的事务处理：

```java
@Service
@Transactional
public class StreamingTransactionService {

    @Autowired
    private UserMapper userMapper;

    @Autowired
    private OrderMapper orderMapper;

    /**
     * 流式查询并更新，使用事务保证数据一致性
     */
    public void processUsersWithTransaction() {
        userMapper.selectList(
            Wrappers.<User>query().eq("status", "pending"),
            new ResultHandler<User>() {
                @Override
                public void handleResult(ResultContext<? extends User> resultContext) {
                    User user = resultContext.getResultObject();

                    try {
                        // 处理用户
                        updateUser(user);

                        // 创建订单
                        createOrder(user);

                        // 标记为已处理
                        markAsProcessed(user);

                    } catch (Exception e) {
                        log.error("处理用户 {} 失败，事务将回滚", user.getId(), e);
                        throw e;  // 抛出异常，触发事务回滚
                    }
                }
            }
        );
    }

    private void updateUser(User user) {
        userMapper.updateById(user);
    }

    private void createOrder(User user) {
        // 创建订单逻辑
    }

    private void markAsProcessed(User user) {
        user.setStatus("processed");
        userMapper.updateById(user);
    }
}
```

### 注意事项

#### 1. 版本兼容性

**重要提示**：在低版本的 MyBatis-Plus 中，如果自定义 ResultHandler 结合分页查询，可能会出现错误。在这种情况下，需要手动关闭 count 查询。

```java
// 关闭 count 查询
Page<User> page = new Page<>(1, 1000);
page.setSearchCount(false);  // 不执行 count 查询
page.setOptimizeCountSql(false);  // 不优化 count SQL
```

#### 2. 连接管理

流式查询会长时间占用数据库连接，需要注意：

```java
// ✅ 推荐：及时关闭资源
try (SqlSession session = sqlSessionFactory.openSession()) {
    UserMapper mapper = session.getMapper(UserMapper.class);
    mapper.selectList(wrapper, resultHandler);
} catch (Exception e) {
    log.error("流式查询失败", e);
}

// ❌ 避免：长时间占用连接
userMapper.selectList(wrapper, new ResultHandler<User>() {
    @Override
    public void handleResult(ResultContext<? extends User> resultContext) {
        User user = resultContext.getResultObject();
        // 避免在这里进行耗时的操作
        heavyProcessing(user);  // 可能导致连接超时
    }
});
```

#### 3. 分页配置

结合分页使用时的注意事项：

```java
// 正确的分页配置
Page<User> page = new Page<>(1, 1000);
page.setSearchCount(true);  // 执行 count 查询获取总数
page.setHitCount(false);  // 不命中缓存

// 流式查询
userMapper.selectList(page, wrapper, new ResultHandler<User>() {
    @Override
    public void handleResult(ResultContext<? extends User> resultContext) {
        User user = resultContext.getResultObject();
        processUser(user);
    }
});
```

### 性能对比

#### 流式查询 vs 传统查询

| 特性 | 传统查询 | 流式查询 |
|------|---------|---------|
| 内存占用 | 高（一次性加载所有数据） | 低（逐条处理） |
| 响应时间 | 慢（等待所有数据加载完成） | 快（立即开始处理） |
| 适用场景 | 小数据量 | 大数据量 |
| OOM 风险 | 高 | 低 |
| 实现复杂度 | 简单 | 中等 |

### 常见问题

**Q1: 流式查询的性能如何？**

A: 流式查询在处理大数据量时性能更好：
- 内存占用低，避免频繁 GC
- 可以边查询边处理，提高响应速度
- 适合数据跑批、实时处理场景

**Q2: 如何在流式查询中使用事务？**

A: 在 Service 方法上添加 `@Transactional` 注解：
```java
@Transactional(rollbackFor = Exception.class)
public void processWithTransaction() {
    userMapper.selectList(wrapper, resultHandler);
}
```

**Q3: 流式查询可以中断吗？**

A: 可以使用 `resultContext.stop()` 中断查询：
```java
userMapper.selectList(wrapper, new ResultHandler<User>() {
    @Override
    public void handleResult(ResultContext<? extends User> resultContext) {
        User user = resultContext.getResultObject();
        if (shouldStop(user)) {
            resultContext.stop();  // 中断查询
        }
    }
});
```

**Q4: 如何获取处理的总记录数？**

A: 在 ResultHandler 中维护计数器：
```java
userMapper.selectList(wrapper, new ResultHandler<User>() {
    private int count = 0;

    @Override
    public void handleResult(ResultContext<? extends User> resultContext) {
        count++;
        // 处理逻辑...
    }

    @Override
    public void end() {
        System.out.println("总处理记录数：" + count);
    }
});
```

---

## 分页插件配置

### 配置分页拦截器

```java
@Configuration
public class MybatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        // 添加分页插件
        PaginationInnerInterceptor paginationInterceptor = new PaginationInnerInterceptor(
            DbType.MYSQL  // 指定数据库类型
        );

        // 设置单页最大限制数量
        paginationInterceptor.setMaxLimit(500L);

        // 设置溢出总页数后是否进行处理
        paginationInterceptor.setOverflow(false);

        interceptor.addInnerInterceptor(paginationInterceptor);

        return interceptor;
    }
}
```

### 使用分页查询

```java
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 分页查询用户
     */
    public IPage<User> selectUserPage(int current, int size) {
        // 创建分页对象
        Page<User> page = new Page<>(current, size);

        // 执行分页查询
        IPage<User> userPage = userMapper.selectPage(page, null);

        // 返回分页结果
        return userPage;
    }

    /**
     * 带条件的分页查询
     */
    public IPage<User> selectUserPageWithCondition(
        int current, int size, String keyword) {

        Page<User> page = new Page<>(current, size);

        LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.like(User::getName, keyword)
                   .orderByDesc(User::getCreateTime);

        return userMapper.selectPage(page, queryWrapper);
    }
}
```

---

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097
