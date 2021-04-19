# Vue3新特性以及特殊用法备忘

## 数据绑定

### v-bind动态参数

-   `<a :[zfHref]="'baidu.com'">click</a>`
-   `{...,zfHref:'href'}`
-   使用 v-bind 绑定动态参数 必须使用 上面格式 不能省略 ：，且动态参数的值也是动态的

## 事件绑定

### 事件对象（$event）

> 传递多个参数 `$event` 放在最后

-   e.srcElement 当前DOM节点
    -   改变 css样式： `e.srcElement.style.background = 'red'`
    -   获取自定义属性： `e.srcElement.dataset.aid` （`data-aid="111"`）
    -   阻止默认行为：`event.preventDefault()`
    -   阻止冒泡：`event.stopPropagation()`

### 多事件处理

> 在事件处理程序中使用逗号分隔多个事件处理程序

`<button @click="one($event), two($event)">Submit</button>`

## 组件

### 父子组件

-   父组件

```html
  <template>
  	<div>parent</div>
  	<div>parent : {{ msg }}</div>
  	<button @click="getChildMsg">get Child Msg</button>
  	<button @click="setChildMsg">set Child Msg</button>
  	<button @click="getChildMethods">get Child Methods</button>
  	<Child ref="Child" />
  </template>

  <script>
  import Child from './components/Child.vue';
  export default {
  	components: { Child },
  	data() {
  		return {
  			msg: 'parent msg',
  		};
  	},
  	methods: {
  		getChildMsg() {
  			// 获取子组件数据
  			console.log(this.$refs.Child.msg);
  		},
  		setChildMsg() {
  			this.$refs.Child.msg = 'parent change child';
  		},
  		getChildMethods() {
  			// 获取子组件方法
  			this.$refs.Child.run();
  		},
  		run() {
  			alert('parent run');
  		},
  	},
  };
  </script>
```

-   子组件

```html
  <template>
  	<br />
  	<div>ChildComponent</div>
  	<div>Child : {{ msg }}</div>
  	<button @click="getParentMsg">get Parent Msg</button>
  	<button @click="setParentMsg">set Parent Msg</button>
  	<button @click="getParentMethods">get Parent Methods</button>
  </template>

  <script>
  export default {
  	data() {
  		return {
  			msg: 'child msg',
  		};
  	},
  	methods: {
  		run() {
  			console.log('child methods');
  		},
  		getParentMsg() {
  			// 获取父组件数据
  			alert(this.$parent.msg);
  		},
  		setParentMsg() {
  			// 修改父组件数据
  			this.$parent.msg = 'child change parent msg';
  		},
  		getParentMethods() {
  			// 获取父组件方法
  			this.$parent.run();
  		},
  	},
  };
  </script>
```

### 自定义事件

#### `emit`

```js
// 在子组件 中 触发 父组件 自定义事件，官方建议 配置 emits:['submit'],
// 自定义事件参数校验，类似props
export default {
	emits: {
		submit: ({ username, password }) => {
			if (username !== '' && password !== '') {
				return true;
			} else {
				return false;
			}
		},
	},
	data() {
		return {
			username: '',
			password: '',
		};
	},
	methods: {
		handle() {
			this.$emit('submit', {
				username: this.username,
				password: this.password,
			});
		},
	},
};
```

#### `mitt`

-   Vue3.x以后从实例中移除了 $on，$off 和 $once 方法，$emit 仍然是现有 API 的一部分，只能实现子组件触发父组件的方法。

> 非父子组件传值 在 vue3 中只能使用第三方插件

-   <https://github.com/developit/mitt>

-   `npm install --save mitt`

```js
  import mitt from 'mitt';
  const event = mitt();
  export default event;
```

```html
  <template>
  	<zf-header />
  	<zf-footer />
  </template>
  <script>...</script>
```

```html
  <template>
  	<div>footer</div>
  </template>

  <script>
  import event from '../model/event.js';
  export default {
  	mounted() {
  		// 监听广播
  		event.on('toFooter', (data) => {
  			console.log(data);
  		});
  	},
  };
  </script>
```

```html
  <template>
  	<div>footer</div>
  </template>

  <script>
  import event from '../model/event.js';
  export default {
  	mounted() {
  		// 监听广播
  		event.on('toFooter', (data) => {
  			console.log(data);
  		});
  	},
  };
  </script>
```

### 自定义组件

#### 自定义组件双向数据绑定

```html
  <template>
  	<MyInput v-model:homemsg="homemsg"> </MyInput>
  	<br />
  	{{ homemsg }}
    <!-- <MyInput v-model:homemsg="homemsg" v-model:homemsg1="homemsg1"> </MyInput> -->
    <!-- {{ homemsg }} {{ homemsg1 }} -->
  </template>

  <script>
  // v-model 可以做多次绑定，子组件使用同样的操作即可
  import MyInput from './MyInput.vue';
  export default {
  	data() {
  		return {
  			homemsg: 'this is. home msg',
        // homemsg1: 'this is. home msg1',
  		};
  	},
  	components: { MyInput },
  };
  </script>
```

```html
  <template>
  	<input
  		type="text"
  		:value="homemsg"
  		placeholder="pl. input content"
  		@input="$emit('update:homemsg', $event.target.value)"
  	/>
    <!-- <input
    type="text"
    :value="homemsg1"
    placeholder="pl. input content"
    @input="$emit('update:homemsg1', $event.target.value)"
    /> -->
  </template>

  <script>
  // $emit() 第一个参数固定写法，update:[v-model绑定的自定义属性名]
  export default {
  	props: ['homemsg'],
    // props: ['homemsg', 'homemsg1'],
  };
  </script>
```

#### 非 Prop 的Attribute 继承

> 在父组件 挂载 一个非 prop 的 attribute（ class、style 和 id ），该组件并没有相应 props 或 emits 定义的 attribute，等价于在子组件上挂载该 Attribute

-   注意：此继承 是被 子组件的根节点（标签）继承

```html
  <!-- 父组件-->
  <template>
	   <my-button class="red" data-zf="zf" @click="myHandle">submit</my-button>
    <my-button class="origin">submit</my-button>
  </template>
  <script>
  import MyButton from './MyButton.vue';
  import MyInput from './MyInput.vue';
  export default {
  	data() {
  		return {
  			homemsg: 'this is. home msg',
  			homemsg1: 'this is. home msg1',
  		};
  	},
  	components: { MyInput, MyButton },
  	methods: {
  		myHandle() {
  			console.log('myHandle');
  		},
  	},
  };
  </script>


  <!-- 子组件-->
  <template>
  	<button><slot></slot></button>
  </template>

  <script>
  export default {
  	mounted() {
  		console.log(this.$attrs);
  		if (this.$attrs.onClick) {
  			this.$attrs.onClick();
  		}
  	},
  };
  </script>

  <style lang="scss" scoped>
  .red {
  	background-color: #f00;
  }
  .origin {
  	background-color: #ff0;
  }
  </style>
```

#### 自定义 Attribute 继承

```html
  <!-- 父组件-->
  <template>
    <date-picker data-time="2021-01-01" />
  </template>

  <!-- 子组件-->
  <template>
  	<div class="date-picker">
      <!-- 指定继承-->
  		<input type="date" v-bind="$attrs" />
  	</div>
  </template>

  <script>
  export default {
  	// 通过将 inheritAttrs 选项设置为 false，可以访问组件的 $attrs property，该 property 包括组件 props 和 emits property 中未包含的所有属性
  	inheritAttrs: false,  // 关闭默认继承
  };
  </script>
```

> 有多个平级节点时需要采用 指定继承（没有根节点）

## 生命周期

> vue3 变更
> vue3 在卸载组件实例之前调用 : beforeUnmount
> vue3 卸载组件实例后调用 : unmount (调用此钩子时，组件实例的所有指令都被解除绑定，所有事件侦听器都被移除，所有子组件实例被卸载)

### keep-alive 动态组件

> 组件之间频繁切换(或路由切换)时，需要保持这些组件的状态，以避免反复重渲染导致的性能问题，此时使用keep-alive

```html
  <keep-alive>
      <life-cycle v-if="isShow"></life-cycle>
  </keep-alive>
  <script>
  // 使用 keep-alive 组件 时，下面生命周期 取代 beforeUnmount unmount
  export default {
    activated(){
      //被 keep-alive 缓存的组件激活时调用。
    },
    deactivated(){
      // 被 keep-alive 缓存的组件停用时调用。
    }
  }
  </script>
```

### this.$nextTick

> Vue中把获取Dom节点的代码放在mounted里面，但是如果要在渲染完成数据后获取DOM节点就需要用到this.$nextTick

-   注意：mounted 不会保证所有的子组件也都一起被挂载。如果希望等到整个视图都渲染完毕，可以在 mounted 内部使用 vm.$nextTick：
-   注意，updated 不会保证所有的子组件也都一起被重绘。如果希望等到整个视图都重绘完毕，可以在 updated 里使用 vm.$nextTick：

```js
mounted() {
  this.$nextTick(function () {
    // 仅在渲染整个视图之后运行的代码
  })
}
```

## vue3 全局绑定

```js
import { createApp } from 'vue'
import App from './App.vue'
import axios from 'axios';

var app=createApp(App)
app.config.globalProperties.axios=axios
app.mount('#app')
```

## 函数防抖思路(性能优化)

```js
getData() {
  clearTimeout(this.timer);
  this.timer = setTimeout(() => {
    // 进行数据请求
  }, 200);
},
```

## mixin

### mixin 合并

-   数据对象在内部会进行递归合并，并在发生冲突时以组件数据优先（即 组件内的mixin 覆盖 引入的 mixin ）
-   同名钩子函数将合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子之前调用（引入 先于 组件内执行）
-   值为对象的选项，例如 methods、components，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对（同名时 组件内 覆盖 引入 ）

## Teleport

> **vue3 新增**
>
> Vue3.x中的组件模板属于该组件，有时候需要把模板的内容移动到当前组件之外的DOM 中，这个时候就可以使用 Teleport。

```js
// 表示teleport内包含的内容显示到body中
<teleport to="body">
  内容
</teleport>

// 表示teleport内包含的内容显示到 id='app' 的DOM节点中
<teleport to="#app">
内容
</teleport>
```

## Composition API （vue3 核心）

### 定义、获取、修改 响应式数据

```html
  <template>
  	<h3>HomePage</h3>
  	<p>{{ title }}</p>
  	<p>{{ userinfo.username }}</p>
  	<p>{{ userinfo.age }}</p>
  	<button @click="getTitle">getTitle</button>
  	<button @click="getUserName">getUserName</button>
  	<button @click="setTitle">setTitle</button>
  	<button @click="setUserName">setUserName</button>
  	<br />
  	<!-- 双向绑定同样可行 -->
  	<input type="text" v-model="title" />
  	<input type="text" v-model="userinfo.username" />
  	<br />
  	<p>{{ description }}</p>
  	<input type="text" v-model="description" />
  </template>

  <script>
  import { reactive, ref, toRefs } from 'vue';
  export default {
  	name: 'HomePage',
  	setup() {
  		// ref reactive 定义响应式数据 等价于 date(){ return {}}
  		// ref 字符串、 数值、 数组、Bool类型
  		// reactive 对象

  		let title = ref('this is title');
  		let userinfo = reactive({
  			username: 'zf',
  			age: 222,
  		});
  		let article = reactive({
  			description: 'this is article',
  			click: 111,
  		});
  		let getTitle = () => {
  			alert(title.value);
  		};
  		let getUserName = () => {
  			alert(userinfo.username);
  		};
  		let setTitle = () => {
  			title.value = 'changed title';
  		};
  		let setUserName = () => {
  			userinfo.username = 'zzzf';
  		};
  		// 模板需要渲染 必须return
  		return {
  			title,
  			userinfo,
  			getTitle,
  			getUserName,
  			setTitle,
  			setUserName,
  			// ...article, // 由于 reactive 定义的是相应式数据，使用结构赋值后，变成了 静态数据（失去了相应式）
  			...toRefs(article), // 使用 toRefs 解构 reactive 定义的响应式对象数据
  			// 原理：把一个响应式对象转换成普通对象，该普通对象的每个 property 都是一个 ref ，和响应式对象 property 一一对应
  		};
  	},
  };
  </script>
```

### 计算属性 computed

```html
  <template>
  	<h3>Login</h3>
  	<input type="text" v-model="firstName" />
  	<input type="text" v-model="lastName" />
  	<input type="text" v-model="score" />
  	<p>{{ fullName }}</p>
  	<p>{{ pass }}</p>

  	<!-- 原始对象 -->
  	<input type="text" v-model="obj.userName" />
  	<p>{{ obj.userName }}</p>
  </template>

  <script>
  import { computed, reactive, readonly, toRefs } from 'vue';
  export default {
  	setup() {
  		// 非响应式数据（原始对象）
  		let obj = {
  			userName: 'zf',
  			age: 11,
  		};
  		let userInfo = reactive({
  			firstName: 'zzz',
  			lastName: 'fff',
  			score: 22,
  		});
  		// readonly “深层”的只读代理
  		// 传入一个对象（响应式或普通）或 ref，返回一个原始对象的只读代理。一个只读的代理是“深层的”，对象内部任何嵌套的属性也都是只读的
      // readonly 响应数据 => 原始数据（对象）
  		// userInfo = readonly(userInfo);
  		let fullName = computed(() => {
  			return userInfo.firstName + '' + userInfo.lastName;
  		});
  		let pass = computed(() => {
  			if (userInfo.score >= 60) {
  				return 'pass';
  			} else {
  				return 'no pass';
  			}
  		});
  		return {
  			...toRefs(userInfo),
  			fullName,
  			pass,
  			obj,
  		};
  	},
  };
  </script>
```

### watchEffect

> 在响应式地跟踪其依赖项时立即运行一个函数，并在更改依赖项时重新运行它。

```html
  <template>
  	<h3>search</h3>
  	<p>{{ num }}</p>
  </template>

  <script>
  import { reactive, toRefs, watchEffect } from 'vue';
  export default {
  	setup() {
  		let data = reactive({
  			num: 1,
  		});
  		// 无论值是否改变，都会先执行一次
  		// 其他项改变不会触发该方法
  		watchEffect(() => {
  			console.log(`num=${data.num}`);
  		});

  		setInterval(() => {
  			data.num++;
  		}, 1000);
  		return {
  			...toRefs(data),
  		};
  	},
  };
  </script>
```

### watch

-   懒执行，也就是说仅在侦听的**源**变更时才执行回调；
-   更明确哪些状态的改变会触发侦听器重新运行；
-   访问侦听状态变化前后的值

```html
  <template>
  	<h3>search</h3>
  	<p>num: {{ num }}</p>
  	<p>count: {{ count }}</p>
  </template>

  <script>
  import { reactive, toRefs, watch } from 'vue';
  export default {
  	setup() {
  		let data = reactive({
  			num: 1,
  			count: 1,
  		});
      // 第一个参数 是 源，不能传 data.num
  		watch(data, () => {
  			console.log('num=' + data.num);
  		});

  		setInterval(() => {
  			data.num++;
  		}, 1000);
  		return {
  			...toRefs(data),
  		};
  	},
  };
  </script>
```

#### watch 与 watchEffect 区别

-   watchEffect
    -   刚开始加载就会触发一次
    -   可以监听具体的数据变更
-   watch
    -   只有变化时触发
    -   可明确指明监听哪个数据的变化，但只能监听源的变化，没法指定 reactive 定义的对象的变化（传参只能传递数据源）
    -   可拿到以前的值和现在的值

```html
  <template>
  	<h3>watch</h3>
  	<input type="text" v-model="keyword" />
  	<br />
  	{{ keyword }}
  </template>

  <script>
  import { ref, watch } from 'vue';
  export default {
  	setup() {
  		let keyword = ref('');
  		watch(keyword, (newData, oldData) => {
  			console.log('newData', newData);
  			console.log('oldData', oldData);
  		});
  		return {
  			keyword,
  		};
  	},
  };
  </script>
```

### 组合式api生命周期钩子

```js
  import { onMounted } from 'vue';
  export default {
  	setup() {
  		console.log('setup');
  		onMounted(() => {
  			console.log('onMounted');
  		});
  		return {};
  	},
  	beforeCreate() {
  		console.log('beforeCreate');
  	},
  	mounted() {
  		console.log('mounted');
  	},
  };
  // => setup
  // => beforeCreate
  // => onMounted
  // => mounted
```

### props

```html
<!-- 父组件 -->
<child :msg="'msg'" :msgObj="{ num: 1 }" />
```

```js
// 子组件
import { toRefs } from 'vue';
export default {
	props: {
		msg: String,
		msgObj: Object,
	},
	setup(props) {
		console.log(props);
		console.log(props.msgObj.num);

    // 因为 props 是响应式的，你不能使用 ES6 解构，因为它会消除 prop 的响应性
    // 若解构 使用 toRefs
    const { msg, msgObj } = toRefs(props);
		console.log(msg.value);
		console.log(msgObj.value.num);
		return {};
	},
};
```

### Provider Inject

#### 非组合式组件中

> provide和inject数据不是响应的，改变的provide的数据，不会响应到inject注入的值；避免inject修改

-   在非组合式中， provide 传递给子孙组件的 data 变量变化时，子孙组件用inject 注入的时候不能同步响应

```js
// 根组件
export default {
	...
  data() {
    return {
      title: 'app title',
    };
  },
	// provide: { title: 'app title' },
  provide() {
		return {
			title: this.title,  // 需要绑定 date 数据时 需要使用该方式，不能使用对象的方式
			userinfo: {
				username: 'zgggf',
			},
		};
	},
};

// 子孙组件
export default {
  inject: ['title', 'userinfo'], // inject 注入
	mounted() {
		console.log(this.title);
		console.log(this.userinfo);
	},
};
```

#### Provider Inject

```js
// 根组件
setup() {
  let title = ref('app title');
  let userinfo = reactive({ username: 'zf', age: 32 });
  provide('title', title);
  provide('userinfo', userinfo);
  return {
    title,
    ...toRefs(userinfo),
  };
},

// 子孙组件（可同步响应，且可逆）
// 根组件改变数据，子孙组件同步
// 子孙组件改变数据，根组件仍同步
setup() {
  let title = inject('title');
  let userinfo = inject('userinfo');
  return { title, ...toRefs(userinfo) };
},
```

## vuex

### 基本使用

```js
  import { createStore } from 'vuex';

  const store = createStore({
  	// 定义状态（数据）
  	state() {
  		return {
  			count: 1, // 组件内获取值：this.$store.state.count
  			list: ['zf', 'fd'],
  			msg: 'this is store',
  		};
  	},
  	// 定义方法（改变state中的数据）
  	mutations: {
  		incCount(state) {
  			state.count++; // 组件内调用：this.$store.commit('incCount');
  		},
  		setCount(state, num) {
  			state.count = num; // 组件内调用：this.$store.commit('setCount', 14);
  		},
  	},
  	// 等价于计算属性
  	getters: {
  		revMsg(state) {
  			// 组件内获取getters中的数据：this.$store.getters.revMsg
  			// 通过 mutations 改变 state 中的msg 的数据，getters 同步更新（重新计算）
  			return state.msg
  				.split('')
  				.reverse()
  				.join('');
  		},
  		num(state) {
  			return state.count + 10;
  		},
  	},
  	// 触发 mutations 里面的方法，作用：可以在 actions 中做异步操作
  	actions: {
  		incCount(context) {
  			// this.$store.dispatch('incCount')
  			context.commit('incCount'); // 执行 mutations 中的 incCount
  		},
  		// 第一个参数使用解构解析做简化,第二个参数为传参
  		setCount({ commit }, num) {
  			// this.$store.dispatch('setCount')
  			setTimeout(() => {
  				commit('setCount', num);
  			}, 1000);
  		},
  	},
  });

  export default store;

  // --- main.js
  import App from './App.vue';
  import store from './vuex/store';

  let app = createApp(App);

  app.use(store);
  app.mount('#app');
```

### 通过 mapState 获取 state

```js
  import { mapState } from 'vuex';
  export default {
  	computed: {
      //  当 state 中的变量与组件所需映射的变量一样时
      // 直接传入一个数组，将需要映射的 state 传入即可
  		...mapState(['count', 'list']),
      //  当 state 中的变量与组件所需映射的变量不一样时，使用下面的方式做映射
      ...mapState({
        num: (state) => state.count,
        mylist: (state) => state.list,
      }),
      // 使用模块时的映射方式 eg. app 模块
      ...mapState('app', ['count','list']),
  	},
  };
```

### 通过 mapGetters 获取 getters

```js
  computed: {
    ...mapGetters(['revMsg']), // 变量名相同
    ...mapGetters({ // 变量名不同
      myMsg: 'revMsg',
    }),
  },
```

### 通过 mapMutations 获取 mutations

-   mapActions的映射同理

```js
  methods: {
    ...mapMutations([
      'incCount', // 将 `this.incCount()` 映射为 `this.$store.commit('incCount')`
      // `mapMutations` 也支持传参：
      'setCount' // 将 `this.setCount(num)` 映射为 `this.$store.commit('setCount', num)`
    ]),
    ...mapMutations({
      add: 'incCount' // 将 `this.add()` 映射为 `this.$store.commit('incCount')`
    }),
    // 使用模块时的映射方式 eg. app 模块
    ...mapMutations('app', ['incCount','setCount']),
  }
```

### Modules

将 vuex 做模块化抽离，便于后期维护

```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = createStore({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

this.$store.state.a // -> `moduleA`'s state
this.$store.state.b // -> `moduleB`'s state
```

### 在 组合式（composition）api 使用 vuex

-   组合式api中没有this.$store，可以使用useStore来替代

```js
  import { computed } from 'vue';

  import { useStore } from 'vuex';
  export default {
  	setup() {
  		const store = useStore();
  		return {
  			count: computed(() => {
  				return store.state.count;
  			}),
  			num: computed(() => {
  				return store.getters.num;
  			}),
  			incCount: () => {
  				store.commit('incCount');
  			},
  			incAction: () => {
  				store.dispatch('incCount');
  			},
  			setAction: (num) => {
  				store.dispatch('setCount', num);
  			},
  		};
  	},
  }
```

# Ts

## 数据类型

-   布尔类型 (boolean)
-   数字类型 (number)
-   字符串类型 (string)
-   数组类型 (array)
-   元组类型 (tuple)
-   枚举类型 (enum)
-   任意类型 (any)
-   null和 undefined
-   void类型
-   never类型

```ts
  let flagB: boolean = true;

  let num: number = 111;

  let str: string = 'zzz';

  let arr1: number[] = [1, 2, 3];
  let arr2: Array<number> = [12, 34, 66];

  // 元组 数组的一种,指定多种类型
  let tuple: [string, number, boolean] = ['zf', 33, true];

  // 枚举类型 定义标识符
  enum Flag {
  	success = 1,
  	error = -1,
  }

  let f: Flag = Flag.success;

  // 默认 索引值
  enum Color {
  	red,
  	blue,
  	orange,
  }

  let c: Color = Color.blue; // => 1

  enum ColorP {
  	red,
  	blue = 5,
  	orange,
  }
  let cpr = ColorP.red; // => 0
  let cpb = ColorP.blue; // => 5
  let cpo = ColorP.orange; // => 6

  // 任意类型()
  let an: any = 11;
  an = 'zf';

  // // ---- 用处
  // let obox: any = document.getElementById('box');
  // obox.style.color = 'red';

  // null（空） undefined（变量未赋值）(其他（never类型）数据类型的子类型)
  // 该类型不能被赋值
  // let und: number;
  // console.log(und); // => undefined 但解析器报错
  // let und: undefined;
  // console.log(und); // 解析器不报错
  // 定义未赋值 的写法
  let und: number | undefined;
  und = 21;
  console.log(und);

  let unde: number | undefined | null;

  // void类型: typescript中的void表示没有任何类型,一般用于定义方法的时候方法没有返回值。
  function run(): void {
  	console.log('run');
  }

  // never类型:是其他类型(包括null和 undefined)的子类型,代表从不会出现的值。这意味着声明 never的变量只能被 never类型所赋值
  let a: undefined;
  a = undefined;

  // --- 使用
  let aa: never;
  aa = (() => {
  	throw new Error('this is error');
  })();
```

## 函数

### 函数的定义

-   声明返回值

```ts
// 函数声明法
function run(): string {
	return 'zf';
}

// 匿名函数
let run1 = (): void => {
	console.log('run1');
};
```

-   声明传参

```ts
function getInfo(name: string, age: number): string {
	return `${name}--${age}`;
}
// 匿名函数 同理
```

### 可选参数

-   可选参数必须配置到参数的最后

```ts
function getInfo(name: string, age?: number): string {
	if (age) {
		return `${name}--${age}`;
	} else {
		return `${name}`;
	}
}
console.log(getInfo('zf'));
```

### 默认参数

```ts
function getInfo(name: string, age: number = 20): string {
	return `${name}--${age}`;
}
console.log(getInfo('zf'));
```

### 剩余参数

```ts
function sum(a: number, ...result: number[]) {
	let sum = a;
	for (let i = 0; i < result.length; i++) {
		sum += result[i];
	}
	return sum;
}
```

### 函数的重载

-   java 中方法的重载:两个或两个以上同名函数,但他们的参数不一样,会出现函数的重载
-   typescript 中的重载: 同名函数传入不同的参数,执行不同的结果

```ts
function getInfo(name: string): string;
function getInfo(age: number): number;
function getInfo(str: any): any {
	if (typeof str === 'string') {
		return 'name is ' + str;
	} else {
		return 'age is ' + str;
	}
}
console.log(getInfo('zf')); // 正确
console.log(getInfo(11)); // 正确
// console.log(getInfo(true)); // 错误

// -----
function getInfo(name: string): string;
function getInfo(name: string, age: number): number;
function getInfo(name: any, age?: any): any {
	if (age) {
		return 'name is ' + name + ' age is ' + age;
	} else {
		return 'name is ' + name;
	}
}
console.log(getInfo('zf')); // 正确
```

## 类

-   前言

```js
  function Person(name) {
  	this.name = name; /*属性*/
  	this.run = function () { /* 实例方法 */
  		alert(this.name + ' running');
  	};
  }
  //原型链上面的属性会被多个实例共享   构造函数不会
  Person.prototype.sex = 'gendel';
  Person.prototype.work = function () {
  	alert(this.name + ' working');
  };
  var p = new Person('zf');
  // alert(p.name);
  // p.run();
  p.work();
  console.log(p.sex);

  // // 静态方法
  // Person.getInfo = function () {
  // 	alert('static method');
  // };
  // //调用静态方法
  // Person.getInfo();

  // // --- 继承 => 对象冒充实现继承
  // function Web() {
  // 	Person.call(this);
  // }
  // var w = new Web();
  // w.run(); //对象冒充可以继承构造函数里面的属性和方法
  // // w.work(); //对象冒充可以继承构造函数里面的属性和方法   但是没法继承原型链上面的属性和方法

  // // --- 继承 => 原型链实现继承(实例化子类的时候没法给父类传参)
  // function Web() {}
  // Web.prototype = new Person();
  // var w = new Web();
  // //原型链实现继承:可以继承构造函数里面的属性和方法 也可以继承原型链上面的属性和方法
  // w.run();
  // w.work();
  // --- 继承 => 原型链+对象冒充的组合继承模式
  function Web(name) {
  	Person.call(this, name); //对象冒充继承   实例化子类可以给父类传参
  }
  // Web.prototype = new Person();
  Web.prototype = Person.prototype;

  let w = new Web('zf');
  w.run();
```

### 定义类

```ts
  class Person {
  	name: string; // 前面省略了 public 关键词 (声明this.变量)
  	// 构造函数 实例化触发的方式 (new)
  	constructor(name: string) {
  		this.name = name;
  	}
  	getName(): string {
  		return this.name;
  	}
  	setName(name: string): void {
  		this.name = name;
  	}
  }
```

### 类的继承

```ts
  class Person {
  	name: string;
  	constructor(name: string) {
  		this.name = name;
  	}
  	run(): string {
  		return `${this.name}--run`;
  	}
  }

  class Web extends Person {
  	constructor(name: string) {
  		super(name); // 初始化父类的构造函数
  	}
  	// 子类不仅可以继承父类的属性和方法,还可以进行扩展
  	work() {
  		console.log(this.name);
  	}
  	// 子类可进行同名方法的重写
  	run(): string {
  		return `${this.name}--run child`;
  	}
  }
  let w = new Web('ffffff');
  console.log(w.run());
  w.work();
```

### 类中的修饰符

-   ts 中定义属性时,提供了三种修饰符
    -   public: 公有类型 在类内部,子类内部,类外部都可以访问
        -   属性不加修饰符 默认:public
    -   protected: 保护类型 在类内部,子类内部 都可以方法,在类外部无法访问
    -   private: 私有类型 在类内部可以访问,在子类,类外部都无法访问

### 静态属性 静态方法

```ts
  class Person {
  	static age: number = 11;
  	// 静态方法中没法 直接调用类里面的属性
  	static print() {
  		console.log('print' + Person.age);
  	}
  }
  console.log(Person.age);
  Person.print();
```

### 多态

> 父类定义一个方法不去实现,让继承他的子类去实现,每个子类有不同的表现(继承的一种表现)

```ts
  class Animal {
  	name: string;
  	constructor(name: string) {
  		this.name = name;
  	}
  	eat() {
  		console.log('eat method');
  	}
  }

  class Dog extends Animal {
  	constructor(name: string) {
  		super(name);
  	}
  	eat() {
  		return this.name + ' dddddd';
  	}
  }

  class Cat extends Animal {
  	constructor(name: string) {
  		super(name);
  	}
  	eat() {
  		return this.name + ' cccc';
  	}
  }

  let d = new Dog('zz');
  let c = new Cat('fff');
  console.log(d.eat());
  console.log(c.eat());
```

### 抽象方法 抽象类

> ts 中的抽象类:是提供其他类继承的基类,不能直接被实例化
> 使用 abstract 关键字 定义抽象类,抽象方法,抽象类中的抽象方法不包含具体实现,且必须在派生类中实现

```ts
  // abstract 抽象方法只能放在抽象类中
  // 抽象类和抽象方法用于定于标准
  abstract class Animal {
  	public name: string;
  	constructor(name: string) {
  		this.name = name;
  	}
  	//子类必须包含 eat 方法
  	abstract eat(): any;
  }
  // let a = new Animal() 错误操作

  class Dog extends Animal {
  	constructor(name: string) {
  		super(name);
  	}
  	// 抽象类的子类必须实现抽象类中的方法
  	eat() {
  		return this.name + ' dddddd';
  	}
  }

  class Cat extends Animal {
  	constructor(name: string) {
  		super(name);
  	}
  	eat() {
  		return this.name + ' cccc';
  	}
  }

  let d = new Dog('zz');
  let c = new Cat('fff');
  console.log(d.eat());
  console.log(c.eat());
```

## 接口

> 一种标准,定义行为动作的规范,是一种约束

### 属性接口(对json的约束)

```ts
  // ts 中自定义方法传入参数 对json 进行约束
  function printLabel(labelInfo: { label: string }): void {
  	console.log('printLabel ' + labelInfo.label);
  }
  printLabel({ label: 'zzz' });

  // 批量约束
  interface FullName {
  	firstName: string; // 必须分号结尾，不是对象，是interface的语法
  	secondName: string;
  }

  function printName(name: FullName): void {
  	console.log('printname ' + name.firstName + name.secondName);
  }
  let a = {
  	// 入参抽离后必须包含 firstName secondName
  	// 入参的顺序可以不一样
  	// 不抽离出来时,入参只能有 firstName secondName
  	age: 33,
  	firstName: 'zzz',
  	secondName: 'fff',
  };
  printName(a);
  // printName({ age: 33, firstName: 'zzz', secondName: 'fff' }); 错误，入参必须与fullname 属性一一对应
```

-   可选属性接口

```ts
interface FullName {
  firstName: string;
  secondName?: string; // 与可选参数同理
}
```

### 函数类型接口

> 对 方法传入的 参数 以及 返回值 进行约束

```ts
  interface encrypt {
  	(key: string, value: string): string;
  }
  let md5: encrypt = (key: string, value: string): string => {
  	return key + value;
  };
  console.log(md5('zf', '----'));
```

### 可索引接口：数组 对象的约束（不常用）

```ts
  // 数组
  interface UserArr {
  	[index: number]: string;
  }
  let arr: UserArr = ['zf', 'dc'];

  // 对象
  interface UserObj {
  	[index: string]: string;
  }
  let obj: UserObj = { name: 'zf', age: '33' };
```

### 类 类型接口：对类的约束 与 抽象类有点相似

```ts
  interface Animal {
  	name: string;
  	eat(str: string): void;
  }
  class Dog implements Animal {
  	name: string;
  	constructor(name: string) {
  		this.name = name;
  	}
  	// 可不传参数，但必须有 eat 方法
  	eat(str: string): void {
  		console.log(this.name + ' ' + str);
  	}
  	// 可扩展其他方法
  	work() {
  		console.log('work');
  	}
  }
  let d = new Dog('zz');
  d.eat('gggg');
  d.work();
```

### 接口的扩展（继承）

```ts
  interface Animal {
  	eat(): void;
  }
  interface Person extends Animal {
  	work(): void;
  }
  class Programmer {
  	name: string;
  	constructor(name: string) {
  		this.name = name;
  	}
  	coding() {
  		console.log(this.name + 'coding');
  	}
  }

  class Web extends Programmer implements Person { // 类的继承 + 接口的扩展
  	constructor(name: string) {
  		super(name);
  	}
  	eat() {
  		console.log('eat');
  	}
  	work() {
  		console.log('work');
  	}
  }
  let w = new Web('zf');
  w.eat();
  w.work();
  w.coding();
```

## 泛型

> 在 C# 和 Java 中，可使用泛型来创建可重用的组件，一个组件可以支持多种类型的数据。 这样用户就可以以自己的数据类型来使用组件。
>
> 通俗理解：泛型就是解决 类 接口 方法的复用性、以及对不特定数据类型的支持(类型校验)

-   泛型解决的问题（可以支持不特定的数据类型）

```ts
  function getData(value: string): string {
  	// 只能返回 string 类型的数据
  	// 需求：入参与返参数据类型一直（使用 any => 放弃了类型检查，不能达到需求）
  	return value;
  }
```

-   泛型的使用

```ts
  // T：表示泛型，具体说明类型，是调用这个方法的时候决定的（可用T,或任意的大写字母，但前提必须统一）
  function getData<T>(value: T): T {
  	return value;
  }
  console.log(getData<number>(11));
  console.log(getData<number>('11')); // 错误写法（类型需一一对应）
```

### 泛型类

```ts
  // 最小堆算法
  class MinClass<T> {
  	public list: T[] = [];
  	add(value: T) {
  		this.list.push(value);
  	}
  	min(): T {
  		let minValue = this.list[0];
  		this.list.forEach((item) => {
  			if (minValue > item) {
  				minValue = item;
  			}
  		});
  		return minValue;
  	}
  }
  let c = new MinClass<string>();
  let c1 = new MinClass<number>();
  c.add('z'); c.add('i'); c.add('p'); c.add('a'); c.add('w');
  c1.add(33); c1.add(3); c1.add(12); c1.add(4); c1.add(7);
  console.log(c.list);
  console.log(c.min());
```

### 泛型接口

```ts
  interface ConfigFn {
  	<T>(value: T): T;
  }
  let setData: ConfigFn = <T>(value: T): T => {
  	return value;
  };

  console.log(setData<string>('zz'));

  // -- 方式 二
  interface ConfigFn<T> {
  	(value: T): T;
  }
  function getData<T>(value: T): T {
  	return value;
  }
  let g: ConfigFn<string> = getData;
  console.log(g('zz'));
```

### 泛型类2.0

-   前言（把类当作参数来约束数据传入类型）

```ts
  // 数据库表字段映射
  class User {
  	username: String | undefined;
  	password: String | undefined;
  }

  class MysqlDb {
  	add(user: User): boolean {
  		console.log(user);
  		return true;
  	}
  }

  let u = new User();
  u.username = 'zf';
  u.password = '111';
  let db = new MysqlDb();
  db.add(u);
```

-   使用泛型动态约束入参类型

```ts
  // 操作数据库的泛型类
  class MysqlDb<T> {
  	add(info: T): boolean {
  		console.log(info);
  		return true;
  	}
  	update(info: T, id: number): boolean {
  		console.log(info, id);
  		return true;
  	}
  }

  class User {
  	username: String | undefined;
  	password: String | undefined;
  }

  let u = new User();
  u.username = 'zf';
  u.password = '111';

  // 去除重复代码，对不特定类型进行数据校验
  let db = new MysqlDb<User>();
  db.add(u);

  class ArticleCate {
  	title: String | undefined;
  	dec: String | undefined;
  	status: number | undefined;
  	constructor(params: {
  		title: String | undefined;
  		dec: String | undefined;
  		status?: number | undefined;
  	}) {
  		this.title = params.title;
  		this.dec = params.dec;
  		this.status = params.status;
  	}
  }

  let a = new ArticleCate({ title: 'title', dec: 'dev' });
  a.status = 1;
  let adb = new MysqlDb<ArticleCate>();
  adb.add(a);
  adb.update(a, 1);
```

### 泛型接口 、泛型类应用

```ts
  interface DBI<T> {
  	add(info: T): boolean;
  	update(info: T, id: number): boolean;
  	delete(id: number): boolean;
  	get(id: number): any[];
  }

  // 实现泛型接口，类也应该是泛型类
  class MysqlDb<T> implements DBI<T> {
  	constructor() {
  		console.log('connect ...');
  	}
  	add(info: T): boolean {
  		console.log(info);
  		return true;
  	}
  	update(info: T, id: number): boolean {
  		throw new Error('Method not implemented.');
  	}
  	delete(id: number): boolean {
  		throw new Error('Method not implemented.');
  	}
  	get(id: number): any[] {
  		throw new Error('Method not implemented.');
  	}
  }

  class User {
  	username: string | undefined;
  	password: string | undefined;
  }

  let u = new User();
  u.username = 'zf';
  u.password = 'zf';

  let m = new MysqlDb<User>();
  m.add(u);
```

## 模块

> TypeScript 1.5 以后 术语名已经发生了变化
> 模块分为 内部模块（现在叫"命名空间"）、 外部模块（现在简称"模块"）
>
> 模块：公共模块的抽离

```ts
let dbUrl = 'xx';
function getDate(): any[] {
	console.log('getDate');
	return [1];
}
export { dbUrl, getDate };

// import { dbUrl, getDate } from './models/db'; 外部引入
```

### 命名空间

> 作用: **组织代码避免命名冲突**

-   基本使用

```ts
  namespace A {
  	export class Zf { // 需要暴露出去
  		constructor() {
  			console.log('A');
  		}
  	}
  }
  namespace B {
  	export class Zf {
  		constructor() {
  			console.log('B');
  		}
  	}
  }
  let az = new A.Zf();
  let bz = new B.Zf();
```

-   模块抽离

```ts
  export namespace A { // 模块抽离出来后 使用 export 暴露出去
  	export class Zf {
  		constructor() {
  			console.log('A');
  		}
  	}
  }
  export namespace B {
  	export class Zf {
  		constructor() {
  			console.log('B');
  		}
  	}
  }

  // ---- 外部 引入
  import { A, B } from './models/db';

  let az = new A.Zf();
  let bz = new B.Zf();
```

## 装饰器

> 装饰器:装饰器是一种特殊类型的声明，它能够被附加到类声明，方法，属性或参数上，可以修改类的行为。

-   装饰器就是一个方法，可以注入到类、方法、属性参数上来扩展类、属性、方法、参数的功能。

### 类装饰器

-   普通装饰器（无法传参）

```ts
  function logClass(params: any) {
  	console.log(params);
  	// params 就是当前类
  	// 扩展 当前类属性、方法
  	params.prototype.apiUrl = 'xxx';
  	params.prototype.run = () => {
  		console.log('this. is run');
  	};
  }

  @logClass
  class HttpClient {
  	constructor() {}
  	getDate() {}
  }

  let h: any = new HttpClient();
  console.log(h.apiUrl); // （在不修改类的前提下）动态扩展的属性
  h.run();
```

-   装饰器工厂（可传参）

```ts
  function logClass(params: string) {
  	return function (target: any) {
  		console.log(target);
  		console.log(params);
  		target.prototype.apiUrl = params;
  	};
  }

  @logClass('xxxx')
  class HttpClient {
  	constructor() {}
  	getDate() {}
  }

  let h: any = new HttpClient();
  console.log(h.apiUrl);
```

-   类装饰器重载构造函数

```ts
  // 类装饰器表达式会在运行时当作函数被调用，类的构造函数作为其唯一的参数。
  // 如果类装饰器返回一个值，它会使用提供的构造函数来替换类的声明。
  function logClass(target: any) {
  	console.log(target);
  	return class extends target {
  		// 构造函数的重载
  		apiUrl: any = 'this .is change aipUrl';
  		getDate() {
  			console.log(this.apiUrl + ' ----------');
  		}
  	};
  }

  @logClass
  class HttpClient {
  	public apiUrl: string | undefined;
  	constructor() {
  		this.apiUrl = 'this is HttpClient constructor apiUrl';
  	}
  	getDate() {
  		console.log(this.apiUrl);
  	}
  }

  let h = new HttpClient();
  h.getDate();
```

### 属性装饰器

-   属性装饰器表达式会在运行时当作函数被调用，传入下列2个参数：
    1.  对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
    2.  成员的名字。

```ts
  // 属性装饰器
  function logProperty(params: any) {
  	return function (target: any, attr: any) {
  		console.log(target);
  		console.log(attr);
  		target[attr] = params;
  	};
  }

  class HttpClient {
  	@logProperty('zzzzzzzzzzzzzz')
  	public url: any | undefined;
  	constructor() {}
  	getDate() {}
  }
  let h = new HttpClient();
  console.log(h.url);
```

### 方法装饰器

> 它会被应用到方法的 属性描述符上，可以用来监视，修改或者替换方法定义。

-   参数装饰器表达式会在运行时当作函数被调用，传入下列3个参数

    1.  对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
    2.  成员的名字。
    3.  成员的属性描述符。

```ts
  function logMethod(params: any) {
  	return function (target: any, methodName: any, methodDesc: any) {
  		console.log(target);
  		console.log(methodName);
  		console.log(methodDesc.value); // 当前的方法
  		target.apiUrl = 'zz.zzz.z'; // 扩展属性
  		// 扩展方法
  		target.run = function () {
  			console.log('this. is ...');
  		};
  		// 修改装饰器的方法
  		// 1. 保存当前的方法
  		let oMethod = methodDesc.value;
  		methodDesc.value = function (...args: any[]) {
  			args = args.map((value) => {
  				return String(value);
  			});
  			console.log(args);
  			// ---------- 以上代码是替换目标方法
  			oMethod.apply(this, args); // 使用 对象冒充 起到方法的代码同样执行的目的
  		};
  	};
  }
  class HttpClient {
  	public url: any | undefined;
  	constructor() {}
  	@logMethod('xx.xx.xx')
  	getData() {
  		console.log('this. is getData');
  	}
  }
  let h: any = new HttpClient();
  console.log(h.apiUrl);
  h.run();
  h.getData([12, 34, 'dd']);
```

-   方法参数装饰器（不常用）

```ts
  function logParams(params: any) {
  	return function (target: any, methodName: any, paramsIndex: any) {
  		console.log(target);
  		console.log(methodName);
  		console.log(paramsIndex); // 参数在 函数函数列表中的 索引
  		target.apiUrl = params;
  	};
  }
  class HttpClient {
  	public url: any | undefined;
  	constructor() {}
  	getData(@logParams('xxx') uuid: any) {
  		console.log(uuid);
  	}
  }
  let h: any = new HttpClient();
  console.log(h.apiUrl);
  h.getData(12);
```

### 装饰器执行顺序

-   属性>方法>方法参数>类

-   如果有多个同样的装饰器，它会先执行后面的（就近原则）

```ts
  @logClass1('zz')
  @logClass2('ff')
  class HttpClient {
  	@logAttribute1()
  	@logAttribute2()
  	public apiUrl: string | undefined;
  	constructor() {}

  	@logMethod1()
  	@logMethod2()
  	getData() {
  		return true;
  	}

  	setData(@logParams1() attr1: any, @logParams2() attr2: any) {}
  }

  var http: any = new HttpClient();

  // console.log =>
  // 属性装饰器2
  // 属性装饰器1
  // 方法装饰器2
  // 方法装饰器1
  // 方法参数装饰器2
  // 方法参数装饰器1
  // 类装饰器2
  // 类装饰器1
```

# Vue3 + Ts

## 基本使用

### ts项目基础

```ts
  // 父组件 导入时 必须把 .vue后缀名带上
  import { defineComponent } from 'vue';

  let title: string = 'this. is home';

  export default defineComponent({
  	data() {
  		return {
  			title,
  			userinfo: {
  				username: 'zf',
  			},
  			age: 20,
  			sex: 'gender',
  		};
  	},
  	methods: {
  		setTitle(): void {
  			this.title = 'dd';
  		},
  	},
  });
```

### 使用接口做条件约束

```ts
  import { defineComponent } from 'vue';

  // data 数据接口
  interface News {
  	title: string;
  	description: string;
  	count: number | string;
  	content?: string;
  }

  // let newsData: News = {
  // 	title: 'this is title',
  // 	description: 'this .is description',
  // 	count: 23,
  // };

  let newsData = {
  	title: 'this is title',
  	description: 'this .is description',
  	count: 23,
  } as News;

  export default defineComponent({
  	data() {
  		return newsData;
  	},
  	methods: {
  		setTitle(title: string): void {
  			this.title = title;
  		},
  	},
  	computed: {
  		revTitle(): string {
  			return this.title
  				.split('')
  				.reverse()
  				.join('');
  		},
  	},
  });
```

### 组合式API + Ts

```html
  <template>
  	<div>
  		User
  		<p>username --- {{ username }}</p>
  		<p>{{ getUserName() }}</p>
  		<p>{{ count }}</p>
  		<p>{{ revUsername }}</p>
  		<button @click="setUserName('XYZ')">click</button>
  	</div>
  </template>

  <script lang="ts">
  import { computed, defineComponent, reactive, ref, toRefs } from 'vue';

  interface User {
  	username: string;
  	age: number | string;
  	setUserName(username: string): void;
  	getUserName(): string;
  }

  export default defineComponent({
  	setup() {
  		// // 接口的 第一种写法
  		// // let user: User = reactive({
  		// // 接口的 第二种写法(源码中 reactive方法 使用 使用泛型做约束)
  		// let user = reactive<User>({
  		// 	username: 'zf',
  		// 	age: 33,
  		// 	setUserName(username) {
  		// 		this.username = username;
  		// 	},
  		// 	getUserName() {
  		// 		return this.username;
  		// 	},
  		// });
  		// 接口的第三中写法
  		let user = reactive({
  			username: 'abcdefg',
  			age: 33,
  			setUserName(username) {
  				this.username = username;
  			},
  			getUserName(): string {
  				return this.username;
  			},
  		}) as User;

  		let count = ref<number | string>('2');
  		// let count: number = ref(2); // 错误
  		let revUsername = computed((): string => {
  			return user.username
  				.split('')
  				.reverse()
  				.join('');
  		});
  		return {
  			...toRefs(user),
  			count,
  			revUsername,
  		};
  	},
  });
  </script>
```

## 路由

### 基本使用

-   根目录下配置路由

```ts
  // /routes.ts
  import { createRouter, createWebHashHistory } from 'vue-router';

  // 引入组件
  import Home from './components/Home.vue';
  import News from './components/News.vue';
  import User from './components/User.vue';

  // 配置路由
  const router = createRouter({
  	history: createWebHashHistory(),
  	routes: [
  		{ path: '/', component: Home },
  		{ path: '/news', component: News },
  		{ path: '/user', component: User },
  	],
  });

  export default router;
```

-   入口文件配置

```ts
  import { createApp } from 'vue';
  import App from './App.vue';
  import router from './routes';

  // createApp(App).mount('#app');
  let app = createApp(App);
  // 挂载路由
  app.use(router);
  app.mount('#app');
```

-   根组件渲染

```html
  <template>
  	<ul>
  		<li><router-link to="/">index</router-link></li>
  		<li><router-link to="/user">user page</router-link></li>
  		<li><router-link to="/news">news page</router-link></li>
  	</ul>
  	<router-view />
  </template>

  <script lang="ts">
  import { defineComponent } from 'vue';

  export default defineComponent({ name: 'App' });
  </script>
```

### 动态路由

-   配置

```ts
import NewsContent from './components/NewsContent.vue';

const router = createRouter({
	history: createWebHashHistory(),
	routes: [
    ...,
		{ path: '/newscontent/:aid', component: NewsContent },
	],
});
```

-   使用

```html
  <!-- News.vue -->
  <template>
  	<div>news</div>
  	<ul>
  		<li v-for="(item, index) in list" :key="index">
  			<router-link :to="`/newscontent/${index}`">{{ item }}</router-link>
  		</li>
  	</ul>
  </template>

  <script lang="ts">
  import { defineComponent } from 'vue';

  interface News {
  	list: string[];
  }

  export default defineComponent({
  	data() {
  		return {
  			list: [],
  		} as News;
  	},
  	mounted() {
  		for (let i = 0; i < 10; i++) {
  			this.list.push(`this is NO.${i} News Content`);
  		}
  	},
  });
  </script>
```

```html
  <!-- NewsContent.vue -->
  <template>
  	<div>NewsContent</div>
  	<p>NewsContent ------ {{ aid }}</p>
  </template>

  <script lang="ts">
  import { defineComponent } from 'vue';

  interface Data {
  	aid: string | string[];
  }

  export default defineComponent({
  	data() {
  		return {
  			aid: '',
  		} as Data;
  	},
  	mounted() {
  		console.log(this.$route.params);
  		this.aid = this.$route.params.aid;
  	},
  });
  </script>
```

### get 传值

-   配置

```ts
{ path: '/newscontent', component: NewsContent }
```

-   使用

```html
<router-link :to="`/newscontent?aid=${index}`">{{ item }}</router-link>
```

-   获取

```ts
this.$route.params
```

### 编程式导航

-   Js跳转路由(在 method 中使用) `this.$router.push({ path: '/home' });`

```html
  <!-- 直接使用-->
  <button @click="this.$router.push({ path: '/news' })">----</button>
  <!-- 使用get传值，配置第二个参数 query-->
  <button @click="this.$router.push({ path: '/newscontent', query: { aid: 3 } })" > ++++ </button>
  <!-- 使用动态路由-->
  <button @click="this.$router.push({ path: '/newscontent/3' })">----</button>
```

### HTML5 History 模式和 hash 模式

-   hash 模式

    -   路由样例：`http://localhost:8080/#/user`

```ts
  import { createRouter, createWebHashHistory } from 'vue-router'

  const router = createRouter({
    history: createWebHashHistory(),
    routes: [
      //...
    ],
  })
```

-   HTML5 History 模式

    -   路由样例：`http://localhost:8080/user`

    -   **注意：**开启Html5 History模式后，发布到服务器需要配置伪静态：
        -   `https://router.vuejs.org/zh/guide/essentials/history-mode.html`

```ts
  import { createRouter, createWebHistory } from 'vue-router'

  const router = createRouter({
    history: createWebHistory(),
    routes: [
      //...
    ]
  })
```

### 命名路由

-   配置时给路由配置名称，跳转可用该命名做跳转

```ts
  const router = new VueRouter({
    routes: [
      {
        path: '/user/:userId',
        name: 'user',
        component: User
      }
    ]
  })
```

-   跳转

```html
  <router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
  <script>
    this.$router.push({ name: 'user', params: { userId: 123 }})//  以上 两种 均为 动态路由 设置方式：/user/123

    this.$router.push({name:'content',query:{aid:222}}) //  get 传值
  </script>
```

### 路由重定向

-   路由重定向

    -   重定向是指用户访问时`/home`，URL将被替换`/`，然后与匹配`/`
    -   `const routes = [{ path: '/home', redirect: '/' }]`

-   命名路由重定向

    -   `const routes = [{ path: '/home', redirect: { name: 'homepage' } }]`

-   使用函数进行动态重定向

    ```ts
      const routes = [{
          path: '/search/:searchText',
          redirect: to => {
            return { path: '/search', query: { q: to.params.searchText } }
          },
      }]
    ```

### 路由别名

-   别名`/`as`/home`表示用户访问时`/home`，URL保持不变`/home`，但将被匹配，就像用户正在访问时一样`/`。

    -   `const routes = [{ path: '/', component: Homepage, alias: '/home' }]`

-   多个别名 可使用数组包裹：`alias:['/home','/list']`
-   动态路由：`const routes = [{ path: '/home/:id', component: Homepage, alias: '/h/:id' }]`

### 嵌套路由

```ts
  const router = createRouter({
      history: createWebHistory(),
      routes: [
          { path: '/', component: Home, alias: '/home' },
          {
              path: '/news', component: News,
              children: [  //子路由
                  { path: '', redirect:"/news/add"},
                  { path: 'add', component: NewsAdd },
                  { path: 'edit', component: NewsEdit },
              ]
          },
          { path: '/user', component: User },
      ],
  })
```

## vuex

### 非 composition API 中的使用

-   在做好 vuex 支持 ts 的配置，即可使用，使用方法如 js

-   mapState 、mapMutations 均可使用

```ts
  import { ComponentCustomProperties } from 'vue';
  import { createStore, Store } from 'vuex';

  //配置让Vuex支持ts
  declare module '@vue/runtime-core' {
  	// state中定义的数据都需要在此声明
  	interface State {
  		count: number;
  		list: string[];
  		msg: string;
  	}

  	interface ComponentCustomProperties {
  		$store: Store<State>;
  	}
  }

  const store = createStore({
  	state() {
  		return {
  			count: 1,
  			list: ['zf', 'fd'],
  			msg: 'this is store',
  		};
  	},

  	mutations: {
  		incCount(state: any) {
  			state.count++;
  		},
  		setCount(state: any, num: number) {
  			state.count = num;
  		},
  		setMsg(state: any, msg: string) {
  			state.msg = msg;
  		},
  	},
  	getters: {
  		revMsg(state: any) {
  			return state.msg
  				.split('')
  				.reverse()
  				.join('');
  		},
  		num(state: any) {
  			return state.count + 10;
  		},
  	},
  	actions: {
  		incCount(context) {
  			context.commit('incCount');
  		},
  		setCount({ commit }, num) {
  			setTimeout(() => {
  				commit('setCount', num);
  			}, 1000);
  		},
  	},
  });

  export default store;
```

### composition API 中的使用

-   使用方式 同 js 中 使用 composition api

```ts
setup() {
  const store = useStore();
  return {
    msg: computed(() => {
      return store.state.msg;
    }),
    setMsg: (): void => {
      store.commit('setMsg', '-- msg');
    },
  };
},
```
