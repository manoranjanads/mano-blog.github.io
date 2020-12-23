---
title: "What is WebRTC"
permalink: /webrtc/what-is-webrtc.html
excerpt: "Introduction to WEB Realtime Communication"
header:
  overlay_image: /assets/images/webrtc/01-01--what-is-webrtc/hero.png
  teaser: /assets/images/webrtc/01-01--what-is-webrtc/what-is-webrtc.jpg
  overlay_filter: 0.5
last_modified_at: 2019-06-02T15:58:49-04:00
redirect_from:
  - /theme-setup/
toc: true
---

**WebRTC is a technology that helps to enable real-time peer to peer media communication between web browsers or devices without any external plugin.**

It is available natively modern web browsers and mobile platforms like Chrome, Firefox, Safari, Edge, Opera Android, and iOS.

## There are 3 primary components in WebRTC

- **MediaStream API**  
The MediaStream API provides the functionality to access camera, microphone or screen using javascript.

- **RTCPeerConnection API**  
The RTCPeerConnection API takes care of the NAT traversal, Codec processing, SDP negotiation, Media transferring and much more on handling the secure connection between peers.

- **RTCDataChannel API**  
The RTCDataChannel API allows to setup bidirectional data transfer channel between peers.

## Establishing the connection between peers

**Signaling** is the process which establishes the connection between peers. It can achieve by WebSockets, XMPP, SIP or any copy & paste mechanism.

{% include figure image_path="/assets/images/webrtc/01-01--what-is-webrtc/signaling.png" alt="this is a placeholder image" caption="Signaling Process" %}

- **Session Description Protocol**  
 Also known as SDP, It is a protocol used to negotiate media capabilities between peers before establishing a connection.
- **ICE (Interactive Connectivity Establishment)**  
ICE is a framework used for NAT traversal mechanism. ICE collects all available candidates (local IP addresses, reflexive addresses – STUN ones and relayed addresses – TURN ones). All the collected addresses are then sent to the remote peer via SDP.
- **STUN server**  
The STUN server allows clients to find out their public address, the type of NAT they are behind and the Internet side port associated by the NAT with a particular local port.
- **TURN server**  
TURN is used to relay media via a TURN server when the use of STUN isn’t possible.

## WebRTC is not all about peer to peer

- **Mesh**  
{% include figure image_path="/assets/images/webrtc/01-01--what-is-webrtc/webrtc-mesh.png" alt="this is a placeholder image" caption="Mesh topology" %}
In Mesh network all peers send their stream directly to other connected peers in network individually.

- **SFU**  
{% include figure image_path="/assets/images/webrtc/01-01--what-is-webrtc/webrtc-sfu.png" alt="this is a placeholder image" caption="SFU topology" %}
SFU stands for Selective Forwarding Unit. An SFU receives the incoming media streams from all users, and then decides which streams to send to which users.

- **MCU**  
{% include figure image_path="/assets/images/webrtc/01-01--what-is-webrtc/webrtc-mcu.png" alt="this is a placeholder image" caption="MCU topology" %}
MCU stands for Multipoint Conferencing Unit. An MCU receives the incoming media streams from all users, decodes it all, creates a new layout of everything and sends it out to all users as a single stream.
