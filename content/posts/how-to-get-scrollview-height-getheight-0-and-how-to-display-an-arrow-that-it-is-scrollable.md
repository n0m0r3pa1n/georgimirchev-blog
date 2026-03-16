---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:22"
_publicize_job_id: "43823252944"
author: gmirchev90
categories:
  - android
date: "2020-05-04T12:52:34+00:00"
guid: http://georgimirchev.com/?p=152
parent_post_id: null
post_id: "152"
summary: |-
  Lets say that we have a ScrollView which should ahave an arrow at its bottom. That arrow is visible only when the ScrollView is scrollable (That means it has more content than it can display).

  In android the scrollview does not have a method isScrollable() and I am still wondering what is the reason. May be too complex realization.
tags:
  - scrollview
timeline_notification: "1588596758"
title: How to get ScrollView height (getHeight() = 0) and how to display an arrow that it is scrollable?
url: /2020/05/04/how-to-get-scrollview-height-getheight-0-and-how-to-display-an-arrow-that-it-is-scrollable/

---
Lets say that we have a ScrollView which should ahave an arrow at its bottom. That arrow is visible only when the ScrollView is scrollable (That means it has more content than it can display).

In android the scrollview does not have a method isScrollable() and I am still wondering what is the reason. May be too complex realization.

I spent more than 8 hours on this so the answer is:  
Attach a ViewTreeObserver. This will fire up when the scrollview is added to the android display and you can call the getHeight() which will return your scrollview’s height. If you try to do it in onViewCreated or somewhere else it will be 0 (personal experience).

```
 mScrollView.setScrollViewListener(this);

 ViewTreeObserver vto = mScrollView.getViewTreeObserver();
 vto.addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
     @Override
     public void onGlobalLayout() {

         ViewTreeObserver obs = mScrollView.getViewTreeObserver();
         if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN) {
             obs.removeOnGlobalLayoutListener(this);
         } else {
             obs.removeGlobalOnLayoutListener(this);
         }

         int totalHeight = getResources().getDimensionPixelSize(R.dimen.article_details_item_img_height) + mArticleText.getHeight() + getResources().getDimensionPixelSize(R.dimen.article_details_picture_margin_bot);

         if (totalHeight <= mScrollView.getHeight()) {
             mIvScrollDown.setVisibility(View.INVISIBLE);
         }
     }
 });
```
