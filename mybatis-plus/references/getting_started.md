# Mybatis-Plus - Getting Started 快速开始

**Pages:** 7

---

## 简介 | MyBatis-Plus

**URL:** https://baomidou.com/introduce

### 什么是 MyBatis-Plus？

MyBatis-Plus 是一个 MyBatis 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

### 核心理念

**只做增强不做改变**：引入它不会对现有工程产生影响，如丝般顺滑。

### 核心特性

#### 1. 强大的 CRUD 操作

只需简单配置，即可快速进行单表 CRUD 操作，从而节省大量时间。

```java
// 无需编写 Mapper XML 文件
public interface UserMapper extends BaseMapper<User> {
    // 继承 BaseMapper 后，自动拥有 CRUD 方法
}

// 使用示例
List<User> users = userMapper.selectList(null);
User user = userMapper.selectById(1L);
```

#### 2. 内置代码生成器

代码生成、自动分页、逻辑删除、自动填充、拦截器等功能一应俱全。

#### 3. 条件构造器

提供强大的条件构造器，支持 Lambda 表达式，避免硬编码字段名。

```java
// Lambda 表达式查询
LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
wrapper.eq(User::getName, "张三")
       .ge(User::getAge, 18)
       .orderByDesc(User::getCreateTime);

List<User> users = userMapper.selectList(wrapper);
```

#### 4. 多种主键策略

支持多种主键策略，包括分布式 ID 生成器。

```java
@TableId(type = IdType.ASSIGN_ID)
private Long id;  // 自动生成雪花算法 ID
```

### 支持的数据库

任何能使用 MyBatis 进行增删改查，并且支持标准 SQL 的数据库都应该在 MyBatis-Plus 的支持范围内：

- **MySQL**（5.5、5.6、5.7、8.0+）
- **Oracle**（11g、12c、19c）
- **PostgreSQL**（9.6、10、11、12+）
- **SQL Server**（2012、2014、2016、2017、2019）
- **H2**（1.4.200+）
- **DB2**
- **MariaDB**（10.1、10.2、10.3、10.4、10.5）
- **SQLite**
- **达梦数据库**
- **人大金仓**
- **神通数据库**
- **南大通用**
- 其他支持标准 SQL 的数据库

### 框架结构

```
┌─────────────┐
│   启动器    │  Spring Boot Starter
├─────────────┤
│   核心      │  核心功能、注解、配置
├─────────────┤
│   扩展      │  代码生成、插件扩展
├─────────────┤
│   示例      │  完整的示例代码
└─────────────┘
```

### 获得的荣誉

MyBatis-Plus 已连续 5 年（2017、2018、2019、2020、2021）获得"OSC 年度最受欢迎中国开源软件"殊荣。

### 代码托管与参与贡献

- **GitHub**: https://github.com/baomidou/mybatis-plus
- **Gitee**: https://gitee.com/baomidou/mybatis-plus

欢迎各路好汉一起来参与完善 MyBatis-Plus，我们期待你的 PR！

### 教程、案例、使用者名单

请移步至 [Awesome-MyBatis-Plus](https://github.com/baomidou/awesome-mybatis-plus) 查看。

---

## 安装 | MyBatis-Plus

**URL:** https://baomidou.com/getting-started/install

### 系统要求

全新的 MyBatis-Plus 3.0 版本基于 JDK8，提供了 lambda 形式的调用，所以安装集成 MP 3.0 要求如下：

- **JDK**: JDK 8.0+
- **Maven**: 3.0+
- **Spring Boot**: 2.0+ / 3.0+

### 版本说明

**版本 3.5.9+ 重要变更**：插件部分开始修改为可选依赖，需要单独引入相应的插件支持模块。

**重要提示**：引入 MyBatis-Plus 之后请不要再次引入 MyBatis 以及 `mybatis-spring-boot-starter` 和 `MyBatis-Spring`，以避免因版本差异导致的问题。

**自 3.5.4 开始**：在没有使用 `mybatis-plus-boot-starter`、`mybatis-plus-spring-boot3-starter` 或 `mybatis-plus-spring-boot4-starter` 的情况下，请自行根据项目情况引入 `mybatis-spring`。

### Spring Boot 2.x 安装

#### Maven 方式

```xml
<dependencies>
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.5.15</version>
    </dependency>
</dependencies>
```

#### Gradle 方式

```gradle
implementation 'com.baomidou:mybatis-plus-boot-starter:3.5.15'
```

### Spring Boot 3.x 安装

#### Maven 方式

```xml
<dependencies>
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-spring-boot3-starter</artifactId>
        <version>3.5.15</version>
    </dependency>
</dependencies>
```

#### Gradle 方式

```gradle
implementation 'com.baomidou:mybatis-plus-spring-boot3-starter:3.5.15'
```

### Spring Boot 4.x 安装（自 3.5.13 开始）

#### Maven 方式

```xml
<dependencies>
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-spring-boot4-starter</artifactId>
        <version>3.5.15</version>
    </dependency>
</dependencies>
```

### Spring MVC 安装（非 Boot 项目）

#### Maven 方式

```xml
<dependencies>
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus</artifactId>
        <version>3.5.15</version>
    </dependency>
</dependencies>
```

### Snapshot 版本安装

快照 SNAPSHOT 版本需要添加仓库，且版本号为快照版本。点击查看最新快照版本号。

**自 3.5.11-SNAPSHOTS 开始**，中央快照仓库地址变更为：

```
https://central.sonatype.com/repository/maven-snapshots/
```

版本有效期为 90 天。

**Maven 配置示例**：

```xml
<repositories>
    <repository>
        <id>sonatype-snapshots</id>
        <url>https://central.sonatype.com/repository/maven-snapshots/</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.5.16-SNAPSHOT</version>
    </dependency>
</dependencies>
```

**注意**：当使用代理仓库无法下载快照时，请在 mirrorOf 中加上 `!snapshots`。

### Maven BOM 依赖管理

使用 Maven BOM 管理依赖，减少版本号的冲突。因为 JSQLParser 5.0+ 版本不再支持 JDK8，针对这个问题解耦了 JSQLParser 依赖。

**正确使用姿势**：

1. 引入 `mybatis-plus-bom` 模块
2. 引入对应的 starter 和 jsqlparser 依赖

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-bom</artifactId>
            <version>3.5.15</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

<dependencies>
    <!-- 核心启动器 -->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
    </dependency>

    <!-- JSQLParser 依赖（跟随最新版本） -->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-jsqlparser</artifactId>
    </dependency>

    <!-- 或者使用特定版本的 JSQLParser -->
    <!--
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-jsqlparser-4.9</artifactId>
    </dependency>
    -->
</dependencies>
```

**依赖说明**：

- `mybatis-plus-jsqlparser`: 此依赖会跟着 JSQLParser 最新版本支持更新
- `mybatis-plus-jsqlparser-xx`: 为具体的 JSQLParser 特定支持版本，也就是无法兼容更新的版本

---

## 快速开始 | MyBatis-Plus

**URL:** https://baomidou.com/getting-started/

### 准备工作

我们将通过一个简单的 Demo 来阐述 MyBatis-Plus 的强大功能，在此之前，我们假设您已经：

- 拥有 Java 开发环境
- 熟悉 Spring Boot
- 熟悉 MyBatis
- 拥有 H2 或 MySQL 数据库环境

### 创建数据库和表

首先，我们创建一个 User 表：

```sql
DROP TABLE IF EXISTS `user`;

CREATE TABLE `user`
(
    id       BIGINT(20) NOT NULL COMMENT '主键ID',
    name     VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
    age      INT(11) NULL DEFAULT NULL COMMENT '年龄',
    email    VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
    create_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    update_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (id)
);

DELETE FROM `user`;

INSERT INTO `user` (id, name, age, email)
VALUES (1, 'Jone', 18, 'test1@baomidou.com'),
       (2, 'Jack', 20, 'test2@baomidou.com'),
       (3, 'Tom', 28, 'test3@baomidou.com'),
       (4, 'Sandy', 21, 'test4@baomidou.com'),
       (5, 'Billie', 24, 'test5@baomidou.com');
```

### 初始化 Spring Boot 工程

创建一个空的 Spring Boot 工程，可以使用 Spring Initializer 快速初始化：

1. 访问 https://start.spring.io/
2. 选择 Spring Boot 版本（推荐 2.7.x 或 3.x）
3. 添加依赖：Spring Web、Spring Data JPA（稍后替换为 MyBatis-Plus）
4. 生成项目并导入 IDE

### 添加依赖

删除 MyBatis 相关的依赖，添加 MyBatis-Plus 依赖：

#### Maven（pom.xml）

```xml
<dependencies>
    <!-- Spring Boot Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- MyBatis-Plus Starter -->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.5.15</version>
    </dependency>

    <!-- 数据库驱动（以 MySQL 为例） -->
    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <scope>runtime</scope>
    </dependency>

    <!-- Lombok（可选） -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>

    <!-- 测试依赖 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

#### Gradle（build.gradle）

```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'com.baomidou:mybatis-plus-boot-starter:3.5.15'
    runtimeOnly 'com.mysql:mysql-connector-j'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

### 配置数据库连接

在 `application.yml` 中配置数据库连接信息：

```yaml
spring:
  datasource:
    # MySQL 8.x 配置
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatis_plus_demo?useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai
    username: root
    password: your_password

# MyBatis-Plus 配置
mybatis-plus:
  configuration:
    # 开启驼峰命名转换
    map-underscore-to-camel-case: true
    # 开启日志输出
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  global-config:
    db-config:
      # 主键类型（AUTO 自增）
      id-type: auto
      # 逻辑删除字段
      logic-delete-field: deleted
      logic-delete-value: 1
      logic-not-delete-value: 0
```

H2 数据库配置示例（用于测试）：

```yaml
spring:
  datasource:
    driver-class-name: org.h2.Driver
    url: jdbc:h2:mem:test
    username: root
    password: test
  h2:
    console:
      enabled: true
```

### 编写实体类

```java
package com.example.mybatisplus.entity;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableField;
import lombok.Data;

import java.time.LocalDateTime;

@Data
public class User {

    /**
     * 主键ID
     * 使用 @TableId 注解标记主键字段
     * type = IdType.AUTO 表示数据库自增
     */
    @TableId(type = IdType.AUTO)
    private Long id;

    /**
     * 姓名
     */
    private String name;

    /**
     * 年龄
     */
    private Integer age;

    /**
     * 邮箱
     */
    private String email;

    /**
     * 创建时间
     */
    private LocalDateTime createTime;

    /**
     * 更新时间
     */
    private LocalDateTime updateTime;

    /**
     * 逻辑删除标记（0-未删除，1-已删除）
     */
    @TableField(select = false)  // 查询时不返回此字段
    private Integer deleted;
}
```

### 编写 Mapper 接口

```java
package com.example.mybatisplus.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.example.mybatisplus.entity.User;
import org.apache.ibatis.annotations.Mapper;

/**
 * User Mapper 接口
 * 继承 BaseMapper 后，自动拥有 CRUD 方法
 */
@Mapper
public interface UserMapper extends BaseMapper<User> {
    // 继承 BaseMapper<User> 后，无需编写任何方法
    // 自动拥有 insert、delete、update、select 等方法
}
```

### 配置 Mapper 扫描

在 Spring Boot 启动类中添加 `@MapperScan` 注解：

```java
package com.example.mybatisplus;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan("com.example.mybatisplus.mapper")  // 扫描 Mapper 接口
public class MybatisPlusApplication {

    public static void main(String[] args) {
        SpringApplication.run(MybatisPlusApplication.class, args);
        System.out.println("MyBatis-Plus 应用启动成功！");
    }
}
```

### 开始使用

编写测试类来验证功能：

```java
package com.example.mybatisplus;

import com.example.mybatisplus.entity.User;
import com.example.mybatisplus.mapper.UserMapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

@SpringBootTest
class MybatisPlusApplicationTests {

    @Autowired
    private UserMapper userMapper;

    /**
     * 查询所有用户
     */
    @Test
    void testSelectList() {
        // selectList() 方法的参数为 MyBatis-Plus 内置的条件构造器 Wrapper
        // 传入 null 表示无条件查询
        List<User> users = userMapper.selectList(null);
        users.forEach(System.out::println);
    }

    /**
     * 根据 ID 查询用户
     */
    @Test
    void testSelectById() {
        User user = userMapper.selectById(1L);
        System.out.println(user);
    }

    /**
     * 插入用户
     */
    @Test
    void testInsert() {
        User user = new User();
        user.setName("张三");
        user.setAge(25);
        user.setEmail("zhangsan@example.com");

        int result = userMapper.insert(user);
        System.out.println("插入结果：" + result);
        System.out.println("生成的主键：" + user.getId());
    }

    /**
     * 更新用户
     */
    @Test
    void testUpdate() {
        User user = new User();
        user.setId(1L);
        user.setAge(20);

        int result = userMapper.updateById(user);
        System.out.println("更新结果：" + result);
    }

    /**
     * 删除用户
     */
    @Test
    void testDelete() {
        int result = userMapper.deleteById(1L);
        System.out.println("删除结果：" + result);
    }
}
```

### 配置详解

#### application.yml 完整配置示例

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatis_plus_demo?useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai
    username: root
    password: your_password

mybatis-plus:
  # MyBatis 原生配置
  configuration:
    # 驼峰命名转换
    map-underscore-to-camel-case: true
    # 日志实现
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
    # 开启二级缓存
    cache-enabled: true
    # 延迟加载
    lazy-loading-enabled: true
    agressive-lazy-loading: false

  # 全局配置
  global-config:
    # 是否打印 LOGO
    banner: true
    # 是否初始化 SqlRunner
    enable-sql-runner: true

    db-config:
      # 主键类型（AUTO-数据库自增，INPUT-用户输入，ASSIGN_ID-雪花算法）
      id-type: auto
      # 表名前缀
      table-prefix: tbl_
      # 字段策略
      field-strategy: not_empty
      # 逻辑删除配置
      logic-delete-field: deleted
      logic-delete-value: 1
      logic-not-delete-value: 0

  # Mapper XML 文件位置
  mapper-locations: classpath*:mapper/**/*.xml
  # 实体类包路径
  type-aliases-package: com.example.mybatisplus.entity

# 日志配置
logging:
  level:
    com.example.mybatisplus.mapper: debug
```

### 小结

通过以上几个简单的步骤，我们就实现了 User 表的 CRUD 功能，甚至连 XML 文件都不用编写！

从以上步骤中，我们可以看到集成 MyBatis-Plus 非常的简单，只需要引入 starter 依赖，简单进行配置即可使用。

**MyBatis-Plus 的优势**：

1. **零 XML 配置**：无需编写 Mapper XML 文件
2. **丰富的 CRUD 方法**：开箱即用的增删改查功能
3. **强大的条件构造器**：支持链式调用和 Lambda 表达式
4. **代码生成器**：快速生成 Entity、Mapper、Service、Controller
5. **分页插件**：内置分页功能，支持多种数据库
6. **性能优化**：批量操作、缓存优化等

但 MyBatis-Plus 的强大远不止这些功能，想要详细了解 MyBatis-Plus 的强大功能？那就继续往下看吧！

---

## 配置 | MyBatis-Plus

**URL:** https://baomidou.com/getting-started/config/

### Spring Boot 项目配置

#### 基础配置类

```java
package com.example.mybatisplus.config;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@MapperScan("com.example.mybatisplus.mapper")  // 扫描 Mapper 接口
public class MybatisPlusConfig {

    /**
     * 配置 MyBatis-Plus 拦截器
     * 包含分页插件、乐观锁插件等
     */
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        // 添加分页插件
        PaginationInnerInterceptor paginationInterceptor = new PaginationInnerInterceptor(DbType.MYSQL);
        paginationInterceptor.setMaxLimit(500L);  // 单页最大限制数量
        paginationInterceptor.setOverflow(false);  // 溢出总页数后是否进行处理

        interceptor.addInnerInterceptor(paginationInterceptor);

        return interceptor;
    }
}
```

#### 配置元对象处理器

```java
package com.example.mybatisplus.handler;

import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import lombok.extern.slf4j.Slf4j;
import org.apache.ibatis.reflection.MetaObject;
import org.springframework.stereotype.Component;

import java.time.LocalDateTime;

@Slf4j
@Component
public class MyMetaObjectHandler implements MetaObjectHandler {

    /**
     * 插入操作时自动填充字段
     */
    @Override
    public void insertFill(MetaObject metaObject) {
        log.info("开始插入填充...");

        // 自动填充创建时间
        this.strictInsertFill(metaObject, "createTime", LocalDateTime.class, LocalDateTime.now());

        // 自动填充更新时间
        this.strictInsertFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now());

        // 可以根据业务需求填充其他字段
        // this.strictInsertFill(metaObject, "createBy", Long.class, getCurrentUserId());
    }

    /**
     * 更新操作时自动填充字段
     */
    @Override
    public void updateFill(MetaObject metaObject) {
        log.info("开始更新填充...");

        // 自动填充更新时间
        this.strictUpdateFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now());

        // 可以根据业务需求填充其他字段
        // this.strictUpdateFill(metaObject, "updateBy", Long.class, getCurrentUserId());
    }

    /**
     * 获取当前用户ID（示例）
     */
    private Long getCurrentUserId() {
        // 从 SecurityContext 或 ThreadLocal 获取当前用户ID
        return 1L;
    }
}
```

### Spring XML 项目配置

#### Spring XML 配置方式

```xml
<!-- MyBatis-Plus 配置 -->
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
    <!-- 数据源 -->
    <property name="dataSource" ref="dataSource"/>

    <!-- Mapper XML 文件位置 -->
    <property name="mapperLocations" value="classpath*:mapper/**/*.xml"/>

    <!-- 实体类包路径 -->
    <property name="typeAliasesPackage" value="com.example.mybatisplus.entity"/>

    <!-- MyBatis 配置文件 -->
    <property name="configLocation" value="classpath:mybatis-config.xml"/>

    <!-- 插件配置 -->
    <property name="plugins">
        <array>
            <ref bean="mybatisPlusInterceptor"/>
        </array>
    </property>

    <!-- 全局配置 -->
    <property name="globalConfig">
        <bean class="com.baomidou.mybatisplus.core.config.GlobalConfig">
            <property name="dbConfig">
                <bean class="com.baomidou.mybatisplus.core.config.GlobalConfig.DbConfig">
                    <property name="idType" value="AUTO"/>
                    <property name="tablePrefix" value="tbl_"/>
                </bean>
            </property>
        </bean>
    </property>
</bean>

<!-- Mapper 扫描配置 -->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.example.mybatisplus.mapper"/>
</bean>

<!-- MyBatis-Plus 拦截器 -->
<bean id="mybatisPlusInterceptor" class="com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor">
    <property name="interceptors">
        <list>
            <bean class="com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor">
                <constructor-arg name="dbType" value="MYSQL"/>
            </bean>
        </list>
    </property>
</bean>
```

### 配置说明

#### Mapper 扫描配置

**Spring Boot 方式**：

```java
@SpringBootApplication
@MapperScan("com.example.mybatisplus.mapper")
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

**Spring XML 方式**：

```xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.example.mybatisplus.mapper"/>
</bean>
```

#### SqlSessionFactory 配置

MyBatis-Plus 提供了自己的 `SqlSessionFactory` 实现，需要使用 `MybatisSqlSessionFactoryBean`。

**重要提示**：调整 SqlSessionFactory 为 MyBatis-Plus 的 SqlSessionFactory 是必须的步骤。

### 常见问题

**Q1: Mapper 接口找不到？**

A: 检查以下几点：
1. 确认 `@MapperScan` 注解的包路径是否正确
2. 确认 Mapper 接口是否添加了 `@Mapper` 注解
3. 检查 Mapper 接口是否继承了 `BaseMapper`

**Q2: 主键没有自动生成？**

A: 检查配置：
1. 实体类主键字段是否添加了 `@TableId` 注解
2. 确认主键类型配置是否正确（`id-type: auto`）
3. 数据库表是否设置了主键自增

**Q3: 日志不输出 SQL？**

A: 添加日志配置：

```yaml
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl

logging:
  level:
    com.example.mybatisplus.mapper: debug
```

---

## 单元测试 | MyBatis-Plus

**URL:** https://baomidou.com/getting-started/test/

### 依赖引入

```xml
<dependencies>
    <!-- MyBatis-Plus 测试启动器 -->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter-test</artifactId>
        <version>3.5.15</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### 使用示例

#### 基础测试类

```java
package com.example.mybatisplus;

import com.baomidou.mybatisplus.test.autoconfigure.MybatisPlusTest;
import com.example.mybatisplus.entity.User;
import com.example.mybatisplus.mapper.UserMapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;

import static org.assertj.core.api.Assertions.assertThat;

@MybatisPlusTest  // 自动配置 MyBatis-Plus 测试环境
class MybatisPlusSampleTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    void testInsert() {
        // 创建测试数据
        User user = new User();
        user.setName("测试用户");
        user.setAge(25);
        user.setEmail("test@example.com");

        // 执行插入
        userMapper.insert(user);

        // 验证结果
        assertThat(user.getId()).isNotNull();
        assertThat(user.getName()).isEqualTo("测试用户");
    }

    @Test
    void testSelectById() {
        // 根据ID查询
        User user = userMapper.selectById(1L);

        // 验证结果
        assertThat(user).isNotNull();
        assertThat(user.getId()).isEqualTo(1L);
    }

    @Test
    void testUpdate() {
        // 创建测试数据
        User user = new User();
        user.setId(1L);
        user.setName("更新后的名称");

        // 执行更新
        int result = userMapper.updateById(user);

        // 验证结果
        assertThat(result).isEqualTo(1);

        // 查询验证
        User updatedUser = userMapper.selectById(1L);
        assertThat(updatedUser.getName()).isEqualTo("更新后的名称");
    }
}
```

#### 使用 Mock 数据

```java
@MybatisPlusTest
@Import(MyMetaObjectHandler.class)  // 导入自定义配置
class UserMapperTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    void testCustomMethod() {
        // 测试自定义方法
        List<User> users = userMapper.selectByAge(18);
        assertThat(users).isNotEmpty();
    }
}
```

### 最佳实践

1. **使用 @MybatisPlusTest 注解**：自动配置测试环境，无需完整的 Spring Boot 上下文
2. **隔离测试**：每个测试方法应该独立，不依赖于其他测试的执行顺序
3. **使用事务回滚**：测试完成后自动回滚，避免污染数据库
4. **使用断言库**：推荐使用 AssertJ 的流式断言

---

## 开源周边 | MyBatis-Plus

**URL:** https://baomidou.com/getting-started/product/

MyBatis-Plus 生态系统提供了丰富的周边产品，包括：

- **MyBatis-Mate**：企业级高级特性
- **Dynamic-Datasource**：多数据源支持
- **MybatisX**：IDEA 插件，提供代码生成和跳转功能

详细的周边产品介绍请参考官方文档。

---

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097
