# FuelPlan Pro — Deployment Guide (Option A: Vercel Web App)

## What's in this package

```
fuelplan-pro/
├── src/
│   ├── main.jsx          ← React entry point
│   └── App.jsx           ← Full app (all features from your session)
├── api/
│   └── claude.js         ← ✅ SECURE backend proxy (API key never exposed)
├── public/               ← Static assets (add favicon here if you want)
├── index.html            ← HTML shell
├── package.json          ← Dependencies
├── vite.config.js        ← Vite build config
├── vercel.json           ← Vercel deployment config
├── .env.example          ← Copy this → .env.local for dev
└── .gitignore            ← Keeps secrets out of git
```

---

## Step-by-Step Deployment

### Step 1 — Install Node.js (if you haven't)
Download from: https://nodejs.org (choose LTS version)

### Step 2 — Set up the project locally
```bash
# Unzip the downloaded folder, then:
cd fuelplan-pro
npm install
```

### Step 3 — Add your API key for local development
```bash
# Copy the example env file
cp .env.example .env.local

# Open .env.local and replace the placeholder:
# ANTHROPIC_API_KEY=sk-ant-your-real-key-here
```
Get your API key from: https://console.anthropic.com

### Step 4 — Test locally
```bash
npm run dev
```
Open http://localhost:5173 — the app should load fully.

> **Note for local dev:** The `/api/claude` proxy requires Vercel's dev server to work.
> Install it with: `npm install -g vercel` then run `vercel dev` instead of `npm run dev`

### Step 5 — Push to GitHub
```bash
git init
git add .
git commit -m "Initial FuelPlan Pro deploy"
# Create a repo on github.com, then:
git remote add origin https://github.com/YOUR_USERNAME/fuelplan-pro.git
git push -u origin main
```

### Step 6 — Deploy to Vercel
1. Go to https://vercel.com and sign up (free)
2. Click **"Add New Project"**
3. Import your GitHub repo
4. Vercel auto-detects Vite — click **Deploy**

### Step 7 — Add your API key to Vercel (CRITICAL)
1. In Vercel dashboard → your project → **Settings → Environment Variables**
2. Add:
   - Name: `ANTHROPIC_API_KEY`
   - Value: your real `sk-ant-...` key
   - Environment: Production + Preview + Development
3. Click **Save**
4. Go to **Deployments** → **Redeploy** (so it picks up the new env var)

### Step 8 — (Optional) Lock down CORS
In Vercel env vars, also add:
- Name: `ALLOWED_ORIGIN`
- Value: `https://your-app-name.vercel.app`

This prevents other sites from using your API proxy.

---

## Your app is now live! 🎉

Vercel gives you a URL like: `https://fuelplan-pro-abc123.vercel.app`

---

## What changed from the prototype

| Before | After |
|--------|-------|
| API key exposed in browser JS | ✅ API key on server only |
| Direct calls to api.anthropic.com | ✅ Calls go through /api/claude proxy |
| Anyone could steal your API key | ✅ Key is never in client code |

---

## Next steps after deployment

- **Custom domain**: Vercel dashboard → Settings → Domains → Add your domain
- **PWA (Option B)**: Add manifest.json + service worker — ask Claude to add this
- **Add fuelplan-deep.jsx**: The hyper-local cuisine module can be added as a second tab
- **7-day plan**: Extend generatePlan() to produce 7 days instead of 3
- **Analytics**: Vercel has built-in analytics (Settings → Analytics)

---

## Troubleshooting

**"API key not configured on server"**
→ You forgot Step 7. Add ANTHROPIC_API_KEY in Vercel env vars and redeploy.

**App loads but Claude doesn't respond**
→ Check Vercel → Functions → Logs for errors

**Local dev proxy not working**
→ Use `vercel dev` instead of `npm run dev` — Vercel CLI handles the /api/ routing locally

---

*Generated: March 23, 2026 | FuelPlan Pro v1.0.0*
