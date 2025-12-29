# Setup ConferenceHaven for Claude Desktop

**Platform:** Claude Desktop app (Windows, macOS, Linux)
**Requirements:** None (just Claude Desktop)
**Setup time:** ~2 minutes
**Authentication:** Optional OAuth (public tools work without authentication)

Get instant access to conference sessions, speakers, and schedules directly in Claude Desktop through the Model Context Protocol (MCP).

---

## Prerequisites

### Download Claude Desktop

If you don't have Claude Desktop yet:
- **Windows/macOS:** [claude.ai/download](https://claude.ai/download)
- **Linux:** Available via Snap Store or AppImage

---

## Installation Steps

### Step 1: Configure Claude Desktop

Claude Desktop needs to know about the MCP server. You'll edit a JSON config file.

####Find Your Config File

**Windows:**
```
%APPDATA%\Claude\claude_desktop_config.json
```
Quick access: Press `Win+R`, paste the path above, press Enter

**macOS:**
```
~/Library/Application Support/Claude/claude_desktop_config.json
```
Quick access: In Finder, press `Cmd+Shift+G`, paste the path above, press Enter

**Linux:**
```
~/.config/Claude/claude_desktop_config.json
```

#### Edit the Config File

Open `claude_desktop_config.json` in any text editor (Notepad, TextEdit, VS Code, etc.).

**If the file is empty or only has `{}`**, paste this:

```json
{
  "mcpServers": {
    "conference-haven": {
      "url": "https://mcp.conferencehaven.com/api/mcp"
    }
  }
}
```

**If the file already has other MCP servers**, add ConferenceHaven to the `mcpServers` section:

```json
{
  "mcpServers": {
    "existing-server": {
      "command": "...",
      "args": ["..."]
    },
    "conference-haven": {
      "url": "https://mcp.conferencehaven.com/api/mcp"
    }
  }
}
```

**Save the file** and close your text editor.

### Step 2: Restart Claude Desktop

1. **Quit Claude Desktop completely** (not just close the window)
   - **Windows:** Right-click Claude in system tray → Exit
   - **macOS:** Cmd+Q or Claude menu → Quit
   - **Linux:** Close all windows and check system tray

2. **Relaunch Claude Desktop**

3. **Look for the 🔨 tools icon** in the bottom-right of the chat input

> **📸 Screenshot Placeholder:** Claude Desktop with tools icon showing ConferenceHaven is connected

---

## Using ConferenceHaven

Once configured, you can ask Claude questions like:

```
"Find sessions about Azure at Microsoft Ignite 2024"
"Show me Donovan Brown's speaking schedule"
"What AI sessions are happening at ESPC 2025?"
"Send a calendar invite for session 123 to myemail@example.com"
```

Claude will automatically use ConferenceHaven tools to search conferences, sessions, and speakers, then send calendar invites.

### Available Tools

#### 🔓 Public Tools (Always Work - No Authentication Required)

- **🔍 Search Conferences** - Find events by name, date, or organizer
- **📅 Search Sessions** - Find sessions by topic, speaker, or track
- **🎯 List Conferences** - Browse all available conferences
- **👤 Get Session Details** - View full session information
- **📧 Send Calendar Invite** - Add sessions to your calendar

**Note:** Calendar invites require you to provide your email address when using Claude Desktop without OAuth.

#### 🔐 Protected Tools (Not Available in Claude Desktop)

- **📊 Get Organizer Analytics** - ❌ Requires OAuth (not supported in Claude Desktop yet)
- **📈 Get Conference Analytics** - ❌ Requires OAuth (not supported in Claude Desktop yet)

**Why not available?**
- Claude Desktop doesn't have native OAuth support yet
- Analytics tools now require OAuth authentication for security
- We're working with Anthropic to add OAuth support to Claude Desktop!

**Workaround for Organizers:**
If you're an organizer and need analytics, use [ChatGPT Desktop](CHATGPT.md) which has native OAuth support.

---

## ⚠️ What Changed? OAuth Migration

### Before (API Keys)
- ✅ Analytics worked with X-Organizer-Key header
- ❌ Less secure (API keys can be leaked)
- ❌ No user tracking

### After (OAuth-Only)
- ✅ More secure (OAuth 2.1 with PKCE)
- ✅ User tracking for better features
- ✅ Auto-filled email for calendar invites
- ❌ Analytics require OAuth (not available in Claude Desktop yet)

### What Still Works in Claude Desktop
- ✅ **ALL public tools** (search, view sessions, send calendar invites)
- ✅ **Same setup process** (just the URL, no OAuth configuration needed)
- ✅ **Same user experience** for searching and browsing conferences

### What Doesn't Work (Organizers Only)
- ❌ Analytics tools (requires OAuth)
- ⚠️ Use ChatGPT Desktop for analytics until Claude Desktop adds OAuth support

---

## Troubleshooting

### "Tools not showing up"

**Check 1: Verify config file syntax**
- Use [jsonlint.com](https://jsonlint.com/) to validate your JSON
- Common issues: missing commas, extra commas, mismatched brackets
- Make sure you're using the `url` format, not `command` format

**Check 2: Test the server directly**
```bash
curl -s https://mcp.conferencehaven.com/.well-known/oauth-protected-resource

# Should return OAuth configuration JSON
```

**Check 3: Check Claude Desktop logs**
- **Windows:** `%APPDATA%\Claude\logs\`
- **macOS:** `~/Library/Logs/Claude/`
- **Linux:** `~/.config/Claude/logs/`

Look for errors mentioning "conference-haven" or "MCP"

### Tools Show But Searches Return No Results

ConferenceHaven is working! The database might not have the conference you're searching for yet.

**Try these test searches:**
- `"Find sessions at ESPC 2025"`
- `"Show me Microsoft Ignite 2024 sessions"`
- `"Find AI sessions at European Collaboration Summit 2026"`

**Want to add a conference?** [Request it here](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=conference-request.md)

### Calendar Invites Asking for Email Every Time

**This is expected behavior in Claude Desktop!**

- Claude Desktop doesn't support OAuth yet
- You'll need to provide your email address each time
- The email is NOT saved for privacy reasons

**Want auto-filled email?** Use [ChatGPT Desktop](CHATGPT.md) which supports OAuth.

### "Analytics tools not working"

**Error:** "Authentication required. Please authenticate via OAuth to access analytics."

**This is expected!** Analytics tools now require OAuth, which Claude Desktop doesn't support yet.

**Solutions:**
1. **Use ChatGPT Desktop** - Has native OAuth support ([setup guide](CHATGPT.md))
2. **Wait for Claude Desktop OAuth support** - We're working with Anthropic on this!
3. **Use public tools** - Search, browse, and calendar invites still work perfectly!

**Why OAuth-only for analytics?**
- Enhanced security (no more API keys)
- User privacy and tracking
- Better feature development insights

---

## Updating ConferenceHaven

ConferenceHaven server updates automatically on Azure. No client-side updates needed!

Your Claude Desktop connects directly to the server via HTTPS, so you always get the latest version automatically.

**Recent Updates:**
- ✅ OAuth authentication for enhanced security
- ✅ User tracking for better recommendations (when using OAuth-capable clients)
- ✅ Auto-filled email for calendar invites (when using OAuth-capable clients)
- ℹ️ Public tools still work great in Claude Desktop!

---

## Uninstalling

### Remove from Claude Desktop

1. Open `claude_desktop_config.json` (see Step 1 above)
2. Delete the `"conference-haven"` section
3. Save and restart Claude Desktop

### Remove the Package

ConferenceHaven runs on a remote server, so there's nothing to uninstall locally. Just removing it from the config file is enough!

---

## Next Steps

- **[ChatGPT Setup](CHATGPT.md)** - Recommended for organizers (has OAuth support)
- **[OAuth Documentation](OAUTH.md)** - Technical details about OAuth authentication
- **[Architecture Overview](../ARCHITECTURE.md)** - How ConferenceHaven works
- **[FAQ](../FAQ.md)** - Common questions
- **[Troubleshooting](../TROUBLESHOOTING.md)** - Detailed debugging

---

## Need Help?

- **GitHub Issues:** [ConferenceHaven-Community/issues](https://github.com/fabianwilliams/ConferenceHaven-Community/issues)
- **Documentation:** [Full docs](../../README.md)
- **Email Support:** support@conferencehaven.com

---

**Quick Links:**
- [← Back to Setup Guides](../SETUP-GUIDES.md)
- [ChatGPT Setup Guide](CHATGPT.md) (Recommended for organizers)
- [LM Studio Setup Guide](LM-STUDIO.md)
- [OAuth Documentation](OAUTH.md)
