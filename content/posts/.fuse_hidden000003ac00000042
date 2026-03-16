---
_elasticsearch_data_sharing_indexed_on: "2025-04-27 06:54:21"
_g_feedback_shortcode_atts_cc362c8e26e2ba1669a48e5804a7db75cc7459da:
  block_template: null
  block_template_part: null
  className: null
  customThankyou: ""
  customThankyouHeading: Your message has been sent
  customThankyouMessage: Thank you for your submission!
  customThankyouRedirect: ""
  hiddenFields: null
  id: 2495
  jetpackCRM: true
  postToUrl: null
  salesforceData: null
  show_subject: "no"
  subject: '[Make Things Happen] Observations of migrating an app from XML to Jetpack Compose'
  submit_button_text: Submit
  to: gmirchev90@gmail.com
  widget: 0
_g_feedback_shortcode_cc362c8e26e2ba1669a48e5804a7db75cc7459da: |-
  [contact-field label="Name" type="name"  required="true" /]
  				[contact-field label="Email" type="email" required="true" /]
  				[contact-field label="Website" type="url" /]
  				[contact-field label="Message" type="textarea" /]
_last_editor_used_jetpack: block-editor
_publicize_job_id: "103615246372"
_publicize_shares: []
author: gmirchev90
categories:
  - android
date: "2025-04-27T06:55:05+00:00"
firehose_sent: "1745736905"
guid: https://georgimirchev.com/?p=2495
parent_post_id: null
post_id: "2495"
reader_suggested_tags: '["Digital Marketing","Technology","SEO","Programming","Web Development"]'
summary: Jetpack Compose is the coolest kid on the block, but it has various different drawbacks that I at least notice when we did the migration of the app I was working on. And I wanted to share them in my blog with everyone, just in case you need to do the same thing in your app.
tags:
  - jetpack-compose
  - jetpack-compose-navigation
  - r8
  - xml
timeline_notification: "1745736906"
title: Observations of migrating an app from XML to Jetpack Compose
url: /2025/04/27/observations-of-migrating-an-app-from-xml-to-jetpack-compose/

---
Jetpack Compose is the coolest kid on the block, but it has various different drawbacks that I at least notice when we did the migration of the app I was working on. And I wanted to share them in my blog with everyone, just in case you need to do the same thing in your app.

{{< figure src="https://media.licdn.com/dms/image/v2/D4D12AQH0n5SueE9qWA/article-cover%5Fimage-shrink%5F720%5F1280/article-cover%5Fimage-shrink%5F720%5F1280/0/1711289424141?e=1751500800&v=beta&t=mVlDU8%5FDzTcaAJKtaJYIUKP87S6Bw4e3EhuYMO4agF4" alt="" caption="" >}}

## Observation of integrating Jetpack Compose

In one of the projects I was working on, we were eager to migrate to Jetpack Compose. We had everything in XML and every new dev that joined the team was eager to implement the new technology and start using it. Not only that, but big bets were introduced as we had to do a redesign of the app. This redesign was supposed to bring a design system and reusable UI components to all screens.

### Add minifyEnabled and R8 optimizations

So a lot of us started doing this migration from XML to compose. And the first thing that popped up with the first screen implemented in Compose is the speed of it. It was amazingly slow on these older devices with Android 7 that we had to support. So slow that we had to implement R8 optimization, which we haven't done until now.

Yes, I know R8 is a must, but when the devices that you use are locked and work in an environment where Google Play is not freely accessible, you have the freedom to not minify and optimize your app until it is really necessary. I agree - in today's world, it is totally a must to have both options implemented but due to our heavy workload, we decided to delay that until later when we have free time.

Well, Jetpack Compose pretty much enforced that into our team. We spent almost a week getting things straight. I figured out there is a bug with R8 that does not obfuscate code correctly, and Google fixed it, so it took like 2 weeks of effort to just get the R8 bit working.

But how about the speed? Well, I could notice a significant difference in the speed of the minified app and the non-minified one. Actually, it was a huge difference. The app would behave without any glitches or long UI pauses when you play around with it. Without the minification, it was a real pain in the ass to use it.

### Adding Jetpack Navigation

Well, fragments & XML go together with the fragment-based navigation from Google. Now that we are migrating to Compose functions, the navigation can also be changed to use the Jetpack Compose Navigation that navigates between those functions so you don't have to deal with them anymore. But what is the problem?

#### BaseFragment vs Scaffold

The problem is as old as the universe - composition vs inheritance. You usually rely on a BaseFragment where you delegate some important "must" properties and each screen should declare what its values are. For example, isSecureScreen or supportExternalKeyboard etc.

A lot of devs don't make the connection between composition and Compose. Compose is not a new thing. When the [Flux](https://facebookarchive.github.io/flux/docs/in-depth-overview/) component-based architecture was introduced, it came from the web. And React introduced the concept of UI components. I did my Android [Flux](https://github.com/n0m0r3pa1n/android_flux_example) example 10 years ago when the guys were laughing at me at an interview for an Android job.

With that in mind, you need to start to use Scaffolds to implement what was previously available in BaseFragment. Yes, you could forget to use the Scaffold but you could also forget to extend from BaseFragment. That's what PR reviews are for. In order to achieve the same behavior, you need to share this scaffold between screens to make sure you declare each property

#### Relying on fragment IDs

If you rely on Fragment IDs, you should be aware that this will now be broken. Composable functions do not have unique IDs, although they have unique routes. So you should consider refactoring your logic to rely on something else rather than the routes of the screen.

#### Speed in Composable navigation

I won't lie that I am not sure why, but again, the speed of navigating between screens is totally not the same as before when we had XML & Fragment-based navigation. It seems slower and if it weren't R8, it would be tragically bad.

### Fixing UI tests

If you had previously UI tests, it could be a pain in the ass to get them working with compose. It should be as simple as adding a ComposeTestRule but that could also not be the case.

You also need to replace usage of IDs with compose test tags or content descriptions etc.

In theory, the migration of the UI tests should be fairly simple but again, it all depends on how tied up is your UI tests architecture with the framework that you are using.

### When Jetpack Compose seems to not be the best fit?

When the speed is a major concern and you need to support older devices. Older devices just behave slower and although R8 brings major optimizations to the scene, it still doesn't have a magic wand that will make everything fly as with XML. (Sorry guys, love Compose but this is all a Google's thing). If you need really fast navigation and although the RecyclerView ViewHolder pattern may seem as shit to implement, it still behaves a lot faster and better than how lists are handled in LazyColumn / Row. And yes, you can use it in debug mode and still feel that you have done a nice job without the UI glitching half of the time.

### When Jetpack Compose seems to be a good fit?

- When you have to support newer devices
- When speed is not a major concern
- When you want to have a shared design system
- When you want to share UI components between apps
- When you want to be on the edge of technology no matter the stability

### Summary

With that being said, time will show whether choosing Compose was the best solution in the app that we were rebranding. As speed is an important aspect of the project I was working on, it will show whether the decision made was correct or not.
