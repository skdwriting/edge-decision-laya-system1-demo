# 🛠️ Setup & Installation Guide

> Complete setup guide for running the Edge AI Decision System on Windows.

---

## Prerequisites

- **Python 3.10+** installed on your laptop
- **Git** (optional, for cloning the repository)
- A modern web browser (Chrome, Edge, or Firefox)
- A phone with a web browser (Chrome on Android or Safari on iOS)
- Both laptop and phone connected to the **same Wi-Fi network**

---

## Step 1: Clone the Repository

```powershell
git clone https://github.com/skdwriting/edge-decision-laya-system1-demo.git
cd edge-decision-laya-system1-demo
```

## Step 2: Create a Python Virtual Environment

```powershell
cd edge-server
python -m venv venv
```

## Step 3: Activate the Virtual Environment

```powershell
.\venv\Scripts\Activate.ps1
```

> **If you get an execution policy error**, run this first:
> ```powershell
> Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
> ```

## Step 4: Install Dependencies

```powershell
pip install -r requirements.txt
```

This installs:

| Package | Version | Purpose |
|---------|---------|---------|
| `fastapi` | 0.115.6 | Async web framework for the edge server |
| `uvicorn` | 0.34.0 | ASGI server to run FastAPI |
| `laya` | latest | Laya System 1 decision model (421M params) |
| `pydantic` | 2.10.4 | Data validation and serialization |
| `jinja2` | 3.1.5 | Template rendering |
| `python-multipart` | 0.0.20 | Form data handling |

> ⏳ **First run**: The Laya library will download model checkpoints (~800MB). This only happens once.

## Step 5: Find Your Laptop's IP Address

```powershell
ipconfig
```

Look for **"Wireless LAN adapter Wi-Fi"** → **"IPv4 Address"**.  
Example: `192.168.1.42`

## Step 6: Start the Edge Server

```powershell
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

You should see:
```
[Edge Server] Loading Laya System 1 model...
[Edge Server] Laya model ready in X.XXs
INFO:     Uvicorn running on http://0.0.0.0:8000
```

> **`--host 0.0.0.0`** makes the server accessible from your phone over the network.

---

## Step 7: Verify the Server

### Health Check
Open in your laptop browser:
```
http://localhost:8000/api/health
```

Expected response:
```json
{
    "status": "ok",
    "model_loaded": true,
    "uptime": 1.23,
    "history_size": 0,
    "clients_connected": 0
}
```

### Dashboard
```
http://localhost:8000/
```

### Mobile UI
On your phone's browser:
```
http://<your-laptop-ip>:8000/mobile
```

---

## Running on macOS / Linux

```bash
cd edge-server
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

Find your IP:
```bash
# macOS
ipconfig getifaddr en0

# Linux
hostname -I
```

---

## Firewall Configuration (Windows)

If your phone cannot reach the server, you may need to allow port 8000 through Windows Firewall:

```powershell
netsh advfirewall firewall add rule name="Edge AI Server" dir=in action=allow protocol=TCP localport=8000
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Phone can't reach server | Ensure both on same Wi-Fi; check firewall allows port 8000 |
| `laya` import error | Run `pip install laya` again; needs Python 3.10+ |
| Model download stalls | Check internet connection; ~800MB download on first run |
| Port 8000 in use | Use `--port 8080` and update phone URL accordingly |
| Slow inference | First inference is slower (model warmup); subsequent calls are faster |
| `ModuleNotFoundError` | Ensure virtual environment is activated (you should see `(venv)` in terminal) |
| WebSocket disconnects | Dashboard reconnects automatically; refresh page if needed |
