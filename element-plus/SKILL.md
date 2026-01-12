---
name: element-plus
description: Use this skill when working with Element Plus - a Vue 3 based component library for designers and developers. Covers all 81+ components including buttons, forms, tables, navigation, feedback, and data display. Includes setup, theming, internationalization, and component API reference.
---

# Element-Plus Skill

Comprehensive assistance with element-plus development, generated from official documentation.

## When to Use This Skill

This skill should be triggered when:
- Working with element-plus
- Asking about element-plus features or APIs
- Implementing element-plus solutions
- Debugging element-plus code
- Learning element-plus best practices

## Quick Reference

### Common Patterns

*Quick reference patterns will be added as you use the skill.*

### Example Code Patterns

**Example 1** (ts):
```ts
type Option = Record<string, any> | string | number | boolean
```

**Example 2** (vue):
```vue
<template>
  <el-skeleton :rows="5" animated />
</template>
```

**Example 3** (vue):
```vue
<template>
  <el-empty description="description" />
</template>
```

**Example 4** (vue):
```vue
<template>
  <el-empty>
    <el-button type="primary">Button</el-button>
  </el-empty>
</template>
```

**Example 5** (vue):
```vue
<template>
  <el-affix :offset="120">
    <el-button type="primary">Offset top 120px</el-button>
  </el-affix>
</template>
```

## Reference Files

This skill includes comprehensive documentation in `references/`:

- **basic.md** - Basic documentation
- **data.md** - Data documentation
- **feedback.md** - Feedback documentation
- **form.md** - Form documentation
- **getting_started.md** - Getting Started documentation
- **navigation.md** - Navigation documentation
- **other.md** - Other documentation
- **others.md** - Others documentation

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
