---
layout: post
title: Introducing Koa 
cover: cave-house.jpg
date: 2014-08-21
categories: webdev nodejs
---


I wrote a simple todo list web app recently as a way of playing with AngularJS. I started off with the basic todo.js sample on [http://angularjs.org](http://angularjs.org) and modified it to allow allocating tasks to groups. Of course as great as AngularJS is in the browser, it is crippled without a backend to talk to, so I created an Express backend to provide the API layer.

As I am a sucker for punishment I decided to ignore the herd and use a CouchDb backend instead of the standard MongoDb. This was partially from curiosity to see if the CouchDb way was any better, and also to keep me from simply copying every other example out there. Thankfully, the nano library saved the day and provided a simple abstraction ove the CouchDb API.

The API was simple, a GET to read a document storing all todos and a POST to replace the document with an updated version. At this point the years of working with a relational database screamed out about the inefficiency of updating the entire document when only a part of it had changed, but it isn't a high performance app. 

The Express routes were pretty simple
{% highlight JavaScript %}
  app.get('/api/todos', api.fetchTodos);
  app.post('/api/todos', api.updateTodos);
{% endhighlight %}

The API itself was simple 2 functions wrapping up calls to Cloudants REST API via the nano library Here is the GET.

{% highlight JavaScript %}
'use strict';
var nano = require('nano')(process.env.COUCH);

exports.fetchTodos = function(req, res) {
  var db = nano.use('todo');
  db.get('wsinned', {}, function(err, body, header) {
    if (!err) {
      res.json(body);
    } else {
      console.log(err);
    }
  });
};
{% endhighlight %}

By the time I had this working I was pleased with myself for having wired up 3 different concerns to make may app work, but displeased with the look of the API code. NodeJs never feels clean to me. All the nesting of callbacks always adds to the contextual load, just to figure out what a single line of code is doing. This happens every time I venture into JavaScript land.

## Enter Koa

Fast forward a month or so, and I have just heard a dicussion of the Koa framework on the [JavaScript Jabber](http://javascriptjabber.com/117-jsj-the-koa-framework-with-gerred-dillon-and-will-conant/) podcast. The concept of EcmaScript 6 generators syntax appeals to me, as does anything that cleans up code so that I can focus on what it does and ignore the boilerplate around it. So I decided to recreate the backend API using Koa to test the water and see if there is a way forward for me and JavaScript on the server.

Firstly, the routes were defined using koa-route middleware. No surprises here, once you know where the route object came from.
{% highlight JavaScript %}
app.use(route.get('/api/todos', api.fetchTodos));
app.use(route.post('/api/todos', api.updateTodos));
{% endhighlight %}

Then the API is defined with the help of co, nano and co-nano.
{% highlight JavaScript %}
'use strict';
var co = require('co');
var coNano = require('co-nano')(require('nano')(process.env.COUCH));

module.exports.fetchTodos = function * fetchTodos(){
  var db = coNano.use('todo');

  try {
    var res = yield db.get('wsinned', {});
    this.body = res[0];
  } catch(e) {
    console.log(e.code);
  }
};
{% endhighlight %}

The key things to note here are

* \* Marks a generator function.
* yield is called on functions that can take time to execute preventing blocking.
* The response from [nano err, body, header] is yielded as [body, header] when using the co-version, and err is thrown, so I am  using try/catch semantics.
* [Co](https://github.com/visionmedia/co) is used to provide generator based flow control, and co-modules like co-nano are co-enabled versions of the original API, exposing the API as thunks.

There is a lot more to Koa than just clean code, and there is a lot I haven't explained here for the sake of brevity. The middleware stack that is enabled by the Koa style of flow control allows complex stacks to be written simply, and understood easily when the code is read later. I am all for write-once read-many code.