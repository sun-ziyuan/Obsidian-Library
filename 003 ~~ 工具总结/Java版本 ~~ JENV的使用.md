
# 1. Jenv概述
> 官方地址：https://www.jenv.be/

[`jenv`](https://www.jenv.be/) Java管理工具，如nodejs的`nvm`和python的`pyenv`一样，`jenv`可以方便的切换本地的Java版本以方便运行或调试程序。


# 2. Jenv安装与使用
```shell
# 安装jenv
$ brew install jenv

# 安装完成后配置环境变量
$ echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(jenv init -)"' >> ~/.bash_profile

$ echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.zshrc
$ echo 'eval "$(jenv init -)"' >> ~/.zshrc


# 刷新配置文件
$ source ~/.bash_profile 
$ source ~/.zshrc

# 
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

# 使用jdk

```