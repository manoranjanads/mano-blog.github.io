---
title: "Jitsi Meet Load Balancing"
permalink: /xmpp/jitsi_haproxy_configure.html
excerpt: "jitsi meet ha proxy load balancer setup on ec2 ubuntu server"
header:
  overlay_image: /assets/images/xmpp/03-openfire-ssl/openfire_cert.jpg
  teaser: /assets/images/xmpp/03-openfire-ssl/openfire_cert.jpg
  overlay_filter: 0.5
last_modified_at: 2019-06-17T14:07:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---
### install ha proxy on ubuntu server
```bash
sudo add-apt-repository ppa:vbernat/haproxy-2.0
sudo apt-get update
sudo apt-get install haproxy
```
### configura haproxy
```bash
sudo vi /etc/haproxy/haproxy.cfg
```

### add below line to the haproxy configuration
```bash
frontend meet
  bind *:443 ssl crt /etc/haproxy/certs/yourdmoani.pem
  option forwardfor
  reqadd X-Forwarded-Proto:\ https
  acl letsencrypt-acl path_beg /.well-known/acme-challenge/
  use_backend letsencrypt-backend if letsencrypt-acl
  default_backend www-backend

backend www-backend

  balance url_param room
  option  httpchk
  hash-type consistent
  server shard1 44.45.3.11  check ssl verify none
  server shard2 32.12.216.187  check ssl verify none
```
### restart haproxy
```bash
 haproxy -c -f /etc/haproxy/haproxy.cfg
 sudo service haproxy restart
```