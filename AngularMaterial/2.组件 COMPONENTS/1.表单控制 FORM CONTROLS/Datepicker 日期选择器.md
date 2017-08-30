[toc]

# 概述

> OVERVIEW

日期选择器允许用户直接在文本框中输入或者在日历中选择一个日期，来输入一个日期。日期选择器由几个组件和指令共同协作：

> The datepicker allows users to enter a date either through text input, or by choosing a date from the calendar. It is made up of several components and directives that work together:

## 当前状态

> Current state

目前日期选择器还在初始阶段，支持日期选择等基本功能。在后续的迭代中，将加入以下功能：

> Currently the datepicker is in the beginning stages and supports basic date selection functionality. There are many more features that will be added in future iterations, including:

- 只支持datetimes（例如May 2, 2017 at 12:30pm）和月+年（例如May 2017）
- 支持选择和展示日期范围
- 支持自定义时区
- 日历月份可滚动
- 内嵌支持[Moment.js](https://momentjs.com/)日期

> - Support for datetimes (e.g. May 2, 2017 at 12:30pm) and month + year only (e.g. May 2017)
> - Support for selecting and displaying date ranges
> - Support for custom time zones
> - Infinite scrolling through calendar months
> - Built in support for [Moment.js](https://momentjs.com/) dates

## 将日期选择器与输入框连接

> Connecting a datepicker to an input

一个日期选择由一个文本输入框和一个弹出日历框组成，通过文本输入框中的mdDatepicker来连接。

> A datepicker is composed of a text input and a calendar pop-up, connected via the mdDatepicker property on the text input.

```html
<input [mdDatepicker]="myDatepicker">
<md-datepicker #myDatepicker></md-datepicker>
```

提供了一个可选择的日期选择器切换按钮。可以在上述例子中加入一个切换按钮：

> An optional datepicker toggle button is available. A toggle can be added to the example above:

```html
<input [mdDatepicker]="myDatepicker">
<button [mdDatepickerToggle]="myDatepicker"></button>
<md-datepicker #myDatepicker></md-datepicker>
```

这与<md-input-container>中的输入框效果一样，并且可以在material input中使用前缀和后缀，来简单的切换。

> This works exactly the same with an input that is part of an <md-input-container> and the toggle can easily be used as a prefix or suffix on the material input:

```html
<md-input-container>
  <input mdInput [mdDatepicker]="myDatepicker">
  <button mdSuffix [mdDatepickerToggle]="myDatepicker"></button>
</md-input-container>
<md-datepicker #myDatepicker></md-datepicker>
```

## 设置日历起始视图

> Setting the calendar starting view

默认日历打开是月视图，可以设置md-datepicker的startView属性为year来改变显示。在年视图中，用户可以看见所有的月份，并可以选择一个月份进入月视图。

> By default the calendar will open in month view, this can be changed by setting the startView property of md-datepicker to "year". In year view the user will see all months of the year and then proceed to month view after choosing a month.

日历打开时显示的年月，取决于当前是否已选择了日期，如果是，将显示日期的年月。如果没有选择日期，将显示当前日期的年月。这个行为可以被md-datepicker的startAt属性重载。

> The month or year that the calendar opens to is determined by first checking if any date is currently selected, if so it will open to the month or year containing that date. Otherwise it will open to the month or year containing today's date. This behavior can be overridden by using the startAt property of md-datepicker. In this case the calendar will open to the month or year containing the startAt date.

## 日期校验

> Date validation

在日期选择器input中增加日期校验共有三个属性。前两个是min和max属性。为了在input中执行校验，这些属性将失效掉弹出日历中在这两个属性前后的所有日期，并防止用户进入到包括min或者max的年月（取决于当前的视图）。

> There are three properties that add date validation to the datepicker input. The first two are the min and max properties. In addition to enforcing validation on the input, these properties will disable all dates on the calendar popup before or after the respective values and prevent the user from advancing the calendar past the month or year (depending on current view) containing the min or max date.

增加日期校验的第二种方法是使用日期选择器input的mdDatepickerFilter属性。此属性接受<D> => boolean函数（<D>是日期选择器使用的日期类型，参考[choosing a date implementation](https://material.angular.io/#choosing-a-date-implementation-and-date-format-settings)章节）。返回结果为true表示日期合法，结果为false表示非法。同样这样做也会失效日历中的非法日期。但使用mdDatepickerFilter和min/max的最大区别是，不会防止用户进入到过滤的日期页面。

> The second way to add date validation is using the mdDatepickerFilter property of the datepicker input. This property accepts a function of <D> => boolean (where <D> is the date type used by the datepicker, see section on [choosing a date implementation](https://material.angular.io/#choosing-a-date-implementation-and-date-format-settings)). A result of true indicates that the date is valid and a result of false indicates that it is not. Again this will also disable the dates on the calendar that are invalid. However, one important difference between using mdDatepickerFilter vs using min or max is that filtering out all dates before or after a certain point, will not prevent the user from advancing the calendar past that point.

在这个例子中，用户可以退回到2005，但是所有在此之前的日期都无法选择。并且日历不能退回到2000之前。如果手动输入一个在min之前的日期，或者max之后的日期，或者过滤之外的，input将有一个验证错误。

> In this example the user can back past 2005, but all of the dates before then will be unselectable. They will not be able to go further back in the calendar than 2000. If they manually type in a date that is before the min, after the max, or filtered out, the input will have validation errors.

每个验证属性有各自的可检查的错误：
- 比min属性小的值将导致一个mdDatepickerMin错误。
- 比max属性小的值将导致一个mdDatepickerMax错误。
- 不符合mdDatepickerFilter属性的值将导致一个mdDatepickerFilter错误。

> Each validation property has a different error that can be checked:
> - A value that violates the min property will have a mdDatepickerMin error.
> - A value that violates the max property will have a mdDatepickerMax error.
> - A value that violates the mdDatepickerFilter property will have a mdDatepickerFilter error.

## 触摸UI模式

> Touch UI mode

日期选择器一般在input的下方弹出。但因为屏幕面积问题，对于触摸设备来说这样不理想，并且需要更大的点击目标。所以md-datepicker有一个touchUi属性，可以设置为true来启用一个触摸友好的UI，并且会弹出更大的日历对话框。

> The datepicker normally opens as a popup under the input. However this is not ideal for touch devices that don't have as much screen real estate and need bigger click targets. For this reason md-datepicker has a touchUi property that can be set to true in order to enable a more touch friendly UI where the calendar opens in a large dialog.

## 手动打开和关闭日历

> Manually opening and closing the calendar

弹出的日历可以通过md-datepicker的open和close方法来控制。还有一个opened属性用来表示弹出状态。

> The calendar popup can be programmatically controlled using the open and close methods on the md-datepicker. It also has an opened property that reflects the status of the popup.

## 国际化

> Internationalization

为了支持国际化，日期选择器支持通过注入的以下三种自定义化：
1. 日期选择器允许的日期实现。
2. 日期选择器使用的展示与解析格式。
3. 日期选择器UI使用的信息字符串。

> In order to support internationalization, the datepicker supports customization of the following three pieces via injection:
> 1. The date implementation that the datepicker accepts.
> 2. The display and parse formats used by the datepicker.
> 3. The message strings used in the datepicker's UI.

### 选择一个日期实现和日期格式设置

> Choosing a date implementation and date format settings

日期选择器设计为日期实现不可知。意味着可以与各种不同的日期实现协同工作。但同样意味着开发者需要提供合适的日期选择器组件，来与选择的日期实现协同工作。最简单的方法是引入预设的模块（目前MdNativeDateModule是material中唯一的实现，但后续计划为支持Moment.js增加模块）：
- MdNativeDateModule - 支持原生JavaScript Date对象

> The datepicker was built to be date implementation agnostic. This means that it can be made to work with a variety of different date implementations. However it also means that developers need to make sure to provide the appropriate pieces for the datepicker to work with their chosen implementation. The easiest way to ensure this is just to import one of the pre-made modules (currently MdNativeDateModule is the only implementation that ships with material, but there are plans to add a module for Moment.js support):
> - MdNativeDateModule - support for native JavaScript Date object

这些模块引入了DateAdapter和MD_DATE_FORMATS。

> These modules include providers for DateAdapter and MD_DATE_FORMATS

```typescript
@NgModule({
  imports: [MdDatepickerModule, MdNativeDateModule],
})
export class MyApp {}
```

因为DateAdapter是泛型类，MdDatepicker和MdDatepickerInput同样需要是泛型。当与这些类（例如作为一个ViewChild）工作时，可以引入对应使用的DateAdapter实现的合适泛型类型。例如：

> Because DateAdapter is a generic class, MdDatepicker and MdDatepickerInput also need to be made generic. When working with these classes (for example as a ViewChild) you should include the appropriate generic type that corresponds to the DateAdapter implementation you are using. For example:

```typescript
@Component({...})
export class MyComponent {
  @ViewChild(MdDatepicker) datepicker: MdDatepicker<Date>;
}
```

*注意：MdNativeDateModule是基于JavaScript原生Date对象的功能，因此与许多locale不兼容。原生Date对象最大的缺点是无法设置解析格式。我们强烈建议使用一个自定义的DateAdapter，使之能与你选择的格式/解析库协作*

> *Please note: MdNativeDateModule is based off of the functionality available in JavaScript's native Date object, and is thus not suitable for many locales. One of the biggest shortcomings of the native Date object is the inability to set the parse format. We highly recommend using a custom DateAdapter that works with the formatting/parsing library of your choice.*

### 自定义日期实现

> Customizing the date implementation

日期选择器通过DateAdapter与date对象进行交互。继承DateAdapter并在provider中使用你的子类，就可以使日期选择器与不同的日期实现协作。同样还需要确认在app中提供的MD_DATE_FORMATS是与你的日期实现兼容的格式。

> The datepicker does all of its interaction with date objects via the DateAdapter. Making the datepicker work with a different date implementation is as easy as extending DateAdapter, and using your subclass as the provider. You will also want to make sure that the MD_DATE_FORMATS provided in your app are formats that can be understood by your date implementation.

```typescript
@NgModule({
  imports: [MdDatepickerModule],
  providers: [
    {provide: DateAdapter, useClass: MyDateAdapter},
    {provide: MD_DATE_FORMATS, useValue: MY_DATE_FORMATS},
  ],
})
export class MyApp {}
```

### 自定义解析和显示格式

> Customizing the parse and display formats

MD_DATE_FORMATS对象是一个日期选择器在解析和展示日期时使用的格式集合。这些格式传递到DateAdapter，你需要确认使用的这些格式对象与app中使用的DateAdapter兼容。这个例子展示了如何在material中使用带有自定义格式的原生的Date实现。

> The MD_DATE_FORMATS object is just a collection of formats that the datepicker uses when parsing and displaying dates. These formats are passed through to the DateAdapter so you will want to make sure that the format objects you're using are compatible with the DateAdapter used in your app. This example shows how to use the native Date implementation from material, but with custom formats.

```typescript
@NgModule({
  imports: [MdDatepickerModule],
  providers: [
    {provide: DateAdapter, useClass: NativeDateAdapter},
    {provide: MD_DATE_FORMATS, useValue: MY_NATIVE_DATE_FORMATS},
  ],
})
export class MyApp {}
```

### 本地化标签和信息

> Localizing labels and messages

在日期选择器中使用的各种文本字符串由MdDatepickerIntl提供。可以在你应用的跟模块中，通过提供包含翻译值的一个子类来完成这些信息的本地化。

> The various text strings used by the datepicker are provided through MdDatepickerIntl. Localization of these messages can be done by providing a subclass with translated values in your application root module.

```typescript
@NgModule({
  imports: [MdDatepickerModule, MdNativeDateModule],
  providers: [
    {provide: MdDatepickerIntl, useClass: MyIntl},
  ],
})
export class MyApp {}
```

# API

## 日期选择器（datepicker）的API参考

> API reference for Angular Material datepicker

```typescript
import {MdDatepickerModule} from '@angular/material';
```

## 服务

> Services

### MdDatepickerIntl

需要国际化的日期选择器数据

> Datepicker data that requires internationalization.

**属性**

> Properties

属性名|描述|Description
-|-|-
changes|当以下标签改变时产生的流。用于初始化后如果标签改变则后通知组件。|Stream that emits whenever the labels here are changed. Use this to notify components if the labels have changed after initialization.
calendarLabel|日历弹出框的标签（用于屏幕阅读者）。|A label for the calendar popup (used by screen readers).
openCalendarLabel|用于打开日历弹出框的按钮的标签（用于屏幕阅读者）。|A label for the button used to open the calendar popup (used by screen readers).
prevMonthLabel|上个月按钮的标签（用于屏幕阅读者）。|A label for the previous month button (used by screen readers).
nextMonthLabel|下个月按钮的标签（用于屏幕阅读者）。|A label for the next month button (used by screen readers).
prevYearLabel|上一年按钮的标签（用于屏幕阅读者）。|A label for the previous year button (used by screen readers).
nextYearLabel|下一年按钮的标签（用于屏幕阅读者）。|A label for the next year button (used by screen readers).
switchToMonthViewLabel|切换到月视图按钮的标签（用于屏幕阅读者）。|A label for the 'switch to month view' button (used by screen readers).
switchToYearViewLabel|切换到年视图按钮的标签（用于屏幕阅读者）。|A label for the 'switch to year view' button (used by screen readers).

## 指令

> Directives

### MdDatepicker

管理弹出日期选择器对话框的组件。

> Component responsible for managing the datepicker popup/dialog.

Selector: md-datepicker

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>startAt|打开日历初始化的日期。|The date to open the calendar to initially.
@Input()<br>startView|打开日历进入的视图。|The view that the calendar should start in.
@Input()<br>touchUi|日历UI是否为触摸模式。在触摸模式下，日历打开为一个更大对话框，各个元素也为了触摸而变大。|Whether the calendar UI is in touch mode. In touch mode the calendar opens in a dialog rather than a popup and elements have more padding to allow for bigger touch targets.
@Input()<br>disabled|是否失效日历选择器的弹出。|Whether the datepicker pop-up should be disabled.
@Output()<br>Deprecated<br>selectedChanged|当选择的日期变更时，传递新选择的日期。|Emits new selected date when selected date changes.
opened|日历是否打开。|Whether the calendar is open.
id|日期选择器日历的id。|The id for the datepicker calendar.

**方法**

> Methods

**open**
- 描述
  * 打开日历
  > Open the calendar.

**close**
- 描述
  * 关闭日历
  > Close the calendar.

### MdDatepickerInput

用于连接一个input到MdDatepicker的指令。

> Directive used to connect an input to a MdDatepicker.

Selector: input[mdDatepicker]
导出属性: mdDatepickerInput

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>mdDatepicker|input关联的日期选择器。|The datepicker that this input is associated with.
@Input()<br>matDatepicker||
@Input()<br>mdDatepickerFilter||
@Input()<br>matDatepickerFilter||
@Input()<br>value|input的值。|The value of the input.
@Input()<br>min|合法日期的最小值。|The minimum valid date.
@Input()<br>max|合法日期的最大值。|The maximum valid date.
@Input()<br>disabled|是否失效日期选择器输入框。|Whether the datepicker-input is disabled.
@Output()<br>dateChange|当在此<input>发生改变事件时传递的事件。|Emits when a change event is fired on this <input>.
@Output()<br>dateInput|当在此<input>发生输入事件时传递的事件。|Emits when an input event is fired on this <input>.

**方法**

> Methods

**registerOnValidatorChange**


**validate**


**getPopupConnectionElementRef**

- 描述
  * 获取日期选择器弹出框连接的元素。 
  > Gets the element that the datepicker popup should be connected to.
- 返回
  * ElementRef : 关联的元素。
  > The element to connect the popup to.


### MdDatepickerToggle

Selector: md-datepicker-toggle

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('for')<br>datepicker|按钮切换到的日期选择器实例。|Datepicker instance that the button will toggle.
@Input()<br>disabled|切换按钮是否失效。|Whether the toggle button is disabled.

## 附加类

> Additional classes

### MdDatepickerInputEvent

用于日期选择器输入和改变事件。通常我们无法获取原生输入或者改变事件，因为可能是由于用户点击了日历弹出框触发的事件。为了一致性，使用MdDatepickerInputEvent来代替。

> An event used for datepicker input and change events. We don't always have access to a native input or change event because the event may have been triggered by the user clicking on the calendar popup. For consistency, we always use MdDatepickerInputEvent instead.

**属性**

> Properties

属性名|描述|Description
-|-|-
value|目标日期选择器input的新值。|The new value for the target datepicker input.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*