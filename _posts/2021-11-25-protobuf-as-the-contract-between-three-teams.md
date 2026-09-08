---
layout    : post
title     : "Protobuf as the contract between three teams"
author    : Dennis Lim
date      : 2021-11-25 23:10:00 +0900
categories: computer science
---

We have a repo that is nothing but proto files. It generates code for TypeScript, for Swift and Kotlin through Objective-C and Java, for Dart, and for Rust. Every team consumes it. Nobody loves it. It is the most important repo in the company.

I want to write about why, because I spent a lot of this fall in it.

The format that describes an interactive video is a tree. Layouts, moments, actions, resources. The maker tool writes it, the player reads it, the admin edits it, the engine executes it. Four consumers in four languages. If each of them had its own model, they would drift, and drift in a media format means a video that plays in the maker and does not play on a phone.

So the model is defined once, in proto, and generated everywhere. Change the proto, regenerate, every consumer sees the new field at compile time. That is the whole idea and it works.

What makes it hard is that the proto repo is where every disagreement between teams becomes concrete. The player team wants a field for rotation ratio so they can play all frames regardless of the max angle. The maker team wants a coordinate type on layouts so they can stop guessing units. The engine team wants the action type copied into a related message so they do not have to walk the tree. Every one of those is a good idea and every one is a review with three teams in it.

Things I learned about running a shared schema:

Generated docs are not optional. We generate a doc page from the proto comments. When I forgot to run gen_doc after a change, someone found out the hard way that the doc and the code disagreed. Now it is in CI.

Comments are the API. The field name says what it is. The comment says what it means and, crucially, what happens if it is missing. Half our bugs this fall were "the field was optional and each consumer defaulted it differently".

Review across teams needs a person, not a process. I became that person by accident. Every proto change gets my eyes not because I own it but because I am the one who has read all four consumers. That is fragile. I am writing down what I look for so it is not just me.

Version the proto repo like a library. Tag releases. Consumers pin a tag. A consumer that is three tags behind is a known state, not a mystery.

The thing I like most about this setup is that it makes architecture visible. In a lot of companies the "architecture" is a diagram that is out of date. Here it is a file. If you want to know what a moment is, you read the message. If you want to change what a moment is, you open a pull request and three teams show up. That is not friction. That is the system working.

Next quarter I want to add a compatibility check to CI, so nobody can remove or renumber a field without the build failing. I have a feeling I will write about why that matters after somebody does it.
