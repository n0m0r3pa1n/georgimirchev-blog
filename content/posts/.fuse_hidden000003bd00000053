---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:21"
_publicize_job_id: "43822580541"
author: gmirchev90
categories:
  - android
date: "2020-05-04T12:30:31+00:00"
guid: http://georgimirchev.com/?p=138
parent_post_id: null
post_id: "138"
summary: We made a simple design for one of our apps which included a hexagonal network of images which should contain profile pictures of the users who liked a certain app.
tags:
  - hexagonal
  - view
timeline_notification: "1588595434"
title: Fighting android canvas – Hexagonal imageview layout for Android
url: /2020/05/04/fighting-android-canvas-hexagonal-imageview-layout-for-android/

---
We made a simple design for one of our apps which included a hexagonal network of images which should contain profile pictures of the users who liked a certain app.

To achieve it after many research we found this:  
[https://github.com/raydac/jhexed/](https://github.com/raydac/jhexed/)

Which is a google java engine for using hexagonal layout. There aren’t many alternatives and the ones we found like [https://github.com/riotopsys/Hexgrid](https://github.com/riotopsys/Hexgrid) were suspected to have memory leaks. So using this hex engine we found that on OS versions prior Android 4.0 everything was fine. But on 4.0+ the canvas added additional padding at the bottom of the view. So it looked something like this:

{{< figure src="/wp-content/uploads/2020/05/48e2a46e-3177-11e5-9e0e-6bda60f7152a.png?w=614" alt="" caption="" >}}

We spent hours on this until we discovered the OS problem.. So as it says here there were some new things from version 14 and above in the canvas which you can check [HERE](http://www.myandroidsolutions.com/2014/04/08/android-canvas-issue-starting-api-level-14/).

To fix this issue we had to do the simplest thing – disable the hardware acceleration for the specific view.  You can check the ways to disable it [HERE](http://developer.android.com/guide/topics/graphics/hardware-accel.html).

After disabling it everything is perfect! If anyone is interested the issue is [HERE](https://github.com/raydac/jhexed/issues/1). Feel free to use the library. It’s great!
