
---
title: Release Notes
description: 
platform: Windows
updatedAt: Thu Jun 13 2019 02:58:57 GMT+0800 (CST)
---
# Release Notes
This page provides the release notes for the Agora Video SDK.

## Overview

The Video SDK for Windows supports the following scenarios:

-   Voice/Video Communication
-   Live Voice/Video Broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Video Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms), [Audio Broadcast Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms) and [Video Broadcast Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

> The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version earlier than v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).

## v2.4.1

V2.4.1 is released on Jun 12th, 2019.

### Before getting started

Ensure that you read the following SDK behavior changes if you migrate from an earlier SDK version.

#### 1. Publishing streams to the CDN

To improve the usability of the CDN streaming service, v2.4.1 defines the following parameter limits:

| Class / Interface  | Parameter Limit                                              |
| ---------------------- | ------------------------------------------------------------ |
| [LiveTranscoding](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html)        | <li>[videoFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a8086eda68ed5b9d106cf830fd4e24f9c): Frame rate (fps) of the CDN live output video stream. The default value is 15. We recommend not setting it to a value higher than 30.<li>[videoBitrate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#ab5ff17deab2e3149f149987cc0d83433): Bitrate (Kbps) of the CDN live output video stream. The default value is 400. Set this parameter according to the [Video Bitrate Table](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html#af10ca07d888e2f33b34feb431300da69). If you set a bitrate beyond the proper range, the SDK automatically adapts it to a value within the range.<li>[videoCodecProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a72fbfdd2c0d4f0cc438d5ca052aa7531): The video codec profile. Set it as **BASELINE**, **MAIN**, or **HIGH** (default). If you set this parameter to other values, Agora adjusts it to the default value of **HIGH**.<li>[width](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#ad0a505490500917dd246510471ef88be) and [height](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a340ee10a10bf0137fb996ca77729002e): Pixel of the video. The minimum value of width x height is 16 x 16.</li> |
| [RtcImage](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_image.html)             | url: The maximum length of this parameter is **1024** bytes. |
| [addPublishStreamUrl](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a5d62a13bd8391af83fb4ce123450f839)    | `url`: The maximum length of this parameter is **1024** bytes. |
| [removePublishStreamUrl](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a30e6c64cb616fbd78bedd8c516c320e7) | `url`: The maximum length of this parameter is **1024** bytes. |

This release also adds the [audioCodecProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a76987efee0f6d507ef648083507bd935) parameter in the `LiveTranscoding` class to set the audio codec profile type. The default type is LC-AAC, which means the low-complexity audio codec profile.

v2.4.1 also adds five error codes to the `error` parameter in the  [onStreamPublished](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a85cc73e9f53766b8a0a7fa8c96f2ea98) method for quick troubleshooting.

#### 2. Renaming the receivedFrameRate parameter in the RemoteVideoStats class

v2.4.1 renames the `receivedFrameRate` parameter to [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a500d2a8457bf877794c219d194ec09b0) in the  [RemoteVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html) class to more accurately describe the statistics of the remote video stream.

### New features

#### 1. Adding media metadata

In live broadcast scenarios, the host can send shopping links, digital coupons, and online quizzes to the audience for more diversified live broadcast interactions. v2.4.1 adds the [registerMediaMetadataObserver](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a84dbf06de1d769b63200d7ec0289cca0) interface and the [IMediaMetadataObserver](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_metadata_observer.html) class, allowing the host to add metadata to the output video and to send media attached information.

#### 2. Optimized screen sharing

To avoid image cropping and distortion in screen sharing, v2.4.1 optimizes the encoding algorithms. If the aspect ratios are different between the encoding [dimensions](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_screen_capture_parameters.html#a9ee8c96bbfe031e8c614d7b2e49ddbb8) that you set in the [ScreenCaptureParameters](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_screen_capture_parameters.html) class and the original screen dimensions, Agora applies the following encoding algorithms:

Suppose the value of **dimensions** is 1920 x 1080 pixels, that is, 2073600 pixels:

- If the value of the screen `dimensions` is lower than that of the encoding dimensions, for example, 1000 x 1000 pixels, the SDK uses 1000 x 1000 pixels for encoding.
- If the value of the screen `dimensions` is higher than that of the encoding dimensions, for example, 2000 x 1500 pixels, the SDK uses the maximum value under 1920 x 1080 pixels with the aspect ratio of the screen dimension (4:3) for encoding, that is, 1440 x 1080 pixels.

Agora uses the [dimensions](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_screen_capture_parameters.html#a9ee8c96bbfe031e8c614d7b2e49ddbb8) value in the[ScreenCaptureParameters](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_screen_capture_parameters.html) class to calculate the charges. If you do not set the value of **dimensions**, the SDK uses the default value of 1920 x 1080 to calculate the charges.

You can also choose whether or not to capture the mouse cursor when sharing the screen. v2.4.1 adds the [captureMouseCursor](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_screen_capture_parameters.html#aa68157203616423bad85f852f3816c1f) parameter in the `ScreenCaptureParameters` class and captures the mouse by default.

#### 3. State of the local video

v2.4.1 adds the [onLocalVideoStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a5a8bfdc3a7c4ba054f90365ed00781d6) callback to indicate the local video state. In this callback, the SDK returns the `STOPPED`,` CAPTURING`, `ENCODING`, or `FAILED` state. When the state is `FAILED`, you can use the error code for troubleshooting. This callback indicates whether or not the interruption is caused by capturing or encoding. This release deprecates the `onCameraReady` and `onVideoStopped` callbacks.

#### 4. State of the RTMP streaming

v2.4.1 adds the [onRtmpStreamingStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a29754dc9d527cbff57dbc55067e3287d) callback to indicate the state of the RTMP streaming and help you troubleshoot issues when exceptions occur. In this callback, the SDK returns the IDLE, `CONNECTING`, `RUNNING`, `RECOVERING`, or `FAILURE` state. When the state is `FAILURE`, you can use the error code for troubleshooting. You can still use the `onStreamPublished` and `onStreamUnpublished` callbacks, but we do not recommend using them.

#### 5. More reasons for a network connection state change

In the [onConnectionStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4) callback, v2.4.1 adds error codes to the `reason` parameter to help you troubleshoot issues when exceptions occur. The SDK returns the `onConnectionStateChanged` callback whenever the connection state changes. This release also deprecates `WARN_LOOK_UP_CHANNEL_REJECTED(105)`, `ERR_TOKEN_EXPIRED(109)`, and `ERR_INVALID_TOKEN(110)`.

#### 6. State of the local network type 

v2.4.1 adds the [onNetworkTypeChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af71617b59ba8564963dfdd642f4e36a4) callback to indicate the local network type. In this callback, the SDK returns the `UNKNOWN`, `DISCONNECTED`, `LAN`, `WIFI`, `2G`, `3G`, or `4G` type. When the network connection is interrupted, this callback indicates whether or not the interruption is caused by a network type change or poor network conditions.

#### 7. Getting the audio mixing volume

v2.4.1 adds the [getAudioMixingPlayoutVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aed9dda5a7b2683776f41f6ba0e1f281c) and [getAudioMixingPublishVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9fafbaaf39578810ec9c11360fc7f027) methods, which respectively gets the audio mixing volume for local playback and remote playback, to help you troubleshoot audio volume related issues.

#### 8. Reporting when the first remote audio frame is received and decoded

To get the more accurate time of the first audio frame from a specified remote user, v2.4.1 adds the [onFirstRemoteAudioDecoded](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7d375e7467bd099ad2d4c9cfc0a6c242) callback to report to the app that the SDK decodes first remote audio. This callback is triggered in either of the following scenarios:

- The remote user joins the channel and sends the audio stream.
- The remote user stops sending the audio stream and re-sends it after 15 seconds.

The difference between the `onFirstRemoteAudioDecoded` and `onFirstRemoteAudioFrame` callbacks is that the `onFirstRemoteAudioFrame` callback occurs when the SDK receives the first audio packet. It occurs before the `onFirstRemoteAudioDecoded` callback.

#### 9. Miscellaneous

- v2.4.1 supports 64-bit operation systems.

### Improvements

#### 1. Reporting more statistics

- v2.4.1 adds the [txPacketLossRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a52232c2b3af1573b3783f74f7ca75c1c) and [rxPacketLossRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a1882c369c5d1bc04b85fb9dcb15700b6) parameters in the [RtcStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html) class. These parameters return the packet loss rate from the local client to the server and vice versa.

- To provide more accurate statistics of the local and remote video, v2.4.1 makes the following changes to the following classes:
  - [LocalVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html): Adds the [encoderOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#ab5df1607ac79acbc51941797189b8dba) and [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#aa91d7d73f3d2c2933658a9dced6ec3ed) parameters
  - [RemoteVideoStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html): Adds the [decoderOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a21776c26b256d836a90944c1051c9322) parameter, and renames the receivedFrameRate parameter to the [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a500d2a8457bf877794c219d194ec09b0) parameter

#### 2. Miscellaneous

- Improved the sound quality of the GAME_STREAMING audio scenario.
- Reduced the audio and video latency.
- Reduced the SDK package size by 0.5 M.
- Improved the accuracy of the network quality after users change the video bitrate.
- Enabled the audio quality notification callback by default, that is, enabled the [onRemoteAudioStats](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a) callback without calling the `enableAudioQualityIndication` method.
- Improved the stability of video services.
- Improved the stability of CDN streaming services.
- Improved the compatibility of the SDK with video devices.

### Issues fixed

#### Audio

- The app cannot play multiple local audio effect files simultaneously. - windows

#### Video

- The sending client sends the low stream unexpectedly.
- The user cannot switch between the screen sharing stream and the camera stream. 

#### Miscellaneous

- Users still receive the `onNetworkQuality` callback after leaving the channel.
- Occasional crashes. 

### API changes

To improve your experience, we made the following changes to the APIs:

#### Unified the C++ interface for all platforms

v2.4.1 unifies the behavior of the C++ interfaces across different platforms so that you can apply the same code logic on different platforms. v2.4.1 implements the methods of the `RtcEngineParameters` class in the `IRtcEngine` class. Refer to [Agora C++ API Reference for All Platforms home page](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/index.html) for the applicable platforms and considerations of each interface.

#### Added

- [getAudioMixingPlayoutVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aed9dda5a7b2683776f41f6ba0e1f281c)
- [getAudioMixingPublishVolume](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9fafbaaf39578810ec9c11360fc7f027)
- [onFirstRemoteAudioDecoded](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7d375e7467bd099ad2d4c9cfc0a6c242)
- [onLocalVideoStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a5a8bfdc3a7c4ba054f90365ed00781d6)
- [onNetworkTypeChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af71617b59ba8564963dfdd642f4e36a4)
- [onRtmpStreamingStateChanged](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af71617b59ba8564963dfdd642f4e36a4)
- [registerMediaMetadataObserver](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a84dbf06de1d769b63200d7ec0289cca0)
- The [IMetadataObserver](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_metadata_observer.html) class
- The [audioCodecProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_live_transcoding.html#a76987efee0f6d507ef648083507bd935) parameter in the `LiveTranscoding` class
- The [captureMouseCursor](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_screen_capture_parameters.html#aa68157203616423bad85f852f3816c1f) parameter in the `ScreenCaptureParameters` class
- The [txPacketLossRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a52232c2b3af1573b3783f74f7ca75c1c) and [rxPacketLossRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_rtc_stats.html#a1882c369c5d1bc04b85fb9dcb15700b6) parameters in the `RtcStats` class
- The [encoderOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#ab5df1607ac79acbc51941797189b8dba) and [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_local_video_stats.html#aa91d7d73f3d2c2933658a9dced6ec3ed) parameters in the `LocalVideoStats` class
- The [decoderOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a21776c26b256d836a90944c1051c9322) and [rendererOutputFrameRate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_remote_video_stats.html#a500d2a8457bf877794c219d194ec09b0) (to replace `receivedRemoteRate`) parameters in the `RemoteVideoStats` class

#### Deprecated

- `enableAudioQualityIndication`
- `onCameraReady`
- `onVideoStopped`
- The `WARN_LOOKUP_CHANNEL_REJECTED(105)` warning code
- The `ERR_TOKEN_EXPIRED(109)` error code
- The `ERR_INVALID_TOKEN(110)` error code
- The `ERR_START_CAMERA(1003)` error code
- The `ERR_VDM_WIM_DEVICE_IN_USE(1502)` error code

## v2.4.0

v2.4.0 is released on April 1, 2019.

### New Features

#### 1. Advanced screen sharing

v2.4.0 upgrades screen sharing and provides the following advanced functions:

- Shares the whole or part of a specified screen in a multi-display environment ([`startScreenCaptureByScreenRect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#a41893fe9a0ca49c054bf6dbd7d9d68f5)).
- Shares the whole or part of a specified window ([`startScreenCaptureByWindowId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#add5ba807256e8e4469a512be14e10e52)) .
- Sets the content hint of the screen sharing to prioritize motion or detail ([`setScreenCaptureContentHint`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#aff9003c492450dbd8c3f3b9835186c95)).
- Sets the dimensions, frame rate and bitrate for screen sharing ([`updateScreenCaptureParameters`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#ad680e114ba3b8a0012454af6867c7498)).

v2.4.0 deprecates the `startScreenCapture` method. We recommend using the new methods for screen sharing. With the new methods, developers need to design the code logic to obtain the `screenRect` and `windowId`. For more information, see [Share the Screen](../../en/Interactive%20Broadcast/screensharing_windows.md).

#### 2. Voice changer and voice reverberation

Adding voice changer and reverberation effects in an audio chat room brings much more fun. v2.4.0 adds the [`setLocalVoiceChanger`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a70175273dade14bcf8d74e71f8de7eda) and [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_rtc_engine_parameters.html#add46038703f2f82dad83c0319d43433b) methods, allowing you to change your voice or reverberation by choosing from the preset options. See [Adjust the pitch and tone](../../en/Interactive%20Broadcast/voice_effect_windows.md).

#### 3. Tracking the sound position of a remote user 

v2.4.0 adds the [`enableSoundPositionIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af91af072cfca4ac04e64907618b0cd2d) and [`setRemoteVoicePosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af85b6be0a185fc67acc6eabe31467d20). Call the [`enableSoundPositionIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af91af072cfca4ac04e64907618b0cd2d) method before joining a channel to enable stereo panning for the remote users, and then you can call the [`setRemoteVoicePosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af85b6be0a185fc67acc6eabe31467d20) method to track the position of a remote user.

#### 4. Pre-call last-mile network probe test

Conducting a last-mile probe test before joining the channel helps the local user to evaluate or predict the uplink network conditions. v2.4.0 adds the [`startLastmileProbeTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#adb3ab7a20afca02f5a5ab6fafe026f2b), [`stopLastmileProbeTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#a94f3494035429684a750e1dee7ef1593), and [`onLastmileProbeResult`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a44134dfda5d412831fa8e44fa533fca5) APIs, allowing you to get the uplink and downlink last-mile network statistics, including the bandwidth, packet loss, jitter, and round-trip time (RTT).

#### 5. Audio device loopback test

v2.4.0 adds the [`startAudioDeviceLoopbackTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac78c08f3212dc3efa000e197207dec53) and [`stopAudioDeviceLoopbackTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_audio_device_manager.html#aad01da1e0bacd3f2fd355483f9e3befb) methods for testing whether the local audio devices are working properly. The test involves only the local audio devices and does not report the network condition.

#### 6. Setting the priority of a remote user's stream

v2.4.0 adds the [`setRemoteUserPriority`](https://docs.agora.io/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#aecb5d85e9b3a60947d569b88253da710) method for setting the priority of a remote user. You can use this method with the [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a50e727c34b662de64c03b0479a7fe8e7) method. If the fallback function is enabled for a remote stream, the SDK ensures the high-priority user gets the best possible stream quality.

#### 7. State of an audio mixing file 

v2.4.0 adds the [`onAudioMixingStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a298389513bfaa50af4277fc3296e3f22) callback to report any change of the audio-mixing file playback state (playback succeeds or fails) and the corresponding reason. This release also adds the warning code 701, which is triggered if the local audio-mixing file does not exist, or if the SDK does not support the file format or cannot access the music file URL when playing the audio-mixing file.

#### 8. Setting the log file size

The SDK has two log files, each with a default size of 512 KB. In case some customers require more than the default size, v2.4.0 adds the [`setLogFileSize`](https://docs.agora.io/en/Interactive%20Broadcast/Video/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a6fb256cb165856a4412bb30b098408a1) method for setting the log file size (KB).

#### 9. Setting the background image for LiveTranscoding

v2.4.0 adds the [`backgroundImage`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/structagora_1_1rtc_1_1_live_transcoding.html#a729037c7cf31b57efd1e8c9fadeab6eb) parameter in the [`LiveTranscoding`](https://docs.agora.io/API%20Reference/cpp/v2.4/structagora_1_1rtc_1_1_live_transcoding.html) Class in Windows to set the background image in the combined video of a live broadcast.

### Improvements

#### 1. Accuracy of call quality statistics

- v2.4.0 adds the intervalInSeconds parameter to the startEchoTest method, allowing you to set the interval between when you speak and when the recording plays back.
- v2.4.0 adds three parameters to the [`LocalVideoStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/structagora_1_1rtc_1_1_local_video_stats.html) class: [`targetBitrate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/structagora_1_1rtc_1_1_local_video_stats.html#a3e8d46c9b67c9fe62487ec56bc587fd6) for setting the target bitrate of the current encoder, [`targetFrameRate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/structagora_1_1rtc_1_1_local_video_stats.html#ac06928c5bf18c5db7eb4661dd783ba0a) for setting the target frame rate, and [`qualityAdaptIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/structagora_1_1rtc_1_1_local_video_stats.html#a902fc5b33956471b7a28f43399fa8b20) for reporting the quality of the local video since last count.

#### 2. Video encoder preferences

v2.4.0 provides the following options for setting video encoder preferences:

- Setting preferences under limited bandwidth. v2.4.0 adds two parameters to the [`VideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/structagora_1_1rtc_1_1_video_encoder_configuration.html) class: minFrameRate and degradationPrefer. You can use these parameters together to set the minimum video encoder frame rate and the video encoding degradation preference under limited bandwidth. For more information, see [Set the Video Profile](../../en/Interactive%20Broadcast/videoProfile_windows.md).
- Setting the camera capture preference. v2.4.0 adds the [`setCameraCapturerConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/(./API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a1fe5e04a5201350a875d28c7fffa59af)) method, allowing you to set the camera capture preference. You can choose system performance over video quality or vice versa as needed. For more information, see the [API Reference](https://docs.agora.io/en/Interactive%20Broadcast/(./API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a1fe5e04a5201350a875d28c7fffa59af)).

#### 3. Core quality improvements

- Reduces the audio delay.
- Improves the video quality and stability.
- Shortens the time to render the first remote video frame. 
- Improves the video smoothness and reduces the time delay when sharing a screen under poor network conditions. 

### Issues Fixed

#### Audio

- Calling the `enableLocalAudio` method disconnects all connected Bluetooth devices.
- The SDK does not support audio mixing URLs with Chinese characters.
- Volume levels of the high-pitch sound are lowered.
- Sounds are occasionally played fast.

#### Video

- If you skip the `renderMode` setting, the video stretches due to a mismatch with the display.
- Video freezes on some lower-end devices.
- It takes too long to render the first received video frame.

#### Miscellaneous

- Occasional crashes.
- The stream for screen sharing is cropped when lowering the screen resolution. We highly recommend using the new methods in v2.4.0 for screen sharing. With the new screen sharing parameters, these methods separate the stream for screen sharing from the camera-captured stream. See [Share the Screen](../../en/Interactive%20Broadcast/screensharing_windows.md) for more information.
- A region cannot be shared if one of its coordinates is negative.
- Occasional echoes.
- The SEI information does not synchronize with the media stream when publishing transcoded streams to the CDN.

### API Changes

To improve your experience, we made the following changes to the APIs:

#### Added

- [`setBeautyEffectOptions`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#a5899cc462e5250028c9afada4df98d48)
- [`startScreenCaptureByScreenRect`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#a41893fe9a0ca49c054bf6dbd7d9d68f5)
- [`startScreenCaptureByWindowId`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#add5ba807256e8e4469a512be14e10e52)
- [`updateScreenCaptureParameters`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#ad680e114ba3b8a0012454af6867c7498)
- [`setScreenCaptureContentHint`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#aff9003c492450dbd8c3f3b9835186c95)
- [`setLocalVoiceChanger`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a70175273dade14bcf8d74e71f8de7eda)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_rtc_engine_parameters.html#add46038703f2f82dad83c0319d43433b)
- [`enableSoundPositionIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af91af072cfca4ac04e64907618b0cd2d)
- [`setRemoteVoicePosition`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_rtc_engine_parameters.html#af85b6be0a185fc67acc6eabe31467d20)
- [`startLastmileProbeTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#adb3ab7a20afca02f5a5ab6fafe026f2b)
- [`stopLastmileProbeTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#a94f3494035429684a750e1dee7ef1593)
- [`setRemoteUserPriority`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#aecb5d85e9b3a60947d569b88253da710)
- [`startEchoTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#a842ed126b6e21a39059adaa4caa6d021)
- [`startAudioDeviceLoopbackTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_audio_device_manager.html#ac78c08f3212dc3efa000e197207dec53)
- [`stopAudioDeviceLoopbackTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_audio_device_manager.html#aad01da1e0bacd3f2fd355483f9e3befb)
- [`setCameraCapturerConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a1fe5e04a5201350a875d28c7fffa59af)
- [`setLogFileSize`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a6fb256cb165856a4412bb30b098408a1)
- [`onAudioMixingStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a298389513bfaa50af4277fc3296e3f22)
- [`onLastmileProbeResult`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a44134dfda5d412831fa8e44fa533fca5) 

#### Deprecated

- `startEchoTest`
- `startScreenCapture`
- `setVideoQualityParameters`

## v2.3.3

v2.3.3 is released on January 24, 2019. 

### Improvements

v2.3.3 optimizes the screen-sharing algorithm for different scenarios. The video smoothness and quality are enhanced when a user presents slides or browses websites. v2.3.3 also improves the initial image quality in the Communication profile.

### Issues Fixed

Occasional inaccurate statistics returned in the `onNetworkQuality` callback.

## v2.3.2

v2.3.2 is released on January 16, 2019. 

### Before Getting Started

Besides the new features and improvements mentioned below, it is worth noting that v2.3.2:

- Improves the SDK's ability to counter packet loss under unreliable network conditions.
- Improves the communication smoothness.
- Reduces video freezes in the Live Broadcast profile.
 
Before upgrading your SDK, ensure that your version is:

- Native SDK v1.11 or later.
- Web SDK v2.1 or later.

If you using an Agora SDK version v2.0.8 and wish to migrate the v2.3.2, refer to the [Migration Guide](../../en/Interactive%20Broadcast/migration_windows.md) for major API changes.

### New Features

#### 1. External video data (Push Mode)

v2.3.2 adds the following methods, allowing you to use the external video during a call or in a live broadcast:

- [`setExternalVideoSource`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#a6716908edc14317f2f6f14ee4b1c01b7): Configures the external video source.
- [`pushVideoFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae064aedfdb6ac63a981ca77a6b315985): Pushes the external video frames.

The application can encapsulate the external video frames in the `AgoraVideoFrame` format and push them to the SDK for encoding and transmitting.

This feature applies to scenarios where a user captures and renders the external video and then pushes the video frames to the SDK for transmission (the application processes the rendering).

#### 2. External audio sink (Pull Mode)

v2.3.2 adds the following methods to improve the user experience in playing the external audio:

- [`setExternalAudioSink`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a08450bffffc578290d4a1317f2938638): Sets the external audio sink.
- [`pullAudioFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#aaf43fc265eb4707bb59f1bf0cbe01940): Pulls the external audio frame.

The application pulls the decoded audio frames (mixed from the remote users) from the media engine for external playback.

The difference between `pullAudioFrame` and [`onPlaybackAudioFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#aefc7f9cb0d1fcbc787775588bc849bac): 

- `pullAudioFrame`: The application pulls the audio frames and:

	- Specifies the number of audio samples for playback.
	- Adjusts the frame buffer. 
	- Allows for a delay in processing the audio frame. 
	
	This method avoids problems caused by jitter in the external audio, such as an unsynchronized audio playback.
	
- `onPlaybackAudioFrame`: The SDK pushes the audio frames to the application once every 10 ms. Any delay in processing the audio frames may result in audio jitter.

#### 3. Video quality in a live broadcast

v2.3.2 adds the [minBitrate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html#ab3249ca88227aaf36f21681b9de7ced2) parameter (minimum encoding bitrate) in the [setVideoEncoderConfiguration](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e) method. The SDK automatically adjusts the encoding bitrate to adapt to the network conditions. Using a value greater than the default value forces the video encoder to output high-quality images but may cause more packet loss and hence sacrifice the smoothness of the video transmission. Agora does not recommend changing this value unless you have special requirements for image quality.

#### 4. Independent audio mixing volume adjustments for local playback and remote publishing

v2.3.2 adds the [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a99ab2878e0c4fbf1be6970a2c545d085) and [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a8f8d2af4b4c7988934e152e3b281d734) methods to complement the [`adjustAudioMixingVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a5e117be71d38d813208198f4064aa964) method, allowing you to independently adjust the audio mixing volume for local playback and remote publishing. See [Adjust the Volume](../../en/Interactive%20Broadcast/volume_windows.md) for the scenarios and corresponding APIs.

#### 5. Fallback options for a live broadcast under unreliable network conditions

Unreliable network conditions affect the overall quality of a live broadcast. v2.3.2 adds the [`setLocalPublishFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a0402734b50749081b20db3826f6f00ec) and [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a50e727c34b662de64c03b0479a7fe8e7) methods to allow the SDK to:

- Automatically disable the video stream when the network conditions cannot support both audio and video, or
- Enable the video when the network conditions improve. 

The SDK triggers the [`onLocalPublishFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ace4279c4d87c23a1fecc3eb8e862a513) or [`onRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7ee343146ad6e3f120bd04a7a6fdda74) callback when the stream falls back to audio-only or switches back to the video.

#### 6. Upstream and Downstream statistics of each remote user/host

v2.3.2 adds the [`onRemoteAudioTransportStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad79bcd56075fa9c9f907bb4a7462352d) and [`onRemoteVideoTransportStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a3b8fd883a31d4a504ac3cbd50b1c5d0f)  callbacks to provide the upstream and downstream statistics of each remote user/host. During a call or live broadcast, the SDK triggers these callbacks once every two seconds after the local user receives audio/video packets from a remote user. The callbacks return the user ID, received audio/video bitrate, packet loss rate, and network time delay (ms).

#### 7. New video encoder configuration

To support video rotation scenarios and improve the quality of the custom video source, v2.3.2 deprecates the [`setVideoProfile`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac8b16d2a4e67bd75231a76e06d2d85eb) method and replaces it with the [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e) method to set the video encoder configurations. The [`VideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html) class provides a set of configurable video parameters, including the dimension, frame rate, bitrate, and orientation. You can still use the `setVideoProfil`e method, but Agora recommends using the `setVideoEncoderConfiguration` method to set the video profile.

#### 8. Virtual sound card

v2.3.2 adds the `deviceName` parameter in the [`enableLoopbackRecording`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a065f485fd23b8c24a593680a47d754aa) method, allowing you to use a virtual sound card for audio recording:

- To use the current sound card, set deviceName as NULL.
- To use a virtual card, set `deviceName` as the name of the virtual card.

### Improvements

#### 1. Improves the accuracy of the call quality statistics

v2.3.2 deprecates the `onAudioQuality` callback and replaces it with the `onRemoteAudioStats` callback to improve the accuracy of the call quality statistics. The `onRemoteAudioStats` callback returns parameters such as the audio frame loss rate, end-to-end audio delay, and jitter buffer delay at the receiver, which are more closely linked to the real user experience. In addition, v2.3.2 optimizes the algorithm of the `onNetworkQuality` callback for the uplink and downlink network qualities.

- [`onRemoteAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a): Reports the statistics of the remote audio stream from each user/host. This callback replaces the onAudioQuality callback. 
- [`onNetworkQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a80003ae8cce02039f3aa0e8ffad7deed): Reports the last mile network quality of each user in the channel.

Agora plans to improve the following callback in subsequent versions:

- [`onLastmileQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ac7e14d1a26eb35ef236a0662d28d2b33): Reports the last mile network quality of the local user before the user joins a channel.

For the list of API methods related to the call quality statistics and on how and when to use them, see [Report In-call Statistics](../../en/Interactive%20Broadcast/in_call_statistics_windows.md).

#### 2. New network connection policy 

v2.3.2 adds the following API method and callback to get the current network connection state and reason for a connection state change:

- [`getConnectionState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a512b149d4dc249c04f9e30bd31767362): Gets the connection state of the SDK.
- [`onConnectionStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4): Occurs when the connection state of the SDK to the server changes.

v2.3.2 deprecates the [`onConnectionInterrupted`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9927b5cd2a67c1f48f17b5ed2303f483) and [`onConnectionBanned`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a38e9d403ae4732dff71110b454149404) callbacks.

In the new API method, the network connection states are "disconnected", "connecting", "connected", "reconnecting", and "failed". The SDK triggers the `onConnectionStateChanged` callback when the network connection state changes. The SDK also triggers the `onConnectionInterrupted` and `onConnectionBanned` callbacks under certain circumstances, but Agora does not recommend using them.

#### 3. Improves the call rating system

v2.3.2 changes the rating parameter in the [`rate`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a748c30a6339ec9798daa0d1b21585411) method to "1 to 5" to encourage more feedback from end-users on the quality of a call or live broadcast. Application developers can use this feedback for future product improvement. Agora strongly recommends integrating this method in your application.


#### 4. Improves the audio device usability

v2.3.2 optimizes the audio device selection to fix the no audio issue when a user switches the audio device during a call or live broadcast.

#### 5. Other improvements

- Minimizes packet loss under unreliable network conditions in the Live Broadcast profile.
- Accelerates the video quality recovery under network congestion.
- Improves the stability in pushing streams.
- Optimizes the API calling threads.
- Checks the headset and Bluetooth device connection.
- Reduces the audio delay.
- Optimizes the screen-sharing function.

### Issues Fixed

The following issues are fixed in v2.3.2:

#### SDK

- Flashes when detecting a device.

#### Audio

- A user joins a live broadcast with a Bluetooth headset. The audio is not played through the Bluetooth headset when the user leaves the channel and opens another application.
- Crashes when calling the `startAudioMixing` method to play music files.
- A previously disabled microphone becomes enabled when the device connects to a headset.
- Cannot adjust the volume of the speaker when users change roles, join and leave channels, or a system phone or Siri interrupts.
- Users do not hear any voice for a while when an application switches back from the background. 
- Crashes in the audio module.

#### Video

- The users on the Web client cannot see the video sent from the Native client due to codec bugs.
- Hardware encoder issues when using an external video source on x86 devices.
- Occasional issues when using an external video source.
- The cursor on the remote side is not in the same position as the local side when sharing the desktop.
- Occasional long intervals between joining a channel and seeing the first remote video frame.
- The remote user's video is mirrored when sharing part of the desktop and when the application calls the `setVideoProfile` method to specify the video resolution.
- Blurry video when sharing a high-resolution screen.
- The cursor on the remote side is not in the same position as the local side when sharing the desktop.

### API Changes

To improve the user experience, Agora has made the following changes to the APIs:

#### Added

- [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e)
- [`setExternalVideoSource`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#a6716908edc14317f2f6f14ee4b1c01b7)
- [`pushVideoFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae064aedfdb6ac63a981ca77a6b315985)
- [`setExternalAudioSink`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a08450bffffc578290d4a1317f2938638)
- [`pullAudioFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#aaf43fc265eb4707bb59f1bf0cbe01940)
- [`setLocalPublishFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a0402734b50749081b20db3826f6f00ec)
- [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a50e727c34b662de64c03b0479a7fe8e7)
- [`getConnectionState`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a512b149d4dc249c04f9e30bd31767362)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a99ab2878e0c4fbf1be6970a2c545d085)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a8f8d2af4b4c7988934e152e3b281d734)
- [`onConnectionStateChanged`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af409b2e721d345a65a2c600cea2f5eb4)
- [`onLocalPublishFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ace4279c4d87c23a1fecc3eb8e862a513)
- [`onRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a7ee343146ad6e3f120bd04a7a6fdda74)
- [`onRemoteAudioStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#af8a59626a9265264fb4638e048091d3a)
- [`onRemoteAudioTransportStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#ad79bcd56075fa9c9f907bb4a7462352d)
- [`onRemoteVideoTransportStats`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a3b8fd883a31d4a504ac3cbd50b1c5d0f)

#### Deprecated

- [`setVideoProfile`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac8b16d2a4e67bd75231a76e06d2d85eb)
- [`onAudioQuality`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a36ad42975f3545382de07875016fb7fa)
- [`onConnectionInterrupted`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a9927b5cd2a67c1f48f17b5ed2303f483)
- [`onConnectionBanned`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a38e9d403ae4732dff71110b454149404)

## v2.2.2

v2.2.2 is released on June 21, 2018.

### Issues Fixed

- Fixed occasional online statistics crashes.
- Fixed the issue of failing to report the uid and volume of the speaker in a channel.
- Fixed the issue of occasional video freeze after a view size change.

## v2.2.1

v2.2.0 is released on May 30, 2018 and improves the internal code implementation. 

## v2.2.0

v2.2.0 is released on May 4, 2018. 

### New Features

#### 1. Play the audio effect in the channel

Adds a <code>publish</code> parameter in the <code>playEffect</code> method to enable the remote user in the channel to hear the audio effect played locally. 

>  If your SDK is upgraded to v2.2 from a previous version, pay attention to the functional changes of this API.

#### 2. Deploy the Proxy at the server

Agora provides a proxy package for enterprise users with corporate firewalls to deploy before accessing the services of Agora. See [Deploying the Enterprise Proxy](../../en/Quickstart%20Guide/proxy.md).

#### 3. Get the remote video state

Adds the <code>remoteVideoStateChangedOfUid</code> method to get the state of the remote video stream. 

#### 4. Add watermarks on the broadcasting video

Adds the watermark function for users to add a PNG file to the local or CDN broadcast as a watermark. Adds the <code>addVideoWatermark</code> and <code>clearVideoWatermarks</code> methods to add and delete watermarks in a local live-broadcast. Adds the <code>watermark</code> parameter in the <code>LiveTranscording</code> interface to add watermarks in CDN broadcasts. 

### Improvements

#### 1. Audio volume indication

Improves the <code>enableAudioVolumeIndication</code> method. This method once enabled, sends the audio volume indication of the speaker in its callback at set intervals, regardless of whether anyone is speaking in the channel.

#### 2. Network quality detection during a session

To meet the need for real-time network quality detection in the channel, the <code>onNetworkQuality</code> callback improves its data accuracy. 

#### 3. Last mile network quality detection before joining a channel

To test if the network condition can support audio or video calls before joining a channel, the <code>onLastmileQuality</code> callback changes its detection base from a fixed bitrate to the bitrate set in <code>videoProfile</code> to improve data accuracy. When the network condition is unknown, the SDK still triggers this callback once every 2 seconds. 

#### 4. Audio quality enhancement

Improves the audio quality in scenarios that involve music playback.

### Issues Fixed

-   Occasional screen display abnormalities when a large number of audience members join as the host in a live-broadcast channel.
-   Occasional audio issues during a live broadcast.


## v2.1.3

v2.1.3 is released on April 19, 2018. 

In v2.1.3, Agora updates the bitrate values of the <code>setVideoProfile</code> method in the live-broadcast profile. The bitrate values in v2.1.3 stay consistent with those in v2.0. 

### Issues Fixed

Occasional recording failures on some phones when a user leaves a channel and turns on the built-in recording device.

### Improvements

Improves the performance of screen sharing by shortening the time interval between which users switch from screen sharing to a normal communication or live-broadcast profile.

## v2.1.2

v2.1.2 is released on April 2, 2018. 

>  If you upgrade the SDK to v2.1.2 from a previous version, the live-broadcast video quality will be better than the communication video quality in the same resolutions, resulting in the live broadcasts using more bandwidth.

### Issues Fixed

-   The co-host is not seen by his/her counterpart after leaving and rejoining the channel.
-   The video resolution of the shared screen is worse in the communication profile than in the live broadcast profile.


## v2.1.1

v2.1.1 is released on March 16, 2018. 

Agora has identified a critical bug in SDK v2.1. Upgrade to v2.1.1 if you are using Agora SDK v2.1.

## v2.1.0

v2.1.0 is released on March 7, 2018. 

### New Features

#### 1. Voice Optimization

Adds a scenario for the game chat room to reduce the bandwidth and cancel the noise with the <code>setAudioProfile</code> method.

#### 2. Enhance the audio effect input from the built-in microphone

In an Interactive Broadcast scenario, the host can enhance the local audio effects from the built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods to implement the expected voice equalization and reverberation effects.

#### 3. Online statistics query

Adds Restful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or a host. See:

- Voice or Video Call scenario: [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_communication.md)
- Interactive Broadcast scenario: [Online Statistics Query API](../../en/API%20Reference/dashboard_restful_live.md)


#### 4. 17-way video

Adds the support of 17-way video in an Interactive Broadcast scenario. See:

-   [Starting a Live Video Broadcast](../../en/Quickstart%20Guide/broadcast_video_ios.md)
-   [Video Conference of 7+ Users](../../en/Interactive%20Broadcast/seventeen_people_windows.md)


#### 5. Injecting an external video stream

Adds the function of injecting an external video stream to an ongoing Live Broadcast. See [Injecting an External Stream to a Live Broadcast](../../en/Quickstart%20Guide/inject_stream_ios.md).


#### 6. Screen sharing for Interactive Broadcast

-   Before v2.1.0: The Agora SDK only supports the screen sharing function in a video call scenario.
-   From v2.1.0: The Agora SDK adds the screen-sharing function in interactive broadcasts.


#### 7. Loopback recording

Adds the <code>enableLoopbackRecording</code> method to collect all local sounds.

### Improvements

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Improvement</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>Video Freeze Rate</td>
<td>Reduces the video freeze rate in the audience mode and for specific devices.</td>
</tr>
<tr><td>Authentication</td>
<td>Supports a new authentication mechanism. Each legacy Dynamic Key (Channel Key) corresponds to a single privilege (for example, joining a channel), but each token in the new authentication mechanism includes all privileges (for example, joining a channel, host-in, and stream-pushing).</td>
</tr>
<tr><td>Billing Optimization</td>
<td>Small video resolutions are charged according to the voice-only mode. For example, 16 &times; 16.</td>
</tr>
</tbody>
</table>



### Issues Fixed

-   Occasional issues of the camera unable to capture the video source.
-   Occasional video freeze issues.
-   Occasional crashes.


## v2.0 and Earlier

### v2.0

v2.0 is released on November 21, 2017. 

#### Before Getting Started

Developers,

Due to the upgrade of Agora products, Windows SDK 2.0 no longer supports the Agora Recording Server version 1.8.2 and before, or relevant APIs.

**This may affect:**

-   Users of both the Windows SDK and Agora Recording Server v1.8.2 or before.
-   The following API methods are no longer supported: startRecordingService, stopRecordingService, and refreshRecordingServiceStatus.

**Two solutions are available:**

1.  Migrate from the Agora Recording Server v1.8.2 or before to the Agora Recording SDK v1.12 or later. The Agora Recording SDK does not require recording on the client side and does not affect subsequent upgrades of the Windows SDK (recommended). For how to use the Agora Recording SDK, refer to:

-   [Integrate the SDK](../../en/Recording/recording_integrate_cpp.md)
-   [Record a Call](../../en/Recording/recording_cmd_cpp.md)
-   [API Reference](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/recording_cpp/index.html)

2.  If you want to continue using the Agora Recording Server, you can use your current Windows SDK version (v1.14 and before). Contact Technical Support for any questions.


#### New Features

-   Adds the <code>setRemoteVideoStreamType</code> and <code>enableDualStreamMode</code> methods in the communication profile to support dual streams.
-   Supports the external audio source in the communication and live broadcast profiles by adding the following API methods:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>setExternalAudioSource</code></td>
<td>Sets the external audio source parameters, including the sample rate and channel number.</td>
</tr>
<tr><td><code>pushExternalAudioFrame</code></td>
<td>Pushes the external audio frame to the Agora SDK.</td>
</tr>
</tbody>
</table>



- Provides a set of RESTful APIs to ban a peer user from the server in the communication and live broadcast profiles. Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable the function, if required.
- Supports Windows (x64).
- Adds API methods such as setting the volume, mute states, and system volume settings.

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>setPlaybackDeviceVolume</code></td>
<td>Sets the volume of the playback device.</td>
</tr>
<tr><td><code>getPlaybackDeviceVolume</code></td>
<td>Gets the volume of the playback device.</td>
</tr>
<tr><td><code>setRecordingDeviceVolume</code></td>
<td>Sets the volume of the microphone.</td>
</tr>
<tr><td><code>getRecordingDeviceVolume</code></td>
<td>Gets the volume of the microphone.</td>
</tr>
<tr><td><code>setPlaybackDeviceMute</code></td>
<td>Mutes the playback device.</td>
</tr>
<tr><td><code>getPlaybackDeviceMute</code></td>
<td>Gets the mute status of the playback device.</td>
</tr>
<tr><td><code>setRecordingDeviceMute</code></td>
<td>Mutes the microphone.</td>
</tr>
<tr><td><code>getRecordingDeviceMute</code></td>
<td>Gets the mute status of the microphone.</td>
</tr>
<tr><td><code>onAudioDeviceVolumeChanged</code></td>
<td>Notifies the application when the volume of the playback device, recording device, or application changes.</td>
</tr>
</tbody>
</table>




### v1.14

v1.14 is released on October 20, 2017. 

#### New Features

-   Adds the <code>setAudioProfile</code> method to set the audio parameters and scenarios.
-   Adds the <code>setLocalVoicePitch</code> method to set the local voice pitch.

-   Live Broadcast: Adds the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor.


#### Improvements

-   Optimizes the audio at high bitrates.
-   Live Broadcast: The audience can view the host within one second in a single-stream mode (226 ms on average, and 204 ms under good network conditions).
-   Adds the ability to reduce the bandwidth:
    -   Before v1.14: If you muted the audio or disabled the video of a specific user, the network still sent the streams.
    -   Starting from v1.14: If you mute the audio or disable the video of a specific user, the network will not send the stream of the user to reduce the bandwidth.
-   Accurate control over the bitrate:
    -   Before v1.14: Inaccurate control over the bitrate often caused huge fluctuations, leading to network congestion and higher rates of packet and frame loss. This affected the accuracy of the bandwidth estimation module, especially when the network was in poor conditions.
    -   Starting from v1.14: Accurate control over the bitrate prevents huge fluctuations avoiding network congestion and shortening the transmission latency.


#### Issues Fixed:

-   Unable to hear any audio on certain Windows systems.
-   Camera-related issues on certain Windows systems.


### v1.13.1

v1.13.1 is released on September 28, 2017 and optimizes the echo issue under certain circumstances.

### v1.13

v1.13 is released on September 4, 2017. 

#### New Features:

-   Adds the function to dynamically enable and disable acquiring the sound card in a live broadcast.
-   Adds the function to disable the audio playback.
-   Supports the profile configuration for stream-pushing on the client side.
-   Adds the <code>onClientRoleChanged</code> callback to report on a user role switch between the host and audience in a live broadcast.
-   Supports the push-stream failure callback on the server side.


#### Improvements:

-   Screen Sharing: Enhances the video definition and fluency.
-   Screen Sharing: Supports updating the captured region dynamically.
-   The video profile is controllable by the software codec.


#### Issues Fixed:

Occasional crashes.

### v1.12

v1.12 is released on July 25, 2017. 

#### New Features:

-   Adds the <code>injectStream</code> method to inject an RTMP stream into the current channel in the Live-broadcast profile.
-   Adds the <code>aes-128-ecb</code> encryption mode in the <code>setEncryptionMode</code> method.
-   Adds the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.
-   Adds a set of APIs to manage the audio effect.
-   Adds the <code>ActiveSpeaker</code> method to report on the active speaker in the current channel.
-   Removes the <code>setScreenCaptureWindow</code> method, and updated the <code>startScreenCapture</code> method to share the whole screen and specify the window or region in the Communication profile.
-   Adds displaying the mouse function when the screen-sharing function is enabled in the Communication profile.
-   Web: Adds and updates a series of APIs to enable interoperability between the web browsers and native clients for the Communication or Live-broadcast profiles. See [Release Notes](../../en/Interactive%20Broadcast/release_web_video.md).


Recording: Adds APIs for real-time video mixing and web recording. See [Release Notes for the Recording SDK](../../en/Product%20Overview/release_recording.md).

#### Improvements:

Android/iOS/macOS/Windows: In the Communication profile, an improvement for the 320 &times; 180 resolution profile:

- Keeps the video smooth under poor network and equipment conditions.
- Enhances the image quality to be better than 180p under good network and equipment conditions.



#### Issues Fixed:

- Android: Bluetooth issues related to audio routing.
- Android/iOS/macOS/Windows: Occasional crashes.







