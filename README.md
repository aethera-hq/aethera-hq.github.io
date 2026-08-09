# Aethera Innovation Lab — website

Single-page marketing site. Plain HTML and CSS, no build step, no dependencies.

## Editing

- `index.html` — all page content and copy
- `styles.css` — all styling; design tokens are at the top under `:root`
- `assets/` — photographs go here

To preview locally, open `index.html` in a browser, or run:

    python3 -m http.server 8000

## Adding photographs

Every photo slot is a `<figure class="frame ...">` placeholder. To use a real image,
replace the whole figure with an `img` tag and keep the sizing class:

    <img class="frame frame-hero" src="assets/build-table.jpg"
         alt="Two students fitting a gearbox at the build table">

Slots currently reserved: hero, workshop, competition pit, and four portraits.

There is also a 16:9 video slot in the "What we do" section for custom-competition
footage. Replace `<div class="frame frame-video">...</div>` with either a local file:

    <video class="frame-video" controls preload="metadata" poster="assets/comp-poster.jpg">
      <source src="assets/custom-competition-2026.mp4" type="video/mp4">
    </video>

or an iframe embed. Keep the `frame-video` class so the aspect ratio holds.

**Before publishing photographs of students, get signed parental media consent.**

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
