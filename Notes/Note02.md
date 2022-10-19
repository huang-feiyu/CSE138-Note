# Clocks and Causality

[TOC]

### Time & Clocks

What do we use clocks for?

* "scheduling": marking *points* in time
* durations/intervals of time

Different types of clocks

* time-of-day clocks
  * tells you what time of day it is
  * can be synchronized between machines using, e.g, NTP (Network Time Protocol)
  * can jump forward/backward!
* monotonic
  * only goes forward
  * only meaningful for particular machine => not comparable between machines

| Physical clocks    | Points in time | internals/durations |
| ------------------ | -------------- | ------------------- |
| time-of-day clocks | 😐              | 🥲                   |
| monotonic clocks   | 🥲              | 😀                   |

***Logical clocks***: Only measure the **order of events**

### Lamport diagrams

> Spacetime diagrams

Visual language for talking about time

given events A and B, we say `A -> B` if:

* A and B occur on the same process with A before B (*internal events*)
* A is send event and B is the corresponding receive event
* if `A -> C` and `C -> B` (transitive)

<img src="https://user-images.githubusercontent.com/70138429/196607647-549514e9-e5ce-4cc6-86f7-19ee4d06ec9d.png" alt="lamport diagrams" style="zoom:50%;" />

---

causal anomaly

<img src="https://user-images.githubusercontent.com/70138429/196608721-684331ce-43fc-451e-ba1a-c63b34dcb714.png" alt="causal anomaly" style="zoom:50%;" />

### Network models

A *synchronous network* is one where there exists an $n$ such that no message takes longer than $n$ units of time to be delivered. => with unbounded timeout, <i><s>synchronous network</s></i>

An ***asynchronous network*** is one where there exists no such $n$.

### Causality and happens-before

`A -> B`: "A happened before B"

What does this tell me about *causality*?

* A could have caused B
* B **could not** have caused A

given events A and B, we say `A -> B` if:

* A and B occur on the same process with A before B (*internal events*)
* A is send event and B is the corresponding receive event
* if `A -> C` and `C -> B` (transitive)

---

When two events have no causality, we say they are **concurrent**.

### State & events

Record event logs from itself and another machine => Get the state

