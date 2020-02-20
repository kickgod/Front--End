### [ECMASCript6  对象扩展](#top)  <b id="top"></b>
`对象（object）是 JavaScript 最重要的数据结构。ES6 对它进行了重大升级，`

----

* [x] [`1.属性的简洁表示法`](#target1)
* [x] [`2.方法的 name 属性`](#target2)
* [x] [`3.属性的可枚举性和遍历`](#target3)
* [x] [`4.属性的遍历`](#target4)
* [x] [`5.super 关键字`](#target5)
* [x] [`6.链判断运算符 和 Null 判断运算符`](#target6)
* [x] [`7.对象的新增方法`](#target7)
  * [`7.1 Object.is()`](#target7)
  * [`7.2 Object.assign()`](#target7)
  * [`7.3 getOwnPropertyDescriptor()`](#target7)
  * [`7.4 __proto__属性，Object.setPrototypeOf()，Object.getPrototypeOf()`](#target7)
  
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

`下面是一个实际的例子。`
```node
let birth = '2000/01/01';

const Person = {

  name: '张三',

  //等同于birth: birth
  birth,

  // 等同于hello: function ()...
  hello() { console.log('我的名字是', this.name); }

};
```

`属性的赋值器（setter）和取值器（getter），事实上也是采用这种写法。`
```node
const cart = {
  _wheels: 4,

  get wheels () {
    return this._wheels;
  },

  set wheels (value) {
    if (value < this._wheels) {
      throw new Error('数值太小了！');
    }
    this._wheels = value;
  }
}
```

##### [2.方法的 name 属性](#top) <b id="target2"></b>
`函数的name属性，返回函数名。对象方法也是函数，因此也有name属性。`

```node
const person = {
  sayName() {
    console.log('hello!');
  },
};

person.sayName.name   // "sayName"
```
`如果对象的方法使用了取值函数（getter）和存值函数（setter），则name属性不是在该方法上面，而是该方法的属性的描述对象的get和set属性上面，返回值是方法名前加上get和set。`

```node
const obj = {
  get foo() {},
  set foo(x) {}
};

obj.foo.name
// TypeError: Cannot read property 'name' of undefined

const descriptor = Object.getOwnPropertyDescriptor(obj, 'foo');

descriptor.get.name // "get foo"
descriptor.set.name // "set foo"
```
`有两种特殊情况：bind方法创造的函数，name属性返回bound加上原函数的名字；Function构造函数创造的函数，name属性返回anonymous。`

```node
(new Function()).name // "anonymous"

var doSomething = function() {
  // ...
};
doSomething.bind().name // "bound doSomething"
```

 ##### [3.属性的可枚举性和遍历](#top) <b id="target3"></b> 
`可枚举性，对象的每个属性都有一个描述对象（Descriptor），用来控制该属性的行为。` `Object.getOwnPropertyDescriptor` `方法可以获取该属性的描述对象。`

```node
let obj = { foo: 123 };
Object.getOwnPropertyDescriptor(obj, 'foo')
//  {
//    value: 123,
//    writable: true,
//    enumerable: true,
//    configurable: true
//  }
```

`目前，有四个操作会忽略enumerable为false的属性。`

* `for...in循环：只遍历对象自身的和继承的可枚举的属性。`
* `Object.keys()：返回对象自身的所有可枚举的属性的键名。`
* `JSON.stringify()：只串行化对象自身的可枚举的属性。`
* `Object.assign()： 忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性。`

`另外，ES6 规定，所有 Class 的原型的方法都是不可枚举的。`

```node
Object.getOwnPropertyDescriptor(class {foo() {}}.prototype, 'foo').enumerable
// false
```

##### [4.属性的遍历](#top) <b id="target4"></b>
`ES6 一共有 5 种方法可以遍历对象的属性。`

* `（1）for...in`
   ```node
   for...in循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。
   ```
* `（2）Object.keys(obj)`
   ```node
   Object.keys返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。
   ```
* `（3）Object.getOwnPropertyNames(obj)`
   ```node
   Object.getOwnPropertyNames返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。
   ```
* `（4）Object.getOwnPropertySymbols(obj)`
   ```node
   Object.getOwnPropertySymbols返回一个数组，包含对象自身的所有 Symbol 属性的键名。
   ```
* `（5）Reflect.ownKeys(obj)`
   ```node
   Reflect.ownKeys返回一个数组，包含对象自身的所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。
   ```
`以上的 5 种方法遍历对象的键名，都遵守同样的属性遍历的次序规则。 `

* `首先遍历所有数值键，按照数值升序排列。`
* `其次遍历所有字符串键，按照加入时间升序排列。`
* `最后遍历所有 Symbol 键，按照加入时间升序排列。`

```node
Reflect.ownKeys({ [Symbol()]:0, b:0, 10:0, 2:0, a:0 })
// ['2', '10', 'b', 'a', Symbol()]
```

`上面代码中，Reflect.ownKeys方法返回一个数组，包含了参数对象的所有属性。这个数组的属性次序是这样的，首先是数值属性2和10，其次是字符串属性b和a，最后是 Symbol 属性。`

##### [5.super 关键字](#top) <b id="target5"></b>
`我们知道，this关键字总是指向函数所在的当前对象，ES6 又新增了另一个类似的关键字super，指向当前对象的原型对象。`

```node
const proto = {
  foo: 'hello'
};

const obj = {
  foo: 'world',
  find() {
    return super.foo;
  }
};

Object.setPrototypeOf(obj, proto);
obj.find() // "hello"
```
`JavaScript 引擎内部，super.foo等同于Object.getPrototypeOf(this).foo（属性）或Object.getPrototypeOf(this).foo.call(this)（方法）。`

```node
const proto = {
  x: 'hello',
  foo() {
    console.log(this.x);
  },
};

const obj = {
  x: 'world',
  foo() {
    super.foo();
  }
}

Object.setPrototypeOf(obj, proto);

obj.foo() // "world"
```
`上面代码中，super.foo指向原型对象proto的foo方法，但是绑定的this却还是当前对象obj，因此输出的就是world。`

##### [6.链判断运算符 和 Null 判断运算符](#top) <b id="target6"></b>
`编程实务中，如果读取对象内部的某个属性，往往需要判断一下该对象是否存在。比如，要读取message.body.user.firstName，安全的写法是写成下面这样。`

```node
const firstName = (message
  && message.body
  && message.body.user
  && message.body.user.firstName) || 'default';
```

`这样的层层判断非常麻烦，因此 ES2020 引入了“链判断运算符”（optional chaining operator）?.，简化上面的写法。`

```node
const firstName = message?.body?.user?.firstName || 'default';
const fooValue = myForm.querySelector('input[name=foo]')?.value
```

`下面是判断对象方法是否存在，如果存在就立即执行的例子。`

```node
iterator.return?.();
```
##### Null 判断运算符
`ES2020 引入了一个新的 Null 判断运算符??。它的行为类似||，但是只有运算符左侧的值为null或undefined时，才会返回右侧的值。`

```node
const headerText = response.settings.headerText ?? 'Hello, world!';
const animationDuration = response.settings.animationDuration ?? 300;
const showSplashScreen = response.settings.showSplashScreen ?? true;
```

##### [7.对象的新增方法](#top) <b id="target7"></b>
`介绍 Object 对象的新增方法。`

##### Object.is() 
`ES5 比较两个值是否相等，只有两个运算符：相等运算符（==）和严格相等运算符（===）。它们都有缺点，前者会自动转换数据类型，后者的NaN不等于自身，以及+0等于-0。JavaScript 缺乏一种运算，在所有环境中，只要两个值是一样的，它们就应该相等。`

`ES6 提出“Same-value equality”（同值相等）算法，用来解决这个问题。Object.is就是部署这个算法的新方法。它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。`

```node
Object.is('foo', 'foo')
// true
Object.is({}, {})
// false

+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

##### Object.assign()
`Object.assign方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。`

```node
const target = { a: 1 };

const source1 = { b: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```
`注意，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。`


```node
const target = { a: 1, b: 1 };

const source1 = { b: 2, c: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

`其他类型的值（即数值、字符串和布尔值）不在首参数，也不会报错。但是，除了字符串会以数组形式，拷贝入目标对象，其他值都不会产生效果。`

```node
const v1 = 'abc';
const v2 = true;
const v3 = 10;

const obj = Object.assign({}, v1, v2, v3);
console.log(obj); // { "0": "a", "1": "b", "2": "c" }
```
`Object.assign拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（enumerable: false）。`

```node
Object.assign({b: 'c'},
  Object.defineProperty({}, 'invisible', {
    enumerable: false,
    value: 'hello'
  })
)
// { b: 'c' }
```

`Object.assign方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。`

```node
const obj1 = {a: {b: 1}};
const obj2 = Object.assign({}, obj1);

obj1.a.b = 2;
obj2.a.b // 2
```

`Object.assign可以用来处理数组，但是会把数组视为对象。`

```node
Object.assign([1, 2, 3], [4, 5])
// [4, 5, 3]
```

##### getOwnPropertyDescriptor()
`ES5 的Object.getOwnPropertyDescriptor()方法会返回某个对象属性的描述对象（descriptor）。ES2017 引入了Object.getOwnPropertyDescriptors()方法，返回指定对象所有自身属性（非继承属性）的描述对象。`

```node
const obj = {
  foo: 123,
  get bar() { return 'abc' }
};

Object.getOwnPropertyDescriptors(obj);
// { foo:
//    { value: 123,
//      writable: true,
//      enumerable: true,
//      configurable: true },
//   bar:
//    { get: [Function: get bar],
//      set: undefined,
//      enumerable: true,
//      configurable: true } }
```

`上面代码中，Object.getOwnPropertyDescriptors()方法返回一个对象，所有原对象的属性名都是该对象的属性名，对应的属性值就是该属性的描述对象。该方法的实现非常容易。`

```node
function getOwnPropertyDescriptors(obj) {
  const result = {};
  for (let key of Reflect.ownKeys(obj)) {
    result[key] = Object.getOwnPropertyDescriptor(obj, key);
  }
  return result;
}
```

##### __proto__属性，Object.setPrototypeOf()，Object.getPrototypeOf()

`__proto__属性（前后各两个下划线），用来读取或设置当前对象的prototype对象。目前，所有浏览器（包括 IE11）都部署了这个属性。`

```node
// es5 的写法
const obj = {
  method: function() { ... }
};
obj.__proto__ = someOtherObj;

// es6 的写法
var obj = Object.create(someOtherObj);
obj.method = function() { ... };
```


--------------------
`作者:` `蒋星 JxKicker` 
`完成时间`:`2020年2月20日16:25:19`
`备注信息`: `任意使用` 
