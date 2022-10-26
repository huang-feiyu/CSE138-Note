# Paxos protocol

[TOC]

### more on consensus

Properties that consensus algorithms, **try** to satisfy:

* ***Termination***: each correct process eventually decides on a value<br/>violation of termination: someone has not decided yet
* ***Agreement***: all correct processes decide on the **same** value<br/>violation of agreement: there are different values
* ***Validity*** (integrity, nontriviality): the agreed-upon value must be one of the proposed values<br/>violation of validity: the result is different from all proposed values

#### FLP result

FLP: 3 scientists who prove it is **impossible** to satisfy all three properties.

Paxos compromises on *termination*. (most protocols do this)

### Paxos

> Paxos: consensus algorithm

#### Terminologies

> One process could take on multiple roles

* *proposer*: proposes values
* *acceptor*: contributes to choosing from among the proposed values
* *learner*: learns the agreed-upon value

---

Paxos node: any node that plays any role, **must**:

* persist data
* know how many nodes is majority of acceptors

#### Phase 1

> up until milestone 1: majority of acceptors have promised on `n`

Proposer:

* send a `Prepare(n)` message to [at least] a majority of acceptors
* proposal number `n` must be
  * unique (**globally**)
  * higher than any proposal number that **this** proposer has used before

[any] Acceptor:

* on receiving a `Prepare(n)` message:<br/>"Did I previously promise to ignore requests with this proposal number?"
  * If so: ignores it
  * If not: it asks:<br/>"Have I **previously accepted** anything"<br/>(1) If so: it replies with `Promise`(n, **(n<sub>prev</sub>, val<sub>prev</sub>)**)<br/>(2) replies with `Promise(n)` (it now promises to ignore any requests with a proposal number lower than `n`)

#### Phase 2

> Proposer has received `Promise(n)` or `Promise`(n, **(n<sub>prev</sub>, val<sub>prev</sub>)**) from a majority of acceptors (for some `n`)
>
> milestone 2: consensus is reached
>
> milestone 3 happens **separately** on the proposer & learners: they **know** consensus is reached

Proposer:

* send an `Accept(n, val)` message to [at least] a majority of acceptors, where
  * `n` is the proposal number, that was promised
  * `val` is chosen as follows:<br/>the val<sub>prev</sub> that went with the highest n<sub>prev</sub>, or, if there are none, whatever it wants

[any] Acceptor:

* on receiving an `Accept(n, val)` message:<br/>"Did I previously promise to ignore requests with this proposal number?"
  * If so: ignores it
  * If not: it replies with `Accepted(n, val)`, and also sends `Accepted(n, val)` to all **learners**

