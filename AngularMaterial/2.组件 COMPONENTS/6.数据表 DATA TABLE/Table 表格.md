[toc]

# 概述

> OVERVIEW

md-table提供了一个用于展示数据行信息的Material Design样式的数据表格。

> The md-table provides a Material Design styled data-table that can be used to display rows of data.

此表格控件基于CDK data-table，并且对于数据源输入和模板等提供了相似的接口，注意元素选择器的前缀由cdk-换为md-。

> This table builds on the foundation of the CDK data-table and uses a similar interface for its data source input and template, except that its element selectors will be prefixed with md- instead of cdk-.

注意列定义指令（cdkColumnDef和cdkHeaderCellDef）依旧是cdk-前缀。

> Note that the column definition directives (cdkColumnDef and cdkHeaderCellDef) are still prefixed with cdk-.

了解更多关于接口的信息，参考[CDK数据表格指南](https://material.angular.io/guide/cdk-table)。

> For more information on the interface and how it works, see the [guide covering the CDK data-table](https://material.angular.io/guide/cdk-table).

## 特性

> Features

<md-table>本身只负责渲染表格结构（行和列）。可通过在表格上方的单元格模板（例如排序表头）内或者表格旁边（例如分页器）增加行为，以创建附加的特性。影响渲染数据（例如排序和分页）的交互应通过表格内的数据源进行传播。

> The <md-table> itself only deals with the rendering of a table structure (rows and cells). Additional features can be built on top of the table by adding behavior inside cell templates (e.g., sort headers) or next to the table (e.g. a paginator). Interactions that affect the rendered data (such as sorting and pagination) should be propagated through the table's data source.

### 分页

> Pagination

<md-paginator>增加了一个与<md-table>一起使用的分页UI。分页器可以传递用于通过表格数据源触发更新的事件。

> The <md-paginator> adds a pagination UI that can be used in conjunction with the <md-table>. The paginator emits events that can be used to trigger an update via the table's data source.

### 排序

> Sorting

使用mdSort指令和<md-sort-header>，可以在表格列的头部增加一个排序UI。排序头可以传递用于通过表格数据源触发更新的事件。

> Use the mdSort directive and <md-sort-header> adds a sorting UI the table's column headers. The sort headers emit events that can be used to trigger an update via the table's data source.

### 过滤

> Filtering

Angular Material并不提供过滤表格数据的一个特定组件，所以可以通过任何自定义的过滤器UI来更新表格数据源。任何过滤模式都只需要通过表格数据源来触发更新。

> While Angular Material does not offer a specific component for filtering tabular data, the table's data source can be updated based on any custom filter UI. Any filtering pattern need only trigger an update via the table's data source.

## 简单表格

> Simple Table

在近期我们将提供一个有轻量易用接口、material样式、数组输入、和更多表格外部特性（排序、分页和选择等）的简化版本的数据表格。

> In the near future, we will provide a simplified version of the data-table with an easy-to-use interface, material styling, array input, and more out-of-the-box features (sorting, pagination, and selection).

# API

## 表格（table）的API参考

> API reference for Angular Material table

```typescript
import {MdTableModule} from '@angular/material';
```

## 指令

> Directives

### CdkTable

连接数据源并获取某类型数据的数据表格，可以渲染表格行头与行数据。当数据源提供新数据时，可以更新行数据。

> A data table that connects with a data source to retrieve data of type T and renders a header row and data rows. Updates the rows when new data is provided by the data source.

Selector: cdk-table

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>trackBy|用于检查数据变更的区别的跟踪函数。与ngFor的trackBy函数使用类似。通过识别行当中的数据与其关联的函数，来判断是否此行需要添加/删除/移动，来优化行操作。接受两个参数的函数：索引和数据项。|Tracking function that will be used to check the differences in data changes. Used similarly to ngFor trackBy function. Optimize row operations by identifying a row based on its data relative to the function to know if a row should be added/removed/moved. Accepts a function that takes two parameters, index and item.
viewChange<br>|包含屏幕上要显示哪些行的最新信息的流。可被数据源用于要提供哪些数据的启发。|Stream containing the latest information on what rows are being displayed on screen. Can be used by the data source to as a heuristic of what data should be provided.
@Input()<br>dataSource|提供了包含要渲染的最新数据数组的流。受表格视图窗口流（当前屏幕有哪些行）的影响。|Provides a stream containing the latest data array to render. Influenced by the table's stream of view window (what rows are currently on screen).

### CdkCellDef

CDK表格的单元格定义。将行单元格数据的模版捕获为单元格特定属性。

> Cell definition for a CDK table. Captures the template of a column's data row cell as well as cell-specific properties.

Selector: [cdkCellDef]

### CdkHeaderCellDef

CDK表格的头单元格定义。将列头单元格数据的模版捕获为单元格特定属性。

> Header cell definition for a CDK table. Captures the template of a column's header cell and as well as cell-specific properties.

Selector: [cdkHeaderCellDef]

### CdkColumnDef

CDK表格列定义。包含一系列表格列可用的单元格定义。

> Column definition for the CDK table. Defines a set of cells available for a table column.

Selector: [cdkColumnDef]

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('cdkColumnDef')<br>name|此列的唯一名称。|Unique name for this column.

### CdkHeaderCell

添加了正确的类和角色的头单元格模版容器。

> Header cell template container that adds the right classes and role.

Selector: cdk-header-cell

### CdkCell

添加了正确的类和角色的单元格模版容器。

> Cell template container that adds the right classes and role.

Selector: cdk-cell

### CdkHeaderRowDef

CDK表格头列定义。捕获了头列的模版和其他头属性，例如显示的列等。

> Header row definition for the CDK table. Captures the header row's template and other header properties such as the columns to display.

Selector: [cdkHeaderRowDef]

### CdkRowDef

CDK表格的数据行定义。捕获了头列模版和其他行数据，例如显示的列等。

> Data row definition for the CDK table. Captures the header row's template and other row properties such as the columns to display.

Selector: [cdkRowDef]

### CdkHeaderRow

头模版容器，包含单元格出口。添加了正确的类和角色。

> Header template container that contains the cell outlet. Adds the right class and role.

Selector: cdk-header-row

### CdkRow

数据行模版容器，包含单元格出口。添加了正确的类和角色。

> Data row template container that contains the cell outlet. Adds the right class and role.

Selector: cdk-row

### MdHeaderCell

添加了正确的类和角色的头单元格模版容器。

> Header cell template container that adds the right classes and role.

Selector: md-header-cell

### MdCell

添加了正确的类和角色的单元格模版容器。

> Cell template container that adds the right classes and role.

Selector: md-cell

### MdTable

包含Material design样式的CdkTable的封装。

> Wrapper for the CdkTable with Material design styles.

Selector: md-table

### MdHeaderRow

包含单元格出口的头模版容器。添加了正确的类和角色。

> Header template container that contains the cell outlet. Adds the right class and role.

Selector: md-header-row

### MdRow

数据行模版容器，包含单元格出口。添加了正确的类和角色。

> Data row template container that contains the cell outlet. Adds the right class and role.

Selector: md-row

## 附加类

> Additional classes

### DataSource

**方法**

> Methods

方法名|描述|Description
-|-|-
connect|将数据源连接到一个集合视图（例如数据表格）。|Connects a collection viewer (such as a data-table) to this data source.
disconnect|将一个集合视图（例如数据表格）与数据源断开连接。可用于视图销毁时的清理与注销。|Disconnects a collection viewer (such as a data-table) from this data source. Can be used to perform any clean-up or tear-down operations when a view is being destroyed.

### BaseRowDef

CdkHeaderRowDef和CdkRowDef的基础类，处理检查变更的列输入，以及通知到表格。

> Base class for the CdkHeaderRowDef and CdkRowDef that handles checking their columns inputs for changes and notifying the table.

**属性**

> Properties

属性名|描述|Description
-|-|-
columns|在此行中要展示的列。|The columns to be displayed on this row.
columnsChange|当列有变化时传出的事件流。|Event stream that emits when changes are made to the columns.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*