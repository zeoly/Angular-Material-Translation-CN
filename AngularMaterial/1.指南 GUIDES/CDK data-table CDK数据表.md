<cdk-table> 非常灵活，拥有完整模板API的可定制化data-table，动态列，以及可访问的DOM结构。作为核心组件，任何人都可以利用它为自己打造专用的数据表体验。

> The <cdk-table> is an unopinionated, customizable data-table with a fully-templated API, dynamic columns, and an accessible DOM structure. This component acts as the core upon which anyone can build their own tailored data-table experience.

表格还为更多特性（如排序和分页等）的实现提供了基础。因为它在这些问题中不带有任何意见，开发者可以完全控制表格的交互模式。

> The table provides a foundation upon which other features, such as sorting and pagination, can be built. Because it enforces no opinions on these matters, developers have full control over the interaction patterns associated with the table.

对于Material Design风格化的表格，可以查看建立在CDK data-table上面的<md-table>[文档](https://material.angular.io/components/table)。

> For a Material Design styled table, see the documentation for <md-table> which builds on top of the CDK data-table.

## 使用 CDK data-table

> Using the CDK data-table

**编写你的表格模板**

> Writing your table template

第一步：编写data-table模板定义列。通过<ng-container>标签与cdkColumnDef指令对列进行定义，同时给列赋予一个名字。每一个列定义都进一步定义了header-cell模板（cdkHeaderCellDef）与data-cell模板（cdkCellDef）。

> The first step to writing the data-table template is to define the columns. A column definition is specified via an <ng-container> with the cdkColumnDef directive, giving column a name. Each column definition then further defines both a header-cell template (cdkHeaderCellDef) and a data-cell template (cdkCellDef).

```html
<ng-container cdkColumnDef="username">
    <cdk-header-cell *cdkHeaderCellDef> User name
    </cdk-header-cell>
    <cdk-cell *cdkCellDef="let row"> {{row.a}} 
    </cdk-cell>
</ng-container>
```

定义的列集合代表了可渲染的列。特定的列在指定的行中进行呈现与排列（见下面）。

> The set of columns defined represent the columns that are available to be rendered. The specific columns rendered in a given row, and their order, are specified on the row (see below).

注意到cdkCellDef导出的行数据，如可以在单元格模板中引用行数据。这个指令也可以用ngFor导出相同属性（如index, even, odd, first, last）。

> Note that cdkCellDef exports the row context such that the row data can be referenced in the cell template. The directive also exports the same properties as ngFor (index, even, odd, first, last).

下一步就是定义表格的header-row（cdkHeaderRowDef）和data-row（cdkRowDef）

> The next step is to define the table's header-row (cdkHeaderRowDef) and data-row (cdkRowDef).

```html
<cdk-header-row *cdkHeaderRowDef="['username', 'age', 'title']">
</cdk-header-row>
<cdk-row *cdkRowDef="let row; columns: ['username', 'age', 'title']">
</cdk-row>
```

这些行模板通过cdkColumnDef指定的名称进行特定列的渲染。

> These row templates accept the specific columns to be rendered via the name given to the cdkColumnDef.

cdkRowDef同样可以导出行内容，用于绑定在行元素上面的属性和事件。任何位于header行或者data行模板将会被忽略，而被上述单元模板的渲染内容所代替。

> The cdkRowDef also exports row context, which can be used for event and property bindings on the row element. Any content placed inside of the header row or data row template will be ignored, as the rendered content of the row comes from the cell templates described above.

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

给定行上的列决定了单元以什么顺序渲染。因此，这些列可以设置成运行时通过绑定支持动态变化的列。

> The columns given on the row determine which cells are rendered and in which order. Thus, the columns can be set via binding to support dynamically changing the columns shown at run-time.

在模板中定义的列并不需要全部展示，或者使用相同顺序进行排序。例如，如果想以姓名与年龄为主要顺序来展示表格，则在行与表头定义中写：

> It is not required to display all the columns that are defined within the template, nor use the same ordering. For example, to display the table with only age and username and in that order, then the row and header definitions would be written as:

```html
<cdk-row *cdkRowDef="let row; columns: myDisplayedColumns"></cdk-row>
```

事件与属性绑定可以直接添加到行元素中。

> Event and property bindings can be added directly to the row element.

**例如：事件与类绑定的表格**

> Example: table with event and class binding

```html
<cdk-header-row *cdkHeaderRowDef="['age', 'username']" (click)=”handleHeaderRowClick(row)”>
</cdk-header-row>

<cdk-row *cdkRowDef="let row; columns: ['age', 'username']" [class.can-vote]=”row.age>= 18” (click)=”handleRowClick(row)”>
</cdk-row>
```

**关联表格的数据资源**

> Connecting the table to a data source

数据是通过数据源提供到表格当中的。当表格接收到数据源的时候，这个数据源被称为一个可以返回可观察的数据数组的数据源连接方法。只要数据源发出数据到流中，表格就会被更新。

> Data is provided to the table through a DataSource. When the table receives a data source, it calls the DataSource's connect function which returns an observable that emits an array of data. Whenever the data source emits data to this stream, the table will update.

因为数据源提供流，而流负责的表格更新的触发工作。这可以基于任何接口：Websocket连接，用户交互，模型更新，时间间隔等。我们最常见的是，排序与分页等用户交互操作将会触发更新。

> Because the data source provides this stream, it bears the responsibility of triggering table updates. This can be based on anything: websocket connections, user interaction, model updates, time-based intervals, etc. Most commonly, updates will be triggered by user interactions like sorting and pagination.

## trackBy

为了提升性能，trackBy方法可以提供类似于angular ngFor trackBy的效果和功能给表格.这就告诉了表格应该如何通过行的唯一识别码追踪每一个更新中的数据变化

> To improve performance, a trackBy function can be provided to the table similar to Angular’s ngFor trackBy. This informs the table how to uniquely identify rows to track how the data changes with each update.

```html
<cdk-table [dataSource]="dataSource" [trackBy]="myTrackById">
```