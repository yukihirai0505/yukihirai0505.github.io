---
layout: post
title:  "How to push regularly to the browser from the server using Comet and Akka Actor"
date:   2016-10-04 13:09:43 +0900
categories: Development
---

I tried the method of regularly pushing the browser from the server with Comet × Akka Actor using Playframework 2.4,
so I will summarize it.

In order to place emphasis on periodically performing browser push,
I will implement it as simple as possible.

## What is Comet

Comet is a WEB application model that keeps requests for long periods of time after sending requests to the server or until server events occur.
To put it briefly, it can push information from the server to the client.

You can check more detail on a following link.
[https://en.wikipedia.org/wiki/Comet_(programming)](https://en.wikipedia.org/wiki/Comet_(programming))

## What is Akka Actor

Parallel processing can be implemented using Akka Actor.

You can check more detail on a following link.
[http://doc.akka.io/docs/akka/current/general/actors.html](http://doc.akka.io/docs/akka/current/general/actors.html)


## How to implement

The flow is following.

- ① Implementation of User (It is not necessary to be a User)
- ② Implementation of Actor
- ③ Implementation of Controller
- ④ Implementation of View

## Implementation of User (It is not necessary to be a User)

First, I created User class that has counts.

{% gist yukihirai0505/ce75b97f80d4fab1a803 %}

### Creating class fot Actor

Next to create a class to pass to the Actor with Comet object.

{% gist yukihirai0505/2d3d4774cd89b7c57fc9 %}

## Implementation of Actor

{% gist yukihirai0505/87a9fc69c9b5d85e8f7f %}

## Implementation of Controller

{% gist yukihirai0505/7d4e68839f8f32baf340 %}

## Implementation of View

{% gist yukihirai0505/ddb872a3a4a35d51461d %}

With the forever iframe,
the content of the iframe will be rewritten each time there is push from the server.

The point is following source code.

{% gist yukihirai0505/cb0718222b0c27f8be12 %}

## Summary

By applying these,
it is possible to create a chat or to keep the user waiting until the value of the table changes.

Github is here.

[https://github.com/yukihirai0505/comet_actor](https://github.com/yukihirai0505/comet_actor)