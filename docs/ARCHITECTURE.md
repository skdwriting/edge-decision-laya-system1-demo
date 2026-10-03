# 🏗️ System Architecture

> Technical architecture documentation for the Edge AI Decision System.

---

## High-Level Architecture

```
┌─────────────────────────┐         HTTP POST           ┌─────────────────────────────┐
│   📱 Mobile Phone       │  ─────── /api/decide ──────▶ │   💻 Laptop Edge Server     │
│   (Edge Entity)         │                              │                             │
│                         │  ◀──── JSON response ──────  │   FastAPI + Uvicorn         │
│  • Scenario selector    │       (~30-50ms)             │                             │
│  • Sensor event presets │                              │   ┌─────────────────────┐   │
│  • Decision display     │                              │   │  📐 Math Agent      │   │
│  • Probability bars     │                              │   │  Threshold analysis  │   │
│  • Telemetry log        │                              │   └────────┬────────────┘   │
└─────────────────────────┘                              │            ▼                │
                                                         │   ┌─────────────────────┐   │
        Same Wi-Fi Network                               │   │  🧠 Laya System 1   │   │
                                                         │   │  421M · <30ms       │   │
┌─────────────────────────┐  WebSocket /ws/live           │   └────────┬────────────┘   │
│   💻 Laptop Dashboard   │ ◀────────────────────────────│            ▼                │
│   (Real-time feed)      │                              │   ┌─────────────────────┐   │
│                         │                              │   │  🛡️ Guardrail       │   │
│  • Live decision feed   │                              │   │  Safety verification │   │
│  • Stats & metrics      │                              │   └─────────────────────┘   │
│  • Architecture diagram │                              │                             │
│  • Mobile connect URL   │                              │  Dashboard: /               │
└─────────────────────────┘                              │  Mobile UI: /mobile         │
                                                         └─────────────────────────────┘
```

---

## Decision Pipeline

The `/api/decide` endpoint processes each request through three layers:

### Layer 1: Math Agent

The `MathAgent` class performs **symbolic numerical analysis** on the sensor text:

- **Threshold comparison**: Extracts `value` and `threshold` from text, computes percentage deviation
- **Percentage anomaly detection**: Identifies "X% above/below" patterns
- **Surge/drop detection**: Catches sudden increases or decreases
- **Kinematic analysis**: Speed drop calculations (e.g., 80 km/h to 0)

Example:
```
Input:  "Vibration at 12mm/s, threshold is 8mm/s"
Output: "Mathematical comparison: vibration (12mm/s) > threshold (8mm/s) (+50.0% above safe limit)"
```

### Layer 2: Laya System 1 Model

The Laya model provides **non-autoregressive structured inference**:

- **Single forward pass** — all decision dimensions computed simultaneously
- **Three question types**:
  - `choice` — Select from predefined options (e.g., "stop machine", "continue")
  - `score` — Ordinal rating (e.g., "low", "moderate", "severe")
  - `noul` — Yes/No probability (e.g., "Is this an emergency?")
- **Calibrated probabilities** — confidence scores are mathematically calibrated

### Layer 3: Neuro-Symbolic Guardrail

The `NeuroSymbolicGuardrail` class provides **deterministic safety verification**:

- Pattern-matches against known critical scenarios per domain
- **Overrides** neural model output when safety rules are violated
- Adjusts probability distributions to reflect guardrailed certainty
- Provides human-readable reasoning for every override

---

## Scenario Domains

| Domain | Decision Dimensions | Example Actions |
|--------|-------------------|-----------------|
| 🏠 Smart Home | action, urgency, is_anomaly | turn off lights, activate alarm, send notification |
| 🏭 Industrial | action, risk_level, needs_human_review | emergency shutdown, stop machine, reduce speed |
| 🏥 Health | triage, severity, is_emergency | emergency, alert doctor, monitor |
| 🔒 Security | threat_type, threat_level, requires_lockdown | unauthorized access, perimeter breach, suspicious activity |
| 🚦 Traffic | recommendation, congestion, is_accident | emergency response, signal adjustment, divert traffic |

---

## Communication Flow

```
1. Phone → Server:  POST /api/decide
   {
     "device_id": "phone-a1b2",
     "scenario": "industrial",
     "state_text": "Motor vibration at 12mm/s, threshold is 8mm/s",
     "metadata": {}
   }

2. Server processes: Math Agent → Laya Model → Guardrail

3. Server → Phone:  JSON Response
   {
     "request_id": "abc12345",
     "decisions": {
       "action": { "label": "stop machine", "confidence": 0.98, "probabilities": {...} },
       "risk_level": { "label": "severe", "confidence": 0.96 },
       "needs_human_review": { "label": "Yes", "value": 0.98 }
     },
     "reasoning": "Machinery protection: vibration exceeds safety limit by +50.0%",
     "math_analysis": "Mathematical comparison: vibration (12mm/s) > threshold (8mm/s) (+50.0%)",
     "latency_ms": 34.2
   }

4. Server → Dashboard:  WebSocket broadcast (same payload)
```

---

## Technology Stack

| Component | Technology | Role |
|-----------|-----------|------|
| Edge Server | FastAPI + Uvicorn | HTTP/WebSocket server |
| AI Model | Laya (421M params) | Non-autoregressive System 1 decisions |
| Math Engine | Regex + Python | Numerical threshold analysis |
| Safety Layer | Pattern matching | Deterministic guardrails |
| Dashboard | Vanilla HTML/CSS/JS | Real-time WebSocket UI |
| Mobile Client | Vanilla HTML/CSS/JS | Sensor event input UI |
| Communication | HTTP REST + WebSocket | Edge entity ↔ Server ↔ Dashboard |
