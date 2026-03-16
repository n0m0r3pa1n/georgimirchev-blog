---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:26"
_publicize_job_id: "43830776129"
author: gmirchev90
categories:
  - android
date: "2020-05-04T16:28:58+00:00"
guid: http://georgimirchev.com/?p=212
parent_post_id: null
post_id: "212"
tags:
  - mockito
  - unit-tests
timeline_notification: "1588609738"
title: Mockito anyString() vs any() – null handling
url: /2020/05/04/mockito-anystring-vs-any-null-handling/

---
Sometimes, when I mock a method in Mockito, I used **anyString** instead of **any** and I get into a mess and my test fails. Why?

```
given(mUserRepo.getUser(anyString(). any())).willReturn(new User());
```

And what will happen if

```
mUserRepo.getUser(null);
```

UserRepo is called with null? Well, the matcher doesn’t work and the test fails. **anyString()** does not work with null values. If you pass null to a mocked service then use **any()** as a matcher.
