# Alphabetic — AI-Powered Database Query Assistant

An intelligent, local-LLM-powered database agent designed to automate schema documentation and democratize data access. Alphabetic securely connects to any relational database, generates business-friendly data dictionaries, and provides a natural language chat interface to query your data in plain English—all without exposing sensitive schemas to external cloud APIs.

![Application Architecture](https://raw.githubusercontent.com/Awesome06/Alphabetic/main/docs/flowDiagram.png)
*(Alphabetic Architecture and Agent Flow)*

## Contents
- [Quick start](#quick-start)
- [Tech stack](#tech-stack)
- [System Architecture](#system-architecture)
- [Core Features & Agentic Flow](#core-features--agentic-flow)
- [Project layout](#project-layout)

## Quick start
Alphabetic runs locally to ensure absolute data privacy. It requires **Python 3.11+**, **Node 20+**, and **Ollama** for local model inference.

### 1. Initialize Local LLMs
Ensure [Ollama](https://ollama.com/) is installed and running, then pull the required models:
```bash
ollama pull llama3
ollama pull codellama
```

### 2. Launch the Backend (FastAPI)
```bash
git clone [https://github.com/Awesome06/Alphabetic.git](https://github.com/Awesome06/Alphabetic.git)
cd Alphabetic/backend

python -m venv .venv
source .venv/bin/activate  # On Windows: .\.venv\Scripts\activate
pip install -r requirements.txt

# Run the API server
python main.py
```

### 3. Launch the Frontend (React)
Open a new terminal window:
```bash
cd Alphabetic/frontend
npm install
npm run dev
```
Access the application at `http://localhost:5173`.

## Tech stack

| Layer | Technology | Purpose |
| :--- | :--- | :--- |
| **Frontend** | React, Vite, Axios | Responsive SPA for database introspection and chat. |
| **Backend API**| Python, FastAPI | High-performance async API for routing LLM prompts and DB queries. |
| **Database ORM**| SQLAlchemy, Pandas | Agnostic connection layer (PostgreSQL, MySQL, SQL Server, SQLite). |
| **AI / LLMs** | Ollama (Llama 3, Code Llama) | Secure, localized inference for natural language and SQL generation. |

## System Architecture

```text
┌──────────────────────────── Browser ────────────────────────────┐
│                        React SPA UI                             │
│                                                                 │
│   ├── Schema Explorer Dashboard                                 │
│   └── Dual-Mode Chat Interface                                  │
└───────────────┬───────────────────────────────▲─────────────────┘
                │ HTTP / REST                   │ JSON Responses
┌───────────────▼───────────────────────────────┴─────────────────┐
│                        FastAPI Backend                          │
│                                                                 │
│   ├── Schema Introspection Engine (SQLAlchemy)                  │
│   └── Prompt Router (Summary Mode vs. SQL Mode)                 │
└───────────────┬───────────────────────────────┬─────────────────┘
                │ Generates SQL / Text          │ Executes SQL
┌───────────────▼────────────────┐      ┌───────▼─────────────────┐
│    Local Ollama Instance       │      │   Target Database       │
│  (Llama 3 / Code Llama)        │      │ (PostgreSQL, MySQL...)  │
└────────────────────────────────┘      └─────────────────────────┘
```

## Core Features & Agentic Flow

Alphabetic goes beyond simple text generation by employing an **Agentic Workflow** with distinct operational modes and self-healing execution loops.

### 1. Automated Schema Introspection
Upon connection, Alphabetic bypasses manual configuration by scanning the target database via SQLAlchemy. It extracts structural metadata, calculates completeness metrics via Pandas, and utilizes **Llama 3** to generate business-friendly summaries for every table and view.

### 2. Dual-Mode Chat Interface
The chat agent routes user queries to the most efficient execution path:
*   **Summary Mode:** Designed for high-level metadata questions (e.g., *"Which tables contain user payment info?"*). It relies entirely on the AI's contextual awareness of the cached schema summaries, returning instant answers without touching the live database.
*   **SQL Mode:** Designed for precise, quantitative data retrieval (e.g., *"What was the total revenue in Q3?"*). It utilizes **Code Llama** to translate the natural language query into optimized SQL, executes it against the database, and translates the raw result set back into plain English.

### 3. Self-Correcting SQL Engine
To handle the inherent unpredictability of LLM-generated code, the SQL Mode features a self-healing execution loop. If Code Llama generates invalid SQL (e.g., hallucinated columns or syntax errors), the backend catches the database exception, feeds the exact error message back into the LLM context window, and commands it to fix the query. This drastically increases the success rate of complex analytical questions.

## Project layout

```text
.
├── backend/
│   ├── main.py                 # FastAPI application and route definitions
│   ├── database.py             # SQLAlchemy connection manager
│   ├── llm_agent.py            # Prompt engineering and Ollama integration
│   ├── sql_executor.py         # Query execution and self-correction loop
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   ├── components/         # Chat interface, Schema Explorer, Metrics Cards
│   │   ├── api/                # Axios configuration and backend communication
│   │   └── App.jsx
│   ├── package.json
│   └── vite.config.js
├── docs/                       # Architecture diagrams and screenshots
└── README.md
```

---

## Contact
**Mrigank Bhatnagar**  
* [LinkedIn](https://linkedin.com/in/mrigank-bhatnagar)
* [GitHub](https://github.com/Awesome06)
