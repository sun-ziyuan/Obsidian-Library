# 1. Git概述
Git 是一个免费的、开源的分布式版本控制系统，可以快速高效地处理从小型到大型的各种 项目。
Git 易于学习，占地面积小，性能极快。 它具有廉价的本地库，方便的暂存区域和多个工作 流分支等特性。其性能优于 Subversion、CVS、Perforce 和 ClearCase 等版本控制工具。

# 2. 安装与卸载

```shell
# 安装git
brew install git

# 卸载
brew uninstall git
```

# 3. Git配置
`Git`提供了一个 `git config` 命令用来配置或读取对应的工作环境变量。 这些变量可以存放在以下三个不同的地方：

|权重|中文名|优先级|配置文件|
|---|---|---|---|
|system|系统级别|低|`/etc/gitconfig`|
|global|全局级别|中|`~/.gitconfig`|
|local|仓库级别|高|`.git/config`|
	对于 git来说，配置文件的权重是 仓库 > 全局 > 系统  
	设置全局配置文件存放位置： win 位置：C:\Users\szy\.gitconfig；      mac位置：~/.gitconfig
## 3.1 设置签名

参考文档：[初次运行Git的配置](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%88%9D%E6%AC%A1%E8%BF%90%E8%A1%8C-Git-%E5%89%8D%E7%9A%84%E9%85%8D%E7%BD%AE) ，一般在新系统上，我们都需要先配置下自己的 Git工作环境。配置工作只需要一次，以后升级时还会沿用现在的配置。当前，如果需要，你随时可以用相同的命令修改已有的配置。

	Git 首次安装必须设置一下用户签名，否则无法提供提交代码。

```shell

# 注意：在这设置用户签名和将来登录GitHub(或其他托管中心)的账号没有任何关系,只是记录谁提交的更新
git config --global user.name "szy"							
git config --global user.email "ziyuan710@163.com"	      

# 在某个特定的项目中使用其他名字和邮箱进行提交，新配置的保存在 .git/config 文件中
git config user.name   用户名
git config user.email  邮箱

# 查看配置 根据权重进行查看  仓库 > 全局 > 系统
git config user.name
git config user.email  
```
## 3.2 设置代理

代理在网络通畅的情况下可以不必设置，但是在网络不好的情况下，设置代理可以有效的提高Git的下载速度。无论是HTTP代理还是HTTPS代理。使用代理都需要以地址和端口号作为参数，如果代理需要身份验证，则还需要提供用户名和密码。设置、取消和检查代理配置都可以使用Git命来令实现。

```shell
# 设置代理
git config --global http.proxy 127.0.0.1:7890
git config --global http.proxy 用户名:密码@代理地址:端口号

git config --global https.proxy 127.0.0.1:7890
git config --global https.proxy 用户名:密码@代理地址:端口号

# 取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy

# 查看当前代理
git config --global --get http.proxy
git config --global --get https.proxy

```
## 3.3 指令设置

```shell
# 查看配置 命令参数 -list,  简写 l  
# 格式：git config [-local | -global | -system] -l
git config -l				# 查看当前生效配置
git config -local -l		# 查看当前仓库配置
git config -global -l		# 查看当前全局配置
git config -system -l		# 藏看当前系统配置


# 编辑配置 命令参数 edit,   简写 e    
# 格式：git config [-local | -global | -system] -e
git config -e 				# 编辑仓库配置文件
git config -local -e		# 编辑仓库配置文件
git config -global -e		# 编辑全局配置文件
git config -system -e		# 编辑系统配置文件 


# 添加配置 命令参数 add  	 注意 add后面 section.key value 一个都不能少，否则添加失败
# 格式：git config [-local | -global | -system] -add section.key value(默认添加在local配置中)
git config -local -add szy.name szy			# 添加仓库配置
git config -global -add szy.name szy		# 添加全局配置
git config -system -add szy.name szy		# 添加本地配置

# 删除配置 命令参数 unset  
# 格式：git config [-local | -global | -system] -unset section.key
git config -local -unset szy.name		  	# 删除仓库配置
git config -global -unset szy.name			# 删除全局配置
git config -system -unset szy.name			# 删除本地配置
```

# 4. Git凭证存储

> 参考文献：https://blog.csdn.net/ttyy1112/article/details/107863210

如果你使用 HTTP/HTTPS 协议访问仓库，那么每一次连接都需要用输入用户名和密码，在使用双重认证的情况下会更麻烦；要想对密码进行保存，需要借助Git的凭证系统来处理这个事情； 凭证：Git账户记录(用户名、密码信息)
```shell
# 远程使用密码获取 Git
http://username:password@github.com/sun-ziyuan/Obsidian-Code
```
## 4.1 凭证系统

Git支持四种形式的凭证模式：

| 模式          | 说明                                                                                                              |
|:------------|:----------------------------------------------------------------------------------------------------------------|
| 默认不缓存       | 每一次连接都会询问你的用户名和密码                                                                                               |
| cache       | 会将凭证存放在内存中一段时间。密码永远不会被存储在磁盘中，并且在15分钟后从内存中清除。                                                                    |
| store       | 会将凭证用明文的形式存放在磁盘中，并且永不过期。这意味着除非你修改了你在Git服务器上的密码，否则你永远不需要再次输入你的凭证信息。这种方式的缺点是你的密码是用明文的方式存放在你的 home 目录下。            |
| osxkeychain | macOS 中安装的 Git 一般默认都是该模式，它会将凭证缓存到你系统用户的钥匙串中。这种方式将凭证存放在磁盘中，并且永不过期，但是是被加密的，这种加密方式与存放 HTTPS 凭证以及 Safari 的自动填写是相同的。 |
| manager     | Windows 中安装的 Git 一般默认都是该模式，它会将凭证缓存在系统的凭证管理器中。这种方式将凭证存放在磁盘中，并且永不过期，同样也是加密的。                                      |  

```shell
# 查看当前使用的 Git凭证模式
git config credential.helper

# 设置Git凭证模式
git config --global credential.helper 凭证模式
git config --global credential.helper store 

# 取消设置 Git 凭证
git config --global --unset credential.helper
```

## 4.2 cache模式

```shell
# 设置当前 GIt凭证模式 默认缓存15分钟
git config --global credential.helper cache

# 设置全局缓存时长.(秒数)，表示缓存1小时 
git config --global credential.helper 'cache --timeout=3600'

# 指定设置当前项目的缓存时长
git config credential.helper 'cache --timeout=3600'  

# 查看配置信息
cat ~/.gitconfig          # 存放位置
git config --list         # 查看配置信息
```

## 4.3 store模式
> 删除凭证后，下次登录时会重新提示输入用户名、密码;

```shell
# 设置 store存储
git config --global credential.helper store

# 查看配置信息
cat ~/.gitconfig

# 通过git命令进行远程操作（如git pull）时，会提示输入用户名、密码，验证成功后会自动保存到 ~/git-credentials中
cat ~/.git-credentials

# 删除凭证
git credential reject

# 删除所有凭证
sudo rm ~/.git-credentials

# 删除某个凭证 (直接进行修改，删除不需要的凭证)
vim ~/.git-credentials
```
## 4.4 osxkeychain模式

```shell
# 如果没有安装osxkeychain辅助工具，则先安装osxkeychain工具
curl http://github-media-downloads.s3.amazonaws.com/osx/git-credential-osxkeychain -o git-credential-osxkeychain
sudo mv git-credential-osxkeychain /usr/local/bin
sudo chmod u+x /usr/local/bin/git-credential-osxkeychain

# 设置 osxkeychain (秘钥串)存储
git config --global credential.helper osxkeychain

# 删除凭证
git credential reject
```

	正常情况下，我们在通过git命令进行远程操作(git pull)时，会提示输入用户名、密码；验证成功后该凭证会保存到系统的钥匙串中，凭证保存后，下次进行远程操作时就不会提示输入用户名、密码。
	
![](https://cdn.nlark.com/yuque/0/2022/png/21644188/1662300937185-444b2b4a-fa5f-46fa-95fe-58bddae509bd.png)

## 4.5 manager模式

```shell
后续补上，暂时用不上
```


## 4.6 命令行操作 ~~ 多模式共存情况下的处理

>以上两个示例均采用单一凭证处理模式，其实是可以同时采用多种凭证存储模式的

**如果同时存在两种凭证存储方式**
- **查询时会按顺序执行各凭证辅助工具进行查询，遇到第一个应答则退出查询;**
- **保存时会调用所有的凭证辅助工具，各辅助工具根据自己的逻辑处理用户名、密码；**
	如果你在闪存上有一个凭证文件，但又希望在该闪存被拔出的情况下使用内存缓存来保存用户名和密码，**.gitconfig配置文件如下**
```shell
[credential]
	helper = store --file /mnt/thumbdrive/.git-credentials
	helper = cache --timeout 30000
```

## 4.7 IDEA凭证存储的设置

可以通过Appearance & Behavior - System Setting - Passwords 设置凭证的存储方式。默认为In native Keychain。即存储到本地钥匙串中。因此如果密码发生变化，可以从钥匙串中删除对应的记录，然后重新登录即可。
![[Pasted image 20230805221610.png]]
![[Pasted image 20230805221737.png]]