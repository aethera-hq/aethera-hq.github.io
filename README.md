# Aethera Innovation Lab — website

Single-page marketing site. Plain HTML and CSS, no build step, no dependencies.

## Editing

- `index.html` — all page content and copy
- `styles.css` — all styling; design tokens are at the top under `:root`
- `assets/` — photographs go here

To preview locally, open `index.html` in a browser, or run:

    python3 -m http.server 8000

## Adding photographs

Live images use `<img class="shot shot-*">`. The `shot-*` class sets the
aspect ratio and `object-fit: cover` handles the crop, so one file works
at every screen width:

    <img class="shot shot-hero" src="assets/hero-field.jpg"
         width="1600" height="1200" alt="...">

Ratios: `shot-hero` 4:3, `shot-tall` 3:4, `shot-video` 16:9.

Export at roughly 2x display size, JPEG quality 60, and aim to keep each
file under 250KB.

**Still outstanding:** the four mentor portraits, currently typographic
monogram tiles (`<figure class="monogram">`). Replace each with:

    <img class="shot shot-portrait" src="assets/portrait-michael.jpg" alt="...">

and add `.shot-portrait { aspect-ratio: 1/1; margin-bottom: 1.15rem; }`
to the stylesheet. Shoot all four in the same place and light, or the
row reads as a collage.

The video slot holds a still. To use real footage, swap the `<img>` for
`<video class="shot shot-video" controls poster="...">`.

**Before publishing photographs of students, get signed parental media
consent.** The three images in use were chosen partly because nobody in
them is identifiable.

## Deploying to Cloudflare Pages

1. Push this repository to GitHub.
2. In the Cloudflare dashboard: Workers & Pages → Create → Pages → connect the repo.
3. Framework preset: **None**. Build command: leave empty. Output directory: `/`.
4. Custom domains → add `aetheraconsulting.com` and `www.aetheraconsulting.com`.

Google Workspace email keeps working as long as the existing Google MX records are
preserved. If the domain's nameservers move to Cloudflare, re-add those MX records
there before the change propagates.

## Notes

- Colors, type and spacing all derive from the tokens in `:root`. Change them there.
- The season log animates on scroll; `prefers-reduced-motion` is respected, and the
  page renders fully without JavaScript.
- The footer carries a FIRST trademark disclaimer. Do not remove it, and do not add
  FIRST, FLL or FTC logos to the site without checking FIRST's branding guidelines.
