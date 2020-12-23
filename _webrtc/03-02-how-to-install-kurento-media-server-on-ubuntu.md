---
title: "How to Install Kurento Media Server on Ubuntu"
permalink: /webrtc/kurento.html
excerpt: "on Ubuntu 16.04 LTS (Xenial Xerus)"
header:
  overlay_image: /assets/images/webrtc/03-02-how-to-install-kurento-media-server-on-ubuntu/03-02-how-to-install-kurento-media-server-to-ubuntu.jpg
  teaser: /assets/images/webrtc/03-02-how-to-install-kurento-media-server-on-ubuntu/03-02-how-to-install-kurento-media-server-to-ubuntu.jpg
  overlay_filter: 0.7
last_modified_at: 2019-07-26T15:59:07-04:00
toc: true
author: Jay
---
**MeetrixIO** team is well experienced with WebRTC related technologies.
We provide **commercial support** for Jitsi Meet, Kurento, OpenVidu, BigBlue Button, Coturn Server and other webRTC related opensource projects.
{: .notice--info}

## Installation

Following one shot commands set will install Kurento Media Server (KMS) on your Ubuntu 16.04 box

```bash
DISTRO="xenial" &&\
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5AFA7A83 &&\
sudo tee "/etc/apt/sources.list.d/kurento.list" >/dev/null <<EOF
deb [arch=amd64] http://ubuntu.openvidu.io/6.7.1 $DISTRO kms6
EOF
sudo apt-get -y update &&\
sudo apt-get -y install kurento-media-server
```

## Start/Stop KMS

Use following commands to start and stop KMS

```bash
sudo service kurento-media-server start
sudo service kurento-media-server stop
```

## Add Startup Script

```json
export LC_CTYPE=en_US.UTF-8
export LC_ALL=en_US.UTF-8
```

Now add startup script

```bash
update-rc.d kurento-media-server defaults
```

Your Kurento Media Server is Ready.

**Looking for commercial support ?** Please contact us via [hello@meetrix.io](href="mailto:hello@meetrix.io?Subject=Commercial%20Support%20for%20Kurento%20Meet")
{: .notice--warning}
