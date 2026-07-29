# MEMORY — lrxtechgroup-website

Running log of what's been done on this repo across sessions, so work can be picked
up without re-deriving context. Newest entries at the top. Update this file every
time you finish a unit of work here.

---

## 2026-07-29 (final one today) — Added WhatsApp to index.html's Contact section

`index.html`'s `#contact` section (`.contact-grid` — Email / Product /
Website cards) is the site's only real "contact info hub"; every other
page just has a bare `mailto:` "Contact" link in its nav/footer, not a
card grid, so this is the one place a new contact channel actually fits
without inventing new UI elsewhere. Added a 4th card: `wa.me/27620498603`
(South African mobile 062 049 8603, converted to E.164 without the `+`,
which is `wa.me`'s required link format), displayed as `+27 62 049 8603`,
`target="_blank" rel="noopener"` since it hands off to WhatsApp itself.

Checked `.contact-grid`'s CSS before adding a 4th item into what was a
3-card row: it's `grid-template-columns: repeat(auto-fit, minmax(220px,
1fr))` with individually-bordered cards (`.contact-item { border: ... }`),
NOT the shared-background-through-gaps trick that caused the real
orphaned-cell bug fixed earlier this session in `.pricing-grid`/
`.solution-grid` — so a 4th card just wraps to its own row on wrap-around
widths with no risk of that bug recurring. Verified anyway: screenshotted
desktop (3 cards top row, WhatsApp card wraps in the correct place next
to Email) and confirmed zero horizontal overflow at 375px mobile.

---

---

## 2026-07-29 (yet later same day) — Removed Sign In + product link from both product pages' footers

`one.html`'s footer had `Home | Sign In | LRX One Billing | Terms |
Privacy | Refund Policy | Cancellation Policy | Contact`; `billing.html`'s
had `Home | LRX One | Terms | Privacy | Refund Policy | Cancellation
Policy | Contact` (no Sign In there to begin with — checked before
assuming both pages had the same two link types to remove). Removed
"Sign In" and the product-name link from `one.html`, and the product-name
link from `billing.html`, leaving both footers as `Home | Terms | Privacy
| Refund Policy | Cancellation Policy | Contact`.

Deliberately left `index.html`'s footer and all 4 legal pages' footers
untouched — checked each first: `index.html`'s footer has `About` and
`Products` (an in-page anchor, not a named product link) instead of Sign
In, structurally different from the pattern being removed; the 4 legal
pages' footers never had Sign In or a product link at all. Also left
`one.html`'s two other "Sign In" buttons alone (the hero CTA and the
pricing `cta-banner` button) — those are prominent mid-page CTAs, not the
"bottom of page, next to policy docs" footer link the request named.

**Verified**: screenshotted both footers after the edit — clean
`Home / Terms / Privacy / Refund Policy / Cancellation Policy / Contact`
row on each, no orphaned separators or spacing artifacts from the removed
`<li>`s.

---

## 2026-07-29 (later same day) — Extended the LRX One wordmark treatment everywhere; removed every remaining arrow

Follow-up to the same-day entry below. The user explicitly said to do
both "everywhere," overriding the scoping decision made in that earlier
pass:

- **Arrows**: removed the 3 remaining ones this repo had - `index.html`'s
  two "See Full Details →" product-card links, `one.html`'s "Sign In →"
  hero button, and the "← Back to lrxtechgroup.com" nav-back link shared
  by all 4 legal pages (`terms.html`, `privacy.html`, `refund-policy.html`,
  `cancellation-policy.html`). Every arrow on the site is now gone, not
  just the ones on Register Interest.
- **LRX One wordmark**: extended the gold "LRX One" / white product-name
  span pattern to every remaining rendered mention across all 7 pages -
  hero descriptions, section-sub/section-body prose, all 4 legal pages'
  intro paragraphs and body text, and the footer link lists (`one.html`'s
  "LRX One Billing" footer link, `billing.html`'s "LRX One" footer link).
  Applied consistently: "LRX One" alone (no Core/Billing following) gets
  just the gold span; "LRX One Core"/"LRX One Billing" gets gold "LRX One"
  + white "Core"/"Billing", including inside `<a>` tags (verified the
  link's own hover-underline still applies across the whole differently-
  colored text, since text-decoration is a property of the anchor, not
  the inner spans) and around a possessive `'s` (left unstyled/inheriting,
  e.g. "**Core**'s AI Assistant").
  **Still not stylable, not a scoping choice**: `<title>` tags and
  `<meta name="description">` content can't contain HTML at all - browsers
  don't render markup inside either, so those stay plain text by hard
  technical constraint, not a design decision. Same for the `mailto:`
  subject-line JS string literals (never rendered as visible text).

**Verified in a real browser again**: screenshotted `terms.html`'s intro
(confirms the pattern reads cleanly even inside dense legal prose, which
was the main open question from the previous pass), `index.html`'s About
section body copy, and `one.html`'s footer (confirms the "LRX One Billing"
footer link's gold/white split doesn't look broken sitting next to the
other plain-grey footer links - reads as an intentional highlight, not an
inconsistency). Re-confirmed zero arrows remain anywhere with a full-repo
grep after the edits, not just spot checks.

---

## 2026-07-29 — Removed arrows/em-dashes site-wide; two-tone LRX One wordmark on both product hero eyebrows

Four-part request from the user, all applied across every `.html` file in
the repo:

- **Arrows out of "Register Interest"**: `index.html`'s two product-card
  links, `one.html`'s consolidated pricing CTA, and `billing.html`'s
  equivalent ("Talk to Us" - same register-interest mailto control, arrow
  removed too for consistency between the two nearly-identical CTA
  sections) all lost their trailing "→". Left every *other* arrow alone
  (e.g. "See Full Details →", "Sign In →", the nav-back "←") since the
  request was specifically about Register Interest, not arrows generally.
- **"Register Interest" out of the email subject**: every `mailto:` link's
  `subject=` (both the static hrefs and the two tier-select JS builders in
  `one.html`/`billing.html`) now ends at the product/tier name -
  e.g. `subject=LRX%20One%20Core` instead of `...%20-%20Register%20Interest`.
  The button label itself still says "Register Interest" - only the
  outgoing email's subject line changed.
- **Long dashes removed site-wide**: every visible em-dash ("—") across
  all 7 pages replaced with a plain hyphen with the same surrounding
  spacing (" - "), including page `<title>`s, meta descriptions, body
  copy, pricing-tier option labels, and the CSS `content: '—'` bullet
  glyphs used before `.pricing-features`/`.doc-section ul` list items
  (also em-dash characters, rendered on the page, so in scope). Left CSS/
  JS *comments* alone (`/* ── HERO ── */` etc. in index.html) since those
  aren't rendered content - not what "around the website" means.
- **LRX One wordmark: gold "LRX One" + white product name**: matched the
  existing pattern already on `index.html`'s two product cards
  (`<span class="gold">LRX ONE</span> | CORE`) onto the one place per
  product page where the name is displayed as a prominent badge -
  `one.html`'s hero-eyebrow ("LRX One Core - Enterprise Operating
  System") and `billing.html`'s ("Introducing LRX One Billing"). Both use
  inline `style="color:var(--gold)"` / `style="color:var(--white)"`, not
  a `.gold` class - checked first and found `.gold` is scoped per-page to
  `.hero-headline .gold` (one.html/billing.html) or `.product-name .gold`
  (index.html), not a standalone global class, so `class="gold"` on a span
  outside those containers would silently apply no color at all. Caught
  and fixed this before it shipped, not after.
  **Deliberately did NOT apply this to**: inline body-prose mentions of
  "LRX One Core"/"LRX One Billing" (hero descriptions, section-sub text,
  legal-page paragraphs), or footer link-list items - gold text scattered
  through dense paragraphs or singled out from an otherwise-uniform list
  of footer links would look inconsistent, not intentional. Reserved the
  two-tone treatment for prominent wordmark-style placements only (hero
  eyebrows, product cards), matching how the pre-existing pattern was
  already used.

**Verified in a real browser, not just by reading the diff**: served the
repo locally (`python3 -m http.server`) and used Playwright/Chromium to
screenshot `one.html`'s and `billing.html`'s heroes (confirmed the gold/
white split renders correctly), `index.html`'s products section (confirmed
"Register Interest" lost its arrow while "See Full Details →" correctly
kept its own), and both product pages at a 375px mobile viewport (zero
horizontal overflow, eyebrow text wraps cleanly across two lines with the
color split intact).

---

## 2026-07-28 (pricing CTA consolidation) — One register-interest control per page

Two asks in one turn. First: why does one pricing card look different?
Answer, not a bug — `.pricing-card.featured` (Business on `one.html`,
Growth on `billing.html`) intentionally gets a lighter `var(--dark)`
background plus a gold top-border gradient via `::before`, the standard
"this is our recommended tier" highlight pattern. Explained rather than
changed.

Second: consolidate every pricing card's own CTA into a single
registration control, for both products. Previously each of the 4 cards
on `one.html` and 4 cards on `billing.html` had its own `mailto:` link
("Register Interest" / "Talk to Us" / "Contact Sales" depending on
tier) — 8 separate links doing the same thing with slightly different
subject lines. Removed all 8, added one consolidated control per page in
the existing `cta-banner` section: a `<select class="tier-select">`
listing the tiers (with price shown inline, e.g. "Business —
R2,299/mo"; `billing.html`'s Enterprise option reads "Custom pricing"
since that tier has no fixed price) next to a single CTA button.

The button's `href` is a `mailto:sales@lrxtechgroup.com` link whose
`subject` is rebuilt by a small vanilla-JS snippet every time the
`<select>` fires a `change` event (and once on page load, so the link is
correct before any interaction):
```js
function updateLink() {
  var tier = select.value;
  link.href = 'mailto:sales@lrxtechgroup.com?subject=' +
    encodeURIComponent('LRX One Core - ' + tier + ' - Register Interest');
}
```
(`billing.html`'s version says "LRX One Billing" instead.) No backend,
no form submission — still a fully static site, same `mailto:` pattern
every other CTA on the site already uses, just aggregated into one
control instead of duplicated per card. First draft put the full option
text (including the price) into the subject line via `select.options[
select.selectedIndex].text`; switched to `select.value` (just the tier
name) for a cleaner subject after testing both — matches the plain
"LRX One Core - Business" style the original per-card links used.

Removed the now fully-dead `.pricing-cta` / `.pricing-cta.primary` /
`.pricing-cta.secondary` CSS rules from both pages once every element
using them was gone (confirmed via grep before deleting).

**Verification**: didn't just eyeball it — used Playwright to actually
select each tier option on both pages and read back the resulting
`href`, confirming the encoded subject line was correct for every tier
including the Enterprise/Custom-pricing edge case on `billing.html`.
Also re-ran the mobile overflow sweep (375/768/1440px, both pages) since
markup changed, to make sure the new `.interest-form` control didn't
introduce a new overflow — clean.

---

## 2026-07-28 (follow-up) — Removed the same redundant nav item from the footer

Direct follow-up to the previous fix, which only removed "LRX One
Billing" from `index.html`'s header nav (it sat next to "Products",
redundant since Products already covers both product cards). The same
exact redundancy existed in that page's footer link list too — "LRX One
Billing" right after "Products" again. Removed it there.

Checked the other two pages for the same pattern before calling it done:
`one.html`'s footer has "LRX One Billing" next to "Sign In" — that
footer has no "Products" link at all, so it's not redundant, left alone.
`billing.html`'s footer links to "LRX One" (the shared login), not to
itself — also not redundant. Confirmed via grep that no
"Products"/"LRX One Billing" adjacency remains anywhere on the site.

---

## 2026-07-28 (user-reported) — Fixed orphaned pricing-grid cell + redundant nav item

Two real bugs the user spotted from actual device screenshots, on top of
everything the earlier automated mobile audit already caught.

**Bug 1 — the gold box.** User sent a screenshot of `billing.html`'s
pricing section on a real device showing an unexplained olive/gold box
occupying two grid cells next to the Enterprise card. Traced it to
`.pricing-grid`'s CSS: `grid-template-columns: repeat(auto-fit,
minmax(240px, 1fr))` combined with `background: var(--gold-line); gap:
1px;` — a common trick where the container's own background shows
through 1px gaps to draw thin gold gridlines between cards. That trick
depends on every grid cell being filled. With exactly 4 pricing cards,
`auto-fit` computes however many 240px+ columns fit the container width;
at any width where that resolves to 3 columns, the 4th card (Enterprise)
sits alone in row 2, leaving 2 grid cells empty — and the gold
background fills those empty cells instead of being invisible. The
screenshot's device was at a CSS viewport width (~923-1080px logical)
that landed exactly in that "3 fits, 4 doesn't" zone.

Fixed on both `billing.html` and `one.html` (identical CSS, identical
4-card grid) by replacing `auto-fit` with explicit breakpoints —
1 column by default, 2 at 620px+, 4 at 1020px+ — which only ever
produces column counts that divide 4 evenly (1, 2, or 4), making the
3-column orphan state impossible rather than just less likely.

While fixing it, checked every other `auto-fit` grid on both pages for
the same pattern (background-fill trick + item count) and found
`.solution-grid` (6 items) had it too — and unlike the pricing grid,
this one was visibly broken even at a normal desktop width (1300px
showed 4 columns with 6 items, 2 empty cells). Fixed identically:
explicit 1 → 2 → 3 column breakpoints (6 divides evenly by 1, 2, 3, 6 —
never 4 or 5). `.problem-grid` and `.contact-grid` don't have the
background-fill trick (individual items carry their own background, not
the grid container), so an uneven split there wouldn't produce a visible
color-mismatch — left alone as lower-priority.

Verified with an automated check across 8 widths (375–1440px) × 2 pages
× 2 grids, measuring whether each grid's last item actually reaches the
container's right edge (the geometric signature of a filled vs. orphaned
last row) — zero orphans found anywhere after the fix. Also visually
reproduced the exact original bug at the screenshot's own viewport width
before fixing, then confirmed the same viewport renders as a clean 2×2
after.

**Bug 2 — redundant nav item.** Second screenshot, same session:
`index.html`'s header nav read "About / Products / LRX One Billing /
Leadership / Contact" — Billing had its own standalone nav entry while
Core never did, and "Products" already anchors to the section containing
both product cards. Removed the standalone "LRX One Billing" `<li>` from
the header nav. Left the identical link in the footer alone — footers
routinely deep-link to specific pages beyond the main nav, and that
wasn't what was flagged.

---

## 2026-07-28 (mobile audit) — Found and fixed real footer overflow on three pages

User asked whether the day's work had been verified on mobile — every
prior check this session used a single 1440px desktop Playwright
viewport. Ran a proper sweep: automated overflow detection
(`document.documentElement.scrollWidth` vs `clientWidth`) across all 7
pages on this site at 375px and 768px, using the same Playwright +
pre-installed Chromium setup as earlier in the session (Node, since no
Python `playwright` package is installed here).

**Real bug found**: `index.html`, `one.html`, and `billing.html` all
overflowed horizontally by 164-291px at 375px (768px was clean on all
three). Traced it with a script that walks every element and reports
any whose bounding box extends past the viewport — the culprit was
`.footer-links`, whose CSS (`display: flex; gap: 24px; list-style:
none;`) never had `flex-wrap: wrap`. That wasn't a problem when each
footer had ~5 links, but today's earlier commits grew all three footers
to 8+ links (Terms, Privacy, Refund Policy, Cancellation Policy, added
piecemeal across several commits) without anyone checking mobile wrap
behavior at the time — the four legal pages built from scratch this
session got `flex-wrap: wrap` correctly from the start, these three
pre-existing pages didn't. Added the missing property to all three;
re-ran the overflow check and confirmed 0px overflow across all 7 pages
at both widths.

Also visually confirmed (screenshots, not just the automated check) that
the products-section intro line and both pricing-section sub-lines added
earlier today reflow correctly at 375px — no issues found there.

See `lrxone-website`'s own MEMORY.md for a second, related bug this same
audit found on that site's nav (not a footer issue, but the same root
cause of "grew the page today without checking mobile").

---

## 2026-07-28 (LRX One umbrella, part 3) — Pricing sections now mention the suite

Third pass in this thread. User specifically asked to check
`billing.html`'s pricing section — its `#pricing` header (eyebrow
"Simple Pricing" + headline "Pay for what you use. Scale as you grow.")
had no suite mention. Checked `one.html`'s equivalent section too, for
the same reason every other single-page check in this thread turned up
a matching gap on the sibling page — found the identical omission there.

Added one `.section-sub` line under each pricing headline (the class
was already defined and used for centered intro copy elsewhere on both
pages, so no new CSS needed):
- `billing.html`: "LRX One Billing pricing is separate from LRX One
  Core's — each product in the LRX One suite is billed on its own plan."
- `one.html`: "LRX One Core pricing is separate from LRX One Billing's
  — each product in the LRX One suite is billed on its own plan."

Framed as a genuinely useful clarification, not just brand messaging —
someone landing on either pricing table might otherwise wonder whether
the price shown includes or relates to the other product's cost, given
both are now positioned as part of one suite. Verified with Playwright
screenshots of both `#pricing` sections specifically.

---

---

## 2026-07-28 (LRX One umbrella, part 2) — one.html and billing.html now mention the suite

Direct follow-up to the products-section fix below. User asked to check
whether the same "LRX One is the umbrella product suite" framing was
reflected on the two dedicated product pages — it wasn't. Grepped both
files for "LRX One" (excluding the "LRX One Core"/"LRX One Billing"
product names themselves) and found nothing but a single unrelated
footer link on `billing.html` ("LRX One" → lrxone.com, really just a
sign-in link, not a suite mention).

Added one sentence to each hero-desc, matching the framing used
everywhere else this session (`lrxone-website`'s pages, this site's own
`#products` section):
- `one.html`: "...Built in South Africa for businesses everywhere. It's
  part of LRX One, LRX Tech Group's product suite, alongside LRX One
  Billing."
- `billing.html`: "...ZAR-native, POPIA-compliant, developer-first. It's
  part of LRX One, LRX Tech Group's product suite, alongside LRX One
  Core."

Kept it to one sentence appended to the existing hero copy on both pages
— consistent with the "light touch" choice made for `index.html`'s
products section, not a hero redesign. Verified with Playwright
screenshots of both heroes (not just HTML validity) since this is
visible, above-the-fold copy.

---

---

## 2026-07-28 (LRX One umbrella) — Named LRX One as the product suite on the homepage

Follow-up to work done this session on `lrxone-website`: that site's
legal pages and sign-in page were corrected twice — first to acknowledge
`app.lrxone.com` serves both LRX One Core and LRX One Billing, then
again to frame "LRX One" correctly as LRX Tech Group's actual umbrella
product/suite rather than merely "the shared login." While fixing that
second point, checked whether this site (the actual corporate homepage)
reflected the same relationship anywhere — it didn't. `index.html`'s
`#products` section showed LRX One Core and LRX One Billing as two
fully independent, equal product cards with zero "LRX One" umbrella
mentions (confirmed via grep, not assumed).

Asked the user how far to take the fix: leave it, add a light umbrella
header above the existing cards, or restructure into one merged "LRX
One" section with Core/Billing as modules. **User chose the light-touch
option.**

- `#products`' header now reads: eyebrow "LRX One — Our Product Suite",
  then the existing headline, then a new one-line intro: "One account,
  two products — LRX One Core and LRX One Billing are both part of LRX
  One, LRX Tech Group's product suite."
- Added `.products-header .section-body { margin: 0 auto; }` since
  `.section-body` (used once before, in a left-aligned context) needed
  explicit centering to sit correctly inside this already-centered
  header block.
- The two product cards themselves are untouched — same content,
  same layout, same CTAs.
- Verified with a Playwright screenshot of just the `#products` section
  (not the whole page) rather than relying on HTML validity alone, since
  this was a visual/layout change.

---

## 2026-07-28 (footer follow-up) — Fixed footer link ordering/completeness across legal pages

User asked to check `billing.html`'s footer against `lrxone-website` —
that site has no `billing.html` (it's a lean single-product sign-in
site), so nothing to compare there. Checked `billing.html`'s footer
against this site's own other pages instead, which surfaced two real
inconsistencies:

1. `index.html`'s footer had all five legal links but Privacy was tacked
   on at the very end, after Contact — `one.html` and `billing.html`
   both insert it right after Terms. Reordered to match.
2. `privacy.html`'s footer only linked to Home, Terms, and Contact —
   `terms.html`, `refund-policy.html`, and `cancellation-policy.html`
   all cross-link to *every* sibling legal page, but `privacy.html` was
   missing Refund Policy and Cancellation Policy. Added both.

Also checked `lrxone-website`'s four legal pages' footers for the same
kind of drift — they all use one shared minimal footer (copyright + a
link back to lrxtechgroup.com, no link list), consistent with each
other by design. Nothing to fix there.

---

## 2026-07-28 (final one today) — Built a real privacy.html

Direct follow-up to the Terms/Refund/Cancellation work immediately
below, which had left privacy handling as a `mailto:...Privacy%20Policy
%20Request` placeholder — a known, explicitly-flagged gap. User asked
for the real page.

Adapted `lrxone-website`'s `privacy.html` the same way the other three
legal pages were adapted: matched this site's actual nav/footer
components instead of copying the "LRX | ONE" branding, and generalised
the body copy to cover both products hosted here ("our products", with
links to `/one.html` and `/billing.html`) instead of being LRX One
Core-specific. The processor table (AWS af-south-1, Anthropic, Stitch,
PayFast, Stripe, Microsoft, self-hosted Keycloak) carried over unchanged
— all of those are genuinely shared across both products, not specific
to one. The Information Officer details (Brandon Le Roux /
brandon@lrxtechgroup.com, Jessica Le Roux / jessica@lrxtechgroup.com,
the registered address) are real and carried over unchanged from the
source page.

- Replaced every `mailto:...Privacy%20Policy%20Request` placeholder
  site-wide: `index.html`'s footer, and the "your data" sections of
  `terms.html` and `cancellation-policy.html` (both now link
  `/privacy.html` directly instead of routing through an email request).
- Added a Privacy Policy footer link to every page that didn't already
  have one — `one.html`, `billing.html`, `terms.html`,
  `refund-policy.html`, `cancellation-policy.html` — so it's reachable
  from anywhere on the site, not just the homepage.
- Verified all seven HTML files on the site still parse cleanly.

This closes out the legal-pages gap PayFast's verification flagged
(Terms, Refund, Cancellation, and now Privacy) — everything the
"further information" request named is now real content on the site
rather than a placeholder.

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
