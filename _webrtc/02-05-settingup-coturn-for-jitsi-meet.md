---
title: "Settingup a Turn Server for Jitsi Meet"
permalink: /webrtc/jitsi/setting-up-a-turn-server-for-jitsi-meet.html
excerpt: "To work behind restricted firewalls."
header:
  overlay_image: /assets/images/webrtc/02-05-settingup-coturn-for-jitsi-meet/02-05-settingup-coturn-for-jitsi-meet.jpg
  teaser: /assets/images/webrtc/02-05-settingup-coturn-for-jitsi-meet/02-05-settingup-coturn-for-jitsi-meet.jpg
  overlay_filter: 0.5
last_modified_at: 2019-07-14T15:58:49-04:00
redirect_from:
  - /theme-setup/
toc: true
author: Mano
---

**MeetrixIO** team is well experienced with WebRTC related technologies.
We provide **commercial support for Jitsi Meet**, Kurento, OpenVidu, BigBlue Button, Coturn Server and other webRTC related opensource projects.
{: .notice--info}

Although Jitsi Meet is fairly easy to setup, you might find some issues when trying to setting it up behind corporate firewalls. Please follow our
 [Jitsi Meet and Firewalls](https://meetrix.io/blog/webrtc/jitsi/jitsi-meet-and-firewalls.html)
post first to get an understanding on how Jitsi Meet works with ports.

If you have tried everything to get Jitsi Meet work behind your firewall and nothing works, you can try to setup a Turn server. In this article, we will seup Coturn to work with Jitsi Meet.

## Setting up Coturn

You can follow our [How to Setup Coturn](https://meetrix.io/blog/webrtc/coturn/installation.html) article and setup coturn with `Time-Limited Credentials Mechanism` and  `with SSL`. You would have a coturn configuration file similar to the following one.

```bash
server-name=coturn.meetrix.io
realm=coturn.meetrix.io
cert=/etc/letsencrypt/live/coturn.meetrix.io/cert.pem
pkey=/etc/letsencrypt/live/coturn.meetrix.io/privkey.pem
fingerprint
listening-ip=0.0.0.0
external-ip=<EXTERNAL_IP>/<INTERNAL_IP> #or just the external ip
listening-port=443
min-port=10000
max-port=20000
log-file=/var/log/turnserver.log
verbose

static-auth-secret=<YOUR_SECRET>
```

## Configuring Prosody

First we need to download and install `mod_turncredentials.lua` to prosody

```bash
cd /tmp && \
wget https://raw.githubusercontent.com/otalk/mod_turncredentials/master/mod_turncredentials.lua && \
sudo cp mod_turncredentials.lua /usr/lib/prosody/modules/
```

Then in your prosody config,

```lua

turncredentials_secret = "<YOUR_TURN_SECRET>";
turncredentials_port = 443;
turncredentials_ttl = 86400;
turncredentials = {
    { type = "stun", host = "coturn.meetrix.io" },
    { type = "turn", host = "coturn.meetrix.io", port = 443},
    { type = "turns", host = "coturn.meetrix.io", port = 443, transport = "tcp" }
}

```

After that, we have to enable `mode_turncredentials` in our virtual host. Like this

```lua
modules_enabled = {
        "bosh";
        "pubsub";
        "ping"; -- Enable mod_ping
        "turncredentials";
    }
```

once you are done, restart the prosody server.

## Jitsi Meet Config

For the JVB, there are two use cases of a Turn Server.
Hence there are two places in Jitsi Meet configuration where we have to configure `useStunTurn`

``` js
    p2p: {
        enabled: true,
        preferH264: true,
        useStunTurn: true, // Using Turn for p2p connections
        stunServers: [
            { urls: "stun:stun.l.google.com:19302" },
            { urls: "stun:stun1.l.google.com:19302" },
            { urls: "stun:stun2.l.google.com:19302" }
        ]
    },
    useStunTurn: true, // Using Turn Server with JVB
```

### Using Turn for p2p connections

If the video bridge fails to establish p2p connections between two participants, we can establish the p2p connection through the Turn Server. To do this, we need to set `useStunTurn: true` in `p2p` settings of Jitsi Meet configurations.

### Using Turn Server with JVB

By setting `useStunTurn: true` and setting `org.jitsi.videobridge.DISABLE_TCP_HARVESTER=true` on JVB (using `sip-communicator.properties` file), we can turn off the TCP Harvester of JVB and use the Turn Server for TCP connections. With this method, JVB will only be uing UDP. If a participant fails to establish a UDP connection with the bridge, TURN server will establish a TCP connection with the participant and then will relay the media traffic over UDP to the bridge.

Special thanks : [Damian Minkov](https://community.jitsi.org/u/damencho) of Jitsi Team.

**Looking for commercial support for Jitsi Meet ?** Please contact us via [hello@meetrix.io](href="mailto:hello@meetrix.io?Subject=Commercial%20Support%20for%20Coturn")
{: .notice--warning}
