# Reliable delivery & reliable broadcast

[TOC]

### forms of fault tolerance

*authentication-detectable Byzantine faults*: use a checksum or something else to validate the message.

```
Byzantine faults > authentication-detectable Byzantine faults > **omission faults** > crash faults > fail-stop faults
```

---

*fault model*: tells you which kinds of faults can occur

```
Byzantine model > omission model > crash model
```

What does it mean to tolerate a class of faults?<br/>a correct program satisfies both its safety & liveness properties

How **wrong** does a program go in the presence of a given class of faults?<br/>=> Fault tolerance

|              | live          | not live    |
| ------------ | ------------- | ----------- |
| **safe**     | *masking*     | *fail-safe* |
| **not safe** | *non-masking* | 🥲           |

### implementing reliable delivery

*reliable delivery*: a liveness property<br/>Let P<sub>1</sub> be a process that sends a message m to process P<sub>2</sub>. **If neither P<sub>1</sub> nor P<sub>2</sub> crashes**, and **not all** messages are lost, then P<sub>2</sub> **eventually delivers m**.

* P<sub>1</sub> (Alice)
  * put message in send buffer
  * on timeout, send what's in the buffer
  * when an ack is received, delete message from send buffer

Problems

1. timeout: unbounded latency
2. receiver message gets lost
3. duplicate messages

### idempotence

A message that's ok to receive more than once is *idempotence*.

$f$ is idempotent if: $f(x) = f(f(x)) = f(f(f(x))) = \dots$

### at-least-once/at-most-once/exactly-once delivery

*at-least-once*: *reliable delivery*

*at-most-once*: send a message naively

*exactly-once*: Systems that **claim** exactly-once delivery<br/>(1) the messages were idempotent anyway<br/>(2) or, they're making an effort to deduplicate messages

### reliable broadcast

Reliable broadcast: If a **correct process** delivers `m`, then all **correct process**es deliver `m`.<br/>correct process: depends on fault models (for now crash model)

If you have a *unicast* primitive, you can implement broadcast by view some sends in a zone as **a *broadcast***.

Implementing: **Fault tolerance often involves making copies**!

Almost everyone sends **copies** of its delivered message to everyone else except the sender, keep track of what they have delivered.

=> **REPLICATION**

