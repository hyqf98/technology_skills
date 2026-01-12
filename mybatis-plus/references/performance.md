# Mybatis-Plus - Performance

**Pages:** 1

---

## 非法SQL拦截插件 | MyBatis-Plus

**URL:** https://baomidou.com/plugins/illegal-sql-intercept/

**Contents:**
- 非法SQL拦截插件
- 简介
- 功能特性
- 使用方法

IllegalSQLInnerInterceptor 是 MyBatis-Plus 框架中的一个安全控制插件，用于拦截和检查非法SQL语句。该插件旨在帮助开发者在SQL执行前发现并解决潜在的安全问题，如全表更新、删除操作，以及对索引的检查等。

IllegalSQLInnerInterceptor 插件是 MyBatis-Plus 提供的一个强大的安全工具，它能够帮助开发者提前发现并解决潜在的SQL安全问题。通过合理配置和使用该插件，可以大大提高数据库操作的安全性和效率。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (php):
```php
@Configurationpublic class MybatisPlusConfig {
    @Bean    public MybatisPlusInterceptor mybatisPlusInterceptor() {        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();        // 添加非法SQL拦截器        interceptor.addInnerInterceptor(new IllegalSQLInnerInterceptor());        return interceptor;    }}
```

Example 2 (jsx):
```jsx
<bean id="mybatisPlusInterceptor" class="com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor">    <property name="interceptors">        <list>            <bean class="com.baomidou.mybatisplus.extension.plugins.inner.IllegalSQLInnerInterceptor"/>        </list>    </property></bean>
```

---
