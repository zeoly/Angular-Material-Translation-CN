[toc]

# 概述

> OVERVIEW

`mat-table`提供了一个用于展示数据行信息的Material Design样式的数据表格。

> The `mat-table` provides a Material Design styled data-table that can be used to display rows of data.

表格模板由两部分组成：列定义和行定义。每个列定义包含列头与单元格的模板。每个行定义包含用于此行并绑定到行元素的各个列。

> The table's template consists of two parts: column definitions and row definitions. Each column definition contains templates for that column's header and content cells. Each row definition captures the columns used for that row and any bindings applied to the row element.

`DataSource`属性通过传递`Observable`流，提供了表格需要渲染的数据。每次传递都包含所有需要展示的数据。监听此流的表格会渲染每一行。对于展示数据的任何操作（例如排序、分页、过滤）都需要包含在`DataSource`中，最后传递表示任何变更的新的数据集合。

> A `DataSource` provides data to the table by emitting an `Observable` stream of the items to be rendered. Each emit includes the entire set of items that should be displayed. The table, listening to this stream, will render a row per item. Any manipulation of the data being displayed (e.g. sorting, pagination, filtering) should be captured by the `DataSource`, ultimately emitting a new set of items to reflect any changes.

此表格控件基于CDK data-table，并且对于数据源输入和模板等提供了相似的接口，注意元素选择器的前缀由`cdk-`换为`mat-`。对于接口以及使用的详细信息，可以参考[CDK数据表指南](https://material.angular.io/guide/cdk-table)。

> This table builds on the foundation of the CDK data-table and uses a similar interface for its data source input and template, except that its element and attribute selectors will be prefixed with `mat-` instead of `cdk-`. For detailed information on the interface and how it works, see the [guide covering the CDK data-table](https://material.angular.io/guide/cdk-table).

## 开始

> Getting Started

### 1. 定义表格列

> 1. Define the table's columns

我们从编写表格的列定义开始。每列的定义应指定唯一的名称，并包含列头与列单元格等内容。

> Start by writing your table's column definitions. Each column definition should be given a unique name and contain the content for its header and row cells.

下面是一个名称为`'userName'`的列定义样例。列头单元格包含文本"Name"，每个列单元格渲染行数据的`name`属性。

> Here's a simple column definition with the name `'userName'`. The header cell contains the text "Name" and each row cell will render the `name` property of each row's data.

```html
<ng-container matColumnDef="userName">
  <mat-header-cell *matHeaderCellDef> Name </mat-header-cell>
  <mat-cell *matCellDef="let user"> {{user.name}} </mat-cell>
</ng-container>
```

### 2. 定义表格行

> 2. Define the table's rows

列定义完成后，需要提供表格渲染的行数据模板。每行需要指定包含的列信息。名称的顺序决定了单元格的渲染顺序。并不要求提供所有的定义列名称，只需要提供你想要渲染的列即可。

> After defining your columns, provide the header and data row templates that will be rendered out by the table. Each row needs to be given a list of the columns that it should contain. The order of the names will define the order of the cells rendered. It is not required to provide a list of all the defined column names, but only the ones that you want to have rendered.

```html
<mat-header-row *matHeaderRowDef="['userName', 'age']"></mat-header-row>
<mat-row *matRowDef="let myRowData; columns: ['userName', 'age']"></mat-row>
```

### 3. 提供数据

> 3. Provide data

列定义与行定义指定了数据如何渲染，剩下的就是提供数据本身。对于简单的客户端操作场景，`MatTableDataSource`提供了快捷简单的入口：创建`MatTableDataSource`实例并设置需要在`data`属性展示的数据。对于高级场景，应用需要实现一个或多个自定义的`DataSource`并指定具体的行为。

> The column and row definitions now capture how data will render - all that's left is to provide the data itself. For simple scenarios with client-side operations, `MatTableDataSource` offers a quick and easy starting point. Simply create an instance of `MatTableDataSource` and set the items to be displayed to the `data` property. For more advanced scenarios, applications will implement one or more custom `DataSource` to capture more specific behaviors.

```ts
this.myDataSource = new MatTableDataSource();
this.myDataSource.data = dataToRender;
```

```html
<mat-table [dataSource]=”myDataSource”>
  ...
</mat-table>
```

## 特性

### 分页

> Pagination

通过在`<mat-table>`后增加`<mat-paginator>`，并向`MatTableDataSource`提供`MatPaginator`，可以添加表格数据分页功能。数据源会自动监听用户的页面变更并向表格传递正确的分页数据。

> To paginate the table's data, add a `<mat-paginator>` after the `<mat-table>` and provide the `MatPaginator` to the `MatTableDataSource`. The data source will automatically listen for page changes made by the user and send the right paged data to the table.

更多使用与配置`<mat-paginator>`的信息，可以参考[mat-paginator文档](https://material.angular.io/components/paginator/overview)。

> For more information on using and configuring the `<mat-paginator>`, check out the [mat-paginator docs](https://material.angular.io/components/paginator/overview).

`MatPaginator`提供了一种分页表格数据的解决方案，但并不是唯一的选择。事实上表格可以与任何自定义的分页UI或策略兼容，因为`MatTable`和`DataSource`接口并没有与任何具体的实现捆绑。

> The `MatPaginator` is one provided solution to paginating your table's data, but it is not the only option. In fact, the table can work with any custom pagination UI or strategy since the `MatTable` and `DataSource` interface is not tied to any one specific implementation.

### 排序

> Sorting

通过在`<mat-table>`中添加`matSort`指令，并在每一个需要触发排序的列头单元格中添加`mat-sort-header`，可以为表格增加排序行为。在`MatTableDataSource`中提供`MatSort`会自动监听排序变更并改变表格渲染数据的顺序。

> To add sorting behavior to the table, add the `matSort` directive to the `<mat-table>` and add `mat-sort-header` to each column header cell that should trigger sorting. Provide the `MatSort` directive to the `MatTableDataSource` and it will automatically listen for sorting changes and change the order of data rendered by the table.

默认`MatTableDataSource`会按列名称与列展示的数据属性名称匹配并进行排序。例如下面的列定义名称为`position`，匹配行单元格中展示的属性。

> By default, the `MatTableDataSource` sorts with the assumption that the sorted column's name matches the data property name that the column displays. For example, the following column definition is named `position`, which matches the name of the property displayed in the row cell.

```html
<!-- Name Column -->
<ng-container matColumnDef="position">
  <mat-header-cell *matHeaderCellDef mat-sort-header> Name </mat-header-cell>
  <mat-cell *matCellDef="let element"> {{element.position}} </mat-cell>
</ng-container>
```

如果数据属性与列名称不匹配，或者需要一个更复杂的数据属性存取器，则需自定义一个`sortingDataAccessor`函数，用于复写`MatTableDataSource`中默认的的数据存取器。

> If the data properties do not match the column names, or if a more complex data property accessor is required, then a custom `sortingDataAccessor` function can be set to override the default data accessor on the `MatTableDataSource`.

关于使用和配置排序行为的更多信息，可以查看[matSort文档](https://material.angular.io/components/sort/overview)。

> For more information on using and configuring the sorting behavior, check out the [matSort docs](https://material.angular.io/components/sort/overview).

`MatSort`提供了一种排序表格数据的解决方案，但并不是唯一的选择。事实上表格可以与任何自定义的分页UI或策略兼容，因为`MatTable`和`DataSource`接口并没有与任何具体的实现捆绑。

> The `MatSort` is one provided solution to sorting your table's data, but it is not the only option. In fact, the table can work with any custom pagination UI or strategy since the `MatTable` and `DataSource` interface is not tied to any one specific implementation.

### 过滤

> Filtering

Angular Material没有提供一个过滤`MatTable`的具体组件，因此没有在表格数据中添加过滤UI的单个通用方法。

> Angular Material does not provide a specific component to be used for filtering the `MatTable` since there is no single common approach to adding a filter UI to table data.

通常方法是添加一个input用于用户输入过滤字符串并监听input的变更来改变提供给表格的数据源。

> A general strategy is to add an input where users can type in a filter string and listen to this input to change what data is offered from the data source to the table.

如果使用`MatTableDataSource`，可以简单地提供一个过滤字符串给`MatTableDataSource`。数据源将减少每一行数据并序列化表单，然后过滤掉不匹配过滤字符串的行。默认行数据减少的函数或连接所有对象的值并转换为小写。

> If you are using the `MatTableDataSource`, simply provide the filter string to the `MatTableDataSource`. The data source will reduce each row data to a serialized form and will filter out the row if it does not contain the filter string. By default, the row data reducing function will concatenate all the object values and convert them to lowercase.

例如，数据对象为`{id: 123, name: 'Mr. Smith', favoriteColor: 'blue'}`将减小为`123mr. smithblue`。如果过滤字符串为`blue`，则将匹配数据对象因为匹配上了减小的字符串，此行数据将会在表格中展示。

> For example, the data object `{id: 123, name: 'Mr. Smith', favoriteColor: 'blue'}` will be reduced to `123mr. smithblue`. If your filter string was `blue` then it would be considered a match because it is contained in the reduced string, and the row would be displayed in the table.

通过一个用于接收数据，过滤字符串并返回匹配结果的自定义`filterPredicate`函数，可复写默认的过滤行为。

> To override the default filtering behavior, a custom `filterPredicate` function can be set which takes a data object and filter string and returns true if the data object is considered a match.

### 选择

> Selection

目前还没有对于添加表格选择UI的正式支持，但Angular Material提供了正确的组件并相关功能。下面的步骤为表格行选择的解决方案之一。

> Right now there is no formal support for adding a selection UI to the table, but Angular Material does offer the right components and pieces to set this up. The following steps are one solution but it is not the only way to incorporate row selection in your table.

1. 添加选择模型

> 1. Add a selection model

从设置`@angular/cdk/collections`中的`SelectionModel`开始，用于维护选择状态。

> Get started by setting up a `SelectionModel` from `@angular/cdk/collections` that will maintain the selection state.

```ts
const initialSelection = [];
const allowMultiSelect = true;
this.selection = new SelectionModel<MyDataType>(allowMultiSelect, initialSelection);
```

2. 定义选择列

> 2. Define a selection column

添加一个列定义来展示行选择框，包括列头的主切换选择框。需要在`<mat-header-row>`和`<mat-row>`的展示列列表中添加列名称。

> Add a column definition for displaying the row checkboxes, including a master toggle checkbox for the header. The column name should be added to the list of displayed columns provided to the `<mat-header-row>` and `<mat-row>`.

```html
<ng-container matColumnDef="select">
  <mat-header-cell *matHeaderCellDef>
    <mat-checkbox (change)="$event ? masterToggle() : null"
                  [checked]="selection.hasValue() && isAllSelected()"
                  [indeterminate]="selection.hasValue() && !isAllSelected()">
    </mat-checkbox>
  </mat-header-cell>
  <mat-cell *matCellDef="let row">
    <mat-checkbox (click)="$event.stopPropagation()"
                  (change)="$event ? selection.toggle(row) : null"
                  [checked]="selection.isSelected(row)">
    </mat-checkbox>
  </mat-cell>
</ng-container>
```

3. 添加事件处理逻辑

> 3. Add event handling logic

在你组件逻辑中实现相关行为，来处理列头的主切换和检查是否所有行都已选择。

> Implement the behavior in your component's logic to handle the header's master toggle and checking if all rows are selected.

```ts
/** Whether the number of selected elements matches the total number of rows. */
isAllSelected() {
  const numSelected = this.selection.selected.length;
  const numRows = this.dataSource.data.length;
  return numSelected == numRows;
}

/** Selects all rows if they are not all selected; otherwise clear selection. */
masterToggle() {
  this.isAllSelected() ?
      this.selection.clear() :
      this.dataSource.data.forEach(row => this.selection.select(row));
}
```

4. 引入浮动样式

> 4. Include overflow styling

最后调整选择列的样式，使其浮动不隐藏。此样式允许扩展单元格的水波效果。

> Finally, adjust the styling for the select column so that its overflow is not hidden. This allows the ripple effect to extend beyond the cell.

```css
.mat-column-select {
  overflow: initial;
}
```

## 无障碍

> Accessibility

没有文本或标签的表格，需要通过`aria-label`或`aria-labelledby`指定一个有意义的标签。如果没有设置，`aria-readonly`默认为`true`。

> Tables without text or labels should be given a meaningful label via `aria-label` or `aria-labelledby`. The `aria-readonly` defaults to `true` if it's not set.

表格的默认角色为`grid`，可以通过`role`属性改为`treegrid`。

> Table's default role is `grid`, and it can be changed to `treegrid` through `role` attribute.

`mat-table`不参与焦点/键盘交互的管理。用户可在应用中增加期望的焦点/键盘交互。

> `mat-table` does not manage any focus/keyboard interaction on its own. Users can add desired focus/keyboard interactions in their application.

# API

## 表格（table）的API参考

> API reference for Angular Material table

```ts
import {MatTableModule} from '@angular/material/table';
```

## 指令

> Directives

### MatCellDef **extends CdkCellDef**

mat-table的单元格定义。包含列数据模版和单元格特定属性。

> Cell definition for the mat-table. Captures the template of a column's data row cell as well as cell-specific properties.

Selector: `[matCellDef]`

### MatHeaderCellDef **extends CdkHeaderCellDef**

mat-table的头单元格定义。包含列头单元格模版捕和单元格特定属性。

> Header cell definition for the mat-table. Captures the template of a column's header cell and as well as cell-specific properties.

Selector: `[matHeaderCellDef]`

### MatColumnDef **extends CdkColumnDef**

mat-table列定义。包含一系列表格列可用的单元格定义。

> Column definition for the mat-table. Defines a set of cells available for a table column.

Selector: `[matColumnDef]`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('matColumnDef')<br>name: string|此列的唯一名称。|Unique name for this column.

### MatHeaderCell **extends CdkHeaderCell**

添加了正确的类和角色的头单元格模版容器。

> Header cell template container that adds the right classes and role.

Selector: `mat-header-cell`

### MatCell **extends CdkCell**

添加了正确的类和角色的单元格模版容器。

> Cell template container that adds the right classes and role.

Selector: `mat-cell`

### MatTable **extends CdkTable**

包含Material design样式的CdkTable的封装。

> Wrapper for the CdkTable with Material design styles.

Selector: `mat-table`

Exported as: `matTable`

### MatHeaderRowDef **extends CdkHeaderRowDef**

mat-table列头行定义。包含列头行模版和其他头属性，例如显示的列等。

> Header row definition for the mat-table. Captures the header row's template and other header properties such as the columns to display.

Selector: `[matHeaderRowDef]`

### MatRowDef **extends CdkRowDef**

mat-table数据行定义。包含列头行模版和其他行数据，例如显示的列等，以及描述此行是否使用的断言。

> Data row definition for the mat-table. Captures the header row's template and other row properties such as the columns to display and a when predicate that describes when this row should be used.

Selector: `[matRowDef]`

### MatHeaderRow **extends CdkHeaderRow**

包含单元格的模版容器。添加了正确的类和角色。

> Header template container that contains the cell outlet. Adds the right class and role.

Selector: `mat-header-row`

Exported as: `matHeaderRow`

### MatRow **extends CdkRow**

包含单元格的数据行模版容器。添加了正确的类和角色。

> Data row template container that contains the cell outlet. Adds the right class and role.

Selector: `mat-row`

Exported as: `matRow`

## 附加类

> Additional classes

### MatTableDataSource

数据源接收客户端数据列表，并包含对过滤，排序（使用MatSort）和分页（使用MatPaginator）的原生支持。

> Data source that accepts a client-side data array and includes native support of filtering, sorting (using MatSort), and pagination (using MatPaginator).

通过复写sortingDataAccessor可实现排序自定义，函数定义了如何访问数据属性。同样通过复写filterTermAccessor可实现自定义过滤，函数定义了行数据如何转换为用于过滤匹配的字符串。

> Allows for sort customization by overriding sortingDataAccessor, which defines how data properties are accessed. Also allows for filter customization by overriding filterTermAccessor, which defines how row data is converted to a string for filter matching.

**属性**

> Properties

属性名|描述|Description
-|-|-
data: T[]|需要表格渲染的数据列表，每个对象代表一行。|Array of data that should be rendered by the table, where each object represents one row.
filter: string|用于过滤数据数组对象的过滤项。可提供自定义函数filterPredicate来复写数据对象如何匹配。|Filter term that should be used to filter out objects from the data array. To override how data objects match to this filter string, provide a custom function for filterPredicate.
filterPredicate: ((data: T, filter: string) => boolean)|检查数据对象是否匹配数据源的过滤字符串。默认每个数据对象都转换为其属性的字符串，如果此filter在转换的字符串中至少出现一次则返回true。默认过滤字符串会去除空格并且匹配大小写不敏感。可通过自定义的实现来复写。|Checks if a data object matches the data source's filter string. By default, each data object is converted to a string of its properties and returns true if the filter has at least one occurrence in that string. By default, the filter string has its whitespace trimmed and the match is case-insensitive. May be overriden for a custom implementation of filter matching.
filteredData: T[]|匹配上过过滤字符串的过滤后的数据集合，如果没有过滤则为所有数据。可用于获取表格展示的数据。例如'selectAll()'函数需要选择当前展示的过滤后的数据，而非所有数据。|The filtered set of data that has been matched by the filter string, or all the data if there is no filter. Useful for knowing the set of data the table represents. For example, a 'selectAll()' function would likely want to select the set of filtered data shown to the user rather than all the data.
paginator: MatPaginator \| null|MatPaginator组件的实例，用于表格控制展示哪一页的数据。MatPaginator传递的页面变更会触发表格渲染数据的更新。注意，数据源是使用paginator属性来计算应该展示哪一页。如果paginator与模板输入一样接收属性，例如[pageLength]=100或[pageIndex]=1，则需确定paginator视图须在分配到数据源之前初始化。|Instance of the MatPaginator component used by the table to control what page of the data is displayed. Page changes emitted by the MatPaginator will trigger an update to the table's rendered data. Note that the data source uses the paginator's properties to calculate which page of data should be displayed. If the paginator receives its properties as template inputs, e.g. [pageLength]=100 or [pageIndex]=1, then be sure that the paginator's view has been initialized before assigning it to this data source.
sort: MatSort \| null|MatSort指令的实例，用于表格控制排序。MatSort传递的排序变更将触发表格渲染数据的更新。|Instance of the MatSort directive used by the table to control its sorting. Sort changes emitted by the MatSort will trigger an update to the table's rendered data.
sortingDataAccessor: ((data: T, sortHeaderId: string) => string \| number)|用于排序的访问数据的数据访问函数。默认的函数是排序头ID（默认为列名）匹配数据的属性（例如列Xyz表示data['Xyz']）。可设置自定义的函数来使用不同的行为。|Data accessor function that is used for accessing data properties for sorting. This default function assumes that the sort header IDs (which defaults to the column name) matches the data's properties (e.g. column Xyz represents data['Xyz']). May be set to a custom function for different behavior.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*