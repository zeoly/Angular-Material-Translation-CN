[toc]

# 概述

> OVERVIEW

`mat-grid-list`是一个排列基于网格布局的单元格的二维列表视图。可以[点击此处](https://www.google.com/design/spec/components/grid-lists.html)查看Material Design说明。

> `mat-grid-list` is a two-dimensional list view that arranges cells into grid-based layout. See Material Design spec [here](https://www.google.com/design/spec/components/grid-lists.html).

## 设置列数

> Setting the number of columns

`mat-grid-list`必须指定`cols`属性，设置网格的列数。行数将基于列数与总项数自动确定。

> An `mat-grid-list` must specify a `cols` attribute which sets the number of columns in the grid. The number of rows will be automatically determined based on the number of columns and the number of items.

## 设置行高

> Setting the row height

通过`rowHeight`属性可以设置网格列表中的行高。有三种方式计算列表行高：

> The height of the rows in a grid list can be set via the `rowHeight` attribute. Row height for the list can be calculated in three ways:

1. **固定高度**：高度单位可为`px`, `em`, `rem`。如果没有指定单位，默认为`px`（例如`100px`, `5em`, `250`）。
2. **比例**：此比例为列宽：行高，必须以分号隔开，不能为小数（例如`4:3`）。
3. **自适应**：设置`rowHeight`为`fit`。此模式根据行数自动分配行高。注意grid-list或其容器的高度必须设置。

> 1. **Fixed height**: The height can be in `px`, `em`, or `rem`. If no units are specified, `px` units are assumed (e.g. `100px`, `5em`, `250`).
> 2. **Ratio**: This ratio is column-width:row-height, and must be passed in with a colon, not a decimal (e.g. `4:3`).
> 3. **Fit**: Setting `rowHeight` to `fit` This mode automatically divides the available height by the number of rows. Please note the height of the grid-list or its container must be set.

如果未设置`rowHeight`，默认为宽高比`1:1`。

> If `rowHeight` is not specified, it defaults to a `1:1` ratio of width:height.

## 设置间隔大小

> Setting the gutter size

可通过`gutterSize`属性设置间隔大小，单位可为`px`, `em`, `rem`。如果没有指定单位，默认为`px`。默认的间隔大小为`1px`。

> The gutter size can be set to any `px`, `em`, or `rem` value with the `gutterSize` property. If no units are specified, `px` units are assumed. By default the gutter size is `1px`.

## 增加跨行或跨列的网格

> Adding tiles that span multiple rows or columns

每个`mat-grid-tile`都可以通过`rowspan`和`colspan`属性单独设置跨行或跨列。如果没有设置，两个属性默认都为`1`。`colspan`不可超过`mat-grid-list`的`cols`属性值。`rowspan`没有太多限制，只会填充更多的行。

> It is possible to set the rowspan and colspan of each `mat-grid-tile` individually, using the `rowspan` and `colspan` properties. If not set, they both default to `1`. The `colspan` must not exceed the number of `cols` in the `mat-grid-list`. There is no such restriction on the `rowspan` however, more rows will simply be added for it the tile to fill.

## 网格头与网格脚

> Tile headers and footers

可通过使用`mat-grid-tile-header`和`mat-grid-tile-footer`元素来向`mat-grid-tile`中添加头部与脚部。

> A header and footer can be added to an `mat-grid-tile` using the `mat-grid-tile-header` and `mat-grid-tile-footer` elements respectively.

## 无障碍

> Accessibility

默认grid-list使用纯装饰风格，因此不设置角色，ARIA属性或快捷键。等同于页面上有个`<div>`元素的序列。任何在grid-list中的交互内容应当基于应用的特定工作流来设置合适的无障碍方式。

> By default, the grid-list assumes that it will be used in a purely decorative fashion and thus sets no roles, ARIA attributes, or keyboard shortcuts. This is equivalent to having a sequence of `<div>` elements on the page. Any interactive content within the grid-list should be given an appropriate accessibility treatment based on the specific workflow of your application.

如果grid-list用于呈现非交互内容，则grid-list元素需指定`role="list"`，每个网格也需要指定`role="listitem"`。

> If the grid-list is used to present a list of non-interactive content items, then the grid-list element should be given `role="list"` and each tile should be given `role="listitem"`.

# API

## 网格列表(grid-list)的API参考

> API reference for Angular Material grid-list

```ts
import {MatGridListModule} from '@angular/material/grid-list';
```

## 指令

> Directives

### MatGridList

Selector: `mat-grid-list`

Exported as: `matGridList`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>cols: any|网格列表的列数。|Amount of columns in the grid list.
@Input()<br>gutterSize: any|网格列表的间隔大小，单位像素。|Size of the grid list's gutter in pixels.
@Input()<br>rowHeight: string \| number|设置行高。|Set internal representation of row height from the user-provided value.

### MatGridTile

Selector: `mat-grid-tile`

Exported as: `matGridTile`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>colspan: number|网格占据的列数。|Amount of columns that the grid tile takes up.
@Input()<br>rowspan: number|网格占据的行数。|Amount of rows that the grid tile takes up.

### MatGridTileText

Selector: `mat-grid-tile-header` `mat-grid-tile-footer`

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*