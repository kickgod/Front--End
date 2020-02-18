### [ECMASCript6  数值的扩展](#top)  <b id="top"></b>
`ES6 对于数值也提供了扩展`
- [x]  [`二进制和八进制表示法`](#doubleeight) 
- [x]  [`Number 扩展方法`](#number) 
- [x]  [`Math 对象的扩展`](#math) 
- [x]  [`新增运算符`](#yunsuan) 
- [x]  [`BigInt 对象`](#bigint) 

#####   [二进制和八进制表示法](#top) <b id="doubleeight"></b>
`ES6 提供了二进制和八进制数值的新的写法，分别用前缀0b（或0B）和0o（或0O）表示。` `是` **`零`** `不是` `欧`
* `二进制`:`Binary`
* `八进制`:`Octal`
* `十六进制`:`Hexadecimal`
```node
0b111110111 === 503 // true
0o767 === 503 // true

Number('0b111')  // 7
Number('0o10')  // 8
```
#####   [Number 扩展方法](#top) <b id="number"></b> 
`ES6 在Number对象上，新提供了Number.isFinite()和Number.isNaN()两个方法。`
`ES6 将全局方法parseInt()和parseFloat()，移植到Number对象上面，行为完全保持不变。`
`ES6 在Number对象上面，新增一个极小的常量Number.EPSILON。根据规格`
* `Number.isFinite()用来检查一个数值是否为有限的（finite），即不是Infinity。`
* `Number.isNaN()用来检查一个值是否为NaN。`
* `Number.parseInt() `
* `Number.parseFloat() `
```node
Number.parseInt === parseInt // true
Number.parseFloat === parseFloat // true
```
* `Number.isInteger()`：`新增方法 判断一个数是否是整数` 
* `Number.EPSILON`：`它表示 1 与大于 1 的最小浮点数之间的差。`
* `Number.isSafeInteger()`:`安全整数 JavaScript 能够准确表示的整数范围在-2^53到2^53之间（不含两个端点），超过这个范围，无法精确表示这个值。`

##### Number.EPSILON
`ES6 在Number对象上面，新增一个极小的常量Number.EPSILON。根据规格，它表示 1 与大于 1 的最小浮点数之间的差。`

`对于 64 位浮点数来说，大于 1 的最小浮点数相当于二进制的1.00..001，小数点后面有连续 51 个零。这个值减去 1 之后，就等于 2 的 -52 次方。`
```node
Number.EPSILON === Math.pow(2, -52)
// true
Number.EPSILON
// 2.220446049250313e-16
Number.EPSILON.toFixed(20)
// "0.00000000000000022204"
```
#####  [Math 对象的扩展](#top) <b id="math"></b>
`Math.trunc() 方法用于去除一个数的小数部分，返回整数部分。`
```node
Math.trunc(4.9) // 4
Math.trunc(-4.1) // -4
```
`Math.sign() 方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。`
* `它会返回五种值。`
   * `参数为正数，返回+1;`
   * `参数为负数，返回-1;`
   * `参数为 0，返回0;`
   * `参数为-0，返回-0;`
   * `其他值，返回NaN。`
```node
Math.sign(-5) // -1
Math.sign(5) // +1
Math.sign(0) // +0
Math.sign(-0) // -0
Math.sign(NaN) // NaN
```
`Math.cbrt() 方法用于计算一个数的立方根。`
```node
Math.cbrt(-1) // -1
Math.cbrt(0)  // 0
Math.cbrt(1)  // 1
Math.cbrt(2)  // 1.2599210498948734

Math.cbrt = Math.cbrt || function(x) {
  var y = Math.pow(Math.abs(x), 1/3);
  return x < 0 ? -y : y;
};
```

`Math.clz32()` `Math.clz32()方法将参数转为 32 位无符号整数的形式，然后返回这个 32 位值里面有多少个前导 0`

```node
Math.clz32(0) // 32
Math.clz32(1) // 31
Math.clz32(1000) // 22
Math.clz32(0b01000000000000000000000000000000) // 1
Math.clz32(0b00100000000000000000000000000000) // 2
```

#####  [新增运算符](#top) <b id="yunsuan"></b>
`ES2016 新增了一个指数运算符（**）。`
```node
2 ** 2 // 4
2 ** 3 // 8

let b = 4;
b **= 3;
// 等同于 b = b * b * b;
```

#####  [BigInt 对象](#top) <b id="bigint"></b>
`JavaScript 原生提供BigInt对象，可以用作构造函数生成 BigInt 类型的数值。转换规则基本与Number()一致，将其他类型的值转为 BigInt。`

```node
BigInt(123) // 123n
BigInt('123') // 123n
BigInt(false) // 0n
BigInt(true) // 1n
```

`BigInt()构造函数必须有参数，而且参数必须可以正常转为数值，下面的用法都会报错。`

```node
new BigInt() // TypeError
BigInt(undefined) //TypeError
BigInt(null) // TypeError
BigInt('123n') // SyntaxError
BigInt('abc') // SyntaxError

/* 参数如果是小数，也会报错。*/

BigInt(1.5) // RangeError
BigInt('1.5') // SyntaxError
```

`上面代码中，尤其值得注意字符串123n无法解析成 Number 类型，所以会报错。`

* `BigInt.asUintN(width, BigInt)`： `给定的 BigInt 转为 0 到 2width - 1 之间对应的值。`
* `BigInt.asIntN(width, BigInt)`：`给定的 BigInt 转为 -2width - 1 到 2width - 1 - 1 之间对应的值。`
* `BigInt.parseInt(string[, radix])`：`近似于Number.parseInt()，将一个字符串转换成指定进制的 BigInt。`

```node
const max = 2n ** (64n - 1n) - 1n;

BigInt.asIntN(64, max)
// 9223372036854775807n
BigInt.asIntN(64, max + 1n)
// -9223372036854775808n
BigInt.asUintN(64, max + 1n)
// 9223372036854775808n
```




