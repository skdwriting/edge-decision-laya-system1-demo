# ⚡ Edge Decision System — Laya System 1 Demo

> Real-time edge AI decisions using the Laya open-source System 1 model + Mathematical Agent + Neuro-Symbolic Safety Guardrails.

[![Python 3.10+](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![Laya](https://img.shields.io/badge/Laya-System%201-667eea)](https://github.com/convai-research/laya)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue)](LICENSE)

---

## 🎯 What This Is

A live demo system where:
- A **laptop** acts as an **Edge Server** running the Laya AI model
- A **phone** acts as an **Edge Entity** sending sensor data
- Decisions are made in **30–50ms** with no cloud dependency

The system demonstrates **neuro-symbolic edge AI** — combining a neural System 1 model with mathematical analysis and deterministic safety guardrails for reliable, ultra-low-latency decision-making.

---

## 📐 Architecture

```
┌──────────────────────┐        HTTP POST         ┌────────────────────────────┐
│  📱 Mobile Phone     │ ────── /api/decide ────▶ │  💻 Laptop Edge Server     │
│  (Edge Entity)       │                          │                            │
│                      │ ◀──── JSON (~30ms) ────  │  ┌──────────────────────┐  │
│  Sensor events       │                          │  │ 📐 Math Agent        │  │
│  Scenario picker     │                          │  │ 🧠 Laya System 1    │  │
│  Decision display    │                          │  │ 🛡️ Safety Guardrails │  │
└──────────────────────┘                          │  └──────────────────────┘  │
                                                  │                            │
       Same Wi-Fi Network                         │  Dashboard: /              │
                                                  │  Mobile UI: /mobile        │
                                                  └────────────────────────────┘
```

---

## 🚀 Quick Start

### 1. Clone & Setup

```powershell
git clone https://github.com/skdwriting/edge-decision-laya-system1-demo.git
cd edge-decision-laya-system1-demo/edge-server
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

### 2. Start the Server

```powershell
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

### 3. Open the Dashboard

```
http://localhost:8000/
```

### 4. Connect Your Phone

Open this URL in your phone's browser (same Wi-Fi):
```
http://<your-laptop-ip>:8000/mobile
```

> 💡 The dashboard shows the exact URL in the **"Connect Phone"** panel.

---

## 📁 Project Structure

```
edge-decision-laya-system1-demo/
│
├── README.md                    # ← You are here
├── LICENSE                      # Apache 2.0
├── .gitignore                   # Python, venv, IDE exclusions
│
├── edge-server/                 # 💻 Edge Server (runs on laptop)
│   ├── main.py                  # FastAPI server + Laya + MathAgent + Guardrails
│   ├── requirements.txt         # Python dependencies
│   ├── README.md                # Server component documentation
│   └── static/
│       ├── dashboard.html       # Real-time laptop dashboard (WebSocket)
│       └── mobile.html          # Mobile UI (served at /mobile)
│
├── edge-mobile/                 # 📱 Mobile Client (standalone reference)
│   ├── index.html               # Standalone mobile edge entity UI
│   └── README.md                # Mobile component documentation
│
└── docs/                        # 📚 Documentation
    ├── SETUP.md                 # Full installation & setup guide
    ├── DEMO_GUIDE.md            # Live demo script & walkthrough
    └── ARCHITECTURE.md          # Technical architecture documentation
```

---

## 🎯 Demo Scenarios

| Scenario | Example Event | AI Decision |
|----------|--------------|-------------|
| 🏠 **Smart Home** | Motion at 2 AM, residents asleep | Action, Urgency, Anomaly detection |
| 🏭 **Industrial** | Motor vibration above threshold | Action, Risk level, Human review needed |
| 🏥 **Health** | Heart rate 145 bpm at rest | Triage, Severity, Emergency flag |
| 🔒 **Security** | Unknown person at server room | Threat type, Threat level, Lockdown |
| 🚦 **Traffic** | Sudden speed drop across 3 lanes | Recommendation, Congestion, Accident |

Each scenario has **6 preset events** (30 total) ready for instant testing.

---

## 🔑 Key Technical Points

| Feature | Description |
|---------|-------------|
| **System 1 Model** | Laya makes instant, intuitive decisions — like a human reflex — not step-by-step reasoning |
| **Non-Autoregressive** | All decisions produced in a single forward pass (~30ms), not token-by-token like LLMs |
| **Edge-Native** | 421M parameters — runs on laptop CPU, no GPU required |
| **Math Agent** | Extracts numbers from text, computes threshold deviations, flags anomalies |
| **Neuro-Symbolic** | Neural model + deterministic safety rules = verified decisions |
| **Calibrated Confidence** | 90% confidence means correct ~90% of the time |
| **Real-Time Dashboard** | WebSocket feed shows decisions as they happen |

---

## 📚 Documentation

| Document | Description |
|----------|-------------|
| [Setup Guide](docs/SETUP.md) | Complete installation and configuration instructions |
| [Demo Guide](docs/DEMO_GUIDE.md) | Step-by-step live demo script with talking points |
| [Architecture](docs/ARCHITECTURE.md) | Technical architecture and decision pipeline details |
| [Edge Server](edge-server/README.md) | Server component documentation |
| [Edge Mobile](edge-mobile/README.md) | Mobile client documentation |

---

## 🔧 Troubleshooting

| Issue | Solution |
|-------|----------|
| Phone can't reach server | Ensure both on same Wi-Fi; check firewall allows port 8000 |
| `laya` import error | Run `pip install laya` again; needs Python 3.10+ |
| Model download stalls | Check internet; ~800MB download on first run |
| Port 8000 in use | Use `--port 8080` and update phone URL accordingly |
| Slow inference | First inference is slower (model warmup); subsequent calls are faster |

---

## 📜 License

This demo is for educational purposes

[Laya](https://github.com/NandhaKishorM/laya) model is licensed under **Apache 2.0** by Convai Innovations.
