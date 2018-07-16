[toc]

# 概述

> OVERVIEW

`mat-tree`提供了一个Material Design样式的树，可用于展示分级的数据。

> The `mat-tree` provides a Material Design styled tree that can be used to display hierarchy data.

此树控件基于CDK树控件构建，并且使用了类似的数据源输入和模板接口，不同的是其元素和属性选择器的前缀使用`mat-`代替了`cdk-`。

> This tree builds on the foundation of the CDK tree and uses a similar interface for its data source input and template, except that its element and attribute selectors will be prefixed with `mat-` instead of `cdk-`.

树有两种类型：平面树与嵌套树。两种树的DOM结构不同。

> There are two types of trees: Flat tree and nested tree. The DOM structures are different for these two types of trees.

## 平面树

> Flat tree

在一个平面树中，其分级是平面的；节点不在节点内部渲染，但会在相邻序列中渲染。一个`TreeFlattener`的实例是用于生成分级数据项的平面列表。每个树节点的“等级”可通过`TreeControl`的`getLevel`方法获得；此等级可用于根据不同的等级应用不同的样式。

> In a flat tree, the hierarchy is flattened; nodes are not rendered inside of each other, but instead are rendered as siblings in sequence. An instance of `TreeFlattener` is used to generate the flat list of items from hierarchical data. The "level" of each tree node is read through the `getLevel` method of the `TreeControl`; this level can be used to style the node such that it is indented to the appropriate level.

```html
<mat-tree>
  <mat-tree-node> parent node </mat-tree-node>
  <mat-tree-node> -- child node1 </mat-tree-node>
  <mat-tree-node> -- child node2 </mat-tree-node>
</mat-tree>
```

通常平面树更容易样式化和观察。对于滚动变量也更友好，例如无限滚动和虚拟滚动。

> Flat trees are generally easier to style and inspect. They are also more friendly to scrolling variations, such as infinite or virtual scrolling

## 嵌套树

> Nested tree

在嵌套树中，子节点位于其父节点的DOM中。父节点有一个包含所有子节点的封装。

> In Nested tree, children nodes are placed inside their parent node in DOM. The parent node has an outlet to keep all the children nodes.

```html
<mat-tree>
   <mat-nested-tree-node>
     parent node
     <mat-nested-tree-node> -- child node1 </mat-nested-tree-node>
     <mat-nested-tree-node> -- child node2 </mat-nested-tree-node>
   </mat-nested-tree-node>
</mat-tree>
```

当继承关系用于显示表示时，嵌套树可以更好的起效，这种情况对于平面树来说就会难以完成了。

> Nested trees are easier to work with when hierarchical relationships are visually represented in ways that would be difficult to accomplish with flat nodes.

## 特性

> Features

`<mat-tree>`本身仅渲染树形结构。其他的特性可通过在内部的节点模板上（例如，间隔与切换等）添加行为来在树的顶点上建立。影响渲染数据的交互操作应通过表单数据源来进行分页。

> The `<mat-tree>` itself only deals with the rendering of a tree structure. Additional features can be built on top of the tree by adding behavior inside node templates (e.g., padding and toggle). Interactions that affect the rendered data (such as expand/collapse) should be propagated through the table's data source.

## TreeControl控件

> TreeControl

`TreeControl`控制了树节点的展开/收缩状态。用户可以通过树控件来递归地展开/收缩一个树节点。对于嵌套树节点，`getChildren`函数需要传入`NestedTreeControl`来使其递归地起效。对于平面树节点，`getLevel`和`isExpandable`函数需要传入`FlatTreeControl`来使其递归起效。

> The `TreeControl` controls the expand/collapse state of tree nodes. Users can expand/collapse a tree node recursively through tree control. For nested tree node, `getChildren` function need to pass to the `NestedTreeControl` to make it work recursively. For flattened tree node, `getLevel` and `isExpandable` functions need to pass to the `FlatTreeControl` to make it work recursively.

## 切换

> Toggle

可以在树节点模板中添加一个`matTreeNodeToggle`来展开/收缩树节点。切换按钮可以`TreeControl`中切换展开/收缩函数，通过设置`[matTreeNodeToggleRecursive]`为`true`，可以递归地来展开/收缩树节点。

> A `matTreeNodeToggle` can be added in the tree node template to expand/collapse the tree node. The toggle toggles the expand/collapse functions in `TreeControl` and is able to expand/collapse a tree node recursively by setting `[matTreeNodeToggleRecursive]` to `true`.

## 填充（仅平面树）

> Padding (Flat tree only)

`matTreeNodePadding`可放置于一个平面树的节点模板中，来展示一个平面树节点的`level`等级信息。

> The `matTreeNodePadding` can be placed in a flat tree's node template to display the `level` information of a flat tree node.

嵌套树不需要此填充，因为在DOM中可以较简单地添加继承结构。

> Nested tree does not need this padding since padding can be easily added to the hierarchy structure in DOM.

## 无障碍 

> Accessibility

没有文本或者标签的树，需要通过`aria-label`或者`aria-labelledby`指定一个有意义的标签。如果没有设置，`aria-readonly`默认为`true`。

> Trees without text or labels should be given a meaningful label via `aria-label` or `aria-labelledby`. The `aria-readonly` defaults to `true` if it's not set.

树的角色是`tree`。父节点为`role="group"`，叶节点为`role="treeitem"`。

> Tree's role is `tree`. Parent nodes are given `role="group"`, while leaf nodes are given `role="treeitem"`

`mat-tree`本身并不管理焦点/键盘交互等。用户可在其应用中添加期望的焦点/键盘交互。

> `mat-tree` does not manage any focus/keyboard interaction on its own. Users can add desired focus/keyboard interactions in their application.

# API

## 树(tree)的API参考

> API reference for Angular Material tree

```ts
import {MatTreeModule} from '@angular/material/tree';
```

## 指令

> Directives

### MatTreeNode

包含Material design样式的CdkTree节点的封装。

> Wrapper for the CdkTree node with Material design styles.

Selector: `mat-tree-node`

Exported as: `matTreeNode`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>disabled: boolean|此组件是否禁用。|Whether the component is disabled.
@Input()<br>role: 'treeitem' \| 'group'||

### MatTreeNodeDef **extends CdkTreeNodeDef**

包含Material design样式的CdkTree节点定义的封装。

> Wrapper for the CdkTree node definition with Material design styles.

Selector: `[matTreeNodeDef]`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('matTreeNode')<br>data: T||
@Input( matTreeNodeDefWhen)<br>when: (index: number, nodeData: T) => boolean|如果此节点用于提供节点数据和索引，此函数应返回true。如果不定义，则认为当没有其他函数返回true时，则此节点使用默认节点模板。对于每个节点来说，应至少传递一个when函数或默认给一个不定义。|Function that should return true if this node template should be used for the provided node data and index. If left undefined, this node will be considered the default node template to use when no other functions return true for the data. For every node, there must be at least one when function that passes or an undefined to default.
template: TemplateRef<any\>||

### MatNestedTreeNode

包含Material design样式的CdkTree嵌套节点的封装。

> Wrapper for the CdkTree nested node with Material design styles.

Selector: `mat-nested-tree-node`

Exported as: `matNestedTreeNode`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>disabled: boolean|此组件是否失效。|Whether the component is disabled.
@Input('matNestedTreeNode')<br>node: T||
nodeOutlet: QueryList<MatTreeNodeOutlet\>||

### MatTreeNodePadding **extends CdkTreeNodePadding**

包含Material design样式的CdkTree内边距的封装。

> Wrapper for the CdkTree padding with Material design styles.

Selector: `[matTreeNodePadding]`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('matTreeNodePaddingIndent')<br>indent: number|等级之间的间隔。默认为40px。|The indent for each level. Default number 40px from material design menu sub-menu spec.
@Input('matTreeNodePadding')<br>level: number|树节点深度的等级。内边距为等级*间隔。|The level of depth of the tree node. The padding will be level * indent pixels.

### MatTree **extends CdkTree**

包含Material design样式的CdkTree的封装。

> Wrapper for the CdkTree with Material design styles.

Selector: `mat-tree`

Exported as: `matTree`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input()<br>dataSource: DataSource<T\> \| Observable<T[]\> \| T[]|提供包含了要渲染的最新数据数组的流。会受树的视图窗口（当前屏幕显示的dataNodes）流的影响。数据源可为数据数组的一个observable对象，或者为一个要渲染的数据数组。|Provides a stream containing the latest data array to render. Influenced by the tree's stream of view window (what dataNodes are currently on screen). Data source can be an observable of data array, or a data array to render.
@Input()<br>trackBy: TrackByFunction<T\>|用于检测数据变更的不同点的跟踪函数。使用方法与ngFor的trackBy类似。通过基于数据关联到函数来识别一个数据，判断一个节点是否被添加/删除/修改，以优化节点操作。接收包含两个参数的函数，参数为index和item。|Tracking function that will be used to check the differences in data changes. Used similarly to ngFor trackBy function. Optimize node operations by identifying a node based on its data relative to the function to know if a node should be added/removed/moved. Accepts a function that takes two parameters, index and item.
@Input()<br>treeControl: TreeControl<T\>|树的控制器|The tree controller
viewChange: new BehaviorSubject<{ start: number; end: number; }\>({ start: 0, end: Number.MAX_VALUE })|包含当前屏幕展示的是哪一行的最新信息的流。可供数据源使用，来确定应提供什么数据。|Stream containing the latest information on what rows are being displayed on screen. Can be used by the data source to as a heuristic of what data should be provided.

**方法**

> Method

insertNode
---
- 描述
  - 为数据节点模板创建一个内嵌视图，并在数据节点视图的容器中，将其放入正确的索引位置。
  - Create the embedded view for the data node template and place it in the correct index location within the data node view container.
- 参数
  - nodeData: T	
  - index: number	
  - viewContainer?: ViewContainerRef	
  - parentData?: T

renderNodeChanges
---
- 描述
  - 检查数据的变更（节点的增加/删除/移动）并渲染。
  - Check for changes made in the data and render each change (node added/removed/moved).
- 参数
  - data: T[]	
  - dataDiffer: IterableDiffer<T> = this._dataDiffer	
  - viewContainer: ViewContainerRef = this._nodeOutlet.viewContainer	
  - parentData?: T

### MatTreeNodeToggle **extends CdkTreeNodeToggle**

包含Material design样式的CdkTree的切换按钮的封装。

> Wrapper for the CdkTree's toggle with Material design styles.

Selector: `[matTreeNodeToggle]`

**属性**

> Properties

属性名|描述|Description
-|-|-
@Input('matTreeNodeToggleRecursive')<br>recursive: boolean||

### MatTreeNodeOutlet

嵌套CdkNode的出口。在标签中添加[matTreeNodeOutlet]，在其中放置子dataNodes。

> Outlet for nested CdkNode. Put [matTreeNodeOutlet] on a tag to place children dataNodes inside the outlet.

Selector: `[matTreeNodeOutlet]`

**属性**

> Properties

属性名|描述|Description
-|-|-
viewContainer: ViewContainerRef||

## 附加类

> Additional classes

### MatTreeFlattener

Tree flattener用于将一个正常类型的节点转换为包含子节点与等级信息的节点。将类型T的嵌套节点转换为类型F的平面节点。

> Tree flattener to convert a normal type of node to node with children & level information. Transform nested nodes of type T to flattened nodes of type F.

例如，类型T的输入数据为嵌套的，其中包含子节点数据：SomeNode: { key: 'Fruits', children: [ NodeOne: { key: 'Apple', }, NodeTwo: { key: 'Pear', } ] }。将树平面化后，数据结构变为：SomeNode: { key: 'Fruits', expandable: true, level: 1 }, NodeOne: { key: 'Apple', expandable: false, level: 2 }, NodeTwo: { key: 'Pear', expandable: false, level: 2 }，并输出的平面类型为包含附加信息的F。

> For example, the input data of type T is nested, and contains its children data: SomeNode: { key: 'Fruits', children: [ NodeOne: { key: 'Apple', }, NodeTwo: { key: 'Pear', } ] } After flattener flatten the tree, the structure will become SomeNode: { key: 'Fruits', expandable: true, level: 1 }, NodeOne: { key: 'Apple', expandable: false, level: 2 }, NodeTwo: { key: 'Pear', expandable: false, level: 2 } and the output flattened type is F with additional information.

**属性**

> Properties

属性名|描述|Description
-|-|-
getChildren: (node: T) => Observable<T[]\> \| T[]||
getLevel: (node: F) => number||
isExpandable: (node: F) => boolean||
transformFunction: (node: T, level: number) => F||

**方法**

> Method

expandFlattenedNodes
---
- 描述
  - 
  - Expand flattened node with current expansion status. The returned list may have different length.
- 参数
  - nodes: F[]	
  - treeControl: TreeControl<F>	
- 返回
  - F[]

flattenNodes
---
- 描述
  - 将类型T节点列表平面化为节点F的平面版本。注意类型T可能是嵌套的，structuredData的长度也可能与返回的列表F[]不同。
  - Flatten a list of node type T to flattened version of node F. Please note that type T may be nested, and the length of structuredData may be different from that of returned list F[].
- 参数
  - structuredData: T[]	
- 返回
  - F[]

### MatTreeFlatDataSource **extends DataSource**

平面树的数据源。数据源需要处理树节点的收缩/展开，并了改变MatTree的数据供给。MatTreeFlattener使类型T的嵌套树节点平面化，并转换为类型F后提供给MatTree。

> Data source for flat tree. The data source need to handle expansion/collapsion of the tree node and change the data feed to MatTree. The nested tree nodes of type T are flattened through MatTreeFlattener, and converted to type F for MatTree to consume.

**属性**

> Properties

属性名|描述|Description
-|-|-
data: T[]||

**方法**

> Method

connect
---
- 参数
  - collectionViewer: CollectionViewer	
- 返回
  - Observable<F[]>

disconnect
---

### MatTreeNestedDataSource **extends DataSource**

嵌套树的数据源。

> Data source for nested tree.

嵌套树的数据源不必考虑节点的平面，以及收缩/扩展的方法。由TreeControl和每个非叶节点来处理收缩/扩展。

> The data source for nested tree doesn't have to consider node flattener, or the way to expand or collapse. The expansion/collapsion will be handled by TreeControl and each non-leaf node.

**属性**

> Properties

属性名|描述|Description
-|-|-
data: T[]|嵌套树的数据。|Data for the nested tree

**方法**

> Method

connect
---
- 参数
  - collectionViewer: CollectionViewer	
- 返回
  - Observable<F[]>

disconnect
---

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*