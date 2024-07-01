# daert-stack
Stack to deploy LLM based applications in production

# Introduction
The AI Ecosystem is evolving at a much faster pace in the last years; the appearance of the transformer in 2017 has brought a lot of attention (no pun intentend ;)) to the field.

By scaling the amount of compute and data, a new kind of models is emerging; such models could be classified under the category of AGIs (Artificial General Intelligences); even though it seems
like the holy grail, the capabilities that emerge in such models by just giving instructions and examples in a prompt make them generalist entities.

Even though such power is at the reach of hand of everyone thanks to the development of open source models, yet it seems hard to exploit the value that can be generated by such models.

Several business models and use cases are emerging, yet there is lack of a clear map of ideas in terms of the needed components in the ecosystem to bring the LLMs capabilities to a new
way of doing things.

The appearance of proprietary API based models is easing the "goto market" for many enterprises, by prompting such models with different intents and prompts; somehow, there is a need to bring
all the technologies that are emerging in the field into a unified open architecture.

The greatest effort in these terms are the ideas devised by a16z (https://a16z.com/emerging-architectures-for-llm-applications/); a source from which Daert starts adding other components to the equation
to bring the value of 2024s developments in the LLM ecosystem.

At a glance, the proposed "daert-stack" seems complex and overengineered, somehow there is a certain feeling that depending on the use case, the stack can be simplified: its modular design makes it easy
to remove components and add new ones as time and needs evolve.

It can be acknowledged that having a fully integrated platform will require development efforts from the community, as the interaction of many of such components is not trivial or well-defined.
In addition, multiple solutions that are in the same group in our proposed architecture may be redundant, or provide features that are lacking in others; the main goal at this stage is to define
an open architecture, and iteratively define production grade infrastructure.

The colors and location of the different groups of components is arbitrary, future iterations may benefit from organizing them in a more suitable way.

# High Level Architecture
- LLM Generation: makes the core of the solution, even though plenty of platforms to run LLMs have appeared, Ollama, LLaMA.cpp and Text-Generation-Inference (TGI) are the more production ready solutions. In these terms, there is no need of further development, as the solutions are meant to be running in a production environment.
- Data Loaders: LLMs need data to extract value for user interactions; in a local environment such data can be given by the user in an easy manner by drag and dropping files into Web Interface; somehow in the enterprise ecosystem such data may come from a Data Lake, Data Silos, Data Pipelines or other 3rd party tools. Airbyte seems to be a well developed solution in the ecosystem.
- RAG: Retrieval Augmented Generation could be one field of study in the LLM ecosystem; somehow, if seen as a component of a whole ecosystem, it can be modularized into different parts: rankers, chunkers, retrieval engines... It seems like a mistake to consider RAG as the main component in the LLM Ecosystem; as its capabilities are just meant to find best pieces of data previously loaded and indexed.
- Vector DB: prior to retrieving data by the RAG modules, there is a need to vectorize and index it. In these terms, different platforms have emerged, at this date pgvector has seem a further improvement in terms of performance with respect the alternatives; somehow, Weaviate seems to survive the test of time being easy to use with local LLMs.
- Data Embedding: in order to add data to the vector DB, it needs to previously be vectorized, such vectorization is done by an embedding model; in this case it is better to embed data with encoder-based transformers than to do so with decoder-only LLMs. BERT-based models are a good solution in most of occasions, with a notable mention to matryoska embeddings.
- Evaluation: in addition to generating data with an LLM, routing its generation and the available data in the system; there is a need to evaluate the generations in different manners: multiple metrics exist from the state of the art to compare generations like "perplexity", or model generated metrics that rank generations binary on in a multiple class manner (NSFW, safe, gore, violence, hate speech...).
- Chatbot: a human user may be the end user of the system (even though there is a rapid development of LLM based systems meant to be used by other programs and systems). In such case, there is a need to run the chatbot through an interface, a common one is Telegram for bot deployment.
- Prompt Optimization: the user may be lazy during the usage of the chat interface, because of that, there is an opportunity to boost the performance of the model by trying to optimize the prompt; by either extending the In-Context examples, or by rephrasing and enriching the provided prompt. In these terms Outlines and DSPy seem like great frameworks.
- Code Interpreter: there will be many occasions in which the LLMs are asked to generate reports from data; such reports are easier to be created if the model is prompted to write code to achieve the task, in these terms there is a need to develop a mechanism that will run LLM generated code in a secure environment, for this case baby-code seems like a good starting point.
- Context Management: the LLM will get the whole story of the conversation unless there is a mechanism to clean it. There is an opportunity to perform this context management by using another LLM, which is prompted to create summaries, choose the most important parts of the chat history, and that discards the uneeded parts.
- Planning: early days development of applications on top of LLMs (mainly GPT3) covered prompts to create plans in order to accomplish a certain task by the LLM. Multiple works have been developed in the field, some of them focused on planning for robotics, videogames... somehow one of the most remarkable works is from Yohei (https://a16z.com/emerging-architectures-for-llm-applications/): BabyAGI.
- Web Search Index: in order to feed LLMs most recent data and avoid hallucinations from information that they learnt during training, there is a need to use Web Indexes and make searches that are related to the user query; in these terms, there are plenty of frameworks that aid in the process of crawling, scraping and indexing the Internet: Solr, Nutch, Elasticsearch, Lucene, Tika.
- Crowd Simulator: multiple tests and experiments have been performed regarding the interaction between multiple LLMs in an environment: either giving them different roles and asking for collaboration, or by launching multiple agents and routing their generations to other agents. In these terms, AgentVerse is one of the most developed solutions.
- Multimodality: one of the latest developments towards AGI is the inclusion of other data mediums apart from text, like images, video, music, and speech. In this case, we consider the need of inclusion of TTS (Text to Speech) and (STT) Speech to Text for such cases in which the user speaks to the LLM, and Next-GPT or similar frameworks for cases in which there is a need to generate text, video or music as output.
- Function and tool Calling: another field of study that could be studied on its own. The use of external tools and calling code functions to comple user tasks make the LLM a central processing unit for the AI. In this case multiple LLMs have been trained to find out which tool to use to complete a user request. Notable examples: Command R models, DBRX or models trained by Mistral are rich in making function and tool use decisions.
- Generation Schedulers: possibly one of the least known pieces of the puzzle; somehow recent studies are suggesting the importance of the algorithm that chooses the next token to be generated by the LLM. Traditionally, the greedy algorithm is used: choose the most probable token (highest probability). Somehow the use of MCTS is likely behind achieving better generations in code and math use cases.
- Context Extension: LLMs will produce output with coherence up to a certain number of tokens; such number of tokens is usually related with the length of text that the LLM has been trained on. There are certain strategies to easily extend the LLM context window like YARN or RoPE.
- LLM Surgery: one could consider targetting LLMs to the use case of generating certain recommendations or advertisements for the user; in such cases there is a need to produce LLM surgery; which basically requires changing model weights to activate in a certain way under certain prompts to generate the desired output. EasyEdit is one of the best frameworks to use.
- Expert Systems: one of the least explored interaction of LLMs with other components; somehow the Expert System capabilities to run a logic engine and apply rules to generate outcomes may aid in the process of running logic programs and making conditionals inside the prompt. CLIPS is likely one of the most used Expert Systems nowadays.
- Knowledge Graphs: as the model consumes data from other sources and generates output, there will be interactions between the different entities in the context of the LLM; in some way it may be of use for letting the LLM draw relationships between different components that are not closely related. In this manner Neo4j is one of the most widely used graph solutions.
- LoRA Orch.: LoRAs have become one of the best ways of teaching the model something new without affecting the performance of the rest of the model. In this manner, there may be different LoRAs trained for different purposes, because of that and depending on the use case of the user there is a need to have a fast way of dispatching these LoRAs to the different user without the needs of loading and offloading the model weights. Lorax is one of the proposed solutions to achieve it.
- LLM Chaining: one of the classic fields in the LLM ecosystem; in this manner, LLMs are called with different prompts and templates with the objective of creating data pipelines in which different parts of the generation are completed by different models or prompts. Langchain, Semantic Kernel are some of the frameworks.
- Hallucination engines: hallucination is usually seen as something bad for the LLM, as usually implies the generation of lies, misinformation or wrong content by the LLM... somehow recently some use cases are getting the benefits of hallucinations and exploiting them, some of such are: websim, worldsim and the use of control vectors (control vectors are usually added to the model to achieve different personalities in the LLM generation).
- LLM Cache: as the LLM generates content in time, there will be occasions in which it is interesting to keep the contents of previous generations in time; a way to keep them is the usage of a cache. One of the best choices for a cache in production is Redis.
- Agents: are common in the LLM literature; they cover multiple use cases, somehow it is expected that as the technology matures they will be categorized into different classes. In these terms, the Daert Stack considers Agents a collection of prompts, code and LLM chaining frameworks that have the objective of finishing a well defined task, and that such results will be taken by another agent down the road for more interaction with the LLM. CrewAI is one of the best choices in this case.
- Mechanistic Interpretability: LLM generations need to be studied and monitored to better understand how the different generations are possible: by recognizing patterns in the generations of different layers it is possible to evaluate how different guardrails can be applied to avoid jaiblreaks. This field is slowly developing, at the time of writing the most common mechanism of interpreting the LLM generation circuits is the use of Sparse Autoencoders.
- LLM Task Orchestrator: there is a need of having a centralized and robust mechanism of launching the LLM pipelines in production; in this case Airflow can be used, due to its production readiness.
- Task Scheduler: as already mentioned, there is a need for using LLMs for planning and generating strategies. Such strategies will usually be defined in the shape of a list. Such tasks need to be performed by an LLM or external tool, in order to do so, a task scheduler is needed. In this case, one could create DAGs for Airflow on the go or create a simple stack based scheduler.
- UI Automation: in order to let the AI make use of the platform where it is deployed and perform tasks of use, there is a need to automate the usage of the User Interface: either if it requires clicking on a Webpage or the Desktop UI. In these terms it is of notable mention frameworks like UI.Vision, SAM for web interfaces (Segment Anything Model) or selenium to automate the clicks and fill ins of web page forms and buttons.
- Hyperparameter Optimizing: multiple hyperparameters can be tuned during the LLM generation, some of them will make the model to perform better under different use cases, because of that it may be of interest collecting results of generating output by the LLM under different prompts and such hyperparameter values. In this case, mlflow, ludwig or weights and biases; even though these solutions may be more prone to be used for finetuning and training, they can serve the purpose of evaluating the inference process.
- Prompt Management: as new use cases for LLMs appear, and new prompts are developed and optimized, there is a need to keep track of changes along different versions of the same prompt; as well as comparisons on how a certain prompt improves the performance in different LLMs. For this use case tools like PromptFoo could be helpful.
- Validation: a malicious user could attempt the model to generate harmful content or provide answers to questions that are out of the safety regions; because of that guardrails should be added to the LLM generation process to avoid the user from seeing such harmful content. Nemo and Guardrails are two examples of such frameworks.
# Low Level Architecture
