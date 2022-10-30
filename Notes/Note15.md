# Sharding & consistent hashing

> [Dynamo](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)

[TOC]

### Sharding (data partitioning)

What's wrong with an approach where **everyone** stores **all** the data?

* What if you have more data than can reasonably fit on one machine?
* If everyone stores all data, consistency is more expensive to maintain

=> *Sharding*

![data partitioning](https://user-images.githubusercontent.com/70138429/198867138-b54ae5f4-99a3-4a44-894a-108edcb2f677.png)

Reasons for doing this:

* increased capacity
* increased throughput

---

How do we decide how to split up our data among the shards?

| approach\property   | distribute data | index data |
| ------------------- | --------------- | ---------- |
| Randomly            | 😀               | 🥲          |
| All in one          | 🥲               | 😀          |
| By key range (hash) | 😀               | 😀          |

### Consistent hashing

If using hash approach, when we add one more node, what should we do?

We need to move data from one to another. *Consistent hashing* gives us minimum movement!

![hash](https://uploads.toptal.io/blog/image/129310/toptal-blog-image-1551794764808-260dc8495f15e76a523f52a512d48acb.png)

When inserting a K/V pair:

1. Hash a key => number
2. Move clockwise until hit required nodes (最少存储数)

When adding a new machine $M$:

1. Some pairs **after** $M$ need to be moved to $M$
2. Everything else works well

When a machine $M$ crashes:

1. The machine **before** $M$ has to take on the pairs of $M$
2. Hopefully the machine **after** $M$ has the backup (machines use gossip to know sth, it should propagate pairs)

---

We cannot assume the machines locate evenly => Use *virtual nodes*

* improve distribution (more even) of nodes on the ring
* account for differences in storage capacity

(Downside/Feature: If a physical node crashes, everyone needs to take on some of the pairs)

