---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:37"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "66228198017"
author: gmirchev90
categories:
  - desktop
date: "2021-12-07T12:43:17+00:00"
guid: https://georgimirchev.com/?p=675
parent_post_id: null
post_id: "675"
summary: I guess I updated recently to the latest linux-headers-kernel and my laptop decided to surprise me one morning. It just didn't boot up. It got stuck at the point where it said it couldn't find the dkms.conf file.
tags:
  - kernel
  - kubuntu
  - ubuntu
  - updates
timeline_notification: "1638880999"
title: Ubuntu not starting due to Kernel panic error - Could not locate dkms file
url: /2021/12/07/ubuntu-not-starting-due-to-kernel-panic-error-could-not-locate-dkms-file/

---
I guess I updated recently to the latest linux-headers-kernel and my laptop decided to surprise me one morning. It just didn't boot up. It got stuck at the point where it said it couldn't find the dkms.conf file.

I found [this](https://askubuntu.com/questions/227258/error-could-not-locate-dkms-conf-file) StackOverflow question really useful.

Basically what I did is to launch it using an old Kernel by holding the **Shift** button and entering the Grub menu. There you selected the "Advanced" option and pick up an older kernel header.

It launched successfully, then it was simple. Just run this command to see what the issue is:

```
for i in /var/lib/dkms/*/[^k]*/source; do [ -e "$i" ] || echo "$i";done
```

In my case, it was the [rts\_pstor](https://github.com/chrisnew/rts_pstor) package that I tried to install recently for my SD card reader on my laptop. I removed it and then ran:

```
sudo apt-get autoremove
sudo apt-get install
```

And after the process finished successfully, restart and everything is back to normal.
