# Part 2: Pull Request Analysis

## 1. Introduction

In this part, i went through all the 10 pull requests from aiokafka. Honestly, at first it was little difficult to understand because some PRs involve deep Kafka concepts.

So i selected 2 PRs which i was able to understand reasonably.

Selected PRs:
- PR 193  
- PR 232  

---

## 2. PR 1 Analysis

### PR Link

https://github.com/aio-libs/aiokafka/pull/193

---

### PR Summary

This PR improves the fetching logic of Kafka consumer. The consumer continuously fetches messages from Kafka brokers.

Before this change, i think the fetching logic was not fully optimized. There could be unnecessary fetch calls or incorrect handling of internal state.

This PR improves how fetch requests are managed and how offsets are tracked.

Offsets are very important because they decide which message should be read next. If they are wrong, it can cause duplicate or missing messages.

So overall, this PR improves reliability of the consumer.

---

### Technical Changes

Files:

- https://github.com/aio-libs/aiokafka/blob/master/aiokafka/consumer/fetcher.py  
- https://github.com/aio-libs/aiokafka/blob/master/aiokafka/consumer/consumer.py  

Changes include:

- Fetch loop updates  
- Offset handling  
- State synchronization  

---

### Implementation Approach

From what i understood, the PR improves how fetch requests are triggered.

In aiokafka, fetching happens asynchronously in background. The consumer sends requests and receives batches of messages.

This PR makes sure that fetch requests are only sent when needed. Earlier it might send them more frequently than required.

It also ensures that offsets are always correct. This helps in avoiding duplicate messages.

Another part is synchronization. Fetcher and consumer both run asynchronously, so their states need to be consistent.

I think the main idea is to make fetching more controlled and predictable.

---

### Potential Impact

This PR affects consumer behavior.

It improves:
- Performance  
- Correctness  
- Stability  

Which is very important in production systems.

---

## 3. PR 2 Analysis

### PR Link

https://github.com/aio-libs/aiokafka/pull/232

---

### PR Summary

This PR focuses on resource handling in async system.

In asyncio, if tasks are not handled properly, they can keep running or not release resources.

This PR improves cleanup logic and ensures proper closing of connections.

---

### Technical Changes

- Cleanup functions  
- Async task handling  
- Connection closing  

---

### Implementation Approach

The PR ensures that async tasks are properly awaited and cancelled.

It also improves error handling so that even if something fails, resources are cleaned.

Kafka connections are also closed properly.

This makes system more stable.

---

### Potential Impact

It improves:

- Memory usage  
- Stability  
- Resource management  

So overall, it makes system safer for long running use.

---

## Declaration

I declare that all written content in this assessment is my own work, created 
without the use of AI language models or automated writing tools. Al technical 
analysis and documentation reflects my personal understanding and has been 
written in my own words.
