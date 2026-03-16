---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:21"
_publicize_job_id: "43820174086"
author: gmirchev90
categories:
  - desktop
date: "2020-05-04T11:12:11+00:00"
guid: http://georgimirchev.com/?p=128
parent_post_id: null
post_id: "128"
tags:
  - git
  - ubuntu
timeline_notification: "1588590732"
title: Ubuntu/Mint show git branch in terminal (bashrc edit)
url: /2020/05/04/ubuntu-mint-show-git-branch-in-terminal-bashrc-edit/

---
I just wanted to share with you how my terminal looks like and how to export the PS1 variable.

To achieve it for Ubuntu: sudo gedit ~/.bashrc

For Linux Mint: sudo gedit /etc/bash.bashrc

And paste the following there at the line before the last one:

```
export PS1='\[\033[01;32m\]\u@\h\[\033[01;34m\] \w\[\033[01;33m\]$(__git_ps1)\[\033[01;34m\] \$\[\033[00m\] '
```
