---
layout: post
title:  "Message sent from SNS to Slack via Lambda"
date:   2016-10-01 13:09:43 +0900
categories: Development
---

I was sending error messages to developer's mail list by SNS.
But, I also want to receive it with Slack.
So I made it available for Slack.

By the way SNS is a service of AWS,
The official name is called Amazon Simple Notification Service.

[https://aws.amazon.com/sns/](https://aws.amazon.com/sns/)

## How to send error messages to Slack

The Flow is following.

- To create Incoming WebHooks with Slack
- To create Lambda
- Test
- Collaboration with SNS

## To create a function in Lambda and test it

First, entering the AWS console, select Lambda, then click "Create a Lambda Function".
Skip the next blueprint.
Next, select NodeJS for language and write code.

Regarding the code, I gave Gist what I wrote, so it may be a good to try this.

{% gist yukihirai0505/0af52c9c98cc998a10a3 %}

Basically,
if not going well, I'd like to solve it if I debug while watching Log.
I think that it is okay if you check the contents of the object while using console.log etc. well.

After creating it, let's test.
We should choose SNS for TEST.

## Collaboration with SNS

Next, select Topics from SNS and create a Subscription for Lambda you want to designate as a destination this time.
In that case,
if you designate the Lambda Endpoint created this time,
it is all right to complete the collaboration.

When there are multiple contact tools,
it is too hard to check this out,
but what you can receive with Slack can be easily checked in one place if you notify Slack.