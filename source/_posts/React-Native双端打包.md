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

和打包 `React Native Android` 应用不同的是，我们无法通过命令一步进行导出 `React Native iOS` 应用。我们需要将`JS`部分的代码和图片资源等打包导出，然后通过`XCode`将其添加到`iOS`
项目中。

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

# 安卓友盟统计+多渠道打包

> 参考：https://blog.csdn.net/k571039838k/article/details/82625295
> 搬运：https://juejin.cn/post/6844904013171785741

友盟集成配置自己看文档，集成的问题应该都不大，毕竟当时没有任何移动端开发经验的我都集成成功了

要多渠道打包主要是修改几个文件

## `android/app/build.gradle`

```gradle
android {
    defaultConfig {
        ...
        +++
        manifestPlaceholders=[UMENG_CHANNEL_VALUE: "Umeng"]
        flavorDimensions "versionCode"
    }
    ...
    // 配置多渠道包支持
    productFlavors{
      Tencent {//投放应用宝市场
              }
      Baidu {//投放百度市场
      }
      Wandoujia {//投放豌豆荚市场
      }
      Vivo {//投放vivo市场
      }
      Oppo {//投放oppo市场
      }
      Xiaomi {//投放小米市场
      }
      Meizu {//投放魅族市场
      }
      Huawei {//投放华为应用市场
      }
      Lenovo {//投放联想市场
      }
      Letv {//投放乐视市场
      }
      Gionee {//投放金立市场
      }
      HiMarket {//投放安卓市场
      }
    }
    productFlavors.all { flavor ->
        flavor.manifestPlaceholders = [UMENG_CHANNEL_VALUE: name]
    }
 
}

```

## `android/app/src/main/AndroidManifest.xml`

```xml

<application>
    ...
    +++
    <!--友盟-->
    <meta-data android:value="友盟的appkey" android:name="UMENG_APPKEY"/>
    <meta-data android:name="UMENG_CHANNEL" android:value="${UMENG_CHANNEL_VALUE}"/>
</application>
```

其他地方我就当你已经自己完美集成了友盟，因为其他地方跟普通集成友盟没有任何不同，不同的只有上面的地方。

## 添加友盟多渠道后使用`react-native run-android` 报错

```bash
react-native run-android --variant channelNameDebug

//channelName是渠道的名字，例如Oppo的是
react-native run-android --variant OppoDebug 

//百度的是
react-native run-android --variant BaiduDebug 
```

我一般直接写在 `package.json` 文件

```json
{
  "scripts": {
    "android": "react-native run-android --variant OppoDebug",
    "ios": "react-native run-ios",
    "start": "react-native start",
    "test": "jest",
    "lint": "eslint . --ext .js,.jsx,.ts,.tsx",
    "bundle-ios": "react-native bundle --platform ios --entry-file index.js --bundle-output ./bundles/main.jsbundle --assets-dest  ./bundles --dev false"
  }
}
```

然后跟以前一样`yarn android`

现在正常运行安卓打包命令，已经可以进行多渠道打包了

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-01-15_18-11-14.png)

输出的文件名是我自定义的，跟多渠道打包无关哈～

测试两个渠道，正常！

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-01-15_18-12-32.png)

# 360多渠道加固打包

之前只想到多渠道打包，没想到加固，等下想回来的时候，我不可能把打包好的十几个apk一个一个再拿去加固吧？

不用代码，直接用360加固客户端，点几下搞定，速度还行。

先看图。

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-03-18_17-45-58.png)

已经把渠道都打包好，而且还加固了，还签名了！

## 操作

先设置好你的渠道，我举个例子。

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-03-18_17-52-20.png)

点击导入后，他会自动跳转到多渠道模板的，一个txt双击导入。

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-03-18_17-55-04.png)

要修改也可以直接在模版上修改，更加的效率。

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-03-18_17-53-37.png)

图中两个地方一定要勾选，不然就是白搞。

接下来就直接上传打包好的apk，等待就行，他会自动给你分渠道和加固。那么360加固究竟干了啥子东西？

## 他做了啥

首先我们清楚看到有个`UMENG_CHANNEL` 在，这个明显就是友盟统计啊大哥，360加固会自动在`AndroidManifest.xml`上加上这么一段代码

```xml

<meta-data
        android:name="UMENG_CHANNEL"
        android:value="qihu360"/>
```

很熟悉，这就是友盟统计的渠道。

此处注意，360是会在最后加上这么一段代码，不会覆盖的，所以之前有自己写`UMENG_CHANNEL`的，记得去掉。

我们用as打开一个apk看下究竟是不是这样。

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-03-18_18-01-20.png)

事实上是的。

接下来我测试一下。

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-03-18_18-32-02.png)

装一个应用宝的看下。

我等了一个小时没看到统计结果，需要改一下。

```java
/**
     * 获取渠道名
     *
     * @param ctx 此处习惯性的设置为activity，实际上context就可以
     * @return 如果没有获取成功，那么返回值为空
     */
    public static String getUMChannelName(Context ctx) {
        if (ctx == null) {
            return null;
        }
        String channelName = null;
        try {
            PackageManager packageManager = ctx.getPackageManager();
            if (packageManager != null) {
                //注意此处为ApplicationInfo 而不是 ActivityInfo,因为友盟设置的meta-data是在application标签中，而不是某activity标签中，所以用ApplicationInfo
                ApplicationInfo applicationInfo = packageManager.getApplicationInfo(ctx.getPackageName(), PackageManager.GET_META_DATA);
                if (applicationInfo != null) {
                    if (applicationInfo.metaData != null) {
                        channelName = applicationInfo.metaData.getString("UMENG_CHANNEL");
                    }
                }
            }
        } catch (PackageManager.NameNotFoundException e) {
            e.printStackTrace();
        }
        return channelName;
    }

```

然后初始化的时候也要改。

```java
RNUMConfigure.init(this, "这是填你自己的app key", AppUtils.getUMChannelName(this), UMConfigure.DEVICE_TYPE_PHONE, null);
```

然后再来一波。

还是没看到，我直接写死也没看到有数据。gg

经过一番谷歌，我发现是他只会统计一台设备第一次安装时候的渠道，所以后面无论我怎么装都是没用的，

我只好找了一台新设备，我后面装了360的，终于看到结果了。

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-03-18_22-00-23.png)
