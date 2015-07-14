---
layout: article
title: "Mergin Pull Request Locally"
date: 2015-07-13T21:15:55-0700
modified:
categories: research
excerpt:
comments: true
tags:
toc: true
ads: true
image:
  feature:
  teaser:
---

If you want to merge the recent pull request to your Caffe file, you can follow
this simple steps to merge the pull requests.

## Adding pull requests to a local git

- Add pull request into fetch. Open `.git/config` file [^1].

{% highlight bash %}
[remote "origin"]
  fetch = +refs/heads/*:refs/remotes/origin/*
  url = https://github.com/BVLC/caffe.git
{% endhighlight %}

Add the line `fetch = +refs/pull/*/head:refs/remotes/origin/pr/*`

{% highlight bash %}
[remote "origin"]
  fetch = +refs/heads/*:refs/remotes/origin/*
  url = https://github.com/BVLC/caffe.git
  fetch = +refs/pull/*/head:refs/remotes/origin/pr/*
{% endhighlight %}


## Fetch the pull request[^2]

I'll use pull request number 1739 for this demo post. If you want to try other
issues, please change 1739 to other issue number that you want to try.

{% highlight bash %}
git fetch origin +refs/pull/1739/head:refs/remotes/origin/1739
git merge origin/1739
{% endhighlight %}

This this will cause a merge conflict. Resolve using `git mergetool`.

[^1]: https://gist.github.com/piscisaureus/3342247
[^2]: http://stackoverflow.com/questions/6368987/how-do-i-fetch-only-one-branch-of-a-remote-git-repository
