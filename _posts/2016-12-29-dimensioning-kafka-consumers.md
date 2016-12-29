---
layout: post
title: "Dimensioning Kafka consumers"
description: ""
category: 
tags: [kafka,bigdata]
---
{% include JB/setup %}
One of the questions that comes up when you start preparing to use [Apache Kafka](https://kafka.apache.org/) - how fast must my Kafka consumer be - to not lose data?

Since you have probably configured retention for your data, you know that after some time (or amount of bytes) - your data will be dropped from the cluster. This usually brings up the previously mentioned question.

What are the factors that you have to consider when dimensioning/sizing your Kafka consumer?

 * producer speed (messages/sec) - sum of speed of all producers that are producing messages to the given Kafka topic. This is the average speed. You have to calculate/estimate it for yourself.
 * retention of your data in the Kafka topic (duration ; minutes) - here I will use retention period - the amount of time (duration) Kafka will retain the data before dropping it.
 * amount of downtime you plan to have (duration ; minutes) - this can be consumer downtime, but can also be producer downtime. When the producer is down, and this producer is able to re-read data that didn't produce, then this will cause a spike, and then the consumer must be able to process messages with greater speed.

I have come up with a formula that takes into account these factors and calculates the minimum Kafka consumer speed that you need to have in order to not lose data. Here it goes.

![kafka-consumer-dimensioning-formula](/images/kafka-consumer-dimensioning-formula.png)


