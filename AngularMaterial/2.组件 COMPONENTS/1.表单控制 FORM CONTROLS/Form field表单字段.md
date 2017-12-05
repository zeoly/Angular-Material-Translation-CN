[TOC]

#概述

> OVERVIEW

`<mat-form-field>`是一个组件，用于封装多个Angular Material组件，并， 用于常见的文本字段样式，如下划线、浮动标签和提示消息。


> `<mat-form-field>` is a component used to wrap several Angular Material components and apply common Text field styles such as the underline, floating label, and hint messages.

在这个文档中，“form field”是指封装器组件的`<mat-form-field>`， “form field control”是指`<mat-form-field>`封装的组件(例如input、textarea、select等)。

> In this document, "form field" refers to the wrapper component `<mat-form-field>` and "form field control" refers to the component that the `<mat-form-field>` is wrapping (e.g. the input, textarea, select, etc.)

下面的Angular Material组件被设计成在一个`<mat-form-field>`中工作：

> The following Angular Material components are designed to work inside a `<mat-form-field>`:

- `<input matInput>` & `<textarea matInput>`
- `<mat-select>`
- `<mat-chip-list>`

## 浮动占位符

> Floating placeholder

浮动占位符是一个文本标签，它在控件不包含任何文本时会显示在表单域控件之上。在默认情况下，当文本出现时，浮动占位符就会浮动在表单控件上方。

> The floating placeholder is a text label displayed on top of the form field control when the control does not contain any text. By default, when text is present the floating placeholder floats above the form field control.

我们可以在表单字段控件上使用`placeholder`属性指定占位符文本，或者在表单字段中添加一个`< mat-placeholder >`元素。应该只使用其中的一个选项，指定两者都将引发错误。

> Placeholder text can be specified using the `placeholder` property on the form field control, or by adding a `<mat-placeholder>` element inside the form field. Only one of these options should be used, specifying both will raise an error.

如果表单字段控件以`required`属性标记，则将在占位符后面添加一个星号，以指示它是必需字段的事实。如果不需要，可以通过在`<mat-form-field>`上设置`hideRequiredMarker`属性来禁用该属性。

> If the form field control is marked with a `required` attribute, an asterisk will be appended to the placeholder to indicate the fact that it is a required field. If unwanted, this can be disabled by setting the `hideRequiredMarker` property on `<mat-form-field>`

`<mat-form-field>`的`floatPlaceholder`属性可以用来改变默认的浮动行为。它可以设置为`永远不`隐藏占位符，而不是在文本出现在表单字段控件时将其浮动。即使在表单字段控件中没有文本的情况下，它也可以设置为`总是`浮动占位符。它还可以设置为'自动'来恢复默认行为。

> The `floatPlaceholder` property of `<mat-form-field>` can be used to change this default floating behavior. It can set to `never` to hide the placeholder instead of float it when text is present in the form field control. It can be set to `always` to float the placeholder even when no text is present in the form field control. It can also be set to `auto` to restore the default behavior.

全局默认占位符选项可以通过在应用程序的根模块中为`MAT_PLACEHOLDER_GLOBAL_OPTIONS`提供一个值来指定。就像属性一样，全局设置也可以是`always`,`never`，或`auto`。

> Global default placeholder options can be specified by setting providing a value for `MAT_PLACEHOLDER_GLOBAL_OPTIONS` in your application's root module. Like the property, the global setting can be either `always`, `never`, or `auto`.

```typescript
@NgModule({
  providers: [
    {provide: MAT_PLACEHOLDER_GLOBAL_OPTIONS, useValue: {float: 'always'}}
  ]
})
```

## 提示标签

> Hint labels

Hint labels are additional descriptive text that appears below the form field's underline. A `<mat-form-field>` can have up to two hint labels; one start-aligned (left in an LTR language, right in RTL), and one end-aligned.

Hint labels are specified in one of two ways: either by using the hintLabel property of `<mat-form-field>`, or by adding a <mat-hint> element inside the form field. When adding a hint via the hintLabel property, it will be treated as the start hint. Hints added via the <mat-hint> hint element can be added to either side by setting the align property on <mat-hint> to either start or end. Attempting to add multiple hints to the same side will raise an error.

Form field with hints



Enter some input
Enter some input
Max 10 characters
0/10
 	
 Select me
Here's the dropdown arrow ^
 Error messages

Error messages can be shown under the form field underline by adding mat-error elements inside the form field. Errors are hidden initially and will be displayed on invalid form fields after the user has interacted with the element or the parent form has been submitted. Since the errors occupy the same space as the hints, the hints are hidden when the errors are shown.

If a form field can have more than one error state, it is up to the consumer to toggle which messages should be displayed. This can be done with CSS, ngIf or ngSwitch. Multiple error messages can be shown at the same time if desired, but the `<mat-form-field>` only reserves enough space to display one error message at a time. Ensuring that enough space is available to display multiple errors is up to the user.

Form field with error messages



Enter your email
Enter your email *
 Prefix & suffix

Custom content can be included before and after the input tag, as a prefix or suffix. It will be included within the visual container that wraps the form control as per the Material specification.

Adding the matPrefix directive to an element inside the `<mat-form-field>` will designate it as the prefix. Similarly, adding matSuffix will designate it as the suffix.

Form field with prefix & suffix



Enter your password
Enter your password
visibility
$ 

Amount
Amount
.00
 Custom form field controls

In addition to the form field controls that Angular Material provides, it is possible to create custom form field controls that work with `<mat-form-field>` in the same way. For additional information on this see the guide on Creating Custom mat-form-field Controls.

 Theming

`<mat-form-field>` has a color property which can be set to primary, accent, or warn. This will set the color of the form field underline and floating placeholder based on the theme colors of your app.

`<mat-form-field>` inherits its font-size from its parent element. This can be overridden to an explicit size using CSS. We recommend a specificity of at least 1 element + 1 class.

mat-form-field.mat-form-field {
  font-size: 16px;
}
Form field theming


Primary	
 Color

16
Font size (px)
 Accessibility

If a floating placeholder is specified, it will be automatically used as the label for the form field control. If no floating placeholder is specified, the user should label the form field control themselves using aria-label, aria-labelledby or <label for=...>.

Any errors and hints added to the form field are automatically added to the form field control's aria-describedby set.

 Troubleshooting

 Error: Placeholder attribute and child element were both specified

This error occurs when you have specified two conflicting placeholders. Make sure that you haven't included both a placeholder property on your form field control and a <mat-placeholder> element.

 Error: A hint was already declared for align="..."

This error occurs if you have added multiple hints for the same side. Keep in mind that the hintLabel property adds a hint to the start side.

 Error: mat-form-field must contain a MatFormFieldControl

This error occurs when you have not added a form field control to your form field. If your form field contains a native <input> or <textarea> element, make sure you've added the matInput directive to it. Other components that can act as a form field control include <mat-select>, <mat-chip-list>, and any custom form field controls you've created.




API reference for Angular Material form-field

import {MatFormFieldModule} from '@angular/material/form-field';
 Directives

 MatError
Single error message to be shown underneath the form field.

Selector: mat-error

Properties

Name	Description
@Input()
id: string
 MatFormField
Container for form controls that applies Material Design styling and behavior.

Selector: mat-input-container mat-form-field

Exported as: matFormField
Properties

Name	Description
@Input()
color: 'primary' | 'accent' | 'warn'
Color of the form field underline, based on the theme.

@Input()
floatPlaceholder: FloatPlaceholderType
Whether the placeholder should always float, never float or float as the user types.

@Input()
hideRequiredMarker: any
Whether the required marker should be hidden.

@Input()
hintLabel: string
Text for the form field hint.

underlineRef: ElementRef
Reference to the form field's underline element.

@Input()
Deprecated
dividerColor: 'primary' | 'accent' | 'warn'
 MatHint
Hint text to be shown underneath the form field control.

Selector: mat-hint

Properties

Name	Description
@Input()
align: 'start' | 'end'
Whether to align the hint label at the start or end of the line.

@Input()
id: string
Unique ID for the hint. Used for the aria-describedby on the form field control.

 MatPlaceholder
The floating placeholder for an MatFormField.

Selector: mat-placeholder

 MatPrefix
Prefix to be placed the the front of the form field.

Selector: [matPrefix]

 MatSuffix
Suffix to be placed at the end of the form field.

Selector: [matSuffix]

 Additional classes

 MatFormFieldControl
An interface which allows a control to work inside of a MatFormField.

Properties

Name	Description
controlType: string
An optional name for the control type that can be used to distinguish mat-form-field elements based on their control type. The form field will add a class, mat-form-field-type-{{controlType}} to its root element.

disabled: boolean
Whether the control is disabled.

empty: boolean
Whether the control is empty.

errorState: boolean
Whether the control is in an error state.

focused: boolean
Whether the control is focused.

id: string
The element ID for this control.

ngControl: NgControl | null
Gets the NgControl for this control.

placeholder: string
The placeholder for this control.

required: boolean
Whether the control is required.

shouldPlaceholderFloat: boolean
Whether the MatFormField label should try to float.

stateChanges: Observable<void>
Stream that emits whenever the state of the control changes such that the parent MatFormField needs to run change detection.

value: T | null
The value of the control.

Methods

onContainerClick
Handles a click on the control's container.

Parameters
event

MouseEvent	
setDescribedByIds
Sets the list of element IDs that currently describe this control.

Parameters
ids

string[]