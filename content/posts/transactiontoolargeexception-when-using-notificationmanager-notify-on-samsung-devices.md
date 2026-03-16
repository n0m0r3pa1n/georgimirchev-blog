---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:17"
_publicize_job_id: "43817061098"
author: gmirchev90
categories:
  - android
date: "2020-05-04T09:27:07+00:00"
guid: http://georgimirchev.com/?p=67
parent_post_id: null
post_id: "67"
summary: |-
  Recently, we got a bug inside the app. The app was crashing in the push notification receiver class that we had on the line where we called

  `notificationManager.notify(notificationId, notificationBuilder.build())`.
tags:
  - notifications
timeline_notification: "1588584427"
title: TransactionTooLargeException when using NotificationManager.notify on Samsung devices
url: /2020/05/04/transactiontoolargeexception-when-using-notificationmanager-notify-on-samsung-devices/

---
Recently, we got a bug inside the app. The app was crashing in the push notification receiver class that we had on the line where we called

`notificationManager.notify(notificationId, notificationBuilder.build())`.

The problem is that we got an exception logged in Crashlytics that was a TransactionTooLargeException pointing to a Bundle that was with size over 1 mb. After few hours of debugging, I wrote a bash script that sends multiple notifications one after another. It looked like this:

```
#!/bin/bash
var='/usr/programs/android-sdk/platform-tools/adb shell am broadcast -a com.google.android.c2dm.intent.RECEIVE -n /com.google.android.gms.gcm.GcmReceiver -e "triggeringNodeId" "df08c523-b1e5-4199-8caf-5b5341bc8b1d" -e "body" "wText" -e "title" "wwwwwwwwwwwwwwwwwwwwwwwww"'
while sleep 0; do eval $var; done
```

You can check [this article](http://georgimirchev.com/2020/05/04/test-push-notifications-on-android-without-a-server/) in order to see how to send push notifications using ADB.

After that, I saw that the app actually crashed with the TransactionTooLargeException. The problem was right here:

```
 NotificationCompat.InboxStyle inboxStyle = new NotificationCompat.InboxStyle();
 inboxStyle.setSummaryText(mContext.getString(R.string.notification_summary_subscript));

 // display notifications in reverse chronological order
 for (int i = notificationData.getCount() - 1; i >= 0; i--) {
     inboxStyle.addLine(notificationData.getBody(i));
 }
```

We had an edge case where the notificationData count was over 100 lines, so adding so many lines would crash on most Samsung devices with Android 7+. In order to fix it, I just limited the line count to the last 10. I hope this will help someone else having this problem.

An additional thing that may cause the same problem is the case where your reuse the same notification builder instead of replacing it with a new one. If you still continue to see the error, just use a new notification builder each time when you update or create a push notification.
