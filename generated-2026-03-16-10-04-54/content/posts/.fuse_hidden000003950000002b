---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:36"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "61976843501"
author: gmirchev90
categories:
  - android
date: "2021-08-17T20:12:52+00:00"
guid: https://georgimirchev.com/?p=556
parent_post_id: null
post_id: "556"
summary: Generating a signed build using Android Studio Arctic Fox version 2021.3.1 seem to be a tricky thing to do. I spent several hours just to figure out where the issue is and at the end, I still don't know what is the exact reason for signing to not work.
tags:
  - android-studio
  - apk
  - signing
timeline_notification: "1629231176"
title: Android Studio Arctic Fox not building a signed APK
url: /2021/08/17/android-studio-arctic-fox-not-building-a-signed-apk/
wordads_ufa: s:wpcom-ufa-v3-beta:1667983708

---
Generating a signed build using Android Studio Arctic Fox version 2021.3.1 seem to be a tricky thing to do. I spent several hours just to figure out where the issue is and at the end, I still don't know what is the exact reason for signing to not work.

### The issue

When you go to:

- Generate a Signed Build
- Generate an APK
- Choose the keystore and enter passwords
- Generate a release or debug build

The build that is generated is not signed.

### How to verify that a build is signed

There are two ways to verify that a build was signed using a keystore successfully.

1. Use the jarsigner tool

```
 jarsigner -verify -verbose -certs <path_to_your_apk>
```

This is how a successful verification looks. At the end it says: " **jar verified**". When the verification cannot be achieved it displays a short message saying " **jar is unsigned**".

{{< figure src="/wp-content/uploads/2021/08/screenshot-2021-08-17-at-22.43.22.png?w=1024" alt="" caption="" >}}

The jarsigner tool can be found in _~/Library/Java/JavaVirtualMachines/openjdk-15.0.1/Contents/Home/bin/_

2\. Use the apksigner tool

```
./apksigner verify -v <path_to_your_apk>
```

The apksigner tool is part of the Android SDK package. _You can find it in ~/Library/Android/sdk/build-tools/31.0.0._

{{< figure src="/wp-content/uploads/2021/08/image.png?w=836" alt="" caption="" >}}

apksigner shows all of the signing methods used on the apk.

### Fixing the APK that is not signed

To fix the issue in my case, I had to do a very simple thing:

```
android {
*****
signingConfigs {
    release {
        storeFile file("../keystore.jks")
        storePassword "pass123"
        keyAlias "key"
        keyPassword "pass123"
        v1SigningEnabled true
        v2SigningEnabled true
    }

    debug {
        storeFile file("../keystore.jks")
        storePassword "pass123"
        keyAlias "key"
        keyPassword "pass123"
        v1SigningEnabled true
        v2SigningEnabled true
    }
}

 buildTypes {
        release {
            debuggable true
            signingConfig signingConfigs.release
        }

        debug {
            applicationIdSuffix ".debug"
            signingConfig signingConfigs.debug
            debuggable true
        }
    }
}
```

Setting the **v1SigningEnabled** and **v2SigningEnabled** to true fixed the issue. Now the APK is signed successfully. If you remove these two parameters, Android Studio Arctic Fox never signs the APK even if you trigger the process manually by clicking on the "Generate signed build" option under Build menu.

### In case it still doesn't work

In our case we had a branch with the Android gradle plugin:

```
classpath "com.android.tools.build:gradle:4.0.1"
```

It seems like version 4.0.1 is buggy. Change this version to 4.2 and problem is resolved. Also make sure you use the latest gradle wrapper or at least:

```
distributionUrl=https\://services.gradle.org/distributions/gradle-6.7.1-bin.zip
```

### **Bonus: Manually signing the APK**

You can always sign the APK manually using the **apksigner** tool. The command is very simple:

```
apksigner sign --ks keystore.jks app.apk
```

It is good if you run the zipalign tool before that. It can be found in the build-tools folder too:

```
zipalign -p -f -v 4 infile.apk outfile.apk
```
