---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:32"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "56752547575"
author: gmirchev90
categories:
  - android
date: "2021-04-04T06:04:55+00:00"
guid: https://georgimirchev.com/?p=412
parent_post_id: null
post_id: "412"
summary: 'Recently I played around with a library that is used in multiple projects. This library has a huge list of dependencies like: coroutines, rxjava, navigation library from Google, material library, custom edit texts that come from 3rd party libraries and many more. Basically this library has also a rich set of screens available to the users that open given certain conditions. But our issues was bigger.'
tags:
  - aar
  - github
  - gitlab
  - gradle
  - library
  - maven
timeline_notification: "1617516301"
title: Publishing a private Android library
url: /2021/04/04/publishing-a-private-android-library/

---
Recently I played around with a library that is used in multiple projects. This library has a huge list of dependencies like: coroutines, rxjava, navigation library from Google, material library, custom edit texts that come from 3rd party libraries and many more. Basically this library has also a rich set of screens available to the users that open given certain conditions. But our issues was bigger.

## The issues

### Building, versioning and manual update

1. The first issue is that currently how the whole process works, we have to manually build the library into an AAR and then open multiple PRs to the different apps that use it. This would take a lot of time and it makes you unsure what is exactly included in the AAR. (sometimes you may forget to do a git pull before that)  

1. The second issue is that lack of versioning. You may include by mistake an old version of the library that wasn't supposed to be released. And then you are fucked up.  

1. Third issue is the manual update of each project. After you update the library it automatically becomes your responsibility to also update the project in which the library is updated. This sometimes can lead to a wrong behavior being implemented as you don't know the project specific details.

### Dependency resolution

Dependency graph is going crazy. If you don't know - AAR files don't include the dependencies of the library. Which means we have to copy + paste the dependency list of the library into the project that uses it.

## The solutions

### Publishing a library with a private access only

[GitHub](https://proandroiddev.com/publishing-android-libraries-to-the-github-package-registry-part-1-7997be54ea5a) and [GitLab](https://proandroiddev.com/creating-a-private-maven-repository-for-android-libraries-on-gitlab-91137c402777) offer the possibility to publish packages with private access. It is totally free until a certain limit but this is nothing to worry about at the moment. This gives you the possibility to not publish to the official Maven repository, as it requires projects to be open source. This is how we solve the visibility issue.

The process how to upload to maven is quite simple. You need to create a developer token in the settings of your profile and then paste it inside your build.gradle file. You use the maven publish plugin and just tell it to use the token and upload to either GitHub or GitLab. For more details, you can follow the tutorials above.

### Dependency resolution between the library and the project

This is the tricky part to implement. In our case, we want the project to use the dependencies that the library already provides so we don't have to declare these dependencies twice. First, we need to discuss some things:

#### AAR files and dependencies

When we export and AAR file and want to use it, the AAR does not include the dependencies that the library uses. This means the AAR file contains only the library code and no other dependency code. But if you use the library as a project and NOT and aar file, then the dependency list will be included. This is by default the Android behavior which has a [feature request](https://issuetracker.google.com/issues/62121508) in the Google issue tracker so that AAR files include dependencies by default.

This means that all of the implementation / api / runtimeOnly scopes, they are totally useless when it comes to building a library module in the AAR format. Even if you set a dependency to be api, it won't be available if you include the library as an AAR as it won't be present there at all. But if you use it as a project, then these scopes make sense and they work.

### Opening the pandora box

When it comes to deciding how you want your dependency resolution inside the project to behave, you really open the pandora box. There are several ways in which you may want your project to behave which are described amazingly good inside [this article](https://jeroenmols.com/blog/2020/11/11/library-dependencies/). I will just share the three of them:  

1. Forced dependency resolution
1. Loosened dependency resolution
1. Remove transitive dependency

This is a very project specific thing so it is all up to you what you want to achieve. In our case we just trust that the library will have the latest versions of what we need so it is our source of truth.

#### Exporting dependencies using the pom.xml

And the secret of this box is actually the POM.xml file. In the case of the manual build of the aar, it is basically empty. But when you build it using the maven-publish plugin, you can make it put all of the library dependencies inside the POM.xml file and in this way, when you include the library into your project, Gradle will automatically download also all of the dependencies in this file. In this way, your project will also have the dependencies of the library.

**This is the way we decided to go with. It is the easiest possible which leaves us with the dependency resolution inside the project.**

#### Exporting dependencies using a Fat AAR

The fat AAR is the archive where we put all of the dependencies declared inside the app project together with the app inside the AAR file. This way, when you share the AAR file, it will include everything needed with it. There are multiple fat AAR plugins but the most popular is [this one](https://github.com/kezong/fat-aar-android). How you resolve dependency conflicts is similar to how you would do it with the Maven shared package. You will need to either exclude a dependency or force a specific version of it as they bring their own separate consequences to the project.

## Show me the code

The most important part is the one in the library build.gradle file where you want to publish all dependencies in the pom.xml file.

```
publishing {
    publications {
        release(MavenPublication) {
            groupId getGroupId()
            artifactId getArtifactId()
            version getReleaseVersionNumber()
            artifact(sourceJar)
            artifact("$buildDir/outputs/aar/app-release.aar")

            pom.withXml {
                def dependenciesNode = asNode().appendNode('dependencies')

                // Iterate over the implementation dependencies (we don't want the test ones), adding a <dependency> node for each
                configurations.implementation.allDependencies.each {
                    // Ensure dependencies such as fileTree are not included in the pom.
                    if (it.name != 'unspecified') {
                        def dependencyNode = dependenciesNode.appendNode('dependency')
                        dependencyNode.appendNode('groupId', it.group)
                        dependencyNode.appendNode('artifactId', it.name)
                        dependencyNode.appendNode('version', it.version)
                    }
                }
            }
        }
        debug(MavenPublication) {
            groupId getGroupId()
            artifactId "${getArtifactId()}-debug"
            version getDebugVersionNumber()
            artifact("$buildDir/outputs/aar/app-debug.aar")

            pom.withXml {
                def dependenciesNode = asNode().appendNode('dependencies')

                // Iterate over the implementation dependencies (we don't want the test ones), adding a <dependency> node for each
                configurations.implementation.allDependencies.each {
                    // Ensure dependencies such as fileTree are not included in the pom.
                    if (it.name != 'unspecified') {
                        def dependencyNode = dependenciesNode.appendNode('dependency')
                        dependencyNode.appendNode('groupId', it.group)
                        dependencyNode.appendNode('artifactId', it.name)
                        dependencyNode.appendNode('version', it.version)
                    }
                }
            }
        }
    }
    repositories {
        maven {
            name = "GitHubPackages"
            url "https://maven.pkg.github.com/georgi-mirchev/pay-core-library"

            credentials {
                /** Create github.properties in root project folder file with
                 ** gpr.usr=GITHUB_USER_ID & gpr.key=PERSONAL_ACCESS_TOKEN
                 ** Set env variable GPR_USER & GPR_API_KEY if not adding a properties file**/

                username = githubProperties['gpr.usr'] ?: System.getenv("GPR_USER")
                password = githubProperties['gpr.key'] ?: System.getenv("GPR_API_KEY")
            }
        }
    }
}
```

Basically this piece of code will make sure all the dependencies will be bundled inside the pom.xml so when you include the library file, they will also be downloaded to your project.

## References

- [Publishing Android libraries to GitHub Packages](https://proandroiddev.com/publishing-android-libraries-to-the-github-package-registry-part-1-7997be54ea5a)
- [How to publish and distribute an Android Library](https://medium.com/swlh/how-to-publish-and-distribute-your-android-library-ce845c68c7f7)
- [Android library development - Getting started](https://jeroenmols.com/blog/2020/10/28/library-gettingstarted/)
- [Android library development - Modularization](https://jeroenmols.com/blog/2020/11/04/library-modularization/)
- [Android library development - Dependencies](https://jeroenmols.com/blog/2020/11/11/library-dependencies/)
