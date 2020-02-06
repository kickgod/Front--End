### [JavaScript DOM 扩展 ](#top) :grey_exclamation: <b id="top"></b>
`DOM1 级主要定义的是 HTML和 XML文档的底层结构。DOM2 和 DOM3级则在这个结构 的基础上引入了更多的交互能力 `

------

- [x] [`1.DOM2/3简介和XML命名空间变化`](#target1)
- [x] [`2.样式`](#target2)
- [x] [`3.操作样式表`](#target3)
- [x] [`4.元素大小`](#target4)
- [x] [`5.遍历`](#target5)
- [x] [`6.范围`](#target6)

 
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

`要想知道某个元素在页面上的偏移量，将这个元素的 offsetLeft 和 offsetTop 与其 offsetParent 的相同属性相加，如此循环直至根元素，就可以得到一个基本准确的值。以下两个函数就可以用于分别 取得元素的左和上偏移量。 `

```node
function getElementLeft(element) {
    var actualLeft = element.offsetLeft;
    var current = element.offsetParent;

    while (current !== null) {
        actualLeft += current.offsetLeft;
        current = current.offsetParent;
    }

    return actualLeft;
}
```

```node
function getElementTop(element) {
    var actualTop = element.offsetTop;
    var current = element.offsetParent;

    while (current !== null) {
        actualTop += current.offsetTop;
        current = current.offsetParent;
    }

    return actualTop;
} 
```
`这两个函数利用 offsetParent 属性在 DOM层次中逐级向上回溯，将每个层次中的偏移量属性 合计到一块。对于简单的 CSS布局的页面，这两函数可以得到非常精确的结果。对于使用表格和内嵌 框架布局的页面，由于不同浏览器实现这些元素的方式不同，因此得到的值就不太精确了。`

##### 2.客户区大小 
`元素的客户区大小（client dimension），指的是元素内容及其内边距所占据的空间大小。有关客户区 大小的属性有两个：clientWidth 和 clientHeight。其中，clientWidth 属性是元素内容区宽度加 上左右内边距宽度 [内容 + padding]；clientHeight 属性是元素内容区高度加上上下内边距高度。图 12-2 形象地说明 了这些属性表示的大小。 `

`从字面上看，客户区大小就是元素内部的空间大小，因此滚动条占用的空间不计算在内。常用到 这些属性的情况，就是像第 8章讨论的确定浏览器视口大小的时候。如下面的例子所示，要确定浏览器 视口大小，可以使用 document.documentElement 或 document.body（在 IE7 之前的版本中）的 clientWidth 和 clientHeight。 `

```node
function getViewport() {
    if (document.compatMode == "BackCompat") {
        return {width: document.body.clientWidth, height: document.body.clientHeight};
    } else {
        return {width: document.documentElement.clientWidth, height: document.documentElement.clientHeight};
    }
} 
```

##### 3.滚动大小 
`后要介绍的是滚动大小（scroll dimension），指的是包含滚动内容的元素的大小。有些元素（例如 <html>元素），即使没有执行任何代码也能自动地添加滚动条；但另外一些元素，则需要通过 CSS 的 overflow 属性进行设置才能滚动。以下是 4个与滚动大小相关的属性。 `

* `scrollHeight`：`在没有滚动条的情况下，元素内容的总高度。` 
* `scrollWidth`：`在没有滚动条的情况下，元素内容的总宽度。` 
* `scrollLeft`：`被隐藏在内容区域左侧的像素数。通过设置这个属性可以改变元素的滚动位置。`
* `scrollTop`：`被隐藏在内容区域上方的像素数。通过设置这个属性可以改变元素的滚动位置。 `

`scrollWidth 和 scrollHeight 主要用于确定元素内容的实际大小。`

`对于不包含滚动条的页面而言，scrollWidth 和 scrollHeight 与 clientWidth 和 clientHeight 之间的关系并不十分清晰。在这种情况下，基于 document.documentElement 查看 这些属性会在不同浏览器间发现一些不一致性问题`

`IE（在标准模式）中的这两组属性不相等，其中 scrollWidth 和 scrollHeight 等于文档内 容区域的大小，而 clientWidth 和 clientHeight 等于视口大小`

`在确定文档的总高度时（包括基于视口的小高度时），必须取得 scrollWidth/clientWidth 和 scrollHeight/clientHeight 中的大值，才能保证在跨浏览器的环境下得到精确的结果。下面就 是这样一个例子。 `

```node
var docHeight = Math.max(document.documentElement.scrollHeight, document.documentElement.clientHeight);

var docWidth = Math.max(document.documentElement.scrollWidth, document.documentElement.clientWidth);
```

`注意，对于运行在混杂模式下的 IE，则需要用 document.body 代替 document.document- Element。 `

##### 4. 确定元素大小 
`IE、Firefox 3+、Safari 4+、Opera 9.5及Chrome为每个元素都提供了一个 getBoundingClientRect()方 法。这个方法返回会一个矩形对象，包含 4个属性：left、top、right 和 bottom。这些属性给出了元素在页面中相对于视口的位置。但是，浏览器的实现稍有不同。IE8 及更早版本认为文档的左上角坐 标是(2, 2)，而其他浏览器包括 IE9则将传统的(0,0)作为起点坐标。因此，就需要在一开始检查一下位于 (0,0)处的元素的位置，在 IE8及更早版本中，会返回(2,2)，而在其他浏览器中会返回(0,0)。来看下面的 函数： `

```node
function getBoundingClientRect(element) {
    if (typeof arguments.callee.offset != "number") {
        var scrollTop = document.documentElement.scrollTop;
        var temp = document.createElement("div");
        temp.style.cssText = "position:absolute;left:0;top:0;";
        document.body.appendChild(temp);
        arguments.callee.offset = -temp.getBoundingClientRect().top - scrollTop;
        document.body.removeChild(temp);
        temp = null;
    }

    var rect = element.getBoundingClientRect();
    var offset = arguments.callee.offset;

    return {
      left: rect.left + offset,
      right: rect.right + offset,
      top: rect.top + offset, 
      bottom: rect.bottom + offset
    };
}
```
`对于不支持 getBoundingClientRect()的浏览器，可以通过其他手段取得相同的信息。一般来 说，right 和 left 的差值与 offsetWidth 的值相等，而 bottom 和 top 的差值与 offsetHeight 相等。而且，left 和 top 属性大致等于使用本章前面定义的 getElementLeft()和 getElementTop() 函数取得的值。综合上述，就可以创建出下面这个跨浏览器的函数： `

```node
function getBoundingClientRect(element) {

    var scrollTop = document.documentElement.scrollTop;
    var scrollLeft = document.documentElement.scrollLeft;
    if (element.getBoundingClientRect) {
        if (typeof arguments.callee.offset != "number") {
            var temp = document.createElement("div");
            temp.style.cssText = "position:absolute;left:0;top:0;";
            document.body.appendChild(temp);
            arguments.callee.offset = -temp.getBoundingClientRect().top - scrollTop;
            document.body.removeChild(temp);
            temp = null;
        }
        var rect = element.getBoundingClientRect();
        var offset = arguments.callee.offset;

        return {
            left: rect.left + offset, 
            right: rect.right + offset, 
            top: rect.top + offset, 
            bottom: rect.bottom + offset

        };
    } else {

        var actualLeft = getElementLeft(element);
        var actualTop = getElementTop(element);

        return {
            left: actualLeft - scrollLeft,
            right: actualLeft + element.offsetWidth - scrollLeft,
            top: actualTop - scrollTop,
            bottom: actualTop + element.offsetHeight - scrollTop
        }
    }
}

var iterator = document.createNodeIterator(root, NodeFilter.SHOW_ELEMENT,filter, false); 
```

##### :octocat: [5.遍历](#top) <b id="target5"></b>  
`“DOM2级遍历和范围”模块定义了两个用于辅助完成顺序遍历 DOM结构的类型：NodeIterator 和 TreeWalker。这两个类型能够基于给定的起点对 DOM结构执行深度优先（depth-first）的遍历操作。 `

```node
var supportsTraversals = document.implementation.hasFeature("Traversal", "2.0"); 
var supportsNodeIterator = (typeof document.createNodeIterator == "function"); 
var supportsTreeWalker = (typeof document.createTreeWalker == "function"); 
```

##### NodeIterator 
`对Dom树 进行遍历的对象` [`官方文档`](https://developer.mozilla.org/zh-CN/docs/Web/API/NodeIterator)

* `root`：`想要作为搜索起点的树中的节点。` 
* `whatToShow`：`表示要访问哪些节点的数字代码。` 
* `filter`：`是一个 NodeFilter 对象，或者一个表示应该接受还是拒绝某种特定节点的函数。` 
* `entityReferenceExpansion`：`布尔值，表示是否要扩展实体引用。这个参数在 HTML 页面 中没有用，因为其中的实体引用不能扩展。`
* `whatToShow`: `参数是一个位掩码，通过应用一或多个过滤器（filter）来确定要访问哪些节点。这个 参数的值以常量形式在` [`NodeFilter`]() `类型中定义，返回一个无符号长整型，它是一个由描述必须呈现的Node类型的常量构成的位掩码。不匹配`

`可以通过 createNodeIterator()方法的 filter 参数来指定自定义的 NodeFilter 对象，或者 指定一个功能类似节点过滤器（node filter）的函数.`

```node
var filter = {
    acceptNode: function (node) {
        return node.tagName.toLowerCase() == "p" ? NodeFilter.FILTER_ACCEPT : NodeFilter.FILTER_SKIP;
    }
}; 

```

```html
<div id="div1"><p><b>Hello</b> world!</p>
    <ul>
        <li>List item 1</li>
        <li>List item 2</li>
        <li>List item 3</li>
    </ul>
</div>
```
`假设我们想要遍历<div>元素中的所有元素，那么可以使用下列代码。 `

```node
let div = document.getElementById("div1");
let iterator = document.createNodeIterator(div, NodeFilter.SHOW_ELEMENT, null, false);
let node = iterator.nextNode();
while (node !== null) {
    alert(node.tagName);        //输出标签名
    node = iterator.nextNode();
}//DIV P B UL LI LI LI
```
`也许用不着显示那么多信息，你只想返回遍历中遇到的<li>元素。很简单，只要使用一个过滤器 即可，如下面的例子所示。 `

```node
var div = document.getElementById("div1");
var filter = function (node) {
    return node.tagName.toLowerCase() == "li" ? NodeFilter.FILTER_ACCEPT : NodeFilter.FILTER_SKIP;
};

var iterator = document.createNodeIterator(div, NodeFilter.SHOW_ELEMENT, filter, false);

var node = iterator.nextNode();
while (node !== null) {
    alert(node.tagName);        //输出标签名     
    node = iterator.nextNode(); 
} 
```
`在这个例子中，迭代器只会返回<li>元素。 `

##### TreeWalker 
`TreeWalker 是 NodeIterator 的一个更高级的版本。除了包括 nextNode()和 previousNode() 在内的相同的功能之外，这个类型还提供了下列用于在不同方向上遍历 DOM结构的方法。 `

* `parentNode()`：`遍历到当前节点的父节点；` 
* `firstChild()`：`遍历到当前节点的第一个子节点；` 
* `lastChild()`：`遍历到当前节点的后一个子节点；` 
* `nextSibling()`：`遍历到当前节点的下一个同辈节点；` 
* `previousSibling()`：`遍历到当前节点的上一个同辈节点。 `

`创建 TreeWalker 对象要使用 document.createTreeWalker()方法，这个方法接受的 4个参数 `:`：作为遍历起点的根节点、要显示的节点类型、过滤 器和一个表示是否扩展实体引用的布尔值。` [`官方文档`](https://developer.mozilla.org/zh-CN/docs/Web/API/TreeWalker)


```node
var div = document.getElementById("div1");
var walker = document.createTreeWalker(div, NodeFilter.SHOW_ELEMENT, null, false);

walker.firstChild();            //转到<p> walker.nextSibling();         // 转到<ul> 

var node = walker.firstChild();        //转到第一个<li> 
 while (node !== null) {    
     alert(node.tagName);     
     node = walker.nextSibling();
 } 
```
##### :octocat: [6.范围](#top) <b id="target6"></b>  
`为了让开发人员更方便地控制页面，“DOM2级遍历和范围”模块定义了“范围”（range）接口。通 过范围可以选择文档中的一个区域，而不必考虑节点的界限（选择在后台完成，对用户是不可见的）。`

##### DOM中的范围 
`DOM2级在 Document 类型中定义了 createRange()方法。在兼容 DOM的浏览器中，这个方法 属于 document 对象。使用 hasFeature()或者直接检测该方法，都可以确定浏览器是否支持范围。 `

```node
var supportsRange = document.implementation.hasFeature("Range", "2.0");
var alsoSupportsRange = (typeof document.createRange == "function"); 
```

```node
var range = document.createRange();
```
`每个范围由一个 Range 类型的实例表示，这个实例拥有很多属性和方法。下列属性提供了当前范 围在文档中的位置信息。`


* `startContainer`：`包含范围起点的节点（即选区中第一个节点的父节点）。` 
* `startOffset`：`范围在 startContainer 中起点的偏移量。如果 startContainer 是文本节 点、注释节点或 CDATA节点，那么 startOffset 就是范围起点之前跳过的字符数量。否则， startOffset 就是范围中第一个子节点的索引。` 
* `endContainer`：`包含范围终点的节点（即选区中后一个节点的父节点）。` 
* `endOffset`：`范围在 endContainer 中终点的偏移量（与 startOffset 遵循相同的取值规则）。`
* `commonAncestorContainer`：`startContainer 和 endContainer 共同的祖先节点在文档树 中位置深的那个。 `

##### 6.1 用 DOM范围实现简单选择
`要使用范围来选择文档中的一部分，简的方式就是使用 selectNode()或 selectNodeContents()。 这两个方法都接受一个参数，即一个 DOM 节点，然后使用该节点中的信息来填充范围`

`selectNode()方法选择整个节点，包括其子节点；而 selectNodeContents()方法则只选择节点的 子节点。`

```html
<!DOCTYPE html>
<html>
<body><p id="p1"><b>Hello</b> world!</p></body>
</html> 
```
`我们可以使用下列代码来创建范围： `

```node
var range1 = document.createRange();     
    range2 = document.createRange();    
    p1 = document.getElementById("p1"); 
    range1.selectNode(p1); 
    range2.selectNodeContents(p1); 
```
`这里创建的两个范围包含文档中不同的部分：rang1 包含<p/>元素及其所有子元素，而 rang2 包 含<b/>元素、文本节点"Hello"和文本节点"world!"`






