---
id: 118
title: 'jshint warning &#8220;Don&#8217;t make functions within a loop&#8221;'
date: 2015-03-20T23:05:15+00:00
author: Geoff Corey
layout: post
guid: http://www.geoffcorey.com/?p=118
permalink: /2015/03/jshint-warning-dont-make-functions-within-a-loop/
pluto_featured_video_id:
  - 
pluto_featured_video_enabled:
  - 0
categories:
  - Node.js
tags:
  - javascript
  - jshint
  - node.js
---
[<img class=" size-medium wp-image-69 alignright" src="http://i2.wp.com/www.geoffcorey.com/wp-content/uploads/2015/03/nodejs-logo.png?fit=300%2C150" alt="node.js" data-recalc-dims="1" />](http://i1.wp.com/www.geoffcorey.com/wp-content/uploads/2015/03/nodejs-logo-e1426944427398.png)Ever get JSHint warning &#8220;Don&#8217;t make functions within a loop&#8221; with your node.js code? I do and my OCD strives to stamp out this warning in my code.

A typical situation for me is I want to augment a model before returning from GET REST call.   Here is a stripped down example:

<pre>'use strict';
/*jshint node: true */
var data_users = [
{
  "id": "aaaa",
  "name": "moe"
},
{
  "id": "bbbb",
  "name": "larry"
},
{
  "id": "cccc",
  "name": "curly"
}];


function fakeRedis(key, cb)
{
  var data_ttl = {
    "aaaa": 600
  };
  var result = data_ttl[key];
  if (!result)
  {
    result = -2;
  }
  cb(null, result);
}

function preResolveHook(users)
{
  for (var i in users)
  {
   if (users.id)
   {
    fakeRedis(users[i].id, function(err, ttl)
    {
       if (users[i])
       {
          users[i].ttl = ttl;
          if (i === users.length - 1)
          {
            console.log(users);
            // next();
          }
        }
      });
    }
  }
}

preResolveHook(data_users);
</pre>

Run jshint on the above code I get

<pre>bad.js: line 48, col 14, Don't make functions within a loop.
</pre>

Using &#8216;Q&#8217; for promises I reworked the code to fire off all the FakeRedis calls and stuff the returned promises in an array. Then I use Q.all([promises]) to wait for all the FakeRedis calls to complete and then augment the model with the result.

<pre>'use strict';
/*jshint node: true */
var Q = require('q');
var data_users = [
{
  "id": "aaaa",
  "name": "moe"
},
{
  "id": "bbbb",
  "name": "larry"
},
{
  "id": "cccc",
  "name": "curly"
}];


function fakeRedis(key, cb)
{
  var data_ttl = {
    "aaaa": 600
  };
  var result = data_ttl[key];
  if (!result)
  {
    result = -2;
  }
  cb(null, result);
}

function promiseTTL(id)
{
  var defer = Q.defer();
  fakeRedis(id, function(err, ttl)
  {
    if (err)
    {
      console.log(err);
      defer.resolve(-3);
    }
    else
    {
      defer.resolve(ttl);
    }
  });
  return defer.promise;
}

function promisePreResolveHook(users)
{
  var pArray = [];
  for (var i in users)
  {
    if (users[i].id)
    {
      pArray.push(promiseTTL(users[i].id));
    }
  }
  Q.all(pArray).then(function(array)
  {
    for (var i in array)
    {
      if (array[i])
      {
        users[i].ttl = array[i];
      }
    }
    // next();
  });
}

promisePreResolveHook(data_users);

</pre>

If you have a better way of handling this situation I would love to hear from you!

<div class="changetip_tipme_button" data-bid="xFu4UWWvufahiS3MkCQzci" data-uid="kZJeSKkyNFLTcR9hhZcRyH">
</div>