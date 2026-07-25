# MEMORY — lrxtechgroup-website

Running log of what's been done on this repo across sessions, so work can be picked
up without re-deriving context. Newest entries at the top. Update this file every
time you finish a unit of work here.

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
