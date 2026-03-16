---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:18"
_publicize_job_id: "43817302346"
author: gmirchev90
categories:
  - desktop
date: "2020-05-04T09:35:35+00:00"
guid: http://georgimirchev.com/?p=86
parent_post_id: null
post_id: "86"
summary: I faced this bug in Ubuntu 18.04. When I used Alt + Shift + 7 inside Webstorm which is the command for Find Usages, it actually changed the language because the command included Alt + Shift (I previously set this by using Gnome Tweak Tool). Here is a bug related to this in [Launchpad](https://web.archive.org/web/20190709014520/https://bugs.launchpad.net/ubuntu/+source/xorg-server/+bug/1683383).
tags:
  - language
  - ubuntu
timeline_notification: "1588584938"
title: Ubuntu – Alt + Shift + any key switches the language and does not work
url: /2020/05/04/ubuntu-alt-shift-any-key-switches-the-language-and-does-not-work/

---
I faced this bug in Ubuntu 18.04. When I used Alt + Shift + 7 inside Webstorm which is the command for Find Usages, it actually changed the language because the command included Alt + Shift (I previously set this by using Gnome Tweak Tool). Here is a bug related to this in [Launchpad](https://web.archive.org/web/20190709014520/https://bugs.launchpad.net/ubuntu/+source/xorg-server/+bug/1683383).

To fix this you need to execute the following commands:

```
sudo add-apt-repository ppa:nrbrtx/xorg-hotkeys
sudo apt-get update
sudo apt-get dist-upgrade

```

and then restart your laptop. This works, but sadly now, whenever you use Alt + Shift + 7, it will search for usages and then change the language.
