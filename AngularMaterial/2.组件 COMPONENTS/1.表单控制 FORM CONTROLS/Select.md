[toc]

# 概述

> OVERVIEW

`<mat-select>`是用来在选项集合中选择一个值的表单控件,与原生的`<select>`元素类似。你可以在[Material Design spec](https://material.google.com/components/menus.html)中阅读更多关于下拉框的信息。此组件用于[`<mat-form-field>`](https://material.angular.io/components/form-field/overview)元素内。

> `<mat-select>` is a form control for selecting a value from a set of options, similar to the native `<select>` element. You can read more about selects in the [Material Design spec](https://material.google.com/components/menus.html). It is designed to work inside of a [`<mat-form-field>`](https://material.angular.io/components/form-field/overview) element.

在`<mat-select>`中增加`<mat-option>`元素，可在下拉框中增加选项。每个`<mat-option>`都由一个`value`属性，当用户选择一个选项时，这个值可用于设置下拉框的值。`<mat-option>`的内容用于对用户展示。

> To add options to the select, add `<mat-option>` elements to the `<mat-select>`. Each `<mat-option>` has a `value` property that can be used to set the value that will be selected if the user chooses this option. The content of the `<mat-option>` is what will be shown to the user.

```html
<mat-form-field>
  <mat-select placeholder="Favorite food">
    <mat-option *ngFor="let food of foods" [value]="food.value">
      {{ food.viewValue }}
    </mat-option>
  </mat-select>
</mat-form-field>
```

## 获取/设置下拉框的值

> Getting and setting the select value

`<mat-select>`的`value`属性支持双向绑定，无需Angular表单。

> The `<mat-select>` supports 2-way binding to the `value` property without the need for Angular forms.

```html
<mat-form-field>
  <mat-select [(value)]="selected">
    <mat-option>None</mat-option>
    <mat-option value="option1">Option 1</mat-option>
    <mat-option value="option2">Option 2</mat-option>
    <mat-option value="option3">Option 3</mat-option>
  </mat-select>
</mat-form-field>

<p>You selected: {{selected}}</p>
```

`<mat-select>`也支持 `FormsModule` (`NgModel`)和`ReactiveFormsModule` (`FormControl`, `FormGroup`等)当中的所有表单指令。与原生`<select>`一样，`<mat-select>`同样支持`compareWith`函数。（可在[Angular表单文档](https://angular.io/api/forms/SelectControlValueAccessor#caveat-option-selection)中查看更多关于使用自定义`compareWith`函数的信息）

> The `<mat-select>` also supports all of the form directives from the core `FormsModule` (`NgModel`) and `ReactiveFormsModule` (`FormControl`, `FormGroup`, etc.) As with native `<select>`, `<mat-select>` also supports a `compareWith` function. (Additional information about using a custom `compareWith` function can be found in the [Angular forms documentation](https://angular.io/api/forms/SelectControlValueAccessor#caveat-option-selection)).

```html
<form>
  <mat-form-field>
    <mat-select placeholder="Favorite food" [(ngModel)]="selectedValue" name="food">
      <mat-option *ngFor="let food of foods" [value]="food.value">
        {{food.viewValue}}
      </mat-option>
    </mat-select>
  </mat-form-field>

  <p> Selected value: {{selectedValue}} </p>
</form>
```

## 表单字段特性

> Form field features

当`<mat-select>`与`<mat-form-field>`使用时，可使用其中的一些特性。包括错误信息、提示文字、前缀后缀、主题等等。可在[表单字段文档](https://material.angular.io/components/form-field/overview)中查看更多关于特性的信息。

> There are a number of `<mat-form-field>` features that can be used with any `<mat-select>`. These include error messages, hint text, prefix & suffix, and theming. For additional information about these features, see the [form field documentation](https://material.angular.io/components/form-field/overview).

```html
<mat-form-field>
  <mat-select placeholder="Favorite animal" [formControl]="animalControl" required>
    <mat-option>--</mat-option>
    <mat-option *ngFor="let animal of animals" [value]="animal">
      {{animal.name}}
    </mat-option>
  </mat-select>
  <mat-error *ngIf="animalControl.hasError('required')">Please choose an animal</mat-error>
  <mat-hint>{{animalControl.value?.sound}}</mat-hint>
</mat-form-field>
```

## 设置静态占位符

> Setting a static placeholder

占位符是当未选中值时，下拉框触发区域展示的一个文本标签。当选中值时，占位符将浮动在下拉框触发区的上方。可通过`<mat-select>`的`placeholder`属性，或者`<mat-select>`同一表单字段中的`<mat-placeholder>`元素，来指定一个占位符。`<mat-form-field>`同样有多种选项来改变占位符的行为。更多信息请查看[表单字段占位符文档](https://material.angular.io/components/select/overview#floating-placeholder)。

> The placeholder is a text label displayed in the select trigger area when no value is selected. When a value is selected, the placeholder will float above the select trigger area. The placeholder can be specified either via a `placeholder` attribute on the `<mat-select>` or a `<mat-placeholder>` element in the same form field as the `<mat-select>`. The `<mat-form-field>` also has additional options for changing the behavior of the placeholder. For more information see the [form field placeholder documentation](https://material.angular.io/components/select/overview#floating-placeholder).

## 失效下拉框或者单个选项

> Disabling the select or individual options

通过使用`<mat-select>`和`<mat-option>`组件中的`disabled`属性，可以分别失效整个下拉框和某单个选项。

> It is possible to disable the entire select or individual options in the select by using the `disabled` property on the `<mat-select>` and the `<mat-option>` components respectively.

```html
<mat-form-field>
  <mat-select placeholder="Choose an option" [disabled]="disableSelect.value">
    <mat-option value="option1">Option 1</mat-option>
    <mat-option value="option2" disabled>Option 2 (disabled)</mat-option>
    <mat-option value="option3">Option 3</mat-option>
  </mat-select>
</mat-form-field>
```

## 重设下拉框值

> Resetting the select value

如果想用选项之一来重新设置下拉框值，你可以省略设置值的步骤。

> If you want one of your options to reset the select's value, you can omit specifying its value.

```html
<mat-form-field>
  <mat-select placeholder="State">
    <mat-option>None</mat-option>
    <mat-option *ngFor="let state of states" [value]="state">{{state}}</mat-option>
  </mat-select>
</mat-form-field>
```

## 创建选项组

> Creating groups of options

可使用`<mat-optgroup>`元素来在副标题下分组选项。组的名称可通过`<mat-optgroup>`的`label`属性设置。与单个的`<mat-option>`元素类似，整个`<mat-optgroup>`也可以通过`disabled`属性来失效。

> The `<mat-optgroup>` element can be used to group common options under a subheading. The name of the group can be set using the `label` property of `<mat-optgroup>`. Like individual `<mat-option>` elements, an entire `<mat-optgroup>` can be disabled or enabled by setting the `disabled` property on the group.

```html
<mat-form-field>
  <mat-select placeholder="Pokemon" [formControl]="pokemonControl">
    <mat-option>-- None --</mat-option>
    <mat-optgroup *ngFor="let group of pokemonGroups" [label]="group.name"
                  [disabled]="group.disabled">
      <mat-option *ngFor="let pokemon of group.pokemon" [value]="pokemon.value">
        {{ pokemon.viewValue }}
      </mat-option>
    </mat-optgroup>
  </mat-select>
</mat-form-field>
```

## 多选

> Multiple selection

`<mat-select>`默认为单选模式，可通过设置`multiple`属性来启用多选模式。此模式下允许用户一次选择多个值。当`<mat-select>`在多选模式下时，该控件的值为所有已选择值的一个排序列表，而不是单个的值。

> `<mat-select>` defaults to single-selection mode, but can be configured to allow multiple selection by setting the `multiple` property. This will allow the user to select multiple values at once. When using the `<mat-select>` in multiple selection mode, its value will be a sorted list of all selected values rather than a single value.

```html
<mat-form-field>
  <mat-select placeholder="Toppings" [formControl]="toppings" multiple>
    <mat-option *ngFor="let topping of toppingList" [value]="topping">{{topping}}</mat-option>
  </mat-select>
</mat-form-field>
```

## 自定义触发器标签

> Customizing the trigger label

如果想在下拉框内展示一个自定义触发器标签，可以使用`<mat-select-trigger>`元素。

> If you want to display a custom trigger label inside a select, you can use the `<mat-select-trigger>` element.

```html
<mat-form-field>
  <mat-select placeholder="Toppings" [formControl]="toppings" multiple>
    <mat-select-trigger>
      {{toppings.value ? toppings.value[0] : ''}}
      <span *ngIf="toppings.value?.length > 1" class="example-additional-selection">
        (+{{toppings.value.length - 1}} others)
      </span>
    </mat-select-trigger>
    <mat-option *ngFor="let topping of toppingList" [value]="topping">{{topping}}</mat-option>
  </mat-select>
</mat-form-field>
```

## 失效水波效果

> Disabling the ripple effect

默认用户点击一个`<mat-option>`时，会有一个水波动画效果。可通过设置`<mat-select>`的`disableRipple`属性来失效此效果。

> By default, when a user clicks on a `<mat-option>`, a b animation is shown. This can be disabled by setting the `disableRipple` property on `<mat-select>`.

```html
<mat-form-field>
  <mat-select placeholder="Select an option" disableRipple>
    <mat-option value="1">Option 1</mat-option>
    <mat-option value="2">Option 2</mat-option>
    <mat-option value="3">Option 3</mat-option>
  </mat-select>
</mat-form-field>
```

## 在下拉面板中添加自定义样式

> Adding custom styles to the dropdown panel

为了使下拉面板更容易样式化，`<mat-select>`提供了`panelClass`属性，用来在下拉面板上应用附加的CSS类。

> In order to facilitate easily styling the dropdown panel, `<mat-select>` has a `panelClass` property which can be used to apply additional CSS classes to the dropdown panel.

```html
<mat-form-field>
  <mat-select placeholder="Panel color" [formControl]="panelColor"
              panelClass="example-panel-{{panelColor.value}}">
    <mat-option value="red">Red</mat-option>
    <mat-option value="green">Green</mat-option>
    <mat-option value="blue">Blue</mat-option>
  </mat-select>
</mat-form-field>
```

```css
.example-panel-red .mat-select-content {
  background: rgba(255, 0, 0, 0.5);
}

.example-panel-green .mat-select-content {
  background: rgba(0, 255, 0, 0.5);
}

.example-panel-blue .mat-select-content {
  background: rgba(0, 0, 255, 0.5);
}
```

## 当显示错误信息时的变更

> Changing when error messages are shown

`<mat-form-field>`允许你在`<mat-select>`中[关联错误信息](https://material.angular.io/components/select/overview#error-messages)。当控件非法并且用户与元素交互或提交其父表单时，将显示错误信息。如果想复写此行为（例如控件非法或者父表单非法后立即显示），可以使用`<mat-select>`中的`errorStateMatcher`属性。此属性接收一个`ErrorStateMatcher`对象实例。一个`ErrorStateMatcher`必须实现`isErrorState`方法，将取代`<mat-select>`和其父表单的`FormControl`，返回一个表示是否展示错误信息的布尔值。（`true`表示应当展示， `false`表示不展示。）

> The `<mat-form-field>` allows you to [associate error messages](https://material.angular.io/components/select/overview#error-messages) with your `<mat-select>`. By default, these error messages are shown when the control is invalid and either the user has interacted with (touched) the element or the parent form has been submitted. If you wish to override this behavior (e.g. to show the error as soon as the invalid control is dirty or when a parent form group is invalid), you can use the `errorStateMatcher` property of the `<mat-select>`. The property takes an instance of an `ErrorStateMatcher` object. An `ErrorStateMatcher` must implement a single method `isErrorState` which takes the `FormControl` for this `<mat-select>` as well as the parent form and returns a boolean indicating whether errors should be shown. (`true` indicating that they should be shown, and `false` indicating that they should not.)

可通过设置`ErrorStateMatcher`provider来指定一个全局错误状态匹配器。将应用到所有的input上。为了方便使用，提供了`ShowOnDirtyErrorStateMatcher`用于设置全局的当input脏数据或者非法时引发input错误。

> A global error state matcher can be specified by setting the `ErrorStateMatcher` provider. This applies to all inputs. For convenience, `ShowOnDirtyErrorStateMatcher` is available in order to globally cause input errors to show when the input is dirty and invalid.

```ts
@NgModule({
  providers: [
    {provide: ErrorStateMatcher, useClass: ShowOnDirtyErrorStateMatcher}
  ]
})
```

## 键盘交互

> Keyboard interaction:

- 下方向键：焦点移动到下一个选项
- 上方向键：焦点移动到上一个选项
- 回车或空格：选择当前焦点的选项

> - DOWN_ARROW: Focus next option
> - UP_ARROW: Focus previous option
> - ENTER or SPACE: Select focused item

## 无障碍

> Accessibility

没有文本或标签的下拉框组件应当通过`aria-label`或`aria-labelledby`指定一个有意义的标签。

> The select component without text or label should be given a meaningful label via `aria-label` or `aria-labelledby`.

下拉框有`role="listbox"`，其内部的选项有`role="option"`。

> The select component has `role="listbox"` and options inside select have `role="option"`.

## 问题定位

> Troubleshooting

### Error: Cannot change `multiple` mode of select after initialization

当将`<mat-select>`的`multiple`属性绑定在一个变量上是，将出现此错误。（例如设置`[multiple]="isMultiple"`，但`isMultiple`在组件的生命周期中发生了改变）。如果想动态改变，需要使用`ngIf`或者`ngSwitch`。

> This error is thrown if you attempt to bind the `multiple` property on `<mat-select>` to a dynamic value. (e.g. `[multiple]="isMultiple"` where the value of `isMultiple` changes over the course of the component's lifetime). If you need to change this dynamically, use `ngIf` or `ngSwitch` instead:

```html
<mat-select *ngIf="isMultiple" multiple>
  ...
</mat-select>
<mat-select *ngIf="!isMultiple">
  ...
</mat-select>
```

### Error: Value must be an array in multiple-selection mode

当指定`null`, `undefined`或者数组以外的值给`<mat-select multiple>`时将出现此错误。例如`mySelect.value = 'option1'`。应该写为`mySelect.value = ['option1']`。

> This error is thrown if you attempt to assign a value other than `null`, `undefined`, or an array to a `<mat-select multiple>`. For example, something like `mySelect.value = 'option1'`. What you likely meant to do was `mySelect.value = ['option1']`.

### Error: `compareWith` must be a function

当指定函数以外的给`compareWith`属性时将出现此错误。可以查看[Angular表单文档](https://angular.io/api/forms/SelectControlValueAccessor#caveat-option-selection)来获取更多关于使用`compareWith`的信息。

> This error occurs if you attempt to assign something other than a function to the `compareWith` property. For more information on proper usage of `compareWith` see the [Angular forms documentation](https://angular.io/api/forms/SelectControlValueAccessor#caveat-option-selection)).

# API

## 下拉框(select)的API参考

> API reference for Angular Material select

```ts
import {MatSelectModule} from '@angular/material/select';
```

## 指令

> Directives

### MatSelectTrigger

允许用户自定义当下拉框有值时触发器的显示。

> Allows the user to customize the trigger that is displayed when the select has a value.

Selector: `mat-select-trigger`

### MatSelect

Selector: `mat-select`

Exported as: `matSelect`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('aria-label')<br>ariaLabel: string|下拉框的aria标签。如果没有设置，将使用占位符作为标签。|Aria label of the select. If not specified, the placeholder will be used as label.
@Input('aria-labelledby')<br>ariaLabelledby: string|设置aria-labelledby属性。|Input that can be used to specify the aria-labelledby attribute.
@Input()<br>compareWith: (o1: any, o2: any) => boolean|比较备选值与已选值的函数。第一个参数是备选项的值。第二个参数是选中的值。返回值为布尔值。|A function to compare the option values with the selected values. The first argument is a value from an option. The second is a value from the selection. A boolean should be returned.
@Input()<br>disableRipple: boolean|是否失效水波效果。|Whether ripples are disabled.
@Input()<br>disabled: boolean|是否失效组件。|Whether the component is disabled.
@Input()<br>errorStateMatcher: ErrorStateMatcher|控制何时显示错误信息的对象。|An object used to control when error messages are shown.
@Input()<br>id: string|元素的唯一id。|Unique id of the element.
@Input()<br>multiple: boolean|是否允许用户选择多个选项。|Whether the user should be allowed to select multiple options.
@Input()<br>panelClass: string \| string[] \| Set<string\> \| { [key: string]: any; }|传递到下拉框面板的类。支持与ngClass相同的语法。|Classes to be passed to the select panel. Supports the same syntax as ngClass.
@Input()<br>placeholder: string|当没有选定值时显示的占位符。|Placeholder to be shown if no value has been selected.
@Input()<br>required: boolean|组件是否必须。|Whether the component is required.
@Input()<br>value: any|下拉框控件的值。|Value of the select control.
@Output()<br>openedChange: EventEmitter<boolean\>|切换下拉框面板的事件。|Event emitted when the select panel has been toggled.
@Output()<br>selectionChange: EventEmitter<MatSelectChange\>|用户改变选中值的事件。|Event emitted when the selected value has been changed by the user.
controlType: 'mat-select'|用于mat-form-field的控件名。|A name for this control that can be used by mat-form-field.
customTrigger: MatSelectTrigger|可复写的触发器元素。|User-supplied override of the trigger element.
empty: boolean|下拉框是否有值。|Whether the select has a value.
errorState: boolean|控件是否在错误状态。|Whether the control is in an error state.
focused: boolean|下拉框是否获取焦点。|Whether the select is focused.
ngControl: NgControl||
optionGroups: QueryList<MatOptgroup\>|所有定义的选项组。|All of the defined groups of options.
optionSelectionChanges: Observable<MatOptionSelectionChange\>|所有子选项改变事件的合并流。|Combined stream of all of the child options' change events.
options: QueryList<MatOption\>|所有定义的下拉框选项。|All of the defined select options.
overlayDir: CdkConnectedOverlay|包含备选项的叠加层面板。|Overlay pane containing the options.
panel: ElementRef|包含下拉框选项的面板。|Panel containing the select options.
panelOpen: boolean|叠加层面板是否打开。|Whether or not the overlay panel is open.
selected: MatOption \| MatOption[]|当前选择的选项。|The currently selected option.
shouldLabelFloat: boolean|MatFormField的标签是否浮动。|Whether the MatFormField label should try to float.
stateChanges: Observable<void\>|当控件状态变更时的事件，父级MatFormField需要此事件来做变更检测。|Stream that emits whenever the state of the control changes such that the parent MatFormField needs to run change detection.
trigger: ElementRef|打开下拉框的触发器。|Trigger that opens the select.
triggerValue: string|触发器中展示的值。|The value displayed in the trigger.

**方法**

> Methods

close
---
- 描述
  - 关闭叠加层面板，并使其属主元素获得焦点。
  - Closes the overlay panel and focuses the host element.

focus
---
- 描述
  - 使下拉框元素获得焦点
  - Focuses the select element.

open
---
- 描述
  - 打开叠加层面板。
  - Opens the overlay panel.

toggle
---
- 描述
  - 切换叠加层面板的打开或关闭状态。
  - Toggles the overlay panel open or closed.

updateErrorState
---

## 附加类

> Additional classes

### MatSelectChange

当下拉框值改变时传出的改变事件对象。

> Change event object that is emitted when the select value has changed.

**属性**

> Properties

属性名|描述|Description
-|-|-
source: MatSelect|触发此事件的下拉框参考。|Reference to the select that emitted the change event.
value: any|触发此事件的当前的下拉框值。|Current value of the select that emitted the event.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*
