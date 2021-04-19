# 基础

## 类似 vue 
1. 小程序不是运行在浏览器中，没有 BOM DOM 对象
2. 小程序的 JS 有额外的成员
   - App 方法：用于定义应用程序实例对象 （类似 vue）
   - Page 方法：用于定义页面对象 （类似 router）
   - getApp 方法: 用于获取全局应用程序对象
   - getCurrentPages 方法：用于获取当前页面的调用栈(数组， 最后一个就是当前页面)
   - wx 对象： 用来提供核心API
3. 小程序支持 commonJS规范
   - 不支持 exports.xxx 导出
   - 只支持 module.exports

## 事件
### 事件绑定
1. bindtap catchtap
   - bindtap 绑定事件，事件冒泡
   - catchtap 绑定事件，阻止事件冒泡

### 事件传参
1. 使用 data- 属性， 通过调用 e.target.dataset / e.currentTarget.dataset(有冒泡的情况下使用) 的方式获取 （类似H5）

## 数据绑定
- 小程序不支持双向数据绑定
> 可以使用 bindinput 监听，this.setData() 方法同步
```js
  data: {
    message: 'initial'
  },
  inputHandle: function(e) {
    this.setData({
      message: e.detail.value
    })
  }
```
```html
  <input value="{{ message }}" bindinput="inputHandle"></input>
  <text>
    {{ message }}
  </text>
```

## API :交互相关

https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.showToast.html

```js
Page({
  btnToDo: function() {
    // 交互操作组件 必须通过调用API方式使用
    wx.showActionSheet({
      // 显示出来的项目列表
      itemList: ['zf', 'zzz', 'fff', 'ccc'],
      // 点击其中任一项的回调
      success: function(res) {
        // res.cancel 指的是是否点击了取消
        if (!res.cancel) {
          // tapIndex指的是点击的下标
          console.log(res.tapIndex)
        }
      }
    })

    wx.showModal({
      title: '提示',
      content: '这是一个模态弹窗',
      success: function(res) {
        if (res.confirm) {
          console.log('用户点击确定')
        }
      }
    })

    wx.showToast({
      title: '成功',
      // 只支持 success 和 loading
      icon: 'loading',
      duration: 2000
    })
  }
})
```

## 滚动条
使用 scroll-view 组件 
- 添加 scroll-y属性 纵向滚动
- 添加 scroll-y属性 横向滚动
- 添加 overflow : hidden ; 样式

## 页面跳转
### navigator
#### 页面传值
- 使用路由问号形式传参
- 获取参数：
  - 使用生命周期函数 onload 中的 第一个参数获取
    ```js
    Page({
      onLoad: function(option) {
        console.log(option)
      }
    })
    ```
#### 添加 redirect 属性 不产生访问历史记录

> cursor pointer 的方式是一个我们发现的小技巧，可以让任何元素点击时高亮

> 使用 navigator 跳转，默认不能跳转到主界面，即：tabbar 配置的页面，强制跳转：使用 open-type 属性，值为 switchTab

### API 跳转
使用事件函数调用 API跳转
```js
Page({
    tapHandle: function () {
        // 当我们点击按钮 系统会自动执行这里的代码
        wx.navigateTo({
          url: '../demo2/demo2?id=123'
        })

        // 相当于加上redirect的 navigator
        // wx.redirectTo({
        //   url: '../demo2/demo2'
        // })
        
        // 默认返回到上一页
        // wx.navigateBack()
        // 指定delta 就是返回到指定页面
        // delta 过大（超出历史记录）默认返回最开始的页面
        // wx.navigateBack({
        //   delta: 100
        // })
        // }
})
```
## API 设置 navigationbar
> https://developers.weixin.qq.com/miniprogram/dev/api/ui/navigation-bar/wx.showNavigationBarLoading.html

## 发请求
1. 调用 wx.request() API
   - ```js
      wx.request({
        url: 'https://******',
        success: res => {
          this.setData({
            ***: res.data
          })
        }
      })
      // ------------ 进阶 promise 封装
      module.exports = url => {
        return new Promise((resolve, reject) => {
          wx.request({
            url: `https://******${url}`,
            success: resolve,
            fail: reject
          })
        })
      }
      // 调用：
      const *** = require('---')
      onLoad: function(options) {
        ***('url').then(res => {
          this.setData({
            data: res.data
          })
        })
      }
     ```
2. 请求地址必须在管理后台添加白名单，域名必须采用 HTTPS
   - 配置 管理员设置
     - https://mp.weixin.qq.com/wxamp/devprofile/get_profile?token=162944412&lang=zh_CN