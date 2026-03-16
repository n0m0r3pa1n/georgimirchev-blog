---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:32"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "56762866721"
author: gmirchev90
categories:
  - android
date: "2021-04-04T12:43:17+00:00"
guid: https://georgimirchev.com/?p=434
parent_post_id: null
post_id: "434"
summary: Gson is outdated a bit with the last commit done [quite some time ago](https://github.com/google/gson). We also had implemented the Moshi library in our Gradle dependencies and these are the 2 reasons why we decided to move away from GSON.
tags:
  - gson
  - json
  - moshi
timeline_notification: "1617540201"
title: Migration from Gson to Moshi - small tricky parts
url: /2021/04/04/migration-from-moshi-to-gson-small-tricky-parts/

---
Gson is outdated a bit with the last commit done [quite some time ago](https://github.com/google/gson). We also had implemented the Moshi library in our Gradle dependencies and these are the 2 reasons why we decided to move away from GSON.

## Guidance

There is an [amazing article](https://proandroiddev.com/goodbye-gson-hello-moshi-4e591116231e) from ProAndroidDev on how to migrate from Gson to Moshi. But they forgot to mention some tricky parts especially when it comes to using ProGuard.

## Tricky parts

### ProGuard

ProGuard obfuscation is always a nightmare for every android dev. You never know whether your app will work properly or not. And the tricky part here is that Moshi can actually use code generation and not use the reflection by default. But if you use reflection, you will need this rule in order to make it work:

```
-dontwarn org.jetbrains.annotations.**
-keep class kotlin.Metadata { *; }

-keepclassmembers class my.models.package.** {
  <init>(...);
  <fields>;
}
```

It will keep the constructors and fields of your JSON class so they will be properly initialized when using reflection.

### List != ArrayList

Moshi doesn't recognize ArrayList as an option. It works with generic lists but if you expect to receive or send an ArrayList, you are in trouble. Moshi does not work with concrete classes.

### @JsonClass(generateAdapter = true)

If you are using Moshi with codegen, don't forgot to add the above annotation to all of your data classes. Otherwise reflection will be used.

### .add(KotlinJsonAdapterFactory())

Never forget to add the KotlinJsonAdapterFactory to an instance of Moshi. This is the last measure that Moshi will use to serialize/deserialize the JSON. This is the part that tells it to use reflection.

### .adapter(AnyClass::class.java) slow

If you use something like moshi.adapter(AnyClass::class.java) during the initialization of your class, then keep in mind this will add at least a second to the startup time. The reason is that it immediately tries to use the PlymorphicJsonAdapterFactory to use reflection when JSON is received. So either lazy initialize it or use Moshi codegen.
