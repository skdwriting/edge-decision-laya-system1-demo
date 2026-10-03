# 💻 Edge Server

> **FastAPI edge server** running the Laya System 1 model + Mathematical Agent + Neuro-Symbolic Guardrails for ultra-low-latency structured decisions.

---

## Overview

The edge server runs on a laptop and processes sensor events from mobile edge entities.  
It uses a three-layer neuro-symbolic architecture:

1. **Math Agent** — Extracts numerical values, computes threshold deviations, percentage changes
2. **Laya System 1 Model** — 421M parameter non-autoregressive model for instant routing & classification
3. **Neuro-Symbolic Guardrail** — Deterministic safety rules that verify and override neural decisions

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Laptop dashboard (real-time WebSocket feed) |
| `/mobile` | GET | Mobile edge entity UI |
| `/api/decide` | POST | Core decision endpoint |
| `/api/health` | GET | Health check |
| `/api/history` | GET | Recent decision history |
| `/ws/live` | WS | WebSocket for real-time dashboard updates |

## Quick Start

```powershell
# Navigate to the edge server directory
cd edge-server

# Create virtual environment
python -m venv venv

# Activate it
.\venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt

# Start the server
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

## Dependencies

| Package | Purpose |
|---------|---------|
| `fastapi` | Web framework for the edge server |
| `uvicorn` | ASGI server to run FastAPI |
| `laya` | Laya System 1 decision model (421M params) |
| `pydantic` | Data validation |
| `jinja2` | Template rendering |
| `python-multipart` | Form data handling |

## Project Structure

```
edge-server/
├── main.py              # FastAPI server + Laya + MathAgent + Guardrails
├── requirements.txt     # Python dependencies
├── README.md            # This file
└── static/
    ├── dashboard.html   # Laptop dashboard (real-time WebSocket UI)
    └── mobile.html      # Mobile edge entity UI (served at /mobile)
```

## Decision Architecture

```
  Sensor Event (text)
       │
       ▼
  ┌─────────────┐
  │  Math Agent  │  ← Extract numbers, compute deviations
  └──────┬──────┘
         ▼
  ┌─────────────┐
  │  Laya Model │  ← System 1 inference (~30ms)
  └──────┬──────┘
         ▼
  ┌─────────────┐
  │  Guardrail  │  ← Symbolic safety rules verify/override
  └──────┬──────┘
         ▼
  Structured Decision (JSON)
```
