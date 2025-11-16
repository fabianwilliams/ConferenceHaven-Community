# Setup ConferenceHaven for ChatGPT Desktop

**Platform:** ChatGPT Desktop app (Windows, macOS)
**Requirements:** ChatGPT Plus subscription
**Setup time:** ~2 minutes

Get instant access to conference sessions, speakers, and schedules directly in ChatGPT Desktop through the Model Context Protocol (MCP).

---

## Prerequisites

### 1. ChatGPT Plus Subscription

MCP support in ChatGPT requires a **ChatGPT Plus** subscription ($20/month).

- **Sign up:** [chatgpt.com/plus](https://chatgpt.com/plus)

### 2. Download ChatGPT Desktop

If you don't have ChatGPT Desktop yet:
- **Windows/macOS:** [chatgpt.com/download](https://chatgpt.com/download)

---

## Installation Steps

### Step 1: Configure ChatGPT Desktop

ChatGPT Desktop needs to know about the MCP server. You'll edit a JSON config file.

#### Find Your Config File

**Windows:**
```
%APPDATA%\OpenAI\ChatGPT\config.json
```
Quick access: Press `Win+R`, paste the path above, press Enter

**macOS:**
```
~/Library/Application Support/OpenAI/ChatGPT/config.json
```
Quick access: In Finder, press `Cmd+Shift+G`, paste the path above, press Enter

**Note:** If the folder doesn't exist yet, run ChatGPT Desktop at least once to create it.

#### Add ConferenceHaven via UI (Recommended)

1. **Open ChatGPT Settings** → **Apps & Connectors**
2. **Click "Add custom connector"**
3. **Enter the following:**
   - **Name:** `Conference Haven`
   - **URL:** `https://mcp.conferencehaven.com/api/mcp`
4. **Click "Add"**
5. **Restart ChatGPT Desktop**

#### OR: Edit Config File Manually

Open `config.json` in any text editor (Notepad, TextEdit, VS Code, etc.).

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
      "url": "..."
    },
    "conference-haven": {
      "url": "https://mcp.conferencehaven.com/api/mcp"
    }
  }
}
```

**Save the file** and close your text editor.

### Step 2: Restart ChatGPT Desktop

1. **Quit ChatGPT Desktop completely** (not just close the window)
   - **Windows:** Right-click ChatGPT in system tray → Exit
   - **macOS:** Cmd+Q or ChatGPT menu → Quit

2. **Relaunch ChatGPT Desktop**

3. **Look for the 🔧 tools indicator** in the chat interface

---

## Using ConferenceHaven

Once configured, you can ask ChatGPT questions like:

```
"Find sessions about Azure at Microsoft Ignite 2024"
"Show me Donovan Brown's speaking schedule"
"What AI sessions are happening at ESPC 2025?"
"Send a calendar invite for the keynote session"
```

ChatGPT will automatically use ConferenceHaven tools to search conferences, sessions, and speakers, then send calendar invites via Outlook.

### Available Tools

- **🔍 Search Conferences** - Find events by name, date, or organizer
- **📅 Search Sessions** - Find sessions by topic, speaker, or track
- **🎤 Search Speakers** - Find speakers and their sessions
- **📧 Send Calendar Invite** - Add sessions to your Outlook calendar

---

## Troubleshooting

### "Tools not showing up"

**Check 1: Verify ChatGPT Plus subscription**
- MCP requires ChatGPT Plus ($20/month)
- Verify at [chatgpt.com/settings](https://chatgpt.com/settings)

**Check 2: Verify config file syntax (if using manual config)**
- Use [jsonlint.com](https://jsonlint.com/) to validate your JSON
- Common issues: missing commas, extra commas, mismatched brackets
- Make sure you're using the `url` format, not `command` format

**Check 3: Test the server directly**
```bash
curl -s https://mcp.conferencehaven.com/api/mcp
# Should return MCP server information
```

**Check 4: Check ChatGPT Desktop logs**
- **Windows:** `%APPDATA%\OpenAI\ChatGPT\logs\`
- **macOS:** `~/Library/Logs/OpenAI/ChatGPT/`

Look for errors mentioning "conference-haven" or "MCP"

### Tools Show But Searches Return No Results

ConferenceHaven is working! The database might not have the conference you're searching for yet.

**Try these test searches:**
- `"Find sessions at ESPC 2025"`
- `"Show me Microsoft Ignite 2024 sessions"`

**Want to add a conference?** [Request it here](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=conference-request.md)

### Calendar Invites Not Sending

Calendar invites require:
1. **Outlook account** (Microsoft 365, Outlook.com, or Exchange)
2. **Microsoft Graph permissions** (organizers only)

If you're an attendee, calendar invites won't work yet—this is an organizer-only feature for now.

### "MCP not supported" Error

Make sure you have:
1. **ChatGPT Plus subscription** (required for MCP)
2. **Latest ChatGPT Desktop version** - Update via app settings

---

## Updating ConferenceHaven

ConferenceHaven server updates automatically on Azure. No client-side updates needed!

Your ChatGPT Desktop connects directly to the server via HTTPS, so you always get the latest version automatically.

---

## Uninstalling

### Remove from ChatGPT Desktop

1. Open `config.json` (see Step 2 above)
2. Delete the `"conferencehaven"` section
3. Save and restart ChatGPT Desktop

### Remove the Package

ConferenceHaven runs on a remote server, so there's nothing to uninstall locally. Just removing it from the config file is enough!

---

## Next Steps

- **[Architecture Overview](../ARCHITECTURE.md)** - How ConferenceHaven works
- **[FAQ](../FAQ.md)** - Common questions
- **[Troubleshooting](../TROUBLESHOOTING.md)** - Detailed debugging

---

## Need Help?

- **GitHub Issues:** [ConferenceHaven-Community/issues](https://github.com/fabianwilliams/ConferenceHaven-Community/issues)
- **Documentation:** [Full docs](../../README.md)

---

**Quick Links:**
- [← Back to Setup Guides](../SETUP-GUIDES.md)
- [Node.js Setup Guide](NODEJS.md)
- [Claude Desktop Setup Guide](CLAUDE-DESKTOP.md)
- [LM Studio Setup Guide](LM-STUDIO.md)
