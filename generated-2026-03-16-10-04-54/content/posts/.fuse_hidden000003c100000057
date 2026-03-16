---
_elasticsearch_data_sharing_indexed_on: "2025-11-23 16:53:42"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "109163288748"
_publicize_shares: []
author: gmirchev90
categories:
  - android
  - thoughts-on-software
date: "2025-11-23T17:01:27+00:00"
firehose_sent: "1763917287"
guid: https://georgimirchev.com/?p=2592
parent_post_id: null
post_id: "2592"
previously_liked_199120357: "1"
reader_suggested_tags: '["AI","Technology","Artificial Intelligence","Chatgpt","LLM"]'
summary: AI is here, and the best we can do now is take a look at it and not ignore it. I have been using Cursor with Claude for the past six months, both on personal and work projects of various sizes, and I can tell for sure that it handles some tasks with ease, while others are a huge struggle until you intervene and give the right direction. But even with that in mind, it still manages to save tons of hours of writing templates and sometimes makes me wonder whether my brain is working less effectively just because I can ask it to figure out a given problem for me. But here are the major problems that I experienced with it until now.
tags:
  - ai
  - cursor
  - markdown
timeline_notification: "1763917288"
title: 'Cursor on Android: Where it struggles and how to make it useful'
url: /2025/11/23/cursor-on-android-where-it-struggles-and-how-to-make-it-useful/

---
AI is here, and the best we can do now is take a look at it and not ignore it. I have been using Cursor with Claude for the past six months, both on personal and work projects of various sizes, and I can tell for sure that it handles some tasks with ease, while others are a huge struggle until you intervene and give the right direction. But even with that in mind, it still manages to save tons of hours of writing templates and sometimes makes me wonder whether my brain is working less effectively just because I can ask it to figure out a given problem for me. But here are the major problems that I experienced with it until now.

# No context = Poor results

At the beginning, you have to be really careful with AI tools as they don't have the context from previous work they did on your project so the chance they will mess up something is huge. The same happened to me, where Cursor with Claude just started writing code without:

- Following the naming conventions
- Using assertThat and not the Kotest's shouldBe comparisons
- Adding tons of unnecessary comments everywhere
- Not following the existing project architecture and creating random "er" suffixed classes everywhere

and many more. Just make sure that you:

- Describe the project architecture to it
- Make it check the currently used naming conventions
- Ask it to read and find something already existing before it starts to code - so learn before you write.

# Not following code architecture

At the beginning especially and given the 200k context window which is not small, but surely not enough in some cases, it is very easy for AI to create code that doesn't follow the existing architecture. Make sure you have it written down somewhere that it should follow clean architecture and study the usage from existing "golden" features if you have such. This will give it an idea what your project specifics are.

One example of the above issue is if you ask it to do analytics implementation for a certain feature. It will check the feature module, it will see that you have common-analytics and then it will put the implementation of the analytics for your feature in the common module and not in the already existing feature one. You have to be careful for such cases. Make sure to review the work that AI has done carefully. It won’t take you much time, but you can easily notice if it missed something or if it’s using the architecture the wrong way. You may also spot missing cases that it skipped.

# Don't trust it for unit tests

## Deleting unit tests

You know that Android is hard. Inject a custom coroutine scope somewhere and let all hell break loose. Especially when you have to unit test it. For AI threading is hard too. A lot of the times using an UnconfinedDispatcher solves the issue for you, but AI instead goes into the rabbit whole searching for non-existing issues and at the end - deleting unit tests.

There are times when you ask it to not delete them but it just cannot figure out the issue by itself. That's why I rely mostly on my instinct about unit tests and make sure I always go through them especially when they are written by AI.

## Unit tests that don't actually test anything

I don't know why but AI simply loves the stupid useless verify tests. I have asked it countless times to assert and not verify as verification is almost a meaningless thing in the unit tests world. We should be checking real values, not verify that a function was called. But AI doesn't give a crap about that and all it creates are verification checks. And even the smallest issue it experiences with asserting a unit tests results in converting the whole test to a verification one.

# Handling OS-specific issues

A lot of the time, knowledge that you have gained through the years is not available to AI (so it doesn't search on StackOverflow for a given issue :D ). So a glitch in Android where it renders an icon orange instead of black and letting AI fix it can lead into an endless loop where the project gets deleted and your OS gets purged. :D

# Lying

A change can be tough to implement. Especially if it involves a core component that is used in tons of modules. Sometimes, AI just fails to figure out what is going on and it just goes into an endless loop where it changes tons of stuff, leaves a lot of unused code and switches the architecture 2-3 times before ending up with this useless mash of spaghetti code spread everywhere. But if you run it, it may still compile.

This has happened for me only for very complicated cases where you need to provide guidance at lots of the steps during the AI investigation. What really helps is trying to have a discussion with it. Ask it to talk to you if it finds itself stuck in the investigation process.

# Suggestions that worked for me

## Always export plans & notes - Markdown is your best friend

Based on everything from the first point, always ask AI to write these notes and even group them by topic in a folder that is being excluded from the .git tracking. This way it will be a lot easier for you whenever you start a new feature to just ask it to visit an existing document and check what the implementation was there. This way, you save yourself even more time writing unnecessary stuff that you have already shared.

Not only that, but whenever Cursor finishes an implementation, make it describe it in a Markdown document. This gives you a record of what it did and helps with debugging or handling similar feature requests later.

## Always review and approve and never let it commit

Before you tell Cursor to implement something, you have to give it enough details so it knows exactly what to do. And because most of us are very lazy, we sometimes write a sentence or two and expect it to figure things out by itself when the details are simply in our heads. And, as expected, the results can be very poor. That’s why you should:

- Always make it prepare a plan.
- Always mention that the plan should be simple and understandable; it should include only the affected classes—not unit tests—as they just bring more noise.
- Write it down in Markdown.
- Have it produce a simple diagram.
- Let it know that you will be reviewing it, and it cannot commit anything without your approval.

Make sure to review the work that AI has done carefully. It won’t take you much time, but you can easily notice if it missed something or if it’s using the architecture the wrong way. You may also spot missing cases that it skipped.

## Commit everything that started working

After each task that you give AI, make sure that you commit the code that was generated. You have 0 guarantee that the next task won't make AI go mad, delete everything and write tons of bullshit. This is the golden rule: "commit if it works when generated by AI".

## Let it read & write explanations of how features work

It is brilliant for that type of work. It can easily follow modules, create diagrams and explain details that will take hours for you to actually understand. It can compare differences between implementations or investigate stuff like why your CI is slow by sharing a YML with it. Everything that involves exploring the codebase and preparing diagrams is a great task for an AI tool.

And of course, always make sure to note them down in an MD document that can be useful in the future.

## Multi-window contexts

It is super useful if you have checked out your BE project and you have it next to your FE project in the same folder. Then you can easily tell Cursor to switch to the BE project, analyze the code in a given PR, come back to the FE project, prepare a plan for the implementation that corresponds to what BE has done and then apply it. The same applies for iOS and Android repos. If someone has already done the work on iOS, you could try to have the same on Android.

The above works only if the feature doesn't have any architecture specifics. If it relates to analytics, for example, you can ask Cursor to extract what iOS has done and apply the same thing in Android in the corresponding places (only after your approval).

## Always make it compile the project, update the tests and run ktlint

Sometimes, there are really simple mistakes done by AI like:

- Forgetting about imports
- Using classes that are in another module that is not available in the current one
- Using libraries that are not being used at all

and many more. Make sure that you tell AI to try to build the project and run the tests whenever it finishes an implementation. This way it identifies code that is simply not working and prepares the necessary fixes for it.

# Overview - AI assistant and not autopilot

Treat AI as an assistant and don't leave it on autopilot unless you want to end up waiting a day for an implementation that is useless and has deleted tons of code. Try to reuse as much as possible by having templates and rules that fit your project needs. Always review plans before the implementation is started. And make sure to commit working code in the process. And who knows, at some point we will all be mostly managing AI instead of writing the code ourselves.
