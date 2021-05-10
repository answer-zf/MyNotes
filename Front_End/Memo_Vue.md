# 备忘(实际项目所遇到的问题在此记录)

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

### watch 的依赖追踪

#### 对象属性监听

```js
// queryData:{name:...,...}

// 方法1：
watch: {
     queryData: {
         handler: function() {
            //do something
         },
         deep: true // 修改了这个queryData中的任何一个属性，都会执行handler这个方法。不过其实这样开销是蛮大的，尤其是对象里面结构嵌套过深的时候
     }
}

// 方法2：
watch: {
     'queryData.name': {
         handler: function() {
            //do something
         },
     }
}

// 方法3:巧用计算属性
computed: {
    getName: function() {
      return this.queryData.name
    }
}
watch: {
     getName: {
         handler: function() {
            //do something
         },
     }
}
```

### mixin / extends

> 两个都可以理解为继承，mixins接收对象数组（可理解为多继承），extends接收的是对象或函数（可理解为单继承）。

#### 生命周期、watch

```javascript

const extend = {
  created () {
    console.log('extends created')
  }
}
const mixin1 = {
  created () {
    console.log('mixin1 created')
  }
}
const mixin2 = {
  created () {
    console.log('mixin2 created')
  }
}
export default {
  extends: extend,
  mixins: [mixin1, mixin2],
  name: 'app',
  created () {
    console.log('created')
  }

// console
// extends created
// mixin1 created
// mixin2 created
// created

```

- 优先调用mixins和extends继承的父类，extends触发的优先级更高，相对于是队列
- watch的值继承规则一样

#### data/computed

```js
// 父类
const extend = {
  data() {
    return {
      name: 'extend name',
    }
  },
}
const mixin1 = {
  data() {
    return {
      name: 'mixin1 name',
    }
  },
}
const mixin2 = {
  data() {
    return {
      name: 'mixin2 name',
    }
  },
}

// 子类
// name = 'name'
export default {
  mixins: [mixin1, mixin2],
  extends: extend,
  name: 'app',
  data() {
    return {
      name: 'name',
    }
  },
}

// name = 'mixin2 name'，extends会被mixins覆盖
export default {
  extends: extend,
  mixins: [mixin1, mixin2],
  name: 'app'
}

// name = 'mixin1 name'，mixins后面继承会覆盖前面的
export default {
  mixins: [mixin2, mixin1],
  extends: extend,
  name: 'app'
}
```

- 结论：子类再次声明，data中的变量都会被重写，以子类的为准。
- 如果子类不声明，data中的变量将会最后继承的父类为准。
- 重写顺序：extend - `mixins[0]` - `mixin[1]` - data（子类再次声明）
- props中属性、methods中的方法和computed的值继承规则一样。

> 综上：extend的优先级最高，mixin 次之，最后为子类

### element / input

> vue项目中element-ui中el-input使用原生JS事件修改值data中数据不同步问题

```html
<template>
  <div>
    <el-input v-model="item.value" v-for="(item, index) in arr" :key="index" @blur="blurHandle"></el-input>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [{ value: 10 }, { value: 50 }],
    }
  },
  methods: {
    blurHandle(e) {
      if (e.target.value > 20) {
        e.target.value = 20
        // e.target.dispatchEvent(new Event('input')) 没有这行代码时，input框显示的数据，于data中的 arr 不同
        // 解开注释，手动绑定
        console.log(this.arr)
      }
    },
  },
}
</script>
```

- 自定义事件

```js
//自定义事件
const inputEvent=new Event('input')

// 手动触发的两种方法

// 1. 这种得需要用window对象监听 相当于$on('input') 
window.addEventListener('input',funciton(){
})
// 2. 手动触发 就这相当vue中$emit('input')
dom.dipatchEvent(inputEvent)
```

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

### ES 6

#### symbol

- Symbol 作为属性名，遍历对象的时候，该属性不会出现在`for...in`、`for...of`循环中，也不会被`Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.stringify()`返回。
- 但是，它也不是私有属性，有一个`Object.getOwnPropertySymbols()`方法，可以获取指定对象的所有 `Symbol` 属性名。该方法返回一个数组，成员是当前对象的所有用作属性名的 `Symbol` 值。

```javascript
const obj = {};
let a = Symbol('a');
let b = Symbol('b');

obj[a] = 'Hello';
obj[b] = 'World';

const objectSymbols = Object.getOwnPropertySymbols(obj);

objectSymbols
// [Symbol(a), Symbol(b)]
```

- `Reflect.ownKeys()`方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。

#### `for ... of`

> 本质：`for...of`循环内部调用的是数据结构的`Symbol.iterator`方法

- `for...of`循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象（比如arguments对象、DOM NodeList 对象）、Generator 对象，以及字符串。
- `for...of`循环调用遍历器接口，数组的遍历器接口只返回具有数字索引的属性。这一点跟`for...in`(为了对象遍历键的循环)循环不一样。

```js
let {keys, values, entries} = Object;
let obj = { a: 1, b: 2, c: 3 };

for (let key of keys(obj)) {
  console.log(key); // 'a', 'b', 'c'
}

for (let value of values(obj)) {
  console.log(value); // 1, 2, 3
}

for (let [key, value] of entries(obj)) {
  console.log([key, value]); // ['a', 1], ['b', 2], ['c', 3]
}
```

### ES 5

- filter()

  - 创建一个新数组，新数组中的元素是通过检查指定数组中符合条件的所有元素(过滤)

- map()

  - 创建一个新数组，新数组中的元素是调用一次提供的函数后的返回（对每个元素的操作后的返回）

- 判断 数组的所有成员是否满足指定的条件

  - some() 只要其中一个为true，就会返回true `一真即真`
  - every() 必须所有都返回true才会返回true `一假即假`

### ES 7

> 将嵌套的数组变成一维数组的过程

- `arr.flat()`

### ES 10

- padStart(len,str)

  - 向左填充 str 字符串，使操作字符串长度等于 len

  - 应用 补 0

    - str1.padStart(2,'0')  // str1 = '5' => '05'

- padEnd(len,str)

  - 反之，向右

## TS

### typeof ArrayInstance[number]

- 作用：将数组每项元素做type

```typescript
const books = [
  { title: "zf", author: "zzzf", pageCount: 1 },
  { title: "df", author: "dddf", pageCount: 2 }
]

type Book = typeof books[number]

// Book type 即 {
//   title: string;
//   author: string;
//   pageCount: number;
// }

```

### InstanceType

- 作用：获取构造函数类型的实例类型
- 示例：获取 element 中的表单类型
  - `const form = ref<InstanceType<typeof ELForm>>()`

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

### 文本选中样式修改

```css
p::selection {
  background-color: #b981d1;
  color: white;
}
p::-moz-selection {
  background-color: #b981d1;
  color: white;
}
p::-webkit-selection {
  background-color: #b981d1;
  color: white;
}
```

## Eslint

### 使用注释取消 报错方法

```javascript
// 1. 在整个文件中取消eslint检查：
/* eslint-disable */
alert(‘foo’);

// 2. 在整个文件中禁用某一项eslint规则的检查：
/* eslint-disable no-alert */
alert(‘foo’);

// 3. 在整个文件中禁用多项eslint规则的检查：
/* eslint-disable no-alert, no-console */
alert(‘foo’);
console.log(‘bar’);

// 4. 针对某一行禁用eslint检查：
alert(‘foo’); // eslint-disable-line

// eslint-disable-next-line
alert(‘foo’);

// 5. 针对某一行的某一具体规则禁用eslint检查：
alert(‘foo’); // eslint-disable-line no-alert

// eslint-disable-next-line no-alert
alert(‘foo’);

// 6. 针对某一行禁用多项具体规则的检查：

alert(‘foo’); // eslint-disable-line no-alert, quotes, semi

// eslint-disable-next-line no-alert, quotes, semi
alert(‘foo’);
```

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
