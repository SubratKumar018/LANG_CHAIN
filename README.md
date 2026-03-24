# 🤖 LangChain + Ollama Course

A complete 9-session hands-on course building local AI pipelines with LangChain and Ollama.
No internet required. No API keys. Everything runs locally on your machine.

---

## 📚 Sessions Covered

| Session | Topic | Key Concepts |
|---|---|---|
| Session 1 | LLM Basics | OllamaLLM, ChatOllama, invoke, stream, batch |
| Session 2 | Prompt Templates | ChatPromptTemplate, variables, format_messages |
| Session 3 | Chains + LCEL | Pipe operator, StrOutputParser, RunnableParallel |
| Session 4 | Output Parsers | JsonOutputParser, PydanticOutputParser, field_validator |
| Session 5 | Memory + Chat History | ChatMessageHistory, MessagesPlaceholder, RunnableWithMessageHistory |
| Session 6 | RAG Pipeline | FAISS, OllamaEmbeddings, retriever, document loaders |
| Session 7 | Agents + Tools | LangGraph agents, @tool decorator, tool calling |
| Session 8 | LangGraph | StateGraph, nodes, edges, conditional routing |
| Session 9 | Full Project | Local AI assistant combining all sessions |

---

## 🛠️ Tech Stack

- **Python** 3.12
- **LangChain** — chains, prompts, output parsers, memory
- **LangGraph** — stateful agent graphs
- **Ollama** — local LLM runner
  - `llama3.1:8b` — main chat model
  - `nomic-embed-text` — embeddings model
- **FAISS** — local vector database
- **Pydantic** — data validation

---

## 🚀 Setup

### 1. Install dependencies
```bash
pip install langchain langchain-ollama langchain-core langchain-community
pip install langchain-text-splitters langgraph faiss-cpu
pip install langchain-experimental
```

### 2. Pull Ollama models
```bash
ollama pull llama3.1:8b
ollama pull nomic-embed-text
```

### 3. Start Ollama
```bash
ollama serve
```

### 4. Run the notebook
Open `LANG_CHAIN.ipynb` in Jupyter and run cell by cell.

---

## 🏗️ Full Project Architecture (Session 9)
```
User Input
    ↓
classify_node  ← LLM router (MATH / DOC / CHAT)
    ↓
    ├→ math_node      → extracts expression → calculator tool
    ├→ doc_node       → FAISS retrieval → answers from documents
    └→ chat_node      → conversation with memory
    ↓
Response
```

---

## 📁 Project Structure
```
langchain_project/
│
├── LANG_CHAIN.ipynb     # Main notebook — all 9 sessions
├── space_facts.txt      # Sample document for RAG pipeline
├── faiss_db/            # Local vector database (auto-generated)
└── README.md
```

---

## 🧠 Core Concepts Explained

### What problem does RAG solve?
LLMs only know what they were trained on. Ask llama3 about your company's internal docs, a PDF you wrote last week, or anything after its training cutoff — it has no idea. RAG fixes this by giving the model access to your own documents at query time. It retrieves the relevant pieces, then answers based on them.

The analogy: instead of asking someone to memorize a textbook, you let them look up the relevant page before answering.

### The RAG Pipeline
```
Your documents
      ↓
1. LOAD        — read the raw text
      ↓
2. SPLIT       — chop into small chunks
      ↓
3. EMBED       — convert chunks to vectors (numbers)
      ↓
4. STORE       — save vectors in a vector database
      ↓
5. RETRIEVE    — find chunks relevant to the user's question
      ↓
6. ANSWER      — send retrieved chunks + question to LLM
```

### Why embeddings enable similarity search
nomic-embed-text converts text into a list of ~768 numbers. That list is a coordinate — a location in high-dimensional space. Text with similar meaning lands close together, text with different meaning lands far apart. Similarity search is literally just measuring distance between points in space.

### Why LLMs are stateless
Every time you call an LLM it boots up fresh with zero memory. It has no database, no storage, no record of previous calls. Every call is like talking to someone who just woke up with amnesia — incredibly smart, but remembers nothing from before this exact moment. Memory only exists if you pass the full history in every call.

### What RunnablePassthrough does
In the RAG chain, two things need to reach the prompt — the retrieved context and the original question. The retriever handles context. RunnablePassthrough forwards the original question unchanged so it doesn't get lost when the retriever runs.
```
Without it:  user question → retriever → chunks   ← question is gone
With it:     user question → retriever → chunks   ← context
             user question → passthrough           ← question preserved
```

### Chain vs Agent

| | Chain | Agent |
|---|---|---|
| Path | Fixed — always same steps | Dynamic — model decides |
| Tools | No tools | Picks tools as needed |
| Loops | Runs once | Can loop multiple times |
| Best for | Predictable tasks | Open-ended tasks |

### What is LangGraph?
LangGraph lets you build AI workflows as a graph — nodes connected by edges, with conditional routing.
```
NODES  — individual steps (a function, an LLM call, a tool)
EDGES  — connections between nodes (what runs next)
STATE  — a shared dict that flows through the whole graph
```

### ReAct Pattern
```
Question → THOUGHT → ACTION → OBSERVATION → THOUGHT → ... → FINAL ANSWER
```
The model keeps looping until it decides it has enough information to answer.

---

## 💡 Key Lessons Learned

- Local models need explicit, concrete instructions — vague prompts fail
- Always use `.get()` instead of `[]` for dict access from LLM output
- Use `field_validator` to coerce types — local models return numbers as strings
- LLMs are stateless — memory only exists if you pass history every call
- Embeddings convert text to vectors so similar meaning = close distance
- `result.strip().split()[0]` — always take first word from LLM classifier output
- `RunnablePassthrough` forwards original input unchanged through a chain

---

## 👤 Author

**Subrat Kumar**
GitHub: [@SubratKumar018](https://github.com/SubratKumar018)
