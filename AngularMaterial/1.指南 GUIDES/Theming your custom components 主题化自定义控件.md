# 定制你的组件主题

[toc]

为了使用Angular Material工具改变你的组件样式，组件样式必须使用Sass进行定义

> In order to style your own components with Angular Material's tooling, the component's styles must be defined with Sass.

## 使用 `@mixin` 自动应用主题

> Using `@mixin` to automatically apply a theme

**为什么使用`@mixin`**

> Why using `@mixin`

使用`@mixin`方法的优点是，每当你更改主题的时候，使用的每个文件都会被自动更新。可以在应用或者组件中调用@mixin的时候引入不同的主题参数，来使用多重主题。

> The advantage of using a `@mixin` function is that when you change your theme, every file that uses it will be automatically updated. Calling the `@mixin` with a different theme argument allows multiple themes within the app or component.

**如何使用`@mixin`**

> How to use `@mixin`

通过把`@mixin`方法添加到主题文件中，然后调用这个方法应用主题，这样我们就能够更好地主题化自定义组件。

> We can better theming our custom components adding a `@mixin` function to its theme file and then calling this function to apply a theme.

你只须要在custom-component-theme.scss文件中创建一个`@mixin`方法。

> All you need is to create a `@mixin` function in the custom-component-theme.scss

```scss
// Import all the tools needed to customize the theme and extract parts of it
@import '~@angular/material/theming';

// Define a mixin that accepts a theme and outputs the color styles for the component.
`@mixin` candy-carousel-theme($theme) {
// Extract whichever individual palettes you need from the theme.
$primary: map-get($theme, primary);
$accent: map-get($theme, accent);

// Use mat-color to extract individual colors from a palette as necessary.
.candy-carousel {
background-color: mat-color($primary);
border-color: mat-color($accent, A400);
}
}
```

现在你只要调用`@mixin`方法就可以应用主题了：

> Now you just have to call the `@mixin` function to apply the theme:

```scss
// Import a pre-built theme
@import '~@angular/material/prebuilt-themes/deeppurple-amber.css';
// Import your custom input theme file so you can call the custom-input-theme function
@import 'app/candy-carousel/candy-carousel-theme.scss';

// Using the $theme variable from the pre-built theme you can call the theming function
@include candy-carousel-theme($theme);
```

可以查看[源码]( https://github.com/angular/material2/blob/master/src/lib/core/theming/_theming.scss)中的注释，来了解更多关于主题函数的细节。


> For more details about the theming functions, see the comments in the source.


**使用`@mixin`的最佳实践。**

> Best practices using `@mixin`

当使用`@mixin`的时候，主题文件只应该包含只认证过的主题定义。

> When using `@mixin`, the theme file should only contain the definitions that are affected by the passed-in theme.

所有不被主题影响的样式应该放在`candy-carousel.scss`文件中。这个文件应该包含所有不被主题样式所影响，如大小(sizes)，转换(transitions)等...

> All styles that are not affected by the theme should be placed in a `candy-carousel.scss` file. This file should contain everything that is not affected by the theme like sizes, transitions...

被主题影响的样式应该放在一个独立的主题文件中，如`_candy-carousel.theme.scss`，并且文件名前必须有`\_`。这个文件应该包含负责应用组件主题的`@mixin`方法

> Styles that are affected by the theme should be placed in a separated theming file as `_candy-carousel-theme.scss` and the file should have a _ before the name. This file should contain the `@mixin` function responsible for applying the theme to the component.

## 使用调色板中的颜色

> Using colors from a palette

你可以使用`@angular/material/theming`中的主题方法和Material调色板变量。你也可以使用`mat-color`方法来获得一个特定的调色板中的颜色，如下：

> You can consume the theming functions and Material palette variables from `@angular/material/theming`. You can use the `mat-color` function to extract a specific color from a palette. For example:

```scss
// Import theming functions
@import '~@angular/material/theming';
// Import your custom theme
@import 'src/unicorn-app-theme.scss';

// Use mat-color to extract individual colors from a palette as necessary.
.candy-carousel {
background-color: mat-color($candy-app-primary);
border-color: mat-color($candy-app-accent, A400);
}
```

*于2017年11月14日修改第二版，调整一些翻译的逻辑。*

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*