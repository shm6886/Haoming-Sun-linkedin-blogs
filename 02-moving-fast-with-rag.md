# Moving Fast With RAG: Redesigning the Tutoring Agent in 8 Weeks

The calculus hallucination had shattered our confidence in single-pass GPT-4. We had students trusting answers that were statistically plausible but factually wrong. The fix wasn't going to be incremental. It was going to be architectural.

That's when RAG — Retrieval-Augmented Generation — became our North Star.

The idea is elegant: instead of asking GPT-4 to generate answers from its training data, give it the actual source material first. Let it read the textbook, then answer the question. Make it ground every response in something real. Make hallucinations structurally impossible.

Simple in theory. We had eight weeks to implement it.

## What RAG Actually Is (And Why It's Perfect for Tutoring)

Here's how RAG works, and why we knew it was the right move.

A traditional LLM like GPT-4 is a generative model. You ask it a question, and it generates the most probable next tokens. Everything comes from its training data — which, for newer information, specific curricula, or niche topics, is just not there. Or worse, the training data contains wrong information, and GPT-4 confidently hallucinates.

RAG flips this. Instead of pure generation, you have two steps:

**Step 1: Retrieval.** A student asks a question. Before sending it to GPT-4, we search a vector database for the most relevant curriculum materials. We find the textbook sections, lesson notes, problem sets — whatever is relevant to that question. Think of it like handing the AI a stack of research papers and saying, "Answer based on these."

**Step 2: Generation.** GPT-4 now generates an answer, but it's grounded. It has context. It's reading from the actual curriculum. If the answer isn't in the materials, the model knows to say so instead of inventing something.

The payoff is enormous for education. Students get source-cited responses. They can click through to the exact textbook section the answer came from. And hallucinations become much, much harder — you can't confidently invent a calculus formula if you're reading from a calculus textbook.

## Why Chroma, And How We Built Multi-Tenant Isolation

We needed a vector database. Something fast, scalable, and most importantly: something that could handle multiple schools with completely isolated data.

We chose Chroma.

The reason wasn't because it was the fanciest or the most feature-rich. It was because tenant_id filtering gave us exactly what we needed: one database, multiple isolated curriculum stores. Each school's data lives in the same Chroma instance, but a query from a student in School A will never return results from School B's curriculum. The filtering happens at the vector store level — it's built in, not bolted on.

Here's how it works: when a student logs in, their tenant_id (their school) gets attached to their request. When they ask a question, we search for relevant vectors with a filter: `tenant_id == their_school_id`. The retrieval only returns materials from their curriculum. This meant we could serve 5,000+ students across different schools without managing separate databases for each one. One Chroma instance. Complete isolation. Efficient scaling.

Implementing this took longer than we expected. Vector embeddings, metadata filtering, ensuring the isolation held under load — there were nights of debugging. But by late July, we had it working. We could embed curriculum documents, store them with tenant metadata, and retrieve only the right materials for each student.

## Shipping v1: The Moment of Truth

When we deployed the first version of the RAG system, something shifted. The system now returned source-cited responses. Students could see exactly which textbook section their answer came from. The hallucinations didn't vanish completely — LLMs are still LLMs — but they became rare. And when they happened, they happened in the context of actual source material, which made them more detectable.

We shipped to the same 5,000+ students. This time, though, we were serving source-grounded answers across isolated curricula. Every response included citations. It felt like a real product.

The metrics looked good. Response quality improved. Students were clicking through to source materials, which meant they trusted the answers enough to verify them. Teachers started noticing the difference. For the first time, the system felt trustworthy.

Then we hit a wall.

The system was working, but it was *slow*. Not catastrophically slow, but noticeably slow. Students were waiting longer for answers. The RAG pipeline — embedding the question, searching the vector store, generating the response — was taking too long. What we'd gained in accuracy, we'd lost in speed.

We hadn't expected that tradeoff to be so steep.

---

**Next: We dug into the slowness. Here's what we found, and what it taught us about RAG systems.**
