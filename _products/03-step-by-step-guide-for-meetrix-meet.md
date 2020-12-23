---
title: "Step by Step Guide for Meetrix Meet"
permalink: /products/meetrix-meet/stemp-by-step-guide.html
excerpt: "The best Jitsi Based Conferencing System"
header:
  overlay_image: /assets/images/products/03-step-by-step-guide-for-meetrix-meet/meetrix-meet-dashboard.png
  teaser: /assets/images/products/03-step-by-step-guide-for-meetrix-meet/meetrix-meet-dashboard.png
  overlay_filter: 0.5
last_modified_at: 2018-09-24T15:58:49-04:00
toc: true
---
What is a Room
---

Room denotes a conferencing session where multiple users join and communicate with each other.
In Meetrix Meet, a Room has following properties

* Room Name
* Start Time
* End Time
* Users(Participants)


A room can only be accessed in between start time and end time by the assigned users.
Users who have not been added to the room cannot access the room anyway. Even the user has been added
to the room, he cannot access the room before the `Start Time` or after the `End Time`.

One of the participants can be assigned as the moderator of the conference room.


Roles in Meetrix Meet
---

In Meetrix Meet, there are three roles. Admin, Moderator and User.

#### <ins>Admin Role</ins>


Admin can perform following operations for rooms

{% include figure image_path="/assets/images/products/03-step-by-step-guide-for-meetrix-meet/admin-rooms-meetrix-meet.png" alt="this is a placeholder image" caption="Content-Type : text/css" %}

* Create Rooms
* Edit Rooms
* Add Users to Rooms
* Remove Users from Rooms
* Set Moderator Role for a user ins a Room
* Delete Rooms


And following operations for users

{% include figure image_path="/assets/images/products/03-step-by-step-guide-for-meetrix-meet/meetrix-meet-admin-users-section.png" alt="this is a placeholder image" caption="Content-Type : text/css" %}
{% include figure image_path="/assets/images/products/03-step-by-step-guide-for-meetrix-meet/meetrix-meet-user-settings.png" alt="this is a placeholder image" caption="Content-Type : text/css" %}

* Create Users
* Edit Users and User Account
* Delete Users


Also admin can perform following operations on Admin Users

{% include figure image_path="/assets/images/products/03-step-by-step-guide-for-meetrix-meet/meetrix-meet-admin-section.png" alt="this is a placeholder image" caption="Content-Type : text/css" %}
{% include figure image_path="/assets/images/products/03-step-by-step-guide-for-meetrix-meet/meetrix-meet-admin-account-section.png" alt="this is a placeholder image" caption="Content-Type : text/css" %}

* Create Admins
* Edit Admins
* Delete Admins


#### User Role

Users only can view the rooms that they have been assigned to and join the conference.


#### Moderator

The `Moderator` role comes in to effect only in a conference room.
`Admin` can nominate one of the participants of the conference room as the `Moderator` of the conference
can perform following actions during the conference

* Add a password to the conference
* Kick-out participants from the conference
* Mute/Unmute participants (This is an added feature to Jitsi Meet)
* Start Live Streaming the conference to youtube.
* Start Recording the conference.

Login
---

To login as an `Admin` you can use demo credentials provided on the [Meetrix Meet](https://meet.meetrix.io) landing page. 

Creating a Room
---

Once you logged in as the `Admin`, you can go to `All Rooms` tab to perform Room Operations. 
There, you can create, edit or delete rooms.

Assigning Users to a Room
---
{% include figure image_path="/assets/images/products/03-step-by-step-guide-for-meetrix-meet/meetrix-meet-admin-assign-users.png" alt="this is a placeholder image" caption="Content-Type : text/css" %}

Once you create a room, you can click `Manage` button on the actions menu. 
On the upper right corner, you will find the `Assign Users` button. Click on that and you will see the all available users in the system.
You can search and filter users based on different criterias such as `username` and `email`.
Once you click on the `Add to Room` button on the Actions Menu, the users will be added to the room.

How can a user join the room
---
{% include figure image_path="/assets/images/products/03-step-by-step-guide-for-meetrix-meet/meetrix-meet-admin-room-list.png" alt="this is a placeholder image" caption="Content-Type : text/css" %}

To join the room, users first has to login to the user dashboard. 
Either you can create and account or you can use the user credentials mentioned on the landing page to login as the user. 
Once you logged in, you will see `My Rooms` tab. There you will see al the rooms that you have been assigned to.
Click on the `initiate` button, to generate the unique link for the conference. After that, please click on 
`Visit` button to join the conference.

Simple as that !

If you are looking for a customized version of `Jitsi Meet` like `Meetrix Meet` shoot us an email to : `hello@meetrix.io`