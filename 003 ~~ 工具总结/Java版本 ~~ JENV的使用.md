
# 1. Jenv概述
> 官方地址：https://www.jenv.be/

[`jenv`](https://www.jenv.be/) Java管理工具，如nodejs的`nvm`和python的`pyenv`一样，`jenv`可以方便的切换本地的Java版本以方便运行或调试程序。

# 2. Jenv安装与使用
```shell
# 安装jenv
$ brew install jenv
```

> 配置：根据你使用的shell不同，配置方式也不同，使用 `echo $SHELL` 命令，查看当前使用的shell


```shell
#  BASE配置
$ echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(jenv init -)"' >> ~/.bash_profile

# Zsh配置
$ echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.zshrc
$ echo 'eval "$(jenv init -)"' >> ~/.zshrc

# 刷新配置文件
$ source ~/.bash_profile 
$ source ~/.zshrc
```
> Jenv常用命令

```shell
jenv version             # 显示当前应用的java版本和来源
jenv versions           # 列出jenv中全部可用的java版本
jenv add   地址        # 添加java到jenv中
jenv remove 版本号  # 移除jenv中的java版本
java global  版本号   # 显示或设置全局Java版本
java local    版本号   # 显示或设置本地指定项目的 java版本
java shell    版本号   # 显示或设置当前会话的java 版本
```

# 3. jdk安装并使用jenv管理
```shell
# 安装java
brew install openjdk@8
brew install openjdk@11
brew install openjdk@17

#  查看系统安装java地址
brew --prefix openjdk@8
brew --prefix openjdk@11
brew --prefix openjdk@17


# 软连接到共享(可以忽略)
sudo ln -sfn $(brew --prefix)/opt/openjdk@8/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk8.jdk 
sudo ln -sfn $(brew --prefix)/opt/openjdk@11/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk11.jdk 
sudo ln -sfn $(brew --prefix)/opt/openjdk@17/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk17.jdk 

# 添加到jenv中进行管理
jenv add /Library/Java/JavaVirtualMachines/openjdk8.jdk/Contents/Home
jenv add /Library/Java/JavaVirtualMachines/openjdk11.jdk/Contents/Home
jenv add /Library/Java/JavaVirtualMachines/openjdk17.jdk/Contents/Home


# 使用jenv管理java版本：使用 jenv shell | local | global 版本或别名  可以设置当前生效的版本
# 表示全局生效
jenv global 1.8
# 表示在当前目录中生效
jenv local 1.8
# 表示在当前shell会话生效
jenv shell 1.8
```