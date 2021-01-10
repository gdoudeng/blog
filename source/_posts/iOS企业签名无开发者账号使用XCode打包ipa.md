---
title: iOS企业签名无开发者账号使用XCode打包ipa
date: 2021-01-10 19:00:08
excerpt: 我没有开发者账号，怎么打包呢？
tags: iOS
categories: 移动端
---

# 场景

一个外包项目，对过来，对方说不上架，没有开发者账号，使用购买企业签名的方式来进行分发，咋搞呢？

一般来说没有开发者账号确实没法打包，曲线救国，我们可以通过Payload方式进行打包。

# 通过Payload方式

## 1. 和Xcode自带打包方式一样，设置好相关证书和编辑Edit Scheme。

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-01-10_19-08-03.png)

## 2. 在edit scheme中设置run的模式为 `release`

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-01-10_19-12-35.png)

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-01-10_19-12-54.png)

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-01-10_19-14-52.png)

## 3. `command+B` 编译一下工程，等待编译完成 

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-01-10_19-16-41.png)

## 4. 展开工程`Product`目录，右键`show in finder`，可以看到.app扩展名文件

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-01-10_19-18-06.png)

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-01-10_19-19-50.png)

## 5. 在桌面创建文件夹`Payload`，名称一定不要打错，然后将刚刚那个.app文件拷贝到该文件中，鼠标右键，选择压缩文件夹，压缩成功后，将.zip扩展名改为.ipa。到此，ipa包已经成功生成。

![](https://picture-transmission.iplus-studio.top/image.png)

## 6. 和Xcode自带打包方式生成ipa包一样，将ipa上产到不同平台进行分发。

![](https://picture-transmission.iplus-studio.top/image2.png)
