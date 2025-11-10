# ConferenceHaven Community

Welcome to the **ConferenceHaven Community** repository! 👋

This is the public home for feedback, documentation, and discussions about ConferenceHaven - your AI-powered conference assistant.

**Website**: [conferencehaven.com](https://conferencehaven.com)  
**MCP Server**: `https://mcp.conferencehaven.com`

---

## What is ConferenceHaven?

**Discover conference sessions with AI** - Search thousands of technical sessions across major conferences using your favorite AI assistant. No authentication required, works with multiple AI clients, and adds sessions directly to your calendar.

### 🌟 Why ConferenceHaven?

- 🔍 **Smart Search** - Search across Microsoft Ignite, Live360, and more conferences using natural language
- 📅 **Calendar Integration** - Add sessions directly to your calendar with a single request
- 🤖 **AI-Powered** - Works with Claude Desktop, ChatGPT, LM Studio, Microsoft Copilot Studio, and more
- 🚀 **Free to Use** - No authentication required for searching and browsing sessions
- 🔒 **Privacy-First** - Use local models (LM Studio, Ollama) for complete privacy - searches never leave your computer

### 🎯 Supported Conferences

- **Microsoft Ignite** - Microsoft's flagship conference
- **Live360** - Developer & IT Pro conference series
- **ESPC25** (European SharePoint, Office 365 & Azure Conference) - May 2025, Stockholm
- More conferences added regularly!

## 📚 Documentation

- [Setup Guides](./docs/SETUP-GUIDES.md) - How to configure ConferenceHaven with different AI clients
- [FAQ](./docs/FAQ.md) - Frequently asked questions
- [Troubleshooting](./docs/TROUBLESHOOTING.md) - Common issues and solutions
- [Architecture Overview](./docs/ARCHITECTURE.md) - High-level technical overview

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

## 🌟 How It Works

ConferenceHaven implements the Model Context Protocol (MCP), which means it works as a **tool** that any AI agent can use:

```
User: "Find Copilot sessions at Microsoft Ignite"
  ↓
AI Agent (Claude, ChatGPT, etc.)
  ↓
ConferenceHaven MCP Tool
  ↓
Natural Language Response with Sessions
```

### Supported AI Clients

- **Claude Desktop** - Native MCP support
- **ChatGPT** - Via GPT Builder
- **GitHub Copilot** - VS Code integration
- **Copilot Studio** - Enterprise integration
- **LM Studio** - Local AI models (privacy-first!)

## 🚀 Quick Start

Choose your AI client and get started in minutes:

### Option 1: Microsoft Copilot Studio (Browser-based)

**Best for**: Enterprise users with Microsoft 365

1. Open [Copilot Studio](https://copilotstudio.microsoft.com/)
2. Navigate to: **Actions** → **Add an action** → **From Swagger**
3. Enter URL: `https://conferencehaven.com/swagger.json`
4. Import and start asking: *"List all conferences"*

[View detailed Copilot Studio setup guide →](docs/SETUP-GUIDES.md#-copilot-studio)

### Option 2: Claude Desktop (Native MCP)

**Best for**: Power users who want full MCP integration

1. Install Node.js if not already installed
2. Edit `~/Library/Application Support/Claude/claude_desktop_config.json`:
```json
{
  "mcpServers": {
    "conferencehaven": {
      "command": "npx",
      "args": ["-y", "@fabianwilliams/mcp-remote", "https://mcp.conferencehaven.com"]
    }
  }
}
```
3. Restart Claude Desktop
4. Look for 🔌 icon and ask: *"What conferences are available?"*

[View detailed Claude Desktop setup guide →](docs/SETUP-GUIDES.md#-claude-desktop)

### Option 3: LM Studio (Local & Private)

**Best for**: Privacy-focused users who want local AI models

1. Download [LM Studio](https://lmstudio.ai)
2. Download a model with function calling support (Llama 3.2, Mistral, Qwen)
3. Settings → MCP Servers → Add Server:
```json
{
  "mcpServers": {
    "conferencehaven": {
      "command": "npx",
      "args": ["-y", "@fabianwilliams/mcp-remote", "https://mcp.conferencehaven.com"]
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

## 📊 Roadmap

Check out our [public roadmap](https://github.com/fabianwilliams/ConferenceHaven-Community/issues?q=is%3Aissue+is%3Aopen+label%3Aroadmap) to see what's coming next!

**Current Focus:**
- Evaluation pipeline for continuous quality improvement
- More conference integrations (SharePoint Dublin, Microsoft Build, etc.)
- Session recommendations based on interests
- Speaker profiles across conferences
- Agent-to-Agent workflow integration

## ❓ Support

Need help?
1. Check the [FAQ](./docs/FAQ.md)
2. Review [Troubleshooting](./docs/TROUBLESHOOTING.md)
3. [Open an issue](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new)
4. Email: **conferencehaven@adotob.com**

## 📜 License

ConferenceHaven is free to use. This community repository is for feedback and documentation.

## 🙏 Acknowledgments

Built with ❤️ by [Fabian Williams](https://github.com/fabianwilliams) and powered by:
- Model Context Protocol (MCP)
- OpenTelemetry for observability
- Azure services
- The amazing tech conference community

---

**Let's make conference discovery better together!** 🚀
