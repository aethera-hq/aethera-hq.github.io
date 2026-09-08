# Handoff — Aethera Innovation Lab website

Read this before touching anything. It is written for a person or an agent
picking the work up cold.

- **Live:** <https://aetheraconsulting.com> (also `https://aethera-hq.github.io/`)
- **Repo:** `aethera-hq/aethera-hq.github.io`
- **Local:** `/Users/inigomerino/work/aethera/website`
- **Branch:** `main` (in sync with `origin/main`)
- **Deploy:** push to `main` → GitHub Pages rebuilds automatically (~40s)

## What this is

A single-page site for Aethera Innovation Lab, a selective STEM leadership
incubator in Sarasota, Florida. Plain HTML and CSS, no build step, no
dependencies. Open `index.html` and edit it.

The audience is **parents of ambitious students**, not schools. Schools and
nonprofits are a secondary audience.

## The positioning, and why it matters

The site went through three repositions and landed here: **Aethera is a
leadership incubator; competitive robotics is the instrument, not the
product.** FIRST is named as the backbone but explicitly not the boundary —
there is a section headed "FIRST is the backbone. It is not the boundary."

Do not let edits drift the page back toward reading as a FIRST mentoring
service. That was an explicit, repeated instruction from the owner.

## Rules that govern the copy

These are not stylistic preferences. Breaking them causes real damage.

1. **Every factual claim on the page is verifiable.** The FIRST statistics
   (109,000 students / 81 countries, 10 Leadership Award winners per season,
   83% of alumni in STEM majors, 200+ scholarship providers, $80M+) all come
   from `firstinspires.org`. The board members named — MIT Corporation
   executive committee, former Yale engineering deputy dean, WPI president —
   are real and checked. If you add a claim, verify it first.
2. **No fabricated proof.** No invented case studies, client logos, or
   testimonials. There are none on the page and there should be none until
   they are real.
3. **The admissions benefit is implied, never asserted.** The page states
   awards and scholarship facts plainly and lets the reader conclude. It
   never claims what admissions committees do. The "reference" passage is
   deliberately worded as a consequence, not a promise.
4. **FIRST trademarks.** The footer disclaimer must stay. Do not add FIRST,
   FTC or FLL logos, and do not imply endorsement.
5. **Photographs of minors need signed parental media consent.** The three
   images currently published were chosen partly because nobody in them is
   identifiable.

## Design system

Warm paper ground carrying a faint graph-paper grid, borrowed from the
engineering notebook — a real, judged artifact in FIRST.

- Tokens live at the top of `styles.css` under `:root`. Change them there.
- Type: **Fraunces** display (low optical size, high SOFT, WONK on — this is
  what keeps it from reading as a generic high-contrast serif), **Karla**
  body, **IBM Plex Mono** for labels and annotations.
- Colour: warm paper `#FDFAF6`, ink `#221F1B`, Gulf teal `#0F6E6B` primary,
  marigold `#DE8F35` used sparingly — only on the two peak moments in the
  season log and a few accents.
- The **season log** is the signature element: a dimensioned rail, 2015 →
  2028, with ticks like a measurement drawing. The 2028 entry uses a dashed
  rail because that season has not happened yet.
- The **selectivity section** (`#fit`) is the only dark section on the page.
  That contrast is deliberate — it is the pivot where families self-select
  out. Do not add a second dark section; it would spend the effect.

## Page structure

`hero` → `#why` → `#path` → `#work` → `#scope` → `#record` → `#learn` →
`#mentors` → `#fit` → `#contact`

Tinted "band" sections alternate with plain ones. If you insert a section,
keep the alternation.

## Done

Most recent first:

| Commit | What |
|---|---|
| `0b4f0bd` | First real photographs; mentor portraits became monogram tiles |
| `eafe5d1` | "numerous teams" → "more than 30 teams" |
| `3f31d14` | `CNAME` file (added by GitHub when the custom domain was set) |
| `3dac0c5` | Deployment runbook with rollback values |
| `584086e` | Favicons, social card, `robots.txt`, 404 page |
| `e536a32` | Decade of teams built for others; off-season competition detail |
| `086e03b` | Commitment set at 20+ hours/week year-round |
| `4883353` | Reposition — leadership incubator, FIRST as backbone not identity |
| `6084cec` | Reframe to leadership development; verified FIRST statistics |
| `4d7f9d3` | Warmed the display type; widened cards; fixed mobile hero crop |

Infrastructure that is finished: org created, repo published, Pages live,
domain verified with GitHub, DNS cut over, HTTPS enforced, SPF added,
Workspace email confirmed working end to end.

## Pending, in order

1. **Four mentor portraits** — Michael, Natalie, Tara, Inigo. Currently
   typographic monogram tiles. Shoot all four in the same place and light or
   the row reads as a collage. Inigo's carries the most commercial weight.
   Wiring instructions are in `README.md`.
2. **Go public.** Two edits and one deploy:
   - remove `<meta name="robots" content="noindex, nofollow">` from `index.html`
   - remove `Disallow: /` from `robots.txt`
   Both carry comments saying so. Do this only once the portraits are in.
3. **Name a team** behind "seasoned world-class competitors and coaches" in
   the off-season competitions card. The owner was not ready to commit to a
   name; it is the last soft claim on the page.
4. **Competition footage** for the video slot, currently a still image.
5. Optional: swap the social card for the shallow-focus game-ball photograph
   (`IMG_0096` in `~/Downloads/website/`). It is the best image in the set
   and is currently unused.

## How to work on it

```bash
cd /Users/inigomerino/work/aethera/website
python3 -m http.server 8000        # preview at http://localhost:8000
# edit index.html / styles.css
git add -A && git commit && git push origin main   # deploys automatically
```

Verify a deploy:

```bash
gh api repos/aethera-hq/aethera-hq.github.io/pages --jq '.status'
```

## Gotchas

- **Work happens on `main`.** This breaks the usual never-work-on-main rule,
  deliberately: GitHub Pages serves from `main`, and this is a one-page site
  with a single maintainer. `feat/initial-site` is stale at `584086e` and can
  be deleted.
- **`CNAME` file must stay.** Deleting it removes the custom domain. If you
  ever force-push or rebuild the tree, keep it.
- **DNS lives at GoDaddy** (Wild West Domains, via Tailor Brands), not
  Cloudflare and not Google. Nameservers are `ns49/ns50.domaincontrol.com`.
  The `MX` record `0 smtp.google.com` and both `google-site-verification` TXT
  records must never be touched — that is Workspace email and domain
  verification. Full rollback values are in `DEPLOY.md`.
- **Apex TTL is 600s**, so any DNS mistake is recoverable in ~10 minutes.
- **Screenshots via headless Chrome** have a ~485px minimum window width. A
  390px screenshot renders a 485px page and crops it, which looks like broken
  mobile layout but is not. Verify with `document.documentElement.scrollWidth`
  before believing it.
- **GitHub Pages forbids e-commerce.** A brochure site with an email link is
  fine. If paid enrollment or booking is added, move to Cloudflare Pages or
  Netlify.
