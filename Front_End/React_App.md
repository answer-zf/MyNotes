# React_App

## 环境安装

> 使用 create-react-app 安装 React app

1. 创建项⽬：

    - `npx create-react-app my-app`

2. 打开项⽬：

    - `cd my-app`

3. 启动项⽬：

    - `npm start`

4. 初始化 git:

    - `git add .`
    - `git commit -m "init"`

5. 暴露配置项：

    - `npm run eject`

## 前提 JSX

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
// import './index.css';
import styles from './index.module.css'; // .app .logo { width: 100px; }  (css模块化)
import logo from './logo192.png';

const name = 'this.is zf';

const obj = {
  foo: 'bar',
  foz: 'baz',
};

const formatName = (name) => {
  return name.foo + ',' + name.foz;
};

const greet = <div>zf</div>;

const show = true; // 条件示例

const a = [1, 2, 3]; // 循环示例

const jsx = (
  <div className={styles.app}>
    hello {name}
    <div>{formatName(obj)}</div>
    {greet}
    {show ? greet : 'login'}
    {show && greet}
    <ul>
   {a.map((item) => (
    <li key={item}>{item}</li>
   ))}
  </ul>
  <img
      src={logo}
      alt="logo"
      // className="logo"
      className={styles.logo} // css模块化
      // style={{ width: '50px', height: '50px' }} 内联写法，使用对象表示
  />
 </div>
);

ReactDOM.render(jsx, document.getElementById('root'));
```

## react 基础

### 组件

#### class 组件

- 在 class 组件中 有 state 状态，处理数据 类似于 vue 中的 data
- 修改状态 需使用 this.setState() 做修改，传参为 需要修改的数据对象

```jsx

import React, { Component } from 'react'

class ClassPage extends Component {
  constructor(props) {
    super(props)
    this.state = {
      zf: 'zzz',
      date: new Date(),
    }
  }

  // 组件 挂载完毕之后
  componentDidMount() {
    this.timer = setInterval(() => {
      this.setState({
        date: new Date(),
      })
    }, 1000)
  }

  // 组件即将卸载时
  componentWillUnmount() {
    // 停止定时器
    clearInterval(this.timer)
  }

  render() {
    const { zf, date } = this.state
    return (
      <div style={{ width: '500px', margin: 'auto' }}>
        <h4> this is class </h4>
        <p>{zf}</p>
        <p>{date.toLocaleTimeString()}</p>
      </div>
    )
  }
}

export default ClassPage

```

#### function 组件

- react v16.8后 使用 hook api 中的 useState() 做状态的保持
- useState() 返回一个数组
  - 数组的第一个成员：一个变量
  - 数组的第二个成员：一个函数，用来更新状态。约定是set前缀加上状态的变量名

- useEffect() 副作用
  - 即 class 组件中的 componentDidMount
  - useEffect() 第一个参数：一个函数，所需要执行的操作
  - useEffect() 第二个参数：如果不希望useEffect()每次渲染都执行，使用它的第二个参数，使用一个数组指定副效应函数的依赖项，只有依赖项发生变化，才会重新渲染
  - useEffect() 允许返回一个函数，在组件卸载时，执行该函数，清理副效应。如果不需要清理副效应，useEffect()就不用返回任何值。

```jsx
// 父组件 
<FunctionPage parent="this is father" />

// 子组件
import React, { useEffect, useState } from 'react'

const FunctionPage = ({ parent }) => {
  let [count, setCount] = useState(10)

  useEffect(() => {
    const timer = setInterval(() => {
      setCount(count => count + 1)
    }, 1000)
    console.log('object')
    return () => {
      clearInterval(timer)
    }
  }, [])
  // 第二个参数数字包含的时依赖项,
  // [parent]由于parent是定值, useEffect 不会重复执行
  // 当为[parent, count],useEffect 1s执行一次
  // []即不绑定依赖 useEffect 就执行一次

  return (
    <div>
      <h5>this is function page</h5>
      <p>{count}</p>
      <p>{parent}</p>
    </div>
  )
}
```

##### 自定义hook

- 自定义 Hook，命名以 use 开头
- 只能在函数最外层调⽤ Hook (不能在 if for function 中 调用)
- 只能在 React 的函数组件中调⽤ Hook
- vue3 和 react 的hook 写法以及在写法中的理念差不多，可在hook中添加生命周期

```jsx
import React, { useEffect, useState } from 'react';

function CustomHookPage(props) {
  return (
    <div>
      <h3>CustomHookPage</h3>
      <p>{useClock().toLocaleTimeString()}</p>
    </div>
  );
}

export default CustomHookPage;

function useClock() {
  const [date, setdate] = useState(new Date());
  useEffect(() => {
    const timer = setInterval(() => {
      setdate(new Date());
    }, 1000);
    return () => clearInterval(timer);
  }, []);
  return date;
}
```

### 生命周期

- 已废除的三个生命周期
  - componentWillMount
  - componentWillReceiveProps
  - componentWillUpdate

#### 挂载时

1. constructor 构造器
2. getDerivedStateFromProps (静态)
3. render
4. componentDidMount

#### 更新时

1. getDerivedStateFromProps (新增,静态)
2. shouldComponentUpdate
3. render
4. getSnapshotBeforeUpdate (新增,非静态)
5. componentDidUpdate

#### 卸载

componentWillUnmount

#### 静态 生命周期使用

`static getDerivedStateFromProps(nextProps, prevState) { }`

#### getSnapshotBeforeUpdate componentDidUpdate 协同

```jsx
getSnapshotBeforeUpdate(prevProps, prevState){
  console.log('getSnapshotBeforeUpdate');
  return '这是快照传来的'
}

// snapshotValue 是上面 return 的值
componentDidUpdate(prevProps, prevState,snapshotValue){
  console.log('componentDidUpdate');
}
```

### 组件复合

- 模板组件 Layout
  - 模板使用的头部组件 TopBar
  - 模板使用的底部组件 BotBar
- 示例组件:
  - 用户页面 UserPage
  - 首页 HomePage

```jsx
// Layout
import BotBar from './BotBar'
import TopBar from './TopBar'

const Layout = props => {
  const { children, topBar, botBar } = props
  return (
    <div>
      {topBar && <TopBar />}
      {children.content}
      {botBar && <BotBar />}
    </div>
  )
}

export default Layout

// UserPage
import Layout from './Layout/Layout'

const UserPage = () => {
  return (
    <Layout topBar={true} botBar={true}>
      {{ content: <div>this is userpage</div> }}
    </Layout>
  )
}
export default UserPage
```

### Redux

> 安装 npm i redux -S

1. 使用createStore 创建 store,入参为reducer函数
   - reducer 函数入参 state(一个应用只能有一个)，action(操作)
2. 引入store，使用store.getState()获取数据，
3. 使用 store.dispatch() 做事件派发，入参为对象，type:操作方式，payload:携带信息
4. 使用 store.subscribe(()=>{this.forceUpdate()}) 更新视图并刷新。

注意点：

- 由于 reducer 是纯函数，不能修改state数据，只能返回一个全新的对象，state是只读的

```jsx
// store/index.js
import { createStore } from 'redux'

const dateReducer = (state = 0, action) => {
  console.log(state)
  switch (action.type) {
    case 'ADD':
      return state + 1 + action.payload

    default:
      return state
  }
}

const store = createStore(dateReducer)
export default store
```

```jsx
import React, { Component } from 'react'
import store from '../../store'

export default class ReduxOrigin extends Component {
  componentDidMount() {
    store.subscribe(() => {
      console.log('this.change')
      this.forceUpdate()
    })
  }

  addHandle = () => {
    const actionCreator = (type, payload) => {
      return {
        type,
        payload,
      }
    }
    store.dispatch(actionCreator('ADD', 3))
  }

  render() {
    console.log(store.getState())
    return (
      <div>
        <h5>ReduxOrigin</h5>
        <p>{store.getState()}</p>
        <button onClick={this.addHandle}>add</button>
      </div>
    )
  }
}
```

#### React-Redux

> 安装 npm i react-redux -S

- Provider 为后代组件提供 store，并且封装了对store.subscribe()进行了封装，子代都可以实时更新视图
- connect 用于从 UI 组件生成容器组件
  - UI组件
    - 只负责 UI 的呈现，不带有任何业务逻辑
    - 没有状态（即不使用this.state这个变量）
    - 所有数据都由参数（this.props）提供
    - 不使用任何 Redux 的 API
  - 容器组件
    - 负责管理数据和业务逻辑，不负责 UI 的呈现
    - 带有内部状态
    - 使用 Redux 的 API
  - 简而言之：UI 组件负责 UI 的呈现，容器组件负责管理数据和逻辑。
  - 第一个参数 为 mapStateToProps 函数，执行后应该返回一个对象
  - 第二次参数 为 mapDispatchToProps
    - 对象，该对象的每个键值对都是一个映射
    - 也可以为函数，返回对象，会得到dispatch和ownProps（容器组件的props对象）两个参数。

```jsx
<Provider store={store}>
  <ReactReduxPage />
</Provider>
```

```jsx
import React, { Component } from 'react'

import { connect } from 'react-redux'

export default connect(
  // mapStateToProps 把 state 映射到 props
  state => ({ data: state }),
  // mapDispatchToProps 把 dispatch 映射到 props
  { add: () => ({ type: 'ADD', payload: 90 }) }
)(
  class ReactReduxPage extends Component {
    render() {
      console.log(this.props)
      const { data, add } = this.props
      return (
        <div>
          <h4>ReactReduxPage</h4>
          <p>{data}</p>
          <button onClick={add}>add</button>
        </div>
      )
    }
  }
)
```

#### context

- 在 react 中，有 React Context API，可以做跨层级状态管理
- 逻辑接近Redux的逻辑
- 是一个高阶组件

```jsx
// 初始状态 即 默认状态
const initialState = {
  inputValue: ""
};

// 创建 context
const Context = React.createContext(initialState);

// 做 状态（数据）的分发，在value属性中赋值，value中的赋值会覆盖数据
export const Provider = Context.Provider;
// 做 状态的接收
export const Consumer = Context.Consumer;
```

```jsx
// 祖先组件做provider 的状态分发，value 进行赋值
import React, { Component } from 'react'
import { Provider } from './orderContext'
import OrderPage from './OrderPage'

class ContextPage extends Component {
  render() {
    return (
      <div>
        <h3>ContextPage</h3>
        {/* 赋值完毕后，默认状态的 inputValue 没了，即 会生成有新的内存地址的对象 */}
        <Provider value={{ orderNum: 222 }}> 
          <OrderPage />
        </Provider>
      </div>
    )
  }
}

export default ContextPage
```

```jsx
// 后代组件 用 consumer 做状态的接收
import React, { Component } from 'react'
import { Consumer } from './orderContext'
import UserPage from './UserPage'

class OrderPage extends Component {
  render() {
    return (
      <div>
        <Consumer>
          {value => (
            <div>
              <p>{value.orderNum}</p>
            </div>
          )}
        </Consumer>
        <UserPage />
      </div>
    )
  }
}

class UserPage extends Component {
  render() {
    return (
      <div>
        <Consumer>
          {({ orderNum }) => (
            <div>
              userpage
              <p>{orderNum}</p>
            </div>
          )}
        </Consumer>
      </div>
    )
  }
}

export default OrderPage
```

#### 多个 context 嵌套

- 前提准备的 context

```jsx
// 分别再两个 文件存放 的 context
import React from 'react'
const orderContext = React.createContext()
export const OrderProvider = orderContext.Provider
export const OrderConsumer = orderContext.Consumer

import React from 'react'
const UserContext = React.createContext()
export const UserProvider = UserContext.Provider
export const UserConsumer = UserContext.Consumer
```

```jsx
// 祖先组件
import React, { Component } from 'react'
import { OrderProvider } from './orderContext'
import OrderPage from './OrderPage'

class ContextPage extends Component {
  render() {
    return (
      <div>
        <h3>ContextPage</h3>
        <OrderProvider value={{ orderNum: 222 }}>
          <OrderPage />
        </OrderProvider>
      </div>
    )
  }
}

export default ContextPage
```

```jsx
// 子代组件
import React, { Component } from 'react'
import { OrderConsumer } from './orderContext'
import { UserProvider } from './userContext'
import UserPage from './UserPage'

class OrderPage extends Component {
  render() {
    return (
      <div>
        <OrderConsumer>
          {value => (
            <div>
              <p>{value.orderNum}</p>
            </div>
          )}
        </OrderConsumer>
        <UserProvider value={{ userNum: 567 }}>
          <UserPage />
        </UserProvider>
      </div>
    )
  }
}

export default OrderPage
```

```jsx
// 子孙组件
import React, { Component } from 'react'
import { OrderConsumer } from './orderContext'
import { UserConsumer } from './userContext'

class UserPage extends Component {
  constructor(props) {
    super(props)
    this.state = {}
  }
  render() {
    return (
      <div>
        <OrderConsumer>
          {({ orderNum }) => (
            <UserConsumer>
              {({ userNum }) => (
                <div>
                  userpage
                  <p>orderNum:{orderNum}</p>
                  <p>userNum:{userNum}</p>
                </div>
              )}
            </UserConsumer>
          )}
        </OrderConsumer>
      </div>
    )
  }
}

export default UserPage
```

### Router

> 安装 npm i -S react-router-dom

- Route渲染内容的三种方式
  - Route渲染优先级：children > component > render
- 编程式跳转，用 `this.props.history.push('/')`
  - 如果 this.props 获取不到history ,使用 witchRouter 对组件做层包裹即（`export default witchRouter(ComponentName)`）
    - 前提，组件不能包含 Router (BrowserRouter) 组件，下面的示例不适用 witchRouter.一般 Router包裹在App的外层。

```jsx
import React, { Component } from 'react'
import { BrowserRouter as Router, Route, Link, Switch } from 'react-router-dom'
import Home from './Home'
import Order from './Order'
import User from './User'

export default class RouterPage extends Component {
  constructor(props) {
    super(props)
    this.state = {}
  }

  render() {
    return (
      <div>
        <h4>router page</h4>

        <Router>
          <Link to="/">home</Link>
          <Link to="/user">user</Link>
          <Link to="/order">order</Link>
          <Switch>
            {/* switch 做唯一匹配（只显示匹配到的第一个路由） */}
            {/* Route 中 exact 做精确匹配 */}
            <Route
              exact
              path="/" // 优先级最高
              // children={() => <div>children</div>}
              // 优先级其次
              // component={HomePage}
              render={() => <div>Render</div>}
            />
            <Route path="/user" component={User} />
            <Route path="/order" component={Order} />
          </Switch>
        </Router>
      </div>
    )
  }
}
```

### Component 、PureComponent

- PureComponent 为 React16 新增的类
- 相比较Component，PureComponent 自定义了shouldComponentUpdate()
  1. Component 组件中，父组件 state 发生变化，子组件会重新渲染
     - 优化方案：子组件用 PureComponent代替Component，或者使用：shouldComponentUpdate(nextProps,nextState){return ...}
  2. Component 组件中，一个点击事件，修改state为一个定值，后续继续点击（无效点击）仍会重新渲染组件，不利于优化
     - 优化方案：

      ```jsx
        // count 为state 中修改的定值
        shouldComponentUpdate(nextProps, nextState) {
          return nextState.count !== this.state.count;
        }
        // 优化方案2. 用PureComponent做代替。
      ```

- 综上 PureComponent 是一种性能优化方案，但是有两个需要注意的点

  1. 只能在 class 中使用
  2. PureComponent 做的 state props 为浅比较
     - 在 state 中修改 对象，出现无效点击，页面重新渲染
     - 用 setState 的第二种写法，即 入参为 state的回调，返回一个对象

      ```jsx
        this.setState(state => {
          state.count = 100
          console.log(state)
          return state
        })
        // state 数据发生了改变，但是页面不渲染，
        // 即shouldComponentUpdate 判断为相同的数据，state可以当作一个对象，里面的元素改变，引用没变。
      ```

- 个人觉得 state 绑定简单数据，而且使用 setState()普通写法，用 PureComponent
- 其他情况都用 Component，用 shouldComponentUpdate 做性能优化。

### memo、useMemo 、useCallback

> memo 功能内 类似 PureComponent 的功能，在hook中使用，同样也是浅比较

- 父组件跟新 state ,传递给 子组件state中的数据 没有变，但会对子组件重新渲染

```jsx
import { memo, useState } from 'react'

const Child = memo(({data}) =>{
    console.log('child render...', data.name)
    return (
        <div>
            <div>child</div>
            <div>{data.name}</div>
        </div>
    );
})

const Test =()=>{
    console.log('Hook render...')
    const [count, setCount] = useState(0)
    const [name, setName] = useState('rose')

    // 点击按钮 Effect组件会render，一旦render， 执行到此
    // 这一行代码会生成有新的内存地址的对象，就算带着 memo的Child组件，也会跟着重新render （memo的浅比较）
    // 产生性能浪费
    const data = { name } 

    // 使用 useMemo
    // render的时候，就会先根据[name]里面的name值判断一下，
    // 因为useMemo 作为一个有着暂存能力的，暂存了上一次的name结果
    // 与上次的name比较后，没发生变化 ，那么这data不会重新赋值成新的对象
    // 子组件便不会重新渲染
    const data = useMemo(()=>{
        return { name } 
    },[name])

    return(
        <div>
            <div>
                {count}
            </div>
            <button onClick={()=>setCount(count+1)}>update count </button>
            <Child data={data}/>
        </div>
    )
}
```

- userMemo
  - 第一个参数是`()=>value`
    - 如果 value是函数 ，第一个参数要写成 `useMemo(()=>(x)=>console.log(x))`
    - 或者使用 useCallback `useCallback(x=>console.log(x)，[m])`
  - 第二个参数是依赖`[m,n]`
  - 只有当依赖变化时，才会重新计算出新的value
  - 如果依赖不变，那么就重用之前的value

- useCallback 是 userMemo 的语法糖

```jsx
// 组件传值(子组件-> 父组件)以及useMemo 返回函数的示例
import React, { useState, memo, useMemo, useCallback } from 'react'

const UseMemoPage = () => {
  const [num1, setNum1] = useState(1)
  const [num2, setNum2] = useState(2)

  const handleClick1 = () => {
    setNum1(num1 + 1)
  }

  const handleClick2 = () => {
    setNum2(num2 + 2)
  }

  // const childHandle = () => {
  //   // 这种写法，子组件依据渲染
  //   console.log('子组件点击事件', num1)
  // }

  // const childHandle = useMemo(
  //   // 写法1
  //   // () => {
  //   //   const fn = e => {
  //   //     console.log('子组件点击事件', num1)
  //   //     console.log(e)
  //   //   }
  //   //   return fn
  //   // },
  //   // [num1]
  //   // 写法2
  //   () => div => {
  //     console.log('子组件点击事件', num1)
  //     console.log(div)
  //   },
  //   [num1]
  // )

  // 写法3
  const childHandle = useCallback(
    div => {
      console.log('子组件点击事件', num1)
      console.log(div)
    },
    [num1]
  )

  return (
    <div>
      <h5>useMemoPage</h5>
      <p>num1:{num1}</p>
      <p>num2:{num2}</p>
      <button onClick={handleClick1}>change1</button>
      <button onClick={handleClick2}>change2</button>

      <div>-----</div>

      <Child1 data={num1} onClick={childHandle} />
    </div>
  )
}

const Child = props => {
  console.log('子组件渲染')
  return (
    <div>
      this.is child
      <button
        onClick={e => {
          props.onClick(e.target)
        }}
      >
        {props.data}
      </button>
    </div>
  )
}
const Child1 = memo(Child)

export default UseMemoPage
```

## umi

### 安装

1. 创建项目文件夹
2. `yarn create umi`

   1. `ant-design-pro`
   2. `pro v4`
   3. `js`
   4. `simple`(精简版)

### 基本使用

1. 创建页面

   - `umi g page about/index --less`

     - 在 pages 目录下创建 `index.js` `index.less` 两个文件

2. 配置 基础路由

   - 在 `/config/routes.js` 文件中
   - `[0].routes.[1].routes` 中添加路由
     - `{path: '/about',component: './about/index'}`

3. 配置 侧边栏

   - 在上一步添加的路由配置中，再添加 `name`,`icon` 两个字段即可
   - 即：`{ name: 'more', icon: 'table', path: '/about', component: './about/index' }`
     - `name` 字段需要达到国际化响应的效果
     - 在 `/locales/zh-CN/menu.js` 中 添加 `menu. + 自定义的name值`即可
     - 即 `'menu.about': '关于',`

### 约定式路由

1. 配置

   - 动态传参：
     - `umi g page product/[id] --less`
     - `umi g page product/index --less`
       - `/config/routes.js` 添加
         - `{ path: '/product/:id', component: './product/[id]' },` (必须传参)
         - `{ path: '/product/:id', component: './product/index' },`

   - 可选动态传参：
     - `umi g page productDetail/[id$] --less`
     - `umi g page productDetail/index --less`
       - `/config/routes.js` 添加
         - `{ path: '/productDetail/:id?', component: './productDetail/[id$]' },` (可传可不传)
         - `{ path: '/productDetail/:id?', component: './productDetail/index' },`

2. 使用传参

```jsx
export default function Page(props) {
  // 动态路由 props 上有 context（框架进行的传值）
  console.log(props);
  const { id } = props.match.params;
  return (
    <div>
      <h1 className={styles.title}>Page product/index</h1>
      <p>{id}</p>
    </div>
  );
}
```

_ps. 由于 umi3 最新的版本将 routes 单独抽离出来一个文件，_
命令行动态生成 路由 的功能 已经失效
约定式路由也同样不会在配置文件自动生成
故. 上面所做的按约定式的方式生成的页面，可以按普通方式生成，只要 路由配置合法即可。

### 嵌套路由

1. 创建

   - `umi g page ofLayout/_layout`
   - `umi g page ofLayout/index`

2. 路由配置

    ```js
      {
        path: '/ofLayout',
        component: './ofLayout/_layout',
        routes: [
          {
            path: '/ofLayout/index',
            component: './ofLayout/index',
          },
        ],
      },
    ```

3. _layout 文件配置

    ```jsx
      export default function Page(props) {
        return (
          <div>
            <h1 className={styles.title}>Page ofLayout/_layout</h1>
            {props.children}
          </div>
        );
      }
    ```

### 页面跳转

```jsx
import { Button } from 'antd';
import { history, Link } from 'umi';
import React from 'react';

export default function Page() {
  return (
    <div>
      <h1 className={styles.title}>Page about/index</h1>

      {/* 命令式 */}
      <Button
        type="primary"
        onClick={() => {
          history.push('/more');
        }}
      >
        go more
      </Button>

      {/* 声明式 */}
      <Link to="/productDetail">to productDetail</Link>
    </div>
  );
}
```

_ps. 在 umi 3.x 中_，使用 history 取代 router，Link 的导入方式也发生的变化.

## antd 修改默认样式

> 使用 :global() 的方式修改 antd 默认样式，类似在 vue 中使用 /deep/

```less
.moreForm { // className 自定义类名
  display: flex;

  :global(.ant-form-item) { // antd 封装后的类名
    margin-right: 10px;
  }
}
```

_ps. 同样可以在 global.less 中_ 直接使用 antd 封装后的类名 修改默认样式，由于该样式挂载至全局，可能造成局部样式的紊乱，除非全局样式，一般不在 global.less 修改antd 默认样式
