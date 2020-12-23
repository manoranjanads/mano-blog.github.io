---
title: How to Setup A Signaling Server
permalink: /webrtc/how-to-setup-a-signaling-server.html
layout: single
read_time: true
author_profile: true
share: true
comments: true
excerpt: "A Guide to setup a simple signaling server for WebRTC"
header:
  overlay_image: /assets/images/webrtc/01-04-how-to-setup-a-signaling-server/jsep.png
  teaser: /assets/images/webrtc/01-04-how-to-setup-a-signaling-server/setup-signaling-server.jpg
  overlay_filter: 0.5
last_modified_at: 2019-07-04T00:15:07-04:00
toc: true
sidebar:
nav: webrtc
---
### Who can use this signaling server? ###
This is a simple signaling server designed specially for [SimpleWebRTC](https://www.simplewebrtc.com/). It simply passes the data between the two parties and can be used with other webrtc solutions if modified. If your use case is specific and complex I recommend you to try other signaling servers.

### What is a signaling server? ###
Signaling plays an important role in the overall flow of webRTC. It basically performs the role of connecting to the other user. Rest of the communication will be handled by webRTC.

### Setup Signaling Server project ###

1. git clone https://github.com/SimpleWebRTC/signalmaster.git
2. npm install
3. sudo apt install nginx
3. sudo npm i -g pm2

### Get a SSL Certificate ###

To establish a secure connection you need to get a ssl certificate. Letsencrypt is a free certificate provider. These commands would work for ubuntu with nginx

```
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo apt-get install certbot python-certbot-nginx
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update

$ sudo apt-get install certbot python-certbot-nginx
$ sudo certbot --nginx
```
Enter your domain and chose to redirect http requests to https

### Configure signaling server ###

Configure the file config/production.json for production configurations and config/development.json for development environment. Given below is a sample configuration for production. You can replace `<domain>` with your domain.

```json
    {
  "isDev": true,
  "server": {
    "port": 8888,
    "/* secure */": "/* whether this connects via https */",
    "secure": true,
    "key":"/etc/letsencrypt/live/<domain>/privkey.pem",
    "cert": "/etc/letsencrypt/live/<domain>/fullchain.pem",
    "password": null
  },
  "rooms": {
    "/* maxClients */": "/* maximum number of clients per room. 0 = no limit */",
    "maxClients": 20
  },
  "stunservers": [
    {
      "urls": "stun:stun.l.google.com:19302"
    }
  ],
  "turnservers": [
    {
      "urls": ["turn:<turn-server-ip>:443"],
      "secret": "<turn-server-secret-key>",
      "expiry": 86400
    }
  ]
 }
```

### Configure Nginx ###

The server runs on `port 888` and you need to proxy the https requests to the port. Add the following to the https server configurations in nginx

```conf
    location / {
        proxy_pass https://localhost:8888;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
```
### Fix sockets.js ###

Since this project was made up using an old version of socket.io you might encounter an error 
`Missing error handler on socket.`
`TypeError: io.sockets.clients is not a function`

To fix this replace the function `clientsInRoom` in sockets.js with the below code.

```javascript
    function clientsInRoom(name) {
            var clientsCount = 0;
        for (var socketId in io.nsps['/'].adapter.rooms[name] || {}) {
            clientsCount++;
        }
        return clientsCount;
    }
```

### Final Steps ###

Restart the nginx service
```
$ sudo service nginx restart
```

Start the server and keep it running with
```
$ pm2 start server.js
```
 
 You should see a line saying `signal master is running at: http://localhost:8888`

### Test ###
To make sure that it is working correctly you can try visiting `https://<your-domain>/socket.io/` and you should get `{"code":0,"message":"Transport unknown"}` as the result