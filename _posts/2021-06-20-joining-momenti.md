---
layout    : post
title     : "Joining Momenti: video that responds to you"
author    : Dennis Lim
date      : 2021-06-20 22:00:00 +0900
categories: life
---

I left the CTO job. Last week I started at Momenti, a company that makes video you can touch.

Let me explain the second part first because it is the reason for the first part.

Normal video plays forward. You watch. Momenti's format lets you interact. Rotate a product by dragging. Scrub through a moment by tilting the phone. Tap something in the frame and the video responds. Under the hood it is not one video, it is many, plus a description of how user input maps to which frames play. The player has to be fast enough that it feels like you are moving the object, not selecting a clip.

That is a hard problem in a way I find exciting. Video codecs, frame accurate seeking, touch input, doing it all on a phone browser with no plugins. The kind of thing where the difference between good and bad is measured in milliseconds and you can feel it in your thumb.

My title is Technical Architect. In practice, for the first month, that means reading. There is a Rust engine, a web player in TypeScript, a Flutter app, a proto definition that ties them together, and a lot of tribal knowledge in people's heads. My first deliverable is a document that describes the media format properly, so that the next person does not have to spend a month reading.

Why I left a CTO title for this:

I liked being CTO more than I expected. I did not love it. The parts I loved were the design docs and the occasional YAML. The parts I did not love were most of the calendar. A year was enough to learn that about myself, and the company was in a good place to hand over.

Also the product. Flights and hotels are a data problem. This is a systems problem. I have been on the data side for five years and I wanted to go back to bits and frames and the kind of bug where you stare at a hex dump.

Small things about the first week:

- Everyone is on a video call. The company is half in Seoul and half elsewhere. This is my first fully distributed team and I am learning to write things down before meetings instead of during.
- The codebase is younger than the last two and it shows in good ways. Fewer layers. More "why is this here" answered by git blame in one hop.
- I switched time tracking apps the same week. Five years and change of one app, one backup file, and a fresh start in a new one. It felt appropriate.

I do not know yet what the hardest part of this job will be. My guess is the player on low end Android browsers. I will write about it when I find out.
