### 在开发时如何获取远程仓库的所有分支

	git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote"; done
	git fetch --all
	git pull --all

### 在开发时如何获取远程仓库中除了master主分支外的其它分支？

	git checkout -b [本地分支名字] origin/[远程分支名字]