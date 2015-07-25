---
layout: article
title: "Merging Pull Requests Locally"
date: 2015-07-13T21:15:55-0700
modified: 2015-07-14T11:58:53-0700
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

If you want to merge pull requests to your git repo, you can follow
this simple steps to merge the pull requests.

## 1. Adding pull requests to a local git

- Add the pull request branch into fetch. Open `.git/config` file [^1].

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


## 2. Fetch the pull request

I'll use pull request number 1739 for this demo post. If you want to try other
issues, please change the number 1739 to another issue number that you want to
merge [^2].

{% highlight bash %}
git fetch origin +refs/pull/1739/head:refs/remotes/origin/1739
git merge origin/1739
{% endhighlight %}

This might cause a merge conflict. Resolve using `git mergetool`.

[^1]: https://gist.github.com/piscisaureus/3342247
[^2]: http://stackoverflow.com/questions/6368987/how-do-i-fetch-only-one-branch-of-a-remote-git-repository
