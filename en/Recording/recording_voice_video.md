
---
title: Recording Voice and Video
description: 
platform: All_Platforms
updatedAt: Wed Jun 12 2019 06:21:51 GMT+0800 (CST)
---
# Recording Voice and Video
This page shows how to use the Agora On-premise Recording SDK to enable voice and video recording and use the transcoding scripts.

-   During voice recording, if a user leaves the channel and rejoins it after 15 seconds, a new recording session will be created, and two separate recorded files will be generated.
-   During video recording, if a user leaves the channel and rejoins (regardless of the time interval), changes the resolution, mutes or unmutes the video, the recorded video file can be split.


## Recording Files

The Agora On-premise Recording SDK supports recording in two modes:

-   Individual recording: To record the voice and video stream respectively for each user ID.
-   Composite recording: To mix the voice and video recordings for different users in the same channel; the video mixing layout is also supported.

> The On-premise Recording SDK must use the same channel profile as the Agora Native/Web SDK, otherwise issues may occur.
### <a name ="individualrecording"></a>Individual Recording

Individual recording enables the SDK to record the voice and video respectively for each user ID in the channel.

Set <code>isMixingEnabled</code> = 0 to start individual recording.

Choose the recording mode by setting the parameters according to the following table:

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Recording Mode</th>
<th>Parameters</th>
<th>File Before Transcoding</th>
</tr>
</thead>
<tbody>
<tr><td>Voice-only</td>
<td><code>isAudioOnly</code> = 1, <code>isVideoOnly</code> = 0</td>
<td>A voice file for each user ID.</td>
</tr>
<tr><td>Video-only</td>
<td><code>isAudioOnly</code> = 0, <code>isVideoOnly</code> = 1</td>
<td><ul>
<li>An MPEG-4 file for each user ID (Agora Native SDK).</li>
<li>A WebM file for each user ID (Web).</li>
</ul>
</td>
</tr>
<tr><td>Voice + video</td>
<td><code>isAudioOnly</code> = 0, <code>isVideoOnly</code> = 0</td>
<td>A voice/MPEG-4/WebM file for each user ID. Voice and video are separated.</td>
</tr>
</tbody>
</table>



To merge the recorded voice and video files of each user ID, use the *video\_convert.py* script for transcoding. See [Using the Transcoding Script](#using-transcoding-script).

### Composite Recording + Unsynchronized Transcoding

Set <code>isMixingEnabled</code> = 1 and <code>mixedVideoAudio</code> = 0 to start composite recording with unsynchronized transcoding.

This enables mixed-voice and mixed-video recordings, and the voice and video files are separated.

You can also call `setVideoMixingLayout` to set the video mixing layout.

Choose the recording mode by setting the parameters according to the following table:

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Recording Mode</th>
<th>Parameter</th>
<th>Files Before Transcoding</th>
</tr>
</thead>
<tbody>
<tr><td>Voice-only</td>
<td><code>isAudioOnly</code> = 1, <code>isVideoOnly</code> = 0</td>
<td>A mixed AAC voice file</td>
</tr>
<tr><td>Video-only</td>
<td><code>isAudioOnly</code> = 0, <code>isVideoOnly</code> = 1</td>
<td>A mixed MPEG-4 video file.</td>
</tr>
<tr><td>Voice + video</td>
<td><code>isAudioOnly</code> = 0, <code>isVideoOnly</code> = 0</td>
<td>A mixed AAC voice file and a mixed MPEG-4 video file (two separate files).</td>
</tr>
</tbody>
</table>



To merge the recorded voice and video files, use the `video\convert.py` script for transcoding. See [Using the Transcoding Script](#using-transcoding-script).

### Composite Recording + Synchronized Transcoding

Set <code>isMixingEnabled</code> = 1 and <code>mixedVideoAudio</code> = 1 to start composite recording with synchronized transcoding. This enables mixed voice and video recording without using the transcoding script.

<table>
<thead>
<tr><th>Recording Mode</th>
<th>Parameter</th>
<th>File</th>
</tr>
</thead>
<tbody>
<tr><td>Voice + video</td>
<td><code>isAudioOnly</code> = 0, <code>isVideoOnly</code> = 0</td>
<td>An MPEG-4 voice + video file.</td>
</tr>
</tbody>
</table>


## Raw Data

The Agora On-premise Recording SDK supports raw data in individual recording, and mixed voice in composite recording.

### Individual Recording

Choose the recording mode by setting the parameters according to the following table:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Recording Mode</th>
<th>File</th>
</tr>
</thead>
<tbody>
<tr><td>Voice-only</td>
<td><ul>
<li>AAC file: AUDIO_FORMAT_AAC_FRAME_TYPE = 1</li>
<li>PCM file: AUDIO_FORMAT_PCM_FRAME_TYPE = 2</li>
</ul>
</td>
</tr>
<tr><td>Video-only</td>
<td><ul>
<li>H264 file: VIDEO_FORMAT_H264_FRAME_TYPE = 1</li>
<li>YUV file: VIDEO_FORMAT_YUV_FRAME_TYPE = 2</li>
</ul>
</td>
</tr>
<tr><td>Voice + video</td>
<td><ul>
<li>H264 + AAC file</li>
<li>H264 + PCM file</li>
<li>YUV + AAC file</li>
<li>YUV + PCM file</li>
</ul>
</td>
</tr>
</tbody>
</table>



> The Web supports raw data in YUV only, not in H264.

### Composite Recording

The Agora On-premise Recording SDK supports raw data for mixed voice, and generates a PCM file.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Recording Mode</th>
<th>Parameter</th>
<th>File</th>
</tr>
</thead>
<tbody>
<tr><td>Voice-only</td>
<td><code>isMixingEnabled</code> = 1, <code>isAudioOnly</code> = 1</td>
<td>PCM file: AUDIO_FORMAT_MIXED_PCM_FRAM_TYPE = 3</td>
</tr>
</tbody>
</table>



## Screen Capture

The Agora On-premise Recording SDK supports screen capture in individual recording only, and no subsequent transcoding is needed.

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Recording Mode</th>
<th>Parameter</th>
<th>File</th>
</tr>
</thead>
<tbody>
<tr><td>Video-only</td>
<td><code>isVideoOnly</code> = 1, <code>decodeVideo</code> = 3/4, <code>captureInterval</code> &ge; <sup>[1]</sup></td>
<td><ul>
<li>JPEG file</li>
<li>JPEG buffer</li>
</ul>
</td>
</tr>
</tbody>
</table>



> [1] The <code>captureInterval</code> parameter sets the time interval for each screen capture. The minimum value is 1 second and the default value is 5 seconds. Ensure that <code>decodeVideo</code> = 3/4. 

## Screen Capture + Recording

The Agora On-premise Recording SDK supports screen capture and recording in individual recording only. Transcoding is not necessary for screen capture. For transcoding the recorded file, see [Individual Recording](#individualrecording).

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Recording Mode</th>
<th>Parameter</th>
<th>File</th>
</tr>
</thead>
<tbody>
<tr><td>Screen capture + recording</td>
<td><code>decodeVideo</code> = 5, <code>captureInterval</code> &ge; 1</td>
<td><ul>
<li>An MPEG-4 file (untranscoded) for each user ID on the Agora Native SDK.</li>
<li>A WebM file (untranscoded) for each user ID on the Agora Web SDK.</li>
<li>A JPEG file.</li>
</ul>
</td>
</tr>
</tbody>
</table>

<a name = "Managing_the_Recorded_Files"></a>

## Managing the Recorded Files

### Set the directory structure

#### Use the default directory structure 

Set the root directory of the recorded files by the `recordFileRootDir` parameter and the sub-directory is generated automatically. 

For example, the path to a recorded MP4 file is `yyyymmdd/ChannelName_HHMMSS_MSUSNS/xxxx.mp4`: 

- `yyyymmdd`: The date when the SDK joins the channel. All files and directories on the same day are stored under this directory and the time zone is UTC+0.

- `ChannelName_HHMMSS_MSUSNS`: The recorded files are stored in the directory created on the same date as the recording. The directory name contains a channel name and a timestamp (hour, minute, second, millisecond, microsecond, and nanosecond). The timestamp is the time when the server starts recording, and the time zone is UTC+0.

> - For versions earlier than v2.3.0, the directory of the recorded files is `ChannelName_HHMMSS`, named after the channel name and timestamp (hour, minute, and second).
> - For v2.3.0 or later, the directory of the recorded files is `ChannelName_HHMMSS_MSUSNS`, named after the channel name and timestamp (hour, minute, second, millisecond, microsecond, and nanosecond).

#### Customize the directory structure

To customize the directory structure, you need to create a configuration file in JSON format and specify the path of the configuration file through the `cfgFilePath` parameter.

Set the `Recording_Dir` parameter in the configuration file to customize the directory structure. For example: `{“Recording_Dir” : “<recording path>”}`. You cannot change the `Recording_Dir` parameter.

> We do not recommend customizing the directory structure because if multiple recording instances use the same configuration parameter, the recorded files of different recording instances are stored in the same directory and cannot be distinguished.

### The recorded files

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>File</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>UID_HHMMSSMS.aac</td>
<td>If you record on the Agora Native SDK, PC, or the Web, each uid has an AAC file containing the voice content.</td>
</tr>
<tr><td>UID_HHMMSSMS.mp4</td>
<td>If you record on the Agora Native SDK or PC, each uid has an MPEG-4 file. If the user joins and leaves the channel multiple times, this uid can have multiple MPEG-4 files containing the video content.</td>
</tr>
<tr><td>UID_HHMMSSMS.webm</td>
<td>If you record on the Web, each uid has a WebM file. If the user joins and leaves the channel multiple times, this uid can have multiple WebM files containing the video content.</td>
</tr>
<tr><td>recording2-done.txt</td>
<td>This text file indicates that the recording in this channel has finished.</td>
</tr>
<tr><td>UID_HHMMSSMS.txt</td>
<td>This text file indicates the start and end time of each voice or video file and the related information, such as the video width and rotation.</td>
</tr>
</tbody>
</table>


## <a name="using-transcoding-script"></a>Using the Transcoding Script

Once recording is finished, use` video_convert.py` and `ffmpeg` to merge the recorded files (decompress the transcoding tool with the command line `tar -xvf`):

-   If multiple voice files are generated after the recording, transcoding merges them into one M4A file in the format of `UIDHHMMSSMS.m4a`.
-   If multiple voice and video files are generated after the recording, transcoding merges them into one MPEG-4 file in the format of `UID_HHMMSSMS_av.mp4`. To merge the voice and video files by the session:
    -   If a uid leaves a channel and rejoins it within 15 seconds, the Agora On-premise Recording SDK considers this session as one. A new voice file is not generated but a new video file is generated and merged into the voice and video file, and a `UIDHHMMSSMSav.mp4` file is generated.
    -   If a uid leaves a channel and rejoins it after 15 seconds, the Agora On-premise Recording SDK considers this as two separate sessions. A new voice and video file is generated creating a new `UIDHHMMSSMSav.mp4` file for the new session.
-   If multiple voice and video files are generated after the recording, and you wish to merge the MPEG-4 files of different sessions by the uid, transcoding merges them into different MPEG-4 files such as `UID_0_merge_av.mp4`, `UID_1_merge_av.mp4`, and `UID_2_merge_av.mp4`.
    -   In the automatically record mode, the <code>-m</code> parameter merges all voice and video files of one uid and generates a single `UID_0_merge_av.mp4` file.
    -   In the manually record mode, the <code>startService</code> and <code>stopService</code> parameters manage and divide the recorded files. Each start/stop makes one service, and the <code>-m</code> parameter generates multiple `UID_XX_merge_av.mp4` files.

The transcoding tool includes `video_convert.py` and `ffmpeg`. The Python script can merge the separated voice and video recorded files into one MPEG-4 file and the script relies on `ffmpeg`. You can get the transcoding tool `ffmpeg` and `video_convert.py` in the **tools** folder in [On-premise Recording SDK](https://docs-preview.agoralab.co/en/Recording/downloads). Decompress `ffmpeg`, and make sure it is in the same directory as `video_convert.py.` Execute the following command to run the transcoding tool:

Execute `python video_convert.py` with the following usage:

```
Usage: video_convert.py [options]

Options:
  -h, --help
  -f FOLDER, --folder=FOLDER
                        Convert folder
  -m MODE, --mode=MODE  Convert merge mode, [0: txt merge A/V(Default); 1: uid
                        merge A/V; 2: uid merge audio; 3: uid merge video]
  -p PFS,  --fps=FPS    Convert fps, default 15
  -s --saving           Convert Do not time sync
  -r RESOLUTION, --resolution=RESOLUTION
                        Specific resolution to convert '-r width height'
                        Eg: '-r 640 360'
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Option</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td><code>-f</code></td>
<td>Directory of the file to be transcoded.</td>
</tr>
<tr><td><code>-m</code></td>
<td><p>Transcoding mode:</p>
<ul>
<li>0: Transcode by the session. Merge the voice and video file of one session into one file.</li>
<li>1: Merge the voice and video file of the same uid into a file chronologically.</li>
<li>2: Merge the voice file of the same uid into a file chronologically.</li>
<li>3: Merge the video file of the same uid into a file chronologically.</li>
</ul>
</td>
</tr>
<tr><td><code>-p</code></td>
<td>Parameter for setting the frame rate in both composite and individual recording. 15 fps is the default value. <sup>[3]</sup></td>
</tr>
<tr><td><code>-s</code></td>
<td>The saving mode that indicates if the transcoding should be strictly synchronized with time; in other words, if the time interval when the user is not in the channel remains in the recorded file. Make sure that you use this parameter together with -m = 1/2/3. The default value indicates “always recording”. <sup>[4]</sup></td>
</tr>
<tr><td><code>-r</code></td>
<td>Parameter for setting the resolution of transcoding in the “width height” format. For the supported resolution, see `joinChannel` to allows an application to join a channel.</td>
</tr>
</tbody>
</table>

> - [3] The <code>-p</code> parameter sets the frame rate from 5 fps to 120 fps. If it is set to less than 5 fps, the SDK treats it as 5 fps.
> - [4] For “always recording”, if a user leaves and rejoins a channel, the idle time is shown as a black screen in the recorded file. For example, if a user is in a channel for 2 minutes, then leaves a channel for 30 minutes before rejoining the channel and spending another 2 minutes in it, then the total recorded time is 34 minutes with 30 minutes of black screen.

See the following figure for the different transcoding script options:

<img alt="../_images/recording_demo.png" src="https://web-cdn.agora.io/docs-files/en/recording_demo.png" />


The transcoded MPEG-4 file supports the following players:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Operation System</th>
<th>Players</th>
</tr>
</thead>
<tbody>
<tr><td>Windows</td>
<td>Windows Media Player, KMPlayer, VLC Media Player</td>
</tr>
<tr><td>Mac</td>
<td>QuickTime, Movist, MPlayerX, KMPlayer</td>
</tr>
<tr><td>iOS</td>
<td>iOS default player, VLC Media Player, KMPlayer</td>
</tr>
<tr><td>Android</td>
<td>Android default player, MX Player, VLC Media Player, KMPlayer</td>
</tr>
</tbody>
</table>


>  You can start transcoding only if a recording2-done.txt file exists in the recording folder. A convert-done.txt file is generated after the transcoding is complete. Once the transcoding script is used, a convert.log file is generated in the same directory as the voice and video files upon completion of the transcoding.

## Protecting Recorded Files

The recorded files are stored on your server only, and Agora has no access to them. You are responsible for the protection and security of the recorded files. Consult a security expert if necessary.

> The `recording_sys.log` files under the same directory of the recorded files list any exception or problem that occurred during recording.


