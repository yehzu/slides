class: center, middle

# Java 8+ Garbage Collectors

---

# Hopes that...

Everyone could answer following questions after this sharing

* How many garbage collectors are available in HotSpot JVM

* Which garbage collector we are using

* How does it work?

* How to read the GCLog?

---


# Online

1. Java (HotSpot JVM) Garbage Collectors

    * types

2. The Garbage Collector we use

3. ZGC

---

# What is GC?

--

### black magic?!

--

### don't call `System.gc()`

--

### get rid of malloc() / free() / memory pools

---

# Two perspectives of GC

## Latency

How long a GC *Stop The World*

## Throughtput

How much time the GC occupied in a period

---

# Java Garbage Collectors

* Prior to Java 8, there are four garbage collectors

--

    1. Serial Garbage Collector
--
    2. Parallel Garbage Collector
--
    3. Concurrent Mark Sweep (CMS) Garbage Collector
--
    4. G1 Garbage Collector (Introduced in Java 7, dafault since Java 9)
--

* In Java 11, HotSpot JVM introduced the ZGC

---
class: left, middle

# Q: What's the GC we use in production?

--

# A: G1


---

# Serial Garbage Collector

* single thread

* freeze all application threads when it runs

```shell
-XX:+UseSerialGC
```

---

# Parallel Garbage Collector

* multi-thread

* still freeze all application threads when it runs

```shell
-XX:+UseParallelGC
```

---

# CMS Garbage Collector

* multi-thread

* still freeze all application threads when it runs

```shell
-XX:+UseParallelGC
```

---

class: center, middle

# G1 GC

---

# G1 GC

1. preditiable low latency

2. concurrent marking

3. two generations

---

# G1 GC

1. Young GC (a.k.a. Minor GC)

2. Global Concurrent Marking

   * write barriers

3. Old GC (a.k.a. Major GC)

4. Full GC

---

# Memory Allocation

* Region based (from 1mb - 32mb)

* make region count in \[2048, 4096\]. (That is, Heap: 2GB - 128GB)

* `-XX:G1HeapRegionSize=N`

---

# Memory Allocation


heap graph, demo the memory alloc process


---

graph

demo young gc triggered

---

# Young GC

* assumption: "infant mortality"

    * most of the objects created on heap die young

    * suvriors normally live quite long

* purpose: release unreferenced objects

---

# Young GC

* STW (Stop-The-World) event

* Triggered when Eden space full

    * size of Eden space is **dynamic**

    * from 5% to 60% of the whole heap

* Scan from GC root through Eden regions

* copy live objects to Survivor space



```desktop
GC pause (young); #1 [Eden: 612.0M(612.0M)->0.0B(532.0M) Survivors: 0.0B->80.0M Heap: 612.0M(12.0G)->611.7M(12.0G)]
GC pause (young); #2 [Eden: 532.0M(532.0M)->0.0B(532.0M) Survivors: 80.0M->80.0M Heap: 1143.7M(12.0G)->1143.8M(12.0G)]
```

---

# Concurrent Marking Cycle

* Triggered when 45% heap is occupied

    * `–XX:InitiatingHeapOccupancyPercent`

* Snapshot-At-The-Beginning (SATB) algorithm


---

# Snapshot-At-The-Beginning (SATB)

1. initial marking

    * Start accompany with a Young GC, share the scanning step

    * shorten the STW

2. concurrent marking

    * pre-write barriers for recording changes of references

3. remark

4. clean up

---

# Write Barriers



---

# Mix GC
---

# Terminology

1. STW (Stop-The-World) event

2. Region

3. Rset (Remerber set)

4. Cset (Collection set)

---

# References

* https://www.infoq.com/articles/G1-One-Garbage-Collector-To-Rule-Them-All/

* https://xmlandmore.blogspot.com/2014/11/g1-gc-what-is-to-space-exhausted-in-gc.html

* https://plumbr.io/blog/garbage-collection/minor-gc-vs-major-gc-vs-full-gc


