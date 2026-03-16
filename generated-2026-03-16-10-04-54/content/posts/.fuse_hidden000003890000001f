---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:26"
_publicize_job_id: "43830825513"
author: gmirchev90
categories:
  - android
date: "2020-05-04T16:30:20+00:00"
guid: http://georgimirchev.com/?p=215
parent_post_id: null
post_id: "215"
summary: |-
  ## card.io 5.4.2

  [Card.io](https://www.card.io/) is a card recognition SDK used by PayPal which allows you to easily scan your own credit/debit cards and get information about them in the code.

  ## Problem

  Using card.io from the source and not from Maven requires you to have the latest Android NDK with which the project will be built. But compiling it with the latest NDK introduces a bug which prevents the library from supporting several phones like Samsung S5, Honor 7 and others.
tags:
  - androidndk
  - cardio
timeline_notification: "1588609820"
title: Card.io issue not supporting Samsung S5 and other devices
url: /2020/05/04/card-io-issue-not-supporting-samsung-s5-and-other-devices/

---
## card.io 5.4.2

[Card.io](https://www.card.io/) is a card recognition SDK used by PayPal which allows you to easily scan your own credit/debit cards and get information about them in the code.

## Problem

Using card.io from the source and not from Maven requires you to have the latest Android NDK with which the project will be built. But compiling it with the latest NDK introduces a bug which prevents the library from supporting several phones like Samsung S5, Honor 7 and others.

## Solution

The solution is to manually download the NDK 11c and use it to compile the card.io library.

### Steps

- Check the NDK revisions [HERE](https://developer.android.com/ndk/downloads/revision_history.html)
- Copy the download link from [HERE](https://developer.android.com/ndk/downloads/index.html)
- Replace the name of the version in the download link – NDK 11c Linux\_x64 link [HERE](https://dl.google.com/android/repository/android-ndk-r11c-linux-x86_64.zip)
- Open your SDK folder and replace the content of the ndk-bundle folder with the one you downloaded
- Build your project in the Android Studio. No need to restart.

For verification open the SDK Manager -> SDK Tools page and see the version of the NDK there. Card.io should work on most phones after building it with NDK 11c.
