[toc]

# 概述

> OVERVIEW

`MatSnackBar`是一个用于展示snack-bar通知的服务。

> `MatSnackBar` is a service for displaying snack-bar notifications.

## 打开snack-bar

> Opening a snack-bar

一个snack-bar中可以包含任意字符串信息或者一个指定的组件。

> A snack-bar can contain either a string message or a given component.

```ts
// Simple message.
let snackBarRef = snackBar.open('Message archived');

// Simple message with an action.
let snackBarRef = snackBar.open('Message archived', 'Undo');

// Load the given component into the snack-bar.
let snackBarRef = snackbar.openFromComponent(MessageArchivedComponent);
```

无论使用哪种方式，都会返回一个`MatSnackBarRef`。此对象可用于撤回snack-bar或者接收snack-bar撤回时的通知。对于包含一个操作的简单消息，`MatSnackBarRef`暴露了一个操作触发时的observable。如果想关闭一个通过`openFromComponent`打开的自定义snack-bar，可以将`MatSnackBarRef`注入到组件内部。

> In either case, a `MatSnackBarRef` is returned. This can be used to dismiss the snack-bar or to receive notification of when the snack-bar is dismissed. For simple messages with an action, the `MatSnackBarRef` exposes an observable for when the action is triggered. If you want to close a custom snack-bar that was opened via `openFromComponent`, from within the component itself, you can inject the `MatSnackBarRef`.

```ts
snackBarRef.afterDismissed().subscribe(() => {
  console.log('The snack-bar was dismissed');
});


snackBarRef.onAction().subscribe(() => {
  console.log('The snack-bar action was triggered!');
});

snackBarRef.dismiss();
```

## 撤回

> Dismissal

调用`open`方法会返回`MatSnackBarRef`对象，一个snack-bar可通过调用此对象的`dismiss`方法来手动撤回。

> A snack-bar can be dismissed manually by calling the `dismiss` method on the `MatSnackBarRef` returned from the call to `open`.

同一时间只能打开一个snack-bar。如果新的snack-bar打开时，前一个消息还在展示中，则前一个消息将自动撤回。

> Only one snack-bar can ever be opened at one time. If a new snackbar is opened while a previous message is still showing, the older message will be automatically dismissed.

snack-bar可以通过可选设置对象指定一个延时。

> A snack-bar can also be given a duration via the optional configuration object:

```ts
snackbar.open('Message archived', 'Undo', {
  duration: 3000
});
```

## 使用自定义snack-bar共享数据

> Sharing data with a custom snack-bar

你可以通过自定义snack-bar共享数据，只需通过`openFromComponent`方法打开并传入`data`属性。

> You can share data with the custom snack-bar, that you opened via the `openFromComponent` method, by passing it through the `data` property.

```ts
snackbar.openFromComponent(MessageArchivedComponent, {
  data: 'some data'
});
```

通过使用`MAT_SNACK_BAR_DATA`注入token，可以访问组件中的数据。

> To access the data in your component, you have to use the `MAT_SNACK_BAR_DATA` injection token:

```ts
import {Component, Inject} from '@angular/core';
import {MAT_SNACK_BAR_DATA} from '@angular/material';

@Component({
  selector: 'your-snack-bar',
  template: 'passed in {{ data }}',
})
export class MessageArchivedComponent {
  constructor(@Inject(MAT_SNACK_BAR_DATA) public data: any) { }
}
```

## 无障碍

> Accessibility

Snack-bar信息通过`aria-live`区申明。焦点并不移动到snack-bar元素上，因为这样对于工作流中的用户来说是一种打断。对于snack-bar中提供的任何操作来说，应用都应该提供给用户一个可选择的方式来执行这个操作（通常是键盘快捷键）。

> Snack-bar messages are announced via an `aria-live` region. Focus is not moved to the snack-bar element, as this would be disruptive to a user in the middle of a workflow. For any action offered in the snack-bar, the application should offer the user an alternative way to perform the action (typically via keyboard shortcut).

# API

## snack-bar的API参考

> API reference for Angular Material snack-bar

```ts
import {MatSnackBarModule} from '@angular/material/snack-bar';
```

## 服务

> Services

### MatSnackBar

用于调度Material Design snack bar消息的服务

> Service to dispatch Material Design snack bar messages.

**方法**

> Methods

dismiss
---
- 描述
  - 撤回当前显示的snack bar。
  - Dismisses the currently-visible snack bar.

open
---
- 描述
  - 打开一个包含消息与可选操作的snackbar。
  - Opens a snackbar with a message and an optional action.
- 参数
  - message: string：在snackbar中展示的消息
  - The message to show in the snackbar.
  - action: string = ''：snackbar操作的标签
  - The label for the snackbar action.
  - config?: MatSnackBarConfig：snackbar的附加配置项
  - Additional configuration options for the snackbar.
- 返回
  - MatSnackBarRef<SimpleSnackBar>

openFromComponent
---
- 描述
  - 创建并调度一个包含自定义组件的snack bar，并会移除当前任何打开的snack bar。
  - Creates and dispatches a snack bar with a custom component for the content, removing any currently opened snack bars.
- 参数
  - component: ComponentType<T>：需要初始化的组件	
  - Component to be instantiated.
  - config?: MatSnackBarConfig：snack bar的额外配置
  - Extra configuration for the snack bar.
- 返回
  - MatSnackBarRef<T>

## 指令

> Directives

### SimpleSnackBar

用于打开默认适配于material的snack bar的组件。只能用于snack bar服务的内部。

> A component used to open as the default snack bar, matching material spec. This should only be used internally by the snack bar service.

Selector: `simple-snack-bar`

**属性**

> Properties

属性名|描述|Description
-|-|-
data: { message: string; action: string; }|注入snack bar的数据。|Data that was injected into the snack bar.
hasAction: boolean|操作按钮是否显示。|If the action button should be shown.
snackBarRef: MatSnackBarRef<SimpleSnackBar>||

**方法**

> Methods

方法名|描述|Description
-|-|-
action|执行snack bar的操作。|Performs the action on the snack bar.

## 附加类

> Additional classes

### MatSnackBarConfig

打开snack-bar时使用的配置。

> Configuration used when opening a snack-bar.

**属性**

> Properties

属性名|描述|Description
-|-|-
announcementMessage: string|MatAriaLiveAnnouncer申明的信息。|Message to be announced by the MatAriaLiveAnnouncer
data: any|注入子组件的数据。|Data being injected into the child component.
direction: Direction|snack bar的文字布局方向。|Text layout direction for the snack bar.
duration: number|snack bar自动撤回时长，单位毫秒。|The length of time in milliseconds to wait before automatically dismissing the snack bar.
horizontalPosition: MatSnackBarHorizontalPosition|snack bar的水平位置。|The horizontal position to place the snack bar.
panelClass: string \| string[]|snack bar容器的额外CSS类。|Extra CSS classes to be added to the snack bar container.
politeness: AriaLivePoliteness|MatAriaLiveAnnouncer申明的优先级。|The politeness level for the MatAriaLiveAnnouncer announcement.
verticalPosition: MatSnackBarVerticalPosition|snack bar的垂直位置。|The vertical position to place the snack bar.
viewContainerRef: ViewContainerRef|放置snack bar的可视容器。|The view container to place the overlay for the snack bar into.
Deprecated<br>extraClasses: string \| string[]|snack bar容器的额外CSS类。|Extra CSS classes to be added to the snack bar container.

### MatSnackBarRef

snack bar服务调度的snack bar的参考。

> Reference to a snack bar dispatched from the snack bar service.

**属性**

> Properties

属性名|描述|Description
-|-|-
instance: T|snack bar内容的组件实例。|The instance of the component making up the content of the snack bar.

**方法**

> Methods

方法名|描述|Description|返回
-|-|-|-
afterDismissed|获取当snack bar撤回完成时的observable。|Gets an observable that is notified when the snack bar is finished closing.|Observable<void>
afterOpened|获取当snack bar打开并出现时的observable。|Gets an observable that is notified when the snack bar has opened and appeared.|Observable<void>
closeWithAction|标记snack bar操作已点击。|Marks the snackbar action clicked.|
dismiss|撤回当前snack bar。|Dismisses the snack bar.|
onAction|获取当snack bar操作被调用时的observable。|Gets an observable that is notified when the snack bar action is called.|

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*