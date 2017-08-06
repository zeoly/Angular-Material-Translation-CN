[toc]

# 概述

> OVERVIEW

<md-input-container>是原生input和textarea元素的封装。此容器使用了Material Design样式与行为，同时允许直接访问其包含的原生元素。

> <md-input-container> is a wrapper for native input and textarea elements. This container applies Material Design styles and behavior while still allowing direct access to the underlying native element.

md-input-container封装的原生元素必须使用mdInput指令进行标记。

> The native element wrapped by the md-input-container must be marked with the mdInput directive.

## input和textarea属性

> input and textarea attributes

一般input与textarea元素可以使用的属性，都可以在md-input-container中使用。还包括Angular指令，例如ngModel和formControl。

> All of the attributes that can be used with normal input and textarea elements can be used on elements inside md-input-container as well. This includes Angular directives such as ngModel and formControl.

唯一的限制是type属性只能为md-input-container支持的其中一个，并且如果md-input-container包含了md-placeholder属性，则原生的元素无法指定placeholder属性。

> The only limitations are that the type attribute can only be one of the values supported by md-input-container and the native element cannot specify a placeholder attribute if the md-input-container also contains a md-placeholder element.

## 支持的input类型

> Supported input types

md-input-container可以使用下列[input类型](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)：

> The following [input types](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input) can be used with md-input-container:

- date
- datetime-local
- email
- month
- number
- password
- search
- tel
- text
- time
- url
- week

## 错误信息

> Error messages

在md-input-container中指定md-error元素，可以在input下方显示错误信息。错误信息默认不显示，当用户与元素有交互后或者父表单提交后，存在无效输入的情况下，将展示错误信息。另外每当错误信息展示时，容器的md-hint标签将会隐藏。

> Error messages can be shown beneath an input by specifying md-error elements inside the md-input-container. Errors are hidden by default and will be displayed on invalid inputs after the user has interacted with the element or the parent form has been submitted. In addition, whenever errors are displayed, the container's md-hint labels will be hidden.

如果input元素有多个错误状态，则由其消费者决定显示哪个错误信息。可使用CSS，ngIf或ngSwitch来解决。

> If an input element can have more than one error state, it is up to the consumer to toggle which messages should be displayed. This can be done with CSS, ngIf or ngSwitch.

注意，当可以同时显示多个错误信息时，建议一次只显示一个。

> Note that, while multiple error messages can be displayed at the same time, it is recommended to only show one at a time.

## 占位符

> Placeholder

占位符是当输入框没有文本时，默认展示的描述性文本。当有文本时，此描述性文本将浮在输入区的上方。

> A placeholder is an indicative text displayed in the input zone when the input does not contain text. When text is present, the indicative text will float above this input zone.

当输入框中有文本输入时，可以设置md-input-container的floatPlaceholder属性为never，来隐藏描述性文本。

> The floatPlaceholder attribute of md-input-container can be set to never to hide the indicative text instead when text is present in the input.

设置floatPlaceholder为always，则浮动标签将一直显示在输入框上方。

> When setting floatPlaceholder to always the floating label will always show above the input.

输入框的占位符有两种设置方法：在input或textarea中使用placeholder属性，或者在md-input-container中使用md-placeholder元素。同时使用两种方法会导致错误。

> A placeholder for the input can be specified in one of two ways: either using the placeholder attribute on the input or textarea, or using an md-placeholder element in the md-input-container. Using both will raise an error.

设置MD_PLACEHOLDER_GLOBAL_OPTIONS provider可以指定全局默认的占位符。此设置将应用到所有支持浮动占位符的组件。

> Global default placeholder options can be specified by setting the MD_PLACEHOLDER_GLOBAL_OPTIONS provider. This setting will apply to all components that support the floating placeholder.

```typescript
@NgModule({
  providers: [
    {provide: MD_PLACEHOLDER_GLOBAL_OPTIONS, useValue: { float: 'always' }}
  ]
})
```

以下是全局设置的可选项：

> Here are the available global options:

名称|类型|可选值|描述
-|-|-|-
float|字符串|auto, always, never|占位符默认浮动行为。The default placeholder float behavior.

## 前缀与后缀

> Prefix and Suffix

HTML可以在input标签的前面或后面，作为前缀或者后缀。点击前缀或者后缀可以使输入框获得焦点。

> HTML can be included before, and after the input tag, as prefix or suffix. It will be underlined as per the Material specification, and clicking it will focus the input.

在md-input-container中的一个元素中添加mdPrefix属性，指定其为前缀。相似的，用mdSuffix执行后缀。

> Adding the mdPrefix attribute to an element inside the md-input-container will designate it as the prefix. Similarly, adding mdSuffix will designate it as the suffix.

## 提示标签

> Hint Labels

提示标签是显示在下划线下的一个标签。一个md-input-container最多可以有两个提示标签；一个在下划线start起始处（），一个在下划线end终止处。

> Hint labels are the labels that show below the underline. An md-input-container can have up to two hint labels; one on the start of the line (left in an LTR language, right in RTL), and one on the end.

有两种方式设置提示标签：使用md-input-container的hintLabel属性，或使用md-input-container中的md-hint元素，并设置其位置的align属性。属性版本假定为start。两次指定其位置将在初始化时导致异常。

> Hint labels are specified in one of two ways: either using the hintLabel attribute of md-input-container, or using an md-hint element inside the md-input-container, which takes an align attribute containing the side. The attribute version is assumed to be at the start. Specifying a side twice will result in an exception during initialization.

## 下划线颜色

> Underline Color

使用md-input-container的color属性，可以改变下划线（输入框内容下方的横线）颜色。primary是默认值，对应主题的主色调。相应的可以设置为accent或者warn，表示使用主题的着重色或警告色。

> The underline (line under the input content) color can be changed by using the color attribute of md-input-container. A value of primary is the default and will correspond to the theme primary color. Alternatively, accent or warn can be specified to use the theme's accent or warn color.

## 自定义错误匹配器

> Custom Error Matcher

默认情况下，当与用户交互的元素或其父表单提交时，且控制为无效状态，将显示错误信息。如果你想重写这个行为（例如当控制为无效或者父表单组非法是立刻显示错误信息），你可以使用mdInput的errorStateMatcher属性。要使用这个属性，需要在组件类中创建一个返回值为布尔的函数。当结果为true时将会展示错误信息。

> By default, error messages are shown when the control is invalid and either the user has interacted with (touched) the element or the parent form has been submitted. If you wish to override this behavior (e.g. to show the error as soon as the invalid control is dirty or when a parent form group is invalid), you can use the errorStateMatcher property of the mdInput. To use this property, create a function in your component class that returns a boolean. A result of true will display the error messages.

```html
<md-input-container>
  <input mdInput [(ngModel)]="myInput" required [errorStateMatcher]="myErrorStateMatcher">
  <md-error>This field is required</md-error>
</md-input-container>
```

```typescript
function myErrorStateMatcher(control: FormControl, form: FormGroupDirective | NgForm): boolean {
  // Error when invalid control is dirty, touched, or submitted
  const isSubmitted = form && form.submitted;
  return !!(control.invalid && (control.dirty || control.touched || isSubmitted)));
}
```

可以通过设置MD_ERROR_GLOBAL_OPTIONS provider来指定一个全局的错误状态匹配器。这样做将会应用到所谓的输入框。为了方便起见，提供了一个showOnDirtyErrorStateMatcher值，当输入为脏数据或者无效时，将显示错误信息。

> A global error state matcher can be specified by setting the MD_ERROR_GLOBAL_OPTIONS provider. This applies to all inputs. For convenience, showOnDirtyErrorStateMatcher is available in order to globally cause input errors to show when the input is dirty and invalid.

```typescript
@NgModule({
  providers: [
    {provide: MD_ERROR_GLOBAL_OPTIONS, useValue: { errorStateMatcher: showOnDirtyErrorStateMatcher }}
  ]
})
```

以下是全局设置的可选项：

> Here are the available global options:

名称|类型|描述|Description
-|-|-|-
errorStateMatcher|函数/Function|返回是否显示错误信息的布尔值。|Returns a boolean specifying if the error should be shown

# API

## 输入框(input)的API参考

> API reference for Angular Material input

```typescript
import {MdInputModule} from '@angular/material';
```

## 指令

> Directives

### MdTextareaAutosize

根据textarea内容自动调整尺寸的指令。

> Directive to automatically resize a textarea to fit its content.

Selector：textarea[mdTextareaAutosize]

导出属性：mdTextareaAutosize

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('mdAutosizeMinRows')<br>minRows||
@Input('mdAutosizeMaxRows')<br>maxRows||

**方法**

> Methods

方法名|描述|Description
-|-|-
resizeToFitContent|调整textarea以适应其中的内容。|Resize the textarea to fit its content.

### MdPlaceholder

占位符指令。声明此指令可以实现更复杂的占位符功能。

> The placeholder directive. The content can declare this to implement more complex placeholders.

Selector: md-placeholder

### MdHint

在输入框下方显示的提示文本。

> Hint text to be shown underneath the input.

Selector: md-hint

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>align|在输入框线的开始还是末尾显示提示标签。|Whether to align the hint label at the start or end of the line.
@Input()<br>id|提示的唯一id。提供给input的aria-describedby使用。|Unique ID for the hint. Used for the aria-describedby on the input.

### MdErrorDirective

在输入框下方显示的单个错误信息。

> Single error message to be shown underneath the input.

Selector: md-error

### MdPrefix

放在输入框开始的前缀。

> Prefix to be placed the the front of the input.

Selector: [mdPrefix] [md-prefix]

### MdSuffix

放在输入框末尾的后缀

> Suffix to be placed at the end of the input.

Selector: [mdSuffix] [md-suffix]

### MdInputDirective

MdInputContainer封装的input元素的标记。

> Marker for the input element that MdInputContainer is wrapping.

Selector: input[mdInput] textarea[mdInput]

**属性**

> Properties

属性名|描述|Description
-|-|-
focused|元素是否获取焦点。|Whether the element is focused or not.
ariaDescribedby|设置输入框的aria-describedby属性。|Sets the aria-describedby attribute on the input for improved a11y.
@Input()<br>disabled|元素是否失效。|Whether the element is disabled.
@Input()<br>id|元素的唯一id。|Unique id of the element.
@Input()<br>placeholder|元素的占位符属性。|Placeholder attribute of the element.
@Input()<br>required|元素是否必需。|Whether the element is required.
@Input()<br>type|元素的input类型|Input type of the element.
@Input()<br>errorStateMatcher|控制错误信息何时展示的函数。|A function used to control when error messages are shown.
value|输入框元素的value。|The input element's value.
empty|输入框是否为空。|Whether the input is empty.

**方法**

> Methods

方法名|描述|Description
-|-|-
focus|使元素获得焦点。|Focuses the input element.

### MdInputContainer

文本输入框的容器，应用Material Design的样式与行为。

> Container for text inputs that applies Material Design styling and behavior.

Selector: md-input-container

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>color|input分割器基于主题的颜色。|Color of the input divider, based on the theme.
@Input()<br>Deprecated<br>dividerColor|分割器颜色。已失效。|
@Input()<br>hideRequiredMarker|必需的标记是否隐藏。|Whether the required marker should be hidden.
@Input()<br>hintLabel|输入框提示文字。|Text for the input hint.
@Input()<br>floatPlaceholder|设置占位符是一直展示、从不展示、或者按用户类型展示。|Whether the placeholder should always float, never float or float as the user types.
underlineRef|输入框下划线元素的参考。|Reference to the input's underline element.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*