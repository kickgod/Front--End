### [JavaScript Dom1 文档对象模型](#top) :grey_exclamation: <b id="top"></b>
`文档对象模型（Document Object Model，简称DOM），是W3C组织推荐的处理可扩展标志语言的标准编程接口。在网页上，组织页面（或文档）的
对象被组织在一个树形结构中，用来表示文档中对象的标准模型就称为DOM。[总之DOM的作用就是让程序员可以随心所欲的操作WEB 页面]` :speech_balloon:

------

- [x] [`1.节点层次`](#target1)
- [x] [`2.Node 类型`](#target2)
- [x] [`3.节点关系`](#target3)
- [x] [`4.节点操作`](#target4)
- [x] [`5.Document 类型`](#target5)
  * [`5.1 Document 查询API`](#target6)
  * [`5.2 Document 一致性检测`](#target7)
- [x] [`6.页面加载顺序`](#target8)
- [x] [`7.Element`](#target9)
- [x] [`8.Text`](#target10)
- [x] [`9.Comment`](#target11)
- [x] [`10.CDATASection`](#target12)
- [x] [`11.DocumentType`](#target13)  
- [x] [`12.DocumentFragment`](#target14)
- [x] [`13.Attr`](#target15)
- [x] [`14.DOM操作技术 `](#target16)

------

#####  :octocat: [1.节点层次](#top) <b id="target1"></b> 
`概括`:`节点是层次关系的 一层嵌套一层 组成了 Dom的结构 本质上 你可以把他看成一棵树,节点之间的关系构成了层次，而所有页面标记则表现为一个以特定节点 为根节点的树形结构 `
```html
<!DOCTYPE html>
<html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>首页</title>
  </head>
  <body>
     <div>Hello World!</div>
  </body>
</html>
```
`Document 是文档对象 最高的根节点 Dom 结构就是数据结构中 树`
```c#
Document
    |_ _ Element Html
          |_ _ Element head
          |     |
          |     |_ _ meta 
          |     |     |_ _ Attribute "charset UTF-8"
          |     |
          |     |_ _ title 
          |          |
          |          |_ _ Text 首页
          |
          |— — Element body
               |_ _ div
                    |_ _ Text Hello World!
```
`文档元素是文档的外层元素，文档中的其他所有元素都包含在文档元素中。`

##### DOM
* `D`: `文档 表示整个HTML网页`
* `O`: `对象 表示将网页里面的每一个部分都转化为对象`
* `M`: `模型 实现对象之间的关系, 用户完成对对象的管理 即DOM树`

#####  :octocat: [2.Node 类型](#top) <b id="target2"></b> 
`Dom1 级定义了一个Node 接口,该接口将由Dom 中所有的节点类型实现 每一个节点都有一个 nodeType 表名节点的类型,任何节点类型必然是这十二个节点之一`

* `Node.ELEMENT_NODE(1)` 
* `Node.ATTRIBUTE_NODE(2)` 
* `Node.TEXT_NODE(3)` 
* `Node.CDATA_SECTION_NODE(4)` 
* `Node.ENTITY_REFERENCE_NODE(5)` 
* `Node.ENTITY_NODE(6)` 
* `Node.PROCESSING_INSTRUCTION_NODE(7)`
* `Node.COMMENT_NODE(8)`
* `Node.DOCUMENT_NODE(9)` 
* `Node.DOCUMENT_TYPE_NODE(10)` 
* `Node.DOCUMENT_FRAGMENT_NODE(11)` 
* `Node.NOTATION_NODE(12)`

|`值`|`节点类型`|`nodeName` |`描述`|`子节点`|
|:---|:----|:----|:----|:----|
|`1`|`Element`|`标签名`|`元素节点例如 p div`|`Element, Text, Comment, ProcessingInstruction, CDATASection, EntityReference`|
|`2`|`Attr`|`属性名`|`代表属性`|`Text, EntityReference`|
|`3`|`Text`|`#text`|`代表元素或属性中的文本内容`|`None`|
|`4`|`CDATASection`|` `|`代表文档中的 CDATA 部分（不会由解析器解析的文本）。`|`None`|
|`5`|`EntityReference`|` `|`代表实体引用`|`Element, ProcessingInstruction, Comment, Text, CDATASection, EntityReference`|
|`6`|`Entity`|` `|`代表实体`|`Element, ProcessingInstruction, Comment, Text, CDATASection, EntityReference`|
|`7`|`ProcessingInstruction`|` `|`代表处理指令`|`None`|
|`8`|`Comment`|` `|`代表注释`|`None`|
|`9`|`Document`|`#document`|`代表整个文档（DOM 树的根节点）。`|`Element, ProcessingInstruction, Comment, DocumentType`|
|`10`|`DocumentType`|` `|`向为文档定义的实体提供接口`|`None`|
|`11`|`DocumentFragment`|` `|`代表轻量级的 Document 对象，能够容纳文档的某个部分`|`Element, ProcessingInstruction, Comment, Text, CDATASection, EntityReference`|
|`12`|`Notation`|` `|`代表 DTD 中声明的符号。`|`None`|

* `nodeName`:`Node.nodeName 表名节点名称 例如 body div p `
* `nodeType`:`返回数字 节点类型的值 它属于 Node 类型枚举之一  `
* `nodeValue`:`这个属性的值始终都是 null 如果是属性节点那么 nodeValue就是属性值 如果是文本节点那么 就是文本内容`

```node
<body id="main">

</body>
var node = document.getElementById("main");
console.log(node.nodeName); // BODY
console.log(node.nodeType); // 1

let isElem = (node.nodeType == Node.ELEMENT_NODE) ? "是Element 类型 节点":"不是Element 类型节点";
console.log(isElem); //是Element 类型 节点

console.log(node.nodeValue); //null
```

#####  :octocat: [3.节点关系](#top) <b id="target3"></b> 
`节点具有如下几种关系 关系都是指针  也成为关系指针 他们是只读的`
* `父子节点`:`元素内部的空格换行也被当做了子节点 非常`
   * `childNodes`:`获得一个节点的所有子节点的 返回  NodeList 类型 它可以使用索引访问  但是他不是数组,但是我们可以将他们转换为数组 返回的
   NodeList 对象是访问时 对于Dom 结构的一次快照 也就是说 他不是动态的  每次获取它  他都会对Dom 结构进行一次[拍摄] 获取 Dom结构快照`
      * `NodeList对象的属性 length`:`节点个数`
      * `forEach`:`遍历方法`
      * `item`:`索引器`
      * `keys`:`每一个Node 的键 也就是 nodeName`
   * `parentNode`:`每一个节点都具有的指向父节点的指针属性`
   * `firstChild`:`第一个节点`
   * `lastChild`:`最后一个节点`
   
* `兄弟节点`
   * `previousSibling`:`当前节点的前一个节点 如果html 节点之前具有字符/空格 那么 值会 返回 #text 没有返回 null`
   * `nextSibling`:`当前节点的后一个节点 如果html 节点之后具有字符/空格 那么 值会 返回 #text 没有返回 null`
* `节点判断`
   * `hasChildNodes`:`判断当前节点是否具有子节点`
   * `ownerDocument`:`该属性指向表示整个文档的文档节点,每一个节点都具有`
`跨浏览器将nodeList 转换为数组`
```node
function ConvertNodeListToArray(_nodeList){
    var array = null;
    try {
        array = Array.prototype.slice.call(_nodeList,0);
    } catch (error) {
        array = new Array();
        for(var i = 0,len = _nodeList.length; i < len; i++ ){
            array.push(_nodeList.item(i));
        }
    }
    return array;
}
```

```node
var body = document.getElementsByTagName("body")[0]; //获得body
console.log(body);
console.log(body.childNodes);

console.log(body.firstChild); //第一个节点
console.log(body.lastChild); //最后一个节点
```

##### 解决换行 空格问题
`很多的时候返回的 子节点中或者 兄弟节点会是 #text 类型也不是我们需要的 Element 类型 这非常的糟糕 新版JS 提供了 只返回Element 类型的属性指针`
* `firstElementChild`:`第一个 Element 类型的元素`
* `lastElementChild`:`最后一个 Element 类型的元素`
* `ParentNode.childElementCount`:`返回 Elment 类型元素的个数`

* `previousElementSibling`:`前一个 Element类型的 兄弟节点`
* `nextElementSibling`:`后一个 Element类型的 兄弟节点`

`找到下一个 Element类型的节点`
```node
function next(node) {
    node = node.nextSibling;
    while (node != null && node.nodeName == "#text") {
        node = node.nextSibling;
    }
    return node;
}
```

#####  :octocat: [4.节点操作](#top) <b id="target4"></b> 
`操作一般分为四种 增删改查`
* `父子节点的操作`
   * `Container.appendChild(node)`:`增加子节点 在 childNodes列表的末尾添加一个节点 添加成功后返回一个新增的节点`
   * `Container.insertBefore(node,nodeplace)`:`nodeplace 是第一个插入位置的参考值,新加入的元素会被插入到  Container 
   元素中的 nodeplace节点之前`
   
   * `Container.replaceChild(newNode,replaceNode):`新节点 替换 Container 节点中的 replace子节点`
   * `Container.removeChild(needMoveNode)`:`删除匹配到的子节点`
   * `Node.cloneNode`:`拷贝一个完全相同的副本`
   * `normalise`:`处理文档树的文本节点,删除空文本节点,合并相邻的文本节点`


#####  :octocat: [5.Document 类型:HTMLDocument](#top) <b id="target5"></b> 
`Js 通过Document类型表示文档,在浏览器中,document对象是 HTMLDocument 继承自 Document 类型的一个实例,表示整个HTML页面,而且document对象是
window对象的一个属性,因此可以将其作为全局对象来访问,Document节点具有下列特征`
* `nodeType`:`9`
* `nodeName`:`#document`
* `nodeValue`:`null`
* `parentNode`:`null`
* `ownerDocument`:`null`
* `它的子节点可能是一个 DocumentType[最多一个] Element[最多一个] ProcessingInstruction或 Comment`


`HTMLDocument 的实例是 document对象`

##### 属性
* `documentElement`:`该属性始终指向HTML 页面中的 html元素,另一个就是通过childNodes 列表访问文档元素,但通过documentElement更快捷`
* `body`:`指向 document.body`
* `doctype`:`指向  <!DOCTYPE html> IE8 之前一直为null, 少用它 而且他也没有什么用`
* `title`:`title 标签的文本内容`
* `URL`:`地址栏中显示的URL`
* `domain`:`包含页面的域名`
* `referrer`:`保存着链接到当前页面的那个页面的URL 无来源则为 空字符串`
```node
有时候 document.firstChild 不是  html 也是<!DOCTYPE html>  切记切记
```

#####  :octocat: [5.1 Document 查询API](#top) <b id="target8"></b> 
* `document.getElementById(id)`:`通过元素ID 获得 元素 返回 HTMLElement 类型`

* `document.getElementsByClassName(name)`:`通过类名获取 元素 返回类型为`  [`HTMLCollection`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection)
* `document.getElementsByName(name)`:`通过属性 name获得元素 返回类型为 HTMLCollection`
* `document.getElementsByTagName(name)`:`通过标签名称获得元素 返回类型为 HTMLCollection`
* `getElementsByTagNameNS(ns,name)`:`在某个域中获取元素 返回类型为 HTMLCollection`

##### 特殊集合 查询API
* `document.anchors`:`包含文档中所有带name的 <a>元素`

* `document.forms`:`文档中所有的 form 表单` = `document.getElementsByTagName("from")`
* `document.images`:`文档中所有的 img 元素` = `document.getElementsByTagName("img")`
* `document.links`:`返回文档中所有包含 href 特性的 a 标签元素`

#####  :octocat: [5.2 Document 一致性检测](#top) <b id="target7"></b> 
`Dom 是分多个级别的 有 DOM1 DOM2 DOM3 三个级别每个级别提供的功能不一样所以检测浏览器实现了Dom 那些部分就十分必要了`

#####  :octocat: [6.页面加载顺序](#top) <b id="target6"></b> 

* `页面加载大致的几个步骤`
  * `1.开始解析HTML文档结构。`
  * `2.加载外部样式表及JavaScript脚本。`
  * `3.解析执行JavaScript脚本。`
  * `4.DOM树渲染完成。`
  * `5.加载未完成的资源（如图片）。`
  * `6.页面加载成功。`
  * `页面加载触发的事件`
  
##### 1.document的readystatechange事件


```node
document.onreadystatechange = function() { // 文档加载状态改变事件处理
    if (document.readyState === "loading") { // document加载中
        console.log(document.readyState);
    }
    if (document.readyState === "interactive") { 
        // 互动文档加载完成，文档解析完成，但是如图像，样式表和框架等子资源仍在加载中
        console.log(document.readyState);
    }
    if (document.readyState === "complete") { // 文档和所有子资源加载完成，load事件即将被触发
        console.log(document.readyState);
    }
}
```
`readyState属性描述了文档的加载状态，整个加载过程中document.readyState会不断变化，每次变化都触发readystatechange事件。也可以用事件监听器去绑定：`

```node
document.addEventListener("readystatechange", function() {
    console.log(document.readyState);
});
```

##### 2.document的DOMContentLoaded事件

`DOM树渲染完成时候触发DOMContentLoaded事件，此时可能外部资源还在加载。jQuery中的ready事件就是实现的这个事件。`

##### 3.window的load事件

`当所有的资源全部加载完成后就会触发window的load事件。`

#####  :octocat: [7.Element](#top) <b id="target9"></b> 
`除了 Document 类型之外，Element 类型就要算是 Web 编程中常用的类型了。Element 类型用 于表现 XML或HTML元素，提供了对元素标签名、子节点及特性的访问。Element 节点具有以下特征`

* `nodeType 的值为 1；` 
* `nodeName 的值为元素的标签名；` 
* `nodeValue 的值为 null；` 
* `parentNode 可能是 Document 或 Element；`
* `其子节点可能是 Element、Text、Comment、ProcessingInstruction、CDATASection 或 EntityReference。 `

`要访问元素的标签名，可以使用 nodeName 属性，也可以使用 tagName 属性；这两个属性会返回 相同的值（使用后者主要是为了清晰起见）。`
```html
<button class="btn" name="saveCode" id="saveCode" data-val="14258Ada87">确认</button>
<script>
  window.onload = function(){
     var btnElement = document.getElementsByName("saveCode").item(0);
     console.log(btnElement.tagName, btnElement.nodeName);    //BUTTON BUTTON
     console.log(btnElement.nodeType, btnElement.nodeValue);  //1 null
  }
 <script/>
```

##### 1. HTML元素 
`所有 HTML元素都由 HTMLElement 类型表示，不是直接通过这个类型，也是通过它的子类型来表 示。HTMLElement类型直接继承自Element并添加了一些属性。添加的这些属性分别对应于每个HTML 元素中都存在的下列标准特性。 `

* `id`:`元素在文档中的唯一标识符。` 
* `title`: `有关元素的附加说明信息，一般通过工具提示条显示出来。` 
* `lang`:`元素内容的语言代码，很少使用。` 
* `dir`:`语言的方向，值为"ltr"（left-to-right，从左至右）或"rtl"（right-to-left，从右至左） ， 也很少使用。`
* `className`:`与元素的class特性对应，即为元素指定的CSS类。没有将这个属性命名为class， 是因为 class 是 ECMAScript的保留字 如果有多个类名就返回多个类名组成的字符串`
* `classList`:`返回类名组成的数组`

##### 2. 取得特性 
`每个元素都有一或多个特性，这些特性的用途是给出相应元素或其内容的附加信息。操作特性的 DOM方法主要有三个`

* `getAttribute()`
* `setAttribute()`
* `removeAttribute()`
* `attributes`:`Element 类型是使用 attributes 属性的唯一一个 DOM节点类型。attributes 属性中包含一个 NamedNodeMap，与 NodeList 类似，也是一个“动态”的集合。元素的每一个特性都由一个 Attr 节 点表示，每个节点都保存在 NamedNodeMap 对象中。NamedNodeMap 对象拥有下列方法。` 
   * `getNamedItem(name)`：`返回 nodeName 属性等于 name 的节点；` 
   * `removeNamedItem(name)`：`从列表中移除 nodeName 属性等于 name 的节点； `
   * `setNamedItem(node)`：`向列表中添加节点，以节点的 nodeName 属性为索引；` 
   * `item(pos)`：`返回位于数字 pos 位置处的节点`
   
```node
var div = document.getElementById("myDiv");
alert(div.getAttribute("id"));         //"myDiv" 
alert(div.getAttribute("class"));      //"bd" 

div.setAttribute("title", "Some other text"); 


window.setTimeout(()=>{
    div.removeAttribute('class');
},3000);
```

```node
var id = element.attributes.getNamedItem("id").nodeValue; 

element.attributes["id"].nodeValue = "someOtherId"; 

var oldAttr = element.attributes.removeNamedItem("id"); 
```
##### 3. 创建元素
`使用 document.createElement()方法可以创建新元素。这个方法只接受一个参数，即要创建元 素的标签名。这个标签名在 HTML文档中不区分大小写`
```node
    var div = document.createElement("div");
    div.innerText = "我是新元素";
    div.className = "box";
    document.body.appendChild(div);
```

#####  :octocat: [8.Text](#top) <b id="target10"></b> 
`文本节点由 Text 类型表示，包含的是可以照字面解释的纯文本内容。纯文本中可以包含转义后的 HTML字符，但不能包含 HTML代码。Text 节点具有以下特征：` 

* `nodeType 的值为 3； `
* `nodeName 的值为"#text"；` 
* `nodeValue 的值为节点所包含的文本； `
* `parentNode 是一个 Element； `
* `不支持（没有）子节点。 `

```node
  <input type="text" name="Code" id="ucode" class="input" value=""  placeholder="请输入秘钥" >

  var btnElement = document.getElementsByName("saveCode").item(0);
  console.log(btnElement.childNodes[0].nodeValue); //确认
```

`可以通过 nodeValue 属性或 data 属性访问 Text 节点中包含的文本，这两个属性中包含的值相同。
对 nodeValue 的修改也会通过 data 反映出来，反之亦然。`

* `appendData(text)：将 text 添加到节点的末尾。 `
* `deleteData(offset, count)：从 offset 指定的位置开始删除 count 个字符。 ` 
* `insertData(offset, text)：在 offset 指定的位置插入 text。 `
* `replaceData(offset, count, text)：用 text 替换从 offset 指定的位置开始到 offset`
* `splitText(offset)：从 offset 指定的位置将当前文本节点分成两个文本节点。` 
* `substringData(offset, count)：提取从 offset 指定的位置开始到 offset+count 为止 处的字符串`
* `length `:`保存着节点中字符的数目。`


##### 创建文本节点 
`document.createTextNode()创建新文本节点，这个方法接受一个参数——要插入节点 中的文本。与设置已有文本节点的值一样，作为参数的文本也将按照 HTML或 XML的格式进行编码`

```node
var element = document.createElement("div"); element.className = "message"; 
 
var textNode = document.createTextNode("Hello world!"); element.appendChild(textNode); 
 
document.body.appendChild(element); 
```

#####  :octocat: [9.Comment](#top) <b id="target11"></b> 
`注释在 DOM中是通过 Comment 类型来表示的`
 * `nodeType 的值为 8`
 * `nodeName 的值为"#comment"；` 
 * `nodeValue 的值是注释的内容；` 
 * `parentNode 可能是 Document 或 Element;`
 
 ```html
<div id="myDiv"><!--A comment --></div> 

var div = document.getElementById("myDiv"); 
var comment = div.firstChild; 
alert(comment.data);    //"A comment" 
```
`使用 document.createComment()并为其传递注释文本也可以创建注释节点`


#####  :octocat: [10.CDATASection](#top) <b id="target12"></b> 
`CDATASection 类型只针对基于 XML 的文档，表示的是 CDATA 区域。与 Comment 类似， CDATASection 类型继承自 Text 类型，因此拥有除 splitText()之外的所有字符串操作方法。 CDATASection 节点具有下列特征： `

* `nodeType 的值为 4；` 
* `nodeName 的值为"#cdata-section"；` 
* `nodeValue 的值是 CDATA区域中的内容；` 
* `parentNode 可能是 Document 或 Element；`

`<div id="myDiv"><![CDATA[This is some content.]]></div> ` : `CDATA 指的是不由 XML 解析器进行解析的文本数据`

`在真正的 XML文档中，可以使用 document.createCDataSection()来创建 CDATA区域，只需 为其传入节点的内容即可`


#####  :octocat: [11.DocumentType](#top) <b id="target13"></b> 

* `nodeType 的值为 10；` 
* `nodeName 的值为 doctype的名称；` 
* `nodeValue 的值为 null；` 
* `parentNode 是 Document；`

`在 DOM1 级中，DocumentType 对象不能动态创建，而只能通过解析文档代码的方式来创建。支 持它的浏览器会把 DocumentType 对象保存在 document.doctype 中。`

* `DOM1 级描述了 DocumentType 对象的 3个属性：name、entities 和 notations。`
   * `name 表示文档类型的名称；` 
   * `entities 是由文档类型描述的实体的 NamedNodeMap 对象；` 
   * `notations 是由文档类型描述的符号的 NamedNodeMap 对象;`
   
```html
<!DOCTYPE html>

alert(document.doctype.name);        //"HTML" 
```

#####  :octocat: [12.DocumentFragment](#top) <b id="target14"></b> 
 
* `nodeType 的值为 11；` 
* `nodeName 的值为"#document-fragment"；` 
* `nodeValue 的值为 null；` 
* `parentNode 的值为 null；`


`虽然不能把文档片段直接添加到文档中，但可以将它作为一个“仓库”来使用，即可以在里面保存将 来可能会添加到文档中的节点`

`要创建文档片段，可以使用 document.createDocumentFragment()方 法，如下所示： `
 
`var fragment = document.createDocumentFragment(); `

```node
var fragment = document.createDocumentFragment(); 

var ul = document.getElementById("myList"); var li = null; 
 
for (var i=0; i < 3; i++){    
  li = document.createElement("li");     
  li.appendChild(document.createTextNode("Item " + (i+1)));     
  fragment.appendChild(li); 
} 
 
ul.appendChild(fragment);  
```

##### :octocat: [13.Attr](#top) <b id="target15"></b>  
`元素的特性在 DOM中以 Attr 类型来表示。在所有浏览器中（包括 IE8），都可以访问 Attr 类型 的构造函数和原型。从技术角度讲，特性就是存在于元素的 attributes 属性中的节点。特性节点具有 下列特征： `

 * `nodeType 的值为 2； `
 * `nodeName 的值是特性的名称；` 
 * `nodeValue 的值是特性的值；` 
 * `parentNode 的值为 null；` 

`尽管它们也是节点，但特性却不被认为是 DOM 文档树的一部分。开发人员常使用的是 getAt- tribute()、setAttribute()和 remveAttribute()方法，很少直接引用特性节点。 `

```node
var attr = document.createAttribute("align"); attr.value = "left"; element.setAttributeNode(attr); alert(element.attributes["align"].value);       //"left" alert(element.getAttributeNode("align").value); //"left" alert(element.getAttribute("align"));           //"left" 
```

##### :octocat: [14.DOM操作技术](#top) <b id="target16"></b>  
`很多时候，DOM操作都比较简明，因此用 JavaScript 生成那些通常原本是用 HTML 代码生成的内 容并不麻烦。不过，也有一些时候，操作 DOM并不像表面上看起来那么简单。由于浏览器中充斥着隐 藏的陷阱和不兼容问题，用 JavaScript代码处理 DOM的某些部分要比处理其他部分更复杂一些`


##### 动态脚本 
`看看代码就应该懂了`
```node
<script type="text/javascript" src="client.js"></script>

var script = document.createElement("script"); 
script.type = "text/javascript"; 
script.src = "client.js"; 
document.body.appendChild(script); 

//整合为一个函数
function loadScript(url){     
  var script = document.createElement("script");     
  script.type = "text/javascript";     
  script.src = url;     
  document.body.appendChild(script); 
} 

//然后，就可以通过调用这个函数来加载外部的 JavaScript文件了： 
 
loadScript("client.js"); 
```

##### 同理有动态样式
`看看就懂了,这样就可以实现换肤了`
```node
<link rel="stylesheet" type="text/css" href="styles.css"> 

使用 DOM代码可以很容易地动态创建出这个元素： 
 
var link = document.createElement("link"); 
link.rel = "stylesheet"; 
link.type = "text/css"; 
link.href = "style.css"; 
var head = document.getElementsByTagName("head")[0]; 
head.appendChild(link); 
 
```

--------------------
`作者:` `JxKicker` 
`完成时间`:`2019年1月24日21:15:23`
`备注信息`: `任意使用` 
