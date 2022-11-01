# MapReduce

> [MapReduce](https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)

[TOC]

### Online vs. Offline systems

* *online systems* / *services*
  * wait for client requests, and try to handle them quickly
  * low latency & availability, often prioritized
  * examples: KVs, web servers, databases
* *offline system* / *batch processing systems*
  * process **lots** of data
  * high throughput, often prioritized
  * examples: MapReduce

### Data orgnizations

When data scientists analyses data, we want a more efficient way to handle data via different data organizations.

* raw data
  * the authoritative version
  * the format new data goes into
* derived data
  * result of taking existing data and processing it somehow

```
input -> [raw data] -> [derived data]
                          |       ^
                          ---------
```

MapReduce is a tool for **computing derived data**.

### Inverted Index

*forward index*: for every document, gets words

*inverted index*: for every word, gets which documents

=> How would you compute the inverted index from the forward index?<br/>It's conceptually simple.

```python
# naive version
for each document D:
    for each word w in D:
        emit <w, D> # intermediate key-value pairs
then, combine Ds in a list for each unique w
```

### Shuffle

Observation: documents in different machines are totally **independent**.

Machines stored raw data use **shuffle** communication pattern to send data to record machines *GFS*. (concurrently)

* Map function for input
* Reduce function for output

![MapReduce](https://user-images.githubusercontent.com/70138429/199194793-c674773a-cf6e-4c55-843d-a08bc2fbf7f3.png)

### Tasks

Typical tasks for MapReduce:

* inverted index
* grep (regex match)
* sort
* word count
* etc.

---

MapReduce use $\text{hash}(key) \mod N$ (not consistent hashing), $N$ is the reduce workers => we know the size of data we need to store, so $N$ is determined.

