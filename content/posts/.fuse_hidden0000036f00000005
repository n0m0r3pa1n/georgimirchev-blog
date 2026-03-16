---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:40"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "84034142849"
author: gmirchev90
categories:
  - android
date: "2023-05-14T07:06:30+00:00"
guid: https://georgimirchev.com/?p=1326
parent_post_id: null
post_id: "1326"
summary: |-
  There are a couple of downsides we figured out in our daily work with Firebase Realtime Database for Android and I would like to share them with you here. All of them revolve around using the provided SDK for Android. So let's start.

  ## Data listeners cannot be stopped

  If you initiate a listener to observe some data like this one for example:

  ```
  mDatabase.child("users").child(userId).get().addOnSuccessListener {
      Log.i("firebase", "Got value ${it.value}")
  }.addOnFailureListener{
      Log.e("firebase", "Error getting data", it)
  }
  ```

  Then you may need to cancel it if your user leaves the screen where this data is presented. There is a cancel() method that you can call but unfortunately, this cancel method only removes the listener callback but does not cancel the operation going on in the background and lets it run until this completes.

  Now imagine if you require a good big chunk of data for the screen and the user just does not want to wait because he has a 3G connection. You call cancel but the operation is still there in the background. You can verify logcat for that. What happens is that this operation still uses RAM memory that your app may need for something else and you can easily hit an OOM exception.

  So for querying big datasets, it is best if you have a dedicated server that returns only the data that you need to work with.
tags:
  - firebase
  - firestore
  - flow
  - kotlin
  - realtime-database
timeline_notification: "1684047992"
title: Downsides of Firebase Realtime DB for Android
url: /2023/05/14/downsides-of-firebase-realtime-db-for-android/
wordads_ufa: s:wpcom-ufa-v3-beta:1684048185

---
There are a couple of downsides we figured out in our daily work with Firebase Realtime Database for Android and I would like to share them with you here. All of them revolve around using the provided SDK for Android. So let's start.

## Data listeners cannot be stopped

If you initiate a listener to observe some data like this one for example:

```
mDatabase.child("users").child(userId).get().addOnSuccessListener {
    Log.i("firebase", "Got value ${it.value}")
}.addOnFailureListener{
    Log.e("firebase", "Error getting data", it)
}
```

Then you may need to cancel it if your user leaves the screen where this data is presented. There is a cancel() method that you can call but unfortunately, this cancel method only removes the listener callback but does not cancel the operation going on in the background and lets it run until this completes.

Now imagine if you require a good big chunk of data for the screen and the user just does not want to wait because he has a 3G connection. You call cancel but the operation is still there in the background. You can verify logcat for that. What happens is that this operation still uses RAM memory that your app may need for something else and you can easily hit an OOM exception.

So for querying big datasets, it is best if you have a dedicated server that returns only the data that you need to work with.

## Pagination is not supported out of the box

I have always had the feeling that when Google develops a product, it should also allow a very easy-to-use integration for it. They have everything needed to provide seamless integration between Firebase and Android apps:

- Firebase is a product they own
- Pagination library built by them
- Android is a platform they bought and own

Unfortunately, even today you still have to create a manual custom integration of paging + Firebase. There is no real-time updates support so you have to place a listener somewhere and then refresh the data. Still a lot of work when you have all of the ingredients in place.

## No control of timings

There is no way you can set the timeout of a request or control the retry mechanism in place. If you block Firebase in Charles Proxy, you will see it retries after around 40 seconds and then maybe a minute or more. But what if you want to receive a response faster or control the retry overall. It seems there is no such way right now.

## Detailed queries with lots of filtering options

Sometimes you need two collections merged into one. You may also need a where statement with some more filtering available.

With Firestore BOM 32.0.0, we have this new Filtering option to actually properly filter the data that we want to get. Feel free to check out the Google I/O 2023 announcement here -> [https://io.google/2023/program/d2aa0ce5-06e2-4f01-b67c-3a5dea8b9caa/](https://io.google/2023/program/d2aa0ce5-06e2-4f01-b67c-3a5dea8b9caa/)

{{< figure src="/wp-content/uploads/2023/05/image.png?w=1024" alt="" caption="" >}}

## Still good old callbacks

Well, this may be an obvious one but as Kotlin support is now recommended for buildscripts, why are we still using the callback mechanism to get some data. Can't we just have Flow's in place? Maybe this is for the Firebase team to answer.

## OutOfMemory is your closest enemy

Now that you have Firebase Android SDK integrated, OOM errors are your closest enemy. They can happen anytime and it is very hard to track where is the exact issue. You can enable debugging on Firebase but besides giving you huge amounts of logs full with JSON data, it doesn't help much unless you really dig into it. It would be so useful if there was a tool indicating that "This is a big query", "This is the amount of data that Firebase has in memory", "You can optimize this query". But no, everything is left to the developer and sometimes you just get the feeling that the Android SDK is kind of left to its own faith.

## Conclusion

Still, Firebase is a great **tech** tool especially for startups. It helps you easily work with data and the real-time functionality is really precious. It just needs a bit more support from Google to put it in a really good shape. And the above list is a really good starting point.
