---
title: "How to Enable Screen Sharing in Jitsi Meet (Deprecated)"
permalink: /webrtc/jitsi/meet/screen_share.html
excerpt: "Jitsi Screen Share"
header:
  overlay_image: /assets/images/webrtc/02-03-how-to-enable-screen-share-in-jitsi-meet/02-03-how-to-enable-screen-share.jpg
  teaser: /assets/images/webrtc/02-03-how-to-enable-screen-share-in-jitsi-meet/02-03-how-to-enable-screen-share.jpg
  overlay_filter: 0.5
last_modified_at: 2018-10-07T14:07:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---
[Meetrix.IO](https://meetrix.io) provides installation, configuration and customization services for Jitsi Meet.
Get fully configured Jitsi Meet setup on your own server (starting from `$300`).
Please shoot an email to `hello@meetrix.io` for more information
{: .notice--success}

## Creating the Chrome Plugin
Sharing screen is an essential feature required in a video conference.
To enable screen sharing in Jitsi-Meet for google chrome, you have to build and deploy Jitsi's chrome extension which is called [Jidesha](https://github.com/jitsi/jidesha).
It’s can be done within few steps.

You just have to edit the,
```bash
 "matches": [
    "*://your.server.com/*"
]
```
in manifest.json for your jitsi server domain. Change the logos to yours and publish it in Chrome Webstore.

## Configure the jitsi meet

Then you can link published screen sharing extension changing the following line in
```bash
desktopSharingChromeDisabled: false
```
in Jitsi Meet config file. 

You have to add the chrome extension id to the config as well.
```bash
desktopSharingChromeExtId: '######################'
```
To enable screen sharing in Firefox you don’t any extension. Just change the following line in Jitsi Meet config.
```bash
desktopSharingFirefoxDisabled: false
```
That's it!