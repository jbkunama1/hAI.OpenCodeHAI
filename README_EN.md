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
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Startup Banner](#-startup-banner)
- [Goal 1: Browser UI](#-goal-1-browser-ui)
- [Goal 2: Console direct](#-goal-2-console-direct)
- [Goal 3: Remote API for 9Router](#-goal-3-remote-api-for-9router)
- [MCP Setup (Filesystem & GitHub)](#-mcp-setup-filesystem--github)
- [Troubleshooting](#-troubleshooting)
- [License](#-license)

---

## 🧠 What is OpenCode?

**OpenCode** is an open-source AI coding agent connecting to various AI models (Claude, GPT, Gemini, DeepSeek, local models via Ollama or custom routers like **9Router**).

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

## 🚀 Installation

```bash
git clone https://github.com/jbkunama1/hAI.OpenCodeHAI.git
cd hAI.OpenCodeHAI
cp .env.example .env
nano .env
docker compose up -d
docker logs -f opencode
```

---

## ⚙️ Configuration

```env
# 9Router (recommended)
OPENAI_API_KEY=sk-...
OPENAI_BASE_URL=https://9router.example.com/v1
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

## 🖥️ Startup Banner

After `docker compose up -d` you'll see in the logs:

```
🚀 Installing OpenCode...
✅ OpenCode ready!

╔════════════════════════════════════════════════╗
║  🤖 hAI.OpenCodeHAI - Ready!                  ║
╠════════════════════════════════════════════════╣
║  🌐 Browser-UI:                                ║
║     http://<server-ip>:4096                    ║
║                                                ║
║  💻 Console (Alias on host):                   ║
║     alias opencode="docker exec -it opencode opencode" ║
║     → then just type: opencode                 ║
║                                                ║
║  🔌 9Router API Endpoint:                      ║
║     http://<server-ip>:4096/v1                 ║
║                                                ║
║  🐚 Inside container:                          ║
║     docker exec -it opencode opencode          ║
╚════════════════════════════════════════════════╝
```

---

## 🌐 Goal 1: Browser UI

```
http://<server-ip>:4096
```

OpenCode serve starts automatically on container start. ✅

---

## 💻 Goal 2: Console direct

```bash
echo 'alias opencode="docker exec -it opencode opencode"' >> ~/.bashrc
source ~/.bashrc

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

## 🔗 MCP Setup (Filesystem & GitHub)

OpenCode supports MCP servers (Model Context Protocol) for extended filesystem and GitHub access directly inside the container.

📄 **Detailed guide (German):** [docs/mcp-setup.md](docs/mcp-setup.md)

### Quick overview

| MCP | Type | Description |
|---|---|---|
| `filesystem` | local | File access to `/workspace` and config directory |
| `github-local` | remote | GitHub-MCP as local HTTP server (port 3001) |

```json
"mcp": {
  "filesystem": {
    "type": "local",
    "command": ["npx", "-y", "@modelcontextprotocol/server-filesystem", "/workspace"],
    "enabled": true
  },
  "github-local": {
    "type": "remote",
    "url": "http://127.0.0.1:3001/mcp",
    "enabled": true
  }
}
```

> **Note:** The GitHub Copilot MCP endpoint is not compatible with OpenCode. Use GitHub-MCP as a local HTTP server instead – see [docs/mcp-setup.md](docs/mcp-setup.md).

---

## 🔧 Troubleshooting

| Problem | Solution |
|---|---|
| ❌ Port 4096 not reachable | Check `docker ps`, check firewall |
| ❌ `opencode: command not found` | Check alias in `.bashrc`, run `source ~/.bashrc` |
| ❌ TUI won't start | `docker exec -it -e TERM=xterm-256color opencode opencode` |
| ❌ API key error | Check `config.json`, restart container |
| ❌ 9Router can't find endpoint | Test with `curl http://<ip>:4096/v1` |
| ❌ MCP Filesystem error (ENOENT) | Don't pass flags – only directories as args, see [MCP Setup](docs/mcp-setup.md) |
| ❌ GitHub-MCP OAuth error | Copilot endpoint not compatible – use local HTTP server, see [MCP Setup](docs/mcp-setup.md) |

---

## 📜 License

MIT License – see [LICENSE](LICENSE).

---

<div align="center">

**Made with ❤️ in Karlsruhe by [therealteacher](https://github.com/jbkunama1)**

🌐 [realteacher.de](http://www.realteacher.de) · 📦 [GitHub](https://github.com/jbkunama1) · 🤖 [opencode.ai](https://opencode.ai)

</div>
