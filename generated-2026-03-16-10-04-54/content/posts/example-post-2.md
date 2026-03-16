---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:16"
_publicize_job_id: "43814396742"
_starter_page_template: ""
author: gmirchev90
categories:
  - android
date: "2020-05-04T07:30:31+00:00"
guid: https://georgimirchevcom.wordpress.com/?p=10
parent_post_id: null
post_id: "10"
sharing_disabled:
  - null
summary: |-
  Having a custom TextView is a totally normal thing these days. The problem comes when you try the following:

  ```
  <com.test.view.MyCustomTextView
      android:layout_width="wrap_content"
      android:layout_height="match_parent"
      android:textAppearance="@style/MyCustomTextAppearance" />
  ```

  If your custom TextView extends from the TextView class then this textAppearance that you have set will probably not work. It works in the Preview but it is never displayed correctly on the emulator or on a real device.
switch_like_status:
  - null
tags:
  - textappearance
  - textview
title: textAppearance does not work on custom TextView
url: /2020/05/04/example-post-2/

---
Having a custom TextView is a totally normal thing these days. The problem comes when you try the following:

```
<com.test.view.MyCustomTextView
    android:layout_width="wrap_content"
    android:layout_height="match_parent"
    android:textAppearance="@style/MyCustomTextAppearance" />
```

If your custom TextView extends from the TextView class then this textAppearance that you have set will probably not work. It works in the Preview but it is never displayed correctly on the emulator or on a real device.

The fix is very simple. **Just extend from AppCompatTextView**. And now everything works!

The AppCompat documentation says it alone:

> This will automatically be used when you use `TextView` in your layouts and the top-level activity / dialog is provided by [appcompat](https://web.archive.org/web/20191005054332/https://developer.android.com/topic/libraries/support-library/packages.html#v7-appcompat). You should only need to manually use this class when writing custom views.
