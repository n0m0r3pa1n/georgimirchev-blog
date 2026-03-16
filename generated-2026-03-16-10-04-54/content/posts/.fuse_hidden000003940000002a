---
_elasticsearch_data_sharing_indexed_on: "2024-04-12 08:17:37"
_jetpack_memberships_contains_paid_content: ""
_jetpack_memberships_contains_paywalled_content: ""
_last_editor_used_jetpack: block-editor
_publicize_job_id: "93499993010"
author: gmirchev90
categories:
  - android
date: "2024-04-12T08:13:40+00:00"
firehose_sent: "1712909621"
guid: https://georgimirchev.com/?p=1967
parent_post_id: null
post_id: "1967"
tags:
  - interceptors
  - ioexception
  - okhttp
  - retrofit
timeline_notification: "1712909621"
title: Throwing anything other than IOException in the OkHttp interceptor will crash your app
url: /2024/04/12/throwing-anything-other-than-ioexception-will-crash-your-app-in-the-okhttp-interceptor/
wordads_ufa: u:wpcom-ufa-v4:1712909922

---
One thing to note, maybe you don't know about it, is that OkHttp interceptors work with IOException. So if you decide to implement some kind of a retry mechanism - to refresh a token when you get a forbidden response code, to retry a request, or something else, keep in mind that if you throw anything different than IOException or a subclass of it, like InvalidStateException, your app will crash and the exception will not even reach the try-catch block of your code.

The reason for it can be found here -> [https://github.com/square/retrofit/issues/3505](https://github.com/square/retrofit/issues/3505)
