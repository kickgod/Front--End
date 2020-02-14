### <a id="top" href="#top"> Javascript 原生 Ajax :grey_exclamation:</a>
`Asynchronous JavaScript + XML 这一技术能够向服务器请求额外的数据而无须卸载页面,会带来更好的用户体验,`:speech_balloon:

-----

- [x] [`1.XMLHttpRequest 对象`](#target1)
- [x] [`2.HTTP头部信息 `](#target2)
- [x] [`3.XMLHttpRequest2 对象 `](#target3)
   - [ ] `1.FormData 对象`
   - [ ] `2.overrideMimeType()`
   - [ ] `3.进度事件`
   - [ ] `4.load事件`  
   - [ ] `5.progress事件 ` 
- [x] [`4.跨源资源共享`](#target4)
- [x] [`5.Comet`](#target5)
- [x] [`6.Http Web Sockets `](https://developer.mozilla.org/zh-CN/docs/Glossary/WebSockets)

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

##### :octocat: 2.3 HTTP支持的八种方法 

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

`而通过向 FormData 构造函数中传入表单元素，也可以用表单元素的数据 预先向其中填入键值对儿`
```node
var data = new FormData(document.forms[0]); 
```
`创建了 FormData 的实例后，可以将它直接传给 XHR的 send()方法，如下所示： `

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

xhr.open("post", "postexample.php", true);
var form = document.getElementById("user-info");
xhr.send(new FormData(form));
```
`使用 FormData 的方便之处体现在不必明确地在 XHR 对象上设置请求头部。XHR对象能够识别传 入的数据类型是 FormData 的实例，并配置适当的头部信息。 `

##### :octocat: 2.超时设定 
`IE8 为 XHR 对象添加了一个 timeout 属性，表示请求在等待响应多少毫秒之后就终止。在给 timeout 设置一个数值后，如果在规定的时间内浏览器还没有接收到响应，那么就会触发 timeout 事 件，进而会调用 ontimeout 事件处理程序。这项功能后来也被收入了 XMLHttpRequest 2级规范中。`

```node
var xhr = XMLHttpRequest();
xhr.onreadystatechange = function () {
    if (xhr.readyState == 4) {
        try {
            if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
                alert(xhr.responseText);
            } else {
                alert("Request was unsuccessful: " + xhr.status);
            }
        } catch (ex) {             /*假设由 ontimeout 事件处理程序处理 */
        }
    }
};


xhr.open("get", "timeout.php", true);
xhr.timeout = 1000;  /*将超时设置为 1 秒钟（仅适用于 IE8+） */
xhr.ontimeout = function () {
    alert("Request did not return in a second.");
};
xhr.send(null); 
```

`这个例子示范了如何使用 timeout 属性。将这个属性设置为 1000毫秒，意味着如果请求在 1秒钟 内还没有返回，就会自动终止。请求终止时，会调用 ontimeout 事件处理程序。但此时 readyState 可能已经改变为 4了，这意味着会调用 onreadystatechange 事件处理程序。可是，如果在超时终止 请求之后再访问 status 属性，就会导致错误。为避免浏览器报告错误，可以将检查 status 属性的语句封装在一个 try-catch 语句当中。 `

##### :octocat: 3.overrideMimeType()
`Firefox早引入了 overrideMimeType()方法，用于重写 XHR响应的 MIME 类型。这个方法后 来也被纳入了 XMLHttpRequest 2级规范。因为返回响应的 MIME类型决定了 XHR对象如何处理它，所 以提供一种方法能够重写服务器返回的 MIME类型是很有用的`

`比如，服务器返回的 MIME类型是 text/plain，但数据中实际包含的是 XML。根据 MIME类型， 即使数据是 XML，responseXML 属性中仍然是 null。通过调用 overrideMimeType()方法，可以保 证把响应当作 XML而非纯文本来处理。 `

```node
var xhr = XMLHttpRequest(); 
xhr.open("get", "text.php", true); 
xhr.overrideMimeType("text/xml"); 
xhr.send(null); 
```

`这个例子强迫 XHR对象将响应当作 XML而非纯文本来处理。调用 overrideMimeType()必须在 send()方法之前，才能保证重写响应的 MIME类型`


##### :octocat: 4.进度事件 
`Progress Events规范是 W3C的一个工作草案，定义了与客户端服务器通信有关的事件。这些事件 早其实只针对 XHR操作，但目前也被其他 API借鉴。有以下 6个进度事件。` 
* `loadstart`：`在接收到响应数据的第一个字节时触发。` 
* `progress`：`在接收响应期间持续不断地触发。` 
* `error`：`在请求发生错误时触发。` 
* `abort`：`在因为调用 abort()方法而终止连接时触发。` 
* `load`：`在接收到完整的响应数据时触发。` 
* `loadend`：`在通信完成或者触发 error、abort 或 load 事件后触发。 每个请求都从触发 loadstart 事件开始，接下来是一或多个 progress 事件，然后触发 error、 abort 或 load 事件中的一个，后以触发 loadend 事件结束。`

#####  :octocat: 5.load事件 
`Firefox在实现 XHR 对象的某个版本时，曾致力于简化异步交互模型。终，Firefox实现中引入了 load 事件，用以替代 readystatechange 事件。响应接收完毕后将触发 load 事件，因此也就没有必 要去检查 readyState 属性了。而 onload 事件处理程序会接收到一个 event 对象，其 target 属性 就指向 XHR对象实例，因而可以访问到 XHR对象的所有方法和属性。然而，并非所有浏览器都兼容此API`

```node
var xhr = createXHR();
xhr.onload = function () {
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
        alert(xhr.responseText);
    } else {
        alert("Request was unsuccessful: " + xhr.status);
    }
};
xhr.open("get", "altevents.php", true);
xhr.send(null);
```

##### :octocat: 6.progress事件 
`Mozilla对 XHR的另一个革新是添加了 progress 事件，这个事件会在浏览器接收新数据期间周期 性地触发。而 onprogress 事件处理程序会接收到一个 event 对象，其 target 属性是 XHR 对象，但 包含着三个额外的属性：lengthComputable、position 和 totalSize 有了这些信息，我们就可以为用户创建一个进度指示器 了`
 
* `lengthComputable`:`是一个表示进度信息是否可用的布尔值`
* `position`:`表示已经接收的字节数`
* `totalSize`:`表示根据 Content-Length 响应头部确定的预期字节数。`

```node
xhr.onload = function (event) {
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
        alert(xhr.responseText);
    } else {
        alert("Request was unsuccessful: " + xhr.status);
    }
};
xhr.onprogress = function (event) {
    var divStatus = document.getElementById("status");
    if (event.lengthComputable) {
        divStatus.innerHTML = "Received " + event.position + " of " + event.totalSize + " bytes";
    }
};

xhr.open("get", "altevents.php", true);
xhr.send(null);
```

`为确保正常执行，必须在调用 open()方法之前添加 onprogress 事件处理程序`

#####  [4.跨源资源共享](#top) <b id="target4"></b> 
`通过 XHR 实现 Ajax 通信的一个主要限制，来源于跨域安全策略。默认情况下，XHR 对象只能访 问与包含它的页面位于同一个域中的资源。这种安全策略可以预防某些恶意行为。但是，实现合理的跨 域请求对开发某些浏览器应用程序也是至关重要的。 `

`CORS（Cross-Origin Resource Sharing，跨源资源共享）是 W3C的一个工作草案，定义了在必须访 问跨源资源时，浏览器与服务器应该如何沟通。CORS背后的基本思想，就是使用自定义的 HTTP头部 让浏览器与服务器进行沟通，从而决定请求或响应是应该成功，还是应该失败。 `

`比如一个简单的使用 GET 或 POST 发送的请求，它没有自定义的头部，而主体内容是 text/plain。在 发送该请求时，需要给它附加一个额外的 Origin 头部，其中包含请求页面的源信息（协议、域名和端 口），以便服务器根据这个头部信息来决定是否给予响应。下面是 Origin 头部的一个示例： `
 
`Origin: http://www.nczonline.net`

`如果服务器认为这个请求可以接受，就在 Access-Control-Allow-Origin 头部中回发相同的源 信息（如果是公共资源，可以回发"*"）。例如： `
 
`Access-Control-Allow-Origin: http://www.nczonline.net` 
 
`如果没有这个头部，或者有这个头部但源信息不匹配，浏览器就会驳回请求。正常情况下，浏览器 会处理请求。注意，请求和响应都不包含 cookie信息。 ` 


##### :octocat: 1.IE对CORS的实现 
`微软在 IE8中引入了 XDR（XDomainRequest）类型。这个对象与 XHR类似，但能实现安全可靠 的跨域通信。XDR对象的安全机制部分实现了 W3C的 CORS规范。`
`以下是 XDR与 XHR的一些不同之 处。`
* `cookie不会随请求发送，也不会随响应返回。` 
* `只能设置请求头部信息中的 Content-Type 字段。` 
* `不能访问响应头部信息。` 
* `只支持 GET 和 POST 请求。 `

`这些变化使 CSRF（Cross-Site Request Forgery，跨站点请求伪造）和 XSS（Cross-Site Scripting，跨 站点脚本）的问题得到了缓解` `被请求的资源可以根据它认为合适的任意数据（用户代理、来源页面等） 来决定是否设置 Access-Control- Allow-Origin 头部。作为请求的一部分，Origin 头部的值表示 请求的来源域，以便远程资源明确地识别 XDR请求。 `

`本人已经放弃IE 所以不解释了`

##### :octocat: 2.其他浏览器对CORS的实现 
`通过 XMLHttpRequest 对象实现了对 CORS 的原生支持。 在尝试打开不同来源的资源时，无需额外编写代码就可以触发这个行 为。要请求位于另一个域中的资源，使用标准的XHR对象并在 open()方法中传入绝对URL即可`

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
xhr.open("get", "http://www.somewhere-else.com/page/", true);
xhr.send(null);
```

`与 IE中的 XDR对象不同，通过跨域 XHR对象可以访问 status 和 statusText 属性，而且还支 持同步请求。跨域 XHR对象也有一些限制，但为了安全这些限制是必需的。以下就是这些限制。 `

* `不能使用 setRequestHeader()设置自定义头部。` 
* `不能发送和接收 cookie。` 
* `调用 getAllResponseHeaders()方法总会返回空字符串。`

`由于无论同源请求还是跨源请求都使用相同的接口，因此对于本地资源，好使用相对 URL，在访 问远程资源时再使用绝对 URL。这样做能消除歧义，避免出现限制访问头部或本地 cookie信息等问题`


##### :octocat: 3.Preflighted Reqeusts 
`CORS通过一种叫做 Preflighted Requests的透明服务器验证机制支持开发人员使用自定义的头部、 GET或 POST之外的方法，以及不同类型的主体内容。在使用下列高级选项来发送请求时，就会向服务 器发送一个 Preflight请求。这种请求使用 OPTIONS方法，发送下列头部。 `

* `Origin`：`与简单的请求相同。` 
* `Access-Control-Request-Method`：`请求自身使用的方法。` 
* `Access-Control-Request-Headers`：`（可选）自定义的头部信息，多个头部以逗号分隔。 以下是一个带有自定义头部 NCZ的使用 POST 方法发送的请求。 `

```node
//以下是一个带有自定义头部 NCZ的使用 POST 方法发送的请求。 
Origin: http://www.nczonline.net 
Access-Control-Request-Method: POST 
Access-Control-Request-Headers: NCZ
```

`发送这个请求后，服务器可以决定是否允许这种类型的请求。服务器通过在响应中发送如下头部与 浏览器进行沟通`

* `Access-Control-Allow-Origin`：`与简单的请求相同。` 
* `Access-Control-Allow-Methods`：`允许的方法，多个方法以逗号分隔。` 
* `Access-Control-Allow-Headers`：`允许的头部，多个头部以逗号分隔。` 
* `Access-Control-Max-Age`：`应该将这个 Preflight请求缓存多长时间（以秒表示）。` 


`例如：` 
```node
Access-Control-Allow-Origin: http://www.nczonline.net 
Access-Control-Allow-Methods: POST, GET 
Access-Control-Allow-Headers: NCZ 
Access-Control-Max-Age: 1728000 
```
`Preflight 请求结束后，结果将按照响应中指定的时间缓存起来。而为此付出的代价只是第一次发送 这种请求时会多一次 HTTP请求 ,支持 Preflight请求的浏览器包括 Firefox 3.5+、Safari 4+和 Chrome。IE 10及更早版本都不支持。 `

##### :octocat: 4.带凭据的请求 
`默认情况下，跨源请求不提供凭据（cookie、HTTP 认证及客户端 SSL 证明等）。通过将 withCredentials 属性设置为 true，可以指定某个请求应该发送凭据。如果服务器接受带凭据的请 求，会用下面的 HTTP头部来响应。 `

```node
Access-Control-Allow-Credentials: true 
```

`如果发送的是带凭据的请求，但服务器的响应中没有包含这个头部，那么浏览器就不会把响应交给 JavaScript（于是，responseText 中将是空字符串，status 的值为 0，而且会调用 onerror()事件处 理程序）。另外，服务器还可以在 Preflight响应中发送这个 HTTP头部，表示允许源发送带凭据的请求。支持 withCredentials 属性的浏览器有 Firefox 3.5+、Safari 4+和 Chrome。IE 10及更早版本都不 支持。`

##### :octocat: 5.跨浏览器的CORS 
`即使浏览器对 CORS的支持程度并不都一样，但所有浏览器都支持简单的（非 Preflight和不带凭据 的）请求，因此有必要实现一个跨浏览器的方案。检测 XHR是否支持 CORS的简单方式，就是检查 是否存在 withCredentials 属性。再结合检测 XDomainRequest 对象是否存在，就可以兼顾所有浏览器了。 `

```node
function createCORSRequest(method, url) {
    var xhr = new XMLHttpRequest();
    if ("withCredentials" in xhr) {
        xhr.open(method, url, true);
    } else if (typeof XDomainRequest != "undefined") {
        vxhr = new XDomainRequest();
        xhr.open(method, url);
    } else {
        xhr = null;
    }
    return xhr;
}

var request = createCORSRequest("get", "http://www.somewhere-else.com/page/");
if (request) {
    request.onload = function () {         /*对 request.responseText 进行处理*/
    };
    request.send();
} 
```

`Firefox、Safari和 Chrome中的 XMLHttpRequest 对象与 IE中的 XDomainRequest 对象类似，都 提供了够用的接口，因此以上模式还是相当有用的。这两个对象共同的属性/方法如下。` 

* `abort()`：`用于停止正在进行的请求。` 
* `onerror`：`用于替代 onreadystatechange 检测错误。` 
* `onload`：`用于替代 onreadystatechange 检测成功。` 
* `responseText`：`用于取得响应内容。` 
* `send()`：`用于发送请求。` 

`以上成员都包含在 createCORSRequest()函数返回的对象中，在所有浏览器中都能正常使用。 `

##### :octocat: 6.图像Ping   
`我们知道，一个网页可以从任何网页中加载图像，不 用担心跨域不跨域。这也是在线广告跟踪浏览量的主要方式` `也可以动态地创 建图像，使用它们的 onload 和 onerror 事件处理程序来确定是否接收到了响应。`

`动态创建图像经常用于图像 Ping。图像 Ping 是与服务器进行简单、单向的跨域通信的一种方式。 请求的数据是通过查询字符串形式发送的，而响应可以是任意内容，但通常是像素图或 204响应。通过 图像 Ping，浏览器得不到任何具体的数据，但通过侦听 load 和 error 事件，它能知道响应是什么时 候接收到的`

```node
var img = new Image();
img.onload = img.onerror = function () {
    alert("Done!");
};
img.src = "http://www.example.com/test?name=Nicholas"; 
```

`这里创建了一个 Image 的实例，然后将 onload 和 onerror 事件处理程序指定为同一个函数。这 样无论是什么响应，只要请求完成，就能得到通知。请求从设置 src 属性那一刻开始，而这个例子在请 求中发送了一个 name 参数。`

`图像 Ping常用于跟踪用户点击页面或动态广告曝光次数。图像 Ping有两个主要的缺点，一是只 能发送 GET 请求，二是无法访问服务器的响应文本。因此，图像 Ping只能用于浏览器与服务器间的单向通信。`

#####  [5.Comet](#top) <b id="target5"></b>  
`Comet是 Alex Russell①发明的一个词儿，指的是一种更高级的 Ajax技术（经常也有人称为“服务器 推送”）。Ajax 是一种从页面向服务器请求数据的技术，而 Comet 则是一种服务器向页面推送数据的技 术。Comet能够让信息近乎实时地被推送到页面上，非常适合处理体育比赛的分数和股票报价。 `

`实现Comet有两种方式：长轮询与http流`

`而长轮询的方式是，页面向服务器发起一个请求，服务器一直保持tcp连接打开，知道有数据可发送。发送完数据后，页面关闭该连接，随即又发起一个新的服务器请求，在这一过程中循环。`

`短轮询和长轮询的区别是：短轮询中服务器对请求立即响应，而长轮询中服务器等待新的数据到来才响应，因此实现了服务器向页面推送实时，并减少了页面的请求次数。`

`http流不同于上述两种轮询，因为它在页面整个生命周期内只使用一个HTTP连接，具体使用方法即页面向浏览器发送一个请求，而服务器保持tcp连接打开，然后不断向浏览器发送数据。`

`以下为客户端实现长轮询的方法：`
```node
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function(){
   if(xhr.readyState == 4){
      result = xhr.responseText
      console.log(result);
      xhr.open('get', 'test2.php');	//在获得数据后重新向服务器发起请求
      xhr.send(null);
   }
}
xhr.open('get', 'test2.php');
xhr.send(null);
```
`以下为客户端实现http流的方法：`
```node
var xhr = new XMLHttpRequest();
received = 0;	//最新消息在响应消息的位置
xhr.onreadystatechange = function(){
   if(xhr.readyState == 3){
      result = xhr.responseText.substring(received);
      console.log(result);
      received += result.length;
   }else if(xhr.readyState == 4){
      console.log("完成消息推送");
   }
}
xhr.open('get', 'test1.php');
xhr.send(null);
```
`以下为服务器端实时推送的实现方法，以php为例：`
```php
<?php
	ini_set('max_execution_time', 10);
	error_reporting(0);
	$i = 0;
	while(true){	//不断推送消息
		echo "Number is $i\n";
		ob_flush();	//将php缓存冲出
		flush();	//从php缓存中输出到tcp缓存
		$i++;
		sleep(1);	
	}
?>
/*浏览器结果*/
Number is 0
Number is 1
Number is 2
Number is 3
...
完成消息推送
```
#####  服务器发送事件 
`另外大多数浏览器实现了SSE(Server-Sent Events,服务器发送事件) API，SSE支持短轮询、长轮询和HTTP流，使用方式如下：` `SSE API 用于创建到服务器的单向连接，服务器通过这个连接可以发送任意数量的数据。服务器响应的 MIME 类型必须是 text/event-stream`

`SSE的 JavaScript API与其他传递消息的 JavaScript API很相似。要预订新的事件流，首先要创建一 个新的 EventSource 对象，并传进一个入口点： `
 ```javascript
var source = new EventSource("myevents.php");
```

`注意，传入的 URL必须与创建对象的页面同源（相同的 URL模式、域及端口）。EventSource 的 实例有一个 readyState 属性，值为 0表示正连接到服务器，值为 1表示打开了连接，值为 2表示关闭 了连接。 另外，还有以下三个事件。` 

* `open`：`在建立连接时触发。` 
* `message`：`在从服务器接收到新事件时触发。` 
* `error`：`在无法建立连接时触发。 就一般的用法而言，onmessage 事件处理程序也没有什么特别的。 `
 
```javascript
source.onmessage = function(event){    
 var data = event.data;   
 //处理数据
}; 
``` 
`服务器发回的数据以字符串形式保存在 event.data 中。 `
 
`默认情况下，EventSource 对象会保持与服务器的活动连接。如果连接断开，还会重新连接。这 就意味着 SSE适合长轮询和 HTTP流。如果想强制立即断开连接并且不再重新连接，可以调用 close() 方法。 `

```javascript
source.close(); 
```

`所谓的服务器事件会通过一个持久的 HTTP 响应发送，这个响应的 MIME 类型为 text/event-stream。响应的格式是纯文本，简单的情况是每个数据项都带有前缀 data:，例如： `

```node
data: foo 

data: bar 

data: foo 
data: bar 
```

`对以上响应而言，事件流中的第一个 message 事件返回的 event.data 值为"foo"，第二个 message 事件返回的 event.data 值为"bar"，第三个 message 事件返回的 event.data 值为 "foo\nbar"（注意中间的换行符）。对于多个连续的以 data:开头的数据行，将作为多段数据解析， 每个值之间以一个换行符分隔。只有在包含 data:的数据行后面有空行时，才会触发 message 事件， 因此在服务器上生成事件流时不能忘了多添加这一行。 `

`通过 id:前缀可以给特定的事件指定一个关联的 ID，这个 ID行位于 data:行前面或后面皆可： `
 
```node
data: foo
id: 1 
```
`设置了 ID后，EventSource 对象会跟踪上一次触发的事件。如果连接断开，会向服务器发送一个 包含名为 Last-Event-ID 的特殊 HTTP头部的请求，以便服务器知道下一次该触发哪个事件。在多次 连接的事件流中，这种机制可以确保浏览器以正确的顺序收到连接的数据段。 `
