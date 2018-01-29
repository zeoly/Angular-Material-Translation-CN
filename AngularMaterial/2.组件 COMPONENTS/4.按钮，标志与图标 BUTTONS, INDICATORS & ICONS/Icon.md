[toc]

# 概述

> OVERVIEW

`mat-icon`可以让你方便地在应用中使用*矢量*图标。此指令支持图标字体与SVG图标，但不支持位图格式（如png，jpg等）。

> `mat-icon` makes it easier to use *vector-based* icons in your app. This directive supports both icon fonts and SVG icons, but not bitmap-based formats (png, jpg, etc.).

## 注册图标

> Registering icons

`MatIconRegistry`是一个可注入的服务，可以用于将图标名和SVG URL相关联，并为CSS字体类定义别名。其中的方法在后续的API中有说明。

> `MatIconRegistry` is an injectable service that allows you to associate icon names with SVG URLs and define aliases for CSS font classes. Its methods are discussed below and listed in the API summary.

## 连体的字体图标

> Font icons with ligatures

有些字体会设计为使用[连体](https://en.wikipedia.org/wiki/Typographic_ligature)来展示图标，例如渲染文字"home"为一个home图像。可以将文字放入`mat-icon`组件中，来使用连体图标。

> Some fonts are designed to show icons by using [ligatures](https://en.wikipedia.org/wiki/Typographic_ligature), for example by rendering the text "home" as a home image. To use a ligature icon, put its text in the content of the `mat-icon` component.

默认`<mat-icon>`要求为[Material图标字体](http://google.github.io/material-design-icons/#icon-font-for-the-web)。（仍然需要引入HTML来加载字体与对应CSS）。你可以在CSS类的中，或者使用`MatIconRegistry.registerFontClassAlias`预注册的一个别名中，通过设置`fontSet`来指定一个不同的字体。

> By default, `<mat-icon>` expects the [Material icons font](http://google.github.io/material-design-icons/#icon-font-for-the-web). (You will still need to include the HTML to load the font and its CSS, as described in the link). You can specify a different font by setting the `fontSet` input to either the CSS class to apply to use the desired font, or to an alias previously registered with `MatIconRegistry.registerFontClassAlias`.

## 字体图标与CSS

> Font icons with CSS

可以通过为每个图标象形文字定义CSS，字体也可以展示图标，通常使用`:before`选择器使图标显示。[FontAwesome](https://fortawesome.github.io/Font-Awesome/examples/)使用的就是这种方式来展示图标。要使用这样的字体，需要在字体的CSS类（无论是类本身或者使用`MatIconRegistry.registerFontClassAlias`注册的别名）中设置`fontSet`输入，并在要显示的指定图标的类中设置`fontIcon`。

> Fonts can also display icons by defining a CSS class for each icon glyph, which typically uses a `:before` selector to cause the icon to appear. [FontAwesome](https://fortawesome.github.io/Font-Awesome/examples/) uses this approach to display its icons. To use such a font, set the `fontSet` input to the font's CSS class (either the class itself or an alias registered with `MatIconRegistry.registerFontClassAlias`), and set the `fontIcon` input to the class for the specific icon to show.

对于字体图标的两种类型，当`fontSet`没有被`MatIconRegistry.setDefaultFontSetClass`明确地设置时，你可以指定默认的字体类来使用。

> For both types of font icons, you can specify the default font class to use when `fontSet` is not explicitly set by calling `MatIconRegistry.setDefaultFontSetClass`.

## SVG图标

> SVG icons

当`mat-icon`组件展示一个SVG图标时，是将SVG内容作为组件的子元素直接插入到页面中。（而不是使用一个tag或者div的背景图片）。这使得在SVG图标中应用CSS样式更加简单。例如SVG内容的默认颜色是CSS的[currentColor](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#currentColor_keyword)值。则默认情况下SVG图标与周围的文字保持同一颜色，并且你可以通过在`mat-icon`元素中设置"color"样式来改变颜色。

> When an `mat-icon` component displays an SVG icon, it does so by directly inlining the SVG content into the page as a child of the component. (Rather than using an  tag or a div background image). This makes it easier to apply CSS styles to SVG icons. For example, the default color of the SVG content is the CSS [currentColor](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#currentColor_keyword) value. This makes SVG icons by default have the same color as surrounding text, and allows you to change the color by setting the "color" style on the `mat-icon` element.

为了避免XSS漏洞，任何传入`MatIconRegistry`的SVG URL必须使用Angular的`DomSanitizer`服务来标记为可信的资源。

> In order to prevent XSS vulnerabilities, any SVG URLs passed to the `MatIconRegistry` must be marked as trusted resource URLs by using Angular's `DomSanitizer` service.

注意所有SVG图标通过XmlHttpRequest获取，并且因为同源策略，其URL必须与所属页面在用一个域下，或者其服务设置为允许跨域访问。

> Also note that all SVG icons are fetched via XmlHttpRequest, and due to the same-origin policy, their URLs must be on the same domain as the containing page, or their servers must be configured to allow cross-domain access.

### 命名图标

> Named icons

使用`MatIconRegistry`的`addSvgIcon`或`addSvgIconInNamespace`方法，可以将一个图标URL与名称关联。在注册一个图标后，可通过设置`svgIcon`输入来显示。对于在默认命名空间中的图标，直接使用名称即可。对于非默认命名空间的，使用格式`[namespace]:[name]`。

> To associate a name with an icon URL, use the `addSvgIcon` or `addSvgIconInNamespace` methods of `MatIconRegistry`. After registering an icon, it can be displayed by setting the `svgIcon` input. For an icon in the default namespace, use the name directly. For a non-default namespace, use the format `[namespace]:[name]`.

### 图标集合

> Icon sets

图标集合允许将多个图标分组到一个SVG文件中。可在单个根`<svg>`的`<defs>`区中增加多个嵌套的`<svg>`来进行分组。每个嵌套的tag由一个`id`属性来区分。此`id`也用于图标的名称。

> Icon sets allow grouping multiple icons into a single SVG file. This is done by creating a single root `<svg>` tag that contains multiple nested `<svg>` tags in its `<defs>` section. Each of these nested tags is identified with an `id` attribute. This `id` is used as the name of the icon.

图标集合使用`MatIconRegistry`的`addSvgIconSet`或`addSvgIconSetInNamespace`方法进行注册。当图标集合注册完成后，可通过`id`属性来访问每个内嵌的图标。要显示图标集合中的一个图标，方法与设置单独注册的图标一样，设置`svgIcon`输入就可以。

> Icon sets are registered using the `addSvgIconSet` or `addSvgIconSetInNamespace` methods of `MatIconRegistry`. After an icon set is registered, each of its embedded icons can be accessed by their `id` attributes. To display an icon from an icon set, use the `svgIcon` input in the same way as for individually registered icons.

可在同一个命名空间中注册多个图标集合。请求一个id出现在多个图标集合中的图标，则将返回最近注册的图标。

> Multiple icon sets can be registered in the same namespace. Requesting an icon whose id appears in more than one icon set, the icon from the most recently registered set will be used.

## 主题

> Theming

图标默认会使用当前字体颜色（`currentColor`）。可使用`color`属性来改变颜色以适配当前主题颜色。可改变为`'primary'`, `'accent'`或者`'warn'`。

> By default, icons will use the current font color (`currentColor`). this color can be changed to match the current theme's colors using the `color` attribute. This can be changed to `'primary'`, `'accent'`, or `'warn'`.

## 无障碍

> Accessibility

与`<img>`元素相似，图标本身不包含任何对屏幕阅读者有用的信息。`<mat-icon>`的用户必须提供额外关于图标如何使用的信息。基于下述的用例，`mat-icon`默认标记为`aria-hidden="true"`，也可以在元素中添加`aria-hidden="false"`来复写。

> Similar to an `<img>` element, an icon alone does not convey any useful information for a screen-reader user. The user of `<mat-icon>` must provide additional information pertaining to how the icon is used. Based on the use-cases described below, `mat-icon` is marked as `aria-hidden="true"` by default, but this can be overriden by adding `aria-hidden="false"` to the element.

在考虑无障碍时，可使用下面三种策略的一种来放置图标：

> In thinking about accessibility, it is useful to place icon use into one of three categories:

1. **装饰式**：图标并不表达任何实际的语义，仅仅是纯装饰。
2. **交互式**：用户会点击或者与图标有其他交互来执行一些动作。
3. **指示器**：图标是非交互的，但表达一些信息，例如某种状态。

> 1. **Decorative**: the icon conveys no real semantic meaning and is purely cosmetic.
> 2. **Interactive**: a user will click or otherwise interact with the icon to perform some action.
> 3. **Indicator**: the icon is not interactive, but it conveys some information, such as a status.

### 装饰性图标

> Decorative icons

当图标用于纯装饰，不表达实际语义时，需要将`<mat-icon>`元素标记为`aria-hidden="true"`。

> When the icon is puely cosmetic and conveys no real semantic meaning, the `<mat-icon>` element should be marked with `aria-hidden="true"`.

### 交互性图标

> Interactive icons

图标本身对于屏幕阅读者用户来说是非交互元素；当用户与页面的某些图标交互时，一个良好的图标应包含以下交互：

> Icons alone are not interactive elements for screen-reader users; when the user would interact with some icon on the page, a more appropriate element should "own" the interaction:

- `<mat-icon>`元素必须是`<button>`或`<a>`元素的子元素。
- 必须将`<mat-icon>`元素标记为`aria-hidden="true"`。
- 父`<button>`或`<a>`元素应包含一个有意义的标签，可通过直接的文本内容，`aria-label`, 或者`aria-labelledby`。

> - The `<mat-icon>` element should be a child of a `<button>` or `<a>` element.
> - The `<mat-icon>` element should be marked with `aria-hidden="true"`.
> - The parent `<button>` or `<a>` should either have a meaningful label provided either through direct text content, `aria-label`, or `aria-labelledby`.

### 指示器图标

> Indicator icons

当显示的图标向用户表达的某些信息时，这些信息必须对屏幕阅读者也可读。最直接的方法如下

> When the presence of an icon communicates some information to the user, that information must also be made available to screen-readers. The most straightforward way to do this is to

1. 将`<mat-icon>`标记为`aria-hidden="true"`。
2. 在`<mat-icon>`元素中添加一个`<span>`作为兄弟元素，其中文字表达了与图标相同的信息。
3. 在`<span>`中加入`cdk-visually-hidden`类。这样信息在屏幕中不可见，但对屏幕阅读者用户是有效的。

> 1. Mark the `<mat-icon>` as `aria-hidden="true"`
> 2. Add a `<span>` as an adjacent sibling to the `<mat-icon>` element with text that conveys the same information as the icon.
> 3. Add the `cdk-visually-hidden` class to the `<span>`. This will make the message invisible on-screen but still available to screen-reader users.

# API

## 图标的API参考

> API reference for Angular Material icon

```typescript
import {MatIconModule} from '@angular/material/icon';
```

## 服务

> Services

### MatIconRegistry

组件用于注册和展示图标的服务。

> Service to register and display icons used by the component.

- 通过命名空间和名称注册图标URL。
- 通过命名空间注册图标集合URL。
- 为CSS类注册别名，用于图标字体。
- 从URL加载图标并从图标集合中提取单独的图标。

> - Registers icon URLs by namespace and name.
> - Registers icon set URLs by namespace.
> - Registers aliases for CSS classes, for use with icon fonts.
> - Loads icons from URLs and extracts individual icons from icon sets.

**方法**

> Methods

addSvgIcon
---
- 描述
  - 通过默认命名空间的URL注册一个图标。
  - Registers an icon by URL in the default namespace.
- 参数
  - iconName: string：需要注册的图标的名称。
  - Name under which the icon should be registered.
  - url: SafeResourceUrl
- 返回
  - this

addSvgIconInNamespace
---
- 描述
  - 通过指定的命名空间的URL注册一个图标。
  - Registers an icon by URL in the specified namespace.
- 参数
  - namespace: string：需要注册的图标的命名空间。	
  - Namespace in which the icon should be registered.
  - iconName: string：需要注册的图标的名称。
  - Name under which the icon should be registered.
  - url: SafeResourceUrl
- 返回
  - this

addSvgIconSet
---
- 描述
  - 在默认命名空间中通过URL注册图标集合。
  - Registers an icon set by URL in the default namespace.
- 参数
  - url: SafeResourceUrl
- 返回
  - this

addSvgIconSetInNamespace
---
- 描述
  - 在指定的命空间中通过URL注册图标集合。
  - Registers an icon set by URL in the specified namespace.
- 参数
  - namespace: string：需要注册的图标集合的命名空间。	
  - Namespace in which to register the icon set.
  - url: SafeResourceUrl
- 返回
  - this

classNameForFontAlias
---
- 描述
  - 返回与registerFontClassAlias调用的别称关联的CSS类名。如果没有CSS类关联，则返回未修改的别名。
  - Returns the CSS class name associated with the alias by a previous call to registerFontClassAlias. If no CSS class has been associated, returns the alias unmodified.
- 参数
  - alias: string
- 返回
  - string

getDefaultFontSetClass
---
- 描述
  - 当一个组件没有fontSet输入值，并且没有通过名称或者URL加载图标时，返回图标字体使用的CSS类名。
  - Returns the CSS class name to be used for icon fonts when an component does not have a fontSet input value, and is not loading an icon by name or URL.
- 返回
  - string

getNamedSvgIcon
---
- 描述
  - 返回生成包含指定名称和命名空间的图标（例如一个`<svg>`DOM元素）的Observable。图标必须由addIcon或者addIconSet注册；如果未注册，Observable将抛出一个错误。
  - Returns an Observable that produces the icon (as an `<svg>` DOM element) with the given name and namespace. The icon must have been previously registered with addIcon or addIconSet; if not, the Observable will throw an error.
- 参数
  - name: string：要获取的图标的名称。
  - Name of the icon to be retrieved.
  - namespace: string = ''：查询图标的命名空间。
  - Namespace in which to look for the icon.
- 返回
  - Observable<SVGElement>

getSvgIconFromUrl
---
- 描述
  - 获取从指定的URL中生成图标（例如一个`<svg>`DOM元素）的Observable。URL的响应可能被缓存，所以不一定每次都是HTTP请求，但生成的元素都为原始获取图标的新的拷贝。（即并不包含之前返回的元素所做的任何修改）。
  - Returns an Observable that produces the icon (as an `<svg>` DOM element) from the given URL. The response from the URL may be cached so this will not always cause an HTTP request, but the produced element will always be a new copy of the originally fetched icon. (That is, it will not contain any modifications made to elements previously returned).
- 参数
  - safeUrl: SafeResourceUrl：获取SVG图标的URL。
  - URL from which to fetch the SVG icon.
- 返回
  - Observable<SVGElement>

registerFontClassAlias
---
- 描述
  - 为用于图标字体的CSS类名定义一个别名。用别名作为fontSet输入创建一个matIcon组件会在元素中应用此类名。
  - Defines an alias for a CSS class name to be used for icon fonts. Creating an matIcon component with the alias as the fontSet input will cause the class name to be applied to the element.
- 参数
  - alias: string：字体的别名。
  - Alias for the font.
  - className: string = alias：用于替代的别名的类名。
  - Class name override to be used instead of the alias.
- 返回
  - this

setDefaultFontSetClass
---
- 描述
  - 当组件没有fontSet输入值时，设置用于图标字体的CSS类名，并且不通过名称或者URL加载图标。
  - Sets the CSS class name to be used for icon fonts when ancomponent does not have a fontSet input value, and is not loading an icon by name or URL.
- 参数
  - className: string
- 返回
  - this

## 指令

> Directives

### MatIcon

用于展示图标的组件。有如下使用方法：

> Component to display an icon. It can be used in the following ways:

- 指定svgIcon输入，通过MatIconRegistry的addSvgIcon, addSvgIconInNamespace, addSvgIconSet, 或addSvgIconSetInNamespace方法注册的URL来加载一个SVG图标。如果svgIcon值包含冒号，则会认定为"[namespace]:[name]"的格式，如果没有冒号，则认为是默认命名空间的图标名称。

> - Specify the svgIcon input to load an SVG icon from a URL previously registered with the addSvgIcon, addSvgIconInNamespace, addSvgIconSet, or addSvgIconSetInNamespace methods of MatIconRegistry. If the svgIcon value contains a colon it is assumed to be in the format "[namespace]:[name]", if not the value will be the name of an icon in the default namespace. Examples:

- 在组件内容中放入连体文字，使用字体连体作为一个图标。默认Material图标字体可参考[http://google.github.io/material-design-icons/#icon-font-for-the-web](http://google.github.io/material-design-icons/#icon-font-for-the-web)。可通过设置fontSet输入，或者CSS类，或者MatIconRegistry.registerFontClassAlias注册的别名，指定字体来使用其他的期望的字体。

> - Use a font ligature as an icon by putting the ligature text in the content of the component. By default the Material icons font is used as described at [http://google.github.io/material-design-icons/#icon-font-for-the-web](http://google.github.io/material-design-icons/#icon-font-for-the-web). You can specify an alternate font by setting the fontSet input to either the CSS class to apply to use the desired font, or to an alias previously registered with MatIconRegistry.registerFontClassAlias. Examples:homesun

- 通过设置fontSet输入指定字体引入CSS规则，并设置fontIcon输入来指定图标，来指定一个字体象形文字。通常fontIcon会指定一个CSS类，可通过:before选择器来展示此象形文字，可参考[https://fortawesome.github.io/Font-Awesome/examples/](https://fortawesome.github.io/Font-Awesome/examples/)。

> - Specify a font glyph to be included via CSS rules by setting the fontSet input to specify the font, and the fontIcon input to specify the icon. Typically the fontIcon will specify a CSS class which causes the glyph to be displayed via a :before selector, as in [https://fortawesome.github.io/Font-Awesome/examples/](https://fortawesome.github.io/Font-Awesome/examples/) Example:

Selector: `mat-icon`

Exported as: `matIcon`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>fontIcon: string|在字体集合中的图标的名称。|Name of an icon within a font set.
@Input()<br>fontSet: string|设置图标的字体。|Font set that the icon is a part of.
@Input()<br>svgIcon: string|SVG图标集合的图标的名称。|Name of the icon in the SVG icon set.
color: ThemePalette|组件的主题颜色。|Theme color palette for the component.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*