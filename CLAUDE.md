# Standing instructions for this repo

## Always keep MEMORY.md and TODO.md current — every session, every unit of work

This repo has two persistent context files:

- **`MEMORY.md`** — a dated changelog. Every time you finish a unit of
  work here (a fix, a feature, an investigation with a real conclusion),
  add a new dated entry at the top describing what was done and why.
- **`TODO.md`** — the living backlog. Check items off (or remove them) as
  they're resolved; add new items as they're found. Keep it reflecting
  real, current state, not a stale snapshot.

**Both files, every time — not one or the other.** Checking an item off
in `TODO.md` is not a substitute for a `MEMORY.md` entry. If you make a
commit that fixes something, investigates something, or changes the
state of this repo in any way worth remembering, that commit (or the one
right after it) must update both files. Before ending a work session or
handing off, verify: does `git log -1 -- MEMORY.md` match `git log -1 --
TODO.md` match (roughly) the actual `HEAD`? If `MEMORY.md` is stale
relative to real commits that happened, that's a bug in the session —
fix it before moving on, don't let it accumulate into a backlog of
undocumented work.

This instruction was added after a real gap: several commits' worth of
substantive work (Android native scaffolding fixes, a Gradle
configuration bug, new test coverage — in a sibling repo,
`lrxone-mobile`) landed with `TODO.md` kept current but `MEMORY.md` never
touched, discovered only when the user asked directly whether both files
were being maintained. Don't let that happen again, here or elsewhere.
