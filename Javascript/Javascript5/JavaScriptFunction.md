#### [JavaScript 函数](#top) <b id="top"></b>

 `Javascript 函数有很多的其他深层次的东西`:white_check_mark:

---
> - [x] [`1.递归`](#proto) 
>    * [`匿名函数`](#lambda)
> - [x] [`2.闭包`](#close) 
> - [x] [`3.this对象`](#this)
> - [x] [`4.Dom操作造成了内存泄漏`](#lost)
> - [x] [`5.作用域和私有变量`](#inner)
> - [x] [`6.通过函数实现私有变量和私有静态变量`](#private)
> - [x] [`7.静态私有变量`](#static)
> - [x] [`8.模块模式`](#module)

----
#####  :octocat: [1.递归](#top) <b id="proto"></b> 
`函数名的本质是什么啊？ 引用 存储着一个执行函数本身的引用地址,既然是引用那么就可以传递,一个函数名可以只要改变引用就可以代表不同的函数,那么如何递归呢？`
* `使用函数名完成递归的潜在威胁`
```node
function test(num){
    if(num <= 1){
        return 1;
    }else{
        return num * test(num-1);
    }
}

let run = test; //吧递归函数的引用传递一下

//修改text的引用
test = function(){
    return 0;
}

console.log(run(10));//跑递归
// 0
```
* `arguments.callee 执行函数本身的指针,严格模式下不允许使用 可以通过它来保证 但是在js严格模式下 不允许使用这个指针`
```node
function factorial(end){
    if(end <= 1){
        return 1;
    }else{
        return end * arguments.callee(end - 1);
    }
}

let run_f = factorial;

factorial = function () {
    return 0;
}

console.log(run_f(10));//跑递归
// 3628800
```
##### `ES6 语法保证递归的正确性`
* `上诉两种方法都有缺陷,第一种递归有函数引用改变的威胁,第二种有严格模式不支持的危险 那么使用第三种吧`
* `通过const 直接不允许改变改变引用 就可以了并且严格模式也支持`
```node
 const test = function(num){
    if(num <= 1){
        return 1;
    }else{
        return num * test(num-1);
    }
}

console.log(test(10));//跑递归
//3628800

```
#####  	:triangular_flag_on_post: [匿名函数](#top) <b id="lambda"></b> 
`通过如下形式创建的函数 叫做匿名函数 又叫` `拉姆达函数`
```javascript
 //function 后面没有标识符
 const test = function(num){
    if(num <= 1){
        return 1;
    }else{
        return num * test(num-1);
    }
}
```
#####  :octocat: [2.闭包](#top) <b id="close"></b> 
`闭包`:`有权访问另一个函数作用域的变量的函数`
`创建闭包的常见方式，就是在一个函数内部创建另一个函数。`
`闭包就是能够读取其他函数内部变量的函数,可以把闭包简单理解成  ‘定义在一个函数内部的函数’ `
```javascript
function getMax(propertyName){
    return function (objext) {
        return Math.max(...objext[propertyName]);
    }
}

let person = {
    name :"jxkicker",
    age:20,
    scores:[98.6,78.5,78.6,95.6,15.5]
};

let val = {
    gg:[99,78,85,56,84,89,56]
}

let func_getmax = getMax("scores");
let func_getggMax = getMax('gg');

let maxScore =  func_getmax(person);
let maxgg = func_getggMax(val);

console.log(maxScore,maxgg); //98.6 99
```
- `func_getmax 依赖于 getMax 的 propertyName `
- `func_getmax 延长了 getMax 的函数作用域 getMax被引用 `
- `func_getmax 被销毁后 getMax 才能被销毁`

* `闭包的最大用处有两个：`
 * `1.可以读取函数内部的变量`
 * `2.让这些变量的值始终保存在内存中`
* `缺点：内存泄露`
```node
function call_one() {
    let n = 999;
    return function() {
        console.log(n);
    }
}

let result = call_one();
    result();//999
```


[`参考博客`](https://www.jianshu.com/p/87762b8864a8)

#####  :octocat: [3.this对象](#top) <b id="this"></b> 
`this 对象是在运行时给予函数的执行环境绑定的,在全局函数中,this等于 window` `注意: 匿名函数的执行环境具有全局性,因此this通常指向window`<br/>
> `在闭包中使用this对象可能会导致一些问题`
```javascript
var name = "The Window";

var obj = {
    name : "My Object",
    getNameFuc() {
        return function(){
           return this.name;
        };
    }
}

console.log(obj.getNameFuc()()); 
//The Window  非严格模式 在浏览器里面执行的结果
//undefined 在v8引擎执行结果
```

> `那么怎么使用 obj.name ？ 一切都是引用`

```node
 var name = "The Window";
 var obj = {
     name : "My Object",
     getNameFuc() {
         var that = this
         return function(){
            return that.name;
         };
     }
 }

 console.log(obj.getNameFuc()()); //My Object
```
`this是JavaScript的一个关键字，他是指函数执行过程中，自动生成的一个内部对象，是指当前的对象，只在当前函数内部使用。（this对象是在运行时基于函数的执行环境绑定的：在全局函数中，this指向的是window；当函数被作为某个对象的方法调用时，this就等于那个对象）`

##### 对象中的方法的this 在对象中的函数,指向对象本身

```node
/**
 * @param {string} id 证件号码
 * @param {string} name 名称
 * @param {number} age 年龄
 * @param {Boolean} sex 性别
 */
function Person(id,name,age,sex = true){
    this.name = name;
    this.age = age;
    this.id = id;
    this.sex= sex;
    
    this.getName = function(){
        return this.name;
    };
};

Person.prototype.toString = function(){
    return `${this.name} ${this.id} ${this.sex} ${this.age}`;
};

let jxKicker = new Person("2016110418","jxKicker",20);
console.log(jxKicker.toString());
```

##### es6 箭头函数 this
`箭头函数的this定义：箭头函数的this是在定义函数时绑定的，不是在执行过程中绑定的。简单的说，函数在定义时，this就继承了定义函数的对象。`

```node
var name = "The Window";

var obj = {
    name : "My Object",
    getNameFuc() {
        return ()=> this.name;
    }
}

console.log(obj.getNameFuc()()); //My Object
```
`此时的this 在定义的时候就已经绑定为obj对象`
#####  :octocat: [4.Dom操作造成了内存泄漏](#top) <b id="lost"></b> 
`有时候对于引用的不释放,特别是在 dom 操作中,可能会其一直保存在内容中,造成内容占用！`
```node
let list =  $('.list');
list.each(function(index,htmlElement){
  $(htmlElement).fadeOut();
});
```
`我们经常忘了去释放引用`
```node
let list =  $('.list');
list.each(function(index,htmlElement){
  $(htmlElement).fadeOut();
});
list = null;
```
#####  :octocat: [5.作用域和私有变量](#top) <b id="inner"></b> 
`JavaScript 5.1 没有块级作用域,所以会发生这样的事情`:`i 跳出循环之后 还在 但是不慌 你记住 用let 和const[常量] 声明的变量就是支持块级作用域的,这是es6的标准并且已经被各大浏览器所支持【现已经完全支持】`

```javascript
function circle(){
    for(var i = 0;i<10;i++){
        console.log(i);
    }
    console.log(i);//10
}

circle();
```

`es6`：`使用let 声明的变量`

```javascript
function circle(){
    for(let i = 0;i<10;i++){
        console.log(i);
    }
    console.log(i);//error: i is not defined
}

circle();
```

#####  :octocat: [6.通过函数实现私有变量和私有静态变量](#top) <b id="private"></b> 
`JavaScript中没有私有成员的概念,所有对象属性都是公有的,不过倒是有一个私有变量的概念,任何在函数中的定义的变量,都可以认为是私有变量,因为不能在函数的外部访问的这些变量,私有变量包括`:`函数的参数`，`局部变量`,`函数内部定义的其他函数`
> `这个函数有三个私有变量 num1 num2 num3`
```node
function add(num1,num2){
    let num3 = num1 + num2;
};
```
`在构造函数内部不使用 this 定义的变量 都是私有的`
```node
function myVariable(){
    var name = "jxkicker";
    var age = 20;
    
    this.setName = function(value) {
        name = value;
    }

    this.getName = function() {
        return name;
    }

    this.getAge = function(){
        return age;
    }
}

let obj = new myVariable();
let name = obj.getName();

console.log(name);
```
`通过参数也是可以的`:`这样一个对象的私有变量 就出来了  知道怎么做了吧`
```javascript
function myVariable(name,age){
    
    this.setName = function(value) {
        name = value;
    }

    this.getName = function() {
        return name;
    }

    this.getAge = function(){
        return age;
    }
}

let obj = new myVariable("Jxkicker","age");
let name = obj.getName();

console.log(name);

```

#####  :octocat: [7.静态私有变量](#top) <b id="static"></b> 
`创建一个私有的作用域`

```node
(function () {
    var name = "";
    Person = function (value) {
        name = value;
    };
    Person.prototype.setName = function (value) {
        name = value;
    }
    Person.prototype.getName = function () {
        return name;
    }
})();

var p1 = new Person("JxKicker");
var p2 = new Person("wangzhe");
console.log(p1.getName());//wangzhe
console.log(p2.getName());//wangzhe
```

#####  :octocat: [8.模块模式](#top) <b id="module"></b> 
`模块模式为单例创建私有变量和特权方法.单例[singleton]的意思 就是只有一个实例的对象,Javascript 使用对象字面量的方式创建单例对象`
```node
var singleton = {
   name:value,
   method:function(){
      //方法的代码
   }
}
```
> **`我们常常`** ：`在Web 程序中我们常常使用一个单例来管理应用程序的信息.这个简单的例子,创建了一个用于管理组件的 application对象,在创建
这个对象的过程中,首先声明一个私有的components`
```node
var application = function () {
    
    var components = new Array();

    components.push(new baseComponent());

    return {
        /**
         * 返回注册组件个数
         */
        getComponentsCount =function(){
            return components.length;
        },
        /**
         * 返回某个组件
         */
        getComponents =function(key){
            return components.filter((val,index,components)=> val.key == key);
        },

        //公开注册之间的方法
        registerComponent:function(component){
            if (typeof component == "object"){
                components.push(component);
            }
        }

    };

}
```
`有一个问题 那就是  函数返回的是一个 对象 但是这个对象 是没有类型的[默认Objcet 对象] 。。。。我们给他一个类型 增强一下`
```node
var application = function () {

    var components = new Array();

    components.push(new baseComponent());

    var app = new ApplicationComponent()

    app.getComponentsCount =function(){
        return components.length;
    };

    app.getComponents = function(key){
        return components.filter((val,index,components)=> val.key == key);
    };
    
    
    app.registerComponent = function(component){
        if (typeof component == "object"){
            components.push(component);
        }
    }

    return app;
}
```
`功能和前者是一样的,但是多了一个类型`
