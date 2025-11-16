# ConferenceHaven Community

Welcome to the **ConferenceHaven Community** repository! 👋

This is the public home for feedback, documentation, and discussions about ConferenceHaven - your AI-powered conference assistant.

**Website**: [conferencehaven.com](https://conferencehaven.com)
**Web Chat**: [conferencehaven.com/chat](https://conferencehaven.com/chat)
**MCP Endpoint**: `https://mcp.conferencehaven.com/api/mcp`

---

## What is ConferenceHaven?

**Discover conference sessions with AI** - Search thousands of technical sessions across major conferences using your favorite AI assistant. Works in your browser, with AI desktop clients, and supports agent-to-agent (A2A) integration.

### 🌟 Why ConferenceHaven?

- 🔍 **Smart Search** - Natural language search across Microsoft Ignite, Live360, ESPC, and more
- 📅 **Calendar Integration** - Add sessions directly to your calendar with a single request
- 🌐 **Multiple Ways to Connect** - Web chat, AI desktop clients (Claude, ChatGPT, LM Studio), or A2A integration
- 🚀 **Zero Friction** - Web chat requires no installation or authentication
- 🤖 **A2A Ready** - Conference organizers can integrate via agent-to-agent protocols
- 🔒 **Privacy Options** - Use local models (LM Studio, Ollama) for complete privacy

### 🎯 Supported Conferences

- **TechCon365 Dallas 2025** - Nov 3-7, 2025 (174+ sessions loaded)
- **Microsoft Ignite 2025** - Nov 18-21, 2025 (coming soon)
- **Live360 Orlando 2025** - Nov 16-21, 2025 (coming soon)
- **ESPC25 Dublin** - Dec 1-4, 2025 (coming soon)
- More conferences added regularly!

---

## 🚀 Quick Start - Choose Your Path

### Path 1: Web Chat (Easiest - No Installation!)

**Best for**: Everyone who wants instant access

👉 **Just visit: [conferencehaven.com/chat](https://conferencehaven.com/chat)**

No downloads. No setup. No authentication. Just open your browser and start asking:

```
You: "Show me AI sessions at TechCon365"
Agent: [Returns sessions with speakers, times, locations]

You: "Send me a calendar invite for session 957 to email@example.com"
Agent: ✅ Calendar invite sent! Check your email.
```

**Why web chat?**
- ✅ Works on any device (desktop, mobile, tablet)
- ✅ HTTPS trusted by all browsers (no security warnings)
- ✅ Accessible at conferences on your phone
- ✅ Zero setup - instant gratification

---

### Path 2: AI Desktop Clients (MCP Integration)

**Best for**: Power users who want ConferenceHaven integrated into their AI workflow

ConferenceHaven implements the **Model Context Protocol (MCP)**, which means it works as a tool inside your favorite AI assistant:

```
User: "Find Copilot sessions at Microsoft Ignite"
  ↓
AI Agent (Claude Desktop, ChatGPT, LM Studio, etc.)
  ↓
ConferenceHaven MCP Tool
  ↓
Natural Language Response with Sessions
```

#### Supported AI Clients

| Client | Best For | Setup Time | Privacy |
|--------|----------|------------|---------|
| **Claude Desktop** | Power users | 5 min | Cloud |
| **LM Studio** | Privacy-focused | 5 min | Local (100% private) |
| **ChatGPT** | Plus/Pro users | 2 min | Cloud |
| **Copilot Studio** | Enterprise | 2 min | Cloud |
| **GitHub Copilot** | Developers | 5 min | Cloud |

#### Quick Setup Options

**A. Native Connectors (Recommended)**

Modern AI clients support **direct HTTP MCP connections** - no packages, no Node.js:

| Platform | Plan Required | Status | Setup |
|----------|--------------|--------|-------|
| **Claude Desktop** | Any (Free/Pro) | ✅ Stable | Add `https://mcp.conferencehaven.com/api/mcp` as custom connector |
| **ChatGPT** | Plus/Pro ($20/mo) | ✅ Stable | Add `https://mcp.conferencehaven.com/api/mcp` in Apps & Connectors |
| **LM Studio** | Free | ✅ Stable | Add `https://mcp.conferencehaven.com/api/mcp` in MCP settings |
| **Copilot Studio** | Microsoft 365 | ✅ Stable | Import `https://conferencehaven.com/swagger.json` |

✨ **Why this is better**: Direct HTTP connections are faster, simpler, and don't require Node.js installation!

**B. Universal Setup (All Users - Free & Paid)**

Works with **any AI client**. Requires one-time Node.js install (~5 minutes).

**Why Node.js?** It's a bridge that lets AI clients talk to ConferenceHaven. Over 20 million developers use it - as simple as installing any other app.

[Jump to detailed setup instructions ↓](#detailed-setup-instructions)

---

### Path 3: Agent-to-Agent (A2A) Integration

**Best for**: Conference organizers and developers building multi-agent systems

Integrate ConferenceHaven into your own agent system via **Agent-to-Agent (A2A)** protocols:

```python
# Your agent delegates to ConferenceHaven (session discovery expert)
response = await your_agent.delegate_to(
    agent_url="https://conferencehaven.com/api/agent",
    query="What AI sessions are at TechCon365?",
    protocol="ACP"  # Agent Communication Protocol
)
```

**Example A2A scenario**:
1. Conference organizer deploys AI assistant on their website
2. Attendee asks: "What sessions are happening today?"
3. Organizer's agent delegates to ConferenceHaven (specialized session expert)
4. ConferenceHaven returns structured session data
5. Organizer's agent formats response for attendee

**Why A2A?**
- ✅ Conference organizers don't need to duplicate session databases
- ✅ Agents can specialize (ConferenceHaven = session discovery expert)
- ✅ Users get better answers through agent collaboration
- ✅ Builds toward future multi-agent ecosystems

**Interested in A2A integration?** Email: **conferencehaven@adotob.com**

---

## 📚 Documentation

- [Setup Guides](./docs/SETUP-GUIDES.md) - How to configure ConferenceHaven with different AI clients
- [FAQ](./docs/FAQ.md) - Frequently asked questions
- [Troubleshooting](./docs/TROUBLESHOOTING.md) - Common issues and solutions
- [Architecture Overview](./docs/ARCHITECTURE.md) - High-level technical overview

---

## Detailed Setup Instructions

Choose your AI client and get started:

### Option 1: Microsoft Copilot Studio (Browser-based)

**Best for**: Enterprise users with Microsoft 365

1. Open [Copilot Studio](https://copilotstudio.microsoft.com/)
2. Navigate to: **Actions** → **Add an action** → **From Swagger**
3. Enter URL: `https://conferencehaven.com/swagger.json`
4. Import and start asking: *"List all conferences"*

[View detailed Copilot Studio setup guide →](docs/SETUP-GUIDES.md#-copilot-studio)

### Option 2: Claude Desktop

**Best for**: Power users who want full MCP integration

**Prerequisites**: Node.js (first-time only, ~5 minutes)
- **Don't have Node.js?** Download from [nodejs.org](https://nodejs.org) and run the installer
- **Already have it?** Check by running: `node --version`

1. Edit your Claude config file:
   - **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

2. Add this configuration:
```json
{
  "mcpServers": {
    "conferencehaven": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.conferencehaven.com/"
      ]
    }
  }
}
```
3. Restart Claude Desktop
4. Look for 🔌 icon and ask: *"What conferences are available?"*

[View detailed Claude Desktop setup guide →](docs/SETUP-GUIDES.md#-claude-desktop)

### Option 3: LM Studio (Local & Private)

**Best for**: Privacy-focused users who want local AI models

**Prerequisites**: Node.js (first-time only, ~5 minutes)
- **Don't have Node.js?** Download from [nodejs.org](https://nodejs.org) and install
- **Already have it?** Check with: `node --version`

1. Download [LM Studio](https://lmstudio.ai)
2. Download a model with function calling support (Llama 3.2, Mistral, Qwen)
3. Settings → MCP Servers → Add Server:
```json
{
  "mcpServers": {
    "conferencehaven": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.conferencehaven.com/"
      ]
    }
  }
}
```
4. Start chat and try: *"Find sessions about AI"*

🛡️ **Complete Privacy**: Your searches never leave your computer—no cloud providers, no tracking!

[View detailed LM Studio setup guide →](docs/SETUP-GUIDES.md#-lm-studio-local-ai)

### Also Works With:

- **ChatGPT** - Via GPT Actions
- **GitHub Copilot** - VS Code integration
- **Ollama** - Local model runtime
- Any MCP-compatible client

[See all setup guides →](docs/SETUP-GUIDES.md)

---

## 💬 Providing Feedback

We welcome all feedback! Here's how to contribute:

### 🐛 Bug Reports
Found a bug? [Create a bug report](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=bug_report.md)

### ✨ Feature Requests
Have an idea for a new feature? [Submit a feature request](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=feature_request.md)

### 🎤 Conference Requests
Want to add your conference to ConferenceHaven? [Request a conference](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=conference_request.md)

### 💭 General Feedback
Just want to share thoughts? [Open a general feedback issue](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=general_feedback.md)

### 📧 No GitHub Account?
No problem! Email feedback to: **conferencehaven@adotob.com**

---

## 🤝 Contributing

We appreciate contributions from the community! Here's how you can help:

### Documentation
- Improve existing guides
- Add tutorials
- Translate documentation

### Feedback
- Report bugs you encounter
- Suggest features that would help you
- Share your use cases

### Spread the Word
- Share ConferenceHaven with your network
- Write blog posts or tweets about your experience
- Present at user groups or conferences

---

## 📊 Roadmap

Check out our [public roadmap](https://github.com/fabianwilliams/ConferenceHaven-Community/issues?q=is%3Aissue+is%3Aopen+label%3Aroadmap) to see what's coming next!

**Current Focus:**
- ✅ Web chat interface (LIVE!)
- 🔄 Agent-to-Agent (A2A) integration guide
- 🔄 More conference integrations (Ignite, ESPC, Live360)
- 📋 Session recommendations based on interests
- 📋 Speaker profiles across conferences
- 📋 Embeddable widget for conference websites

---

## ❓ Support

Need help?
1. Check the [FAQ](./docs/FAQ.md)
2. Review [Troubleshooting](./docs/TROUBLESHOOTING.md)
3. [Open an issue](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new)
4. Email: **conferencehaven@adotob.com**

---

## 📜 License

ConferenceHaven is free to use. This community repository is for feedback and documentation.

---

## 🙏 Acknowledgments

Built with ❤️ by [Fabian Williams](https://github.com/fabianwilliams) and powered by:
- **Microsoft Agent Framework** - Agent orchestration
- **Model Context Protocol (MCP)** - Standardized agent-tool communication
- **OpenTelemetry** - Observability and tracing
- **Azure** - Cloud infrastructure
- The amazing tech conference community

---

**Let's make conference discovery accessible to everyone - whether you're using a browser, desktop AI client, or building agent-to-agent systems!** 🚀
