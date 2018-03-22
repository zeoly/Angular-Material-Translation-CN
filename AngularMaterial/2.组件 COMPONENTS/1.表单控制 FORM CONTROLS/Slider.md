[toc]

# 概述

> OVERVIEW

`<mat-slider>`允许用鼠标、触摸或者键盘在一个范围内选择一个值，与`<input type="range">`相似。

> `<mat-slider>` allows for the selection of a value from a range via mouse, touch, or keyboard, similar to `<input type="range">`.

*注意：此组件的滑动行为需要在页面加载HammerJS。*

> *Note: the sliding behavior for this component requires that HammerJS is loaded on the page.*

## 选择一个值

> Selecting a value

slider默认的最小值是`0`，最大值是`100`，滑块的步进是`1`。可通过`min`, `max`, `step`属性来分别设置各个值。如果没有其他设置，初始值为最小值。

> By default the minimum value of the slider is `0`, the maximum value is `100`, and the thumb moves in increments of `1`. These values can be changed by setting the `min`, `max`, and `step` attributes respectively. The initial value is set to the minimum value unless otherwise specified.

```html
<mat-slider min="1" max="5" step="0.5" value="1.5"></mat-slider>
```

## 方向

> Orientation

默认slider是水平的，最小值在左边，最大值在右边。可以在slider中添加一个`vertical`属性，将其设置为垂直方向，最小值在下，最大值在上。

> By default sliders are horizontal with the minimum value on the left and the maximum value on the right. The `vertical` attribute can be added to a slider to make it vertical with the minimum value on bottom and the maximum value on top.

```html
<mat-slider vertical></mat-slider>
```

也可以添加一个`invert`属性，来反转滑块移动的方向。一个反转的水平slider最小值在右边，最大值在左边。一个反转的垂直slider最小值在上面，最大值在下面。

> An `invert` attribute is also available which can be specified to flip the axis that the thumb moves along. An inverted horizontal slider will have the minimum value on the right and the maximum value on the left, while an inverted vertical slider will have the minimum value on top and the maximum value on bottom.

```html
<mat-slider invert></mat-slider>
```

## 刻度线

> Tick marks

默认slider不显示刻度。可通过`tickInterval`属性来启用此功能。`tickInterval`的值表示每个刻度之间的步进数量。例如`tickInterval`为`3`，`step`为`4`表示每`3`个步进画一个刻度，也就是`12`。

> By default, sliders do not show tick marks along the thumb track. This can be enabled using the `tickInterval` attribute. The value of `tickInterval` should be a number representing the number of steps between between ticks. For example a `tickInterval` of `3` with a `step` of `4` will draw tick marks at every `3` steps, which is the same as every `12` values.

```html
<mat-slider step="4" tickInterval="3"></mat-slider>
```

`tickInterval`也可以设置为`auto`，表示自动选择步进的数量来画刻度，每个刻度间隔至少`30px`。

> The `tickInterval` can also be set to `auto` which will automatically choose the number of steps such that there is at least `30px` of space between ticks.

```html
<mat-slider tickInterval="auto"></mat-slider>
```

slider的起始和末尾总会显示刻度。如果剩余的长度不适配的间隔，最后一个间隔将会缩短或延长来保证末尾显示刻度。

> The slider will always show a tick at the beginning and end of the track. If the remaining space doesn't add up perfectly the last interval will be shortened or lengthened so that the tick can be shown at the end of the track.

在[aterial Design手册](https://material.google.com/components/sliders.html)中建议只在显示分离值（例如打分1到5）的情况下使用`tickInterval`属性（与`thumbLabel`属性一样设为`1`）。

> The [Material Design spec](https://material.google.com/components/sliders.html) recommends using the `tickInterval` attribute (set to `1` along with the `thumbLabel` attribute) only for sliders that are used to display a discrete value (such as a 1-5 rating).

## 键盘交互

> Keyboard interaction

slider有如下按键对应：

> The slider has the following keyboard bindings:

按键|动作|Action
-|-|-
右方向键|slider值增加一个步进（从右到左方向为减少）。|Increment the slider value by one step (decrements in RTL).
上方向键|slider值增加一个步进。|Increment the slider value by one step.
左方向键|slider值减少一个步进（从右到左方向为增加）。|Decrement the slider value by one step (increments in RTL).
下方向键|slider值减少一个步进。|Decrement the slider value by one step.
Page up|slider值增加10个步进。|Increment the slider value by 10 steps.
Page down|slider值减少10个步进。|Decrement the slider value by 10 steps.
End|设置为最大值。|Set the value to the maximum possible.
Home|设置为最小值|Set the value to the minimum possible.

## 无障碍

> Accessibility

没有文本和标签的slider需要通过`aria-label`或`aria-labelledby`指定一个有意义的标签。

> Sliders without text or labels should be given a meaningful label via `aria-label` or `aria-labelledby`.

# API

## 滑块(slider)的API参考

> API reference for Angular Material slider

```ts
import {MatSliderModule} from '@angular/material/slider';
```

## 指令

> Directives

### MatSlider

允许用户使用一个滑块在一个范围内选择一个值。与原生的`<input type="range">`元素的行为相似。

> Allows users to select from a range of values by moving the slider thumb. It is similar in behavior to the native `<input type="range"\>` element.

Selector: `mat-slider`

Exported as: `matSlider`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>color: ThemePalette|组件的主题色调。|Theme color palette for the component.
@Input()<br>disabled: boolean|组件是否失效。|Whether the component is disabled.
@Input()<br>invert: boolean|slider是否反转。|Whether the slider is inverted.
@Input()<br>max: number|slider的最大值。|The maximum value that the slider can have.
@Input()<br>min: number|slider的最小值。|The minimum value that the slider can have.
@Input()<br>step: number|slider的步进。|The values at which the thumb will snap.
@Input()<br>thumbLabel: boolean|是否显示滑块标签。|Whether or not to show the thumb label.
@Input()<br>tickInterval: 'auto' \| number|刻度间隔。与步进相关，为步进的整数倍。例如间隔为4，步进为3，表示刻度间隔为12。|How often to show ticks. Relative to the step so that a tick always appears on a step. Ex: Tick interval of 4 with a step of 3 will draw a tick every 4 steps (every 12 values).
@Input()<br>value: number \| null|slider的值。|Value of the slider.
@Input()<br>vertical: boolean|slider是否为垂直。|Whether the slider is vertical.
@Output()<br>change: EventEmitter<MatSliderChange\>|slider值发生改变的事件。|Event emitted when the slider value has changed.
@Output()<br>input: EventEmitter<MatSliderChange\>|slider的滑块移动的事件。|Event emitted when the slider thumb moves.
displayValue: string \| number|用于展示目的的值。|The value to be used for display purposes.
onTouched: () => any|通过registerOnTouch (ControlValueAccessor)注册的触摸事件。|onTouch function registered via registerOnTouch (ControlValueAccessor).
percent: number|slider值对应的百分比。|The percentage of the slider that coincides with the value.

**方法**

> Methods

blur
---
- 描述
  - 失焦属主元素。
  - blur the host element

focus
---
- 描述
  - 使属主元素获得焦点。
  - set focus to the host element

## 附加类

> Additional classes

### MatSliderChange

由MatSlider组件传递的一个简单变更事件。

> A simple change event emitted by the MatSlider component.

**属性**

> Properties

属性名|描述|Description
-|-|-
source: MatSlider|发生变更的MatSlider。|The MatSlider that changed.
value: number \| null|源slider 的新值。|The new value of the source slider.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*