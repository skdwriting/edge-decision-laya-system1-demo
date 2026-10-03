# 📱 Edge Mobile Client

> **Mobile edge entity** that sends sensor events to the Edge AI Decision Server and displays structured decisions.

---

## Overview

This is the mobile client (edge entity) component of the Edge Decision System.  
It runs in any mobile browser and sends sensor event data to the edge server's `/api/decide` endpoint.

## How It Works

1. Open `index.html` in your phone's browser (served by the edge server at `/mobile`)
2. Select a **scenario domain** (Smart Home, Industrial, Health, Security, Traffic)
3. Tap a **preset event** or type a custom sensor reading
4. Tap **⚡ Transmit to Edge Server**
5. View the AI decision response with:
   - Prominent action banner
   - Confidence scores
   - Decision reasoning
   - Math analysis (if applicable)
   - Full probability distributions

## Accessing the Mobile UI

### Option A: Served by Edge Server (Recommended)
The edge server automatically serves this UI at `/mobile`:
```
http://<laptop-ip>:8000/mobile
```

### Option B: Standalone
Open `index.html` directly in a browser and enter the edge server URL manually.

## Features

- 📡 **5 Domain Scenarios** with 6 preset events each (30 total)
- ⚡ **Real-time edge inference** with latency display
- 📐 **Math analysis** for numerical comparisons
- 💡 **Decision reasoning** with detailed explanations
- 📊 **Probability distributions** for each decision dimension
- 📋 **Telemetry log** tracking all edge interactions
- 🔗 **Auto-detect server** when served by edge server
