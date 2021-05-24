# 补遗 (干货)

- [补遗 (干货)](#补遗-干货)
  - [原型链](#原型链)
    - [`prototype` / `__proto__` / `constructor` 属性](#prototype--__proto__--constructor-属性)
    - [`__proto__` 属性](#__proto__-属性)
    - [`prototype` 属性](#prototype-属性)
    - [`constructor` 属性](#constructor-属性)
  - [冒泡与捕获](#冒泡与捕获)
  - [缓存](#缓存)
  - [异步相关](#异步相关)
    - [微任务、宏任务](#微任务宏任务)
    - [async、await(ES7)](#asyncawaites7)
    - [异步转同步](#异步转同步)
    - [常见的 微、宏任务面试题](#常见的-微宏任务面试题)
  - [内存泄漏](#内存泄漏)
    - [垃圾回收](#垃圾回收)
      - [标记清除](#标记清除)
      - [引用计数](#引用计数)
    - [内存泄漏](#内存泄漏-1)
      - [代码关注点](#代码关注点)
      - [Vue 中的内存泄漏问题](#vue-中的内存泄漏问题)
      - [内存泄漏优化](#内存泄漏优化)
      - [内存泄漏调试](#内存泄漏调试)
  - [element](#element)
    - [message原理](#message原理)
    - [关于加载大量数据的优化不做分页](#关于加载大量数据的优化不做分页)
      - [使用element](#使用element)
      - [使用第三方组件](#使用第三方组件)
  - [admin 框架优化点](#admin-框架优化点)

## 原型链

### `prototype` / `__proto__` / `constructor` 属性

| 类型  | 具有的属性                            | 备注                                                  |
| :---: | :------------------------------------ | :---------------------------------------------------- |
| 对象  | `__proto__` `constructor`             | 对象独有                                              |
| 函数  | `prototype` `__proto__` `constructor` | `prototype` 函数独有 <br> 函数也是对象,具有对象的属性 |

### `__proto__` 属性

> `__proto__` 是对象独有的属性,由一个对象指向一个对象(他的原型对象,父对象)
> 通过 `__proto__` 属性将对象连接起来的这条链路 即 原型链

```javascript
function Foo() {}
let f1 = new Foo()

// 每个实例对象（object）都有一个私有属性（称之为 __proto__ ）指向它的构造函数的原型对象（prototype ）
f1.__proto__ === Foo.prototype
// 该原型对象也有一个自己的原型对象( __proto__ ) ，层层向上直到(Object.prototype.)__proto__属性的终点null，然后返回undefined,再往上找就报错
// ! 几乎所有 JavaScript 中的对象都是位于原型链顶端的Object的实例
// 这就是原型链,原型链是通过 __proto__ 进行连接.
Foo.prototype.__proto__ === Object.prototype
```

### `prototype` 属性

> `prototype` 是函数独有的属性,由一个函数指向一个对象(原型对象)
> `prototype` 属性 可以理解为 构造函数的公有属性,当构造函数被实例化后,实例化对象的原型链指向 Foo.prototype
> 作用 让函数 所 实例化的对象找到公有的属性和方法 即 `f1.__proto__ === Foo.prototype`

### `constructor` 属性

> `constructor` 是对象独有的属性,由一个对象指向一个函数(该对象的构造函数),并且存在于 prototype 上.
> 所有函数（看成对象）最终的构造函数都指向Function

- 在例子中,f1本身不具有 constructor 属性
- 通过 `__proto__` 属性到原型链中找,又由于`f1.__proto__ === Foo.prototype`
- Foo.prototype 具有 constructor 属性 指向 Foo
- 即 `(f1.__proto__.constructor===)f1.constructor === Foo` (不是本身拥有的,是继承而来的)

## 冒泡与捕获

- 前提：`element.addEventListener(event, function, useCapture)`
  - useCapture:可选，布尔值，指定事件是否在捕获或冒泡阶段执行。
    - 默认值：false，事件句柄在冒泡阶段执行
    - true，事件句柄在捕获阶段执行

- 执行顺序：先捕获，后冒泡，捕获从上到下，冒泡从下到上
  - 即：捕获像石头沉入海底，冒泡则像气泡冒出水面

```javascript
<div>
  <button>123</button>
</div>
<script>
  var btn = document.querySelector('button');
  var div = document.querySelector('div');
  btn.addEventListener('click', function () {
    console.log('bubble', 'btn');
  }, false);
  btn.addEventListener('click', function () {
    console.log('capture', 'btn');
  }, true);
  div.addEventListener('click', function () {
    console.log('bubble', 'div');
  }, false);
  div.addEventListener('click', function () {
    console.log('capture', 'div');
  }, true);
</script>

// capture div
// capture btn
// bubble btn
// bubble div
```

## 缓存

- cookie localStorage sessionStorage indexDB 的区别

| 特性         | cookie                                                                          | localStorage                                | sessionStorage               |
| :----------- | :------------------------------------------------------------------------------ | :------------------------------------------ | :--------------------------- |
| 数据生命周期 | 一般由服务器生产,可以设置过期时间<br>过期时间之前一直有效，即使窗口或浏览器关闭 | 硬盘存储持久数据<br>除非被清理,否则一直存在 | 存在内存中<br>页面关闭就清理 |
| 数据存储大小 | 4K                                                                              | 5M                                          | 5M                           |
| 与服务端通信 | 每次都会携带在header中,对请求性能影响                                           | 不参与                                      | 不参与                       |

## 异步相关

> 内部执行顺序同步执行，由异步方法，与微任务、宏任务执行方式一样，会在等待队列中做相应的优先处理

Promise.all() 方法用于将多个Promise实例，包装成一个新的Promise实例。

- 接受一个数组作为参数，数组内的成员 需要是 Promise对象的实例
- 所有成员状态都为 成功，返回 所有成员的返回值组成的一个数组（同步），传递给该实例的回调函数
- 若有一个成员被 rejected，则返回 第一个被reject的实例的返回值，传递给该实例的回调函数

### 微任务、宏任务

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

### async、await(ES7)

- 提交成功后，立即渲染页面时，不要分在两个函数中写，有异步的问题

- 带有 async 关键字的函数，返回的是一个promise，即成功 、 失败的结果
- await 关键字 所等待的是 返回结果，函数内代码依旧执行

### 异步转同步

- 使用 promise 结合 async await 实现。

```javascript
function async() {
  setTimeout(() => {
    console.log(1)
  }, 0);
  console.log(2);
}
async() // 执行顺序：2 => 1

// ---- 转同步 即 修改代码 后执行顺序改为 1 => 2

function sync() {
  const async = () => {
    return new Promise(resolve => {
      setTimeout(() => {
        console.log(1)
        resolve(1)
      }, 0)
    })
  }

  const sync2 = async () => {
    await async()
    console.log(2)
  }

  sync2()
}
sync() // 执行顺序：1 => 2
```

### 常见的 微、宏任务面试题

```javascript
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

## 内存泄漏

### 垃圾回收

> 浏览器 javascript 具有自动垃圾回收机制 (GC)
> 原理:垃圾回收器会定期(周期性) 找出那些不在继续使用的变量,然后释放其内存

- 实现方式: 引用计数 标记清除

#### 标记清除

> js中最常用的垃圾回收方式就是标记清除。当变量进入环境时，例如，在函数中声明一个变量，就将这个变量标记为“进入环境”。从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流进入相应的环境，就可能会用到它们。而当变量离开环境时，则将其标记为“离开环境”。

```javascript
function test(){
  // 在函数中生命变量 即 进入环境
  const a = 10 // 被标记为 进入环境
  const b = 20 // 被标记为 进入环境
}
test() // 执行完毕后 a\b 被标记 离开环境 ,被回收
```

#### 引用计数

> 跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型值赋给该变量时，则这个值的引用次数就是 1。如果同一个值又被赋给另一个变量，则该值的引用次数加 1。相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数减 1。当这个值的引用次数变成 0 时，则说明没有办法再访问这个值了，因而就可以将其占用的内存空间回收回来。这样，当垃圾回收器下次再运行时，它就会释放那些引用次数为 0 的值所占用的内存。

```javascript
function test() {
  var a = {}; // a指向对象的引用次数为1
  var b = a; // a指向对象的引用次数加1，为2
  var c = a; // a指向对象的引用次数再加1，为3
  var b = {}; // a指向对象的引用次数减1，为2
}
```

### 内存泄漏

> 不再用到的内存，没有及时释放，就叫做内存泄漏

#### 代码关注点

1. DOM 中的 addEventLisner 函数及派生的事件监听，比如 Jquery 中的 on 函数，Vue 组件实例的 $on 函数

2. 其它 BOM 对象的事件监听， 比如 websocket 实例的 on 函数

3. 避免不必要的函数引用

4. 如果使用 render 函数，避免在 HTML 标签中绑定 DOM/BOM 事件

5. DOM泄露

   - 当原有的COM被移除时，子结点引用没有被移除则无法回收。

    ```javascript
    var select = document.querySelector;
    var treeRef = select('#tree');
    
    //在COM树中leafRef是treeFre的一个子结点
    var leafRef = select('#leaf'); 
    var body = select('body');
    
    body.removeChild(treeRef);
    
    //#tree不能被回收入，因为treeRef还在
    //解决方法:
    treeRef = null;
    //tree还不能被回收，因为叶子结果leafRef还在
    leafRef = null;
    //现在#tree可以被释放了。
    ```

#### Vue 中的内存泄漏问题

1. 如果在 mounted/created 钩子中使用 JS 绑定了DOM/BOM对象中的事件，需要在 beforeDestroy中做对应解绑处理；
2. 如果在 mounted/created 钩子中使用了第三方库初始化，需要在 beforeDestroy中做对应销毁处理（一般用不到，因为很多时候都是直接全局 Vue.use）；
3. 如果组件中使用了 setInterval，需要在 beforeDestroy中做对应销毁处理；
4. [在线案例](https://blog.csdn.net/weixin_34381687/article/details/87940958?utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control&dist_request_id=1331996.9503.16189053256708211&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control)

```javascript
mounted() {
    const box = document.getElementById('time-line')
    this.width = box.offsetWidth
    this.resizefun = () => {
      this.width = box.offsetWidth
    }
    window.addEventListener('resize', this.resizefun)
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.resizefun)
    // 解绑(手动清除)
    this.resizefun = null
  }
```

#### 内存泄漏优化

> 确保占用最少的内存可以让页面获得更好的性能。而优化内存占用的最佳方式，就是为执行中的代码只保存必要的数据.

1. 解除引用
   - 一旦数据不再有用，最好通过将其值设置为 null 来释放其引用——这个做法叫做解除引用
   - 解除一个值的引用并不意味着自动回收该值所占用的内存。解除引用的真正作用是让值脱离执行环境，以便垃圾收集器下次运行时将其回收。
2. **在业务不需要用到的内部函数，可以重构在函数外，实现解除闭包**
3. **注意清除定时器和事件监听器**
4. 避免创建过多生命周期较长的对象，或将对象分解成多个子对象
5. 避免过多使用闭包

ps. 关注点:闭包,DOM\BOM操作,定时器\时间监听,以及循环组件时且数据量庞大可能造成内存泄漏

#### 内存泄漏调试

- chrome Memory 面板 进行快照调试。

## element

### message原理

- 事先准备好的 提示框组件 `zfMsg.vue`

```vue
<template>
  <div>
    <ul>
      <li v-for="(item, index) in messages" :key="index" class="message">
        {{ item.message }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      messages: [],
      id: 0
    }
  },
  methods: {
    add(options) {
      const layer = {
        id: this.id++,
        ...options
      }
      layer.timer = setTimeout(() => {
        this.remove(layer)
        this.id = 0
      }, 2000)
      this.messages.push(layer)
      console.log(layer.id)
    },
    remove(layer) {
      this.messages = this.messages.filter(item => item.id !== layer.id)
    }
  }
}
</script>

<style>
.message {
  position: fixed;
  left: 50%;
  top: 30px;
  transform: translate3d(-50%, 0, 0);
  background: #000;
  background: #f0f9eb;
  color: #67c23a;
  padding: 10px 20px;
  border-radius: 4px;
  animation: move 0.3s;
}

@keyframes move {
  0% {
    top: 0;
    opacity: 0;
  }
  100% {
    top: 30px;
    opacity: 1;
  }
}
</style>

```

- 使用 类 的方式将组件进行挂载

```javascript
import Vue from 'vue'
import zfMsg from './zfMsg.vue'

class Msg {
  constructor() {
    const vm = new Vue({
      render: h => h(zfMsg)
    }).$mount() // 手动挂载实例
    // 将 vue 实例的根DOM元素 挂载至body尾部
    document.body.appendChild(vm.$el)
    // 获取实例下的子组件
    this.component = vm.$children[0]
  }

  success(options) {
    this.component.add(options)
  }
}

Msg.getInstance = (function() {
  let instance
  return function() {
    if (!instance) {
      instance = new Msg()
    }
    return instance
  }
})()

export const Message = Msg.getInstance()

```

- 使用

```vue
<template>
  <el-button @click="handleMsg">ddd</el-button>
</template>

<script>
import { Message } from '@/components/zfMsg/index.js'
export default {
  methods: {
    handleMsg() {
      Message.success({ message: '111' })
    }
  }
}
</script>
```

### 关于加载大量数据的优化不做分页

#### 使用element

- 使用 InfiniteScroll 无线滚动组件
- 先将 海量数据做 存储，然后捞出显示区域内的数据，触底调用函数，再将部分数据捞出显示。
- 使用的数组操作，截取 slice，合并 concat，删除 splice

#### 使用第三方组件

> 第三方组件很多，以 vue-virtual-scroll-list 举例

- `vue-virtual-scroll-list`
  - `https://github.com/tangbc/vue-virtual-scroll-list`
  - 组件参数：
    - data-key (type: string)：每个数据对象中获取唯一键，键名（id）
    - data-sources (type: Array)：列表数据：每一行都必须有一个唯一的值（id）
    - data-component (type: Component)：每一行的子组件
    - keeps(type: number)：默认30个，默认渲染的个数
  - 子组件传递参数
    - index (type: number)：每行索引
    - source (type: object)：每行内容

    ```vue
    <template>
      <div>
        <virtual-list style="height: 360px; overflow-y: auto;"
          :data-key="'id'"
          :data-sources="items"
          :data-component="itemComponent"
        />
      </div>
    </template>
    
    <script>
      import Item from './Item'
      import VirtualList from 'vue-virtual-scroll-list'
    function createData(len) {
        const arr = []
        for (let index = 0; index < len; index++) {
            const obj = { id: index, text: Math.random() }
            arr.push(obj)
        }
        return arr
    }
      export default {
        name: 'root',
        data () {
          return {
            itemComponent: Item,
            items: createData(200)
          }
        },
        components: { 'virtual-list': VirtualList }
      }
    </script>

    <!--  Item    -->
    <template>
      <div>每一行的内容</div>
    </template>
    
    <script>
      export default {
        name: 'item-component',
        props: {
          index: { // 每一行的索引
            type: Number
          },
          source: { // 每一行的内容
            type: Object,
            default () {
              return {}
            }
          }
        }
      }
    </script>
    ```

- `vue-virtual-scroller`
  - `https://github.com/Akryum/vue-virtual-scroller#recyclescroller`
  - items：呈现数据
  - item-size：呈现数据的个数
  - key-filed：如果 items 是对象，需要用这个做唯一标识

    ```vue
    <template>
      <RecycleScroller
        style="height: 200px; overflow: auto"
        class="scroller"
        :items="listItem"
        :item-size="20"
        key-field="data"
      >
        <template v-slot="{ item }">
          <el-checkbox :key="item.data" :label="item.label" />
        </template>
      </RecycleScroller>
    </template>

    <script>
    export default {
      data() {
            return {
                listItem: []
            }
        }
    }
    </script>
    ```

## admin 框架优化点

- 使用动态路由的形式做路由添加（addRoutes）

```javascript
const router = new VueRouter([
  {
    path: "/",
    name: "Home",
    component: Home
  }
]);
let route=[
{
  path: '/pageA',
  name: 'pageA',
  component: pageA,
}]
router.addRoutes(route);
export default router
```
