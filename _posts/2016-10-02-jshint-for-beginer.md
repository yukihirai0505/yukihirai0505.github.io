---
layout: post
title:  "Beginners will be able to write beautiful Javascript using JSHint (ESLint / GJSLint)"
date:   2016-10-02 13:09:43 +0900
categories: Development
---

A beginner like Javascript like me,
after seeing the code after continuing to write Javascript,
it is often a strange way of writing. (Is it only me?)

So I'll write about the tools that point out that we are strangely writing.

It is JSHint.
I will also explain about ESLint and GJSLint.

## What is JSHint?

> JSHint is a static code analysis tool used in software development for checking if JavaScript source code complies with coding rules.
It was forked from Douglas Crockford's JSLint project,
as it was felt that the original did not allow enough customization options.
qt: [JSHint](https://en.wikipedia.org/wiki/JSHint)


So if you use SHint, 
that helps to check Javascript source code.

Node.js is necessary to install, so do not forget to install it.
From the command line 

    npm install -g jshint

After installing it,
let's create the JSHint configuration file (.jshintrc) in your home directory like following.

{% gist yukihirai0505/a2c3c589365dc8b09828 %}

You can check more configuration.

[http://jshint.com/docs/](http://jshint.com/docs/)

From the command line

    jshint [javascript file]

You can see like following logs.

> jquery.js: line 232, col 0, Identifier 'core_slice' is not in camel case.
jquery.js: line 238, col 18, Expected '===' and instead saw '=='.
jquery.js: line 277, col 0, Identifier 'core_slice' is not in camel case.

## When using JSHint with Vim

Since it is annoying to run jshint on command line every time, I set it with Vim.

### Setting for Vim

In the case of Vim, you can add a plug-in.
NeoBundle is useful for managing plugins, so I use it here.

Install NeoBundle like this

{% gist yukihirai0505/d00f9cd3c9622f26ab5d %}

After installing it, let's set up `.vimrc` file.

{% gist yukihirai0505/20fdf1edb42f44a90037 %}

Next, install this plugin of syntax check system.

    NeoBundle 'scrooloose/syntastic'

{% gist yukihirai0505/e76108e927581c1d1b93 %}

Now that you open Vim you will be asked if you want to install scrooloose / syntastic and install it.
Next, you should set up `.vimrc` again.

{% gist yukihirai0505/a8a8fcbfbc560d1db1ec %}

If you set it like this,
JSHint syntax check will work when editing with Vim.
In this case, the syntax check runs when opening a file or saving.
It may be a little bit annoying, so please go ahead and set your preferences.

## How to instsall ESLint and GJSLint

ESLint has the same function as JSHint,
and analysis rules are completely pluggable,
so you can add your own rules freely.

To install it,

    npm install -g eslint

Next, GSLint is a tool that can check / modify according to the Google JavaScript Style Guide.

To install it,

{% gist yukihirai0505/8d1e623bfa62edeecb9c %}


After that, you should set up `.vimrc` file.

{% gist yukihirai0505/b99e30b2ccb83ab40ee0 %}


By the way, when you edit a file with vim, the following command helps to check your environment.

    :SyntasticInfo

    Syntastic version: 3.6.0-156 (Vim 703, Darwin)
    Info for filetype: javascript
    Global mode: active
    Filetype javascript is active
    The current file will be checked automatically
    Available checkers: eslint gjslint jshint
    Currently enabled checkers: eslint jshint gjslint


I would like to aim for beautiful Javascript code with syntax checking tool.
