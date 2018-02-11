[toc]

# 概述

> OVERVIEW

`<mat-progress-spinner>`和`<mat-spinner>`是一个进度与活动的环形指示器。

> `<mat-progress-spinner>` and `<mat-spinner>` are a circular indicators of progress and activity.

## 进度模式

> Progress mode

进度旋转器支持两种模式，“确定”（"determinate"）与“未决”（"indeterminate"）。`<mat-spinner>`组件是`<mat-progress-spinner mode="indeterminate">`的一个别名。

> The progress-spinner supports two modes, "determinate" and "indeterminate". The `<mat-spinner>` component is an alias for `<mat-progress-spinner mode="indeterminate">`.

模式|描述|Description
-|-|-
determinate|标准进度指示器，从0%到100%|Standard progress indicator, fills from 0% to 100%
indeterminate|指示了某件事正在进行，不表示具体的进度。|Indicates that something is happening without conveying a discrete progress

默认的模式是“确定”（"determinate"）。在此模式下，可通过`value`属性设置进度，取值为0到100内的整数。

> The default mode is "determinate". In this mode, the progress is set via the `value` property, which can be a whole number between 0 and 100.

在“未决”（"indeterminate"）模式下，`value`属性被忽略。

> In "indeterminate" mode, the `value` property is ignored.

## 主题

> Theming

使用`color`属性可以改变进度旋转器的颜色。进度旋转器模式使用主题的主色调。可改变为`'accent'`或者`'warn'`。

> The color of a progress-spinner can be changed by using the `color` property. By default, progress-spinners use the theme's primary color. This can be changed to `'accent'` or `'warn'`.

## 无障碍

> Accessibility

每个进度旋转器都需要通过`aria-label`或者`aria-labelledby`指定一个有意义的标签。

> Each progress spinner should be given a meaningful label via `aria-label` or `aria-labelledby`.

# API

## 进度旋转器(progress-spinner)的API参考

> API reference for Angular Material progress-spinner

```ts
import {MatProgressSpinnerModule} from '@angular/material/progress-spinner';
```

## 指令

> Directives

### MatProgressSpinner

组件。

> component.

Selector: `mat-progress-spinner`

Exported as: `matProgressSpinner`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>diameter: number|进度旋转器的直径（将设置svg的宽度和高度）。|The diameter of the progress spinner (will set width and height of svg).
@Input()<br>mode: ProgressSpinnerMode|进度环的模式。|Mode of the progress circle
@Input()<br>strokeWidth: number|进度旋转器的线宽。|Stroke width of the progress spinner.
@Input()<br>value: number|进度环的值。|Value of the progress circle.
color: ThemePalette|组件的主题色。|Theme color palette for the component.

### MatSpinner **extends MatProgressSpinner**

组件

> component.

用于快速创建未决实例的一个组件定义。

> This is a component definition to be used as a convenience reference to create an indeterminate instance.

Selector: `mat-spinner`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>diameter: number|进度旋转器的直径（将设置svg的宽度和高度）。|The diameter of the progress spinner (will set width and height of svg).
@Input()<br>mode: ProgressSpinnerMode|进度环的模式。|Mode of the progress circle
@Input()<br>strokeWidth: number|进度旋转器的线宽。|Stroke width of the progress spinner.
@Input()<br>value: number|进度环的值。|Value of the progress circle.
color: ThemePalette|组件的主题色。|Theme color palette for the component.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*