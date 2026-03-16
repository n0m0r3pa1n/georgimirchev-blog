---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:20"
_publicize_job_id: "43819605042"
author: gmirchev90
categories:
  - code-architecture
date: "2020-05-04T10:52:57+00:00"
guid: http://georgimirchev.com/?p=113
parent_post_id: null
post_id: "113"
summary: |-
  ## The Content

  I recently got to watch several videos about architecture that got me a bit deeper into the topic and I decided to share my thoughts here. There first two videos are from Realm:

  - [MVC vs MVP vs MVVM vs MVI by Marcin Moskala](https://academy.realm.io/posts/mvc-vs-mvp-vs-mvvm-vs-mvi-mobilization-moskala/)
  - [Android Architecture Components Considered Harmful by Vasiliy Zukanov](https://academy.realm.io/posts/android-architecture-components-considered-harmful-mobilization/)

  They are mostly Android oriented but there are some general rules which are applicable to code architecture.
tags:
  - architecture
  - mvc
  - mvp
  - oop
timeline_notification: "1588589578"
title: Thinking about Code Architecture &amp; Object Oriented Programming
url: /2020/05/04/thinking-about-code-architecture-object-oriented-programming/

---
## The Content

I recently got to watch several videos about architecture that got me a bit deeper into the topic and I decided to share my thoughts here. There first two videos are from Realm:

- [MVC vs MVP vs MVVM vs MVI by Marcin Moskala](https://academy.realm.io/posts/mvc-vs-mvp-vs-mvvm-vs-mvi-mobilization-moskala/)
- [Android Architecture Components Considered Harmful by Vasiliy Zukanov](https://academy.realm.io/posts/android-architecture-components-considered-harmful-mobilization/)

They are mostly Android oriented but there are some general rules which are applicable to code architecture.

The other videos are mostly related to this guy – Jim Coplien. I found him recently, I didn’t know much about him. But his lecture was really inspiring and got me to think about Object Oriented Programming again, a moment happening again after I read the Object Thinking book. Here is the lecture

- [‘A Glimpse of Trygve: From Class-oriented Programming to Real OO’ – Jim Coplien \[ACCU 2016\]](https://www.youtube.com/watch?v=lQQ_CahFVzw).

And another great source towards thinking about real Object Oriented programming is:

- [Object Thinking by David West](https://www.goodreads.com/book/show/43940.Object_Thinking)

And one Medium article that I think is very mind opening:

- [Retracing Original Object-Oriented](https://medium.com/skyfishtech/retracing-original-object-oriented-programming-f8b689c4ce50)

## Some Thoughts

After watching and reading some of the content above you may find yourself wondering as I did some time ago. Here are some questions that raised inside my mind:

- Are we doing the right thing in programming?
- Is there a right thing?
- I am doing OO programming or I am following concepts introduced by other guys that just find them useful and working?
- How can I try to do just pure OO programming by following simple principles like SOLID, DRY, KISS & simple design patterns?

Let’s dissect these sources bit by bit and see what we’ll come up with.

#### MVC vs MVP vs MVVM vs MVI

If we set aside all of the MVC history and stuff and all of the detailed explanations what each pattern is and how it is used, this is the most important thing from the whole lecture. No matter what MV\* pattern we use, this is what we are aiming for. Scalable, maintainable & reliable solutions and we have to choose the cost of creation & learning. Every different architecture has a different cost as you can see from the video. It all depends on what you are aiming for.

#### Android Architecture Components Considered Harmful

Although I don’t agree with most of the points Vasiliy made, there is something very interesting in his presentation.  He started with the point “What is architecture?”.

- Is the packaging by feature or by type architecture?
- Or the online vs offline app design?
- Event bus? Design patterns?
- Dependency injection?

And then continued with different definitions of architectures:

- Architecture represents the significant design decisions that shape a system, where significance is measured by cost of **change** – Grady Booch (inventor of UML)
- Architecture is the set of decisions that are hard to **change** – Martin Fowler
- If it does not **change**, there is no block that encapsulates it on architecture diagram – Juval Lowy (CEO of iDesign)

As you can see, all these quotes have the “change” word in common. Vasiliy interprets it quite cool:

> Architecture is our attempt to manage the risk involved in changing requirements. The decisions that we make to handle more easily the expected changing requirements will determine the cost of future changes. In order to understand to have our architecture in our minds, we need to know the business domain that is our target.

The point that he tries to make is that architecture is not the actual structure of the code, but more the business domain model which is expressed from our knowledge about it. It is quite an interesting point as most people think about architecture in terms of classes and coupling between them. But actually, it is not about the actual classes, it is about our business domain model knowledge.

I will not go deep into the other stuff he mentioned, but he really made me think about architecture as handling changes instead of fighting them.

#### From Class-oriented Programming to Real OO &  
Retracing Original Object-Oriented Programming &  
Object Thinking

##### MVC real meaning

By watching the video from Jim Coplien and reading the medium article  you should have understood by now that MVC is not about:

- Fragments & Activities as Views
- Controllers as separate POJO classes
- Models represented with factories hidden behind interfaces and doing network queries

MVC is about people. It is about our thinking and the mental model we have in our heads.

- About the model of the thing we want to achieve, which is in our heads. The way we envision our interaction with the PC and communicate with it.
- The controller, that is the code in the machine that actually does the thing that we have in our minds
- And the display, which shows the view that we expect to see after we take a given action

Do you see it? It does not have a lot connected with the MVC design pattern or whatever you want to call it.

##### Communication & Objects

If you haven’t read the article by now, you should do it. There is a great explanation of objects. Think of the human body. Our body is a living organism. Our body contains:

- Organs
- Tissues
- Cells
- Organels

And how all of them make this machine that does different things? Well, they communicate. And that is the main goal of OOP. We want simple objects, similar to a real world organism, that communicate with each other.

TL;DR for OOP ideas

1. Messaging
1. Local retention, protection, and hiding of state-process
1. Extreme late-binding of all things

##### How to think about objects?

There are several examples that I liked in the Object Thinking book:

- Objects as Lego building blocks – You should easily grab an object and place it somewhere else without the whole structure falling apart.
- Object as person – an object needs to be an agent capable of providing a specified set of services. It has access to a body of knowledge (some of it internalized) that it uses to respond to our service requests as well as any necessary mechanisms (talents and skills) and resources (instruments, a computer, or whatever else it may need). It also tells us that the (only) appropriate way to determine whether an object will suit our needs is by a careful review of its interface (résumé).
- Software as Theater; Programmers as Directors – Software development is analogous to casting and directing a play. The first task is to select your players, the objects that will collectively complete the expected tasks. Provide the cast with a script (cues and dialog). Test them (practice or rehearsals) to make sure you have the right actors and the right cues and dialog, and then, when you’re satisfied, put them onstage and raise the curtain. If you have done your job well, the actors will proceed through the play and the audience will be provided the service of entertainment.

## Summary

The idea of this blog post was to share with you the notion of what object-oriented programming is about. I feel like we, as developers, are so much into following certain patterns that we rarely think of programming as something more than just MVC, MVP or whatever. It is a craft, which delivers the same joy as when you build something physical with your own hands when it is done right. The tricky thing to it is that it requires much more than just writing code. It requires plannings, meetings, a group of people discussing the problem and so much more.

If any of you have seen a real-world OOP example that you enjoy, feel free to share it in the comments section. I plan on doing one myself when I have free time.
