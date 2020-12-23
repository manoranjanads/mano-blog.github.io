---
title: "Most Common Errors in Jitsi Meet"
permalink: /webrtc/jitsi-meet-common-erros.html
excerpt: "How to Fix most common erros in Jitsi Meet"
header:
  overlay_image: /assets/images/webrtc/02-08-jitsimeet-with-ejabberd/02-08-jitsi-meet-with-ejabbered.jpg
  teaser: /assets/images/webrtc/02-08-jitsimeet-with-ejabberd/02-08-jitsi-meet-with-ejabbered.jpg
  overlay_filter: 0.5
last_modified_at: 2019-06-02T15:58:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---
**MeetrixIO** team is well experienced with WebRTC related technologies.
We provide **commercial support for Jitsi Meet**, Kurento, OpenVidu, BigBlue Button, Coturn Server and other webRTC related opensource projects.
{: .notice--info}

THIS GUIDE IS YET TO BE COMPLETED
{: .notice--warning}

## Jitsi Certificate Erros

```java
javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path validation failed: java.security.cert.CertPathValidatorException
```

```java
javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException
```

One of these erros might occur when you have a multi server Jitsi Installation or we have have done any reinstallatins. The problem could be the prosody certificates being not recognized by Jicofo, JVB, Jigasi or Jibri.

One thing you could try is disabling certificate verification by adding the following to `/etc/jitsi/videobridge/sip-communicator.properties` file

`org.jitsi.videobridge.xmpp.user.shard.DISABLE_CERTIFICATE_VERIFICATION=true`.

If that does not work, you can try to regenerate prosody certificates

```bash
prosodyctl cert generate jitsi.example.com
prosodyctl cert generate auth.jitsi.example.com
ln -sf /var/lib/prosody/auth.jitsi.corgos.de.crt /usr/local/share/ca-certificates/auth.jitsi.corgos.de.crt -f
update-ca-certificates -f
```

## Prosody Memeory Configuration Error in Jitsi Meet Installation

```lua
stack traceback:
	/usr/share/lua/5.2/prosody/util/cache.lua:66: in function 'set'
	/usr/lib/prosody/modules/muc/mod_muc.lua:185: in function 'track_room'
	/usr/lib/prosody/modules/muc/mod_muc.lua:213: in function </usr/lib/prosody/modules/muc/mod_muc.lua:200>
	(...tail calls...)
```

The value of `storage` in `/etc/prosody/conf.avail/your-domain.conf.lua` should be change depending on prosody version

Prosody trunk `storage = "null"` <br />
Prosody 0.9 `storage = "none"` <br />
Prosody 0.11 `storage = "memory"` <br />


## Jitsi Video Bridge Global IO pool Error

```java
Exception in thread "Global IO pool-31346" java.lang.NoClassDefFoundError: Could not initialize class com.fasterxml.jackson.databind.deser.std.JdkDeserializers
```

This is one of the latest issues that we have seen. Please use an older stable version of Jitsi Video Bridge for now

**Looking for commercial support for Jitsi Meet ?** Please contact us via [hello@meetrix.io](https://meetrix.io/contact-us)
{: .notice--warning}
