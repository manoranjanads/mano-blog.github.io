---
title: "Introduction to XMPP"
permalink: /xmpp/xmpp-intro.html
excerpt: "Xmpp Introduction"
header:
  overlay_image: /assets/images/xmpp/01-introduction/xmpp.jpeg
  teaser: /assets/images/xmpp/01-introduction/xmpp.jpeg
  overlay_filter: 0.5
last_modified_at: 2019-10-26-17T14:07:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---

## What is XMPP

XMPP  stands for  `Extensible Messaging and Presence Protocol`. It's a mature and a well-defined protocol.
xmpp  enables the near-real-time exchange of structured xml data between two or more network entities. But it's designed for small chuks or data, rather thand large blobs or binary data.

## Main Components of XMPP

The XMPP protocol can be divided in to two parts, Core and Extensible part.
XMPP core serveice  provides one to one messaging capabilities, user Presence, contactlist and security mechanisms for communication beetween two parties.

you can read more about core xmpp here [RFC6120](https://tools.ietf.org/html/rfc6120), [RFC6121](https://tools.ietf.org/html/rfc6121).
{: .notice--success}

`XMPP` Extensible part provides additional sevices such as multi user chat, service discovery services.

## How does Xmpp Communication Work

XMPP is based on the client serve architecture.
It can provide the means of communication between client to server or server to server.
There are many XMPP servers on the internet and those are interconnected with federated networks.
Each an every client who is connected to an XMPP server is assigned a unique identifier with the following format.

`user@domain.com/resources`

where, user is username domain.com is domain  that used to reach to the node.
This identifier is called jabber id.
When client a starts a session with XMPP server, the client opens a long lived tcp connection with server and starts an XML stream to the server. 
When the server accepts it, server also opens an XML stream to the client. Therefore there are two xml streams. One from client to server and the other one from server to client.

## XMPP Stanzas

The most basic unit of communication in XMPP is called a stanza. there are 3 type of stanzas used in xmpp.

1. Presence

`Presence` stanza is used to share Presence information between a client and the contacts of the clints (roaster).

```xml
    <Presence
    from="user1@mydomain.com"
    id="1232312312312"
    to="user2@mydomain.com"
    >
      <show>online</show>
    </Presence>
```

{:start="2"}
2. Message

The stanza type `Message` is used to share chat messages between two parties.

```xml
 <message
 from="user1@mydomain.com"
 id="1232312312312"
 to="user2@mydomain.com"
 type="chat"
 >
 <body>Hi friends</body>
 </message>

```

{:start="3"}
3. IQ

`IQ` is used to share information between XMPP server and the client.

Sample IQ request

```xml
<iq id="1952c42c-8fbf-43d6-9b85-5b0e79c3e3f7:sendIQ" to="user2@mydomain.com" type="get" xmlns="jabber:client">
<vCard xmlns="vcard-temp"/>
</iq>
```

Sample IQ response

  ```xml
<iq type="result" id="1952c42c-8fbf-43d6-9b85-5b0e79c3e3f7:sendIQ" from="user2@mydomain.com" to="user2@mydomain.com/converse.js-132196857" xmlns="jabber:client"><vCard xmlns="vcard-temp">
    <FN>Nifro</FN>
    <NICKNAME>Akalana</NICKNAME>
    <URL/>
    <ROLE>Dev</ROLE>
    <EMAIL><INTERNET/><PREF/><USERID>demo@gmail.com</USERID></EMAIL>
    <PHOTO>
      <TYPE>image/png</TYPE>
      <BINVAL>iVBORw0KGgoAAAANSUhEU</BINVAL>
    </PHOTO>
</vCard></iq>
```

`xmlns` attribute describs wheather the client is requesting data from the server or sending data to the serve. For example, if the client wants to get his contact list, he will use xmlns "jabber:iq:roster"

### Popular XMPP Servers and Clients

Here are three popular XMPP servers.

1. [ejabberd](https://www.ejabberd.im/)
2. [openfire](https://www.igniterealtime.org/projects/openfire/)
3. [prosody](https://prosody.im/)

[conversejs](https://conversejs.org/) is one of our favourite XMPP JS clients. You can find the list of XMPP clients [here](https://xmpp.org/software/clients.html)



