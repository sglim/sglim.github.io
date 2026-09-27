---
layout    : post
title     : "A Mac Studio in one day, with nix"
author    : Dennis Lim
date      : 2026-09-27 20:30:00 +0900
categories: computer science
---

I bought a Mac Studio this week. M5 Max, sitting on a shelf at home with no monitor. Its job is to run my side-project bots and take the heavy builds off the old M1 that has been doing everything. I set it up today, from zero, and I want to write down how, because most of what I learned came from things that broke on the M1 first.

The rule I set before unboxing: nothing gets installed by hand. Every tool, every service, every macOS setting lives in one git repo of nix files. The M1 already worked that way for my user account with home-manager. For the new machine I added nix-darwin, which manages the whole system, not just my home folder. Both configs live in the same flake. The M1 keeps its old setup and the Studio gets the new one, side by side.

Setup was two manual steps. Install Xcode's command line tools, clone the repo. Then one bootstrap script that installs Homebrew, installs Nix, applies the config, and switches the login shell. It is safe to run again, so when it stopped halfway I just ran it again. Five config generations later, by dinner, the machine was mine.

A few things I did differently this time, and why.

**The login shell.** On the M1 my login shell was fish, from the nix profile. Twice, an update changed that path out from under me and SSH stopped working. Both times I had to walk to the machine. The workaround there is a plain /bin/zsh login shell that hands over to fish only for interactive SSH. On the Studio, nix-darwin puts fish at a per-user path that stays the same across updates, so the shell is fish again and I sleep fine.

**Files nix takes over.** When home-manager starts managing a dotfile, it moves the old one aside and does not carry anything over. On the M1 that silently deleted the one line in .zshenv that put nix on the PATH, and zsh could not find a single nix binary. Now I check the moved-aside files after every first apply.

**Use Apple's ssh.** macOS has a local network privacy check. A non-Apple-signed binary talking to a private address gets "No route to host". Nix's openssh failed exactly like that, while /usr/bin/ssh to the same address worked. The nix path changes on every update, so granting permission never sticks. I just use the system ssh on Macs.

**tmux from a tiny app.** My tmux server has to start on the logged-in desktop side. Started over SSH, it can't unlock the keychain. Started from a launchd shell script, it can't get file permissions. So there is a fifty-line C program, wrapped as an app, that launchd keeps alive and that starts tmux if it is not running. macOS grants permissions to that app once and everything inside tmux inherits them. Each tmux tab also shows a small colored dot for every coding agent running in it: waiting, working, needs me, done. It sounds silly. I look at it all day.

**The hostname is the identity.** darwin-rebuild picks which config to apply by the machine's local hostname. macOS renames a machine when it sees a name collision on the network. My laptop went from one numbered name to another in a week. So the config pins the hostname, and that one line is what makes the Studio know which machine it is.

**Services, declared but off.** The bots are declared in nix as launchd agents, with a flag that keeps them off. That let me apply the full config today without two machines running the same jobs. A generator script rewrote their Homebrew paths to the nix profile, so the services do not have Homebrew on their PATH at all. Migration day is one flag.

**A rule for tools.** The M1's nix store grew to 41GB because nothing ever cleaned it. The Studio collects garbage weekly from day one. I also counted which command line tools I actually ran in the last 90 days, from shell history and the agent's command logs. Anything used twenty times or more is installed. Everything else I get with `nix shell` when I need it, and the weekly cleanup takes it away.

**What nix can't decide.** The config says never sleep and restart after a power failure. But FileVault is on, and after a power cut the Mac waits at the disk unlock screen, where SSH can't reach it. No setting fixes that. It is a choice between security and uptime, and I have not made it yet.

The machines talk over Tailscale now, named m1, m3, m5. When I need a file from one of them, a small script asks both and pulls the newer copy.

A new Mac used to cost me a weekend of clicking. This one cost a day, and most of that day was spent writing down what I learned. The next one should take an hour.
