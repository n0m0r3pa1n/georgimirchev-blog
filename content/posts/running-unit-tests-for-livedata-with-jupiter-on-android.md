---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:30"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "50971247411"
author: gmirchev90
categories:
  - android
date: "2020-11-12T09:27:01+00:00"
guid: http://georgimirchev.com/?p=290
parent_post_id: null
post_id: "290"
summary: |-
  Running unit tests for ViewModels on Android where you have LiveData objects can be tricky. It is very possible that you get this exception:

  **java.lang.RuntimeException: Method getMainLooper in android.os.Looper not mocked.**
tags:
  - kotlin
  - livedata
  - unit-tests
timeline_notification: "1605173221"
title: Running unit tests for LiveData with Jupiter on Android
url: /2020/11/12/running-unit-tests-for-livedata-with-jupiter-on-android/

---
Running unit tests for ViewModels on Android where you have LiveData objects can be tricky. It is very possible that you get this exception:

**java.lang.RuntimeException: Method getMainLooper in android.os.Looper not mocked.**

Creating a custom rule won't work if you are using JUnit 5 (Jupiter). The proper way is to have an extension like this one:

```
import androidx.arch.core.executor.ArchTaskExecutor
import androidx.arch.core.executor.TaskExecutor
import org.junit.jupiter.api.extension.AfterEachCallback
import org.junit.jupiter.api.extension.BeforeEachCallback
import org.junit.jupiter.api.extension.ExtensionContext

class InstantExecutorExtension : BeforeEachCallback, AfterEachCallback {
    override fun beforeEach(context: ExtensionContext?) {
        ArchTaskExecutor.getInstance().setDelegate(object : TaskExecutor() {
            override fun executeOnDiskIO(runnable: Runnable) = runnable.run()
            override fun postToMainThread(runnable: Runnable) = runnable.run()
            override fun isMainThread(): Boolean = true
        })
    }

    override fun afterEach(context: ExtensionContext?) {
        ArchTaskExecutor.getInstance().setDelegate(null)
    }
}
```

And then over the test class that you have, you have to add the following annotation:

```
@ExtendWith(InstantExecutorExtension::class)
class MainActivityViewModelTest { }
```

Ta-Da. This is all you need in order to test LiveData classes.
