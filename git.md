## git 常用命令

### 查看分支

查看本地分支：git branch 

查看远程分支：git branch -r

### 创建分支

创建本地分支：git branch [分支名称]

创建远程分支：git push --set-upstream origin [分支名称]

### 删除分支

删除本地分支：git branch -d [分支名称]

删除远程分支；git push origin :heads/[分支名称]


************************************************************

### 一个demo

```
mkdir myApp
cd myApp
git init
touch README.md
vim README.md
git commit -m 'by lzb'
git remote add origin git@github.com:lvzhenbang/myApp.git
git push -u origin master

```

***********************************************************
### 切换远程的仓库

git remote set-url --push origin ［new remote repository


### 版本的切换

git reset --hard [版本号]

git ref log 可查看版本号

### 版本的合并

git merge [分支名]

### 暂存工作区 切换到其他分支修改bug

git stash
git stash pop

