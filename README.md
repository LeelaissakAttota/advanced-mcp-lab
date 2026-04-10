# 🚀 Advanced MCP Lab: Remote HTTP & Secure Roots

![Language](https://img.shields.io/badge/Language-Python%203.11-3776AB?style=flat-square&logo=python&logoColor=white)
![Framework](https://img.shields.io/badge/Framework-FastMCP%202.12.5-0052CC?style=flat-square)
![Protocol](https://img.shields.io/badge/Protocol-Model%20Context%20Protocol-6A0DAD?style=flat-square)
![Transport](https://img.shields.io/badge/Transport-Streamable%20HTTP-00897B?style=flat-square)
![UI](https://img.shields.io/badge/UI-Gradio-FF7C00?style=flat-square)
![LLM](https://img.shields.io/badge/LLM-OpenAI%20GPT--4o--mini-412991?style=flat-square&logo=openai&logoColor=white)
![Focus](https://img.shields.io/badge/Focus-Agentic%20AI%20Architecture-2E7D32?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=flat-square)

---

## 📌 Project Overview

This project demonstrates an **advanced, production-ready implementation 
of the Model Context Protocol (MCP)** — upgrading from basic STDIO 
to fully remote **Streamable HTTP transport**.

It implements **filesystem Roots security boundaries**, 
**server-initiated LLM sampling**, and **OpenAI GPT-4o-mini integration** 
— real-world patterns used in enterprise agentic AI deployments.

**Domain:** Agentic AI — Advanced MCP Architecture  
**Language:** Python 3.11  
**Server:** HTTP on `http://127.0.0.1:8000/mcp`  
**Workspace:** Strictly sandboxed `workspace/` directory  

---

## 🏗️ System Architecture

```
┌──────────────────────────────────────────────────┐
│            AI Host App                            │
│   OpenAI GPT-4o-mini                             │
│   Natural Language → OpenAI Function Calling     │
│   → MCP Tool Calls → Results → User              │
└─────────────────────┬────────────────────────────┘
                      │
┌─────────────────────▼────────────────────────────┐
│         Base HTTP Client (MCPHTTPClient)          │
│   streamablehttp_client → /mcp endpoint          │
│   list_tools / call_tool / list_resources        │
│   list_prompts / get_prompt / read_resource      │
└─────────────────────┬────────────────────────────┘
                      │  Streamable HTTP Port 8000
┌─────────────────────▼────────────────────────────┐
│      FastMCP HTTP Server (127.0.0.1:8000)        │
│                                                  │
│  🔧 Tools              📦 Resources              │
│  ├── read_file         file://workspace/{file}   │
│  ├── write_file                                  │
│  ├── list_files        💬 Prompts                │
│  └── analyze_code      ├── review_code           │
│      (Sampling)        └── analyze_security      │
│                                                  │
│  🛡️ Roots: workspace/ only — is_within_roots()  │
└──────────────────────────────────────────────────┘
```

---

## 📂 Project Structure

```
advanced-mcp-lab/
│
├── mcp_http_server.py        # FastMCP HTTP server — tools, resources, prompts
├── mcp_http_client_base.py   # Base client — Streamable HTTP transport layer
├── mcp_http_client_app.py    # Gradio GUI client — manual tool/resource testing
├── mcp_http_host_app.py      # OpenAI GPT-4o-mini AI host application
│
└── workspace/                # Secure sandbox — only accessible directory
    ├── test.txt              # Sample test file
    └── README.md             # Workspace description
```

---

## 🛠️ Tech Stack

| Component | Technology | Version |
|---|---|---|
| MCP Framework | FastMCP | 2.12.5 |
| MCP Protocol | Model Context Protocol | 1.16.0 |
| Language | Python | 3.11+ |
| HTTP Transport | streamablehttp_client | httpx 0.28.1 |
| UI Framework | Gradio | 5.49.1 |
| LLM Integration | OpenAI GPT-4o-mini | openai 1.26.1 |
| Server Endpoint | http://127.0.0.1:8000/mcp | Streamable HTTP |

---

## 🔧 MCP Server — Tools, Resources & Prompts

### Tools (4 total)

| Tool | Description | Roots Check |
|---|---|---|
| `read_file` | Read file from workspace | ✅ Yes |
| `write_file` | Write content to workspace file | ✅ Yes |
| `list_files` | List files/dirs in workspace | ✅ Yes |
| `analyze_code` | Triggers server-initiated sampling | ✅ Yes |

### Resources
| URI Pattern | Description |
|---|---|
| `file://workspace/{filename}` | Read any workspace file as resource |

### Prompts
| Prompt | Purpose |
|---|---|
| `review_code` | Generate code review prompt for a file |
| `analyze_security` | Generate security analysis prompt for a file |

---

## 🛡️ Roots Security Implementation

All tool operations are validated against the `workspace/` directory:

```python
def is_within_roots(path: Path) -> bool:
    try:
        path.resolve().relative_to(BASE_DIR.resolve())
        return True
    except ValueError:
        return False
```

**What this prevents:**
- Path traversal attacks (`../../etc/passwd`)
- Access to system files outside workspace
- Unauthorized reads/writes to host filesystem

---

## 🤖 Server-Initiated Sampling

The `analyze_code` tool demonstrates the **sampling pattern**:

```
Tool Call: analyze_code(code, focus)
         │
         ▼
Server builds sampling/createMessage request
         │
         ▼
Request sent back to client:
{
  "method": "sampling/createMessage",
  "params": {
    "messages": [{"role": "user", "content": "Analyze this code..."}],
    "maxTokens": 500
  }
}
         │
         ▼
Client shows approval → User approves
         │
         ▼
Client calls LLM → Returns analysis to server
```

**Key benefit:** LLM API keys never leave the client side! ✅

---

## 🚀 How to Run

**Step 1 — Install dependencies:**
```bash
pip install mcp==1.16.0 fastmcp==2.12.5 httpx==0.28.1 gradio==5.49.1 openai==1.26.1
```

**Step 2 — Set OpenAI API key:**
```bash
export OPENAI_API_KEY="your-api-key-here"
```

**Step 3 — Start MCP HTTP Server (Terminal 1):**
```bash
python mcp_http_server.py
# Output: Starting HTTP MCP Server on http://127.0.0.1:8000
# Output: Workspace roots: ./workspace
```

**Step 4 — Launch GUI Client (Terminal 2):**
```bash
python mcp_http_client_app.py
```

**Step 5 — Launch AI Host App (Terminal 3):**
```bash
python mcp_http_host_app.py
```

---

## 🌐 HTTP vs STDIO Transport

| Feature | STDIO (Basic MCP) | HTTP (This Project) |
|---|---|---|
| Connectivity | Local process only | Remote / Cloud ready |
| Clients | Single client | Multiple concurrent |
| Server lifecycle | Tied to client | Independent process |
| Deployment | Same machine | Any server / cloud |
| Endpoint | stdin/stdout | `http://host:port/mcp` |

---

## 🎓 Skills Demonstrated

- Advanced Model Context Protocol (MCP) with HTTP transport
- Streamable HTTP client-server communication
- Filesystem Roots security boundary implementation
- Server-initiated LLM sampling pattern
- OpenAI GPT-4o-mini function calling integration
- MCP tools, resources & prompts implementation
- Synthetic tools — wrapping MCP resources as OpenAI functions
- Gradio UI for AI application development
- Base/Derived class architecture (MCPHTTPClient → Host/GUI App)
- Async Python — asyncio, AsyncExitStack, httpx
- Production-ready agentic AI system design

---

## 🔗 Related Project

| Project | Transport | Focus |
|---|---|---|
| [MCP Security with Permissions](https://github.com/Leelaissakattaota/MCP-Security-with-Permissions-and-Elicitation) | STDIO | Permission Management & Elicitation |
| **Advanced MCP Lab** ← You are here | HTTP | Roots Security & Sampling |

---

## 📜 Certifications

| Certification | Issuer | Platform |
|---|---|---|
| IBM Data Science Professional Certificate | IBM | Coursera |
| IBM Generative AI Professional Certificate | IBM | Coursera |
| IBM Agentic AI with RAG Certificate | IBM | Coursera |
| IBM RAG and Agentic AI Professional Certificate | IBM | Coursera |

---

## 🤝 Connect with Me

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Leela%20A-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/leela-a)
[![Gmail](https://img.shields.io/badge/Gmail-attotaleelaissak@gmail.com-D14836?style=flat-square&logo=gmail&logoColor=white)](mailto:attotaleelaissak@gmail.com)
[![GitHub](https://img.shields.io/badge/GitHub-Leelaissakattaota-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/Leelaissakattaota)
