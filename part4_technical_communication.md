# Part 4: Technical Communication

The main reason i selected PR 193 is because it was more understandable compared to other PRs. Some PRs were either very small or very complex, so it was difficult to follow them properly.

This PR focuses on consumer fetching logic, which is an important part of aiokafka. Since i already have some understanding of async programming, it was easier for me to understand this.

Currently i am working as an AI Engineer at Interviewgod. In my role, i work on systems involving LLMs, backend logic and async workflows. So i have some experience with background tasks and async execution, which helped me here.

Even then, there are some challenges.

Async programming can be tricky. Even small mistakes can cause race conditions. Also, offset management is critical, because if it is wrong, it can cause duplicate or missing messages.

Another challenge is understanding how different components interact. Fetcher and consumer run separately but share state, so it is not very simple.

To handle this, i would first understand the existing code properly. I will trace how fetch requests are made and how offsets are updated.

Then i will implement changes step by step and test each part. I will also add logs to understand behavior better.

Overall, i selected this PR because it is understandable and also helps me learn more about async systems.

---

## Declaration

I declare that all written content in this assessment is my own work, created 
without the use of AI language models or automated writing tools. Al technical 
analysis and documentation reflects my personal understanding and has been 
written in my own words.
