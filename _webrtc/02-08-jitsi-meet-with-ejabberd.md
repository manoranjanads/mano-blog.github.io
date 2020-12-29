---
title: "Jitsi Meet with Ejabberd"
permalink: /webrtc/jitsi-meet-with-ejabberd.html
excerpt: "Jitsi Meet with Openfire"
header:
  overlay_image: /assets/images/webrtc/02-08-jitsimeet-with-ejabberd/02-08-jitsi-meet-with-ejabbered.jpg
  teaser: /assets/images/webrtc/02-08-jitsimeet-with-ejabberd/02-08-jitsi-meet-with-ejabbered.jpg
  overlay_filter: 0.5
last_modified_at: 2019-06-02T15:58:49-04:00
redirect_from:
  - /theme-setup/
toc: true
author: Mano
---
**MeetrixIO** team is well experienced with WebRTC related technologies.
We provide **commercial support for Jitsi Meet**, Kurento, OpenVidu, BigBlue Button, Coturn Server and other webRTC related opensource projects.
{: .notice--info}

THIS GUIDE IS YET TO BE COMPLETED
{: .notice--warning}

## Ejabberd Settings

### Creating a Componenet for Jicofo

```yml
  -
    port: 5280
    ip: "::"
    module: ejabberd_http
    request_handlers:
      "/ws": ejabberd_http_ws
      "/bosh": mod_bosh
      "/api": mod_http_api
    ##  "/pub/archive": mod_http_fileserver
    web_admin: true
    ## register: true
    ## captcha: true
    tls: true
    protocol_options: 'TLS_OPTIONS'
```

```yml
  -
    port: 5275
    ip: "::"
    module: ejabberd_service
    access: all
    shaper_rule: fast
  ##   ip: "127.0.0.1"
  ##   privilege_access:
  ##      roster: "both"
  ##      message: "outgoing"
  ##      presence: "roster"
  ##   delegations:
  ##      "urn:xmpp:mam:1":
  ##        filtering: ["node"]
  ##      "http://jabber.org/protocol/pubsub":
  ##        filtering: []
    hosts:
      "jitsi-videobridge.<YOUR_XMPP_DOMAIN>":
        password: "<JVB_SECRET>"
      "focus.<YOUR_XMPP_DOMAIN>":
        password: "<JICOFO_COMPONENT_SECRET>"
```

```yml
acl:
  ##
  ## The 'admin' ACL grants administrative privileges to XMPP accounts.
  ## You can put here as many accounts as you want.
  ##
  admin:
     user:
       - "admin@ejabberd.meetrix.io"
       - "focus@ejabberd.meetrix.io"
```

### Creating a User for Jicofo

```bash
ejabberdctl register focus ejabberd.meetrix.io <JICOFO_USER_PASSWORD>
```

## Jicofo Setup

in `/etc/jitsi/jicofo/sip-communicator.properties` add following line to accept the Openfire Certificate

```java
org.jitsi.jicofo.ALWAYS_TRUST_MODE_ENABLED=true
```

in `/etc/jitsi/jicofo/config`

```java
# Jitsi Conference Focus settings
# sets the host name of the XMPP server
JICOFO_HOST=<YOUR_XMPP_HOST>

# sets the XMPP domain (default: none)
JICOFO_HOSTNAME=<YOUR_XMPP_DOMAIN>

# sets the secret used to authenticate as an XMPP component
JICOFO_SECRET=<JICOFO_COMPONENT_SECRET>

# sets the port to use for the XMPP component connection
JICOFO_PORT=5275

# sets the XMPP domain name to use for XMPP user logins
JICOFO_AUTH_DOMAIN=<YOUR_XMPP_DOMAIN>

# sets the username to use for XMPP user logins
JICOFO_AUTH_USER=focus

# sets the password to use for XMPP user logins
JICOFO_AUTH_PASSWORD=<JICOFO_USER_PASSWORD>

# extra options to pass to the jicofo daemon
JICOFO_OPTS=""

# adds java system props that are passed to jicofo (default are for home and logging config file)
JAVA_SYS_PROPS="-Dnet.java.sip.communicator.SC_HOME_DIR_LOCATION=/etc/jitsi -Dnet.java.sip.communicator.SC_HOME_DIR_NAME=jicofo -Dnet.java.sip.communicator.SC_LOG_DIR_LOCATION=/var/log/jitsi -Djava.util.logging.config.file=/etc/jitsi/jicofo/logging.properties"
```

## Jitsi Video Bridge Setup

in `/etc/jitsi/videobridge/sip-communicator.properties`

```java
org.jitsi.videobridge.AUTHORIZED_SOURCE_REGEXP=focus@<YOUR_XMPP_DOMAIN>/.*
```

in `/etc/jitsi/videobridge/config`

```bash
# sets the XMPP domain (default: none)
JVB_HOSTNAME=<YOUR_XMPP_DOMAIN>

# sets the hostname of the XMPP server (default: domain if set, localhost otherwise)
JVB_HOST=<YOUR_JVB_HOST>

# sets the port of the XMPP server (default: 5275)
JVB_PORT=

# sets the shared secret used to authenticate to the XMPP server
JVB_SECRET=<YOUR_JVB_SECRET>

# extra options to pass to the JVB daemon
JVB_OPTS=""


# adds java system props that are passed to jvb (default are for home and logging config file)
JAVA_SYS_PROPS="-Dnet.java.sip.communicator.SC_HOME_DIR_LOCATION=/etc/jitsi -Dnet.java.sip.communicator.SC_HOME_DIR_NAME=videobridge -Dnet.java.sip.communicator.SC_LOG_DIR_LOCATION=/var/log/jitsi -Djava.util.logging.config.file=/etc/jitsi/videobridge/logging.properties"
```

**Looking for commercial support for Jitsi Meet ?** Please contact us via [hello@meetrix.io](https://meetrix.io/contact-us)
{: .notice--warning}
