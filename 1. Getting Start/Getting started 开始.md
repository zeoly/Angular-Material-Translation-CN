# 开始 Getting started

[toc]

通过检出[Angular CLI](https://cli.angular.io/)开始创建你的第一个Angular应用。

>For help getting started with a new Angular app, check out the [Angular CLI](https://cli.angular.io/).

如果已经创建app，可以按照下面步骤开始使用`Angular Material`。

> For existing apps, follow these steps to begin using Angular Material.

## 第一步：安装 Angular Material 与 Angular CDK

Step 1: Install Angular Material and Angular CDK

```
npm install --save @angular/material @angular/cdk
```

由主线分支的最新的快照版本也可供下载，但是需要注意的是，这个快照版本不是稳定的，在发布期间也可能中断。

> A snapshot build with the latest changes from master is also available. Note that this snapshot build should not be considered stable and may break between releases.

```
npm install --save angular/material2-builds angular/cdk-builds
```

## 第二步：动画

>Step 2: Animations

为了表现更高级的过渡动画，一些Material组件需要Angular动画模块。如果你想这些动画在你的应用里面起效，需要在应用安装`@angular/animations`模块并引入`BrowserAnimationsModule`模组。

> Some Material components depend on the Angular animations module in order to be able to do more advanced transitions. If you want these animations to work in your app, you have to install the `@angular/animations` module and include the `BrowserAnimationsModule` in your app.

```
npm install --save @angular/animations
```

注意：`@angular/animations` 使用的是WebAnimation的API,而且现在还不支持所有的浏览器。如果你想在这些浏览器中支持Material组件，你需要引入polyfill。

> Note: @angular/animations uses the WebAnimation API that isn't supported by all browsers yet. If you want to support Material component animations in these browsers, you'll have to include a polyfill.



```typescript
import {BrowserAnimationsModule} from '@angular/platform-browser/animations';
@NgModule({
...
imports: [BrowserAnimationsModule],
...
})
export class PizzaPartyAppModule { }
```

如果不想在项目中增加另一个依赖，可以使用`NoopAnimationsModule`。

> If you don't want to add another dependency to your project, you can use the `NoopAnimationsModule`.

```typescript
import {NoopAnimationsModule} from '@angular/platform-browser/animations';
@NgModule({
...
imports: [NoopAnimationsModule],
...
})
export class PizzaPartyAppModule { }
```

## 第三步：导入组件模块

> Step 3: Import the component modules

在每个要用到的组件中导入`NgModule`模块。

> Import the NgModule for each component you want to use:

```typescript
import {MdButtonModule, MdCheckboxModule} from '@angular/material';
@NgModule({
...
imports: [MdButtonModule, MdCheckboxModule],
...
})
export class PizzaPartyAppModule { }
```

或者，你可以单独创建的NgModule，在里面导入所有你要用到的Angular Material组件，然后通过引入这个模块来使用这些模块组件。

> Alternatively, you can create a separate NgModule that imports all of the Angular Material components that you will use in your application. You can then include this module wherever you'd like to use the components.

```typescript
import {MdButtonModule, MdCheckboxModule} from '@angular/material';
@NgModule({
imports: [MdButtonModule, MdCheckboxModule],
exports: [MdButtonModule, MdCheckboxModule],
})
export class MyOwnCustomMaterialModule { }
```

不管使用哪种方法导入组件，必须确保在Angular的` BrowserModule`模块后面引入Angular Material各个模块，因为导入顺序对NgModules很重要。

> Whichever approach you use, be sure to import the Angular Material modules *after* Angular's `BrowserModule`, as the import order matters for NgModules.

## 第四步：引入主题

> Step 4: Include a theme

为了引入主题，**需要**在应用程序中引入所有的核心和主题样式。

> Including a theme is required to apply all of the core and theme styles to your application.

如果要使用预建的主题，需要先将一个Angular Material预建主题在你的应用中全局引入。如果你正在使用Angular CLI工具，你可以将下列代码添加至styles.css文件中。

> To get started with a prebuilt theme, include one of Angular Material's prebuilt themes globally in your application. If you're using the Angular CLI, you can add this to your styles.css:

```css
@import "~@angular/material/prebuilt-themes/indigo-pink.css";
```

如果你没有使用Angular CLI，你可以在`index.html`文件中通过添加`<link>`标签来引入预建主题。

> If you are not using the Angular CLI, you can include a prebuilt theme via a `<link>` element in your `index.html`.

请通过[主题指南](https://material.angular.io/guide/theming)板块，查看更多的主题类信息与如何创建自定义主题等。

> For more information on theming and instructions on how to create a custom theme, see the [theming guide](https://material.angular.io/guide/theming).

## 第五步：手势支持
> Step 5: Gesture Support

一些组件（如`md-slide-toggle`, `md-slider`, `mdTooltip`）需要依赖[HammerJS]( http://hammerjs.github.io/)文件。为了使用完整的特性集，HammerJS文件必须要加载到应用中。

> Some components (md-slide-toggle, md-slider, mdTooltip) rely on [HammerJS]( http://hammerjs.github.io/) for gestures. In order to get the full feature-set of these components, HammerJS must be loaded into the application.

你也可以通过[npm](https://www.npmjs.com/package/hammerjs)命令或者链接CDN（比如 [Google CDN](https://developers.google.com/speed/libraries/#hammerjs)）将HammerJS引入至你的应用。

> You can add HammerJS to your application via [npm](https://www.npmjs.com/package/hammerjs), a CDN (such as the [Google CDN](https://developers.google.com/speed/libraries/#hammerjs)), or served directly from your app.

通过下列npm命令来安装HammerJS：

> To install via npm, use the following command:

```
npm install --save hammerjs
```

安装完成后，将其导入至你的应用根模块。

> After installing, import it on your app's root module.

```typescript
import 'hammerjs';
```

## 第六步（可选）：添加Material图标

> Step 6 (Optional): Add Material Icons

如果想使用官方[Material Design图标]( https://material.io/icons/)的`md-icon`组件时，加载图标字体到你的`index.html`文件中。

> If you want to use the md-icon component with the official [Material Design Icons]( https://material.io/icons/), load the icon font in your index.html.

```html
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
```

请通过[图标指南板块](https://google.github.io/material-design-icons/)查看更多关于的图标类信息。

> For more information on using Material Icons, check out the [Material Icons Guide](https://google.github.io/material-design-icons/).

请注意`md-icon`支持所有字体和svg图标，使用Material图标只是其中一种选择。

> Note that `md-icon` supports any font or svg icons; using Material Icons is one of many options.

## 附录

> Appendix: Configuring SystemJS

如果你的项目正在使用SystemJS加载模块，你需要在SyetemJS配置文件中加入`@angular/material`和`@angular/cdk`：

> If your project is using SystemJS for module loading, you will need to add `@angular/material` and `@angular/cdk` to the SystemJS configuration.

`@angular/cdk`的包由多个入口点组成。每个入口点都应该单独与SystemJS注册。

> The @angular/cdk package is organized of multiple entry-points. Each of these entry-points must be registered individually with SystemJS.

下面是`@angular/material`, `@angular/cdk/platform`和`@angular/cdk/a11y`的设置例子:

> Here is a example configuration where @angular/material, @angular/cdk/platform and @angular/cdk/a11y are used:

```typescript
System.config({
// existing configuration options
map: {
// ...
'@angular/material': 'npm:@angular/material/bundles/material.umd.js',
// ...
}
});
```

*于2017年11月13日修改第三版，官网增加了三段内容，分别为动画部分的NOTE，附录的SystemJS配置。

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*