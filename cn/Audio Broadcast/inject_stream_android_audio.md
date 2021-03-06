
---
title: 输入在线媒体流
description: 
platform: Android
updatedAt: Mon Feb 18 2019 07:32:27 GMT+0000 (UTC)
---
# 输入在线媒体流
## 简介

**输入在线媒体流**功能可以将媒体流作为一个发送端接入正在进行的直播房间。通过将正在播放的音频添加到直播中，主播和观众可以在一起收听媒体流的同时实时互动。

Agora SDK 从 v2.1.0 版本开始，新增 `addInjectStreamUrl` 接口，通过该接口：

- 主播可以指定媒体流输入源，作为音频源输入给直播频道内的所有观众。
- 如果主播开启并设置了旁路直播，输入的媒体流也可以直播给所有旁路观众。

## 常见使用场景

在线流媒体输入主要适用于如下场景：

- 同一直播间内，主播与观众在欣赏音乐的同时，实时讨论或交流想法。

## 注意事项

- 在语音直播中，你只能输入纯音频流，而不能够将视频文件的音频作为在线媒体流输入。
- 频道内同一时间只允许输入一个在线媒体流。
- 只有主播可以输入和移除在线媒体流，连麦主播和普通用户不可以。
- 主播在直播过程中启用输入在线媒体流。观众需要订阅主播才能收听外部音频。
- 支持的媒体流格式包括：RTMP、HLS、FLV。
- 如果媒体流输入成功，该媒体流会出现在频道中，并收到 `onUserJoined` 和 `onFirstRemoteVideoDecoded` 回调，其中 `uid` 为 666。
- 如果媒体流输入失败，会返回错误码。可能会出现的错误码及处理方法如下：

  - `ERR_INVALID_ARGUMENT(2)`：输入的 URL 为空。请重新调用该方法，并确认输入的媒体流的 URL 是有效的
  - `ERR_NOT_INITIALIZED(7)`：引擎没有初始化。请确认调用该方法前已创建 `RtcEngine` 对象并完成初始化
  - `ERR_NOT_SUPPORTED(4)`：频道非直播模式。请调用 `setChannelProfile` 并将频道设置为直播模式再调用该方法
  - `ERR_NOT_READY(3)`：没有加入频道。请确认 App 在频道内

## 实现方法

实现在线媒体流输入首先需要用户以主播身份加入一个直播频道。如果你对如何初始化引擎对象和加入直播频道不了解，请参考 [快速开始](https://docs.agora.io/cn/Audio%20Broadcast/android_video?platform=Android)。

- 输入在线媒体流：

	直播频道的主播可以使用 `addInjectStreamUrl` ，指定一个在线媒体流作为连麦端接入房间。

	```java
	// java
	String urlPath = "Some online RTMP/HLS url path"

	LiveInjectStreamConfig config = new LiveInjectStreamConfig();
	config.width = 0;
	config.height = 0;
	config.videoGop = 0;
	config.videoFramerate = 0;
	config.videoBitrate = 0;
	config.audioSampleRate = LiveInjectStreamConfig.AudioSampleRateType.TYPE_44100;        config.audioBitrate = 48;
	config.audioChannels = 1;

	rtcEngine.addInjectStreamUrl(urlPath, config);
	```

	你可以通过修改 `config` 的参数值控制接入媒体流的音频采样率、音频码率和音频频道数。详见 [AgoraLiveInjectStreamConfig 参数说明](https://docs.agora.io/cn/Audio%20Broadcast/.API%20Reference/java/classio_1_1agora_1_1rtc_1_1live_1_1_live_inject_stream_config.html) 。
	
- 移除在线媒体流

	频道内的主播可以使用 `removeInjectStreamUrl` 接口，移除一个已经接入的在线媒体流。
	
	```java
	// java
	String urlPath = "The same online RTMP/HLS url path added before"
	rtcEngine.removeInjectStreamUrl(urlPath)
	```

	> 主播退出频道后，无需再调用 `removeInjectStreamUrl` 接口。


## 工作原理
- 频道中的主播通过 Video Inject 服务器，将在线媒体流拉取到 Agora SD-RTN 上，推送到直播频道内，频道内的连麦主播、普通观众都可以接收到对应的媒体流。
- 如果主播开启了 CDN 推流，对应的媒体流也会被推送到 CDN 上，CDN 观众就也可以听到这路媒体流。

