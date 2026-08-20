# smtghaus website — deployment guide

Everything you need to put this site live on your IONOS domain, hosted free on GitHub Pages, with a working contact form via Formspree.

## What's in this folder

- `index.html` — your site (already built)
- `assets/` — your real images (logo, team photo, 13 client logos). All good to go.
  - Note: `logo-mark-new.png`, `wordmark.png`, and `statement.jpg` are in this folder but not referenced anywhere in `index.html` — they won't show up on the site. If you meant to use them instead of `logo-mark.png` / `logo-stacked.png`, or add `statement.jpg` somewhere, let me know and I'll update the HTML to point to them.
- `CNAME` — tells GitHub Pages which custom domain to serve. Already set to `smtghaus.com`.

---

## Step 1 — Create the GitHub repo

1. Go to [github.com](https://github.com) and sign in (create a free account if you don't have one).
2. Click the **+** in the top right → **New repository**.
3. Name it whatever you like (e.g. `smtghaus-website`). Keep it **Public**. Don't add a README/gitignore. Click **Create repository**.
4. On the new repo's page, click **uploading an existing file**.
5. Drag in `index.html`, `CNAME`, and the whole `assets` folder (drag the folder in — GitHub keeps the folder structure).
6. Scroll down, click **Commit changes**.

*(If you're comfortable with git/terminal instead: `git init`, `git add .`, `git commit -m "initial site"`, `git remote add origin <your-repo-url>`, `git push -u origin main`.)*

## Step 2 — Turn on GitHub Pages

1. In your repo, go to **Settings** → **Pages** (left sidebar).
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Branch: `main`, folder: `/ (root)`. Click **Save**.
4. Wait ~1 minute, refresh — GitHub gives you a URL like `https://yourusername.github.io/smtghaus-website`. Open it to confirm the site loads (images will look like placeholders until you swap in real assets).

## Step 3 — Connect your custom domain

1. Still in **Settings → Pages**, find **Custom domain**. Type `smtghaus.com` and click **Save**. GitHub will also (re)write the `CNAME` file in your repo automatically — it should already say `smtghaus.com` since I set that for you.
2. Now go to **IONOS** → Domains & SSL → `smtghaus.com` → **DNS** settings, and add/edit these records (delete any conflicting default A or CNAME records first — IONOS often auto-creates a parked-page A record you need to remove):

   | Type | Host/Name | Points to | 
   |---|---|---|
   | A | @ (or blank, or `smtghaus.com`) | 185.199.108.153 |
   | A | @ (or blank, or `smtghaus.com`) | 185.199.109.153 |
   | A | @ (or blank, or `smtghaus.com`) | 185.199.110.153 |
   | A | @ (or blank, or `smtghaus.com`) | 185.199.111.153 |
   | CNAME | www | yourusername.github.io |

   (The four IPs are GitHub Pages' fixed servers — same for everyone. For the `www` CNAME, replace `yourusername` with your actual GitHub username — e.g. if your repo is at `github.com/janedoe/smtghaus-website`, use `janedoe.github.io`.)

3. DNS changes can take anywhere from a few minutes to a few hours to propagate.
4. Back in GitHub **Settings → Pages**, once it detects the DNS, tick **Enforce HTTPS** (may take a bit to become available — check back later if it's greyed out).

Your site is now live at your domain.

## Step 4 — Set up Formspree for the contact form

1. Go to [formspree.io](https://formspree.io) and sign up (free plan is fine to start).
2. Click **+ New Form**, give it a name (e.g. "smtghaus contact form"), set the email where submissions should land.
3. Formspree gives you a form endpoint like `https://formspree.io/f/xxxxabcd`. Copy that ID (`xxxxabcd`).
4. In `index.html`, find this line (around line 672):

   ```html
   <form class="contact-form" id="contactForm" action="https://formspree.io/f/REPLACE_WITH_YOUR_FORM_ID" method="POST">
   ```

   Replace `REPLACE_WITH_YOUR_FORM_ID` with your actual form ID.
5. Re-upload the updated `index.html` to GitHub (Add file → Upload files, or edit it directly in GitHub's web editor — click the pencil icon on the file).
6. Submit a test enquiry on your live site and confirm you receive the email. Formspree will ask you to confirm/verify the first submission via email — do that once.

---

## Quick checklist before launch

- [x] Real images in `assets/` (already done)
- [x] `CNAME` file contains `smtghaus.com` (already set)
- [ ] DNS records added at IONOS (4 A records + 1 CNAME for www)
- [ ] HTTPS enforced in GitHub Pages settings
- [ ] Formspree form ID plugged in and tested
- [ ] Updated copyright year / footer details if needed
