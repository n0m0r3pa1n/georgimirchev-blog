---
author: gmirchev90
date: "2026-09-17T00:00:00+03:00"
title: "Making Android Studio Quail projects open in tabs again on MacOS"
draft: false
tags:
  - android-studio
  - macos
  - troubleshooting
categories:
  - android
---

If you upgraded to Android Studio Quail 4 (2026.1.4) on macOS and lost project tabs or the "Merge All Project Windows" feature, you're not losing your mind. It's a JetBrains Runtime regression, not an IDE setting.

The culprit is bundled JBR 25.0.3-b508.16, which ships with Quail 4 (tracked as IJPL-253186). Most developers assume their $JAVA_HOME or Homebrew JDK is the problem, but those don't affect the IDE itself — only Gradle builds. The UI runs on a separate boot runtime, and that's what broke.

The good news: you don't need to downgrade Android Studio. Just swap out the boot runtime.

## The Fix

1. **Download the previous JBR version**
   - Go to [JetBrains Runtime releases](https://github.com/JetBrains/JetBrainsRuntime/releases)
   - Find version 25.0.3 and download:
     - **Apple Silicon (M1/M2/M3):** [jbr_jcef-25.0.3-osx-aarch64-b329.124.pkg](https://github.com/JetBrains/JetBrainsRuntime/releases/download/jb25.0.3/jbr_jcef-25.0.3-osx-aarch64-b329.124.pkg)
     - **Intel Mac:** [jbr_jcef-25.0.3-osx-x86_64-b329.124.pkg](https://github.com/JetBrains/JetBrainsRuntime/releases/download/jb25.0.3/jbr_jcef-25.0.3-osx-x86_64-b329.124.pkg)

2. **Open Android Studio's boot JVM picker**
   - Press `Shift` twice to open the "Search Everywhere" dialog
   - Type: `Choose Boot Java Runtime`
   - Press Enter

3. **Point it to the downloaded JBR**
   - Click the folder icon to browse
   - Navigate to your downloaded `.pkg` file and select it

4. **Restart Android Studio**
   - The IDE will restart with the new runtime
   - Project tabs and window merging should work again

## Why This Matters

The boot JVM is invisible to most developers — it's bundled with the IDE and only used for the IDE's own UI. When JetBrains ships a bad runtime version, it affects everyone silently until they discover the workaround.

No $JAVA_HOME tweaks needed. No Gradle config changes. Just swap the runtime and move on.
