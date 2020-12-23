---
title: "Jitsi Meet GeoGraphically"
permalink: /xmpp/jitsi_geograph.html
excerpt: "jitsi meet serve base on the user geolocation usong route53"
header:
  overlay_image: /assets/images/xmpp/03-openfire-ssl/openfire_cert.jpg
  teaser: /assets/images/xmpp/03-openfire-ssl/openfire_cert.jpg
  overlay_filter: 0.5
last_modified_at: 2019-06-17T14:07:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---
## we can server jitsi meet web front according to the user geographical area using aws route53. if we want client communicate with nearest jitsi-videobridge then we have to load different meet configuration with userRegion. so we can use this methonoly for load diffent configurations with jitsi-meet.

1. you can flow this blog for jitsi meet s3 configuration
2. change jitsi-meet configutaion with userregion
```
config.deploymentInfo: {
      region: 'eu-central-1',
      userRegion: 'eu-central-1',
  },
```
1. create multiple s3 install according to geographical area and change the configuration above manner
2. use cloudfront distrubution to every s3 bucket with ssl
3. make cloudfront alise with you domain and keep remember change the routing policy choosing Geolocation
