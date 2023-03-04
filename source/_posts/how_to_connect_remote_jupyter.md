---
title: 如何远程连接jupyter notebook
categories: tools
---

# 如何远程连接jupyter notebook

<!-- more -->

## 步骤

1. 首先，保证远程的服务器和自己的本机上都安装有jupyter notebook。
2. 在远程的服务器上，在终端上运行如下命令：

```
jupyter notebook --no-browser --port=8889
# --port 指定一个自己喜欢的端口号
```

3. 在自己的本机打开一个终端，并运行如下命令：

```
ssh -N -f -L localhost:8888:localhost:8889 username@your_remote_host_name -p 22

# 第一个localhost:后面跟的是本机打开的端口，第二个localhost:后面跟的是远程服务器上打开的端口；
# -p参数后面是服务器开放给ssh连接的端口号，若不指定的话，默认ssh端口号是22
# 请修改username并确保那是自己真实的用户名，并且将your_remote_host_name修改为远程服务器的ip地址
```

4. 运行上述命令后，可能会要求输入`ssh`连接服务器的密码，正常输入即可。
5. 在本机上打开任意一个浏览器，键入`localhost:8888`（须与`ssh`命令中第一个`localhost`指定的端口对应）访问即可。

## 可能遇到的问题及我的解决方法

1. 在本机打开之后，界面上要求输入验证，但是输入token之后仍然要求输入token，并且不停地循环。

解决方案：运行jupyter notebook的时候设置无须密码登录：
```
jupyter notebook --no-browser --port=8889 --NotebookApp.allow_password_change=False
```
