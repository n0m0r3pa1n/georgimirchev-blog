---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:27"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "43833380013"
author: gmirchev90
categories:
  - desktop
date: "2020-05-04T17:46:18+00:00"
guid: http://georgimirchev.com/?p=227
parent_post_id: null
post_id: "227"
tags:
  - chrome
  - fullscreen
  - lubuntu
timeline_notification: "1588614378"
title: Fix Lubuntu 14.04 Chromium fullscreen mode
url: /2020/05/04/fix-lubuntu-14-04-chromium-fullscreen-mode/

---
I bought an old Dell D430 laptop which is pretty old and small (12″) and I installed Lubuntu 14.04.02

The problem is that the fullscreen mode on Chromium and Chrome is not working as expected. It hides the application bar at the bottom but does not allow the browsers to go fullscreen – tabs and address bar are staying in their place when you press F11.

The problem is that the F11 key is taken from the system to make the current focused window fullscreen. You can see it here:

[https://help.ubuntu.com/community/Lubuntu/Keyboard](https://help.ubuntu.com/community/Lubuntu/Keyboard)

1. Just go to **~/.config/openbox**
1. Backup the **lubuntu-rc.xml** file
1. Edit the original one replacing the F11 shortcut for toggle fullscreen with F10 for example
1. Logout and login again

And now when you press F11 in chrome or chromium it will go fullscreen, instead of using the default system shortcut.
