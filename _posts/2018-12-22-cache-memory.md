---
layout: post
title:  "Cache Memory"
date:   2018-12-22 22:22:32 +0530
permalink: /cache-memory/
---
# Cache Memory
22/12/2018

Cache memory is fast on chip memory available to the CPU

It is different from DRAM, which is slow. 

Cache is SRAM. 

There are different levels of cache L1, L2, L3. 

A cache miss occurs when CPU expects data from the cache, but data is not available. 

Only limited data is available on the cache. Therefore data needs to be cycled from the cache from time to time. So that frequently accessed data is always available. 

There are 4 cache replacement algorithms, which cycle data

- First in first out
- Last in first out
- Least recently used
- Most recently used
