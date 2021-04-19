## Typora

```shell
## 图像设置
./media/${filename}. assets
```

主题文件：

```css
/* 添加文件 base.user.css */
code {
  padding: 2px 4px;
  border: 1px solid #999;
  background: transparent;
  color: #dd0055;
}
```

## Cmder

### 将 λ 修改为 \$

```lua

-- cmder\vendor\clink.lua文件中第51行中{lamb}修改为$

-- 修改前
local cmder_prompt = "\x1b[1;32;40m{cwd} {git}{hg}{svn} \n\x1b[1;39;40m{lamb} \x1b[0m"

-- 修改后
local cmder_prompt = "\x1b[1;32;40m{cwd} {git}{hg}{svn} \n\x1b[1;39;40m$ \x1b[0m"

```

### 右键快捷：

1. 设置环境变量：`C:\Application\cmder` 添加到 `Path` 中

2. 管理员身份运行

```shell
$ where Cmder.exe

$ Cmder.exe /REGISTER ALL
```

## MD

### shortcut

| Command | Key Binding      |
| :------ | :--------------- |
| 链接    | Ctrl + V         |
| 预览    | Ctrl + Shift + V |
