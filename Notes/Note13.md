# Strong eventual consistency & CAP trade-off

[TOC]

### eventual consistency

*Strong consistency* ultimately relies on consensus (if you want fault tolerance).

Consensus algorithm is expensive somehow, we want only *eventual consistency* in some workloads. In some situation, the totally-order is not necessary for *eventual consistency*.

---

*eventual consistency* (liveness): Replicas eventually agree, if clients stop submitting updates.

### strong convergence & SEC

*strong convergence* (safety): Replicas that have delivered the same **set** of updates have equivalent state.

*strong eventual consistency* (SEC): *eventual consistency* & *strong convergence*

### intro to application-specific conflict resolution

For *strong convergence*, we store all concurrent written values. When reading the value, return the **set** of values.

Amazon shopping car:

![shopping car](https://user-images.githubusercontent.com/70138429/198569661-cad3f6f2-4109-4154-ac41-25b552f3fcc8.png)

### network partitions

In fail-stop model, anything across the partition is considered lost messages.

Network partition: a division of a computer network into relatively independent subnets (an unfortunate fact)

### availability

*Availability*: "every request receives a response"

PB (Primary-Backup) chooses consistency over availability.

Dynamo, etc. choose availability over consistency.

### CAP trade-off

* C: Consistency 一致性
* A: Availability 可用性
* P: Partition tolerance 分区容错 (partition already happen)

Priority trade-off between *Consistency* & *Availability*

### testing distributed systems

When testing our system locally, our machines are close and communication is fast.

1. Artificially delay
2. Artificially drop messages
3. etc.

=> Netflix "chaos engineering"

