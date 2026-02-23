# Part 3: Prompt Preparation

## 1. Repository Context 

The aiokafka library is a Python library that allows developers to use Apache Kafka asynchronously. This means that rather than blocking your program like traditional libraries do, aiokafka uses asyncio to run multiple tasks simultaneously in order to allow for proctecess of messages that may continuously arrive without delays.

The aiokafka repository has two major parts: producers and consumers. The producer sends messages to Kafka topics and the consumer reads them from those Kafka topics. The consumer side of the library is more complicated than the producer side of the library due to the fact that the consumer has to keep track of offsets and partitions while simultaneously running a background fetch loop to retrieve new messages from Kafka.

Internally, there are a number of components included in the library such as the fetcher which handles the actual operation of requesting the messages from Kafka. After the consumer has received the message it is responsible for maintaining the state of that message and makes it available to the user. Because the aiokafka library uses an asynchronous design, it is important that each of these components remain consistent with one another.

The aiokafka library is primarily used in backend systems, such as event-driven architectures and streaming pipelines. Due to the fact that Apache Kafka can process very large amounts of data, even the smallest mistakes made during the fetching or tracking of offsets can result in issues such as duplicate records or missing records. Therefore, it is vital that this library continues to provide reliable results.

---

## 2. Pull Request Description 

The purpose of PR 193 is to enhance the logic for fetching items within the consumer. Initially it was not too intuitive for me, due to the large number of files that are interrelated such as consumer and fetcher. However, after reading the PR a few times, it begins to make sense.

Within aiokafka, a consumer continuously executes a fetch loop asynchronously in the background sending requests to the Kafka broker and receiving batches of messages. This execution is dependent on state management.

Prior to this PR there were issues in relation to expected fetch loop behaviours for certain scenarios. For example, on failure or cancellation of a task, the fetch loop may terminate suddenly and without any notification. In addition, the coordination of state between the fetcher and the consumer was also not clearly defined.

As a result of this PR, the fetch loop should now provide a more reliable execution for proper error handling and improve management of offsets, therefore providing the consumer always knows where to fetch their next message from.

With these changes to aiokafka, we should see increased stability and predictability of the overall system particularly with regards to async environments where tasks may behave differently.

---

## 3. Acceptance Criteria

1. The fetch loop should continue running without stopping unexpectedly  

2. Offsets should always represent the correct next position to read  

3. Messages should not be duplicated or missed  

4. Fetch requests should only be sent when needed  

5. Async errors should not break the consumer  

6. Fetcher and consumer state should remain consistent  

7. System should behave predictably under load  

---

## 4. Edge Cases

1. Fetch task stopping  
   - If fetch loop stops due to error, system should handle or restart it  

2. Network delays  
   - Slow or delayed responses should not break the flow  

3. Offset mismatch  
   - Incorrect offset should not cause crash or wrong data  

4. Empty partitions  
   - Should not create unnecessary loops  

5. High throughput  
   - System should still work correctly with large data  

---

## 5. Initial Prompt 

You are working on the aiokafka repository, which is an asynchronous Kafka client written in Python using asyncio. The repository contains a consumer component that continuously fetches messages from Kafka brokers using a background fetch loop.

The fetching logic is implemented using asynchronous tasks. The fetcher component is responsible for sending fetch requests and receiving message batches, while the consumer manages offsets and exposes messages to the user.

Currently, there are some issues in the fetching mechanism. In certain cases, the fetch loop may stop unexpectedly, especially when errors occur or tasks are cancelled. This can result in the consumer not receiving messages even though it is still running.

Your task is to improve the fetching logic similar to PR 193.

You need to make sure that:

- The fetch loop keeps running continuously and does not stop unexpectedly  
- Async errors are handled properly without breaking the system  
- Offsets are always updated correctly after fetching messages  
- The next fetch request starts from the correct offset  
- Duplicate or missing messages are avoided  
- Fetcher and consumer state remain synchronized  

While implementing this, follow the acceptance criteria:

- Fetch loop should be stable  
- Offsets should always be correct  
- No unnecessary fetch requests should be made  
- Async execution should not cause race conditions  

Also consider edge cases such as:

- Fetch task termination  
- Network delays or failures  
- Empty partitions  
- High load scenarios  
- Consumer restart  

You will need to modify files like:

- aiokafka/consumer/fetcher.py  
- aiokafka/consumer/consumer.py  

Make sure your implementation works correctly with asyncio event loop and does not block execution.

After implementing, write tests to verify:

- Fetch loop continues working  
- Offsets are correct  
- No duplicate messages  
- Errors are handled properly  

The final solution should improve reliability of the consumer and make the fetching mechanism more robust.

---

## Declaration

I declare that all written content in this assessment is my own work, created 
without the use of AI language models or automated writing tools. Al technical 
analysis and documentation reflects my personal understanding and has been 
written in my own words.
