[toc]

# 概述

> OVERVIEW

<md-radio>提供了与原生<input type="radio">相同的功能，并增强了Material Design样式与动画。

> <md-radio> provides the same functionality as a native <input type="radio"> enhanced with Material Design styling and animations.

具有相同name的一组单选按钮，一次只能选中其中一个。

> All radio-buttons with the same name comprise a set from which only one may be selected at a time.

## Radio-button标签

> Radio-button label

radio-button标签提供为<md-checkbox>元素的内容。通过设置labelPosition属性为'before'或者'after'，可以指定标签的位置显示在单元按钮前面还是后面。

> The radio-button label is provided as the content to the <md-checkbox> element. The label can be positioned before or after the radio-button by setting the labelPosition property to 'before' or 'after'.

如果不想让标签显示在单选按钮的旁边，你可以使用[aria-label](https://www.w3.org/TR/wai-aria/states_and_properties#aria-label)或者[aria-labelledby](https://www.w3.org/TR/wai-aria/states_and_properties#aria-labelledby)来指定合适的标签。

> If you don't want the label to appear next to the radio-button, you can use [aria-label](https://www.w3.org/TR/wai-aria/states_and_properties#aria-label) or [aria-labelledby](https://www.w3.org/TR/wai-aria/states_and_properties#aria-labelledby) to specify an appropriate label.

## 单选按钮组

> Radio groups

radio-button通常放置在<md-radio-group>中，除非DOM接口不允许（例如，放置在表格单元中）。radio-group有一个value属性，对应当前组内选定的radio-button。

> Radio-buttons should typically be placed inside of an <md-radio-group> unless the DOM structure would make that impossible (e.g., radio-buttons inside of table cells). The radio-group has a value property that reflects the currently selected radio-button inside of the group.

在同一个radio-group中的radio-button将继承单选按钮组的name。

> Individual radio-buttons inside of a radio-group will inherit the name of the group.

## 与@angular/forms一起使用

> Use with @angular/forms

<md-radio-group>兼容@angular/form，并且支持FormsModule和ReactiveFormsModule。

> <md-radio-group> is compatible with @angular/forms and supports both FormsModule and ReactiveFormsModule.

# API

## 单选按钮(radio)的API参考 

> API reference for Angular Material radio

```typescript
import {MdRadioModule} from '@angular/material';
```

## 指令

> Directives

### MdRadioGroup

一组单选按钮。可能包括一个或多个<md-radio-button>元素。

> A group of radio buttons. May contain one or more <md-radio-button> elements.

Selector: md-radio-group

**属性**

> Properties

属性名|描述|Description
-|-|-
@Output()<br>change|当单选按钮组的值发生改变时的触发事件。只有当用户与单选按钮交互导致值改变时才会触发事件。（与<input type="radio">的行为一致）|Event emitted when the group value changes. Change events are only emitted when the value changes due to user interaction with a radio button (the same behavior as <input type="radio">).
@Input()<br>name|单选按钮组的名称。其中所有的单选按钮都使用这个name。|Name of the radio button group. All radio buttons inside this group will use this name.
@Input()<br>Deprecated<br>align|相对于标签，单选按钮的放置位置。可为'before'或者'after'。|Alignment of the radio-buttons relative to their labels. Can be 'before' or 'after'.
@Input()<br>labelPosition|标签显示在单选按钮的前面还是后面。默认是'after'。|Whether the labels should appear after or before the radio-buttons. Defaults to 'after'
@Input()<br>value|单选按钮的value值。|Value of the radio button.
@Input()<br>selected|单选按钮是否已选择。|Whether the radio button is selected.
@Input()<br>disabled|单选按钮组是否失效。|Whether the radio group is diabled

### MdRadioButton

一个单选按钮。

> A radio-button. May be inside of

Selector: md-radio-button

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>id|单选按钮的唯一id。|The unique ID for the radio button.
@Input()<br>name|模拟HTML的name属性，用来分组单选按钮。|Analog to HTML 'name' attribute used to group radios for unique selection.
@Input('aria-label')<br>ariaLabel|用来设置下属input元素的'aria-label'属性。|Used to set the 'aria-label' attribute on the underlying input element.
@Input('aria-labelledby')<br>ariaLabelledby|'aria-labelledby'属性，比元素文本的优先级高。|The 'aria-labelledby' attribute takes precedence as the element's text alternative.
@Input()<br>disableRipple|设置单选按钮的水波效果是否失效。|Whether the ripple effect for this radio button is disabled.
@Input()<br>checked|单选按钮是否选中。|Whether this radio button is checked.
@Input()<br>value|单选按钮的值。|The value of this radio button.
@Input()<br>Deprecated<br>align|单选按钮显示在标签的前面还是后面。|Whether or not the radio-button should appear before or after the label.
@Input()<br>labelPosition|标签显示在单选按钮的前面还是后面。默认是'after'|Whether the label should appear after or before the radio button. Defaults to 'after'
@Input()<br>disabled|单选按钮是否失效。|Whether the radio button is disabled.
@Output()<br>change|当单选按钮是否选择发生改变时的触发事件。只有当用户与单选按钮交互导致值改变时才会触发事件。（与<input type="radio">的行为一致）|Event emitted when the checked state of this radio button changes. Change events are only emitted when the value changes due to user interaction with the radio button (the same behavior as <input type="radio">).
radioGroup|父单元按钮组。可能存在也可能不存在。|The parent radio group. May or may not be present.
inputId|<md-radio-button>内的原生input元素的id。|ID of the native input element inside <md-radio-button>

**方法**

> Methods

方法名|描述|Description
-|-|-
focus|使单选按钮获得焦点。|Focuses the radio button.

## 附加类 

> Additional classes

### MdRadioChange

由MdRadio和MdRadioGroup触发的change事件对象。

> Change event object emitted by MdRadio and MdRadioGroup.

**属性**

> Properties

属性名|描述|Description
-|-|-
source|触发此事件的MdRadioButton。|The MdRadioButton that emits the change event.
value|MdRadioButton的值。|The value of the MdRadioButton.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*