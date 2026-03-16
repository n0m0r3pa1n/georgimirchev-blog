---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:38"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "71394395186"
author: gmirchev90
categories:
  - android
date: "2022-04-15T05:11:26+00:00"
guid: https://georgimirchev.com/?p=757
parent_post_id: null
post_id: "757"
summary: Let's admit - the Android navigation library is a really cool thing but it has its own issues and drawbacks. One of the issues you can have is the TransactionTooLargeException. And here is how it happens.
tags:
  - android-navigation-library
  - backstack
  - fragment
timeline_notification: "1649999489"
title: Android navigation library and TransactionTooLargeException
url: /2022/04/15/android-navigation-library-and-transactiontoolargeexception/

---
Let's admit - the Android navigation library is a really cool thing but it has its own issues and drawbacks. One of the issues you can have is the TransactionTooLargeException. And here is how it happens.

## The case

Let's say you have a payment app or a video app. You have a kind of a circular flow of screens in the app. Examples:

1. Doing multiple payments one after another
1. Viewing the details of a video and then choosing another video to watch, then another, then another

In these cases you can have something like:

Details screen -> List screen -> Details screen -> List screen and this can go on forever.

Or it can be even a bit more complex like:

Pay screen -> Gratuity screen -> Donation screen -> Complete screen -> Pay screen

You can see the circle here, right? Well, you will be amazed when one day you start receiving the TransactionTooLargeException for your app. Let's say you pass the data through the fragment bundle and this data continues to grow and grow. And with it, the backstack grows too if you have not specified **popUpToInclusive** and **popUpTo** props.

What happens is that on each fragment navigation the Android navigation library just creates a new fragment and pushes it to the backstack. This way this backstack continuously grows until the user decides to exit this circular flow. And what will happen if the user dims the screen while he has pushed 20 fragments on the backstack? You guessed it right - TransactionTooLargeException.

## How to avoid the exception?

Well, there are several things you can do:

1. Figure out a point in the flow that the user cannot go back from. When you try to reach it always use **popUpToInclusive** and **popUpTo** to clear all fragments from the backstack. From that point on you will start fresh with an empty backstack until the circular flow reaches this screen again.
1. Don't pass a huge amount of data through the fragment bundle. Always have in mind that the user can minimize the app at any given time.
1. Use launchSingleTop for fragments - don't let the system open separate instances of a given fragment

## One nasty pitfall of the navigation library

There is the one really nasty pitfall of the navigation library. Let's say you specify **popUpTo** to a certain fragment. **If this fragment is not in the backstack, the navigation library won't clear anything and your backstack will continue to grow**. Why do I say this is nasty? Because it doesn't give any indication to the developer that the fragment it tries to pop up to is missing. It logs in the logcat but we all know how polluted the logcat is and how hard it is to follow. That's why just make sure you always popUpTo a fragment that is in the backstack.

## Debugging fragment backstack

If you want to monitor your app's backstack, you can use the following snippet:

```
navController.addOnDestinationChangedListener { controller, destination, arguments ->
    val navHostFragment = supportFragmentManager.findFragmentById(R.id.nav_host_fragment)
    val backStackEntryCount = navHostFragment?.childFragmentManager?.backStackEntryCount

    Timber.e("BACKSTACK SIZE : $backStackEntryCount}")
}
```

Hope this helps and it is useful for everyone facing the issue with the navigation library!
