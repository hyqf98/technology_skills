---
name: mybatis-plus
description: MyBatis-Plus - MyBatis 的强大增强工具，简化 Java/Spring Boot 应用中的数据库操作。包括 CRUD 操作、代码生成器、分页、Lambda 表达式等功能。
---

# MyBatis-Plus 技能文档

基于官方文档生成的 MyBatis-Plus 开发综合指南。

## 何时使用此技能

在以下场景中使用此技能：
- 开发 Spring Boot + MyBatis-Plus 应用
- 查询 MyBatis-Plus 功能或 API
- 实现 MyBatis-Plus 解决方案
- 调试 MyBatis-Plus 代码
- 学习 MyBatis-Plus 最佳实践

## 技术概述

**MyBatis-Plus** 是一个 MyBatis 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。它提供了丰富的 CRUD 操作封装，大幅减少 SQL 编写工作。

### 核心特性

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响
- **损耗小**：启动即会自动注入基本 CRUD，性能基本无损耗
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，通过少量配置即可实现单表大部分 CRUD 操作
- **Lambda 表达式**：编写查询条件无需担心字段写错
- **分页插件**：支持多种数据库物理分页
- **代码生成器**：通过配置即可生成 Mapper、Model、Service、Controller 层代码
- **内置分页插件**：基于 Mybatis 物理分页，开发者无需关心具体操作

### 版本说明

- **当前稳定版本**：3.5.15（2025年11月30日发布）
- **最低要求**：JDK 8+
- **兼容性**：支持 Spring Boot 2.x 和 3.x

## 快速参考

### 安装与配置

#### Maven 依赖

```xml
<dependencies>
    <!-- MyBatis-Plus 启动器 -->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.5.15</version>
    </dependency>

    <!-- 代码生成器（可选） -->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-generator</artifactId>
        <version>3.5.0</version>
    </dependency>

    <!-- 数据库驱动（以 MySQL 为例） -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

#### Gradle 依赖

```gradle
implementation 'com.baomidou:mybatis-plus-boot-starter:3.5.15'
implementation 'com.baomidou:mybatis-plus-generator:3.5.0'
runtimeOnly 'mysql:mysql-connector-java'
```

#### 配置文件

```yaml
# application.yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mydb?useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai
    username: root
    password: password
    driver-class-name: com.mysql.cj.jdbc.Driver

mybatis-plus:
  # Mapper XML 扫描路径
  mapper-locations: classpath*:/mapper/**/*.xml
  # 实体类包路径
  type-aliases-package: com.example.entity
  # 全局配置
  global-config:
    db-config:
      # 主键类型
      id-type: auto
      # 逻辑删除字段
      logic-delete-field: deleted
      # 逻辑删除值
      logic-delete-value: 1
      # 逻辑未删除值
      logic-not-delete-value: 0
  # 配置打印 SQL
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

### 常见配置模式

#### 模式 1：实体类定义

```java
package com.example.entity;

import com.baomidou.mybatisplus.annotation.*;
import lombok.Data;

import java.time.LocalDateTime;

@Data
@TableName("sys_user")
public class User {

    /**
     * 主键自增
     */
    @TableId(value = "id", type = IdType.AUTO)
    private Long id;

    /**
     * 用户名
     */
    @TableField("username")
    private String username;

    /**
     * 密码
     */
    @TableField("password")
    private String password;

    /**
     * 邮箱
     */
    @TableField("email")
    private String email;

    /**
     * 逻辑删除标记（0=未删除，1=已删除）
     */
    @TableLogic
    @TableField("deleted")
    private Integer deleted;

    /**
     * 创建时间
     */
    @TableField(value = "create_time", fill = FieldFill.INSERT)
    private LocalDateTime createTime;

    /**
     * 更新时间
     */
    @TableField(value = "update_time", fill = FieldFill.INSERT_UPDATE)
    private LocalDateTime updateTime;
}
```

#### 模式 2：Mapper 接口

```java
package com.example.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.example.entity.User;
import org.apache.ibatis.annotations.Mapper;

/**
 * 继承 BaseMapper 后无需编写 mapper.xml 文件
 * 即可获得基本的 CRUD 功能
 */
@Mapper
public interface UserMapper extends BaseMapper<User> {
    // 可以自定义 SQL 方法
    // User selectByUsername(String username);
}
```

#### 模式 3：Service 接口与实现

```java
package com.example.service;

import com.baomidou.mybatisplus.extension.service.IService;
import com.example.entity.User;

/**
 * 继承 IService 后获得丰富的 CRUD 方法
 */
public interface UserService extends IService<User> {
    // 可以自定义业务方法
    User getUserByUsername(String username);
}
```

```java
package com.example.service.impl;

import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import com.example.entity.User;
import com.example.mapper.UserMapper;
import com.example.service.UserService;
import org.springframework.stereotype.Service;

/**
 * ServiceImpl 提供了 IService 的默认实现
 * 只需传入 Mapper 和实体类型
 */
@Service
public class UserServiceImpl
        extends ServiceImpl<UserMapper, User>
        implements UserService {

    @Override
    public User getUserByUsername(String username) {
        // 使用 Lambda 查询
        return lambdaQuery()
                .eq(User::getUsername, username)
                .one();
    }
}
```

#### 模式 4：Lambda 条件构造器

```java
package com.example.controller;

import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.example.entity.User;
import com.example.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/users")
public class UserController {

    @Autowired
    private UserService userService;

    /**
     * 根据 ID 查询
     */
    @GetMapping("/{id}")
    public User getById(@PathVariable Long id) {
        return userService.getById(id);
    }

    /**
     * Lambda 查询列表
     */
    @GetMapping
    public List<User> list(@RequestParam(required = false) String username) {
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(User::getUsername, username)
               .orderByDesc(User::getCreateTime);
        return userService.list(wrapper);
    }

    /**
     * 分页查询
     */
    @GetMapping("/page")
    public Page<User> page(
            @RequestParam(defaultValue = "1") Integer current,
            @RequestParam(defaultValue = "10") Integer size) {
        Page<User> page = new Page<>(current, size);
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        wrapper.orderByDesc(User::getCreateTime);
        return userService.page(page, wrapper);
    }

    /**
     * 保存或更新
     */
    @PostMapping
    public boolean save(@RequestBody User user) {
        return userService.saveOrUpdate(user);
    }

    /**
     * 根据 ID 删除
     */
    @DeleteMapping("/{id}")
    public boolean delete(@PathVariable Long id) {
        return userService.removeById(id);
    }
}
```

#### 模式 5：自定义 SQL（多表关联）

```java
package com.example.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.example.entity.User;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

import java.util.List;
import java.util.Map;

@Mapper
public interface UserMapper extends BaseMapper<User> {

    /**
     * 自定义 SQL 查询
     */
    @Select("SELECT * FROM sys_user WHERE username = #{username}")
    User selectByUsername(@Param("username") String username);

    /**
     * 多表关联查询
     */
    @Select("SELECT u.*, r.role_name " +
            "FROM sys_user u " +
            "LEFT JOIN sys_user_role ur ON u.id = ur.user_id " +
            "LEFT JOIN sys_role r ON ur.role_id = r.id " +
            "WHERE u.id = #{userId}")
    Map<String, Object> selectUserWithRole(@Param("userId") Long userId);

    /**
     * 使用 XML 方式定义的复杂查询
     */
    List<User> selectUsersByCondition(@Param("condition") Map<String, Object> condition);
}
```

```xml
<!-- resources/mapper/UserMapper.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.mapper.UserMapper">

    <select id="selectUsersByCondition" resultType="com.example.entity.User">
        SELECT * FROM sys_user
        <where>
            <if test="condition.username != null and condition.username != ''">
                AND username LIKE CONCAT('%', #{condition.username}, '%')
            </if>
            <if test="condition.email != null and condition.email != ''">
                AND email = #{condition.email}
            </if>
            <if test="condition.startTime != null">
                AND create_time >= #{condition.startTime}
            </if>
            <if test="condition.endTime != null">
                AND create_time <= #{condition.endTime}
            </if>
        </where>
        ORDER BY create_time DESC
    </select>

</mapper>
```

#### 模式 6：分页插件配置

```java
package com.example.config;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MybatisPlusConfig {

    /**
     * 配置分页插件
     */
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        // 添加分页插件
        PaginationInnerInterceptor paginationInterceptor = new PaginationInnerInterceptor();
        paginationInterceptor.setDbType(DbType.MYSQL); // 设置数据库类型
        paginationInterceptor.setLimit(1000); // 设置单页最大限制数量
        interceptor.addInnerInterceptor(paginationInterceptor);

        return interceptor;
    }
}
```

#### 模式 7：自动填充配置

```java
package com.example.config;

import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import org.apache.ibatis.reflection.MetaObject;
import org.springframework.stereotype.Component;

import java.time.LocalDateTime;

@Component
public class MyMetaObjectHandler implements MetaObjectHandler {

    /**
     * 插入时自动填充
     */
    @Override
    public void insertFill(MetaObject metaObject) {
        this.strictInsertFill(metaObject, "createTime", LocalDateTime.class, LocalDateTime.now());
        this.strictInsertFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now());
    }

    /**
     * 更新时自动填充
     */
    @Override
    public void updateFill(MetaObject metaObject) {
        this.strictUpdateFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now());
    }
}
```

### 核心注解说明

| 注解 | 说明 | 使用位置 |
|------|------|---------|
| `@TableName` | 指定数据库表名 | 实体类 |
| `@TableId` | 指定主键字段 | 实体字段 |
| `@TableField` | 指定数据库字段名 | 实体字段 |
| `@TableLogic` | 指定逻辑删除字段 | 实体字段 |
| `@Version` | 指定乐观锁字段 | 实体字段 |
| `@EnumValue` | 指定枚举字段 | 枚举类 |
| `@TableField(fill = FieldFill.INSERT)` | 插入时自动填充 | 实体字段 |
| `@TableField(fill = FieldFill.INSERT_UPDATE)` | 插入和更新时自动填充 | 实体字段 |

## CRUD 接口速查

### BaseMapper 方法

```java
// 插入
int insert(T entity);                      // 插入一条记录

// 删除
int deleteById(Serializable id);           // 根据 ID 删除
int deleteByMap(Map<String, Object> map);  // 根据 map 条件删除
int delete(Wrapper<T> wrapper);            // 根据 wrapper 条件删除
int deleteBatchIds(Collection<?> ids);    // 批量删除

// 更新
int updateById(T entity);                  // 根据 ID 更新
int update(T entity, Wrapper<T> wrapper);  // 根据 wrapper 更新

// 查询
T selectById(Serializable id);             // 根据 ID 查询
List<T> selectBatchIds(Collection<?> ids); // 批量查询
List<T> selectByMap(Map<String, Object> map); // 根据 map 条件查询
List<T> selectList(Wrapper<T> wrapper);    // 根据 wrapper 查询列表
Long selectCount(Wrapper<T> wrapper);      // 根据 wrapper 查询总数
```

### IService 方法

```java
// 保存
boolean save(T entity);                    // 保存
boolean saveBatch(Collection<T> list);     // 批量保存
boolean saveOrUpdate(T entity);            // 保存或更新

// 删除
boolean removeById(Serializable id);       // 根据 ID 删除
boolean removeByMap(Map<String, Object> map);
boolean remove(Wrapper<T> wrapper);
boolean removeByIds(Collection<?> ids);   // 批量删除

// 更新
boolean updateById(T entity);              // 根据 ID 更新
boolean update(Wrapper<T> wrapper);        // 根据 wrapper 更新
boolean updateBatchById(Collection<T> list);

// 查询
T getById(Serializable id);                // 根据 ID 查询
List<T> list();                            // 查询全部
List<T> list(Wrapper<T> wrapper);          // 根据 wrapper 查询
long count();                              // 查询总数
long count(Wrapper<T> wrapper);

// 分页
Page<T> page(Page<T> page);                // 无条件分页
Page<T> page(Page<T> page, Wrapper<T> wrapper); // 有条件分页
```

## 最佳实践

### 1. 使用 Lambda 表达式避免字段拼写错误

```java
// ❌ 不推荐：字符串容易写错
LambdaQueryWrapper<User> wrapper1 = new LambdaQueryWrapper<>();
wrapper1.eq("username", "admin");  // 字段名写错不会报错

// ✅ 推荐：使用 Lambda 表达式
LambdaQueryWrapper<User> wrapper2 = new LambdaQueryWrapper<>();
wrapper2.eq(User::getUsername, "admin");  // 编译时检查
```

### 2. 链式查询

```java
// 使用链式查询提高代码可读性
List<User> users = lambdaQuery()
        .eq(User::getStatus, 1)
        .like(User::getUsername, "admin")
        .gt(User::getAge, 18)
        .orderByDesc(User::getCreateTime)
        .list();
```

### 3. 批量操作优化

```java
// 批量插入（推荐使用 saveBatch）
List<User> users = new ArrayList<>();
// 添加用户...
userService.saveBatch(users, 1000); // 每批 1000 条

// 批量更新
userService.updateBatchById(users, 1000);
```

### 4. 逻辑删除

```java
// 查询时自动过滤已删除数据
List<User> users = userService.list();

// 真正的物理删除（慎用）
userService.removeById(id, true);  // 设置 true 为物理删除
```

## 参考文档

此技能在 `references/` 目录中包含全面的文档：

| 文档 | 描述 |
|------|------|
| **advanced.md** | 高级功能文档 |
| **annotation.md** | 注解文档 |
| **code_generator.md** | 代码生成器文档 |
| **configuration.md** | 配置文档 |
| **core_features.md** | 核心功能文档 |
| **getting_started.md** | 入门指南 |
| **other.md** | 其他文档 |
| **pagination.md** | 分页文档 |
| **performance.md** | 性能优化文档 |

## 使用指南

### 对于初学者
1. 从 `getting_started.md` 开始了解基础
2. 创建 Spring Boot 项目并添加依赖
3. 定义实体类并添加注解
4. 创建 Mapper 接口继承 BaseMapper
5. 使用 CRUD 接口进行开发

### 对于进阶使用
- 学习 Lambda 条件构造器
- 掌握自定义 SQL 编写
- 配置分页插件
- 使用代码生成器

### 对于性能优化
- 合理使用批量操作
- 优化分页查询
- 使用索引
- 参考 `performance.md`

## 资源

### references/
从官方来源提取的组织化文档，包含：
- 详细的功能说明
- 带语言标注的代码示例
- 原始文档链接
- 快速导航目录

### scripts/
添加用于常见自动化任务的辅助脚本。

### assets/
添加模板、样板代码或示例项目。

## 注意事项

- 此技能从官方文档自动生成
- 确保 JDK 版本为 8 或以上
- 注意主键类型与数据库字段类型匹配
- 逻辑删除功能需要在全局配置中启用

## 更新说明

刷新此技能的文档：
1. 使用相同配置重新运行爬虫
2. 技能将使用最新信息重建

## 相关资源

### 官方文档
- [MyBatis-Plus 官方网站](https://baomidou.com/)
- [MyBatis-Plus GitHub](https://github.com/baomidou/mybatis-plus)
- [更新日志](https://baomidou.com/resources/changlog/)

### 中文资源
- [SpringBoot 3.0 集成MyBatis-Plus全流程实践指南](https://comate.baidu.com/zh/page/vaj57mux9gm)
- [SpringBoot 整合MyBatis Plus实现完整CRUD操作指南](https://comate.baidu.com/zh/page/sn23eivjdua)
- [【MyBatis-Plus】核心开发指南：高效CRUD与进阶实践](https://blog.csdn.net/2301_81073317/article/details/149438458)
- [SpringBoot | 基于MyBatis-Plus实现Lambda Query查询](https://blog.csdn.net/Andya_net/article/details/145016001)
