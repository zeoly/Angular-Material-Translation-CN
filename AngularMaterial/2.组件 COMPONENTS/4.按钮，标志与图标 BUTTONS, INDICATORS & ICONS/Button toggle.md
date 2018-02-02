[toc]

# 概述

> OVERVIEW

`<mat-button-toggle>`是一个按钮外观的开关切换。切换行为可以配置为单选按钮或者多选框。当单独使用时，通常是作为`mat-button-toggle-group`的一部分。

> `<mat-button-toggle>` are on/off toggles with the appearance of a button. These toggles can be configured to behave as either radio-buttons or checkboxes. While they can be standalone, they are typically part of a `mat-button-toggle-group`.

## 单选和多选

> Exclusive selection vs. multiple selection

`mat-button-toggle-group`默认情况下与radio-button group类似，只允许选择一个。在这种模式下，`mat-button-toggle-group`的`value`值为选择的按钮的值，并且支持`ngModel`。

> By default, `mat-button-toggle-group` acts like a radio-button group- only one item can be selected. In this mode, the `value` of the `mat-button-toggle-group` will reflect the value of the selected button and `ngModel` is supported.

添加`multiple`属性将允许多选（与checkbox类似）。在此模式下不使用切换按钮的值，`mat-button-toggle-group`没有value值，也不支持`ngModel`。

> Adding the `multiple` attribute allows multiple items to be selected (checkbox behavior). In this mode the values of the the toggles are not used, the `mat-button-toggle-group` does not have a value, and `ngModel` is not supported.

## 无障碍

> Accessibility

切换按钮基于`multiple`属性的存在来确定自己是多选框还是单选按钮。

> The button-toggles will present themselves as either checkboxes or radio-buttons based on the presence of the `multiple` attribute.

对于只包含图标的切换按钮，每个切换按钮都应当通过`aria-label`或`aria-labelledby`指定一个有意义的标签。

> For button toggles containing only icons, each button toggle should be given a meaningful label via `aria-label` or `aria-labelledby`.

对于切换按钮组，每个组都应当通过`aria-label`或`aria-labelledby`指定一个有意义的标签。

> For button toggle groups, each group should be given a meaningful label via `aria-label` or `aria-labelledby`.

## 方向

> Orientation

切换按钮可以通过添加`vertical`属性来渲染为垂直方向。

> The button-toggles can be rendered in a vertical orientation by adding the `vertical` attribute.

# API

## 切换按钮(grid-list)的API参考

> API reference for Angular Material button-toggle

```ts
import {MatButtonToggleModule} from '@angular/material/button-toggle';
```

## 指令

> Directives

### MatButtonToggleGroup

与单选按钮组类似的一个单元切换按钮组。

> Exclusive selection button toggle group that behaves like a radio-button group.

Selector: `mat-button-toggle-group:not([multiple])`

Exported as: `matButtonToggleGroup`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>name: string|内层输入元素的name属性。|name attribute for the underlying input element.
@Input()<br>selected: MatButtonToggle \| null|切换按钮组是否为选中。|Whether the toggle group is selected.
@Input()<br>value: any|切换按钮组的值。|Value of the toggle group.
@Input()<br>vertical: boolean|切换按钮组是否为垂直。|Whether the toggle group is vertical.
@Output()<br>change: EventEmitter&lt;MatButtonToggleChange&gt;|切换按钮组值发生变更的事件。|Event emitted when the group's value changes.
disabled: boolean|该组件是否无效。|Whether the component is disabled.

### MatButtonToggleGroupMultiple

多选的切换按钮组。此模式下不支持ngModel。

> Multiple selection button-toggle group. ngModel is not supported in this mode.

Selector: `mat-button-toggle-group[multiple]`

Exported as: `matButtonToggleGroup`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>vertical: boolean|切换按钮组是否为垂直。|Whether the toggle group is vertical.
disabled: boolean|该组件是否无效。|Whether the component is disabled.

### MatButtonToggle

在切换按钮组中的单个按钮。

> Single button inside of a toggle group.

Selector: `mat-button-toggle`

Exported as: `matButtonToggle`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('aria-label')<br>ariaLabel: string|添加到属主元素的aria-label属性。大多数情况下aria-labelledby优先级搞，此属性将被忽略。|Attached to the aria-label attribute of the host element. In most cases, arial-labelledby will take precedence so this may be omitted.
@Input('aria-labelledby')<br>ariaLabelledby: string \| null|用户可以指定aria-labelledby属性来传入输入元素。|Users can specify the aria-labelledby attribute which will be forwarded to the input element
@Input()<br>checked: boolean|按钮是否为checked。|Whether the button is checked.
@Input()<br>disabled: boolean|按钮是否为失效。|Whether the button is disabled.
@Input()<br>id: string|切换按钮的唯一ID。|The unique ID for this button toggle.
@Input()<br>name: string|用于分组单元按钮的HTML的'name'属性。|HTML's 'name' attribute used to group radios for unique selection.
@Input()<br>value: any|MatButtonToggleGroup用此来设置自己的value。|MatButtonToggleGroup reads this to assign its own value.
@Output()<br>change: EventEmitter&lt;MatButtonToggleChange&gt;|切换按钮组的值发生改变的事件。|Event emitted when the group value changes.
buttonToggleGroup: MatButtonToggleGroup|父切换按钮组（单选）。可选。|The parent button toggle group (exclusive selection). Optional.
buttonToggleGroupMultiple: MatButtonToggleGroupMultiple|父切换按钮组（多选）。可选。|The parent button toggle group (multiple selection). Optional.
inputId: string|内层输入元素的唯一ID。|Unique ID for the underlying input element.

**方法**

> Methods

方法名|描述|Description
-|-|-
focus|使按钮获得焦点。|Focuses the button.

## 附加类

> Additional classes

### MatButtonToggleChange

MatButtonToggle传递的变更事件对象。

> Change event object emitted by MatButtonToggle.

**属性**

> Properties

属性名|描述|Description
-|-|-
source: MatButtonToggle \| null|传递事件的MatButtonToggle。|The MatButtonToggle that emits the event.
value: any|MatButtonToggle的值。|The value assigned to the MatButtonToggle.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*