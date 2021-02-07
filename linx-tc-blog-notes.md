---
date: 2020-10-08T17:40
tags:
  - linux
  - networking
  - tc
---

# Blog notes: linux TC

Linux TC wiki definition: [source](https://en.wikipedia.org/wiki/Tc_%28Linux)

> tc (traffic control) is the user-space utility program used to configure the
> Linux kernel packet scheduler. Tc is usually packaged as part of the
> iproute2 package.

The linux packet scheduler [source](https://en.wikipedia.org/wiki/Network_scheduler#Linux_kernel)

> The Linux kernel packet scheduler is an integral part of the Linux kernel's
> network stack and manages the transmit and receive ring buffers of all NICs,
> by working on the layer 2 of the OSI model and handling Ethernet frames, for
> example.

also on the wikipedia of network scheduling

> Berkeley Packet Filter filters can be attached to the packet scheduler's
> classifiers. The eBPF functionality brought by version 4.1 of the Linux
> kernel in 2015 extends the classic BPF programmable classifiers to eBPF.[21]
> These can be compiled using the LLVM eBPF backend and loaded into a running
> kernel using the tc utility.

These parts tell us that linux packet scheduler works on layer 2 and makes use of
the ring buffers of NICs. It also tells us a bit that we can use eBPF programs to
use as classifiers for linux tc.

The linux tc [man page](https://linux.die.net/man/8/tc) also some more insight
into what linux tc does. It says traffic control is composed out of the following
parts:

* shaping: this occurs on egress and modifies the bandwidth traffic can use.
  Under modification not only lowering the bandwidth is counted, it can also
  smooth traffic bursts.
* scheduling: Also something that happens on egress. Scheduling helps garantee
  certain bandwidth limits.
* policing: this happens on ingress. What shaping does for egress, policing does
  for ingress.
* dropping: if traffic exceeds bandwidth limits, it gets dropped (both on egress
  or ingress).

keep in mind, it does all of the above, purely on layer 2 information as that is
where the Linux packet scheduler works.

The man page then continues that to achieve the goals mentioned above, tc makes
use of 3 types of objects:

* qdiscs: which stands for queueing discipline. Whenever something sends a packet
  to an interface, it gets enqueued into a qdisc. The kernel then tries to get as
  many packets as possible from the qdisc for the interface to handle.
* classes: A subset of a qdisc. When a qdisc has classes, we call it a classful
  qdisc (contrary to classless qdiscs). Each class also has "inner" qdiscs
  associated with it. The different configuration of these classes and "inner"
  qdiscs allow us to dequeue a packet from one traffic flow before another (aka,
  prioritization of traffic).
* filters: Classful qdiscs use filter te determine in which class a packet will
  be enqueued. This is needed because when traffic arrive at a class with
  subclasses, we need to say to which subclass the traffic should go (a filter is
  one way of achieving this). When a packet arrive at a class, **all** filters of
  that class are called until one of them has a verdict. If no verdict comes,
  other criteria can be used.

The man page makes a note that filters reside within classes, and are not masters
of what happen.


There are 3 criteria on which TC decides what to do with the packet:

* tc filters: filters attached to classes (as explained earlier)
* type of service: some qdisc have rules to work on ToS fields of a packet
* `skb->priority`: Userspace programs can set a priority by using the
  `SO_PRIORITY` option

Not all qdiscs will use all 3 criteria, and when no classification is done, the
packet gets send to the leaf qdisc attached to the class.

All objects have IDs (either manually specified or automagically generated). IDs
exist of a `major` and `minor` part seperated by `:`.

Qdiscs generally get assigned a `major:` id, which leaves te `minor` part
available for classes. Classes have a `major:minor` id, the `major` part
referring to the qdisc that holds the class and `minor` part for the classid.
Filters have a 3 part ID (which isn't used commonly).

There are 3 major qdiscs/hooks to which we can attach objects:

* `root`: the normal egress traffic.
* `ingress`: incoming traffic, so filters can be attached to it.
* `clsact`: bot incoming and outgoing traffic. Used to attach classifiers to all
  traffic of the stack.

# sources

* [FOSDEM talk 2016](https://archive.fosdem.org/2016/schedule/event/ebpf/attachments/slides/1159/export/events/attachments/ebpf/slides/1159/ebpf.pdf)
* [tc manual](https://linux.die.net/man/8/tc) 
* [tc-filters](http://linux-ip.net/gl/tc-filters/tc-filters.html)
* [tc howto](https://tldp.org/HOWTO/Traffic-Control-HOWTO/index.html)
