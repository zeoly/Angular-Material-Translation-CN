[toc]

# 概述

> OVERVIEW

Angular Material标签页将内容分配到不同的视图中，一次只有一个视图可见。每个标签页的标签显示在标签页头部，激活的标签页的标签会有一个动态墨水样式条。当标签页的标签列表超过头部的宽度时，会出现分页控件来让用户左右滚动标签。

> Angular Material tabs organize content into separate views where only one view can be visible at a time. Each tab's label is shown in the tab header and the active tab's label is designated with the animated ink bar. When the list of tab labels exceeds the width of the header, pagination controls appear to let the user scroll left and right across the labels.

激活的标签页可通过`selectedIndex`输入来设置，或者当用户选择头部的标签页标签时也可以激活。

> The active tab may be set using the `selectedIndex` input or when the user selects one of the tab labels in the header.

## 事件

> Events

当激活的标签页变化时，会触发`selectedTabChange`事件。

> The `selectedTabChange` output event is emitted when the active tab changes.

当用户使头部的一个标签页获得焦点，会触发`focusChange`事件。通常是键盘导航。

> The `focusChange` output event is emitted when the user puts focus on any of the tab labels in the header, usually through keyboard navigation.

## 标签

> Labels

如果标签页的标签仅为文本，则可使用简单的tab-group API。

> If a tab's label is only text then the simple tab-group API can be used.

```html
<mat-tab-group>
  <mat-tab label="One">
    <h1>Some tab content</h1>
    <p>...</p>
  </mat-tab>
  <mat-tab label="Two">
    <h1>Some more tab content</h1>
    <p>...</p>
  </mat-tab>
</mat-tab-group>
```

如果是复杂的标签，需要在`mat-tab`内容通过`mat-tab-label`指令增加一个模板。

> For more complex labels, add a template with the `mat-tab-label` directive inside the `mat-tab`.

```html
<mat-tab-group>
  <mat-tab>
    <ng-template mat-tab-label>
      The <em>best</em> pasta
    </ng-template>
    <h1>Best pasta restaurants</h1>
    <p>...</p>
  </mat-tab>
  <mat-tab>
    <ng-template mat-tab-label>
      <mat-icon>thumb_down</mat-icon> The worst sushi
    </ng-template>
    <h1>Terrible sushi restaurants</h1>
    <p>...</p>
  </mat-tab>
</mat-tab-group>
```

## 动态高度

> Dynamic Height

默认情况下，标签页组的高度不会随当前激活的标签页高度的变化而变化。可以通过设置`dynamicHeight`输入为true，来设置此功能。标签页body会根据激活标签页的高度自动调节高度。

> By default, the tab group will not change its height to the height of the currently active tab. To change this, set the `dynamicHeight` input to true. The tab body will animate its height according to the height of the active tab.

## 标签页与导航

> Tabs and navigation

当使用`<mat-tab-group>`在单路由中的不同视图之间切换时，`<nav mat-tab-nav-bar>`提供了标签页样式的UI来在路由中导航。

> While `<mat-tab-group>` is used to switch between views within a single route, `<nav mat-tab-nav-bar>` provides a tab-like UI for navigating between routes.

```html
<nav mat-tab-nav-bar>
  <a mat-tab-link
     *ngFor="let link of navLinks"
     [routerLink]="link.path"
     routerLinkActive #rla="routerLinkActive"
     [active]="rla.isActive">
    {{link.label}}
  </a>
</nav>

<router-outlet></router-outlet>
```

`tab-nav-bar`不和特定的路由绑定；仅与普通的`<a>`元素共同作用，并使用`active`属性来决定当前哪个标签页为激活。对应的`<router-outlet>`可放置在视图的任何位置。

> The `tab-nav-bar` is not tied to any particular router; it works with normal `<a>` elements and uses the `active` property to determine which tab is currently active. The corresponding `<router-outlet>` can be placed anywhere in the view.

## 无障碍

> Accessibility

没有文本或者标签的标签页，应通过`aria-label`或者`aria-labelledby`指定一个有意义的标签。对于`MatTabNav`，`<nav>`元素也应当有一个标签。

> Tabs without text or labels should be given a meaningful label via `aria-label` or `aria-labelledby`. For `MatTabNav`, the `<nav>` element should have a label as well.

### 键盘快捷键

> Keyboard shortcuts

快捷键|动作|Action
-|-|-
左方向键|移动焦点到上一个标签页。|Move focus to previous tab
右方向键|移动焦点到下一个标签页。|Move focus to next tab
HOME|移动焦点到第一个标签页。|Move focus to first tab
END|移动焦点到最后一个标签页。|Move focus to last tab
空格或回车|切换至获得焦点的标签页。|Switch to focused tab

# API

## 标签页(tabs)的API参考

> API reference for Angular Material tabs

```ts
import {MatTabsModule} from '@angular/material/tabs';
```

## 指令

> Directives

### MatTab

Selector: `mat-tab`

Exported as: `matTab`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>disabled: boolean|组件是否失效。|Whether the component is disabled.
@Input('label')<br>textLabel: string|标签页的纯文本标签，当没有模板标签时使用此属性。|The plain text label for the tab, used when there is no template label.
isActive: false|标签页当前是否激活。|Whether the tab is currently active.
origin: number \| null|如果在已存在一个选中标签页的情况下，一个标签页被创建和选中时的初值相对位置来源值。用于表示标签页的来源上下文。|The initial relatively index origin of the tab if it was created and selected after there was already a selected tab. Provides context of what position the tab should originate from.
position: number \| null|组件相对位置，0表示中间，负数表示在左，正数表示在右。|The relatively indexed position where 0 represents the center, negative is left, and positive represents the right.
templateLabel: MatTabLabel|使用<ng-template mat-tab-label\>指定的标签页的标签内容。|Content for the tab label given by <ng-template mat-tab-label\>.

### MatTabLabel **extends CdkPortal**

用于入口指令标识的标签。

> Used to flag tab labels for use with the portal directive

Selector: `[mat-tab-label]` `[matTabLabel]`

**属性**

> Properties

属性名|描述|Description
-|-|-
context: C \| undefined|传给内嵌视图的上下文数据。|Contextual data to be passed in to the embedded view.
isAttached: boolean|该入口是否附属于一个属主。|Whether this portal is attached to a host.
origin: ElementRef||
templateRef: TemplateRef<C\>|用于在属主内实例化一个内嵌视图的内嵌模板。|The embedded template that will be used to instantiate an embedded View in the host.
viewContainerRef: ViewContainerRef|ViewContainer参考。优先级高于模板。|Reference to the ViewContainer into which the template will be stamped out.

## 方法

> Methods

attach
---
- 描述
  - 
  - Attach the the portal to the provided PortalOutlet. When a context is provided it will override the context property of the TemplatePortal instance.
- 参数
 - host: PortalOutlet	
 - context: C | undefined = this.context	
- 返回
  - C

detach
---

setAttachedHost
---
- 描述
  - 不用attach()方法来设置PortalOutlet参考。当调用attach()或detach()时被PortalOutlet直接使用。
  - Sets the PortalOutlet reference without performing attach(). This is used directly by the PortalOutlet when it is performing an attach() or detach().
- 参数
  - host: PortalOutlet | null

### MatTabNav

适用于标签页组头部样式的导航组件。提供了动态墨水样式条的锚点导航。

> Navigation component matching the styles of the tab group header. Provides anchored navigation with animated ink bar.

Selector: `[mat-tab-nav-bar]`

Exported as: `matTabNavBar, matTabNav`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>backgroundColor: ThemePalette|标签页导航的背景色。|Background color of the tab nav.
@Input()<br>color: ThemePalette|组件的主题色调。|Theme color palette for the component.
@Input()<br>disableRipple: boolean|是否失效所有链接的水波效果。|Whether ripples should be disabled for all links or not.

## 方法

> Methods

updateActiveLink
---
- 描述
  - 通知组件激活的连接已变更。
  - Notifies the component that the active link has been changed.
- 参数
  - element: ElementRef

### MatTabLink

mat-tab-nav-bar内部链接。

> Link inside of a mat-tab-nav-bar.

Selector: `[mat-tab-link]` `[matTabLink]`

Exported as: `matTabLink`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>active: boolean|链接是否激活。|Whether the link is active.
@Input()<br>disableRipple: boolean|是否失效水波效果。|Whether ripples are disabled.
@Input()<br>disabled: boolean|组件是否失效。|Whether the component is disabled.

### MatTabGroup

Material design的tab-group组件。支持基本的标签页配对（标签+内容）和内含的动态墨水条，键盘导航，屏幕阅读等。参考：[https://www.google.com/design/spec/components/tabs.html](https://www.google.com/design/spec/components/tabs.html)

> Material design tab-group component. Supports basic tab pairs (label + content) and includes animated ink-bar, keyboard navigation, and screen reader. See: [https://www.google.com/design/spec/components/tabs.html](https://www.google.com/design/spec/components/tabs.html)

Selector: `mat-tab-group`

Exported as: `matTabGroup`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>backgroundColor: ThemePalette|标签页组的背景色。|Background color of the tab group.
@Input()<br>color: ThemePalette|组件的主题色调。|Theme color palette for the component.
@Input()<br>disableRipple: boolean|是否失效水波效果。|Whether ripples are disabled.
@Input()<br>dynamicHeight: boolean|标签页组是否自动适配激活标签页的尺寸。|Whether the tab group should grow to the size of the active tab.
@Input()<br>headerPosition: MatTabHeaderPosition|标签页头部的位置。|Position of the tab header.
@Input()<br>selectedIndex: number \| null|激活的标签页下标。|The index of the active tab.
@Output()<br>animationDone: EventEmitter<void\>|当body动画完成时的事件。|Event emitted when the body animation has completed
@Output()<br>focusChange: EventEmitter<MatTabChangeEvent\>|标签页组内的焦点发生变更时的事件。|Event emitted when focus has changed within a tab group.
@Output()<br>selectedIndexChange: EventEmitter<number\>|开启使用[(selectedIndex)]双向绑定。|Output to enable support for two-way binding on [(selectedIndex)]
@Output()<br>selectedTabChange: EventEmitter<MatTabChangeEvent\>|标签页选中发生变更的事件。|Event emitted when the tab selection has changed.

## 附加类

> Additional classes

### MatTabChangeEvent

当获得焦点或者选中变更时的事件。

> A simple change event emitted on focus or selection changes.

**属性**

> Properties

属性名|描述|Description
-|-|-
index: number|当前选中标签页的下标。|Index of the currently-selected tab.
tab: MatTab|当前选中标签页的参考。|Reference to the currently-selected tab.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*