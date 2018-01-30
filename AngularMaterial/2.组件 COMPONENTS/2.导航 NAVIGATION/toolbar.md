[toc]

# 概述

> OVERVIEW

`<mat-toolbar>`是头部，标题，或者动作的一个容器。

> `<mat-toolbar>` is a container for headers, titles, or actions.

## 单行

> Single row

在大多数情况下，工具栏位于你应用的顶部，包括应用标题在内只有一行。

> In the most situations, a toolbar will be placed at the top of your application and will only have a single row that includes the title of your application.

```html
<mat-toolbar>
  <span>My Application</span>
</mat-toolbar>
```

## 多行

> Multiple rows

Material Design规范描述了工具栏也可以有多行。在`<mat-toolbar>`中使用`<mat-toolbar-row>`元素，可以在在Angular Material创建多行的工具栏。

> The Material Design specifications describe that toolbars can also have multiple rows. Creating toolbars with multiple rows in Angular Material can be done by placing `<mat-toolbar-row>` elements inside of a `<mat-toolbar>`.

```html
<mat-toolbar>  
  <mat-toolbar-row>
    <span>First Row</span>
  </mat-toolbar-row>

  <mat-toolbar-row>
    <span>Second Row</span>
  </mat-toolbar-row>
</mat-toolbar>
```

**注意**：当指定了多行时，是不支持在`<mat-toolbar-row>`外放置内容的。

> **Note**: Placing content outside of a `<mat-toolbar-row>` when multiple rows are specified is not supported.

## 定位工具栏内容

> Positioning toolbar content

工具栏不定位其中的内容。这使得用户可以自由管理位置以适配应用。

> The toolbar does not perform any positioning of its content. This gives the user full power to position the content as it suits their application.

通常的模式是左边是标题，右边是一些操作。使用`display: flex`可以简单实现。

> A common pattern is to position a title on the left with some actions on the right. This can be easily accomplished with `display: flex`:

```html
<mat-toolbar color="primary">
  <span>Application Title</span>

  <!-- This fills the remaining space of the current row -->
  <span class="example-fill-remaining-space"></span>

  <span>Right Aligned Text</span>
</mat-toolbar>
```

```css
.example-fill-remaining-space {
  /* This fills the remaining space, by using flexbox. 
     Every toolbar row uses a flexbox row layout. */
  flex: 1 1 auto;
}
```

## 主题化

> Theming

使用`color` 属性可以改变`<mat-toolbar>`的颜色。默认工具栏会基于当前主题（light或者dark）使用中性的背景颜色。可以改为`'primary'`, `'accent'`, 或者`'warn'`。

> The color of a `<mat-toolbar>` can be changed by using the `color` property. By default, toolbars use a neutral background color based on the current theme (light or dark). This can be changed to `'primary'`, `'accent'`, or `'warn'`.

## 无障碍

> Accessibility

默认工具栏会假定用于纯装饰的场景，因此没有设置角色、ARIA属性或者键盘快捷键。这等同于在页面中有一系列的`<div>`元素。

> By default, the toolbar assumes that it will be used in a purely decorative fashion and thus sets no roles, ARIA attributes, or keyboard shortcuts. This is equivalent to having a sequence of `<div>` elements on the page.

通常工具栏会作为头部使用，`role="header"`比较合适。

> Generally, the toolbar is used as a header where `role="header"` would be appropriate.

当工具栏的使用场景与role="toolbar"匹配时，用户需要添加角色，并通过`aria-label`或者`aria-labelledby`添加一个合适的标签。

> Only if the use-case of the toolbar match that of role="toolbar", the user should add the role and an appropriate label via `aria-label` or `aria-labelledby`.

# API

## 工具栏(toolbar)的API参考

> API reference for Angular Material toolbar

```ts
import {MatToolbarModule} from '@angular/material/toolbar';
```

## 指令

> Directives

### MatToolbarRow

Selector: `mat-toolbar-row`

Exported as: `matToolbarRow`

### MatToolbar

Selector: `mat-toolbar`

Exported as: `matToolbar`

**属性**

> Properties

属性名|描述|Description
-|-|-
color: ThemePalette|组件的主题颜色。|Theme color palette for the component.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*