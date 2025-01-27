
---
title: 云端录制 RESTful API
description: Cloud recording restful api reference
platform: All Platforms
updatedAt: Thu Jun 13 2019 07:47:41 GMT+0800 (CST)
---
# 云端录制 RESTful API
阅读本文前请确保你已经了解如何使用 [RESTful API 录制](../../cn/cloud-recording/cloud_recording_rest.md)。

## <a name="acquire"></a>获取云端录制资源的 API

在开始云端录制之前，你需要调用该方法获取一个 resource ID。

>  一个 resource ID 只能用于一次云端录制服务。

- 方法：POST
- 接入点：/v1/apps/\<appid\>/cloud_recording/acquire

> 请求数限制为每秒钟 10 次。

调用该方法成功后，你可以从 HTTP 响应包体中的 `resourceId` 字段得到一个 resource ID。这个 resource ID 的时效为 5 分钟，你需要在 5 分钟内用这个 resource ID 去调用[开始云端录制的 API](#start)。

### 参数

该 API 需要在 URL 中传入以下参数。

| 参数    | 类型   | 描述                                                         |
| :------ | :----- | :----------------------------------------------------------- |
| `appid` | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。必须使用和待录制的频道相同的 App ID。 |

该 API 需要在请求包体中传入以下参数。

| 参数            | 类型   | 描述                                                         |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | 待录制的频道名。                                             |
| `uid`           | String | 云端录制使用的用户 ID，32 位无符号整数，取值范围 1 到 (2<sup>32</sup>-1)，需保证唯一性。 |
| `clientRequest` | JSON   | 特定的客户请求参数，对于该方法无需填入任何内容，为一个空的 JSON。 |

### HTTP 请求示例

```http
POST /v1/apps/<yourappid>/cloud_recording/acquire
Host: api.agora.io
Content-type: application/json;charset=utf-8
Authorization: Basic ZGJhZDMyNmFkMzQ0NDk2NGEzYzAwNjZiZmYwNTZmNjo2ZjIyMmZhMTkzNWE0MWQzYTczNzg2ODdiMmNiYjRh
Body:
{
    "cname": "httpClient463224",
    "uid": "527841",
    "clientRequest":{
    }
}
```

### 响应示例

```http
{
"Code": 200,
"Body":
{
 "resourceId": "JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG"
}
}
```

- `code`: Number 类型，[响应状态码](#status)。
- `resourceId`: String 类型，云端录制资源 resource ID，使用这个 resource ID 可以开始一段云端录制。这个 resource ID 的有效期为 5 分钟，超时需要重新请求。

## <a name="start"></a>开始云端录制的 API

获取 resource ID 后，调用该方法开始云端录制。

- 方法：POST
- 接入点：/v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/mode/\<mode\>/start

> 请求数限制为每秒钟 10 次。

### 参数

该 API 需要在 URL 中传入以下参数。

| 参数         | 类型   | 描述                                                         |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。必须使用和待录制的频道相同的 App ID。 |
| `resourceid` | String | 通过 [`acquire`](#acquire) 请求获取的 resource ID。          |
| `mode`       | String | 录制模式，目前只支持合流模式 `mix`。                         |

该 API 需要在请求包体中传入以下参数。

| 参数            | 类型   | 描述                                                         |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | 待录制的频道名。                                             |
| `uid`           | String | 云端录制使用的用户 ID，32 位无符号整数，取值范围 1 到 (2<sup>32</sup>-1)，需保证唯一性。 |
| `clientRequest` | JSON   | 特定的客户请求参数，对于该请求请参考下面的列表设置。         |

`clientRequest` 中需要填写的内容如下：

- `recordingConfig`：（选填）JSON 类型，录制的详细设置。
  - `streamTypes`：（选填）Number 类型，录制的媒体流类型。
    - `0`：仅录制音频
    - `1`：仅录制视频
    - `2`：（默认）录制音频和视频
  - `decryptionMode`：（选填）Number 类型，解密方案。如果频道设置了加密，该参数必须设置。解密方式必须与频道设置的加密方式一致。
    - `0`：无（默认）
    - `1`：设置 AES128XTS 解密方案
    - `2`：设置 AES128ECB 解密方案
    - `3`：设置 AES256XTS 解密方案
  - `secret`：（选填）String 类型。启用解密模式后，设置的解密密码。
  - `channelType`：（选填）Number 类型，频道模式。频道模式必须与 Agora Native/Web SDK 的设置一致，否则可能导致问题。
	  - `0`：通信模式（默认）
	  - `1`：直播模式
  - `audioProfile`：（选填）设置录制文件的音频采样率，码率，编码模式和声道数。
    - `0`：（默认）48 KHz 采样率，音乐编码，单声道，编码码率约 48 Kbps
    - `1`：48 KHz 采样率，音乐编码, 单声道，编码码率约 128 Kbps
    - `2`：48 KHz 采样率，音乐编码, 双声道，编码码率约 192 Kbps
  - `videoStreamType`：（选填）Number 类型，设置录制的视频流类型。如果频道中有用户开启了双流模式，你可以选择录制视频大流或者小流。
    - `0`：视频大流（默认），即高分辨率高码率的视频流
    - `1`：视频小流，即低分辨率低码率的视频流
  - `maxIdleTime`：（选填）Number 类型，最长空闲频道时间。默认值为 30 秒，该值需大于等于 5。如果频道内无用户的状态持续超过该时间，录制程序会自动退出。
  - `transcodingConfig`：（选填）JSON 类型，视频转码的详细设置。
    - `width`：（选填）Number 类型，录制视频的宽度，单位为像素，默认值 360。支持的最大分辨率为 1080p，超过该分辨率会报错。
    - `height`：（选填）Number 类型，录制视频的高度，单位为像素，默认值 640。支持的最大分辨率为 1080p，超过该分辨率会报错。
    - `fps`：（选填）Number 类型，录制视频的帧率，单位 fps，默认值 15。
    - `bitrate`：（选填）Number 类型，录制视频的码率，单位 Kbps，默认值 500。
    - `maxResolutionUid`：（选填）String 类型，如果视频合流布局设为垂直布局，用该参数指定显示大视窗画面的用户 ID。
    - `mixedVideoLayout`：（选填）Number 类型，视频合流布局，详见[设置合流布局](../../cn/cloud-recording/cloud_layout_guide.md)。
      - `0`：（默认）悬浮布局。第一个加入频道的用户在屏幕上会显示为大视窗，铺满整个画布，其他用户的视频画面会显示为小视窗，从下到上水平排列，最多 4 行，每行 4 个画面，最多支持共 17 个录制画面。
      - `1`：自适应布局。根据用户的数量自动调整每个画面的大小，每个用户的画面大小一致，最多支持 17 个录制画面。
      - `2`：垂直布局。指定一个用户在屏幕左侧显示大视窗画面，其他用户的小视窗画面在右侧垂直排列，最多两列，一列 8 个画面，最多支持共 17 个录制画面。
  
- `storageConfig`：JSON 类型，第三方云存储的详细设置。
  - `vendor`：Number 类型，第三方云存储供应商。    
    - `0`：七牛云
    - `1`：Amazon S3
    - `2`：阿里云
  - `region`：Number 类型，第三方云存储指定的地区信息。
    当 `vendor` = 0，即第三方云存储为七牛云时：  
    - `0`：Huadong 
    - `1`：Huabei 
    - `2`：Huanan 
    - `3`：Beimei  

    当 `vendor` = 1，即第三方云存储为 Amazon S3 时：
    - `0`：US_EAST_1 
    - `1`：US_EAST_2 
    - `2`：US_WEST_1 
    - `3`：US_WEST_2 
    - `4`：EU_WEST_1 
    - `5`：EU_WEST_2 
    - `6`：EU_WEST_3 
    - `7`：EU_CENTRAL_1 
    - `8`：AP_SOUTHEAST_1 
    - `9`：AP_SOUTHEAST_2 
    - `10`：AP_NORTHEAST_1 
    - `11`：AP_NORTHEAST_2 
    - `12`：SA_EAST_1 
    - `13`：CA_CENTRAL_1 
    - `14`：AP_SOUTH_1 
    - `15`：CN_NORTH_1 
    - `16`：CN_NORTHWEST_1 
    - `17`：US_GOV_WEST_1 

    当 `vendor` = 2，即第三方云存储为阿里云时： 
    - `0`：CN_Hangzhou 
    - `1`：CN_Shanghai 
    - `2`：CN_Qingdao 
    - `3`：CN_Beijin 
    - `4`：CN_Zhangjiakou 
    - `5`：CN_Huhehaote 
    - `6`：CN_Shenzhen 
    - `7`：CN_Hongkong 
    - `8`：US_West_1 
    - `9`：US_East_1 
    - `10`：AP_Southeast_1 
    - `11`：AP_Southeast_2 
    - `12`：AP_Southeast_3 
    - `13`：AP_Southeast_5 
    - `14`：AP_Northeast_1 
    - `15`：AP_South_1 
    - `16`：EU_Central_1 
    - `17`：EU_West_1 
    - `18`：EU_East_1
  
  - `bucket`：String 类型，第三方云存储的 bucket。
  - `access_key`：String 类型，第三方云存储的 access key。
  - `secret_key`：String 类型，第三方云存储的 secret key。

### HTTP 请求示例

```http
POST /v1/apps/<yourappid>/cloud_recording/resourceid/JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG/mode/mix/start
Host: api.agora.io
Content-type: application/json;charset=utf-8
Authorization: Basic ZGJhZDMyNmFkMzQ0NDk2NGEzYzAwNjZiZmYwNTZmNjo2ZjIyMmZhMTkzNWE0MWQzYTczNzg2ODdiMmNiYjRh
Body:
{
    "uid": "527841",
    "cname": "httpClient463224",
    "clientRequest": {
        "recordingConfig": {
            "maxIdleTime": 30,
            "streamTypes": 2,
            "audioProfile": 1,
            "channelType": 0, 
            "videoStreamType": 1, 
            "transcodingConfig": {
                "height": 640, 
                "bitrate": 500, 
                "fps": 15, 
                "mixedVideoLayout": 1,
                "maxResolutionUid": "1",
                "width": 360
            }
        }, 
        "storageConfig": {
            "accessKey": "xxxxxxf",
            "region": 3,
            "bucket": "xxxxx",
            "secretKey": "xxxxx",
            "vendor": 2
        }
    }
}
```

### 响应示例

```http
{
"Code": 200,
"Body":
{
  "sid": "38f8e3cfdc474cd56fc1ceba380d7e1a", 
  "resourceId": "JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG"
}
}
```

- `code`: Number 类型，[响应状态码](#status)。
- `resourceId`: String 类型，云端录制使用的 resource ID。
- `sid`: String 类型，录制 ID。成功开始云端录制后，你会得到一个 sid （录制 ID)。该 ID 是一次录制周期的唯一标识。

## <a name="query"></a>查询云端录制状态的 API

- 方法：GET
- 接入点：/v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/sid/\<sid\>/mode/\<mode\>/query

> 请求数限制为每秒钟 10 次。

### 参数

该 API 需要在 URL 中传入以下参数。

| 参数       | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| appid      | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。必须使用和待录制的频道相同的 App ID。 |
| resourceid | String | 通过 [`acquire`](#acquire) 请求获取的 resource ID。          |
| sid        | String | 通过 [`start`](#start) 请求获取的录制 ID。                   |
| mode       | String | 录制模式，目前只支持合流模式 `mix`。                         |

### HTTP 请求示例

```http
GET /v1/apps/<yourappid>/cloud_recording/resourceid/JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG/sid/38f8e3cfdc474cd56fc1ceba380d7e1a/mode/mix/query
Host: api.agora.io
Content-type: application/json;charset=utf-8
Authorization: Basic ZGJhZDMyNmFkMzQ0NDk2NGEzYzAwNjZiZmYwNTZmNjo2ZjIyMmZhMTkzNWE0MWQzYTczNzg2ODdiMmNiYjRh
```

### 响应示例

```http
{
"Code": 200,
"Body":
{
  "resourceId":"JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG",
  "sid":"38f8e3cfdc474cd56fc1ceba380d7e1a",
  "serverResponse":{
    "fileList": "xxxxx.m3u8",
    "status": "5"
    }    
}
}
```

- `code`：Number 类型，[响应状态码](#status)。
- `resourceId`: String 类型，云端录制使用的 resource ID。
- `sid`: String 类型，录制 ID。成功开始云端录制后，你会得到一个 sid （录制 ID)。该 ID 是一次录制周期的唯一标识。
- `serverResponse`：JSON 类型，录制的具体信息。
  - `fileList`：String 类型，录制生成的文件索引。每次录制均会生成一个 M3U8 文件，用于索引该次录制所有的 TS 文件。你可以通过 M3U8 文件播放和管理录制文件。
  - `status`：Number 类型，当前录制的状态。
    - `0`：没有开始云端录制。
    - `1`：云端录制初始化完成。
    - `2`：录制组件开始启动。
    - `3`：上传组件启动完成。
    - `4`：录制组件启动完成。
    - `5`：已成功上传第一个文件。第一个文件上传完成后，录制在进行中均会返回此状态。
    - `6`：已经停止录制。
    - `7`：云端录制服务全部停止。
    - `8`：云端录制准备退出。
    - `20`：云端录制异常退出。

## <a name="stop"></a>停止云端录制的 API

录制完成后，调用该方法离开频道，停止录制。录制停止后如需再次录制，必须再调用  [`acquire`](#acquire) 方法请求一个新的 resource ID。

- 方法：POST
- 接入点：/v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/sid/\<sid\>/mode/\<mode\>/stop

> - 请求数限制为每秒钟 10 次。
> - 当频道空闲（无用户）超过预设时间（默认为 30 秒） 后，云端录制也会自动退出频道停止录制。

### 参数

该 API 需要在 URL 中传入以下参数。

| 参数         | 类型   | 描述                                                         |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | 你的项目使用的 [App ID](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-appid-a-app-id)。必须使用和待录制的频道相同的 App ID。 |
| `resourceid` | String | 通过 [`acquire`](#acquire) 请求获取的 resource ID。          |
| `sid`        | String | 通过 [`start`](#start) 请求获取的录制 ID。                   |
| `mode`       | String | 录制模式，目前只支持合流模式 `mix`。                         |

### HTTP 请求示例

```http
POST /v1/apps/<yourappid>/cloud_recording/resourceid/JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG/sid/38f8e3cfdc474cd56fc1ceba380d7e1a/mode/mix/stop
Host: api.agora.io
Content-type: application/json;charset=utf-8
Authorization: Basic ZGJhZDMyNmFkMzQ0NDk2NGEzYzAwNjZiZmYwNTZmNjo2ZjIyMmZhMTkzNWE0MWQzYTczNzg2ODdiMmNiYjRh
```

### 响应示例

```http
{
"Code": 200,
"Body":
{
  "resourceId":"JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG",
  "sid":"38f8e3cfdc474cd56fc1ceba380d7e1a",
  "serverResponse":{
    "fileList": "xxxxx.m3u8",
    "uploadingStatus": "uploaded"
  }
}
}
```

- `code`: Number 类型，[响应状态码](#status)。
- `resourceId`: String 类型，云端录制使用的 resource ID。
- `sid`: String 类型，录制 ID。成功开始云端录制后，你会得到一个 sid （录制 ID)。该 ID 是一次录制周期的唯一标识。
- `serverResponse`: JSON 类型，录制的具体信息。
  - `fileList`：String 类型，录制生成的文件索引。每次录制均会生成一个 M3U8 文件，用于索引该次录制所有的 TS 文件。你可以通过 M3U8 文件播放和管理录制文件。
  - `uploadingStatus`：String 类型，当前录制上传的状态。
    - `"uploaded"`：本次录制的文件已经全部上传至客户指定的第三方云存储。
    - `"backuped"`：本次录制的文件已经全部上传完成，但是至少有一个 TS 文件上传到了 Agora 云备份。Agora 服务器会自动将这部分文件继续上传至指定的第三方云存储。
    - `"unknown"`：未知状态，请尝试调用 [`query`](#query) 方法查询状态。

## <a name="status"></a>响应状态码

| 状态码 | 描述        |
| :----- | :-------------------- |
| 200    | 请求成功。            |
| 201    | 成功请求并创建了新的资源。              |
| 206    | 服务器成功处理了部分 GET 请求。      |
| 400    | 请求的语法错误（如参数错误），服务器无法理解。            |
| 404    | 服务器无法根据请求找到资源（网页）。         |
| 500    | 服务器内部错误，无法完成请求。 |
| 504    | 充当网关或代理的服务器未及时从远端服务器获取请求。      |
