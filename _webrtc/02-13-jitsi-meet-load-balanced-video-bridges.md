---
title: "Jitsi Meet Load Balancing"
permalink: /webrtc/jitsi/jitsi-meet-load-balancing.html
excerpt: "Setup multi server, load balanced videobriges"
header:
  overlay_image: /assets/images/webrtc/02-04-jitsi-meet-and-firewalls/02-04-jitsi-meet-and-firewall.jpg
  teaser: /assets/images/webrtc/02-04-jitsi-meet-and-firewalls/02-04-jitsi-meet-and-firewall.jpg
  overlay_filter: 0.5
last_modified_at: 2020-10-19T15:58:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---

**MeetrixIO** is a well experienced company with WebRTC related technologies.
We provide **commercial support for Jitsi Meet**, Kurento, OpenVidu, BigBlue Button, Coturn Server and other webRTC related opensource projects.
{: .notice--info}

One of the amazing features in Jitsi Meet is the inbuilt horizontal scalability.
When you want to cater large number of concurrent users, you can spin up multiple video bridges to handle the load.
If we setup mulple video bridges and connect them to the same shard, Jicofo, the conference manager selects the least loaded Videobridge for the next new conference.

## Run Prosody on all interfaces

By default prosody runs only on local interface (127.0.0.0), to allow the xmpp connections from external servers, we have to run prosody on all interfaces. To do that, add the following line at the beginning of `/etc/prosody/prosody.cfg.lua`

```lua
component_interface = "0.0.0.0"
```

## Allow inbound traffic for port 5222

Open inbound traffic from JVB server on port `5222` (TCP) of Prosody server. But DO NOT open this publicly.

## Install JVB on a seperate server

Install a Jitsi Video Bridge on a different server.

## Copy JVB configurations from Jitsi Meet server

Replace `/etc/jitsi/videbridge/config` and `/etc/jitsi/video-bridge/sip-communicator.properties` of JVB server with the same files from the original Jitsi Meet server

## Update JVB config file

In `/etc/jitsi/videbridge/config` set the `XMPP_HOST` to the ip address/domain of the prosody server

```java
# Jitsi Videobridge settings

# sets the XMPP domain (default: none)
JVB_HOSTNAME=example.com

# sets the hostname of the XMPP server (default: domain if set, localhost otherwise)
JVB_HOST=<XMPP_HOST>

# sets the port of the XMPP server (default: 5275)
JVB_PORT=5347

# sets the shared secret used to authenticate to the XMPP server
JVB_SECRET=<JVB_SECRET>

# extra options to pass to the JVB daemon
JVB_OPTS="--apis=rest,xmpp"


# adds java system props that are passed to jvb (default are for home and logging config file)
JAVA_SYS_PROPS="-Dnet.java.sip.communicator.SC_HOME_DIR_LOCATION=/etc/jitsi -Dnet.java.sip.communicator.SC_HOME_DIR_NAME=videobridge -Dnet.java.sip.communicator.SC_LOG_DIR_LOCATION=/var/log/jitsi -Djava.util.logging.config.file=/etc/jitsi/videobridge/logging.properties"
```

In `/etc/jitsi/video-bridge/sip-communicator.properties` file update the following properties

1. `<XMPP_HOST>`: The ip address of the prosody server. Better if you can use the private IP address if that can be accessed from JVB server.
2. `<JVB_NICKNAME>`: This should be a unique string used by Jicofo to identify each JVB

```java
org.ice4j.ice.harvest.DISABLE_AWS_HARVESTER=true
org.ice4j.ice.harvest.STUN_MAPPING_HARVESTER_ADDRESSES=meet-jit-si-turnrelay.jitsi.net:443
org.jitsi.videobridge.ENABLE_STATISTICS=true
org.jitsi.videobridge.STATISTICS_TRANSPORT=muc
org.jitsi.videobridge.xmpp.user.shard.HOSTNAME=<XMPP_HOST>
org.jitsi.videobridge.xmpp.user.shard.DOMAIN=auth.example.com
org.jitsi.videobridge.xmpp.user.shard.USERNAME=jvb
org.jitsi.videobridge.xmpp.user.shard.PASSWORD=<JVB_SECRET>
org.jitsi.videobridge.xmpp.user.shard.MUC_JIDS=JvbBrewery@internal.auth.example.com
org.jitsi.videobridge.xmpp.user.shard.MUC_NICKNAME=<JVB_NICKNAME>
org.jitsi.videobridge.xmpp.user.shard.DISABLE_CERTIFICATE_VERIFICATION=true
```

You can add the following line at the beginning of `/usr/share/jitsi/jvb/jvb.sh` to generate a unique nickname for the JVB at each startup. This might be useful if you are using an auto-scaling mechanism.

```sh
sed -i "s/org.jitsi.videobridge.xmpp.user.shard.MUC_NICKNAME=.*/org.jitsi.videobridge.xmpp.user.shard.MUC_NICKNAME=$(cat /proc/sys/kernel/random/uuid)/g" /etc/jitsi/videobridge/sip-communicator.properties
```

**Looking for commercial support for Jitsi Meet ?** Please contact us via [hello@meetrix.io](https://meetrix.io/contact-us)
{: .notice--warning}
