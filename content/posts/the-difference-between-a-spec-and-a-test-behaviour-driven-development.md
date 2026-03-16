---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:26"
_publicize_job_id: "43830921467"
author: gmirchev90
categories:
  - code-architecture
date: "2020-05-04T16:33:05+00:00"
guid: http://georgimirchev.com/?p=218
parent_post_id: null
post_id: "218"
summary: |-
  # What is a spec

  There was a time where I saw some tests which my colleagues made where the file name in the test folder would be: “users.spec.coffee”. So “spec” is short of specification and it is a totally different way of how we think  about tests.
tags:
  - behaviour-driven-development
  - unit-tests
timeline_notification: "1588609985"
title: The difference between a spec and a test (Behaviour Driven Development)
url: /2020/05/04/the-difference-between-a-spec-and-a-test-behaviour-driven-development/

---
# What is a spec

There was a time where I saw some tests which my colleagues made where the file name in the test folder would be: “users.spec.coffee”. So “spec” is short of specification and it is a totally different way of how we think  about tests.

# What I have been doing wrong

I always had the feeling that I was not doing tests right. I do them, I write a lot of tests and test almost everything but the thing that I didn’t feel right was the organization I had. I had files with a single describe in them and then several tests within:

```
describe("customers tests")

it("should create customers", function () {})
it("should not create customer with empty email", function () {})
...

```

The thing that is wrong in this situation is that reading these tests does not feel natural. When you get an error it looks like: “customers tests should create customers line 10”. You have to separate this sentence in your mind and go to the line to understand what is happening.

# More natural reading

Instead when your test fails you should understand what the error is just by looking at the error title: “when customer is new does not create him with missing email”. This reads much natural and you should have a describe with title: “when customer is new” and an it test with title “does not create him with missing email”. And If you follow this style of writing it would be much easier for you to organize and write tests.

```
context "When a gutter game is rolled" do
  setup do
    @g = Game.new
    20.times {@g.roll 0}
  end

  specify "score should be zero" do
    @g.score.should == 0
  end

  specify "game should be complete" do
    @g.complete?.should_be true
  end
end

context "When all ones are rolled" do
  setup do
    @g = Game.new
    20.times{@g.roll 1}
  end

  specify "score should be 20" do
    @g.score.should == 20
  end

  specify "game should be complete" do
    @g.complete?.should_be true
  end
end
```

# Where is it going?

Well, as you may see it makes you write your tests in a whole different way. You think and organize your tests in different types of contexts and ways the users interact with your program. It is more like of making tests based on a specification you received from the business people. And I really enjoy the idea!

# Connection between specs and BDD (Behaviour driven development)

Basically specs fit excellent in the behaviour driven development idea. To understand the BDD here are some good video explanations in YouTube:

- [Introduction](https://www.youtube.com/watch?v=njcHzGYv7nI)
- [Wikipedia of course!](https://en.wikipedia.org/wiki/Behavior-driven_development)

# More info on specs

I have found some sites on the topic which I think will be very useful for each programmer:

[Betterspecs](http://betterspecs.org/)  
[Specs vs Tests](https://sites.google.com/site/unclebobconsultingllc/specs-vs-tests)  
[Introducing BDD](https://dannorth.net/introducing-bdd/)
