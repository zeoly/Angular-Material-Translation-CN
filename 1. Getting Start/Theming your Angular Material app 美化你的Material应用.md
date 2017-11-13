
# 主题化你的Material应用

> Theming your Angular Material app

[toc]

## 什么是主题
> What is a theme?

**主题**是由不同颜色构成的一种组合，可以应用到Angular Material组件上。主题库用法可以参考[Material Design说明](https://material.google.com/style/color.html#color-color-palette) 产品指导。

> A theme is the set of colors that will be applied to the Angular Material components. The library's approach to theming is based on the guidance from the [Material Design spec](https://material.google.com/style/color.html#color-color-palette).

在Angular Material中主题是通过多个色调组合来创建的。特别说明，一个主题由以下组成：

> In Angular Material, a theme is created by composing multiple palettes. In particular, a theme consists of:

* 主色调：全部屏幕和组件中广泛使用的颜色。
* 强调色调：浮动操作按钮和交互元素中使用的颜色。
* 警告色调：提示错误状态的颜色。
* 前景色调：字体和图标的颜色。
* 背景色调：元素背景的颜色。

>* A primary palette: colors most widely used across all screens and components.
>* An accent palette: colors used for the floating action button and interactive elements.
>* A warn palette: colors used to convey error state.
>* A foreground palette: colors for text and icons.
>* A background palette: colors used for element backgrounds.

在Angular Material中，所有的主题样式都是在应用建立的时候静态生成的，所以你的应用不需要在启动时花费周期来生成主题样式。

> In Angular Material, all theme styles are generated statically at build-time so that your app doesn't have to spend cycles generating theme styles on startup.

##  使用预建主题
> Using a pre-built theme

一些预建主题的CSS文件已经预打包在Angular Material当中。这些主题文件同样包含了所有核心样式（样式同样适用于所有组件），所以你只需要在应用中引入一个单独Angular Material的CSS文件就可以了。

> Angular Material comes prepackaged with several pre-built theme css files. These theme files also include all of the styles for core (styles common to all components), so you only have to include a single css file for Angular Material in your app.

你可以通过`@angular/material/prebuilt-themes`直接引入主题文件到应用中。

> You can include a theme file directly into your application from @angular/material/prebuilt-themes

可用的预建主题：

> Available pre-built themes:

* deeppurple-amber.css
* indigo-pink.css
* pink-bluegrey.css
* purple-green.css

如果你正在使用Angular CLI，通过在styles.css文件中加入下列代码能快速将其样式引入：

> If you're using Angular CLI, this is as simple as including one line in your `styles.css` file:

```css
@import '~@angular/material/prebuilt-themes/deeppurple-amber.css';
```

或者也可以用文件链接的方式引入，如下：

> Alternatively, you can just reference the file directly. This would look something like:

```html
<link href="node_modules/@angular/material/prebuilt-themes/indigo-pink.css" rel="stylesheet">
```

实际路径需要根据你的应用服务来设置。

> The actual path will depend on your server setup.

你也可以将此文件与你应用的css文件合并。

> You can also concatenate the file with the rest of your application's css.

最后，如果你的应用内容**不是**放置在`md-sidenav-container`元素中，你需要添加`mat-app-backgroud`类到封装的元素中（比如`body`元素），确保主题背景能正确的应用到你的页面中。

> Finally, if your app's content **is not** placed inside of a `md-sidenav-container` element, you need to add the `mat-app-background` class to your wrapper element (for example the `body`). This ensures that the proper theme background is applied to your page.

## 创建定制主题

> Defining a custom theme

当你想拥有更多定制化的主题时，你可以创建自己的主题文件。

> When you want more customization than a pre-built theme offers, you can create your own theme file.

自定义主题文件需要做两件事：

> A custom theme file does two things:

1. 导入`mat-core()` sass mixin。这个文件包括了多种组件的所有公共样式，**并且只需要在应用中引入一次即可**。如果文件被引入多次，你的应用就将会存在此公共样式多个副本。

>1. Imports the `mat-core()` sass mixin. This includes all common styles that are used by multiple components. **This should only be included once in your application**. If this mixin is included multiple times, your application will end up with multiple copies of these common styles.

2. 用多个色调的组合，来一个**主题**数据结构。这个数据对象可以用`mat-light-theme`方法或者`mat-dark-theme`方法生成。这些方法输出的数据传到`angular-material-theme mixin`，然后会输出所有主题相应的样式。

>2. Defines a **theme** data structure as the composition of multiple palettes. This object can be created with either the `mat-light-theme` function or the `mat-dark-theme` function. The output of this function is then passed to the `angular-material-theme` mixin, which will output all of the corresponding styles for the theme.

一个典型的主题文件会如下：

> A typical theme file will look something like this:

```scss
@import '~@angular/material/theming';
// Plus imports for other components in your app.

// Include the common styles for Angular Material. We include this here so that you only
// have to load a single css file for Angular Material in your app.
// Be sure that you only ever include this mixin once!
@include mat-core();

// Define the palettes for your theme using the Material Design palettes available in palette.scss
// (imported above). For each palette, you can optionally specify a default, lighter, and darker
// hue.
$candy-app-primary: mat-palette($mat-indigo);
$candy-app-accent:  mat-palette($mat-pink, A200, A100, A400);

// The warn palette is optional (defaults to red).
$candy-app-warn:    mat-palette($mat-red);

// Create the theme object (a Sass map containing all of the palettes).
$candy-app-theme: mat-light-theme($candy-app-primary, $candy-app-accent, $candy-app-warn);

// Include theme styles for core and each component used in your app.
// Alternatively, you can import and @include the theme mixins for each component
// that you are using.
@include angular-material-theme($candy-app-theme);
```

你只需要这个单独的Sass文件即可，你并不需要用Sass来修改所有应用的样式。

> You only need this single Sass file; you do not need to use Sass to style the rest of your app.

如果你正在使用Angular CLI，将Sass文件编译到css文件的功能已经内置在其中，你只需要将指向主题文件的`angular-cli.json`文件中的`"styles"`列表中增加条目即可。（比如`unicorn-app-theme.scss`）

> If you are using the Angular CLI, support for compiling Sass to css is built-in; you only have to add a new entry to the `"styles"` list in `angular-cli.json` pointing to the theme file (e.g., `unicorn-app-theme.scss`).

如果你没有使用Angular CLI，你可以使用任何Sass工具来创建css文件（比如gulp-sass或者grunt-sass）。最简单的方法是使用`node-sass` CLI工具；如下：

> If you're not using the Angular CLI, you can use any existing Sass tooling to build the file (such as gulp-sass or grunt-sass). The simplest approach is to use the `node-sass` CLI; you simply run:

```cmd
node-sass src/unicorn-app-theme.scss dist/unicorn-app-theme.css
```

接着在index.html文件中引入输出文件。

> and then include the output file in your index.html.

主题文件**不应该**导入到其它SCSS文件中。这会导致多个样式副本被写入CSS输出文件。如果你想消费其它SCSS文件中的定义的主题对象（如`$candy-app-theme`），接着定义的主题对象可以划分到相应的文件中，分别放置在`mat-core`和`angular-material-theme` mixins中。

> The theme file **should not** be imported into other SCSS files. This will cause duplicate styles to be written into your CSS output. If you want to consume the theme definition object (e.g., `$candy-app-theme`) in other SCSS files, then the definition of the theme object should be broken into its own file, separate from the inclusion of the `mat-core` and `angular-material-theme` mixins.

主题文件可以通过应用中的CSS文件拼接和最小化处理。

> The theme file can be concatenated and minified with the rest of the application's css.

注意到如果你要在Angular组件的`styleUrls`中引入生成的主题文件，那么这些样式将被封装在组件[视图封装](https://angular.io/docs/ts/latest/guide/component-styles.html#!#view-encapsulation)中。

> Note that if you include the generated theme file in the `styleUrls` of an Angular component, those styles will be subject to that component's [view encapsulation](https://angular.io/docs/ts/latest/guide/component-styles.html#!#view-encapsulation).

**多个主题**

> Multiple themes

你可以在应用中通过多次引入`angular-material-theme` mixin来创建多个主题，并由额外的CSS类进行调制。

> You can create multiple themes for your application by including the 
`angular-material-theme` mixin multiple times, where each inclusion is gated by an additional CSS class.

记住只能引入一次`@mat-core` mixin，不需要每个主题都引入。

> Remember to only ever include the `@mat-core` mixin only once; it should not be included for each theme.

## 定义多个主题如下：

> Example of defining multiple themes:

```scss
@import '~@angular/material/theming';
// Plus imports for other components in your app.

// Include the common styles for Angular Material. We include this here so that you only
// have to load a single css file for Angular Material in your app.
// **Be sure that you only ever include this mixin once!**
@include mat-core();

// Define the default theme (same as the example above).
$candy-app-primary: mat-palette($mat-indigo);
$candy-app-accent:  mat-palette($mat-pink, A200, A100, A400);
$candy-app-theme:   mat-light-theme($candy-app-primary, $candy-app-accent);

// Include the default theme styles.
@include angular-material-theme($candy-app-theme);


// Define an alternate dark theme.
$dark-primary: mat-palette($mat-blue-grey);
$dark-accent:  mat-palette($mat-amber, A200, A100, A400);
$dark-warn:    mat-palette($mat-deep-orange);
$dark-theme:   mat-dark-theme($dark-primary, $dark-accent, $dark-warn);

// Include the alternative theme styles inside of a block with a CSS class. You can make this
// CSS class whatever you want. In this example, any component inside of an element with
// `.unicorn-dark-theme` will be affected by this alternate dark theme instead of the default theme.
.unicorn-dark-theme {
  @include angular-material-theme($dark-theme);
}
```

在上述例子中，在`unicorn-dark-theme`父类中的任何组件都会使用深色主题，而其它的组件则会退回使用默认`$candy-app-theme`主题中。

> In the above example, any component inside of a parent with the `unicorn-dark-theme` class will use the dark theme, while other components will fall back to the default `$candy-app-theme`.

你可以通过这种方法引入任意个主题，你也可以在各个文件中通过`@include`引入`angular-material-theme`，并基于终端用户交互懒加载它们（如何懒加载CSS资源将会根据你的应用有所不同）。

> You can include as many themes as you like in this manner. You can also `@include` the `angular-material-theme` in separate files and then lazily load them based on an end-user interaction (how to lazily load the CSS assets will vary based on your application).

非常重要的是，`mat-core` mixin 只需要引入一次！

> It's important to remember, however, that the `mat-core` mixin should only ever be included once.

## 多个主题与基于覆盖的组件

> Multiple themes and overlay-based components

在全局覆盖的容器中单个确定的组件（如菜单，选择，对话框等）需要额外的一个步骤，就是需要主题的CSS类选择器对组件的影响(上个例子中的`.unicorn-dark-theme`)。

> Since certain components (e.g. menu, select, dialog, etc.) are inside of a global overlay container, an additional step is required for those components to be affected by the theme's css class selector (`.unicorn-dark-theme` in the example above).

通过上述方法，你可以声明一个全局覆盖的容器中的`themeClass`。如下：

> To do this, you can specify a themeClass on the global overlay container. For the example above, this would look like:

```typescript
import {OverlayContainer} from '@angular/material';

@NgModule({
  // ...
})
export class UnicornCandyAppModule {
  constructor(overlayContainer: OverlayContainer) {
    overlayContainer.themeClass = 'unicorn-dark-theme';
  }
}
```

**主题化特定的组件**

> Theming only certain components

`angular-material-theme` mixin会输出[库中所有组件](https://github.com/angular/material2/blob/master/src/lib/core/theming/_all-theme.scss)的样式。如果你只想使用某些组件（或者你只想改变特定组件的主题），你可以引入component-specific theme mixins。你也同样需要引入`mat-core-theme` mixin，包含普通行为的特定主题样式（如ripples）。

> The `angular-material-theme` mixin will output styles for [all components in the library](https://github.com/angular/material2/blob/master/src/lib/core/theming/_all-theme.scss). If you are only using a subset of the components (or if you want to change the theme for specific components), you can include component-specific theme mixins. You also will need to include the `mat-core-theme` mixin as well, which contains theme-specific styles for common behaviors (such as ripples).

```scss
@import '~@angular/material/theming';
// Plus imports for other components in your app.

// Include the common styles for Angular Material. We include this here so that you only
// have to load a single css file for Angular Material in your app.
// **Be sure that you only ever include this mixin once!**
@include mat-core();

// Define the theme.
$candy-app-primary: mat-palette($mat-indigo);
$candy-app-accent:  mat-palette($mat-pink, A200, A100, A400);
$candy-app-theme:   mat-light-theme($candy-app-primary, $candy-app-accent);

// Include the theme styles for only specified components.
@include mat-core-theme($candy-app-theme);
@include mat-button-theme($candy-app-theme);
@include mat-checkbox-theme($candy-app-theme);
```
## 主题化你的组件

> Theming your own components

通过[theming-your-components.md](https://material.angular.io/guide/theming-your-components)查看更多关于主题化你的组件内容。

> For more details about theming your own components, see [theming-your-components.md](https://material.angular.io/guide/theming-your-components).

## 未来的工作

> Future work

一旦CSS变量（自定义属性）在所有浏览器中可用，我们将研究如何利用这个特性使主题化工作更简单。

> Once CSS variables (custom properties) are available in all the browsers we support, we will explore how to take advantage of them to make theming even simpler.

更多预制主题将会添加进来。

> More prebuilt themes will be added as development continues.

*于2017年11月13日修改第二版，重新调整了相关名词的引用格式。删除下面多余的一段*

>~~The themeClass of the OverlayContainer can be changed at any time to change the active theme class.~~

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*