---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:28"
_publicize_job_id: "43833557882"
author: gmirchev90
categories:
  - desktop
date: "2020-05-04T17:51:58+00:00"
guid: http://georgimirchev.com/?p=237
parent_post_id: null
post_id: "237"
summary: I want to stop the update manager from popping up in Ubuntu because I think that they release bad updates sometimes and really make the OS unstable. This is my personal opinion after 3 reinstallations in 2 months.
tags:
  - ubuntu
  - updates
timeline_notification: "1588614718"
title: Disabling update manager popup in Ubuntu
url: /2020/05/04/disabling-update-manager-popup-in-ubuntu/

---
I want to stop the update manager from popping up in Ubuntu because I think that they release bad updates sometimes and really make the OS unstable. This is my personal opinion after 3 reinstallations in 2 months.


First of all I stopped the updates in the Software tab and then I ran the following script:

```
sudo sed -i 's/NoDisplay=true/NoDisplay=false/g' /etc/xdg/autostart/*.desktop
```

This script opens each file in the /etc/xdg/autostart directory with .desktop extension and changes the NoDisplay to false if it is to true. That makes all of the apps visible in the Startup Applications dialog. You have more apps running than you see. 😉

So you can untick the update manager from there and it won’t start each time your OS starts.  
Thanks to [Ubuntu forums](http://ubuntuforums.org/showthread.php?t=1966228).
