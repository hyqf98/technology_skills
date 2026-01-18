# Mybatis-Plus - Advanced 高级功能

**Pages:** 1

---

## 数据权限插件 | MyBatis-Plus

**URL:** https://baomidou.com/plugins/data-permission/

**Contents:**
- 数据权限插件
- 插件原理
- 插件地址和测试用例
- 核心代码
- JSQLParser
- 使用方法
  - 第一步：实现数据权限逻辑
  - 第二步：注册数据权限拦截器

### 功能概述

DataPermissionInterceptor 是 MyBatis-Plus 提供的一个强大的插件，用于实现细粒度的数据权限控制。它通过拦截执行的 SQL 语句，并动态拼接权限相关的 SQL 片段，来实现对用户数据访问的精确控制。

### 应用场景

- **多租户数据隔离**：根据租户ID自动过滤数据
- **部门权限控制**：用户只能访问本部门及下属部门的数据
- **区域权限控制**：根据用户所属区域限制数据访问范围
- **自定义权限规则**：根据业务需求实现复杂的数据权限逻辑

### 工作原理

DataPermissionInterceptor 的工作原理与租户插件类似，它会在 SQL 执行前拦截 SQL 语句，并根据用户权限动态添加权限相关的 SQL 片段。这样，只有用户有权限访问的数据才会被查询出来。

### 技术架构

MyBatis-Plus 的数据权限功能依托于 **JSQLParser** 的解析能力。JSQLParser 是一个开源的 SQL 解析库，可方便地解析和修改 SQL 语句。

### 版本要求（3.5.15+）

从 MyBatis-Plus 3.5.9 版本开始，PaginationInnerInterceptor 已分离出来，需要单独引入 `mybatis-plus-jsqlparser` 依赖。

### 使用步骤

#### 第一步：实现数据权限处理器

```java
import com.baomidou.mybatisplus.extension.plugins.inner.DataPermissionInterceptor;
import com.baomidou.mybatisplus.extension.plugins.handler.MultiDataPermissionHandler;
import net.sf.jsqlparser.expression.Expression;
import net.sf.jsqlparser.expression.LongValue;
import net.sf.jsqlparser.expression.operators.conditional.AndExpression;
import net.sf.jsqlparser.expression.operators.relational.EqualsTo;
import net.sf.jsqlparser.schema.Column;
import org.springframework.stereotype.Component;

import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;

/**
 * 自定义数据权限处理器
 * 实现基于部门的数据权限控制
 */
@Component
public class CustomDataPermissionHandler extends MultiDataPermissionHandler {

    // 缓存 SQL 片段映射
    private static final Map<String, String> SQL_SEGMENT_MAP = new ConcurrentHashMap<>();

    static {
        // 初始化数据权限 SQL 片段
        SQL_SEGMENT_MAP.put("com.example.mapper.UserMapper.selectList", "user.department_id IN (1, 2, 3)");
        SQL_SEGMENT_MAP.put("com.example.mapper.OrderMapper.selectList", "order.create_by = #{currentUserId}");
    }

    @Override
    public Expression getSqlSegment(Table table, Expression where, String mappedStatementId) {
        try {
            // 1. 从缓存中获取预定义的 SQL 片段
            String sqlSegment = SQL_SEGMENT_MAP.get(mappedStatementId);

            if (sqlSegment != null) {
                // 使用预定义的权限规则
                return parseExpression(sqlSegment);
            }

            // 2. 动态生成权限条件
            return generateDynamicPermission(table, where);

        } catch (Exception e) {
            log.error("数据权限SQL拼接失败: {}", e.getMessage(), e);
            return null;
        }
    }

    /**
     * 动态生成数据权限条件
     */
    private Expression generateDynamicPermission(Table table, Expression where) {
        String tableName = table.getName();

        // 根据表名生成不同的权限条件
        if ("user".equalsIgnoreCase(tableName)) {
            // 用户表：基于部门权限
            EqualsTo deptEqualsTo = new EqualsTo();
            deptEqualsTo.setLeftExpression(new Column("department_id"));
            deptEqualsTo.setRightExpression(new LongValue(getCurrentUserDeptId()));

            return where != null
                ? new AndExpression(where, deptEqualsTo)
                : deptEqualsTo;

        } else if ("order".equalsIgnoreCase(tableName)) {
            // 订单表：只能查看自己创建的订单
            EqualsTo userEqualsTo = new EqualsTo();
            userEqualsTo.setLeftExpression(new Column("create_by"));
            userEqualsTo.setRightExpression(new LongValue(getCurrentUserId()));

            return where != null
                ? new AndExpression(where, userEqualsTo)
                : userEqualsTo;
        }

        return null;
    }

    /**
     * 获取当前用户部门ID
     */
    private Long getCurrentUserDeptId() {
        // 从 SecurityContext 或 ThreadLocal 获取当前用户信息
        return SecurityContextHolder.getCurrentUserId();
    }

    /**
     * 获取当前用户ID
     */
    private Long getCurrentUserId() {
        return SecurityContextHolder.getCurrentUserId();
    }

    /**
     * 解析 SQL 表达式
     */
    private Expression parseExpression(String sqlSegment) {
        try {
            return CCJSqlParserUtil.parseCondExpression(sqlSegment);
        } catch (JSQLParserException e) {
            throw new RuntimeException("解析SQL表达式失败: " + sqlSegment, e);
        }
    }
}
```

#### 第二步：注册数据权限拦截器

```java
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.DataPermissionInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MybatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(CustomDataPermissionHandler dataPermissionHandler) {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        // 创建数据权限拦截器
        DataPermissionInterceptor dataPermissionInterceptor = new DataPermissionInterceptor();

        // 设置自定义的数据权限处理器
        dataPermissionInterceptor.setDataPermissionHandler(dataPermissionHandler);

        // 添加到拦截器链（注意顺序：分页插件应放在最后）
        interceptor.addInnerInterceptor(dataPermissionInterceptor);

        return interceptor;
    }
}
```

### 高级用法示例

#### 示例1：基于角色的数据权限

```java
public class RoleBasedDataPermissionHandler extends MultiDataPermissionHandler {

    @Override
    public Expression getSqlSegment(Table table, Expression where, String mappedStatementId) {
        Long userId = getCurrentUserId();
        List<String> roles = getUserRoles(userId);

        // 管理员可以看到所有数据
        if (roles.contains("ADMIN")) {
            return where; // 不添加任何限制条件
        }

        // 部门经理可以看到本部门数据
        if (roles.contains("DEPT_MANAGER")) {
            EqualsTo deptEqualsTo = new EqualsTo();
            deptEqualsTo.setLeftExpression(new Column(table.getName() + ".department_id"));
            deptEqualsTo.setRightExpression(new LongValue(getUserDeptId(userId)));

            return where != null
                ? new AndExpression(where, deptEqualsTo)
                : deptEqualsTo;
        }

        // 普通用户只能看到自己的数据
        EqualsTo userEqualsTo = new EqualsTo();
        userEqualsTo.setLeftExpression(new Column(table.getName() + ".create_by"));
        userEqualsTo.setRightExpression(new LongValue(userId));

        return where != null
            ? new AndExpression(where, userEqualsTo)
            : userEqualsTo;
    }
}
```

#### 示例2：基于组织树的数据权限

```java
public class OrganizationDataPermissionHandler extends MultiDataPermissionHandler {

    @Override
    public Expression getSqlSegment(Table table, Expression where, String mappedStatementId) {
        Long userId = getCurrentUserId();
        List<Long> accessibleDeptIds = getAccessibleDepartmentIds(userId);

        if (accessibleDeptIds.isEmpty()) {
            // 没有权限访问任何部门数据
            return new EqualsTo(); // 返回一个永远为false的条件
        }

        // 构建IN条件：department_id IN (1, 2, 3, ...)
        InExpression inExpression = new InExpression();
        inExpression.setLeftExpression(new Column(table.getName() + ".department_id"));

        ExpressionList expressionList = new ExpressionList();
        List<Expression> expressions = accessibleDeptIds.stream()
            .map(LongValue::new)
            .collect(Collectors.toList());
        expressionList.setExpressions(expressions);
        inExpression.setRightExpression(expressionList);

        return where != null
            ? new AndExpression(where, inExpression)
            : inExpression;
    }

    /**
     * 获取用户可访问的部门ID列表（包括子部门）
     */
    private List<Long> getAccessibleDepartmentIds(Long userId) {
        // 实现组织树遍历逻辑
        Long userDeptId = getUserDeptId(userId);
        return organizationService.getAllChildDeptIds(userDeptId);
    }
}
```

### 使用 JSQLParser 修改 SQL

```java
import net.sf.jsqlparser.parser.CCJSqlParserUtil;
import net.sf.jsqlparser.statement.Statement;
import net.sf.jsqlparser.statement.select.PlainSelect;
import net.sf.jsqlparser.statement.select.Select;

public class JsqlParserExample {

    public static void main(String[] args) throws JSQLParserException {
        // 原始 SQL
        String originalSql = "SELECT * FROM user WHERE status = 'active'";

        // 解析 SQL
        Statement statement = CCJSqlParserUtil.parse(originalSql);
        Select select = (Select) statement;
        PlainSelect plainSelect = (PlainSelect) select.getSelectBody();

        // 修改 WHERE 条件
        Expression newWhere = CCJSqlParserUtil.parseCondExpression(
            "status = 'inactive' AND department_id = 1"
        );
        plainSelect.setWhere(newWhere);

        // 输出修改后的 SQL
        System.out.println("修改后的SQL: " + select.toString());
        // 输出：SELECT * FROM user WHERE status = 'inactive' AND department_id = 1
    }
}
```

### 最佳实践

1. **性能优化**
   - 缓存权限规则，避免每次查询都重新计算
   - 对于复杂的权限规则，考虑使用 Redis 缓存
   - 合理使用索引，确保权限字段有索引

2. **安全性考虑**
   - 严禁通过前端传参来动态拼接权限条件
   - 权限规则的判断必须在后端完成
   - 记录权限相关的日志，便于审计

3. **代码组织**
   - 将权限处理器单独放在一个包中，便于维护
   - 为不同的业务场景创建不同的处理器
   - 使用策略模式管理多种权限规则

4. **测试建议**
   - 编写单元测试验证各种权限场景
   - 测试边界条件，如空权限、超级权限等
   - 进行性能测试，确保插件不影响系统性能

### 注意事项

1. **依赖管理**
   - MyBatis-Plus 3.5.9+ 需要单独引入 JSQLParser 依赖
   - 确保使用兼容的 JSQLParser 版本

2. **SQL 解析缓存**
   - 从 3.5.6 版本开始支持线程池解析复用
   - 可以自定义线程池配置以提高性能

3. **插件执行顺序**
   - 数据权限插件应该在分页插件之前执行
   - 如果有多个插件，注意它们的执行顺序

### Maven 依赖配置

```xml
<dependencies>
    <!-- MyBatis-Plus 核心 -->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.5.15</version>
    </dependency>

    <!-- JSQLParser（3.5.9+ 需要） -->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-jsqlparser</artifactId>
        <version>3.5.15</version>
    </dependency>
</dependencies>
```

### 常见问题解决

**Q1: 数据权限条件没有生效？**

A: 检查以下几点：
1. 确认拦截器已正确注册到 Spring 容器
2. 检查 Mapper 接口是否被正确扫描
3. 查看日志确认 SQL 是否被正确拼接

**Q2: 性能问题，查询变慢？**

A: 优化建议：
1. 启用 SQL 解析缓存
2. 为权限字段添加数据库索引
3. 简化复杂的权限逻辑

**Q3: 如何忽略某些方法的权限控制？**

A: 使用 `@InterceptorIgnore` 注解：

```java
@InterceptorIgnore(dataPermission = "true")
List<User> selectAllUsers(); // 此方法不会添加数据权限条件
```

---

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097
