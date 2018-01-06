## 合并分支（merge  vs rebase）

两用两个例子分别来说一下：

### merge

使用merge合并分支，项目的提交历史记录是树状的。

一个关于merge的小demo 

// 创建开发分支（如果是demo操作可不要，实际开发建议创建）
git checkout -b develop master

// 创建特性分支
git checkout -b feature develop

// 创建一个.txt文件
touch a.txt

// 提交修改记录
git commit -m 'feature add a.txt'

// 切换到develop分支

// 合并feature分支到develop分支
git merge feature （git merge --no-ff feature）

// 删除feature
git branch -d feature


### rebase

rebase 实际上就是取出一系列的提交记录，复制它们，然后在另一个地方逐个的放下去。

rebase的优势就是可以创建更线性的提交历史，如果只使用rebase，代码库的提交历史将变得异常清晰。

一个关于rebase的小demo

// 创建开发分支（如果是demo操作可不要，实际开发建议创建）
git checkout -b develop master

// 创建特性分支
git checkout -b feature develop

// 创建一个.txt文件
touch a.txt

// 提交修改记录
git commit -m 'feature add a.txt'

// 切换到develop分支

// 合并feature分支到develop分支
git rebase feature

// 删除feature
git branch -d feature

### rebase 为何如此受欢迎

正如，有人说rebase虽然很受欢迎，但是它是初学者应该远离的巫毒。但是在实际的团队项目协作开发中，我们会发现它令我们的工作变得轻松。

没有对比就没有伤害，下面就对开发中一些使用用两种命令进行对比。

#### 概念回顾

首先，我们要明确git rebase 和 git merge 解决的是相同的问题。虽然它们都是将一个分支合并到另一个分支，但是他们使用的方式不同。

当你正在使用一个新建的特性分支（feature）来开发一个新功能时，你的团队中另一个成员用一个新的提交更新了主分支，这就产生了一个分叉历史历史。

[分叉历史](https://wac-cdn.atlassian.com/dam/jcr:01b0b04e-64f3-4659-af21-c4d86bc7cb0b/01.svg?cdnVersion=jb)

假设master中的新提交与你正在处理的特性相关。要将新的提交记录合并到feature分支中，我们一般有两种选择:

1.merge

操作命令：

	git checkout feature
	git merge master

	// or

	git merge master feature

这时将会在feature分支中创建一个新的提交记录 （ `merge commit` ），图示如下：

[merge——分支提交记录结构](https://wac-cdn.atlassian.com/dam/jcr:e229fef6-2c2f-4a4f-b270-e1e1baa94055/02.svg?cdnVersion=jb)

merge 是一个非破坏性的操作。现有的分支在任何地方都没有改变。避免了所有潜在的重新建立的风险。

在使用merge合并master中的新提交记录时，我们需要一个额外的和并提交建立。这样可能会污染你的feature分支历史，虽然我们可以使用高级的git日志来缓解这个问题，但是它会给其他开发人员造成理解项目历史困难的问题。

2.rebase

操作命令：

	git checkout feature
	git rebase master

这将是整个特性分支从master分支的顶部开始，有效地将所有的提交都包含在主分支中。同时rebase也不用创建 `merge commit` ，它是将所有的原始分支的每个提交复制一份来创建全新的项目提交历史。

[rebase——分支提交记录结构](https://wac-cdn.atlassian.com/dam/jcr:5b153a22-38be-40d0-aec8-5f2fffc771e5/03.svg?cdnVersion=jb)

rebase 的主要好处就是你可以获得一个更清晰的项目提交历史。没有 `git merge` 所需要的合并提交。它会形成一个完全现行的项目历史，顺着该历史记录你可以很容易的使用git日志，git bisect和gitk等命令来导航你的项目。

对于这个原始的提交历史，有两点你需要注意：安全性和可追溯性

不合理的重新编写项目历史对你的团队工作可能造成很严重的后果；rebase 的合并会丢失分支记录（只保持一条分支）

### 交互式的rebase

交互式的rebase使你拥有更改提交的可能，它提供了对分支的提交历史的完全控制。通常在feature分支合并到master之前，用它来清理混乱的历史。

使用如下命令：

	git checkout feature
	git rebase -i master

然后，将自动打开vim文本编辑器，理出将要移动的所有提交：

### 参考资料

概念回顾：[Merging vs. Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)