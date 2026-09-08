---
layout    : post
title     : "Josa: a tiny Dart library for Korean particles"
author    : Dennis Lim
date      : 2019-05-10 21:40:00 +0900
categories: computer science
---

I published a Dart package this week. It is very small. It does one thing. I want to write about it because the one thing turns out to be a nice example of a problem that looks trivial and is not.

Korean has particles that attach to the end of nouns. The particle changes depending on whether the noun ends in a consonant or a vowel. "은" after a consonant, "는" after a vowel. Same for "이/가", "을/를", "과/와" and a few others. Every Korean speaker does this without thinking. Every app that puts a user's name into a sentence gets it wrong.

You have seen this. "철수는 ..." looks fine. "지민는 ..." looks like the app is broken. Because it is. The app has a template with a fixed particle and it does not know that 지민 ends in a consonant.

The fix is not hard once you know Hangul is systematic. Every syllable is a Unicode code point in a block, and the code point encodes the initial consonant, the vowel, and the optional final consonant. Subtract the base, mod by 28, and if the result is not zero there is a final consonant. That is the whole algorithm. Ten lines.

So why a library?

Because the ten lines have edge cases. Names that end in a number, where the particle depends on how you pronounce the number. Names that end in an English letter. Names that end in "ㄹ" which is special for one particle. Mixed strings where the last character is a closing bracket and you need to look past it. I did not know most of these until I wrote tests and a coworker came by and said "what about this one".

And because we needed it in the Flutter app. We show sentences with user data in them. Three different screens had three different half correct implementations. Now there is one, it is tested, and it is on pub.dev where the next Dart developer can find it instead of writing the fourth one.

Some notes on publishing a Dart package for the first time:

- The tooling is good. pub publish does the right thing and complains about the right things.
- Write the README as if the reader does not speak Korean. Half the people who will hit this problem are building for Korean users without being Korean.
- Version it like you mean it. I published 0.1.0 because I do not trust it yet. That is honest.

It took an evening. It will save me an hour a year for the rest of my life and maybe save someone else an embarrassing screenshot. Good trade.

The package is called josa, which is just the Korean word for particle. I am not good at names.
