---
layout    : post
title     : "A model that only answers yes or no"
author    : Dennis Lim
date      : 2026-09-21 02:30:00 +0900
categories: computer science
---

For the last few weeks I have been wiring a strange little model into everything I touch. It is called [Jev](https://docs.typesafe.ai), from TypeSafe, and it does not write text. You hand it some state and a typed question, and it hands back a probability. Yes or no. One of these options. Where on this scale. That is the whole API. It costs about four cents per million input tokens and answers in half a second.

I was skeptical. Then I put it in about sixty places across work and side projects in one long day, measured every one, and kept roughly half. Here is what I learned, in the order I learned it.

**It belongs between the code and the big model, not in place of either.** The pattern that survived every time was three layers. Code does parsing, counting, set operations, thresholds. The small model answers one semantic question with a closed answer: is this diff touching tax calculation, is this SQL irreversible, is this column a national ID in disguise. The big model writes prose only when someone needs prose. Every attempt that broke the layering failed. Every one.

**It works when the evidence is literally in the text and the answer is closed.** Diffs, error logs, short human-written notes, column names plus the shape of their values. On those it separates cleanly, usually above 0.9 or below 0.1. It fails on anything that needs two steps of reasoning, anything where the answer lives outside the input, anything shaped like counting, and anything that is really a matter of taste. When scores pile up between 0.5 and 0.7, the question is wrong. Lowering the threshold never fixed that. Narrowing the question sometimes did.

**Confidence measures agreement among the options you gave it, not truth.** My worst mistake: I asked why a game level was unbeatable, offered seven causes, and got "too many guards" at 92%. I nerfed the guards. Nothing changed. I had left the chase speed out of the state entirely. Add four numbers and the answer flipped to "chase is too strong" at 96%. It was not wrong either time. It picked the best story from what I gave it. When you write the state and you already have an opinion, a confidence of 1.00 is a warning, not a result. I now ask forward-looking questions twice with opposite framing and look at the gap.

**Thresholds are per axis, and the defaults are traps.** On one axis, noise sat under 0.04 and real signal spread from 0.12 up. A conventional 0.6 cutoff would have thrown away exactly the item I built the filter to catch. On another axis the same 0.6 was too loose. I stopped guessing. Print the whole distribution first, find the noise ceiling, put the line two or three times above it, and if you cannot find real positives in the data, make some and see where they land.

**The biggest payoff had nothing to do with the model.** To test it you need ground truth, and building ground truth means reading your own code and data harder than you ever do normally. That is how I found a comparison script whose regex had been matching zero lines for weeks, a payroll onboarding default that marked a tax-free allowance as taxable at hundreds of companies, and an audit job quietly shipping employee names and home IPs to an external API. The model found none of those. The discipline of measuring it did. I am careful now to say what actually fixed a thing, because "the model caught it" leads the next person to write an API call where three lines of code would do.

**The real cost is not money.** Sixty experiments came to under two dollars. The real costs were three near-misses in one day where personal data almost left the building inside the state, and six separate client wrappers with six different ideas about what to redact. One client, one redaction function, and never connect the output to anything irreversible. It ranks, gates, and audits. It does not decide.

I ran it on a side project too, an esports stats site where I score draft posts on six quality axes. The scores looked great and correlated with reader response at exactly zero. What predicted a reaction was the subject, not the sentence. The model was right about the sentences. I was wrong about what to ask.

Would I keep using Jev? Yes, more than before, and more narrowly. It is a cheap, honest, literal-minded second reader that never gets tired. You just have to remember which of you is holding the pen.
