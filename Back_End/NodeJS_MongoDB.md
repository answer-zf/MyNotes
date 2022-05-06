# mongoDB

https://www.runoob.com/mongodb/mongodb-tutorial.html

## 关系型数据库和非关系型数据库

### 关系型数据库

表就是关系，或者说表与表之间存在关系

- 所有的关系型数据库都需要通过 `sql` 语言来操作
- 所有的关系型数据库在操作前都需要设计表结构
- 数据表还支持约束
  - 唯一的
  - 主键
  - 默认值
  - 非空

### 非关系型数据库

非常灵活

- 有的非关系型数据库就是 key-value 对
- 在 MongoDB 是长得最像关系型数据库的非关系型数据库
  - 数据库 - > 数据库
  - 数据表 - > 集合（数组）
  - 表记录 - > (文档对象)
- MongoDB 不需要设计表结构
- 可以任意往里面存数据，没有结构性这么一说

## MongoDB 数据库基本概念

- 数据库 : 可以有多个数据库
- 集合 : 一个数据库可以有多个集合（表）
- 文档：一个集合中可以有多个文档（表记录）
- 文档结构非常灵活，没有任何限制
- MongoDB 非常灵活，不需要想 MySQL 一样先创建数据库、表、设计表结构
  - 这里只需要，当你需要插入数据的时候，只需要指定往哪个数据库的哪个集合操作就可以
  - 一切都有 MongoDB 来帮你自动完成建库建表的事

```js
{
  qq:{   // 数据库
    users: [   // 集合
      {name: 'zhangsan', age: 16},   // 文档
      {name: 'zhangsan', age: 16},
      {name: 'zhangsan', age: 16},
      {name: 'zhangsan', age: 16},
      ...
    ],
    products: {

    }
    ...
  },
  taobao:{

  },
  baidu{

  }
}
```

总结：

- 一个计算机上可以有一个数据库服务实例
- 一个数据服务实例上可以有多个数据库
- 一个数据库中可以有多个集合
  - 集合根据数据的业务类型划分
  - 例如用户数据、商品信息数据、广告信息数据。。。
  - 对数据进行分门别类的存储
  - 集合在 MongoDB 中就类似于数组
- 一个集合中可以有多个文档
  - 文档在 MongoDB 中就是一个 类似于 JSON 的数据对象
  - 文档对象是动态的，可以随意的生成
  - 为了便于管理，最好一个集合中存储的数据一定要保持文档结构的统一（数据完整性）

## 安装

https://www.runoob.com/mongodb/mongodb-window-install.html

- 下载
- 安装
- 配置环境变量
- `mongod --version` 测试是否安装成功

## 启动和关闭数据库

启动:

```shell
# mongodb  默认使用执行 mongod 命令所处盘符根目录下 /data/db 作为自己的数据存储目录
# 在第一次执行该命令之前先自己手动新建一个 /data/db
mongod
```

如果需要修改默认数据存储目录，可以

```shell
mongod --dbpath=数据存储目录路径
```

停止：

```shell
在开启服务的控制台，直接 Ctrl+c 即可停止
或者直接关1闭开启服务的控制台
```

## 连接和退出数据库

连接：

```shell
# 该命令默认连接本机的 MongoDB 服务
mongo
```

退出：

```shell
# 在连接状态输入 exit 退出连接
exit
```

## 基本命令

- `show dbs`

  - 查看显示书友数据库

- `db`

  - 查看当前操作的数据库

- `use 数据库名称`

  - 切换到指定数据库（如果没用会新建）

- 查询数据

  ![MongoDB_cmd](http://localhost:9710/blog/Back_End/MongoDB_cmd.png)

## 在 Node 中操作 MongoDB 数据库

### MongoDB

- 使用官方 `MongoDB` 包来操作

  > https://github.com/mongodb/node-mongodb-native

### mongoose

- 使用第三方 mongoose 来操作 MongoDB 数据库

- 第三方包：`mongoose` 基于 MongoDB 官方的 `mongodb` 包再一次做了封装。

  > https://mongoosejs.com/

#### 起步：

安装：

```shell
npm i mongoose
```

hello world:

```js
var mongoose = require('mongoose')

// 连接 MongoDB 数据库
mongoose.connect('mongodb://localhost/test', { useMongoClient: true })

// 创建一个模型
// 就是在设计数据库
// MongoDB 是动态的，非常灵活，只需要在代码中设计数据库就可以了
// mongoose 这个包就可以让你的设计编写过程变得非常简单
var Cat = mongoose.model('Cat', { name: String })

// 实例化一个Cat
var kitty = new Cat({ name: 'Zildjian' })

// 持久化保存 Kitty 实例
kitty.save(function(err) {
  if (err) {
    console.log(err)
  } else {
    console.log('meow')
  }
})
```

#### 官方指南

#####设计 Schema 发布 Module

```js
var mongoose = require('mongoose')

// 创建一个模型架构，设计数据结构和约束
var Schema = mongoose.Schema
// 1. 连接数据库
// 指定连接的数据库不需要存在，当你插入第一条数据之后就会自动被创建出来
mongoose.connect('mongodb://localhost/text')

// 2. 设计文档结构（表结构）
// 字段名称就是表结构中的属性名称
// 约束的目的是为了保护数据的完整性，避免脏数据
var userSchema = new Schema({
  username: {
    type: String,
    required: true // 必须有
  }
})

// 3. 将文档架构发布为模型
//    mongoose.model 方法就是用来将一个架构发布为 model
//    第一个参数：传入一个大写名词单数字符串用来表示数据库名称（帕斯卡命名法、大驼峰）
//              mongoose 会自动将大写名词的字符串生成 小写复数 的集合名称
//    第二个参数：架构 Schema
//    返回值：模型构造函数
var User = mongoose.model('User', userSchema)

// 4. 使用这个构造函数 操作 users 集合中的数据
```

##### 设计 Schema

```js
var userSchema = new Schema({
  email: {
    type: String,
    required: true // 必填的
  },
  gender: {
    type: Number,
    enum: [-1, 0, 1], // 枚举， 三个值内选一个
    default: -1
  },
  created_time: {
    type: Date,
    default: Date.now
  }
  // 不能是 Date.now() 会即刻调用 （死数据）
  // 用 Date.now 方法 ：当 new model 时 用户没有传递 create_time ，mongoose 会调用 default指定的 Date.now 方法，使用其返回值作为默认值
})
```

##### 增加数据

```js
var admin = new User({
  username: 'admin',
  password: '123456',
  email: 'admin@admin.com'
})

admin.save(function(err, ret) {
  if (err) {
    console.log('保存失败')
  } else {
    console.log('保存成功')
    console.log(ret)
  }
})
```

##### 查询数据

查询所有：

```js
User.find(function(err, ret) {
  if (err) {
    console.log('查询失败')
  } else {
    console.log(ret)
  }
})
```

按条件查询所有：

```js
User.find(
  {
    username: 'zhangsan'
  },
  function(err, ret) {
    if (err) {
      console.log('查询失败')
    } else {
      console.log(ret)
    }
  }
)
```

按条件查询单个：

```js
User.findOne(
  {
    username: 'zhangsan'
  },
  function(err, ret) {
    if (err) {
      console.log(err)
    } else {
      console.log(ret)
    }
  }
)
```

##### 删除数据

根据条件删除所有：

```js
User.remove(
  {
    username: 'zhangsan'
  },
  function(err, ret) {
    if (err) {
      console.log('删除数据')
    } else {
      console.log('删除成功')
    }
  }
)
```

根据条件删除一个：

```js
Model.findOneAndeRemove(conditions, [options], [callback])
```

根据 id 删除一个：

```js
Model.findByIdAndeRemove(id, [options], [callback])
```

##### 更新数据

根据条件更新所有：

```js
Module.update(conditions, doc, [options], [callback])
```

根据指定条件更新一个：

```js
Module.findOneAndUpdate([conditions], [update], [options], [callback])
```

根据 id 更新一个：

```js
User.findByIdAndUpdate(
  '5da6b53252309800c0231134',
  {
    password: '963'
  },
  function(err, ret) {
    if (err) {
      console.log('更新失败')
    } else {
      console.log('更新成功')
    }
  }
)
```

##### 模糊匹配

`/advert/one/:advertId` 是一个模糊匹配路径
可以匹配 `/advert/one/*` 的路径形式
例如：`/advert/one/1 /advert/one/2 /advert/one/a /advert/one/abc` 等路径
但是 `/advert/one` 或者 `/advert/one/a/b` 是不行的
至于 `advertId` 是自己起的一个名字，可以在处理函数中通过 `req.params` 来进行获取
