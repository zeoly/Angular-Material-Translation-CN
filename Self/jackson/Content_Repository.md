[toc]

**JCR: Java Content Repository**

内容仓库提供存储、访问、内容管理等信息管理服务系统；扩展版本、访问控制（权限）、全文搜索、事件监控等功能。

内容仓库不是内容管理系统（CMS），虽然CMS通常有一套自己的内容仓库实现，但一般基于文件系统或关系型数据库。

**JSR 170: Content Repository for Java technology API**

**JSR 283: Content Repository for Java Technology API Version 2.0**



JSR-170 API对不同的人员提供了不同的好处：

- 对于开发者无需了解厂家的仓库特定的API，只要兼容JSR-170就可以通过JSR-170访问其仓库。
- 对于使用CMS的公司则无需花费资金用于在不同种类CMS的内容仓库之间进行转换。
- 对于CMS厂家，无需自己开发内容仓库，而专注于开发CMS应用。



Jackrabbit三层架构：

- Content Application Layer 内容应用层

- API Layer接口层（javax.jcr，org.apache.jackrabbit）

- Content Repository Implementation Layer 内容仓库实现层（Oak）



Oak：

- 在JCR实现层，依赖JCR代理，并依赖Oak API层，下层Oak core层
- 最外还是JCR接口层的，相当于Jackrabbit的优化实现，解决用户体验（个性化，交互，协同），架构（分布式，松耦合，多平台数据），硬件设计（水平而不是垂直）

## node state model 节点状态模型

### Persistent Workspace 持久化工作空间

JCR仓库由一个或多个工作空间组成，有自己的唯一名称

### Item

分为node或property，property为叶节点，内容数据在此。

每个工作空间至少有一个Item，即根节点。

#### Shared Nodes 共享节点

通常工作空间是item树，但在共享场景下，一个item可能有多个parent

### Names 名称

根节点名称为“”，空串，其他item均有名称

### Path 路径

用于定位item在workspace中的位置，从根节点计算，与文件路径类似

### Identifiers

每个node均有一个id

### Node Types 节点类型

每个节点都有类型，用于定义复杂存储对象

### Sessions

用户通过传递一系列的认证，以及要方位的空间名，来连接仓库。仓库返回session。

## Names 名称

JCR名称是有序成对的字符串(N, L)，N是JCR命名空间，L是JCR本地名称。

## 3.9 Shareable Nodes Model 可共享节点模型

通过不同的路径访问同一片数据，是内容存储系统中常见的特性，在JCR中称为共享节点。

## 3.13 Versioning Model 版本模型

版本记录了节点的状态，并支持在未来恢复至某个过去状态。
