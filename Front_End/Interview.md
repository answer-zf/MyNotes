# 常见面试题解析

> markdown 创建目录命令：Create Table of Contents

- [常见面试题解析](#常见面试题解析)
  - [vue](#vue)
    - [v-if / v-for 优先级即性能问题](#v-if--v-for-优先级即性能问题)
    - [在 vue 组件中的data为什么是函数，而根实例没有限制](#在-vue-组件中的data为什么是函数而根实例没有限制)
    - [vue 中的key的作用和工作原理](#vue-中的key的作用和工作原理)
    - [diff 算法](#diff-算法)
      - [必要性](#必要性)
      - [执行方式](#执行方式)
      - [高效性](#高效性)
      - [diff 结论](#diff-结论)
    - [组件化的理解](#组件化的理解)
    - [vue 设计原则](#vue-设计原则)
    - [MVC MVP MVVM](#mvc-mvp-mvvm)

## vue

### v-if / v-for 优先级即性能问题

- 案例

```vue
<template>
  <div>
    <h3>userList - if / for</h3>
    <!-- if / for 同级 -->
    <!-- eslint-disable-next-line -->
    <ul v-for="item in userList" :key="item.id" v-if="userList.length">
      <li>{{ item.name }}</li>
    </ul>

    <!-- if / for 不同级(常用的优化手段) -->
    <!-- <template v-if="userList.length">
      <ul v-for="item in userList" :key="item.id">
        <li>{{ item.name }}</li>
      </ul>
    </template> -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      userList: [
        { id: 1, name: 'zf' },
        { id: 2, name: 'cf' },
        { id: 3, name: 'df' }
      ]
    }
  },
  mounted() {
    console.log(this.$options.render)
    // 同级时 返回 核心代码（生成的渲染函数）
    // _vm._l(_vm.userList, function(item) { // _l 列表渲染的函数，最终会循环输出所有的children
    //   return item.id > 1 // 可知 if / for 平级使用，if的条件在循环体里
    //     ? _c("ul", { key: item.id }, [_c("li", [_vm._v(_vm._s(item.name))])])
    //     : _vm._e()
    // })

    // 不同级时 返回 核心代码（生成的渲染函数）
    // _vm.userList.length // 将 if 放置外层，在渲染函数中 先条件判断在 进行 循环体操作
    //   ? _vm._l(_vm.userList, function(item) {
    //     return _c("ul", { key: item.id }, [
    //       _c("li", [_vm._v(_vm._s(item.name))])
    //     ])
    //   })
    // : _vm._e()
  }
}
</script>
```

***

- 结论
  
> 通过 案例测试，以及查看源码（`node_modules\vue\src\compiler\codegen\index.js` 64行）得出的相应结论

- v-for 优先级高于 v-if,从而影响render函数的生成结果。
  - 在生成结果中，如果 同时出现（同级），循环一定包含所以的条件判断，即每次渲染先执行循环，在执行条件判断。
  - 无论如何循环都无法避免。浪费性能。
  - 为了避免这种情况的发生，需要将条件写到外围，在外层嵌套 template，在这一层进行条件判断，然后再内部进行循环。
  - 如果，不可避免的需要对数组元素进行判断，再循环渲染，此时的解决方案为，使用计算属性，进行过滤，再使用 render 函数渲染。
    - 减少 render 函数的 频繁执行，使其效率变高，即性能优化。

### 在 vue 组件中的data为什么是函数，而根实例没有限制

> 组件： 源码位置 node_modules\vue\src\core\instance\state.js 112行 中的 initData()
> 根：源码位置 node_modules\vue\src\core\instance\init.js 31行 合并选项。组件的操作和根的操作 不尽相同。
> node_modules\vue\src\core\util\options.js 121 行 取消 了根实例的检测限制。

- 源码的解读：如果 data 是一个函数就是执行，并将其结果作为data选项的值，否则使用使用设置的data.
- 如果每个组件，都是用的是对象，用户所设置的data，就会直接作为组件实例中的 data选项。
- 由于组件的 component 方法在声明组件的时候，只执行一次，选项处理的时候只会走一次
- 固组件拿进来的data选项都指向了同一个地方。即一个组件的不同的实例之间 使用的 数据共享，产生了数据污染。

- 组件中data为函数的原因即，在多实例的情况下，保证 组件状态（数据）的 相互不干扰，不污染。
- 在根实例 可以为对象的原因，每一次创建根实例的时候，通过 new 的方式只能创建一个，即全局范围内，是单例的不可能出现多个实例。

***

- 结论

  - vue 的组件可能存在多个实例，使用 对象定义data，会导致他们公用一个data对象， 将来的状态之间产生污染，即状态变更影响所有的组件实例
  - 使用 函数形式定义data,在initData时会将其作为工厂函数，返回全新的data对象。
  - 即当实例创建的时候，每一次都会执行这个函数，每一次都会创建新的data对象，data对象相互独立，即使对象的数据是一样的，但指向不同的引用。
  - 有效规避多个实例状态污染。
  - 根实例在创建过程中，不存在该限制，因为根实例在整个应用程序范围内只有一个，不需要担心这个问题。即使创建多个跟这个也没有关系。

  - （组件中）数据初始化 部分代码，会检测 data的类型，从而执行具体的执行方式，
  - 根实例在创建时候，在合并选项的时候，只有根实例会拿到实例，可以躲过data选项的校验。
  - 普通组件不存在实例，没办法躲过校验的检测

### vue 中的key的作用和工作原理

> 源码 node_modules\vue\src\core\vdom\patch.js 424

- 主要的作用：高效更新虚拟 dom
- 原理：在 patch 的过程中会执行 patch vnode，在patch vnode 过程中会执行 updateChildren 方法
  - 会更新所有的两个新旧子元素，通过key,可以精准判断目前循环的元素的节点是不是一个节点
  - 如果没有加 key 永远都认为是两个相同的节点，只能强制更新，没法避免频繁更新不同的元素，会额外做很多没必要的dom操作
  - 不加key的性能会很差
  - 加 key ，利用内部优化的算法，猜测首尾结构的相似性，由于大部分情况下元素不会出现大量的位置变化
  - 可以很高效的结束循环，减少大量的更新过程， 减少不必要的dom操作
- 不设置key，在列表更新会有隐蔽的bug
- 在vue中使用相同的标签做过渡效果，由于没有使用 key 做唯一性的判断，很有可能导致动画效果不会触发（元素做了复用）

```vue
<template>
  <div>
    <div v-for="item in list" :key="item">{{ item }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: ['a', 'b', 'c', 'd', 'e']
    }
  },
  mounted() {
    setTimeout(() => {
      this.list.splice(2, 0, 'f')
    }, 200)
  }

  // 原数组：a,b,c,d,e
  // 新数组：a,b,f,c,d,e

  // 不使用 key 时，更新方式
  // a,b 不变，没有key,不知道节点是不是自己的，只能认为是自己的，将 f 的内容 覆盖更新到 c 上，
  // 同理 c => d, d => e,前面更新完毕后，创建新的元素追加在最后。

  // 使用 key
  // 首次 循环 patch a
  // a,b,c,d,e
  // a,b,f,c,d,e
  // 第二次 循环 patch b
  // b,c,d,e
  // b,f,c,d,e
  // 第三次 循环 patch e diff 算法中 updateChildren 的 首尾判断策略 新旧首首 新旧尾尾 新首旧尾 新尾旧首 便于复用。
  // c,d,e
  // f,c,d,e
  // 第四次 循环 patch d
  // c,d
  // f,c,d
  // 第五次 循环 patch c
  // c
  // f,c
  // oldCh 全部处理结束，newCh 只剩下 f ，创建 f 并插入到 c 前面

  // -- 使用key --
  //    更新了五次，但没有执行任何操作，因为更新的是完全相同的五个节点，
  //    数据没发生任何变化，固这五次操作什么事情都没发生，
  //    执行 patch 的过程中不会做任何事情，也不会执行dom操作。
  //    结果，只做了一次创建插入操作。

  // -- 不使用 key --
  //    同样更新了五次，但实际 更新了三次 dom，(相同节点不更新)，同时做了一次创建插入操作。
}
</script>
```

### diff 算法

#### 必要性

> 源码 node_modules\vue\src\core\instance\lifecycle.js 141 mountComponent()
> 源码 node_modules\vue\src\core\vdom\patch.js 548 patchVnode

- mountComponent 使用 $mount 调用，即任何一个vue组件的实例在创建完成之后去挂载的时候，去调用的。
  - 一个组件执行一次 $mount ，便需要创建一个 Watcher。（在vue中 Watcher 实例和组件实例一一对应）
  - 组件中 可能存在很多 data 中的 key 的使用，只有一个Watcher 要做的精准定位便需要diff算法
    - 当界面中多个key发生变化，执行一次diff算法，进行新旧两个虚拟dom的比较，可以精确比较出变化的地方。

- 在vue中 Watcher 粒度的降低，一个组件一个watcher，为了精确知道 更新的过程中，谁发生变化，必须使用diff

#### 执行方式

- patchVnode 是 diff 发生的地方
- 整体策略：深度优先，同层比较
  - 比较根节点，判断是否有子节点
  - 有子节点，则更新 子节点 updateChildren（以此递归）
  - 在子节点中，还有子节点，再次深入到第三层级比较

```bash
      A
  B       E
C   D   F   G

# 比较顺序：A -> B -> C -> B -> D -> B -> E -> F -> E -> G -> E -> A
```

#### 高效性

- updateChildren()

- 做首尾猜测判断，四种情况的比较。
- 四种情况判断完后，在首尾都没有找到相同节点，则做遍历查询
- 拿出 新数组中的首个，在老数组尝试查找
- 如果找到，做相对性的操作（更新）
- 找不到，则创建新增节点

- 当 把子节点数组处理完成后，一定会剩下一些节点（做整理工作）
- 做批量的新增和删除

#### diff 结论

1. diff算法 是 虚拟dom 技术的重点，即 对比新旧两个虚拟dom的算法。

   - 当数据发生变化，虚拟dom 必然发生变化，最后将变化的地方更新在真实dom上
   - 核心点就是 通过 新旧虚拟dom的对比（即：diff），找到虚拟 dom 的变化
     - [diff 详解](https://blog.csdn.net/weixin_42707287/article/details/113994483)
   - 通过 diff 的高效执行 对比过程，从而降低了时间复杂度

2. 在vue中Watcher 粒度的降低，每个组件只有一个 watcher 与之对应，为了精准找到变化发生的地方，需要diff

3. 当修改数据的时候，由于数据响应式，触发了setter，setter会触发通知，通知的方式尝试把wather添加到异步更新队列，

   - 在每次事件循环结束后，会清空队列，在清空队列的过程中 watcher 尝试执行 更新函数，更新函数在执行的时候
   - 会调用组件的渲染函数，和更新函数，这时候 会重新渲染最新的虚拟dom，执行更新函数，比较新旧的虚拟dom，这个时刻即diff真正的执行时刻（触发时刻）
   - 而 新旧dom对比的过程 称之为 打补丁 即 patch

4. diff 过程整体遵循 深度优先，同层比较 策略

   - 两个节点之间比较，先会判断它们是否拥有子节点和文本节点，做对应的操作
   - 核心点就是 两组子节点的比较
   - 使用 头尾猜测策略，做四种可能性的对比尝试，过滤出相同点，减少循环次数，避免大量的循环查找
   - 如果没有找到相同节点才按照通用方式遍历查找，查找结束再按情况处理剩下节点
   - 借助key 同层可以非常精确找到相同节点，使整个patch过程非常高效。

### 组件化的理解

- 提升开发效率、测试性和复用性
- 页面组件、业务组件、通用组件
- vue 中的组件是基于配置，使用 .vue 写的组件是配置，而非组件，
- 框架会后续生成其构造函数（基于 VueComponent,扩展自vue，在扩展的过程会继承vue中已经有的选项）
- 组件技术：prop、自定义事件、插槽
- 组件切割粒度
  - 将经常发生数据变化的部分，切割为组件， 将来频繁执行 的 渲染函数、更新函数 以及 patch 的范围较小
  - 有利于将来在执行渲染的时候中，只做 他自己 数据的重新生成和比对，性能必然提高
- 组件思想：高内聚，低耦合。分治思想的落地。

### vue 设计原则

- 渐进式js框架

  1. 使用纯vue开发
  2. 引入路由、vuex .. 开发
  3. 引入 vue-cli .. 模块开发

### MVC MVP MVVM

- MVC

  - Model：负责保存数据，与后台数据同步
  - Controller：负责业务逻辑，根据用户的行为对model数据进行修改
  - View：负责视图展示，将model的数据可视化出来。

- MVP

  - Presenter：集中的控制中心，连接 view 和 Model 层

- MVVM

  - ViewModel:通过实现一套数据响应式机制，虚拟DOM，自动响应model层变化（数据驱动）

- 总结

  - 三个框架主要解决 model 和 view 间的耦合问题、代码分层、维护问题。
  - MVC 专注于 服务端，ajax 后前端使用 mvc 的模式开发
    - 优点 分层清晰，缺点 数据流混乱，维护性差
  - MVP：MVC的进化，presenter 作为中间层，负责 M层 和 V层 的通信
    - 解决了耦合问题，但是 P层 过于臃肿，维护性差
  - MVVM
    - 解决了耦合性的同时，解决了维护两者间映射关系的大量繁杂代码和dom操作代码（数据响应式机制，虚拟DOM）
    - 提升开发效率，代码可读性，以及性能优化
