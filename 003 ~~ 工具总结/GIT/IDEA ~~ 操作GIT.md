有个很好的在线学习网站： [https://learngitbranching.js.org/](https://learngitbranching.js.org/)






# 2. Git常用命令 ~~ 代码提交和同步代码

```shell
git status                                 # 查看状态 
git relog                                  # 查看版本信息
git log                                     # 查看详细版本信息
git init                                     # 初始化本地库
```


```shell

# 1. 克隆
git clone 远程地址                                                  # 将远程仓库的内容克隆到本地
git clone -b 分支 远程地址                                       # 将远程仓库分支的内容克隆到本地
git clone http://username:password@github.com/      # 通过用户名和密码进行克隆

# 2. 添加到暂存区
git add -all                                              # 当前项目下的所有更改
git add .                                                  # 当前目录下的所有更改
git add xx/xx.java xx/xx2.java                    # 添加某几个文件


# 3. 提交到本地仓库
git commit -m "<这里写commit的描述>"     # 提交暂存区到仓库区
git commit -a -m '第二次提交'                   # add + commit 两次操作

# 4. 推送到远端仓库
# -u：在进行推送代码到远端分支，且之后希望持续向该远程分支推送，则可以在推送命令中添加 -u 参数，简化之后的推送命令输入
git push -u origin master                         # 第一次提交需要关联上
git push                                                 # 之后再次推送就不用指明应该推送那个远程分支了


# 5. Git的分支命令
git branch.                                             # 查看当前分支
git branch 分支名称                                 # 创建分支
git branch -v                                         # 查看分支 
git branch -a                                         # 查看所有分支
git branch -d 分支名称                            # 删除分支

# 6. Git 的 切换分支
git checkout 分支名称                              # 切换分支
git checkout -b 分支名称                          # 创建并切换分支
git merge  分支名称                                 # 合并分支

# 7. 远程仓库命令
git remote -v                                          # 查看当前所有远程地址别名
git remote add 别名 远程库                       # 添加别名
git remote rm  别名                                  # 删除别名
git remote rename old_name new_name     # 修改别名

```



```shell
# 代码没有add到暂存区 撤销方式（删除操作）
git checkout Xxx.java               # 对单个文件进行撤回
git checkout .                         # 对所有文件撤回

# 代码没有commit到本地仓库 撤销方式
git reset                                # 暂存区的修改恢复到工作区
git reset --soft                      # 与 git reset 等价，回到已修改状态,修改的内容仍然在工作区中
git reset --hard                     # 回到未修改状态, 清空暂存区和工作区(删除文件修改)

# 代码没有 push 到远程仓库，撤回方法
git reset --soft  166cdab        # 撤销commit到 暂存区
git reset --mixed  166cdab     # 撤回commit和add两个动作


# 代码以push到远程仓库
git reset --hard (回退版本号)             # 撤回并舍弃版本号之后的提交记录，使用需要谨慎;
git push -f                                       # 强制提交
git pull                                             # 再次拉取代码

git revert (当前版本号)                        # 撤回，但是保留了提交记录；复制去掉的版本号
git push  







git reset --hard origin/master						# 回退与本地远程仓库一致
git reset --hard HEAD^								# 回退到本地仓库上一个版本
git reset --hard <hash code>						# 回退到任意版本
git reset --soft/git reset								# 回退且回到已修改状态,修改仍保留在工作区中;







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
### 1. git常用操作--git rebase和git merge

