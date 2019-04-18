# git基操

## 仓库初始化

初始化代码仓库
`git init`

## 代码提交

选择提交文件
`git add <filename>`

编写提交信息
`git commit -m "commit message"`

推送到远程
`git push`

## 操作分支

创建分支
`git branch <branchname>`

显示分支列表
`git branch`

切换分支
`git checkout <branch>`

合并分支
`git merge <branchname>`

删除分支
`git branch -d <branchname>`

## 操作远程

复制现有的远程数据库
`git clone <url>`

显示远程仓库列表
```
git remote
// 添加-v选项就可以显示远程数据库的详细情况
````

添加远程仓库
`git remote add <name> <url>`

同步远程仓库最新历史记录
`git fetch`

同步远程仓库最新修改到本地
`git pull`

推送更新到远程仓库
`git push`


## 其他操作

显示提交记录
`git log`

显示修改文件清单
`git status`

查看提交的详细记录
`git show <commit>`

## git设定

设定用户名/电子邮件地址

`git config --global user.name <username>`

`git config --global user.email <mailaddress>`
