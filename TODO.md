# TODO — lrxtechgroup-website

Living backlog. Check items off (or move to MEMORY.md as a dated entry) as they're
done — don't just accumulate; keep this reflecting real, current state.

## Fixed 2026-07-29 — Arrows/em-dashes removed, LRX One wordmark two-toned on hero eyebrows

- [x] Removed the arrow from every "Register Interest" (and billing.html's
      equivalent "Talk to Us") link/button across `index.html`, `one.html`,
      `billing.html`.
- [x] Stripped "Register Interest" out of every `mailto:` subject line
      (static hrefs and the two tier-select JS builders) - the button
      label is unchanged, only the outgoing email subject.
- [x] Replaced every visible em-dash ("—") with a plain hyphen across all
      7 pages, including the CSS bullet-glyph `content: '—'` declarations
      before pricing/legal-doc list items. Left non-rendered CSS/JS
      comments alone.
- [x] `one.html`'s and `billing.html`'s hero-eyebrows now show "LRX One"
      in gold and the product name ("Core"/"Billing") in white, matching
      `index.html`'s existing product-card pattern. See MEMORY.md for why
      this wasn't extended to body-prose mentions or footer links, and for
      a real `.gold`-class scoping mistake caught before it shipped.
- [ ] **Not done, flagging rather than assuming**: body-prose mentions of
      "LRX One Core"/"LRX One Billing" throughout the site (hero
      descriptions, section-sub text, all 4 legal pages, footer link
      lists) are still plain-colored, not gold/white. If "throughout" was
      meant to include those too, say so and it's a quick follow-up -
      deliberately scoped out this pass to avoid gold text scattered
      through dense legal paragraphs or singling out one link from an
      otherwise-uniform footer list.

## Fixed 2026-07-28 (pricing CTA consolidation) — One register-interest control per page

- [x] User asked why one pricing card looked different — explained
      (not a bug): `.pricing-card.featured` is the intentional
      "recommended tier" highlight (Business on `one.html`, Growth on
      `billing.html`), lighter background + gold top border.
- [x] Removed every pricing card's own CTA link ("Register Interest" /
      "Talk to Us" / "Contact Sales") on both `one.html` and
      `billing.html` — 8 links total. Replaced with one consolidated
      control in each page's `cta-banner` section: a tier `<select>` +
      a single CTA button whose `mailto:` href is rebuilt via a small
      vanilla-JS listener whenever the selection changes, e.g.
      `mailto:sales@lrxtechgroup.com?subject=LRX%20One%20Core%20-%20Business%20-%20Register%20Interest`.
      No backend involved — still a pure static site, same `mailto:`
      pattern every other CTA on the site already uses, just aggregated
      into one place instead of one link per card.
- [x] Removed the now-fully-unused `.pricing-cta` / `.pricing-cta.primary`
      / `.pricing-cta.secondary` CSS rules from both pages (dead code
      once every usage was removed).
- [x] Verified interactively with Playwright (not just visually) —
      selected each tier in the dropdown on both pages and confirmed the
      mailto href updates correctly each time, plus re-ran the mobile
      overflow sweep (375/768/1440px) to confirm nothing broke.

## Fixed 2026-07-28 (follow-up) — Same redundancy was also in index.html's footer

- [x] The previous fix only removed "LRX One Billing" from the header
      nav; it was also sitting directly next to "Products" in the
      footer's link list. Removed it there too. Checked `one.html`'s
      footer (has "LRX One Billing" next to "Sign In", no "Products"
      link in that footer at all — not redundant, left alone) and
      `billing.html`'s footer (links to "LRX One", the login, not
      itself — not redundant either). Confirmed via grep that no other
      "Products"/"LRX One Billing" adjacency remains anywhere on the
      site.

## Fixed 2026-07-28 (user-reported) — Orphaned gold box in pricing grids + nav redundancy

- [x] User sent a real-device screenshot of `billing.html`'s pricing
      section showing an unexplained olive/gold box next to the
      Enterprise card. Root cause: `.pricing-grid` used
      `grid-template-columns: repeat(auto-fit, minmax(240px, 1fr))`
      with a `background: var(--gold-line)` + `gap: 1px` trick to draw
      thin gridlines — but with exactly 4 cards, any viewport width
      that computes 3 columns leaves 1 orphaned card and 2 empty grid
      cells in the last row, and the container's own gold background
      shows through those empty cells. Fixed on both `billing.html` and
      `one.html` (identical CSS, identical 4-card layout) by replacing
      `auto-fit` with explicit breakpoints (1 → 2 → 4 columns) that only
      ever produce column counts dividing 4 evenly, making the orphan
      state mathematically impossible.
- [x] Found the same bug pattern on `.solution-grid` (6 items, same
      gold-background-fill trick) on both pages while checking — this
      one was visible even on wide desktop (1300px), not just a mobile
      edge case. Fixed the same way: explicit 1 → 2 → 3 column
      breakpoints (6 divides evenly by 1, 2, 3, or 6, never 4 or 5).
      Verified with an automated sweep (8 widths × 2 pages × 2 grids,
      checking each grid's last item reaches the container's edge) —
      zero orphans anywhere now.
- [x] Second user report, same session: `index.html`'s header nav had
      "LRX One Billing" as its own standalone item alongside "Products"
      — redundant and asymmetric (Core was never in the nav on its own).
      Removed the standalone nav entry; "Products" already anchors to
      the section containing both product cards. Left the footer's
      "LRX One Billing" link alone — that's a normal footer deep-link,
      not what was flagged.

## Fixed 2026-07-28 (mobile audit) — Footer links overflowing at phone widths

- [x] User asked whether today's changes had been checked on mobile —
      they hadn't, only at a 1440px desktop viewport. Ran an automated
      overflow check (Playwright, comparing `scrollWidth` vs
      `clientWidth`) across all 7 pages on this site at 375px and
      768px. Found real horizontal overflow on `index.html`, `one.html`,
      and `billing.html` at 375px only (+291px / +244px / +164px) —
      `terms.html`/`privacy.html`/`refund-policy.html`/
      `cancellation-policy.html` were all clean.
- [x] Root cause: `.footer-links { display: flex; gap: 24px; }` was
      missing `flex-wrap: wrap` on these three pages (the four legal
      pages built from scratch this session had it correctly). Not
      a problem with the original ~5-link footer, but became one once
      today's work grew each footer to 8+ links (Terms, Privacy, Refund
      Policy, Cancellation Policy all added across earlier commits
      today) without checking mobile wrap behavior at the time. Added
      `flex-wrap: wrap` to all three. Re-verified: zero overflow at
      both widths on all 7 pages.

## Fixed 2026-07-28 (LRX One umbrella, part 3) — Pricing sections on both product pages

- [x] User asked to check `billing.html`'s pricing section specifically.
      Neither `billing.html` nor `one.html` had any suite mention in
      their `#pricing` sections — added a `.section-sub` line under each
      headline clarifying pricing is separate per product within the
      LRX One suite (fixed both for consistency, not just the one
      asked about). Verified with Playwright screenshots of both
      `#pricing` sections.

## Fixed 2026-07-28 (LRX One umbrella, part 2) — one.html and billing.html now mention the suite too

- [x] User asked to check `one.html`/`billing.html` for the same LRX One
      umbrella framing added to `index.html`'s products section —
      neither mentioned it (confirmed via grep: no "LRX One" standalone
      mention on either page beyond a single unrelated footer link).
      Added one sentence to each page's hero-desc: `one.html` now says
      LRX One Core "is part of LRX One, LRX Tech Group's product suite,
      alongside LRX One Billing"; `billing.html` says the mirror image.
      Verified with Playwright screenshots of both heroes.

## Fixed 2026-07-28 (LRX One umbrella) — Products section now names LRX One as the suite

- [x] `index.html`'s `#products` section showed LRX One Core and LRX One
      Billing as two independent cards with no unifying brand mention —
      confirmed via grep that "LRX One" (standalone) appeared nowhere on
      this page before this fix. Added an eyebrow ("LRX One — Our
      Product Suite") and a one-line intro above the two cards
      explaining both products are part of LRX One. User explicitly
      chose the light-touch option (add a header, don't merge the
      cards) over a bigger restructure. See MEMORY.md and
      `lrxone-website`'s own MEMORY.md for the full context.

## Fixed 2026-07-28 (footer follow-up) — Footer link ordering/completeness

- [x] `index.html`'s footer had Privacy out of order (after Contact
      instead of after Terms, unlike `one.html`/`billing.html`) — fixed.
- [x] `privacy.html`'s footer was missing Refund Policy and Cancellation
      Policy links that the other three legal pages all cross-link to
      each other with — added both. See MEMORY.md.
- [x] Confirmed `lrxone-website` has no `billing.html` to compare (it's
      a single-product sign-in site) and that its legal pages' shared
      minimal footer is internally consistent — nothing to fix there.

## Fixed 2026-07-28 (final one today) — Real privacy.html built

- [x] Built a real `privacy.html`, closing the gap flagged immediately
      below and the older one from 2026-07-25. Adapted from
      `lrxone-website`'s version, generalised to cover both products the
      same way the other three legal pages were, with the real
      Information Officer details (Brandon Le Roux / Jessica Le Roux)
      carried over unchanged since they were already accurate. See
      MEMORY.md.
- [x] Replaced every `mailto:...Privacy%20Policy%20Request` placeholder
      site-wide (`index.html`'s footer, and the "your data" sections of
      `terms.html` and `cancellation-policy.html`) with real
      `/privacy.html` links. Added a Privacy Policy link to every
      footer that didn't already have one (`one.html`, `billing.html`,
      `terms.html`, `refund-policy.html`, `cancellation-policy.html`).

## Fixed 2026-07-28 (even later still) — Terms, Refund, and Cancellation pages added

- [x] Built `terms.html`, `refund-policy.html`, `cancellation-policy.html`
      at the site root — this was a real PayFast account-verification
      requirement (see MEMORY.md), and none of the three existed on this
      site before (only `lrxone-website`, a separate repo, had them).
      Content adapted from `lrxone-website`'s versions but generalised to
      cover both products hosted here (LRX One Core and LRX One Billing)
      instead of being LRX One Core-specific.
- [x] Confirmed no "free tier" language exists anywhere on the site
      (already clean from the earlier STARTER-pricing pass) and added
      explicit "we don't offer a free tier" call-outs to the Refund and
      Cancellation pages so a reader can't assume otherwise.
- [x] Billing/finance-related contact points (refund requests,
      cancellation requests, the Refund/Cancellation pages' general
      contact sections) now route to `billing@lrxtechgroup.com` instead
      of `sales@lrxtechgroup.com`. Pre-sales inquiries (pricing-page
      CTAs like "Register Interest" / "Talk to Us" / "Contact Sales" on
      `index.html`/`one.html`/`billing.html`) were deliberately left on
      `sales@` — those are prospective-customer inquiries, not billing
      correspondence.
- [x] Linked all three new pages from the footers of `index.html`,
      `one.html`, and `billing.html`.
- [x] The mailto-placeholder gap flagged here is now fixed — see the
      entry above (real `privacy.html` built).

## Fixed 2026-07-28 (later still) — Growth/Business pricing cards updated (price-per-user ladder)

- [x] Matches `lrxone`'s billing-service price change (Growth R499→R899,
      Business R1,299→R2,299 — user caps unchanged). See MEMORY.md and
      `lrxone`'s own MEMORY.md for the full reasoning.

## Fixed 2026-07-28 (even later) — Enterprise pricing card updated to R6,499/mo

- [x] Matches `lrxone`'s billing-service price fix (R3,999 → R6,499,
      raised to bring AI cost back to a healthy ~43% of revenue). See
      MEMORY.md and `lrxone`'s own MEMORY.md for the full reasoning.

## Fixed 2026-07-28 (the actual last one today) — Full pricing table revised on one.html

- [x] All four tiers' pricing cards updated to match the real,
      margin-checked billing-service config — same prices, higher
      allowances, Starter now shows AI Assistant + Integrations as
      included. See `lrxone`'s own MEMORY.md for the full numbers and
      the cost model behind them.

## Fixed 2026-07-28 (very last one today) — STARTER pricing updated (no longer free)

- [x] `one.html`'s pricing card and section headline updated to match the
      real R199/mo Starter price. See MEMORY.md.

## Open questions (need user input, not yet actionable)

- [ ] Scope of any future "LRX One" work on this specific site was never
      pinned down (expand the existing product card? new dedicated page?
      something else?) — the user redirected LRX One work to the separate
      `lrxtechgroup/lrxone` repo instead. If this comes up again here,
      re-clarify rather than assuming.

## Fixed 2026-07-25

- [x] Footer `/privacy` link pointed at a page that doesn't exist. Not
      writing fabricated legal content at the time — pointed it at a
      `mailto:` instead. (Real Privacy Policy content was written and
      linked on 2026-07-28 — see above.)
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

## Fixed 2026-07-28

- [x] Built `one.html` — a dedicated LRX One product page (features grid,
      pricing, dashboard mockup), mirroring `billing.html`'s pattern.
      Fixes a real gap: when `lrxone-website` was cut down to a lean
      sign-in page, its full content was removed but never actually
      relocated here — only linked back to the summary card. See
      MEMORY.md.
- [x] Both product cards on `index.html` now link to their dedicated
      pages (`/one.html`, `/billing.html`) via a "See Full Details" link,
      in addition to the existing "Register Interest" mailto — neither
      card linked anywhere but `mailto:` before this.
- [x] `one.html`'s pricing table was stale placeholder copy (3 tiers,
      guessed prices) — replaced with the real 4-tier structure
      (Starter/Growth/Business/Enterprise) and prices straight from
      `lrxone/services/billing-service`'s `application.yml` +
      `V2__pricing_marketplace.sql`. See MEMORY.md.

## Fixed 2026-07-28 (later same day)

- [x] Renamed the flagship product "LRX One" → "LRX One Core" (trademark
      consolidation, parallel to the earlier LRX Billing → LRX One
      Billing rename) — `index.html` product card wordmark/copy/mailto
      subjects and `one.html` title/meta/hero/copy/every pricing CTA
      subject line. See MEMORY.md.

## Note for later

- [ ] When LRX One Core and/or LRX One Billing actually go live (real
      production infra deployed, real payment credentials configured),
      flip the relevant card's badge back and restore the direct signup
      CTA/link — don't forget this is currently deliberately understated.

## Not investigated yet

- [ ] No test/build/lint tooling exists for this repo (it's now seven
      static HTML files: `index.html` + `billing.html` + `one.html` +
      `terms.html` + `privacy.html` + `refund-policy.html` +
      `cancellation-policy.html`) — confirm that's intentional before
      adding any tooling unprompted.
