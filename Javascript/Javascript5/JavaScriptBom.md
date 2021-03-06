### [JavaScript BOM](#top) :grey_exclamation: <b id="top"></b>
 `Javascript BOM提供了很多对象,用于访问浏览器的功能,这些功能与任何网页内容无关 ES5的BOM 具有浏览器各种不兼容的地方,好在H5 已经在逐渐规范了`
 :white_check_mark: 

---
> - [x] [`1.window 对象`](#window) 
> - [x] [`2.窗口关系和框架`](#frame) 
> - [x] [`3.窗口位置`](#place) 
>   * [`3.1浏览器相对于屏幕的位置,大小`](#screen)
>   * [`3.2打开一个新的窗口`](#open)
> - [x] [`4.定时器`](#setInternal) 
> - [x] [`5.系统对话框`](#dialog) 
> - [x] [`6.location 对象`](#location) 
> - [x] [`7.navigator 对象`](#navigator) 
> - [x] [`8.screen 对象`](#dscreen) 
> - [x] [`9.history 对象`](#history) 

----

#####  :octocat: [1.window 对象](#top) <b id="window"></b> 
`它是全局作用域 有两个身份`
* :one: `它是通过JavaScript 访问浏览器窗口的一个借口`
* :two: `ECMAScript 规定的Global对象`
```node
var index = 0;

function addItem(num){
    this.index += num;
    return this.index;
}

window.addItem(10);
window.addItem(20);

console.log(window.index);//30

```
##### [window API](https://developer.mozilla.org/en-US/docs/Web/API/Window)

* `属性`
  * `closed`:`read-only property indicates whether the referenced window is closed or not.`
  * `console `:`property returns a reference to the Console object, which provides methods for logging information to the browser's console.`
  * `Crypto`:`该Crypto接口表示当前上下文中可用的基本加密功能。它允许访问加密强度高的随机数生成器和加密基元。`
  ```node
     var array = new Uint32Array(2);
     window.crypto.getRandomValues(array);

     console.log("Your lucky numbers:");
     for (var i = 0; i < array.length; i++) {
         console.log(array[i]);
     }
  ```
  * [`customElements`](https://blog.csdn.net/YiYour/article/details/79425242)
  * `document`
  * `event`
  * `history`
  * `indexedDB` :`只读属性WindowOrWorkerGlobalScope为应用程序提供了一种机制，可以异步访问索引数据库的功能。`
  * `innerHeight`
  * `innerWidth`
  * `length`：`返回窗口中的frame数量`
  * [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Storage):`The read-only localStorage property allows you to access a Storage object for the Document's origin; the stored data is saved across browser sessions. localStorage is similar to sessionStorage, except that while data stored in localStorage has no expiration time, data stored in sessionStorage gets cleared when the page session ends — that is, when the page is closed`
  * [`sessionStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)
  * `navigator`
  * `scrollbars`:`返回scrollbars对象，可以检查其可见性`
  ```html
   <script>
     let visibleScrollbars = window.scrollbars.visible;
   </script>  
  ```
  * `scrollX`:`您可以从scrollX属性获取垂直滚动文档的像素数`
  * `scrollY`:`您可以从scrollY属性获取垂直滚动文档的像素数`
* `方法`
  * `blur()`:`将焦点从窗口移开。`
  * `alert()`
  * `prompt()`
  * `setInterval()`
  * `setTimeout()`
  * `clearInterval()`
  * `clearTimeout()`
  * `close()`:`立即关闭当前页面`
  * `confirm()`
  * `focus()`
  * [`matchMedia()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/matchMedia):`媒体匹配查询`
  ```node
  let mql = window.matchMedia('(max-width: 600px)');
  ```
  * `scrollTo()`
  * `scroll()`
  * `scrollBy()`
  * `stop()`:`将window.stop()停止在当前浏览器上下文进一步的资源负荷，相当于在浏览器中的停止按钮。由于脚本的执行方式，该方法无法中断其父文档的加载，但会停止其图像，新窗口和其他仍在加载的对象。`
* `事件`
  * `error`
  * `focus`
  * `blur`
  * `load`
  * `unload`
  * `offline`
  * `online`
  * `focus`
 #####  :octocat: [2.窗口关系和框架](#top) <b id="frame"></b> 
`如果页面中包含框架 [frame 标签] 则每一个框架都拥有自己window 对象,并且保存在 frame集合中,在frames集合中,可以通过数值索引【从0 开始 从左向右 自上而下】 或者通过 框架名称访问相应的 windo对象`
```html
<body>
    <frameset>
        <iframe src="./frameTop.html" name="top" frameborder="0"></iframe>
        <iframe src="./frameBottom.html" name="bottom" frameborder="0"></iframe>
    </frameset>
</body>
```
* :arrow_lower_left: `1.每个 window对象都有一个name属性 其中包含框架的名称`
* :arrow_lower_left: `2.window.frames[0].parent 执行框架的上层框架 `
* :arrow_lower_left: `3.window.frames[0].window 每一个框架都有自己的 window对象`
* :arrow_lower_left: `4.window.frames[0].top 始终实现最外围的框架,也就是浏览器窗口`
```javascript
console.log(window.index);//30

console.log(window.frames.length); //具有两个 frame
console.log(window.frames["bottom"]); // 第二个 
console.log(window.frames[0]); // 第一个top

console.log(window.frames[0].window);//每个frame 都有自己的window 对象

console.log(window.frames[0].parent == window.frames[1].parent); //true
```
----
`请注意 都 9102年了，就不要再你的html文档里面是有 iframe 这种过时的玩意儿了 `

#####  :octocat: [3.窗口位置](#top) <b id="place"></b>
`窗口分好多类, 浏览器窗口 可视区域 页面位置....眼花缭乱的！！！！！并且各个浏览器兼容性还不一样`

##### :one:  [ 3.1 浏览器相对于屏幕的位置,大小](#top) <b id="screen"></b>
`screenLeft screenTop`:`IE Safari Opera Chrome 都支持` <br/>
`screenX ScreenY`:`FireFox Chrome 支持`<br/>
```node
/**
 *  浏览器和屏幕的距离 左上角的距离
 */
 function getScreenPosition(){
    let left = (typeof window.screenLeft == "number") ? window.screenLeft:window.screenX;
    let top = (typeof window.screenTop == "number") ? window.screenTop:window.screenY;
    return [left,top];
 }
```
`window.moveTo(10,0)`:`将窗口移动到 10.0 [现在已经不起作用了]`<br/>
`window.moveBy(0,100)`:`窗口向下移动100像素[现在已经不起作用了]`<br/>

##### 窗口大小
`跨浏览器解决窗口大小不是一件很简单的事情,IE9,FireFox Safari,Opera,Chrome 为此提供了四个属性：innerWith`
* `window.innerHeight;`
* `window.innerWidth;`
* `window.outerWidth;` 
* `window.outerHeight;`
> * `Chrome : innerHeight,innerWidth 和 outerWidth,outerHeight 返回相同的值`
> * `Opera`:`outerWidth,outerHeight  返回浏览器窗口本身的尺寸 innerHeight,innerWidth 则表示该容器中页面视图区的大小[减去边框宽度]`
```node
 window.onresize= function getLocation() {
    console.log(window.innerHeight); 
    console.log(window.innerWidth); 
    console.log(window.outerHeight); 
    console.log(window.outerWidth); 
 }
```
* `document.documentElement.clientWidth;`:`页面视口的宽度`
* `document.documentElement.clientHeight;`:`页面视口的高度`
> * `IE9,FireFox Safari,Opera,Chrome  它保存视口信息 IE6 必须在标准模式下才有用[请自动忽略IE 浏览器所有兼容问题,让我们联合起来消灭IE]`
> * `混杂模式[Dom 文档的一种模式]下需要使用如下API来得到视口信息 混杂模式下 Chrome 不支持clientWidth 和 clientHeight`
>     * ` document.body.clientWidth`
>     * ` document.body.clientHeight`
> * `移动浏览器通过documentElement 度量视口`
```node
let width = document.documentElement.clientWidth;
let height = document.documentElement.clientHeight;

console.log(width,height);

let dWidth = document.body.clientWidth;
let dHeight = document.body.clientHeight;

console.log(dWidth,dHeight);
```
##### 窗口大小问题
**`许多浏览器都禁止调整浏览器窗口大小的能力！----所以下面的函数 就很尴尬...`**<br/>
`window.resizeTo(100,100)`:`将窗口大小变成 100,100`<br/>
`window.resizeBy(100,100)`:`将窗口长宽各增加 100,100`<br/>

##### :two:  [ 3.2打开一个新的窗口](#top) <b id="open"></b>
`使用window.open() 方法既可以导航到一个特点的URL 也可以打开一个新的 浏览器窗口,这个方法接受四个参数:要加载的URL。窗口目标,一个特性字符串以及表示
新页面是否取代浏览器历史记录当前加载页面的布尔值`
> `9102 年了,我们可以放弃它了 需要看的`[`参考文档`](https://developer.mozilla.org/en-US/docs/Web/API/Window/open)

##### :octocat:  [ 4.定时器](#top) <b id="setInternal"></b>
`什么时候执行,每隔多少秒执行一次 你需要我解释这个 API 吗? 不可能的这太简单了`
* `let timer = setTimeOut(function(params...),ms,params...)`
* `clearTimeOut(timer)`
* `let internal = setInterval(function(params...),ms,params...)`
* `clearInternal(internal)`
```node
let internal = window.setInterval(function(num,num2){
    console.log(num + num2);//一秒一个8
},1000,3,5);


window.setTimeout(function(){
    clearInterval(internal);
},5000); //停止输出8
```
##### :octocat:  [ 5.系统对话框](#top) <b id="dialog"></b>
`alert(info)`:`弹出警告框` </br>
`let input = propmt(title)`: `input 返回用户输入` </br>
`let isOk = confirm(title)`: `返回布尔值用户的输入` </br>

##### :octocat:  [ 6.location](#top) <b id="location"></b>
`这是最有用的Bom对象,它提供了当前窗口加载的文档的Url有关信息,提供了导航功能`
* `window.location 它下面的一样 是同一个对象 `
* `document.location`

|`属性名`|`例子`|`说明`|
|:----|:----|:----|
|`hash` |`#contents`|`返回URL中的hash(#号后跟零或多个字符),如果URL 中不包含散列,则返回空字符串`|
|`host`|`www.baidu.com:443`|`返回服务器名称[ip]和 端口号`|
|`hostname`|`www.baidu.com`|`服务器名称不带端口`|
|`href`|`https://www.baidu.com`|`完整的URL locationd对象的toString() 方法也返回这个值`|
|`pathname`|`/kicker/user/`|`返回文件目录`|
|`port`|`端口号`|`返回URL 中指定的端口号。如果URL中不包含端口号,则这个属性返回空字符串`|
|`protocol`|`http:`|`返回页面使用的协议 http/https`|
|`search`|`?qid=201611jxkicker`|`返回URL查询字符串`|

```node
console.log(location.href);
console.log(location.pathname);
console.log(location.hostname);
```
##### `获得查询字符串`
`http://localhost/dist/index.html?index=5&name=kicker&sex=true`
```node
function getQueryStringArgs(){

    //get query string without the initial ?
    var qs = (location.search.length > 0 ? location.search.substring(1) : ""),

        //object to hold data
        args = {},

        //get individual items
        items = qs.length ? qs.split("&") : [],
        item = null,
        name = null,
        value = null,

        //used in for loop
        i = 0,
        len = items.length;

    //assign each item onto the args object
    for (i=0; i < len; i++){
        item = items[i].split("=");
        name = decodeURIComponent(item[0]);
        value = decodeURIComponent(item[1]);

        if (name.length){
            args[name] = value;
        }
    }

    return args;
}

console.log(getQueryStringArgs());
```
##### :two:  [ 6.2.位置操作](#top) <b id="navigator"></b>
`使用location对象可以通过很多方式来改变浏览器的位置,首先,也是最常用的方式，就是使用assign() 方法`
* `以下三个方法都是可以重新定位页面 他们本质都是调用 assign方法`
```node
location.assign("http://www.baidu.com");

window.location = "http://www.baidu.com";
window.location.href = "http://www.baidu.com";
```
* `你可以通过改变上面的除了 hash之外的任意属性 都会导致页面重定向,同时关键的是`:`他会在浏览器的历史中生成的一条记录`
* `location.reload()`:`重新加载 如果不传参数 有可能从缓存中加载`
* `location.reload(true)`:`重新加载,从服务端重新加载`

##### :octocat:  [7.navigator 对象](#top) <b id="navigator"></b>
`它是标识客户端浏览器的事实标准`

|`属性方法`|`说明`|
|:----|:-----|
|`appCodeName`|`浏览器的名称。通常都是Mozilla 即使在非Mozilla浏览器中也是如此`|
|`appMinorVersion`|`此版本信息`|
|`appName`|`完整的浏览器名称`|
|`appVersion`|`完整的版本,一般不与实际的浏览器版本对应`|
|`buildID`|`浏览器编译版本`|
|`cookieEnable`|`标识cookie是否启用`|
|`language`|`浏览器语言`|
|`cpuClass`|`客户端计算机使用的CPU类型(X86,Alpha，PPC,Other)`|
|`mineType`|`浏览器中注册的MIME类型数组`|
|`onLine`|`表示浏览器是否联网`|
|`oscpu`|`客户端计算机的操作系统或使用的CPU`|
|`platform`|`浏览器所在的系统平台`|
|`plugins`|`浏览器安装插件信息的数组`|
|`preference()`|`设置系统首选项`|
|`product`|`产品名称`|
|`userAgent`|`浏览器的用户代理字符串`|

##### 插件检查
`检查浏览器是否安装了特定的插件 通过 plugins plugins 数组成员具有一些属性` 
* `name`:`插件名称`
* `description`:`插件的描述`
* `filename`:`插件文件名称`
* `length`:`插件所处理的MIME类型的数量`
```javascript
export function hasPlugins(name){
    name = name.toLowerCase();
    for( let index = 0 ;index < navigator.plugins.length; index++){
        if(navigator.plugins[index].name.toLowerCase().indexOf(name) > -1 ){
            return true;
        }
    }
    return false;
}

if(hasPlugins("Flash")){
    alert("具备Flash");
}else{
    alert("尚未安装Flash");
}
```
##### :octocat:  [8.screen 对象](#top) <b id="dscreen"></b>
`他的能力就是展示一些客户端的信息,例如浏览器窗口外部显示器的信息,如像素宽度,和高度等等 每一个浏览器的screen 属性可能各不相同`

|`属性`|`说明`|`兼容性`|
|:----|:----|:----|
|`avaliHeight`|`屏幕的像素的高端减系统部件高度之后的值[只读]`|`都支持`|
|`avaliWidth`|`屏幕的像素宽度减系统部件宽度之后的值[只读]`|`都支持`|
|`colorDepth`|`由于表现颜色的位数,多数是32位[只读]`|`都支持`|
|`width`|`屏幕的宽度`|`都支持`|
|`height`|`屏幕的高度`|`都支持`|

##### :octocat:  [9.history 对象](#top) <b id="history"></b>
`这个对象保存着用户上网的历史记录,从这个窗口打开的那一刻算起,因为history是window对象的属性,所以每一个窗口框架都有自己的 history,出于安全，开发人员无法得知用户浏览过的URL。不过,借由用户访问过的页面列表,痛呀可以在不知道实际URL的情况下进行后退和前进`


* `go()`:`histroy.go(-1) 后退一页，histroy.go(1) 前进一页 `
* `back()`:`后退一页`
* `forward()`:`前进一页`
* `length`:`用户打开了几个页面了！`
```node
if(history.length == 0){
   //这是第一次打开的页面
}
```
* `history.pushState()方法将状态添加到浏览器的会话历史记录堆栈中。`
```node
let state = { 'page_id': 1, 'user_id': 5 }
let title = ''
let url = 'hello-world.html'

history.pushState(state, title, url)
```
* `replaceState()`:`更新历史记录堆栈上的最新条目以具有指定的数据，标题和URL（如果提供）。数据被DOM视为不透明；您可以指定任何可以序列化的JavaScript对象。请注意，除Safari外，所有浏览器当前都忽略title参数。` 

`他们最大的特点是添加或替换历史记录后，浏览器地址栏会变成你传的地址，而页面并不会重新载入或跳转`

-------
`Bom 就这些内容吧 ！ 相信你都应该记住它们！ 当前还有许多的不常用的API 并未提及`















