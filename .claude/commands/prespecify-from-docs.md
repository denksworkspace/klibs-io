---
description: Seed a prespec from an existing document (e.g. a YouTrack ticket), then run the same per-section /prespecify interview with each section pre-filled for edit-or-approve.
---

# /prespecify-from-docs

A seeded variant of `/prespecify`. Read a source document that already contains a human's vision (e.g. a YouTrack ticket), pre-fill each prespec section from it, then walk the sections one at a time — each arriving **pre-filled**, for the developer to **edit or approve** instead of writing from scratch.

This command **inherits everything from `/prespecify`** — the template `.claude/templates/prespec.md`, the 5 sections, section blurbs, mandatory/optional markers, success-criteria discipline, cross-checking, `status: Draft` output, and the downstream `/spec-from-prespec` gate. Only the differences below apply.

## Input
$ARGUMENTS

**One argument: the source.** No name is passed — the name is proposed in Step 0.
- **File path** (`docs/tickets/KTL-1234.md`) → read it.
- **URL** (`http…`) → best-effort fetch. If it returns a login/auth wall or an error page, **STOP** and ask the developer to paste the content or give a file path. Never seed from a login page.
- **Empty** → prompt: *"Paste the ticket / document text below."* and read the pasted text.

Announce what was parsed on the opening line (e.g. *"source: file `docs/tickets/KTL-1234.md`"*) so a misparse is caught immediately.

## Process

1. **Acquire the source** per the rules above.
2. **Step 0 — propose a name.** Derive a 3–6 word slug (lowercase, hyphens) from the document's subject. Show it and ask the developer to **approve or edit**. The result sets the target: `docs/specs/<name>/prespec.md`. If that file already exists, read it and resume rather than overwrite.
3. **Extract into the template.** Map the document's content onto the 5 sections. For each section, prepare a pre-filled proposal tagged with provenance (`— extracted from <source>`). Where the document doesn't cover a section, leave it blank (do **not** invent).
4. **Boundary is advisory here (trust the developer).** If the document contains a *design decision* ("use a daily job", "add a column"), do **not** silently place it in §1/§2 — **flag it** and suggest moving it to §3 as a pointer, but let the developer decide. (Enforced silently only to the extent of not seeding §1/§2 with raw mechanisms.)
5. **Seeded per-section walk.** Go section by section, in order. For each:
    - Show the section blurb, then the **pre-filled proposal**.
    - Ask the developer to **edit or approve**. Quick approval is fine — no forced friction, no retyping. Only stop to resolve genuine problems.
    - **Cross-check** each approved answer against already-filled sections; challenge contradictions before moving on (same as `/prespecify`).
6. **Keep the non-trust minimums** (these are about the artifact, not distrust):
    - Mandatory fields must end up filled — §1 problem + desired state, §2 primary objective + out of scope, §4 at least one `PRESPEC-SC`. If the document didn't cover one, **ask** (fall back to the normal blank `/prespecify` question).
    - Success-criteria discipline unchanged: observable, measurable, must-haves only; `PRESPEC-SC-01`, `-02`, … durable IDs.
7. **Handle "I don't know" by kind** (same as `/prespecify`): unknown *solution* → §3 `research required` / `possible spike` pointer; unknown *problem/intent* in a mandatory field → push until stated.
8. **Write `docs/specs/<name>/prespec.md`** from the template. Frontmatter: `name`, `author` (git config user.name), `created` (today's date), `status: Draft`; keep `reviewer`/`approved` commented. **Cite the source** in §5 or as a reference line (e.g. `Source: <path-or-URL>`) so the trail back to the original vision survives.
9. **STOP.** Report:
    - Source parsed, and the approved name.
    - Path of the file written.
    - The `PRESPEC-SC` list with IDs.
    - Which sections were seeded from the document vs. filled/asked interactively.
    - Any design decisions you flagged and where they landed (§3 pointer or kept by developer choice).
    - Any mandatory field the document didn't cover that you had to ask for.
    - Reminder: the prespec is `Draft`; a human edits `status` to `Approved` (adding `reviewer`/`approved`) before `/spec-from-prespec <name>` can consume it.

The prespec is the deliverable. Do not generate a spec, plan, or code.
