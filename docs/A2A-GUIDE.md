# Agent-to-Agent (A2A) Protocol Guide

ConferenceHaven implements the **A2A (Agent-to-Agent) Protocol** - enabling true multi-agent collaboration where AI agents communicate using natural language.

---

## What is A2A?

**A2A (Agent-to-Agent Protocol)** is an open standard developed by Google that enables AI agents to communicate and collaborate. Unlike tool protocols (MCP), A2A enables **agent-level** communication where one AI agent delegates tasks to another AI agent.

> "A2A focuses on enabling opaque, autonomous agents—powered by various LLM frameworks—to communicate naturally without exposing their internal architectures."
> — [A2A Protocol Specification](https://google.github.io/A2A/specification/)

### The Key Insight: Two LLMs Collaborating

The defining characteristic of A2A is that **both sides have their own LLM**:

```
┌─────────────────────┐         ┌─────────────────────┐
│    Host Agent       │         │   Remote Agent      │
│    ┌─────────┐      │  A2A    │      ┌─────────┐    │
│    │  LLM    │      │ ─────── │      │  LLM    │    │
│    └─────────┘      │         │      └─────────┘    │
│    Decides WHEN     │         │    Decides HOW      │
│    to delegate      │         │    to respond       │
└─────────────────────┘         └─────────────────────┘
```

This is fundamentally different from MCP (Model Context Protocol), where one LLM calls tools directly.

---

## A2A vs MCP: Understanding the Difference

| Aspect | A2A Protocol | MCP Protocol |
|--------|--------------|--------------|
| **Purpose** | Agent → Agent | Agent → Tools |
| **Communication** | Natural language | Function calls |
| **Decision Making** | Remote agent decides | Caller decides |
| **LLMs Required** | Both sides | Only caller |
| **Abstraction** | High (intent-based) | Low (explicit) |

### Example: "Find AI sessions at ESPC"

**With MCP** (one LLM):
```
Your LLM: "I'll call search_sessions(query='AI', conference='ESPC')"
MCP Server: [returns raw session data]
Your LLM: [formats and presents]
```

**With A2A** (two LLMs):
```
Host Agent LLM: "I'll delegate this to ConferenceHaven"
                ↓ sends natural language
ConferenceHaven LLM: "I'll search for AI sessions, filter relevance, format nicely"
                     [uses its own tools, applies its own judgment]
                ↓ returns formatted response
Host Agent LLM: [presents to user]
```

---

## ConferenceHaven A2A Architecture

### Live Demo: https://a2a.conferencehaven.com

```
┌─────────────────────────────────────────────────────────────┐
│                    Host Agent                                │
│                 a2a.conferencehaven.com                      │
│  ┌───────────────────────────────────────────────────────┐  │
│  │              Azure OpenAI (GPT-4)                      │  │
│  │                                                        │  │
│  │  Capabilities:                                         │  │
│  │  - General conference knowledge                        │  │
│  │  - Knows which conferences are in ConferenceHaven      │  │
│  │  - Decides when to delegate vs answer directly         │  │
│  │  - Handles questions about conferences NOT in CH       │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          │ A2A Protocol
                          │ POST /a2a/message:send
                          │ (natural language)
                          ▼
┌─────────────────────────────────────────────────────────────┐
│               ConferenceHaven Agent                          │
│            backend-agent.azurecontainerapps.io               │
│  ┌───────────────────────────────────────────────────────┐  │
│  │                 OpenAI (GPT-4)                         │  │
│  │                                                        │  │
│  │  Capabilities:                                         │  │
│  │  - Detailed session information                        │  │
│  │  - Speaker details                                     │  │
│  │  - Session search and filtering                        │  │
│  │  - Calendar invite generation                          │  │
│  └───────────────────────────────────────────────────────┘  │
│                          │                                   │
│                          │ MCP Tools                         │
│                          ▼                                   │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  search_sessions | get_session | send_calendar_invite │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

### How Requests Flow

| User Question | Who Handles | Why |
|---------------|-------------|-----|
| "What is ESPC about?" | Host Agent | General knowledge |
| "Find AI sessions at ESPC" | ConferenceHaven (via A2A) | Needs session database |
| "Sessions at AWS re:Invent" | Host Agent | Not in ConferenceHaven |
| "Send me a calendar invite for session 123" | ConferenceHaven (via A2A) | Needs calendar tool |
| "What hotels are near the Dublin venue?" | Host Agent | General question |

---

## A2A Protocol Implementation

### Agent Discovery

Agents advertise capabilities via an **Agent Card** at `/.well-known/agent.json`:

```bash
curl https://backend-agent.azurecontainerapps.io/.well-known/agent.json
```

```json
{
  "name": "ConferenceHaven",
  "description": "AI-powered conference session discovery and management",
  "version": "2.0.0",
  "url": "https://backend-agent.azurecontainerapps.io/a2a/message:send",
  "capabilities": {
    "streaming": false,
    "taskManagement": true
  },
  "skills": [
    {
      "id": "search_sessions",
      "name": "Search Conference Sessions",
      "description": "Search for sessions by topic, speaker, or track"
    },
    {
      "id": "send_calendar_invite",
      "name": "Send Calendar Invite",
      "description": "Send calendar invitations for sessions"
    }
  ]
}
```

### Sending A2A Messages

Per [A2A Protocol Specification](https://a2a-protocol.org/latest/definitions/):

```bash
curl -X POST https://backend-agent.azurecontainerapps.io/a2a/message:send \
  -H "Content-Type: application/json" \
  -d '{
    "message": {
      "message_id": "msg-123",
      "role": "user",
      "parts": [
        {"kind": "text", "text": "Find AI sessions at ESPC"}
      ]
    }
  }'
```

### Response (Task with Artifacts)

```json
{
  "task": {
    "id": "task-456",
    "context_id": "ctx-789",
    "status": {
      "state": "completed",
      "timestamp": "2025-11-28T20:00:00Z"
    },
    "artifacts": [
      {
        "parts": [
          {
            "kind": "text",
            "text": "I found 15 AI-related sessions at ESPC25..."
          }
        ]
      }
    ]
  }
}
```

### Task Lifecycle

Per [A2A Specification](https://a2a-protocol.org/latest/definitions/):

```
submitted → working → completed
                   ↘ failed
                   ↘ canceled
```

---

## A2A Endpoints

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/.well-known/agent.json` | GET | Agent discovery |
| `/a2a/message:send` | POST | Send message, get task |
| `/a2a/tasks/{id}` | GET | Get task status |
| `/a2a/tasks/{id}:cancel` | POST | Cancel running task |
| `/a2a/health` | GET | Health check |

---

## Building Your Own A2A Integration

### Python Client Example

```python
import httpx
import uuid

async def send_a2a_message(agent_url: str, message: str) -> dict:
    """Send a message to an A2A agent."""
    async with httpx.AsyncClient() as client:
        response = await client.post(
            f"{agent_url}/a2a/message:send",
            json={
                "message": {
                    "message_id": str(uuid.uuid4()),
                    "role": "user",
                    "parts": [{"kind": "text", "text": message}]
                }
            }
        )
        return response.json()

# Usage
result = await send_a2a_message(
    "https://backend-agent.azurecontainerapps.io",
    "Find Copilot sessions at ESPC"
)
print(result["task"]["artifacts"][0]["parts"][0]["text"])
```

### Building a Host Agent

To build your own Host Agent that delegates to ConferenceHaven:

1. **Create your agent with its own LLM**
2. **Discover ConferenceHaven** via agent card
3. **Decide when to delegate** (session queries → ConferenceHaven)
4. **Send natural language** via A2A, not tool calls

See our reference implementation: [a2a-foundry-demo](https://github.com/fabianwilliams/ConferenceHaven/tree/main/a2a-foundry-demo)

---

## References

### Protocol Specifications
- [A2A Protocol Specification](https://a2a-protocol.org/latest/) - Official specification
- [A2A Protocol Definitions](https://a2a-protocol.org/latest/definitions/) - Message/Task schemas
- [A2A Python SDK](https://github.com/a2aproject/a2a-python) - Official Python SDK

### How A2A Works with MCP
- [A2A + MCP Diagram](https://a2a-protocol.org/latest/#how-does-a2a-work-with-mcp) - Visual explanation
- [Model Context Protocol](https://modelcontextprotocol.io/) - MCP specification

### ConferenceHaven Resources
- [Live A2A Demo](https://a2a.conferencehaven.com) - Try it now
- [MCP Integration Guide](./MCP-GUIDE.md) - Direct tool access
- [GitHub Repository](https://github.com/fabianwilliams/ConferenceHaven)

---

## Contact

Questions about A2A integration?

📧 Email: **conferencehaven@adotob.com**
