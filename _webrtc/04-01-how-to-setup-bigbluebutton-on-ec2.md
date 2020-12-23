---
title: How to Setup BigBlueButton HTML5 Version on EC2
permalink: /webrtc/how-to-setup-bigbluebutton-on-ec2.html
layout: single
read_time: true
author_profile: true
share: true
comments: true
excerpt: "A Complete Guide From Setting up the Security Groups to Configuring Turn Server"
header:
  overlay_image: /assets/images/webrtc/04-01-how-to-setup-bigbluebutton-on-ec2/04-01-how-to-setup-big-blue-button.jpg
  teaser: /assets/images/webrtc/04-01-how-to-setup-bigbluebutton-on-ec2/04-01-how-to-setup-big-blue-button.jpg
  overlay_filter: 0.5
last_modified_at: 2019-06-25T23:34:07-04:00
toc: true
sidebar:
nav: webrtc
---

BigBlueButton provides a script to install BigBlueButton with single command but it is always better to go through each step, so that you know what is really being done and makes it easy to do any modification or debug any error.

### Minimum Requirements ###

1. It is adviced to use a fresh server for the installation of BigBlueButton. The minimum requirement would be a c5.xlarge instance which posses 4 cores and 8GB of RAM. 
2. A domain to host the bbb server
3. `Ubuntu 16.04 64-bi`t OS running `Linux kernel 4.x`
4. `TCP` ports `80` and `443` open
5. `TCP` ports `7443` (to configure SSL) and `5066` open
6. `UDP` ports `16384 - 32768` open
7. `500GB` of free disk space (Recommended for production)
8. `250 Mbits/sec` bandwidth

Having enough disk space is essential if you have number of sessions been done since recordings would take up the space. Also consider using a domain and obtain SSL certificate to ensure technologies like WebRTC which is used by BigBlueButton to make real time communication works without any error.

### Configure environment ###

Please go through the below steps to ensure that there are no errors during the installation

1. Check that the locale of the server is en_US.UTF-8.
    ```bash
    $ cat /etc/default/locale
    LANG="en_US.UTF-8"
    ```

    If you don’t see LANG="en_US.UTF-8", enter the following commands to set the local to en_US.UTF-8.
    ```bash
    $ sudo apt-get install language-pack-en
    $ sudo update-locale LANG=en_US.UTF-8
    ```

    Exit from the ssh session and login again before proceding with any other command. Run the first command again to make sure the locale is set correctly.

2. ```bash
    $ sudo systemctl show-environment
    LANG=en_US.UTF-8
    PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
  ```

    If you don't see `LANG=en_US.UTF-8` run 
    ```bash 
    $ sudo systemctl set-environment LANG=en_US.UTF-8 
    ``` 
    and check the output again

### Setup Security Groups in EC2 ###

As mentioned above BigBlueButton needs several TCP and UDP ports to be allowed access to. 
1. Go to `Security Groups` in aws ec2 console
2. `Create Security Group`
3. Give a name and then press `Add Rule`
4. Select `Custom TCP` and enter 80 for `Port Range`
5. Select `Anywhere` for `Source` if you would not mind allowing traffic from any ip.
6. Press `Create`
7. Repeat the steps from 2-6 for ports 443, 5066 and 7443
8. Also select `Custom UDP` ,enter 16384 - 32768 and repeat the steps 5-6


### Install necessary packages and security updates ###

1. ```bash
$ grep "multiverse" /etc/apt/sources.list
```
Here an uncommented line should have `multiverse` word with it. If not 
```bash
$ echo "deb http://archive.ubuntu.com/ubuntu/ xenial multiverse" | sudo tee -a /etc/apt/sources.list
```
2. ```bash
$ add-apt-repository ppa:jonathonf/ffmpeg-4 -y
$ add-apt-repository ppa:rmescandon/yq -y
```
to add repositores `yq` and `ffmpeg` needed for bigbluebutton
3. ```bash
$ sudo apt-get update
$ sudo apt-get dist-upgrade
```
 to update the system with the added repositories
4. ```bash
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
$ echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/
sources.list.d/mongodb-org-3.4.list
$ sudo apt-get update
$ sudo apt-get install -y mongodb-org curl
```
 to install mongodb which is used by bigbluebutton
5. ```bash
$ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
$ sudo apt-get install -y nodejs` to install nodejs
```

### Add Public Key to server's key chain and get bigbluebutton packages ###

```bash
$ wget https://ubuntu.bigbluebutton.org/repo/bigbluebutton.asc -O- | sudo apt-key add -
```
to add project's public key to server key chain

We will be installing bigbluebutton-2.2beta which is the latest at the time of this article. Since your server needs to know where to download the BigBlueButton 2.2-beta packages,

```bash
$ echo "deb https://ubuntu.bigbluebutton.org/xenial-220-beta/ bigbluebutton-xenial main" | sudo tee /etc/apt/sources.list.d/bigbluebutton.list
$ sudo apt-get update
```

### Install BigBlueButton ###

If you have followed the steps properly you should be ready to install bigbluebutton now.

```bash
$ sudo apt-get install bigbluebutton
$ sudo apt-get install bbb-html5

$ sudo apt-get dist-upgrade
```
Next, restart BigBlueButton:

```bash
$ sudo bbb-conf --restart
```

### Configure BigBlueButton ###

Go to `/usr/share/bbb-web/WEB-INF/classes/bigbluebutton.properties` and update the following property.

```bash
bigbluebutton.web.serverURL=https://example.site.com
```

Go to `/usr/share/red5/webapps/screenshare/WEB-INF/screenshare.properties` and update the following properties.

```
streamBaseUrl=rtmp://example.site.com/screenshare
jnlpUrl=https://example.site.com/screenshare
jnlpFile=https://example.site.com/screenshare/screenshare.jnlp
```

Run the following command to configure the BigBlueButton server with your hostname

```bash
$ bbb-conf --setip example.site.com
```

### Install Nginx and SSL certificates ###

I would recommend to use nginx with bbb. 
To install nginx 
```bash
$ sudo apt install nginx
```

If you have a domain you can get SSL certificates and configure nginx automatically with letsencrypt

```bash
  $ sudo apt-get update
  $ sudo apt-get install software-properties-common
  $ sudo add-apt-repository universe
  $ sudo add-apt-repository ppa:certbot/certbot
  $ sudo apt-get update

  $ sudo apt-get install certbot python-certbot-nginx
  $ sudo certbot --nginx
```

### Test the installation ###

Go to `https://example.site.com/` and if loads a screen like below the installation is successfull.

![bigbluebutton](/blog/assets/images/webrtc/14-how-to-setup-bigbluebutton-on-ec2/bigbluebutton.png)

### Debugging BBB ###

If you experience an error when starting the bbb server you can first try uing 
```bash
$ sudo bbb-conf --check
``` 
which will show you a summary of the bbb services and their status. It will also show errors under `** Potential problems described below **` from which you can get a clue of what you are doing wrong.