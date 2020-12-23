---
title: "Jibri Connect to the Openfire"
permalink: /xmpp/jibri-install-openfire.html
excerpt: "Intrigate jibri recorder to openfire"
header:
  overlay_image: /assets/images/xmpp/03-openfire-ssl/openfire_cert.jpg
  teaser: /assets/images/xmpp/03-openfire-ssl/openfire_cert.jpg
  overlay_filter: 0.5
last_modified_at: 2019-06-17T14:07:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---
## openfire 
  we can enable conerence option on openfire by installing ofmeet plugin but there is no way to reocrd conference yet. The ofmeet plugin has been created  embbeding jitsi meet. The jibri is the componet which use for conference record on jitsi meet. I could configure jibri to openfire.
## step to install jibri to openfire
  ### ofmeet plugin customization
  1. first you have to add recording buttons to the ofmeet plugin 
  ```  
  /config/src/main/java/org/igniterealtime/openfire/plugin/ofmeet/config/OFMeetConfig.java
    public List<String> getButtonsEnabled()
    {
        return JiveGlobals.getListProperty( "ofmeet.buttons.enabled", Arrays.asList( "camera", "chat", "desktop", "etherpad", "feedback", "fodeviceselection", "fullscreen", "hangup", "info", "invite", "microphone", "profile", "raisehand", "settings", "sharedvideo", "shortcuts", "stats", "videoquality","recording", "livestreaming" ) );
    }

     public List<String> getButtonsImplemented()
    {
        // These should match the implementations that are provided in the Toolbox.web.js file in jitsi-meet.
        return JiveGlobals.getListProperty( "ofmeet.buttons.implemented", Arrays.asList( "camera", "chat", "desktop", "etherpad", "feedback", "fodeviceselection", "fullscreen", "hangup", "info", "invite", "microphone", "profile", "raisehand", "settings", "sharedvideo", "shortcuts", "stats", "videoquality","recording", "livestreaming"  ) );
    }
  ```
  2. add toolbar buttons
   ```
    /ofmeet/src/i18n/ofmeet_i18n.properties
    ofmeet.toolbar.button.recording.description=Enable Recording
    ofmeet.toolbar.button.livestreaming.description=Enable Live Streaming
   ```
  3. enbale recording on config
   ```
    web/src/main/java/org/jivesoftware/openfire/plugin/ofmeet/ConfigServlet.java
    config.put( "fileRecordingsEnabled", true);
    config.put( "liveStreamingEnabled", true);
   ```
  4.  add sip communicatopr properties
   ```
   /ofmeet/src/java/org/jivesoftware/openfire/plugin/ofmeet/OfMeetPlugin.java
   System.setProperty( "org.jitsi.jicofo.jibri.BREWERY", JiveGlobals.getProperty( "org.jitsi.jicofo.jibri.BREWERY", "JibriBrewery@conference.yurdomain.com" ));
    System.setProperty( "org.jitsi.jicofo.jibri.PENDING_TIMEOUT", JiveGlobals.getProperty( "org.jitsi.jicofo.jibri.PENDING_TIMEOUT","90"));
   ``` 

   ### jibri customization
   the ofmeet confenrence url something like this https://yourdomain.com:7443/ofmeet/1234
   when we recording conrence jibri try to open confernece like this https://yourdomain.com:7443/1234 it is now correct therefor we have to provice base url for the jibri to join conference.

   1. you have to add meetUrl to config.js
   ```
   /resources/debian-package/etc/jitsi/jibri/config.json
    "meet_url":"meet.domain",
   ```
   2. change the callurl
   ```
   /src/main/kotlin/org/jitsi/jibri/api/xmpp/XmppApi.kt
   val callUrlInfo = getCallUrlInfoFromJid(
             startIq.room,
             xmppEnvironment.stripFromRoomDomain,
             xmppEnvironment.xmppDomain,
             xmppEnvironment.meetUrl
         )
   ```
   3. add meetUrl to xmpp environment
    ```bash
    src/main/kotlin/org/jitsi/jibri/config/JibriConfig.kt

    @JsonProperty("meet_url")
    val meetUrl: String,

    ```

