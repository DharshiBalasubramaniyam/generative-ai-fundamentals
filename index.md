# Generative AI Fundamentals

## LLM
- An LLM is an AI model trained on massive amounts of data, specialized in text understanding and generation. 
- E.g., OpenAI GPT, Anthropic Claude, Google Gemini 
- They understand queries and answer based on their training data. 

### Context window
- The context window of an LLM is the amount of text, in tokens, that the model can remember at any one time. This includes prompt, conversation history, and generated output. 
- When prompt + conversation exceeds an LLM’s context window, it must be truncated or summarized for the model to proceed.

### Temperature
- In a large language model (LLM), temperature is a parameter that controls how random or creative the model's output is,
- When an LLM predicts the next word, it assigns probabilities to possible choices.
- Low temperature (<1) makes the LLM select high-probability words; High temperature (>1) flattens the distribution, giving lower-probability words a better chance.
- Temperature = 0 → the model will always pick the most mathematically probable next word. Almost every time, you'll get the same answer. E.g. coding. maths
- Pushing the temperature too far beyond 1 or 1.5 often causes the model to generate factually incorrect text. E.g., fiction, poetry 
- Medium Temperature (0.3 to 0.7) provides a balanced mix of coherence and variety. This is typically the default setting for general conversational AI. E.g., emails, explanations.

### Top-k Sampling
- Only consider the top K most likely tokens after assigning probabilities. Ignore the rest.

### Top-p Sampling
- Instead of fixing the number of tokens as tok-k, it chooses the smallest set of tokens whose cumulative probability reaches p.

### Structured output
- Structured output in AI is a feature that forces models to return data in a strict, predictable format (like JSON or XML) instead of free-form text.

### Hallucination
- When unsure, LLMs often make up answers that sound right but aren’t factually correct. This is known as a hallucination. To overcome this:
     - Ground the Model with RAG
     - Adjust System Settings: Temperature, System instructions
     - Use prompt engineering
     - Use structured output and Implement Validation Checks

## Prompt engineering 
- Prompt engineering is the process of writing effective instructions for a model, such that it consistently generates content that meets your requirements.
     - Zero-shot prompting: You give the task directly, no examples. Example: Classify this review as positive or negative: "The product is amazing."
     - Few-shot prompting: You provide examples so the model learns the pattern. Example: Review: "Great phone" → Positive, Review: "Very slow device" → Negative, Review: "Battery lasts long" → ?
     - Role prompting (System-style framing): You assign a role to guide behavior. Example: You are a senior Java backend engineer. Explain microservices clearly.
     - Instruction clarity (Explicit prompting): Make tasks unambiguous. Bad: Explain APIs; Better: Explain REST APIs in 5 bullet points with a real-world example.
     - Constraint prompting: You restrict behavior. Example: Answer in under 50 words. Do not use technical jargon. Use only bullet points.
     - Context stuffing (RAG-style prompting): You provide relevant external knowledge inside the prompt.
     - ReAct prompting (Reason + Act): Model alternates between reasoning and tool use.

## Embeddings
- Computers can’t directly understand words, but they can work with numbers. A vector/embedding is an array of numbers. Each number in a vector captures some aspect of the meaning of the word or text.

### Vector store
- A vector database stores vectors (or embeddings) of texts alongside other metadata that can help identify, organize, or retrieve the relevant vector when performing searches.
- Each embedded text takes a place in vector space. When a query comes in, it is converted into vectors and placed in the same vector space. If the query vector is close to a document vector, it means the document is relevant to the question.
- E.g., Pinecone, Chroma, Milvus, FAISS

### Similarity Search
- Similarity search is the technique used to compute the distance between the vectors in vector space. Techniques: Cosine similarity, Dot product, Euclidean distance
- Semantic search uses vector embeddings + similarity search to find results based on meaning, not keywords.

## RAG
- Retrieval-Augmented Generation (RAG) is a technique that combines an LLM with an external knowledge base, instead of relying solely on the training data.
- They understand queries and answers based on an external knowledge base. 
- It pulls only the most relevant information from the knowledge base using vector stores and semantic search and inserts it into the context window of the LLM.

## Agent
- An AI agent is a technique that combines an LLM with tools that can think, plan, decide, and perform actions to achieve a goal. 

### Function Calling (Tool calling)
- Function calling is a mechanism that allows Large Language Models (LLMs) to connect to external systems and access data outside their training data.
     - The developer provides the LLM with a list of available functions, including their names, detailed descriptions, and input parameters formatted via JSON schema.
     - The user sends a prompt. The model detects that it cannot answer using its static training data alone
     - The model outputs a JSON payload matching the registered function schema instead of writing a conversational reply.
     - Your backend application parses this JSON response, executes the actual database query or API call, and retrieves the real-time data.
     - Your application sends the tool's raw result back to the model. The LLM reads this fresh context and outputs a natural, user-friendly response.

### Single-agent system
- A single-agent system means one AI agent is responsible for planning, reasoning, using tools, and completing a task through multiple steps.
- This can plan the sequence of actions and invoke one or more tools as needed.

### Multi-Agent System
- A multi-agent system combines multiple AI agents, each with a specialized role.
- Even though a single-agent system can plan and invoke multiple tools, that doesn’t mean it’s always the best architecture. Architecture can become complex when too many responsibilities are mixed or the context window is exceeded. So, a multi-agent system solves this by delegating roles into specialized agents.

#### Sequential Pipeline Architecture
- Agents are arranged sequentially.
```
Agent A → Agent B → Agent C → Agent D
```

#### Router-Based Architecture
- A router agent decides which worker agent should handle the task, collects the results, and returns it.
- The router agent calls only one agent to complete a task.
```
                     Router Agent
                          │
     ┌────────────────────┼──────────────────┐
     ▼                    ▼                  ▼
 Worker Agent 1     Worker Agent 2    Worker Agent 3
```

#### Hierarchical Architecture
- Unlike the router agent, the manager agent breaks down the task, assigns subtasks across multiple worker agents, collects results, and produces final output.
```
                     Manager Agent
                          │
     ┌────────────────────┼──────────────────┐
     ▼                    ▼                  ▼
 Worker Agent 1     Worker Agent 2    Worker Agent 3
```

#### Graph Architecture
- Agents are connected by predefined edges, and execution moves between agents according to a predefined workflow and conditional routing rules.
```
A → Yes → B → No → C
│         │
▼         ▼
No        Yes
│         │
▼         ▼
D → → E → F
```

#### Network Architecture
- Each agent can talk to any other agent in the whole collection. No predefined workflow
```
A ↔ B ↔ C
↕   ↕   ↕
D ↔ E ↔ F
```
  
### LangChain
- LangChain is an abstraction layer that simplifies AI development by providing prebuilt components. 

