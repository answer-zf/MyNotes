# VersionControl

## 什么是版本控制？

版本管理就是管理更新的历史记录，
它给我们提供了一些在软件开发过程中必不可少的功能，例如：

- 记录一款软件添加或更改源代码的过程
- 回滚到特定阶段，恢复误删除的文件
- 合并多人协作的文件等
- 多人协同，文件传输

## 版本控制分类

- 集中式
  - SVN
- 分布式
  - Git

# SVN

SVN 全称 Apache Subversion，是一个开放源代码的集中式版本管理系统。
在 2000 年由 CollabNet 开发，现已发展成为 Apache 软件基金会的一个开源项目。

## SVN 交互协作流程

![svn交互流程](http://localhost:9710/blog/Config_Document/svn.png)

## 集中式

早期的版本管理就是以 `Apache Subversion` 为代表的集中式版本管理，
集中式版本管理将所有的数据集中存放在服务器中，这是有便于统一管理的优点。
但是一旦开发者所处的环境不能连接服务器，就无法获取最新源代码，开发也就无法进行。
服务器宕机时也是同样的道理，而且万一服务器故障导致数据丢失，
恐怕开发者就再也见不到最新的源代码了。

简而言之：

- 中央服务器好比是一个图书馆
- 你要改一本书，必须先从图书馆借出来（checkout）
- 然后回到家自己改，改完了，再放到图书馆（commit）

## 一些术语

- 源代码库（repository）：源代码统一存放的地方
- 检出（checkout）：当你手上没有源代码的时候，就需要从 responsive checkout 一份
- 提交（commit）：当你已经修改了代码，就需要 commit 到 repository
- 更新（update）：当你已经 checkout 了一份源代码，Update 一下就可以和 repository 上的源代码同步，你手上的代码就会有最新的变更

## 环境安装

### 使用 VisualSVN 搭建 SVN 服务器：

SVN 服务器：运行 Subversion 服务的计算机。

为了方便，我们这里使用比较流行的图形化工具 `VisualSVN`
来搭建我们的 SVN 服务。

安装：

​ [https://www.visualsvn.com](https://www.visualsvn.com/) = > VisualSVN Server = > download

配置：

- 记住设置的端口号

- 安装完毕之后，基本使用流程如下：
  - 创建用户 `users`
  - 创建版本仓库 `Repositories`
  - 设定用户权限

### 使用 TortoiseSVN 作为 SVN 客户端

SVN 客户端：用户通过 SVN 客户端同 SVN 服务器交互。

这里我们使用最流行的 `TortoiseSVN`

安装：

​ https://tortoisesvn.net/index.zh.html = > download

TortoiseSVN 客户端基本操作流程：

- 检出项目：`checkout`
  - 在没有源代码的前提下，需要通过 tortoise-svn 客户端下载
- 提交修改：`commit`
  - 帮你记录当前开发的软件的状态
- 更新文件或目录：`update`（更新）
  - 别的开发人员在已有源代码的前提下可以通过 update 更新服务器上最新的版本
- 查看版本日志：`log`（日志）

## 版本管理使用建议

- 不要频繁的提交版本
  - 一般有比较成熟的功能模块的时候，再去提交
  - 修复了功能性 bug 的时候再去提交
  - 提交的代码最好无 bug
- 每次 commit 之前都要 update
  - 因为你在编辑这个文件的时候，可能比人已经编辑并提交了某个版本
  - 所以先 update，目的是为了检查一下服务器上有没有最新版，如果有，直接更新
    - 更新的过程中如果遇到冲突，不要慌，去手动解决
- 每次 commit 的时候都务必要写提交日志
  - 这个提交日志就好比你保存副本的时候加的一个标记
  - 目的是为了日后做版本的回退查找以及查看记录更新状态

## 使用总结

- 版本控制管理系统
- 源代码仓库 repository
- 检出代码 checkout
- 更新最新源代码 update
- 提交修改 commit
