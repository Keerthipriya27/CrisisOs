Nirnay — AI Emergency Command System

Nirnay (निर्णय) — Sanskrit/Hindi for "decision."

Existing tools tell you what's happening. Nirnay tells you what to do next — and explains why, even when the information disagrees.

Nirnay is an AI-powered emergency decision-support system that turns fragmented, uncertain, real-time disaster information into clear, explainable, coordinated response decisions — while keeping human commanders in control.

🚨 The Problem

During disasters (floods, cyclones, earthquakes, wildfires), response teams must decide fast with information that is:

Incomplete — not everything is reported.
Conflicting — a citizen says a road is flooded, a sensor says it's fine.
Rapidly changing — a road that was open 10 minutes ago may now be closed.

The hard part isn't collecting data — it's turning uncertain, fragmented information into timely, coordinated decisions.

💡 Our Solution

Nirnay builds a live digital picture of an affected area — roads, hospitals, shelters, resources, and incoming field reports — and continuously:

Assesses uncertainty in incoming reports instead of blindly trusting any single source.
Identifies high-priority situations (which roads, zones, or hospitals matter most right now).
Recommends resource allocation and emergency routes.
Explains its reasoning in plain language, so commanders can trust and verify every recommendation.
Simulates "what if" scenarios — e.g., "what if this road closes?" or "what if this hospital reaches capacity?" — instantly, before it happens.
🎯 Target Users

Disaster management agencies, emergency operations centers, search-and-rescue teams, humanitarian organizations, and healthcare networks — adaptable across countries, disaster types, and infrastructure environments.

✨ Key Features (MVP)
Feature	What it does
Live Risk Map	Real road network (via OpenStreetMap) colored green/yellow/red by live risk score
Confidence Scoring	When sources disagree about a road's status, shows a confidence % instead of a false-certain answer
"What If" Simulation	Click any road → simulate closure → instantly see affected population, isolated hospitals, and new optimal routes
AI Explanations	Every recommendation comes with a plain-language "why" — powered by an LLM reading the system's structured decision data
Live Disaster Playback	A scripted event stream shows the map reacting in real time, as if live reports were arriving
🏗️ Architecture
        ┌─────────────────────┐        ┌──────────────────────┐
        │  OpenStreetMap/OSMnx │        │  Simulated Live Feed  │
        │  (road network)       │        │  (citizen reports,    │
        └──────────┬───────────┘        │  sensors, satellite)  │
                   │                    └───────────┬───────────┘
                   ▼                                ▼
        ┌────────────────────────────────────────────────┐
        │              DATABASE (PostGIS)                 │
        │  roads | nodes | hospitals | shelters | reports │
        │  resources | risk_scores | events (time-series) │
        └───────────────────┬──────────────────────────────┘
                            ▼
        ┌────────────────────────────────────────────────┐
        │                BACKEND (FastAPI)                │
        │  • Graph builder (OSMnx → networkx)              │
        │  • Risk scoring engine                          │
        │  • Confidence / conflict resolver                │
        │  • Route optimizer (Dijkstra / A*)               │
        │  • Simulation engine                             │
        └───────────────────┬──────────────────────────────┘
                            ▼
        ┌────────────────────────────────────────────────┐
        │        AI EXPLANATION LAYER (Claude API)         │
        └───────────────────┬──────────────────────────────┘
                            ▼
        ┌────────────────────────────────────────────────┐
        │        FRONTEND (React + Leaflet)                │
        │  Live map · confidence panel · simulate button   │
        │  · resource dashboard · "ask why" explanation     │
        └────────────────────────────────────────────────┘
🛠️ Tech Stack
Layer	Technology
Road data	OpenStreetMap + OSMnx + NetworkX
Database	PostgreSQL + PostGIS
Backend	FastAPI (Python)
Realtime	WebSockets
Routing	Dijkstra / A* (NetworkX)
Frontend	React + Leaflet
AI Explanations	Anthropic Claude API
Hosting (demo)	Localhost / ngrok, or Vercel (frontend) + Render/Railway (backend)
📁 Project Structure
nirnay/
├── backend/
│   ├── app/
│   │   ├── main.py            # FastAPI app entrypoint
│   │   ├── graph_builder.py   # OSMnx → NetworkX graph
│   │   ├── risk_scoring.py    # per-road risk calculation
│   │   ├── simulation.py      # "what if" closure logic
│   │   ├── confidence.py      # conflict/confidence resolver
│   │   └── explain.py         # LLM explanation calls
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   ├── components/        # Map, Sidebar, SimulatePanel, ExplainBox
│   │   └── App.jsx
│   └── package.json
├── database/
│   ├── schema.sql
│   └── seed_data.sql
├── data/
│   └── scenario_events.json   # simulated disaster event timeline
├── docs/
│   └── Nirnay_Roadmap.md
└── README.md
🔌 API Endpoints
Endpoint	Method	Purpose
/graph	GET	Returns road network + risk scores as GeoJSON
/route	GET	Returns optimal route between two points
/simulate/close_road/{id}	POST	Simulates closing a road, returns impact
/confidence/{road_id}	GET	Returns confidence score for a road's status
/explain	POST	Returns an AI-generated plain-language explanation for a decision
🚀 Getting Started
Prerequisites
Python 3.10+
Node.js 18+
PostgreSQL 14+ with PostGIS extension
Backend
bash
cd backend
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload
Frontend
bash
cd frontend
npm install
npm run dev
Database
bash
createdb nirnay
psql nirnay -c "CREATE EXTENSION postgis;"
psql nirnay -f database/schema.sql
psql nirnay -f database/seed_data.sql
🎬 Demo Script
Show the calm city map — green roads, hospitals, shelters visible.
Click "Start Disaster" — simulated reports stream in, roads shift color.
Click a road with conflicting reports → confidence score appears.
Click "Simulate Road Closure" on a critical road → impact numbers appear instantly.
Click "Ask why" → AI explains the reasoning in plain language.
Close with the pitch: explainable decisions under uncertainty, not just routing.
🗺️ Roadmap

See docs/Nirnay_Roadmap.md for the full 3-day, member-wise build plan.

👥 Team
Role	Owns
Backend / Graph Engineer	Routing, risk scoring, simulation engine
Database / Geospatial Engineer	PostGIS schema, seed data, geo queries
Frontend / UX Engineer	Map UI, dashboards, interactions
AI / Integration Lead	Confidence scoring, LLM explanations, glue between layers
📌 Category

Social Impact / AI-ML — an AI reasoning system built for humanitarian disaster response.

⚠️ Note on Data

Live sensor/satellite/citizen-report feeds are simulated via a scripted event timeline for demo purposes. The reasoning pipeline (confidence scoring, risk scoring, simulation, explanation) operates on real logic and real OpenStreetMap road data.
