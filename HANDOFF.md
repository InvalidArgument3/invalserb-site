# Handoff — www.invalserb.com

## What this repo is

Static one-page flyer for the Autisnautics 1.21 modpack and its Minecraft server.
Live on GitHub Pages. No build step, no framework, no CI.

- `index.html` — the whole page; all CSS inline in `<style>`, one inline `<script>` for the click-to-copy server address.
- `assets/icon.png` — pack icon, 512×512, copied from the Prism instance (`minecraft/icon.png`).
- `CNAME` — `www.invalserb.com`. GitHub Pages reads this and sets the custom domain; keep it.
- `.nojekyll` — stops Jekyll from filtering anything.

## State as of 2026-11-23

- Repo: `InvalidArgument3/invalserb-site`, default branch `main`.
- Pages: enabled, source = `main` / root. Last build: success.
- Pages custom domain: `www.invalserb.com` (from the CNAME file).
- HTTPS certificate for the custom domain: **not provisioned** — GitHub only issues it after DNS resolves to Pages.
- Public DNS: **not done**. `www.invalserb.com` still has Cloudflare-proxied records that answer nothing.

## Remaining step: the DNS record

Cloudflare zone `invalserb.com` (same zone as `gok.invalserb.com`):

| Type | Name | Content | Proxy |
|---|---|---|---|
| CNAME | `www` | `invalidargument3.github.io` | DNS only (grey cloud) |

Delete or edit the existing `www` records first (they are proxied A/AAAA records pointing at a dead origin).
Grey cloud matters: proxied plus GitHub Pages needs SSL mode Full and gives the origin GitHub's own cert anyway — DNS-only is what `www.ramshacklegame.com` uses and it just works.
After the record resolves, GitHub issues the Let's Encrypt cert on its own (usually within an hour). Then tick "Enforce HTTPS" in repo Settings → Pages.

Apex `invalserb.com` is not part of this repo. If it should redirect to `www`, that is a Cloudflare Redirect Rule, not a Pages thing.

## Redeploying

```bash
cd C:\Users\User\Desktop\autisnautics-site
# edit index.html
git add -A && git commit -m "..." && git push
```
Pages rebuilds in under a minute; check `gh api repos/InvalidArgument3/invalserb-site/pages/builds/latest`.

## Content rules

- Facts on the page come from the pack repo and server: NeoForge 21.1.265, MC 1.21.1, 416 client / 335 server jars, 830 quests / 20 chapters, server `136.51.7.220:25565`, voice UDP 24454.
- Pack version appears in three places: hero button, `<span id="build">`, and the specs table. Bump all three on release, along with the download link (which points at `/releases/latest`, so only the version text changes).
- Register: technical, no marketing adjectives. One fact per sentence.
- Style is intentionally close to www.ramshacklegame.com: dark navy (`#040913` / `#0b1320`), blue accent `#4fb3ff`, Trebuchet MS, uppercase letter-spaced section labels. That site is Hugo (`sterikworks/ramshacklegamesite`, theme `hugo-landing-sections` from Codeberg); this one is deliberately plain HTML so there is nothing to build.

## Not done / ideas

- No screenshot or hero art yet — only the 512×512 pack icon. Real in-game screenshots would carry the page.
- No analytics, no favicon beyond the pack icon, no OG image (og:image is absent).
- No page for the server map, player list, or changelog; the page links to GitHub releases for that.
