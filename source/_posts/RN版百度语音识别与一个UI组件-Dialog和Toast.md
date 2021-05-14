---
title: RN版百度语音识别与一个UI组件
date: 2021-03-06 18:03:49
tags: 前端
excerpt: RN版百度语音识别与一个UI组件 Dialog 和 Toast
---

# 预告

好久没跟新过，因为根本没时间了，目前在赶一个项目

本次项目完成后将开源两个组件库，一个是`RN`版百度语音识别，另一个是`UI`组件，

`Dialog`和`Toast`，Toast其实就是基于`react-native-root-toast`的，看到这个库使用api的形式展现toast，非常好用，

然后搞了一个对话框也是基于api形式进行展现。

# Dialog

- 基于api形式进行展现。

预告图

![](https://picture-transmission.iplus-studio.top/Snipaste_2021-03-06_18-19-04.png)

```typescript
export interface DialogOptions {
  title?: string,
  description?: string,
  okText?: string,
  cancelText?: string,
  onOKPress?: () => void;
  onCancelPress?: () => void;
  onDismiss?: () => void;
  destroyDialog?: () => void;
  contentView?: React.ElementType;
  okTextColor?: string;
  cancelTextColor?: string;
}
```

使用：
```typescript
Tips.showDialog(TipsType.confirm, {
      onCancelPress: this.handleCancelSavePress,
      onOKPress: this.handleSavePress,
      description: "是否修改保存？",
      okText: "保存",
      cancelText: "不保存"
    });
```

基本上自定义啥`props`都行，后面可以改。

其实也是基于其他开源进行改造的，我发现我改完后会与`react-native-root-toast`冲突，所以干脆合在一起了。

# RN版百度语音识别

目前看到这两个库

- [react-native-speech-iflytek-sino](https://github.com/sinooceanland/react-native-speech-iflytek-sino)

- [react-native-speech-iflytek](https://github.com/zphhhhh/react-native-speech-iflytek)

现在看到这两个库，把这两个库有的功能利用[百度语音识别](https://ai.baidu.com/ai-doc/SPEECH/Ek39uxgre)实现。

# 开源

项目结束后会开源这两个库，可以关注我 [github](https://github.com/gdoudeng) 和 [gitee](https://gitee.com/gdoudeng) ，我也会在这里说。

百度语音已经开源了，另外一个UI库看时间安排上。

# `react-native-baidu-asr`

[react-native-baidu-asr](https://github.com/gdoudeng/react-native-baidu-asr) 是一个 React Native 下的百度语音库，可以进行语音识别以及语音唤醒。
