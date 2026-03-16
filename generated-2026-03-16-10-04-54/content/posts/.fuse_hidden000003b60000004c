---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:19"
_publicize_job_id: "43819098141"
author: gmirchev90
categories:
  - desktop
date: "2020-05-04T10:35:25+00:00"
guid: http://georgimirchev.com/?p=103
parent_post_id: null
post_id: "103"
summary: 'Recently, I installed MongoDB following the [tutorial](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/#install-mongodb-community-edition) on their site on Ubuntu 16.04 64 bit. But when trying to start the mongod service I got the following error: “Failed to start mongod.service: Unit mongod.service not found.”'
tags:
  - mongodb
  - ubuntu
timeline_notification: "1588588525"
title: 'Failed to start mongod.service: Unit mongod.service not found. – Ubuntu 16.04'
url: /2020/05/04/failed-to-start-mongod-service-unit-mongod-service-not-found-ubuntu-16-04/

---
Recently, I installed MongoDB following the [tutorial](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/#install-mongodb-community-edition) on their site on Ubuntu 16.04 64 bit. But when trying to start the mongod service I got the following error: “Failed to start mongod.service: Unit mongod.service not found.”

To resolve it I followed the [solution](https://askubuntu.com/questions/770054/mongodb-3-2-doesnt-start-on-lubuntu-16-04-lts-as-a-service) suggested on StackOverflow.

### Create the configuration file for MongoDB

sudo nano /etc/systemd/system/mongodb.service

### Add the following content to the file

```
[Unit]
Description=High-performance, schema-free document-oriented database
After=network.target

[Service]
User=mongodb
ExecStart=/usr/bin/mongod --quiet --config /etc/mongod.conf

[Install]
WantedBy=multi-user.target

```

### Run the mongod service

sudo systemctl start mongodb  
sudo systemctl status mongodb

### Make the service permanent

sudo systemctl enable mongodb
