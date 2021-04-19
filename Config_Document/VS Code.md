# VS Code

## Setting Sync：远程配置同步插件的 token：

**sync.gist.id：42cafa1971c8eabf28258d9dd154f762**

## 字体：[FiraCode](https://github.com/tonsky/FiraCode)

## vue 代码时使用 eslint

```shell
# 安装
cnpm i eslint
cnpm i eslint-plugin-vue
cnpm i -g eslint
cnpm i -g eslint-plugin-vue
npm init -y

# 配置
eslint --init
```

[选项配置信息](https://blog.csdn.net/Gabriel_wei/article/details/90269165)

## shortcut

### 代码操作

#### 光标

| 快捷键               | 作用                                    |
| :------------------- | :-------------------------------------- |
| Home / End           | 将光标定位到当前行的最 左 / 右 侧       |
| Shift + Alt + ↑ / ↓  | 向 上 / 下 添加光标                     |
| Shift + Alt + i      | （所选区域）每一行的末尾都创建一个光标  |
| Esc                  | 取消多选光标                            |
| Ctrl + Shift + space | 函数内显示参数信息                      |
| Alt + F              | 选中括号，引号...范围内部代码（自定义） |

#### 删除

| 快捷键                    | 作用                          |
| :------------------------ | :---------------------------- |
| Ctrl + Backspace / Delete | 删除光标之 前 / 后 的一个单词 |
| Ctrl + Shift + K          | 删除整行                      |

#### 编辑

| 快捷键              | 作用                   |
| :------------------ | :--------------------- |
| Shift + Enter       | 在当前行下插入新的一行 |
| Ctrl + Enter        | 在当前行上插入新的一行 |
| Alt + ↑ / ↓         | 将代码向上 / 下移动    |
| Shift + Alt + ↑ / ↓ | 将代码向上 / 下复制    |
| Ctrl + J            | 合并行                 |

#### 折叠代码

| 快捷键               | 作用                       |
| :------------------- | :------------------------- |
| Ctrl + Shift + [ / ] | 折叠 / 展开 所有子区域代码 |
| Ctrl + K Ctrl + 3    | 折叠所有 （html）          |
| Ctrl + K Ctrl + J    | 展开 所有                  |

#### F 键

| 快捷键 | 作用          |
| ------ | ------------- |
| F4     | 匹配下一个    |
| F12    | html 跳转 css |

#### 终端

| 快捷键                | 作用 |
| --------------------- | ---- |
| Ctrl + \` \| 打开终端 |      |

### 编辑器

#### 基本操作

| 快捷键                           | 作用                             |
| -------------------------------- | -------------------------------- |
| Ctrl + Pagedown/Pageup           | 在已经打开的文件之间进行切换     |
| Ctrl + + Shift + Pagedown/Pageup | 切换标签页的位置                 |
| Ctrl + P                         | 在当前的项目工程里，全局搜索文件 |
| Ctrl + G                         | 跳转到指定行                     |
| Ctrl + shift + O                 | 在当前文件的各种方法之间进行跳转 |

#### 分屏

| 快捷键                      | 作用       |
| --------------------------- | ---------- |
| Shift + Alt + 2 / Ctrl + \| | 分屏       |
| Shift + Alt + 1             | 合并       |
| Ctrl + 1 / 2                | 切换分屏   |
| Ctrl + W                    | 关闭编辑器 |

### 插件

#### quokka （ JS TS 辅助 ）

| 快捷键     | 作用                               |
| ---------- | ---------------------------------- |
| Ctrl + K Q | 打开新的 quokka 文件 => JavaScript |
| Ctrl + K J | 打开新的 quokka 文件 => TypeScript |
| Ctrl + K T | 在现有文件重新启动                 |

#### Settings Sync（ 配置同步 ）

| 快捷键          | 作用     |
| --------------- | -------- |
| Shift + Alt + U | 上传配置 |
| Shift + Alt + D | 下载配置 |

## 插件

### Debugger For Chrome

-   快捷键 F5 调试

-   HTML 调试：

    1.  安装 http-server：`npm install -g http-server`
    2.  F5 启动、launch.json 配置

    ```json
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome against localhost",
            "url": "http://localhost:8080",
            "webRoot": "${workspaceFolder}/wwwroot"
        }
    ] 
    ```
    3.  启动 http-server，命令行 使用 `http-server`
    4.  再使用 F5 即可
    