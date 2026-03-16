---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:20"
_publicize_job_id: "43819332852"
author: gmirchev90
categories:
  - android
date: "2020-05-04T10:43:12+00:00"
guid: http://georgimirchev.com/?p=106
parent_post_id: null
post_id: "106"
summary: 'Recently, I had a very strange problem to debug. I had the following interactor:'
tags:
  - rxjava
  - schedulers
timeline_notification: "1588588992"
title: .blockingGet &amp; AndroidSchedulers.mainThread() make the app stuck
url: /2020/05/04/blockingget-androidschedulers-mainthread-make-the-app-stuck/

---
Recently, I had a very strange problem to debug. I had the following interactor:

```
public class GetUserUpdatesInteractor {
    private final UserRepository mUserRepository;
    private final InteractorSchedulers mSchedulers;

    @Inject
    public GetUserUpdatesInteractor(UserRepository userRepository, InteractorSchedulers schedulers) {
        mUserRepository = userRepository;
        mSchedulers = schedulers;
    }

    public Observable<Optional<User>> getUserUpdates() {
        return mUserRepository.getUserUpdates()
               .subscribeOn(mSchedulers.getExecutionScheduler()) // Schedulers.io()
               .observeOn(mSchedulers.getPostExecutionScheduler()); // Android mainThread()
    }

}

```

The problem was that I wanted to makes a blocking call on the interactor. So I would do the following:

```
Optional<User> user = mGetUserUpdatesInteractor.getUserUpdates().blockingFirst();

```

The above line would make the app stuck in a deadlock and totally unresponsive. A short [search in StackOverflow](https://stackoverflow.com/questions/46614494/blockingget-block-ui-thread-rxjava-2) shows that using a blockingFirst() on something that has already set the AndroidSchedulers.mainThread() does not work well. In order for the app the be working, the schedulers must be moved outside of the interactors layer and let each caller decide whether to set schedulers or not.
