---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:29"
_oembed_4f088581798e7339646b8d9e5e9f97e7: '{{unknown}}'
_publicize_job_id: "43864885982"
author: gmirchev90
categories:
  - android
date: "2020-05-05T12:44:33+00:00"
guid: http://georgimirchev.com/?p=244
parent_post_id: null
post_id: "244"
summary: We have all seen the white square at the top type of a notification in the notification bar.
tags:
  - icon
  - notifications
  - push-notifications
timeline_notification: "1588682676"
title: Android Notification White Square Icon
url: /2020/05/05/android-notification-white-square-icon/

---
We have all seen the white square at the top type of a notification in the notification bar.

Well, it is pretty easy to fix it if you are using Firebase, but still, using a colored icon won’t fix it. You can try to add this metadata:

```
<meta-data
         android:name="com.google.firebase.messaging.default_notification_icon"
            android:resource="@mipmap/ic_launcher" />
```

and it will probably work on some devices. But to really fix it you need to make a specific white icon for it. Here is the [LINK](https://developer.android.com/guide/practices/ui_guidelines/index.html) & [LINK](https://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html) to the Google icon guidelines. Otherwise, your app will default to the white square icon. And [HERE](http://stackoverflow.com/questions/37325051/notification-icon-with-the-new-firebase-cloud-messaging-system) is the issue from StackOverflow. So better make a white icon and use it in the notification builder or in the AndroidManifest file or it will not show.
