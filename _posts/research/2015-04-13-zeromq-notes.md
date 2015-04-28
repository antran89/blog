---
layout: article
title: "ZeroMQ Notes"
date: 2015-04-13T15:01:06-0700
modified:
categories: research
excerpt:
comments: true
mathjax: true
tags:
toc: true
ads: true
image:
  feature:
  teaser:
---


Zeromq notes


### Bind vs Connect

There are two types of socket connections: bind and connect. with bind, you do
not know that how many peers will join in advance and you cannot create queues.
Instead queues are created as peers connect to the bound socket.

With connect, you assume that there is at least one peer and it creates a
single queue immediately.

For instance, for multiple publishers and one subscriber, you can set the fixed
point to be the subscriber and set  using this setting.

**Client**

{% highlight cpp %}
//  Prepare our context and publisher
zmq::context_t context (1);
zmq::socket_t subscriber (context, ZMQ_PUB);
publisher.bind("tcp://localhost:5555");
{% endhighlight cpp %}


**Server**

{% highlight cpp %}
//  Prepare our context and publisher
zmq::context_t context (1);
zmq::socket_t publisher (context, ZMQ_PUB);
publisher.connect("tcp://localhost:5555");
{% endhighlight cpp %}


### Pitfall of ZMQ variable destruction

If you created a class that contains `zmq::context_t` and `zmq::socket_t` as
member variables, you should beware of the class destruction order. Since the
socket will go defunct if the corresponding context is destroyed first. To
prevent such case, you must list `socket_t` later than `context_t` declaration.
For instance,

{% highlight cpp %}
class Test{
  ...
  zmq::context_t context;
  zmq::socket_t socket;
  ...
}
{% endhighlight %}

Class destructor will destroy the variables in the reverse order of
construction [^1].

### Inproc Communication

When you use multiple threads and want to communicate between threads,
`inproc://*` might be the best communication protocol. When you use the
protocol, make sure to use the same `zmq::context_t` for all sockets that you
want to communicate.

### Missing Message Problem Solver

When you experience the missing message problem. Visit the following url and
follow the diagram.
<http://zguide.zeromq.org/page:all#Missing-Message-Problem-Solver>

[^1]: http://stackoverflow.com/questions/14688285/c-local-variable-destruction-order
