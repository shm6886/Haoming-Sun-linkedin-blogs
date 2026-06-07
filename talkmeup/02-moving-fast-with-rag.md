# Moving Fast With RAG: Redesigning the Tutoring Agent in 8 Weeks

The calculus hallucination had changed everything. We had students trusting answers that sounded right but were factually wrong. We couldn't just patch this. We needed to rebuild.

That's when RAG became our strategy.

Retrieval-Augmented Generation. The idea is elegant: instead of asking GPT-4 to generate from what it knows, give it the actual source material first. Let it read the textbook, then answer the question. Make every response grounded in something real.

We had eight weeks to make it work.

## What RAG Actually Is (And Why It's Perfect for Tutoring)

Here's the difference between how traditional LLMs work and how RAG works.

A standard LLM like GPT-4 generates answers from its training data. You ask a question, it generates the most probable tokens. Everything comes from what it's learned. For specific curricula, newer information, or anything niche, that's just not there. Or worse—the training data has wrong information, and GPT-4 confidently makes something up.

RAG flips this. Two steps:

**Step 1: Retrieval.** Student asks a question. Before we send it to GPT-4, we search a vector database for the most relevant curriculum materials. We find the textbook sections, lesson notes, problem sets—anything relevant. We hand the AI a pile of actual source material.

**Step 2: Generation.** GPT-4 now generates an answer, but it's anchored. It has context. It's reading from the actual curriculum. If the answer isn't in the materials, the model knows to say so instead of making something up.

The payoff for education is huge. Students get source-cited answers. They can click through to the exact textbook section. And hallucinations become much, much harder—you can't confidently invent a formula when you're reading from a calculus textbook.

## Why Chroma, And How We Built Multi-Tenant Isolation

We needed a vector database. Something fast, something that could scale, and most importantly: something that could keep data from different schools completely separate.

We went with Chroma.

Not because it was the fanciest option, but because of one feature: tenant_id filtering. One database, multiple isolated curriculum stores. Each school's data lives in the same Chroma instance, but when a student from School A asks a question, they only get results from School A's curriculum. The isolation happens at the database level—it's built in, not jury-rigged.

Here's how it actually works: when a student logs in, their tenant_id (their school) gets attached to their request. When they ask a question, we search with a filter: `tenant_id == their_school_id`. Only their curriculum comes back. That meant we could serve 5,000+ students across different schools without managing separate databases for each one. One database. Complete isolation. Clean.

Getting this working took longer than we expected. Vector embeddings, metadata filtering, making sure the isolation held under load—there were nights of debugging that blurred together. But by late July, we had it. We could embed curriculum documents, tag them with tenant metadata, and retrieve only the right stuff for each student.

## Shipping v1: The Moment of Truth

When we deployed the first version, something shifted. The system returned source-cited responses. Students could see exactly which textbook section the answer came from. Hallucinations didn't disappear—LLMs are still LLMs—but they became rare. And when they happened, they were grounded in actual source material, which made them more detectable.

We shipped to the same 5,000+ students. This time, serving source-grounded answers across isolated curricula. Every response had citations. It felt like a real product.

The metrics looked good. Response quality went up. Students were clicking through to source materials, which meant they actually trusted the answers enough to verify them. Teachers started noticing the difference. For the first time, the system felt trustworthy.

Then we hit something we didn't expect.

The system was working, but it was slow. Not broken-slow, but noticeably slow. Students were waiting longer for answers. The RAG pipeline—embedding the question, searching the vector store, generating the response—was taking too long. What we'd gained in accuracy, we'd lost in speed.

We hadn't expected that tradeoff to cut so deep.

---

**Next: We dug into the slowness. Here's what we found, and what it taught us about RAG systems.**
