# System Architecture

## Overview

Plux is designed as a modular full-stack system consisting of a mobile client, backend services, and a content extraction pipeline.

The architecture separates content ingestion, enrichment, and itinerary management to maintain scalability and flexibility.

---

## Components

### Mobile Client
Handles user interaction, trip planning, and rendering structured itineraries.

### Backend API (Django)
Exposes REST endpoints for itinerary management, user operations, and pipeline coordination.

### Extraction & Enrichment Pipeline
Processes unstructured social content into structured location data using parsing, ranking, and external API enrichment.

### Database (PostgreSQL)
Stores users, trips, destinations, and itinerary relationships.

### External APIs
- Google Places API for location enrichment  
- LLM fallback (Groq) for ambiguous parsing cases  

---

## Design Considerations

- Separation of concerns between ingestion and user-facing services  
- Modular pipeline design for extensibility  
- External API usage for reliable geospatial data  