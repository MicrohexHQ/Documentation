
---
title: 推流到 CDN
description: 
platform: Windows
updatedAt: Mon Jun 10 2019 06:36:25 GMT+0800 (CST)
---
# 推流到 CDN
## 功能描述

将直播流发布到 CDN（Content Delivery Network）的过程称为 CDN 直播推流，用户无需安装 App，可以通过 Web 浏览器观看直播。

在推流到 CDN 过程中，当频道中有多个主播时，通常会涉及到[转码](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#转码)，将多个直播流组合成单个流，并设置这个流的音视频属性和合图布局。



推流实现原理如下：

<img alt="../_images/live_ios_publishing_stream_cn.png" src="https://web-cdn.agora.io/docs-files/cn/live_ios_publishing_stream_cn.png"/>

## 前提条件

请确保在使用该功能前已联系 [sales@agora.io](mailto:sales@agora.io) 开通旁路推流功能。

## 实现方法

声网提供的 CDN 旁路推流方案主要基于以下 API 进行推流、转码设置：

-   [`setLiveTranscoding`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a0601e4671357dc1ec942cccc5a6a1dde)：设置直播转码参数
-   [`addPublishStreamUrl`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5d62a13bd8391af83fb4ce123450f839)：添加推流地址
-   [`removePublishStreamUrl`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a30e6c64cb616fbd78bedd8c516c320e7)：删除推流地址

其中：

- 主播在 `joinChannel` 成功后通过 `setLiveTranscoding` 接口定义转码参数和设置（RTMP 画布大小等信息）。CDN 音频推流时仍需设置一个 16 &times; 16 的最小视窗。
- 主播通过 `addPublishStreamUrl`接口指定推流地址。推流地址可以在开始推流后动态增删。

### 示例代码：

```C++
// CDN 转码设置
LiveTranscoding config;
config.audioSampleRate = TYPE_44100;
config.audioChannels = 2;
config.audioBitrate = 48;
config.width = 16;
config.height = 16;
config.videoFramerate = 15;
config.videoCodecProfile = HIGH;

// 分配用户视窗的合图布局
config.transcodingUser = new TranscodingUser[config.userCount];
config.transcodingUser[0].uid = 123456; //uid 应和 joinChannel() 输入的 uid 保持一致
config.transcodingUser[0].x = 0;
config.transcodingUser[0].audioChannel = 0;
config.transcodingUser[0].y = 0;
config.transcodingUser[0].width = 16;
config.transcodingUser[0].height = 16;

m_rtcEngine->setLiveTranscoding(config);
```

```C++
// 添加推流地址
// transcodingEnabled 设置为 true，表示开启转码。如开启，则必须通过 setLiveTranscoding 接口配置 LiveTranscoding 类。单主播模式下，我们不建议使用转码。
m_rtcEngine->addPublishStreamUrl(url, true);

 if (config.transcodingUsers)
   {
      delete[] config.transcodingUsers;
   }
```


```C++
// 删除推流地址
m_rtcEngine->removePublishStreamUrl(url);
```

## 开发注意事项

使用该功能需要联系 sales@agora.io 开通旁路推流。


