[toc]

# 概述

> OVERVIEW

mdSort和md-sort-header主要用于添加排序状态和展示表格数据。

> The mdSort and md-sort-header are used, respectively, to add sorting state and display to tabular data.

## 在表头中添加排序

> Adding sort to table headers

在每个表头中增加<md-sort-header>组件并提供一个定义的id，可以为一系列的表头增加排序行为与样式。这些表头必须包含在有mdSort指令的父元素中，当用户在表头中触发排序时，将产生一个mdSortChange事件。

> To add sorting behavior and styling to a set of table headers, add the <md-sort-header> component to each header and provide an id that will identify it. These headers should be contained within a parent element with the mdSort directive, which will emit an mdSortChange event when the user triggers sorting on the header.

用户可以通过鼠标点击或者键盘操作，触发排序表头。当触发后，mdSort将产生包含被触发表头id的mdSortChange事件，并进行排序（正序或者倒序）。

> Users can trigger the sort header through a mouse click or keyboard action. When this happens, the mdSort will emit an mdSortChange event that contains the ID of the header triggered and the direction to sort (asc or desc).

### 改变排序顺序

> Changing the sort order

默认情况下，一个排序表头先是正序，然后是倒序。倒序后再触发排序表头，将去除排序。

> By default, a sort header starts its sorting at asc and then desc. Triggering the sort header after desc will remove sorting.

为了对所有的表头反转排序的顺序，可以在mdSort指令中设置mdSortStart为desc。如果只反转某一个表头的排序顺序，在表头中设置start输入。

> To reverse the sort order for all headers, set the mdSortStart to desc on the mdSort directive. To reverse the order only for a specific header, set the start input only on the header instead.

为了防止用户清除已排序列的排序状态，在mdSort中设置mdSortDisableClear为true，会对所有表头起效，或者在某一个表头中设置disableClear为true，对一个表头起效。

> To prevent the user from clearing the sort sort state from an already sorted column, set mdSortDisableClear to true on the mdSort to affect all headers, or set disableClear to true on a specific header.

### 用the md-table进行排序

> Using sort with the md-table

当使用md-table表头时，不是必须要设置一个md-sort-header的id，因为默认会使用列的id。

> When used on an md-table header, it is not required to set an md-sort-header id on because by default it will use the id of the column.

# API

## 排序（sort）的API参考

> API reference for Angular Material sort

```typescript
import {MdSortModule} from '@angular/material';
```

## 服务

> Services

### MdSortHeaderIntl

修改展示的标签与文本，创建一个MdSortHeaderIntl实例并将其引入一个自定义个provider中。

> To modify the labels and text displayed, create a new instance of MdSortHeaderIntl and include it in a custom provider.

**属性**

> Properties

属性名|描述|Description
-|-|-
sortButtonLabel||
sortDescriptionLabel|描述当前排序的标签（只对屏幕阅读者可见）|A label to describe the current sort (visible only to screenreaders).

## 指令

> Directives

### MdSortHeader

在一个元素中应用排序行为（点击改变排序）和样式，包括显示当前排序顺序的箭头。

> Applies sorting behavior (click to change sort) and styles to an element, including an arrow to display the current sort direction.

必须提供一个id，以及必须包含在一个父MdSort指令中。

> Must be provided with an id and contained within a parent MdSort directive.

如果在CdkTable的头单元格中使用排序，将默认自动使用包含的列定义id作为其id。

> If used on header cells in a CdkTable, it will automatically default its id from its containing column definition.

Selector: [md-sort-header]

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('md-sort-header')<br>id|排序表头的id。如果在CdkColumnDef的上下文中使用，将默认为列名。|ID of this sort header. If used within the context of a CdkColumnDef, this will default to the column's name.
@Input()<br>arrowPosition|设置排序时的箭头位置。|Sets the position of the arrow that displays when sorted.
@Input('start')<br>start|重载MdSortable中包含的MdSort的排序起始值。|Overrides the sort start value of the containing MdSort for this MdSortable.
@Input()<br>disableClear|重载MdSortable中包含的MdSort的失效清除值。|Overrides the disable clear value of the containing MdSort for this MdSortable.

### MdSort

MdSortables的容器，用于管理排序状态和提供默认的排序参数。

> Container for MdSortables to manage the sort state and provide default sort parameters.

Selector: [mdSort]

**属性**

> Properties

属性名|描述|Description
-|-|-
sortables|此指令管理的所有注册的sortable集合。|Collection of all registered sortables that this directive manages.
@Input('mdSortActive')<br>active|最近排序的MdSortable的id。|The id of the most recently sorted MdSortable.
@Input('mdSortStart')<br>start|当MdSortable初次排序时的排序顺序。可被MdSortable的排序start重载。|The direction to set when an MdSortable is initially sorted. May be overriden by the MdSortable's sort start.
@Input('mdSortDirection')<br>direction|当前激活的MdSortable的排序顺序。|The sort direction of the currently active MdSortable.
@Input('mdSortDisableClear')<br>disableClear|是否取消用户在排序顺序循环后清除排序。可被MdSortable的取消清除input重载。|Whether to disable the user from clearing the sort by finishing the sort direction cycle. May be overriden by the MdSortable's disable clear input.
@Output()<br>mdSortChange|当用户改变或者激活排序或者排序顺序时传递的事件。|Event emitted when the user changes either the active sort or sort direction.

**方法**

> Methods

register
---
- 描述
  * 注册供MdSortables使用的函数。在MdSortables中增加MdSortable。
  > Register function to be used by the contained MdSortables. Adds the MdSortable to the collection of MdSortables. 
- 参数
  * sortable (MdSortable)

deregister
---
- 描述
  * 注销供MdSortables使用的函数。在MdSortables中移除MdSortable。
  > Unregister function to be used by the contained MdSortables. Removes the MdSortable from the collection of contained MdSortables.
- 参数
  * sortable (MdSortable)

sort
---
- 描述
  * 设置激活排序id，并确定新的排序顺序。
  > Sets the active sort id and determines the new sort direction.
- 参数
  * sortable (MdSortable)

getNextSortDirection
---
- 描述
  * 返回激活排序的下一个排序顺序，检查潜在的重载。
  > Returns the next sort direction of the active sortable, checking for potential overrides.
- 参数
  * sortable (MdSortable)
- 返回
  * SortDirection

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*