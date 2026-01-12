# Mybatis-Plus - Getting Started

**Pages:** 7

---

## 安装 | MyBatis-Plus

**URL:** https://baomidou.com/getting-started/install

**Contents:**
- 安装
- Release
  - Spring Boot2
  - Spring Boot3
  - Spring Boot4 (自3.5.13开始)
  - Spring
  - Solon
- Snapshot
- Maven bom

全新的 MyBatis-Plus 3.0 版本基于 JDK8，提供了 lambda 形式的调用，所以安装集成 MP3.0 要求如下：

版本 3.5.9+ 插件部分开始修改为可选依赖，具体查看下文 maven bom 部分，根据自行项目选取对应插件支持模块。

引入 MyBatis-Plus 之后请不要再次引入 MyBatis 以及 mybatis-spring-boot-starter和MyBatis-Spring，以避免因版本差异导致的问题。

自3.5.4开始，在没有使用mybatis-plus-boot-starter或mybatis-plus-spring-boot3-starter，或mybatis-plus-spring-boot4-starter情况下，请自行根据项目情况引入mybatis-spring。

快照 SNAPSHOT 版本需要添加仓库，且版本号为快照版本 点击查看最新快照版本号。

自3.5.11-SNAPSHOTS开始，中央快照仓库地址变更为 https://central.sonatype.com/repository/maven-snapshots/ ，版本有效期为90天。

当你使用旧版本时,需要修改为: https://oss.sonatype.org/content/repositories/snapshots/

当使用代理仓库无法下载快照时，请在 mirrorOf 中加上 !snapshots。

使用 maven bom 管理依赖，减少版本号的冲突。因为 jsqlparser 5.0+ 版本不再支持 jdk8 针对这个问题解耦 jsqlparser 依赖。 正确打开姿势，引入 mybatis-plus-bom 模块，然后引入 ..starter 和 ..jsqlparser.. 依赖

mybatis-plus-jsqlparser: 此依赖会跟着jsqlparser最新版本支持更新

mybatis-plus-jsqlparser-xx: 为具体的jsqlparser特定支持版本，也就是无法兼容更新的板本。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (typescript):
```typescript
<dependency>    <groupId>com.baomidou</groupId>    <artifactId>mybatis-plus-boot-starter</artifactId>    <version>3.5.15</version></dependency>
```

Example 2 (json):
```json
implementation 'com.baomidou:mybatis-plus-boot-starter:3.5.15'
```

Example 3 (typescript):
```typescript
<dependency>    <groupId>com.baomidou</groupId>    <artifactId>mybatis-plus-spring-boot3-starter</artifactId>    <version>3.5.15</version></dependency>
```

Example 4 (json):
```json
implementation 'com.baomidou:mybatis-plus-spring-boot3-starter:3.5.15'
```

---

## 开源🎁周边 | MyBatis-Plus

**URL:** https://baomidou.com/getting-started/product/

**Contents:**
- 开源🎁周边
  - 夏日 T恤 👕

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

---

## 简介 | MyBatis-Plus

**URL:** https://baomidou.com/introduce

**Contents:**
- 简介
- 特性
- 支持数据库
- 框架结构
- 代码托管
- 参与贡献
- 教程、案例、使用者名单

MyBatis-Plus 是一个 MyBatis 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

我们的愿景是成为 MyBatis 最好的搭档，就像 魂斗罗 中的 1P、2P，基友搭配，效率翻倍。

任何能使用 MyBatis 进行增删改查，并且支持标准 SQL 的数据库应该都在 MyBatis-Plus 的支持范围内，具体支持情况如上。

如果您想要的数据库类型不在上面的列表，欢迎给我们 PR 您的数据库方言。

欢迎各路好汉一起来参与完善 MyBatis-Plus，我们期待你的 PR！

请移步至 Awesome-MyBatis-Plus 查看。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

---

## 单元测试 | MyBatis-Plus

**URL:** https://baomidou.com/getting-started/test/

**Contents:**
- 单元测试
- 示例工程
- 使用教程

自动导入 MyBatis-Plus 测试所需相关配置，通过 @MybatisPlusTest 注解快速配置测试类。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (typescript):
```typescript
<dependency>    <groupId>com.baomidou</groupId>    <artifactId>mybatis-plus-boot-starter-test</artifactId>    <version>3.5.15</version></dependency>
```

Example 2 (json):
```json
implementation 'com.baomidou:mybatis-plus-boot-starter-test:3.5.15'
```

Example 3 (java):
```java
import org.junit.jupiter.api.Test;import org.springframework.beans.factory.annotation.Autowired;
import static org.assertj.core.api.Assertions.assertThat;
@MybatisPlusTestclass MybatisPlusSampleTest {
    @Autowired    private SampleMapper sampleMapper;
    @Test    void testInsert() {        Sample sample = new Sample();        sampleMapper.insert(sample);        assertThat(sample.getId()).isNotNull();    }}
```

---

## 快速开始 | MyBatis-Plus

**URL:** https://baomidou.com/getting-started/

**Contents:**
- 快速开始
- 初始化工程
- 添加依赖
  - Spring Boot2
  - Spring Boot3
  - Spring Boot4 (自3.5.13开始)
- 配置
- 编码
- 开始使用
- 小结

我们将通过一个简单的 Demo 来阐述 MyBatis-Plus 的强大功能，在此之前，我们假设您已经：

如果从零开始用 MyBatis-Plus 来实现该表的增删改查我们需要做什么呢？

创建一个空的 Spring Boot 工程，加入 H2 数据库进行集成测试。

点此 Spring Initializer 可快速初始化一个 Spring Boot 工程

引入 MyBatis-Plus Starter 依赖

在 application.yml 配置文件中添加 H2 数据库的相关配置：

上面的配置是任何一个 Spring Boot 工程都会配置的数据库链接信息，如果您使用的是其他数据库，如 MySQL，则需要修改相应的配置信息。

在 Spring Boot 启动类中添加 @MapperScan 注解，扫描 Mapper 文件夹：

上面的代码中使用了 Lombok 进行代码生成，如果您不习惯，请自行生成相关 Getter/Setter 方法。

编写 Mapper 接口类 UserMapper.java：

UserMapper 中的 selectList() 方法的参数为 MP 内置的条件封装器 Wrapper，所以不填写就是无任何条件

完整的代码示例请移步：Spring Boot 快速启动示例 | Spring MVC 快速启动示例

通过以上几个简单的步骤，我们就实现了 User 表的 CRUD 功能，甚至连 XML 文件都不用编写！

从以上步骤中，我们可以看到集成 MyBatis-Plus 非常的简单，只需要引入 starter 依赖，简单进行配置即可使用。

但 MyBatis-Plus 的强大远不止这些功能，想要详细了解 MyBatis-Plus 的强大功能？那就继续往下看吧！

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (sql):
```sql
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user`(    id BIGINT NOT NULL COMMENT '主键ID',    name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',    age INT NULL DEFAULT NULL COMMENT '年龄',    email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',    PRIMARY KEY (id));
```

Example 2 (sql):
```sql
DELETE FROM `user`;
INSERT INTO `user` (id, name, age, email) VALUES(1, 'Jone', 18, 'test1@baomidou.com'),(2, 'Jack', 20, 'test2@baomidou.com'),(3, 'Tom', 28, 'test3@baomidou.com'),(4, 'Sandy', 21, 'test4@baomidou.com'),(5, 'Billie', 24, 'test5@baomidou.com');
```

Example 3 (typescript):
```typescript
<dependency>    <groupId>com.baomidou</groupId>    <artifactId>mybatis-plus-boot-starter</artifactId>    <version>3.5.15</version></dependency>
```

Example 4 (json):
```json
implementation 'com.baomidou:mybatis-plus-boot-starter:3.5.15'
```

---

## 配置 | MyBatis-Plus

**URL:** https://baomidou.com/getting-started/config/

**Contents:**
- 配置
- Spring Boot 工程
- Spring 工程

集成 MyBatis-Plus 非常的简单，我们仅需要一些简单的配置即可使用 MyBatis-Plus 的强大功能！

在讲解配置之前，请确保您已经安装了 MyBatis-Plus，如果您尚未安装，请查看 安装 一章。

调整 SqlSessionFactory 为 MyBatis-Plus 的 SqlSessionFactory

通常来说，一般的简单工程，通过以上配置即可正常使用 MyBatis-Plus，具体可参考以下项目：Spring Boot 快速启动示例、Spring MVC 快速启动示例。

同时 MyBatis-Plus 提供了大量的个性化配置来满足不同复杂度的工程，大家可根据自己的项目按需取用，详细配置请参考使用配置一文。

针对复杂的表结构，我们也提供了丰富的字段注解以满足大家的特殊需求，详情请参考注解一文。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (java):
```java
@SpringBootApplication@MapperScan("com.baomidou.mybatisplus.samples.quickstart.mapper")public class Application {
    public static void main(String[] args) {        SpringApplication.run(Application.class, args);    }
}
```

Example 2 (jsx):
```jsx
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">    <property name="basePackage" value="com.baomidou.mybatisplus.samples.quickstart.mapper"/></bean>
```

Example 3 (jsx):
```jsx
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">    <property name="dataSource" ref="dataSource"/></bean>
```

---

## 安装 | MyBatis-Plus

**URL:** https://baomidou.com/getting-started/install/

**Contents:**
- 安装
- Release
  - Spring Boot2
  - Spring Boot3
  - Spring Boot4 (自3.5.13开始)
  - Spring
  - Solon
- Snapshot
- Maven bom

全新的 MyBatis-Plus 3.0 版本基于 JDK8，提供了 lambda 形式的调用，所以安装集成 MP3.0 要求如下：

版本 3.5.9+ 插件部分开始修改为可选依赖，具体查看下文 maven bom 部分，根据自行项目选取对应插件支持模块。

引入 MyBatis-Plus 之后请不要再次引入 MyBatis 以及 mybatis-spring-boot-starter和MyBatis-Spring，以避免因版本差异导致的问题。

自3.5.4开始，在没有使用mybatis-plus-boot-starter或mybatis-plus-spring-boot3-starter，或mybatis-plus-spring-boot4-starter情况下，请自行根据项目情况引入mybatis-spring。

快照 SNAPSHOT 版本需要添加仓库，且版本号为快照版本 点击查看最新快照版本号。

自3.5.11-SNAPSHOTS开始，中央快照仓库地址变更为 https://central.sonatype.com/repository/maven-snapshots/ ，版本有效期为90天。

当你使用旧版本时,需要修改为: https://oss.sonatype.org/content/repositories/snapshots/

当使用代理仓库无法下载快照时，请在 mirrorOf 中加上 !snapshots。

使用 maven bom 管理依赖，减少版本号的冲突。因为 jsqlparser 5.0+ 版本不再支持 jdk8 针对这个问题解耦 jsqlparser 依赖。 正确打开姿势，引入 mybatis-plus-bom 模块，然后引入 ..starter 和 ..jsqlparser.. 依赖

mybatis-plus-jsqlparser: 此依赖会跟着jsqlparser最新版本支持更新

mybatis-plus-jsqlparser-xx: 为具体的jsqlparser特定支持版本，也就是无法兼容更新的板本。

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097

**Examples:**

Example 1 (typescript):
```typescript
<dependency>    <groupId>com.baomidou</groupId>    <artifactId>mybatis-plus-boot-starter</artifactId>    <version>3.5.15</version></dependency>
```

Example 2 (json):
```json
implementation 'com.baomidou:mybatis-plus-boot-starter:3.5.15'
```

Example 3 (typescript):
```typescript
<dependency>    <groupId>com.baomidou</groupId>    <artifactId>mybatis-plus-spring-boot3-starter</artifactId>    <version>3.5.15</version></dependency>
```

Example 4 (json):
```json
implementation 'com.baomidou:mybatis-plus-spring-boot3-starter:3.5.15'
```

---
