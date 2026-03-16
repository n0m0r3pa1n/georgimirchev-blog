---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:23"
_publicize_job_id: "43823657442"
author: gmirchev90
categories:
  - android
date: "2020-05-04T13:04:36+00:00"
guid: http://georgimirchev.com/?p=167
parent_post_id: null
post_id: "167"
summary: |-
  You probably know the famous card detection library called [card.io](https://www.card.io/). It is developed by PayPal and used worldwide. There is a lot of [OpenCV](http://opencv.org/) usage behind it, but I will leave this for another article.

  ## The problem

  The problem is that the library can only be used as a separate module. You can call the card.io Activity and it will return you the results from the detection. But what if you want to embed the detection view inside your app? Well, sadly, there is no fragment for that. But the library is open-source so you can change everything to fit your needs. So let’s start then!
tags:
  - cardio
  - fragment
timeline_notification: "1588597479"
title: Exporting Card.IO as a fragment
url: /2020/05/04/exporting-card-io-as-a-fragment/

---
You probably know the famous card detection library called [card.io](https://www.card.io/). It is developed by PayPal and used worldwide. There is a lot of [OpenCV](http://opencv.org/) usage behind it, but I will leave this for another article.

## The problem

The problem is that the library can only be used as a separate module. You can call the card.io Activity and it will return you the results from the detection. But what if you want to embed the detection view inside your app? Well, sadly, there is no fragment for that. But the library is open-source so you can change everything to fit your needs. So let’s start then!

## First steps

1. You need to copy the \[card.io-Android-Source\](https://github.com/card-io/card.io-Android-source) files to your project – “git clone **–recursive** git@github.com:card-io/card.io-Android-source.git” (Don’t forget to clone it with the recursive option so the card.io submodule will be downloaded!)
1. You need to add it as a module – Right click on your module folder -> Open Module Settings -> The green plus at top left -> Import Gradle Project -> Select your project folder
1. Clean up the build.gradle file – you will not need many of the plugins there like signing or nexus-staging.

Add the following code in your **root project** build.gradle next to the settings.gradle so the card.io can find the needed dependencies.

```
project.ext {
    sdkVersion = 25
    buildToolsVersion = "25.0.2"
}
```

After you finish importing the project, open again your module settings and add is as a dependency to the “app” module.  

So far, so good. You have the code within your app and now we can modify it to our needs.

## Observe before create

If we dig into the code we see there is the [CardIOActivity class](https://github.com/card-io/card.io-Android-source/blob/master/card.io/src/main/java/io/card/payment/CardIOActivity.java). It looks suspicious and if we dig a little bit deeper we find it uses the [CardScanner class](https://github.com/card-io/card.io-Android-source/blob/master/card.io/src/main/java/io/card/payment/CardIOActivity.java#L484) to receive data about scanned cards. And the bad thing is that the [CardScanner class holds a reference to the CardIOActivity](https://github.com/card-io/card.io-Android-source/blob/master/card.io/src/main/java/io/card/payment/CardScanner.java#L87). We will need to change that.

The second observation of the CardIOActivity usages leads us to the [OverlayView class](https://github.com/card-io/card.io-Android-source/blob/master/card.io/src/main/java/io/card/payment/OverlayView.java#L121) which again uses CardIOActivity to call the toggleFlash and triggerAutoFocus methods on it. We will need to change that too.

## Creating CardIOFragment

### Problem 1 – CardScanner

The CardScanner class [works directly with CardIOActivity](https://github.com/card-io/card.io-Android-source/blob/master/card.io/src/main/java/io/card/payment/CardScanner.java#L192). This is a very poor design decision for me as we all know tight coupling is something that should be avoided in the programming world. The first thing to do is make the CardScanner class public and make it work with an abstraction, instead of working with the activity directly.

#### Abstracting CardIOActivity from CardScanner

The next thing we notice is that there are 3 properties which are controlled from the [intent with which the activity was started](https://github.com/card-io/card.io-Android-source/blob/master/card.io/src/main/java/io/card/payment/CardScanner.java#L195-L198). mSupressScan is [passed in the nSetup method](https://github.com/card-io/card.io-Android-source/blob/master/card.io/src/main/java/io/card/payment/CardScanner.java#L202), so we will pass it through the constructor. Other properties can have setter methods.

```
    public CardScanner(CardIOScanDetection scanDetection, int currentFrameOrientation, boolean supressScan) {
        this.mSuppressScan = supressScan;

        mScanActivityRef = new WeakReference<>(scanDetection);
        mFrameOrientation = currentFrameOrientation;
        nSetup(mSuppressScan, MIN_FOCUS_SCORE, mUnblurDigits);
    }

    public void setScanExpiry(boolean scanExpiry) {
        this.mScanExpiry = scanExpiry;
    }

    public void setUnblurDigits(int unblurDigits) {
        this.mUnblurDigits = unblurDigits;
    }

```

mScanActivityRef is the variable holding a reference to the CardIOActivity. We will make it hold a reference to an interface instead. Let’s call the interface CardIOScanDetection. If we observe the usage of the variable, here are the methods it should have:

```
public interface CardIOScanDetection {
    Activity getActivity();
    void onFirstFrame(int orientation);
    void onCardDetected(Bitmap detectedBitmap, DetectionInfo dInfo);
    void onEdgeUpdate(DetectionInfo dInfo);
}

```

And after that with some smaller tweaks to the CardIOActivity class, it should be working with the new CardScanner.

```
// Add the implements CardIOScanDetection
public final class CardIOActivity extends Activity implements CardIOScanDetection {
    // Override the needed methods
    @Override
    public Activity getActivity() {
        return this;
    }

    ...

    // Fix creation of scanner
    mCardScanner = new CardScanner(this, mFrameOrientation, false);

   // Fix modifiers on the methods from the CardIOScanDetection interface
   public void onEdgeUpdate ...
   public void onCardDetected ...
}

```

Great! Now we can pass everything that implements the CardIOScanDetection interface to the scanner and the scanner should return data using a weak reference to it.

### Problem 2 – Abstracting CardIOActivity from the OverlayView

As we saw from our observations, the OverlayView has a variable which [keeps a reference to the CardIOActivity](https://github.com/card-io/card.io-Android-source/blob/master/card.io/src/main/java/io/card/payment/OverlayView.java#L400-L402). We need to abstract that reference. So we create the interface called CardIOCameraControl. Here is how it looks like:

```
public interface CardIOCameraControl {
    void toggleFlash();
    void triggerAutoFocus();
}

```

So we change the constructor to receive the Activity class and the CardIOCameraControl interface, which is assigned to the mScanActivityRef variable. We don’t need to depend on CardIOActivity again. Here is a part of the changes:

```
public OverlayView(Activity activity, CardIOCameraControl cameraControl, AttributeSet attributeSet, boolean showTorch) {
        super(activity, attributeSet);

        mShowTorch = showTorch;
        mScanActivityRef = new WeakReference(cameraControl);
        ...
        mLogo = new Logo(activity);
}

```

Great, now OverlayView is independent of CardIOActivity. We can pass whatever implements CardIOCameraControl. Here is how CardIOActivity looks now:

```
public final class CardIOActivity extends Activity implements CardIOScanDetection, CardIOCameraControl
...
mOverlay = new OverlayView(this, this, null, Util.deviceSupportsTorch(this));

```

### Problem 3 – The ugly part

Here is the part where a bunny cries. We need to copy the whole functionality from the CardIOActivity to the CardIOFragment. The sad story about the code in the CardIOActivity is that it does way too many things:

- Controls the camera – open/start/stop/exceptions
- Controls the camera overlay
- Controls the camera preview & restarts it when needed
- Detects processor support
- Fixes the image size

There are just too many responsibilities that this class has. It is 1087 lines which are a HUGE indication that something is going wrong. I will skip you all the details on moving all the CardIOActivity logic to the fragment class. I removed the usage of the application theme, some unused checks and it was working as far as I tested it. In either case, you should get the idea how the export should look like so you can do it yourself. You can check the implementation of the [CardIO fragment](https://github.com/n0m0r3pa1n/card.io-Android-source/blob/master/card.io/src/main/java/io/card/payment/CardIOFragment.java) I made.

## Final words

You can find all of the source code of the [project here](https://github.com/n0m0r3pa1n/cardio_android_fragment) and the modified card.io [Android source here](https://github.com/n0m0r3pa1n/card.io-Android-source). You can visit the “commits” section to see what I have added and the changes made. Feel free to modify it or make it better. I hope that I have helped someone to use card.io as part of his activity.
