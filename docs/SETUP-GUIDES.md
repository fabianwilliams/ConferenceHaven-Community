# ConferenceHaven Setup Guides

Complete setup instructions for using ConferenceHaven with different AI clients.

**Website**: [conferencehaven.com](https://conferencehaven.com)
**MCP Server**: `https://mcp.conferencehaven.com`

---

## 🎯 Choose Your Setup Method

### ⚡ Native Connectors (Zero Installation)

If you have a **paid AI subscription**, you can connect instantly with no installation needed!

| Platform | Plan Required | Setup Method |
|----------|--------------|--------------|
| [Claude Desktop](#-claude-desktop-native-connector) | Pro/Max ($20/mo) | Settings → Connectors |
| [ChatGPT](#-chatgpt-native-connector) | Plus/Pro ($20/mo) | Settings → Add Connector |
| [Copilot Studio](#-copilot-studio) | Microsoft 365 | Browser-based |

⚠️ **Note**: Native connectors are in **beta** - they may have reliability issues. For the most stable experience, use the [Universal Setup](#universal-setup-requires-nodejs) below.

---

### 🌍 Universal Setup (Requires Node.js)

Works with **all AI platforms** and **all plan tiers** (including free accounts).

**One-time setup**: Install Node.js (~5 minutes)
- **What is Node.js?** A free tool that lets AI clients connect to ConferenceHaven. Think of it like installing a USB driver - you do it once, and it just works.
- **Is it safe?** Absolutely! Over 20 million developers use it worldwide. Download from [nodejs.org](https://nodejs.org)
- **Already have it?** Check by running: `node --version`

**Supported clients**:
- [Claude Desktop](#-claude-desktop) (all plans)
- [ChatGPT](#-chatgpt-via-gpt-builder) (all plans)
- [LM Studio](#-lm-studio-local-ai) (free, local AI)
- [GitHub Copilot](#-github-copilot-vs-code) (paid subscription)

---

## ⚡ Native Connector Setup (Paid Plans)

### 🧠 Claude Desktop (Native Connector)

**Requirements**: Claude Pro or Max subscription ($20/month)
**Status**: Beta (may be unreliable)

#### Setup Steps:

1. **Open Claude Desktop**

2. **Go to Settings**:
   - Click your profile icon (bottom left)
   - Select "Settings"

3. **Add Connector**:
   - Go to "Connectors" tab
   - Click "Add custom connector"
   - Enter:
     - **Name**: ConferenceHaven
     - **Remote MCP server URL**: `https://mcp.conferencehaven.com/`

4. **Done!**
   - The connector will appear in your list
   - Start chatting: "What conferences are available?"

#### Troubleshooting:
- **Not seeing Connectors tab?** Make sure you have Claude Pro or Max
- **Connection fails?** Native connectors are in beta - try the [Universal Setup](#-claude-desktop) instead

---

### 💬 ChatGPT (Native Connector)

**Requirements**: ChatGPT Plus or Pro subscription ($20/month)
**Status**: Beta (may be unreliable)

#### Setup Steps:

1. **Open ChatGPT** ([chat.openai.com](https://chat.openai.com))

2. **Go to Settings**:
   - Click your profile icon
   - Select "Settings"

3. **Add Connector**:
   - Go to "Beta Features" or "Connectors"
   - Click "New Connector"
   - Fill in:
     - **Name**: ConferenceHaven
     - **MCP Server URL**: `https://mcp.conferencehaven.com/`
     - **Authentication**: None

4. **Done!**
   - Ask: "Find sessions about AI at Microsoft Ignite"

#### Troubleshooting:
- **Can't find Connectors?** This feature may not be available in all regions yet
- **Connection issues?** Use the [Universal Setup](#-chatgpt-via-gpt-builder) instead

---

## 🌍 Universal Setup (Works Everywhere)

### 📋 Install Node.js (First Time Only)

If you don't have Node.js installed yet, here's how:

#### Windows:
1. Go to [nodejs.org](https://nodejs.org)
2. Download the "LTS" version (the green button)
3. Run the installer (node-vXX.X.X-x64.msi)
4. Click "Next" through the wizard (accept all defaults)
5. Restart your terminal/PowerShell

#### macOS:
1. Go to [nodejs.org](https://nodejs.org)
2. Download the "LTS" version
3. Run the .pkg installer
4. Follow the installation wizard
5. Restart your terminal

#### Check Installation:
Open a new terminal and run:
```bash
node --version
npm --version
```
You should see version numbers like `v20.x.x` and `10.x.x` ✅

---

## 🧠 Claude Desktop

Claude Desktop has native MCP support built-in.

### Setup Steps:

1. **Locate config file**:
   - **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
   - **Linux**: `~/.config/Claude/claude_desktop_config.json`

2. **Edit config file** (create if it doesn't exist):
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

3. **Restart Claude Desktop**

4. **Test it**:
   - Open Claude Desktop
   - Look for the 🔌 icon in the bottom left (indicates MCP tools loaded)
   - Try: "What conferences are available?"

### Troubleshooting:
- **Tools not appearing?** Check that your JSON is valid (no trailing commas)
- **Connection errors?** Verify `npx` is installed: `npx --version`
- **Still not working?** Restart Claude Desktop completely

---

## 🤖 ChatGPT (via GPT Builder)

ChatGPT can connect to ConferenceHaven using GPT Actions.

### Setup Steps:

1. **Go to GPT Builder**: [https://chat.openai.com/gpts/editor](https://chat.openai.com/gpts/editor)

2. **Create a new GPT**:
   - Name: "Conference Navigator"
   - Description: "Search conference sessions and add them to your calendar"

3. **Add Actions**:
   - Click "Configure"
   - Click "Add Actions"
   - Import OpenAPI schema from: `https://mcp.conferencehaven.com/api/swagger.json`

4. **Configure Authentication**:
   - Select "None" (public API)

5. **Test it**:
   - Try: "Find sessions about Copilot at Microsoft Ignite"

### Note:
ChatGPT requires OpenAPI/Swagger format. ConferenceHaven provides this automatically.

---

## 💼 Copilot Studio

For enterprise Microsoft 365 environments using Copilot Studio.

### Setup Steps:

1. **Open Copilot Studio**: [https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com)

2. **Create a new Agent**:
   - Go to "Agents" → "New agent"
   - Name: "Conference Assistant"

3. **Add Action**:
   - Click "Actions" → "Add action"
   - Select "Import from URL"
   - URL: `https://mcp.conferencehaven.com/api/swagger.json`

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
        "command": "npx",
        "args": [
          "-y",
          "mcp-remote",
          "https://mcp.conferencehaven.com/"
        ]
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
curl https://mcp.conferencehaven.com/health
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
3. Check `npx` is installed: `npx --version`
4. Try manually: `npx -y mcp-remote https://mcp.conferencehaven.com/`

### Connection errors?
1. Check internet connection
2. Verify URL: `https://mcp.conferencehaven.com` (no trailing slash)
3. Test health endpoint: `curl https://mcp.conferencehaven.com/health`

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
