# Building ConferenceHaven: A Journey Through MCP and A2A

**Author**: Fabian Williams
**Date**: November 2025
**Project**: ConferenceHaven - AI-powered conference session discovery

---

## The Vision

What if you could ask any AI assistant - Claude, ChatGPT, Copilot, or your local LLM - to find conference sessions for you? No searching through massive conference websites. No manually comparing schedules. Just ask.

That's ConferenceHaven.

**Live Today:**
- [conferencehaven.com/chat](https://conferencehaven.com/chat) - Web chat
- [mcp.conferencehaven.com](https://mcp.conferencehaven.com) - MCP integration for AI clients
- [a2a.conferencehaven.com](https://a2a.conferencehaven.com) - A2A demo with two collaborating LLMs

---

## The Art of the Possible

This document shares what I learned building ConferenceHaven - the wins, the challenges, and the opportunities I discovered. My hope is that sharing this journey helps others and contributes to making AI agent development more accessible.

### What We Built

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    ConferenceHaven - November 2025                          │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  THREE WAYS TO ACCESS:                                                      │
│                                                                             │
│  1. WEB CHAT - Just open your browser                                       │
│     conferencehaven.com/chat                                                │
│                                                                             │
│  2. MCP TOOLS - Add to Claude Desktop, LM Studio, Copilot Studio           │
│     URL: https://mcp.conferencehaven.com/api/mcp                           │
│                                                                             │
│  3. A2A AGENTS - Two LLMs collaborating intelligently                      │
│     Demo: https://a2a.conferencehaven.com                                  │
│                                                                             │
│  DATA: 4 conferences, 2,060+ sessions                                      │
│  • Microsoft Ignite 2025 (1,545 sessions)                                  │
│  • Live360 Orlando 2025 (196 sessions)                                     │
│  • TechCon365 Dallas 2025 (174 sessions)                                   │
│  • ESPC25 Dublin 2025 (145 sessions)                                       │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## The Journey: From MCP to A2A

### Chapter 1: Starting with MCP

My first goal was simple: let any AI client search conference sessions via the **Model Context Protocol (MCP)**.

**What MCP Does Well:**
- Standardized way for AI clients to call tools
- Works with Claude Desktop, LM Studio, VS Code Copilot, and more
- Clear tool definitions with JSON schemas

**The Challenge I Hit:**
The documentation focused on local (stdio) transport. I needed HTTP for remote access - no one wants to install a local package just to search conferences.

**The Solution:**
I used FastMCP directly (not through Agent Framework) to create an HTTP endpoint:

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("conferencehaven", stateless_http=True)

@mcp.tool()
def search_sessions(query: str, conference: str = None):
    """Search conference sessions by topic, speaker, or keyword."""
    # ... implementation
```

**Result:** `https://mcp.conferencehaven.com/api/mcp` - works with any MCP-compatible client!

---

### Chapter 2: The A2A Revelation

After getting MCP working, I discovered something interesting: MCP is great for **tools**, but what about exposing an **agent**?

The difference matters:
- **MCP (tools)**: Your LLM decides what to call, remote server just executes
- **A2A (agents)**: Remote agent has its own LLM, makes its own decisions

**Why This Matters:**

| Question | MCP Response | A2A Response |
|----------|--------------|--------------|
| "What is ESPC about?" | Tool doesn't know | Host Agent answers from knowledge |
| "Find AI sessions" | Returns raw data | Agent searches, filters, formats intelligently |
| "AWS re:Invent sessions" | Error: not in database | Host Agent explains it's not supported yet |

With A2A, the **remote agent can exercise judgment**. It can answer questions the tools can't handle.

---

### Chapter 3: Building A2A Ourselves

Microsoft's A2A support for Python was listed as "in the works." Rather than wait, I built it myself.

**The Two-LLM Architecture:**

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         A2A: Two LLMs Collaborating                         │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────┐         ┌─────────────────────────────────┐   │
│  │     HOST AGENT          │         │    CONFERENCEHAVEN AGENT        │   │
│  │   (Azure OpenAI)        │         │       (OpenAI)                  │   │
│  │                         │         │                                 │   │
│  │   "Should I delegate    │  A2A    │   "How should I search and     │   │
│  │    this question?"      │ ──────> │    format this response?"      │   │
│  │                         │         │                                 │   │
│  │   DECIDES: IF           │         │   DECIDES: HOW                 │   │
│  └─────────────────────────┘         └─────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

**What This Enables:**

The Host Agent at `a2a.conferencehaven.com` can:
- Answer general questions ("What is ESPC?") from its knowledge
- Delegate specific queries ("Find AI sessions") to ConferenceHaven
- Gracefully handle unsupported requests ("AWS re:Invent sessions")

**Try it yourself:** Visit [a2a.conferencehaven.com](https://a2a.conferencehaven.com)

---

## Key Learnings

### What Worked Beautifully

1. **FastMCP for HTTP endpoints** - Clean, simple, works with every MCP client
2. **Agent Card discovery** - `/.well-known/agent.json` is an elegant pattern for agent discovery
3. **Two LLMs > One LLM** - The Host Agent adds judgment that tools alone can't provide
4. **Multiple access patterns** - Different users need different entry points (web, MCP, A2A)

### Opportunities for the Ecosystem

These aren't complaints - they're opportunities I'd love to see the community address:

1. **A2A Python SDK** - I built A2A manually; an official SDK would make this easier for everyone
2. **MCP HTTP documentation** - More examples of remote MCP deployment would help
3. **Agent observability** - Production tracing for multi-agent systems is an emerging need

### What I'd Tell My Past Self

1. **Start with the user experience** - Web chat first, then expand to MCP and A2A
2. **Don't fight local-only tooling** - If something is designed for local dev, use proper production alternatives
3. **Two protocols are better than one** - MCP for tools, A2A for agents - they complement each other
4. **Build the demo** - Nothing clarifies thinking like a working implementation

---

## The Technical Stack

For those interested in the implementation:

| Component | Technology | Purpose |
|-----------|------------|---------|
| MCP Server | FastMCP + FastAPI | Tool access for AI clients |
| Web Chat | Microsoft Agent Framework | Browser-based chat |
| A2A Server | Custom Python + FastAPI | Agent-to-agent protocol |
| Host Agent | Azure OpenAI + Gradio | A2A demo interface |
| Database | Azure SQL | Session storage |
| Analytics | FastAPI + React + Auth0 | Usage insights |
| Observability | OpenTelemetry + Aspire | Production monitoring |

---

## Try It Yourself

### Easiest: Web Chat
Just visit [conferencehaven.com/chat](https://conferencehaven.com/chat)

### For AI Power Users: MCP Integration
Add to your AI client's MCP settings:
```json
{
  "mcpServers": {
    "conferencehaven": {
      "url": "https://mcp.conferencehaven.com/api/mcp"
    }
  }
}
```

### For Developers: A2A Integration
Discover the agent:
```bash
curl https://agent-chat.agreeablehill-3054e8bb.eastus2.azurecontainerapps.io/.well-known/agent.json
```

Send a message:
```bash
curl -X POST https://agent-chat.agreeablehill-3054e8bb.eastus2.azurecontainerapps.io/a2a/message:send \
  -H "Content-Type: application/json" \
  -d '{
    "message": {
      "message_id": "test-1",
      "role": "user",
      "parts": [{"kind": "text", "text": "Find AI sessions at ESPC"}]
    }
  }'
```

---

## What's Next

The AI agent ecosystem is evolving rapidly. Here's what I'm excited about:

- **More conferences** - Expanding to additional tech conferences
- **Smarter recommendations** - Using session history to suggest relevant talks
- **Deeper A2A integration** - Multi-agent workflows for conference planning
- **Community contributions** - Your feedback shapes the roadmap!

---

## Get Involved

- **Try it**: [conferencehaven.com/chat](https://conferencehaven.com/chat)
- **Integrate it**: Add the MCP server to your AI workflow
- **Give feedback**: [Open an issue](https://github.com/fabianwilliams/ConferenceHaven-Community/issues)
- **Connect**: [conferencehaven@adotob.com](mailto:conferencehaven@adotob.com)

---

## Acknowledgments

This project stands on the shoulders of giants:

- **Microsoft Agent Framework** team - For ChatAgent and the vision
- **Anthropic** - For MCP and Claude
- **Google** - For the A2A protocol specification
- **The conference organizers** - For making session data available
- **The community** - For feedback and encouragement

---

**Building the future of conference discovery, one agent at a time.**

*Fabian Williams*
*November 2025*
