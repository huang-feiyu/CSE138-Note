# Consistency models

[TOC]

### total order vs. determinism

Even with one replica, we don't necessarily have determinism.

TO: If a process delivers m<sub>1</sub>, and then m<sub>2</sub>, then all processes delivering both m<sub>1</sub> and m<sub>2</sub> deliver m<sub>1</sub> first.

### consistency models

> model: set of assumptions when you build a system

RYW (Read-Your-Writes) violation

*FIFO consistency* violation

*causal consistency* violation

---

* *FIFO consistency*: Writes done by a process are seen by all processes in the order they were issued.
* *causal consistency*: Writes that are related by happens-before must be seen in the same [causal] order by all processes.
* *strong consistency*: A replicated storage system is *strongly consistent* if clients cannot tell that the data is replicated.

```
all execution > RYW consistency > FIFO consistency > causal consistency > strong consistency
```

trade-off between **speed** and **correctness**

### dealing with failure in replication protocols

fault models

* fail-stop: crashes can occur and can be detected by the environment
* crash: crashes can occur and no one knows it

---

strongly consistent replication protocols (in fail-stop model)

* primary-backup replication
* chain replication
  * If head crashes, change the head to the next replica
  * If tail crashes, change the tail to the previous replica
  * If coordinator crashes, solutions: multiple coordinators (=> **consensus**)

### Intro to consensus

When do you need consensus?

You have a bunch of process, and ...

* you need to make sure they deliver the same messages in the same order => **totally-ordered broadcast** (atomic broadcast)
* you need them all to know what other processes exist, and keep those lists up to date => **group membership** (failure detection)
* you need one of them to paly a particular special role, and everyone else needs to know about => **leader election**
* you need them to take turns getting access to a shared resource => **distributed mutual exclusion**
* they're participating in a transaction, and need to agree on a commit/abort decision => **distributed transaction commit**

