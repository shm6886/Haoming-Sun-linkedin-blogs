# What I Learned About LLM Products: Reasoning > Retrieval

August ended, and we shipped. Not because we'd built the perfect system. But because we'd built something that worked, that scaled, and that we understood deeply.

Looking back on three months, the most important thing I learned had nothing to do with RAG or vector databases or eval pipelines. It was something simpler: building a product with AI means obsessing over what actually matters to users, even when it's invisible in the code.

## The Journey in One Arc

Here's how we got here:

We started with single-pass GPT-4 — fast, hallucinating. We switched to RAG — correct, slow. We discovered that correct wasn't enough if it wasn't teaching. We built evals to measure what mattered. And in building those evals, we realized the real challenge wasn't retrieval or generation. It was reasoning.

A tutoring system that's both fast *and* teaches isn't a technical problem waiting for the right architecture. It's a design problem. It requires asking: what does good tutoring actually look like? What are we optimizing for? What are we willing to trade off?

Most LLM products start with a technical decision (use GPT-4, use RAG, use fine-tuning). But the real work is figuring out what "good" means for your users, and then building infrastructure to measure and maintain it.

## Why This Matters Beyond Tutoring

Before this internship, I thought building LLM products was mostly about picking the right model and the right architecture. I was wrong.

Here's what I've learned: the architecture matters, sure. But what matters more is the measurement. Once you can measure what you care about, you can iterate. You can catch problems before they ship. You can make informed tradeoffs.

Any LLM product has this challenge. A customer support chatbot needs to be helpful, not just informative. A content generator needs to match brand voice, not just be grammatically correct. A coding assistant needs to teach, not just generate.

These aren't retrieval problems. They're measurement problems. How do you know your system is doing what you want? How do you catch it when it isn't?

That's the eval pipeline question. And it's bigger than evals.

## The Team Factor

I want to say something about the four other interns I worked with. We were all learning. None of us had built LLM systems before. We made mistakes. We shipped slow code. We over-indexed on metrics that didn't matter.

But we also had something valuable: we all cared about the product. We weren't chasing the coolest architecture. We were chasing something that actually worked for students.

That mattered more than any individual decision. We'd debate about Chroma vs. other vector databases, about chunk sizes, about prompt strategies. But underneath all those debates was a shared obsession: what do students actually need?

That alignment — around purpose, not just implementation — is what made the difference. It's also what's hardest to hire for or build into a team. But it matters.

## What I'd Do Differently

If I built this again, knowing what I know now:

**I'd measure earlier.** We didn't build the eval pipeline until halfway through. If we'd started with "here's what good looks like, here's how we measure it," we would have caught the teaching-moment problem earlier and spent less time on the slowness optimization.

**I'd focus less on metrics, more on users.** We spent a lot of time looking at precision, recall, latency. We spent less time watching actual students use the system. Watching a student give up waiting for an answer taught us more than any latency number.

**I'd accept uncertainty longer.** We wanted to be right quickly. But the first month of building, we should have just been exploring. What works? What doesn't? What's surprising? We could have shipped scrappier, learned faster, iterated more.

**I'd start with constraints, not features.** We had 5,000 students and three months. That constraint should have shaped everything. Instead, we over-built in some areas and under-built in others. Constraints force clarity.

## What I'm Watching For Next

I'm starting a new role in September, and I'm thinking differently about LLM systems now.

I'm watching for: What are people actually trying to optimize for? Not in the design docs — in the code. What's the eval suite measuring? What gets shipped without measurement?

I'm skeptical of: Systems that optimize for one dimension (speed, or accuracy, or cost) without thinking about user experience. LLMs make tradeoff decisions invisible. You ship fast code that hallucinates, or slow code that teaches. You need to know which one you chose, and why.

I'm excited about: Teams that measure the hard stuff. Not just "does it work" but "does it do what users need." That's harder to build. But it's the difference between a product and a feature.

## The Real Takeaway

The calculus hallucination that kicked off this journey wasn't actually a failure of GPT-4. It was a failure of architecture. Single-pass generation without grounding is a choice that comes with consequences.

RAG fixed the hallucination, but created slowness. That's not a failure either — it's a tradeoff.

The teaching-moment problem was the deepest one, because we'd missed it entirely. We'd optimized for the wrong thing. That's the risk when you're building in unfamiliar territory.

But that's also why measurement matters. Once you can see the problem, you can fix it. Once you can measure what you care about, you can make real decisions.

Three months in, we'd shipped a system that tutored thousands of students. It wasn't perfect. But it was measured, understood, and defended by automated tests. That's not luck. That's infrastructure.

And infrastructure is everything.

---

**Thanks for following this journey. If you're building LLM products, I'd love to hear how you're thinking about measurement and tradeoffs.**
