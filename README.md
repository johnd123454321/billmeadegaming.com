# billmeadegaming.com

The official Billmeade Gaming website. Plain static HTML/CSS — no build step, no
dependencies, no framework. Edit a file, refresh, done.

## Structure

```
index.html                                  Home
games/index.html                            All games
games/self-made/index.html                  SELF MADE: Investment Life Sim
games/federal-reserve-simulator/index.html  Federal Reserve Simulator
games/federal-reserve-simulator/privacy/    FRS privacy policy (App Store URL)
games/federal-reserve-simulator/support/    FRS support page (App Store URL)
games/yolo-mode/index.html                  YOLO Mode
about/index.html                            Studio story
press/index.html                            Press kit / factsheet
404.html                                    Not-found page (GitHub Pages picks it up)
assets/css/site.css                         The one stylesheet
assets/img/…                                Capsule art, brand images
CNAME                                       Custom-domain marker for GitHub Pages
```

Every page is self-contained (header/footer are duplicated per page). To change
nav or footer everywhere, find-and-replace across the html files.

## Preview locally

```bash
cd ~/Desktop/Github/billmeadegaming.com && python3 -m http.server 4173
```

Then open http://localhost:4173 — root-absolute paths (`/assets/...`) require
serving over http; opening files directly with `file://` will look unstyled.

## Deploy (GitHub Pages — recommended, free)

1. Create the repo and push:
   ```bash
   cd ~/Desktop/Github/billmeadegaming.com
   git add -A && git commit -m "Initial site"
   gh repo create billmeadegaming.com --public --source=. --push
   ```
2. On GitHub → repo **Settings → Pages** → Source: **Deploy from a branch**,
   Branch: `main` / root. The `CNAME` file tells Pages the custom domain.
3. In the same Pages settings, enter `billmeadegaming.com` as the custom domain
   and (after DNS propagates) check **Enforce HTTPS**.

### Porkbun DNS records

In Porkbun → Domain Management → billmeadegaming.com → **DNS Records**.
First delete Porkbun's default parked-page records (the ALIAS/CNAME pointing at
`pixie.porkbun.com`), then add:

| Type  | Host | Answer                    |
|-------|------|---------------------------|
| A     | (blank / @) | 185.199.108.153    |
| A     | (blank / @) | 185.199.109.153    |
| A     | (blank / @) | 185.199.110.153    |
| A     | (blank / @) | 185.199.111.153    |
| CNAME | www  | johnd123454321.github.io  |

DNS usually propagates in minutes but can take up to an hour. Verify with
`dig billmeadegaming.com +short` — it should list the four 185.199.x.153 IPs.

### Updating the live site

```bash
git add -A && git commit -m "Update site" && git push
```

Pages redeploys automatically in about a minute.

## Adding a new game

1. Copy an existing folder, e.g. `games/yolo-mode/` → `games/<new-slug>/`.
2. Update title/meta/copy/art in its `index.html`.
3. Drop capsule art into `assets/img/<new-slug>/` (616×353 for cards).
4. Add a card to `index.html` + `games/index.html`, a footer link on each page
   (optional), and a `<url>` entry in `sitemap.xml`.

## Notes

- Prices/dates on the site are the launch values; if a game's Steam price
  changes, update the game page, the cards, and `press/index.html`.
- The FRS support + privacy URLs are stable and suitable for the App Store
  listing fields (replacing the temporary chatgpt.site URLs):
  - `https://billmeadegaming.com/games/federal-reserve-simulator/support/`
  - `https://billmeadegaming.com/games/federal-reserve-simulator/privacy/`
- No invented studio branding: the favicon is a plain typographic "B", and
  social-share (og) images use the games' real capsule art. The FRS card/hero
  crops come from the game's own resources.
