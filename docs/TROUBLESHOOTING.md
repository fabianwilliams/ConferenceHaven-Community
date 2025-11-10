# Troubleshooting Guide

Solutions to common ConferenceHaven issues.

---

## 🔌 MCP Tools Not Appearing

### Claude Desktop

**Symptom**: No 🔌 icon in bottom left corner, or tools don't show up when querying.

**Solutions**:

1. **Verify config file location**:
   ```bash
   # macOS
   cat ~/Library/Application\ Support/Claude/claude_desktop_config.json
   
   # Windows
   type %APPDATA%\Claude\claude_desktop_config.json
   
   # Linux
   cat ~/.config/Claude/claude_desktop_config.json
   ```

2. **Check JSON validity**:
   - No trailing commas
   - Proper quotes (use `"`, not `'`)
   - Valid JSON structure
   
   Test with:
   ```bash
   cat claude_desktop_config.json | jq
   ```

3. **Verify npx is installed**:
   ```bash
   npx --version
   ```
   
   If not installed:
   ```bash
   # Install Node.js from nodejs.org
   # Or use package manager:
   brew install node    # macOS
   choco install nodejs # Windows
   ```

4. **Test MCP connection manually**:
   ```bash
   npx -y @fabianwilliams/mcp-remote https://mcp.conferencehaven.com
   ```
   
   Should output: "MCP Remote Client connected to..."

5. **Restart Claude Desktop completely**:
   - Quit from menu (not just close window)
   - On macOS: `Cmd+Q`
   - On Windows: Right-click taskbar icon → Quit

6. **Check Claude logs** (macOS):
   ```bash
   tail -f ~/Library/Logs/Claude/mcp*.log
   ```

**Expected config**:
```json
{
  "mcpServers": {
    "conferencehaven": {
      "command": "npx",
      "args": [
        "-y",
        "@fabianwilliams/mcp-remote",
        "https://mcp.conferencehaven.com"
      ]
    }
  }
}
```

### GitHub Copilot (VS Code)

**Symptom**: ConferenceHaven tools not available in Copilot Chat.

**Solutions**:

1. **Verify settings.json**:
   ```bash
   # Check workspace settings
   cat .vscode/settings.json
   ```

2. **Check GitHub Copilot extension is active**:
   - Open Command Palette: `Cmd/Ctrl+Shift+P`
   - Search "Copilot: Status"
   - Verify status is "Ready"

3. **Reload VS Code window**:
   - Command Palette → "Developer: Reload Window"

4. **Check MCP is enabled** in VS Code settings:
   - Settings → Search "MCP"
   - Verify "GitHub Copilot: Enable MCP" is checked

5. **Try user settings instead of workspace**:
   ```json
   // ~/.config/Code/User/settings.json (macOS/Linux)
   // %APPDATA%\Code\User\settings.json (Windows)
   {
     "github.copilot.advanced": {
       "mcp.servers": {
         "conferencehaven": {
           "command": "npx",
           "args": ["-y", "@fabianwilliams/mcp-remote", "https://mcp.conferencehaven.com"]
         }
       }
     }
   }
   ```

### LM Studio

**Symptom**: MCP server doesn't connect, tools not showing.

**Solutions**:

1. **Check MCP is enabled**:
   - LM Studio → Settings → MCP Servers
   - Verify "Enable MCP" is checked

2. **Verify server configuration**:
   - Settings → MCP Servers → Edit Config
   - Should match Claude Desktop config format

3. **Test with a function-calling model**:
   - Not all models support function calling
   - Recommended: Llama 3.2, Mistral, Qwen
   - Check model description for "function calling" support

4. **Check model is loaded**:
   - Make sure model is fully loaded (progress bar complete)
   - Try reloading the model

5. **Restart LM Studio completely**

---

## 🔍 Search Issues

### No Results Returned

**Symptom**: Search returns "No sessions found" when you know sessions exist.

**Solutions**:

1. **Try broader search terms**:
   ```
   ❌ "Microsoft 365 Copilot integration with SharePoint and Teams"
   ✅ "Copilot SharePoint"
   ✅ "Copilot"
   ```

2. **Include past conferences**:
   - Add `include_past=true` to search
   - Or ask: "Search all conferences including past ones"

3. **Verify conference has sessions**:
   ```
   "List all conferences"
   ```
   Check that your target conference shows sessions > 0.

4. **Check conference slug**:
   ```
   ❌ "ESPC 2025"
   ✅ "espc25"
   ```
   Use slugs from `list_conferences` output.

5. **Try without filters**:
   - Remove track, level, or other filters
   - Start broad, then narrow down

### Search Returns Wrong Results

**Symptom**: Results don't match your query intent.

**Solutions**:

1. **Be more specific**:
   ```
   ❌ "AI sessions"           → Too broad
   ✅ "Copilot AI sessions"   → Better
   ✅ "AI sessions about automation" → Even better
   ```

2. **Use exact phrases**:
   ```
   "Find sessions with 'Power Automate' in the title"
   ```

3. **Specify the conference**:
   ```
   "Find Copilot sessions at ESPC25"
   ```

4. **Filter by track or level**:
   ```
   "Find beginner sessions about SharePoint at ESPC25"
   "Show me sessions in the Developer track"
   ```

### Slow Search Performance

**Symptom**: Search takes > 5 seconds to return results.

**Possible causes**:
- Database query complexity
- Too many results (default limit is 20)
- Server load

**Solutions**:

1. **Reduce limit**:
   ```
   "Find Copilot sessions, limit to 10 results"
   ```

2. **Use more specific filters**:
   ```
   "Find Copilot sessions at ESPC25 in the Business track"
   ```

3. **Check server health**:
   ```bash
   curl https://mcp.conferencehaven.com/health
   ```

4. **Report persistent slowness**:
   [Open performance issue](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=bug_report.md)

---

## 📧 Calendar Invite Issues

### Invite Not Arriving

**Symptom**: Requested calendar invite but never received it.

**Solutions**:

1. **Check spam/junk folder**:
   - Search for "conferencehaven.com"
   - Mark as "Not Spam" if found

2. **Verify email address**:
   ```
   "Send calendar invite for session 935 to fabian@example.com"
   ```
   Double-check spelling.

3. **Check email server delays**:
   - Can take 1-5 minutes
   - Gmail: Usually instant
   - Corporate email: May have delays or blocks

4. **Whitelist sender**:
   Add `noreply@conferencehaven.com` to safe senders list.

5. **Try a different email**:
   - Gmail often works best for testing
   - Yahoo/iCloud should work fine
   - Corporate emails may block

6. **Check server logs** (organizer only):
   ```bash
   # Verify invite was sent
   curl -H "X-API-Key: YOUR_KEY" \
     https://mcp.conferencehaven.com/api/organizer/logs?type=calendar
   ```

### Wrong Timezone in Calendar

**Symptom**: Session shows at wrong time in your calendar.

**Root cause**: Session times are in conference local timezone (e.g., Stockholm for ESPC25).

**Solutions**:

1. **Check your calendar app timezone settings**:
   - Google Calendar: Settings → General → Time zone
   - Outlook: File → Options → Calendar → Time zones
   - Apple Calendar: System Preferences → Date & Time → Time Zone

2. **Check device timezone**:
   - Make sure automatic timezone detection is enabled
   - Or manually set your timezone

3. **Expected behavior**:
   - Session: "10:00 AM Stockholm time"
   - Your calendar in New York: Should show "4:00 AM EST"
   - Your calendar in Tokyo: Should show "6:00 PM JST"

4. **If timezone is still wrong**:
   [Report issue](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=bug_report.md) with:
   - Session ID
   - Your timezone
   - What time you see vs. what you expect

### Calendar File Won't Open

**Symptom**: `.ics` file downloaded but won't open in calendar app.

**Solutions**:

1. **Check default calendar app**:
   - macOS: Should open in Apple Calendar by default
   - Windows: Should open in Outlook by default
   - Can also manually import to Google Calendar

2. **Manual import to Google Calendar**:
   - Open [calendar.google.com](https://calendar.google.com)
   - Click "+" next to "Other calendars"
   - Select "Import"
   - Choose downloaded `.ics` file

3. **Manual import to Outlook**:
   - File → Open & Export → Import/Export
   - Import an iCalendar (.ics) file
   - Select downloaded file

4. **Check file integrity**:
   ```bash
   cat session_935_invite.ics | head
   ```
   Should start with `BEGIN:VCALENDAR`.

---

## 🌐 Connection Issues

### "Cannot connect to MCP server"

**Symptom**: Error message when AI tries to use tools.

**Solutions**:

1. **Verify server is up**:
   ```bash
   curl https://mcp.conferencehaven.com/health
   ```
   
   Expected response:
   ```json
   {
     "status": "healthy",
     "service": "ConferenceHaven MCP Server"
   }
   ```

2. **Check your internet connection**:
   ```bash
   ping confhaven-mcp.io
   ```

3. **Verify URL is correct**:
   ```
   ✅ https://mcp.conferencehaven.com
   ❌ https://mcp.conferencehaven.com/    (trailing slash)
   ❌ https://confhaven-mcp.io    (https not yet supported)
   ❌ http://conferencehaven.com  (wrong domain)
   ```

4. **Check firewall/proxy**:
   - Corporate networks may block external MCP connections
   - Try from personal device/network to test

5. **Restart MCP client**:
   - Restart your AI assistant completely
   - May need to clear cache

### "Connection timeout"

**Symptom**: Request hangs for 30+ seconds then times out.

**Possible causes**:
- Network issues
- Server under load
- Database query too complex

**Solutions**:

1. **Retry the request**:
   - Sometimes transient network issues
   - Give it 30 seconds between retries

2. **Simplify the query**:
   ```
   ❌ "Find all sessions about AI, Copilot, SharePoint, Teams, Power Automate, etc."
   ✅ "Find AI sessions"
   ```

3. **Check status page** (coming soon):
   - Will provide real-time server status
   - Incident notifications

4. **Report persistent timeouts**:
   [Open issue](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=bug_report.md)

---

## 🛠️ Tool-Specific Issues

### search_sessions

**Error: "Invalid conference slug"**

Solution:
```
"List all conferences"  ← Get valid slugs
"Search sessions in espc25"  ← Use exact slug
```

**Error: "Invalid level"**

Solution: Use one of:
- `Introductory`
- `Intermediate`
- `Advanced`

Not "Beginner" or "Intro".

### get_session

**Error: "Session not found"**

Solutions:
1. Verify session ID is correct (numeric)
2. Check session exists: `"List all sessions in espc25"`
3. May have been deleted by organizer

### send_calendar_invite

**Error: "Invalid email address"**

Solutions:
1. Check email format: `user@example.com`
2. No spaces or special characters (except `.` `-` `_`)
3. Must include `@` and domain

**Error: "Session is in the past"**

Solutions:
1. Can only send invites for future sessions
2. Past sessions can still be viewed with `get_session`

### get_organizer_analytics

**Error: "Authentication required"**

Solutions:
1. Must provide `X-API-Key` header or `api_key` parameter
2. Contact conferencehaven@adotob.com for organizer API key
3. Not available to general users (organizers only)

---

## 📱 Platform-Specific Issues

### macOS

**Issue: "npx command not found"**

Solution:
```bash
# Install Node.js
brew install node

# Verify
npx --version
```

**Issue: "Permission denied" errors**

Solution:
```bash
# Fix npm permissions
sudo chown -R $(whoami) ~/.npm
```

### Windows

**Issue: "npx is not recognized"**

Solution:
1. Install Node.js from [nodejs.org](https://nodejs.org)
2. Restart terminal/VS Code
3. Verify: `npx --version`

**Issue: "Config file not found"**

Solution:
```cmd
# Create config directory
mkdir %APPDATA%\Claude

# Create config file
notepad %APPDATA%\Claude\claude_desktop_config.json
```

### Linux

**Issue: "Cannot find config file"**

Solution:
```bash
# Create config directory
mkdir -p ~/.config/Claude

# Create config file
nano ~/.config/Claude/claude_desktop_config.json
```

---

## 🔬 Debugging Tips

### Enable Verbose Logging (Claude Desktop)

1. Set environment variable:
   ```bash
   # macOS/Linux
   export CLAUDE_DEBUG=1
   
   # Windows
   set CLAUDE_DEBUG=1
   ```

2. Restart Claude Desktop

3. View logs:
   ```bash
   # macOS
   tail -f ~/Library/Logs/Claude/mcp*.log
   
   # Windows
   type %LOCALAPPDATA%\Claude\logs\mcp*.log
   ```

### Test MCP Connection Manually

```bash
# Test connection
npx -y @fabianwilliams/mcp-remote https://mcp.conferencehaven.com

# Should output connection confirmation
# Press Ctrl+C to exit
```

### Test REST API Directly

```bash
# Health check
curl https://mcp.conferencehaven.com/health

# List conferences
curl https://mcp.conferencehaven.com/api/conferences

# Search sessions
curl "https://mcp.conferencehaven.com/api/sessions?query=copilot&limit=5"

# Get session details
curl https://mcp.conferencehaven.com/api/sessions/935
```

### Check OpenAPI Schema

```bash
# View available endpoints
curl https://mcp.conferencehaven.com/api/swagger.json | jq
```

---

## 🆘 Getting Help

### Before Reporting an Issue

Collect this information:
1. **What you tried**: Exact query or command
2. **What happened**: Error message, unexpected behavior
3. **What you expected**: What should have happened
4. **Your setup**: 
   - AI client (Claude Desktop, ChatGPT, etc.)
   - OS (macOS, Windows, Linux)
   - Version (if known)
5. **Logs**: Any error messages or logs

### Reporting Issues

[Open a bug report](https://github.com/fabianwilliams/ConferenceHaven-Community/issues/new?template=bug_report.md) with the above information.

### Need Immediate Help?

- **Community discussions**: [GitHub Discussions](https://github.com/fabianwilliams/ConferenceHaven-Community/discussions)
- **Email**: conferencehaven@adotob.com (allow 24-48 hours)

---

**Last updated**: November 9, 2025
