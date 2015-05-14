---
layout: article
title: "Killing Multiple Processes"
date: 2015-05-12T21:47:55-0700
modified:
categories: notes
comments: true
excerpt:
tags: [linux]
ads: true
image:
  feature:
  teaser:
---

In Linux, you might get into a situation where you start a lot of processes.
If you want to kill them with a specific process name automatically, you can
use following technique [^1].

`ps | grep specialstring | cut -c 1-5 | xargs kill`

- `ps` will give you a process id, name
- `grep` will select a process line with a special string (or a regular expression)
- `cut` will select `1`st to `5`th characters
- `xargs` will pipe a line as an argument to the following command
- `kill` will kill the process using the given argument as the pid

[^1]: <http://blog.stackoverflow.com/2014/09/introducing-runnable-javascript-css-and-html-code-snippets/>

