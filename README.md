<p align="center">
  <img src="Assets/Banner-devnovate.png" alt="Devnovate Hackathon Banner" width="100%">
</p>

---

<p align="center">
  <img src="[https://img.shields.io/badge/Next.js-Client%20%26%20API-000000?style=for-the-badge&logo=nextdotjs&logoColor=white](https://img.shields.io/badge/Next.js-Client%20%26%20API-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)">
  <img src="[https://img.shields.io/badge/Groq-Fast%20Inference-F55036?style=for-the-badge](https://img.shields.io/badge/Groq-Fast%20Inference-F55036?style=for-the-badge)">
  <img src="[https://img.shields.io/badge/Vectorize-Hindsight%20Memory-0052FF?style=for-the-badge](https://img.shields.io/badge/Vectorize-Hindsight%20Memory-0052FF?style=for-the-badge)">
  <img src="[https://img.shields.io/badge/Node.js-v18%2B-339933?style=for-the-badge&logo=nodedotjs&logoColor=white](https://img.shields.io/badge/Node.js-v18%2B-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)">
</p>

<h1 align="center">Foresight: Persistent Relationship Intelligence AI Assistant</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white">
  <img src="https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black">
  <img src="https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white">
  <img src="https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white">
  <img src="https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white">
  <img src="https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white">
  <img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white">
</p>

---

## 📑 Table of Contents

- [Architecture Overview](#architecture-overview)
- [Memory Retain and Recall Lifecycle](#memory-retain-and-recall-lifecycle)
- [Core Capabilities](#core-capabilities)
- [Setup and Installation](#setup-and-installation)
- [Error Handling Architecture](#error-handling-architecture)
- [Running the Automated Test Suite](#running-the-automated-test-suite)
- [Design System: Claymorphism](#design-system-claymorphism)

---

# Foresight: Persistent Relationship Intelligence

> An executive meeting preparation system powered by **Vectorize Hindsight** persistent memory banks and **Groq** high-speed inference. Never walk into a meeting having forgotten a past commitment, unresolved objection, or shifting stakeholder sentiment.

---

## Architecture Overview

Foresight sits on top of two core backbones:
1. **Vectorize Hindsight (`@vectorize-io/hindsight-client`)**: Acts as the agent's persistent hippocampus, storing structured facts, temporal metadata, entity relationships, and synthesis patterns across discussions.
2. **Groq (`groq-sdk`)**: Provides low-latency reasoning and synthesis using open-weight models (`openai/gpt-oss-120b`).

```mermaid
flowchart LR
    subgraph Client["Next.js Claymorphism Client (Browser)"]
        direction TB
        UI_Home["Landing & Roster (/app)"]
        UI_Overview["Portfolio Risk Overview (/overview)"]
        UI_Curve["Longitudinal Learning Curve"]
        UI_Briefing["Pre-Meeting Briefing & Compare"]
        UI_Debrief["Post-Meeting Debrief Modal"]
    end

    subgraph API["Next.js App Router Backend (/api)"]
        direction TB
        Route_Memory["/api/memory"]
        Route_Overview["/api/overview"]
        Route_Curve["/api/learning-curve"]
        Route_Briefing["/api/briefing"]
        Route_Debrief["/api/debrief"]
    end

    subgraph Services["Orchestration Layer"]
        direction TB
        Insights_Orch["Insights Service<br/>(risk ranking & learning curve)"]
        Briefing_Orch["Briefing Orchestrator<br/>(recall + reflect + synthesis<br/>+ provenance linking)"]
        Cache[("Briefing Cache<br/>(contactId + agenda hash)")]
        Debrief_Orch["Debrief Orchestrator<br/>(extract promises + retain)"]
    end

    subgraph Hindsight["Hindsight Memory Platform (Vectorize)"]
        direction TB
        HS_Recall["recall():<br/>Semantic & Temporal Queries"]
        HS_Reflect["reflect():<br/>Outcome Synthesis & Tactics"]
        HS_Retain["retain():<br/>Notes, Promises, Debriefs, Tags"]
        HS_Bank[("Memory Bank: 'Script'")]
    end

    subgraph Groq["Groq Cloud Inference"]
        Groq_LLM["Model: gpt-oss-120b<br/>(Briefing Synthesis & Promise Extraction)"]
    end

    UI_Home -->|"GET"| Route_Memory
    UI_Overview -->|"GET"| Route_Overview
    UI_Curve -->|"GET"| Route_Curve
    UI_Briefing -->|"POST"| Route_Briefing
    UI_Debrief -->|"POST"| Route_Debrief

    Route_Memory --> Insights_Orch
    Route_Overview --> Insights_Orch
    Route_Curve --> Insights_Orch
    Route_Briefing --> Briefing_Orch
    Route_Debrief --> Debrief_Orch

    Briefing_Orch <--> Cache
    Insights_Orch --> HS_Recall
    Briefing_Orch -->|"1. recall()"| HS_Recall
    Briefing_Orch -->|"2. reflect()"| HS_Reflect
    Debrief_Orch -->|"2. retain()"| HS_Retain
    Briefing_Orch <-->|"3. Synthesize"| Groq_LLM
    Debrief_Orch <-->|"1. Extract"| Groq_LLM

    HS_Recall --> HS_Bank
    HS_Reflect --> HS_Bank
    HS_Retain --> HS_Bank
```

---

## Memory Retain and Recall Lifecycle

### 1. Retain Flow (After Every Meeting)
```mermaid
sequenceDiagram
    autonumber
    actor User as Account Executive
    participant App as Debrief UI Modal
    participant API as /api/debrief
    participant Svc as Debrief Orchestrator
    participant Groq as Groq (gpt-oss-120b)
    participant HS as Hindsight Bank ('Script')
    participant Cache as Briefing Cache

    User->>App: Paste raw meeting notes & select outcome (advanced/stalled/lost_ground)
    App->>API: POST notes, date, outcome, contactId
    API->>API: Validate payload (400 if contactId or notes missing)
    API->>Svc: Hand off validated debrief
    Svc->>Groq: Extract explicit promises (ours & theirs) and due dates
    Groq-->>Svc: Extracted commitments JSON

    par Retain in parallel
        Svc->>HS: retain() raw debrief note with tags: [contact, outcome, type:debrief]
    and
        Svc->>HS: retain() individual commitments with tags: [type:promise, outcome]
    end

    HS-->>Svc: Stored document IDs
    Svc->>Cache: Invalidate cached briefings for contact
    Svc-->>API: Stored confirmation summary
    API-->>App: Return stored confirmation summary
    App-->>User: Display stored memory facts and extracted promises

    alt Groq or Hindsight failure
        Svc-->>API: {service, statusCode, error}
        API-->>App: Error payload
        App-->>User: Inline error in modal, notes preserved, retry available
    end
```

### 2. Recall Flow (Before Upcoming Meetings)
```mermaid
sequenceDiagram
    autonumber
    actor User as Account Executive
    participant App as Briefing UI (/app)
    participant API as /api/briefing
    participant Svc as Briefing Orchestrator
    participant HS as Hindsight Bank ('Script')
    participant Groq as Groq (gpt-oss-120b)
    participant Prov as Provenance Linker

    User->>App: Enter upcoming agenda, goal, and click "Generate Briefing"
    App->>API: POST contactId, upcomingAgenda, useMemory=true
    API->>API: Validate payload (400 missing contactId, 404 unknown contact)
    API->>Svc: Start briefing request

    par Parallel memory retrieval
        Svc->>HS: recall() past commitments, deadlines, objections, and milestones
    and
        Svc->>HS: reflect() tactics that led to advanced vs stalled meetings
    end

    HS-->>Svc: Recalled Memory Facts (occurredStart, docId) + Tactical Patterns
    Svc->>Groq: Prompt with contact metadata + top 10 memory facts + reflect synthesis
    Groq-->>Svc: Structured briefing JSON
    Svc->>Prov: Map items to exact meeting dates and documentIds
    Prov-->>Svc: Items with sourceDate and sourceDocumentId
    Svc-->>API: Grounded briefing
    API-->>App: Full briefing with clickable provenance tags
    App-->>User: Interactive claymorphic briefing ready for executive review

    alt Groq rate limit (429)
        Svc-->>App: Terracotta alert: wait 10 seconds or Try Without Memory
        App->>API: POST useMemory=false (lightweight prompt)
    else Hindsight failure (502)
        Svc-->>App: Soft brick alert: verify HINDSIGHT_API_KEY
    end
```

---

## Core Capabilities

1. **Portfolio Risk Overview (`/overview`)**:
   - Ranks all 4 active enterprise accounts by risk (`#1 Elena Rostova` Critical, `#2 Amara Chen` High, `#3 Devon Miller` Moderate, `#4 Marcus Vance` Low/Expanding).
   - Displays real-time relationship status, count of documented broken promises, and next recommended actions with target deadlines.
   - Clicking any row opens that contact's pre-meeting briefing directly.

2. **Longitudinal Learning Curve (`/app?tab=learning-curve`)**:
   - Demonstrates how relationship intelligence compounds across 3 historical memory thresholds for the same upcoming agenda:
     - **Stage 1 (Before 1st meeting)**: 0 promises, 0 risks, 0 open questions (cold baseline).
     - **Stage 2 (Before 4th meeting)**: 2 promises, 1 risk, 2 questions (early technical traction).
     - **Stage 3 (Before 7th meeting)**: 5 promises, 3 risks, 3 questions (crisis, Confluent fallback threat, MSA liability redlines).
   - Pre-generated cache for instant (<5ms) demo loading.

3. **Side-by-Side Memory Contrast**:
   - One-click side-by-side briefing comparison showing the difference between memory-augmented intelligence and generic zero-memory responses.

4. **Provenance Tracking**:
   - Every promise, unresolved question, and sensitivity links back to the source meeting note in the timeline via small clickable labels (`Source: Aug 14 meeting`).

5. **Post-Meeting Debrief & Reflection**:
   - Ingests unstructured meeting notes, extracts promises using Groq, retains memories tagged with outcomes (`advanced`, `stalled`, `lost_ground`), and queries Hindsight `reflect()` to guide future tactics.

---

## Setup and Installation

### 1. Prerequisites
- **Node.js**: `v18.0.0` or higher (`v24.x` recommended)
- **npm**: `v9.x` or higher
- **Hindsight API Key & Bank**: Obtainable from [vectorize.io](https://hindsight.vectorize.io/)
- **Groq API Key**: Obtainable from [console.groq.com](https://console.groq.com/)

### 2. Clone and Install Dependencies
```bash
git clone <repository-url>
cd foresight
npm install
```

### 3. Configure Environment Variables
Copy the example environment file:
```bash
cp .env.local.example .env.local
```

Edit `.env.local` with your credentials:
```env
# Vectorize Hindsight API Configuration
HINDSIGHT_ENDPOINT=https://api.hindsight.vectorize.io
HINDSIGHT_API_KEY=your_hindsight_api_key_here
HINDSIGHT_BANK_ID=Script

# Groq Cloud API Configuration
GROQ_API_KEY=your_groq_api_key_here
GROQ_MODEL=openai/gpt-oss-120b
```

> **Security Note:** `.env.local` is explicitly ignored in `.gitignore` and must never be committed to source control.

### 4. Run the Development Server
```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

- **Landing Page**: [http://localhost:3000](http://localhost:3000)
- **Portfolio Risk Overview**: [http://localhost:3000/overview?demo=true](http://localhost:3000/overview?demo=true)
- **Main Workspace**: [http://localhost:3000/app?demo=true](http://localhost:3000/app?demo=true)

---

## Error Handling Architecture

Foresight includes end-to-end graceful degradation and structured error handling for external dependencies:

| Failure Mode | Detection | Backend Handling | UI Message & Action |
| :--- | :--- | :--- | :--- |
| **Groq TPM/RPM Rate Limit (429)** | Catches 429 / `rate_limit_exceeded` | Returns `{ service: 'groq', statusCode: 429, error: '...' }` | Renders a terracotta alert banner advising the user to wait 10 seconds or click **"Try Without Memory (Lightweight Prompt)"** to bypass token limits. |
| **Groq Service Outage / Invalid Key** | Catches 5xx or auth failures | Returns `{ service: 'groq', statusCode: 502, error: '...' }` | Clear error banner identifying Groq LLM failure with diagnostic logs and retry button. |
| **Hindsight Bank Connection Failure** | Catches recall rejection or network timeout | Returns `{ service: 'hindsight', statusCode: 502, error: '...' }` | Soft brick alert banner identifying Hindsight Memory Bank failure, recommending verification of `HINDSIGHT_API_KEY`. |
| **Post-Meeting Debrief Failure** | Catches extraction or retention errors | Returns `{ service: 'hindsight' \| 'groq', ... }` | Inline error box inside the Debrief modal allowing the user to edit notes or retry without losing inputs. |

---

## Running the Automated Test Suite

The project includes unit and integration tests covering the briefing route with and without memory, parameter validation, and error states using Node's native test runner:

```bash
npm test
```

### Test Coverage Breakdown
- `POST /api/briefing` missing `contactId` &rarr; 400 Bad Request
- `POST /api/briefing` non-existent contact &rarr; 404 Not Found
- `POST /api/briefing` **Without Memory** (`useMemory: false`) &rarr; 200 OK, zero recalled memories, intentional fallback placeholders displayed for sentiment and commitments
- `POST /api/briefing` **With Memory** (`useMemory: true`) &rarr; 200 OK, verified Hindsight facts recalled, commitments extracted with provenance metadata (`sourceDate`, `sourceDocumentId`)

---

## Design System: Claymorphism

The application adheres to a restrained enterprise **Claymorphism** aesthetic:
- **Palette**: Warm sand background (`#e6dfd3`), molded clay surfaces (`#eee8dc`), terracotta accent (`#b8623f`), muted sage (`#7d9080`), and near-black warm body text (`#2a2622`).
- **Surfaces**: 20px to 28px border radii, multi-layered warm drop shadows with inner top-left highlights and bottom-right shades.
- **Typography**: Source Serif 4 for headings, Source Sans 3 for body.
- **Integrity**: Strict zero-emoji, zero-em-dash policy; hand-crafted inline SVG icons.
