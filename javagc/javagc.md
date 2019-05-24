class: center, middle

# Java 8+ Garbage Collectors

---

# Online

1. Java (HotSpot JVM) Garbage Collectors
    * types

2. G1 (Garbage First) Garbage Collector
3. ZGC

---

class: center, middle

# Java Garbage Collectors

---

# Two perspective of GC

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

* In Java 11, HotSpot JVM introduces the ZGC

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
