---
layout    : post
title     : "Colored dots for my agents, in plain tmux"
author    : Seunggi Lim
date      : 2026-09-28 21:00:00 +0900
categories: computer science
---

I keep a lot of Claude Code sessions open on my home server. On Saturday I counted eighteen, all inside tmux. Some were working, some were waiting for me, and some were done. In the tab bar they all looked the same. Nothing told me which one needed me.

So I went looking for a multiplexer that knows what an agent is.

**herdr**

herdr is a terminal multiplexer built for coding agents. It is a single Rust binary. It reads each window's screen, matches it against rules, and tells you if the agent there is working, blocked on you, done, or idle. It also has a JSON CLI and a socket API, so one agent can open windows and read the state of the others. That part is really good.

I tried it on the M1 home server with three Claude sessions in three windows. It detected all three, and in my short test it was right every time.

Then the small problems started to pile up.

- It is 0.x. It ships every one to three weeks, and compatibility has broken several times.
- In nixpkgs it only exists in unstable. My config is pinned to a stable release.
- Two of the three sessions were started with a separate Claude config directory. After a server restart, herdr brought them back with their conversations but with the default config. I turned automatic restore off.
- An agent running in tmux inside a herdr window is invisible to herdr.
- If you start the herdr server from inside a Claude session, environment variables leak into its windows, and the Claude sessions in there stop saving their conversations.
- There are open macOS issues: disconnects after sleep, a client that freezes, launchd restarting it forever.

So I kept tmux. herdr puts a dot for each agent next to the window name. I copied that idea into tmux. tmux can draw it. Somebody just has to tell it the state.

**Hooks write, tmux draws**

Claude Code has hooks. They run a command on events like "prompt submitted", "about to use a tool", "needs permission", "stopped". Mine all call one small shell script, `claude-tmux-state`, with a state name. They run async, so Claude never waits for them.

![Claude Code hooks call claude-tmux-state, which writes a tmux pane option. tmux draws it in three places.](/assets/images/2026-09-28/claude-tmux-flow.svg)

The script finds its own pane from `$TMUX_PANE` and writes the state into a pane option:

```sh
[ -n "$TMUX_PANE" ] || exit 0
tmux set -p -t "$TMUX_PANE" @claude_state "$state"
```

The rest is tmux format strings. The window tab loops over its panes with `#{P:...}` and draws a dot for every pane that has a state. The pane's title line gets a colored strip. When a pane is done or waiting for me, the script also tints its background, so I can spot it in a split without reading anything.

![Which hook event sets which state, and how the tab and the pane title look for it.](/assets/images/2026-09-28/claude-tmux-states.svg)

Filled green needed some care. "Done" should stay lit until I actually see it. So tmux has its own hooks: when I switch windows, switch sessions, or select a pane, tmux calls the script with `seen`, and that pane goes from done to idle. It works per pane, not per window. And if a session finishes while I'm already looking at it, it skips done and goes straight to idle.

**Things I ran into**

*Esc doesn't fire Stop.* If I interrupt Claude with Esc, the Stop hook never comes, so the pane stays yellow. I reproduced it in a throwaway session. The idle notification didn't show up within 100 seconds either. I could fix it by reading the progress line off the screen, but then my dots depend on Claude's UI text, and I didn't want that. The next prompt clears it anyway.

*Questions in bypass mode stayed yellow.* With permissions bypassed, a multiple choice question from Claude doesn't send a permission request. It only sends PreToolUse. So now the script checks the tool name. If it is `AskUserQuestion`, that counts as blocked.

*set-hook without an index wipes the array.* tmux keeps hooks in an array, and setting one without an index clears the rest. So my hooks use a fixed index, `[42]`. Another config that sets a hook later without an index can still wipe `[42]`, so these lines come after the plugins load, near the end of the config.

*The keychain.* For Claude running under a tmux server started over SSH, the macOS keychain is locked. So tmux gets started by a tiny app bundle that launchd keeps alive on the logged-in desktop side. I wrote about it in [the Mac Studio post]({% post_url 2026-09-27-a-mac-studio-in-one-day-with-nix %}). A small bonus this time: home-manager wraps launchd jobs in /bin/sh, so the list of background items showed it as "sh". Setting `AssociatedBundleIdentifiers` makes it show up under the app's own name.

![A mock of the tab bar and three panes: a finished pane with a green strip and tinted background, an idle pane, and a plain shell.](/assets/images/2026-09-28/claude-tmux-mock.svg)

All of this lives in my dot-files: the hook script, the format strings, the launcher, and the step that registers the hooks. On a new Mac, once Claude Code is installed, one apply and the screen looks the same.

Eighteen sessions still look like eighteen sessions. Now I just know which one to open.
