### [JavaScript DOM 扩展 ](#top) :grey_exclamation: <b id="top"></b>
`DOM1 级主要定义的是 HTML和 XML文档的底层结构。DOM2 和 DOM3级则在这个结构 的基础上引入了更多的交互能力 `

------
- [x] [`简介`](#target1)


-----
#####  :octocat: [1.简介](#top) <b id="target1"></b> 
`DOM2/3支持了更高级的 XML特性。为此，DOM2和 DOM3 级分为许多模块（模块之间具有某种关联），分别描述了 DOM 的某个非常具体的子集。这些模块 如下。 `

* `DOM2级核心（DOM Level 2 Core）：在 1级核心基础上构建，为节点添加了更多方法和属性。` 
* `DOM2级视图（DOM Level 2 Views）：为文档定义了基于样式信息的不同视图。` 
* `DOM2级事件（DOM Level 2 Events）：说明了如何使用事件与 DOM文档交互。` 
* `DOM2级样式（DOM Level 2 Style）：定义了如何以编程方式来访问和改变 CSS样式信息。` 
* `DOM2级遍历和范围（DOM Level 2 Traversal and Range）：引入了遍历 DOM文档和选择其特定 部分的新接口。` 
* `DOM2级 HTML（DOM Level 2 HTML）：在 1级 HTML基础上构建，添加了更多属性、方法和 新接口`

`DOM2级和 3级的目的在于扩展 DOM API，以满足操作 XML的所有需求，同时提供更好的错误处 理及特性检测能力 可以
通过下列代码来确定浏览器是否支持这些 DOM模块`

```node
var supportsDOM2Core = document.implementation.hasFeature("Core", "2.0");  
var supportsDOM3Core = document.implementation.hasFeature("Core", "3.0"); 
var supportsDOM2HTML = document.implementation.hasFeature("HTML", "2.0"); 
var supportsDOM2Views = document.implementation.hasFeature("Views", "2.0"); 
var supportsDOM2XML = document.implementation.hasFeature("XML", "2.0"); 
```

##### 针对XML命名空间的变化 [可忽略]
**`在今日(2020年)，XHTML5比起HTML5仍远远并非主流。 可以负略XHTML`**

`有了 XML 命名空间，不同 XML 文档的元素就可以混合在一起，共同构成格式良好的文档，而不 必担心发生命名
冲突。从技术上说，HTML不支持 XML命名空间，但 XHTML支持 XML命名空间`

##### Document 类型的变化
`DOM2级中的 Document 类型也发生了变化，包含了下列与命名空间有关的方法。 `

* `createElementNS(namespaceURI, tagName)：使用给定的 tagName 创建一个属于命名空 间 namespaceURI 的新元素`
* `createAttributeNS(namespaceURI, attributeName)：使用给定的 attributeName 创 建一个属于命名空间 namespaceURI 的新特性。 `
* `getElementsByTagNameNS(namespaceURI, tagName)：返回属于命名空间 namespaceURI 的 tagName 元素的 NodeList。 `




