# orgsite — source for the organization GitHub Pages site

Staging copy of the content that is published as the organization site:

    https://schaeffler-monitoring-services-gmbh.github.io

The Pages site requires a repository named exactly `<org>.github.io`, i.e.
`Schaeffler-Monitoring-Services-GmbH.github.io`, owned by the organization.

## Files

| File | Purpose |
|---|---|
| `index.html` | The landing page. Self-contained: inline CSS, no external requests, no cookies, no tracking. |
| `.nojekyll` | Tells GitHub Pages to publish the files as-is (no Jekyll build). |
| `404.html` | Minimal not-found page linking back to the landing page. |

## Deliberate design constraints

- **No external resources at all** — no CDN, no web fonts, no analytics. Keeps
  the page inside the data boundaries and works without third-party requests.
- **No personal data** — only organization-level content; the contact address
  is the already public organization address.
- Content mirrors `.github/profile/README.md` so the org profile on GitHub and
  the Pages site do not drift apart.

## Publishing

Use `../scripts/setup-org-pages.sh` (idempotent, prints `[ok]`/`[FAIL]` per step).

**Precondition:** the organization currently has Pages creation *disabled*
(`members_can_create_pages=false`, set deliberately by
`scripts/apply-hardening.sh`). Enabling the site requires re-enabling that
policy — see `MANUAL-UI-STEPS.md`.

## Rollback

```sh
# remove the published site, keep the repository
gh api -X DELETE repos/Schaeffler-Monitoring-Services-GmbH/Schaeffler-Monitoring-Services-GmbH.github.io/pages

# remove the repository entirely (also removes the site)
gh repo delete Schaeffler-Monitoring-Services-GmbH/Schaeffler-Monitoring-Services-GmbH.github.io
```

Re-disable Pages creation afterwards to restore the previous hardening state:

```sh
gh api -X PATCH orgs/Schaeffler-Monitoring-Services-GmbH -F members_can_create_pages=false
```
