---
layout: article
title: "Tmux Notes"
date: 2015-01-11T05:27:32-08:00
modified:
categories: notes
comments: true
excerpt:
tags: [tmux]
ads: true
image:
  feature:
  teaser:
---

Tmux  commands and shortcuts


### Tmux session commands

~~~
tmux ls # list tmux sessions
tmux attach-session -t [new-name] # attach to existing session
tmux rename-session [-t current-name] [new-name] # rename session
tmux detach -a # detach all attached session
~~~

The command `tmux detach -a` is the hot fix for small windowed tmux session. The tmux session always is fixed with the smallest tmux session window so detaching all the other sessions can be convenient when you want to resize session.

Keyboard short cut for rename

<kbd>CTRL</kbd>+<kbd>b</kbd>, <kbd>$</kbd> [new-name]

Detach from current session

<kbd>CTRL</kbd>+<kbd>b</kbd>, <kbd>d</kbd>

New vertical split

<kbd>CTRL</kbd>+<kbd>b</kbd>, <kbd>%</kbd>

Horizontal split

<kbd>CTRL</kbd>+<kbd>b</kbd>, <kbd>"</kbd>

