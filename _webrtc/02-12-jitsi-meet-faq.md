---
title: "Jitsi Meet FAQs"
permalink: /webrtc/jitsi-meet-faq.html
excerpt: "Frequently Asked Questions About Jitsi Meet"
header:
  overlay_image: /assets/images/webrtc/02-08-jitsimeet-with-ejabberd/02-08-jitsi-meet-with-ejabbered.jpg
  teaser: /assets/images/webrtc/02-08-jitsimeet-with-ejabberd/02-08-jitsi-meet-with-ejabbered.jpg
  overlay_filter: 0.5
last_modified_at: 2019-09-02T15:58:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---
**MeetrixIO** team is well experienced with WebRTC related technologies.
We provide **commercial support for Jitsi Meet**, Kurento, OpenVidu, BigBlue Button, Coturn Server and other webRTC related open-source projects.
{: .notice--info}

THIS GUIDE IS YET TO BE COMPLETED
{: .notice--warning}

## What is the maximum number of participants on a meeting / per call ?

If all participants are actively participating, our recommendation is 35. If there is only one presenter and others are only participating, you can go up to 300 with OCTO.

## Can I run a Webinar with 1000 participants ?

Yes you can configure the video bridges in a way that they can cater 1000 users in a Webinar.

## Are Jitsi Meet Video Calls Encrypted ?

Yes. But not end-to-end encrypted. The calls are encrypted between the users browsers and video bridges (Because webRTC media is encrypted by default). The packets are decrypted and encrypted again in JVB.

## Are Jitsi Meet Video Calls End to End Encrypted ?

No. The packets are decrypted and encrypted again in JVB. You can take a look at [Jitsi Meet blog post experimental end-to-end encryption with insertable streams on chrome](https://jitsi.org/blog/e2ee/) and [webRTC Hacks Article about that](https://webrtchacks.com/true-end-to-end-encryption-with-webrtc-insertable-streams/).


## Is Jitsi Meet better than Zoom ?

No. If you compare Zoom desktop and Jitsi Meet. Zoom is better when it comes to features. But, if you want to embed a meeting solution to your application with more control, Jitsi Meet is better

## Is Jitsi Meet better than Google Meet ?

No. Even though Jitsi is built to compete with Google Meet, Google Meet is better than Jitsi Meet in many ways and it might stay as it is. WebRTC protocol was initially developed Engineers at Google and the Chrome browser is owned by Google. So, Goole has the competitive advantage over all most all factors.
But again, if you want to build a custom product, you have to come back to Jitsi or any other open source webRTC product.

## Is Jitsi Better than Kurento, Janus, FreeSwitch ect ?

If you are looking for a complete conferencing solution, Jitsi is the most mature product with the most active community in open-source WebRTC world. But, if you want to build something from scratch with lot more control over lower layers, you might have to go with Kurento, Janus or any other tool depending on the requirement. But, building your own product from scratch, might take a lot of time and effort.

**Looking for commercial support for Jitsi Meet ?** Please contact us via [hello@meetrix.io](https://meetrix.io/contact-us)
{: .notice--warning}
