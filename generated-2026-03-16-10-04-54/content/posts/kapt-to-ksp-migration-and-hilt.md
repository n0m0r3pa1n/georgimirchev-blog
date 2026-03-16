---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:41"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "91080214006"
author: gmirchev90
categories:
  - android
date: "2024-01-14T14:37:25+00:00"
firehose_sent: "1705243045"
guid: https://georgimirchev.com/?p=1670
parent_post_id: null
post_id: "1670"
reader_suggested_tags: '["Games","Gaming","Nasional","Writing"]'
summary: There is an issue with Dagger / Hilt when you migrate your project from the [KAPT](https://kotlinlang.org/docs/kapt.html) to [KSP](https://kotlinlang.org/docs/ksp-overview.html) code generation plugin.
tags:
  - dagger
  - hilt
  - kapt
  - ksp
timeline_notification: "1705243045"
title: KAPT to KSP migration and Hilt
url: /2024/01/14/kapt-to-ksp-migration-and-hilt/
wordads_ufa: u:wpcom-ufa-v4:1705243587
wpcom_is_first_post: "1"

---
There is an issue with Dagger / Hilt when you migrate your project from the [KAPT](https://kotlinlang.org/docs/kapt.html) to [KSP](https://kotlinlang.org/docs/ksp-overview.html) code generation plugin.

When you try to provide BuildConfig values through a Dagger module or if you try to provide DataBinding values, then we end up with the following error:

```
@Module
@InstallIn(SingletonComponent::class)
object KeypadModule {

    @Provides
    @ShuffledKeypad
    fun provideIsKeypadEnabled() = BuildConfig.IS_KEYPAD_ENABLED
}
```

Error:

```
e: [ksp] ModuleProcessingStep was unable to process 'com.test.keypad.KeypadModule' because 'error.NonExistentClass' could not be resolved.

Dependency trace:
    => element (OBJECT): com.test.keypad.KeypadModule
    => element (METHOD): provideIsKeypadEnabled()
    => type (ERROR return type): error.NonExistentClass
```

The fix is in the following Google ticket -> [https://issuetracker.google.com/301245705](https://issuetracker.google.com/301245705)

```
androidComponents {
onVariants(selector().all(), { variant ->
            afterEvaluate {
                // This is a workaround for https://issuetracker.google.com/301245705 which depends on internal
                // implementations of the android gradle plugin and the ksp gradle plugin which might change in the future
                // in an unpredictable way.
                project.tasks.getByName("ksp" + variant.name.capitalize() + "Kotlin") {
                    def buildConfigTask = (GenerateBuildConfig) project.tasks.getByName("generate${variant.name.capitalize()}BuildConfig")
                    ((AbstractKotlinCompileTool) it).setSource(buildConfigTask.sourceOutputDir)
                }
            }
        })
    }
```

You need to add the above code to your app build.gradle file.
