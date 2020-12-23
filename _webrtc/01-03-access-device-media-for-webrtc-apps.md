---
title: "Access device media for WebRTC Applications"
permalink: /webrtc/access-device-media-for-webrtc-apps.html
excerpt: "Plugin-free media access"
header:
  overlay_image: /assets/images/webrtc/01-03--access-device-media-for-webrtc-apps/overlay.png
  teaser: /assets/images/webrtc/01-03--access-device-media-for-webrtc-apps/access-device-media.jpg
  overlay_filter: 0.5
last_modified_at: 2019-06-16T15:58:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---

**Accessing media devices is an essential part of the WebRTC**

With the introduction of HTML5, modern browsers expose various interfaces to access device hardware without any external plugin support like Flash, Silverlight, etc.

## mediaDevices API

`mediaDevices` API is the interface that helps web applications to access device camera, microphone, and screen. that API can be referred by:
```js 
  const mediaDevices = navigator.mediaDevices;
```
The returned `mediaDevices`  object properties are static properties. hence they can be accessed directly like,
```js
  mediaDevices.staticMethod().then( /* do something with result */ );
  let devicesInfo = mediaDevices.staticProperty;
```

## There are 4 Methods in getUserMedia Object

### 1. enumerateDevices()

This returns Promise which resolves an array of media input/output devices available on the system.
```js
  navigator.mediaDevices.enumerateDevices().then(/* array of media devices */);
```
```js
  // resolved array of devices
  [
    {
      deviceId: "default",
      groupId: "c129c8f2cba601134ad686a3306f0d7848d01c1b3fcf049d2d2fafea70785002",
      kind: "audioinput",
      label: ""
    },
    {
      deviceId: "7c783ef37f481e307014add32a3dee2a2df517bd2efc51eddd32214537eef609",
      groupId: "e459c8f2cba601134ad686a3306f0d7848d01c1b3fcf049d2d2fafea707855678",
      kind: "videoinput",
      label: ""
    },
  ]
```
These array of device information can be used for selecting the devices, which want to share using webRTC.


### 2. getSupportedConstraints()

This returns an object which includes the supported constraints supported by the browser.
```js
  const supportedConstraints = navigator.mediaDevices.getSupportedConstraints();
```
```js
  // returned object
  supportedConstraints === {
    aspectRatio: true,
    autoGainControl: true,
    brightness: true,
    channelCount: true,
    colorTemperature: true,
    contrast: true,
    deviceId: true,
    echoCancellation: true,
    exposureCompensation: true,
    ....
    ....
  }
```
The list only contained with supported constraints, all key values are set to true. If some constraint unsupported by browser, that key value will be false. Therefore unsupported values will not be listed there.


### 3. getUserMedia()

This method returns Promise, which resolves a `MediaStream` object that represents a stream of media content (Interface). The stream can contain multiple video, audio tracks. That Interface allows developers to manipulate the stream of media such as adding or removing tracks, get available tracks in the stream, also the interface allows cloning the `MediaStream` object as a new instance (with unique id). It also contains event listeners which are fires when stream content changed.

When the `getUserMedia()` method executes, the browser will ask for permissions to access the devices according to provided arguments to the method.

To execute `getUserMedia()` method, it needs pass media constraints as arguments. The browser supported media constraints type can be got by `getSupportedConstraints()` method as mentioned above.

```js
const options = {
  video: true, // if false, the method returns stream without video
  audio: true // if false, the method returns stream without audio
}

navigator.mediaDevices.getUserMedia(options)
  .then( stream => {
    // resolves `MediaStream` Object
    // For an example, feed the stream to HTML video element
    document.querySelector('video').srcObject = stream;
    // After the above code line executed, the video element will show the stream.
  })
  .catch( /* handle errors*/ );
```



**Customize the stream:**
```js
  options = {
    
    audio: {
      sampleRate: 22000,
      sampleSize: 16,
      volume: 0.9,

      // removes the audio that went to the speakers from the input that comes through the mic.
      echoCancellation: true,
    },
    
    /*
    video: { height: {exact: 1280}},

    video: { width: 1280, height: 720 },

    video: { width: { min: 1280 }, height: { min: 720 }},
    */

    video: {
      width: { min: 1024, ideal: 1280, max: 1920 },

      height: { min: 776, ideal: 720, max: 1080 },

      facingMode: "user", /* get front camera, to get rear camera set value as 'environment' */

      deviceId: "preferred-device-id", /* get devices id from `mediaDevices.enumerateDevices()` */

      /*
      facingMode: { exact: "user"},
      deviceId: { exact: "preferred device id"},
      */
    }
  }
```

when constraints value supplied with `min`, `max` or `exact` keywords, it means that values are mandatory. If the browser can't find the configuration the Promise will reject with `OverConstrainedError`. with the keyword `ideal`, if the browser can't find that configuration, it will choose the configuration with nearest values.

### 4. getDisplayMedia()

This API allows browsers to capture the screen without any external plugin.
```js
  navigator.getDisplayMedia({video: true} /* this constraints object is optional*/)
    .then( stream => {
          // As getUserMedia(), this returns a stream object.
          document.querySelector('video').srcObject = stream;
    })
    .catch( /* handle errors*/ )
```
 when the method runs, the browser will prompt the user to choose which portion of the screen to share (chrome tabs, application window, the entire screen, which display if user has multiple displays).

