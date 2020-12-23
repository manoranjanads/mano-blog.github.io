---
title: "Jitsi Meet Octo"
permalink: /xmpp/jitsi_octo.html
excerpt: "jitsi meet octo configuration pubsub methonology"
header:
  overlay_image: /assets/images/xmpp/03-openfire-ssl/openfire_cert.jpg
  teaser: /assets/images/xmpp/03-openfire-ssl/openfire_cert.jpg
  overlay_filter: 0.5
last_modified_at: 2019-06-17T14:07:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---
## jitsi meet octo configuration
### jicofo configuration
```bash
org.jitsi.jicofo.BridgeSelector.BRIDGE_SELECTION_STRATEGY=RegionBasedBridgeSelectionStrategy
org.jitsi.focus.pubsub.ADDRESS=yourdomain
org.jitsi.jicofo.STATS_PUBSUB_NODE=sharedStatsNode
```
### jitsi-videobridge configuration
```bash
org.jitsi.videobridge.AUTHORIZED_SOURCE_REGEXP=focus@tourdomain.com/.*
org.jitsi.videobridge.octo.BIND_ADDRESS=private ip
org.jitsi.videobridge.octo.PUBLIC_ADDRESS=public ip
org.jitsi.videobridge.octo.BIND_PORT=4096 
org.jitsi.videobridge.REGION=us-west-2
org.jitsi.videobridge.ENABLE_STATISTICS=true
org.jitsi.videobridge.STATISTICS_TRANSPORT=pubsub
org.jitsi.videobridge.PUBSUB_SERVICE=yourdomain.com
org.jitsi.videobridge.PUBSUB_NODE=sharedStatsNode
```
### prosody configuration
```bash

admins = {
"jvb1.domain.com",
"jvb2.domain.com",
"jvb3.domain.com"
}

```
