# Deploy New Site — GitHub Repo to Cloudflare Pages Subdomain

Reusable checklist for deploying a new GitHub repo as a subdomain on an existing Cloudflare-managed domain. Worked example throughout: repo `hs-cardgame` deployed to `cards.thehueshift.com`.

## Prerequisites
- Repo pushed to GitHub (user `galnova`) with `index.html` at the repo root (Pages 404s at root if the entry file isn't named `index.html`)
- Target domain is already a zone in your Cloudflare account (`thehueshift.com`, `greyvoth.com`, `godsvmen.com`)

## Part 1 — Create the Pages project
1. Cloudflare dashboard → **Workers & Pages** → **Create** → **Pages** tab (not Workers — if you land on "Create a Worker," click **Looking to deploy Pages?** at the bottom)
2. **Import an existing Git repository** → select `hs-cardgame`
3. If the repo isn't listed: click the **Cloudflare Pages** GitHub app link → GitHub → Settings → **Repository access** → **Select repositories** → add `hs-cardgame` → **Save** (the green button — closing the dropdown alone doesn't apply it). Return to Cloudflare and refresh.
4. Build settings for plain HTML/CSS/JS: Framework preset `None`, build command empty, output directory `/`
5. **Save and Deploy** → verify `hs-cardgame.pages.dev` loads (first visit may 404 for 1-2 min while it propagates)

## Part 2 — Attach the custom domain
1. Project → **Custom domains** tab → **Set up a custom domain**
2. Enter the full subdomain with the exact registered spelling: `cards.thehueshift.com` (not `cards.hueshift.com` — a typo triggers a "transfer your DNS" screen; if you see that screen, the domain is wrong, go back and fix it)
3. Confirm the CNAME preview (`cards.thehueshift.com` → `hs-cardgame.pages.dev`) → **Activate domain**
4. Wait 1-5 min for status = **Active** (up to 15 min; if it sticks longer, remove and re-add the domain)
5. Test `https://cards.thehueshift.com`

## Part 3 — Ongoing workflow
- Every merge to `main` auto-deploys — no manual uploads
- Optional: add a nav link on the parent site (`thehueshift.com`) pointing to the new subdomain

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| Repo not in the import list | GitHub app access not saved | Re-open the GitHub app's repository access settings, add the repo, click the green **Save** button |
| Root URL 404s | Entry file isn't named `index.html` | Rename the entry file to `index.html` at the repo root |
| Page loads unstyled | Asset paths broken (absolute vs. relative, or output dir mismatch) | Check `<link>`/`<script>` paths are relative, confirm output directory is `/` |
| "Transfer your DNS" screen | Subdomain typo doesn't match the registered zone | Go back, re-enter the exact domain spelling as it appears in the Cloudflare zone list |
| Domain stuck "Initializing" | SSL cert still provisioning | Wait up to 15 min; if stuck longer, remove the custom domain and re-add it |
