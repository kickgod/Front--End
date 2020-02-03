### [JavaScript DOM 扩展 ](#top) :grey_exclamation: <b id="top"></b>
`DOM1 级主要定义的是 HTML和 XML文档的底层结构。DOM2 和 DOM3级则在这个结构 的基础上引入了更多的交互能力 `

------
- [x] [`DOM2/3简介和XML命名空间变化`](#target1)
- [x] [`样式`](#atrget2)

-----
#####  :octocat: [1.DOM2/3简介和XML命名空间变化[部分版本]](#top) <b id="target1"></b> 
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

[`在今日(2020年)，XHTML5比起HTML5仍远远并非主流。 可以负略XHTML`]

`有了 XML 命名空间，不同 XML 文档的元素就可以混合在一起，共同构成格式良好的文档，而不 必担心发生命名
冲突。从技术上说，HTML不支持 XML命名空间，但 XHTML支持 XML命名空间`

##### Document 类型的变化
`DOM2级中的 Document 类型也发生了变化，包含了下列与命名空间有关的方法。 `

* `createElementNS(namespaceURI, tagName)：使用给定的 tagName 创建一个属于命名空 间 namespaceURI 的新元素`
* `createAttributeNS(namespaceURI, attributeName)：使用给定的 attributeName 创 建一个属于命名空间 namespaceURI 的新特性。 `
* `getElementsByTagNameNS(namespaceURI, tagName)：返回属于命名空间 namespaceURI 的 tagName 元素的 NodeList。 `
* `setNamedItemNS(node)`：`添加 node，这个节点已经事先指定了命名空间信息。 由于一般都是通过元素访问特性，所以这些方法很少使用`

##### 其他方面的变化 
`DocumentType `:`DocumentType 类型新增了 3个属性：publicId、systemId 和 internalSubset。前两 个属性表示的是文档类型声明中的两个信息段，这两个信息段在 DOM1 级中是没有办法访问到的。`

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"     "http://www.w3.org/TR/html4/strict.dtd">
```
`对这个文档类型声明而言，publicId是"-//W3C//DTD HTML 4.01//EN"，而systemId是"http: //www.w3.org/TR/html4/strict.dtd"。在支持 DOM2级的浏览器中，应该可以运行下列代码。` 

```node
alert(document.doctype.publicId); 
alert(document.doctype.systemId); 
```
`Document `:`Document 类型的变化中唯一与命名空间无关的方法是 importNode()。这个方法的用途是从一个 文档中取得一个节点，然后将其导入到另一个文档，使其成为这个文档结构的一部分。`

`每个节点都有一个 ownerDocument 属性，表示所属的文档`

`说起来，importNode()方法与 Element 的 cloneNode()方法非常相似，它接受两个参数：要复制的节点和一个表示是否复制子节点的布尔值。返回的结果是原来节点的副本，但能够在当前文档中使 用。来看下面的例子：`

```node
var newNode = document.importNode(oldNode, true); //导入节点及其所有子节点
document.body.appendChild(newNode); 
```

#####  :octocat: [2.样式](#top) <b id="target2"></b> 
`在 HTML中定义样式的方式有 3种：通过<link/>元素包含外部样式表文件、使用<style/>元素 定义嵌入式样式，以及使用 style 特性定义针对特定元素的样式`

```node
var supportsDOM2CSS = document.implementation.hasFeature("CSS", "2.0"); 
var supportsDOM2CSS2 = document.implementation.hasFeature("CSS2", "2.0");
```
##### 访问元素的样式 
`任何支持 style 特性的 HTML元素在 JavaScript中都有一个对应的 style 属性。这个 style 对象 是 CSSStyleDeclaration 的实例，包含着通过 HTML的 style 特性指定的所有样式信息，但不包含 与外部样式表或嵌入样式表经层叠而来的样式`

```node
var myDiv = document.getElementById("myDiv"); 
 
//设置背景颜色 
myDiv.style.backgroundColor = "red"; 
 
//改变大小 myDiv.style.width = "100px";
myDiv.style.height = "200px"; 

//指定边框 
myDiv.style.border = "1px solid black";
```
`在以这种方式改变样式时，元素的外观会自动被更新`

* `DOM样式属性和方法`:`“DOM2级样式”规范还为style对象定义了一些属性和方法。这些属性和方法在提供元素的style 特性值的同时，也可以修改样式。`
  * `cssText`：`如前所述，通过它能够访问到 style 特性中的 CSS代码。` 
  * `length`：`应用给元素的 CSS属性的数量。` 
  * `parentRule`：`表示 CSS信息的 CSSRule 对象。。` 
  * `getPropertyCSSValue(propertyName)`：`返回包含给定属性值的 CSSValue 对象。` 
  * `getPropertyPriority(propertyName)`：`如果给定的属性使用了!important 设置，则返回 "important"；否则，返回空字符串。 `
  * `getPropertyValue(propertyName)`：`返回给定属性的字符串值。` 
  * `item(index)`：`返回给定位置的 CSS属性的名称。` 
  * `removeProperty(propertyName)`：`从样式中删除给定属性。` 
  * `setProperty(propertyName,value,priority)`：`将给定属性设置为相应的值，并加上优先 权标志（"important"或者一个空字符串）。`

`通过cssText属性可以访问style特性中的CSS代码。在读取模式下，cssText返回浏览器对style 特性中 CSS代码的内部表示。`

 ```node
 myDiv.style.cssText = "width: 25px; height: 100px; background-color: green"; 
 alert(myDiv.style.cssText); 
 ```
 
