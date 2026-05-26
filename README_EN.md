# 🤖 hAI.OpenCodeHAI

![GitHub stars](https://img.shields.io/github/stars/jbkunama1/hAI.OpenCodeHAI?style=for-the-badge&color=yellow)
![GitHub forks](https://img.shields.io/github/forks/jbkunama1/hAI.OpenCodeHAI?style=for-the-badge&color=blue)
![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Portainer](https://img.shields.io/badge/Portainer-Stack-13BEF9?style=for-the-badge&logo=portainer&logoColor=white)
![DietPi](https://img.shields.io/badge/DietPi-compatible-C7A228?style=for-the-badge)
![Open Source](https://img.shields.io/badge/Open%20Source-%E2%9D%A4-red?style=for-the-badge)

---

> **OpenCode** as a full Docker/Portainer stack on your DietPi or Debian server.  
> Free, open source, multi-model support. 🚀

🇩🇪 [Deutsche Version → README.md](README.md)

---

## 📋 Table of Contents

- [What is OpenCode?](#-what-is-opencode)
- [Requirements](#-requirements)
- [Project Structure](#-project-structure)
- [Installation via Portainer Stack](#-installation-via-portainer-stack)
- [Manual Docker Installation](#-manual-docker-installation)
- [Configuration & Free Models](#-configuration--free-models)
- [Usage](#-usage)
- [DietPi Tips](#-dietpi-tips)
- [Troubleshooting](#-troubleshooting)
- [License](#-license)

---

## 🧠 What is OpenCode?

**OpenCode** is a next-generation open-source, terminal-based AI coding agent.  
It connects to various AI models (Claude, GPT, Gemini, DeepSeek, local models via Ollama) and helps you write, debug, and understand code directly in the terminal.

```
┌──────────────────────────────────────────┐
│  🖥️  DietPi / Debian Server             │
│  ┌─────────────────────────────────────┐ │
│  │  🐳 Docker Container               │ │
│  │  ┌─────────────────────────────┐   │ │
│  │  │  🤖 OpenCode AI Agent       │   │ │
│  │  │  📂 Project folder (mount)  │   │ │
│  │  │  🔑 API-Keys (.env)         │   │ │
│  │  └─────────────────────────────┘   │ │
│  └─────────────────────────────────────┘ │
│  📦 Portainer Stack Manager             │
└──────────────────────────────────────────┘
```

---

## ✅ Requirements

| Requirement | Details |
|---|---|
| 🐧 **OS** | DietPi, Debian 11/12, Ubuntu 22.04+ |
| 🐳 **Docker** | Version 24.x or newer |
| 📦 **Docker Compose** | Version 2.x |
| 🖥️ **Portainer** | CE or BE (optional but recommended) |
| 💾 **RAM** | at least 512 MB free |
| 🔑 **API Key** | e.g. DeepSeek, Google, OpenAI, Anthropic |
| 🌐 **Internet** | required for cloud models |

---

## 📁 Project Structure

```
hAI.OpenCodeHAI/
├── 📄 README.md              ← German version
├── 📄 README_EN.md           ← This file
├── 📄 LICENSE                ← MIT License
├── 🐳 docker-compose.yml     ← Portainer Stack definition
├── ⚙️  .env.example           ← Example configuration
├── 🚫 .gitignore
└── 🌐 docs/
    └── index.html            ← GitHub Pages
```

---

## 🚀 Installation via Portainer Stack

### Step 1 – Open Portainer

```
http://<your-server-ip>:9000
```

### Step 2 – Create Stack

1. 📌 **Stacks** → **+ Add Stack**
2. ✏️ Name: `opencode`
3. 📋 Choose **Web editor** and paste `docker-compose.yml` content
4. 🔧 Add environment variables (API keys)
5. ✅ Click **Deploy the stack**

### Step 3 – Enter the Container

```bash
# Via Portainer: Containers → opencode → Console → /bin/sh
# Or via SSH:
docker exec -it opencode sh
cd /workspace && opencode
```

---

## 🐳 Manual Docker Installation

```bash
git clone https://github.com/jbkunama1/hAI.OpenCodeHAI.git
cd hAI.OpenCodeHAI
cp .env.example .env
nano .env
docker compose up -d
docker exec -it opencode sh
cd /workspace && opencode
```

---

## ⚙️ Configuration & Free Models

```env
# 🔑 Only fill in keys you need
DEEPSEEK_API_KEY=sk-...
GOOGLE_API_KEY=AIza...
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
OPENROUTER_API_KEY=sk-or-...

# 🤖 Default model
OPENCODE_MODEL=deepseek/deepseek-chat

# 🌐 Ollama (local models, optional)
OLLAMA_HOST=http://host.docker.internal:11434
```

### 🆓 Free Model Options

| Model | Provider | Free? | Quality |
|---|---|---|---|
| `deepseek/deepseek-chat` | DeepSeek | ✅ Free tier | ⭐⭐⭐⭐ |
| `google/gemini-2.0-flash` | Google AI | ✅ Free tier | ⭐⭐⭐⭐ |
| `qwen/qwen-2.5-coder-32b:free` | OpenRouter | ✅ Free tier | ⭐⭐⭐⭐ |
| Local models via Ollama | Self-hosted | ✅ Completely free | ⭐⭐⭐ |

---

## 💡 Usage

```bash
opencode
opencode run "Create a Python Fibonacci function"
opencode --model deepseek/deepseek-chat
opencode init
```

---

## 🍓 DietPi Tips

```bash
dietpi-software install 162  # Docker
dietpi-software install 134  # Portainer
docker stats opencode
docker logs -f opencode
```

> 💡 **Tip:** On low-RAM DietPi → `OPENCODE_MODEL=deepseek/deepseek-chat` (API only – no local RAM usage!)

---

## 🔧 Troubleshooting

| Problem | Solution |
|---|---|
| ❌ `opencode: command not found` | Run `npm install -g opencode-ai@latest` again |
| ❌ API key error | Check `.env`, restart container |
| ❌ Container won't start | Check `docker logs opencode` |
| ❌ Not enough RAM | Use lighter model or increase swap |
| ❌ Ollama connection fails | Check `OLLAMA_HOST`, use `host.docker.internal` |

---

## 📜 License

MIT License – see [LICENSE](LICENSE).

---

<div align="center">

**Made with ❤️ in Karlsruhe by [therealteacher](https://github.com/jbkunama1)**

🌐 [realteacher.de](http://www.realteacher.de) · 📦 [GitHub](https://github.com/jbkunama1) · 🤖 [opencode.ai](https://opencode.ai)

</div>
