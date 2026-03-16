---
_elasticsearch_data_sharing_indexed_on: "2024-08-22 11:54:01"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "97224095582"
author: gmirchev90
categories:
  - android
date: "2024-08-22T11:54:52+00:00"
firehose_sent: "1724327693"
guid: https://georgimirchev.com/?p=2274
parent_post_id: null
post_id: "2274"
summary: |-
  ```
  Resolving javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path building failed
  ```

  Recently, I tried to build an Android project and it failed with the above error. It was a super nasty error to fix and I spent quite some time on it.
tags:
  - android-studio
  - java
  - macos
timeline_notification: "1724327693"
title: Android app fails to build with SSLHandshakeException
url: /2024/08/22/android-app-fails-to-build/

---
```
Resolving javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path building failed
```

Recently, I tried to build an Android project and it failed with the above error. It was a super nasty error to fix and I spent quite some time on it.

Actually, there was only one fix that worked for me. **Install a new Java environment or JDK and set it as a default Java instance to be used**. Re-start Android Studio with Invalidate Cache & Restart and then in Settings -> Build, Execution and Deployment change the Java version to be used and you should be good to go.
