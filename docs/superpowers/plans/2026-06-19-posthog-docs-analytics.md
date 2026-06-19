# PostHog Analytics for the Help Center — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Enable PostHog analytics (pageviews, autocapture, session replay) on the hosted Mintlify help center, feeding the shared EU PostHog project via the existing reverse proxy.

**Architecture:** Hosted Mintlify exposes no code surface for `posthog-js`. The only mechanism is the native `integrations.posthog` block in `docs.json`. A single config edit enables analytics; no JS, no dependencies, no PostHog dashboard changes.

**Tech Stack:** Mintlify (`docs.json` schema), PostHog EU cloud (project 156223) via reverse proxy `https://b.elimuboraerp.com`.

## Global Constraints

- PostHog project token (verbatim): `phc_qpX7FwgEJa6osP32UU2YEpCp6hYGcwbzVBUdtdnhgPz6`
- apiHost (verbatim): `https://b.elimuboraerp.com` — must be explicit; the Mintlify default routes to US ingestion and would drop data into the wrong region.
- `sessionRecording`: `true`
- No PostHog dashboard configuration is required (the "authorized domains for recordings" allowlist is deprecated; empty list = record everywhere).
- `docs.json` must remain valid JSON conforming to `https://mintlify.com/docs.json`.

---

### Task 1: Add the PostHog integration block to `docs.json`

**Files:**
- Modify: `docs.json` (add a new top-level `"integrations"` key)

**Interfaces:**
- Consumes: nothing (first and only task).
- Produces: the deployed docs initialize PostHog and emit `$pageview` for the docs host into project 156223.

- [ ] **Step 1: Add the `integrations` key to `docs.json`**

Insert a new top-level `"integrations"` key. Place it immediately after the
`"styling"` block (any valid top-level position works; this keeps it near other
behavior config). The block:

```json
"integrations": {
  "posthog": {
    "apiKey": "phc_qpX7FwgEJa6osP32UU2YEpCp6hYGcwbzVBUdtdnhgPz6",
    "apiHost": "https://b.elimuboraerp.com",
    "sessionRecording": true
  }
}
```

Ensure a comma separates it from the preceding/following keys so the file stays valid JSON.

- [ ] **Step 2: Verify `docs.json` is still valid JSON**

Run: `python3 -m json.tool docs.json > /dev/null && echo "valid JSON"`
Expected: prints `valid JSON` (no parse error).

- [ ] **Step 3: Verify the integration block is present and correct**

Run: `python3 -c "import json; p=json.load(open('docs.json'))['integrations']['posthog']; print(p['apiKey'], p['apiHost'], p['sessionRecording'])"`
Expected: `phc_qpX7FwgEJa6osP32UU2YEpCp6hYGcwbzVBUdtdnhgPz6 https://b.elimuboraerp.com True`

- [ ] **Step 4: Commit**

```bash
git add docs.json
git commit -m "feat: add PostHog analytics integration to help center"
```

---

### Task 2: Manual analytics verification

**Files:** none (runtime verification only).

**Interfaces:**
- Consumes: the `integrations.posthog` block from Task 1.
- Produces: confirmation that pageviews and replays reach the PostHog project.

- [ ] **Step 1: Run the docs locally**

Run: `mint dev`
Expected: local docs server starts (default `http://localhost:3000`). If `mint`
is not installed, run `npm i -g mint` first, or verify on the deployed preview
instead.

- [ ] **Step 2: Generate traffic**

Open the local (or deployed preview) docs in a browser, navigate across 2–3
pages, and click a few links so autocapture and replay have activity.

- [ ] **Step 3: Confirm events in PostHog**

In the PostHog project (156223, EU) open **Activity → Live events** (or
**Activity** feed). Filter/look for a `$pageview` event whose `$host` matches the
docs host you just browsed.
Expected: a `$pageview` event for the docs host appears within a minute.

- [ ] **Step 4: Confirm a session replay was captured**

In PostHog open **Session replay** and look for a recent session from the docs host.
Expected: a replay of your browsing session appears.

Note: localhost replays/events normally work, but if an adblocker or `mint dev`
quirk suppresses them, repeat Steps 2–4 against the deployed Mintlify preview.

---

## Self-Review

**Spec coverage:**
- Native `integrations.posthog` block → Task 1. ✓
- Shared project token / EU project → Global Constraints + Task 1. ✓
- `apiHost` = proxy, explicit, with US-default rationale → Global Constraints + Task 1. ✓
- `sessionRecording: true` → Task 1. ✓
- No PostHog dashboard changes → Global Constraints (stated). ✓
- Verification via pageview + replay in live events → Task 2. ✓
- "No custom events possible" → inherent (no code surface); nothing to implement, correctly absent from tasks. ✓
- Optional `$host`-filtered dashboard → explicitly optional in spec; intentionally omitted from the plan (YAGNI). ✓

**Placeholder scan:** No TBD/TODO/vague steps; every step has exact commands and expected output. ✓

**Type consistency:** Field names (`apiKey`, `apiHost`, `sessionRecording`) and values are identical across Global Constraints, Task 1, and the verification task. ✓
