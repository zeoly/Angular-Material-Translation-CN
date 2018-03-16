[toc]

# 概述

> OVERVIEW

日期选择器允许用户通过文本输入一个日期，或者通过日历选择一个日期。由多个组件和指令协作构成。

> The datepicker allows users to enter a date either through text input, or by choosing a date from the calendar. It is made up of several components and directives that work together.

## 在input中关联一个日期选择器

> Connecting a datepicker to an input

日期选择器由一个文本输入框和一个弹出日历组成，在文本输入框中通过`matDatepicker`属性关联。

> A datepicker is composed of a text input and a calendar pop-up, connected via the `matDatepicker` property on the text input.

```html
<input [matDatepicker]="myDatepicker">
<mat-datepicker #myDatepicker></mat-datepicker>
```

提供了一个可选的日期选择器切换按钮。可在上述例子中添加一个切换：

> An optional datepicker toggle button is available. A toggle can be added to the example above:

```html
<input [matDatepicker]="myDatepicker">
<mat-datepicker-toggle [for]="myDatepicker"></mat-datepicker-toggle>
<mat-datepicker #myDatepicker></mat-datepicker>
```

日期选择器与input一样，都是`<mat-form-field>`的一部分，并且切换按钮也可以作为material input中的前缀或者后缀：

> This works exactly the same with an input that is part of an `<mat-form-field>` and the toggle can easily be used as a prefix or suffix on the material input:

```html
<mat-form-field>
  <input matInput [matDatepicker]="myDatepicker">
  <mat-datepicker-toggle matSuffix [for]="myDatepicker"></mat-datepicker-toggle>
  <mat-datepicker #myDatepicker></mat-datepicker>
</mat-form-field>
```

如果想自定义`mat-datepicker-toggle`内部渲染的图标，可以使用`matDatepickerToggleIcon`指令。

> If you want to customize the icon that is rendered inside the `mat-datepicker-toggle`, you can do so by using the `matDatepickerToggleIcon` directive:

## 设置日历起始视图

> Setting the calendar starting view

`<mat-datepicker>`的`startView`属性可用于设置当日历第一次打开时显示的视图。可设置为`month`, `year`, 或者`multi-year`；默认打开为月视图。

> The `startView` property of `<mat-datepicker>` can be used to set the view that will show up when the calendar first opens. It can be set to `month`, `year`, or `multi-year`; by default it will open to month view.

日历打开时是什么日期范围取决于初始检查时是否选中某个日期，如果选中则会打开包含该日期的月视图或年视图。否则将打开包含当前日期的月视图或年视图。此行为可通过`<mat-datepicker>`的`startAt`属性进行复写。这样日历将会在包含`startAt`日期的月视图或年视图中打开。

> The month, year, or range of years that the calendar opens to is determined by first checking if any date is currently selected, if so it will open to the month or year containing that date. Otherwise it will open to the month or year containing today's date. This behavior can be overridden by using the `startAt` property of `<mat-datepicker>`. In this case the calendar will open to the month or year containing the `startAt` date.

## 设置选中日期

> Setting the selected date

日期选择器的值类型取决于应用中提供的`DateAdapter`的类型。例如`NativeDateAdapter`直接使用JavaScript原生`Date`对象。使用`MomentDateAdapter`时，值均为Moment.js实例。此适配器模式的用法允许日期选择器组件适配
任意自定义`DateAdapter`的日期。更多信息参考[选择一个日期实现](https://material.angular.io/components/datepicker/overview#choosing-a-date-implementation-and-date-format-settings)。

> The type of values that the datepicker expects depends on the type of `DateAdapter` provided in your application. The `NativeDateAdapter`, for example, works directly with plain JavaScript `Date` objects. When using the `MomentDateAdapter`, however, the values will all be Moment.js instances. This use of the adapter pattern allows the datepicker component to work with any arbitrary date representation with a custom `DateAdapter`. See [Choosing a date implementation](https://material.angular.io/components/datepicker/overview#choosing-a-date-implementation-and-date-format-settings) for more information.

因为使用了`DateAdapter`，日期选择器会自动反序列化常见日期格式。例如对于`NativeDateAdapter`和`MomentDateAdapter`接受[ISO 8601](https://tools.ietf.org/html/rfc3339)字符串作为日期选择器的输入，并自动转换为合适的对象类型。当直接绑定数据到日期选择器时，将会非常方便。但日期选择器不接受用户自定义格式的字符串，例如`"1/2/2017"`，因为这样将引起歧义并且会根据浏览器的区域而表示不同的含义。

> Depending on the `DateAdapter` being used, the datepicker may automatically deserialize certain date formats for you as well. For example, both the `NativeDateAdapter` and `MomentDateAdapter` allow [ISO 8601](https://tools.ietf.org/html/rfc3339) strings to be passed to the datepicker and automatically converted to the proper object type. This can be convenient when binding data directly from your backend to the datepicker. However, the datepicker will not accept date strings formatted in user format such as `"1/2/2017"` as this is ambiguous and will mean different things depending on the locale of the browser running the code.

日期选择器作为`<input>`的其他类型，也适配`@angular/forms`指令，例如`formGroup`, `formControl`, `ngModel`等等。

> As with other types of `<input>`, the datepicker works with `@angular/forms` directives such as `formGroup`, `formControl`, `ngModel`, etc.

## 日期校验

> Date validation

在日期选择器input中有三个属性用来添加日期校验。前两个是`min`和`max`属性。为了在input中强制校验，这两个属性将失效弹出日历中不在范围内的所有日期，并防止用户前进到包含`min`和`max`日期的`month`或者`year`（取决于当前视图）。

> There are three properties that add date validation to the datepicker input. The first two are the `min` and `max` properties. In addition to enforcing validation on the input, these properties will disable all dates on the calendar popup before or after the respective values and prevent the user from advancing the calendar past the `month` or `year` (depending on current view) containing the `min` or `max` date.

添加日期校验的第二种方法是使用日期选择器input中的`matDatepickerFilter`属性。此属性接受参数为`<D> => boolean`的函数（其中`<D>`是日期选择器使用的日期类型，参考[选择一个日期实现](https://material.angular.io/components/datepicker/overview#choosing-a-date-implementation-and-date-format-settings)）。结果为`true`表示日期合法，结果为`false`则表示非法。这同样会失效日历上的非法日期。与使用`min`和`max`最大的不同是实时过滤所有的日期，不能防止用户前进到某个日期。

> The second way to add date validation is using the `matDatepickerFilter` property of the datepicker input. This property accepts a function of `<D> => boolean` (where `<D>` is the date type used by the datepicker, see [Choosing a date implementation](https://material.angular.io/components/datepicker/overview#choosing-a-date-implementation-and-date-format-settings)). A result of `true` indicates that the date is valid and a result of `false` indicates that it is not. Again this will also disable the dates on the calendar that are invalid. However, one important difference between using `matDatepickerFilter` vs using `min` or `max` is that filtering out all dates before or after a certain point, will not prevent the user from advancing the calendar past that point.

暂无例子。

> In this example the user can back past 2005, but all of the dates before then will be unselectable. They will not be able to go further back in the calendar than 2000. If they manually type in a date that is before the min, after the max, or filtered out, the input will have validation errors.

每个校验属性都有一个检查错误：

> Each validation property has a different error that can be checked:

- 违反`min`属性的值将导致`matDatepickerMin`错误。
- 违反`max`属性的值将导致`matDatepickerMax`错误。
- 违反`matDatepickerFilter`属性的值将导致`matDatepickerFilter`错误。

> - A value that violates the `min` property will have a `matDatepickerMin` error.
> - A value that violates the `max` property will have a `matDatepickerMax` error.
> - A value that violates the `matDatepickerFilter` property will have a `matDatepickerFilter` error.

## input与change事件 

> Input and change events

input原生的`(input)`和`(change)`事件只在用户与input元素交互时触发；当用户从弹出的日历中选择一个日期时不会触发。因此日期选择器input支持(dateInput)`和`(dateChange)`事件。当用户与input和弹出日历交互时都会触发。

> The input's native `(input)` and `(change)` events will only trigger due to user interaction with the input element; they will not fire when the user selects a date from the calendar popup. Therefore, the datepicker input also has support for `(dateInput)` and `(dateChange)` events. These trigger when the user interacts with either the input or the popup.

当用户输入或者从日历中选择一个日期，导致value值发生变更时，将触发`(dateInput)`事件。当用户完成输入（`<input>`的blur事件）或者从日历中选中一个日期时，将触发`(dateChange)`事件。

> The `(dateInput)` event will fire whenever the value changes due to the user typing or selecting a date from the calendar. The `(dateChange)` event will fire whenever the user finishes typing input (on `<input>` blur), or when the user chooses a date from the calendar.

## 失效日期管理器的部件

> Disabling parts of the datepicker

> As with any standard `<input>`, it is possible to disable the datepicker input by adding the `disabled` property. By default, the `<mat-datepicker>` and `<mat-datepicker-toggle>` will inherit their disabled state from the `<input>`, but this can be overridden by setting the `disabled` property on the datepicker or toggle elements. This can be useful if you want to disable text input but allow selection via the calendar or vice-versa.

## 触摸UI模式

> Touch UI mode

日期选择器一般在input的下方弹出。但因为屏幕面积问题，对于触摸设备来说这样不理想，并且需要更大的点击目标。所以`<mat-datepicker>`有一个`touchUi`属性，可以设置为`true`来启用一个方便触摸的UI，并且会弹出更大的日历对话框。

> The datepicker normally opens as a popup under the input. However this is not ideal for touch devices that don't have as much screen real estate and need bigger click targets. For this reason `<mat-datepicker>` has a `touchUi` property that can be set to `true` in order to enable a more touch friendly UI where the calendar opens in a large dialog.

## 手动打开和关闭日历

> Manually opening and closing the calendar

弹出的日历可以通过`<mat-datepicker>`的`open`和`close`方法来控制。还有一个`opened`属性用来表示日历弹出状态。

> The calendar popup can be programmatically controlled using the `open` and `close` methods on the `<mat-datepicker>`. It also has an `opened` property that reflects the status of the popup.

## 国际化

> Internationalization

日期选择器的国际化通过四个方面设置：

> Internationalization of the datepicker is configured via four aspects:

1. 日期地区。
2. 日期选择器允许的日期实现。
3. 日期选择器使用的展示与解析格式。
4. 日期选择器UI使用的信息字符串。

> 1. The date locale.
> 2. The date implementation that the datepicker accepts.
> 3. The display and parse formats used by the datepicker.
> 4. The message strings used in the datepicker's UI.

### 设置地区码

> Setting the locale code

默认`MAT_DATE_LOCALE`注入token会使用`@angular/core`中已存在的`LOCALE_ID`地区码。如果想复写，可以提供一个`MAT_DATE_LOCALE`的新值。

> By default, the `MAT_DATE_LOCALE` injection token will use the existing `LOCALE_ID` locale code from `@angular/core`. If you want to override it, you can provide a new value for the `MAT_DATE_LOCALE` token:

```ts
@NgModule({
  providers: [
    {provide: MAT_DATE_LOCALE, useValue: 'en-GB'},
  ],
})
export class MyApp {}
```

也可以通过`DateAdapter`中的`setLocale`方法来实时设置地区。

> It's also possible to set the locale at runtime using the `setLocale` method of the `DateAdapter`.

### 选择一个日期实现和日期格式设置

> Choosing a date implementation and date format settings

日期选择器设计为与日期实现无关。意味着可以与各种不同的日期实现协作。但同样意味着开发者需要提供合适的日期选择器组件，来与选择的日期实现协作。最简单的方法是引入预设的模块：

> The datepicker was built to be date implementation agnostic. This means that it can be made to work with a variety of different date implementations. However it also means that developers need to make sure to provide the appropriate pieces for the datepicker to work with their chosen implementation. The easiest way to ensure this is just to import one of the pre-made modules:

模块|日期类型|支持地区|依赖|引入方式
-|-|-|-|-
MatNativeDateModule|Date|en-US|None|@angular/material
MatMomentDateModule|Moment|[参见项目](https://github.com/moment/moment/tree/develop/src/locale)|[Moment.js](https://momentjs.com/)|@angular/material-moment-adapter

*注意：`MatNativeDateModule`是基于JavaScript原生`Date`对象的功能，因此与许多地区不兼容。原生`Date`对象最大的缺点是无法设置解析格式。我们强烈建议使用`MomentDateAdapter`或者一个自定义的`DateAdapter`，使之能与你选择的格式/解析库协作*

> *Please note: `MatNativeDateModule` is based off of the functionality available in JavaScript's native `Date` object, and is thus not suitable for many locales. One of the biggest shortcomings of the native `Date` object is the inability to set the parse format. We highly recommend using the `MomentDateAdapter` or a custom `DateAdapter` that works with the formatting/parsing library of your choice.*

这些模块引入了`DateAdapter`和`MAT_DATE_FORMATS`

> These modules include providers for `DateAdapter` and `MAT_DATE_FORMATS`

```ts
@NgModule({
  imports: [MatDatepickerModule, MatNativeDateModule],
})
export class MyApp {}
```

因为`DateAdapter`是泛型类，所以`MatDatepicker`和`MatDatepickerInput`同样需要是泛型。当与这些类（例如作为一个`ViewChild`）协作时，可以引入对应使用的`DateAdapter`实现的合适泛型类型。例如：

> Because `DateAdapter` is a generic class, `MatDatepicker` and `MatDatepickerInput` also need to be made generic. When working with these classes (for example as a `ViewChild`) you should include the appropriate generic type that corresponds to the `DateAdapter` implementation you are using. For example:

```ts
@Component({...})
export class MyComponent {
  @ViewChild(MatDatepicker) datepicker: MatDatepicker<Date>;
}
```

也可以创建自己的`DateAdapter`来适配应用需求的日期格式。需要完成一个`DateAdapter`子类并将其作为`DateAdapter`的实现。同样你也希望能确保应用中提供的`MAT_DATE_FORMATS`格式化后被你的日期实现解析。关于`MAT_DATE_FORMATS`的更多信息可以参考[自定义格式的解析与展示](https://material.angular.io/components/datepicker/overview#customizing-the-parse-and-display-formats)

> It is also possible to create your own `DateAdapter` that works with any date format your app requires. This is accomplished by subclassing `DateAdapter` and providing your subclass as the `DateAdapter` implementation. You will also want to make sure that the `MAT_DATE_FORMATS` provided in your app are formats that can be understood by your date implementation. See [Customizing the parse and display formats](https://material.angular.io/components/datepicker/overview#customizing-the-parse-and-display-formats) for more information about `MAT_DATE_FORMATS`.

```ts
@NgModule({
  imports: [MatDatepickerModule],
  providers: [
    {provide: DateAdapter, useClass: MyDateAdapter},
    {provide: MAT_DATE_FORMATS, useValue: MY_DATE_FORMATS},
  ],
})
export class MyApp {}
```

### 自定义格式的解析与展示

> Customizing the parse and display formats

`MAT_DATE_FORMATS`对象是一个日期选择器在解析和展示日期时使用的格式集合。这些格式通过`DateAdapter`传递，所以你需要确认使用的这些格式对象与应用中使用的`DateAdapter`兼容。

> The `MAT_DATE_FORMATS` object is just a collection of formats that the datepicker uses when parsing and displaying dates. These formats are passed through to the `DateAdapter` so you will want to make sure that the format objects you're using are compatible with the `DateAdapter` used in your app.

如果想使用Angular Material附带的`DateAdapters`，同时使用自定义的`MAT_DATE_FORMATS`，可以引入`NativeDateModule`或者`MomentDateModule`。这些模块与"Mat"前缀的版本（`MatNativeDateModule`和`MatMomentDateModule`）一致，除了不包含默认的格式。例如：

> If you want use one of the `DateAdapters` that ships with Angular Material, but use your own `MAT_DATE_FORMATS`, you can import the `NativeDateModule` or `MomentDateModule`. These modules are identical to the "Mat"-prefixed versions (`MatNativeDateModule` and `MatMomentDateModule`) except they do not include the default formats. For example:

```ts
@NgModule({
  imports: [MatDatepickerModule, NativeDateModule],
  providers: [
    {provide: MAT_DATE_FORMATS, useValue: MY_NATIVE_DATE_FORMATS},
  ],
})
export class MyApp {}
```

### 本地化标签和信息

> Localizing labels and messages

可通过`MatDatepickerIntl`提供日期选择器中使用的各种文本字符串。可以在你应用的根模块中，通过提供包含翻译值的一个子类来完成这些信息的本地化。

> The various text strings used by the datepicker are provided through `MatDatepickerIntl`. Localization of these messages can be done by providing a subclass with translated values in your application root module.

```ts
@NgModule({
  imports: [MatDatepickerModule, MatNativeDateModule],
  providers: [
    {provide: MatDatepickerIntl, useClass: MyIntl},
  ],
})
export class MyApp {}
```

## 无障碍

> Accessibility

`MatDatepickerInput`指令会在原生input元素中添加`aria-haspopup`属性，当`role="dialog"`时会触发一个日历对话框。

> The `MatDatepickerInput` directive adds `aria-haspopup` attribute to the native input element, and it triggers a calendar dialog with `role="dialog"`.

`MatDatepickerIntl`包含`aria-label`需要的字符串。日期选择器input应当包含一个占位符，或者通过`aria-label`, `aria-labelledby`, `MatDatepickerIntl`指定一个有意义的标签。

> `MatDatepickerIntl` includes strings that are used for `aria-label`s. The datepicker input should have a placeholder or be given a meaningful label via `aria-label`, `aria-labelledby` or `MatDatepickerIntl`.

### 键盘快捷键

> Keyboard shortcuts

日期选择器支持下列键盘快捷键：

> The datepicker supports the following keyboard shortcuts:

快捷键|动作|Action
-|-|-
`ALT` + `下方向键`|打开日历弹出框。|Open the calendar pop-up
`ESCAPE`|关闭日历弹出框。|Close the calendar pop-up

在月视图中：

> In month view:

快捷键|动作|Action
-|-|-
`左方向键`|前一天|Go to previous day
`右方向键`|后一天|Go to next day
`上方向键`|上一周的同一天|Go to same day in the previous week
`下方向键`|下一周的同一天|Go to same day in the next week
`HOME`|本月第一天|Go to the first day of the month
`END`|本月最后一天|Go to the last day of the month
`PAGE_UP`|上个月的同一天|Go to the same day in the previous month
`ALT` + `PAGE_UP`|去年的同一天|Go to the same day in the previous year
`PAGE_DOWN`|下个月的同一天|Go to the same day in the next month
`ALT` + `PAGE_DOWN`|明年的同一天|Go to the same day in the next year
`ENTER`|选择当前日期|Select current date

在年视图中：

> In year view:

快捷键|动作|Action
-|-|-
`左方向键`|上个月|Go to previous month
`右方向键`|下个月|Go to next month
`上方向键`|前4个月|Go up a row (back 4 months)
`下方向键`|后4个月|Go down a row (forward 4 months)
`HOME`|今年第一个月|Go to the first month of the year
`END`|今年最后一个月|Go to the last month of the year
`PAGE_UP`|去年的同一个月|Go to the same month in the previous year
`ALT` + `PAGE_UP`|10年前同一个月|Go to the same month 10 years back
`PAGE_DOWN`|明年同一个月|Go to the same month in the next year
`ALT` + `PAGE_DOWN`|10年后同一个月|Go to the same month 10 years forward
`ENTER`|选择当月|Select current month

## 在跨年视图中：

> In multi-year view:

快捷键|动作|Action
-|-|-
`左方向键`|前一年|Go to previous year
`右方向键`|后一年|Go to next year
`上方向键`|前四年|Go up a row (back 4 years)
`下方向键`|后四年|Go down a row (forward 4 years)
`HOME`|当前范围的第一年|Go to the first year in the current range
`END`|当前范围的最后一年|Go to the last year in the current range
`PAGE_UP`|前24年|Go back 24 years
`ALT` + `PAGE_UP`|前240年|Go back 240 years
`PAGE_DOWN`|后24年|Go forward 24 years
`ALT` + `PAGE_DOWN`|后240年|Go forward 240 years
`ENTER`|选择当前年|Select current year

## 问题定位

> Troubleshooting

### Error: MatDatepicker: No provider found for DateAdapter/MAT_DATE_FORMATS

当没有提供所有日期选择器需要的注入时会出现此错误。最简单的解决方法是在应用的根模块中引入`MatNativeDateModule`或者`MatMomentDateModule`。更多信息请参考[选择一个日期实现](https://material.angular.io/components/datepicker/overview#choosing-a-date-implementation-and-date-format-settings)。

> This error is thrown if you have not provided all of the injectables the datepicker needs to work. The easiest way to resolve this is to import the `MatNativeDateModule` or `MatMomentDateModule` in your application's root module. See [Choosing a date implementation](https://material.angular.io/components/datepicker/overview#choosing-a-date-implementation-and-date-format-settings) for more information.

### Error: A MatDatepicker can only be associated with a single input

当多个`<input>`都尝试对同一个`<mat-datepicker>`有所有权时（通过input中的`matDatepicker`属性）将出现此错误。一个日期选择器只能关联一个input。

> This error is thrown if more than one `<input>` tries to claim ownership over the same `<mat-datepicker>` (via the `matDatepicker` attribute on the input). A datepicker can only be associated with a single input.

### Error: Attempted to open an MatDatepicker with no associated input.

当`<mat-datepicker>`没有关联任何`<input>`时会出现此错误。需要为日期选择器创建一个模板参考，并指定其为input的`matDatepicker`属性。

> This error occurs if your `<mat-datepicker>` is not associated with any `<input>`. To associate an input with your datepicker, create a template reference for the datepicker and assign it to the `matDatepicker` attribute on the input:

```html
<input [matDatepicker]="picker">
<mat-datepicker #picker></mat-datepicker>
```

# API

## 日期选择器（datepicker）的API参考

> API reference for Angular Material datepicker

```typescript
import {MatDatepickerModule} from '@angular/material/datepicker';
```

## 服务

> Services

### MatDatepickerIntl

需要国际化的日期选择器数据

> Datepicker data that requires internationalization.

**属性**

> Properties

属性名|描述|Description
-|-|-
calendarLabel: string|日历弹出框的标签（用于屏幕阅读者）。|A label for the calendar popup (used by screen readers).
changes: Subject<void\>|当以下标签改变时产生的事件流。用于初始化后当标签改变之后通知组件。|Stream that emits whenever the labels here are changed. Use this to notify components if the labels have changed after initialization.
nextMonthLabel: string|下个月按钮的标签（用于屏幕阅读者）。|A label for the next month button (used by screen readers).
nextMultiYearLabel: string|下一跨年按钮的标签（用于屏幕阅读者）。|A label for the next multi-year button (used by screen readers).
nextYearLabel: string|下一年按钮的标签（用于屏幕阅读者）。|A label for the next year button (used by screen readers).
openCalendarLabel: string|用于打开日历弹出框的按钮的标签（用于屏幕阅读者）。|A label for the button used to open the calendar popup (used by screen readers).
prevMonthLabel: string|上个月按钮的标签（用于屏幕阅读者）。|A label for the previous month button (used by screen readers).
prevMultiYearLabel: string|上一跨年按钮的标签（用于屏幕阅读者）。|A label for the previous multi-year button (used by screen readers).
prevYearLabel: string|上一年按钮的标签（用于屏幕阅读者）。|A label for the previous year button (used by screen readers).
switchToMonthViewLabel: string|切换到月视图按钮的标签（用于屏幕阅读者）。|A label for the 'switch to month view' button (used by screen readers).
switchToMultiYearViewLabel: string|切换到年视图按钮的标签（用于屏幕阅读者）。|A label for the 'switch to year view' button (used by screen readers).

## 指令

> Directives

### MatDatepicker

管理日期选择器弹出框和对话框的组件。

> Component responsible for managing the datepicker popup/dialog.

Selector: `mat-datepicker`

Exported as: `matDatepicker`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>disabled: boolean|是否失效日历选择器的日历弹出。|Whether the datepicker pop-up should be disabled.
@Input()<br>opened: boolean|日历是否打开。|Whether the calendar is open.
@Input()<br>panelClass: string \| string[]|传递给日期选择器面板的类。与ngClass语法一致。|Classes to be passed to the date picker panel. Supports the same syntax as ngClass.
@Input()<br>startAt: D \| null|打开日历初始化的日期。|The date to open the calendar to initially.
@Input()<br>startView: 'month' \| 'year'|日历的初始视图。|The view that the calendar should start in.
@Input()<br>touchUi: boolean|日历UI是否为触摸模式。在触摸模式下，日历打开为一个更大对话框，各个元素也为了触摸而变大。|Whether the calendar UI is in touch mode. In touch mode the calendar opens in a dialog rather than a popup and elements have more padding to allow for bigger touch targets.
@Output('closed')<br>closedStream: EventEmitter<void\>|日期选择器关闭事件。|Emits when the datepicker has been closed.
@Output('opened')<br>openedStream: EventEmitter<void\>|日期选择器打开事件。|Emits when the datepicker has been opened.
id: string|日期选择器日历的id。|The id for the datepicker calendar.

**方法**

> Methods

open
---
- 描述
  - 打开日历
  - Open the calendar.

close
---
- 描述
  - 关闭日历
  - Close the calendar.

### MatDatepickerInput

用于连接一个input到MatDatepicker的指令。

> Directive used to connect an input to a MatDatepicker.

Selector: `input[matDatepicker]`

Exported as: `matDatepickerInput`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>disabled: boolean|是否失效日期选择器输入框。|Whether the datepicker-input is disabled.
@Input()<br>matDatepicker: MatDatepicker<D\>|input关联的日期选择器。|The datepicker that this input is associated with.
@Input()<br>matDatepickerFilter: (date: D \| null) => boolean|在日期选择器内用于过滤日期的函数。|Function that can be used to filter out dates within the datepicker.
@Input()<br>max: D \| null|合法日期的最大值。|The maximum valid date.
@Input()<br>min: D \| null|合法日期的最小值。|The minimum valid date.
@Input()<br>value: D \| null|input的值。|The value of the input.
@Output()<br>dateChange: EventEmitter<MatDatepickerInputEvent<D\>\>|当在此<input\>发生改变事件时传递的事件。|Emits when a change event is fired on this <input\>.
@Output()<br>dateInput: EventEmitter<MatDatepickerInputEvent<D\>\>|当在此<input\>发生输入事件时传递的事件。|Emits when an input event is fired on this <input\>.

**方法**

> Methods

getPopupConnectionElementRef
---
- 描述
  - 获取日期选择器弹出框连接的元素。 
  - Gets the element that the datepicker popup should be connected to.
- 返回
  - ElementRef : 关联的元素。
  - The element to connect the popup to.

### MatDatepickerToggleIcon

用于复写matDatepickerToggle的图标

> Can be used to override the icon of a matDatepickerToggle.

Selector: `[matDatepickerToggleIcon]`

### MatDatepickerToggle

Selector: `mat-datepicker-toggle`

Exported as: `matDatepickerToggle`


**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('for')<br>datepicker: MatDatepicker<D\>|切换按钮触发的日期选择器实例。|Datepicker instance that the button will toggle.
@Input()<br>disabled: boolean|切换按钮是否失效。|Whether the toggle button is disabled.

## 附加类

> Additional classes

### MatDatepickerInputEvent

用于日期选择器input和change事件。因为可能由于用户点击了日历弹出框按钮而触发的事件，通常无法获取原生input或者change事件。为了一致性，使用MatDatepickerInputEvent来代替。

> An event used for datepicker input and change events. We don't always have access to a native input or change event because the event may have been triggered by the user clicking on the calendar popup. For consistency, we always use MatDatepickerInputEvent instead.

**属性**

> Properties

属性名|描述|Description
-|-|-
target: MatDatepickerInput<D\>|触发此事件的日期选择器input组件参考。|Reference to the datepicker input component that emitted the event.
targetElement: HTMLElement|关联日期选择器input的原生input元素参考。|Reference to the native input element associated with the datepicker input.
value: D \| null|目标日期选择器input的新值。|The new value for the target datepicker input.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*