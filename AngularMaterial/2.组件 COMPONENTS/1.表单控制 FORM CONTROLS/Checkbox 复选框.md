[toc]

# 概述

> OVERVIEW

<md-checkbox>提供了与原生<input type="checkbox">相同的功能，并增强了Material Design样式与动画。

> <md-checkbox> provides the same functionality as a native <input type="checkbox"> enhanced with Material Design styling and animations.

## 复选框标签

> Checkbox label

复选框标签是<md-checkbox>元素的内容。通过设置labelPosition属性为'before'或者'after'，可以将标签放置在复选框的前面或者后面。

> The checkbox label is provided as the content to the <md-checkbox> element. The label can be positioned before or after the checkbox by setting the labelPosition property to 'before' or 'after'.

如果不想复选框旁边出现标签，可以使用[aria-label](https://www.w3.org/TR/wai-aria/states_and_properties#aria-label)或者[aria-labelledby](https://www.w3.org/TR/wai-aria/states_and_properties#aria-labelledby)来指定合适的标签。

> If you don't want the label to appear next to the checkbox, you can use aria-label or aria-labelledby to specify an appropriate label.

## 使用@angular/forms

> Use with @angular/forms

<md-checkbox>与@angular/forms兼容，支持FormsModule和ReactiveFormsModule。

> <md-checkbox> is compatible with @angular/forms and supports both FormsModule and ReactiveFormsModule.

## 不确定状态

> Indeterminate state

<md-checkbox>与原生的<input type="checkbox">相似，支持不确定状态。当复选框的indeterminate属性为true时，将无视选中值，当作不确定进行渲染。任何用户与复选框的交互（例如点击）将移除此不确定状态。

> <md-checkbox> supports an indeterminate state, similar to the native <input type="checkbox">. While the indeterminate property of the checkbox is true, it will render as indeterminate regardless of the checked value. Any interaction with the checkbox by a user (i.e., clicking) will remove the indeterminate state.

## 主题

> Theming

使用color属性可以改变<md-checkbox>的颜色。复选框默认使用主题的重点色。此属性可以变更为'primary'或者'warn'。

> The color of a <md-checkbox> can be changed by using the color property. By default, checkboxes use the theme's accent color. This can be changed to 'primary' or 'warn'.

# API

## 复选框(checkbox)的API参考

> API reference for Angular Material checkbox

```typescript
import {MdCheckboxModule} from '@angular/material';
```

## 指令

> Directives

### MdCheckbox

这是一个material design的复选框组件。支持HTML5复选框的所有功能，并暴露相似的API。一个MdCheckbox可以为checked，unchecked，indeterminate，或者disabled。注意所有附加的可访问属性都由此组件维护，所以不需要你自己提供。但如果你想忽略一个标签并需要访问复选框，可以提供一个[aria-label]输入，参考[https://www.google.com/design/spec/components/selection-controls.html](https://www.google.com/design/spec/components/selection-controls.html)。

> A material design checkbox component. Supports all of the functionality of an HTML5 checkbox, and exposes a similar API. A MdCheckbox can be either checked, unchecked, indeterminate, or disabled. Note that all additional accessibility attributes are taken care of by the component, so there is no need to provide them yourself. However, if you want to omit a label and still have the checkbox be accessible, you may supply an [aria-label] input. See: https://www.google.com/design/spec/components/selection-controls.html

Selector：md-checkbox

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('aria-label')<br>ariaLabel|附属于属主元素的aria-label属性。大多数情况下，arial-labelledby优先，所以此属性可能被忽略。|Attached to the aria-label attribute of the host element. In most cases, arial-labelledby will take precedence so this may be omitted.
@Input('aria-labelledby')<br>ariaLabelledby|用户可以指定被转发到输入元素的aria-labelledby属性。|Users can specify the aria-labelledby attribute which will be forwarded to the input element
@Input()<br>id|复选框的唯一id。如果没有提供，将会自动生成。|A unique id for the checkbox. If one is not supplied, it is auto-generated.
@Input()<br>disableRipple|复选框的连锁反应是否失效。|Whether the ripple effect for this checkbox is disabled.
inputId|在<md-checkbox>中原生输入元素的id。|ID of the native input element inside <md-checkbox>
@Input()<br>required|是否引入复选框。|Whether the checkbox is required.
@Input()<br>Deprecated<br>align|复选框是否在标签前面或后面出现。|Whether or not the checkbox should appear before or after the label.
@Input()<br>labelPosition|标签出现在复选框前面或后面。默认为'after'。|Whether the label should appear after or before the checkbox. Defaults to 'after'
@Input()<br>name|应用到input元素的name值|Name value will be applied to the input element if present
@Output()<br>change|复选框checked值发生改变时发出的事件。|Event emitted when the checkbox's checked value changes.
@Output()<br>indeterminateChange|复选框indeterminate值发生改变时发出的事件。|Event emitted when the checkbox's indeterminate value changes.
@Input()<br>value|原生input元素的value属性。|The value attribute of the native input element
@Input()<br>checked|复选框是否勾选。|Whether the checkbox is checked.
@Input()<br>indeterminate|复选框是否为不确定状态。这可以被当作“混合”模式，表示具有三种状态的复选框。例如：表示一个有可选项的嵌套列表的复选框。但是要注意当复选框被手动点击后，不确定状态将立刻变为false。|Whether the checkbox is indeterminate. This is also known as "mixed" mode and can be used to represent a checkbox with three states, e.g. a checkbox that represents a nested list of checkable items. Note that whenever checkbox is manually clicked, indeterminate is immediately set to false.

**方法**

> Methods

方法名|描述|Description
-|-|-
|toggle|切换复选框的选中状态|Toggles the checked state of the checkbox.|
|focus|使复选框获得焦点|Focuses the checkbox.|

## 附加类

> Additional classes

### MdCheckboxChange

MdCheckbox发出的change事件对象

> Change event object emitted by MdCheckbox.

**属性**

> Properties

属性名|描述|Description
-|-|-
source|事件的源MdCheckbox|The source MdCheckbox of the event.
checked|复选框的新的选择状态|The new checked value of the checkbox.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*