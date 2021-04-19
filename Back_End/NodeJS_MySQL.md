# 使用 Node 操作 MySQL 数据库

## 安装

```shell
npm install --save mysql
```

## 使用

```js
var mysql = require('mysql')

// 1. 创建连接
var connection = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'root',
  database: 'users'
})

// 2. 连接数据库
connection.connect()

// 3. 执行数据操作
connection.query('SELECT * FROM `users`', function(error, results, fields) {
  if (error) throw error
  console.log('The solution is: ', results)
})

// 4. 关闭连接
connection.end()
```

- 增加数据

```js
connection.query('INSERT INTO users VALUES(NULL, "admin", "123456")', function(
  error,
  results,
  fields
) {
  if (error) throw error
  console.log('The solution is: ', results)
})
```
