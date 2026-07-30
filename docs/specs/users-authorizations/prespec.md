---
name: users-authorizations
author: Daniel Volostnov (denk)
created: 2026-07-28
status: Approved
reviewer: Daniel Volostnov (denk)
approved: 2026-07-28
---

# Prespec: User authentication and authorization

<!-- Seeded via /prespecify-from-docs from KTL-2853 (pasted), then reviewed section by section. -->

## 1. Core idea

- **Current state:** No user identity or authentication on klibs.io. Project information (descriptions, tags) can only be changed by klibs.io admins via manual updates; there is no way for users to authenticate or engage.
- **Problem:** Library **authors/maintainers** can't keep their own project info accurate — they must file a request and wait for a klibs.io admin. Library **users** can't authenticate, so they can't engage (e.g. like libraries, submit reports).
- **Desired state:**
  - A user can **sign in** — via JetBrains Account, or via GitHub only — and **connect a GitHub account**.
  - Their **eligible indexed projects are associated automatically** (those where they have GitHub *Maintain* permission).
  - A **verified maintainer can edit that project's description and tags directly on klibs.io**, and the edit **persists** — it stays visible and isn't overwritten by later imports.
  - A user **without** Maintain permission (or who loses it) **cannot edit** — controls are unavailable/blocked.

## 2. Objectives

- **Primary objective:** First stage — let a **verified GitHub maintainer** sign in, connect GitHub, get their eligible projects auto-associated, and **self-serve edit those projects' description and tags** on klibs.io (persisted), removing the admin-mediated update loop.
- **Secondary objectives:** Establish a **unified klibs.io identity (JetBrains Account)** as the foundation for later community engagement; support signing in with **GitHub only**.
- **Must not regress:** The indexing/import pipeline must keep working; a maintainer's klibs.io override must not be overwritten by later imports; non-maintainers must never gain edit access.
- **Allowed trade-offs:** Willing to **re-check GitHub permission on every edit** (extra GitHub API calls per edit) to guarantee the permission is *current* — correctness/security over call-count.
- **Out of scope (first stage):** Liking libraries and submitting reports; author/organization discovery pages (KTL-2039); custom org identity & branding (KTL-2387); further project-management capabilities; GitLab / other providers; extendable multi-provider auth architecture.

## 3. Investigation

- **Existing project mechanisms:** `integrations/github` (GitHub API already integrated for indexing); `core/scm-repository` & `core/scm-owner` (repo↔owner mapping); `core/project` (where a description/tags override would live).
- **Candidate tools:** **JetBrains Hub** (backend behind JetBrains Account) already provides many auth/authorization options out of the box (OAuth providers, account linking, permission model); GitHub OAuth for account connect; GitHub *Maintain*-permission lookup (REST/GraphQL).
- **Research required:**
  - What **JetBrains Hub already gives us** — GitHub connection, multi-provider auth, account linking — vs. what we'd need to build ourselves (may subsume provider-extensibility).
  - How to determine a user has GitHub **Maintain** permission on a repo via API, and the **per-edit revalidation cost** (rate limits).
  - How to **auto-associate** an authenticated user's GitHub identity → indexed projects they maintain.
- **Possible spikes:** probe GitHub API for Maintain-permission detection + call cost; JetBrains Hub / Account OAuth integration spike.
- **Design area to investigate (flagged, not decided):** provider-extensible auth architecture — parked here so it isn't baked into requirements.

## 4. Success criteria

- **PRESPEC-SC-01:** A user can sign in via **JetBrains Account and connect GitHub**, and can sign in with **GitHub only**.
- **PRESPEC-SC-02:** Indexed projects where the signed-in user has **current GitHub *Maintain* permission** are **associated to them automatically** (no manual step).
- **PRESPEC-SC-03:** A maintainer with current Maintain permission can **edit the associated project's description and tags**, and the change is saved and shown on klibs.io.
- **PRESPEC-SC-04:** A project's description/tags can be updated **only** by a user holding a **current** GitHub Maintain permission on that repo, **verified at edit time**. Any user lacking current Maintain permission — including a maintainer who has since lost it — cannot update the project.
- **PRESPEC-SC-05:** A klibs.io description/tags **override stays visible and is not overwritten** by later imports/re-indexing.
- **PRESPEC-SC-06:** The system **tracks the list of connected maintainers** (feature-relevance metric).
- **PRESPEC-SC-07:** The system **tracks maintainers with >2 logins within a 30-day window** (engagement metric).

## 5. Future developments

- **Author/organization discovery page (KTL-2039):** a "Community" tab listing authors/organizations, **sortable** (by sum of stars, by followers) and **filterable** (*hireable* — GitHub "Available for hire"; *sponsorable* — GitHub Sponsors enabled).
- **Custom organization display names (KTL-2387):** let a **custom display name** be set for a project's organization (shown in the description and search results) and displayed **instead of** the original org name — e.g. show *JetBrains* instead of *ktorio* for Ktor.
- Further **project-management capabilities** (based on validated author needs).
- **GitLab / other source-control providers** — keep provider-extensibility in mind (see §3).
- **Community engagement** (liking libraries, submitting reports) — deferred from first stage, but the identity foundation should not preclude it.

**Source:** `KTL-2853` (pasted); original request: `https://github.com/JetBrains/klibs-io-issue-management/issues/112`
