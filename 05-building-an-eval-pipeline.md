# Building an Eval Pipeline: When Automated Testing Catches What Humans Miss

We had a new mandate: measure what we couldn't see.

How do you test for "good teaching"? How do you catch regressions in something as fuzzy as pedagogical quality?

The answer wasn't to solve it perfectly. The answer was to build infrastructure that caught problems before they shipped.

We built an automated eval pipeline using LangSmith and prompt versioning. Here's why it mattered so much.

## Setting Up the Pipeline

An eval pipeline in the LLM world does what unit tests do in traditional software: runs test cases, measures output against criteria, flags problems.

But LLM evals are different. You can't just check if the output equals an expected value. You need to evaluate across multiple dimensions:

- **Factuality**: Is the answer correct? Does it match the source material?
- **Pedagogy**: Does it explain, or just answer?
- **Clarity**: Is it understandable to a high school student?
- **Completeness**: Does it actually address the question?

We set up LangSmith to trace every API call—every embedding, every retrieval, every generation. We created versioned prompts so we could track what changed between iterations. When we adjusted the system prompt (how we tell GPT-4 to be a tutor), we'd version it. When we tweaked the retrieval strategy, we'd version that too.

Then we built test cases. Real student questions from our logs, each with:
- The question
- The expected source material
- A reference explanation (what a good answer should look like)
- Evaluation criteria

We'd run new versions against these test cases and see what changed. Did precision go up? Did quality stay the same while latency improved? Did we break something we didn't notice?

## The Moment It Paid Off

We were experimenting with chunk sizes in Chroma. Bigger chunks would give GPT-4 more context, which should theoretically improve responses. We bumped it up and ran the eval.

The results were brutal. Precision dropped from 0.79 to 0.61. Nearly 20 points.

We hadn't caught this in manual testing. The responses looked fine. But the eval caught it: more context was actually making things worse. The larger chunks were adding noise—irrelevant information that was confusing the model instead of helping it.

Here's the key: we caught this before shipping to students. If we'd pushed that change to production, we would have degraded the system for thousands of learners, and we might not have noticed for weeks. The eval caught it in hours.

That moment sold the entire team on automated evals. It wasn't optional. It was a safety net.

From that point on, every prompt change, every architecture tweak, every config update went through the eval gate first. The pipeline became our QA. It wasn't perfect—evals have blind spots—but it was infinitely better than shipping changes blind.

## Why Automation Matters at Scale

The real insight here is about scale. When you're serving 5,000 students, manual testing doesn't work anymore. You can't have a human manually check every variation of your system. You can't spend hours QAing a one-line prompt change.

But you can write an eval. You can specify what good looks like. You can run it in minutes. You can catch regressions before they become incidents.

We were five interns. No QA team. No extensive manual testing workflows. The eval pipeline was our substitute for all of that. It was automation doing the work that humans couldn't scale to.

And it democratized our ability to make changes. Any of us could propose a system improvement, run it against the eval suite, see the results, and know whether we'd broken something. That speed mattered.

## The Broader Lesson

Building evals taught us something important about LLM systems: they're not like traditional software. You can't unit test your way to quality. But you can build evaluation infrastructure that catches problems at the boundaries—where your changes meet reality.

The eval pipeline wasn't perfect. It had blind spots. There were categories of failure it couldn't measure. But it was measurable, automated, and it caught real problems. That was enough.

By August, our eval pipeline had caught several regressions, prevented multiple bad deployments, and become the thing we trusted most about our development process.

---

**Next: Three months in, what did all of this teach us about building LLM products?**
