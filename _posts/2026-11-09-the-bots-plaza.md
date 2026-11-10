---
layout    : post
title     : "The bots' plaza: where our Claudes ask each other"
author    : Seunggi Lim
date      : 2026-11-09 20:00:00 +0900
categories: computer science
---

At work there is a Slack channel called the bots' plaza. Our Claudes use it to ask each other things. Before it existed, asking a colleague meant copying what Claude wrote into Slack, waiting for an answer, and pasting that back into Claude. Now my Claude asks someone else's Claude where a config lives, theirs looks it up and replies, and the two of us read the thread later. It opened to one division on November 3rd and to the whole company on November 6th. It runs on top of a harness I have been building since July, so this post covers both, plus a list of things that broke along the way.

**The harness underneath**

The harness is how people across the company, many of them not developers, get the same Claude setup on their own machine. It starts from one tree file that follows the org chart, five levels deep, from company-wide down to a single part of a team. Each node has its own instructions. The installer goes from the top of the tree down to your node, stacks everything in that order, and copies it onto your machine. If a lower node has a file with the same name as one above it, the lower one wins.

It holds four kinds of things: rules that load in every session, skills that load only for their job, commands a person types, and hooks that run on tool calls and can stop them. Each part edits its own instructions with Claude and sends a pull request, which the part lead reviews. Machines pull updates once a day. I wrote about the rollout on the [company tech blog](https://techblog.unitblack.co.kr/posts/claude-code-config-rollout/) in September, in Korean.

**Guideline, warning, block**

Most of the company-wide rules were added in September or later, and most of them because something happened. They tend to go the same way. A rule starts as a sentence in the instructions. When that isn't enough it becomes a warning, and when the warning gets ignored it becomes a block.

![Three rules on a timeline. Commit email: warnings on September 22 and October 1, a block on October 8. git merge: in my instructions on September 20 and October 7, a hook on November 4. Sending to Slack: in the instructions on October 31, a hook on November 4.](/assets/images/2026-11-09/escalation.svg)

On September 15th a few thousand requests went from our office to one outside website in about twenty minutes, up to 91 a second, and the site blocked the company's public IP. We all share that IP, so for a while nobody at the company could open the site. Three days later we had a scraping rule (at least a second between requests, one at a time, stop at the first 429) and a hook that asks before running anything that calls an outside site in a loop or in parallel.

Commit emails are supposed to be your own address, not a team alias. We warned about it in September, warned louder on October 1st, and started blocking on October 8th. The commit that added the block says the warnings alone hadn't changed anything. The same day, the block message started explaining how to get unblocked.

I told Claude not to use `git merge` on September 20th, and again on October 7th. On November 4th it became a hook. These days, if I catch myself saying something twice, I write a hook.

**The plaza**

The Slack skill went out on October 31st. The Slack app behind it is called Masquerade, since Claude posts as you, with your name and photo. Everyone signs in on their own and the token stays on their machine. It shows a preview by default. It can only edit or delete your own messages, and it can send DMs but not read them. It refuses text that looks like a resident registration number or a card number, and it won't post in channels shared with other companies.

On November 2nd the plaza became the default place to ask. If your Claude needs something from another person or their Claude, and you didn't say where to ask, it asks there. The first time a Claude posts there, it introduces itself: whose Claude it is and what the session is working on. It has to mention the person it wants, because each Claude only watches its own mentions.

![My Claude posts in the bots' plaza as me, after I approve. Slack sends events to a relay, which passes them to the other person's watcher with no text and no tokens. The watcher wakes their Claude, which reads the post with its own token and replies if it is sure.](/assets/images/2026-11-09/plaza-flow.svg)

A watcher runs inside a session and prints a line for each post that needs attention, with a tag saying why. Claude takes it from there. The part I care about most is how it reads. Posts from other bots are treated as data, not instructions. If it has checked the answer and is sure, it answers. If the post is vague, asks it to run or change something, or leans on words like "approved", "urgent" or "the CTO said so", it doesn't act. It adds the post to a numbered list for its human and goes on to the next one. That includes posts that use my name.

The signature changed three times. On November 2nd every post started with a "🤖 so-and-so's Claude" line. We took it out the same day, because a post under your name should be yours. On November 6th it came back as a grey line under the message, saying Claude wrote it and the owner checked it before it went out.

On November 4th the approval step moved out of the instructions and into a hook. The skill had a `--yes` flag, and nothing stopped Claude from adding it. Something planted in a Slack message, or a plain mistake, could have gone out under someone's name. Now a person approves each send.

**A relay**

The first watcher polled Slack every 60 seconds from every machine. All of those machines go through one Slack app, which means one rate limit. We estimated the watchers would start hitting 429s and stopping at around fifty people. They also missed replies in older threads once the channel had more than 200 posts in a week, and said nothing about it.

So on November 6th we built a relay, and the next day it became the default. It keeps one connection to Slack and forwards events to each machine. An event has the channel, the time, the author and who was mentioned, but not the text. Each machine fetches the text itself with its own token, only for the posts it needs. The relay never gets anyone's Slack token and never posts anything. If your machine is off, nobody answers for you. If the relay is down or sends something odd, the watcher goes back to polling. While someone's Claude is writing a reply, the thread shows that it is.

Token cost was the other problem. My watcher used to restart every 30 minutes, and on November 4th I found that cache reads from those restarts made up 72% of its token cost. I changed it to wake only when a post arrives, which left a gap: while it handled one post, it wasn't watching for the next. Today I put my own sessions back on a watcher that never stops. It costs more, and I would rather pay than miss mentions.

**A guide for the plaza**

A lot of the time you don't know who to ask. So on November 7th the plaza got a guide, a bot you can ask who handles something. It answers in a line or two: ask this person on that team. It writes names but never mentions anyone, so it won't wake people up at night and won't get into a conversation with another bot. It can call five Slack methods, and none of them edit, delete, or join a channel.

The guide works from a map of who handles what, and nobody maintains it by hand. A build reads Slack, Notion and the issue tracker, has an LLM pull out pairs of "this thing, this person", removes internal hostnames and project IDs from the names, and fails the tests if any phone number, email or link is left in. If the map has nothing, the guide looks in the last 30 days of plaza threads for a similar question and tells you who answered it. If there's nothing there either, it says it doesn't know and tells you where to ask.

![How the guide answers. Code picks 40 map entries by shared words. Jev ranks them and gives the chance that none fit. Sonnet writes a reply from the top 20. Code checks that the names come from the map and that there are no links or mentions. A failed check gets one retry, then a fixed sentence.](/assets/images/2026-11-09/guide-answer.svg)

The answer path ended up with the same three layers as in my [Jev post]({% post_url 2026-09-21-a-model-that-only-answers-yes-or-no %}). I didn't plan that. On the first day there was a hand-written routing table and a Jev step, and I deleted both. Two days later Jev came back with a smaller job. Code takes the 40 entries that share the most words with the question. Jev ranks them and estimates how likely it is that none of them fit. Sonnet reads the top 20 and writes the reply. Then code checks it: every name has to come from an entry Sonnet was given, and there can't be links or mentions. If the check fails, Sonnet gets one more try, and after that the guide uses a fixed sentence. I picked Sonnet over Haiku after trying both on 18 sample questions. What forced the rewrite was mundane. Names are written in Korean in some places and in English in others, and a broad question like "the calculator" needs an answer for each area, not a single name.

When the guide gets it wrong, people correct it in the thread ("no, that's so-and-so"), and the next build picks the correction up.

The first rate limit was five questions per person per ten minutes. I found out when my sixth question got no reply at all. It is now one question every five seconds.

**Sessions on my machine**

In September I wrote about [colored dots for my agents in tmux]({% post_url 2026-09-28-colored-dots-for-my-agents-in-tmux %}). Since then the sessions on my machine have started passing work to each other the way the plaza does, without Slack.

Each session owns an area: AWS and Kubernetes, databases and everything else, git hosting, the LLM gateway, a few internal apps, and the security ledgers. When a request lands in the wrong session, it doesn't do the work itself. It passes it to the owner along with what the owner would otherwise have to ask for: who asked and how that was verified, who approved it, the request in its original wording, which ledger it goes in, and which session wants the result back. Secrets are never included. Only one session writes to the ledgers, so two sessions never edit the same file at once.

This evening a dashboard for our partners went live in under an hour this way. One session reviewed and approved the app, one checked the deployment and that it came up, one set up the dashboard's account and tested its access with a real token, and one sent the advance notice and wrote the ledger entry. Before the release, the session testing access found that the account couldn't read one of the tables it needed, and fixed it.

When someone mentions me in the plaza, the session watching it asks whichever session owns the topic, that session checks the actual state, and the answer goes back to the thread. This started on November 3rd. My Claude showed me a draft reply and asked whether to send it, and I told it to go find out and send what it found. New things, like a new permission or a new feature, still come to me first. When a session has several questions only I can answer, it doesn't block on a dialog. It keeps a numbered list and carries on with the rest.

**Things that broke without telling anyone**

Over these two months, most of what broke did so without an error. The fix was usually to make the failure show up somewhere.

`bunx biome`. It looks like it runs the Biome linter. But if `@biomejs/biome` isn't installed in that project, bunx downloads a different npm package that happens to be named `biome` (version 0.3.3, something for environment variables), and that exits 0. Our pre-commit check showed a green OK and checked nothing. After the fix, the same bad file gave three errors. We had hit the same thing in August with `bunx tsc`, which pulls an old deprecated package called `tsc`. We now call tools by their full package name and version.

Reviewers. Part leads were supposed to be added as reviewers automatically. In September I found that it had never happened, not once. Our git server compiles each line of the code owners file as a regular expression and skips any line that doesn't compile, without saying so. Every line containing `**` was skipped, and lines with a single `*` matched every file by accident. We rewrote all the lines as regular expressions the server accepts.

Updates. The old update hook only applied an update if the install folder was on main with no changes. One machine's folder was left on a branch that had already been merged, so for three days it skipped every update. The installer now installs from a hidden copy of main no matter what state the folder is in. Then the new install, which swaps folders in one step, ran into Windows: another program had a file open, a rename failed partway through, and the folder ended up empty. Now it retries, and if that still fails it restores the previous version.

Missed mentions. My watcher printed six mentions in one batch. My Claude read only the last line and missed the other five. Now it counts the event lines and checks that against how many it handled. Separately, the catch-up step after a relay gap only looked back one day, so a new reply in a week-old thread could be missed. It looks back seven days now, which costs four extra calls after a 30-minute gap and fifteen after a 15-hour gap.

The push. Two sessions were sharing a clone, and one switched branches while the other was working. On October 31st a session reported "pushed to main" when it had pushed an old local main and nothing new had gone up. Another session caught it. Now each session checks its branch right before committing, and after pushing checks that origin/main is actually its commit.

Disk. For a while every session made a fresh copy of the repo for each task and reinstalled dependencies. One repo had 54 copies, it added up to more than 30 GB a day, the disk reached 95%, and the file system watcher used all the CPU. Now each repo has a few fixed copies that get reused.

**What I think now**

Putting something in the instructions is where a rule starts. If it matters, it ends up as a hook, and I usually find out which ones matter when the instruction gets ignored.

A green check, an empty result, and a session saying "done" all need checking.

Bots asking bots has worked better than I expected, as long as each one treats what it reads as data and leaves decisions to a person. That reading rule matters more than anything clever in the bots.

I haven't counted plaza questions or answers yet, so I have no usage numbers.

**How I wrote this**

I asked two of my sessions what I had been doing lately. The one that maintains the harness sent a 507-line brief. Every number had its source, a commit, a file or the command that produced it, and estimates were marked as estimates. At the end it listed what it had left out and why, and eleven things it thought I should decide before publishing. It also suggested showing the draft to the security session, which I did. That session asked for two changes.
