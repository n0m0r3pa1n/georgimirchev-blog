---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:21"
_publicize_job_id: "43821597157"
author: gmirchev90
categories:
  - web
date: "2020-05-04T12:00:35+00:00"
guid: http://georgimirchev.com/?p=134
parent_post_id: null
post_id: "134"
summary: So we wanted to integrate a websockets server within our REST api for which we were using [HapiJS](http://hapijs.com/). I was previously using [Socket.IO](http://socket.io/) solution for websockets and I was very happy with it. So we decided to use it with our REST server in a single codebase.
tags:
  - nodejs
  - socketio
timeline_notification: "1588593635"
title: Let’s hook up Socket.IO with HapiJS in a single code base
url: /2020/05/04/lets-hook-up-socket-io-with-hapijs-in-a-single-code-base/

---
So we wanted to integrate a websockets server within our REST api for which we were using [HapiJS](http://hapijs.com/). I was previously using [Socket.IO](http://socket.io/) solution for websockets and I was very happy with it. So we decided to use it with our REST server in a single codebase.

But we faced a problem with several solutions:

> We wanted an action, which happens in the REST api (like a POST request), to trigger an event which will tell the sockets server to send some new info to selected clients which are connected.

To implement this you have several solutions:

1. Separate the two servers and make the talk to each other using [RabbitMQ](https://www.rabbitmq.com/) . This is the hardest and trickiest solution.
1. Use the [Socket.IO-Emitter](https://github.com/socketio/socket.io-emitter). But it requires you to have a Redis database so you can talk to the sockets server. Not nice solution.
1. Use an ordinary client to connect to the Socket.IO server. But do you have to open a connection each time a request is made? It sounds bad.
1. Use [HapiNES](https://github.com/hapijs/nes) – sockets implementation specific for Hapi with option to communicate between each other. I haven’t tried this because I wanted to stick with Socket.IO.
1. Use the NodeJS EventEmitter to send events from the one server to the other. I didn’t know about this. Thanks to Matt Harrison for [answering](http://stackoverflow.com/questions/32760678/connection-between-socket-io-and-hapijs) my question.

I decided to use the 5-th option to communicate through the built-in EventEmitter. And it worked as Matt’s example is showing. Here is the example:

```
var Hapi = require('hapi');

// Make an event emitter for managing communication
// between hapi and socket.io code

var EventEmitter = require('events');
var notifier = new EventEmitter();

// Setup API + WS server with hapi

var server = new Hapi.Server();
server.register(require('inert'), function () {});

server.connection({ port: 4000, labels: ['api'] });
server.connection({ port: 4001, labels: ['ws'] });

var apiServer = server.select('api');
var wsServer = server.select('ws');

apiServer.route({
    method: 'GET',
    path: '/',
    handler: function (request, reply) {

        reply.file('index.html');
    }
});

apiServer.route({
    method: 'GET',
    path: '/action',
    handler: function (request, reply) {

        notifier.emit('action', { time: Date.now() });
        reply('ok');
    }
});

// Setup websocket stuff

var io = require('socket.io')(wsServer.listener);<!DOCTYPE html>
io.on('connection', function (socket) {

    // Subscribe this socket to `action` events

    notifier.on('action', function (action) {
        socket.emit('action', action);
    });
});

server.start(function () {
    console.log('Server started');
});
```

Here is a basic HTML page:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="http://localhost:4001/socket.io/socket.io.js"></script>
</head>
<body>
    <script>
    var socket = io('http://localhost:4001');
    socket.on('action', function (action) {
        console.log(action);
    });
    </script>
</body>
</html>
```
