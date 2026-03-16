---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:40"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "78169424252"
author: gmirchev90
categories:
  - android
date: "2022-10-28T08:49:16+00:00"
guid: https://georgimirchev.com/?p=1006
parent_post_id: null
post_id: "1006"
summary: |-
  [KtLint](https://github.com/pinterest/ktlint) & [Detekt](https://github.com/detekt/detekt) are tools that help us keep our code formatted in the same way and sometimes even help us spot errors before they are actually merged to master or develop.

  Most of the CI integrations already include a detekt step before the unit tests are run. To avoid waiting for the CI, you can easily integrate ktlint as part of Git hooks. So every time when you decide to commit, Git hooks run and ktlint analyzes the committed files and prevents you from pushing until you fix your errors.
tags:
  - android-studio
  - detekt
  - ktlint
timeline_notification: "1666946958"
title: Local KtLint configuration for Android
url: /2022/10/28/local-ktlint-configuration-for-android/

---
[KtLint](https://github.com/pinterest/ktlint) & [Detekt](https://github.com/detekt/detekt) are tools that help us keep our code formatted in the same way and sometimes even help us spot errors before they are actually merged to master or develop.

Most of the CI integrations already include a detekt step before the unit tests are run. To avoid waiting for the CI, you can easily integrate ktlint as part of Git hooks. So every time when you decide to commit, Git hooks run and ktlint analyzes the committed files and prevents you from pushing until you fix your errors.

{{< figure src="/wp-content/uploads/2022/10/image.png?w=1024" alt="" caption="" >}}

## Setting up pre-commit hook

The first thing you need to set up is the pre-commit hook. This script will be executed every time before you actually commit some code. It will check which the changed files are and then execute ktlint on them. This is a sample pre-commit hook:

**pre-commit.sh**

```
#!/usr/bin/env bash
OUTPUT="/tmp/ktlint-$(date +%s)"
export PATH=/usr/local/bin:$PATH
git diff --name-only --cached --relative | grep '\.kt[s"]\?$' | grep -v "Test.kt" | grep -v "Navigator.kt" | xargs ktlint --android --relative . > "$OUTPUT"
EXIT_CODE=$?
if [ $EXIT_CODE -ne 0 ]; then
  echo "***************************************************"
  echo "                   Ktlint failed                   "
  echo " Please fix the following issues before committing "
  echo "***************************************************"
  cat "$OUTPUT"
  exit $EXIT_CODE
fi
```

## Installing the pre-commit hook

To install this hook, all you need to do is actually copy it to .git/hooks folder of your project. But as you may want to do it for every team member on the team, it makes sense to include it automatically somewhere in the project lifecycle. For example, on a clean build. Here is some sample Groovy code that does it:

**git-hooks.gradle**

```
static def isLinuxOrMacOs() {
    def osName = System.getProperty('os.name').toLowerCase(Locale.ROOT)
    return osName.contains('linux') || osName.contains('mac os') || osName.contains('macos')
}

task copyGitHooks(type: Copy) {
    description 'Copies the Git hooks from config/git/scripts/ to the .git/hooks folder.'
    from("${rootDir}/config/git/script") {
        include '**/*.sh'
        rename '(.*).sh', '$1'
    }
    into "${rootDir}/.git/hooks/"
    onlyIf { isLinuxOrMacOs() }
}

task installGitHooks(type: Exec) {
    description 'Installs the Git hooks from script/git-hooks.'
    group 'git hooks'
    workingDir rootDir
    commandLine 'chmod'
    args '-R', '+x', '.git/hooks'
    dependsOn copyGitHooks
    onlyIf { isLinuxOrMacOs() }
    doLast {
        logger.info('Git hooks installed successfully.')
    }
}

afterEvaluate {
    tasks['clean'].dependsOn installGitHooks
}
```

Applying this gradle file is as simple as

```
apply from: "$rootDir/config/git/git-hooks.gradle"
```

This is done inside your project/build.gradle (not the app one). After that, just do a clean build and the hook should be moved to your hooks folders. Go and check it out yourself.

If not, you can always manually run the task in gradle:

```
./gradlew installGitHooks
```

## Install & configure ktlint

Installing ktlint on OSX is quite simple:

```
brew install ktlint
```

Now you can enter your project folder and just run **ktlint** to see what the output will be. Maybe there are some rules that you want to ignore. You can put them inside the .editorconfig file in the root folder of your project. Sample:

**.editorconfig**

```
[*.{kt,kts}]
max_line_length = 150
ij_kotlin_allow_trailing_comma = true
ij_kotlin_allow_trailing_comma_on_call_site = true
ktlint_disabled_rules = import-ordering
```

## Close and open Android Studio again

Once you do that, the next time you try to commit, in the commit preferences "Run with Git hooks" will be enabled and you will start seeing the errors (if you create any) in a dialog that shows up. If you want to skip the ktlint check, just uncheck this option before you commit and you will able to commit and push and let the CI detect any issues with your code.

{{< figure src="/wp-content/uploads/2022/10/image-1.png?w=726" alt="" caption="" >}}

## In case of an error after updating ktlint

In case you start seeing hundreds of errors after you update ktlint, then feel free to regenerate the ktlint hook:

```
./gradlew installGitHooks
ktlint installGitPreCommitHook
```

This will probably resolve your issue.

## That's it

Simple as that, you have ktlint installed. Keep in mind different versions of ktlint may cause different issues, so try to update it as often as you can.
