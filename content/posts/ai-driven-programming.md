---
author: gmirchev90
date: "2026-11-05T00:00:00+03:00"
title: "AI driven programming"
draft: false
tags:
  - AI
  - Android
  - Programming
  - Architecture
  - Approaches
categories:
  - android
  - code-architecture
  - thoughts-on-software
---

Only 2 months after I wrote my last post about AI, I am writing yet another one. I will try to not blame companies, complain about the poor quality, mention the huge stress in my daily work, point at the uncontrollable portions of bloated code that goes out and treat colleagues bad just because they press Approve without even looking at the PRs. We are all under the pressure to deliver more and more - both in terms of code and in our daily lives.

# What doesn't work in AI driven programming now?

There are few aspects that I notice in the last 2 years that do not work yet and haven't figured out their own paths in the AI driven IT world.

## Building features

### Gather context

Let AI analyse:

- JIRA ticket requirements
- Figma designs
- Existing BE or FE PRs
- Slack for any conversations about the feature and expectation laid out
- Google Drive RFCs for any plans

Then ask it to lay out an MD file that can be used as reference when you build the feature. You can use that file yourself too. It gives you an idea;

- What is the current status
- What are the expectations
- Who should you talk to
- What was already done

In the current fast pace, I think the above information is super important to get your job done.

### Separate agents for FE and BE

I am experimenting with this one. There is a very thin line where it stops working.

#### Single agent

I have figured out single agents work amazingly when it comes to feature of average size. They can modify both iOS / Android and BE without any issues. The problem comes when the feature becomes a long-lived one where you have to ask it on totally random topics from how a feature flag impacts the BE to why certain UI element is rendered badly. Then you can easily fill your context and the agent starts to suck.

#### Multiple agents

On the other side, if I split 3 agents:

- iOS
- Android
- BE

I figured out that I can ask them to figure out stuff like why the emulator doesn't work, why Android is different from iOS and basically let them communicate between themselves. I don't use Claude Teams but I am their manager so I kind of micro-manage them. Not only that but this gives me the freedom to make sure that both iOS and Android follow the same patterns and domain architecture so we don't up with 2 totally different understandings of the same thing.

Another benefit is that BE and FE can talk and figure out issues and save context in sessions. 

Unfortunately, the issue is that sometimes there is a very thin line where BE tried to do FE stuff or the other way around. Then everything messes up and context flies away for a goodbye. I am still indentifying that line and putting the limits around it so it can't be reached so easily. But I guess I need more features with it.

## Reviews PRs

Things haven't changed a lot. What changed is the PR size. Having 1200 lines PRs is the daily normal in the software industries now. Previously, anything above 500 lines was auto rejected. Now it seems if you don't review it, you are the bad guy that stops progress from moving. And now a lot of us understand how writing actually put stuff in our head while reading lets stuff disappear in hours. So, how do I tackle this one:

### Do notes

Start reviewing the PR. Take notes:

- How that things works?
- What classes are used?
- Does it follow the correct style and architecture?
- Is the module structure correct?

This will help you create a mental model of the PR.

### Talk it out loud

If you want to understand it faster, instead of writing, use the old classic of rubbder duck debugging. Talk it out load. Explain it to yourself. This, for me, doesn't work each time. I easily get distracted so I have to be in a really isolated environment to be able to talk it out.

### Use Mermaid diagrams

Diagrams are a tool I notice that a lot of devs rarely use. They help me soo much! I create two diagrams:

- Before the PR
- After the PR

This gives me a great idea what the PR modified, how the feature currently works and if any problems can arise

### Understand the bigger picture

Let AI play the teaching role. No matter if this is an Android project or a brand new BE project that you need to join and barely know how the code works. Start from the idea itself. Let AI:

- Read the JIRA tickets
- Prepare the diagrams
- Review the code architecture

And come back to you and teach you in steps how the PR works. You can give it the freedom of the steps but you don't need to learn everything at once. Let it start from the story telling and proceed with the actual architecture and with the thinking behind the more complicated pieces of code.

### Use an appropriate app to preview everything

The app is super important for me. I want something that previews diagrams and shows everything together with the prompt that I did. I use Typora for that purpose. It previews diagrams amazingly, I ask AI to output everything in the doc and open it in Typora. So I can reviews steps at once.

## Raising PRs

Reviewing is one part. Raising PRs is another part. I follow some practices that I find useful

### No comments

Ask the AI to not add any comments unless explicitly necessary. AI can be small functions with proper names. Follow that approach instead of adding those ugly huge KDocs

### Add diagrams

Make sure to add diagrams in PRs. Before and after again. This helps reviewers in their understanding of what changed and what not.

### Short descriptions

Make sure to explicitly set that AI should not generate long descriptions. They should be straight to the point, sharing the:

- Challenges faces in the PR implementation
- Why certain decisions were taken
- Testing done

### Self reviews

I used to do self reviews myself. I go through the PR and leave small short comments why a certain decision was taken (if it sounds like it shouldn't be or any decisions led to it).

Now I ask AI to do it. Especially if PRs are bigger I always found it useful to also confirm my understanding on the subject.

## Testing

### Add notes for QAs

When the final phase of a feature / bugfix is done, ask AI to generate notes for the manual QA testing. Those notes can be attached to the PR or to the JIRA ticket as comment. You can potentially automate that process

### Automated tests

Make sure that in the process of creating a feature your agent is following the appropriate testing strategies in place. Not only unit tests but UI tests too. On personal apps, I find it extremely useful to have UI tests added while the feature is being developed. This way in future iterations I can prove that we have not broken anything.

### Mock servers

If you start or modify your app to support mock servers, this will make debugging BE issues so much easier. You can ask AI to mock easily responses that you can observe causing issues on Sentry or any other platform for monitoring crashes. Plan this as one of the biggest benefits that AI can bring you. 

### Real devices

AI agents work totally fine with real devices. They can set up the appropriate proxies in place to communicate with the device, route it through the local HTTP mock server and mock responses.

## Debugging

### Logcat

Our best friend. Ask your agent to build and install the app and observe logcat when you notice a certain issue happening. You can ask it to add additional logging so it can easily flow through the app lifecycle and screens and identify potential issues.

### Networking

There are 2 ways to approach networking.

1. If you are using OkHttp - the use the [HttpLoggingInterceptor](https://github.com/lysine-dev/okhttp/tree/main/okhttp-logging-interceptor) and let AI observe logcat
2. Use the Network Profiler in Android Studio

In the second approach, you will have to manually provide the requests to your agents as I have not figured out a proper MCP server to use that can connects to the tool. What I do is:

- Copy the request as cURL - right click on it and the option shows
- Copy the response body

Provide both and the agent does its magic. In comparison to iOS, Android works like magic in the networking layer.

## Profiling

### Trace MCPs

Profiling is such a magic nowadays. I have done it manually for years and when I figured out that there is an [MCP server](https://github.com/zabirauf/mcp-trace) for the trace files, it made me so happy. 

Install and use that MCP server as per the guide. Makes your life tons easier to understand what is going on. You still have to do the profiling yourself but on reading and understanding what is going on AI assists so much.

### Android CLI tools & skills

Use the Android CLI tools to do stuff like recomposition count. Use the skills to analyse the data and save you some manual recordings. Let AI generate the release variant of your app and analyse the APK size, proguard rules and stability reports by the Compose Compiler.
