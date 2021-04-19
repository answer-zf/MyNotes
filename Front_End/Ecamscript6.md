# Ecamscript6

## 前言

- ECMAScript 6.0（以下简称 ES6）是 JavaScript 语言的下一代标准，已经在 2015 年 6 月正式发布了。
- Ecmascript 是 JavaScript 语言的标注规范
- JavaScript 是 Ecmascript 规范的具体实现
  - 具体实现取决于各大浏览器厂商的支持进度
- Ecmascript 6 也被称作 Ecmascript 2015
- 各大浏览器厂商对于最新的 Ecmascript 6 标准支持可以参照：
  - http://kangax.github.io/compat-table/es6/
- 对于不支持 ES6 的环境，可以使用一些编译转码工具做转换处理再使用
  - 例如 babel

## let 和 const

let：

- let 类似于 var，用来声明变量
- 通过 let 声明的变量不同于 var，只在 let 命令所在的代码块内有效（块级作用域）
- let 声明的变量不存在变量提升
- let 不允许在相同作用域内，重复声明同一个变量

const：

- const 声明一个只读的常量。一旦声明，常量的值就不能改变
- const 声明必须初始化
- const 的作用域与 let 命令相同：只在声明所在的块级作用域内有效
- const 命令声明的常量也是不提升，必须先声明后使用
- const 声明的常量，也与 let 一样不可重复声明

## 解构赋值

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

数组解构：

```js
let [a, b, c] = [123, 456, 789]
console.log(a, b, c) 123 456 789
```

对象解构：

```js
let { name, age } = { name: 'Jack', age: 18 }
console.log(name, age) Jack 18
```

函数参数解构：

```js
function f (p1, { p2 = 'aa', p3 = 'bb' }) {
  console.log(p1, p2, p3)
}

f('p1', { p2: 'p2' }) p1 p2 bb
```

字符串解构：

```js
let [a, b, c, d, e] = 'hello'
console.log(a, b, c, d, e) h e l l o
```

## 字符串

实用方法：

```js
includes(String) // 返回布尔值，表示是否找到了参数字符串。
startsWith(String) // 返回布尔值，表示参数字符串是否在源字符串的头部。
endsWith(String) // 返回布尔值，表示参数字符串是否在源字符串的尾部。
repeat(Number) // repeat方法需要指定一个数值，然后返回一个新字符串，表示将原字符串重复Number次。
padStart(num, str) // 如果某个字符串不够指定长度，会在头部补全,num:字符串长度，str：用来补全的字符串
padEnd(num, str) // 如果某个字符串不够指定长度，会在尾部补全
```

模板字符串：

```js
let basket = { count: 5, onSale: true }
$('#result').append(`
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
`)
```

- 模板字符串（template string）是增强版的字符串，用反引号（`）标识
- 它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量
- 如果使用模板字符串表示多行字符串，所有的空格和缩进都会被保留在输出之中
- 模板字符串中嵌入变量，需要将变量名写在 `${}` 之中
  - 大括号内部可以放入任意的 JavaScript 表达式，可以进行运算，以及引用对象属性
  - 大括号内部还可以调用函数

## 数组

方法：

```js
Array.from() // 将一个伪数组转为一个真正的数组
// 实际应用中，常见的类似数组的对象是DOM操作返回的NodeList集合，
// 以及函数内部的arguments对象。Array.from都可以将它们转为真正的数组。
Array.of() // Array.of方法用于将一组值，转换为数组
// 这个方法的主要目的，是弥补数组构造函数Array()的不足。
// 因为参数个数的不同，会导致Array()的行为有差异。
find() // 查找数组中某个元素
findIndex() // 查找数组中某个元素的索引下标
filter() // 创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素
includes() // 返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似
```

实例方法：

ES6 提供三个新的方法——entries()，keys()和 values()——用于遍历数组.
可以用 `for...of` 循环进行遍历，唯一的区别是 `keys()` 是对键名的遍历、
`values()` 是对键值的遍历，`entries()` 是对键值对的遍历。

```js
let arr = ['a', 'b', 'c']
console.log(arr.keys())
for (let index of arr.keys()) {
  console.log(index)
}
// 0
// 1
// 2

for (let item of arr.values()) {
  console.log(item)
}
// 'a'
// 'b'
// 'c'

for (let [index, item] of arr.entries()) {
  console.log(index, item)
}
// 0 'a'
// 1 'b'
// 2 'c'
```

扩展运算符：

```js
console.log(...[1, 2, 3]) 1 2 3
console.log(1, ...[2, 3, 4], 5) 1 2 3 4 5

// 合并数组
let arr1 = [123, 456]
let arr2 = [789, 890]
console.log([...arr1, ...arr2])   // [ 123, 456, 789, 890 ]
```

### find 详解

方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined

#### 语法：

```js
arr.find(callback)
```

#### 参数：

callback

在数组每一项上执行的函数，接收 3 个参数：

- `element` 当前遍历到的元素。
- `index`（可选）当前遍历到的索引。
- `array`（可选）数组本身。

#### 返回值：

数组中第一个满足所提供测试函数的元素的值，否则返回 `undefined`。

#### 实例：

```js
var array = [5, 12, 8, 130, 44]

var found = array.find(function(item) {
  return item === 130
})

console.log(found) // 结果：130
```

#### 底层原理

```js
// find 接收一个方法作为参数，方法内部返回一个条件
// find 会遍历所有的元素，执行你给定的带有条件返回值的函数
// 符合该条件的元素会作为 find 方法的返回值
// 如果遍历结束还没有符合该条件的元素，则返回 undefined
Array.prototype.myFind = function(conditionFunc) {
  for (var i = 0; i < this.length; i++) {
    if (conditionFunc(this[i], i)) {
      return this[i]
    }
  }
}

var ret = users.myFind(function(item, index) {
  return item.id === 2
})
```

every -- 方法测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值。

some -- 方法测试数组中是不是有元素通过了被提供的函数测试。它返回的是一个 Boolean 类型的值。

findIndex-- 方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)。

includes -- 方法返回数组中满足提供的测试函数的第一个元素的索引。否则返回-1。

map -- 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

reduce -- 方法对数组中的每个元素执行一个由您提供的**reducer**函数(升序执行)，将其结果汇总为单个返回值

## 函数的扩展

### 函数参数的默认值：

```js
ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。

function log(x, y = 'World') {
  console.log(x, y);
}

log('Hello') Hello World
log('Hello', 'China') Hello China
log('Hello', '') Hello
```

- 通常情况下，定义了默认值的参数，应该是函数的尾参数
  - 因为这样比较容易看出来，到底省略了哪些参数
  - 如果非尾部的参数设置默认值，实际上这个参数是没法省略的。
- 指定了默认值以后，函数的 length 属性，将返回没有指定默认值的参数个数
  - 也就是说，指定了默认值后，length 属性将失真

### rest 参数：

```js

f(1, 2, 3)

function f(x, y, z) {
  console.log(Array.from(arguments)) // [ 1, 2, 3 ]
}
// ...args 叫做 rest 参数
// ...args 是一种语法，...不能省略，args 是随便起的一个名字
// ...args 可以将用户传递进来的参数包装到一个数组中
function f(...args) {
  console.log(args) // [ 1, 2, 3 ]
}

// ...args 是把用户传递参数的没有对应到具体形参的变量解析到一个数组中
function f(a, ...args) {
  console.log(args)  // [2, 3]
}

function add(...values) {
  let sum = 0;

  for (var val of values) {
    sum += val;
  }

  return sum;
}

add(2, 5, 3) 10
```

### 箭头函数：

```js
var f = v => v

上面的箭头函数等同于：

var f = function(v) {
  return v
}

let foo = (x, y) => {
  console.log(x, y)
  return x + y
}
```

- 箭头函数体内的 this 对象，就是定义时所在(父级作用域)的对象，而不是使用时所在的对象
- 箭头函数不可以当作构造函数，也就是说，不可以使用 new 命令，否则会抛出一个错误
- 箭头函数内部不可以使用 arguments 对象，该对象在函数体内不存在
  - 如果要用，可以用 Rest 参数代替

使用场景：

- 箭头函数最好都用在匿名函数的地方

### 对象

属性的简洁表示法：

```js
var foo = 'bar';
var baz = {foo};
baz {foo: "bar"}

等同于
var baz = {foo: foo}

除了属性简写，方法也可以简写:
var o = {
  method() {
    return "Hello!"
  }
}

等同于

var o = {
  method: function() {
    return "Hello!"
  }
}
```

### babel

详细配置使用方式请见：[ECMAScript 6 简介](http://es6.ruanyifeng.com/#docs/intro#Babel转码器)

在项目根目录下创建一个 `.babelrc` 文件，写入以下内容：

```json
{
  "presets": ["@babel/env", "@babel/preset-react"],
  "plugins": []
}
```

在项目中安装 转码规则、依赖等：

```shell
$ npm install --save-dev @babel/cli @babel/core @babel/preset-env @babel/preset-react
```

使用：（后面的操作，上面的配置必不可少）

- `babel/cli` 命令行转码
- `babel/core` 如果某些代码需要调用 Babel 的 API 进行转码，就要使用 `babel/core` 模块

- `babel/register` ：**使用 babel/register 作为开发的编译转换环境，可以在代码运行的过程中实时编译转换**

  - 在项目目录中，安装 `babel/register`

    - `$ npm install --save-dev @babel/register`

  - 添加钩子文件（main.js）

    ```js
    require('babel-register') // 加载一次 babel-register
    require('核心功能代码入口文件模块') // 指定要加载的自己的入口文件模块

    // node 钩子文件  index.js 可以实时转码
    ```

  - 使用 node 执行 钩子文件（main.js）,而不是入口文件。

##面向对象编程

`class` 关键字，是 ES6 中提供的新语法，是用来 实现 ES6 中面向对象编程的方式

`Java` `C#` 实现面向对象的方式完全一样了， `class` 是从后端语言中借鉴过来的， 来实现面向对象

```js
class Person {
  // 使用 static 关键字，可以定义静态属性
  static info = { name: 'zs', age: 20 }
}
```

静态属性：

- 可以直接通过 类名， 直接访问的属性

  - `console.log(Person.info)`

实例属性：

- 只能通过类的实例，来访问的属性

  - `var p1 = new Person()`
  - `console.log(p1.name)`

`js` 中实现：

```js
function Animal(name) {
  this.name = name
}
Animal.info = 123
var a1 = new Animal('小花')
// 这是静态属性：
console.log(Animal.info)
// 这是实例属性：
console.log(a1.name)
```

## Module

模块功能主要由两个命令构成：`export` 和 `import`。`export` 命令用于规定模块的对外接口，`import` 命令用于输入其他模块提供的功能。

### 前提

以下实例使用的目录结构：

```js

## └─ducument
##    ├─main.js
##    └─index.js

// main.js 加载 index.js
// export  实例书写在 ：index.js
// import  实例书写在 ：main.js
```

### export

```js
export const foo = 'bar'
export function f() {}

// 或者

const foo = 'bar'
function f() {}
export { foo, f }
```

### export default

```js
const foo = 'bar'
function f() {}

export default {
  foo,
  f
}
```

### import

通过 `export` 导出的成员：

- 必须通过解构赋值按需加载

  - `import {f, foo} from './main'`

- 通过 `* as 变量名` 的形式加载所有通过 export 关键字导出的接口成员

  - `import * as com from './main'`

- `export`可以向外暴露多个成员， 同时，如果某些成员，我们在 `import` 的时候，不需要，则可以 不在`{}` 中定义
- 使用 `export`导出的成员，必须严格按照 导出时候的名称，来使用 `{}` 按需接收；
- 使用 `export`导出的成员，如果 就想 换个 名称来接收，可以使用 `as` 来起别名；

通过 `export default` 导出的成员：

- 常规加载
  - `import main from './main'`
- 使用 `export default` 向外暴露的成员，可以使用任意的变量来接收
- 在一个模块中， `export default` 只允许向外暴露 1 次

在一个模块中，可以同时使用 `export default` 和 `export`向外暴露成员

## 异步编程

### 回调函数：获取异步操作结果

~~不成立情况：~~

```js
function add(x, y) {
  console.log(1)
  setTimeout(function() {
    console.log(2)
    var ret = x + y
    return ret
  }, 1000)
  console.log(3)
  // 到这里执行结束，不会等到前面的定时器，所以直接返回默认值 undefined
}
console.log(add(10, 20)) // => undefined
```

~~不成立情况：~~

```js
function add(x, y) {
  var ret
  console.log(1)
  setTimeout(function() {
    console.log(2)
    var ret = x + y
  }, 1000)
  console.log(3)
  return ret
}
console.log(add(10, 20)) // => undefined
```

**如果需要获取一个函数异步操作的结果，必须使用回调函数来获取**

```js
function add(x, y, callback) {
  // callback 就是回调函数
  setTimeout(function() {
    var ret = x + y
    callback(ret) // ret -> 实参
  }, 1000)
}
add(10, 20, function(ret) {
  // ret -> 形参
  console.log(ret)
})
```

基于原生 XMLHTTPRequest 封装 get 方法

```js
function get(url, callback) {
  var oReq = new XMLHttpRequest()
  // 当请求加载成功之后要调用指定的函数
  oReq.onload = function() {
    callback(oReq.responseText)
  }
  oReq.open('get', url, true)
  oReq.send()
}

get('data.json', function(data) {
  console.log(data)
})
```

- 异步 API 一般都 伴随着回调函数(上层定义，下层调用)

  - setTimeout
  - readFile
  - writeFile
  - readdir
  - ajax

- a 链接默认是同步请求

### Promise

> 参考文档：http://es6.ruanyifeng.com/#docs/promise

#### 前提

callbackhell：

![callbackhell](http://images.dorc.top/blog/Ecamscript6/callbackhell.jpg)

无法保证顺序的代码：

```js
var fs = require('fs')

fs.readFile('./data/a.txt', 'utf8', function(err, data) {
  if (err) {
    // return console.log('读取失败')
    // 抛出异常（做测试的时候经常使用）
    //    1. 阻止程序的执行 （程序奔溃直接退出）
    //    2. 把错误消息打印到控制台
    throw err
  }
  console.log(data)
})

fs.readFile('./data/b.txt', 'utf8', function(err, data) {
  if (err) {
    throw err
  }
  console.log(data)
})

fs.readFile('./data/c.txt', 'utf8', function(err, data) {
  if (err) {
    throw err
  }
  console.log(data)
})
```

通过回调嵌套的方式来保证顺序：

```js
var fs = require('fs')

fs.readFile('./data/a.txt', 'utf8', function(err, data) {
  if (err) {
    throw err
  }
  console.log(data)
  fs.readFile('./data/b.txt', 'utf8', function(err, data) {
    if (err) {
      throw err
    }
    console.log(data)
    fs.readFile('./data/c.txt', 'utf8', function(err, data) {
      if (err) {
        throw err
      }
      console.log(data)
    })
  })
})
```

为了解决以上编码方式带来的问题（回调地狱嵌套），在 Ecamscript 6 中新增了一个 API：`Promise`

#### Promise 基本语法

- Promise - 承诺、保证

原理：

1. Promise 是一个 构造函数，既然是构造函数， 那么，我们就可以 new Promise() 得到一个 Promise 的实例；
2. 在 Promise 上，有两个函数，分别叫做 resolve（成功之后的回调函数） 和 reject（失败之后的回调函数）
3. 在 Promise 构造函数的 Prototype 属性上，有一个 .then() 方法，也就说，只要是 Promise 构造函数创建的实例，都可以访问到 .then() 方法
4. Promise 表示一个 异步操作；每当我们 new 一个 Promise 的实例，这个实例，就表示一个具体的异步操作；
5. 既然 Promise 创建的实例，是一个异步操作，那么，这个 异步操作的结果，只能有两种状态：
   5.1 状态 1： 异步执行成功了，需要在内部调用 成功的回调函数 resolve 把结果返回给调用者；
   5.2 状态 2： 异步执行失败了，需要在内部调用 失败的回调函数 reject 把结果返回给调用者；
   5.3 由于 Promise 的实例，是一个异步操作，所以，内部拿到 操作的结果后，无法使用 return 把操作的结果返回给调用者； 这时候，只能使用回调函数的形式，来把 成功 或 失败的结果，返回给调用者；

```js
/**
 * Promise 在 Ecmascript 6 中体现出来就是一个对象
 * Promise 是一个容器
 * 一般用来封装一个异步操作
 *   异步操作是一个无法预测的事情，要吗成功，要吗失败
 * 容器内部有三种状态：
 *     pending  正在处理
 *     resolved 成功，已解决
 *     rejected 驳回，失败
 */

const fs = require('fs')

// Promise 对象一经创建，立即执行
new Promise((resolve, reject) => {
  fs.readFile('./data/a.txt', (err, data) => {
    if (err) {
      // 当 Promise 对象内部的异步操作结果失败的时候，告诉 Promise 对象容器，该异步任务失败了
      // 其实就是将 Promise 内部的 Pending 状态改为 Rejected
      reject(err)
    }
    // 当代码执行到这里，说明该 Promise 对象内部的异步操作没有错误发生，证明成功了
    // 然后在这里将 Promise 内部的 Pending 状态改为 Resolved
    // resolve 不能传入多数据，需用[]/{}
    resolve(data)
  })
})
  // Promise 实例对象有一个方法：then 方法
  // then 需要传递两个回调处理函数
  // 其中第一个回调处理函数就是 Promise 对象内部的  resolve 函数
  // 第二个回调处理函数是可选的，如果传递则就是 Promise 对象内部的 reject 函数
  .then(
    data => {
      console.log(111)
      console.log(data.toString())
      return new Promise((resolve, reject) => {
        fs.readFile('./data/b.txt', (err, data) => {
          if (err) {
            // 这里没有使用 return 的原因就是 Promise 的状态只能从 Pending 变为 Resolve 或者 Rejected
            // 状态一旦改变，就不会再发生变化
            reject(err)
          }
          resolve(data)
        })
      })
    },
    err => {
      console.log('读取文件失败了')
    }
  )
  // then 方法之后可以继续链式调用 then
  // 后续的每一个 then 中指定的回调处理函数都会被执行
  // 后续的 then 中指定的回调处理函数可以接收上一个 then 中指定的成功的回调处理函数的返回结果
  //    1. 没有返回值，默认就是 undefined
  //    2. 有普通的返回值，数字、字符串、对象、数组。。。
  //    3. 返回一个新的 Promise 对象
  .then(data => {
    console.log(222)
    console.log(data.toString())
    return new Promise((resolve, reject) => {
      fs.readFile('./data/c.txt', (err, data) => {
        if (err) {
          // 这里没有使用 return 的原因就是 Promise 的状态只能从 Pending 变为 Resolve 或者 Rejected
          // 状态一旦改变，就不会再发生变化
          reject(err)
        }
        resolve(data)
      })
    })
  })
  .then(data => {
    console.log(333)
    // 二进制数据调用 toString() 方法可以转换为普通的字符，默认就是 utf8 编码
    console.log(data.toString())
  })
```

简化：

```js

var fs = require('fs')
// promise是一个构造函数
// 不是异步，但里面往往封装一个异步任务

// 创建 Promise 容器
// Promise 容器一旦创建，就开始执行里面的代码
// 每当 new 一个 Promise 实例的时候，就会立即 执行这个 异步操作中的代码
// 也就是说，new 的时候，除了能够得到 一个 promise 实例之外，还会立即调用 我们为 Promise 构造函数传递的那个 function，执行这个 function 中的 异步操作代码；
var p1 = new Promise(function(resolve, reject) {
  fs.readFile('./data/a.txt', 'utf8', function(err, data) {  ## 异步任务
    if (err) {
      // 承诺容器中的任务失败，
      // console.log(err)
      // 把容器中的 Pending 状态变为 rejected
      // 调用 reject 就相当于调用了 then 方法的第二个参数
      reject(err)  ## 失败调用
    } else {
      // 承诺容器中的任务成功，
      // console.log(data)
      // 把容器中的 Pending 状态变为 resolved
      // 调用 resolve 就相当于调用了 then 方法的传递的那个function
      resolve(data)  ## 成功调用
    }
  })
})

// 当 p1 成功了 然后（then） 做指定操作
// then 方法接收的 function 就是容器中的 resolve 函数
p1.then(
  function(data) {
    console.log(data)
  },
  function(err) {
    console.log(err)
  }
)

```

#### Promise 的封装

实例推导（ readFile ）

- 异步调用链式编程

```js
var fs = require('fs')

var p1 = new Promise(function(resolve, reject) {
  fs.readFile('./data/a.txt', 'utf8', function(err, data) {
    if (err) {
      reject(err)
    } else {
      resolve(data)
    }
  })
})
var p2 = /...
var p3 = /...
p1.then(
  function(data) {
    console.log(data)
    // 当 p1 读取成功的时候
    // 当前函数中 return 的结果就可以在后面的 then 中 function 接收到，故：
    // 当 return 123 后面就接收到 123
    // 没有 return 后面就接收的是 undefined
    // 同理可以 return 一个 Promise 对象
    // 当 return 一个Promise 对象的时候，后续的then中的 方法的第一个参数会作为p2 的 resolve
    return p2
  },
  function(err) {
    console.log(err)
  }
)
  .then(function(data) {
    console.log(data)
    return p3
  })
  .then(function(data) {
    console.log(data)
    console.log('end')
  })
```

![Snipaste_2019-10-19_15-40-57](http://images.dorc.top/blog/Ecamscript6/Snipaste_2019-10-19_15-40-57.jpg)

=> 封装实例 promise

```js
var fs = require('fs')

function pReadFile(filePath) {
  // 1 定义函数
  var promise = new Promise(function(resolve, reject) {
    // 3 new Promise 实例 并 立即执行方法 (异步执行中)
    fs.readFile(filePath, 'utf8', function(err, data) {
      // 7 读取文件
      if (err) {
        reject(err)
      } else {
        resolve(data)
      }
    })
  })
  return promise // 4 瞬间返回 promise 实例 (异步未执行完)
}
var p = pReadFile('./data/a.txt') // 2 主程序执行方法
// 5 得到 promise 实例 (异步未执行完)
p.then(function(data) {
  // 6 瞬间通过 .then 指定 resolve reject 的回调 (先于第7步执行完，所以必定能访问到 resolve reject )
  console.log(data)
  return pReadFile('./data/b.txt')
})
  .then(function(data) {
    console.log(data)
    return pReadFile('./data/c.txt')
  })
  .then(function(data) {
    console.log(data)
  })
```

=> Ecmascript 封装 promise

```js
const fs = require('fs')

readFile('./data/a.txt', 'utf8')
  .then(data => {
    console.log(data)
    return readFile('./data/b.txt', 'utf8')
  })
  .then(data => {
    console.log(data)
    return readFile('./data/c.txt', 'utf8')
  })
  .then(data => {
    console.log(data)
  })

function readFile(...args) {
  return new Promise((resolve, reject) => {
    fs.readFile(...args, (err, data) => {
      if (err) {
        reject(err)
      }
      resolve(data)
    })
  })
}
```

#### 错误处理 catch

在使用 `Promise` 做异步流程控制的时候，
关于异常的处理可以通过在最后一个 `then` 之后设置一个 `catch` ，然后指定一个失败处理函数
该函数可以捕获前面所有的 `Promise` 对象本身以及 `then` 内部的任务错误
当前面任何一个发生异常，直接进入 `catch`，后续所有的 `Promise` 包括 `then` 不再执行

```js
const fs = require('fs')

readFile('./data/d.txt', 'utf8')
  .then(data => {
    console.log(data)
    return readFile('./data/b.txt', 'utf8')
  })
  .then(data => {
    console.log(data)
    return readFile('./data/c.txt', 'utf8')
  })
  .then(data => {
    console.log(data)
  })
  .catch(err => {
    console.log(err)
  })

function readFile(...args) {
  return new Promise((resolve, reject) => {
    fs.readFile(...args, (err, data) => {
      if (err) {
        reject(err)
      }
      resolve(data)
    })
  })
}
```

当使用 `then` 的第二个参数单独设置异常处理函数之后，无法阻止整体的 `promise` 处理流程的执行。
一般不添加 `then` 的第二个参数， 交给 `catch` 统一处理。（程序级别的错误统一处理）
