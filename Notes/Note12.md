# Paxos wrap-up & passive/active replication

[TOC]

### Paxos: dueling proposers

**Non-termination** because of multiple proposers => ***dueling proposers***

We can use leader election algorithm to use only one proposer, but it is also probably non-terminated.

### Multi-Paxos

What if you need to decide on a **sequence** of values?

It is slow to use one-Paxos for a sequence of values, because we need to going through all this for every single value we want to get consensus on.

Solution: Acceptors keep on accepting another value with same proposal number `n` in Phase 2 => *multi-Paxos*

### fault tolerance in Paxos

Why can't we just have 1 acceptor?<br/>**It might crash!**

In general, a minority of acceptors can crash.<br/>If $f$ is the number of acceptor crashes you want to tolerate, $2f+1$ is how many acceptors we need.

---

**Omission fault** tolerance: Paxos does OK under omission faults! (It won't terminate somehow)

---

Paxos is *fail-safe* tolerance (safe but not live). It won't get all things down (safe), but also won't terminate (live).

### other consensus protocols

* Raft: easier to understand than Paxos, but there is nothing new here<br/>**emphasis on understandability**
* Zab (Zookeeper Atomic/Totally-ordered Broadcast)
* View-stamped Replication

All of theses are for a sequence of values, like multi-Paxos.

All do leader election.

### passive/active replication

* *Active* (state machine replication): Execute an operation on each replica.
  * The operation is sent to primary & all backups and finally return to application. 
  * Better if state update is large
* *Passive*: State update gets sent to backups.
  * Execute operation in primary backups and send new state to all backups.
  * Better if operation is expensive

