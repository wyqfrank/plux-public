# Content Extraction & Enrichment Pipeline

## Overview

The pipeline transforms unstructured social media travel content into structured locations that can be used for itinerary generation.

Rather than relying entirely on an LLM, the system uses a staged approach that combines heuristic parsing with model fallback for ambiguous cases. This was designed to improve reliability, reduce cost, and keep the output more controllable.

![Content extraction pipeline](../diagrams/content-pipeline.png)

---

## Goals

- Extract candidate destinations from noisy social media content
- Resolve candidate locations to real-world places
- Enrich destinations with structured metadata
- Produce consistent outputs for downstream itinerary generation

---

## Pipeline Stages

### 1. Scraping

The first stage collects raw content and metadata from the source input.

Typical inputs include:
- captions
- hashtags
- embedded location text
- post metadata

This stage is intentionally lightweight and focuses on capturing as much potentially useful signal as possible before filtering.

---

### 2. Parsing

The parser converts raw text into candidate travel entities.

This includes:
- identifying possible place names
- extracting supporting context
- normalising noisy text into cleaner candidate strings

At this stage, the output is broad and may still include false positives.

---

### 3. Ranking

Candidate locations are scored based on confidence and relevance.

Ranking primarily uses heuristic signals such as:
- explicit location mentions
- repeated place references
- travel-related context

For ambiguous cases, an LLM fallback is used to improve ranking quality and disambiguation.

This hybrid approach was chosen because:
- heuristics are cheap and predictable
- LLMs are more flexible for edge cases
- relying entirely on an LLM would increase cost and reduce control

---

### 4. Resolution

High-confidence candidates are mapped to real-world places.

This stage uses:
- fuzzy matching
- deduplication
- external API lookups

The goal is to convert candidate strings into structured place entities that can be persisted and used downstream.

---

### 5. Enrichment

Resolved locations are enriched with additional metadata from external services such as Google Places.

Example enriched attributes include:
- coordinates
- place categories
- ratings
- business metadata

This allows the output of the pipeline to be both human-useful and application-ready.

---

## Example Output

A successful pipeline run produces a structured destination object that can be used by the itinerary system, for example:

```json
{
  "name": "Bondi Icebergs",
  "city": "Sydney",
  "country": "Australia",
  "coordinates": {
    "lat": -33.8915,
    "lng": 151.2767
  },
  "category": "restaurant"
}