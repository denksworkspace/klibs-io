---
description: Generate a klibs.io feature spec from an approved prespec — carry its success criteria forward by ID, gated on human approval.
---

# /spec-from-prespec

Generate a feature spec from an **approved prespec**. The prespec captured the *problem and intent*; this command turns prespec into a reviewable *contract*. The prespec's success criteria are durable — carry each `PRESPEC-SC-NN` forward by ID so the spec reviewer can verify every one is covered.

The spec must follow the template at `.claude/templates/spec.md` and respect the rules in `CLAUDE.md` (module-by-feature boundaries, minimal diff, no unsolicited refactors).

If you're starting from a vague description with no prespec, use `/specify` instead. If formalizing a spike, use `/spec-from-spike`.

## Input
$ARGUMENTS

`$ARGUMENTS` should be the feature `name` (the directory name under `docs/specs/`). Slugify it → `<name>`. If empty, ask which prespec to formalize.

## Process
1. **Read `docs/specs/<name>/prespec.md`.** If it doesn't exist, STOP and say so.
2. **Check the gate.** Read `status` from the frontmatter. If it is anything other than `Approved`, STOP and ask the human to confirm they really want to spec a non-approved prespec before continuing. Do not silently proceed.
3. **Read `.claude/templates/spec.md`** for the full structure.
4. **Translate the prespec into the spec**, section by section:
    - §1 Core idea (problem, desired state) → spec §1 Goal / §2 Problem.
    - §2 primary/secondary objectives → shape spec §3 scenarios and §4 FRs; §2 *out of scope* → spec §6 Out of scope; §2 *allowed trade-offs* and *must-not-regress* → constraints in §5/§8.
    - §3 Investigation pointers (candidate tools, research, spikes) → seed spec §8 Design decisions as options to weigh — **not** as settled choices. Keep the prespec's discipline: pointers become explored options, not foregone conclusions.
    - §4 `PRESPEC-SC-NN` → carry **each one forward by its ID**. Every PRESPEC-SC must map to at least one FR / acceptance scenario that would satisfy it. Reference the ID inline (e.g. "satisfies PRESPEC-SC-02") so coverage is traceable.
    - §5 Future developments → shape §6 Out of scope and note design headroom where relevant.
5. **Separate requirements from design decisions** — the spec's central rule. §4 is observable contracts only; every how/which-mechanism goes to §8. Re-scan §4 against the template's §4 litmus and move anything misfiled to §8.
6. **For anything the prespec doesn't answer**, insert `[NEEDS CLARIFICATION: <specific question>]` inline. Do not invent details to fill silence.
7. **Cite the prespec** in spec §13 References: `Prespec: docs/specs/<name>/prespec.md`.
8. **Write to `docs/specs/<name>/spec.md`** — never overwrite the prespec. Both artifacts stay. Don't include why-now / priority-rationale / project-management context.
9. **STOP.** Do not generate a plan, tasks, or code. Report back:
    - Path of the spec written, and whether the gate was `Approved` or overridden.
    - A **PRESPEC-SC coverage table**: each `PRESPEC-SC-NN` → the FR(s)/scenario(s) that satisfy it. Flag any SC you could not cover.
    - The design options you recorded in §8 (so the reviewer can challenge the *how* separately).
    - `[NEEDS CLARIFICATION]` markers with their lines, and top-3 assumptions.

The spec is the deliverable. Plan and implementation come later under separate commands.
