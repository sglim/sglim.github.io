---
layout    : posts
title     : "Smart sockets and two Ender-3s"
author    : Dennis Lim
date      : 2018-06-15 22:50:00 +0900
categories: computer science
---

I'm the tech lead of an IoT team now. That sentence would have surprised me a year ago. The travel app is in good hands with the team that grew around it, and the company wanted to try hardware. Smart power sockets, specifically. The kind you plug a lamp into and then control from your phone, except we want the socket to be smart about what is plugged into it.

So for the last few months my life has been Bluetooth, gRPC over weird transports, and a gateway that sits between little devices and the cloud.

Some things about hardware that software people (me, three months ago) do not know:

Firmware updates are terrifying. On a server, a bad deploy is a rollback. On a socket in somebody's living room, a bad firmware is a brick. We spent more time on the update path than on any feature. Two partitions, verify before switching, fall back if the new one does not boot. Boring and essential.

Bluetooth Low Energy is low energy because it does almost nothing. The payload sizes are tiny. We ended up designing our own little RPC layer on top of BLE characteristics so the phone and the socket could talk in something like a protocol instead of raw bytes. I have new respect for anyone who ships a BLE product that works reliably.

The physical world is noisy. Power measurements jump around. Wi-Fi drops when the microwave runs. A socket that reports once a second produces a lot of garbage and you have to smooth it before it means anything. Android's "it works on my device" problem is nothing compared to "it works on my kitchen outlet".

And then there are the printers.

We bought two Ender-3s for the project. Cheap 3D printers, a couple hundred dollars each. The idea was to print enclosures for prototype boards instead of waiting weeks for a case. That is what they are for. That is not what happened.

What happened is that I started printing everything. Cable clips. A phone stand. A better spool holder for the printer itself, which is a very 3D printing thing to do. At the demo day last quarter I was officially listed as the mechanical engineer, which is generous, but I did design and print the stand that held the demo together.

The printers taught me something about the IoT work too. A 3D printer is an IoT device. It has firmware, it has a serial protocol, it fails in physical ways. When a print goes wrong at layer 200 of 300, you learn to think about failure modes that a web developer never sees. The bed was not level. The filament got wet. The room got cold overnight. None of that is in the logs.

I'm thinking about buying one for home. My apartment is small. It would be a bad idea. I'm thinking about it anyway.
