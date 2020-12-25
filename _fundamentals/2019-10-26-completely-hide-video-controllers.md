---
title: "How to hide HTML5 video controls completely"
permalink: /general/hide-video-controls-completely.html
excerpt: "A simple CSS trick to completely hide html5 video controls"
header:
  overlay_image: /assets/images/general/2019-10-26-completely-hide-video-controllers/header-image.jpg
  teaser: /assets/images/general/2019-10-26-completely-hide-video-controllers/header-image.jpg
  overlay_filter: 0.4
last_modified_at: 2019-10-26T07:36:19.859Z
toc: true
# categories:
#   - css
#   - html
# tags:
#   - html
#   - css
#   - video
#   - controls
---

When we use HTML5 video elements to build an application with Kurento, OpenVidu, Jitsi or any other RTC libraries, we want to completely hide the video controls.

We can hide the controls by **not adding** the `controls` attribute to the video element.

```html
<video autoplay playsinline></video>
```

Even without `controls` attribute on the elements the user can view the controls section by right-clicking on the video and enabling the `show controls`.

{% include figure image_path="assets/images/general/2019-10-26-completely-hide-video-controllers/show-controls.png" alt="Enabling Controls" caption="User can enable the controls" %}

To avoid this scenario, we can apply a single CSS attribute to make the video element never the target of pointer events.

```html
<video autoplay playsinline style="pointer-events: none;"></video>
```

The above CSS attribute makes the video element not listen to pointer events. Therefore the user cant enables the controls by right-click menu.