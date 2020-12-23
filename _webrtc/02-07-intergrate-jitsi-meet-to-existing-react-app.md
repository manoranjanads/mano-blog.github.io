---
title: "Integrate Jitsi Meet to React application"
permalink: /webrtc/integrate-jitsi-meet-to-react-app.html
excerpt: "Way to include the world's best video conferencing platform to your existing React application."
header:
  overlay_image: /assets/images/webrtc/02-07-integrate-jitsi-meet-to-existing-react-app/Integrate-jitsi-meet-to-react-application.png
  teaser: /assets/images/webrtc/02-07-integrate-jitsi-meet-to-existing-react-app/Integrate-jitsi-meet-to-react-application.png
  overlay_filter: 0.5
last_modified_at: 2019-10-27T06:18:45.301Z
redirect_from:
  - /theme-setup/
toc: true
---
Jitsi is a set of open-source projects that allows you to easily build and deploy secure videoconferencing solutions.

To embed Jitsi Meet into your application you have to add the Jitsi Meet API library:

```html
<script src='https://meet.jit.si/external_api.js'></script>
```

You can place the script just before the closing of the **body** tag.

```html
<body>
 <div id="root"></div>
 <script src='https://meet.jit.si/external_api.js'></script>
</body>
```

Lets make a React component for loading Jitsi Meet in it.

```jsx
import React, { useState, useEffect } from 'react';
import ProgressComponent from '@material-ui/core/CircularProgress';

function JitsiMeetComponent() {
  const [loading, setLoading] = useState(true);
  const containerStyle = {
    width: '800px',
    height: '400px',
  };

  const jitsiContainerStyle = {
    display: (loading ? 'none' : 'block'),
    width: '100%',
    height: '100%',
  }

 function startConference() {
  try {
   const domain = 'meet.jit.si';
   const options = {
    roomName: 'roomName',
    height: 400,
    parentNode: document.getElementById('jitsi-container'),
    interfaceConfigOverwrite: {
     filmStripOnly: false,
     SHOW_JITSI_WATERMARK: false,
    },
    configOverwrite: {
     disableSimulcast: false,
    },
   };

   const api = new JitsiMeetExternalAPI(domain, options);
   api.addEventListener('videoConferenceJoined', () => {
    console.log('Local User Joined');
    setLoading(false);
    api.executeCommand('displayName', 'MyName');
   });
  } catch (error) {
   console.error('Failed to load Jitsi API', error);
  }
 }

 useEffect(() => {
  // verify the JitsiMeetExternalAPI constructor is added to the global..
  if (window.JitsiMeetExternalAPI) startConference();
  else alert('Jitsi Meet API script not loaded');
 }, []);

 return (
  <div
   style={containerStyle}
  >
   {loading && <ProgressComponent />}
   <div
    id="jitsi-container"
    style={jitsiContainerStyle}
   />
  </div>
 );
}

export default JitsiMeetComponent;
```

If you don't set the **parentNode** property value, the Jitsi iFrame element will append as a child component of the document's body tag.

Then place the component, where you want to render the conference.
Since this is a functional component, the component uses **useEffect** hook for initializing the Jitsi Meet. If you are building class-based component, place initializing code inside the **componentDidMount** lifecycle method. Otherwise, the multiple conferences will render on the view when every time re-renders the component.

{% include figure image_path="assets/images/webrtc/02-07-integrate-jitsi-meet-to-existing-react-app/preview.png" alt="Enabling Controls" caption="Now Jitsi is integrated in to your application" %}
