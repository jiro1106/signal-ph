# Signal PH

Signal PH is a cellular coverage intelligence platform for the Philippines. It forecasts coverage along routes, compares providers, highlights weak-signal areas, and recommends a better SIM for a trip.

![Signal PH landing page](docs/screenshots/signal_1.png)

![Signal PH coverage forecast dashboard](docs/screenshots/signal_2.png)

## Features

- Route and point coverage analysis
- Globe, Smart, and DITO comparison
- SIM/provider recommendations
- Weak-signal and dead-zone detection
- Interactive tower, route, and coverage maps
- Crowdsourced reports, anomaly detection, and offline-readiness guidance
- AI chat assistant powered by the same analysis tools

## How it works

1. The user selects a place or route.
2. The frontend sends the request to FastAPI.
3. The backend loads route geometry, nearby towers, and community reports.
4. Tower matches are scored by distance, range, radio type, samples, and reports.
5. The frontend displays coverage, gaps, provider rankings, and recommendations.

## Architecture

```
React + Vite frontend
  → FastAPI + MCP analysis backend
  → Supabase/PostgreSQL tower and report data
  → OSRM, OpenCellID, and optional Google Maps services
```

## Tech stack

| Area | Technologies |
| --- | --- |
| Frontend | React 19, TypeScript, Vite, Tailwind CSS, React Router |
| Maps | Leaflet, React Leaflet, Leaflet Routing Machine, Deck.gl |
| AI | Wllama, local LFM model, LangChain, LangGraph |
| Backend | Python 3.13+, FastAPI, Uvicorn, Pydantic |
| Data | Supabase PostgreSQL, OpenCellID, crowdsourced reports |
| Integrations | MCP, OSRM, optional Google Maps Platform |
| Security | JWT, cryptography, rate limiting |

## Project structure

## Project structure