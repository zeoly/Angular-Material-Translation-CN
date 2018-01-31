[toc]

# 概述

> OVERVIEW

`<mat-divider>`是一个有多个方向选项线条分割，并接受Material样式的组件。

> `<mat-divider>` is a component that allows for Material styling of a line separator with various orientation options.

## 简单分割

> Simple divider

`<mat-divider>`元素可用于创建一个有Material主体样式的水平或者垂直的线条。

> A `<mat-divider>` element can be used on its own to create a horizontal or vertical line styled with a Material theme

```html
<mat-divider></mat-divider>
```

## 内嵌分割

> Inset divider

添加`inset`属性可设置此分割是否为一个内嵌分割。

> Add the `inset` attribute in order to set whether or not the divider is an inset divider.

```html
<mat-divider [inset]="true"></mat-divider>
```

## 垂直分割

> Vertical divider

添加`vertical`属性可设置分割是否为垂直方向。

> Add the `vertical` attribute in order to set whether or not the divider is vertically-oriented.

```html
<mat-divider [vertical]="true"></mat-divider>
```

## 包含内嵌分割的列表

> Lists with inset dividers

在列表中添加分割是分开不同区域的一种方式。也可以添加内嵌分割来提供列表中不同元素的显示，这样就不需要分组例如头像图标或者图标等内容。注意不要在列表的最后一个元素中添加内嵌分割，这会和区域的分割重叠。

> Dividers can be added to lists as a means of separating content into distinct sections. Inset dividers can also be added to provide the appearance of distinct elements in a list without cluttering content like avatar images or icons. Make sure to avoid adding an inset divider to the last element in a list, because it will overlap with the section divider.

```html
<mat-list>
   <h3 mat-subheader>Folders</h3>
   <mat-list-item *ngFor="let folder of folders; last as last">
      <mat-icon mat-list-icon>folder</mat-icon>
      <h4 mat-line>{{folder.name}}</h4>
      <p mat-line class="demo-2"> {{folder.updated}} </p>
      <mat-divider [inset]="true" *ngIf="!last"></mat-divider>
   </mat-list-item>
   <mat-divider></mat-divider>
   <h3 md-subheader>Notes</h3>
   <mat-list-item *ngFor="let note of notes">
      <mat-icon md-list-icon>note</mat-icon>
      <h4 md-line>{{note.name}}</h4>
      <p md-line class="demo-2"> {{note.updated}} </p>
   </mat-list-item>
</mat-list>
```

# API

## 分割线(divider)的API参考

> API reference for Angular Material divider

```ts
import {MatDividerModule} from '@angular/material/divider';
```

## 指令

> Directives

### MatDivider

Selector: `mat-divider`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>inset: boolean|分割线是否为内嵌分割。|Whether the divider is an inset divider.
@Input()<br>vertical: boolean|分割线是否为垂直。|Whether the divider is vertically aligned.


*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*