[toc]

# 概述

> OVERVIEW

自动完成是一个包含建议备选项的普通的文本输入框的扩展。你可以在[Material Design spec](https://material.io/guidelines/components/text-fields.html#text-fields-auto-complete-text-field)了解到更多关于自动完成。

> The autocomplete is a normal text input enhanced by a panel of suggested options. You can read more about autocompletes in the [Material Design spec](https://material.io/guidelines/components/text-fields.html#text-fields-auto-complete-text-field).

## 简单的自动完成

> Simple autocomplete

我们从添加一个`matInput`到模板中开始，假定使用`ReactiveFormsModule`模块中的`fromControl`指令来跟踪输入的值。

> Start by adding a regular `matInput` to your template. Let's assume you're using the `formControl` directive from `ReactiveFormsModule` to track the value of the input.

注意：如果你愿意，可以使用模板驱动表单来替代。在此例中我们使用响应式表单，是因为这样做，订阅输入框值的变化非常容易。在这个例子中，在你的`NgModule`中，引入`@angular/forms`中的`ReactiveFormsModule`模块。如果你对使用响应式表单不熟悉，可以在[Angular文档](https://angular.io/guide/reactive-forms)中阅读关于这个主题的相关内容。

> Note: It is possible to use template-driven forms instead, if you prefer. We use reactive forms in this example because it makes subscribing to changes in the input's value easy. For this example, be sure to import `ReactiveFormsModule` from `@angular/forms` into your `NgModule`. If you are unfamiliar with using reactive forms, you can read more about the subject in the [Angular documentation](https://angular.io/guide/reactive-forms).

*my-comp.html*
```html
<mat-form-field>
   <input type="text" matInput [formControl]="myControl">
</mat-form-field>
```

接下来，创建自动完成面板和其中展示的备选项。每一个备选项由一个`mat-option`标签定义。备选项的值可以设置为任何值。

> Next, create the autocomplete panel and the options displayed inside it. Each option should be defined by an `mat-option` tag. Set each option's value property to whatever you'd like the value of the text input to be upon that option's selection.

*my-comp.html*
```html
<mat-autocomplete>
   <mat-option *ngFor="let option of options" [value]="option">
      {{ option }}
   </mat-option>
</mat-autocomplete>
```

现在我们需要将文字输入框与其面板关联起来。我们可以将自动完成面板的实例导出到一个本地模板变量（我们定义其为“auto”），并把这个变量绑定到输入框的`matAutocomplete`属性。

>Now we'll need to link the text input to its panel. We can do this by exporting the autocomplete panel instance into a local template variable (here we called it "auto"), and binding that variable to the input's `matAutocomplete` property.

*my-comp.html*
```html
<mat-form-field>
   <input type="text" matInput [formControl]="myControl" [matAutocomplete]="auto">
</mat-form-field>

<mat-autocomplete #auto="matAutocomplete">
   <mat-option *ngFor="let option of options" [value]="option">
      {{ option }}
   </mat-option>
</mat-autocomplete>
```

## 添加自定义过滤器

> Adding a custom filter

现在，自动完成面板可以通过焦点切换，也可以选择它的备选项。但如果我们想在输入的同时过滤备选项，那么就需要添加一个自定义过滤器。

> At this point, the autocomplete panel should be toggleable on focus and options should be selectable. But if we want our options to filter when we type, we need to add a custom filter.

你可以在文本输入框\*上用任何方式过滤备选项。我们用一个简单的字符串测试来看看，备选项的值是否能从第一个字母开始匹配输入值。我们已经通过`FormControl`获得了内置的可观察值`valueChanges`，然后我们可以通过这个过滤器传递文本框输入值，并将其与备选项进行匹配。通过使用`async`管道，可以将可观察的结果（`filteredOptions`）添加模板中代替`options`属性。

> You can filter the options in any way you like based on the text input\*. Here we will perform a simple string test on the option value to see if it matches the input value, starting from the option's first letter. We already have access to the built-in `valueChanges` observable on the `FormControl`, so we can simply map the text input's values to the suggested options by passing them through this filter. The resulting observable (`filteredOptions`) can be added to the template in place of the `options` property using the `async` pipe.

接下来我们同样用`null`值填充值变化流，以便用初始值（在有任何值变化之前）来过滤备选项。

> Below we are also priming our value change stream with `null` so that the options are filtered by that value on init (before there are any value changes).

\*为了达到最佳的访问体验，我们可能考虑在页面中添加相应的文字说明来解释过滤器规则。假设你使用了一个不限制从头匹配的非标准的过滤器，这种对于屏幕阅读使用者来说特别有帮助。

> *For optimal accessibility, you may want to consider adding text guidance on the page to explain filter criteria. This is especially helpful for screenreader users if you're using a non-standard filter that doesn't limit matches to the beginning of the string.

## 设置分离的控制值与展示值

> Setting separate control and display values

如果你想让备选项控制值（存储在表单中的）与展示值（文本框中展示的）不同，则需要在自动完成元素中设置`displayWith`属性。一个常见的使用场景是，如果想将数据作为一个对象保存，但是展示的只是备选项的一个字符串属性。

> If you want the option's control value (what is saved in the form) to be different than the option's display value (what is displayed in the actual text field), you'll need to set the `displayWith` property on your autocomplete element. A common use case for this might be if you want to save your data as an object, but display just one of the option's string properties.

为了使其起效，在组件类中创建一个函数，功能是将控制值映射到期望的显示值上。然后将其绑定到自动完成的`displayWith`属性。

> To make this work, create a function on your component class that maps the control value to the desired display value. Then bind it to the autocomplete's `displayWith` property.

## 键盘交互

> Keyboard interaction

* 向下：激活下一个备选项。
* 向上：激活上一个备选项。
* 回车：选择当前激活项。

> * DOWN_ARROW: Next option becomes active.
> * UP_ARROW: Previous option becomes active.
> * ENTER: Select currently active item.

## 备选项组

> Option groups

可以通过使用`mat-optgroup`元素将`mat-option`分组。

`mat-option` can be collected into groups using the `mat-optgroup` element:

```html
<mat-autocomplete #auto="matAutocomplete">
  <mat-optgroup *ngFor="let group of filteredGroups | async" [label]="group.name">
    <mat-option *ngFor="let option of group.options" [value]="option">
      {{ option.name }}
    </mat-option>
  </mat-optgroup>
</mat-autocomplete>
```

## 可接入性

> Accessibility

自动完成中的输入框如果没有文本和标签，需要通过`aria-label`或者`aria-labelledby`来指定一个有意义的标签。
`
The input for autocomplete without text or labels should be given a meaningful label via `aria-label` or `aria-labelledby`.

指定`role="combobox"`设置自动完成的触发器。此触发器设置`aria-owns`为自动完成的id，设置`aria-activedescendant`为当前备选项的id。

Autocomplete trigger is given `role="combobox"`. The trigger sets `aria-owns` to the autocomplete's id, and sets `aria-activedescendant` to the active option's id.

# API

## 自动完成(autocomplete)的API参考

> API reference for Angular Material autocomplete

```typescript
import {MatAutocompleteModule} from '@angular/material/autocomplete';
```

## 指令

> Directives

### MdAutocomplete

Selector: `mat-autocomplete`

导出属性: `matAutocomplete`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('class')<br>classList: string|在mat-autocomplete元素中加入这些类，并应用到覆盖容器内的面板中来允许一些简单的样式。|Takes classes set on the host mat-autocomplete element and applies them to the panel inside the overlay container to allow for easy styling.
@Input()<br>displayWith: ((value: any) => string) \| null|将一个备选项的控制值匹配到显示值的触发器函数。|Function that maps an option's control value to its display value in the trigger.
@Output()<br>optionSelected: EventEmitter<MatAutocompleteSelectedEvent>|当列表中的一个备选项被选中时传递出的事件。|Event that is emitted whenever an option from the list is selected.
id: string|提供给自动完成触发器"aria-owns"属性使用的唯一id。|Unique ID to be used by autocomplete trigger's "aria-owns" property.
isOpen: boolean|自动完成面板是否为打开状态。|Whether the autocomplete panel is open.
panel: ElementRef|包含自动完成备选项的面板元素。|Element for the panel containing the autocomplete options.
showPanel: false|自动完成面板是否显示，依赖于备选项的长度。|Whether the autocomplete panel should be visible, depending on option length.

### MdAutocompleteTrigger

Selector: `input[matAutocomplete]` `textarea[matAutocomplete]`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('matAutocomplete')<br>autocomplete: MatAutocomplete||
activeOption: MatOption \| null|当前激活的备选项，强制为MatOption类型。|The currently active option, coerced to MatOption type.
optionSelections: Observable<MatOptionSelectionChange>|自动完成备选项选择流。|Stream of autocomplete option selections.
panelClosingActions: Observable<MatOptionSelectionChange>|关闭自动完成面板的动作流，包括备选项的选择，失去焦点，按TAB按钮。|A stream of actions that should close the autocomplete panel, including when an option is selected, on blur, and when TAB is pressed.
panelOpen: boolean||

**方法**

> Methods

方法名|描述|Description
-|-|-
openPanel|打开自动完成的建议面板|Opens the autocomplete suggestion panel.
closePanel|关闭自动完成的建议面板|Closes the autocomplete suggestion panel.

## 附件类

> Additional classes

### MatAutocompleteSelectedEvent

当一个自动完成的备选项被选择时传递出的事件对象

> Event object that is emitted when an autocomplete option is selected

**属性**

> Properties

属性名|描述|Description
-|-|-
option: MatOption||
source: MatAutocomplete||

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*
