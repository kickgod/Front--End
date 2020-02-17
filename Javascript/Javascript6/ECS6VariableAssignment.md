### <a href="#top" id="top" >JavaScript6 变量的解构赋值 </a>
`ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。然而有点逆天 这个语法,简单的还是可以用一下`

-----
- [x] [`数组的解构赋值`](#arraygetvalue) 
- [x] [`默认值`](#defaultvalue) 
- [x] [`对象的解构赋值`](#objectresolve)
- [x] [`字符串的解构赋值`](#stringresolve)
- [x] [`数值和布尔值的解构赋值`](#numberbooleanresolve)
- [x] [`函数参数的解构赋值`](#functionresolve)
- [x] [`圆括号问题`](#target1)
- [x] [`用途`](#target2)

----

##### [数组的解构赋值](#top) <b id="arraygetvalue"></b>
`基本的赋值结构是定义变量然后再逐个赋值例如：`
```node
int a = 10;
int b = 18;
int c = 8;
```
`ES6 允许写成下面这样。` **`最常见的用法`**
```node
 int [a,b,c] = [10,18,8];
```
`本质上，这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值` **`装逼的用法`**
```node
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1 bar // 2 baz // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1  tail // [2, 3, 4]

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, y, ...z] = ['a'];
x // "a"  y // undefined  z // []
```
* `如果解构不成功，变量的值就等于undefined。但是如果[] 解构不成功会变成 []  空数组`
* `有一种情况是不完全解构,不完全解构,那么会有部分值可以获得值但是解构还是可以成功`
###### [不完全解构](#top)
```node
let [x, y] = [1, 2, 3];
x // 1 y // 2

let [a, [b], d] = [1, [2, 3], 4];
a // 1 b // 2 d // 4
```
`如果等号的右边不是数组（或者严格地说，不是可遍历的结构，Iterator那么将会报错。`
```node
function* fibs() {
  let a = 0;
  let b = 1;
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

let [first, second, third, fourth, fifth, sixth] = fibs();
```
##### [默认值](#top) <b id="defaultvalue"></b>
* `如果右边没有你需要的值,或者右边的值为 undefined `
* `ES6 内部使用严格相等运算符（===），判断一个位置是否有值。所以，只有当一个数组成员严格等于undefined，默认值才会生效。`
```node
let [foo = true] = [];
foo // true

let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```
`如果默认值是一个表达式，那么这个表达式是惰性求值的，即只有在用到的时候，才会求值。`
```node
function f() {
  console.log('aaa');
}
let [x = f()] = [1];
```
##### [对象的解构赋值](#top) <b id="objectresolve"></b>
* `解构不仅可以用于数组，还可以用于对象。`
* `对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值`
* `如果解构失败，变量的值等于undefined。`
```node
let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined
```
*`如果吧一个对象的 name 属性赋值给一个变量 而这个变量又不是name名称该怎么办呢？ 例如这个变量 叫 Code`*
```
let {name:Code} = { name:"jxkivker",age:17,sex:true };
```
* `如果解构失败，变量的值等于undefined。` <br/>
* `赋值关系如下 那么复杂,你还是不要用这个了? 这不是脑子瓦特了，就是想秀技术！`
```javascript
let obj = {
  p: [
    'Hello',
    { y: 'World' }
  ]
};

let { p, p: [x, { y }] } = obj;
x // "Hello"
y // "World"
p // ["Hello", {y: "World"}]
```

##### [字符串的解构赋值](#top) <b id="stringresolve"></b>
* `类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值。`
```javascript
const [a, b, c, d, e] = 'hello';
// a "h"
// b "e"
// c "l"
// d "l"
// e "o"
```
* `类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值。`
```node
let {length : len} = 'hello';
len // 5
```
##### [数值和布尔值的解构赋值](#top) <b id="numberbooleanresolve"></b>
`数值和布尔值本质上还是对象 ,按照对象解构赋值就行`

```javascript
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true
```
##### [函数参数的解构赋值](#top) <b id="functionresolve"></b>
`函数的参数也可以使用解构赋值。`
```node
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3

[[1, 2], [3, 4]].map(([a, b]) => a + b);
// [ 3, 7 ]
```
##### [圆括号问题](#top) <b id="target1"></b>
`解构赋值虽然很方便，但是解析起来并不容易。对于编译器来说，一个式子到底是模式，还是表达式，没有办法从一开始就知道，必须解析到（或解析不到）等号才能知道。`

`由此带来的问题是，如果模式中出现圆括号怎么处理。ES6 的规则是，只要有可能导致解构的歧义，就不得使用圆括号。`

`以下三种解构赋值不得使用圆括号。`
* `1.变量声明语句`

   ```node
   // 全部报错
   let [(a)] = [1];

   let {x: (c)} = {};
   let ({x: c}) = {};
   let {(x: c)} = {};
   let {(x): c} = {};

   let { o: ({ p: p }) } = { o: { p: 2 } };
   ```
   `上面 6 个语句都会报错，因为它们都是变量声明语句，模式不能使用圆括号。`

* `2.函数参数`: `函数参数也属于变量声明，因此不能带有圆括号。`
   ```node
   // 报错
   function f([(z)]) { return z; }
   // 报错
   function f([z,(x)]) { return x; }
   ```
* `3.赋值语句的模式`
   ```node
   // 全部报错
   ({ p: a }) = { p: 42 };
   ([a]) = [5];
   ```
   `上面代码将整个模式放在圆括号之中，导致报错。`
   ```node
   // 报错
   [({ p: a }), { x: c }] = [{}, {}];
   ```
   `上面代码将一部分模式放在圆括号之中，导致报错。`

##### [用途](#top) <b id="target2"></b>
`变量的解构赋值用途很多。`

* `1.交换变量的值`
```node
let x = 1;
let y = 2;

[x, y] = [y, x];
```
`上面代码交换变量x和y的值，这样的写法不仅简洁，而且易读，语义非常清晰。`

* `2.从函数返回多个值`
`函数只能返回一个值，如果要返回多个值，只能将它们放在数组或对象里返回。有了解构赋值，取出这些值就非常方便。`

```node
// 返回一个数组

function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();

// 返回一个对象

function example() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();
```
* `3.函数参数的定义`
`解构赋值可以方便地将一组参数与变量名对应起来。`
```node
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```
* `4.提取 JSON 数据`
`解构赋值对提取 JSON 对象中的数据，尤其有用。`
```node
let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, data: number } = jsonData;

console.log(id, status, number);
// 42, "OK", [867, 5309]
```
`上面代码可以快速提取 JSON 数据的值。`

* `5.函数参数的默认值`
```node
jQuery.ajax = function (url, {
  async = true,
  beforeSend = function () {},
  cache = true,
  complete = function () {},
  crossDomain = false,
  global = true,
  // ... more config
} = {}) {
  // ... do stuff
};
```
`指定参数的默认值，就避免了在函数体内部再写var foo = config.foo || 'default foo';这样的语句。`

* `6.遍历 Map 结构`
`任何部署了 Iterator 接口的对象，都可以用for...of循环遍历。Map 结构原生支持 Iterator 接口，配合变量的解构赋值，获取键名和键值就非常方便。`
```node
const map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world
```
`如果只想获取键名，或者只想获取键值，可以写成下面这样。`
```node
// 获取键名
for (let [key] of map) {
  // ...
}

// 获取键值
for (let [,value] of map) {
  // ...
}
```
* `7.输入模块的指定方法`
`加载模块时，往往需要指定输入哪些方法。解构赋值使得输入语句非常清晰。`
```node
const { SourceMapConsumer, SourceNode } = require("source-map");
```
