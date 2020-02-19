### [ECMASCript6  对象扩展](#top)  <b id="top"></b>
`对象（object）是 JavaScript 最重要的数据结构。ES6 对它进行了重大升级，`

----

* [x]  [`1.属性的简洁表示法`](#target1)

----
 ##### [1.属性的简洁表示法](#top) <b id="target1"></b>
 `ES6 允许在大括号里面，直接写入变量和函数，作为对象的属性和方法。这样的书写更加简洁。`
 
 ```node
const foo = 'bar';
const baz = {foo};
baz // {foo: "bar"}

// 等同于
const baz = {foo: foo};
 ```
