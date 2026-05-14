# EWS — Apocalypse Early Warning System

A mobile web app built on [Kyle McDonald’s EWS](https://ews.kylemcdonald.net) — tracks ~31,000 private jets via ADS-B transponder data and computes an emergency level (1–5) based on how many are airborne vs. the rolling 24-hour baseline.

## How it works

A GitHub Actions workflow fetches `dashboard.json` from `ews.kylemcdonald.net` every 30 minutes and commits it to `data/dashboard.json` in this repo. The app reads from that local file — no CORS issues, always live.

```
GitHub Actions (every 30 min)
  └── fetches ews.kylemcdonald.net/dashboard.json
       └── commits to data/dashboard.json
            └── GitHub Pages serves index.html
                 └── app reads ./data/dashboard.json ✅
```

## Setup

### 1. Create the repo

```bash
git init ews-app
cd ews-app
# copy all files here
git add .
git commit -m "init"
git remote add origin https://github.com/YOUR_USERNAME/ews-app.git
git push -u origin main
```

### 2. Enable GitHub Pages

- Go to **Settings → Pages**
- Source: **Deploy from a branch**
- Branch: `main` / `/ (root)`
- Save — your app will be live at `https://YOUR_USERNAME.github.io/ews-app`

### 3. Enable the workflow

- Go to **Actions** tab in your repo
- If prompted, click **“I understand my workflows, enable them”**
- The workflow runs automatically every 30 minutes
- You can also trigger it manually: **Actions → Fetch EWS Data → Run workflow**

### 4. Check it’s working

After the first workflow run, `data/dashboard.json` will be updated with a `fetchedAt` timestamp. The app shows **LIVE · HH:MM** in the header when reading real data.

## File structure

```
/
├── index.html                        # the app
├── data/
│   └── dashboard.json               # auto-updated by GitHub Actions
├── .github/
│   └── workflows/
│       └── fetch-data.yml           # the workflow
└── README.md
```

## Status indicators

|Status         |Meaning                                        |
|---------------|-----------------------------------------------|
|`LIVE · 14:30` |Real data fetched at 14:30                     |
|`STALE · 14:30`|Last fetch from EWS failed, showing cached data|
|`DEMO DATA`    |Couldn’t read `data/dashboard.json` at all     |

## Credits

Original concept and data pipeline by [Kyle McDonald](https://kylemcdonald.net).  
ADS-B data from [ADS-B Exchange](https://www.adsbexchange.com).
