# Deployment Guide — Dr Ahmed Portfolio Website

The portfolio is a single `index.html` file — pure HTML/CSS/JavaScript with no build step required.
Deploy in under 5 minutes to any of the three platforms below.

---

## Option 1: GitHub Pages (Free, fastest)

1. Create a new GitHub repository (e.g., `dr-ahmed-portfolio`)
2. Copy the contents of the `website/` folder to the repo root
3. Also copy any downloadable assets to the repo root:
   - `AI_Workflow_Engineer_Portfolio_DrAhmed.docx`
   - `Portfolio_DrAhmed_AI_Workflow_Engineer.pptx`
   - `RESUME_BULLETS.md`
4. In GitHub repo → **Settings** → **Pages**
5. Source: **Deploy from a branch** → branch: `main` → folder: `/ (root)`
6. Click Save
7. Your site will be live at: `https://<your-username>.github.io/<repo-name>/`

**Custom domain (optional):**
- Add a `CNAME` file containing your domain name (e.g., `ahmed.dev`)
- Point your domain's DNS A records to GitHub's IPs: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`

---

## Option 2: Vercel (Recommended for best performance)

```bash
# Install Vercel CLI
npm install -g vercel

# From the website/ directory:
cd website/
vercel

# Follow the prompts:
# - Set up and deploy? Yes
# - Which scope? (your account)
# - Link to existing project? No
# - Project name: dr-ahmed-portfolio
# - Directory: ./
# - Override settings? No

# Production deploy:
vercel --prod
```

Your site will be live at `https://dr-ahmed-portfolio.vercel.app` (or custom domain).

**Custom domain on Vercel:**
1. Vercel Dashboard → Your project → Settings → Domains
2. Add your domain
3. Follow DNS configuration instructions

---

## Option 3: Netlify (Drag-and-drop deploy)

**Method A — Drag & drop (no CLI needed):**
1. Go to [app.netlify.com](https://app.netlify.com)
2. Drag the `website/` folder onto the deploy area
3. Done — live in ~30 seconds

**Method B — Git-connected:**
1. Push the `website/` folder to a GitHub repo
2. Netlify Dashboard → **Add new site** → **Import an existing project**
3. Select GitHub → Select your repo
4. Build command: (leave empty)
5. Publish directory: `.`
6. Click **Deploy site**

---

## Updating the download links

The download buttons in the website link to:
- `AI_Workflow_Engineer_Portfolio_DrAhmed.docx` (relative path)
- `Portfolio_DrAhmed_AI_Workflow_Engineer.pptx` (relative path)
- `RESUME_BULLETS.md` (relative path)

Place these files in the same directory as `index.html` and they will download correctly.

To use absolute URLs (e.g., from Google Drive):
Open `index.html` and replace the `href` values in the `.download-card` section with your direct download links.

---

## PDF export from website

To export the portfolio website as PDF:
1. Open `index.html` in Chrome
2. Press `Ctrl+P` (or `Cmd+P`)
3. Select **Save as PDF**
4. In **More settings**: Margins → None, Background graphics → On
5. Click Save

For a better PDF export, use [Puppeteer](https://pptr.dev/):
```bash
npm install puppeteer
node -e "
const puppeteer = require('puppeteer');
(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('file:///path/to/index.html', {waitUntil:'networkidle0'});
  await page.pdf({path:'portfolio.pdf', format:'A4', printBackground:true, margin:{top:'0',right:'0',bottom:'0',left:'0'}});
  await browser.close();
})();
"
```

---

## File structure for deployment

```
website/                          ← deploy this folder
├── index.html                    ← main portfolio page
├── vercel.json                   ← Vercel config
├── netlify.toml                  ← Netlify config
├── _config.yml                   ← GitHub Pages config
├── DEPLOY.md                     ← this file
├── AI_Workflow_Engineer_Portfolio_DrAhmed.docx   ← download asset
├── Portfolio_DrAhmed_AI_Workflow_Engineer.pptx   ← download asset
└── RESUME_BULLETS.md             ← download asset
```

---

## Performance notes

- The website loads a Google Fonts stylesheet (Inter + JetBrains Mono). If you need fully offline operation, replace the `<link>` tag with system fonts.
- No JavaScript frameworks — the page loads in under 1 second on any connection.
- No cookies, no analytics, no tracking by default. Add your own analytics snippet before the `</body>` tag if needed.
