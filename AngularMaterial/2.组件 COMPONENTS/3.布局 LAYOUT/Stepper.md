[toc]

# 概述

> OVERVIEW

Angular Material的stepper通过分割内容为逻辑的步骤，提供了一个向导式的工作流。

> Angular Material's stepper provides a wizard-like workflow by dividing content into logical steps.

Material的stepper基于CDK stepper构建，用于驱动步骤化的工作流逻辑。Material stepper继承了CDK stepper并应用了Material Design样式。

> Material stepper builds on the foundation of the CDK stepper that is responsible for the logic that drives a stepped workflow. Material stepper extends the CDK stepper and has Material Design styling.

## Stepper变量

> Stepper variants

stepper组件有两种：`mat-horizontal-stepper`和`mat-vertical-stepper`。使用方法相同，不同的只是方向。`mat-horizontal-stepper`选择器可用于创建水平的stepper，`mat-vertical-stepper`用于创建垂直的stepper。`mat-step`组件可放置于这两者其一之中。

> There are two stepper components: `mat-horizontal-stepper` and `mat-vertical-stepper`. They can be used the same way. The only difference is the orientation of stepper. `mat-horizontal-stepper` selector can be used to create a horizontal stepper, and `mat-vertical-stepper` can be used to create a vertical stepper. `mat-step` components need to be placed inside either one of the two stepper components.

## 标签

> Labels

如果step的标签只是文本，可使用`label`属性。

> If a step's label is only text, then the `label` attribute can be used.

```html
<mat-vertical-stepper>
  <mat-step label="Step 1">
    Content 1
  </mat-step>
  <mat-step label="Step 1">
    Content 2
  </mat-step>
</mat-vertical-stepper>
```

如果是更复杂的标签，可在`mat-step`中使用`matStepLabel`指令添加一个模板。

> For more complex labels, add a template with the `matStepLabel` directive inside the `mat-step`.

```html
<mat-vertical-stepper>
  <mat-step>
    <ng-template matStepLabel>...</ng-template>
    ...
  </mat-step>
</mat-vertical-stepper>
```

## Stepper按钮

> Stepper buttons

共有两个按钮来支持不同step之间的导航：`matStepperPrevious`和`matStepperNext`。

> There are two button directives to support navigation between different steps: `matStepperPrevious` and `matStepperNext`.

```html
<mat-horizontal-stepper>
  <mat-step>
    ...
    <div>
      <button mat-button matStepperPrevious>Back</button>
      <button mat-button matStepperNext>Next</button>
    </div>
  </mat-step>
</mat-horizontal-stepper>
```

## 线性stepper

> Linear stepper

可在`mat-horizontal-stepper`和`mat-vertical-stepper`中设置`linear`属性来创建一个线性的stepper，要求用户在进入后续步骤前需要完成前面的步骤。对于每个`mat-step`，可在顶级`AbstractControl`中设置`stepControl`属性，用于校验每一步。

> The `linear` attribute can be set on `mat-horizontal-stepper` and `mat-vertical-stepper` to create a linear stepper that requires the user to complete previous steps before proceeding to following steps. For each `mat-step`, the `stepControl` attribute can be set to the top level `AbstractControl` that is used to check the validity of the step.

有两种方法，一种是使用单个表单，一种是每个step使用不同的表单。

> There are two possible approaches. One is using a single form for stepper, and the other is using a different form for each step.

如果你不想使用Angular表单，可以在每个step传入`completed`属性，当此属性为`true`时，才允许用户继续下去。注意如果`completed`和`stepControl`都设置了，`stepControl`优先级高。

> Alternatively, if you don't want to use the Angular forms, you can pass in the `completed` property to each of the steps which won't allow the user to continue until it becomes `true`. Note that if both `completed` and `stepControl` are set, the `stepControl` will take precedence.

### 使用单个表单

> Using a single form

当在stepper使用单个表单时，`matStepperPrevious`和`matStepperNext`需要设置`type="button"`来防止在所有步骤完成前表单被提交。

> When using a single form for the stepper, `matStepperPrevious` and `matStepperNext` have to be set to `type="button"` in order to prevent submission of the form before all steps are completed.

```html
<form [formGroup]="formGroup">
  <mat-horizontal-stepper formArrahttps://www.yahacode.com/wp-admin/post-new.php?post_type=pageyName="formArray" linear>
    <mat-step formGroupName="0" [stepControl]="formArray.get([0])">
      ...
      <div>
        <button mat-button matStepperNext type="button">Next</button>
      </div>
    </mat-step>
    <mat-step formGroupName="1" [stepControl]="formArray.get([1])">
      ...
      <div>
        <button mat-button matStepperPrevious type="button">Back</button>
        <button mat-button matStepperNext type="button">Next</button>
      </div>
    </mat-step>
    ...
  </mat-horizontal-stepper>
</form>
```

### 每个step使用不同的表单

> Using a different form for each step

```html
<mat-vertical-stepper linear>
  <mat-step [stepControl]="formGroup1">
    <form [formGroup]="formGroup1">
      ...
    </form>
  </mat-step>
  <mat-step [stepControl]="formGroup2">
    <form [formGroup]="formGroup2">
      ...
    </form>
  </mat-step>
</mat-vertical-stepper>
```

## step类型

> Types of steps

### 可选的step

> Optional step

如果在线性stepper中的step非必须完成，可在`mat-step`中设置`optional`属性。

> If completion of a step in linear stepper is not required, then the `optional` attribute can be set on `mat-step`.

### 可编辑step

> Editable step

默认step都是可编辑的，意味着用户可以返回上一个已完成的步骤然后进行编辑。可在`mat-step`中设置`editable="true"`来改变默认设置。

> By default, steps are editable, which means users can return to previously completed steps and edit their responses. `editable="true"` can be set on `mat-step` to change the default.

### 完成的step

> Completed step

如果step有效（在线性stepper中）且用户与此step有交互，则默认step的`completed`会返回`true`。用户也可以根据需要设置`completed`属性来复写默认的`completed`行为。

> By default, the `completed` attribute of a step returns `true` if the step is valid (in case of linear stepper) and the user has interacted with the step. The user, however, can also override this default `completed` behavior by setting the `completed` attribute as needed.

### 复写图标

> Overriding icons

默认step的头部会通过`<mat-icon>`元素使用Material design图标集中的`create`和`done`图标。如果想提供不同的图标，你可以在想要复写的图标中添加`matStepperIcon`。

> By default, the step headers will use the `create` and `done` icons from the Material design icon set via `<mat-icon>` elements. If you want to provide a different set of icons, you can do so by placing a `matStepperIcon` for each of the icons that you want to override:

```html
<mat-vertical-stepper>
  <ng-template matStepperIcon="edit">
    <custom-icon>edit</custom-icon>
  </ng-template>

  <ng-template matStepperIcon="done">
    <custom-icon>done</custom-icon>
  </ng-template>

  <!-- Stepper steps go here -->
</mat-vertical-stepper>
```

## 键盘交互

> Keyboard interaction

- 左方向键：前一个step头部获得焦点
- 右方向键：下一个step头部获得焦点
- 回车，空格：选中当前获取焦点的step
- TAB：使下一个可选的元素获得焦点
- TAB+SHIFT：使上一个可选的元素获得焦点

> - LEFT_ARROW: Focuses the previous step header
> - RIGHT_ARROW: Focuses the next step header
> - ENTER, SPACE: Selects the step that the focus is currently on
> - TAB: Focuses the next tabbable element
> - TAB+SHIFT: Focuses the previous tabbable element

## 本地化标签

> Localizing labels

可通过`MatStepperIntl`提供stepper使用的标签。可通过在根模块提供一个包括已翻译值的子类来完成本地化的信息。

> Labels used by the stepper are provided through `MatStepperIntl`. Localization of these messages can be done by providing a subclass with translated values in your application root module.

```ts
@NgModule({
  imports: [MatStepperModule],
  providers: [
    {provide: MatStepperIntl, useClass: MyIntl},
  ],
})
export class MyApp {}
```

## 无障碍

> Accessibility

stepper被当作一个标签页视图的无障碍方案，所以默认为`role="tablist"`。step的头部被点击后可被选中，所以为`role="tab"`，选中后可展开的内容指定为`role="tabpanel"`。step头部的`aria-selected`属性和step内容的`aria-expanded`属性可基于step改变而自动设置。

> The stepper is treated as a tabbed view for accessibility purposes, so it is given `role="tablist"` by default. The header of step that can be clicked to select the step is given `role="tab"`, and the content that can be expanded upon selection is given `role="tabpanel"`. `aria-selected` attribute of step header and `aria-expanded` attribute of step content is automatically set based on step selection change.

stepper和每一个step都需要通过`aria-label`或者`aria-labelledby`来指定一个有意义的标签。

> The stepper and each step should be given a meaningful label via `aria-label` or `aria-labelledby`.

# API

## stepper的API参考

> API reference for Angular Material stepper

```ts
import {MatStepperModule} from '@angular/material/stepper';
```

## 服务

> Services

### MatStepperIntl

需要国际化的stepper数据。

> Stepper data that is required for internationalization.

**属性**

> Properties

属性名|描述|Description
-|-|-
changes: Subject<void\>|当标签发生改变的事件。用于初始化后通知组件标签发生变化。|Stream that emits whenever the labels here are changed. Use this to notify components if the labels have changed after initialization.
optionalLabel: string|在可选step下渲染的标签。|Label that is rendered below optional steps.

## 指令

> Directives

### MatStepLabel **extends CdkStepLabel**

Selector: `[matStepLabel]`

### MatStep **extends CdkStep**

Selector: `mat-step`

Exported as: `matStep`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>completed: boolean|step是否标记为已完成。|Whether step is marked as completed.
@Input()<br>editable: boolean|step被标记为已完成后，用户是否可退回至此step。|Whether the user can return to this step once it has been marked as complted.
@Input()<br>label: string|step的标签。|Label of the step.
@Input()<br>optional: boolean|step的完成为可选。|Whether the completion of step is optional.
@Input()<br>stepControl: AbstractControl|step的顶级抽象控件。|The top level abstract control of the step.
content: TemplateRef<any\>|step内容的模板。|Template for step content.
interacted: false|用户是否已打开可扩展的step内容。|Whether user has seen the expanded step content or not.
stepLabel: MatStepLabel|<ng-template matStepLabel\>指定的step标签内容。|Content for step label given by <ng-template matStepLabel\>.

**方法**

> Methods

isErrorState
---
- 描述
  - 自定义的错误状态匹配器，用于验证交互表单。
  - Custom error state matcher that additionally checks for validity of interacted form.
- 参数
  - control: FormControl | null	
  - form: FormGroupDirective | NgForm | null	
- 返回
 - boolean

reset
---
- 描述
  - 重置step为初始状态。也会重置表单数据。
  - Resets the step to its initial state. Note that this includes resetting form data.

select
---
- 描述
  - 选择step组件。
  - Selects this step component.

### MatStepper **extends CdkStepper**

Selector: `[matStepper]`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>linear: boolean|是否校验前面的step。|Whether the validity of previous steps should be checked or not.
@Input()<br>selected: CdkStep|已选择的step。|The step that is selected.
@Input()<br>selectedIndex: number|已选择的step的索引。|The index of the selected step.
@Output()<br>selectionChange: EventEmitter<StepperSelectionEvent\>|选择的step发生改变的事件。|Event emitted when the selected step has changed.

**方法**

> Methods

next
---
- 描述
  - 选中并使下一个step获得焦点。
  - Selects and focuses the next step in list.

previous
---
- 描述
  - 选中并使上一个step获得焦点。
  - Selects and focuses the previous step in list.

reset
---
- 描述
  - 重置step为初始状态。也会重置表单数据。
  - Resets the stepper to its initial state. Note that this includes clearing form data.

### MatHorizontalStepper **extends MatStepper**

Selector: `mat-horizontal-stepper`

Exported as: `matHorizontalStepper`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>linear: boolean|是否校验前面的step。|Whether the validity of previous steps should be checked or not.
@Input()<br>selected: CdkStep|已选择的step。|The step that is selected.
@Input()<br>selectedIndex: number|已选择的step的索引。|The index of the selected step.
@Output()<br>selectionChange: EventEmitter<StepperSelectionEvent\>|选择的step发生改变的事件。|Event emitted when the selected step has changed.

**方法**

> Methods

next
---
- 描述
  - 选中并使下一个step获得焦点。
  - Selects and focuses the next step in list.

previous
---
- 描述
  - 选中并使上一个step获得焦点。
  - Selects and focuses the previous step in list.

reset
---
- 描述
  - 重置step为初始状态。也会重置表单数据。
  - Resets the stepper to its initial state. Note that this includes clearing form data.

### MatVerticalStepper **extends MatStepper**

Selector: `mat-vertical-stepper`

Exported as: `matVerticalStepper`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>linear: boolean|是否校验前面的step。|Whether the validity of previous steps should be checked or not.
@Input()<br>selected: CdkStep|已选择的step。|The step that is selected.
@Input()<br>selectedIndex: number|已选择的step的索引。|The index of the selected step.
@Output()<br>selectionChange: EventEmitter<StepperSelectionEvent\>|选择的step发生改变的事件。|Event emitted when the selected step has changed.

**方法**

> Methods

next
---
- 描述
  - 选中并使下一个step获得焦点。
  - Selects and focuses the next step in list.

previous
---
- 描述
  - 选中并使上一个step获得焦点。
  - Selects and focuses the previous step in list.

reset
---
- 描述
  - 重置step为初始状态。也会重置表单数据。
  - Resets the stepper to its initial state. Note that this includes clearing form data.

### MatStepperNext **extends CdkStepperNext**

用于在step工作流中移动到下一个step的按钮。

> Button that moves to the next step in a stepper workflow.

Selector: `button[matStepperNext]`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>type: string|按钮的类型。如果不指定，默认为"submit"。|Type of the next button. Defaults to "submit" if not specified.

### MatStepperPrevious **extends CdkStepperPrevious**

用于在step工作流中移动到上一个step的按钮。

> Button that moves to the previous step in a stepper workflow.

Selector: `button[matStepperPrevious]`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>type: string|按钮的类型。如果不指定，默认为"submit"。|Type of the previous button. Defaults to "submit" if not specified.

### MatStepHeader

Selector: `mat-step-header`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>active: boolean|指定的step标签是否为激活。|Whether the given step label is active.
@Input()<br>iconOverrides: { [key: string]: TemplateRef<any\>; }|复写头部图标，通过stepper传入。|Overrides for the header icons, passed in via the stepper.
@Input()<br>index: number|指定的step索引。|Index of the given step.
@Input()<br>label: MatStepLabel \| string|指定的step标签。|Label of the given step.
@Input()<br>optional: boolean|指定的step是否为可选。|Whether the given step is optional.
@Input()<br>selected: boolean|指定的step是否为选中。|Whether the given step is selected.
@Input()<br>state: string|指定的step状态。|State of the given step.

### MatStepperIcon

step头部内的用于复写图标的模板。

> Template to be used to override the icons inside the step header.

Selector: `ng-template[matStepperIcon]`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('matStepperIcon')<br>name: 'edit' \| 'done'|被复写的图标名称。|Name of the icon to be overridden.
templateRef: TemplateRef<any\>||

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*