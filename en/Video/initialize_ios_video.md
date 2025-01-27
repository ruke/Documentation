
---
title: Create and Initialize an AgoraRtcEngine Instance
description: 
platform: iOS
updatedAt: Thu Dec 13 2018 22:40:20 GMT+0800 (CST)
---
# Create and Initialize an AgoraRtcEngine Instance
Before creating an AgoraRtcEngine instance, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Video/ios_video.md).

## Implementation

Create an AgoraRtcEngine instance by invoking `sharedEngineWithAppId` before joining a live broadcast channel.

In this method:

- Pass the Agora App ID. Only apps with the same App ID can join the same channel.
- Specify a delegate object. The Agora SDK uses the delegate object to inform the app of Agora engine runtime events, such as joining or leaving a channel and adding new users into the channel.

```objective-c
//Objective-C
#import <AgoraRtcEngineKit/AgoraRtcEngineKit.h>

...

- (void)initializeAgoraEngine {
  self.agoraKit = [AgoraRtcEngineKit sharedEngineWithAppId:@"Your App ID" delegate:self];
}
```

```swift
//Swift
import AgoraRtcEngineKit

...

func initializeAgoraEngine() {
   agoraKit = AgoraRtcEngineKit.sharedEngine(withAppId: "Your App ID", delegate: self)
}
```

> Ensure that you call the `sharedEngineWithAppId` method to intiialize the AgoraRtcEngine before calling any other API. 

## Next Steps
You have created the AgoraRtcEngine instance and can start a video call with the following steps:
* [Join a Channel](../../cn/Video/join_video_ios.md)
* [Publish and Subscribe to Streams](../../cn/Video/publish_ios.md)

To check the network connection or audio quality before joining a channel, you can refer to the following sections:
* [Conduct a Last Mile Test](../../cn/Video/lastmile_ios.md)
* [Set the Stereo/High-fidelity Audio Profile](../../cn/Video/audio_profile_ios.md)
