## vue项目经验

>   由于公司给到 一个做 crm 的 需求，在项目需求编写前做的准备，向后端要了一个功能复杂的 用 node 编写简易接口 做练手的项目，在后期由于公司考虑到成本、团队 等多方因素后终止了 项目

-   项目展示：http://shop.dorc.top
    -   账号：admin
    -   密码：123456

-   项目介绍：购物商城性质的后台管理系统，包括账号管理

    -	服务端已开启 CORS 跨域支持
    -	统一使用 Token 认证
    -	数据返回格式统一使用 JSON
    -	使用 element ui 

-   重要技术要点：

    1.	使用 axios 配置拦截器做预处理登录验证

    2.	由于个人失误，没有上线性能优化。原本所做的技术如下：

        -  	自定义webpack配置，通过 vue.config.js 中 chainWebpack 切换开发和发布模式
        -  	通过 external设置排除项 外部加载CDN
        -  	通过 tap 设置 自定义标识 通过 ..options… 区别开发模式和发部模式
        -  	使用路由懒加载做优化

-   其他vue 项目：
    -   使用 mintUI做过移动端 商城项目
    -   主要技术点 是 使用 vuex 做购物车的后端数据同步
    -   由于 练手没有上线 源码可以另附

## 前端项目：

备注：只是项目站，没有使用vue

### 前端设计均有我一个人完成的项目

#### 公司官网

-	前端设计均有我一个人完成的项目
    
[尚京官网](https://www.shangjinggroup.cn/)

#### 项目站

> 做项目前 仅 提供一套vi，在交付后端前所有工作均由我一个人完成

-   在我手上设计的每个项目站，我自己的设计要求是，依据产品手册展示出的风格，达到每个项目的可辨识

[金汉亭](http://www.jinhanting.com/)

[海鲜时间](http://www.haixianshijian.com/) 

[御厨传奇](http://www.yuchuchuanqi.cn/)

### 只负责前端的项目部分列举

> 由设计交付过来设计稿， 我自己经过添加交互稍作修改 实现前端

-   付上的作品 是已经上线的，由于公司在改主体的审核的过程中 只把部分重要的项目站放在香港服务器做暂时性的线上，后续主体审核通过会全部上线所有的项目站。

[尚品宫](http://www.shangpingong.com/)

[韩鱼客](http://www.hanyuke.cn/)

[辣圈食汇](https://www.laquansc.com/)

[伦度](http://www.lunduhg.com/)

[大米君](http://www.damijun.cn/)

[铁板皇](http://www.tiebanhuang.com/)

[食将](http://www.shijiangbiandang.com/)

[因为鱼](http://www.yinweiyu.cn/)

[乌茶语](http://www.wuchayu.com/)

[靓谷粥铺](http://www.liangguzhoupu.com/)

[海囧](http://www.haijiongmeat.com/)

[美食记](http://www.shiguomeishiji.com/)


## 小彩蛋 python

> 接口已开启，可使用postman 进行测试

-   [接口文档](API)

-   后续自己使用 python - django 写了一个类似的接口

-   此项目在构建阶段没有使用 html 页面，全程 postman 调试
  
-   此项目模拟商业项目，所有上线的核心功能均已实现，但是未作上线的 优化)