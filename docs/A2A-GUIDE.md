# Agent-to-Agent (A2A) Integration Guide

**ConferenceHaven supports the A2A (Agent-to-Agent) protocol** - enabling your AI agents to discover and collaborate with ConferenceHaven as a specialized conference session expert.

---

## What is A2A?

**A2A (Agent-to-Agent Protocol)** is an open standard that enables AI agents to:
- **Discover** other agents and their capabilities
- **Communicate** using standardized message formats
- **Collaborate** on complex tasks through delegation

Think of it like HTTP for agents - a common language that lets AI assistants work together regardless of who built them.

### A2A vs MCP: Complementary Protocols

| Protocol | Purpose | Use Case |
|----------|---------|----------|
| **MCP** | Agent → Tools | Your AI uses ConferenceHaven's search tools |
| **A2A** | Agent → Agent | Your AI delegates to ConferenceHaven as an expert |

**Together they enable**: An orchestrator agent can discover ConferenceHaven via A2A, then delegate conference-related tasks, which ConferenceHaven handles using its MCP tools.

---

## ConferenceHaven A2A Capabilities

### Agent Discovery

ConferenceHaven exposes an **Agent Card** that describes its capabilities:

```bash
curl https://mcp.conferencehaven.com/.well-known/agent.json
```

The agent card tells your system:
- **What ConferenceHaven can do** (skills)
- **How to communicate** (endpoint URL)
- **Authentication requirements** (none for public tools)

### Available Skills

| Skill | Description | Example Use |
|-------|-------------|-------------|
| `search_sessions` | Search conference sessions by topic, speaker, track | "Find AI workshops at ESPC" |
| `get_session` | Get detailed information about a specific session | "Tell me more about session 1234" |
| `list_conferences` | List all available conferences | "What conferences are available?" |
| `send_calendar_invite` | Send calendar invites via email | "Add session 1234 to my calendar" |

---

## Integration Patterns

### Pattern 1: Multi-Agent Orchestration

Your host agent routes requests to specialized agents:

```
User: "I'm going to Microsoft Ignite. Find AI sessions and add the best one to my calendar."

┌─────────────────────┐
│  Your Host Agent    │ ← Receives user request
│  (Orchestrator)     │
└──────────┬──────────┘
           │
           │ Recognizes: "conference-related"
           │ Discovers: ConferenceHaven via agent card
           ▼
┌─────────────────────┐
│  ConferenceHaven    │ ← Specialist agent
│  (Conference Expert)│
└──────────┬──────────┘
           │
           │ 1. search_sessions("AI", conference="ignite")
           │ 2. Ranks results
           │ 3. send_calendar_invite(best_session, email)
           ▼
┌─────────────────────┐
│  Response to User   │
│  "Added 'AI in..."  │
└─────────────────────┘
```

### Pattern 2: Conference Website Integration

Conference organizers embed ConferenceHaven into their site:

```
Attendee: "What sessions are happening in Room A tomorrow?"

┌─────────────────────┐
│  Conference         │ ← Organizer's AI assistant
│  Website Chatbot    │
└──────────┬──────────┘
           │
           │ A2A delegation
           ▼
┌─────────────────────┐
│  ConferenceHaven    │ ← Handles session queries
└──────────┬──────────┘
           │
           │ Formatted response
           ▼
┌─────────────────────┐
│  Attendee sees      │
│  tomorrow's Room A  │
│  sessions           │
└─────────────────────┘
```

### Pattern 3: Personal AI Assistant

End users integrate ConferenceHaven into their AI workflow:

```
User: "I'm interested in Power Platform. What should I attend at TechCon365?"

┌─────────────────────┐
│  User's Personal    │ ← Claude, ChatGPT, custom agent
│  AI Assistant       │
└──────────┬──────────┘
           │
           │ Discovers ConferenceHaven
           ▼
┌─────────────────────┐
│  ConferenceHaven    │
│  search + recommend │
└─────────────────────┘
```

---

## Getting Started

### Step 1: Discover the Agent

Fetch the agent card to understand capabilities:

```python
import httpx

async def discover_conferencehaven():
    async with httpx.AsyncClient() as client:
        response = await client.get(
            "https://mcp.conferencehaven.com/.well-known/agent.json"
        )
        agent_card = response.json()

        print(f"Agent: {agent_card['name']}")
        print(f"Skills: {[s['id'] for s in agent_card['skills']]}")

        return agent_card
```

### Step 2: Communicate via MCP

ConferenceHaven uses MCP as its tool protocol. Send requests to the MCP endpoint:

```python
async def search_sessions(query: str):
    async with httpx.AsyncClient() as client:
        response = await client.post(
            "https://mcp.conferencehaven.com/api/mcp",
            json={
                "jsonrpc": "2.0",
                "method": "tools/call",
                "params": {
                    "name": "search_sessions",
                    "arguments": {"query": query, "limit": 10}
                },
                "id": "1"
            }
        )
        return response.json()
```

### Step 3: Handle Responses

Parse the MCP response and present to your user:

```python
result = await search_sessions("AI workshops")

# Extract session data from MCP response
sessions = json.loads(result["result"]["content"][0]["text"])

for session in sessions[:5]:
    print(f"- {session['title']} by {session['speakers']}")
```

---

## Use Cases

### For Conference Organizers

- **Embed** ConferenceHaven into your conference website's AI assistant
- **Reduce** the need to build custom session search
- **Benefit** from continuous improvements to search quality
- **Track** analytics on session interest

### For Enterprise Developers

- **Integrate** conference discovery into internal AI tools
- **Enable** employees to find relevant sessions across multiple conferences
- **Automate** calendar scheduling for team events

### For AI Platform Builders

- **Add** ConferenceHaven as a specialist agent in your multi-agent system
- **Demonstrate** A2A interoperability in your platform
- **Provide** conference discovery as a built-in capability

---

## Technical Details

### Protocol Standards

ConferenceHaven implements:
- **A2A Protocol** ([a2a-protocol.org](https://a2a-protocol.org/)) - Agent discovery and metadata
- **MCP** ([modelcontextprotocol.io](https://modelcontextprotocol.io/)) - Tool communication
- **OpenTelemetry** - Observability and tracing

### Endpoints

| Endpoint | Purpose |
|----------|---------|
| `/.well-known/agent.json` | A2A agent card (discovery) |
| `/agent.json` | A2A agent card (convenience alias) |
| `/api/mcp` | MCP tool endpoint |

### Authentication

- **Public tools** (search, list, get): No authentication required
- **Analytics tools**: Requires organizer API key

---

## Resources

- [A2A Protocol Specification](https://a2a-protocol.org/)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [Microsoft Agent Framework](https://learn.microsoft.com/en-us/agent-framework/)
- [Google A2A Announcement](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/)

---

## Contact

Interested in A2A integration? Have questions?

📧 Email: **conferencehaven@adotob.com**

We're happy to help you integrate ConferenceHaven into your agent ecosystem!
