# Overview

> Uncertainty is the fundamental property of distributed systems.

[TOC]

### What is a distributed system?

A distributed system is:

* running on several *node*s (a computer or an instance on a computer)<br/>connected by a network
* characterized by *partial failure* (some part [maybe] broken, others ok)

Another definition: partial failure + unbounded latency

### *partial failure*

> On a single computer, we do not talk about *partial failure*. The more computers there are, the big possibility of some computers failing.

* HPC (High Performance Computing) philosophy
  * treat partial failure as total failure
  * checkpointing
* "Cloud computing" philosophy
  * treat partial failure as expected and work around it
  * Everything fails all the time

### Problems

**Example**: Consider two machines M1 & M2, M1 requests $x$ from M2, M2 respond back $x$

How could this go wrong?

1. Request from M1 gets lost
2. Request from M1 is slow
3. M2 crashes
4. M2 is slow to respond
5. Response from M2 is slow
6. Response from M2 could get lost
7. "corrupt transmission" (传输损坏)<br/>M2 lies to M1; Byzantine faults (There is an attacker)

---

> **If you send a request to another node and don't receive a response, it is impossible for you to know why**.

How do real systems try to **cope with this**?

with timeouts: Wait a certain amount of time, then assume failure and try again

There is ***unbounded latency*** => imperfect solution

### Why there is a distributed system?

Why deal with all this "pain"?

* You want to make things faster
* more data than can fit on one machine
* reliability (more copies for data)
* throughput

It's worthwhile to construct a distributed system.

