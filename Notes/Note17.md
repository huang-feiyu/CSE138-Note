# MapReduce Recap

> [MapReduce](https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)

[TOC]

### MapReduce Recap

3 phases in MapReduce

* map phase
  * `map` function: input k-v pair -> **set** of intermediate k-v pairs
  * `<Doc1, [the, quick, fox]>` -> `{<the, Doc1>, <quick, Doc1>, <fox, Doc1>}`
* shuffle phase: data (intermediate k-v) from map workers is sent to reduce workers, according to some data partitioning function, e.g. `hash(key) % R` (would be bad if R changes)
* reduce phase
  * `reduce` function: (key, set of values) -> set of output values
  * `<dog, {Doc1, Doc3}>` -> `<dog, [Doc1, Doc3]`<br/>`<dog, {1, 3, 2}>` -> `<dog, 6>`

