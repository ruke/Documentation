
---
title: Agora RTM Overview
description: 
platform: All Platforms
updatedAt: Mon May 20 2019 06:20:26 GMT+0800 (CST)
---
# Agora RTM Overview
The Agora RTM SDK provides a stable messaging mechanism for you to build real-time messaging scenarios with low latency and high concurrency at a global level. 

## Functions

The Agora RTM SDK enables the following functions:

-   Send and receive peer-to-peer messages.
-   Send and receive channel messages.
-   Get the member list of the channel.
-   Create, send, cancel, accept, or decline a call invitation. 


## Applications

The Agora RTM SDK can be used in the following scenarios:

<table>
  <tr>
    <th>Industry</th>
    <th>Application</th>
  </tr>
  <tr>
    <td>Live Broadcast</td>
    <td><li>Commentaries<br><li>Chatrooms<br><li>Send gifts<br><li>Likes<br><li>Maintenance of the chat room status (e.g., number of the channel members)<br><li>Privilege management (remove or mute a specified user)<br></td>
  </tr>
  <tr>
    <td>Social Network</td>
    <td><li>Private chat messages<br><li>Group messages<br><li>Voice/Video call invitation commands<br></td>
  </tr>
  <tr>
    <td>Education</td>
    <td><li>Class group messages<br><li>Private chat messages<br><li>Whiteboard<br><li>Privilege management (awards, presenting, hands up or likes)<br></td>
  </tr>
  <tr>
    <td>IoT</td>
    <td>Control messages</td>
  </tr>
</table>

## Features

The Agora RTM SDK provides the following features:

<table border="1" width="100%">
  <tr>
    <th width="20%">Feature </th>
    <th width="50%">Description</th>
  </tr>
  <tr>
    <td>High concurrency</td>
    <td>Supports sending up to a million  channel messages at the same time. Can cope with the high concurrency situations typical in an online quizzing scenario. <br></td>
  </tr>
  <tr>
    <td>High reliability</td>
    <td>Service availability at 99.999%</td>
  </tr>
	  <tr>
    <td>Low latency</td>
    <td>Our data centers are deployed globally. Ensures the average latency is:<li>Less than 200 ms at a global level;<br><li>Less than 100 ms in the same region.<br></td>
  </tr>
	  <tr>
    <td>Compatibility</td>
    <td>Supports the following platforms:<li>iOS, Android (arm64, armv7, x86), macOS, Windows (coming soon), Linux, and Web;<br><li>Java server and C++ server<br></td>
  </tr>
</table>

## Real-time Messaging vs. Signaling

The Agora Real-time Messaging SDK (RTM) is designed to replace the legacy Signaling SDK with much more exciting features. 

Our plan: 

- Interoperability between the Agora RTM SDK and the Agora Signaling SDK is not yet implemented but will be implemented soon. 
- Maintenance to the legacy Signaling SDK will end in the fourth quarter of 2019. 

