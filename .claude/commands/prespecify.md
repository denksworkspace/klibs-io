---
description: Interactively interview the developer to produce a compact, human-owned prespec — a problem/intent alignment doc reviewed before /specify.
---

# /prespecify

Run an **interactive interview** (in the style of `/grill-me` (https://github.com/mattpocock/skills/blob/733d312884b3878a9a9cff693c5886943753a741/skills/productivity/grill-me/SKILL.md)) that helps the developer author a *prespec* — a cheap, human-owned alignment artifact reviewed **before** a spec exists. The prespec aligns on the **problem and intent**; it may carry solution **pointers** but never solution **decisions** (those belong in the spec's §8). The developer supplies the thinking — you own consistency and boundary-enforcement, not the content.

## Input
$ARGUMENTS

A single argument: the feature `name` (e.g. `create authorization`). Slugify it (lowercase, hyphens) → `<name>`. If `$ARGUMENTS` is empty, STOP and ask for a name.

## Process

1. **Resolve paths.** `<name>` = slugified argument. Target file: `docs/specs/<name>/prespec.md`. If it already exists, read it and *resume* the interview from the first unfilled/weak section rather than overwriting.
2. **Read `.claude/templates/prespec.md`** for the exact structure, section blurbs, and mandatory/optional markers.
3. **Interview one section at a time.** For each section:
    - Show the section's short "what belongs here / what doesn't" blurb first, so the developer knows the altitude.
    - Ask for their vision in their own words. Ask **one section at a time** — never dump all sections at once.
    - **Cross-check the answer against everything already filled.** If it contradicts or is misleading relative to an earlier section, challenge it and resolve before moving on. Otherwise continue.
    - **Enforce the boundary (the most important job).** If the developer states a *design decision* ("we'll add a column", "use a daily job"), redirect it: it is not a prespec answer — capture it instead as a §3 *pointer* (candidate tool / research required / possible spike).
4. **Handle "I don't know" by kind:**
    - Unknown **solution** (which tool, how many calls, does an approach work) → park it as a §3 `research required` / `possible spike` pointer and move on. Do **not** insert `[NEEDS CLARIFICATION]` — that is a spec-stage device.
    - Unknown **problem/intent** in a mandatory field → do **not** move on. Push until the developer can state it. You cannot align on a problem no one can articulate.
5. **Mandatory to finish** (interview may not complete until these exist): §1 problem + desired state; §2 primary objective + out of scope; §4 at least one `PRESPEC-SC`. Optional/skippable: §2 secondary/trade-offs, all of §3, §5.
6. **Success criteria discipline (§4).** Each must be observable, measurable, and a *must-have*. Nice-to-haves go to §2 secondary objectives. Number them `PRESPEC-SC-01`, `-02`, … — these IDs are durable and travel downstream.
7. **Write `docs/specs/<name>/prespec.md`** from the template. Fill frontmatter: `name`, `author` (git config user.name), `created` (today's date), `status: Draft`. Keep the `reviewer`/`approved` lines commented until a human approves. Keep every answer short — this is a few-minutes artifact, not a spec.
8. **STOP.** Report:
    - Path of the file written.
    - The `PRESPEC-SC` list with IDs.
    - Any design decisions you redirected into §3 pointers (so the developer sees the boundary was applied).
    - Any solution-unknowns parked in §3.
    - Reminder: the prespec is `Draft`. A human reviews it and edits `status` to `Approved` (adding `reviewer`/`approved`) before `/spec-from-prespec <name>` can consume it.

The prespec is the deliverable. Do not generate a spec, plan, or code.
