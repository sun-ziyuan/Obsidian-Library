有个很好的在线学习网站： [https://learngitbranching.js.org/](https://learngitbranching.js.org/)






# 2. Git常用命令 ~~ 代码提交和同步代码

```shell
# 1. 工作区与仓库保持一致
# 2. 文件增删改，变成已修改状态
git status                                 # 查看状态 

# 3. git add，变为已暂存状态
git add -all                               # 当前项目下的所有更改
git add .                                   # 当前目录下的所有更改
git add xx/xx.java xx/xx2.java     # 添加某几个文件

# 4. git commit 改变已提交状态
git commit -m "<这里写commit的描述>"

# 5. git push，变为已推送状态
# -u：在进行推送代码到远端分支，且之后希望持续向该远程分支推送，则可以在推送命令中添加 -u 参数，简化之后的推送命令输入
git push -u origin master          # 第一次提交需要关联上
git push                                  # 之后再次推送就不用指明应该推送那个远程分支了



git brach                                  
git brach -a 
```



### 1. git常用操作--git rebase和git merge


git 回退

1. 已提交,没有push
	git reset --soft         撤回commit
	git reset --mixed      撤回commit和add两个动作

2. 已提交，并且push
	git reset --hard  撤回并舍弃版本号之后的提交记录，使用需要谨慎。
	git revert            撤回，但是保留了提交记录。
