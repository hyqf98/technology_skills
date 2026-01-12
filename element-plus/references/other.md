# Element-Plus - Other

**Pages:** 37

---

## Use the imperative mood in the subject line

**URL:** llms-txt#use-the-imperative-mood-in-the-subject-line

---

## Remember to

**URL:** llms-txt#remember-to

---

## Changelog

**URL:** llms-txt#changelog

Element Plus team uses **weekly** release strategy under normal circumstance, but critical bug fixes would require hotfix so the actual release number **could be** more than 1 per week.

On this page you can only see the latest 30 records of our [Changelog](https://github.com/element-plus/element-plus/blob/dev/CHANGELOG.en-US.md).

You can see the [Upgrade Changes](https://github.com/element-plus/element-plus/issues/15834) after version ^(2.5.0).

---
Title: Commit Examples
URL: https://element-plus.org/en-US/guide/commit-examples
---

---

## Custom theme

**URL:** llms-txt#custom-theme

**Contents:**
- Change theme color
  - By SCSS variables
  - How to override it?
  - By CSS Variable

Element Plus uses BEM-styled CSS so that you can override styles easily. But if
you need to replace styles at a large scale, e.g. change the theme color from
blue to orange or green, maybe overriding them one by one is not a good idea.

We provide four ways to change the style variables.

## Change theme color

These are examples about custom theme.

- Full import: [element-plus-vite-starter](https://github.com/element-plus/element-plus-vite-starter)
- On demand: [unplugin-element-plus/examples/vite](https://github.com/element-plus/unplugin-element-plus)

### By SCSS variables

`theme-chalk` is written in SCSS.
You can find SCSS variables in [`packages/theme-chalk/src/common/var.scss`](https://github.com/element-plus/element-plus/blob/dev/packages/theme-chalk/src/common/var.scss).

### How to override it?

If your project also uses SCSS, you can directly change Element Plus style variables. Create a new style file, e.g. `styles/element/index.scss`:

Then in the entry file of your project, import this style file instead of Element's built CSS:

Create a `element/index.scss` to combine your variables and variables of element-plus. (If you import them in ts, they will not be combined.)

If you are using vite, and you want to custom theme when importing on demand.

Use `scss.additionalData` to compile variables with scss of every component.

If you are using webpack, and you want to custom theme when importing on demand.

CSS Variables is a very useful feature, already supported by almost all browsers. (IE: Wait?)

> Learn more from [Using CSS custom properties (variables) | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)

We have used css variables to reconstruct the style system of almost all components.

This means you can dynamically change individual variables inside the component to better customize it without having to modify scss and recompile it.

> In the future, the css variable names and role documentation for each component will be written to each component.

If you just want to customize a particular component, just add inline styles for certain components individually.

For performance reasons, it is more recommended to custom css variables under a class rather than the global `:root`.

If you want to control css var by script, try this:

If you want a more elegant way, check this out.
[useCssVar | VueUse](https://vueuse.org/core/usecssvar/)

---
Title: Built-in Transition
URL: https://element-plus.org/en-US/guide/transitions
---

**Examples:**

Example 1 (scss):
```scss
$colors: () !default;
$colors: map.deep-merge(
  (
    'white': #ffffff,
    'black': #000000,
    'primary': (
      'base': #409eff,
    ),
    'success': (
      'base': #67c23a,
    ),
    'warning': (
      'base': #e6a23c,
    ),
    'danger': (
      'base': #f56c6c,
    ),
    'error': (
      'base': #f56c6c,
    ),
    'info': (
      'base': #909399,
    ),
  ),
  $colors
);
```

Example 2 (unknown):
```unknown
Then in the entry file of your project, import this style file instead of Element's built CSS:



Create a `element/index.scss` to combine your variables and variables of element-plus. (If you import them in ts, they will not be combined.)
```

Example 3 (unknown):
```unknown
If you are using vite, and you want to custom theme when importing on demand.

Use `scss.additionalData` to compile variables with scss of every component.
```

Example 4 (unknown):
```unknown
If you are using webpack, and you want to custom theme when importing on demand.
```

---

## Navigation

**URL:** llms-txt#navigation

**Contents:**
- Choose the right navigation
- Side Navigation
  - Level 1 categories
  - Level 2 categories
  - Level 3 categories
- Top Navigation

Navigation focuses on solving the users' problems of where to go and how to get
there. Generally it can be categorized into 'sidebar navigation' and 'top navigation'.

## Choose the right navigation

An appropriate navigation gives users a smooth experience, while an inappropriate
one leads to confusion. Here are the differences between sidebar navigation and
top navigation.

Affix the navigation at the left edge, thus improves its visibility, making it
easier to switch between pages. In this case, the top area of the page holds
commonly used tools, e.g. search bar, help button, notice button, etc. Suitable
for background management or utility websites.

### Level 1 categories

Suitable for simply structured sites with only one level of pages. No breadcrumb is needed.

### Level 2 categories

Sidebar displays up to two levels of navigation. Breadcrumbs are recommended in
combination of second level navigation, making it easier for the users to locate
and navigate.

### Level 3 categories

Suitable for complicated utility websites. The left sidebar holds first level
navigation, and the middle column displays second level navigation or other utility
options.

Conforms to the normal browsing order from top to bottom, which makes things more
natural. The navigation amount and text length are limited to the width of the top.

Suitable for sites with few navigation and large chunks.

<TopNavigationExample />

---
Title: Quick Start
URL: https://element-plus.org/en-US/guide/quickstart
---

---

## Can use multiple lines with "-" for bullet points in body

**URL:** llms-txt#can-use-multiple-lines-with-"-"-for-bullet-points-in-body

---

## Development FAQ

**URL:** llms-txt#development-faq

**Contents:**
- If you encounter dependency related issues
- Link local dependencies

Here are the problems that are easy to encounter in development.

## If you encounter dependency related issues

## Link local dependencies

**Examples:**

Example 1 (shell):
```shell
rm -rf node_modules
pnpm i
```

---

## eg.

**URL:** llms-txt#eg.

**Contents:**
  - Sync locale files

pnpm gen awesome
pnpm gen awesome-button
shell
pnpm locale:sync
```

will sync the new fields from the `en.ts` locale file to other locale files and add the comment `// to be translated`.

---
Title: Internationalization
URL: https://element-plus.org/en-US/guide/i18n
---

**Examples:**

Example 1 (unknown):
```unknown
will generate a component template in `packages/components/awesome` and `packages/components/awesome-button` directory.

### Sync locale files

With command
```

---

## Quick Start

**URL:** llms-txt#quick-start

**Contents:**
- Usage
  - Full Import
  - On-demand Import
  - Manually import
- Starter Template
- Global Configuration
- Using Nuxt.js
- Let's Get Started

This section describes how to use Element Plus in your project.

If you don’t care about the bundle size so much, it’s more convenient to use full import.

If you use volar, please add the global component type definition to `compilerOptions.types` in `tsconfig.json`.

You need to use an additional plugin to import components you used.

#### Auto import <el-tag type="primary" style="vertical-align: middle;" effect="dark" size="small">Recommend</el-tag>

First you need to install `unplugin-vue-components` and `unplugin-auto-import`.

Then add the code below into your `Vite` or `Webpack` config file.

For more bundlers ([Rollup](https://rollupjs.org/), [Vue CLI](https://cli.vuejs.org/)) and configs please reference [unplugin-vue-components](https://github.com/antfu/unplugin-vue-components#installation) and [unplugin-auto-import](https://github.com/antfu/unplugin-auto-import#install).

For Nuxt users, you only need to install `@element-plus/nuxt`.

Then add the code below into your config file.

Refer to the [docs](https://github.com/element-plus/element-plus-nuxt#readme) for how to configure it.

Element Plus provides out of box [Tree Shaking](https://webpack.js.org/guides/tree-shaking/)
functionalities based on ES Module.

But you need install [unplugin-element-plus](https://github.com/element-plus/unplugin-element-plus) for style import.
And refer to the [docs](https://github.com/element-plus/unplugin-element-plus#readme) for how to configure it.

We provide a [Vite Template](https://github.com/element-plus/element-plus-vite-starter).

For Nuxt users we have a [Nuxt Template](https://github.com/element-plus/element-plus-nuxt-starter).

For Laravel users we have a [Laravel Template](https://github.com/element-plus/element-plus-in-laravel-starter).

## Global Configuration

When registering Element Plus, you can pass a global config object with `size` and
`zIndex` to set the default `size` for form components, and `zIndex` for
popup components, the default value for `zIndex` is `2000`.

We can also use [Nuxt.js](https://nuxt.com). Please refer to [Element Plus Nuxt.js starter template](https://github.com/element-plus/element-plus-nuxt-starter) for more details.

You can bootstrap your project from now on. For each components usage, please
refer to [the individual component documentation](https://element-plus.org/en-US/component/button.html).

---
Title: Server-Side Rendering (SSR)
URL: https://element-plus.org/en-US/guide/ssr
---

**Examples:**

Example 1 (unknown):
```unknown
#### Volar support

If you use volar, please add the global component type definition to `compilerOptions.types` in `tsconfig.json`.
```

Example 2 (unknown):
```unknown
### On-demand Import

You need to use an additional plugin to import components you used.

#### Auto import <el-tag type="primary" style="vertical-align: middle;" effect="dark" size="small">Recommend</el-tag>

First you need to install `unplugin-vue-components` and `unplugin-auto-import`.

::: code-group
```

Example 3 (unknown):
```unknown

```

Example 4 (unknown):
```unknown

```

---

## Installation

**URL:** llms-txt#installation

**Contents:**
- Compatibility ^(2.5.0)
  - Sass
  - Version
- Using Package Manager
- Import in Browser
  - unpkg
  - jsDelivr
- Hello World

## Compatibility ^(2.5.0)

Element Plus can run on browsers that support last 2 versions.

If you really need to support outdated browsers, please add [Babel](https://babeljs.io/) and Polyfill yourself.

Since Vue 3 no longer supports IE11, Element Plus does not support IE either.

| version | ![Chrome](https://cdn.jsdelivr.net/npm/@browser-logos/chrome/chrome_32x32.png) <br> Chrome | ![IE](https://cdn.jsdelivr.net/npm/@browser-logos/edge/edge_32x32.png) <br> Edge | ![Firefox](https://cdn.jsdelivr.net/npm/@browser-logos/firefox/firefox_32x32.png) <br> Firefox | ![Safari](https://cdn.jsdelivr.net/npm/@browser-logos/safari/safari_32x32.png) <br> Safari |
| ------- | ------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| < 2.5.0 | Chrome ≥ 64                                                                                | Edge ≥ 79                                                                        | Firefox ≥ 78                                                                                   | Safari ≥ 12                                                                                |
| 2.5.0 + | Chrome ≥ 85                                                                                | Edge ≥ 85                                                                        | Firefox ≥ 79                                                                                   | Safari ≥ 14.1                                                                              |

Version `2.8.5` and later, the minimum compatible version of [Sass](https://github.com/sass) is `1.79.0`.

If your terminal prompts `legacy JS API Deprecation Warning`, you can configure the following code in [vite.config.ts](https://vitejs.dev/config/shared-options.html#css-preprocessoroptions).

Element Plus is currently in a rapid development iteration. [![ElementPlus version badge](https://img.shields.io/npm/v/element-plus.svg?style=flat-square)](https://www.npmjs.org/package/element-plus)

In addition, every commit and PR on the dev branch will be published to [pkg.pr.new](https://github.com/stackblitz-labs/pkg.pr.new), if you want to use some unpublished content, you can refer to [here](https://github.com/element-plus/element-plus/issues/18433#issuecomment-2392618431).

## Using Package Manager

**We recommend using the package manager (NPM, [Yarn](https://classic.yarnpkg.com/lang/en/), [pnpm](https://pnpm.io/)) to install Element Plus**,
so that you can utilize bundlers like [Vite](https://vitejs.dev) and
[webpack](https://webpack.js.org/).

Choose a package manager you like.

If your network environment is not good, it is recommended to use a mirror registry [cnpm](https://github.com/cnpm/cnpm) or [npmmirror](https://npmmirror.com/).

Import Element Plus through browser HTML tags directly, and use global variable `ElementPlus`.

According to different CDN providers, there are different introduction methods.
Here we use [unpkg](https://unpkg.com) and [jsDelivr](https://jsdelivr.com) as example.
You can also use other CDN providers.

With CDN, we can easily use Element Plus to
write a Hello World page. [Online Demo](https://codepen.io/iamkun/pen/YzWMaVr)

<iframe height="469" style="width: 100%;" scrolling="no" title="YzWMaVr" src="https://codepen.io/iamkun/embed/YzWMaVr?height=469&theme-id=light&default-tab=html,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/iamkun/pen/YzWMaVr'>YzWMaVr</a> by iamkun
  (<a href='https://codepen.io/iamkun'>@iamkun</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

If you are installing via package manager and want to use it with
a packaging tool, please read the
next section: [Quick Start](/en-US/guide/quickstart).

---
Title: Migration
URL: https://element-plus.org/en-US/guide/migration
---

**Examples:**

Example 1 (unknown):
```unknown
### Version

Element Plus is currently in a rapid development iteration. [![ElementPlus version badge](https://img.shields.io/npm/v/element-plus.svg?style=flat-square)](https://www.npmjs.org/package/element-plus)

In addition, every commit and PR on the dev branch will be published to [pkg.pr.new](https://github.com/stackblitz-labs/pkg.pr.new), if you want to use some unpublished content, you can refer to [here](https://github.com/element-plus/element-plus/issues/18433#issuecomment-2392618431).

## Using Package Manager

**We recommend using the package manager (NPM, [Yarn](https://classic.yarnpkg.com/lang/en/), [pnpm](https://pnpm.io/)) to install Element Plus**,
so that you can utilize bundlers like [Vite](https://vitejs.dev) and
[webpack](https://webpack.js.org/).

Choose a package manager you like.

::: code-group
```

Example 2 (unknown):
```unknown

```

Example 3 (unknown):
```unknown

```

Example 4 (unknown):
```unknown
:::

If your network environment is not good, it is recommended to use a mirror registry [cnpm](https://github.com/cnpm/cnpm) or [npmmirror](https://npmmirror.com/).
```

---

## |<---- Try To Limit Each Line to a Maximum Of 72 Characters ---->|

**URL:** llms-txt#|<-----try-to-limit-each-line-to-a-maximum-of-72-characters----->|

---

## Translation

**URL:** llms-txt#translation

**Contents:**
- Documentation
  - Background
  - How do I translate the documentation?
  - How can I become a proofreader?

In this chapter, we will discuss how to help translating the documentation of Element Plus.

Before we did the upgrade of the documentation architecture, each documentation update needs 5 languages,
which most of the contributors use online translator for their non-native languages,
sometimes it would be not only inaccurate but also stressful to them.

So we decided to give the documentation site an upgrade.

- From webpack to Vite
- From manually maintained to automated

We took [Crowdin](https://crowdin.com) as our first step to make the documentation site more automated.

### How do I translate the documentation?

1. Create an account on [Crowdin](https://crowdin.com), it is recommended that you use your GitHub account to authorize Crowdin.
2. Go to [Element Plus](https://crowdin.com/project/element-plus) project.
3. Choose the language you want to contribute to.
4. Find the file you want to translate.
5. Do the translation.

That simple, and the UI is very intuitive to use, you should have no trouble using it.
After you submit your work, it would be online once the translation is approved by proofreader.

### How can I become a proofreader?

You can [raise an issue](https://crowdin.com/project/element-plus/discussions) on Crowdin to us for
becoming a proofreader of the language you wish to be.

---
Title: Affix
URL: https://element-plus.org/en-US/component/affix
---

---

## Use issues and merge requests' full URLs instead of short references,

**URL:** llms-txt#use-issues-and-merge-requests'-full-urls-instead-of-short-references,

---

## Use the body to explain what and why vs. how

**URL:** llms-txt#use-the-body-to-explain-what-and-why-vs.-how

---

## Subject must contain at least 3 words

**URL:** llms-txt#subject-must-contain-at-least-3-words

---

## DatePicker

**URL:** llms-txt#datepicker

**Contents:**
- Enter Date
- Other measurements
- Date Range
- Month Range
- Year Range ^(2.8.0)
- Default Value
- Date Formats
- Default time for start date and end date
- Set custom content of prefix
- Custom content

Use Date Picker for date input.

Basic date picker measured by 'day'.

## Other measurements

You can choose week, month, year or multiple dates by extending the standard date picker component.

Picking a date range is supported.

Picking a month range is supported.

## Year Range ^(2.8.0)

Picking a year range is supported.

If user hasn't picked a date, shows today's calendar by default. You can use `default-value` to set another date. Its value should be parsable by `new Date()`.

If type is `daterange`, `default-value` sets the left side calendar.

Use `format` to control displayed text's format in the input box. Use `value-format` to control binding value's format.

By default, the component accepts and emits a `Date` object.

Check the list [here](https://day.js.org/docs/en/display/format#list-of-all-available-formats) of all available formats of Day.js.

## Default time for start date and end date

When picking a date range, you can assign the time part for start date and end date.

## Set custom content of prefix

The content of prefix can be customized.

The content of cell can be customized, in scoped-slot you can get the cell data. Note that the custom content structure should be consistent with the default structure, otherwise style misalignment may occur.

## Custom icon ^(2.8.0)

Custom icons available with slots.

For data details, please refer:

The default locale of is English, if you need to use other languages, please check [Internationalization](/en-US/guide/i18n)

Note, date time locale (month name, first day of the week ...) are also configured in localization.

| Name                         | Description                                                                                                                           | Type                                                                                                                                                           | Default                            |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| model-value / v-model        | binding value, if it is an `range` picker, the length of the array should be 2                                                        | ^[number] / ^[string] / ^[Date] / ^[array]`number[] \| string[] \| Date[]`                                                                                     | ''                                 |
| readonly                     | whether DatePicker is read only                                                                                                       | ^[boolean]                                                                                                                                                     | false                              |
| disabled                     | whether DatePicker is disabled                                                                                                        | ^[boolean]                                                                                                                                                     | false                              |
| size                         | size of Input                                                                                                                         | ^[enum]`'' \| 'large' \| 'default' \| 'small'`                                                                                                                 | —                                  |
| editable                     | whether the input is editable                                                                                                         | ^[boolean]                                                                                                                                                     | true                               |
| clearable                    | whether to show clear button                                                                                                          | ^[boolean]                                                                                                                                                     | true                               |
| placeholder                  | placeholder in non-range mode                                                                                                         | ^[string]                                                                                                                                                      | ''                                 |
| start-placeholder            | placeholder for the start date in range mode                                                                                          | ^[string]                                                                                                                                                      | —                                  |
| end-placeholder              | placeholder for the end date in range mode                                                                                            | ^[string]                                                                                                                                                      | —                                  |
| type                         | type of the picker                                                                                                                    | ^[enum]`'year' \| 'years' \|'month' \| 'months' \| 'date' \| 'dates' \| 'datetime' \| 'week' \| 'datetimerange' \| 'daterange' \| 'monthrange' \| 'yearrange'` | date                               |
| format                       | format of the displayed value in the input box                                                                                        | ^[string] see [date formats](/en-US/component/date-picker#date-formats)                                                                                        | YYYY-MM-DD                         |
| popper-class                 | custom class name for DatePicker's dropdown                                                                                           | ^[string]                                                                                                                                                      | —                                  |
| popper-style                 | custom style for DatePicker's dropdown                                                                                                | ^[string] / ^[object]                                                                                                                                          | —                                  |
| popper-options               | Customized popper option see more at [popper.js](https://popper.js.org/docs/v2/)                                                      | ^[object]`Partial<PopperOptions>`                                                                                                                              | {}                                 |
| range-separator              | range separator                                                                                                                       | ^[string]                                                                                                                                                      | '-'                                |
| default-value                | optional, default date of the calendar                                                                                                | ^[object]`Date \| [Date, Date]`                                                                                                                                | —                                  |
| default-time                 | optional, the time value to use when selecting date range                                                                             | ^[object]`Date \| [Date, Date]`                                                                                                                                | —                                  |
| value-format                 | optional, format of binding value. If not specified, the binding value will be a Date object                                          | ^[string] see [date formats](/en-US/component/date-picker#date-formats)                                                                                        | —                                  |
| id                           | same as `id` in native input                                                                                                          | ^[string] / ^[object]`[string, string]`                                                                                                                        | —                                  |
| name                         | same as `name` in native input                                                                                                        | ^[string] / ^[object]`[string, string]`                                                                                                                        | ''                                 |
| unlink-panels                | unlink two date-panels in range-picker                                                                                                | ^[boolean]                                                                                                                                                     | false                              |
| prefix-icon                  | custom prefix icon component. By default, if the value of `type` is `TimeLikeType`, the value is `Clock`, else is `Calendar`          | ^[string] / ^[object]`Component`                                                                                                                               | ''                                 |
| clear-icon                   | custom clear icon component                                                                                                           | ^[string] / ^[object]`Component`                                                                                                                               | `CircleClose`                      |
| validate-event               | whether to trigger form validation                                                                                                    | ^[boolean]                                                                                                                                                     | true                               |
| disabled-date                | a function determining if a date is disabled with that date as its parameter. Should return a Boolean                                 | ^[Function]`(data: Date) => boolean`                                                                                                                           | —                                  |
| shortcuts                    | an object array to set shortcut options                                                                                               | ^[object]`Array<{ text: string, value: Date \| Function }>`                                                                                                    | []                                 |
| cell-class-name              | set custom className                                                                                                                  | ^[Function]`(data: Date) => string`                                                                                                                            | —                                  |
| teleported                   | whether date-picker dropdown is teleported to the body                                                                                | ^[boolean]                                                                                                                                                     | true                               |
| empty-values ^(2.7.0)        | empty values of component, [see config-provider](/en-US/component/config-provider#empty-values-configurations)                        | ^[array]                                                                                                                                                       | —                                  |
| value-on-clear ^(2.7.0)      | clear return value, [see config-provider](/en-US/component/config-provider#empty-values-configurations)                               | ^[string] / ^[number] / ^[boolean] / ^[Function]                                                                                                               | —                                  |
| fallback-placements ^(2.8.4) | list of possible positions for Tooltip [popper.js](https://popper.js.org/docs/v2/modifiers/flip/#fallbackplacements)                  | ^[array]`Placement[]`                                                                                                                                          | ['bottom', 'top', 'right', 'left'] |
| placement ^(2.8.4)           | position of dropdown                                                                                                                  | `Placement`                                                                                                                                                    | bottom                             |
| show-footer ^(2.10.5)        | whether to show footer                                                                                                                | ^[boolean]                                                                                                                                                     | true                               |
| show-confirm ^(2.11.0)       | whether to show the confirm button                                                                                                    | ^[boolean]                                                                                                                                                     | true                               |
| show-week-number ^(2.10.3)   | show the week number besides the week                                                                                                 | ^[boolean]                                                                                                                                                     | false                              |
| automatic-dropdown ^(2.11.4) | this prop decides if the date picker panel pops up when the input is focused. (The default value will be set to false in version 3.0) | ^[boolean]                                                                                                                                                     | true                               |

| Name            | Description                                                           | Type                                                                                      |
| --------------- | --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| change          | triggers when user confirms the value or click outside                | ^[Function]`(val: typeof v-model) => void`                                                |
| blur            | triggers when Input blurs                                             | ^[Function]`(e: FocusEvent) => void`                                                      |
| focus           | triggers when Input focuses                                           | ^[Function]`(e: FocusEvent) => void`                                                      |
| clear ^(2.7.7)  | triggers when the clear icon is clicked in a clearable DatePicker     | ^[Function]`() => void`                                                                   |
| calendar-change | triggers when the calendar selected date is changed. Only for `range` | ^[Function]`(val: [Date, null \| Date]) => void`                                          |
| panel-change    | triggers when the navigation button click.                            | ^[Function]`(date: Date \| [Date, Date], mode: 'month' \| 'year', view?: string) => void` |
| visible-change  | triggers when the DatePicker's dropdown appears/disappears            | ^[Function]`(visibility: boolean) => void`                                                |

| Name                | Description                    |
| ------------------- | ------------------------------ |
| default             | custom cell content            |
| range-separator     | custom range separator content |
| prev-month ^(2.8.0) | prev month icon                |
| next-month ^(2.8.0) | next month icon                |
| prev-year ^(2.8.0)  | prev year icon                 |
| next-year ^(2.8.0)  | next year icon                 |

| Name                  | Description                    | Type                    |
| --------------------- | ------------------------------ | ----------------------- |
| focus                 | focus the DatePicker component | ^[Function]`() => void` |
| blur ^(2.8.7)         | blur the DatePicker component  | ^[Function]`() => void` |
| handleOpen ^(2.2.16)  | open the DatePicker popper     | ^[Function]`() => void` |
| handleClose ^(2.2.16) | close the DatePicker popper    | ^[Function]`() => void` |

<details>
  <summary>Show declarations</summary>

### custom-content.vue

### custom-prefix-icon.vue

### default-value.vue

### other-measurements.vue

---
Title: DateTimePicker
URL: https://element-plus.org/en-US/component/datetime-picker
---

**Examples:**

Example 1 (ts):
```ts
interface DateCell {
  column: number
  customClass: string | undefined
  disabled: boolean
  end: boolean
  inRange: boolean
  row: number
  selected: Dayjs | undefined
  isCurrent: boolean | undefined
  isSelected: boolean
  renderText: string | undefined
  start: boolean
  text: number
  timestamp: number
  date: Date
  dayjs: Dayjs
  type: 'normal' | 'today' | 'week' | 'next-month' | 'prev-month'
}
```

Example 2 (ts):
```ts
import type { Options as PopperOptions } from '@popperjs/core'

type TimeLikeType = 'datetime' | 'datetimerange'

type Placement =
  | 'top'
  | 'top-start'
  | 'top-end'
  | 'bottom'
  | 'bottom-start'
  | 'bottom-end'
  | 'left'
  | 'left-start'
  | 'left-end'
  | 'right'
  | 'right-start'
  | 'right-end'
```

Example 3 (vue):
```vue
<template>
  <div class="demo-date-picker">
    <el-date-picker
      v-model="value"
      type="date"
      placeholder="Pick a day"
      format="YYYY/MM/DD"
      value-format="YYYY-MM-DD"
    >
      <template #default="cell">
        <div class="cell" :class="{ current: cell.isCurrent }">
          <span class="text">{{ cell.text }}</span>
          <span v-if="isHoliday(cell)" class="holiday" />
        </div>
      </template>
    </el-date-picker>
    <el-date-picker v-model="month" type="month" placeholder="Pick a month">
      <template #default="cell">
        <div class="el-date-table-cell" :class="{ current: cell.isCurrent }">
          <span class="el-date-table-cell__text">{{ cell.text + 1 }}期</span>
        </div>
      </template>
    </el-date-picker>
    <el-date-picker v-model="year" type="year" placeholder="Pick a year">
      <template #default="cell">
        <div class="el-date-table-cell" :class="{ current: cell.isCurrent }">
          <span class="el-date-table-cell__text">{{ cell.text + 1 }}y</span>
        </div>
      </template>
    </el-date-picker>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref('2021-10-29')
const month = ref('')
const year = ref('')
const holidays = [
  '2021-10-01',
  '2021-10-02',
  '2021-10-03',
  '2021-10-04',
  '2021-10-05',
  '2021-10-06',
  '2021-10-07',
]

const isHoliday = ({ dayjs }) => {
  return holidays.includes(dayjs.format('YYYY-MM-DD'))
}
</script>

<style scoped>
.demo-date-picker {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.demo-date-picker > * {
  margin: 0 !important;
}

.cell {
  height: 30px;
  padding: 3px 0;
  box-sizing: border-box;
}

.cell .text {
  width: 24px;
  height: 24px;
  display: block;
  margin: 0 auto;
  line-height: 24px;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  border-radius: 50%;
}

.cell.current .text {
  background: #626aef;
  color: #fff;
}

.cell .holiday {
  position: absolute;
  width: 6px;
  height: 6px;
  background: var(--el-color-danger);
  border-radius: 50%;
  bottom: 0px;
  left: 50%;
  transform: translateX(-50%);
}

@media screen and (max-width: 768px) {
  .demo-date-picker {
    gap: 1.5rem;
  }
}
</style>
```

Example 4 (vue):
```vue
<template>
  <div class="demo-date-picker-icon">
    <div class="container">
      <div class="block">
        <div class="demonstration">date</div>
        <el-date-picker
          v-model="value1"
          type="date"
          placeholder="Pick a day"
          format="YYYY/MM/DD"
          value-format="YYYY-MM-DD"
        >
          <template #prev-month>
            <el-icon><CaretLeft /></el-icon>
          </template>
          <template #next-month>
            <el-icon><CaretRight /></el-icon>
          </template>
          <template #prev-year>
            <el-icon>
              <svg
                viewBox="0 0 20 20"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
              >
                <g stroke-width="1" fill-rule="evenodd">
                  <g fill="currentColor">
                    <path
                      d="M8.73171,16.7949 C9.03264,17.0795 9.50733,17.0663 9.79196,16.7654 C10.0766,16.4644 10.0634,15.9897 9.76243,15.7051 L4.52339,10.75 L17.2471,10.75 C17.6613,10.75 17.9971,10.4142 17.9971,10 C17.9971,9.58579 17.6613,9.25 17.2471,9.25 L4.52112,9.25 L9.76243,4.29275 C10.0634,4.00812 10.0766,3.53343 9.79196,3.2325 C9.50733,2.93156 9.03264,2.91834 8.73171,3.20297 L2.31449,9.27241 C2.14819,9.4297 2.04819,9.62981 2.01448,9.8386 C2.00308,9.89058 1.99707,9.94459 1.99707,10 C1.99707,10.0576 2.00356,10.1137 2.01585,10.1675 C2.05084,10.3733 2.15039,10.5702 2.31449,10.7254 L8.73171,16.7949 Z"
                    />
                  </g>
                </g>
              </svg>
            </el-icon>
          </template>
          <template #next-year>
            <el-icon>
              <svg
                viewBox="0 0 20 20"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
              >
                <g stroke-width="1" fill-rule="evenodd">
                  <g fill="currentColor">
                    <path
                      d="M11.2654,3.20511 C10.9644,2.92049 10.4897,2.93371 10.2051,3.23464 C9.92049,3.53558 9.93371,4.01027 10.2346,4.29489 L15.4737,9.25 L2.75,9.25 C2.33579,9.25 2,9.58579 2,10.0000012 C2,10.4142 2.33579,10.75 2.75,10.75 L15.476,10.75 L10.2346,15.7073 C9.93371,15.9919 9.92049,16.4666 10.2051,16.7675 C10.4897,17.0684 10.9644,17.0817 11.2654,16.797 L17.6826,10.7276 C17.8489,10.5703 17.9489,10.3702 17.9826,10.1614 C17.994,10.1094 18,10.0554 18,10.0000012 C18,9.94241 17.9935,9.88633 17.9812,9.83246 C17.9462,9.62667 17.8467,9.42976 17.6826,9.27455 L11.2654,3.20511 Z"
                    />
                  </g>
                </g>
              </svg>
            </el-icon>
          </template>
        </el-date-picker>
      </div>
      <div class="line" />
      <div class="block">
        <div class="demonstration">date range</div>
        <el-date-picker
          v-model="value2"
          type="daterange"
          start-placeholder="Start date"
          end-placeholder="End date"
          format="YYYY/MM/DD"
          value-format="YYYY-MM-DD"
          unlink-panels
        >
          <template #prev-month>
            <el-icon><CaretLeft /></el-icon>
          </template>
          <template #next-month>
            <el-icon><CaretRight /></el-icon>
          </template>
          <template #prev-year>
            <el-icon>
              <svg
                viewBox="0 0 20 20"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
              >
                <g stroke-width="1" fill-rule="evenodd">
                  <g fill="currentColor">
                    <path
                      d="M8.73171,16.7949 C9.03264,17.0795 9.50733,17.0663 9.79196,16.7654 C10.0766,16.4644 10.0634,15.9897 9.76243,15.7051 L4.52339,10.75 L17.2471,10.75 C17.6613,10.75 17.9971,10.4142 17.9971,10 C17.9971,9.58579 17.6613,9.25 17.2471,9.25 L4.52112,9.25 L9.76243,4.29275 C10.0634,4.00812 10.0766,3.53343 9.79196,3.2325 C9.50733,2.93156 9.03264,2.91834 8.73171,3.20297 L2.31449,9.27241 C2.14819,9.4297 2.04819,9.62981 2.01448,9.8386 C2.00308,9.89058 1.99707,9.94459 1.99707,10 C1.99707,10.0576 2.00356,10.1137 2.01585,10.1675 C2.05084,10.3733 2.15039,10.5702 2.31449,10.7254 L8.73171,16.7949 Z"
                    />
                  </g>
                </g>
              </svg>
            </el-icon>
          </template>
          <template #next-year>
            <el-icon>
              <svg
                viewBox="0 0 20 20"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
              >
                <g stroke-width="1" fill-rule="evenodd">
                  <g fill="currentColor">
                    <path
                      d="M11.2654,3.20511 C10.9644,2.92049 10.4897,2.93371 10.2051,3.23464 C9.92049,3.53558 9.93371,4.01027 10.2346,4.29489 L15.4737,9.25 L2.75,9.25 C2.33579,9.25 2,9.58579 2,10.0000012 C2,10.4142 2.33579,10.75 2.75,10.75 L15.476,10.75 L10.2346,15.7073 C9.93371,15.9919 9.92049,16.4666 10.2051,16.7675 C10.4897,17.0684 10.9644,17.0817 11.2654,16.797 L17.6826,10.7276 C17.8489,10.5703 17.9489,10.3702 17.9826,10.1614 C17.994,10.1094 18,10.0554 18,10.0000012 C18,9.94241 17.9935,9.88633 17.9812,9.83246 C17.9462,9.62667 17.8467,9.42976 17.6826,9.27455 L11.2654,3.20511 Z"
                    />
                  </g>
                </g>
              </svg>
            </el-icon>
          </template>
        </el-date-picker>
      </div>
    </div>
    <div class="container">
      <div class="line" />
      <div class="block">
        <div class="demonstration">month range</div>
        <el-date-picker
          v-model="value3"
          type="monthrange"
          start-placeholder="Start date"
          end-placeholder="End date"
          format="YYYY/MM/DD"
          value-format="YYYY-MM-DD"
          unlink-panels
        >
          <template #prev-month>
            <el-icon><CaretLeft /></el-icon>
          </template>
          <template #next-month>
            <el-icon><CaretRight /></el-icon>
          </template>
          <template #prev-year>
            <el-icon>
              <svg
                viewBox="0 0 20 20"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
              >
                <g stroke-width="1" fill-rule="evenodd">
                  <g fill="currentColor">
                    <path
                      d="M8.73171,16.7949 C9.03264,17.0795 9.50733,17.0663 9.79196,16.7654 C10.0766,16.4644 10.0634,15.9897 9.76243,15.7051 L4.52339,10.75 L17.2471,10.75 C17.6613,10.75 17.9971,10.4142 17.9971,10 C17.9971,9.58579 17.6613,9.25 17.2471,9.25 L4.52112,9.25 L9.76243,4.29275 C10.0634,4.00812 10.0766,3.53343 9.79196,3.2325 C9.50733,2.93156 9.03264,2.91834 8.73171,3.20297 L2.31449,9.27241 C2.14819,9.4297 2.04819,9.62981 2.01448,9.8386 C2.00308,9.89058 1.99707,9.94459 1.99707,10 C1.99707,10.0576 2.00356,10.1137 2.01585,10.1675 C2.05084,10.3733 2.15039,10.5702 2.31449,10.7254 L8.73171,16.7949 Z"
                    />
                  </g>
                </g>
              </svg>
            </el-icon>
          </template>
          <template #next-year>
            <el-icon>
              <svg
                viewBox="0 0 20 20"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
              >
                <g stroke-width="1" fill-rule="evenodd">
                  <g fill="currentColor">
                    <path
                      d="M11.2654,3.20511 C10.9644,2.92049 10.4897,2.93371 10.2051,3.23464 C9.92049,3.53558 9.93371,4.01027 10.2346,4.29489 L15.4737,9.25 L2.75,9.25 C2.33579,9.25 2,9.58579 2,10.0000012 C2,10.4142 2.33579,10.75 2.75,10.75 L15.476,10.75 L10.2346,15.7073 C9.93371,15.9919 9.92049,16.4666 10.2051,16.7675 C10.4897,17.0684 10.9644,17.0817 11.2654,16.797 L17.6826,10.7276 C17.8489,10.5703 17.9489,10.3702 17.9826,10.1614 C17.994,10.1094 18,10.0554 18,10.0000012 C18,9.94241 17.9935,9.88633 17.9812,9.83246 C17.9462,9.62667 17.8467,9.42976 17.6826,9.27455 L11.2654,3.20511 Z"
                    />
                  </g>
                </g>
              </svg>
            </el-icon>
          </template>
        </el-date-picker>
      </div>
      <div class="line" />
      <div class="block">
        <div class="demonstration">year range</div>
        <el-date-picker
          v-model="value4"
          type="yearrange"
          range-separator="To"
          start-placeholder="Start Year"
          end-placeholder="End Year"
        >
          <template #prev-year>
            <el-icon>
              <svg
                viewBox="0 0 20 20"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
              >
                <g stroke-width="1" fill-rule="evenodd">
                  <g fill="currentColor">
                    <path
                      d="M8.73171,16.7949 C9.03264,17.0795 9.50733,17.0663 9.79196,16.7654 C10.0766,16.4644 10.0634,15.9897 9.76243,15.7051 L4.52339,10.75 L17.2471,10.75 C17.6613,10.75 17.9971,10.4142 17.9971,10 C17.9971,9.58579 17.6613,9.25 17.2471,9.25 L4.52112,9.25 L9.76243,4.29275 C10.0634,4.00812 10.0766,3.53343 9.79196,3.2325 C9.50733,2.93156 9.03264,2.91834 8.73171,3.20297 L2.31449,9.27241 C2.14819,9.4297 2.04819,9.62981 2.01448,9.8386 C2.00308,9.89058 1.99707,9.94459 1.99707,10 C1.99707,10.0576 2.00356,10.1137 2.01585,10.1675 C2.05084,10.3733 2.15039,10.5702 2.31449,10.7254 L8.73171,16.7949 Z"
                    />
                  </g>
                </g>
              </svg>
            </el-icon>
          </template>
          <template #next-year>
            <el-icon>
              <svg
                viewBox="0 0 20 20"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
              >
                <g stroke-width="1" fill-rule="evenodd">
                  <g fill="currentColor">
                    <path
                      d="M11.2654,3.20511 C10.9644,2.92049 10.4897,2.93371 10.2051,3.23464 C9.92049,3.53558 9.93371,4.01027 10.2346,4.29489 L15.4737,9.25 L2.75,9.25 C2.33579,9.25 2,9.58579 2,10.0000012 C2,10.4142 2.33579,10.75 2.75,10.75 L15.476,10.75 L10.2346,15.7073 C9.93371,15.9919 9.92049,16.4666 10.2051,16.7675 C10.4897,17.0684 10.9644,17.0817 11.2654,16.797 L17.6826,10.7276 C17.8489,10.5703 17.9489,10.3702 17.9826,10.1614 C17.994,10.1094 18,10.0554 18,10.0000012 C18,9.94241 17.9935,9.88633 17.9812,9.83246 C17.9462,9.62667 17.8467,9.42976 17.6826,9.27455 L11.2654,3.20511 Z"
                    />
                  </g>
                </g>
              </svg>
            </el-icon>
          </template>
        </el-date-picker>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { CaretLeft, CaretRight } from '@element-plus/icons-vue'

const value1 = ref('')
const value2 = ref('')
const value3 = ref('')
const value4 = ref('')
</script>

<style scoped>
.demo-date-picker-icon {
  display: flex;
  width: 100%;
  padding: 0;
  flex-wrap: wrap;
  gap: 1px;
}

.demo-date-picker-icon .container {
  flex: 1;
  min-width: 400px;
  border-right: solid 1px var(--el-border-color);
  display: flex;
  flex-direction: column;
}

.demo-date-picker-icon .container:last-child {
  border-right: none;
}

.demo-date-picker-icon .block {
  padding: 1.5rem 0;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.demo-date-picker-icon .block:not(:first-child) {
  border-top: solid 1px var(--el-border-color);
}

.demo-date-picker-icon .demonstration {
  display: block;
  color: var(--el-text-color-secondary);
  font-size: 14px;
  margin-bottom: 1rem;
  width: 100%;
}

@media screen and (max-width: 1200px) {
  .demo-date-picker-icon {
    gap: 0;
  }

  .demo-date-picker-icon .container {
    flex: 0 0 100%;
    min-width: auto;
    border-right: none;
  }

  .demo-date-picker-icon .container:not(:last-child) {
    border-bottom: solid 1px var(--el-border-color);
  }

  .demo-date-picker-icon .block {
    padding: 1rem 0;
  }

  .demo-date-picker-icon .block:not(:first-child) {
    position: relative;
    margin-top: -1px;
  }
}
</style>
```

---

## Migration

**URL:** llms-txt#migration

**Contents:**
- Vue 3 migration build
- Migration Tool :hammer_and_wrench:
- Custom namespace ^(2.2.0)
  - Set `ElConfigProvider`
  - Set Scss & Css Vars

[This guide](https://github.com/element-plus/element-plus/discussions/5658) will help you upgrade from Element 2.x to Element Plus.

## Vue 3 migration build

You may encounter some issues when using Element Plus with Vue 3 migration build. Some of the components rely on Vue 3 internal API's. It's worth trying to adjust compatConfig mode to 3, either globally or [per component in your project](https://v3-migration.vuejs.org/migration-build.html).

## Migration Tool :hammer_and_wrench:

We have made a migration tool for you to migrate your project from [Element UI](https://element.eleme.io) to Element Plus.
You can find the [gogo code migration tool](https://github.com/thx/gogocode/tree/main/packages/gogocode-plugin-element) here.

We have tested this on [Vue Element Admin](https://github.com/PanJiaChen/vue-element-admin) (Vue 2 + Element UI). You can find the transpiled code [here](https://github.com/gogocodeio/vue-element-admin).

<style scoped>
  details {
    margin-top: 8px;
  }
</style>

---
Title: Custom namespace
URL: https://element-plus.org/en-US/guide/namespace
---

## Custom namespace ^(2.2.0)

Default namespace is `el`.
In special cases, we may need to customize its namespace.

Since we use sass to write styles, if you want to customize all namespaces.
We have to assume that you already use sass.

You must set `ElConfigProvider` and scss `$namespace` at the same time.

### Set `ElConfigProvider`

Use `ElConfigProvider` wrap your root component.

### Set Scss & Css Vars

Create `styles/element/index.scss`:

Import `styles/element/index.scss` in `vite.config.ts`:

> The same is true for webpack, which needs to be set in `preprocessorOptions`.

---
Title: Navigation
URL: https://element-plus.org/en-US/guide/nav
---

<style>
:root {
  --categories-c-bg: #F9FAFC;
  --categories-c-page: #E5E9F2;
  --categories-c-overlay: white;
  --categories-c-text: #99A9BF;
  --categories-c-icon: #E5E9F2;
  --categories-c-line: #E5E9F2;
}

.dark {
  --categories-c-bg: #1D1E1F;
  --categories-c-page: #0A0A0A;
  --categories-c-overlay: #141414;
  --categories-c-text: #53637A;
  --categories-c-icon: #2F333D;
  --categories-c-line: #242529;
}
</style>

**Examples:**

Example 1 (unknown):
```unknown
### Set Scss & Css Vars

Create `styles/element/index.scss`:
```

Example 2 (unknown):
```unknown
Import `styles/element/index.scss` in `vite.config.ts`:

> The same is true for webpack, which needs to be set in `preprocessorOptions`.
```

---

## |<---- Using a Maximum Of 72 Characters ---->|

**URL:** llms-txt#|<-----using-a-maximum-of-72-characters----->|

---

## --- COMMIT END ---

**URL:** llms-txt#----commit-end----

---

## Commits that change 30 or more lines across at least 3 files should

**URL:** llms-txt#commits-that-change-30-or-more-lines-across-at-least-3-files-should

---

## Server-Side Rendering (SSR)

**URL:** llms-txt#server-side-rendering-(ssr)

**Contents:**
- Provide an ID
- Provide ZIndex
- Teleports
  - Render the Teleport on the mount
  - Inject the teleport markup

When using Element Plus for SSR development, you need to carry out special handling during SSR to avoid hydrate errors.

The provided value is used to generate the unique ID in Element Plus.
Because the different IDs are prone to hydrate errors in SSR, in order to ensure that the server side and client side generate the same ID, we need to inject the `ID_injection_key` into Vue.

When you using SSR for development, you may encounter hydration errors caused by `z-index`. In this case, we recommend injecting an initial value to avoid such errors.

[Teleport](https://vuejs.org/guide/scaling-up/ssr.html#teleports) is used internally by multiple components in Element Plus (eg. ElDialog, ElDrawer, ElTooltip, ElDropdown, ElSelect, ElDatePicker ...), so special handling is required during SSR.

### Render the Teleport on the mount

An easier solution is to conditionally render the Teleport on the mount.

For example, use the `ClientOnly` component in Nuxt.

### Inject the teleport markup

Another way is to inject the teleport markup into the correct location in your final page HTML.

You need to inject the teleport markup close to the `<body>` tag.

---
Title: Custom theme
URL: https://element-plus.org/en-US/guide/theming
---

**Examples:**

Example 1 (unknown):
```unknown
## Provide ZIndex

When you using SSR for development, you may encounter hydration errors caused by `z-index`. In this case, we recommend injecting an initial value to avoid such errors.
```

Example 2 (unknown):
```unknown
## Teleports

[Teleport](https://vuejs.org/guide/scaling-up/ssr.html#teleports) is used internally by multiple components in Element Plus (eg. ElDialog, ElDrawer, ElTooltip, ElDropdown, ElSelect, ElDatePicker ...), so special handling is required during SSR.

### Render the Teleport on the mount

An easier solution is to conditionally render the Teleport on the mount.

For example, use the `ClientOnly` component in Nuxt.
```

Example 3 (unknown):
```unknown
or
```

Example 4 (unknown):
```unknown
### Inject the teleport markup

Another way is to inject the teleport markup into the correct location in your final page HTML.

You need to inject the teleport markup close to the `<body>` tag.
```

---

## Local Development

**URL:** llms-txt#local-development

**Contents:**
- Bootstrap project
- Website preview
- Local development
- The following commands are also useful during development
  - Generate component template

the project will install all dependencies.

the project will launch website for you to preview all existing component.

See [Local development guide](https://github.com/element-plus/element-plus/blob/dev/CONTRIBUTING.md)

will start the local development environment.

2. Add your component into `play/src/App.vue`

Modify `App.vue` file per your needs to get things work.

## The following commands are also useful during development

### Generate component template

```shell
pnpm gen <component-name>

**Examples:**

Example 1 (shell):
```shell
pnpm i
```

Example 2 (shell):
```shell
pnpm docs:dev
```

Example 3 (shell):
```shell
pnpm dev
```

Example 4 (unknown):
```unknown
Modify `App.vue` file per your needs to get things work.

## The following commands are also useful during development

### Generate component template

With command
```

---

## Internationalization

**URL:** llms-txt#internationalization

**Contents:**
- Global configuration
- ConfigProvider
- Date and time localization
- CDN Usage

Element Plus components use English **by default**. If you want to use other
languages, read on to find out how.

## Global configuration

Element Plus provides global configuration options.

Element Plus also provides a Vue component [ConfigProvider](/en-US/component/config-provider)
for globally configuring locale and other settings.

## Date and time localization

We use [Day.js](https://day.js.org/docs/en/i18n/i18n) library to manage date and time in components like `DatePicker`. It is important to set a proper locale in Day.js to make internationalization work properly. You have to import Day.js's locale config separately.

If you are using Element Plus via CDN, you need to do the following. Let's take
unpkg as an example:

For full documentation, refer to: [ConfigProvider](/en-US/component/config-provider)

[Supported Language List](https://github.com/element-plus/element-plus/tree/dev/packages/locale/lang)

<ul class="language-list">
  <li>Simplified Chinese (zh-cn)</li>
  <li>American English (en)</li>
  <li>Azerbaijani (az)</li>
  <li>German (de)</li>
  <li>Portuguese (pt)</li>
  <li>Spanish (es)</li>
  <li>Danish (da)</li>
  <li>French (fr)</li>
  <li>Norwegian (nb-NO)</li>
  <li>Traditional Chinese (zh-tw)</li>
  <li>Italian (it)</li>
  <li>Korean (ko)</li>
  <li>Japanese (ja)</li>
  <li>Dutch (nl)</li>
  <li>Vietnamese (vi)</li>
  <li>Russian (ru)</li>
  <li>Turkish (tr)</li>
  <li>Brazilian Portuguese (pt-br)</li>
  <li>Farsi (fa)</li>
  <li>Thai (th)</li>
  <li>Indonesian (id)</li>
  <li>Bulgarian (bg)</li>
  <li>Pashto (pa)</li>
  <li>Polish (pl)</li>
  <li>Finnish (fi)</li>
  <li>Swedish (sv)</li>
  <li>Greek (el)</li>
  <li>Slovak (sk)</li>
  <li>Catalunya (ca)</li>
  <li>Czech (cs)</li>
  <li>Ukrainian (uk)</li>
  <li>Turkmen (tk)</li>
  <li>Tamil (ta)</li>
  <li>Latvian (lv)</li>
  <li>Afrikaans (af)</li>
  <li>Estonian (et)</li>
  <li>Slovenian (sl)</li>
  <li>Arabic (ar)</li>
  <li>Hebrew (he)</li>
  <li>Lao (lo)</li>
  <li>Lithuanian (lt)</li>
  <li>Mongolian (mn)</li>
  <li>Kazakh (kk)</li>
  <li>Hungarian (hu)</li>
  <li>Romanian (ro)</li>
  <li>Kurdish (ku)</li>
  <li>Kurdish (ckb)</li>
  <li>Uighur (ug-cn)</li>
  <li>Khmer (km)</li>
  <li>Serbian (sr)</li>
  <li>Basque (eu)</li>
  <li>Kyrgyz (ky)</li>
  <li>Armenian (hy-am)</li>
  <li>Croatian (hr)</li>
  <li>Esperanto (eo)</li>
  <li>Bengali (bn)</li>
  <li>Malay (ms)</li>
  <li>Madagascar (mg)</li>
  <li>Swahili (sw)</li>
  <li>Uzbek (uz-uz)</li>
  <li>Egyptian Arabic (ar-eg)</li>
  <li>Burmese (my)</li>
  <li>Hindi (hi)</li>
  <li>Norsk (no)</li>
  <li>Hongkong Chinese (zh-hk)</li>
  <li>Macau Chinese (zh-mo)</li>
  <li>Telugu (te)</li>
</ul>

If you need any other languages, [PR](https://github.com/element-plus/element-plus/pulls)
is always welcome, you only need to add a language file [here](https://github.com/element-plus/element-plus/tree/dev/packages/locale/lang).

<style>
  .language-list {
    list-style: disc
  }
</style>

---
Title: Installation
URL: https://element-plus.org/en-US/guide/installation
---

**Examples:**

Example 1 (unknown):
```unknown
## ConfigProvider

Element Plus also provides a Vue component [ConfigProvider](/en-US/component/config-provider)
for globally configuring locale and other settings.
```

Example 2 (unknown):
```unknown
## Date and time localization

We use [Day.js](https://day.js.org/docs/en/i18n/i18n) library to manage date and time in components like `DatePicker`. It is important to set a proper locale in Day.js to make internationalization work properly. You have to import Day.js's locale config separately.
```

Example 3 (unknown):
```unknown
## CDN Usage

If you are using Element Plus via CDN, you need to do the following. Let's take
unpkg as an example:
```

---

## Do not end the subject line with a period

**URL:** llms-txt#do-not-end-the-subject-line-with-a-period

---

## get dist

**URL:** llms-txt#get-dist

pnpm build
cd dist/element-plus

---

## DatePickerPanel ^(beta)

**URL:** llms-txt#datepickerpanel-^(beta)

**Contents:**
- Enter Date
- Border
- Disabled
- Types
- Localization
- API
  - Attributes
  - Events
  - Slots
- Vue Examples

`DatePickerPanel` is the core component of `DatePicker`.

Basic date picker measured by 'day'.

By default the date-picker-panel is bordered but in some case you don't want it.
For example `DatePicker` don't inherit `border`.

The `disabled` attribute determines if the date picker is fully disabled.

The measurement is determined by the `type` attribute.

The default locale of is English, if you need to use other languages, please check [Internationalization](/en-US/guide/i18n)

Note, date time locale (month name, first day of the week ...) are also configured in localization.

| Name                  | Description                                                                                                                  | Type                                                                                                                                                           | Default    |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| model-value / v-model | binding value, if it is an `range` picker, the length of the array should be 2                                               | ^[number] / ^[string] / ^[Date] / ^[array]`number[] \| string[] \| Date[]`                                                                                     | ''         |
| border                | whether the date picker is bordered                                                                                          | ^[boolean]                                                                                                                                                     | true       |
| disabled              | whether DatePicker is disabled                                                                                               | ^[boolean]                                                                                                                                                     | false      |
| clearable             | whether to show clear button                                                                                                 | ^[boolean]                                                                                                                                                     | true       |
| editable ^(2.13.0)    | whether the input is editable                                                                                                | ^[boolean]                                                                                                                                                     | true       |
| type                  | type of the picker                                                                                                           | ^[enum]`'year' \| 'years' \|'month' \| 'months' \| 'date' \| 'dates' \| 'datetime' \| 'week' \| 'datetimerange' \| 'daterange' \| 'monthrange' \| 'yearrange'` | date       |
| default-value         | optional, default date of the calendar                                                                                       | ^[object]`Date \| [Date, Date]`                                                                                                                                | —          |
| default-time          | optional, the time value to use when selecting date range                                                                    | ^[object]`Date \| [Date, Date]`                                                                                                                                | —          |
| value-format          | optional, format of binding value. If not specified, the binding value will be a Date object                                 | ^[string]                                                                                                                                                      | —          |
| date-format           | optional, format of the date displayed in input's inner panel                                                                | ^[string] see [date formats](https://day.js.org/docs/en/display/format)                                                                                        | YYYY-MM-DD |
| time-format           | optional, format of the time displayed in input's inner panel                                                                | ^[string] see [date formats](https://day.js.org/docs/en/display/format)                                                                                        | HH:mm:ss   |
| unlink-panels         | unlink two date-panels in range-picker                                                                                       | ^[boolean]                                                                                                                                                     | false      |
| disabled-date         | a function determining if a date is disabled with that date as its parameter. Should return a Boolean                        | ^[Function]`(data: Date) => boolean`                                                                                                                           | —          |
| shortcuts             | an object array to set shortcut options                                                                                      | ^[object]`Array<{ text: string, value: Date \| Function }>`                                                                                                    | []         |
| cell-class-name       | set custom className                                                                                                         | ^[Function]`(data: Date) => string`                                                                                                                            | —          |
| show-footer           | whether to show footer where the date picker is one ^[enum]`'dates' \| 'months' \| 'years' \| 'datetime' \| 'datetimerange'` | ^[boolean]                                                                                                                                                     | false      |
| show-confirm          | whether to show the confirm button                                                                                           | ^[boolean]                                                                                                                                                     | false      |
| show-week-number      | show the week number besides the week                                                                                        | ^[boolean]                                                                                                                                                     | false      |

| Name            | Description                                                           | Type                                                                                      |
| --------------- | --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| calendar-change | triggers when the calendar selected date is changed. Only for `range` | ^[Function]`(val: [Date, null \| Date]) => void`                                          |
| panel-change    | triggers when the navigation button click.                            | ^[Function]`(date: Date \| [Date, Date], mode: 'month' \| 'year', view?: string) => void` |

| Name       | Description         |
| ---------- | ------------------- |
| default    | custom cell content |
| prev-month | prev month icon     |
| next-month | next month icon     |
| prev-year  | prev year icon      |
| next-year  | next year icon      |

---
Title: DatePicker
URL: https://element-plus.org/en-US/component/date-picker
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <div>
    <div class="flex gap-4">
      <div class="flex flex-col basis-150px gap-1">
        <span>Type:</span>
        <el-select v-model="type">
          <el-option
            v-for="optionType in types"
            :key="optionType"
            :value="optionType"
          />
        </el-select>
      </div>
    </div>
    <el-divider />
    <div class="flex justify-center">
      <el-date-picker-panel v-model="date" :type="type" />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, watch } from 'vue'

import type { DatePickerType } from 'element-plus'

const date = ref()
const type = ref<DatePickerType>('date')

watch(type, () => {
  date.value = undefined
})

const types: DatePickerType[] = [
  'year',
  'years',
  'month',
  'months',
  'date',
  'dates',
  'week',
  'datetime',
  'datetimerange',
  'daterange',
  'monthrange',
  'yearrange',
]
</script>
```

Example 2 (vue):
```vue
<template>
  <div class="flex justify-center">
    <el-date-picker-panel v-model="value" />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref()
</script>
```

Example 3 (vue):
```vue
<template>
  <div
    ref="containerRef"
    :class="['date-picker--example', { 'is-narrow': isNarrow }]"
  >
    <div class="text-center">No border:</div>
    <el-divider />
    <div class="date-picker--flex-container">
      <div class="p-[20px]">
        <el-date-picker-panel v-model="value" :border="false" />
      </div>
      <el-divider
        class="divider"
        :direction="isNarrow ? 'horizontal' : 'vertical'"
      />
      <el-card>
        <el-date-picker-panel v-model="value" :border="false" />
      </el-card>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { computed, ref } from 'vue'
import { useElementSize } from '@vueuse/core'

const value = ref()
const containerRef = ref<HTMLElement>()

const { width } = useElementSize(containerRef)

const isNarrow = computed(() => width.value < 815)
</script>

<style scoped>
.date-picker--flex-container {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
  justify-content: center;
}
.divider {
  height: auto;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <div class="flex flex-col items-center">
    <el-switch
      v-model="disabled"
      active-text="Disabled"
      inactive-text="Enabled"
    />
    <el-date-picker-panel v-model="value" :disabled="disabled" />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref()
const disabled = ref(true)
</script>
```

---

## Capitalize the subject line

**URL:** llms-txt#capitalize-the-subject-line

---

## (If applied, this commit will...) <subject> (Max 72 characters)

**URL:** llms-txt#(if-applied,-this-commit-will...)-<subject>-(max-72-characters)

---

## describe these changes in the commit body

**URL:** llms-txt#describe-these-changes-in-the-commit-body

---

## Splitter ^(beta)

**URL:** llms-txt#splitter-^(beta)

**Contents:**
- Basic usage
- Vertical
- Collapsible
- Disable drag
- Panel size
- Lazy ^(2.11.0)
- Splitter API
  - Splitter Attributes
  - Splitter Events
- SplitterPanel API

Divide the area horizontally or vertically, and freely drag to adjust the size of each area.

The most basic usage, if no default size is passed, it will be automatically divided equally.

Use vertical orientation.

Configuring `collapsible` provides quick shrinking capability. You can use the `min` property to prevent expanding through dragging after collapsing.

When either panel disables `resizable`, dragging will be disabled.

`v-model:size` can get the panel size.

When `lazy` is enabled, the panel size will not update in real time during dragging, but only after the drag ends.

### Splitter Attributes

| Name           | Description                      | Type                                | Default    |
| -------------- | -------------------------------- | ----------------------------------- | ---------- |
| layout         | Layout direction of the splitter | ^[enum]`'horizontal' \| 'vertical'` | horizontal |
| lazy ^(2.11.0) | Whether to enable lazy mode      | ^[boolean]                          | false      |

| Name               | Description                                                              | type                                                                          |
| ------------------ | ------------------------------------------------------------------------ | ----------------------------------------------------------------------------- |
| resize-start       | Triggered when starting to resize a panel, `index` is the drag bar index | ^[Function]`(index: number, sizes: number[]) => void`                         |
| resize             | Triggered while resizing a panel, `index` is the drag bar index          | ^[Function]`(index: number, sizes: number[]) => void`                         |
| resize-end         | Triggered when panel resizing ends, `index` is the drag bar index        | ^[Function]`(index: number, sizes: number[]) => void`                         |
| collapse ^(2.10.3) | Triggered when a panel is collapsed, `index` is the drag bar index       | ^[Function]`(index: number, type: 'start' \| 'end', sizes: number[]) => void` |

### SplitterPanel Attributes

| Name                | Description                                         | Type                  | Default |
| ------------------- | --------------------------------------------------- | --------------------- | ------- |
| size / v-model:size | Size of the panel (in pixels or percentage)         | ^[string] / ^[number] | -       |
| min                 | Minimum size of the panel (in pixels or percentage) | ^[string] / ^[number] | -       |
| max                 | Maximum size of the panel (in pixels or percentage) | ^[string] / ^[number] | -       |
| resizable           | Whether the panel can be resized                    | ^[boolean]            | true    |
| collapsible         | Whether the panel can be collapsed                  | ^[boolean]            | false   |

### SplitterPanel Events

| Name        | Description                       | type                                |
| ----------- | --------------------------------- | ----------------------------------- |
| update:size | Triggered when panel size changes | ^[Function]`(size: number) => void` |

### SplitterPanel Slots

| Name              | Description                                     |
| ----------------- | ----------------------------------------------- |
| default           | Default content of the panel                    |
| start-collapsible | Custom content for the start collapsible button |
| end-collapsible   | Custom content for the end collapsible button   |

### SplitterPanel Exposes

| Name                       | Description                 | Type                           |
| -------------------------- | --------------------------- | ------------------------------ |
| splitterPanelRef ^(2.11.9) | splitter-panel html element | ^[object]`Ref<HTMLDivElement>` |

---
Title: Statistic
URL: https://element-plus.org/en-US/component/statistic
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <div
    style="height: 250px; box-shadow: var(--el-border-color-light) 0px 0px 10px"
  >
    <el-splitter>
      <el-splitter-panel size="30%">
        <div class="demo-panel">1</div>
      </el-splitter-panel>
      <el-splitter-panel :min="200">
        <div class="demo-panel">2</div>
      </el-splitter-panel>
    </el-splitter>
  </div>
</template>

<style scoped>
.demo-panel {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}
</style>
```

Example 2 (vue):
```vue
<template>
  <el-switch
    v-model="isCollapsible"
    active-text="enable"
    inactive-text="disable"
    inline-prompt
    class="mb-2"
  />
  <div
    style="height: 250px; box-shadow: var(--el-border-color-light) 0px 0px 10px"
  >
    <el-splitter>
      <el-splitter-panel :collapsible="isCollapsible" min="50">
        <div class="demo-panel">1</div>
      </el-splitter-panel>
      <el-splitter-panel :collapsible="isCollapsible">
        <div class="demo-panel">2</div>
      </el-splitter-panel>
      <el-splitter-panel>
        <div class="demo-panel">3</div>
      </el-splitter-panel>
      <el-splitter-panel :collapsible="isCollapsible">
        <el-splitter layout="vertical">
          <el-splitter-panel :collapsible="isCollapsible">
            <div class="demo-panel">4</div>
          </el-splitter-panel>
          <el-splitter-panel :collapsible="isCollapsible">
            <div class="demo-panel">5</div>
          </el-splitter-panel>
        </el-splitter>
      </el-splitter-panel>
    </el-splitter>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const isCollapsible = ref(true)
</script>

<style scoped>
.demo-panel {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <el-switch
    v-model="resizable"
    active-text="enable"
    inactive-text="disable"
    inline-prompt
    class="mb-2"
  />
  <div
    style="height: 250px; box-shadow: var(--el-border-color-light) 0px 0px 10px"
  >
    <el-splitter>
      <el-splitter-panel>
        <div class="demo-panel">1</div>
      </el-splitter-panel>
      <el-splitter-panel :resizable="resizable">
        <div class="demo-panel">
          drag {{ resizable ? 'enable' : 'disable' }}
        </div>
      </el-splitter-panel>
      <el-splitter-panel>
        <div class="demo-panel">3</div>
      </el-splitter-panel>
    </el-splitter>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const resizable = ref(false)
</script>

<style scoped>
.demo-panel {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <div
    style="height: 250px; box-shadow: var(--el-border-color-light) 0px 0px 10px"
  >
    <el-splitter lazy>
      <el-splitter-panel collapsible min="50">
        <div class="demo-panel">1</div>
      </el-splitter-panel>
      <el-splitter-panel collapsible>
        <div class="demo-panel">2</div>
      </el-splitter-panel>
      <el-splitter-panel collapsible>
        <div class="demo-panel">3</div>
      </el-splitter-panel>
    </el-splitter>
  </div>
</template>

<style scoped>
.demo-panel {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}
</style>
```

---

## Explain why this change is being made

**URL:** llms-txt#explain-why-this-change-is-being-made

---

## set cur element-plus to global `node_modules`

**URL:** llms-txt#set-cur-element-plus-to-global-`node_modules`

---

## Built-in Transition

**URL:** llms-txt#built-in-transition

**Contents:**
- Fade
- Zoom
- Collapse
- On-demand import
- Vue Examples
  - collapse.vue
  - fade.vue
  - zoom.vue

You can use Element's built-in transitions directly.
Before that, please read the [transition docs](https://vuejs.org/guide/built-ins/transition.html).

For collapse effect, use the `el-collapse-transition` component.

---
Title: Translation
URL: https://element-plus.org/en-US/guide/translation
---

**Examples:**

Example 1 (unknown):
```unknown
## Vue Examples

### collapse.vue
```

Example 2 (unknown):
```unknown
### fade.vue
```

Example 3 (unknown):
```unknown
### zoom.vue
```

---

## Do not use Emojis

**URL:** llms-txt#do-not-use-emojis

---

## ---

**URL:** llms-txt#---

Title: Changelog
URL: https://element-plus.org/en-US/guide/changelog
---

<style scoped lang="scss">
@at-root .hero-content {
  padding: 32px;
}
</style>

---

## Dark Mode ^(2.2.0)

**URL:** llms-txt#dark-mode-^(2.2.0)

**Contents:**
- How to enable it?
- Custom variables
  - By CSS
  - By SCSS

After a long time, Element Plus supports dark mode!

We extracted and unified all necessary variables to make it possible to implement based on CSS Vars.

First you can create a switch to toggle `dark` class of html.

> If you only need dark mode, just add `dark` class for html.

> If you want to toggle it, i recommend [useDark | VueUse](https://vueuse.org/core/useDark/).

Then, you can quickly enable it with just one line of code to import CSS in your entry.

> If you want an example, you can refer to [element-plus-vite-starter](https://github.com/element-plus/element-plus-vite-starter).

Just override it by CSS Vars.

For example, new file `styles/dark/css-vars.css`:

Import it after styles of Element Plus:

If you use scss, you can also import scss file to compile.

> You can refer [Theming](./theming.md) to get more info.

---
Title: Design Disciplines
URL: https://element-plus.org/en-US/guide/design
---

**Examples:**

Example 1 (html):
```html
<html class="dark">
  <head></head>
  <body></body>
</html>
```

Example 2 (unknown):
```unknown
> If you want an example, you can refer to [element-plus-vite-starter](https://github.com/element-plus/element-plus-vite-starter).

## Custom variables

### By CSS

Just override it by CSS Vars.

For example, new file `styles/dark/css-vars.css`:
```

Example 3 (unknown):
```unknown
Import it after styles of Element Plus:
```

Example 4 (unknown):
```unknown
### By SCSS

If you use scss, you can also import scss file to compile.

> You can refer [Theming](./theming.md) to get more info.
```

---

## TimePicker

**URL:** llms-txt#timepicker

**Contents:**
- Arbitrary time picker
- Limit the time range
- Arbitrary time range
- API
  - Attributes
  - Events
  - Exposes
- Type Declarations
- Vue Examples
  - basic-range.vue

Use Time Picker for time input.

## Arbitrary time picker

Can pick an arbitrary time.

## Limit the time range

You can also limit the time range.

## Arbitrary time range

Can pick an arbitrary time range.

| Name                         | Description                                                                                                          | Type                                                                                            | Default                            |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | ---------------------------------- |
| model-value / v-model        | binding value, if it is an array, the length should be 2                                                             | ^[number] / ^[string] / ^[object]`Date \| [Date, Date] \| [number, number] \| [string, string]` | ''                                 |
| readonly                     | whether TimePicker is read only                                                                                      | ^[boolean]                                                                                      | false                              |
| disabled                     | whether TimePicker is disabled                                                                                       | ^[boolean]                                                                                      | false                              |
| editable                     | whether the input is editable                                                                                        | ^[boolean]                                                                                      | true                               |
| clearable                    | whether to show clear button                                                                                         | ^[boolean]                                                                                      | true                               |
| size                         | size of Input                                                                                                        | ^[enum]`'large' \| 'default' \| 'small'`                                                        | —                                  |
| placeholder                  | placeholder in non-range mode                                                                                        | ^[string]                                                                                       | ''                                 |
| start-placeholder            | placeholder for the start time in range mode                                                                         | ^[string]                                                                                       | —                                  |
| end-placeholder              | placeholder for the end time in range mode                                                                           | ^[string]                                                                                       | —                                  |
| is-range                     | whether to pick a time range                                                                                         | ^[boolean]                                                                                      | false                              |
| arrow-control                | whether to pick time using arrow buttons                                                                             | ^[boolean]                                                                                      | false                              |
| popper-class                 | custom class name for TimePicker's dropdown                                                                          | ^[string]                                                                                       | ''                                 |
| popper-style                 | custom style for TimePicker's dropdown                                                                               | ^[string] / ^[object]                                                                           | —                                  |
| popper-options               | Customized popper option see more at [popper.js](https://popper.js.org/docs/v2/)                                     | ^[object]`Partial<PopperOptions>`                                                               | {}                                 |
| fallback-placements ^(2.8.4) | list of possible positions for Tooltip [popper.js](https://popper.js.org/docs/v2/modifiers/flip/#fallbackplacements) | ^[array]`Placement[]`                                                                           | ['bottom', 'top', 'right', 'left'] |
| placement ^(2.8.4)           | position of dropdown                                                                                                 | `Placement`                                                                                     | bottom                             |
| range-separator              | range separator                                                                                                      | ^[string]                                                                                       | '-'                                |
| format                       | format of the displayed value in the input box                                                                       | ^[string] see [date formats](/en-US/component/date-picker#date-formats)                         | —                                  |
| default-value                | optional, default date of the calendar                                                                               | ^[Date] / ^[object]`[Date, Date]`                                                               | —                                  |
| value-format                 | optional, format of binding value. If not specified, the binding value will be a Date object                         | ^[string] see [date formats](/en-US/component/date-picker#date-formats)                         | —                                  |
| id                           | same as `id` in native input                                                                                         | ^[string] / ^[object]`[string, string]`                                                         | —                                  |
| name                         | same as `name` in native input                                                                                       | ^[string]                                                                                       | ''                                 |
| aria-label ^(a11y) ^(2.7.2)  | same as `aria-label` in native input                                                                                 | ^[string]                                                                                       | —                                  |
| prefix-icon                  | Custom prefix icon component                                                                                         | ^[string] / ^[Component]                                                                        | Clock                              |
| clear-icon                   | Custom clear icon component                                                                                          | ^[string] / ^[Component]                                                                        | CircleClose                        |
| disabled-hours               | To specify the array of hours that cannot be selected                                                                | ^[Function]`(role: string, comparingDate?: Dayjs) => number[]`                                  | —                                  |
| disabled-minutes             | To specify the array of minutes that cannot be selected                                                              | ^[Function]`(hour: number, role: string, comparingDate?: Dayjs) => number[]`                    | —                                  |
| disabled-seconds             | To specify the array of seconds that cannot be selected                                                              | ^[Function]`(hour: number, minute: number, role: string, comparingDate?: Dayjs) => number[]`    | —                                  |
| teleported                   | whether time-picker dropdown is teleported to the body                                                               | ^[boolean]                                                                                      | true                               |
| tabindex                     | input tabindex                                                                                                       | ^[string] / ^[number]                                                                           | 0                                  |
| empty-values ^(2.7.0)        | empty values of component, [see config-provider](/en-US/component/config-provider#empty-values-configurations)       | ^[array]                                                                                        | —                                  |
| value-on-clear ^(2.7.0)      | clear return value, [see config-provider](/en-US/component/config-provider#empty-values-configurations)              | ^[string] / ^[number] / ^[boolean] / ^[Function]                                                | —                                  |
| label ^(a11y) ^(deprecated)  | same as `aria-label` in native input                                                                                 | ^[string]                                                                                       | —                                  |

| Name           | Description                                                       | Type                                                                                                         |
| -------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| change         | triggers when user confirms the value                             | ^[Function]`(val: number \| string \| Date \| [number, number] \| [string, string] \| [Date, Date]) => void` |
| blur           | triggers when Input blurs                                         | ^[Function]`(e: FocusEvent) => void`                                                                         |
| focus          | triggers when Input focuses                                       | ^[Function]`(e: FocusEvent) => void`                                                                         |
| clear ^(2.7.7) | triggers when the clear icon is clicked in a clearable TimePicker | ^[Function]`() => void`                                                                                      |
| visible-change | triggers when the TimePicker's dropdown appears/disappears        | ^[Function]`(visibility: boolean) => void`                                                                   |

| Name                  | Description                    | Type                    |
| --------------------- | ------------------------------ | ----------------------- |
| focus                 | focus the TimePicker component | ^[Function]`() => void` |
| blur                  | blur the TimePicker component  | ^[Function]`() => void` |
| handleOpen ^(2.2.16)  | open the TimePicker popper     | ^[Function]`() => void` |
| handleClose ^(2.2.16) | close the TimePicker popper    | ^[Function]`() => void` |

<details>
  <summary>Show declarations</summary>

---
Title: TimeSelect
URL: https://element-plus.org/en-US/component/time-select
---

**Examples:**

Example 1 (ts):
```ts
type Placement =
  | 'top'
  | 'top-start'
  | 'top-end'
  | 'bottom'
  | 'bottom-start'
  | 'bottom-end'
  | 'left'
  | 'left-start'
  | 'left-end'
  | 'right'
  | 'right-start'
  | 'right-end'
```

Example 2 (vue):
```vue
<template>
  <div class="example-basic">
    <el-time-picker
      v-model="value1"
      :disabled-hours="disabledHours"
      :disabled-minutes="disabledMinutes"
      :disabled-seconds="disabledSeconds"
      placeholder="Arbitrary time"
    />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value1 = ref(new Date(2016, 9, 10, 18, 30))

const makeRange = (start: number, end: number) => {
  const result: number[] = []
  for (let i = start; i <= end; i++) {
    result.push(i)
  }
  return result
}
const disabledHours = () => {
  return makeRange(0, 16).concat(makeRange(19, 23))
}
const disabledMinutes = (hour: number) => {
  if (hour === 17) {
    return makeRange(0, 29)
  }
  if (hour === 18) {
    return makeRange(31, 59)
  }
}
const disabledSeconds = (hour: number, minute: number) => {
  if (hour === 18 && minute === 30) {
    return makeRange(1, 59)
  }
}
</script>

<style>
.example-basic .el-date-editor {
  margin: 8px;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <div class="example-basic">
    <el-time-picker v-model="value1" placeholder="Arbitrary time" />
    <el-time-picker
      v-model="value2"
      arrow-control
      placeholder="Arbitrary time"
    />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value1 = ref()
const value2 = ref()
</script>

<style>
.example-basic .el-date-editor {
  margin: 8px;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <div class="demo-range">
    <el-time-picker
      v-model="value1"
      is-range
      range-separator="To"
      start-placeholder="Start time"
      end-placeholder="End time"
    />
    <el-time-picker
      v-model="value2"
      is-range
      arrow-control
      range-separator="To"
      start-placeholder="Start time"
      end-placeholder="End time"
    />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value1 = ref<[Date, Date]>([
  new Date(2016, 9, 10, 8, 40),
  new Date(2016, 9, 10, 9, 40),
])
const value2 = ref<[Date, Date]>([
  new Date(2016, 9, 10, 8, 40),
  new Date(2016, 9, 10, 9, 40),
])
</script>

<style>
.demo-range .el-date-editor {
  margin: 8px;
}

.demo-range .el-range-separator {
  box-sizing: content-box;
}
</style>
```

---
