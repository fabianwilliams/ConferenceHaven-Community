# Setup ConferenceHaven for LM Studio

**Platform:** LM Studio (Windows, macOS, Linux)  
**Requirements:** Node.js (see [Node.js setup guide](NODEJS.md)), LM Studio app  
**Setup time:** ~5 minutes

Get instant access to conference sessions, speakers, and schedules in LM Studio through the Model Context Protocol (MCP).

---

## Prerequisites

### 1. Install Node.js

LM Studio uses Node.js to run MCP servers. If you don't have it yet:

👉 **[Follow our Node.js installation guide](NODEJS.md)** (~5 minutes)

Verify Node.js is installed:
```bash
node --version
# Should show v18.0.0 or higher
```

### 2. Download LM Studio

If you don't have LM Studio yet:
- **All platforms:** [lmstudio.ai/download](https://lmstudio.ai/download)

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

### Step 2: Configure LM Studio

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

### Step 3: Restart LM Studio

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

**Check 3: Verify Node.js is installed**
```bash
node --version
# Should show v18.0.0 or higher
```

**Check 4: Check LM Studio logs**
- **Windows:** `%USERPROFILE%\.lmstudio\logs\`
- **macOS:** `~/.lmstudio/logs/`
- **Linux:** `~/.lmstudio/logs/`

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

ConferenceHaven updates automatically when you restart LM Studio. The `npx -y` command always fetches the latest version.

**To force an update:**
```bash
npx clear-npx-cache
# Then restart LM Studio
```

---

## Uninstalling

### Remove from LM Studio

1. Open `mcp_config.json` (see Step 2 above)
2. Delete the `"conferencehaven"` section
3. Save and restart LM Studio

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
- **LM Studio Discord:** [lmstudio.ai/discord](https://lmstudio.ai/discord)

---

**Quick Links:**
- [← Back to Setup Guides](../SETUP-GUIDES.md)
- [Node.js Setup Guide](NODEJS.md)
- [Claude Desktop Setup Guide](CLAUDE-DESKTOP.md)
- [ChatGPT Setup Guide](CHATGPT.md)
