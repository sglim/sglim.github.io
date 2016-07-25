---
layout    : posts
title     : "Logstash, GeoIP, and the first server I actually owned"
author    : Dennis Lim
date      : 2016-07-25 22:45:00 +0900
categories: computer science
---

The travel app has a name now and I'm the tech lead for it, which at a company this size means I'm the guy who writes the server and also the guy who gets paged when it breaks.

Stack, for the record: Node.js on the backend, a bunch of microservices behind a gateway, Swift on iOS. We are searching flights, which means we are talking to airline data providers that were designed in a decade when XML was the future. The fun part is that every provider is different and every provider lies a little.

This week was not about flights though. It was about logs.

We had been logging to files and grepping. That works for one server. We have several now and I got tired of ssh-ing into each one to find out which of them threw the error. So I set up the ELK stack. Elasticsearch, Logstash, Kibana. Two days of work, mostly fighting Logstash config syntax, which looks like Ruby but is not Ruby and will happily accept a config that does nothing.

The thing that made it click was GeoIP. Logstash has a filter that takes an IP address and adds country, city, coordinates. I turned it on and suddenly Kibana had a map. Little dots where our users were. Most of them in Seoul, some in Tokyo, a surprising cluster in Southeast Asia that turned out to be one of the founders on vacation testing the app.

I stared at that map for way too long. It is a dumb feature. It changed how I think about the service. Before, users were rows in a database. After, they were dots that appeared when someone opened the app on their phone at an airport. When a dot appeared in a city we did not support yet, I felt it.

Some notes for future me:

- Structured logs from the start. Log JSON, not sentences. Logstash can parse sentences with grok but you will hate yourself.
- Put the request id in every log line. Every single one. Tracing a request across three services without it is guessing.
- Kibana dashboards rot. Make three good ones and delete the rest.
- Elasticsearch will eat all the memory you give it and then ask for more. Set the heap. Set it again after you forget you set it.

The bigger lesson is about ownership. At the game company somebody else ran the servers. Here I do. The first time you own the box end to end, deploy to it, watch its memory, read its logs, get woken up by it, you understand your own code differently. You stop writing "it should never happen" comments because you know exactly who has to deal with it when it happens.

It will happen. Probably on a Friday.
