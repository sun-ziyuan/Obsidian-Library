


# 2. 安装git
```shell
# 安装git
brew install git
# 配置git

# 配置用户名，邮箱
git config --global user.name '用户名'
git config --global user.email '邮箱'
```



### 代理

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


