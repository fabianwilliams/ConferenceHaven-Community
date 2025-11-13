# ConferenceHaven 🎯

**Trust-First AI Agent System for Conference Discovery**

[![Live Demo](https://img.shields.io/badge/Live%20Demo-conferencehaven.com-blue?style=for-the-badge)](https://conferencehaven.com/chat)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Agent Framework](https://img.shields.io/badge/Microsoft-Agent%20Framework-purple?style=for-the-badge)](https://github.com/microsoft/agent-framework)
[![A2A Ready](https://img.shields.io/badge/A2A-Ready-orange?style=for-the-badge)](docs/A2A-INTEGRATION.md)

---

## 📖 Read the Story

**Building Trust-First Agent Systems for the A2A Future**

👉 **[Read the full article](LINKEDIN-ARTICLE.md)** — Why trust matters more than features, and how we're building for the agent-to-agent future.

**TL;DR**: Conference catalogs are overwhelming. We have AI in 2025, yet attendees still ctrl+F through 400-page PDFs. ConferenceHaven solves this with a **web-based AI agent** that requires zero setup and is ready for agent-to-agent (A2A) communication.

**Key insight**: People trust web pages more than desktop apps. We built for the TOP of the trust hierarchy—HTTPS in a browser, accessible to everyone, ready for both humans and agents.

---

## 🚀 Quick Start

### For Humans (Web Chat)

Visit **[conferencehaven.com/chat](https://conferencehaven.com/chat)** and start asking:

```
You: "Show me AI sessions at Microsoft Ignite"
Agent: [Returns 3 relevant sessions with speakers, times, locations]

You: "Send me a calendar invite for session 957"
Agent: ✅ Calendar invite sent! Check your email.
```

**That's it.** No installation, no configuration, no command-line tools.

### For Agents (A2A Integration)

Integrate ConferenceHaven into your own agent system:

```python
# Your agent delegates to ConferenceHaven agent (A2A handoff)
response = await your_agent.delegate_to(
    agent_url="https://conferencehaven.com/api/agent",
    query="What AI sessions are at TechCon365?",
    protocol="ACP"  # Agent Communication Protocol
)
```

**Example A2A scenario**:
- Conference organizer deploys their own AI assistant
- Attendee asks: "What sessions are happening today?"
- Organizer's agent delegates to ConferenceHaven (specialized session expert)
- ConferenceHaven returns structured data
- Organizer's agent formats response for user

See **[A2A Integration Guide](docs/A2A-INTEGRATION.md)** for details.

---

## 🏗️ Architecture (High-Level)

### Trust-First Design for Humans and Agents

```
┌──────────────────────────────────────────────────────────────┐
│  HUMAN USERS (Browser - Desktop or Mobile)                   │
│  https://conferencehaven.com/chat                            │
│  - Zero installation                                         │
│  - HTTPS trusted by browsers                                 │
│  - Mobile-friendly (discover sessions on expo floor)         │
└──────────────────────────┬───────────────────────────────────┘
                           │
                           ↓ HTTPS (trusted)

┌──────────────────────────────────────────────────────────────┐
│  CONFERENCE AGENTS (Agent-to-Agent Future)                   │
│  ESPC Agent, Ignite Agent, TechCon365 Agent, etc.            │
│                                                              │
│  Scenario: Conference organizer's AI assistant delegates     │
│  to ConferenceHaven for session discovery instead of         │
│  duplicating session database.                               │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐  │
│  │  Attendee: "What AI sessions are happening?"         │  │
│  │       ↓                                               │  │
│  │  Organizer Agent → ConferenceHaven Agent (A2A)       │  │
│  │       ↓                                               │  │
│  │  Returns: Structured session data                    │  │
│  └────────────────────────────────────────────────────────┘  │
└──────────────────────────┬───────────────────────────────────┘
                           │
                           ↓ ACP or OpenAI Agents handoff

┌──────────────────────────────────────────────────────────────┐
│  CONFERENCEHAVEN AGENT (Microsoft Agent Framework)           │
│  - Understands natural language (human queries)              │
│  - Accepts structured A2A requests (agent queries)           │
│  - Orchestrates tool calls                                   │
│  - Returns formatted or structured results                   │
└──────────────────────────┬───────────────────────────────────┘
                           │
                           ↓ Tool calls

┌──────────────────────────────────────────────────────────────┐
│  DATA & SERVICES LAYER                                       │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐   │
│  │ Session      │  │ Calendar     │  │ Multi-Conference │   │
│  │ Search       │  │ Invites      │  │ Support          │   │
│  │ (Azure SQL)  │  │ (MS Graph)   │  │ (Extensible)     │   │
│  └──────────────┘  └──────────────┘  └──────────────────┘   │
└──────────────────────────────────────────────────────────────┘
```

---

## ✨ Features

### For Attendees
- 🔍 **Natural Language Search**: "Show me AI sessions" instead of ctrl+F through PDFs
- 📧 **Calendar Invites**: Get .ics files sent to your email in seconds
- 📱 **Mobile-First**: Discover sessions while walking the expo floor
- 🌐 **Multi-Conference**: TechCon365, Microsoft Ignite, ESPC, Live360 (and growing)
- ⚡ **Zero Friction**: No downloads, no setup—just open a browser

### For Conference Organizers
- 🤖 **AI Assistant Integration**: Your agent can delegate to ConferenceHaven (A2A)
- 📊 **Analytics**: See what sessions attendees are searching for
- 🔌 **Embeddable Widget**: Add `<iframe>` to your website (coming soon)
- 💰 **Zero Infrastructure**: No need to duplicate session databases

### For Developers (A2A)
- 🔗 **Agent-to-Agent Ready**: Supports ACP and OpenAI Agents handoff protocols
- 🛠️ **Specialized Agent**: Session discovery expert that other agents can delegate to
- 📖 **Open Source**: See how to build trust-first agent systems
- 🧪 **Extensible**: Add your own conferences and data sources

---

## 🎯 Why ConferenceHaven?

### 1. **Trust Over Features**

**The Trust Hierarchy (2025):**
```
MOST TRUSTED
    ↓
📱 Web browser (HTTPS) ← ConferenceHaven is here
    ↓
🏢 App Store
    ↓
📦 Package managers
    ↓
💾 Downloaded .exe files
    ↓
LEAST TRUSTED
```

We built ConferenceHaven for the **top** of the trust hierarchy. No downloads, no security warnings, no friction.

### 2. **Accessibility for Everyone**

- **Non-technical users**: Just open a web page
- **Mobile users**: Discover sessions at the conference
- **Conference organizers**: Embed on your website
- **Developers**: Integrate via A2A protocols

### 3. **Building for the A2A Future**

We're not just building for humans. We're building for **agent-to-agent communication**.

**Why specialization matters:**
- Not every agent can know everything
- ConferenceHaven is a **session discovery expert**
- Other agents can delegate to it (A2A handoff)
- Users get better answers through agent collaboration

---

## 🌍 Supported Conferences

| Conference | Dates | Location | Sessions |
|------------|-------|----------|----------|
| **TechCon365 Dallas 2025** | Nov 3-7, 2025 | Dallas, TX | 174+ |
| **Microsoft Ignite 2025** | Nov 18-21, 2025 | San Francisco, CA | Coming soon |
| **ESPC25 Dublin** | Dec 1-4, 2025 | Dublin, Ireland | Coming soon |
| **Live360 Orlando 2025** | Nov 16-21, 2025 | Orlando, FL | Coming soon |

Want to add your conference? See **[Contributing Guide](CONTRIBUTING.md)**.

---

## 💡 Use Cases

### Scenario 1: Conference Attendee (Human)

```
Attendee opens conferencehaven.com/chat on their phone

You: "What AI sessions are happening this afternoon?"
Agent: I found 3 AI sessions this afternoon at TechCon365:

1. Getting Started with MCP
   Speaker: Fabian Williams
   Thursday, Nov 6, 2:20 PM - Room G7

2. AI in SharePoint Online
   Speaker: John Doe
   Thursday, Nov 6, 3:30 PM - Room A3

You: "Add session 1 to my calendar at attendee@example.com"
Agent: ✅ Calendar invite sent to attendee@example.com! Check your inbox.
```

**Result**: Attendee planned their afternoon in 30 seconds without opening a PDF.

### Scenario 2: Conference Organizer (Agent-to-Agent)

```
Conference organizer deploys AI assistant on their website

Visitor: "What sessions are about security?"
Organizer Agent: [Delegates to ConferenceHaven via A2A]
ConferenceHaven Agent: [Returns 5 security sessions with structured data]
Organizer Agent: [Formats response for visitor + adds booth locations]
```

**Result**: Organizer's agent provides better answers without duplicating session databases.

### Scenario 3: Multi-Agent Collaboration (Future)

```
Personal AI assistant: "I need to plan my week at Ignite"
    ↓ delegates to
ConferenceHaven Agent: "Here are sessions matching your interests"
    ↓ sends structured data to
Calendar Agent: "Added 8 sessions to your calendar, flagged conflicts"
    ↓ coordinates with
Travel Agent: "Booked dinner reservations near venue for free evenings"
```

**Result**: Agent collaboration creates seamless conference planning.

---

## 🛠️ Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Agent Framework** | Microsoft Agent Framework | Agent orchestration, tool calling |
| **Web Frontend** | React/TypeScript | Web chat interface |
| **Backend API** | FastAPI (Python) | RESTful API, A2A endpoint |
| **Database** | Azure SQL Database | Session data, conferences |
| **Calendar Service** | Microsoft Graph API | Send .ics invites via email |
| **Observability** | OpenTelemetry + Aspire | Distributed tracing, metrics |
| **Deployment** | Azure Container Apps | Serverless container hosting |

---

## 📚 Documentation

- **[Architecture Overview](docs/ARCHITECTURE.md)** - Detailed system design
- **[A2A Integration Guide](docs/A2A-INTEGRATION.md)** - Connect your agent to ConferenceHaven
- **[Contributing Guide](CONTRIBUTING.md)** - Add conferences, improve features
- **[Deployment Guide](docs/DEPLOYMENT.md)** - Run your own instance
- **[API Reference](docs/API.md)** - HTTP endpoints for web and A2A

---

## 🤝 Contributing

We welcome contributions! Here's how to help:

1. **Add more conferences**: Submit session data from upcoming conferences
2. **Improve A2A protocols**: Test ACP, OpenAI Agents handoff
3. **Build integrations**: Embed ConferenceHaven in your agent system
4. **Report issues**: Found a bug? Open an issue on GitHub

See **[CONTRIBUTING.md](CONTRIBUTING.md)** for details.

---

## 🗺️ Roadmap

### Phase 3 (Current - November 2025)
- ✅ Web chat interface live
- ✅ Agent Framework backend deployed
- ✅ TechCon365 Dallas 2025 sessions loaded
- 🔄 Community documentation (this repo!)

### Phase 4 (December 2025)
- [ ] A2A integration guide with code samples
- [ ] Submit to [microsoft/agent-framework samples](https://github.com/microsoft/agent-framework)
- [ ] Add Microsoft Ignite, ESPC, Live360 sessions
- [ ] Embeddable widget for conference websites

### Phase 5 (2026)
- [ ] Multi-conference federation (A2A session sharing)
- [ ] Voice input for mobile users
- [ ] ACP and OpenAI Agents handoff protocol support
- [ ] Conference organizer partnerships

---

## 📄 License

MIT License - See [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgments

- **Microsoft Agent Framework** - For making agent orchestration accessible
- **Model Context Protocol (MCP)** - For standardizing agent-tool communication
- **Claude Code & GitHub Copilot** - AI pair programmers that built this with me
- **TechCon365** - For inspiring this project with 400-page session catalogs 😅

---

## 📬 Contact

**Fabian Williams**
- LinkedIn: [fabianwilliams](https://linkedin.com/in/fabianwilliams)
- GitHub: [@fabianwilliams](https://github.com/fabianwilliams)
- Conference Speaker: TechCon365, Microsoft Ignite, ESPC

**Questions?** Open an issue on GitHub or DM me on LinkedIn.

**A2A Integration inquiries?** Let's talk about how your agent can collaborate with ConferenceHaven.

---

**Built with ❤️ for conference attendees, organizers, and the agent-to-agent future** 🤖🤝🤖

*Making conference discovery accessible to everyone—humans and agents alike.*
