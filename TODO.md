# TODO — lrxtechgroup-website

Living backlog. Check items off (or move to MEMORY.md as a dated entry) as they're
done — don't just accumulate; keep this reflecting real, current state.

## Open questions (need user input, not yet actionable)

- [ ] Scope of any future "LRX One" work on this specific site was never
      pinned down (expand the existing product card? new dedicated page?
      something else?) — the user redirected LRX One work to the separate
      `lrxtechgroup/lrxone` repo instead. If this comes up again here,
      re-clarify rather than assuming.

## Fixed 2026-07-25

- [x] Footer `/privacy` link pointed at a page that doesn't exist. Not
      writing fabricated legal content — pointed it at a `mailto:`
      instead. Real Privacy Policy content still needs to be written (by
      the site owner) and linked once it exists.
- [x] Added favicon, `robots.txt`, and `sitemap.xml`.
- [x] Footer copyright year was hardcoded "© 2025" — made it dynamic via
      a small inline script.

## Fixed 2026-07-27

- [x] The "Coming Soon" second product card (`LRX | ___`) now has real
      content — replaced with LRX Billing, linking to the new
      `billing.html`. See MEMORY.md.
- [x] Both product cards (LRX One, LRX Billing) set to "Coming Soon" —
      neither is actually live/deployed yet. Nav CTA softened to match.
      See MEMORY.md.
- [x] `billing.html` given the same treatment (pricing CTAs were
      already fine, checked - but its own nav "LRX One →" button and
      missing "Coming Soon" badge weren't). See MEMORY.md.

## Note for later

- [ ] When LRX One and/or LRX Billing actually go live (real production
      infra deployed, real payment credentials configured), flip the
      relevant card's badge back and restore the direct signup CTA/link
      — don't forget this is currently deliberately understated.

## Not investigated yet

- [ ] No test/build/lint tooling exists for this repo (it's now two static
      HTML files, `index.html` + `billing.html`) — confirm that's
      intentional before adding any tooling unprompted.
