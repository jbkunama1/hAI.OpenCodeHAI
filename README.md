# 🤖 hAI.OpenCodeHAI

![GitHub stars](https://img.shields.io/github/stars/jbkunama1/hAI.OpenCodeHAI?style=for-the-badge&color=yellow)
![GitHub forks](https://img.shields.io/github/forks/jbkunama1/hAI.OpenCodeHAI?style=for-the-badge&color=blue)
![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Portainer](https://img.shields.io/badge/Portainer-Stack-13BEF9?style=for-the-badge&logo=portainer&logoColor=white)
![DietPi](https://img.shields.io/badge/DietPi-kompatibel-C7A228?style=for-the-badge)
![Open Source](https://img.shields.io/badge/Open%20Source-%E2%9D%A4-red?style=for-the-badge)

---

> **OpenCode** als vollständiger Docker/Portainer-Stack auf deinem DietPi- oder Debian-Server.  
> Kostenlos, quelloffen, modellflexibel. 🚀

🇬🇧 [English version → README_EN.md](README_EN.md)

---

## 📋 Inhaltsverzeichnis

- [Was ist OpenCode?](#-was-ist-opencode)
- [Voraussetzungen](#-voraussetzungen)
- [Projektstruktur](#-projektstruktur)
- [Installation via Portainer Stack](#-installation-via-portainer-stack)
- [Manuelle Docker-Installation](#-manuelle-docker-installation)
- [Konfiguration & kostenlose Modelle](#-konfiguration--kostenlose-modelle)
- [Nutzung](#-nutzung)
- [Tipps für DietPi](#-tipps-für-dietpi)
- [Troubleshooting](#-troubleshooting)
- [Lizenz](#-lizenz)

---

## 🧠 Was ist OpenCode?

**OpenCode** ist ein quelloffener, Terminal-basierter AI-Coding-Agent der nächsten Generation.  
Er verbindet sich mit verschiedenen KI-Modellen (Claude, GPT, Gemini, DeepSeek, lokale Modelle via Ollama) und hilft beim Schreiben, Debuggen und Verstehen von Code direkt im Terminal.

```
┌──────────────────────────────────────────┐
│  🖥️  DietPi / Debian Server             │
│  ┌─────────────────────────────────────┐ │
│  │  🐳 Docker Container               │ │
│  │  ┌─────────────────────────────┐   │ │
│  │  │  🤖 OpenCode AI Agent       │   │ │
│  │  │  📂 Projektordner (mount)   │   │ │
│  │  │  🔑 API-Keys (.env)         │   │ │
│  │  └─────────────────────────────┘   │ │
│  └─────────────────────────────────────┘ │
│  📦 Portainer Stack Manager             │
└──────────────────────────────────────────┘
```

---

## ✅ Voraussetzungen

| Anforderung | Details |
|---|---|
| 🐧 **OS** | DietPi, Debian 11/12, Ubuntu 22.04+ |
| 🐳 **Docker** | Version 24.x oder neuer |
| 📦 **Docker Compose** | Version 2.x |
| 🖥️ **Portainer** | CE oder BE (optional, aber empfohlen) |
| 💾 **RAM** | mindestens 512 MB frei |
| 🔑 **API-Key** | z. B. DeepSeek, Google, OpenAI, Anthropic |
| 🌐 **Internetzugang** | für Cloud-Modelle erforderlich |

---

## 📁 Projektstruktur

```
hAI.OpenCodeHAI/
├── 📄 README.md              ← Diese Datei (DE)
├── 📄 README_EN.md           ← English version
├── 📄 LICENSE                ← MIT Lizenz
├── 🐳 docker-compose.yml     ← Portainer Stack Definition
├── ⚙️  .env.example           ← Beispiel-Konfiguration
├── 🚫 .gitignore
└── 🌐 docs/
    └── index.html            ← GitHub Pages
```

---

## 🚀 Installation via Portainer Stack

### Schritt 1 – Portainer öffnen

```
http://<deine-server-ip>:9000
```

### Schritt 2 – Stack anlegen

1. 📌 **Stacks** → **+ Add Stack**
2. ✏️ Name: `opencode`
3. 📋 **Web editor** wählen und `docker-compose.yml` einfügen
4. 🔧 Environment variables eintragen (API-Keys)
5. ✅ **Deploy the stack** klicken

### Schritt 3 – Container betreten

```bash
# Via Portainer: Containers → opencode → Console → /bin/sh
# Oder per SSH:
docker exec -it opencode sh
cd /workspace && opencode
```

---

## 🐳 Manuelle Docker-Installation

```bash
# 1. Repo klonen
git clone https://github.com/jbkunama1/hAI.OpenCodeHAI.git
cd hAI.OpenCodeHAI

# 2. .env anlegen
cp .env.example .env
nano .env  # API-Keys eintragen

# 3. Stack starten
docker compose up -d

# 4. Container betreten & OpenCode starten
docker exec -it opencode sh
cd /workspace && opencode
```

---

## ⚙️ Konfiguration & kostenlose Modelle

```env
# 🔑 Nur benötigte Keys eintragen
DEEPSEEK_API_KEY=sk-...
GOOGLE_API_KEY=AIza...
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
OPENROUTER_API_KEY=sk-or-...

# 🤖 Standard-Modell
OPENCODE_MODEL=deepseek/deepseek-chat

# 🌐 Ollama (lokale Modelle, optional)
OLLAMA_HOST=http://host.docker.internal:11434
```

### 🆓 Kostenlose Modell-Optionen

| Modell | Provider | Kostenlos? | Qualität |
|---|---|---|---|
| `deepseek/deepseek-chat` | DeepSeek | ✅ Free-Tier | ⭐⭐⭐⭐ |
| `google/gemini-2.0-flash` | Google AI | ✅ Free-Tier | ⭐⭐⭐⭐ |
| `qwen/qwen-2.5-coder-32b:free` | OpenRouter | ✅ Free-Tier | ⭐⭐⭐⭐ |
| Lokale Modelle via Ollama | Self-hosted | ✅ Komplett kostenlos | ⭐⭐⭐ |

---

## 💡 Nutzung

```bash
# OpenCode interaktiv starten
opencode

# Direkten Befehl ausführen
opencode run "Erstelle eine Python-Funktion für Fibonacci"

# Mit spezifischem Modell
opencode --model deepseek/deepseek-chat

# Projektkontext initialisieren
opencode init
```

---

## 🍓 Tipps für DietPi

```bash
# Docker & Portainer auf DietPi installieren
dietpi-software install 162  # Docker
dietpi-software install 134  # Portainer

# RAM-Verbrauch prüfen
docker stats opencode

# Logs anzeigen
docker logs -f opencode
```

> 💡 **Tipp:** Auf DietPi mit wenig RAM → `OPENCODE_MODEL=deepseek/deepseek-chat`  
> (Free-Tier, nur API-Call – kein lokaler RAM-Verbrauch!)

---

## 🔧 Troubleshooting

| Problem | Lösung |
|---|---|
| ❌ `opencode: command not found` | `npm install -g opencode-ai@latest` erneut ausführen |
| ❌ API-Key-Fehler | `.env` prüfen, Container neu starten |
| ❌ Container startet nicht | `docker logs opencode` prüfen |
| ❌ Zu wenig RAM | Leichteres Modell wählen oder Swap erhöhen |
| ❌ Ollama-Verbindung schlägt fehl | `OLLAMA_HOST` prüfen, `host.docker.internal` nutzen |

---

## 📜 Lizenz

MIT License – siehe [LICENSE](LICENSE).

---

<div align="center">

**Made with ❤️ in Karlsruhe by [therealteacher](https://github.com/jbkunama1)**

🌐 [realteacher.de](http://www.realteacher.de) · 📦 [GitHub](https://github.com/jbkunama1) · 🤖 [opencode.ai](https://opencode.ai)

</div>
