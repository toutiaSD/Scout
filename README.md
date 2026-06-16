[README.md](https://github.com/user-attachments/files/29021938/README.md)
# Scout — Self-Updating Job Dashboard

Fetches jobs from 55+ companies and job boards every 6 hours via GitHub Actions.
AI-scores each listing against your profile. Dashboard auto-refreshes.

## Setup (10 minutes)

### 1. Create a GitHub repo
Go to github.com → New repository → name it `scout` → Create (keep it public for free GitHub Pages).

### 2. Upload these files
Your repo structure should look like this:
```
scout/
├── .github/
│   └── workflows/
│       └── fetch-jobs.yml
├── data/
│   └── jobs.json          ← created automatically on first run
├── dashboard/
│   └── index.html
├── fetch_jobs.py
└── README.md
```

Upload all files via GitHub's web UI (drag and drop) or via git:
```bash
git init
git remote add origin https://github.com/YOUR_USERNAME/scout.git
git add .
git commit -m "init"
git push -u origin main
```

### 3. Add your Anthropic API key
- Go to your repo → Settings → Secrets and variables → Actions
- Click "New repository secret"
- Name: `ANTHROPIC_API_KEY`
- Value: your key from console.anthropic.com
- Click "Add secret"

Without this, jobs still fetch but AI scoring is disabled (all scores show 50).

### 4. Enable GitHub Pages
- Go to your repo → Settings → Pages
- Source: Deploy from a branch
- Branch: `main` / folder: `/dashboard`
- Save

Your dashboard will be live at:
`https://YOUR_USERNAME.github.io/scout/`

### 5. Trigger the first run
- Go to your repo → Actions → "Fetch Jobs" → "Run workflow"
- Wait ~3 minutes for it to complete
- Reload your dashboard — jobs will appear

After that, it runs automatically at 06:00, 12:00, 18:00, and 00:00 UTC (08:00, 14:00, 20:00, 02:00 Brussels time).

## Customising

### Add a company
In `fetch_jobs.py`, add to the `WATCHLIST` array:
```python
{"name": "Company Name", "ats": "greenhouse", "token": "their-greenhouse-token"},
```
ATS options: `greenhouse`, `lever`, `ashby`, `bamboo`, `custom`

For custom pages, use:
```python
{"name": "Company Name", "ats": "custom", "url": "https://company.com/careers"},
```

### Change fetch frequency
In `.github/workflows/fetch-jobs.yml`, edit the cron line:
```yaml
- cron: "0 6,12,18,0 * * *"   # 4x per day
- cron: "0 */4 * * *"          # every 4 hours
- cron: "0 8 * * *"            # once per day at 8am UTC
```

## Sources
- **Greenhouse ATS**: Anthesis, Braze, Climate Impact Partners, First Street, Healthie, Multiplier, Position Green, Sojern, Sylvera, Synthesia
- **Lever ATS**: Carbon180, Datamaran
- **Ashby ATS**: Collaborative Earth, FarmRaise, Isometric, Paces
- **BambooHR**: C40 Cities
- **Custom pages**: Veolia, hub.brussels, ERM, Calvert Impact, Climate United, Just Climate, Pyxera Global, Travalyst, Villars Institute, Allied Climate Partners, Blue Earth Capital, Energize Capital
- **Job boards**: Remotive, WeWorkRemotely, CTVC, ClimateTechList, EuroBrussels
