# Teaching Moments, Not Just Answers: Why Retrieval Isn't Enough

We found ourselves at a crossroads. While RAG was slower than expected, it was right. Single-pass GPT-4 was faster, but it hallucinated. We solved one problem and introduced another.

But then we received a new type of feedback.

The teacher reached out to us and pointed out an interesting pattern she observed in students using the tutoring bot. When a student asked a question and received an answer with a source citation, they were satisfied. Students would read the response, recognize the page number in their textbooks, and move on. There was no learning happening – students simply found the answer to their questions.

In retrospect, this shouldn't have come as a surprise. But it did.

The Distinction Between Answering and Teaching Questions

We failed to recognize one crucial distinction – answering a question and teaching someone how to answer a question are two different tasks.

For example, if a student poses the question "How do I find the derivative of x²?", they may receive two different responses:

Response A: "The derivative of x² is 2x."

Source: Page 47, Math Textbook.

Response B: "According to the power rule, if you have x^n, its derivative equals n·x^(n-1). Thus, the derivative of x² is equal to 2·x^(2-1), which gives 2x. Let me show you why the power rule holds true. When you differentiate a function, you calculate its slope…"

Source: Page 47, Math Textbook.

Response A is correct and accurate. It cites the source and provides a quick reply. If this was the only information a student received, they would simply memorize this fact.

Response B requires more time from the tutor. However, it builds up the logical reasoning process and explains how the student can derive the answer themselves. This approach represents a true tutoring process – it's not enough to provide an answer; a tutor needs to explain how the answer was achieved.

Thus, we had developed a system capable of generating Response A at scale. We did not pay attention to Response B.

The Difference for AI Tutoring Systems in Education

From this experience, I've learned that a tutoring system isn't a search engine. Providing students with the right source and the answer is not the ultimate goal. Tutoring involves reasoning, illustrating, providing examples and counter-examples, verifying knowledge, and adapting to students' needs.

Thus, an artificial intelligence tutor should:

- Understand a student's confusion (not necessarily the initial question),

- Distinguish complex concepts into smaller parts,

- Provide multiple examples and counter-examples,

- Verify the student's comprehension of the subject matter,

- Adapt explanation approaches if necessary.

All this involves generation – intelligent generation that mimics a teacher's explanation skills.

While we can easily incorporate source citations and prevent hallucinations, the resulting content will not serve as a tutorial. Instead, it would amount to a simple copy-paste operation (Ctrl+F).

The Discovery That Shaped Our Work

I recall discussing this teacher's feedback with our colleagues. We looked at examples of answers generated with RAG technology and compared them to what a human tutor would say. The difference was obvious.

Our focus on reliability and speed misled us into optimizing the wrong metric. Although we ensured that our system was more credible by anchoring its responses to sources, we failed to teach students. In our case, we traded education for trustworthiness.

Now, we faced a new challenge – how could we evaluate the effectiveness of our tutoring system? While we can easily measure hallucinations and latency, evaluating the learning impact of the response remains an open question.

This problem motivated our next move. We needed a solution that would allow us to estimate the quality of teaching content. To prevent regressions in factual accuracy, we also needed to ensure that the system would continue delivering reliable answers.

We required an evaluation pipeline.

---

**Next: Here's how we built automated testing for something we didn't even know how to measure.**
