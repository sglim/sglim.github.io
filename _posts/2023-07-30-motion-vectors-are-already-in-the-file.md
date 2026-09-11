---
layout    : post
title     : "Motion vectors are already in the file"
author    : Dennis Lim
date      : 2023-07-30 21:40:00 -0400
categories: computer science
---

I spent the last two weeks on a side quest that I think will turn into a main quest. It started with a question that had been bothering me for a year. We do a lot of work to figure out how things move in a video. Optical flow, tracking, expensive stuff. And modern video codecs already know how things move. That is how they compress. Why are we computing it twice?

Every inter frame in an H.264 or HEVC stream is described as "take this block from that previous frame, moved by this much". The "moved by this much" is a motion vector. The file is full of them. Thousands per frame. The decoder uses them and throws them away.

So I went looking for a way to keep them.

There is a small open source tool that patches into the decoding path and dumps the motion vectors per frame as a matrix. I forked it, fixed a few things that had rotted since it was written, got it building against a current codec library, and pointed it at our test clips. Then I wrote a visualizer. Arrows on the frame, one per block, colored by magnitude.

The result is rough and it is also obviously useful. You can see the object moving. You can see the camera pan as a uniform field. You can see where the compressor got confused at an edge. All of this for free, at decode speed, no neural network, no GPU.

What it is good for, in our case: our format needs to know how an object moves across a clip so the player can map a drag gesture to the right frames. Right now that mapping is computed with a proper motion estimation step. It is accurate and it is slow. Motion vectors are less accurate and essentially free. For a lot of clips, less accurate is plenty, and free means we could do it on device instead of on a server.

What it is bad for: precision. Codec motion vectors are chosen to minimize bits, not to describe the true motion. At a flat wall the encoder will pick any vector that works, and "any vector" is noise to us. Around occlusions it is worse. So this is a first pass, a cheap prior that a real algorithm can refine, not a replacement.

I put together a proposal for the team with the visualizer and a few clips. The reaction was better than I expected. The engine lead's question was "how fast" and the answer was "faster than decoding, because it is part of decoding". That is the kind of answer that gets a project approved.

Some notes on the work itself:

- Reviving an old open source tool is half archaeology. The commit history told me more than the README.
- Write the visualizer first. Numbers in a matrix convinced nobody. Arrows on a frame convinced everyone.
- The idea was not new. It is in papers from fifteen years ago. What was missing was someone on our team who had time to try it.

It is a quiet summer, the good kind. I have arrows on frames and a proposal in review, and that is a good two weeks.
