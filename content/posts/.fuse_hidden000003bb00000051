---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:36"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "63517649951"
author: gmirchev90
categories:
  - android
date: "2021-09-28T08:15:50+00:00"
guid: https://georgimirchev.com/?p=597
parent_post_id: null
post_id: "597"
summary: |-
  Setting a toolbar title on the activity should be easy, right? But what about if you want do it through a fragment? Still easy? Well, not exactly.

  The issue is that you may try to touch the toolbar in a moment where the activity is actually still not inflated. This will cause an exception immediately and your app will crash. What is the exact issue?
tags:
  - fragment
  - toolbar
timeline_notification: "1632816950"
title: Approaches not to cause a crash when setting toolbar title on the activity through a fragment
url: /2021/09/28/approaches-not-to-cause-a-crash-when-setting-toolbar-title-on-the-activity-through-a-fragment/

---
Setting a toolbar title on the activity should be easy, right? But what about if you want do it through a fragment? Still easy? Well, not exactly.

The issue is that you may try to touch the toolbar in a moment where the activity is actually still not inflated. This will cause an exception immediately and your app will crash. What is the exact issue?

## The issue

You probably thought that you can touch the toolbar in one of the following lifecycle methods of the Fragment:

- onCreateView
- onViewCreated
- onActivityCreated
- onAttach

Well, not exactly. The only one safe of the above is onActivityCreated but it is deprected. If you try setting the toolbar title in the other ones, you can get a crash of the following type:

```
Error inflating class fragment
        at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2666)
        at android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:2727)
        at android.app.ActivityThread.-wrap12(ActivityThread.java)
        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1478)
        at android.os.Handler.dispatchMessage(Handler.java:102)
        at android.os.Looper.loop(Looper.java:154)
        at android.app.ActivityThread.main(ActivityThread.java:6121)
        at java.lang.reflect.Method.invoke(Native Method)
        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:889)
        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:779)
     Caused by: android.view.InflateException: Binary XML file line #49: Binary XML file line #49: Error inflating class fragment
     Caused by: android.view.InflateException: Binary XML file line #49: Error inflating class fragment
     Caused by: kotlin.UninitializedPropertyAccessException: lateinit property binding has not been initialized
*******
        at androidx.fragment.app.Fragment.performAttach(Fragment.java:2922)
        at androidx.fragment.app.FragmentStateManager.attach(FragmentStateManager.java:464)
        at androidx.fragment.app.FragmentStateManager.moveToExpectedState(FragmentStateManager.java:275)
        at androidx.fragment.app.FragmentStore.moveToExpectedState(FragmentStore.java:112)
        at androidx.fragment.app.FragmentManager.moveToState(FragmentManager.java:1647)
        at androidx.fragment.app.FragmentManager.dispatchStateChange(FragmentManager.java:3126)
        at androidx.fragment.app.FragmentManager.dispatchCreate(FragmentManager.java:3059)
        at androidx.fragment.app.Fragment.restoreChildFragmentState(Fragment.java:1891)
        at androidx.fragment.app.Fragment.onCreate(Fragment.java:1867)
        at androidx.navigation.fragment.NavHostFragment.onCreate(NavHostFragment.java:206)
        at androidx.fragment.app.Fragment.performCreate(Fragment.java:2949)
        at androidx.fragment.app.FragmentStateManager.create(FragmentStateManager.java:475)
        at androidx.fragment.app.FragmentStateManager.moveToExpectedState(FragmentStateManager.java:278)
        at androidx.fragment.app.FragmentLayoutInflaterFactory.onCreateView(FragmentLayoutInflaterFactory.java:140)
        at androidx.fragment.app.FragmentController.onCreateView(FragmentController.java:135)
        at androidx.fragment.app.FragmentActivity.dispatchFragmentsOnCreateView(FragmentActivity.java:319)
        at androidx.fragment.app.FragmentActivity.onCreateView(FragmentActivity.java:298)
        at android.view.LayoutInflater.createViewFromTag(LayoutInflater.java:777)
        at android.view.LayoutInflater.createViewFromTag(LayoutInflater.java:727)
        at android.view.LayoutInflater.rInflate(LayoutInflater.java:858)
        at android.view.LayoutInflater.rInflateChildren(LayoutInflater.java:821)
        at android.view.LayoutInflater.rInflate(LayoutInflater.java:861)
        at android.view.LayoutInflater.rInflateChildren(LayoutInflater.java:821)
        at android.view.LayoutInflater.inflate(LayoutInflater.java:518)
        at android.view.LayoutInflater.inflate(LayoutInflater.java:426)
```

So from the above exception you can see what is happening. It is quite obvious.

1. The MainActivity reaches onCreate.
1. In onCreate the MainActivityBinding.inflate is called to set the content.
1. Then the fragment is actually called to be created. onCreateView is called
1. in onCreateView we call setToolbarTitle()
1. setToolbarTitle() crashes because MainActivityBinding has not finished the inflate and there is no UI to touch

onAttach will also cause the same exception. So what are your choices to approach this problem?

## The possible solutions

### Inheritance approach

You can always try to move the call to the appropriate lifecycle method. Maybe you can move every single call to setToolbarTitle to onStart. And if you have BaseFragment it may be even easy to do this. But do you really want to care about lifecycle when you just want to set a toolbar title?

### Composition approach

Android already has components which know about lifecycle. Why should you care when they already know about it? Yes, I am talking about LiveData and StateFlow. You could really easily let the

```
val toolbarTitle: LiveData<String> = _toolbarTitle
private val _toolbarTitle = MutableLiveData<String>()
```

do everything for you. Then in the activity, you can just observe it and it will set the title when the activity is there. Do you feel the approach already?

1. Create a ToolbarViewModel that is observed in the activity
1. Use **by activityViewModel<ToolbarViewModel>** to access it in the fragments
1. Set the title when the toolbarViewModel emits a title

Simple as that. The LiveData/StateFlow will take care for everything for you. You don't need to care about the lifecycle and where it is to make the appropriate call.

Hope I was helpful to anyone reading this article and having this problem!
