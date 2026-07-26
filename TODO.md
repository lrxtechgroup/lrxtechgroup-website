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

## Not investigated yet

- [ ] No test/build/lint tooling exists for this repo (it's a single static
      HTML file) — confirm that's intentional before adding any tooling
      unprompted.
- [ ] The "Coming Soon" second product card (`LRX | ___`) has no real content
      — check with the user whether/when to fill it in.
