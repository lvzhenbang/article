## git 常用命令

从一个demo开始学习git，我们首先创建一个项目，然后用 `git init` 命令初始化它，我们新建一个文件，然后将文件添加到缓存区，紧接着编译项目，下一步连接本地仓库和远程仓库，推送demo。

这样就将一个demo上传到GitHub，或其他git版本管理服务器上。

下面我们就具体来认识一下git的常用命令。

### 一个demo

```
mkdir myApp
cd myApp
git init
touch README.md
vim README.md
git add README.md
git commit -m 'by lzb'
git remote add origin git@github.com:lvzhenbang/myApp.git
git push -u origin master

```

***********************************************************

### 查看分支

* 查看本地分支：git branch 
* 查看远程分支：git branch -r

### 创建分支

* 创建本地分支：git branch [分支名称]
* 创建远程分支：git push --set-upstream origin [分支名称]

### 删除分支

* 删除本地分支：git branch -d [分支名称]
* 删除远程分支；git push origin :heads/[分支名称]


************************************************************

### 版本的切换

* git reset --hard [版本号]
* git ref log 可查看版本号

### 版本的合并

* git merge [分支名]

### 暂存工作区 切换到其他分支修改bug

* git stash 备份当前工作区的内容到git栈中
* git stash pop 从git栈中读取最近一次保存的内容，并恢复到工作区
* git stash list 显示git栈中的所有备份，可以利用这个列表来决定从那个地方恢复
* git stash clear 清空git栈

### git取消操作命令

* commit: git commit --amend
* stage: git reset HEAD [上一次 add 的文件名]
* modify: git checkout -- [上一次 modify 的文件名]

### git 移动文件

git mv [源文件] [目的文件] // 也可以用来修改文件的名字

```
mv 源文件 目的文件
git rm 源文件
git add 目的文件
```

### 远程仓库的使用

管理远程仓库包括查看远程仓库、添加远程仓库、移除无效的远程仓库、管理不同的远程分支并定义他们是否被跟踪

#### 查看远程仓库

git remote
-v 显示需要读写的远程仓库使用的git保存的简写与对应的URL

#### 添加远程仓库

git remote add [shortname] [url] 

添加远程仓库后我们可以获取相应git保存简写的数据
git fetch [shortname]

#### 推送到远程仓库

git push [remote-name] [branch-name] // git push origin master

#### 查看远程仓库

git remote show [remote-name] // git remote show origin

#### 远程仓库的移除和重命名

* git remote rm [remote-name] // git remote rm origin
* git remote rename [remote-name:old] [remote-name:new] // git remote rename pb paul

### 标签

#### 查看所有标签

git tag

#### 创建标签

* 附注标签 git tag -a [v1.4] -m ['my version is 1.4']
* 查看标签 git show v1.4
* 轻量标签 git tag v1.4-lw

#### 共享标签

git push origin v1.5

#### 输出标签

git checkout -b version2 v2.0.0

### 别名

git config --global alias.[命令别名] ’git的相关的命令‘
