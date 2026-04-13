# AgentZ — SOC Level AI

[![Live Demo](https://img.shields.io/badge/Live%20Demo-zrnge.github.io%2Fagentz-f97316?style=flat-square&logo=github)](https://zrnge.github.io/agentz)
[![GitHub Pages](https://img.shields.io/badge/Deployed%20on-GitHub%20Pages-222?style=flat-square&logo=github)](https://zrnge.github.io/agentz)
[![Ollama](https://img.shields.io/badge/Powered%20by-Ollama-3b82f6?style=flat-square)](https://ollama.com)
[![100% Local](https://img.shields.io/badge/AI%20Inference-100%25%20Local-22c55e?style=flat-square&logo=shield)](https://zrnge.github.io/agentz)
[![License: MIT](https://img.shields.io/badge/License-MIT-8b97aa?style=flat-square)](LICENSE)

Local AI-powered security incident triage. Connect it to your SIEM, point it at a local Ollama model, and get instant analysis on every alert — nothing leaves your machine.

![AgentZ Screenshot](https://github.com/zrnge/AgentZ/blob/main/Screen3.png)

---

## What it does

AgentZ sits between your SIEM and your brain. It polls your security platform on a set interval, pulls in new alerts, and runs each one through a local LLM to give you a plain-English breakdown: what happened, what's affected, likely attack vector, and what to do first.

It works with SentinelOne, CrowdStrike Falcon, Microsoft Defender, Elastic SIEM, Splunk, or any custom API. You can also paste in old alerts or exported JSON manually and analyze those too.

Everything stays local. The AI never touches an external server.

---

## Getting started

### 1. Run Ollama with cross-origin access enabled

By default Ollama blocks requests from browser origins. You need to start it with `OLLAMA_ORIGINS` set to `*` so the page can reach it.

**Windows (PowerShell)**
```powershell
$env:OLLAMA_ORIGINS="*"; ollama serve
```

**macOS / Linux**
```bash
OLLAMA_ORIGINS="*" ollama serve
```

**Linux — if you run Ollama as a systemd service**

Edit the service file:
```bash
sudo systemctl edit ollama.service
```
Add this under `[Service]`:
```ini
[Service]
Environment="OLLAMA_ORIGINS=*"
```
Then restart:
```bash
sudo systemctl daemon-reload
sudo systemctl restart ollama
```

Once Ollama is running, pull a model if you haven't already:
```bash
ollama pull llama3.1
```
Any model works. Larger ones give better analysis — llama3.1 8B is a good starting point.

---

### 2. Open AgentZ

Go to **[zrnge.github.io/agentz](https://zrnge.github.io/agentz)** or open `index.html` locally in your browser.

> **Note on HTTPS + Ollama:** GitHub Pages is served over HTTPS, which means the browser will block calls to `http://localhost` (mixed content). To use AgentZ from GitHub Pages, either run Ollama behind a reverse proxy with TLS, or just open the `index.html` file directly from your machine instead.

---

### 3. Configure

Click the **⚙ Settings** button in the top-right corner.

| Setting | What to put |
|---|---|
| Ollama API URL | `http://localhost:11434` (default) |
| AI Model | `llama3.1` or whatever you have pulled |
| Platform | Your SIEM — SentinelOne, CrowdStrike, Defender, Elastic, Splunk, or Custom |
| API Base URL | Your SIEM console URL |
| API Token | Your API key or Bearer token |
| Poll Interval | How often to check for new alerts (seconds) |
| Min Severity | Filter out noise — Medium+ is a good default |

Hit **Test Ollama Connection** to make sure the model is reachable, then click **▶ Start Agent**.

---

## Features

- **Auto-polling** — checks your SIEM on a configurable interval and auto-analyzes every new alert
- **Manual import** — paste raw JSON, log lines, or plain text from any previous incident and analyze it on demand
- **AI analysis** — summary, severity assessment, affected assets, MITRE ATT&CK mapping, and remediation steps
- **HTML report export** — generates a clean standalone report file you can share or archive
- **JSON export** — export the raw incident + analysis as JSON
- **Dark / light mode** — toggle in the header, preference is saved
- **Desktop notifications + sound** — optional alerts when new incidents come in
- **Filter & search** — filter by severity, search by title or source
- **100% offline AI** — all inference runs locally through Ollama, no data ever leaves your network

---

## Supported platforms

| Platform | Notes |
|---|---|
| SentinelOne | Uses `/web/api/v2.1/threats` |
| CrowdStrike Falcon | Uses `/detects/queries/detects/v1` |
| Microsoft Defender | Uses `/api/alerts` |
| Elastic SIEM | Queries `.siem-signals-*` index |
| Splunk | Uses `/services/search/jobs/export` |
| Custom API | Bring your own endpoint — expects JSON with `id`, `title`, `severity` fields |

---

## Deploying to GitHub Pages

This is a single HTML file with no build step and no dependencies.

1. Fork or clone this repo
2. Go to **Settings → Pages** in your GitHub repo
3. Set source to **Deploy from a branch**, pick `main`, root `/`
4. Your instance will be live at `https://<your-username>.github.io/<repo-name>`

That's it.

---

## Privacy

- API tokens are stored in browser session memory only — they are never written to disk or sent anywhere except the SIEM endpoint you configure
- If you enable "Save settings to browser", Ollama URL, model name, and platform settings are saved to `localStorage` — tokens are never persisted
- All AI inference runs on your local Ollama instance

---

## Author

Built by **[Zrnge](https://github.com/zrnge)**

---

## License

MIT
