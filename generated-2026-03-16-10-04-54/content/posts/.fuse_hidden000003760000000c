---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:24"
_publicize_job_id: "43824893347"
author: gmirchev90
categories:
  - web
date: "2020-05-04T13:41:58+00:00"
guid: http://georgimirchev.com/?p=187
parent_post_id: null
post_id: "187"
summary: Let’s keep the things short. I am working on a [SailsJS](http://sailsjs.org/get-started) app and I am missing a lot the [HapiJS Joi](https://github.com/hapijs/joi) validation features. It keeps the validation logic separate and isolated from the controllers and other stuff. So I was wondering how to integrate Joi with Sails.
tags:
  - joi
  - nodejs
  - sailsjs
timeline_notification: "1588599719"
title: How to add Joi params, query, body validation to Sails JS?
url: /2020/05/04/how-to-add-joi-params-query-body-validation-to-sails-js/

---
Let’s keep the things short. I am working on a [SailsJS](http://sailsjs.org/get-started) app and I am missing a lot the [HapiJS Joi](https://github.com/hapijs/joi) validation features. It keeps the validation logic separate and isolated from the controllers and other stuff. So I was wondering how to integrate Joi with Sails.

Sails has the concept of policies. A policy is a function that receives request and response, executes some code and either prevents the request from continuing or calls the next() function so the request can propagate to the other policies. A policy is a simple js file placed in the policies folder of the project and registered in the config/policies.js file of your project. I made a small project on Github to illustrate that – [HERE](http://git@github.com:n0m0r3pa1n/sails_params_roles_validation.git).

I made a [modelValidation.js](https://github.com/n0m0r3pa1n/sails_params_roles_validation/blob/master/api/policies/modelValidation.js) policy which is registered in the [config/policies.js](https://github.com/n0m0r3pa1n/sails_params_roles_validation/blob/master/config/policies.js) file for the [AuthController.login](https://github.com/n0m0r3pa1n/sails_params_roles_validation/blob/master/api/controllers/AuthController.coffee) which is a POST method – you can verify it in the [config/routes.js](https://github.com/n0m0r3pa1n/sails_params_roles_validation/blob/master/config/routes.js) file. Here is what the policy does:

```
function modelValidation(req, res, next) {
  // get the req.options.validation property and try to load a file
  // from the api/validaton directory with the property name
  var schema = require('../validation/' + req.options.validation)
  if (!schema) {
    return res.serverErrorJson(new Error("Missing validation schema for " + req.options.controller + " " + req.options.action))
  }

  // Here body can also be used with params, query etc.
  Joi.validate(req.body, schema.body, function (err, value) {
    if (err != null) {
      error = err.details[0]
      error.statusCode = 400
      return res.badRequest({ error: error }, req.options.action);
    } else {
      return next();
    }
  })
};

```

Shortly speaking. It takes the property of req.options.validation. It can be seen in the [routes.js](https://github.com/n0m0r3pa1n/sails_params_roles_validation/blob/master/config/routes.js#L3) file. The validation property is “login”. So it will look for the following file _api/validation/login.js_.

It will validate then the request body against the schema.body declared in the login.js file, which is a Joi validation schema and based on that will either continue or return bad request to the client. [Here](https://github.com/n0m0r3pa1n/sails_params_roles_validation/blob/master/api/validation/login.js) is the required schema, it requires the password to contain only numbers. Using this same approach you can validate the params or the query of the body. You just have to add some additional validation logic.

So this is the whole concept of using Joi with Sails. If you have any questions ask me here or write me an email. And don’t forget to check the [GITHUB example](https://github.com/n0m0r3pa1n/sails_params_roles_validation).
