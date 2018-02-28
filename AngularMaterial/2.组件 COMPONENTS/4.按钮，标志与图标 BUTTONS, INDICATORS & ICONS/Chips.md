[toc]

# 概述

> OVERVIEW

`<mat-chip-list>`展示单独的、键盘可访问的、小圆片状的一系列值。

> `<mat-chip-list>` displays a list of values as individual, keyboard accessible, chips.

```html
<mat-chip-list>
  <mat-chip>Papadum</mat-chip>
  <mat-chip>Naan</mat-chip>
  <mat-chip>Dal</mat-chip>
</mat-chip-list>
```

## 非样式化的chips

> Unstyled chips

`<mat-chip>`会默认应用Material Design样式。使用`<mat-basic-chip>`可使一个chip不应用样式。然后你可以通过添加自己的CSS来自定义样式。

> By default, `<mat-chip>` has Material Design styles applied. For a chip with no styles applied, use `<mat-basic-chip>`. You can then customize the chip appearance by adding your own CSS.

提示：除`mat-chip`类外，`<mat-basic-chip>`也接收`mat-basic-chip`CSS类。

> Hint: `<mat-basic-chip>` receives the `mat-basic-chip` CSS class in addition to the `mat-chip` class.

## 选择

> Selection

可通过`selected`属性选择chips。也可在`<mat-chip-list>`中设置`selectable`为`false`来失效选择功能。

> Chips can be selected via the `selected` property. Selection can be disabled by setting `selectable` to `false` on the `<mat-chip-list>`.

当选择状态发生改变时，将通过`(selectionChange)`传递一个`ChipSelectionChange`事件。

> Whenever the selection state changes, a `ChipSelectionChange` event will be emitted via `(selectionChange)`.

## 失效chips

> Disabled chips

可通过设置`disabled`属性来失效单个的chips。当为失效时，chips不可被选择也不可获取焦点。目前失效的chips不接收指定的样式。

> Individual chips may be disabled by applying the `disabled` attribute to the chip. When disabled, chips are neither selectable nor focusable. Currently, disabled chips receive no special styling.

## chip输入

> Chip input

`MatChipInput`指令可与chip-list一起使用，来组织两个组件之间的交互。此指令在`<mat-form-field>`元素中添加chip特定的行为，来添加或删除chips。使用`MatChipInput`的`<input>`可放置于chip-list元素的内部或外部。

> The `MatChipInput` directive can be used together with a chip-list to streamline the interaction between the two components. This directive adds chip-specific behaviors to the input element within `<mat-form-field>` for adding and removing chips. The `<input>` with `MatChipInput` can be placed inside or outside the chip-list element.

## 键盘交互

> Keyboard interaction

用户可通过方向键在chips之间移动，通过空格键来选择或反选。当被点击时，chips将获得焦点，确保键盘导航在正确的chip开始。

> Users can move through the chips using the arrow keys and select/deselect them with the space. Chips also gain focus when clicked, ensuring keyboard navigation starts at the appropriate chip.

## 方向

> Orientation

如果你希望列表中的chips不是水平排列，而是垂直排列，你可以应用`mat-chip-list-stacked`类，并设置`aria-orientation="vertical"`属性：

> If you want the chips in the list to be stacked vertically, instead of horizontally, you can apply the `mat-chip-list-stacked` class, as well as the `aria-orientation="vertical"` attribute:

```html
<mat-chip-list class="mat-chip-list-stacked" aria-orientation="vertical">
  <mat-chip>Papadum</mat-chip>
  <mat-chip>Naan</mat-chip>
  <mat-chip>Dal</mat-chip>
</mat-chip-list>
```

## 主题

> Theming

可使用`color`属性来改变`<mat-chip>`的选中颜色。chips默认使用基于当前主题（light或dark）的背景色。可变为`'primary'`, `'accent'`或者`'warn'`。

> The selected color of an `<mat-chip>` can be changed by using the `color` property. By default, chips use a neutral background color based on the current theme (light or dark). This can be changed to `'primary'`, `'accent'`, or `'warn'`.

## 无障碍

> Accessibility

chip-list的行为类似`role="listbox"`，每一个chip为`role="option"`。chip输入应当有一个占位符，或者通过`aria-label`或`aria-labelledby`指定一个有意义的标签。

> A chip-list behaves as a `role="listbox"`, with each chip being a `role="option"`. The chip input should have a placeholder or be given a meaningful label via `aria-label` or `aria-labelledby`.

# API

## chips的API参考

> API reference for Angular Material chips

```typescript
import {MatChipsModule} from '@angular/material/chips';
```

## 指令

> Directives

### MatChipList

一个material design chips组件（因为与List组件相似，命名为ChipList）。

> A material design chips component (named ChipList for it's similarity to the List component).

Selector: `mat-chip-list`

Exported as: `matChipList`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('aria-orientation')<br>ariaOrientation: 'horizontal' \| 'vertical'|chip列表的方向。|Orientation of the chip list.
@Input()<br>compareWith: (o1: any, o2: any) => boolean|用于比对备选值与已选值的函数。第一个参数为备选值，第二个参数为选中的值。返回为布尔值。|A function to compare the option values with the selected values. The first argument is a value from an option. The second is a value from the selection. A boolean should be returned.
@Input()<br>errorStateMatcher: ErrorStateMatcher|用于控制是否展示错误信息的对象。|An object used to control when error messages are shown.
@Input()<br>multiple: boolean|是否允许用户选择多个chips。|Whether the user should be allowed to select multiple chips.
@Input()<br>selectable: boolean|chip是否可选。当为不可选时，将总是忽略选中状态。|Whether or not this chip is selectable. When a chip is not selectable, its selected state is always ignored.
@Output()<br>change: EventEmitter<MatChipListChange\>|当用户改变chip列表值时传递的事件。|Event emitted when the selected chip list value has been changed by the user.
chipBlurChanges: Observable<MatChipEvent\>|所有子chips失去焦点事件的合并流。|Combined stream of all of the child chips' blur change events.
chipFocusChanges: Observable<MatChipEvent\>|所有子chips获得焦点事件的合并流。|Combined stream of all of the child chips' focus change events.
chipRemoveChanges: Observable<MatChipEvent\>|所有子chips删除事件的合并流。|Combined stream of all of the child chips' remove change events.
chipSelectionChanges: Observable<MatChipSelectionChange\>|所有子chips选中事件的合并流。|Combined stream of all of the child chips' selection change events.
chips: QueryList<MatChip\>|chip列表中的组件。|The chip components contained within this chip list.
errorState: boolean|控件是否为错误状态。|Whether the control is in an error state.
focused: boolean|当前chip-list中的chips或matChipInput是否获取焦点。|Whether any chips or the matChipInput inside of this chip-list has focus.
role: string \| null|chip列表应用的ARIA角色。|The ARIA role applied to the chip list.
selected: MatChip[] \| MatChip|chip列表内的选中的chips数组。|The array of selected chips inside chip list.
stateChanges: Observable<void\>|当控件状态改变时，父MatFormField需要运行改变检测而传递的流。|Stream that emits whenever the state of the control changes such that the parent MatFormField needs to run change detection.

**方法**

> Methods

focus
---
- 描述
  - 使chip列表中第一个非失效的chip获得焦点，或者当没有合适的chip时关联输入。
  - Focuses the the first non-disabled chip in this chip list, or the associated input when there are no eligible chips.

registerInput
---
- 描述
  - 将chip列表与HTML input元素关联。
  - Associates an HTML input element with this chip list.
- 参数
  - inputElement: MatChipInput

updateErrorState
---

### MatChip

Material design样式的Chip组件。在MatChipList组件内部使用。

> Material design styled Chip component. Used inside the MatChipList component.

Selector: `mat-basic-chip` `[mat-basic-chip]` `mat-chip` `[mat-chip]`

Exported as: `matChip`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>color: ThemePalette|组件的主题色调。|Theme color palette for the component.
@Input()<br>disabled: boolean|此组件是否失效。|Whether the component is disabled.
@Input()<br>removable: boolean|此chip是否移除样式与事件。|Determines whether or not the chip displays the remove styling and emits (remove) events.
@Input()<br>selectable: boolean|此chip是否可选。当不可选时，将忽略选中状态。|Whether or not the chips are selectable. When a chip is not selectable, changes to it's selected state are always ignored.
@Input()<br>selected: boolean|此chip是否被选中。|Whether the chip is selected.
@Input()<br>value: any|chip的值。默认为<mat-chip\>标签内的内容。|The value of the chip. Defaults to the content inside <mat-chip\> tags.
@Output()<br>destroyed: EventEmitter<MatChipEvent\>|当chip被销毁时的事件。|Emitted when the chip is destroyed.
@Output()<br>removed: EventEmitter<MatChipEvent\>|当chip被移除时的事件。|Emitted when a chip is to be removed.
@Output()<br>selectionChange: EventEmitter<MatChipSelectionChange\>|当chip被选择或反选时的事件。|Emitted when the chip is selected or deselected.
ariaSelected: string \| null|被选择应用到chip上的ARIA。|The ARIA selected applied to the chip.

**方法**

> Methods

deselect
---
- 描述
  - 反选此chip。
  - Deselects the chip.

focus
---
- 描述
  - 使此chip获得焦点。
  - Allows for programmatic focusing of the chip.

remove
---
- 描述
  - 移除此chip。当按下DELETE或BACKSPACE时，由MatChipList调用。将此移除请求通知所有的监听者。但并不会将此chip从DOM中移除。
  - Allows for programmatic removal of the chip. Called by the MatChipList when the DELETE or BACKSPACE keys are pressed. Informs any listeners of the removal request. Does not remove the chip from the DOM.

select
---
- 描述
  - 选中此chip。
  - Selects the chip.

selectViaInteraction
---
- 描述
  - 选中此chip并触发选中事件。
  - Select this chip and emit selected event

toggleSelected
---
- 描述
  - 切换此chip的选中状态。
  - Toggles the current selected state of this chip.
- 参数
  - isUserInput: boolean = false	
- 返回
  - boolean

### MatChipRemove

使用Material Design的取消图标（[https://material.io/icons/#ic_cancel](https://material.io/icons/#ic_cancel)），并支持点击和添加样式。

> Applies proper (click) support and adds styling for use with the Material Design "cancel" icon available at [https://material.io/icons/#ic_cancel](https://material.io/icons/#ic_cancel).

样例：

> Example:

```html
<mat-chip>
  <mat-icon matChipRemove>cancel</mat-icon>
</mat-chip>
```

你可以使用自定义的图标，但可能需要重写mat-chip-remove位置样式来将图标放置与chip的中间。

> You may use a custom icon, but you may need to override the mat-chip-remove positioning styles to properly center the icon within the chip.

Selector: `[matChipRemove]`

### MatChipInput

在<mat-form-field\>内部的一个input元素中添加chip特定行为。可能放置在<mat-chip-list\>的内部或外部。

> Directive that adds chip-specific behaviors to an input element inside <mat-form-field\>. May be placed inside or outside of an <mat-chip-list\>.

Selector: `input[matChipInputFor]`

Exported as: `matChipInput, matChipInputFor`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('matChipInputAddOnBlur')<br>addOnBlur: boolean|当input失去焦点时，是否传递chipEnd事件。|Whether or not the chipEnd event will be emitted when the input is blurred.
@Input('matChipInputFor')<br>chipList: MatChipList|在chip列表中注册input。|Register input for chip list
@Input()<br>placeholder: string|input的占位符文本。|The input's placeholder text.
@Input('matChipInputSeparatorKeyCodes')<br>separatorKeyCodes: number[]|可触发chipEnd事件的按键代码。默认为[ENTER]。|The list of key codes that will trigger a chipEnd event. Defaults to [ENTER].
@Output('matChipInputTokenEnd')<br>chipEnd: EventEmitter<MatChipInputEvent\>|当一个chip被添加的事件。|Emitted when a chip is to be added.
empty: boolean<br>|input是否为空。|Whether the input is empty.
focused: boolean<br>|控件是否获得焦点。|Whether the control is focused.

**方法**

> Methods

focus
---
- 描述
  - 使input获得焦点。
  - Focuses the input.

## 附加类

> Additional classes

### MatChipListChange

当chip列表值发生改变时的事件对象。

> Change event object that is emitted when the chip list value has changed.

**属性**

> Properties

属性名|描述|Description
-|-|-
source: MatChipList|触发此事件的chip列表。|Chip list that emitted the event.
value: any|事件被触发时的chip列表的值。|Value of the chip list when the event was emitted.

### MatChipSelectionChange

MatChip被选中或反选时触发的事件。

> Event object emitted by MatChip when selected or deselected.

**属性**

> Properties

属性名|描述|Description
-|-|-
isUserInput: false|选中改变是否是用户交互导致。|Whether the selection change was a result of a user interaction.
selected: boolean|触发事件时chip的选中状态。|Whether the chip that emitted the event is selected.
source: MatChip|触发事件的chip参考。|Reference to the chip that emitted the event.

## 附加接口

> Additional interfaces

### MatChipEvent

由单个mat-chip触发的事件。

> Represents an event fired on an individual mat-chip.

**属性**

> Properties

属性名|描述|Description
-|-|-
chip: MatChip|触发事件的源chip。|The chip the event was fired on.

### MatChipInputEvent

matChipInput上的input事件。

> Represents an input event on a matChipInput.

**属性**

> Properties

属性名|描述|Description
-|-|-
input: HTMLInputElement|触发事件的原生<input\>元素。|The native <input\> element that the event is being fired for.
value: string|input的值。|The value of the input.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*