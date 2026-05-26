# 🤖 hAI.OpenCodeHAI

![GitHub stars](https://img.shields.io/github/stars/jbkunama1/hAI.OpenCodeHAI?style=for-the-badge&color=yellow)
![GitHub forks](https://img.shields.io/github/forks/jbkunama1/hAI.OpenCodeHAI?style=for-the-badge&color=blue)
![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Portainer](https://img.shields.io/badge/Portainer-Stack-13BEF9?style=for-the-badge&logo=portainer&logoColor=white)
![DietPi](https://img.shields.io/badge/DietPi-compatible-C7A228?style=for-the-badge)
![Port](https://img.shields.io/badge/Port-4096-purple?style=for-the-badge)
![Open Source](https://img.shields.io/badge/Open%20Source-%E2%9D%A4-red?style=for-the-badge)

---

> **OpenCode** as a full Docker/Portainer stack on DietPi/Debian –  
> usable in the **browser**, on the **console** and as **remote API for 9Router**. 🚀

🇩🇪 [Deutsche Version → README.md](README.md)

---

## 📋 Table of Contents

- [What is OpenCode?](#-what-is-opencode)
- [3 Use Cases](#-3-use-cases)
- [Requirements](#-requirements)
- [Installation via Portainer Stack](#-installation-via-portainer-stack)
- [Configuration](#-configuration)
- [Goal 1: Browser UI](#-goal-1-browser-ui)
- [Goal 2: Console direct](#-goal-2-console-direct)
- [Goal 3: Remote API for 9Router](#-goal-3-remote-api-for-9router)
- [Troubleshooting](#-troubleshooting)
- [License](#-license)

---

## 🧠 What is OpenCode?

**OpenCode** is an open-source AI coding agent connecting to various AI models (Claude, GPT, Gemini, DeepSeek, local models via Ollama or custom routers like **9Router**).

```
┌──────────────────────────────────────────┐
│  🖥️  DietPi / Debian Server              │
│  ┌────────────────────────────────────┐  │
│  │  🐳 Docker Container               │  │
│  │  ┌──────────────────────────────┐  │  │
│  │  │  🤖 OpenCode (Port 4096)     │  │  │
│  │  │  🌐 Web-UI in Browser        │  │  │
│  │  │  💻 Console Alias            │  │  │
│  │  │  🔌 API Endpoint for 9Router │  │  │
│  │  └──────────────────────────────┘  │  │
│  └────────────────────────────────────┘  │
│  📦 Portainer Stack Manager              │
└──────────────────────────────────────────┘
```

---

## 🎯 3 Use Cases

| # | Goal | Access | Port |
|---|---|---|---|
| 1️⃣ | **Browser UI** | `http://<server-ip>:4096` | 4096 |
| 2️⃣ | **Console direct** | `opencode` (alias) | – |
| 3️⃣ | **Remote API for 9Router** | `http://<server-ip>:4096/v1` | 4096 |

---

## ✅ Requirements

| Requirement | Details |
|---|---|
| 🐧 **OS** | DietPi, Debian 11/12, Ubuntu 22.04+ |
| 🐳 **Docker** | Version 24.x or newer |
| 📦 **Docker Compose** | Version 2.x |
| 🖥️ **Portainer** | CE or BE (recommended) |
| 💾 **RAM** | at least 512 MB free |
| 🔑 **API Key** | 9Router, DeepSeek, Google, OpenAI etc. |

---

## 🚀 Installation via Portainer Stack

### Step 1 – Clone repo

```bash
git clone https://github.com/jbkunama1/hAI.OpenCodeHAI.git
cd hAI.OpenCodeHAI
cp .env.example .env
nano .env  # Enter API key
```

### Step 2 – Start stack

```bash
docker compose up -d
docker logs -f opencode
```

Expected output:
```
🚀 Installing OpenCode...
✅ OpenCode ready!
🌐 Server running on http://0.0.0.0:4096
```

### Step 3 – Portainer

1. 📌 **Stacks** → **+ Add Stack**
2. ✏️ Name: `opencode`
3. 📋 Paste `docker-compose.yml` content
4. 🔧 Add environment variables
5. ✅ **Deploy the stack**

---

## ⚙️ Configuration

```env
# 9Router (recommended)
OPENAI_API_KEY=sk-...
OPENAI_BASE_URL=https://9router.example.com/v1

# Or direct DeepSeek
DEEPSEEK_API_KEY=sk-...

# Default model
OPENCODE_MODEL=deepseek/deepseek-chat
```

### OpenCode config.json

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

## 🌐 Goal 1: Browser UI

After stack start, accessible at:

```
http://<server-ip>:4096
```

OpenCode serve starts automatically on container start. ✅

---

## 💻 Goal 2: Console direct

Set alias once – then just type `opencode`:

```bash
echo 'alias opencode="docker exec -it opencode opencode"' >> ~/.bashrc
source ~/.bashrc

# Now directly usable:
opencode
opencode run "Explain this code"
opencode --version
```

---

## 🔌 Goal 3: Remote API for 9Router

Add new provider in your 9Router dashboard:

```
Base URL:  http://<server-ip>:4096/v1
API Key:   (any value, e.g. opencode-local)
```

---

## 🔧 Troubleshooting

| Problem | Solution |
|---|---|
| ❌ Port 4096 not reachable | Check `docker ps`, check firewall |
| ❌ `opencode: command not found` | Check alias in `.bashrc`, run `source ~/.bashrc` |
| ❌ TUI won't start | `docker exec -it -e TERM=xterm-256color opencode opencode` |
| ❌ API key error | Check `config.json`, restart container |
| ❌ 9Router can't find endpoint | Test URL `http://<ip>:4096/v1` with curl |

---

## 📜 License

MIT License – see [LICENSE](LICENSE).

---

<div align="center">

**Made with ❤️ in Karlsruhe by [therealteacher](https://github.com/jbkunama1)**

🌐 [realteacher.de](http://www.realteacher.de) · 📦 [GitHub](https://github.com/jbkunama1) · 🤖 [opencode.ai](https://opencode.ai)

</div>
