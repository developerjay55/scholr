# Scholr — AI-Powered Study Platform
### Built for Prince George's Community College (PGCC)
**Powered by 25 Alpha / jaevhanwalters.com**

---

## What Is Scholr?

Scholr is an AI-powered study platform built specifically for PGCC students. It includes a full NTR 1010 (Nutrition) and SOC 1010 (Sociology) curriculum with AI-generated flashcards, quizzes, study guides, and a live AI tutor — all in a single HTML file deployed to Cloudflare Pages.

---

## Features

| Feature | Description |
|---|---|
| 🃏 Flashcards | 40 NTR 1010 + 30 SOC 1010 cards, flip animation |
| 🎯 Practice Quiz | 20 questions per course with instant AI explanations |
| 🤖 AI Tutor | Live chat + 6 AI tools powered by Claude |
| 🗂️ My Study Plan | Students add personal classes, AI generates materials |
| 📅 Syllabus Calendar | Paste syllabus → AI extracts all dates |
| 📊 Admin Dashboard | Student stats, leaderboard, class performance |
| 🤖 AI Content Studio | Generate flashcards/quizzes from any notes |
| 📖 Study Guide Builder | Section-by-section AI-assisted study guides |
| ⚡ XP & Leaderboard | Gamified learning with badges and rankings |

---

## Tech Stack

- **Frontend**: Single HTML file — no framework, no build tools
- **Backend**: Cloudflare Pages Functions (serverless Workers)
- **AI**: Anthropic Claude API (`claude-sonnet-4-20250514`)
- **Storage**: Browser `localStorage` — no database required
- **Hosting**: Cloudflare Pages (free tier)

---

## Repository Structure

```
scholr/
├── index.html              # Full Scholr application
├── functions/
│   └── api/
│       └── claude.js       # Cloudflare Worker — Claude API proxy
├── wrangler.toml           # Cloudflare Pages config
├── .gitignore
├── .dev.vars.example       # Template for local secrets
└── README.md
```

---

## Deployment Guide

### Prerequisites
- Cloudflare account (free)
- Anthropic API key (`sk-ant-...`) — get at console.anthropic.com
- Node.js installed
- Wrangler CLI: `npm install -g wrangler`

---

### Step 1 — Clone the repo

```bash
git clone https://github.com/YOUR_USERNAME/scholr.git
cd scholr
```

---

### Step 2 — Login to Cloudflare

```bash
wrangler login
```

---

### Step 3 — Create the Pages project (first time only)

```bash
wrangler pages project create scholr-platform
```

---

### Step 4 — Set your Anthropic API key as a secret

```bash
wrangler pages secret put ANTHROPIC_API_KEY --project-name=scholr-platform
# When prompted, paste your key: sk-ant-...
```

> ⚠️ **Never put your API key in any file that gets committed to GitHub.**
> The Worker reads it from Cloudflare's encrypted secret store.

---

### Step 5 — Deploy

```bash
wrangler pages deploy . --project-name=scholr-platform
```

---

### Step 6 — Add your custom domain (optional)

1. Cloudflare Dashboard → **Pages** → `scholr-platform`
2. Click **Custom Domains** → **Add a Custom Domain**
3. Enter your domain (e.g., `scholr.pgcc.edu` or `scholr.yourdomain.com`)
4. Cloudflare auto-configures DNS if your domain is already on Cloudflare

---

### Step 7 — Purge cache

After every deploy:
- Cloudflare Dashboard → Your Zone → **Caching** → **Purge Everything**

---

### Verify

```bash
curl -I https://scholr-platform.pages.dev
# Should return HTTP 200
```

---

## Connect GitHub → Cloudflare Pages (Auto-Deploy)

For automatic deployments on every `git push`:

1. Cloudflare Dashboard → **Pages** → **Create a project** → **Connect to Git**
2. Select your GitHub repo
3. Build settings:
   - **Framework preset**: None
   - **Build command**: *(leave blank)*
   - **Build output directory**: `.` (root)
4. Add environment variable:
   - `ANTHROPIC_API_KEY` = your key (mark as **Secret**)
5. Deploy

Every `git push` to `main` now auto-deploys. ✅

---

## Local Development

```bash
# Copy the secrets template
cp .dev.vars.example .dev.vars
# Edit .dev.vars and add your real ANTHROPIC_API_KEY

# Run locally with Wrangler
wrangler pages dev . --port=8788

# Open http://localhost:8788
```

---

## Admin Login

| Field | Value |
|---|---|
| Username | `jaevhan` |
| Password | *(set by admin)* |

> Change the admin password in `index.html` → `USERS_DEFAULT` before deploying to production. Future versions will support password changes via the admin panel.

---

## Invite Students

1. Login as admin
2. Go to **Admin Panel** → **Users & Classes**
3. Copy the **Invite Code** (default: `PGCC-2026`)
4. Share it with students
5. Students go to your Scholr URL and register with the invite code

---

## Built By

**25 Alpha / jaevhanwalters.com**  
Carlton A. James & Jaev'han Walters  
25 Bravo Inc.

---

## License

Private — All rights reserved. Built exclusively for PGCC students under the 25 Alpha platform.
