### <a id="top"  href="#top"> JavaScript数据存储 </a>  :grey_exclamation: 
----

- [ ] [`Cookie`](#CookieInfo)
  - [x] <a href="#CookieInfo">`Cookie 信息`</a>
  - [x] <a href="#SubCookie">`Sub Cookie`</a>
  - [x] <a href="#NoticeCookie">`Cookie 注意`</a>
- [ ] [`本地存储`](#localStorage)
  - [x] <a href="#localStorage">`localStorage`</a>
  - [x] <a href="#sessionStorage">`sessionStorage`</a>
- [ ] [`区别`](#distinction)
 
##### [1.Cookie 信息](#top) <span id="CookieInfo"></span>
`用于在客户端存储信息`
* `Cookie的构成`
  * `名称`:`key ,cookie不区分大小写,但是实践中最好当做区分大小写,因为某些服务会这样处理,Cookie的名称必须进过URL编码的。`
  * `值`:`存储信息`
  * `域`:`所有想改域发送的请求都会包含这个cookie信息`
  * `路径`:`指定域的那个路径,可以向服务器发送Cookie，例如可以指定只有在http://www.wangzhe.com/book/ 才能访问,那么http://www.wangzhe.com 就
          不能发送cookie信息,即使请求来着同一个域`
  * `失效时间`:`什么时候浏览器删除掉的时间戳, 这个值是GMT时间格式 Wdy,DD-Mon-YYYY HH:MM:SS`    
     
     * 1.`GMT转普通格式`
     
     ```javascript
       GMTToStr(time){
            let date = new Date(time)
            let Str=date.getFullYear() + '-' +
            (date.getMonth() + 1) + '-' + 
            date.getDate() + ' ' + 
            date.getHours() + ':' + 
            date.getMinutes() + ':' + 
            date.getSeconds()
            return Str
        }
     ```
     * 2.`普通格式转GMT`
     
     ```javascript
       StrToGMT(time){
            let GMT = new Date(time)
            return GMT
        }
     ```
     
     
     
  * `安全标志`:`指定后,cookie只有在使用SSL链接的时候才能发送到服务器`
  <br/>
  
  ```http
    HTTP/1.1 200 OK
    Content-type: text/html
    Set-Cookie: name=value; expires=Mon， 22-Jan-07 07:10:24 GMT; domin=.www.com
    Other-header: other-header-value
  ```
* `设置Cookie 使用Cookie `
  ```javascript
      //设置一个cookie键值对: "name":"JiangXing"
      document.cookie=encodeURIComponent("name")+"="+encodeURIComponent("JiangXing");
  ```
  
* `Cookie 操作对象` <a href="#top">置顶</a>
  
  ```javascript
    var CookieUtil = {
          get: function (name){
              var cookieName = encodeURIComponent(name) + "=",
                  cookieStart = document.cookie.indexOf(cookieName),
                  cookieValue = null,
                  cookieEnd;     
              if (cookieStart > -1){
                  cookieEnd = document.cookie.indexOf(";", cookieStart);
                  if (cookieEnd == -1){
                      cookieEnd = document.cookie.length;
                  }
                  cookieValue = decodeURIComponent(document.cookie.substring(cookieStart + 
                  cookieName.length, cookieEnd));
              } 
              return cookieValue;
          },

          set: function (name, value, expires, path, domain, secure) {
              var cookieText = encodeURIComponent(name) + "=" + encodeURIComponent(value);
              if (expires instanceof Date) {
                  cookieText += "; expires=" + expires.toGMTString();
              }
              if (path) {
                  cookieText += "; path=" + path;
              }
              if (domain) {
                  cookieText += "; domain=" + domain;
              }
              if (secure) {
                  cookieText += "; secure";
              }
              document.cookie = cookieText;
          },

          unset: function (name, path, domain, secure){
              this.set(name, "", new Date(0), path, domain, secure);
          }
    };
  ```
* `使用 CookieUtil`  <a href="#top">置顶</a>

  ```javascript
        //document.cookie=encodeURIComponent("name")+"="+encodeURIComponent("JiangXing");
        if(CookieUtil.get("name")!=null){
            let Name=CookieUtil.get("name");
            console.info(Name);
            CookieUtil.unset("name",null,null,null);
        }else{
            console.log("cookie 不存在!");

        }
  ```
##### <a href="#ttp" id="ttp"> 2.子 Cookie </a>  <a href="#top" id="SubCookie" >置顶</a> 

`子Cookie一般格式`:`name=name1=value1&name2=value2&name3=value3`

* `操作 子Cookie`

```javascript
    var SubCookieUtil = {

    get: function (name, subName){
        var subCookies = this.getAll(name);
        if (subCookies){
            return subCookies[subName];
        } else {
            return null;
        }
    },

    getAll: function(name){
        var cookieName = encodeURIComponent(name) + "=",
            cookieStart = document.cookie.indexOf(cookieName),
            cookieValue = null,
            cookieEnd,
            subCookies,
            i,
            parts,
            result = {};

        if (cookieStart > -1){
            cookieEnd = document.cookie.indexOf(";", cookieStart)
            if (cookieEnd == -1){
                cookieEnd = document.cookie.length;
            }
            cookieValue = document.cookie.substring(cookieStart + cookieName.length, cookieEnd);

            if (cookieValue.length > 0){
                subCookies = cookieValue.split("&");

                for (i=0, len=subCookies.length; i < len; i++){
                    parts = subCookies[i].split("=");
                    result[decodeURIComponent(parts[0])] = decodeURIComponent(parts[1]);
                }

                return result;
            }  
        } 

        return null;
    },

    set: function (name, subName, value, expires, path, domain, secure) {

        var subcookies = this.getAll(name) || {};
        subcookies[subName] = value;
        this.setAll(name, subcookies, expires, path, domain, secure);

    },

    setAll: function(name, subcookies, expires, path, domain, secure){

        var cookieText = encodeURIComponent(name) + "=",
            subcookieParts = new Array(),
            subName;

        for (subName in subcookies){
            if (subName.length > 0 && subcookies.hasOwnProperty(subName)){
                subcookieParts.push(encodeURIComponent(subName)
                +
                "=" + encodeURIComponent(subcookies[subName]));
            }
        }

        if (subcookieParts.length > 0){
            cookieText += subcookieParts.join("&");

            if (expires instanceof Date) {
                cookieText += "; expires=" + expires.toGMTString();
            }

            if (path) {
                cookieText += "; path=" + path;
            }

            if (domain) {
                cookieText += "; domain=" + domain;
            }

            if (secure) {
                cookieText += "; secure";
            }
        } else {
            cookieText += "; expires=" + (new Date(0)).toGMTString();
        }

        document.cookie = cookieText;        

    },

    unset: function (name, subName, path, domain, secure){
        var subcookies = this.getAll(name);
        if (subcookies){
            delete subcookies[subName];
            this.setAll(name, subcookies, null, path, domain, secure);
        }
    },

    unsetAll: function(name, path, domain, secure){
        this.setAll(name, null, new Date(0), path, domain, secure);
    }

    };
```
* `操作`
```javascript
    function StrToGMT(time){
        let GMT = new Date(time);
        return GMT;
    }
    SubCookieUtil.set("data","name","JiangXing");
    SubCookieUtil.set("data","book","JavaScript5 高级教程",StrToGMT("2018-8-5 20:24"));
    console.log(SubCookieUtil.get("data","book"));    
```
##### <a id="NoticeCookie" href="#top">Cookie 注意</a>
`还有一类Cookie被称为` `HTTP 专有Cookie 可以从浏览器或者服务器设置,但是只能在服务器端读取,因为Js无法获取HTTP专有 Cookie的值`

##### <a id="localStorage" href="#top">localStorage</a>
`HTML 本地存储提供了两个在客户端存储数据的对象：`
* `window.localStorage `:`存储没有截止日期的数据 本质上是一个 Storage 对象` [`官方文档`](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage)
* `window.sessionStorage`:`针对一个 session 来存储数据（当关闭浏览器标签页时数据会丢失）` [`官方文档`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage)

`在使用本地存储时，请检测 localStorage 和 sessionStorage 的浏览器支持：`
```javascript
if (typeof(Storage) !== "undefined") {
    // 针对 localStorage/sessionStorage 的代码
} else {
    // 抱歉！不支持 Web Storage ..
}
```
* `属性API`
  * `Storage.length` `只读 返回一个整数，表示存储在 Storage 对象中的数据项数量。`
* `方法API`
  * `Storage.key(index)`:`该方法接受一个数值 n 作为参数，并返回存储中的第 n 个键名。`
  ```node
   function populateStorage() {
     localStorage.setItem('bgcolor', 'yellow');
     localStorage.setItem('font', 'Helvetica');
     localStorage.setItem('image', 'cats.png');

     localStorage.key(2); // 应该返回 'image'
   }
  ```
  * `Storage.getItem(keyName)`:`该方法接受一个键名作为参数，返回键名对应的值。`
  * `Storage.setItem(keyName,value)`:`该方法接受一个键名和值作为参数，将会把键值对添加到存储中，如果键名存在，则更新其对应的值。`
  * `Storage.removeItem(keyName)`: `该方法接受一个键名作为参数，并把该键名从存储中删除。`
  * `Storage.clear()`:`调用该方法会清空存储中的所有键名。`

##### <a id="sessionStorage" href="#top">sessionStorage</a>
`sessionStorage 对象等同 localStorage 对象，不同之处在于只对一个 session 存储数据。如果用户关闭具体的浏览器标签页，数据也会被删除。`

```node
sessionStorage.setItem('myCat', 'Tom');
```

`sessionStorage的API 等同于 localStorage`

##### <a id="distinction" href="#top">三者区别</a> 
[`摘抄出处`](https://www.cnblogs.com/wwmm1996/p/10981958.html) `javaScript有三种数据存储方式，分别是： sessionStorage,localStorage,cookier 相同点：都保存在浏览器端`

##### 不同点

* `1.传递方式不同`
 * `cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递。`
 * `sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。`
 
* `2.数据大小不同`
 * `cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下。存储大小限制也不同，cookie数据不能超过4k，同时因为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识。`
 * `sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。`
 
* `3.数据有效期不同`
 * `sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；`
 * `localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；`
 * `cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。`
 
* `4.作用域不同`
 * `sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；`
 * `localStorage 在所有同源窗口中都是共享的；`
 * `cookie也是在所有同源窗口中都是共享的。`
 * `Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。`
 * `Web Storage 的 api 接口使用更方便。`

