[toc]

# 概述

> OVERVIEW

`matSort`和`mat-sort-header`分别用于添加排序状态和展示表格数据

> The `matSort` and `mat-sort-header` are used, respectively, to add sorting state and display to tabular data.

## 在表头中添加排序

> Adding sort to table headers

在每个表头中增加`<mat-sort-header>`组件并提供一个定义的`id`，可以为一组表头增加排序行为与样式。这些表头必须包含在有`matSort`指令的父元素中，当用户在表头中触发排序时，将产生一个`matSortChange`事件。

> To add sorting behavior and styling to a set of table headers, add the `<mat-sort-header>` component to each header and provide an `id` that will identify it. These headers should be contained within a parent element with the `matSort` directive, which will emit an `matSortChange` event when the user triggers sorting on the header.

用户可以通过鼠标点击或者键盘操作，触发排序表头。当触发后，`matSort`将产生包含被触发表头ID的`matSortChange`事件和排序顺序（`asc`或者`desc`）。

> Users can trigger the sort header through a mouse click or keyboard action. When this happens, the `matSort` will emit an `matSortChange` event that contains the ID of the header triggered and the direction to sort (`asc` or `desc`).

### 改变排序顺序

> Changing the sort order

默认情况下，一个排序表头先是`asc`，然后是`desc`。`desc`后再触发排序表头，将去除排序。

> By default, a sort header starts its sorting at `asc` and then `desc`. Triggering the sort header after `desc` will remove sorting.

为了对所有的表头反转排序的顺序，可以在`matSort`指令中设置`matSortStart`为`desc`。如果只反转某一个表头的排序顺序，在表头中设置`start`输入。

> To reverse the sort order for all headers, set the `matSortStart` to `desc` on the `matSort` directive. To reverse the order only for a specific header, set the `start` input only on the header instead.

为了防止用户清除一个已排序列的排序状态，在`matSort`中设置`matSortDisableClear`为`true`，会对所有表头起效，或者在某一个表头中设置`disableClear`为`true`，对一个表头起效。

> To prevent the user from clearing the sort sort state from an already sorted column, set `matSortDisableClear` to `true` on the `matSort` to affect all headers, or set `disableClear` to `true` on a specific header.

### 禁用排序

> Disabling sorting

如果不想让用户改变任何列的排序，可以在`mat-sort`中使用`matSortDisabled`，或者在某一个`mat-sort-header`设置`disabled`。

> If you want to prevent the user from changing the sorting order of any column, you can use the `matSortDisabled` binding on the `mat-sort`, or the `disabled` on an single `mat-sort-header`.

### 使用mat-table进行排序

> Using sort with the mat-table

当在`mat-table`表头中使用时，并不是一定要设置一个`mat-sort-header`的id，因为默认会使用列的id。

> When used on an `mat-table` header, it is not required to set an `mat-sort-header` id on because by default it will use the id of the column.

## 无障碍

> Accessibility

排序按钮的`aria-label可在`MatSortHeaderIntl`中设置。

> The `aria-label` for the sort button can be set in `MatSortHeaderIntl`.

# API

## 排序（sort）的API参考

> API reference for Angular Material sort

```ts
import {MatSortModule} from '@angular/material/sort';
```

## 服务

> Services

### MatSortHeaderIntl

创建MatSortHeaderIntl实例并将其引入自定义的provider中，可以修改显示的标签和文本。

> To modify the labels and text displayed, create a new instance of MatSortHeaderIntl and include it in a custom provider.

**属性**

> Properties

属性名|描述|Description
-|-|-
changes: Subject&lt;void&gl;|此处标签发生变更时产生的流。用于标签在初始化后发生变更时通知组件。|Stream that emits whenever the labels here are changed. Use this to notify components if the labels have changed after initialization.
sortButtonLabel: (id: string) => { return `Change sorting for ${id}`; }|排序按钮的ARIA标签。|ARIA label for the sorting button.
sortDescriptionLabel: (id: string, direction: SortDirection) => { return `Sorted by ${id} ${direction == 'asc' ? 'ascending' : 'descending'}`; }|描述当前排序的标签（只对屏幕阅读者可见）|A label to describe the current sort (visible only to screenreaders).

## 指令

> Directives

### MatSortHeader

在一个元素中应用排序行为（点击改变排序）和样式，包括显示当前排序顺序的箭头。

> Applies sorting behavior (click to change sort) and styles to an element, including an arrow to display the current sort direction.

必须提供一个id，以及必须包含在一个父MatSort指令中。

> Must be provided with an id and contained within a parent MatSort directive.

如果在CdkTable的头单元格中使用排序，将默认自动使用包含的列定义id作为其id。

> If used on header cells in a CdkTable, it will automatically default its id from its containing column definition.

Selector: `[mat-sort-header]`

Exported as: `matSortHeader`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>arrowPosition: 'before' \| 'after'|设置排序时的箭头位置。|Sets the position of the arrow that displays when sorted.
@Input()<br>disableClear: boolean|复写MatSortable中包含的MatSort的失效清除值。|Overrides the disable clear value of the containing MatSort for this MatSortable.
@Input('mat-sort-header')<br>id: string|排序表头的ID。如果在CdkColumnDef的上下文中使用，将默认为列名。|ID of this sort header. If used within the context of a CdkColumnDef, this will default to the column's name.
@Input('start'<br>start: 'asc' \| 'desc'|复写MatSortable中包含的MatSort的排序起始值。|Overrides the sort start value of the containing MatSort for this MatSortable.

### MatSort

MatSortables的容器，用于管理排序状态和提供默认的排序参数。

> Container for MatSortables to manage the sort state and provide default sort parameters.

Selector: `[matSort]`

Exported as: `matSort`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('matSortActive')<br>active: string|最近排序的MatSortable的id。|The id of the most recently sorted MatSortable.
@Input('matSortDisableClear')<br>disableClear: boolean|是否失效用户在排序顺序循环后清除排序。可被MatSortable的取消清除输入复写。|Whether to disable the user from clearing the sort by finishing the sort direction cycle. May be overriden by the MatSortable's disable clear input.
@Input('matSortStart')<br>start: 'asc' \| 'desc'|当MatSortable初次排序时的排序顺序。可被MatSortable的排序开始复写。|The direction to set when an MatSortable is initially sorted. May be overriden by the MatSortable's sort start.
@Output('matSortChange')<br>sortChange: new EventEmitter&lt;Sort&gt;()|当用户改变或者激活排序或者排序顺序时传递的事件。|Event emitted when the user changes either the active sort or sort direction.
direction: SortDirection|当前激活的MatSortable的排序顺序。|The sort direction of the currently active MatSortable.
sortables: new Map&lt;string, MatSortable&gt;()|此指令管理的所有注册的sortable集合。|Collection of all registered sortables that this directive manages.

**方法**

> Methods

deregister
---
- 描述
  - 供MatSortables使用的注销函数。在包含的一组MatSortables中移除MatSortable。
  - Unregister function to be used by the contained MatSortables. Removes the MatSortable from the collection of contained MatSortables.
- 参数
  - sortable: MatSortable

getNextSortDirection
---
- 描述
  - 返回激活排序的下一个排序顺序，检查潜在的重载。
  - Returns the next sort direction of the active sortable, checking for potential overrides.
- 参数
  - sortable: MatSortable
- 返回
  - 排序顺序
  - SortDirection

register
---
- 描述
  - 供MatSortables使用的注册函数。在MatSortables集合中增加MatSortable。
  - Register function to be used by the contained MatSortables. Adds the MatSortable to the collection of MatSortables. 
- 参数
  - sortable: MatSortable

sort
---
- 描述
  - 设置激活排序id，并确定新的排序顺序。
  - Sets the active sort id and determines the new sort direction.
- 参数
  - sortable: MatSortable

## 附加接口

> Additional interfaces

### MatSortable

保存MatSortHeader使用的排序状态的指令接口。

> Interface for a directive that holds sorting state consumed by MatSortHeader.

**属性**

> Properties

属性名|描述|Description
-|-|-
disableClear: boolean|是否禁用清楚排序状态。|Whether to disable clearing the sorting state.
id: string|已排序列的id。|The id of the column being sorted.
start: 'asc' \| 'desc'|起始排序顺序。|Starting sort direction.

### Sort

当前排序状态。

> The current sort state.

**属性**

> Properties

属性名|描述|Description
-|-|-
active: string|已排序列的id。|The id of the column being sorted.
direction: SortDirection|排序顺序。|The sort direction.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*