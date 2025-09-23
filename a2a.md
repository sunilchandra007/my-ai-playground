# 🤖 Agent2Agent (A2A) Protocol

- An **open standard** to enable seamless **communication** and **collaboration** between AI agent
- Provides a **common language** for agents built using diverse framework(Goodle ADK) and different vendors, equipped with any tool/MCP 

---

## 📘 Agent Card
- Similar to business card
- describes what can it do, lists the skills
- hosted at /.well-known/agent.json
---

## 📘 Overview
The A2A Protocol enables:
- **Inter-agent messaging** across heterogeneous systems.
- **Secure and verifiable identity exchange**.
- **Task delegation and coordination**.
- **Stateful and stateless interactions**.

---

## 🧩 Core Components

### 1. **Agent Identity**
Each agent must have a unique identity, typically defined by:
- `agent_id`: UUID or DID
- `agent_profile`: Metadata including capabilities, trust level, and endpoint

### 2. **Message Envelope**
All messages follow a structured envelope format:
```json
{
  "sender": "agent_id",
  "recipient": "agent_id",
  "timestamp": "ISO8601",
  "type": "message_type",
  "payload": { ... }
}

---

## Reference
- [Agent2Agent (A2A) Crash Course](https://www.youtube.com/watch?v=mFkw3p5qSuA)
- [A2A Protocol Introduction](https://a2a-protocol.org/latest/) 

--
