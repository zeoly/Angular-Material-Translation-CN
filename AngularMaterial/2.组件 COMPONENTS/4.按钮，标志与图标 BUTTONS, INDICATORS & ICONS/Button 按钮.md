[toc]

# 概述

> OVERVIEW

Angular Material是原生的`<button>`或者`<a>`元素，并扩展了Material Design样式与墨水效果。

> Angular Material buttons are native `<button>` or `<a>` elements enhanced with Material Design styling and ink ripples.

原生`<button>`和`<a>`元素总是用来给用户提供最直接的交互体验。`<button>`元素用于执行某个*动作*。`<a>`元素用于用户想*跳转*到另一个视图。

> Native `<button>` and `<a>` elements are always used in order to provide the most straightforward and accessible experience for users. A `<button>` element should be used whenever some *action* is performed. An `<a>` element should be used whenever the user will *navigate* to another view.

按钮一共有5种变体，每种对应一个属性：

> There are five button variants, each applied as an attribute:

属性|描述|Description
-|-|-
`mat-button`|扁平效果的矩形按钮|Rectangular button w/ no elevation.
`mat-raised-button`|立体效果的矩形按钮|Rectangular button w/ elevation
`mat-icon-button`|背景透明的圆形按钮，用来包含一个图标|Circular button with a transparent background, meant to contain an icon
`mat-fab`|立体效果的圆形按钮，默认为主题的重点色|Circular button w/ elevation, defaults to theme's accent color
`mat-mini-fab`|与`md-fab`相同，但小一号|Same as `mat-fab` but smaller

## 主题

> Theming

通过设置按钮的`color`属性为`primary`, `accent`, 或者`warn`，可以变更按钮的背景色为主题的对应颜色。默认情况下，只有浮动动作按钮变色；`mat-button`和`mat-raised-button`的默认背景色为主题的背景色。

> Buttons can be colored in terms of the current theme using the `color` property to set the background color to `primary`, `accent`, or `warn`. By default, only FABs (Floating Action Button) are colored; the default background color for `mat-button` and `mat-raised-button` matches the theme's background color.

## 首字母大写

> Capitalization

根据Material design spec，按钮文字需要首字母大写，但在某些地方可能导致问题，所以我们可以通过`text-transform: uppercase`选择不自动大写。需要注意使用全大写的文字可能对屏幕阅读者不便，可能导致一个字母一个字母来读文本。在消费级应用中如何处理这类问题，我们保留讨论。

> According to the Material design spec button text has to be capitalized, however we have opted not to capitalize buttons automatically via `text-transform: uppercase`, because it can cause issues in certain locales. It is also worth noting that using ALL CAPS in the text itself causes issues for screen-readers, which will read the text character-by-character. We leave the decision of how to approach this to the consuming app.

## 可接入性

> Accessibility

Angular Material默认使用原生的`<button>`和`<a>`元素来保证可接入性体验。在当前页面执行动作的交互时应该使用`<button>`元素。*跳转到另一个视图*时应使用`<a>`元素。

> Angular Material uses native `<button>` and `<a>` elements to ensure an accessible experience by default. The `<button>` element should be used for any interaction that performs an action on the current page. The `<a>` element should be used for any interaction that *navigates to another view*.

只包含图标（例如`mat-fab`, `mat-mini-fab`和`mat-icon-button`）的按钮和链接，应通过`aria-label`或者`aria-labelledby`指定一个有意义的标签。

> Buttons or links containing only icons (such as `mat-fab`, `mat-mini-fab`, and `mat-icon-button`) should be given a meaningful label via `aria-label` or `aria-labelledby`.

# API

## 按钮的API参考

> API reference for Angular Material button

```typescript
import {MatButtonModule} from '@angular/material/button';
```

## 指令

> Directives

### MatButton

普通的Material design按钮。

> Material design button.

Selector: `button[mat-button]` `button[mat-raised-button]` `button[mat-icon-button]` `button[mat-fab]` `button[mat-mini-fab]`

Exported as: `matButton`

**方法**

> Methods

方法名|描述|Description
-|-|-
focus|使按钮获得焦点。|Focuses the button.

### MatAnchor **extends MatButton**

立体的Material design按钮。

> Raised Material design button.

Selector: `a[mat-button]` `a[mat-raised-button]` `a[mat-icon-button]` `a[mat-fab]` `a[mat-mini-fab]`

Exported as: `matButton`, `matAnchor`

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*
