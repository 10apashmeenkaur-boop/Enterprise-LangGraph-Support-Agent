# ⚡ Multi-Agent RAG Customer Support Architecture (LangGraph)

An institutional-grade, stateful Multi-Agent Support System built to automate complex Tier-1 and Tier-2 enterprise customer inquiries using **LangGraph**, **LangChain**, and **Qdrant**.

## 🏗️ System Architecture 

Unlike standard linear RAG pipelines, this system utilizes a **Cyclic Graph State Machine** to route user intent dynamically across specialized sub-agents:

1. **Triage Agent:** Evaluates sentiment, extracts customer ID, and classifies domain intent.
2. **Technical Support Agent:** Has tool-access to a Qdrant Vector DB containing product documentation. 
3. **Billing & Operations Agent:** Interfaces with mocked SQL customer ledgers to resolve refund/invoice disputes.
4. **Human Escalation Node:** Deterministic guardrail that catches high-risk or abusive queries and freezes the graph state for human operator intervention.

## ⚙️ Tech Stack

* **Orchestration:** Python 3.11, LangGraph, LangChain
* **Vector Store:** Qdrant (In-Memory / Docker)
* **Embeddings:** `text-embedding-3-small`
* **Serving:** FastAPI, Uvicorn

## 🚀 Key Engineering Considerations

* **State Persistence:** Implemented `MemorySaver()` checkpointer to allow multi-turn conversation memory across different session threads.
* **Hallucination Mitigation:** The Technical Agent is strictly bound to a *Context-Precision* system prompt; if the vector retrieval yields `<0.70` cosine similarity, the agent is forced to trigger the `EscalateToHuman` tool rather than guessing.

## 📋 Local Setup

```bash
git clone [https://github.com/yourusername/enterprise-langgraph-support-agent.git](https://github.com/yourusername/enterprise-langgraph-support-agent.git)
cd enterprise-langgraph-support-agent
pip install -r requirements.txt
