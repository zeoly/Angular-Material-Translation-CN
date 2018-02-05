[toc]

# 概述

> OVERVIEW

`<mat-card>`是同一个主题中的文本、照片、操作等的一个内容容器。

> `<mat-card>` is a content container for text, photos, and actions in the context of a single subject.

## 基本卡片区域

> Basic card sections

大多数基本卡片只需要包含某些内容的一个<mat-card>元素。但Angular Material提供了一些在<mat-card>中使用的预设区域：

> The most basic card needs only an <mat-card> element with some content. However, Angular Material provides a number of preset sections that you can use inside of an <mat-card>:

元素|描述|Description
-|-|-
`<mat-card-title>`|卡片标题|Card title
`<mat-card-subtitle>`|卡片副标题|Card subtitle
`<mat-card-content>`|卡片主要内容。主要为内容块|Primary card content. Intended for blocks of text
`<img mat-card-image>`|卡片图片。图片宽度为容器宽度|Card image. Stretches the image to the container width
`<mat-card-actions>`|卡片底部的按钮容器|Container for buttons at the bottom of the card
`<mat-card-footer>`|卡片最底部的区域|Section anchored to the bottom of the card

这些元素都提供了预设样式的内容容器，无需额外的API。但其中`<mat-card-actions>`的`align`属性可设置为`'start'`或`'end'`来控制容器中操作元素的位置。

> These elements primary serve as pre-styled content containers without any additional APIs. However, the `align` property on `<mat-card-actions>` can be used to position the actions at the `'start'` or `'end'` of the container.

## 卡片头部

> Card headers

除了上述的区域外，`<mat-card-header>`可在卡片中加入一个富头部。此头部包括如下：

> In addition to the aforementioned sections, `<mat-card-header>` gives the ability to add a rich header to a card. This header can contain:

元素|描述|Description
-|-|-
`<mat-card-title>`|头部的标题|A title within the header
`<mat-card-subtitle>`|头部的副标题|A subtitle within the header
`<img mat-card-avatar>`|头部作为头像的一个图片|An image used as an avatar within the header

## 标题组

> Title groups

`<mat-card-title-group>`可用于合并标题、副标题、图片为一个区域。此元素可包括如下：

> `<mat-card-title-group>` can be used to combine a title, subtitle, and image into a single section. This element can contain:

- `<mat-card-title>`
- `<mat-card-subtitle>`
- One of:
  - `<img mat-card-sm-image>`
  - `<img mat-card-md-image>`
  - `<img mat-card-lg-image>`

## 无障碍

> Accessibility

卡片的应用场景比较多，且可以包含多种类型的内容。因此无障碍的体验取决于如何使用`<mat-card>`。

> Cards can be used in a wide variety of scenarios and can contain many different types of content. Due to this dynamic nature, the appropriate accessibility treatment depends on how `<mat-card>` is used.

### 组，区域和标志

> Group, region, and landmarks

对于代表语义含义的UI的表达部分，有几种ARIA角色。根据应用中卡片内容的不容含义，通常应在`<mat-card>`元素中应用[`role="group"`](https://www.w3.org/TR/wai-aria/roles#group), [`role="region"`](https://www.w3.org/TR/wai-aria/roles#region), 或[one of the landmark roles][3]。

> There are several ARIA roles that communicate that a portion of the UI represents some semantically meaningful whole. Depending on what the content of the card means to your application, [`role="group"`](https://www.w3.org/TR/wai-aria/roles#group), [`role="region"`](https://www.w3.org/TR/wai-aria/roles#region), or [one of the landmark roles][3] should typically be applied to the `<mat-card>` element.

当卡片作为纯装饰容器，并不表达一个单一主题内容组的含义时，角色不是必须的。在这种情况下，卡片的内容应当遵循文档内容的标准实践。

> A role is not necessary when the card is used as a purely decorative container that does not convey a meaningful grouping of related content for a single subject. In these cases, the content of the card should follow standard practices for document content.

### 焦点

> Focus

为了更好地使用卡片，最好在`<mat-card>`中合理使用`tabindex`。如果卡片是应用中的主要交互机制，则设置`tabindex="0"`较合适。如果关注在卡片上，但并不是文档流的一部分，则设置`tabindex="-1"`较合适。

> Depending on how cards are used, it may be appropriate to apply a `tabindex` to the `<mat-card>` element. If cards are a primary mechanism through which user interact with the application, `tabindex="0"` is appropriate. If attention can be sent to the card, but it's not part of the document flow, `tabindex="-1"` is appropriate.

如果卡片表现为一个纯装饰的容器，则其不需要通过tab被聚焦。在这种情况下，卡片内容应遵循正常的tab顺序。

> If the card acts as a purely decorative container, it does not need to be tabbable. In this case, the card content should follow normal best practices for tab order.

# API

## 卡片(card)的API参考

> API reference for Angular Material card

```ts
import {MatCardModule} from '@angular/material/card';
```

## 指令

> Directives

### MatCard

一个添加了Material design卡片样式的基础内容容器组件。

> A basic content container component that adds the styles of a Material design card.

当此组件单独使用时，也提供了一些常用卡片区域的预设样式，包括：

> While this component can be used alone, it also provides a number of preset styles for common card sections, including:

- mat-card-title
- mat-card-subtitle
- mat-card-content
- mat-card-actions
- mat-card-footer

Selector: `mat-card`

Exported as: `matCard`

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*