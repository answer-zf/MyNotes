# 前端包管理工具

## NVM

> Node Version Management node 版本控制工具

 <https://github.com/coreybutler/nvm-windows>

- `nvm list`：查看所有已安装的 node 版本
- `nvm install 版本号`：安装指定版本的 node
- `nvm use 版本号`：切换到指定版本号

### nvm 镜像使用

1. 找到目录下的：setting.txt
2. 在最后添加镜像地址

    ```txt
    node_mirror: https://npm.taobao.org/mirrors/node/
    npm_mirror: https://npm.taobao.org/mirrors/npm/
    ```

## NRM

> Node Registry Manager npm 镜像地址管理

- `npm i -g nrm` 安装
- `nrm ls` 地址列表
- `nrm use 镜像名` 使用镜像

## NPM

> Node Package Management

### npm 命令行工具

```shell
npm --version     ## 查看版本
```

```shell
npm install --global npm     ## 升级npm
```

### npm 常用命令

- 初始化：npm init -y

- 全局安装：
  - npm i 包名 -g

- 安装依赖：
  - npm i
  - npm install --production
    - 只安装 dependencies 依赖项，不安装devDependencies （用于生产环境）

- 安装运行依赖（主要库，vue、axios、vuex..）：
  - 安装至：`dependencies`
  - npm i 包名 --save
  - npm i 包名 -S

- 安装开发依赖（工具库：babel-plugin-*、*-loader、webpack）：
  - 安装至：`devDependencies`
  - npm i 包名 --save-dev
  - npm i 包名 -D

- 清除缓存
  - `npm cache clean --force`

## YARN

> Facebook 做的npm，高效快速，尤其在 react 项目中建议使用

### 安装

- npm install -g yarn

### 使用

- `yarn init` == `npm init`

- `yarn add 包名` == `npm install --save 包名`

- `yarn global add 包名` == `npm install -g 包名`

- `yarn install` == `npm install`

- `yarn remove 包名` == `npm uninstall 包名`

- `yarn add 包名@版本号 --offline` : 离线安装
