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

## 💡 Key Lessons Learned

- Local models need explicit, concrete instructions — vague prompts fail
- Always use `.get()` instead of `[]` for dict access from LLM output
- Use `field_validator` to coerce types — local models return numbers as strings
- LLMs are stateless — memory only exists if you pass history every call
- Embeddings convert text to vectors so similar meaning = close distance

---

## 👤 Author

**Subrat Kumar**
GitHub: [@SubratKumar018](https://github.com/SubratKumar018)
