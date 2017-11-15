# CDK数据表

`<cdk-table>` 是非常灵活、可定制化data-table，拥有完全模板化的API、动态列、以及可访问的DOM结构。任何人都可以利用它为自己打造专用的数据表。

> The `<cdk-table>` is an unopinionated, customizable data-table with a fully-templated API, dynamic columns, and an accessible DOM structure. This component acts as the core upon which anyone can build their own tailored data-table experience.

表格还为更多特性（如排序和分页等）的实现提供了基础。因为它对于这些特性非常开放，开发者可以完全控制表格的交互模式。

> The table provides a foundation upon which other features, such as sorting and pagination, can be built. Because it enforces no opinions on these matters, developers have full control over the interaction patterns associated with the table.

对于Material Design风格化的表格，可以查看建立在CDK data-table上面的`<md-table>`[文档](https://material.angular.io/components/table)。

> For a Material Design styled table, see the [documentation for](https://material.angular.io/components/table) `<md-table>` which builds on top of the CDK data-table.

## 使用 CDK data-table

> Using the CDK data-table

**编写你的表格模板**

> Writing your table template

第一步：编写data-table模板首先需要先定义列。通过`<ng-container>`标签与`cdkColumnDef`指令来指定列名，以对列进行定义。然后需要再对每一列定义header-cell模板（`cdkHeaderCellDef`）与data-cell模板（`cdkCellDef`）。

> The first step to writing the data-table template is to define the columns. A column definition is specified via an `<ng-container>` with the`cdkColumnDef` directive, giving column a name. Each column definition then further defines both a header-cell template (`cdkHeaderCellDef`) and a data-cell template (`cdkCellDef`).

```html
<ng-container cdkColumnDef="username">
  <cdk-header-cell *cdkHeaderCellDef> User name </cdk-header-cell>
  <cdk-cell *cdkCellDef="let row"> {{row.a}} </cdk-cell>
</ng-container>
```

定义的列集合代表了可渲染的列。这些列可以在指定的行中渲染，并且在行中指定其顺序（见下面）。

> The set of columns defined represent the columns that are available to be rendered. The specific columns rendered in a given row, and their order, are specified on the row (see below).

注意到`cdkCellDef`导出的行上下文，如行数据可以在单元格模板中被引用。这个指令可以导出与`ngFor`指令相同的属性（如index, even, odd, first, last）。

> Note that `cdkCellDef` exports the row context such that the row data can be referenced in the cell template. The directive also exports the same properties as `ngFor` (index, even, odd, first, last).

下一步就是定义表格的行头header-row（`cdkHeaderRowDef`）和行数据data-row（`cdkRowDef`）

> The next step is to define the table's header-row (`cdkHeaderRowDef`) and data-row (`cdkRowDef`).

```html
<cdk-header-row *cdkHeaderRowDef="['username', 'age', 'title']"></cdk-header-row>
<cdk-row *cdkRowDef="let row; columns: ['username', 'age', 'title']"></cdk-row>
```

这些行模板通过`cdkColumnDef`指定的名称进行特定列的渲染。

> These row templates accept the specific columns to be rendered via the name given to the `cdkColumnDef`.

`cdkRowDef`同样可以导出用于绑定在行元素上面的属性和事件的行上下文。任何位于header行或者data行*内部*的模板将会被忽略，并作为上述单元模板中的行内渲染内容。

> The `cdkRowDef` also exports row context, which can be used for event and property bindings on the row element. Any content placed inside of the header row or data row template will be ignored, as the rendered content of the row comes from the cell templates described above.

例如：一个三列的表格

> Example: table with three columns

```html
<cdk-table [dataSource]="dataSource">
  <!-- User name Definition -->
  <ng-container cdkColumnDef="username">
    <cdk-header-cell *cdkHeaderCellDef> User name </cdk-header-cell>
    <cdk-cell *cdkCellDef="let row"> {{row.username}} </cdk-cell>
  </ng-container>

  <!-- Age Definition -->
  <ng-container cdkColumnDef="age">
    <cdk-header-cell *cdkHeaderCellDef> Age </cdk-header-cell>
    <cdk-cell *cdkCellDef="let row"> {{row.age}} </cdk-cell>
  </ng-container>

  <!-- Title Definition -->
  <ng-container cdkColumnDef="title">
    <cdk-header-cell *cdkHeaderCellDef> Title </cdk-header-cell>
    <cdk-cell *cdkCellDef="let row"> {{row.title}} </cdk-cell>
  </ng-container>

  <!-- Header and Row Declarations -->
  <cdk-header-row *cdkHeaderRowDef="['username', 'age', 'title']"></cdk-header-row>
  <cdk-row *cdkRowDef="let row; columns: ['username', 'age', 'title']"></cdk-row>
</cdk-table>
```

行中指定的列决定了哪个单元格要渲染，以什么顺序渲染。因此，这些列可以在运行时通过绑定支持动态变化的列来设置。

> The columns given on the row determine which cells are rendered and in which order. Thus, the columns can be set via binding to support dynamically changing the columns shown at run-time.

```html
<cdk-row *cdkRowDef="let row; columns: myDisplayedColumns"></cdk-row>

```

在模板中定义的列并不需要全部展示，或者使用相同顺序进行排序。例如，如果想以姓名与年龄为主要顺序来展示表格，则在行与表头定义中写：

> It is not required to display all the columns that are defined within the template, nor use the same ordering. For example, to display the table with only `age` and `username` and in that order, then the row and header definitions would be written as:

```html
<cdk-row *cdkRowDef="let row; columns: ['age', 'username']"></cdk-row>

```

事件与属性绑定可以直接添加到行元素中。

> Event and property bindings can be added directly to the row element.

**例如：事件与类绑定的表格**

> Example: table with event and class binding

```html
<cdk-header-row *cdkHeaderRowDef="['age', 'username']"
                (click)="handleHeaderRowClick(row)">
</cdk-header-row>

<cdk-row *cdkRowDef="let row; columns: ['age', 'username']"
          [class.can-vote]="row.age >= 18"
          (click)="handleRowClick(row)">
</cdk-row>
```

## 样式化的列

> Styling columns

每个表头与行单元格都可以提供包含其对应列的CSS类。例如在`name`列中显示的单元格设置了`cdk-column-name`类。这样将使列与对应的表头和行的样式一致。

> Each header and row cell will be provided a CSS class that includes its column. For example, cells that are displayed in the column `name` will be given the class `cdk-column-name`. This allows columns to be given styles that will match across the header and rows.

因为列名可以指定为任意字符串，所以可能不能直接应用到CSS类中（例如`*nameColumn!`）。在这种情况下，特殊的字符可以替换为`-`字符。例如在列名为`*nameColumn!`中的单元格可以指定为类`cdk-column--nameColumn-`。

> Since columns can be given any string for its name, its possible that it cannot be directly applied to the CSS class (e.g. `*nameColumn!`). In these cases, the special characters will be replaced by the `-` character. For example, cells container in a column named `*nameColumn!` will be given the class `cdk-column--nameColumn-`.

**关联表格的数据资源**

> Connecting the table to a data source

数据是通过数据源`DataSource`提供到表格当中的。当表格接收到数据源的时候，将会调用数据源的`connect()`方法，并返回一个传递数据数组的可观察对象。当数据源传递数据到流中，表格就会被渲染更新。

> Data is provided to the table through a `DataSource`. When the table receives a data source, it calls the DataSource's `connect()` method which returns an observable that emits an array of data. Whenever the data source emits data to this stream, the table will render an update.

因为*数据源*提供了这个流，而流负责表格触发更新的工作。这可以基于任何事物：websocket连接，用户交互，模型更新，时间间隔等。我们最常见的是，排序与分页等用户交互操作将会触发更新。


> Because the *data source* provides this stream, it bears the responsibility of triggering table updates. This can be based on anything: websocket connections, user interaction, model updates, time-based intervals, etc. Most commonly, updates will be triggered by user interactions like sorting and pagination.

## `trackBy`

为了提升性能，`trackBy`方法可以提供类似于angular [`ngFor trackBy`](https://angular.io/api/common/NgForOf#change-propagation)的效果和功能给表格.这就告诉了表格应该如何确认唯一的行来追踪每一个更新中的数据如何变化

> To improve performance, a `trackBy` function can be provided to the table similar to Angular’s [`ngFor trackBy`](https://angular.io/api/common/NgForOf#change-propagation). This informs the table how to uniquely identify rows to track how the data changes with each update.

```html
<cdk-table [dataSource]="dataSource" [trackBy]="myTrackById">
```
*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*