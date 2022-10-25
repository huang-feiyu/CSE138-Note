# Replication

[TOC]

### reasons to do replication

Why replicate state/data?

* fault tolerance: prevent data loss
* data locality: have the data close to clients that need it
* diving up the work

---

Downsides of replication

* expensive machines
* have to keep replicas consistent (often)

### strong consistency

*strong consistency* (informally): A replicated storage system is *strongly consistent* if clients cannot tell that the data is replicated.

### primary backup replication

*primary backup replication*: A protocol to ensure strong consistency.

![Client, Primary and multiple backups](https://user-images.githubusercontent.com/70138429/197696585-17d67fb0-3872-458e-8088-a1d49c34afeb.png)

How about advantages

* fault tolerance (😀)
* data locality (💩)
* diving up the work (💩)

### chain replication

*chain replication*: propagate write, read from the tail.

![client, head, backups, tail](https://user-images.githubusercontent.com/70138429/197697857-f1e0d13b-39d9-4ed9-885e-165906264430.png)

How about general advantages

* fault tolerance (😀)
* data locality (💩)
* diving up the work (👌)

---

*throughput*: number of actions per unit of time

Depending on the workload (mix of writes/reads), *chain replication* could give you better throughput than *primary backup replication*.

*latency*: time between start and end of one action

RC has worse write latency than PB, depending on chain length.

=> Trade-off between **throughput** and **latency**

