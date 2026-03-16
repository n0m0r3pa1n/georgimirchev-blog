---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:31"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "52318113882"
author: gmirchev90
categories:
  - android
date: "2020-12-17T01:58:47+00:00"
guid: http://georgimirchev.com/?p=314
parent_post_id: null
post_id: "314"
summary: |-
  ## Problem

  Each Android app has an authentication mechanism or at least most of them do. What this auth mechanism does is that it refreshes an access token using a refresh token every N minutes. In our case these N minutes are 7. And now the question is: How do I implement this?
tags:
  - cache
  - observable
  - rxjava
  - timeout
timeline_notification: "1608170331"
title: Execute an RxJava observable only once in a given time period
url: /2020/12/17/execute-an-rxjava-observable-only-once-in-a-given-time-period/

---
## Problem

Each Android app has an authentication mechanism or at least most of them do. What this auth mechanism does is that it refreshes an access token using a refresh token every N minutes. In our case these N minutes are 7. And now the question is: How do I implement this?

I joined the project at a later stage where everything was already there and I couldn't give another idea. **So what I have found is that the devs have agreed to have this authenticate() method that does the refreshing of the access token**. And how it works? Well, you call it every time where you call a method which may need to use a fresh access token. What I just said means that this method now is called at 70+ places ... And imagine the case where you put the app to the background, leave it there for 20 mins and then open it from the Recent apps list. You know what will happen, right? All of the screens/usecases/architecture elements where this method is called will call it multiple times. In our case we had this method executed like 10-12 times after the app is opened. This would lead to the case where multiple different threads try to use the same access token to be refreshed which would lead to the backend server returning us the invalid\_grant error which in turn would log out the user. This amazingly bad experience would also result in bad reviews and in cases where you pay for premium stuff but actually get logged out every day and don't have the possibility to properly enjoy them.

## Solution

What is the solution to this? **The correct solution would be to use a single OkHttp client instance (really basic one) shared for all of you API classes**. Inside OkHttp you could have an interceptor that listen for a certain type of an error (let's say 401 Unauthorized) and then tries to do the refresh. In this way you won't need to call a method at multiple places and whenever the token expires, it will be handled at a single place inside the OkHttpClient. **This way you won't need to do this kind of polling the server, instead you are waiting for the event to happen**.

But as an alternative one (as we don't have the time to refactor stuff at this stage at the project) I did some research what is the best approach to execute an observable that we have (we were using this open-source project called AppAuth) created using Observable.create only once in a given time period. This would mean the following:

- 10 threads try to execute the same observable
- First thread executes it and starts the timer
- 2-10 threads join in the given N seconds interval and only listen for the result
- Thread 11 joins after the given N seconds interval. It executes it again.
- Thread 12 joins immediately after thread 11. It would listen for the result.

Simple concept, right? In this way we reduce our 11 calls initially to a single call and after that, when the app is used, it would behave pretty straightforward as only one execution of the method would happen for a given N seconds period of time.

## Approaches

I tried several approaches but none of them seemed to work for my case. Some of these include:

- Using publish().refCount() or share() - you don't have the time period there and if you have 10 threads entering 300 ms one after another it would again hit the multiple refresh calls with old state
- Using .replayingShare() - same result as above. First 5 threads would be fine but then the 6-th will execute it again and again same problems
- Using Guava to cache the observable and get it from the cache - just didn't work out at all

The final solution that worked for me is using the Observable.compose() method and having a BehaviorSubject & AtomicBoolean variables.

1. So thread A enters the observable, it starts it and sets the AtomicBoolean to true (that it is executing).
1. Thread B enters, sees the AtomicBoolean value that it is running and then subscribes to the BehaviorSubject.
1. Thread A finishes execution and calls onNext on the subject
1. Thread C enters, sees that the observable is not running BUT it is still in the given time period and takes the value from the BehaviorSubject

The tricky thing is the BehaviorSubject which stores the last emitted value so if a thread joins after the value is emitted but before the given timeout is reached, it will work with the cached value and won't get stale as it would with a PublishSubject.

Here is how our CacheObservable looks like:

```
import io.reactivex.Observable
import io.reactivex.ObservableSource
import io.reactivex.ObservableTransformer
import io.reactivex.Observer
import io.reactivex.functions.Consumer
import io.reactivex.subjects.BehaviorSubject
import java.util.concurrent.TimeUnit
import java.util.concurrent.atomic.AtomicBoolean

class CacheObservable<T> private constructor(
    private val timeout: Long,
    private val unit: TimeUnit,
    private val evaluator: (state: T) -> Boolean = { true }
) : ObservableTransformer<T, T> {
    override fun apply(upstream: Observable<T>): ObservableSource<T> {
        val lastSeen = LastSeen<T>(
            timeout, unit
        )
        return LastSeenObservable(upstream.doOnNext(lastSeen), lastSeen, evaluator)
    }

    companion object {
        fun <T> cache(timeout: Long, unit: TimeUnit, evaluator: (state: T) -> Boolean = { true }): CacheObservable<T> {
            return CacheObservable(timeout, unit, evaluator)
        }
    }
}

internal class LastSeen<T>(private val timeout: Long, private val unit: TimeUnit) : Consumer<T> {
    private var lastEmissionTimestamp: Long = 0
    override fun accept(latest: T) {
        lastEmissionTimestamp = System.currentTimeMillis()
    }

    val isValid: Boolean
        get() = System.currentTimeMillis() - lastEmissionTimestamp <= unit.toMillis(timeout)
}

internal class LastSeenObservable<T>(
    private val upstream: Observable<T>,
    private val lastSeen: LastSeen<T>,
    private val evaluator: (state: T) -> Boolean
) : Observable<T>() {
    private val isRunning = AtomicBoolean(false)
    private val subject: BehaviorSubject<OptionalCompat<T>> = BehaviorSubject.create()
    override fun subscribeActual(observer: Observer<in T>) {
        if (lastSeen.isValid || !isRunning.compareAndSet(false, true)) {
            subject
                .filter { it.isPresent() }
                .map { it.requireData() }
                .filter { evaluator.invoke(it) }
                .subscribe(observer)
        } else {
            subject.onNext(OptionalCompat.empty())
            isRunning.set(true)
            upstream.doOnNext { t: T ->
                subject.onNext(OptionalCompat.of(t))
                isRunning.set(false)
            }
                .subscribe(observer)
        }
    }
}
```

As you can see, we always want to work with fresh data, that is why when we start the execution of the observable, if any thread subscribes and there is value in the BehaviorSubject that is already present, we don't want that thread to work with it. That's why we immediately push an OptionalCompat.empty(). Similar as you would call BehaviorSubject.clearCachedValue().

## Usage

How do you use the above solution?

```
private val authenticateObservable = myObservable
.compose(CacheObservable.cache(AUTHENTICATE_EXECUTION_THROTTLE_SEC, TimeUnit.SECONDS))
```

And then you can use the val wherever you want. You can also test it using the TestScheduler, works fine :)

I hope this post was useful for someone fighting the same thing I did. It took me quite a lot of hours to figure it out but I am a bit more happy with it now, when I see it working. But again, this is not the right approach to the problem, it is more like a temporary solution.
