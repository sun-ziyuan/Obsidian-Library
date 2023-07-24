# 1. Nvm概述
	NVM（Node Version Manager）是一个用于在基于Linux系统上安装和管理Node.js的shell脚本。MacOS用户可以使用 homebrew 来安装NVM.

# 2. NVM安装

```shell
# 删除现有node版本
brew uninstall --ignore-dependencies node 
brew uninstall --force node

# 安装nvm版本
brew update 
brew install nvm

# 配置环境变量
echo "source $(brew --prefix nvm)/nvm.sh" >> .bash_profile
echo "source $(brew --prefix nvm)/nvm.sh" >> .zshrc
source ~/.bash_profile
source ~/.zshrc

# 安装node
nvm install 8.14.0


```
> nvm常用命令概述

```shell

nvm install 8.14.0               # 安装指定版本node
nvm install stable              # 安装最新稳定版本node
nvm uninstall 8.14.0           # 卸载指定版本node

nvm ls                             # 列出所有安装的版本
nvm ls-remote                 # 列出所有官方的历史版本
nvm current                    # 显示当前版本
nvm use 8.14.0                # 切换使用指定的版本node
nvm -v                           # 查看nvm版本

nvm list                          # 是查找本电脑上所有的node版本 
	- nvm list                   # 查看已经安装的版本 
	- nvm list installed      # 查看已经安装的版本 
	- nvm list available     # 查看网络可以安装的版本


nvm reinstall-packages   # 在当前版本 node 环境下，重新全局安装指定版本号的 npm 包
```