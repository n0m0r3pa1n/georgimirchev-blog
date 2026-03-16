---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:25"
_publicize_job_id: "43830559200"
author: gmirchev90
categories:
  - web
date: "2020-05-04T16:22:48+00:00"
guid: http://georgimirchev.com/?p=200
parent_post_id: null
post_id: "200"
summary: |-
  I had to implement a feature toggle recently.

  ## What is a feature toggle?

  I will keep it short. You have to add a new feature to your product, but you want that feature to be available only for certain type of users. What you do? Well, you just make an if check to see the user can see this feature and then you toggle it for him. Here is a sample check:

  ```
  if(ToggleRouter.isEnabled("comments")) {
    // Display the feature to the user
  }

  ```
tags:
  - feature-toggle
  - nodejs
timeline_notification: "1588609369"
title: Feature toggles in NodeJS
url: /2020/05/04/feature-toggles-in-nodejs/

---
I had to implement a feature toggle recently.

## What is a feature toggle?

I will keep it short. You have to add a new feature to your product, but you want that feature to be available only for certain type of users. What you do? Well, you just make an if check to see the user can see this feature and then you toggle it for him. Here is a sample check:

```
if(ToggleRouter.isEnabled("comments")) {
  // Display the feature to the user
}

```

## How to configure it?

You should be able to easily configure the feature toggle. You should do it from:

- a config file
- from the environment variables (node process.env.COMMENTS\_ENABLED=true app.js)
- in the DB
- teach it from context or cookies

## Categorization of toggles

- How long will the feature live?
- How dynamic the toggling can be?

### Release

Allow incomplete and untested code paths to be shipped to prod. Not wanting to expose functions. Two-three week of living.

### Experiment

Multivariate or A/B testing. Send user to different paths. Long enough to generate state.

### Ops

Control operational aspects of the program. Used when we have a new feature with nuclear performance. Short lived.

### Permissioning

Release a feature to a certain set of users.

## Implementation techniques

1. De-coupling decision points from decision logic
1. Inversion of Decision – inject a config object
1. Strategy pattern
1. Avoid conditionals for long-lived toggles

## Libraries for NodeJS

1. Feature-Toggles – [https://www.npmjs.com/package/feature-toggles](https://www.npmjs.com/package/feature-toggles)
1. fflip – [https://github.com/FredKSchott/fflip](https://github.com/FredKSchott/fflip)
1. Flipper – [https://github.com/nomiddlename/flipper](https://github.com/nomiddlename/flipper)

I am using the first library in the project and I find it useful enough. Still, our case is a simple one and we don’t need any complicated logic to handle it.

## Before adding a feature toggling library

Think if you can make a workaround. For example, we needed to allow our users to login with 2 separate login providers. So what we did: we made 2 different endpoints for the 2 providers. We know that we won’t have more than 2, so that is why we made it this way. No need for a feature toggle to decide which provider to use.

## More info

If you want to know more just check the article at [Martin Folwer .com](http://web.archive.org/web/20190723003520/http://martinfowler.com/articles/feature-toggles.html).
