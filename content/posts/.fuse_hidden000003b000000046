---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:18"
_publicize_job_id: "43817189834"
author: gmirchev90
categories:
  - desktop
date: "2020-05-04T09:31:42+00:00"
guid: http://georgimirchev.com/?p=78
parent_post_id: null
post_id: "78"
tags:
  - grub
  - ubuntu
timeline_notification: "1588584703"
title: Installing Grub fails on Ubuntu 16.04
url: /2020/05/04/installing-grub-fails-on-ubuntu-16-04/

---
I got this nasty error when it came to having a fresh install of Ubuntu 16.04. It says that GRUB cannot be installed on /target device. In order to fix this you need to:

1. Create a partition around 300 mbs
1. Use the FAT32 format and target it as EFI partition
1. Choose that the GRUB installer should be installed on this 300 mbs partition

Then the installation should finish succesfully without any errors.
