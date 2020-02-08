### <a id="top" href="#top">JavaScript 原生Form :grey_exclamation:</a> 

----
`一个网站的关键在于表单，他决定用户的输入,向服务器提交的数据,Js对于它有许多的方法和属性对象,以便我们开发更优秀的前端界面` :speech_balloon:
- [x] <a href="#FormBasicKnow">`表单的基本知识`</a>
  - <a href="#GetFromElement">`获取表单：HTMLFormElement `</a> 
  - <a href="#FormSubmit">`提交表单：Submit `</a> 
  - <a href="#resetform">`重置表单：Reset `</a> 
  - <a href="#formfiled">`表单字段 element`</a> 
  - <a href="#formfiledProperty">`表单字段公共属性`</a> 
  - <a href="#formfiledFunction">`表单字段公共方法`</a> 
  - <a href="#formfiledEventFunction">`表单字段公共事件`</a> 
- [x] <a href="#TextScript">`文本框脚本`</a>
  - <a href="#getvaluetext">`获取表单文本的值`</a> 
  - <a href="#Shoosetext">`选择文本select`</a> 
- [x] <a href="#keypressevent">`过滤输入`</a>
  - <a href="#copypaste">`粘贴板事件`</a>
- [x] <a href="#target1">`自动切换焦点 `</a>
- [x] <a href="#target2">`HTML5约束验证API  `</a>
- [x] <a href="#target3">`选择框脚本`</a>

#####  <a id="FormBasicKnow" href="#FormBasicKnow">表单的基本知识</a>  <a href="#top">   ↑</a>
<a href="#">`表单对应：HTMLFormElement  --->继承自 HTMLElement`</a> <br/>

* `在HTML中，表单由form标签，在javascript中，表单对应HTMLFormElement类型，HTMLFormElement类型继承HTMLElement类型，所有它和其他的Element元素有相同的默认属性，同时它也有自己的属性和方法：`
  * `acceptCharset`：`服务器能够处理的字符集；等价于 HTML 中的 accept-charset 特性。`
  * `action`：`接受请求的 URL；等价于 HTML 中的 action 特性 。`
  * `elements`：`表单中所有控件的集合（HTMLCollection）。`
  * `enctype`：`请求的编码类型；等价于 HTML 中的 enctype 特性。`
  * `length`：`表单中控件的数量。`
  * `method`：`要发送的 HTTP 请求类型，通常是"get"或"post"；等价于 HTML 的 method 特性。`
  * `name`：`表单的名称；等价于 HTML 的 name 特性。`
  * `reset()`：`将所有表单域重置为默认值。`
  * `submit()`：`提交表单。`
  * `target`：`用于发送请求和接收响应的窗口名称；等价于 HTML 的 target 特性`
##### <a href="#GetFromElement" id="GetFromElement">获取表单：HTMLFormElement</a> <br/>
`每一个表单都应该有一个唯一的id和name 且id和name应该相同`
* `我们可以通过id或者name获取`：`var formElement=document.getElementById("form1");` 
* `通过document.forms来取得页面中的所有表单元素`:`var forms=document.forms;`
* `通过forms属性通过数组字典获取form`：`var firstForm=document.forms[0];` `var form2=document.forms['form2'];`
```javascript
window.onload=function(){
  var formElement=document.getElementById("form1");
  console.log("表单提交方式："+formElement.method);
  console.log("表单请求的编码类型："+formElement.enctype);
  console.log("表单中控件数目："+formElement.length);
  console.log("表单提交服务器: "+formElement.action);
  console.log("表单名称："+formElement.name);
  console.log("Form 表单个数: ["+formElement.elements.length +"]");
  console.log("Form Target: "+formElement.target);

  var forms=document.forms;
  console.log("HTML表单数目: "+forms.length);
}
```
##### <a href="#FormSubmit" id="FormSubmit">提交表单：submit submit事件 </a> 
`每一个表单里面的input或者,type为submit的button,或者图像按钮 都可以下直接提交表单`
```html
 <input type="submit" value="提交" class="btn btn-warning btn-sm"  />
 <button type="submit" class="btn btn-danger btn-sm">提交</button>
 <input type="image" src="../Resources/Image/Icon/button.png" width="25px" />
```
`当浏览器在将请求发送给服务器之前会触发submit事件,这样就有机会验证表单数据,这样我们就有机会验证表单数据,并据此决定是否允许表单提交,如果想住个提交
，那么阻止这个事件的默认行为就可以取消表单提交`
```javascript
formElement.addEventListener("submit",function(event){
 alert("输出错误,无法提交");
 this.submit();
 event.preventDefault();//取消浏览器默认事件
},false);
```
##### <a href="#resetform" id="resetform">重置表单：reset() reset事件 </a> 
`用户单击重置,表单会被重置。使用type 特性值的<input> 或<button> 都可以创建重置按钮,如下面的例如所示。`

````html
 <input type="reset"  value="Reset" />
 <button type="reset">重置</button>
````
`表单重置的时候也会在重置之前触发reset事件--如果阻止默认事件那么会让表单放弃提交,使用form.reset() 方法可以提交表单`
```javascript
formElement.addEventListener("reset",function(event){
  alert("无法重置");
  //this.reset();
  event.preventDefault();//取消浏览器默认事件
},false);
```

#####  <a id="formfiled" href="#formfiled">表单字段：elements </a>  <a href="#top"> ↑ </a>
`form 的属性elements 属性 可以让你以name字典或者 数组索引的方式访问form表单的所有input textare fieldset button 控件`
```javascript
  console.log(formElement.elements["StudentID"]);
  console.log(formElement.elements[0]);
  
  <input type="radio" name="color" value="Red" /> Red
  <input type="radio" name="color" value="Yellow" /> Yellow
  <input type="radio" name="color" value="Blue" /> Blue
```
**`Radio的问题 `**
```html           	
console.log(formElement.elements["color"]);
//输出了是一个RadioNodelist[input input input] 集合
<input type="radio" name="color" value="Red" /> Red
<input type="radio" name="color" value="Yellow" /> Yellow
<input type="radio" name="color" value="Blue" /> Blue
```
#####  <a id="formfiledProperty" href="#formfiledProperty">表单字段公共属性</a>  <a href="#top"> ↑ </a>
* Form 表单字段拥有的一些相同的属性
  * `disabled`：`布尔值，表示当前字段是否被禁用。`
  * `form`：`指向当前字段所属表单的指针；只读。`
  * `name`：`当前字段的名称。`
  * `readOnly`：`布尔值，表示当前字段是否只读。`
  * `tabIndex`：`表示当前字段的切换（tab）序号。`
  * `type`：`当前字段的类型，如"checkbox"、 "radio"，等等。`
    * `<select>...</select>`:**`select-one`**
    * `<select multiple>...</select>`:**`select-multiple`**
    * `<button>..</button>`:**`submit`**
  * `value`：`当前字段将被提交给服务器的值。对文件字段来说，这个属性是只读的，包含着文件在计算机中的路径`
```javascript
  console.log("表单字段 value: "+formElement.elements[0].value);
  console.log("表单字段 disabled: "+formElement.elements[0].disabled);
  console.log(formElement.elements[0].tabIndex);
  console.log(formElement.elements[0].name);
  console.log(formElement.elements[0].readOnly);
  console.log(formElement.elements[0].form.id);
```
`有时候在我们提交表单以后,我们就应该将提交按钮设置为disable 以免用户重复提交,为此我们可以监听submit事件,当用户提交按钮以后就禁用按钮,一直到得到返回之后,如果是Ajax 那么就是等ajax 回调函数执行的时候 在重新启用它`

#####  <a id="formfiledFunction" href="#formfiledFunction">表单字段公共方法</a>   <a href="#top"> ↑ </a>
* `公有的方法有两个`
  * `focus()`:`可以将浏览器焦点设置到表单的字段上面 ----请注意是方法  不是 onfocus事件 ，但是html5新增的autofocus 属性`
  * `blur()`：`可以使得字段失去焦点`
  
#####  <a id="formfiledEventFunction" href="#formfiledEventFunction">表单字段公共事件</a>   <a href="#top"> ↑ </a>
* `公共的事件`
  * `focus` `焦点事件`： `当浏览器的字段获得焦点的时候触发`
  * `blur` `失去焦点事件` ：`失去焦点`
  * `change` : `value的值改变或者 <select> 标签的选项改变`
#####  <a id="TextScript" href="#TextScript">文本框脚本</a>   <a href="#top"> ↑ </a>
* `在HTML中，有两种方式来表现文本框：一种是使用<input>元素的单行文本框，另一种是使用<textarea>的多行文本框。这两个控件非常相似，而且多数时候的行为也差不多。不过，它们之间仍然存在一些重要的区别。`
* `要表现文本框，必须将<input>元素的type特性设置为”text”。而通过设置size特性，可以指定文本框中能够显示的字符数。通过value特性，可以设置文本框的初始值，而maxlength特性则用于指定文本框可以接受的最大字符数。`
* `相对而言，<textarea>元素则始终会显示为一个多行文本框。要指定文本框的大小，可以使用rows和cols特性。其中rows特性指定的是文本框的字符行数，而cols特性指定的是文本框的字符列数。`

#####  <a id="getvaluetext" href="#getvaluetext">获取表单文本的值</a>   <a href="#top"> ↑ </a>
`要是用value 属性,才能获得input 的value值,获得控件的文本`
```javascript
 var input_text = document.forms[0].elements["userinput"];
 var val = input_text.value;
 console.log("用户输入:"+val);
```
#####  <a id="Shoosetext" href="#Shoosetext">选择文本方法/事件 </a>   <a href="#top"> ↑ </a>
`文本控件都有哦一个select事件,可以让控件选择整个文本,当然这个时候最好当焦点在文本框的时候才选择文本才好,所有最好按照用的需求来使用这个方法`<br/>
**`select 方法`**
```javascript
 var input_text = document.forms[0].elements["userinput"];
 input_text.select();
```
**`select 事件`**
* `当用户选择了文本的时候触发,并且h5给我们提供了两个有用的属性`
  * `selectionStart`:`用户选择文本的开始位置索引`
  * `selectionEnd`:`用户选择文本的结束位置索引`
```javascript
  window.onload=function(){
    let val;
    document.getElementById("UserInfoSelect").addEventListener("select",function(event){
      val=this.value.substring(this.selectionStart,this.selectionEnd);	
      alert("用户选择:"+val);
    },false);
  }
```
**`选择部分文本:`** `选择部分文本可以使用setSelectionRange() 方法 使用这个访问应该在调用这个方法之前或之后就调用focus方法聚焦到文本框,IE8之前支持的是通过 createTextRange() 方法创建一个范围,并将其放在恰当的位置上,然后在调用moveStart和moveEnd 方法将范围移动到位 在调用这些方法之前
还有调用collapse 方法将范围折叠刀文本框的开始位置`
```javascript
 range.collpase(true);
 range.moveStart("character",0);
 range.moveEnd("character",5);
 range.select()
```
**`IE9/Firefox/Chrome/Opera 都支持的方法`**
```javascript
  var txt= document.getElementById("UserInfoSelect");
  txt.setSelectionRange(1,20);
  txt.focus();
```
#####  <a id="keypressevent" href="#keypressevent">过滤输入</a>   <a href="#top"> ↑ </a>
`过滤输入最快捷的方式就是监控keypress 事件,然后阻止浏览器的默认事件就可以阻止用户的输入，但是因为输入法的关系用户还是可以输入中文数字和英文
已经不可以输入了`

##### 屏蔽字符 
`响应向文本框中插入字符操作的是 keypress 事件。因此，可以通过阻止这个事件的默 认行为来屏蔽此类字符`
```node
 txt.addEventListener("keypress",function(event){
   event.preventDefault();
 },false);
```
##### <a id="copypaste" href="#copypaste">粘贴板事件</a>   <a href="#top"> ↑ </a> 
* `关于粘贴复制剪切操作的事件`
  * `beforecopy`：`在发生复制操作前触发`
  * `copy`：`在发生复制操作时触发`
  * `beforecut`：`在发生剪切操作前触发`
  * `cut`：`在发生剪切操作时触发`
  * `beforepaste`：`在发生粘贴操作前触发`
  * `paste`：`在发生粘贴操作时触发`

`由于没有针对剪贴板操作的标准，这些事件及相关对象会因浏览器而异。在 Safari、Chrome和 Firefox 中，beforecopy、beforecut 和 beforepaste 事件只会在显示针对文本框的上下文菜单（预期将发 生剪贴板事件）的情况下触发。但是，IE则会在触发 copy、cut 和 paste 事件之前先行触发这些事件。 至于 copy、cut 和 paste 事件，只要是在上下文菜单中选择了相应选项，或者使用了相应的键盘组合 键，所有浏览器都会触发它们`
  
* `访问剪切板中的数据`
  * `使用clipboardData对象[类型: DataTransfer ]`
  * `1.这个对象有三个方法`
    * `getDate`:`用于从剪贴板中取得数据，它接受一个参数，即要取得的数据的格式。在 IE中，有两种数据格式："text" 和"URL"。在 Firefox、Safari 和 Chrome 中，这个参数是一种 MIME 类型；不过，可以用"text"代表 "text/plain"。 `
    * `setDate`：`第一个参数也是数据类型，第二个参数是要放在剪贴板中的文本。对于 第一个参数，IE 照样支持"text"和"URL"，而 Safari 和 Chrome 仍然只支持 MIME 类型。但是，与 getData()方法不同的是，Safari和 Chrome的 setData()方法不能识别"text"类型。这两个浏览器在 成功将文本放到剪贴板中后，都会返回 true；否则，返回 false`
    * `clearDate()`:`清楚数据 参数为,一个 string指定要删除的数据类型。如果此参数为空字符串或未提供，则将删除所有类型的数据。`
    
```node
var EventUtil = {
    getClipboardText: function (event) {
        var clipboardData = (event.clipboardData || window.clipboardData);
        return clipboardData.getData("text");
    },
    setClipboardText: function (event, value) {
        if (event.clipboardData) {
            return event.clipboardData.setData("text/plain", value);
        } else if (window.clipboardData) {
            return window.clipboardData.setData("text", value);
        }
    },
}; 
```

#####  <a id="target1" href="#top">自动切换焦点</a>   <a href="#top"> ↑ </a>
`使用 JavaScript可以从多个方面增强表单字段的易用性。其中，常见的一种方式就是在用户填写 完当前字段时，自动将焦点切换到下一个字段。通常，在自动切换焦点之前，必须知道用户已经输入了 既定长度的数据（例如电话号码）。例如，美国的电话号码通常会分为三部分：区号、局号和另外 4 位 数字。为取得完整的电话号码，很多网页中都会提供下列 3个文本框`

```html
<input type="text" name="tel1" id="txtTel1" maxlength="3"> 
<input type="text" name="tel2" id="txtTel2" maxlength="3"> 
<input type="text" name="tel3" id="txtTel3" maxlength="4"> 
```

`为增强易用性，同时加快数据输入，可以在前一个文本框中的字符达到大数量后，自动将焦点切 换到下一个文本框。换句话说，用户在第一个文本框中输入了 3个数字之后，焦点就会切换到第二个文 本框，再输入 3个数字，焦点又会切换到第三个文本框。这种“自动切换焦点”的功能，可以通过下列 代码实现： `

```node
(function () {

    function tabForward(event) {
        event = EventUtil.getEvent(event);
        var target = EventUtil.getTarget(event);

        if (target.value.length == target.maxLength) {
            var form = target.form;

            for (var i = 0, len = form.elements.length; i < len; i++) {
                if (form.elements[i] == target) {
                    if (form.elements[i + 1]) {
                        form.elements[i + 1].focus();
                    }
                    return;
                }
            }
        }
    }

    var textbox1 = document.getElementById("txtTel1");
    var textbox2 = document.getElementById("txtTel2");
    var textbox3 = document.getElementById("txtTel3");

    EventUtil.addHandler(textbox1, "keyup", tabForward);
    EventUtil.addHandler(textbox2, "keyup", tabForward);
    EventUtil.addHandler(textbox3, "keyup", tabForward);

})(); 
```
#####  <a id="target2" href="#top">HTML5约束验证API </a>   <a href="#top"> ↑ </a>
`为了在将表单提交到服务器之前验证数据，HTML5新增了一些功能。有了这些功能，即便 JavaScript 被禁用或者由于种种原因未能加载，也可以确保基本的验证。`

##### 1.required
`必填字段 `:`第一种情况是在表单字段中指定了 required 属性，如下面的例子所示： `
```html 
<input type="text" name="username" required> 
```
`任何标注有 required 的字段，在提交表单时都不能空着。这个属性适用于<input>、<textarea> 和<select>字段`

`可以检查某个表单字段是否为必填字段。 `
```html 
var isUsernameRequired = document.forms[0].elements["username"].required; 
```

##### 2.其他输入类型 
`HTML5为<input>元素的 type 属性又增加了几个值。这些新的类型不仅能反映数据类型的信息， 而且还能提供一些默认的验证功能。其中，"email"和"url"是两个得到支持多的类型，各浏览器也 都为它们增加了定制的验证机制。例如`

```html
<input type="email" name ="email"> 
<input type="url" name="homepage"> 
```
##### 3.数值范围 
`除了"email"和"url"，HTML5 还定义了另外几个输入元素。这几个元素都要求填写某种基于数字的值："number"、"range"、"datetime"、"datetime-local"、"date"、"month"、"week"， 还有"time"。浏览器对这几个类型的支持情况并不好，因此如果真想选用的话，要特别小心。`

```html
<input type="number" min="0" max="100" step="5" name="count"> 
```
##### 4.输入模式 
`HTML5为文本字段新增了 pattern 属性。这个属性的值是一个正则表达式，用于匹配文本框中的 值。例如，如果只想允许在文本字段中输入数值，可以像下面的代码一样应用约束： `

```html
<input type="text" pattern="\d+" name="count"> 
```
`使用以下代码可以检测浏览器是否支持 pattern 属性。 `

```node
var isPatternSupported = "pattern" in document.createElement("input"); 
```

##### 5.检测有效性 
`使用 checkValidity()方法可以检测表单中的某个字段是否有效。所有表单字段都有个方法，如 果字段的值有效，这个方法返回 true，否则返回 false.`

```node
if (document.forms[0].elements[0].checkValidity()){  
  //字段有效，继续 
  } else {    
  //字段无效
} 
```
`要检测整个表单是否有效，可以在表单自身调用 checkValidity()方法。如果所有表单字段都有 效，这个方法返回 true；即使有一个字段无效，这个方法也会返回 false。 `

```node
if(document.forms[0].checkValidity()){   
  //表单有效，继续 
  } else { 
  //表单无效 
} 
```

`与 checkValidity()方法简单地告诉你字段是否有效相比，validity 属性则会告诉你为什么字 段有效或无效。这个对象中包含一系列属性，每个属性会返回一个布尔值。 `

* `customError` ：`如果设置了 setCustomValidity()，则为 true，否则返回 false。` 
* `patternMismatch`：`如果值与指定的 pattern 属性不匹配，返回 true。` 
* `rangeOverflow`：`如果值比 max 值大，返回 true。` 
* `rangeUnderflow`：`如果值比 min 值小，返回 true。` 
* `stepMisMatch`：`如果 min 和max 之间的步长值不合理，返回 true。` 
* `tooLong`：`如果值的长度超过了 maxlength 属性指定的长度，返回 true。有的浏览器（如Firefox 4） 会自动约束字符数量，因此这个值可能永远都返回 false。` 
* `typeMismatch`：`如果值不是"mail"或"url"要求的格式，返回 true。` 
* `valid`：`如果这里的其他属性都是 false，返回 true。checkValidity()也要求相同的值。` 
* `valueMissing`：`如果标注为 required 的字段中没有值，返回 true。 `

`因此，要想得到更具体的信息，就应该使用 validity 属性来检测表单的有效性。下面是一个例子。`

```node
if (input.validity && !input.validity.valid) {
    if (input.validity.valueMissing) {
        alert("Please specify a value.")
    } else if (input.validity.typeMismatch) {
        alert("Please enter an email address.");
    } else {
        alert("Value is invalid.");
    }
}
```

##### 6.禁用验证
`通过设置 novalidate 属性，可以告诉表单不进行验证。 `

```html
<form method="post" action="signup.php" novalidate>    
  <!--这里插入表单元素--> 
</form> 
```
`在 JavaScript中使用 noValidate 属性可以取得或设置这个值，如果这个属性存在，值为 true， 如果不存在，值为 false。 `
`document.forms[0].noValidate = true; //禁用验证 `

`如果一个表单中有多个提交按钮，为了指定点击某个提交按钮不必验证表单，可以在相应的按钮上 添加 formnovalidate 属性`

```html
<form method="post" action="foo.php">   
  <!--这里插入表单元素-->    
  <input type="submit" value="Regular Submit"> 
  <input type="submit" formnovalidate name="btnNoValidate" value="Non-validating Submit"> 
</form> 
```
`在这个例子中，点击第一个提交按钮会像往常一样验证表单，而点击第二个按钮则会不经过验证而 提交表单。使用 JavaScript也可以设置这个属性。 `

```node
//禁用验证 
document.forms[0].elements["btnNoValidate"].formNoValidate = true; 
```

#####  <a id="target3" href="#top">选择框脚本</a>   <a href="#top"> ↑ </a>
`选择框是通过<select>和<option>元素创建的。为了方便与这个控件交互，除了所有表单字段共 有的属性和方法外，HTMLSelectElement 类型还提供了下列属性和方法`

* `add(newOption, relOption)`：`向控件中插入新<option>元素，其位置在相关项（relOption） 之前。` 
* `multiple`：`布尔值，表示是否允许多项选择；等价于 HTML中的 multiple 特性。` 
* `options`：`控件中所有<option>元素的 HTMLCollection。` 
* `remove(index)`：`移除给定位置的选项。` 
* `selectedIndex`：`基于 0的选中项的索引，如果没有选中项，则值为-1。对于支持多选的控件， 只保存选中项中第一项的索引。  size：选择框中可见的行数；等价于 HTML中的 size 特性。`

`选择框的 type 属性不是"select-one"，就是"select-multiple"，这取决于 HTML代码中有 没有 multiple 特性。选择框的 value 属性由当前选中项决定，相应规则如下。`

* `如果没有选中的项，则选择框的 value 属性保存空字符串。` 
* `如果有一个选中项，而且该项的 value 特性已经在 HTML中指定，则选择框的 value 属性等 于选中项的 value 特性。即使 value 特性的值是空字符串，也同样遵循此条规则。` 
* `如果有一个选中项，但该项的 value 特性在 HTML中未指定，则选择框的 value 属性等于该 项的文本。 `
* `如果有多个选中项，则选择框的 value 属性将依据前两条规则取得第一个选中项的值。 `

`在 DOM 中，每个<option>元素都有一个 HTMLOptionElement 对象表示。为便于访问数据， HTMLOptionElement 对象添加了下列属性：`

* `index`：`当前选项在 options 集合中的索引。` 
* `label`：`当前选项的标签；等价于 HTML中的 label 特性。` 
* `selected`：`布尔值，表示当前选项是否被选中。将这个属性设置为 true 可以选中当前选项。` 
* `text`：`选项的文本。  value：选项的值（等价于 HTML中的 value 特性）`


`其中大部分属性的目的，都是为了方便对选项数据的访问。虽然也可以使用常规的 DOM功能来访 问这些信息，但效率是比较低的，如下面的例子所示： `
```node
var selectbox = document.forms[0].elements["location"]; 
 
//不推荐 
var text = selectbox.options[0].firstChild.nodeValue;       //选项的文本 
var value = selectbox.options[0].getAttribute("value");     //选项的值 

var selectbox = document.forms[0]. elements["location"]; 
 
/*
以上代码使用标准 DOM方法，取得了选择框中第一项的文本和值。可以与下面使用选项属性的代 码作一比较： 
*/ 

//推荐 
var text = selectbox.options[0].text;         //选项的文本 
var value = selectbox.options[0].value;       //选项的值 
```

##### 选择选项 
`对于只允许选择一项的选择框，访问选中项的简单方式，就是使用选择框的 selectedIndex 属 性，如下面的例子所示：`

`var selectedOption = selectbox.options[selectbox.selectedIndex]; `

`取得选中项之后，可以像下面这样显示该选项的信息： `
```node 
var selectedIndex = selectbox.selectedIndex; 
var selectedOption = selectbox.options[selectedIndex]; 
```

#####  <a id="" href="#"></a>   <a href="#top"> ↑ </a>
#####  <a id="" href="#"></a>   <a href="#top"> ↑ </a>
#####  <a id="" href="#"></a>   <a href="#top"> ↑ </a>
#####  <a id="" href="#"></a>   <a href="#top"> ↑ </a>





