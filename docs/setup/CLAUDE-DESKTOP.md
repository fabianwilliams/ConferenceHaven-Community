# Setup ConferenceHaven for Claude Desktop

**Platform:** Desktop app (Windows, macOS, Linux)  
**Requirements:** Node.js (see [Node.js setup guide](NODEJS.md))  
**Setup time:** ~5 minutes

Get instant access to conference sessions, speakers, and schedules directly in Claude Desktop through the Model Context Protocol (MCP).

---

## Prerequisites

### 1. Install Node.js

Claude Desktop uses Node.js to run MCP servers. If you don't have it yet:

👉 **[Follow our Node.js installation guide](NODEJS.md)** (~5 minutes)

Verify Node.js is installed:
```bash
node --version
# Should show v18.0.0 or higher
```

### 2. Download Claude Desktop

If you don't have Claude Desktop yet:
- **Windows/macOS:** [claude.ai/download](https://claude.ai/download)
- **Linux:** Available via Snap Store or AppImage

---

## Installation Steps

### Step 1: Install ConferenceHaven MCP Server

Open your **terminal** (macOS/Linux) or **Command Prompt** (Windows) and run:

```bash
npx -y @fabianwilliams/conferencehaven-mcp
```

**What this does:**
- Downloads the ConferenceHaven MCP server
- Sets it up in your system
- Takes ~30 seconds

**Expected output:**
```
✓ @fabianwilliams/conferencehaven-mcp installed successfully
```

### Step 2: Configure Claude Desktop

Claude Desktop needs to know about the MCP server. You'll edit a JSON config file.

#### Find Your Config File

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
    "conferencehaven": {
      "command": "npx",
      "args": [
        "-y",
        "@fabianwilliams/conferencehaven-mcp"
      ]
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
    "conferencehaven": {
      "command": "npx",
      "args": [
        "-y",
        "@fabianwilliams/conferencehaven-mcp"
      ]
    }
  }
}
```

**Save the file** and close your text editor.

### Step 3: Restart Claude Desktop

1. **Quit Claude Desktop completely** (not just close the window)
   - **Windows:** Right-click Claude in system tray → Exit
   - **macOS:** Cmd+Q or Claude menu → Quit
   - **Linux:** Close all windows and check system tray

2. **Relaunch Claude Desktop**

3. **Look for the 🔨 tools icon** in the bottom-right of the chat input

---

## Using ConferenceHaven

Once configured, you can ask Claude questions like:

```
"Find sessions about Azure at Microsoft Ignite 2024"
"Show me Donovan Brown's speaking schedule"
"What AI sessions are happening at ESPC 2025?"
"Send a calendar invite for the keynote session"
```

Claude will automatically use ConferenceHaven tools to search conferences, sessions, and speakers, then send calendar invites via Outlook.

### Available Tools

- **🔍 Search Conferences** - Find events by name, date, or organizer
- **📅 Search Sessions** - Find sessions by topic, speaker, or track
- **🎤 Search Speakers** - Find speakers and their sessions
- **📧 Send Calendar Invite** - Add sessions to your Outlook calendar

---

## Troubleshooting

### "Tools not showing up"

**Check 1: Verify config file syntax**
- Use [jsonlint.com](https://jsonlint.com/) to validate your JSON
- Common issues: missing commas, extra commas, mismatched brackets

**Check 2: Verify Node.js is installed**
```bash
node --version
# Should show v18.0.0 or higher
```

**Check 3: Check Claude Desktop logs**
- **Windows:** `%APPDATA%\Claude\logs\`
- **macOS:** `~/Library/Logs/Claude/`
- **Linux:** `~/.config/Claude/logs/`

Look for errors mentioning "conferencehaven" or "MCP"

### "npx command not found"

Node.js installation didn't complete properly. Reinstall Node.js:
- 👉 **[Node.js installation guide](NODEJS.md)**

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

---

## Updating ConferenceHaven

ConferenceHaven updates automatically when you restart Claude Desktop. The `npx -y` command always fetches the latest version.

**To force an update:**
```bash
npx clear-npx-cache
# Then restart Claude Desktop
```

---

## Uninstalling

### Remove from Claude Desktop

1. Open `claude_desktop_config.json` (see Step 2 above)
2. Delete the `"conferencehaven"` section
3. Save and restart Claude Desktop

### Remove the Package

```bash
npm uninstall -g @fabianwilliams/conferencehaven-mcp
```

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
- [ChatGPT Setup Guide](CHATGPT.md)
- [LM Studio Setup Guide](LM-STUDIO.md)
