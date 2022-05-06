# Vue

## Node（后端）中的 MVC 与 前端中的 MVVM 之间的区别

- MVC 是后端的分层开发概念，分为 Model ，View，Controllers
- MVVM 是前端视图层的概念，主要关注于 视图层分离，也就是说：MVVM 把前端的视图层，分为了 三部分 Model, View , VM ViewModel

![01.MVC和MVVM的关系图解](http://localhost:9710/blog/vue/MVC&MVVM.png)

## Vue 操作

声明式编程：模板的结构和最终显示的效果基本一致

- 与原生 js： 字符串拼接 DOM，并追加到页面中的编程方式有一定的区别
- 增强编程体验

### 基本代码结构

- el 指定要控制的区域
- data 是个对象，指定了控制的区域内要用到的数据
- methods 虽然带个 s 后缀，但是是个对象，这里可以自定义了方法

```vue
...
<!-- 1. 导入Vue的包 -->
<script src="./lib/vue-2.4.0.js"></script>
<!-- 将来 new 的Vue实例，会控制这个 元素中的所有内容 -->
<!-- Vue 实例所控制的这个元素区域，就是我们的 V  -->
<div id="app">
  <p>{{ msg }}</p>
</div>
<script>
// 2. 创建一个Vue的实例
// 当我们导入包之后，在浏览器的内存中，就多了一个 Vue 构造函数
// 注意：我们 new 出来的这个 vm 对象，就是我们 MVVM中的 VM调度者
var vm = new Vue({
  el: '#app', // 表示，当前我们 new 的这个 Vue 实例，要控制页面上的哪个区域
  // 这里的 data 就是 MVVM中的 M，专门用来保存 每个页面的数据的
  data: {
    // data 属性中，存放的是 el 中要用到的数据
    msg: '欢迎学习Vue' // 通过 Vue 提供的指令，很方便的就能把数据渲染到页面上，程序员不再手动操作DOM元素了【前端的Vue之类的框架，不提倡我们去手动操作DOM元素了】
  }
})
</script>
```

### Vue 基础

#### 数据绑定：v-cloak 、 v-text 、v-html 、v-pre

##### v-cloak

```vue
<style>
[v-cloak] {
  display: none;
}
</style>
<div v-cloak>{{ msg }}</div>
```

- 会由于`script` 标签位置的不确定性，会出现闪烁问题

- 相较于 `v-text` ,插值表达式只会替换自己的这个占位符，不会把 整个元素的内容清空

- 可以使用 `v-cloak` 解决 闪烁问题

##### v-text

```vue
<div v-text="msg"></div>
```

- 默认 `v-text` 是没有闪烁问题的
- `v-text`会覆盖元素中原本的内容

##### v-html

```vue
<div v-html="msg2">1212112</div>
```

- 能够解析 `html` 标签 并 渲染 前面两个只能把标签当作字符串输出
- 默认 `v-html` 是没有闪烁问题的
- `v-html`会覆盖元素中原本的内容
- 存在安全隐患，
- 本网站内部数据可以使用，来自第三方的数据不可使用

`v-pre`

- 显示原始信息跳过编译过程
- 跳过这个元素和它的子元素的编译过程。
- **一些静态的内容不需要编译加这个指令可以加快渲染**

```vue
<span v-pre>{{ this will not be compiled }}</span> // 显示的是{{ this will not be compiled }}
```

#### 数据响应、v-once

- 数据的响应式（数据的变化导致页面内容的变化）
- 数据绑定：将数据填充到标签中

`v-once`

- 执行一次性的插值【当数据改变时，插值处的内容不会继续更新】

```vue
<!-- 即使data里面定义了msg 后期我们修改了 仍然显示的是第一次data里面存储的数据-->
<span v-once>{{ msg}}</span>
```

#### 双向数据绑定：v-model

- 当数据发生变化的时候，视图也就发生变化
- 当视图发生变化的时候，数据也会跟着同步变化

`v-bind:`只能实现数据的单向绑定，从 M 自动绑定到 V，无法实现数据的双向绑定

`v-model` 指令，可以实现表单元素和 Model 中数据的双向数据绑定（只能在表单元素中使用）

```vue
<input type="text" name="" v-model="msg" id="">
```

> 在 Vue 中，已经实现了数据的双向绑定，每当我们修改了 data 中的数据，Vue 会默认监听到数据的改动，自动把最新的数据，应用到页面上

#### 事件绑定：v-on

Vue 中提供了 `v-on:` 事件绑定机制

```vue
<input type="button" value="按钮" v-on:click="show" id="app">
<script>
    var vm = new Vue({
      el: '#app',
      methods: { // 这个 methods属性中定义了当前Vue实例所有可用的方法
        show: function () {
          alert('Hello')
        }
      }
    })
</script>
```

- `v-on:` 指令可以被简写为 `@`要绑定的属性
  - `<input type="button" value="按钮" @click="show">`
- 使用事件绑定机制，为元素指定事件处理函数时，如加`()`，就可以给函数传参。
- 如果事件直接绑定函数名称，那么默认会传递事件对象作为事件函数的第一个参数
- 如果事件绑定函数调用，那么事件对象必须作为最后一个参数显示传递，并且事件对象的名称必须是`$event`

```html
<div id="app">
  <div>{{num}}</div>
  <div>
    <button v-on:click="handle1">点击1</button>
    <button v-on:click="handle2(123, 456, $event)">点击2</button>
  </div>
</div>
<script type="text/javascript" src="js/vue.js"></script>
<script type="text/javascript">
  var vm = new Vue({
    el: '#app',
    data: {
      num: 0
    },
    methods: {
      handle1: function(event) {
        console.log(event.target.innerHTML)
      },
      handle2: function(p, p1, event) {
        console.log(p, p1)
        console.log(event.target.innerHTML)
        this.num++
      }
    }
  })
</script>
```

#### 事件修饰符

- .stop 阻止冒泡

- .prevent 阻止默认事件

- .capture 添加事件侦听器时使用事件捕获模式

- .self 只当事件在该元素本身（比如不是子元素）触发时触发回调

- .once 事件只触发一次

###### 实例结构

```vue
<style>
.inner {
  height: 150px;
  background-color: darkcyan;
}

.outer {
  padding: 40px;
  background-color: red;
}
</style>
<div id="app">
  
</div>
<script>
// 创建 Vue 实例，得到 ViewModel
var vm = new Vue({
  el: '#app',
  data: {},
  methods: {
    div1Handler(event) {
      console.log('这是触发了 inner div 的点击事件')
      // 阻止冒泡 （原生js）
      // event.stopPropagation();
    },
    btnHandler() {
      console.log('这是触发了 btn 按钮 的点击事件')
    },
    linkClick(event) {
      console.log('触发了连接的点击事件')
      // 阻止默认行为 （原生js）
      // event.preventDefault();
    },
    div2Handler() {
      console.log('这是触发了 outer div 的点击事件')
    }
  }
})
</script>
```

###### 使用 .stop 阻止冒泡 （里 -> 外）

```vue
<div class="inner" @click="div1Handler">
  <input type="button" value="戳他" @click.stop="btnHandler">
</div>
```

###### 使用 .prevent 阻止默认行为

```vue
<a href="http://www.baidu.com" @click.prevent="linkClick">有问题，先去百度</a>
```

###### 使用 .capture 实现捕获触发事件的机制 （外 -> 里）

```vue
<div class="inner" @click.capture="div1Handler">
  <input type="button" value="戳他" @click="btnHandler">
</div>
```

###### 使用 .self 实现只有点击当前元素时候，才会触发事件处理函数

```vue
<div class="inner" @click="div1Handler">
  <input type="button" value="戳他" @click="btnHandler">
</div>
```

- 相较于`.self` 只会阻止自己身上冒泡行为的触发，并不会真正阻止 冒泡的行为

  ```vue
  <div class="outer" @click="div2Handler">
    <div class="inner" @click.self="div1Handler">
      <input type="button" value="戳他" @click="btnHandler">
    </div>
  </div>
  ```

###### 使用 .once 只触发一次事件处理函数

```vue
<a
  href="http://www.baidu.com"
  @click.prevent.once="linkClick">有问题，先去百度</a>
```

- 事件修饰符可以串联
- `.prevent` 与 `.once` 前后关系

#### 键盘修饰符以及自定义键盘修饰符

##### 1.x 中自定义键盘修饰符【了解即可】

```js
Vue.directive('on').keyCodes.f2 = 113
```

##### 2.x 中自定义键盘修饰符

1. 通过`Vue.config.keyCodes.名称 = 按键值`来自定义案件修饰符的别名：

```js
Vue.config.keyCodes.f2 = 113
```

2. 使用自定义的按键修饰符：

```html
<input type="text" v-model="name" @keyup.f2="add" />
```

3. 相关文档：

> js 里面的键盘事件对应的键码：http://www.cnblogs.com/wuhua1/p/6686237.html
> vue 2.x 键盘修饰符文档：https://cn.vuejs.org/v2/guide/events.html#键值修饰符

#### 属性绑定：v-bind

`v-bind`: 是 Vue 中，提供的用于绑定属性的指令

```vue
<input type="button" value="按钮" v-bind:title="msg + '123'">
```

- `v-bind:` 指令可以被简写为 `:`要绑定的属性
  - `<input type="button" value="按钮" :title="msg + '123'">`
- `v-bind` 中，可以写合法的 JS 表达式

v-model 的低层实现原理分析：

```html
<input v-bind:value="msg" v-on:input="msg=$event.target.value" />
```

#### 样式绑定

##### 使用 class 样式(演示案例忽略 css 样式-- active,error,text,base)

- 对象绑定

  ```html
  <div v-bind:class="{ active: isActive,error: isError}">div</div>
  <script>
    ...
    data:{
      isActive:true,
      isError:true,
      obj: {active:true, error:true}
    }
    ...// 可用事件绑定控制 标识符，实现动态更新 class类名
  </script>
  ```

  - class 绑定的值简化操作 : 对象的属性是类名，由于 对象的属性可带引号，也可不带引号； 属性的值 是一个标识符

    ```html
    <div :class="obj">div</div>
    ```

- 数组绑定

  ```html
  <div v-bind:class="[activeClass, errorClass]">div</div>
  <script>
    ...
    data:{
      activeClass: 'active',
      errorClass: 'error',
      isText: true,
      arrClass: ['active', 'error']
    }
    ...
  </script>
  ```

1. 数组中嵌套对象

   ```html
   <div :class="[activeClass,errorClass, {text: isText}]">text</div>
   ```

2. class 绑定的值简化操作

   ```html
   <div class="base" :class="arrClass"></div>
   ```

> 默认的 class 如何处理？默认的 class 会保留

区别

- 绑定对象的时候 对象的属性 即要渲染的类名 对象的属性值对应的是 data 中的数据
- 绑定数组的时候数组里面存的是 data 中的数据

##### 使用内联样式

对象绑定：

```html
<div
  :style="{border: borderStyle, width: widthStyle, height: heightStyle}"
></div>
<script>
  ...
  data: {
    borderStyle: '1px solid #0f0',
    widthStyle: '100px',
    heightStyle: '100px',
    objStyles: {
      border: '1px solid #00f',
      width: '50px',
      height: '50px'
    },
    overrideStyles: {
      border: '5px solid orange',
      backgroundColor: 'blue'
      		// CSS 属性名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用单引号括起来)
    }
  }
  ...
</script>
```

简化：

```html
<div :style="objStyles"></div>
```

数组绑定：可以将多个样式对象应用到同一个元素:

```html
<div :style="[objStyles,overrideStyle]"></div>
```

#### 循环结构：v-for 和 key 属性

1. 遍历数组

```vue
<ul>
  <li v-for="(item, i) in list">索引：{{i}} --- 姓名：{{item.name}} --- 年龄：{{item.age}}</li>
</ul>
```

2. 遍历对象中的属性

   ```html
   <!-- 循环遍历对象身上的属性 -->
   <!-- 注意：在遍历对象身上的键值对的时候， 除了有 val key ,在第三个位置还有 一个 索引  -->
   <p v-for="(val, key, i) in list">
     值是： {{ val }} --- 键是： {{key}} -- 索引： {{i}}
   </p>
   ```

3. 遍历数字

```vue
<!-- 注意：如果使用 v-for 迭代数字的话，前面的 count 值从 1 开始 -->
<p v-for="i in 10">这是第 {{i}} 个P标签</p>
```

> 2.2.0+ 的版本里，**当在组件中使用** v-for 时，key 现在是必须的。

当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “**就地复用**” 策略。如果数据项的顺序被改变，Vue 将**不是移动 DOM 元素来匹配数据项的顺序**， 而是**简单复用此处每个元素**，并且确保它在特定索引下显示已被渲染过的每个元素。

为了给 Vue 一个提示，**以便它能跟踪每个节点的身份，从而重用和重新排序现有元素**，你需要为每项提供一个唯一 key 属性。

```vue
<div id="app">

  <div>
    <label>Id:
      <input type="text" v-model="id">
    </label>
    <label>Name:
      <input type="text" v-model="name">
    </label>
    <input type="button" value="添加" @click="add">
  </div>
  <!-- 注意： v-for 循环的时候，key 属性只能使用 number或string -->
  <!-- 注意： key 在使用的时候，必须使用 v-bind 属性绑定的形式，指定 key 的值 -->
  <!-- 在组件中，使用v-for循环的时候，或者在一些特殊情况中，如果 v-for 有问题，必须 在使用 v-for 的同时，指定 唯一的 字符串/数字 类型 :key 值 -->
  <p v-for="item in list" :key="item.id">
    <input type="checkbox">{{item.id}} --- {{item.name}}
  </p>
</div>
<script>
// 创建 Vue 实例，得到 ViewModel
var vm = new Vue({
  el: '#app',
  data: {
    id: '',
    name: '',
    list: [
      { id: 1, name: '李斯' },
      { id: 2, name: '嬴政' },
      { id: 3, name: '赵高' },
      { id: 4, name: '韩非' },
      { id: 5, name: '荀子' }
    ]
  },
  methods: {
    add() {
      // 添加方法
      this.list.unshift({ id: this.id, name: this.name })
    }
  }
})
</script>
```

#### 分支结构： v-if 和 v-show

##### v-if / v-else / v-else-if

```html
<div id="app">
  <div v-if="score>= 90">very good</div>
  <div v-else-if="score < 90 && score>= 80">better</div>
  <div v-else-if="score < 80 && score>= 60">good</div>
  <div v-else>bad</div>
</div>
<script>
  ...
  data:{
  	score: 87
  }
  ...
</script>
```

- 每次都会重新删除或创建元素
- 有较高的切换性能消耗

##### v-show

- 每次不会重新进行 DOM 的删除和创建操作，只是切换了元素的 display:none 样式
- 有较高的初始渲染消耗

```vue
<input type="submit" value="切换" @click="flag=!flag">
<h3 v-if="flag">v-if--------Lorem ipsum dolor, sit </h3>
<h3 v-show="flag">v-show--------Lorem ipsum dolor, sit</h3>
```

区别：

- v-if 控制元素是否渲染到页面
- v-show 控制元素是否显示（已经渲染到了页面）

如果元素涉及到频繁的切换，最好不要使用 v-if, 而是推荐使用 v-show

如果元素可能永远也不会被显示出来被用户看到，则推荐使用 v-if

### Vue 高级

#### 表单基本操作

- 获取单选框中的值

  1. 两个单选框需要同时通过 v-model 双向绑定 一个值
  2. 每一个单选框必须要有 value 属性 且 value 值不能一样
  3. 当某一个单选框选中的时候 v-model 变成 当前的 value 值

  ```html
  <input type="radio" id="male" value="1" v-model="gender" />
  <label for="male">男</label>
  <input type="radio" id="female" value="2" v-model="gender" />
  <label for="female">女</label>
  <script>
    ...
       data: {
         // 默认会让当前的 value 值为 2 的单选框选中
         // gender 的值就是选中的值，我们只需要实时监控他的值就可以了
         gender: 2,
       }
    ...
  </script>
  ```

- 获取复选框中的值

  1. 通过 v-model
  2. 和获取单选框中的值一样
  3. 复选框 `checkbox` 这种的组合时 data 中的 hobby 要定义成数组 否则无法实现多选
  4. 单独使用 `checkbox` 实现点击全选/全不选 功能时 可用 `v-model='false/true'`,进行双向控制

  ```html
  <div>
    <span>爱好：</span>
    <input type="checkbox" id="ball" value="1" v-model="hobby" />
    <label for="ball">篮球</label>
    <input type="checkbox" id="sing" value="2" v-model="hobby" />
    <label for="sing">唱歌</label>
    <input type="checkbox" id="code" value="3" v-model="hobby" />
    <label for="code">写代码</label>
  </div>
  <script>
    ...
       data: {
          // 默认会让当前的 value 值为 2 和 3 的复选框选中
          hobby: ['2', '3'],
       }
    ...
  </script>
  ```

- 获取下拉框和文本框中的值

  1. 需要给 select 通过 v-model 双向绑定 一个值
  2. 每一个 option 必须要有 value 属性 且 value 值不能一样
  3. 当某一个 option 选中的时候 v-model 改变成 当前的 value 值
  4. 文本框中的值与单选框一样，v-model 绑定
     - 不能再双标签中间处理数据，只能用 v-model 进行数据绑定

  ```html
  <div>
    <span>职业：</span>
    <!--occupation 的值就是选中的值，我们只需要实时监控他的值就可以了-->
    <!-- multiple  多选 -->
    <select v-model="occupation" multiple>
      <option value="0">请选择职业...</option>
      <option value="1">教师</option>
      <option value="2">软件工程师</option>
      <option value="3">律师</option>
    </select>
    <!-- textarea 是 一个双标签   不需要绑定value 属性的  -->
    <textarea v-model="desc"></textarea>
  </div>
  <script>
    ...
       data: {
         // 默认会让当前的 value 值为 2 和 3 的下拉框选中
         occupation: ['2', '3'],
         // 加 multiple 属性后 occupation 的值必须为数组，否则报错
         // 未加多选功能的时候，occupation 的值可以直接为 数值
         desc: 'nihao'
       }
    ...
  </script>
  ```

#### 表单域修饰符

- .number 转换为数值 `<input type="text" v-model.number="age">`

  - 注意点：
  - 当开始输入非数字的字符串时，因为 Vue 无法将字符串转换成数值
  - 所以属性值将实时更新成相同的字符串。即使后面输入数字，也将被视作字符串。

- .trim 自动过滤用户输入的首尾空白字符 `<input type="text" v-model.trim="info">`

  - 只能去掉首尾的 不能去除中间的空格

- .lazy 将 input 事件切换成 change 事件 `<input type="text" v-model.lazy="msg">`
  - input 事件 每次输入内容都会被触发
  - change 事件 在失去焦点 或者 按下回车键时才触发
  - .lazy 修饰符延迟了同步更新属性值的时机。即将原本绑定在 input 事件的同步逻辑转变为绑定在 change 事件上 即：在失去焦点 或者 按下回车键时才更新

#### 自定义指令

##### 全局自定义指令：

使用 `Vue.directive('自定义属性名', { })` 定义全局的自定义指令

- 参数 1 ： 指令的名称，注意: 在定义的时候，指令的名称前面，不需要加 v- 前缀

- 在调用的时候，必须 在指令名称前 加上 v- 前缀来进行调用

- 参数 2： 是一个对象，这个对象身上，有一些指令相关的函数，这些函数可以在特定的阶段，执行相关的操作

  > vue 2.x 自定义指令文档：https://cn.vuejs.org/v2/guide/custom-directive.html

  ```js
  ···
  <input type="text" class="form-control" v-model="keywords" id="search" v-focus v-color="'blue'">// 'blue'不加单引号会当成变量在，data 中找。
  ···			   // 可以传 data 中的数据 v-color="msg"

  Vue.directive('focus', {
    bind: function (el) { // 每当指令绑定到元素上的时候，会立即执行这个 bind 函数，只执行一次
      // 注意： 在每个 函数中，第一个参数，永远是 el ，表示 被绑定了指令的那个元素，这个 el 参数，是一个原生的JS对象
      // 在元素 刚绑定了指令的时候，还没有 插入到 DOM中去，这时候，调用 focus 方法没有作用
      // 因为，一个元素，只有插入DOM之后，才能获取焦点
      // el.focus()
    },// inserted 表示元素 插入到DOM中的时候，会执行 inserted 函数【触发1次】
    inserted: function (el) {
      el.focus()
      // 和JS行为有关的操作，最好在 inserted 中去执行，防止 JS行为不生效
    },
    update: function (el) {  // 当VNode更新的时候，会执行 updated， 可能会触发多次
    }
  })

  Vue.directive('color', {
  // 样式，只要通过指令绑定给了元素，不管这个元素有没有被插入到页面中去，这个元素肯定有了一个内联的样式
  // 将来元素肯定会显示到页面中，这时候，浏览器的渲染引擎必然会解析样式，应用给这个元素
    bind: function (el, binding) {
      // 和样式相关的操作，一般都可以在 bind 执行
      // console.log(binding.value)      指令的绑定值  				clg: blue
      // console.log(binding.expression) 字符串形式的指令表达式 clg: 'blue'
      el.style.color = binding.value
      // el.style.color = binding.value.color
      // data中的数据
      // msg:{
    //    color: 'orange'
      // }
  }
  })
  ```

  总结 ： 与样式有关的操作，设置到 `bind` 中，与行为有关的操作，设置到 `inserted` 中。

  ​ `bind` 的执行时机，早与 `inserted` 。

##### 局部自定义指令：

在 Vue 实例中，增加一个属性值：`directives：{}`

```html
<input type="text" class="form-control" v-model="keywords" v-fontweight="200" />
··· directives: { 'fontweight': { bind(el, binding) { el.style.fontWeight =
binding.value } } } ···
```

##### 自定义属性中函数简写：

若只使用 `bind` 和 `update` 触发相同行为，而不关心其它的钩子。 可简写：

```html
<input type="text" class="form-control" v-model="keywords" v-fontweight="200" />
··· directives: { // 注意：这个 function 等同于 把 代码写到了 bind 和 update
中去 'fontsize': function (el, binding) { el.style.fontSize =
parseInt(binding.value) + 'px' } } ···
```

#### 计算属性

表达式的计算逻辑可能会比较复杂，使用计算属性可以使模板内容更加简洁

- 模板中放入太多的逻辑会让模板过重且难以维护 使用计算属性可以让模板更加的简洁

  - 把模板中复杂的计算逻辑抽取出来，使模板变得更加简单
  - **计算属性是基于它们的响应式依赖（data 中的数据）进行缓存的**

  - computed 比较适合对多个变量或者对象进行处理后返回一个结果值，也就是数多个变量中的某一个值发生了变化则我们监控的这个值也就会发生变化

  ```js
    // html : <p>{{total}}</p>
    const app = new Vue({
        el: '#app',
        data() {
            return {
                courses: ['zf', 'zf']
            }
        },
        computed: {
            total() {
                return this.courses.length + 'eeeeeee'
            }
        },
    })
  ```

#### 侦听器 watch

- 使用 watch 来响应数据的变化
- 一般用于异步或者开销较大的操作
- watch 中的属性 一定是 data 中 已经存在的数据
- **当需要监听一个对象的改变时，普通的 watch 方法无法监听到对象内部属性的改变，只有 data 中的数据才能够监听到变化，此时就需要 deep 属性对对象进行深度监听**

![Snipaste_2019-12-12_08-48-43](http://localhost:9710/blog/vue/Snipaste_2019-12-12_08-48-43.png)

```js
    // html : <p>{{totalcount}}</p>
    const app = new Vue({
        el: '#app',
        data() {
            return {
                courses: ['zf', 'zf'],
                totalcount: 0
            }
        },
        // watch: {
        //     courses(newValue, oldValue) {
        //         totalcount = newValue.length
        //     }
        // },
        // 默认 不带选项的 watch 初始化时不执行
        watch: {
            courses: {
                immediate: true, // 立即执行一次
                // deep: true,
                handler(newValue, oldValue) {
                    this.totalcount = newValue.length
                }
            }
        },
    })
```

_计算属性 vs 监听器_

-   监听器更通用，理论上计算属性能实现的侦听器也能实现
-   处理数据的场景不同，监听器适合一个数据影响多个数据（一个值变化以后 做很多事情），计算属性适合一个数据受多个数据影响
-   计算属性有缓存性，计算所得的值如果没有变化不会重复执行
-   监听器适合执行异步操作或较大开销操作的情况

#### 过滤器

- 概念：Vue.js 允许你自定义过滤器，**可被用作一些常见的文本格式化**。
- 过滤器可以用在两个地方：**mustache 插值和 v-bind 表达式**。
- 过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符指示。
  - 调用：
    - `{{ name | 过滤器名称 }}`
    - `v-bind:id=" name | 过滤器名称 "`
- 支持级联操作（同时使用多个）: 把前面 过滤器 的结果在作为下一个过滤器的输入值，在进行处理，再产出新的结果。
- 过滤器不改变真正的`data`，而只是改变渲染的结果，并返回过滤后的版本

##### 全局过滤器

`Vue.filter('过滤器名称', function () { })`

其中的 function ，第一个参数，已经被规定死了，永远都是 过滤器 管道符前面 传递过来的数据

##### 私有过滤器

在 Vue 实例中，增加一个属性值：`filters：{}`

> 注意：当有局部和全局两个名称相同的过滤器时候，会以就近原则进行调用，即：局部过滤器优先于全局过滤器被调用！

##### 基本使用：

```html
<div id="app">
  <input type="text" v-model="msg" />
  <div>{{msg | upper}}</div>
  <div>{{msg | upper | lower}}</div>
  <div :abc="msg | upper">测试数据</div>
</div>
<script type="text/javascript">
  /*
    过滤器
    1、可以用与插值表达式和属性绑定
    2、支持级联操作
  */
  // Vue.filter('upper', function(val) {
  //   return val.charAt(0).toUpperCase() + val.slice(1);
  // });
  Vue.filter('lower', function(val) {
    return val.charAt(0).toLowerCase() + val.slice(1)
  })
  var vm = new Vue({
    el: '#app',
    data: {
      msg: ''
    },
    filters: {
      upper: function(val) {
        return val.charAt(0).toUpperCase() + val.slice(1)
      }
    }
  })
</script>
```

##### 过滤器中传递参数

```html
<div>{{date | format('yyyy-MM-dd')}}</div>
<script>
  Vue.filter('format', function(val, arg) {
    // 过滤器接受参数从第二个参数开始接收
    if (arg == 'yyyy-MM-dd') {
      return (
        val.getFullYear() + '-' + (val.getMonth() + 1) + '-' + val.getDate()
      )
    }
    return val
  })
</script>
```

##### 过滤器示例

```html
<script>
  // 定义一个全局过滤器
  Vue.filter('dataFormat', function(input, pattern = '') {
    var dt = new Date(input)
    // 获取年月日
    var y = dt.getFullYear()
    var m = (dt.getMonth() + 1).toString().padStart(2, '0')
    var d = dt
      .getDate()
      .toString()
      .padStart(2, '0')

    // 如果 传递进来的字符串类型，转为小写之后，等于 yyyy-mm-dd，那么就返回 年-月-日
    // 否则，就返回  年-月-日 时：分：秒
    if (pattern.toLowerCase() === 'yyyy-mm-dd') {
      return `${y}-${m}-${d}`
    } else {
      // 获取时分秒
      var hh = dt
        .getHours()
        .toString()
        .padStart(2, '0')
      var mm = dt
        .getMinutes()
        .toString()
        .padStart(2, '0')
      var ss = dt
        .getSeconds()
        .toString()
        .padStart(2, '0')

      return `${y}-${m}-${d} ${hh}:${mm}:${ss}`
    }
  })
</script>

<!-- 在element ui 中使用全局过滤器，需要借助作用域插槽 -->
<el-table-column prop="add_time" label="创建时间" width="140px">
  <template v-slot="scope">
    {{ scope.row.add_time | dataFormat }}
  </template>
</el-table-column>
```

#### [vue 实例的生命周期](https://cn.vuejs.org/v2/guide/instance.html#实例生命周期)

什么是生命周期：从 Vue 实例创建、运行、到销毁期间，总是伴随着各种各样的事件，这些事件，统称为生命周期！

[生命周期钩子](https://cn.vuejs.org/v2/api/#选项-生命周期钩子)：就是生命周期事件的别名而已；

生命周期钩子 = 生命周期函数 = 生命周期事件

![lifecycle](http://localhost:9710/blog/vue/lifecycle.jpg)

主要的生命周期函数分类：

- 创建期间的生命周期函数：`挂载`（初始化相关属性）

  - beforeCreate：实例刚在内存中被创建出来，此时，还没有初始化好 data 和 methods 属性
    - 在实例初始化之后，数据观测和事件配置之前被调用。此时 data 和 methods 以及页面的 DOM 结构都没有初始化 什么都做不了
  - created：实例已经在内存中创建 OK，此时 data 和 methods 已经创建 OK，此时还没有开始 编译模板
    - 在实例创建完成后被立即调用
  - beforeMount：此时已经完成了模板的编译，但是还没有挂载到页面中
    - 在 beforeMount 执行的时候，页面中的元素，还没有被真正替换过来，只是之前写的一些模板字符串
  - 在挂载开始之前被调用
  - mounted：此时，已经将编译好的模板，挂载到了页面指定的容器中显示
    - 注意： mounted 是 实例创建期间的最后一个生命周期函数，当执行完 mounted 就表示，实例已经被完全创建好了，此时，如果没有其它操作的话，这个实例，就静静的 躺在我们的内存中，一动不动
    - el 被新创建的 vm.\$el 替换，并挂载到实例上去之后调用该钩子。

- 运行期间的生命周期函数：`更新`（元素或组件的变更操作）
  - beforeUpdate：状态更新之前执行此函数， 此时 data 中的状态值是最新的，但是界面上显示的 数据还是旧的，因为此时还没有开始重新渲染 DOM 节点
    - beforeUpdate 数据更新时调用，发生在虚拟 DOM 打补丁之前。
  - updated：实例更新完毕之后调用此函数，此时 data 中的状态值 和 界面上显示的数据，都已经完成了更新，界面已经被重新渲染好了！
    - 由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。
- 销毁期间的生命周期函数：`销毁`（销毁相关属性，目的释放资源）
  - beforeDestroy：实例销毁之前调用。在这一步，实例仍然完全可用。
  - destroyed：Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

常用：

mounted： 初始化已经完成，页面中模板内容已经存在，可以向里面填充数据了，

- 应用场景： 调用后台接口，获取数据，然后把数据填充到模板里（保证页面中已经有模板内容了）

#### 数组变异方法

- 在 Vue 中，直接修改对象属性的值无法触发响应式。当你直接修改了对象属性的值，你会发现，只有数据改了，但是页面内容并没有改变
- Vue 将被侦听的数组的变异方法进行了包裹，所以它们也将会触发视图更新
- 变异数组方法即保持数组方法原有功能不变的前提下对其进行功能拓展

| `push()`    | 往数组最后面添加一个元素，成功返回当前数组的长度                                                                      |
| ----------- | --------------------------------------------------------------------------------------------------------------------- |
| `pop()`     | 删除数组的最后一个元素，成功返回删除元素的值                                                                          |
| `shift()`   | 删除数组的第一个元素，成功返回删除元素的值                                                                            |
| `unshift()` | 往数组最前面添加一个元素，成功返回当前数组的长度                                                                      |
| `splice()`  | 有三个参数，第一个是想要删除的元素的下标（必选），第二个是想要删除的个数（必选），第三个是删除 后想要在原位置替换的值 |
| `sort()`    | sort() 使数组按照字符编码默认从小到大排序,成功返回排序后的数组                                                        |
| `reverse()` | reverse() 将数组倒序，成功返回倒序后的数组                                                                            |

#### 替换数组

- 不会改变原始数组，但总是返回一个新数组

| filter | filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。 |
| ------ | ------------------------------------------------------------------------------------- |
| concat | concat() 方法用于连接两个或多个数组。该方法不会改变现有的数组                         |
| slice  | slice() 方法可从已有的数组中返回选定的元素。该方法并不会修改数组，而是返回一个子数组  |

#### 动态数组响应式数据

- Vue.set(vm.items, indexOfItem, newValue)
- vm.\$set(vm.items, indexOfItem, newValue)
  - 参数一表示要处理的数组名称
  - 参数二表示要处理的数组的索引
  - 参数三表示要处理的数组的值

```html
<div id="app">
  <div v-for="(item,i ) in list" :key="i">{{item}}</div>
  <div>{{info.name}}</div>
  <div>{{info.age}}</div>
  <div>{{info.gender}}</div>
</div>
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      list: ['apple', 'orange', 'banana'],
      info: {
        name: 'lisi',
        age: 12
      }
    }
  })
  vm.list[1] = 'lemon' // 用索引的方式修改数据不是响应式的
  Vue.set(vm.list, '2', 'tomato')
  vm.$set(vm.list, '1', 'pear')
  vm.info.gender = 'male' // 不是响应式的
  vm.$set(vm.info, 'gender', 'male')
</script>
```

### [vue-resource 实现 get, post, jsonp 请求](https://github.com/pagekit/vue-resource)

> 除了 vue-resource 之外，还可以使用 `axios` 的第三方包实现实现数据的请求

vue-resource 向 vue 挂载了一个属性 `this.$http`

#### get 请求

`Vue.http.get('/Url', [config]).then(successCallback, errorCallback)`

```html
<input type="button" value="get请求" @click="getInfo" />
<script>
   ...
    methods: {
      getInfo() {
        this.$http.get('http://www.liulongbin.top:3005/api/getprodlist').then(function (result) {
          // 通过 result.body 拿到服务器返回的成功的数据
          console.log(result.body)
        })
      }
    }
  ...
</script>
```

#### post 请求

`Vue.http.post('/someUrl', [body], [config]).then(successCallback, errorCallback)`

```html
<input type="button" value="POST" @click="postInfo" />
<script>
   ...
  postInfo() { // 发起 post 请求   application/x-wwww-form-urlencoded
    //  手动发起的 Post 请求，默认没有表单格式，所以，有的服务器处理不了
    //  通过 post 方法的第三个参数， { emulateJSON: true } 设置 提交的内容类型 为 普通表单数据格式
    this.$http.post('http://www.liulongbin.top:3005/api/post', {}, { emulateJSON: true }).then(result => {
      console.log(result.body)
    })
  }
  ...
</script>
```

#### jsonp 请求

`Vue.http.jsonp('/Url', [config]).then(successCallback, errorCallback)`

```html
<input type="button" value="jsonp请求" @click="jsonpInfo" />
<script>
   ...
    jsonpInfo() {
      this.$http.jsonp('http://www.liulongbin.top:3005/api/jsonp').then(result => {
        console.log(result.body)
      })
    }
  ...
</script>
```

#### jsonp 原理

- 由于浏览器的安全性限制，不允许 AJAX 访问 协议不同、域名不同、端口号不同的 数据接口，浏览器认为这种访问不安全；
- 可以通过动态创建 script 标签的形式，把 script 标签的 src 属性，指向数据接口的地址，因为 script 标签不存在跨域限制，这种数据获取方式，称作 JSONP（注意：根据 JSONP 的实现原理，知晓，JSONP 只支持 Get 请求）；
- 具体实现过程：

* 先在客户端定义一个回调方法，预定义对数据的操作；
* 再把这个回调方法的名称，通过 URL 传参的形式，提交到服务器的数据接口；
* 服务器数据接口组织好要发送给客户端的数据，再拿着客户端传递过来的回调方法名称，拼接出一个调用这个方法的字符串，发送给客户端去解析执行；
* 客户端拿到服务器返回的字符串之后，当作 Script 脚本去解析执行，这样就能够拿到 JSONP 的数据了

```js
/---------- node 服务端
server.on('request', (req, res) => {
	const { pathname: url , query} =ulModule.parse(req.url, true)
	// console.log(pathname, query)
	if (url === '/getscript') {
		var data = {
			name: '123',
			agg: 18,
			gender: 'fomale'
		}
		var scriptStr = `${query.callback}(${JSON.stringify(data)})`
		res.end(scriptStr)
	} else {
		res.end('404')
	}
})

/---------- html 客户端

<script>
  function showInfo(data) {
    console.log(data)
  }
</script>
<script src="http://127.0.0.1:3000/getscript?callback=showInfo"></script>
```

#### vue-resource 的全局配置

配置根域名：

`Vue.http.options.root = 'http://vue.studyit.io/'`

- 如果通过全局配置了，请求的数据接口 根域名，则 ，在每次单独发起 http 请求的时候，请求的 url 路径，应该以相对路径开头，前面不能带 / ，否则 不会启用根路径做拼接

启用`emulateJSON` 配置

`Vue.http.options.emulateJSON = true`

- 作用：将 `content-type`设置 `application/x-www-form-urlencoded` 模拟表单发送请求

#### 应用实例

```js
// 核心代码：
Vue.http.options.root = 'http://www.liulongbin.top:3005/'
Vue.http.options.emulateJSON = true
var vm = new Vue({
  el: '#app',
  data: {
    name: '',
    list: [
      { id: 1, name: 'BMW', ctime: new Date() },
      { id: 2, name: 'AD', ctime: new Date() }
    ]
  },
  methods: {
    getAllList() {
      this.$http.get('api/getprodlist').then(result => {
        var data = result.body
        if (data.status !== 0) {
          return console.log('404')
        }
        this.list = data.message
      })
    },
    add() {
      this.$http.post('api/addproduct', { name: this.name }).then(result => {
        if (result.body.status !== 0) {
          return alert('404')
        }
        this.getAllList()
        this.name = ''
      })
    },
    del(id) {
      this.$http.get('api/delproduct/' + id).then(result => {
        if (result.body.status !== 0) {
          return alert('404')
        }
        this.getAllList()
      })
    }
  },
  created() {
    this.getAllList()
  }
})
```

### [Vue 中的动画](https://cn.vuejs.org/v2/guide/transitions.html)

#### 使用过渡类名

使用 transition 元素，把 需要被动画控制的元素，包裹起来：

```html
<transition name="zf">
  <h4 v-show="flag">Lorem, ipsum dolor sit amet consectetur.</h4>
</transition>
```

自定义两组样式，来控制 transition 内部的元素实现动画：

```css
/* v-enter 【这是一个时间点】 是进入之前，元素的起始状态，此时还没有开始进入 */
/* v-leave-to 【这是一个时间点】 是动画离开之后，离开的终止状态，此时，元素 动画已经结束了 */
.zf-enter,
.zf-leave-to {
  opacity: 0;
  transform: translateX(150px);
}

/* v-enter-active 【入场动画的时间段】 */
/* v-leave-active 【离场动画的时间段】 */
.zf-enter-active,
.zf-leave-active {
  transition: all 0.8s ease;
}
```

自定义 `v-` 前缀：

在 `transition` 元素中加 `name` 属性后，`css` 中的类名 可将默认的 `v-` 前缀 换成 `name` 的属性值加 `-`，达到区分不同动画。

#### [使用第三方 CSS 动画库](https://cn.vuejs.org/v2/guide/transitions.html#自定义过渡类名)

1. 导入动画类库：

```
<link rel="stylesheet" type="text/css" href="./lib/animate.css">
```

2. 定义 transition 及属性：

```html
<div id="app">
  <input type="button" value="toggle" @click="flag= !flag" />
  <transition
    enter-active-class="bounceIn"
    leave-active-class="bounceOut"
    :duration="{ enter: 200, leave: 400 }"
  >
    <h4 v-if="flag" class="animated">
      Lorem ipsum dolor sit amet consectetur, adipisicing elit.
    </h4>
  </transition>
</div>
```

- 使用 `:duration="毫秒值"` 来统一设置 入场 和 离场 时候的动画时长

- 使用 `:duration="{ enter: 200, leave: 400 }"` 来分别设置 入场的时长 和 离场的时长

#### 使用动画钩子函数：（半场动画）

```html
<div id="app">
  <input type="button" value="click" @click="flag = !flag" />
  <transition
    @before-enter="beforeEnter"
    @enter="enter"
    @after-enter="afterEnter"
  >
    <div class="ball" v-if="flag"></div>
  </transition>
</div>

<script>
  // 创建 Vue 实例，得到 ViewModel
  var vm = new Vue({
    el: '#app',
    data: {
      flag: false
    },
    methods: {
      // 注意： 动画钩子函数的第一个参数：el，表示 要执行动画的那个DOM元素，是个原生的 JS DOM对象
      // el 是通过 document.getElementById('') 方式获取到的原生JS DOM对象
      beforeEnter(el) {
        // beforeEnter 表示动画入场之前，此时，动画尚未开始，可以 在 beforeEnter 中，设置元素开始动画之前的起始样式
        // 设置小球开始动画之前的，起始位置
        el.style.transform = 'translate(0, 0)'
      },
      enter(el, done) {
        // 这句话，没有实际的作用，但是，如果不写，出不来动画效果；
        // 可以认为 el.offsetWidth/offsetHeight/offsetLeft/offsetTop 会强制动画刷新
        el.offsetWidth
        // enter 表示动画 开始之后的样式，这里，可以设置小球完成动画之后的，结束状态
        el.style.transform = 'translate(150px, 450px)'
        el.style.transition = 'all 1s ease'
        // 这里的 done， 起始就是 afterEnter 这个函数，也就是说：done 是 afterEnter 函数的引用
        done()
      },
      afterEnter(el) {
        // 动画完成之后，会调用 afterEnter
        // 第一个功能，是控制小球的显示与隐藏
        // 第二个功能： 直接跳过后半场动画，让 flag 标识符 直接变为 false
        // 当第二次再点击 按钮的时候， flag  false  ->    true
        this.flag = !this.flag
        // Vue 把一个完整的动画，使用钩子函数，拆分为了两部分：
        // 我们使用 flag 标识符，来表示动画的切换；
        // 刚以开始，flag = false  ->   true   ->   false
      }
    }
  })
</script>
```

#### [v-for 的列表过渡](https://cn.vuejs.org/v2/guide/transitions.html#列表的进入和离开过渡)

1.  定义过渡样式：

```css
.v-enter,
.v-leave-to {
  opacity: 0;
  transform: translateY(80px);
}

.v-enter-active,
.v-leave-active {
  transition: all 0.6s ease;
}
```

2. 定义 DOM 结构，其中，需要使用 transition-group 组件把 v-for 循环的列表包裹起来：

   ```html
   <div id="app">
     <div>
       <label for="">
         Id:
         <input type="text" v-model="id" />
       </label>
       <label for="">
         Name:
         <input type="text" v-model="name" />
       </label>
       <input type="button" value="add" @click="add" />
     </div>
     <transition-group appear tag="ul">
       <li v-for="(item, i) in list" :key="item.id" @click="del(i)">
         {{item.id}} --- {{item.name}}
       </li>
     </transition-group>
   </div>
   ```

   - 在实现列表过渡的时候，如果需要过渡的元素，是通过 `v-for` 循环渲染出来的，不能使用 `transition` 包裹，需要使用 `transitionGroup`
   - 如果要为 `v-for` 循环创建的元素设置动画，必须为每一个 元素 设置 `:key` 属性
   - 给 `transition-group` 添加 `appear` 属性，实现页面刚展示出来时候，入场时候的效果
   - 通过 为 `transition-group` 元素，设置 `tag` 属性，指定 `transition-group` 渲染为指定的元素，如果不指定 `tag` 属性，默认，渲染为 `span` 标签

3. 定义 VM 中的结构：

   ```js
   var vm = new Vue({
     el: '#app',
     data: {
       id: '',
       name: '',
       list: [
         { id: 1, name: 'qwe' },
         { id: 2, name: 'asd' }
       ]
     },
     methods: {
       add() {
         this.list.push({ id: this.id, name: this.name })
         this.id = this.name = ''
       },
       del(i) {
         this.list.splice(i, 1)
       }
     }
   })
   ```

4. 列表的排序过渡:

   `<transition-group>` 组件还有一个特殊之处。不仅可以进入和离开动画，**还可以改变定位**。要使用这个新功能只需了解新增的 `v-move` 特性，**它会在元素的改变定位的过程中应用**。

   - `v-move` 和 `v-leave-active` 结合使用，能够让列表的过渡更加平缓柔和：

   ```css
   .v-move {
     transition: all 0.6s ease;
   }
   .v-leave-active {
     position: absolute;
   }
   ```

## Vue 中的组件

组件：

- 组件 (Component) 是 Vue.js 最强大的功能之一
- 组件可以扩展 HTML 元素，封装可重用的代码

### 组件注册

#### 全局注册

- `Vue.component('组件名称', { })` 第 1 个参数是标签名称，第 2 个参数是一个选项对象
- **全局组件**注册后，任何**vue 实例**都可以用

##### 组件基础用

```html
<div id="example">
  <!-- 2、 组件使用 组件名称 是以HTML标签的形式使用  -->
  <my-component></my-component>
</div>
<script>
  //   注册组件
  // 1、 my-component 就是组件中自定义的标签名
  Vue.component('my-component', {
    template: '<div>A custom component!</div>'
  })

  // 创建根实例
  new Vue({
    el: '#example'
  })
</script>
```

##### 组件注意事项

- 组件参数的 data 值必须是函数同时这个函数要求返回一个对象
  - 使用函数的作用：形成闭包的环境，使每个组件都拥有独立的数据
- 组件模板必须是单个根元素
- 组件模板的内容可以是模板字符串

```HTML
  <div id="app">
     <!--
		4、  组件可以重复使用多次
	    因为data中返回的是一个对象所以每个组件中的数据是私有的
		  即每个实例可以维护一份被返回对象的独立的拷贝
	-->
    <button-counter></button-counter>
    <button-counter></button-counter>
    <button-counter></button-counter>
      <!-- 8、必须使用短横线的方式使用组件 -->
     <hello-world></hello-world>
  </div>

<script type="text/javascript">
	//5  如果使用驼峰式命名组件，那么在使用组件的时候，只能在字符串模板中用驼峰的方式使用组件，
  // 7、但是在普通的标签模板中，必须使用短横线的方式使用组件
  Vue.component('HelloWorld', {
    data: function(){
      return {
        msg: 'HelloWorld'
      }
    },
    template: '<div>{{msg}}</div>'
  });
  Vue.component('button-counter', {
    // 1、组件参数的data值必须是函数
    // 同时这个函数要求返回一个对象
    data: function(){
      return {
        count: 0
      }
    },
    //  2、组件模板必须是单个根元素
    //  3、组件模板的内容可以是模板字符串
    template: `
      <div>
        <button @click="handle">点击了{{count}}次</button>
        <button>测试123</button>
    #  6 在字符串模板中可以使用驼峰的方式使用组件
     <HelloWorld></HelloWorld>
      </div>
    `,
    methods: {
      handle: function(){
        this.count += 2;
      }
    }
  })
  var vm = new Vue({
    el: '#app',
    data: {

    }
  });
</script>
```

#### 局部注册

- 只能在当前注册它的 vue 实例中使用

```html
<div id="app">
  <my-component></my-component>
</div>
<script>
  // 定义组件的模板
  var Child = {
    template: '<div>A custom component!</div>'
  }
  new Vue({
    //局部注册组件
    components: {
      // <my-component> 将只在父模板可用  一定要在实例上注册了才能在html文件中使用
      'my-component': Child
    }
  })
</script>
```

### Vue 组件之间传值

#### 父组件向子组件传值

- 父组件发送的形式是以属性的形式绑定值到子组件身上。
- 然后子组件用属性 props 接收
  - 与 组建中的 `data`的 区别：
    - 子组件中的 `data` 数据，并不是通过 父组件传递过来的，而是子组件自身私有的，比如： 子组件通过 Ajax ，请求回来的数据，都可以放到 `data` 身上；且 `data` 上的数据，都是可读可写的
    - 组件中的 所有 `props` 中的数据，都是通过 父组件传递给子组件的；且`props` 中的数据，都是只读的，无法重新赋值
  - props 传递数据原则：单向数据流，只允许父组件向自组建传递数据，不允许子组件直接操作，props 中的数据
- 在 props 中使用驼峰形式，模板中需要使用短横线的形式字符串形式的模板中没有这个限制

```html
<div id="app">
  <div>{{pmsg}}</div>
  <!--1、menu-item  在 APP中嵌套着 故 menu-item   为  子组件      -->
  <!-- 给子组件传入一个静态的值 -->
  <menu-item title="来自父组件的值"></menu-item>
  <!-- 2、 需要动态的数据的时候 需要属性绑定的形式设置 此时 ptitle  来自父组件data 中的数据 . 
		  传的值可以是数字、对象、数组等等
	-->
  <menu-item :title="ptitle" content="hello"></menu-item>
</div>

<script type="text/javascript">
  Vue.component('menu-item', {
    // 3、 子组件用属性props接收父组件传递过来的数据
    props: ['title', 'content'],
    data: function() {
      return {
        msg: '子组件本身的数据'
      }
    },
    template: '<div>{{msg + "----" + title + "-----" + content}}</div>'
  })
  var vm = new Vue({
    el: '#app',
    data: {
      pmsg: '父组件中内容',
      ptitle: '动态绑定属性'
    }
  })
</script>
```

#### 子组件向父组件传值

- 子组件用`$emit()`触发事件
- `$emit()` 第一个参数为 自定义的事件名称 第二个参数为需要传递的数据
- 父组件用 v-on 监听子组件的事件

```html
<div id="app">
  <div :style='{fontSize: fontSize + "px"}'>{{pmsg}}</div>
  <!-- 2 父组件用v-on 监听子组件的事件
	这里 enlarge-text  是从 $emit 中的第一个参数对应   handle 为对应的事件处理函数	
	-->
  <menu-item :parr="parr" @enlarge-text="handle($event)"></menu-item>
</div>
<script type="text/javascript" src="js/vue.js"></script>
<script type="text/javascript">
  /*
    子组件向父组件传值-携带参数
  */
  Vue.component('menu-item', {
    props: ['parr'],
    template: `
      <div>
        <ul>
          <li :key='index' v-for='(item,index) in parr'>{{item}}</li>
        </ul>
    ###  1、子组件用$emit()触发事件
    ### 第一个参数为 自定义的事件名称   第二个参数为需要传递的数据  
        <button @click='$emit("enlarge-text", 5)'>扩大父组件中字体大小</button>
        <button @click='$emit("enlarge-text", 10)'>扩大父组件中字体大小</button>
      </div>
    `
  })
  var vm = new Vue({
    el: '#app',
    data: {
      pmsg: '父组件中内容',
      parr: ['apple', 'orange', 'banana'],
      fontSize: 10
    },
    methods: {
      handle: function(val) {
        // 扩大字体大小
        this.fontSize += val
      }
    }
  })
</script>
```

#### 兄弟之间的传递

- 兄弟之间传递数据需要借助于事件中心，通过事件中心传递数据
  - 提供事件中心 `var hub = new Vue()`
- 传递数据方，通过一个事件触发`hub.$emit(方法名，传递的数据)`
- 接收数据方，通过`mounted(){}` 钩子中 触发`hub.$on()`方法名
- 销毁事件 通过`hub.$off()`方法名销毁之后无法进行传递数据

```html
<div id="app">
  <div>父组件</div>
  <div>
    <button @click="handle">销毁事件</button>
  </div>
  <test-tom></test-tom>
  <test-jerry></test-jerry>
</div>
<script type="text/javascript" src="js/vue.js"></script>
<script type="text/javascript">
  /*
    兄弟组件之间数据传递
  */
  //1、 提供事件中心
  var hub = new Vue()

  Vue.component('test-tom', {
    data: function() {
      return {
        num: 0
      }
    },
    template: `
      <div>
        <div>TOM:{{num}}</div>
        <div>
          <button @click='handle'>点击</button>
        </div>
      </div>
    `,
    methods: {
      handle: function() {
        //2、传递数据方，通过一个事件触发hub.$emit(方法名，传递的数据)   触发兄弟组件的事件
        hub.$emit('jerry-event', 2)
      }
    },
    mounted: function() {
      // 3、接收数据方，通过mounted(){} 钩子中  触发hub.$on(方法名
      hub.$on('tom-event', val => {
        this.num += val
      })
    }
  })
  Vue.component('test-jerry', {
    data: function() {
      return {
        num: 0
      }
    },
    template: `
      <div>
        <div>JERRY:{{num}}</div>
        <div>
          <button @click='handle'>点击</button>
        </div>
      </div>
    `,
    methods: {
      handle: function() {
        //2、传递数据方，通过一个事件触发hub.$emit(方法名，传递的数据)   触发兄弟组件的事件
        hub.$emit('tom-event', 1)
      }
    },
    mounted: function() {
      // 3、接收数据方，通过mounted(){} 钩子中  触发hub.$on()方法名
      hub.$on('jerry-event', val => {
        this.num += val
      })
    }
  })
  var vm = new Vue({
    el: '#app',
    data: {},
    methods: {
      handle: function() {
        //4、销毁事件 通过hub.$off()方法名销毁之后无法进行传递数据
        hub.$off('tom-event')
        hub.$off('jerry-event')
      }
    }
  })
</script>
```

### 组件插槽

- 组件的最大特性就是复用性，而用好插槽能大大提高组件的可复用能力

#### 匿名插槽

```html
<div id="app">
  <!-- 这里的所有组件标签中嵌套的内容会替换掉slot  如果不传值 则使用 slot 中的默认值  -->
  <alert-box>有bug发生</alert-box>
  <alert-box>有一个警告</alert-box>
  <alert-box></alert-box>
</div>
<script type="text/javascript">
  /*
    组件插槽：父组件向子组件传递内容
  */
  Vue.component('alert-box', {
    template: `
      <div>
        <strong>ERROR:</strong>
  				# 当组件渲染的时候，这个 <slot> 元素将会被替换为“组件标签中嵌套的内容”。
  				# 插槽内可以包含任何模板代码，包括 HTML
        <slot>默认内容</slot>
      </div>
    `
  })
  var vm = new Vue({
    el: '#app'
  })
</script>
```

#### 具名插槽

- 具有名字的插槽
- 使用 `<slot>` 中的 "name" 属性绑定元素
- vue 2.6 + 使用 v-slot 取代 slot slot-scope, v-slot: 简写 #

```HTML
<div id="app">
  <base-layout>
     <!-- 2、 通过slot属性来指定, 这个slot的值必须和下面slot组件得name值对应上
      如果没有匹配到 则放到匿名的插槽中   -->
    <p slot='header'>标题信息</p>
    <p>主要内容1</p>
    <p>主要内容2</p>
    <p slot='footer'>底部信息信息</p>
  </base-layout>
  <base-layout>
    <!-- 注意点：template临时的包裹标签最终不会渲染到页面上     -->
    <!-- 多标签的情况下使用 -->
    <!-- vue 2.6 + 使用 v-slot 取代 slot slot-scope -->
    <!-- v-slot: 简写 # -->
    <template #header>
      	<!-- <template v-slot:header> -->
        <!-- <template slot="header"> -->
      <p>标题信息1</p>
      <p>标题信息2</p>
    </template>
    <p>主要内容1</p>
    <p>主要内容2</p>
    <template slot='footer'>
      <p>底部信息信息1</p>
      <p>底部信息信息2</p>
    </template>
  </base-layout>
</div>
<script type="text/javascript" src="js/vue.js"></script>
<script type="text/javascript">
  /*
    具名插槽
  */
  Vue.component('base-layout', {
    template: `
      <div>
        <header>
    				###	1、 使用 <slot> 中的 "name" 属性绑定元素 指定当前插槽的名字
          <slot name='header'></slot>
        </header>
        <main>
          <slot></slot>
        </main>
        <footer>
    			  ###  注意点：
    				###  具名插槽的渲染顺序，完全取决于模板，而不是取决于父组件中元素的顺序
          <slot name='footer'></slot>
        </footer>
      </div>
    `
  });
  var vm = new Vue({
    el: '#app'
  });
</script>
```

#### 作用域插槽

- 父组件对子组件加工处理
- 既可以复用子组件的 slot，又可以使 slot 内容不一致
- 2.6+ 以上的版本 用 v-slot 取代 slot-scope, 而且可以配合具名插槽使用

```html
<div id="app">
  <!-- 
  1、当我们希望li 的样式由外部使用组件的地方定义，因为可能有多种地方要使用该组件，
  但样式希望不一样 这个时候我们需要使用作用域插槽 

-->
  <fruit-list :list="list">
    <!-- 2、 父组件中使用了<template>元素,而且包含scope="slotProps",
    slotProps在这里只是临时变量   
  --->
    <!-- <template v-slot:default="slotProps"> 未命名可省略default-->
    <template v-slot="slotProps">
      <!-- template v-slot:zf="slotProps" -->
      <!-- <template slot-scope='slotProps'> -->
      <strong v-if="slotProps.info.id==3" class="current">
        {{slotProps.info.name}}
      </strong>
      <span v-else>{{slotProps.info.name}}</span>
    </template>
  </fruit-list>
</div>
<script type="text/javascript" src="js/vue.js"></script>
<script type="text/javascript">
  /*
    作用域插槽
  */
  Vue.component('fruit-list', {
    props: ['list'],
    template: `
      <div>
        <li :key='item.id' v-for='item in list'>
    ###  3、 在子组件模板中,<slot>元素上有一个类似props传递数据给组件的写法msg="xxx",
    ###   插槽可以提供一个默认内容，如果如果父组件没有为这个插槽提供了内容，会显示默认的内容。
        如果父组件为这个插槽提供了内容，则默认的内容会被替换掉
          <slot :info='item'>{{item.name}}</slot>
             ### <slot :info='item' name="zf">{{item.name}}</slot>
        </li>
      </div>
    `
  })
  var vm = new Vue({
    el: '#app',
    data: {
      list: [
        {
          id: 1,
          name: 'apple'
        },
        {
          id: 2,
          name: 'orange'
        },
        {
          id: 3,
          name: 'banana'
        }
      ]
    }
  })
</script>
```

## Vue 中的前后端交互

### 前提

#### 接口调用方式

- 原生 ajax
- 基于 jQuery 的 ajax
- fetch
- axios

#### URL 地址格式：

**传统形式的 URL：**

- 格式：schema://host:port/path?query#fragment
  1.  schema: 协议。例如：http、https、ftp 等
  2.  host: 域名或者 IP 地址
  3.  port: 端口，http 默认 80 端口可以省略
  4.  path: 路径 例如：comments/study/...
  5.  query: 查询参数，例如：uname=zf&age=22
  6.  fragment: 锚点（哈希 Hash），用于定位页面的某个位置

**Restful 形式的 URL：**

- HTTP 请求方式

  | 请求方式 | 作用  |
  | :------: | :---: |
  |   GET    | 查询  |
  |   POST   | 添加  |
  |   PUT    | 修改  |
  |  DELETE  | 删除  |

- 符合规则的 URL 地址：

  1.  http://www.dorc.top/comments GET
  2.  http://www.dorc.top/comments POST
  3.  http://www.dorc.top/comments/3 PUT
  4.  http://www.dorc.top/comments/3 DELETE

#### 异步调用：

JavaScript 的执行环境是「单线程」

所谓单线程，是指 JS 引擎中负责解释和执行 JavaScript 代码的线程只有一个，也就是一次只能完成一项任务，这个任务执行完后才能执行下一个，它会「阻塞」其他任务。这个任务可称为主线程

异步效果分析：

- 定时任何
- ajax
- 事件函数

多次异步调用的依赖分析：

- 多次异步调用的结果顺序不确定
- 使用嵌套，使异步调用结果存在依赖关系

#### Promise 概述

Promise 是异步编程的一种解决方案，语法上，Promise 是一个对象，从他可以获取异步操作的信息

- 主要解决异步深层嵌套的问题
- promise 提供了简洁的 API 使得异步操作更加容易

```js
/* 1. Promise基本使用
     我们使用new来构建一个Promise  Promise的构造函数接收一个参数，是函数，并且传入两个参数：		   resolve，reject， 分别表示异步操作执行成功后的回调函数和异步操作执行失败后的回调函数 */

var p = new Promise(function(resolve, reject) {
  //2. 这里用于实现异步任务  setTimeout
  setTimeout(function() {
    var flag = false
    if (flag) {
      //3. 正常情况
      resolve('hello')
    } else {
      //4. 异常情况
      reject('出错了')
    }
  }, 100)
})
//  5 Promise实例生成以后，可以用then方法指定resolved状态和reject状态的回调函数
//  在then方法中，你也可以直接return数据而不是Promise对象，在后面的then中就可以接收到数据了
p.then(
  function(data) {
    console.log(data)
  },
  function(info) {
    console.log(info)
  }
)
```

##### 基于 Promise 发送 Ajax 请求

```js
function queryData(url) {
  // 1.1 创建一个Promise实例
  var p = new Promise(function(resolve, reject) {
    var xhr = new XMLHttpRequest()
    xhr.onreadystatechange = function() {
      if (xhr.readyState != 4) return
      if (xhr.readyState == 4 && xhr.status == 200) {
        // 1.2 处理正常的情况
        resolve(xhr.responseText)
      } else {
        // 1.3 处理异常情况
        reject('服务器错误')
      }
    }
    xhr.open('get', url)
    xhr.send(null)
  })
  return p
}
// 注意：  这里需要开启一个服务
// 在then方法中，你也可以直接return数据而不是Promise对象，在后面的then中就可以接收到数据了
queryData('http://localhost:3000/data')
  .then(function(data) {
    console.log(data)
    // 1.4 想要继续链式编程下去 需要 return
    return queryData('http://localhost:3000/data1')
  })
  .then(function(data) {
    console.log(data)
    return queryData('http://localhost:3000/data2')
  })
  .then(function(data) {
    console.log(data)
  })
```

##### then 参数中函数的返回值：

1. 返回 Promise 实例对象：
   - 返回的该实例对象会调用下一个 then
2. 返回普通值：
   - 返回的普通值会直接传递给下一个 then ，通过 then 参数中函数的参数接受该值
   - then 默认产生 promise 实例对象，保证下一个 then 可以进行链式操作

##### Promise 常用 API：

1. 实例方法：
   - p.then() 得到异步操作的正确结果
   - p.catch() 获取异常信息(取代 then 中的第二个参数)
   - p.finally() 成功与否都会执行（不是正式标准）
   ```js
   foo()
     .then(result => {
       console.log(result)
     })
     .catch(err => {
       console.log(err)
     })
     .finally(() => {
       console.log('finish')
     })
   ```
2. 对象方法：(静态方法)
   - Promise.all 并发处理多个异步任务，所有任务都执行完才能得到结果
     - `Promise.all`方法接受一个数组作参数，数组中的对象（p1、p2、p3）均为 promise 实例（如果不是一个 promise，该项会被用`Promise.resolve`转换为一个 promise)。它的状态由这三个 promise 实例决定
   - Promise.race 并发处理多个异步任务，只要有一个任务完成就能得到结果
     - `Promise.race`方法同样接受一个数组作参数。当 p1, p2, p3 中有一个实例的状态发生改变（变为`fulfilled`或`rejected`），p 的状态就跟着改变。并把第一个改变状态的 promise 的返回值，传给 p 的回调函数

```js
var p1 = queryData('http://localhost:3000/a1')
var p2 = queryData('http://localhost:3000/a2')
var p3 = queryData('http://localhost:3000/a3')
Promise.all([p1, p2, p3]).then(function(result) {
  console.log(result)
  //   all 中的参数  [p1,p2,p3]   和 返回的结果一 一对应
  //   ["HELLO TOM", "HELLO JERRY", "HELLO SPIKE"]
})
Promise.race([p1, p2, p3]).then(function(result) {
  console.log(result) // "HELLO TOM"
  //   由于p1执行较快，Promise的then()将获得结果'P1'。
  //   p2,p3仍在继续执行，但执行结果将被丢弃。
})
```

### Fetch API 概述：

- Fetch API 是新的 ajax 解决方案 Fetch 会返回 Promise
- **fetch 不是 ajax 的进一步封装，而是原生 js ，没有使用 XMLHttpRequest 对象**。
- `fetch(url, options).then(）`

```js
/*
  Fetch API 基本用法
  	fetch(url).then()
 	第一个参数请求的路径   Fetch会返回Promise   所以我们可以使用then 拿到请求成功的结果 
*/
fetch('http://localhost:3000/fdata')
  .then(function(data) {
    // text()方法属于fetchAPI的一部分，它返回一个Promise实例对象，用于获取后台返回的数据
    return data.text()
  })
  .then(function(data) {
    //   在这个then里面我们能拿到最终的数据
    console.log(data)
  })
```

#### fetch API 中的 HTTP 请求

**常用配置选项：**

- method(String) : HTTP 请求方法，默认为 GET (GET, POST, PUT, DELETE)
- body(String) : HTTP 的请求参数
- header(Object) : HTTP 的请求头，默认{}

HTTP 协议，它给我们提供了很多的方法，如 POST，GET，DELETE，UPDATE，PATCH 和 PUT

- 默认的是 GET 请求
- 需要在 options 对象中 指定对应的 method method:请求使用的方法
- post 和 普通 请求的时候 需要在 options 中 设置 请求头 headers 和 body

```js
// 1.1 GET参数传递 - 传统URL  通过url  ？ 的形式传参
fetch('http://localhost:3000/books?id=123', {
  // get 请求可以省略不写 默认的是GET
  method: 'get'
})
  .then(function(data) {
    // 它返回一个Promise实例对象，用于获取后台返回的数据
    return data.text()
  })
  .then(function(data) {
    // 在这个then里面我们能拿到最终的数据
    console.log(data)
  })
//------------- 以下省略 then 部分代码 -------------

// 1.2 GET参数传递  restful形式的URL  通过/ 的形式传递参数
//     即  id = 456 和id后台的配置有关
fetch('http://localhost:3000/books/456', {
  method: 'get'
})

// 2.1  DELETE请求方式参数传递      删除id  是  id=789
fetch('http://localhost:3000/books/789', {
  method: 'delete'
})

// 3.1  POST请求传参（查询字符串）
fetch('http://localhost:3000/books', {
  method: 'post',
  //  传递数据
  body: 'uname=lisi&pwd=123',
  //  设置请求头
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
})

// 3.2  POST请求传参（json字符串）
fetch('http://localhost:3000/books', {
  method: 'post',
  body: JSON.stringify({
    uname: '张三',
    pwd: '456'
  }),
  headers: {
    'Content-Type': 'application/json'
  }
})

// 4 PUT请求传参     修改id 是 123
fetch('http://localhost:3000/books/123', {
  method: 'put',
  body: JSON.stringify({
    uname: '张三',
    pwd: '789'
  }),
  headers: {
    'Content-Type': 'application/json'
  }
})
```

#### Fetch API 中 响应格式

用 fetch 来获取数据，如果响应正常返回，我们首先看到的是一个 response 对象，其中包括返回的一堆原始字节，这些字节需要在收到后，需要我们通过调用方法将其转换为相应格式的数据，比如 `JSON` ，`BLOB` 或者 `TEXT` 等等

```js
fetch('http://localhost:3000/json')
  .then(function(data) {
    // return data.json();   //  将获取到的数据使用 json 转换对象
    return data.text() //  //  将获取到的数据 转换成字符串
  })
  .then(function(data) {
    // console.log(data.uname)
    // console.log(typeof data)
    var obj = JSON.parse(data)
    console.log(obj.uname, obj.age, obj.gender)
  })
```

### axios 概述：

- 基于 promise 用于浏览器和 node.js 的 http 客户端
- 支持 浏览器 和 node.js
- 支持 promise
- 能拦截请求和响应
- 自动转换 JSON 数据
- 能转换请求和响应数据

#### axios 基础用法

- get 和 delete 请求传递参数

  - 通过传统的 url 以 ? 的形式传递参数
  - restful 形式传递参数
  - 通过 params 形式传递参数

- post 和 put 请求传递参数
  - 通过选项传递参数
  - 通过 URLSearchParams 传递参数 (application/x-www-form-urlencoded)

```js
// 1. 发送get 请求
axios.get('http://localhost:3000/adata').then(function(ret) {
  // 拿到 ret 是一个对象      所有的对象都存在 ret 的data 属性里面
  // 注意data属性是固定的用法，用于获取后台的实际数据
  // console.log(ret.data)
  console.log(ret)
})

// 2.  get 请求传递参数

// 2.1  通过传统的url  以 ? 的形式传递参数
axios.get('http://localhost:3000/axios?id=123').then(function(ret) {
  console.log(ret.data)
})

// 2.2  restful 形式传递参数
axios.get('http://localhost:3000/axios/123').then(function(ret) {
  console.log(ret.data)
})

// 2.3  通过params  形式传递参数
axios
  .get('http://localhost:3000/axios', {
    params: {
      id: 789
    }
  })
  .then(function(ret) {
    console.log(ret.data)
  })

// 3 axios delete 请求传参     传参的形式和 get 请求一样
axios
  .delete('http://localhost:3000/axios', {
    params: {
      id: 111
    }
  })
  .then(function(ret) {
    console.log(ret.data)
  })

// 4  axios 的 post 请求

// 4.1  通过选项传递参数
axios
  .post('http://localhost:3000/axios', {
    uname: 'lisi',
    pwd: 123
  })
  .then(function(ret) {
    console.log(ret.data)
  })

// 4.2  通过 URLSearchParams  传递参数
var params = new URLSearchParams()
params.append('uname', 'zhangsan')
params.append('pwd', '111')
axios.post('http://localhost:3000/axios', params).then(function(ret) {
  console.log(ret.data)
})

// 5  axios put 请求传参   和 post 请求一样
axios
  .put('http://localhost:3000/axios/123', {
    uname: 'lisi',
    pwd: 123
  })
  .then(function(ret) {
    console.log(ret.data)
  })
```

#### axios 的响应结果

响应结果的主要属性：

- data: 实际响应回来的数据
- headers: 响应头信息
- status: 响应状态码
- statusText: 响应状态信息

axios 的全局配置：

```js
// 配置公共的请求头
axios.defaults.baseURL = 'https://api.example.com'
// 配置 超时时间
axios.defaults.timeout = 2500
// 配置公共的请求头
axios.defaults.headers['mytoken'] = 'hello'
// axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
// 配置公共的 post 的 Content-Type
axios.defaults.headers.post['Content-Type'] =
  'application/x-www-form-urlencoded'
```

#### axios 拦截器

- 请求拦截器

  - 请求拦截器的作用是在请求发送前进行一些操作

    - 例如在每个请求体里加上 token，统一做了处理如果以后要改也非常容易

- 响应拦截器
  - 响应拦截器的作用是在接收到响应后进行一些操作
    - 例如在服务器返回登录状态失效，需要重新登录的时候，跳转到登录页

```js
// 1. 请求拦截器
axios.interceptors.request.use(
  function(config) {
    console.log(config.url)
    // 1.1  任何请求都会经过这一步   在发送请求之前做些什么
    config.headers.mytoken = 'nihao'
    // 1.2  这里一定要return   否则配置不成功
    return config
  },
  function(err) {
    // 1.3 对请求错误做点什么
    console.log(err)
  }
)

// 2. 响应拦截器
axios.interceptors.response.use(
  function(res) {
    // 2.1  在接收响应做些什么
    var data = res.data
    return data
  },
  function(err) {
    // 2.2 对响应错误做点什么
    console.log(err)
  }
)
```

### async / await 概述：

- `async` / `await` 是 ES7 引入的新语法，可以更加方便的进行异步操作
- `async` 作为一个关键字放到函数前面（ `async` 函数的返回值，是 `Promise` 实例对象）
  - 任何一个 `async` 函数都会隐式返回一个`promise`
- `await` 关键字只能在使用 `async` 定义的函数中使用， `await` 可以得到 异步的结果
  - `await` 后面要跟 `Promise` 实例对象，在 `Promise` 中处理异步任务。
  - `await` 函数不能单独使用

```js
// 1.  async 基础用法
// 1.1 async作为一个关键字放到函数前面
async function queryData() {
  // 1.2 await 关键字只能在使用async定义的函数中使用  await 后面可以直接跟一个 Promise 实例对象
  var ret = await new Promise(function(resolve, reject) {
    setTimeout(function() {
      resolve('nihao')
    }, 1000)
  })
  // console.log(ret.data)
  return ret
}
// 1.3 任何一个async函数都会隐式返回一个promise   我们可以使用then 进行链式编程
queryData().then(function(data) {
  console.log(data)
})

// 2.  async    函数处理多个异步函数
axios.defaults.baseURL = 'http://localhost:3000'

async function queryData() {
  // 2.1  添加await之后 当前的await 返回结果之后才会执行后面的代码

  var info = await axios.get('async1')
  // 2.2  让异步代码看起来、表现起来更像同步代码
  var ret = await axios.get('async2?info=' + info.data)
  return ret.data
}

queryData().then(function(data) {
  console.log(data)
})
```

## Vue 中的路由

### 路由基本概念：

**路由的本质就是一种对应关系**

- 在 url 地址中输入要访问的 url 地址之后，浏览器要去请求这个 url 地址对应的资源。
- 那么 url 地址和真实的资源之间就有一种对应的关系，就是路由。

路由分为前端路由和后端路由：

1. 后端路由是由服务器端进行实现，并完成资源的分发
2. 前端路由是依靠 hash 值(锚链接)的变化进行实现
   - 前端路由的基本概念：根据不同的事件来显示不同的页面内容，即事件与事件处理函数之间的对应关系
   - 前端路由主要做的事情就是监听事件并分发执行事件处理函数

### 前端路由基础：

前端路由是基于 hash 值的变化进行实现的（比如点击页面中的菜单或者按钮改变 URL 的 hash 值，根据 hash 值的变化来控制组件的切换）

**核心实现依靠一个事件，即监听 hash 值变化的事件**

```js
window.onhashchange = function() {
  //location.hash可以获取到最新的hash值
  location.hash
}
```

前端路由实现 tab 栏切换：

核心思路：

- 当点击这些超链接的时候，就会改变 `url` 地址中的 `hash` 值，当 `hash` 值被改变时，就会触发 `onhashchange` 事件
- 在触发 `onhashchange` 事件的时候，根据 `hash` 值来让不同的组件进行显示

```html
<!-- 被 vue 实例控制的 div 区域 -->
<div id="app">
  <!-- 切换组件的超链接 -->
  <a href="#/zhuye">主页</a>
  <a href="#/keji">科技</a>
  <a href="#/caijing">财经</a>
  <a href="#/yule">娱乐</a>
  <!-- 根据 :is 属性指定的组件名称，把对应的组件渲染到 component 标签所在的位置 -->
  <!-- 可以把 component 标签当做是【组件的占位符】 -->
  <component :is="comName"></component>
</div>

<script>
  // #region 定义需要被切换的 4 个组件 略
  // #endregion

  // #region vue 实例对象
  const vm = new Vue({
    el: '#app',
    data: {
      comName: 'zhuye'
    },
    // 注册私有组件
    components: {
      zhuye,
      keji,
      caijing,
      yule
    }
  })
  // #endregion

  // 监听 window 的 onhashchange 事件，根据获取到的最新的 hash 值，切换要显示的组件的名称
  window.onhashchange = function() {
    // 通过 location.hash 获取到最新的 hash 值
    console.log(location.hash)
    switch (location.hash.slice(1)) {
      case '/zhuye':
        vm.comName = 'zhuye'
        break
      case '/keji':
        vm.comName = 'keji'
        break
      case '/caijing':
        vm.comName = 'caijing'
        break
      case '/yule':
        vm.comName = 'yule'
        break
    }
  }
</script>
```

### Vue Router 概述：

- 它是一个 `Vue.js` 官方提供的路由管理器。是一个功能更加强大的前端路由器，推荐使用。
- `Vue Router` 和 `Vue.js` 非常契合，可以一起方便的实现 **SPA(single page web application,单页应用程序)** 应用程序的开发。
- `Vue Router` 依赖于 `Vue` ，所以需要先引入 `Vue` ，再引入 `Vue Router`

#### Vue Router 的特性：

- 支持 H5 历史模式或者 hash 模式
- 支持嵌套路由
- 支持路由参数
- 支持编程式路由
- 支持命名路由
- 支持路由导航守卫
- 支持路由过渡动画特效
- 支持路由懒加载
- 支持路由滚动行为

#### Vue Router 的使用步骤：

1. 导入 js 文件

   ```html
   <script src="lib/vue_2.5.22.js"></script>
   <script src="lib/vue-router_3.0.2.js"></script>
   ```

2. 添加路由链接: `<router-link>` 是路由中提供的标签，默认会被渲染为 `a` 标签， `to` 属性默认被渲染为 `href` 属性，`to` 属性的值会被渲染为 `#` 开头的 `hash` 地址

   ```html
   <router-link to="/user">User</router-link>
   <router-link to="/login">Login</router-link>
   ```

3. 添加路由填充位（路由占位符）

   ```html
   <router-view></router-view>
   ```

4. 定义路由组件

   ```js
   var User = { template: '<div>This is User</div>' }
   var Login = { template: '<div>This is Login</div>' }
   ```

5. 配置路由规则并创建路由实例

   - 路由重定向：可以通过路由重定向为页面设置默认展示的组件
     - 在路由规则中添加一条路由规则即可

   ```js
   var myRouter = new VueRouter({
     //routes是路由规则数组
     routes: [
       //path设置为/表示页面最初始的地址 / ,redirect表示要被重定向的新地址，设置为一个路由即可
       { path: '/', redirect: '/user' },
       //每一个路由规则都是一个对象，对象中至少包含path和component两个属性
       //path表示  路由匹配的hash地址，component表示路由规则对应要展示的组件对象
       { path: '/user', component: User },
       { path: '/login', component: Login }
     ]
   })
   ```

6. 将路由挂载到 Vue 实例中

   ```js
   new Vue({
     el: '#app',
     //通过router属性挂载路由对象
     router: myRouter
   })
   ```

**总结：**

- 导入 js 文件
- 添加路由链接
- 添加路由占位符(最后路由展示的组件就会在占位符的位置显示)
- 定义路由组件
- 配置路由规则并创建路由实例
- 将路由挂载到 Vue 实例中

#### 嵌套路由

- 当我们进行路由的时候显示的组件中还有新的子级路由链接以及内容。

  ```js
  var User = { template: '<div>This is User</div>' }
  //Login组件中的模板代码里面包含了子级路由链接以及子级路由的占位符
  var Login = {
    template: `<div>
      <h1>This is Login</h1>
      <hr>
      <router-link to="/login/account">账号密码登录</router-link>
      <router-link to="/login/phone">扫码登录</router-link>
      <!-- 子路由组件将会在router-view中显示 -->
      <router-view></router-view>
      </div>`
  }

  //定义两个子级路由组件
  var account = { template: '<div>账号：<input><br>密码：<input></div>' }
  var phone = { template: '<h1>扫我二维码</h1>' }
  var myRouter = new VueRouter({
    //routes是路由规则数组
    routes: [
      { path: '/', redirect: '/user' },
      { path: '/user', component: User },
      {
        path: '/login',
        component: Login,
        //通过children属性为/login添加子路由规则
        children: [
          { path: '/login/account', component: account },
          { path: '/login/phone', component: phone }
        ]
      }
    ]
  })
  ```

#### 动态路由匹配

##### 基本用法：

```js
const user = {
  template: '<h1>user page --- user id : {{$route.params.id}}</h1>'
}
const myRouter = new VueRouter({
  //routes是路由规则数组
  routes: [
    //通过/:参数名  的形式传递参数
    { path: '/user/:id', component: User }
  ]
})
```

##### 组件传参 推荐使用 props

- 使用 `$route.params.id` 来获取路径传参的数据不够灵活。
- 可以使用 props 来传参

**props 的值为 Booleans**

- 需要在所对应组件的 路由规则中添加 `props` 将其设置为 `true`，那么在相应的组件中使用 props 接收 id ,即可接收到 id 的值

  ```js
  var User = {
    props: ['id'],
    template: '<div>用户：{{id}}</div>'
  }

  var myRouter = new VueRouter({
    //routes是路由规则数组
    routes: [
      //通过/:参数名  的形式传递参数
      //如果props设置为true，route.params将会被设置为组件属性
      { path: '/user/:id', component: User, props: true }
    ]
  })
  ```

**props 的值为 对象**

- 另外，可以将 `props` 设置为**对象**，那么就直接将对象的数据传递给组件进行使用
  - 相当于 ：为 user 组件通过路由的形式传递了两个动态参数 分别是 username pwd
  - 想要使用，在相应的组件中接收即可
  - 此时 id 不能访问到，id 相当于失效了
  ```js
  var User = {
    props: ['username', 'pwd'],
    template: '<div>用户：{{username}}---{{pwd}}</div>'
  }
  var myRouter = new VueRouter({
    //routes是路由规则数组
    routes: [
      //通过/:参数名  的形式传递参数
      //如果props设置为对象，则传递的是对象中的数据给组件
      {
        path: '/user/:id',
        component: User,
        props: { username: 'jack', pwd: 123 }
      }
    ]
  })
  ```

**props 的值为 函数**

- 如果想要获取传递的参数值还想要获取传递的对象数据，那么 props 应该设置为 **函数形式**。

  ```js
  var User = {
    props: ['username', 'pwd', 'id'],
    template: '<div>用户：{{id}} -> {{username}}---{{pwd}}</div>'
  }

  var myRouter = new VueRouter({
    // routes是路由规则数组
    routes: [
      // 通过/:参数名  的形式传递参数
      // 如果props设置为函数，则通过函数的第一个参数获取路由对象
      // 并可以通过路由对象的params属性获取传递的参数
      {
        path: '/user/:id',
        component: User,
        props: route => ({ username: 'jack', pwd: 123, id: route.params.id })
      }
    ]
  })
  ```

#### 命名路由

```html
<router-link :to="{ name:'user' , params: {id:123} }">User</router-link>

<script>
  var myRouter = new VueRouter({
    //routes是路由规则数组
    routes: [
      //通过name属性为路由添加一个别名
      //添加了别名之后，可以使用别名进行跳转
      { path: '/user/:id', component: User, name: 'user' }
    ]
  })
</script>
```

#### 编程式导航

**页面导航的两种方式：**

1. 声明式导航：通过点击链接的方式实现的导航
   - 普通网页中的 `<a></a>` 链接 或 `vue` 中的 `<router-link></router-link>`
2. 编程式导航：调用 js 的 api 方法实现导航
   - 普通网页中的 `location.href`

**Vue-Router 中常见的导航方式：**

```js
this.$router.push('hash地址')
this.$router.push('/login')
this.$router.push({ name: 'user', params: { id: 123 } })
this.$router.push({ path: '/login' })
this.$router.push({ path: '/login', query: { username: 'jack' } })

this.$router.go(n) //n为数字，参考history.go
this.$router.go(-1)
```

## Vue 中的模块化

### 模块化概述：

- **模块化** 就是把单独的一个功能封装到一个模块（文件）中，模块之间相互隔离，但是可以通过特定的接口公开内部成员，也可以依赖别的模块
- 模块化开发的好处：方便代码的重用，从而提升开发效率，并且方便后期的维护

#### 模块化相关规范

1. 浏览器端模块化规范
   - AMD(Asynchronous Module Definition,异步模块定义)
     - 代表产品为：Require.js
   - CMD(Common Module Definition,通用模块定义)
     - 代表产品为：Sea.js
2. 服务器端的模块化
   - CommonJS 规范
     - 使用 require 引入其他模块或者包
     - 使用 exports 或者 module.exports 导出模块成员
     - 一个文件就是一个模块，都拥有独立的作用域
3. 大一统的模块化规范 - ES6 模块化
   - 每一个 js 文件都是独立的模块
   - 导入模块成员使用 import 关键字
   - 暴露模块成员使用 export 关键字

**小结：**

- 推荐使用 ES6 模块化，因为 AMD，CMD 局限使用与浏览器端，而 CommonJS 在服务器端使用。
- ES6 模块化是浏览器端和服务器端通用的规范.

#### Node.js 中通过 babel 体验 ES6 模块化

1. 安装：

   ```shell
   npm install --save-dev @babel/core @babel/cli @babel/preset-env @babel/node
   npm install --save @babel/polyfill
   ```

2. 配置文件

   - 项目跟目录创建文件 babel.config.js
     ```js
     const presets = [
       [
         '@babel/env',
         {
           targets: {
             edge: '17',
             firefox: '60',
             chrome: '67',
             safari: '11.1'
           }
         }
       ]
     ]
     //暴露
     module.exports = { presets }
     ```

3. 在项目中 创建 index.js 入口文件

4. 使用 npx 执行文件

   - 打开终端，输入命令：`npx babel-node ./index.js`

##### 设置默认导入/导出

默认导出:

```js
let a = 10
let c = 30
function show() {
  console.log('777777777777777')
}
export default {
  a,
  c,
  show
}
```

默认导入:

```js
// import 接收名称 from "模块标识符"，
import m1 from './m1.js'
```

**注意：**

- 在一个模块中，只允许使用 export default 向外默认暴露一次成员，千万不要写多个 export default。
- 如果在一个模块中没有向外暴露成员，其他模块引入该模块时将会得到一个空对象

##### 设置按需导入/导出

按需导出:

```js
export let s1 = 'aa'
export let s2 = 'cc'
export function say() {
  console.log('986')
}
```

按需导入:

```js
// 同时导入 默认导出 的成员 以及 按需导入 的成员
// 使用 as 关键字 起别名，取代原名使用，原名失效
import m1, { s1, s2 as ss2, say } from './m1.js'
```

注意：

- 一个模块中既可以按需导入也可以默认导入，一个模块中既可以按需导出也可以默认导出
- 每个模块中，可以使用多次按需导出

##### 直接导入并执行模块代码

- 只想单纯执行某个模块中的代码，并不需要得到模块中向外暴露的成员，此时，可以直接导入并执行模块代码。

```js
// ----------- 入口文件
import './m2.js'
// ----------- m2.js 文件
for (let i = 0; i < 3; i++) {
  console.log(i)
}
```

### webpack 概述

#### webpack 简介

- webpack 是一个流行的前端项目构建工具（打包工具），可以解决当前 web 开发中所面临的困境
- webpack 提供了友好的模块化支持，以及代码压缩混淆、处理 js 兼容问题、性能优化等强大的功能，从而让程序员把工作的重心放到具体的功能实现上，提高了开发效率和项目的可维护性。

#### webpack 的基本使用

1. 安装：
   - 打开项目目录终端，输入命令:
   - `npm install webpack webpack-cli -D`
2. webpack 配置
   - 在项目根目录中，创建一个 webpack.config.js 的配置文件用来配置 webpack
   - 在 webpack.config.js 文件中编写代码进行 webpack 配置：
     ```js
     module.exports = {
       mode: 'development'
       //可以设置为development(开发模式)，production(发布模式)
     }
     ```
     - 补充：mode 设置的是项目的编译模式。
     - 如果设置为 development 则表示项目处于开发阶段，不会进行压缩和混淆，打包速度会快一些
     - 如果设置为 production 则表示项目处于上线发布阶段，会进行压缩和混淆，打包速度会慢一些
3. npm 配置
   - 修改项目中的 package.json 文件添加运行脚本 dev ，如下：
     ```js
     "scripts":{
         "dev":"webpack"
     }
     ```
   - 注意：scripts 节点下的脚本，可以通过 npm run 运行
   - 将会启动 webpack 进行项目打包
4. 运行 dev 命令进行项目打包，并在页面中引入项目打包生成的 js 文件
   - 打开项目目录终端，输入命令: `npm run dev`
   - 等待 webpack 打包完毕之后，找到默认的 dist 路径中生成的 main.js 文件，将其引入到 html 页面中。

#### 配置 webpack 的打包入口/出口

- 在 webpack 4.x 中，默认会将 src/index.js 作为默认的打包入口 js 文件
  默认会将 dist/main.js 作为默认的打包输出 js 文件
- 如果不想使用默认的入口/出口 js 文件，我们可以通过改变 webpack.config.js 来设置入口/出口的 js 文件

```js
const path = require('path')
module.exports = {
  mode: 'development',
  //设置入口文件路径
  entry: path.join(__dirname, './src/index.js'),
  //设置出口文件
  output: {
    //设置路径
    path: path.join(__dirname, './dist'),
    //设置文件名
    filename: 'bundle.js'
  }
}
```

#### 配置 webpack 的自动打包功能

> 默认情况下，我们更改入口 js 文件的代码，需要重新运行命令打包 webpack，才能生成出口的 js 文件那么每次都要重新执行命令打包，这是一个非常繁琐的事情，那么，自动打包可以解决这样繁琐的操作。

1. 安装支持项目自动打包的工具

   - `npm install webpack-dev-server –D`

2. 修改 `package.json` -> `scripts` 中的 `dev` 命令

   ```js
   "scripts": {
     "dev": "webpack-dev-server"
     // script 节点下的脚本，可以通过 npm run 执行
   }
   ```

3. 将 `src` -> `index.html` 中，`script` 脚本的引用路径，修改为 `"/buldle.js"`

4. 运行 `npm run dev` 命令，重新进行打包

5. 在浏览器中访问 `http://localhost:8080` 地址，查看自动打包效果

**注意：**

- `webpack-dev-server` 会启动一个实时打包的 http 服务器
- `webpack-dev-server` 打包生成的输出文件，默认放到了项目根目录中，而且是虚拟的、看不见的

#### 配置 html-webpack-plugin 生成预览页面

- 因为当我们访问默认的 `http://localhost:8080/` 的时候，看到的是一些文件和文件夹，想要查看我们的页面 还需要点击文件夹点击文件才能查看，那么我们希望默认就能看到一个页面，而不是看到文件夹或者目录。

1. 安装生成预览页面的插件
   `npm install html-webpack-plugin –D`

2. 修改 `webpack.config.js` 文件头部区域，添加如下配置信息:

   ```js
   // 导入生成预览页面的插件，得到一个构造函数
   const HtmlWebpackPlugin = require('html-webpack-plugin')
   const htmlPlugin = new HtmlWebpackPlugin({
     // 创建插件的实例对象
     template: './src/index.html', // 指定要用到的模板文件
     filename: 'index.html' // 指定生成的文件的名称，该文件存在于内存中在目录中不显示
   })
   ```

3. 修改 `webpack.config.js` 文件中向外暴露的配置对象，新增如下配置节点：
   ```js
   module.exports = {
     ...
     plugins: [ htmlPlugin ]
     // plugins 数组是 webpack 打包期间会用到的一些插件列表
   }
   ```

#### 配置自动打包相关的参数

- 在自动打包完毕之后，默认打开服务器网页，实现方式就是打开 `package.json` 文件，修改 `dev` 命令：

```js
// package.json中的配置
// --open 打包完成后自动打开浏览器页面
// --host 配置 IP 地址
// --port 配置端口
"scripts": {
"dev": "webpack-dev-server --open --host 127.0.0.1 --port 8888"
},
```

#### webpack 中的加载器

##### 通过 loader 打包非 js 模块

- 在实际开发过程中， `webpack` 默认只能打包处理以 `.js` 后缀名结尾的模块，其他非 `.js` 后缀名结尾的模块，`webpack` 默认处理不了，需要调用 `loader` 加载器才可以正常打包，否则会报错！

`loader` 加载器可以协助 `webpack` 打包处理特定的文件模块:

- less-loader 可以打包处理 .less 相关的文件
- sass-loader 可以打包处理 .scss 相关的文件
- url-loader 可以打包处理 css 中与 url 路径相关的文件

##### webpack 中加载器的基本使用

1. 打包处理 css 文件

   - 安装处理 css 文件的 loader
     - 运行 `npm i style-loader css-loader -D` 命令
   - 在 `webpack.config.js` 的 `module` -> `rules` 数组中，添加 `loader` 规则
     ```js
     // 所有第三方文件模块的匹配规则
     module: {
       rules: [
         //test设置需要匹配的文件类型，支持正则
         //use表示该文件类型需要调用的loader
         { test: /\.css$/, use: ['style-loader', 'css-loader'] }
       ]
     }
     ```

   **注意：**

   - use 数组中指定的 loader 顺序是固定的
   - 多个 loader 的调用顺序是：从后往前调用

2. 打包处理 less 文件

   - 安装处理 less 文件的 loader
     - 运行 `npm i less-loader less -D` 命令
   - 在 `webpack.config.js` 的 `module` -> `rules` 数组中，添加 `loader` 规则
     ```js
     { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] }
     ```

3. 打包处理 scss 文件

   - 安装处理 scss 文件的 loader
     - 运行 `npm i sass-loader node-sass -D` 命令
       - node-sass 必须要用 cnpm 安装否则报错
       - sass-loader 高版本有识别问题，需要安装：7.3.1 版本
   - 在 `webpack.config.js` 的 `module` -> `rules` 数组中，添加 `loader` 规则
     ```js
     { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] }
     ```

4. 配置 postCSS 自动添加 css 的兼容前缀

   - 安装包
     - 运行 `npm i postcss-loader autoprefixer -D` 命令
   - 在项目根目录中创建 `postcss` 的配置文件 `postcss.config.js`，并初始化
     ```js
     // 导入自动添加前缀的插件
     const autoprefixer = require('autoprefixer')
     module.exports = {
       plugins: [autoprefixer] // 挂载插件
     }
     ```
   - 在 `webpack.config.js` 的 `module` -> `rules` 数组中，**修改** `css` 的 `loader` 规则
     ```js
     { test:/\.css$/, use: ['style-loader', 'css-loader', 'postcss-loader'] }
     ```

5. 打包样式表中的图片和字体文件

   - 安装处理 打包图片文件 以及 字体文件的 loader
     - 运行 `npm i url-loader file-loader -D` 命令
   - 在 `webpack.config.js` 的 `module` -> `rules` 数组中，添加 `loader` 规则
     ```js
     {
       test: /\.jpg|png|gif|bmp|ttf|eot|svg|woff|woff2$/,
       use: 'url-loader?limit=16940'
     }
     ```

   **注意：**

   - 其中 ? 之后的是 loader 的参数项。
   - limit 用来指定图片的大小，单位是字节(byte),只有小于 limit 大小的图片，才会被转为 base64 图片

6. 打包处理 js 文件中的高级语法

   - 安装 babel 转换器相关的包
     - 运行 `npm i babel-loader @babel/core @babel/runtime -D` 命令
   - 安装 babel 语法插件相关的包
     - 运行 `npm i @babel/preset-env @babel/plugin-transform-runtime @babel/plugin-proposal-class-properties –D` 命令
   - 在项目根目录中，创建 `babel` 配置文件 `babel.config.js` 并初始化基本配置
     ```js
     module.exports = {
       presets: ['@babel/preset-env'],
       plugins: [
         '@babel/plugin-transform-runtime',
         '@babel/plugin-proposal-class-properties'
       ]
     }
     ```
   - 在 `webpack.config.js` 的 `module` -> `rules` 数组中，添加 `loader` 规则：
     ```js
     // exclude 为排除项，表示 babel-loader 不需要处理 node_modules 中的 js 文件
     { test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ }
     ```

### Vue 单文件组件

#### 传统组件的问题和解决方案

**问题:**

- 全局定义的组件必须保证组件的名称不重复
- 字符串模板缺乏语法高亮，在 HTML 有多行的时候，需要用到丑陋的 \
- 不支持 CSS 意味着当 HTML 和 JavaScript 组件化时，CSS 明显被遗漏
- 没有构建步骤限制，只能使用 HTML 和 ES5 JavaScript, 而不能使用预处理器（如：Babel）

**解决方案:**

- 针对传统组件的问题，Vue 提供了一个解决方案 —— 使用 Vue 单文件组件。

#### Vue 单文件组件的基本用法

**单文件组件的组成结构**

- template 组件的模板区域
- script 业务逻辑区域
- style 样式区域

```html
<template>
  <!-- 这里用于定义Vue组件的模板内容 -->
</template>
<script>
  // 这里用于定义Vue组件的业务逻辑
  export default {
  data: () { return {} }, // 私有数据
  methods: {} // 处理函数
  // ... 其它业务逻辑
  }
</script>
<style scoped>
  /* 这里用于定义组件的样式 */
</style>
```

#### webpack 中配置 vue 组件的加载器

1. 运行 `npm i vue-loader vue-template-compiler -D` 命令
2. 在 `webpack.config.js` 配置文件中，添加 `vue-loader` 的配置项

   ```js
   const VueLoaderPlugin = require('vue-loader/lib/plugin')
   module.exports = {
     module: {
       rules: [
         // ... 其它规则
         { test: /\.vue$/, loader: 'vue-loader' }
       ]
     },
     plugins: [
       // ... 其它插件
       new VueLoaderPlugin() // 请确保引入这个插件！
     ]
   }
   ```

#### 在 webpack 项目中使用 vue

- 运行 npm i vue –S 安装 vue
- 在 src -> index.js 入口文件中，通过 `import Vue from 'vue'` 来导入 vue 构造函数
- 创建 vue 的实例对象，并指定要控制的 el 区域
- 通过 render 函数渲染 App 根组件

  ```js
  // 1. 导入 Vue 构造函数
  import Vue from 'vue'
  // 2. 导入 App 根组件
  import App from './components/App.vue'
  const vm = new Vue({
    // 3. 指定 vm 实例要控制的页面区域
    el: '#app',
    // 4. 通过 render 函数，把指定的组件渲染到 el 区域中
    render: h => h(App)
  })
  ```

#### webpack 打包发布

- 上线之前需要通过 webpack 将应用进行整体打包，可以通过 package.json 文件配置打包命令

  ```js
  // 在package.json文件中配置 webpack 打包命令
  // 该命令默认加载项目根目录中的 webpack.config.js 配置文件
  "scripts": {
  // 用于打包的命令
  "build": "webpack -p",
  // 用于开发调试的命令
  "dev": "webpack-dev-server --open --host 127.0.0.1 --port 3000",
  },
  ```

### Vue 脚手架

**Vue 脚手架可以快速生成 Vue 项目基础的架构。**

#### 3.x 版本的 Vue 脚手架

**安装：3.x 版本的 Vue 脚手架：**

- `npm install -g @vue/cli`

**基于 3.x 版本的脚手架创建 Vue 项目：**

1. 使用命令创建 Vue 项目

   - 命令：vue create my-project
   - 选择 Manually select features (选择特性以创建项目)
   - 勾选特性可以用空格进行勾选。
     - Babel 、 Router 、 2Linter / Formatter
   - 是否选用历史模式的路由：n
   - ESLint 选择：ESLint + Standard config
   - 何时进行 ESLint 语法校验：Lint on save
   - babel，postcss 等配置文件如何放置：In dedicated config files+ 独使用文件进行配置)
   - 是否保存为模板：n
   - 使用哪个工具安装包：npm

2. 基于 ui 界面创建 Vue 项目

- `vue ui`
- 在自动打开的创建项目网页中配置项目信息。

#### 2.x 版本的 Vue 脚手架

**安装：2.x 版本的 Vue 脚手架：**

- `npm install -g @vue/cli-init`

**基于 3.x 版本的脚手架创建 Vue 项目：**

- `vue init webpack my-project`

#### 分析 Vue 脚手架生成的项目结构

- node_modules:依赖包目录
- public：静态资源目录
- src：源码目录
- src/assets:资源目录
- src/components：组件目录
- src/views:视图组件目录
- src/App.vue:根组件
- src/main.js:入口 js
- src/router.js:路由 js
- babel.config.js:babel 配置文件
- .eslintrc.js:

#### Vue 脚手架的自定义配置

```js
// 通过 package.json 进行配置 [不推荐使用]
  "vue":{
      "devServer":{
          "port":"9990",
          "open":true
      }
  }
// 通过单独的配置文件进行配置，创建vue.config.js
  module.exports = {
      devServer:{
          port:8888,
          open:true
      }
  }
```

#### Element-UI 的基本使用

**Element-UI：一套为开发者、设计师和产品经理准备的基于 Vue 2.0 的桌面端组件库。**

##### 基于命令行方式手动安装

- 运行 `npm i element-ui –S` 命令
- 导入 `Element-UI` 相关资源

  ```js
  // 导入组件库
  import ElementUI from 'element-ui'
  // 导入组件相关样式
  import 'element-ui/lib/theme-chalk/index.css'
  // 配置 Vue 插件
  Vue.use(ElementUI)
  ```

##### 基于图形化界面自动安装

1. 运行 vue ui 命令，打开图形化界面
2. 通过 Vue 项目管理器，进入具体的项目配置面板
3. 点击 插件 -> 添加插件，进入插件查询面板
4. 搜索 vue-cli-plugin-element 并安装
5. 配置插件，实现按需导入，从而减少打包后项目的体积

### 实例项目技术点：

#### 登录/退出功能

**登录状态保持：**

- 如果服务器和客户端同源，建议可以使用 cookie 或者 session 来保持登录状态
- 如果客户端和服务器跨域了，建议使用 token 进行维持登录状态。

**token 原理：**

![token](http://localhost:9710/blog/vue/token.png)

1. 将登录成功之后的 token ，保存到客户端的 sessionStorage 中
   - 项目中除了 login 之外其他的 API 接口，必须在登录以后才能访问
   - token 只应该在当前网站打开期间生效，所以将 token 保存在 sessionStorage 中
   ```js
   // 将服务端传递的 token 传递给 sessionStorage
   window.sessionStorage.setItem('token', res.data.token)
   ```
2. 通过编程式导航跳转到后台主页
   - `this.$router.push('/home')`

**路由导航守卫控制访问权限：**

- 如果用户没有登录，但是直接通过 URL 访问特定页面，需要重新导航到登录页面。

```js
// 为路由对象，添加 beforeEach 导航守卫
// to 将要访问的路径
// from 代表从哪个路径跳转而来
// next 是一个函数，表示放行
//     next()  放行    next('/login')  强制跳转
router.beforeEach((to, from, next) => {
  // 如果用户访问的登录页，直接放行
  if (to.path === '/login') return next()
  // 从 sessionStorage 中获取到 保存的 token 值
  const tokenStr = window.sessionStorage.getItem('token')
  // 没有token，强制跳转到登录页
  if (!tokenStr) return next('/login')
  next()
})
// 最后在将 router 用 export default 暴露出去
export default router
```

**退出：**

- 基于 token 的方式实现退出比较简单，只需要销毁本地的 token 即可。这样，后续的请求就不会携带 token ，必须重新登录生成一个新的 token 之后才可以访问页面。

```js
// 清空token
window.sessionStorage.clear()
// 跳转到登录页
this.$router.push('/login')
```

#### token 接口授权配置

- 需要授权的 API ，必须在请求头中使用 `Authorization` 字段提供 `token` 令牌

```js
// axios请求拦截
axios.interceptors.request.use(config => {
// 为请求头对象，添加 Token 验证的 Authorization 字段
config.headers.Authorization = window.sessionStorage.getItem('token')
return config
}
```

#### 获取 元素的实例对象（获取 DOM 节点，直接操作 DOM）

获取 DOM 节点：

1. 目标标签中添加 `ref` 属性，自定义名称

   ```html
   <div id="app">
     <input type="button" value="getDOM" @click="getDOM" />
     <h3 ref="content">Lorem ipsum dolor sit amet.</h3>
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

## Vuex

概念：
vuex 是 Vue 配套的 公共数据管理工具，它可以把一些共享的数据，保存到 vuex 中，方便 整个程序中的任何组件直接获取或修改我们的公共数据；

[Vuex](https://vuex.vuejs.org/zh/)

- vuex 是 为了保存组件间共享数据而诞生的，如果组件间有要共享的数据， 可以直接挂载到 vuex 中，不必通过父子组件传值，不需要共享的私有数据不必挂载到 vuex 中 vuex 是 为了保存组件间共享数据而诞生的，如果组件间有要共享的数据， 可以直接挂载到 vuex 中，不必通过父子组件传值，不需要共享的私有数据不必挂载到 vuex 中
  - vuex 是 全局数据共享区域

配置 vuex 的步骤

1. 运行 `cnpm i vuex -S`

2. 导入包 `import Vuex from 'vuex'`
3. 注册`vuex`到`vue`中 `Vue.use(Vuex)`
4. `new Vuex.Store()` 实例，得到一个 数据仓储对象

```js
var store = new Vuex.Store({
  state: {
    // 大家可以把 state 想象成 组件中的 data ,专门用来存储数据的
    // 如果在 组件中，想要访问，store 中的数据，只能通过 this.$store.state.*** 来访问
    count: 0
  },
  mutations: {
    // 注意： 如果要操作 store 中的 state 值，只能通过 调用 mutations 提供的方法，才能操作对应的数据，不推荐直接操作 state 中的数据，因为 万一导致了数据的紊乱，不能快速定位到错误的原因，因为，每个组件都可能有操作数据的方法；
    increment(state) {
      state.count++
    },
    // 注意： 如果组件想要调用 mutations 中的方法，只能使用 this.$store.commit('方法名')
    // 这种 调用 mutations 方法的格式，和 this.$emit('父组件中方法名')
    subtract(state, obj) {
      // 注意： mutations 的 函数参数列表中，最多支持两个参数，其中，参数1： 是 state 状态； 参数2： 通过 commit 提交过来的参数；
      console.log(obj)
      state.count -= obj.c + obj.d
    }
  },
  getters: {
    // 注意：这里的 getters， 只负责 对外提供数据，不负责 修改数据，如果想要修改 state 中的数据，请 去找 mutations
    optCount: function(state) {
      return '当前最新的count值是：' + state.count
    }
    // 经过咱们回顾对比，发现 getters 中的方法， 和组件中的过滤器比较类似，因为 过滤器和 getters 都没有修改原数据， 都是把原数据做了一层包装，提供给了 调用者；
    // 其次， getters 也和 computed 比较像， 只要 state 中的数据发生变化了，那么，如果 getters 正好也引用了这个数据，那么 就会立即触发 getters 的重新求值；
  }
})
```

5. 将 vuex 创建的 store 挂载到 VM 实例上， 只要挂载到了 vm 上，任何组件都能使用 store 来存取数据

总结：

1. state 中的数据，不能直接修改，如果想要修改，必须通过 mutations

2. 如果组件想要直接 从 state 上获取数据： 需要 this.\$store.state.\*\*\*

3. 如果 组件，想要修改数据，必须使用 mutations 提供的方法，需要通过 this.\$store.commit('方法的名称'， 唯一的一个参数)

4. 如果 store 中 state 上的数据， 在对外提供的时候，需要做一层包装，那么 ，推荐使用 getters, 如果需要使用 getters ,则用 this.\$store.getters.\*\*\*

## 相关文档

1. [vue.js 1.x 文档](https://v1-cn.vuejs.org/)
2. [vue.js 2.x 文档](https://cn.vuejs.org/)
3. [String.prototype.padStart(maxLength, fillString)](http://www.css88.com/archives/7715)
4. [js 里面的键盘事件对应的键码](http://www.cnblogs.com/wuhua1/p/6686237.html)
5. [Vue.js 双向绑定的实现原理](http://www.cnblogs.com/kidney/p/6052935.html)
6. [pagekit/vue-resource](https://github.com/pagekit/vue-resource)
7. [navicat 如何导入 sql 文件和导出 sql 文件](https://jingyan.baidu.com/article/a65957f4976aad24e67f9b9b.html)
8. [贝塞尔在线生成器](http://cubic-bezier.com/#.4,-0.3,1,.33)
9. [URL 中的 hash（井号）](http://www.cnblogs.com/joyho/articles/4430148.html)
