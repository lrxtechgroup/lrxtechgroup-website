# MEMORY — lrxtechgroup-website

Running log of what's been done on this repo across sessions, so work can be picked
up without re-deriving context. Newest entries at the top. Update this file every
time you finish a unit of work here.

---

## 2026-08-04 (product-name pipe divider fixed) — "LRX ONE | HIVE" / "LRX ONE | BILLING" divider capped to text height, recolored to the nav's grey

User flagged (with a screenshot of the Products section) that the literal
`|` character between "LRX ONE" and "HIVE"/"BILLING" in `.product-name`
(32px/900-weight) rendered taller than the surrounding capital letters —
a plain pipe glyph's height comes from the font's full line-box metrics,
not the cap-height, so at that size/weight it visibly overshoots the
text it's supposed to separate. It also inherited the default white
text color rather than any deliberate brand grey.

Replaced the literal `|` with a sized `<span class="divider"></span>`
(`display:inline-block`, `width:3px`, `height:22px` — matched to this
font/weight's cap-height rather than the pipe glyph's box, `vertical-
align:middle`) and set its color to `var(--light-grey)` (`#888888`) —
the same grey the nav wordmark uses for "GROUP" under "LRX TECH".
Applied to both product cards ("LRX ONE | HIVE" and "LRX ONE |
BILLING"); confirmed no other page uses this pipe pattern. Verified via
headless-Chromium screenshot of the live Products section.

## 2026-08-04 (nav heading optically re-centered against the logo) — text nudged +13px down from bounding-box center

Follow-up to the "nav logo enlarged" entry below. User felt the heading
text ("LRX TECH / Group") still wasn't centered against the logo even
though `.nav-logo`'s `align-items: center` puts both elements' bounding
boxes at the exact same geometric center — confirmed via
`getBoundingClientRect()` (icon and text wrapper both centered at
y≈35.5 in the 72px nav).

The mismatch is optical, not geometric: measured actual ink/pixel
weight (screenshot → grayscale → per-row brightness centroid,
background-subtracted) shows the icon's visual weight centered around
y≈41.7 (the "hx" mark's ink is denser in its lower half) while the
two-line text block's visual weight centers around y≈31.8 (all-caps
text has no descenders, so it sits high in its own line box). That's
roughly a 10px gap between how "centered" each element actually reads.

Rather than guess, built a side-by-side mockup (three variants: 0px/
+5px/+10px shift) using the measured centroid as the "full correction"
reference point, got user sign-off on the +10px variant as close, then
iterated by two more explicit user-specified nudges (+2px, then +1px)
to land on the final `.nav-logo-text { transform: translateY(13px); }`
across all 8 pages.

User asked for the nav logo to be bigger, with the heading text ("LRX
TECH / GROUP") centered to the right of it. `.nav-logo` already used
`display: flex; align-items: center`, so growing `.nav-logo-icon`'s
height doesn't need any other layout change — the cross-axis centering
re-derives automatically against the taller icon. Applied the same
`height: 36px` → `52px` edit to `.nav-logo-icon` across all 8 pages
(confirmed byte-identical before scripting, same pattern as the earlier
favicon/logo rollout). Nav bar is 72px tall, so 52px still leaves
comfortable padding — no clipping. Verified via headless-Chromium
screenshot of the live nav (both cropped and full-page) before
committing.

## 2026-08-04 (logo recolored to match brand gold, all 5 LRX repos) — extracted mark's gradient replaced with the site's exact --gold/--gold-dark palette

Follow-up across all 5 `lrxtechgroup` repos: user asked to make sure
the logo's colour actually matched each site's colour scheme, not just
"looks gold." Checked, and it didn't quite — the extracted icon's own
gradient (sampled directly from pixel data: min `#513C1C`, max
`#F5C46F`, mean `#C7974A`) was measurably warmer/more orange-bronze
than this site's (and every sibling site/app's) actual brand gold
(`--gold: #D4AF37`, `--gold-dark: #B8922E` — confirmed identical across
`lrxtechgroup-website`, `lrxone-website`, and `lrxone`'s Tailwind
`brand` scale before doing anything, so there was one true target, not
three slightly different ones).

**Fix**: recolored the icon's gradient in place rather than replacing
it with a flat fill — computed each pixel's luminosity (contrast-
stretched across the icon's own observed brightness range) and used it
to interpolate between `--gold-dark` (shadow end) through `--gold`
(midtone) to a slightly brighter highlight tone (`#E8C66A`, matching
how this site's own buttons brighten toward their highlight edge) for
the lightest pixels. This keeps the ribbon-fold 3D shading structure
intact (the highlights and shadows are still there, in the same
places) while every pixel's actual hue now sits on the site's real
gold, not the artwork's original off-brand gradient. Verified by
resampling the recolored PNG: new mean `#D3AE40`, a near-exact match to
`--gold`'s `#D4AF37`.

Regenerated the full asset pipeline from this recolored master and
redeployed it over the previous (correctly-cropped, but off-color)
versions — same file set as before (`favicon.ico` + sized PNGs +
`apple-touch-icon.png` + `logo-mark.png`), just recolored. Same
regeneration applied identically to `lrxone-website`, `lrxone`'s
frontend favicon/nav assets (see that repo's own MEMORY.md — this pass
also fixed a real pre-existing bug there, a favicon link pointing at a
`favicon.svg` that never existed), and `lrxone-mobile`'s Android
launcher icons (see that repo's MEMORY.md).

**Verified**: local `http.server` + Playwright screenshot of both
sites' nav bars side by side with their own gold UI elements (the
"CONTACT"/"SIGN IN" buttons, "LRX TECH"/"LRX One" text) — icon and
surrounding brand gold now read as the same colour family instead of a
visibly warmer icon next to cooler UI gold.

---

## 2026-08-02 (real logo, both sites) — favicon + nav logo replaced with the actual LRX Tech Group mark

User uploaded the real logo artwork (`LRXTECHGROUPLOGO_FullColour.png`
— a black textured background, the gold ribbon "LX" icon mark, then
"LRX TECH GROUP" wordmark text and an "INNOVATE · INTEGRATE ·
ELEVATE" tagline below it) and asked to use just the icon mark — not
the full lockup — as both the favicon and nav logo, on this site and
`lrxone-website`.

**Extraction** (installed Pillow/numpy via pip, PyPI is allowlisted
through this sandbox's egress proxy even though Docker registries
aren't): auto-detected the icon's bounding box by scanning for
gold-colored pixel rows/columns rather than eyeballing crop
coordinates, found the row-gap that separates the icon from the
wordmark text below it, and cropped there. The source background was a
textured near-black (not flat #000, had visible grain/noise) — removed
it by ramping alpha from the pixel brightness (max channel), calibrated
against the actual measured brightness distributions of background vs.
gold pixels in this specific image so real anti-aliased edges stay
smooth while background noise speckle doesn't survive as faint grey
flecks (an earlier pass with a looser threshold visibly left some;
verified fixed by compositing onto a white background and inspecting
before finalizing, not just trusting the transparent PNG alone).

**Assets generated**: a rectangular transparent PNG for nav use, a
square-padded transparent version for browser-tab favicons (16/32/192/
512px + multi-size `.ico`), and a square version with a solid `#0E0E0E`
background for `apple-touch-icon.png` specifically — iOS doesn't
render alpha transparency on home-screen icons well, so that one
variant needed an opaque brand-matched backdrop instead of transparency
like the others.

**Applied to all 8 pages on this site** (`index.html`, `one.html`,
`billing.html`, `privacy.html`, `terms.html`, `refund-policy.html`,
`cancellation-policy.html`, `contact.html` — confirmed byte-identical
favicon `<link>` and nav-logo `<svg>` blocks across all 8 before
scripting the replacement, so one Python find-replace pass could safely
cover every file): the old inline-SVG data-URI favicon replaced with
real `<link rel="icon">`/`apple-touch-icon` tags pointing at
`/favicon.ico` and `/images/favicon-*.png`; the nav's inline SVG
hexagon-plus-"LRX"-text icon replaced with an `<img>` tag pointing at
`/images/logo-mark.png`. `.nav-logo-icon`'s CSS changed from a fixed
36x36 square (which would have squashed the new non-square mark) to
`height: 36px; width: auto`. The "LRX TECH GROUP" text lockup next to
the icon was left untouched — the request was to swap the icon
graphic, not replace the existing text treatment with the logo file's
own wordmark.

Same pass applied to `lrxone-website` (5 pages) — see that repo's
MEMORY.md for the sibling entry, since its nav previously had no icon
at all (text-only "LRX One"), so this added one rather than swapping
one.

**Verified**: local `http.server` + Playwright screenshot of the nav
bar on both sites (icon reads cleanly at 36-40px display height,
aligns with the text), plus a `curl` check that every new asset URL
(`/favicon.ico`, `/images/favicon-32.png`, `/images/apple-touch-icon.png`,
`/images/logo-mark.png`) returns 200 rather than 404.

---

## 2026-08-02 (leadership titles) — "Founder" → "Co-Founder" for Brandon and Jessica

User asked to change Founder to Co-Founder for both. Updated
`index.html`'s `#founder` leadership section: "Founder & CEO · LRX Tech
Group" → "Co-Founder & CEO" (Brandon Le Roux), "Founder & COO" →
"Co-Founder & COO" (Jessica Le Roux).

Checked for other mentions before touching only this one spot: grepped
every page on this site, `lrxone-website`, and `lrxone`'s docs/config.
Found no other title reference to either of them anywhere — `privacy.html`
names them as "Information Officer"/"Deputy Information Officer" (a
POPIA-specific designation, unrelated to Founder/Co-Founder), and
`lrxone`'s strategy docs use "founder" generically in planning prose,
not as a title field naming either person. So `index.html`'s leadership
section was the only real match.

**Verified visually**: local `http.server` + Playwright screenshot of
`#founder` after the edit — both cards now read "CO-FOUNDER & CEO" /
"CO-FOUNDER & COO".

---

## 2026-08-02 (pricing intro wording tweak) — "LRX One" → "the LRX One suite"

Follow-up to the entry below: user asked to change the just-simplified
pricing intro from "Each product in LRX One is billed separately." to
"Each product in the LRX One suite is billed separately." — same fix
on both `one.html` and `billing.html`.

---

## 2026-08-02 (pricing intro simplified) — dropped naming both products by name on `one.html`/`billing.html`

User flagged that the "Predictable plans. Scale as you grow." intro
paragraph named both products explicitly ("LRX One Hive pricing is
separate from LRX One Billing's - each product in the LRX One suite is
billed on its own plan.") and pointed out this doesn't scale — every
future product added to the suite would need this sentence edited
again. Replaced with a generic, product-count-agnostic version: "Each
product in LRX One is billed separately."

Checked `billing.html` as asked (it mirrors `one.html`'s copy, naming
the same two products in the opposite order) — same fix applied there.
`index.html` doesn't have this section at all (checked via grep, no
match), so no third page involved.

Also pulled in upstream changes that had landed on `origin/main` since
this session's local clone was last synced: the "LRX One Core" → "LRX
One Hive" rename (see the entry below) plus several other commits — a
plain `git pull --ff-only` before starting, no merge needed.

**Verified visually**: local `http.server` + Playwright screenshots of
the pricing section on both `one.html` and `billing.html` after the
edit — clean single-sentence intro, no leftover product names, layout
unaffected.

---

## 2026-07-31 (org-wide rename) — "LRX One Core" → "LRX One Hive" across every live page

User asked to systematically rename the product across all `lrxtechgroup`
repos: every occurrence of "Core" that's part of the product name "LRX
One Core" becomes "LRX One Hive" — not a generic find/replace of the
word "core".

Updated all 8 HTML pages that reference the product: `index.html`,
`one.html`, `billing.html`, `privacy.html`, `refund-policy.html`,
`terms.html`, `cancellation-policy.html`, `contact.html`. Two patterns
needed separate handling since a plain string search for "LRX One Core"
alone missed them:
- The gold/white two-span brand styling
  (`<span style="color:var(--gold)">LRX One</span> <span
  style="color:var(--white)">Core</span>`) used throughout body copy —
  the word "Core" is in its own span, not adjacent text.
- `index.html`'s products-grid product name, styled as `LRX ONE |
  CORE` (all-caps, no spaces around the pipe).
- URL-encoded `mailto:` links (`subject=LRX%20One%20Core`) in `one.html`
  and `index.html`'s "Register Interest" buttons.

Deliberately left `MEMORY.md`/`TODO.md` history untouched — those are a
changelog of what was true at the time each entry was written, not live
copy, so past entries still say "LRX One Core" describing the old name
correctly for that point in time. This entry and the one below are the
only place the new name enters these two files going forward.

**Verified**: `grep -rn '\bCore\b\|\bCORE\b'` across all `.html` files
after the change returns nothing — no leftover brand references to the
old name.

---

## 2026-07-29 (WhatsApp footer link removed) — one.html, matching the earlier billing.html fix

User asked for the same fix already applied to `billing.html`'s footer
("Contact" left, "WhatsApp" removed) - `one.html` had the identical
link. Removed it, so both product pages' footers now match.

---

## 2026-07-29 (hero eyebrow centered + "Introducing" added) — closer match to billing.html's copy and layout

Follow-up to the restack: user wanted the "Coming Soon" badge and
"LRX One Core" eyebrow centered (like `billing.html`'s hero, which is
centered because its whole hero is a single centered column), and the
copy changed to "Introducing LRX One Core" matching `billing.html`'s
"Introducing LRX One Billing".

`one.html`'s hero is a two-column grid (text left, dashboard mockup
right) rather than billing's single centered column, so centering the
*entire* hero-left would have pulled the headline/description/buttons
out of alignment with each other - not what was asked. Centered just
the badge and eyebrow instead:
- `.hero-badge`: `inline-block` → `block; width: fit-content; margin: 0
  auto 20px` so the pill self-centers regardless of siblings.
- `.hero-eyebrow`: added `text-align: center` (it's already a
  full-width block from the earlier restack, so centering its text
  centers the visible line).
- Copy: "LRX One Core" → "Introducing LRX One Core".

**Verified**: local `http.server` + Playwright, desktop and mobile -
badge and eyebrow both centered above the still-left-aligned headline,
matches the intent without forcing the whole two-column hero into
billing.html's single-column centered layout.

---

## 2026-07-29 (hero eyebrow restacked, line removed) — one.html now matches billing.html's pattern

User asked to remove the decorative gold line next to "Coming Soon" and
stack "LRX One Core" underneath the badge instead of beside it.
Discovered `billing.html`'s hero already does exactly this - its
`.hero-eyebrow` is a plain block `<p>` with no flex/line styling, so it
naturally falls onto its own row below `.hero-badge`. `one.html`'s
version had extra styling (`display: inline-flex; align-items: center;
gap: 10px;` plus a `::before` divider line) that kept it sitting beside
the badge on the same row instead.

Removed the `::before` rule and the flex/gap properties from `one.html`'s
`.hero-eyebrow`, matching `letter-spacing` to `billing.html`'s `0.35em`
too - now byte-for-byte the same pattern on both product pages.

**Verified**: local `http.server` + Playwright, desktop and mobile -
"LRX ONE CORE" now sits directly under "Coming Soon", no line, matches
`billing.html`'s "INTRODUCING LRX ONE BILLING" layout exactly.

---

## 2026-07-29 (pillars strip fixed for mobile) — all 5 tiles visible instead of horizontally scrolling

Follow-up to the mobile pass on `one.html`: the pillars band (Connect/
Automate/Analyze/Scale/Secure) had been flagged in that pass as
"intentionally horizontally-scrollable, contained, no page overflow" -
but the user pointed out that's exactly the problem: with no scroll
indicator, mobile users only ever see the first ~3 tiles (Automate/
Analyze/Scale peeking at the edge) and have no visual cue that Connect
and Secure exist off-screen. Effectively invisible.

Replaced the `flex; overflow-x: auto` layout with a CSS grid, reusing
the same gap+background hairline-divider technique this page already
uses for `.pricing-grid` (`gap: 1px; background: var(--gold-line)`,
each item's own `background: var(--dark)` painting over the gap lines).
5 columns on desktop (unchanged visually), 2 columns under 640px with
the 5th item (`Secure`) spanning both columns via `grid-column: 1 / -1`
so it doesn't leave a dangling half-empty row.

**Verified**: local `http.server` + Playwright - all 5 tiles visible
without scrolling at 390px (2+2+1 layout), desktop unchanged (5 across),
zero horizontal overflow.

---

## 2026-07-29 (one.html mobile eyebrow fixed) — hero-eyebrow no longer wraps into a mess

User sent a mobile screenshot showing the hero eyebrow badly broken:
"LRX One Core - Enterprise Operating System" (an `inline-flex` `<p>`
where the gold divider line, "LRX One", "Core", and the trailing
"- Enterprise Operating System" text were all separate flex items) was
overflowing its available width and wrapping across three ragged lines
next to the "Coming Soon" badge.

Shortened `.hero-eyebrow` to just "LRX One Core" (dropping "- Enterprise
Operating System" and its dash) - that fixed both the layout break and
the "remove the dash by LRX One Core" request in one change, since the
dash was part of the removed tail text. The full phrase isn't lost:
it's still in the `<title>`, the meta description, and the hero
description paragraph below. Left the decorative gold divider line
(`.hero-eyebrow::before`) in place - that's a design element used
consistently before eyebrow text sitewide, not literal "wording", and
wasn't part of what the user asked to remove.

Did a fuller mobile pass on the rest of the page per the user's "sort
out the mobile layout" ask - pillars strip (intentionally
horizontally-scrollable via `overflow-x: auto`, contained, no page-level
overflow), pricing grid, CTA banner with the tier dropdown, and footer
all check out clean at 390px with nothing else broken.

**Verified**: local `http.server` + Playwright mobile screenshots (after
letting the `fadeUp` reveal animations finish, since a too-early
screenshot makes still-fading-in content look missing rather than just
timing) - eyebrow now renders "COMING SOON — LRX ONE CORE" on one tidy
row, zero horizontal overflow anywhere on the page.

---

## 2026-07-29 (Home button added to product page navs) — one.html and billing.html

User asked for a "Home" button at the top of every screen when moving
away from the homepage. The five standalone utility pages (`terms.html`,
`privacy.html`, `refund-policy.html`, `cancellation-policy.html`,
`contact.html`) already had this via their `.nav-back` link (renamed to
"Home" earlier today). The gap was `one.html` and `billing.html`: their
nav only had a logo (which links to `/` but isn't labeled) plus
Products/Pricing/Leadership/Contact - and on mobile, `.nav-links`
disappears entirely (`display: none` under 768px), leaving *no* visible
navigation text at all, home or otherwise.

Fixed by wrapping the existing `<ul class="nav-links">` and a new
`<a class="nav-home" href="/">Home</a>` together in a `.nav-right` flex
container, so `nav` itself still has exactly two direct children (logo,
nav-right) and the `space-between` layout is unaffected. The mobile
`display: none` rule still only targets `.nav-links`, so "Home" stays
visible at every breakpoint while Products/Pricing/Leadership/Contact
still collapse as before (unchanged, pre-existing behavior - not part of
this request).

**Verified**: local `http.server` + Playwright, desktop (1200px) and
mobile (390px) screenshots of both pages - "Home" renders in the nav
row on desktop and survives alone on mobile after the rest of
`.nav-links` hides, zero horizontal overflow.

---

## 2026-07-29 (WhatsApp footer link removed) — billing.html only

User asked to remove the "WhatsApp" footer link specifically from
`billing.html`. Removed it there only - `one.html` has the identical
link but wasn't mentioned, so left untouched (same scope discipline as
the earlier Billing-link-on-refund-policy-only fix).

---

## 2026-07-29 (highlighted pricing tier removed) — one.html and billing.html

User asked to remove the highlighted/"featured" pricing card on both
product pages - `one.html`'s Business tier and `billing.html`'s Growth
tier each had a `.pricing-card.featured` modifier (dark background +
gold top border) marking them as the recommended plan. Removed the
`featured` class from both cards' markup and deleted the now-dead
`.pricing-card.featured` / `.pricing-card.featured::before` CSS rules
from both files (nothing else referenced them - not a documented
reusable pattern like the founder name-plate, just a one-off highlight
being explicitly removed).

**Verified**: local `http.server` + Playwright screenshots of both
pricing grids - all four cards on each page now render identically, no
leftover grid/column irregularities.

---

## 2026-07-29 (Enterprise Billing tier priced) — "Custom" replaced with R14,999/mo

User asked for `billing.html`'s Enterprise tier to have a real
subscription amount instead of "Custom" (which had been deliberately
kept as "Custom pricing" up to now - see 2026-07-28's product-page
migration entry, which explicitly left this tier untouched since it's
a different pricing model from `one.html`'s Core tiers).

Set it to **R14,999/mo**, following the existing ladder's tightening
multiplier pattern (Starter R999 → Growth R2,999 is ~3.0x, → Business
R6,999 is ~2.3x, → Enterprise R14,999 is ~2.1x - consistent economies
of scale, no arbitrary jump). Left the "+ 0.15% per transaction" fee
untouched - only the base subscription amount was asked for. Updated
both the pricing card (`pricing-price`) and the tier-dropdown option
label ("Enterprise - Custom pricing" → "Enterprise - R14,999/mo") so
they stay in sync.

**Verified**: local `http.server` + Playwright - card renders cleanly
alongside the other three tiers, dropdown shows the new price, and
selecting "Enterprise" still correctly updates the register-interest
mailto link's subject line.

---

## 2026-07-29 (pricing headline matched on one.html too) — "Predictable plans. Scale as you grow."

Follow-up to the `billing.html` pricing headline fix: user asked to
change `one.html`'s equivalent headline as well. It didn't have the
same pay-per-use wording ("Start small. Scale when you're ready." -
no refund-implying language), but changed it to match `billing.html`'s
new "Predictable plans. Scale as you grow." for consistency between
the two product pages.

---

## 2026-07-29 (pricing headline reworded) — "Pay for what you use" removed from billing.html

User flagged that "Pay for what you use. Scale as you grow." (the
`billing.html` pricing section headline) implies usage-based billing
where unused quota might be refundable - not the actual model (flat
subscription tiers, non-refundable per the Refund Policy except when
charged after cancellation). Changed to "Predictable plans. Scale as
you grow." - same rhythm, no pay-per-use implication. `one.html` never
had this phrase, so no change needed there.

While checking this, also re-verified the "LRX One Billing pricing is
separate from LRX One Core's..." paragraph the user asked about
earlier (color mismatch report) - confirmed via fresh screenshot that
all three "LRX One" mentions render consistently gold in the current
source on both `one.html` and `billing.html`. That was a stale-cache
issue on the user's end, not a real bug - no code change was needed for
that part.

---

## 2026-07-29 (footer logo "GROUP" now grey) — matches the nav logo's treatment at the top of every page

User asked for the footer's "GROUP" to be grey, "like the top of page" -
the nav logo already splits "LRX TECH" (gold) from "GROUP" (grey,
`.nav-logo-text .tech`), but the footer's `.footer-logo` span rendered
"LRX TECH GROUP" as one uniformly gold string. Wrapped just "GROUP" in
`<span style="color:var(--light-grey)">` inside `.footer-logo` on all
eight pages that share this identical footer markup: `index.html`,
`one.html`, `billing.html`, `terms.html`, `privacy.html`,
`refund-policy.html`, `cancellation-policy.html`, `contact.html`.

**Verified**: local `http.server` + Playwright screenshot of
`index.html`'s nav and footer together - both now read gold "LRX TECH"
+ grey "GROUP" consistently.

---

## 2026-07-29 (custom dropdown extended to pricing tier pickers) — one.html and billing.html

User sent a screenshot of the same native-select problem on `one.html`'s
"Ready to elevate your business?" tier picker (the same grey Android
system popup as the earlier Contact email dropdown), asking for the
custom dropdown treatment there too.

This picker's behaviour is different from the email one: it's a
*persisted* selection, not a one-shot action - selecting a tier doesn't
navigate anywhere itself, it updates a separate "Register Interest"
(`one.html`) / "Talk to Us" (`billing.html`) button's `mailto:` href to
include the chosen tier in the subject line, and stays showing that
tier until changed again. Built a second custom dropdown component
(`.tier-dropdown`) reusing the same visual language (dark background,
gold border/chevron, `--gold-dim` hover) as the email dropdown, but with:
- A toggle button showing the *currently selected* tier's label (not a
  placeholder), matching the site's existing button sizing
  (`.btn-primary` proportions) so it sits naturally next to the CTA
  button in the flex row.
- Menu options are `<button>`s (not links) that set `aria-current="true"`
  on the chosen one (styled gold) and update the toggle label + the CTA
  link's `href` via the existing `updateLink(tier)` logic, then close.

Applied to both `one.html` (Starter/Growth/Business/Enterprise -> Core
CTA, default "Business") and `billing.html` (Starter/Growth/Business/
Enterprise -> Billing CTA, default "Growth") - both pages had the
identical native-`<select>` pattern before this fix. Removed the old
`.tier-select` CSS rule from both (fully replaced, no longer used).

**Verified**: local `http.server` + Playwright on both pages - closed
state matches the button sizing next to the CTA, open state shows the
dark/gold menu with the current tier highlighted, selecting a new tier
(tested "Enterprise" on `one.html`) correctly updates the register
link's `href` (`mailto:...?subject=LRX%20One%20Core%20-%20Enterprise`),
zero horizontal overflow on mobile, no native OS popup on either page.

---

## 2026-07-29 (Billing footer link removed) — refund-policy.html only

User asked to remove the "Billing" mailto footer link specifically from
`refund-policy.html`. Removed it there only - `cancellation-policy.html`
has the identical link but wasn't mentioned, so left untouched to avoid
overreaching beyond what was asked.

---

## 2026-07-29 (nav-back link simplified) — "Back to lrxtechgroup.com" is now just "Home"

Applied to all five pages sharing this exact nav-back link/markup:
`contact.html`, `terms.html`, `privacy.html`, `refund-policy.html`,
`cancellation-policy.html`. User only showed a screenshot of
`contact.html`, but since all five use the identical
`<a class="nav-back" href="/">Back to lrxtechgroup.com</a>` markup,
changed it consistently everywhere rather than leaving the other four
inconsistent.

---

## 2026-07-29 (native select replaced with custom dropdown) — Open menu now matches the site too, not just the closed box

User sent a screenshot showing the actual problem with the earlier
"restyled" select: once tapped open on Android Chrome, the option list
is rendered by the OS/browser itself (grey system popup) and CSS can't
touch it - only the closed box was ever going to look on-brand. Asked
whether to build a proper custom dropdown; user chose that over keeping
the native select or reverting to three stacked links.

Replaced `<select class="contact-email-select">` in `index.html` with a
custom disclosure component:
- `<button class="contact-email-toggle" aria-haspopup aria-expanded>`
  showing the same closed-box look as before (dark background, gold
  border/text, chevron that rotates 180° when open).
- `<ul class="contact-email-menu" role="menu" hidden>` of plain
  `mailto:` links (`role="menuitem"`), styled with the card's `--dark`
  background, gold border, and a gold-dim hover per item - fully
  CSS-controlled since it's real page DOM, not an OS control.
- Small vanilla JS block (next to the existing footer-year script):
  toggle on click, close on outside click, close on Escape (returning
  focus to the toggle), and close whenever a menu link is clicked.

**Verified**: local `http.server` + Playwright - closed state pixel-
matches the prior restyle, open state renders the dark/gold menu on both
desktop and mobile viewports (no native OS popup), outside-click-closes
confirmed via `menu.hidden` after a click elsewhere, zero horizontal
overflow at 390px.

---

## 2026-07-29 (email dropdown restyled) — Custom gold chevron and centered value text to match the rest of the site

User said the Email `<select>` should "have the same feel as the rest
of the website" - it was rendering with the browser's default OS-styled
dropdown arrow and left-aligned text, standing out against the
hand-styled dark/gold cards around it. Restyled `.contact-email-select`
in `index.html`:

- `appearance: none` + an inline SVG gold chevron (matching `--gold`,
  `#D4AF37`) replacing the native arrow.
- Centered text (`text-align` + `text-align-last`) at 14px/600 weight,
  matching the sibling WhatsApp/Call cards' `.contact-value` styling
  instead of a plain 13px left-aligned box.
- Hover/focus now uses the same `border-color: var(--gold); background:
  var(--gold-dim)` treatment already used on the site's `.btn-outline`
  buttons, instead of just a border-color change.
- Dropdown option list background changed from pure black to `--dark`
  (matches card backgrounds) with left-aligned option text (the
  native OS option list can't take the custom chevron/centering, but
  the closed-box appearance is what needed to match).

**Verified**: local `http.server` + Playwright screenshots (default,
hover, mobile) - zero horizontal overflow, chevron and hover state
render correctly.

---

## 2026-07-29 (Product card removed from contact grids) — lrxone.com isn't a contact resource

User pointed out the "Product → lrxone.com" card didn't belong in
either contact grid - it's the sign-in link to the application, not a
way to reach a person or team. Removed it from both `contact.html`'s
resource grid (now 5 cards: Sales/Support/Billing/WhatsApp/Call) and
`index.html`'s homepage "Let's talk" quick-access section (now 3 cards:
Email/WhatsApp/Call). lrxone.com is still reachable everywhere else it
belongs (nav, footer product links, pillars band on lrxone.com itself)
- this was specifically about the contact-purpose grids.

**Verified**: local `http.server` + Playwright, desktop and mobile,
both pages - grids reflow cleanly with no dangling cells, zero
horizontal overflow.

---

## 2026-07-29 (contact.html intro copy) — Simplified to "Every way you can contact us in One place"

User asked to replace the intro paragraph with a shorter line, with
"One" in gold - a small wordplay on the product name. Changed
`contact.html`'s `.doc-intro` from the earlier "Every way to reach LRX
Tech Group, and what each one is for..." to "Every way you can contact
us in <span style="color:var(--gold)">One</span> place."

---

## 2026-07-29 (email dropdown + dedicated Contact Us page) — Real `<select>` for department, full contact.html added

Two follow-ups to the Email card added earlier today:

1. **Dropdown instead of stacked links**: the three-link stack
   (Sales/Support/Billing) worked but wasn't what the user asked for -
   they wanted an actual dropdown selection. Replaced it with a
   `<select class="contact-email-select">` styled to match the site's
   dark/gold theme, with an inline `onchange` handler
   (`window.location.href = this.value`) that opens the right
   `mailto:` link, then resets the select back to the placeholder so it
   can be used again. Removed the now-unused `.contact-email-options` /
   `.contact-email-option` / `.contact-email-role` /
   `.contact-email-address` CSS from the earlier version.

2. **New `contact.html` page**: a dedicated Contact Us page listing
   every contactable resource (Sales, Support, Billing emails,
   WhatsApp, Call, Product/lrxone.com) as its own card with a short
   description of what to contact that resource for - e.g. Billing's
   card links out to the Refund and Cancellation policies, Sales'
   explains it's for pricing/demos/setup. Built on the same
   nav/footer shell as `terms.html`/`privacy.html` (logo + "Back to
   lrxtechgroup.com", plain footer link list) rather than the
   full marketing nav, matching how the other standalone utility pages
   are built.
   - Updated the nav-cta and footer "Contact" links on `index.html`,
     `one.html`, `billing.html`, and `terms.html` (previously pointing
     at `#contact` / `/#contact`, from this morning's earlier fix) to
     point at `/contact.html` instead, since it's now the fuller,
     canonical contact destination.
   - Left `refund-policy.html`/`cancellation-policy.html` alone (they
     only ever had a "Billing" mailto link, no "Contact" link, and
     adding one wasn't asked for) and `privacy.html`'s POPIA Information
     Officer link alone (different purpose, as before).
   - The homepage's inline `#contact` "Let's talk" section stays in
     place for quick access, now with a text link ("See what each
     contact option is for") pointing to the new page for the fuller
     picture.

**Verified**: local `http.server` + Playwright, desktop and mobile, both
`index.html`'s contact section and the new `contact.html` - zero
horizontal overflow on either. Confirmed the select's `value` resolves
to the correct `mailto:` target per option via `page.evaluate`
(mailto navigation itself hands off to the OS mail client, so it isn't
observable as an in-page navigation in headless Chromium - expected
browser behaviour, not a bug).

---

## 2026-07-29 (name-plate overlay removed) — Founder photos no longer have duplicate title text on top of them

User pointed out the role captions overlaid on the founder photos
("Founder & CEO" / "Founder & COO", the `.founder-name-plate` bottom
gradient bar) duplicated the same text already shown just below/beside
each photo in `.founder-meta` ("Brandon Le Roux" / "Founder & CEO ·
LRX Tech Group"). Removed the `.founder-name-plate` `<div>` from both
founder cards' `.founder-visual--photo` markup, leaving clean photos.

Left the `.founder-name-plate` / `.founder-visual--photo
.founder-name-plate` CSS rules in place even though nothing currently
uses them - they're an existing, documented fallback pattern (caption
under plain initials, or overlay bar on a photo) for any future team
member added without their own photo yet, not something newly dead from
this change.

**Verified**: local `http.server` + Playwright screenshot of the
Leadership section - both photos are now clean, titles still visible in
the text beside each quote.

---

## 2026-07-29 (Contact stops defaulting to email) — Added Call option, "Contact" links now go to the contact section

User caught this from another live screenshot: the footer's "Contact"
link (`index.html`, and the nav-cta/footer "Contact" links on `one.html`,
`billing.html`, `terms.html`) was a `mailto:sales@lrxtechgroup.com` link
that jumped straight into an email compose with no choice of channel or
address - even though the homepage already had a full Contact section
with Email/WhatsApp/Product options.

- Changed all of those "Contact" links to point at `#contact` (same-page
  anchor on `index.html`) or `/#contact` (root-relative, from the other
  pages), so pressing "Contact" anywhere on the site lands on the full
  contact section instead of defaulting to one inbox.
- Added a fourth contact-item on `index.html`: "Call" (`tel:+27620498603`),
  the same number as the WhatsApp card, per the user's explicit request.
- Deliberately left `privacy.html`'s footer "Contact" link
  (`mailto:brandon@lrxtechgroup.com`) untouched - that's the POPIA
  Information Officer contact required on the privacy policy, a
  different purpose from general site navigation.

**Verified**: local `http.server` + Playwright mobile screenshot of the
now-4-card contact grid (Email/WhatsApp/Call/Product) - stacks cleanly,
zero horizontal overflow.

---

## 2026-07-29 (email options added) — Contact card now offers Sales/Support/Billing instead of one address

User caught this from a live screenshot: the Contact section's Email
card was a single `mailto:sales@lrxtechgroup.com` link, so every inquiry
- sales, support, or billing - went to the same inbox with no way to
pick. Since this is a static site with no backend, a JS-driven picker
would be an over-engineered fix for what a plain link list already
solves: converted the card from a single `<a>` into a `<div>` containing
three separate `mailto:` links (Sales, Support, Billing), each with its
own small role label above the address, reusing `billing@lrxtechgroup.com`
(already live per the Refund/Cancellation policy pages) and
`sales@lrxtechgroup.com` (already live sitewide), and adding
`support@lrxtechgroup.com` as the new third option.

Left the footer's separate `mailto:sales@lrxtechgroup.com` "Contact"
shortcut link untouched - that's a different element the user didn't
ask about, out of scope for this fix.

**Verified**: local `http.server` + Playwright screenshot, desktop and
mobile - the grid's other two cards stretch to match the taller Email
card's height automatically (no CSS changes needed), zero horizontal
overflow on mobile.

---

## 2026-07-29 (scroll label removed) — Hero's "Scroll" text label dropped, indicator line kept

User asked to remove the word "Scroll" from the hero. Removed the
`<span>Scroll</span>` from `.hero-scroll` in `index.html`, keeping the
animated gold `.scroll-line` indicator itself. Also removed the now-dead
`.hero-scroll span` CSS rule (nothing else on the site used that
selector).

**Verified**: local `http.server` + Playwright mobile screenshot of the
hero after the reveal animation - line indicator still pulses, no
leftover label text.

---

## 2026-07-29 (redundant website link removed) — Contact grid no longer links to the page you're already on

User spotted this from a live screenshot: the Contact section's grid had
a "Website → lrxtechgroup.com" card, which is redundant since anyone
seeing it is already on lrxtechgroup.com. Removed that `.contact-item`
from `index.html`, leaving Email / WhatsApp / Product (lrxone.com). The
grid uses `repeat(auto-fit, minmax(220px, 1fr))`, so it reflowed cleanly
to 3 balanced cards with no CSS changes needed. Checked one.html and
billing.html for the same pattern - neither had it, this was
index.html-only.

**Verified**: local `http.server` + Playwright screenshot of the Contact
section, desktop and mobile, zero horizontal overflow.

---

## 2026-07-29 (pricing notice clause) — Terms of Service now commits to one month's notice on pricing changes

User asked for an explicit commitment in the Terms: LRX Tech Group
reserves the right to change pricing, but will give at least one
month's notice before a change takes effect. The existing subscriptions
clause (`terms.html`, Section 3) only had a vague "may change with
reasonable notice" line - split it into two bullets: one on plan
features/limits as published at signup, and a new one stating the
one-month notice commitment explicitly, and that a pricing change only
applies to billing periods starting after that notice period (so it
can't be applied retroactively mid-cycle).

---

## 2026-07-29 (Jessica photo replaced) — New photo swapped in, shoulders-up crop tuned to clear the caption bar

User uploaded a new photo of Jessica (different from the first one used
earlier) and asked for the same shoulders-up treatment as Brandon's card,
with an explicit constraint: the crop had to leave enough room below her
chin that the "Founder & COO" caption overlay bar (the bottom gradient
bar from `.founder-visual--photo .founder-name-plate`) wouldn't sit on
her face.

Produced two square candidate crops from the original photo (a wider one
showing more shoulder/dress, a tighter one closer to Brandon's framing)
and sent both for approval. User then sent back a phone screenshot/zoom
of the tighter candidate as "use this" - that file was a blurry upscale
(1289×1415, clearly a pinch-zoomed re-crop of the 700×700 preview I'd
sent, not a fresh source photo), so rather than ship a soft image to
production, the same tighter framing was rebuilt from the original
high-resolution source photo to keep the final asset sharp. Sent that
rebuild for a final check; approved ("Yes, use that one").

Saved over `images/jessica-le-roux.jpg` (crop resized to 700×700, JPEG
quality 85, optimize=True - same pipeline as Brandon's photo). No
HTML/CSS changes needed - the `<img>` tag already pointed at this
filename.

**Verified**: local `http.server` + Playwright screenshot of the
Leadership section - caption bar sits cleanly below her chin with no
overlap, framing matches Brandon's card, zero horizontal overflow on
mobile viewport.

---

## 2026-07-29 (photo re-crop) — Brandon's photo re-cropped tighter on the left

Follow-up to the approved Brandon photo: user asked to crop more from the
left. First attempt shifted the whole crop window right (which also moved
the right edge) - user corrected that: "leave the right as it was." Redid
it properly: kept the original approved crop's right edge (x=1960 in the
source photo's coordinate space) fixed, only moved the left edge in
(660 → 790), then re-centered top/bottom around the same vertical midpoint
to keep the crop square (1170×1170) rather than feeding a non-square
source into the `object-fit: cover` square avatar box, which would have
let the browser do its own unpredictable additional crop at render time.
Sent for approval again before touching the repo - same approval-gated
workflow as the original photo.

Approved, then re-saved over `images/brandon-le-roux.jpg` (resized to
700×700, same optimization settings as before) - no HTML/CSS changes
needed, the `<img>` tag already pointed at this filename.

**Verified**: screenshotted the Leadership section again post-swap -
tighter left crop, same headroom/shoulder framing as before, still
consistent with Jessica's card.

---

---

## 2026-07-29 (post-deploy fix) — Hero headline's "LRX"/"TECH" colour mismatch

User caught this from a real screenshot of the live site after the deploy
merge, not a code read: the hero `<h1>` only wrapped `LRX` in
`.lrx-gold`, leaving `TECH` (and `GROUP`, on its own line) in the default
white body colour — same wordmark as the nav logo (`LRX TECH` gold,
`GROUP` grey subtitle), but rendered inconsistently in the hero.  Widened
the existing span to cover both words: `<span class="lrx-gold">LRX
TECH</span><br/>GROUP` — no new CSS, `GROUP` intentionally left white,
matching the nav logo's own LRX TECH (gold) / GROUP (secondary) split.

**Verified**: screenshotted the hero at mobile width (412px) post-fix -
"LRX TECH" reads as one consistent gold line, "GROUP" white beneath it,
zero horizontal overflow. Committed straight to `main` (confirmed deploy
path: this host auto-deploys from `main`), not a feature branch.

---

---

## 2026-07-29 (genuinely the last one today) — Brandon's real photo replaces BLR initials, both founder cards now match

User supplied a full-length wedding-style photo (him + his son, both in
tuxedos); the brief was specifically "shoulder-up crop" for the leadership
card, and to send the crop for approval before wiring it in - a real
gate, not a formality, given the source photo included a second person
who obviously shouldn't end up on a company leadership card. Cropped with
Pillow to an approval-first workflow: computed a candidate square region
around his face/shoulders, rendered it to the session scratchpad, sent it
to the user via `SendUserFile` as a preview (not committed, not even
written into `images/` yet), and only wrote the real asset + wired it into
`index.html` after explicit approval ("use that for Brandon card").

Saved as `images/brandon-le-roux.jpg` (700×700, JPEG quality 85, 61KB) -
same optimization treatment as Jessica's photo. Swapped his founder-card
markup to the same `.founder-visual--photo` pattern built for her (no new
CSS needed, confirming that pattern was worth building generically rather
than one-off): `<img class="founder-photo" ...>` replacing the `BLR`
initials div, `.founder-name-plate` automatically becomes the bottom
overlay caption via the modifier class already in place.

Both leadership cards are now real photos, consistently styled - the
initials-placeholder state (`.founder-initials`, still defined in CSS)
has no remaining callers on this page but wasn't deleted, since it's a
generic building block a future third leadership card could still want
before having a photo ready.

**Verified**: screenshotted both cards together at desktop (1280px) -
same gold top border, same caption-overlay treatment, same crop tightness
- and confirmed zero horizontal overflow on mobile (375px).

---

---

## 2026-07-29 (actually the final one today) — Real photo replaces Jessica's "JLR" initials placeholder

User supplied a headshot; saved to `images/jessica-le-roux.jpg` (resized
from the original 1125×1513 to 700×941 via Pillow, EXIF-orientation-
corrected, JPEG quality 85 — 77KB vs the original 305KB, since this repo
had no image-asset pipeline before this and a full-res upload was
unnecessarily heavy for a small avatar box). This is the first real image
file in a repo that was previously 100% inline HTML/CSS/SVG (even the
favicon is a data URI) — new `images/` directory.

`.founder-visual` was built for two states (initials-on-dark-background,
unchanged for Brandon's card, which has no photo yet) and now also a
photo variant: added `.founder-visual--photo` modifier + `.founder-photo`
(absolutely positioned, `object-fit: cover`, `object-position: center
22%` to keep the face framed rather than the empty ceiling space visible
in the original photo) and turned `.founder-name-plate` into a bottom
overlay bar (dark gradient behind white text) only when that modifier
class is present — Brandon's card keeps its original centered-under-
initials layout untouched. Also added `z-index: 1` to `.founder-visual`'s
existing gold top-border pseudo-element, which is also absolutely
positioned inset — without it the new photo (also absolute, `inset: 0`)
would have painted over that 2px gold line.

**Verified**: screenshotted the Leadership section at desktop (1280px)
and mobile (375px) — face is well-framed at both sizes, the gold top
border and "FOUNDER & COO" caption both render above the photo as
intended, zero horizontal overflow on mobile.

---

---

## 2026-07-29 (really the final one today) — WhatsApp added to one.html/billing.html footers too

Follow-up to the same-day `index.html` contact-grid entry - user confirmed
they wanted it "on both website pages" too (the two product pages,
matching the "both sites" phrasing used earlier this session for the
footer Sign-In/product-link removal). Neither page has a contact-info hub
like `index.html`'s, just a bare `mailto:` "Contact" footer link, so added
a matching `WhatsApp` (`wa.me/27620498603`, new tab) `<li>` right after
Contact in both `one.html`'s and `billing.html`'s `.footer-links` lists -
same list style/hover treatment as every other footer link, no new UI
invented.

**Verified**: screenshotted both footers at desktop (1280px) and mobile
(375px) - `.footer-links` already had `flex-wrap: wrap` from an earlier
mobile-overflow fix this session, so the extra item wraps cleanly with
zero horizontal overflow at either width, confirmed programmatically
(`scrollWidth - clientWidth === 0`) on all 4 combinations, not just visual
spot-checks.

---

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
