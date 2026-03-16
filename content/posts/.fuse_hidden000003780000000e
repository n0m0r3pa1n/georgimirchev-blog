---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:18"
_publicize_job_id: "43817211780"
author: gmirchev90
categories:
  - desktop
date: "2020-05-04T09:32:30+00:00"
guid: http://georgimirchev.com/?p=81
parent_post_id: null
post_id: "81"
tags:
  - inotify
  - npm
  - ubuntu
timeline_notification: "1588584750"
title: “No space left on device” message with npm &amp; Ubuntu 18.04
url: /2020/05/04/no-space-left-on-device-message-with-npm-ubuntu-18-04/

---
I got the “No space left on device” message when trying to run a React website using npm. The fix was fast and easy following [THIS advice:](https://web.archive.org/web/20191021221952/https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers)

```
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```
