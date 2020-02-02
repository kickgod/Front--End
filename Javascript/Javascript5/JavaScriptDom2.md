### [JavaScript DOM 扩展 ](#top) :grey_exclamation: <b id="top"></b>
`尽管 DOM作为 API 已经非常完善了，但为了实现更多的功能，仍然会有一些标准或专有的扩 展2008年之前，浏览器中几乎所有的 DOM扩展都是专
有的。此后，W3C着手将一些已经 成为事实标准的专有扩展标准化并写入规范当中。` :speech_balloon:

`对 DOM的两个主要的扩展是 Selectors API（选择符 API）和 HTML5`

------

- [x] [`1.选择符 API `](#target1)
   - [x] [`1.1 querySelector()`]
   - [x] [`1.2 querySelectorAll()`]
   - [x] [`1.3 matchesSelector()`]
- [x] [`2.元素遍历`](#target2)
   - [x] [`2.1 childElementCount`]
   - [x] [`2.2 firstElementChild`]
   - [x] [`2.3 lastElementChild`]
   - [x] [`2.4 previousElementSibling`]
   - [x] [`2.6 nextElementSibling`]
- [x] [`3.HTML5`](#target3)
   - [x] [`3.1 getElementsByClassName`]
   - [x] [`3.2 classList属性`]
   - [x] [`3.3 焦点管理`]
   - [x] [`3.4 readyState`]
   - [x] [`3.5 兼容模式`]
   - [x] [`3.6 head 属性`]
   - [x] [`3.7 charset 自定义数据属性dataset `]
- [x] [`4.插入标记`](#target4)
   - [x] [`4.1 innerHTML `] 
   - [x] [`4.2 outerHTML `] 
   - [x] [`4.3 insertAdjacentHTML `] 
- [x] [`5.scrollIntoView`](#target5)
- [x] [`6.专有扩展`](#target6)
   - [x] [`6.1 children`]
   - [x] [`6.2 contains`]
   - [x] [`6.3 innerText`]
   - [x] [`6.4 outerText`]
   - [x] [`6.5 滚动`]
   
------

#####  :octocat: [1.选择符 API ](#top) <b id="target1"></b> 
`是由 W3C 发起制定的一个标准，致力于让浏览器原 生支持 CSS查询。所有实现这一功能的 JavaScript库都会写一个基础的 CSS解析器，然后再使用已有的 DOM 方法查询文档并找到匹配的节点。尽管库开发人员在不知疲倦地改进这一过程的性能，但到头来 都只能通过运行 JavaScript代码来完成查询操作。而把这个功能变成原生 API之后，解析和树查询操作 可以在浏览器内部通过编译后的代码来完成，极大地改善了性能`

##### 1.1 querySelector()方法 
`querySelector()方法接收一个 CSS选择符，返回与该模式匹配的第一个元素，如果没有找到匹 配的元素，返回 null。请看下面的例子。 `

```node
//取得 body 元素 
var body = document.querySelector("body"); 
 
//取得 ID 为"myDiv"的元素 
var myDiv = document.querySelector("#myDiv"); 
```

##### 1.2 querySelectorAll()方法
`querySelectorAll()方法接收的参数与 querySelector()方法一样，都是一个 CSS选择符，但 返回的是所有匹配的元素而不仅仅是一个元素。这个方法返回的是一个 NodeList 的实例`
 
`只要传给 querySelectorAll()方法的 CSS选择符有效，该方法都会返回一个 NodeList 对象， 而不管找到多少匹配的元素。如果没有找到匹配的元素，NodeList 就是空的。` 

```node
//取得某<div>中的所有<em>元素（类似于 getElementsByTagName("em")） 
var ems = document.getElementById("myDiv").querySelectorAll("em"); 
 
//取得类为"selected"的所有元素 
var selecteds = document.querySelectorAll(".selected"); 
 
//取得所有<p>元素中的所有<strong>元素 
var strongs = document.querySelectorAll("p strong"); 
```

##### 1.3 matchesSelector()方法 
`Selectors API Level 2规范为 Element 类型新增了一个方法 matchesSelector()。这个方法接收 一个参数，即 CSS选择符，如果调用元素与该选择符匹配，返回 true；否则，返回 false。`

```node
if (document.body.matchesSelector("body.page1")){      
  //true 
}
```
#####  :octocat: [2.元素遍历 ](#top) <b id="target2"></b> 
`对于元素间的空格，IE9及之前版本不会返回文本节点，而其他所有浏览器都会返回文本节点。这样， 就导致了在使用 childNodes 和 firstChild 等属性时的行为不一致。为了弥补这一差异，而同时又保 持DOM规范不变，Element Traversal规范（www.w3.org/TR/ElementTraversal/）新定义了一组属性。 `

`Element Traversal API为 DOM元素添加了以下 5个属性。 `

* `childElementCount`：`返回子元素（不包括文本节点和注释）的个数。` 
* `firstElementChild`：`指向第一个子元素；firstChild 的元素版。` 
* `lastElementChild`：`指向后一个子元素；lastChild 的元素版。` 
* `previousElementSibling`：`指向前一个同辈元素；previousSibling 的元素版。` 
* `nextElementSibling`：`指向后一个同辈元素；nextSibling 的元素版。`

`支持的浏览器为 DOM元素添加了这些属性，利用这些元素不必担心空白文本节点，从而可以更方便地查找 DOM元素了。 `

#####  :octocat: [3.HTML5 ](#top) <b id="target3"></b>
`而 HTML5规范则围绕如何使用新增标记定义了大量 JavaScript API。其中一些 API与 DOM重叠， 定义了浏览器应该支持的 DOM扩展`

##### 与类相关的扩充 

* `1. getElementsByClassName()方法 `
> `接收一个参数，即一个包含一或多个类名的字符串，返回带有 指定类的所有元素的 NodeList。传入多个类名时，类名的先后顺序不重要。`
  
```node
//取得所有类中包含"username"和"current"的元素，类名的先后顺序无所谓 
var allCurrentUsernames = document.getElementsByClassName("username current");
```

* `2.classList属性 `:`返回类名数组 。这个 classList 属性是新集合类型 DOMTokenList 的实例。 这个新类型还定义如下方法。 `
  * `add(value)`：`将给定的字符串值添加到列表中。如果值已经存在，就不添加了。 `  
  * `contains(value)`：`表示列表中是否存在给定的值，如果存在则返回 true，否则返回 false。`
  * `remove(value)`：`从列表中删除给定的字符串。` 
  * `toggle(value)`：`如果列表中已经存在给定的值，删除它；如果列表中没有给定的值，添加它。`

* `3.焦点管理 `
> `HTML5也添加了辅助管理 DOM焦点的功能。首先就是 document.activeElement 属性，这个属性始终会引用 DOM中当前获得了焦点的元素。`

```node
var button = document.getElementById("myButton"); 
button.focus(); 
alert(document.activeElement === button);   //true 
```

`默认情况下，文档刚刚加载完成时，document.activeElement 中保存的是 document.body 元 素的引用。`

`文档加载期间，document.activeElement 的值为 null。 `

`另外就是新增了` `document.hasFocus()` `方法，这个方法用于确定文档是否获得了焦点。 `
```node
button.focus(); 
alert(document.hasFocus());  //true 
```
`通过检测文档是否获得了焦点，可以知道用户是不是正在与页面交互。 `

##### HTMLDocument 变化
`HTML5扩展了 HTMLDocument，增加了新的功能。与 HTML5中新增的其他 DOM扩展类似，这些 变化同样基于那些已经得到很多浏览器完美支持的专有扩展。所以，尽管这些扩展被写入标准的时间相 对不长，但很多浏览器很早就已经支持这些功能了。 `

* `1. readyState 属性 `: `IE4早为 document 对象引入了 readyState 属性。然后，其他浏览器也都陆续添加这个属性， 终 HTML5把这个属性纳入了标准当中。Document 的 readyState 属性有两个可能的值：`
   * `loading，正在加载文档；` 
   * `complete，已经加载完文档`

* `2. 兼容模式`:`自从 IE6开始区分渲染页面的模式是标准的还是混杂的，检测页面的兼容模式就成为浏览器的必要 功能。IE为此给 document 添加了一个名为 compatMode 的属性`

> `。IE为此给 document 添加了一个名为 compatMode 的属性，这个属性就是为了告诉开发人员浏 览器采用了哪种渲染模式。就像下面例子中所展示的那样，在标准模式下，document.compatMode 的 值等于"CSS1Compat"，而在混杂模式下，document.compatMode 的值等于"BackCompat"。 `
```node
if (document.compatMode == "CSS1Compat"){     
  alert("Standards mode"); 
  } else {
  alert("Quirks mode"); 
}
```
* `3.head属性`:`作为对 document.body 引用文档的<body>元素的补充，HTML5新增了 document.head 属性， 引用文档的<head>元素。要引用文档的<head>元素，可以结合使用这个属性和另一种后备方法。 `  `如果可用，就使用 document.head，否则仍然使用 getElementsByTagName()方法。 `

```node
var head = document.head || document.getElementsByTagName("head")[0]; 
```

##### 字符集属性 
`HTML5新增了几个与文档字符集有关的属性。其中，charset 属性表示文档中实际使用的字符集， 也可以用来指定新字符集`

```node
alert(document.charset); //"UTF-16" 
document.charset = "UTF-8"; 
```
`另一个属性是 defaultCharset，表示根据默认浏览器及操作系统的设置，当前文档默认的字符集 应该是什么。如果文档没有使用默认的字符集，那 charset 和 defaultCharset 属性的值可能会不一 样`

```node
if (document.charset != document.defaultCharset){      
   alert("Custom character set being used.");
} 
```
##### 自定义数据属性 
`HTML5规定可以为元素添加非标准的属性，但要添加前缀 data-，目的是为元素提供与渲染无关的 信息，或者提供语义信息。这些属性可以任意添加、随便命名，只要以 data-开头即可 ，每个 data-name 形式 的属性都会有一个对应的属性，只不过属性名没有 data-前缀（比如，自定义属性是 data-myname， 那映射中对应的属性就是 myname）。`
```node
<div id="myDiv" data-appId="12345" data-myname="Nicholas"></div> 

var div = document.getElementById("myDiv"); 
 
//取得自定义属性的值 
var appId = div.dataset.appId; 
var myName = div.dataset.myname;

//设置值 
div.dataset.appId = 23456; 
div.dataset.myname = "Michael";
```
#####  :octocat: [4.插入标记 ](#top) <b id="target4"></b>  

##### 1. innerHTML 属性 
`在读模式下，innerHTML 属性返回与调用元素的所有子节点（包括元素、注释和文本节点）对应 的 HTML标记`

```html
 <div id="content">
     <p>This is a <strong>paragraph</strong> with a list following it.</p>
     <ul>
         <li>Item 1</li>
         <li>Item 2</li>
         <li>Item 3</li>
     </ul>
 </div>
```
`对于上面的<div>元素来说，它的 innerHTML 属性会返回如下字符串。 `

```html
<p>This is a <strong>paragraph</strong> with a list following it.</p>
<ul>
   <li>Item 1</li>
   <li>Item 2</li>
   <li>Item 3</li>
</ul>
```
`使用 innerHTML 属性也有一些限制。比如，在大多数浏览器中，通过 innerHTML 插入<script> 元素并不会执行其中的脚本。`

`div.innerHTML = "_<script defer>alert('hi');<\/script>";`

##### 2.outerHTML 属性
`在读模式下，outerHTML 返回调用它的元素及所有子节点的 HTML标签。在写模式下，outerHTML 会根据指定的 HTML字符串创建新的 DOM子树，然后用这个 DOM子树完全替换调用元素。下面是一 个例子。 `

`对于上面的<div>元素来说，它的 outerHTML 属性会返回如下字符串。 `

```node
 <div id="content">
     <p>This is a <strong>paragraph</strong> with a list following it.</p>
     <ul>
         <li>Item 1</li>
         <li>Item 2</li>
         <li>Item 3</li>
     </ul>
 </div>
```
##### 3.insertAdjacentHTML
`插入标记的后一个新增方式是insertAdjacentHTML()方法。这个方法早也是在IE中出现的， 它接收两个参数：插入位置和要插入的 HTML文本。第一个参数必须是下列值之一： `

* `"beforebegin"，在当前元素之前插入一个紧邻的同辈元素；` 
* `"afterbegin"，在当前元素之下插入一个新的子元素或在第一个子元素之前再插入新的子元素；`
* `"beforeend"，在当前元素之下插入一个新的子元素或在后一个子元素之后再插入新的子元素；`
* `"afterend"，在当前元素之后插入一个紧邻的同辈元素。`

#####  :octocat: [5.scrollIntoView ](#top) <b id="target5"></b>  
`如何滚动页面也是 DOM规范没有解决的一个问题。为了解决这个问题，浏览器实现了一些方法， 以方便开发人员更好地控制页面滚动。在各种专有方法中，HTML5终选择了 scrollIntoView()作 为标准方法`

 `scrollIntoView()可以在所有 HTML 元素上调用，通过滚动浏览器窗口或某个容器元素，调用 元素就可以出现在视口中。如果给这个方法传入 true 作为参数，或者不传入任何参数，那么窗口滚动 之后会让调用元素的顶部与视口顶部尽可能平齐。如果传入 false 作为参数，调用元素会尽可能全部 出现在视口中，（可能的话，调用元素的底部会与视口顶部平齐。）不过顶部不一定平`

`让元素可见`: `document.forms[0].scrollIntoView(); `

#####  :octocat: [6.专有扩展 ](#top) <b id="target6"></b>
`通过 document.documentMode 属性可以知道给定页面使用的是什么文档模式。这个属性是 IE8 中新增的，它会返回使用的文档模式的版本号（在 IE9中，可能返回的版本号为 5、7、8、9）： `

##### children
`这个属性是 HTMLCollection 的实例，只包含元素中同样还是元素的子节点。除此之外， children 属性与 childNodes 没有什么区别`
```node
var childCount = element.children.length; 
var firstChild = element.children[0]; 
```

##### contains()
`在实际开发中，经常需要知道某个节点是不是另一个节点的后代。IE为此率先引入了 contains() 以便不通过在 DOM文档树中查找即可获得这个信息。调用 contains()方法的应该是祖先节点`

```node
alert(document.documentElement.contains(document.body));    //true 
```

##### 插入文本 

* `innerText`:`通过 innertText 属性可以操作元素中包含的所有文本内容，包括子文档树中的文本。`
```node
<div id="content">
  <p>This is a <strong>paragraph</strong> with a list following it.</p>
  <ul>
      <li>Item 1</li>
      <li>Item 2</li>
      <li>Item 3</li>
  </ul>
</div>
//innerText 为
This is a paragraph with a list following it. 
Item 1 
Item 2 
Item 3 
```
* `outerText `:`除了作用范围扩大到了包含调用它的节点之外，outerText 与 innerText 基本上没有多大区别。 在读取文本值时，outerText 与 innerText 的结果完全一样。但在写模式下，outerText 就完全不 同了：outerText 不只是替换调用它的元素的子节点，而是会替换整个元素（包括子节点）`


##### 滚动
`如前所述，HTML5 之前的规范并没有就与页面滚动相关的 API 做出任何规定。但 HTML5 在将 scrollIntoView()纳入规范之后，仍然还有其他几个专有方法可以在不同的浏览器中使用。下面列出 的几个方法都是对 HTMLElement 类型的扩展，因此在所有元素中都可以调用。 `

*  `scrollIntoViewIfNeeded(alignCenter)`：`只在当前元素在视口中不可见的情况下，才滚 动浏览器窗口或容器元素，终让它可见。如果当前元素在视口中可见，这个方法什么也不做。 如果将可选的 alignCenter 参数设置为 true，则表示尽量将元素显示在视口中部（垂直方向）。 Safari和 Chrome实现了这个方法。` 

*  `scrollByLines(lineCount)：将元素的内容滚动指定的行高，lineCount 值可以是正值， 也可以是负值。Safari和 Chrome实现了这个方法。`

* `scrollByPages(pageCount)：将元素的内容滚动指定的页面高度，具体高度由元素的高度决 定。Safari和 Chrome实现了这个方法`


--------------------
`作者:` `JxKicker` 
`完成时间`:`2020年2月2日16:46:58`
`备注信息`: `任意使用` 
