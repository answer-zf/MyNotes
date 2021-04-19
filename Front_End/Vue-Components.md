## Vue 中的组件

### 定义：

什么是组件： 组件的出现，就是为了拆分 Vue 实例的代码量的，能够让我们以不同的组件，来划分不同的功能模块，将来我们需要什么样的功能，就可以去调用对应的组件即可；
组件化和模块化的不同：

- 模块化： 是从代码逻辑的角度进行划分的；方便代码分层开发，保证每个功能模块的职能单一；
- 组件化： 是从 UI 界面的角度进行划分的；前端的组件化，方便 UI 组件的重用；

### 全局组件定义的三种方式

定义组件的时候，如果要定义全局的组件， `Vue.component('组件的名称', {})`

1. 使用 `Vue.extend` 配合 `Vue.component` 方法：

   ```js
   // 使用 Vue.extend 来创建全局的Vue组件
   var com1 = Vue.extend({
     template: '<h3>Lorem ipsum dolor sit amet consectetur</h3>'
   })
   // 使用 Vue.component('组件的名称', 创建出来的组件模板对象)
   Vue.component('myCom1', com1)
   ```

   简写：

   ```js
   Vue.component(
     'myCom1',
     Vue.extend({
       template: '<h3>Lorem ipsum dolor sit amet consectetur</h3>'
     })
   )
   ```

   - 第一个参数: 组件的名称, 将来在引用组件的时候, 就是一个标签形式 来引入 它的
   - 第二个参数: `Vue.extend` 创建的组件 ,其中 `template` 就是组件将来要展示的 HTML 内容

   如果要使用组件，直接，把组件的名称，以 HTML 标签的形式，引入到页面中，即可

   ```html
   <div id="app">
     <my-com1></my-com1>
   </div>
   ```

   - 如果使用 Vue.component 定义全局组件的时候，组件名称使用了 驼峰命名，那么在使用组件的时候，只能在字符串模板中用驼峰的方式使用组件，但是在普通的标签模板中，需要把 大写的驼峰改为小写的字母，同时，两个单词之前，使用 - 链接
   - 如果不使用驼峰,则直接拿名称来使用即可

   ##### 组件注意事项

   - 组件参数的 data 值必须是函数同时这个函数要求返回一个对象
     - 使用函数的作用：形成闭包的环境，使每个组件都拥有独立的数据
   - 组件模板必须是单个根元素
   - 组件模板的内容可以是模板字符串

2. 直接使用 Vue.component 方法：

   ```js
   Vue.component('myCom1', {
     template: '<h3>Lorem ipsum dolor sit amet consectetur</h3>'
   })
   ```

3. 将模板字符串，定义到 `template` 元素中：

   ```js
   Vue.component('myCom1', {
     template: '#tmpl'
   })
   ```

   在 被控制的 `#app` 外面,使用 `template` 元素,定义组件的 HTML 模板结构

   ```html
   <div id="app">
     <my-Com1></my-Com1>
   </div>
   <template id="tmpl">
     <div>
       <h3>Lorem ipsum dolor sit amet consectetur</h3>
     </div>
   </template>
   ```

`注意 : 不论是哪种方式创建出来的组件,组件的 template 属性指向的模板内容,必须有且只能有唯一的一个根元素`

### 私有组件定义

- 局部组件只能在注册他的父组件中使用

```js
var vm2 = new Vue({
  el: '#app2',
  components: {
    myCom2: {
      template: '<h3>consectetur adipisicing elit. Commodi, nihil?</h3>'
    }
  }
})
```

`template: '#tmpl2'`同理全局定义方式。

### 组件中展示数据和响应事件

1. 组件可以有自己的 `data` 数据

   - 组件的 `data` 和 实例的 `data` 有点不一样,实例中的 `data` 可以为一个对象,但是 组件中的 data 必须是一个方法，而且这个方法内部必须返回一个对象
   - 组件中 的`data` 数据,使用方式,和实例中的 `data` 使用方式完全一样

   ```js
   Vue.component('vuecontent', {
     template: '<h2>this is h2,------data is {{msg}}</h2>',
     data() {
       return {
         msg: 'Lorem ipsum dolor sit amet'
       }
     }
   })
   ```

2. 组件可以有自己的 `methods` 响应时间。

   - 使用同 `vue` 实例

### 组件间切换

#### 使用 flag 标识符结合 v-if 和 v-else 切换组件

页面结构：

```html
<div id="app">
  <a href="#" @click.prevent="flag = true">login</a>
  <a href="#" @click.prevent="flag = false">register</a>
  <login v-if="flag"></login>
  <register v-else="flag"></register>
</div>
```

vue 实例：

```js
Vue.component('login', {
  template: '<h3>login</h3>'
})
Vue.component('register', {
  template: '<h3>register</h3>'
})
```

#### 使用 components 属性定义局部子组件

Vue 提供了 `component` ,来展示对应名称的组件

`component` 是一个占位符, `:is` 属性,可以用来指定要展示的组件的名称

页面结构：

```html
<div id="app">
  <a href="#" @click.prevent="comName = 'login'">login</a>
  <a href="#" @click.prevent="comName = 'register'">register</a>
  <component :is="comName"></component>
</div>
```

vue 实例：

```js
var vm = new Vue({
  el: '#app',
  data: {
    comName: 'login' // 当前 component 中的 :is 绑定的组件的名称
  },
  methods: {}
})
```

#### 组件切换的动画应用：

使用 `transition`元素包裹 `component`元素，并设置样式即可

并且通过 `transition`元素中的 `mode` 属性,设置组件切换时候的 模式

页面结构：

```html
<transition mode="out-in">
  <component :is="comName"></component>
</transition>
```

css 样式：

```css
.v-enter,
.v-leave-to {
  opacity: 0;
  transform: translateX(100px);
}
.v-enter-active,
.v-leave-active {
  transition: all 0.8s ease;
}
```

### 组件传值：

#### 父组件向子组件传值：

- 本质：父组件向子组件传递数据 `data`

子组件中，默认无法访问到 父组件中的 `data` 上的数据 和 `methods` 中的方法

父组件，可以在引用子组件的时候， 通过 属性绑定`(v-bind:)` 的形式, 把 需要传递给 子组件的数据，以属性绑定的形式，传递到子组件内部，供子组件使用

```html
<div id="app">
  <com1 :parentmsg="msg"></com1>
</div>
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      msg: '132'
    },
    components: {
      com1: {
        template: '<h1>9-----{{parentmsg}}</h1>',
        props: [
          // 把父组件传递过来的 parentmsg 属性，
          // 先在 props 数组中，定义一下，这样，才能使用这个数据
          'parentmsg'
        ]
      }
    }
  })
</script>
```

- 与 组建中的 `data`的 区别：
  - 子组件中的 `data` 数据，并不是通过 父组件传递过来的，而是子组件自身私有的，比如： 子组件通过 Ajax ，请求回来的数据，都可以放到 `data` 身上；且 `data` 上的数据，都是可读可写的
  - 组件中的 所有 `props` 中的数据，都是通过 父组件传递给子组件的；且`props` 中的数据，都是只读的，无法重新赋值
- 父组件发送的形式是以属性的形式绑定值到子组件身上。
- 然后子组件用属性 props 接收
- props 传递数据原则：单向数据流，只允许父组件向自组建传递数据，不允许子组件直接操作，props 中的数据
- 在 props 中使用驼峰形式，模板中需要使用短横线的形式字符串形式的模板中没有这个限制

#### 子组件向父组件传值：

- 父组件向子组件传递方法 `methods`

父组件向子组件 传递 方法，使用的是 事件绑定机制； `v-on`, 当我们自定义了 一个 事件属性之后，那么，子组件就能够，通过某些方式，来调用 传递进去的 这个 方法

```html
<div id="app">
  <com1 @func="show"></com1> // 切记 show
  不能加（），不能传参，表示的是方法而不是调用！
  <com1 @func="show($event)"></com1>
</div>
<template id="tmpl">
  <div>
    <h1>this is component for son</h1>
    <input type="button" value="touch" @click="touch" />
  </div>
</template>
<script>
  var com1 = {
    // 定义了一个字面量类型的 组件模板对象
    template: '#tmpl', // 通过指定了一个 Id, 表示要去加载这个指定Id的 template 元素中的内容，当作组件的HTML结构
    data() {
      return {
        sonmsg: {
          name: 'zf',
          age: 18
        }
      }
    },
    methods: {
      touch() {
        // 当点击子组件按钮的时候，通过 this.$emit 拿到父组件传递过来的 func 方法，并调用
        this.$emit('func', this.sonmsg)
      }
    }
  }
  var vm = new Vue({
    el: '#app',
    data: {
      datamsgFromSon: null
    },
    methods: {
      show(data) {
        this.datamsgFromSon = data
      }
    },
    components: {
      com1
    }
  })
</script>
```

#### 利用传值原理，在 localStorage 存储数据：

将数据存放在内存中使用 h5 的新特性 `localStorage`

- `localStorage` 用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去删除。

- `localStorage` 中的键值对总是以字符串的形式存储 ，而且是只读的
- 保存数据：`localStorage.setItem('myCat', 'Tom')`
- 读取数据：`localStorage.getItem('myCat')`

页面结构：

```html
<div id="app">
  <comment-box @load="loadComments"></comment-box>
  <ul class="list-group">
    <li class="list-group-item" v-for="item in list" :key="item.id">
      <span class="badge">person: {{item.name}}</span>
      <div>{{item.content}}</div>
    </li>
  </ul>
</div>
<template id="tmpl">
  <div>
    <div class="form-group">
      <label for="">person:</label>
      <input type="text" class="form-control" v-model="name" />
    </div>
    <div class="form-group">
      <label for="">content:</label>
      <textarea class="form-control" v-model="content"></textarea>
    </div>
    <div class="form-group">
      <input type="button" value="add" class="btn btn-primary" @click="add" />
    </div>
  </div>
</template>
```

vue 实例：

```js
var commentBox = {
  data() {
    return {
      name: '',
      content: ''
    }
  },
  template: '#tmpl',
  methods: {
    add() {
      var tmpl = {
        id: Date.now(),
        name: this.name,
        content: this.content
      }
      var list = JSON.parse(localStorage.getItem('cmts') || '[]')
      console.log(list)
      list.unshift(tmpl)
      localStorage.setItem('cmts', JSON.stringify(list))
      this.name = this.content = ''
      this.$emit('load')
    }
  }
}
var vm = new Vue({
  el: '#app',
  data: {
    list: null
  },
  methods: {
    loadComments() {
      var list = JSON.parse(localStorage.getItem('cmts') || '[]')
      this.list = list
    }
  },
  components: {
    commentBox
  },
  created() {
    this.loadComments()
  }
})
```

### 使用 this.\$refs 来获取元素和组件

获取 DOM 节点：

1. 目标标签中添加 `ref` 属性，自定义名称

   ```html
   <div id="app">
     <input type="button" value="getDOM" @click="getDOM" />
     <h3 ref="content">
       Lorem ipsum dolor sit amet consectetur adipisicing elit.
     </h3>
   </div>
   ```

2. 在 vue 实例中使用 `this.$refs.自定义名称.innerText`

   ```js
   methods: {
     getDOM() {
       console.log(this.$refs.content.innerText);
     }
   }
   ```

获取组件的数据和方法：
`this.$refs.自定义名称.数据名 / 方法名()`

## Vue 中的路由

1. **后端路由：**对于普通的网站，所有的超链接都是 URL 地址，所有的 URL 地址都对应服务器上对应的资源；

2. **前端路由：**对于单页面应用程序来说，主要通过 URL 中的 hash(#号)来实现不同页面之间的切换，同时，hash 有一个特点：HTTP 请求中不会包含 hash 相关的内容；所以，单页面程序中的页面跳转主要用 hash 实现；

3. 在单页面应用程序中，这种通过 hash 改变来切换页面的方式，称作前端路由（区别于后端路由）；

### 路由的基本使用

1. 安装 `vue-router` 路由模块 （`script`标签引包）
2. 创建一个路由对象， 当 导入 `vue-router` 包之后，在 window 全局对象中，就有了一个 路由的构造函数，叫做 `VueRouter`
3. 在 `new` 路由对象的时候，可以为 构造函数，传递一个配置对象
4. 每个路由规则，都是一个对象，这个规则对象，身上，有两个必须的属性：
   - 属性 1 是 `path`， 表示监听 哪个路由链接地址；
   - 属性 2 是 `component`， 表示，如果 路由是前面匹配到的 `path` ，则展示 `component` 属性对应的那个组件
     - 注意： `component` 的属性值，必须是一个 组件的模板对象， 不能是 组件的引用名称；
   - 属性 2 可以设置 `redirect`，表示 ，重定向所匹配到的 `path`， 属性值为 所重定向的路由
     - 注意：这里的 `redirect` 和 Node 中的 `redirect` 完全是两码事
5. 将路由规则对象，注册到 vm 实例上，用来监听 URL 地址的变化，然后展示对应的组件
6. `router-view` 是 `vue-router` 提供的元素，专门用来 当作占位符的，将来，路由规则，匹配到的组件，就会展示到这个 `router-view` 中去
   - 可以把 `router-view` 认为是一个占位符

```html
<div id="app">
  // router-link 默认渲染为一个a 标签，添加 tag 属性可将标签改为值所对应的标签
  <router-link to="/login">login</router-link> // 不论是不是 a
  标签都有点击切换的功能
  <router-link to="/register">register</router-link>
  <transition mode="out-in">
    // 再加 过渡样式 即可使用动画
    <router-view></router-view>
  </transition>
</div>
<script>
  var login = {
    template: '<h3>login</h3>'
  }
  var register = {
    template: '<h3>register</h3>'
  }
  const routerObj = new VueRouter({
    routes: [
      // 路由匹配规则
      { path: '/', redirect: '/login' }, // 设置路由重定向
      { path: '/login', component: login },
      { path: '/register', component: register }
    ],
    linkActiveClass: 'zf-active' // 自定义高亮 class 类名，修改 class 即可自定义高亮样式
  }) // 默认值：'router-link-active'
  // 创建 Vue 实例，得到 ViewModel
  var vm = new Vue({
    el: '#app',
    data: {},
    methods: {},
    router: routerObj
  })
</script>
```

url 地址改变 -> 触发路由监听事件（url 改变后进行路由规则的匹配） -> 匹配后展示所对应的 `component` 组件 放到 `router-view` 区域

### 路由中的参数

#### 在路由规则中定义参数：

1. 查询字符串传参：`(query)`

   - 使用 查询字符串，给路由传递参数，则 不需要修改 路由规则的 path 属性
   - 在 vue 实例中，使用 `this.$route.query.key` 即可获取参数

   - 在模板对象中获取参数，在插值表达式中，可省略`this.`

   - 支持多个参数传递

   ```html
   <div id="app">
     <router-link to="/login?id=10&name=123">login</router-link>
     <router-link to="/register">register</router-link>
     <router-view></router-view>
   </div>

   <script>
     var login = {
       template:
         '<h3>login - page {{ $route.query.id }} -- {{ $route.query.name }}</h3>',
       created() {
         console.log(this.$route.query.id)
       }
     }
     var register = {
       template: '<h3>register - page</h3>'
     }
     const router = new VueRouter({
       routes: [
         { path: '/login', component: login },
         { path: '/register', component: register }
       ]
     })
     var vm = new Vue({
       el: '#app',
       data: {},
       methods: {},
       router
     })
   </script>
   ```

2) `:id`方式传参：`(params)`

   ```js
   var login = {
     template:
       '<h3>login - page -- {{ $route.params.id }} --{{ $route.params.name }}</h3>',
     created() {
       console.log(this.$route.params.id)
     }
   }
   var register = {
     template: '<h3>register - page</h3>'
   }
   const router = new VueRouter({
     routes: [
       { path: '/login/:id/:name', component: login },
       { path: '/register', component: register }
     ]
   })
   ```

### 路由跳转

在网页中，有两种跳转方式：

方式 1： 使用 a 标签 的形式叫做 标签跳转

方式 2： 使用 `window.location.href` 的形式，叫做 编程式导航

#### this.$route or this.$router

`this.$route` 指的是：路由【参数对象】，所有路由中的参数，`params,query` 都属于他

`this.$router` 指的是：一个路由【导航对象】，用它 可以方便使用 JS 代码，实现路由的前进、后退、跳转到新的 URL 地址

- [编程式导航](https://router.vuejs.org/zh/guide/essentials/navigation.html)

- 其中的 name 属性需要在 配置路由中的 route 自定义 name 来相互匹配 代替 path

### 路由嵌套

- 使用`children`属性实现路由嵌套

```html
<div id="app">
  <router-link to="/account">account</router-link>
  <router-view></router-view>
</div>

<template id="tmpl">
  <div>
    <h1>this is component</h1>
    <router-link to="/account/login">login</router-link>
    <router-link to="/account/register">register</router-link>
    <router-view></router-view>
  </div>
</template>
<script>
  var account = {
    template: '#tmpl'
  }
  var login = {
    template: '<h3>login</h3>'
  }
  var register = {
    template: '<h3>register</h3>'
  }
  var router = new VueRouter({
    routes: [
      {
        path: '/account',
        component: account,
        children: [
          // 使用 children 属性，子路由的 path 前面不要带 / ，否则永远以根路径开始请求
          { path: 'login', component: login },
          { path: 'register', component: register }
        ]
      }
    ]
  })
  // 创建 Vue 实例，得到 ViewModel
  var vm = new Vue({
    el: '#app',
    data: {},
    methods: {},
    router
  })
</script>
```

### 命名视图

```html
<div id="app">
  <router-view></router-view>
  <router-view name="side"></router-view> // 组件添加 name 属性
  <router-view name="main"></router-view>
</div>
<script>
  var header = {
    template: '<h3>header - page </h3>'
  }
  var sidebar = {
    template: '<h3>sidebar - page </h3>'
  }
  var mainbox = {
    template: '<h3>mainbox - page </h3>'
  }
  var router = new VueRouter({
    routes: [
      {
        path: '/',
        components: {
          // 使用 components 配置同级路由
          default: header,
          side: sidebar,
          main: mainbox
        }
      }
    ]
  })
  var vm = new Vue({
    el: '#app',
    data: {},
    methods: {},
    router
  })
</script>
```

## Vue 实例 中的其他属性

### 监听数据的变化

#### Vue 实例中的 watch 属性：

- 可以监视 data 中指定数据的变化，然后触发这个 watch 中对应的 function 处理函数

- 可以传递两个参数，第一个参数是指定数据改变后的数据，第二个参数是最近一次更改前的数据
- 使用 `watch` 的优势，可以监听非 DOM 元素的改变，`ex：路由改变, wacth监听 $route.path 即可` 这是事件绑定做不到的

```html
<div id="app">
  <input type="text" v-model="firstname" /> +
  <input type="text" v-model="lastname" /> =
  <input type="text" v-model="fullname" />
</div>
<script>
  // 创建 Vue 实例，得到 ViewModel
  var vm = new Vue({
    el: '#app',
    data: {
      firstname: '',
      lastname: '',
      fullname: ''
    },
    methods: {},
    watch: {
      firstname: function(newVal, oldVal) {
        // key 为所要监听的变量 ex: '$route.path'
        this.fullname = newVal + '-' + this.lastname
      },
      lastname(newVal) {
        this.fullname = this.firstname + newVal
      }
    }
  })
</script>
```

#### Vue 实例中的 computed 属性：

在 `computed` 中，可以定义一些 属性，这些属性，叫做 【计算属性】， 计算属性的，本质，就是 一个方法，只不过，我们在使用 这些计算属性的时候，是把 它们的 名称，直接当作 属性来使用的；并不会把 计算属性，当作方法去调用.

- 计算属性，在引用的时候，一定不要加 () 去调用，直接把它 当作 普通 属性去使用
- 只要 计算属性，这个 `function` 内部，所用到的 任何 `data` 中的数据发送了变化，就会 立即重新计算 这个 计算属性的值
- 计算属性的求值结果，会被缓存起来，方便下次直接使用； 如果 计算属性方法中，所以来的任何数据，都没有发生过变化，则，不会重新对 计算属性求值

```html
<div id="app">
  <input type="text" v-model="firstname" /> +
  <input type="text" v-model="lastname" /> =
  <input type="text" v-model="fullname" />
</div>

<script>
  // 创建 Vue 实例，得到 ViewModel
  var vm = new Vue({
    el: '#app',
    data: {
      firstname: '',
      lastname: ''
    },
    methods: {},
    computed: {
      fullname() {
        return this.firstname + '-' + this.lastname
      }
    }
  })
</script>
```

#### watch 、 computed 和 methods 之间的对比

1. `computed`属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当作属性来使用(操作数据)
2. `methods`方法表示一个具体的操作，主要书写业务逻辑；(方法调用)
3. `watch`一个对象，键是需要观察的表达式，值是对应回调函数。主要用来监听某些特定数据的变化，从而进行某些具体的业务逻辑操作；可以看作是`computed`和`methods`的结合体；(监听虚拟的数据，ex：router)

### 使用 render 渲染页面：

```js
var login = {
  template: '<h1>这是登录组件</h1>'
}

// 创建 Vue 实例，得到 ViewModel
var vm = new Vue({
  el: '#app',
  data: {},
  methods: {},
  render: function(createElements) {
    // createElements 是一个 方法，调用它，能够把 指定的 组件模板，渲染为 html 结构
    return createElements(login)
    // 注意：这里 return 的结果，会 替换页面中 el 指定的那个 容器
  }
})
```
