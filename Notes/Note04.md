# Delivery Protocols

[TOC]

### Concepts

*protocol*: a set of rules that processes use to communicate with each other

*sending* a message is something you do

*receiving* a message is something that happens to you

*delivering* a message is something you can do with a message you receive<br/>(you can queue up *received* messages and wait to *deliver* them)

### Delivery guarantees

##### FIFO delivery

FIFO delivery: If **a process** sends message m<sub>2</sub> after message m<sub>1</sub>, any process delivering both delivers m<sub>1</sub> first (Already a part of TCP)

Implementation with **sequence numbers**:

* messages get tagged with sender ID, and sender sequence number
* senders increment their sequence number after sending
* if a received message's SN is the SN of the previous message from that sender + 1, deliver<br/>(using SNs for FIFO delivery only works well if you also have **reliable delivery**)

Implementation with **serial**:<br/>Alice sends a message to Bob, before Bob replies "delivered", Alice cannot send another message.

##### causal delivery

*causal history of $A$*: the collection of events happens-before $A$

*causal delivery*: If m<sub>1</sub>'s send happened before m<sub>2</sub>'s send, then m<sub>1</sub>'s delivery must happen before m<sub>2</sub>'s delivery.

##### totally-ordered delivery

*totally-ordered delivery*: If a process delivers m<sub>1</sub>, and then m<sub>2</sub>, then all processes delivering both m<sub>1</sub> and m<sub>2</sub> deliver m<sub>1</sub> first.

