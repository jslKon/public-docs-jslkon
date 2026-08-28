---
title: Partitions and Consumer Groups
description: Why ordering is per-partition, and why extra consumers buy you nothing.
tags:
  - kafka
  - messaging
  - distributed-systems
date: 2026-08-28
---

> **Placeholder content.** Written to exercise the standalone `.puml` embed and a table.
> Replace the prose before publishing.

Kafka is an append-only distributed log. Most questions reduce to two facts: ordering is
per-partition, and consumers track their own offsets.

![Kafka cluster topology](kafka.puml)

## Partitions decide everything

- A **topic** is split into partitions; each partition is an ordered, immutable sequence
- Ordering is guaranteed **within** a partition, never across a topic
- The partition is chosen by `hash(key) % partitionCount`, so the key is what buys you ordering
- Partition count can grow but never shrink, and growing it rehashes future keys

## Consumer groups

Each partition is consumed by exactly one member of a group, which is why adding consumers
beyond the partition count buys nothing.

| Consumers in group | Partitions | Result |
| --- | --- | --- |
| 2 | 4 | Each consumer owns 2 partitions |
| 4 | 4 | One partition each, maximum parallelism |
| 6 | 4 | 2 consumers idle |
