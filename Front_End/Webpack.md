# Webpack

> webpack 是前端的一个项目构建工具，它是基于 Node.js 开发出来的一个前端工具；

## 前提

### 网站构建中：网页中引入的静态资源过多，出现的问题

1. 网页加载速度慢， 因为 我们要发起很多的二次请求；
2. 要处理错综复杂的依赖关系

### 如何解决上述两个问题

1. 合并、压缩、精灵图、图片的 Base64 编码
2. 可以使用`requireJS`、也可以使用`webpack`可以解决各个包之间的复杂依赖关系；

### 解决方案

1. 使用 Gulp， 是基于 task 任务的；
2. 使用`Webpack`， 是基于整个项目进行构建的；
   - 借助于`webpack`这个前端自动化构建工具，可以完美实现资源的合并、打包、压缩、混淆等诸多功能。
   - 根据官网的图片介绍`webpack`打包的过程

## 安装

1. 运行`npm i webpack -g`全局安装`webpack`，这样就能在全局使用`webpack`的命令
2. 在项目根目录中运行`npm i webpack --save-dev`安装到项目依赖中

## 初步使用 webpack 打包构建列表隔行变色案例

1. 运行`npm init`初始化项目，使用 `npm` 管理项目中的依赖包
2. 创建项目基本的目录结构
3. 使用`cnpm i jquery --save`安装 `jquery` 类库
4. 创建`main.js`并书写各行变色的代码逻辑：

```js
// 导入jquery类库
import $ from 'jquery'
// 设置偶数行背景色，索引从0开始，0是偶数
$('#list li:even').css('backgroundColor', 'lightblue')
// 设置奇数行背景色
$('#list li:odd').css('backgroundColor', 'pink')
```

5. 直接在页面上引用 `main.js` 会报错，因为浏览器不认识 `import` 这种高级的 JS 语法，需要使用 `webpack` 进行处理，`webpack` 默认会把这种高级的语法转换为低级的浏览器能识别的语法；
6. 运行 `webpack 入口文件路径 输出文件路径` 对 `main.js` 进行处理：

```shell
webpack src/js/main.js dist/bundle.js
```

总结：

1. `webpack` 能够处理 JS 文件的互相依赖关系；
2. `webpack` 能够处理 JS 的兼容问题，把 高级的、浏览器不是别的语法，转为 低级的，浏览器能正常识别的语法

## 使用 webpack 的配置文件简化打包时候的命令

1. 在项目根目录中创建`webpack.config.js`
2. 由于运行 `webpack` 命令的时候，`webpack` 需要指定入口文件和输出文件的路径，所以，我们需要在`webpack.config.js`中配置这两个路径：

```js
const path = require('path')
module.exports = {
  entry: path.join(__dirname, './src/main.js'), // 项目入口文件
  output: {
    // 配置输出选项
    path: path.join(__dirname, './dist'), // 配置输出的路径
    filename: 'bundle.js' // 配置输出的文件名
  }
}
```

在控制台，直接输入 `webpack` 命令执行的时候，`webpack` 做了以下几步：

1. 首先，`webpack` 发现，我们并没有通过命令的形式，给它指定入口和出口
2. `webpack` 就会去 项目的 根目录中，查找一个叫做 `webpack.config.js` 的配置文件
3. 当找到配置文件后，`webpack` 会去解析执行这个 配置文件，当解析执行完配置文件后，就得到了 配置文件中，导出的配置对象
4. 当 `webpack` 拿到 配置对象后，就拿到了 配置对象中，指定的 入口 和 出口，然后进行打包构建；

## 实现 webpack 的实时打包构建

1. 由于每次重新修改代码之后，都需要手动运行`webpack`打包的命令，比较麻烦，所以使用`webpack-dev-server`来实现代码实时打包编译，当修改代码之后，会自动进行打包构建。

2. 运行`npm i webpack-dev-server --save-dev`安装到开发依赖

   - 由于，我们是在项目中，本地安装的 `webpack-dev-server` ， 所以，无法把它当作 脚本命令，在`powershell` 终端中直接运行；（只有那些 安装到 全局 -g 的工具，才能在 终端中正常执行）

   - `webpack-dev-server` 这个工具，如果想要正常运行，要求，在本地项目中，必须安装 `webpack`

3. 安装完成之后，在命令行直接运行`webpack-dev-server`来进行打包，发现报错，此时需要借助于`package.json`文件中的指令，来进行运行`webpack-dev-server`命令，在`scripts`节点下新增`"dev": "webpack-dev-server"`指令，发现可以进行实时打包，但是`dist`目录下并没有生成`bundle.js`文件，这是因为`webpack-dev-server`将打包好的文件托管到了 电脑的内存中，并没有存放到 实际的 物理磁盘上，所以，我们在 项目根目录中，根本找不到 这个打包好的 `bundle.js`

   - 我们可以认为， `webpack-dev-server` 把打包好的 文件，以一种虚拟的形式，托管到了 咱们项目的 根目录中，虽然我们看不到它，但是，可以认为， 和 `dist` `src` `node_modules` 平级，有一个看不见的文件，叫做 `bundle.js`

- 把`bundle.js`放在内存中的好处是：由于需要实时打包编译，所以放在内存中速度会非常快

- 这个时候访问`webpack-dev-server`启动的`http://localhost:8080/`网站，发现是一个文件夹的面板，需要点击到`src`目录下，才能打开我们的 index 首页，此时引用不到`bundle.js`文件，需要修改`index.html`中`script`的`src`属性为:`<script src="/bundle.js"></script>`

- 在 `package.json` 中设置 `scripts`节点下中的`"dev"`

  - `--open` 运行直接打开浏览器
  - `--port 3000`设置启动时候的运行端口
  - `--contentBase src` 设置托管的根目录
  - `--hot` 减少不必要的代码更新，浏览器无刷新重载样式

  ```
  "dev": "webpack-dev-server --open --port 3000 --contentBase src --hot"
  ```

  也可以在 `webpack.config.js` 配置文件中配置

  ```js
  const webpack = require('webpack') // 启用热更新的 第1步
  module.exports = {
  	···
    devServer: { // 这是配置 dev-server 命令参数的第二种形式，相对来说，这种方式麻烦一些
      open: true, // 自动打开浏览器
      port: 3000, // 设置启动时候的运行端口
      contentBase: 'src', // 指定托管的根目录
      hot: true // 启用热更新的 第2步
    },
    plugins: [ // 配置插件的节点
      new webpack.HotModuleReplacementPlugin()
    ] 																	// new 一个热更新的 模块对象，启用热更新的 第3步
    ···
  }
  ```

## 使用 html-webpack-plugin 插件配置启动页面

在内存中根据`index.html` 模板页面生成内存中的首页。

1. 运行`cnpm i html-webpack-plugin --save-dev`安装到开发依赖

2. 配置 `webpack.config.js`

   ```js
   const htmlWebpackPlugin = require('html-webpack-plugin')
   // 导入在内存中生成 HTML 页面的 插件
   // 只要是插件，都一定要 放到 plugins 节点中去
   // 这个插件的两个作用：
   //  1. 自动在内存中根据指定页面生成一个内存的页面
   //  2. 自动，把打包好的 bundle.js 追加到页面中去
   module.exports = {
   	···
     plugins: [
       new htmlWebpackPlugin({ // 创建一个 在内存中 生成 HTML  页面的插件
         template: path.join(__dirname, './src/index.html'), // 指定 模板页面，将来会根据指定的页面路径，去生成内存中的 页面
         filename: 'index.html' // 指定生成的页面的名称
       })
     ]
     ···
   }
   ```

## 使用 webpack 打包 css 文件

注意：`webpack`, 默认只能打包处理 JS 类型的文件，无法处理 其它的非 JS 类型的文件

如果要处理 非 JS 类型的文件，我们需要手动安装一些 合适 第三方 loader 加载器；

1. 使用 import 语法，导入 CSS 样式表 `import './css/index.css'`

2. 如果想要打包处理 `css` 文件，需要安装 `cnpm i style-loader css-loader -D`

3. 打开 `webpack.config.js` 这个配置文件，

   - 在 里面，新增一个 配置节点，叫做 `module`, 它是一个对象；
   - 在 这个 `module` 对象身上，有个 `rules` 属性，这个 `rules` 属性是个 数组；
   - 这个数组中，存放了，所有第三方文件的 匹配和 处理规则；
   - `use`表示使用哪些模块来处理`test`所匹配到的文件；
   - `use`中相关 loader 模块的调用顺序是从右向左调用的；

   ```js
   module: {
     // 这个节点，用于配置 所有 第三方模块 加载器
     rules: [
       // 所有第三方模块的 匹配规则
       { test: /\.css$/, use: ['style-loader', 'css-loader'] }
       //  配置处理 .css 文件的第三方loader 规则
     ]
   }
   ```

注意： `webpack` 处理第三方文件类型的过程：

1. 发现这个 要处理的文件不是`JS`文件，然后就去 配置文件中，查找有没有对应的第三方 `loader` 规则
2. 如果能找到对应的规则， 就会调用 对应的 `loader` 处理 这种文件类型；
3. 在调用`loader` 的时候，是从后往前调用的；
4. 当最后的一个 `loader` 调用完毕，会把 处理的结果，直接交给 `webpack` 进行 打包合并，最终输出到 `bundle.js` 中去

### 使用`webpack`打包 less 文件

1. 运行`npm i less-loader less -D`
   - `less-loader`内部依赖`less`， 不需要显示定义配置文件中
2. 修改`webpack.config.js`这个配置文件：

```js
{ test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] },
```

### 使用`webpack`打包 sass 文件

1. 运行`npm i sass-loader node-sass --save-dev`
2. 在`webpack.config.js`中添加处理 sass 文件的 loader 模块：

```js
{ test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] }
```

注意：`Webpack2X` 以上版本，加载器必须带 `-loader`，1X 的版本不需要带。

## webpack 中的 url-loader

默认情况下，`webpack` 无法 处理 `CSS` 文件中的 `url` 地址，不管是图片还是 字体库， 只要是 `URL` 地址，都处理不了

1. 运行 `npm i url-loader file-loader -D`

2. 在`webpack.config.js`中配置：

   ```js
   { // 处理 图片路径的 loader
     test: /\.(jpg|png|gif|bmp|jpeg)$/,
     use: 'url-loader?limit=38000&name=[hash:8]-[name].[ext]'
   },
   { // 处理 字体文件的 loader
     test: /\.(ttf|eot|svg|woff|woff2)$/,
     use: 'url-loader'
   }
   ```

   - `limit` 给定的值，是图片的大小，单位是 `byte`， 如果引用的 图片，大于或等于给定的 `limit`值，则不会被转为`base64`格式的字符串， 如果 图片小于给定的 `limit` 值，则会被转为 `base64`的字符串
   - `name` 自定义文件重命名，`[hash:8]` 8 位哈希值（最长 32），`[name]`原文件名，`[ext]` 原文件后缀名

## webpack 中的 babel-loader

在 `webpack` 中，默认只能处理 一部分 ES6 的新语法，一些更高级的 ES6 语法或者 ES7 语法，`webpack` 是处理不了的；这时候，就需要 借助于第三方的 `loader`，来帮助`webpack` 处理这些高级的语法。

- 当第三方`loader` 把 高级语法转为 低级的语法之后，会把结果交给 `webpack` 去打包到 `bundle.js` 中

- 通过 `Babel` ，可以帮我们将 高级的语法转换为 低级的语法

1. 在 `webpack` 中，可以运行如下两套 命令，安装两套包，去安装 `Babel` 相关的`loader`功能：
   1.1 第一套包(转化工具)： `cnpm i babel-core babel-loader babel-plugin-transform-runtime -D`
   1.2 第二套包(语法工具)： `cnpm i babel-preset-env babel-preset-stage-0 -D`

2. 打开 `webpack` 的配置文件，在 `module` 节点下的 `rules` 数组中，添加一个 新的 匹配规则：
   2.1 `{ test:/\.js$/, use: 'babel-loader', exclude:/node_modules/ }`
   2.2 注意： 在配置 `babel` 的 `loader`规则的时候，必须 把 `node_modules` 目录，通过 `exclude` 选项排除掉：原因有俩：
   2.2.1 如果 不排除 `node_modules`， 则`Babel` 会把 `node_modules` 中所有的 第三方 `JS` 文件，都打包编译，这样，会非常消耗`CPU`，同时，打包速度非常慢；
   2.2.2 哪怕，最终，`Babel` 把 所有 `node_modules` 中的`JS`转换完毕了，但是，项目也无法正常运行！

3. 在项目的 根目录中，新建一个 叫做 `.babelrc` 的`Babel` 配置文件，这个配置文件，属于 JSON 格式，所以，在写 `.babelrc` 配置的时候，必须符合`JSON`语法规范： 不能写注释，字符串必须用双引号
   3.1 在 `.babelrc` 写如下的配置： 大家可以把 `preset` 翻译成 【语法】 的意思

```js
{
  "presets": ["env", "stage-0"],
  "plugins": ["transform-runtime"]
}
```

4. 了解： 目前，我们安装的 `babel-preset-env`, 是比较新的 ES 语法， 之前， 我们安装的是`babel-preset-es2015`, 现在，出了一个更新的 语法插件，叫做 `babel-preset-env` ，它包含了 所有的 和 es\*\*\*相关的语法

## 在 webpack 中构建 vue

### 在使用 webpack 构建的 Vue 项目中使用模板对象

- 在`webpack.config.js`中添加`resolve`属性：

  ```js
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js'
    }
  }
  ```

- 或者 入口文件中引入：`import Vue from '../node_modules/vue/dist/vue.js'`

  - 代替 `import Vue from 'vue'`

回顾 包的查找规则：

1. 找 项目根目录中有没有 `node_modules` 的文件夹
2. 在`node_modules 中` 根据包名，找对应的 `vue` 文件夹
3. 在 `vue`文件夹中，找 一个叫做 `package.json` 的包配置文件
4. 在 `package.json` 文件中，查找 一个 main 属性【main 属性指定了这个包在被加载时候，的入口文件】

### 在`webpack`中配置`.vue`组件页面的解析

1. 运行`npm i vue -S` 将`vue`安装为运行依赖；

   - 默认，`webpack` 无法打包 `.vue` 文件，需要安装 相关的 loader：

2. 运行`npm i vue-loader vue-template-compiler -D`将解析转换`vue`的包安装为开发依赖；

3. 在`webpack.config.js`中，新增`loader`的配置项：

   - `{ test: /\.vue$/, use: 'vue-loader' }`

4. 组件页面：

   ```vue
   <template>
     <div>
       <h1>.vue</h1>
     </div>
   </template>

   <script></script>

   <style></style>
   ```

5. 入口文件：

   ```js
   import Vue from 'vue'
   import login from './login.vue'
   var vm = new Vue({
     el: '#app',
     data: {
       msg: '123'
     },
     methods: {},
     render: c => c(login)
   })
   ```

总结梳理： `webpack`中如何使用 `vue`:

1. 安装`vue`的包： `cnpm i vue -S`
2. 由于 在 `webpack`中，推荐使用`.vue`这个组件模板文件定义组件，所以，需要安装 能解析这种文件的 loader `cnpm i vue-loader vue-template-complier -D`
3. 在`main.js`中，导入 `vue`模块 `import Vue from 'vue'`
4. 定义一个`.vue`结尾的组件，其中，组件有三部分组成： `template script style`
5. 使用 `import login from './login.vue'` 导入这个组件
6. 创建 `vm`的实例 `var vm = new Vue({ el: '#app', render: c => c(login) })`
7. 在页面中创建一个 `id`为 `app`的 `div`元素，作为我们 `vm`实例要控制的区域；

### 结合 `webpack` 使用 `vue-router`

1. 运行`npm i vue-router -S` 将`vue-router`安装为运行依赖；
2. 导入 `vue-router` 包
3. 手动安装`VueRouter`
4. 创建路由对象
5. 将路由对象挂载到 `vm`上

```js
import Vue from 'vue'
// 1. 导入 vue-router 包
import VueRouter from 'vue-router'
// 2. 手动安装 VueRouter
Vue.use(VueRouter)

// 导入 app 组件
import app from './App.vue'
// 导入 Account 组件
import account from './main/Account.vue'
import goodslist from './main/GoodsList.vue'

// 3. 创建路由对象
var router = new VueRouter({
  routes: [
    // account  goodslist
    { path: '/account', component: account },
    { path: '/goodslist', component: goodslist }
  ]
})

var vm = new Vue({
  el: '#app',
  render: c => c(app), // render 会把 el 指定的容器中，所有的内容都清空覆盖，所以 不要 把 路由的 router-view 和 router-link 直接写到 el 所控制的元素中，，需要写到 `App` 这个组件中
  router // 4. 将路由对象挂载到 vm 上
})
```

注意： `App`这个组件，是通过 `VM`实例的 `render`函数，渲染出来的， `render` 函数如果要渲染 组件， 渲染出来的组件，只能放到 `el: '#app'`所指定的 元素中；
`Account`和 `GoodsList`组件， 是通过 路由匹配监听到的，所以， 这两个组件，只能展示到 属于 路由的 `<router-view></router-view>` 中去；

### 结合 `webpack` 实现路由嵌套以及路由提取

```js
import VueRouter from 'vue-router'

// 导入 Account 组件
import account from './main/Account.vue'
import goodslist from './main/GoodsList.vue'

// 导入Account的两个子组件
import login from './subcom/login.vue'
import register from './subcom/register.vue'

// 3. 创建路由对象
var router = new VueRouter({
  routes: [
    {
      path: '/account',
      component: account,
      children: [
        { path: 'login', component: login },
        { path: 'register', component: register }
      ]
    },
    { path: '/goodslist', component: goodslist }
  ]
})

// 把路由对象暴露出去
export default router
```

`account.vue` 中引入路由模块。

```vue
<template>
  <div>
    <h2>this is Account page</h2>
    <router-link to="/account/login">login</router-link>
    <router-link to="/account/register">register</router-link>
    <router-view></router-view>
  </div>
</template>
```

入口文件：

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

import App from './App.vue'
import router from './router.js'

var vm = new Vue({
  el: '#app',
  render: c => c(App),
  router
})
```

### .vue 中 样式配置

- 在 `.vue` 组件中，`style` 的样式作用于全局，开始 `scoped` 属性，则可以作用域局部，全局不受影响。
- 普通的 `style` 标签只支持 普通的 样式，如果想要启用 `scss` 或 `less` ，需要为 `style` 元素，设置 `lang` 属性

- 只要 咱们的 style 标签， 是在 `.vue` 组件中定义的，那么，推荐都为 `style` 开启 `scoped` 属性

```html
<style lang="scss" scoped>
  body {
    div {
      font-style: italic;
    }
  }
</style>
```

## 在 webpack 中使用 MintUI

### Mint-UI 中按需导入的配置方式

[Github 仓储地址](https://github.com/ElemeFE/mint-ui)

[Mint-UI 官方文档](http://mint-ui.github.io/#!/zh-cn)

#### 导入所有的 MIntUI 组件

```js
导入 Mint-UI
import MintUI from 'mint-ui' //把所有的组件都导入进来
// 这里 可以省略 node_modules 这一层目录
import 'mint-ui/lib/style.css'
// 将 MintUI 安装到 Vue 中
Vue.use(MintUI) // 把所有的组件，注册为全局的组件
```

#### 按需导入 Mint-UI 组件

```js
import { Button } from 'mint-ui'
// 使用 Vue.component 注册 按钮组件
Vue.component(Button.name, Button) // Button.name 可以自定义。
```

#### 在模板区域引入：

`<mt-button type="danger" icon="more" @click="show">default</mt-button>`

如果按需导入 组件，自定义组件名称，可使用自定义的名称做标签名。

### 使用 js components

```js
// 按需导入 Toast 组件
import { Toast } from 'mint-ui'

export default {
  data() {
    return {
      toastInstanse: null
    }
  },
  created() {
    this.getList()
  },
  methods: {
    getList() {
      // 模拟获取列表的 一个 AJax 方法
      // 在获取数据之前，立即 弹出 Toast 提示用户，正在加载数据
      this.show()
      setTimeout(() => {
        //  当 3 秒过后，数据获取回来了，要把 Toast 移除
        this.toastInstanse.close()
      }, 3000)
    },
    show() {
      // Toast("提示信息");
      this.toastInstanse = Toast({
        message: '这是消息',
        duration: -1, // 如果是 -1 则弹出之后不消失
        position: 'top',
        iconClass: 'glyphicon glyphicon-heart', // 设置 图标的类
        className: 'mytoast' // 自定义Toast的样式，需要自己提供一个类名
      })
    }
  }
}
```

## 在 webpack 中使用 MUI

> 注意： MUI 不同于 Mint-UI，MUI 只是开发出来的一套好用的代码片段，里面提供了配套的样式、配套的 HTML 代码段，类似于 Bootstrap； 而 Mint-UI，是真正的组件库，是使用 Vue 技术封装出来的 成套的组件，可以无缝的和 VUE 项目进行集成开发；
> 因此，从体验上来说， Mint-UI 体验更好，因为这是别人帮我们开发好的现成的 Vue 组件；
> 从体验上来说， MUI 和 Bootstrap 类似；
> 理论上，任何项目都可以使用 MUI 或 Bootstrap，但是，MInt-UI 只适用于 Vue 项目；

注意： MUI 并不能使用 npm 去下载，需要自己手动从 github 上，下载现成的包，自己解压出来，然后手动拷贝到项目中使用；

[官网首页](http://dev.dcloud.net.cn/mui/)

[文档地址](http://dev.dcloud.net.cn/mui/ui/)

1. 导入 MUI 的样式表：

```
import '../lib/mui/css/mui.min.css'
```

3. 根据官方提供的文档和 example，尝试使用相关的组件

## 使用 mui 的 tab-top-webview-main 完成分类滑动栏

### 兼容问题

1. 和 App.vue 中的 `router-link` 身上的类名 `mui-tab-item` 存在兼容性问题，导致 tab 栏失效，可以把`mui-tab-item`改名为`mui-tab-item1`，并复制相关的类样式，来解决这个问题；

2. `tab-top-webview-main`组件第一次显示到页面中的时候，无法被滑动的解决方案：

- 先导入 mui 的 JS 文件:

```
import mui from '../../../lib/mui/js/mui.min.js'
```

- 在 组件的 `mounted` 事件钩子中，注册 mui 的滚动事件：

```
	mounted() {
   	// 需要在组件的 mounted 事件钩子中，注册 mui 的 scroll 滚动事件
       mui('.mui-scroll-wrapper').scroll({
         deceleration: 0.0005 //flick 减速系数，系数越大，滚动速度越慢，滚动距离越小，默认值0.0006
       });
 	}
```

3. 滑动的时候报警告：`Unable to preventDefault inside passive event listener due to target being treated as passive. See https://www.chromestatus.com/features/5093566007214080`

```
解决方法，可以加上* { touch-action: pan-y; } 这句样式去掉。
```

原因：（是 chrome 为了提高页面的滑动流畅度而新折腾出来的一个东西） http://www.cnblogs.com/pearl07/p/6589114.html
https://developer.mozilla.org/zh-CN/docs/Web/CSS/touch-action

## 移除严格模式

[babel-plugin-transform-remove-strict-mode](https://github.com/genify/babel-plugin-transform-remove-strict-mode)

## [vue-preview](https://github.com/LS1231/vue-preview)

一个 Vue 集成 PhotoSwipe 图片预览插件

## .babelrc 配置

```json
{
  "presets": ["env", "stage-0"],
  "plugins": ["transform-runtime"]
}
```

## webpack.config.js 最终配置

```js
const path = require('path')
const htmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
  entry: path.join(__dirname, './src/main.js'),
  output: {
    path: path.join(__dirname, './dist'),
    filename: 'bundle.js'
  },
  plugins: [
    new htmlWebpackPlugin({
      template: path.join(__dirname, './src/index.html'),
      filename: 'index.html'
    })
  ],
  module: {
    rules: [
      { test: /\.css$/, use: ['style-loader', 'css-loader'] },
      { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] },
      { test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] },
      {
        test: /\.(jpg|png|gif|bmp|jpeg)$/,
        use: 'url-loader?limit=38000&name=[hash:8]-[name].[ext]'
      },
      {
        test: /\.(ttf|eot|svg|woff|woff2)$/,
        use: 'url-loader'
      },
      { test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ },
      { test: /\.vue$/, use: 'vue-loader' }
    ]
  }
}
```

## Debug：

- `json` 文件中不能写注释

- `json` 文件中不能写注释

- 不是内部命令或外部命令，也不是可运行的程序，

  - 删除 `node_modules` 文件夹 重新 `npm init`

- 有兼容问题的插件：

  ```js
  "devDependencies": {
    "sass-loader": "^7.3.1",
    "webpack": "^3.8.1",
    "webpack-dev-server": "^2.9.3"
  }
  ```

## webpack senior （webpack 4）

**若使用 webpack 4 , 在安装 webpack 的同时还需要安装 webpack-cli**

```bash
# 若 vscode 不能使用 yarn/webpack 命令做一下操作

# 以管理员身份运行vs code
Set-ExecutionPolicy -Scope CurrentUser
RemoteSigned
get-ExecutionPolicy
# => RemoteSigned 修改成功

yarn add webpack webpack-cli --dev
```

### 快速配置

> 最好使用 npm 安装，node-sass yarn 安装不上

#### webpack 以及实时打包工具、启动页面

1. 安装

```bash
npm install webpack webpack-cli html-webpack-plugin webpack-dev-server -D
```

2.  配置

`webpack.config.js`

```javascript
const path = require('path')
const htmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: path.join(__dirname, './src/main.js'),
  output: { path: path.join(__dirname, './dist'), filename: 'bundle.js' },
  plugins: [
    new htmlWebpackPlugin({
      template: path.join(__dirname, './src/index.html'),
      filename: 'index.html'
    })
  ]
}
```

`package.json` 中的 script 添加

```json
{
  "scripts": {
    "dev": "webpack-dev-server --open --port 3000"
  }
}
```

#### 加载器（loader）

1. 安装

**css url 加载器**

```bash
npm install style-loader css-loader sass-loader node-sass url-loader file-loader -D
```

> ps: postcss-loader 插件也可以安装 => 给 CSS3 的属性添加前缀，样式格式校验（stylelint），提前使用 css 的新特性比如：表格布局，更重要的是可以实现 CSS 的模块化，防止 CSS 样式冲突。

作者：最底层的技术渣
链接：https://www.jianshu.com/p/15d51e796dca
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

**babel 加载器**

```bash
# babel-core 8.0.0版本有问题 需要使用 7.1.5
npm install babel-core babel-loader@7.1.5 babel-plugin-transform-runtime babel-preset-env babel-preset-stage-0 -D
```

2. 配置

`webpack.config.js`

```javascript
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: ['style-loader', 'css-loader'] },
      { test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] },
      { test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ },
      { test: /\.(png|gif|jpg|bmp)$/, use: 'url-loader?limit=5000' }
    ]
  }
}
```

`.babelrc`

```javascript
{
  "presets": ["env", "stage-0"],
  "plugins": ["transform-runtime"]
}
```

### 优化项目并打包

1. 新建一个 js 文件 命名：`webpack.pub.config.js`

2. package.json 中新添加一个 script 脚本：`"pub": "webpack --config webpack.pub.config.js"`

3. clean-webpack-plugin 每次发布，删除之前发布的文件

   1. 安装：`npm install clean-webpack-plugin -D`
   2. 配置 `webpack.pub.config.js`中

```javascript
// webpack 4 新写法
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
// plugins 节点中新增：
`new CleanWebpackPlugin()`,
```

4. 分离第三方包

   - `webpack.pub.config.js`

   1. `修改：entry` 节点

   ```javascript
   entry: {
       // 配置入口节点
       app: path.join(__dirname, './src/main.js'),
       vendors: ['jquery'] // 把需要抽离的第三方包名称，放到该数组中
     }
   ```

   2. 新增 `optimization` 节点，与 `entry` 同级（webpack 4 新写法）

   ```javascript
   optimization: {
    splitChunks: {
      cacheGroups: {
        commons: {
          name: 'vendors', // 指定要抽离的入口名称
          chunks: 'initial',
          minChunks: 2,
          filename: 'vendors.js' // 发布时将抽离的第三方包放到该文件中
        }
      }
    }
   }
   ```

5. 分离 css

**webpack 4 必须使用 `mini-css-extract-plugin`**

> 该插件 只能在生产环境中使用，不支持 style-loader 即：<style></style>标签中使用 css，不支持 hrm

> 已内置 CSS 压缩器，无需使用 `optimize-css-assets-webpack-plugin`

1. 新增 `const MiniCssExtractPlugin = require('mini-css-extract-plugin')`
2. 在 plugins 节点中新增

```javascript
new MiniCssExtractPlugin({
  filename: 'css/styles.css'
})
```

3. 修改与 css 相关的 rules

```javascript
{
  test: /\.(sa|sc|c)ss$/,
  use: [
    {
      loader: MiniCssExtractPlugin.loader,
      options: {
        publicPath: '../' // 指定公共路径
      }
    },
    'css-loader',
    'sass-loader'
  ]
}
```

**完整 webpack.pub.config.js 文件配置**

```javascript
const path = require('path')
const htmlWebpackPlugin = require('html-webpack-plugin')
// 插件：每次发布，删除之前发布的文件
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
// css 抽离 webpack4中
const MiniCssExtractPlugin = require('mini-css-extract-plugin')

module.exports = {
  entry: {
    // 配置入口节点
    app: path.join(__dirname, './src/main.js'),
    vendors: ['jquery'] // 把需要抽离的第三方包名称，放到该数组中
  },
  output: { path: path.join(__dirname, './dist'), filename: 'js/bundle.js' },
  plugins: [
    new htmlWebpackPlugin({
      template: path.join(__dirname, './src/index.html'),
      filename: 'index.html'
    }),
    new CleanWebpackPlugin(),
    new MiniCssExtractPlugin({
      filename: 'css/styles.css'
    })
  ],
  optimization: {
    splitChunks: {
      cacheGroups: {
        commons: {
          //打包第三方类库
          name: 'vendors',
          chunks: 'initial',
          minChunks: 2,
          filename: 'js/vendors.js'
        }
      }
    }
  },
  module: {
    rules: [
      {
        test: /\.(sa|sc|c)ss$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              publicPath: '../'
            }
          },
          'css-loader',
          'sass-loader'
        ]
      },
      { test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ },
      {
        test: /\.(png|gif|jpg|bmp)$/,
        use: 'url-loader?limit=5000&name=images/[hash:8]-[name].[ext]' // 优化扩展名及图片存储位置
      }
    ]
  }
}
```
