# Deploy to GitHub Pages + point wayfinderlawgroup.com at it

Total time: ~30 minutes of clicking, plus up to 24 hours of waiting for DNS to propagate.

---

## Part 1 — Get the site onto GitHub (10 min)

### 1. Create a GitHub account (skip if you have one)

Sign up at https://github.com/signup. Pick a username — this becomes part of your site's fallback URL (e.g., `mcummings.github.io`). Free tier is all you need.

### 2. Create a new repository

- Go to https://github.com/new
- **Repository name:** `wayfinderlawgroup.com` (or anything — name doesn't matter for the domain to work, but matching makes it tidy)
- **Public** (Pages on Free tier requires public — your site files would be visible to anyone, which is fine for a public website)
- Do **not** initialize with a README (you already have one)
- Click **Create repository**

### 3. Upload the files

Easiest path — drag & drop in the browser:

- On the new empty repo page, click **uploading an existing file**
- Open `C:\Users\FreeGeek\Dropbox\Wayfinder Law Group\Website Rebuild\site\` in File Explorer
- Select **all** files inside (`index.html`, `styles.css`, `CNAME`, `.nojekyll`, `README.md`, `DEPLOY.md`, plus the `assets/` folder)
- Drag them onto the GitHub upload area
- Scroll down, click **Commit changes**

> Note: `.nojekyll` is a hidden file. In File Explorer enable **View → Show → Hidden items** if you don't see it. If GitHub still doesn't show it after upload, that's OK — you can also create it directly in the GitHub UI by clicking **Add file → Create new file** and naming it exactly `.nojekyll` (with the leading dot).

### 4. Turn on GitHub Pages

- In your repo, click **Settings** (top right)
- Left sidebar: **Pages**
- Under **Build and deployment**:
  - **Source:** "Deploy from a branch"
  - **Branch:** `main` / `/ (root)` → **Save**
- Wait ~60 seconds, refresh. You'll see "Your site is live at `https://<username>.github.io/<reponame>/`"
- Click that link and confirm it loads

At this point your site is live on a github.io URL. Now we hook up the custom domain.

---

## Part 2 — Point wayfinderlawgroup.com at GitHub Pages

### 5. Find out who currently controls your DNS

You said the domain is at Squarespace. Squarespace acquired Google Domains in 2023, so your domain is most likely under **Squarespace Domains** at https://account.squarespace.com/domains. Log in there.

(If you registered the domain somewhere else — Namecheap, GoDaddy, Cloudflare — log in to that registrar instead. The DNS record steps are the same; only the UI differs.)

### 6. Decide: keep the domain at Squarespace, or move the registrar elsewhere?

**You don't have to move the registrar.** Cancelling Squarespace's *website builder* subscription is separate from your *domain registration*. You can keep the domain at Squarespace for ~$20/year and still host the site on GitHub Pages for free.

Two reasonable paths:

| Option | Cost | Notes |
|---|---|---|
| **A. Keep domain at Squarespace, just update DNS** | ~$20/yr for the domain | Simplest. Recommended for now. |
| **B. Transfer registrar to Cloudflare** | ~$10/yr at cost, no markup | Cheaper long-term, but adds a transfer step and a 7-day waiting period. Do this later if you want. |

The rest of this guide assumes **Option A**.

### 7. Update DNS records at Squarespace Domains

In Squarespace Domains, open your domain → **DNS** (or **DNS Settings**).

You need to set up **four A records** for the apex (`wayfinderlawgroup.com`) and **one CNAME** for `www`. Delete any existing A or CNAME records that conflict (Squarespace may have its own pointing at their servers — those go).

**Add these A records** (Host = `@`, sometimes shown blank or as the domain name):

| Type | Host | Value           | TTL    |
|------|------|-----------------|--------|
| A    | @    | 185.199.108.153 | 1 hour |
| A    | @    | 185.199.109.153 | 1 hour |
| A    | @    | 185.199.110.153 | 1 hour |
| A    | @    | 185.199.111.153 | 1 hour |

**Add one CNAME** for `www`:

| Type  | Host | Value                       | TTL    |
|-------|------|-----------------------------|--------|
| CNAME | www  | `<your-github-username>.github.io.` | 1 hour |

Replace `<your-github-username>` with your actual GitHub username — for example `mcummings.github.io.` (trailing dot is optional in most UIs).

Save changes.

### 8. Tell GitHub the custom domain

- Back at your repo → **Settings → Pages**
- Under **Custom domain**, enter `wayfinderlawgroup.com` → **Save**
- GitHub will run a DNS check. The first time it can say "DNS check in progress" for several minutes up to a few hours — that's normal.
- Once it's green, check the box **Enforce HTTPS** (may take an hour to become available — GitHub provisions a free SSL cert via Let's Encrypt).

> The `CNAME` file you already have in the repo tells GitHub the same thing. You may see the file edited automatically when you enter the domain in Settings — that's expected.

### 9. Wait for DNS to propagate

A records usually update in 15 min – 2 hours, but can take up to 24–48 hours worldwide. To check:

- https://www.whatsmydns.net/#A/wayfinderlawgroup.com — should show the four `185.199.*` IPs in most rows
- Open an incognito window and visit https://wayfinderlawgroup.com — should load your new site

### 10. Cancel the Squarespace website subscription

Once https://wayfinderlawgroup.com is loading your GitHub-hosted site cleanly:

- Squarespace account → **Billing & Account** → **Billing**
- Find the website plan (NOT the domain) and **Cancel subscription**
- Confirm the domain registration is on its own line item and stays active

If you accidentally cancel the domain registration instead of the hosting, the domain could lapse and someone else could grab it. Read the cancellation screen carefully — Squarespace will warn you what is being cancelled.

---

## Troubleshooting

**"Domain does not resolve to GitHub Pages servers"** in the Pages settings
→ DNS hasn't propagated yet. Wait 30 min and click "Check again."

**Site loads at `www.wayfinderlawgroup.com` but not the apex** (or vice versa)
→ Missing the A records for the apex, or missing the `www` CNAME. Go back to Step 7.

**Browser shows "Not Secure" / no padlock**
→ HTTPS cert is still provisioning. Wait an hour, then enable **Enforce HTTPS** in repo Settings → Pages.

**Old Squarespace site still showing**
→ Browser cache. Hard refresh (Ctrl+F5) or try incognito.

**You want to update the site later**
→ Edit `index.html` or `styles.css` directly on GitHub (pencil icon on any file → edit → Commit). Changes go live within a minute.
