---
title: Caching
description: Thiết kế cache, cache invalidation, và Redis.
tags:
  - caching
  - redis
date: 2026-09-12
---

Ghi chép từ các buổi học caching trong khoá **Ronin Engineer S02**. Hai trang bổ trợ nhau: trang
đầu nói về cách thiết kế cache độc lập với công cụ, trang sau đi sâu vào Redis là công cụ phổ biến
nhất để làm remote cache.

- **[Caching Design](/documents/caching/caching-design)** — read-aside, write-around, tại sao
  cache invalidation khó, phân tích race condition, và các sự cố điển hình: avalanche, breakdown,
  penetration, hotkey, large key.
- **[Redis](/documents/caching/redis)** — ba kiến trúc triển khai, hash slot, tại sao Redis nhanh,
  cơ chế expire, chọn cấu trúc dữ liệu, và bốn practice: delay queue, big key, hashtag,
  distributed lock.
