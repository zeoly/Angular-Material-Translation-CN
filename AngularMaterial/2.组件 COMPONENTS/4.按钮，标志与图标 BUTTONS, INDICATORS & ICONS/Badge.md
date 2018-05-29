[toc]

# 概述

> OVERVIEW

标记是UI元素的一个小的状态描述。一个标记由一个小圆圈组成，通常包含一个数字或者其他短字母，并紧挨着其他的对象出现。

> Badges are small status descriptors for UI elements. A badge consists of a small circle, typically containing a number or other short set of characters, that appears in proximity to another object.

## 标记位置

> Badge position

默认标记的位置是`above after`。可通过定义`matBadgePosition`属性为`above|below`和`before|after`来改变方向。

> By default, the badge will be placed `above after`. The direction can be changed by defining the attribute `matBadgePosition` follow by `above|below` and `before|after`.

```html
<mat-icon matBadge="22" matBadgePosition="above after">home</mat-icon>
```

也可以使用`matBadgeOverlap`来定义标记与其内部内容的交叠。通常你希望交叠的是图标而不是文字短语等。默认内容会与标记交叠。

> The overlap of the badge in relation to its inner contents can also be defined using the `matBadgeOverlap` tag. Typically, you want the badge to overlap an icon and not a text phrase. By default it will overlap.

```html
<h1 matBadge="11" matBadgeOverlap="false">
  Email
</h1>
```

## 标记尺寸

> Badge sizing

标记有三个尺寸：`small`, `medium`和`large`。标记默认的尺寸为`medium`。你可以通过在属主元素中设置`matBadgeSize`来改变尺寸。

> The badge has 3 sizes: `small`, `medium` and `large`. By default, the badge is set to `medium`. You can change the size by adding `matBadgeSize` to the host element.

```html
<h1 matBadge="11" matBadgeSize="large">
  Email
</h1>
```

## 标记可见性

> Badge visibility

可通过定义`matBadgeHidden`来切换标记的可见性。

> The badge visibility can be toggled programmatically by defining `matBadgeHidden`.

```html
<h1 matBadge="11" [matBadgeHidden]="!visible">
  Email
</h1>
```

## 主题

> Theming

标记可根据当前的主题使用`matBadgeColor`属性，来设置背景色为`primary`, `accent`或`warn`。

> Badges can be colored in terms of the current theme using the `matBadgeColor` property to set the background color to `primary`, `accent`, or `warn`.

```html
<mat-icon matBadge="22" matBadgeColor="accent">
  home
</mat-icon>
```

## 无障碍

> Accessibility

标记需通过`matBadgeDescription`来指定一个有意义的描述。

> Badges should be given a meaningful description via `matBadgeDescription`. This description will be applied, via `aria-describedby` to the element decorated by `matBadge`.

当在一个`<mat-icon>`应用一个标记时，要知道图标默认被标记为`aria-hidden`。如果图标和标记的组合表示一些有意义的信息，则信息应以其他方式呈现。查看[指示器图标](https://material.angular.io/components/badge/overview#indicator-icons)的教程来获取更多信息。

> When applying a badge to a `<mat-icon>`, it is important to know that the icon is marked as `aria-hidden` by default. If the combination of icon and badge communicates some meaningful information, that information should be surfaced in another way. See the guidance on [indicator icons](https://material.angular.io/components/badge/overview#indicator-icons) for more information.

# API

## 标记的API参考

> API reference for Angular Material badge

```typescript
import {MatBadgeModule} from '@angular/material/badge';
```

## 指令

> Directives

###MatBadge

显示一个文本标记的指令。

> Directive to display a text badge.

Selector: `[matBadge]`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('matBadgeColor')<br>color: ThemePalette|标记的颜色，可以为primary, accent, 或者warn。|The color of the badge. Can be primary, accent, or warn.
@Input('matBadge')<br>content: string|标记的内容。|The content for the badge
@Input('matBadgeDescription')<br>description: string|通过aria-describedby描述装饰元素的信息。|Message used to describe the decorated element via aria-describedby
@Input('matBadgeHidden')<br>hidden: boolean|标记是否隐藏。|Whether the badge is hidden.
@Input('matBadgeOverlap')<br>overlap: boolean|标记是否与其内容交叠。|Whether the badge should overlap its contents or not
@Input('matBadgePosition')<br>position: MatBadgePosition|标记放置的位置。可由'above'\|'below'和'before'\|'after'进行组合。|Position the badge should reside. Accepts any combination of 'above'\|'below' and 'before'\|'after'
@Input('matBadgeSize')<br>size: MatBadgeSize|标记的尺寸，可以为'small', 'medium', 或者'large'。|Size of the badge. Can be 'small', 'medium', or 'large'.

## 方法

> Methods

isAbove
---
- 描述
  - 标记是否在属主的上面
  - Whether the badge is above the host or not
- 返回
  - boolean

isAfter
---
- 描述
  - 标记是否在属主的后面
  - Whether the badge is after the host or not
- 返回
  - boolean

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*