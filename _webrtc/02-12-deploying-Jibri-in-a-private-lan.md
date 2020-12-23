---
title: "Deploying Jibri in a private LAN"
permalink: /webrtc/deploying-jibri-in-a-private-lan.html
excerpt: "Setup multi server, load balanced videobriges on a LAN"
header:
  overlay_image: /assets/images/webrtc/02-04-jitsi-meet-and-firewalls/02-04-jitsi-meet-and-firewall.jpg
  teaser: /assets/images/webrtc/02-04-jitsi-meet-and-firewalls/02-04-jitsi-meet-and-firewall.jpg
  overlay_filter: 0.5
last_modified_at: 2020-11-16T15:58:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---
**MeetrixIO** is a well experienced company with WebRTC related technologies.
We provide **commercial support for Jitsi Meet**, Kurento, OpenVidu, BigBlue Button, Coturn Server and other webRTC related opensource projects.
{: .notice--info}

## What is Jibri
Jibri provides services for recording or streaming a Jitsi Meet conference.
It works by launching a Chrome instance rendered in a virtual framebuffer, capturing and encoding the output with ffmpeg. It is intended to be run on a separate machine (or a VM), with no other applications using the display or audio devices. Only one recording at a time is supported on a single jibri.



## Configuring Jitsi Meet to allow Jibri to connect and ‘participate’ in any videoconference.
* Prosody (this as the background xmpp server. You can imagine this to be similar like an instan-messaging backend service)
* Jicofo (this is another background service which takes care of switching/relaying the processes between all participants and the videobridge)
* Jitsi Meet (consider this as the master service which takes care of presenting all functionality in your web browser)


## Prosody configuration

Configure Prosody
```
nano /etc/prosody/prosody.cfg.lua
```

Uncomment the items in the "conference" section:
```
---Set up a MUC (multi-user chat) room server on conference.example.com:
Component "conference.jitsi.example.com" "muc"
--- Store MUC messages in an archive and allow users to access it
modules_enabled = { "muc_mam" }
```

Create the internal Multi User Component (MUC) and the the recorder virtual host entry, login to server “jitsi” and:
```
nano /etc/prosody/conf.avail/meet.meetrix.io.cfg.lua
```
At the end of this file, add a dedicated section
```
-- internal muc component, meant to enable pools of jibri and jigasi clients
Component "internal.auth.meet.meetrix.io" "muc"
    modules_enabled = {
	    "ping";
    }
    storage = "memory"
    muc_room_cache_size = 1000
	
VirtualHost "recorder.meet.meetrix.io"
    modules_enabled = {
        "ping";
    }
    authentication = "internal_plain"
```
Reload prosody:
```
systemctl reload prosody
```
Create two users for Jibri (user: jibri and user: recorder):
```
prosodyctl register jibri auth.meet.meetrix.io JibrisPass
prosodyctl register recorder recorder.meet.meetrix.io RecordersPass
```
## Jicofo configuration

Set the Multi User Component (MUC) to look out for Jibri:
```
nano /etc/jitsi/jicofo/sip-communicator.properties
```
In this file, add two lines:
```
org.jitsi.jicofo.jibri.BREWERY=JibriBrewery@internal.auth.meet.meetrix.io
org.jitsi.jicofo.jibri.PENDING_TIMEOUT=90
```
Reload Jicofo:
```
systemctl reload Jicofo
```
## Jitsi Meet configuration

Make sure we have the button for recording and/or streaming in our config:
```
nano /etc/jitsi/meet/meet.meetrix.io-config.js
```
### Bosh Connection using IP instead of Domain Name
Default Config for Domain Name
```
bosh: '//meet.meetrix.io/http-bind'
```
Changing the Domain to IP
```
bosh: '//192.168.10.10/http-bind'
```
Look for following lines, uncomment the line and set value to ‘true’ or add if the line does not exist:
```
fileRecordingsEnabled: true, // If you want to enable file recording
liveStreamingEnabled: true, // If you want to enable live streaming
hiddenDomain: 'recorder.meet.meetrix.io',
```

# Install Jibri:

## Jibri PreRequisite

* First make sure the ALSA loopback module is available. The extra modules (including ALSA loopback) can be installed on Ubuntu 16.04/18.04 using package name linux-image-extra-virtual
* Perform the following tasks as the root user
  * Set up the module to be loaded on boot: echo "snd-aloop" >> /etc/modules
  * Load the module into the running kernel: modprobe snd-aloop
* Check to see that the module is already loaded: lsmod | grep snd_aloop
* If the output shows the snd-aloop module loaded, then the ALSA loopback configuration step is complete.

Enable the ALSA loopback module to start on boot, and also start it for the current boot:
```
echo "snd_aloop" >> /etc/modules
modprobe snd_aloop
```
### FFmpeg with X11 capture support
* Jibri requires a relatively modern ffmpeg install with x11 capture compiled in. This comes by default in Ubuntu 16.04/18.04, by installing the ffmpeg package.
* If building Jibri for Ubuntu 14.04 (trusty), the mc3man repo provides packages. They can be used by the following in Ubuntu 14.04:

Setup FFmpeg PPA
```
sudo add-apt-repository ppa:jonathonf/ffmpeg-4
```

Install FFmpeg on Ubuntu

After enabling the PPA, Lets exec below commands to install ffmpeg on Ubuntu system. This will also install many packages for the dependencies.
```
sudo apt-get update
sudo apt-get install ffmpeg
```
Check FFmpeg Version
```
ffmpeg -version
```
Miscelleneous 
```
sudo apt-get install wget gnupg software-properties-common default-jre-headless  curl alsa-utils icewm xdotool xserver-xorg-input-void xserver-xorg-video-dummy
```

Install Google Chrome:
```
curl -o - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add
echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list
apt update
apt install google-chrome-stable
```
Install ChromeDriver:
```
CHROME_DRIVER_VERSION=`curl -sS chromedriver.storage.googleapis.com/LATEST_RELEASE`
wget -N http://chromedriver.storage.googleapis.com/$CHROME_DRIVER_VERSION/chromedriver_linux64.zip -P ~/
unzip ~/chromedriver_linux64.zip -d ~/
rm ~/chromedriver_linux64.zip
sudo mv -f ~/chromedriver /usr/local/bin/chromedriver
sudo chmod 0755 /usr/local/bin/chromedriver
```
Turn off warnings about scripted control in Chrome:
```
mkdir -p /etc/opt/chrome/policies/managed

echo '{ "CommandLineFlagSecurityWarningsEnabled": false }' >>/etc/opt/chrome/policies/managed/managed_policies.json
```

Jitsi Debian Repository

The Jibri packages can be found in the stable repository on downloads.jitsi.org. First install the Jitsi repository key onto your system:
```
wget -qO - https://download.jitsi.org/jitsi-key.gpg.key | sudo apt-key add -
```
Create a sources.list.d file with the repository:
```
sudo sh -c "echo 'deb https://download.jitsi.org stable/' > /etc/apt/sources.list.d/jitsi-stable.list"
```
Update your package list:
```
sudo apt-get update
```
Install the latest jibri
```
sudo apt-get install jibri
```
Add Jibri's user account to the necessary groups:

```
sudo usermod -aG adm,audio,video,plugdev jibri
```

## Jibri Configuration
Step 1: Install the required packages that are available from Debian 10's default repositories:

We need to configure the xmpp environments and the directory where we want to store our recordings:
```
nano /etc/jitsi/jibri/config.json
```
Change settings according to:
```
{
    "recording_directory":"/srv/recordings",
    "finalize_recording_script_path": "/path/to/finalize_recording.sh",
    "xmpp_environments": [
        {
            "name": "prod environment",
            "xmpp_server_hosts": [
                "meet.meetrix.io"
            ],
            "xmpp_domain": "meet.meetrix.io",
            "control_login": {
                // The domain to use for logging in
                "domain": "auth.meet.meetrix.io",
                // The credentials for logging in
                "username": "jibri",
                "password": "JibrisPass"
            },
            "control_muc": {
                "domain": "internal.auth.meet.meetrix.io",
                "room_name": "JibriBrewery",
                "nickname": "jibri-nickname"
            },
            "call_login": {
                "domain": "recorder.meet.meetrix.io",
                "username": "recorder",
                "password": "RecordersPass"
            },
            "room_jid_domain_string_to_strip_from_start": "conference.",
            "usage_timeout": "0"
        }
    ]
}
```

Jibri Code

[https://github.com/jitsi/jibri](href=https://github.com/jitsi/jibri)

```
/src/main/kotlin/org/jitsi/jibri/selenium/JibriSelenium.kt
```
Changing the Code to accept the Self Signed /Insecure Certificate
```
chromeOptions.addArguments("--ignore-certificate-errors")
chromeOptions.setAcceptInsecureCerts(true)
```
Setting RTMP URL
```
src/main/kotlin/org/jitsi/jibri/service/impl/StreamingJibriService.kt
```
```
const val YOUTUBE_URL = "rtmp://zb01.lmt.glcx.cnpc:1935/myapp"
```
Then we compile the Jibri Code
```
mvn package -DskipTests
```
After it is compiled.Replace the New jar with the old one
```
sudo /opt/jibri/target/jibri-8.0-Snapshot-with-dependies.jar /opt/jitsi/jibri
```

Reload Jibri
```
systemctl reload jibri
```

Make a directory to store recordings and set its permissions appropriately:
```
mkdir /recordings
chown jibri:jibri /recordings
```
Install Java 8
```
wget -O - https://adoptopenjdk.jfrog.io/adoptopenjdk/api/gpg/key/public | sudo apt-key add -
add-apt-repository https://adoptopenjdk.jfrog.io/adoptopenjdk/deb/
apt update
apt install adoptopenjdk-8-hotspot
```
Configure Jibri to start with Java 8 instead of the default Java version (replace the word "java" with the full path to Java 8):
```
nano /opt/jitsi/jibri/launch.sh
/usr/lib/jvm/adoptopenjdk-8-hotspot-amd64/bin/java
```

Restart all services, enable and start jibri
```
systemctl restart prosody
systemctl restart jicofo
systemctl restart jitsi-videobridge2
systemctl enable --now jibri
```

## Limitation of Deployment of Jitsi in LAN
1. You have to use a self-signed certificate (because of the lack of a public IP address and domain)
2. Therefore all browsers will grumble because they do not find a valid certificate chain. But they work if the users accept the exception. Unless you distribute valid certificates to all devices…
3. Mobile apps might not work at all, because they require a valid certificate chain.

**Looking for commercial support for Jitsi Meet ?** Please contact us via [hello@meetrix.io](https://meetrix.io/contact-us)
{: .notice--warning}