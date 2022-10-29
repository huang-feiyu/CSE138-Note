# anti-entropy & gossip, quorum consistency

> [Dynamo](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)

[TOC]

![Summary](https://user-images.githubusercontent.com/70138429/198820061-d4dee6e3-96b1-41b3-bfd0-24d6b4dcd2c0.png)

### Old ideas/Recap

* *Availability*: "every request gets a response"
* *Network partitions*: some machines[s] can't talk to other[s], temporary & unintentional
* *Eventual consistency* (liveness property): replicas eventually agree if updates stop arriving
* *Application-specific conflict resolution*: Amazon shopping cart, when adding/deleting items concurrently, **take the union** (last write wins) of them => a "big" shopping carts<br/>There is a bug: deleted item may re-appear => Deletion is an unusual operation

### Dealing with replicas that disagree

* *Strong convergence*: write commute (可交换写) in a updates set => replicas agree

* Dealing with replicas that disagree
  1. Find out that they disagree
  2. Approach #1: anti-entropy - resolving conflicts in application (KVS) [BIG\] state<br/>Approach #2: gossip - resolving conflicts view (who's up) [small\] state

(anti-entropy and gossip are synonyms in general, but differ in Dynamo)

#### anti-entropy with Merkle trees

> anti-entropy: a replica synchronization protocol to detect the inconsistencies between replicas

Dynamo minimize data transfer cost with ***Merkle trees***, *hash trees*.

In case of a replica, hash one KV pair, concatenate several pairs into another hash value until there is only one hash. We will compare the **root hash values** of replicas.

![hash tree](https://upload.wikimedia.org/wikipedia/commons/9/95/Hash_Tree.svg)

#### gossip protocol

peer-to-peer gossip to ensure that data is disseminated to all members of a group

Know who is awake in gossip with **others**.

### quorum consistency

How many replicas should a client talk to?<br/>*Quorum systems* let you configure this.

* $N$: number of replicas
* $M$: "write quorum", how many replicas have to acknowledge a write operation
* $R$: "read quorum", how many replicas have to acknowledge (i.e., respond to) a read operation

For example, $N = 3, W = 3, R = 1$: when do a write, need all acks; when do a read, need only one response.  (Read-One-Write-All) => maybe strong consistency, we do not need it in Dynamo

If we have $R+W>N$, read quorums will intersect with write quorums. For better availability, $R/W$ $< N$.

### tail latency

*latency*: time between start & end of a single action

What's the latency at the 99.9%<sup>th</sup> of the distribution?<br/>=> tail latency: latency at high end of distribution

![tail latency](https://robertovitillo.com/static/c180e7dd8afb0578f1349aa73af1aa54/8c557/distribution.png)

