## Browser

### Firefox

| 作用          | 快捷键                                      |
| :------------ | :------------------------------------------ |
| 前进/退后     | Atl + ←/→ (Backspace/ Shift Backspace)      |
| 刷新/强制刷新 | Ctrl + R / Ctrl + Shift + R (F5/ Ctrl + F5) |
| 新标签页      | Ctrl + T                                    |
| 书签          | Ctrl + B                                    |
| 历史          | Ctrl + H                                    |
| 下载          | Ctrl + J                                    |
| 清除数据      | Ctrl + Shift + Del                          |
| 选中地址      | Alt + D / F6 / Ctrl + L                     |
| setting       | 地址栏输入：about:preferences               |


## CMD 常用命令

### 操作类

```shell
# 切盘
$ D:

# 删除文件
$ del

# 退出dos
$ exit

# 清空命令行
$ cls

# &&   同时执行两个操作 用 && 衔接
```

### 目录操作类

```shell
#显示目录
$ dir   ## 详细

#创建目录
$ md 目录名

#删除目录
$ rd 目录名
$ rd /s/q 目录名   ## 强制删除文件文件夹和文件夹内所有文件

#进入指定目录
$ cd 目录名

#树形图显示目录结构
$ tree
```

### 文件操作类

```shell
# 重名名文件
$ ren

######	进阶操作  ==>  批量重命名(指定字符串替换)
$ ren *.html *.php

# 替换文件
$ replace

# 删除文件
$ replace
$ del 文件名

# 创建文件
$ echo on > a.txt

# 向文件添加内容 （覆盖）
$ echo 内容 > a.txt

# 向文件添加内容 （向后追加）
$ echo 追加 >> a.txt
```

### 服务操作类

```shell
# 查询服务
$ sc query MySQL

# 启动服务
$ net start MySQL

# 停止服务
$ net stop MySQL

# 删除服务
$ sc delete MySQL

# 查询端口情况
$ netstat -ano
```

## Window

```shell
sysdm.cpl  cmd 开启服务

# 强制删除  =>  SHFIT + delete

# 显示桌面			==>   win + D

# 最小化所有窗口	  ==>   win + M

# 最小化除当前窗口以外所有窗口	==>   win + HOME

# 打开资源管理器	  ==>   win + E

# 打开设置	        ==>   win + I

# 截图			 ==>   win + PrintScreen

# 放大缩小视口	  ==>   WIN + +/-

# 切换输入法    	   ==>   WIN + space

# 自定义截图		   ==>   WIN + SHFIT +S
```

    win + Ctrl + D       新建桌面
    win + Ctrl + 方向键   切换桌面
    win + tab						 列表

## Sublime

```shell
# 命令行 用 sublime 打开当前目录 .表示当前目录
subl .

# 命令行 用 vscode 打开当前目录
code .

# 删除整行  		==>    CTRL + SHIFT + K

# 括号内部选中  	   ==>    CTRL + SHIFT + M

# 直接跳下一行  	   ==>    CTRL + ENTER

# 直接跳上一行  	   ==>    CTRL + SHIFT + ENTER

# 查找选择器		    ==>	   CTRL + R

# 替换同一个目录下所有文件公共部分
# ==>侧边栏目录下子文件右键选项   Find Advanced  >  In Parent Folder

# 打开所在目录		==>    右击文件选  Reveal
```

### 搜索栏技巧

```shell
# 搜索关键词如有空格，可用  ""  包含，视为一个词

# 搜索限定

$ 关键词 + site:cnblogs.com

# 限定在 cnblogs.com 搜索
```

## 右键菜单分栏设置地址

> HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Explorer\\CommandStore\\shell\\
