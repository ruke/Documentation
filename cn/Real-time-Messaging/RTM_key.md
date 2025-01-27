
---
title: 校验用户权限
description: 
platform: All Platforms
updatedAt: Thu May 23 2019 07:10:38 GMT+0800 (CST)
---
# 校验用户权限
## 简介

Agora RTM SDK 提供两种鉴权机制：App ID 和 Token 。这两种鉴权机制的关系以及适用场景详见下图：

![](https://web-cdn.agora.io/docs-files/1555490792498)

其中：

- App ID 易于获取，适用于对安全要求不高的场景，例如测试环境。

- RTM Token 适用于对安全要求较高的生产环境。
- App Certificate 仅用于生成 RTM Token，不可单独使用。一旦启用了 App Certificate，则必须使用 RTM Token。

<a name = "Get-an-App-ID"></a>

## 获取和使用 App ID

每个 Agora 账号可以创建多个项目，而每个项目都有一个唯一标识，即 App ID。一旦有人非法获取了你的 App ID，他就可以在 Agora 提供的 SDK 中使用你的 App ID，因此，请务必保管好你的 App ID。

获取 App ID 的步骤如下：

1.  进入 [Agora 注册页面](https://sso.agora.io/cn/login) ，按照屏幕提示创建一个开发者账号。
2.  登录 Dashboard，在首页点击**创建**按钮，填写**项目名称**后创建一个新项目。
3.  项目创建完成后，Dashboard 会自动跳转至**项目管理**页面，在这里，你可以查看该项目对应的 App ID。

你需要在初始化客户端时，需要填写 `appId` 参数。

> 更换 App ID，需要先调用 `release` 方法销毁当前实例。

## 获取和使用 RTM Token

登录 Agora RTM 系统时，你需要传入 RTM Token 参数。获取和使用 RTM Token 的步骤如下：

### 获取 App ID

详见上文 [获取 App ID](#Get-an-App-ID)。

### 获取 App Certificate

获取 App Certificate 的步骤如下：

1.  登录 [Agora Dashboard](https://dashboard.agora.io)。
2.  先点击左侧导航栏**项目管理**按钮进入**项目管理**页面，再点击目标项目下方**编辑**按钮，进入**项目编辑**页面。
3.  点击 App Certificate 右方的**启用**按钮。仔细阅读关于 App Certificate 介绍后，根据屏幕提示，确认启用 App Certificate。
4.  点击 App Certificate 右方的 ![](https://web-cdn.agora.io/docs-files/1551773294761) 图标，显示完整的 App Certificate。如需隐藏 App Certificate，再次点击 ![](https://web-cdn.agora.io/docs-files/1551773306258) 图标。

> -   App Certificate 仅用于生成 RTM Token，不可单独使用。
> -   将你的 App Certificate 保存在服务器端，且对任何客户端均不可见。
> -   通常 App Certificate 在启用一小时后生效。
> -   当项目的 App Certificate 被启用后，你必须使用 Token 作为鉴权方式。
> -   **信令 Token 调试开关**暂不影响 RTM 项目，无需设置。

### 部署 RTM Token Generator 

Agora 的 Token 方案基于请求—响应机制，流程如下：

1. 在 Server 端部署一个 Token Generator。
2. Client 端需要 Token 相关服务时，向 Server 端发送获取 Token 的请求。
3. Server 端收到请求后 Token Generator 生成一个 Token，然后将生成的 Token 发送给 Client 端。

因此，在使用 Token 之前，你需要先在你的 Server 端部署一个 Token Generator 用来生成 Token。Agora 提供以下平台 Token Generator 的示例代码。

-   [RTM Token Builder for C++](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/cpp/sample/rtm_builder.cpp)
-   [RTM Token Builder for Java](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/java/sample/io/agora/media/sample/RtmTokenBuilderSample.java)
-   [RTM Token Builder for Python](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/python/sample/sample_rtm_builder.py)
-   [RTM Token Builder for PHP](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/php/sample/RtmTokenBuilderSample.php )

Agora 提供的示例代码包含下列内容：

-    生成 RTM Token
-    设置 RTM Client 角色


### 发送获取 RTM Token 的请求

当你需要 RTM Token 时，向 Server 端发送获取 Token 的请求。

生成 RTM Token时，需要传入下列参数：

- `appID`：项目的 App ID，详见 <a href="#getting-an-app-id">获取 App ID</a>。
- `userId`：申请登录 Agora RTM 系统的用户 ID。

> 将你的 App Certificate 保存在服务器端，且对任何客户端均不可见。

设置 RTM Client 角色时，需要传入下列参数：

- `privilege` ：RTM Client 暂时只支持一种角色，将该值设为 1000。
- `expireTimeStamp`：该功能目前仍在开发中，将该值设为 0。

Server 端收到请求后 Token Generator 会生成一个 RTM Token，然后将生成的 RTM Token 发送给 Client 端。

> 出于安全考虑，请在 Token 生成后 24 小时内登录 Agora RTM 系统。超过 24 小时则需要重新生成 Token。
