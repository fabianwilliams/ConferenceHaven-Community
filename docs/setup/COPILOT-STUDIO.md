# Setup ConferenceHaven for Microsoft Copilot Studio

**Platform:** Web-based (copilot.microsoft.com)  
**Requirements:** Microsoft account, Copilot Studio access  
**Setup time:** ~3 minutes  
**No Node.js required!** ✅

Add ConferenceHaven to your Copilot Studio copilots using a simple Swagger import—no coding or command-line tools needed.

---

## Prerequisites

### 1. Microsoft Account

You need a Microsoft account (work, school, or personal) to use Copilot Studio.

- **Sign up:** [microsoft.com/account](https://microsoft.com/account)

### 2. Copilot Studio Access

Copilot Studio is available with:
- **Microsoft 365 Business** (includes Copilot Studio)
- **Power Platform** trial or license
- **Free trial:** [copilot.microsoft.com](https://copilot.microsoft.com)

---

## Installation Steps

### Step 1: Open Copilot Studio

1. Go to **[copilotstudio.microsoft.com](https://copilotstudio.microsoft.com)**
2. Sign in with your Microsoft account
3. Select or create a **copilot** (or create a new one)

### Step 2: Add ConferenceHaven Action

1. In your copilot, click **Actions** in the left menu
2. Click **+ Add an action**
3. Select **From Swagger** (OpenAPI specification)

### Step 3: Import ConferenceHaven Swagger

In the Swagger URL field, paste this URL:

```
https://conferencehaven.com/swagger.json
```

Click **Copy** button here to copy: <button onclick="navigator.clipboard.writeText('https://conferencehaven.com/swagger.json')">📋 Copy URL</button>

**Then click "Next" or "Import"**

### Step 4: Review Actions

Copilot Studio will show you the available actions:

- ✅ **Search Conferences** - Find conferences by name, date, or organizer
- ✅ **Search Sessions** - Find sessions by topic, speaker, or track
- ✅ **Search Speakers** - Find speakers and their sessions
- ✅ **Send Calendar Invite** - Add sessions to Outlook calendar

**Click "Add" or "Save"** to enable these actions in your copilot.

### Step 5: Test Your Copilot

Click **Test** in the top-right corner and try asking:

```
"Find sessions about Azure at Microsoft Ignite 2024"
"Show me Donovan Brown's speaking schedule"
"What AI sessions are happening at ESPC 2025?"
```

Your copilot will automatically use ConferenceHaven actions to search and respond!

---

## Using ConferenceHaven in Your Copilot

Once configured, users can ask your copilot:

### Conference Search Examples
```
"What conferences are happening in November 2025?"
"Find Microsoft conferences"
"Show me SharePoint conferences"
```

### Session Search Examples
```
"Find sessions about Azure AI"
"Show me keynote sessions at Ignite"
"What sessions cover Power Platform?"
```

### Speaker Search Examples
```
"Find Fabian Williams' speaking schedule"
"Show me all speakers at ESPC 2025"
"Who is speaking about Copilot?"
```

### Calendar Invite Examples
```
"Send me a calendar invite for the keynote"
"Add this session to my calendar"
```

---

## Customizing Your Copilot

### Add Instructions

In Copilot Studio, add instructions to guide how your copilot uses ConferenceHaven:

**Example instructions:**
```
You are a helpful conference assistant. When users ask about tech conferences, 
sessions, or speakers, use ConferenceHaven actions to search and provide results. 
Always include session details like time, track, and room when available.
```

### Add Topics

Create specific topics for common requests:

**Topic: "Find Conference"**
- Trigger phrases: "find conference", "search event", "what conferences"
- Actions: Call `search_conferences` action
- Response: Display conference name, dates, and location

**Topic: "Session Details"**
- Trigger phrases: "session details", "tell me about", "session info"
- Actions: Call `search_sessions` action
- Response: Display session title, speaker, time, track

---

## Troubleshooting

### "Could not import Swagger"

**Check 1: Verify URL**
Make sure you copied the full URL exactly:
```
https://conferencehaven.com/swagger.json
```

**Check 2: Check network connection**
- Make sure you can access conferencehaven.com in your browser
- Corporate firewalls may block external APIs

**Check 3: Try again later**
- Temporary network issues may prevent import
- Wait a few minutes and retry

### Actions Not Working

**Check 1: Actions are enabled**
- Go to Actions in your copilot
- Make sure ConferenceHaven actions are toggled ON

**Check 2: Test with simple queries**
Try basic searches first:
- `"Find sessions at ESPC 2025"`
- `"Show me Microsoft Ignite 2024"`

**Check 3: Check Copilot Studio logs**
- Click **Analytics** → **Conversations**
- Review conversation logs for error messages

### No Results Returned

ConferenceHaven is working! The database might not have the conference you're searching for yet.

**Try these test searches:**
- `"Find sessions at ESPC 2025"`
- `"Show me Microsoft Ignite 2024 sessions"`

**Want to add a conference?** [Request it here](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=conference-request.md)

### Calendar Invites Not Sending

Calendar invites in Copilot Studio require:
1. **Microsoft Graph connector** configured in your copilot
2. **User permissions** to send calendar invites
3. **Outlook integration** enabled

This is an advanced feature—see [Copilot Studio documentation](https://learn.microsoft.com/copilot-studio/) for Graph connector setup.

---

## Publishing Your Copilot

Once you're happy with your copilot:

1. Click **Publish** in the top-right
2. Choose where to publish:
   - **Web widget** - Embed on your website
   - **Teams** - Add to Microsoft Teams
   - **Power Apps** - Integrate with Power Apps
   - **Mobile app** - Via Power Apps mobile

3. Share the link with your team!

---

## Updating ConferenceHaven

ConferenceHaven updates automatically—no action needed! When new conferences or features are added, they're immediately available to your copilot.

**To manually refresh:**
1. Go to **Actions** → **ConferenceHaven**
2. Click **Refresh** or **Re-import**
3. This will fetch the latest Swagger specification

---

## Removing ConferenceHaven

1. In your copilot, go to **Actions**
2. Find **ConferenceHaven** in the list
3. Click **...** (three dots) → **Remove**
4. Confirm removal

---

## Next Steps

- **[Copilot Studio Docs](https://learn.microsoft.com/copilot-studio/)** - Official Microsoft docs
- **[Architecture Overview](../ARCHITECTURE.md)** - How ConferenceHaven works
- **[FAQ](../FAQ.md)** - Common questions

---

## Need Help?

- **GitHub Issues:** [ConferenceHaven-Community/issues](https://github.com/fabianwilliams/ConferenceHaven-Community/issues)
- **Copilot Studio Support:** [Microsoft Learn](https://learn.microsoft.com/copilot-studio/support)
- **Documentation:** [Full docs](../../README.md)

---

**Quick Links:**
- [← Back to Setup Guides](../SETUP-GUIDES.md)
- [Claude Desktop Setup Guide](CLAUDE-DESKTOP.md)
- [ChatGPT Setup Guide](CHATGPT.md)
- [LM Studio Setup Guide](LM-STUDIO.md)

---

## Why Copilot Studio is Different

Unlike other AI clients, Copilot Studio uses **REST APIs** instead of MCP. This means:

✅ **No Node.js required** - Everything is web-based  
✅ **No command-line tools** - Just point and click  
✅ **Enterprise-ready** - Built for business use cases  
✅ **Easy to share** - Publish to Teams, web, or mobile

Perfect for organizations that want AI-powered conference search without complex setup!
