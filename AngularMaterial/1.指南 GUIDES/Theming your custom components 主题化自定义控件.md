# 定制你的组件主题

[toc]

为了使用Angular Material工具改变你的组件样式，组件样式必须使用Sass进行定义

> In order to style your own components with Angular Material's tooling, the component's styles must be defined with Sass.

## 使用 @mixin 自动应用主题

> Using @mixin to automatically apply a theme

**为什么使用@mixin**

> Why using @mixin

使用@mixin方法的优点是，当你更改主题后，每个使用到主题的文件能自动更新。在应用或者组件内可以通过引入不同的主题参数来使用多重主题。

> The advantage of using a @mixin function is that when you change your theme, every file that uses it will be updated automatically. Calling it with a different theme argument allow multiple themes within the app or component.

**如何使用@mixin**

> How to use @mixin

在主题文件中添加一个@mixin的方法，然后调用这个方法来应用主题，这样我们就能更好地定制自定义组件的主题。

> We can better theming our custom components adding a @mixin function to its theme file and then calling this function to apply a theme.

你需要做的只是在自定义组件主题的scss文件中创建一个@mixin方法。

> All you need is to create a @mixin function in the custom-component-theme.scss

```typescript
// Import all the tools needed to customize the theme and extract parts of it
@import '~@angular/material/theming';

// Define a mixin that accepts a theme and outputs the color styles for the component.
@mixin candy-carousel-theme($theme) {
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

现在你只要调用@mixin方法就可以应用主题了：

> Now you just have to call the @mixin function to apply the theme:

```typescript
// Import a pre-built theme
@import '~@angular/material/prebuilt-themes/deeppurple-amber.css';
// Import your custom input theme file so you can call the custom-input-theme function
@import 'app/candy-carousel/candy-carousel-theme.scss';

// Using the $theme variable from the pre-built theme you can call the theming function
@include candy-carousel-theme($theme);
For more details about the theming functions, see the comments in the source.
```

**使用@mixin的最佳实践。**

> Best practices using @mixin

当使用@mixin的时候，主题文件只应该包含只认证过的主题定义。

> When using @mixin, the theme file should only contain the definitions that are affected by the passed-in theme.

所有不被主题影响的样式应该放在candy-carousel.scss文件中。这个文件应该包含所有不被主题样式所影响，如大小(sizes)，转换(transitions)等...

> All styles that are not affected by the theme should be placed in a candy-carousel.scss file. This file should contain everything that is not affected by the theme like sizes, transitions...

被主题影响的模式应该放在不同分离的主题文件中，如 \_candy-carousel.theme.scss与名字前有\_的文件。这个文件应该包含负责应用组件主题的@mixin方法

> Styles that are affected by the theme should be placed in a separated theming file as _candy-carousel-theme.scss and the file should have a _ before the name. This file should contain the @mixin function responsible for applying the theme to the component.

## 使用调色板中的颜色

> Using colors from a palette

你可以通过@angular/material/theming来使用主题方法和Material调色板变量。你也可以使用mat-color方法来获得一个特定的底板中的颜色，如下：

> You can consume the theming functions and Material palette variables from @angular/material/theming. You can use the mat-color function to extract a specific color from a palette. For example:

```typescript
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

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*