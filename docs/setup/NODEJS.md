# Node.js NOT Required for ConferenceHaven

**TL;DR:** ❌ **You DO NOT need Node.js** for ConferenceHaven!

ConferenceHaven is a **remote HTTP-based MCP server**. You just add a URL to your AI client config - that's it!

---

## Why No Node.js?

ConferenceHaven uses **Streaming HTTP** transport (FastMCP + FastAPI), which means:

- ✅ **Remote server** hosted on Azure Container Apps
- ✅ **Just add URL** - no local installation
- ✅ **Always up-to-date** - server updates automatically
- ✅ **No dependencies** - works out of the box

```json
{
  "mcpServers": {
    "conference-haven": {
      "url": "https://mcp.conferencehaven.com/api/mcp"
    }
  }
}
```

That's it! No `npx`, no `node`, no local server.

---

## How is This Different?

### ❌ Traditional MCP Servers (Stdio Transport)
```json
{
  "mcpServers": {
    "some-local-server": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-example"]
    }
  }
}
```
**Requires:** Node.js installed, local process, stdio communication

### ✅ ConferenceHaven (HTTP Transport)
```json
{
  "mcpServers": {
    "conference-haven": {
      "url": "https://mcp.conferencehaven.com/api/mcp"
    }
  }
}
```
**Requires:** Nothing! Just add the URL.

---

## Architecture

```
AI Client (ChatGPT/Claude)
    ↓
    HTTPS (Streaming)
    ↓
ConferenceHaven MCP Server (Azure Container Apps)
    ↓
FastMCP + FastAPI + Python 3.11
    ↓
Azure SQL Database
```

**Key Points:**
- Server runs on **Azure** (not your machine)
- Communication via **HTTPS** (not stdio)
- **Stateless** HTTP requests
- **OAuth 2.1** authentication (when needed)

---

## Setup Instructions

**All you need to do:**

1. Add URL to your AI client config
2. Restart the client
3. Done!

**Detailed guides:**
- [ChatGPT Desktop Setup](CHATGPT.md)
- [Claude Desktop Setup](CLAUDE-DESKTOP.md)
- [LM Studio Setup](LM-STUDIO.md)
- [Copilot Studio Setup](COPILOT-STUDIO.md)

---

## Why This Document Exists

This document clarifies a common misconception. Many MCP servers run locally via Node.js/npx, but ConferenceHaven doesn't.

**If you see instructions mentioning:**
- ❌ "Install Node.js"
- ❌ "Run npx @modelcontextprotocol/..."
- ❌ "command" or "args" in config

**Those are WRONG for ConferenceHaven!** We use HTTP transport.

---

## Technical Details

**MCP Transport Protocol:** Streaming HTTP (Server-Sent Events)
**FastMCP Documentation:** https://github.com/jlowin/fastmcp
**MCP Specification:** https://spec.modelcontextprotocol.io/specification/architecture/transports/

**How Streaming HTTP Works:**
1. Client opens HTTP connection to `https://mcp.conferencehaven.com/api/mcp`
2. Server streams JSON-RPC messages via Server-Sent Events (SSE)
3. Client sends requests via HTTP POST
4. Server responds via SSE stream
5. Connection stays open for bi-directional communication

**Advantages over stdio:**
- ✅ No local process management
- ✅ Works over internet/VPN
- ✅ Easier authentication (OAuth headers)
- ✅ Server-side scaling
- ✅ Zero client-side dependencies

---

## For Developers Building MCP Servers

If you're building your own MCP server, you have two options:

### Option 1: Stdio Transport (Requires Node.js/Python locally)
```json
{
  "mcpServers": {
    "my-server": {
      "command": "node",
      "args": ["server.js"]
    }
  }
}
```

### Option 2: HTTP Transport (Remote server - Recommended!)
```json
{
  "mcpServers": {
    "my-server": {
      "url": "https://my-server.example.com/api/mcp"
    }
  }
}
```

**ConferenceHaven uses Option 2** - HTTP transport with FastMCP.

---

## Questions?

- **GitHub Issues:** [ConferenceHaven-Community/issues](https://github.com/fabianwilliams/ConferenceHaven-Community/issues)
- **Documentation:** [Full docs](../../README.md)
- **FastMCP Guide:** https://github.com/jlowin/fastmcp

---

**Quick Links:**
- [← Back to Setup Guides](../SETUP-GUIDES.md)
- [ChatGPT Setup](CHATGPT.md) (No Node.js needed!)
- [Claude Desktop Setup](CLAUDE-DESKTOP.md) (No Node.js needed!)
- [Architecture Documentation](../ARCHITECTURE.md)
