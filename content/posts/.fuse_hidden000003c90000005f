---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:23"
_publicize_job_id: "43823398835"
author: gmirchev90
categories:
  - android
date: "2020-05-04T12:57:12+00:00"
guid: http://georgimirchev.com/?p=158
parent_post_id: null
post_id: "158"
summary: Recently, I got a task to implement face detection for a camera preview we have within our mobile app. So if the app detects a face, a button should be displayed, otherwise – the button should remain hidden. There are several ways to implement face detection in Android
tags:
  - camera
  - face-detection
  - opencv
  - vision-api
timeline_notification: "1588597036"
title: Implementing Face Detection in Android
url: /2020/05/04/implementing-face-detection-in-android/

---
Recently, I got a task to implement face detection for a camera preview we have within our mobile app. So if the app detects a face, a button should be displayed, otherwise – the button should remain hidden. There are several ways to implement face detection in Android

## Ways to implement face detection

1. OpenCV – implement manually face detection using OpenCV for Android. Here are [Sample 1](https://github.com/opencv/opencv/tree/master/samples/android/face-detection) and [Sample 2](http://opencv.org/platforms/android/opencv4android-samples.html)
1. Camera – use the built-in functionality for face detection in the [Camera API](https://developer.android.com/guide/topics/media/camera.html#face-detection)
1. Vision API – integrate [Google Vision API](https://github.com/googlesamples/android-vision) in your Android app.

## Drawbacks of each approach

### OpenCV

Integrating OpenCV lib will slow down your build process. It will take more time to build the whole project. It is also very big so integrating a huge library just for face recognition may not be something you will be keen on.

### Camera API

Using the default camera face detection API is not a good choice. It is not supported by some devices like Moto G (2nd generation) and probably many others. It just does not return recognized faces and that may be something your app functionality depends on.

### Vision API

Vision API works flawlessly with many and different phones but the only drawback is that it requires the phone to download some libs before using the functionality. The download period takes no more than 2-3 seconds, but that may be too long for you app to wait.

## Ease of integration

All APIs are easy to integrate and won’t require a big hassle.

## Preferred approach for us

We decided to go with Vision API. It has good documentation and is well tested. It supports all of the phones we tested it on and has no problem detecting faces. It is free and has no usage limitations. And after all, it is a Google library and the sample made the integration very easy. Google use their own camera source behind the scenes so hopefully, everything should work. Feel free to try OpenCV too. But the standard built-in camera API is not reliable! Every other choice is a better one!
