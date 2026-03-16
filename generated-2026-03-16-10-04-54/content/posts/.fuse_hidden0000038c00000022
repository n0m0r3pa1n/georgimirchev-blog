---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:22"
_publicize_job_id: "43823141130"
author: gmirchev90
categories:
  - desktop
date: "2020-05-04T12:48:56+00:00"
guid: http://georgimirchev.com/?p=149
parent_post_id: null
post_id: "149"
summary: I wanted to stop the mongod service running on system boot in Ubuntu. This service is starting the mongodb database.
tags:
  - mongodb
  - ubuntu
timeline_notification: "1588596539"
title: How to stop services from running in Ubuntu or how to stop mongo from autostart?
url: /2020/05/04/how-to-stop-services-from-running-in-ubuntu-or-how-to-stop-mongo-from-autostart/

---
I wanted to stop the mongod service running on system boot in Ubuntu. This service is starting the mongodb database.

To do that I used the following two methods:  
1. sudo update-rc.d -f mongodb remove  
2\. Open /etc/init/ folder and comment the following lines in the mongo.conf file:

start on runlevel \[2345\]  
stop on runlevel \[06\]

This will stop the service from running on start up.
