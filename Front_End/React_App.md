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

```js
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
