---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:18"
_publicize_job_id: "43817167617"
author: gmirchev90
categories:
  - desktop
date: "2020-05-04T09:30:56+00:00"
guid: http://georgimirchev.com/?p=75
parent_post_id: null
post_id: "75"
tags:
  - app
  - launcher
  - ubuntu
  - unity
timeline_notification: "1588584656"
title: Add application to launcher in Ubuntu 16.04
url: /2020/05/04/add-application-to-launcher-in-ubuntu-16-04/

---
Just use a program called alacarte:

`sudo apt-get install alacarte`

Click on the New Item button, write the command & name of the app, choose a logo if you want and you will see it appearing shortly in the Unity launcher by typing the name. Otherwise, you will have to create a \*.desktop file and add it to .local/share/applications which I couldn’t make it work after spending a couple of hours.
