# Plux — AI-Powered Social Travel Planning Platform

An iOS app that turned social media travel posts into trip itineraries. You'd share a TikTok or Instagram post, and Plux would extract the locations, enrich them with real data (Google Places), and let you build a shareable itinerary from it.

Built as a startup idea — validated with a 200+ user survey and demoed a working TestFlight MVP to 50 users.
Shelving it to work on another problem I'm more interested in.

This repo documents the architecture and technical decisions behind the project.

## Demo

<div align="center">
  <a href="https://youtube.com/shorts/5A_hpj0dwu4">
    <img src="https://img.youtube.com/vi/5A_hpj0dwu4/0.jpg" alt="Plux Demo" width="300">
  </a>
  <p>Click to watch the demo</p>
</div>

## How It Worked

```
React Native App (Expo)
│
▼
REST API (Django)
│
├── Content extraction — pull locations from social media posts
├── Location enrichment — coordinates, ratings, categories via Google Places
├── Itinerary builder — organise destinations into trip plans
├── Collaboration — shared trips between users
▼
PostgreSQL
```

## Tech Stack

- **Frontend:** React Native / Expo
- **Backend:** Django, REST APIs
- **Database:** PostgreSQL
- **APIs:** Google Places, Groq

## Project Structure

```
plux-public/
├── diagrams/                    # Architecture diagrams
├── docs/
│   ├── ai-pipeline.md           # How the data pipeline works
│   ├── system-architecture.md   # System design overview
│   └── lessons-learned.md       # What I'd do differently
├── screenshots/                 # Product UI
└── README.md
```

## What I Took Away From It

- Preprocessing was more effective than prompt tuning for this use case — social media posts follow predictable patterns, so simple heuristics handled most cases, with the LLM used for the remaining ambiguity (also reducing inference cost)
- Multi-step pipelines over single model calls — unstructured content needed cleaning and normalisation before it was useful for model inference
- Consumer mobile apps are heavily distribution-driven — product quality alone wasn’t enough without growth loops

## Code
The source code is private. This repo is focused on the design and architecture docs.