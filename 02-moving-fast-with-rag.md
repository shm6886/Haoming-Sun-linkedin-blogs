# Moving Fast With RAG: Redesigning the Tutoring Agent in 8 Weeks

Students were too heavily dependent on the tutor's fictional answers, proving that GPT-4 isn't always reliable. And we faced a serious issue that demanded us to redesign the entire architecture of the solution.

That is why we chose RAG — Retieval-Augmented Generation.

The algorithm is straightforward: don't allow GPT-4 to rely exclusively on its training. Provide the model with the necessary source material (give it to read relevant parts of textbooks and produce an answer based on that). The idea was to reduce the number of hallucinations as much as possible.

That sounds pretty promising. We had only 8 weeks to create a prototype.

What is RAG and Why Does It Suit Our Purpose Well?

Here is the process description and an explanation of why RAG suits our purposes perfectly.

While pure generator architectures such as GPT-4 have only generation in their pipeline, RAG consists of two stages. At the first step, a student asks the agent a question. In order to answer it, the AI is provided with the sources to the questions (relevant textbook passages). That is, the algorithm is aimed at minimizing hallucinations.

The next step is obvious: GPT-4 produces an answer basing on all the provided materials. If GPT-4 cannot find the answer, it tells us so.

In an educational environment, the benefits of RAG are obvious: students receive answers with citations. A simple click on the source allows the user to go directly to the related passage, thus ensuring that there are almost no hallucinations since the answer comes from the book.

Why Chroma and Multitenant Isolation?

We needed a fast and scalable vector database that would provide multitenancy isolation.

And so, we choose Chroma.

Our choice wasn't based on the availability of fancy tools. With Chroma, we got exactly what we needed — several isolated curriculum stores in the vector database. Our goal was to store curriculum material of numerous schools in one vector database and support requests with tenant ID metadata for isolating data per tenant.

Here is how we made it work. When a student logged in, their tenant ID (their school) was attached automatically. When making requests, the system searched for the relevant documents using the metadata: tenant ID = their_school_id. Thus, the results belonged only to the particular curriculum material of one specific tenant.

So, we could easily store thousands of documents from many schools in one database. We scaled from one student to five thousand in no time and with no extra efforts using only one Chroma instance. And all that stayed isolated.

Creating this pipeline was a challenge. We had to work with vector embeddings, filter the content based on metadata and ensure isolation under the high load — lots of debugging had to be done. Nevertheless, in mid-July everything finally started working. We embed documents with tenant ID metadata and retrieve the best matches for each request.

Shipping v1: One Step Further

This was a milestone release.

For the first time, students received answers with citations. It allowed users to click the links and directly open the textbook sections that contained answers to their questions. While some hallucinations still appeared in our application, their number decreased dramatically. Hallucinations occurred rarely and were always grounded on some real material.

Releasing a system to thousands of active users, allowing students to get answers with citations and storing their curriculum materials in one vector database was a huge step for us as a team. Product development came to the end.

Metrics proved that the quality of answers improved compared to the previous version. The fact was supported by the increased number of clicks on citation links, which proves the increased user trust towards the system. The improvement was noted even by educators who made their comments. This was the first time that the solution got credibility.

Unfortunately, there was one serious problem.

The system became noticeably slower. Speeding down the application, we exchanged that for quality. While the speed remained acceptable, we should make some improvements in order to increase it.

Unexpectedly tough trade-off.

To be continued...
