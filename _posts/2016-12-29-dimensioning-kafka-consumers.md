---
layout: post
title: "Dimensioning Kafka consumers"
description: ""
category: 
tags: [kafka,bigdata]
---
{% include JB/setup %}
One of the questions that comes up when you start preparing to use [Apache Kafka](https://kafka.apache.org/) is - how fast must my Kafka consumer be - to not lose data?

Since you have probably configured retention for your data, you know that after some time (or amount of bytes) - your data will be dropped from the Kafka cluster. This usually brings up the previously mentioned question.

What are the factors that you have to consider when dimensioning/sizing your Kafka consumer?

 * producer speed (messages/sec) - sum of speed of all producers that are producing messages to the given Kafka topic. This is the average speed. You have to calculate/estimate it for yourself.
 * retention of your data in the Kafka topic (duration ; minutes) - here I will use retention period - the amount of time (duration) Kafka will retain the data before dropping it.
 * amount of downtime you plan to have (duration ; minutes) - this can be consumer downtime, but can also be producer downtime. When the producer is down, and this producer is able to re-read data that didn't produce, then this will cause a spike, and then the consumer must be able to process messages with greater speed.

I have come up with a formula that takes into account these factors and calculates the minimum Kafka consumer speed that you need to have in order to not lose data. Here it goes.

![kafka-consumer-dimensioning-formula](/assets/images/kafka-consumer-dimensioning-formula.png)

The details of how I have come up with this formula, I will write in another post, I hope ;)

An interactive version of the given formula, where you can change the three factors/variables and dimension your Kafka consumers:
<iframe scrolling="no" src="https://www.geogebra.org/material/iframe/id/VwAswM8G/width/800/height/400/border/888888" width="800px" height="400px" style="border:0px;"> </iframe>

If you can guarantee no downtime, then you don't need this formula, then the consumer speed will have to be the same as the producer speed. You can actually enter 0 for <code>DOWNTIME<sub>Min</sub></code> in the interactive formula, and you'll see that:

<code>Consumer<sub>MinSpeed</sub> = Producer<sub>Speed</sub></code>

But I regularly have this situation - an app that is reading data from another datacenter (DC) - because of DC link failure or congestion - stops reading data, and thus stops producing data to Kafka. When the link is back online, this app will start producing messages, but now the number of messages per second is not the regular speed, it's 20x of the regular speed for example. And if your consumer on the other side is only able to consume data at regular speed, then you have somehow scale-up your consumer. How to scale-up is up to you, but here are some options:

 * add more consumer instances - horizontal scaling
 * add more hardware to the consumer - vertical scaling
 * optimize your code that is processing the messages - lower the processing latency (increase of throghput)

Hope this can help you dimensioning/sizing your Kafka consumers.

If you have any questions or suggestions, write a comment down below.


