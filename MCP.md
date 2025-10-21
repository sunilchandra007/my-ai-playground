# 🧠 Model **Context** Protocol (MCP)

The **Model Context Protocol (MCP)** is an open standard that defines how AI models—especially LLMs—connect to external data sources and APIs. It enables the creation of intelligent agents and complex workflows by providing structured **context** to AI applications.

### 🔗 Key Highlights
- A **standard protocol** for connecting AI models to **external data sources** and APIs.
- Enables AI applications to provide **context** to LLMs in a consistent, reusable way.
- Facilitates building **agents and workflows** on top of LLMs.
- Allows models to **read data** and **execute actions** via a universal connector.

### 🔗 Transport Mode
- STDIO (for local communication)
- HTTP (for remote network communication)
```python
mcp.run(transport="stdio")
mcp.run(transport="streamable-http")
mcp.run(transport="http", host="localhost", port=8000)
```

### 🔗 Session State
- MCP is stateful by default - MCP server maintains session state
```python
mcp = FastMCP("StatefulServer")
mcp = FastMCP("StatelessServer", stateless_http=True)
```
  
📖 **Reference**: [modelcontextprotocol.io/introduction](https://modelcontextprotocol.io/introduction)

---

## 🧩 MCP Primitives

MCP defines a set of **primitives**—building blocks that clients and servers can expose to each other.

### 🔧 Server-Side Primitives
Servers can expose the following core primitives:

- **🛠 Tools**  
  Executable functions that AI applications can invoke to perform actions  
  _Examples: file operations, API calls, database queries_

  ```python
  @mcp.tool()
  def search_flights(origin: str, destination: str) -> dict:
      """Search for flights between two airports"""
      return {
          "flights": [
              {"id": "FL123", "origin": origin, "destination": destination, "price": 299},
              {"id": "FL456", "origin": origin, "destination": destination, "price": 399}
          ]
      }
  ```

- **📚 Resources**  
  Data sources that provide contextual information to AI applications  
  _Examples: file contents, database records, API responses_
  ```python
  @mcp.resource("file://airports")
  def get_airports():
      """Get list of available airports"""
      return {
          "LAX": {"name": "Los Angeles International", "city": "Los Angeles"},
          "JFK": {"name": "John F. Kennedy International", "city": "New York"},
          "LHR": {"name": "London Heathrow", "city": "London"}
      }
  ```

- **📝 Prompts**  
  - Reusable templates that structure interactions with language models
  - AI guidance templates for interactions
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

## 🧩 MCP Inspector

```bash
npx @modelcontextprotocol/inspector
```

---
Reference - MCP Explorer

- https://mcp.azure.com/ - helps in testing MCP Server
- https://code.visualstudio.com/mcp
- [Azure MCP Server](https://learn.microsoft.com/en-us/azure/developer/azure-mcp-server/tools/)
- [GitHub Remote MCP Server](https://github.com/github/github-mcp-server)
- [mcp-for-beginners](https://github.com/microsoft/mcp-for-beginners)
- [modelcontextprotocol/python-sdk](https://github.com/modelcontextprotocol/python-sdk)
- [Creating Your First MCP Server](https://www.youtube.com/watch?v=44SUYJ9fqxs)

## Explored MCP Servers
- [chrome devtools](https://developer.chrome.com/blog/chrome-devtools-mcp)
- https://fastmcp.me/MCP/Explore
- 
