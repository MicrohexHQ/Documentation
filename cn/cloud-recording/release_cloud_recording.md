
---
title: 云端录制发版说明
description: 
platform: Linux
updatedAt: Thu Jul 25 2019 09:28:23 GMT+0800 (CST)
---
# 云端录制发版说明
## 简介

Agora 云端录制，可为 Agora 实时音视频提供录制服务。同 Agora 本地服务端录制相比，Agora 云端录制无需部署 Linux 服务器，减轻了研发和运维的压力，更轻量便捷。点击[云端录制产品概述](../../cn/cloud-recording/product_cloud_recording.md)了解关键特性。

### 兼容性

Agora 云端录制与以下 Agora SDK 兼容:

| SDK              | 描述                                                         |
| :--------------- | :----------------------------------------------------------- |
| Agora Native SDK | 云端录制与全平台 Agora Native SDK 1.7.0 或更高版本兼容，如果频道内有任何人使用了 1.6 版本的 Agora Native SDK， 则整个频道无法录制。 |
| Agora Web SDK    | 云端录制 与 Agora Web SDK 1.12.0 或更高版本兼容。            |

## 1.2.0 版

该版本于 2019 年 7 月 19 日发布，新增特性、改进及修复问题如下。

### 新增特性

#### 自定义合流布局

RESTful API 新增自定义的合流布局。使用方法详见[自定义合流布局](../../cn/cloud-recording/cloud_layout_guide.md)。

你可以在开始录制时将 `mixedVideoLayout` 设为 3，并在 `layoutConfig` 中设置每个用户的画面大小和位置。你也可以在录制过程中调用 `updateLayout` 方法随时更新合流布局。

#### 自定义背景色

RESTful API 新增 `backgroundColor` 参数，支持自定义合流的画布背景色。无论使用何种合流布局，都可以通过该参数设置画布的背景色。

#### 录制的时间戳

为方便开发者获得精准的录制开始时间，RESTful API 提供录制开始第一片切片的 Unix 时间戳 `sliceStartTime`，可以通过 `query` 方法查询获得。同时在 RESTful API 的回调通知中新增事件 `recorder_slice_start`，提供第一片录制切片开始的时间和上一次录制失败的时间。

### 改进

RESTful API 优化了对 `resourceId` 和 `cname` 以及 `uid` 是否对应的检查。

### 修复问题

修复了默认的合流布局中存在的一些小问题。

## 1.1.1 版

该版本于 2019 年 7 月 2 日发布，主要改进如下。

- 将合流布局默认背景色修改为黑色。
- 改善了在弱网环境下录制的视频卡顿问题。

## 1.1.0 版

该版本于 2019 年 6 月 13 日发布。

该版本主要新增了对 RESTful API 的支持，无需集成 SDK，直接通过网络请求来控制云端录制。

你可以参考下面的文档使用 RESTful API：

- [RESTful API 录制](../../cn/cloud-recording/cloud_recording_rest.md)：使用 RESTful API 进行录制
- [云端录制 RESTful API](../../cn/cloud-recording/cloud_recording_api_rest.md)：RESTful API 参考
- [云端录制 RESTful API 回调服务](../../cn/cloud-recording/cloud_recording_callback_rest.md)：开通回调服务接收云端录制事件通知

##  1.0.0 版

该版本于 2019 年 4 月 29 日发布。本次发版为云端录制的第一次发版，主要包括以下功能:

- Agora Native SDK 和 Agora Web SDK 的高清音视频通话的录制
- 频道内所有用户的音视频合流录制
- 支持 3 种合流布局样式：Float（默认布局）、BestFit（自适应布局）、Vertical（垂直布局）。具体可参考 [TranscodingConfig](../../cn/cloud-recording/cloud_recording_api.md) 表格中的描述。
- 第三方云存储，目前支持七牛云、阿里云和 Amazon S3
- 提供 C++ 和 Java SDK 包
