---
name: mybatis-plus
description: Use this skill when working with MyBatis-Plus - a powerful enhancement tool for MyBatis that simplifies database operations in Java/Spring Boot applications. Includes CRUD operations, code generation, pagination, Lambda expressions, and more.
---

# Mybatis-Plus Skill

Comprehensive assistance with mybatis-plus development, generated from official documentation.

## When to Use This Skill

This skill should be triggered when:
- Working with mybatis-plus
- Asking about mybatis-plus features or APIs
- Implementing mybatis-plus solutions
- Debugging mybatis-plus code
- Learning mybatis-plus best practices

## Quick Reference

### Common Patterns

*Quick reference patterns will be added as you use the skill.*

### Example Code Patterns

**Example 1** (typescript):
```typescript
<dependency>    <groupId>com.baomidou</groupId>    <artifactId>mybatis-plus-boot-starter</artifactId>    <version>3.5.15</version></dependency>
```

**Example 2** (json):
```json
implementation 'com.baomidou:mybatis-plus-boot-starter:3.5.15'
```

**Example 3** (typescript):
```typescript
<dependency>    <groupId>com.baomidou</groupId>    <artifactId>mybatis-plus-generator</artifactId>    <version>3.5.0</version></dependency>
```

**Example 4** (typescript):
```typescript
IPage<UserVo> selectPageVo(IPage<?> page, Integer state);// 或者自定义分页类MyPage selectPageVo(MyPage page);// 或者返回 ListList<UserVo> selectPageVo(IPage<UserVo> page, Integer state);
```

**Example 5** (csharp):
```csharp
private transient String noColumn;
```

## Reference Files

This skill includes comprehensive documentation in `references/`:

- **advanced.md** - Advanced documentation
- **annotation.md** - Annotation documentation
- **code_generator.md** - Code Generator documentation
- **configuration.md** - Configuration documentation
- **core_features.md** - Core Features documentation
- **getting_started.md** - Getting Started documentation
- **other.md** - Other documentation
- **pagination.md** - Pagination documentation
- **performance.md** - Performance documentation

Use `view` to read specific reference files when detailed information is needed.

## Working with This Skill

### For Beginners
Start with the getting_started or tutorials reference files for foundational concepts.

### For Specific Features
Use the appropriate category reference file (api, guides, etc.) for detailed information.

### For Code Examples
The quick reference section above contains common patterns extracted from the official docs.

## Resources

### references/
Organized documentation extracted from official sources. These files contain:
- Detailed explanations
- Code examples with language annotations
- Links to original documentation
- Table of contents for quick navigation

### scripts/
Add helper scripts here for common automation tasks.

### assets/
Add templates, boilerplate, or example projects here.

## Notes

- This skill was automatically generated from official documentation
- Reference files preserve the structure and examples from source docs
- Code examples include language detection for better syntax highlighting
- Quick reference patterns are extracted from common usage examples in the docs

## Updating

To refresh this skill with updated documentation:
1. Re-run the scraper with the same configuration
2. The skill will be rebuilt with the latest information
