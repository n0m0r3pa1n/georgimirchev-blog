---
date: '2026-09-07T21:42:53+03:00'
draft: false
title: 'Software Expectations and Reality in the AI-Driven World'
---

Almost two years doing AI programming in Android, feels like time is flying and with a baby in my family I just don't have much time to share my experience. But here we got after a not-so-short break of posting on the blog.

# What companies seem to expect nowadays?

In today's AI world, most software companies expect that you will deliver more and more simply by using AI. Expectations are that managers will code, designers will use AI to design and programmers will quadruple the features they deliver because "Hey, can't you simply use AI to do it?". A sentence that you can easily hear multiple times in a week.

Not only that but as AI gives the false promise of achieving quality stuff fast, people expect you to deliver **quality features fast**. 

![Fast & Cheap, Huh?](https://media1.tenor.com/m/pMicD5OgPfMAAAAC/pick-two.gif)

Not to disappoint all CEO's and managers reading this post, but this is not how things work. And I will describe in a bit why.

Unfortunately, a lot of people got affected by the above thinking where companies decided to go all in and cut jobs thinking that AI can replace people. 

And some companies expect you to cut your AI costs while you deliver high quality features fast so they can be cheap. If you use Opus 5 too much - use Sonnet for execution and Opus for planning. Otherwise, you have a budget and once you reach it, it is all gone.

# What happens now in the AI world?

Seems that expectations and real world experience are two totally different worlds. 

## AI is really good but it really sucks in more complicated problems

If your codebase is a bit more complicated or you face a really niche issue, relying purely on AI can become a real mess. At one points it tells you "Hey, I see the problem let's use approach A, it will solve the issue". You open a new session and boom "Approach A is a proven degradation in the device performance, we shouldn't be using it" - right after you have deployed a new app version with it.

## AI-generated architecture sucks often and a lot

You have skills telling it what to do and how to do it and this sucker still creates a random mess of code that doesn't comply to your existing app architecture. You can introduce some tools to validate it but why do you have to micromanage it when it is all written? All features are written using clean architecture and it decides to add a brand new class that is neither a domain, nor a data implementation and puts it in a random module. The bottleneck has shifted from writing code to supervising generated code.

## Bloated PR descriptions and code comments

Tons of these make PR reviews a nightmare for the author itself. You go and check the PR description and you end up with a novel describing a simple feature being added. Not only that but the code ends up bloated with comments mentioning JIRA tickets or domain related logic in the data layer and so much more. Awful experience in terms of this bloating that eats up context.

## AI optimizes local execution, but it does not automatically optimize coordination across a software organization

Now you have AI - be fast. Everyone goes away and implements something on their own. Let's say you need to impelent a picture brightness validation. The backend defines one validation contract, iOS solves the problem through EXIF metadata, and Android implements a completely different luminance-based approach. Everyone shipped quickly, yet the product ended up with three interpretations of the same feature.!? We deliver fast random code that is supposed to work and tons of tokens flying around 3 project just to get them on board with the same approach.

## FE becomes BE, BE becomes FE

What a mess? Now you are asked to do BE code that you don't have an idea how it works but "Hey, can't you use AI to do it?". Backend try to do FE feature which are a mess and you spend more time reviewing this mess which would be less if you do it yourslef. You write some GO code (or Claude writes it for you) and you don't know the details below, you don't know how it works but you need to test it and bother several BE engineers to get it merged. And instead of having more capable people on the team, we now have more bloated agents. AI has dramatically reduced the cost of writing code in an unfamiliar stack. It has not reduced the cost of understanding the consequences of that code. 

## Ain't no model for the right task

Use Opus for planning, use Sonnet for execution, use Haiku for raising PRs - ah, no, use Luna for general stuff, use Terra for reviews, use Gemini for reading documents. What the heck? I have to switch the model 10 times in a session or use 10 subagents to achieve a simple thing. This mess leads to high costs that no company wants but at the same time, they want to have the high quality fast delivery. With Haiku, if possible.

## The cognitive load did not disappear

Reviewing and managing so much context in parallel can simply make you mad. Chasing AI to do the proper thing, revisit it, review code, test it and verify it works - all of this still requires work in your head. And it is just not physically possible to do all of this on a daily basis. AI can get you both quite happy and quite mad, constantly falsifying stuff that are not the solution that should be in place.

# TLDR;

AI is an incredibly powerful engineering tool, but companies are making a mistake when they treat faster code generation as equivalent to faster software delivery. Architecture, product alignment, cross-team coordination, testing, domain knowledge and review still exist — and in some cases AI creates more of them, not less.

The companies that benefit most from AI will probably not be the ones that simply demand four times more features. They will be the ones that learn where AI genuinely removes work, where it merely moves the work somewhere else, and where experienced engineers are still the bottleneck for a very good reason.

