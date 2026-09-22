# OVA - Next-Gen AI Studio

OVA (Omni Virtual Assistant) is a Flask-based personal AI assistant and web workspace powered by Groq's high-speed inference engine. OVA features real-time token streaming, prompt-guided deep reasoning mode, live web search via Tavily, persistent memory via SQLite, file upload ingestion, and a subprocess-based Python code runner with an 8-second execution timeout.

---

## Features

* **⚡ High-Speed Token Streaming**: Real-time token-by-token response streaming using Server-Sent Events (SSE) via `/chat/stream`.
* **🧠 Deep Reasoning Mode**: Optional prompt-guided step-by-step thinking block (`<think>...</think>`) parsed and rendered in the UI before presenting the solution.
* **🌐 Live Web & News Search**: Real-time web and news retrieval powered by the Tavily API with automatic keyword trigger detection and source citations.
* **💾 Persistent Memory Vault**: SQLite-backed memory store that detects remembering triggers in chat messages and injects saved facts into subsequent prompt contexts.
* **📁 Document & Code Ingestion**: Upload documents and code files (PDF, TXT, CSV, JSON, Python, JS, Markdown, etc.) with PDF text extraction via `pypdf` (max 10 MB per file).
* **💻 Canvas Studio & Python Code Runner**: Interactive code view with execution via a local Python subprocess (`subprocess.run`) protected by an 8-second timeout.
* **💬 Chat Management**: Manage multiple chat sessions, auto-generate conversation titles, rename, delete, and export transcripts to Markdown or JSON.
* **🎨 Modern UI**: Dark-mode interface with KaTeX LaTeX math rendering, Highlight.js syntax highlighting, and Marked.js markdown parsing.

---

## Tech Stack

* **Backend**: Python 3.13+, Flask
* **AI & LLM Inference**: [Groq Cloud Python SDK](https://groq.com/)
* **Web Search**: [Tavily Python SDK](https://tavily.com/) (with `requests` fallback)
* **Database**: SQLite3 (Python standard library)
* **Frontend**: HTML5, Vanilla CSS, Vanilla JavaScript
* **Third-Party Python Libraries**:
  * `Flask`: Web framework and streaming SSE responses
  * `groq`: Official client library for Groq API
  * `python-dotenv`: Environment variable management
  * `pypdf`: Text extraction from uploaded PDF documents
  * `requests`: HTTP requests fallback for Tavily search
  * `tavily-python`: Official Tavily search API client
* **Frontend CDN Assets**:
  * `KaTeX`: Mathematical equation rendering
  * `Highlight.js`: Code syntax highlighting
  * `Marked.js`: Markdown parsing

---

## Project Structure

```
├── app.py                 # Flask server, routes, streaming, and database helpers
├── services/
│   ├── __init__.py        # Package initializer
│   ├── ai.py              # AI service placeholder
│   ├── files.py           # File upload saving, validation, and PDF extraction
│   ├── memory.py          # SQLite memory extraction and storage
│   ├── search.py          # Tavily search integration and query classification
│   └── tools.py           # Tool integration placeholder
├── static/
│   ├── app.js             # Client-side UI logic, SSE stream handling, and syntax rendering
│   └── style.css          # Styling, dark-mode theme variables, and layout
├── templates/
│   └── index.html         # Single-page application interface
├── .env.example           # Template for required environment variables
├── .gitignore             # Git ignore configuration
├── README.md              # Project documentation
└── requirements.txt       # Third-party Python dependencies
```

---

## Getting Started

### Prerequisites

* **Python**: Version 3.10 or higher (Python 3.13 recommended)
* **Git**: Installed and configured
* **API Keys**:
  * [Groq Cloud API Key](https://console.groq.com/) (Required for chat completions)
  * [Tavily API Key](https://tavily.com/) (Optional, required for live news & web search)

---

### Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/MeghrajKundu/OVA-AI-.git
   cd OVA-AI-
   ```

2. **Create and activate a virtual environment**:

   * On Windows (PowerShell):
     ```powershell
     python -m venv .venv
     .venv\Scripts\Activate.ps1
     ```

   * On macOS / Linux:
     ```bash
     python3 -m venv .venv
     source .venv/bin/activate
     ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

---

### Configuration

1. Copy `.env.example` to create your local `.env` file:
   * Windows (PowerShell):
     ```powershell
     Copy-Item .env.example .env
     ```
   * Linux / macOS:
     ```bash
     cp .env.example .env
     ```

2. Open `.env` and add your API credentials:
   ```env
   GROQ_API_KEY=your_actual_groq_api_key_here
   TAVILY_API_KEY=your_actual_tavily_api_key_here
   ```

> [!CAUTION]
> **Security Warning**: Never commit your `.env` file or publish your API keys. The repository `.gitignore` is preconfigured to keep `.env`, `.env.*`, database files (`*.db`), and cache files strictly local.

---

### Running the Application

Start the Flask server:

```bash
python app.py
```

Open your browser and navigate to:
```text
http://127.0.0.1:5000
```

---

## Supported Models

The application configures the following models directly in `app.py`:

| Model Name | Model ID | Tag | Description |
| :--- | :--- | :--- | :--- |
| **GPT-OSS 120B** | `openai/gpt-oss-120b` | Flagship | Deep multi-step reasoning, high intelligence & coding *(Default)* |
| **Qwen 3.8 27B** | `qwen/qwen3.8-27b` | Ultra-Fast | Lightning fast coding, math & reasoning |
| **Compound Web** | `groq/compound` | Composite | Automated web browsing and multi-agent synthesis |
| **GPT-OSS 20B** | `openai/gpt-oss-20b` | Lightweight | Ultra-efficient for rapid chat & summaries |

---

## Security & Execution Notes

* **Code Runner**: The Python code runner executes user code via a local `subprocess.run` command with a hard 8-second timeout. It is not an isolated or sandboxed container environment; execute only trusted code.
* **File Ingestion**: Uploads are restricted to approved file extensions and capped at 10 MB.
* **Local Storage**: Conversation databases (`chat_history.db`), uploads (`uploads/`), and workspaces (`workspace/`, `workspaces/`) remain local and are excluded from version control.

---

## License

A license has not yet been selected for this project. All rights are reserved by the author until an open-source license is officially added.
