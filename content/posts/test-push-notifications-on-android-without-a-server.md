---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:18"
_publicize_job_id: "43817120253"
author: gmirchev90
categories:
  - android
date: "2020-05-04T09:29:16+00:00"
guid: http://georgimirchev.com/?p=71
parent_post_id: null
post_id: "71"
summary: |-
  ## How I used to do it

  The biggest problem I have with push notifications and especially [GCM](https://web.archive.org/web/20190917083845/https://developers.google.com/cloud-messaging/) is testing them. Every time I want to test a notification, I would create a sample project in Google Developers Console, use the JSON there and then use [Postman](https://web.archive.org/web/20190917083845/https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=bg) to send HTTP requests to the device, after obtaining the device registration id and the token for the project.
tags:
  - notifications
  - test
timeline_notification: "1588584556"
title: Test push notifications on Android without a server
url: /2020/05/04/test-push-notifications-on-android-without-a-server/

---
## How I used to do it

The biggest problem I have with push notifications and especially [GCM](https://web.archive.org/web/20190917083845/https://developers.google.com/cloud-messaging/) is testing them. Every time I want to test a notification, I would create a sample project in Google Developers Console, use the JSON there and then use [Postman](https://web.archive.org/web/20190917083845/https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=bg) to send HTTP requests to the device, after obtaining the device registration id and the token for the project.

## How I do it now

Recently, a colleague of mine reminded me that push notifications are displayed using a broadcast receiver, meaning we could actually use the **adb** command tool to send fake broadcasts that are caught by the system and delivered to the app. Simple as that! Here is how a command looks like:

```
./adb shell am broadcast -a com.google.android.c2dm.intent.RECEIVE -n <your.app.package>/com.google.android.gms.gcm.GcmReceiver --es "key1" "123"
```

This command is in case you use Google Cloud Messaging as notifications provider. But whichever notifications provider you use, you can easily see the broadcast receiver registered in the manifest, copy its package and intent filter and just paste it into the command.

You may need to remove the:

```
 android:permission="com.google.android.c2dm.permission.SEND"
```

permission from the manifest if the command line tool doesn’t work. Having that parameter there, states we only want messages coming from activities having the SEND permission – which the command line tool doesn’t have.
