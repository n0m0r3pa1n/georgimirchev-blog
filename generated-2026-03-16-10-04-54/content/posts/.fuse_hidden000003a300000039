---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:24"
_publicize_job_id: "43825003739"
author: gmirchev90
categories:
  - web
date: "2020-05-04T13:45:20+00:00"
guid: http://georgimirchev.com/?p=190
parent_post_id: null
post_id: "190"
summary: 'You have probably heard this form me. I am working on a [Sails](http://sailsjs.org/) js project and I wanted to implement a simple role management. That means that I have several routes like: “/admin”, “/login”, “/users” and I want the admin route  to be accessed only from admins, login to be accessed from admin and users and the users route to be accessed from only the users.'
tags:
  - nodejs
  - roles
  - sailsjs
timeline_notification: "1588599921"
title: Simple role management for SailsJS
url: /2020/05/04/simple-role-management-for-sailsjs/

---
You have probably heard this form me. I am working on a [Sails](http://sailsjs.org/) js project and I wanted to implement a simple role management. That means that I have several routes like: “/admin”, “/login”, “/users” and I want the admin route  to be accessed only from admins, login to be accessed from admin and users and the users route to be accessed from only the users.

I have created a simple project which can be found on [GitHub](https://github.com/n0m0r3pa1n/sails_params_roles_validation).

I have a Roles global object which looks like this:

```
global.Roles =
  ADMIN: "ADMIN"
  USER: "USER"

```

My user model has a roles field which is an array. It looks like this:

```
UserSchema = new mongoose.Schema
  username:
    type: String
    required: true
    index: true
  password:
    type: String
    required: true
  roles:
    type: [String]
    required: true
  isActive:
    type: Boolean
    required: true
    index: true
    default: true

```

## How stuff works?

Sails has the convention of [policies](http://sailsjs.org/documentation/concepts/policies). What I did was to register a [_api/policies/sessionAuth.js_](https://github.com/n0m0r3pa1n/sails_params_roles_validation/blob/master/api/policies/sessionAuth.js) policy which determines if the current logged in user, who is stored in the req.session variable, has the role which is needed to access the route.

The role for each route can be seen in the _[config/routes.js](https://github.com/n0m0r3pa1n/sails_params_roles_validation/blob/master/config/routes.js)_ file. I simply attach the custom “roles” property which is an array of Roles to the request route. Then in the [sessionAuth](https://github.com/n0m0r3pa1n/sails_params_roles_validation/blob/master/api/policies/sessionAuth.js#L31) policy I access it from the request.options.

```
'get /home': { controller: "HomeController", action: "getDashboard", roles: [Roles.ADMIN] }

```

The last step is simply to validate if the user has each of the roles required in the route. If he does not -> throw him an exception… haha.

Simple as that using policies you can attach custom properties to the route and evaluate them later in a policy. This way I made a custom property for params validation of a request. It is in my [previous blog post](https://georgimirchev.com/2020/05/04/how-to-add-joi-params-query-body-validation-to-sails-js/).
