---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:27"
_publicize_job_id: "43833266528"
author: gmirchev90
categories:
  - desktop
date: "2020-05-04T17:42:42+00:00"
guid: http://georgimirchev.com/?p=224
parent_post_id: null
post_id: "224"
tags:
  - guake
  - terminal
  - ubuntu
timeline_notification: "1588614163"
title: Guake Terminal for Ubuntu not opening another terminal in the same folder
url: /2020/05/04/guake-terminal-for-ubuntu-not-opening-another-terminal-in-the-same-folder/

---
I use Guake for Ubuntu as a terminal. And there is this option in the Guake properties to set it to open the same directory you were in if you want another terminal. This is just now working with the latest version of Guake. To fix it just run the following:

gconftool-2 –set /apps/guake/general/open\_tab\_cwd –type=boolean true

And it will add a variable saying that you want the same folder opened when you open another terminal. 🙂

The bug can be found here: [https://github.com/Guake/guake/issues/578](https://github.com/Guake/guake/issues/578)
