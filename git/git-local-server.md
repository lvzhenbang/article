## 本地仓库到仓库管理服务器

注：以常见的github仓库管理服务器为例。

## 常见的操作

1. 从github下载自己所要参与的项目(如：`git clone https://github.com/lvzhenbang/webpack4.x-multi-page.git` 这样的脚本命令可以将 `webpack4.x-multi-page` 项目下载到本地)；
2. 然后，修改本地的项目并将其上传到github。如果是下载后第一次上传，需要先`git add .`，然后 `git commit -m 'init'`，紧接着用 `git push --set-upstream origin master` 和仓库服务器的分支建立联系；如果是不是首次的话，只用 `git push` 就可以和github建立联系，其它的操作和第一次上传相同。
3. 如果是一个全新的项目，然后需要上传到github。本地不管如何操作，但都需要在github上[ `创建一个项目` ](https://help.github.com/en/articles/create-a-repo)。接着说本地如何操作？如果一开始就打算用github管理项目，可以先在github上创建一个项目，然后用 `第二条中操作` 中的方法来实现本地的操作。
4. 如果一开始只是一个demo，之后想着要把它当作一个项目，放在github来进行持续的维护。首先，可以用 `git init` 脚本命令来初始本地项目，然后用 `git add .`, 紧接着用 `git commit -m 'init'`，这样就完成了本地的操作，再然后就是在github创建一个项目，紧接着用`git remote add origin https://github.com/lvzhenbang/webpack4.x-multi-page.git` 和github上新创建的 `webpack4.x-multi-page.git` 建立联系，随后本地项目的修改和操作可参考 `第二条操作` 。
5. 如果后期你的项目向两个或多个方向走，那么可以创建不同的分支，可参考[ `git branch` ](https://github.com/lvzhenbang/article/blob/master/git/git-branch.md)。
6. 如果随着时间的增长分歧越来越大就像[ `angularjs与anglar` ](https://github.com/angular)一样，你需要重新开启一个项目，那么该怎么做呢？简单的方法就是将原项目重新拷贝一份，然后用 `rm -f .git` 脚本命令删除原项目的历史记录（或者手动删除项目根目录下的`.git`文件夹），然后用 `第二中操作` 中的方法来实现本地的操作，紧接着是用 `第四条中操作` 中的相关方法来实现本地项目的上传。
7. 随着新内容或者新技术的引入，抑或旧内容和旧技术的替代，开发中需要用到 [`git tag ***` ](https://github.com/lvzhenbang/article/blob/master/git/git-tag.md)来进行项目的版本管理。 


### 概念

上面的操作是日常开发，尤其是团队合作的项目需要的。这样的操作方法称之为 `软操作`。

当然，说到`软操作`就有入职对应的`硬操作`。从它的名字，就应该知道这种方案粗暴、简单。

这种方案适合个人项目的管理，不适合团队间的项目管理，因为它往往伴随着不可逆转的操作。

### 硬操作示例

上传 : `常见操作` 模块中的 `第二条操作` ，要将本地项目上传到github，第一次需要用 `git push --set-upstream origin master`，当然也可以用`git push -u --force origin master` 。如果创建了新分支第一次也要用 `git push --set-upstream origin master` 这要做不但上传了本地仓库，同时也与github之间建立了联系，之后可以只用 `git push` ；如果同一个分支之后的本地操作还是用`git push -u --force origin master` 那么，这种上传操作其实是一种覆盖，如果是团队合作这样做很危险，它不会管github上是否有新内容，都会上传成功，如果用 `git push --set-upstream origin master` 第一次先建立与github的联系，之后用`git push`上传本地的新内容，如果遇到github上有团队中其它成员新上传的内容，git会提示需要用`git pull`脚本命令拉取github上新内容的提示（这里面临着默认的 `git merge` 操作，然后去除重复的信息）。也可以用`git push origin gh-pages:gh-pages`这种硬操作上传。

不同分支间的操作：在github上有一个特殊的`gh-pages`分支，它可以在github上创建一个静态站，要保证`gh-pages`分支的信息和`master`分支的一致，首先用`git checkout gh-pages`切换到`gh-pages`分支，然后用`git rebase master`脚本命令即可，然后`git push`。也可以用`git merge master`，如果是这种方法比较麻烦，需要经过去重检查和本地的`git add . & git commit -m 'merge'`这两步操作，然后在上传。也可以先删除`gh-pages`分支，然后新建`gh-pages`分支，然后`git push --set-upstream origin gh-pages || git push -u origin gh-pages`。如果是`gh-pages`可以不用管是否是团队合作，如果是其他分支，团队合作用`git merge`，个人可以用`git rebase master` 。删除分支这种方法在团队合作和个人开发中都很少用。