---
title: "How to Install Jitsi Meet"
permalink: /webrtc/jitsi/meet/installing.html
excerpt: "on Ubuntu 16.04"
header:
  overlay_image: /assets/images/webrtc/02-01-how-to-install-jitsi-meet-on-ubuntu-16.04/02-01-how-to-intall-jitsi-meet-on-ubuntu-16.04.jpg
  teaser: /assets/images/webrtc/02-01-how-to-install-jitsi-meet-on-ubuntu-16.04/02-01-how-to-intall-jitsi-meet-on-ubuntu-16.04.jpg
  overlay_filter: 0.5
last_modified_at: 2018-10-19T15:58:49-04:00
redirect_from:
  - /theme-setup/
toc: true
author: Jay
---
[Meetrix.IO](https://meetrix.io) provides installation, configuration and customization services for Jitsi Meet.
Get fully configured Jitsi Meet setup on your own server (starting from `$300`).
Please shoot an email to `hello@meetrix.io` for more information
{: .notice--success}

Jitsi Meet is a mature opensource conferencing system. In this tutorila, we are going to install an instance of jitsi meet on Ubuntu 16.04 (an AWS Ec2 instance).
Here are the components that will be installed when installing Jitsi Meet

1. Nginx
2. Prosody
3. Jicofo (Jitsi Conference Focus)
4. JVB (Jitsi Video Bridge)
5. Jitsi Meet Frontend

You should know the domain/sub-domain name that you are going to use with the installation.

## Installing Nginx

```bash
sudo apt-get install nginx
```

## Install Jitsi Meet package

First we need to add the repository

```bash
echo 'deb https://download.jitsi.org stable/' >> /etc/apt/sources.list.d/jitsi-stable.list
wget -qO -  https://download.jitsi.org/jitsi-key.gpg.key | apt-key add -
```

Then update the package list

```bash
sudo apt-get update
```

install jitsi meet

```bash
apt-get -y install jitsi-meet
```

Enter the domain name that you are going to use with the installation.
ex : meet.meetrix.io

## Installing Lets Encrypt SSL Certificates

If you have pointed the domain to the server, copy and past following commands to install eetf certbot client and install ssl certificates

```bash
sudo apt-get -y update && \
sudo apt-get -y install software-properties-common && \
sudo add-apt-repository -y ppa:certbot/certbot && \
sudo apt-get -y update && \
sudo apt-get -y install python-certbot-nginx && \
sudo certbot --nginx
```

That's it..
