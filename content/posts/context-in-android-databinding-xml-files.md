---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:26"
_g_feedback_shortcode_16364797221f7e4663832fabfae2bb3d9b3849a7: |-
  [contact-field label="Name" type="name"  required="true" /]
  				[contact-field label="Email" type="email" required="true" /]
  				[contact-field label="Website" type="url" /]
  				[contact-field label="Message" type="textarea" /]
_g_feedback_shortcode_atts_16364797221f7e4663832fabfae2bb3d9b3849a7:
  block_template: null
  block_template_part: null
  className: null
  customThankyou: ""
  customThankyouHeading: Your message has been sent
  customThankyouMessage: Thank you for your submission!
  customThankyouRedirect: ""
  hiddenFields: null
  id: 209
  jetpackCRM: true
  postToUrl: null
  salesforceData: null
  show_subject: "no"
  subject: '[Make Things Happen] Context in Android Databinding XML files'
  submit_button_text: Submit
  to: gmirchev90@gmail.com
  widget: 0
_last_editor_used_jetpack: block-editor
_publicize_job_id: "43830750631"
author: gmirchev90
categories:
  - android
date: "2020-05-04T16:28:11+00:00"
guid: http://georgimirchev.com/?p=209
parent_post_id: null
post_id: "209"
tags:
  - context
  - databinding
timeline_notification: "1588609692"
title: Context in Android Databinding XML files
url: /2020/05/04/context-in-android-databinding-xml-files/

---
It is so easy guys, just don’t forget that DataBinding creates a field called context which you can access by default in your XML file. So you can write something like this:

```
<data>
   <variable name="name" type="String"/>
</data>
<layout>
  <TextView
      app:text="@{StringUtils.format(context, name)}" />
</layout>

```

And it will be just fine. You have the context from the parent view. Check it [HERE](https://developer.android.com/topic/libraries/data-binding/index.html#variables).
