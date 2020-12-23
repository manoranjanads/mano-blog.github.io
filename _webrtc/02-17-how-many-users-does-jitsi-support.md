---
title: "How many Users and Conferences can Jitsi support on AWS"
permalink: /webrtc/jitsi/how-many-users-does-jitsi-support.html
excerpt: "While Jitsi supports unlimited use cases, it's important to know how many conferences and how many users per conference Jitsi handles on AWS infrastructure"
header:
  overlay_image: /assets/images/webrtc/02-04-jitsi-meet-and-firewalls/02-04-jitsi-meet-and-firewall.jpg
  teaser: /assets/images/webrtc/02-04-jitsi-meet-and-firewalls/02-04-jitsi-meet-and-firewall.jpg
  overlay_filter: 0.5
last_modified_at: 2020-11-16T15:58:49-04:00
redirect_from:
  - /theme-setup/
toc: true
author: Achala 
---
**MeetrixIO** is a well experienced company with WebRTC related technologies.
We provide **commercial support for Jitsi Meet**, Kurento, OpenVidu, BigBlue Button, Coturn Server and other webRTC related opensource projects.
{: .notice--info}


## How many conferences and users does Jitsi support?

Meetrix is a Jitsi solutions provider with over 6 years of experience in WebRTC. Our solutions engineers include front end, middle-ware and dev. ops teams who are experts in customizing Jitsi for a diverse range of business needs.

Before you begin your journey with Jitsi based online conferencing solutions you would have some basic statistics questions in relation to Jitsi's performance matrices.

## How many users can one Jitsi conference support?

As Jitsi relays video streams from one user to all other users in multiple bit rates according to available bandwidths and network speeds, most performance related issues would occur on the user machines rather than the Jitsi video bridge it self.

For safety's sake we recommend 30 users per conference with 10-15 on video. If the users machines are Core i5/i7 and the network infrastructure has good specifications Jitsi meetings can accommodate ~30 video users with 75 users all total (balance users with audio only).
We can also configure Jitsi webconference mode where 100-300 users can easily participate and listen and see one presenter is also possible.

## How many concurrent meetings and concurrent users can Jitsi handle?

Jitsi video bridges and Jibri video stream recorders can be auto-scaled on demand user Kubernetes or AWS cloudwatch services and geographically optimized using Octo upon a HA-proxy load balanced multi-shard setup. This can theoretically support unlimited user conferences. But most business would not require very large clusters and even one shard could be well enough for small or medium enterprises.

## What servers should Jitsi be setup on?

On AWS we recommend 8 core or 16 core server instances with 8 or 16GB of RAM for JVBs. T3 Large, C5XLarge, C52XLarge of even high compute powered servers need to configured case by case considering performance and cost.

## What performance matrices should be considered?

CPU and RAM on servers as well the in/out bandwidth quality and limitations would impact your Jitsi service quality. Load testing your rigs for highly available on-demand services is also recommended.

**Looking for commercial support for Jitsi Meet ?** Please contact us via [hello@meetrix.io](https://meetrix.io/contact-us)
{: .notice--warning}