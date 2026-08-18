````md
# CrisisOS 

### AI-Powered Emergency Decision Support System

CrisisOS is an AI-powered emergency decision-support system designed to help disaster-response teams make faster, more informed decisions when roads, resources, and information are constantly changing.

Instead of simply showing where a disaster is happening, CrisisOS focuses on answering:

> **“What should we do next?”**

The system combines real-world road network data, disaster risk, emergency infrastructure, conflicting field reports, and AI-generated reasoning into a single interactive command interface.

---

## Problem

During floods, cyclones, earthquakes, wildfires, and other large-scale emergencies, response teams operate with:

- Rapidly changing road conditions
- Limited emergency resources
- Hospitals and shelters with finite capacity
- Incomplete field information
- Conflicting reports from different sources
- Constantly changing disaster conditions

The challenge is not simply collecting information.

The real challenge is **turning uncertain information into timely and coordinated decisions.**

---

## Our Solution

CrisisOS creates a dynamic representation of a small affected area and continuously evaluates the situation.

The MVP combines:

-  Real road networks from OpenStreetMap
-  Road-level disaster risk scoring
-  Road closure simulation
-  Hospital and shelter locations
-  Estimated population impact
-  Conflicting reports and confidence scoring
-  AI-generated explanations
-  Emergency route recalculation

The goal is to help emergency teams move from:

> **“What is happening?”**

to:

> **“What should we do next?”**

while keeping human decision-makers in control.

---

# MVP

The CrisisOS MVP is intentionally limited to **five core capabilities**.

### 1. Real Road Network

A small geographic area is selected and its real road network is obtained from **OpenStreetMap**.

Roads are represented as a graph containing:

- Intersections
- Road segments
- Connectivity
- Geographic coordinates

---

### 2. Road Risk Visualization

Every road receives a simple disaster-risk score.

Roads are visually represented as:

 **Low Risk**

 **Medium Risk**

 **High Risk**

The initial risk score can combine factors such as:

```text
Risk Score =
    Flood Probability
    + Elevation
    + Road Importance
````

The MVP intentionally uses a simple weighted model rather than attempting to build a complex disaster prediction model.

---

### 3. Road Closure Simulation

A response operator can select a road and simulate its closure.

The system then calculates the potential impact, including:

* People potentially affected
* Hospitals potentially isolated
* Emergency routes that need recalculation
* Approximate response-time change

Example:

```text
🚧 Road Closure Simulated

People affected:        8,200
Hospitals isolated:     2
Routes recalculated:    4
Response time change:   +6 min
```

The purpose is to demonstrate **decision impact**, not simply display a blocked road.

---

### 4. Conflicting Information & Confidence

Emergency information is not always reliable.

For example:

```text
Citizen Report:
"Road A is completely flooded."

Sensor:
"Water level is moderate."

Satellite:
"Possible flooding detected."
```

Instead of blindly trusting one source, CrisisOS combines factors such as:

* Source reliability
* Report recency
* Agreement between sources
* Available evidence

The system produces an estimated confidence score.

Example:

```text
Road A

Flood Status: LIKELY BLOCKED
Confidence: 78%

Evidence:
✓ Satellite observation
✓ Sensor reading
⚠ Conflicting citizen reports
```

---

### 5. AI Explanation

CrisisOS does not stop at producing a number.

The system can explain **why a situation matters**.

Example:

> **“Road A is prioritized because its closure could isolate two healthcare facilities and significantly increase emergency response time for the surrounding population.”**

This makes the system's recommendations easier for human operators to understand and evaluate.

---

# Core Demo Flow

The complete MVP follows this flow:

```text
OpenStreetMap
      ↓
Road Network
      ↓
Risk Scoring
      ↓
Interactive Map
      ↓
Select Road
      ↓
Simulate Road Closure
      ↓
Impact Analysis
      ↓
Route Recalculation
      ↓
Confidence / Conflicting Reports
      ↓
AI Explanation
```

---

# System Architecture

```text
                   ┌───────────────────┐
                   │  OpenStreetMap    │
                   └─────────┬─────────┘
                             ↓
                   ┌───────────────────┐
                   │ OSMnx / NetworkX  │
                   │   Road Graph      │
                   └─────────┬─────────┘
                             ↓
              ┌─────────────────────────────┐
              │      FastAPI Backend        │
              │                             │
              │  /graph                     │
              │  /route                     │
              │  /simulate                  │
              │  /confidence                │
              │  /explain                   │
              └───────┬──────────┬──────────┘
                      │          │
             ┌────────┘          └─────────┐
             ↓                              ↓
      ┌───────────────┐             ┌──────────────┐
      │ PostgreSQL +   │             │ AI / LLM     │
      │ PostGIS        │             │ Explanation  │
      └───────┬───────┘             └──────┬───────┘
              │                            │
              └────────────┬───────────────┘
                           ↓
                  ┌─────────────────┐
                  │ React + Leaflet │
                  │ Command Map     │
                  └─────────────────┘
```

---

# 🛠️ Technology Stack

## Frontend

* React
* JavaScript / TypeScript
* Leaflet
* Interactive map visualization

## Backend

* Python
* FastAPI
* NetworkX
* OSMnx

## Database

* PostgreSQL
* PostGIS

## AI

* LLM-based explanation
* Confidence / conflict resolution logic

## Data

* OpenStreetMap
* Simulated disaster reports
* Simulated sensor observations
* Simulated satellite observations

---

# Team Work Division

CrisisOS is developed using four feature branches.

| Branch                  | Responsibility                                              |
| ----------------------- | ----------------------------------------------------------- |
| `feature/frontend`      | Dashboard, interactive map, visualization and UX            |
| `feature/backend`       | FastAPI, graph operations, routing and simulation APIs      |
| `feature/ai-automation` | Confidence engine, AI explanations and scenario integration |
| `Dataengineer`          | Geospatial data, OSM processing, database and data pipeline |

The `main` branch is reserved for the **stable, tested version**.

---

# 3-Day Development Roadmap

## Day 1 — Foundation

### Backend / Graph

* Set up FastAPI
* Retrieve OSM road network
* Convert roads into NetworkX graph
* Implement `/graph`
* Implement basic `/route`

### Database / Geospatial

* Set up PostgreSQL + PostGIS
* Create core tables
* Seed hospitals and shelters
* Provide database access to backend

### Frontend

* Set up React + Leaflet
* Render real road network
* Add hospital and shelter markers

### AI / Integration

* Create disaster scenario data
* Create conflicting reports
* Define API contracts
* Ensure backend and frontend agree on request/response formats

### End-of-Day Goal

> Real roads render on the map and `/route` returns a real path.

---

## Day 2 — Intelligence

### Backend

* Add road-risk scoring
* Implement road closure simulation
* Calculate affected population
* Identify isolated hospitals
* Recalculate emergency routes

### Database

* Add risk scores
* Add event/time-series data
* Implement population-impact queries
* Optimize slow queries

### Frontend

* Display road risk using colors
* Add road information sidebar
* Add road closure simulation
* Display impact numbers
* Add **Ask Why** functionality

### AI

* Build confidence/conflict resolver
* Calculate confidence scores
* Integrate LLM explanation
* Expose explanation endpoint

### End-of-Day Goal

> Clicking a road → seeing risk → closing it → seeing impact → receiving an AI explanation.

---

## Day 3 — Integration & Demo

Day 3 is primarily for:

* Integration testing
* Bug fixing
* Performance improvements
* UI polish
* Disaster scenario playback
* Error handling
* Demo preparation

### Final Demo Flow

```text
NORMAL CITY
     ↓
START DISASTER
     ↓
Road risks change
     ↓
Conflicting information appears
     ↓
AI calculates confidence
     ↓
Operator selects critical road
     ↓
SIMULATE ROAD CLOSURE
     ↓
Impact is calculated
     ↓
Emergency route changes
     ↓
AI explains WHY
```

### End-of-Day Goal

> A judge should be able to click through the entire demo without a developer fixing anything during the presentation.

---

# Development Rules

### `main` is protected conceptually

Do not push development work directly to `main`.

Development happens on feature branches:

```text
feature/frontend
feature/backend
feature/ai-automation
Dataengineer
```

Changes reach `main` through Pull Requests after review and testing.

---

# MVP Constraints

To keep the project achievable within three days:

* Use a **small geographic area**
* Use real OSM road data
* Use approximate population values where necessary
* Keep risk scoring simple
* Use simulated reports for the conflicting-information demo
* Avoid unnecessary features
* Prioritize a working end-to-end demo over complexity

### We will NOT expand the scope until the five MVP capabilities work together live.

---

# Demo Success Criteria

CrisisOS is considered MVP-complete when the team can demonstrate:

* [x] Real roads from OpenStreetMap
* [x] Road risk visualization
* [x] Road closure simulation
* [x] Impact analysis
* [x] Route recalculation
* [x] Conflicting information
* [x] Confidence score
* [x] AI-generated explanation

---

# Vision

CrisisOS is designed around a simple idea:

> Emergency response systems should not only tell people what is happening. They should help decision-makers understand what to do next.

The MVP demonstrates this concept through a focused combination of geospatial intelligence, simulation, uncertainty handling, route optimization, and explainable AI.

---

## Project Status

**Currently in active development**

This repository contains the CrisisOS MVP being developed as a rapid prototype.

---

## License

To be decided.

```
```
