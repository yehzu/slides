class: center, middle

# Java 8+ Garbage Collectors

---

# Online

1. Java (HotSpot JVM) Garbage Collectors

    * types

2. The Garbage Collector we use

3. ZGC

---

# What is GC?

--

* malloc() / free()

* segmentations

* black magic


---

# Two perspectives of GC

1. Latency

2. Throughtput

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

class: left, middle
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


---

# Young GC

* STW (Stop-The-World) event

* Triggered when Eden space full

    * size of Eden space is **dynamic**

    * from 5% to 60% of heap


```desktop
GC pause (young); #1 [Eden: 612.0M(612.0M)->0.0B(532.0M) Survivors: 0.0B->80.0M Heap: 612.0M(12.0G)->611.7M(12.0G)]
GC pause (young); #2 [Eden: 532.0M(532.0M)->0.0B(532.0M) Survivors: 80.0M->80.0M Heap: 1143.7M(12.0G)->1143.8M(12.0G)]
```

---

# Terminology

1. STW (Stop-The-World) event

2. Region

3. Rset (Remerber set)

4. Cset (Collection set)

---

# References

* https://plumbr.io/blog/garbage-collection/minor-gc-vs-major-gc-vs-full-gc


