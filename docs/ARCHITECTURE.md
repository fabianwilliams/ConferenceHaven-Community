# ConferenceHaven Architecture

**Last Updated:** November 14, 2025

## System Overview

ConferenceHaven is a cloud-native AI-powered platform for discovering and managing conference sessions across multiple tech events. The platform provides:

- **Natural Language Search**: Find sessions using conversational queries via MCP or agent-chat
- **Multi-Conference Support**: Live360, Microsoft Ignite, ESPC, TechCon365, and more
- **Calendar Integration**: Send calendar invites via Microsoft Graph API
- **Analytics Dashboard**: Track usage, popular sessions, and tool performance
- **OpenTelemetry Observability**: Full distributed tracing with GenAI semantic conventions

---

## Architecture Evolution

### Phase 1: Basic Search (August 2024)

```
┌──────────────────────────────────────────────────┐
│              CLIENT LAYER                        │
│  [Claude Desktop] [VS Code] [Copilot]           │
│   (MCP Client)     (MCP)     (MCP)               │
└────────┬─────────────┬──────────┬────────────────┘
         │             │          │
         v             v          v
┌──────────────────────────────────┐
│   MCP Server (Python/SSE)        │
│                                  │
│ • search_sessions (keyword)      │
│ • send_calendar                  │
└──────────────┬───────────────────┘
               │
               v
┌──────────────────────────────────┐
│   Azure SQL Database             │
│                                  │
│ • Conferences                    │
│ • Sessions                       │
│ • Full-text Search (LIKE)        │
└──────────────────────────────────┘
```

**Limitations:**
- Keyword-only search (no semantic understanding)
- No usage analytics
- No multi-turn conversation support
- Limited client types

---

### Phase 2: Current Architecture (November 2024)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         CLIENT LAYER                                        │
│                                                                             │
│  [Claude Desktop]  [VS Code]  [ChatGPT]  [Copilot]  [Web Interface]       │
│   (MCP Client)     (MCP)      (MCP)      (MCP)       (HTTPS)               │
└────────┬─────────────┬──────────┬────────────┬────────────┬──────────────────┘
         │             │          │            │            │
         └─────────────┴──────────┴────────────┴────────────┘
                       │
         ┌─────────────┴──────────────┐
         │                            │
         v                            v
┌──────────────────────┐    ┌──────────────────────┐
│   MCP Server         │    │   Agent-Chat         │
│   (Python/SSE)       │    │   (Agent Framework)  │
│                      │    │                      │
│ • search_sessions    │    │ • Multi-turn AI      │
│ • send_calendar      │    │ • Session tracking   │
│ • get_analytics      │    │ • Conversation hist  │
│                      │    │                      │
│ Logging:             │    │ Logging:             │
│ → UserInteractions   │    │ → EvaluationData     │
│   (with conf_id)     │    │   (with conf_id)     │
└──────────┬───────────┘    └──────────┬───────────┘
           │                           │
           └───────────┬───────────────┘
                       │
                       v
         ┌─────────────────────────────┐
         │    Backend Agent (FastAPI)  │
         │                             │
         │  • Search sessions          │
         │  • Calendar API             │
         │  • Analytics endpoints      │
         │    - /overview              │
         │    - /sessions-count        │
         │    - /tool-usage            │
         │    - /top-queries           │
         │    - /daily-trend           │
         └─────────────┬───────────────┘
                       │
                       v
         ┌─────────────────────────────┐
         │    Azure SQL Database       │
         │                             │
         │  • Conferences              │
         │  • Sessions                 │
         │  • UserInteractions (MCP)   │
         │  • EvaluationData (Chat)    │
         │  • CalendarInvites          │
         └─────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                    Analytics Dashboard                                      │
│                    (React + Recharts + Auth0)                               │
│                                                                             │
│  • Conference selector with date filtering                                 │
│  • Overview cards (Queries, Sessions, Response Time, Tools)                │
│  • Tool Usage Breakdown (pie chart)                                        │
│  • Daily Trend (line chart)                                                │
│  • Top Queries list                                                        │
│                                                                             │
│  Queries: Backend API → UserInteractions + EvaluationData (dual-table)     │
│  URL: https://analytics.conferencehaven.com                                │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                    OpenTelemetry / Observability                            │
│                    (Aspire Dashboard via OTLP/gRPC)                         │
│                                                                             │
│  • GenAI semantic conventions (gen_ai.operation.name)                      │
│  • MCP-specific attributes (mcp.client.type, mcp.tool.name)                │
│  • Distributed tracing across all components                               │
│  URL: https://aspire.conferencehaven.com                                   │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Key Improvements:**
- Dual analytics tables (UserInteractions + EvaluationData)
- Conference ID extraction for accurate analytics
- Multi-client support (MCP, web, chat, Copilot)
- Real-time analytics dashboard
- OpenTelemetry observability with GenAI conventions
- Calendar invite tracking

**Remaining Limitations:**
- Still keyword-only search ("MCP" doesn't find "Model Context Protocol")
- No semantic understanding of queries
- No session embedding/vector search

---

### Phase 3: Planned - Hybrid Semantic Search (In Progress)

```
[Coming Soon: Adding Qdrant vector DB for semantic search]

Search Flow Enhancement:
┌─────────────────────┐
│   User Query        │
│   "MCP sessions"    │
└──────────┬──────────┘
           │
           v
┌─────────────────────────────────────────┐
│      Hybrid Search Engine               │
│                                         │
│  ┌──────────────┐  ┌─────────────────┐ │
│  │  Keyword     │  │  Semantic       │ │
│  │  (SQL LIKE)  │  │  (Qdrant Vector)│ │
│  │              │  │                 │ │
│  │ Finds: "MCP" │  │ Finds: "Model   │ │
│  │              │  │  Context Proto" │ │
│  └──────────────┘  └─────────────────┘ │
│           │               │             │
│           └───────┬───────┘             │
│                   │                     │
│           ┌───────v────────┐            │
│           │  RRF Merge     │            │
│           │  (Re-rank)     │            │
│           └────────────────┘            │
└─────────────────────────────────────────┘
```

**Planned Features:**
- Qdrant vector database deployment
- Small embedding model (all-MiniLM-L6-v2, 384 dims)
- Reciprocal Rank Fusion (RRF) for result merging
- Session embeddings cached in Qdrant
- Improved search quality for synonyms and semantic matches

---

## Core Components

### 1. MCP Server (mcp-server/)
- **Purpose**: Primary interface for AI clients using Model Context Protocol
- **Technology**: Python, Starlette, SSE
- **Tools**: search_sessions, get_session, list_conferences, send_calendar_invite, get_organizer_analytics
- **Logging**: Writes to UserInteractions with conference_id extraction
- **URL**: https://mcp.conferencehaven.com

### 2. Agent-Chat (agent-chat/)
- **Purpose**: Conversational AI agent with multi-turn context
- **Technology**: Python, Agent Framework, OpenAI GPT-4o-mini
- **Features**: Conversation history, session tracking
- **Logging**: Writes to EvaluationData via backend API
- **URL**: https://chat.conferencehaven.com

### 3. Backend Agent (backend-agent/)
- **Purpose**: Core business logic and database operations
- **Technology**: Python, FastAPI, SQLAlchemy, Azure SQL
- **Features**: Full-text search, calendar API, dual-table analytics

### 4. Analytics Dashboard (analytics-dashboard/)
- **Purpose**: Visual analytics for conference organizers
- **Technology**: React 18, Vite, Tailwind CSS, Recharts, Auth0
- **URL**: https://analytics.conferencehaven.com
- **Current Features** (v1.1):
  - Conference selector with date range filtering
  - Overview cards (Total Queries, Sessions, Avg Response Time, Tools Used)
  - Tool Usage Breakdown (pie chart showing MCP tool distribution)
  - Daily Usage Trend (line chart with query volume and response times)
  - Top Queries List (most common user questions)

---

## MCP Protocol Implementation

### Tools Exposed

1. **search_sessions**
   - Input: `query`, `conference`, `track`, `level`, `limit`, `include_past`
   - Output: List of session objects
   - Timeout: 10 seconds

2. **get_session**
   - Input: `session_id`
   - Output: Full session object with details
   - Timeout: 5 seconds

3. **list_conferences**
   - Input: `include_past` (optional)
   - Output: List of conference objects
   - Timeout: 5 seconds

4. **send_calendar_invite**
   - Input: `session_id`, `user_email`
   - Output: Success/error message with calendar event
   - Timeout: 15 seconds

5. **get_organizer_analytics** (authenticated)
   - Input: `api_key`, `conference_id`, `days`
   - Output: Analytics JSON
   - Timeout: 20 seconds

### Transport

**Server-Sent Events (SSE)**:
- Endpoint: `https://mcp.conferencehaven.com/sse`
- Keeps connection open for streaming
- Heartbeats every 30 seconds
- Reconnect on disconnect

**Direct REST**:
- Endpoint: `https://mcp.conferencehaven.com/api/*`
- OpenAPI/Swagger documentation
- For non-MCP clients

---

## Data Model

### Core Tables

**Conferences**
- Conference metadata (name, location, dates)
- `is_active` flag for visibility
- Public API access

**Sessions**
- Full session details (title, abstract, speakers)
- Time and location information
- Full-text search indexes
- Tags and difficulty levels

**UserInteractions** (MCP Server Logs)
```sql
CREATE TABLE UserInteractions (
    id INT PRIMARY KEY,
    client_type NVARCHAR(50),  -- claude_desktop, vscode_copilot, copilot, etc.
    tool_name NVARCHAR(100),   -- search_sessions, send_calendar_invite
    conference_id INT NULL,     -- Extracted from query
    session_id INT NULL,
    response_time_ms INT,
    created_at DATETIME
);
```

**EvaluationData** (Agent-Chat Logs)
```sql
CREATE TABLE EvaluationData (
    id INT PRIMARY KEY,
    user_query NVARCHAR(MAX),
    ai_response NVARCHAR(MAX),
    conference_id INT NULL,  -- Extracted from response
    session_id INT NULL,
    session_id NVARCHAR(200), -- Chat session ID
    tool_name NVARCHAR(100),
    tokens_used INT,
    response_time_ms INT,
    trace_id NVARCHAR(200),
    created_at DATETIME
);
```

**CalendarInvites**
```sql
CREATE TABLE CalendarInvites (
    id INT PRIMARY KEY,
    session_id INT,
    recipient_email NVARCHAR(255),
    sent_at DATETIME,
    status NVARCHAR(50)  -- sent, failed
);
```

---

## Search Architecture

### Current: Full-Text Search

Uses Azure SQL's `CONTAINS()` for natural language search:

```sql
SELECT * FROM Sessions
WHERE CONTAINS((title, abstract, tags), @search_query)
AND conference_id = @conference_id
ORDER BY relevance DESC
```

### Ranking Algorithm

1. **Exact title match**: +10 points
2. **Title partial match**: +5 points
3. **Abstract match**: +2 points
4. **Tag match**: +3 points
5. **Speaker match**: +1 point

### Planned: Hybrid Semantic Search

- Qdrant vector database for semantic understanding
- Small embedding model (all-MiniLM-L6-v2)
- RRF (Reciprocal Rank Fusion) to combine keyword + semantic results
- Improved handling of synonyms ("MCP" → "Model Context Protocol")

---

## OpenTelemetry & Observability

- **Aspire Dashboard**: https://aspire.conferencehaven.com
- **Protocol**: OTLP/gRPC on port 4317
- **GenAI Semantic Conventions**:
  - `gen_ai.operation.name` - search, chat, calendar_invite
  - `gen_ai.tool.name` - search_sessions, send_calendar_invite
  - `mcp.client.type` - claude_desktop, vscode_copilot, copilot
  - `gen_ai.response.time_ms` - Response time tracking
  - `gen_ai.conference.id` - Conference context
- **Sampling**: 100% for development, configurable for production
- **Exporters**: Console (dev), OTLP (production)

---

## Deployment Architecture

| Service | Technology | Platform |
|---------|------------|----------|
| MCP Server | Python, Starlette, SSE | Azure Container Apps |
| Agent-Chat | Python, Agent Framework, OpenAI | Azure Container Apps |
| Backend API | Python, FastAPI, SQLAlchemy | Azure Container Apps |
| Analytics Dashboard | React 18, Vite, Recharts | Azure Container Apps |
| Database | Azure SQL | Managed Database |
| Observability | Aspire Dashboard | Azure Container Instances |

### CI/CD Pipeline

```
GitHub Push
  ↓
GitHub Actions
  ↓
Build Docker Image (Python 3.11-slim / Node 18)
  ↓
Push to Azure Container Registry
  ↓
Deploy to Azure Container Apps
  ↓
Health Check
  ↓
Route Traffic
```

### Scaling

**MCP Server** (Azure Container Apps):
- **Min replicas**: 1
- **Max replicas**: 10
- **Scale trigger**: CPU > 70% or Request Queue > 20

**Database** (Azure SQL):
- **Current**: Basic tier (sufficient for current load)
- **Plan**: Upgrade to Standard for production scaling

---

## Security & Authentication

### Current State
- **Public endpoints**: No authentication (search, list conferences)
- **Analytics Dashboard**: Auth0 authentication required
- **Calendar invites**: Microsoft Graph OAuth2

### Data Protection
- **SQL Injection**: Parameterized queries only
- **XSS**: Input sanitization
- **Secrets**: Azure Key Vault
- **Encryption**: TLS in transit (HTTPS), at-rest (Azure SQL)

---

## Technology Stack

### Backend
- **Language**: Python 3.11+
- **Framework**: FastAPI (async/await)
- **ORM**: SQLAlchemy 2.0
- **Validation**: Pydantic

### Frontend
- **Framework**: React 18
- **Build**: Vite
- **Styling**: Tailwind CSS
- **Charts**: Recharts
- **Auth**: Auth0

### Infrastructure
- **Hosting**: Azure Container Apps
- **CI/CD**: GitHub Actions
- **Registry**: Azure Container Registry
- **Database**: Azure SQL
- **Observability**: Aspire Dashboard

### Integrations
- **Calendar**: Microsoft Graph API
- **AI**: OpenAI GPT-4o-mini
- **Auth**: Auth0

---

## Roadmap

### Completed ✅
- [x] MCP server with SSE transport
- [x] Multi-conference support (Live360, Ignite, ESPC, TechCon365)
- [x] Calendar integration (Microsoft Graph)
- [x] Dual-table analytics (UserInteractions + EvaluationData)
- [x] Analytics dashboard with Auth0
- [x] OpenTelemetry with GenAI conventions
- [x] Conference ID extraction
- [x] Multi-client support (Claude, VS Code, ChatGPT, Copilot)

### In Progress 🚧
- [ ] Hybrid semantic search with Qdrant
- [ ] Small embedding model integration
- [ ] Improved search quality

### Planned 📋
- [ ] Redis caching layer
- [ ] Advanced analytics (cohort analysis, funnel metrics)
- [ ] Session recommendation engine
- [ ] Multi-region deployment

---

## Key Design Decisions

### Why MCP?
- Universal protocol for AI integrations
- Supported by all major AI platforms (Claude, ChatGPT, Copilot, VS Code)
- Better than building separate integrations for each platform

### Why FastAPI?
- Modern Python async/await
- Automatic OpenAPI generation
- High performance
- Rich type hints with Pydantic

### Why Azure SQL vs. MongoDB?
- Full-text search built-in
- Strong consistency for calendar/analytics
- SQL is familiar for most contributors
- Good integration with Azure ecosystem

### Why Dual-Table Analytics?
- UserInteractions tracks MCP server usage
- EvaluationData tracks agent-chat conversations
- Conference ID extraction enables accurate analytics
- Combine both for complete usage picture

---

## Getting Started

### For Developers

Want to contribute or deploy your own instance?

1. Check out the [Setup Guides](SETUP-GUIDES.md)
2. Review the [FAQ](FAQ.md)
3. Read [Troubleshooting](TROUBLESHOOTING.md)

### For Conference Organizers

Interested in adding your conference to ConferenceHaven?

- Email: fabian [at] conferencehaven.com
- Join our community discussions
- Check out the [Organizer Guide](ORGANIZER-GUIDE.md)

---

## Additional Resources

- **Main Repository**: [github.com/fabianwilliams/ConferenceHaven](https://github.com/fabianwilliams/ConferenceHaven) (private - organizer access)
- **Community Repository**: [github.com/fabianwilliams/ConferenceHaven-Community](https://github.com/fabianwilliams/ConferenceHaven-Community) (public)
- **Live Site**: [conferencehaven.com](https://conferencehaven.com)
- **MCP Endpoint**: [mcp.conferencehaven.com](https://mcp.conferencehaven.com)
- **Analytics**: [analytics.conferencehaven.com](https://analytics.conferencehaven.com)

---

**Last updated**: November 14, 2025
