## NPM

**Node Package Management**

### npm 网站

npmjs.com

### npm 命令行工具

只要安装了node ,就安装了npm

npm也有版本概念

```shell
npm --version     ## 查看版本
```

```shell
npm install --global npm     ## 升级npm
```

### npm 常用命令

-   npm init [--yes]
    -   npm init -y 跳过向导，快速生成
-   npm install
    -   一次性把 dependencies 选项中的依赖项全部安装
    -   npm i
    -   npm install --production
        -   只安装dependencies 依赖项，不安装devDependencies （用于生产环境）
-   npm install 包名
    -   只下载
    -   npm i 包名
-   npm install 包名 --save
    -   下载并保存依赖项（ package.json 文件中的 dependencies 选项）
    -   npm i -S 包名
-   npm install 包名 --save-dev
    -   下载并保存依赖项（ package.json 文件中的 devDependencies 选项 ，对应上文的 --production）
    -   npm i -D 包名
-   npm uninstall 包名
    -   只删除，如果有依赖项会依然保存
    -   npm un 包名
-   npm uninstall --save 包名
    -   删除的同时也会把依赖信息也去除
    -   npm un -S 包名
-   npm help
    -   查看使用帮助
-   npm 命令 --help
    -   查看指定命令的使用帮助
-   npm root -g

    -   查看全局包安装目录

-   清除缓存
    -   `npm cache clean --force`

#### --save 和 --save-dev

通过 `--save` 参数安装的包，是将依赖项保存到 package.json 文件中的 dependencies 选项中。
通过 `--save-dev` 参数安装的包，是将依赖项保存到 package.json 文件中的 devDependencies 选项中。

无论是 `--save` 或者 `--save-dev` 安装的包，通过执行 `npm install` 都会将对应的依赖包安装进来。

但是，在开发阶段会有一些仅仅用来辅助开发的一些第三方包或是工具，然后最终上线运行（到了生产环境），
这些开发依赖项就不再需要了，就可以通过 `npm install --production` 命令仅仅安装 `dependencies` 中的
依赖项。

### npm 自定义脚本命令 的配置

-   在 `package.json` 中配置  scripts - 自定义脚本命令
-   自定义 键 的名字 ，值为对应的命令，使用时 用 `npm run key（所自定义键的名字）`
    -   键为 `start` 时，可省略 run 即：`npm start`
-   每一个自定义脚本命令支持一种规则：
    -   `pre + dev（自定义名称）`
        -   `npm run dev` 的时候先执行 `predev` 再执行 `dev`
    -   `post + dev（自定义名称）`
        -   `npm run dev` 的时候先执行 `dev` 再执行 `postdev`

实例：

```json
// babel 配置后：
// package.json 中配置  scripts - 自定义脚本命令

{
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",

    // 开发环境 实时编译 执行
    "dev": "nodemon main.js",
    // 配置后 运行 `$ npm run dev`  ==  `$ nodemon main.js`

    // 生产环境 转译 后执行 -- 生产环境借助 `@babel/cli` 工具 直接编译成 es5 后运行
    "build": "babel src --out-dir dist",
    "prestart": "npm run build",
    "start": "node dist/app.js"
     // 配置后 运行 `$ npm start`  先执行 build 在 执行 start
  }
}
```

### package.json

-   每个项目的根目录下都要有一个 package.json 文件 （包描述文件）

-   执行`npm install` 包名的时候都加上 --save，用来 保存依赖项信息

-   package.json 可以通过 `npm init`的方式自动初始化出来
    -   `dependencies` 选项，保存第三方包的依赖信息
-   若删除了node_modules 文件夹，且package.json 存在
    -   直接使用 `npm install` 找回
        -   `npm install` 自动把package.json 中的dependencies 中所有的依赖项，都下载回来.

### package.json 和 package-lock.json

npm5 以前是不会有 `package-lock.json` 这个文件的

npm5以后才加入的

当你安装包的时候，npm 都会生成或者更新 `package-lock.json` 这个文件

-   npm5以后的版本安装包，不需要加 `--save` 参数，他会自动保存依赖信息
-   当安装包的时候，会自动创建或者是更新 `package-lock.json` 这个文件
-   `package-lock.json` 会保存 `node_modules` 中所有包的信息（版本、下载地址）
    -   这样的话重新 `npm install` 的时候速度就可以提升
-   从文件看来，有一个 `lock` 称之为 锁
-   这个`lock` 是用来锁定版本的
-   如果项目依赖1.1.1版本
-   你重新install 其实会下载最新版本，而不是1.1.1
-   我们的目的希望可以锁住1.1.1这个版本
-   `package-lock.json`这个文件的另一个作用就是锁定版本号，防止自动升级到最新版本

## NVM

**Node Version Management**

 <https://github.com/coreybutler/nvm-windows>

-   nvm list 查看所有已安装的 node 版本
-   nvm install 版本号 安装指定版本的 node
-   nvm use 版本号 切换到指定版本号
-   nvm proxy 代理地址 配置代理进行下载

## CNPM

npm存储包文件的服务器在国外，有时候会被墙，速度很慢

<https://npm.taobao.org/> 淘宝的开发团队，把npm在国内做了备份

步骤：

1.  安装淘宝的cnpm：

    ```shell
    npm install --global cnpm
    ## --global表示安装到全局，而非当前目录
    ## 这条命令中 --global不能省略
    ## 所有需要用 --global 来安装的包都可以在任意目录执行
    ```

2.  安装时包时将`npm` 替换成 `cnpm`

    ```shell
    # 这里还是走国外的npm服务器，速度比较慢
    npm install jquery
    # 使用 cnpm 通过淘宝的服务器下载
    cnpm install jquery
    ```

3.  `如果不想安装 cnpm 又想使用淘宝的服务器来下载` **--常用**

    ```shell
    npm install jquery --registry=https://registry.npm.taobao.org
    ```

    -   每次手动加参数过于繁琐，可以把这个选项加入配置文件中：

        ```shell
        npm config set registry https://registry.npm.taobao.org

        ## 查看npm配置信息
        npm config list
        ```

    -   只要经过上面命令配置，以后所有的`npm install` 都会默认通过淘宝服务器来下载

## NRM

**Node Registry Manager**

作用：提供了一些最常用的NPM包镜像地址，能够让我们快速的切换安装包时候的服务器地址；
什么是镜像：原来包刚一开始是只存在于国外的NPM服务器，但是由于网络原因，经常访问不到，这时候，我们可以在国内，创建一个和官网完全一样的NPM服务器，只不过，数据都是从人家那里拿过来的，除此之外，使用方式完全一样；

安装：

```shell
$ npm install -g nrm
```

查看当前所有可用的镜像源地址以及当前所使用的镜像源地址；

```shell
$ nrm ls
```

切换不同的镜像源地址；

```shell
$ nrm use '镜像名'
```

> 注意： nrm 只是单纯的提供了几个常用的 下载包的 URL地址，并能够让我们在 这几个 地址之间，很方便的进行切换，但是，我们每次装包的时候，使用的 装包工具，都是  npm

## YARN

Yarn 是一个 Facebook 开源的一个类似于 npm 的一个包管理工具，也就是 npm 能做的，
yarn 也能做。

安装：

```bash
npm install -g yarn
```

使用：

```bash
# npm init
yarn init

# npm install --save 包名
yarn add 包名

# 离线安装
yarn add 包名@版本号 --offline

# npm install
yarn install

# npm uninstall 包名
yarn remove 包名

# npm install -g 包名
yarn global add 包名

# npm uninstall -g 包名
yarn global remove 包名
```
