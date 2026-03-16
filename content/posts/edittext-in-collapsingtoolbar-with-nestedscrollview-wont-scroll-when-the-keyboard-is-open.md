---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:17"
_publicize_job_id: "43814693081"
author: gmirchev90
categories:
  - android
date: "2020-05-04T08:02:30+00:00"
guid: http://georgimirchev.com/?p=36
parent_post_id: null
post_id: "36"
summary: |-
  ## The problem

  What we currently have

  - CoordinatorLayout with CollapsingToolbar inside
  - The CollapsingToolbar has an ordinary Toolbar inside that displays text
  - A NestedScrollView with multiple EditText fields inside of it which actually pushes the CollapsingToolbar to collapse when you try to enter something in the EditText
tags:
  - collapsingtoolbarlayout
  - edittext
  - nestedscrollview
timeline_notification: "1588579351"
title: EditText in CollapsingToolbar with NestedScrollView won’t scroll when the keyboard is open
url: /2020/05/04/edittext-in-collapsingtoolbar-with-nestedscrollview-wont-scroll-when-the-keyboard-is-open/

---
## The problem

What we currently have

- CoordinatorLayout with CollapsingToolbar inside
- The CollapsingToolbar has an ordinary Toolbar inside that displays text
- A NestedScrollView with multiple EditText fields inside of it which actually pushes the CollapsingToolbar to collapse when you try to enter something in the EditText

## The actual problem

The problem was that when you tap on one of the EditText fields and the keyboard opens, then you cannot scroll down to the last EditText that is visible so you can enter some text into it. Here is a video demonstrating the problem:

Video Link: [https://drive.google.com/open?id=1vX-RmLPnx8LAhu6j\_n9qaO-mebYMXKrv](https://drive.google.com/open?id=1vX-RmLPnx8LAhu6j_n9qaO-mebYMXKrv)

## The solution

Actually, the problem was that the parent **CoordinatorLayout** had the android:fitsSystemWindows flag set to true. This would stop the NestedScrollView from scrolling when the keyboard is visible. It would just stop at the point where the CollapsingToolbarLayout is collapsed and won’t scroll a step more. In order to fix this, you just have to set the fitsSystemWindows flag to false for the root layout in your XML.

```
 <android.support.design.widget.CoordinatorLayout
        android:id="@+id/rootLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:fitsSystemWindows="false">

        <android.support.design.widget.AppBarLayout
            android:id="@+id/appbar"
            android:layout_width="match_parent"
            android:layout_height="150dp"
            android:background="@color/background"
            android:fitsSystemWindows="true"
            app:elevation="0dp">

            <android.support.design.widget.CollapsingToolbarLayout
                android:id="@+id/collapsingToolbar"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:fitsSystemWindows="true"
                app:collapsedTitleTextAppearance="@style/Typography1L"
                app:expandedTitleTextAppearance="@style/Typography44"
                app:layout_scrollFlags="scroll|exitUntilCollapsed">

                <android.support.v7.widget.Toolbar
                    android:id="@+id/toolbar"
                    android:layout_width="match_parent"
                    android:layout_height="?attr/actionBarSize"
                    android:layout_alignParentTop="true"
                    app:layout_collapseMode="pin"
                    app:navigationIcon="?attr/homeAsUpIndicator"
                    app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />

            </android.support.design.widget.CollapsingToolbarLayout>

        </android.support.design.widget.AppBarLayout>

        <android.support.v4.widget.NestedScrollView
            android:id="@+id/credentialsContainer"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:animateLayoutChanges="true"
            android:fillViewport="true"
            android:fitsSystemWindows="true"
            app:layout_behavior="@string/appbar_scrolling_view_behavior">

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                tools:context=".addbank.activities.AddBankInAppActivity">

		<EditText
                	android:layout_width="match_parent"
                	android:layout_height="wrap_content"
			android:hint="Enter some info here" />

               <EditText
                	android:layout_width="match_parent"
                	android:layout_height="wrap_content"
			android:hint="Enter some info here" />
		<EditText
                	android:layout_width="match_parent"
                	android:layout_height="wrap_content"
			android:hint="Enter some info here" />

		<EditText
                	android:layout_width="match_parent"
                	android:layout_height="wrap_content"
			android:hint="Enter some info here" />

            </LinearLayout>
        </android.support.v4.widget.NestedScrollView>

    </android.support.design.widget.CoordinatorLayout>
```

Video Link: [https://drive.google.com/open?id=1lot\_e505OL3jfHaDzocHM0Hg7QL5HKA7](https://drive.google.com/open?id=1lot_e505OL3jfHaDzocHM0Hg7QL5HKA7)
