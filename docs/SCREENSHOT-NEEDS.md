# Screenshot Needs for OAuth Documentation

**Purpose:** Visual guide for users setting up ConferenceHaven with OAuth

This document lists all screenshot placeholders in the documentation that need to be filled with actual images.

---

## ChatGPT Setup Guide (`docs/setup/CHATGPT.md`)

### 1. Adding Custom Connector
**Location:** Line 59
**Screenshot:** ChatGPT Settings → Apps & Connectors → Add custom connector dialog

**What to capture:**
- ChatGPT Desktop settings window
- "Apps & Connectors" tab selected
- "Add custom connector" button visible
- Dialog showing Name and URL fields
- Example values filled in:
  - Name: "Conference Haven"
  - URL: "https://mcp.conferencehaven.com/api/mcp"

**Suggested filename:** `chatgpt-add-connector.png`

---

### 2. Tools Indicator
**Location:** Line 104
**Screenshot:** ChatGPT with tools indicator showing ConferenceHaven is connected

**What to capture:**
- ChatGPT Desktop main chat interface
- 🔧 Tools indicator visible in the interface
- ConferenceHaven listed as available tool
- Optionally: hover tooltip showing "ConferenceHaven MCP Server"

**Suggested filename:** `chatgpt-tools-indicator.png`

---

### 3. OAuth Authentication Required
**Location:** Line 160
**Screenshot:** ChatGPT "Authentication Required" prompt

**What to capture:**
- User asks: "Show me analytics for European Collaboration Summit 2026"
- ChatGPT response showing "Authentication Required" message
- Explanation that OAuth is needed
- Button or link to authenticate

**Suggested filename:** `chatgpt-auth-required.png`

---

### 4. Auth0 Login Popup
**Location:** Line 161
**Screenshot:** Auth0 login popup

**What to capture:**
- Auth0 login page in popup window
- ConferenceHaven branding/logo (if customized)
- Email and password fields
- "Continue" or "Log In" button
- Optional: Social login buttons (Google, Microsoft)

**Suggested filename:** `auth0-login-popup.png`

---

### 5. Successful Authentication + Analytics
**Location:** Line 162
**Screenshot:** Successful authentication + analytics displayed

**What to capture:**
- ChatGPT message: "Successfully authenticated as youremail@example.com"
- Analytics data displayed:
  - Top sessions by views
  - Search queries
  - Client usage breakdown
  - Time range (last 30 days)
- Professional-looking analytics output

**Suggested filename:** `chatgpt-analytics-success.png`

---

## Claude Desktop Setup Guide (`docs/setup/CLAUDE-DESKTOP.md`)

### 1. Tools Icon
**Location:** Line 92
**Screenshot:** Claude Desktop with tools icon showing ConferenceHaven is connected

**What to capture:**
- Claude Desktop main chat interface
- 🔨 Tools icon in bottom-right of chat input
- ConferenceHaven listed as available tool
- Optionally: tools panel expanded showing available tools

**Suggested filename:** `claude-tools-icon.png`

---

## Screenshot Specifications

### Technical Requirements

**Resolution:**
- Minimum: 1920x1080 (Full HD)
- Preferred: 2560x1440 (2K) or 3840x2160 (4K)
- Format: PNG (for crisp text)

**Capture Method:**
- macOS: Cmd+Shift+4 → Select area
- Windows: Snipping Tool or Win+Shift+S
- Use built-in screenshot tools for native resolution

**Editing:**
- Blur or redact any personal information (email addresses, names)
- Highlight important UI elements with red boxes or arrows (optional)
- Ensure good contrast and readability
- Compress PNGs for web (use tinypng.com or similar)

---

## Image Storage

**Recommended Structure:**
```
ConferenceHaven-Community/
└── docs/
    └── setup/
        └── images/
            ├── chatgpt/
            │   ├── chatgpt-add-connector.png
            │   ├── chatgpt-tools-indicator.png
            │   ├── chatgpt-auth-required.png
            │   ├── auth0-login-popup.png
            │   └── chatgpt-analytics-success.png
            └── claude/
                └── claude-tools-icon.png
```

---

## Markdown Updates

After taking screenshots, update the placeholder lines:

### Before:
```markdown
> **📸 Screenshot Placeholder:** ChatGPT Settings → Apps & Connectors → Add custom connector dialog
```

### After:
```markdown
![ChatGPT Add Connector](images/chatgpt/chatgpt-add-connector.png)
*ChatGPT Settings → Apps & Connectors → Add custom connector dialog*
```

---

## Priority Order

**High Priority** (User-facing setup):
1. ✅ ChatGPT Add Connector Dialog
2. ✅ ChatGPT Tools Indicator
3. ✅ Claude Desktop Tools Icon

**Medium Priority** (OAuth flow):
4. ⚠️ ChatGPT Auth Required Prompt
5. ⚠️ Auth0 Login Popup
6. ⚠️ Analytics Success Screen

---

## Alternative: Community Contributions

If you don't have time to take all screenshots:

1. **Open a GitHub Issue** requesting screenshot contributions
2. **Provide template** with this document
3. **Community members** can submit screenshots via PR
4. **Review and approve** screenshots for quality

---

## Testing the Screenshots

Before publishing:

1. **View documentation** in GitHub README preview
2. **Check image loading** - all images display correctly
3. **Verify readability** - text is crisp and legible
4. **Confirm relevance** - screenshots match current UI
5. **Test on mobile** - images scale appropriately

---

## Next Steps

1. [ ] Take screenshots according to this guide
2. [ ] Create `docs/setup/images/` directory structure
3. [ ] Save images with suggested filenames
4. [ ] Update markdown files to reference images
5. [ ] Commit and push to repository
6. [ ] Verify images display correctly on GitHub

---

## Questions?

- **GitHub Issues:** [ConferenceHaven-Community/issues](https://github.com/fabianwilliams/ConferenceHaven-Community/issues)
- **Email:** support@conferencehaven.com
