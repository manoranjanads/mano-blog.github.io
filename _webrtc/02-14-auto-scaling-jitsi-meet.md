---
title: "Auto Scaling Jitsi Meet on AWS"
permalink: /webrtc/jitsi/jitsi-meet-auto-scaling.html
excerpt: "Auto Scale Jitsi Meet. Complete Auto Scaled Jitsi Meet Deployment by Meetrix"
header:
  overlay_image: /assets/images/webrtc/02-04-jitsi-meet-and-firewalls/02-04-jitsi-meet-and-firewall.jpg
  teaser: /assets/images/webrtc/02-04-jitsi-meet-and-firewalls/02-04-jitsi-meet-and-firewall.jpg
  overlay_filter: 0.5
last_modified_at: 2020-11-03T15:58:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---

**MeetrixIO** is a well experienced company with WebRTC related technologies.
We provide **commercial support for Jitsi Meet**, Kurento, OpenVidu, BigBlue Button, Coturn Server and other webRTC related opensource projects. If you looking for a Jitsi Meet deployment for thousands of simultaneous users, contact us via hello@meetrix.io. We can help you to AutoScale Jitsi VideoBride and Jibri clusters.
{: .notice--info}

If you are planing to run Jitsi Meet at a scale to cater thousands of users you should have

1. A Multi shard deployment with HA proxy
2. Load balanced video bridges in each shard ( with auto scaling for cost optimization)
3. A pool of Jibris for recording or streaming ( with auto scaling for cost optimization)
4. A comprehensive monitoring system to monitor the system load and conference metrics such as number of conferences and users
5. A load testing mechanism to test your infrastructure to make sure that it is production ready.

## A Multi shard deployment with HA proxy

If you are planing to cater more than thousand users with Jitsi Meet, you should consider distributing the load between multiple shards of Jitsi Meet deployments rather than using a single shard because, the XMPP server (Prosody by default) and Jicofo could be a bottleneck when you increase the number of Jitsi Video Bridge instances in a single shard.
This can be accomplished by deploying multiple shards of Jitsi Meet behind HAproxy using URL based routing and stick tables. This setup ensures that a given meeting room is always hosted on the same shard.
For an example, say that you have two Jitsi Meet shards behind HA proxy. If user `A` joins conference room `myconference1` which will be created on `shard1`, all the other users who are going to join `myconference1` will be proxied to `shard1`

{% include figure image_path="/assets/images/webrtc/02-14-auto-scaling-jitsi-meet/02-14-jitsi-meet-deployment-diagram-01.png" alt="Jitsi Meet Auto Scaling" caption="Auto Scaling Jitsi Meet" %}

## Load balanced Video Bridges in each shard with Auto Scaling

If you are running hundreds of conferences in a single shard, they should be load balanced between multiple Jitsi Video Bridges. Once you configure multiple video bridges in a single shard this load balancing is done by Jicof (Jitsi conference focus).
If you are hosting on the cloud (say on AWS), these video bridges can be auto scaled to handle dynamic loads in a cost effective way.
For and example, in the mid-day there might be 500 concurrent users in the shard but in the evening there might be only 100 concurrent users. At night there will be no users. To optimize the cost, the number of video bridges should be automatically kept at the optimal value. To cater 500 users we might need 10 video bridges, to cater 100 users we might need three bridges and at night, we might only need two video bridges.
This can be done by AWS auto-scaling groups, SQS, SNS and CloudWatch. And the autoscaling parameters can be fine-tuned for each use-case.

## A pool of Jibris for recording or streaming

Jibri is the recording and streaming service for Jitsi. A single Jibri can only record a single conference at a time. Therefore, if you want to record 100 conferences at a time, there should be 100 running Jibri servers. Well, running a pool of Jibris to cater the maximum number of simultaneous conferences that you expect to record will get you a huge AWS bill.
To solve this problem, we maintain a constant number of free Jibris (say 3) at any given time. When someone starts to record a meeting, one of the available Jibris will be allocated to record that conference and another Jibri will spin up to join the free Jibri pool. Once the recording is done, the Jibri can upload the recording to a storage service like S3 and return to the free Jibri pool. If there are more than 3 free Jibris, the autoscaling mechanism will shutdown one of the Jibris.
The number of constant Jibris should be optimized according to the use-case.

## A comprehensive monitoring system

Monitoring is crucial for any system. When it comes to Jitsi, , we should also monitor number of users, conferences, Video bridges, Jibris in addition to the usual system metrics such as CPU, Memory, Network and IO. If you are on AWS, CloudWatch can be used as a monitoring system.

{% include figure image_path="/assets/images/webrtc/02-14-auto-scaling-jitsi-meet/02-14-cloudwatch-monitoring-01.png" alt="Jitsi Meet Auto Scaling" caption="Jitsi Meet Monitoring with Cloudwatch" %}

If you have an on-premise deployment, a monitoring system based with ELK can be used

{% include figure image_path="/assets/images/webrtc/02-14-auto-scaling-jitsi-meet/02-14-elk-monitoring-01.png" alt="Jitsi Meet Monitoring with ELK" caption="Jitsi Meet Monitoring with Elastic Search" %}

Meetrix offeres number of Jitsi Meet deployment solutions that suit for both cloud (AWS) and on-premise deployments.

## A load testing mechanism to test your infrastructure

To make sure everything is properly configured and your infrastructure can cater the desired number of concurrent users, you have to perform a load test before you go live. This can be done via Jitsi Torture.
A Torture setup can flood fake conference users in to meetings and generate a load on your infrastructure.

{% include figure image_path="/assets/images/webrtc/02-12-load-testing-jitsi-meet/02-10-jitsi-meet-dummy-users.png" alt="Jitsi Meet Load Testing" caption="Jitsi Meet Load Testing with Torture" %}

## Looking for professionals support ?

Meetrix.IO is well experienced team with Jitsi Meet and WebRTC. If you are looking for a Jitsi Meet deployment to cater thousands of concurrent users, please contact us via `hello@meetrix.io`.


**Looking for commercial support for Jitsi Meet ?** Please contact us via [hello@meetrix.io](https://meetrix.io/contact-us)
{: .notice--warning}
