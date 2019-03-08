## git 常用命令

从一个demo开始学习git。

首先，创建一个项目根目录`myApp`，命令如下：

```
mkdir myApp
```

然后，进入到该目录下，命令如下：

```
cd myApp
```

紧接着，用`git`初始化该项目，命令如下：

```
git init
```

再然后，在项目中任意添加一个文件，如：`README.md`，可以使用如下脚本命令：

```
touch README.md
```

紧接着，可以使用`vim`或者其它编辑工具，在`README.md`文件中随意添加一些内容，并保存；

再然后，将新添加的文件，添加到缓存区中，命令如下：

```
git add README.md
```

紧接着，编译项目，命令如下：

```
git commit -m 'add: readme.md'
```

再然后，在github上创建一个`myApp`的同名项目，可参考[ `create repository` ](https://help.github.com/en/articles/creating-a-new-repository)。


拷贝项目地址后，如: `https://github.com/lvzhenbang/myApp`，然后执行如下命令，就可以与github上的`myApp`项目建立联系：

```
git remote add origin https://github.com/lvzhenbang/myApp.git
```

再然后，执行如下脚本命令，就可以将本地编译后的项目上传到github项目管理服务器：

```
git push -u origin master
```

完整的命令如下：

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
