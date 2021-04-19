# ReactJs

## React 基础

### 安装：

```bash
npm install react react-dom -S
```

### 起步

```js
import React from 'react'
import ReactDOM from 'react-dom'
// 参数1：创建元素的类型 => string
// 参数2：元素上的属性 => Object
// 参数3 + ：当前元素的子节点
const div = React.createElement('div',{id: 'zf', className: 'zf', title: 'title'}, 'this.is.div')
// 使用 ReactDOM 把元素渲染到页面指定的容器中：
// 参数1：要渲染的虚拟DOM元素
// 参数2：要渲染到页面上的哪个位置中
ReactDOM.render(div, document.getElementById('app'))
```

### JSX (语法糖)

1.  安装：

    -   `npm i babel-preset-react -D`

2.  配置：

    -   在 .babelrc 文件 presets 的值添加 "react"

3.  使用：

```js
const div = React.createElement('div',{id: 'zf', className: 'zf', title: 'title'}, 'this.is.div')
// 可替换为：
const div = (
  <div className="zf" id="zf" title="title">
    this.is.div
  </div>
)
```

4.  语法：

    -   变量的使用，用 `{}` 包含使用

        ```js
        const title = 'this.is.variable'
        const div = (
          <div className="zf" id="zf" title={title}>
            this.is.div
          </div>
        )
        ```

        > 当 编译引擎，在编译JSX代码的时候，如果遇到了&lt; 那么就把它当作 HTML 代码去编译，如果遇到了 {} 就把 花括号内部的代码当作 普通JS代码去编译；

    -   在 `{}` 内部，可以写任何符合 JS 规范的代码
    -   在 JSX 中，如果要为元素添加 class 属性了，必须写成 className , 避免与 ES6 中的 class 冲突
    -   label标签的 for 属性需要替换为 htmlFor
    -   在 JSX 创建 DOM 的时候，所有的节点，必须用**唯一**的根元素进行包裹
    -   注释必须放到 {} 内部 => `{/* 注释 */}`

```js
// 综合事例
const arr = []
  for (let i = 0; i < 10; i++) {
    const p = <p key={i}>this.is.p</p>
    arr.push(p)
  }
  const title = 'this.is.variable'
  const div = (
    <div className="zf" id="zf" title={title}>
      this.is.div
      {arr}
      {/* 注释 */}
    </div>
  )
ReactDOM.render(div, document.getElementById('app'))
```

## React 组件：

### 创建组件

#### 以 function 的方式创建

    -   在 react 中，构造函数就是一个组件。
    -   如果想要把组件放到页面中，只需把 构造函数的名称 当做 组件名称，以 HTML 标签形式引入页面中即可。
    -   构造函数首字母必须大写，否则会按普通 HTML 标签渲染

        ```js
        function MyDiv() {
          return (
            <div>
              <h1>this.is.component</h1>
            </div>
          )
        }
        ReactDOM.render(
          <div>
            <MyDiv></MyDiv>
          </div>,
          document.getElementById('app')
        )
        ```

**组件传值：**

-   父组件向子组件传递数据

    -   传值：

        -   非对象的值的传递：使用属性向子组件传值
        -   对象的传递：...obj 使用 es6 中的属性扩散, 即:简化属性的方式的操作

    -   获取：

        -   非对象的值，直接通过属性名获取
        -   对象：通过向构造函数传递形参（props）,通过 props 接收
            -   通过 props 获取的值是只读的,不能重新赋值

```js
const username = 'z...f'
const person = {
  age: 30,
  name: 'zf',
  address: 'anhui'
}
// 组件未分离
function MyDiv(props) {
  return (
    <div>
      <h1>
        this.is.component
        {username}
        {props.name}
        {props.address}
      </h1>
    </div>
  )
}
// end - 组件未分离

// 组件分离
// - mini.js
import React from 'react'
function Person(props) {
  return (
    <div>
      <h1>this.is h1 {props.name}</h1>
      <p></p>
    </div>
  )
}
export default Person
// - end - mini.js
import Person from './components/mini.jsx'
// end - 组件分离
ReactDOM.render(
  <div>
    <MyDiv {...person}></MyDiv>
    <Person {...person}></Person>
  </div>,
  document.getElementById('app')
)
```

#### 以 class 关键字的方式创建

**前提**

-   class 基本使用

    -   面向对象的语言有三部分组成：封装、继承、多态

        -   封装：class 的类、方法 都属于封装
        -   继承：子类继承父类的属性和方法
        -   多态：父类只定义 方法的名称和作用，但不定义具体的实现逻辑，由子类继承后自己实现后才调用

    ```js
    class Person {
      static info = 'this.is.person'
      constructor(name, age) {
        this.name = name
        this.age = age
      }
      say() {
        console.log('ok')
      }
    }
    class Chinese extends Person {
      // 使用 extends 实现继承，entends 的前面是子类， 后面是父类
      static cInfo = 'this.is.chinese'
      constructor(name, age, color, lang) {
        super(name, age) // 使用 extends 实现了继承，在子类的 constructor 构造函数中必须显示调用 super() 方法，super() 是父类 constructor的引用
        this.color = color
        this.lang = lang
      }
    }

    const p1 = new Person('zf', 16)
    console.log(p1)

    const c1 = new Chinese('zzzzz', 1666, 'yellow', 'english')
    console.log(c1)
    console.log(Chinese.info)
    ```

-   组件的创建

    -   使用 class 创建的类，通过 extends 继承 React.Component 之后就是一个组件模板
    -   在 class 创建的组件内部，必须有 render 函数，且必须 return 一个内容
    -   引用这个组件 把类的名称 以标签的形式，导入 jsx 中

-   组件传值

    -   与 function 传值的方式相同，使用属性的方式传递
    -   与 function 获取传值的方式不同，class 创建的类 可以在 render 函数中，直接调用 this.props.属性名 的方式获取，不需要传递 props
    -   props 与 function 一样是只读的不允许修改(**只要是 组件的 props 都是只读的**)
    -   在 constructor 中想要访问 props 需要在 constructor 构造器的参数列表中，显示定义 props 来接收
    -   父组件 向 子组件 不仅可以传递普通属性，还可以传递方法属性
        -   父组件传递：`<CmtBox reload={this.loadComments}></CmtBox>`
        -   子组件使用：`this.props.reload()`

```js
class Person extends React.Component {
  constructor(props) {
    // 在 constructor 中想要访问 props 需要在 constructor 构造器的参数列表中，显示定义 props 来接收，
    super(props)
  }
  render() {
    return (
      <div>
        <h1>{this.props.info}</h1>
        <h2>{this.props.address}</h2>
      </div>
    )
  }
}
ReactDOM.render(
  <div>
    <Person address="address" info="infoo"></Person>
  </div>,
  document.getElementById('app')
)
```

-   私有属性：this.state

    -   this.state 当前组件实例的私有数据对象，相当于 vue 组件实例的 data(){ return {}}
    -   事件绑定
        -   react 中事件绑定机制都使用的是驼峰命名（react 将传统js事件重新定义为驼峰）
        -   为 react 事件绑定 处理函数的时候，需要通过 this.函数名 进行引用。
    -   this.state 重新赋值

        -   直接重新赋值，可以修改 state 中的数值，但页面不会被更新（react 不推荐）
            -   `this.state.msg = '66666'`
        -   推荐使用 this.setState({配置对象})，为 state 重新赋值

```js
class Person extends React.Component {
  constructor() {
    this.state = {
      msg: 'this.msg1111111',
      info: '****'
    }
  }
  render() {
    return (
      <div>
        <h1>{this.props.info}</h1>
        <h2>{this.props.address}</h2>
        <h3>{this.state.msg}</h3>
        <input
          type="button"
          value="editMsg"
          id="changeMsg"
          onClick={this.changeMsg}
        />
        <br />
      </div>
    )
  }
  // react 定义: 在方法中 默认 this 指向 undefined，需要使用箭头函数改变 this 指向，指向外部的 class
  changeMsg = () => {
    // 推荐使用 this.setState({配置对象})，为 state 重新赋值
    // this.setState(),只会重新覆盖显示定义的属性值，没有提供的属性值不会被覆盖
    // this.setState({
    //   msg: '123'
    // })
    // this.setState() 也可以传递一个 function ，function 内部必须 return 一个对象
    // 在 function 中支持传递两个参数，
    // this.setState()在调用的时候是 内部异步操作
    this.setState(
      function(prevState, props) {
        console.log(prevState) // 第一个参数 prevState 修改之前的 state 的数据
        console.log(props) // 第二个参数 props 外界传递给当前组件的 props 数据
        return {
          msg: '12111'
        }
      },
      // 由于 setState 是异步执行，想要拿到最新的修改结果，需要在回调函数中操作新数据
      function() {
        console.log(this.state.msg)
      }
    )
  }
}

ReactDOM.render(
  <div>
    <Person address="address" info="infoo"></Person>
  </div>,
  document.getElementById('app')
)
```

### 无状态组件 有状态组件

-   使用 function 构造函数创建的组件，内部没有 state 私有数据，只有 一个 props 来接收外界传递过来的数据
-   使用 class 关键字 创建的组件，内部，除了有 this.props 这个只读属性之外，还有一个 专门用于 存放自己私有数据的 this.state 属性，这个 state 是可读可写的！

**无状态组件：**  使用 function 创建的组件
**有状态组件：**  使用 class 创建的组件

-   有状态组件和无状态组件，最本质的区别，就是有无 state 属性；
-   class 创建的组件，有自己的生命周期函数，function 创建的 组件，没有自己的生命周期函数；

```jsx
//#region 组件的 render 部分截取

// 在 有状态组件中， render 函数是必须的
render() {
  //#region 循环 评论列表：基础版
  /* var arr = []
  this.state.cmts.forEach(item => {
    arr.push(<h1>{item.user}</h1>)
  }) */
  //#endregion

  return <div>
    <h1 className="title">评论列表案例</h1>
    {/* 直接在 JSX 语法内部，使用 数组的 map 函数，来遍历数组的每一项，并使用 map 返回操作后的最新的数组 */}
    {this.state.cmts.map((item, i) => {
      // return <CommentItem user={item.user} content={item.content} key={i}></CommentItem>
      return <CommentItem {...item} key={i}></CommentItem>
    })}
  </div>
}

//#endregion

//#region 子组件 使用无状态组件
export default function CommentItem(props) {
  return <div>
    <h1>评论人：{props.user}</h1>
    <h3>评论内容：{props.content}</h3>
  </div>
}
//#endregion
```

### 组件的样式

###### 组件内行内的样式

-   写行内样式 外层 {} jsx语法的标志，内层的 {} 表示的是 js 对象
    -   即：行内样式书写时，值为 一个对象
    -   e.g.: `<h2 style={{ fontSize: 16, color: 'purple' }}>{props.user}</h2>`
-   style 样式规则中 若属性值的单位是 px , px可省了，直接写一个数值
-   可以将行内样式 抽离出单个 js 文件

```js
import React from 'react'
import inlineStyle from './CmtItemStyles.js'

export default function CmtItem(props) {
  return (
    // 写行内样式 外层 {} jsx语法的标志，内层的 {} 表示的是 js 对象
    // 即：行内样式书写时，值为 一个对象
    // style 样式规则中 若属性值的单位是 px , px可省了，直接写一个数值
    <div style={inlineStyle.boxStyle}>
      <h2 style={{ fontSize: 16, color: 'purple' }}>{props.user}</h2>
      <h3 style={inlineStyle.h3Style}>{props.content}</h3>
    </div>
  )
}

//#region 抽离的js文件，CmtItenStyle.js
export default {
  boxStyle: {
    border: '1px solid #eee',
    margin: '10px 0',
    paddingLeft: '15px'
  },
  h2Style: { fontSize: 16, color: 'purple' },
  h3Style: { fontSize: 14, color: 'red', fontWeight: 200 }
}
//#endregion
```

###### css 的模块化

> 在 组件内部 引入 css 样式，父组件的样式与子组件的样式会冲突

1.  启动 css 模块化

    -   修改 webpack.config.js 有关 css 的配置信息

    ```js
    {
      test: /\.css$/,
      use: [
        { loader: 'style-loader' },
        {
          loader: 'css-loader',
          options: { // 使用 css 的模块化，会对 className 类名 重命名，此配置为：优化命名名称（webpack4书写规则）
            modules: { localIdentName: '[name]-[local]-[hash:5]' }
          }
        }
      ]
    }
    ```

2.  模块化 实例

```js
import React from 'react'
// 使用此方式引入，是 css 模块化的方式， 此时的类是私有的，会将类名重新命名
import itemStyles from '../../css/commentItem.css'

export default function CmtItem(props) {
  return (
    // 将重新命名的类 与 className 衔接代替之前的 "box"
    <div className={itemStyles.box}>
      <h2 className={itemStyles.title}>{props.user}</h2>
      <h3 className={itemStyles.body}>{props.content}</h3>
    </div>
  )
}
```

```css
/*#region commentItem.css 样式表 */
.box {
  padding-left: 15px;
  margin: 10px 0;
  border: 1px solid #ccc;
  box-shadow: 0 0 6px #ccc;
}
/* 可以使用 :global() 的方式将该类全局暴露，此时的类不在私有，不会被重命名，上文中的 itemStyle.title 无效*/
/* 等同于未模块化的效果 className = "title" 即可实现效果*/
:global(.title) {
  font-size: 16px;
  color: purple;
}
.title{
  color: green;
}
.body {
  font-size: 14px;
  color: red;
}
/*#endregion */
```

### 组件的生命周期

1.  组件创建阶段：执行一次

    -   componentWillMount：将要挂载 未创建 虚拟 DOM
    -   render
    -   componentDidMount ：完成挂载

2.  组件运行阶段：根据模块组件的

    -   componentWillReceiveProps：父组件为子组件传递传递新的属性值 即触发
    -   shouldComponentUpdate：尚未被更新，state props 是最新的
    -   componentWillUpdate：尚未开始更新，虚拟DOM为旧的
    -   render
    -   componentDidUpdate：页面重新渲染，state 虚拟DOM 页面保持同步

3.  组件销毁阶段：执行一次 state 和 props 改变，触发0次或多次

    -   componentDidUpdate：组件将要被卸载，组件还可以正常使用

![React-LifeCycle.jpg](http://images.dorc.top/blog/React/React-LifeCycle.jpg)

-   defaultProps

> 在组件创建之前，会先初始化默认的props属性，这是全局调用一次，严格地来说，这不是组件的生命周期的一部分。在组件被创建并加载候，首先调用 constructor 构造器中的 this.state = {}，来初始化组件的状态。

**React生命周期的回调函数总结**

![React-LifeCycle-Table](http://images.dorc.top/blog/React/React-LifeCycle-Table.png)

**组件生命周期的执行顺序：**

-   Mounting：

    -   constructor()
    -   componentWillMount()
    -   render()
    -   componentDidMount()

-   Updating：

    -   componentWillReceiveProps(nextProps)
    -   shouldComponentUpdate(nextProps, nextState)
    -   componentWillUpdate(nextProps, nextState)
    -   render()
    -   componentDidUpdate(prevProps, prevState)

-   Unmounting：

    -   componentWillUnmount()

**实例演示生命周期：**

```jsx
//#region main.js
import React from 'react'
import ReactDOM from 'react-dom'

import Counter from './components/Counter.jsx'
import Text from './components/text.jsx'
ReactDOM.render(
  <div>
    <Counter initcount={3}></Counter>
    <Text></Text>
  </div>,
  document.getElementById('app')
)
//#endregion

//#region ./components/Counter.jsx

import React from 'react'
// 只提供常见的数据类型用于做数据校验
import ReactPropTypes from 'prop-types'

export default class Counter extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      msg: 'this.is.msg',
      count: props.initcount, // 赋值 props 的值，可操作
    }
  }
  // 在封装组件的时候，组件内部有一些数据是必须的，即使用户没有传递启动参数，组件内部需要提供默认值
  static defaultProps = {
    // 设置属性默认值
    initcount: 0,
  }
  // 把外界传递过来的属性，做类型校验
  // 做类型校验 需要安装 `npm i prop-types -S`
  static propTypes = {
    initcount: ReactPropTypes.number,
  }
  componentWillMount() {
    // => vue 中的 create
    console.log(document.getElementById('h3')) // => null
    console.log(this.props.initcount)
    console.log(this.state.msg)
    this.zfHandle()
  }
  render() {
    // 渲染虚拟DOM，但页面上尚未显示 DOM 元素
    console.log(this.refs.h3 && this.refs.h3.innerHTML)
    // 在render 中使用 setState 陷入死循环
    // this.setState({
    //   count: this.state.count + 1
    // })
    return (
      <div>
        <h1>this.is Counter</h1>
        <input
          id="btn"
          type="button"
          value="counter+1"
          onClick={this.incrementHandle}
        />
        <hr />
        <h3 id="h3" ref="h3">
          count: {this.state.count}
        </h3>
      </div>
    )
  }
  // 页面上有可见的 DOM 元素 操作 DOM 元素最早的生命周期函数 => vue 中的 mounted
  componentDidMount() {
    // document.getElementById('btn').onclick = () => {
    //   this.setState({
    //     count: this.state.count + 1
    //   })
    // }
  }
  // 执行完 componentDidMount 后进入运行中的状态
  shouldComponentUpdate(nextProps, nextState) {
    // 必须返回 Boolean
    // 返回 false 不会执行后续的生命周期函数，直接退回了运行中的状态，此时页面不会被更新，但 state  的值被修改了
    // shouldComponentUpdate中通过this.state 拿到的值 是上一次旧的数据，不是最新的
    // 最新的值 使用参数列表中的 参数 使用
    console.log(this.state.count + '----' + nextState.count)
    // return nextState.count % 2 === 0 ? true : false
    return true
  }
  // 即将更新，尚未更新 内存中的虚拟 DOM 是旧的，页面上的 DOM 也是旧的
  componentWillUpdate() {
    // console.log(document.getElementById('h3').innerHTML)
    // react 建议使用 ref 获取界面元素
    console.log(this.refs.h3.innerHTML)
  }

  componentDidUpdate() {
    // 此时 state中的 数据，虚拟DOM，页面上的DOM都是最新的
    console.log(this.refs.h3.innerHTML)
  }

  zfHandle() {
    console.log('zfHandle')
  }

  incrementHandle = () => {
    this.setState({
      count: this.state.count + 1,
    })
  }
}
//#endregion

//#region ./components/text.jsx
import React from 'react'

export default class Parent extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      msg: 'parentmsg',
    }
  }

  render() {
    return (
      <div>
        <h1>parents</h1>
        <input
          type="button"
          value="click modify MSG"
          onClick={this.changeMessageHandle}
        />
        <hr />
        <Son pmsg={this.state.msg}></Son>
      </div>
    )
  }

  changeMessageHandle = () => {
    this.setState({
      msg: 'modify msg',
    })
  }
}

class Son extends React.Component {
  constructor(props) {
    super(props)
    this.state = {}
  }

  render() {
    return (
      <div>
        <h3>son,this.is.pmsg : {this.props.pmsg}</h3>
      </div>
    )
  }

  // 组件将要接收，外界传的新的 props 的值
  // 子组件第一次渲染到页面上不会触发
  // 只有父组件通过事件修改了传递给子组件的 props 值，才触发事件
  // 触发时 this.props 获取属性值，不是最新的，是上一次旧的属性值，需要获取最新的属性值，需要通过参数列表获取
  componentWillReceiveProps(nextProps) {
    console.log(this.props.pmsg)
    console.log(nextProps.pmsg)
  }
}
//#endregion
```

### 组件的事件绑定

**在事件处理函数中直接使用 bind 绑定 this 并传参**

1.  bind 的基本使用：修改函数内部 this 的指向，指向 bind 参数列表中的第一个参数
2.  bind 传参：bind 中的第一个参数是修改 this 的指向，之后所有的参数，都会当做将要调用该函数的参数
    -   call/apply 修改完 this 指向会立即调用，bind 不会
    -   `onClick={this.changeMsgHandle.bind(this, 'params1', 'params2')}`
    -   此时,使用 `changeMsgHandle(arg1,arg2){}` 定义函数，函数内部 this 指向为组件

**在构造函数 bind 绑定 this 并传参 (全局)**

1.  函数调用 bind 改变 this 的指向后，bind 函数调用的结果 返回的是被改变 this 指向后的函数的引用，bind 不会修改原函数 this 的指向
    -   在 constructor 构造器中：
    -   `this.changeMsgHandle2 = this.changeMsgHandle2.bind(this,'parmas3','parmas4')`

### context

作用：在父组件直接共享一个 context 对象，子孙组件就不需要逐层传递数据了

使用：

1.  在父组件中，定义一个 function ,有一个固定名称 getChildContext ,内部必须返回一个对象，这个对象就是要共享给所有子孙自建的数据
2.  使用属性校验，规定传递给子组件的数据类型，需要定义一个静态的（static）childContextTypes(固定写法)
3.  在子孙组件中，先属性校验，

> 记住一串单词组合`getChildContextTypes` 前3个、后3个、后两个 => 一个方法、两个静态属性

```jsx
import React from 'react'
import ReactPropTypes from 'prop-types'

export default class Parent extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      color: 'red',
    }
  }
  render() {
    return (
      <div>
        <Son></Son>
      </div>
    )
  }
  getChildContext() {
    return {
      color: this.state.color,
    }
  }
  static childContextTypes = {
    color: ReactPropTypes.string, // 传递给子组件的数据类型
  }
}
class Son extends React.Component {
  render() {
    return (
      <div>
        <h3>son</h3>
        <Grandson></Grandson>
      </div>
    )
  }
}
class Grandson extends React.Component {
  // context 共享的数据 必须属性校验
  static contextTypes = {
    color: ReactPropTypes.string,
  }

  render() {
    return (
      <div>
        <h5>grandson --- {this.context.color}</h5>
      </div>
    )
  }
}
```

## React 路由

### 起步

1.  安装
    -   `npm install react-router-dom -S`
2.  基本使用：按需导入

    -   `import { HashRouter, Route, Link } from 'react-router-dom'`
        -   HashRouter：路由的根容器，一个网站只是用一次
            -   当使用 HashRouter 把 APP 根组件元素包裹起来后，网站就已启用路由
            -   HashRouter 内部只能有为唯一一个 根元素
        -   Route：路由规则，其中有两个重要的属性：
            -   path / component 作用类似 vue 中的 route 的配置
            -   Route 标签的作用 等价于 vue 中的 Router-view 标签的作用（路由‘占位符’）
        -   Link：路由链接 => vue 中的 `<router-link to=""></router-link>`

    ```jsx
    import React from 'react'
    import { HashRouter, Route, Link } from 'react-router-dom'

    import Home from './components/Home.jsx'
    import About from './components/About.jsx'
    import Contact from './components/Contact.jsx'

    export default class App extends React.Component {
      constructor(props) {
        super(props)
        this.state = {}
      }
      render() {
        return (
          <HashRouter>
            <div>
              <h1>App.jsx</h1>
              <hr />
              <Link to="/home">home</Link>&nbsp;&nbsp;
              <Link to="/about">about</Link>&nbsp;&nbsp;
              <Link to="/contact">contact</Link>&nbsp;&nbsp;
              <hr />
              <Route path="/home" component={Home}></Route>
              <Route path="/about" component={About}></Route>
              <Route path="/contact" component={Contact}></Route>
            </div>
          </HashRouter>
        )
      }
    }
    ```

3.  路由传参

    -   默认情况下路由是模糊匹配的，路由部分匹配成功，就展示路由对应的组件，开启精确匹配，添加属性 exact
    -   匹配参数，可以在匹配规则中使用 `:` 修饰符，表示匹配参数
        -   `<Route path="/about/:type/:id" component={About} exact></Route>`
    -   在路由模块中匹配参数使用：`this.props.match.params.参数名`

4.  编程式导航

```jsx
pageChangedHandle = (page) => {
  // 使用 react-router-dom 实现 编程式导航
  this.props.history.push('/movie/' + this.state.movieType + '/' + page)
  // window.location.href = '/#/movie/' + this.state.movieType + '/' + page
}
```

## React fetch

> 在 React 中，使用 fetch API 获取数据（基于 promise 封装）

```jsx
componentWillMount() {
  // 使用 fetch API 第一个 .then 获取不到数据，拿到的是 response 对象
  fetch('https://douban.uieee.com/v2/movie/in_theaters')
    .then((response) => return response.json())
    .then((data) => console.log(data))
}
```

> 默认的window.fetch 受到跨域限制无法使用，跨域时使用 fetch-jsonp ，用法与浏览器内置的 fetch 完全兼容

安装：`npm i fetch-jsonp -S`

# Ant Design

1.  安装：`npm install antd -S`

2.  使用前配置， 取消对 css 的模块化，开启对 scss 的模块化

    -   原因：一般情况下，第三方UI组件都是以 .css 结尾

3.  按需导入：
    -   安装插件：`npm i babel-plugin-import -D`
    -   在.babelrc 的 plugins 中添加 `["import", { "libraryName": "antd", "libraryDirectory": "es", "style": "css"}]`
    -   然后只需从 antd 引入模块即可，无需单独引入样式

# ReactNative

## 移动 App 开发环境配置

### 安装 Java jdk 1.8 (开发网站，客户端的环境)

**配置环境变量**

-   新建系统环境变量 `JAVA_HOME` , 值为：`C:\Program Files\Java\jdk1.8.0_121` (jdk 的根目录)

-   修改系统环境变量 `Path` , 新增：`%JAVA_HOME%\bin` 、 `%JAVA_HOME%\jre\bin`

-   新建系统环境变量 `CLASSPATH` , 值为：`.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;`

-   验证：命令行中输入 `javac`

### 安装 Node.js

### 安装 C++

### 安装 Git

### 安装 Python 2.7

> 注意勾选安装界面上的 Add Python to path , 验证：命令行中输入 `python`

### 配置安卓环境

1.  安装 SDK

    -   勾选：`Install for anyone using this computer`

    -   修改路径 `C:\Android`

2.  打开 `C:\Android\platforms` ，将 `android-23`(react-native 依赖) 、 `android-24` 、 `android-25` 解压后，放到该文件夹下

3.  打开 `C:\Android` , 将 `platform-tools` 解压后，放到该文件夹下

4.  配置 `build-tools`

    -   打开 `C:\Android` , 新建 `build-tools` 文件夹
    -   将
        -   `build-tools_r23.0.1-windows.zip` (react-native 依赖) ,
        -   `build-tools_r23.0.2-windows.zip` (weex 依赖) ,
        -   `build-tools_r23.0.3-windows.zip` 分别解压后，
    -   分别改名为 `23.0.1` , `23.0.2` , `23.0.3` , 放到该文件夹下

5.  打开 `C:\Android` , 新建 `extras` 文件夹，在 `extras` 文件夹下新建 `android` 文件夹；解压 `m2responsitory` 文件夹和 `support` 文件夹，放到新建的 `C:\Android\platforms\extras\android` 下

6.  配置环境变量

    -   新建系统环境变量 `ANDROID_HOME` , 值为：`C:\Android` (SDK 的根目录)

    -   修改系统环境变量 `Path` , 新增：`%ANDROID_HOME%\tools` 、 `%ANDROID_HOME%\platform-tools`

### 安装 yarn react-native-cli

-   `npm install -g yarn react-native-cli`

-   设置 yarn 淘宝镜像：
    -   `yarn config set registry https://registry.npm.taobao.org -g`
    -   `yarn config set disturl https://npm.taobao.org/dist --global`

### react native 快速打包

1.  初始化项目 ：`react-native init projectname`

2.  **切换到项目目录**, 将手机连接电脑, 运行 `adb devices` , 确认手机是否连接

3.  `react-native run-android` , 设备上安装并启动应用

4.  会新增一个新的 node 窗口，此窗口为 ReactNative Package 窗口，作用：实时编译项目源代码，并把编译的结果，应用到手机，随时查看最新的项目代码 效果

    -   实时打包编译代码的功能是个服务，运行中在本机8081端口
    -   若误关闭窗口、编译时报错，可以在项目根目录中运行 `react-native start` 来重启 ReactNative Package

5.  在项目根目录运行命令行工具，使用 `adb shell input keyevent 82` 可在手机端弹出 reload 选项

[Weex 快速打包](http://weex.apache.org/cn/guide/tools/toolkit.html)

1.  安装依赖:Weex 官方提供了 weex-toolkit 的脚手架工具来辅助开发和调试。首先，你需要最新稳定版的 Node.js 和 Weex CLi。
2.  运行`npm install -g weex-toolkit`安装 Weex 官方提供的 `weex-toolkit` 脚手架工具到全局环境中
3.  运行`weex create project-name`初始化 Weex 项目
4.  进入到项目的根目录中，打开 cmd 窗口，运行`weex platform add android`安装 android 模板，首次安装模板时，等待时间较长，建议 fq 安装模板
5.  打开`android studio`中的`安卓模拟器`，或者将`启用USB调试的真机`连接到电脑上，运行`weex run android`，打包部署 weex 项目
6.  部署完成，查看项目效果

## ReactNative 起步

-   基于 react 框架语法进行开发
-   RN 提供了移动端专用的一些组件，网页中的 div p img 不能使用，只能使用 RN 固有组件
-   结合：安卓的 签名打包步骤，并使用 RN 提供的打包命令，进行完整 apk 文件的发布，最终发布出来 Release 版本的项目，可以上传到应用商店；

1.  在 RN 中只能使用 .js 不能使用 .jsx
2.  RN 常用组件：

    -   布局：View
    -   文本：Text（文本必须使用text，否则报错）
    -   文本框：TextInput
    -   图片：Image (引入网络中的图片指定加宽高)

        ```js
        <Image source={require('./images/text.jpg')}></Image>
        <Image
          style={{width: 64, height: 64}}
          source={{
            uri: 'http://images.dorc.top/blog/blog-logo.png',
          }}></Image>
        ```

    -   按钮：Button
        -   title：按钮内的文本
        -   onPress：点击按钮触发的操作
    -   加载中：ActivityIndicator
    -   ScrollView（一次渲染全部）：在 RN 中，页面超出屏幕宽度，不会自动提供滚动条，实现页面滚动要使用 SrollView 包裹
    -   FlatList （渲染可见部分，不可见的用空白代替）： ListView 的升级版


3.  创建基本RN页面步骤：
    -   导入 react 包 创建组件
    -   导入 react-native 包中的 开发组件进行开发、StyleSheet 组件，调用 create 方法创建样式对象
        -   只能使用 RN库 的提供固有组件，不能使用网页中的任何元素
    -   使用 react 语法创建出来的组件就是合法的 RN 组件页面

## 项目

### react-navigation 使用

1.  安装

```bash
# 安装包
npm install @react-navigation/native

# 安装依赖
npm install react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view

# 按需安装相应组件
# tabbar
npm install @react-navigation/bottom-tabs
```

2.  项目中导入

`import 'react-native-gesture-handler';`

```js
//#region 导包部分
// react navigation 相关导入
import 'react-native-gesture-handler';
import React from 'react';

import {NavigationContainer} from '@react-navigation/native';
import {createBottomTabNavigator} from '@react-navigation/bottom-tabs';
const Tab = createBottomTabNavigator();

// 图标库
import Ionicons from 'react-native-vector-icons/AntDesign';
//#endregion

//#region 渲染部分
<NavigationContainer
  initialRouteName="Home"
  tabBarOptions={{
    activeTintColor: '#e91e63',
  }}>
  <Tab.Navigator>
    <Tab.Screen
      name="Home"
      component={Home}
      options={{
        tabBarLabel: 'Home',
        tabBarIcon: ({color, size}) => (
          <Ionicons name="home" color={color} size={size} />
        ),
      }}
    />
    <Tab.Screen
      name="Me"
      component={Me}
      options={{
        tabBarLabel: 'Me',
        tabBarIcon: ({color, size}) => (
          <Ionicons name="user" color={color} size={size} />
        ),
      }}
    />
  </Tab.Navigator>
</NavigationContainer>
//#endregion
```

### react-native-vector-icons 图标库使用

1.  安装

    -   `npm install --save react-native-vector-icons`

2.  配置

        -   自动关联：`react-native link react-native-vector-icons`
        -   配置：

            ```js
            // 进入android/app/build.gradle文件，添加如下内容：
            project.ext.vectoricons = [
                iconFontNames: [ 'MaterialIcons.ttf', 'EvilIcons.ttf' ]
            ]
            apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"

            // 项目中使用
            import Icon from 'react-native-vector-icons/FontAwesome';
            // 注意不同的库名不同按需导入，库名对应下面图标名
            <Icon name={'angle-right'} size={24} color={'#999'} />
            ```

### 产品发布

-   命令行运行
    -   `keytool -genkey -v -keystore app.keystore -alias zhoufeng -keyalg RSA -keysize 2048 -validity 10000`
-   把 app.keystore 文件放到项目文件中的 android/app 文件夹下。
-   打开项目目录下的 android/app/build.gradle 文件，添加如下的签名配置：

```gradle
...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            keyAlias 'yingqi' //别名
            keyPassword '123456' //密钥密码 之前设置秘钥口令
            storeFile file('app.keystore') //my-release-key.keystore文件的绝对路径
            storePassword '123456' //存储密码
        }
    }
    buildTypes {
        release {
            ...
            minifyEnabled enableProguardInReleaseBuilds // 在 当前文件中，找到变量 enableProguardInReleaseBuilds ,将其值修改为 ue
            signingConfig signingConfigs.release // 引用签名
        }
    }
}
...
```

-   修改应用名称：打开项目目录下的android/app/src/main/res/values/strings.xml文件，修改名称

```xml
<resources>
    <string name="app_name">豆瓣</string>
</resources>
```

-   修改应用 icon : 将如下文件夹中的 icon 替换成需要修改的图标即可,注意icon大小保持一致

    -   `E:\answer_zf\code\doubanRN\android\app\src\main\res` 下的 `mipmap-***` 文件夹中

-   打包命令：进入根目录下的 Android 中，运行`./gradlew assembleRelease`

    -   Release 发行版存放的目录：`E:\answer_zf\code\doubanRN\android\app\build\outputs\apk\release`
