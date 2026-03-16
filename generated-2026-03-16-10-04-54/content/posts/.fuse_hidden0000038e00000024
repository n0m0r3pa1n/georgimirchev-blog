---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:23"
_publicize_job_id: "43823475231"
author: gmirchev90
categories:
  - android
date: "2020-05-04T12:59:33+00:00"
guid: http://georgimirchev.com/?p=161
parent_post_id: null
post_id: "161"
summary: That bastard drove me nuts today. I do not understand why is it so hard to fix. I want the text to be white. After 2 hours of reading useless posts, I found a solution working for me.
tags:
  - spinner
timeline_notification: "1588597176"
title: Spinner in Toolbar – How to change the selected item text color?
url: /2020/05/04/spinner-in-toolbar-how-to-change-the-selected-item-text-color/

---
That bastard drove me nuts today. I do not understand why is it so hard to fix. I want the text to be white. After 2 hours of reading useless posts, I found a solution working for me.

## Copy the existing android.R.layout.simple\_list\_item\_1

I made one which looks like this

```
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@android:id/text1"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:textAppearance="?android:attr/textAppearanceListItemSmall"
    android:textColor="@android:color/white"
    android:gravity="center_vertical"
    android:paddingStart="?android:attr/listPreferredItemPaddingStart"
    android:paddingEnd="?android:attr/listPreferredItemPaddingEnd"
    android:minHeight="?android:attr/listPreferredItemHeightSmall" />

```

It has **textColor** set to white.

## Set it to the Spinner adapter

```
        ArrayAdapter<String> adapter = new ArrayAdapter<>(this,
                R.layout.item_spinner, new ArrayList<String>() {{
            add(getString(R.string.meetings_all));
            add(getString(R.string.meetings_future));
            add(getString(R.string.meetings_past));
        }});
        spMeetingStatus.setAdapter(adapter);

```

## Set popUpTheme and theme to Spinner

Here is how my spinner looks like

```
    <android.support.v7.widget.Toolbar
        android:id="@+id/toolbar_meetings"
        style="@style/TourMeetToolbar"
        android:layout_alignParentTop="true"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize">
        <Spinner
            android:id="@+id/spinner_meeting_status"
            android:spinnerMode="dropdown"
            local:popupTheme="@style/ThemeOverlay.AppCompat.Dark"
            local:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content" />
    </android.support.v7.widget.Toolbar>

```

Basically, the “popUpTheme” sets how the elements inside are displayed. They are in a black background. You can change that by setting “popUpBackground” property to another color.
