# Mybatis-Plus - Index 文档索引

## 📚 文档结构

欢迎使用 MyBatis-Plus 中文技术文档！本文档基于 MyBatis-Plus 3.5.15 版本编写，提供了完整的技术说明、代码示例和最佳实践。

## 📂 文档分类

### 快速入门

**文件:** `getting_started.md` | **页数:** 7

- **简介**: MyBatis-Plus 概述、核心特性、支持的数据库
- **安装**: Spring Boot 2/3/4 安装、Maven/Gradle 配置、BOM 依赖管理
- **快速开始**: 第一个 MyBatis-Plus 应用、数据库配置、实体类和 Mapper
- **配置**: Spring Boot 和 Spring MVC 配置详解
- **单元测试**: 使用 @MybatisPlusTest 进行测试

**适合人群**: MyBatis-Plus 初学者、需要快速上手的开发者

---

### 核心功能

**文件:** `core_features.md` | **页数:** 7

- **代码生成器**: AutoGenerator 使用、模板定制、自定义配置
- **条件构造器**: Wrapper 使用、Lambda 表达式、复杂查询
- **持久层接口**: BaseMapper、IService 方法详解
- **高级特性**: 数据权限、字段加密、多数据源

**适合人群**: 需要深入理解 MyBatis-Plus 核心功能的开发者

---

### 注解配置

**文件:** `annotation.md` | **页数:** 6

- **@TableName**: 表名映射配置
- **@TableId**: 主键策略配置
- **@TableField**: 字段映射配置
- **@Version**: 乐观锁注解
- **@EnumValue**: 枚举映射
- **@TableLogic**: 逻辑删除
- **自动填充**: MetaObjectHandler 实现
- **类型处理器**: TypeHandler 使用
- **字段策略**: FieldStrategy 详解

**适合人群**: 需要详细了解注解用法的开发者

---

### 配置说明

**文件:** `configuration.md` | **页数:** 11

- **分页插件**: PaginationInnerInterceptor 配置和使用
- **常见问题**: 开发中常见问题及解决方案
- **数据变动记录**: DataChangeRecorderInnerInterceptor
- **逻辑删除**: 配置和使用方法
- **SQL 分析**: p6spy 集成
- **使用配置**: application.yml 完整配置示例
- **注解配置**: 各注解详细属性说明
- **动态表名**: DynamicTableNameInnerInterceptor
- **代码生成器配置**: 新版代码生成器详解
- **MybatisX 插件**: IDEA 插件使用
- **主键生成策略**: IdType 详解

**适合人群**: 需要进行项目配置和问题排查的开发者

---

### 高级功能

**文件:** `advanced.md` | **页数:** 1

- **数据权限插件**: DataPermissionInterceptor
  - 基于部门的数据权限控制
  - 基于角色的数据权限控制
  - 基于组织树的数据权限控制
- **JSQLParser**: SQL 解析和修改
- **最佳实践**: 性能优化、安全考虑、代码组织

**适合人群**: 需要实现复杂数据权限控制的企业级应用开发者

---

### 代码生成器

**文件:** `code_generator.md` | **页数:** 2

- **自定义 ID 生成器**: IdentifierGenerator 实现
- **代码生成器**: FastAutoGenerator 使用
  - 数据库配置
  - 全局配置
  - 包配置
  - 策略配置
  - 模板配置

**适合人群**: 需要使用代码生成器提高开发效率的开发者

---

### 分页功能

**文件:** `pagination.md` | **页数:** 1

- **流式查询**: ResultHandler 使用、大数据处理
- **分页插件**: IPage 使用、自定义分页

**适合人群**: 需要处理大数据量和分页查询的开发者

---

### 性能优化

**文件:** `performance.md` | **页数:** 1

- **非法 SQL 拦截**: IllegalSQLInnerInterceptor
- **性能优化建议**: 索引优化、批量操作、缓存策略

**适合人群**: 关注应用性能的开发者

---

### 其他功能

**文件:** `other.md` | **页数:** 22

- **多数据源**: dynamic-datasource 集成
- **插件主体**: MybatisPlusInterceptor 详解
- **SQL 注入器**: 自定义全局方法
- **批量操作**: MybatisBatch 使用
- **数据安全**: 字段加密、配置加密、SQL 注入防护
- **自动维护 DDL**: 数据库表结构自动维护
- **防全表更新**: BlockAttackInnerInterceptor
- **多租户插件**: TenantLineInnerInterceptor
- **更新日志**: 各版本更新内容
- **企业级生态**: MyBatis-Mate、第三方产品

**适合人群**: 需要使用高级功能和企业级特性的开发者

---

## 🚀 快速导航

### 按学习路径

#### 1️⃣ 入门阶段（1-2天）
1. 阅读 [getting_started.md](./getting_started.md) - 了解基本概念
2. 跟随快速开始示例 - 创建第一个项目
3. 学习基础注解 - @TableName、@TableId、@TableField
4. 掌握基本 CRUD - BaseMapper 的常用方法

#### 2️⃣ 进阶阶段（3-5天）
1. 学习条件构造器 - Wrapper 的各种用法
2. 掌握分页插件 - PaginationInnerInterceptor 配置
3. 了解自动填充 - MetaObjectHandler 实现
4. 学习逻辑删除 - @TableLogic 使用

#### 3️⃣ 高级阶段（1-2周）
1. 深入代码生成器 - 定制化代码生成
2. 学习插件机制 - 自定义插件开发
3. 掌握多租户 - TenantLineInnerInterceptor
4. 了解数据权限 - DataPermissionInterceptor

#### 4️⃣ 专家阶段（持续学习）
1. 性能优化 - SQL 优化、索引优化
2. 安全加固 - SQL 注入防护、数据加密
3. 企业级特性 - 分布式主键、多数据源
4. 源码研读 - 理解底层实现原理

### 按功能分类

#### CRUD 操作
- **基础 CRUD**: [getting_started.md](./getting_started.md) - 快速开始
- **条件构造器**: [core_features.md](./core_features.md) - 条件构造器
- **批量操作**: [other.md](./other.md) - 批量操作

#### 插件扩展
- **分页插件**: [configuration.md](./configuration.md) - 分页插件
- **乐观锁**: [annotation.md](./annotation.md) - @Version
- **多租户**: [other.md](./other.md) - 多租户插件
- **数据权限**: [advanced.md](./advanced.md) - 数据权限插件

#### 代码生成
- **快速生成**: [code_generator.md](./code_generator.md) - 代码生成器
- **详细配置**: [configuration.md](./configuration.md) - 代码生成器配置
- **模板定制**: [core_features.md](./core_features.md) - 自定义模板

#### 企业级特性
- **多数据源**: [other.md](./other.md) - 多数据源支持
- **数据安全**: [other.md](./other.md) - 数据安全保护
- **分布式 ID**: [code_generator.md](./code_generator.md) - 自定义 ID 生成器
- **性能优化**: [performance.md](./performance.md) - 性能优化

---

## 📖 版本说明

本文档基于 **MyBatis-Plus 3.5.15** 版本编写。

### 版本亮点

#### 3.5.15 (2025-11-30)
- 升级 JSQLParser 到 4.9
- 优化分页插件性能
- 修复已知问题

#### 3.5.9+ (2024-10-23)
- **重要变更**: 插件部分改为可选依赖
- 需要单独引入 `mybatis-plus-jsqlparser` 依赖
- 支持 JSQLParser 4.9 和 5.x 版本

#### 3.5.5 (2024-07-31)
- 新增批量操作功能
- 优化 SQL 解析缓存
- 支持自定义线程池

#### 3.5.3.2 (2024-06-15)
- Wrapper 查询支持 TypeHandler
- 优化连续换行语句处理
- 新增 SQL 注入检查

---

## 💡 使用建议

### 1. 版本选择

- **Spring Boot 2.x**: 使用 `mybatis-plus-boot-starter`
- **Spring Boot 3.x**: 使用 `mybatis-plus-spring-boot3-starter`
- **Spring Boot 4.x**: 使用 `mybatis-plus-spring-boot4-starter` (3.5.13+)
- **非 Boot 项目**: 使用 `mybatis-plus`

### 2. 依赖管理

推荐使用 Maven BOM 管理依赖版本：

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
```

### 3. 学习路径建议

- **初学者**: 从 [getting_started.md](./getting_started.md) 开始，逐步学习
- **有经验开发者**: 重点阅读 [core_features.md](./core_features.md) 和 [configuration.md](./configuration.md)
- **架构师**: 深入研究 [advanced.md](./advanced.md) 和 [other.md](./other.md)

### 4. 实战建议

1. **从小项目开始**: 先在简单项目中熟悉 MyBatis-Plus
2. **阅读源码**: 理解核心功能的实现原理
3. **关注更新**: 定期查看官方更新日志
4. **参与社区**: 在 GitHub 上提交 Issue 和 PR

---

## 🔗 相关资源

### 官方资源
- **官方网站**: https://baomidou.com
- **GitHub**: https://github.com/baomidou/mybatis-plus
- **Gitee**: https://gitee.com/baomidou/mybatis-plus
- **API 文档**: https://baomidou.com/api/

### 社区资源
- **Awesome-MyBatis-Plus**: https://github.com/baomidou/awesome-mybatis-plus
- **示例代码**: https://github.com/baomidou/mybatis-plus-samples
- **视频教程**: 搜索 "MyBatis-Plus" 相关教程

### 开发工具
- **MybatisX 插件**: IDEA 插件，提供代码生成和跳转功能
- **MyBatis-Mate**: 企业级高级特性（付费）

---

## ❓ 常见问题

### Q: 如何选择 MyBatis-Plus 版本？

A:
- Spring Boot 2.x: 使用 3.5.x 系列版本
- Spring Boot 3.x: 使用 3.5.3+ 系列版本
- 推荐使用最新的稳定版本（当前 3.5.15）

### Q: MyBatis 和 MyBatis-Plus 可以同时使用吗？

A: 不建议同时使用。MyBatis-Plus 已经包含了 MyBatis 的全部功能，不需要额外引入 MyBatis 依赖。

### Q: 如何处理数据库关键字？

A:
1. 在表名或字段名上使用反引号
2. 使用 `@TableName` 和 `@TableField` 注解指定映射
3. 全局配置 `db-config.keyword-handler`

### Q: 如何实现复杂查询？

A:
1. 使用条件构造器 Wrapper
2. 在 Mapper XML 中编写自定义 SQL
3. 使用 `@Select` 注解编写 SQL

### Q: 性能如何？

A: MyBatis-Plus 在 MyBatis 基础上进行了优化：
- 内置缓存机制
- 支持批量操作
- SQL 优化建议
- 详见 [performance.md](./performance.md)

---

## 📝 更新日志

本文档最后更新时间：2025-01-18

### 最近更新
- ✅ 优化了所有中文文件的技术说明
- ✅ 补充了 3.5.15 版本的新特性
- ✅ 添加了更多代码示例和使用场景
- ✅ 完善了最佳实践和注意事项

---

## 🤝 贡献指南

欢迎参与文档改进：

1. Fork 本项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

---

## 📄 许可证

本文档采用 CC BY-NC-SA 4.0 许可证

---

## 📮 联系方式

如有问题或建议，请通过以下方式联系：

- **GitHub Issues**: https://github.com/baomidou/mybatis-plus/issues
- **官方论坛**: https://baomidou.com

---

**祝您使用愉快！🎉**

© 2016-2025 Baomidou™. All Rights Reserved.

Power by Astro Starlight | Sponsored by JetBrains

渝ICP备2021000141号-1 | 渝公网安备50011302222097
