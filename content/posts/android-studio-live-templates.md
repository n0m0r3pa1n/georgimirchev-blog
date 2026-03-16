---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:28"
_publicize_job_id: "43833491955"
author: gmirchev90
categories:
  - android
date: "2020-05-04T17:49:51+00:00"
guid: http://georgimirchev.com/?p=230
parent_post_id: null
post_id: "230"
summary: 'I was really wondering how I can remove repetitive tasks from Android Studio by assigning shortcuts or tags and I found that IntelliJ has Live Templates which I bet that little programmers use. So as an example I wanted when I type the word “tag” within a class the following to be autocompleted:'
tags:
  - android-studio
  - live-templates
timeline_notification: "1588614592"
title: Android Studio Live Templates
url: /2020/05/04/android-studio-live-templates/

---
I was really wondering how I can remove repetitive tasks from Android Studio by assigning shortcuts or tags and I found that IntelliJ has Live Templates which I bet that little programmers use. So as an example I wanted when I type the word “tag” within a class the following to be autocompleted:

```
public static final String TAG = MyClass.class.getSimpleName();
```

and I didn’t want to write anything by myself. Just by writing “tag” and pressing TAB this should be written.  
To do it:

1. Open File -> Settings -> Live Templates
1. In the “other” category press the + to the right and select Live Template to add new element
1. In abbreviation add “tag” and in template text add: “public static final String TAG = $CLASS\_NAME$.class.getSimpleName();$END$”
1. In the applicable context select Java – Declarations
1. Click Edit Variables and select the className() method in the Expression column. And check the skip if defined value.
1. Now when you write “tag” and press tab everything should be populated!
