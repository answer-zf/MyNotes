# 备忘(实际项目所遇到的问题在此记录)

- [备忘(实际项目所遇到的问题在此记录)](#备忘实际项目所遇到的问题在此记录)
  - [Vue](#vue)
    - [Vue3](#vue3)
      - [覆盖框架样式](#覆盖框架样式)
      - [dom操作](#dom操作)
      - [nextTick](#nexttick)
      - [ref/reactive](#refreactive)
    - [watch的依赖追踪](#watch的依赖追踪)
      - [对象属性监听](#对象属性监听)
    - [在created时获取dom方法(nextTick)](#在created时获取dom方法nexttick)
    - [事件总线](#事件总线)
      - [初始化](#初始化)
      - [派发事件](#派发事件)
      - [接收事件](#接收事件)
    - [mixin/extends](#mixinextends)
      - [生命周期、watch](#生命周期watch)
      - [data/computed](#datacomputed)
    - [Element](#element)
      - [input与原生事件冲突](#input与原生事件冲突)
      - [button点击后选中状态不取消](#button点击后选中状态不取消)
      - [dialog > resetField问题](#dialog--resetfield问题)
      - [scrollbar 的使用](#scrollbar-的使用)
      - [table 表头与内容错位](#table-表头与内容错位)
      - [国际化table表头实时更新问题](#国际化table表头实时更新问题)
    - [Vuex](#vuex)
    - [import/require](#importrequire)
  - [JS](#js)
    - [undefined / null / ''](#undefined--null--)
    - [判断类型](#判断类型)
    - [js 精度保持](#js-精度保持)
    - [ES 6](#es-6)
      - [symbol](#symbol)
      - [`for...of`](#forof)
    - [ES 5](#es-5)
    - [ES 7](#es-7)
    - [ES 10](#es-10)
  - [TS](#ts)
    - [typeof ArrayInstance[number]](#typeof-arrayinstancenumber)
    - [InstanceType](#instancetype)
  - [CSS](#css)
    - [溢出省略隐藏](#溢出省略隐藏)
    - [垂直对齐调整](#垂直对齐调整)
    - [文本选中样式修改](#文本选中样式修改)
  - [Eslint](#eslint)
    - [使用注释取消 报错方法](#使用注释取消-报错方法)
  - [项目优化](#项目优化)
    - [tailwind](#tailwind)
      - [添加 tailwind postcss autoprefixer stylelint](#添加-tailwind-postcss-autoprefixer-stylelint)
      - [配置样式前缀autoprefixer](#配置样式前缀autoprefixer)
      - [tailwind 框架](#tailwind-框架)
      - [配置 stylelint css 验证](#配置-stylelint-css-验证)
    - [生产阶段 移除 console](#生产阶段-移除-console)
    - [配置 生产 开发的入口文件](#配置-生产-开发的入口文件)
    - [ssr / 预渲染](#ssr--预渲染)
      - [预渲染](#预渲染)
      - [SSR](#ssr)
        - [使用官网推荐 的 vue-server-renderer](#使用官网推荐-的-vue-server-renderer)
        - [使用 nust.js(基于vue)](#使用-nustjs基于vue)

## Vue

### Vue3

#### 覆盖框架样式

> 在 vue3 scss 中 覆盖框架样式 使用 ::v-deep(...) 而 不使用 /deep/ ...

#### dom操作

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

#### ref/reactive

- ref 定义基本数据类型 使用 变量名.value 获取 (watchEffect)
- reactive 定义复杂的数据类型 直接获取 (watch)

### watch的依赖追踪

#### 对象属性监听

```js
// queryData:{name:...,...}

// 方法1：
watch: {
     queryData: {
         handler: function() {
            //do something
         },
         // 修改了这个queryData中的任何一个属性，
        //  都会执行handler这个方法。不过这样开销会很大，尤其是对象里面结构嵌套过深的时候
         deep: true 
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

### 在created时获取dom方法(nextTick)

```javascript
created() {
  this.$nextTick(() => {
    console.log(this.$refs.btn)
  })
},
```

### 事件总线

#### 初始化

```javascript
// event-bus.js
import Vue from 'vue'
export const EventBus = new Vue()

// main
import {EventBus} from './event-bus.js'
Vue.prototype.$EventBus=EventBus

// 或者直接再main中初始化
Vue.prototype.$EventBus = new Vue()
```

#### 派发事件

- `this.$EventBus.$emit(发送的事件名,传递的参数)`

```javascript
methods: {
  sendMsg() {
    // 调用全局Vue实例中的$EventBus事件总线中的$emit属性，
    // 发送事件"aMsg",并携带A组件中的Msg
    this.$EventBus.$emit("aMsg", this.MsgA);
  }
}
```

#### 接收事件

- `this.$EventBus.$on(监听的事件名, 回调函数)`
- 有事件监听，必须在组件销毁前做移除

```javascript
mounted() {
  // 调用全局Vue实例中的$EventBus事件总线中的$on属性,
  // 监听其他组件发送到事件总线中的aMsg事件
  this.$EventBus.$on("aMsg", (data) => {
    //将A组件传递过来的参数data赋值给msgB
    this.msgB = data;
  });
},
beforeDestroy(){
  // 只要做了监听，必须再销毁之前移除监听事件，防止内存泄漏
  // 移除监听事件"aMsg"
  this.$EventBus.$off("aMsg")
}
```

### mixin/extends

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

### Element

#### input与原生事件冲突

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

#### button点击后选中状态不取消

- 点击后，偶尔出现 选中状态不能取消的情况

```typescript
// <el-button @click="clickHandle(id, $event)"></el-button>

// MSInputMethodContext typescript 原生类型，不确定合理性，可直接用 any
const handleClick = (id: string, event: MSInputMethodContext) => {
  // * 解决 按钮点击后状态不消失的bug
  let target = event.target as HTMLElement
  if (target.nodeName == 'SPAN') {
    target = event.target.parentNode as HTMLElement
  }
  target.blur()
  console.log(id, target)
}
```

#### dialog > resetField问题

- resetField 的坑

  - 问题：重置到最近一次提交，快速连续提交会出问题(连点)
  - 解决：在 dialog 上加 `destroy-on-close` 属性，用于重置验证信息，或者使用 v-if
  - `destroy-on-close` 关闭时销毁 Dialog 中的元素

#### scrollbar 的使用

- 在纵向滚动避免显示原生横向滚动条，需要在外层包裹一个元素设定定高
- scrollbar 高度设置 100%

```vue
<template>
  <div style="height: 200px;">
    <el-scrollbar style="height: 100%;" class="showScrollBar">
    ...
    </el-scrollbar>
  </div>
</template>

<style lang="scss" scoped>
.showScrollBar {
  /deep/ .el-scrollbar__wrap {
    overflow-x: hidden; // 做纵向滚动设置
  }
}
</style>
```

#### table 表头与内容错位

- 表头与内容错位

```css
body .el-table th.gutter{
  display: table-cell!important;
}

body .el-table colgroup.gutter{
  display: table-cell!important;
}
```

#### 国际化table表头实时更新问题

**使用 vue-i18n 的方案做项目的国际化的注意事项：**

- 当使用 element 表格组件进行二次封装时，若 i18n 不更新，需要在表头添加`:header-cell-style="{}"`
  - 使用自定义表头样式 覆盖原有样式，实现国际化组件的实时更新

### Vuex

- mapState,mapMutations... (vuex 映射)

  - `...mapMutations('user', ['updateIsAdmin']),`
  - 'user': modules/user.js
  - 'updateIsAdmin': 模块中 mutations项中的方法名
  - mapState 同理

### import/require

- url 包含变量时只能使用 require 做动态导入
  - const lang = require(`./lang/${key}`) (√)
- import / import() 的方式导入均报错
  - import xx from `./lang/${key}` (x)
  - const a = () => import(`./lang/${key}`)
    - 不建议使用，在循环中使用 报错
    - 但有文档显示 在 vue-router 中的 component 中可以使用，但未测试

## JS

### undefined / null / ''

[参考文档](https://blog.csdn.net/cauyahui/article/details/86569573)

一般情况设置默认值如下

- 数字：默认值 undefined
- 字符串：默认值 ''
- 对象：默认值 null

### 判断类型

`Object.prototype.toString.call(value)`

### js 精度保持

- 保持 0.1 + 0.2 的精度

```javascript
console.log(0.1+0.2) // 0.30000000000000004

(0.1+0.2).toFixed(5) // '0.30000' type:string

parseFloat((0.1+0.2).toFixed(5)) // 0.3
```

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

#### `for...of`

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

```javascript
let arr=[2,4,1,5,3,1];

let res1=arr.map(function (item,index,array) {
  // return array[index]; //用这种方法也可以获取到当前处理的元素
  return item>1;
});

let res2=arr.filter(function (item,index,array) {
  return item>1;
});
console.log(res1,res2); //[ true, true, false, true, true, false ] [ 2, 4, 5, 3 ]
```

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

### tailwind

#### 添加 tailwind postcss autoprefixer stylelint

#### 配置样式前缀autoprefixer

> 如果 项目中没有 tailwind ，css 的优化只完成该项即可实现前缀的补充。

- `npm i postcss autoprefixer -D`

- 由于 vue 对 postcss 做了内置的配置，安装完毕插件以后配置 `.browserslistrc`即可

    ```config
    > 1%
    last 3 versions
    not ie <= 8
    chrome >= 14
    safari >= 3
    ios >= 8
    android >= 4.0
    ```

#### tailwind 框架

> 安装tailwind 同时需要配合 postcss autoprefixer stylelint（css代码验证） 使用

1. 安装

   - `npm install tailwindcss@npm:@tailwindcss/postcss7-compat @tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9 -D`
   - 由于使用最新的版本，不能对 postcss 8 兼容，固使用指定的兼容版本。

2. 配置 对 tailwind 做优化配置: `tailwind.config.js`

    ```javascript
    module.exports = {
      purge: ['./src/**/*.{vue,js,ts,jsx,tsx}'],
      darkMode: false, // or 'media' or 'class'
      theme: {
        extend: {},
      },
      variants: {
        extend: {},
      },
      plugins: [],
    }
    ```

3. vue 中内置了 对 postcss autoprefixer 的配置,只需要添加postcss 对tailwind 的 autoprefixer 的支持即可,配置：`postcss.config.js`

    ```javascript
    module.exports = {
      plugins: [require('tailwindcss'), require('autoprefixer')],
    }
    ```

#### 配置 stylelint css 验证

1. 安装

   - `npm i stylelint stylelint-config-standard -D`

2. 配置

   - 根目录下，添加 `.stylelintrc.js` 文件

   - 添加如下配置

      ```javascript
      module.exports = {
        processors: [],
        plugins: [],
        extends: 'stylelint-config-standard',
        ignoreFiles: ['node_modules/**', 'dist/**'],
        rules: {
          // 忽略 at rule 验证，针对 tailwind 的配置。
          'at-rule-no-unknown': [
            true,
            {
              ignoreAtRules: ['tailwind', 'layer', 'apply', 'variants', 'responsive', 'screen'],
            },
          ],
          // 忽略 ::v-deep( .zf p){...} vue3 中的修改框架自带配置。
          'selector-pseudo-element-no-unknown': [
            true,
            {
              ignorePseudoElements: ['v-deep'],
            },
          ],
        },
      }
      ```

3. 添加 vscode 工作区配置 `.vscode/settings.json`

    ```javascript
    {
      // 关闭重复的 vue template 内的校验
      // "vetur.validation.template": false,
      // 在 vue 的 template 中可使用 ts提示
      "vetur.experimental.templateInterpolationService": true,
      // ? 对上面的功能的校验，选择是否关闭。
      // ? "vetur.validation.interpolation": false,
      // tailwind css 中的 atRules 不被识别
      // 忽略 scss 中的 atRules 验证
      "scss.lint.unknownAtRules": "ignore",
      // 直接关闭 css 验证，使用 stylelint 全权代理
      "css.validate": false
    }
    ```

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

### ssr / 预渲染

#### 预渲染

> 改善少数营销页面 SEO，将前端组件作为完全静态的站点，进行渲染。

1. 将路由模式改为 history

    ```javascript
    const router = new VueRouter({
      mode: 'history',
      base: process.env.BASE_URL, // 默认 / ,即导出后所在的根目录，可设置多级目录
      routes
    })
    ```

2. 安装插件

   - `npm i prerender-spa-plugin -D`

3. 插件配置

```javascript
// vue.config.js
const path = require('path')
const PrerenderSPAPlugin = require('prerender-spa-plugin')
// 调用渲染器
const Renderer = PrerenderSPAPlugin.PuppeteerRenderer

module.exports = {
  configureWebpack: {
    plugins: [
      // 预渲染的配置
      new PrerenderSPAPlugin({
        // 静态资源目录
        staticDir: path.join(__dirname, './dist'),
        // 需要预加载的路由
        routes: ['/', '/about'],
        // 没有这个配置没有预编译
        renderer: new Renderer({
          // 注入信息
          inject: {
            foo: 'bar'
          },
          headless: false,
          // 在项目入口使用 document.dispatchEvent(new Event('render-event'))
          // 在 main.js 中 document.dispatchEvent(new Event('render-event'))，两者的事件名称要对应上
          renderAfterDocumentEvent: 'render-event'
        })
      })
    ]
  }
}

// main.js

new Vue({
  router,
  render: (h) => h(App),
  // 添加事件调用
  mounted() {
    document.dispatchEvent(new Event('render-event'))
  }
}).$mount('#app')
```

#### SSR

##### 使用官网推荐 的 vue-server-renderer

1. 安装相应的包文件

   - `npm init --y`
   - `npm i vue express vue-server-renderer -S`

2. 新建 server.js 文件

    ```javascript
    const Vue = require('vue')
    const express = require('express')()
    // 创建服务端渲染器
    const renderer = require('vue-server-renderer').createRenderer()

    // 创建 vue 实例
    const app = new Vue({
      template: `<div>hello world</div>`,
    })

    // 服务端渲染的核心
    // 通过 vue-server-renderer 插件中的renderToString()方法
    // 将vue实例转换为字符串插入到html文件
    express.get('/', (req, res) => {
      renderer.renderToString(app, (err, html) => {
        console.log(html)
        if (err) {
          return res.state(500).end('运行错误')
        }
        res.send(`
            <!DOCTYPE html>
            <html lang="en">
            <head>
              <meta charset="UTF-8" />
              <title>vue2 SSR 渲染</title>
            </head>
            <body>
              ${html}
            </body>
            </html>
        `)
      })
    })

    express.listen(8111, () => {
      console.log('服务器已启动...')
    })
    ```

3. 运行 `node server.js`

##### 使用 nust.js(基于vue)

1. 安装

   - vue-cli 脚手架
   - 全局安装 create-nust-app（`npm i create-nuxt-app -g`）

2. 创建项目

   - `npx create-nuxt-app nust-ssr`

3. 重要的配置文件 `nuxt.config.js`

4. 路由

   - nuxt 路由自动生成

    ```vue
    <!-- 主文件 --> 
    <template>
      <div class="container">
        <h2>nuxt start</h2>

        <ul>
          <li> <nuxt-link :to="{ name: 'about' }"> about </nuxt-link> </li>
          <li> <nuxt-link :to="{ name: 'user', params: { id: 123 } }"> user </nuxt-link> </li>
          <li> <a href="/user/zf"> user - 动态id</a> </li>
        </ul>
      </div>
    </template>
    <!-- /pages/user/index.vue --> 
    <template>
      <div class="container">
        <h2>user</h2>
        <p>id:{{ $route.params.id }}</p>
        <nuxt-link :to="{ name: 'index' }">
          to home
        </nuxt-link>
      </div>
    </template>

    <!-- 使用动态路由，文件必须以下划线的方式命名，名字和文件内的键对应 -->
    <!-- /pages/user/_id.vue --> 
    <template>
      <div class="container">
        <h2>user - 动态id</h2>
        <p>id:{{ $route.params.id }}</p>
        <nuxt-link :to="{ name: 'index' }">
          to home
        </nuxt-link>
      </div>
    </template>

    <script>
    export default {
      validate({ params }) {
        return params.id === 'zf'
      },
    }
    </script>
    ```

5. 目录

   - `static`中的文件不会被打包构建处理
   - 会被映射到应用的目录之下

6. 异步数据

```vue
<!-- 
static\temData.json 模拟数据
{
  "code": 0,
  "msg": "临时数据",
  "data": {
    "title": "zzf",
    "desc": "ddddf"
  }
}
-->

<template>
  <div class="container">
    <h2>user</h2>
    <p>id:{{ $route.params.id }}</p>
    <p>data {{ zf }}</p>
    <p>asyncData {{ info.title }}</p>
    <nuxt-link :to="{ name: 'index' }">
      to home
    </nuxt-link>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  asyncData() {
    return axios({
      methods: 'get',
      url: '/temData.json',
    }).then(_data => {
      return { info: _data.data.data }
    })
  },
  data() {
    return {
      zf: 'zf',
    }
  },
  head() {
    return {
      title: this.info.title,
    }
  },
}
</script>
```
