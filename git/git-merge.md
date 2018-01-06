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

然后，将自动打开vim文本编辑器，修改提交记录顺序即可


### rebase 的黄金规则

如果你一旦明白了什么是重新建立，最重要的是了解什么时候不去做。
git rebase 的黄金规则就是永远不要在公共分支上使用它。
举个例子，如果你叫master放到feature分支上，你会想到放生什么：

[图解](https://wac-cdn.atlassian.com/dam/jcr:1d22f018-b2c7-4096-9db1-c54940cf4f4e/05.svg?cdnVersion=jb)

rebase 将所有的提交都移动到feature分支的顶端。完成后，这个的改变只会保存在你的本地repository，而其他的开发人员，还是用原来的master分支，你的master分支与其他人将不一样，要同步你的master和其他人的master的方法是将它们合并在一起，从而产生一个额外的合并提交和包含连个相同更改的提交（原始的变更，以及来自你用rebase的分支变更），这样会造成一个混乱的局面。在使用rebase前我们要问自己当前分支是否有其他人看，如果有尝试使用一种非破坏性的方式来实现合并。

### Force-Pushing

如果这时你想远程版本管理服务器推送本地仓库，你将发现请求被阻止，因为存在着冲突，这也就是上文所说的要同步自己和其他人的库要先进行一个额外的合并提交，冲突原因是你本地的提交记录列表中有和远程仓库提交记录列表中位置相同但内容不同的提交记录。所以会冲突，并且提示信息会提醒你要合并两个分支。

但是，你可以强行将本地仓库推送到远程仓库。

git push --force (慎重使用此命令)

这样会重写远程分支来匹配你的本地仓库创建的新分支（rebase），这样的操作会使你的队友感觉很混乱。所以，使用时你一定要知道你在做什么，同时操作时要万分小心。

### 工作流程介绍

将rebase添加到你的现有工作流中，这可能时你的团队所乐意接受的。那么，我们就聊聊rebase在项目功能开发的各个阶段所带来的好处。

第一步，我们为每一个功能特性创建一个feature分支，这提供了必要的分支结构，让你可以安全的使用rebase；

[示图](https://wac-cdn.atlassian.com/dam/jcr:6af9de07-088b-4f8b-97a7-b66569a9e4ac/06.svg?cdnVersion=jb)

#### 本地清理

在你的工作流程中加入rebase的最好办法就是清理本地的、正在开发的特性。通过定期执行一个交互式的rebase，你可以确保你的feature中的每一个提交都是重点和有意义的。这样你就可以不用担心你写的代码破坏了相关的提交，你可以在事后修补它。

在调用 `git rebase` 时，你有两个新基础选项：特性功能的父分支（master），你的feature中早期提交。我们在交互式的rebase中看到了第一个选项示例。但你只需要修复最后几个提交时，后一种方法就很好。

	git cheeckout feature
	git rebase -i HEAD~2

[rebase后新分支](https://wac-cdn.atlassian.com/dam/jcr:079532c4-2594-40ed-a5c4-0e3621b9edff/07.svg?cdnVersion=jb)

如果你想要使用此方法重新编写整个特性，`git merge-base` 命令可以很方便的找到feature分支的原始基础。获取返回的原基的提交ID(哈希值)，然后你可以将其传递给 `git rebase`。

	git merge-base feature master

使用交互式的rebase是将 `git rebase` 引入工作流的一种很好的方法，因为他只影响本地分支，因为，其他开发者只会看到你的成品，它应该是干净的、易于跟随的特性分支历史。

很明显，这只适用于私有的分支。如果你通过相同的feature（功能）分支与其他开发人员协作，该分支是公开的，那么你将不被允许重写它的历史。

在使用交互式的rebase进行本地提交时，没有个i他合并选项。

### 将上游分支合并到一个功能分支

我们在上文中已经说了一个feature分支如何使用 `git merge` 或 `git rebase` 来合并来自master的上游修改。merge是一个安全的方法，它可以保存你的本地库中的整个历史，而rebase通过将你的特性分支移动到master的顶端来创建一个线性历史。

`git rebase` 的使用类似于本地清理，在工作流中它整合了来自master的上游提交。

在远程分支而非在本地master中rebase是合法的。这可能发生在你与另一个开发人员协作时，你需要将他的变更合并到你的存储中。

例如：如果你和一个叫John的开发人员提交了feature分支，那么远程从John的存储库中拉取feature分支时，你的存储库如下所示：
	
[](https://wac-cdn.atlassian.com/dam/jcr:0bb661aa-361d-47ba-8c7b-00b3be0546cb/08.svg?cdnVersion=jb)

两种合并方法

[merge](https://wac-cdn.atlassian.com/dam/jcr:1896adb1-5d49-419a-9b50-3a36adac186c/09.svg?cdnVersion=jb)

注意：rebase并没有违背rebase的黄金法则，因为只有你的本地feature是被移动的，在此之前一切未被修改。在大多数情况下这笔使用merge提交与远程分支同步更直观。默认情况下，`git pull` 执行一个合并，但你可以强制它将远程分支与一个rebase集成，通过给它传递 `--rebase` 参数项。

#### 用Pull Request检查feature

如果你使用pull请求作为代码审查的一部分，那么你需要在创建pull请求后避免使用 `git rebase` 。一旦你提出了pull请求，其他开发人员，将会查看到你的提交，这说明中特分支是一个公共分支。重新编写它的lisih提交记录，将使你的同时不能跟踪到任何添加到feature的后续提交，与此同时其他开发人员的任何更改都需要用 `git merge` 而非 `git rebase`。

#### 整合一个被批准的 feature

在你的团队批准了一个特性（feature）之后，你可以使用 `git merge` 将特性(feature)在集成到主代码库之前，将该特性重新放到master分支的末端。

这是一个和将上游更改合并到一个特性分支中类似的情况，但是由于你不允许在主分支中重写提交，所以你必须最终使用 `git merge` 来集成该特性。但是，在合并之前执行一个rebase，你可以确信合并将会被快速转发，从而导致一个完美的线性历史。这也为你提供了在pull请求中添加任何后续提交的机会。

[feature](https://wac-cdn.atlassian.com/dam/jcr:df39b1f1-2686-4ee5-90bf-9836783342ce/10.svg?cdnVersion=jb)

如果你对 `git rebase` 不那么熟悉，你可以创建一个临时分支，用它执行rebase，这样的话一旦你不小心弄乱了你的特性历史，你可以检查原来的的feature分支，然后再尝试一次，代码如下：

	git checkout feature
	git checkout -b temporary-branch
	git rebase -i master
	git checkout master
	git merge temporary-branch

### 参考资料

概念回顾：[Merging vs. Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)