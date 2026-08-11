# TODO — lrxtechgroup-website

Living backlog. Check items off (or move to MEMORY.md as a dated entry) as they're
done — don't just accumulate; keep this reflecting real, current state.

## Fixed 2026-08-11 (user-directed: "indicate on the websites that the pricing is still subject to changes")

- [x] Added a "pricing subject to change" note under the pricing grid on
      `one.html` and `billing.html` (the only two pages with real
      pricing tables — `lrxone-website` just links to these), linking
      to `terms.html#pricing`'s existing one-month-notice clause. See
      MEMORY.md.

## Fixed 2026-08-04 (Brandon's photo over-zoom fixed)

- [x] Root cause was a non-square (570x700) source fighting
      `object-fit: cover` on a square container, compounding the
      crop's own tightness. Replaced with a user-supplied
      near-square, well-framed source (1099x1122 → 1099x1099 →
      700x700), verified comparable framing to Jessica's card.
      Cache-busting bumped to `?v=4`. See MEMORY.md.

## Fixed 2026-08-04 (Jessica's photo swapped for a business portrait)

- [x] Replaced `jessica-le-roux.jpg` with a new business-appropriate
      portrait (dark green top, not the wedding dress) — already
      square/well-framed, no trim needed. Cache-busting bumped to
      `?v=3`. See MEMORY.md.

## Fixed 2026-08-04 (founder photo cache-busting added)

- [x] Added `?v=3`/`?v=2` query strings to the founder `<img src>`s so
      browsers stop serving the stale cached photo from before this
      session's swaps. Confirmed via md5sum the correct file was
      already live on `origin/main` — this was purely a caching
      issue, not a deploy issue. See MEMORY.md.

## Fixed 2026-08-04 (Brandon's photo: left-side space trimmed)

- [x] Trimmed 130px of empty black space off the left edge (700x700 →
      570x700), face now centered in the card. See MEMORY.md.

## Fixed 2026-08-04 (Brandon's photo swapped for a better-quality version)

- [x] Replaced `brandon-le-roux.jpg` again with a cleaner
      already-black-background version the user supplied directly
      (better edge quality than the `rembg` cutout). See MEMORY.md.

## Fixed 2026-08-04 (leadership photos replaced with real higher-res sources)

- [x] User supplied the actual higher-quality wedding photos requested
      below — cropped tight, background removed via `rembg`, composited
      onto the site's `#0E0E0E` black. Both `brandon-le-roux.jpg` and
      `jessica-le-roux.jpg` replaced in place at 700x700. See MEMORY.md.

## Fixed 2026-08-04 (leadership photos sharpened, real upscale still pending)

- [x] Applied unsharp-mask sharpening + higher JPEG quality re-save to
      both founder photos — crisper edges, no fabricated detail.
- [x] ~~Still only 700x700px.~~ Resolved above — real source photos
      now used instead of an AI upscale (which was deliberately
      declined, since that would fabricate facial detail on real
      people). See MEMORY.md.

## Fixed 2026-08-04 (leadership quote dashes changed to commas)

- [x] Brandon's "...deserve -" and Jessica's "...half the work -"
      dashes both changed to commas. See MEMORY.md.

## Fixed 2026-08-04 (product-name divider re-centered against both sides)

- [x] `.product-name .divider` `top: -3px` → `-2px`, now measured
      within 0.3px of center against both "LRX ONE" and "HIVE" (was
      only checked against one side previously). See MEMORY.md.

## Fixed 2026-08-04 (nav logo/text gap widened)

- [x] `.nav-logo` gap 10px → 18px across all 8 pages. See MEMORY.md.

## Fixed 2026-08-04 (nav wordmark scale-up re-applied)

- [x] The size revert two entries back was a misread of user intent —
      re-applied the 33px/19px scale-up across all 8 pages. See
      MEMORY.md.

## Fixed 2026-08-04 (nav wordmark text size reverted)

- [x] Reverted the "match logo height" text scaling — "LRX TECH /
      GROUP" back to its original 16px/9px sizing across all 8 pages.
      See MEMORY.md.

## Fixed 2026-08-04 (nav horizontal padding capped for large monitors)

- [x] `nav { padding: 0 5%; }` → `padding: 0 clamp(20px, 3vw, 56px);`
      across all 8 pages — logo no longer drifts further right the
      wider the monitor gets. See MEMORY.md.

## Fixed 2026-08-04 (nav wordmark scaled to match logo height)

- [x] `.nav-logo-text` ("LRX TECH / GROUP") scaled up to span the same
      52px height as the logo icon, across all 8 pages. See MEMORY.md.

## Fixed 2026-08-04 ("Continue to LRX One" destination corrected)

- [x] All 3 "Continue to LRX One" links (billing.html hero, one.html
      hero + bottom CTA) now point to `https://www.lrxone.com` instead
      of straight into `app.lrxone.com/login`. See MEMORY.md.

## Fixed 2026-08-04 (one.html hero: "See Pricing" button restored)

- [x] Added "See Pricing" (`#pricing`) back next to "Continue to LRX
      One" in `one.html`'s hero, matching `billing.html`'s hero-buttons
      layout. See MEMORY.md.

## Fixed 2026-08-04 (billing.html hero note matched to one.html)

- [x] Added the same hero-note text under `billing.html`'s hero
      buttons that `one.html` already has ("Already have a workspace,
      or ready to get started?..."), plus the `.hero-note` CSS that
      page never had. See MEMORY.md.

## Fixed 2026-08-04 ("Let's talk" link reworded)

- [x] `index.html`'s "See what each contact option is for" link →
      "Not sure which to pick?" (user's pick from 4 options). See
      MEMORY.md.

## Fixed 2026-08-04 ("Register Interest" removed, moved to LRX One)

- [x] Removed every "Register Interest" mailto CTA from `index.html`,
      `one.html` (hero + bottom CTA banner, including the now-unused
      tier-dropdown markup/CSS/script), and `billing.html` (hero).
      Registration now happens on `app.lrxone.com` via "Continue to
      LRX One." Left `billing.html`'s "Talk to Us" sales-contact CTA
      alone — different ask, out of scope. See MEMORY.md.

## Fixed 2026-08-04 (product cross-links, "Sign In" reworded)

- [x] `one.html`'s "LRX One Billing" hero mention and `billing.html`'s
      "LRX One Hive" hero mention now link to each other's product
      page.
- [x] "Sign In" (→ `app.lrxone.com/login`) reworded to "Continue to
      LRX One" across `one.html` (hero button, bottom CTA button, hero
      note) since it's a cross-domain handoff, not an in-page sign-in.
      See MEMORY.md.

## Fixed 2026-08-04 (billing copy: PayFast-only, af-south-1 de-jargoned)

- [x] Removed all Stitch/Stripe billing-integration claims (they were
      never actually built) across `billing.html`, `index.html`,
      `privacy.html`, `refund-policy.html` — now PayFast-only,
      matching `lrxone`'s real `billing-service`.
- [x] `af-south-1` reworded to `AWS South Africa` everywhere it
      appeared. See MEMORY.md.

## Fixed 2026-08-04 (products intro copy simplified)

- [x] `index.html`'s Products section intro no longer names both
      products individually — "One account, all LRX One products -
      part of LRX Tech Group's product suite." See MEMORY.md.

## Fixed 2026-08-04 (product-name divider re-centered)

- [x] `.product-name .divider` given `position: relative; top: -3px;`
      to fix a measured 2.7px optical low-bias from `vertical-align:
      middle` — now within 0.3px of "LRX ONE"'s true glyph-ink center.
      See MEMORY.md.

## Fixed 2026-08-04 (product-name pipe divider fixed)

- [x] `.product-name`'s `|` divider (between "LRX ONE" and "HIVE"/
      "BILLING") was taller than the surrounding text and plain white —
      replaced with a sized `.divider` span capped to text cap-height,
      colored `var(--light-grey)` (same grey as nav's "GROUP"). See
      MEMORY.md.

## Fixed 2026-08-04 (nav heading optically re-centered)

- [x] `.nav-logo-text` given `transform: translateY(13px)` across all 8
      pages so the heading's visual weight lines up with the logo's
      (bounding boxes were already centered, but optically the logo
      reads lower — see MEMORY.md for the measurement method).

## Fixed 2026-08-04 (nav logo enlarged, heading kept centered)

- [x] `.nav-logo-icon` height 36px → 52px across all 8 pages; heading
      text stays vertically centered beside it via the existing
      `align-items: center` on `.nav-logo`. See MEMORY.md.

## Fixed 2026-08-04 (logo recolored to match brand gold, all 5 repos)

- [x] Recolored the extracted logo mark's gradient from its original
      off-brand warm/orange tone (measured mean `#C7974A`) to the
      site's actual `--gold`/`--gold-dark` palette (`#D4AF37`/
      `#B8922E`), preserving the original shading structure. Same fix
      applied identically across `lrxone-website`, `lrxone`, and
      `lrxone-mobile`. See MEMORY.md.

## Fixed 2026-08-02 (real logo, both sites)

- [x] Extracted just the icon mark (not the full "LRX TECH GROUP" +
      tagline lockup) from the user-supplied logo artwork, background
      removed, and applied as both the favicon (`/favicon.ico` +
      `/images/favicon-*.png` + `apple-touch-icon.png`) and the nav
      logo (`/images/logo-mark.png`) across all 8 pages on this site
      and all 5 on `lrxone-website`. See MEMORY.md for the extraction
      method and verification.
- [ ] Only English/default favicon set generated — no dark/light-mode
      variant favicons (some browsers support `media` query icons).
      Not requested, flagged as a possible future nicety only.

## Fixed 2026-08-02 (leadership titles)

- [x] "Founder & CEO"/"Founder & COO" → "Co-Founder & CEO"/"Co-Founder
      & COO" for Brandon and Jessica Le Roux in `index.html`'s
      leadership section. Checked for other mentions across this site
      and the sibling repos — none found. See MEMORY.md.

## Fixed 2026-08-02 (pricing intro wording tweak)

- [x] "Each product in LRX One is billed separately." → "Each product
      in the LRX One suite is billed separately." on both `one.html`
      and `billing.html`. See MEMORY.md.

## Fixed 2026-08-02 (pricing intro simplified)

- [x] `one.html`/`billing.html` pricing intro no longer names both
      products explicitly — now "Each product in LRX One is billed
      separately," so it doesn't need editing every time a new product
      is added to the suite. `index.html` has no equivalent section
      (checked). See MEMORY.md.

## Fixed 2026-07-31 (org-wide rename) — "LRX One Core" → "LRX One Hive"

- [x] Renamed the product across all 8 live pages (`index.html`,
      `one.html`, `billing.html`, `privacy.html`, `refund-policy.html`,
      `terms.html`, `cancellation-policy.html`, `contact.html`),
      including the split gold/white span styling, the all-caps `LRX
      ONE | CORE` product-grid label, and URL-encoded mailto subject
      lines. See MEMORY.md.

## Fixed 2026-07-29 (WhatsApp footer link removed) — one.html

- [x] Removed the "WhatsApp" footer link from `one.html`, matching the
      earlier `billing.html` fix. See MEMORY.md.

## Fixed 2026-07-29 (hero eyebrow centered + "Introducing" added) — closer match to billing.html

- [x] Centered the "Coming Soon" badge and eyebrow line (not the whole
      hero - it's a two-column layout unlike billing.html's single
      centered column), and changed copy to "Introducing LRX One Core".
      See MEMORY.md.

## Fixed 2026-07-29 (hero eyebrow restacked, line removed) — matches billing.html

- [x] Removed the gold line next to "Coming Soon" and restacked "LRX
      One Core" underneath it - `.hero-eyebrow` now matches
      `billing.html`'s plain-block pattern exactly. See MEMORY.md.

## Fixed 2026-07-29 (pillars strip fixed for mobile) — all 5 tiles visible

- [x] Replaced the pillars band's horizontal-scroll layout (only ~3 of
      5 tiles visible, no scroll indicator) with a wrapping grid - 2
      columns on mobile, 5th tile spans full width. See MEMORY.md.

## Fixed 2026-07-29 (one.html mobile eyebrow fixed) — no more wrap-mess, dash removed

- [x] Shortened the hero eyebrow to "LRX One Core" (was "LRX One Core -
      Enterprise Operating System") - fixes both the mobile wrapping
      break and removes the dash. Checked the rest of the page's mobile
      layout too - nothing else broken. See MEMORY.md.

## Fixed 2026-07-29 (Home button added to product page navs) — one.html and billing.html

- [x] Added an always-visible "Home" nav link to `one.html` and
      `billing.html` (previously only the unlabeled logo linked home,
      and it was the only nav item left on mobile). See MEMORY.md.

## Fixed 2026-07-29 (WhatsApp footer link removed) — billing.html only

- [x] Removed the "WhatsApp" footer link from `billing.html`. See
      MEMORY.md.

## Fixed 2026-07-29 (highlighted pricing tier removed) — one.html and billing.html

- [x] Removed the "featured" highlight (dark background + gold top
      border) from `one.html`'s Business card and `billing.html`'s
      Growth card - all pricing tiers now render identically. See
      MEMORY.md.

## Fixed 2026-07-29 (Enterprise Billing tier priced) — R14,999/mo instead of "Custom"

- [x] `billing.html`'s Enterprise pricing card and tier-dropdown option
      now show R14,999/mo instead of "Custom"/"Custom pricing". See
      MEMORY.md.

## Fixed 2026-07-29 (pricing headline matched on one.html) — "Predictable plans. Scale as you grow."

- [x] Changed `one.html`'s pricing headline to match `billing.html`'s
      new wording for consistency. See MEMORY.md.

## Fixed 2026-07-29 (pricing headline reworded) — billing.html no longer implies pay-per-use refunds

- [x] "Pay for what you use. Scale as you grow." -> "Predictable plans.
      Scale as you grow." on `billing.html`'s pricing section - avoids
      implying clients could get refunds for unused quota. See
      MEMORY.md.

## Fixed 2026-07-29 (footer logo "GROUP" now grey) — matches nav logo, all 8 pages

- [x] Footer's "LRX TECH GROUP" now splits gold "LRX TECH" / grey
      "GROUP", matching the nav logo above it, across every page. See
      MEMORY.md.

## Fixed 2026-07-29 (custom dropdown extended to pricing tier pickers) — one.html and billing.html

- [x] Replaced the native tier-select `<select>` on both `one.html` and
      `billing.html`'s pricing CTA banners with a custom dropdown
      matching the earlier Contact email fix - toggle shows the current
      tier, menu highlights it, selecting a new tier updates the
      Register Interest/Talk to Us link's mailto subject. See MEMORY.md.

## Fixed 2026-07-29 (Billing footer link removed) — refund-policy.html only

- [x] Removed the "Billing" mailto footer link from `refund-policy.html`.
      See MEMORY.md.

## Fixed 2026-07-29 (nav-back link simplified) — "Back to lrxtechgroup.com" is now "Home"

- [x] Simplified the nav-back link text on `contact.html`, `terms.html`,
      `privacy.html`, `refund-policy.html`, and `cancellation-policy.html`
      from "Back to lrxtechgroup.com" to "Home". See MEMORY.md.

## Fixed 2026-07-29 (native select replaced with custom dropdown) — Open menu now on-brand everywhere

- [x] Replaced the native `<select>` with a custom button+menu
      disclosure component so the open list matches the dark/gold theme
      on every device, not just the closed box (the native `<select>`'s
      open list is OS-rendered on mobile and can't be styled). See
      MEMORY.md.

## Fixed 2026-07-29 (email dropdown restyled) — Custom gold chevron, centered value text

- [x] Restyled the homepage Email `<select>` to match the rest of the
      site: custom gold chevron instead of the browser default, centered
      bold gold text, and the same hover treatment used on `.btn-outline`
      buttons. See MEMORY.md.

## Fixed 2026-07-29 (Product card removed from contact grids) — lrxone.com isn't a contact resource

- [x] Removed the "Product → lrxone.com" card from `contact.html` and
      `index.html`'s contact grids - it's the app sign-in link, not a
      contact resource. See MEMORY.md.

## Fixed 2026-07-29 (contact.html intro copy) — "Every way you can contact us in One place"

- [x] Replaced `contact.html`'s intro paragraph with the shorter
      "Every way you can contact us in One place" line, "One" in gold.
      See MEMORY.md.

## Fixed 2026-07-29 (email dropdown + dedicated Contact Us page) — Real `<select>`, full contact.html added

- [x] Email card is now a real `<select>` dropdown (Sales/Support/
      Billing) instead of three stacked links.
- [x] Added `/contact.html` - every contact resource (Sales, Support,
      Billing, WhatsApp, Call, Product) as its own card with a
      description of what it's for. Nav-cta/footer "Contact" links
      across the site now point here instead of the homepage anchor.
      See MEMORY.md.

## Fixed 2026-07-29 (name-plate overlay removed) — Founder photos no longer show duplicate title text

- [x] Removed the "Founder & CEO"/"Founder & COO" overlay caption from
      both founder photos - that title already appears in the text next
      to each quote. See MEMORY.md.

## Fixed 2026-07-29 (Contact stops defaulting to email) — Added Call option, "Contact" links now go to the contact section

- [x] "Contact" links on `index.html` footer, and nav-cta/footer on
      `one.html`/`billing.html`/`terms.html`, now point to `#contact` /
      `/#contact` instead of jumping straight into a `mailto:` compose.
- [x] Added a "Call" contact card (`tel:+27620498603`) to `index.html`'s
      contact grid, same number as WhatsApp. See MEMORY.md.

## Fixed 2026-07-29 (email options added) — Contact card now offers Sales/Support/Billing instead of one address

- [x] Converted `index.html`'s Email contact card from a single
      `mailto:sales@` link into three options (Sales/Support/Billing),
      each its own `mailto:` link. See MEMORY.md.

## Fixed 2026-07-29 (scroll label removed) — Hero "Scroll" text dropped

- [x] Removed the "Scroll" label under the hero's animated line
      indicator, and the now-unused `.hero-scroll span` CSS rule. See
      MEMORY.md.

## Fixed 2026-07-29 (redundant website link removed) — Contact grid no longer links to lrxtechgroup.com

- [x] Removed the "Website → lrxtechgroup.com" card from `index.html`'s
      Contact grid - redundant since visitors are already on that site.
      Grid reflows to 3 cards (Email / WhatsApp / Product) with no CSS
      changes. See MEMORY.md.

## Fixed 2026-07-29 (pricing notice clause) — Terms of Service commits to one month's notice on pricing changes

- [x] `terms.html` Section 3 (Subscriptions and payment): split the vague
      "may change with reasonable notice" line into an explicit
      commitment - we reserve the right to change pricing, but will give
      at least one month's notice, applying only to billing periods
      starting after that notice period. See MEMORY.md.

## Fixed 2026-07-29 (Jessica photo replaced) — New photo swapped in with a tighter shoulders-up crop

- [x] Replaced `images/jessica-le-roux.jpg` with a crop of a new photo the
      user supplied, tuned so the "Founder & COO" caption overlay bar
      doesn't sit on her face. Sent candidates for approval, rebuilt the
      final approved framing from the original high-res source (the
      user's "use this" file was a blurry phone-zoomed screenshot of a
      preview, not a fresh source photo) to keep the site asset sharp.
      No HTML/CSS changes needed. See MEMORY.md.

## Fixed 2026-07-29 (photo re-crop) — Brandon's photo cropped tighter on the left

- [x] Re-cropped `images/brandon-le-roux.jpg`, keeping the right edge from
      the originally-approved crop fixed and trimming more off the left,
      re-centered to stay square. Approved and swapped in. See MEMORY.md.

## Fixed 2026-07-29 (post-deploy fix) — Hero "LRX"/"TECH" colour mismatch

- [x] User spotted this from a real screenshot of the live site after the
      main-branch deploy - the hero `<h1>` only gold'd "LRX", leaving
      "TECH" white. Widened the existing `.lrx-gold` span to cover both
      words, matching the nav logo's own LRX TECH (gold) / GROUP (white/
      grey) split. See MEMORY.md.

## Fixed 2026-07-29 (genuinely the last one today) — Brandon's real photo replaces BLR initials

- [x] Cropped the user's supplied photo (shoulders-up, face/second-person
      excluded) and sent it for approval before touching the repo -
      approved, then saved as `images/brandon-le-roux.jpg` and wired into
      the same `.founder-visual--photo` pattern Jessica's card already
      uses. Both leadership cards are now real photos, no CSS changes
      needed to add the second one. See MEMORY.md.
- [x] The earlier "Brandon still shows initials" open item is resolved -
      both founders now have real, consistently-styled photos.

## Fixed 2026-07-29 (actually the final one today) — Jessica's real photo replaces JLR initials

- [x] Added `images/jessica-le-roux.jpg` (resized/optimized) and wired it
      into the Leadership section's founder card, replacing the "JLR"
      initials placeholder. See MEMORY.md for the CSS approach.

## Fixed 2026-07-29 (really the final one today) — WhatsApp added to Contact section + both product pages

- [x] Added a WhatsApp card (`wa.me/27620498603`) to `index.html`'s
      `#contact` grid, alongside Email/Product/Website.
- [x] User confirmed they also wanted it on `one.html` and `billing.html`
      - added a "WhatsApp" footer link next to "Contact" on both, since
      neither page has a contact-grid to add a card to. See MEMORY.md for
      both entries.
- [ ] Not verified: whether 062 049 8603 actually has WhatsApp Business
      registered/active on it - the link format itself is correct
      regardless, but that's a real-world check outside what I can
      confirm from here.

## Fixed 2026-07-29 (yet later same day) — Sign In + product link removed from both product-page footers

- [x] `one.html` footer: removed "Sign In" and the "LRX One Billing" link.
      `billing.html` footer: removed the "LRX One" link (it never had a
      Sign In link in its footer). Both now read Home / Terms / Privacy /
      Refund Policy / Cancellation Policy / Contact. See MEMORY.md.
- [x] Confirmed `index.html` and all 4 legal pages' footers don't have
      this pattern (different link sets entirely) - left untouched, not
      overlooked.

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
**Done 2026-07-29 (later same day)** - [x] User confirmed "everywhere"
was meant literally. Extended the gold/white wordmark to every remaining
body-prose mention and footer link across all 7 pages, and removed the 3
remaining non-Register-Interest arrows (`index.html`'s "See Full Details
→" ×2, `one.html`'s "Sign In →", the 4 legal pages' shared "← Back to
lrxtechgroup.com"). Zero arrows and zero unstyled "LRX One" mentions
remain anywhere except `<title>`/`<meta description>` and `mailto:`
subject JS strings, which can't render HTML at all - not a scoping
choice, a hard technical constraint. See MEMORY.md for the full list and
the browser verification (legal-page prose, About section, footer link
styling all screenshotted).

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
