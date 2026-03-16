---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:27"
_publicize_job_id: "43830977406"
author: gmirchev90
categories:
  - android
date: "2020-05-04T16:34:42+00:00"
guid: http://georgimirchev.com/?p=221
parent_post_id: null
post_id: "221"
tags:
  - adb
  - genymotion
timeline_notification: "1588610083"
title: Android adb server version (32) doesn’t match this client (Genymotion)
url: /2020/05/04/android-adb-server-version-32-doesnt-match-this-client-genymotion/

---
Probably you have seen this type of error when using Genymotion as the Android emulator on which you test your device. This is how the error looks like:

adb server version (32) doesn’t match this client (35); killing…  
error: could not install \*smartsocket\* listener: Address already in use  
ADB server didn’t ACK  
\\* failed to start daemon \*

There is one simple thing that worked for me. I opened the Settings menu of Genymotion, then ADB tab and selected: Use custom android SDK tools and I set the path to the tools. By default Genymotion uses its built-in tools. After that killing all working emulators and starting them again makes everything  work.

Hope I have helped!
