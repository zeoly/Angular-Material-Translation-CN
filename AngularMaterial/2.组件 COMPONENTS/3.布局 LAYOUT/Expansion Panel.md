[toc]

# 概述

> OVERVIEW

`<mat-expansion-panel>`提供了一个可扩展的细节概要视图。

> `<mat-expansion-panel>` provides an expandable details-summary view.

## 扩展面板内容

> Expansion-panel content

每个扩展面板都必须包含一个头部，可能包含一个操作条。

> Each expansion-panel must include a header and may optionally include an action bar.

### 头部

> Header

`<mat-expansion-panel-header>`展示了面板内容概要并控制面板的展开和收缩。头部可以包含一个`<mat-panel-title>`和一个`<mat-panel-description>`，用于格式化头部的内容以适配Material Design标准。

> The `<mat-expansion-panel-header>` shows a summary of the panel content and acts as the control for expanding and collapsing. This header may optionally contain an `<mat-panel-title>` and an `<mat-panel-description>`, which format the content of the header to align with Material Design specifications.

默认扩展面板头部在尾部有一个切换图标，指示当前展开状态。可通过`hideToggle`属性隐藏此图标。

> By default, the expansion-panel header includes a toggle icon at the end of the header to indicate the expansion state. This icon can be hidden via the `hideToggle` property.

```html
<mat-expansion-panel>
  <mat-expansion-panel-header>
    <mat-panel-title>
      This is the expansion title
    </mat-panel-title>
    <mat-panel-description>
      This is a summary of the content
    </mat-panel-description>
  </mat-expansion-panel-header>

  <p>This is the primary content of the panel.</p>

</mat-expansion-panel>
```

### 操作条

> Action bar

面板底部可选则添加操作条，且只有当面板为展开状态时可见。

> Actions may optionally be included at the bottom of the panel, visible only when the expansion is in its expanded state.

```html
<mat-expansion-panel>
  <mat-expansion-panel-header>
    This is the expansion title
  </mat-expansion-panel-header>

  <p>This is the primary content of the panel.</p>

  <mat-action-row>
    <button mat-button>Click me</button>
  </mat-action-row>
</mat-expansion-panel>
```

### 失效一个面板

> Disabling a panel

使用`disabled`属性可以失效扩展面板。用户无法切换一个失效的面板，但仍然可以通过程序来操作。

> Expansion panels can be disabled using the `disabled` attribute. A disabled expansion panel can't be toggled by the user, but can still be manipulated programmatically.

```html
<mat-expansion-panel [disabled]="isDisabled">
  <mat-expansion-panel-header>
    This is the expansion title
  </mat-expansion-panel-header>
  <mat-panel-description>
    This is a summary of the content
  </mat-panel-description>
</mat-expansion-panel>
```

## 折叠面板

> Accordion

多个折叠面板可以合并为一个折叠面板。设置`multi="true"`输入允许为每个折叠面板单独设置折叠状态。设置`multi="false"`（默认）表示一次只能有一个面板是扩展状态。

> Multiple expansion-panels can be combined into an accordion. The `multi="true"` input allows the expansions state to be set independently of each other. When `multi="false"` (default) just one panel can be expanded at a given time:

```html
<mat-accordion>

  <mat-expansion-panel>
    <mat-expansion-panel-header>
      This is the expansion 1 title
    </mat-expansion-panel-header>

    This the expansion 1 content

  </mat-expansion-panel>

  <mat-expansion-panel>
    <mat-expansion-panel-header>
      This is the expansion 2 title
    </mat-expansion-panel-header>

    This the expansion 2 content

  </mat-expansion-panel>

</mat-accordion>
```

## 延迟渲染

> Lazy rendering

默认情况下，面板关闭时扩展面板的内容也会初始化。如果想在面板打开的时候再初始化，应当将内容作为`ng-template`提供：

> By default, the expansion panel content will be initialized even when the panel is closed. To instead defer initialization until the panel is open, the content should be provided as an `ng-template`:

```html
<mat-expansion-panel>
  <mat-expansion-panel-header>
    This is the expansion title
  </mat-expansion-panel-header>

  <ng-template matExpansionPanelContent>
    Some deferred content
  </ng-template>
</mat-expansion-panel>
```

## 无障碍

> Accessibility

扩展面板模仿了原生`<details>`和`<summary>`元素的体验。扩展面板头部有`role="button"`，并且使用扩展面板的id值作为`aria-controls`属性。

> The expansion-panel aims to mimic the experience of the native `<details>` and `<summary>` elements. The expansion panel header has `role="button"` and also the attribute `aria-controls` with the expansion panel's id as value.

扩展面板的头部为按钮。用户可以使用键盘来激活扩展面板，切换其扩展和收缩状态。因为头部是作为一个按钮，所以不可在头部内部放置其他交互元素。

> The expansion panel headers are buttons. Users can use the keyboard to activate the expansion panel header to switch between expanded state and collapsed state. Because the header acts as a button, additional interactive elements should not be put inside of the header.

# API

## 扩展面板(expansion)的API参考

> API reference for Angular Material expansion

```ts
import {MatExpansionModule} from '@angular/material/expansion';
```

## 指令

> Directives

### MatAccordion **extends CdkAccordion**

Material Design折叠面板指令

> Directive for a Material Design Accordion.

Selector: `mat-accordion`

Exported as: `matAccordion`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>displayMode: MatAccordionDisplayMode|折叠面板中所有的扩展面板的显示模式。目前有两种显示模式：default - 每个扩展面板的周围都有一个凹陷，可在折叠面板中设置扩展面板的不同高度。flat - 扩展面板周围没有间隔，所有的面板都在同一高度上。|The display mode used for all expansion panels in the accordion. Currently two display modes exist: default - a gutter-like spacing is placed around any expanded panel, placing the expanded panel at a different elevation from the reset of the accordion. flat - no spacing is placed around expanded panels, showing all panels at the same elevation.
@Input()<br>hideToggle: boolean|是否隐藏扩展指示器。|Whether the expansion indicator should be hidden.
@Input()<br>multi: boolean|折叠面板是否允许多个面板同时扩展。|Whether the accordion should allow multiple expanded accordion items simulateously.
id: \`cdk-accordion-${nextId++}\`|用于唯一选择的id值。|A readonly id value to use for unique selection coordination.

### MatExpansionPanel

此组件可作为单一元素来展示可扩展的内容，或者作为MatAccordion指令的子元素之一。

> This component can be used as a single element to show expandable content, or as one of multiple children of an element with the MatAccordion directive attached.

使用方法请参考README.md中的例子。

> Please refer to README.md for examples on how to use it.

Selector: `mat-expansion-panel`

Exported as: `matExpansionPanel`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>hideToggle: boolean|是否隐藏切换指示器。|Whether the toggle indicator should be hidden.
accordion: MatAccordion|可选定义扩展面板所属的折叠面板。|Optionally defined accordion the expansion panel belongs to.
disabled: boolean|此组件是否失效。|Whether the component is disabled.

### MatExpansionPanelActionRow

Selector: `mat-action-row`

### MatExpansionPanelHeader

此组件为扩展面板的头部元素。

> This component corresponds to the header element of an.

使用方法请参考README.md中的例子。

> Please refer to README.md for examples on how to use it.

Selector: `mat-expansion-panel-header`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>collapsedHeight: string|面板收缩时头部的高度。|Height of the header while the panel is collapsed.
@Input()<br>expandedHeight: string|面板展开时头部的高度。|Height of the header while the panel is expanded.
panel: MatExpansionPanel||

### MatExpansionPanelDescription

指令。此指令用于MatExpansionPanelHeader组件内部。

> directive.
> This direction is to be used inside of the MatExpansionPanelHeader component.

Selector: `mat-panel-description`

### MatExpansionPanelTitle

指令。此指令用于MatExpansionPanelHeader组件内部。

> directive.
> This direction is to be used inside of the MatExpansionPanelHeader component.

Selector: `mat-panel-title`

### MatExpansionPanelContent

第一次打开后的延迟渲染的扩展面板内容。

> Expansion panel content that will be rendered lazily after the panel is opened for the first time.

Selector: `ng-template[matExpansionPanelContent]`

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*