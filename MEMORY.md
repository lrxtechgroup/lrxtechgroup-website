# MEMORY — lrxtechgroup-website

Running log of what's been done on this repo across sessions, so work can be picked
up without re-deriving context. Newest entries at the top. Update this file every
time you finish a unit of work here.

---

## 2026-07-27 (yet later same day) — Renamed LRX Billing to LRX One Billing (trademark consolidation)

User request: nest all products under the "LRX One" trademark rather
than each having its own separate product name — easier to trademark
one mark with sub-brands than several independent ones. "LRX Billing" →
"LRX One Billing" everywhere on this site: `<title>`, meta description,
hero eyebrow, section headline, mailto subject lines, nav/footer link
text (`index.html` and `billing.html`).

Also restyled the product-card wordmark to match: `index.html`'s
billing card now reads `LRX ONE | BILLING` (gold "LRX ONE", plain
"BILLING" after the pipe) instead of `LRX | BILLING` — same pattern the
flagship product's own `LRX | ONE` wordmark already uses, now visually
signalling it's a sub-product rather than a standalone brand. Left
`billing.html`'s own nav using the corporate "LRX TECH Group" mark
unchanged — it's a page hosted under lrxtechgroup.com, not a separately
branded product site, so the corporate identity there is correct as-is.

---

## 2026-07-27 (yet later same day) — Contact email changed info → sales

User request. All `mailto:` links across `index.html` and `billing.html`
(Register Interest, Talk to Us, Contact Sales, general Contact links,
the visible email in the contact-grid) now go to
`sales@lrxtechgroup.com` instead of `info@lrxtechgroup.com`. Verified
both files still parse cleanly after the swap.

## 2026-07-27 (even later same day) — Same "Coming Soon" treatment on billing.html

User asked to double-check `billing.html`'s pricing CTAs specifically.
They were already fine — every pricing card CTA was already `mailto:`
("Talk to Us"/"Contact Sales"), never a live signup/checkout flow, so
nothing there implied availability that isn't real.

Checking turned up two things that *did* need the same fix already
applied to `index.html`:
- The nav still had the same gold "LRX One →" button linking straight
  to `lrxone.com` — same class of inconsistency fixed on `index.html`
  earlier today, missed here since `billing.html` was written before
  that fix. Changed to a "Contact" CTA (`mailto:`), same button styling.
- No "Coming Soon" signal existed on this page at all, unlike the other
  two. Added the same `.hero-badge.coming-soon`-style pill above the
  hero eyebrow (new CSS, matching the pattern from `lrxone-website`).
- Softened the hero's "Get Started" button (already `mailto:`, just the
  label) to "Register Interest", matching the other pages' wording.
- Verified `billing.html` still parses cleanly.

## 2026-07-27 (later same day) — Both product cards set to "Coming Soon"

User request, direct: neither LRX One nor LRX Billing is actually ready
for a live "sign up now" push yet (LRX One's real infra isn't deployed to
AWS yet, LRX Billing barely has any real implementation beyond the
pricing page) — reflect that honestly on the site rather than advertise
availability that isn't real.

- Both product cards' badges changed from "Available Now"/"New" to
  "Coming Soon" (reusing the existing `.product-badge.coming-soon` CSS
  class, already defined for the old placeholder card). Kept the real
  descriptions/feature lists at full visual weight — unlike the old
  generic "LRX | ___" placeholder, these describe real, specific
  products, just not ones open for signup yet.
- Both cards' CTAs changed from direct links into the live apps
  (`lrxone.com`, `/billing.html`) to `mailto:` "Register Interest" links
  — a lower-commitment ask that doesn't send visitors into a signup flow
  sitting on infrastructure that isn't actually live in production.
- Also caught and fixed a resulting inconsistency on the same page: the
  nav bar still had a prominent gold "LRX One →" button linking straight
  to the live app — directly contradicting "Coming Soon" two sections
  down. Changed it to a neutral "Contact" CTA (same button styling,
  points to `#contact` instead).
- Left the hero buttons (`Our Products`/`About Us`) and the contact
  section's `lrxone.com` link alone — neither implies live signup, both
  are already informational/neutral.
- Verified `index.html` still parses cleanly.

---

## 2026-07-27 — Added the real LRX Billing product page, resolved the "Coming Soon" placeholder

Part of working through `lrxtechgroup/lrxone`'s `CLAUDE_TODO.md` (item
18), which had a full spec for an "LRX Billing" marketing page. This
directly resolved an item already flagged in this file's own backlog
(2026-07-25's "the 'Coming Soon' second product card has no real content
— check with the user whether/when to fill it in") — LRX Billing is that
second product, decided elsewhere (`lrxone`'s billing-service work), so
the placeholder card is now real.

- New `billing.html`: hero, "why not just use Stripe" problem section,
  solution grid (Stitch/PayFast/ZAR invoicing/POPIA residency/webhooks/
  dashboard), a code example, and a 4-tier pricing table — matching this
  site's existing gold-on-black brand exactly (reused the same CSS
  variables/nav/footer structure as `index.html`, not reinvented).
- `index.html`: replaced the "Coming Soon — LRX | ___" product card with
  a real LRX Billing card linking to `/billing.html`. Updated the hero
  stats band from "1 Flagship Product" to "2 Products" (was literally
  false with two real products now). Added nav and footer links to the
  new page.
- Verified both HTML files parse cleanly (`html.parser`).

Confirmed item 19 (hero blank-screen fix, Jessica Le Roux in leadership,
font fallbacks) was already fully done in this repo from earlier
session work — checked directly, no changes needed.

---

## 2026-07-25/26 — Minor findings fixed, merged to main

Small findings from a code-review pass across `lrxtechgroup` org repos,
fixed on `claude/lrx-one-work-cgvqnx` and merged to `main` (this repo's
established direct-to-main convention):

- Footer `/privacy` link pointed at a page that doesn't exist — pointed
  it at `mailto:info@lrxtechgroup.com?subject=Privacy%20Policy%20Request`
  instead of fabricating legal content. Real Privacy Policy content still
  needs to be written by the site owner and linked once it exists.
- Added an inline-SVG favicon (none existed), `robots.txt`, and
  `sitemap.xml`.
- Footer copyright year was a hardcoded "© 2025" — made it dynamic via a
  small inline script.

---

## 2026-07-25 — Repo orientation, memory file set up

No code changes made this session. Session started with "continue the work on lrx
one," which was ambiguous — this repo is the corporate marketing site
(`lrxtechgroup.com`), a single static `index.html` (~600 lines, plain CSS/JS, no
build step), and it already has an "LRX One" product card in the `#products`
section (name, tagline, feature list, link to `https://lrxone.com`). There was no
open PR, issue, or branch work specifically about "LRX One" beyond that card —
the working branch (`claude/lrx-one-work-cgvqnx`) was identical to `main`.

Asked the user to clarify scope (expand the product card vs. build a dedicated
LRX One page vs. something else); they redirected to the separate `lrxone`
product monorepo (`lrxtechgroup/lrxone`) instead — that's where the actual
"LRX One" work is happening (see that repo's own `MEMORY.md`/`TODO.md`). No
further work was done here this session beyond adding this file and `TODO.md`
per a standing request to add both to every repo worked in.

**Next step when resuming:** nothing queued. If asked to work on this site
again, re-confirm scope before assuming — the repo is small enough that "LRX
One" could mean anything from a copy tweak to a new page.

---

## Repo orientation (for future sessions — not a changelog entry)

- Entire site is `index.html` — no framework, no build step, no package.json.
  Fonts via Google Fonts CDN (Montserrat). Plain inline `<style>`.
- Sections (in order): nav, hero, `#about`, `#products` (LRX One card +
  "Coming Soon" placeholder card), `#founder` (Brandon Le Roux CEO, Jessica Le
  Roux COO), `#contact`, footer.
- `README.md` is a single line — no other docs exist in this repo.
- Branch convention per this session's task instructions: develop on
  `claude/lrx-one-work-cgvqnx`, push there (not directly to `main`).
