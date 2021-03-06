
---
title: 发版说明
description: 
platform: Linux
updatedAt: Mon Feb 18 2019 09:19:03 GMT+0000 (UTC)
---
# 发版说明
## 概览

Agora Signaling SDK 用于构建实时通信场景，可实现呼叫、消息传递、状态同步等功能。点击 [信令产品概述](https://docs.agora.io/cn/Signaling/product_signaling?platform=All%20Platforms) 了解关键特性。

### 已知问题和局限性

-   一个频道目前最多可容纳一万人同时在线。声网建议用户在使用千人以上大频道时关闭用户上下线通知以减小 SDK 压力和带宽消耗。详见 `channelSetAttr` 方法。

-   频道消息：

    -   每条消息最大为 8k 可见字符。

    -   每个用户不超过 60 条/秒，整个频道不能超过 200 条消息/秒，超过频率限制的消息将丢失。

    -   暂不支持离线消息。

    -   仅支持 UTF-8 字符编码。

-   点对点消息：

    -   每条消息最大为 8k 可见字符。

    -   暂不支持离线消息。

    -   仅支持 UTF-8 字符编码。

-   频道属性回调：

    -   每条频道属性回调不超过 10 条/s。



## 1.4.0 BETA 版

本版本发布于 2018 年 8 月 24 日。

修复了一个由 DNS 导致的系统崩溃问题。

## 1.3.3 版 

本版本发布于 2018 年 7 月 11 日。

修复了一个由 DNS 导致的系统崩溃问题。

## 1.3.0 BETA 版

本版本发布于 2018 年 5 月 4 日。

-   `channelInviteUser2` 的 `extra` 新增特殊属性` _timeout` 。用于控制呼叫超时失败的时间，允许设置值为大于等于 1s。


## 1.2.1 版 

本版本发布于 2018 年 3 月 20 日。

修复并优化了以下问题：

-   内存泄漏

-   回调触发


## 1.2.0 版

本版本发布于 2018 年 3 月 7 日，为首次发布。


