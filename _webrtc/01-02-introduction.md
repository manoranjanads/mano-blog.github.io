---
title: "Introduction to WebRTC Libraries"
permalink: /webrtc/introduction.html
excerpt: "WebRTC Library Comparision"
header:
  overlay_image: /assets/images/webrtc/01-02-introduction/introduction.png
  teaser: /assets/images/webrtc/01-02-introduction/introduction.png
  overlay_filter: 0.5
last_modified_at: 2018-10-19T15:58:49-04:00
redirect_from:
  - /theme-setup/
toc: true
author: Jay
---

If you are visiting this blog, hope that you are already familiar with [WebRTC](https://webrtc.org/). This guide will not cover the basics of WebRTC

[Meetrix.IO](https://meetrix.io) provides professional services for webRTC. If you are looking for a service regarding any of the following libraries, please shoot an email to `hello@meetrix.io`
{: .notice--success}

## What this guide is about

Well, this would be more in to the use cases of webrtc. Specially our experience with opensource WebRTC libraries and tools. We'll discuss about settingup, bringing them to production, pros and cons.
This guide would be useful if you are,
1. Not willing to use proprietary libraries or third party APIs for your WebRTC application
2. Want to build your own WebRTC infrastructure
3. Heavily using AWS infrastructure

**Note:** This might not discuss about all the available WebRTC libraries and tools, but the ones we have experienced.
{: .notice--info}

## What this is Not About

Definitely this is not an introduction to WebRTC. And this might not be useful in following cases.

1. If you dont want to manage your own WebRTC infrastructure
2. If you just need a single peer to peer connection, and not looking for scale it up. 

## Use Cases

These are some use cases that we have experienced with and this guide is mainly written with that experience.

1. Online Tutoring
2. Online Interviewing
3. Video Conferencing with SIP integration
4. Tele-Medicine
5. Remote Expert Support
6. Real-Time Object Recognition
		

## Comparision

Here is the list of the tools and libraries that we are going to discuss and a comparision about their features.

|                   |   P2P         | SFU Mode  | MCU Mode  | Chat  | Screen Sharing    | File Sharing  | Recording | Mobile SDK|
|:---:              |---:           |---:       |---:       |---:   |---:               |---:           |---:       |---:       |
| Jitsi             | yes           | yes       | x         | yes   | yes               | x             | yes       | yes       |
| Kurento           | x             | yes       | yes       | yes   | no                | x             | yes       | yes       |
| SimpleWebRTC      | yes           | x         | x         | yes   | yes               | yes           | x         | x         |
{: rules="groups"}
---


## Jitsi

[Jitsi](https://jitsi.org/) is a matured open-source web-based conferencing system.
Jitsi project has an open-source and complete video conferencing solution similar to Google Hangouts called [Jitsi Meet](https://github.com/jitsi/jitsi-meet).
It consists with Jitsi Video Bridge (JVB), Jitsi Conference Focus(Jicofo) and Prosody as default XMPP signaling and message passing component.
Backend is written in Java and the front-end is written in react. Jitsi also has native apps built with react-bative and high level SDKs for Android and IOS.

Jitsi Video Bridge (JVB) is the core of Jitsi Meet. It's a Selective Forwarding Unit (SFU) which means it works as a video relay and
does not mix the video streams together to generate a composite conference video. If there is a video conference with 3 users,
User `A` will be sending his stream to to JVB. JVB will be relaying the video stream to `B` and `C` users.  
In this case, each user will be uploading one video stream and downloading two video streams.
An advantage of SFU over P2P model is, you only have one upstream. But you have same number of down streams as P2P model.
Compared to MCU an advantage of SFU is that you do not need a lot of computing power since the video streams does not get mixed.
You can easily setup a demo Jitsi Meet on AWS even with a t2 micro instance (with 1Gig Memory an 1 VCPU).
The major disadvantage of SFU is that, the server and participants should have a lot of available bandwidth. So remember, 
if you setup Jitsi Meet, you need to have a good bandwidth.

- [Official Jitsi website](http://jitsi.org)
- [Jitsi on GitHub](https://github.com/jitsi/jitsi)


## Kurento

[Kurento](http://kurento.org) is one of the most famous and well-designed Multipoint Control Unit (MCU) based conferencing tools.
It also can be configured to function as a SFU. Although the project went silent after
the Kurento Team has been aqui-hired by [Twilo](https://www.twilio.com/), still it's one of our favorites. 
Since MCU mixes up the video/audio streams of the participants to create a composite video stream (you can decide this), you should definitely provide lot of computing power when the number of participants increases in a conference.
When configured to act as an MCU, each participant will be uploading a single video stream, and will be receiving a single mixed video stream from Kurento Media Server.
Despite of cpu requirement, it has the advantage of low bandwith consumption.
The core of Kurento is written in C++ and it exposes a control API.
There are bunch of SDKs and samples apps built by Kurento Team for Java, , NodeJs and JavaScript. But some of these repositories are not well documented.
One of the most interesting things with Kurento is it's modular architecture.
For example you can apply diffrent filters and plugins (say OpenCV plugis) for video streams.

- [Official Kurento website](http://kurento.org)
- [Kurento on Git Hub](https://github.com/kurento)
- [Documentation](https://doc-kurento.readthedocs.io/en/stable/)

## SimpleWebRTC

Although the open-source version of the SipmpleWebRTC project has been archived by &Yet, this is one of our favourite and simplest JavaScript 
WebRTC libraries. It is designed for Peer to Peer webRTC conferences. The signaling component is called [Signal Master](https://github.com/andyet/signalmaster) and can be found on github.
It also has the capability of screen sharing and file transfer. If the users are behind a NAT, you have to setup a TURN server for media traffic traversal.
In SimpleWebRTC, every user in the conference will be sending a video/audio stream for every other participant which creates a mesh network.
An advantage of Simple WebRTC is the simplicity that they provide. Not having open-source SDKs for mobile and the complexity that comes when the number of participants increases in a conference are disadvantage of SimpleWebRTC. 

- [Archived SimpleWebRTC Website with documentation](https://github.com/andyet/SimpleWebRTC/tree/gh-pages)
- [Archived SimpleWebRTC on Github](https://github.com/andyet/SimpleWebRTC)

