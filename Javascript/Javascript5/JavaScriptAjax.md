### <a id="top" href="#top"> Javascript 原生 Ajax :grey_exclamation:</a>
`Asynchronous JavaScript + XML 这一技术能够向服务器请求额外的数据而无须卸载页面,会带来更好的用户体验,`:speech_balloon:

-----

- [x] [`1.XMLHttpRequest 对象`](#target1)
- [x] [`2.HTTP头部信息 `](#target2)
- [x] [`3.XMLHttpRequest2 对象 `](#target3)
- [x] [`?.XMLHttpRequest2 对象`](#XMLHttpRequest2)

----

#####   [1.XMLHttpRequest 对象](#top) <b id="target1"></b>
`IE5是第一款引入 XHR对象的浏览器。在 IE5中，XHR对象是通过 MSXML 库中的一个 ActiveX 对象实现的。因此，在 IE 中可能会遇到三种不同版本的 XHR 对象`
`所以啊！ 活该IE被彻底淘汰掉` [`MDN文档`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)

`IE7+、Firefox、Opera、Chrome和 Safari都支持原生的 XHR对象，在这些浏览器中创建 XHR对象 要像下面这样使用 XMLHttpRequest 构造函数。 `
```javascript 
var xhr = new XMLHttpRequest();
```

#####  :octocat: 1.1 XHR的用法 
`在使用 XHR 对象时，要调用的第一个方法是 open()，它接受 3 个参数：要发送的请求的类型 （"get"、"post"等）、请求的 URL和表示是否异步发送请求的布尔值。`

```javascript
xhr.open("get", "example.php", false); 
```

`这行代码会启动一个针对 example.php 的 GET 请求。有关这行代码，需要说明两点：一是 URL 相对于执行代码的当前页面（当然也可以使用绝对路径）；二是调用 open()方法并不会真正发送请求， 而只是启动一个请求以备发送`

`要发送特定的请求，必须像下面这样调用 send()方法： `
 
 ```node
let xhr = new XMLHttpRequest();
xhr.open("get", "example.txt", false);
xhr.send(null); 
```
`这里的 send()方法接收一个参数，即要作为请求主体发送的数据。如果不需要通过请求主体发送 数据，则必须传入 null，因为这个参数对有些浏览器来说是必需的。调用 send()之后，请求就会被分 派到服务器。 `

`由于这次请求是同步的，JavaScript 代码会等到服务器响应之后再继续执行。在收到响应后，响应 的数据会自动填充 XHR对象的属性，相关的属性简介如下。 `

* `responseText`：`作为响应主体被返回的文本。` 
* `responseXML`：`如果响应的内容类型是"text/xml"或"application/xml"，这个属性中将保 存包含着响应数据的 XML DOM文档。` 
* `status`：`响应的 HTTP状态。 `
* `statusText`：`HTTP状态的说明`

`在接收到响应后，第一步是检查 status 属性，以确定响应已经成功返回。一般来说，可以将 HTTP 状态代码为 200作为成功的标志。此时，responseText 属性的内容已经就绪，而且在内容类型正确的 情况下，responseXML 也应该能够访问了`

`状态代码为 304 表示请求的资源并没有被修改，可 以直接使用浏览器中缓存的版本；当然，也意味着响应是有效的。为确保接收到适当的响应，应该像下 面这样检查上述这两种状态代码：`

```node
xhr.open("get", "example.txt", false); 
xhr.send(null); 
 
if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){ 
   alert(xhr.responseText); 
 } else {  
   alert("Request was unsuccessful: " + xhr.status);
}
```
`根据返回的状态代码，这个例子可能会显示由服务器返回的内容，也可能会显示一条错误消息。我 们建议读者要通过检测 status 来决定下一步的操作，不要依赖 statusText，因为后者在跨浏览器使 用时不太可靠。`

##### :octocat: 1.2 readyState 
`像前面这样发送同步请求当然没有问题，但多数情况下，我们还是要发送异步请求，才能让 JavaScript继续执行而不必等待响应。此时，可以检测 XHR对象的 readyState 属性，该属性表示请求 /响应过程的当前活动阶段。这个属性可取的值如下。 `

* `0`：`未初始化。尚未调用open()方法`
* `1`：`启动。已经调用open()方法，但尚未调用send()方法`
* `2`：`发送。已经调用send()方法，但尚未接收到响应`
* `3`：`接收。已经接收到部分响应数据`
* `4`：`完成。已经接收到全部响应数据，而且已经可以在客户端使用了`

`只要 readyState 属性的值由一个值变成另一个值，都会触发一次 readystatechange 事件。可 以利用这个事件来检测每次状态变化后 readyState 的值。通常，我们只对 readyState 值为 4的阶 段感兴趣，因为这时所有数据都已经就绪。不过，必须在调用 open()之前指定 onreadystatechange 事件处理程序才能确保跨浏览器兼容性。`

```node
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function () {
    if (xhr.readyState == 4) {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
            alert(xhr.responseText);
        } else {
            alert("Request was unsuccessful: " + xhr.status);
        }
    }
};
xhr.open("get", "example.txt", true);
xhr.send(null); 
```
`与其他事件处理程序不同，这里没有向 onreadystatechange 事件处理程序中传递 event 对象； 必须通过 XHR 对象本身来确定下一步该怎么做。 `

`另外，在接收到响应之前还可以调用 abort()方法来取消异步请求，如下所示： `
 
```node
xhr.abort();
```
`调用这个方法后，XHR 对象会停止触发事件，而且也不再允许访问任何与响应有关的对象属性。在 终止请求之后，还应该对 XHR对象进行解引用操作。由于内存原因，不建议重用 XHR 对象。 `

#####  [2.HTTP头部信息](#top) <b id="target2"></b>
`每个 HTTP请求和响应都会带有相应的头部信息，其中有的对开发人员有用，有的也没有什么用。 XHR 对象也提供了操作这两种头部（即请求头部和响应头部）信息的方法。 `

* `Accept`：`浏览器能够处理的内容类型。` 
* `Accept-Charset`：`浏览器能够显示的字符集。` 
* `Accept-Encoding`：`浏览器能够处理的压缩编码。` 
* `Accept-Language`：`浏览器当前设置的语言。` 
* `Connection`：`浏览器与服务器之间连接的类型。` 
* `Cookie`：`当前页面设置的任何 Cookie。` 
* `Host`：`发出请求的页面所在的域 。` 
* `Referer`：`发出请求的页面的 URI。注意，HTTP规范将这个头部字段拼写错了，而为保证与规范一致，也只能将错就错了。（这个英文单词的正确拼法应该是 referrer。）` 
* `User-Agent`：`浏览器的用户代理字符串。 `

`虽然不同浏览器实际发送的头部信息会有所不同，但以上列出的基本上是所有浏览器都会发送的。 使用 setRequestHeader()方法可以设置自定义的请求头部信息。这个方法接受两个参数：头部字段 的名称和头部字段的值。`

`要成功发送请求头部信息，必须在调用 open()方法之后且调用 send()方法 之前调用 setRequestHeader()，如下面的例子所示`

```node
var xhr = createXHR();
xhr.onreadystatechange = function () {
    if (xhr.readyState == 4) {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
            alert(xhr.responseText);
        } else {
            alert("Request was unsuccessful: " + xhr.status);
        }
    }
};
xhr.open("get", "example.php", true);
xhr.setRequestHeader("MyHeader", "MyValue");
xhr.send(null);
```
`服务器在接收到这种自定义的头部信息之后，可以执行相应的后续操作。我们建议读者使用自定义 的头部字段名称，不要使用浏览器正常发送的字段名称，否则有可能会影响服务器的响应。有的浏览器 允许开发人员重写默认的头部信息，但有的浏览器则不允许这样做。 `

* `getResponseHeader()` :`方法并传入头部字段名称，可以取得相应的响应头部信息。`
* `getAllResponseHeaders()`: `方法可以取得一个包含所有头部信息的长字符串。 `

```javascript
var myHeader = xhr.getResponseHeader("MyHeader");
var allHeaders = xhr.getAllResponseHeaders(); 
```
`在服务器端，也可以利用头部信息向浏览器发送额外的、结构化的数据。在没有自定义信息的情况 下，getAllResponseHeaders()方法通常会返回如下所示的多行文本内容： `
```text
Date: Sun, 14 Nov 2004 18:04:03 GMT 
Server: Apache/1.3.29 (Unix) 
Vary: Accept 
X-Powered-By: PHP/4.3.8 
Connection: close 
Content-Type: text/html; charset=iso-8859-1
```
`这种格式化的输出可以方便我们检查响应中所有头部字段的名称，而不必一个一个地检查某个字段 是否存在。 `

##### :octocat: 2.2 HTTP方法 
`HTTP方法 比如get或者post大小写无所谓`

 * `GET`:`请求应该用于从服务器端获取数据为目的,GET 是常见的请求类型，常用于向服务器查询某些信息。必要时，可以将查询字符串参数追加 到 URL的末尾，以便将信息发送给服务器`
 * `POST`:`方式主要是为了将数据提交给服务器端并且存储（比如将数据提交到服务器端，且写入数据库）！`'
 
 
 `使用 GET 请求经常会发生的一个错误，就是查询字符串的格式有问题。查询字符串中每个参数的名 称和值都必须使用 encodeURIComponent()进行编码，然后才能放到 URL 的末尾；而且所有名-值对 儿都必须由和号（&）分隔`
 
`使用频率仅次于 GET 的是 POST 请求，通常用于向服务器发送应该被保存的数据。POST 请求应该 把数据作为请求的主体提交，而 GET 请求传统上不是这样。POST 请求的主体可以包含非常多的数据， 而且格式不限。在 open()方法第一个参数的位置传入"post"，就可以初始化一个 POST 请求，如下面 的例子所示` 

```node
function submitData() {
    var xhr = createXHR();
    xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
            if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
                alert(xhr.responseText);
            } else {
                alert("Request was unsuccessful: " + xhr.status);
            }
        }
    };

    xhr.open("post", "postexample.php", true);
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    /*首先将 Content-Type 头部信息设置为 application/x-www-form-urlencoded
      也就是表单 提交时的内容类型
    */
    var form = document.getElementById("user-info");
    xhr.send(serialize(form)); /*serialize() 格式化表单信息*/
}
```

##### :octocat: 2.2 HTTP支持的八种方法 

|`序号`|`方法`|`说明`|
|:--|:--|:--|
|`1`|`GET`|`发送请求来获得服务器上的资源，请求体中不会包含请求数据，请求数据放在协议头中。另外get支持快取、缓存、可保留书签等。幂等`|
|`2`|`POST`|`和get一样很常见，向服务器提交资源让服务器处理，比如提交表单、上传文件等，可能导致建立新的资源或者对原有资源的修改。提交的资源放在请求体中。不支持快取。非幂等`|
|`3`|`HEAD`|`本质和get一样，但是响应中没有呈现数据，而是http的头信息，主要用来检查资源或超链接的有效性或是否可以可达、检查网页是否被串改或更新，获取头信息等，特别适用在有限的速度和带宽下。`|
|`4`|`PUT`|`和post类似，html表单不支持，发送资源与服务器，并存储在服务器指定位置，要求客户端事先知道该位置；比如post是在一个集合上（/province），而put是具体某一个资源上（/province/123）。所以put是安全的，无论请求多少次，都是在123上更改，而post可能请求几次创建了几次资源。幂等`|
|`5`|`DELETE`|`请求服务器删除某资源。和put都具有破坏性，可能被防火墙拦截。如果是https协议，则无需担心。幂等`|
|`6`|`CONNECT`|`HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。就是把服务器作为跳板，去访问其他网页然后把数据返回回来，连接成功后，就可以正常的get、post了。`|
|`7`|`OPTIONS`|`获取http服务器支持的http请求方法，允许客户端查看服务器的性能，比如ajax跨域时的预检等。`|
|`8`|`TRACE`|`回显服务器收到的请求，主要用于测试或诊断。一般禁用，防止被恶意攻击或盗取信息。`|


#####  [3.XMLHttpRequest2](#top) <b id="target3"></b>
`鉴于 XHR 已经得到广泛接受，成为了事实标准，W3C 也着手制定相应的标准以规范其行为。 XMLHttpRequest 1级只是把已有的 XHR对象的实现细节描述了出来。而 XMLHttpRequest 2级则进一步 发展了 XHR。`

##### :octocat: 1.FormData 
`现代 Web 应用中频繁使用的一项功能就是表单数据的序列化，XMLHttpRequest 2 级为此定义了 FormData 类型。FormData 为序列化表单以及创建与表单格式相同的数据（用于通过 XHR传输）提供 了便利。下面的代码创建了一个 FormData 对象，并向其中添加了一些数据。 `

```node
var data = new FormData(); 
data.append("name", "Nicholas"); 
```
`这个 append()方法接收两个参数：键和值，分别对应表单字段的名字和字段中包含的值`































  
##### [?.XMLHttpRequest2 对象](#top) <b id="XMLHttpRequest"></b>

```javascript
一、初探浏览器原生Ajax接口
    1、创建XHR对象
       我们整个Ajax技术的接口，核心是通过XMLHttpRequest这个对象（简称XHR对象）来完成！
        XHR对象怎么得到？
            对于IE7+，谷歌，火狐，是通过XMLHttpRequest这个引用类型来创建出XHR对象!
            var xhr=new XMLHttpRequest();//得到XHR对象，变量名随便取
        对于IE7以下的ie浏览器创建出XHR对象的方法我们这边不做介绍，一个原因是jQuery后面为Ajax操作
        提供了兼容各浏览器的方法，另一个原因是IE7以下的极地版本的IE浏览器，使用率已经非常低！
```

#####  XMLHttpRequest对象
`不考虑IE7以下的兼容问题,不管直接使用XMLHttpRequest 对象`

```javascript
      var xhr=new XMLHttpRequest();
      xhr.open("GET","web/a.php",true);
      xhr.send(null);
      xhr.onreadystatechange=function(){
          if(xhr.readyState==4){
              console.info(xhr.status);
              console.info(xhr.statusText);
              console.log(xhr.responseText);
          }
      }
```

```php
  // a.php
  <?php
      echo "<div>来吧小伙子,了解一下Ajax</div>"
  ?>
```

##### 启动请求
`使用open(get/post,url,async= true/false),但是真正发送请求要使用send方法,get方式 使用send(null) 如果是post方法  使用send(urldata),返回的数据会自动填充到对象的属性中`

```javascript    
    2、启用一个请求等待发送
        XHR对象.open('HTTP方法','请求的地址',是否异步发送请求的布尔值,用户名,密码);
        调用open()方法并不会真正发送请求，而只是启动一个请求以备发送
        1）'HTTP方法'
            比如get或者post	大小写无所谓
            GET请求应该用于从服务器端获取数据为目的
            POST方式主要是为了将数据提交给服务器端并且存储（比如将数据提交到服务器端，且写入数据库）！
        2）请求的url地址
           如果该地址指向的是静态内容（比如html等）会直接将指定文件里面的内容返回给客户端！
           如果是动态的脚本（比如PHP）那么会在服务器端执行这个PHP文件，并且将其输出的内容返回给客户端！
        3）是否异步发送请求的布尔值	
           true表示异步请求（默认），false表示同步请求
           同步请求的特点
                客户端提交请求->等待服务器处理（等待服务器端回应之前客户端不能做其他事情）->服务器端处理完毕返回给
                浏览器即：浏览器在得到服务器端的数据回应之前，浏览器不能和用户有任何交互，后面的JavaScript 代码会
                等到服务器响应之后再继续执行！
           异步请求的特点
                客户端提交请求（请求之后，可以继续去做其他事情）
                    ->服务器进行处理（处理过程就像在后台完成一样，用户可能都感觉不到，还可以继续浏览网页中的内容并
                    且交互，后面的JavaScript 代码也会继续执行而不会等待异步请求的响应！）
                处理完毕（处理完毕之后，浏览器会得到这个通知，然后可以执行对应的一些准备好的操作）
                优点：在异步请求的过程中用户依然可以操作我们的页面内容，后面的js代码也会继续执行，而无需等待发起的
                请求的响应！
           把客户端（浏览器）和服务器端比喻两个人，浏览器我们称作A，服务器端称作B
           同步请求：
                A到餐厅吃饭，点了一桌菜，约B来一起吃饭，约完之后，A就坐在餐厅一直等待，直到B到了，坐了下来，两人一
                起吃！
           异步请求：
                A到餐厅吃饭，点了一桌菜，约B来一起吃饭，约完之后，A就直接拿起筷子吃了，边吃边等，B来了之后，拿起
                筷子吃！
        4）最后两个参数是 服务器如果设置了需要认证请求，写上您在服务器端设置的用户名和密码（一般很少使用）
    3、发送请求
        XHR对象.send(要发送的数据);
        传入的参数作为主体内容发送给服务器！
        如果不需要传主体数据，必须传入null，否则有些浏览器无法兼容！
```  

```javascript
     //post请求
    <script>        
        function encodeU(data) {
            var str='';
            for (var i in data) {
                str+=encodeURIComponent(i)+'='+encodeURIComponent(data[i])+'&';
            }
            return str.substr(0,str.length-1);
        }
        window.onload=function(){
            var xhr=new XMLHttpRequest();
            var data=encodeU({
                UserPwd:"152123595",
                UserID:"858585",
                UserName:"王俊凯"
            });   
            xhr.open("POST","postdata.php",true);
            xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
            xhr.send(data);
            xhr.onreadystatechange=function(){
                if(xhr.readyState==4){
                    console.info(xhr.status);
                    console.info(xhr.statusText);
                    console.log(xhr.responseText);
                }
            }
        }
    </script>
```


##### 获得数据

```javascript   
    4、如何获取服务器响应的数据
       在收到服务器端响应后，响应的数据会自动填充XHR对象的属性，部分属性和方法简介如下
       1）responseText：作为响应主体被返回的文本
       2）responseXML：如果响应的内容是XML格式的，就根据内容创建XML DOM对象以便操作（XML格式已经很少使用，所以这个属
	               性现在也很少使用了）
       3）status：响应的HTTP状态（200表示成功，404表示未找到）
       4）statusText：HTTP状态的说明
```

* 5）readyState：请求/响应过程的当前活动阶段，可能的值
  * 0：`未初始化。尚未调用open()方法`
  * 1：`启动。已经调用open()方法，但尚未调用send()方法`
  * 2：`发送。已经调用send()方法，但尚未接收到响应`
  * 3：`接收。已经接收到部分响应数据`
  * 4：`完成。已经接收到全部响应数据，而且已经可以在客户端使用了`
  
* 6）readystatechange事件:
  `只要readyState属性的值由一个值变成另一个值，都会触发一次readystatechange事件`  
  `注：必须在调用open()之前指定readystatechange事件处理程序才能确保跨浏览器兼容性。
       对于readystatechange事件处理程序内部最好不要使用this来代表当前的xhr对象，因为有的浏览器会出现不可
       预知的问题，所以我们在处理程序内部就用XHR对象的变量名就好了。`
##### 7）abort()
	  取消当前正在执行的请求


```javascript
    5、设置http头部信息
	我们在浏览器与服务器之间来回的传递数据，这些数据除了我们传递的主体数据之外，还会动加上一些http头信息
	http头部信息
	    为什么要传头部信息？主要就是告诉对方自己的能力，告诉对方主体内容的类型格式！
	    1）请求的头部信息(由浏览器发送给服务器端的http头部信息)
		不同浏览器实际发送的头部信息会有所不同，下面列出的是常见的请求头部信息
		Accept：浏览器能够处理的内容类型
		Accept-Encoding：浏览器能够处理的压缩编码
		Accept-Language：浏览器当前设置的语言
		Accept-Charset：浏览器能够显示的字符集
		Host：发出请求的页面所在的域
		Referer：发出请求的页面的URI
		User-Agent：浏览器的用户代理字符串，通过它我们在服务器端可以大致知道客户端是用的什么类型的浏览器
		设置本次请求的头信息：
                 XHR对象.setRequestHeader()
                 这个方法接受两个参数：头部字段的名称和头部字段的值（也可以发送自定义的头信息）
                 注：要成功发送请求头部信息，必须在调用open()方法之后且调用send()方法之前调用
                 setRequestHeader()
	    2）响应头部信息（由服务器端发送给客户端的http头部信息）
		XHR对象.getResponseHeader()
		    传入头部字段名称，可以取得相应的响应头部信息
		getAllResponseHeaders()
		    包含所有头部信息的长字符串
	    
    6、向服务器发送数据
	1）GET方式的请求
	    请求的url地址?参数1&参数2&参数3&.........
			  每个参数的格式：参数名称=参数值
			  符合?后面的格式的这个字符串，我们可以称作 查询字符串！
	    注：查询字符串中每个参数的名称和值都必须使用encodeURIComponent()进行编码，否则可能会出现乱码的情况！
	2）对于post请求，需将要传的数据传入send方法，并且符合相应的格式（这个符合格式的字符串我们称作查询字符串）
           比如：username=%E5%AD%99%E8%83%9C%E5%88%A9&sex=true，传入send方法！
                     和查询字符串一样，多个参数用&隔开，每个参数的名称和值也经过了encodeURIComponent()进行编码！
           注：我们使用ajax技术发起POST请求并且传递给服务器数据时，需要手动的做一些设置，服务器端才能顺利的接受到
                  数据！将请求的Content-Type头部信息设置为application/x-www-form-urlencoded
                  请求的Content-Type头部信息，表示告诉服务器发送的主体数据的内容类型（我们用post方式提交表单时默认
                  就是采用这个内容类型）！只有这样明确的告诉服务器端我们发送的主体数据的内容类型，服务器端才能合理的
                  识别解析它们，且放入$_POST变量中！
                  application/x-www-form-urlencoded这种内容类型形式上是这样子的：
                  username=%E5%AD%99%E8%83%9C%E5%88%A9&sex=true
           注：直接使用表单提交时这些操作，浏览器都帮我们自动做了，所以我们感觉不到！
    7、处理服务器响应的数据
	服务器端可以返回各种各样格式的数据，不同的数据格式，处理起来也是各不相同！
	首先我们需要知道一点：
	    服务器端响应数据，存放在XHR对象内的responseText这个属性里面，这个属性的数据类型是字符串！
	    但是我们一个字符串里面的数据格式可以有许多种不同的形式（数据格式）
	1）html格式的数据
	    html格式的数据即返回的数据本身就是html代码，这种数据使用起来比较直接，可以直接将其通过append之类
      的方法放到当前页面中   
	2）script格式的数据
	    字符串内直接就是js代码
	    即返回的数据直接就是javascript代码，可以使用js的eval函数来执行这个字符串中的代码
	3）xml格式的数据
	    是Ajax技术刚被提出是最常用的服务器端与客户端交互的数据格式，甚至ajax中的x指的就是xml，但是这种格式的
      数据书写麻烦，
            使用不便，目前已经很少使用这个格式的数据进行交互。
	4）json格式的数据
	    JSON格式的数据形式：
	    形式一：{"键名":值,...}
	    形式二：[{"键名":值},...]
	    JSON数据格式非常严格，主要的注意点：
		键名称必须是用双引号括起来的字符串
		值如果是字符串类型，那么也必须使用双引号括起来而不是单引号！
		对于JSON格式的字符串，我们可以通过JSON.parse()方法将其转换为对象
		    注意：字符串必须符合JSON格式！
		    如果浏览器不支持JSON.parse这个方法可以使用eval函数来投机取巧一下！
	    注意：JSON格式的数据，大家要养成良好的习惯，将其写的标准！
    8、跨源请求
	默认情况下，浏览器只允许向与本页面同源的服务器发起请求
	JSONP（JSON with padding简写，参数式JSON的意思）技术
	跨源 请求的目的：从不同的url下面的脚本请求数据！
	在不同源的php文件那边将JSON格式的数据作为参数传给要调用的函数！
	    注意：只能是get方式
	<script type="text/javascript">
	    function sunshengli(data){
		console.log(data);
	    }
	</script>
	<script type="text/javascript" src="http://127.0.0.1/ajax/demo9_4/test/c.php?callback=sun
  shengli"></script>
 ``` 
 ##### <a id="XMLHttpRequest2" href="#XMLHttpRequest2">XMLHttpRequest2 级</a>
 
 ##### 数据序列化 FromData <a href="#top">置顶</a>
 
 ```Javascript
       window.onload=function(){
          var xhr=new XMLHttpRequest();  
          xhr.open("POST","postdata.php",true);
          var data_fromdata=new FormData();
          data_fromdata.append("UserPwd","152123595psd");
          data_fromdata.append("UserID","858585");
          data_fromdata.append("UserName","王俊凯");    
          xhr.send(data_fromdata);
          xhr.onreadystatechange=function(){
              if(xhr.readyState==4){
                  console.info(xhr.status); //Http状态码
                  console.info(xhr.statusText);
                  console.log(xhr.responseText);
              }
          }
      }
 ```
##### OverrideMineType 设置返回数据的类型
`Serve返回的MIME类型是text/plain,但数据中实际包含的是XML,根据MIME类型,即使数据是XML，response属性中任然是null,通过此方法可以保证报相应
当做XML而非存文本来处理`

`可能没什么用`
```javascript
    window.onload=function(){
        var xhr=new XMLHttpRequest();  
        xhr.open("GET","suser.xml",true);
        xhr.overrideMimeType("text/xml");
        xhr.setRequestHeader("Content-Type","text/xml");
        xhr.send(null);
        xhr.onreadystatechange=function(){
            if(xhr.readyState==4){
               var xmlper=xhr.responseText;
               console.log(xmlper);
            }
        }
    }
```
