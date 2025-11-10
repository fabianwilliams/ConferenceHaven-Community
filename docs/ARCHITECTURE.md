# Architecture Overview

High-level technical overview of ConferenceHaven.

---

## 🏗️ System Architecture

ConferenceHaven is a cloud-native MCP (Model Context Protocol) server that provides AI assistants with conference session search and calendar integration capabilities.

```
┌─────────────────┐
│   AI Clients    │  (Claude, ChatGPT, Copilot, LM Studio)
└────────┬────────┘
         │ MCP / OpenAPI
         ▼
┌─────────────────┐
│   MCP Server    │  (Python FastAPI + Agent Framework)
│  confhaven-mcp  │  Handles tool calls, observability
└────────┬────────┘
         │
    ┌────┴────┬──────────┬─────────────┐
    ▼         ▼          ▼             ▼
┌────────┐ ┌──────┐  ┌─────────┐  ┌────────┐
│ Azure  │ │ MS   │  │ Aspire  │  │ Stripe │
│  SQL   │ │ Graph│  │Dashboard│  │   API  │
└────────┘ └──────┘  └─────────┘  └────────┘
  Sessions  Calendar  Observability Payments
```

### Key Components

1. **MCP Server** (`mcp-server/`)
   - Python FastAPI application
   - Agent Framework for MCP protocol
   - SSE (Server-Sent Events) transport
   - Deployed to Azure Container Apps

2. **Backend Agent** (`backend-agent/`)
   - REST API façade over Azure SQL
   - SQLAlchemy ORM
   - Background scraping and data sync

3. **Database** (`database/`)
   - Azure SQL Database
   - Full-text search indexes
   - Evaluation and analytics tables

4. **Observability Stack**
   - OpenTelemetry (OTLP/gRPC)
   - Aspire Dashboard for logs, traces, metrics
   - Deployed to Azure Container Instances

5. **Static Web** (`static-web/`)
   - Azure Static Web Apps
   - Landing page + documentation
   - Stripe onboarding flow

---

## 🔧 Technology Stack

### Backend
- **Language**: Python 3.11+
- **Framework**: FastAPI (async/await)
- **MCP**: Agent Framework (Microsoft)
- **ORM**: SQLAlchemy 2.0
- **HTTP**: Starlette, HTTPX
- **Validation**: Pydantic

### Data
- **Primary DB**: Azure SQL Database
- **Search**: Full-text search with `CONTAINS()` queries
- **Caching**: Azure Cache for Redis (planned)

### Observability
- **Protocol**: OpenTelemetry (OTLP/gRPC)
- **Telemetry**: Logs, Traces, Metrics
- **Dashboard**: Aspire Dashboard (Blazor)
- **Sampling**: AlwaysOnSampler (100% capture)

### Infrastructure
- **Hosting**: Azure Container Apps (MCP server)
- **CI/CD**: GitHub Actions
- **Registry**: Azure Container Registry (ACR)
- **Domain**: Cloudflare DNS + Azure Front Door

### Integrations
- **Calendar**: Microsoft Graph API (send invites)
- **Payments**: Stripe (organizer subscriptions)
- **Auth**: Azure Entra ID (organizers)

---

## 📊 Data Model

### Core Tables

**Conferences**
- `id` - Primary key
- `name` - Display name (e.g., "Microsoft ESPC25")
- `slug` - URL-safe identifier (e.g., "espc25")
- `location` - City, venue
- `start_date`, `end_date` - Conference dates
- `website` - Official URL
- `is_active` - Visibility flag

**Sessions**
- `id` - Primary key
- `conference_id` - Foreign key to Conferences
- `external_id` - ID from source system
- `title` - Session title
- `abstract` - Full description
- `speaker_names` - Comma-separated
- `start_time`, `end_time` - Session times (in conference timezone)
- `room` - Location/track
- `level` - Introductory/Intermediate/Advanced
- `tags` - Comma-separated keywords
- Full-text index on `title`, `abstract`, `tags`

**EvaluationData**
- `id` - Primary key
- `user_query` - Natural language query
- `tool_name` - Which tool was called
- `tool_parameters` - JSON parameters
- `response_json` - JSON response
- `trace_id` - OpenTelemetry trace ID
- `created_at` - Timestamp
- `status` - pending/approved/rejected
- `human_label` - Quality score (1-5)
- `llm_judge_score` - Automated evaluation

**Organizers**
- `id` - Primary key
- `email` - Azure Entra ID email
- `api_key` - For API access
- `stripe_customer_id` - Payment tracking
- `subscription_tier` - free/pro/enterprise

### Relationships
```
Conferences (1) ←→ (N) Sessions
Conferences (1) ←→ (N) Organizers
EvaluationData (1) ←→ (1) Trace (via trace_id)
```

---

## 🔍 Search Architecture

### Full-Text Search

Uses Azure SQL's `CONTAINS()` for natural language search:

```sql
SELECT * FROM Sessions
WHERE CONTAINS((title, abstract, tags), 'copilot OR "AI assistant"')
AND conference_id = @conference_id
ORDER BY relevance DESC
```

### Ranking Algorithm

1. **Exact title match**: +10 points
2. **Title partial match**: +5 points
3. **Abstract match**: +2 points
4. **Tag match**: +3 points
5. **Speaker match**: +1 point

### Future Enhancements
- Vector embeddings (OpenAI Ada-002)
- Semantic search with Azure AI Search
- User preference learning
- Session recommendations

---

## 🛠️ MCP Protocol Implementation

ConferenceHaven implements MCP using Microsoft Agent Framework:

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
   - Output: Success/error message
   - Timeout: 15 seconds (includes email sending)

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
- OpenAPI/Swagger: `/api/swagger.json`
- For non-MCP clients (ChatGPT, Copilot Studio)

---

## 📈 Observability Architecture

### OpenTelemetry Configuration

**Exporter**: OTLP/gRPC to Aspire Dashboard
**Endpoint**: `http://aspire-confhaven.eastus.azurecontainer.io:4317`
**Sampling**: AlwaysOnSampler (100% trace capture)

### Telemetry Captured

**Logs**:
- HTTP requests/responses
- Database queries
- Tool invocations
- Errors and exceptions
- Custom events (user queries, client detection)

**Traces**:
- Full request lifecycle
- Tool execution spans
- Database query spans
- External API calls (Graph, Stripe)
- Parent/child span relationships

**Metrics**:
- Request count, duration
- Tool usage frequency
- Error rates
- Database connection pool
- Session search volume

### Custom Attributes

- `mcp.tool_name` - Which tool was called
- `mcp.user.query` - User's natural language query
- `mcp.client_type` - electron_app / chatgpt / copilot / unknown
- `mcp.conference_slug` - Target conference
- `session.count` - Number of results returned
- `db.query_duration` - SQL execution time

---

## 🔐 Security Model

### Authentication

**Public Endpoints** (no auth required):
- `search_sessions`
- `get_session`
- `list_conferences`
- `send_calendar_invite`

**Authenticated Endpoints**:
- `get_organizer_analytics` - Requires `X-API-Key` header
- Organizer management - Azure Entra ID OAuth

### Authorization

**Organizers** can:
- View analytics for their conferences only
- Update their conference data
- Manage sessions

**Attendees** can:
- Search all public conferences
- Get session details
- Request calendar invites (no account needed)

### Data Protection

- **SQL Injection**: Parameterized queries only
- **XSS**: Input sanitization
- **Rate Limiting**: 100 requests/minute per IP (planned)
- **DDoS**: Azure Front Door WAF (planned)
- **Secrets**: Azure Key Vault
- **Encryption**: TLS in transit (HTTPS coming), at-rest (Azure SQL)

---

## 🚀 Deployment Architecture

### Environments

**Production**:
- MCP Server: `https://mcp.conferencehaven.com`
- Aspire Dashboard: `http://aspire-confhaven.eastus.azurecontainer.io:18888`
- Static Web: `https://conferencehaven.com`

**Development**:
- Local Docker Compose
- SQLite or local Azure SQL
- Local Aspire Dashboard

### CI/CD Pipeline

```
GitHub Push
  ↓
GitHub Actions
  ↓
Build Docker Image (Python 3.11-slim)
  ↓
Push to Azure Container Registry
  ↓
Deploy to Azure Container Apps (Blue/Green)
  ↓
Run Database Migrations
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
- **Cold start**: ~2 seconds

**Database** (Azure SQL):
- **Current**: Basic tier (5 DTUs)
- **Plan**: Upgrade to Standard S2 (50 DTUs) for production load
- **Connections**: Max 100 concurrent (connection pooling)

**Observability** (Aspire Dashboard):
- **Fixed**: 1 container (2 vCPU, 4 GB RAM)
- **Storage**: Logs retained 7 days

---

## 🔮 Future Architecture

### Planned Improvements

1. **Caching Layer**
   - Azure Cache for Redis
   - Cache search results (5 min TTL)
   - Cache conference/session data (1 hour TTL)

2. **Vector Search**
   - Azure AI Search integration
   - Hybrid search (keyword + semantic)
   - User preference embeddings

3. **Real-Time Updates**
   - Azure SignalR Service
   - Push session changes to connected clients
   - Live schedule updates

4. **Multi-Region**
   - Azure Front Door for global routing
   - Read replicas in EU/Asia
   - CDN for static assets

5. **GraphQL API**
   - Alternative to REST
   - Flexible querying
   - Reduce over-fetching

---

## 📚 Key Design Decisions

See [ARCHITECTURE-DECISIONS.md](https://github.com/fabianwilliams/ConferenceHaven/blob/main/ARCHITECTURE-DECISIONS.md) for detailed rationale (private repo).

### Why MCP?
- Universal protocol for AI integrations
- Supported by all major AI platforms
- Better than building separate ChatGPT Plugin, Claude API, Copilot connector

### Why FastAPI?
- Modern Python async/await
- Automatic OpenAPI generation
- High performance (comparable to Node.js)
- Rich type hints with Pydantic

### Why Azure SQL vs. MongoDB?
- Full-text search built-in
- Strong consistency for calendar/analytics
- SQL is familiar for most contributors
- Easier migration path to PostgreSQL

### Why AlwaysOnSampler?
- Need 100% trace capture for evaluation
- Cost acceptable at current volume (<100k requests/month)
- Plan to use TraceIdRatioBased(0.1) for high volume

### Why Aspire Dashboard?
- Free, lightweight observability
- Better than Azure Monitor for development
- Real-time logs/traces/metrics in one place
- Plan to migrate to Azure Monitor for production

---

## 🛠️ Development Environment

### Prerequisites
- Python 3.11+
- Node.js 18+ (for MCP remote client)
- Azure CLI (for deployment)
- Docker (for observability stack)

### Local Setup

```bash
# Clone repo (private - organizer access only)
git clone https://github.com/fabianwilliams/ConferenceHaven.git
cd ConferenceHaven

# Setup Python environment
python -m venv .venv
source .venv/bin/activate  # or `.venv\Scripts\activate` on Windows

# Install MCP server dependencies
cd mcp-server
pip install -e .

# Install backend agent dependencies
cd ../backend-agent
pip install -r requirements.txt

# Start observability stack
cd ..
docker compose -f docker-compose.observability.yml up -d

# Run database migrations
cd database
python run-migration.py --target 05-add-evaluation-tables

# Start MCP server
cd ../mcp-server
python -m src.main
```

Server runs at `http://localhost:8000/sse`

### Testing

```bash
# Test MCP connection
npx -y @fabianwilliams/mcp-remote http://localhost:8000

# Test REST API
curl http://localhost:8000/health
curl http://localhost:8000/api/conferences
```

---

## 📖 Additional Resources

- **Setup Guides**: [SETUP-GUIDES.md](SETUP-GUIDES.md)
- **FAQ**: [FAQ.md](FAQ.md)
- **Troubleshooting**: [TROUBLESHOOTING.md](TROUBLESHOOTING.md)
- **Main Repo**: [github.com/fabianwilliams/ConferenceHaven](https://github.com/fabianwilliams/ConferenceHaven) (private)

---

**Last updated**: November 9, 2025
