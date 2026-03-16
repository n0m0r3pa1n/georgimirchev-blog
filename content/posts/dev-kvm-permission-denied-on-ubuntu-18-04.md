---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:18"
_publicize_job_id: "43817341785"
author: gmirchev90
categories:
  - desktop
date: "2020-05-04T09:36:57+00:00"
guid: http://georgimirchev.com/?p=89
parent_post_id: null
post_id: "89"
summary: |-
  Edit (06.06.2018):{{ double-space-with-newline }}You can follow the [simple](https://web.archive.org/web/20191021224412/https://github.com/uw-it-aca/spacescout-android/wiki/1.-Setting-Up-Android-Studio-on-Ubuntu) [tutorial](https://github.com/uw-it-aca/spacescout-android/wiki/1.-Setting-Up-Android-Studio-on-Ubuntu) [here](https://web.archive.org/web/20191021224412/https://github.com/uw-it-aca/spacescout-android/wiki/1.-Setting-Up-Android-Studio-on-Ubuntu) to install KVM. Thanks to azbest!

  I got this error when I was trying to create an emulator:

  /dev/kvm permission denied
tags:
  - android
  - kvm
  - ubuntu
timeline_notification: "1588585017"
title: /dev/kvm permission denied on Ubuntu 18.04
url: /2020/05/04/dev-kvm-permission-denied-on-ubuntu-18-04/

---
Edit (06.06.2018):  
You can follow the [simple](https://web.archive.org/web/20191021224412/https://github.com/uw-it-aca/spacescout-android/wiki/1.-Setting-Up-Android-Studio-on-Ubuntu) [tutorial](https://github.com/uw-it-aca/spacescout-android/wiki/1.-Setting-Up-Android-Studio-on-Ubuntu) [here](https://web.archive.org/web/20191021224412/https://github.com/uw-it-aca/spacescout-android/wiki/1.-Setting-Up-Android-Studio-on-Ubuntu) to install KVM. Thanks to azbest!

I got this error when I was trying to create an emulator:

/dev/kvm permission denied

I did several things to fix it.

1. sudo apt install qemu-kvm
1. sudo chown /dev/kvm <your\_username>
1. Logout and Login again

The error should not be there. You could also create a permissions group and associate it with the file and add your current user to that group like:

1. sudo groupadd -g 123456 kvm
1. sudo chgrp -R kvm /dev/kvm
1. sudo adduser <your\_username> kvm

You can try this approach too.
