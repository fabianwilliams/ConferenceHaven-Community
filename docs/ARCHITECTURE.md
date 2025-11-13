
High-level technical overview of ConferenceHaven's dual-service architecture.

---

## 🏗️ System Architecture

ConferenceHaven is a cloud-native platform with **two main services**:
1. **MCP Server** - For AI tool integrations (Claude, ChatGPT, Copilot, etc.)
2. **Agent Framework Chat** - For web-based conversational AI chat

Both services share a common backend API and observability stack.

```
┌─────────────────────────────────────────────────────────────┐
│                     AI Client Layer                          │
│  (Claude Desktop, ChatGPT, LM Studio, Copilot, Web Browser) │
└────────────┬──────────────────────────────┬─────────────────┘
             │                              │
             │ MCP Protocol                 │ HTTPS/JSON
             │                              │
      ┌──────▼──────────┐           ┌──────▼──────────────┐
      │   MCP Server    │           │  Agent Chat Service │
      │  confhaven-mcp  │           │  (FastAPI + Agent   │
      │                 │           │   Framework)        │
      │ - Tool registry │           │ - Conversation UI   │
      │ - SSE transport │           │ - Session history   │
      │ - Observability │           │ - Smart prompts     │
      └────────┬────────┘           └─────────┬───────────┘
               │                              │
               │                              │
               └──────────┬───────────────────┘
                          │
                   ┌──────▼──────────┐
                   │  Backend Agent  │
                   │  (REST API)     │
                   │                 │
                   │ - Session search│
                   │ - SQL queries   │
                   │ - Data sync     │
                   └────────┬────────┘
                            │
        ┌───────────────────┼────────────────────┐
        │                   │                    │
   ┌────▼─────┐      ┌─────▼──────┐     ┌──────▼────────┐
   │  Azure   │      │  Microsoft │     │  OpenTelemetry│
   │   SQL    │      │   Graph    │     │    (OTLP)     │
   │ Database │      │    API     │     │               │
   │          │      │  (Calendar)│     │  Aspire       │
   │ Sessions │      │            │     │  Dashboard    │
   └──────────┘      └────────────┘     └───────────────┘
```

### Key Components

1. **MCP Server** (`mcp-server/`)
   - **Purpose**: Expose conference search as MCP tools for AI assistants
   - **Tech Stack**: Python FastAPI + Agent Framework
   - **Transport**: SSE (Server-Sent Events) for MCP protocol
   - **Deployment**: Azure Container Apps
   - **URL**: `https://mcp.conferencehaven.com`
   - **Tools**: search_sessions, get_session, list_conferences, send_calendar_invite, get_organizer_analytics

2. **Agent Framework Chat** (`agent-chat/`)
   - **Purpose**: Web-based conversational AI chat interface
   - **Tech Stack**: Python FastAPI + Microsoft Agent Framework + OpenAI
   - **Features**: 
     - Conversation history (per session)
     - Smart ambiguity detection (e.g., "Bobby Chang" → asks clarification)
     - Question teaching (guides users to better queries)
     - Calendar integration
   - **Deployment**: Azure Container Apps
   - **URL**: `https://conferencehaven.com/chat`
   - **Version**: v19-prompt-fix (deployed Nov 13, 2025)

3. **Backend Agent** (`backend-agent/`)
   - **Purpose**: REST API façade over Azure SQL
   - **Tech Stack**: Python FastAPI + SQLAlchemy ORM
   - **Features**:
     - Session search with full-text search
     - Conference management
     - Background data scraping
     - Analytics endpoints (organizer-only)
   - **Deployment**: Azure Container Apps
   - **URL**: Internal API (not publicly exposed)

4. **Database** (`database/`)
   - **Primary**: Azure SQL Database
   - **Search**: Full-text indexes on title, abstract, tags
   - **Tables**: Conferences, Sessions, EvaluationData, Organizers
   - **Migrations**: Python scripts in `database/` folder

5. **Observability Stack**
   - **Protocol**: OpenTelemetry (OTLP/gRPC)
   - **Collector**: Aspire Dashboard (Blazor app)
   - **Deployment**: Azure Container Instances
   - **URL**: `https://aspire.conferencehaven.com` (token-protected)
   - **Services Monitored**:
     - `conferencehaven-mcp` (blue traces)
     - `conferencehaven-agent-framework` (orange traces)
   - **Telemetry**: Logs, Traces (100% sampling), Metrics
   - **Events**: User queries, agent responses, tool calls

6. **Static Web** (`static-web/`)
   - **Purpose**: Landing page + documentation + chat UI
   - **Tech Stack**: HTML, CSS, JavaScript (React-based chat UI)
   - **Deployment**: Azure Static Web Apps
   - **URL**: `https://conferencehaven.com`
   - **Features**:
     - Landing page with setup guides
     - Chat interface (`/chat`) - connects to Agent Framework service
     - Stripe onboarding for organizers

---

## 🔧 Technology Stack

### Backend Services
- **Language**: Python 3.11+
- **Framework**: FastAPI (async/await)
- **MCP Implementation**: Microsoft Agent Framework (MCP server only)
- **AI Agent**: Microsoft Agent Framework + OpenAI GPT-4 (Agent Chat service)
  - **Core**: `agent-framework-core` (ChatAgent, OpenAIChatClient, tool registration)
  - **DevUI**: `agent-framework-devui` (debugging interface)
- **ORM**: SQLAlchemy 2.0
- **HTTP Client**: HTTPX (async)
- **Validation**: Pydantic v2

### Data Layer
- **Primary DB**: Azure SQL Database
- **Search**: Full-text search with `CONTAINS()` queries
- **Indexes**: Full-text on `title`, `abstract`, `tags`, `speaker_names`
- **Caching**: Planned - Azure Cache for Redis

### Observability
- **Protocol**: OpenTelemetry (OTLP/gRPC)
- **SDK**: opentelemetry-sdk 1.38.0
- **Exporter**: opentelemetry-exporter-otlp-proto-grpc
- **Dashboards**: 
  - **Aspire Dashboard**: Blazor + .NET 9 (telemetry for both services)
  - **Agent Framework DevUI**: Agent-specific debugging (conversation flows)
- **Sampling**: AlwaysOnSampler (100% trace capture)
- **Telemetry Types**:
  - **Logs**: HTTP requests, database queries, tool invocations
  - **Traces**: Request lifecycle, tool execution, database spans
  - **Metrics**: Request count, duration, error rates, tool usage
  - **Events**: User queries, agent responses (timeline markers)
- **Agent Framework Integration**: Native support for conversation history, tool tracing, state inspection

### Infrastructure
- **Hosting**: Azure Container Apps (both MCP server + Agent Chat)
- **CI/CD**: GitHub Actions (manual builds for now)
- **Registry**: Azure Container Registry (ACR)
- **DNS**: GoDaddy + Azure DNS
- **CDN**: Planned - Azure Front Door

### Integrations
- **AI Models**: OpenAI GPT-4 (via Azure OpenAI or OpenAI API)
- **Calendar**: Microsoft Graph API (send .ics invites via email)
- **Payments**: Stripe (organizer subscriptions)
- **Auth**: Azure Entra ID (organizers only)

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
- Full-text index on `title`, `abstract`, `tags`, `speaker_names`

**EvaluationData**
- `id` - Primary key
- `user_query` - Natural language query
- `tool_name` - Which tool was called
- `tool_parameters` - JSON parameters
- `response_json` - JSON response
- `trace_id` - OpenTelemetry trace ID (links to observability)
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
WHERE CONTAINS((title, abstract, tags, speaker_names), 'copilot OR "AI assistant"')
  AND conference_id = @conference_id
ORDER BY relevance DESC
```

### Ranking Algorithm

1. **Exact title match**: +10 points
2. **Title partial match**: +5 points
3. **Speaker name match**: +4 points
4. **Abstract match**: +2 points
5. **Tag match**: +3 points

### Ambiguity Detection (Agent Chat Only)

Agent Framework chat service includes smart query analysis:
- Detects single-word queries that might be ambiguous (e.g., "Chang")
- Asks clarifying questions: "Are you looking for sessions by speaker Bobby Chang, or sessions about 'Chang'?"
- Teaches users how to ask better questions
- Warns if search results look wrong (e.g., searching for "change" instead of "Chang")

### Future Enhancements
- Vector embeddings (OpenAI Ada-002)
- Semantic search with Azure AI Search
- User preference learning
- Session recommendations

---

## 🛠️ Service Details

### MCP Server

**Purpose**: Expose conference search as tools for AI assistants

**Tools Exposed**:

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

**Transport**: Server-Sent Events (SSE)
- Endpoint: `https://mcp.conferencehaven.com/sse`
- Keeps connection open for streaming
- Heartbeats every 30 seconds
- Reconnect on disconnect

**Direct REST API** (for non-MCP clients):
- Endpoint: `https://mcp.conferencehaven.com/api/*`
- OpenAPI/Swagger: `/api/swagger.json`
- Used by ChatGPT Actions, Copilot Studio

### Agent Framework Chat Service

**Purpose**: Web-based conversational AI with session history

**Features**:
- **Conversation History**: Each session_id maintains thread history
- **Smart Prompting**: Detects ambiguous queries and asks for clarification
- **Question Teaching**: Guides users to better search queries
- **Speaker Search Awareness**: Warns if results don't match intent
- **Calendar Integration**: Same as MCP server
- **Timeout Handling**: 60-second timeout with helpful error messages

**API Endpoints**:
- `POST /chat` - Send message, get response
  - Body: `{"message": "...", "session_id": "..."}`
  - Returns: `{"response": "...", "session_id": "..."}`
- `GET /health` - Health check
- `GET /` - Service info

**System Prompt** (v19):
```
You are a helpful conference session search assistant...

CRITICAL: When users provide just a NAME (e.g., "Bobby Chang"), 
ask for clarification:
"Are you looking for:
1. Sessions BY speaker Bobby Chang, or
2. Sessions ABOUT 'Bobby Chang'?"

Guide users to ask clear questions...
```

---

## 📈 Observability Architecture

### OpenTelemetry Configuration

**Both services** (MCP + Agent Chat) export telemetry to Aspire Dashboard:

**Exporter**: OTLP/gRPC
**Endpoint**: `http://aspire-confhaven.eastus.azurecontainer.io:4317`
**Sampling**: AlwaysOnSampler (100% trace capture)
**Flush**: Force flush after each request (ensures no trace loss)

### Telemetry Captured

**MCP Server Traces** (blue in Aspire):
- Span names: `mcp.tool.search_sessions`, `mcp.tool.get_session`, etc.
- Attributes:
  - `mcp.tool.name` - Tool called
  - `mcp.client.type` - electron_app / chatgpt / copilot / unknown
  - `mcp.search.query` - User's search term
  - `mcp.search.result_count` - Results returned
  - `mcp.tool.duration_ms` - Execution time
- Events:
  - `mcp.user.query` - Captures full user query
  - `mcp.tool.response` - Captures response preview (first 500 chars)

**Agent Chat Traces** (orange in Aspire):
- Span names: `agent.chat`, `agent-startup-test`
- Attributes:
  - `session_id` - Conversation session
  - `message` - User query (first 100 chars)
  - `response_length` - Character count
  - `error` - Error type if timeout/failure
- Events:
  - `agent.user.query` - Full user query
  - `agent.response` - Agent response preview (first 500 chars)

**Custom Attributes**:
- Service name: `conferencehaven-mcp` or `conferencehaven-agent-framework`
- Deployment environment: `production`
- Service version: `1.0.0`
- Telemetry SDK: `opentelemetry` (Python) version `1.38.0`

### Aspire Dashboard

**Access**: https://aspire.conferencehaven.com/login?t={TOKEN}
- Token is dynamic (regenerates on container restart)
- Fetch token: `bash get-aspire-token.sh` (in private repo)

**Views Available**:
- **Traces**: See all spans from both services side-by-side
- **Logs**: Console output from containers
- **Metrics**: Request counts, durations, error rates
- **Resources**: Service metadata

**Benefits**:
- Unified observability for both services
- Real-time debugging (no delayed ingestion)
- Event timeline (see query → response flow)
- Parent-child span relationships
- Full trace context propagation

---

## 🔐 Security Model

### Authentication

**Public Endpoints** (no auth required):
- All MCP tools: `search_sessions`, `get_session`, `list_conferences`, `send_calendar_invite`
- Agent chat: `/chat`, `/health`

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
- Use chat interface anonymously

### Data Protection

- **SQL Injection**: Parameterized queries only (SQLAlchemy ORM)
- **XSS**: Input sanitization in chat responses
- **Rate Limiting**: Planned - 100 requests/minute per IP
- **DDoS**: Planned - Azure Front Door WAF
- **Secrets**: Azure Key Vault + Container App secrets
- **Encryption**: TLS in transit (HTTPS on all public endpoints), at-rest (Azure SQL)

---

## 🚀 Deployment Architecture

### Environments

**Production**:
- MCP Server: `https://mcp.conferencehaven.com`
- Agent Chat: `https://conferencehaven.com/chat`
- Backend API: Internal only (not exposed publicly)
- Aspire Dashboard: `https://aspire.conferencehaven.com` (token-protected)
- Static Web: `https://conferencehaven.com`

**Development**:
- Local Docker Compose for observability stack
- Local Python virtual environments
- SQLite or local Azure SQL
- Local Aspire Dashboard: `docker compose -f docker-compose.observability.yml up`

### CI/CD Pipeline

Currently **manual builds** via Azure CLI:

```
Developer workstation
  ↓
az acr build --registry acrconfhavenlean --image <service>:<version>
  ↓
Push to Azure Container Registry
  ↓
az containerapp update --image <service>:<version>
  ↓
Deploy to Azure Container Apps
  ↓
Health Check (/health endpoint)
  ↓
Route Traffic (100% to new revision)
```

**Planned**: GitHub Actions automation

### Scaling

**MCP Server** (Azure Container Apps):
- **Min replicas**: 1
- **Max replicas**: 3
- **Scale trigger**: CPU > 70% or Request Queue > 20
- **Cold start**: ~2 seconds

**Agent Chat** (Azure Container Apps):
- **Min replicas**: 1
- **Max replicas**: 3
- **Scale trigger**: CPU > 70%
- **Cold start**: ~3 seconds (OpenAI client initialization)

**Backend Agent** (Azure Container Apps):
- **Min replicas**: 1
- **Max replicas**: 5
- **Scale trigger**: HTTP request queue
- **Cold start**: ~2 seconds

**Database** (Azure SQL):
- **Current**: Basic tier (5 DTUs)
- **Plan**: Upgrade to Standard S2 (50 DTUs) for production load
- **Connections**: Max 100 concurrent (connection pooling enabled)

**Aspire Dashboard** (Azure Container Instance):
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
   - Auto-instrumentation of Azure AI Search calls in observability

3. **Real-Time Updates**
   - Azure SignalR Service
   - Push session changes to connected chat clients
   - Live schedule updates

4. **Multi-Region**
   - Azure Front Door for global routing
   - Read replicas in EU/Asia
   - CDN for static assets

5. **GraphQL API**
   - Alternative to REST
   - Flexible querying
   - Reduce over-fetching

6. **Auto-Instrumentation**
   - OpenAI SDK auto-instrumentation (`opentelemetry-instrumentation-openai`)
   - HTTPX auto-instrumentation (`opentelemetry-instrumentation-httpx`)
   - GenAI semantic conventions for LLM traces

7. **Azure Monitor Integration**
   - Long-term trace retention (Aspire is ephemeral)
   - Alerting and dashboards
   - Cost management

---

## 📚 Key Design Decisions

See [ARCHITECTURE-DECISIONS.md](https://github.com/fabianwilliams/ConferenceHaven/blob/main/ARCHITECTURE-DECISIONS.md) for detailed rationale (private repo).

### Why Two Services (MCP + Agent Chat)?

- **MCP Server**: Optimized for AI tool integrations (Claude Desktop, ChatGPT Actions, etc.)
  - Lightweight, stateless
  - Strict tool contracts
  - No conversation history needed
- **Agent Chat**: Optimized for web-based conversations
  - Session-based history
  - Smart prompting and teaching
  - Better error handling for end users
- **Shared**: Both use same backend API (DRY principle)

### Why Agent Framework?

- Official Microsoft AI framework
- Native OpenTelemetry support (with manual instrumentation)
- Built-in conversation history management
- DevUI for debugging (planned)
- Better than Semantic Kernel for agentic patterns

### Why FastAPI?

- Modern Python async/await
- Automatic OpenAPI generation (critical for ChatGPT/Copilot Studio)
- High performance (comparable to Node.js)
- Rich type hints with Pydantic
- Excellent observability hooks

### Why Azure SQL vs. MongoDB?

- Full-text search built-in (no external search service needed)
- Strong consistency for calendar/analytics
- SQL is familiar for most contributors
- Easier migration path to PostgreSQL if needed

### Why 100% Trace Sampling?

- Need complete data for AI evaluation (LLM judge)
- Current volume (<10k requests/month) makes cost acceptable
- Plan to use TraceIdRatioBased(0.1) at high volume

### Why Aspire Dashboard vs. Azure Monitor?

- **Aspire**: Free, lightweight, real-time, perfect for development
- **Azure Monitor**: Production-grade, long-term retention, alerting
- **Plan**: Keep both - Aspire for debugging, Azure Monitor for production analytics

---

## 🛠️ Development Environment

### Prerequisites
- Python 3.11+
- Node.js 18+ (for MCP remote client)
- Azure CLI (for deployment)
- Docker (for observability stack)

### Local Setup

```bash
# Clone private repo (organizer/contributor access only)
git clone https://github.com/fabianwilliams/ConferenceHaven.git
cd ConferenceHaven

# Setup Python environment
python -m venv .venv
source .venv/bin/activate  # or `.venv\Scripts\activate` on Windows

# Install MCP server dependencies
cd mcp-server
pip install -e .

# Install Agent Chat dependencies
cd ../agent-chat
pip install -r requirements.txt

# Install backend agent dependencies
cd ../backend-agent
pip install -r requirements.txt

# Start observability stack (Aspire Dashboard)
cd ..
docker compose -f docker-compose.observability.yml up -d

# Run database migrations
cd database
python run-migration.py --target 05-add-evaluation-tables

# Start MCP server
cd ../mcp-server
python -m src.main  # Runs on http://localhost:8000/sse

# OR start Agent Chat (different terminal)
cd ../agent-chat
python main.py  # Runs on http://localhost:8000
```

### Testing

```bash
# Test MCP connection (requires Node.js)
npx -y mcp-remote http://localhost:8000

# Test Agent Chat
curl http://localhost:8000/health
curl -X POST http://localhost:8000/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "What conferences are available?", "session_id": "test-123"}'

# Test REST API (backend-agent)
curl http://localhost:8001/api/conferences
curl "http://localhost:8001/api/sessions/search?query=copilot&limit=5"

# View observability
open http://localhost:18888  # Aspire Dashboard
```

---

## 📖 Additional Resources

- **Setup Guides**: [SETUP-GUIDES.md](SETUP-GUIDES.md)
- **FAQ**: [FAQ.md](FAQ.md)
- **Troubleshooting**: [TROUBLESHOOTING.md](TROUBLESHOOTING.md)
- **Main Repo**: [github.com/fabianwilliams/ConferenceHaven](https://github.com/fabianwilliams/ConferenceHaven) (private)
- **Community Repo**: [github.com/fabianwilliams/ConferenceHaven-Community](https://github.com/fabianwilliams/ConferenceHaven-Community) (public)

---

**Last updated**: November 13, 2025
**Architecture Version**: 2.0 (dual-service)
**MCP Server**: v1.0 (stable)
**Agent Chat**: v19-prompt-fix (latest)
```

---

## 2. SETUP-GUIDES.md - MINOR UPDATES

Add this section after the "🎯 Choose Your Setup Method" section:

```markdown
---

### 🌐 Web Chat Interface (Zero Setup!)
