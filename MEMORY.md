# MEMORY — lrxtechgroup-website

Running log of what's been done on this repo across sessions, so work can be picked
up without re-deriving context. Newest entries at the top. Update this file every
time you finish a unit of work here.

---

## 2026-07-28 (even later still) — Added Terms of Service, Refund Policy, and Cancellation Policy

User instruction: this site (lrxtechgroup.com, the corporate parent site
hosting both LRX One Core and LRX One Billing) needed all three legal
pages — same underlying PayFast account-verification requirement that
prompted building them on `lrxone-website` a few commits back, but this
site had none of the three of its own. Also flagged two things to fix
while building them: there's no free tier anymore (confirmed already
true everywhere on this site), and finance/billing-related email should
go to `billing@lrxtechgroup.com` instead of `sales@lrxtechgroup.com`.

Built `terms.html`, `refund-policy.html`, `cancellation-policy.html` at
the site root, styled to match this site's actual nav/footer components
(the `LRX TECH` logo mark and footer used across `index.html`/`one.html`/
`billing.html`) rather than copy-pasting `lrxone-website`'s "LRX | ONE"
nav verbatim. Content itself is adapted from `lrxone-website`'s versions
(same section structure, same South African legal framing — PayFast,
ZAR, POPIA-adjacent data handling, South African governing law) but
**generalised to cover both products hosted here** instead of being LRX
One Core-specific: "our products", not "LRX One Core", with links out to
each product's own pricing page (`/one.html`, `/billing.html`).

**Email routing** — finance/billing-specific contact points now go to
`billing@lrxtechgroup.com`: refund requests, cancellation requests, and
the Refund/Cancellation pages' own "Contact us" sections. Terms of
Service's contact section splits the two explicitly (general questions →
`sales@`, billing/payment/refund/cancellation questions → `billing@`).
Deliberately did **not** touch the pre-sales mailto links elsewhere on
the site (pricing-page CTAs like "Register Interest" / "Talk to Us" /
"Contact Sales" on `index.html`/`one.html`/`billing.html`) — those are
prospective-customer inquiries about buying the product, not billing
correspondence about an existing account, so they stay on `sales@`.

**No free tier** — already true everywhere on this site (confirmed via a
repo-wide `\bfree\b` search — zero matches before this change; the
STARTER tier was priced at R199/mo back on 2026-07-28 and nothing since
has reintroduced free-tier language). Added it as an explicit statement
on the Refund and Cancellation pages anyway ("we don't offer a free
tier") so a reader can't assume otherwise from the "downgrade to Starter"
framing that `lrxone-website`'s original cancellation-policy.html used
(that page pre-dates the STARTER pricing change and still said "the free
Starter tier" — not fixed here, since that's a different repo).

**Known gap, not fixed here**: the new pages' data-handling sections
point to a `mailto:...Privacy%20Policy%20Request` link rather than a
real Privacy Policy page — this site still doesn't have one (see
2026-07-25 below). Same stopgap as the existing footer `Privacy` link,
not a new problem, but worth a real `/privacy.html` at some point rather
than perpetuating the mailto pattern.

- Linked all three new pages from the footers of `index.html`,
  `one.html`, and `billing.html`.
- Verified all six HTML files (three new + three existing, since the
  footer edits touched them) still parse cleanly.

---

## 2026-07-28 (later still) — Growth and Business pricing cards updated (price-per-user ladder)

Follow-up to the Enterprise margin fix below. The user wanted
subscription price ÷ user cap to decrease as tier increases; Starter and
Enterprise already did (R99.50/user → R64.99/user) but Growth and
Business both undershot Enterprise's ratio, breaking the curve. Fixed by
raising list price only (user caps untouched) — see `lrxone`'s own
MEMORY.md for the full reasoning and the geometric interpolation used to
land on the numbers.

- `one.html`: Growth card "R499/mo" → "R899/mo", Business card
  "R1,299/mo" → "R2,299/mo". No other copy on either card changed.
- Verified the file still parses cleanly.

---

## 2026-07-28 (even later) — Enterprise pricing card updated (R3,999 → R6,499/mo)

Follow-up to the pricing table revision below, which deliberately left
Enterprise's price alone. `lrxone`'s billing-service was found to be
losing ~70% of Enterprise's monthly revenue to AI API cost alone at the
10,000 msg/mo allowance. Asked the user which lever to fix it with (cut
the AI allowance, raise the price, or a hybrid); they chose raising the
price. See `lrxone`'s own MEMORY.md for the full numbers and cost model.

- `one.html`'s Enterprise pricing card: "R3,999/mo" → "R6,499/mo". No
  other copy on the card changed — the AI Assistant allowance stated
  there (10,000 msgs/mo) was already correct and stays unchanged.
- Verified the file still parses cleanly.

---

## 2026-07-28 (the actual last one today) — Full LRX One Core pricing table revised on one.html

User asked to relook at the whole tier structure — competitive, and
generous enough that businesses don't feel pushed into marketplace
add-ons. Real market research first (Zapier/Monday.com/ClickUp/Make.com
converted to ZAR) showed prices were already competitive; the fix was
allowances. A first draft that roughly doubled/tripled every allowance
including AI messages got caught by the user's own follow-up question —
"would the fee cover full usage?" — which surfaced that several tiers
would have lost money on AI cost alone. See `lrxone`'s own MEMORY.md for
the full cost model (grounded in the real `ai-service` implementation,
not guessed) and the corrected numbers.

`one.html`'s pricing table updated to match exactly: all four prices
unchanged, users/workflows/documents/storage roughly doubled per tier,
Starter's card now lists AI Assistant and Integrations as included
(previously excluded entirely), Growth/Business/Enterprise's AI message
lines updated to the margin-corrected numbers (600/2,000/10,000 — not
the initially-drafted 1,500/5,000/20,000). Verified the file still
parses cleanly.

---

## 2026-07-28 (last one today) — STARTER tier priced at R199/mo instead of free

User decision ("we not going to offer a free one, rather at a fairly low
fee") — asked for the exact number rather than picking one, since this is
a real billing-config change reflected here as marketing copy, not just a
wording tweak. Confirmed R199/mo. See `lrxone`'s own MEMORY.md for the
actual `billing-service` config change this page now matches.

- `one.html`'s Starter pricing card: "Free" → "R199/mo".
- Pricing section headline "Start free. Scale when you're ready." → "Start
  small. Scale when you're ready." — the old headline was a direct claim
  about a Starter price that no longer exists.
- Verified `one.html` still parses cleanly.

---

## 2026-07-28 (yet later same day) — Renamed the flagship product to "LRX One Core"

User request, following a short naming discussion: for trademark
efficiency, "LRX One" should be a pure house mark that every product
nests under (already the pattern for "LRX One Billing"), not
double-duty as both the house mark AND the specific name of the
flagship product. Landed on **Core** as the flagship's suffix — matches
existing copy ("Enterprise Operating System"), cheapest to roll out,
and signals "the foundational product other things nest under" without
overclaiming.

- `index.html`: product card wordmark `LRX | ONE` → `LRX ONE | CORE`
  (same gold-mark + pipe + plain-suffix convention already used for the
  Billing card), meta description, About section body copy, and the
  Register Interest mailto subject line.
- `one.html`: `<title>`, meta description, hero eyebrow (now literally
  names the product — "LRX One Core — Enterprise Operating System" —
  matching how `billing.html`'s eyebrow names its own product), hero
  desc paragraph, features section intro, CTA banner copy, and every
  mailto subject line across the hero, all four pricing tiers, and the
  CTA banner (five occurrences total, `sed`-verified none missed).
- Also renamed in `lrxone-website` (nav-brand and footer wordmarks,
  title/meta, hero desc, learn-more strip, footer mailto subject, and
  every "LRX One" reference in `privacy.html`/`terms.html`'s legal text
  — the defined product name in those documents needed to match too) —
  see that repo's own MEMORY.md.
- Also renamed the small set of literal "LRX One" strings exposed in
  the app frontend itself (`lrxone/frontend`): the sidebar logo, the
  login page welcome heading, the app-loading spinner text, the accept-
  invitation copy, and one workflow-template notification subject —
  five files, all just literal UI strings, no logic changes.
- Verified `index.html` and `one.html` still parse cleanly.

---

## 2026-07-28 (later same day) — Updated one.html's pricing to the real 4-tier structure

User asked to update LRX One's pricing "according to the adjusted
structures" — the 3-tier pricing I'd put on `one.html` when building it
earlier today was placeholder copy carried over from `lrxone-website`'s
old content, not the actual current pricing. Went to the source of
truth instead of guessing: `lrxone/services/billing-service`'s
`application.yml` (`lrx.billing.tiers`) and its `V2__pricing_marketplace.sql`
migration, which record a real, already-implemented 4-tier model:

- **Starter** — Free. 1 user, 5 workflows, 50 documents, 1GB storage, no AI/integrations.
- **Growth** — R499/mo. 5 users, 25 workflows, 500 documents, AI (500 msgs/mo), integrations, 10GB.
- **Business** — R1,299/mo (now the featured card). 15 users, 100 workflows, 5,000 documents, AI (2,000 msgs/mo), 50GB.
- **Enterprise** — R3,999/mo. 50 users, unlimited workflows/documents, AI (10,000 msgs/mo), 500GB, priority support included.

This replaces the old 3-tier set (Starter/Business/Enterprise at
Free/R999/R4,999) that never actually existed in the backend — those
numbers were invented when the original `lrxone-website` pricing
section was written, before the billing-service had a real tiers
config at all. The backend's own migration also retired an old
`UNLIMITED` tier and renamed things (old `BUSINESS` limits became the
new `GROWTH`, etc.), so tier *names* shifted too, not just prices —
matched those exactly rather than just updating numbers under the old
names.

Widened `.pricing-grid` to `max-width: 1200px` to fit four cards
cleanly (was sized for three), matching the layout `billing.html`
already uses for its own 4-tier grid. Added a one-line note below the
pricing grid mentioning resource marketplace add-ons (extra users/
workflows/AI messages/documents/storage, also real and defined in
`billing-service`'s `AddonType` enum) exist for topping up beyond a
tier's base allowance, without building out a full add-on pricing table
here — that level of detail belongs in-app, not on the marketing page.

Deliberately did NOT touch `billing.html`'s own pricing (Starter R999/
Growth R2,999/Business R6,999/Enterprise Custom + txn%) — that's a
different product (the LRX One Billing merchant platform) with its own
separate, unrelated pricing model; the migration that prompted this
update only touched the tenant-subscription tiers for using LRX One
itself.

Verified `one.html` still parses cleanly.

---

## 2026-07-28 — Built the dedicated LRX One product page, fixed both product-card links

User caught a real gap from the previous day's site-role restructure:
when `lrxone-website` (lrxone.com) was cut down to a lean sign-in page,
its full features grid, pricing tiers, and dashboard mockup were
*removed* but never actually *relocated* here — the restructure only
added a link back to this site's existing `#products` summary card,
not a real dedicated page. The user's original intent (stated when the
restructure was requested) was for that content to become a proper
LRX One product page here, parallel to how `billing.html` already
serves LRX One Billing.

- New `one.html`: hero (two-column, with the dashboard mockup carried
  over verbatim from `lrxone-website`), the five-item pillars band
  (Connect/Automate/Analyze/Scale/Secure), a features section built
  from the original six-feature grid copy (Workflow Automation, AI
  Business Assistant, Real-time Analytics, Document Management,
  Integration Hub, Team & Permissions — reusing `billing.html`'s
  `.solution-grid` CSS pattern rather than inventing a new one), and a
  pricing section with the original three tiers (Starter Free, Business
  R999/mo, Enterprise R4,999/mo). Same CSS variables, nav pattern
  (corporate `LRX TECH Group` mark, not a product-specific one — same
  choice already made for `billing.html`), and footer as the rest of
  this site. "Coming Soon" badge kept; Sign In button is a real link
  (`app.lrxone.com/login`) since it'll just start working once the app
  is actually deployed, same reasoning already applied on
  `lrxone-website` itself.
- Pricing CTAs are `mailto:` "Register Interest"/"Contact Sales" links,
  not live checkout — consistent with the site-wide "Coming Soon"
  policy (matches how `billing.html`'s pricing CTAs already work).
- While fixing this, found a second, related gap: **neither** product
  card on `index.html` linked to its own dedicated page at all — both
  CTAs were `mailto:` only, so `billing.html` itself was never actually
  reachable from its own product card either. Added a "See Full
  Details →" link (new `.product-actions` wrapper, `.product-link`
  style reused) to both cards, alongside the existing "Register
  Interest" link — `/one.html` on the LRX One card, `/billing.html` on
  the LRX One Billing card.
- Updated `lrxone-website/index.html`'s two outbound links (the hero
  note and the "Features & Pricing" footer link) from
  `lrxtechgroup.com/#products` to `lrxtechgroup.com/one.html` — they
  now point at the actual dedicated page instead of the summary card.
- Verified `index.html`, `one.html`, and `billing.html` all still parse
  cleanly.

---

## 2026-07-27 (yet even later same day) — Fixed the off-center hero scroll indicator

User spotted it via a real device screenshot (mobile, `07:26`, live
site) — the "SCROLL" cue at the bottom of the hero was visibly shifted
right of the two buttons above it, which are correctly centered.

Root cause: `.hero-scroll` centers itself with
`left: 50%; transform: translateX(-50%)`, but also has
`animation: fadeUp ... both`. A CSS animation's `transform` REPLACES the
element's static `transform` value entirely rather than composing with
it — `fadeUp`'s keyframes only set `translateY(...)`, so once the
animation reaches its `to` state, `translateX(-50%)` is gone and
`both` fill-mode keeps that broken state applied permanently (not just
during the animation). The element ends up sitting at literal `left:
50%` with no horizontal centering, shifted right by half its own width.

Fixed with a dedicated `@keyframes scrollFadeUp` that carries
`translateX(-50%)` through both the `from` and `to` states, so the
centering survives the animation completing. Checked whether this same
`translateX(-50%)`-for-centering + shared-`fadeUp`-animation pattern
existed anywhere else across this file, `billing.html`, or
`lrxone-website/index.html` — `.hero-scroll` is the only element using
it, so this was a contained, one-off bug, not a systemic pattern.

Verified `index.html` still parses cleanly.

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
