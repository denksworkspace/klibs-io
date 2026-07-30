---
name: [feature-name]
author: [git config user.name]
created: [YYYY-MM-DD]
status: Draft
# When approved, a reviewer edits the two lines below and flips status to Approved:
# reviewer: [who approved]
# approved: [YYYY-MM-DD]
---

# Prespec: [FEATURE NAME]

<!--
A prespec is a cheap, HUMAN-authored alignment artifact, reviewed BEFORE a spec exists.
It aligns on the PROBLEM and the INTENT. It may carry solution POINTERS (things to
explore) but never solution DECISIONS ("we will use X") — those belong in spec §8.
If you catch yourself deciding a mechanism, move it to §3 as something to investigate.
Fillable by hand in a few minutes. Keep every answer short.
-->

## 1. Core idea
<!-- MANDATORY: problem + desired state. What are we doing and why, in plain words. -->

- **Current state:** <!-- What exists today? "Not implemented" is a valid answer. -->
- **Problem:** <!-- What's broken/missing, and WHO is affected (end users, library authors, developers)? -->
- **Desired state:** <!-- The observable end state — what a user/consumer will SEE or be able to do. No mechanism. -->

## 2. Objectives
<!-- MANDATORY: primary objective + out of scope. The intent and its boundaries. -->

- **Primary objective:** <!-- The one outcome this work must achieve. -->
- **Secondary objectives:** <!-- Nice-to-haves. Optional. -->
- **Must not regress:** <!-- Existing behavior/guarantees that must still hold. -->
- **Allowed trade-offs:** <!-- What you're willing to give up (e.g. "more API calls OK if it's more reliable"). -->
- **Out of scope:** <!-- Explicitly NOT doing this now. The cheapest way to prevent a review round-trip. -->

## 3. Investigation
<!-- OPTIONAL. Pointers to explore, NOT decisions. No "we will use X". -->
<!-- This is also where genuine solution-unknowns get parked instead of guessed. -->

- **Existing project mechanisms:** <!-- Things already in the codebase that likely apply (e.g. GraphQL client, Kohsuke, a scheduled job). -->
- **Candidate tools:** <!-- Options worth considering — not chosen. -->
- **Research required:** <!-- Open solution questions to answer before/while speccing. -->
- **Possible spikes:** <!-- Hypotheses worth a throwaway /spike before committing. -->

## 4. Success criteria
<!-- MANDATORY: at least one. Measurable, observable, MUST-haves only. -->
<!-- Nice-to-haves belong in §2 secondary objectives, not here. -->
<!-- These IDs are durable: /spec-from-prespec carries each one forward, and the spec must cover it. -->

- **PRESPEC-SC-01:** <!-- An observable, checkable outcome (e.g. "notify appears ~1 day after deploy for almost every archived repo"). -->
- **PRESPEC-SC-02:** <!-- e.g. "API calls do not exceed 2× current — fewer is better." -->

## 5. Future developments
<!-- OPTIONAL. Coming work that should shape THIS design now, even though it's out of scope to build. -->

- <!-- e.g. "Website will let users filter out archived repositories." -->
