# Setup ConferenceHaven for ChatGPT Desktop

**Platform:** ChatGPT Desktop app (Windows, macOS)
**Requirements:** ChatGPT Plus subscription
**Setup time:** ~2 minutes
**Authentication:** Automatic OAuth (for analytics)

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
   - **Description:** `Gives me conference information`
   - **MCP Server URL:** `https://mcp.conferencehaven.com/api/mcp`
   - **Authentication:** `OAuth`
   - **OAuth Client ID:** `eysrv7VweawkpzKzNWngCm7ORQCzNsiW`
   - **OAuth Client Secret:** Leave blank (optional)
4. **Click "Create"**
5. **Check "I understand and want to continue"** (custom MCP server warning)
6. **Restart ChatGPT Desktop**

> **📸 Screenshot Placeholder:** ChatGPT Settings → Apps & Connectors → Add custom connector dialog

**Important:** ChatGPT does **NOT** support Dynamic Client Registration (DCR), so you **must** provide the Client ID above. Without it, OAuth authentication will not work for organizer analytics.

#### OR: Edit Config File Manually

Open `config.json` in any text editor (Notepad, TextEdit, VS Code, etc.).

**If the file is empty or only has `{}`**, paste this:

```json
{
  "mcpServers": {
    "conference-haven": {
      "url": "https://mcp.conferencehaven.com/api/mcp",
      "oauth": {
        "clientId": "eysrv7VweawkpzKzNWngCm7ORQCzNsiW"
      }
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
      "url": "https://mcp.conferencehaven.com/api/mcp",
      "oauth": {
        "clientId": "eysrv7VweawkpzKzNWngCm7ORQCzNsiW"
      }
    }
  }
}
```

**Save the file** and close your text editor.

**Note:** The `oauth.clientId` is **required** for ChatGPT because it doesn't support Dynamic Client Registration. Without this, organizer analytics tools won't work (public tools will still work fine).

### Step 2: Restart ChatGPT Desktop

1. **Quit ChatGPT Desktop completely** (not just close the window)
   - **Windows:** Right-click ChatGPT in system tray → Exit
   - **macOS:** Cmd+Q or ChatGPT menu → Quit

2. **Relaunch ChatGPT Desktop**

3. **Look for the 🔧 tools indicator** in the chat interface

> **📸 Screenshot Placeholder:** ChatGPT with tools indicator showing ConferenceHaven is connected

---

## Using ConferenceHaven

Once configured, you can ask ChatGPT questions like:

```
"Find sessions about Azure at Microsoft Ignite 2024"
"Show me Donovan Brown's speaking schedule"
"What AI sessions are happening at ESPC 2025?"
"Send a calendar invite for the keynote session"
```

ChatGPT will automatically use ConferenceHaven tools to search conferences, sessions, and speakers, then send calendar invites.

### Available Tools

#### 🔓 Public Tools (No Authentication Required)

- **🔍 Search Conferences** - Find events by name, date, or organizer
- **📅 Search Sessions** - Find sessions by topic, speaker, or track
- **👤 Get Session Details** - View full session information
- **📧 Send Calendar Invite** - Add sessions to your calendar (email auto-filled when authenticated!)

#### 🔐 Protected Tools (OAuth Authentication Required - Organizers Only)

- **📊 Get Organizer Analytics** - View session popularity and user engagement
- **📈 Get Conference Analytics** - Detailed analytics for your conferences

---

## 🔐 OAuth Authentication (Automatic)

### What is OAuth?

ConferenceHaven uses OAuth 2.1 with Auth0 for secure authentication. This means:
- ✅ **No passwords stored** - You log in through Auth0
- ✅ **Automatic in ChatGPT** - No manual configuration needed
- ✅ **Only for analytics** - Public tools work without authentication

### When Will I See the OAuth Prompt?

ChatGPT will **automatically prompt you to authenticate** when you:
1. Try to access organizer analytics (e.g., "Show me analytics for my conference")
2. Use a protected tool for the first time

### OAuth Flow (What to Expect)

1. **You ask for analytics**: "Show me analytics for European Collaboration Summit 2026"
2. **ChatGPT detects auth needed**: Shows "Authentication Required" message
3. **OAuth popup opens**: Auth0 login page appears
4. **You log in**: Enter your email and password
5. **Authenticated!**: ChatGPT receives your credentials and shows analytics

> **📸 Screenshot Placeholder:** ChatGPT "Authentication Required" prompt
> **📸 Screenshot Placeholder:** Auth0 login popup
> **📸 Screenshot Placeholder:** Successful authentication + analytics displayed

### Enhanced Features with OAuth

When you authenticate, you get:
- ✅ **Auto-filled email** - Calendar invites use your email automatically (no need to type it!)
- ✅ **Personalized analytics** - See engagement data for your conferences
- ✅ **Better tracking** - We can track feature adoption to improve the service

### What Data is Tracked?

When you authenticate via OAuth, we track:
- Your email address
- Tools you use (search, calendar, analytics)
- Conferences you engage with
- Interaction timestamps

**Purpose:** Improve recommendations, measure feature adoption, understand user needs.

**Opt-Out:** Use tools without authentication (anonymous mode).

**Data Retention:** 90 days of interaction history.

See our [Privacy Policy](../../PRIVACY.md) and [Data Use Policy](../../DATA-USE.md) for full details.

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
# Check server is running
curl -s https://mcp.conferencehaven.com/.well-known/oauth-protected-resource

# Should return OAuth configuration JSON
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
- `"Find AI sessions at European Collaboration Summit 2026"`

**Want to add a conference?** [Request it here](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=conference-request.md)

### OAuth Authentication Failed

**Error:** "Authentication required. Please authenticate via OAuth to access analytics."

**Solutions:**
1. **Click the Auth0 login link** - ChatGPT will show a popup
2. **Enter your email and password** - Use your Auth0 account credentials
3. **Check you're an organizer** - Only organizers in our database can access analytics
   - Not an organizer? Contact your conference administrator

**Still not working?**
- Clear ChatGPT cache: Settings → Advanced → Clear cache
- Restart ChatGPT Desktop
- Check Auth0 is not blocked by firewall/VPN

### Calendar Invites Not Working

**With OAuth (Recommended):**
- ✅ Email is auto-filled from your OAuth profile
- ✅ Calendar invite sent automatically
- ❌ If it fails, check your email address is valid

**Without OAuth (Anonymous):**
- ⚠️ ChatGPT will ask for your email address
- ✅ Calendar invite sent to the email you provide
- ❌ You'll need to provide email every time

**Common Issues:**
- **Invalid email format** - Make sure it's a valid email address
- **Email not received** - Check your spam folder
- **Wrong session** - Verify the session ID is correct

### "You don't have access to conference analytics"

**Error:** "Organizer access required. You are authenticated but not registered as an organizer."

**What this means:**
- ✅ OAuth authentication worked!
- ❌ Your account is not in the OrganizerUsers database
- 📧 Contact your conference administrator to request organizer access

**Who is an organizer?**
- Conference managers who can view analytics
- Event staff with admin privileges
- Registered organizers in the ConferenceHaven system

**Not an organizer?** You can still use all public tools (search, calendar invites) without any restrictions!

---

## Updating ConferenceHaven

ConferenceHaven server updates automatically on Azure. No client-side updates needed!

Your ChatGPT Desktop connects directly to the server via HTTPS, so you always get the latest version automatically.

**Recent Updates:**
- ✅ OAuth authentication for enhanced security
- ✅ User tracking for better recommendations
- ✅ Auto-filled email for calendar invites

---

## Uninstalling

### Remove from ChatGPT Desktop

1. Open `config.json` (see Step 1 above)
2. Delete the `"conference-haven"` section
3. Save and restart ChatGPT Desktop

### Revoke OAuth Access

If you want to revoke ConferenceHaven's access to your Auth0 account:

1. Go to [Auth0 Account Settings](https://manage.auth0.com/dashboard)
2. Navigate to **Applications** → **Authorized Applications**
3. Find "ConferenceHaven MCP OAuth"
4. Click **Revoke**

---

## Next Steps

- **[OAuth Deep Dive](OAUTH.md)** - Technical details about OAuth authentication
- **[Architecture Overview](../ARCHITECTURE.md)** - How ConferenceHaven works
- **[FAQ](../FAQ.md)** - Common questions
- **[Privacy Policy](../../PRIVACY.md)** - Data collection and usage
- **[Troubleshooting](../TROUBLESHOOTING.md)** - Detailed debugging

---

## Need Help?

- **GitHub Issues:** [ConferenceHaven-Community/issues](https://github.com/fabianwilliams/ConferenceHaven-Community/issues)
- **Documentation:** [Full docs](../../README.md)
- **Email Support:** support@conferencehaven.com

---

**Quick Links:**
- [← Back to Setup Guides](../SETUP-GUIDES.md)
- [Claude Desktop Setup Guide](CLAUDE-DESKTOP.md)
- [LM Studio Setup Guide](LM-STUDIO.md)
- [OAuth Documentation](OAUTH.md)
