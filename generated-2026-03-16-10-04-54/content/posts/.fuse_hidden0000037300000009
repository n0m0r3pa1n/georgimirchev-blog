---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:30"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "47619511788"
author: gmirchev90
categories:
  - android
date: "2020-08-12T06:13:14+00:00"
guid: http://georgimirchev.com/?p=272
parent_post_id: null
post_id: "272"
summary: I recently had a very strange bug when it comes to using a BehaviorSubject inside the object that I am testing.
tags:
  - rxjava
  - unit-tests
timeline_notification: "1597212798"
title: Race conditions in unit tests with RxJava when using TestScheduler
url: /2020/08/12/race-conditions-in-unit-tests-with-rxjava-when-using-testscheduler/

---
I recently had a very strange bug when it comes to using a BehaviorSubject inside the object that I am testing.

## The case

We have a Presenter. This presenter receives events through an Observable called uiEvents. It listens for them and reacts differently based on the type of the event that it receives. So we have something like this

```
class MyPresenter (
  private val uiEvents: Observable<UiEvent>,
  private val uiScheduler: Scheduler,
  private val ioScheduler: Scheduler
) {

  val actions: Observable<PresenterAction> = PublishSubject.create()

  fun createSubscriptions() {
    uiEvents.ofType(ButtonClickEvent::class.java)
      .switchMapSingle { callTheApi() }
      .subscribeOn(ioScheduler)
      .observeOn(uiScheduler)
      .subscribe({ actions.onNext(OpenThatScreenAction()) })
  }

  private fun callTheApi() = Single.just(1,2,3)
}

class MyPresenterTest {
  private val uiEvents = BehaviorSubject.create<UiEvent>()
  private val testScheduler = TestScheduler()
  private val underTest = MyPresenter(
    uiEvents = uiEvents,
    uiScheduler = testScheduler,
    ioScheduler = testScheduler
  )

  @Test
  fun returnsOpenThatScreenAction() {
    val testObserver = underTest.actions.test()
    underTest.createSubscriptions()

    uiEvents.onNext(ButtonClickEvent())

    testObserver.assertValue { it is OpenThatScreenAction }
  }
}
```

Simple as that. And you want to test that when you submit an event the expected thing happens. But basically this assertion never passes. You can add doOnSubscribe and see that it gets called but when you try to assert that the value count is 1, it never passes.

## The answer

There is this small article that gives [the answer](https://proandroiddev.com/avoiding-racing-with-subjects-testscheduler-b306e7306fcf).

> Turns out the actual subscription( _[subscribeActual()](https://imgur.com/a/V80vlNx)_ method in **PublishSubject**) which adds the subscriber to Subject’s list of subscribers doesn’t happen until _triggerActions_ is invoked on the **TestScheduler**.

So you need to call the testScheduler.triggerActions() in order to get the subscribe method called. This results in a race condition where even though the subscriber seems subscribed, the actual subscription and emission happen concurrently. The emission is therefore ignored due to absence of any subscribers.
