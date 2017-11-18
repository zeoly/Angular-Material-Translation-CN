# 定制组件样式

## 样式原则

> Styling concepts

在定制Angular Material组件样式的时候，有三个问题需要记住：

> There are 3 questions to keep in mind while customizing the styles of Angular Material components:

1. 你的样式封装了吗？
2. 你的样式比默认样式更特定吗？
3. 这个组件是你的组件中的子组件，或者是DOM中的其它组件吗？

>1. Are your styles encapsulated?
>2. Are your styles more specific than the defaults?
>3. Is the component a child of your component, or does it exist elsewhere in the DOM?

**视图封装**

> View encapsulation

默认情况下，Angular组件样式是被限定在只影响组件视图的范围之内。这意味着你写的样式只会影响你的组件模板中的所有元素。它们*不会*影响你组件模板中的其它子组件内的元素。你可以通过[Angular文档](https://angular.io/guide/component-styles#view-encapsulation)阅读更多关于视图封装的知识。你也可以在Angular博客中看一眼[Angular中的CSS状态](https://blog.angular.io/the-state-of-css-in-angular-4a52d4bd2700)查看更多。

> By default, Angular component styles are scoped to affect the component's view. This means that the styles you write will affect all the elements in your component template. They will *not* affect elements that are children of other components within your template. You can read more about view encapsulation in the [Angular documentation](https://angular.io/guide/component-styles#view-encapsulation). You may also wish to take a look at [*The State of CSS in Angular*](https://blog.angular.io/the-state-of-css-in-angular-4a52d4bd2700) on the Angular blog.

**选择器特征**

> Selector specificity

每个CSS声明都有一个基于选择器的类型和数量的特定级别。更特定的样式将优先于不太特定的样式。Angular Material使用了最不特定的样式，这样使得其它组件可以轻易覆盖它们。你可以通过[MDN web docs](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity).查看更多关于其特征与计算方式。


> Each CSS declaration has a level of specificity based on the type and number of selectors used. More specific styles will take precedence over less specific styles. Angular Material uses the least specific selectors possible for its components in order to make it easy to override them. You can read more about specificity and how it is calculated on the [MDN web docs](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity).

**组件定位**

> Component location

一些特别是基于覆盖层的Angular Material组件如MatDialog，MatSnackbar等不能作为你的组件的子组件。它们在别处经常被注入到DOM中。需要注意的是，因为即使使用了高特定性和阴影穿刺选择器，也不会针对那些不是你组件的直接子元素的元素。推荐使用全局样式来定位这些组件。

> Some Angular Material components, specifically overlay-based ones like MatDialog, MatSnackbar, etc., do not exist as children of your component. Often they are injected elsewhere in the DOM. This is important to keep in mind, since even using high specificity and shadow-piercing selectors will not target elements that are not direct children of your component. Global styles are recommended for targeting such components.

## 覆盖组件样式

> Styling overlay components

基于覆盖性的组件有一个可以用于定位覆盖面板`panelClass`属性（或者类似）。举个例子，从对话框里移除padding：

> Overlay-based components have a panelClass property (or similar) that can be used to target the overlay pane. For example, to remove the padding from a dialog:

```css
// Add this to your global stylesheet after your theme setup
.myapp-no-padding-dialog .mat-dialog-container {
  padding: 0;
}
this.dialog.open(MyDialogComponent, {panelClass: 'myapp-no-padding-dialog'})
```

自从你在全局样式表中添加样式后，最佳实践方法是将其控制在适当的范围内。尝试在你的选择器前面增加前缀或者“custom”字段。同样需要注意到是`mat-dialog-container`的padding属性通过选择器默认会特定性为1.定制的样式的特定性为2，因此它们将始终优先。

> Since you are adding the styles to your global stylesheet, it is good practice to scope them appropriately. Try prefixing your selector with your app name or "custom". Also note that the `mat-dialog-container`'s padding is added by default via a selector with specificity of 1. The customizing styles have a specificity of 2, so they will always take precedence.

## 其它组件样式

> Styling other components

如果你的组件视图封装已经打开(默认下是打开)，你的组件样式只会影响模板中的顶层。属于子组件的HTML元素不能通过你的组件样式来定位，除非你做到下面几条：

> If your component has view encapsulation turned on (default), your component styles will only affect the top level children in your template. HTML elements belonging to child components cannot be targeted by your component styles unless you do one of the following:

- 在全局样式表中添加覆盖样式。将选择器的范围控制在只影响你需要的特定元素中。
- 关闭组件中的视图封装功能。如果你确实需要，确保将你的样式控制在合理的范围内。否则，您可能会偶然地将目标锁定在应用程序中的其他组件。
- 通过使用一个不被用过的暗影穿透的后代组合器来强制样式适用于所有的子元素。


>- Add the overriding style to you global stylesheet. Scope the selectors so that it only affects the specific elements you need it to.
>- Turn view encapsulation off on your component. If you do this, be sure to scope your styles appropriately, or else you may end up incidentally targeting other components elswhere in your application.
>- Use a deprecated shadow-piercing descendant combinator to force styles to apply to all the child elements. Read more about this deprecated solution in the Angular documentation.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*