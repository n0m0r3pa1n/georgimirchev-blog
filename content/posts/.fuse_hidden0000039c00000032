---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:24"
_publicize_job_id: "43823800285"
author: gmirchev90
categories:
  - android
date: "2020-05-04T13:08:56+00:00"
guid: http://georgimirchev.com/?p=170
parent_post_id: null
post_id: "170"
summary: In the new version of the Google Play Services library there is a lot of new code for the material design which really made my app huge and it crossed the border of 65 536 methods.
tags:
  - google-play
timeline_notification: "1588597739"
title: How to download older version of Google Play Services?
url: /2020/05/04/how-to-download-older-version-of-google-play-services/

---
In the new version of the Google Play Services library there is a lot of new code for the material design which really made my app huge and it crossed the border of 65 536 methods.

So I found a solution to the problem in [stackoverflow](http://stackoverflow.com/questions/20982533/how-to-download-older-google-play-services). I will paste it here:

I realise that this is an old question, but I had the same problem today (for real devices not for the emulator) and have found a solution.

I know that you can add code to prompt the user to upgrade to the latest version (see the comment by @nathan-walters) but, for complicated reasons, in my case I need to work with whatever version the user already has installed on their device (if any) and hence prefer to compile/build with an older version of the client library (google-play-services\_lib)

So researching further, I unpacked the xml that Android SDK Manager uses to find and install the ‘extras’ packages including the latest version of Google Play Services.  
If you are interested start here: [https://dl-ssl.google.com/android/repository/addon.xml](https://dl-ssl.google.com/android/repository/addon.xml)

For the current version (as of Nov 10th 2014) that gives you the library for google-play-services\_lib at:

```
https://dl-ssl.google.com/android/repository/google_play_services_6171000_r21.zip
```

The older versions are in the same location on the google servers (although they could disappear at any time), so if you are prepared to move the current installed library aside and manually unpack an older zip then you can install an old version. I found the following:

- [https://dl-ssl.google.com/android/repository/google\_play\_services\_4132530\_r14.zip](https://dl-ssl.google.com/android/repository/google_play_services_4132530_r14.zip)
- [https://dl-ssl.google.com/android/repository/google\_play\_services\_4242030\_r15.zip](https://dl-ssl.google.com/android/repository/google_play_services_4242030_r15.zip)
- [https://dl-ssl.google.com/android/repository/google\_play\_services\_4323030\_r16.zip](https://dl-ssl.google.com/android/repository/google_play_services_4323030_r16.zip)
- [https://dl-ssl.google.com/android/repository/google\_play\_services\_5077000\_r18.zip](https://dl-ssl.google.com/android/repository/google_play_services_5077000_r18.zip)
- [https://dl-ssl.google.com/android/repository/google\_play\_services\_5089000\_r19.zip](https://dl-ssl.google.com/android/repository/google_play_services_5089000_r19.zip)
- [https://dl-ssl.google.com/android/repository/google\_play\_services\_6171000\_r21.zip](https://dl-ssl.google.com/android/repository/google_play_services_6171000_r21.zip)

The same technique would allow you to test against the non-upgradable older versions Google Maps (Google Play Services) hardwired in older versions of the emulator. I haven’t tested all these versions against the emulator but have successfully tested Google Maps in the emulator for an AVD based on Google APIs (API 19).

Google does make the occasional attempt to bring the emulator up to date. Here is an issue tracker thread discussing version incompatibility issues between the client library and the emulator and google’s various attempts to fix them: [https://code.google.com/p/android/issues/detail?id=57880](https://code.google.com/p/android/issues/detail?id=57880)
