# TODO — lrxtechgroup-website

Living backlog. Check items off (or move to MEMORY.md as a dated entry) as they're
done — don't just accumulate; keep this reflecting real, current state.

## Open questions (need user input, not yet actionable)

- [ ] Scope of any future "LRX One" work on this specific site was never
      pinned down (expand the existing product card? new dedicated page?
      something else?) — the user redirected LRX One work to the separate
      `lrxtechgroup/lrxone` repo instead. If this comes up again here,
      re-clarify rather than assuming.

## Found 2026-07-25 — code review pass, minor gaps only

- [ ] Footer links to `/privacy` — this page doesn't exist anywhere in the
      repo, and there's no routing/redirect layer (single static
      `index.html`). Either add the page or point the link elsewhere.
- [ ] No favicon, `robots.txt`, or `sitemap.xml`.
- [ ] Copyright year in the footer is hardcoded "© 2025" — will read as
      stale once well into 2026; consider making it dynamic or just
      remembering to bump it periodically.

## Not investigated yet

- [ ] No test/build/lint tooling exists for this repo (it's a single static
      HTML file) — confirm that's intentional before adding any tooling
      unprompted.
- [ ] The "Coming Soon" second product card (`LRX | ___`) has no real content
      — check with the user whether/when to fill it in.
