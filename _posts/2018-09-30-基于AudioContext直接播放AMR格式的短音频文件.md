---
layout:     post
title:      基于AudioContext直接播放AMR格式的短音频文件
subtitle:   关于音频原生API的使用
date:       2018-09-30
author:     CNJ
header-img: img/post-bg-audio-web.jpg
catalog: true
tags:
    - WEB
    - 开发技巧
---

# 前言

随着微信的兴起，语音聊天是一种与文字交流不同的交流方式。很多互联网产品，例如金融类APP、游戏类APP等除了会加入传统的文字聊天模块，有些APP也会加入语音聊天模块。

# 业务背景
一般语音的数据量比较大，通常会用amr格式作为存储格式，移动端APP可以调用底层模块进行读取amr格式的文件，但是如果开发的是WEB应用，就比较麻烦了，因为如果直接使用`<audio>`标签的src属性读取服务器文件，没办法识别amr格式文件。因此，引入了一个Web API [AudioContext](https://developer.mozilla.org/zh-CN/docs/Web/API/AudioContext/AudioContext) 。

# AudioContext
***写在最前***： AudioContext是一个**实验中**的新标准，换句话说，现在这篇博客的内容可能由于标准版本修订而有所改变，如果未来浏览器版本出现兼容性问题，需自行解决。

AduioContext是一个专门用于音频处理的接口，原理是将每一个音频节点拼接起来并进行相关处理。除了能够播放amr文件外，AudioContext还可能通过JavaScript代码进行音频的生成和剪辑，具体功能就需要读者自己去探寻API的深层次应用了，本文仅提供利用该API进行amr文件读取的处理过程。

# 处理过程
针对不同浏览器进行相关处理后实例化*AudioContext*

```
var AudioContext = window.AudioContext || window.webkitAudioContext;

var audioContext = new AudioContext(); //实例化AudioContext对象
```
支持AudioContext的浏览器版本如下表：
<!-- |    浏览器    |   Chrome    | Firefox(Gecko)|    IE  |  Opera
|:-----------:|:-----------:|:---------:|:---------:|:-----------:|
|   支持版本   |    55.0     | 25.0 (25.0) | 不支持 | 15.0(webkit) -->
| 浏览器 | Chrome | Firefox | IE | Opera | Safari
| - | :-: | :-: | :-: | :-: | :-: | 
| 版本号 | 55.0+| 25.0+ | 不支持| 15.0+ | 6.0

**AudioContext**主要是用于处理数据流的API，因此首先需要处理从服务端AJAX请求回来的数据。
首先，定义AJAX请求返回的数据类型为二进制数据流
```
    request.responseType = 'arraybuffer'
```
然后调用createBufferSource() 方法用于创建一个新的*AudioBufferSourceNode*接口, 该接口可以通过*AudioBuffer*对象来播放音频数据。 *AudioBuffer*对象可以通过AudioContext.createBuffer 来创建或者通过 AudioContext.decodeAudioData()成功解码音轨后获取。
```
  source = audioContext.createBufferSource();
    request.onload = function() {
    var audioData = request.response;
    audioContext.decodeAudioData(audioData, function(buffer) {  // 进行二进制数据转换为音轨解码，并且根据解码情况调用成功或失败的回调函数
        source.buffer = buffer;
        source.connect(audioContext.destination); // 设置AudioBufferSourceNode为循环
        source.loop = true;
      },
      function(e){"Error with decoding audio data" + e.err});

  }
  或者可以写成Promise写法
  audioContext.decodeAudioData(audioData).then((buffer) => {
        source.buffer = buffer;
        source.connect(audioContext.destination); 
        source.loop = true;
  }).catch(err => { console.log(err)})
```
最后，通过调用source内置的方法就可以启动或停止音频的播放。
```JavaScript
    source.start() // 启动
    source.stop() // 停止
```