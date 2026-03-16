---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:25"
_publicize_job_id: "43830682443"
author: gmirchev90
categories:
  - android
date: "2020-05-04T16:26:26+00:00"
guid: http://georgimirchev.com/?p=203
parent_post_id: null
post_id: "203"
summary: |-
  Situation is like this:

  1. You got an AccessToken and RefreshToken (AT and RT for now on)
  2. Every API call needs to contain the AT
  3. When the AT expires you need to refresh it using your RT
tags:
  - okhttp
  - retrofit
  - token
timeline_notification: "1588609586"
title: Retrofit – Authorization &amp; Expiring Tokens
url: /2020/05/04/retrofit-authorization-expiring-tokens/

---
Situation is like this:

1. You got an AccessToken and RefreshToken (AT and RT for now on)
1. Every API call needs to contain the AT
1. When the AT expires you need to refresh it using your RT

## Interceptor: Attaching Access Token to every network request

What you need is a [Network Interceptor](https://github.com/square/okhttp/wiki/Interceptors#network-interceptors). Interceptors are a very powerful mechanism to rewrite calls. After hooking the interceptor to the OkClient every single call will pass through it. That is the perfect moment to attach your AT:

```
@Override
public Response intercept(Chain chain) throws IOException {
   Request originalRequest = chain.request();

   // Add authorization header with updated authorization value to intercepted request
   Request authorisedRequest = originalRequest.newBuilder()
           .header(AUTHORIZATION, accessToken)
           .build();
   return chain.proceed(authorisedRequest);
}
```

You can tweak this interceptor to handle edge cases. For example, when your accessToken is empty or when the request already contains an authorization header you may want to skip the rebuild. This can easily be done:

```
@Override
public Response intercept(Chain chain) throws IOException {
   Request originalRequest = chain.request();

   // Nothing to add to intercepted request if:
   // a) Authorization value is empty because user is not logged in yet
   // b) There is already a header with updated Authorization value
   if (authorizationTokenIsEmpty() || alreadyHasAuthorizationHeader(originalRequest)) {
       return chain.proceed(originalRequest);
   }

   // Add authorization header with updated authorization value to intercepted request
   Request authorisedRequest = originalRequest.newBuilder()
           .header(AUTHORIZATION, authorizationValue)
           .build();
   return chain.proceed(authorisedRequest);
}
```

## Authenticator: Refreshing Access Token

How do you know it is time to refresh your AT? This is the approach:

- Monitor to network calls looking for errors.
- Whenever you get a `401 Not Authorised` it means your AT is no longer valid.
- Refresh AT using RT.
- Repeat failed call.

This can be accomplish with `Interceptors` as well, but there is a much better way: [Authenticators](http://square.github.io/okhttp/javadoc/com/squareup/okhttp/Authenticator.html) an interface designed specifically for [this purpose](https://github.com/square/okhttp/wiki/Recipes#handling-authentication).

When you set up an `Authenticator` `OkHttp` client will **automatically** ask the Authenticator for credentials when a response is `401 Not Authorised` **retrying last failed** request with provied new credentials.

```
@Override
public Request authenticate(Proxy proxy, Response response) throws IOException {
   // Refresh your access_token using a synchronous api request
   newAccessToken = service.refreshToken();

   // Add new header to rejected request and retry it
   return response.request().newBuilder()
           .addHeader(AUTHORIZATION, newAccessToken)
           .build();
}

```

## Setting everything up

Create an `OkHttpClient` and attach your `NetworkInterceptor` and your `Authenticator` to it

```
OkHttpClient okHttpClient = new OkHttpClient();
okHttpClient.networkInterceptors().add(authInterceptor);
okHttpClient.setAuthenticator(authAuthenticator);

```

Then use this client when creating your `Retrofit RestAdapter`

```
RestAdapter restAdapter = new RestAdapter.Builder()
                .setEndpoint(ENDPOINT)
                .setClient(new OkClient(okHttpClient))
                .build();
return restAdapter.create(API.class);
```
