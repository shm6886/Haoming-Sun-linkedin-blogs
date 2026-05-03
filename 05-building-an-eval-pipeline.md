# Building an Eval Pipeline: When Automated Testing Catches What Humans Miss

We had a new mandate: measure what we couldn't see. How do you test for "good teaching"? How do you catch regressions in something as fuzzy as pedagogical quality?

The answer wasn't to solve the problem perfectly. The answer was to build infrastructure that would catch problems *before* they shipped.

We built an automated eval pipeline using LangSmith and prompt versioning. Here's why it mattered so much.

## Setting Up the Pipeline

An eval pipeline in the LLM world does what unit tests do in traditional software: it runs a set of test cases, measures the output against criteria, and flags problems.

But LLM evals are different. You can't just check if the output equals an expected value. You need to evaluate along multiple dimensions:

- **Factuality**: Is the answer correct? Does it match the source material?
- **Pedagogy**: Does it explain, or just answer?
- **Clarity**: Is it understandable to a high school student?
- **Completeness**: Does it address the actual question?

We set up LangSmith to trace every API call — every embedding, every retrieval, every generation. We created versioned prompts so we could track what changed between iterations. When we adjusted the system prompt (the instructions we give GPT-4 about how to be a tutor), we'd tag it with a version number. When we tweaked the retrieval strategy, we'd version that too.

Then we built test cases. Real student questions from our logs, each one with:
- The question
- The expected source material
- A reference explanation (what a good answer should look like)
- Evaluation criteria

We'd run new versions against these test cases and measure performance. Did precision go up? Did response quality stay the same while latency improved? Did we break something we didn't notice?

## The Moment It Paid Off

We were experimenting with chunk sizes in Chroma. Larger chunks would provide more context to GPT-4, which theoretically should improve responses. We bumped the chunk size up and let the eval pipeline run.

The results were brutal. Precision on our test set dropped from 0.79 to 0.61. Nearly 20 points.

We hadn't noticed this in manual testing. The responses *looked* reasonable. But the eval caught it: more context was actually making the system worse. The larger chunks were introducing noise — irrelevant information that was confusing the model instead of helping it.

Here's the thing: we caught this *before* shipping to students. If we'd just pushed that change to production, we would have degraded the system for thousands of learners, and we might not have noticed for weeks. But the eval caught it in hours.

That moment sold the entire team on automated evals. It wasn't a nice-to-have. It was a safety net.

From that point on, every prompt change, every architectural tweak, every configuration update went through the eval gate first. The pipeline became our QA process. It wasn't perfect — evals have their own blind spots — but it was infinitely better than shipping changes blind.

## Why Automation Matters at Scale

The deeper insight here is about scale. When you're shipping to 5,000 students, manual testing doesn't cut it anymore. You can't have a human manually check every permutation of your system. You can't spend hours QAing a one-line prompt change.

But you can write an eval. You can specify what good looks like. You can run it in minutes. You can catch regressions before they become incidents.

We were five interns. We didn't have QA teams or extensive manual testing workflows. The eval pipeline was our substitute for that. It was automation doing the work that humans couldn't scale to.

And it democratized our ability to make changes. Any of us could propose a system improvement, run it against the eval suite, see the results, and know whether we'd broken something. That speed mattered.

## The Broader Lesson

Building evals taught us something important about LLM systems: they're not like traditional software. You can't unit test your way to quality. But you *can* build evaluation infrastructure that catches problems at the boundaries — where your changes meet reality.

The eval pipeline wasn't perfect. It had blind spots. There were categories of failure it couldn't measure. But it was measurable, it was automated, and it caught real problems. That was enough.

By August, our eval pipeline had caught several regressions, prevented multiple bad deployments, and become the thing we trusted most about our development process.

---

**Next: Three months in, what did all of this teach us about building LLM products?**
