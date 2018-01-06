## 提交的技巧

### 技巧一

场景描述：

在我们提交记录后，有时会发现某些文件也需要进行提交，这是我们就需要撤销刚才的提交。

命令代码：

	// 刚才提交
	git commit -m 'initial commit'
	// 将想要添加的文件加入缓存
	git add forgotten_file
	// 修正提交
	git commit --amend

### 技巧二

场景描述：

创建一个feature分支，在该分支上做了一次提交，然后又基于该分支创建了feature-2分支，然后有提交了一次。此时我们返回feature分支做了一些修改，虽然这个分支的提交记录并不是最新的。

修改前后提交记录对比图示：

![原提交记录](https://github.com/lvzhenbang/article/blob/master/img/git/commit/1-r.png)

![修改结果](https://github.com/lvzhenbang/article/blob/master/img/git/commit/1-d.png)

解决方案

* 首先，用 git rebase -i 将提交重新排序，然后把我们想要修改的提交记录挪到最前
* 然后，用 commit --amend 来进行一些小修改
* 接着，用 git rebase -i 来将他们调回原来的顺序
* 最后，把master 移动到修改的最前端即可

命令代码；

	// 移动哈希值为C2的提交记录
	git commit --amdend
	// 将HEAD向前移动2个提交记录的位置
	git rebase -i HEAD~2
	// 强行将master分支，移动到HEAD所指向的位置 
	git branch -f master 

### 技巧三

注：这个技巧相对于技巧一我们不需要对提交记录进行排序，我们只需要将所需要的提交记录挪到最前端就行了，这样就可以重新排序成我们所需要的顺序。

场景描述：

创建一个feature分支，在该分支上做了一次提交，然后又基于该分支创建了feature-2分支，然后有提交了一次。此时我们返回feature分支做了一些修改，虽然这个分支的提交记录并不是最新的。

修改前后提交记录对比图示：

![原提交记录](https://github.com/lvzhenbang/article/blob/master/img/git/commit/2-r.png)

![修改结果](https://github.com/lvzhenbang/article/blob/master/img/git/commit/2-d.png)

解决方案

* 首先，用 git cherry-pick <哈希值>... 将提交重新排序，然后把我们想要修改的提交记录挪到最前
* 然后，用 commit --amend 来进行一些小修改
* 最后，用 git cherry-pick <哈希值>... 将刚才的修改后的提交重新排序，即可完成目标

命令代码；

	// 移动哈希值为C2的提交记录
	git cherry-pick C2
	// 撤销修改提交记录
	git commit --amend 
	// 将哈希值为C3的提交记录挪到最前端
	git cherry-pick C3

