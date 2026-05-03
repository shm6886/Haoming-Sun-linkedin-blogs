# Inheriting a Broken System: Why Single-Pass GPT-4 Tutoring Agents Hallucinate

It was June 2025. We had a mandate: build an AI tutoring agent that could answer student questions across multiple school curricula. No existing system. No foundation to work from. Just a blank canvas and eight weeks to make it work.

The approach seemed straightforward. GPT-4 was the best model available. Call it with a question, get an answer back. Simple, fast, done. We built it and shipped it to over 5,000 students across different schools. By every metric that mattered—speed, cost, whether students were getting answers—it worked.

Then someone filed a bug report.

A student had asked about calculus, something about derivatives. The specifics blur now, but the answer stuck with me. GPT-4 had generated something that sounded authoritative, well-structured, and completely wrong. The formula it invented didn't exist. A student who memorized it would fail their exam.

That's when it became real. This wasn't a technical problem anymore. It was an educational one.

## How Single-Pass GPT-4 Works (and Why It Fails)

To understand why it broke, you have to understand how it was built.

When you send a question to GPT-4, the model processes your prompt through layers of neural networks and generates the next most probable tokens—one at a time. That's it. It's predicting what text should come next, based on patterns from training data. It's not looking anything up. It's not reasoning from a knowledge base. It's statistical pattern matching.

This works great for a lot of things. Write an email? GPT-4 nails it. Explain a concept you've already encountered? It'll do that too. But education is different. When a student asks about a specific curriculum—a particular textbook, a specific chapter, a formula in a specific format—the model is still just making predictions. If the training data had similar questions, it guesses well. If not, it hallucinates. It generates plausible-sounding answers because that's what the patterns predict when it doesn't actually know.

It's like asking someone who's read a lot of calculus books to answer a question without letting them look at those books. They might remember correctly. Or they might confidently invent something that sounds right but is completely wrong.

## Why Hallucinations Destroy Trust in Education

Here's what makes this dangerous in a tutoring context: students trust what you tell them.

When you're a high schooler using an AI tutor, you assume it knows what it's talking about. You're not fact-checking. You're learning. If it hallucinates once, maybe you notice. If it happens twice and you don't catch it? You've learned something false. You've built your understanding on faulty ground.

This isn't just a bad user experience. It's a broken product. The entire value of an AI tutor is that you can trust it. A human tutor might make a mistake, but you can push back—"Wait, that doesn't sound right"—and they'll reconsider. GPT-4 doesn't do that. It commits to its hallucination. It stands by the invented formula with complete confidence.

By the time that bug report hit us, we'd already shipped to thousands of students. How many wrong answers had they memorized? How many would fail exams because they'd learned something false? We didn't know. That uncertainty was the real problem.

## The Moment It Clicked

I remember sitting with the team looking at that calculus response, feeling the weight of what we'd done. We'd shipped something fast. We'd shipped something that looked like it was working. But we'd shipped something fundamentally broken.

This was June. We had July and August.

The question wasn't "how do we make single-pass GPT-4 better?" The question was "how do we rebuild this so hallucinations stop being possible?"

That's when someone mentioned RAG. Retrieval-Augmented Generation. The idea was simple: instead of generating from memory, give the model the actual source material first. Let it read the curriculum, then answer. Make hallucinations impossible at the architecture level.

We didn't know if we could pull it off in eight weeks. We didn't know if it would scale. We didn't even know if we had the right tools.

But we knew single-pass GPT-4 wasn't going to cut it.

And in that calculus hallucination, we had our proof.

---

**Next: We redesigned the entire system. Here's what we learned about shipping fast with RAG.**
