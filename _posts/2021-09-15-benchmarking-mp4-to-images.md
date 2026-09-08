---
layout    : post
title     : "Benchmarking mp4 to images"
author    : Dennis Lim
date      : 2021-09-15 22:45:00 +0900
categories: computer science
---

Three months in and the first real architecture question landed on my desk. It is a small question with a big answer, which is my favorite kind.

The question: when the player needs a specific frame of a video, right now, what is the fastest way to get it?

Our format is interactive. The user drags and the picture has to follow their finger. That means random access to frames, not sequential playback. Normal video players are built for sequential. Seek is an afterthought, and on the web it is a slow afterthought, because the browser has to find the nearest keyframe and decode forward from there.

So there is a tempting alternative. Do not use video at all. Turn the mp4 into a pile of images ahead of time, one per frame, and just swap images. Random access becomes an array lookup.

That trades decode time for download size and memory. The question is by how much. I did not want to argue about it, so I wrote a benchmark.

Same source clips, a few resolutions, a few lengths. Four approaches:

1. mp4, seek via the video element, measure time to first painted frame after seek.
2. mp4, decode everything up front into a frame cache, measure decode time and memory.
3. Pre extracted images, JPEG, measure total bytes and time to display frame N.
4. Pre extracted images, WebP, same.

Some numbers, rounded because the exact ones depend on the machine:

Seeking the video element is slow and inconsistent. Tens to hundreds of milliseconds, worse on Android, and it depends on where the keyframes are. For dragging it is unusable. You can feel every seek.

Decoding everything up front is fast to access and terrible on memory. A ten second clip at a modest resolution is hundreds of megabytes of raw frames. Mobile browsers kill the tab.

Images are the middle path. WebP came out about a third smaller than JPEG at similar quality. Total download for a clip is several times the mp4 size, which hurts, but access is instant and memory is whatever the browser decides to cache. Progressive loading helps: load every fourth frame first, fill in the rest. The user can start dragging before the download finishes.

What we decided: images for the interactive parts, video for the linear parts. The format already distinguishes them, so the player can choose per segment. Not elegant. Correct for now.

What I actually learned:

- Write the benchmark before the meeting. The meeting was fifteen minutes because the numbers were on the screen.
- Put the benchmark in a repo with a README. Somebody will want to rerun it when a new codec or a new API shows up, and "I tested it once in September" is not reproducible.
- The obvious answer, "just use video", was wrong for our use case, and the way to find that out was not to think harder, it was to measure.

There is a longer term answer involving decoding in the engine itself rather than relying on the browser. That is a bigger project and a future post. For now, images. Lots of images.
