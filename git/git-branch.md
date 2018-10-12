## git分支

git 的优势：

1. 创建的分支轻量
2. 创建的分支速度快，几乎瞬间完成
3. 便捷的在不同的分支之间切换


git鼓励在工作流程中频繁地使用分支和合并

### 查看分支

* git branch
* git branch -v 查看每一个分支的最后一次提交
* git branch --merged/--no-merged  查看已经合并和尚未合并到当前分支的分支

### 查看远程分支

* git branch -r // 远程分支列表
* git remote show [url] [分支名] // 查看某远程分支的详细信息


### 创建本地分支

* git branch [分支名字]

注：git中的HEAD指针可以让我们知道当前所在的本地分支

### 切换分支

* git checkout [分支名字]
* git checkout -b [分支名字] 创建并切换到创建分支

### 创建远程分支

创建远程分支需要有一个名字形同的本地分支，然后切换到该本地分支，执行下面命令

git push -u origin [远程分支名字/所在本地分支名字]

### 项目分叉历史

git log --oneline --decorate --graph --all

注: Git 的分支实质上仅是包含所指对象校验和（长度为 40 的 SHA-1 值字符串）的文件，所以它的创建和销毁都异常高效。创建一个新分支就相当于往一个文件中写入 41 个字节（40 个字符和 1 个换行符）

### 合并分支

例如，将功能分支（lzb）合并到master分支：

git checkout master

git merge lzb

注：在实际的开发项目中不建议使用这种命令，我们往往使用pull merge请求来合并新建的分支，这个请求的过程多了一个人工审核的过程。

注2：需要先切换到主分支

### 跟踪分支 

当克隆一个仓库时，它通常会自动地创建一个跟踪 origin/master 的 master 分支。
当你想创建远程仓库上其他分支的跟踪分支，可以使用如下命令

git checkout -b [本地分支名字] origin/[远程分支名字]

简写：

git checkout --track origin/[分支名字]

设置已有的本地分支跟踪一个刚刚拉取下来的远程分支，或者想要修改正在跟踪的上游分支

git branch -u[--set-upstream-to] origin/[分支名字]

查看所有的跟踪分支 git branch -vv


### 删除分支

git branch -d [分支名字]

注：删除一个分支，需要先确定该分支已经被合并到了master（主分支）分支上

注2：需要先切换到主分支

### 删除远程分支

* git push [url] -d [branch-name]
* git push origin :heads/[远程分支名字]

### 合并冲突

git mergetool

解决冲突的方案：

	git status 查看状态

	找到冲突的地方，修改存在冲突的问题

	再次，git status 查看状态

### 获取远程分支信息

git pull origin [远程分支名字]

抓取数据 git fetch [地址] [分支名字]

