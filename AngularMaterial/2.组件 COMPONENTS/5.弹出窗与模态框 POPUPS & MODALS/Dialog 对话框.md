[toc]

# 概述

> OVERVIEW

使用MdDialog服务，可以打开包含Material Design样式与动画的对话框。

> The MdDialog service can be used to open modal dialogs with Material Design styling and animations.

传入一个需要加载的组件和可选配置对象，调用open方法可以打开对话框。open方法将返回一个MdDialogRef实例。

> A dialog is opened by calling the open method with a component to be loaded and an optional config object. The open method will return an instance of MdDialogRef:

```typescript
let dialogRef = dialog.open(UserProfileComponent, {
  height: '400px',
  width: '600px',
});
```

MdDialogRef提供了对已打开对话框的操作。可以用来关闭对话框，并在关闭后接收通知。

> The MdDialogRef provides a handle on the opened dialog. It can be used to close the dialog and to receive notification when the dialog has been closed.

```typescript
dialogRef.afterClosed().subscribe(result => {
  console.log(`Dialog result: ${result}`); // Pizza!
});

dialogRef.close('Pizza!');
```

由MdDialog创建的组件可以*注入*MdDialogRef，并使用MdDialogRef关闭其包含的对话框。当对话框关闭时，可以提供一个可选结果值。这个结果值被转发为afterClosed promise的结果。

> Components created via MdDialog can *inject* MdDialogRef and use it to close the dialog in which they are contained. When closing, an optional result value can be provided. This result value is forwarded as the result of the afterClosed promise.

## 用对话框组件共享数据

> Sharing data with the Dialog component.

如果你想使用对话框共享数据，你可以使用data选项向对话框组件传入信息。

> If you want to share data with your dialog, you can use the data option to pass information to the dialog component.

```typescript
let dialogRef = dialog.open(YourDialog, {
  data: 'your data',
});
```

使用MD_DIALOG_DATA注入token来访问对话框组件中的数据。

> To access the data in your dialog component, you have to use the MD_DIALOG_DATA injection token:

```typescript
import {Component, Inject} from '@angular/core';
import {MD_DIALOG_DATA} from '@angular/material';

@Component({
  selector: 'your-dialog',
  template: 'passed in {{ data }}',
})

export class YourDialog {
  constructor(@Inject(MD_DIALOG_DATA) public data: any) { }
}
```

## 对话框内容

> Dialog content

使用一些指令可以使你更容易地结构化对话框的内容。

> Several directives are available to make it easier to structure your dialog content:

指令名|描述|Description
-|-|-
md-dialog-title|[属性]对话框标题设置为标题元素(例如&lt;h1&gt;, &lt;h2&gt;)|[Attr] Dialog title, applied to a heading element (e.g., &lt;h1&gt;, &lt;h2&gt;)
<md-dialog-content>|对话框的基本可滚动内容|Primary scrollable content of the dialog
<md-dialog-actions>|对话框底部的操作按钮容器|Container for action buttons at the bottom of the dialog
md-dialog-close|[属性]在<button>中添加，使按钮使用绑定的可选结果值来关闭对话框|[Attr] Added to a <button>, makes the button close the dialog with an optional result from the bound value.

例如：

> For example:

```html
<h2 md-dialog-title>Delete all</h2>
<md-dialog-content>Are you sure?</md-dialog-content>
<md-dialog-actions>
  <button md-button md-dialog-close>No</button>
  <!-- Can optionally provide a result for the closing dialog. -->
  <button md-button [md-dialog-close]="true">Yes</button>
</md-dialog-actions>
```

当对话框打开时，焦点将会自动放在第一个可选择的元素上。

> Once a dialog opens, the dialog will automatically focus the first tabbable element.

你可以通过设置tabindex属性，来控制元素在按tab时是否停下。

> You can control which elements are tab stops with the tabindex attribute

```html
<button md-button tabindex="-1">Not Tabbable</button>
```

## AOT编译

> AOT Compilation

因为MdDialog的动态特性和即时使用ViewContainerRef#createComponent()创建组件的用法，AOT编译器将默认不会为你的对话框组件创建ComponentFactory属性。

> Due to the dynamic nature of the MdDialog, and its usage of ViewContainerRef#createComponent() to create the component on the fly, the AOT compiler will not know to create the proper ComponentFactory for your dialog component by default.

你必须在模块定义的entryComponents列表中引入你的对话框类，这样AOT编译器才知道为其创建ComponentFactory属性。

> You must include your dialog class in the list of entryComponents in your module definition so that the AOT compiler knows to create the ComponentFactory for it.

```typescript
@NgModule({
  imports: [
    // ...
    MdDialogModule
  ],

  declarations: [
    AppComponent,
    ExampleDialogComponent
  ],

  entryComponents: [
    ExampleDialogComponent
  ]

  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule() {}
```

# API

## 对话框(dialog)的API参考

> API reference for Angular Material dialog

```typescript
import {MdDialogModule} from '@angular/material';
```

## 服务

> Services

### MdDialog

打开Material Design模态对话框的服务。

> Service to open Material Design modal dialogs.

**属性**

> Properties

属性名|描述|Description
-|-|-
afterOpen|获取一个观察对话框已打开的对象|Gets an observable that is notified when a dialog has been opened.
afterAllClosed|获取一个观察所有对话框已关闭的对象|Gets an observable that is notified when all open dialog have finished closing.

**方法**

> Methods

open
---
- 描述
  - 打开包含指定组件的对话框。
- 参数
  - componentOrTemplateRef (ComponentType<T> | TemplateRef<T>)：加载进对话框的组件类型，或者是作为对话框内容实例化的TemplateRef
  - config? (MdDialogConfig)：附加设置。
- 返回
  - MdDialogRef<T>：新打开的对话框参考。

closeAll
---
- 描述
  - 关闭所有当前已打开的对话框。

## 指令

> Directives

### MdDialogClose

关闭当前对话框的按钮。

> Button that will close the current dialog.

Selector: button[md-dialog-close]

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('aria-label')<br>ariaLabel|按钮的屏幕阅读标签。|Screenreader label for the button.
@Input('md-dialog-close')<br>dialogResult|对话框关闭input。|Dialog close input.

### MdDialogTitle

对话框元素的标题。当滚动时也固定在对话框顶部。

> Title of a dialog element. Stays fixed to the top of the dialog when scrolling.

Selector: [md-dialog-title] [mdDialogTitle]

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>id||

### MdDialogContent

对话框的可滚动内容的容器。

> Scrollable content container of a dialog.

Selector: [md-dialog-content] md-dialog-content

### MdDialogActions

对话框底部操作按钮的容器。当滚动时也固定在底部。

> Container for the bottom action buttons in a dialog. Stays fixed to the bottom when scrolling.

Selector: [md-dialog-actions] md-dialog-actions

## 附加类

> Additional classes

### MdDialogConfig

用MdDialog服务打开的模态对话框的配置。

> Configuration for opening a modal dialog with the MdDialog service.

**属性**

> Properties

属性名|描述|Description
-|-|-
viewContainerRef|定义所属的组件在Angular逻辑组件树的位置。将影响注入的那些可以使用，并改变对话框中已实例化组件的监测顺序。不影响对话框内容在哪里渲染。|Where the attached component should live in Angular's logical component tree. This affects what is available for injection and the change detection order for the component instantiated inside of the dialog. This does not affect where the dialog content will be rendered.
role|对话框元素的ARIA角色。|The ARIA role the dialog element.
panelClass|遮盖面板的自定义类。|Custom class for the overlay pane.
hasBackdrop|对话框是否有背景。|Whether the dialog has a backdrop.
backdropClass|自定义背景类。|Custom class for the backdrop,
disableClose|用户是否可以按Esc或者点击对话框外部来关闭一个对话框。|Whether the user can use escape or clicking outside to close a modal.
width|对话框宽度。|Width of the dialog.
height|对话框高度。|Height of the dialog.
position|位置重载。|Position overrides.
data|注入子组件的数据。|Data being injected into the child component.
direction|对话框内容的布局方向。|Layout direction for the dialog's content.
ariaDescribedBy|描述对话框的元素的id。|ID of the element that describes the dialog.

### MdDialogRef

> Reference to a dialog opened via the MdDialog service.

**属性**

> Properties

属性名|描述|Description
-|-|-
componentInstance|对话框中的组件实例。|The instance of component opened into the dialog.
disableClose|是否允许用户关闭对话框。|Whether the user is allowed to close the dialog.

**方法**

> Methods

方法名|描述|Description
-|-|-
close|关闭对话框。|Close the dialog.
afterClosed|获取观察对话框关闭完成的对象|Gets an observable that is notified when the dialog is finished closing.
updatePosition|更新对话框位置。|Updates the dialog's position.
updateSize|更新对话框宽度与高度。|Updates the dialog's width and height.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*