# <p align="center"><font color="#2E86C1">🚀 Advanced MCP: Remote HTTP & Secure Roots</font></p>

<p align="center">
  <img src="https://img.shields.io/badge/Architecture-Base--Derived-blueviolet?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Security-Filesystem_Roots-red?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Transport-Streamable_HTTP-success?style=for-the-badge" />
  <img src="https://img.shields.io/badge/AI-GPT--4o--mini-orange?style=for-the-badge" />
</p>

---

## <font color="#D35400">📖 Project Overview</font>
This project demonstrates an advanced implementation of the **Model Context Protocol (MCP)**. It focuses on production-ready patterns for remote connectivity, enterprise-grade security, and server-initiated AI logic.



### <font color="#2980B9">🏗️ System Architecture</font>
The application is built using a **Base/Derived class architecture** to ensure maximum code reusability:
* **MCP HTTP Server**: A `FastMCP` server providing tools, resources, and prompts while enforcing security boundaries.
* **Base HTTP Client**: A reusable logic layer handling bidirectional communication via **Streamable HTTP**.
* **GUI Client App**: A Gradio interface for manual discovery and testing of MCP primitives.
* **AI Host App**: An agentic host that integrates **OpenAI GPT-4o-mini** to interact with the server via natural language.

---

## <font color="#1E8449">🛡️ Core Features</font>

| Feature | Description | Benefit |
| :--- | :--- | :--- |
| **<font color="#1E8449">HTTP Transport</font>** | Server runs independently on port 8000. | Supports multiple concurrent clients and remote cloud deployment. |
| **<font color="#D35400">Roots Security</font>** | Strict security boundaries for file access. | Prevents path traversal and unauthorized access to system files. |
| **<font color="#2E86C1">Sampling</font>** | Server-initiated LLM requests with user approval. | Enables agentic behavior without exposing LLM API keys to the server. |
| **<font color="#8E44AD">Synthetic Tools</font>** | Wraps MCP resources and prompts as OpenAI tools. | Bridges the gap between MCP primitives and LLM function calling. |

---

## <font color="#1F618D">🚀 Getting Started</font>

### 1️⃣ Installation
Ensure you have Python 3.11+ installed, then set up your environment:
```bash
pip install mcp==1.16.0 fastmcp==2.12.5 httpx==0.28.1 gradio==5.49.1 openai==1.26.1
