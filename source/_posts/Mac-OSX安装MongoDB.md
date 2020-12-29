---
title: Mac OSX安装MongoDB
excerpt: 直接看官网就差不多了
date: 2020-12-29 21:12:51
tags: MongoDB
categories: 后台
---


# 安装

[官网教程](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

通过`Homebrew`安装，方便快捷省事。

按照官网教程装完后：

- 配置文件 (`/usr/local/etc/mongod.conf`)

- 日志目录路径 (`/usr/local/var/log/mongodb`)

- 数据目录路径 (`/usr/local/var/mongodb`) 数据库文件都存在这里

此时运行  `mongod -version`  会打印出安装版本证明安装成功

![](https://picture-transmission.iplus-studio.top/Snipaste_2020-12-29_21-17-55.png)

# 启动 MongoDB 服务

- 使用 `brew` 将 `MongoDB` 作为 `macOS` 服务运行

```bash
# 启动
brew services start mongodb-community@4.4
# 停止
brew services stop mongodb-community@4.4
```

- 作为后台启动

`mongod --config /usr/local/etc/mongod.conf --fork`

- 可以通过下面命令查看正在运行的 `MongoDB` 服务 

`brew services list`

![](https://picture-transmission.iplus-studio.top/Snipaste_2020-12-29_21-25-33.png)

- 如果是后台启动的

`ps aux | grep -v grep | grep mongod`

![](https://picture-transmission.iplus-studio.top/Snipaste_2020-12-29_21-26-36.png)

# 连接并使用 MongoDB

启动了`MongoDB`后，就可以连接使用了。

运行  `mongo`  连接服务，可以看到成功连接 `mongo` 服务。

![](https://picture-transmission.iplus-studio.top/Snipaste_2020-12-29_21-22-35.png)
