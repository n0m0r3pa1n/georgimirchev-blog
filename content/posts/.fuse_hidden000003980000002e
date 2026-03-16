---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:28"
_last_editor_used_jetpack: block-editor
_publicize_job_id: "43833528356"
author: gmirchev90
categories:
  - desktop
date: "2020-05-04T17:51:01+00:00"
guid: http://georgimirchev.com/?p=234
parent_post_id: null
post_id: "234"
summary: Following the answer from [HERE](http://stackoverflow.com/questions/4663103/multiple-heroku-accounts) it is becoming a simple task.
tags:
  - git
  - ubuntu
timeline_notification: "1588614662"
title: How to handle multiple heroku accounts with GIT
url: /2020/05/04/how-to-handle-multiple-heroku-accounts-with-git/

---
Following the answer from [HERE](http://stackoverflow.com/questions/4663103/multiple-heroku-accounts) it is becoming a simple task.

1. Generate SSH key by using the command:   
ssh-keygen -t rsa -C "your\_email@example.com"
1. Go to ~/.ssh
1. If you don’t have a config file – create one with name config
1. Add the following to the newly created config

```
Host heroku.work
  HostName heroku.com
  IdentityFile ~/.ssh/id_heroku_work_rsa
  IdentitiesOnly yes

```

id\_heroku\_work\_rsa is the generated ssh key file using the command from as shown [HERE](https://help.github.com/articles/generating-ssh-keys/):

Go to your project .git folder and find the config file there. Open it and change remote heroku to look like this:

```
[remote "heroku"]
   url = git@heroku.work:<you_app_git_path>.git
```
