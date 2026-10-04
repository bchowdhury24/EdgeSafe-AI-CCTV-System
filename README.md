# AI-Based Video Surveillance Platform

**Case study** · Architect (ongoing)
`Python` `OpenCV` `YOLO` `InsightFace` `[streaming: TBD]` `AWS` `[GPU infra: TBD]`

---

## Overview

An ongoing platform for real-time video analytics across large surveillance
deployments: monitoring thousands of CCTV feeds simultaneously on cloud
infrastructure, with teams able to access and review analytics from any
location. Built for businesses, government organizations, and smart-city
initiatives — the goal is faster response time, better security, and
operational efficiency at a scale humans alone can't watch.

> Active project — code private. Public case study of the architecture.

## The problem

Traditional CCTV is *recording*, not *surveillance*. A wall of monitors
watched by humans catches almost nothing:

- A human operator reliably monitors ~[9–16] feeds. A city has thousands.
- Forensic review ("find the person in the red jacket from Tuesday") takes
  days of manual scrubbing.
- By the time an incident is noticed from footage, the response window
  has closed.

The actual problem: turn passive video into **searchable, alerting
infrastructure**.

## Architecture

```
┌────────────┐ ┌────────────┐ ┌────────────┐
│  CCTV 01   │ │  CCTV 02   │ │  CCTV N    │   (thousands of feeds)
└─────┬──────┘ └─────┬──────┘ └─────┬──────┘
      │              │              │
      ▼              ▼              ▼
┌─────────────────────────────────────────────┐
│  Ingest & decode workers (distributed)      │
│  stream selection · quality adaptation      │
└──────────────────┬──────────────────────────┘
                   │
      ┌────────────┼────────────┐
      ▼            ▼            ▼
┌───────────┐┌───────────┐┌──────────────┐
│ Detection ││ Tracking  ││ Face /       │
│ (YOLO)    ││ (cross-   ││ identity     │
│           ││  frame)   ││ (InsightFace)│
└─────┬─────┘└─────┬─────┘└──────┬───────┘
      └────────────┼────────────┘
                   ▼
      ┌────────────────────────────┐
      │  Event & metadata store    │
      │  (searchable incidents)    │
      └────────────┬───────────────┘
                   │
      ┌────────────▼───────────────┐
      │  Alerting & review UI      │
      │  (teams, any location)     │
      └────────────────────────────┘
```

## Key decisions

**1. Metadata, not video, is the product.**
Raw video is petabytes; events are kilobytes. The platform's core value is
converting streams into structured, searchable events — detections, tracks,
identities, timestamps — so review is a query, not a marathon.

**2. Model cascade for cost control.**
Running heavy models on every frame of every feed is unaffordable at
thousands of cameras. A lightweight detection/filter stage gates the
expensive models (fine-grained detection, recognition) so GPU budget is
spent only where something is actually happening.

**3. Cloud access without cloud dependency for alerts.**
Edge-vs-cloud tradeoffs shaped the design: latency-critical alerting works
local-first, while the searchable archive and multi-team review live in the
cloud.

## Results

- [Feeds per deployment: TBD]
- [Detection/alert latency achieved]
- [Deployments: businesses / government / smart-city pilots]

## Status

Ongoing — actively building. [Add a short note on what's next: e.g.,
"Currently expanding multi-camera tracking across sites."]

---

*Code private. Questions on video analytics architecture welcome: [email]*
