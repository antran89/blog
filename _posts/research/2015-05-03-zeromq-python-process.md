---
layout: article
title: "Graceful Exit for Python multiprocessing Processes with Infinite Loop and a ZMQ Server Client Example"
date: 2015-05-03T14:23:45-0700
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
However, if we have an underlying `while` loop in the `run()` function, it is
difficult to end such loop.

## Graceful Exit for Infinite Loop of Python Process

To simplify joining, I created a class `ClientProcess` that inherits
`multiprocessing.Process` and added a member variable called `joined` which is
of type `multiprocessing.Value(..., lock=False)`. I did not use lock since it
would slow down by introducing a mutex.

{% highlight python %}
import time
from multiprocessing import Process, Value


class ClientProcess(Process):
    def __init__(self, epoch):
        Process.__init__(self)
        # Lock free value speed comparison
        # http://stackoverflow.com/questions/26258728/
        self.joined = Value('b', False, lock=False)
        self.epoch = Value('i', epoch, lock=False)

    def join(self):
        self.joined.value = True
        super(ClientProcess, self).join()

    def run(self):
        while not self.joined.value:
            self.epoch.value += 1
            time.sleep(0.1)

# Main functions
proc = ClientProcess(1)

proc.start()

print "Join"

for _ in xrange(10):
    time.sleep(0.2)
    print proc.epoch.value

proc.join()
{% endhighlight %}

The process simply increments the value `epoch` every 100ms and we just print
the value every 200ms. After we finish, we join the process to gracefully exit
the `ClientProcess`.


## ZMQ inter process communication

As a toy application for such joinable process, I used ZMQ (Zero Message Queue)
to simulate server-client setting. `ClientProcess` simply sends messages and
`ServerProcess` prints the received messages.

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
