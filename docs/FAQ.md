# Frequently Asked Questions

Common questions about ConferenceHaven.

---

## General Questions

### What is ConferenceHaven?

ConferenceHaven is an MCP (Model Context Protocol) server that lets AI assistants search conference sessions and add them to your calendar. Think of it as giving your AI assistant the ability to be your conference buddy - it can find sessions you'd love, remember details, and even send you calendar invites.

### Is it free?

Yes! ConferenceHaven is free for attendees. Conference organizers have paid tiers for analytics and management features.

### What conferences are supported?

Currently:
- **Microsoft ESPC25** (European SharePoint, Office 365 & Azure Conference) - May 2025, Stockholm
- More conferences coming soon!

Want your conference added? [Request it here](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=conference_request.md).

### Do I need to create an account?

Nope! No sign-up required. Just connect your AI assistant and start searching.

### Is my data private?

Conference session data is public (pulled from conference websites). Your queries and calendar invites are processed but not stored long-term. See our [Privacy Policy](#privacy--security) below.

---

## Setup & Configuration

### Which AI assistants work with ConferenceHaven?

- ✅ **Claude Desktop** - Native MCP support
- ✅ **ChatGPT** - Via GPT Actions
- ✅ **GitHub Copilot** - VS Code integration
- ✅ **Copilot Studio** - Enterprise Microsoft 365
- ✅ **LM Studio** - Local AI models
- ✅ Any AI that supports MCP or OpenAPI

See our [Setup Guides](SETUP-GUIDES.md) for step-by-step instructions.

### What's the MCP server URL?

```
https://mcp.conferencehaven.com
```

Use this URL in your MCP client configuration.

### Do I need to install anything?

**For Claude Desktop / LM Studio / GitHub Copilot**:
- You need `npx` (comes with Node.js)
- Install Node.js from [nodejs.org](https://nodejs.org) if you don't have it

**For ChatGPT / Copilot Studio**:
- No installation needed! Just configure the OpenAPI/Swagger endpoint

### I updated my config but nothing changed?

Restart your AI client completely. Most changes require a full restart, not just a refresh.

---

## Using ConferenceHaven

### How do I search for sessions?

Just ask naturally! Examples:
- "Find sessions about Copilot"
- "Show me AI sessions at ESPC25"
- "Which sessions cover SharePoint and Power Automate?"
- "Find beginner sessions about Microsoft Graph"

The AI will use the `search_sessions` tool automatically.

### Can I search past conferences?

Yes! Add `include_past=true` to your search, or ask explicitly:
- "Show me sessions from past conferences about Copilot"

By default, only upcoming conferences are searched.

### How do I get session details?

Ask for a specific session:
- "Show me details for session 935"
- "Tell me more about the 'Copilot and Agents' session"

The AI will use the `get_session` tool.

### Can I add sessions to my calendar?

Yes! Use the `send_calendar_invite` tool:
- "Send me a calendar invite for session 935 to fabian@example.com"
- "Add this session to my calendar at work@company.com"

You'll receive an `.ics` file via email that works with all calendar apps (Gmail, Outlook, Apple Calendar, etc.).

### The calendar invite didn't arrive?

1. Check your spam/junk folder
2. Make sure you typed your email correctly
3. Add `conferencehaven.com` to your safe senders list
4. Try again - sometimes email delivery takes a few minutes

### Can I search multiple conferences at once?

Yes! Just don't specify a conference:
- "Find sessions about AI across all conferences"

Or specify multiple conferences in your query:
- "Show me Copilot sessions at ESPC25 and Microsoft Ignite"

---

## Technical Questions

### What's MCP?

**Model Context Protocol (MCP)** is a standard way for AI assistants to connect to external tools and data sources. Think of it like USB for AI - one protocol that works everywhere.

Developed by Anthropic, it's now supported by:
- Claude (Anthropic)
- ChatGPT (OpenAI)
- GitHub Copilot (Microsoft)
- LM Studio (local models)
- Many more...

### How does ConferenceHaven work?

1. You ask your AI assistant a question about conferences
2. The AI decides which tool to use (search, get details, send invite)
3. The AI calls ConferenceHaven's MCP server
4. ConferenceHaven queries its database of conference sessions
5. Results are returned to the AI
6. The AI presents the results to you in natural language

### What database does it use?

Azure SQL Database with full-text search capabilities. Session data is indexed for fast natural language searches.

### Is there an API?

Yes! ConferenceHaven provides:
- **MCP protocol** (`https://mcp.conferencehaven.com`) - Preferred for AI clients
- **OpenAPI/Swagger** (`https://mcp.conferencehaven.com/api/swagger.json`) - For ChatGPT/Copilot Studio
- **Direct REST API** - Available for advanced use cases

### Can I run it locally?

The public server is free and fast, but if you want to run your own instance:
- Code is in the private repo (organizer/contributor access only)
- Built with Python FastAPI + Agent Framework
- Requires Azure SQL Database
- Docker deployment supported

### How often is session data updated?

- **Live conferences**: Real-time updates as organizers make changes
- **Upcoming conferences**: Daily sync from conference websites
- **Past conferences**: Archived and available for search

---

## Privacy & Security

### What data do you collect?

We collect:
- **Session data**: Public info from conference websites (titles, speakers, descriptions, times)
- **Search queries**: Logged for analytics and improving search quality
- **Calendar requests**: Email + session ID (not stored after sending)
- **Usage metrics**: Tool calls, client types, error rates (no personal info)

We **DO NOT** collect:
- Full AI conversation history
- Personal information beyond email for calendar invites
- Payment information (Stripe handles all payments)

### Is my email address stored?

No. When you request a calendar invite, we:
1. Generate the `.ics` file
2. Send it to your email
3. Delete the email address immediately

We don't have a database of user emails.

### Can organizers see my queries?

Organizers can see:
- **Aggregate analytics**: "50 people searched for 'Copilot' this week"
- **Popular sessions**: "Session 935 was viewed 100 times"

Organizers **cannot see**:
- Individual user queries
- Who requested specific sessions
- Email addresses (except their own test invites)

### Is the connection secure?

- **In production**: Yes, uses HTTPS with TLS encryption (coming soon - currently HTTP)
- **MCP protocol**: Encrypted end-to-end
- **Calendar invites**: Sent via secure email

### GDPR compliance?

Yes:
- No personal data stored long-term
- Email addresses deleted after invite sent
- Users can request data deletion (though we store almost nothing)
- Conference organizers are data controllers for their session data

---

## For Conference Organizers

### How do I add my conference?

[Submit a conference request](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=conference_request.md) with:
- Conference name & dates
- Location (city, venue)
- Session data source (website URL, API, or manual upload)
- Contact email

We'll reach out to coordinate data integration.

### What formats do you accept for session data?

- **CSV**: Preferred for bulk uploads
- **JSON**: For API integrations
- **Manual entry**: Web interface coming soon
- **Sched.com**: Direct integration available
- **Sessionize**: Direct integration available

### What analytics do you provide?

Free tier:
- Total session views
- Top searched sessions
- Search term trends (last 30 days)

Paid tiers:
- Real-time analytics dashboard
- Attendee engagement metrics
- Session recommendations
- Export to Excel/CSV
- Custom reports

### How much does it cost?

**For organizers**:
- **Free tier**: Basic analytics, up to 500 sessions
- **Pro tier**: $99/month - Advanced analytics, unlimited sessions
- **Enterprise**: Custom pricing for multi-conference organizers

**For attendees**: Always free!

### Can I get early access?

Yes! Email conferencehaven@adotob.com with:
- Conference name
- Number of sessions
- Expected attendees
- Special requirements

Early access includes free Pro tier for 3 months.

---

## Troubleshooting

### Tools not showing up in my AI assistant?

See our [Troubleshooting Guide](TROUBLESHOOTING.md) for detailed solutions.

Quick fixes:
1. Restart your AI client completely
2. Verify your config JSON is valid (no trailing commas)
3. Check that `npx` is installed: `npx --version`
4. Test manually: `npx -y @fabianwilliams/mcp-remote https://mcp.conferencehaven.com`

### Search returns no results?

1. Try broader search terms: "Copilot" instead of "Microsoft 365 Copilot with SharePoint and Teams"
2. Check spelling
3. Set `include_past=true` if searching older conferences
4. Verify the conference has sessions loaded: "List all conferences"

### Calendar invite has wrong timezone?

Session times are stored in the conference's local timezone (e.g., Stockholm time for ESPC25). Your calendar app should convert to your local timezone automatically.

If it's showing wrong:
1. Check your calendar app's timezone settings
2. Check your device's timezone settings
3. Report the issue with session ID and your timezone

---

## Contributing

### How can I help?

- **Report bugs**: [Open an issue](https://github.com/fabianwilliams/ConferenceHaven-Community/issues)
- **Suggest features**: [Open a feature request](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=feature_request.md)
- **Improve docs**: Submit PRs to this Community repo
- **Spread the word**: Share with your conference buddies!

### Can I contribute code?

The core codebase is private (for now), but we welcome:
- Documentation improvements
- Issue templates
- Use cases and examples
- Integration guides

For code contributions to the core platform, email conferencehaven@adotob.com.

---

## Still have questions?

- **Open an issue**: [New issue](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new)
- **Start a discussion**: [Discussions](https://github.com/fabianwilliams/ConferenceHaven-Community/discussions)
- **Email**: conferencehaven@adotob.com

---

**Last updated**: November 9, 2025
