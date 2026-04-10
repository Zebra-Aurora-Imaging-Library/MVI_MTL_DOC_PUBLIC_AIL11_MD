---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distributed_AIL
section: Best_practice
module_tag: app
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / distributed-ail / Best practice"
---

# Best practice

When developing a Distributed Aurora Imaging Library application, you should be aware of some network limitations and adhere to some good practices to develop the most efficient application.

With regards to network usage, you should be aware of the following points:

- Even if high bandwidth is available with Gigabit Ethernet, latency is inherent to network communication.
- Latency can add up quickly when multiple synchronous successive calls are made.
- Saturation of the network link increases latency.
- Collisions on the network link increase latency.

To implement an efficient Distributed Aurora Imaging Library application, there are a few good practices that you should adhere to:

- Use a dedicated subnet for remote computers to minimize interference with other network traffic.
- Do not force the thread ([`MthrControl`](../../Reference/thr/MthrControl.md)) or system ([`MsysControl`](../../Reference/sys/MsysControl.md)) to be synchronous ([`M_THREAD_MODE`](../../Reference/thr/MthrControl.md) set to [`M_SYNCHRONOUS`](../../Reference/thr/MthrControl.md)).
- Use remote files as much as possible. Remote files can also be on a file server accessible by all remote computers.
- Avoid transmitting buffer data back and forth between computers.
- Avoid unnecessary inter-systems calls. Compensation means copy.
- For processing that involves lots of calls for each grabbed frame, consider a user-defined function ([`MfuncAlloc`](../../Reference/func/MfuncAlloc.md)).
- Allocate displays only if necessary.
- Use asynchronous mode for displays with the lowest possible frame rate.
