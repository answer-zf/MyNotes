# 常见面试题解析

> markdown 创建目录命令：Create Table of Contents

- [常见面试题解析](#常见面试题解析)
  - [vue](#vue)
    - [v-if / v-for 优先级即性能问题](#v-if--v-for-优先级即性能问题)
      - [案例](#案例)
      - [结论](#结论)

## vue

### v-if / v-for 优先级即性能问题

#### 案例

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

#### 结论
  
> 通过 案例测试，以及查看源码（`node_modules\vue\src\compiler\codegen\index.js` 64行）得出的相应结论

- v-for 优先级高于 v-if,从而影响render函数的生成结果。
  - 在生成结果中，如果 同时出现（同级），循环一定包含所以的条件判断，即每次渲染先执行循环，在执行条件判断。
  - 无论如何循环都无法避免。浪费性能。
  - 为了避免这种情况的发生，需要将条件写到外围，在外层嵌套 template，在这一层进行条件判断，然后再内部进行循环。
  - 如果，不可避免的需要对数组元素进行判断，再循环渲染，此时的解决方案为，使用计算属性，进行过滤，再使用 render 函数渲染。
    - 减少 render 函数的 频繁执行，使其效率变高，即性能优化。
