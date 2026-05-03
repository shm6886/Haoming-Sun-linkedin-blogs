# The Slowness Problem: Diagnosing RAG Bottlenecks

In real-life applications, performance issues don't break anything abruptly; they gradually degrade user experience to a level where it is no longer usable. That's why it was challenging to detect any issues with RAG integration until students stopped using it actively.

The new system became less popular, receiving fewer repeat questions. Also, more students faced timeout errors. While the new system still provided technically correct answers, the response time significantly increased, making the service less efficient.

Measuring Response Time

Before adding RAG, a single-pass GPT-4 system took about 2-3 seconds to provide answers to questions. The new system required 8-12 seconds, which is a noticeable improvement for students expecting a prompt reply. The difference in response time was significant enough to influence their decisions and behavior.

## Understanding the Bottleneck

The RAG pipeline consists of three primary steps that require some time:

- Question embedding. The system converts students' queries into vectors and sends them to the vector database.

- Materials retrieval. The Chroma model searches through the curriculum dataset and returns the closest vectors.

- Response generation. GPT-4 uses the retrieved materials and creates an appropriate response.

Our analysis demonstrated that step 3 accounts for most of the time taken by the system to generate answers. On average, the question generation step requires 6-10 seconds, and both the previous stages combined take only about one second.

It becomes evident after observing the GPT-4's response creation mechanism. To provide relevant responses, the model reads materials and generates tokens sequentially, analyzing their meaning and trying to produce an accurate and insightful answer. The process is time-consuming.

Why Latency Mattered More Than We Thought

Unlike in other applications, where latency primarily influences user convenience, it significantly changes user behavior in educational products. In this case, the time difference between two-second and twelve-second responses drastically affects the interaction pattern.

A 2-second reply can make students pause for a couple of minutes and wait for an answer. However, if the response takes 12 seconds, users will likely close the tab and use another tool to solve the problem. As a result, the tutoring service would be a secondary choice.

From our perspective, the situation was critical for several reasons. First, we were losing access to data since we could only observe students who were patient enough to wait. Those who switched to other methods or abandoned the problem left us without information regarding their difficulties.

Moreover, there were financial considerations. In our case, the system paid per call, and a larger number of tokens resulted in higher fees. Thus, we were forced to make trade-offs between accuracy and latency.

Potential Solutions

We had four options for improving the pipeline's performance.

- Option 1: Changing the architecture. We could replace GPT-4 with a smaller and faster model like Claude 3.5 Haiku. The experiment would help determine whether there were any significant improvements, although we would lose some quality.

- Option 2: Optimizing RAG. Using another embedding model and limiting the amount of context given to GPT-4 could accelerate the pipeline. Parallel execution of retrieval and generation processes would reduce the time as well. However, each approach had drawbacks.

- Option 3: Accepting the trade-off. Instead of changing the architecture, we could accept that RAG with GPT-4 was slow. Then, we should optimize the UX and use some tricks, such as loading animations, to mitigate the effects.

- Option 4: Developing a hybrid system. Simple questions would skip the entire pipeline, while complex questions would receive a full response. Nevertheless, we didn't have sufficient time to implement every change and refine our system before August.

While we couldn't resolve all issues, we managed to identify critical factors influencing our decision-making. In particular, we discovered that RAG and GPT-4 combination was a powerful yet inefficient solution. For tutoring platforms, latency was a design problem, not a performance one. In other words, choosing the best architecture often comes at a price.

The slowness of our system revealed yet another issue – the lack of data. While we optimized our system's accuracy and correctness, we failed to consider the impact of response time on student performance. It became evident that the system required some improvements.

As a result, we decided to examine student needs, which brought us to another discovery.
