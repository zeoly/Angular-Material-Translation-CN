[toc]

# 概述

> OVERVIEW

Angular Material提供了两组组件，用来添加在主内容旁边的可折叠的侧边内容（通常用于导航）。这些是侧边导航和抽屉组件。

> Angular Material provides two sets of components designed to add collapsible side content (often navigation, though it can be any content) alongside some primary content. These are the sidenav and drawer components.

侧边导航组件用于添加全屏应用的侧边内容。设置侧边导航需要使用三个组件：内容和侧边导航的结构化容器`<mat-sidenav-container>`，表示主要内容的`<mat-sidenav-content>`，表示已添加的侧边内容的`<mat-sidenav>`。

> The sidenav components are designed to add side content to a fullscreen app. To set up a sidenav we use three components: `<mat-sidenav-container>` which acts as a structural container for our content and sidenav, `<mat-sidenav-content>` which represents the main content, and `<mat-sidenav>` which represents the added side content.

抽屉组件用于在应用的小区域中添加侧边内容。使用`<mat-drawer-container>`, `<mat-drawer-content>`, 和`<mat-drawer>`组件实现，与侧边导航组件基本相似。我们并不是将侧边内容作为一个整体添加到应用中，而是添加到应用的小块区域中。两者几乎支持相同的特性，但不支持固定位置。

> The drawer component is designed to add side content to a small section of your app. This is accomplished using the `<mat-drawer-container>`, `<mat-drawer-content>`, and `<mat-drawer>` components, which are analogous to their sidenav equivalents. Rather than adding side content to the app as a whole, these are designed to add side content to a small section of your app. They support almost all of the same features, but do not support fixed positioning.

## 指定主内容和侧边内容

> Specifying the main and side content

主内容和侧边内容都需要放置在`<mat-sidenav-container>`中，不想被侧边导航影响的内容，例如header或者footer，可以放在容器的外面。

> Both the main and side content should be placed inside of the `<mat-sidenav-container>`, content that you don't want to be affected by the sidenav, such as a header or footer, can be placed outside of the container.

侧边内容需包含在`<mat-sidenav>`元素中。可以使用`position`属性来指定应该在主内容的哪一边放置侧边内容。`position`可设置为`start`或者`end`来将放置侧边内容。如果未设置`position`，则默认值为`start`。一个`<mat-sidenav-container>`最多可包含两个`<mat-sidenav>`元素，但一边只能有一个。

> The side content should be wrapped in a `<mat-sidenav>` element. The `position` property can be used to specify which end of the main content to place the side content on. `position` can be either `start` or `end` which places the side content on the left or right respectively in left-to-right languages. If the `position` is not set, the default value of `start` will be assumed. A `<mat-sidenav-container>` can have up to two `<mat-sidenav>` elements total, but only one for any given side.

主内容需包含在`<mat-sidenav-content>`中。如果`<mat-sidenav-container>`中没有指定`<mat-sidenav-content>`，将隐式创建一个，并且`<mat-sidenav-container>`内部除了`<mat-sidenav>`的所有内容都将替换。

> The main content should be wrapped in a `<mat-sidenav-content>`. If no `<mat-sidenav-content>` is specified for a `<mat-sidenav-container>`, one will be created implicitly and all of the content inside the `<mat-sidenav-container>` other than the `<mat-sidenav>` elements will be placed inside of it.

以下是合理的侧边导航布局的例子：

> The following are examples of valid sidenav layouts:

```html
<!-- Creates a layout with a left-positioned sidenav and explicit content. -->
<mat-sidenav-container>
  <mat-sidenav>Start</mat-sidenav>
  <mat-sidenav-content>Main</mat-sidenav-content>
</mat-sidenav-container>
```

```html
<!-- Creates a layout with a left and right sidenav and implicit content. -->
<mat-sidenav-container>
  <mat-sidenav>Start</mat-sidenav>
  <mat-sidenav position="end">End</mat-sidenav>
  <section>Main</section>
</mat-sidenav-container>
```

```html
<!-- Creates an empty sidenav container with no sidenavs and implicit empty content. -->
<mat-sidenav-container></mat-sidenav-container>
```

以下是不合理的侧边导航布局例子：

> And these are examples of invalid sidenav layouts:

```html
<!-- Invalid because there are two `start` position sidenavs. -->
<mat-sidenav-container>
  <mat-sidenav>Start</mat-sidenav>
  <mat-sidenav position="start">Start 2</mat-sidenav>
</mat-sidenav-container>
```

```html
<!-- Invalid because there are multiple `<mat-sidenav-content>` elements. -->
<mat-sidenav-container>
  <mat-sidenav-content>Main</mat-sidenav-content>
  <mat-sidenav-content>Main 2</mat-sidenav-content>
</mat-sidenav-container>
```

```html
<!-- Invalid because the `<mat-sidenav>` is outside of the `<mat-sidenav-container>`. -->
<mat-sidenav-container></mat-sidenav-container>
<mat-sidenav></mat-sidenav>
```

抽屉组件也有同样的规则。

> These same rules all apply to the drawer components as well.

## 打开和关闭一个侧边导航

> Opening and closing a sidenav

`<mat-sidenav>`可以通过`open()`, `close()`和`toggle()`方法打开或关闭。这三个方法都会返回`Promise<boolean>`，当侧边导航以打开状态结束时返回`true`，关闭状态则为`false`。

> A `<mat-sidenav>` can be opened or closed using the `open()`, `close()` and `toggle()` methods. Each of these methods returns a `Promise<boolean>` that will be resolved with `true` when the sidenav finishes opening or `false` when it finishes closing.

折叠状态也可以使用模板中绑定的`opened`属性来设置。此属性支持双向绑定。

> The opened state can also be set via a property binding in the template using the `opened` property. The property supports 2-way binding.

`<mat-sidenav>`同样支持输出属性，如打开和关闭事件，分别为`(opened)`和`(closed)`属性。

> `<mat-sidenav>` also supports output properties for just open and just close events, The `(opened)` and `(closed)` properties respectively.

所有属性和方法也对`<mat-drawer>`同样有效。

> All of these properties and methods work on `<mat-drawer>` as well.

## 改变侧边导航行为

> Changing the sidenav's behavior

基于`mode`属性，`<mat-sidenav>`有三种不同的渲染方式。

> The `<mat-sidenav>` can render in one of three different ways based on the `mode` property.

Mode|描述|Description
-|-|-
`over`|侧边导航浮动在主内容之上，被背景覆盖|Sidenav floats over the primary content, which is covered by a backdrop
`push`|侧边导航推出主内容，同样被背景覆盖|Sidenav pushes the primary content out of its way, also covering it with a backdrop
`side`|侧边导航在主内容的旁边，会挤压主内容的宽度来放置侧边导航|Sidenav appears side-by-side with the main content, shrinking the main content's width to make space for the sidenav.

如果为设置`mode`，则默认为over`。

> If no `mode` is specified, `over` is used by default.

`<mat-drawer>`也支持同样的模式。

> `<mat-drawer>` also supports all of these same modes.

## 失效自动关闭

> Disabling automatic close

点击背景或按Esc键通常会关闭侧边导航。也可以通过设置`<mat-sidenav>`或者`<mat-drawer>`中的`disableClose`属性来失效自动关闭的行为。

> Clicking on the backdrop or pressing the Esc key will normally close an open sidenav. However, this automatic closing behavior can be disabled by setting the `disableClose` property on the `<mat-sidenav>` or `<mat-drawer>` that you want to disable the behavior for.

在`<mat-sidenav>`中添加一个按键监听可以自定义Esc的处理。设置`<mat-sidenav-container>`中的`(backdropClick)`输出属性可以自定义背景点击处理。

> Custom handling for Esc can be done by adding a keydown listener to the `<mat-sidenav>`. Custom handling for backdrop clicks can be done via the `(backdropClick)` output property on `<mat-sidenav-container>`.

## 调整打开的侧边导航尺寸

> Resizing an open sidenav

默认Material只会在几个关键点（打开，窗口大小调整，模式变更）测量并调整抽屉容器的尺寸，以防止布局错误，但某些情况下仍会有错误。如果你的应用在抽屉打开时需要调整宽度，可以使用`autosize`选项来告诉Material保持持续测量。注意需**谨慎使用**此选项，因为可能导致某些性能问题。

> By default, Material will only measure and resize the drawer container in a few key moments (on open, on window resize, on mode change) in order to avoid layout thrashing, however there are cases where this can be problematic. If your app requires for a drawer to change its width while it is open, you can use the `autosize` option to tell Material to continue measuring it. Note that you should use this option **at your own risk**, because it could cause performance issues.

## 设置侧边导航尺寸

> Setting the sidenav's size

默认`<mat-sidenav>`和`<mat-drawer>`会适应其内容的尺寸。也可以通过CSS明确指定宽度：

>The `<mat-sidenav>` and `<mat-drawer>` will, by default, fit the size of its content. The width can be explicitly set via CSS:

```css
mat-sidenav {
  width: 200px;
}
```

避免使用百分数，因为`resize`事件目前还不支持。

> Try to avoid percent based width as `resize` events are not (yet) supported.

## 固定位置的侧边导航

> Fixed position sidenavs

只有`<mat-sidenav>`支持固定位置（`<mat-drawer>`不支持）。可通过设置`fixedInViewport`属性开启功能。另外可通过`fixedTopGap`和`fixedBottomGap`设置顶部和底部的空间。这两个属性都接收像素值参数作为顶部和底部的空间。

> For `<mat-sidenav>` only (not `<mat-drawer>`) fixed positioning is supported. It can be enabled by setting the `fixedInViewport` property. Additionally, top and bottom space can be set via the `fixedTopGap` and `fixedBottomGap`. These properties accept a pixel value amount of space to add at the top or bottom.

## 为移动和桌面创建响应式布局

> Creating a responsive layout for mobile & desktop

在移动端和桌面端，通常侧边导航需要有不同的行为。在桌面端，通常只需要内容区滚动。但移动端通常希望body为滚动元素；可以做到地址自动隐藏。可通过设置侧边导航的CSS来适配不同的设备。

> A sidenav often needs to behave differently on a mobile vs a desktop display. On a desktop, it may make sense to have just the content section scroll. However, on mobile you often want the body to be the element that scrolls; this allows the address bar to auto-hide. The sidenav can be styled with CSS to adjust to either type of device.

## 无障碍

> Accessibility

`<mat-sidenav>`和`<mat-sidenav-content>`需根据使用的内容来指定一个适当的`role`属性。

> The `<mat-sidenav>` an `<mat-sidenav-content>` should each be given an appropriate `role` attribute depending on the context in which they are used.

例如，包含导向其他页面的超链接的`<mat-sidenav>`应当标记`role="navigation"`，包含内容表格的应标记`role="directory"`。如果没有特定的角色描述侧边导航，则推荐`role="region"`。

> For example, a `<mat-sidenav>` that contains links to other pages might be marked `role="navigation"`, whereas one that contains a table of contents about might be marked as `role="directory"`. If there is no more specific role that describes your sidenav, `role="region"` is recommended.

类似的，`<mat-sidenav-content>`也应基于包含的内容指定一个角色。如果表示页面的主内容，则标记为`role="main"`。如果没有特定角色，也推荐标记为`role="region"`。

> Similarly, the `<mat-sidenav-content>` should be given a role based on what it contains. If it represents the primary content of the page, it may make sense to mark it `role="main"`. If no more specific role makes sense, `role="region"` is again a good fallback.

## 问题定位

> Troubleshooting

### Error: A drawer was already declared for 'position="..."'

如果在一个指定的容器有多个侧面导航或者抽屉拥有相同的`position`，则会出现这个错误。`position`属性默认是`start`，所以将末尾的侧面导航设置为`position="end"`即可。

> This error is thrown if you have more than one sidenav or drawer in a given container with the same `position`. The `position` property defaults to `start`, so the issue may just be that you forgot to mark the `end` sidenav with `position="end"`.

# API

## 侧边导航(sidenav)的API参考

> API reference for Angular Material sidenav

```ts
import {MatSidenavModule} from '@angular/material/sidenav';
```

## 指令

> Directives

### MatDrawerContent

Selector: `mat-drawer-content`

### MatDrawer

此组件为可在抽屉容器中打开的一个抽屉。

> This component corresponds to a drawer that can be opened on the drawer container.

Selector: `mat-drawer`

Exported as: `matDrawer`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>disableClose: boolean|当按Esc键或点击背景时，是否关闭抽屉。|Whether the drawer can be closed with the escape key or by clicking on the backdrop.
@Input()<br>mode: 'over' \| 'push' \| 'side'|抽屉的模式，取值为三者其一。|Mode of the drawer; one of 'over', 'push' or 'side'.
@Input()<br>opened: boolean|抽屉是否为打开状态。当我们触发一个事件的开始或结束时需要获取。|Whether the drawer is opened. We overload this because we trigger an event when it starts or end.
@Input()<br>position: 'start' \| 'end'|抽屉应该贴附上的位置。|The side that the drawer is attached to.
@Output()<br>closedStart: Observable&lt;void&gt;|抽屉开始关闭时的事件。|Event emitted when the drawer has started closing.
@Output('positionChanged')<br>onPositionChanged: new EventEmitter&lt;void&gt;()|抽屉位置发生变更的事件。|Event emitted when the drawer's position changes.
@Output()<br>openedChange: EventEmitter&lt;boolean&gt;|抽屉打开状态发生变更的事件。|Event emitted when the drawer open state is changed.
@Output()<br>openedStart: Observable&lt;void&gt;|抽屉开始打开时的事件。|Event emitted when the drawer has started opening.

**方法**

> Methods

close
---
- 描述
  - 关闭抽屉
  - Close the drawer.
- 返回
  - Promise<void>

open
---
- 描述
  - 打开抽屉
  - Open the drawer.
- 参数
  - openedVia?FocusOrigin：抽屉是由按键，鼠标点击，还是程序打开。用于侧边导航关闭后的焦点控制。
  - Whether the drawer was opened by a key press, mouse click or programmatically. Used for focus management after the sidenav is closed.
- 返回
  - Promise<void>

toggle
---
- 描述
  - 切换抽屉
  - Toggle this drawer.
- 参数
  - isOpen boolean = !this.opened：抽屉是否应被打开。
  - Whether the drawer should be open.
  - openedVia FocusOrigin = 'program'：抽屉是由按键，鼠标点击，还是程序打开。用于侧边导航关闭后的焦点控制。
  - Whether the drawer was opened by a key press, mouse click or programmatically. Used for focus management after the sidenav is closed.
- 返回
  - Promise<void>	

### MatDrawerContainer

这是一到两个组件的父组件，用于校验内部状态和协调背景与内容样式。

> This is the parent component to one or twos that validates the state internally and coordinates the backdrop and content styling.

Selector: `mat-drawer-container`

Exported as: `matDrawerContainer`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>autosize: boolean|当抽屉的尺寸发生变更时，其容器是否自动调整尺寸。**谨慎使用**开启此选项将导致在抽屉每次监测到变更时，都会导致布局变更。可通过MAT_DRAWER_DEFAULT_AUTOSIZE进行全局设置。|Whether to automatically resize the container whenever the size of any of its drawers changes.**Use at your own risk!** Enabling this option can cause layout thrashing by measuring the drawers on every change detection cycle. Can be configured globally via the MAT_DRAWER_DEFAULT_AUTOSIZE token.
@Output()<br>backdropClick: new EventEmitter&lt;void&gt;()|抽屉背景被点击时的事件。|Event emitted when the drawer backdrop is clicked.
end: MatDrawer \| null|子抽屉的末尾位置。|The drawer child with the end position.
start: MatDrawer \| null|自抽屉的开始位置。|The drawer child with the start position.

**方法**

> Methods

方法名|描述|Description
-|-|-
close|关闭所有子抽屉。|Calls close of both start and end drawers
open|打开所有子抽屉。|Calls open of both start and end drawers

### MatSidenavContent **extends MatDrawerContent**

Selector: `mat-sidenav-content`

### MatSidenav **extends MatDrawer**

Selector: `mat-sidenav`

Exported as: `matSidenav`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>disableClose: boolean|当按Esc键或点击背景时，是否关闭抽屉。|Whether the drawer can be closed with the escape key or by clicking on the backdrop.
@Input()<br>fixedBottomGap: number|当侧边导航为固定模式时，侧边导航底部和可视区域底部之间的间隔。|The gap between the bottom of the sidenav and the bottom of the viewport when the sidenav is in fixed mode.
@Input()<br>fixedInViewport: boolean|侧边导航是否固定在可是区域中。|Whether the sidenav is fixed in the viewport.
@Input()<br>fixedTopGap: number|当侧边导航为固定模式时，侧边导航顶部和可视区域顶部之间的间隔。|The gap between the top of the sidenav and the top of the viewport when the sidenav is in fixed mode.
@Input()<br>mode: 'over' \| 'push' \| 'side'|抽屉的模式，取值为三者其一。|Mode of the drawer; one of 'over', 'push' or 'side'.
@Input()<br>opened: boolean|抽屉是否为打开状态。当我们触发一个事件的开始或结束时需要获取。|Whether the drawer is opened. We overload this because we trigger an event when it starts or end.
@Input()<br>position: 'start' \| 'end'|抽屉应该贴附上的位置。|The side that the drawer is attached to.
@Output()<br>closedStart: Observable&lt;void&gt;|抽屉开始关闭时的事件。|Event emitted when the drawer has started closing.
@Output('positionChanged')<br>onPositionChanged: new EventEmitter&lt;void&gt;()|抽屉位置发生变更的事件。|Event emitted when the drawer's position changes.
@Output()<br>openedChange: EventEmitter&lt;boolean&gt;|抽屉打开状态发生变更的事件。|Event emitted when the drawer open state is changed.
@Output()<br>openedStart: Observable&lt;void&gt;|抽屉开始打开时的事件。|Event emitted when the drawer has started opening.

**方法**

> Methods

close
---
- 描述
  - 关闭抽屉
  - Close the drawer.
- 返回
  - Promise<void>

open
---
- 描述
  - 打开抽屉
  - Open the drawer.
- 参数
  - openedVia?FocusOrigin：抽屉是由按键，鼠标点击，还是程序打开。用于侧边导航关闭后的焦点控制。
  - Whether the drawer was opened by a key press, mouse click or programmatically. Used for focus management after the sidenav is closed.
- 返回
  - Promise<void>

toggle
---
- 描述
  - 切换抽屉
  - Toggle this drawer.
- 参数
  - isOpen boolean = !this.opened：抽屉是否应被打开。
  - Whether the drawer should be open.
  - openedVia FocusOrigin = 'program'：抽屉是由按键，鼠标点击，还是程序打开。用于侧边导航关闭后的焦点控制。
  - Whether the drawer was opened by a key press, mouse click or programmatically. Used for focus management after the sidenav is closed.
- 返回
  - Promise<void>	

### MatSidenavContainer **extends MatDrawerContainer**

Selector: `mat-sidenav-container`

Exported as: `matSidenavContainer`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>autosize: boolean|当抽屉的尺寸发生变更时，其容器是否自动调整尺寸。**谨慎使用**开启此选项将导致在抽屉每次监测到变更时，都会导致布局变更。可通过MAT_DRAWER_DEFAULT_AUTOSIZE进行全局设置。|Whether to automatically resize the container whenever the size of any of its drawers changes.**Use at your own risk!** Enabling this option can cause layout thrashing by measuring the drawers on every change detection cycle. Can be configured globally via the MAT_DRAWER_DEFAULT_AUTOSIZE token.
@Output()<br>backdropClick: new EventEmitter&lt;void&gt;()|抽屉背景被点击时的事件。|Event emitted when the drawer backdrop is clicked.
end: MatDrawer \| null|子抽屉的末尾位置。|The drawer child with the end position.
start: MatDrawer \| null|自抽屉的开始位置。|The drawer child with the start position.

**方法**

> Methods

方法名|描述|Description
-|-|-
close|关闭所有子抽屉。|Calls close of both start and end drawers
open|打开所有子抽屉。|Calls open of both start and end drawers

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*