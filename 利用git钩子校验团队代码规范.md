# 利用git钩子校验团队代码规范

统一的代码规范能保持大伙的代码风格统一，我们常常使用eslint来辅助我们，使用统一的风格。而然在实际开发中还是会有一些不符合规范的代码被提交、新人加入公司得普及适应代码规范等烦恼，小的开发团队，可能问题不会很突显，编辑好代码规范文档，下发到团队手上。如果开发团队人数较多，这方面的工作将会反复且耗费精力，只能从工具的层面来限制了。

git钩子（hooks）在特定的事件触发后调用，让你可以定制git操作后的行为。git hooks配置ESlint，如果eslint校验不通过，用户将不能成功提交代码，真正强限制代码风格统一。
远程git仓库钩子定义在hooks目录内，本地仓库在`<GIT_DIR>/.git/hooks`目录。

## HOOKS
* pre-push
* commit-msg
* pre-rebase
* post-update
* prepare-commit-msg
* pre-applypatch
* pre-commit
* pre-receive
* udpate
* post-receive

各个脚本执行的阶段如下：
<img src="https://s2.ax1x.com/2019/01/18/k9Q8IO.jpg" alt="k9Q8IO.jpg" border="0" />

hook分本地hook与服务端hook，本地hook是指git本地操作触发的事件，服务端hook则是需要更远程仓库打交道的时候才会触发。

钩子具体含义请查看文档：[Git - Git Hooks](https://git-scm.com/book/zh-tw/v1/Git-%E5%AE%A2%E8%A3%BD%E5%8C%96-Git-Hooks)

## ESLint
ESLint相信大家都很熟悉，在一些cli项目，都有eslintd的身影，我们也会在IDE上安装eslint插件

## GitLab
Gitlab 是不错的代码托管工具。最新的 8.15 版本加入了自定义 git hook 的支持，在服务器端写脚本控制用户的提交情况变得很方便。

### GitLab配置服务器端 custom hook
#### 1. 修改 gitlab 配置 
在vi `/etc/gitlab/gitlab.rb` ，增加 custom_hooks_dir 路径: 

``` shell
gitlab_shell['custom_hooks_dir'] = "<custom_hooks_dir>"
```

#### 2.执行 
`sudo gitlab-ctl reconfigure`

#### 3.创建 hook 文件
自定义脚本目录要符合 `<custom_hooks_dir>/<hook_name.d>/* `的规范。
根据remote hook我们知道，我们能创建三类：
* pre-receive.d
* update.d
* post-receive.d
> 在每个文件夹下可创建任意文件，在对应的 hook 时期，gitlab 就会主动调用，文件名以 ~ 结尾的文件会被忽略

目录结构示意：

```
[root@localhost custom_hooks]# tree

├── post-receive.d
│   ├── 01.sh
│   └── 02.sh~
├── pre-receive.d
│   ├── 01.sh
│   ├── 02.py
│   └── 03.rb
└── update.d
    ├── 01.sh
    └── 02.sh
```


#### 4.编写hook脚本
> Hook 脚本就是 git 自身的规范，写 shell、python、ruby 都可以。
example
```shell
#!/bin/sh

echo "Say hi from gitlab server. 😄"
exit 0
```

#### 5.测试hook是否生效