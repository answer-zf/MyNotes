# git

## git gists

- VS Code 远程同步 token：`42cafa1971c8eabf28258d9dd154f762`
- Atom 远程同步 token：`6abd13e01b3fa94069f5122f59a32cec7315dc57`

## 常用git指令

1. 配置

   1. 配置 username：`git config --global user.name "user_name"`
   2. 配置 email：`git config --global user.email "email_url"`
   3. 查看记录：`git log --oneline`
   4. 查看配置：`git config --list`

2. origin 相关

   1. 添加 `git remote add origin git_url`
   2. 删除 `git remote rm origin`

3. ssh: `ssh-keygen -t rsa -C "email_url"`

4. 分支：

   1. 创建并切换至该分支：`git checkout -b branch_name`
   2. 切换分支：`git checkout branch_name`
   3. 将目标分支合并：`git merge branch_name`

5. tag

   1. 查看 tag：`git tag`
   2. 创建 tag：`git tag tag_name`
   3. 删除 tag：`git tag -d tag_name`
   4. 创建 tag，并添加注释：`git tag -a tag_name -m "tag_info"`
   5. 提交至远端：`git push origin --tags`

6. 报错解决

   1. LF 的报错：`git config --global core.autocrlf false`
