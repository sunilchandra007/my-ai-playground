# 🧠 Model **Context** Protocol (MCP)

The **Model Context Protocol (MCP)** is an open standard that defines how AI models—especially LLMs—connect to external data sources and APIs. It enables the creation of intelligent agents and complex workflows by providing structured **context** to AI applications.

### 🔗 Key Highlights
- A **standard protocol** for connecting AI models to **external data sources** and APIs.
- Enables AI applications to provide **context** to LLMs in a consistent, reusable way.
- Facilitates building **agents and workflows** on top of LLMs.
- Allows models to **read data** and **execute actions** via a universal connector.

📖 **Reference**: [modelcontextprotocol.io/introduction](https://modelcontextprotocol.io/introduction)

---

## 🧩 MCP Primitives

MCP defines a set of **primitives**—building blocks that clients and servers can expose to each other.

### 🔧 Server-Side Primitives
Servers can expose the following core primitives:

- **🛠 Tools**  
  Executable functions that AI applications can invoke to perform actions  
  _Examples: file operations, API calls, database queries_

- **📚 Resources**  
  Data sources that provide contextual information to AI applications  
  _Examples: file contents, database records, API responses_

- **📝 Prompts**  
  Reusable templates that structure interactions with language models  
  _Examples: system prompts, few-shot examples_

Each primitive supports:
- `*/list` – Discover available primitives
- `*/get` – Retrieve specific primitive data
- `tools/call` – Execute a tool (where applicable)

---

### 🤝 Client-Side Primitives
Clients can also expose primitives to enable richer interactions initiated by servers:

- **🎯 Sampling**  
  Servers can request completions from the client’s AI model using `sampling/complete`.

- **💬 Elicitation**  
  Servers can request additional information or confirmation from users via `elicitation/request`.

- **📋 Logging**  
  Servers can send log messages to clients for debugging and monitoring.

---
Reference - MCP Explorer

- https://mcp.azure.com/ - helps in testing MCP Server
- https://code.visualstudio.com/mcp
- [Azure MCP Server](https://learn.microsoft.com/en-us/azure/developer/azure-mcp-server/tools/)
- [GitHub Remote MCP Server](https://github.com/github/github-mcp-server)
