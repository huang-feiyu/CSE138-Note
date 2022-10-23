# Chandy-Lamport snapshot algorithm

[TOC]

### C-L: Details

> We are recording snapshot (record events) while everything is still running by using multiple "cameras".

*consistent snapshot*: If we have events $A$ and $B$, where $A \to B$ and $B$ is in the snapshot, $A$ should be in the snapshot too!

---

Recording a **consistent** snapshot

* An initiator process: (one or more)
  * Records its own state
  * Sends a *marker message* out on all its outgoing channels
  * Start **recording** the messages it receives on all its incoming channels
* When process P<sub>i</sub> receives a marker message on C<sub>k i</sub>:
  * If it is the **first** marker P<sub>i</sub> has seen (sent or received):
    * P<sub>i</sub> records its state
    * P<sub>i</sub> marks channel C<sub>k i</sub> as empty
    * P<sub>i</sub> sends a *marker* out on all its outgoing channels
    * P<sub>i</sub> starts recording on all incoming channels except C<sub>k i</sub>
  * Otherwise:
    * P<sub>i</sub> **stops** recording on C<sub>k i</sub>

With $N$ processes, $N \cdot (N-1)$  marker messages!

### C-L: Big picture

* Limitations
  * Channels are **FIFO**
  * No pausing application messages
* Assumptions
  * Messages aren't lost, corrupted or duplicated
  * Processes don't crash
* Properties
  * Snapshots it takes are **consistent**
  * Guaranteed to terminate
  * **Works fine with more than one initiator**

---

What are snapshots for?

* Checkpointing
* Deadlock detection
* Detection of any stable property

### Centralized vs. Decentralized algorithms

* A *decentralized* algorithm is one that can have multiple initiators.
  * Examples: Chandy-Lamport; Paxos
* A *centralized* algorithm must be initiated by exactly one process.

