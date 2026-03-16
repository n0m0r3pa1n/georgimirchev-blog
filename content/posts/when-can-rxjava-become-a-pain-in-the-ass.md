---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:32"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "54717629153"
author: gmirchev90
categories:
  - android
date: "2021-02-13T12:32:42+00:00"
guid: http://georgimirchev.com/?p=363
parent_post_id: null
post_id: "363"
summary: The idea of this post is really simple. If you don't know the details around RxJava - don't implement it in a project which has the idea of earning money.
tags:
  - rxjava
timeline_notification: "1613219565"
title: When can RxJava become a pain in the ass?
url: /2021/02/13/when-can-rxjava-become-a-pain-in-the-ass/

---
The idea of this post is really simple. If you don't know the details around RxJava - don't implement it in a project which has the idea of earning money.

## Let me tell you a little story

### The beginning

Once upon a time there were these guys, who thought that having a player app with a playlist of songs / podcasts was a cool idea. They went to a contracting company and this is where shit gets real. The contracting company talks about stuff like architecture, dependency injection, Android lifecycle and stuff like these but when they started the project they just wrapped everything in RxJava and never did anything that they were thinking about.

### The complexities

The app has a lot of complexities. And most of them are related to real time processing. The user can login/logout. He uses the oauth2 mechanism and if he is a premium one - he can access some premium songs / podcasts that otherwise he would be able to play. You need to know the progress of each song, display a progressbar to the user, not allow the system to kill your player service and the best part of it - refresh your app token if the app is streaming the mp3. Why is that needed? Because you can end up in a case where your token expires, the stream tries to fetch the next part of the mp3 file but it fails as the token is invalid.

### The RxJava solution

With so many real time requirements there is this obvious solution that comes to mind, right? RxJava Observable / Subject. You can use them to emit all kinds of stuff. It really looks like using an EventBus but with a more proper tracking in place so you could follow the code a bit easier. But what is the price of it? Let's see it.

## The RxJava price tag

When you don't know much about RxJava, it is very easy to get into the deep shit where things just get out of control. What I mean?

### Disposing a stream

STREAMS MUST BE DISPOSED! You cannot rely on the GC to grab your disposable and kill it. This is just ridiculous. You must dispose a stream at the appropriate lifecycle event and there are many solutions to that. So inside your ViewModel in onClear or inside your presenter by using a LifecycleObserver of onDestroy, the stream must be killed! Why?

1. You reduce memory usage and there are no stuck objects in memory
1. Reduced CPU usage as when the app is destroyed nothing needs to keep working
1. When a screen is destroyed and you have forgotten that you were using a Singleton class inside of it, that Singleton is just stuck there. Maybe it keeps a reference to the context, maybe it just does a network request that was not needed to be executed, this has a huge potential for memory leaks
1. You get a guarantee that your app won't rely on state that is not there. The system may kill the app anytime and you need to make sure you don't rely on that singleton that should be there but it is not.

### Subjects are not trustworthy

Wrapping everything in subjects can really become a bad idea. If you have a BehaviorSubject or PublishSubject, you can easily get into trouble when you put too much responsibilities on them. What I mean by that is that you never have a guarantee when the system will kill your app. And if you listen for a subject emission in your UI, then you are really close to getting fucked up. On app restore this subject may not have the data if it wasn't stored in shared prefs before that, let's say. Subjects that are used through the app should be always thought about in the following ways:  
 \- What if the app is killed? Can it make the app hang if it doesn't have the data?  
 \- Should the subject use .startWith and always start with some stored data?

### Concurrency issues

RxJava is a godlike library when it comes to handling stuff on different threads. But when it comes to a thing that needs to be executed only once even if it gets subscribed to 50 times in 10 seconds, then it can be a pain in the ass to understand what happens. Code which is syncrhonized is way more readable and easy to understand.

#### Debugging streams which stay there forever

Let's say you have a stream that emits every 1 second and checks whether the player is playing or not. Finding such streams that were forgotten to be disposed is really really hard. This is a thing that comes to mind when you see your app memory usage.

## RxJava the proper way

**You have to do the job**

RxJava doesn't care whether it runs on Android or on your desktop PC. It is your job as a software developer to introduce the proper architecture that conforms to where you want to use the RX library.

**Android architecture with RxJava**

Never use RxJava in your UI layer so you don't end up with messed up state. There are so many ways to use it with MVVM or MVP. Clear the disposables using a lifecycle observer or using onClear in the Android ViewModel component. Find the proper places and create some base classes.

**Always think about stream state**

A stream can be in one of the following state:  
 \- Success  
 \- Failure  
 \- Completed  
 \- Disposed

Never miss them and think about all of them together. If you make a compromise with one of them - it can lead to memory leaks or app crashes.

#### Do you really need to cache it?

Caching in RxJava is easy but tricky. You can just use the cache() operator and then you can rely that the data will be there when you fetch it. But what about if the user puts the app to the background and opens it from recents apps 40 minutes later? Do you think the data will be there?

#### Knowledge is essential

The other thing is that you must have a really good knowledge how the operators work in RxJava. You flatMap to a Maybe and you expect to get a result and instead you don't get anything. The Maybe can complete without having a result and this wasn't something that you expected?. Or you flatMap [an Observable to a Single](https://proandroiddev.com/rxjava-observable-to-single-49c2a14da625) and you expect the stream to complete but it never completes. Guess why?

## Summary

RxJava is a powerful beast that you should know how to handle before adding it to an app that is 5 screens in total. It should not be present in the UI layer, should be hidden behind repositories and interactors/use cases and should be used with caution. Always dispose streams when they are not needed and don't forget that your app can be killed at any time. States (Success, Error, Completed, Disposed) should be handled appropriately and should conform to the app lifecycle. If you need it inside the UI layer, maybe it is time to introduce a proper architecture in place.
