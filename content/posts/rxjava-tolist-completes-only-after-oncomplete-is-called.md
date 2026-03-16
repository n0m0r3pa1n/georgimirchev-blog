---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:21"
_publicize_job_id: "43820467211"
author: gmirchev90
categories:
  - android
date: "2020-05-04T11:21:54+00:00"
guid: http://georgimirchev.com/?p=131
parent_post_id: null
post_id: "131"
summary: |-
  ```
   mGetActiveHubUpdatesInteractor.getActiveHubUpdates()
                  .filter(Optional::isPresent)
                  .firstOrError()
                  .map(Optional::get)
                  .flatMap(activeHub ->
                          mGetActiveHubEventsInteractor.getActiveHubEvents(activeHub, startTime,
                                  endTime))
                  .flattenAsObservable(list -> list)
                  .map(mActiveHubEventUiMapper::toUiActiveHubEvent)
                  .toList()
                  .subscribe(this::populateEvents, this::handleError);
  ```
tags:
  - rxjava
timeline_notification: "1588591314"
title: RxJava toList – completes only after onComplete is called
url: /2020/05/04/rxjava-tolist-completes-only-after-oncomplete-is-called/

---
```
 mGetActiveHubUpdatesInteractor.getActiveHubUpdates()
                .filter(Optional::isPresent)
                .firstOrError()
                .map(Optional::get)
                .flatMap(activeHub ->
                        mGetActiveHubEventsInteractor.getActiveHubEvents(activeHub, startTime,
                                endTime))
                .flattenAsObservable(list -> list)
                .map(mActiveHubEventUiMapper::toUiActiveHubEvent)
                .toList()
                .subscribe(this::populateEvents, this::handleError);
```

This is a quick reminder for those of you who use RxJava and the toList() operator.  toList() completes only after the Observable emits onComplete. So keep in mind if your operators after toList never get called.

[Here is an article](http://tomstechnicalblog.blogspot.co.uk/2015/11/rxjava-operators-tolist.html) that explains the operator in more detail. The highlight from the article is:

> The toList() operator intercepts onNext() calls. Rather than handing the items further down the chain, it catches them instead and adds them to a List (specifically an ArrayList). Only when the source Observable calls onCompleted() will it emit the entire List up the chain, _then_ it will call onCompleted() up the chain.
