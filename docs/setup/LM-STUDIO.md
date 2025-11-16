# Setup ConferenceHaven for LM Studio

**Platform:** LM Studio (Windows, macOS, Linux)
**Requirements:** LM Studio v0.3.0+ (with MCP support)
**Setup time:** ~2 minutes

Get instant access to conference sessions, speakers, and schedules in LM Studio through the Model Context Protocol (MCP).

---

## Prerequisites

### Download LM Studio

If you don't have LM Studio yet:
- **All platforms:** [lmstudio.ai/download](https://lmstudio.ai/download)

---

## Installation Steps

### Step 1: Configure LM Studio

LM Studio needs to know about the MCP server. You'll edit a JSON config file.

#### Find Your Config File

**Windows:**
```
%USERPROFILE%\.lmstudio\mcp_config.json
```
Quick access: Press `Win+R`, paste `%USERPROFILE%\.lmstudio`, press Enter

**macOS:**
```
~/.lmstudio/mcp_config.json
```
Quick access: In Finder, press `Cmd+Shift+G`, paste `~/.lmstudio`, press Enter

**Linux:**
```
~/.lmstudio/mcp_config.json
```

**Note:** If the folder doesn't exist yet, run LM Studio at least once to create it.

#### Edit the Config File

Open `mcp_config.json` in any text editor (Notepad, TextEdit, VS Code, nano, etc.).

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

### Step 2: Restart LM Studio

1. **Quit LM Studio completely** (close all windows)
   - **Windows:** Right-click LM Studio in system tray → Exit
   - **macOS:** Cmd+Q or LM Studio menu → Quit
   - **Linux:** Close all windows

2. **Relaunch LM Studio**

3. **Enable MCP in Settings**
   - Open LM Studio Settings
   - Navigate to **Extensions** or **MCP** section
   - Toggle **Enable MCP** to ON

4. **Look for ConferenceHaven** in the available tools/extensions list

---

## Using ConferenceHaven

Once configured, you can ask your local LLM questions like:

```
"Find sessions about Azure at Microsoft Ignite 2024"
"Show me Donovan Brown's speaking schedule"
"What AI sessions are happening at ESPC 2025?"
"Send a calendar invite for the keynote session"
```

Your local model will automatically use ConferenceHaven tools to search conferences, sessions, and speakers, then send calendar invites via Outlook.

### Available Tools

- **🔍 Search Conferences** - Find events by name, date, or organizer
- **📅 Search Sessions** - Find sessions by topic, speaker, or track
- **🎤 Search Speakers** - Find speakers and their sessions
- **📧 Send Calendar Invite** - Add sessions to your Outlook calendar

---

## Troubleshooting

### "MCP not enabled" or "Tools not showing up"

**Check 1: Enable MCP in LM Studio**
- Open LM Studio Settings → Extensions
- Toggle **Enable MCP** to ON
- Restart LM Studio

**Check 2: Verify config file syntax**
- Use [jsonlint.com](https://jsonlint.com/) to validate your JSON
- Common issues: missing commas, extra commas, mismatched brackets
- Make sure you're using the `url` format, not `command` format

**Check 3: Test the server directly**
```bash
curl -s https://mcp.conferencehaven.com/api/mcp
# Should return MCP server information
```

**Check 4: Check LM Studio logs**
- **Windows:** `%USERPROFILE%\.lmstudio\logs\`
- **macOS:** `~/.lmstudio/logs/`
- **Linux:** `~/.lmstudio/logs/`

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

### Model Not Using Tools

Some local models have poor tool/function calling support. Try these recommended models:

**Best for tool calling:**
- **Llama 3.1 (8B or 70B)** - Excellent tool support
- **Mistral 7B Instruct** - Good tool support
- **Qwen 2.5 (7B or 14B)** - Strong tool support

**Enable tool calling:**
- In LM Studio chat, check that "Enable function calling" is ON
- Some models need explicit prompting: "Use available tools to answer"

---

## Updating ConferenceHaven

ConferenceHaven server updates automatically on Azure. No client-side updates needed!

Your LM Studio connects directly to the server via HTTPS, so you always get the latest version automatically.

---

## Uninstalling

### Remove from LM Studio

1. Open `mcp_config.json` (see Step 2 above)
2. Delete the `"conferencehaven"` section
3. Save and restart LM Studio

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
- [ChatGPT Setup Guide](CHATGPT.md)
