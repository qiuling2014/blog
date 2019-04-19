# git基操

git基本操作。

## 仓库初始化

初始化代码仓库
`git init`

## 代码提交

查看更新的文件
`git status`

选择提交文件
`git add <filename>`

把暂存区的指定 file 放到工作区中
`git reset HEAD <filename>`

编写提交信息
`git commit -m "commit message"`

推送到远程
`git push`

放弃工作区的修改

```
放弃工作区某一文件的修改：
git checkout <file-name>

放弃工作区所有修改：
git checkout .
```

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
添加-v选项就可以显示远程数据库的详细情况
````

添加远程仓库
`git remote add origin <remote-url>`

修改远程仓库的url
`git remote set-url origin <URL>`

同步远程仓库最新历史记录
`git fetch`

同步远程仓库最新修改到本地
`git pull`

推送更新到远程仓库
`git push`

## 贮藏，存储当前的修改，但不用提交commit

展示所有 stashes

`git stash list`

回到某个 stash 的状态

`git stash apply <stash@{n}>`

回到最后一个 stash 的状态，并删除这个 stash

`git stash pop`

删除所有的 stash

`git stash clear`

从 stash 中拿出某个文件的修改

`git checkout <stash@{n}> -- <file-path>`


## 其他操作

显示提交记录
`git log`

显示修改文件清单
`git status`

查看提交的详细记录
`git show <commit>`

把 A 分支的某一个 commit，放到 B 分支上

`git checkout <branch-name> && git cherry-pick <commit-id>`

给git命令起别名
```
git config --global alias.<handle> <command>

比如：git status 改成 git st，这样可以简化命令

git config --global alias.st status
```

## git设定

设定用户名/电子邮件地址

`git config --global user.name <username>`

`git config --global user.email <mailaddress>`
