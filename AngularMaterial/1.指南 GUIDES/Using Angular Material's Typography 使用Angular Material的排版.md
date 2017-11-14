## 什么是排版

> What is typography?

排版是一种排列方式，可以让文字易读易懂，显示美观。Angular Material的排版是基于[Material Design Spec]( https://material.io/guidelines/style/typography.html)的规范分成了不同的排版层级。每一个层级都包含了`font-size`,`line-height`和`font-weight`。层级如下：

> Typography is a way of arranging type to make text legible, readable, and appealing when displayed. Angular Material's typography is based on the guidelines from the [Material Design Spec]( https://material.io/guidelines/style/typography.html) and is arranged into typography levels. Each level has a font-size, line-height and font-weight. The available levels are:

* `display-4`, `display-3`, `display-2`和`display-1`,大的一次性的头部, 通常在页面顶部 (e.g. a hero header).
* `headline` - 对应`<h1>`标签的段落标题。
* `title` -  对应`<h2>`标签的段落标题。
* `subheading-2` - 对应`<h3>`标签的段落标题。
* `subheading-1` - 对应`<h4>`标签的段落标题。
* `body-1` - 基础 body 文本.
* `body-2` - 加粗 body 文本.
* `caption` - 更小的 body 和 hint 文本.
* `button` - 按钮和锚点.

排版层级放在排版设置部分,用于生成CSS文件。

> The typography levels are collected into a typography config which is used to generate the CSS.

## 用法

> Usage

你首先要引入300,400和500大小的Roboto字体，你可以自己托管它，或者从[Google字体库](https://fonts.google.com/)库引入它：

> To get started, you first include the Roboto font with the 300, 400 and 500 weights. You can host it yourself or include it from [Google Fonts](https://fonts.google.com/):

```html
<link href="https://fonts.googleapis.com/css?family=Roboto:300,400,500" rel="stylesheet">
```

现在你可以在元素中添加要使用的合适的CSS类了：

> Now you can add the appropriate CSS classes to the elements that you want to style:

```html
<h1 class="mat-display-1">Jackdaws love my big sphinx of quartz.</h1>
<h2 class="mat-h2">The quick brown fox jumps over the lazy dog.</h2>
```

Angular Material默认不应用全局CSS。为了更广泛的使用库的排版样式，可以利用`mat-typography` CSS类。此类会将样式应用到所有继承的原生元素中。

> By default, Angular Material doesn't apply any global CSS. To apply the library's typographic styles more broadly, you can take advantage of the mat-typography CSS class. This class will style all descendant native elements.

```html
<!-- By default, Angular Material applies no global styles to native elements. -->
<h1>This header is unstyled</h1>

<!-- Applying the mat-tyography class adds styles for native elements. -->
<section class="mat-typography">
  <h1>This header will be styled</h1>
</section>
```

## 定制

> Customization

定制排版是一个Angular Material 基于SASS主题化的扩展。类似于创建自定义主题，你可以创建一个定制的**排版设置**。

> Typography customization is an extension of Angular Material's SASS-based theming. Similar to creating a custom theme, you can create a custom typography configuration.

```typescript
@import '~@angular/material/theming';

// Define a custom typography config that overrides the font-family as well as the
// `headlines` and `body-1` levels.
$custom-typography: mat-typography-config(
  $font-family: monospace,
  $headline: mat-typography-level(32px, 48px, 700),
  $body-1: mat-typography-level(16px, 24px, 500)
);
```

正如上面例子展示的，通过使用`mat-typography-config`功能创建了一个排版设置，其中设置了字体和前面描述的排版层级。每个由`mat-typography-level`定义的排版层级，都需要`font-size`, `line-height`, 和`font-weight`。

> As the above example demonstrates, a typography configuration is created by using the mat-typography-config function, which is given both the font-family and the set of typographic levels described earlier. Each typographic level is defined by the mat-typography-level function, which requires a font-size, line-height, and font-weight.

一旦定制的排版定义创建完成，就可以通过不同的SASS mixins来生成样式。

> Once the custom typography definition is created, it can be consumed to generate styles via different SASS mixins.


```typescript
// Override typography CSS classes (e.g., mat-h1, mat-display-1, mat-typography, etc.).
@include mat-base-typography($custom-typography);

// Override typography for a specific Angular Material components.
@include mat-checkbox-typography($custom-typography);

// Override typography for all Angular Material, including mat-base-typography and all components.
@include angular-material-typography($custom-typography);
```

如果你使用的是Material主题，也可以将你的排版设置传给`mat-core` mixin。

> If you're using Material's theming, you can also pass in your typography config to the `mat-core` mixin:

```typescript
// Override the typography in the core CSS.
@include mat-core($custom-typography);
```


关于排版方法和默认配置更多细节，请点击[这里](https://github.com/angular/material2/blob/master/src/lib/core/typography/_typography.scss)。

> For more details about the typography functions and default config, see the [source](https://github.com/angular/material2/blob/master/src/lib/core/typography/_typography.scss).

## 定制CSS文件中的Material排版

> Material typography in your custom CSS

Angular Material 引入了排版工具mixins和函数方法，这样你可以自定义你自己的组件了：

> Angular Material includes typography utility mixins and functions that you can use to customize your own components:

* `mat-font-size($config, $level)` - 获取基于提供的config与level参数的`font-size`
* `mat-font-family($config)` - 获取基于提供的config参数的`font-family`
* `mat-line-height($config, $level)` - 获取基于提供的config与level参数的`line-height`
* `mat-font-weight($config, $level)` - 获取基于提供的config与level参数的`font-weight`
* `mat-typography-level-to-styles($config, $level)` - 接受配置对象和排版层级的`mixin`，输出简单的CSS `font`声明。

```typescript
@import '~@angular/material/theming';

// Create a config with the default typography levels.
$config: mat-typography-config();

// Custom header that uses only the Material `font-size` and `font-family`.
.unicorn-header {
  font-size: mat-font-size($config, headline);
  font-family: mat-font-family($config);
}

// Custom title that uses all of the typography styles from the `title` level.
.unicorn-title {
  @include mat-typography-level-to-styles($config, title);
}
```
a