# Fault models & two generals problem

[TOC]

### safety & liveness

Delivery guarantees are included in safety properties.

* *safety properties*
  * say that a "bad" thing won't happen
  * can be violated in a finite execution
* *liveness properties*
  * say that a "good" thing will (eventually) happen
  * cannot be violated in a finite execution

### reliable delivery

*reliable delivery*: Let P<sub>1</sub> be a process that sends a message m to process P<sub>2</sub>. **If neither P<sub>1</sub> nor P<sub>2</sub> crashes**, and **not all** messages are lost, then P<sub>2</sub> **eventually delivers m**.

### fault models

*fault model*: tells you which kinds of faults can occur

1. Request from M1 gets lost -- omission fault
2. Request from M1 is slow -- timing fault
3. M2 crashes -- crash fault
4. M2 is slow to respond -- timing fault
5. Response from M2 is slow -- timing fault
6. Response from M2 could get lost -- omission fault
7. "corrupt transmission" (传输损坏) i.e., M2 lies to M1 -- Byzantine fault

---

*fail-stop fault*: a process fails by halting, and everyone knows it crashed

*crash fault*: a process fails by halting<br/>(stops sending/receiving messages)

*omission fault*: a message is lost<br/>(a process to send or receive one message)

*time fault*: a process responds too late (or too early)<br/>(In an asynchronous world, there are no timing faults)

*Byzantine fault*: a process behaves in an arbitrary or even malicious way

```
Byzantine faults > omission faults > crash faults > fail-stop faults
```

---

A *fault model* is specification that says what kinds of faults a system can exhibit, and this tells you what kinds of faults need to be tolerated.

### two generals problem

![problem](https://user-images.githubusercontent.com/70138429/197460502-90f4e64f-2eab-4762-9311-b8f35b315d16.png)

* Workaround #1: probabilistic certainty
* Workaround #2: common knowledge<br/>we say there's common knowledge of P when: everyone knows P, everyone knows that everyone knows P, ...

