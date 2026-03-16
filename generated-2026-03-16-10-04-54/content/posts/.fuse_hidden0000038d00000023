---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:31"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "54691372403"
author: gmirchev90
categories:
  - android
date: "2021-02-12T20:50:43+00:00"
guid: http://georgimirchev.com/?p=339
parent_post_id: null
post_id: "339"
summary: |-
  ## The RxJava approach

  I am currently working on an app where the devs before me took this strange decision in regard to the refresh token mechanism - have a single entry point of the app which is called every time whenever you need to have a fresh token. What I mean is this:
tags:
  - authentication
  - oauth2
  - okhttp
  - rxjava
  - token
timeline_notification: "1613163047"
title: Android Oauth2 token refresh mechanism - OkHttp vs RxJava layering
url: /2021/02/12/android-token-refresh-mechanism-okhttp-vs-rxjava-layering/

---
## The RxJava approach

I am currently working on an app where the devs before me took this strange decision in regard to the refresh token mechanism - have a single entry point of the app which is called every time whenever you need to have a fresh token. What I mean is this:

```
class AuthUseCase {
    fun authenticate(): Single<RefreshedState>
}
```

So this method now will be called every time before we do an API request so we make sure that the token is fresh and we won't get a 401 response. This introduces several major issues that I will mention here:

#### RxJava is hard to support

It is really easy to get tangled into the RxJava world. You call this Single, then you flatMap it to a Completable, then you add an Observable and then it is hard to understand what really happens behinds the scenes.

#### RxJava cache can lead to some amazing spaghetti code

This can be really problematic. You put a subject here and there, you add the [replayingShare()](https://github.com/JakeWharton/RxReplayingShare) operator done by Jake Wharton and you are in the deep shit. Basically you can end up in a case where you try to refresh a token that was already refreshed but because you cached it, the thread never understood that it was already refreshed and tries to refresh it again.

#### Concurrency issues with RxJava when multiple API calls or the invalid\_grant hell

Let's say your Android app has a ViewPager that loads 3 fragments immediately and for these 3 screens you make 8 requests in total. You can imagine that each one of these requests calls authenticate(), flatMaps it to do the request with the fresh token and then subscribes to it. Do you feel where this is going? And if this is done by different threads, you end up with a single token being refresh multiple times on different threads.

And this is where you end up with the invalid\_grant exception. Never heard of it? Well, you can refresh a given access token only once. If you try to refresh the same access token twice - you will end up with the invalid\_grant exception. And this is where you end up trying share() or replayingShare() or trying to figure out a [strategy where the refresh happens only once](https://georgimirchev.com/2020/12/17/execute-an-rxjava-observable-only-once-in-a-given-time-period/). And the question that raises up is: Is RxJava the right approach for this task?

## The OkHttp approach

OkHttp is one of the most popular networking libraries especially on android. So it has it's own mechanism to refresh tokens. You know what it is called? Well, it is the [Authenticator](https://square.github.io/okhttp/4.x/okhttp/okhttp3/-authenticator/) class.

#### Using Authenticator

So OkHttp provides you this Authenticator interface which you can extend and override the authenticate method. This method will be called when any requests finishes with 401 code (Unauthorized) so it will be your job to refresh the token and continue the request again. This method is totally synchronized meaning you cannot use RxJava or coroutines. You could actually called blockingGet() and that is totally fine. So here is what I did:

```
class TokenAuthenticator @Inject constructor(
    private val authenticationTokenRefresher: AuthenticationTokenRefresher,
    private val invalidGrantUseCase: InvalidGrantUseCase
) : Authenticator {
    override fun authenticate(route: Route?, response: Response): Request? {
        synchronized(this) {
            try {
                authenticationTokenRefresher.updateLatestAuthState()
            } catch (ex: Exception) {
                if (isGrantAccessError(ex)) {
                    invalidGrantUseCase.logoutWithInvalidGrant()
                    return null
                }

                if (!shouldRetryForError(ex) || responseCountLimitReached(response)) {
                    throw ex
                }
            }

            return response
                .request
                .newBuilder()
                .build()
        }
    }

 private fun responseCountLimitReached(response: Response): Boolean {
        var result = 1
        var responseCounter = response
        while (responseCounter.priorResponse != null) {
            responseCounter = responseCounter.priorResponse!!
            result++
            if (result > REFRESH_TOKEN_RETRY_COUNT) {
                return true
            }
        }
        return result > REFRESH_TOKEN_RETRY_COUNT
    }
}
```

There are several stuff happening here.

1. synchronize makes sure only the first thread refreshes the state while the other ones just receive the updated state
1. authenticationTokenRefresher.updateLatestAuthState checks whether we need to refresh token - if yes, we call blockingGet() on the API service to give us the new access token and then we have a Singleton interceptor that we update with the refreshed token
1. try / catch makes sure we logout the user on invalid\_grant (it really means that the refresh token is invalid and the user need to login again) OR we check whether we need to retry with the request

This is how we guarantee that even if multiple calls happen, the access token will be refreshed only once. Also we don't pollute the code with unnecessary calls to authenticate() every time we need to make a request.

#### Benefits of using OkHttp

1. Switching from poll to push - you don't have to make sure that you always have a refresh token. Instead, when you get a 401 you just refresh it
1. Single place where the code is placed
1. Easy synchronization between threads
1. Reduced code pollution
1. No need into getting a complex token refresh logic
1. No chance of having cached/stale state
1. Already proven solution that works without having to reinvent the wheel

#### Drawbacks of using OkHttp authenticator

The major drawback is that you are tied to the OkHttp class to secure the case where you want to always have a fresh token and succeeding requests. The issue comes when you want to have a websocket implementation or let's say - a player one.

Let's say you have a specific player implementation. It will be tricky to implement OkHttp for it. ExoPlayer comes with the [OkHttpDataSourceFactory](https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/ext/okhttp/OkHttpDataSourceFactory.html) and it is easy to use but for more specific cases - you may end up needing again to refresh the token by yourself.
