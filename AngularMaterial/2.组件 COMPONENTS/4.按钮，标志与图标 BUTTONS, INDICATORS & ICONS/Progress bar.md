[toc]

# 概述

> OVERVIEW

`<mat-progress-bar>`是一个指示进度和活动的水平的进度条。

> `<mat-progress-bar>` is a horizontal progress-bar for indicating progress and activity.

## 进度模式

> Progress mode

进度条支持四种模式：determinate, indeterminate, buffer和query.

> The progress-bar supports four modes: determinate, indeterminate, buffer and query.

### Determinate

已知操作完成度的百分比的，应使用此模式。

> Operations where the percentage of the operation complete is known should use the determinate indicator.

此模式为默认模式，使用`value`属性来表示进度值。

> This is the default mode and the progress is represented by the `value` property.

### Indeterminate

当需要用户等待某操作完成，并且不需要指示具体需要多久时，应使用此模式。

> Operations where the user is asked to wait while something finishes and it’s not necessary to indicate how long it will take should use the indeterminate indicator.

在此模式下将忽略`value`属性。

> In this mode the `value` property is ignored.

### Buffer

当用户希望指示一些活动或者从服务器加载等，可以使用此模式。

> Operations where the user wants to indicate some activity or loading from the server, use the buffer indicator.

在buffer模式下，`value`值决定主进度条的进度，`bufferValue`用于展示附加缓冲进度。

> In "buffer" mode, `value` determines the progress of the primary bar while the `bufferValue` is used to show the additional buffering progress.

### Query

当用户希望指示预加载（直到可以开始加载），使用此模式。

> For situations where the user wants to indicate pre-loading (until the loading can actually be made), use the query indicator.

在query模式下，进度条渲染为反向的indeterminate条。当响应进度条可用时，模式`mode`应变为determinate来表达进度。在此模式下`property`属性忽略。

> In "query" mode, the progress-bar renders as an inverted "indeterminate" bar. Once the response progress is available, the `mode` should be changed to determinate to convey the progress. In this mode the `property` is ignored.

## 主题

> Theming

可使用`color`属性来改变进度条的颜色。进度条默认使用主体的主色调。也可改变为`'accent'`或者`'warn'`。

> The color of a progress-bar can be changed by using the `color` property. By default, progress-bars use the theme's primary color. This can be changed to `'accent'` or `'warn'`.

## 无障碍

> Accessibility

每个进度条都应该通过`aria-label`或者`aria-labelledby`指定一个有意义的标签。

> Each progress bar should be given a meaningful label via `aria-label` or `aria-labelledby`.

# API

## 进度条(progress-bar)的API参考

> API reference for Angular Material progress-bar

```typescript
import {MatProgressBarModule} from '@angular/material/progress-bar';
```

## 指令

> Directives

### MatProgressBar

`<mat-progress-bar>`组件。

> `<mat-progress-bar>` component.

Selector: `mat-progress-bar`

Exported as: `matProgressBar`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>bufferValue: number|进度条的缓冲值。默认为0。|Buffer value of the progress bar. Defaults to zero.
@Input()<br>color: ThemePalette|组件的主题色调。|Theme color palette for the component.
@Input()<br>mode: 'determinate' \| 'indeterminate' \| 'buffer' \| 'query'|进度条模式。必须为四个值其中一个，默认为determinate。|Mode of the progress bar.Input must be one of these values: determinate, indeterminate, buffer, query, defaults to 'determinate'. Mirrored to mode attribute.
@Input()<br>value: number|进度条的值。默认为0。|Value of the progress bar. Defaults to zero. Mirrored to aria-value now.
progressbarId: `mat-progress-bar-${progressbarId++}`|进度条的id。|The id of the progress bar.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*