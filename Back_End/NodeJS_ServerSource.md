# Node.js_Server

## Node 模拟 Apache 基础功能

### url 访问文件

```js
var http = require('http')
var fs = require('fs')

var server = http.createServer()

var dir = 'E:/php-zf/text_zf/02' // 文件访问目录

server.on('request', function(req, res) {
  var url = req.url
  var filePath = '/index.html'

  if (url !== '/') {
    filePath = url
  }

  fs.readFile(dir + filePath, function(err, data) {
    if (err) {
      res.setHeader('Content-Type', 'text/plain;charset=uft-8')
      res.end('404 Not Found')
      return
    }

    res.end(data)
  })
})

server.listen('3000', function() {
  console.log('Server is running')
})
```

### 目录浏览（ 未用模板引擎，原始版 ）

```js

## template.html 代码		<tbody> {{tr}} </tbody>

var http = require('http')
var fs = require('fs')

var server = http.createServer()

var dir = 'E:/php-zf/text_zf/02'

server.on('request', function(req, res) {
  var url = req.url
  fs.readFile('./template.html', function(err, data) {
    if (err) {
      res.end('Not Found 404')
      return
    }
    fs.readdir(dir, function(err, files) {
      if (err) {
        res.end('Not Found Root Dir')
        return
      }
      var content = ''
      files.forEach(function(item) {
        content += `
        <tr>
          <td data-value=".vscode/"><a class="icon dir" href="">${item}/</a></td>
          <td class="detailsColumn" data-value="0"></td>
          <td class="detailsColumn" data-value="1570514535"></td>
        </tr>
        `
      })
      data = data.toString()
      data = data.replace('{{tr}}', content)
      res.end(data)
    })
  })
})

server.listen('3000', function() {
  console.log('Server is running')
})

```

## node.js 使用模板引擎

- npm 下载

- require 引入第三方模板

- var result = template.render('模板字符串'，{ 解析替换对象 })

- response.end( result )

  ```js
  var template = require('art-template')
  var html = `
  name : {{name}}
  age : {{age}}
  hb : {{each hb}} {{$value}} {{/each}}
  `

  var res = template.render(html, {
    name: 'ans',
    age: 18,
    hb: ['adsf', '123', 'pipo']
  })
  ```
