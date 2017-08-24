[toc]

# 概述

> OVERVIEW

<md-paginator>提供了分页信息的导航，通常与表格一同使用。

> <md-paginator> provides navigation for paged information, typically used with a table.

## 基本用法

> Basic use

每个分页器实例都需要如下内容：
- 每页的数据数量（默认为50）
- 需要的分页的总数据量

> Each paginator instance requires:
> - The number of items per page (default set to 50)
> - The total number of items being paged

当前页数的索引默认为0，但可以通过设置pageIndex来指定。

> The current page index defaults to 0, but can be explicitly set via pageIndex.

当用户与分页器交互时，会触发PageEvent事件，可以用来更新任何关联的数据视图。

> When the user interacts with the paginator, a PageEvent will be fired that can be used to update any associated data view.

## 页数选项

> Page size options

分页器展示了一个页数下拉框，供用户选择。下拉框的选项可以通过pageSizeOptions设置。

> The paginator displays a dropdown of page sizes for the user to choose from. The options for this dropdown can be set via pageSizeOptions

当前的pageSize总是展示在下拉框中；不管其是否在pageSizeOptions中。

> The current pageSize will always appear in the dropdown, even if it is not included in pageSizeOptions.

## 国际化

> Internationalization

可以通过新建MdPaginatorIntl实例来自定义分页器的标签。可以改变如下内容：
1. 每页长度的标签。
2. 展示给用户的范围文本。
3. 导航按钮的提示信息。

> The labels for the paginator can be customized by providing your own instance of MdPaginatorIntl. This will allow you to change the following:
> 1. The label for the length of each page.
> 2. The range text displayed to the user.
> 3. The tooltip messages on the navigation buttons.

# API

## 分页器（paginator）的API参考

> API reference for Angular Material paginator

```typescript
import {MdPaginatorModule} from '@angular/material';
```

## 指令

> Directives

### MdPaginator

在分页信息间提供导航的组件。展示了当前分页大小，可选择的分页大小，要展示哪些数据，和上下翻页的导航按钮。

> Component to provide navigation between paged information. Displays the size of the current page, user-selectable options to change that size, what items are being shown, and navigational button to go to the previous or next page.

Selector: md-paginator

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>pageIndex|展示的数据列表的页码索引，从0开始。默认为0。|The zero-based page index of the displayed list of items. Defaulted to 0.
@Input()<br>length|要分页的数据总量。默认为0。|The length of the total number of items that are being paginated. Defaulted to 0.
@Input()<br>pageSize|单页展示的数据数量。默认为50。|Number of items to display on a page. By default set to 50.
@Input()<br>pageSizeOptions|展示给用户的提供单页数量选项的集合。|The set of provided page size options to display to the user.
@Output()<br>page|当单页数量或者页码索引变更时触发的事件。|Event emitted when the paginator changes the page size or page index.

**方法**

> Methods

方法名|描述|Description
-|-|-
nextPage|前进到下一页（如果存在）。|Advances to the next page if it exists.
previousPage|退回到上一页（如果存在）。|Move back to the previous page if it exists.
hasPreviousPage|是否有上一页。|Whether there is a previous page.
hasNextPage|是否有下一页。|Whether there is a next page.

## 附件类

> Additional classes

### PageEvent

当用户选择不同的单页数量或者导航到另一页时的改变事件对象。

> Change event object that is emitted when the user selects a different page size or navigates to another page.

**属性**

> Properties

属性名|描述|Description
-|-|-
pageIndex|当前页码索引。|The current page index.
pageSize|当期单页数量。|The current page size
length|当前分页的数据总量。|The current total number of items being paged

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*