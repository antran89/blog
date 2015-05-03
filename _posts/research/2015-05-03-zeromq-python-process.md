---
layout: article
title: "ZeroMQ and Python Joinable Processes"
date: 2015-05-03T01:22:20-0700
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

After creating a python process, we have to clean it up by joining the process.
However, if we have an underlying while loop in the `run()` function, it is
difficult to end such loop.

To simplify joining, I inherited `multiprocessing.Process` and added a member
variable called `joined` which is of type `multiprocessing.Value(...,
lock=False)`. I did not use lock since it would slow down by introducing a
mutex.

`ClientProcess` simply send messages and `ServerProcess` prints the received
messages.

{% highlight python %}
import sys
import time
import zmq
from multiprocessing import Process, Value


class ClientProcess(Process):
    def __init__(self, context, end_point, client_id):
        Process.__init__(self)
        self.joined = Value('b', False, lock=False)
        self.context = context
        self.client_id = client_id
        self.end_point = end_point

    def join(self):
        self.joined.value = True
        super(ClientProcess, self).join()

    def run(self):
        socket = self.context.socket(zmq.PUSH)
        socket.connect("ipc://%s" % self.end_point)
        while not self.joined.value:
            socket.send("a msg from id %d" % self.client_id)
            time.sleep(0.8)


class ServerProcess(Process):
    def __init__(self, context, end_point):
        Process.__init__(self)
        self.joined = Value('b', False, lock=False)
        self.context = context
        self.end_point = end_point
        # Socket cannot be initialized during the initialization stage

    def join(self):
        self.joined.value = True
        super(ServerProcess, self).join()

    def run(self):
        # Socket cannot be initialized during the initialization stage
        socket = self.context.socket(zmq.PULL)
        socket.bind("ipc://%s" % self.end_point)
        poller = zmq.Poller()
        poller.register(socket, zmq.POLLIN)

        while not self.joined.value:
            socks = dict(poller.poll(1000))
            if socks:
                a = socket.recv()
                sys.stdout.write("server got %s\n" % a)
                sys.stdout.flush()

# Main functions
end_point = "/tmp/feeds"
server = ServerProcess(zmq.Context(), end_point)
proc1 = ClientProcess(zmq.Context(), end_point, 1)
proc2 = ClientProcess(zmq.Context(), end_point, 2)

server.start()
proc1.start()
proc2.start()

time.sleep(1)

print "Join"

proc1.join()
proc2.join()
server.join()
{% endhighlight %}
