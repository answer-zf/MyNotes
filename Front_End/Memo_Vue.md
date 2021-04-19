# Vue 相关 备忘

## Vue

### Vue3

#### 覆盖框架样式

> 在 vue3 scss 中 覆盖框架样式 使用 ::v-deep(...) 而 不使用 /deep/ ...

#### dom 操作

- 获取一个元素的DOM

```typescript
// <div ref="zf">cf</div>
import { ref, onMounted } from 'vue';

export default {
  setup() {
    const zf = ref<any>(null)

    onMounted(() => {
      // 等价于 this.$refs.zf
      console.dir(zf.value);
    });

    return {
      zf,
    }

  },
}
```

- 获取多个dom元素

```typescript
// <div :ref="scrollRefs" v-for="(item, index) in arr" :key="index">{{ item }}</div>
import { ref, nextTick } from 'vue';

export default {
  setup() {
    const arr = ref([1, 2, 3])
    // 使用数组获取多个DOM
    const scrollRefsArr = ref([])
    const scrollRefs = (el: unknown) => {
      ;(scrollRefsArr.value as any[]).push(el)
    }

    nextTick(() => {
      // 实例数组
      console.dir(scrollRefsArr.value);
    });

    return {
      arr,
      scrollRefs,
    }
  },
}
```

#### nextTick

> 将回调推迟到下一个 DOM 更新周期之后执行。在更改了一些数据以等待 DOM 更新后立即使用它。

```typescript
nextTick(()=>{
  ...
})
```

#### ref / reactive

- ref 定义基本数据类型 使用 变量名.value 获取 (watchEffect)
- reactive 定义复杂的数据类型 直接获取 (watch)

### Vuex

- mapState,mapMutations... (vuex 映射)

  - `...mapMutations('user', ['updateIsAdmin']),`
  - 'user': modules/user.js
  - 'updateIsAdmin': 模块中 mutations项中的方法名
  - mapState 同理

### dialog

- resetField 的坑

  - 问题：重置到最近一次提交，快速连续提交会出问题(连点)
  - 解决：在 dialog 上加 `destroy-on-close` 属性，用于重置验证信息，或者使用 v-if
  - `destroy-on-close` 关闭时销毁 Dialog 中的元素

### table

- 表头与内容错位

```css
body .el-table th.gutter{
  display: table-cell!important;
}

body .el-table colgroup.gutter{
  display: table-cell!important;
}
```

### 国际化相关

**使用 vue-i18n 的方案做项目的国际化的注意事项：**

- 当使用 element 表格组件进行二次封装时，若 i18n 不更新，需要在表头添加`:header-cell-style="{}"`
  - 使用自定义表头样式 覆盖原有样式，实现国际化组件的实时更新

### import / require

- url 包含变量时只能使用 require 做动态导入
  - const lang = require(`./lang/${key}`) (√)
- import / import() 的方式导入均报错
  - import xx from `./lang/${key}` (x)
  - const a = () => import(`./lang/${key}`)
    - 不建议使用，在循环中使用 报错
    - 但有文档显示 在 vue-router 中的 component 中可以使用，但未测试

## JS

### undefined null ''

[参考文档](https://blog.csdn.net/cauyahui/article/details/86569573)

一般情况设置默认值如下

- 数字：默认值 undefined
- 字符串：默认值 ''
- 对象：默认值 null

### 判断类型

`Object.prototype.toString.call(value)`

### 数组

#### ES 5

- filter()

  - 创建一个新数组，新数组中的元素是通过检查指定数组中符合条件的所有元素(过滤)

- map()

  - 创建一个新数组，新数组中的元素是调用一次提供的函数后的返回（对每个元素的操作后的返回）

- 判断 数组的所有成员是否满足指定的条件

  - some() 只要其中一个为true，就会返回true `一真即真`
  - every() 必须所有都返回true才会返回true `一假即假`

#### 数组扁平化(ES10)

> 将嵌套的数组变成一维数组的过程

- `arr.flat()`

### 字符串

#### ES 7

- padStart(len,str)

  - 向左填充 str 字符串，使操作字符串长度等于 len

  - 应用 补 0

    - str1.padStart(2,'0')  // str1 = '5' => '05'

- padEnd(len,str)

  - 反之，向右

### 异步相关

> 内部执行顺序同步执行，由异步方法，与微任务、宏任务执行方式一样，会在等待队列中做相应的优先处理

Promise.all() 方法用于将多个Promise实例，包装成一个新的Promise实例。

- 接受一个数组作为参数，数组内的成员 需要是 Promise对象的实例
- 所有成员状态都为 成功，返回 所有成员的返回值组成的一个数组（同步），传递给该实例的回调函数
- 若有一个成员被 rejected，则返回 第一个被reject的实例的返回值，传递给该实例的回调函数

#### 微任务、宏任务

> JS执行有同步任务队列和等待任务队列
> 主任务队列存储的都是同步任务
> 等待任务队列存储的都是异步任务
>
> 事件循环: 首先浏览器会把主任务队列中的同步任务挨个全部执行完，然后再去等待任务队列中看哪个任务可以执行了，然后把该执行的任务放到主任务队列中去执行，等这个任务执行完，再去等待任务中看谁可以执行了，再把这个任务放到主任务队列中执行... 如此循环

- 宏任务和微任务是等待任务队列中的异步任务的处理机制
- 微任务的优先级比宏任务的要高（在第一次宏任务执行后，将所有微任务执行完，在执行第二个宏任务）

- 微任务（由JS引擎发起）：

  1. Promise的then的回调函数
  2. async 函数await下面的代码

- 宏任务（由宿主{Node、浏览器}发起）：

  - 定时器：setTimeout/setInterval
  - script（即第一个同步任务就是第一个宏任务）

#### async、await(ES7)

- 提交成功后，立即渲染页面时，不要分在两个函数中写，有异步的问题

- 带有 async 关键字的函数，返回的是一个promise，即成功 、 失败的结果
- await 关键字 所等待的是 返回结果，函数内代码依旧执行

```js
// 示例
async function async1() {
  console.log('async1 start')
  await async2()
  console.log('async1 end')
}
async function async2() {
  console.log('async2')
}
console.log('script start')
setTimeout(function() {
  console.log('setTimeout')
}, 0)
async1()
new Promise(function(resolve) {
  console.log('promise1')
  resolve()
}).then(function() {
  console.log('promise2')
})
console.log('script end')

// 执行步骤：
// script start => async1 start => async2 => promise1 => script end  同步代码
// async1 end => promise2 => setTimeout 优先执行微服务
// 在浏览器打印如上面所示，在node中打印结果不同，node解析与浏览器不同
// 原因：浏览器的Event loop是在HTML5中定义的规范，而node中则由libuv库实现
```

### 原型链

`prototype`（原型对象）是构造函数有的属性
`__proto__`（隐式原型）是所有对象都有的属性

> 实例对象的隐式原型 指向 构造函数的原型对象

### localStorage sessionStorage

- localStorage 里面存储的数据没有过期时间设置
  - 可跨浏览器窗口和选项卡间共享。
  - 多个浏览器窗口和标签页中共享数据，使用 LocalStorage
- 存储在 sessionStorage 里面的数据在页面会话结束时会被清除
  - 页面会话在浏览器打开期间一直保持，并且重新加载或恢复页面仍会保持原来的页面会话
  - 打开多个相同的URL的Tabs页面，会创建各自的sessionStorage
  - 关闭对应浏览器窗口（Window）/ tab，会清除对应的sessionStorage
  - SessionStorage数据独立于其他选项卡和窗口。如果同时打开了两个选项卡，其中一个更新了SessionStorage，则在其他选项卡和窗口中不会反映出来

## CSS

### 溢出省略隐藏

```css
div{
  /* 单行 */
  text-overflow: ellipsis !important;
  white-space: nowrap !important;
  overflow: hidden !important;

  /* 两行 */
  text-overflow: -o-ellipsis-lastline;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  line-clamp: 2;
  -webkit-box-orient: vertical;
}
```

### 垂直对齐调整

vertical-align

- 默认与父元素基线对齐(baseline)
- 当垂直对齐方式出现异常时可使用该属性
  - 比如父元素由于浮动等其他原因导致子元素对齐出现问题时 使用 text-top (把元素的顶端与父元素字体的顶端对齐)

## 项目优化

### 生产阶段 移除 console

- 使用 `babel-plugin-transform-remove-console` babel插件解决

```js
// 项目发布阶段需要用到的 插件数组
const prodPlugins = []
if (process.env.NODE_ENV === 'production') {
  // 在 babel.config.js 按需移除 console
  prodPlugins.push('transform-remove-console')
}

module.exports = {
  presets: ['@vue/cli-plugin-babel/preset'],
  plugins: [
    [
      'component',
      {
        libraryName: 'element-ui',
        styleLibraryName: 'theme-chalk'
      }
    ],
    // 发布产品时候的插件数组
    ...prodPlugins,
    // 路由懒加载
    '@babel/plugin-syntax-dynamic-import'
  ]
}
```

### 配置 生产 开发的入口文件

```js
// vue.config.js
module.exports = {
  chainWebpack: config => {

    // 发布模式
    config.when(process.env.NODE_ENV === 'production', config => {

      config
        .entry('app')
        .clear()
        .add('./src/main-prod.js')

      // 将 import 引入的第三方包，改为 cdn
      config.set('externals', {
        vue: 'Vue',
        'vue-router': 'VueRouter',
        axios: 'axios',
        echarts: 'echarts',
        nprogress: 'NProgress',
        'element-ui': 'ELEMENT'
      })

      // index.html 添加 cdn
      // main-prod.js 注释 import 引入的第三方包（只注释导入的 css）
      // <!-- css -->
      // <!-- nprogress-->
      // <link rel="stylesheet" href="https://cdn.staticfile.org/nprogress/0.2.0/nprogress.min.css">
      //
      // <!-- element-ui -->
      // <link rel="stylesheet" href="https://cdn.staticfile.org/element-ui/2.14.0/theme-chalk/index.min.css">
      //
      // <!-- js -->
      // <script src="https://cdn.staticfile.org/vue/2.6.11/vue.min.js"></script>
      // <script src="https://cdn.staticfile.org/vue-router/3.2.0/vue-router.min.js"></script>
      // <script src="https://cdn.staticfile.org/axios/0.20.0/axios.min.js"></script>
      // <script src="https://cdn.staticfile.org/echarts/4.9.0-rc.1/echarts.min.js"></script>
      // <script src="https://cdn.staticfile.org/nprogress/0.2.0/nprogress.min.js"></script>
      // <script src="https://cdn.staticfile.org/element-ui/2.14.0/index.min.js"></script>

      config.plugin('html').tap(args => {
        // 使用 htmlWebpackPlugin.options.xxx 在 html 调用
        args[0].isProd = true
        args[0].title = '后台管理系统'
        return args
      })

    })

    // 开发模式
    config.when(process.env.NODE_ENV === 'development', config => {
      config
        .entry('app')
        .clear()
        .add('./src/main-dev.js')

      config.plugin('html').tap(args => {
        args[0].isProd = false
        args[0].title = '后台管理系统'
        return args
      })
    })
  }
}
```
