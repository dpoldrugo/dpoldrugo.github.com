---
layout: post
title: "Dimensioning Kafka consumers"
description: ""
category: 
tags: [kafka,bigdata]
---
{% include JB/setup %}
One of the questions that comes up when you start preparing to use [Apache Kafka](https://kafka.apache.org/) - how fast must my Kafka consumer be - to not lose data? Since you have probably configured retention for your data, you know that after some time (or amount of bytes) - your data will be dropped from the cluster. This usually brings up the previously mentioned question.

What are the factors that you have to consider when dimensioning/sizing your Kafka consumer?
 * producer speed
