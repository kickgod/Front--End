### [JavaScript DOM 扩展 ](#top) :grey_exclamation: <b id="top"></b>
`DOM1 级主要定义的是 HTML和 XML文档的底层结构。DOM2 和 DOM3级则在这个结构 的基础上引入了更多的交互能力 `

------
- [x] [`DOM2/3简介和XML命名空间变化`](#target1)
- [x] [`样式`](#target2)
- [x] [`操作样式表`](#target3)
- [x] [`元素大小`](#target4)
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
 
`计算的样式`:`虽然 style 对象能够提供支持 style 特性的任何元素的样式信息，但它不包含那些从其他样式表 层叠而来并影响到当前元素的样式信息。“DOM2 级样式”增强了 document.defaultView，提供了 getComputedStyle()方法。这个方法接受两个参数：要取得计算样式的元素和一个伪元素字符串（例 如":after"）如果不需要伪元素信息，第二个参数可以是 null。getComputedStyle()方法返回一 个 CSSStyleDeclaration 对象（与 style 属性的类型相同），其中包含当前元素的所有计算的样式。`

```html
<!DOCTYPE html>
<html>
<head><title>Computed Styles Example</title>
    <style type="text/css">
        #myDiv {
            background-color: blue;
            width: 100px;
            height: 200px;
        }
    </style>
</head>
<body>
    <div id="myDiv" style="background-color: red; border: 1px solid black"></div>
</body>
</html>
```

`应用给这个例子中<div>元素的样式一方面来自嵌入式样式表（<style>元素中的样式），另一方 面来自其 style 特性。但是，style 特性中设置了 backgroundColor 和 border，没有设置 width 和 height，后者是通过样式表规则应用的。以下代码可以取得这个元素计算后的样式。 `

```node
var myDiv = document.getElementById("myDiv");
var computedStyle = document.defaultView.getComputedStyle(myDiv, null);

alert(computedStyle.backgroundColor);  // "red" 
alert(computedStyle.width);    // "100px"
alert(computedStyle.height);    // "200px"
alert(computedStyle.border);    // 在某些浏览器中是"1px solid black"
```

`IE不支持 getComputedStyle()方法，但它有一种类似的概念。在 IE中，每个具有 style 属性 的元素还有一个 currentStyle 属性。这个属性是 CSSStyleDeclaration 的实例，包含当前元素全 部计算后的样式。所以兼容IE`

```node
var computeStyle = document.defaultView.getComputedStyle(myDiv, null) ||  myDiv.currentStyle; 
```

#####  :octocat: [3.操作样式表](#top) <b id="target3"></b> 
`CSSStyleSheet 类型表示的是样式表，包括通过<link>元素包含的样式表和在<style>元素中定义 的样式表。这两个元素本身分别是由 HTMLLinkElement 和 HTMLStyleElement 类型 表示的。但是，CSSStyleSheet 类型相对更加通用一些，它只表示样式表，而不管这些样式表在 HTML 中是如何定义的上述两个针对元素的类型允许修改 HTML特性，但 CSSStyleSheet 对象则是一 套只读的接口（有一个属性例外）。使用下面的代码可以确定浏览器是否支持 DOM2级样式表。 `

```javascript
var supportsDOM2StyleSheets =  document.implementation.hasFeature("StyleSheets", "2.0"); 
```
`CSSStyleSheet 继承自 StyleSheet，后者可以作为一个基础接口来定义非 CSS 样式表。从 StyleSheet 接口继承而来的属性如下`

* ` disabled`:`表示样式表是否被禁用的布尔值。这个属性是可读/写的，将这个值设置为 true 可 以禁用样式表。 `
* ` href`：`如果样式表是通过<link>包含的，则是样式表的 URL；否则，是 null。 `
* `media`：`当前样式表支持的所有媒体类型的集合。与所有 DOM 集合一样，这个集合也有一个 length 属性和一个 item()方法。也可以使用方括号语法取得集合中特定的项。如果集合是空 列表，表示样式表适用于所有媒体。在 IE中，media 是一个反映<link>和<style>元素 media 特性值的字符串。 `
* `ownerNode`：`指向拥有当前样式表的节点的指针，样式表可能是在 HTML 中通过<link>或 <style/>引入的（在 XML中可能是通过处理指令引入的）。如果当前样式表是其他样式表通过 @import 导入的，则这个属性值为 null。IE不支持这个属性。 `
* `parentStyleSheet`：`在当前样式表是通过@import 导入的情况下，这个属性是一个指向导入 它的样式表的指针。 `
* `title`：`ownerNode 中 title 属性的值。 `
* `type`：`表示样式表类型的字符串。对 CSS样式表而言，这个字符串是"type/css"。 除了 disabled 属性之外，其他属性都是只读的。`

`在支持以上所有这些属性的基础上， CSSStyleSheet 类型还支持下列属性和方法： `

* `cssRules`：`样式表中包含的样式规则的集合。IE不支持这个属性，但有一个类似的 rules 属性。`
* `ownerRule`：`如果样式表是通过@import 导入的，这个属性就是一个指针，指向表示导入的规 则；否则，值为 null。IE不支持这个属性。 `
* `deleteRule(index)`：`删除 cssRules 集合中指定位置的规则。IE 不支持这个方法，但支持 一个类似的 removeRule()方法。`
* `insertRule(rule,index)`：`向 cssRules 集合中指定的位置插入 rule 字符串。IE不支持这 个方法，但支持一个类似的 addRule()方法。 `

`应用于文档的所有样式表是通过 document.styleSheets 集合来表示的. 通过这个集合的 length 属性可以获知文档中样式表的数量,而通过方括号语法或 item()方法可以访问每一个样式表`

```node
var sheet = null; 

for (var i=0, len=document.styleSheets.length; i < len; i++){
    sheet = document.styleSheets[i];
    alert(sheet.href);
}
```
`以上代码可以输出文档中使用的每一个样式表的 href 属性（<style>元素包含的样式表没有 href 属性） `

```node
function getStyleSheet(element){     
  return element.sheet || element.styleSheet; 
}
//这里的 getStyleSheet()返回的样式表对象与 document.styleSheets 集合中的样式表对象相同
```

##### 1. CSS规则 
`CSSRule 对象表示样式表中的每一条规则。实际上，CSSRule 是一个供其他多种类型继承的基类 型，其中常见的就是 CSSStyleRule 类型，表示样式信息（其他规则还有@import、@font-face、 @page 和@charset，但这些规则很少有必要通过脚本来访问）`

* `cssText`：`返回整条规则对应的文本。由于浏览器对样式表的内部处理方式不同，返回的文本 可能会与样式表中实际的文本不一样；Safari 始终都会将文本转换成全部小写。IE 不支持这个 属性。 `
* `parentRule`：`如果当前规则是导入的规则，这个属性引用的就是导入规则；否则，这个值为 null。IE不支持这个属性。`
* `parentStyleSheet`：`当前规则所属的样式表。IE不支持这个属性。 `
* `selectorText`：`返回当前规则的选择符文本。`
* `style`：`一个 CSSStyleDeclaration 对象，可以通过它设置和取得规则中特定的样式值。 `
* `type`：`表示规则类型的常量值。对于样式规则，这个值是 1。IE不支持这个属性。 `

```html
<style>
div.box {    
 background-color: blue;
 width: 100px;     
 height: 200px; 
} 
</style>

<script>
 var sheet = document.styleSheets[0]; 
 var rules = sheet.cssRules || sheet.rules;   //取得规则列表 
 var rule = rules[0];                      //取得第一条规则 
 alert(rule.selectorText);                  //"div.box" 
 alert(rule.style.cssText);                  //完整的 CSS 代码 
 alert(rule.style.backgroundColor);          //"blue" 
 alert(rule.style.width);                  //"100px" 
 alert(rule.style.height);                  //"200px" 
</script>
```

##### 2.创建规则 
`DOM 规定，要向现有样式表中添加新规则，需要使用 insertRule()方法。这个方法接受两个参 数：规则文本和表示在哪里插入规则的索引`

```node
sheet.insertRule("body { background-color: silver }", 0); //DOM方法 
```

`IE8 及更早版本支持一个类似的方法，名叫 addRule()，也接收两必选参数：选择符文本和 CSS 样式信息；一个可选参数：插入规则的位置。在 IE中插入与前面例子相同的规则，可使用如下代码`

```node
sheet.addRule("body", "background-color: silver", 0); //仅对 IE有效 
// 有关这个方法的规定中说，多可以使用 addRule()添加 4 095条样式规则。超出这个上限的调用 将会导致错误。 
```

`要以跨浏览器的方式向样式表中插入规则，可以使用下面的函数。`

```node
function insertRule(sheet, selectorText, cssText, position) {
    if (sheet.insertRule) {
        sheet.insertRule(selectorText + "{" + cssText + "}", position);
    } else if (sheet.addRule) {
        sheet.addRule(selectorText, cssText, position);
    }
} 

insertRule(document.styleSheets[0], "body", "background-color: silver", 0); 
```

##### 3. 删除规则 
```node
sheet.deleteRule(0);    //DOM方法 

sheet.removeRule(0);    //仅对 IE有效 

//跨浏览器
function deleteRule(sheet, index) {
    if (sheet.deleteRule) {
        sheet.deleteRule(index);
    } else if (sheet.removeRule) {
        sheet.removeRule(index);
    }
} 
```
#####  :octocat: [4.元素大小](#top) <b id="target4"></b> 
`本节介绍的属性和方法并不属于“DOM2级样式”规范，但却与 HTML元素的样式息息相关。`

##### 1.偏移量 
* `offsetHeight：元素在垂直方向上占用的空间大小，以像素计。包括元素的高度、（可见的） 水平滚动条的高度、上边框高度和下边框高度。[包含边框厚度] `
* `offsetWidth：元素在水平方向上占用的空间大小，以像素计。包括元素的宽度、（可见的）垂 直滚动条的宽度、左边框宽度和右边框宽度。 [包含边框厚度]`
* `offsetLeft：元素的左外边框至包含元素的左内边框之间的像素距离。 `
* `offsetTop：元素的上外边框至包含元素的上内边框之间的像素距离。`

`offsetLeft 和 offsetTop 属性与包含元素有关，包含元素的引用保存在 offsetParent 属性中。offsetParent 属性不一定与 parentNode 的值相等。`
