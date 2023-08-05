







```shell

Disk（本地代码）

	init    查看修改：  git diff
		                     git status -> Changes not staged for commit
		    撤销修改：  git checkout <changed_file>    (旧版git)
							 git restore  <changed_file>      (新版git)

Staging（git add暂存区）

	init    查看修改：  git status -> Changes to be committed
			撤销修改：  git reset <changed_file>          (旧版git)               不会对源代码本身进行修改
							 git restore --staged <changed_file>   (新版git)    不会对源代码本身进行修改
							 git checkout HEAD <changed_file>     丢失硬盘上的修改


Local（本地库）

	init   撤销commit:  git reset --soft HEAD~1     (不会对源代码进行修改)

Remote（远程库）

	init 

```
# 1. Git Merge  Rebase区别

