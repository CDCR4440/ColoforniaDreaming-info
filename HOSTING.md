# Hosting ColoforniaDreaming.info

*Researched Oct 21, 2026. DNS values come from each provider's official docs, linked below. Check the values in the provider's dashboard before you enter them, since the dashboard shows the exact target for your site.*

## The situation

- The wiki is a **static site**: plain HTML/CSS/JS in `site/` (zipped as `coloforniadreaming_info_site.zip`). It needs no database and no server code.
- The domain is registered at **GoDaddy**. John makes the DNS changes himself in GoDaddy → My Products → coloforniadreaming.info → **DNS**.
- **Before you start:** remove any GoDaddy *forwarding* or *parked-page* records on the .info domain. Also delete the default `@` A record that points at GoDaddy parking. Each host below needs that slot. GitHub's docs: "If your DNS provider automatically sets a default record, remove it before continuing."

## Option 1: GitHub Pages ✅ **Recommended**

**Cost:** free for public repositories. **Deploy:** `mkdocs gh-deploy` (built into MkDocs) or a GitHub Actions workflow. HTTPS is free, via the "Enforce HTTPS" checkbox.

**Steps:** create a repo (e.g., `coloforniadreaming-wiki`) → push the project → Settings → Pages → Custom domain: `coloforniadreaming.info` → Save. The project already includes `docs/CNAME`. Then add these records in GoDaddy:

| Type | Name (Host) | Value |
|---|---|---|
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| AAAA *(optional, IPv6)* | `@` | `2606:50c0:8000::153` |
| AAAA *(optional)* | `@` | `2606:50c0:8001::153` |
| AAAA *(optional)* | `@` | `2606:50c0:8002::153` |
| AAAA *(optional)* | `@` | `2606:50c0:8003::153` |
| CNAME | `www` | `<github-username>.github.io` *(no repo name)* |

GitHub's notes: add the custom domain in repo settings **before** you change DNS (this prevents domain takeover). Don't use wildcard (`*`) records. Verifying the domain in your GitHub account settings is recommended. DNS can take up to 24 hours, and the HTTPS option may take up to 24 hours to appear.
Source: <https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site>

**Why it's the pick:**

- It's free, and MkDocs deploys to it natively.
- Every page edit is a versioned commit, which gives the wiki the history and rollback a real wiki has.
- Once John connects GitHub (a GitHub connector is available to Tim), Tim can publish new and updated articles directly. John never has to touch the files, and DNS is set once and left alone.

## Option 2: Netlify (easiest no-code alternative)

**Deploy:** drag-and-drop the `site/` folder or zip in the Netlify dashboard, or connect a Git repo. Free starter plan. In Netlify, add the domain first, then add these in GoDaddy:

| Type | Name (Host) | Value |
|---|---|---|
| A *(fallback for providers without ALIAS/ANAME/flattened CNAME)* | `@` | `75.2.60.5` |
| ALIAS/ANAME *(only if GoDaddy offers it; Netlify prefers it)* | `@` | `apex-loadbalancer.netlify.com` |
| CNAME | `www` | `<your-site>.netlify.app` |

Netlify automatically adds both the apex and `www` and recommends `www` as the primary domain when DNS is hosted outside Netlify. Propagation can take up to a day.
Source: <https://docs.netlify.com/manage/domains/configure-domains/configure-external-dns/>

## Option 3: Cloudflare Pages (best if the .info DNS ever moves to Cloudflare)

- **Apex domain (`coloforniadreaming.info`)** needs the domain to be a **Cloudflare zone**. That means changing the domain's **nameservers** at GoDaddy to Cloudflare's, which moves all DNS management for that domain to Cloudflare. Cloudflare then creates the record automatically.
- **Subdomain only** (e.g., `wiki.coloforniadreaming.com`) works without moving nameservers: `CNAME  wiki → <project>.pages.dev`. Add the domain in the Pages dashboard *first*, or the site will return a 522 error.
- Supports direct upload of the built site.
Source: <https://developers.cloudflare.com/pages/configuration/custom-domains/>

This is a good platform, but for the bare `.info` domain it's a bigger change than adding a few records.

## Recommendation

**Use GitHub Pages:** 4 A records plus 1 `www` CNAME in GoDaddy, set once. It's free, versioned, and Tim can publish updates once GitHub is connected. If John would rather not have a GitHub account, use **Netlify** (1 A record plus 1 CNAME, with drag-and-drop uploads).

## Should it become a "true" editable wiki later?

Not yet. A static wiki has no logins, no database, no server to patch and nothing to hack, and it costs nothing. A true wiki engine makes sense once **other people** (contributors, moderators) need to edit pages in a browser:

- **Wiki.js:** modern UI, Markdown, page history and permissions. It needs a server running Node.js and a database (PostgreSQL recommended), so plan on a small paid VPS plus upkeep. It can sync content to and from a Git repo, so today's Markdown pages could be imported. <https://js.wiki/>
- **DokuWiki:** older, very lightweight PHP wiki that stores pages as plain text files, with no database. It runs on cheap PHP hosting, including many shared hosts. <https://www.dokuwiki.org/>

**Middle path (recommended next step):** keep MkDocs, and take reader corrections and suggestions through X, email or a simple form. If the community grows, move the same Markdown into Wiki.js.

## Maintenance notes

- Build: `cd /workspace/wiki && .venv/bin/mkdocs build` → output in `site/`.
- Versions are pinned in `requirements.txt` (`mkdocs==1.6.1`, `mkdocs-material==9.7.7`). The Material team warns that the coming MkDocs 2.0 breaks plugins and themes, so **don't upgrade to MkDocs 2.x** without testing.
