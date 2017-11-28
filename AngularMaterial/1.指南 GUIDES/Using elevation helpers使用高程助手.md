# 使用高度助手

[TOC]

Angular Material的高度类和mixin可以让你在z轴上添加元素之间的分隔。所有material设计元素都有静止的高度。此外，一些元素可能会改变其对用户交互的响应。Material Design说明书会告诉你如何最佳使用高度。

> Angular Material's elevation classes and mixins allow you to add separation between elements along the z-axis. All material design elements have resting elevations. In addition, some elements may change their elevation in response to user interaction. The [Material Design spec](https://material.io/guidelines/material-design/elevation-shadows.html) explains how to best use elevation.

Angular Material提供两种方法来控制元素的高度:预定义的CSS类和mixin。

> Angular Material provides two ways to control the elevation of elements: predefined CSS classes and mixins.

## 预定义CSS类

> Predefined CSS classes

将提升添加到元素的最简单方法是简单地添加一个预定义的CSS类`mat-elevation-z#`，其中`#`是您想要的高度值，0-24。动态高度可通过转换高度类实现:

> The easiest way to add elevation to an element is to simply add one of the predefined CSS classes `mat-elevation-z#` where `#` is the elevation number you want, 0-24. Dynamic elevation can be achieved by switching elevation classes:


```html
<div [class.mat-elevation-z2]="!isActive" [class.mat-elevation-z8]="isActive"></div>
Elevation CSS classes
```

在CSS中，也可以通过`mat-elevation` mixin来增加高度，这需要一个数字0-24表示元素的高度。为了使用mixin，您必须导入`~@angular/material/theming`：


> Elevations can also be added in CSS via the `mat-elevation` mixin, which takes a number 0-24 indicating the elevation of the element. In order to use the mixin, you must import `~@angular/material/theming`:

```typescript
@import '~@angular/material/theming';

.my-class {
  @include mat-elevation(2);
}
```

为了方便起见，您可以使用`mat-elevation-transition` mixin在高度变化时添加一个CSS转换:

> For convenience, you can use the `mat-elevation-transition` mixin to add a transition when the elevation changes:

```typescript
@import '~@angular/material/theming';

.my-class {
  @include mat-elevation-transition;
  @include mat-elevation(2);

  &:active {
    @include mat-elevation(8);
  }
}
```

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*