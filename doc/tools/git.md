## Git

> 分布式版本控制系统

### 基本概念

```
工作区
	本地存放文件的目录
版本库
	目录下的.git
暂存区
	通过git add命令，将文件添加到暂存区
分支
	通过git commit命令，将暂存区文件提交到分支
```





### 基本命令

```
初始化Git本地仓库
    cd C:/git	// 切换到C盘git目录下
	git init	// 初始化当前目录为git仓库
把文件添加到暂存区
	git add
把文件提交到本地仓库
	git commit
查看当前仓库的状态
	git status
查看修改内容
	git diff
显示从最近到最远的提交日志
	git log
回退到HEAD版本：HEAD指向的版本就是当前版本
	git reset --hard <HEAD>
查看命令历史
	 git reflog
查看工作区和版本库里面最新版本的区别
	git diff HEAD --<file>
撤销文件在工作区的修改
	git checkout --<file>
删除一个文件
	git rm <file>
```



### 远程仓库



#### 同时提交多个远程仓库

修改 `.git` 目录下的 `config` 文件

在 `remote` 下添加远程分支的 `url` 

```
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
	symlinks = false
	ignorecase = true
[remote "origin"]
	# github address
	url = git@github.com:zhangzhibo1014/MyNote.git
	# gitee address 在这里加上gitee仓库的url
	url = git@gitee.com:it_zhangzhibo/MyNote.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
```

