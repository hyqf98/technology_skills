# Mybatis-Plus - Advanced

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

DataPermissionInterceptor 是 MyBatis-Plus 提供的一个插件，用于实现数据权限控制。它通过拦截执行的 SQL 语句，并动态拼接权限相关的 SQL 片段，来实现对用户数据访问的控制。

DataPermissionInterceptor 的工作原理与租户插件类似，它会在 SQL 执行前拦截 SQL 语句，并根据用户权限动态添加权限相关的 SQL 片段。这样，只有用户有权限访问的数据才会被查询出来。

仔细阅读插件的主要部分使用说明，确保正确注入数据权限插件，并自行定制 SQL 拼装逻辑。

JSQLParser 是一个开源的 SQL 解析库，可方便地解析和修改 SQL 语句。它是插件实现权限逻辑的关键工具，MyBatis-Plus 的数据权限依托于 JSQLParser 的解析能力。

以下示例展示如何使用 JSQLParser 来修改 SQL：

自定义 MultiDataPermissionHandler，实现特定业务逻辑。

将自定义的处理器注册到 DataPermissionInterceptor 中。

通过使用 DataPermissionInterceptor，你可以轻松地在 MyBatis-Plus 应用中实现数据权限控制，确保用户只能访问其权限范围内的数据，从而提高数据的安全性。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (java):
```java
new DataPermissionInterceptor(new MultiDataPermissionHandler() {
    @Override    public Expression getSqlSegment(final Table table, final Expression where, final String mappedStatementId) {        try {            String sqlSegment = sqlSegmentMap.get(mappedStatementId, table.getName());            if (sqlSegment == null) {                logger.info("{} {} AS {} : NOT FOUND", mappedStatementId, table.getName(), table.getAlias());                return null;            }            Expression sqlSegmentExpression = CCJSqlParserUtil.parseCondExpression(sqlSegment);            logger.info("{} {} AS {} : {}", mappedStatementId, table.getName(), table.getAlias(), sqlSegmentExpression.toString());            return sqlSegmentExpression;        } catch (JSQLParserException e) {            e.printStackTrace();        }        return null;    }});
```

Example 2 (sql):
```sql
// 示例 SQLString sql = "SELECT * FROM user WHERE status = 'active'";Expression expression;
try {    expression = CCJSqlParserUtil.parseCondExpression("status = 'inactive'");    PlainSelect select = (PlainSelect) ((Select) CCJSqlParserUtil.parse(sql)).getSelectBody();    select.setWhere(expression);
    System.out.println(select); // 输出：SELECT * FROM user WHERE status = 'inactive'} catch (JSQLParserException e) {    e.printStackTrace();}
```

Example 3 (java):
```java
public class CustomDataPermissionHandler extends MultiDataPermissionHandler {    @Override    public Expression getSqlSegment(Table table, Expression where, String mappedStatementId) {        // 在此处编写自定义数据权限逻辑        try {            String sqlSegment = "..."; // 数据权限相关的 SQL 片段            return CCJSqlParserUtil.parseCondExpression(sqlSegment);        } catch (JSQLParserException e) {            e.printStackTrace();            return null;        }    }}
```

Example 4 (unknown):
```unknown
// 在 MyBatis 配置中Interceptor dataPermissionInterceptor = new DataPermissionInterceptor(new CustomDataPermissionHandler());mybatisConfiguration.addInterceptor(dataPermissionInterceptor);
```

---
