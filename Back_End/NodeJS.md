# Node.js

## Node.js 概述

- Node.js 是 JavaScript 运行时环境

- 可以解析执行 JavaScript 代码

- 没有 BOM 、DOM

- 遵循 EcmaScript

- 为 JavaScript 提供了服务器级别的操作 API

- 构建与 Chrome 的 V8 引擎之上

  - Google Chrome 中的 V8 引擎世界上公认的解析执行 JavaScript 代码最快的

  - Node.js 作者把 Google Chrome 中的 V8 引擎移出来，开发了独立的 JavaScript 运行时环境

## Node.js 特性

- event-driven 事件驱动

- non-blocking I/O model 非阻塞 IO 模型（异步）

- lightweight and efficient 轻量和高效

## Node.js 功能

- WEB 服务器后台

  - B/S 编程模型（与语言无关）
  - 模块化编程 （类似 less @import('文件路径') 引用加载文件）
  - 异步编程

    - promise
    - async
    - generator

  - Express Web 开发框架
  - Ecmascript 6

- 命令行工具

  - git （ C ）
  - npm（ Node ）
  - hexo（ Node ）

## Node.js 基本操作

### 执行文件

```shell

## 创建编写js脚本文件
## 打开终端，定位到脚本文件所属目录
## 输入node '文件名' 执行对应的文件  -- 文件名不能以node.js命名否则会打开这个文件

$node begin.js

```

### 读取文件

```js

// 执行文件操作必须引入fs这个核心模块（file-system）
// 使用 require 方法载入fs核心模块

var fs = require('fs')

fs.readFile('url',function(error,data){   // URL：要读取的文件路径 （统一资源定位符）
	if(error){
	 	console.log('读取错误')
        return
    }
    console.log(data.toString())
})
// 读取成功 error 返回 null   ，data 返回 数据
// 读取失败 error 返回 错误对象，data 返回 undefined

## ps: data 返回的数据是将文件存储的二进制数据 转为 十六进制数据，展现
##	   可以用 toString 方法转为 字符串

## readFile 的第二个参数是可选的，传入 utf8 就是告诉他把读取到的文件直接按照 utf8 编码转成字符串
## 等价于 data.toString()
```

### 读取目录

```javascript
var fs = require('fs')
fs.readdir('url', function(err, files) {
  // files: 返回数组
  if (err) {
    res.end(err.message) // err对象 中有一个属性 message
    return
  }
  console.log(files)
})
```

### 写入文件

```js

var fs = require('fs')
fs.writeFile('url','content',function(error){ // content: 写入文件内容   error: 形参
     if(error){
		console.log('写入失败')
     }else{
        console.log('写入成功')
     }
})

## 写入成功 error 返回 null
## 写入失败 error 返回 错误对象

```

### 将查询字符串转为对象

```js
const queryString = require('querystring')

queryString.parse('查询字符串')
```

### 创建服务器

```javascript

// Node中有一个核心模块 http ,职责创建编写服务器

// 加载http核心模块
var http = require('http')

// 使用http.createServer() 方法创建web服务    ## 返回一个Server实例
var server = http.createServer()

// 接受请求
// 处理请求
// 返回响应

// 注册 request 请求事件
// 当客户端请求时，自动触发服务器的 request 请求事件，然后执行第二个参数：回调处理函数
server.on('request', function(request,response){
    console.log('收到请求,请求路径' + request.url)
    response.write('hello')
    response.write(' node.js')
    response.end()
  	// 简化
  	response.end('hello node.js')
})

## request  请求事件处理函数，需接收两个参数：

## Request  请求对象：用来获取当前客户端的一些请求数据或者请求报文信息
						## req.url 获取端口号以后的路径，所有url都是以 / 开头的 默认为 /
            ## req.query 用来获取查询字符串数据
            ## req.method 用来当前请求方法
## Response 响应对象：用来向当前请求客户端发送消息数据
						## writer方法 ：给客户端发送响应数据
            ## write 可以使用多次，但是最后一定要使用end结束响应，否则客户端会一直等待。
            ## 简化操作 直接end的同时发送响应数据 response.end('str')
            ## response.end()支持两种数据类型：二进制 字符串

## response.end()	一次请求对应一次响应，响应结束这次请求也结束  不执行后续代码  类似return
## response.end() 必须存在

## response.write() / response.end() 只能接受 字符串 和 二进制数据

// 绑定端口号，启动服务器。

server.listen(3000,function(){
    console.log('服务器启动成功，可以通过 http://127.0.0.1:3000/，进行访问')
})

// 此时终端被服务占用，关闭终端即关闭服务器（X掉，或者 Ctrl+c 终止），有响应便返回响应

```

#### - 创建服务简写

```js
http
  .createServer(function(req, res) {})
  .listen(3000, function() {
    console.log('Server is running')
  })
```

### 获取路径

- 采用 URL 模块，获取

```js

var url = require('url')

var obj =url.parse('http://127.0.0.1:3000/post?name=fasdf&mes= asdf', true)
														// true： 可以让里面的query 将所传入的参数转为对象
console.log(obj)

------------------------------------------
$
  protocol: 'http:',	// 协议
  slashes: true,
  auth: null,
  host: '127.0.0.1:3000',
  port: '3000',	// 端口号
  hostname: '127.0.0.1',	// 主机名
  hash: null,
  search: '?name=fasdf&mes=%20asdf',	// 查询字符串（ GET参数 ）
  query: [Object: null prototype] { name: 'fasdf', mes: ' asdf' },
  pathname: '/post',
  path: '/post?name=fasdf&mes=%20asdf',
  href: 'http://127.0.0.1:3000/post?name=fasdf&mes=%20asdf' }
```

## Node.js 中 的 JavaScript

- EcmaScript
- 核心模块
- 第三方模块
- 用户自定义模块

### 核心模块

Node 为 JavaScript 提供了很多服务器级别的 API，而且这些 API 绝大多数都被包装到了一个具名的核心模块中。他们都有自己特殊的名称标识，若要使用这些模块，必须用 **_require_** 加载模块。

- 文件操作的核心模块：fs
- 服务构建的核心模块：http
- 路径处理的核心模块：path
- 路径操作的核心模块：url
- 操作系统信息的核心模块：os
- ...

```javascript
var path = require('path')
console.log(path.extname('url'))

// 返回扩展名 .txt
```

### 用户定义模块

#### require 方法

​ **用来加载模块，并执行里面的代码**（ 可加载执行多个 JavaScript 脚本文件 ）

​ **拿到被加载文件模块导出的接口对象**

- node 中模块分三种

  - 具名的核心模块 （ fs 、http ...）

  - 用户编写的文件模块

    ​ 相对路径必须加 ./ 或 ../ （ ./ 不能省略，否则报错）

    ​ 可以省略后缀名

  - 第三方模块


    ```js
    console.log('a.js => stat')
    require('./b')
    console.log('a.js => end')
    ```

- node 中没有全局作用域，只有模块作用域（即文件作用域）

  - 模块是完全封闭的
    - 文件与文件之间可以完全避免变量命名冲突、污染问题
    - 外部访问不到内部，内部访问不到外部

#### exports 对象

**每个文件模块都提供了 _exports_ 对象 （ 默认是空对象 ）**

- 由于 node 只有模块作用域，想要做到模块间通信需要用到 **_exports_**

- 把需要被外部访问的成员手动挂载到 **_exports_** 接口对象中

- 多次在 **exports** 添加成员，实现对外导出多个内部成员

- 哪个文件 **_require_** 这个的模块，就可以得到模块内部的 **_exports_** 接口对象

  - 即：**_require_** 的返回值

  ```javascript

  ## └─ducument
  ##    ├─a.js
  ##    └─b.js

  ## ----  b.js content

  var foo = '1231234'
  exports.foo = foo

  exports.add = function (x, y) {
      return x + y
  }

  ------------------------------------------

  ## ----  a.js content

  var bExports = require('./b')
  console.log(bExports.foo)
  console.log(bExports.add(10, 210))

  ```

- 一个模块需要直接导出某个成员，而非挂载的方式必须使用

  `module.exports = add`

  - add 可为 function，string， array。。都可以

## Web 服务端开发

### IP 地址 与 端口号

- 所有联网的程序都要进行网络通信

- 计算机中只有一个物理网卡，且同一个局域网中的网卡地址必须唯一。

- 网卡是通过唯一的 ip 地址进行定位

**IP 地址用来定位计算机**

**端口号用来定位应用程序**

- 所有需要网络通信的软件都必须有端口号
- 端口号使用范围 0 ~ 65536 之间
- 计算机中有一些默认端口号 尽量不去使用 ex : 80 ..
- 一台计算机，同一个端口号在同一时间，只能被一个
- Node.js 可以开启多个服务，但是一定确保不同服务占用不同端口号

### Content-Type

- 服务端发送的数据默认，是 utf-8 编码的

- 浏览器在不知道服务器响应内容的编码的情况下，会按照当前操作系统默认的编码去解析

  - 中文操作系统默认编码是 GBK
  - 在 http 协议中 Content-Type 是用来告知，对方给你发送数据内容的数据类型
  - 图片不需要指定编码，常说的编码一般指的是：字符编码，一般只为字符数据指定编码

- **通过设置响应头的方式设置 Content-Type 的方式解决乱码问题**

  ```js
  server.on('request', function(req, res) {
    res.setHeader('Content-Type', 'text/plain; charset=utf-8')
    res.end('hello 世界')
  })
  ```

  - 服务器最好把每次响应的数据是什么内容类型 ，正确的告诉客户端
  - 不同的资源对应的 Content-Type 是不一样，具体参照：http://tool.oschina.net/commons
  - 对于文本类型的数据，最好都加上编码，目的是为了防止中文解析乱码问题

* 除了用 Content-Type 指定编码，也可以在 HTML 页面，通过 meta 元数据（用来 描述、特征、信息，存储内容的数据）来声明当前文本的编码格式

### 请求与响应

- 当浏览器收到 HTML 的响应内容以后，开始从上到下一次解析，
- 在解析过程中若发现

  - link
  - script
  - img
  - iframe
  - video
  - audio

- 等带有 src href 属性标签的时候，浏览器会自动对这些资源发起新的请求

### 统一资源管理

- 为了方便统一处理静态资源，顾将静态资源存放在同一位置
- 通过代码灵活控制那些资源能被访问，那些资源不允许访问

```js
var http = require('http')
var fs = require('fs')
http
  .createServer(function(req, res) {
    var url = req.url
    if (url === '/') {
      fs.readFile('./view/index.html', function(err, data) {
        if (err) {
          res.end('404 Not Found')
          return
        }
        res.end(data)
      })
    } else if (url.indexOf('/public/') === 0) {
      // public 开启访问权限
      fs.readFile('.' + url, function(err, data) {
        if (err) {
          res.end('404 Not Found')
          return
        }
        res.end(data)
      })
    }
  })
  .listen(3000, function() {
    console.log('Server is running')
  })
```

- 上个实例，只有 public 目录可以提供访问，灵活控制访问资源

### 服务器重定向

- 状态码设置 302 临时重定向

  - 301 为永久重定向 浏览器会记住
  - a => b ,下次请求 a，不经过 a 直接到 b
  - 302 为临时重定向 浏览器会记住
    - a => b ,下次继续请求 a，a => b
  - response.statusCode = 302

- 响应头中通过 Location 告诉客户端往哪重定向

  - response.setHeader( 'Location', '/' )

- 客户端发现收到的服务器的响应状态码是 302，会自动在响应头中找 Location，然后对改地址发起新的请求。
- 客户端自动跳转

## Node 中的模块系统

### 前 提

- 使用 Node 编写应用程序主要是使用
  - EcamScript 语言
  - 核心模块
  - 第三方模块
  - 用户自定义模块

### 模块化

- 文件作用域

- 通信规则

  - 加载

  - 导出

### CommonJS 模块规范

JavaScript 本身并不支持模块化 在 Node 中不仅支持，还有一个很重要的概念 **模块系统**

- 模块作用域
  - 默认模块中任何内容不能被外部访问
- 使用 require 方法加载模块
- 使用 exports 接口对象导出模块中的成员

#### 加载 require

##### 语法：

```js
var custom = require('module')
```

##### 作用：

1. 执行被加载模块中代码
2. 得到被加载模块中的 `exports` 导出接口对象

##### 加载规则：

模块查找机制：优先从缓存加载 => 核心模块 => 路径形式文件模块 => 第三方模块

###### 优先从缓存加载

- 优先从缓存加载，不会重复加载，目的是为了避免重复加载，提高模块加载效率

- 可以拿到其中的接口对象，但是不会重复执行里面的代码

  ```js
  ## └─ducument
  ##    ├─a.js
  ##    ├─b.js
  ##    └─main.js

  ## ----  main.js content

  require('./a')
  var fn = require('./b')
  console.log(fn)
  ----------------------------------

  ## ----  a.js content

  console.log('a.js 被加载了')
  var fn = require('./b')
  console.log(fn)
  ----------------------------------

  ## ----  b.js content

  console.log('b.js 被加载了')
  module.exports = function () {
    console.log('hello bbb')
  }
  ----------------------------------

  ## ----	 main.js输出结果
  a.js 被加载了
  b.js 被加载了
  [Function]
  [Function]
  ```

###### 判断模块模块标识(符)

**require('模块标识')**

- 核心模块
  - 本质：文件。
  - 已被编译到了二进制文件中，只需要按名字加载即可
  - 模块标识 ：模块名
- 第三方模块
  - 凡是第三方模块必须通过 npm 下载，通过 require('包名')进行加载使用
  - 不可能有一个第三方包 与 核心模块 重名
  - 模块标识 ：模块名
- 用户模块
  - 模块标识 ：路径

**路径形式的模块**：

- .js 后缀名可以省略
- ./ 当前目录 （不可省略）
- ../ 上一级目录 （不可省略）
- /xxx 绝对路径 ( 首位的 / 表示当前文件模块所属磁盘根路径) ==> 几乎不用
- d:/xxxx 绝对路径 ==> 几乎不用

**既不是核心模块，也不是路径形式的模块**

1. 模块加载规则
   - 先找到当前文件所属目录中的 `node_modules` 目录 ( 以 art-template 为例 )
   - == > node_modules/art-template
   - == > node_modules/art-template/package.json 文件
   - == > node_modules/art-template/package.json 文件中的 main 属性
   - main 属性记录了 art-template 的入口模块
   - 加载使用 art-template
   - 实际上最终加载的还是文件
2. 特殊情况
   - 如果 package.json 文件不存在或者 main 指定的入口模块也没有，则 node 会找该目录下的 index.js
     - index.js 会作为默认备选项
   - 若所述所有条件均不成立，则会进人上一级目录中的 node_modules 目录执行查找
   - 若上一级还没有，则继续往上上一级查找
   - 。。。
   - 如果直到当前磁盘根目录还找不到，最后报错 `can not find module xxx`

**在项目中有且只有一个 `node_modules` ，不会出现多个**

**位置：放在项目根目录中，这样项目中所有子目录中的代码都可以加载第三方包**

#### 导出 exports

- Node 中是模块作用域，默认文件中所有成员只在当前文件模块有效

- 想要做到模块间通信需要用到 `exports` ，把需要被外部访问的成员手动挂载到 `exports` 接口对象中

  - 导出多个成员（必须在对象中）：

    - 多次在 `exports` 添加成员，实现对外导出多个内部成员

      ```js
      exports.a = 123
      exports.b = 'string'
      exports.c =	function(){
        console.log('string')
      }
      exports.d = {
        foo = 'bar'
      }
      ```

  - 导出单个成员（拿到的是函数、字符串、数组。。。）：

    - 一个模块需要直接导出单个成员，而非挂载的方式必须使用

      ```js
      module.exports = 'string'
      ```

      ```js
      module.exports = function(x, y) {
        return x + y
      }
      ```

    - 若重复使用，则后者覆盖前者

    - 也可以用 `module.exports =` 的操作导出多个成员

      ```js
      module.exports = {
        add: function(x, y) {
          return x + y
        },
        str: 'string'
      }
      ```

##### 原理

- 在 Node 中，每一个模块内部都有一个自己的 `module` 对象

- 该 `module` 对象中，有一个成员叫： `exports` 也是一个对象（ 默认为空 ）

- 若需要对外导出成员，只需要把导出的成员挂载到 `module.exports` 中

- 由于每次导出接口成员的时候都通过 `module.exports.xxx = xxx` 比较麻烦，node 为了简化操作专门提供一个变量 `exports` 等价于 `module.exports`

  ```js
  console.log(exports === module.exports) // => true
  exports.foo = 'bar'
  //等价于
  module.exports.add = 'bar'
  ```

  固（混搭）：

  ```js
  exports.foo = 'bar'
  module.exports.add = function(x, y) {
    return x + y
  }
  -------------------(
    // require结果
    { foo: 'bar', add: [Function] }
  )
  ```

- 当一个模块需要导出单个成员的时候

  - 不能使用：`exports = 'string'`

    - `exports` 仅仅只是 `module.exports` 的引用,底层最后的代码是：
      - `var exports = module.exports`
      - `return module.exports`
    - 重新赋值不再指向 `module.exports` , 便丢失了引用关系
    - 只是快捷方式，可以忽略

  - 只能使用：`module.exports = 'string'`

    - 重新赋值以后 `exports` 便直接失效。

      1. 底层代码：`return module.exports`
      2. 将对象赋值给变量，所存放的是地址

      ```js
      module.exports = 'string'
      exports.foo = 'bar'
      -------------(
        // require结果
        'string'
      )
      ```

##### 底层代码模拟

```js
var module = {
	exports: {
	},
  ...
}
// 哪个文件 require 这个的模块，就可以得到 module.exports
// 在node最底层
// 还有一句
var exports = module.exports
// 默认在代码的最后 ：
return module.exports
```

## path 路径操作模块

> 参考文档： https://nodejs.org/dist/latest-v12.x/docs/api/path.html

### 常用 API：

- path.basename

  - 获取一个路径的文件名（默认包含扩展名）

- path.dirname

  - 获取一个路径中的目录部分

- path.extname

  - 获取一个路径中的扩展名部分

- path.parse

  - 把一个路径转为对象
    - root 根路径
    - dir 目录
    - base 包含后缀名的文件名
    - ext 后缀名
    - name 不包含后缀名的文件名

- path.isAbsolute

  - 判断一个路径是不是绝对路径

- path.join()

  - 作用：拼接路径
  - 参数可以为任意，多写或者少写 `/` 不影响

  ```js
  path.join('c:/a', 'b')
  --'c:\\a\\b'

  path.join('c:/a', '/b', 'c/', './f')
  --'c:\\a\\b\\c\\f'
  ```

## Node 中的其他成员

在每个模块中，出来 `require` 、`exports`等模块相关 API 之外，还有两个特殊的成员：

- `__dirname` **动态获取** 当前文件模块所属目录的绝对路径
- `__filename` **动态获取** 当前文件的绝对路径
- `__dirname` 和 `__filename` 不受 node 命令所属路径影响

### 使用前提

在文件操作路径中，相对路径设计的就是相对于执行 node 命令所处的路径

```js
fs.readFile('./a.txt',function(...){...})
-- 相对于执行 node 命令所处的终端路径
```

### 问题

```js
## ├─app.js
## └─foo
##    ├─a.txt
##    └─index.js

var fs = require('fs')
fs.readFile('./a.txt', function(err, data){
    if (err) { throw err }
    console.log(data)
})
--------------------------- index.js

var fooIndex = require('./foo/index')
--------------------------- app.js

// 在app.js 当前目录执行终端 则加载不到 a.txt
```

在文件操作中，使用相对路径是不可靠的，因为在 Node 中文操作的路径被设计为相对于执行 node 命令所处的路径。（不是 bug ）

为了解决这个问题：把相对路径变为绝对路径即可

### 解决

可以使用 `__dirname` 或者 `__filename` 解决问题

```js
fs.readFile(__dirname + '/a.txt', function(err, data) {
  if (err) {
    throw err
  } // node 执行中会把 / 转为 \
  console.log(data)
})
---------------------------index.js
```

在拼接路径的过程中，为了避免手动拼接带来的低级错误，推荐多使用, `path.join()`来辅助拼接。

```js
fs.readFile(path.join(__dirname, './a.txt'), 'utf8', function(err, data) {
  if (err) {
    throw err
  }
  console.log(data)
})
---------------------------index.js
```

为了尽量避免前面所描述的问题，以后文件操作中使用的相对路径都统一转换为 **动态的绝对路径**。

> 补充： 模块中的路径标识和文件操作中的相对路径标识，不一样
>
> ​ 模块中的路径标识就是相对于当前文件模块就，不受执行 node 命令所处路径影响

## Node_Express

**原生的 http 在某些方面不足以应对我们对开发的需求，需要使用框架加快开发效率，框架的目的就是提高效率，让代码更高度统一。**

**在 Node 中有很多 web 开发框架，Express 是其中一种** http://expressjs.com/

### 起步

#### 安装：

```js
npm install --save express

```

#### hello world

```js
var express = require('express')
// 创建app   =>相当于 http.creataServer
var app = express()
app.get('/', function(req, res) {
  res.send('hello world')
})
app.listen(5000, function() {
  console.log('express app is running...')
})
```

#### 基本路由 router

路由：根据不同的请求路径分发到具体的请求处理函数

- 请求方法
- 请求路径
- 请求处理函数

get：

```js
// 当以 get 方法请求 / 的时候，执行对应的处理函数 => 路由 / 映射关系
app.get('/', function(req, res) {
  res.send('hello world')
})
```

post:

```js
// 当以 post 方法请求 / 的时候，执行对应的处理函数 => 路由 / 映射关系
app.post('/', function(req, res) {
  res.send('Got a POST request')
})
```

重定向：

```js
res.redirect('/')
```

#### 静态服务

```js
## └─Project Directory
##    └─public
## 			 └─main.js

// 当以 /public/ 开头的时候 ，去 ./public/ 目录中 查找对应的资源
app.use('/public/', express.static('./public/'))      ## 推荐
--------
## 访问路径：http://127.0.0.1:5000/public/main.js

// 当省略第一个参数的时候，可以通过省略/public的方式来访问
app.use(express.static('./public/'))
--------
## 访问路径：http://127.0.0.1:5000/main.js

// /a 相当于 /public的别名
app.use('/static/', express.static('./public/'))
--------
## 访问路径：http://127.0.0.1:5000/static/main.js

```

#### 在 Express 中获取表单 GET 请求参数

Express 内置了一个 API，可以直接通过 `req.query` 来获取

```js
req.query
```

#### 在 Express 中获取表单 POST 请求体数据

在 Express 中没有内置获取表单 POST 请求体的 API，需要主要使用第三方包：`body-parser` 中间件（插件，专门用来解析表单 post 请求体）

安装：

```js
npm install --save body-parser

```

配置：

```js
var express = require('express')
var bodyParser = require('body-parser')

var app = express()

// 配置 body-parser
// 加入这个配置后,则在 req 请求对象上会多出来一个属性： body
// 通过 req.body 获取表单 POST 请求体数据
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))
// parse application/json
app.use(bodyParser.json())
```

使用：

```js
app.use(function(req, res) {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  // 可以通过 req.body 来获取表单 POST 请求体数据
  res.end(JSON.stringify(req.body, null, 2))
})
```

#### 其他 API

##### Express 中的 json 方法

- `res.json()` 该方法接收一个对象作为参数，自动把对象转为字符串，在发送给浏览器
- 等价于`res.end(JSON.stringify(req.body, null, ' '))`

##### Express 中 req.hearder

- 获取当前请求的请求报文中的请求头信息
- 其中一个参数，`content-length` GET 请求没有，POST 请求有，表示：传入参数的字节长度

### 在 Express 中配置使用 art-template 模板引擎

- [art-template - GitHub 仓库](https://github.com/aui/art-template)
- [art-template - 官方文档](https://aui.github.io/art-template/zh-cn/index.html)

#### 安装：

```shell
npm install --save art-template
npm install --save express-art-template

```

#### 配置：

```js
app.engine('html', require('express-art-template'))
```

#### 使用：

```js
app.get('/', function(req, res) {
  // express 默认会去项目中的 views 目录中找 index.html
  // render方法 => 渲染文件 详解见说明
  res.render('index.html', {
    title: 'hello world'
  })
})
```

- 如果希望修改默认的 `views` 视图渲染存储目录

  ```js
  // 注意第一个参数 views 千万不能错
  app.set('views', 目录路径)
  ```

#### 说明:

- **配置 art-template 模板引擎**

  ```js
  app.engine('art', require('express-art-template'))
  ```

  - 第一个参数表示：当渲染以 .art 结尾的文件的时候，使用 art-template 模板引擎
    - 个人习惯 `app.engine('html', require('express-art-template'))`
  - express-art-template 是专门用来在 Express 中 把 art-template 整合到 Express 中
  - 虽然这里不需要加载 art-template 但是也必须安装
  - 原因是 express-art-template 依赖了 art-template

- **使用 art-template 模板引擎**

  - Express 为 Response 相应对象提供了一个方法：render
  - render 方法默认是不可以使用的，但是如果配置了模板引擎就可以使用了

  ```js
  res.render('html模板名', { 模板数据 })
  ```

  - 第一个参数不能写路径，默认会去项目中的 views 目录查找该模板文件
  - Express 有个约定，开发人员把所有的视图文件都放到 views 目录中

  ```js
  app.get('/', function(req, res) {
    res.render('index.html') // 若不需要模板引擎渲染，第二个参数不用传，直接渲染文件页面
  })
  ```

  - 若要访问 views 下目录中的文件，直接跳过 views/ 即可

  ```js
  ## └─ views
  ##    └─ admin
  ## 			 └─ index.js
  app.get('/admin', function(req, res) {
    res.render('admin/index.html', {
      title: 'index page'
    })
  })

  ```

### 在 Express 中配置使用 nunjucks 模板引擎

NodeJS 最火的模板引擎

[nunjucks 官网](https://mozilla.github.io/nunjucks/)

#### 安装：

```shell
$ npm install nunjucks
```

#### 配置：

```js
nunjucks.configure('视图渲染存储目录路径', {
  // 一般在配置文件中封装绝对路径
  autoescape: true,
  express: app
})
```

#### 使用：

使用类似 `art-template`

`nunjucks` 模板引擎没有对模板文件名的后缀名做特定限制

```js
app.get('/', (req, res) => {
  res.render('index.html')
})
```

#### 高级语法

- extends
- block
- include

```HTML
<!--------   layout.html   -------->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <link rel="stylesheet" href="normalize.css">
  {% block style %}
  {% endblock %}
</head>
<body>
  {% include "header.html" %}
  {% include "sidebar.html" %}
  {% block body %}
  {% endblock %}
  {% include "footer.html" %}
  <script src="jquery.js"></script>
  {% block script %}
  {% endblock %}
</body>
</html>
```

```html
<!--------index.html-------->

{% extends "layout.html" %} {% block body %}
<h1>这是首页自己的内容 hello {{ foo }}</h1>
{% endblock %}
```

### Express 中配置使用 Express-session

express 中默认不支持 session 和 cookie，使用第三方中间件 `express-session`解决

安装：

`npm install express-session`

配置：（必须在 app.use(router)之前）

- 该插件会为 req 请求对象添加一个成员：`req.session`，默认是一个对象。

```js
var session = require('express-session')
app.use(
  session({
    // 配置加密字符串，会在原有加密基础之上，和这个字符串拼起来去加密
    // 目的为了增加安全性，防止客户端恶意伪造
    secret: 'keyboard cat',
    resave: false,
    saveUninitialized: true // 无论是否使用 session ，默认直接分配一把钥匙（空 session ）
    // false：存数据的时候才会分配钥匙
  })
)
```

使用：

- 可以通过 req.session 来发访问和设置 Session 成员
  - 添加 session 数据：
    - req.session.foo = 'bar'
  - 获取 session 数据：
    - req.session.foo
  - 删除 session 数据：
    - req.session.foo = null
    - 更严谨的做法使用 `delete` 语法
      - delete req.session.foo

提示：默认 Session 数据是内存存储的，服务器一旦存储就会丢失，真正的生产环境会把 Session 进行持久化存储。

### 中间件

http://expressjs.com/en/guide/using-middleware.html

![1-130I0234953631](http://images.dorc.top/blog/Back_End/1-130I0234953631.png)

中间件：用来处理 http 请求的一个具体的环节（可能要执行某个具体的处理函数）
中间件一般都是通过修改 req 或者 res 对象来为后续的处理提供便利的使用

**中间件的本质** 就是一个请求处理方法，把用户从请求到响应的整个过程分发到多个中间件中去处理，这样做的目的是提高代码的灵活性，动态可扩展。

- 同一个请求所经过的中间件都是同一个请求对象和响应对象。

#### 中间件匹配机制

在 http 中，没有请求就没有响应，服务端不可能主动给客户端发请求，就是一问一答的形式

当请求进来，会从第一个中间件开始进行匹配

- 如果匹配，则进来
- 如果请求进入中间件之后，没有调用 next 则代码会停在当前中间件
- 如果调用了 next 则继续向后找到第一个匹配的中间件
- 如果不匹配，则继续判断匹配下一个中间件
- 如果没有能匹配的中间件，则 Express 会默认输出：Cannot GET 路径
- 对于一次请求来说，只能响应一次，如果发送了多次响应，则只有第一次生效

#### 中间件类目

##### 应用程序级别中间件

万能匹配 ：不关心请求方法和请求路径，没有具体路由规则，任何请求都会进入该中间件

```js
// 中间件本身是一个方法，该方法接收三个参数：
//    Request 请求对象
//    Response 响应对象
//    next     下一个中间件
// 当一个请求进入一个中间件之后，如果不调用 next 则会停留在当前中间件
// 所以 next 是一个方法，用来调用下一个中间件的
// 调用 next 方法也是要匹配的（不是调用紧挨着的那个）
app.use(function(req, res, next) {
  console.log('Time:', Date.now())
  next()
})
```

只要是以 `/xxx/` 开头的：不关心请求方法，只关心请求路径的中间件

```js
app.use('/a', function(req, res, next) {
  console.log('Time:', Date.now())
  next()
})
```

##### 路由级别中间件（具体路由规则中间件）

get:

```js
app.get('/', function(req, res) {
  res.send('Hello World!')
})
```

post:

```js
app.post('/', function(req, res) {
  res.send('Got a POST request')
})
```

put:

```js
app.put('/user', function(req, res) {
  res.send('Got a PUT request at /user')
})
```

delete:

```js
app.delete('/user', function(req, res) {
  res.send('Got a DELETE request at /user')
})
```

##### 错误处理中间件

```js
app.use(function(err, req, res, next) {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```

##### 内置中间件

- [express.static](http://expressjs.com/en/4x/api.html#express.static) serves static assets such as HTML files, images, and so on.
- [express.json](http://expressjs.com/en/4x/api.html#express.json) parses incoming requests with JSON payloads. **NOTE: Available with Express 4.16.0+**
- [express.urlencoded](http://expressjs.com/en/4x/api.html#express.urlencoded) parses incoming requests with URL-encoded payloads. **NOTE: Available with Express 4.16.0+**

##### 第三方中间件

http://expressjs.com/en/guide/using-middleware.html

- [body-parser](http://expressjs.com/en/resources/middleware/body-parser.html)

- [compression](http://expressjs.com/en/resources/middleware/compression.html)
- [cookie-parser](http://expressjs.com/en/resources/middleware/cookie-parser.html)
- [morgan](http://expressjs.com/en/resources/middleware/morgan.html)
- [response-time](http://expressjs.com/en/resources/middleware/response-time.html)
- [serve-static](http://expressjs.com/en/resources/middleware/serve-static.html)
- [session](http://expressjs.com/en/resources/middleware/session.html)

#### 中间件应用

##### 模拟封装 express.static

- 前提知识储备：

  - Express 内置了一个 API，可以通过 `req.path` 来获取请求 URL 的路径部分

    - ```js
      // ...com/users?sort=desc
      req.path // 获取结果：/users
      ```

  - 在 `use` 方法(中间件)中，如果指定了第一个路径参数，则通过 req.path 获取到的是不包含该请求路径的字符串，具体实例在封装中体现。

    - ```js
      // ...com/public/a.jpg
      req.path // 拿到的就是 a.jpg

      // ...com/public/a/a.css
      req.path // 拿到的就是 a/a.css
      ```

```js
const fs = require('fs')
const path = require('path')

module.exports = function(dirPath) {
  return (req, res, next) => {
    const filePath = path.join(dirPath, req.path)
    fs.readFile(filePath, (err, data) => {
      if (err) {
        return res.end('404 Not Found.')
      }
      res.end(data)
    })
  }
}
```

- 使用时只需加载该模块就可以了

##### 配置处理 404 的中间件

```js
// 在项目入口文件的最后（app.listen之前）
app.use(function(req, res) {
  res.render('404.html')
})
```

##### 配置全局处理中间件

- 当调用 next 的时候，如果传递了参数，则直接往后找到带有 四个参数的应用程序级别中间件

  ```js
  // 在项目入口文件的最后（app.listen之前）
  app.use((err, req, res, next) => {
    const error_log = `
  ====================================
  错误名：${err.name}
  错误消息：${err.message}
  错误堆栈：${err.stack}
  错误时间：${new Date()}
  ====================================\n\n`
    fs.appendFile('./err_log.txt', error_log, err => {
      res.writeHead(500, {})
      res.end('500 服务器正忙，请稍后重试')
    })
  })
  ```

- 当发生错误的时候，我们可以调用 next 传递错误对象

  ```js
  ···
  if (err) {
    return next(err) // 省去大量重复代码
  }
  ···
  ```

- 然后就会被全局错误处理中间件匹配到并处理之

##### 模拟封装 body-parser

由于表单 POST 请求可能会携带大量的数据，所以在进行请求提价的时候会分为多次提交

具体分为多少次进行提交不一定，取决于数据量的大小

在 Node 中，对于处理这种不确定的数据，使用事件的形式处理

可以通过监听 req 对象的 data 事件，然后通过对应的回调处理函数中的参数 chunk 拿到每一次接收到的数据 data 事件触发多少次，不一定

当数据接收完毕之后，会自动触发 req 对象的 end 事件，然后就可以在 end 事件中使用接收到的表单 POST 请求体

最后，手动给 req 对象挂载一个 body 属性，值就是当前表单 POST 请求体对象

在后续的处理中间件中，就可以直接使用 req.body 了

因为`在同一个请求中，流通的都是同一个 req 和 res 对象`

```js
// 解析处理表单 POST 请求体中间件
app.use((req, res, next) => {
  let data = ''
  req.on('data', chunk => {
    // chunk中获取的是二进制数据，与字符串拼接自动 toString
    data += chunk
  })
  req.on('end', () => {
    req.body = queryString.parse(data)
    next()
  })
})
```

## 在 Node 中使用 formidable 处理文件上传

> 具体使用方式参照官方文档：https://www.npmjs.com/package/formidable

安装：

```shell
$ npm install formidable
```

使用：

```js
app.post('/', (req, res, next) => {
  // parse a file upload
  const form = new formidable.IncomingForm()

  // 指定上传路径
  form.uploadDir = './upload' // 文件目录必须手动创建

  // 保持原来的扩展名
  form.keepExtensions = true

  // err 就是可能发生的错误对象
  // fields 就是普通的表单字段
  // files 就是文件内容数据信息
  form.parse(req, (err, fields, files) => {
    if (err) {
      throw err
    }
    // console.log(fields)
    console.log(files)
    res.end('success')
  })
})
```

- `files` 返回信息

  ```js
  { avatar:
     File {
       _events: [Object: null prototype] {},
       _eventsCount: 0,
       _maxListeners: undefined,
       size: 470457, // 文件大小。单位：字节。
       path: // 文件路径，formidable自动将接受解析到的文件重命名后，存储到操作系统的临时目录
  'C:\\Users\\Administrator\\AppData\\Local\\Temp\\upload_ccba69d6c1695b583c2a5d3bb7a72d54',
       name: '微信图片_20190703082406.png', // 原文件名
       type: 'image/png',
       hash: null,
       lastModifiedDate: 2019-11-11T02:00:35.969Z,
       _writeStream: WriteStream {
          _writableState: [WritableState],
          writable: false,
          _events: [Object: null prototype] {},
          _eventsCount: 0,
          _maxListeners: undefined,
          path:
  'C:\\Users\\Administrator\\AppData\\Local\\Temp\\upload_ccba69d6c1695b583c2a5d3bb7a72d54',
          fd: null,
          flags: 'w',
          mode: 438,
          start: undefined,
          autoClose: true,
          pos: undefined,
          bytesWritten: 470457,
          closed: false
        }
     }
  }
  ```

### 异步上传文件

原理：

```js
$('form').on('submit', function(e) {
  var formData = new FormData()
  formData.append('name', 'zf')
  formData.append('file', document.getElementById('file').files[0])
  var xhr = new XMLHttpRequest()
  xhr.open('post', '/advert/add')
  xhr.send(formData)
  return false // 阻止本身提交行为
})
```

ajax 异步上传：

```js
$('form').on('submit', function(e) {
  $.ajax({
    url: $(this).attr('action'),
    type: $(this).attr('method'),
    data: new FormData($(this)[0]),
    processData: false, // 当 data 选项被提交一个 FormData 对象时：使jQuery异步请求生效
    contentType: false, // jQuery会默认将 contentType 设置为
    success: function(data) {
      // application/x-www-form-urlencoded; charset=UTF-8
      if (data.err_code === 0) {
        window.location.href = '/advert'
      }
    }
  })
  return false // 阻止本身提交行为
})
```

## crud 案例

### 模块化思想

模块符合划分：

- 模块职责要单一

### 案例

https://github.com/asnwer-zf/nodeText_express_crud

## 其他：

### 修改完成代码自动重启

**第三方命令行工具`nodemon`，可以解决频繁修改代码重启服务器问题**

**`nodemon`是基于 node.js 开发的第三方命令行工具，需要独立安装**

```shell
## 所有需要用 --global 来安装的包都可以在任意目录执行
npm install --global nodemon
```

**安装完毕以后，使用**

```shell
node app.js
## 执行 nodemon 替换 node
nodemon app.js
```

**通过 `nodemon` 启动的服务，会监视文件变化，当文件发生变化，自动重启服务器**

### 文件操作中的 / 与模块标识中的 /

- **文件标识中的路径可以省略 `./` **
- **在模块加载中，相对路径中的 `./` 不能省略**

### 快捷创建服务

#### http-server

##### 安装

```shell
npm install -g http-server@0.9.0 ## 新版本报错
```

##### 启动

```shell
hs -c-l -o
```

#### json-server

##### 安装

```shell
npm install -g json-server
```

##### 启动

```shell
json-server --watch 文件名
```
