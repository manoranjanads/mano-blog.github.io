---
title: "Openfire Let's Encrypt Cetificate Installation"
permalink: /xmpp/openfire_ssl.html
excerpt: "ubuntu openfire ssl setup with certbot"
header:
  overlay_image: /assets/images/xmpp/03-openfire-ssl/openfire_cert.jpg
  teaser: /assets/images/xmpp/03-openfire-ssl/openfire_cert.jpg
  overlay_filter: 0.5
last_modified_at: 2019-06-17T14:07:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---
## let's encrypt ssl certificate installation on openfire with ubuntu 18.04
### generate ssl cetificate with certbot standalone

1. [follow this instruction to generate ssl cetificate](https://certbot.eff.org/lets-encrypt/ubuntubionic-other)  
   <span style="color:red"> you have to open port 80 for this installation</span>
2. open the certificate store management console from your openfire server  
   yourdomain.com:9090/security-certificate-store-management.jsp  
   ![certificate management](/blog/assets/images/xmpp/03-openfire-ssl/openfire_certificate_console.png)  
   click the <span style="text-decoration:underline;color:orange">Manage Store Contents</span> under the IdentityStore  
3. ![identity store](/blog/assets/images/xmpp/03-openfire-ssl/openfire_ssl_identity_store.png)
  click imported here
  <br>
4. ![import](/blog/assets/images/xmpp/03-openfire-ssl/openfire_ssl_import.png)
    on you openfire server go to the /etc/letsencrypt/live/yourdomain.com/   
    paste private key of privkey.pem on __Content of private Key file:__  
    paste certificate key of cert.pem on __Content of Certificate file:__  
    save  
5. ![identity store](/blog/assets/images/xmpp/03-openfire-ssl/openfire_import_done.png)
   remove self sign key
6. click the <span style="text-decoration:underline;color:orange">Manage Store Contents</span> under the __Trust store used for connections from other servers__ certificate store management console from your openfire server    
   http://yourdomain.com:9090/security-truststore.jsp?connectionType=SOCKET_S2S
   click import form
7.  ![trust store](/blog/assets/images/xmpp/03-openfire-ssl/openfire_trust_store.png)
    set Alies as Let's Encrypt  
    paste one of certificate key from fullchain.key on __Content of Certificate file:__  
    save
8. Done ![let's encrypt](/blog/assets/images/xmpp/03-openfire-ssl/openfire_ssl_finish.png)
