---
_elasticsearch_data_sharing_indexed_on: "2025-10-11 07:05:00"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "108073364743"
_publicize_shares: []
author: gmirchev90
categories:
  - android
date: "2025-10-11T07:07:12+00:00"
firehose_sent: "1760166432"
guid: https://georgimirchev.com/?p=2581
parent_post_id: null
post_id: "2581"
previously_liked_199120357: "1"
summary: |-
  I had to implement this migration for an object that was stored in DataStore using Protobuf and Kotlinx Serialisable on Android. The **original object** looked like this:

  ```
  @Serializable
  data class Assignment(val state: State)

  @Serializable
  sealed interface State {
    @Serializable
    data object Pending : State
     // More implementations
  }
  ```

  The **new object** was supposed to look like this:

  ```
  @Serializable
  sealed interface State {
    @Serializable
    data class Pending(val id: String) : State
     // More implementations
  }
  ```
tags:
  - datastore
timeline_notification: "1760166433"
title: Fixing Android DataStore migration from a data object to a data class
url: /2025/10/11/fixing-android-datastore-migration-from-a-data-object-to-a-data-class/

---
I had to implement this migration for an object that was stored in DataStore using Protobuf and Kotlinx Serialisable on Android. The **original object** looked like this:

```
@Serializable
data class Assignment(val state: State)

@Serializable
sealed interface State {
  @Serializable
  data object Pending : State
   // More implementations
}
```

The **new object** was supposed to look like this:

```
@Serializable
sealed interface State {
  @Serializable
  data class Pending(val id: String) : State
   // More implementations
}
```

As you can see, the sealed interface implementation of Pending changed from a data object to a data class. But the problem is that the original object was stored in DataStore, so executing this migration directly would result in the following crash:

```
Caused by: java.lang.NoSuchFieldError: No field INSTANCE of type Lcom/georgimirchev/****/State$Pending; or its superclasses (declaration of /data/data/****/State$Pending) appears in /data/data/***/classes16.dex
```

The issue is that DataStore continued to try to serialize the object by using the INSTANCE field although the object was no longer such; it became a data class.

I tried many things, like:

- Clearing the DataStore file and purely renaming it
- Uninstall and reinstall the app
- Making the state field nullable

And none of the above worked. Only 2 things worked:

A) Renaming the Pending field  
 B) Creating the INSTANCE field in the companion object of the class itself

I chose approach B) as it seems the least invasive one. The thing I did was simply this:

```
data class Pending(val id: String?) : State {
  companion object {
    @JvmField
    val INSTANCE = Pending(null)
  }
}
```

By providing the INSTANCE field by using JvmField and the companion object it seems that it tricked DataStore that it could create the data object it was using before. Hopefully this works for you too!
