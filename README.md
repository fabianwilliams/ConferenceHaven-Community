# ConferenceHaven Community

Welcome to the ConferenceHaven community repository! This is the public home for feedback, feature requests, documentation, and community discussions about ConferenceHaven.

## 🎯 What is ConferenceHaven?

ConferenceHaven is a free, open-source MCP (Model Context Protocol) tool that helps conference speakers, attendees, and organizers discover sessions across multiple tech conferences using natural language AI assistants.

**Key Features:**
- 🔍 Search sessions across multiple conferences (ESPC, Microsoft Ignite, TechCon365, etc.)
- 📅 Add sessions to your calendar with one command
- 🤖 Works with Claude Desktop, ChatGPT, GitHub Copilot, LM Studio, and more
- 🔒 Privacy-first: Works with local AI models (no data leaves your machine)
- 📊 Analytics for conference organizers
- 🆓 Completely free, no paywalls or premium tiers

**Website:** [conferencehaven.com](https://conferencehaven.com)

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
No problem! Email feedback to: **fabian[at]conferencehaven.com** (replace [at] with @)

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

### Claude Desktop
1. Open `~/Library/Application Support/Claude/claude_desktop_config.json`
2. Add ConferenceHaven to your MCP servers:
```json
{
  "mcpServers": {
    "conferencehaven": {
      "command": "npx",
      "args": ["-y", "@fabianwilliams/mcp-remote", "http://confhaven-mcp.io"]
    }
  }
}
```
3. Restart Claude Desktop
4. Try: "What conferences are available?"

See [Setup Guides](./docs/SETUP-GUIDES.md) for other AI clients.

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
4. Email: **fabian[at]conferencehaven.com**

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
