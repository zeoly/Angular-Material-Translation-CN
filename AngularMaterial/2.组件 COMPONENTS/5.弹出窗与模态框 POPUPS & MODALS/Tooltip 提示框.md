[toc]

# 概述

> OVERVIEW

Angular Material提供了一个文本标签，当用户悬停或长按一个元素时会显示。

> The Angular Material tooltip provides a text label that is displayed with the user hovers over or longpresses an element.

## 位置

> Positioning

提示框展示在元素的下方，但可以通过mdTooltipPosition来设置。提示框可以展示在元素的上、下、左、右。默认在下方。如果提示框在RTL布局方向中需要切换左右，则需要用before和after来替代left和right。

> The tooltip will be displayed below the element but this can be configured using the mdTooltipPosition input. The tooltip can be displayed above, below, left, or right of the element. By default the position will be below. If the tooltip should switch left/right positions in an RTL layout direction, then the positions before and after should be used instead of left and right, respectively.

Position|描述|Description
-|-|-
above|展示在元素上方|Always display above the element
below|展示在元素下方|Always display beneath the element
left|展示在元素左边|Always display to the left of the element
right|展示在元素右边|Always display to the right of the element
before|在LTR布局中展示在左边，在RTL布局中展示在右边|Display to the left in left-to-right layout and to the right in right-to-left layout
after|在LTR布局中展示在右边，在RTL布局中展示在左边|Display to the right in left-to-right layout and to the left in right-to-left layout

## 显示与隐藏

> Showing and hiding

当鼠标悬停在元素上时，提示框会立即显示，移开后则会立即隐藏。通过设置mdTooltipShowDelay和mdTooltipHideDelay可以设置提示框显示和隐藏的延迟。

> The tooltip is immediately shown when the user's mouse hovers over the element and immediately hides when the user's mouse leaves. A delay in showing or hiding the tooltip can be added through the inputs mdTooltipShowDelay and mdTooltipHideDelay.

在移动端，当用户长按元素时显示，并且在1500ms后隐藏。长按操作需要引入HammerJS。

> On mobile, the tooltip is displayed when the user longpresses the element and hides after a delay of 1500ms. The longpress behavior requires HammerJS to be loaded on the page.

还可以通过show和hide指令方法来控制提示框的显示与隐藏，也接受毫秒单位的数字来控制显示变更的延迟。

> The tooltip can also be shown and hidden through the show and hide directive methods, which both accept a number in milliseconds to delay before applying the display change.

使用mdTooltipDisabled可以关闭提示框，不再展示给用户。

> To turn off the tooltip and prevent it from showing to the user, use the mdTooltipDisabled input flag.

# API

## 提示框（tooltip）的API参考

> API reference for Angular Material tooltip

```typescript
import {MdTooltipModule} from '@angular/material';
```

## 指令

> Directives

### MdTooltip

在元素中添加一个material design提示框的指令。可在指定位置（默认在元素下方）动画展示和隐藏提示框。

> Directive that attaches a material design tooltip to the host element. Animates the showing and hiding of a tooltip provided position (defaults to below the element).

[https://material.google.com/components/tooltips.html](https://material.google.com/components/tooltips.html)

Selector: [md-tooltip] [mdTooltip]

导出属性: mdTooltip

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('mdTooltipPosition')<br>position|允许用户设置提示框相对于父元素的位置。|Allows the user to define the position of the tooltip relative to the parent element
@Input('mdTooltipDisabled')<br>disabled|失效提示框的显示。|Disables the display of the tooltip.
@Input('mdTooltipShowDelay')<br>showDelay|单位毫秒的默认显示延迟。|The default delay in ms before showing the tooltip after show is called
@Input('mdTooltipHideDelay')<br>hideDelay|单位毫秒的默认隐藏延迟。|The default delay in ms before hiding the tooltip after hide is called
@Input('mdTooltip')<br>message|显示在提示框中的信息。|The message to be displayed in the tooltip
@Input('mdTooltipClass')<br>tooltipClass|传递到提示框的类。支持与ngClass相同的语法。|Classes to be passed to the tooltip. Supports the same syntax as ngClass.

**方法**

> Methods

方法名|描述|Description
-|-|-
show|在单位毫秒后显示提示框，默认与tooltip-delay-show一致，未设置则为0ms|Shows the tooltip after the delay in ms, defaults to tooltip-delay-show or 0ms if no input
hide|在单位毫秒后隐藏提示框，默认与tooltip-delay-hide一致，未设置则为0ms|Hides the tooltip after the delay in ms, defaults to tooltip-delay-hide or 0ms if no input
toggle|显示/隐藏提示框|Shows/hides the tooltip

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*