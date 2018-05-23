[toc]

# 概述

> OVERVIEW

`MatBottomSheet`可用于在屏幕底部打开Material Design面板。此面板主要用于移动设备的交互，可当作对话框或者菜单的备选方案。

> The `MatBottomSheet` service can be used to open Material Design panels to the bottom of the screen. These panels are intended primarily as an interaction on mobile devices where they can be used as an alternative to dialogs and menus.

你可以通过调用`open`方法来打开一个底部菜单，加载一个组件与一个可选的配置对象。`open`方法将返回一个`MatBottomSheetRef`实例：

> You can open a bottom sheet by calling the `open` method with a component to be loaded and an optional config object. The `open` method will return an instance of `MatBottomSheetRef`:

```ts
const bottomSheetRef = bottomSheet.open(SocialShareComponent, {
  ariaLabel: 'Share on social media'
});
```

`MatBottomSheetRef`是当前打开的底部菜单的一个参考，可用来关闭或者订阅事件。注意同时只能打开一个底部菜单。任何在底部菜单内部的组件都可以注入`MatBottomSheetRef`。

> The `MatBottomSheetRef` is a reference to the currently-opened bottom sheet and can be used to close it or to subscribe to events. Note that only one bottom sheet can be open at a time. Any component contained inside of a bottom sheet can inject the `MatBottomSheetRef` as well.

```ts
bottomSheetRef.afterDismissed().subscribe(() => {
  console.log('Bottom sheet has been dismissed.');
});

bottomSheetRef.dismiss();
```

## 与底部菜单组件共享数据

> Sharing data with the bottom sheet component.

如果向传递一些数据到底部菜单，可以使用`data`属性：

> If you want to pass in some data to the bottom sheet, you can do so using the `data` property:

```ts
const bottomSheetRef = bottomSheet.open(HobbitSheet, {
  data: { names: ['Frodo', 'Bilbo'] },
});
```

随后可以使用`MAT_BOTTOM_SHEET_DATA`注入token来访问注入的数据：

> Afterwards you can access the injected data using the `MAT_BOTTOM_SHEET_DATA` injection token:

```ts
import {Component, Inject} from '@angular/core';
import {MAT_BOTTOM_SHEET_DATA} from '@angular/material';

@Component({
  selector: 'hobbit-sheet',
  template: 'passed in {{ data.names }}',
})
export class HobbitSheet {
  constructor(@Inject(MAT_BOTTOM_SHEET_DATA) public data: any) { }
}
```

## 通过`entryComponents`配置底部菜单内容

> Configuring bottom sheet content via `entryComponents`

与`MatDialog`相似，`MatBottomSheet`在运行时初始化组件。为了使其生效，Angular编译器需要额外的信息来为你的底部菜单内容组件创建必要的`ComponentFactory`。

> Similarly to `MatDialog`, `MatBottomSheet` instantiates components at run-time. In order for it to work, the Angular compiler needs extra information to create the necessary `ComponentFactory` for your bottom sheet content component.

引入底部菜单的任何组件，都需要添加进`NgModule`中的`entryComponents`中。

> Any components that are include inside of a bottom sheet have to be added to the `entryComponents` inside your `NgModule`.

```ts
@NgModule({
  imports: [
    // ...
    MatBottomSheetModule
  ],

  declarations: [
    AppComponent,
    ExampleBottomSheetComponent
  ],

  entryComponents: [
    ExampleBottomSheetComponent
  ],

  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

## 无障碍

> Accessibility

默认情况下，在根元素的底部菜单有角色`role="dialog"`，并可使用`MatBottomSheetConfig`中的`ariaLabel`属性来标签化。

> By default, the bottom sheet has `role="dialog"` on the root element and can be labelled using the `ariaLabel` property on the `MatBottomSheetConfig`.

当一个底部菜单打开时，焦点将移动到第一个可获得焦点的元素上。为了防止用户tab进入后台的元素，Material底部菜单使用了一个[焦点陷阱](https://material.angular.io/components/bottom-sheet/overview#focustrap)将焦点包在其内部。一旦底部菜单关闭，会将焦点返回给打开之前的焦点元素上。

> When a bottom sheet is opened, it will move focus to the first focusable element that it can find. In order to prevent users from tabbing into elements in the background, the Material bottom sheet uses a [focus trap](https://material.angular.io/components/bottom-sheet/overview#focustrap) to contain focus within itself. Once a bottom sheet is closed, it will return focus to the element that was focused before it was opened.

### 焦点管理

> Focus management

默认在底部菜单打开时第一个可获得焦点的元素将获得焦点。也可通过设置`cdkFocusInitial`属性来使另一个元素获得焦点。

> By default, the first tabbable element within the bottom sheet will receive focus upon open. This can be configured by setting the `cdkFocusInitial` attribute on another focusable element.

## 键盘交互

> Keyboard interaction

默认按Esc键可以关闭底部菜单。也可以通过`disableClose`选项关闭此行为，通常来说，用户应避免这样做，以为会打断屏幕阅读者预期的交互模式。

> By default pressing the escape key will close the bottom sheet. While this behavior can be turned off via the `disableClose` option, users should generally avoid doing so as it breaks the expected interaction pattern for screen-reader users.

# API

## 底部菜单(bottom-sheet)的API参考

> API reference for Angular Material bottom-sheet

```ts
import {MatBottomSheetModule} from '@angular/material/bottom-sheet';
```

## 服务

> Services

### MatBottomSheet

触发Material Design底部菜单的服务。

> Service to trigger Material Design bottom sheets.

**方法**

> Methods

dismiss
---
- 描述
  - 释放当前可见的底部菜单。
  - Dismisses the currently-visible bottom sheet.

open
---
- 参数
  - componentOrTemplateRef (ComponentType<T> | TemplateRef<T>)
  - config? (MatBottomSheetConfig<D>)	
- 返回
  - MatBottomSheetRef<T, R>

## 附加类

> Additional classes

### MatBottomSheetConfig

用于打开一个底部菜单的配置。

> Configuration used when opening a bottom sheet.

**属性**

> Properties

属性名|描述|Description
-|-|-
ariaLabel: string \| null|设置底部菜单元素的Aria标签。|Aria label to assign to the bottom sheet element.
backdropClass: string|背景的自定义类。|Custom class for the backdrop.
data: D \| null|注入子组件中的数据。|Data being injected into the child component.
direction: Direction|底部菜单的文字布局方向。|Text layout direction for the bottom sheet.
disableClose: boolean|用户是否可通过Esc或者点击外部来关闭底部菜单。|Whether the user can use escape or clicking outside to close the bottom sheet.
hasBackdrop: boolean|底部菜单是否有一个背景。|Whether the bottom sheet has a backdrop.
panelClass: string \| string[]|底部菜单容器的附加CSS类。|Extra CSS classes to be added to the bottom sheet container.
viewContainerRef: ViewContainerRef|放置底部菜单的视图容器。|The view container to place the overlay for the bottom sheet into.

### MatBottomSheetRef

底部菜单服务分配的一个底部菜单的参考。

> Reference to a bottom sheet dispatched from the bottom sheet service.

**属性**

> Properties

属性名|描述|Description
-|-|-
instance: T|组成底部菜单内容的组件实例。|Instance of the component making up the content of the bottom sheet.

**方法**

> Methods

afterDismissed
---
- 描述
  - 获取一个用于通知底部菜单完成关闭时的可观察对象。
  - Gets an observable that is notified when the bottom sheet is finished closing.
- 返回
  - Observable<R | undefined>

afterOpened
---
- 描述
  - 获取一个用于通知底部菜单打开和显示时的可观察对象。
  - Gets an observable that is notified when the bottom sheet has opened and appeared.
- 返回
  - Observable<void>

backdropClick
---
- 描述
  - 获取一个用于传递点击背景事件的可观察对象。
  - Gets an observable that emits when the overlay's backdrop has been clicked.
- 返回
  - Observable<MouseEvent>

dismiss
---
- 描述
  - 释放底部菜单。
  - Dismisses the bottom sheet.
- 参数
  - result?R：传回底部菜单打开器的数据。
  - Data to be passed back to the bottom sheet opener.

keydownEvents
---
- 描述
  - 获取一个传递在叠加层发生的按键事件的可观察对象。
  - Gets an observable that emits when keydown events are targeted on the overlay.
- 返回
  - Observable<KeyboardEvent>

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*