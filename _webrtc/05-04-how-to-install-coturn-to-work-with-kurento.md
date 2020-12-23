---
title: "How to Install Coturn to Work with Kurento"
permalink: /webrtc/coturn.html
excerpt: "on Ubuntu with Long Term Credentials Mechanism"
header:
  overlay_image: /assets/images/webrtc/05-04-how-to-install-coturn-to-work-with-kurento/05-04-how-to-install-coturn-to-work-with-kurento.jpg
  teaser: /assets/images/webrtc/05-04-how-to-install-coturn-to-work-with-kurento/05-04-how-to-install-coturn-to-work-with-kurento.jpg
  overlay_filter: 0.5
last_modified_at: 2019-03-05-19T15:59:07-04:00
toc: true
author: Jay
---

Get Coturn configured by [Meetrix.IO](https://meetrix.io) just for `$250`.
Please shoot an email to `hello@meetrix.io` for more information.
{: .notice--success}

You can install the Kurento Media server with using our [Kurento installation guide](https://meetrix.io/webrtc/kurento.html). To make Kurento work perfectly behind NATs, you need a Turn server.
[Coturn](https://github.com/coturn/coturn) is an opensource turn server. We can easily setup Coturn on Ubuntu 16.04 (Xenial) with official Coturn repo. 

## Firewall Rules
First Make sure that you have opened up following ports in your firewall


```
3478 : UDP
49152–65535 : UDP
```

If you are installing Coturn on the same box as Kurento, you have to open additional ports for Kurento as well


## Installing Coturn
Login to Ubuntu  shell and enter following command to install Coturn


```bash
sudo apt-get -y update
sudo apt-get -y install coturn
```

## Start the Kurento Daemon at Startup
To setup kurento start at system startup

```
sudo vim /etc/default/coturn
```

Uncomment the following line by removing the `#` at the beginning to run Coturn as an automatic system service daemon
```bash
TURNSERVER_ENABLED=1
```

## Creating a User

#### With the Configuration File

This method should work with most of the versions of Coturn.
Open (or create) `/etc/turnserver.conf` file and past the following content. Replace `<YOUR_USERNAME>`, `<YOUR_PASSWORD>` and `<YOUR_PUBLIC_IP_ADDRESS>` values with your own ones.

```bash
fingerprint
user=<YOUR_USERNAME>:<YOUR_PASSWORD>
lt-cred-mech
realm=kurento.org
log-file=/var/log/turnserver/turnserver.log
simple-log
external-ip=<YOUR_PUBLIC_IP_ADDRESS>
```

Now restart the coturn service

```bash
sudo service coturn restart
```

#### With Turn Admin (Quick and Easy)

By default, coturn is configured to use an sqllite database.
We can create a user in the database using the utility called `turnadmin` that comes with coturn.

to create a user with `turnadmin` use following command
```bash
turnadmin -k -u <YOUR_USERNAME> -p <YOUR_PASSWORD> -r <REALM>
```

For an example

```bash
turnadmin -k -u meetrix -p 1234 -r meetrix.io
```

## Testing
Go to [trickle-ice](https://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/) page and enter following details.

```bash
STUN or TURN URI : turn:<YOUR_PUBLIC_IP_ADDRESS>:3478
TURN username: <YOUR_USERNAME>
TURN password: <YOUR_PASSWORD>
```

Then click `Add Server` and then `Gather candidates` button. If you have done everything correctly, you should see `Done` as the final result. If you do not get any response or if you see any error messages, please double check if you have followed this guide as it is.

## Configuring Kurento

open `/etc/kurento/modules/kurento/WebRtcEndpoint.conf.ini`. Remove everything and add following line

```bash
turnURL=<YOUR_USERNAME>:<YOUR_PASSWORD>@<YOUR_PUBLIC_IP_ADDRESS>:3478
```

Now restart Kurento Media Server

`sudo service kurento-media-server restart`

That's it !