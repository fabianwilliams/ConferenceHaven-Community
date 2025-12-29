# OAuth Authentication for ConferenceHaven

**Last Updated:** December 29, 2025
**OAuth Version:** OAuth 2.1 with PKCE
**Identity Provider:** Auth0

This document explains how OAuth authentication works in ConferenceHaven MCP Server, including technical details for developers and power users.

---

## Table of Contents

- [Overview](#overview)
- [Why OAuth?](#why-oauth)
- [How It Works](#how-it-works)
- [OAuth Flow](#oauth-flow)
- [Client Support](#client-support)
- [Configuration](#configuration)
- [User Tracking](#user-tracking)
- [Privacy & Security](#privacy--security)
- [Troubleshooting](#troubleshooting)
- [For Developers](#for-developers)

---

## Overview

ConferenceHaven uses **OAuth 2.1** with **PKCE** (Proof Key for Code Exchange) for secure authentication. This replaces the previous API key system with a more secure, standards-based approach.

### Key Features

- ✅ **Secure**: No API keys to leak or manage
- ✅ **Standard**: OAuth 2.1 with PKCE (RFC 7636)
- ✅ **Flexible**: Optional for public tools, required for analytics
- ✅ **Transparent**: Clear user consent and data tracking
- ✅ **Automatic**: Native support in ChatGPT (no manual configuration)

---

## Why OAuth?

### Before: API Keys
```
❌ API keys stored in config files
❌ Risk of leaking in logs/repos
❌ No user identity tracking
❌ Difficult to revoke access
❌ No consent mechanism
```

### After: OAuth 2.1
```
✅ Tokens expire automatically (1 hour)
✅ User authenticates via Auth0
✅ User identity tracked for analytics
✅ Easy to revoke access
✅ Built-in consent mechanism
✅ Industry standard security
```

---

## How It Works

### Tool Access Levels

ConferenceHaven has two types of tools:

#### 1. Public Tools (OAuth Optional)
- `search_sessions` - Search conference sessions
- `get_session` - Get session details
- `list_conferences` - List all conferences
- `send_calendar_invite` - Send calendar invites

**Behavior:**
- ✅ Work without authentication (anonymous)
- ✅ Enhanced when authenticated (email auto-fill, user tracking)
- ✅ No OAuth required

#### 2. Protected Tools (OAuth Required)
- `get_organizer_analytics` - View session popularity
- `get_conference_analytics_oauth` - Detailed conference analytics

**Behavior:**
- ❌ Require OAuth authentication
- ❌ Require organizer status in database
- ✅ Clear error messages guide users to authenticate

### Authentication Flow

```
User → MCP Client → MCP Server → Auth0 → User Authentication → Token → MCP Server → Response
```

1. **User** requests a protected tool (e.g., analytics)
2. **MCP Client** (ChatGPT) detects OAuth required
3. **MCP Server** returns authentication error with Auth0 URL
4. **Auth0** shows login page to user
5. **User** authenticates (email + password)
6. **Auth0** returns access token to MCP Client
7. **MCP Client** sends token to MCP Server
8. **MCP Server** validates token and returns data

---

## OAuth Flow

### 1. Discovery Phase

When a client connects to ConferenceHaven, it queries the OAuth metadata endpoint:

```bash
GET https://mcp.conferencehaven.com/.well-known/oauth-protected-resource
```

**Response:**
```json
{
  "resource": "https://mcp.conferencehaven.com/api",
  "authorization_servers": [
    "https://dev-lx3n7y842xhsyhna.us.auth0.com/"
  ],
  "bearer_methods_supported": ["header"],
  "resource_signing_alg_values_supported": ["RS256"],
  "authorization_server_metadata": {
    "issuer": "https://dev-lx3n7y842xhsyhna.us.auth0.com/",
    "authorization_endpoint": "https://dev-lx3n7y842xhsyhna.us.auth0.com/authorize",
    "token_endpoint": "https://dev-lx3n7y842xhsyhna.us.auth0.com/oauth/token",
    "code_challenge_methods_supported": ["S256"],
    "scopes_supported": ["read:analytics", "openid", "profile", "email"]
  }
}
```

### 2. Authorization Phase

Client initiates OAuth flow with PKCE:

```
GET /authorize?
  response_type=code
  &client_id=eysrv7VweawkpzKzNWngCm7ORQCzNsiW
  &redirect_uri=<client_redirect_uri>
  &scope=openid profile email read:analytics
  &code_challenge=<SHA256(code_verifier)>
  &code_challenge_method=S256
```

### 3. User Authentication

User logs in at Auth0:
- Email and password
- Or social login (Google, Microsoft, etc.)
- Multi-factor authentication (if enabled)

### 4. Token Exchange

Client exchanges authorization code for access token:

```
POST /oauth/token
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&code=<authorization_code>
&redirect_uri=<client_redirect_uri>
&client_id=eysrv7VweawkpzKzNWngCm7ORQCzNsiW
&code_verifier=<original_code_verifier>
```

**Response:**
```json
{
  "access_token": "eyJhbGci...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "scope": "openid profile email read:analytics"
}
```

### 5. API Requests

Client makes authenticated requests:

```
POST /api/mcp
Authorization: Bearer eyJhbGci...
Content-Type: application/json

{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "get_organizer_analytics",
    "arguments": { "conference_id": 1 }
  },
  "id": 1
}
```

---

## Client Support

### ✅ ChatGPT Desktop (Native OAuth Support)

**Setup:** Just add the URL - ChatGPT handles OAuth automatically

```json
{
  "mcpServers": {
    "conference-haven": {
      "url": "https://mcp.conferencehaven.com/api/mcp"
    }
  }
}
```

**Experience:**
- 🎯 **Automatic** - ChatGPT detects OAuth metadata endpoint
- 🎯 **Seamless** - OAuth popup appears when needed
- 🎯 **Smart** - Tokens cached between sessions
- 🎯 **Transparent** - Clear "Authenticating..." messages

### ⚠️ Claude Desktop (Limited OAuth Support)

**Setup:** Same URL, but analytics won't work

```json
{
  "mcpServers": {
    "conference-haven": {
      "url": "https://mcp.conferencehaven.com/api/mcp"
    }
  }
}
```

**Experience:**
- ✅ **Public tools work** - Search, view, calendar invites
- ❌ **Analytics don't work** - Requires OAuth (not supported yet)
- ⏳ **Coming soon** - We're working with Anthropic on OAuth support

### 🛠️ Custom MCP Clients

**OAuth Discovery:**
```python
import requests

# Query OAuth metadata
response = requests.get("https://mcp.conferencehaven.com/.well-known/oauth-protected-resource")
oauth_config = response.json()

# Extract Auth0 endpoints
auth_server = oauth_config["authorization_server_metadata"]
auth_url = auth_server["authorization_endpoint"]
token_url = auth_server["token_endpoint"]
```

**PKCE Flow:**
```python
import hashlib
import base64
import secrets

# Generate PKCE code verifier
code_verifier = base64.urlsafe_b64encode(secrets.token_bytes(32)).decode('utf-8').rstrip('=')

# Generate code challenge
code_challenge = base64.urlsafe_b64encode(
    hashlib.sha256(code_verifier.encode('utf-8')).digest()
).decode('utf-8').rstrip('=')

# Build authorization URL
params = {
    "response_type": "code",
    "client_id": "eysrv7VweawkpzKzNWngCm7ORQCzNsiW",
    "redirect_uri": "http://localhost:8080/callback",
    "scope": "openid profile email read:analytics",
    "code_challenge": code_challenge,
    "code_challenge_method": "S256"
}
```

See [MCP OAuth Specification](https://spec.modelcontextprotocol.io/specification/architecture/oauth/) for full details.

---

## Configuration

### Auth0 Application Settings

**Application Type:** Single Page Application (SPA)
**Client ID:** `eysrv7VweawkpzKzNWngCm7ORQCzNsiW`
**Domain:** `dev-lx3n7y842xhsyhna.us.auth0.com`
**API Audience:** `https://mcp.conferencehaven.com/api`

### Allowed Callback URLs

```
https://chatgpt.com/auth/callback
http://localhost:*/callback
http://127.0.0.1:*/callback
```

### Token Configuration

- **Token Lifetime:** 1 hour (3600 seconds)
- **Refresh Tokens:** Not enabled (client must re-authenticate)
- **Token Type:** JWT (JSON Web Token)
- **Signing Algorithm:** RS256

### Scopes

- `openid` - OpenID Connect authentication
- `profile` - User profile information
- `email` - User email address
- `read:analytics` - Read analytics data (organizers only)

---

## User Tracking

### What We Track

When you authenticate via OAuth, we track:

| Data Point | Purpose | Retention |
|-----------|---------|-----------|
| Email address | User identification, calendar invites | 90 days |
| Auth0 user ID | Unique user identification | 90 days |
| Tool usage | Feature adoption, analytics | 90 days |
| Conference engagement | Recommendation improvement | 90 days |
| Session views | Session popularity | 90 days |
| Interaction timestamps | Usage patterns | 90 days |

### Database Schema

**UserInteractions Table:**
```sql
CREATE TABLE UserInteractions (
    id INT IDENTITY(1,1) PRIMARY KEY,
    user_email NVARCHAR(255) NULL,           -- From OAuth token
    auth0_user_id NVARCHAR(255) NULL,        -- Auth0 'sub' claim
    organizer_id INT NULL,                    -- If user is organizer
    is_authenticated BIT DEFAULT 0,           -- OAuth flag
    tool_name NVARCHAR(100),
    conference_id INT,
    session_id INT,
    response_time_ms INT,
    success BIT,
    created_at DATETIME2 DEFAULT GETUTCDATE()
);
```

### Analytics Views

**User Engagement:**
```sql
SELECT
    user_email,
    COUNT(*) AS total_interactions,
    COUNT(DISTINCT tool_name) AS unique_tools_used,
    COUNT(DISTINCT conference_id) AS conferences_engaged
FROM UserInteractions
WHERE is_authenticated = 1
GROUP BY user_email;
```

**Feature Adoption:**
```sql
SELECT
    tool_name,
    COUNT(DISTINCT user_email) AS unique_users,
    AVG(response_time_ms) AS avg_response_time
FROM UserInteractions
WHERE is_authenticated = 1
GROUP BY tool_name
ORDER BY unique_users DESC;
```

---

## Privacy & Security

### Data Protection

- ✅ **HTTPS Only** - All connections encrypted with TLS 1.3
- ✅ **No Password Storage** - Authentication handled by Auth0
- ✅ **Token Expiry** - Access tokens expire after 1 hour
- ✅ **Secure Headers** - CORS, CSP, X-Frame-Options configured
- ✅ **No Logging of Tokens** - Tokens never logged or stored

### User Rights

- **Access:** View your tracked data via analytics endpoints
- **Deletion:** Request data deletion (90-day retention policy)
- **Opt-Out:** Use public tools without authentication
- **Revocation:** Revoke OAuth access at any time

### Compliance

- **GDPR Compliant** - EU data protection regulations
- **CCPA Compliant** - California consumer privacy
- **SOC 2 Type II** - Auth0 is SOC 2 certified
- **ISO 27001** - Auth0 is ISO certified

---

## Troubleshooting

### Token Validation Errors

**Error:** "Invalid or expired OAuth token"

**Causes:**
- Token expired (after 1 hour)
- Token revoked by user
- Auth0 service unavailable

**Solutions:**
1. Re-authenticate (MCP client will prompt automatically)
2. Check Auth0 status: https://status.auth0.com
3. Clear MCP client cache and restart

### UserInfo Endpoint Failures

**Error:** "Failed to validate token with Auth0 UserInfo endpoint"

**Causes:**
- Network connectivity issues
- Auth0 service downtime
- Invalid token format

**Solutions:**
1. Check internet connection
2. Verify Auth0 domain is accessible: `curl https://dev-lx3n7y842xhsyhna.us.auth0.com/userinfo`
3. Check firewall/VPN not blocking Auth0

### Organizer Access Denied

**Error:** "Organizer access required. You are authenticated but not registered as an organizer."

**What this means:**
- ✅ OAuth authentication succeeded
- ❌ Your email is not in OrganizerUsers table
- 📧 Contact conference administrator for access

**Verification:**
```sql
SELECT id, email, full_name, is_active
FROM OrganizerUsers
WHERE email = 'your.email@example.com';
```

---

## For Developers

### Local Development

**Test OAuth Flow:**
```bash
# Start local MCP server
cd mcp-server
python -m uvicorn src.server:app --reload --port 8000

# Test OAuth metadata endpoint
curl http://localhost:8000/.well-known/oauth-protected-resource | jq .

# Test protected endpoint (will fail without token)
curl -X POST http://localhost:8000/api/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"get_organizer_analytics"},"id":1}'

# Test with Bearer token
curl -X POST http://localhost:8000/api/mcp \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"get_organizer_analytics"},"id":1}'
```

### Environment Variables

```bash
# Auth0 Configuration
AUTH0_DOMAIN=dev-lx3n7y842xhsyhna.us.auth0.com
AUTH0_CLIENT_ID=eysrv7VweawkpzKzNWngCm7ORQCzNsiW
AUTH0_API_AUDIENCE=https://mcp.conferencehaven.com/api

# Database Connection
AZURE_SQL_SERVER=sqlsvr-confhaven-lean.database.windows.net
AZURE_SQL_DATABASE=sqldb-confhaven-prod
AZURE_SQL_USERNAME=sqladmin
AZURE_SQL_PASSWORD=<secure_password>
```

### Testing OAuth in Postman

1. **Create new request** → POST `https://mcp.conferencehaven.com/api/mcp`
2. **Authorization tab** → Type: OAuth 2.0
3. **Configure New Token:**
   - Grant Type: Authorization Code (with PKCE)
   - Auth URL: `https://dev-lx3n7y842xhsyhna.us.auth0.com/authorize`
   - Access Token URL: `https://dev-lx3n7y842xhsyhna.us.auth0.com/oauth/token`
   - Client ID: `eysrv7VweawkpzKzNWngCm7ORQCzNsiW`
   - Scope: `openid profile email read:analytics`
   - Code Challenge Method: SHA-256
4. **Get New Access Token** → Login via Auth0
5. **Use Token** → Postman adds `Authorization: Bearer` header automatically

**Test Request Body:**
```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "get_organizer_analytics",
    "arguments": {
      "conference_id": 1,
      "days": 30
    }
  },
  "id": 1
}
```

### Further Reading

- **MCP OAuth Specification:** https://spec.modelcontextprotocol.io/specification/architecture/oauth/
- **Auth0 Documentation:** https://auth0.com/docs
- **OAuth 2.1 RFC:** https://datatracker.ietf.org/doc/html/draft-ietf-oauth-v2-1-10
- **PKCE RFC 7636:** https://datatracker.ietf.org/doc/html/rfc7636

---

## Need Help?

- **GitHub Issues:** [ConferenceHaven-Community/issues](https://github.com/fabianwilliams/ConferenceHaven-Community/issues)
- **Email Support:** support@conferencehaven.com
- **Documentation:** [Full docs](../../README.md)

---

**Quick Links:**
- [← Back to Setup Guides](../SETUP-GUIDES.md)
- [ChatGPT Setup](CHATGPT.md)
- [Claude Desktop Setup](CLAUDE-DESKTOP.md)
- [Architecture Overview](../ARCHITECTURE.md)
