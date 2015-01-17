---
layout: infinite-post-article
title: "All Posts"
date: 2014-05-30T11:39:03-04:00
modified:
excerpt: 
tags: []
image:
  feature:
  teaser:
---
<div id="main" role="main">
	<article class="wrap" itemscope itemtype="http://schema.org/Article">
        <div class="inner-wrap" >
			<nav class="toc"></nav><!-- /.toc -->
		    <div id="content" id="infinite-add" class="page-content" itemprop="articleBody">
            {% for post in site.posts limit:10 %}
              <div id="content-only">
               	<div class="page-title">
               		<h1>{{ post.title }}</h1>
               	</div>
                   {{ post.content }}
              </div>
            {% endfor %}
            </div>
        </div>
    </article>
</div>
