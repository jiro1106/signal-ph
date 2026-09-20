# Signal PH

Signal PH is a cellular coverage intelligence platform for the Philippines. It helps people understand signal quality at a location or along a route, compare providers, identify weak-signal areas, and choose a better SIM for a trip.

![Signal PH landing page](docs/screenshots/signal_1.png)

![Signal PH coverage forecast dashboard](docs/screenshots/signal_2.png)

## Features

- Route coverage forecasting between two locations
- Point-based signal analysis for a selected location
- Provider comparison for Globe, Smart, and DITO
- Recommended provider/SIM based on nearby tower evidence
- Weak-signal and dead-zone detection along routes
- Interactive map layers for routes, towers, coverage radii, and signal gaps
- Crowdsourced signal reports and nearby report summaries
- Anomaly detection when computed coverage conflicts with community reports
- Offline-readiness guidance for trips through weak-signal areas
- AI chat assistant for questions about coverage, routes, and SIM choices
- MCP tools for reusable point analysis, route analysis, and report submission

## How it works

1. A user selects a location or enters an origin and destination.
2. The frontend requests a point or route analysis from the FastAPI backend.
3. The backend resolves route geometry with OSRM when route points are not supplied.
4. Candidate cell towers and nearby community reports are loaded from Supabase/PostgreSQL.
5. Each route point is matched to nearby towers using geographic distance and estimated tower range.
6. Provider scores are calculated from tower proximity, coverage range, radio technology, tower samples, and report adjustments.
7. The result identifies weak or dead segments, ranks providers, and returns map-ready data.
8. The frontend renders the route forecast, provider scoreboard, signal gaps, and recommendations.
9. The chatbot can use the same backend analysis tools, then synthesize a user-facing answer with a local LFM model.

## Architecture

```
User
  ↓
React + Vite frontend
  ├─ Interactive map and route selection
  ├─ Provider scoreboard and trip summary
  └─ AI chat orchestrator
       ↓
FastAPI backend
  ├─ Route and point analysis
  ├─ Tower matching and scoring
  ├─ Crowdsourced reports and anomaly logging
  └─ MCP tool endpoint
       ↓
Supabase PostgreSQL + external services
  ├─ Cell tower and report data
  ├─ OSRM route geometry
  ├─ OpenCellID tower data
  └─ Optional Google Maps services
```

## Tech stack

### Frontend

- React 19
- TypeScript
- Vite
- Tailwind CSS
- React Router
- Leaflet, React Leaflet, and Leaflet Routing Machine
- Deck.gl map layers
- Framer Motion
- Wllama for browser-local LFM inference

### Backend

- Python 3.13+
- FastAPI
- Uvicorn
- Pydantic and Pydantic Settings
- Supabase PostgreSQL
- LangChain and LangGraph
- MCP server tooling
- Requests and Google Maps client libraries
- JWT/cryptography utilities and rate limiting

### Data and integrations

- OpenCellID tower data
- Supabase for cell tower, report, and anomaly storage
- OSRM for route geometry
- Optional Google Maps Platform services
- Optional OpenAI-compatible local LFM endpoint

## Project structure

```
.
├── backend/
│   ├── agents/          # Analysis and recommendation agents
│   ├── db/              # Supabase/PostgreSQL repositories
│   ├── mcp_server/      # MCP tools and resources
│   ├── routes/          # FastAPI API routes
│   ├── services/        # Routing, scoring, caching, and recommendations
│   └── main.py          # FastAPI application entry point
└── frontend/
    ├── src/components/  # Maps, chatbot, navigation, and UI
    ├── src/orchestration/ # Chat routing and tool execution
    ├── src/pages/       # Landing, login, and reporting pages
    ├── src/services/    # Backend API clients
    └── src/main.tsx     # Frontend entry point
```

## Getting started

### Prerequisites

- Python 3.13+
- Node.js and npm
- Supabase project or compatible PostgreSQL database
- Optional: OpenCellID and Google Maps API keys
- Optional: local LFM server, unless using the browser model

### Backend

```bash
cd backend
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Create `backend/.env` with the required database settings. The backend defaults to port `8001`.

```bash
python -m backend.main
```

API documentation:

- http://localhost:8001/docs
- http://localhost:8001/redoc
- http://localhost:8001/health

### Frontend

```bash
cd frontend
npm install
npm run dev
```

The Vite development server normally runs at http://localhost:5173.

Useful frontend environment variables:

```env
VITE_API_URL=http://localhost:8001
VITE_SIGNALPH_API_BASE_URL=http://localhost:8001
VITE_LFM_MODE=browser
VITE_LFM_MODEL_URL=/model/LFM2.5-350M.i1-Q6_K.gguf
```

For a server-hosted LFM, set `VITE_LFM_MODE=server`, `VITE_LFM_BASE_URL`, and `VITE_LFM_MODEL_NAME`.

## API and MCP tools

The backend exposes standard health and analysis endpoints, including:

- `GET /`
- `GET /health`
- `GET /info`
- `POST /analyze/point`
- `POST /analyze/route`

The MCP endpoint supports:

- `analyze_point`
- `analyze_route`
- `submit_signal_report`

## Current status

The route forecast, map visualization, provider comparison, backend analysis pipeline, and chatbot flow are implemented. The `/report` frontend route is currently a placeholder while the backend report capability is available through the API/MCP layer.

## License

No license has been specified yet.
