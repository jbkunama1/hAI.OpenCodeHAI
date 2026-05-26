# 🤖 hAI.OpenCodeHAI

![GitHub stars](https://img.shields.io/github/stars/jbkunama1/hAI.OpenCodeHAI?style=for-the-badge&color=yellow)
![GitHub forks](https://img.shields.io/github/forks/jbkunama1/hAI.OpenCodeHAI?style=for-the-badge&color=blue)
![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Portainer](https://img.shields.io/badge/Portainer-Stack-13BEF9?style=for-the-badge&logo=portainer&logoColor=white)
![DietPi](https://img.shields.io/badge/DietPi-kompatibel-C7A228?style=for-the-badge)
![Port](https://img.shields.io/badge/Port-4096-purple?style=for-the-badge)
![Open Source](https://img.shields.io/badge/Open%20Source-%E2%9D%A4-red?style=for-the-badge)

---

> **OpenCode** als vollständiger Docker/Portainer-Stack auf DietPi/Debian –  
> nutzbar im **Browser**, auf der **Konsole** und als **Remote-API für 9Router**. 🚀

🇬🇧 [English version → README_EN.md](README_EN.md)

---

## 📋 Inhaltsverzeichnis

- [Was ist OpenCode?](#-was-ist-opencode)
- [3 Nutzungsziele](#-3-nutzungsziele)
- [Voraussetzungen](#-voraussetzungen)
- [Installation](#-installation)
- [Konfiguration](#-konfiguration)
- [Startup-Banner](#-startup-banner)
- [Ziel 1: Browser-UI](#-ziel-1-browser-ui)
- [Ziel 2: Konsole direkt](#-ziel-2-konsole-direkt)
- [Ziel 3: Remote-API für 9Router](#-ziel-3-remote-api-für-9router)
- [Troubleshooting](#-troubleshooting)
- [Lizenz](#-lizenz)

---

## 🧠 Was ist OpenCode?

**OpenCode** ist ein quelloffener AI-Coding-Agent, der sich mit verschiedenen KI-Modellen verbindet (Claude, GPT, Gemini, DeepSeek, lokale Modelle via Ollama oder eigene Router wie **9Router**).

---

## 🎯 3 Nutzungsziele

| # | Ziel | Zugriff | Port |
|---|---|---|---|
| 1️⃣ | **Browser-UI** | `http://<server-ip>:4096` | 4096 |
| 2️⃣ | **Konsole direkt** | `opencode` (Alias) | – |
| 3️⃣ | **Remote-API für 9Router** | `http://<server-ip>:4096/v1` | 4096 |

---

## ✅ Voraussetzungen

| Anforderung | Details |
|---|---|
| 🐧 **OS** | DietPi, Debian 11/12, Ubuntu 22.04+ |
| 🐳 **Docker** | Version 24.x oder neuer |
| 📦 **Docker Compose** | Version 2.x |
| 🖥️ **Portainer** | CE oder BE (empfohlen) |
| 💾 **RAM** | mindestens 512 MB frei |
| 🔑 **API-Key** | 9Router, DeepSeek, Google, OpenAI o.ä. |

---

## 🚀 Installation

```bash
# 1. Repo klonen
git clone https://github.com/jbkunama1/hAI.OpenCodeHAI.git
cd hAI.OpenCodeHAI

# 2. .env anlegen
cp .env.example .env
joe .env  # API-Key eintragen

# 3. Stack starten
docker compose up -d

# 4. Logs prüfen
docker logs -f opencode
```

---

## ⚙️ Konfiguration

```env
# 9Router (empfohlen)
OPENAI_API_KEY=sk-...
OPENAI_BASE_URL=https://9router.example.com/v1
OPENCODE_MODEL=deepseek/deepseek-chat
```

### OpenCode config.json

```bash
mkdir -p /var/lib/docker/volumes/opencode_config/_data
joe /var/lib/docker/volumes/opencode_config/_data/config.json
```

```json
{
  "provider": {
    "9router": {
      "npm": "@ai-sdk/openai-compatible",
      "options": {
        "baseURL": "https://9router.example.com/v1",
        "apiKey": "sk-..."
      },
      "models": {
        "deepseek/deepseek-chat": {
          "name": "deepseek/deepseek-chat",
          "modalities": {
            "input": ["text", "image"],
            "output": ["text"]
          }
        }
      }
    }
  },
  "model": "9router/deepseek/deepseek-chat"
}
```

---

## 🖥️ Startup-Banner

Nach `docker compose up -d` siehst du in den Logs:

```
🚀 Installing OpenCode...
✅ OpenCode ready!

╔════════════════════════════════════════════════╗
║  🤖 hAI.OpenCodeHAI - Ready!                  ║
╠════════════════════════════════════════════════╣
║  🌐 Browser-UI:                                ║
║     http://<server-ip>:4096                    ║
║                                                ║
║  💻 Konsole (Alias auf dem Host):              ║
║     alias opencode="docker exec -it opencode opencode" ║
║     → dann einfach: opencode                   ║
║                                                ║
║  🔌 9Router API-Endpoint:                      ║
║     http://<server-ip>:4096/v1                 ║
║                                                ║
║  🐚 In Container:                              ║
║     docker exec -it opencode opencode          ║
╚════════════════════════════════════════════════╝
```

---

## 🌐 Ziel 1: Browser-UI

```
http://<server-ip>:4096
```

OpenCode serve startet automatisch beim Container-Start. ✅

---

## 💻 Ziel 2: Konsole direkt

Alias einmalig setzen – danach einfach `opencode` tippen:

```bash
echo 'alias opencode="docker exec -it opencode opencode"' >> ~/.bashrc
source ~/.bashrc

# Ab jetzt direkt nutzbar:
opencode
opencode run "Erkläre mir diesen Code"
opencode --version
```

---

## 🔌 Ziel 3: Remote-API für 9Router

In deinem 9Router-Dashboard neuen Provider eintragen:

```
Base URL:  http://<server-ip>:4096/v1
API Key:   (beliebig, z.B. opencode-local)
```

---

## 🔧 Troubleshooting

| Problem | Lösung |
|---|---|
| ❌ Port 4096 nicht erreichbar | `docker ps` prüfen, Firewall checken |
| ❌ `opencode: command not found` | Alias in `.bashrc` prüfen, `source ~/.bashrc` |
| ❌ TUI startet nicht | `docker exec -it -e TERM=xterm-256color opencode opencode` |
| ❌ API-Key-Fehler | `config.json` prüfen, Container neu starten |
| ❌ 9Router findet Endpoint nicht | URL mit `curl http://<ip>:4096/v1` testen |
| ❌ DNS-Fehler (EAI_AGAIN) | `dns: [8.8.8.8, 1.1.1.1]` in docker-compose.yml (Tailscale-Workaround) |

---

## 📜 Lizenz

MIT License – siehe [LICENSE](LICENSE).

---

<div align="center">

**Made with ❤️ in Karlsruhe by [therealteacher](https://github.com/jbkunama1)**

🌐 [realteacher.de](http://www.realteacher.de) · 📦 [GitHub](https://github.com/jbkunama1) · 🤖 [opencode.ai](https://opencode.ai)

</div>
