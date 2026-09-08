---
layout    : post
title     : "Shipping a Rust engine to the browser with WASM"
author    : Dennis Lim
date      : 2022-09-25 21:15:00 -0400
categories: computer science
---

The engine that plays our interactive video format is written in Rust. Until this year it ran on phones through FFI and on the web through a separate TypeScript reimplementation. Two players, one format, and every new feature had to be built twice. This summer I led the work to ship the Rust engine to the browser directly through WebAssembly, and it is live now, so here is the write up.

Why it was worth doing:

The TypeScript player was good. But every format change meant a change in the engine and a matching change in the web player, reviewed by different people, tested separately, and they drifted. When a video behaved differently on web and mobile, the first hour of every bug was figuring out which one was right. One engine means one truth.

What the architecture looks like now:

The Rust core knows nothing about platforms. It takes the format, takes input events, and produces "show frame N of resource R at this transform". That is it. Around it are thin platform layers. On iOS and Android the layer is FFI and native rendering. On web the layer is a WASM module and a canvas. The layers are small enough that one person can read each in an afternoon.

The hard parts, in order of how much time they ate:

Memory. WASM has its own linear memory and the browser has its own. Frames live in the browser side as decoded images. The engine needs to know about them but must not copy them. We ended up with the engine holding handles and the JS side owning the pixels. Getting that boundary right took longer than everything else combined.

Size. The first build was several megabytes. Nobody downloads that to watch a ten second clip. Strip symbols, opt for size, remove a serialization library that was pulling in half the world, lazy load the module after the first frame is already showing from a poster image. Down to a size that is fine on mobile networks. Not tiny. Fine.

Threads. Rust wants threads. Browsers give you web workers with restrictions and a flag that half the hosting configurations do not set. We run single threaded on web for now. The engine was written so that is a configuration, not a rewrite.

Debugging. When the engine panics inside WASM, the browser gives you a number. Source maps for Rust in WASM exist and mostly work and the word mostly is doing a lot of work in that sentence. We log aggressively at the boundary and it is enough.

What I got out of it personally: I came to Momenti reading Rust and not writing it. I write it now. Not beautifully. But I can open the engine, find the thing, and fix the thing, and that was the goal I set in January.

The TypeScript player is in maintenance mode. It will be deleted in a quarter or two once the last few customers move. I will not miss it. I will miss the excuse it gave me to say "that's a web player bug" when it was really a format bug.

Next: the same engine on more platforms through FFI. Same core, another thin layer. That is the whole point.
