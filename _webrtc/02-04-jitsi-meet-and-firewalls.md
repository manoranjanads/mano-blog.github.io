---
title: "Jitsi Meet and Firewalls"
permalink: /webrtc/jitsi/jitsi-meet-and-firewalls.html
excerpt: "Setting up Jitsi Meet Behind Corporate Firewalls"
header:
  overlay_image: /assets/images/webrtc/02-04-jitsi-meet-and-firewalls/02-04-jitsi-meet-and-firewall.jpg
  teaser: /assets/images/webrtc/02-04-jitsi-meet-and-firewalls/02-04-jitsi-meet-and-firewall.jpg
  overlay_filter: 0.5
last_modified_at: 2019-06-02T15:58:49-04:00
redirect_from:
  - /theme-setup/
toc: true
author: Mano
---

**MeetrixIO** team is well experienced with WebRTC related technologies.
We provide **commercial support for Jitsi Meet**, Kurento, OpenVidu, BigBlue Button, Coturn Server and other webRTC related open-source projects.
{: .notice--info}

## How Jitsi Video Bridge Handles WebRTC Traffic

Jitsi Meet has the ability to handle webRTC traffic over UDP as TCP.

`UDP, ports 10000 - 20000` is the default configuration for Media Traffic in Jitsi Video Bridge(JVB). If your firewall has not restricted these UDP port default Jitsi Meet setup would work without any issue.

In case if your firewall has restricted UDP traffic, the video bridge will try to initialize a TCP connection between the client behind the firewall and the Video Bridge. If you have setup the Jitsi Video Bridge on the same server as Jitsi Meet, Prosody and Jicoco, Jitsi Video Bridge (JVB) will try to use `port 4443` over TCP for webRTC Media traffic. Actually Jitsi Video Bridge is configured (by default) to use `port 443` for TCP and `port 4443` is the fallback port. But if you have all the jitsi meet components on the same server, the web server which serves the frontend will occupy the `port 443`.

Some strict restricted firewalls are only allowing TCP 80 and 443 for incoming/outgoing traffic. So the default Jitsi Meet installation will not work in this scenario.

As a solution what you can do is setting up Jitsi Video Bridge (JVB) in a separate box. This will free up `port 443` for Jitsi Video Bridge (If you install Jitsi Video Bridge on the Same server as other Jitsi Meet Components, Java Server or Nginx which is used to server Jitsi Meet frontend will occupy port 443).

If even this does not work, then you can follow [Settingup a Turn Server for Jitsi Meet](https://meetrix.io/blog/webrtc/jitsi/setting-up-a-turn-server-for-jitsi-meet.html) guide to setup a Turn Server with `tls` support to relay media traffic.

**Looking for commercial support for Jitsi Meet ?** Please contact us via [hello@meetrix.io](https://meetrix.io/contact-us)
{: .notice--warning}