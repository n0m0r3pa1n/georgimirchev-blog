---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:19"
_publicize_job_id: "43818455367"
author: gmirchev90
categories:
  - android
date: "2020-05-04T10:13:41+00:00"
guid: http://georgimirchev.com/?p=93
parent_post_id: null
post_id: "93"
tags:
  - mockito
  - test
  - unit-tests
timeline_notification: "1588587221"
title: Mockito verify input parameter was called with a given value
url: /2020/05/04/mockito-verify-input-parameter-was-called-with-a-given-value/

---
Edit 23.04.2018  
According to this [issue](https://github.com/mockito/mockito/issues/584), Mockito cannot capture var-args still.

There is a very easy way to verify a method has been called with the right parameter in Mockito. All you need to do is use captors, to capture the actual argument. Here is how you can use them:

```

ArgumentCaptor argument = ArgumentCaptor.forClass(RuleEntity.class);
 verify(mRulesRepository).createRule(argument.capture());
 assertEquals(1,argument.getValue().getRules().get(0).getTriggers().size())

```

Simple it is, just a quick reminder for starters.
