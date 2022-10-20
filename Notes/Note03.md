# Logical Clocks - Part I

[TOC]

### Partial orders & Total orders

Partial order: 偏序关系

* A set $S$, together with

* A binary relation, often written ≤, that lets your compare elements of $S$, and has the following properties:

  * Reflexivity: 自反性<br/>for all $a$ in $S$, $a \le a$
  * Anti-symmetry: 反对称性<br/>for all $a, b$ in $S$, if $a \le b$ and $b \le a$, then $a = b$
  * Transitivity: 传递性<br/>for all $a,b,c$ in $S$, if $a \le b$ and $b \le c$, then $a \le c$

In lamport diagram, the set is the *collection of events*, the relation is *happens-before*. => **{*events*, *happens-before*}**

| Yes/No             | Property for happens-before |
| ------------------ | --------------------------- |
| No                 | 自反性                      |
| Yes (vacuums true) | 反对称性                    |
| Yes                | 传递性                      |

In partial ordered set (poset), some elements cannot be compared => concurrent in lamport diagram => A||B means that A,B is concurrent

total order: 全序关系<br/>All elements can compared with each other

### Lamport Clocks

> logical clocks - ordering of events only

lamport clocks - a kind of *logical clock*.

* LC(A) - the lamport clock of event A
* "clock condition": If A -> B, then LC(A) < LC(B)<br/>NOTE: If LC(A) < LC(B), A happens before B **potentially**.
* lamport clocks are *consistent with causality*.

Algorithm:

1. Every process keeps a counter, initialized to 0
2. On every event on a process, that process increments its counter by 1
3. When sending a message, a process includes its current counter
4. When receiving a message, a process sets its counter to max(local, received) + 1

---

*consistent with [potential] causality*: A->B => logical clock of A < logical clock of B (可使用逆否命题)

*characterizes causality*: logical clock of A < logical clock of B => A->B

Lamport clocks have previous property but next.

### Vector Clocks

vector clocks: A->B <=> VC(A)<VC(B)

Algorithm:

1. Every process keeps a **vector** (length $N$ for $N$ processes) of integers, initialized to zeroes
2. On every event (sends, receives, internal events), a process increments its own position and its vector clock
3. When sending a message a process includes its current vector clock<br/>(after the increment from step 2, because sends are events)
4. When receiving a message, a process updates its vector clock to max(local, received)<br/>(after incrementing its position, because receives are events)

max of vectors:<br/>[1, 12, 4] & [7, 0, 2] => [7, 12, 4]

