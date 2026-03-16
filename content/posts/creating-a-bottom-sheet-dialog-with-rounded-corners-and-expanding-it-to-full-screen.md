---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:35"
_last_editor_used_jetpack: block-editor
_oembed_8d816d60082066a017bbafc58bfe2c72: '{{unknown}}'
_oembed_9b4cbaf0eda3e2a594ee99d041555bcd: '{{unknown}}'
_publicize_job_id: "60375582883"
_thumbnail_id: "534"
author: gmirchev90
categories:
  - android
cover:
  alt: title
  image: /wp-content/uploads/2021/07/title.png
date: "2021-07-05T09:10:46+00:00"
guid: https://georgimirchev.com/?p=523
parent_post_id: null
post_id: "523"
summary: Sometimes even simple and easy stuff can be complex on Android. Such is the case where you want to have a bottom sheet dialog with rounded corners which expands to fullscreen. Here is how I did it on Android 6.
tags:
  - bottomsheetdialog
  - fullscreen
  - rounded-corners
timeline_notification: "1625476249"
title: Creating a bottom sheet dialog with rounded corners and expanding it to full screen
url: /2021/07/05/creating-a-bottom-sheet-dialog-with-rounded-corners-and-expanding-it-to-full-screen/

---
Sometimes even simple and easy stuff can be complex on Android. Such is the case where you want to have a bottom sheet dialog with rounded corners which expands to fullscreen. Here is how I did it on Android 6.

{{< figure src="/wp-content/uploads/2021/07/screenshot-2021-07-05-at-12.02.54.png?w=465" alt="" caption="" >}}

I know it may be a bit ugly when you think about it with all that blank space at the bottom. Also, it may be also a good fit to have it as a fragment and not a bottom sheet dialog at all. But sometimes we have to face the decisions of UI/UX designers and try to achieve the things they want. And the above is what they wanted.

## Rounded corners

There are several ways to achieve rounded corners on Android. The simplest one is to use a CardView and just use **app:cardCornerRadius**. Another way is the one I will write about:

In the layout for the bottom sheet dialog, we always had a ConstraintLayout as the top parent of the XML file. This is how it looked like:

```
 <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@drawable/bottom_sheet_corners_background"
        android:backgroundTint="@color/white"
        android:paddingBottom="16dp">
```

The two bolded lines are the ones that display the ConstraintLayout with the rounded corners:

https://gist.github.com/n0m0r3pa1n/5e37e489937b3c047af137b7aaa671bb

So in every layout that you have for the bottom sheet, you need to use this drawable with the background color properly set. Otherwise, it will show with a transparent background.

## Show/Hide bottom sheet dialog extension

Our extension function should support two things:

- Whether to show the bottom sheet dialog fullscreen
- Whether to show it in an expanded state initially

And this is how it looks like:

https://gist.github.com/n0m0r3pa1n/c1a28dd7c6098bf5f73ad27d61bf242e

I hope this works for you too. On our side the achievement is how it looked in the screenshot above.
