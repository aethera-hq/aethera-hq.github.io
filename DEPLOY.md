# Deployment runbook — aetheraconsulting.com

Host: GitHub Pages. Org: `aethera-hq`. Repo: `aethera-hq.github.io`.
DNS: GoDaddy (Wild West Domains, reached through Tailor Brands).

Work top to bottom. Phases 0–1 touch no DNS at all.

---

## Rollback values — record before starting

If anything goes wrong, restoring these returns the domain to its
current state. Apex TTL is 600 seconds, so recovery takes ~10 minutes.

| Record | Type | Current value |
|---|---|---|
| `@` | A | `76.223.67.189` |
| `@` | A | `13.248.213.45` |
| `www` | CNAME | `aetheraconsulting.com` |

## Never touch these

| Record | Type | Value | Why |
|---|---|---|---|
| `@` | MX | `smtp.google.com` (priority 0) | Workspace email. Editing this stops mail. |
| `@` | TXT | `google-site-verification=nKnWEMPtLjHo0Z7XTvHgxXNVuQKtul6TFVXgGcBn2Ek` | Google domain verification |
| `@` | TXT | `google-site-verification=U3-3XrtQuGGnMu6RDXX_8p0-iEdfuShSHYVkSKntn_s` | Google domain verification |

No other subdomains exist on this domain — 19 common names were probed
and all were empty — so nothing else can be disturbed.

---

## Phase 0 — pre-flight

1. In the GoDaddy / Tailor Brands DNS panel, confirm **Forwarding is OFF**.
   Forwarding silently overrides A records and will send visitors elsewhere
   no matter what is configured below.
2. Confirm the page currently at `aetheraconsulting.com` is disposable.
   It disappears in Phase 3.

## Phase 1 — GitHub. No DNS impact; fully reversible.

3. Create the organization at <https://github.com/organizations/new>.
   Name: `aethera-hq`. Plan: **Free**.
   (Browser only — GitHub's API cannot create organizations.)

4. Create the repository and push:

       cd /Users/inigomerino/work/aethera/website
       gh repo create aethera-hq/aethera-hq.github.io --public --source=. --remote=origin --push

5. Enable Pages:

       gh api -X POST repos/aethera-hq/aethera-hq.github.io/pages \
         -f 'source[branch]=main' -f 'source[path]=/'

6. Wait ~2 minutes, then confirm <https://aethera-hq.github.io/> loads.

**This is the temporary URL. Stop here if photographs are not ready.**

## Phase 2 — domain verification. Additive only; cannot break anything.

7. GitHub → your org → Settings → Pages → **Add a domain** →
   enter `aetheraconsulting.com`. GitHub returns a TXT challenge.
8. In GoDaddy, **add** (do not edit anything):

   | Type | Name | Value |
   |---|---|---|
   | TXT | `_github-pages-challenge-aethera-hq` | *(value GitHub gives you)* |

9. Back in GitHub, click **Verify**.

## Phase 3 — DNS cutover. The only step carrying risk.

Edit the two existing A records rather than deleting them, so the domain
is never left with zero A records.

10. **Edit** apex A `76.223.67.189` → `185.199.108.153`
11. **Edit** apex A `13.248.213.45` → `185.199.109.153`
12. **Add** apex A `185.199.110.153`
13. **Add** apex A `185.199.111.153`
14. **Edit** `www` CNAME → `aethera-hq.github.io`
15. **Add** SPF, so replies to parents are less likely to land in spam:

    | Type | Name | Value |
    |---|---|---|
    | TXT | `@` | `v=spf1 include:_spf.google.com ~all` |

For roughly ten minutes after this the domain returns a GitHub 404.
That is expected and clears at step 16.

## Phase 4 — claim the domain

16. Repo → Settings → Pages → **Custom domain** → `aetheraconsulting.com` → Save.
    GitHub commits a `CNAME` file to the repo.
17. Locally, take that commit so the next push is not rejected:

        git pull

18. Confirm <https://aetheraconsulting.com> serves the site.
19. Once the certificate is issued (up to 24 hours), tick **Enforce HTTPS**
    in the same settings pane.

## Phase 5 — confirm nothing broke

20. `dig +short MX aetheraconsulting.com` → must still be `0 smtp.google.com.`
21. Send a test email to `info@aetheraconsulting.com` and confirm it arrives.
22. Confirm the old Google Sites page still loads. It is hosted on
    `sites.google.com` and is unaffected by any of the above.

---

## Rollback

1. Restore the two apex A records from the table at the top; delete the
   four GitHub addresses.
2. Restore `www` CNAME → `aetheraconsulting.com`.
3. Repo → Settings → Pages → clear the custom domain.
4. Delete the `CNAME` file from the repo if it was committed.

Effective in ~10 minutes at the current TTL.

---

## Before the site is genuinely public

- Replace the seven photo placeholders; obtain parental media consent
  for any image containing students.
- Remove `<meta name="robots" content="noindex, nofollow">` from
  `index.html` and the `Disallow: /` line from `robots.txt`.
- GitHub Pages may not be used for sites primarily facilitating
  commercial transactions. A brochure site with an email link is fine;
  if paid enrollment or booking is added later, move to Cloudflare Pages
  or Netlify.
