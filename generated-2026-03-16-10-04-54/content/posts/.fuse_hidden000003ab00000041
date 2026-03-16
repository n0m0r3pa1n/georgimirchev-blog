---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:19"
_publicize_job_id: "43818514796"
author: gmirchev90
categories:
  - android
date: "2020-05-04T10:15:48+00:00"
guid: http://georgimirchev.com/?p=96
parent_post_id: null
post_id: "96"
summary: Probably, most of you already know this, but it is good to keep it in mind when you want to dynamically change the values returned by a mock you have.
tags:
  - mockito
  - test
  - unit-tests
timeline_notification: "1588587348"
title: How to mock a dynamic value returned from a method with Mockito?
url: /2020/05/04/how-to-mock-a-dynamic-value-returned-from-a-method-with-mockito/

---
Probably, most of you already know this, but it is good to keep it in mind when you want to dynamically change the values returned by a mock you have.

```
doAnswer(invocation -> {
            @SuppressWarnings("unchecked")
            RestCallback restCallback = (RestCallback)  invocation.getArguments()[0];
            restCallback.onSuccess(null);
            return null;
        }).when(mApi).updateNode(Matchers.anyObject(),
                any(NodeEntity.class),
                eq(NODE_ID))
﻿
```

The most important things here are:  
– doAnswer – method that accepts the invocation from which you can get the actual arguments and mock the return value  
– when – method that is used on the mocked instance and does the magic to return whatever you want

Another way to mock the returned value is to use:

```
verify(mApi).getSchedule(NODE_ID);
```

which actually verifies that the mApi.getSchedule was called with the NODE\_ID constant.
