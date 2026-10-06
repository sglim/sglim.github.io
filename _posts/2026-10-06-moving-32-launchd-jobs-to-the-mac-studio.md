---
layout    : post
title     : "Moving 32 launchd jobs to the Mac Studio"
author    : Seunggi Lim
date      : 2026-10-06 13:00:00 +0900
categories: computer science
---

I moved [everystat](https://everystat.ai) to the [Mac Studio]({% post_url 2026-09-27-a-mac-studio-in-one-day-with-nix %}) last week. It is a LoL esports stats site, and behind it is a set of launchd jobs that pull match data, deploy the site, and run a few bots. They all ran on the old M1 MacBook. The switch took about four minutes. The M1's scheduled jobs wrote their last logs at 16:22, and the plists landed on the Studio at 16:26. The days around it took longer.

In the Mac Studio post I wrote that migration day is mostly one flag. For this project it wasn't. I took everystat out of the nix config, so the same job could never be started from two places, and moved it with a small tool of mine that turns the plists in the repo into templates and loads them.

**Before the switch**

The new Mac has a different user name, so every hardcoded home path was a bug. 342 files in my repos had the old path. 187 of them were code that actually runs, and I fixed 146 of those. The rest were plists and config formats that can't expand a variable. Logs and notes I left alone.

I grouped the jobs. Things that always run one after another now sit behind one runner, and 47 plists became 31.

And I found a plist I had installed by hand in September and never committed. A tool that reads the repo would have left it behind. When I moved from my previous Mac to the M1 in August, two plists were missing for the same reason. Same hole, twice. It is in the repo now, so 32 jobs.

**What broke anyway**

launchd does not expand `$HOME` or `~`. I knew that, and it got me twice. My `.envrc` loader passed `$HOME` along as plain text. My shell was fine because direnv expands it, and only the launchd jobs got a path that did not exist. Then the template tool wrote `~/` into environment variables, but the runner only expanded the arguments. The job that posts match results to X failed from the move until I fixed it the next morning.

Redis didn't move the way I planned. The plan was to copy the dump file. The M1 runs Redis 8.10 and the Studio 8.8, and the older one can't read the newer file. I wrote a short script that copies the keys by type as JSON, 306 of them, and skipped the streams, which were only old notifications.

The translation code is generated at build time and is in `.gitignore`, so a fresh clone did not have it. The first job on the Studio failed. The next run, 48 seconds later, passed.

The biggest find was older than the move. The bot's automatic deploy had not succeeded once since the August move. Two symlinks pointed outside the Docker build context, so every build died, and the site was only being updated by my manual deploys. I fixed it the day after the move.

**Now**

The jobs run on the Studio, and I still commit from the M1 sometimes, so a sync job keeps `main` in step between the two Macs. The M1 keeps what is plugged into it: the Zigbee dongle, the wallpad bridge, the Stream Deck.

Six days later, there are 33 everystat jobs on the Studio and none on the M1.
