# git骚操

git骚气操作。

## 场景一

> 小Q同学平时用惯了sourcetree，对git命令行操作不熟。在某个简单功能的开发中，小Q同学本地commit了多次，通过`git log`可以看到他的提交记录，分别是：commit-1、commit-2、commit-3，它们都隶属于同一个功能，经过大神同事的教育，应该把合并成同一条commit记录。通过google了解到，可以通过`git reset`来回滚，小Q同学二话不说，一顿操作，拿到需要回滚提交commit的hash码，`git reset --hard xxxxxxx`，一条命令下去，小Q发现不对劲，自己敲了两天的码不见，`git log`也看不到提交记录了，急哭😭。

我们知道`git reset --hard`会丢弃所有工作副本改动，对应到sourcetree，就是重置分支提交中选择`强行合并`模式的操作。下面让我们来拯救小Q同学。

我们只需用到两条命令：

`git reflog`：可以查看所有分支的所有操作记录（包括已经被删除的commit记录和reset的操作，`git log`是看不到的）

`git cherry-pick <commit id>`：选择某一个分支中的一个或几个commit(s)来进行操作，以理解merge的个性定制版本。

操作步骤：

1. 首先通过git reflog找到commit-3的commit id；
2. 然后运行`git cherry-pick <commit id>`命令。

此时`git log`，被删除的提交又回来了。

而对于小Q同学的需求，只需跑一下`git reset --soft <commit id>`，再重新提交一次即可。

## 场景二

> 马虎的小Q同学在做commit的操作时经常遗漏一些修改的文件没有add，厌倦了反反复复的`git reset`，你想知道有没有更加便捷的方式。

我们只需用重新再做一次commit操作。

步骤：

1. `git add <filename>`添加遗漏的文件；
2. `git commit --amend`;
3. vim重新编辑提交信息并进行保存。

此时`git log`，可以看到遗漏的文件已经加上。


