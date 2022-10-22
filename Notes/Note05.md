# Causal Broadcast

[TOC]

### Delivery guarantees (continued)

* *FIFO delivery*: If **a process** sends message m<sub>2</sub> after message m<sub>1</sub>, any process delivering both delivers m<sub>1</sub> first (Already a part of TCP)
* ***causal delivery***: If m<sub>1</sub>'s send happened before m<sub>2</sub>'s send, then m<sub>1</sub>'s delivery must happen before m<sub>2</sub>'s delivery. (Today)
* *totally-ordered delivery*: If a process delivers m<sub>1</sub>, and then m<sub>2</sub>, then all processes delivering both m<sub>1</sub> and m<sub>2</sub> deliver m<sub>1</sub> first.

```
All executions >
FIFO delivery  >
causal delivery
---
All executions >
totally ordered delivery
```

![Execution](https://user-images.githubusercontent.com/70138429/197324200-e0fb75e6-cdf7-49ec-bc05-9314d36389ca.png)

### Implementing causal broadcast

*unicast messages*: 1 sender, 1 receiver (point-to-point)

*multicast messages*: 1 sender, multiple receivers<br/>special case, ***broadcast***: 1 sends, **everyone** receives

---

vector clocks algorithm (with a twist, don't count **message receives** as events)

* Every process keeps a VC, initially 0s
* When a process **sends** a message, it increments its own position in its VC, and includes the updated VC with the message
* When a process **delivers** a message, it updates its VC to the pointwise maximum of its local VC and the received VC on the message

---

*Causal delivery* is the property of executions.

*Causal broadcast* is an algorithm that gives causal delivery in a setting where all messages are broadcast messages.

---

causal broadcast algorithm: We want to define a *deliverability condition* that tells us if a received message is or is not OK to deliver.

**Deliverability**: a message `m` is deliverable at a process `p` if<br/>(1) `VC(m)[k] = VC(p)[k] + 1`, if `k` is the sender<br/>(2) `VC(m)[k] <= VC(p)[k]`, otherwise

If a message has not deliverability condition, then queue it in a wait queue.

### Snapshots of distributed systems

Property that we want: If we have events $A$ and $B$, where $A \to B$, and $B$ is in the snapshot: $A$ is in the snapshot too!

Chandy-Lamport algorithm: TODO:

*channel*: connection from one process P<sub>1</sub> to another P<sub>2</sub>, with FIFO ordering.<br/>C<sub>1 2</sub>: channel from P<sub>1</sub> to P<sub>2</sub>

