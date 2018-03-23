[toc]

# 概述

> OVERVIEW

`<mat-slide-toggle>`是一个可以通过点击或者拖动来切换的一个开关控件。

> `<mat-slide-toggle>` is an on/off control that can be toggled via clicking or dragging.

切换开关的行为与复选框相似，但不支持`<mat-checkbox>`中的`indeterminate`状态。

> The slide-toggle behaves similarly to a checkbox, though it does not support an `indeterminate` state like `<mat-checkbox>`.

*此组件的滑动行为需要在页面加载HammerJS*

> *Note: the sliding behavior for this component requires that HammerJS is loaded on the page.*

## 滑动开关标签

> Slide-toggle label

可以在`<mat-slide-toggle>`元素的内容中提供滑动开关标签。

> The slide-toggle label is provided as the content to the `<mat-slide-toggle>` element.

如果不想标签显示在滑动开关的旁边，可以通过`aria-label`或者`aria-labelledby`指定一个有意义的标签。

> If you don't want the label to appear next to the slide-toggle, you can use `aria-label` or `aria-labelledby` to specify an appropriate label.

## 在`@angular/forms`中使用

> Use with `@angular/forms`

`<mat-slide-toggle>`与`@angular/forms`兼容，支持`FormsModule`和`ReactiveFormsModule`。

> `<mat-slide-toggle>` is compatible with `@angular/forms` and supports both `FormsModule` and `ReactiveFormsModule`.

## 主题

> Theming

可通过`color`属性来改变`<mat-slide-toggle>`的颜色。默认滑动开关使用主题的主色调。也可改变为`'primary'`或者`'warn'`。

> The color of a `<mat-slide-toggle>` can be changed by using the `color` property. By default, slide-toggles use the theme's accent color. This can be changed to `'primary'` or `'warn'`.

## 无障碍

> Accessibility

`<mat-slide-toggle>`使用内部的`<input type="checkbox">`来提供无障碍体验。此内部的checkbox获取焦点并自动使用`<mat-slide-toggle>`元素的文本内容作为标签。

> The `<mat-slide-toggle>` uses an internal `<input type="checkbox">` to provide an accessible experience. This internal checkbox receives focus and is automatically labelled by the text content of the `<mat-slide-toggle>` element.

没有文本或者标签的滑动开关可以通过`aria-label`或者`aria-labelledby`指定一个有意义的标签。

> Slide toggles without text or labels should be given a meaningful label via `aria-label` or `aria-labelledby`.

# API

## 滑动开关(slide-toggle)的API参考

> API reference for Angular Material slide-toggle

```ts
import {MatSlideToggleModule} from '@angular/material/slide-toggle';
```

## 指令

> Directives

### MatSlideToggle

表示可切换的一个滑块开关，可在开/关状态间切换。

> Represents a slidable "switch" toggle that can be moved between on and off.

Selector: `mat-slide-toggle`

Exported as: `matSlideToggle`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('aria-label')<br>ariaLabel: string \| null|input元素内的滑块开关元素是否用来设置aria-label标签。|Whether the slide-toggle element is checked or not Used to set the aria-label attribute on the underlying input element.
@Input('aria-labelledby')<br>ariaLabelledby: string \| null|设置input元素的aria-labelledby属性。|Used to set the aria-labelledby attribute on the underlying input element.
@Input()<br>checked: boolean|滑块元素是否失效。|Whether the slide-toggle element is checked or not
@Input()<br>color: ThemePalette|组件的主题色调。|Theme color palette for the component.
@Input()<br>disableRipple: boolean|是否失效水波效果。|Whether ripples are disabled.
@Input()<br>disabled: boolean|组件是否失效。|Whether the component is disabled.
@Input()<br>id: string|滑块开关的唯一id。如果没有设置，将自动生成。|A unique id for the slide-toggle input. If none is supplied, it will be auto-generated.
@Input()<br>labelPosition: 'before' \| 'after'|标签显示在滑块开关的前面还是后面。默认是'after'。|Whether the label should appear after or before the slide-toggle. Defaults to 'after'
@Input()<br>name: string \| null|应用到input元素的name值。|Name value will be applied to the input element if present
@Input()<br>required: boolean|滑动开关是否必须。|Whether the slide-toggle is required.
@Output()<br>change: EventEmitter<MatSlideToggleChange\>|滑动开关的值变更的事件。|An event will be dispatched each time the slide-toggle changes its value.
inputId: string|返回视觉上隐藏的input的唯一id。|Returns the unique id for the visual hidden input.

**方法**

> Methods

focus
---
- 描述
  - 使滑动开关获得焦点。
  - Focuses the slide-toggle.

toggle
---
- 描述
  - 切换滑动开关的选中状态。
  - Toggles the checked state of the slide-toggle.

## 附加类

> Additional classes

### MatSlideToggleChange

MatSlideToggle传递的变更事件。

> Change event object emitted by a MatSlideToggle.

**属性**

> Properties

属性名|描述|Description
-|-|-
checked: boolean|MatSlideToggle新的选中状态。|The new checked value of the MatSlideToggle.
source: MatSlideToggle|传递事件的源MatSlideToggle。|The source MatSlideToggle of the event.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*