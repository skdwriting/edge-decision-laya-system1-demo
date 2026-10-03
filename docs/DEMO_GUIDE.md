# 🎯 IEEE Theory Conclave — Live Demo Guide

> Step-by-step guide for presenting the Edge AI Decision System demo at the IEEE Theory Conclave.

---

## 🕐 Before the Demo (10 minutes before)

### 1. Pre-flight Checklist

- [ ] Laptop is connected to Wi-Fi (or hotspot is ready)
- [ ] Phone is connected to the **same network**
- [ ] Python virtual environment is set up and dependencies installed
- [ ] Laya model has been downloaded (first run downloads ~800MB)
- [ ] Browser tabs ready on laptop (dashboard at `http://localhost:8000/`)

### 2. Start the Edge Server

```powershell
cd edge-server
.\venv\Scripts\Activate.ps1
uvicorn main:app --host 0.0.0.0 --port 8000
```

Wait for:
```
[Edge Server] Laya model ready in X.XXs
INFO:     Uvicorn running on http://0.0.0.0:8000
```

### 3. Open the Dashboard

Open `http://localhost:8000/` in your laptop browser.  
You should see the **Edge AI Decision Server** dashboard with stats, architecture diagram, and a live feed panel.

### 4. Find Your IP Address

```powershell
ipconfig
```
Look for **"Wireless LAN adapter Wi-Fi" → "IPv4 Address"**  
(e.g., `192.168.1.42`)

The dashboard also displays the mobile URL in the **"Connect Phone"** panel.

---

## 🎤 Demo Flow (Recommended Script)

### Act 1: Setting the Stage (1–2 min)

**Say:**
> _"Today I'll show you an edge AI decision system that runs entirely on this laptop — no cloud.  
> It uses the **Laya open-source System 1 model** — a 421 million parameter non-autoregressive model  
> combined with a **Mathematical Agent** and **Neuro-Symbolic Safety Guardrails**.  
> My phone acts as an edge entity — like a sensor node — sending data to this laptop."_

Point to the **architecture diagram** on the dashboard (right panel).

### Act 2: Connect the Phone (30 sec)

1. Show the **mobile URL** from the dashboard's "Connect Phone" panel
2. Open it on your phone: `http://<your-ip>:8000/mobile`
3. Show the phone screen to the audience — they should see the **Edge Entity** interface

### Act 3: Live Decisions (3–5 min)

Run through scenarios in this order for maximum impact:

#### Scenario 1: 🔒 Security (High Drama)
1. Select **Security** on the phone
2. Tap: _"Unknown person detected at server room door at 11 PM, no badge scan"_
3. Tap **⚡ Transmit to Edge Server**
4. **Show on phone:** UNAUTHORIZED ACCESS decision with critical threat level
5. **Show on laptop:** Same decision appears instantly on the dashboard via WebSocket

**Say:**
> _"The system instantly classified this as an unauthorized access attempt with 98% confidence.  
> Notice the reasoning — it detected this as a high-security facility breach outside business hours.  
> The inference latency was just 30–50 milliseconds. No cloud round-trip."_

#### Scenario 2: 🏭 Industrial (Math Agent Showcase)
1. Select **Industrial**
2. Tap: _"Conveyor belt motor vibration at 12mm/s, threshold is 8mm/s"_
3. Transmit

**Say:**
> _"Now notice the **Math Agent** — it extracted the numbers 12 and 8, computed a 50% deviation  
> above the safety threshold, and flagged this as severe. The Laya model confirmed: stop the machine.  
> This is neuro-symbolic AI — the neural model and mathematical rules work together."_

#### Scenario 3: 🏥 Health (Life-Critical)
1. Select **Health**
2. Tap: _"Blood oxygen level dropped to 88%, patient is 72 years old"_
3. Transmit

**Say:**
> _"SpO2 below 90% in an elderly patient — the system triages this as a critical emergency  
> with 99% confidence. In real edge healthcare, this decision needs to happen in milliseconds,  
> not seconds. That's why we use a System 1 model, not an LLM."_

#### Scenario 4: 🚦 Traffic (Real-Time Infrastructure)
1. Select **Traffic**
2. Tap: _"Wrong-way driver detected on highway exit ramp"_
3. Transmit

**Say:**
> _"Emergency response dispatched instantly. In traffic systems, every millisecond counts.  
> This is running on a laptop CPU — no GPU needed — making it deployable on actual edge hardware."_

### Act 4: Key Takeaways (1 min)

**Say:**
> _"Three key technical points:_
>
> 1. **System 1 vs System 2** — Laya is a System 1 model. It doesn't reason step-by-step like ChatGPT.  
>    It produces all decisions in a single forward pass — like a human reflex.
>
> 2. **Non-autoregressive** — Unlike GPT which generates one token at a time,  
>    Laya produces structured decisions simultaneously. That's why it's 30ms, not 3 seconds.
>
> 3. **Neuro-symbolic** — We pair the neural model with a Mathematical Agent and deterministic  
>    Safety Guardrails. The math agent does the numerical analysis; the guardrail ensures the  
>    neural model's decision is verified before acting on it."_

---

## 🆘 Emergency Fallbacks

| Problem | Quick Fix |
|---------|-----------|
| Phone can't connect | Create a mobile hotspot from laptop, connect phone to it |
| Server crashes | Restart: `uvicorn main:app --host 0.0.0.0 --port 8000` |
| Model not loading | The demo still works — guardrails handle decisions even without the neural model |
| Dashboard not updating | Refresh the browser page (WebSocket reconnects automatically) |
| Slow first response | First inference is always slower (model warmup). Do a test run before the demo. |

---

## 📊 What to Show on Screen

| Screen | What to Display |
|--------|----------------|
| **Laptop** | Dashboard at `http://localhost:8000/` — live feed shows decisions as they arrive |
| **Phone** | Mobile UI at `http://<ip>:8000/mobile` — shows scenario picker, presets, and decision response |
| **Projector** | Connect laptop to projector showing the dashboard |

---

## 🎯 Audience Q&A Prep

| Question | Answer |
|----------|--------|
| _"How is this different from ChatGPT?"_ | Laya is a System 1 model — it classifies in one pass, not token-by-token. It's 100x faster and designed for decisions, not conversations. |
| _"Can it run on a Raspberry Pi?"_ | At 421M parameters, it needs ~2GB RAM. It can run on Pi 5 or any edge device with enough memory. |
| _"What about accuracy?"_ | The confidence scores are calibrated — 90% confidence means it's correct ~90% of the time. Plus the guardrails catch edge cases. |
| _"Is Laya open source?"_ | Yes — Apache 2.0 license by Convai Innovations. |
| _"Why not just use rules?"_ | Pure rules can't handle the variety of natural language inputs. The neural model generalizes; the rules verify. Best of both worlds. |
