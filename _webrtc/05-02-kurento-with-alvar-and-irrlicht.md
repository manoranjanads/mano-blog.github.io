---
title: "How to Setup Kurento to work with ALVAR and Irrlicht"
permalink: /webrtc/kurento/kurento-with-alvar-and-irrlicht.html
excerpt: "WebRTC based Machine Vision"
header:
  overlay_image: /assets/images/webrtc/05-02-kurento-with-alvar-and-irrlicht/05-02-kurento-with-alvar-and-irrlicht.jpg
  teaser: /assets/images/webrtc/05-02-kurento-with-alvar-and-irrlicht/05-02-kurento-with-alvar-and-irrlicht.jpg
  overlay_filter: 0.5
last_modified_at: 2018-10-07T14:07:49-04:00
redirect_from:
  - /theme-setup/
toc: true
author: Mano
---
[Meetrix.IO](https://meetrix.io) provides installation, configuration and customization services for Kurento.
Get fully configured Kurento setup on your own server (starting from `$250`).
Please shoot an email to `hello@meetrix.io` for more information
{: .notice--success}

ALVAR Augmented Reality library and has not been maintained for years. 
How ever there is an interesting demo with ALVAR and Kurento which can render  3d models on detected ALVAR Markers of real-time video streams.
In this post, we are bringing back this example to live, after years. 


Installing other ALVAR Dependencies
--
- `sudo apt-get install libopencv-dev freeglut3-dev libopenscenegraph-dev`


Compiling ALVAR
--
First we need ALVA library, I have foked a mirror of ALVAR and changed it to compile with gcc 5.0.
You can find the git repo [here](https://github.com/meetrix/alvar).
- Clone the repo `git clone https://github.com/meetrix/alvar.git`
- Run the script inside `alvar` directory `cd ./build && chmod +x generate*.sh && ./generate_gcc5.sh`
- `cd ./build/build_gcc5_release` then `make`
- Install with `make install`

`src/server/interface/meetrixremotesupportalpha.MeetrixRemoteSupportAlpha.kmd.json`
- Create the AR Process
`MeetrixRemoteSupportAlphaOpenCVImpl.hpp`

