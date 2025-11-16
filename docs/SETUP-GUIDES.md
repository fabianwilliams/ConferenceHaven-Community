# ConferenceHaven Setup Guides

Complete setup instructions for using ConferenceHaven with different AI clients.

**Website**: [conferencehaven.com](https://conferencehaven.com)
**MCP Server**: `https://mcp.conferencehaven.com/api/mcp`

---

## 🎯 Simple URL-Based Setup

ConferenceHaven works with a simple URL - no downloads, no packages, no Node.js required!

**Just add this URL to your AI client:**
```
https://mcp.conferencehaven.com/api/mcp
```

**Supported clients**:
- [Claude Desktop](#-claude-desktop) (all plans)
- [ChatGPT](#-chatgpt) (all plans)
- [LM Studio](#-lm-studio-local-ai) (free, local AI)
- [Copilot Studio](#-copilot-studio) (Microsoft 365)
- [GitHub Copilot](#-github-copilot-vs-code) (paid subscription)

---

## 🧠 Claude Desktop

Claude Desktop has native MCP support built-in.

### Setup Steps:

1. **Open Claude Desktop Settings**
   - Click your profile icon (bottom left)
   - Select "Settings"

2. **Navigate to Developer Settings**
   - Go to "Developer" tab
   - Click "Edit Config"

3. **Add ConferenceHaven**:
```json
{
  "mcpServers": {
    "conferencehaven": {
      "url": "https://mcp.conferencehaven.com/api/mcp"
    }
  }
}
```

4. **Save and Restart Claude Desktop**

5. **Test it**:
   - Open Claude Desktop
   - Look for the 🔌 icon in the bottom left (indicates MCP tools loaded)
   - Try: "What conferences are available?"

### Troubleshooting:
- **Tools not appearing?** Check that your JSON is valid (no trailing commas)
- **Connection errors?** Verify the URL is correct: `https://mcp.conferencehaven.com/api/mcp`
- **Still not working?** Restart Claude Desktop completely

---

## 🤖 ChatGPT

ChatGPT can connect to ConferenceHaven using MCP connectors.

### Setup Steps:

1. **Open ChatGPT Settings**
   - Click your profile icon
   - Select "Settings"

2. **Add MCP Connector**:
   - Go to "Beta Features" or "Connectors" section
   - Click "Add Connector" or "New MCP Server"
   - Enter:
     - **Name**: ConferenceHaven
     - **URL**: `https://mcp.conferencehaven.com/api/mcp`
     - **Authentication**: None

3. **Save and Test**:
   - Start a new chat
   - Try: "Find sessions about Copilot at Microsoft Ignite"

### Troubleshooting:
- **Can't find Connectors?** Make sure you're using the latest version of ChatGPT
- **Connection issues?** Verify the URL is correct

---

## 💼 Copilot Studio

For enterprise Microsoft 365 environments using Copilot Studio.

### Setup Steps:

1. **Open Copilot Studio**: [https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com)

2. **Create a new Agent**:
   - Go to "Agents" → "New agent"
   - Name: "Conference Assistant"

3. **Add MCP Connector**:
   - Click "Actions" → "Add action"
   - Select "MCP Connector"
   - URL: `https://mcp.conferencehaven.com/api/mcp`

4. **Configure**:
   - Authentication: None
   - Review and approve available tools

5. **Test**:
   - In the test panel: "List available conferences"

### Enterprise Benefits:
- Integrated with Microsoft 365
- Governed access
- SharePoint/Teams integration
- Audit logs

---

## 🖥️ LM Studio (Local AI)

Run ConferenceHaven with local AI models for maximum privacy.

### Setup Steps:

1. **Install LM Studio**: [lmstudio.ai](https://lmstudio.ai)

2. **Download a model**:
   - Recommended: Llama 3.2, Mistral, or Qwen
   - Any model with function calling support

3. **Enable MCP** (in LM Studio settings):
   - Go to Settings → MCP Servers
   - Click "Add Server"

4. **Add ConferenceHaven**:
```json
{
  "mcpServers": {
    "conferencehaven": {
      "url": "https://mcp.conferencehaven.com/api/mcp"
    }
  }
}
```

5. **Start chat**:
   - Select your model
   - Try: "What conferences can I search?"

### Privacy Note:
Your queries go to the local model, which then calls ConferenceHaven. The session data is public, but your conversation stays local.

---

## 🔧 GitHub Copilot (VS Code)

Use ConferenceHaven directly in VS Code with GitHub Copilot.

### Setup Steps:

1. **Install GitHub Copilot extension** (if not already installed)

2. **Add to workspace settings** (`.vscode/settings.json`):
```json
{
  "github.copilot.advanced": {
    "mcp.servers": {
      "conferencehaven": {
        "url": "https://mcp.conferencehaven.com/api/mcp"
      }
    }
  }
}
```

3. **Reload VS Code**

4. **Test in Copilot Chat**:
   - Open Copilot Chat panel
   - Try: "@workspace Find AI sessions at ESPC"

---

## 🛠️ Available Tools

Once connected, you have access to these tools:

### 1. **search_sessions**
Search for conference sessions using natural language.

**Examples**:
- "Find sessions about Copilot"
- "Show me AI sessions at Microsoft Ignite"
- "Which sessions cover SharePoint and Teams?"

**Parameters**:
- `query` - Search term (optional)
- `conference` - Conference slug (optional)
- `track` - Track name (optional)
- `level` - Intro/Intermediate/Advanced (optional)
- `limit` - Max results (default 20)
- `include_past` - Include past conferences (default false)

### 2. **get_session**
Get detailed information about a specific session.

**Examples**:
- "Show me details for session 935"
- "Tell me about the Copilot and Agents session"

**Parameters**:
- `session_id` - Session ID (required)

### 3. **list_conferences**
List all available conferences.

**Examples**:
- "What conferences are available?"
- "Show me all upcoming conferences"

**Parameters**:
- `include_past` - Include past conferences (default false)

### 4. **send_calendar_invite**
Send a calendar invite to your email.

**Examples**:
- "Send me a calendar invite for session 935"
- "Add the Copilot session to my calendar at fabian@example.com"

**Parameters**:
- `session_id` - Session ID (required)
- `user_email` - Your email address (required)

**Note**: Works with Gmail, Outlook, Yahoo, iCloud, any email provider!

### 5. **get_organizer_analytics** (Organizer Only)
Get analytics for your conference (requires API key).

**Examples**:
- "Show me analytics for my conference"
- "What are the most popular sessions?"

**Parameters**:
- `api_key` - Your organizer API key (optional if configured in headers)
- `conference_id` - Conference ID (optional)
- `days` - Days to look back (default 30)

---

## 🧪 Testing Your Setup

### Quick Test Commands:

1. **"What conferences are available?"**
   - Should return list of conferences

2. **"Find sessions about Copilot"**
   - Should search and return relevant sessions

3. **"Show me details for session 935"**
   - Should return session details (if session exists)

### Health Check:
```bash
curl https://mcp.conferencehaven.com/api/mcp/health
```

**Expected response**:
```json
{
  "status": "healthy",
  "service": "ConferenceHaven MCP Server",
  "environment": "production"
}
```

---

## 🆘 Troubleshooting

### Tools not appearing?
1. Restart your AI client completely
2. Verify JSON config is valid (no trailing commas)
3. Verify the URL is correct: `https://mcp.conferencehaven.com/api/mcp`
4. Check your internet connection

### Connection errors?
1. Check internet connection
2. Verify URL: `https://mcp.conferencehaven.com/api/mcp`
3. Test health endpoint: `curl https://mcp.conferencehaven.com/api/mcp/health`

### Search returns no results?
1. Try broader search terms
2. Check if conference has sessions loaded
3. Set `include_past=true` to search all conferences

### Calendar invite not arriving?
1. Check spam/junk folder
2. Verify email address is correct
3. Allow emails from conferencehaven.com domain

---

## 📞 Need Help?

- **Issues/Bugs**: [Open an issue](https://github.com/fabianwilliams/ConferenceHaven-Community/issues)
- **Questions**: [Start a discussion](https://github.com/fabianwilliams/ConferenceHaven-Community/discussions)
- **Email**: conferencehaven@adotob.com

---

**Last updated**: November 9, 2025
