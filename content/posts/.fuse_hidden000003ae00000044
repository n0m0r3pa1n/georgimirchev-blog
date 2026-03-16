---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:17"
_publicize_job_id: "43814448478"
_starter_page_template: ""
author: gmirchev90
categories:
  - android
date: "2020-05-04T07:30:30+00:00"
guid: https://georgimirchevcom.wordpress.com/?p=11
parent_post_id: null
post_id: "11"
sharing_disabled:
  - null
summary: Using RxJava with the architecture components from Google in the [latest project](https://web.archive.org/web/20191005041357/https://github.com/n0m0r3pa1n/bulgarian_history_revisited) that I was working on, it made me think how does error handling fit with the LiveData and can you keep the RxJava error handling which I find quite nice.
switch_like_status:
  - null
tags:
  - architecture
  - architecture-components
  - kotlin
  - livedata
  - rxjava
title: RxJava &amp; LiveData combined error handling
url: /2020/05/04/example-post/

---
Using RxJava with the architecture components from Google in the [latest project](https://web.archive.org/web/20191005041357/https://github.com/n0m0r3pa1n/bulgarian_history_revisited) that I was working on, it made me think how does error handling fit with the LiveData and can you keep the RxJava error handling which I find quite nice.

The way to use LiveData and have error handling inside your RxJava app was to introduce a wrapper class, probably something called Response which has two states: Error & Success. So when RxJava throws an exception in onError, you just create this wrapper class with error state. In the UI you check the state, if it is error -> show it, if it is success -> show the data.   But this thing actually didn’t seem quite right to me. Having this wrapper complicates the stuff and makes them look ugly. Why do I have to wrap every single data in my application in a class just to have proper error handling!?

So I found a way in Github where a guy has created a simple app that introduced the onError method call for LiveData, similar on how we use it with RxJava.   
What he did is very simple:

- Create Flowable, Completable, Maybe and Single LiveData classes
- Wrap the result in Optional<> so you can call FlowableLiveData.onError in case of Optinal.Error
- Add extension functions to the LiveData classes like subscribeSingle with onNext and onError functions

So here is how the code looks in the ViewModel:

```
fun getEvent(eventId: String) =
    eventsRepository.getEvent(eventId)
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .doOnSuccess { childId.set(R.id.content) }
        .toReactiveSource()

fun <T: Any> Single<T>.toReactiveSource() =
    SingleReactiveSource.from(this)
```

Here is how the code looks in the view (fragment):

```
eventsViewModel.getEvent(arguments!!.getString(ARG_EVENT_ID)!!)
            .subscribeSingle(this,
                onSuccess = { event ->
                    oldTitle = activity?.title
                    activity?.title = event.title
                    binding.event = event
                },
                onError = {
    errorHandler.handleError(requireContext(), it) })
```
