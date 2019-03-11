# git分支操作

包括创建分支，切换分支，删除分支，查看分支，合并分支等常见操作。

## 创建分支

包括创建本地分支和创建远程分支。

### 创建本地分支

脚本命令：git branch [分支名字]

demo: `git branch lzb`

注：git中的HEAD指针可以让我们知道当前所在的本地分支

注2：日常开发中，经常会用到创建一个分支并跳转到该分支的情况，需要添加`-b`这样一个参数，脚本命令修改成 `git checkout -b [分支名字]`这样。

注3：` git checkout -b [branch-name] origin/branch-name] `，从远程已有的分支创建本地分支，并且换到新创建的本地分支。

### 创建远程分支

描述：创建远程分支分支的有多种手段。

服务器端：可以在`git 服务器`端创建，如github，可参考[https://help.github.com/en/desktop/contributing-to-projects/creating-a-branch-for-your-work#creating-a-branch]。

客户端：

第一种方法：git push -u origin [远程分支名字:所在本地分支名字]

demo: `git push -u origin lzb:lzb`

第二种方法：git push --set-upstream origin lzb:lzb

demo: `git push --set-upstream origin lzb`

第三种方法：从远程服务器（github）创建分支，并且换到本地分支

demo: `git checkout -b lzb origin/lzb`

## 切换分支

* git checkout [分支名字]
* git checkout -b [分支名字] 切换到某分支，如果没有该分支，就先创建这个分支，然后切换到该分支

## 查看分支

包括查看本地分支和查看远程分支

* git brranch -a // 查看本地仓库的分支，以及远程仓库的分支
* git branch -vv // 查看本地仓库和远程仓库彼此分支关联的情况

### 产看本地分支

* git branch  // 输出本地所有分支名字
* git branch -v // 输出每一个分支的最后一次提交
* git branch --merged/--no-merged  // 输出已经合并和尚未合并到当前分支的分支

### 查看远程分支

* git branch -r // 列出所有的远程分支
* git remote show [url] [分支名] // 查看某远程分支的详细信息

## 删除分支

包括删除本地分支、删除远程分支

### 删除本地分支

git branch -d [分支名字]

注：删除一个分支，需要先确定该分支已经被合并到了master（主分支）分支上

注2：需要先切换到主分支

### 删除远程分支

* 方法一：

脚本命令：git push [url] -d [branch-name]

demo：`git push https://github.com/lvzhenbang/article.git -d lzb` 或者 `git push git@github.com:lvzhenbang/article.git -d lzb`

注：开发中常用`HTTPS`，使用`SSH`需要输入`RSA key`

* 方法二：

脚本命令：git push origin -d [远程分支名字]

demo: `git push origin -lzb`

* 方法三：

脚本命令：git push origin :heads/[远程分支名字]

demo: `git push origin :heads/lzb`

## 合并分支

例如，将功能分支（lzb）合并到master分支，需要用到如下命令：

```
git checkout master
git merge lzb
```

注：如果你想要将分支`A`合并到分支`B`上，也就是说让原本没有`A`分支上的记录的`B`分支拥有`A`分支的记录，需要先切换到`B`分支（命令：git checkout B），然后执行`git merge A`的命令。

## 重命名分支名字

### 重命名本地分支名字

脚本命令：git branch -m [分支新名字]
