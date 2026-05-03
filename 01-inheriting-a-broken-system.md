# Inheriting a Broken System: Why Single-Pass GPT-4 Tutoring Agents Hallucinate

It was June 2025, and we had a mandate: build an AI tutoring agent that could answer student questions across multiple school curricula. No existing system. No foundation to build on. Just a blank canvas and a summer to ship something.

The choice was obvious. GPT-4 was the best language model available, and the approach was straightforward — call the API with a student's question, get back an answer. Single-pass. Fast. Done. We built it and deployed it to 5,000+ students across different schools. By all metrics, it was working: response times were quick, the API calls were cheap, and students were getting answers.

Then the bug report came in.

A student had asked about calculus — something about derivatives, the specifics don't matter anymore. GPT-4 generated an answer that sounded authoritative, well-structured, and completely wrong. The formula it invented didn't exist. A student who trusted the system and memorized it would fail their exam. And that's when the problem became visceral: this wasn't a technical failure. It was an educational failure.

## How Single-Pass GPT-4 Works (and Why It Fails)

Let me explain what we built, because understanding the mechanics is the only way to understand why it broke.

When you send a question to GPT-4, here's what happens: the model ingests your prompt, processes it through layers of neural networks, and generates the most probable next tokens — one token at a time. It's generating based on patterns it learned during training. Nothing more. It's not looking anything up. It's not reasoning from a knowledge base. It's predicting what text should come next based on statistical patterns.

This is powerful for a lot of things. Write an email? GPT-4 will nail it. Explain a concept you've seen before? It'll do that too. But here's the thing about education: when you're asking about a specific curriculum — a particular school's textbook, a specific chapter, a formula in a specific format — the model is still just predicting the most probable continuation of your prompt. If the training data contained similar questions, it'll guess well. If not, it'll hallucinate. It'll invent plausible-sounding answers because that's what tokens predict.

It's like asking someone who's read a lot of calculus books to answer a calculus question without being able to look at the books. They might get it right if it's something they remember. They might also confidently invent a formula that sounds right but is completely wrong.

## Why Hallucinations Destroy Trust in Education

Here's what makes this uniquely dangerous in a tutoring context: students trust authority. When you're a high schooler using an AI tutor, you assume the answers are right. You're not fact-checking GPT-4's calculus formulas — you're learning from them. If the system hallucines once, maybe you notice. If it happens twice and you don't catch it? You've learned something false. You've built your mental model on sand.

This isn't just a UX problem. It's a product problem. The entire value proposition of an AI tutor is that it's reliable. A human tutor can hallucinate too, but if you push back, they self-correct. "Wait, that doesn't sound right" — and they reconsider. GPT-4 doesn't do that. It commits to its hallucination. It stands by the invented formula with the same confidence it would stand by a real one.

By the time that bug report landed, we'd shipped to thousands of students. How many wrong answers had they already memorized? How many would fail an exam because they learned something false? We didn't know. And that uncertainty was the real problem.

## The Moment It Clicked

I remember sitting with the team, looking at that calculus response, and feeling the weight of it. We'd built something fast. We'd shipped something that worked *on the surface*. But we'd shipped something fundamentally broken in a way that mattered more than speed or cost.

This was June. We had July and August left.

The question wasn't "how do we make single-pass GPT-4 a little better?" It was "how do we fundamentally change the architecture so that hallucinations stop being possible?"

That's when someone mentioned RAG. Retrieval-Augmented Generation. The idea was simple: instead of generating from memory, give the model the actual source material first. Let it ground its answers in real curriculum data. Make hallucinations structurally impossible.

We didn't know if we could pull it off in eight weeks. We didn't know if the architecture would perform well enough for 5,000 concurrent students. We didn't even know if we had the right tools — Chroma, Langsmith, the whole stack was new to us.

But we knew single-pass GPT-4 wasn't the answer.

And in that calculus hallucination, we had the proof.

---

**Next: We redesigned the entire system with retrieval-augmented generation. Here's what we learned about shipping fast with RAG.**
