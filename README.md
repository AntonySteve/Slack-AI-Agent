# 🤖 Slack AI Agent
> **Automatically analyze every new Slack member — score their fit, research their background, and post insights to your team channel. Instantly.**

[![Node.js](https://img.shields.io/badge/Node.js-18+-339933?style=flat-square&logo=node.js&logoColor=white)](https://nodejs.org)
[![Slack](https://img.shields.io/badge/Slack-Bolt-4A154B?style=flat-square&logo=slack&logoColor=white)](https://slack.dev/bolt-js)
[![Gemini](https://img.shields.io/badge/Google-Gemini_2.0-4285F4?style=flat-square&logo=google&logoColor=white)](https://aistudio.google.com)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

---

## ⚡ What It Does

Every time someone joins your Slack workspace, this agent:

1. 🔍 **Researches** their company domain and GitHub profile automatically
2. 🧠 **Analyzes** their fit using Google Gemini AI (fit score 0–100)
3. 💬 **Posts** a structured analysis card to your private Slack channel
4. 💾 **Saves** every analysis to a local database for future reference

No manual work. No missed leads. Just instant intelligence on every new member.

---

## 🏗️ Architecture

```
New Member Joins Slack
        │
        ▼
 Slack Bolt (Socket Mode)
        │
        ▼
  Research Engine
  ├── Company Website Scraper
  └── GitHub Profile Lookup
        │
        ▼
  Google Gemini AI
  └── Fit Score + Insights + Recommendations
        │
        ▼
  SQLite Database ──► Slack Channel Post
```

---

## 🚀 Quick Start

### Prerequisites

- Node.js 18+
- A Slack App with Socket Mode enabled
- Google Gemini API key

### 1. Clone & Install

```bash
git clone https://github.com/AntonySteve/Slack-AI-Agent.git
cd Slack-AI-Agent
npm install
```

### 2. Configure Environment

```bash
cp .env.example .env
```

Fill in your `.env` file with real values (see [Environment Variables](#-environment-variables) below).

### 3. Run

```bash
# Development
NODE_ENV=development node index.js

# Production
node index.js
```

---

## 🔑 Environment Variables

| Variable | Description | Where to Get It |
|---|---|---|
| `SLACK_BOT_TOKEN` | Bot OAuth token | [api.slack.com](https://api.slack.com/apps) → OAuth & Permissions |
| `SLACK_APP_TOKEN` | App-level token | [api.slack.com](https://api.slack.com/apps) → Basic Information → App-Level Tokens |
| `SLACK_SIGNING_SECRET` | Request signing secret | [api.slack.com](https://api.slack.com/apps) → Basic Information |
| `SLACK_PRIVATE_CHANNEL_ID` | Channel to post analyses | Right-click channel in Slack → View Details → Channel ID |
| `GEMINI_API_KEY` | Google Gemini API key | [aistudio.google.com](https://aistudio.google.com/app/apikey) |
| `COMPANY_NAME` | Your company name | — |
| `COMPANY_PRODUCT` | Your product name | — |
| `NODE_ENV` | `development` or `production` | — |
| `PORT` | Express server port (default: 3000) | — |

---

## 🧪 Test the Agent (Development Mode)

When `NODE_ENV=development`, a test endpoint is available:

```powershell
# PowerShell
Invoke-WebRequest -Uri "http://localhost:3000/test/analyze-member" `
  -Method POST `
  -ContentType "application/json" `
  -Body '{"memberInfo":{"name":"John Doe","email":"john@techcorp.com","title":"Senior Software Engineer"}}'
```

```bash
# bash / WSL
curl -X POST http://localhost:3000/test/analyze-member \
  -H "Content-Type: application/json" \
  -d '{"memberInfo":{"name":"John Doe","email":"john@techcorp.com","title":"Senior Software Engineer"}}'
```

**Health Check:**
```bash
curl http://localhost:3000/health
```

---

## 📊 Sample Output

When a new member joins, your Slack channel receives:

```
┌─────────────────────────────────────┐
│  New Member: John Doe               │
├─────────────────────────────────────┤
│  Fit Score: 87/100  🟢              │
│  Email: john@techcorp.com           │
│  Title: Senior Software Engineer    │
├─────────────────────────────────────┤
│  Recommendations:                   │
│  • Invite to #product-demo channel  │
│  • Share enterprise case studies    │
│  • Tag for Q1 outreach campaign     │
├─────────────────────────────────────┤
│  Analyzed: 2025-06-06T10:32:00Z     │
└─────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Slack Integration | [@slack/bolt](https://slack.dev/bolt-js) + Socket Mode |
| AI Analysis | [Google Gemini 2.0 Flash](https://aistudio.google.com) via LangChain |
| Web Framework | [Express.js](https://expressjs.com) |
| Database | SQLite (via custom `db.js`) |
| HTTP Client | [Axios](https://axios-http.com) |
| Config | [dotenv](https://github.com/motdotla/dotenv) |

---

## 📁 Project Structure

```
Slack-AI-Agent/
├── index.js          # Main agent — Slack events, Express routes, AI logic
├── db.js             # Database init, save, and query functions
├── .env              # Your secrets (never commit this)
├── .env.example      # Template for environment variables
├── .gitignore        # Keeps .env and node_modules out of git
├── package.json
└── README.md
```

---

## ⚙️ Slack App Setup

### Required Bot Token Scopes
```
channels:read
chat:write
users:read
users:read.email
```

### Required Event Subscriptions
```
team_join
member_joined_channel
```

### Enable Socket Mode
Go to your Slack App → **Socket Mode** → Toggle **Enable Socket Mode** → Generate an App-Level Token with `connections:write` scope.

---

## 🔒 Security

- **Never commit `.env`** — it's in `.gitignore` by default
- **Rotate your keys** if they were ever pushed to a public repo
- All secrets are loaded via `process.env` — no hardcoded credentials anywhere in the codebase

---

## 📄 License

MIT — do whatever you want with it.

---

## 🙋 Author

Built by [AntonySteve](https://github.com/AntonySteve)
