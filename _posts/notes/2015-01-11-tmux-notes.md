---
layout: article
title: "Tmux Notes"
date: 2015-02-12T19:42:04-0800
modified:
categories: notes
excerpt:
comments: true
tags: [tmux]
toc: true
ads: true
image:
  feature:
  teaser:
---

Tmux  commands and shortcuts

## Scoll


## Session commands

{% highlight bash %}
tmux ls # list tmux sessions
tmux attach-session -t [new-name] # attach to existing session
tmux rename-session [-t current-name] [new-name] # rename session
tmux detach -a # detach all attached session
{% endhighlight %}

The command `tmux detach -a` is the hot fix for small windowed tmux session. The tmux session always is fixed with the smallest tmux session window so detaching all the other sessions can be convenient when you want to resize session.

Keyboard short cut for rename
/bin/sh: 1: kkkkk: not found
<kbd>CTRL</kbd>+<kbd>b</kbd>, <kbd>$</kbd> [new-name]

Detach from current session

<kbd>CTRL</kbd>+<kbd>b</kbd>, <kbd>d</kbd>

New vertical split

<kbd>CTRL</kbd>+<kbd>b</kbd>, <kbd>%</kbd>

Horizontal split

<kbd>CTRL</kbd>+<kbd>b</kbd>, <kbd>"</kbd>


## New session inside tmux

{% highlight bash %}
TMUX= tmux new-session -d -s name
tmux switch-client -t name
{% endhighlight %}

## Move Panes

<kbd>CTRL</kbd>+<kbd>b</kbd>, <kbd>{</kbd>          move the current pane to the previous position
<kbd>CTRL</kbd>+<kbd>b</kbd>, <kbd>}</kbd>          move the current pane to the next position
<kbd>CTRL</kbd>+<kbd>b</kbd>, <kbd>o</kbd>          rotate window "up" (i.e. move all panes)
<kbd>CTRL</kbd>+<kbd>b</kbd>, <kbd>M-o              rotate window "down"
<kbd>CTRL</kbd>+<kbd>b</kbd>, <kbd>!</kbd>          move the current pane into a new separate window (break pane)
<kbd>CTRL</kbd>+<kbd>b</kbd>, <kbd>:move-pane -t :3.2</kbd>    split window 3's pane 2 and move the current pane there

## Cannot Connect to X server within TMUX

check your DISPLAY environment variable and set it to your current display

{% highlight bash %}
echo $DISPLAY
export DISPLAY=:0.0
{% endhighlight %}

If you want to set the display automatically, add the following command on the `~/.tmux.conf`.[^1]

{% highlight bash %}
set-environment -g DISPLAY $DISPLAY
{% endhighlight %}

Or use the following to retrieve environment variables. [^2]

{% highlight bash %}
set-option -ga update-environment ‘ YOUR_VAR’
{% endhighlight %}

## 256 color in tmux[^3]

Set an environment variaD`]ble `TERM` to `screen-256color`

{% highlight bash %}
export TERM=screen-256color
{% endhighlight %}

and in the `.tmux.conf`

{% highlight bash %}
set -g default-terminal “screen-256color”
{% endhighlight %}

### Reorder TMUX windows[^4]

To swap a window `a` with `b`, use

{% highlight bash %}
swap-window -s a -t b
{% endhighlight %}

To swap the current window with the 
[^1]: http://sourceforge.net/p/tmux/mailman/message/32267463/
[^2]: http://stackoverflow.com/questions/8645053/how-do-i-start-tmux-with-my-current-environment
[^3]: http://unix.stackexchange.com/questions/1045/getting-256-colors-to-work-in-tmux
[^4]: http://superuser.com/questions/343572/how-do-i-reorder-tmux-windows
