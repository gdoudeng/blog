---
title: React Native双端打包 
excerpt: RN打包 
date: 2021-01-10 19:26:31
tags: React Native 
categories: 移动端
---

# Android

首先，我觉得官网就说的很详细很好，我贴一下网址，

[中文的](https://reactnative.cn/docs/signed-apk-android)

[英文的](https://reactnative.dev/docs/signed-apk-android)

官网说的非常好，我不重复了，我贴一下常用代码

- 生成app密钥

```bash
keytool -genkeypair -v -keystore my-upload-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```

生产的 `key` 放到 `app` 目录下

## 生成发行 APK 包

只需在终端中运行以下命令：

```bash
$ cd android
$ ./gradlew assembleRelease
```

或者

```bash
$ cd android
$ ./gradlew bundleRelease
```

在0.60之前的版本都是用第一种命令的，到0.60之后就有了第二种打包命令，但是我发现继续用第一种也没啥问题。

# iOS

这个官网也有提及，但是我觉得作用不大

[传送门](https://reactnative.cn/docs/publishing-to-app-store)

## 1. 导出 `js bundle` 包和图片资源

和打包 `React Native Android` 应用不同的是，我们无法通过命令一步进行导出 `React Native iOS` 应用。我们需要将`JS`部分的代码和图片资源等打包导出，然后通过`XCode`将其添加到`iOS`项目中。

- 导出`js bundle`的命令

```
react-native bundle --platform ios --entry-file index.js --bundle-output ./bundles/main.jsbundle --assets-dest  ./bundles --dev false
```

一般来说，不会每次都运行这么长都命令，所以我们都是将其添加到package.json文件中，

```json
{
"scripts": {
    "android": "react-native run-android",
    "ios": "react-native run-ios",
    "start": "react-native start",
    "test": "jest",
    "bundle-ios": "react-native bundle --platform ios --entry-file index.js --bundle-output ./bundles/main.jsbundle --assets-dest  ./bundles --dev false",
    "lint": "eslint ."
  }
}
```

运行命令之前先要在项目根目录建一个文件夹 `bundles`

然后就可以在项目根目录下运行 `yarn bundle-ios`

## 2. 将 `js bundle` 包和图片资源导入到`iOS`项目中

这一步我们需要用到`XCode`，选择`assets`文件夹与`main.jsbundle`文件将其拖拽到`XCode`的项目导航面板中即可。

在`Xcode`中添加资源到项目中，必须使用`Create folder references`的方式(也就是文件夹的方式)添加`bundle`文件夹。

必须使用`Create folder references`的方式添加！

![](https://picture-transmission.iplus-studio.top/714.png)

- 添加成功后`bundle`文件夹为蓝色（如下图）

![](https://picture-transmission.iplus-studio.top/imag52e.png)

## 3. 修改`AppDelegate.m`文件

修改`AppDelegate.m`文件，添加如下代码：

```c
- (NSURL *)sourceURLForBridge:(RCTBridge *)bridge
{
#if DEBUG  
   return [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index" fallbackResource:nil];
#else   
   // 没有使用CodePush热更新的情况
   return [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
   // 如果在项目中使用了CodePush热更新，那么我们需要就可以直接通过CodePush来读取本地的jsbundle，方法如下： For React Native >=0.59,https://github.com/microsoft/react-native-code-push/blob/master/docs/setup-ios.md
   // return [CodePush bundleURL];
#endif 
}
```

上述代码的作用是让`React Native`去使用我们刚才导入的`jsbundle`，这样以来我们就摆脱了对本地`nodejs`服务器的依赖。

到目前为止呢，我们已经将`js bundle`包和图片资源导入到`iOS`项目中，接下来我们就可以发布我们的`iOS`应用了。

`RN` 应用和纯`iOS`应用打包唯一不同的是上面两步，按照这个教程执行完第二步，剩下的步骤就和`iOS`正常`APP`打包一样了

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-01-10_19-50-48.png)
