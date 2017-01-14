---
layout: post
title:  "How to use PostCSS with gulp"
date:   2016-10-10 13:09:43 +0900
categories: Development
---

Today I will explain how to use PostCSS with gulp.

## What is Gulp

> Automate and enhance your workflow
qt: [Official page](http://gulpjs.com/)

In other words,
it automates the work and makes it efficient.
Although I was aware that it was a tool that automates the tasks around the front end somehow
it was the first time to actually use it.

It is required that Node.js and npm are already installed on your develop environment.
I think that it is better to proceed with installation by checking the official documentation.

→[Getting Started](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md)

## How to use PostCSS

This time I will use the following in PostCSS.

- autoprefixer => Automatically grant vendor prefix
- cssnext => Transform the CSS notation that the browser is not yet implemented at the present stage so that the current browser can interpret it
- postcss-nested => You can write CSS nested
- postcss-mixins => to use mixins
- postcss-simple-vars => CSS makes variables available
- cssnano => CSS will be reduced except for spaces and line breaks

They need to install the module by using npm.

{% gist yukihirai0505/2b03c9a41ce052252976 %}

## To set up gulpfile.js

I set up `gulpfile.js` like following.

{% gist yukihirai0505/1375aeb81fcfe1d8514f %}

I will execute for the css file of this path.

    src/**/*.css

The content of css file is following.

{% gist yukihirai0505/f7af05c87363e82621cb %}

After running gulp

{% gist yukihirai0505/2ef05756187d0e4968e8 %}

When nesting and variables become available we can describe CSS considerably comfortably.
Also it is very pleasant to attach vendor prefix automatically.
It seems to be good even if you do not investigate every time at [CanIuse] (http://caniuse.com/).
In addition to gulp you can monitor files with watch to automatically execute tasks, livereload etc. using gulp - webserver, and so on.
Gulp that handles troublesome processing easily is the best.