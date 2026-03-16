---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:38"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "69952756616"
author: gmirchev90
categories:
  - android
date: "2022-03-09T12:05:27+00:00"
guid: https://georgimirchev.com/?p=743
parent_post_id: null
post_id: "743"
summary: |-
  What is the difference between this piece of code:

  ```
  <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
          <item name="windowActionBar">false</item>
  </style>
  ```

  and this one:

  ```
  <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
          <item name="android:windowActionBar">false</item>
  </style>
  ```
tags:
  - material
  - themes
  - windowactionbar
timeline_notification: "1646827527"
title: Don't forget about the difference between android:windowActionBar vs windowActionBar in your theme
url: /2022/03/09/dont-forget-about-the-difference-between-androidwindowactionbar-vs-windowactionbar-in-your-theme/

---
What is the difference between this piece of code:

```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="windowActionBar">false</item>
</style>
```

and this one:

```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="android:windowActionBar">false</item>
</style>
```

The answer is simple. **windowActionBar** is an attribute provided in AppCompat library, whereas **android:windowActionBar** is provided in Material theme.

Same applies for all of the other attributes:

```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
</style>
```
