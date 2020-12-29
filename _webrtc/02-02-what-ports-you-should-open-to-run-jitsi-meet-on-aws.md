---
title: "What Ports You Should Open to Run Jitsi Meet"
permalink: /webrtc/jitsi/meet/what-port-your-should-open.html
excerpt: "Jitsi on AWS"
header:
  overlay_image: /assets/images/webrtc/02-02-what-ports-you-should-open-to-run-jitsi-meet-on-aws/02-02-what-ports-you-should-open-to-run-jitsi-meet-on-aws.jpg
  teaser: /assets/images/webrtc/02-02-what-ports-you-should-open-to-run-jitsi-meet-on-aws/02-02-what-ports-you-should-open-to-run-jitsi-meet-on-aws.jpg
  overlay_filter: 0.5
last_modified_at: 2018-10-19T15:58:49-04:00
redirect_from:
  - /theme-setup/
toc: true
author: Mano
---

[Meetrix.IO](https://meetrix.io) provides installation, configuration and customization services for Jitsi Meet.
Get fully configured Jitsi Meet setup on your own server (starting from `$300`).
Please shoot an email to `hello@meetrix.io` for more information
{: .notice--success}

You have to open following on EC2 Security groups to run jitsi-meet (single server installation) properly

Here is the list of the tools and libraries that we are going to discuss and a comparision about their features.

|    Description                                |   Protocol    | Port      |
|:---:                                          |---:           |---:       |
| Media Traffic                                 | UDP           | 10000      |
| HTTP/Bosh/WebSocket                           | TCP           | 80        |
| HTTPS/Bosh/Secure Websocket                   | TCP           | 443       |
| Media Traffic in Restricted Firewalls         | TCP           | 4443      |
| SSH (optional)                                | TCP           | 22        |
{: rules="groups"}
---

You have to consider opening 5222 and 5347 ports for TCP as well if you have configured Jitsi components in multiple boxes.