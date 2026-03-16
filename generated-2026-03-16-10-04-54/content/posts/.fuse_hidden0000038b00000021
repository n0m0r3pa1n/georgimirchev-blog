---
_elasticsearch_data_sharing_indexed_on: "2025-04-12 18:47:58"
_g_feedback_shortcode_atts_b7b281792e34dacc9c0fa3c0962bfd7fab5420ef:
  block_template: null
  block_template_part: null
  className: null
  customThankyou: ""
  customThankyouHeading: Your message has been sent
  customThankyouMessage: Thank you for your submission!
  customThankyouRedirect: ""
  hiddenFields: null
  id: 2485
  jetpackCRM: true
  postToUrl: null
  salesforceData: null
  show_subject: "no"
  subject: '[Make Things Happen] '
  submit_button_text: Submit
  to: gmirchev90@gmail.com
  widget: 0
_g_feedback_shortcode_b7b281792e34dacc9c0fa3c0962bfd7fab5420ef: |-
  [contact-field label="Name" type="name"  required="true" /]
  				[contact-field label="Email" type="email" required="true" /]
  				[contact-field label="Website" type="url" /]
  				[contact-field label="Message" type="textarea" /]
_last_editor_used_jetpack: block-editor
_publicize_job_id: "103231952226"
_publicize_shares: []
author: gmirchev90
categories:
  - android
date: "2025-04-12T18:48:01+00:00"
firehose_sent: "1744483682"
guid: https://georgimirchev.com/?p=2485
parent_post_id: null
post_id: "2485"
reader_suggested_tags: '["Technology","AI","Azure","Linux","Programming"]'
summary: Blackbox tests are tests that are being done end-to-end. This means a UI automaton clicking on your mobile app that connects to the actual backend server - not prod, of course, but something close to staging. On Android specifically, you could use Espresso or UiAutomator to achieve those results.
tags:
  - black-box-tests
  - end-to-end-tests
  - test
  - ui-tests
timeline_notification: "1744483682"
title: Why black-box tests could be a bad idea?
url: /2025/04/12/why-black-box-tests-could-be-a-bad-idea/

---
Blackbox tests are tests that are being done end-to-end. This means a UI automaton clicking on your mobile app that connects to the actual backend server - not prod, of course, but something close to staging. On Android specifically, you could use Espresso or UiAutomator to achieve those results.

## What are the issues with black-box tests?

### Flakiness

Blackbox tests are dependent on the server. Especially given that you don't have a stable server instance and you test against staging, you are guaranteed to have issues and flaky tests. This means that these tests don't give you the confidence that you need, just because sometimes the backend can be broken because of a failed push.

### Stable connection

If you are using sockets, you know that the stability of the connection is something you can never take for granted. The same applies to executing tests against a socket connection. They tend to be flaky as you don't have much control over the connection and when it is initialized. Which results again in those failed tests, that you simply re-run or ignore and that bring 0 benefit to your team.

### Controlling and verifying the state

As we all know, staging DBs can get dropped or data can change. At some point, someone (like the manual QA in the team) may decide to use an account that is already used by the CI. And the mess becomes true. State is changed, screens, text and design do not match the ones expected in tests. And debugging this can take you ages just to get back on track.

### Running once per day

Besides all of the above issues, black-box tests usually take a couple of hours to run, especially if you have lots of them. That means you can run them only once per day, usually nightly. Including all of the above flakiness, they don't give you 100% confidence when it comes to the nightly execution. One failure and you are wondering - did I break something or is it because of a flaky test?

## The Solution

To me, it seems that gray-box testing is the more convenient and secure way to proceed with end-to-end testing. Mocking the server by using a tool like the [OkHttp MockWebServer](https://github.com/square/okhttp/tree/master/mockwebserver) or [Wiremock](https://wiremock.org/) can give you the stability and confidence you need. Yes, it can be a bit more complicated to setup and control the server state but once you get there, you have a better guarantee that you will catch an issue the moment you cause it.
