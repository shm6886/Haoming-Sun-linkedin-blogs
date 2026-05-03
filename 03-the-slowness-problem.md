# The Slowness Problem: Diagnosing RAG Bottlenecks

Performance issues in production are sneaky. They don't break things. They just make them worse, gradually, until users stop showing up.

After we shipped RAG, student engagement dropped. Not dramatically. Just noticeably. Fewer repeat questions. More timeouts. The system was technically working. Responses were correct. But students were waiting too long.

We measured it. The original single-pass GPT-4 system? 2-3 seconds per answer. The RAG system? 8-12 seconds. For a student waiting on homework, that's the difference between "this is useful" and "forget it, I'll ask someone else."

## Understanding the Bottleneck

The RAG pipeline has three main stages. Each one takes time:

1. **Embedding the question.** Convert the student's question into a vector—a numerical representation of what it means. Send that to the database.

2. **Retrieving relevant materials.** Chroma searches for similar vectors in the curriculum database and returns the top results. Usually fast, but depends on database size and load.

3. **Generating the response.** GPT-4 reads the retrieved materials and generates an answer. This is where the time goes.

We profiled the system and found it: step 3. Generation was eating 6-10 of those 8-12 seconds. Embedding and retrieval combined barely took a second.

It made sense once we understood it. GPT-4 generates tokens one at a time. When it's reading through retrieved materials and crafting a thoughtful response, it's doing real work. It's not just predicting the next word—it's reasoning about what the context means, relating it to the curriculum, structuring an explanation. That takes compute.

## Why This Mattered More Than We Thought

Here's the thing about latency in education: it's not just annoying. It changes behavior.

A 2-second response? Students wait. A 12-second response? Students close the tab. They ask their friend instead. They move to the next problem. The tutoring system stops being the first thing they try.

And we were losing visibility. The students who stuck around and waited were only a subset. The ones who gave up? We'd never know what they were struggling with. Our system was optimizing for the patient, not for the learners who needed fast feedback.

There was also a cost angle. We were paying for GPT-4 API calls, and longer generations meant more tokens. At scale, that was starting to add up.

## How We Approached the Fix

We had options, none of them clean.

**Option 1: Switch to a smaller, faster model.** Something like Claude 3.5 Haiku might be competitive on speed. Risk: quality could drop.

**Option 2: Optimize the RAG pipeline.** Use a faster embedding model, limit the context we pass to GPT-4, parallelize retrieval with generation. Risk: each optimization has its own tradeoffs.

**Option 3: Accept the latency.** Keep RAG, build UX workarounds. Show a loading animation. Stream the response token-by-token so it feels faster.

**Option 4: Hybrid approach.** Simple questions skip the full RAG pipeline. Complex ones get the full treatment.

We didn't have time to optimize everything perfectly. We had August left. We couldn't rebuild the whole thing.

What we could do was understand the tradeoff well enough to make smart decisions going forward. We learned that RAG + GPT-4 was powerful but slow. We learned that for tutoring, latency isn't just a performance problem—it's a feature problem. We learned that sometimes the right architectural choice comes with a cost you have to pay.

The slowness taught us something else too: we weren't measuring the right things. We'd been optimizing for accuracy and correctness. We hadn't been optimizing for actual student experience—which includes how long they wait for an answer.

That realization set us up for the next question: if RAG was correct but slow, and single-pass GPT-4 was fast but hallucinated, what were we actually building? What did "good" actually mean?

---

**Next: We started asking what students actually needed from their tutor. That's when we realized the problem wasn't just retrieval—it was teaching.**
