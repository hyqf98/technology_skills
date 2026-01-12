# Element-Plus - Form

**Pages:** 22

---

## TimeSelect

**URL:** llms-txt#timeselect

**Contents:**
- Fixed time picker
- Time Formats
- Fixed time range
- API
  - Attributes
  - Events
  - Exposes
- Vue Examples
  - basic.vue
  - time-formats.vue

Use Time Select for time input.

The available time range is 00:00 to 23:59

Provide a list of fixed time for users to choose.

Use `format` to control format of time(hours and minutes).

Check the list [here](https://day.js.org/docs/en/display/format#list-of-all-available-formats) of all available formats of Day.js.

If start( end ) time is picked at first, then the status of end( start ) time's options will change accordingly.

| Name                      | Description                                                                                                    | Type                                                                                             | Default     |
| ------------------------- | -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | ----------- |
| model-value / v-model     | binding value                                                                                                  | ^[string]                                                                                        | —           |
| disabled                  | whether TimeSelect is disabled                                                                                 | ^[boolean]                                                                                       | false       |
| editable                  | whether the input is editable                                                                                  | ^[boolean]                                                                                       | true        |
| clearable                 | whether to show clear button                                                                                   | ^[boolean]                                                                                       | true        |
| include-end-time ^(2.9.3) | whether `end` is included in options                                                                           | ^[boolean]                                                                                       | false       |
| size                      | size of Input                                                                                                  | ^[enum]`'large' \| 'default' \| 'small'`                                                         | default     |
| placeholder               | placeholder in non-range mode                                                                                  | ^[string]                                                                                        | —           |
| name                      | same as `name` in native input                                                                                 | ^[string]                                                                                        | —           |
| effect                    | Tooltip theme, built-in theme: `dark` / `light`                                                                | ^[string] / ^[enum]`'dark' \| 'light'`                                                           | light       |
| prefix-icon               | custom prefix icon component                                                                                   | ^[string] / ^[Component]                                                                         | Clock       |
| clear-icon                | custom clear icon component                                                                                    | ^[string] / ^[Component]                                                                         | CircleClose |
| start                     | start time                                                                                                     | ^[string]                                                                                        | 09:00       |
| end                       | end time                                                                                                       | ^[string]                                                                                        | 18:00       |
| step                      | time step                                                                                                      | ^[string]                                                                                        | 00:30       |
| min-time                  | minimum time, any time before this time will be disabled                                                       | ^[string]                                                                                        | —           |
| max-time                  | maximum time, any time after this time will be disabled                                                        | ^[string]                                                                                        | —           |
| format                    | set format of time                                                                                             | ^[string] see [formats](https://day.js.org/docs/en/display/format#list-of-all-available-formats) | HH:mm       |
| empty-values ^(2.7.0)     | empty values of component, [see config-provider](/en-US/component/config-provider#empty-values-configurations) | ^[array]                                                                                         | —           |
| value-on-clear ^(2.7.0)   | clear return value, [see config-provider](/en-US/component/config-provider#empty-values-configurations)        | ^[string] / ^[number] / ^[boolean] / ^[Function]                                                 | —           |
| popper-class ^(2.11.4)    | custom class name for TimeSelect's dropdown                                                                    | ^[string]                                                                                        | ''          |
| popper-style ^(2.11.4)    | custom style for TimeSelect's dropdown                                                                         | ^[string] / ^[object]                                                                            | —           |

| Name           | Description                                                       | Type                                     |
| -------------- | ----------------------------------------------------------------- | ---------------------------------------- |
| change         | triggers when user confirms the value                             | ^[Function]`(value: string) => void`     |
| blur           | triggers when Input blurs                                         | ^[Function]`(event: FocusEvent) => void` |
| focus          | triggers when Input focuses                                       | ^[Function]`(event: FocusEvent) => void` |
| clear ^(2.7.7) | triggers when the clear icon is clicked in a clearable TimeSelect | ^[Function]`() => void`                  |

| Method | Description               | Type                    |
| ------ | ------------------------- | ----------------------- |
| focus  | focus the Input component | ^[Function]`() => void` |
| blur   | blur the Input component  | ^[Function]`() => void` |

---
Title: Timeline
URL: https://element-plus.org/en-US/component/timeline
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-time-select
    v-model="value"
    style="width: 240px"
    start="08:30"
    step="00:15"
    end="18:30"
    placeholder="Select time"
  />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref('')
</script>
```

Example 2 (vue):
```vue
<template>
  <el-time-select
    v-model="value"
    style="width: 240px"
    start="00:00"
    step="00:30"
    end="23:59"
    placeholder="Select time"
    format="hh:mm A"
  />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref('')
</script>
```

Example 3 (vue):
```vue
<template>
  <div class="demo-time-range flex flex-wrap gap-4">
    <el-time-select
      v-model="startTime"
      style="width: 240px"
      :max-time="endTime"
      placeholder="Start time"
      start="08:30"
      step="00:15"
      end="18:30"
    />
    <el-time-select
      v-model="endTime"
      style="width: 240px"
      :min-time="startTime"
      placeholder="End time"
      start="08:30"
      step="00:15"
      end="18:30"
    />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const startTime = ref('')
const endTime = ref('')
</script>
```

---

## Virtualized Table ^(beta)

**URL:** llms-txt#virtualized-table-^(beta)

**Contents:**
- Basic usage
- Auto resizer
- Customize Cell Renderer{#customize-cell-renderer}
- Table with selections
- Inline editing
- Table with status
- Table with sticky rows
- Table with fixed columns
- Grouping header
- Filter

Along with evolutionary web development, table component has always been the most popular component in our web apps especially for dashboards, data analysis. For [Table V1](./table.md), with even just 1000 records of data, it can be very annoying when using it, because of the poor performance.

With Virtualized Table, you can render massive chunks of data in a blink of an eye.

Let's demonstrate the performance of the Virtualized Table by rendering a basic example with 10 columns and 1000 rows.

When you do not want to manually pass the `width` and `height` properties to the table, you can wrap the table component with the AutoResizer.
This will automatically update the width and height for you.

Resize your browser to see how it works.

## Customize Cell Renderer{#customize-cell-renderer}

Of course, you can render the table cell according to your needs. Here's a simple example of how to customize your cell.

## Table with selections

Using customized cell renderer to allow selection for your table.

Just as we demonstrated with selections above, you can use the same method to enable inline editing.

You can highlight your table content to distinguish between "success, information, warning, danger" and other states.

To customize the appearance of rows, use the `row-class-name` attribute. For example, every 10th row is highlighted using the `bg-blue-200` class, and every 5th row with the `bg-red-100` class.

## Table with sticky rows

You can make some rows stick to the top of the table, and that can be very easily achieved by using the `fixed-data` attribute.

You can dynamically set the sticky row based on scroll events, as shown in this example.

## Table with fixed columns

If you want to have columns stick to the left or right for some reason, you can achieve this by adding special attributes to the table.

You can set the column's attribute `fixed` to `true` (representing `FixedDir.LEFT`) or `FixedDir.LEFT` or `FixedDir.RIGHT`

By customizing your header renderer, you can group your header as shown in this example.

Virtualized Table provides custom header renderers for creating customized headers. We can then utilize these to render filters.

You can sort the table with sort state.

You can define multiple sortable columns as needed. Keep in mind that if you define multiple sortable columns, the UI
may appear confusing to your users, as it becomes unclear which column is currently being sorted.

When dealing with a large list, it's easy to lose track of the current row and column you are visiting. In such cases, using this feature can be very helpful.

The virtualized table doesn't use the built-in `table` element, so `colspan` and `rowspan` behave a bit differently compared to [TableV1](./table.md). However, with a customized row renderer, these features can still be implemented. In this section, we'll demonstrate how to achieve this.

Since we have covered [Colspan](#colspan), it's worth noting that we also have row span. It's a little bit different from colspan but the idea
is basically the same.

## Rowspan and Colspan together

We can combine rowspan and colspan together to meet your business goal!

Virtual Table can also render data in a tree-like structure. By clicking the arrow icon, you can expand or collapse the tree nodes.

## Dynamic height rows

Virtual Table is capable of rendering rows with dynamic heights. If you're working with data and are uncertain about the content size,
this feature is ideal for rendering rows that adjust to the content's height. To enable this, pass down the `estimated-row-height` attribute.
The closer the estimated height matches the actual content, the smoother the rendering experience.

Using dynamic height rendering, you can also display a detailed view within the table.

Render a customized footer when you want to show a concluding message or information.

## Customized Empty Renderer

Render a customized empty element.

Render an overlay on top of the table when you want to show a loading indicator or something else.

Use the methods provided by Table V2 to scroll manually/programmatically with desired offset/rows.

### TableV2 Attributes

| Name                      | Description                                                                                                                | Type                                                   | Default   |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ | --------- |
| cache                     | Number of rows rendered in advance to boost the performance                                                                | `number`                                               | 2         |
| estimated-row-height      | The estimated row height for rendering dynamic height rows                                                                 | `number`                                               | —         |
| header-class              | Customized class name passed to header wrapper                                                                             | `string` / Function<[HeaderClassGetter](#typings)>     | —         |
| header-props              | Customized props name passed to header component                                                                           | `object` / Function<[HeaderPropsGetter](#typings)>     | —         |
| header-cell-props         | Customized props name passed to header cell component                                                                      | `object` / Function<[HeaderCellPropsGetter](#typings)> | —         |
| header-height             | The height of the header is set by `height`. If given an array, it renders header rows equal to its length                 | `number`/ `number[]`                                   | 50        |
| footer-height             | The height of the footer element, when provided, will be part to the calculation of the table's height.                    | `number`                                               | 0         |
| row-class                 | Customized class name passed to row wrapper                                                                                | `string` / Function<[RowClassGetter](#typings)>        | —         |
| row-key                   | The key of each row, if not provided, will be the index of the row                                                         | `string` / `Symbol` / `number`                         | id        |
| row-props                 | Customized props name passed to row component                                                                              | `object` / Function<[RowPropsGetter](#typings)>        | —         |
| row-height                | The height of each row, used for calculating the total height of the table                                                 | `number`                                               | 50        |
| row-event-handlers        | A collection of handlers attached to each row                                                                              | `object`\<[RowEventHandlers](#typings)\>               | —         |
| cell-props                | extra props passed to each cell (except header cells)                                                                      | `object` / Function<[CellPropsGetter](#typings)>       | —         |
| columns                   | An array of column definitions.                                                                                            | [Column[]](#column-attribute)                          | —         |
| data                      | An array of data to be rendered in the table.                                                                              | [Data[]](#typings)                                     | []        |
| data-getter               | A method to customize data fetch from the data source.                                                                     | Function<[DataGetter\<T\>](#typings)>                  | —         |
| fixed-data                | Data for rendering rows above the main content and below the header                                                        | `object`\<[Data](#typings)\>                           | —         |
| expand-column-key         | The column key indicates which row is expandable                                                                           | `string`                                               | —         |
| expanded-row-keys         | An array of keys for expanded rows, can be used with `v-model`                                                             | [KeyType[]](#typings)                                  | —         |
| default-expanded-row-keys | An array of keys for default expanded rows, **NON REACTIVE**                                                               | [KeyType[]](#typings)                                  | —         |
| class                     | Class name for the virtual table, will be applied to all three tables (left, right, main)                                  | `string` / `array` / `object`                          | —         |
| fixed                     | Flag indicates the table column's width to be fixed or flexible.                                                           | `boolean`                                              | false     |
| width ^(required)         | Width of the table                                                                                                         | `number`                                               | —         |
| height ^(required)        | Height of the table                                                                                                        | `number`                                               | —         |
| max-height                | Maximum height of the table                                                                                                | `number`                                               | —         |
| indent-size               | horizontal indentation of tree table                                                                                       | `number`                                               | 12        |
| h-scrollbar-size          | Indicates the horizontal scrollbar's size for the table, used to prevent the horizontal and vertical scrollbar to collapse | `number`                                               | 6         |
| v-scrollbar-size          | Indicates the vertical scrollbar's size for the table, used to prevent the horizontal and vertical scrollbar to collapse   | `number`                                               | 6         |
| scrollbar-always-on       | If true, the scrollbar will always be shown instead of when mouse is placed above the table                                | `boolean`                                              | false     |
| sort-by                   | Sort indicator                                                                                                             | `object`\<[SortBy](#typings)\>                         | {}        |
| sort-state                | Multiple sort indicator                                                                                                    | `object`\<[SortState](#typings)\>                      | undefined |

| Name        | Type                                        |
| ----------- | ------------------------------------------- |
| cell        | `object`\<[CellSlotProps](#typings)\>       |
| header      | `object`\<[HeaderSlotProps](#typings)\>     |
| header-cell | `object`\<[HeaderCellSlotProps](#typings)\> |
| row         | `object`\<[RowSlotProps](#typings)\>        |
| footer      | —                                           |
| empty       | —                                           |
| overlay     | —                                           |

| Name                 | Description                                                                                                                     | Parameters                                    |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| column-sort          | Invoked when column sorted                                                                                                      | `object`\<[ColumnSortParam](#typings)\>       |
| expanded-rows-change | Invoked when expanded rows changed                                                                                              | [KeyType[]](#typings)                         |
| end-reached          | Invoked when the end of the table is reached. The callback contain the remain distance, it is the usually the scrollbar height. | ^[Function]`(remainDistance: number) => void` |
| scroll               | Invoked after scrolling                                                                                                         | `object`\<[ScrollParams](#typings)\>          |
| rows-rendered        | Invoked when rows are rendered                                                                                                  | `object`\<[RowsRenderedParams](#typings)\>    |
| row-expand           | Invoked when expand/collapse the tree node by clicking the arrow icon                                                           | `object`\<[RowExpandParams](#typings)\>       |

| Method       | Description                                          | Parameters                                                                             |
| ------------ | ---------------------------------------------------- | -------------------------------------------------------------------------------------- |
| scrollTo     | Scroll to a given position                           | ^[Function]`(param: {scrollLeft?: number, scrollTop?: number}) => void`                |
| scrollToLeft | Scroll to a given horizontal position                | ^[Function]`(scrollLeft: number) => void`                                              |
| scrollToTop  | Scroll to a given vertical position                  | ^[Function]`(scrollTop: number) => void`                                               |
| scrollToRow  | scroll to a given row with specified scroll strategy | ^[Function]`(row: number, strategy?: 'center' \| 'end' \| 'start' \| 'smart') => void` |

| Name               | Description                                                           | Type                                                                                                                                                                 | Default |
| ------------------ | --------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| align              | Alignment of the table cell content                                   | [Alignment](https://github.com/element-plus/element-plus/blob/b92b22932758f0ddea98810ae248f6ca62f77e25/packages/components/table-v2/src/constants.ts#L6)             | left    |
| class              | Class name for the column                                             | `string`                                                                                                                                                             | —       |
| key                | Unique identification                                                 | [KeyType](#typings)                                                                                                                                                  | —       |
| dataKey            | Unique identification of data                                         | [KeyType](#typings)                                                                                                                                                  | —       |
| fixed              | Fixed direction of the column                                         | `boolean` / [FixedDir](https://github.com/element-plus/element-plus/blob/b92b22932758f0ddea98810ae248f6ca62f77e25/packages/components/table-v2/src/constants.ts#L11) | false   |
| flexGrow           | CSSProperties flex grow, Only useful when this is not a fixed table   | `number`                                                                                                                                                             | 0       |
| flexShrink         | CSSProperties flex shrink, Only useful when this is not a fixed table | `number`                                                                                                                                                             | 1       |
| headerClass        | Used for customizing header column class                              | `string`                                                                                                                                                             | —       |
| hidden             | Whether the column is invisible                                       | `boolean`                                                                                                                                                            | —       |
| style              | Customized style for column cell, will be merged with grid cell       | ^[object]`CSSProperties`                                                                                                                                             | —       |
| sortable           | Indicates whether the column is sortable                              | `boolean`                                                                                                                                                            | —       |
| title              | The default text rendered in header cell                              | `string`                                                                                                                                                             | —       |
| maxWidth           | Maximum width for the column                                          | `number`                                                                                                                                                             | —       |
| minWidth           | Minimum width for the column                                          | `number`                                                                                                                                                             | —       |
| width ^(required)  | Width for the column                                                  | `number`                                                                                                                                                             | —       |
| cellRenderer       | Customized Cell renderer                                              | `VueComponent` / (props: [CellRenderProps](#typings)) => VNode                                                                                                       | —       |
| headerCellRenderer | Customized Header renderer                                            | `VueComponent` / (props: [HeaderRenderProps](#typings)) => VNode                                                                                                     | —       |

<details>
<summary>Show Type Declarations</summary>

#### How do I render a list with a checkbox in the first column?

Since you are allowed to define your own cell renderer, you can do what the example
[Customize Cell Renderer](#customize-cell-renderer) did to render `checkbox` yourself, and maintain the
state by yourself.

#### Why does virtualized table provide less features than [TableV1](./table.md)

For virtualized table, we intend to provide less feature and let our users implement their own features as needed.
Integrating too many features makes the code hard to maintain and for most users the basic features are enough. Some key
features were not developed yet. We would love to hear from you. Join [Discord](https://discord.com/invite/gXK9XNzW3X) to stay tuned.

### cell-templating.vue

### controlled-sort.vue

### cross-hovering.vue

### detailed-view.vue

### dynamic-height.vue

### fixed-columns.vue

### grouping-header.vue

### inline-editing.vue

### manual-scroll.vue

---
Title: Table
URL: https://element-plus.org/en-US/component/table
---

**Examples:**

Example 1 (ts):
```ts
type HeaderClassGetter = (param: {
  columns: Column<any>[]
  headerIndex: number
}) => string

type HeaderPropsGetter = (param: {
  columns: Column<any>[]
  headerIndex: number
}) => Record<string, any>

type HeaderCellPropsGetter = (param: {
  columns: Column<any>[]
  column: Column<any>
  columnIndex: number
  headerIndex: number
  style: CSSProperties
}) => Record<string, any>

type RowClassGetter = (param: {
  columns: Column<any>[]
  rowData: any
  rowIndex: number
}) => string

type RowPropsGetter = (param: {
  columns: Column<any>[]
  rowData: any
  rowIndex: number
}) => Record<string, any>

type CellPropsGetter = (param: {
  column: Column<any>
  columns: Column<any>[]
  columnIndex: number
  cellData: any
  rowData: any
  rowIndex: number
}) => void

type DataGetterParams<T> = {
  columns: Column<T>[]
  column: Column<T>
  columnIndex: number
} & RowCommonParams

type DataGetter<T> = (params: DataGetterParams<T>) => T

type CellRenderProps<T> = {
  cellData: T
  column: Column<T>
  columns: Column<T>[]
  columnIndex: number
  rowData: any
  rowIndex: number
}

type HeaderRenderProps<T> = {
  column: Column<T>
  columns: Column<T>[]
  columnIndex: number
  headerIndex: number
}

type ScrollParams = {
  xAxisScrollDir: 'forward' | 'backward'
  scrollLeft: number
  yAxisScrollDir: 'forward' | 'backward'
  scrollTop: number
}

type CellSlotProps<T> = {
  column: Column<T>
  columns: Column<T>[]
  columnIndex: number
  depth: number
  style: CSSProperties
  rowData: any
  rowIndex: number
  isScrolling: boolean
  expandIconProps?:
    | {
        rowData: any
        rowIndex: number
        onExpand: (expand: boolean) => void
      }
    | undefined
}

type HeaderSlotProps = {
  cells: VNode[]
  columns: Column<any>[]
  headerIndex: number
}

type HeaderCellSlotProps = {
  class: string
  columns: Column<any>[]
  column: Column<any>
  columnIndex: number
  headerIndex: number
  style: CSSProperties
  headerCellProps?: any
  sortBy: SortBy
  sortState?: SortState | undefined
  onColumnSorted: (e: MouseEvent) => void
}

type RowCommonParams = {
  rowData: any
  rowIndex: number
}

type RowEventHandlerParams = {
  rowKey: KeyType
  event: Event
} & RowCommonParams

type RowEventHandler = (params: RowEventHandlerParams) => void
type RowEventHandlers = {
  onClick?: RowEventHandler
  onContextmenu?: RowEventHandler
  onDblclick?: RowEventHandler
  onMouseenter?: RowEventHandler
  onMouseleave?: RowEventHandler
}

type RowsRenderedParams = {
  rowCacheStart: number
  rowCacheEnd: number
  rowVisibleStart: number
  rowVisibleEnd: number
}

type RowSlotProps = {
  columns: Column<any>[]
  rowData: any
  columnIndex: number
  rowIndex: number
  data: any
  key: number | string
  isScrolling?: boolean
  style: CSSProperties
}

type RowExpandParams = {
  expanded: boolean
  rowKey: KeyType
} & RowCommonParams

type Data = {
  [key: KeyType]: any
  children?: Array<any>
}

type FixedData = Data

type KeyType = string | number | symbol

type ColumnSortParam<T> = { column: Column<T>; key: KeyType; order: SortOrder }

enum SortOrder {
  ASC = 'asc',
  DESC = 'desc',
}

enum Alignment {
  LEFT = 'left',
  CENTER = 'center',
  RIGHT = 'right',
}

type SortBy = { key: KeyType; Order: SortOrder }
type SortState = Record<KeyType, SortOrder>
```

Example 2 (vue):
```vue
<template>
  <div style="height: 400px">
    <el-auto-resizer>
      <template #default="{ height, width }">
        <el-table-v2
          :columns="columns"
          :data="data"
          :width="width"
          :height="height"
          fixed
        />
      </template>
    </el-auto-resizer>
  </div>
</template>

<script lang="ts" setup>
const generateColumns = (length = 10, prefix = 'column-', props?: any) =>
  Array.from({ length }).map((_, columnIndex) => ({
    ...props,
    key: `${prefix}${columnIndex}`,
    dataKey: `${prefix}${columnIndex}`,
    title: `Column ${columnIndex}`,
    width: 150,
  }))

const generateData = (
  columns: ReturnType<typeof generateColumns>,
  length = 200,
  prefix = 'row-'
) =>
  Array.from({ length }).map((_, rowIndex) => {
    return columns.reduce(
      (rowData, column, columnIndex) => {
        rowData[column.dataKey] = `Row ${rowIndex} - Col ${columnIndex}`
        return rowData
      },
      {
        id: `${prefix}${rowIndex}`,
        parentId: null,
      }
    )
  })

const columns = generateColumns(10)
const data = generateData(columns, 200)
</script>
```

Example 3 (vue):
```vue
<template>
  <el-table-v2
    :columns="columns"
    :data="data"
    :width="700"
    :height="400"
    fixed
  />
</template>

<script lang="ts" setup>
const generateColumns = (length = 10, prefix = 'column-', props?: any) =>
  Array.from({ length }).map((_, columnIndex) => ({
    ...props,
    key: `${prefix}${columnIndex}`,
    dataKey: `${prefix}${columnIndex}`,
    title: `Column ${columnIndex}`,
    width: 150,
  }))

const generateData = (
  columns: ReturnType<typeof generateColumns>,
  length = 200,
  prefix = 'row-'
) =>
  Array.from({ length }).map((_, rowIndex) => {
    return columns.reduce(
      (rowData, column, columnIndex) => {
        rowData[column.dataKey] = `Row ${rowIndex} - Col ${columnIndex}`
        return rowData
      },
      {
        id: `${prefix}${rowIndex}`,
        parentId: null,
      }
    )
  })

const columns = generateColumns(10)
const data = generateData(columns, 1000)
</script>
```

Example 4 (vue):
```vue
<template>
  <el-table-v2
    :columns="columns"
    :data="data"
    :width="700"
    :height="400"
    fixed
  />
</template>

<script lang="tsx" setup>
import { ref } from 'vue'
import dayjs from 'dayjs'
import {
  ElButton,
  ElIcon,
  ElTag,
  ElTooltip,
  TableV2FixedDir,
} from 'element-plus'
import { Timer } from '@element-plus/icons-vue'

import type { Column } from 'element-plus'

let id = 0

const dataGenerator = () => ({
  id: `random-id-${++id}`,
  name: 'Tom',
  date: '2020-10-1',
})

const columns: Column<any>[] = [
  {
    key: 'date',
    title: 'Date',
    dataKey: 'date',
    width: 150,
    fixed: TableV2FixedDir.LEFT,
    cellRenderer: ({ cellData: date }) => (
      <ElTooltip content={dayjs(date).format('YYYY/MM/DD')}>
        {
          <span class="flex items-center">
            <ElIcon class="mr-3">
              <Timer />
            </ElIcon>
            {dayjs(date).format('YYYY/MM/DD')}
          </span>
        }
      </ElTooltip>
    ),
  },
  {
    key: 'name',
    title: 'Name',
    dataKey: 'name',
    width: 150,
    align: 'center',
    cellRenderer: ({ cellData: name }) => <ElTag>{name}</ElTag>,
  },
  {
    key: 'operations',
    title: 'Operations',
    cellRenderer: () => (
      <>
        <ElButton size="small">Edit</ElButton>
        <ElButton size="small" type="danger">
          Delete
        </ElButton>
      </>
    ),
    width: 150,
    align: 'center',
  },
]

const data = ref(Array.from({ length: 200 }).map(dataGenerator))
</script>
```

---

## Segmented

**URL:** llms-txt#segmented

**Contents:**
- Basic Usage
- Direction Usage ^(2.8.7)
- Disabled
- Aliases for custom options ^(2.9.8)
- Block
- Custom Content
- Custom Style
- API
  - Attributes
  - props

Display multiple options and allow users to select a single option.

Set `v-model` to the option value is selected.

## Direction Usage ^(2.8.7)

Set `vertical` to change direction.

Set `disabled` of segmented or option to `true` to disable it.

## Aliases for custom options ^(2.9.8)

When your `options` format is different from the default format, you can customize the alias of the `options` through the `props` attribute

Set `block` to `true` to fit the width of parent element.

Set default slot to render custom content.

Set custom styles using CSS variables.

| Name                     | Description                                    | Type                                           | Default    |
| ------------------------ | ---------------------------------------------- | ---------------------------------------------- | ---------- |
| model-value / v-model    | binding value                                  | ^[string] / ^[number] / ^[boolean]             | —          |
| options                  | data of the options                            | ^[array]`Option[]`                             | []         |
| [props](#props) ^(2.9.8) | configuration options, see the following table | ^[object]                                      | —          |
| size                     | size of component                              | ^[enum]`'' \| 'large' \| 'default' \| 'small'` | ''         |
| block                    | fit width of parent content                    | ^[boolean]                                     | false      |
| disabled                 | whether segmented is disabled                  | ^[boolean]                                     | false      |
| validate-event           | whether to trigger form validation             | ^[boolean]                                     | true       |
| name                     | native `name` attribute                        | ^[string]                                      | —          |
| id                       | native `id` attribute                          | ^[string]                                      | —          |
| aria-label ^(a11y)       | native `aria-label` attribute                  | ^[string]                                      | —          |
| direction ^(2.8.7)       | display direction                              | ^[enum]`'horizontal' \| 'vertical'`            | horizontal |

| Attribute | Description                                                     | Type      | Default  |
| --------- | --------------------------------------------------------------- | --------- | -------- |
| value     | specify which key of node object is used as the node's value    | ^[string] | value    |
| label     | specify which key of node object is used as the node's label    | ^[string] | label    |
| disabled  | specify which key of node object is used as the node's disabled | ^[string] | disabled |

| Name   | Description                                                                   | Type                            |
| ------ | ----------------------------------------------------------------------------- | ------------------------------- |
| change | triggers when the selected value changes, the param is current selected value | ^[Function]`(val: any) => void` |

| Name    | Description     | Type                        |
| ------- | --------------- | --------------------------- |
| default | option renderer | ^[object]`{ item: Option }` |

<details>
  <summary>Show declarations</summary>

### custom-content.vue

### custom-direction.vue

---
Title: Virtualized Select
URL: https://element-plus.org/en-US/component/select-v2
---

**Examples:**

Example 1 (ts):
```ts
type Option = Record<string, any> | string | number | boolean
```

Example 2 (vue):
```vue
<template>
  <div class="flex flex-col items-start gap-4">
    <el-segmented v-model="value" :options="options" size="large" />
    <el-segmented v-model="value" :options="options" size="default" />
    <el-segmented v-model="value" :options="options" size="small" />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref('Mon')

const options = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
</script>
```

Example 3 (vue):
```vue
<template>
  <div>
    <el-segmented v-model="value" :options="options" block />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref('Mon')

const options = [
  'Mon',
  'Tue',
  'Wed',
  'Thu',
  'Fri',
  'Sat',
  'Sunday long long long long long long long',
]
</script>
```

Example 4 (vue):
```vue
<template>
  <div>
    <el-segmented v-model="value" :options="options">
      <template #default="scope">
        <div class="flex flex-col items-center gap-2 p-2">
          <el-icon size="20">
            <component :is="scope.item.icon" />
          </el-icon>
          <div>{{ scope.item.label }}</div>
        </div>
      </template>
    </el-segmented>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import {
  Apple,
  Cherry,
  Grape,
  Orange,
  Pear,
  Watermelon,
} from '@element-plus/icons-vue'

const value = ref('Apple')

const options = [
  {
    label: 'Apple',
    value: 'Apple',
    icon: Apple,
  },
  {
    label: 'Cherry',
    value: 'Cherry',
    icon: Cherry,
  },
  {
    label: 'Grape',
    value: 'Grape',
    icon: Grape,
  },
  {
    label: 'Orange',
    value: 'Orange',
    icon: Orange,
  },
  {
    label: 'Pear',
    value: 'Pear',
    icon: Pear,
  },
  {
    label: 'Watermelon',
    value: 'Watermelon',
    icon: Watermelon,
  },
]
</script>
```

---

## InputTag

**URL:** llms-txt#inputtag

**Contents:**
- Basic Usage
- Custom Trigger
- Maximum Tags
- Collapse Tags ^(2.11.0)
- Disabled
- Clearable
- Custom Clear Icon ^(2.11.0)
- Draggable
- Delimiter ^(2.9.9)
- Sizes

The InputTag component allows users to add content as tags.

Press the Enter key to add the input as a tag.

You can customize the key used to trigger the input tag. The default key is Enter.

You can set a limit on the number of tags that can be added.

## Collapse Tags ^(2.11.0)

Use the collapse tags attribute to merge them into one piece of text. You can use the collapse tags tooltip property to enable the behavior of hovering over collapsed text to display specific selected values. Using the collapse tags tooltip attribute will render the max attribute invalid.

You can set the InputTag to be disabled.

You can set whether to show the clear button.

## Custom Clear Icon ^(2.11.0)

You can customize the clear icon by setting the `clear-icon` attribute.

You can set whether tags can be dragged.

## Delimiter ^(2.9.9)

You can add a tag when a delimiter is matched.

Add `size` attribute to change the size of InputTag. In addition to the default size, there are two other options: `large`, `small`.

You can customize the tag content by `tag` slot.

## Custom Prefix and Suffix

You can customize the prefix and suffix of the InputTag by `prefix` and `suffix` slot.

| Name                            | Description                                                                                                    | Type                                                        | Default     |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- | ----------- |
| model-value / v-model           | binding value                                                                                                  | ^[array]`string[]`                                          | —           |
| max                             | max number tags that can be enter                                                                              | ^[number]                                                   | —           |
| tag-type                        | tag type                                                                                                       | ^[enum]`'' \| 'success' \| 'info' \| 'warning' \| 'danger'` | info        |
| tag-effect                      | tag effect                                                                                                     | ^[enum]`'' \| 'light' \| 'dark' \| 'plain'`                 | light       |
| trigger                         | the key to trigger input tag                                                                                   | ^[enum]`'Enter' \| 'Space'`                                 | Enter       |
| draggable                       | whether tags can be dragged                                                                                    | ^[boolean]                                                  | false       |
| delimiter ^(2.9.9)              | add a tag when a delimiter is matched                                                                          | ^[string] / ^[regex]                                        | —           |
| size                            | input box size                                                                                                 | ^[enum]`'large' \| 'default' \| 'small'`                    | —           |
| collapse-tags ^(2.11.0)         | whether to collapse tags to a text when multiple selecting                                                     | ^[boolean]                                                  | false       |
| collapse-tags-tooltip ^(2.11.0) | whether show all selected tags when mouse hover text of collapse-tags. To use this, collapse-tags must be true | ^[boolean]                                                  | false       |
| save-on-blur ^(2.9.7)           | whether to save the input value when the input loses focus                                                     | ^[boolean]                                                  | true        |
| clearable                       | whether to show clear button                                                                                   | ^[boolean]                                                  | false       |
| clear-icon ^(2.11.0)            | custom clear icon component                                                                                    | ^[string] / ^[object]`Component`                            | CircleClose |
| disabled                        | whether to disable input-tag                                                                                   | ^[boolean]                                                  | false       |
| validate-event                  | whether to trigger form validation                                                                             | ^[boolean]                                                  | true        |
| readonly                        | same as `readonly` in native input                                                                             | ^[boolean]                                                  | false       |
| autofocus                       | same as `autofocus` in native input                                                                            | ^[boolean]                                                  | false       |
| id                              | same as `id` in native input                                                                                   | ^[string]                                                   | —           |
| tabindex                        | same as `tabindex` in native input                                                                             | ^[string] / ^[number]                                       | —           |
| max-collapse-tags ^(2.11.0)     | the max tags number to be shown. To use this, collapse-tags must be true                                       | ^[number]                                                   | 1           |
| maxlength                       | same as `maxlength` in native input                                                                            | ^[string] / ^[number]                                       | —           |
| minlength                       | same as `minlength` in native input                                                                            | ^[string] / ^[number]                                       | —           |
| placeholder                     | placeholder of input                                                                                           | ^[string]                                                   | —           |
| autocomplete                    | same as `autocomplete` in native input                                                                         | ^[string]                                                   | off         |
| aria-label ^(a11y)              | native `aria-label` attribute                                                                                  | ^[string]                                                   | —           |

| Name               | Description                             | Type                                                                     |
| ------------------ | --------------------------------------- | ------------------------------------------------------------------------ |
| change             | triggers when the modelValue change     | ^[Function]`(value: string[]) => void`                                   |
| input              | triggers when the input value change    | ^[Function]`(value: string) => void`                                     |
| add-tag            | triggers when a tag is added            | ^[Function]`(value: string \| string []) => void`                        |
| remove-tag         | triggers when a tag is removed          | ^[Function]`(value: string, index: number) => void`                      |
| drag-tag ^(2.11.3) | triggers when a tag is dragged          | ^[Function]`(oldIndex: number, newIndex: number, value: string) => void` |
| focus              | triggers when InputTag focuses          | ^[Function]`(event: FocusEvent) => void`                                 |
| blur               | triggers when InputTag blurs            | ^[Function]`(event: FocusEvent) => void`                                 |
| clear              | triggers when the clear icon is clicked | ^[Function]`() => void`                                                  |

| Name   | Description                | Type                                        |
| ------ | -------------------------- | ------------------------------------------- |
| tag    | content as tag             | ^[object]`{ value: string, index: number }` |
| prefix | content as InputTag prefix | —                                           |
| suffix | content as InputTag suffix | —                                           |

| Name  | Description             | Type                    |
| ----- | ----------------------- | ----------------------- |
| focus | focus the input element | ^[Function]`() => void` |
| blur  | blur the input element  | ^[Function]`() => void` |

### prefix-suffix.vue

---
Title: Input
URL: https://element-plus.org/en-US/component/input
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-input-tag
    v-model="input"
    placeholder="Please input"
    aria-label="Please click the Enter key after input"
  />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const input = ref<string[]>()
</script>
```

Example 2 (vue):
```vue
<template>
  <el-input-tag
    v-model="input"
    clearable
    :clear-icon="CloseBold"
    placeholder="Custom clear icon"
  />
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { CloseBold } from '@element-plus/icons-vue'

const input = ref<string[]>(['custom', 'clear', 'icon'])
</script>
```

Example 3 (vue):
```vue
<template>
  <el-input-tag v-model="input" clearable placeholder="Please input" />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const input = ref<string[]>(['tag1', 'tag2', 'tag3'])
</script>
```

Example 4 (vue):
```vue
<template>
  <div class="m-4">
    <p>use collapse-tags</p>
    <el-input-tag
      v-model="input1"
      collapse-tags
      placeholder="Please input"
      aria-label="Please click the Enter key after input"
    />
    <p>use collapse-tags-tooltip</p>
    <el-input-tag
      v-model="input2"
      collapse-tags
      collapse-tags-tooltip
      placeholder="Please input"
      aria-label="Please click the Enter key after input"
    />
    <p>use max-collapse-tags</p>
    <el-input-tag
      v-model="input3"
      collapse-tags
      collapse-tags-tooltip
      :max-collapse-tags="3"
      placeholder="Please input"
      aria-label="Please click the Enter key after input"
    />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const input1 = ref<string[]>()
const input2 = ref<string[]>()
const input3 = ref<string[]>()
</script>
```

---

## Tag

**URL:** llms-txt#tag

**Contents:**
- Basic usage
- Removable Tag
- Edit Dynamically
- Sizes
- Theme
- Rounded
- Checkable Tag
- Tag API
  - Tag Attributes
  - Tag Events

Used for marking and selection.

You can use the `close` event to add and remove tag dynamically.

Besides default size, Tag component provides three additional sizes for you to choose among different scenarios.

Tag provide three different themes: `dark`、`light` and `plain`

Tag can also be rounded like button.

Sometimes because of the business needs, we might need checkbox like tag, but **button like checkbox** cannot meet our needs, here comes `check-tag`. You can use `type` prop in ^(2.5.4).

| Name                | Description                          | Type                                                               | Default |
| ------------------- | ------------------------------------ | ------------------------------------------------------------------ | ------- |
| type                | type of Tag                          | ^[enum]`'primary' \| 'success' \| 'info' \| 'warning' \| 'danger'` | primary |
| closable            | whether Tag can be removed           | ^[boolean]                                                         | false   |
| disable-transitions | whether to disable animations        | ^[boolean]                                                         | false   |
| hit                 | whether Tag has a highlighted border | ^[boolean]                                                         | false   |
| color               | background color of the Tag          | ^[string]                                                          | —       |
| size                | size of Tag                          | ^[enum]`'large' \| 'default' \| 'small'`                           | —       |
| effect              | theme of Tag                         | ^[enum]`'dark' \| 'light' \| 'plain'`                              | light   |
| round               | whether Tag is rounded               | ^[boolean]                                                         | false   |

| Name  | Description                  | Type                                   |
| ----- | ---------------------------- | -------------------------------------- |
| click | triggers when Tag is clicked | ^[Function]`(evt: MouseEvent) => void` |
| close | triggers when Tag is removed | ^[Function]`(evt: MouseEvent) => void` |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

### CheckTag Attributes

| Name                      | Description                       | Type                                                               | Default |
| ------------------------- | --------------------------------- | ------------------------------------------------------------------ | ------- |
| checked / v-model:checked | is checked                        | ^[boolean]                                                         | false   |
| disabled ^(2.8.2)         | whether the check-tag is disabled | ^[boolean]                                                         | false   |
| type ^(2.5.4)             | type of CheckTag                  | ^[enum]`'primary' \| 'success' \| 'info' \| 'warning' \| 'danger'` | primary |

| Name   | Description                        | Type                                  |
| ------ | ---------------------------------- | ------------------------------------- |
| change | triggers when Check Tag is clicked | ^[Function]`(value: boolean) => void` |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

---
Title: Text
URL: https://element-plus.org/en-US/component/text
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <div class="flex gap-2">
    <el-tag type="primary">Tag 1</el-tag>
    <el-tag type="success">Tag 2</el-tag>
    <el-tag type="info">Tag 3</el-tag>
    <el-tag type="warning">Tag 4</el-tag>
    <el-tag type="danger">Tag 5</el-tag>
  </div>
</template>
```

Example 2 (vue):
```vue
<template>
  <div class="flex gap-2">
    <el-check-tag checked>Checked</el-check-tag>
    <el-check-tag :checked="checked" @change="onChange">Toggle me</el-check-tag>
    <el-check-tag disabled>Disabled</el-check-tag>
  </div>
  <div class="flex gap-2 mt-4">
    <el-check-tag :checked="checked1" type="primary" @change="onChange1">
      Tag 1
    </el-check-tag>
    <el-check-tag :checked="checked2" type="success" @change="onChange2">
      Tag 2
    </el-check-tag>
    <el-check-tag :checked="checked3" type="info" @change="onChange3">
      Tag 3
    </el-check-tag>
    <el-check-tag :checked="checked4" type="warning" @change="onChange4">
      Tag 4
    </el-check-tag>
    <el-check-tag :checked="checked5" type="danger" @change="onChange5">
      Tag 5
    </el-check-tag>
    <el-check-tag
      :checked="checked6"
      disabled
      type="success"
      @change="onChange6"
    >
      Tag 6
    </el-check-tag>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const checked = ref(false)
const checked1 = ref(true)
const checked2 = ref(true)
const checked3 = ref(true)
const checked4 = ref(true)
const checked5 = ref(true)
const checked6 = ref(true)

const onChange = (status: boolean) => {
  checked.value = status
}

const onChange1 = (status: boolean) => {
  checked1.value = status
}

const onChange2 = (status: boolean) => {
  checked2.value = status
}

const onChange3 = (status: boolean) => {
  checked3.value = status
}

const onChange4 = (status: boolean) => {
  checked4.value = status
}

const onChange5 = (status: boolean) => {
  checked5.value = status
}

const onChange6 = (status: boolean) => {
  checked6.value = status
}
</script>
```

Example 3 (vue):
```vue
<template>
  <div class="flex gap-2">
    <el-tag
      v-for="tag in dynamicTags"
      :key="tag"
      closable
      :disable-transitions="false"
      @close="handleClose(tag)"
    >
      {{ tag }}
    </el-tag>
    <el-input
      v-if="inputVisible"
      ref="InputRef"
      v-model="inputValue"
      class="w-20"
      size="small"
      @keyup.enter="handleInputConfirm"
      @blur="handleInputConfirm"
    />
    <el-button v-else class="button-new-tag" size="small" @click="showInput">
      + New Tag
    </el-button>
  </div>
</template>

<script lang="ts" setup>
import { nextTick, ref } from 'vue'

import type { InputInstance } from 'element-plus'

const inputValue = ref('')
const dynamicTags = ref(['Tag 1', 'Tag 2', 'Tag 3'])
const inputVisible = ref(false)
const InputRef = ref<InputInstance>()

const handleClose = (tag: string) => {
  dynamicTags.value.splice(dynamicTags.value.indexOf(tag), 1)
}

const showInput = () => {
  inputVisible.value = true
  nextTick(() => {
    InputRef.value!.input!.focus()
  })
}

const handleInputConfirm = () => {
  if (inputValue.value) {
    dynamicTags.value.push(inputValue.value)
  }
  inputVisible.value = false
  inputValue.value = ''
}
</script>
```

Example 4 (vue):
```vue
<template>
  <div class="flex gap-2">
    <el-tag v-for="tag in tags" :key="tag.name" closable :type="tag.type">
      {{ tag.name }}
    </el-tag>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

import type { TagProps } from 'element-plus'

interface TagsItem {
  name: string
  type: TagProps['type']
}

const tags = ref<TagsItem[]>([
  { name: 'Tag 1', type: 'primary' },
  { name: 'Tag 2', type: 'success' },
  { name: 'Tag 3', type: 'info' },
  { name: 'Tag 4', type: 'warning' },
  { name: 'Tag 5', type: 'danger' },
])
</script>
```

---

## Radio

**URL:** llms-txt#radio

**Contents:**
- Basic usage
- Disabled
- Radio Group
- With borders
- Options attribute ^(2.11.2)
- Radio Button
- Radio API
  - Radio Attributes
  - Radio Events
  - Radio Slots

Single selection among multiple options.

Radio should not have too many options. Otherwise, use the Select component instead.

`disabled` attribute is used to disable the radio.

Suitable for choosing from some mutually exclusive options.

## Options attribute ^(2.11.2)

Radio with button group visual effect.

| Name                  | Description                                                            | Type                                     | Default |
| --------------------- | ---------------------------------------------------------------------- | ---------------------------------------- | ------- |
| model-value / v-model | binding value                                                          | ^[string] / ^[number] / ^[boolean]       | —       |
| value ^(2.6.0)        | the value of Radio                                                     | ^[string] / ^[number] / ^[boolean]       | —       |
| label                 | the label of Radio. If there's no `value`, `label` will act as `value` | ^[string] / ^[number] / ^[boolean]       | —       |
| disabled              | whether Radio is disabled                                              | ^[boolean]                               | false   |
| border                | whether to add a border around Radio                                   | ^[boolean]                               | false   |
| size                  | size of the Radio                                                      | ^[enum]`'large' \| 'default' \| 'small'` | —       |
| name                  | native `name` attribute                                                | ^[string]                                | —       |

| Name   | Description                           | Type                                                      |
| ------ | ------------------------------------- | --------------------------------------------------------- |
| change | triggers when the bound value changes | ^[Function]`(value: string \| number \| boolean) => void` |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

### RadioGroup Attributes

| Name                        | Description                                                                                    | Type                                                             | Default                                                  |
| --------------------------- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | -------------------------------------------------------- |
| model-value / v-model       | binding value                                                                                  | ^[string] / ^[number] / ^[boolean]                               | —                                                        |
| size                        | the size of radio buttons or bordered radios                                                   | ^[string]                                                        | default                                                  |
| disabled                    | whether the nesting radios are disabled                                                        | ^[boolean]                                                       | false                                                    |
| validate-event              | whether to trigger form validation                                                             | ^[boolean]                                                       | true                                                     |
| text-color                  | font color when button is active                                                               | ^[string]                                                        | #ffffff                                                  |
| fill                        | border and background color when button is active                                              | ^[string]                                                        | #409eff                                                  |
| aria-label ^(a11y) ^(2.7.2) | same as `aria-label` in RadioGroup                                                             | ^[string]                                                        | —                                                        |
| name                        | native `name` attribute                                                                        | ^[string]                                                        | —                                                        |
| id                          | native `id` attribute                                                                          | ^[string]                                                        | —                                                        |
| label ^(a11y) ^(deprecated) | same as `aria-label` in RadioGroup                                                             | ^[string]                                                        | —                                                        |
| options ^(2.11.2)           | data of the options, the key of `value` and `label` and `disabled` can be customize by `props` | ^[array]`Array<{[key: string]: any}>`                            | —                                                        |
| props ^(2.11.2)             | configuration options                                                                          | ^[object]`{ value?: string, label?: string, disabled?: boolean}` | `{value: 'value', label: 'label', disabled: 'disabled'}` |
| type ^(2.11.5)              | component type to render options (e.g. `'button'`)                                             | ^[enum]`'radio' \| 'button'`                                     | 'radio'                                                  |

### RadioGroup Events

| Name   | Description                           | Type                                                      |
| ------ | ------------------------------------- | --------------------------------------------------------- |
| change | triggers when the bound value changes | ^[Function]`(value: string \| number \| boolean) => void` |

| Name    | Description               | Subtags             |
| ------- | ------------------------- | ------------------- |
| default | customize default content | Radio / RadioButton |

### RadioButton Attributes

| Name           | Description                                                            | Type                               | Default |
| -------------- | ---------------------------------------------------------------------- | ---------------------------------- | ------- |
| value ^(2.6.0) | the value of Radio                                                     | ^[string] / ^[number] / ^[boolean] | —       |
| label          | the label of Radio. If there's no `value`, `label` will act as `value` | ^[string] / ^[number] / ^[boolean] | —       |
| disabled       | whether Radio is disabled                                              | ^[boolean]                         | false   |
| name           | native 'name' attribute                                                | ^[string]                          | —       |

### RadioButton Slots

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

---
Title: Rate
URL: https://element-plus.org/en-US/component/rate
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-radio-group v-model="radio1">
    <!-- works when >=2.6.0, recommended ✔️ not work when <2.6.0 ❌ -->
    <el-radio value="Value 1">Option 1</el-radio>
    <!-- works when <2.6.0, deprecated act as value when >=3.0.0 -->
    <el-radio label="Label 2 & Value 2">Option 2</el-radio>
  </el-radio-group>
</template>
```

Example 2 (vue):
```vue
<template>
  <div class="mb-2 ml-4">
    <el-radio-group v-model="radio1">
      <el-radio value="1" size="large">Option 1</el-radio>
      <el-radio value="2" size="large">Option 2</el-radio>
    </el-radio-group>
  </div>
  <div class="my-2 ml-4">
    <el-radio-group v-model="radio2">
      <el-radio value="1">Option 1</el-radio>
      <el-radio value="2">Option 2</el-radio>
    </el-radio-group>
  </div>
  <div class="my-4 ml-4">
    <el-radio-group v-model="radio3">
      <el-radio value="1" size="small">Option 1</el-radio>
      <el-radio value="2" size="small">Option 2</el-radio>
    </el-radio-group>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const radio1 = ref('1')
const radio2 = ref('1')
const radio3 = ref('1')
</script>
```

Example 3 (vue):
```vue
<template>
  <el-radio v-model="radio" disabled value="disabled">Option A</el-radio>
  <el-radio v-model="radio" disabled value="selected and disabled">
    Option B
  </el-radio>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const radio = ref('selected and disabled')
</script>
```

Example 4 (vue):
```vue
<template>
  <el-radio-group v-model="radio" :options="options" :props="props" />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const radio = ref(3)
const props = { value: 'id', label: 'name', disabled: 'unable' }
const options = [
  {
    id: 3,
    name: 'Option A',
  },
  {
    id: 6,
    name: 'Option B',
  },
  {
    id: 9,
    name: 'Option C',
    unable: true,
  },
]
</script>
```

---

## Message Box

**URL:** llms-txt#message-box

**Contents:**
- Alert
- Confirm
- Prompt
- Use VNode
- Customization
- Use HTML String
- Distinguishing cancel and close
- Centered content
- Customized Icon
- Draggable

A set of modal boxes simulating system message box, mainly for alerting information, confirm operations and prompting messages.

Alert interrupts user operation until the user confirms.

Confirm is used to ask users' confirmation.

Prompt is used when user input is required.

`message` can be VNode.

Can be customized to show various content.

`message` supports HTML string.

## Distinguishing cancel and close

In some cases, clicking the cancel button and close button may have different meanings.

Content of MessageBox can be centered.

The icon can be customized to any Vue component or [render function (JSX)](https://vuejs.org/guide/extras/render-function.html).

MessageBox can be draggable.

If Element Plus is fully imported, it will add the following global methods for `app.config.globalProperties`: `$msgbox`, `$alert`, `$confirm` and `$prompt`. So in a Vue instance you can call `MessageBox` like what we did in this page. The parameters are:

- `$msgbox(options)`
- `$alert(message, title, options)` or `$alert(message, options)`
- `$confirm(message, title, options)` or `$confirm(message, options)`
- `$prompt(message, title, options)` or `$prompt(message, options)`

## App context inheritance <el-tag>> 2.0.4</el-tag>

Now message box accepts a `context` as second (forth if you are using message box variants) parameter of the message constructor which allows you to inject current app's context to message which allows you to inherit all the properties of the app.

If you prefer importing `MessageBox` on demand:

The corresponding methods are: `ElMessageBox`, `ElMessageBox.alert`, `ElMessageBox.confirm` and `ElMessageBox.prompt`. The parameters are the same as above.

| Name                              | Description                                                                                                                              | Type                                                                                                | Default                                          |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| autofocus                         | auto focus when open MessageBox                                                                                                          | ^[boolean]                                                                                          | true                                             |
| title                             | title of the MessageBox                                                                                                                  | ^[string]                                                                                           | ''                                               |
| message                           | content of the MessageBox                                                                                                                | ^[string] / ^[VNode] / ^[Function]`() => VNode` ^(2.2.17)                                           | —                                                |
| dangerouslyUseHTMLString          | whether `message` is treated as HTML string                                                                                              | ^[boolean]                                                                                          | false                                            |
| type                              | message type, used for icon display                                                                                                      | ^[enum]`'primary' (2.9.11) \| 'success' \| 'info' \| 'warning' \| 'error'`                          | ''                                               |
| icon                              | custom icon component, overrides `type`                                                                                                  | ^[string] / ^[Component]                                                                            | ''                                               |
| closeIcon ^(2.9.5)                | custom close icon component, default is Close                                                                                            | ^[string] / ^[Component]                                                                            | ''                                               |
| customClass                       | custom class name for MessageBox                                                                                                         | ^[string]                                                                                           | ''                                               |
| customStyle                       | custom inline style for MessageBox                                                                                                       | ^[CSSProperties]                                                                                    | {}                                               |
| modal                             | whether a mask is displayed                                                                                                              | ^[boolean]                                                                                          | true                                             |
| modalClass                        | custom class names for mask                                                                                                              | string                                                                                              | —                                                |
| callback                          | MessageBox closing callback if you don't prefer Promise                                                                                  | ^[Function]`(value: string, action: Action) => any \| (action: Action) => any`                      | null                                             |
| showClose                         | whether to show close icon of MessageBox                                                                                                 | ^[boolean]                                                                                          | true                                             |
| beforeClose                       | callback before MessageBox closes, and it will prevent MessageBox from closing                                                           | ^[Function]`(action: Action, instance: MessageBoxState, done: () => void) => void`                  | null                                             |
| distinguishCancelAndClose         | whether to distinguish canceling and closing the MessageBox                                                                              | ^[boolean]                                                                                          | false                                            |
| lockScroll                        | whether to lock body scroll when MessageBox prompts                                                                                      | ^[boolean]                                                                                          | true                                             |
| showCancelButton                  | whether to show a cancel button                                                                                                          | ^[boolean]                                                                                          | false (true when called with confirm and prompt) |
| showConfirmButton                 | whether to show a confirm button                                                                                                         | ^[boolean]                                                                                          | true                                             |
| cancelButtonText                  | text content of cancel button                                                                                                            | ^[string]                                                                                           | Cancel                                           |
| confirmButtonText                 | text content of confirm button                                                                                                           | ^[string]                                                                                           | OK                                               |
| cancelButtonLoadingIcon ^(2.7.7)  | loading icon content of cancel button                                                                                                    | ^[string] / ^[Component]                                                                            | Loading                                          |
| confirmButtonLoadingIcon ^(2.7.7) | loading icon content of confirm button                                                                                                   | ^[string] / ^[Component]                                                                            | Loading                                          |
| cancelButtonClass                 | custom class name of cancel button                                                                                                       | ^[string]                                                                                           | ''                                               |
| confirmButtonClass                | custom class name of confirm button                                                                                                      | ^[string]                                                                                           | ''                                               |
| closeOnClickModal                 | whether MessageBox can be closed by clicking the mask                                                                                    | ^[boolean]                                                                                          | true (false when called with alert)              |
| closeOnPressEscape                | whether MessageBox can be closed by pressing the ESC                                                                                     | ^[boolean]                                                                                          | true (false when called with alert)              |
| closeOnHashChange                 | whether to close MessageBox when hash changes                                                                                            | ^[boolean]                                                                                          | true                                             |
| showInput                         | whether to show an input                                                                                                                 | ^[boolean]                                                                                          | false (true when called with prompt)             |
| inputPlaceholder                  | placeholder of input                                                                                                                     | ^[string]                                                                                           | ''                                               |
| inputType                         | type of input, see more in [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#Form_%3Cinput%3E_types)                 | ^[string]`'text' \| 'textarea' \| 'number' \| 'password' \| 'email' \| 'search' \| 'tel' \|  'url'` | text                                             |
| inputValue                        | initial value of input                                                                                                                   | ^[string]                                                                                           | ''                                               |
| inputPattern                      | regexp for the input                                                                                                                     | ^[regexp]                                                                                           | null                                             |
| inputValidator                    | validation function for the input. Should returns a boolean or string. If a string is returned, it will be assigned to inputErrorMessage | ^[Function]`(value: string) => boolean \| string`/ `undefined`                                      | undefined                                        |
| inputErrorMessage                 | error message when validation fails                                                                                                      | ^[string]                                                                                           | Illegal input                                    |
| center                            | whether to align the content in center                                                                                                   | ^[boolean]                                                                                          | false                                            |
| draggable                         | whether MessageBox is draggable                                                                                                          | ^[boolean]                                                                                          | false                                            |
| overflow ^(2.5.4)                 | draggable MessageBox can overflow the viewport                                                                                           | ^[boolean]                                                                                          | false                                            |
| roundButton                       | whether to use round button                                                                                                              | ^[boolean]                                                                                          | false                                            |
| buttonSize                        | custom size of confirm and cancel buttons                                                                                                | ^[string]`'small' \| 'default' \| 'large'`                                                          | default                                          |
| appendTo ^(2.2.19)                | set the root element for the message box                                                                                                 | ^[CSSSelector] / ^[HTMLElement]                                                                     | —                                                |

### centered-content.vue

### customization.vue

### customized-icon.vue

### distinguishable-close-cancel.vue

---
Title: Message
URL: https://element-plus.org/en-US/component/message
---

**Examples:**

Example 1 (ts):
```ts
import { getCurrentInstance } from 'vue'
import { ElMessageBox } from 'element-plus'

// in your setup method
const { appContext } = getCurrentInstance()!
// You can pass it like:
ElMessageBox({}, appContext)
// or if you are using variants
ElMessageBox.alert('Hello world!', 'Title', {}, appContext)
```

Example 2 (ts):
```ts
import { ElMessageBox } from 'element-plus'
```

Example 3 (vue):
```vue
<template>
  <el-button plain @click="open">Click to open the Message Box</el-button>
</template>

<script lang="ts" setup>
import { ElMessage, ElMessageBox } from 'element-plus'

import type { Action } from 'element-plus'

const open = () => {
  ElMessageBox.alert('This is a message', 'Title', {
    // if you want to disable its autofocus
    // autofocus: false,
    confirmButtonText: 'OK',
    callback: (action: Action) => {
      ElMessage({
        type: 'info',
        message: `action: ${action}`,
      })
    },
  })
}
</script>
```

Example 4 (vue):
```vue
<template>
  <el-button plain @click="open">Click to open Message Box</el-button>
</template>

<script lang="ts" setup>
import { ElMessage, ElMessageBox } from 'element-plus'

const open = () => {
  ElMessageBox.confirm(
    'proxy will permanently delete the file. Continue?',
    'Warning',
    {
      confirmButtonText: 'OK',
      cancelButtonText: 'Cancel',
      type: 'warning',
      center: true,
    }
  )
    .then(() => {
      ElMessage({
        type: 'success',
        message: 'Delete completed',
      })
    })
    .catch(() => {
      ElMessage({
        type: 'info',
        message: 'Delete canceled',
      })
    })
}
</script>
```

---

## Form

**URL:** llms-txt#form

**Contents:**
- Basic Form
- Inline Form
- Alignment
- Validation
- Custom Validation Rules
- Add/Delete Form Item
- Number Validate
- Size Control
- Accessibility
- Form API

Form consists of `input`, `radio`, `select`, `checkbox` and so on. With form, you can collect, verify and submit data.

It includes all kinds of input items, such as `input`, `select`, `radio` and `checkbox`.

When the vertical space is limited and the form is relatively simple, you can put it in one line.

Depending on your design, there are several different ways to align your label element.

You can set `label-position` of `el-form-item` separately ^(2.7.7). If the value is empty, the `label-position` of `el-form` is used.

Form component allows you to verify your data, helping you find and correct errors.

## Custom Validation Rules

This example shows how to customize your own validation rules to finish a two-factor password verification.

## Add/Delete Form Item

All components in a Form inherit their `size` attribute from that Form. Similarly, FormItem also has a `size` attribute.

When only a single input (or related control such as select or checkbox) is inside of a `el-form-item`, the form item's label will automatically be attached to that input. However, if multiple inputs are inside of the `el-form-item`, the form item will be assigned the [WAI-ARIA](https://www.w3.org/WAI/standards-guidelines/aria/) role of [group](https://www.w3.org/TR/wai-aria/#group) instead. In this case, it is your responsibility to assign assistive labels to the individual inputs.

| Name                              | Description                                                                                                                                                                                                                      | Type                                           | Default |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | ------- |
| model                             | Data of form component.                                                                                                                                                                                                          | ^[object]`Record<string, any>`                 | —       |
| rules                             | Validation rules of form.                                                                                                                                                                                                        | ^[object]`FormRules`                           | —       |
| inline                            | Whether the form is inline.                                                                                                                                                                                                      | ^[boolean]                                     | false   |
| label-position                    | Position of label. If set to `'left'` or `'right'`, `label-width` prop is also required.                                                                                                                                         | ^[enum]`'left' \| 'right' \| 'top'`            | right   |
| label-width                       | Width of label, e.g. `'50px'`. All its direct child form items will inherit this value. `auto` is supported.                                                                                                                     | ^[string] / ^[number]                          | ''      |
| label-suffix                      | Suffix of the label.                                                                                                                                                                                                             | ^[string]                                      | ''      |
| hide-required-asterisk            | Whether to hide required fields should have a red asterisk (star) beside their labels.                                                                                                                                           | ^[boolean]                                     | false   |
| require-asterisk-position         | Position of asterisk.                                                                                                                                                                                                            | ^[enum]`'left' \| 'right'`                     | left    |
| show-message                      | Whether to show the error message.                                                                                                                                                                                               | ^[boolean]                                     | true    |
| inline-message                    | Whether to display the error message inline with the form item.                                                                                                                                                                  | ^[boolean]                                     | false   |
| status-icon                       | Whether to display an icon indicating the validation result.                                                                                                                                                                     | ^[boolean]                                     | false   |
| validate-on-rule-change           | Whether to trigger validation when the `rules` prop is changed.                                                                                                                                                                  | ^[boolean]                                     | true    |
| size                              | Control the size of components in this form.                                                                                                                                                                                     | ^[enum]`'' \| 'large' \| 'default' \| 'small'` | —       |
| disabled                          | Whether to disable all components in this form. Before ^(2.12.0), if set to `true`, it will override the `disabled` prop of the inner component. After ^(2.12.0), the configuration of the internal components takes precedence. | ^[boolean]                                     | false   |
| scroll-to-error                   | When validation fails, scroll to the first error form entry.                                                                                                                                                                     | ^[boolean]                                     | false   |
| scroll-into-view-options ^(2.3.2) | When validation fails, it scrolls to the first error item based on the scrollIntoView option. [scrollIntoView](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollIntoView).                                         | ^[object]`ScrollIntoViewOptions` / ^[boolean]  | true    |

| Name     | Description                             | Type                                                                         |
| -------- | --------------------------------------- | ---------------------------------------------------------------------------- |
| validate | triggers after a form item is validated | ^[Function]`(prop: FormItemProp, isValid: boolean, message: string) => void` |

| Name    | Description               | Subtags  |
| ------- | ------------------------- | -------- |
| default | customize default content | FormItem |

| Name               | Description                                                        | Type                                                                                                                              |
| ------------------ | ------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| validate           | Validate the whole form. Receives a callback or returns `Promise`. | ^[Function]`(callback?: FormValidateCallback) => Promise<void>`                                                                   |
| validateField      | Validate specified fields.                                         | ^[Function]`(props?: Arrayable<FormItemProp> \| undefined, callback?: FormValidateCallback \| undefined) => FormValidationResult` |
| resetFields        | Reset specified fields and remove validation result.               | ^[Function]`(props?: Arrayable<FormItemProp> \| undefined) => void`                                                               |
| scrollToField      | Scroll to the specified fields.                                    | ^[Function]`(prop: FormItemProp) => void`                                                                                         |
| clearValidate      | Clear validation messages for all or specified fields.             | ^[Function]`(props?: Arrayable<FormItemProp> \| undefined) => void`                                                               |
| fields ^(2.7.3)    | Get all fields context.                                            | ^[array]`FormItemContext[]`                                                                                                       |
| getField ^(2.10.2) | Get a field context.                                               | ^[Function]`(prop: FormItemProp) => FormItemContext \| undefined`                                                                 |

### FormItem Attributes

| Name                    | Description                                                                                                                                                            | Type                                                | Default |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- | ------- |
| prop                    | A key of `model`. It could be a path of the property (e.g `a.b.0` or `['a', 'b', '0']`). In the use of `validate` and `resetFields` method, the attribute is required. | ^[string] / ^[string&#91;&#93;]                     | —       |
| label                   | Label text.                                                                                                                                                            | ^[string]                                           | —       |
| label-position ^(2.7.7) | Position of item label. If set to `'left'` or `'right'`, `label-width` prop is also required. Default extend `label-position` of `form`.                               | ^[enum]`'left' \| 'right' \| 'top'`                 | ''      |
| label-width             | Width of label, e.g. `'50px'`. `'auto'` is supported.                                                                                                                  | ^[string] / ^[number]                               | —       |
| required                | Whether the field is required or not, will be determined by validation rules if omitted.                                                                               | ^[boolean]                                          | —       |
| rules                   | Validation rules of form, see the [following table](#formitemrule), more advanced usage at [async-validator](https://github.com/yiminghe/async-validator).             | ^[object]`Arrayable<FormItemRule>`                  | —       |
| error                   | Field error message, set its value and the field will validate error and show this message immediately.                                                                | ^[string]                                           | —       |
| show-message            | Whether to show the error message.                                                                                                                                     | ^[boolean]                                          | true    |
| inline-message          | Inline style validate message.                                                                                                                                         | ^[boolean]                                          | false   |
| size                    | Control the size of components in this form-item.                                                                                                                      | ^[enum]`'' \| 'large' \| 'default' \| 'small'`      | —       |
| for                     | Same as for in native label.                                                                                                                                           | ^[string]                                           | —       |
| validate-status         | Validation state of formItem.                                                                                                                                          | ^[enum]`'' \| 'error' \| 'validating' \| 'success'` | —       |

| Name    | Description                     | Type                        | Default |
| ------- | ------------------------------- | --------------------------- | ------- |
| trigger | How the validator is triggered. | ^[enum]`'blur' \| 'change'` | —       |

| Name    | Description                                   | Type                         |
| ------- | --------------------------------------------- | ---------------------------- |
| default | Content of Form Item.                         | —                            |
| label   | Custom content to display on label.           | ^[object]`{ label: string }` |
| error   | Custom content to display validation message. | ^[object]`{ error: string }` |

| Name            | Description                                       | Type                                                                                                 |
| --------------- | ------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| size            | Form item size.                                   | ^[object]`ComputedRef<'' \| 'large' \| 'default' \| 'small'>`                                        |
| validateMessage | Validation message.                               | ^[object]`Ref<string>`                                                                               |
| validateState   | Validation state.                                 | ^[object]`Ref<'' \| 'error' \| 'validating' \| 'success'>`                                           |
| validate        | Validate form item.                               | ^[Function]`(trigger: string, callback?: FormValidateCallback \| undefined) => FormValidationResult` |
| resetField      | Reset current field and remove validation result. | ^[Function]`() => void`                                                                              |
| clearValidate   | Remove validation status of the field.            | ^[Function]`() => void`                                                                              |

<details>
  <summary>Show declarations</summary>

### accessibility.vue

### custom-validation.vue

### number-validate.vue

---
Title: Icon
URL: https://element-plus.org/en-US/component/icon
---

**Examples:**

Example 1 (ts):
```ts
type Arrayable<T> = T | T[]

type FormValidationResult = Promise<boolean>

// ValidateFieldsError: see [async-validator](https://github.com/yiminghe/async-validator/blob/master/src/interface.ts)
type FormValidateCallback = (
  isValid: boolean,
  invalidFields?: ValidateFieldsError
) => Promise<void> | void

// RuleItem: see [async-validator](https://github.com/yiminghe/async-validator/blob/master/src/interface.ts)
interface FormItemRule extends RuleItem {
  trigger?: Arrayable<string>
}

type Primitive = null | undefined | string | number | boolean | symbol | bigint
type BrowserNativeObject = Date | FileList | File | Blob | RegExp
type IsTuple<T extends ReadonlyArray<any>> = number extends T['length']
  ? false
  : true
type ArrayMethodKey = keyof any[]
type TupleKey<T extends ReadonlyArray<any>> = Exclude<keyof T, ArrayMethodKey>
type ArrayKey = number
type PathImpl<K extends string | number, V> = V extends
  | Primitive
  | BrowserNativeObject
  ? `${K}`
  : `${K}` | `${K}.${Path<V>}`
type Path<T> =
  T extends ReadonlyArray<infer V>
    ? IsTuple<T> extends true
      ? {
          [K in TupleKey<T>]-?: PathImpl<Exclude<K, symbol>, T[K]>
        }[TupleKey<T>]
      : PathImpl<ArrayKey, V>
    : {
        [K in keyof T]-?: PathImpl<Exclude<K, symbol>, T[K]>
      }[keyof T]
type FieldPath<T> = T extends object ? Path<T> : never
// MaybeRef: see [@vueuse/core](https://github.com/vueuse/vueuse/blob/main/packages/shared/utils/types.ts)
// UnwrapRef: see [vue](https://github.com/vuejs/core/blob/main/packages/reactivity/src/ref.ts)
type FormRules<T extends MaybeRef<Record<string, any> | string> = string> =
  Partial<
    Record<
      UnwrapRef<T> extends string ? UnwrapRef<T> : FieldPath<UnwrapRef<T>>,
      Arrayable<FormItemRule>
    >
  >

type FormItemValidateState = (typeof formItemValidateStates)[number]
type FormItemProps = ExtractPropTypes<typeof formItemProps>

type FormItemContext = FormItemProps & {
  $el: HTMLDivElement | undefined
  size: ComponentSize
  validateMessage: string
  validateState: FormItemValidateState
  isGroup: boolean
  labelId: string
  inputIds: string[]
  hasLabel: boolean
  fieldValue: any
  propString: string
  addInputId: (id: string) => void
  removeInputId: (id: string) => void
  validate: (
    trigger: string,
    callback?: FormValidateCallback
  ) => FormValidationResult
  resetField(): void
  clearValidate(): void
}
```

Example 2 (vue):
```vue
<template>
  <el-form label-position="left" label-width="auto" style="max-width: 600px">
    <el-space fill>
      <el-alert type="info" show-icon :closable="false">
        <p>"Full Name" label is automatically attached to the input:</p>
      </el-alert>
      <el-form-item label="Full Name">
        <el-input v-model="formAccessibility.fullName" />
      </el-form-item>
    </el-space>
    <el-space fill>
      <el-alert type="info" show-icon :closable="false">
        <p>
          "Your Information" serves as a label for the group of inputs. <br />
          You must specify labels on the individal inputs. Placeholders are not
          replacements for using the "label" attribute.
        </p>
      </el-alert>
      <el-form-item label="Your Information">
        <el-row :gutter="20">
          <el-col :span="12">
            <el-input
              v-model="formAccessibility.firstName"
              aria-label="First Name"
              placeholder="First Name"
            />
          </el-col>
          <el-col :span="12">
            <el-input
              v-model="formAccessibility.lastName"
              aria-label="Last Name"
              placeholder="Last Name"
            />
          </el-col>
        </el-row>
      </el-form-item>
    </el-space>
  </el-form>
</template>

<script lang="ts" setup>
import { reactive } from 'vue'

const formAccessibility = reactive({
  fullName: '',
  firstName: '',
  lastName: '',
})
</script>
```

Example 3 (vue):
```vue
<template>
  <el-form
    :label-position="labelPosition"
    label-width="auto"
    :model="formLabelAlign"
    style="max-width: 600px"
  >
    <el-form-item label="Form Align" label-position="right">
      <el-radio-group v-model="labelPosition" aria-label="label position">
        <el-radio-button value="left">Left</el-radio-button>
        <el-radio-button value="right">Right</el-radio-button>
        <el-radio-button value="top">Top</el-radio-button>
      </el-radio-group>
    </el-form-item>
    <el-form-item label="Form Item Align" label-position="right">
      <el-radio-group
        v-model="itemLabelPosition"
        aria-label="item label position"
      >
        <el-radio-button value="">Empty</el-radio-button>
        <el-radio-button value="left">Left</el-radio-button>
        <el-radio-button value="right">Right</el-radio-button>
        <el-radio-button value="top">Top</el-radio-button>
      </el-radio-group>
    </el-form-item>
    <el-form-item label="Name" :label-position="itemLabelPosition">
      <el-input v-model="formLabelAlign.name" />
    </el-form-item>
    <el-form-item label="Activity zone" :label-position="itemLabelPosition">
      <el-input v-model="formLabelAlign.region" />
    </el-form-item>
    <el-form-item label="Activity form" :label-position="itemLabelPosition">
      <el-input v-model="formLabelAlign.type" />
    </el-form-item>
  </el-form>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue'

import type { FormItemProps, FormProps } from 'element-plus'

const labelPosition = ref<FormProps['labelPosition']>('right')
const itemLabelPosition = ref<FormItemProps['labelPosition']>('')
const formLabelAlign = reactive({
  name: '',
  region: '',
  type: '',
})
</script>
```

Example 4 (vue):
```vue
<template>
  <el-form :model="form" label-width="auto" style="max-width: 600px">
    <el-form-item label="Activity name">
      <el-input v-model="form.name" />
    </el-form-item>
    <el-form-item label="Activity zone">
      <el-select v-model="form.region" placeholder="please select your zone">
        <el-option label="Zone one" value="shanghai" />
        <el-option label="Zone two" value="beijing" />
      </el-select>
    </el-form-item>
    <el-form-item label="Activity time">
      <el-col :span="11">
        <el-date-picker
          v-model="form.date1"
          type="date"
          placeholder="Pick a date"
          style="width: 100%"
        />
      </el-col>
      <el-col :span="2" class="text-center">
        <span class="text-gray-500">-</span>
      </el-col>
      <el-col :span="11">
        <el-time-picker
          v-model="form.date2"
          placeholder="Pick a time"
          style="width: 100%"
        />
      </el-col>
    </el-form-item>
    <el-form-item label="Instant delivery">
      <el-switch v-model="form.delivery" />
    </el-form-item>
    <el-form-item label="Activity type">
      <el-checkbox-group v-model="form.type">
        <el-checkbox value="Online activities" name="type">
          Online activities
        </el-checkbox>
        <el-checkbox value="Promotion activities" name="type">
          Promotion activities
        </el-checkbox>
        <el-checkbox value="Offline activities" name="type">
          Offline activities
        </el-checkbox>
        <el-checkbox value="Simple brand exposure" name="type">
          Simple brand exposure
        </el-checkbox>
      </el-checkbox-group>
    </el-form-item>
    <el-form-item label="Resources">
      <el-radio-group v-model="form.resource">
        <el-radio value="Sponsor">Sponsor</el-radio>
        <el-radio value="Venue">Venue</el-radio>
      </el-radio-group>
    </el-form-item>
    <el-form-item label="Activity form">
      <el-input v-model="form.desc" type="textarea" />
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="onSubmit">Create</el-button>
      <el-button>Cancel</el-button>
    </el-form-item>
  </el-form>
</template>

<script lang="ts" setup>
import { reactive } from 'vue'

// do not use same name with ref
const form = reactive({
  name: '',
  region: '',
  date1: '',
  date2: '',
  delivery: false,
  type: [],
  resource: '',
  desc: '',
})

const onSubmit = () => {
  console.log('submit!')
}
</script>
```

---

## Autocomplete

**URL:** llms-txt#autocomplete

**Contents:**
- Basic Usage
- Custom template
- Remote search
- Custom Loading ^(2.5.0)
- Custom Header & Footer ^(2.10.6)
- API
  - Attributes
  - Events
  - Slots
  - Exposes

Get some recommended tips based on the current input.

Autocomplete component provides input suggestions.

Customize how suggestions are displayed.

Search data from server-side.

## Custom Loading ^(2.5.0)

Override loading content.

## Custom Header & Footer ^(2.10.6)

You can customize both the header and footer of the dropdown using slots

| Name                                 | Description                                                                                                                | Type                                                                                      | Default      |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ------------ |
| model-value / v-model                | binding value                                                                                                              | ^[string]                                                                                 | —            |
| placeholder                          | the placeholder of Autocomplete                                                                                            | ^[string]                                                                                 | —            |
| clearable                            | whether to show clear button                                                                                               | ^[boolean]                                                                                | false        |
| disabled                             | whether Autocomplete is disabled                                                                                           | ^[boolean]                                                                                | false        |
| value-key                            | key name of the input suggestion object for display                                                                        | ^[string]                                                                                 | value        |
| debounce                             | debounce delay when typing, in milliseconds                                                                                | ^[number]                                                                                 | 300          |
| placement                            | placement of the popup menu                                                                                                | ^[enum]`'top' \| 'top- start' \| 'top-end' \| 'bottom' \| 'bottom-start' \| 'bottom-end'` | bottom-start |
| fetch-suggestions                    | a method to fetch input suggestions. When suggestions are ready, invoke `callback(data:[])` to return them to Autocomplete | ^[Array] / ^[Function]`(queryString: string, callback: callbackfn) => void`               | —            |
| trigger-on-focus                     | whether show suggestions when input focus                                                                                  | ^[boolean]                                                                                | true         |
| select-when-unmatched                | whether to emit a `select` event on enter when there is no autocomplete match                                              | ^[boolean]                                                                                | false        |
| name                                 | same as `name` in native input                                                                                             | ^[string]                                                                                 | —            |
| aria-label ^(a11y) ^(2.7.2)          | native `aria-label` attribute                                                                                              | ^[string]                                                                                 | —            |
| hide-loading                         | whether to hide the loading icon in remote search                                                                          | ^[boolean]                                                                                | false        |
| popper-class                         | custom class name for autocomplete's dropdown                                                                              | ^[string] / ^[object]                                                                     | ''           |
| popper-style ^(2.11.4)               | custom style for autocomplete's dropdown                                                                                   | ^[string] / ^[object]                                                                     | —            |
| teleported                           | whether select dropdown is teleported to the body                                                                          | ^[boolean]                                                                                | true         |
| append-to ^(2.9.9)                   | which select dropdown appends to                                                                                           | ^[CSSSelector] / ^[HTMLElement]                                                           | —            |
| highlight-first-item                 | whether to highlight first item in remote search suggestions by default                                                    | ^[boolean]                                                                                | false        |
| fit-input-width                      | whether the width of the dropdown is the same as the input                                                                 | ^[boolean]                                                                                | false        |
| popper-append-to-body ^(deprecated)  | whether to append the dropdown to body. If the positioning of the dropdown is wrong, you can try to set this prop to false | ^[boolean]                                                                                | false        |
| loop-navigation ^(2.11.4)            | whether keyboard navigation loops from end to start                                                                        | ^[boolean]                                                                                | true         |
| [input props](./input.md#attributes) | —                                                                                                                          | —                                                                                         | —            |

| Name   | Description                                                     | Type                                             |
| ------ | --------------------------------------------------------------- | ------------------------------------------------ |
| blur   | triggers when Input blurs                                       | ^[Function]`(event: FocusEvent) => void`         |
| focus  | triggers when Input focuses                                     | ^[Function]`(event: FocusEvent) => void`         |
| input  | triggers when the Input value change                            | ^[Function]`(value: string \| number) => void`   |
| clear  | triggers when the Input is cleared by clicking the clear button | ^[Function]`() => void`                          |
| select | triggers when a suggestion is clicked                           | ^[Function]`(item: Record<string, any>) => void` |
| change | triggers when the icon inside Input value change                | ^[Function]`(value: string \| number) => void`   |

| Name             | Description                           | Type                                     |
| ---------------- | ------------------------------------- | ---------------------------------------- |
| default          | custom content for input suggestions  | ^[object]`{ item: Record<string, any> }` |
| header ^(2.10.6) | content at the top of the dropdown    | -                                        |
| footer ^(2.10.6) | content at the bottom of the dropdown | -                                        |
| prefix           | content as Input prefix               | -                                        |
| suffix           | content as Input suffix               | -                                        |
| prepend          | content to prepend before Input       | -                                        |
| append           | content to append after Input         | -                                        |
| loading ^(2.5.0) | override loading content              | -                                        |

| Name             | Description                                 | Type                                                |
| ---------------- | ------------------------------------------- | --------------------------------------------------- |
| activated        | if autocomplete activated                   | ^[object]`Ref<boolean>`                             |
| blur             | blur the input element                      | ^[Function]`() => void`                             |
| close            | collapse suggestion list                    | ^[Function]`() => void`                             |
| focus            | focus the input element                     | ^[Function]`() => void`                             |
| handleSelect     | triggers when a suggestion is clicked       | ^[Function]`(item: any) => promise<void>`           |
| handleKeyEnter   | handle keyboard enter event                 | ^[Function]`() => promise<void>`                    |
| highlightedIndex | the index of the currently highlighted item | ^[object]`Ref<number>`                              |
| highlight        | highlight an item in a suggestion           | ^[Function]`(itemIndex: number) => void`            |
| inputRef         | el-input component instance                 | ^[object]`Ref<ElInputInstance>`                     |
| loading          | remote search loading indicator             | ^[object]`Ref<boolean>`                             |
| popperRef        | el-tooltip component instance               | ^[object]`Ref<ElTooltipInstance>`                   |
| suggestions      | fetch suggestions result                    | ^[object]`Ref<record<string, any>[]>`               |
| getData ^(2.8.4) | loading suggestion list                     | ^[Function]`(queryString: string) => promise<void>` |

### autocomplete-template.vue

### custom-header-footer.vue

### custom-loading.vue

### remote-search.vue

---
Title: Avatar
URL: https://element-plus.org/en-US/component/avatar
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-autocomplete
    v-model="state"
    :fetch-suggestions="querySearch"
    popper-class="my-autocomplete"
    placeholder="Please input"
    @select="handleSelect"
  >
    <template #suffix>
      <el-icon class="el-input__icon" @click="handleIconClick">
        <edit />
      </el-icon>
    </template>
    <template #default="{ item }">
      <div class="value">{{ item.value }}</div>
      <span class="link">{{ item.link }}</span>
    </template>
  </el-autocomplete>
</template>

<script lang="ts" setup>
import { onMounted, ref } from 'vue'
import { Edit } from '@element-plus/icons-vue'

interface LinkItem {
  value: string
  link: string
}

const state = ref('')
const links = ref<LinkItem[]>([])

const querySearch = (queryString: string, cb) => {
  const results = queryString
    ? links.value.filter(createFilter(queryString))
    : links.value
  // call callback function to return suggestion objects
  cb(results)
}
const createFilter = (queryString: string) => {
  return (restaurant: LinkItem) => {
    return (
      restaurant.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0
    )
  }
}
const loadAll = () => {
  return [
    { value: 'vue', link: 'https://github.com/vuejs/vue' },
    { value: 'element', link: 'https://github.com/ElemeFE/element' },
    { value: 'cooking', link: 'https://github.com/ElemeFE/cooking' },
    { value: 'mint-ui', link: 'https://github.com/ElemeFE/mint-ui' },
    { value: 'vuex', link: 'https://github.com/vuejs/vuex' },
    { value: 'vue-router', link: 'https://github.com/vuejs/vue-router' },
    { value: 'babel', link: 'https://github.com/babel/babel' },
  ]
}
const handleSelect = (item: Record<string, any>) => {
  console.log(item)
}

const handleIconClick = (ev: Event) => {
  console.log(ev)
}

onMounted(() => {
  links.value = loadAll()
})
</script>

<style>
.my-autocomplete li {
  line-height: normal;
  padding: 7px;
}
.my-autocomplete li .name {
  text-overflow: ellipsis;
  overflow: hidden;
}
.my-autocomplete li .addr {
  font-size: 12px;
  color: #b4b4b4;
}
.my-autocomplete li .highlighted .addr {
  color: #ddd;
}
</style>
```

Example 2 (vue):
```vue
<template>
  <div class="demo-autocomplete">
    <div class="demo-block">
      <div class="demo-title">list suggestions when activated</div>
      <el-autocomplete
        v-model="state1"
        :fetch-suggestions="querySearch"
        clearable
        class="w-50"
        placeholder="Please Input"
        @select="handleSelect"
      />
    </div>
    <div class="demo-block">
      <div class="demo-title">list suggestions on input</div>
      <el-autocomplete
        v-model="state2"
        :fetch-suggestions="querySearch"
        :trigger-on-focus="false"
        clearable
        class="w-50"
        placeholder="Please Input"
        @select="handleSelect"
      />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { onMounted, ref } from 'vue'

interface RestaurantItem {
  value: string
  link: string
}

const state1 = ref('')
const state2 = ref('')

const restaurants = ref<RestaurantItem[]>([])
const querySearch = (queryString: string, cb: any) => {
  const results = queryString
    ? restaurants.value.filter(createFilter(queryString))
    : restaurants.value
  // call callback function to return suggestions
  cb(results)
}
const createFilter = (queryString: string) => {
  return (restaurant: RestaurantItem) => {
    return (
      restaurant.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0
    )
  }
}
const loadAll = () => {
  return [
    { value: 'vue', link: 'https://github.com/vuejs/vue' },
    { value: 'element', link: 'https://github.com/ElemeFE/element' },
    { value: 'cooking', link: 'https://github.com/ElemeFE/cooking' },
    { value: 'mint-ui', link: 'https://github.com/ElemeFE/mint-ui' },
    { value: 'vuex', link: 'https://github.com/vuejs/vuex' },
    { value: 'vue-router', link: 'https://github.com/vuejs/vue-router' },
    { value: 'babel', link: 'https://github.com/babel/babel' },
  ]
}

const handleSelect = (item: Record<string, any>) => {
  console.log(item)
}

onMounted(() => {
  restaurants.value = loadAll()
})
</script>

<style scoped>
.demo-autocomplete {
  display: flex;
  flex-wrap: wrap;
  gap: 2rem;
}

.demo-block {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.demo-title {
  font-size: 0.875rem;
  color: var(--el-text-color-secondary);
  min-height: 2.5em;
  display: flex;
  align-items: center;
}

@media screen and (max-width: 768px) {
  .demo-autocomplete {
    gap: 1rem;
  }

  .demo-block {
    width: 100%;
  }
}
</style>
```

Example 3 (vue):
```vue
<template>
  <div class="autocomplete-custom-header-footer">
    <div>
      <p>Custom header content</p>
      <el-autocomplete
        v-model="headerSlotState"
        :fetch-suggestions="querySearchAsync"
        placeholder="Please input"
        @select="handleSelect"
      >
        <template #header>header content</template>
      </el-autocomplete>
    </div>
    <div>
      <p>Custom footer content</p>
      <el-autocomplete
        ref="footerAutocompleteRef"
        v-model="footerSlotstate"
        :fetch-suggestions="querySearchAsync"
        placeholder="Please input"
        @select="handleSelect"
      >
        <template #footer>
          <el-button link size="small" @click="handleClear"> Clear </el-button>
        </template>
      </el-autocomplete>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { onMounted, ref } from 'vue'

const headerSlotState = ref('')
const footerSlotstate = ref('')

interface LinkItem {
  value: string
  link: string
}

const links = ref<LinkItem[]>([])

const loadAll = () => {
  return [
    { value: 'vue', link: 'https://github.com/vuejs/vue' },
    { value: 'element', link: 'https://github.com/ElemeFE/element' },
    { value: 'cooking', link: 'https://github.com/ElemeFE/cooking' },
    { value: 'mint-ui', link: 'https://github.com/ElemeFE/mint-ui' },
    { value: 'vuex', link: 'https://github.com/vuejs/vuex' },
    { value: 'vue-router', link: 'https://github.com/vuejs/vue-router' },
    { value: 'babel', link: 'https://github.com/babel/babel' },
  ]
}

let timeout: ReturnType<typeof setTimeout>
const querySearchAsync = (queryString: string, cb: (arg: any) => void) => {
  const results = queryString
    ? links.value.filter(createFilter(queryString))
    : links.value

  clearTimeout(timeout)
  timeout = setTimeout(() => {
    cb(results)
  }, 3000 * Math.random())
}
const createFilter = (queryString: string) => {
  return (restaurant: LinkItem) => {
    return (
      restaurant.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0
    )
  }
}

const handleSelect = (item: Record<string, any>) => {
  console.log(item)
}

onMounted(() => {
  links.value = loadAll()
})

const footerAutocompleteRef = ref()
const handleClear = () => {
  footerSlotstate.value = ''
  footerAutocompleteRef.value.getData()
}
</script>

<style scoped>
.autocomplete-custom-header-footer {
  display: flex;
}

.autocomplete-custom-header-footer > div {
  flex: 1;
  text-align: center;
}
.autocomplete-custom-header-footer > div > :deep(.el-autocomplete) {
  width: 50%;
}

.autocomplete-custom-header-footer > div:not(:last-child) {
  border-right: 1px solid var(--el-border-color);
}
</style>
```

Example 4 (vue):
```vue
<template>
  <div class="demo-autocomplete">
    <div class="demo-block">
      <div class="demo-title">loading icon1</div>
      <el-autocomplete
        v-model="state"
        :fetch-suggestions="querySearchAsync"
        class="w-50"
        placeholder="Please input"
        @select="handleSelect"
      >
        <template #loading>
          <svg class="circular" viewBox="0 0 50 50">
            <circle class="path" cx="25" cy="25" r="20" fill="none" />
          </svg>
        </template>
      </el-autocomplete>
    </div>
    <div class="demo-block">
      <div class="demo-title">loading icon2</div>
      <el-autocomplete
        v-model="state"
        :fetch-suggestions="querySearchAsync"
        class="w-50"
        placeholder="Please input"
        @select="handleSelect"
      >
        <template #loading>
          <el-icon class="is-loading">
            <svg class="circular" viewBox="0 0 20 20">
              <g
                class="path2 loading-path"
                stroke-width="0"
                style="animation: none; stroke: none"
              >
                <circle r="3.375" class="dot1" rx="0" ry="0" />
                <circle r="3.375" class="dot2" rx="0" ry="0" />
                <circle r="3.375" class="dot4" rx="0" ry="0" />
                <circle r="3.375" class="dot3" rx="0" ry="0" />
              </g>
            </svg>
          </el-icon>
        </template>
      </el-autocomplete>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { onMounted, ref } from 'vue'

const state = ref('')

interface LinkItem {
  value: string
  link: string
}

const links = ref<LinkItem[]>([])

const loadAll = () => {
  return [
    { value: 'vue', link: 'https://github.com/vuejs/vue' },
    { value: 'element', link: 'https://github.com/ElemeFE/element' },
    { value: 'cooking', link: 'https://github.com/ElemeFE/cooking' },
    { value: 'mint-ui', link: 'https://github.com/ElemeFE/mint-ui' },
    { value: 'vuex', link: 'https://github.com/vuejs/vuex' },
    { value: 'vue-router', link: 'https://github.com/vuejs/vue-router' },
    { value: 'babel', link: 'https://github.com/babel/babel' },
  ]
}

let timeout: ReturnType<typeof setTimeout>
const querySearchAsync = (queryString: string, cb: (arg: any) => void) => {
  const results = queryString
    ? links.value.filter(createFilter(queryString))
    : links.value

  clearTimeout(timeout)
  timeout = setTimeout(() => {
    cb(results)
  }, 5000 * Math.random())
}
const createFilter = (queryString: string) => {
  return (restaurant: LinkItem) => {
    return (
      restaurant.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0
    )
  }
}

const handleSelect = (item: Record<string, any>) => {
  console.log(item)
}

onMounted(() => {
  links.value = loadAll()
})
</script>

<style scoped>
.demo-autocomplete {
  display: flex;
  flex-wrap: wrap;
  gap: 2rem;
}

.demo-block {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.demo-title {
  font-size: 0.875rem;
  color: var(--el-text-color-secondary);
  min-height: 2.5em;
  display: flex;
  align-items: center;
}

@media screen and (max-width: 768px) {
  .demo-autocomplete {
    gap: 1rem;
  }
  .demo-block {
    width: 100%;
  }
}
</style>

<style>
.circular {
  display: inline;
  height: 30px;
  width: 30px;
  animation: loading-rotate 2s linear infinite;
}
.path {
  animation: loading-dash 1.5s ease-in-out infinite;
  stroke-dasharray: 90, 150;
  stroke-dashoffset: 0;
  stroke-width: 2;
  stroke: var(--el-color-primary);
  stroke-linecap: round;
}
.loading-path .dot1 {
  transform: translate(3.75px, 3.75px);
  fill: var(--el-color-primary);
  animation: custom-spin-move 1s infinite linear alternate;
  opacity: 0.3;
}
.loading-path .dot2 {
  transform: translate(calc(100% - 3.75px), 3.75px);
  fill: var(--el-color-primary);
  animation: custom-spin-move 1s infinite linear alternate;
  opacity: 0.3;
  animation-delay: 0.4s;
}
.loading-path .dot3 {
  transform: translate(3.75px, calc(100% - 3.75px));
  fill: var(--el-color-primary);
  animation: custom-spin-move 1s infinite linear alternate;
  opacity: 0.3;
  animation-delay: 1.2s;
}
.loading-path .dot4 {
  transform: translate(calc(100% - 3.75px), calc(100% - 3.75px));
  fill: var(--el-color-primary);
  animation: custom-spin-move 1s infinite linear alternate;
  opacity: 0.3;
  animation-delay: 0.8s;
}
@keyframes loading-rotate {
  to {
    transform: rotate(360deg);
  }
}
@keyframes loading-dash {
  0% {
    stroke-dasharray: 1, 200;
    stroke-dashoffset: 0;
  }
  50% {
    stroke-dasharray: 90, 150;
    stroke-dashoffset: -40px;
  }
  100% {
    stroke-dasharray: 90, 150;
    stroke-dashoffset: -120px;
  }
}
@keyframes custom-spin-move {
  to {
    opacity: 1;
  }
}
</style>
```

---

## Cascader

**URL:** llms-txt#cascader

**Contents:**
- Basic usage
- Disabled option
- Clearable
- Custom Clear Icon ^(2.11.0)
- Display only the last level
- Multiple Selection
- Select any level of options
- Dynamic loading
- Filterable
- Custom option content

If the options have a clear hierarchical structure, Cascader can be used to view and select them.

There are two ways to expand child option items.

Disable an option by setting a `disabled` field in the option object.

Set `clearable` attribute for `el-cascader` and a clear icon will appear when selected and hovered

## Custom Clear Icon ^(2.11.0)

You can customize the clear icon by setting the `clear-icon` attribute

## Display only the last level

The input can display only the last level instead of all levels.

## Multiple Selection

Add `:props="props"` in tag and set data `props = { multiple: true }` to use multiple selection.

## Select any level of options

In single selection, only the leaf nodes can be checked, and in multiple selection, check parent nodes will lead to leaf nodes be checked eventually. When enable this feature, it can make parent and child nodes unlinked and you can select any level of options.

Dynamic load its child nodes when checked a node.

Search and select options with a keyword.

## Custom option content

You can customize the content of cascader node.

## Custom suggestion item ^(2.9.5)

You can customize the filter suggestion item by `suggestion-item` slot. You'll have access to `item` in the scope, standing for the suggestion item.

`CascaderPanel` is the core component of `Cascader` which has various of features such as single selection, multiple selection, dynamic loading and so on.

## Custom Tag ^(2.10.3)

You can customize tags.

## Show Checked Strategy ^(2.10.5)

Control how selected values are displayed in multiple selection mode.

## Click to Check Node ^(2.10.5)

Only using `multiple` or `checkStrictly` attributes.

You can add `checkOnClickNode` to be able to click on the node in addition with the prefix icon.\
Toggle the visibility of the prefix with `showPrefix`.

## Custom Header & Footer ^(2.10.5)

You can customize both the header and footer of the dropdown using slots.

### Cascader Attributes

| Name                                       | Description                                                                                                                                                                      | Type                                                                                                                                                                        | Default      |
| ------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| model-value / v-model                      | binding value                                                                                                                                                                    | ^[string] / ^[number] /^[object]`string[] \| number[] \| any`                                                                                                               | —            |
| options                                    | data of the options, the key of `value` and `label` can be customize by `CascaderProps`.                                                                                         | ^[object]`CascaderOption[]`                                                                                                                                                 | —            |
| [props](#cascaderprops)                    | configuration options, see the following `CascaderProps` table.                                                                                                                  | ^[object]`CascaderProps`                                                                                                                                                    | —            |
| size                                       | size of input                                                                                                                                                                    | ^[enum]`'large' \| 'default' \| 'small'`                                                                                                                                    | —            |
| placeholder                                | placeholder of input                                                                                                                                                             | ^[string]                                                                                                                                                                   | —            |
| disabled                                   | whether Cascader is disabled                                                                                                                                                     | ^[boolean]                                                                                                                                                                  | —            |
| clearable                                  | whether selected value can be cleared                                                                                                                                            | ^[boolean]                                                                                                                                                                  | —            |
| clear-icon ^(2.11.0)                       | custom clear icon component                                                                                                                                                      | ^[string] / ^[object]`Component`                                                                                                                                            | CircleClose  |
| show-all-levels                            | whether to display all levels of the selected value in the input                                                                                                                 | ^[boolean]                                                                                                                                                                  | true         |
| collapse-tags                              | whether to collapse tags in multiple selection mode                                                                                                                              | ^[boolean]                                                                                                                                                                  | —            |
| collapse-tags-tooltip                      | whether show all selected tags when mouse hover text of collapse-tags. To use this, `collapse-tags` must be true                                                                 | ^[boolean]                                                                                                                                                                  | false        |
| max-collapse-tags-tooltip-height ^(2.10.2) | max height of collapse-tags tooltip.                                                                                                                                             | ^[string] / ^[number]                                                                                                                                                       | —            |
| separator                                  | option label separator                                                                                                                                                           | ^[string]                                                                                                                                                                   | ' / '        |
| filterable                                 | whether the options can be searched                                                                                                                                              | ^[boolean]                                                                                                                                                                  | —            |
| filter-method                              | customize search logic, the first parameter is `node`, the second is `keyword`, and need return a boolean value indicating whether it hits.                                      | ^[Function]`(node: CascaderNode, keyword: string) => boolean`                                                                                                               | —            |
| debounce                                   | debounce delay when typing filter keyword, in milliseconds                                                                                                                       | ^[number]                                                                                                                                                                   | 300          |
| before-filter                              | hook function before filtering with the value to be filtered as its parameter. If `false` is returned or a `Promise` is returned and then is rejected, filtering will be aborted | ^[Function]`(value: string) => boolean`                                                                                                                                     | —            |
| popper-class                               | custom class name for Cascader's dropdown and tags' tooltip                                                                                                                      | ^[string]                                                                                                                                                                   | ''           |
| popper-style                               | custom style for Cascader's dropdown and tags' tooltip                                                                                                                           | ^[string] / ^[object]                                                                                                                                                       | —            |
| teleported                                 | whether cascader popup is teleported                                                                                                                                             | ^[boolean]                                                                                                                                                                  | true         |
| effect ^(2.10.5)                           | tooltip theme, built-in theme: `dark` / `light`                                                                                                                                  | ^[enum]`'dark' \| 'light'` / ^[string]                                                                                                                                      | light        |
| tag-type                                   | tag type                                                                                                                                                                         | ^[enum]`'success' \| 'info' \| 'warning' \| 'danger'`                                                                                                                       | info         |
| tag-effect ^(2.7.8)                        | tag effect                                                                                                                                                                       | ^[enum]`'light' \| 'dark' \| 'plain'`                                                                                                                                       | light        |
| validate-event                             | whether to trigger form validation                                                                                                                                               | ^[boolean]                                                                                                                                                                  | true         |
| max-collapse-tags ^(2.3.10)                | The max tags number to be shown. To use this, `collapse-tags` must be true                                                                                                       | ^[number]                                                                                                                                                                   | 1            |
| empty-values ^(2.7.0)                      | empty values of component, [see config-provider](/en-US/component/config-provider#empty-values-configurations)                                                                   | ^[array]                                                                                                                                                                    | —            |
| value-on-clear ^(2.7.0)                    | clear return value, [see config-provider](/en-US/component/config-provider#empty-values-configurations)                                                                          | ^[string] / ^[number] / ^[boolean] / ^[Function]                                                                                                                            | —            |
| persistent ^(2.7.8)                        | when dropdown is inactive and `persistent` is `false`, dropdown will be destroyed                                                                                                | ^[boolean]                                                                                                                                                                  | true         |
| fallback-placements ^(2.8.1)               | list of possible positions for Tooltip [popper.js](https://popper.js.org/docs/v2/modifiers/flip/#fallbackplacements)                                                             | ^[array]`Placement[]`                                                                                                                                                       | —            |
| placement ^(2.8.1)                         | position of dropdown                                                                                                                                                             | ^[enum]`'top' \| 'top-start' \| 'top-end' \| 'bottom' \| 'bottom-start' \| 'bottom-end' \| 'left' \| 'left-start' \| 'left-end' \| 'right' \| 'right-start' \| 'right-end'` | bottom-start |
| popper-append-to-body ^(deprecated)        | whether to append the popper menu to body. If the positioning of the popper is wrong, you can try to set this prop to false                                                      | ^[boolean]                                                                                                                                                                  | true         |
| show-checked-strategy ^(2.10.5)            | strategy for displaying checked nodes in multiple selection mode. Use `parent` when you want things tidy. Use `child` when every single item matters                             | ^[enum]`'parent' \| 'child'`                                                                                                                                                | child        |

| Name           | Description                                                   | Type                                                        |
| -------------- | ------------------------------------------------------------- | ----------------------------------------------------------- |
| change         | triggers when the binding value changes                       | ^[Function]`(value: CascaderValue) => void`                 |
| expand-change  | triggers when expand option changes                           | ^[Function]`(value: CascaderValue) => void`                 |
| blur           | triggers when Cascader blurs                                  | ^[Function]`(event: FocusEvent) => void`                    |
| focus          | triggers when Cascader focuses                                | ^[Function]`(event: FocusEvent) => void`                    |
| clear ^(2.7.7) | triggers when the clear icon is clicked in a clearable Select | ^[Function]`() => void`                                     |
| visible-change | triggers when the dropdown appears/disappears                 | ^[Function]`(value: boolean) => void`                       |
| remove-tag     | triggers when remove tag in multiple selection mode           | ^[Function]`(value: CascaderNode['valueByOption']) => void` |

| Name                     | Description                                                                                    | Type                                                      |
| ------------------------ | ---------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| default                  | the custom content of cascader node, which are current Node object and node data respectively. | ^[object]`{ node: any, data: any }`                       |
| empty                    | content when there is no matched options.                                                      | —                                                         |
| prefix ^(2.9.4)          | content as Input prefix                                                                        | —                                                         |
| suggestion-item ^(2.9.5) | custom content for suggestion item when searching                                              | ^[object]`{ item: CascaderNode }`                         |
| tag ^(2.10.3)            | custom tags style                                                                              | ^[object]`{ data: Tag[], deleteTag: (tag: Tag) => void }` |
| header ^(2.10.5)         | content at the top of the dropdown                                                             | —                                                         |
| footer ^(2.10.5)         | content at the bottom of the dropdown                                                          | —                                                         |

| Name                          | Description                                                                                                       | Type                                                            |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| getCheckedNodes               | get an array of currently selected node,(leafOnly) whether only return the leaf checked nodes, default is `false` | ^[Function]`(leafOnly: boolean) => CascaderNode[] \| undefined` |
| cascaderPanelRef              | cascader panel ref                                                                                                | ^[object]`ComputedRef<any>`                                     |
| togglePopperVisible ^(2.2.31) | toggle the visible type of popper                                                                                 | ^[Function]`(visible?: boolean) => void`                        |
| contentRef                    | cascader content ref                                                                                              | ^[object]`ComputedRef<any>`                                     |
| presentText ^(2.8.4)          | selected content text                                                                                             | ^[object]`ComputedRef<string>`                                  |
| focus ^(2.11.8)               | focus the input element                                                                                           | ^[Function]`() => void`                                         |
| blur ^(2.11.8)                | blur the input element                                                                                            | ^[Function]`() => void`                                         |

### CascaderPanel Attributes

| Name                    | Description                                                                              | Type                                                       | Default |
| ----------------------- | ---------------------------------------------------------------------------------------- | ---------------------------------------------------------- | ------- |
| model-value / v-model   | binding value                                                                            | ^[string]/^[number]/^[object]`string[] \| number[] \| any` | —       |
| options                 | data of the options, the key of `value` and `label` can be customize by `CascaderProps`. | ^[object]`CascaderOption[]`                                | —       |
| [props](#cascaderprops) | configuration options, see the following `CascaderProps` table.                          | ^[object]`CascaderProps`                                   | —       |

### CascaderPanel Events

| Name              | Description                                                             | Type                                                     |
| ----------------- | ----------------------------------------------------------------------- | -------------------------------------------------------- |
| change            | triggers when the binding value changes                                 | ^[Function]`(value: CascaderValue \| undefined) => void` |
| update:modelValue | triggers when the binding value changes                                 | ^[Function]`(value: CascaderValue \| undefined) => void` |
| expand-change     | triggers when expand option changes                                     | ^[Function]`(value: CascaderNodePathValue) => void`      |
| close             | close panel event, provided to Cascader to put away the panel judgment. | ^[Function]`() => void`                                  |

### CascaderPanel Slots

| Name           | Description                                                                                    | Type                                |
| -------------- | ---------------------------------------------------------------------------------------------- | ----------------------------------- |
| default        | the custom content of cascader node, which are current Node object and node data respectively. | ^[object]`{ node: any, data: any }` |
| empty ^(2.8.3) | the content of the panel when there is no data.                                                | —                                   |

### CascaderPanel Exposes

| Name              | Description                                                                                                       | Type                                                            |
| ----------------- | ----------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| getCheckedNodes   | get an array of currently selected node,(leafOnly) whether only return the leaf checked nodes, default is `false` | ^[Function]`(leafOnly: boolean) => CascaderNode[] \| undefined` |
| clearCheckedNodes | clear checked nodes                                                                                               | ^[Function]`() => void`                                         |

| Attribute                  | Description                                                                                                                     | Type                                                                    | Default  |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- | -------- |
| expandTrigger              | trigger mode of expanding options                                                                                               | ^[enum]`'click' \| 'hover'`                                             | click    |
| multiple                   | whether multiple selection is enabled                                                                                           | ^[boolean]                                                              | false    |
| checkStrictly              | whether checked state of a node not affects its parent and child nodes                                                          | ^[boolean]                                                              | false    |
| emitPath                   | when checked nodes change, whether to emit an array of node's path, if false, only emit the value of node.                      | ^[boolean]                                                              | true     |
| lazy                       | whether to dynamic load child nodes, use with `lazyload` attribute                                                              | ^[boolean]                                                              | false    |
| lazyLoad                   | method for loading child nodes data, only works when `lazy` is true. The reject parameter is supported after version ^(2.11.5). | ^[Function]`(node: Node, resolve: Resolve, reject: () => void) => void` | —        |
| value                      | specify which key of node object is used as the node's value                                                                    | ^[string]                                                               | value    |
| label                      | specify which key of node object is used as the node's label                                                                    | ^[string]                                                               | label    |
| children                   | specify which key of node object is used as the node's children                                                                 | ^[string]                                                               | children |
| disabled                   | specify which key of node object is used as the node's disabled                                                                 | ^[string]                                                               | disabled |
| leaf                       | specify which key of node object is used as the node's leaf field                                                               | ^[string]                                                               | leaf     |
| hoverThreshold             | hover threshold of expanding options                                                                                            | ^[number]                                                               | 500      |
| checkOnClickNode ^(2.10.5) | whether to check or uncheck node when clicking on the node                                                                      | ^[boolean]                                                              | false    |
| checkOnClickLeaf ^(2.10.5) | whether to check or uncheck node when clicking on leaf node (last children).                                                    | ^[boolean]                                                              | true     |
| showPrefix ^(2.10.5)       | whether to show the radio or checkbox prefix                                                                                    | ^[boolean]                                                              | true     |

<details>
  <summary>Show declarations</summary>

### check-on-click-node.vue

### custom-content.vue

### custom-header-footer.vue

### custom-suggestion-item.vue

### dynamic-loading.vue

### multiple-selection.vue

### option-disabling.vue

### show-checked-strategy.vue

---
Title: Checkbox
URL: https://element-plus.org/en-US/component/checkbox
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-cascader :props="props" />
</template>

<script lang="ts" setup>
const props = { multiple: true }
</script>
```

Example 2 (vue):
```vue
<template>
  <!--  Object literal binging here is invalid syntax for cascader  -->
  <el-cascader :props="{ multiple: true }" />
</template>
```

Example 3 (ts):
```ts
type CascaderNodeValue = string | number
type CascaderNodePathValue = CascaderNodeValue[]
type CascaderValue =
  | CascaderNodeValue
  | CascaderNodePathValue
  | (CascaderNodeValue | CascaderNodePathValue)[]

type Resolve = (data: any) => void

type ExpandTrigger = 'click' | 'hover'

type LazyLoad = (node: Node, resolve: Resolve, reject: () => void) => void

type isDisabled = (data: CascaderOption, node: Node) => boolean

type isLeaf = (data: CascaderOption, node: Node) => boolean

interface CascaderOption extends Record<string, unknown> {
  label?: string
  value?: CascaderNodeValue
  children?: CascaderOption[]
  disabled?: boolean
  leaf?: boolean
}

interface CascaderProps {
  expandTrigger?: ExpandTrigger
  multiple?: boolean
  checkStrictly?: boolean
  emitPath?: boolean
  lazy?: boolean
  lazyLoad?: LazyLoad
  value?: string
  label?: string
  children?: string
  disabled?: string | isDisabled
  leaf?: string | isLeaf
  hoverThreshold?: number
}

class Node {
  readonly uid: number
  readonly level: number
  readonly value: CascaderNodeValue
  readonly label: string
  readonly pathNodes: Node[]
  readonly pathValues: CascaderNodePathValue
  readonly pathLabels: string[]

  childrenData: ChildrenData
  children: Node[]
  text: string
  loaded: boolean
  /**
   * Is it checked
   *
   * @default false
   */
  checked: boolean
  /**
   * Used to indicate the intermediate state of unchecked and fully checked child nodes
   *
   * @default false
   */
  indeterminate: boolean
  /**
   * Loading Status
   *
   * @default false
   */
  loading: boolean

  // getter
  isDisabled: boolean
  isLeaf: boolean
  valueByOption: CascaderNodeValue | CascaderNodePathValue

  // method
  appendChild(childData: CascaderOption): Node
  calcText(allLevels: boolean, separator: string): string
  broadcast(): void
  emit(): void
  onParentCheck(checked: boolean): void
  onChildCheck(): void
  setCheckState(checked: boolean): void
  doCheck(checked: boolean): void
}

Node as CascaderNode
```

Example 4 (vue):
```vue
<template>
  <div class="m-4">
    <p>Select any level of options (Single selection)</p>
    <el-cascader :options="options" :props="props1" clearable />
  </div>
  <div class="m-4">
    <p>Select any level of options (Multiple selection)</p>
    <el-cascader :options="options" :props="props2" clearable />
  </div>
</template>

<script lang="ts" setup>
const props1 = {
  checkStrictly: true,
}

const props2 = {
  multiple: true,
  checkStrictly: true,
}

const options = [
  {
    value: 'guide',
    label: 'Guide',
    children: [
      {
        value: 'disciplines',
        label: 'Disciplines',
        children: [
          {
            value: 'consistency',
            label: 'Consistency',
          },
          {
            value: 'feedback',
            label: 'Feedback',
          },
          {
            value: 'efficiency',
            label: 'Efficiency',
          },
          {
            value: 'controllability',
            label: 'Controllability',
          },
        ],
      },
      {
        value: 'navigation',
        label: 'Navigation',
        children: [
          {
            value: 'side nav',
            label: 'Side Navigation',
          },
          {
            value: 'top nav',
            label: 'Top Navigation',
          },
        ],
      },
    ],
  },
  {
    value: 'component',
    label: 'Component',
    children: [
      {
        value: 'basic',
        label: 'Basic',
        children: [
          {
            value: 'layout',
            label: 'Layout',
          },
          {
            value: 'color',
            label: 'Color',
          },
          {
            value: 'typography',
            label: 'Typography',
          },
          {
            value: 'icon',
            label: 'Icon',
          },
          {
            value: 'button',
            label: 'Button',
          },
        ],
      },
      {
        value: 'form',
        label: 'Form',
        children: [
          {
            value: 'radio',
            label: 'Radio',
          },
          {
            value: 'checkbox',
            label: 'Checkbox',
          },
          {
            value: 'input',
            label: 'Input',
          },
          {
            value: 'input-number',
            label: 'InputNumber',
          },
          {
            value: 'select',
            label: 'Select',
          },
          {
            value: 'cascader',
            label: 'Cascader',
          },
          {
            value: 'switch',
            label: 'Switch',
          },
          {
            value: 'slider',
            label: 'Slider',
          },
          {
            value: 'time-picker',
            label: 'TimePicker',
          },
          {
            value: 'date-picker',
            label: 'DatePicker',
          },
          {
            value: 'datetime-picker',
            label: 'DateTimePicker',
          },
          {
            value: 'upload',
            label: 'Upload',
          },
          {
            value: 'rate',
            label: 'Rate',
          },
          {
            value: 'form',
            label: 'Form',
          },
        ],
      },
      {
        value: 'data',
        label: 'Data',
        children: [
          {
            value: 'table',
            label: 'Table',
          },
          {
            value: 'tag',
            label: 'Tag',
          },
          {
            value: 'progress',
            label: 'Progress',
          },
          {
            value: 'tree',
            label: 'Tree',
          },
          {
            value: 'pagination',
            label: 'Pagination',
          },
          {
            value: 'badge',
            label: 'Badge',
          },
        ],
      },
      {
        value: 'notice',
        label: 'Notice',
        children: [
          {
            value: 'alert',
            label: 'Alert',
          },
          {
            value: 'loading',
            label: 'Loading',
          },
          {
            value: 'message',
            label: 'Message',
          },
          {
            value: 'message-box',
            label: 'MessageBox',
          },
          {
            value: 'notification',
            label: 'Notification',
          },
        ],
      },
      {
        value: 'navigation',
        label: 'Navigation',
        children: [
          {
            value: 'menu',
            label: 'Menu',
          },
          {
            value: 'tabs',
            label: 'Tabs',
          },
          {
            value: 'breadcrumb',
            label: 'Breadcrumb',
          },
          {
            value: 'dropdown',
            label: 'Dropdown',
          },
          {
            value: 'steps',
            label: 'Steps',
          },
        ],
      },
      {
        value: 'others',
        label: 'Others',
        children: [
          {
            value: 'dialog',
            label: 'Dialog',
          },
          {
            value: 'tooltip',
            label: 'Tooltip',
          },
          {
            value: 'popover',
            label: 'Popover',
          },
          {
            value: 'card',
            label: 'Card',
          },
          {
            value: 'carousel',
            label: 'Carousel',
          },
          {
            value: 'collapse',
            label: 'Collapse',
          },
        ],
      },
    ],
  },
  {
    value: 'resource',
    label: 'Resource',
    children: [
      {
        value: 'axure',
        label: 'Axure Components',
      },
      {
        value: 'sketch',
        label: 'Sketch Templates',
      },
      {
        value: 'docs',
        label: 'Design Documentation',
      },
    ],
  },
]
</script>
```

---

## Select

**URL:** llms-txt#select

**Contents:**
- Basic usage
- Options attribute ^(2.10.5)
- Disabled option
- Disabled select
- Clearable
- Sizes
- Basic multiple select
- Custom template
- Header of the dropdown ^(2.4.3)
- Footer of the dropdown ^(2.4.3)

When there are plenty of options, use a drop-down menu to display and select desired ones.

## Options attribute ^(2.10.5)

Disable the whole component.

You can clear Select using a clear icon.

## Basic multiple select

Multiple select uses tags to display selected options.

You can customize HTML templates for options.

## Header of the dropdown ^(2.4.3)

You can customize the header of the dropdown.

## Footer of the dropdown ^(2.4.3)

You can customize the footer of the dropdown.

Display options in groups.

You can filter options for your desired ones.

Enter keywords and search data from server.

Create and select new items that are not included in select options

## Use value-key attribute

If the binding value of Select is an object, make sure to assign `value-key` as its unique identity key name.

## Custom Tag ^(2.5.0)

You can customize tags.

## Custom Loading ^(2.5.2)

Override loading content.

## Empty Values ^(2.7.0)

If you want to support empty string, please set `empty-values` to `[null, undefined]`.

If you want to change the clear value to `null`, please set `value-on-clear` to `null`.

## Custom Label ^(2.7.4)

You can customize label.

### Select Attributes

| Name                            | Description                                                                                                                              | Type                                                                                                                                                                        | Default                                        |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| model-value / v-model           | binding value                                                                                                                            | ^[string] / ^[number] / ^[boolean] / ^[object] / ^[array]                                                                                                                   | —                                              |
| multiple                        | whether multiple-select is activated                                                                                                     | ^[boolean]                                                                                                                                                                  | false                                          |
| options ^(2.10.5)               | data of the options, the key of `value` and `label` and `disabled` can be customize by `props`                                           | ^[array]`Array<{[key: string]: any}>`                                                                                                                                       | —                                              |
| [props](#props) ^(2.10.5)       | configuration options                                                                                                                    | ^[object]                                                                                                                                                                   | —                                              |
| disabled                        | whether Select is disabled                                                                                                               | ^[boolean]                                                                                                                                                                  | false                                          |
| value-key                       | unique identity key name for value, required when value is an object                                                                     | ^[string]                                                                                                                                                                   | value                                          |
| size                            | size of Input                                                                                                                            | ^[enum]`'' \| 'large' \| 'default' \| 'small'`                                                                                                                              | —                                              |
| clearable                       | whether select can be cleared                                                                                                            | ^[boolean]                                                                                                                                                                  | false                                          |
| collapse-tags                   | whether to collapse tags to a text when multiple selecting                                                                               | ^[boolean]                                                                                                                                                                  | false                                          |
| collapse-tags-tooltip ^(2.3.0)  | whether show all selected tags when mouse hover text of collapse-tags. To use this, `collapse-tags` must be true                         | ^[boolean]                                                                                                                                                                  | false                                          |
| multiple-limit                  | maximum number of options user can select when `multiple` is `true`. No limit when set to 0                                              | ^[number]                                                                                                                                                                   | 0                                              |
| id                              | native input id input                                                                                                                    | ^[string]                                                                                                                                                                   | —                                              |
| name                            | the name attribute of select input                                                                                                       | ^[string]                                                                                                                                                                   | —                                              |
| effect                          | tooltip theme, built-in theme: `dark` / `light`                                                                                          | ^[enum]`'dark' \| 'light'` / ^[string]                                                                                                                                      | light                                          |
| autocomplete                    | the autocomplete attribute of select input                                                                                               | ^[string]                                                                                                                                                                   | off                                            |
| placeholder                     | placeholder, default is 'Select'                                                                                                         | ^[string]                                                                                                                                                                   | —                                              |
| filterable                      | whether Select is filterable                                                                                                             | ^[boolean]                                                                                                                                                                  | false                                          |
| allow-create                    | whether creating new items is allowed. To use this, `filterable` must be true                                                            | ^[boolean]                                                                                                                                                                  | false                                          |
| filter-method                   | custom filter method, the first parameter is the current input value. To use this, `filterable` must be true                             | ^[Function]`(query: string) => void`                                                                                                                                        | —                                              |
| remote                          | whether options are loaded from server                                                                                                   | ^[boolean]                                                                                                                                                                  | false                                          |
| debounce ^(2.11.7)              | debounce delay during remote search, in milliseconds                                                                                     | ^[number]                                                                                                                                                                   | 300                                            |
| remote-method                   | function that gets called when the input value changes. Its parameter is the current input value. To use this, `filterable` must be true | ^[Function]`(query: string) => void`                                                                                                                                        | —                                              |
| remote-show-suffix              | in remote search method show suffix icon                                                                                                 | ^[boolean]                                                                                                                                                                  | false                                          |
| loading                         | whether Select is loading data from server                                                                                               | ^[boolean]                                                                                                                                                                  | false                                          |
| loading-text                    | displayed text while loading data from server, default is 'Loading'                                                                      | ^[string]                                                                                                                                                                   | —                                              |
| no-match-text                   | displayed text when no data matches the filtering query, you can also use slot `empty`, default is 'No matching data'                    | ^[string]                                                                                                                                                                   | —                                              |
| no-data-text                    | displayed text when there is no options, you can also use slot `empty`, default is 'No data'                                             | ^[string]                                                                                                                                                                   | —                                              |
| popper-class                    | custom class name for Select's dropdown and tags' tooltip                                                                                | ^[string]                                                                                                                                                                   | ''                                             |
| popper-style ^(2.11.0)          | custom style for Select's dropdown and tags' tooltip                                                                                     | ^[string] / ^[object]                                                                                                                                                       | —                                              |
| reserve-keyword                 | when `multiple` and `filterable` is true, whether to reserve current keyword after selecting an option                                   | ^[boolean]                                                                                                                                                                  | true                                           |
| default-first-option            | select first matching option on enter key. Use with `filterable` or `remote`                                                             | ^[boolean]                                                                                                                                                                  | false                                          |
| teleported                      | whether select dropdown is teleported, if `true` it will be teleported to where `append-to` sets                                         | ^[boolean]                                                                                                                                                                  | true                                           |
| append-to ^(2.8.4)              | which element the select dropdown appends to                                                                                             | ^[CSSSelector] / ^[HTMLElement]                                                                                                                                             | —                                              |
| persistent                      | when select dropdown is inactive and `persistent` is `false`, select dropdown will be destroyed                                          | ^[boolean]                                                                                                                                                                  | true                                           |
| automatic-dropdown              | for non-filterable Select, this prop decides if the option menu pops up when the input is focused                                        | ^[boolean]                                                                                                                                                                  | false                                          |
| clear-icon                      | custom clear icon component                                                                                                              | ^[string] / ^[object]`Component`                                                                                                                                            | CircleClose                                    |
| fit-input-width                 | whether the width of the dropdown is the same as the input                                                                               | ^[boolean]                                                                                                                                                                  | false                                          |
| suffix-icon                     | custom suffix icon component                                                                                                             | ^[string] / ^[object]`Component`                                                                                                                                            | ArrowDown                                      |
| tag-type                        | tag type                                                                                                                                 | ^[enum]`'' \| 'success' \| 'info' \| 'warning' \| 'danger'`                                                                                                                 | info                                           |
| tag-effect ^(2.7.7)             | tag effect                                                                                                                               | ^[enum]`'' \| 'light' \| 'dark' \| 'plain'`                                                                                                                                 | light                                          |
| validate-event                  | whether to trigger form validation                                                                                                       | ^[boolean]                                                                                                                                                                  | true                                           |
| offset ^(2.8.8)                 | offset of the dropdown                                                                                                                   | ^[number]                                                                                                                                                                   | 12                                             |
| show-arrow ^(2.8.8)             | whether the dropdown has an arrow                                                                                                        | ^[boolean]                                                                                                                                                                  | true                                           |
| placement ^(2.2.17)             | position of dropdown                                                                                                                     | ^[enum]`'top' \| 'top-start' \| 'top-end' \| 'bottom' \| 'bottom-start' \| 'bottom-end' \| 'left' \| 'left-start' \| 'left-end' \| 'right' \| 'right-start' \| 'right-end'` | bottom-start                                   |
| fallback-placements ^(2.5.6)    | list of possible positions for dropdown [popper.js](https://popper.js.org/docs/v2/modifiers/flip/#fallbackplacements)                    | ^[array]`Placement[]`                                                                                                                                                       | ['bottom-start', 'top-start', 'right', 'left'] |
| max-collapse-tags ^(2.3.0)      | the max tags number to be shown. To use this, `collapse-tags` must be true                                                               | ^[number]                                                                                                                                                                   | 1                                              |
| popper-options                  | [popper.js](https://popper.js.org/docs/v2/) parameters                                                                                   | ^[object]refer to [popper.js](https://popper.js.org/docs/v2/) doc                                                                                                           | {}                                             |
| aria-label ^(a11y)              | same as `aria-label` in native input                                                                                                     | ^[string]                                                                                                                                                                   | —                                              |
| empty-values ^(2.7.0)           | empty values of component, [see config-provider](/en-US/component/config-provider#empty-values-configurations)                           | ^[array]                                                                                                                                                                    | —                                              |
| value-on-clear ^(2.7.0)         | clear return value, [see config-provider](/en-US/component/config-provider#empty-values-configurations)                                  | ^[string] / ^[number] / ^[boolean] / ^[Function]                                                                                                                            | —                                              |
| suffix-transition ^(deprecated) | animation when dropdown appears/disappears icon                                                                                          | ^[boolean]                                                                                                                                                                  | true                                           |
| tabindex ^(2.9.0)               | tabindex for input                                                                                                                       | ^[string] / ^[number]                                                                                                                                                       | —                                              |

| Attribute         | Description                                                     | Type      | Default  |
| ----------------- | --------------------------------------------------------------- | --------- | -------- |
| value             | specify which key of node object is used as the node's value    | ^[string] | value    |
| label             | specify which key of node object is used as the node's label    | ^[string] | label    |
| options ^(2.11.0) | specify which key of node object is used as the node's children | ^[string] | options  |
| disabled          | specify which key of node object is used as the node's disabled | ^[string] | disabled |

| Name                  | Description                                                   | Type                                                                |
| --------------------- | ------------------------------------------------------------- | ------------------------------------------------------------------- |
| change                | triggers when the selected value changes                      | ^[Function]`(value: any) => void`                                   |
| visible-change        | triggers when the dropdown appears/disappears                 | ^[Function]`(visible: boolean) => void`                             |
| remove-tag            | triggers when a tag is removed in multiple mode               | ^[Function]`(tagValue: any) => void`                                |
| clear                 | triggers when the clear icon is clicked in a clearable Select | ^[Function]`() => void`                                             |
| blur                  | triggers when Input blurs                                     | ^[Function]`(event: FocusEvent) => void`                            |
| focus                 | triggers when Input focuses                                   | ^[Function]`(event: FocusEvent) => void`                            |
| popup-scroll ^(2.9.4) | triggers when dropdown scrolls                                | ^[Function]`(data:{scrollTop: number, scrollLeft: number}) => void` |

| Name             | Description                                                                                     | Subtags                                                                                                               |
| ---------------- | ----------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| default          | option component list                                                                           | Option Group / Option                                                                                                 |
| header ^(2.4.3)  | content at the top of the dropdown                                                              | —                                                                                                                     |
| footer ^(2.4.3)  | content at the bottom of the dropdown                                                           | —                                                                                                                     |
| prefix           | content as Select prefix                                                                        | —                                                                                                                     |
| empty            | content when there is no options                                                                | —                                                                                                                     |
| tag ^(2.5.0)     | content as Select tag, subTags `data`, `selectDisabled` and `deleteTag` introduced in ^(2.10.3) | ^[object]`{ data: OptionBasic[], selectDisabled: boolean, deleteTag: (event: MouseEvent, tag: OptionBasic) => void }` |
| loading ^(2.5.2) | content as Select loading                                                                       | —                                                                                                                     |
| label ^(2.7.4)   | content as Select label. `index` introduced in ^(2.11.2)                                        | ^[object]`{ index: number, label: string \| any, value: string \| any }`                                              |

| Name                   | Description                                     | Type                                       |
| ---------------------- | ----------------------------------------------- | ------------------------------------------ |
| focus                  | focus the Input component                       | ^[Function]`() => void`                    |
| blur                   | blur the Input component, and hide the dropdown | ^[Function]`() => void`                    |
| selectedLabel ^(2.8.5) | get the currently selected label                | ^[object]`ComputedRef<string \| string[]>` |

### Option Group Attributes

| Name     | Description                                  | Type       | Default |
| -------- | -------------------------------------------- | ---------- | ------- |
| label    | name of the group                            | ^[string]  | —       |
| disabled | whether to disable all options in this group | ^[boolean] | false   |

### Option Group Slots

| Name    | Description               | Subtags |
| ------- | ------------------------- | ------- |
| default | customize default content | Option  |

### Option Attributes

| Name     | Description                                 | Type                                           | Default |
| -------- | ------------------------------------------- | ---------------------------------------------- | ------- |
| value    | value of option                             | ^[string] / ^[number] / ^[boolean] / ^[object] | —       |
| label    | label of option, same as `value` if omitted | ^[string] / ^[number]                          | —       |
| disabled | whether option is disabled                  | ^[boolean]                                     | false   |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

### custom-footer.vue

### custom-header.vue

### custom-loading.vue

### custom-template.vue

### disabled-option.vue

### remote-search.vue

---
Title: Skeleton
URL: https://element-plus.org/en-US/component/skeleton
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-select
    v-model="value"
    multiple
    filterable
    allow-create
    default-first-option
    :reserve-keyword="false"
    placeholder="Choose tags for your article"
    style="width: 240px"
  >
    <el-option
      v-for="item in options"
      :key="item.value"
      :label="item.label"
      :value="item.value"
    />
  </el-select>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref<string[]>([])
const options = [
  {
    value: 'HTML',
    label: 'HTML',
  },
  {
    value: 'CSS',
    label: 'CSS',
  },
  {
    value: 'JavaScript',
    label: 'JavaScript',
  },
]
</script>
```

Example 2 (vue):
```vue
<template>
  <el-select v-model="value" placeholder="Select" style="width: 240px">
    <el-option
      v-for="item in options"
      :key="item.value"
      :label="item.label"
      :value="item.value"
    />
  </el-select>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref('')

const options = [
  {
    value: 'Option1',
    label: 'Option1',
  },
  {
    value: 'Option2',
    label: 'Option2',
  },
  {
    value: 'Option3',
    label: 'Option3',
  },
  {
    value: 'Option4',
    label: 'Option4',
  },
  {
    value: 'Option5',
    label: 'Option5',
  },
]
</script>
```

Example 3 (vue):
```vue
<template>
  <el-select
    v-model="value"
    clearable
    placeholder="Select"
    style="width: 240px"
  >
    <el-option
      v-for="item in options"
      :key="item.value"
      :label="item.label"
      :value="item.value"
    />
  </el-select>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref('')
const options = [
  {
    value: 'Option1',
    label: 'Option1',
  },
  {
    value: 'Option2',
    label: 'Option2',
  },
  {
    value: 'Option3',
    label: 'Option3',
  },
  {
    value: 'Option4',
    label: 'Option4',
  },
  {
    value: 'Option5',
    label: 'Option5',
  },
]
</script>
```

Example 4 (vue):
```vue
<template>
  <el-select v-model="value" placeholder="Select" style="width: 240px">
    <el-option
      v-for="item in cities"
      :key="item.value"
      :label="item.label"
      :value="item.value"
    />
    <template #footer>
      <el-button v-if="!isAdding" text bg size="small" @click="onAddOption">
        Add an option
      </el-button>
      <template v-else>
        <el-input
          v-model="optionName"
          class="option-input"
          placeholder="input option name"
          size="small"
        />
        <el-button type="primary" size="small" @click="onConfirm">
          confirm
        </el-button>
        <el-button size="small" @click="clear">cancel</el-button>
      </template>
    </template>
  </el-select>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

import type { CheckboxValueType } from 'element-plus'

const isAdding = ref(false)
const value = ref<CheckboxValueType[]>([])
const optionName = ref('')
const cities = ref([
  {
    value: 'Beijing',
    label: 'Beijing',
  },
  {
    value: 'Shanghai',
    label: 'Shanghai',
  },
  {
    value: 'Nanjing',
    label: 'Nanjing',
  },
  {
    value: 'Chengdu',
    label: 'Chengdu',
  },
  {
    value: 'Shenzhen',
    label: 'Shenzhen',
  },
  {
    value: 'Guangzhou',
    label: 'Guangzhou',
  },
])

const onAddOption = () => {
  isAdding.value = true
}

const onConfirm = () => {
  if (optionName.value) {
    cities.value.push({
      label: optionName.value,
      value: optionName.value,
    })
    clear()
  }
}

const clear = () => {
  optionName.value = ''
  isAdding.value = false
}
</script>

<style>
.option-input {
  width: 100%;
  margin-bottom: 8px;
}
</style>
```

---

## Separate subject from body with a blank line

**URL:** llms-txt#separate-subject-from-body-with-a-blank-line

---

## Virtualized Select

**URL:** llms-txt#virtualized-select

**Contents:**
- Background
- Basic usage
- Multi select
- Sizes
- Hide extra tags when the selected items are too many
- Filterable multi-select
- Disabled selector and select options
- Option Grouping
- Clearable selector
- Customized option renderer

In some use-cases, a single selector may end up loading tens of thousands of rows of data.
Rendering that much data into the DOM could be a burden to the browser, which can result in performance issues.
For a better user and developer experience, we decided to add this component.

The simplest selector

The basic multi-select selector with tags

## Hide extra tags when the selected items are too many

You can collapse tags to a text by using `collapse-tags` attribute. You can check them when mouse hover collapse text by using `collapse-tags-tooltip` attribute.

## Filterable multi-select

When the options are overwhelmingly too many, you can use `filterable` option to enable filter feature for finding out the desired option

## Disabled selector and select options

You can choose to disable selector itself or the option.

We can group option as we wanted, as long as the data satisfies the pattern.

## Clearable selector

We can clear all the selected options at once, also applicable for single select.

## Customized option renderer

We can define our own template for rendering the option in the popup.

## Header of the dropdown ^(2.5.2)

You can customize the header of the dropdown.

## Footer of the dropdown ^(2.5.2)

You can customize the footer of the dropdown.

Create and select new items that are not included in select options

By using the `allow-create` attribute, users can create new items by typing in the input box. Note that for `allow-create` to work, `filterable` must be `true`. This example also demonstrates `default-first-option`. When this attribute is set to `true`, you can select the first option in the current option list by hitting enter without having to navigate with mouse or arrow keys.

Enter keywords and search data from server.

## Use value-key attribute

when `options.value` is an object, you should set a unique identity key name for value

## Aliases for custom options ^(2.4.2)

When your `options` format is different from the default format, you can customize the alias of the `options` through the `props` attribute

## Custom Tag ^(2.5.0)

You can customize tags.

## Custom Loading ^(2.5.2)

Override loading content.

## Empty Values ^(2.7.0)

If you want to support empty string, please set `empty-values` to `[null, undefined]`.

If you want to change the clear value to `null`, please set `value-on-clear` to `null`.

## Custom Label ^(2.7.4)

You can customize label.

## Custom Width ^(2.9.2)

The width of dropdown box is calculated by default based on the value of `label`. If you customize the dropdown box options through the `default slot`, it is likely that the text displayed in the options is not equal to the value of `label`, resulting in calculation errors. In this case, you can set the `fit-input-width` attribute to a number to fix its width.

| Name                                | Description                                                                                                                              | Type                                                                                                                                                                        | Default                                        |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| model-value / v-model               | binding value                                                                                                                            | ^[string] / ^[number] / ^[boolean] / ^[object] / ^[array]                                                                                                                   | —                                              |
| options                             | data of the options, the key of `value` and `label` can be customize by `props`                                                          | ^[array]                                                                                                                                                                    | —                                              |
| [props](#props) ^(2.4.2)            | configuration options, see the following table                                                                                           | ^[object]                                                                                                                                                                   | —                                              |
| multiple                            | is multiple                                                                                                                              | ^[boolean]                                                                                                                                                                  | false                                          |
| disabled                            | is disabled                                                                                                                              | ^[boolean]                                                                                                                                                                  | false                                          |
| value-key                           | unique identity key name for value, required when value is an object                                                                     | ^[string]                                                                                                                                                                   | value                                          |
| size                                | size of component                                                                                                                        | ^[enum]`'' \| 'large' \| 'default' \| 'small'`                                                                                                                              | ''                                             |
| clearable                           | whether select can be cleared                                                                                                            | ^[boolean]                                                                                                                                                                  | false                                          |
| clear-icon                          | custom clear icon                                                                                                                        | ^[string] / ^[object]`Component`                                                                                                                                            | CircleClose                                    |
| collapse-tags                       | whether to collapse tags to a text when multiple selecting                                                                               | ^[boolean]                                                                                                                                                                  | false                                          |
| multiple-limit                      | maximum number of options user can select when multiple is true. No limit when set to 0                                                  | ^[number]                                                                                                                                                                   | 0                                              |
| id                                  | native input id input                                                                                                                    | ^[string]                                                                                                                                                                   | —                                              |
| name                                | the name attribute of select input                                                                                                       | ^[string]                                                                                                                                                                   | —                                              |
| effect                              | tooltip theme, built-in theme: `dark` / `light`                                                                                          | ^[enum]`'dark' \| 'light'` / ^[string]                                                                                                                                      | light                                          |
| autocomplete                        | autocomplete of select input                                                                                                             | ^[string]                                                                                                                                                                   | off                                            |
| placeholder                         | placeholder                                                                                                                              | ^[string]                                                                                                                                                                   | Please select                                  |
| filterable                          | whether Select is filterable                                                                                                             | ^[boolean]                                                                                                                                                                  | false                                          |
| allow-create                        | whether creating new items is allowed. To use this, `filterable` must be true                                                            | ^[boolean]                                                                                                                                                                  | false                                          |
| filter-method                       | custom filter method, the first parameter is the current input value. To use this, `filterable` must be true method                      | ^[Function]`(query: string) => void`                                                                                                                                        | —                                              |
| loading                             | whether Select is loading data from server                                                                                               | ^[boolean]                                                                                                                                                                  | false                                          |
| loading-text                        | displayed text while loading data from server, default is 'Loading'                                                                      | ^[string]                                                                                                                                                                   | —                                              |
| reserve-keyword                     | whether reserve the keyword after select filtered option.                                                                                | ^[boolean]                                                                                                                                                                  | true                                           |
| default-first-option                | select first matching option on enter key. Use with `filterable` or `remote`                                                             | ^[boolean]                                                                                                                                                                  | false                                          |
| no-match-text                       | displayed text when no data matches the filtering query, you can also use slot `empty`, default is 'No matching data'                    | ^[string]                                                                                                                                                                   | —                                              |
| no-data-text                        | displayed text when there is no options, you can also use slot empty                                                                     | ^[string]                                                                                                                                                                   | No Data                                        |
| popper-class                        | custom class name for Select's dropdown and tags' tooltip                                                                                | ^[string] / ^[object]                                                                                                                                                       | ''                                             |
| popper-style ^(2.11.0)              | custom style for Select's dropdown and tags' tooltip                                                                                     | ^[string] / ^[object]                                                                                                                                                       | —                                              |
| teleported                          | whether select dropdown is teleported, if `true` it will be teleported to where `append-to` sets                                         | ^[boolean]                                                                                                                                                                  | true                                           |
| append-to ^(2.8.8)                  | which element the select dropdown appends to                                                                                             | ^[CSSSelector] / ^[HTMLElement]                                                                                                                                             | —                                              |
| persistent                          | when select dropdown is inactive and `persistent` is `false`, select dropdown will be destroyed                                          | ^[boolean]                                                                                                                                                                  | true                                           |
| popper-options                      | [popper.js](https://popper.js.org/docs/v2/) parameters                                                                                   | ^[object]refer to [popper.js](https://popper.js.org/docs/v2/) doc                                                                                                           | {}                                             |
| automatic-dropdown                  | for non-filterable Select, this prop decides if the option menu pops up when the input is focused                                        | ^[boolean]                                                                                                                                                                  | false                                          |
| fit-input-width ^(2.9.2)            | whether the width of the dropdown is the same as the input, if the value is `number`, then the width is fixed                            | ^[boolean] / ^[number]                                                                                                                                                      | true                                           |
| suffix-icon ^(2.9.8)                | custom suffix icon component                                                                                                             | ^[string] / ^[object]`Component`                                                                                                                                            | ArrowDown                                      |
| height                              | The height of the dropdown panel, 34px for each item                                                                                     | ^[number]                                                                                                                                                                   | 274                                            |
| item-height                         | The height of the dropdown item                                                                                                          | ^[number]                                                                                                                                                                   | 34                                             |
| scrollbar-always-on                 | Controls whether the scrollbar is always displayed                                                                                       | ^[boolean]                                                                                                                                                                  | false                                          |
| remote                              | whether search data from server                                                                                                          | ^[boolean]                                                                                                                                                                  | false                                          |
| debounce ^(2.11.7)                  | debounce delay during remote search, in milliseconds                                                                                     | ^[number]                                                                                                                                                                   | 300                                            |
| remote-method                       | function that gets called when the input value changes. Its parameter is the current input value. To use this, `filterable` must be true | ^[Function]`(query: string) => void`                                                                                                                                        | —                                              |
| remote-show-suffix ^(2.11.9)        | in remote search method show suffix icon                                                                                                 | ^[boolean]                                                                                                                                                                  | false                                          |
| validate-event                      | whether to trigger form validation                                                                                                       | ^[boolean]                                                                                                                                                                  | true                                           |
| offset ^(2.8.8)                     | offset of the dropdown                                                                                                                   | ^[number]                                                                                                                                                                   | 12                                             |
| show-arrow ^(2.8.8)                 | whether the dropdown has an arrow                                                                                                        | ^[boolean]                                                                                                                                                                  | true                                           |
| placement                           | position of dropdown                                                                                                                     | ^[enum]`'top' \| 'top-start' \| 'top-end' \| 'bottom' \| 'bottom-start' \| 'bottom-end' \| 'left' \| 'left-start' \| 'left-end' \| 'right' \| 'right-start' \| 'right-end'` | bottom-start                                   |
| fallback-placements ^(2.5.6)        | list of possible positions for dropdown [popper.js](https://popper.js.org/docs/v2/modifiers/flip/#fallbackplacements)                    | ^[array]`Placement[]`                                                                                                                                                       | ['bottom-start', 'top-start', 'right', 'left'] |
| collapse-tags-tooltip ^(2.3.0)      | whether show all selected tags when mouse hover text of collapse-tags. To use this, `collapse-tags` must be true                         | ^[boolean]                                                                                                                                                                  | false                                          |
| max-collapse-tags ^(2.3.0)          | The max tags number to be shown. To use this, `collapse-tags` must be true                                                               | ^[number]                                                                                                                                                                   | 1                                              |
| tag-type ^(2.5.0)                   | tag type                                                                                                                                 | ^[enum]`'' \| 'success' \| 'info' \| 'warning' \| 'danger'`                                                                                                                 | info                                           |
| tag-effect ^(2.7.7)                 | tag effect                                                                                                                               | ^[enum]`'' \| 'light' \| 'dark' \| 'plain'`                                                                                                                                 | light                                          |
| aria-label ^(a11y) ^(2.5.0)         | same as `aria-label` in native input                                                                                                     | ^[string]                                                                                                                                                                   | —                                              |
| empty-values ^(2.7.0)               | empty values of component, [see config-provider](/en-US/component/config-provider#empty-values-configurations)                           | ^[array]                                                                                                                                                                    | —                                              |
| value-on-clear ^(2.7.0)             | clear return value, [see config-provider](/en-US/component/config-provider#empty-values-configurations)                                  | ^[string] / ^[number] / ^[boolean] / ^[Function]                                                                                                                            | —                                              |
| popper-append-to-body ^(deprecated) | whether to append the popper menu to body. If the positioning of the popper is wrong, you can try to set this prop to false              | ^[boolean]                                                                                                                                                                  | false                                          |
| tabindex ^(2.9.0)                   | tabindex for input                                                                                                                       | ^[string] / ^[number]                                                                                                                                                       | —                                              |

| Attribute | Description                                                     | Type      | Default  |
| --------- | --------------------------------------------------------------- | --------- | -------- |
| value     | specify which key of node object is used as the node's value    | ^[string] | value    |
| label     | specify which key of node object is used as the node's label    | ^[string] | label    |
| options   | specify which key of node object is used as the node's children | ^[string] | options  |
| disabled  | specify which key of node object is used as the node's disabled | ^[string] | disabled |

| Name           | Description                                                                                                | Type                                     |
| -------------- | ---------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| change         | triggers when the selected value changes, the param is current selected value                              | ^[Function]`(val: any) => void`          |
| visible-change | triggers when the dropdown appears/disappears, the param will be true when it appears, and false otherwise | ^[Function]`(visible: boolean) => void`  |
| remove-tag     | triggers when a tag is removed in multiple mode, the param is removed tag value                            | ^[Function]`(tagValue: any) => void`     |
| clear          | triggers when the clear icon is clicked in a clearable Select                                              | ^[Function]`() => void`                  |
| blur           | triggers when Input blurs                                                                                  | ^[Function]`(event: FocusEvent) => void` |
| focus          | triggers when Input focuses                                                                                | ^[Function]`(event: FocusEvent) => void` |

| Name             | Description                                                                                     | Type                                                                                                           |
| ---------------- | ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| default          | Option renderer                                                                                 | —                                                                                                              |
| header ^(2.5.2)  | content at the top of the dropdown                                                              | —                                                                                                              |
| footer ^(2.5.2)  | content at the bottom of the dropdown                                                           | —                                                                                                              |
| empty            | content when options is empty                                                                   | —                                                                                                              |
| prefix           | prefix content of input                                                                         | —                                                                                                              |
| tag ^(2.5.0)     | content as Select tag, subTags `data`, `selectDisabled` and `deleteTag` introduced in ^(2.10.3) | ^[object]`{ data: Option[], selectDisabled: boolean, deleteTag: (event: MouseEvent, option: Option) => void }` |
| loading ^(2.5.2) | content as Select loading                                                                       | —                                                                                                              |
| label ^(2.7.4)   | content as Select label. `index` introduced in ^(2.11.2)                                        | ^[object]`{ index: number, label: string \| any, value: string \| any }`                                       |

| Name                   | Description                                     | Type                                       |
| ---------------------- | ----------------------------------------------- | ------------------------------------------ |
| focus                  | focus the Input component                       | ^[Function]`() => void`                    |
| blur                   | blur the Input component, and hide the dropdown | ^[Function]`() => void`                    |
| selectedLabel ^(2.8.5) | get the currently selected label                | ^[object]`ComputedRef<string \| string[]>` |

### custom-footer.vue

### custom-header.vue

### custom-loading.vue

### customized-option.vue

### hide-extra-tags.vue

### remote-search.vue

---
Title: Select
URL: https://element-plus.org/en-US/component/select
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <div style="flex: auto">
    <div>
      <el-select-v2
        v-model="value1"
        :options="options"
        placeholder="Please select"
        style="width: 240px; margin-right: 16px; vertical-align: middle"
        allow-create
        default-first-option
        filterable
        multiple
        clearable
      />
      <el-select-v2
        v-model="value2"
        :options="options"
        placeholder="Please select"
        style="width: 240px; vertical-align: middle"
        allow-create
        default-first-option
        filterable
        clearable
      />
    </div>
    <div>
      <p style="margin-top: 20px; margin-bottom: 8px">
        set reserve-keyword false
      </p>
      <el-select-v2
        v-model="value3"
        :options="options"
        placeholder="Please select"
        style="width: 240px; margin-right: 16px; vertical-align: middle"
        allow-create
        default-first-option
        filterable
        multiple
        clearable
        :reserve-keyword="false"
      />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const initials = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']

const value1 = ref([])
const value2 = ref()
const value3 = ref([])
const options = Array.from({ length: 1000 }).map((_, idx) => ({
  value: `Option ${idx + 1}`,
  label: `${initials[idx % 10]}${idx}`,
}))
</script>
```

Example 2 (vue):
```vue
<template>
  <el-select-v2
    v-model="value"
    :options="options"
    placeholder="Please select"
    style="width: 240px"
  />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const initials = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']

const value = ref()
const options = Array.from({ length: 1000 }).map((_, idx) => ({
  value: `Option ${idx + 1}`,
  label: `${initials[idx % 10]}${idx}`,
}))
</script>
```

Example 3 (vue):
```vue
<template>
  <el-select-v2
    v-model="value1"
    :options="options"
    placeholder="Please select"
    style="width: 240px; margin-right: 16px; vertical-align: middle"
    multiple
    clearable
  />
  <el-select-v2
    v-model="value2"
    :options="options"
    placeholder="Please select"
    style="width: 240px; vertical-align: middle"
    clearable
  />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const initials = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']

const value1 = ref([])
const value2 = ref()
const options = Array.from({ length: 1000 }).map((_, idx) => ({
  value: `Option ${idx + 1}`,
  label: `${initials[idx % 10]}${idx}`,
}))
</script>
```

Example 4 (vue):
```vue
<template>
  <el-select-v2
    ref="select"
    v-model="value"
    :options="options"
    placeholder="Select"
    style="width: 240px"
  >
    <template #footer>
      <el-button v-if="!isAdding" text bg size="small" @click="onAddOption">
        Add an option
      </el-button>
      <div v-else class="select-footer">
        <el-input
          v-model="optionName"
          class="option-input"
          placeholder="input option name"
          size="small"
        />
        <div>
          <el-button type="primary" size="small" @click="onConfirm">
            confirm
          </el-button>
          <el-button size="small" @click="clear">cancel</el-button>
        </div>
      </div>
    </template>
  </el-select-v2>
</template>

<script lang="ts" setup>
import { nextTick, ref } from 'vue'

import type { CheckboxValueType, SelectV2Instance } from 'element-plus'

const initials = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
const select = ref<SelectV2Instance>()
const isAdding = ref(false)
const value = ref<CheckboxValueType[]>([])
const optionName = ref('')
const options = ref(
  Array.from({ length: 1000 }).map((_, idx) => ({
    value: `Option ${idx + 1}`,
    label: `${initials[idx % 10]}${idx}`,
  }))
)

const onAddOption = () => {
  isAdding.value = true
}

const onConfirm = () => {
  if (optionName.value) {
    options.value.push({
      label: optionName.value,
      value: optionName.value,
    })
    clear()
    nextTick(() => {
      select.value?.scrollTo(options.value.length - 1)
    })
  }
}

const clear = () => {
  optionName.value = ''
  isAdding.value = false
}
</script>

<style>
.select-footer {
  display: flex;
  flex-direction: column;

  .option-input {
    width: 100%;
    margin-bottom: 8px;
  }
}
</style>
```

---

## Checkbox

**URL:** llms-txt#checkbox

**Contents:**
- Basic usage
- Disabled State
- Checkbox group
- Options attribute ^(2.11.2)
- Indeterminate
- Minimum / Maximum items checked
- Button style
- With borders
- Checkbox API
  - Checkbox Attributes

A group of options for multiple choices.

Checkbox can be used alone to switch between two states.

Disabled state for checkbox.

It is used for multiple checkboxes which are bound in one group, and indicates whether one option is selected by checking if it is checked.

## Options attribute ^(2.11.2)

The `indeterminate` property can help you to achieve a 'check all' effect.

## Minimum / Maximum items checked

The `min` and `max` properties can help you to limit the number of checked items.

Checkbox with button styles.

### Checkbox Attributes

| Name                           | Description                                                                                                                                                    | Type                                           | Default |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | ------- |
| model-value / v-model          | binding value                                                                                                                                                  | ^[string] / ^[number] / ^[boolean]             | —       |
| value ^(2.6.0)                 | value of the Checkbox when used inside a `checkbox-group`                                                                                                      | ^[string] / ^[number] / ^[boolean] / ^[object] | —       |
| label                          | label of the Checkbox when used inside a `checkbox-group`. If there's no value, `label` will act as `value`                                                    | ^[string] / ^[number] / ^[boolean] / ^[object] | —       |
| true-value ^(2.6.0)            | value of the Checkbox if it's checked                                                                                                                          | ^[string] / ^[number]                          | —       |
| false-value ^(2.6.0)           | value of the Checkbox if it's not checked                                                                                                                      | ^[string] / ^[number]                          | —       |
| disabled                       | whether the Checkbox is disabled                                                                                                                               | ^[boolean]                                     | false   |
| border                         | whether to add a border around Checkbox                                                                                                                        | ^[boolean]                                     | false   |
| size                           | size of the Checkbox                                                                                                                                           | ^[enum]`'large' \| 'default' \| 'small'`       | —       |
| name                           | native 'name' attribute                                                                                                                                        | ^[string]                                      | —       |
| checked                        | if the Checkbox is checked                                                                                                                                     | ^[boolean]                                     | false   |
| indeterminate                  | Set indeterminate state, only responsible for style control                                                                                                    | ^[boolean]                                     | false   |
| validate-event                 | whether to trigger form validation                                                                                                                             | ^[boolean]                                     | true    |
| tabindex                       | input tabindex                                                                                                                                                 | ^[string] / ^[number]                          | —       |
| id                             | input id                                                                                                                                                       | ^[string]                                      | —       |
| aria-controls ^(a11y) ^(2.7.2) | same as [aria-controls](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-controls), takes effect when `indeterminate` is `true` | ^[string]                                      | —       |
| aria-label ^(a11y)             | native `aria-label` attribute                                                                                                                                  | ^[string]                                      | —       |
| true-label ^(deprecated)       | value of the Checkbox if it's checked                                                                                                                          | ^[string] / ^[number]                          | —       |
| false-label ^(deprecated)      | value of the Checkbox if it's not checked                                                                                                                      | ^[string] / ^[number]                          | —       |
| controls ^(a11y) ^(deprecated) | same as [aria-controls](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-controls), takes effect when `indeterminate` is `true` | ^[string]                                      | —       |

| Name   | Description                             | Type                                                      |
| ------ | --------------------------------------- | --------------------------------------------------------- |
| change | triggers when the binding value changes | ^[Function]`(value: string \| number \| boolean) => void` |

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

### CheckboxGroup Attributes

| Name                        | Description                                                                                    | Type                                                             | Default                                                  |
| --------------------------- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | -------------------------------------------------------- |
| model-value / v-model       | binding value                                                                                  | ^[object]`string[] \| number[]`                                  | []                                                       |
| size                        | size of checkbox                                                                               | ^[enum]`'large' \| 'default' \| 'small'`                         | —                                                        |
| disabled                    | whether the nesting checkboxes are disabled                                                    | ^[boolean]                                                       | false                                                    |
| min                         | minimum number of checkbox checked                                                             | ^[number]                                                        | —                                                        |
| max                         | maximum number of checkbox checked                                                             | ^[number]                                                        | —                                                        |
| aria-label ^(a11y) ^(2.7.2) | native `aria-label` attribute                                                                  | ^[string]                                                        | —                                                        |
| text-color                  | font color when button is active                                                               | ^[string]                                                        | #ffffff                                                  |
| fill                        | border and background color when button is active                                              | ^[string]                                                        | #409eff                                                  |
| tag                         | element tag of the checkbox group                                                              | ^[string]                                                        | div                                                      |
| validate-event              | whether to trigger form validation                                                             | ^[boolean]                                                       | true                                                     |
| label ^(a11y) ^(deprecated) | native `aria-label` attribute                                                                  | ^[string]                                                        | —                                                        |
| options ^(2.11.2)           | data of the options, the key of `value` and `label` and `disabled` can be customize by `props` | ^[array]`Array<{[key: string]: any}>`                            | —                                                        |
| props ^(2.11.2)             | configuration options                                                                          | ^[object]`{ value?: string, label?: string, disabled?: boolean}` | `{value: 'value', label: 'label', disabled: 'disabled'}` |
| type ^(2.11.5)              | component type to render options (e.g. `'button'`)                                             | ^[enum]`'checkbox' \| 'button'`                                  | 'checkbox'                                               |

### CheckboxGroup Events

| Name   | Description                             | Type                                               |
| ------ | --------------------------------------- | -------------------------------------------------- |
| change | triggers when the binding value changes | ^[Function]`(value: string[] \| number[]) => void` |

### CheckboxGroup Slots

| Name    | Description               | Subtags                    |
| ------- | ------------------------- | -------------------------- |
| default | customize default content | Checkbox / Checkbox-button |

## CheckboxButton API

### CheckboxButton Attributes

| Name                      | Description                                                                                                 | Type                                           | Default |
| ------------------------- | ----------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | ------- |
| value ^(2.6.0)            | value of the checkbox when used inside a `checkbox-group`                                                   | ^[string] / ^[number] / ^[boolean] / ^[object] | —       |
| label                     | label of the checkbox when used inside a `checkbox-group`. If there's no value, `label` will act as `value` | ^[string] / ^[number] / ^[boolean] / ^[object] | —       |
| true-value ^(2.6.0)       | value of the checkbox if it's checked                                                                       | ^[string] / ^[number]                          | —       |
| false-value ^(2.6.0)      | value of the checkbox if it's not checked                                                                   | ^[string] / ^[number]                          | —       |
| disabled                  | whether the checkbox is disabled                                                                            | ^[boolean]                                     | false   |
| name                      | native 'name' attribute                                                                                     | ^[string]                                      | —       |
| checked                   | if the checkbox is checked                                                                                  | ^[boolean]                                     | false   |
| true-label ^(deprecated)  | value of the checkbox if it's checked                                                                       | ^[string] / ^[number]                          | —       |
| false-label ^(deprecated) | value of the checkbox if it's not checked                                                                   | ^[string] / ^[number]                          | —       |

### CheckboxButton Slots

| Name    | Description               |
| ------- | ------------------------- |
| default | customize default content |

---
Title: Collapse
URL: https://element-plus.org/en-US/component/collapse
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-checkbox-group v-model="checkList">
    <!-- works when >=2.6.0, recommended ✔️ value not work when <2.6.0 ❌ -->
    <el-checkbox label="Option 1" value="Value 1" />
    <!-- works when <2.6.0, deprecated act as value when >=3.0.0 -->
    <el-checkbox label="Option 2 & Value 2" />
  </el-checkbox-group>
</template>
```

Example 2 (vue):
```vue
<template>
  <div>
    <el-checkbox v-model="checked1" label="Option 1" size="large" />
    <el-checkbox v-model="checked2" label="Option 2" size="large" />
  </div>
  <div class="my-2">
    <el-checkbox v-model="checked3" label="Option 1" />
    <el-checkbox v-model="checked4" label="Option 2" />
  </div>
  <div class="mt-2">
    <el-checkbox v-model="checked5" label="Option 1" size="small" />
    <el-checkbox v-model="checked6" label="Option 2" size="small" />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const checked1 = ref(true)
const checked2 = ref(false)
const checked3 = ref(false)
const checked4 = ref(false)
const checked5 = ref(false)
const checked6 = ref(false)
</script>
```

Example 3 (vue):
```vue
<template>
  <div>
    <el-checkbox-group v-model="checkboxGroup1" size="large">
      <el-checkbox-button v-for="city in cities" :key="city" :value="city">
        {{ city }}
      </el-checkbox-button>
    </el-checkbox-group>
  </div>
  <div class="demo-button-style">
    <el-checkbox-group v-model="checkboxGroup2">
      <el-checkbox-button v-for="city in cities" :key="city" :value="city">
        {{ city }}
      </el-checkbox-button>
    </el-checkbox-group>
  </div>
  <div class="demo-button-style">
    <el-checkbox-group v-model="checkboxGroup3" size="small">
      <el-checkbox-button
        v-for="city in cities"
        :key="city"
        :value="city"
        :disabled="city === 'Beijing'"
      >
        {{ city }}
      </el-checkbox-button>
    </el-checkbox-group>
  </div>
  <div class="demo-button-style">
    <el-checkbox-group v-model="checkboxGroup4" size="small" disabled>
      <el-checkbox-button v-for="city in cities" :key="city" :value="city">
        {{ city }}
      </el-checkbox-button>
    </el-checkbox-group>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const checkboxGroup1 = ref(['Shanghai'])
const checkboxGroup2 = ref(['Shanghai'])
const checkboxGroup3 = ref(['Shanghai'])
const checkboxGroup4 = ref(['Shanghai'])
const cities = ['Shanghai', 'Beijing', 'Guangzhou', 'Shenzhen']
</script>

<style scoped>
.demo-button-style {
  margin-top: 24px;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <el-checkbox v-model="checked1" disabled>Disabled</el-checkbox>
  <el-checkbox v-model="checked2">Not disabled</el-checkbox>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const checked1 = ref(false)
const checked2 = ref(true)
</script>
```

---

## TreeSelect

**URL:** llms-txt#treeselect

**Contents:**
- Basic usage
- Select any level
- Multiple Selection
- Disabled Selection
- Filterable
- Custom content
- LazyLoad
- Use node-key attribute
- API
  - Attributes

The tree selector of the dropdown menu,
it combines the functions of components `el-tree` and `el-select`.

Selector for tree structures.

When using the `check-strictly=true` attribute, any node can be checked,
otherwise only leaf nodes are supported.

## Multiple Selection

Multiple selection using clicks or checkbox.

## Disabled Selection

Disable options using the disabled field.

Use keyword filtering or custom filtering methods.
`filterMethod` can custom filter method for data,
`filterNodeMethod` can custom filter method for data node.

Contents of custom tree nodes.

Lazy loading of tree nodes, suitable for large data lists.

## Use node-key attribute

By default the `modelValue` is looking for the `value` key.
For a different data structure `node-key` must be provided to work normally.

Since this component combines the functions of components `el-tree` and `el-select`,
the original properties have not been changed, so no repetition here,
and please go to the original component to view the documentation.

| Attributes                              | Exposes                              | Events                              | Slots                              |
| --------------------------------------- | ------------------------------------ | ----------------------------------- | ---------------------------------- |
| [tree](./tree.md#attributes)            | [tree](./tree.md#exposes)            | [tree](./tree.md#events)            | [tree](./tree.md#slots)            |
| [select](./select.md#select-attributes) | [select](./select.md#select-exposes) | [select](./select.md#select-events) | [select](./select.md#select-slots) |

| Name                 | Description                                                                                                         | Type                     | Default |
| -------------------- | ------------------------------------------------------------------------------------------------------------------- | ------------------------ | ------- |
| cache-data ^(2.2.26) | The cached data of the lazy node, the structure is the same as the data, used to get the label of the unloaded data | ^[object]`CacheOption[]` | []      |

| Name                                 | Description                                                                                                          | Type                                                                                                                                                                                                                                                                                        |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| treeRef ^(2.11.3)                    | Tree component instance                                                                                              | `TreeInstance`                                                                                                                                                                                                                                                                              |
| selectRef ^(2.11.3)                  | Select component instance                                                                                            | `SelectInstance`                                                                                                                                                                                                                                                                            |
| filter ^(deprecated)                 | filter all tree nodes, filtered nodes will be hidden                                                                 | Accept a parameter which will be used as first parameter for filter-node-method                                                                                                                                                                                                             |
| updateKeyChildren ^(deprecated)      | set new data to node, only works when `node-key` is assigned                                                         | (key, data) Accept two parameters: 1. key of node 2. new data                                                                                                                                                                                                                               |
| getCheckedNodes ^(deprecated)        | If the node can be selected (`show-checkbox` is `true`), it returns the currently selected array of nodes            | (leafOnly, includeHalfChecked) Accept two boolean type parameters: 1. default value is `false`. If the parameter is `true`, it only returns the currently selected array of sub-nodes. 2. default value is `false`. If the parameter is `true`, the return value contains halfchecked nodes |
| setCheckedNodes ^(deprecated)        | set certain nodes to be checked, only works when `node-key` is assigned                                              | an array of nodes to be checked                                                                                                                                                                                                                                                             |
| getCheckedKeys ^(deprecated)         | If the node can be selected (`show-checkbox` is `true`), it returns the currently selected array of node's keys      | (leafOnly) Accept a boolean type parameter whose default value is `false`. If the parameter is `true`, it only returns the currently selected array of sub-nodes.                                                                                                                           |
| setCheckedKeys ^(deprecated)         | set certain nodes to be checked, only works when `node-key` is assigned                                              | (keys, leafOnly) Accept two parameters: 1. an array of node's keys to be checked 2. a boolean parameter. If set to `true`, only the checked status of leaf nodes will be set. The default value is `false`.                                                                                 |
| setChecked ^(deprecated)             | set node to be checked or not, only works when `node-key` is assigned                                                | (key/data, checked, deep) Accept three parameters: 1. node's key or data to be checked 2. a boolean typed parameter indicating checked or not. 3. a boolean typed parameter indicating deep or not.                                                                                         |
| getHalfCheckedNodes ^(deprecated)    | If the node can be selected (`show-checkbox` is `true`), it returns the currently half selected array of nodes       | —                                                                                                                                                                                                                                                                                           |
| getHalfCheckedKeys ^(deprecated)     | If the node can be selected (`show-checkbox` is `true`), it returns the currently half selected array of node's keys | —                                                                                                                                                                                                                                                                                           |
| getCurrentKey ^(deprecated)          | return the highlight node's key (null if no node is highlighted)                                                     | —                                                                                                                                                                                                                                                                                           |
| getCurrentNode ^(deprecated)         | return the highlight node's data (null if no node is highlighted)                                                    | —                                                                                                                                                                                                                                                                                           |
| setCurrentKey ^(deprecated)          | set highlighted node by key, only works when `node-key` is assigned                                                  | (key, shouldAutoExpandParent=true) 1. the node's key to be highlighted. If `null`, cancel the currently highlighted node 2. whether to automatically expand parent node                                                                                                                     |
| setCurrentNode ^(deprecated)         | set highlighted node, only works when `node-key` is assigned                                                         | (node, shouldAutoExpandParent=true) 1. the node to be highlighted 2. whether to automatically expand parent node                                                                                                                                                                            |
| getNode ^(deprecated)                | get node by data or key                                                                                              | (data) the node's data or key                                                                                                                                                                                                                                                               |
| remove ^(deprecated)                 | remove a node, only works when node-key is assigned                                                                  | (data) the node's data or node to be deleted                                                                                                                                                                                                                                                |
| append ^(deprecated)                 | append a child node to a given node in the tree                                                                      | (data, parentNode) 1. child node's data to be appended 2. parent node's data, key or node                                                                                                                                                                                                   |
| insertBefore ^(deprecated)           | insert a node before a given node in the tree                                                                        | (data, refNode) 1. node's data to be inserted 2. reference node's data, key or node                                                                                                                                                                                                         |
| insertAfter ^(deprecated)            | insert a node after a given node in the tree                                                                         | (data, refNode) 1. node's data to be inserted 2. reference node's data, key or node                                                                                                                                                                                                         |
| focus ^(deprecated)                  | focus the Input component                                                                                            | ^[Function]`() => void`                                                                                                                                                                                                                                                                     |
| blur ^(deprecated)                   | blur the Input component, and hide the dropdown                                                                      | ^[Function]`() => void`                                                                                                                                                                                                                                                                     |
| selectedLabel ^(2.8.5) ^(deprecated) | get the currently selected label                                                                                     | ^[object]`ComputedRef<string \| string[]>`                                                                                                                                                                                                                                                  |

<details>
  <summary>Show declarations</summary>

### check-strictly.vue

---
Title: Tree V2 virtualized tree
URL: https://element-plus.org/en-US/component/tree-v2
---

**Examples:**

Example 1 (ts):
```ts
type CacheOption = {
  value: string | number | boolean | object
  currentLabel: string | number
  isDisabled: boolean
}
```

Example 2 (vue):
```vue
<template>
  <el-tree-select
    v-model="value"
    :data="data"
    :render-after-expand="false"
    style="width: 240px"
  />
  <el-divider />
  show checkbox:
  <el-tree-select
    v-model="value"
    :data="data"
    :render-after-expand="false"
    show-checkbox
    style="width: 240px"
  />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref()

const data = [
  {
    value: '1',
    label: 'Level one 1',
    children: [
      {
        value: '1-1',
        label: 'Level two 1-1',
        children: [
          {
            value: '1-1-1',
            label: 'Level three 1-1-1',
          },
        ],
      },
    ],
  },
  {
    value: '2',
    label: 'Level one 2',
    children: [
      {
        value: '2-1',
        label: 'Level two 2-1',
        children: [
          {
            value: '2-1-1',
            label: 'Level three 2-1-1',
          },
        ],
      },
      {
        value: '2-2',
        label: 'Level two 2-2',
        children: [
          {
            value: '2-2-1',
            label: 'Level three 2-2-1',
          },
        ],
      },
    ],
  },
  {
    value: '3',
    label: 'Level one 3',
    children: [
      {
        value: '3-1',
        label: 'Level two 3-1',
        children: [
          {
            value: '3-1-1',
            label: 'Level three 3-1-1',
          },
        ],
      },
      {
        value: '3-2',
        label: 'Level two 3-2',
        children: [
          {
            value: '3-2-1',
            label: 'Level three 3-2-1',
          },
        ],
      },
    ],
  },
]
</script>
```

Example 3 (vue):
```vue
<template>
  <el-tree-select
    v-model="value"
    :data="data"
    check-strictly
    :render-after-expand="false"
    style="width: 240px"
  />
  <el-divider />
  show checkbox:
  <el-tree-select
    v-model="value"
    :data="data"
    check-strictly
    :render-after-expand="false"
    show-checkbox
    style="width: 240px"
  />
  <el-divider />
  show checkbox with `check-on-click-node`:
  <el-tree-select
    v-model="value"
    :data="data"
    check-strictly
    :render-after-expand="false"
    show-checkbox
    check-on-click-node
    style="width: 240px"
  />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref()

const data = [
  {
    value: '1',
    label: 'Level one 1',
    children: [
      {
        value: '1-1',
        label: 'Level two 1-1',
        children: [
          {
            value: '1-1-1',
            label: 'Level three 1-1-1',
          },
        ],
      },
    ],
  },
  {
    value: '2',
    label: 'Level one 2',
    children: [
      {
        value: '2-1',
        label: 'Level two 2-1',
        children: [
          {
            value: '2-1-1',
            label: 'Level three 2-1-1',
          },
        ],
      },
      {
        value: '2-2',
        label: 'Level two 2-2',
        children: [
          {
            value: '2-2-1',
            label: 'Level three 2-2-1',
          },
        ],
      },
    ],
  },
  {
    value: '3',
    label: 'Level one 3',
    children: [
      {
        value: '3-1',
        label: 'Level two 3-1',
        children: [
          {
            value: '3-1-1',
            label: 'Level three 3-1-1',
          },
        ],
      },
      {
        value: '3-2',
        label: 'Level two 3-2',
        children: [
          {
            value: '3-2-1',
            label: 'Level three 3-2-1',
          },
        ],
      },
    ],
  },
]
</script>
```

Example 4 (vue):
```vue
<template>
  <el-tree-select v-model="value" :data="data" style="width: 240px" />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref()

const data = [
  {
    value: '1',
    label: 'Level one 1',
    disabled: true,
    children: [
      {
        value: '1-1',
        label: 'Level two 1-1',
        disabled: true,
        children: [
          {
            disabled: true,
            value: '1-1-1',
            label: 'Level three 1-1-1',
          },
        ],
      },
    ],
  },
  {
    value: '2',
    label: 'Level one 2',
    children: [
      {
        value: '2-1',
        label: 'Level two 2-1',
        children: [
          {
            value: '2-1-1',
            label: 'Level three 2-1-1',
          },
        ],
      },
      {
        value: '2-2',
        label: 'Level two 2-2',
        children: [
          {
            value: '2-2-1',
            label: 'Level three 2-2-1',
          },
        ],
      },
    ],
  },
  {
    value: '3',
    label: 'Level one 3',
    children: [
      {
        value: '3-1',
        label: 'Level two 3-1',
        children: [
          {
            value: '3-1-1',
            label: 'Level three 3-1-1',
          },
        ],
      },
      {
        value: '3-2',
        label: 'Level two 3-2',
        children: [
          {
            value: '3-2-1',
            label: 'Level three 3-2-1',
          },
        ],
      },
    ],
  },
]
</script>
```

---

## Card

**URL:** llms-txt#card

**Contents:**
- Basic usage
- Simple card
- With images
- Shadow
- API
  - Attributes
  - Slots
- Vue Examples
  - basic.vue
  - shadow.vue

Integrate information in a card container.

Card includes title, content and operations.

The header part can be omitted.

Display richer content by adding some configs.

You can define when to show the card shadows

| Name                  | Description                                                    | Type                              | Default |
| --------------------- | -------------------------------------------------------------- | --------------------------------- | ------- |
| header                | title of the card. Also accepts a DOM passed by `slot#header`  | ^[string]                         | —       |
| footer ^(2.4.3)       | footer of the card. Also accepts a DOM passed by `slot#footer` | ^[string]                         | —       |
| body-style            | CSS style of card body                                         | ^[object]`CSSProperties`          | —       |
| header-class ^(2.9.8) | custom class name of card header                               | ^[string]                         | —       |
| body-class ^(2.3.10)  | custom class name of card body                                 | ^[string]                         | —       |
| footer-class ^(2.9.8) | custom class name of card footer                               | ^[string]                         | —       |
| shadow                | when to show card shadows                                      | ^[enum]`always \| never \| hover` | always  |

| Name    | Description                |
| ------- | -------------------------- |
| default | customize default content  |
| header  | content of the Card header |
| footer  | content of the Card footer |

---
Title: Carousel
URL: https://element-plus.org/en-US/component/carousel
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-card style="max-width: 480px">
    <template #header>
      <div class="card-header">
        <span>Card name</span>
      </div>
    </template>
    <p v-for="o in 4" :key="o" class="text item">{{ 'List item ' + o }}</p>
    <template #footer>Footer content</template>
  </el-card>
</template>
```

Example 2 (vue):
```vue
<template>
  <div class="flex flex-wrap gap-4">
    <el-card style="width: 480px" shadow="always">Always</el-card>
    <el-card style="width: 480px" shadow="hover">Hover</el-card>
    <el-card style="width: 480px" shadow="never">Never</el-card>
  </div>
</template>
```

Example 3 (vue):
```vue
<template>
  <el-card style="max-width: 480px">
    <p v-for="o in 4" :key="o" class="text item">{{ 'List item ' + o }}</p>
  </el-card>
</template>
```

Example 4 (vue):
```vue
<template>
  <el-card style="max-width: 480px">
    <template #header>Yummy hamburger</template>
    <img
      src="https://shadow.elemecdn.com/app/element/hamburger.9cf7b091-55e9-11e9-a976-7f4d0b07eef6.png"
      style="width: 100%"
    />
  </el-card>
</template>
```

---

## Input Number

**URL:** llms-txt#input-number

**Contents:**
- Basic usage
- Disabled
- Steps
- Step strictly
- Precision
- Size
- Controls Position
- Custom Icon ^(2.6.3)
- With prefix and suffix ^(2.8.4)
- API

Input numerical values with a customizable range.

Allows you to define incremental steps.

Use attribute `size` to set additional sizes with `large` or `small`.

## Custom Icon ^(2.6.3)

## With prefix and suffix ^(2.8.4)

| Name                          | Description                                      | Type                                          | Default                 |
| ----------------------------- | ------------------------------------------------ | --------------------------------------------- | ----------------------- |
| model-value / v-model         | binding value                                    | ^[number] / ^[null]                           | —                       |
| min                           | the minimum allowed value                        | ^[number]                                     | Number.MIN_SAFE_INTEGER |
| max                           | the maximum allowed value                        | ^[number]                                     | Number.MAX_SAFE_INTEGER |
| step                          | incremental step                                 | ^[number]                                     | 1                       |
| step-strictly                 | whether input value can only be multiple of step | ^[boolean]                                    | false                   |
| precision                     | precision of input value                         | ^[number]                                     | —                       |
| size                          | size of the component                            | ^[enum]`'large' \| 'default' \| 'small'`      | default                 |
| readonly ^(2.2.16)            | same as `readonly` in native input               | ^[boolean]                                    | false                   |
| disabled                      | whether the component is disabled                | ^[boolean]                                    | false                   |
| controls                      | whether to enable the control buttons            | ^[boolean]                                    | true                    |
| controls-position             | position of the control buttons                  | ^[enum]`'' \| 'right'`                        | —                       |
| name                          | same as `name` in native input                   | ^[string]                                     | —                       |
| aria-label ^(a11y) ^(2.7.2)   | same as `aria-label` in native input             | ^[string]                                     | —                       |
| placeholder                   | same as `placeholder` in native input            | ^[string]                                     | —                       |
| id                            | same as `id` in native input                     | ^[string]                                     | —                       |
| value-on-clear ^(2.2.0)       | value should be set when input box is cleared    | ^[number] / ^[null] / ^[enum]`'min' \| 'max'` | —                       |
| validate-event                | whether to trigger form validation               | ^[boolean]                                    | true                    |
| label ^(a11y) ^(deprecated)   | same as `aria-label` in native input             | ^[string]                                     | —                       |
| inputmode ^(2.10.3)           | same as `inputmode` in native input              | ^[string]                                     | —                       |
| align ^(2.10.5)               | alignment for the inner input text               | ^[enum]`'left' \| 'center' \| 'right'`        | 'center'                |
| disabled-scientific ^(2.10.5) | disables input of scientific notation (e.g. 'e') | ^[boolean]                                    | false                   |

| Name                   | Description                           |
| ---------------------- | ------------------------------------- |
| decrease-icon ^(2.6.3) | custom input box button decrease icon |
| increase-icon ^(2.6.3) | custom input box button increase icon |
| prefix ^(2.8.4)        | content as Input prefix               |
| suffix ^(2.8.4)        | content as Input suffix               |

| Name   | Description                     | Type                                                                                    |
| ------ | ------------------------------- | --------------------------------------------------------------------------------------- |
| change | triggers when the value changes | ^[Function]`(currentValue: number \| undefined, oldValue: number \| undefined) => void` |
| blur   | triggers when Input blurs       | ^[Function]`(event: FocusEvent) => void`                                                |
| focus  | triggers when Input focuses     | ^[Function]`(event: FocusEvent) => void`                                                |

| Name  | Description                      | Type                    |
| ----- | -------------------------------- | ----------------------- |
| focus | get focus the input component    | ^[Function]`() => void` |
| blur  | remove focus the input component | ^[Function]`() => void` |

### step-strictly.vue

### with-prefix-suffix.vue

---
Title: InputTag
URL: https://element-plus.org/en-US/component/input-tag
---

**Examples:**

Example 1 (vue):
```vue
<template>
  <el-input-number v-model="num" :min="1" :max="10" @change="handleChange" />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const num = ref(1)
const handleChange = (value: number | undefined) => {
  console.log(value)
}
</script>
```

Example 2 (vue):
```vue
<template>
  <div class="flex flex-wrap items-center gap-4">
    <el-input-number
      v-model="num"
      :min="1"
      :max="10"
      controls-position="right"
      size="large"
      @change="handleChange"
    />
    <el-input-number
      v-model="num"
      :min="1"
      :max="10"
      controls-position="right"
      @change="handleChange"
    />
    <el-input-number
      v-model="num"
      :min="1"
      :max="10"
      size="small"
      controls-position="right"
      @change="handleChange"
    />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const num = ref(1)
const handleChange = (value: number | undefined) => {
  console.log(value)
}
</script>
```

Example 3 (vue):
```vue
<template>
  <el-space direction="vertical">
    <el-space>
      <el-input-number v-model="num" />
      <el-input-number v-model="num">
        <template #decrease-icon>
          <el-icon>
            <ArrowDown />
          </el-icon>
        </template>
        <template #increase-icon>
          <el-icon>
            <ArrowUp />
          </el-icon>
        </template>
      </el-input-number>
    </el-space>
    <el-space>
      <el-input-number v-model="num" controls-position="right" />
      <el-input-number v-model="num" controls-position="right">
        <template #decrease-icon>
          <el-icon>
            <Minus />
          </el-icon>
        </template>
        <template #increase-icon>
          <el-icon>
            <Plus />
          </el-icon>
        </template>
      </el-input-number>
    </el-space>
  </el-space>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { ArrowDown, ArrowUp, Minus, Plus } from '@element-plus/icons-vue'

const num = ref(1)
</script>
```

Example 4 (vue):
```vue
<template>
  <el-input-number v-model="num" :disabled="true" />
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const num = ref(1)
</script>
```

---

## Commit Examples

**URL:** llms-txt#commit-examples

**Contents:**
- Why this chapter exists
  - Rule for writing commit message

## Why this chapter exists

Please refer to [Conventional Commits](https://www.conventionalcommits.org/) for more information.

A good commit message enables us:

1. To understand what the contributor is trying to do
2. Automatically generates change log

### Rule for writing commit message

---

## Slider

**URL:** llms-txt#slider

**Contents:**
- Basic usage
- Discrete values
- Slider with input box
- Sizes
- Placement
- Range selection
- Vertical mode
- Show marks
- API
  - Attributes

Drag the slider within a fixed range.

The current value is displayed when the slider is being dragged.

The options can be discrete.

## Slider with input box

Set value via a input box.

You can custom tooltip placement.

Selecting a range of values is supported.

| Name                        | Description                                                                                                                                          | Type                                                                                                                                                                        | Default |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| model-value / v-model       | binding value                                                                                                                                        | ^[number] / ^[object]`number[]`                                                                                                                                             | 0       |
| min                         | minimum value                                                                                                                                        | ^[number]                                                                                                                                                                   | 0       |
| max                         | maximum value                                                                                                                                        | ^[number]                                                                                                                                                                   | 100     |
| disabled                    | whether Slider is disabled                                                                                                                           | ^[boolean]                                                                                                                                                                  | false   |
| step                        | step size                                                                                                                                            | ^[number]                                                                                                                                                                   | 1       |
| show-input                  | whether to display an input box, works when `range` is false                                                                                         | ^[boolean]                                                                                                                                                                  | false   |
| show-input-controls         | whether to display control buttons when `show-input` is true                                                                                         | ^[boolean]                                                                                                                                                                  | true    |
| size                        | size of the slider wrapper, will not work in vertical mode                                                                                           | ^[enum]`'' \| 'large' \| 'default' \| 'small'`                                                                                                                              | default |
| input-size                  | size of the input box, when set `size`, the default is the value of `size`                                                                           | ^[enum]`'' \| 'large' \| 'default' \| 'small'`                                                                                                                              | default |
| show-stops                  | whether to display breakpoints                                                                                                                       | ^[boolean]                                                                                                                                                                  | false   |
| show-tooltip                | whether to display tooltip value                                                                                                                     | ^[boolean]                                                                                                                                                                  | true    |
| format-tooltip              | format to display tooltip value                                                                                                                      | ^[Function]`(value: number) => number \| string`                                                                                                                            | —       |
| range                       | whether to select a range                                                                                                                            | ^[boolean]                                                                                                                                                                  | false   |
| vertical                    | vertical mode                                                                                                                                        | ^[boolean]                                                                                                                                                                  | false   |
| height                      | slider height, required in vertical mode                                                                                                             | ^[string]                                                                                                                                                                   | —       |
| aria-label ^(a11y) ^(2.7.2) | native `aria-label` attribute                                                                                                                        | ^[string]                                                                                                                                                                   | —       |
| range-start-label           | when `range` is true, screen reader label for the start of the range                                                                                 | ^[string]                                                                                                                                                                   | —       |
| range-end-label             | when `range` is true, screen reader label for the end of the range                                                                                   | ^[string]                                                                                                                                                                   | —       |
| format-value-text           | format to display the `aria-valuenow` attribute for screen readers                                                                                   | ^[Function]`(value: number) => string`                                                                                                                                      | —       |
| tooltip-class               | custom class name for the tooltip                                                                                                                    | ^[string]                                                                                                                                                                   | —       |
| placement                   | position of Tooltip                                                                                                                                  | ^[enum]`'top' \| 'top-start' \| 'top-end' \| 'bottom' \| 'bottom-start' \| 'bottom-end' \| 'left' \| 'left-start' \| 'left-end' \| 'right' \| 'right-start' \| 'right-end'` | top     |
| marks                       | marks, type of key must be `number` and must in closed interval `[min, max]`, each mark can custom style                                             | ^[object]`SliderMarks`                                                                                                                                                      | —       |
| validate-event              | whether to trigger form validation                                                                                                                   | ^[boolean]                                                                                                                                                                  | true    |
| persistent ^(2.9.5)         | when slider tooltip inactive and `persistent` is `false` , tooltip will be destroyed. `persistent` always be `false` when `show-tooltip ` is `false` | ^[boolean]                                                                                                                                                                  | true    |
| label ^(a11y) ^(deprecated) | native `aria-label` attribute                                                                                                                        | ^[string]                                                                                                                                                                   | —       |

| Name   | Description                                                                                                       | Type                                               |
| ------ | ----------------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| change | triggers when the value changes (if the mouse is being dragged, this event only fires when the mouse is released) | ^[Function]`(value: Arrayable<number>) => boolean` |
| input  | triggers when the data changes (It'll be emitted in real time during sliding)                                     | ^[Function]`(value: Arrayable<number>) => boolean` |

<details>
  <summary>Show declarations</summary>

### discrete-values.vue

### range-selection.vue

### slider-with-input-box.vue

### vertical-mode.vue

---
Title: Space
URL: https://element-plus.org/en-US/component/space
---

**Examples:**

Example 1 (ts):
```ts
type SliderMarks = Record<number, string | { style: CSSProperties; label: any }>
type Arrayable<T> = T | T[]
```

Example 2 (vue):
```vue
<template>
  <div class="slider-demo-block">
    <span class="demonstration">Default value</span>
    <el-slider v-model="value1" />
  </div>
  <div class="slider-demo-block">
    <span class="demonstration">Customized initial value</span>
    <el-slider v-model="value2" />
  </div>
  <div class="slider-demo-block">
    <span class="demonstration">Hide Tooltip</span>
    <el-slider v-model="value3" :show-tooltip="false" />
  </div>
  <div class="slider-demo-block">
    <span class="demonstration">Format Tooltip</span>
    <el-slider v-model="value4" :format-tooltip="formatTooltip" />
  </div>
  <div class="slider-demo-block">
    <span class="demonstration">Disabled</span>
    <el-slider v-model="value5" disabled />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value1 = ref(0)
const value2 = ref(0)
const value3 = ref(0)
const value4 = ref(0)
const value5 = ref(0)

const formatTooltip = (val: number) => {
  return val / 100
}
</script>

<style scoped>
.slider-demo-block {
  max-width: 600px;
  display: flex;
  align-items: center;
}
.slider-demo-block .el-slider {
  margin-top: 0;
  margin-left: 12px;
}
.slider-demo-block .demonstration {
  font-size: 14px;
  color: var(--el-text-color-secondary);
  line-height: 44px;
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-bottom: 0;
}
.slider-demo-block .demonstration + .el-slider {
  flex: 0 0 70%;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <div class="slider-demo-block">
    <span class="demonstration">Breakpoints not displayed</span>
    <el-slider v-model="value1" :step="10" />
  </div>
  <div class="slider-demo-block">
    <span class="demonstration">Breakpoints displayed</span>
    <el-slider v-model="value2" :step="10" show-stops />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value1 = ref(0)
const value2 = ref(0)
</script>

<style scoped>
.slider-demo-block {
  max-width: 600px;
  display: flex;
  align-items: center;
}
.slider-demo-block .el-slider {
  margin-top: 0;
  margin-left: 12px;
}
.slider-demo-block .demonstration {
  font-size: 14px;
  color: var(--el-text-color-secondary);
  line-height: 44px;
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-bottom: 0;
}
.slider-demo-block .demonstration + .el-slider {
  flex: 0 0 70%;
}
</style>
```

Example 4 (vue):
```vue
<template>
  <div class="slider-demo-block">
    <el-slider v-model="value1" />
  </div>
  <div class="slider-demo-block">
    <el-slider v-model="value2" placement="bottom" />
  </div>
  <div class="slider-demo-block">
    <el-slider v-model="value3" placement="right" />
  </div>
  <div class="slider-demo-block">
    <el-slider v-model="value4" placement="left" />
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value1 = ref(0)
const value2 = ref(0)
const value3 = ref(0)
const value4 = ref(0)
</script>

<style scoped>
.slider-demo-block {
  max-width: 600px;
  display: flex;
  align-items: center;
}
.slider-demo-block .el-slider {
  margin-top: 0;
  margin-left: 12px;
}
</style>
```

---

## Upload

**URL:** llms-txt#upload

**Contents:**
- Basic Usage
- Cover Previous File
- User Avatar
- Photo Wall
- Custom Thumbnail
- File List with Thumbnail
- File List Control
- Drag to Upload
- Manual Upload
- API

Upload files by clicking or drag-and-drop.

## Cover Previous File

Use `before-upload` hook to limit the upload file format and size.

Use `list-type` to change the fileList style.

Use `scoped-slot` to change default thumbnail template.

## File List with Thumbnail

Use `on-change` hook function to control upload file list.

You can drag your file to a certain area to upload it.

| Name                          | Description                                                                                                                                                                           | Type                                                                                                                                       | Default                                                                                                            |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------ |
| action ^(required)            | request URL.                                                                                                                                                                          | ^[string]                                                                                                                                  | #                                                                                                                  |
| headers                       | request headers.                                                                                                                                                                      | ^[object]`Headers \| Record<string, any>`                                                                                                  | —                                                                                                                  |
| method                        | set upload request method.                                                                                                                                                            | ^[string]                                                                                                                                  | post                                                                                                               |
| multiple                      | whether uploading multiple files is permitted.                                                                                                                                        | ^[boolean]                                                                                                                                 | false                                                                                                              |
| data                          | additions options of request. support `Awaitable` data and `Function` since v2.3.13.                                                                                                  | ^[object]`Record<string, any> \| Awaitable<Record<string, any>>` / ^[Function]`(rawFile: UploadRawFile) => Awaitable<Record<string, any>>` | {}                                                                                                                 |
| name                          | key name for uploaded file.                                                                                                                                                           | ^[string]                                                                                                                                  | file                                                                                                               |
| with-credentials              | whether cookies are sent.                                                                                                                                                             | ^[boolean]                                                                                                                                 | false                                                                                                              |
| show-file-list                | whether to show the uploaded file list.                                                                                                                                               | ^[boolean]                                                                                                                                 | true                                                                                                               |
| drag                          | whether to activate drag and drop mode.                                                                                                                                               | ^[boolean]                                                                                                                                 | false                                                                                                              |
| accept                        | accepted [file types](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#attr-accept), will not work when `thumbnail-mode === true`.                                     | ^[string]                                                                                                                                  | ''                                                                                                                 |
| crossorigin                   | native attribute [crossorigin](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/crossorigin).                                                                             | ^[enum]`'' \| 'anonymous' \| 'use-credentials'`                                                                                            | —                                                                                                                  |
| on-preview                    | hook function when clicking the uploaded files.                                                                                                                                       | ^[Function]`(uploadFile: UploadFile) => void`                                                                                              | —                                                                                                                  |
| on-remove                     | hook function when files are removed.                                                                                                                                                 | ^[Function]`(uploadFile: UploadFile, uploadFiles: UploadFiles) => void`                                                                    | —                                                                                                                  |
| on-success                    | hook function when uploaded successfully.                                                                                                                                             | ^[Function]`(response: any, uploadFile: UploadFile, uploadFiles: UploadFiles) => void`                                                     | —                                                                                                                  |
| on-error                      | hook function when some errors occurs.                                                                                                                                                | ^[Function]`(error: Error, uploadFile: UploadFile, uploadFiles: UploadFiles) => void`                                                      | —                                                                                                                  |
| on-progress                   | hook function when some progress occurs.                                                                                                                                              | ^[Function]`(evt: UploadProgressEvent, uploadFile: UploadFile, uploadFiles: UploadFiles) => void`                                          | —                                                                                                                  |
| on-change                     | hook function when select file or upload file success or upload file fail.                                                                                                            | ^[Function]`(uploadFile: UploadFile, uploadFiles: UploadFiles) => void`                                                                    | —                                                                                                                  |
| on-exceed                     | hook function when limit is exceeded.                                                                                                                                                 | ^[Function]`(files: File[], uploadFiles: UploadUserFile[]) => void`                                                                        | —                                                                                                                  |
| before-upload                 | hook function before uploading with the file to be uploaded as its parameter. If `false` is returned or a `Promise` is returned and then is rejected, uploading will be aborted.      | ^[Function]`(rawFile: UploadRawFile) => Awaitable<void \| undefined \| null \| boolean \| File \| Blob>`                                   | —                                                                                                                  |
| before-remove                 | hook function before removing a file with the file and file list as its parameters. If `false` is returned or a `Promise` is returned and then is rejected, removing will be aborted. | ^[Function]`(uploadFile: UploadFile, uploadFiles: UploadFiles) => Awaitable<boolean>`                                                      | —                                                                                                                  |
| file-list / v-model:file-list | default uploaded files.                                                                                                                                                               | ^[object]`UploadUserFile[]`                                                                                                                | []                                                                                                                 |
| list-type                     | type of file list.                                                                                                                                                                    | ^[enum]`'text' \| 'picture' \| 'picture-card'`                                                                                             | text                                                                                                               |
| auto-upload                   | whether to auto upload file.                                                                                                                                                          | ^[boolean]                                                                                                                                 | true                                                                                                               |
| http-request                  | override default xhr behavior, allowing you to implement your own upload-file's request.                                                                                              | ^[Function]`(options: UploadRequestOptions) => XMLHttpRequest \| Promise<unknown>`                                                         | ajaxUpload [see](https://github.com/element-plus/element-plus/blob/dev/packages/components/upload/src/ajax.ts#L55) |
| disabled                      | whether to disable upload.                                                                                                                                                            | ^[boolean]                                                                                                                                 | false                                                                                                              |
| limit                         | maximum number of uploads allowed.                                                                                                                                                    | ^[number]                                                                                                                                  | —                                                                                                                  |

| Name    | Description                         | Type                                           |
| ------- | ----------------------------------- | ---------------------------------------------- |
| default | customize default content.          | -                                              |
| trigger | content which triggers file dialog. | -                                              |
| tip     | content of tips.                    | -                                              |
| file    | content of thumbnail template.      | ^[object]`{ file: UploadFile, index: number }` |

| Name         | Description                                                                                            | Type                                                                              |
| ------------ | ------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------- |
| abort        | cancel upload request.                                                                                 | ^[Function]`(file: UploadFile) => void`                                           |
| submit       | upload the file list manually.                                                                         | ^[Function]`() => void`                                                           |
| clearFiles   | clear the file list (this method is not supported in the `before-upload` hook).                        | ^[Function]`(status?: UploadStatus[]) => void`                                    |
| handleStart  | select the file manually.                                                                              | ^[Function]`(rawFile: UploadRawFile) => void`                                     |
| handleRemove | remove the file manually. `file` and `rawFile` has been merged. `rawFile` will be removed in `v2.2.0`. | ^[Function]`(file: UploadFile \| UploadRawFile, rawFile?: UploadRawFile) => void` |

<details>
  <summary>Show declarations</summary>

### custom-thumbnail.vue

### drag-and-drop.vue

### file-list-with-thumbnail.vue

---
Title: Watermark
URL: https://element-plus.org/en-US/component/watermark
---

**Examples:**

Example 1 (ts):
```ts
type UploadFiles = UploadFile[]

type UploadUserFile = Omit<UploadFile, 'status' | 'uid'> &
  Partial<Pick<UploadFile, 'status' | 'uid'>>

type UploadStatus = 'ready' | 'uploading' | 'success' | 'fail'

type Awaitable<T> = Promise<T> | T

type Mutable<T> = { -readonly [P in keyof T]: T[P] }

interface UploadFile {
  name: string
  percentage?: number
  status: UploadStatus
  size?: number
  response?: unknown
  uid: number
  url?: string
  raw?: UploadRawFile
}

interface UploadProgressEvent extends ProgressEvent {
  percent: number
}

interface UploadRawFile extends File {
  uid: number
  isDirectory?: boolean
}

interface UploadRequestOptions {
  action: string
  method: string
  data: Record<string, string | Blob | [string | Blob, string]>
  filename: string
  file: UploadRawFile
  headers: Headers | Record<string, string | number | null | undefined>
  onError: (evt: UploadAjaxError) => void
  onProgress: (evt: UploadProgressEvent) => void
  onSuccess: (response: any) => void
  withCredentials: boolean
}
```

Example 2 (vue):
```vue
<template>
  <el-upload
    class="avatar-uploader"
    action="https://run.mocky.io/v3/9d059bf9-4660-45f2-925d-ce80ad6c4d15"
    :show-file-list="false"
    :on-success="handleAvatarSuccess"
    :before-upload="beforeAvatarUpload"
  >
    <img v-if="imageUrl" :src="imageUrl" class="avatar" />
    <el-icon v-else class="avatar-uploader-icon"><Plus /></el-icon>
  </el-upload>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { ElMessage } from 'element-plus'
import { Plus } from '@element-plus/icons-vue'

import type { UploadProps } from 'element-plus'

const imageUrl = ref('')

const handleAvatarSuccess: UploadProps['onSuccess'] = (
  response,
  uploadFile
) => {
  imageUrl.value = URL.createObjectURL(uploadFile.raw!)
}

const beforeAvatarUpload: UploadProps['beforeUpload'] = (rawFile) => {
  if (rawFile.type !== 'image/jpeg') {
    ElMessage.error('Avatar picture must be JPG format!')
    return false
  } else if (rawFile.size / 1024 / 1024 > 2) {
    ElMessage.error('Avatar picture size can not exceed 2MB!')
    return false
  }
  return true
}
</script>

<style scoped>
.avatar-uploader .avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>

<style>
.avatar-uploader .el-upload {
  border: 1px dashed var(--el-border-color);
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  transition: var(--el-transition-duration-fast);
}

.avatar-uploader .el-upload:hover {
  border-color: var(--el-color-primary);
}

.el-icon.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  text-align: center;
}
</style>
```

Example 3 (vue):
```vue
<template>
  <el-upload
    v-model:file-list="fileList"
    class="upload-demo"
    action="https://run.mocky.io/v3/9d059bf9-4660-45f2-925d-ce80ad6c4d15"
    multiple
    :on-preview="handlePreview"
    :on-remove="handleRemove"
    :before-remove="beforeRemove"
    :limit="3"
    :on-exceed="handleExceed"
  >
    <el-button type="primary">Click to upload</el-button>
    <template #tip>
      <div class="el-upload__tip">
        jpg/png files with a size less than 500KB.
      </div>
    </template>
  </el-upload>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { ElMessage, ElMessageBox } from 'element-plus'

import type { UploadProps, UploadUserFile } from 'element-plus'

const fileList = ref<UploadUserFile[]>([
  {
    name: 'element-plus-logo.svg',
    url: 'https://element-plus.org/images/element-plus-logo.svg',
  },
  {
    name: 'element-plus-logo2.svg',
    url: 'https://element-plus.org/images/element-plus-logo.svg',
  },
])

const handleRemove: UploadProps['onRemove'] = (file, uploadFiles) => {
  console.log(file, uploadFiles)
}

const handlePreview: UploadProps['onPreview'] = (uploadFile) => {
  console.log(uploadFile)
}

const handleExceed: UploadProps['onExceed'] = (files, uploadFiles) => {
  ElMessage.warning(
    `The limit is 3, you selected ${files.length} files this time, add up to ${
      files.length + uploadFiles.length
    } totally`
  )
}

const beforeRemove: UploadProps['beforeRemove'] = (uploadFile, uploadFiles) => {
  return ElMessageBox.confirm(
    `Cancel the transfer of ${uploadFile.name} ?`
  ).then(
    () => true,
    () => false
  )
}
</script>
```

Example 4 (vue):
```vue
<template>
  <el-upload action="#" list-type="picture-card" :auto-upload="false">
    <el-icon><Plus /></el-icon>

    <template #file="{ file }">
      <div>
        <img class="el-upload-list__item-thumbnail" :src="file.url" alt="" />
        <span class="el-upload-list__item-actions">
          <span
            class="el-upload-list__item-preview"
            @click="handlePictureCardPreview(file)"
          >
            <el-icon><zoom-in /></el-icon>
          </span>
          <span
            v-if="!disabled"
            class="el-upload-list__item-delete"
            @click="handleDownload(file)"
          >
            <el-icon><Download /></el-icon>
          </span>
          <span
            v-if="!disabled"
            class="el-upload-list__item-delete"
            @click="handleRemove(file)"
          >
            <el-icon><Delete /></el-icon>
          </span>
        </span>
      </div>
    </template>
  </el-upload>

  <el-dialog v-model="dialogVisible">
    <img w-full :src="dialogImageUrl" alt="Preview Image" />
  </el-dialog>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { Delete, Download, Plus, ZoomIn } from '@element-plus/icons-vue'

import type { UploadFile } from 'element-plus'

const dialogImageUrl = ref('')
const dialogVisible = ref(false)
const disabled = ref(false)

const handleRemove = (file: UploadFile) => {
  console.log(file)
}

const handlePictureCardPreview = (file: UploadFile) => {
  dialogImageUrl.value = file.url!
  dialogVisible.value = true
}

const handleDownload = (file: UploadFile) => {
  console.log(file)
}
</script>
```

---

## DateTimePicker

**URL:** llms-txt#datetimepicker

**Contents:**
- Date and time
- DateTime Formats
- Date and time formats in dropdown panel
- Date and time range
- Default time value for start date and end date
- Custom icon ^(2.8.0)
- API
  - Attributes
  - Events
  - Slots

Select date and time in one picker.

Use `format` to control displayed text's format in the input box. Use `value-format` to control binding value's format.

By default, the component accepts and emits a `Date` object.

Check the list [here](https://day.js.org/docs/en/display/format#list-of-all-available-formats) of all available formats of Day.js.

## Date and time formats in dropdown panel

Use `date-format` and `time-format` to control displayed text's format in the dropdown panel's input box.

## Date and time range

## Default time value for start date and end date

## Custom icon ^(2.8.0)

Custom icons available with slots.

| Name                         | Description                                                                                                          | Type                                                                                           | Default                            |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- | ---------------------------------- |
| model-value / v-model        | binding value, if it is an `range` picker, the length of the array should be 2                                       | ^[number] / ^[string] / ^[Date] / ^[array]`number[] \| string[] \| Date[]`                     | ''                                 |
| readonly                     | whether DatePicker is read only                                                                                      | ^[boolean]                                                                                     | false                              |
| disabled                     | whether DatePicker is disabled                                                                                       | ^[boolean]                                                                                     | false                              |
| editable                     | whether the input is editable                                                                                        | ^[boolean]                                                                                     | true                               |
| clearable                    | whether to show clear button                                                                                         | ^[boolean]                                                                                     | true                               |
| size                         | size of Input                                                                                                        | ^[enum]`'large' \| 'default' \| 'small'`                                                       | default                            |
| placeholder                  | placeholder in non-range mode                                                                                        | ^[string]                                                                                      | —                                  |
| start-placeholder            | placeholder for the start date in range mode                                                                         | ^[string]                                                                                      | —                                  |
| end-placeholder              | placeholder for the end date in range mode                                                                           | ^[string]                                                                                      | —                                  |
| arrow-control                | whether to pick time using arrow buttons                                                                             | ^[boolean]                                                                                     | false                              |
| type                         | type of the picker                                                                                                   | ^[enum]`'year' \| 'month' \| 'date' \| 'datetime' \| 'week' \| 'datetimerange' \| 'daterange'` | date                               |
| format                       | format of the displayed value in the input box                                                                       | ^[string] see [date formats](/en-US/component/date-picker#date-formats)                        | YYYY-MM-DD HH:mm:ss                |
| popper-class                 | custom class name for DateTimePicker's dropdown                                                                      | ^[string]                                                                                      | —                                  |
| popper-style                 | custom style for DateTimePicker's dropdown                                                                           | ^[string] / ^[object]                                                                          | —                                  |
| popper-options               | Customized popper option see more at [popper.js](https://popper.js.org/docs/v2/)                                     | ^[object]`Partial<PopperOptions>`                                                              | {}                                 |
| fallback-placements ^(2.8.4) | list of possible positions for Tooltip [popper.js](https://popper.js.org/docs/v2/modifiers/flip/#fallbackplacements) | ^[array]`Placement[]`                                                                          | ['bottom', 'top', 'right', 'left'] |
| placement ^(2.8.4)           | position of dropdown                                                                                                 | `Placement`                                                                                    | bottom                             |
| range-separator              | range separator                                                                                                      | ^[string]                                                                                      | '-'                                |
| default-value                | optional, default date of the calendar                                                                               | ^[object]`Date \| [Date, Date]`                                                                | —                                  |
| default-time                 | the default time value after picking a date. Time `00:00:00` will be used if not specified                           | ^[object]`Date \| [Date, Date]`                                                                | —                                  |
| value-format                 | optional, format of binding value. If not specified, the binding value will be a Date object                         | ^[string] see [date formats](https://day.js.org/docs/en/display/format)                        | —                                  |
| date-format ^(2.4.0)         | optional, format of the date displayed in input's inner panel                                                        | ^[string] see [date formats](https://day.js.org/docs/en/display/format)                        | YYYY-MM-DD                         |
| time-format ^(2.4.0)         | optional, format of the time displayed in input's inner panel                                                        | ^[string] see [date formats](https://day.js.org/docs/en/display/format)                        | HH:mm:ss                           |
| id                           | same as `id` in native input                                                                                         | ^[string] / ^[object]`[string, string]`                                                        | —                                  |
| name                         | same as `name` in native input                                                                                       | ^[string]                                                                                      | —                                  |
| unlink-panels                | unlink two date-panels in range-picker                                                                               | ^[boolean]                                                                                     | false                              |
| prefix-icon                  | Custom prefix icon component                                                                                         | ^[string] / `Component`                                                                        | Date                               |
| clear-icon                   | Custom clear icon component                                                                                          | ^[string] / `Component`                                                                        | CircleClose                        |
| shortcuts                    | an object array to set shortcut options                                                                              | ^[object]`Array<{ text: string, value: Date \| Function }>`                                    | —                                  |
| disabled-date                | a function determining if a date is disabled with that date as its parameter. Should return a Boolean                | ^[Function]`(data: Date) => boolean`                                                           | —                                  |
| disabled-hours               | To specify the array of hours that cannot be selected                                                                | ^[Function]`(role: string, comparingDate?: Dayjs) => number[]`                                 | —                                  |
| disabled-minutes             | To specify the array of minutes that cannot be selected                                                              | ^[Function]`(hour: number, role: string, comparingDate?: Dayjs) => number[]`                   | —                                  |
| disabled-seconds             | To specify the array of seconds that cannot be selected                                                              | ^[Function]`(hour: number, minute: number, role: string, comparingDate?: Dayjs) => number[]`   | —                                  |
| cell-class-name              | set custom className                                                                                                 | ^[Function]`(data: Date) => string`                                                            | —                                  |
| teleported                   | whether datetime-picker dropdown is teleported to the body                                                           | ^[boolean]                                                                                     | true                               |
| empty-values ^(2.7.0)        | empty values of component, [see config-provider](/en-US/component/config-provider#empty-values-configurations)       | ^[array]                                                                                       | —                                  |
| value-on-clear ^(2.7.0)      | clear return value, [see config-provider](/en-US/component/config-provider#empty-values-configurations)              | ^[string] / ^[number] / ^[boolean] / ^[Function]                                               | —                                  |
| show-now ^(2.8.7)            | whether to show the now button                                                                                       | ^[boolean]                                                                                     | true                               |
| show-footer ^(2.10.5)        | whether to show footer where the date picker is one ^[enum]`'datetime' \| 'datetimerange'`                           | ^[boolean]                                                                                     | true                               |
| show-confirm ^(2.11.0)       | whether to show the confirm button                                                                                   | ^[boolean]                                                                                     | true                               |
| show-week-number ^(2.10.3)   | show the week number besides the week                                                                                | `boolean`                                                                                      | false                              |

| Name            | Description                                                           | Parameters                                                                                |
| --------------- | --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| change          | triggers when user confirms the value or click outside                | ^[Function]`(val: typeof v-model) => void`                                                |
| blur            | triggers when Input blurs                                             | ^[Function]`(e: FocusEvent) => void`                                                      |
| focus           | triggers when Input focuses                                           | ^[Function]`(e: FocusEvent) => void`                                                      |
| clear ^(2.7.7)  | triggers when the clear icon is clicked in a clearable DateTimePicker | ^[Function]`() => void`                                                                   |
| calendar-change | triggers when the calendar selected date is changed. Only for `range` | ^[Function]`(val: [Date, null \| Date]) => void`                                          |
| panel-change    | triggers when the navigation button click.                            | ^[Function]`(date: Date \| [Date, Date], mode: 'month' \| 'year', view?: string) => void` |
| visible-change  | triggers when the DateTimePicker's dropdown appears/disappears        | ^[Function]`(visibility: boolean) => void`                                                |

| Name                | Description                    |
| ------------------- | ------------------------------ |
| default             | custom cell content            |
| range-separator     | custom range separator content |
| prev-month ^(2.8.0) | prev month icon                |
| next-month ^(2.8.0) | next month icon                |
| prev-year ^(2.8.0)  | prev year icon                 |
| next-year ^(2.8.0)  | next year icon                 |

| Method        | Description                    | Type                    |
| ------------- | ------------------------------ | ----------------------- |
| focus         | focus the DatePicker component | ^[Function]`() => void` |
| blur ^(2.8.7) | blur the DatePicker component  | ^[Function]`() => void` |

<details>
  <summary>Show declarations</summary>

### date-and-time-formats-panel.vue

### date-and-time-formats.vue

### date-and-time-range.vue

### date-and-time.vue

---
Title: Descriptions
URL: https://element-plus.org/en-US/component/descriptions
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
  <div class="demo-datetime-picker-icon">
    <div class="block">
      <el-date-picker
        v-model="value1"
        type="datetime"
        placeholder="Pick a Date"
        format="YYYY-MM-DD HH:mm:ss"
        date-format="MMM DD, YYYY"
        time-format="HH:mm"
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
      <el-date-picker
        v-model="value2"
        type="datetimerange"
        start-placeholder="Start date"
        end-placeholder="End date"
        format="YYYY-MM-DD HH:mm:ss"
        date-format="YYYY/MM/DD ddd"
        time-format="A hh:mm:ss"
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
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { CaretLeft, CaretRight } from '@element-plus/icons-vue'

const value1 = ref('')
const value2 = ref('')
</script>

<style scoped>
.demo-datetime-picker-icon {
  display: flex;
  width: 100%;
  padding: 0;
  flex-wrap: wrap;
  justify-content: space-around;
  align-items: stretch;
}
.demo-datetime-picker-icon .block {
  padding: 30px 0;
  text-align: center;
  min-width: 300px;
  flex: 1;
}
.line {
  width: 1px;
  background-color: var(--el-border-color);
}

@media (max-width: 768px) {
  .demo-datetime-picker-icon .block {
    flex: 100%;
    border-bottom: solid 1px var(--el-border-color);
  }

  .demo-datetime-picker-icon .block:last-child {
    border-bottom: none;
  }

  .line {
    display: none;
  }

  :deep(.el-date-editor.el-input) {
    width: 100%;
  }

  :deep(.el-date-editor.el-input__wrapper) {
    width: 100%;
    max-width: 300px;
  }
}
</style>
```

Example 3 (vue):
```vue
<template>
  <div class="demo-datetime-picker">
    <div class="block">
      <el-date-picker
        v-model="value1"
        type="datetime"
        placeholder="Pick a Date"
        format="YYYY-MM-DD HH:mm:ss"
        date-format="MMM DD, YYYY"
        time-format="HH:mm"
      />
    </div>
    <div class="line" />
    <div class="block">
      <el-date-picker
        v-model="value2"
        type="datetimerange"
        start-placeholder="Start date"
        end-placeholder="End date"
        format="YYYY-MM-DD HH:mm:ss"
        date-format="YYYY/MM/DD ddd"
        time-format="A hh:mm:ss"
      />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value1 = ref('')
const value2 = ref('')
</script>

<style scoped>
.demo-datetime-picker {
  display: flex;
  width: 100%;
  padding: 0;
  flex-wrap: wrap;
  justify-content: space-around;
  align-items: stretch;
}
.demo-datetime-picker .block {
  padding: 30px 0;
  text-align: center;
  min-width: 300px;
  flex: 1;
}
.line {
  width: 1px;
  background-color: var(--el-border-color);
}

@media (max-width: 768px) {
  .demo-datetime-picker .block {
    flex: 100%;
    border-bottom: solid 1px var(--el-border-color);
  }

  .demo-datetime-picker .block:last-child {
    border-bottom: none;
  }

  .line {
    display: none;
  }

  :deep(.el-date-editor.el-input) {
    width: 100%;
  }

  :deep(.el-date-editor.el-input__wrapper) {
    width: 100%;
    max-width: 300px;
  }
}
</style>
```

Example 4 (vue):
```vue
<template>
  <div class="demo-datetime-picker">
    <div class="block">
      <span class="demonstration">Emits Date object</span>
      <div class="demonstration">Value: {{ value1 }}</div>
      <el-date-picker
        v-model="value1"
        type="datetime"
        placeholder="Pick a Date"
        format="YYYY/MM/DD HH:mm:ss"
      />
    </div>
    <div class="block">
      <span class="demonstration">Use value-format</span>
      <div class="demonstration">Value：{{ value2 }}</div>
      <el-date-picker
        v-model="value2"
        type="datetime"
        placeholder="Pick a Date"
        format="YYYY/MM/DD hh:mm:ss"
        value-format="YYYY-MM-DD h:m:s a"
      />
    </div>
    <div class="block">
      <span class="demonstration">Timestamp</span>
      <div class="demonstration">Value：{{ value3 }}</div>
      <el-date-picker
        v-model="value3"
        type="datetime"
        placeholder="Pick a Date"
        format="YYYY/MM/DD hh:mm:ss"
        value-format="x"
      />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value1 = ref('')
const value2 = ref('')
const value3 = ref('')
</script>

<style scoped>
.demo-datetime-picker {
  display: flex;
  width: 100%;
  padding: 0;
  flex-wrap: wrap;
}
.demo-datetime-picker .block {
  padding: 30px 0;
  text-align: center;
  border-right: solid 1px var(--el-border-color);
  flex: 1;
  min-width: 300px;
}
.demo-datetime-picker .block:last-child {
  border-right: none;
}
.demo-datetime-picker .demonstration {
  display: block;
  color: var(--el-text-color-secondary);
  font-size: 14px;
  margin-bottom: 20px;
}

@media (max-width: 768px) {
  .demo-datetime-picker .block {
    flex: 100%;
    border-right: none;
    border-bottom: solid 1px var(--el-border-color);
  }

  .demo-datetime-picker .block:last-child {
    border-bottom: none;
  }

  :deep(.el-date-editor.el-input) {
    width: 100%;
  }

  :deep(.el-date-editor.el-input__wrapper) {
    width: 100%;
    max-width: 300px;
  }
}
</style>
```

---

## For more information: https://chris.beams.io/posts/git-commit/

**URL:** llms-txt#for-more-information:-https://chris.beams.io/posts/git-commit/

---
