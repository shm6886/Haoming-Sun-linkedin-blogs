Hallucinations of a Single-Pass AI Tutoring Agent Based on GPT-4: Lessons Learned

June 2025. We had a project – creating a tutoring AI, which would be able to answer questions of students from multiple educational programs within any school curriculum. We had no ready-made solution to use – we were starting from scratch. The deadline was eight weeks.

The decision seemed straightforward. Using GPT-4 as our language model, we decided to create an application sending a prompt and receiving the answer via the API. One pass and go. It took us a month to build it and test it. At last, the tutoring AI worked for more than 5,000 students. According to our metrics, everything was okay: fast replies, acceptable API costs, students get the replies to their queries.

That's when a bug report came into our inbox and everything changed.

A student asked about calculus with derivatives. I forget the details. What I remember perfectly – the answer of our system. It looked very official and structured; yet, all the formulas it used were nonsense. There was no such formula. Even if a student would try to remember it before the exam, he wouldn't be able to provide a good result. We went from a tech error to a serious educational mistake.

What Is a Single-Pass AI Tutor Based on GPT-4 (And How Does It Fail)?

To understand why this happened, let's explore the principle behind the application.

The prompt of the user enters the pipeline and becomes a text processed by GPT-4 algorithm. The model takes the prompt and produces a reply one word at a time – based on statistical data received from the training process. There's no search in the database. There's no reasoning. The model just uses probability theory to generate the most likely text it can.

The algorithm does pretty well for a variety of tasks. It may write an email letter, it may explain some concepts, it may produce text in a required format. But this kind of AI application is never perfect for education, because when it comes to a particular topic of a school curriculum, it has to guess what it needs to generate. It does pretty well if there's such a pattern in its training data. Otherwise, it starts generating some random phrases sounding like truth.

It is like asking someone who is well-read on some scientific topics to give an answer without books. The person may repeat something he remembers and gets it right. But there's always the risk of him making something up.

Why Hallucinations Are Problematic in Education

It was especially bad in case of our tutoring application, as students would accept the replies coming from the system as correct. There would be nothing fact-checked, and even if the system hallucinated once and the student noticed the mistake, he wouldn't know if the second reply to his question is going to be correct. It would become a problem of product design – not UX.

By this moment, the bug report made us realize that our tutoring system is unreliable. We couldn't continue to support it, even though everything seemed okay according to our metrics. Students would receive incorrect information on the subjects they were studying. No matter how confident in their abilities they would feel, all their work would be ruined due to errors made by AI. And we couldn't fix that with anything except rewriting our whole algorithm.

Pivot Point

Looking now on that report and the calculus formula generated, I realize that the moment was crucial for us. We had eight weeks to deliver something – and we did that. But we failed to create a product. We created something which could have been a problem for students.

At this moment, we started thinking differently. We still had two months, and while the deadline became irrelevant to us, the goal remained the same – create a tutoring AI, which wouldn't generate any false statements.

We considered RAG – Retriever-Augmented Generation algorithm, which uses source documents from the real world for generation process. This meant that hallucinations couldn't happen anymore. The answer could be provided by the system in full reliance on the actual facts from textbooks or other sources.

We didn't know whether it was possible to achieve it in eight weeks, and whether the algorithm would cope with over 5,000 concurrent users. Also, we weren't sure whether we had enough skills to use Chroma, Langsmith, and our new technological environment properly. But we knew that a single-pass AI based on GPT-4 was not the solution.
