---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:29"
_g_feedback_shortcode_93f1588177c82790107488f649771ad9332413e7: |-
  [contact-field required="1" type="name" label="Name"/]

  [contact-field label="Email" required="1" type="email"/]

  [contact-field type="url" label="Website"/]

  [contact-field required="1" type="textarea" label="Message"/]
_g_feedback_shortcode_atts_93f1588177c82790107488f649771ad9332413e7:
  customThankyou: ""
  customThankyouMessage: Thank you for your submission!
  customThankyouRedirect: ""
  id: 248
  show_subject: "no"
  subject: Contact georgimirchev.com
  submit_button_text: Submit
  to: gmirchev90@gmail.com
  widget: 0
_publicize_job_id: "44273099641"
author: gmirchev90
categories:
  - android
  - desktop
date: "2020-05-15T12:20:42+00:00"
guid: http://georgimirchev.com/?p=248
parent_post_id: null
post_id: "248"
summary: In Android we decided that we want to implement a unit test performance monitor. The reason for it is that sometimes some unit tests would execute in more than 1 second and it was all because we would use the Schedulers.trampoline() RxJava scheduler instead of the TestScheduler which is part of the RX package.
tags:
  - gradle
  - kotlin
  - performance
  - plugin
  - sonarqube
  - unit-tests
timeline_notification: "1589545246"
title: Creating a SonarQube custom plugin for Kotlin
url: /2020/05/15/creating-a-sonarqube-custom-plugin-for-kotlin/

---
In Android we decided that we want to implement a unit test performance monitor. The reason for it is that sometimes some unit tests would execute in more than 1 second and it was all because we would use the Schedulers.trampoline() RxJava scheduler instead of the TestScheduler which is part of the RX package.

## How to whole process works?

1. We use Gradle to do most of our tasks. So this means that we will hook into Gradle and make it export a JSON file with the execution time of each test
1. We are also using SonarQube android plugin which will then pass the path & test threshold to SonarQube as a property
1. At the end, the magic that controlls the execution order of all stuff is a python script that we have, this is where it depends on your individual CI setup

## Gradle

First thing was to find a way to export the class name, method name and the excecution time of each test. Here is how it goes in our _testperformance.gradle_:

```
import groovy.json.JsonOutput
import org.gradle.api.tasks.testing.logging.TestExceptionFormat
import org.gradle.api.tasks.testing.logging.TestLogEvent

def testPerformanceResultsDirectory = "$buildDir/test-performance-results"
def fileName = "results.json"

def testsResults = []
tasks.withType(Test) { testTask ->
    jvmArgs += ["-Xmx1536M"]

    testTask.testLogging { logging ->
        events TestLogEvent.FAILED

        exceptionFormat TestExceptionFormat.FULL
        showExceptions true
        showCauses true
        showStackTraces true
    }

    afterTest { TestDescriptor td, TestResult tr ->
        String className = ""
        String packageName = ""
        if (td.className?.trim()) {
            def split = td.className.split("\\.")
            className = split[split.length - 1]
            try {
                packageName = td.className.substring(0, td.className.length() - className.length() - 1)
            } catch (Exception ex) {
                println "Test does not have a package ${td.className}"
                println(ex)
            }
        }

        testsResults.add([
                getClassName  : { className },
                getPackageName: { packageName },
                getName       : { td.name },
                getDuration   : { tr.endTime - tr.startTime },
                getStatus     : { tr.resultType }
        ])
    }
}

gradle.buildFinished {
    if (testsResults.isEmpty()) {
        println "Test results are empty"
        return
    }

    def results = JsonOutput.toJson(testsResults.collect {
        [
                class_name  : it.getClassName(),
                name        : it.getName(),
                package_name: it.getPackageName(),
                duration_ms : it.getDuration(),
                status      : it.getStatus()
        ]
    })

    File testResultsDirectory = new File(testPerformanceResultsDirectory)
    if (!testResultsDirectory.exists()) {
        testResultsDirectory.mkdir()
    }
    new File("$testPerformanceResultsDirectory/$fileName").text = results
    testsResults.clear()
}

```

So basically what happens above is that after each test run we push the result into an array which after everything finishes we visit, take each element from it and then create a JSON file inside _app/build/test-performance-results/results.json_. You can just copy-paste this code inside a separate gradle file and then in _app build.gradle_ put

```
apply from: 'testperformance.gradle'
```

at the top below all the other plugins declaration.

## SonarQube Custom Plugin

Basically what we wanted to achieve is really simple in theory. Grab that JSON report file, see which unit tests take more than 1 second time and then make SonarQube underline the methods as a Major issue with type Code Smell.

There are several ways to achieve this results:

1. Extend an already existing plugin - FindBugs or Detekt
1. Create a totally new custom plugin

### Extend an existing plugin

Existing plugins in SonarQube are:

#### FindBugs

This one is used for analyzing Java code. Sadly, it doesn't work for Kotlin. And also extending it seems tricky.

#### Detekt

Detekt is a Kotlin plugin with quite a lot of different rules integrated inside of it. Actually, extending the plugin is easy like I did [here](https://github.com/n0m0r3pa1n/sonarqube-detekt-plugin). But there are several problems.

- First is that you need to add the detekt Gradle plugin inside your project and then pass the JAR which contains your extension inside of it. I didn't want to add more dependencies inside our project.
- The other things is that there is no way to directly integrate your change inside Detekt. You will always have to either use the [CLI](https://arturbosch.github.io/detekt/cli.html) and pass the JAR as an additional dependency or use the Detekt Gradle plugin or just download the whole Detekt and pack it again. None of these seemed OK for me.

You can check how to extend Detekt with a custom plugin [here](https://arturbosch.github.io/detekt/extensions.html).

### Creating a custom plugin

So basically the decision was made to create a custom plugin. We don't need any other ones to relate to and actually creating a custom plugin is not so hard as I expected it to be.

First, I created the plugin and tried applying a custom rule. In this rule I would try to parse the JSON file and underline the corresponding method which has slow execution. Then the problem came.

[https://docs.sonarqube.org/latest/extend/adding-coding-rules/#header-1](https://docs.sonarqube.org/latest/extend/adding-coding-rules/#header-1)

> Languages not listed here don't support custom rules
>
> SonarQube docs

And Kotlin was not there. So you could add custom rules using the Detekt library, but if you want to write custom rules directly, they will not work. This is when things get a bit tougher.

Then my team lead, who is a contributor to SonarQube, sent me this example: [https://github.com/SonarSource/sonar-custom-plugin-example](https://github.com/SonarSource/sonar-custom-plugin-example)

And actually it contains like 70% of the work that we had to do. Let's summarize the steps that had to be done:

1. Remove all of the JS files from the example and everything NodeJS related
1. Remove all of the unnecessary stuff like custom properties. Leave just the CreateIssuesSensor there
1. Modify the sensor to handle our case
1. Build the custom JAR and deploy it to the extensions/plugins folder in our SonarQube instance

So let's start. The repository with the custom plugin for SonarQube can be seen [HERE](https://github.com/n0m0r3pa1n/SonarQube-Kotlin-Custom-Plugin).

Basically the whole magic is inside the [TestPerformanceSensor](https://github.com/n0m0r3pa1n/SonarQube-Kotlin-Custom-Plugin/blob/master/src/main/java/com/georgimirchev/plugins/testperformance/rules/TestPerformanceSensor.java). In the execute function several things happen:

1. We take the path to the results.json file from Gradle
1. We take the threshold MS for each tests as a property
1. Then we take all of the methods from results.json which take more than the given threshold
1. And we find the corresponding files and line for each method name
1. We create a new issue for that line with the given description

## Android SonarQube configuration

Inside the allprojects build.gradle file we set the sonarqube properties:

```
sonarqube {
        properties {
            property 'sonar.testperformance.report', 'app/build/test-performance-results/results.json'
            property 'sonar.testperformance.threshold_ms', "1000"
        }
}
```

Inside the buildscript dependencies you need the sonarqube plugin

```
classpath "org.sonarsource.scanner.gradle:sonarqube-gradle-plugin:2.7.1"
```

And you also need to apply the plugin

```
apply plugin: 'org.sonarqube'
```

Check **HERE** for more details.

## Running the whole configuration

There is a [deploy.sh](https://plugins.gradle.org/plugin/org.sonarqube) script that I have made. It summarizes each step that needs to be done.

1. Download SonarQube from their website
1. Download the custom plugin repository and cd into it
1. Run **mvn clean package** so you will get the plugin jar built into the target folder of your project
1. Copy the jar from the target folder into SonarQube/extensions/plugins
1. Restart the SonarQube instance by entering SonarQube/bin/windows-linux-mac/sonar.sh restart
1. Open SonarQube (http://localhost:9000), create a new project (save the token somewhere) and go to the Rules section. Select Kotlin language, find the rule there and enable it for Kotlin
1. Open the Android project
1. Run ./gradlew sonarqube --scan --debug -Dsonar.host.url=http://localhost:9000 -Dsonar.login=<TOKEN\_FROM\_SQ\_FOR\_YOUR\_PROJECT> --info
1. After analysis is done you will see the results of this custom plugin under the Test Performance rule

Basically that's all. The magic is in the jar, but the code is pretty clear, just [take a look at it](https://github.com/n0m0r3pa1n/SonarQube-Kotlin-Custom-Plugin).

I hope I have helped someone to create another custom plugin for SonarQube! Enojoy!
