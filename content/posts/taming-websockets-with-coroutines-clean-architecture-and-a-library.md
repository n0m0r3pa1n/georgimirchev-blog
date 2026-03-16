---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:39"
_last_editor_used_jetpack: block-editor
_oembed_f9e83948145a7343fdbae3db06a275cd: '{{unknown}}'
_publicize_job_id: "73020874911"
author: gmirchev90
categories:
  - android
date: "2022-05-28T03:27:10+00:00"
guid: https://georgimirchev.com/?p=770
parent_post_id: null
post_id: "770"
summary: |-
  Websocket integration is one of the biggest issues I had in my whole experience with Android. Why is it such a huge pain? There are several reasons behind that:

  1. Connection should remain stable no matter what - and we know how complex the Android lifecycle can be.
  2. Sending data can be a one-way operation without giving any result - did you succeed in sending it or did you fail? Only Server knows.
  3. Retrying connection or network requests can be tricky to implement - from pushing logic to interactors to having an internal logic inside a repository, be sure - nothing will work at 100%. Not even at 90% either.
  4. To get it right you need both frontend and backend collaboration. Nothing can be done if both sides do it separated from each other.
tags:
  - clean-architecture
  - coroutines
  - scarlet
  - tinder
  - websockets
timeline_notification: "1653708434"
title: Taming websockets with Coroutines, Clean Architecture and a library
url: /2022/05/28/taming-websockets-with-coroutines-clean-architecture-and-a-library/

---
Websocket integration is one of the biggest issues I had in my whole experience with Android. Why is it such a huge pain? There are several reasons behind that:

1. Connection should remain stable no matter what - and we know how complex the Android lifecycle can be.
1. Sending data can be a one-way operation without giving any result - did you succeed in sending it or did you fail? Only Server knows.
1. Retrying connection or network requests can be tricky to implement - from pushing logic to interactors to having an internal logic inside a repository, be sure - nothing will work at 100%. Not even at 90% either.
1. To get it right you need both frontend and backend collaboration. Nothing can be done if both sides do it separated from each other.

## Keeping connection stable

There are 2 ways you can approach the issue of having a websocket connection:

1. You can have your own OkHttpWebSocketClient
1. You can use a library

### OkHttpWebSocketClient

Let's start with the custom OkHttpWebSocketClient. A sample one that we used in one of our apps is this one:

<script src="https://gist.github.com/n0m0r3pa1n/a7e5283e66a0f446ded47f5cbe7cf60a.js"></script>

The issues are several:

- First is the complexity it has. It can be very cumbersome to wrap your head around it although what it does is really simple. It tries a connection and if it fails - it tries again.
- You can really easily end up in a hierarchy of classes that is huge and takes a good amount of time for a new joiner to understand
- The logic to send messages that **must** arrive can make the codebase even more significant - it has to keep them in cache and retry sending them whenever the connection is there
- Android lifecycle is a nightmare - I hope we all know that. And keeping the connection alive is a real challenge. There are onCreate/onResume/onDestroy methods but also the Doze project and the battery optimizations happening during the night. It is almost mission impossible.
- Doing reconnection is a challenge too - you have to keep in mind that reconnection may happen from multiple places and managing it properly can be cumbersome with mutexes, locks etc.

### Using Scarlet from Tinder

After our initial approach with the custom OkHttpClient, we saw all the disadvantages of managing stuff manually and decided to try out one of the libraries out there. This is the [Scarlet](https://github.com/Tinder/Scarlet) library from Tinder.

It made our lives easier because:

1. It allows usage of simple APIs similar to Retrofit  

{{< figure src="/wp-content/uploads/2022/05/image.png?w=848" alt="" caption="" >}}

2\. It works with a StateMachine so you control the state of the connection - start / stop or complete it. It has built-in support for the Android lifecycle. And you can initialize it lazily if you want to have server-driven URLs and then tell the library to start the connection

{{< figure src="/wp-content/uploads/2022/05/image-1.png?w=684" alt="" caption="" >}}

3\. Ready to use retry mechanism in place - you just pass the retry that you want to use like Linear or Exponential and it works like a charm.

4\. Simplifies codebase - your layering ends up looking like:

Fragment -> ViewModel -> Interactor -> Repository -> API

No more classes needed than these.

5\. A minus is it seems like Scarlet is at the end of the support - there are several issues, and releases seem to be halted. There are a lot of branches hanging in the air but in the end - the idea is great. Maybe the open-source community can help keep this project alive.

## Retrying to deliver a message

One of the tricky parts when working with WebSockets is working with messages. Knowing whether a message was successfully sent and received is one of the major drawbacks of this way of communication. Let's think about messages in a more generic way for a bit.

We can separate messages into 2 types:

- Messages that must be sent - these require that a connection is in place before sending
- Messages that may be sent - maybe the server won't receive them but you don't actually care

### Messages that must be sent

These types of messages are the ones we really care about. For them we have several ways to make sure they are received from the backend:

1. Send based on the connection status - we check what is the current connection status before we attempt to send the message. If the status is "Connected", then we proceed.
1. Waiting for backend response - backend responds with an acknowledgment message after each one that we send. If we don't receive such in a reasonable amount of time ex. 10 seconds, we treat the message as failed
1. Checking the connection status AND waiting for backend response - the above 2 points merged into one

#### Sending based on the connection status

We expose a method from our API class the observe the connection status. For Scarlet, this can automatically be achieved by observing events of the type WebSocket.Event / ProtocolEvent.

```
suspend operator fun invoke(correlationId: UUID) {
        pacApi.observeConnectionEvents()
            .filter { it.isConnected() }
            .onEach { acceptCancellationRequest(correlationId) }
            .first()
    }
```

So by observing the connection status, you force the user to wait on the screen to have a guaranteed connection before sending the event. This is what the ".first()" does.

#### Waiting for backend response

In this approach, you send the message and wait for an acknowledgment message from the backend. This can be expressed as a single flow function.

```
pacApi.receiveConfirmSignatureStatus()
        .onStart { pacApi.confirmSignature(txResult) }
        .firstOrNull() ?: SignatureVerificationResult.REJECTED
```

Here you can choose how to handle this function. You can even wrap the receiveConfirmSignatureStatus inside a withTimeout block so in case you don't receive the message in an appropriate amount of time - you show an error to the user.

#### Messages based on both approaches

Would be as simple as this:

```
pacApi.observeConnectionEvents()
        .filter { it.isConnected() }
        .flatMapConcat { pacApi.receiveConfirmSignatureStatus() }
        .onStart { pacApi.confirmSignature(txResult) }
        .firstOrNull() ?: SignatureVerificationResult.REJECTED
```

### Messages that may be sent

For this type of message it is obvious we don't really care about the status of the connection. As long as they don't crash the app when the connection is not there, we really don't care whether they were received or not.

## Picking up the right layer where retry logic should reside

> Should reconnection be handled internally inside repositories or externally by interactors who use suspend functions as the way to send data upon reconnection?
>
> Websocket Connection Fighter

Android apps are famous for their layering of the code and the architecture that is used. We have ViewModels, Interactors, Repositories, hidden helper classes, and many more. With the introduction of Kotlin coroutines, our choice can actually become harder to guess than it looks initially.

There are many ways that you may decide to implement the retry logic. Some people may have strong arguments to keep it internal in the repository, so it is the only one responsible for managing the requests - caching them and retrying.

Others may argue that hiding this logic can bring a lot of unnecessary complexity when you have a lot of messages that you must send. Then you can use suspending functions and jobs to manage the execution of the requests and cancel the jobs when you feel that the function has reached its timeout.

In our case, we chose the Interactors as the place where we put the retry logic. Interactors are business objects that care or don't care about delivering the messages. Based on the domain knowledge that they manage, they can be the ones to decide whether they need a timeout, whether the message must be sent and how they want to treat the failure.

Usually, our approach is to make the user wait on a screen in case the connection is not there and if it fails after a maximum amount of time - we show him that an error has occurred.

Sometimes not everything that needs to happen is in front of the user's eyes. Maybe we have a class that uses these interactions in a background thread. Then it is the responsibility of that class to manage the suspending functions that run and handle their cancellation. As you know, if there is no connection, these functions may get stuck in memory waiting for it. And if you try to execute it once again, you have 2 functions staying in memory now. That's why you need to manage the respective Jobs and cancel them when you feel you don't need to wait for a connection anymore.

### Scopes to retry delivering a message

Following from the above, we can split the scopes where we retry to deliver a message into 3 respective ones:

- Application lifecycle scope - retry to deliver the message in the background until the application is alive
- Screen scope - until the screen is present to the user
- Background scope - even when the app is backgrounded, the message should be delivered

#### Application lifecycle scope

As mentioned, use Jobs to control the count of the functions that are suspended in the app memory.

#### Screen scope

Basically the viewModelScope. You can use it to make the user wait on the screen for the connection in case it is not there. One thing that is good to know here is the following:

**Make sure to cancel the job if the fragment is in the background or it will continue to listen for messages**.

{{< figure src="https://s3.amazonaws.com/media-p.slid.es/uploads/323674/images/9525366/pasted-from-clipboard.png" alt="" caption="" >}}

For this piece of code, if you don't cancel the job in onStop or onPause, it will continue to listen for socket events when the fragment is in the background which may have unexpected consequences.

#### Background scope

Sometimes we need a more complex retry mechanism that also includes some other factors like connectivity type, time, and others. We can end up using WorkManager to help us achieve that job. WorkManager job can be the one that observes the connection status in the background and tries to send a bunch of messages. Just keep in mind the [100 jobs limit for WorkManager](https://stackoverflow.com/questions/60205993/what-does-workmanager-do-with-jobs-after-hitting-jobscheduler-limit-of-100).

## Problematic points for WebSockets

- Integration tests are hard - how to simulate connectivity issues !?
- Keeping the connection alive when the battery is low (< 15%) is hard - battery optimizations may kill it anytime
- Many times communication with the backend is unclear - you don't receive an acknowledgment message and this makes it hard to implement
- Dealing with async coroutines is hard and you can end up in some threading problems if you execute multiple messages
- There are not many libraries to handle the WebSocket state properly - even Scarlet seems to become outdated

So the next time you think about websockets, keep in mind all of the details mentioned here. Maybe there is something out there that will fit your needs better. gRPC seems kind of better for now - you don't have to observe any connection events there, if it doesn't have a connection it just throws an exception. Sadly, this is not the case with websockets.

I hope I have helped with this post and it was interesting to you. If you find it useful, please feel free to share it.
