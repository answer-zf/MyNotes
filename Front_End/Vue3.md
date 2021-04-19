# 总结

## MVVM

-   M(数据模型) 通过响应的方式 进行数据绑定到 V(视图)中
-   在 V 中，用户通过事件反作用事件修改 M
-   通过 vue这个中间层(VM) 做桥梁，避免用户直接接触 DOM

## 生命周期

-   示例：异步获取列表数据

```js
    // 模拟异步数据调用
    function getCourses() {
        return new Promise(resolve => {
            setTimeout(() => {
                resolve(['zf', 'zf'])
            }, 2000);
        })
    }
    const app = new Vue({
        // created钩子中调用接口
        async created() {
            // 组件实例已创建，由于为挂载，DOM 不存在
            const courses = await getCourses()
            this.courses = courses
        }
    })
```

-   生命周期演示

```html
<body>
    <div id="demo">
        <h1>初始化流程</h1>
        <p>{{foo}}</p>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script>
    <script>
        // 创建实例
        const app = new Vue({
            el: '#demo',
            data: {
                foo: 'foo'
            },
            beforeCreate() { console.log('beforeCreate') },
            created() { console.log('created ' + this.$el) },
            beforeMount() { console.log('beforeMount') },
            mounted() {
                setTimeout(() => {
                    this.foo = 'foooooo'
                }, 2000);
                console.log('mounted ' + this.$el)
            },
            beforeUpdate() { console.log('beforeUpdate') },
            updated() { console.log('updated') },
        });
    </script>
</body>
```

-   场景分析

```javascript
{
    beforeCreate(){} // 执行时组件实例还未创建，通常用于插件开发中执行一些初始化任务
    created(){} // 组件初始化完毕，各种数据可以使用，常用于异步数据获取
    beforeMount(){} // 未执行渲染、更新，dom未创建
    mounted(){} // 初始化结束，dom已创建，可用于获取访问数据和dom元素
    beforeUpdate(){} // 更新前，可用于获取更新前各种状态
    updated(){} // 更新后，所有状态已是最新
    beforeDestroy(){} // 销毁前，可用于一些定时器或订阅的取消
    destroyed(){} // 组件已销毁，作用同上
}
```

## 组件

### 父组件向子组件传参
    
    -   通过属性传参
    -   子组件通过props接收

```javascript
    // html: <course-list :courses="courses"></course-list>
    Vue.component('course-list', {
        data() {
            return {
                selectCourse: '',
            }
        },
        props: {
            courses: {
                type: Array,
                default: []
            }
        },
        template: `
        <div>
            <h2 v-bind:title="title">{{title}}</h2>
            <p v-if="courses.length == 0">none</p>
            <div v-for="c in courses" :key="c.id" :style="{backgroundColor: selectCourse === c ? '#ddd':'transparent'}">
                <div>{{ c }}</div>
            </div>
        </div>
        `
    })

    const app = new Vue({
        el: '#app',
        data() {
            return {
                title: 'value',
                course: '',
                courses: [],
                totalcount: 0
            }
        },
    })
```

### 子组件向父组件传参(`$emit` 自定义事件)

    -   子组件 通过 $emit 自定义事件（第二个参数开始 传参） 并触发
    -   父组件 监听 $emit 自定义事件

> 当子组件需要和父级组件进行通信，可以派发并监听自定义事件。

```javascript
    // html: <course-add @add-course="addCourse"></course-add>
    Vue.component('course-add', {
        data() {
            return {
                course: ''
            }
        },
        template: `
        <div>
            <input type="text" @keydown.enter="addCourse" v-model="course">
        </div>
        `,
        methods: {
            addCourse() {
                // 派发事件 通知父组件做新增（事件命名使用 - 不使用 驼峰）
                this.$emit('add-course', this.course)
                this.course = ''
            },
        },
    })

    const app = new Vue({
        el: '#app',
        data() {
            return {
                courses: [],
            }
        },
        methods: {
            addCourse(course) {
                this.courses.push(course)
            }
        }
    })
```

### 在组件上使用 v-model （双向绑定）

> 将数据存储在 父组件 内部 维护，子组件负责修改

#### 前提 v-model 的实现

> v-model 即 语法糖

v-model="value_"

1.  @input="value_=$event" $event 即用户输入的值
2.  :value="value_"

#### 组件实现

```javascript
    // <course-add @add-course="addCourse" v-model="course"></course-add>
    Vue.component('course-add', {
        // 接收父组件传递value，不需要额外维护course
        props: ['value'],
        template: `
        <div>
            <input type="text" @keydown.enter="addCourse" :value='value' @input="onInput">
        </div>
        `,
        methods: {
            addCourse() {
                // 派发事件 通知父组件做新增
                // 派发事件不再需要传递数据
                this.$emit('add-course')
            },
            onInput(e) {
                this.$emit('input', e.target.value)
            }
        },
    })

    const app = new Vue({
    el: '#app',
    data() {
        return {
            course: '',
            courses: []
        }
    }
    methods: {
        addCourse() {
            this.courses.push(this.course)
            this.course = ''
        }
    }
})
```

### 内容分发 （插槽）

> 通过使用vue提供的 <slot> 元素可以给组件传递内容

```javascript
// 方法1. <message :show="show" @close="show=$event">xxxxxxxxxxx</message>
// 方法2. 使用 sync 修饰符，默认自定义事件 @update:value_="value_ = $event" 简化操作，没有特殊操作省略
//    <message :show.sync="show">xxxxxxxxxxx</message>

Vue.component('message', {
    props: ['show'],
    template: `
        <div class="message-box" v-if="show">
            <!-- 通过 slot 插槽获取 传入内容 -->
            <slot></slot>
            // 方法1 
            <span class="message-box-close" @click="$emit('close',false)">x</span>
            // 方法2 
            <span class="message-box-close" @click="$emit('update:show',false)">x</span>
        </div>
    `
})

const app = new Vue({
    el: '#app',
    data() {
        return {
            course: '',
            courses: [],
            show: false,
        }
    },
    methods: {
        addCourse() {
            this.courses.push(this.course)
            this.course = ''
            this.show = true
        }
    }
})

```

#### 具名插槽

```html
<message :show.sync="show">
    <!-- 命名为 title 插槽内容 -->
    <template v-slot:title>
        <h2>。。。。。。。。</h2>
    </template>
    <!-- 默认插槽内容 v-slot:default 可省略 -->
    <template v-slot:default>
        default slot content
    </template>
</message>

<script>
    Vue.component('message', {
        props: ['show'],
        template: `
            <div class="message-box" v-if="show">
                <!-- 具名插槽 没有提供内容显示 默认内容-->
                <slot name="title"> 默认内容 </slot>
                <!-- 通过 slot 插槽获取 传入内容 -->
                <slot></slot>
                <span class="message-box-close" @click="$emit('update:show',false)">x</span>
            </div>
        `
    })
    //...
</script>
```

#### 作用域插槽

> 插槽内容 都是由 父组件 提供 ，想要在插槽使用子组件的内容

```html
    <message :show.sync="show">
        <!-- 命名为 title 插槽内容 -->
        <template v-slot:title="slotProps">
            <h2>{{ slotProps.title }}</h2>
        </template>
        <!-- 默认插槽内容 v-slot:default 可省略 -->
        <template v-slot:default>
            default slot content
        </template>
    </message>
    <script>
        Vue.component('message', {
            props: ['show'],
            template: `
                <div class="message-box" v-if="show">
                    <!-- 具名插槽 -->
                    <slot name="title" title="from message component content"> 默认内容 </slot>
                    <!-- 通过 slot 插槽获取 传入内容 -->
                    <slot></slot>
                    <span class="message-box-close" @click="$emit('update:show',false)">x</span>
                </div>
            `
        })
    </script>
```

### 组件化

> 组件是可复用的 Vue 实例，组件是VueComponent的实例，继承自Vue
> 软件工程中原则的践行，高内聚，低耦合，增强程序的复用性、可维护性和可测试性

-   使用场景

    -   通用组件：实现最基本的功能，具有通用性、复用性，例如按钮组件、输入框组件、布局组件等
    -   业务组件：它们完成具体业务，具有一定的复用性，例如登录组件、轮播图组件。
    -   页面组件：组织应用各部分独立内容，需要时在不同页面组件间切换，例如列表页、详情页组件


-   本质
    -   vue中的组件经历如下过程
        -   组件配置 => VueComponent实例 => render() => Virtual DOM=> DOM
    -   组件的本质是产生虚拟DOM

## API

### 数据相关API

#### Vue.set / Vue.delete

> 向响应式对象中 添加/删除 一个属性，并确保这个新属性同样是响应式的，且触发视图更新。

-   `Vue.set(target, propertyName/index, value)`

    -   target: 需要修改的对象
    -   propertyName/index : 对象的key
    -   value: 值

-   `Vue.delete(target, propertyName/index)`

```javascript
// <input type="text" v-model.number="price" @keydown.enter="batchUpdate">
const app = new Vue({
    el: '#app',
    data() {
        return {
            course: '',
            courses: [{ name: 'zf' }, { name: 'zf' }],
            price: 0
        }
    },
    methods: {
        batchUpdate() {
            this.courses.forEach(c => {
                // c.price = this.price  数据已改，界面没有响应式更新。
                // Vue.set(c, 'price', this.price) 
                this.$set(c, 'price', this.price) // 也可以使用 同名实例方法  Vue.delete 也同理 this.$delete()
            });
        }
    }
});
```

### 事件相关API

#### vm.$on 

> 监听当前实例上的自定义事件。事件可以由 vm.$emit 触发。回调函数会接收所有传入事件触发函数的额外参数。

```javascript
// 即 模板中的 @click ... 的 编程写法
vm.$on('test', function (msg) { // @test=" .. "
    console.log(msg) 
})
```

#### vm.$emit

> 触发当前实例上的事件。附加参数都会传给监听器回调。

#### 事件总线

> 通过在Vue原型上添加一个Vue实例作为事件总线，实现组件间相互通信，而且不受组件间关系的影响

`Vue.prototype.$bus = new Vue()`

> 这样做可以在任意组件中使用 this.$bus 访问到该Vue实例

```html
<!--给之前新增成功消息添加.success-->
<message :show.sync="show" class="success">...</message>
<!--新增警告提示窗-->
<message :show.sync="showWarn" class="warning">...</message>
```

```javascript
const app = new Vue({
    data() {
        return {
            // 控制警告信息显示状态
            showWarn: false,
        }       
    },
    methods: {
        addCourse() {
            // 增加输入校验
            if (this.course) {
                // ...  
                this.show = true
            } else {
            // 提示警告信息
                this.showWarn = true
            }
        }
    },
})
```

**功能实现**

```javascript
// 弹窗组件
Vue.component('message', {
    // ...
    // 监听关闭事件
    props: ['show'],
    template: `
        <div class="message-box" v-if="show">
            <!-- 具名插槽 -->
            <slot name="title" title="from message component content"> 默认内容 </slot>
            <!-- 通过 slot 插槽获取 传入内容 -->
            <slot></slot>
            <span class="message-box-close" @click="$emit('update:show',false)">x</span>
        </div>
    `,
    mounted () {
        this.$bus.$on('message-close', () => {
            this.$emit('update:show', false)  // 等价于 批处理 click 事件
        });
    },
})
```

```html
<!-- 派发关闭事件 -->
<div class="toolbar">
    <button @click="$bus.$emit('message-close')">清空提示框</button>
</div>
```

#### vm.$once

> 监听一个自定义事件，但是只触发一次。一旦触发之后，监听器就会被移除。 -> 与 $on 唯一的区别

#### vm.$off

-   移除自定义事件监听器。
    -   如果没有提供参数，则移除所有的事件监听器；
    -   如果只提供了事件，则移除该事件所有的监听器；
    -   如果同时提供了事件与回调，则只移除这个回调的监听器。

### 组件或元素引用

#### ref和vm.$refs（操作 DOM）

-   ref 被用来给元素或子组件注册引用信息。
-   引用信息将会注册在父组件的 $refs 对象上。
-   如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素（直接操作 DOM）
-   如果用在子组件上，引用就指向组件实例

```html
<input type="text" ... ref="inp">
<script>
    mounted(){
        // 设置输入框焦点
        this.$refs.inp.focus()
    }
</script>
```

**注意：**

-   ref 是作为渲染结果被创建的，在初始渲染时不能访问它们
-   $refs 不是响应式的，不要试图用它在模板中做数据绑定
-   当 v-for 用于元素或组件时，引用信息将是包含 DOM 节点或组件实例的数组。

## 动画

> transition组件会为嵌套元素 自动添加class，可用于做css过度动画

```html
<style>
/* 定义过度动画 */
.fade-enter-active,
.fade-leave-active {
    transition: opacity .5s;
}
.fade-enter,
.fade-leave-to {
    opacity: 0;
}
</style>
<script>
Vue.component('message', {
    // 使用transition组件应用过度动画
    template: `
    <transition name="fade">
    ...
    </transition>
    `,
})
</script>
```

1.  v-enter ：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。
2.  v-enter-active ：定义进入过渡生效时的状态。在元素被插入之前生效，在过渡/动画完成之后移除.
3.  v-enter-to : 定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter 被移除)，在过渡/动画完成之后移除。
    -   `.fade-enter-to { opacity: 1; }`
4.  v-leave : 定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。
    -   `.fade-leave { opacity: 1; }`
5.  v-leave-active ：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
6.  v-leave-to : 定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave 被删除)，在过渡/动画完成之后移除。

### 使用CSS动画库

> 通过自定义过度类名可以有效结合Animate.css这类动画库制作更精美的动画效果。

[自定义过度](https://cn.vuejs.org/v2/guide/transitions.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BF%87%E6%B8%A1%E7%9A%84%E7%B1%BB%E5%90%8D)

-   引用
    -   `<link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">`

-   使用
    -   `<transition enter-active-class="animated bounceIn" leave-active-class="animated bounceOut">`

### JavaScript 钩子

-   可以在<transition>属性中声明 JavaScript 钩子，使用JS实现动画。

```html
<script
src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js">
</script>
<script>
    Vue.component('message', {
        template: `
        <transition name="fade"
            :css="false" // 禁用css
            @before-enter="beforeEnter"
            @enter="enter"
            @before-leave="beforeLeave"
            @leave="leave">
        </transition>
        `,
        methods: {
            beforeEnter(el) {
                el.style.opacity = 0
            },
            enter(el, done) {
                Velocity(el, { opacity: 1 }, { duration: 500, complete: done })
            },
            beforeLeave(el) {
                el.style.opacity = 1
            },
            leave(el, done) {
                Velocity(el, { opacity: 0 }, { duration: 500, complete: done })
            }
        },
    })
</script>
```

## 可复用

### 过滤器

> Vue.js 允许你自定义过滤器，可被用于一些常见的文本格式化。过滤器可以用在两个地方：双花括号插 值和 v-bind 表达式 (后者从 2.1.0+ 开始支持)。过滤器应该被添加在 JavaScript 表达式的尾部，由“管 道”符号指示

```javascript
// {{ c.price | currency('RMB') }}
// 全局过滤器
Vue.filter('currency',function (value) {
    return value
})
// 局部过滤器
filters: {
    currency(value, symbol = '￥') {
        return symbol + value;
    }
}
```

### 自定义指令

> 对普通 DOM 元素进行底层操作，用 自定义指令

-  按钮权限控制
```javascript
// <button v-permission="'admin'"></button>
const role = 'user'
Vue.directive('permission', {
    inserted(el, binding) {
        if (role !== binding.value) {
            el.parentElement.removeChild(el)
        }
    }
})
```

### 渲染函数

> Vue 推荐使用模板创建 HTML。然而需要 JavaScript 的 完成编程的能力，用渲染函数

```javascript
render: function (createElement) {
    // createElement函数返回结果是VNode
    return createElement(
        tag, // 标签名称
        data, // 传递数据
        children // 子节点数组
    )
}
```

-   使用实例

```html
<heading level="1" :title="title">
    {{title}}
</heading>
```

```javascript
Vue.component('heading', {
    props: {
        level: {
            type: String,
            required: true
        }
    },
    // 返回 createElement 的 VNODE
    render(h) {
        console.log(this.$slots.default);
        return h(
            'h' + this.level, // tagname 即标签名
            // 动态添加的属性，不处理，vue默认行为添加到根节点上
            this.$slots.default
        )
    }
})
```

#### 虚拟DOM

> Vue 通过建立一个虚拟 DOM 来追踪自己要如何改变真实 DOM。

```javascript
const vnode = h(
    'h' + level,
    { attrs: { title: this.title } }, // 之前省略了title的处理
    this.$slots.default
)
console.log(vnode);
```

#### createElement 参数

[createElement-参数](https://cn.vuejs.org/v2/guide/render-function.html#createElement-%E5%8F%82%E6%95%B0)

[深入数据对象](https://cn.vuejs.org/v2/guide/render-function.html#%E6%B7%B1%E5%85%A5%E6%95%B0%E6%8D%AE%E5%AF%B9%E8%B1%A1)


-   示例

```html
<!-- 渲染的目标结果 <h2> <svg>
        <use xlink:href="#icon-CART"></use>
    </svg> </h2> -->
<heading :title="title" level="1" icon="CART">
    {{title}}
</heading>
```

```javascript
Vue.component('heading', {
    props: {
        level: { type: String, required: true },
        title: { type: String, default: '' },
        icon: { type: String }
    },
    render(h) {
        // 子节点数组
        let children = []
        if (this.icon) {
            children.push(h(
                // 标签
                'svg',
                // 数据
                { class: 'icon' },
                // 子节点
                [h( 'use', { attrs: { 'xlink:href': '#icon-' + this.icon } } )]
            ))
        }
        children = children.concat(this.$slots.default)
        const vnode = h(
            'h' + this.level, // tagname 即标签名
            // 动态添加的属性，不处理，vue默认行为添加到根节点上
            { attrs: { title: this.title } },
            children
        )
        return vnode
    }
})
```

#### 函数式组件

> 组件没有管理任何状态，也没有监听任何传递给它的状态，也没有生命周期方法时，可以将组件标记为 functional ，这意味它无状态 (没有响应式数据)，也没有实例 (没有 this 上下文)。

-   优化上述示例

```javascript
Vue.component('heading', {
    functional: true, // 函数式组件
    props: { level: { type: String, required: true }, title: { type: String, default: '' }, icon: { type: String } },
    render(h, context) {
        // 子节点数组
        let children = []
        // 属性获取
        const { icon, title, level } = context.props
        if (icon) {
            children.push(h(
                // 标签
                'svg',
                // 数据
                { class: 'icon' },
                // 子节点
                [h( 'use', { attrs: { 'xlink:href': '#icon-' + icon } } )]
            ))
        }

        children = children.concat(context.children)

        const vnode = h(
            'h' + level, // tagname 即标签名
            // 动态添加的属性，不处理，vue默认行为添加到根节点上
            { attrs: { title: title } },
            children
        )
        return vnode
    }
})
```

### 混入

**组件逻辑复用**

[混入](https://cn.vuejs.org/v2/guide/mixins.html#ad)

### 插件

[插件](https://cn.vuejs.org/v2/guide/plugins.html#ad)

-   示例：重构 heading 使其变成插件

```javascript
// 在使用的地方引入即可
// <script src="plugins/heading.js"></script>
// 声明插件（创建） /plugins/heading.js
const myPlugin = {
    install(Vue, options) {
        Vue.component('heading', {
            functional: true, // 函数式组件
            props: {
                level: {
                    type: String,
                    required: true,
                },
                title: {
                    type: String,
                    default: '',
                },
                icon: {
                    type: String,
                },
            },
            render(h, context) {
                // 子节点数组
                console.log(context)

                let children = []
                // 属性获取
                const { icon, title, level } = context.props
                if (icon) {
                    children.push(
                        h(
                            // 标签
                            'svg',
                            // 数据
                            { class: 'icon' },
                            // 子节点
                            [
                                h('use', {
                                    attrs: {
                                        'xlink:href': '#icon-' + icon,
                                    },
                                }),
                            ]
                        )
                    )
                }

                children = children.concat(context.children)

                const vnode = h(
                    'h' + level, // tagname 即标签名
                    // 动态添加的属性，不处理，vue默认行为添加到根节点上
                    { attrs: { title: title } },
                    children
                )
                return vnode
            },
        })
    },
}
if (typeof window !== 'undefined' && window.Vue) {
    // 使用插件
    window.Vue.use
}

```

## vue-cli

### 快速原型开发

-   安装：`npm install -g @vue/cli-service-global`
-   使用：vue serve filename.vue

```bash
# 1. ~/.vuerc
{
  "useTaobaoRegistry": true,
  "packageManager": "npm"
}

```

### 创建项目

-   vue create project_name

### 添加插件

-   vue add router

### 资源路径

> 当你在 JavaScript、CSS 或 *.vue 文件中使用相对路径 (必须以 . 开头) 引用一个静态资源时，该资源将被webpack处理。

### 转换规则

-   如果 URL 是一个绝对路径 ，它将会被保留不变(存放在 pulic 文件夹下，webpack 不会处理)
    
    -   文件存放在 public 下，webpack 不会处理，即 public 就是根目录
    -   `<img alt="Vue logo" src="/assets/logo.png">`

-   如果 URL 以 @ 开头, Vue CLI 默认会设置一个指向 src 的别名 @ 。即根目录是 src
    -   `import CourseList from '@/components/CourseList.vue'`

#### 何时使用 public 文件夹

-   通过 webpack 的处理的好处（使用相对路径）：
    1.  脚本和样式表会被压缩且打包在一起，从而避免额外的网络请求。
    2.  文件丢失会直接在编译时报错，而不是到了用户端才产生 404 错误。
    3.  最终生成的文件名包含了内容哈希，因此你不必担心浏览器会缓存它们的老版本。

-   使用 public 文件夹的好处（使用绝对路径）
    1.  需要在构建输出中指定一个固定的文件名字。
    2.  有上千个图片，需要动态引用它们的路径。
    3.  有些库可能和 webpack 不兼容，除了将其用一个独立的 `<script>` 标签引入没有别的选择。


#### 使用public文件夹的注意事项

-   如果应用没有部署在域名的根部时，需要为 URL 配置 publicPath 前缀

```javascript
// vue.config.js
module.exports = {
    publicPath: process.env.NODE_ENV === 'production'
        ? '/cart/'
        : '/'
}
```

-   在 public/index.html 等通过 html-webpack-plugin 用作模板的 HTML 文件中，需要通过 <%= BASE_URL %> 设置链接前缀：

    -   `<link rel="icon" href="<%= BASE_URL %>favicon.ico">`

-   在模板中，先向组件传入BASE_URL：

```javascript
data () {
    return {
        publicPath: process.env.BASE_URL
    }
}
```

-   然后在 组件模板（template）中使用：

```html
<img :src="`${publicPath}my-image.png`">
```

#### css

-   如果创建项目时没有选择需要的预处理器（Sass/Less/Stylus），则需手动安装相应loader

```bash
# Sass
npm install -D sass-loader node-sass
# Less
npm install -D less-loader less
# Stylus
npm install -D stylus-loader stylus
```

```html
<style scoped lang="scss">
    $color: #42b983;
    a {
        color: $color;
    }
</style>
```

#### 自动化导入样式

> 自动化导入样式文件 (用于颜色、变量、mixin等)，可以使用 style-resources-loader。

-   安装

```bash
npm i -D style-resources-loader
```

-   配置

```javascript
// 1.  创建 @styles/imports.scss 文件，在该文件 预定义需要导入的内容
// 2.  配置 vue.config.js 文件
//       即 在使用之初，先将imports.scss 文件载入，故在所有文件中可以直接使用预定义的变量、定义..
const path = require('path')
function addStyleResource(rule) {
    rule.use('style-resource')
        .loader('style-resources-loader')
        .options({
            patterns: [path.resolve(__dirname, './src/styles/imports.scss')],
        })
}
module.exports = {
    chainWebpack: (config) => {
        const types = ['vue-modules', 'vue', 'normal-modules', 'normal']
        types.forEach((type) =>
            addStyleResource(config.module.rule('scss').oneOf(type))
        )
    },
}
```

#### Scoped CSS

> 当 `<style>` 标签有 scoped 属性时，它的 CSS 只作用于当前组件中的元素。

-   原理

```html
<style scoped>
    .red {
        color: red;
    }
</style>

<!-- 原理如下: 通过使用 PostCSS 来实现转换-->

<template>
    <div class="red" data-v-f3f3eg9>hi</div>
</template>
<style>
    .red[data-v-f3f3eg9] {
        color: red;
    }
</style>
```

-   深度作用选择器：使用 >>> 操作符可以使 scoped 样式中的一个选择器能够作用得“更深”，例如影响子组件

-   Sass 之类的预处理器无法正确解析 >>> 。这种情况下你可以使用 /deep/ 或 ::v-deep 操作符取而代之

```css
#app {
    /deep/ a {
        color: rgb(196, 50, 140)
    }
    ::v-deep a {
        color: rgb(196, 50, 140)
    }
}
```
   
#### CSS Module

> 用于模块化和组合 CSS 的系统

```html
<style module lang="scss">
    .red {
        color: #f00;
    }
    .bold {
        font-weight: bold;
    }
</style>
```

-   模板中通过$style.xx访问

```html
<a :class="$style.red">awesome-vue</a>
<a :class="{[$style.red]:isRed}">awesome-vue</a>
<a :class="[$style.red, $style.bold]">awesome-vue</a>
```

### 数据访问相关

#### 数据模拟

> 使用开发服务器配置before选项，可以编写接口，提供模拟数据

-   [配置](https://cli.vuejs.org/zh/config/#devserver)

```javascript
// vue.config.js
module.exports = {
    devServer: {
        before(app) {  // app是 express 示例
            app.get('/api/courses', (req, res) => {
                setTimeout(() => {
                    res.json([
                        { name: 'web全栈', price: 8999 },
                        { name: 'web高级', price: 8999 },
                    ])
                }, 1000);
            })
        },
    },
}

// api/xx.js
import axios from 'axios'
// 模拟异步数据调用
export function getCourses() {
    return axios.get('/api/courses').then((res) => res.data)
}

// 引用
import { getCourses } from '@/api/course.js'
export default {
    async created() {
        const courses = await getCourses()
        this.courses = courses
    },
}
```

#### 代理

> 设置开发服务器代理选项可以有效避免调用接口时出现的跨域问题。

```javascript
// vue.config.js
module.exports = {
    devServer: {
        proxy: 'http://localhost:3000', 
        // 在 接口中用 axios 请求 '/api/courses' 地址
        // 先尝试找 静态资源，没有相匹配的后
        // 开发服务器代理转发 所配置的地址上 防止跨域问题的出现，相当于请求本地服务
    },
}
```

## vue router

```html
<div id="nav">
    <!-- 导航链接 即 a 标签 -->
    <!-- to 即 href -->
    <router-link to="/">Home</router-link> |
    <router-link to="/admin">Admin</router-link>
</div>
<!-- 路由出口 -->
<!-- 路由匹配到的组件将渲染在这里 -->
<router-view />
```

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../views/Home.vue'

Vue.use(VueRouter)

// 定义路由
const routes = [
    { path: '/', name: 'Home', component: Home, },
    { path: '/admin', name: 'admin',
        // 懒加载
        component: () => import('../views/Admin.vue'),
    },
]

// 创建 并暴露出 router 实例
const router = new VueRouter({
    routes,
})

export default router
```

### 动态路由匹配

-   路由设计：`{ path: '/user/:id', component: User }`
-   参数匹配：`{{ $route.params.name }} `

### 通配符匹配

> 适合做404页面路由

```javascript
{
    // 会匹配所有路径
    path: '*',
    component: () => import('../views/404.vue')
}

```

### 嵌套路由

```javascript
    {
        path: '/admin',
        name: 'admin',
        component: () => import('../views/Admin.vue'),
        children: [
            {
                path: '/admin/course/:name',
                name: 'detail',
                component: () => import('../views/Detail.vue'),
            },
        ],
    },
```

### 编程导航

>借助 router 的实例方法，可编写代码来实现编程式导航

```html
<div @click="$router.push(`/admin/course/${c.name}`)"></div>

<!-- 等价于 -->
<div @click="onClick(e)"></div>
<script>
...
methods: {
    onClick(e) {
        this.$router.push(`/admin/course/${c.name}`)
    }
},
</script>

<!-- 等价于 -->
<router-link :to="`/admin/course/${c.name}`"></router-link>

```

-   常用 路由的别人进行路由的配置
```javascript
// 带动态参数 ：/admin/course/:name
// 带查询参数：使用 query：{xx:'xx'}
this.$router.push(
    name:'detail',
    params: {
        name: c.name
    }
)
```

-   组件复用时的路由问题

    -   在嵌套式路由中，父组件跳转，在子组件 created 钩子中 axios 获取数据时，
    -   只能获取一次数据，即在第一次跳转创建子组件的时刻 获取数据
    -   避免该问题，使用 watch 监听 $router

    ```javascript
        export default {
            watch: {
                $route: {
                    immediate: true, // created时 立即执行一次
                    handler() {
                        console.log('获取数据')
                    },
                },
            },
        }
    ```

### 路由守卫

#### 全局守卫

```javascript
// 判断路由是否需要守卫,在配置路由挂在 meta 数据
// meta 数据
router.beforeEach((to, from, next) => {
    if (to.meta.auth) {
        if (window.isLogin) {
            next()
        } else {
             // 登录后重定向，即跳转到登录前所要跳转的位置
            next('/login?redirect='+to.fullPath)
        }
    } else {
        next()
    }   
})

```

```javascript
{
    path: '/admin',
    name: 'admin',
    meta: {
        auth: true
    }
},
{
    path: '/login',
    component: () => import('../views/Login.vue')
},
```

```html
<template>
    <div>
        <button @click="login" v-if="!isLogin">登录</button>
        <button @click="logout" v-else>登出</button>
    </div>
</template>
<script>
export default {
    methods: {
        login() {
            window.isLogin = true
            this.$router.push(this.$route.query.redirect)
        },
        logout() {
            window.isLogin = false
            this.$router.push('/')
        }
    },
    computed: {
        isLogin() {
            return window.isLogin
        }
    },
}
</script>

```

#### 路由独享的守卫

```javascript
{
    path: '/admin',
    name: 'admin',
    // 不需要使用meta 数据的判断 直接做守卫即可
    // 小项目使用
    beforeEnter(to, from, next) {
        if (window.isLogin) {
            next()
        } else {
            next('/login?redirect=' + to.fullPath)
        }
    },
},
```

#### 组件内守卫 （最小守卫）

-   可以在路由组件内直接定义以下路由导航守卫：
    -   beforeRouteEnter
    -   beforeRouteUpdate
    -   beforeRouteLeave

### 数据获取

-   路由激活时，获取数据的时机有两个：

    -   路由导航前
    -   路由导航后

[数据获取](https://router.vuejs.org/zh/guide/advanced/data-fetching.html)

### 动态路由

> 通过router.addRoutes(routes)方式动态添加路由

```javascript
// 全局守卫修改为：要求用户必须登录，否则只能去登录页
router.beforeEach((to, from, next) => {
if (window.isLogin) {
    if (to.path === '/login') {
        next('/')
    } else {
        next()
    }
} else {
    if (to.path === '/login') {
        next()
    } else {
        next('/login?redirect=' + to.fullPath)
    }
}
})
```
```javascript
// Login.vue用户登录成功后动态 添加/admin
login() {
    window.isLogin = true;
    this.$router.addRoutes([
        {
            path: "/admin", //...
        }
    ]);
    const redirect = this.$route.query.redirect || "/";
    this.$router.push(redirect);
}
```

### 路由组件缓存

> 利用keepalive做组件缓存，保留组件状态，提高执行效率

```html
<keep-alive include="admin">
    <router-view></router-view>
</keep-alive>
```

-   使用include或exclude时要给组件设置name,**不是路由设置的 name**
    -   可以动态使用：`:include=['admin']`
    -   exclude: 排除 exclude 的内的组件，其余都缓存
    -   max: 最多缓存 xx 个，多了以后需要 进一个 前要出一个
-   两个特别的生命周期：activated、deactivated

### 路由懒加载

>路由组件的懒加载能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

`component: () => import("../views/About.vue")`

## VueX

> Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以可预测的方式发生变化。

```javascript
// store/index.js
export default new Vuex.Store({
    state: {
        // 将应用全局状态定义在state中
        isLogin: false,
    },
    mutations: {
        // 修改state只能通过mutation
        login(state) {
            state.isLogin = true
        },
        logout(state) {
            state.isLogin = false
        },
    },
    // Action 类似于 mutation，不同在于：
    //     Action 提交的是 mutation，而不是直接变更状态。
    //     Action 可以包含任意异步操作。
    actions: {
        // 参数1. vuex 传递的上下文context：{}
        login({ commit }, username) {
            // 模拟登录 api调用，一秒后 如果用户名时 admin 则登录成功
            return new Promise((resolve, reject) => {
                setTimeout(() => {
                    if (username === 'admin') {
                        commit('login')
                        resolve()
                    } else {
                        reject()
                    }
                }, 1000)
            })
        },
    },

    modules: {},
})
```

-   获取:使用`store.state`获取状态

    -   `<button @click="login" v-if="!$store.state.isLogin"> Login </button>`

-   修改状态只能通过`store.dispatch(mutation)`
    -   `this.$store.commit('login')`

-   actions 派发动作
    
    ```javascript
    // 派发动作，触发 actions
    this.$store
        .dispatch('login', 'admin')
        .then(() => {
            this.$router.push(this.$route.query.redirect)
        })
        .catch(() => {
            alert('error')
        }) 
    ```

### 实践

#### 模块化

-   使用modules定义多个子模块利于组件复杂状态

```javascript
import user from './user'
export default new Vuex.Store({
    modules: {
        user,
    }
})
```

-   移动先前登录状态相关代码到user.js

```javascript
export default {
    namespaced: true, // 设置独立命名空间，避免命名错误
    // ...
}
```

-   访问方式响应变化

```js
// Login.vue
<button @click="login" v-if="!$store.state.user.isLogin">登录</button>

this.$store
    .dispatch('user/login', 'admin')
    .then(() => {
        const redirect = this.$route.query.redirect || '/'
        this.$router.push(redirect)
    })
    .catch(() => {
        alert('用户名或密码错误')
    })
```

```javascript
// router/index.js
store.state.user.isLogin
```

#### 映射 mapState()/mapMutation()/mapAction()

-   通过这些映射方法可以让大家少敲几个字，避免对$store直接访问。

-   state 状态映射

```javascript
import { mapState } from 'vuex'
computed: {
    ...mapState('user', ['isLogin'])
}

<button @click="login" v-if="!isLogin">登录</button>
```

-   action 映射

```javascript
import { mapActions } from 'vuex'
methods: {
    login() {
        this['user/login']('admin').then(...)
    },
    ...mapActions(['user/login', 'user/logout'])
},
```

#### 派生状态 Getters

> 可以使用getters从store的state中派生出一些状态

-   换句话说就是计算属性在 vuex的迁移

```javascript
// vuex user 模块的改造
// 嵌入 Getters
export default {
    namespaced: true,
    state: {
        isLogin: false,
        username: '',
    },
    mutations: {
        login(state, username) {
            state.isLogin = true
            state.username = username
        },
        logout(state) {
            state.isLogin = false
            state.username = ''
        },
    },
    Getters: {
        welcome: (state) => {
            state.username + 'welcome...'
        },
    },
    actions: {
        login({ commit }, username) {
            return new Promise((resolve, reject) => {
                setTimeout(() => {
                    if (username === 'admin') {
                        commit('login', username)
                        resolve()
                    } else {
                        reject()
                    }
                }, 1000)
            })
        },
    },
}
```

```javascript
// vuex 映射
import { mapState, mapGetters } from 'vuex'

export default {
    name: 'app',
    computed: {
        ...mapState('user', ['isLogin']),
        ...mapGetters('user', ['welcome']),
    },
}

// 组件中直接使用即可
<span v-if="isLogin">
    {{ welcome }}
    <button>logout</button>
</span>

```

#### 严格模式

> 严格模式下，无论何时发生了状态变更且不是由 mutation 函数引起的，将会抛出错误。这能保证所有 的状态变更都能被调试工具跟踪到。

```javascript
const store = new Vuex.Store({
    // ...
    strict: true
})
```

#### 插件

```javascript
export default (store) => {
    // store 初始化时 将 localStorage 中的状态栏还原
    if (localStorage) {
        const user = JSON.parse(localStorage.getItem('user'))
        if (user) {
            store.commit('login', user.username)
        }
    }
    // 用户状态发生变化时缓存之
    store.subscribe((mutation, state) => {
        // {type: 'user/login'}
        // {type: 'user/logout'}
        if (mutation.type===('user/login')) {
            const user = JSON.stringify(state.user)
            localStorage.setItem('user', user)
        } else if (mutation.type === 'user/logout') {
            localStorage.removeItem('user')
        }
    })
}}

```

# Typescript

## TS开发环境搭建

-   安装typescript并初始化配置

```bash
npm i typescript -g
tsc --init
npm init -y
```

## 工程化

-   安装webpack, webpack-cli, webpack-dev-server

```bash
npm i webpack webpack-cli webpack-dev-server ts-loader typescript html-webpack-plugin -D
```

-   配置文件：build/webpack.config.js

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    entry: './src/index.ts',
    output: {
        filename: 'app.js'
    },
    resolve: {
        extensions: ['.js', '.ts', '.tsx']
    },
    devtool: 'cheap-module-eval-source-map',
    module: {
        rules: [
            {
                test: /\.tsx?$/i,
                use: [{
                    loader: 'ts-loader'
                }],
                exclude: /node_modules/
            }
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html'
        })
    ]
}
```

-   添加开发脚本，package.json

```json
"scripts": {
    "dev": "webpack-dev-server --config ./build/webpack.config.js"
}
```

## TS特点

-   类型注解、类型检测
-   类
-   接口
-   泛型
-   装饰器
-   类型声明

### 类型注解

```typescript
// ts-test.ts
let var1: string; // 类型注解
var1 = "zzz"; // 正确
var1 = 4; // 错误

// 类型推论： 编译器类型推断可省略这个语法
let var2 = true;
// 常见原始类型: string,number,boolean,undefined,null,symbol

// 联合类型
let var3: string | undefined;
```

-   其他类型

```typescript
// 类型数组
let arr: string[]
arr = ['xx']

// 任意类型 any
let varAny: any
varAny = 'xx'
varAny = 33

// any 用于数组
let arrAny: ang[]
arrAny = [1, true, 'free']
aryAny[1] = 100

// 函数类型约束
function greet(person: string):string {
    return 'hello,' + person
}
const msg = greet('zf')

// void类型，常用于没有返回值的函数
function warn(): viod{}

// 对象object: 不是原始类型就是对象类型
function fn1(o:object) {}
fn1({prop:0}) // ok
fn1(1) // error
fn1('string') //error

// 更好的约束方式
function fn2(o: { prop: number }) {}
fn2({ prop: 0 }) // OK
fn2({ prop: 'zf'}) // error

// 类型别名 type:自定义类型
type Prop = {prop: number}
function fn3(o:Prop) {} // 等同于 fn2
```

### 类型断言

> 某些情况下用户会比编译器更确定某个变量的具体类型，可用类型断言as

```typescript
const someValue: any = "this is a string";
const strLength = (someValue as string).length; // 通常类型断言会将一种更范的类型断言为更具体的类型
```

### 联合类型

> 希望某个变量或参数的类型是多种类型其中之一

```typescript
let union: string | number;
union = '1'; // ok
union = 1; // ok
```

### 交叉类型(扩展类型)

> 想要定义某种由多种类型合并而成的类型使用交叉类型

```typescript
type First = {first: number};
type Second = {second: number};
type FirstAndSecond = First & Second;
function fn3(param: FirstAndSecond): FirstAndSecond {
    return {first:1, second:2}
}
```

### 函数

-   必填参：参数一旦声明，就要求传递，且类型需符合
-   默认值：`age = 22`
-   可选参数：参数名后面加上问号，变成可选参数,必须放在参数最后

```typescript
function greeting(person: string, age = 22, msg?: string): string {
	return 'Hello, ' + person
}
```

#### 函数重载：以参数数量或类型区分多个同名函数

```typescript
// 重载1
function watch(cb1: () => void): void;
// 重载2
function watch(cb1: () => void, cb2: (v1: any, v2: any) => void): void;
// 实现
function watch(cb1: () => void, cb2?: (v1: any, v2: any) => void) {
    if (cb1 && cb2) {
        console.log('执行watch重载2');
    } else {
        console.log('执行watch重载1');
    }
}
```

### class的特性

ts中的类和es6中大体相同，这里重点关注ts带来的访问控制等特性

```typescript
class Parent {
    private _foo = "foo"; // 私有属性，不能在类的外部访问
    protected bar = "bar"; // 保护属性，可以在子类中访问
    
    // 参数属性：构造函数参数加修饰符，能够定义为成员属性
    constructor(public tua = "tua") {}
    
    // 方法也有修饰符
    private someMethod() {}
    
    // 存取器：属性方式访问，可添加额外逻辑，控制读写性
    get foo() {
        return this._foo;
    }
    set foo(val) {
        this._foo = val;
    }
}

```

-   vue示例

```html
<!-- components/Hello.vue -->
<template>
	<div>
		<p><input type="text" @keydown.enter="addFeature" /></p>
		<ul>
			<li v-for="feature in features" :key="feature">{{ feature }}</li>
			<li>{{ count }}</li>
		</ul>
	</div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'

@Component
export default class Hello extends Vue {
	// 属性就是data
	features: string[] = []

	// 函数直接作为回调
	addFeature(e: KeyboardEvent) {
		const inp = e.target as HTMLInputElement
		this.features.push(inp.value)
		inp.value = ''
	}

	// 如果和生命周期钩子同名，就是生命周期
	created() {
		this.features = ['类型注解', '编译型语言']
	}

    // 存取器用于计算属性
	get count() {
		return this.features.length
	}
}
</script>
```

```vue
// app.vue
<template>
	<div id="app">
		<Hello></Hello>
	</div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import Hello from './components/Hello.vue'

@Component({
	components: {
		Hello,
	},
})
export default class App extends Vue {}
</script>
```

### 接口

>接口仅约束结构，不要求实现，使用更简单

```typescript
// Person接口定义了解构
interface Person {
firstName: string;
lastName: string;
}
// greeting函数通过Person接口约束参数解构
function greeting(person: Person) {
return 'Hello, ' + person.firstName + ' ' + person.lastName;
}
greeting({firstName: 'Jane', lastName: 'User'}); // 正确
greeting({firstName: 'Jane'}); // 错误

```

-   vue 示例

```typescript
// @/types/index.ts
export interface Feature {
	id: number
	name: string
}
```

```html
<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'
import { Feature } from '@/types'

@Component
export default class Hello extends Vue {
	// 属性就是data
	features: Feature[] = []

	// 函数直接作为回调
	addFeature(e: KeyboardEvent) {
		const inp = e.target as HTMLInputElement
		this.features.push({ id: this.features.length + 1, name: inp.value })
		inp.value = ''
	}

	// 如果和生命周期钩子同名，就是生命周期
	created() {
		this.features = [{ id: 1, name: '类型注解' }]
	}
}
</script>
```

### 泛型

-   泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。以此增加代码通用性。

```typescript
// 不用泛型
// interface Result {
// ok: 0 | 1;
// data: Feature[];
// }
// 使用泛型
interface Result<T> {
ok: 0 | 1;
data: T;
}
// 泛型方法
function getResult<T>(data: T): Result<T> {
    return {ok:1, data};
}
// 用尖括号方式指定T为string
getResult<string>('hello')
// 用类型推断指定T为number
getResult(1)
```

-   泛型优点：
    -   函数和类可以支持多种类型，更加通用
    -   不必编写多条重载，冗长联合类型，可读性好
    -   灵活控制类型约束
-   不仅通用且能灵活控制，泛型被广泛用于通用库的编写。

-   实战

```javascript
// vue.config.js
module.exports = {
	devServer: {
		before(app) {
			app.get('/api/list', (req, res) => {
				res.json([
					{ id: 1, name: '类型注解' },
					{ id: 2, name: '编译型语言' },
				])
			})
		},
	},
}
```

```typescript
// @/api/feature.ts
import axios from 'axios'
import { Feature } from '@/types'

export function getFeatures() {
	return axios.get<Feature[]>('/api/list')
}
```

```typescript
// Hello.vue
import { Component, Vue } from 'vue-property-decorator'
import { Feature } from '@/types'
import { getFeatures } from '@/api/feature'

@Component
export default class Hello extends Vue {
	// 属性就是data
	features: Feature[] = []

	// 如果和生命周期钩子同名，就是生命周期
	created() {
		getFeatures().then((res) => {
			this.features = res.data
		})
	}
}
```

### 声明文件

>使用ts开发时如果要使用第三方js库的同时还想利用ts诸如类型检查等特性就需要声明文件，类似 xx.d.ts 同时，vue项目中还可以在shims-vue.d.ts中编写声明，从而扩展模块，这个特性叫模块补充

-   挂载$axios到vue原型上在组件里面直接用

```typescript
// shims-vue.d.ts
import Vue from "vue";
import { AxiosInstance } from "axios";

// 声明后缀 .vue 文件处理
declare module '*.vue' {
	export default Vue
}

// 模块扩展 vue
declare module "vue/types/vue" {
    interface Vue {
        $axios: AxiosInstance;
    }
}

// 在 入口模块 main.js 挂在 axios 
import axios from 'axios'
Vue.prototype.$axios = axios;
```

```typescript
// shims-vue.d.ts
// 扩展 ComponentOptions
import VueRouter from "vue-router";
import { Store } from "vuex";
declare module "vue/types/options" {
    interface ComponentOptions<V extends Vue> {
        router?: VueRouter;
        store?: Store<any>;
    }
}
```

### 装饰器

> 装饰器用于扩展类或者它的属性和方法。@xxx就是装饰器的写法

#### 属性声明：@Prop

> 除了在@Component中声明，还可以采用@Prop的方式声明组件属性

```typescript
export default class HelloWorld extends Vue {
    // Props()参数是为vue提供属性选项
    // !称为明确赋值断言，它是提供给ts的
    @Prop({type: String, required: true})
    private msg!: string;
}
```

父组件向子组件传递属性时直接 使用。

