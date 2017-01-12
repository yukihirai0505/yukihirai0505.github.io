---
layout: post
title:  "How to automate Analytics reports with Google Spreadsheet and GoogleAppsScript"
date:   2016-09-28 13:09:43 +0900
categories: Development
---

I manage data on Analytics on Google Spreadsheet.
This time I tried to automate that work with GoogleAppsScript.

When I took over reporting work, it seems too troublesome.
open analytics each time, select a period, copy and paste the result...
This is daily, weekly, monthly,,,

So I decided to automate it using GoogleAppsScript.

## How to automate this reporting work

There are some ways

- Using add-on
- Using GoogleAppsScript

I experienced to use GoogleAppsScript,
So I chose the later.

## A script for Google Analytics API

First, I enable GoogleAnalyticsAPI on script editor.
Next, I wrote codes.

{% gist yukihirai0505/80e75c5493698a85f927 %}

In the above code,
if you specify path,
we get the whole result and the results for each path and set the data in the spreadsheet.

## A script for date

Since I often dealt with dates, I summarized the processing.

{% gist yukihirai0505/5d23464ba18213628ead %}

## Execute by daily / weekly / monthly

After that, I wrote a script to execute by daily / weekly / monthly using the above method.

{% gist yukihirai0505/be0716571902d23c2600 %}

This is a script that is executed weekly,
So I made something like this on a daily / monthly basis.

## Trigger settings

We can set trigger for each script on GoogleAppsScript.

- Run at 10 o'clock every day
- Run at 10 o'clock every Monday
- Run at 10 o'clock on the 1st of every month

[https://gist.github.com/yukihirai0505](https://gist.github.com/yukihirai0505)

