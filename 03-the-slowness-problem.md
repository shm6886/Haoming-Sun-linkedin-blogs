# The Slowness Problem: Diagnosing RAG Bottlenecks

Performance issues are insidious in production systems. They don't break things — they just make them worse, gradually, until users stop using them.

After we shipped RAG, student engagement dropped. Not dramatically. Just... slower. Fewer repeated questions. More students timing out. The system was technically working. Responses were correct. But they were taking too long to arrive.

We measured it. The original single-pass GPT-4 system responded in about 2-3 seconds. The RAG system was taking 8-12 seconds. For a student waiting for a homework answer, that's the difference between "this is useful" and "forget it, I'll Google it instead."

## Understanding the Bottleneck

The RAG pipeline has three main steps. Each one takes time:

1. **Embedding the question.** We convert the student's question into a vector — a numerical representation of its meaning. This embedding gets sent to the vector database.

2. **Retrieving relevant materials.** Chroma searches for the most similar vectors in the curriculum database and returns the top results. This is usually fast, but depends on database size and query load.

3. **Generating the response.** GPT-4 reads the retrieved materials and generates an answer. This is where most of the latency lives.

We profiled the pipeline and found the culprit: step 3. Generation was taking 6-10 of those 8-12 seconds. The embedding and retrieval combined barely took a second.

This made sense once we understood it. GPT-4 generates tokens one at a time, and when it's reading through retrieved materials to craft a thoughtful response, it's doing a lot of work. It's not just predicting the next word — it's reasoning about what the context means, relating it to the curriculum, structuring an explanation. That takes compute.

## Why This Mattered More Than We Thought

Here's the thing about latency in education: it's not just an annoyance. It changes behavior.

A 2-second response? Students wait. A 12-second response? Students close the tab. They ask their friend instead. They move on to the next problem. The tutoring system stops being their first instinct.

And from our perspective, we were losing data. The students who stuck around and waited for answers were only a subset of the population. The students who gave up? We'd never know what questions they had or what they were struggling with. Our system was optimizing for the patient, not for the learners who needed quick feedback.

There was also a cost dimension. We were paying for GPT-4 API calls, and longer generations meant more tokens processed. At scale, that was adding up. The tradeoff between accuracy (retrieved context + careful reasoning) and cost/speed (fast generation) was real.

## How We Approached the Fix

We had a few options, none of them clean.

**Option 1: Use a smaller, faster model.** GPT-4 was doing the heavy lifting, but newer smaller models like Claude 3.5 Haiku were competitive. We could swap it in and see if response times improved. Risk: quality might drop.

**Option 2: Optimize the RAG pipeline.** Use a faster embedding model, limit the amount of context passed to GPT-4, parallelize retrieval with generation. Risk: each optimization has tradeoffs.

**Option 3: Accept the tradeoff.** Keep RAG, acknowledge the latency, and find UX workarounds. Maybe show a loading animation, or stream the response token-by-token so it *feels* faster.

**Option 4: Hybrid approach.** Simple questions bypass the full RAG pipeline. Complex ones get the full treatment.

We didn't have time to optimize everything perfectly. We had August left. We couldn't rewrite the whole system.

What we *could* do was understand the tradeoff deeply enough to make informed decisions going forward. We learned that RAG + GPT-4 was powerful but slow. We learned that for tutoring, latency was a feature problem, not just a performance problem. And we learned that sometimes the right architectural choice comes with costs you have to pay.

The slowness taught us something else too: our measurement was incomplete. We'd been optimizing for accuracy and correctness. We hadn't been optimizing for the actual student experience — which includes the time it takes to get an answer.

That realization set us up for the next big question: if RAG was correct but slow, and single-pass GPT-4 was fast but hallucinated, what were we actually optimizing for? What did "good" actually mean?

---

**Next: We started examining what students actually needed from their tutor. That's when we realized the problem wasn't just retrieval — it was teaching.**
